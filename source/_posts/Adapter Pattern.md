# Adapter Pattern
title:  Adapter Pattern
date: 2015-11-07 23:01:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---

An adapter helps two incompatible interfaces, whose inner functionality should suit the need, to work together. The Adapter design pattern achieves this by converting the interface of one class into an interface expected by the clients.
<!--more-->

## Intent
- Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
- Wrap an existing class with a new interface.
- Match an old component to a new system

## Implementation
![Class Diagram of Adapter Pattern](http://i.imgur.com/ldPLwmp.png)
The following example is adapted from [this post](http://www.java2s.com/Tutorials/Java/Java_Design_Patterns/0060__Java_Adapter_Pattern.htm).
``` java
// Player.java
interface Player {
   public void play(String type, String fileName);
}
```
``` java
// AudioPlayer.java
class AudioPlayer {
   @Override
   public void playAudio(String fileName) {
      System.out.println("Playing. Name: "+ fileName);
   }
}
```
``` java
// VideoPlayer.java
class VideoPlayer {
   @Override
   public void playVideo(String fileName) {
      System.out.println("Playing. Name: "+ fileName);
   }
}
```
``` java
// MyPlayer.java
class MyPlayer implements Player {

   AudioPlayer audioPlayer = new AudioPlayer();
   VideoPlayer videoPlayer = new VideoPlayer();

   @Override
   public void play(String audioType, String fileName) {
      if(audioType.equalsIgnoreCase("avi")){
         videoPlayer.playVideo(fileName);
      }else if(audioType.equalsIgnoreCase("mp3")){
         audioPlayer.playAudio(fileName);
      }
   }
}
```
``` java
// Main.java
public class Main {
   public static void main(String[] args) {
      Player myPlayer = new MyPlayer();

      myPlayer.play("mp3", "h.mp3");
      myPlayer.play("avi", "me.avi");
   }
}
```
## Reference
[Wikipedia](https://en.wikipedia.org/wiki/Adapter_pattern)