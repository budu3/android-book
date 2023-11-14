---
layout: page
title: Views and Layouts
permalink: /views/
---

# Views and Layouts

## Introduction

In this chapter we are going delve into the user interface elements that make up your app. We are going to talk about Views, ViewGroups and Layouts which are the building block of the visual elements in your app.

## Views

In Android, Views are the basic building blocks for user interface elements. For instance the Button, TextField, WebView widgets are all examples of Views. A particular View will cover a rectangular portion of the screen and is responsible for drawing its components on that portion and handling events (for instance what happen when you touch or swipe) in that area.

## ViewGroup

A ViewGroup is a container that is able to hold other Views and ViewGroups. They can be used to create more complex user interface layouts.

## Layouts

A Layout is a type of ViewGroup that helps you organize your UI components on the screen. For instance, the Grid View allows you to arrange your UI components in a grid. If you look under the hood you will discover that Layouts are made up of Views. Some of the Layouts available on Android are,

* Linear Layout, arranges user interface elements from top to bottom or left to right
* Relative Layout, arranges elements relative to other elements
* Table Layout, arranges elements in a tabular format
* Absolute Layout, this layout allows you to place elements in exact x/y coordinates
* Frame Layout, with this views elements are stacked on top of each other
* List View, arranges elements in the form of a scrollable list
* Grid View, arranges the elements in a grid



The following code snippet from an `activity_main.xml` layout file that shows an example of a TextView inside of a LinearLayout.


~~~
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="horizontal">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"/>

</LinearLayout>
~~~

![TextView inside of a LinearLayout](../images/linear_layout.png)

## Conclusion

In this chapter we learned about Views being the building blocks of all Android user interface elements. We also learned about the relationship between Views and ViewGroups. Particularly, we learned that ViewGroups hold other Views and ViewGroups. We also got the know about how Layouts are a type of ViewGroup and the different kinds of Layouts that exist in Android.
