## Browser extensions 101 : Helpful resources

### Quick intro

About two weeks ago, I started building a browser extension for my side project :  [Tweekr](https://twitter.com/tweekr_app). After a lot of searching, I've curated a list of resources that have helped me the most. 

This post is a list all those resources. I'll keep updating this list as I keep landing on more.

### Resources

- [Mozilla's browser extension docs](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions)

I found Mozilla's documentation to be more self contained and helpful than Chrome's. This is the first place I go to, if I need to learn something new. Replace the word `browser` with `chrome` and you'll end up with a chrome extension. Cool isn't it?

- [Chrome extension docs](https://developer.chrome.com/docs/extensions/mv2/getstarted/)

I've included Chrome's docs as well, because why not :)

- [Example extensions on GitHub](https://github.com/GoogleChrome/chrome-extensions-samples)

This is one of the most helpful resources I've come across. Whenever I wanted to understand how to implement something, I could go and see how other developers have done it.

- [Coding Train's playlist on YouTube](https://www.youtube.com/playlist?list=PLRqwX-V7Uu6bL9VOMT65ahNEri9uqLWfS)

After going through this playlist, I could easily understand the difference between background scripts and content scripts and could create a few simple chrome extensions.

- [Rusty zone's YouTube Channel](https://www.youtube.com/channel/UC-h4Q0_5zTX66AxJucRmxRQ/playlists)

This channel is a treasure house of content related to creating chrome extensions. I learnt about integrating Firebase into a Chrome extension, authenticating users using Firebase's authentication service and much more!!

- [Codedamn's playlist](https://www.youtube.com/playlist?list=PLYxzS__5yYQlWil-vQ-y7NR902ovyq1Xi)

This one is very underrated in my opinion. I didn't go through the entire playlist but watched a few videos here and there. I learnt about working with context menus and connecting to a database from an extension. 

- [CSS Trick's article by Sarah Drasner](https://css-tricks.com/how-to-build-a-chrome-extension/)

This article is a good primer on creating chrome extensions. 

CSS tricks + Sarah Drasner = You can never go wrong :)

- [HTML tree generator browser extension](https://chrome.google.com/webstore/detail/html-tree-generator/dlbbmhhaadfnbbdnjalilhdakfmiffeg)

This extension shows all the DOM elements on a page in the form of a tree and I think that's extremely helpful while working with DOM injection using content scripts.

- [Get the source code of any extension](https://www.youtube.com/watch?v=pzYeArH-Vs0)

This is a gem of a video that shows how to get the source code of any chrome extension. Very helpful to see how developers have coded a feature

## Indirect resources

These are not directly related to creating extensions. I referred to them but had to debug and change quite a lot of things to fit my needs. Nevertheless they were very useful

- [Maksim's Firebase cookie authentication video](https://www.youtube.com/watch?v=kX8by4eCyG4)

This one is very close to how I've implemented shared authentication between Tweekr's browser extension and web app.

- [Maksim's Firebase authentication in a React application video](https://www.youtube.com/watch?v=unr4s3jd9qA)

- [User authentication using Firebase's custom ID tokens](https://www.youtube.com/watch?v=WtYzHTXHBp0)

## Conclusion

This one is the first article of a series that I'm starting, called "Browser extensions 101", as a part of which I'll be writing posts here and will make a video on YouTube to accompany it. Hope this was useful!

I'll be back with another post next week :)