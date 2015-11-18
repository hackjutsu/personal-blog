# How many ways to read a file in Java?

title:  How many ways to read a file in Java?
date: 2015-11-17 22:20:00
tags:
- Java

---

The number of total classes of Java I/O is large, and it is easy to get confused when to use which. The following are methods for reading a file line by line.
<!--more-->

## Read a file line by line
``` java
private static void readFile1(File fin) throws IOException {
    FileInputStream fis = new FileInputStream(fin);

    //Construct BufferedReader from InputStreamReader
    BufferedReader br = new BufferedReader(new InputStreamReader(fis));

    String line = null;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }

    br.close();
}
```
``` java
private static void readFile2(File fin) throws IOException {
    // Construct BufferedReader from FileReader
    BufferedReader br = new BufferedReader(new FileReader(fin));

    String line = null;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }

    br.close();
}
```
``` java
// For java 1.7 or above
private static void readFile3(String fin) throws IOException {

    try(BufferedReader br = new BufferedReader(new FileReader(fin))) {
        StringBuilder sb = new StringBuilder();
        String line = br.readLine();

        while (line != null) {
            sb.append(line);
            sb.append(System.lineSeparator());
            line = br.readLine();
        }
        String everything = sb.toString();
    }
}
```


----------


## Read everything as a string
``` java
FileInputStream inputStream = new FileInputStream("foo.txt");
try {
    String everything = IOUtils.toString(inputStream);
} finally {
    inputStream.close();
}
```
``` java
// For java 1.7 or above
try(FileInputStream inputStream = new FileInputStream("foo.txt")) {
    Session IOUtils;
    String everything = IOUtils.toString(inputStream);
}
```
----------


## Read a file as a byte array
``` java
// For java 1.7 or above
Path path = Paths.get("path/to/file");
byte[] data = Files.readAllBytes(path);
```