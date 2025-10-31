---
layout: post
title:  "Promises.all() groups promises"
date:   2025-10-31 9:00:00 -0500
categories: jekyll update
---

I recently learned that the javascript function Promise.all() takes a list of promises and returns a single promise that resolves to the list of all fulfill promises. This let me do something like this

```javascript
Promise.all([fetch(url), fetch(url2)]).then(values) => 
{
  // do stuff with the json data
}
```
