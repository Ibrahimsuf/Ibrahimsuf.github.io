---
layout: post
title:  "Mobile Development Environment are tricky"
date:   2025-10-15 9:00:00 -0500
categories: jekyll update
---

This week I have been trying to develop a very simple mobile app that shows me how much money I spent this week and how much I have left until I hit my spending limit. I thought this would be easy it's just one api call to get my transactions and then filtering and adding up the data in the response json.

I was wrong. Very wrong. 

![JFK because they were easy meme]({{ 'assets/images/easy.jpg' | relative_url }})

First it's not just one api call. Most banks (including mine) don't have an api that can be used for individual developers so you need to use a third party api to manage authorization for bank account information. 

This by itself wasn't that hard. Plaid is decently easy to get up an running by following the getting started section of the documentation, and it lets you login to your bank and receive all transaction information going back way longer than I needed. 

However the problem is that plaid for mobile uses react native so I had to build a react native app. 
This I was somewhat worried about because I have always been a little scared of mobile development since I tried using swift for a project that was way out of my depth a couple of years ago. This wasn't too bad though I got the getting started react app up very quickly. Part of the reason for this is that the react-native getting started demo uses expo which based on my understanding handles a lot of the ios specific features and prevents you from spending a lot of time dealing with dependencies.

The problem was that some of the plaid-link components (the part that let the user log in) are written in swift and expo doesn't allow installing packages that aren't written in react-native. This forced me to use react-native without a framework which is much harder and has less stack overflow answers. 

For a quick survey of some problems I had:

* I had to update both my mac and my iphone so the xcode developer tools would cooperate, (expo worked without updating on the first try with just a qr code scan)
* native-wind (react-native's verison of tailwind) requires the newest version of node and react-native which I wasn't using because I didn't tell nvm to use the latest version of node. This required essentially restarting the project using the newest version of node and then getting tailwind to work
* forgetting to `pod install`: Some npm packages work with just an `npm install` but others requiring running `pod install` in the ios directory. I'm still figuring out why this is. But it caused a lot of headaches to figure out that this was the problem. 

Long story short these were some of the many problems I had while trying to develop a mobile app. Here's somethings that I am trying to make it easier

* don't use plaid-link. The plaid token lasts for 90 days so I can just log in once and then store the token on my phone and manually update it in a couple of months. This way I don't have to worry about plaid link
* use basecamps framework for mobile app development this might be easier given my rails background