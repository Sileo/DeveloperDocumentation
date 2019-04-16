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
Sileo featured are written using JSON. To made Sileo featured banner, create JSON file and name it ```sileo-featured.json``` at the top of your repository file.

**Note:** It must have followings.

* 16:9 ratio (1920x1080 px recommended) banner that does not have the name of the package inside it

## Structure
Sileo featured uses a class called ```FeaturedBannersView```

## Root object
| Key | Type | Description |
|---|:---:|---|
| `itemSize` | String | Size of image in featured banner |
| `itemCornerRadius` | String | Corner radius of featured banner |
| `banners` | Array of Banner objects | An array of banners that the featured should contain |

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

Recommended `itemSize` is `263, 148` and recommended `itemCornerRadius` is `10`.

## Banner object
| Key | Type | Description |
|---|:---:|---|
| `url` | String (URL) | A URL to the image that should be displayed in featured banner |
| `title` | String | The title of the banner |
| `package` | String (Bundle ID) | A bundle id of featured package |
| `hideShadow` | Boolean | A Shadow under the title

### Usage
```json
{  
   "url":"url of banner image",
   "title":"title of banner",
   "package":"bundle id of package",
   "hideShadow":false
}
```
For example, this is the banner of PowerGuard

```json
{  
   "url":"https://nexusrepo.kro.kr/sileo-featured-images/powerguardbanner.png",
   "title":"PowerGuard",
   "package":"com.peterdev.powerguard",
   "hideShadow":false
}
```

## Finish
Last, You need to put Banner object into Root object

### Code
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
You can refer to this.

[**Sileo featured of Dynastic Repository**](https://repo.dynastic.co/sileo-featured.json)  
[**Sileo featured of Nexus Repository**](https://nexusrepo.kro.kr/sileo-featured.json)
