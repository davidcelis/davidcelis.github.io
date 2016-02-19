---
layout: page
title: "goodbre.ws"
description: "A recommendation site for beer."
permalink: /projects/goodbrews/
categories: [ruby, goodbrews, recommendable]
---

goodbre.ws was a project borne out of my last year in the [Rollins College][rollins] Computer Science department. As a senior in the honors program, I was expected to produce an exceptional capstone project, present it, and defend it to the faculty. I chose to build my own [collaborative filtering system][collaborative filtering] based on the [Jaccardian Similarity Coefficient][jaccardian index]. To provide a practical application for this system, I created goodbre.ws, a recommendation website for beer written in Ruby on Rails.

The idea behind goodbre.ws was simple: users would indicate beers they like or dislike via an intuitive binary rating system (literally just "Like" or "Dislike"), and the system would find other like-minded users. By comparing across all of these users, the system could provide accurate recommendations and steer users towards beers they would like.

An unexpected spike in traffic and users came when goodbre.ws was featured on [lifehacker][lifehacker] and [The Huffington Post][huffington post]. This interest ultimately took down my servers. I was able to recover, but from that point on, goodbre.ws was unfortunately plagued by very slow generation of new recommendations. By the time I solved this issue, goodbre.ws had been unusable for long enough that I decided to rewrite the website.

goodbre.ws remained my [White Whale][white whale] for several years until I admitted to myself that I would never truly re-release it. It had become such a passion of mine that I knew I wouldn't accept anything less than perfection, which is of course an impossibility. Thankfully, [Next Glass][next glass] appeared soon after and the premise is way better anyway.

[collaborative filtering]: /posts/collaborative-filtering-with-likes-and-dislikes/
[huffington post]: http://www.huffingtonpost.com/2012/10/01/goodbrews-beer-recommendations-exploration-website_n_1930567.html
[jaccardian index]: http://en.wikipedia.org/wiki/Jaccard_index
[lifehacker]: http://lifehacker.com/5947790/goodbrews-tracks-the-beer-you-like-suggests-brews-youd-love
[next glass]: http://nextglass.co
[rollins]: http://rollins.edu/
[white whale]: http://www.urbandictionary.com/define.php?term=white+whale&defid=5468452
