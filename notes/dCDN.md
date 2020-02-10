# Decentralized Content Delivery Network

A decentralized content delivery network.

## Example 

Suppose we have a super simple `app.js` running under https://example.com:
```javascript
import { greeting } from './dCDN/248d6a61d20638b8e5c026930c3e6039a33ce459.js' 
greeting() // hello world!
```
To serve `248d6a61d20638b8e5c026930c3e6039a33ce459.js` we're installing the dCDN's standard service worker file under `./dCDN/service-worker.js`.
It intercepts any request to `https://example.com/dCDN/<filename>` and proxies it into a decentralized network of CDNs, serving the files redundantly.
The service worker keeps a list of root URLs of known dCDNs. 
Every dCDN advertises a list of its known dCDNs.

## Running dCDNs
Running a dCDN should be as simple as possible. They mirror files and serve them statically over HTTP.
For efficient queries, a CDNs organizes its directory in a simple Radix tree:

```
peers.js // Advertising all other known dCDNs
/1
  /a
    /...
  /b
    /...
  /2
    /...
/2
/c
  /a
    /b
      /...
/d
...
```

This is simple to update. If a subfolder contains too many files it is split up.


## Foreign Fetch
We can use a foreign fetch service worker to centralize all requests of all apps running in a browser into a single service worker caching those responses.
