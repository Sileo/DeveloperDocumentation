---
layout: doc
title: Native Depictions
permalink: /native-depictions
---
# Native Depictions
**NOTE:** This page is under heavy development and is subject to change.

Sileo uses a revolutionary new format for depictions, which can give your users amazing new experiences whilst browsing your repository. Learn how simple it is to support this new format, with examples.

---

## Getting started

Sileo native depictions are written using JSON. To add support for a Sileo depiction to a package, simply add a `SileoDepiction` key to your repo's Packages file. Here's a (shortend) example package entry for ShortLook.

```
Package: co.dynastic.ios.tweak.shortlook
Name: ShortLook
...
Depiction: https://repo.dynastic.co/depiction/68292503631036416/
SileoDepiction: https://repo.dynastic.co/sileo-depiction/68292503631036416/
```

**Note:** The `SileoDepiction` key will always take prescedent over the `Depiction` key when using Sileo.

<br>    

## Structure

Sileo depictions are composed of tabs, which contain an array of views. Views can be formed from any of the classes listed in the below section.

### Root object

<!-- TODO: Get AB to style tables :/ Spacing also seems slighly off. -->

| Key           | Type                                  | Description                                                                             |
|---------------|---------------------------------------|-----------------------------------------------------------------------------------------|
| `minVersion`  | String                                | Minimum app version required for Sileo to display the depiction. Should be 0.1 for now. |
| `headerImage` | String (URL)                          | A URL to the image that should be displayed in the header of the package page.          |
| `tabs`        | [Array of Tab objects](#tab-object) | An array of tabs that the depiction should display.                                     |

<br>

### Tab object

| Key       | Type                                  | Description                    |
|-----------|---------------------------------------|--------------------------------|
| `tabname` | String                                | The name of the tab.           |
| `views`   | [Array of View objects](#view-object) | The views (layout) of the tab. |

<br>

### View object

TODO: Document this and classes. SO. MUCH. EFFORT.