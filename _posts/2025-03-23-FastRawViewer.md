---
layout: post
title: "Culling and selecting photos using FastRawViewer"
date: 2025-03-22 12:00:00 +0000
categories: photography
img: FastRawViewer_title.png 
---

When taking thousands of photos during a birding outing, the last thing I want to do is import them all into Lightroom. Once they end up in my Lightroom catalog, they tend to stay there for life. Instead, I use FastRawViewer to identify keepers directly off the SD card before importing them into Lightroom. I usually keep fewer than 10 photos per session from approximately 1,500 shots. Speed of viewing and sorting directly to the sdcard is fast on my v90 rated sdcards connected over usb-c or usb3, I'm sure slower cards would make this workflow painful. 

This blog post covers step 2 in my workflow. For step 1—setting up the camera, see [OM-1 - Birds Settings](https://rhett.cc/om-1-birds-in-flight-settings/)

![image](/assets/img/2025-03-23-FastRawViewer/PhotoEditingWorkflow.png)

There are other alternative programs to FastRawViewer, mainly PhotoMechanic, and some people use the free XnView for culling and selecting. I wasn't prepared to pay the price of PhotoMechanic, and I preferred FastRawViewer over XnView for this task. For $20 USD, I thought it was a bargain.

I have set up FastRawViewer to behave similarly to Lightroom. I find its default configuration difficult to work with, so I have outlined my preference modifications below. 

## Preference changes

In the General Preferences, go to the "File Operations" tab.

![image](/assets/img/2025-03-23-FastRawViewer/Pasted-image-20250320162248.png)

Turn off the "Advanced selection mode." To select multiple concurrent photos, I just single-click on the first photo, hold Shift down, and select the last file. Once selected, you can reject with "X" or put them in the "_selected" folder with "3."

![image](/assets/img/2025-03-23-FastRawViewer/Pasted-image-20250322123937.png)

Turn off the warning for removable media every time you insert an SD card. 

![image](/assets/img/2025-03-23-FastRawViewer/Pasted-image-20250322193733.png)

Under the Keyboard Shortcuts preferences, edit the "File Copy/Move/Reject" tab.

![image](/assets/img/2025-03-23-FastRawViewer/Pasted-image-20250320162645.png)

## Usage

When I press "X" on a photo, it moves the photo to a "_rejected" folder on the SD card. 

When I press "3" on a photo, it moves the photo to a "_selected" folder on the SD card. This is the only folder I import to Lightroom.

This is basically it. Just plug your SD card in, point FastRawViewer to it, and select any photos you want to import with the "3" key.

Once complete, load up Lightroom and import files from the "_selected" folder on the SD card.
