---
layout: post
title: Layer Make
blurb: "The real novelty, though, is that this uncovers a different way of handling media where people push their video as source material instead of maintaining ownership. Your video is only as useful as the final form and everyone in the creation chain gets rewarded."
permalink: layer-make
tags: product
---

## Three Guiding Examples.

In 2004, the movie [Downfall](https://en.wikipedia.org/wiki/Downfall_(2004_film)) came out. It is much better known today for its [legacy](https://en.wikipedia.org/wiki/Downfall_(2004_film)#Legacy) of memes. The [original](https://www.youtube.com/watch?v=lreKS6DGBMM) meme was uploaded to Youtube in 2007. There is a website dedicated to them, a YT channel for them with 200k subscribers, and copious derivative content with 1m+ views. They are each a particular clip from the film in its original form except that English subtitles have been added to the bottom that jive with the expression in the scene but is completely different from the actual meaning behind the German speech. They are poignant in time, very viral, and funny af.

Recently, a singer named Rich Brian pulled off some fantastic marketing [voodoo](https://myx.abs-cbn.com/features/25148/rich-brian-love-in-my-pocket-greenscreen-video) and released a [video](https://youtu.be/qBG-tyMWNeo) of him singing in front of a green screen, telling fans to do whatever they wanted with his video “because [i] ran out of budget for vfx lol.” His fans obliged and [released](https://twitter.com/jojischeerios/status/1282286631404199937) [countless](https://twitter.com/richbrian/status/1282178897434406913) [interpretations](https://twitter.com/ryluhee/status/1281654441435779073).

Finally, there is a new company called [Papercup](http://papercup.ai/). Their goal is to translate your content into other languages but still have it sound like you. They are doing this for DIY videos where someone has, say, built a wall-mounted standing desk in English and now wants to get interest from the spanish-speaking communities.

## Abstraction.

We can generalize the above with the idea of layers from Photoshop. Each of the examples are a set of user-generated layers built on an underlying video.

1. Downfall memes:
    1. First apply the clip layer to the Downfall video at a certain start and end.
    2. Then apply the caption layer at all of the audio locations in the clip.
2. Rich Brian memes:
    1. First apply the greenscreen layer to a video.
    2. Then apply the background addition layer to a video.
    3. … Potentially apply more object addition / removal layers.
3. Papercup translations:
    1. First apply the caption layer to a video.
    2. Then apply the translation layer to those transcriptions.
    3. Then apply a text-to-speech layer to the resulting captions.

This abstraction encompasses a large category of possibilities for both artistic creation (/ memes) and for legitimate utility. The interface of layers built on videos immediately becomes a way for people to not just build on others’ videos but also others’ layers and additionally get credited for it through the layering mechanism having known authorship.

<img src="/assets/layermake-1.png">

## Why? A new model for video.

The real novelty, though, is that this uncovers a different way of handling media. The change is now people willingly give up their video as source material instead of thinking about it in terms of ownership. Your video is only as useful as the final form(s) and everyone in the creation chain gets rewarded.

We already know that people do these. Look on YouTube and you can see countless examples of people editing and metamorphosing videos for new expression. Look on VideoLectures for a central repository of video lectures on which the annotations have been grafted. TikTok has examples of people building on top of others’ videos through duets. And Reddit has many photoshop requests that turn into artistic interpretation battles.

However, all of those models are built on users’ original content remaining unchanged and so each step becomes a solo endeavor. It’s annoyingly difficult for me to add dubbing to a Downfall meme because I have to go through and get every caption by hand. None of these were designed with collaboration on a single video in mind with someone you may have never met and may not even have the same intent. With this approach, the person dubbing can access the captions from the caption layer really easily in the edit mode.

Maybe Joe wants to annotate the lecture you uploaded with a small can of coke in the professor’s hand while Jerry wants to add lecture notes that appear at timely locations as thought bubbles. Let them both do it; let the most popular experience rise to the top and/or everything get shared; let everyone involved in the creation benefit.

## TikTok and Sway (Aug 1 edit).

It was pointed out to me (h/t Lucy) that TikTok can be seen as a lip syncing layer in that their [prime](https://www.techbuzzchina.com/bytedance/kelly-nan-zhang-bytedance-china-ceo-and-creator-of-douyin-tiktok-how-we-created-a-product-with-a-billion-views-a-day-in-18-months-part-1) [innovation](https://www.techbuzzchina.com/bytedance/kelly-nan-zhang-bytedance-china-ceo-and-creator-of-douyin-tiktok-how-we-created-a-product-with-a-billion-views-a-day-in-18-months-part-2) was replacing the sound in their video with sound (dominantly music) from other videos in their feed. There was not an existing easy-to-use tool that allowed for this interaction and it was (evidently) a good one.

Another layer is the one that [Sway](https://getsway.app/) is building, originally founded upon [Everybody Dance Now](https://arxiv.org/pdf/1808.07371.pdf). They can do a person-swap of a motion so that you become a really good dancer or a martial artist, etc.

We can think of both TikTok and Sway as being attempts to build products off of one layer. TikTok showed that their chosen layer was in great demand and then build on top of that by introducing duets (side-by-side video). It is unclear how Sway is doing but they seem to be advancing into further areas by letting users position stickers of their motions and not just single videos of their motions.

As far as I can tell, both of these products position the video as being immutable and creates a copy each time. That keeps the model as the traditional one where every video is its own island and owned by a single creator. Yes, it’s true that TikTok videos have lineage, but that lineage is not transparent, nor is it important to the resulting product view; Finding a new duet similar to one you already like relies on TikTok surfacing it. The layered product perspective would keep these all in one central place where they can be more easily binge viewed, mutated, and built upon.

## How? Example layers.

We want the layers to be mixed and matched on any given video. The translations in the Papercup example don’t actually need to incorporate the translation layer. If Jane made the captions, Dick the translations, and Harry the TTS, Mary can make a new TTS layer that uses Harry’s TTS voice but acts on Jane’s original captions. The video could be viewed in five different ways:

1. Original
2. Captions alone.
3. Captions with spoken dubbing using Jane and Mary’s layers.
4. Translated captions with Jane and Dick’s layers.
5. Translated captions with dubbing using Jane, Dick, and Harry’s layers.

I have listed a set of preliminary layers below. This can of course be started without all of them. It can also be started with inadequate tooling and letting creators bring everything to the table. A lot of the magic in it though comes from having strong tooling that creates for easy-to-use and compatible layers.

1. Subtitles: Text on screen while some audio is happening. We can help the creator by guiding them to the times when audio is playing. They type in what they want to add. Awesome [example](https://www.youtube.com/watch?v=49OY0kV0n_M) from a fan producing a video for Eminem.
2. Dubbing: Replace some audio track with another. We help the creator by both guiding them to the times when there is audio playing as well as helping them position their new tracks within those times.
3. Faceswap: Replace some target face T with another face. We can help the creator by finding all of the Ts, assuming they want to swap all of them, and then letting them limit areas where they don’t want that face swapped.
4. Annotate: Mark a video with a person (“Obama”) or event tag (“Sunset”) at any start/end interval. This both pops up in the scrubber (like soundcloud) and becomes searchable in the database to then weave into other clips.
5. Weaves: Weave in clips from other videos at certain times. You can search for these clips in our database or upload your own. This lets you do sports highlights or combinations like this amazing one where [Obama sings Ed Sheeran](https://www.youtube.com/watch?v=UhVk7D659pM) or this fan [production](https://www.youtube.com/watch?v=EZIUx_sTrYM).
6. Highlights: Put fact/link boxes in the video that appear at those times and stay for some time. These can be factoids (thought bubble on a professor) or links to places like MathOverflow or Quora.
7. VoiceChange: Change a voice to that of someone else. We can help the creator with a library of celebrities or let them upload their own.
8. RemoveObject: Remove an object from the video. This requires us to help the user with semantic understanding in order for the user to pick out the object.
9. AddObject: Add an object to the video. This requires us to help the user with semantic understanding in order for it to be well done.
10. Side-by-side: Position two videos side by side. See TikTok’s duets for an example. ML can help here in that it can let us [align the frames](https://arxiv.org/abs/1904.07846).
11. Drawsistant**:** The user [draws](https://vm.tiktok.com/JjJ8Und/) something and we help them. This can be done with content-aware ML that then [completes](https://twitter.com/yaroslav_ganin/status/1180120687131926528?lang=en) [the](https://experiments.withgoogle.com/sketch-rnn-demo) [drawing](https://twitter.com/oriolvinyalsml/status/1162693942607077378?lang=en) and gifts the object affordances. Imagine drawing a bow and arrow on top of your video and it being able to fire.

## Address failure points.

Besides the common trope of “someone else does it first”, I think the biggest concern is that the design is all wrong. There’s a ton of potential to have the tooling be easier as well as having ML help the tools but none of those matter if the design isn’t right for getting people piqued by this mode of behavior re videos being mutable.

We already see people doing these actions (both the “give to the world” as well as the “make funny stuff with other peoples” works) and so there is demand. Is that demand and this design approach enough to convince people to adopt this platform? I think the answer is yes if the tooling is easy enough because we see that story play over frequently such as with image filters and IG, video editing and TikTok, etc.

But maybe that’s all wrong. Why does 3blue1brown put his wares on Youtube? Probably because that’s the biggest market for him to reach and he can get lots of views and support himself. This doesn’t have that for a long time so does that stifle a big part of the video segment, namely the more serious videos that make this a legitimate utility? Maybe, probably even likely. But if it opens up the creative outlet with easier tooling, then that’s a big plus that would draw people in and the rest can blossom from there. It’s also not a secret - there are quite a few companies doing “[creative](http://facet.ai/) [content-aware](http://runway.ml/) [editing](http://rosebud.ai/)” right now.
