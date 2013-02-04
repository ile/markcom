# markcom

A full-featured markdown parser and compiler, written in javascript.
Built for speed.

This is forked from [marked](https://github.com/chjj/marked), and is intended to be used in a comment sections on website, thus a little different feature set.

## Features added as compared to [marked](https://github.com/chjj/marked)

### options
 - linksToNewTab
	
	Open links in a new tab, default: false

## Features removed as compared to [marked](https://github.com/chjj/marked)

- headings

## Benchmarks

node v0.4.x

``` bash
$ node test --bench
markcom completed in 12071ms.
showdown (reuse converter) completed in 27387ms.
showdown (new converter) completed in 75617ms.
markdown-js completed in 70069ms.
```

node v0.6.x

``` bash
$ node test --bench
markcom completed in 6448ms.
markcom (gfm) completed in 7357ms.
markcom (pedantic) completed in 6092ms.
discount completed in 7314ms.
showdown (reuse converter) completed in 16018ms.
showdown (new converter) completed in 18234ms.
markdown-js completed in 24270ms.
```

__markcom is now faster than Discount, which is written in C.__

For those feeling skeptical: These benchmarks run the entire markdown test suite
1000 times. The test suite tests every feature. It doesn't cater to specific
aspects.

## Install

``` bash
$ npm install markcom
```

## Another Javascript Markdown Parser

The point of markcom was to create a markdown compiler where it was possible to
frequently parse huge chunks of markdown without having to worry about
caching the compiled output somehow...or blocking for an unnecesarily long time.

markcom is very concise and still implements all markdown features. It is also
now fully compatible with the client-side.

markcom more or less passes the official markdown test suite in its
entirety. This is important because a surprising number of markdown compilers
cannot pass more than a few tests. It was very difficult to get markcom as
compliant as it is. It could have cut corners in several areas for the sake
of performance, but did not in order to be exactly what you expect in terms
of a markdown rendering. In fact, this is why markcom could be considered at a
disadvantage in the benchmarks above.

Along with implementing every markdown feature, markcom also implements
[GFM features](http://github.github.com/github-flavored-markdown/).

## Options

markcom has a few different switches which change behavior.

- __pedantic__: Conform to obscure parts of `markdown.pl` as much as possible.
  Don't fix any of the original markdown bugs or poor behavior.
- __gfm__: Enable github flavored markdown (enabled by default).
- __sanitize__: Sanitize the output. Ignore any HTML that has been input.
- __highlight__: A callback to highlight code blocks.
- __tables__: Enable GFM tables. This is enabled by default. (Requires the
  `gfm` option in order to be enabled).
- __breaks__: Enable GFM line breaks. Disabled by default.
- __smartLists__: Use smarter list behavior than the original markdown.
  Disabled by default. May eventually be default with the old behavior
  moved into `pedantic`.
- __langPrefix__: Set the prefix for code block classes. Defaults to `lang-`.

## Usage

``` js
// Set default options
markcom.setOptions({
  gfm: true,
  tables: true,
  breaks: false,
  pedantic: false,
  sanitize: true,
  smartLists: true,
  langPrefix: 'language-',
  highlight: function(code, lang) {
    if (lang === 'js') {
      return highlighter.javascript(code);
    }
    return code;
  }
});
console.log(markcom('i am using __markdown__.'));
```

You also have direct access to the lexer and parser if you so desire.

``` js
var tokens = markcom.lexer(text, options);
console.log(markcom.parser(tokens));
```

``` js
var lexer = new markcom.Lexer(options);
var tokens = lexer.lex(text);
console.log(tokens);
console.log(lexer.rules);
```

``` bash
$ node
> require('markcom').lexer('> i am using markcom.')
[ { type: 'blockquote_start' },
  { type: 'paragraph',
    text: 'i am using markcom.' },
  { type: 'blockquote_end' },
  links: {} ]
```

## CLI

``` bash
$ markcom -o hello.html
hello world
^D
$ cat hello.html
<p>hello world</p>
```

## License

Copyright (c) 2011-2013, Christopher Jeffrey. (MIT License)
		
Copyright (c) 2013 partly, Ilkka Huotari. (MIT License)

See LICENSE for more info.
