---
layout: page
title: Intent
permalink: /intent/
---

# Intent

## Introduction

An intent is used to send a message from one Activity to another. For instance an Intent can be used by one Activity to start the Activity within another app. We have two kinds of Intents, **implicit Intents** and **explicit Intents**.

**Implicit Intents** send out a message an rely on the underlying system to determine which app responds to the message.

**Explicit Intents** as the name suggest, specify the Activity that will respond to the message.

## Implicit Intents

The following snippet shows an implicit Intent in action. The system will open the registered email client to respond when this Intent is invoked.
 
{lang="java"}

~~~
//>> emailintent
                // Create the text message with a string
                Intent emailIntent = new Intent(Intent.ACTION_SENDTO);
                emailIntent.setData(Uri.parse("mailto:developer@example.com"));

                // Verify that the intent will resolve to an activity
                if (emailIntent.resolveActivity(getPackageManager()) != null) {
                    startActivity(emailIntent);
                }
                //<<
~~~


Code Listing 14 is a full code listing of the _MainActivity.java_ file. It demonstrates how to implement an implicit intent in our Netflix clone app.

{lang="java"}

~~~
//>> emailintent
                // Create the text message with a string
                Intent emailIntent = new Intent(Intent.ACTION_SENDTO);
                emailIntent.setData(Uri.parse("mailto:developer@example.com"));

                // Verify that the intent will resolve to an activity
                if (emailIntent.resolveActivity(getPackageManager()) != null) {
                    startActivity(emailIntent);
                }
                //<<
~~~


## Explicit Intents

When you create an explicit Intent you specify that class that you want to respond to the Intent. In the following code snippet we specify the `Main2Activity` class to handle our message.


~~~
//>> emailintent
                // Create the text message with a string
                Intent emailIntent = new Intent(Intent.ACTION_SENDTO);
                emailIntent.setData(Uri.parse("mailto:developer@example.com"));

                // Verify that the intent will resolve to an activity
                if (emailIntent.resolveActivity(getPackageManager()) != null) {
                    startActivity(emailIntent);
                }
                //<<
~~~


The following code listing shows the `MainActivity.java` file that calls the second activity that holds our search functionality.

{lang="java"}

~~~
//>> emailintent
                // Create the text message with a string
                Intent emailIntent = new Intent(Intent.ACTION_SENDTO);
                emailIntent.setData(Uri.parse("mailto:developer@example.com"));

                // Verify that the intent will resolve to an activity
                if (emailIntent.resolveActivity(getPackageManager()) != null) {
                    startActivity(emailIntent);
                }
                //<<
~~~


The following code listing shows the `MainActivity2.java` file that is called by `MainActivity.java`.


~~~
//>> emailintent
                // Create the text message with a string
                Intent emailIntent = new Intent(Intent.ACTION_SENDTO);
                emailIntent.setData(Uri.parse("mailto:developer@example.com"));

                // Verify that the intent will resolve to an activity
                if (emailIntent.resolveActivity(getPackageManager()) != null) {
                    startActivity(emailIntent);
                }
                //<<
~~~
