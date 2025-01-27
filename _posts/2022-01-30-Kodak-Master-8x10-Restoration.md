---
layout: post
title: "Restoring a Kodak Master 8x10 Camera"
date: 2022-01-30
categories: photography restoration
img: KodakMaster8x10/After/4.jpg
---

In December 2021, I purchased a wreck of an 8x10 camera from eBay. I knew exactly what I was looking for, but the options were slim. These cameras rarely came up for sale, and prices were steadily increasing. Looking at the photos in the listing, I wasn’t entirely sure what I was getting myself into. Here are the original photos from the eBay listing.

<div class="image-gallery-container">
  <div class="image-gallery">
    <img src="/assets/img/KodakMaster8x10/Before/1.jpg" alt="Kodak Master - Before - Image 1" data-full="/assets/img/KodakMaster8x10/Before/1.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/2.jpg" alt="Kodak Master - Before - Image 2" data-full="/assets/img/KodakMaster8x10/Before/2.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/3.jpg" alt="Kodak Master - Before - Image 3" data-full="/assets/img/KodakMaster8x10/Before/3.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/4.jpg" alt="Kodak Master - Before - Image 4" data-full="/assets/img/KodakMaster8x10/Before/4.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/5.jpg" alt="Kodak Master - Before - Image 5" data-full="/assets/img/KodakMaster8x10/Before/5.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/6.jpg" alt="Kodak Master - Before - Image 6" data-full="/assets/img/KodakMaster8x10/Before/6.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/7.jpg" alt="Kodak Master - Before - Image 7" data-full="/assets/img/KodakMaster8x10/Before/7.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/8.jpg" alt="Kodak Master - Before - Image 8" data-full="/assets/img/KodakMaster8x10/Before/8.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/9.jpg" alt="Kodak Master - Before - Image 9" data-full="/assets/img/KodakMaster8x10/Before/9.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/10.jpg" alt="Kodak Master - Before - Image 10" data-full="/assets/img/KodakMaster8x10/Before/10.jpg">
    <img src="/assets/img/KodakMaster8x10/Before/11.jpg" alt="Kodak Master - Before - Image 11" data-full="/assets/img/KodakMaster8x10/Before/11.jpg">
  </div>
</div>

<div id="lightbox" class="lightbox">
  <span class="close-button">&times;</span>
  <span class="nav-button prev-button">&#10094;</span>
  <span class="nav-button next-button">&#10095;</span>
  <img class="lightbox-content" id="lightbox-img">
  <div id="lightbox-caption"></div>
</div>

<style>
  .image-gallery-container {
    width: 100%;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
  }
  .image-gallery {
    display: flex;
    gap: 10px;
    padding: 10px 0;
  }
  .image-gallery img {
    height: 200px;
    width: auto;
    object-fit: cover;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    cursor: pointer;
    transition: transform 0.3s ease;
  }
  .image-gallery img:hover {
    transform: scale(1.05);
  }
  .lightbox {
    display: none;
    position: fixed;
    z-index: 999;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.9);
    justify-content: center;
    align-items: center;
  }
  .lightbox-content {
    max-width: 90%;
    max-height: 90%;
    object-fit: contain;
  }
  .close-button {
    position: absolute;
    top: 15px;
    right: 35px;
    color: #f1f1f1;
    font-size: 40px;
    font-weight: bold;
    cursor: pointer;
  }
  #lightbox-caption {
    position: absolute;
    bottom: 20px;
    left: 0;
    right: 0;
    text-align: center;
    color: #fff;
    padding: 10px;
    background-color: rgba(0, 0, 0, 0.5);
  }
  .nav-button {
    color: white;
    font-size: 30px;
    font-weight: bold;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    cursor: pointer;
    user-select: none;
    -webkit-user-select: none;
    padding: 16px;
    background-color: rgba(0,0,0,0.3);
  }
  .prev-button {
    left: 15px;
  }
  .next-button {
    right: 15px;
  }
  .nav-button:hover {
    background-color: rgba(0,0,0,0.8);
  }
