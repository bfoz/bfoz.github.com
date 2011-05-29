---
wordpress_id: 14
layout: post
title: Hungarian Tagging
date: Sat May 14 13:07:07 -0700 2005
wordpress_url: http://bfoz.net/blog/2005/05/14/14/
---
I finally got around to reading [Joel Spolsky's](http://www.joelonsoftware.com/) post about [Making Wrong Code Look Wrong](http://www.joelonsoftware.com/articles/LeakyAbstractions.html) where he proposes an interpretation of Hungarian notation. It occurred to me that the whole Hungarian notation concept  is just a form of tagging (Hungarian Tagging?), an ugly form, but tagging still. As Joel points out, this tagging of variables in source code is a very useful concept. But it is truly ugly, and the Systems Hungarian variant has been largely rejected.

So how can Hungarian Tagging be made better? Syntax highlighting comes to mind. What if there was a way to actually apply tags to variable names in a way that an editor can recognize and then apply a style to? If that were the case the editor could make something far more pleasing to the eye than variable name prefixes. Taken a step further, I can imagine a compiler that can recognize the tags and generate warnings when incompatible tags are used in a Bad Way(TM). Of course, this implies some sort of mechanism for setting rules about tags, but that's not too hard. Even if adding compiler support is too onerous, just having a compatible editor would provide all of the benefits of the notation without the ugliness.

What mechanism can provide this? Source code is generally stored in flat text files and adding any kind of markup or embedding would only make things uglier. After reading the [Ars](http://arstechnica.com/) [review of Tiger](http://arstechnica.com/reviews/os/macosx-10.4.ars) not too long ago I've been pondering the possibilities of extended attributes. Perhaps an editor could somehow store tagging info in the text file's extended attributes. Anything that supports the tagging attributes would be able to read them and use the extra info for processing the code (displaying, compiling, etc...) and anything that doesn't support tagging can just ignore the attributes. Cross platform support might be an issue, however both OS X and FreeBSD support attributes with a remarkably similar API (no surprise there). I imagine Linux has a version too, but I don't know for sure. I think I saw somewhere that Windows also has some sort of attribute capability too, but I know even less about that. So portability shouldn't be an unsurmountable issue.

Now that I think about it...extended attributes should allow for Google-style tagging on files too. Something to think about...
