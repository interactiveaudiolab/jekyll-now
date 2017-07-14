---
layout: post
title: Hello World!
published: true
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

Hello World! We are happy to present the _nussl blog_, wherein we will provide a tutorial on many of the blind source separation algorithms contained within the Northwestern University Source Separation Library (`nussl`), an open-source source separation package. Usually, the blog posts will have code sprinkled throughout them, to root the concepts in real-world examples. But before we proceed onto explanations of the algorithms, we’d like to spend this post defining what “blind source separation” is and why it’s important.

## Table of Contents
<ul>
  <li><a href="#what-is-blind-source-separation">What is Blind Source Separation?</a></li>
  <li><a href="#why-is-source-separation-important">Why is Source Separation importatnt?</a></li>
  <li><a href="#what-is-a-stream-what-is-a-source">What is a stream? What is a source?</a></li>
  <li><a href="#underdetermined-source-separation">Underdetermined Source Separation</a></li>
  <li><a href="#onward">Onward!</a></li>
</ul>

## What is Blind Source Separation?

![Source separation is the process of extracting individual components of an auditory scene.]({{ site.baseurl }}/images/overview.png)

Here's a little demo of a mixture with isolated sources <a id="footnote-1-ref" href="#footnote-1">[1]</a>:

<iframe height="275" width="100%" src="https://interactiveaudiolab.github.io/nussl-blog/examples/hello_world_player.html" seemless>
</iframe>