</style>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const lightbox = document.getElementById('lightbox');
    const lightboxImg = document.getElementById('lightbox-img');
    const lightboxCaption = document.getElementById('lightbox-caption');
    const closeButton = document.querySelector('.close-button');
    const prevButton = document.querySelector('.prev-button');
    const nextButton = document.querySelector('.next-button');
    const galleryImages = document.querySelectorAll('.image-gallery img');

    function showNextImage() {
      const currentImg = Array.from(galleryImages).find(img => img.src === lightboxImg.src || img.getAttribute('data-full') === lightboxImg.src);
      const nextImg = currentImg.nextElementSibling || galleryImages[0];
      lightboxImg.src = nextImg.getAttribute('data-full') || nextImg.src;
      lightboxCaption.textContent = nextImg.alt;
    }

    function showPreviousImage() {
      const currentImg = Array.from(galleryImages).find(img => img.src === lightboxImg.src || img.getAttribute('data-full') === lightboxImg.src);
      const prevImg = currentImg.previousElementSibling || galleryImages[galleryImages.length - 1];
      lightboxImg.src = prevImg.getAttribute('data-full') || prevImg.src;
      lightboxCaption.textContent = prevImg.alt;
    }

    galleryImages.forEach(img => {
      img.addEventListener('click', function() {
        lightbox.style.display = 'flex';
        lightboxImg.src = this.getAttribute('data-full') || this.src;
        lightboxCaption.textContent = this.alt;
      });
    });

    function closeLightbox() {
      lightbox.style.display = 'none';
    }

    closeButton.addEventListener('click', closeLightbox);
    prevButton.addEventListener('click', function(e) {
      e.stopPropagation();
      showPreviousImage();
    });
    nextButton.addEventListener('click', function(e) {
      e.stopPropagation();
      showNextImage();
    });

    lightbox.addEventListener('click', function(e) {
      if (e.target === this) {
        closeLightbox();
      }
    });

    document.addEventListener('keydown', function(e) {
      if (lightbox.style.display === 'flex') {
        if (e.key === 'Escape') {
          closeLightbox();
        } else if (e.key === 'ArrowRight') {
          showNextImage();
        } else if (e.key === 'ArrowLeft') {
          showPreviousImage();
        }
      }
    });
  });
</script>

When the camera arrived, it seemed to be in decent enough condition. The front standard had a homemade lens board attached, but it was easy to remove, and there was no major damage. These cameras typically use an obscure pressed metal lens board, which isn’t easy to find, so I understood why someone might have opted for a DIY solution.

## The Restoration Process

After stripping the camera down, I had some decisions to make. The camera was too far gone to keep it original, and honestly, I wasn’t a fan of the original color scheme anyway. I decided to use black wrinkle paint—the kind often used on car parts. Interestingly, the Kodak Master camera originally had a wrinkle paint finish from the factory.

Restoring this camera in the winter posed a challenge with the wrinkle paint. To achieve the desired effect, a warm summer day is ideal. Instead, I spray-painted the parts and cured them in my grill. Surprisingly, it worked! All the brass components were polished and coated with several layers of lacquer for protection.

The ground glass was worn, and most of the frame lines had faded. I decided to regrind it to restore its functionality.

Here’s the final result after the restoration:

<div class="image-gallery-container">
  <div class="image-gallery">
    <img src="/assets/img/KodakMaster8x10/After/1.jpg" alt="Kodak Master - After - Image 1" data-full="/assets/img/KodakMaster8x10/After/1.jpg">
    <img src="/assets/img/KodakMaster8x10/After/2.jpg" alt="Kodak Master - After - Image 2" data-full="/assets/img/KodakMaster8x10/After/2.jpg">
    <img src="/assets/img/KodakMaster8x10/After/3.jpg" alt="Kodak Master - After - Image 3" data-full="/assets/img/KodakMaster8x10/After/3.jpg">
    <img src="/assets/img/KodakMaster8x10/After/4.jpg" alt="Kodak Master - After - Image 4" data-full="/assets/img/KodakMaster8x10/After/4.jpg">
    <img src="/assets/img/KodakMaster8x10/After/5.jpg" alt="Kodak Master - After - Image 5" data-full="/assets/img/KodakMaster8x10/After/5.jpg">
    <img src="/assets/img/KodakMaster8x10/After/6.jpg" alt="Kodak Master - After - Image 6" data-full="/assets/img/KodakMaster8x10/After/6.jpg">
    <img src="/assets/img/KodakMaster8x10/After/7.jpg" alt="Kodak Master - After - Image 7" data-full="/assets/img/KodakMaster8x10/After/7.jpg">

  </div>
</div>

<div id="lightbox" class="lightbox">
  <span class="close-button">&times;</span>
  <span class="nav-button prev-button">&#10094;</span>
  <span class="nav-button next-button">&#10095;</span>
  <img class="lightbox-content" id="lightbox-img">
  <div id="lightbox-caption"></div>
