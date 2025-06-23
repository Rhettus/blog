---
layout: post
title: "Lightroom - Am I stuck With It?"
date: 2025-06-22 12:00:00 +0000
categories: photography
---

With Adobe increasing the price of the Lightroom + Photoshop Photography Plan, I started seriously looking for alternatives. I’ve never loved the idea of being locked into Adobe products, and the TOS changes and price hikes left a sour taste in my mouth. In the past, I tried the usual suspects—DxO PhotoLab and Darktable—but they fell short of delivering the full workflow I need. 

### **My Workflow Needs**

_Before evaluating tools, here are the core stages of my workflow:_

- Cull/select keepers
- Ingest from SD card
- Organize into long-term archive (DAM)
- Rename/add EXIF/keywords
- Lens correction & denoise
- Edits (crop, color, retouch, denoise, masking)
- Panorama merge
- Export (often with borders)


I decided to break down my requirements into a matrix. These are the tasks I require for my photo workflow:

| Application       | Quick Culling | Ingest w/o Duplicates | DAM | EXIF | Lens Correction | AI Denoise | AI Masking | Panoramas | Add Borders | Price            |
| ----------------- | ------------- | --------------------- | --- | ---- | --------------- | ---------- | ---------- | --------- | ----------- | ---------------- |
| FastRawViewer     | Y             |                       |     |      |                 |            |            |           |             | $20              |
| Photo Mechanic    | Y             | Y                     |     | Y    |                 |            |            |           |             | $200             |
| Darktable         |               | Y                     | B   | Y    | Y               |            |            |           | Y           | Free             |
| Photomator        |               |                       | B   | Y    |                 | Y          | Y          |           |             | $30/year         |
| Excire Photo      | Y             |                       | Y   | Y    |                 |            |            |           |             | $200             |
| PTGui             |               |                       |     |      |                 |            |            | Y         |             | $205             |
| DxO PureRAW       |               |                       |     |      | Y               | Y          |            |           |             | $120             |
| Hugin             |               |                       |     |      |                 |            |            | Y         |             | Free             |
| Lightroom Classic |               | Y                     | Y   | Y    | Y               | Y          | Y          | Y         | Y – plugin  | $180/year        |
| Photoshop         |               |                       |     |      | Y               | Y          | Y          | Y         | Y           | Bundled with LrC |

**Y = Yes**  
**B = Basic Support**

The table gives a quick view of each application's capabilities, but the devil is in the details. Let’s run through the pros, cons, and recommendations for each:

---

### **FastRawViewer**
*Culling and selecting photos*

Used to select keepers directly from an SD card.

**Pros:**
- Does exactly what you want in the selection process.
- Works with RAW files, is very fast, and the controls are intuitive.

**Cons:**
- Can’t really be used for ingesting photos into a DAM; still requires a manual step.
- Weak in renaming files or adding EXIF data beyond stars and color labels.

**Recommendation:**  
Highly recommended, especially considering the price. Its main rival is Photo Mechanic, which costs about 10× more.

---

### **Photo Mechanic**
*Photo ingest, renaming, and metadata entry*

**Pros:**
- Excellent for bulk renaming, adding EXIF metadata, and skipping already-imported files.

**Cons:**
- Requires copying the SD card to a staging area to get the most out of it.
- Controls feel clunky.
- Only uses the embedded JPEG in RAW files for previews—FastRawViewer is better for evaluating focus and detail.

**Recommendation:**  
Best-in-class for renaming and EXIF enrichment. While I prefer FastRawViewer for culling, Photo Mechanic is highly recommended if you shoot professionally—despite the $200 price tag.

---

### **Darktable**
*Free photo editor and basic digital asset manager*

**Pros:**
- Free and packed with powerful modules.
- Particularly strong in highlight recovery and sharpening.

**Cons:**
- Steep learning curve.
- Editing takes longer compared to other editors.
- DAM is basic and directory-based.

**Recommendation:**  
Not recommended as an overall solution. It’s average in most areas, and the GUI isn’t great, making it a struggle for day-to-day use.

---

### **Photomator**
*Modern photo editor with AI tools (macOS only)*

