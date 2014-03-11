---
layout: post
title: "No Need to Support Experiments While You Sleep"
date: 2014-03-11 10:59
categories: [programming]
external: http://blog.newrelic.com/2014/03/11/experimental-features-business-hours/
---

At New Relic, we got tired of experimental features waking us up in the middle of the night. Going straight from testing in a staging environment to a production environment is a bigger step than your feature might be ready for, especially if you’d rather not be getting paged in the middle of the night. To that end, we created a feature flag to make sure we were only testing in production when we could reasonably support our experimental features. Here’s how we got there, and why it will save your precious Z’s.
