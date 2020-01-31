---
title: "How to setup and run JavaFX version 13 `Hello World` project using IntelliJ IDEA"
published: true
---

## Introduction:

JavaFX 2.0 used to be part of JDK 8 however if you want to use the latest Java langauge version, JavaFX doesn't come bundled with the standard JDK anymore. This is not a big problem, as it can be used as a 3rd party library in our Java Projects. There are just a few steps that we have to follow. I've written them down below.

## Pre-requisite :
At the time of writing this tutorial I'm using Java 13 so first step is to make sure you hava JDK 13.x on your machine. You can can download and intall either Oracle JDK or the open source OpenJDK from https://jdk.java.net/13/.

## JavaFX Setup :
Make sure you have the corresponding version of JavaFX Runtime, in my case it would be JavaFX 13.x which can be downloaded from https://gluonhq.com/products/javafx/. You can save it anywhere, I generally put in in the same directory where JDK is installed.

## IntelliJ Setup & Running Hello World Application :

1. Create a new Project--> JavaFX --> JavaFX Application (make sure Project SDK maps to JDK 13)
2. Follow the Wizard to create the project and give is a meaningful name (in my case I called it HelloJavaFX)
3. You should see a Simple JavaFX Project created for you, click on Main.java file and notice how the classes are not loaded.
4. Install the JavaFX library for your Project by going to File --> Project Structure --> Library --> Java --> location to JavaFX lib folder
5. Notice the Main.Java file will now become error free as the IDE will be able to detect the required JavaFX classes
6. Click on "Run" in menu bar --> "Edit Configuration" and now we need to provide JVM options in order to execute JavaFX program.
7. In "VM Options" provide JavaFX module path to JVM by typing : --module-path PathToFolderJavaFXSDK/lib --add-modules javafx.controls,javafx.fxml
8. Click on "Apply" and "Ok"
9. Run the Project by clicking "Run" menu --> "Run `Main`"
10. Notice the JavaFX project starts and in the title bar it says "Hello World". *Congrautlations, you are now running JavaFX project!*