**Pros:**
- Simple, clean interface.
- Offers advanced features like upscaling, AI denoise, and AI masking.
- Affordable.

**Cons:**
- macOS only.

**Recommendation:**  
A great, simple, and powerful editor at a reasonable price.

---

### **Excire Photo**
*AI-powered digital asset manager (DAM)*

**Pros:**
- Very fast indexing.
- AI-powered library search.
- Finds duplicates.
- Can sort by attributes like aesthetics, sharpness, etc.

**Cons:**
- Expensive for a DAM-only tool.

**Recommendation:**  
Much faster and more powerful than Lightroom’s Library module. If not for the price, it would be a no-brainer.

---

### **PTGui**
*Professional panorama stitching software*

**Pros:**
- Best-in-class stitcher.
- Offers both automatic and manual controls.

**Cons:**
- Price. At $200, it’s hard to justify for occasional use.

**Recommendation:**  
Only worth it if you frequently create panoramas and need the best results.

---

### **Hugin**
*Free panorama stitching tool*

**Pros:**
- Free.
- Capable when it works.
- Allows manual control points.

**Cons:**
- Often requires more manual work.

**Recommendation:**  
Recommended, especially considering it’s free.

---

### **DxO PureRAW**
*Pre-processing tool for lens correction and noise reduction*

**Pros:**
- Best-in-class for lens correction and AI denoising.
- Works as a standalone tool or plugin.
- Great as a pre-processor for Darktable (which lacks AI denoise) or Photomator (which lacks lens correction).
- Can also integrate with Lightroom Classic.

**Cons:**
- None worth noting.

**Recommendation:**  
Yes. The perpetual license is a strong selling point. Even if other tools offer similar features, DxO does them better. If you select something like Photomator as your editor, DxO really helps with its lens correction which Photomator does not do.

---

### **Lightroom Classic**
*All-in-one photo editor, DAM, and output platform*

**Pros:**
- Truly a one-stop shop.
- Handles importing, DAM, editing, masking, denoise, plugins, and printing.
- Supports HDR and panorama merging with good automation.
- Integrates well with Photoshop, DxO, etc.

**Cons:**
- Slow, especially when culling.
- Vendor lock-in.

**Recommendation:**  
I hesitate, but this is still the best-value photo editor. It does many things well, even if it’s not the best at any single one. Combined with Photoshop’s excellent generative AI tools, it’s hard to beat this combo. Just be careful not to get too locked into the Adobe cloud ecosystem, in case you want to leave it later.

---

### **Photoshop**
*Advanced pixel-level editor with generative AI features*

**Pros:**
- Excellent for composite editing, retouching, and generative AI tools.
- Complements Lightroom seamlessly.

**Cons:**
- Complexity and steep learning curve for some tasks.

**Recommendation:**  
Best paired with Lightroom. Especially valuable for photographers who also do creative editing, layout, or graphic work.

---

## Summary of Best Tools

- **Best Importer:** Photo Mechanic  
- **Best Selector/Culler:** FastRawViewer  
- **Best DAM:** Excire Photo  
- **Best Lens Correction/Denoise:** DxO PureRAW  
- **Best Panorama Stitcher:** PTGui  
- **Best Editor:** Lightroom  

**Best Overall:** Lightroom

At $15/month, Lightroom + Photshop actually ends up cheaper than combining the best-in-class tools. The ultimate setup for quality might be:

- Photo Mechanic – $200  
- Excire Photo – $200  
- DxO PureRAW – $120  
- PTGui – $200  
- Editor (choose one) – $20 to $300  

That adds up to $800+—and doesn’t even include a Photoshop alternative. Add **Affinity Photo** for $70 (no generative AI), and you’re pushing **$900**. These software usually require a discount upgrade purchase for major version releases, so there is on going costs every few years.

In contrast, **Lightroom + Photoshop for $180/year** isn’t that bad. Plus, it gives you a more cohesive, time-saving workflow.

But lets look at the bright side, you can totally replace Adobe Lightroom with alternatives which can produce the same or better output. I will be continuing to renew Adobe this year, making sure I don't tie myself to specific Adobe features, like the cloud offerings or web portfolio and I will re-evaluated each renewel.
