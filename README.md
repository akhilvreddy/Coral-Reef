# Coral Reef Analysis
Research project dealing with Coral Reefs and image detection.

## Abstract

The goal of this lab was to take a sample of coral reefs (based out of Honlulu, Hawaii) and then analyze their average RGB values to see if they are healthy. This was made possible by the use of YOLOV4 - an image detection software, some software that I wrote and put into an android device, and the knowledge of members from the Biochemistry department. 

The members from the biochemistry department would take a snap of a reef and it would look like this: 

<p align="center">
  <img 
    width="544"
    height="378"
    src="https://github.com/akhilvreddy/CoralReefAnalysis/blob/main/Mcap41.jpg"
  >
</p>

> [Picture 1] The red, blue, and green is the relative RGB values based on the camera that took the image, so we have to compare to those specific values. 

From this, we would have to detect the border of the image, find the average RBG value, do some more chemical analysis on it, and send it back to the biochemistry folks in order for them to analyze it more. In the following, I will explain what I did and how this came to fruition. 

## Starting this Project

I started off everything by writing a Java function that would be able to do border and image detection. I started off using an open source edge detector, but quickly realized that it would take a lot of processing power from the somewhat cheap phone that I was given. After some thinking, this is what I did next: 


<p align="center">
  <img 
    width="344"
    height="394"
    src="https://github.com/akhilvreddy/CoralReefAnalysis/blob/main/reefpic3.jpg"
  >
  <img 
    width="344"
    height="394"
    src="https://github.com/akhilvreddy/CoralReefAnalysis/blob/main/reefpic2.jpg"
  >
</p>

> [Picture 2 & 3] Picture 2 represents the kind of image generated by the YOLOV-4 software and picture 3 is the border detection I was aiming to do. 

I decided to compare pixels, and looped through the image using a nested for loop.

## How the Code Works
You can find the real code in the java file in this repository (Main.java), but I have included some pseudocode here to better understand it. 

```
for (int i = 0; i < image.width; i++){
  for (int j = 0; j < image.height; j++){
   
    if ( _condition_ )
      -> get RGB Value of pixel, understand the value, and store it
      
   }
}

return averageRedValue, averageGreenValue, averageBlueValue, colorDensity, etc..
```

Here, the condition is the main part that does the border detection. It was kind of lengthy. 

With the squares that YOLOV-4 identified, they always did an overshoot of the coral. Because of this I knew that the the four corners had to be the background color, which was significantly different than the coral coral (which they did on purpose). 

```
-> upper left corner: cornerPixel1
-> upper right corner: cornerPixel2
-> lower left corner: cornerPixel3
-> lower right corner: cornerPixel4

backgroundRedValue = (cornerPixel1.Red+cornerPixel2.Red+cornerPixel3.Red+cornerPixel4.Red)/4
backgroundGreenValue = (cornerPixel1.Green+cornerPixel2.Green+cornerPixel3.Green+cornerPixel4.Green)/4
backgroundBlueValue = (cornerPixel1.Blue+cornerPixel2.Blue+cornerPixel3.Blue+cornerPixel4.Blue)/4
```
From here on out, I've assumed that the 4 pixels average is ****the color components of the background****.

Now, knowing the red, green, and blue value of the background, I can compare the current pixel in the loop to the background color to see if they match. If they don't match, that's how we know its a coral reef pixel. 

```
 if ( *condition* )
 
 here, the condition will be: 
 
 currentPixel.red!=backgroundRedValue && currentPixel.blue!=backgroundBlueValue && currentPixel.green!=backgroundgreenValue
```
This seems like an efficient way to do it, but can lead to several issues. Each pixel of the background can have varying pixel element colors. We need a range of values for the pixel to be in so that this condition. 

This can be done like the following: 

```
condition: Math.abs(currentPixel.red-backgroundRedValue) <= 10 ; Math.abs(currentPixel.blue-backgroundBlueValue) <= 10 ; Math.abs(currentPixel.blue-backgroundBlueValue) <= 10
```
From testing different RGB values, I saw that even if two of these conditions are true, the detection works well. Putting this into the if condition makes it work well. Here is what I got as a comparison: 

<p align="center">
  <img 
    width="344"
    height="394"
    src="https://github.com/akhilvreddy/CoralReefAnalysis/blob/main/detectionpic.png"
  >
  <img 
    width="344"
    height="394"
    src="https://github.com/akhilvreddy/CoralReefAnalysis/blob/main/45pic9.png"
  >
</p>

> [Picture 4 & 5] Picture 4 is the average rgb value I got from my algorithm and picture 5 is the real rgb value of the image using ImageJ.

The harder it is to see the difference between these two images means the better this algorithm is working :)

Now, using the average RGB values that I got, I packed them into a single file and sent that out. Others can analyze the coral from there. The data was packed like the following. 

<p align="center">
  <img 
    width="744"
    height="134"
    src="https://github.com/akhilvreddy/CoralReefAnalysis/blob/main/44pic0.png"
  >
</p>

>  Here, the different lines are supposed to represent the different types of coral that we have. 

## Training the algorithm (.tflife file)
This part didn't require much coding, but rather some learning on how neural networks work. What we did was gather. 
We ran into some issues of the network not being too accurate at first and not detecting some of them, so we went back and trained the network further by importing and describing more images.

We used _**TensorFlow**_ and the ImageDataGenerator in python3 to do the analysis. The end goal of this was for the YOLOV-4 software to interpret the differnet coral in the pictures as shown in picture 2. Then I would take multiple different types of images like picture 2 for multiple coral, strengthen the border detection by using my above algorithm, and then outputting the values to the biochem people. 

## Remarks 
I hope that this project is able to understand coral reefs more. By understanding the health and colors of the reef in a better fashion, it would benefit our environment and acquatic life. 
