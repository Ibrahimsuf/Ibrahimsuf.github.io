---
layout: post
title:  "Respond to Format Order Matters in Ruby?"
date:   2025-10-10 9:00:00 -0500
categories: jekyll update
---

At my job I am working on converting our charts which currently use chart director into highcharts because the newest version of ruby does not support chart director. As part of this I needed to decide if we wanted to render the charts as pngs using the highcharts server or if I wanted to have the server render json and then use the highcharts javascript library to render the chart.
Long story short we decided to do both, render the charts as json for most purposes but still render pngs for creating pdfs. Rails makes this easy to do with a `respond_to` block

```ruby
respond_to do |format|
    format.html  {render "shared/html_with_png", locals{ chart: chart }}
    format.json  { render :json => chart_json }
end
```

So I set up the controller to send both the json or the png, however when I used javascript to get the json and render the chart with client side Highcharts, I noticed it only worked when the format.json block was before the format.html block. Otherwise I was getting a json parsing error.
This lead me to realize that the `response.json()` method doesn't have anything to do with if the webpage returns json or html. The method just tries to convert what ever the request returned into a javascript object assuming the request returned json.
It turns out my javascript code to request the json from the server had a bug in it. I was doing 

```javascript
  const response = await fetch(url);
  if (!response.ok) {
    throw new Error(`Response status: ${response.status}`);
  }
  const result = await response.json();
```

However this code doesn't actually tell our server to respond with json so the last line fails. According to the [Rails Docs,]("https://apidock.com/rails/ActionController/MimeResponds/InstanceMethods/respond_to") `Rails determines the desired response format from the HTTP Accept header submitted by the client.` looking at the network section of developer tools (right click, inspect, network) I could see the request headers looked something like this 

```HTTP
GET /path/respond_to HTTP/1.1
Host: server_host
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:143.0) Gecko/20100101 Firefox/143.0
Accept: */* # 👈
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br, zstd
Referer: https://www.google.com/
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Priority: u=0, i
Pragma: no-cache
Cache-Control: no-cache
```

Where the accept line is the key line. If you don't tell the fetch block what to put in the accept header it just defaults to accept, everything which would have been fine accept the server can respond with either html or json so rails just rendered whichever block was placed first. This was fixed by adding the `headers: {"Content-Type": "application/json",}` to the fetch call. I also discovered a nice way to test this that doesn't require rerunning the javascript.
In Firefox right clicking and opening the network tab shows all of the HTTP requests that your browser has made. If you right click on one of those you are given the edit and resend option which let's you change the headers. 

## TLDR

1. by default `fetch` in javascript accepts any type of format
2. when you don't specify in the `Accept:` request header, Rails will return whichever format is first in the `respond_to` block.
3. You can resend requests after changing the header by right clicking on the request in the network tab and clicking `edit and resend`
