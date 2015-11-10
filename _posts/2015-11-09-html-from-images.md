---
layout: post
title: Making Html of Images
permalink: making-html-of-images
tags: html,pdfs
---

Here's an idea.

Let's say you want to manipulate lots of PDFs. Some tasks demonstrating what I mean:

- <b>Extract table data</b>:

  PDFs frequently have tabular information. These can be explicit or implicit (like a table of contents). Regardless, it isn't easy to get this information. There are a number of libraries that try. The best open source one I've found is [Tabula](http://www.tabula.technology). None of them, however, work reliably enough so that we can pipe many PDFs at once through the system and expect consistently solid results.

- <b>Convert images to text</b>:

  It's common for PDFs to have images. This can be innocuous like an embedded diagram supporting the text. Frequently it is not so simple, like when entire pages are images. They might even <i>look</i> normal, but they can't be selected, copied, or searched. Every audience hates this. Converting these images to text makes them much more accessible.

- <b>Translating the document</b>:

  To translate a PDF, you have to extract the text into coherent "blobs" so that structures like paragraphs remain together and aren't split by line. Then translate each of those blobs into a target language before reproducing the document as faithfully as possible. This is easy with HTML - the blobs are already together - but is difficult when the source document is similar to a PDF. There, concepts like blobs don't exist on the right granularity. And so reliably reproducing the document is very hard.

All of these are gritty problems that have existed for years without great solutions.

Imagine if the PDF was instead an image. By doing that, we ignore the PDF format in favor of utilizing image processing software. This makes it easy to extract table positions because that's just looking for grid lines with OpenCV. On the other hand, it's hard to extract non-romanic text, which needs OCR software. There aren't any good open-source implementations for that that I'm familiar with[^1].

But what if we could skip developing an OCR algorithm for every language? Instead, train a model to generate html from images. Every web page on the internet is free data. Step one would be to gather a lot of website htmls with their stylesheets. Then, munge the two together so that the stylesheets are inserted into the html. Now take images of the resulting pages. Perturb a single page as much as you like to generate more data.

The input to our model is the images and the output is the merged html. We train it to understand how to produce the html from the image[^2]. On the face of it, this looks like a very useful application that utilizes advances in understanding long-term memory dependencies and not much more. As a quick example of the memory dependencies, what if the entire page is just two columns? Then the resulting html could be a parent div with two child divs, each of which are fixed percentage width and floated left. The model has to learn that this large amount of information should all be in this one dom element. And if there is a white space separation some length down the page, then the model needs to grasp that.

But then again, this might be much harder than it looks. For one thing, html representations are not unique. There are other ways to reproduce the two-column grid that I mentioned above. Another consideration is that you can't really get anything wrong:

- Misalign dom element clojures and your html won't compile.

- Use a ```<span>``` tag when you meant to use a ```<div>``` tag and the widths will be off.

- Fail to get the right number of cells for every row and the table is going to be wonky.

The list continues. So why use html? Why not output absolute positioned characters and lines? This is closer to the OCR route and wouldn't depend on learning and remembering html over long sequences of text.

I have two answers. The first is that we have an unbounded amount of html training data. You can also easily perturb an html document in countless ways and produce a different image each time. The second answer is that this is much more expressive and powerful! A working solution would resolve each of the examples above. The same isn't true for simpler approaches[^3].

So ... <i>how</i> hard is this? It might be harder than machine translation. In both cases, there isn't an indempotent solution, the ordering matters a great deal, and the representations are different. Here, though, there is little credit for partial correctness - in MT, there is some. The length at which you have to remember dom elements is more than it is in sentence translation. And the representations may not even have the same embedding space. What I mean is that an English string and a German string can be reduced to approximately the same vector embedding. This is because they are closely related encodings of the same representation. Is that true of html describing a website and an image of the website?

[^1]: The best one is still [Tesseract](). One that I'm seeing gain steam is [Ocropy](). If you know of any better, especially with open-source language packs, please do let me know!
[^2]: An amusing thought is to see what happens when you train it on different html structures. We could have a Bootstrap bot, a Shopify bot, and a Google bot that all strive to turn images into pages that look like how sites with those signature frameworks are displayed.
[^3]: They also aren't as elegant as the images -> html approach. That counts.