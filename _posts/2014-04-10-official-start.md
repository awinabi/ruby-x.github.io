---
layout: news
author: Torsten
---

Well, it has been a good holiday, two months in Indonesia, Bali and diving Komodo. It brought clarity, and so 
i have to start a daunting task.

When i learned programming at University, they were still teaching Pascal. So when I got to choose c++ in my first
bigger project that was a real step up. But even i wrestled templates, it was Smalltalk that took my heart 
immediately when i read about it. And I read quite a bit, including the Blue Book about the implementation of it.  

The next disctinct step up was Java, in 1996, and then ruby in 2001. Until i mostly stopped coding in 2004 when i 
moved to the country side and started our <a href="http://villataika.fi/en/index.html"> B&amp;B </a>
But then we needed web-pages, and before long a pos for our shop, so i was back on the keyboard. And since it was
a thing i had been wanting to do, I wrote a database.

Purple was my current idea of an ideal data-store. Save by reachability, automatic loading by traversal
and schema-free any ruby object saving. In memory, based on Judy, it did about 2000 transaction per second. 
Alas, it didn't have any searching.

So i bit the bullet and implemented an sql interface to it. After a failed attempt with rails 2 and after 2 major rewrites
i managed to integrate what by then was called warp into Arel (rails3). 
But while raw throughput was still about the same, when
it had to go through Arel it crawled to 50 transactions per second, about the same as sqlite. 

This was maybe 2011, and there was not doubt anymore. Not the database, but ruby itself was the speed hog. I aborted.

In 2013 I bought a Raspberry Pi and off course I wanted to use it with ruby. Alas... Slow pi + slow ruby = nischt gut. 
I gave up.

So then the clarity came with the solution, build your own ruby. I started designing a bit on the beach already.
Still, daunting. But maybe just possible....

