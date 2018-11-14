# Chrome dev summit - Day 2

## Keynote

by Malte Ubl / Nicole Sullivan

Web stack:
 - Web Components (Carousel)
 - Frameworks (React)
 - Build in modules
 - Web primitives (Button)

Web development is hard: Developer experience vs User experience - they are not always in line

### Updates from the frameworks
React.js / Polymer / Vue / Svelte / AMP / Ember all made big improvements on the performance side

Example: batched re-hydration (Ember / React Suspense)

### 3 announcements:

1) Include frameworks in the Chrome Intent to implement process
2) $200,000 of funding for improving performance best practices in frameworks. http://bit.ly/framework-perf
3) Increase collaboration with frameworks from the chrome team

### New features

- Display locking - lock the DOM - don't update the DOM inadvertently
- Page transitions on the web with Portals
- Report-only / enforce mode for feature policies. Examples: sync XHR / unoptimized images / oversized images / unsized media
- Instant loading - load the website before the user clicks.
Web packaging - privacy preserving instant loading for the web
Document author signs the content with their certificate. Anyone can deliver it.
https://twitter.com/meyerweb/status/1026848459515723777 also securing web sites
- Scheduling API - schedulers in frameworks have serious problems / Grand Central Dispatch inpired APIs in the browser. Gets really interesting if you have tiny tasks!
- Animation worklet - web animations that support a custom time source
- Virtual Scroller - Searchable and Visible DOM - UITableView is coming to the web

## Feature Policy & the Well-Lit Path for Web Development
Jason Chase, Chrome Engineer

Making web development easier with guardrails - rails stop you from leaving the path
- Answer 1: AMP :s
- Answer 2: Feature policy - restrict what features the browser can access

Examples:
 - unoptimized-images
 - unsized-media

Support best practices: detection vs enforcement =>
- You can enable them on your staging server
- Different routes can have different policies (works well with the new Pinterest routing)
- Report-only mode
- Support in Chrome / Firefox (partial) / Safari (intent to implement)
- https://github.com/WICG/feature-policy/blob/master/reporting.md
Chrome extension: https://chrome.google.com/webstore/detail/feature-policy-tester-dev/pchamnkhkeokbpahnocjaeednpbpacop

## virtual-scroller: Let there be less (DOM)
@graynorton - manager on the polymer team

https://github.com/valdrinkoshi/virtual-scroller

Examples
- Android: RecyclerView / iOS:UITableView
- React virtualized / material virtural scrolling / ...

Questions
 - How do we make links work better in a virtualized world
 - Find in page
 - Indexability (search crawlers)
  - How can we increase virtualization usage?

Problems we're trying to solve
 - Rendering
 - Loading (how quickly we make it interactive / usable)
 - Scrolling

How
- Learn from prior art
- Make it simple
- Make it work (accessibility - tabbing from item to item)
- Make it fast

Embrace layering

1) https://github.com/domenic/infinite-list-study-group
2) https://github.com/valdrinkoshi/virtual-scroller

Making nodes invisible instead of doing `display:none` - able to get to a specific element
Using the `invisible` attribute

Next:
 - More invisible DOM integration
 - Additional primitives?
 - Framework collaborations
 - Advanced use cases
 - Performance optimizations
 - Down-the-stack explorations
 - Standards - pushing that forward


## A Quest to Guarantee Responsiveness: Scheduling On and Off the Main Thread
Shubhie Panicker, Chrome Engineer
Jason Miller, Developer Programs Engineer

Deeper dive into web workers

- It's usually pretty impractical to use web workers / doing prioritization
- We need a scheduler: execute work at the best time
- Input handlers / rAFs

New API's
 - getExpectedFrameTime()
 - isInputPending()
 - fetch with `priority: 'idle'`

Global TaskQueue API

- Downside of the main / worker thread API with postMessage (there is always a hop which incures a latency)
- thread hops / manual thread management / complete API reflection is hard / impossible
- Other way: MessageChannel / BroadcastChannel
- Next (future way): Transferable Streams

Proxying
- Using something like `postTask` API - Grand Central Dispatch / AsyncTaks

New proposal: Task Worklets minimizing thread hops with a sticky thread pool.
https://github.com/developit/task-worklet

Base coast of a worker
 - Startup: 10ms
 - Termination: 5ms
 - Thread hop - 1-15ms

Workers can make your input delay slower - but make the rendering faster

## Architecting Web Apps - Lights, Camera, Action!
Paul Lewis & Surma

- The Actor Model - is a really good fit for the web
- Example of different actors: State / User interface / Broadcaster / Storage
- Yields to browser

Code code code - the Actor Model

- xstate - declare a state machine
https://www.npmjs.com/package/xstate


## From Low Friction to Zero Friction with Web Packaging and Portals
Kinuko Yasuda - Rudy Galfi (lead product manger for AMP)

### Web packaging
- Privacy preserving navigation for all (not just AMP)
- Way to fix: prefetch / prefetch + cache
- Browser needs a way to have a proof of origin
- Multiple - signed exchanges + bundled exchanges

http://bit.ly/try-sxg
Signed HTTP Exchanges
http://bit.ly/webpackaging

Cloudflare AMP Package manager
g.co/webpackagepreview 

Portals: enable seamless page transitions
 - Create portal as an html element


## State of Houdini
Surma, Developer Advocate

Different stages of rendering in the browser:
Style => Layout => Paint => Composite

4 major APIs
 - Layout API
 - Paint API
 - Parser API
 - Properties & Values API

Worklets vs Workers (very different)
 - It's all about the event loop 

 Worker - has a completely different event loop
 Worklet - also isolated JavaScript scope - they already attach the original (existing) event loop

 Worklet - load a JavaScript file into a worklet

```
CSS.paintWorklet.addModule('my-paint-worklet.js')
```
`background-image: pain(my-paint)`


- Auto-sized (compared to the canvas API)
- Off the main thread
- No DOM overhead! Only using 1 DOM element

```
if ("paintWorklet" in CSS) {}
```

3PigStability (TM)
 - bit.ly/houdini-paint-particle

Animation worklet API
 - CSS transitions
 - CSS keyframe animations
 - Web animations API (badly supported)

spring timing function - only in Safari

custom display values - `display: layout:
https://github.com/GoogleChromeLabs/houdini-samples


## Building Engaging Immersive Experiences
Chris Wilson - John Pallett

- Launch a version of chrome into a 3d environment
- Hop between VR and 2 web
- Chrome VRBrowser - used by Daydream

WebXR Device API - replaces the deprecated WebVR API
https://github.com/immersive-web 

WebXR Polyfill - JS only implementation - for any mobile browser (using orientation events)
https://github.com/mozilla/webxr-polyfill 

View in your room

<model-viewer> web component 
 - 3d models without 3d programming
 - work accross all browsers
 - automatically improve
gltf components

## Using WebAssembly and Threads
Alex Danilo, Developer Advocate
Thomas Nattestad, Product Manager

## The Virtue of Laziness: Leveraging Incrementality for Faster Web UI
Justin Fagnani, Chrome Engineer

## Chrome OS: Ready for Web Development
Dan Dascalescu - Stephen Barbers

TODO
 - Look into feature policies (per route) - synchronous XHR (images too big for the screen): https://feature-policy-demos.appspot.com/
 - Investigate <virtual-scroller> https://github.com/valdrinkoshi/virtual-scroller

