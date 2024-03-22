# Web Scraping Guide (Under Construction)
**This guide is for scraping website data and creating a plugin using the Miru app API.**

**For detailed API information, please refer to https://miru.js.org/developer/**

# Basics
## Plugin Writing
Need some basic knowledge of JavaScript. If you're not familiar with JS, it's recommended to explore more detailed information online.

### Plugin Information
```javascript
// ==MiruExtension==
// @name  Extension Name
// @version v0.0.1
// @author Xxxx
// @lang zh-cn
// @license MIT
// @nsfw false
// @icon https://xxx.xxx.xxx/xxx.png
// @package xxx.xxx.xxx
// @type bangumi
// @webSite https://xxx.xxx.xxx/
// @description extension description
// ==/MiruExtension==
```
##### Type Explanation
|     @type     |      Description           |
| ------------- | ------------------------------ |
| `manga`      | Manga       |
| `bangumi`   | Video  ***Any video content (anime, movies, etc.) falls under bangumi***  |
| `fikushon`|       Novel |

### Basic Framework
```javascript
export default class extends Extension {
  async latest(page) {
    // Recently updated
  }
  async search(kw, page, filter) {
    // Search, called when app data reaches the bottom, or when the user uses filters and clicks "confirm"
  }
  async detail(url) {
    // Detailed information
  }
  async watch(url) {
    // Watch, called when clicking on an episode
  }
}
```

### Additional Settings (Optional)
```javascript
async load() {
  // Called when the plugin is loaded
}
async createFilter(filter) {
  // Create filters
}
```

### Return Format
#### latest
```javascript
[{
  title: String, // Title
  cover: String, // Cover link
  url: String, // URL for detail
  update: String, //  Updated detail for episode
}, ...]
```

#### search
```javascript
[{
  title: String, // Title
  cover: String, // Cover link
  url: String, // URL for detail
  update: String, // Updated detail for episode
}, ...]
```

#### watch
Different data returned for different types (@type)
+ #### bangumi
```javascript
{
  type: String, // Streaming type, only "hls," "mp4," "torrent" are valid
  url: String, // Link
  subtitles: [{
    title: String, // Subtitle title
    url: String // Subtitle link
  }, ...],
  audioTrack: String, // Audio link
  headers: {} // Additional headers if needed
}
```

+ #### manga
```javascript
{
  urls: [], // Image links
  headers: {} // Additional headers if needed
}
```

+ #### fikushon
```javascript
{
  content: [], // Text content
  title: String,
  headers: {} // Additional headers if needed
}
```

#### detail
```javascript
{
  title: String, // Title
  cover: String, // Cover link
  desc: String, // Description
  episodes: [{
    title: String, // Subtitle
    urls: {
      name: String, // Name
      url: String // Episode link
    }
  }, ...]
}
```

#### request
Send requests
```javascript
request(urlTail, {
  headers: {
    "Miru-Url": urlHead
  }
  // Optional
})
```
The sent URL is `urlHead + urlTail`. If `"Miru-Url"` is not declared, the app will automatically set `urlHead` it to `@website`.
##### example
```javascript
// the site you want to request is https://example.com/example and the @website is set to htts://example.com/
request("example")
// the site you want to request is https://anotherhost.com/example and the @website is set to htts://example.com/
request("",{
  headers:{
      "Miru-url":"https://anotherhost.com/example"
    }
})
request("example",{
  headers:{
      "Miru-url":"https://anotherhost.com/"
    }
})
```

### Regex
For regex, use JavaScript's native methods such as [`match`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match) and [`replace`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace).

### CSS Selectors

#### querySelector
```javascript
querySelector(document, selector)
```
Similar to the original `document.querySelector`.

#### querySelectorAll
```javascript
querySelectorAll(document, selector)
```

#### getAttributeText
```javascript
getAttributeText(document, selector, attribute)
```

## Resources and Tools
### Reference Resources
[miru repo](https://github.com/miru-project/repo) - Miru's official plugin repository.

The following are personal tools I often use and recommend:

+ ### Regex
  + [Regex101](https://regex101.com/)
    + A great website for testing regex.

+ ### CSS Selectors
  + [uBlock Origin](https://github.com/gorhill/uBlock)
    + Although it's an ad blocker, it has built-in CSS selectors. Clicking on the web interface will reveal the CSS path.
    + Built-in recorder records requests and files accessed on visited sites (useful for anti-debugging if  dev tools are blocked).
    + One-click to disable JavaScript.

+ ### Web API Testing
  + Postman

+ ### Tools for Anti-Debugger
  + Resource Override
