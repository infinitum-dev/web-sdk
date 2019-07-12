# Infinitum Web SDK

## Installation

The [Infinitum](https://www.npmjs.com/package/@infinitum/web-sdk) SDK can be
installed via [npm](https://npmjs.org/).

```
npm install @infinitum/web-sdk --save
```

## Usage

## API Initialization

To use the Inifnitum you need to provide the `API Key`, `API Secret`,
`API Token` and the corresponding `workspace`.

```js
import InfinitumSDK from '@infinitum/web-sdk'

const options = {
  workspace: env.WORKSPACE,
  app_token: env.APP_TOKEN,
  app_key: env.APP_KEY,
  app_secret: env.APP_SECRET,
}

InfinitumSDK.init(options)
  .then(() => {})
  .catch(error => {})
```

## Modules

To access modules, you must first fetch the module accessing the imported `InfinitumSDK`
object.

```js
InfinitumSDK.device
  .create(options)
  .then(response => {})
  .catch(error => {})
```

and then the methods are accessible (methods listed further on).

All error response objects are structure in the same fashion: 

```js
{
  "error":  {
    "message": "Error Message describing the error.",
    "type": "ERROR_TYPE",
    "status": 400
  }
}
```

### Analytics

#### Storing new Analytics Auth object

```js
let options = {
  provider: provider, // required
  method: method, // required
  device: device,
  value: value,
  action: action,
  proximity: proximity,
  position: position,
  enroll: enroll,
  identity: identity,
  app: app,
}

InfinitumSDK.analytics.auth
  .create(options)
  .then(response => {})
  .catch(error => {})
```

* `provider` - Authentication method provider.
* `method` - Authentication method.
* `device` - Device that issued the authentication request.
* `action` - Action that triggered the request.
* `proximity` - Proximity value.
* `position` - JSON Array containing the GPS coordinates of the device. (`{"lat": ..., "lng": ...}`)
* `enroll` - Enroll user if not registered flag.
* `identity` - Unique device identity.
* `app` - App info that issued the authentication request.


#### Get Analytics Auth object by ID

```js
InfinitumSDK.analytics.auth
  .create(id)
  .then(response => {})
  .catch(error => {})
```

#### Get all Analytics Auth objects

Retrieve a paginator object containing the first page of the analytics auth registry. 

```js
InfinitumSDK.analytics.auth
  .all()
  .then(response => {})
  .catch(error => {})
```

To retrieve the next page, use the `analytics.auth.nextPage()` method with the `next_page_url` parameter from the first request response.

```js
InfinitumSDK.analytics.auth
  .nextPage(response["next_page_url"])
  .then(nextpage => {})
  .catch(error => {})
```


### User

#### Create a new User

Register a new Infinitum user.

```js
let options = {
  name: name,
  birthdate: birthdate,
  email: email,
  phone: phone,
  photo: photo,     // file
  photo64: photo64, // base64
  langugage: language, 
  data: data,
}

InfinitumSDK.user
  .create(options)
  .then(response => {})
  .catch(error => {})
```

* `name` - User name.
* `birthdate` - User birthdate (DD/MM/AAAA).
* `email` - User email address.
* `phone` - User phone number.
* `photo` - User's face photo.
* `photo64` - User's face photo encoded in `base64`.
* `language` - User language (ex: 'pt-PT').
* `data` - JSON array containing additional custom information.

### Device

#### Create a new Device

Register a new Infinitum device.

```js
let options = {
  name: name, // required
  app_id: app_id, // required
  licensed: licensed, // required (0 or 1)
  ip: ip,
  mac_address: mac_address,
  identity: identity,
  device_type: device_type,
  app_version: app_version, 
  users: users, // json
}

InfinitumSDK.device
  .create(options)
  .then(response => {})
  .catch(error => {})
```

* `name` - Device name.
* `app_id` - Device corresponding application unique ID.
* `licensed` - Flag to indicate if the device is licensed on the API.
* `ip` - Device IP address.
* `mac_address` - Device MAC address.
* `identity` - Device unique identifier.
* `device_type` - Device type identifier - `Windows`, `Raspberry Pi`, `Android`... (defined in the API). 
* `app_version` - Device corresponding application version.
* `users` - JSON Array of users associated with the device.


#### Auth

#### Biometric

Authenticate against the Infinitum API using a user's face biometrics.

```js
let options = {
  photo: photo, // required (file)
}

InfinitumSDK.auth
  .biometric(options)
  .then(response => {})
  .catch(error => {})
```
* `photo` - User's face photo.

#### Camera

Before using any methods it is advised to `start(options)` the camera, otherwise the camera object will be placed in the HTML body.
The available options are: 
```js
let options = {
  container: container,
  elem: elem,
  device: device,
  preview: preview,
  frame: frame,
  live: live
}
```

* `container` - The container HTML element. Can contain a video element that will be used for the camera feed.
* `elem` - The container HTML element ID. (optional if provided container)
* `device` - `front` or `back` string, or camera device reference. ([docs](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/enumerateDevices))
* `preview` - Flag to show preview video. 
* `frame` - Flag to show rectangular frame on preview video. 
* `live` - Flag to enable live mode. 


#### Take Photo

```js
InfinitumSDK.camera
  .takePhoto()
  .then(response => {})
  .catch(error => {})
```

Returns the photo in `base64` encoding.

#### Take Document photo

Takes a photo of a document within a drawn frame and checks if it is valid. 

```js
InfinitumSDK.camera
  .takeDocument()
  .then(response => {})
  .catch(error => {})
```

Returns the photo in `base64` encoding.


## Socket Events

To use the Infinitum socket events, you must use the real time management module, `InfinitumSDK.rtm` and make use of the receiving and sending methods listed in the following table. The module is automatically initialized when the `.init()` is done.

```js
InfinitumSDK.rtm.on('connect', () => { });
```

#### Events

| Type 	|    Name    	| Payload 	|
|:----:	|:----------:	|:-------:	|
| on   	| connect    	|         	|
| on   	| disconnect 	|         	|
