# Re-using Layouts with tag `<include/>` in Android

title: Re-using Layouts with tag "include" in Android
date: 2015-10-19 18:00:00
tags:
- Android

---

When designing the Android layout file, we sometimes might need to re-use larger components that require a special layout. To efficiently achieve this, we can use the `<include/>`  tag to embed another layout inside the current layout.

<!--more-->

## Create a Re-usable Layout
Create a new XML file and define the layout. For example, here is a XML layout file `mojo_introduction_layout.xml`  from the app MojoTeahouse.
``` xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="250dp"
    android:background="?attr/colorPrimary">

    <android.support.v7.widget.Toolbar.../>

    <ImageView.../>

    <TextView.../>

</RelativeLayout>
```
![Re-usable Layout](http://i.imgur.com/fbLi5Vk.png)


## Use the "include" Tag
Inside the layout to which we want to add the re-usable component, add the `<include/>` tag. For example, here's a layout from the `activity_about.xml` that includes the `mojo_introduction_layout` from above.


``` xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.mojoteahouse.mojotea.activity.AboutActivity">

    <include layout="@layout/mojo_introduction_layout" />

    <LinearLayout...>

</LinearLayout>
```

![AboutActivity layout](http://i.imgur.com/AmNIlCy.png)

We can also override all the layout parameters (any `android:layout_*` attributes) of the included layout's root view by specifying them in the `<include/>` tag. For example:

``` xml
<include android:id=”@+id/mojo_introduction_head”
         android:layout_width=”match_parent”
         android:layout_height=”match_parent”
         layout=”@layout/mojo_introduction_layout"/>
```

However, if you want to override layout attributes using the `<include>` tag, you must override both `android:layout_height` and `android:layout_width` in order for other layout attributes to take effect.


----------

## Use the "merge" Tag
This following section is cited from the Android [document](http://developer.android.com/training/improving-layouts/reusing-layouts.html#Merge). We haven't tried the `<merge>` tag in our application yet.

The <merge /> tag helps eliminate redundant view groups in your view hierarchy when including one layout within another. For example, if your main layout is a vertical LinearLayout in which two consecutive views can be re-used in multiple layouts, then the re-usable layout in which you place the two views requires its own root view. However, using another LinearLayout as the root for the re-usable layout would result in a vertical LinearLayout inside a vertical LinearLayout. The nested LinearLayout serves no real purpose other than to slow down your UI performance.

To avoid including such a redundant view group, you can instead use the <merge> element as the root view for the re-usable layout. For example:

``` xml
<merge xmlns:android="http://schemas.android.com/apk/res/android">

    <Button
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/add"/>

    <Button
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/delete"/>

</merge>
```
Now, when you include this layout in another layout (using the `<include/>` tag), the system ignores the `<merge>` element and places the two buttons directly in the layout, in place of the `<include/>` tag.