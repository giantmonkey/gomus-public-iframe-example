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
    var regex = new RegExp("[?&]" + name + "(=([^&#]*)|&|#|$)"),
      results = regex.exec(url);
    if (!results) return null;
    if (!results[2]) return '';
    return decodeURIComponent(results[2].replace(/\+/g, " "));
  }


  var el = document.getElementById('go_marker');
  var go_url = getParameterByName('go_url');

  if ((typeof(el) != 'undefined' && el != null) && (typeof(go_url) != 'undefined' && go_url != null)) {
    // get the go url
    var go_url_arr = go_url.split("/");
    var go_url_base = go_url_arr[0] + "//" + go_url_arr[2]

    // create the iframe in pure javascript
    var ifrm = document.createElement('iframe');

    ifrm.setAttribute('id', 'go_ifrm');
    ifrm.setAttribute('csp', "\"object-src 'none'\"");
    ifrm.setAttribute('allow', "camera "+go_url_base+"; microphone "+go_url_base);

    // insert into exisiting element
    el.appendChild(ifrm);

    // assign url
    ifrm.setAttribute('src', go_url);

    // set frameborder
    ifrm.setAttribute('frameborder', 'none');

    // set allowtransparency
    ifrm.setAttribute('allowtransparency', 'true');

    // assign width
    ifrm.setAttribute('width', '100%');
    ifrm.setAttribute('height', '800');
  }
```

Add an iframe container div into your page:

```html
  <div id="go_marker"></div>

```
