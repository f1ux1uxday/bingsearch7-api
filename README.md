# Bingsearch7 API
Node.js lib for the Microsoft Cognitive Services Bing Web Search API

A fork of [node-bing-api](https://github.com/goferito/node-bing-api) by [goferito](https://github.com/goferito)

All I did was change the endpoint URL to 7.0

## Installation
````
npm install bingsearch7-api
````

## Usage

Require the library and initialize it with your account key:

```js
var Bing = require('bingsearch7-api')({ accKey: "your-account-key" });
```

## Promises

This API provdes callbacks by default, but users of node 8 and newer can make the library return Promises with `util.promisify()`. For example, to use `Bing.web`:

```js
var util = require('util'),
  Bing = require('node-bing-api')({ accKey: 'someKey' }),
  searchBing = util.promisify(Bing.web.bind(Bing));
```

#### Web Search (same as Composite):

```js
Bing.web("Pizza", {
    count: 10,  // Number of results (max 50)
    offset: 3   // Skip first 3 results
  }, function(error, res, body){

    // body has more useful information besides web pages
    // (image search, related search, news, videos)
    // but for this example we are just
    // printing the first two web page results
    console.log(body.webPages.value[0]);
    console.log(body.webPages.value[1]);
  });
```

#### Composite Search (same as Web):
```js
Bing.composite("Playstation 4 Pro", {
    count: 10,  // Number of results (max 15 for news, max 50 if other)
    offset: 3   // Skip first 3 results
  }, function(error, res, body){
    console.log(body.news);
  });
```

#### News Search:
```js
Bing.news("Anonymous", {
    count: 10,  // Number of results (max 15)
    offset: 3   // Skip first 3 results
  }, function(error, res, body){
    console.log(body);
  });
```

#### Video Search:
```js
Bing.video("monkey vs frog", {
    count: 10,  // Number of results (max 50)
    offset: 3   // Skip first 3 result
  }, function(error, res, body){
    console.log(body);
  });
```

#### Images Search:
```js
Bing.images("Ninja Turtles", {
  count: 15,   // Number of results (max 50)
  offset: 3    // Skip first 3 result
  }, function(error, res, body){
    console.log(body);
  });
```

#### Related Search (same as Web):
```js
Bing.relatedSearch('berlin'
, { market: 'en-US' }
, (err, res, body) => {
    const suggestions = body.relatedSearches.value.map(r => r.displayText)
    console.log(suggestions.join('\n'))
  })
```

#### Spelling Suggestions:
```js
Bing.spelling('awsome spell', function (err, res, body) {
  console.log(body.flaggedTokens.suggestions[0].suggestion); //awesome spell
});
```

Requires specific Account key

Available Options:
* mode:\<Proof | Spell\>
* preContextText:\<*String*\>
* postContextText:\<*String*\>

### request() options

* maxSockets: integer (default: Infinity)
* reqTimeout: ms
  
#### Specify Market
Getting spanish results:
```js
Bing.images("Ninja Turtles"
          , {count: 5, market: 'es-ES'}
          , function(error, res, body){

  console.log(body);
});
```

Full list of supported markets:
es-AR,en-AU,de-AT,nl-BE,fr-BE,pt-BR,en-CA,fr-CA,es-CL,da-DK,fi-FI,fr-FR,
de-DE,zh-HK,en-IN,en-ID,en-IE,it-IT,ja-JP,ko-KR,en-MY,es-MX,nl-NL,en-NZ,
no-NO,zh-CN,pl-PL,pt-PT,en-PH,ru-RU,ar-SA,en-ZA,es-ES,sv-SE,fr-CH,de-CH,
zh-TW,tr-TR,en-GB,en-US,es-US


#### Adult Filter
```js
Bing.images('Kim Kardashian'
          , {market: 'en-US', adult: 'Strict'}
          , function(error, res, body){

  console.log(body.value);
});
```
Accepted values: "Off", "Moderate", "Strict".

*Moderate level should not include results with sexually explicit images
or videos, but may include sexually explicit text.*

#### Web Only API Subscriptions
To use this library with a web only subscription, you can require and initialize it with an alternate root url:
```js
var Bing = require('node-bing-api')
            ({
              accKey: "your-account-key",
              rootUri: "https://api.datamarket.azure.com/Bing/SearchWeb/v1/"
            });
```

## Running Tests
In order to run the tests, the integration tests require to create a `secrets.js` file
from the provided `secrets.js.example` example, and fill it in with a valid access key.

Then just `mocha`.


## License
MIT

