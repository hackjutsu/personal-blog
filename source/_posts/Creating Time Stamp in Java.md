# Creating Time Stamp in Java

title:  Creating Time Stamp in Java
date: 2016-01-01 18:00:01
tags:
- Java

---

Here is a quick reference for creating time stamp in Java using `SimpleDateFormat`. The example comes from *Konstantina Dimtsa's* [snippet](https://examples.javacodegeeks.com/core-java/text/java-simpledateformat-example/) on **Java Code Geeks**. Please refer to [the original post](https://examples.javacodegeeks.com/core-java/text/java-simpledateformat-example/) for more details.

<!--more-->

```java
package com.javacodegeeks.corejava.text;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;

public class SimpleDateFormatExample {
	public static void main(String[] args) {

		Date curDate = new Date();

		SimpleDateFormat format = new SimpleDateFormat();
		String DateToStr = format.format(curDate);
		System.out.println("Default pattern: " + DateToStr);

		format = new SimpleDateFormat("yyyy/MM/dd");
		DateToStr = format.format(curDate);
		System.out.println(DateToStr);

		format = new SimpleDateFormat("dd-M-yyyy hh:mm:ss");
		DateToStr = format.format(curDate);
		System.out.println(DateToStr);

		format = new SimpleDateFormat("dd MMMM yyyy zzzz", Locale.ENGLISH);
		DateToStr = format.format(curDate);
		System.out.println(DateToStr);

		format = new SimpleDateFormat("MMMM dd HH:mm:ss zzzz yyyy",
				Locale.ITALIAN);
		DateToStr = format.format(curDate);
		System.out.println(DateToStr);

		format = new SimpleDateFormat("E, dd MMM yyyy HH:mm:ss z");
		DateToStr = format.format(curDate);
		System.out.println(DateToStr);

		try {
			Date strToDate = format.parse(DateToStr);
			System.out.println(strToDate);
		} catch (ParseException e) {
			e.printStackTrace();
		}
	}
}
```
