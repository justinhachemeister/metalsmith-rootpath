# metalsmith-rootpath

> Easily find the relative path to the root directory in your Metalsmith templates.
> 
> *Makes relative-to-root links a breeze!*

[![Build Status](https://travis-ci.org/radiovisual/metalsmith-rootpath.svg)](https://travis-ci.org/radiovisual/metalsmith-rootpath)

## Install

```
$ npm install --save metalsmith-rootpath
```
    
## Usage

**Note:** metalsmith-rootpath must run after any plugins that move files around, or create new files in your metalsmith build chain (like [metalsmith-permalinks](https://github.com/segmentio/metalsmith-permalinks), for example).

```js
var rootPath = require('metalsmith-rootpath');

Metalsmith(__dirname)
  .use(rootPath())
  .build(function(err) {
    if (err) throw err;
  });
  
```

#### Now the files in your Metalsmith build have a `rootPath` variable assigned to them.

## Examples

Let's assume you have a directory structure like this:

```
.
├── index.html
│ 
├── dir1
│   └── index.html
│        
├── dir2       
│   └── foo
│       └── index.html
│       
├── dir3   
│   └── foo
│       └── bar
│           └── baz
│               └── index.html
```

The `rootPath` values in each `index.html` file would be:

| File                               | `rootPath`        |
| :----------------------------------|:------------------|
| index.html                         | ""                | 
| dir1/index.html                    | "../"             |
| dir2/foo/index.html                | "../../"          |   
| dir3/foo/bar/baz/index.html        | "../../../../"    | 


#### Relative Links

Use the `rootPath` variable anywhere you want to grab static files relative to your directory. `rootPath` will 
find the root folder no matter how many levels deep your templates are.

For example, if the following line of markup were placed in `dir3/foo/bar/baz/index.html`:
```html
<link src="{{rootPath}}css/main.css" type="text/css" />
```

It would result in the following output:
```html
<link src="../../../../css/main.css" type="text/css" />
```

#### Relative Navigation

This `rootPath` variable is useful when building a relative navigation structure, for example, in your Handlebars 
template `partials/navigation.hbs`

```html
<ul>
    <li><a href="{{rootPath}}about/">About Us</a></li>
    <li><a href="{{rootPath}}awesome-page/">Awesome Page</a></li>
    <li><a href="{{rootPath}}contact/">Contact Us</a></li>
</ul>
```

### Important Notes

1. I am using the Handlebars syntax in the above examples, but the `rootPath` value is assigned to every file's metadata, 
so it can be accessed just as easily with your template language of choice.
1. You must run metalsmith-rootpath **AFTER** any other metalsmith plugins that move or create files, otherwise the rootPath value won't be assigned to any moved or created files. For example, if you are running the [metalsmith-permalinks](https://github.com/segmentio/metalsmith-permalinks) plugin that moves/creates files, then run metalsmith-rootpath **AFTER** you run permalinks.   

### License 

MIT @ [Michael Wuergler](http://www.numetriclabs.com)


