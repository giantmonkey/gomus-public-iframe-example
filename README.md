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
	
	// get the go url
	var go_url = getParameterByName('go_url');
	
	// create the iframe in pure javascript
	var ifrm = document.createElement('iframe');
	
	// assign an id
	ifrm.setAttribute('id', 'go_ifrm');
	
	// insert into exisiting element
	var el = document.getElementById('go_marker');
	el.appendChild(ifrm);
	
	// assign url
	ifrm.setAttribute('src', go_url);
	
	// set frameborder
	ifrm.setAttribute('frameborder', 'none');
	
	// set allowtransparency
	ifrm.setAttribute('allowtransparency', 'true');
	
	// assign width
	ifrm.setAttribute('width', '100%');
	
	// assign height
	// ifrm.setAttribute('height', '500');
	// or
	// use iframe resizer
	// see https://github.com/davidjbradshaw/iframe-resizer for details
	iFrameResize();

```

Add an iframe container div into your page:

```html
	<div id="go_marker"></div>

```
