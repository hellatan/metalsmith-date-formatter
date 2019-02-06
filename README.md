# Metalsmith Date Formatter

[![Build Status](https://travis-ci.org/hellatan/metalsmith-date-formatter.svg?branch=master)](https://travis-ci.org/hellatan/metalsmith-date-formatter)

Format article/post dates based on [YAML Front Matter (YFM)](http://jekyllrb.com/docs/frontmatter/) data and [moment](http://momentjs.com/)

 ## Installation
 
 ```bash
 npm install --save-dev metalsmith-date-formatter
 ```

## Usage

Front Matter:
```markdown
---
publishDate: 2015-05-30
modifedDate: 2015-05-31
---
```

JavaScript API:

```js
var Metalsmith = require('metalsmith');
var dateFormatter = require('metalsmith-date-formatter');

Metalsmith()
    .use(dateFormatter());
```

In your template::
```html
<p>published: {{ publishDate }}</p>
<p>last modified: {{ modifiedDate }}</p>
```

## options

### dates

This option takes multiple formats

array of objects with `key` and `format` properties. 

- The `key` property is the YFM property name
- The `format` property is optional but takes any moment format value

```js
.use(dateFormatter({
    dates: [
        {
            key: 'publishDate',
            format: 'MM DD YYYY'
        },
        {
            key: 'modifiedDate',
            format: 'MM YYYY'
        }
    ]
})
```

array of strings

```js
.use(dateFormatter({
    dates: ['publishDate', 'modifiedDate']
})
```

string

```js
.use(dateFormatter({
    dates: 'publishDate'
})
```

### create multiple formats from a single date

By default the formatted date is output with the same key as the original date (so the original date gets replaced).

It can be useful to output a single date in more than one format. This can be achieved by:
- using the *array of objects* format for `dates`
- adding copies of date objects which include an `out_key` property

The following example re-uses the same `date` and outputs it in 2 formats named `dateNice`, `dateYear`.
```js
.use(dateFormatter({
    dates: [
        {
            key: 'date',
            format: 'MMMM DD, YYYY',
            out_key: 'dateNice'
        }, {
            key: 'date',
            format: 'YYYY',
            out_key: 'dateYear'
        }
    ]
})
```
(`out_key` can also be used to rename a date)

### format

Any date format that `moment` accepts, defaults to `MMMM DD, YYYY`

## Notes
 
The metalsmith cli workflow has not been tested

