# Node Cloud Vision API
node-cloud-vision-api-comoc is a node client wrapper for Cloud Vision API.

Cloud Vision API Docs
https://cloud.google.com/vision/docs/

Note that currently only limited preview for alpha-test users.

Supported features

Feature Type  | Description
------------- | -------------
FACE_DETECTION  | Run face detection
LANDMARK_DETECTION  | Run models to execute landmark detection
LOGO_DETECTION | Run models to execute product logo detection
LABEL_DETECTION | Run models to execute Image Content Analysis
TEXT_DETECTION | Run models to execute OCR on an image
SAFE_SEARCH_DETECTION | Run models to compute image safe search properties

## Difference between this version and the original version: [tejitak/node-cloud-vision-api](https://github.com/tejitak/node-cloud-vision-api) 

- v1(not v1alpha1) API compliant
- Base64 encoded image input avalilable

## Setup
### Preparation
- Sign up limited preview for Cloud Vision API https://cloud.google.com/vision/
- Cloud Vision API Key is needed

### Install
` npm install node-cloud-vision-api-comoc --save`

### Auth
API requests on node-cloud-vision-api-comoc is internally managed by [google-api-nodejs-client](https://github.com/google/google-api-nodejs-client/)

You can setup auth data with the following samples

* Use Server Key
```JavaScript
const vision = require('node-cloud-vision-api-comoc')
vision.init({auth: 'YOUR_API_KEY'})
```

* Use OAuth
```JavaScript
const vision = require('node-cloud-vision-api-comoc')
const google = require('googleapis')
const oauth2Client = new google.auth.OAuth2('YOUR_GOOGLE_OAUTH_CLIENT_ID', 'YOUR_GOOGLE_OAUTH_SECRET', 'YOUR_GOOGLE_OAUTH_CALLBACK_URL')
oauth2Client.setCredentials({refresh_token: 'YOUR_GOOGLE_OAUTH_REFRESH_TOKEN'})
vision.init({auth: oauth2Client})
```

* For others, see references.
[google-api-nodejs-client](https://github.com/google/google-api-nodejs-client/)

## Sample

```JavaScript
'use strict'
const vision = require('node-cloud-vision-api-comoc')

// init with auth
vision.init({auth: 'YOUR_API_KEY'})

// construct parameters
const req = new vision.Request({
  image: new vision.Image('/Users/tejitak/temp/test1.jpg'),
  features: [
    new vision.Feature('FACE_DETECTION', 4),
    new vision.Feature('LABEL_DETECTION', 10),
  ]
})

// send single request
vision.annotate(req).then((res) => {
  // handling response
  console.log(JSON.stringify(res.responses))
}, (e) => {
  console.log('Error: ', e)
})
```
See more in [test_annotate.js](https://github.com/tejitak/node-cloud-vision-api-comoc/blob/master/test_annotate.js)

## Remote image file sample
Image files on web can be specified with 'url' paramters in Image object

```JavaScript
const req = new vision.Request({
  image: new vision.Image({
    url: 'https://scontent-nrt1-1.cdninstagram.com/hphotos-xap1/t51.2885-15/e35/12353236_1220803437936662_68557852_n.jpg'
  }),
  features: [
    new vision.Feature('FACE_DETECTION', 1),
    new vision.Feature('LABEL_DETECTION', 10),
  ]
})
```
See more in [test_annotate_remote.js](https://github.com/tejitak/node-cloud-vision-api-comoc/blob/master/test_annotate_remote.js)

## Multiple Requests per API call

```JavaScript
// construct parameters
// 1st image of request is load from local
const req1 = new vision.Request({
  image: new vision.Image({
    path: '/Users/tejitak/temp/test1.jpg'
  }),
  features: [
    new vision.Feature('FACE_DETECTION', 4),
    new vision.Feature('LABEL_DETECTION', 10),
  ]
})

// 2nd image of request is load from Web
const req2 = new vision.Request({
  image: new vision.Image({
    url: 'https://scontent-nrt1-1.cdninstagram.com/hphotos-xap1/t51.2885-15/e35/12353236_1220803437936662_68557852_n.jpg'
  }),
  features: [
    new vision.Feature('FACE_DETECTION', 1),
    new vision.Feature('LABEL_DETECTION', 10),
  ]
})

// send multi requests by one API call
vision.annotate([req1, req2]).then((res) => {
  // handling response for each request
  console.log(JSON.stringify(res.responses))
}, (e) => {
  console.log('Error: ', e)
})
```
See more in [test_annotate_remote.js](https://github.com/tejitak/node-cloud-vision-api-comoc/blob/master/test_annotate_remote.js)

## Base64 encoded image sample
Base64 enocded image data can be specified with 'base64' paramters in Image object

```JavaScript
const req = new vision.Request({
  image: new vision.Image({
    base64: '/9j/4AAQSkZJRgABAQ...' // someting like this
  }),
  features: [
    new vision.Feature('FACE_DETECTION', 1),
    new vision.Feature('LABEL_DETECTION', 10),
  ]
})
```
## Supported Node Version

Recommended node version is above v4.0.0 because this module is implemented with ES6.

