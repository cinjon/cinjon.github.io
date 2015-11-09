---
layout: post
title: Making Html of Images
permalink: making-html-of-images
tags: html,pdfs
---

Here's an idea.

Let's say you have lots of PDFs and you want to manipulate them. Some example tasks:

- <b>Extract table data</b>:

  PDFs frequently have tabular information. Sometimes, this is explicit where the tables are delineated with lines. Other times, it is implicit like in a table of contents. Regardless, it isn't easy to get this information. There are a number of libraries that try. The best open source one I've found is [Tabula](http://www.tabula.technology). None of them, however, work reliably enough so that we can pipe many PDFs at once through the system and expect consistently solid results.

- <b>Convert images to text</b>:

  It's common for PDFs to have images. Sometimes, this is innocuous like an embedded png that supports the document text (like a diagram). Frequently it is not so simple like when entire pages are images. They might even <i>look</i> like normal pages but they can't be selected, copied, or searched. Every audience hates this. Converting these images to their equivalent in text makes it much easier to access.

- <b>Translating the document</b>:

  Translating a PDF means extracting the text into coherent "blobs" so that structures like paragraphs remain together and not split by line. It means then taking those blobs and translating them into a target language before reproducing the document as faithfully as possible. This is easy when done with HTML - the blobs are already together and the spacing is already determined - but is very difficult when the source document is as loose as a PDF. There, concepts like blobs just don't exist on the right granularity. And so any effort to reliably reproduce the document is very hard.

All of these are gritty problems that have existed for years without great solutions.

Imagine if the PDF was instead an image. We are ignoring the instructions given in the PDF in favor of utilizing image processing software. This makes it quite easy to find and extract table positions but quite hard to extract non-romanic text. The former is just about looking for grid lines and can be done in Python with OpenCV. The latter needs OCR software. Outside of romanic text, open-source implementations are not very good [yet].

But what if we could skip developing an OCR algorithm for every language? Instead, we train a model to understand how to generate images with html. We have a lot of data - every web page on the internet suffices. Step one would be to gather a lot of website htmls with their stylesheets. Then, munge the two together so that the stylesheets are inserted into the html. Now take images of the resulting page.

The input to our model is the images and the output is the merged html. We train it to understand the how to produce the html from the image. It's not clear to me that this would be advancing the frontier of research or need anything beyond what we have today. It <i>does</i> look like a very useful application that utilizes advances in understanding long-term memory dependencies.

As a quick example of that, what if the entire page is just two columns? Then the resulting html could be a parent div with two child divs, each of which are fixed percentage width and floated left. The model has to learn that this large amount of information should all be in this one dom element. And if there is a white space separation some length down the page, then the model needs to grasp that.

An amusing thought is to see what happens when you train it on different html structures. We could have a Bootstrap bot, a Shopify bot, and a Google bot that all strive to turn images into pages that look like how sites with those signature frameworks are displayed.
