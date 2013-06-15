# How to add Flat-UI Design.
Here is a instruction how to add the [Flat-UI](https://github.com/designmodo/Flat-UI) Design.

1. Open up your Jquery File Upload,
Download [Flat-UI](https://github.com/designmodo/Flat-UI)
And extract the files in the css folder to your webserver's root.

2. Go to this line: 

```html
<link rel="stylesheet" href="http://blueimp.github.io/cdn/css/bootstrap.min.css">
```

3. And add: 

```html
<link rel="stylesheet" href="http://Yourdomain/css/flat-ui.css">
```

Under it.

 So it will look like this:

```html
<!-- Bootstrap CSS Toolkit styles -->
<link rel="stylesheet" href="http://blueimp.github.io/cdn/css/bootstrap.min.css">
<link rel="stylesheet" href="http://yourdomain.tld/css/flat-ui.css">
```

Its so easy as slicing through butter.