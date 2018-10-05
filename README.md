# Vue Sharelinks

Add social media sharing links to your page with a simple Vue component.

## Supported Platforms

* Whatsapp
* Facebook
* Twitter
* Pinterest
* Tumblr
* Google+
* Linkedin

## Usage

```javascript
import VueSharelinks from 'vue-sharelinks';

new Vue({
    components: {VueSharelinks},
});
```

```html
<vue-sharelinks></vue-sharelinks>
```

## Props

* **url**

    The URL that will be shared. Defaults to `window.location.href`.

* **title**

    The title of the page being shared. Defaults to `window.title`.

* **image**

    An URL to an image preview of the URL being shared. Optional. Only used on pinterest.

* **platforms**

    An array of platform names to show. Default: all platforms will be displayed. Platforms specified in `extra-platforms` will always be shown.

* **extra-platforms**

    An array of platform definitions. Default: `[]`. E.g.:

    ```javascript
    {
        name: 'facebook',
        href: 'https://www.facebook.com/sharer/sharer.php?u=%URL%',
        width: 400,
        height: 500
    },
    ```

## Events

* **share-link-clicked(platform, url)**

    Fired when a share link is clicked.

## Slots

#### Link Contents

By default the links will contain the name of the platform. You can override these by providing a slot with the name of the platform.

```html
<vue-sharelinks>
  <slot name="facebook">
    <img src="facebook.jpg" alt="Facebook">
  </slot>
</vue-sharelinks>
```

#### Before and after

You can insert extra content at the start or end of the list.

```html
<vue-sharelinks>
  <slot name="before">
    Here is a list of social links
  </slot>
  <slot name="after">
    There was a list of social links
  </slot>
</vue-sharelinks>
```
