---
layout: doc
title: Sileo Featured
permalink: /sileo-featured
---
# Featured
Each repository can show featured banner in Sources tab.

## Preview
<img src="https://i.imgur.com/rsdFWQm.png" width="200" height="50%">
<img src="https://i.imgur.com/CM2tjK6.png" width="200" height="50%">

## Getting Started
Sileo's featured banners are written using JSON. To feature a tweak on your repo in a Sileo featured banner, create a JSON file and name it ```sileo-featured.json``` at the top of your repository file.

**Note:** It must have the following pre-requisites.

* A banner with a 16:9 ratio (1920x1080 px recommended).
  * While not required but reccomended, you should avoid putting text inside of the banner, as to get featured on Sileo's main page, you cannot have text inside banners.

## Structure
Sileo's featured tab uses a class called ```FeaturedBannersView```.

## Root object

| Key                | Type           | Description                                                | Required? |
| ------------------ | -------------- | ---------------------------------------------------------- | --------- |
| `itemSize`         | String         | Size of the image in the featured banner.                  | Yes       |
| `itemCornerRadius` | String         | The corner radius of the banners.                          | Yes       |
| `banners`          | String (Color) | An array of banners that the featured view should contain. | Yes       |


### Usage

```json
{  
   "class":"FeaturedBannersView",
   "itemSize":"{263, 148}",
   "itemCornerRadius":10,
   "banners":[  

   ]
}
```

The reccomendeded `itemSize` is `263, 148` and, recommendeded `itemCornerRadius` is `10`.

## Banner object

| Key          | Type               | Description                                                         |
| ------------ | ------------------ | ------------------------------------------------------------------- |
| `url`        | String (URL)       | A URL to the image that should be displayed in the featured banner. |
| `title`      | String             | The title of the banner.                                            |
| `package`    | String (Bundle ID) | A bundle id of featured package.                                    |
| `hideShadow` | Boolean            | Hide the shadow under/around the banner.                            |

<br>
### Usage
```json
{  
   "url":"url of banner image",
   "title":"title of banner",
   "package":"bundle id of package",
   "hideShadow":false
}
```
For example, this is the featured JSON for ShortLook. 

```json
{
   "url": "https://repo-cdn.dynastic.co/91373858589769729.97ea6548c456372ba1ba0e6640e7e73f-750?size=750",
   "title": "ShortLook",
   "package": "co.dynastic.ios.tweak.shortlook",
   "hideShadow": false
}
```

## Finish
To finish, put each `banner` object inside the root object.

### Usage

```json
{  
   "class":"FeaturedBannersView",
   "itemSize":"{263, 148}",
   "itemCornerRadius":10,
   "banners":[  
      {  
         "url":"url of banner image",
         "title":"title of banner",
         "package":"bundle id of package",
         "hideShadow":false
      },
      {  
         "url":"url of banner image",
         "title":"title of banner",
         "package":"bundle id of package",
         "hideShadow":false
      },
   ]
}
```

## Example
You can refer to these repo's `sileo-featured.json` files.

[**Sileo featured file for Dynastic Repository**](https://repo.dynastic.co/sileo-featured.json)  