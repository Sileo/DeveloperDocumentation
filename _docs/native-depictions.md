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

<!-- TODO: Get AB to style tables properly. -->

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

A tab is made up of multiple views, allowing repos to customize how their information is displayed on screen. There are multiple different kinds of views, each dictated by a different class.

Each view will define the `class` key, with the remaining keys required being determined by what class is used.

#### Headers

Class: `DepictionHeaderView`

Creates a large title intended for separating major sections of a given tab.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `title` | String | The title of the header.           | Yes
| `useMargins` | Boolean | Allow margins above/below the header. | No
| `useBottomMargin` | Boolean | Adds a margin below the header (does nothing if useMargins = false). | No
| `useBoldText` | Boolean | Make the text bold. | No

#### Subheaders

Class: `DepictionSubheaderView`

Subheaders are smaller headers.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `title` | String | The title of the header. | Yes
| `useMargins` | Boolean | Allow margins above/below the header. | No
| `useBottomMargin` | Boolean | Adds a margin below the header (does nothing if useMargins = false). | No
| `useBoldText` | Boolean | Make the text bold. | No

#### Markdown Text

Class: `DepictionMarkdownView`

Allows for basic Markdown or HTML to be displayed, ideal for large blocks of text.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `markdown` | String | The text to be rendered as Markdown (or HTML)           | Yes
| `useSpacing` | Boolean | Enable or disable the use of vertical spacing.           | No
| `useMargins` | Boolean | Enable or disable the use of horizontal margins.           | No
| `useRawFormat` | Boolean | If true, `markdown` will accept basic HTML instead of Markdown. | No

#### Screenshots

Class: `DepictionScreenshotsView`

Displays a screenshot carousel with provided images.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `screenshots` | [Array of Screenshot objects](#screenshot-object) | The screenshots to be used. | Yes
| `itemCornerRadius` | Double | Change the roundness of the view's corners.           | No
| `itemSize` | Dimensions (`{x,y}`) | Change the size of the view. | No

##### Screenshot Object

Screenshot objects just store a URL to an image and accessibility text to suppliment it.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `url` | String (URL) | A URL to the screenshot. | Yes
| `accessibilityText` | String | Text to be interpreted by accessibility features like VoiceOver. | Yes
| `video` | Boolean | Sets whether the URL is a video rather than an image | No

#### Table Text

Class: `DepictionTableTextView`

Adds a table cell with a given value that is ideal for displaying the current version or release date of a tweak.

| Key       | Type                                     | Description    | Required?
|-----------|---------------------------------------|-------------------|----------------|
| `title` | String | The title of the row. | Yes
| `text` | String | The text to be displayed next to the title. | Yes

#### Table Button

Class: `DepictionTableButtonView`

Adds a table cell that opens a given URL when tapped.

| Key       | Type                                     | Description    | Required?
|-----------|---------------------------------------|-------------------|----------------|
| `title` | String | The button's label. | Yes
| `action` | String (URL) | The URL to open when the button is pressed. | Yes
| `backupAction` | String (URL) | An alternate action to try if the action is not supported. | No
| `yPadding` | Double | Padding to put above and below the button. | No
| `openExternal` | Double | Set whether to open the URL in an external app. | No

#### Separator

Class: `DepictionSeparatorView`

Displays a separator.

#### Spacer

Class: `DepictionSpacerView`

Adds a space of variable height.

| Key       | Type                                     | Description    | Required?
|-----------|---------------------------------------|-------------------|----------------|
| `spacing` | Double | How high the spacer should be. | Yes

#### AdMob Integration

Class: `DepictionAdmobView`

Adds an [AdMob](https://admob.google.com/home/) banner to the depiction if you wish to display an advertisement.

| Key       | Type                                     | Description    | Required?
|-----------|---------------------------------------|-------------------|----------------|
| `adUnitID` | String | Your Ad Unit ID provided by Google. | Yes
