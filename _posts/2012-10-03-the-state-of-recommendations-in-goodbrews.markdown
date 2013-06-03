---
layout: post
title: "The State of Recommendations in goodbre.ws"
date: 2012-10-03 11:27
categories: [programming, goodbrews, recommendable]
---

Hello, friends and new faces. I want to take a moment to address a question
that many of you have had on your mind since you came to
[goodbre.ws][goodbre.ws].

> Where are my recommendations?
>
> _Everybody, ever_

There is (obviously) a problem with how long it is taking for your
recommendations to be delivered, and I want to respond to that. I never
expected goodbre.ws to be gaining popularity at its current speed. It's all
beyond what I had hoped for and is very exciting. You likely found goodbre.ws
on [Lifehacker][lifehacker] or [The Huffington Post][huffington]. I was
surprised to discover goodbre.ws was suddenly getting press, and the spikes of
traffic have made for a stressful (albeit exciting) few days.

Mainly, however, the heavy increase of traffic has shown me that my current
way of serving recommendations isn't particularly scalable. As people join the
site, recommendations become exponentially slower. But don't fret! I'm
currently working on what will end up being a complete overhaul of
[Recommendable][recommendable], the library that I wrote to power goodbre.ws.
I'm hoping to have a solution out the door in the next week or two, and I
think that it will alleviate this problem.

Until then, some newer users may not see any recommendations at all. I
apologize for this. But I also appreciate patience during this period as I
figure out what to do with goodbre.ws. Please keep in mind that I'm just one
guy doing this on my free time and trying to provide what I think is a really
simple and really cool service. Meanwhile, I think that goodbre.ws is still a
great way to keep track of the beers you like and don't like. I've been
getting great feedback and suggestions from so many people, so I hope that
you'll continue to use the site during the next week or two with the
understanding that recommendations _are_ coming soon.

Thank you for being patient.

_UPDATE (14:52 PST, 16 October 2012): Several days ago, I updated
[recommendable][recommendable] to be much speedier with Recommendations. If
you're still not seeing recommendations, please rate one more beer. This will
place you in a (now very small) queue, out of which 25 jobs can be processed
in parallel. Once your turn comes, it takes about 10 minutes on average to get
recommendations. Once you get them, you'll always have them. Any further
ratings just improve accuracy. So just be patient for that initial process._

[goodbre.ws]: https://goodbre.ws/
[lifehacker]: http://lifehacker.com/5947790/goodbrews-tracks-the-beer-you-like-suggests-brews-youd-love
[huffington]: http://www.huffingtonpost.com/2012/10/01/goodbrews-beer-recommendations-exploration-website_n_1930567.html?utm_hp_ref=technology
[recommendable]: https://github.com/davidcelis/recommendable
