# Mobile CLI Server API

JSON-RPC API for mobile device automation and control

**Version:** 0.0.1

## Table of Contents

- [device.apps.clear](#deviceappsclear)
- [device.apps.foreground](#deviceappsforeground)
- [device.apps.install](#deviceappsinstall)
- [device.apps.launch](#deviceappslaunch)
- [device.apps.list](#deviceappslist)
- [device.apps.terminate](#deviceappsterminate)
- [device.apps.uninstall](#deviceappsuninstall)
- [device.boot](#deviceboot)
- [device.crashes.get](#devicecrashesget)
- [device.crashes.list](#devicecrasheslist)
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
- [device.webview.attach](#devicewebviewattach)
- [device.webview.content](#devicewebviewcontent)
- [device.webview.detach](#devicewebviewdetach)
- [device.webview.element.check](#devicewebviewelementcheck)
- [device.webview.element.click](#devicewebviewelementclick)
- [device.webview.element.fill](#devicewebviewelementfill)
- [device.webview.element.focus](#devicewebviewelementfocus)
- [device.webview.element.getAttribute](#devicewebviewelementgetattribute)
- [device.webview.element.getProperty](#devicewebviewelementgetproperty)
- [device.webview.element.hover](#devicewebviewelementhover)
- [device.webview.element.inspect](#devicewebviewelementinspect)
- [device.webview.element.press](#devicewebviewelementpress)
- [device.webview.element.scrollIntoView](#devicewebviewelementscrollintoview)
- [device.webview.element.selectOption](#devicewebviewelementselectoption)
- [device.webview.element.type](#devicewebviewelementtype)
- [device.webview.element.waitFor](#devicewebviewelementwaitfor)
- [device.webview.evaluate](#devicewebviewevaluate)
- [device.webview.goBack](#devicewebviewgoback)
- [device.webview.goForward](#devicewebviewgoforward)
- [device.webview.goto](#devicewebviewgoto)
- [device.webview.list](#devicewebviewlist)
- [device.webview.query](#devicewebviewquery)
- [device.webview.reload](#devicewebviewreload)
- [device.webview.screenshot](#devicewebviewscreenshot)
- [device.webview.title](#devicewebviewtitle)
- [device.webview.url](#devicewebviewurl)
- [device.webview.waitForLoadState](#devicewebviewwaitforloadstate)
- [device.webview.waitForUrl](#devicewebviewwaitforurl)
- [devices.list](#deviceslist)
- [server.info](#serverinfo)
- [server.shutdown](#servershutdown)
- [Error Codes](#error-codes)
- [Schemas](#schemas)

## Methods

### device.apps.clear

**Clear application data**

Clears all data (cache, preferences, databases) for an application without uninstalling it. Supported on Android and iOS Simulator. Not supported on real iOS devices.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` |  | ID of the target device (optional - will auto-select if not provided) |
| `bundleId` | `string` | ✓ | Bundle identifier (iOS) or package name (Android) of the application to clear |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.apps.clear",
  "params": {
    "deviceId": "string",
    "bundleId": "string"
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

**Type:** [`SuccessResult`](#successresult)

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
| `locales` | Array<`string`> |  | BCP 47 locale tags to set for the app (e.g. ["fr-FR", "en-GB"]). On iOS this is a per-launch argument. On Android 13+ this is persistent. |

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
    "bundleId": "string",
    "locales": [
      "string"
    ]
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

**Type:** [`SuccessResult`](#successresult)

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


### device.crashes.get

**Get a crash report**

Returns the full content of a specific crash report by ID. The ID is obtained from device.crashes.list.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` |  | ID of the target device (optional - will auto-select if not provided) |
| `id` | `string` | ✓ | Crash report ID (from device.crashes.list) |

#### Response

**Type:** `object`

Crash report content

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.crashes.get",
  "params": {
    "deviceId": "string",
    "id": "string"
  },
  "id": 1
}
```


### device.crashes.list

**List crash reports**

Returns a list of crash reports from the specified device. Supports iOS real devices (via crashreport service), iOS simulators (reads from DiagnosticReports), and Android devices (parses adb logcat crash buffer).

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` |  | ID of the target device (optional - will auto-select if not provided) |

#### Response

**Type:** Array<`object`>

List of crash reports

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.crashes.list",
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

**Type:** [`DeviceInfo`](#deviceinfo)

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

**Type:** [`SuccessResult`](#successresult)

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

**Type:** [`SuccessResult`](#successresult)

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

**Type:** [`SuccessResult`](#successresult)

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

**Type:** [`SuccessResult`](#successresult)

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

**Type:** [`SuccessResult`](#successresult)

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

**Type:** [`SuccessResult`](#successresult)

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

**Type:** [`SuccessResult`](#successresult)

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

**Type:** [`ScreenshotResult`](#screenshotresult)

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

**Type:** [`SuccessResult`](#successresult)

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


### device.webview.attach

**Attach to a webview**

Allocates a session for the given webview and injects the bridge shim. Returns a session ID used by all subsequent webview calls.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `id` | `string` | ✓ | Webview ID from device.webview.list |

#### Response

**Type:** [`WebViewSession`](#webviewsession)

Webview session

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.attach",
  "params": {
    "deviceId": "string",
    "id": "string"
  },
  "id": 1
}
```


### device.webview.content

**Get webview HTML content**

Returns the outerHTML of the attached webview's <html> element.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |

#### Response

**Type:** `object`

Webview HTML content

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.content",
  "params": {
    "deviceId": "string",
    "sessionId": "string"
  },
  "id": 1
}
```


### device.webview.detach

**Detach from a webview**

Releases the session and frees server-side state. The session ID is invalid after this call.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID from device.webview.attach |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.detach",
  "params": {
    "deviceId": "string",
    "sessionId": "string"
  },
  "id": 1
}
```


### device.webview.element.check

**Set checked state on a webview checkbox or radio**

Sets the checked state of a checkbox or radio input. No-op if the element is already in the requested state.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID of the checkbox or radio |
| `checked` | `boolean` | ✓ | Desired checked state |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.check",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string",
    "checked": false
  },
  "id": 1
}
```


### device.webview.element.click

**Click webview element**

Clicks the given DOM element. Auto-waits for the element to be actionable (visible, enabled, stable) before dispatching.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID from device.webview.query |
| `options` | `object` |  | Click options (button, clickCount, force, etc) |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.click",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string",
    "options": {}
  },
  "id": 1
}
```


### device.webview.element.fill

**Fill webview input element**

Clears the input element's value and types the given value into it.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID of the input element |
| `value` | `string` | ✓ | Value to set |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.fill",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string",
    "value": "string"
  },
  "id": 1
}
```


### device.webview.element.focus

**Focus webview element**

Calls focus() on the given element.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID to focus |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.focus",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string"
  },
  "id": 1
}
```


### device.webview.element.getAttribute

**Get a webview element attribute**

Returns the value of the given DOM attribute, or null if the attribute is not set.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID |
| `name` | `string` | ✓ | Attribute name |

#### Response

**Type:** `object`

Attribute value, or null when the attribute is not set

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.getAttribute",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string",
    "name": "string"
  },
  "id": 1
}
```


### device.webview.element.getProperty

**Get a webview element JS property**

Returns the JSON-serialized value of the given DOM property (e.g. 'value', 'checked', 'innerText').

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID |
| `name` | `string` | ✓ | Property name |

#### Response

**Type:** `object`

Serialized property value

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.getProperty",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string",
    "name": "string"
  },
  "id": 1
}
```


### device.webview.element.hover

**Hover over webview element**

Dispatches mouseenter/mouseover/mousemove events at the center of the given element.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID to hover |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.hover",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string"
  },
  "id": 1
}
```


### device.webview.element.inspect

**Inspect webview element state**

Returns a bundled snapshot of the element's common state in a single round-trip: text, value, attributes, bounding box, and visibility/enabled/checked flags.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID to inspect |

#### Response

**Type:** [`WebViewElementState`](#webviewelementstate)

Element state snapshot

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.inspect",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string"
  },
  "id": 1
}
```


### device.webview.element.press

**Press a key on a webview element**

Dispatches a keyboard event to the given element (e.g. 'Enter', 'ArrowDown', 'a').

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID to dispatch the key to |
| `key` | `string` | ✓ | Key name following the W3C UI Events KeyboardEvent.key spec |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.press",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string",
    "key": "string"
  },
  "id": 1
}
```


### device.webview.element.scrollIntoView

**Scroll webview element into view**

Calls scrollIntoView({ block: 'center', inline: 'center' }) on the given element.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID to scroll into view |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.scrollIntoView",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string"
  },
  "id": 1
}
```


### device.webview.element.selectOption

**Select options in a webview <select>**

Selects one or more options in a <select> element by value. For single-select, only the first value is used.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID of the <select> element |
| `values` | Array<`string`> | ✓ | Option values to select |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.selectOption",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string",
    "values": [
      "string"
    ]
  },
  "id": 1
}
```


### device.webview.element.type

**Type into webview element**

Appends the given text to the focused element. Does not clear existing content.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID to focus and type into |
| `text` | `string` | ✓ | Text to type |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.type",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string",
    "text": "string"
  },
  "id": 1
}
```


### device.webview.element.waitFor

**Wait for webview element state**

Polls server-side until the given element matches the requested state or the timeout elapses.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `nodeId` | `string` | ✓ | Node ID to wait on |
| `state` | enum: `visible, hidden, attached, detached, enabled, disabled` | ✓ | State to wait for |
| `timeout` | `integer` |  | Timeout in milliseconds |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.element.waitFor",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "nodeId": "string",
    "state": "visible",
    "timeout": 30000
  },
  "id": 1
}
```


### device.webview.evaluate

**Evaluate JavaScript in webview**

Evaluates a JavaScript expression in the attached webview and returns the JSON-serialized result. Throws WebViewEvaluateError if the script throws.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `expression` | `string` | ✓ | JavaScript expression to evaluate. May be a function body invoked with args. |
| `args` | `array` |  | Arguments passed to the expression when it is a function body |

#### Response

**Type:** `object`

Serialized return value

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.evaluate",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "expression": "string",
    "args": []
  },
  "id": 1
}
```


### device.webview.goBack

**Navigate back in webview history**

Navigates one step back in the attached webview's session history.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.goBack",
  "params": {
    "deviceId": "string",
    "sessionId": "string"
  },
  "id": 1
}
```


### device.webview.goForward

**Navigate forward in webview history**

Navigates one step forward in the attached webview's session history.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.goForward",
  "params": {
    "deviceId": "string",
    "sessionId": "string"
  },
  "id": 1
}
```


### device.webview.goto

**Navigate webview to URL**

Navigates the attached webview to the given URL and optionally waits for a load state.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `url` | `string` | ✓ | URL to navigate to |
| `waitUntil` | enum: `load, domcontentloaded` |  | Load state to wait for after navigation |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.goto",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "url": "string",
    "waitUntil": "load"
  },
  "id": 1
}
```


### device.webview.list

**List webviews**

Returns all embedded webviews currently visible in the foreground app on the device. Browser apps (Safari, Chrome) are not included.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |

#### Response

**Type:** Array<[`WebView`](#webview)>

List of embedded webviews

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.list",
  "params": {
    "deviceId": "string"
  },
  "id": 1
}
```


### device.webview.query

**Query DOM elements in webview**

Resolves DOM elements in the attached webview using one of the supported strategies. Returns server-allocated node IDs that remain valid until the DOM mutates or the session ends.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `root` | `string` |  | Optional ancestor node ID to scope the search |
| `strategy` | enum: `css, role, text, label, placeholder, testId, altText, title` | ✓ | Selection strategy |
| `value` | `string` | ✓ | Strategy value (e.g. CSS selector, role name, text) |
| `options` | `object` |  | Strategy-specific options (e.g. exact, name) |
| `all` | `boolean` |  | If true, return every match; otherwise return at most one |

#### Response

**Type:** `object`

Matching node IDs

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.query",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "root": "string",
    "strategy": "css",
    "value": "string",
    "options": {},
    "all": false
  },
  "id": 1
}
```


### device.webview.reload

**Reload webview**

Reloads the current page in the attached webview.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `waitUntil` | enum: `load, domcontentloaded` |  | Load state to wait for after reload |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.reload",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "waitUntil": "load"
  },
  "id": 1
}
```


### device.webview.screenshot

**Capture webview screenshot**

Captures a screenshot of the attached webview's region only and returns it as base64 data.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `format` | enum: `png, jpeg` |  | Image format (png or jpeg) |
| `quality` | `integer` |  | Image quality (1-100, only used for JPEG) |

#### Response

**Type:** [`ScreenshotResult`](#screenshotresult)

Screenshot data

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.screenshot",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "format": "png",
    "quality": 1
  },
  "id": 1
}
```


### device.webview.title

**Get webview title**

Returns document.title of the attached webview.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |

#### Response

**Type:** `object`

Document title

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.title",
  "params": {
    "deviceId": "string",
    "sessionId": "string"
  },
  "id": 1
}
```


### device.webview.url

**Get webview URL**

Returns the current top-level URL of the attached webview.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |

#### Response

**Type:** `object`

Current URL

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.url",
  "params": {
    "deviceId": "string",
    "sessionId": "string"
  },
  "id": 1
}
```


### device.webview.waitForLoadState

**Wait for webview load state**

Polls the attached webview server-side until the requested load state is reached or the timeout elapses.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `state` | enum: `load, domcontentloaded` |  | Load state to wait for |
| `timeout` | `integer` |  | Timeout in milliseconds |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.waitForLoadState",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "state": "load",
    "timeout": 30000
  },
  "id": 1
}
```


### device.webview.waitForUrl

**Wait for webview URL**

Polls server-side until the attached webview's URL matches the given pattern or the timeout elapses.

#### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deviceId` | `string` | ✓ | ID of the target device |
| `sessionId` | `string` | ✓ | Webview session ID |
| `url` | `string` | ✓ | URL to match. Either a literal string or a JavaScript regex source (without delimiters). |
| `isRegex` | `boolean` |  | Treat url as a regex source instead of a literal string |
| `timeout` | `integer` |  | Timeout in milliseconds |

#### Response

**Type:** [`SuccessResult`](#successresult)

Operation result

#### Example Request

```json
{
  "jsonrpc": "2.0",
  "method": "device.webview.waitForUrl",
  "params": {
    "deviceId": "string",
    "sessionId": "string",
    "url": "string",
    "isRegex": false,
    "timeout": 30000
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

**Type:** Array<[`Device`](#device)>

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

**Type:** [`ServerInfo`](#serverinfo)

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

**Type:** [`SuccessResult`](#successresult)

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
| `-32100` | **WebViewNotFound** | Webview not found | The supplied webview id does not match any currently-attached webview |
| `-32101` | **WebViewSessionExpired** | Webview session expired | The session is no longer valid; the client must re-attach to the webview |
| `-32102` | **WebViewNodeNotFound** | Webview node not found | The supplied node id is stale because the DOM has mutated; the client must re-query |
| `-32103` | **WebViewNavigationFailed** | Webview navigation failed | The webview did not reach the requested load state in time |
| `-32104` | **WebViewEvaluateError** | Webview evaluate error | The script passed to device.webview.evaluate threw an exception |

## Schemas

### Device

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `id` | `string` | ✓ | Unique device identifier |
| `name` | `string` | ✓ | Device name |
| `platform` | enum: `ios, android` | ✓ | Device platform |
| `status` | `string` | ✓ | Device connection status |
| `model` | `string` | ✓ | Device model |
| `provider` | [`DeviceProvider`](#deviceprovider) |  | Provider information for this device |

### DeviceInfo

Detailed device information

`object`

### DeviceProvider

Describes where the device is provided from

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `type` | `string` | ✓ | Provider type (e.g. 'mobilefleet', 'local') |
| `sessionId` | `string` |  | Session identifier for this device allocation |

### Rect

Rectangle in pixels

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `x` | `integer` | ✓ |  |
| `y` | `integer` | ✓ |  |
| `width` | `integer` | ✓ |  |
| `height` | `integer` | ✓ |  |

### ScreenshotResult

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `format` | enum: `png, jpeg` | ✓ | Image format |
| `data` | `string` | ✓ | Base64 encoded image data with data URI prefix |

### ServerInfo

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `name` | `string` | ✓ | Server name |
| `version` | `string` | ✓ | Server version |

### SuccessResult

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `status` | enum: `ok` | ✓ | Operation status |

### WebView

An embedded webview attached to the foreground app

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `id` | `string` | ✓ | Stable handle for this webview within the device session |
| `url` | `string` | ✓ | Current top-level URL |
| `title` | `string` | ✓ | document.title |
| `bundleId` | `string` | ✓ | Bundle ID (iOS) or package name (Android) of the host app |
| `processName` | `string` |  | Host process name or identifier |
| `bounds` | [`Rect`](#rect) | ✓ | Webview position on screen, in screen coordinates |
| `isVisible` | `boolean` | ✓ | True when the webview has non-zero on-screen bounds |

### WebViewBoundingBox

Element bounds in two coordinate spaces so the client can compose with native locator bounds

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `screen` | [`Rect`](#rect) | ✓ | Element bounds in screen coordinates |
| `frame` | [`Rect`](#rect) | ✓ | Element bounds relative to the webview's own viewport |

### WebViewElementState

Bundled element state returned by device.webview.element.inspect

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `text` | `string` | ✓ | Element innerText |
| `value` | `string` |  | Input value (only present for form elements) |
| `attributes` | `object` | ✓ | DOM attributes as a name -> string-value map |
| `boundingBox` | [`WebViewBoundingBox`](#webviewboundingbox) | ✓ |  |
| `isVisible` | `boolean` | ✓ |  |
| `isHidden` | `boolean` | ✓ |  |
| `isEnabled` | `boolean` | ✓ |  |
| `isDisabled` | `boolean` | ✓ |  |
| `isChecked` | `boolean` |  | Only present for checkboxes and radios |

### WebViewSession

Session handle returned by device.webview.attach

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `sessionId` | `string` | ✓ | Opaque session identifier used by all subsequent webview calls |