</div>

<style>
  .image-gallery-container {
    width: 100%;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
  }
  .image-gallery {
    display: flex;
    gap: 10px;
    padding: 10px 0;
  }
  .image-gallery img {
    height: 200px;
    width: auto;
    object-fit: cover;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    cursor: pointer;
    transition: transform 0.3s ease;
  }
  .image-gallery img:hover {
    transform: scale(1.05);
  }
  .lightbox {
    display: none;
    position: fixed;
    z-index: 999;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.9);
    justify-content: center;
    align-items: center;
  }
  .lightbox-content {
    max-width: 90%;
    max-height: 90%;
    object-fit: contain;
  }
  .close-button {
    position: absolute;
    top: 15px;
    right: 35px;
    color: #f1f1f1;
    font-size: 40px;
    font-weight: bold;
    cursor: pointer;
  }
  #lightbox-caption {
    position: absolute;
    bottom: 20px;
    left: 0;
    right: 0;
    text-align: center;
    color: #fff;
    padding: 10px;
    background-color: rgba(0, 0, 0, 0.5);
  }
  .nav-button {
    color: white;
    font-size: 30px;
    font-weight: bold;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    cursor: pointer;
    user-select: none;
    -webkit-user-select: none;
    padding: 16px;
    background-color: rgba(0,0,0,0.3);
  }
  .prev-button {
    left: 15px;
  }
  .next-button {
    right: 15px;
  }
  .nav-button:hover {
    background-color: rgba(0,0,0,0.8);
  }
</style>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const lightbox = document.getElementById('lightbox');
    const lightboxImg = document.getElementById('lightbox-img');
    const lightboxCaption = document.getElementById('lightbox-caption');
    const closeButton = document.querySelector('.close-button');
    const prevButton = document.querySelector('.prev-button');
    const nextButton = document.querySelector('.next-button');
    const galleryImages = document.querySelectorAll('.image-gallery img');

    function showNextImage() {
      const currentImg = Array.from(galleryImages).find(img => img.src === lightboxImg.src || img.getAttribute('data-full') === lightboxImg.src);
      const nextImg = currentImg.nextElementSibling || galleryImages[0];
      lightboxImg.src = nextImg.getAttribute('data-full') || nextImg.src;
      lightboxCaption.textContent = nextImg.alt;
    }

    function showPreviousImage() {
      const currentImg = Array.from(galleryImages).find(img => img.src === lightboxImg.src || img.getAttribute('data-full') === lightboxImg.src);
      const prevImg = currentImg.previousElementSibling || galleryImages[galleryImages.length - 1];
      lightboxImg.src = prevImg.getAttribute('data-full') || prevImg.src;
      lightboxCaption.textContent = prevImg.alt;
    }

    galleryImages.forEach(img => {
      img.addEventListener('click', function() {
        lightbox.style.display = 'flex';
        lightboxImg.src = this.getAttribute('data-full') || this.src;
        lightboxCaption.textContent = this.alt;
      });
    });

    function closeLightbox() {
      lightbox.style.display = 'none';
    }

    closeButton.addEventListener('click', closeLightbox);
    prevButton.addEventListener('click', function(e) {
      e.stopPropagation();
      showPreviousImage();
    });
    nextButton.addEventListener('click', function(e) {
      e.stopPropagation();
      showNextImage();
    });

    lightbox.addEventListener('click', function(e) {
      if (e.target === this) {
        closeLightbox();
      }
    });

    document.addEventListener('keydown', function(e) {
      if (lightbox.style.display === 'flex') {
        if (e.key === 'Escape') {
          closeLightbox();
        } else if (e.key === 'ArrowRight') {
          showNextImage();
        } else if (e.key === 'ArrowLeft') {
          showPreviousImage();
        }
      }
    });
  });
</script>

## Solving the Lens Board Problem

One issue remained: the lens board. I went through several 3D-printed designs I created myself. I even developed models that could mount a Sinar shutter to the front, allowing the use of barrel lenses. Ultimately, I designed a Kodak Master to Sinar board adapter. I then purchased a metal Sinar-to-Technika adapter, which fit into my Kodak Master-to-Sinar adapter. This setup allowed me to standardize on the Technika mount, making lens removal and swapping much easier.

This project was a labor of love, and I’m thrilled with how it turned out. The camera is now fully functional and ready to capture stunning 8x10 images once again.
