RTLCSS for PHP
--------------

RTLCSS is a framework for converting Left-To-Right (LTR) Cascading Style Sheets(CSS) to Right-To-Left (RTL).

## Usage

### Installation using composer

Add the library to your composer.json

```json
{
    "repositories": [{
        "type": "vcs",
        "url": "https://github.com/moodlehq/rtlcss-php"
    }],
    "require-dev": {
        "moodlehq/rtlcss-php": "*"
    }
}
```

### Flipping

```php
$parser = new Sabberworm\CSS\Parser($css);
$tree = $parser->parse()
$rtlcss = new MoodleHQ\RTLCSS\RTLCSS($tree);
$rtlcss->flip();
echo $tree->render();
```

For parsing options and rendering, refer to [Sabeerword/PHP-CSS-Parser](https://github.com/sabberworm/PHP-CSS-Parser).

## Output sample

```css
.div {
    direction: ltr;
    left: 10px;
    border: 10px 5px 0px 2px;
    float: left;
}
```

Becomes:

```css
.div {
    direction: rtl;
    right: 10px;
    border: 10px 2px 0px 5px;
    float: right;
}
```

## Options

You can prefix your CSS with comments starting with `/*rtl:*/` for special handling.

### Do not flip values

Prepend with `/*rtl:ignore*/`, or wrap within `/*rtl:begin:ignore*/` and `/*rtl:end:ignore*/`.

```css
.div {
    /*rtl:ignore*/
    float: left;
    left: 10px;
}
.div {
    direction: ltr;
    /*rtl:begin:ignore*/
    float: left;
    left: 10px;
    /*rtl:end:ignore*/
}
```

Becomes:

```css
.div {
    float: left;
    right: 10px;
}
.div {
    direction: rtl;
    float: left;
    left: 10px;
}
```

### Remove CSS

Prepend with `/*rtl:remove*/`, wrap within `/*rtl:begin:remove*/` and `/*rtl:end:remove*/`.

```css
.div {
    /*rtl:remove*/
    float: left;
    left: 10px;
}
.div {
    direction: ltr;
    /*rtl:begin:remove*/
    float: left;
    left: 10px;
    /*rtl:end:remove*/
}
```

Becomes:

```css
.div {
    right: 10px;
}
.div {
    direction: rtl;
}
```

### Additional CSS

Write the CSS in a content starting with `/*rtl:raw:`.

```css
.div {
    /*rtl:raw:
        text-align: left;
    */
    float: left;
}
```

Becomes:

```css
.div {
    text-align: left;
    float: right;
}
```

## About this tool

This tool is very heavily inspired by [MohammaYounes/rtlcss](https://github.com/MohammadYounes/rtlcss), even though at this stage it does not include all of its features. See this library as a partial port of the latter.

## Credits

* [MohammaYounes/rtlcss](https://github.com/MohammadYounes/rtlcss) for being the example we followed.
* [Sabeerword/PHP-CSS-Parser](https://github.com/sabberworm/PHP-CSS-Parser) for parsing CSS in PHP.

## License

Licensed under the [GNU GPL License](http://www.gnu.org/copyleft/gpl.html).
