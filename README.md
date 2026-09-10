# gomus-public-iframe-example

Example of handling go~mus public routes

### Schema

![go_url](https://raw.githubusercontent.com/giantmonkey/gomus-public-iframe-example/master/go_url.png)

### Code

Include the following code into your page:

```javascript
  function getParameterByName(name, url) {
    if (!url) {
      url = window.location.href;
    }
    name = name.replace(/[\[\]]/g, "\\$&");
    var pattern = "[?&]" + name + "(=([^&#]*)|&|#|$)";
    var regex = new RegExp(pattern);
    var results = regex.exec(url);

    if (!results || !results[2]) return null;

    var raw = results[2].replace(/\+/g, " ");
    var resultUrl = decodeURIComponent(raw);

    var urlObject;
    try {
      urlObject = new URL(resultUrl);
    } catch (e) {
      return null;
    }
    var goDomain =
      /^([a-z0-9-]+\.)?gomus\.(de|cloud|lu|at)$/;

    if (urlObject.protocol !== "https:" ||
        !goDomain.test(urlObject.hostname)) {
      return null;
    }

    return resultUrl;
  }

  var el = document.getElementById('go_marker');
  var go_url = getParameterByName('go_url');

  if (el != null && go_url != null) {
    var go_url_arr = go_url.split("/");
    var go_url_base = go_url_arr[0] + "//" +
      go_url_arr[2];

    var ifrm = document.createElement('iframe');
    ifrm.setAttribute('id', 'go_ifrm');
    ifrm.setAttribute('csp', "\"object-src 'none'\"");
    var allow = "camera " + go_url_base +
      "; microphone " + go_url_base;
    ifrm.setAttribute('allow', allow);
    el.appendChild(ifrm);
    ifrm.setAttribute('src', go_url);
    ifrm.setAttribute('frameborder', 'none');
    ifrm.setAttribute('allowtransparency', 'true');
    ifrm.setAttribute('width', '100%');
    ifrm.setAttribute('height', '800');
  }
```

Add an iframe container div into your page:

```html
  <div id="go_marker"></div>
```

### What the check does

`go_url` comes from the address bar, so anyone can craft it. The script only loads the value into the iframe when it is an `https:` URL on a go~mus host. Checking the hostname alone is not enough: `javascript://x.gomus.cloud/%0a...` has a valid hostname and would run script in your page's origin.

Tighter and recommended: replace the generic pattern with the host of your own go~mus instance, for example

```javascript
    var goDomain = /^museum\.gomus\.de$/;
```
