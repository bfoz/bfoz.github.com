---
wordpress_id: 25
layout: post
title: "Javascript: Single-shot event handlers"
date: Tue Aug 08 13:15:59 -0700 2006
wordpress_url: http://bfoz.net/blog/2006/08/08/25/
---
I was looking through the code for [Session Manager](https://addons.mozilla.org/firefox/2324/) and noticed a number of event handlers that unregister themselves as soon as they're called. Actually, a proxy function gets called first and it does the the unregistering before calling the intended event handler. At first that looked really odd to me. After a bit of thinking it made perfect sense, but it still felt wrong.

Typically when registering an object method as an event handler (all handlers in a Mozilla extension should be methods to prevent namespace collisions) you pass addEventListener() an anonymous function that wraps the actual method call. However, a single-shot handler needs to call `removeEventListener()`, which wants a reference to the handler to be removed, which you don't have because the handler-wrapper is anonymous. [Session Manager](https://addons.mozilla.org/firefox/2324/) gets around this problem by defining non-anonymous proxy functions for every single-shot event handler. It's those proxy functions that handle the unregistration and then call the intended event handler. That works just fine. But it's cumbersome and bloated, and I really don't feel like writing two functions to handle each event type.

In most cases a function is passed as the 2nd argument to `addEventListener()`, but an object can be passed instead as long as it supports the [EventListener](http://www.w3.org/TR/DOM-Level-2-Events/events.html#Events-EventListener) interface. Passing an object is convenient because inside the handler function `this` will point to the registered object instead of the event target, which of course allows extra info to be stored in the object and made available to the handler. Consequently, an anonymous object can be defined that maps to the intended handler through a proxy that first unregisters the object. Everything the proxy needs to know is either provided in the anonymous object or in the standard [Event](http://www.w3.org/TR/DOM-Level-2-Events/events.html#Events-Event) object. But there's no reason to write a boiler plate object for every call to `addEventListener()`, so I created `addSSEventListener()` that handles the drudgery.

    function addSSEventListener(target, type, func, obj, useCapture)
    {
        function proxy(e)
        {
            e.target.removeEventListener(e.type, this, false);
            return this.f.call(this.o, e);
        }
        return target.addEventListener(type, {o:obj, f:func, handleEvent:proxy}, useCapture);
    }
<dl>
 <dt>target</dt>	<dd>The DOM element that generates the event</dd>
 <dt>type</dt>		<dd>The event type ("load", "blur", etc)</dd>
 <dt>func</dt>		<dd>The event handler (ie. <code>myobj.handler()</code> )</dd>
 <dt>obj</dt>		<dd>The object that contains the handler (ie. myobj)</dd>
 <dt>useCapture</dt>	<dd>Same as <code>addEventlistener()</code></dd>
</dl>
