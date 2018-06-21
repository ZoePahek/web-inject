# webInject

[![Packagist](https://img.shields.io/packagist/l/doctrine/orm.svg)](https://github.com/EvanLiu2968/web-inject)[![npm](https://img.shields.io/npm/v/web-inject.svg)](https://www.npmjs.com/package/web-inject)[![continuousphp](https://img.shields.io/continuousphp/git-hub/doctrine/dbal/master.svg)](https://www.npmjs.com/package/web-inject)[![Github file size](https://img.shields.io/github/size/Evanliu2968/web-inject/dist/webInject.min.js.svg)](https://raw.githubusercontent.com/EvanLiu2968/web-inject/master/dist/webInject.min.js)

Inject js and css into document, or preload images, audios or videos resources.
and you can call it with chaining.

## Usage

in ES5, you can ...
```html
<script type="text/javascript" src="https://raw.githubusercontent.com/EvanLiu2968/web-inject/master/dist/webInject.min.js"></script>
<script>
  window.webInject
  .js('https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js',function(){
    alert('jQuery is injected!')
  })
  .css('https://cdn.bootcss.com/bootstrap/4.1.0/css/bootstrap.min.css',function(){
    alert('Bootstrap is injected!')
  })
</script>
```

in ES6+, you can ...
```bash
# install it from npm
npm install web-inject --save
```
```javascript
import webInject from 'web-inject'
// or
const webInject = require('web-inject')
```


### Inject js or css tag

```javascript
const webInject = require('web-inject')
webInject
  .js('https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js',function(){
    alert('jQuery is injected!')
  })
  .css('https://cdn.bootcss.com/bootstrap/4.1.0/css/bootstrap.min.css',function(){
    alert('Bootstrap is injected!')
  })
```

### Inject js or css into document

```javascript
const webInject = require('web-inject')
const onComplete = function(){ alert('inject is completed!')}
webInject
  .js(
`
  [].forEach.call(document.querySelectorAll("*"), function(a) {
    a.style.outline = "1px solid #" + (~~(Math.random() * (1 << 24))).toString(16)
  });
`)
  .css(
`
  body{
    background: #20a0ff;
  }
`)
```

### Inject js or css list

```javascript
const webInject = require('web-inject')
webInject
  .js([
    'https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js',
    'https://cdn.bootcss.com/lodash.js/4.17.5/lodash.min.js'
  ])
  .css([
    'https://cdn.bootcss.com/bootstrap/4.1.0/css/bootstrap.min.css',
    'https://cdn.bootcss.com/font-awesome/4.7.0/css/font-awesome.min.css'
  ])
```

### Preload images, audios or videos

```javascript
const webInject = require('web-inject')
webInject
.preload({
  image: [
    'https://www.evanliu2968.com.cn/public/images/horse.png',
    'https://www.evanliu2968.com.cn/public/images/eagle.png'
  ],
  audio: [
    '/static/images/music/%E5%AE%8B%E5%86%AC%E9%87%8E%20-%20%E8%8E%89%E8%8E%89%E5%AE%89.mp3'
  ],
  video: [
    'http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4'
  ],
  urlMap: function(url, type){
    if(type == 'audio'){
      return 'https://evanliu2968.github.io' + url
    } else {
      return url
    }
  },
  onProgress: function(progress){
    // progress is float number between 0 and 100
  },
  onError: function(error){
    // error occured
  },
  onComplete: function(){
    // a callback when all resourses are preloaded
  }
})
```

### Create a new webInject

```javascript
/*
 * the webInject is new instance by create
 * then, It's the same usage as above.
 */
const webInject = require('web-inject').create({
  urlMap: function(url, type){
    if(type == 'css' && (! /^(http|\/)/.test(url))){
      // innerCSS opacity mixins for IE
      var t = url.match(/opacity:(\d?\.\d+);/);
      if (t != null) url = url.replace(t[0], "filter:alpha(opacity=" + parseFloat(t[1]) * 100 + ")")
      url = url + "\n"; // format perform view
    }
    if(type == 'image' && (! /^(http|\/\/)/.test(url))){
      // base url
      url = 'https://www.evanliu2968.com.cn' + url
    }
    return url
  },
  maxConnection: 4 // max Simultaneous Browser Connections at the same.
})
```

## License

[MIT](LICENSE)