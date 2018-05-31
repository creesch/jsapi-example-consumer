# Reddit jsAPI example consumer extension.

This repository provides example code for the implementation of jsAPI consumers. When loaded in the browser it will do the following things:

- Log all jsAPI events to the browser console.
- Display the location of jsAPI containers on the page in the following format `⚠ type`. 
- Display the payload data as title (tooltip) in the JSAPI container.

The jsAPI specific code can be found here:

`/src/inject/inject.js`


# What is this jsAPI?

jsAPI is shorthand for "*Javascript API for Third Party Tools*".

##  Introduction 

The redesign of reddit means that third party extensions and userscripts will no longer work and need to be rebuilt as well. This rebuilding and integration in the redesigned reddit cannot be done with previously used methods because the following reasons: 

1. **CSS classnames are no longer static.** They are instead computed by the style engine, not manually assigned by devs. Class names may change when code affecting those components is deployed, and new code is deployed many times a day. 
2. **Content is lazy rendered**. Meaning that only what is visible on the page gets actually rendered and content outside the viewport may not be rendered until the user scrolls it into view.
3. **DOM nodes are frequently updated due to React reusing DOM nodes**. Anything a third party extension adds might be scrubbed clean a second later.  
4. **Navigation is partially done through `pushState()` methods**. Third party extension can no longer rely on getting the page context on page load. 


## Solution 

To help out third party developers the jsAPI has been developed as a way to communicate what is present on page (posts, users, comments, etc) and changes in what is present (new posts loaded, url changes, etc). Additionally it provides specific dom nodes as containers for third party developers to write into. 

Third part extensions that register as consumer will first receive a backlog of currently relevant events. After having received the backlog interaction with reddit will result in events being fired for relevant things allowing the extensions to act on those.

## Considerations 

Aside from the jsAPI still being in development there is one rather important thing to consider: 

- Third party containers are *shared spaces* but can have consumer-specific sections. 

This means that everyone gets the same sandbox to do their thing in. This might change in the future but until it does it is important to **only manipulate elements inside the container labeled for your consumer**. Be careful to **not** overwrite other elements in the container. 

If your extension includes a name in the meta `content` attribute as shown in the code example then each container will include a `<span data-name="the-name-value" />`. These containers will be alphabetically sorted by name.




