# Mobile CLI Server API

JSON-RPC API for mobile device automation and control

**Version:** 1.0.0

## Table of Contents

- [devices.list](#devices-list)
- [pool.list](#pool-list)
- [pool.allocate](#pool-allocate)
- [pool.release](#pool-release)
- [device.screenshot](#device-screenshot)
- [device.screencapture](#device-screencapture)
- [device.io.tap](#device-io-tap)
- [device.io.longpress](#device-io-longpress)
- [device.io.text](#device-io-text)
- [device.io.button](#device-io-button)
- [device.io.gesture](#device-io-gesture)
- [device.url](#device-url)
- [device.io.swipe](#device-io-swipe)
- [device.info](#device-info)
- [device.io.orientation.get](#device-io-orientation-get)
- [device.io.orientation.set](#device-io-orientation-set)
- [device.boot](#device-boot)
- [device.shutdown](#device-shutdown)
- [device.reboot](#device-reboot)
- [device.dump.ui](#device-dump-ui)
- [device.apps.launch](#device-apps-launch)
- [device.apps.terminate](#device-apps-terminate)
- [device.apps.list](#device-apps-list)
- [device.apps.foreground](#device-apps-foreground)
- [device.apps.install](#device-apps-install)
- [uploads.create](#uploads-create)
- [devices.subscribe](#devices-subscribe)
- [devices.unsubscribe](#devices-unsubscribe)
- [devices.changed](#devices-changed)
- [Error Codes](#error-codes)

## Methods

### devices.list

**List all connected devices**

Returns a list of all connected mobile devices (iOS and Android)

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `includeOffline` | `boolean` |  | Include offline devices in the list |
| `platform` | enum: `ios, android` |  | Filter devices by platform (ios or android) |
| `type` | `string` |  | Filter devices by type (device or simulator) |

#### Response

**Type:** Array<`Device`>

List of connected devices

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "devices.list",
  "params": {
    "includeOffline": false,
    "platform": "ios",
    "type": "string"
  },
  "id": 1
}
```


### pool.list

**List available pool devices**

Returns unique device offerings from unallocated devices in the pool, deduplicated by platform/type/name/version

#### Response

**Type:** `object`

Available pool devices

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "pool.list",
  "params": {},
  "id": 1
}
```


### pool.allocate

**Allocate a device from the pool**

Allocates an available device matching the given criteria for the authenticated user

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `platform` | enum: `ios, android` | ✓ | Device platform (ios or android) |
| `type` | `string` |  | Device type filter (e.g. phone, tablet) |
| `version` | `string` |  | OS version filter |

#### Response

**Type:** `object`

Allocated device information

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "pool.allocate",
  "params": {
    "platform": "ios",
    "type": "string",
    "version": "string"
  },
  "id": 1
}
```


### pool.release

**Release an allocated device**

Releases a device allocated by the authenticated user. Sends a reboot command to the device host and removes it from the registry. The device will reconnect after rebooting.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the device to release |

#### Response

**Type:** `SuccessResult`

Release operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "pool.release",
  "params": {
    "deviceId": "string"
  },
  "id": 1
}
```


### device.screenshot

**Take a screenshot of a device**

Captures a screenshot from the specified device and returns it as base64 data

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `format` | enum: `png, jpeg` |  | Image format (png or jpeg) |
| `quality` | `integer` |  | Image quality (1-100, only used for JPEG) |

#### Response

**Type:** `ScreenshotResult`

Screenshot data

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.screenshot",
  "params": {
    "deviceId": "string",
    "format": "png",
    "quality": 1
  },
  "id": 1
}
```


### device.screencapture

**Start screen capture streaming**

Starts screen capture streaming for the specified device. Supports MJPEG (iOS and Android) and AVC/H.264 (Android only) formats.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `format` | enum: `mjpeg, avc` |  | Video format - 'mjpeg' for MJPEG stream (iOS and Android) or 'avc' for H.264 stream (Android only) |
| `quality` | `integer` |  | Video quality (only used for MJPEG format) |
| `scale` | `number` |  | Video scale factor |

#### Response

**Type:** `string`

Video stream - multipart/x-mixed-replace for MJPEG or video/h264 for AVC

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.screencapture",
  "params": {
    "deviceId": "string",
    "format": "mjpeg",
    "quality": 0,
    "scale": 0
  },
  "id": 1
}
```


### device.io.tap

**Perform tap gesture**

Performs a tap gesture at the specified coordinates on the device screen

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `x` | `integer` | ✓ | X coordinate for the tap |
| `y` | `integer` | ✓ | Y coordinate for the tap |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.io.tap",
  "params": {
    "deviceId": "string",
    "x": 0,
    "y": 0
  },
  "id": 1
}
```


### device.io.longpress

**Perform long press gesture**

Performs a long press gesture at the specified coordinates on the device screen

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `x` | `integer` | ✓ | X coordinate for the long press |
| `y` | `integer` | ✓ | Y coordinate for the long press |
| `duration` | `integer` |  | Duration of the long press in milliseconds |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.io.longpress",
  "params": {
    "deviceId": "string",
    "x": 0,
    "y": 0,
    "duration": 500
  },
  "id": 1
}
```


### device.io.text

**Input text**

Inputs the specified text on the device

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `text` | `string` | ✓ | Text to input |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.io.text",
  "params": {
    "deviceId": "string",
    "text": "string"
  },
  "id": 1
}
```


### device.io.button

**Press device button**

Presses a physical or virtual button on the device

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `button` | `string` | ✓ | Button to press |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.io.button",
  "params": {
    "deviceId": "string",
    "button": "string"
  },
  "id": 1
}
```


### device.io.gesture

**Perform custom gesture**

Performs a custom gesture with multiple actions on the device

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `actions` | Array<`object`> | ✓ | List of gesture actions to perform |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.io.gesture",
  "params": {
    "deviceId": "string",
    "actions": [
      {}
    ]
  },
  "id": 1
}
```


### device.url

**Open URL**

Opens the specified URL on the device

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `url` | `string` | ✓ | URL to open |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.url",
  "params": {
    "deviceId": "string",
    "url": "string"
  },
  "id": 1
}
```


### device.io.swipe

**Perform swipe gesture**

Performs a swipe gesture from one coordinate to another on the device screen

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `x1` | `integer` | ✓ | Starting X coordinate |
| `y1` | `integer` | ✓ | Starting Y coordinate |
| `x2` | `integer` | ✓ | Ending X coordinate |
| `y2` | `integer` | ✓ | Ending Y coordinate |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.io.swipe",
  "params": {
    "deviceId": "string",
    "x1": 0,
    "y1": 0,
    "x2": 0,
    "y2": 0
  },
  "id": 1
}
```


### device.info

**Get device information**

Returns detailed information about the specified device

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |

#### Response

**Type:** `DeviceInfo`

Device information

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.info",
  "params": {
    "deviceId": "string"
  },
  "id": 1
}
```


### device.io.orientation.get

**Get device orientation**

Returns the current orientation of the device screen

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |

#### Response

**Type:** `object`

Current device orientation

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.io.orientation.get",
  "params": {
    "deviceId": "string"
  },
  "id": 1
}
```


### device.io.orientation.set

**Set device orientation**

Sets the orientation of the device screen

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `orientation` | `string` | ✓ | Desired orientation |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.io.orientation.set",
  "params": {
    "deviceId": "string",
    "orientation": "string"
  },
  "id": 1
}
```


### device.boot

**Boot a device**

Boots the specified device (simulators/emulators only)

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |

#### Response

**Type:** `object`

Boot operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.boot",
  "params": {
    "deviceId": "string"
  },
  "id": 1
}
```


### device.shutdown

**Shutdown a device**

Shuts down the specified device (simulators/emulators only)

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |

#### Response

**Type:** `object`

Shutdown operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.shutdown",
  "params": {
    "deviceId": "string"
  },
  "id": 1
}
```


### device.reboot

**Reboot a device**

Reboots the specified device (simulators/emulators only)

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |

#### Response

**Type:** `object`

Reboot operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.reboot",
  "params": {
    "deviceId": "string"
  },
  "id": 1
}
```


### device.dump.ui

**Dump UI hierarchy**

Dumps the UI hierarchy of the device screen

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `format` | enum: `json, raw` |  | Output format (json or raw) |

#### Response

**Type:** `object`

UI hierarchy data

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.dump.ui",
  "params": {
    "deviceId": "string",
    "format": "json"
  },
  "id": 1
}
```


### device.apps.launch

**Launch an application**

Launches an application by bundle ID on the specified device

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `bundleId` | `string` | ✓ | Bundle ID of the application to launch |

#### Response

**Type:** `object`

Launch operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.apps.launch",
  "params": {
    "deviceId": "string",
    "bundleId": "string"
  },
  "id": 1
}
```


### device.apps.terminate

**Terminate an application**

Terminates a running application by bundle ID on the specified device

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `bundleId` | `string` | ✓ | Bundle ID of the application to terminate |

#### Response

**Type:** `object`

Terminate operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.apps.terminate",
  "params": {
    "deviceId": "string",
    "bundleId": "string"
  },
  "id": 1
}
```


### device.apps.list

**List installed applications**

Returns a list of installed applications on the specified device

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` |  | ID of the target device (optional - will auto-select if not provided) |

#### Response

**Type:** Array<`object`>

List of installed applications

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.apps.list",
  "params": {
    "deviceId": "string"
  },
  "id": 1
}
```


### device.apps.foreground

**Get foreground application**

Returns the currently foreground (active) application on the specified device

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` |  | ID of the target device (optional - will auto-select if not provided) |

#### Response

**Type:** `object`

Foreground application information

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.apps.foreground",
  "params": {
    "deviceId": "string"
  },
  "id": 1
}
```


### device.apps.install

**Install an application on a device**

Downloads a previously uploaded file from S3 and installs it on the specified device. The caller must be the same user who created the upload.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `uploadId` | `string` | ✓ | ID of the upload (from uploads.create) |

#### Response

**Type:** `SuccessResult`

Install operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.apps.install",
  "params": {
    "deviceId": "string",
    "uploadId": "string"
  },
  "id": 1
}
```


### uploads.create

**Create a new file upload**

Generates a presigned S3 URL for uploading a file. The URL enforces the exact file size specified. The filename may only contain letters, digits, hyphens, underscores, and dots.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `filename` | `string` | ✓ | Name of the file to upload (only 0-9, a-z, A-Z, hyphen, underscore, and dot allowed) |
| `filesize` | `integer` | ✓ | Exact size of the file in bytes |

#### Response

**Type:** `UploadResult`

Upload details

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "uploads.create",
  "params": {
    "filename": "string",
    "filesize": 1
  },
  "id": 1
}
```


### devices.subscribe

**Subscribe to device change notifications**

Starts receiving devices.changed notifications for the authenticated user's allocated devices. The subscription is tied to the WebSocket connection and ends when the connection closes or devices.unsubscribe is called.

#### Response

**Type:** `SuccessResult`

Subscription acknowledgment

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "devices.subscribe",
  "params": {},
  "id": 1
}
```


### devices.unsubscribe

**Unsubscribe from device change notifications**

Stops receiving devices.changed notifications. The subscription also ends automatically when the WebSocket connection closes.

#### Response

**Type:** `SuccessResult`

Unsubscription acknowledgment

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "devices.unsubscribe",
  "params": {},
  "id": 1
}
```


### devices.changed

**Device change notification (server-to-client)**

A JSON-RPC notification (no id field) sent by the server when the user's device list changes. Only sent while the client has an active subscription via devices.subscribe.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `added` | Array<`Device`> | ✓ | Devices newly allocated to this user |
| `removed` | Array<`Device`> | ✓ | Devices no longer allocated to this user |
| `updated` | Array<`Device`> | ✓ | Devices whose state has changed (e.g. online/offline) |

#### Response

**Type:** `null`

This is a notification — no response is expected

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "devices.changed",
  "params": {
    "added": [
      null
    ],
    "removed": [
      null
    ],
    "updated": [
      null
    ]
  },
  "id": 1
}
```


## Error Codes

| Code | Name | Message | Description |
|------|------|---------|-------------|
| `-32700` | **ParseError** | Parse error | Invalid JSON was received by the server |
| `-32600` | **InvalidRequest** | Invalid Request | The JSON sent is not a valid Request object |
| `-32601` | **MethodNotFound** | Method not found | The method does not exist or is not available |
| `-32602` | **InvalidParams** | Invalid params | Invalid method parameters |
| `-32603` | **InternalError** | Internal error | Internal JSON-RPC error |
| `-32000` | **ServerError** | Server error | Unexpected internal server error |
| `-32001` | **Unauthorized** | Unauthorized | Missing or invalid authentication credentials |
| `-32002` | **TokenExpired** | Token expired | Authentication token has expired |
| `-32003` | **Forbidden** | Forbidden | Authenticated but not authorized to access this resource |
| `-32010` | **DeviceNotFound** | Device not found | The specified device does not exist |
| `-32011` | **UploadNotFound** | Upload not found | The specified upload does not exist |
| `-32020` | **DeviceNotAllocated** | Device not allocated | The device is not currently allocated or has been released |
| `-32021` | **DeviceUnavailable** | Device unavailable | The device host session is disconnected |
| `-32022` | **NoMatchingDevice** | No matching device | No available device matches the requested criteria |
| `-32030` | **TooManyConnections** | Too many connections | Connection limit exceeded for this user |
| `-32031` | **FileSizeExceeded** | File size exceeded | File size exceeds the maximum allowed limit |
| `-32040` | **InvalidFilename** | Invalid filename | Filename contains invalid characters |
| `-32041` | **InvalidFileSize** | Invalid file size | File size must be a positive number |
| `-32042` | **UploadFailed** | Upload failed | Failed to generate upload URL |
| `-32050` | **DeviceTimeout** | Device timeout | The device did not respond in time |