Click the play button to hear the full mixture and click the circles next to the track names ("Bass", "Guitar", etc) to hear the sources in isolation, or click the speaker with the X to mute that channel. (This is an excerpt from a MIDI version of [Vulfpeck's Dean Town](https://www.youtube.com/watch?v=le0BLAEO93g).)

In general, audio source separation is the process of trying to extract an individual sonic _"stream"_ from its surrounding auditory scene. An example of this is extracting the sound of a single person talking when you’re in a crowded cocktail party, or focusing on the guitar solo when the entire band is also playing. The object of interest that we try to extract is called a source (e.g., the single person talking, or the guitar solo) and its surrounding auditory scene (e.g. the cocktail party, or full band playing) is called the mixture. 

The best way to actually do source separation (blind or non-blind) is to be a living creature that has ears and a brain that can do this all for you. If you’re reading this right now, you probably already have all the equipment you need to do source separation: a brain and two ears. (Heck, you might be doing source separation right now!) Coming in a distant second place for the source separation competition: computers. 

Possibly the best non-living-animal way to do (non-blind) source separation is to have control of how you make the recording, and never mix the sounds together in the first place. A recording studio is set up to do just this; the singer goes into an isolation booth to record themselves, isolated from the rest of the band. Similarly, with the clarinetist, and the string section: they each have a microphone and recording track dedicated to record the isolated sound that comes from each of their instruments. Then later the sound engineer can control the volume of each of these instruments individually, without affecting the volume of any other instrument.

We're going to assume we don't have access to the original isolated sources and can't record the separated sounds from a living brain. In this case, we have a mixture of sounds  in a stereo or mono recording that we wish to separate. When we have no information about how the mixture was recorded or about what the sources sound like, we call the process of doing source separation, _Blind_ <a id="footnote-2-ref" href="#footnote-2">[2]</a> Source Separation or BSS. When researchers say “source separation” they are often referring to blind source separation. Likewise, in this blog, you can assume that, when we say source separation, we really mean blind source separation (unless otherwise noted).

## Why is Source Separation important?

Okay, so why is source separation important? Why do computer scientists care to tackle this problem in the first place? Aside from being inherently interesting to the people who work on this, source separation has a lot of applications. We’ve already outlined the Amazon Echo use-case, but in fact many speech recognition systems are enhanced by the use of source separation technology and utilize source separation algorithms to clean up audio before being sent to speech-to-text algorithms. Another great use-case is to the creation of better hearing aides. A system that can isolate a speaker in a noisy room or eliminate particularly unwanted sounds at the push of a button is a system that will advance hearing aid technology. 

One of the most interesting applications, to this humble author, is the potential for end-user applications for musicians and sound engineers. A young musician can extract one of Paul Chamber’s bass tracks on an early Miles Davis recording so that she can listen to it more clearly as she learns it. A sound engineer can extract Edith Piaf’s voice for remastering. An electronic artist can clean up a sample they found. We can think of a million more examples like this. There do already exist a few tools that enable non-researchers to do source separation (Audionamix ADX Trax Pro, ISSE, SpectraLayers, etc.), but we still feel that there is a lot of room to explore interfaces that can further unleash this technology for non-academics.

Another compelling reason that we do this research—at least from a philosophical point of view—is for the advancement of artificial intelligence. As we mentioned, humans and other animals can easily focus on one sound in a mix, but it is a challenge for computers to accomplish this task. Much of the work in this field has been in tandem with work that researchers are doing to understand how hearing works in the brain. As such, we hope to one day be able to replicate human levels of hearing on a computer. Source separation is one of the many approaches to accomplishing this goal.



## What is a stream? What is a source?

Above, we refer to individual sonic _"streams"_ <a id="footnote-3-ref" href="#footnote-3">[3]</a>, a term popularized by psychologist Albert Bregman; by _"stream"_ we mean a set of connected auditory events. Often these are from the same sound source. Consider for a second, the sound of footsteps, or even a sentence. Each footstep is discreet, but they are all from the same source and go together. Similarly, the individual words in a sentence spoken aloud are typically grouped together, even if there is silence between words. A single note is just a note, but many notes ordered in a particular way can create a violin solo, or back beat. 
 

We now examine what a **source** _is_ and what it _is not_. While it may seem obvious, at first, the definition of “a source” is ill-defined. For instance, consider a guitar-bass-drums trio. The guitar playing a solo seems like a single source, while the bass player and drummer are also individual sources, but doesn't a guitar have five strings? Couldn't they each be considered a source? How about each of the individual drums on the drumset. What if the trio's song is playing on the radio at a cocktail party? Then the radio might be one source, and each individual talker might be a source too. We could go on and on and on…

The decision of what constitutes a source is context dependent. Therefore the best decision makers about what a source (or a stream) is are humans. Researchers have crafted many algorithms that implicitly define what a source is, thereby baking those decisions into the algorithms. These algorithms separate out sources based on _cues_ in an auditory scene. Such cues include repetition (everything that is in a repeating pattern is a source), timbre (all things that sound like a violin are a source), and spatialization (things coming from the left are a source). In the coming weeks, we will explain many of these solutions in this blog. 


## Underdetermined Source Separation

If you don’t have a multi-million-dollar recording studio, you can do source separation (and localization) with a process called [beamforming](https://en.wikipedia.org/wiki/Beamforming), which is where you have many microphones (called microphone arrays; the Amazon Echo has 7) and use slight differences in time-of-arrival between microphones to determine location of a sound and therefore a source. Beamforming happens in real-time, and requires a microphone array where the position of each microphone is known. But not all recordings were created with microphone arrays, and in fact almost all music was not. Furthermore, every single sound that has been analyzed by your brain has been input from just two microphones on the side of your head (your ears). When there are more streams than microphones, this is a situation we call _underdetermined_ source separation. This situation is ubiquitous and poses a very hard problem to solve. Most of the algorithms we put in `nussl` are for the underdetermined case.

To illustrate how difficult the problem actually is, let’s step back and take a gander at what an audio mixture actually is. The phenomenon that we call sound is a succession of very rapid changes in air pressure and displacement.

![Sound is quick changes in air pressure.]({{ site.baseurl }}/images/sound.png)

The way sound is captured by a computer is by using a microphone to turn those air pressure changes into voltage, and then using a chip to turn those voltages into numbers that a computer can read, store, manipulate, play back, etc.

![Sound is stored in a computer as a series of numbers that correspond to air pressure.]({{ site.baseurl }}/images/adc.png)


Once the sound is in the computer, it is stored as an array of numbers. We can think of this as a function, \\(f [t]\\), that outputs a value at a given discreet-valued timestep, \\(t\\). So the sound pictured above is stored as such:

\\[ f[t] = \[-1, 0, 1, 1, 3, 4, 3, ...\] \\]

A mixture is a typically a linear combination of sounds with some mixing parameter that scales the sound and determines how loud the sound is. For instance, let’s say we have a mixture, \\(m[t]\\), is a combination of two sources, \\(s_1[t]\\) and \\(s_2[t]\\), with mixing parameters \\(l_1\\) and \\(l_2\\), respectively. Then \\(m[t]\\) is thusly:

\\[ m[t] = l_1 s_1[t] + l_2 s_2[t] \\]

Both \\(s_1[t]\\) and \\(s_2[t]\\) are time series just like \\(m[t]\\) and \\(f[t]\\). But we only know what the resultant mixture looks like:

\\[ m[t] = \[12, 85, 123, -6, -45, 3, ...\] \\]

Now we get to the crux of the problem: how do we determine what \\(s_1[t]\\) and \\(s_2[t]\\) are when we only know what our mixture, \\(m[t]\\), is? This problem can be simplified by looking at the first value in the mixture, \\(m[t=0] = 12\\) and assume our mixing parameters are both 1. Now, we’re merely to trying to figure out what two (scaled) integers sum to 12, i.e., what are \\(s_1[t=0]\\) and \\(s_2[t=0]\\) such that \\(s_1[0] + s_2[0] = 12\\)? The number of possible pairs that sum to the value \\(m[0]\\) is infinite but only one pair is correct!

But uncompressed CD-quality audio is stored as 16-bit integers and sampled at 44.1 kHz. If we limit the range of possible values that our sources can be as such both sources are 16-bits, i.e., can be any value between -32768 and 32767, inclusive<a id="footnote-4-ref" href="#footnote-4">[4]</a>. So now our underdetermined source separation toy problem has moved away from the realm of the impossible and into the land of the prohibitively difficult. Keep in mind that once we figure out the right pair for t=0, we still have to find the correct pair for 44,099 other mixture values in order to separate a single second of our mixture! Additionally, we’re only trying to separate two sources; most music has more than two sources, and in that case, the problem becomes “what 3/4/5/… numbers add up to X?” Clearly this is an impractical way to go about this problem.

What do researchers do when they’ve hit an impasse? Make [assumptions](https://en.wikipedia.org/wiki/Spherical_cow )! There is clearly no general solution to this underdetermined problem, so researchers develop algorithms that make assumptions about the mixtures they are separating. These assumptions are woven into the fabric of the algorithms; they power the separation at a very fundamental level. The assumptions are based on things like timbre (the inherent characteristic of a sound), spatialization (where a sound is “located” in a mixture), following a melody, knowing that sources enter or exit, musical structure, etc. Making assumptions also means that those assumptions can and will fail, but understanding the assumptions means understanding the strengths and limitations of these algorithms.

As this blog progresses and we explore many of these algorithms, we will also learn about the assumptions these algorithms make. We will learn which circumstances warrant which algorithms as well as learning the mechanics of the algorithm.

## Onward!

So now we’ve laid some groundwork for what source separation is and why it’s important. In the following installments of this blog we will describe the many ways that researchers have tackled this problem. As you can probably guess, the methods researchers have employed are as varied as the myriad definitions of a source that we've outlined above. Join us as we explore many of these methods!


<br />
<br />

<p id="footnote-1">
  [1] Web player by <a href="http://www.binarymind.org">Bastien Liutkus</a> from <a href="https://github.com/binarymind/multitrackHTMLPlayer">here</a>. <a href="#footnote-1-ref">&#8617;</a>
</p>

<p id="footnote-2">
	[2] The irony of calling an auditory process “blind” is not lost on this researcher. <a href="#footnote-2-ref">&#8617;</a>
</p>

<p id="footnote-3">
  [3] Bregman, Albert S. <i>Auditory scene analysis: The perceptual organization of sound</i>. MIT press, 1994. <a href="#footnote-3-ref">&#8617;</a>
</p>

<p id="footnote-4">
	[4] We leave it to the combinatorics experts to figure out how many possible integer pair solutions there are within a 16-bit space. <a href="#footnote-4-ref">&#8617;</a>
</p>
