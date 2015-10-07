# Retrofit 2.0 Reference 
title: Retrofit 2.0 Reference
date: 2015-10-06 18:00:45
tags: 
- Android
- Retrofit

---

**Retrofit** is one of the most popular Android http library. In [its latest 2.0 version](http://square.github.io/retrofit/), it comes with many new features over its earlier versions, for example, the ability to cancel an ongoing HTTP request or the ability to plugin multiple serialization converters.

Here is a quick reference for adopting **Retrofit 2.0** into our Android app. 
<!-- more -->
## A Simple Example
Here is an example about downloading the name of the contributors and the number of their commits from the [Github Retrofit project](https://github.com/square/retrofit).

We use the Github public API for retrieving contributors' information. 
> api.github.com/repos/{owner}/{repo}/contributors

In our case, the owner is `square` and the repo is `retrofit`.

The JSON details can be found in this [link](https://api.github.com/repos/square/retrofit/contributors). For each JSON object, we're interested in its `login` value and `contributions` value.
``` javascript
[
  {
    "login":"JakeWharton"
    ...
    "contributions":"618"
  }
  {
    "login":"swankjesse"
    ...
    "contributions":"93"
  }
  ...
]
```
First, let's define the `Contributor` class with its `login` member and its `contributions` member we are interested in.
``` java
public class Contributor {

    public String login; // GitHub username.
    public int contributions; // Commit count.
}
```
Then we define the HTTP API via annotations on interfaces and parameters indicating how a request will be handled.
``` java
public interface GitHubService {

    @GET("/repos/{owner}/{repo}/contributors")
    Call<List<Contributor>> contributors(
            @Path("owner") String owner,
            @Path("repo") String repo);
}
```
To send a HTTP request, we need a `Call` instance. The `Call` instances can be executed either synchronously or aysnchronously. Each instance can only be used once, but calling `clone()` will create a new instance that can be used again.
``` java
// Create a very simple REST adapter which points the GitHub API.
Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("https://api.github.com")
        .addConverterFactory(GsonConverterFactory.create())
        .build();
        
// Create an instance of our GitHub API interface.
GitHubService github = retrofit.create(GitHubService.class);

// Create a call instance for looking up Retrofit contributors.
Call<List<Contributor>> call = github.contributors("square", "retrofit");
```
### Synchronous Request
Use the `execute()` method to send a **synchronous** request. 
``` java
List<Contributor> contributors = call.execute().body();
for (Contributor contributor : contributors) {
    System.out.println(contributor.login + " (" + contributor.contributions + ")");
}
```
### Asynchronous Request
Use the `enqueue()` methodto send an **asynchronous** request. The `enqueue()`will make a request in the background thread and retrieve a result as an object which we can extract from response with `response.body()`. Note that `onResponse()` and `onFailure()` will be called in the **Main Thread**, which suits Android pretty well.
``` java
call.enqueue(new Callback<List<Contributor>>() {
    @Override
    public void onResponse(Response<List<Contributor>> response, Retrofit retrofit) {
        List<Contributor> contributors = response.body();
        for (Contributor contributor : contributors) {
            Log.d(TAG, contributor.login + " (" + contributor.contributions + ")");
        }
    }

    @Override
    public void onFailure(Throwable t) {
        Log.d(TAG, "Failed!!");
    }
});
```
### Cancel Transaction
Use `cancel()`to cancel an ongoing transaction.

## URL Resolving
Retrofit 2.0 comes with new URL resolving concept. Base URL and @Url have not just simply been combined together but have been resolved the same way as what `<a href="...">` does instead. Please take a look for the examples below for the clarification.
``` java
public interface APIService {
    @POST("list")
    Call<Repo> loadRepo();
}

public void doSomthing() {
    Retrofit retrofit = new Retrofit.Builder()
            .baseUrl("http://api.nuuneoi.com/base/level1")
            .addConverterFactory()
            .build();
}
// Resolved: http://api.nuuneoi.com/base/list
```
``` java
public interface APIService {
    @POST("/list")
    Call<Repo> loadRepo();
}

public void doSomthing() {
    Retrofit retrofit = new Retrofit.Builder()
            .baseUrl("http://api.nuuneoi.com/base/level1")
            .addConverterFactory()
            .build();
}
// Resolved: http://api.nuuneoi.com/list
```
``` java
public interface APIService {
    @POST("list")
    Call<Repo> loadRepo();
}

public void doSomthing() {
    Retrofit retrofit = new Retrofit.Builder()
            .baseUrl("http://api.nuuneoi.com/base/level1/")
            .addConverterFactory()
            .build();
}
// Resolved: http://api.nuuneoi.com/base/level1/list
```
Suggestion on the new URL declaration pattern in Retrofit 2.0:
- Base URL: always ends with /
- @Url: DO NOT start with /

## URL Manipulation
A request URL can be updated dynamically using replacement blocks and parameters on the method. A replacement block is a string surrounded by `{` and `}`. A corresponding parameter must be annotated with `@Path` using the same string.
``` java
@GET("/group/{id}/users")
Call<User> groupList(@Path("id") int groupId);
```
Query parameters can also be added.
``` java
@GET("/group/{id}/users")
Call<User> groupList(@Path("id") int groupId, @Query("sort") String sort);
```
For complex query parameter combinations a Map can be used.
``` java
@GET("/group/{id}/users")
Call<User> groupList(@Path("id") int groupId, @QueryMap Map<String, String> options);
```

## Header Manipulation
We can set static headers for a method using the `@Headers` annotation.
``` java
@Headers("Cache-Control: max-age=640000")
@GET("/widget/list")
Call<List<Widget>> widgetList();
```
``` java
@Headers({
    "Accept: application/vnd.github.v3.full+json",
    "User-Agent: Retrofit-Sample-App"
})
@GET("/users/{username}")
Call<User> getUser(@Path("username") String username);
```
Note that headers do not overwrite each other. All headers with the same name will be included in the request.

A request Header can be updated dynamically using the `@Header` annotation. A corresponding parameter must be provided to the `@Header`. If the value is null, the header will be omitted. Otherwise, `toString` will be called on the value, and the result used.
``` java
@GET("/user")
Call<User> getUser(@Header("Authorization") String authorization)
```
Headers that need to be added to every request can be specified using an [OkHttp interceptor](https://github.com/square/okhttp/wiki/Interceptors).

## OkHttp Interceptor
Interceptors are a powerful mechanism that can monitor, rewrite, and retry calls.  Here is a quick example about the Application Interceptor. We can use the interceptors to modify the request and the response.
``` java
OkHttpClient client = new OkHttpClient();
client.interceptors().add(new Interceptor() {
    @Override
    public Response intercept(Chain chain) throws IOException {
        Request request = chain.request();
        request = request.newBuilder()
                .addHeader("appid", "hello")
                .addHeader("deviceplatform", "android")
                .removeHeader("User-Agent")
                .addHeader("User-Agent", "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:38.0) Gecko/20100101 Firefox/38.0")
                .build();
        Response response = chain.proceed(request);
        return response;
    }
});

Retrofit retrofit = new Retrofit.Builder()
        .baseUrl(BASE_API_URL)
        .client(client)
        .addConverterFactory(GsonConverterFactory.create())
        .build();
```
A call to chain.proceed(request) is a critical part of each interceptor’s implementation. This simple-looking method is where all the HTTP work happens, producing a response to satisfy the request.

There are [two kinds of interceptors in OkHttp](https://github.com/square/okhttp/wiki/Interceptors):  `Application Interceptors` and `Network Interceptors`. 

``` java
// Application Interceptors
OkHttpClient client = new OkHttpClient();
client.interceptors().add(new LoggingInterceptor());

// Network Interceptors
OkHttpClient client = new OkHttpClient();
client.networkInterceptors().add(new LoggingInterceptor());
```

## Plugable Serialization Converter
In **Retrofit 2.0**, `Converter` is not included in the package. We need to plug a `Converter` in ourselves or `Retrofit` will be able to accept only the `String` result. 

To parse the JSON response, we need to plug in the Gson Converter.
``` java
compile 'com.squareup.retrofit:converter-gson:2.0.0-beta2'
```
And then plug in via `addConverterFactory()`.
``` java
Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("http://api.nuuneoi.com/base/")
        .addConverterFactory(GsonConverterFactory.create())
        .build();
 
service = retrofit.create(APIService.class);
```

## RxJava
RxJava – Reactive Extensions for the JVM – a library for composing asynchronous and event-based programs using observable sequences for the Java VM.

I haven't investigated in RxJava, but we can refer to its usage from [here](https://github.com/ReactiveX/RxJava).

## Others
### Get Raw JSON Response
Simply set the [POJO](https://en.wikipedia.org/wiki/Plain_Old_Java_Object) in the HTTP API interface as `JSONElement` and set `Gson` as the converter.
### Parse Nested JSON Value
http://stackoverflow.com/questions/32942661/how-can-retrofit-2-0-parse-nested-json-object
### Retrofit 2.0 Tutorials
- [Tutorial 1](https://realm.io/news/droidcon-jake-wharton-simple-http-retrofit-2/)
- [Tutorial 2](http://blog.robinchutaux.com/blog/a-smart-way-to-use-retrofit/)
- [Tutorial 3](http://inthecheesefactory.com/blog/retrofit-2.0/en)

@(Learning Cards)[Marxico|Android]

