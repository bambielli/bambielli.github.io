---
title:  "Optimizing Images for the Web"
date:   2017-02-05 15:01:00
category: til
tags: [performance]
---

TIL of some simple tricks to optimize images for the web.

I ran Google's [page speed insights][psi]{:target="_blank"} (PSI) on my site yesterday. While my mobile design score was quite high (if you're on mobile currently, you're welcome!) some of the performance metrics on both mobile and desktop were being dragged down due to the size of the images I was serving on the landing page and the travel pages.

My PSI score was an abysmal 0/100 on the [travel][travel]{:target="_blank"} page, due to the size of the images I was serving! I was directed by PSI to [this article][article]{:target="_blank"} by Google client side performance guru Ilya Grigorik, which offered a couple of quick suggestions for optimization of images to reduce their size and get your pages loaded faster.

### Step #1: JPEG vs PNG

To be honest, I didn’t actually know the difference between JPEG and PNG image formats. I picked JPEG randomly when uploading my images to my site. For my purposes, though, I appear to have chosen right: JPEG images can undergo a "lossy" compression step that PNGs cannot. This allows the image owner to manipulate the quality of a JPEG image by executing a compression algorithm to remove pixel data from the original image. This has the potential of greatly reducing the ending file size of JPEGs, since much of the original data won’t even be there!

PNGs do not have an equivalent lossy compression option. The only way to reduce the file size of a PNG is to manipulate the resolution of the color palate that is stored for the image (reducing color fidelity) or by reducing the physical dimensions of the image itself. 

I wanted the images on my cover page to load quickly, so I decided to reduce the quality of the JPEGs for both the cover photo and the picture of me in the sidebar via the lossy compression step. These images were both good candidates for this type of compression, because they both have CSS overlays applied on top of them which obscure some of the resolution loss. 

After performing this step alone, I reduced the number of bytes served on initial page load of my blog’s landing page by close to 400Kb!

### Sidebar: Lossless Compression

There is a second compression step that both PNGs and JPEGs go through called a "lossless" compression step. Unlike lossy, lossless compression reduces the overall file size of a saved image without removing any pixel data from the result. This allows the image to be reconstructed with the same resolution / fidelity of the original since no data loss happens during this step. It doesn’t reduce file size as drastically as lossy does, but it does have a small effect.

### Step #2: Removing Image Metadata

Not only do images contain pixel data, but they also often contain a bunch of metadata like camera information, color profile, geolocation, etc... 

I ran my photos through a tool called [Image Optim][io]{:target="_blank"} that strips out that metadata for you. Running through this step reduced image file sizes across my site by an average of 12%.

### Step #3: Natural vs Rendered Image Dimensions

A final suggestion that I implemented from the article was to check the "natural" vs "rendered" image dimensions on a page. Natural dimensions refer to the dimensions of the file that was served, while rendered dimensions refer to the dimensions of the file that is actually rendered in your view. Serving images that are of larger dimension than those needed for your page is clearly wasteful. You can check this information by inspecting an image that is rendered on your page, and hovering over the resource path in the HTML (see figure below). 

<figure>
  <img src="/assets/images/NaturalVsActual.jpg">
  <figcaption>Natural vs Rendered File Size for me.jpg</figcaption>
</figure>

So per the inspector, the natural dimensions of the file I was shipping to the browser was 512 x 512 pixels, while the rendered dimensions of that image was only 110 x 110 pixels. *That is a difference of 250044 pixels, the equivalent of 25Kb, being unnecessarily shipped!* No wonder google PSI flagged this as a problem asset. 

I reduced the image dimensions accordingly... I promise I did :) Go ahead, check it out by inspecting it in the sidebar: you shouldn't see the "natural" size anymore when you hover over the asset path.

### Conclusion

When I first added the images to my site, I uploaded them with no real consideration of performance or optimization that could be done to make the experience better for my end users. 

It was lazy. 

Now that I have a few simple tools and tricks to reduce file sizes while still maintaining image quality, I'll be much more cognizant of the sizes of my assets moving forward. I also confirmed that Cloudflare, which is the CDN proxy in front of my site, enables browser caching of these assets, so they aren't being unnecessarily downloaded over and over (particularly important on mobile). 

Expecting a mobile visitor - really any visitor - to be ok with downloading MEGABYTES of assets is unacceptable. With the web shifting more and more to a "mobile first" mindset, simple things like making sure your assets are groomed for production will separate your website from the rest.

[psi]: https://developers.google.com/speed/pagespeed/insights/
[travel]: /travel/
[article]: https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization
[io]: https://imageoptim.com/mac
