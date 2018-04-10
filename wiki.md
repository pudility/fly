# Documentation
---
You can look over here for API References

## Outline
<!-- TODO: depending on how long this ends up being, maybe add an outline/table of contents -->

## Basic Usage
You can use `fly.http.respondWith` to respond to any request made to your server. By default fly looks for a `index.js` file to run.

To start the server just run:
```bash
$ fly server
```

For more option, run
```bash
$ fly server --help
```

### `fly.http.respondWith`
```JavaScript
fly.http.respondWith(async function (req) {

})

//above is the equivalent of:
addEventListener('fetch', function (req) {

})
```
To send a response, just return a `new Response`. For more on responses visit [this page](https://developer.mozilla.org/en-US/docs/Web/API/Response). Responses basically contain two parts, the text or data and an object that can contain a status and headers. For example:
```JavaScript
new Response('Redirecting', // Before redirecting you the page will display "Redirecting"
  {
    headers: { // These are the html headers that will be sent
      'Location': 'https://fly.io/docs/apps/' //In this case the headers point to another location to be redirected to
    },
    status: 302 // This is the status (obviously)
  }
))
```

## Examples
[To check out some examples, look under the apps directory.](https://github.com/superfly/fly/tree/master/apps)

## Images
Basic image usage
```JavaScript
const image = await fly.Image.imageFromPath(pictureURL) //Make an image

return await image.toResponse() //Return a response with the image
```
Advanced image usage:
```JavaScript
const resp = await fetch(pictureURL)
const image = new fly.Image(await resp.arrayBuffer())

// webp for browsers that support it
if (webpAllowed(req.headers.get("accept"))) {
    image = image.webp()
    resp.headers.set("content-type", "image/webp")
}

image = await image.toImage()
return new Response(image.data, resp)
```

Fly uses [sharp](https://github.com/lovell/sharp) for image manipulation. Sharp uses C++ under the hood, so its super fast.

### Methods
#### background
Set the background for the embed, flatten and extend operations. The default background is {r: 0, g: 0, b: 0, alpha: 1}, black without transparency.

Example Usage:
```JavaScript
let image = await fly.Image.imageFromPath(pictureURL)
image = await image
  .background('#7743CE')
  .extend(10)
  .flatten()
return await resized.toResponse()
```

#### crop
Crop the resized image to the exact size specified, the default behavior.

Possible attributes of the optional sharp.gravity are north, northeast, east, southeast, south, southwest, west, northwest, center and centre.

Example Usage:
```JavaScript
const image = await fly.Image.imageFromPath(pictureURL)
const cropped = await image
  .resize(200, 200)
  .crop(fly.Image.strategy.entropy)
return await cropped.toResponse()
```

#### embed
Preserving aspect ratio, resize the image to the maximum width or height specified then embed on a background of the exact width and height specified.

If the background contains an alpha value then WebP and PNG format output images will contain an alpha channel, even when the input image does not.

Example Usage:
```JavaScript
const image = await fly.Image.imageFromPath(pictureURL)
const resized = await image
  .resize(200, 300)
  .embed()
return await resized.toResponse()
```

These are just some of the many methods, the rest can be found [here](https://fly.io/docs/apps/api/classes/fly.image.html).

## Serving static files
Fly can also serve static files. All files that are used must be put in the `.fly.yml` file like so:
```yml
files:
  - foo.html
  - bar.js
  - styles.css
```
File globs coming soon!

### Example usage
```JavaScript
fly.http.respondWith(async (req) => {
  const url = new URL(req.url)
  const res = await fetch("file:/" + url.pathname)
  res.headers.set("content-type", "text/html")
  return res
})
```
This will serve a file based on the pathname. For example, `localhost:3000/index.html` would return a response with the file `index.html`

Note: make sure that you do not respond with the same file for every pathname. <!-- Unsure about the wording of this - feel free to edit -->

## Caching
Fly also has powerful caching
<!-- TODO: fill this in -->

## Fetch
Fetch is a powerful method that allows you to fetch resources and make HTTP requests.

### Usage
Fetch can be used in the following way:
```JavaScript
fetch(input: RequestInfo, init?: RequestInit): Promise<Response>

// for example:
await fetch(url)
```

## The Fleet
Fly has 15 data centers in:

- Newark, New Jersey
- San Jose, California
- Seattle, Washington
- Los Angeles, California
- Dallas, Texas
- Chicago, Illinois
- Atlanta, Georgia
- Ashburn, Virginia
- Toronto, Canada
- Amsterdam, Netherlands
- Frankfurt, Germany
- Singapore
- Hong Kong, China
- Sydney, Australia
- Tokyo, Japan
- Sao Paulo, Brazil (soon)
- Johannesburg, South Africa (soon)

[![img](https://media.giphy.com/media/k8crD6isrjtEaj0jwF/giphy.gif)](https://github.com/pudility/fly/tree/Demos/apps/fly-fleet)
