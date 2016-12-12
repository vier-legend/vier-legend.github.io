---
layout: article
title: "Game Boy Aesthetics and Limitations"
modified:
categories: devblog
excerpt: "Hello and welcome to our first blog post for the Vier Legend devblog!"
tags: []
image:
  feature:
  teaser: devblog/1/title-min.png
  thumb: devblog/1/gray-shades.png
date: 2016-12-01T22:08:14-03:00
---

<p class="center">
<img src="/images/devblog/1/title-min.png" alt="title">
</p>

>A 1000 year long war!

Hello and welcome to our first blog post for the Vier Legend devblog! This will be the introduction to a series of posts on the inner workings of our little Game Boy inspired game, Vier Legend, which is to be out not far in the future.

***

In this first iteration, we’ll be talking about how we are dealing with the Game Boy limitations in all aspects of the game.

But first, why did we choose the Game Boy aesthetic? The answer is that this game was supposed to be a much simpler jam game for the GBJAM 5, but the resulting game we had when the jam ended was not really satisfactory to us, which was actually quite frustrating, but nothing else could be done at the time, aside from releasing a terrible game with nice art only… So we decided to continue development after the end of the jam and fine tune it for a proper release, since we had a lot of the characters, art and designs laid down. That said, we are still fans of the Game Boy and do like the idea of working under its limitations!

Now that we have this game going in a more solid road, we are intending to bring the truest experience possible in terms of Game Boy limitations and feel, even though it’s not really running on a Game Boy.

***

## Graphics

So let’s talk a bit about the graphical limitations. The most basic thing to simulate the Game Boy graphics is limiting the colors to 4 shades of gray (or green). We ended up using gray shades, since it’s less straining on eyes, at least for us.

<p class="center">
<img src="/images/devblog/1/green-shades.png" alt="green-shades">
<img src="/images/devblog/1/gray-shades.png" alt="gray-shades">
</p>

>We think Game Boy Pocket style gray shades look much better!

Limiting the screen to only show 4 colors actually makes things a bit more challenging to work with precision. Every fade in and out must only go through these 4 colors. As we didn’t have much time to do that in the jam, we ended up using a workaround with tiles to make transition to other scenes look nice without having to precisely switch the color of everything on screen.

<p class="center">
<img src="https://cdn-images-1.medium.com/max/720/1*5WXkCi99DNuMHUybLFV3yA.gif" alt="parallax-animation">
</p>

>Also note that in this animation we are considering that the Game Boy has to clean up all the fade in tiles before showing anything other than the background, which is a layer natively supported by the Game Boy, aside from the sprites. The background can use all 4 shades of gray, so we are fine with that one!

However, now that we have more time, we are using a color limiting shader, which does the job nicely.

<p class="center">
<img src="https://cdn-images-1.medium.com/max/720/1*Gsigia600SzLrNe9YqinYQ.gif" alt="fade-shaders">
</p>

>Looks quite legit! We still use the tiled fade sometimes, though, because it also looks nice.

Now, one aspect to make things look like they pertain to a Game Boy game is working with 8x8 tiles. The tricky part of working with tiles like these comes mostly when a sprite has transparency. In cases like this, the sprite must have no more than 3 colors (the 4th color is transparent), because each tile can only work with 4 colors at most. So when creating our assets, we must pay close attention to how many colors there are in each of the sprite’s tiles at a given moment. Most of the times, we can easily keep an eye out for this by activating a screen grid in our engine and making careful observation of everything that goes on when animating our scenes.

<p class="center">
<img src="https://cdn-images-1.medium.com/max/720/1*Zfa9tq5pwbcQizcAKRuKTw.gif" alt="8x8-grid">
</p>

>There’s a nice 8x8 grid guiding us through all this.

Another graphical limitation we had to consider was the amount of sprites that can be drawn per horizontal line. The Game Boy can only process 10 8x8 or 8x16 sprites per horizontal line before that line starts to flicker. Flickering is actually a common thing in a few commercial Game Boy games, such as Contra. In our case, the game may display more sprites per line than the Game Boy can handle, though we are trying our best not to go much beyond this limit, because then we would have to simulate flickering intentionally, and man… That would not only be hard to do, but it would bring back something that’s ugly and frustrating, and that’s not very wise. This is the only thing we have deliberately chosen not to follow strictly, so that players can enjoy how this game was meant to be played.

<p class="center">
<img src="/images/devblog/1/gameplay.png" alt="gameplay">
</p>

>So many bullets… How much can the Game Boy take?

Lastly, we are making the game render in the same exact Game Boy resolution of 160x144. This is actually pretty good in making things look authentic, because the pixels will snap right to where they should be, whenever there’s movement. The larger screen modes are just zooming in on the original resolution.

***

## Audio

Now, into the audio department, the limitation of the Game Boy is quite strict: it has only 4 channels to work with (2 square waves, 1 wavetable and 1 white noise), so all of the music and sounds must be made within these confinements. On the musical side, we are going to great lengths to make it completely authentic. It’s so authentic we will be making the soundtrack available in GBS (Game Boy Sound System) format as a special treat to the chip enthusiasts like us out there, along with the common album release!

<iframe width="100%" height="166" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/293436331&amp;color=ff5500"></iframe>

>Do you hear the sound of nostalgia? This is the actual Game Boy chip being emulated!

Now, doing accurate ingame sound effects is the trickiest part of making the sound authentic. As the Game Boy has only these 4 channels, one or more of the channels must stop playing the music when the SFX plays, as would happen in Pokémon when you were low on HP and that annoying beeping would take up one square wave channel of the music. We’ve thought up a system to deal with this: basically we’ll play layered music stream files at the same time that make up the music when together, and each SFX would come with an instruction to instantly lower the volume of pre determined channels. So if we make a SFX that uses the square wave channel and the noise channel, we instantly lower the volume of the music stream representing these channels and then bring the volume back as soon as the SFX ends. Of course this won’t be exactly the same as what would happen on real tracked sound environment such as the Game Boy, but it’s close enough. This system is pretty much done by now and can be checked out in one of our recent tweets over here:

<p class="center">

<blockquote class="twitter-tweet" data-lang="pt"><p lang="en" dir="ltr">We&#39;re working on a sound system to imitate the GB behavior! Playing SFX will mute music channels momentarily. <a href="https://twitter.com/hashtag/gamedev?src=hash">#gamedev</a> <a href="https://twitter.com/hashtag/indiedev?src=hash">#indiedev</a> <a href="https://twitter.com/hashtag/chiptune?src=hash">#chiptune</a> <a href="https://t.co/vManQs4Elt">pic.twitter.com/vManQs4Elt</a></p>&mdash; Vier Legend (@vierlegend) <a href="https://twitter.com/vierlegend/status/804147714661482496">1 de dezembro de 2016</a></blockquote>
</p>

<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<p class="center">
<img src="/images/devblog/1/sound-room.png" alt="sound-room">
</p>

>Of course, if you want the music intact while playing, you can toggle this effect on or off. Ah, the marvels of modern technology!

That’s about it for this post. We hope you like our little project, and if you want to see more, check us out on Twitter [@vierlegend](http://twitter.com/vierlegend), since we bring news firsthand over there!

See you next time!