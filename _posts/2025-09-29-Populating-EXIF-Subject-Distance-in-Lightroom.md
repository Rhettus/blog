---
layout: post
title: "Populating EXIF Subject Distance in Lightroom"
date: 2025-09-29 12:00:00 +0000
categories: photography
---

When taking photos of birds and wildlife, I want to know the distance to the subject so I can evaluate detail. I'm trying to figure out the sweet spot of my lens, or even the atmospheric effects at a given distance. The issue is my camera, and most cameras, populate that information in EXIF under "MakerNotes". Programs like Lightroom don't allow you to display information from there. This post shows a method of populating this information in the standard "Subject Distance" attribute so we can see it easily. It must be performed before import into Lightroom for it to show up.

## exiftool

To do this, we need to install `exiftool`. I'm on Mac so it's simple to install via brew:

```
brew install exiftool
```

To see all the attributes under MakerNotes in EXIF, you can run the following (replace filename with a sample RAW file):

```
exiftool -MakerNotes:All $filename
```

exiftool knows a lot of the manufacturer-specific EXIF and makes our job simple.

The only command that needs to be run to populate the value is:
```
exiftool "-SubjectDistance<FocusDistance" -overwrite_original $filename
```


This is all well and good, but I really don't want to add another step to my processing pipeline. I want to automate it.
  
## Hazel

Next, we install Hazel, which allows us to monitor files and run actions against them. In my previous post [FastRawViewer]({% post_url 2025-03-23-FastRawViewer %}) , I showed how I select files directly from my SD Card. I'm using the "_Selected" directory from this process as my monitoring target. As I have multiple SD cards, and never really know where the "_Selected" folder will be on each of them, I have to monitor all subdirectories but only process files under these directories.

## Set Up a Folder  

In my case, I just monitor all `/Volumes` as this is where my SD cards mount. Hazel doesn't monitor subdirectories by default, so we need to set up two rules: one to monitor subdirectories, and the other to run our exiftool command.

Click "Add folder"

![image](/assets/img/2025-09-29-Populating-EXIF-Subject-Distance-in-Lightroom/Pasted-image-20250929212938.png)
  
A popup will appear. I could not navigate to /Volumes so I had to enter it manually after doing a "Command + Shift + G" and typing it in.

  
### Rule 1: Process ORF Files


Add a new rule under the folder with the following configuration:

![image](/assets/img/2025-09-29-Populating-EXIF-Subject-Distance-in-Lightroom/Pasted-image-20250929205715.png)


The script only updates files in which the Subject Distance is not the same value as the proprietary Focus Distance field (it could have no value, for instance). This avoids updating already processed files and Lightroom picking them up as new files again.


```bash
if ORF)$ ; then
    focus=$(/opt/homebrew/bin/exiftool -s -s -s -FocusDistance "$1")
    subject=$(/opt/homebrew/bin/exiftool -s -s -s -SubjectDistance "$1")
    
    # Only update if they're different and FocusDistance exists
    if  -n "$focus" && "$focus" != "$subject" ; then
        /opt/homebrew/bin/exiftool '-SubjectDistance<FocusDistance' -overwrite_original "$1"
    fi
fi
```

### Rule 2: Iterate Subfolders

To process subdirectories, a second rule is needed.

https://www.noodlesoft.com/manual/hazel/advanced-topics/processing-subfolders/

![image](/assets/img/2025-09-29-Populating-EXIF-Subject-Distance-in-Lightroom/Pasted-image-20250929205808.png)

  

Hazel runs in the background performing this operation on any file under the "_Selected" directories it finds. Once set up, you don't need to interact with it.

## Lightroom

In the library mode of Lightroom Classic, we can choose which EXIF data to show:
  

![image](/assets/img/2025-09-29-Populating-EXIF-Subject-Distance-in-Lightroom/Pasted-image-20250929212510.png)


You can add new fields by selecting the "Customize" button.
  

Now when you import files from the "_Selected" directory off your SD Card, Subject Distance should always be populated.