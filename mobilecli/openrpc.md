# Mobile CLI Server API

JSON-RPC API for mobile device automation and control

**Version:** 0.0.1

## Table of Contents

- [device.apps.foreground](#deviceappsforeground)
- [device.apps.install](#deviceappsinstall)
- [device.apps.launch](#deviceappslaunch)
- [device.apps.list](#deviceappslist)
- [device.apps.terminate](#deviceappsterminate)
- [device.apps.uninstall](#deviceappsuninstall)
- [device.boot](#deviceboot)
- [device.dump.ui](#devicedumpui)
- [device.info](#deviceinfo)
- [device.io.button](#deviceiobutton)
- [device.io.gesture](#deviceiogesture)
- [device.io.longpress](#deviceiolongpress)
- [device.io.orientation.get](#deviceioorientationget)
- [device.io.orientation.set](#deviceioorientationset)
- [device.io.swipe](#deviceioswipe)
- [device.io.tap](#deviceiotap)
- [device.io.text](#deviceiotext)
- [device.reboot](#devicereboot)
- [device.screencapture](#devicescreencapture)
- [device.screenshot](#devicescreenshot)
- [device.shutdown](#deviceshutdown)
- [device.url](#deviceurl)
- [devices.list](#deviceslist)
- [server.info](#serverinfo)
- [server.shutdown](#servershutdown)
- [Error Codes](#error-codes)

## Methods

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

**Install an application**

Installs an application on the specified device from a local file path. Supports optional IPA re-signing for real iOS devices.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `path` | `string` | ✓ | Local file path to the application package (.apk, .ipa, or .app) |
| `forceResign` | `boolean` |  | Re-sign the IPA with a local provisioning profile before installing (only for .ipa files on real iOS devices) |
| `provisioningProfile` | `string` |  | Path to a .mobileprovision file to use for re-signing. If not provided, a matching profile is auto-detected. |
| `signingIdentity` | `string` |  | Signing identity name or SHA-1 hash to use for re-signing. If not provided, a matching identity is auto-detected. |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.apps.install",
  "params": {
    "deviceId": "string",
    "path": "string",
    "forceResign": false,
    "provisioningProfile": "string",
    "signingIdentity": "string"
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


### device.apps.uninstall

**Uninstall an application**

Uninstalls an application from the specified device by its bundle/package ID

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` |  | ID of the target device (optional - will auto-select if not provided) |
| `bundleId` | `string` | ✓ | Bundle identifier (iOS) or package name (Android) of the application to uninstall |

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.apps.uninstall",
  "params": {
    "deviceId": "string",
    "bundleId": "string"
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


### server.info

**Get server information**

Returns the server name and version

#### Response

**Type:** `ServerInfo`

Server information

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "server.info",
  "params": {},
  "id": 1
}
```


### server.shutdown

**Shutdown the server**

Initiates a graceful server shutdown

#### Response

**Type:** `SuccessResult`

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "server.shutdown",
  "params": {},
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
| `-32010` | **DeviceNotFound** | Device not found | The specified device does not exist |
| `-32050` | **DeviceTimeout** | Device timeout | The device did not respond in time |
