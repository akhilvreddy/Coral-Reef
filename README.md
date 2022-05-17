# CoralReefAnalysis
Research project dealing with Coral Reefs and image detection.

## Abstract

The goal of this lab was to take a sample of coral reefs (based out of Honlulu, Hawaii) and then analyze their average RGB values to see if they are healthy. This was made possible by the use of YOLOV4 - an image detection software, some software that I wrote and put into an android device, and the knowledge of members from the Biochemistry department. 

The members from the biochemistry department would take a snap of a reef and it would look like this: 

//picture here

From this, we would have to detect the border of the image, find the average RBG value, do some more chemical analysis on it, and send it back to the biochemistry folks in order for them to analyze it more. In the following, I will explain what I did and how this came to fruition. 

## Starting this Project

I started off everything by writing a Java function that would be able to do border and image detection. I started off using an open source edge detector, but quikcly realized that it would take a lot of processing power from the somewhat cheap phone that I was given. After some thinking, this is what I did next: 

//add picture of diffferences

I decided to compare pixels, and looped through the image using a nested for loop.

## Training the algorithm (.tflife file)


## Demonstration

## Issues 

## What's next 
