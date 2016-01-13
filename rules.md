### Rules

#### 解决解析错误

CSS lint首要关心的是CSS文件中没有解析错误，CSS解析错误意味着CSS代码的不合法，CSS解析错误通常会导致浏览器无法解析某一元素标签的整个样式规则或者无法解析样式规则中某一条属性。解析错误通常也会被CSSLint视为最应该被修复的错误。

#### 一、Possible Errors

下面的规则可能会检测出CSS文件中潜在的错误

1.1 [Beware of box model size](https://github.com/CSSLint/csslint/wiki/Beware-of-box-model-size)

##### 规则详情

规则ID: `box-model`

该规则的目的用于消除盒子模型可能导致的潜在错误，正如这条规则会在如下情境下发出警告：

1. `width`属性和 `border`, `border-left`, `border-right`, `padding`, `padding-left`, `padding-right`同时使用
2. `height`属性和 `border`, `border-top`, `border-bottom`, `padding`, `padding-top`, `padding-bottom`属性同时使用

但是当使用了`box-sizing`属性的时候，及时出现了上面两种情况，该条规则也不会发出警告，CSSLint会认为你已经准确得理解了盒子模型。

下面的CSS样式将被视为警告：

``` css
/* width and border */
.mybox {
    border: 1px solid black;
    width: 100px;
}

/* height and padding */
.mybox {
    height: 100px;
    padding: 10px;
}
```

而下面的CSS样式不会引起警告：

``` css
/* width and border with box-sizing */
.mybox {
    box-sizing: border-box;
    border: 1px solid black;
    width: 100px;
}

/* width and border-top */
.mybox {
    border-top: 1px solid black;
    width: 100px;
}

/* height and border-top of none */
.mybox {
    border-top: none;
    height: 100px;
}

```

如果你想了解更多关于盒子模型的信息，请参考如下链接：

* [The CSS Box Model](http://css-tricks.com/2841-the-css-box-model/)


* [Understanding the Box Model](http://blog.simonwillison.net/post/58225400497/theboxmodel)



1.2 [Require properties appropriate for display](https://github.com/CSSLint/csslint/wiki/Require-properties-appropriate-for-display)

##### 规则详情

规则ID：`display-property-grouping`

在CSS样式中，当`display`属性使用时，一些属性样式在和`display`同时使用时将无法生效，该条规则的目的就是为了消除这些不生效的样式属性，使得CSS文件更加易懂和简洁，当检测到如下样式规则时，csslint将会发出警告：

1. `display: inline` 和`width`, `height`, `margin`, `margin-top`, `margin-bottom`, `float`同时使用
2. `display: inline` 和`float`同时使用
3. `display: block` 和`vertical-align` 同时使用
4. `display: table-*` 和 `margin` `float`同时使用的时候

下面的样式将被警告

``` css
/* inline with height */
.mybox {
    display: inline;
    height: 25px;
}

/* inline-block with float */
.mybox {
    display: inline-block;
    float: left;
}

/* table-cell and margin */
.mybox {
    display: table-cell;
    margin: 10px;
}
```

而下面的样式将被视为合法

``` css
/* inline with margin-left */
.mybox {
    display: inline;
    margin-left: 10px;
}

/* table and margin */
.mybox {
    display: table;
    margin-bottom: 10px;
}

```



1.3 [Disallow duplicate properties](https://github.com/CSSLint/csslint/wiki/Disallow-duplicate-properties)

##### 规则详情

规则ID：duplicate-properties

该条规则的目的就是用来消除CSS样式中的重复声明的样式。当CSSLint检测到如下样式将发出警告：

1. 同一样式属性被定义了两次同时使用了相同的样式属性值。
2. 同一样式属性被定义了两次，这两个相同的样式被其他样式属性分开。

举个栗子，下面的样式将被警告

``` css
/* properties with the same value */
.mybox {
    border: 1px solid black;
    border: 1px solid black;
}

/* properties separated by another property */
.mybox {
    border: 1px solid black;
    color: green;
    border: 1px solid red;
}
```

而下面的样式将被视为合法

``` css
/* one after another with different values */
.mybox {
    border: 1px solid black;
    border: 1px solid red;
}
```



1.4 [Disallow empty rules](https://github.com/CSSLint/csslint/wiki/Disallow-empty-rules)

#### 规则详情

规则ID：`empty-rules`

该条规则的目的就是用以消除空规则（没有样式属性的规则），下面的规则都是空规则。

``` css
.mybox { }

.mybox {

}

.mybox {
    /* a comment */
}
```



1.5 [Require use of known properties](https://github.com/CSSLint/csslint/wiki/Require-use-of-known-properties)

规则详情

规则ID：know-properties

该条规则会检测CSS样式中的每条属性，保证每条属性都是合法的样式属性。CSS样式属性是CSS解析器维护的一部分，该条规则使用了解析器中关于样式属性相关信息来决定该样式属性是否是合法的。该样式属性也会随着CSS样式的发展而不断更新，该条样式规则的目的就是减少拼写错误的发生。

该样式规则会忽略所有的浏览器前缀，除此之外，该条规则不仅检测样式属性，还检测属性值是否合法或为空，该规则保证了大部分样式属性值的合法性。

下面的栗子将被视为不合法：

``` css
/* clr isn't a known property */
a {
    clr: red;
}

/* 'foo' isn't a valid color */
a {
    color: foo;
}
```

而下面的样式将不会被视为错误：

``` css
/* -moz- is a vendor prefix, so ignore */
a {
    -moz-foo: bar;
}
```





#### 二、Compatibility

2.1 [Disallow adjoining classes](https://github.com/CSSLint/csslint/wiki/Disallow-adjoining-classes)

规则ID：`adjoining-classes`

该条规则用于消除class选择器的连用情况：举个栗子下面的样式将被视为不合法：

``` css
.foo.bar {
    border: 1px solid black;
}

.first .abc.def {
    color: red;
}
```

而下面的样式将被视为合法

``` css
/* space in between classes */
.foo .bar {
    border: 1px solid black;
}

```

**该条规则主要是针对IE6及更早的IE浏览器的，因此可以无视。

2.2 [Disallow box-sizing](https://github.com/CSSLint/csslint/wiki/Disallow-box-sizing)

##### 规则详情

规则ID：`box-sizing`

当使用`box-sizing`时，该条规则将会发出警告,举个栗子，下面的代码将被视为不合规则

``` css
.mybox {
    box-sizing: border-box;
}

.mybox {
    box-sizing: content-box;
}
```



2.3 [Require compatible vendor prefixes](https://github.com/CSSLint/csslint/wiki/Require-compatible-vendor-prefixes)

##### 规则详情

规则ID：`compatible-vendor-prefixes`

当需要加浏览器前缀的时候，却没有加浏览器前缀，该条规则将会警告，(需要加上所有的浏览器前缀)需要加浏览器前缀的属性有：

- `animation`
- `animation-delay`
- `animation-direction`
- `animation-duration`
- `animation-fill-mode`
- `animation-iteration-count`
- `animation-name`
- `animation-play-state`
- `animation-timing-function`
- `appearance`
- `border-end`
- `border-end-color`
- `border-end-style`
- `border-end-width`
- `border-image`
- `border-radius`
- `border-start`
- `border-start-color`
- `border-start-style`
- `border-start-width`
- `box-align`
- `box-direction`
- `box-flex`
- `box-lines`
- `box-ordinal-group`
- `box-orient`
- `box-pack`
- `box-sizing`
- `box-shadow`
- `column-count`
- `column-gap`
- `column-rule`
- `column-rule-color`
- `column-rule-style`
- `column-rule-width`
- `column-width`
- `hyphens`
- `line-break`
- `margin-end`
- `margin-start`
- `marquee-speed`
- `marquee-style`
- `padding-end`
- `padding-start`
- `tab-size`
- `text-size-adjust`
- `transform`
- `transform-origin`
- `transition`
- `transition-delay`
- `transition-duration`
- `transition-property`
- `transition-timing-function`
- `user-modify`
- `user-select`
- `word-break`
- `writing-mode`

举个栗子，下面的代码将被视为不合规则

``` css
/* Missing -moz, -ms, and -o */
.mybox {
    -webkit-transform: translate(50px, 100px);
}

/* Missing -webkit */
.mybox {
    -moz-border-radius: 5px;
}
```



2.4 [Require all gradient definitions](https://github.com/CSSLint/csslint/wiki/Require-all-gradient-definitions)

#### 规则详情

规则ID：`gradients`

 当gradient加了一个浏览器前缀时，那么`gradient`也应该把其他浏览器前缀都加上，该条规则的目的就在于保证了`gradient`加上所有浏览器前缀，举个栗子，下面的代码将被视为不合法：

``` css
/* Missing -moz, -ms, and -o */
.mybox {
    background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,#1e5799), color-stop(100%,#7db9e8));
    background: -webkit-linear-gradient(top,  #1e5799 0%,#7db9e8 100%);
}

/* Missing old and new -webkit */
.mybox {
    background: -moz-linear-gradient(top,  #1e5799 0%, #7db9e8 100%); 
    background: -o-linear-gradient(top,  #1e5799 0%,#7db9e8 100%);
    background: -ms-linear-gradient(top,  #1e5799 0%,#7db9e8 100%);
}
```

而下面的代码将被视为合法的

``` css
.mybox {
    background: -moz-linear-gradient(top,  #1e5799 0%, #7db9e8 100%);
    background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,#1e5799), color-stop(100%,#7db9e8));
    background: -webkit-linear-gradient(top,  #1e5799 0%,#7db9e8 100%);
    background: -o-linear-gradient(top,  #1e5799 0%,#7db9e8 100%);
    background: -ms-linear-gradient(top,  #1e5799 0%,#7db9e8 100%); 
}
```



2.5 [Disallow negative text-indent](https://github.com/CSSLint/csslint/wiki/Disallow-negative-text-indent)

##### 规则详情

规则ID：`text-indent`

该条规则的目的就是检测出CSS 代码中可能错用`text-indent`的情景，当`text-indent`的值为-99或相似时（i.e,-100，-999，etc），并且没有和`direction: ltr`同时使用时，该规则将会发出警告：

举个栗子，下面的代码将被视为错误：

``` css
/* missing direction */
.mybox {
    text-indent: -999px;
}

/* missing direction */
.mybox {
    text-indent: -999em;
}

/* direction is rtl */
.mybox {
    direction: rtl;
    text-indent: -999em;
}
```

而下面的代码将不被视为错误：

``` css
/* direction used */
.mybox {
    direction: ltr;
    text-indent: -999em;
}

/* Not obviously used to hide text */
.mybox {
    text-indent: -1em;
}
```

##### 延伸阅读

- [CSS text-indent: An Excellent Trick To Style Your HTML Form](http://aext.net/2010/02/css-text-indent-style-your-html-form/)
- [Stop Using the text-indent: -9999px Trick](http://luigimontanez.com/2010/stop-using-text-indent-css-trick/)
- [CSS Image Replacement Museum](http://css-tricks.com/examples/ImageReplacement/)



2.6 [Require standard property with vendor prefix](https://github.com/CSSLint/csslint/wiki/Require-standard-property-with-vendor-prefix)

##### 规则详情

规则ID: `vendor-prefix`

该条规则保证了在使用浏览器前缀属性的同时也需要包含标准的属性，出现如下情况下，该条规则将发出警告：

1. 在使用了浏览器前缀属性时没有添加相同属性的标准写法。
2. 标注写法写在了浏览器前缀属性的前面。

举个栗子，下面的代码将被视为错误：

``` css
/* missing standard property */
.mybox {
    -moz-border-radius: 5px;
}

/* standard property should come after vendor-prefixed property */
.mybox {
    border-radius: 5px;
    -webkit-border-radius: 5px;
}
```

而下面的代码将不被视为错误：

``` css
/* both vendor-prefix and standard property */
.mybox {
    -moz-border-radius: 5px;
    border-radius: 5px;
}
```

##### 延伸阅读

- [Remember non-vendor-prefixed CSS 3 properties (and put them last)](http://www.456bereastreet.com/archive/201009/remember_non-vendor-prefixed_css_3_properties_and_put_them_last/)



2.7 [Require fallback colors](https://github.com/CSSLint/csslint/wiki/Require-fallback-colors)

##### 规则详情

规则ID：`fallback-colors`

该条规则的目的就是保证颜色表示法在各个浏览器中都能够正确显示，当初下下面情景时，该规则将发出警告：

当`color` `background` `background-color`样式属性使用了`reba()` ` hsl()` ``hsla()` 颜色表示法时候，而没有使用一个传统的颜色表示法，将会报错。举个栗子，下面的代码将被视为不合法：

``` css
/* missing fallback color */
.mybox {
    color: rgba(100, 200, 100, 0.5);
}

/* missing fallback color */
.mybox {
    background-color: hsla(100, 50%, 100%, 0.5);
}

/* missing fallback color */
.mybox {
    background: hsla(100, 50%, 100%, 0.5) url(foo.png);
}

/* fallback color should be before */
.mybox {
    background-color: hsl(100, 50%, 100%);
    background-color: green;
}
```

而下面的代码将被视为合法：

``` css
/* fallback color before newer format */
.mybox {
    color: red;
    color: rgba(255, 0, 0, 0.5);
}
```



2.8 [Disallow star hack](https://github.com/CSSLint/csslint/wiki/Disallow-star-hack)

##### 规则详情

规则ID:`star-property-hack`

该条规则的目的用于消除在属性名前面使用星号这样的hack CSS样式，当出现下面情况下，该条规则将发出警告：

``` css
.mybox {
    border: 1px solid black;
    *width: 100px;
}
```



2.9[Disallow underscore hack](https://github.com/CSSLint/csslint/wiki/Disallow-underscore-hack)

##### 规则详情

规则ID：`underscore-property-hack`

该规则的目的在于消除使用下划线的hack CSS样式，当csslint检测到样式属性名前面带有下划线的时候，就会发出警告。

举个栗子，下面的代码将被视为错误：

``` css
.mybox {
    border: 1px solid black;
    _width: 100px;
}
```

##### 延伸阅读

- [Underscore Hack](http://en.wikipedia.org/wiki/CSS_filter#Underscore_hack)
- [Avoiding CSS Hacks for Internet Explorer](http://24ways.org/2005/avoiding-css-hacks-for-internet-explorer)



2.10 [Bulletproof font-face](https://github.com/CSSLint/csslint/wiki/Bulletproof-font-face) (new in v0.9.10)

#### 规则详情

规则ID: `bulletproof-font-face`

当使用`@font-face` 声明不用样式的字体来跨浏览器兼容时，在老的IE浏览器下可能会出现404错误，这是由于IE浏览器解析字体所造成的。例如：如下格式将会导致IE6,7,8出现404错误。

``` css
@font-face {
    font-family: 'MyFontFamily';
    src: url('myfont-webfont.eot') format('embedded-opentype'), 
        url('myfont-webfont.woff') format('woff'), 
        url('myfont-webfont.ttf')  format('truetype'),
        url('myfont-webfont.svg#svgFontName') format('svg');
}
```

解决的方法就是在第一个url后面添加一个问号，紧跟`iefix` .这样IE浏览器就会把添加的字符串当做请求字符串识别。

``` css
@font-face {
    font-family: 'MyFontFamily';
    src: url('myfont-webfont.eot?#iefix') format('embedded-opentype'), 
        url('myfont-webfont.woff') format('woff'), 
        url('myfont-webfont.ttf')  format('truetype'),
        url('myfont-webfont.svg#svgFontName') format('svg');
}
```

这条规则的目的就在于消除早期IE浏览器在解析web font URLs时出现的404错误。

##### 延伸阅读

- [The Fontspring Bulletproof @font-face syntax](http://www.fontspring.com/blog/the-new-bulletproof-font-face-syntax)



#### 三、Performance

下面的规则主要用于提升CSS样式性能，包括运行性能和样式文件的大小。

3.1 [Don't use too many web fonts](https://github.com/CSSLint/csslint/wiki/Don%27t-use-too-many-web-fonts)

##### 规则详情

规则ID: `font-faces`

Web字体现在使用的人越来越多，然而由于web 字体文件通常比较大，往往会影响网页性能，一些浏览器甚至当下载字体的时候会阻塞网页渲染，由于这些原因，CSSLint在一份样式表中超过5份Web字体时会发出警告。

3.2 [Disallow @import](https://github.com/CSSLint/csslint/wiki/Disallow-%40import)

`@import`命令同在在CSS文件中引入其他CSS文件。如下：

``` css
@import url(more.css);
@import url(andmore.css);

a {
    color: black;
}
```

上面的样式中应用了两个样式表，当浏览器解析到`@import`的时候，会下载import的文件，在现在import文件的时候，浏览器不会并行下载其他样式文件，也就是说，使用@import的时候，浏览器无法并行下载CSS样式表。

`@import`有两种替代方案

1. 通过构建系统把相关联的CSS文件打包使用
2. 通过多个`\<link>`标签加载多份样式表，这样浏览器可以并行下载多份样式表。

规则ID：`import`

该规则在我们使用了`import`的时候会发出警告。举个栗子，下面的代码将会发出警告：

``` css
@import url(foo.css);
```

##### 延伸阅读

- [Don't use @import](http://www.stevesouders.com/blog/2009/04/09/dont-use-import/)



3.3 [Disallow selectors that look like regular expressions](https://github.com/CSSLint/csslint/wiki/Disallow-selectors-that-look-like-regular-expressions)

##### 规则详情

规则ID:`regex-selectors`

该规则的目的用于消除选择器带来的潜在性能影响。正如，当在选择器中发现`*=` `|=` `^=` `$=` `~=`时，该规则会发出警告

下面的栗子将会引起警告：

``` css
.mybox[class~=xxx]{
    color: red;
}

.mybox[class^=xxx]{
    color: red;
}

.mybox[class|=xxx]{
    color: red;
}

.mybox[class$=xxx]{
    color: red;
}

.mybox[class*=xxx]{
    color: red;
}

```

而下面的例子，在该规则下是合法的：

``` css
.mybox[class=xxx]{
    color: red;
}

.mybox[class]{
    color: red;
}

```

##### 延伸阅读

- [The Skinny on CSS Attribute Selectors](http://css-tricks.com/5591-attribute-selectors/)



3.4 [Disallow universal selector](https://github.com/CSSLint/csslint/wiki/Disallow-universal-selector)

##### 规则详情

规则ID：`universal-selector`

该规则的目的在于消除在使用通配符（*）所带来的性能影响。当在选择器关键部分发现通配符时，该规则会发出警告：

举个栗子，下面的代码将会被警告：

``` css
* {
    color: red;
}

.selected * {
    color: red;
}
```

而下面的代码将不会引起警告：

``` css
/* universal selector is not key */
.selected * a {
    color: red;
}
```



3.5 [Disallow unqualified attribute selectors](https://github.com/CSSLint/csslint/wiki/Disallow-unqualified-attribute-selectors)

##### 规则详情

规则ID:`unqualified-attributes`

该规则的目的在于消除使用不受限制的属性选择器所带来的性能影响，当在选择器关键部分(译者注：选择器最右边的子选择器)使用了不受限制的属性选择器时，将会发出警告,举个栗子，下面的代码将会被视为不合法的：

``` css
[type=text] {
    color: red;
}

.selected [type=text] {
    color: red;
}
```

而下面的代码将被视为可行。

``` css
/* unqualified attribute selector is not key */
.selected [type=text] a {
    color: red;
}
```



3.6 [Disallow units for zero values](https://github.com/CSSLint/csslint/wiki/Disallow-units-for-zero-values)

##### 规则详情

规则ID：`zero-units`

当样式属性值为0时，如果还是用单位，将会报错，当属性值为0时不适用单位的原因在于可以缩减代码量。举个栗子，下面的代码将被视为不合法：

``` css
.mybox {
    margin: 0px;
}

.mybox {
    width: 0%;
}

.mybox {
    padding: 10px 0px;
}
```

而下面的代码将被视为合法：

``` css
.mybox {
    margin: 0;
}

.mybox {
    padding: 10px 0;
}
```



3.7 [Disallow overqualified elements](https://github.com/CSSLint/csslint/wiki/Disallow-overqualified-elements)

##### 规则详情

规则ID:`overqualified-element`

该条规则的目的在于消除对选择器的不必要的限制，来减少字节量，同时也会带来一定的性能提升。当元素名和类选择器同时使用时会发出警告，而当不同的标签元素拥有相同的类名时，将不会发出警告。举个栗子，下面的代码将会被警告：

``` css
div.mybox {
    color: red;   
}

.mybox li.active {
    background: red;
}
```

而下面的代码将不会被警告

``` css
/* Two different elements in different rules with the same class */
li.active {
    color: red;
}

p.active {
    color: green;
}
```



3.8 [Require shorthand properties](https://github.com/CSSLint/csslint/wiki/Require-shorthand-properties)

##### 规则详情

规则ID:`shorthand`

该规则会检测样式表中多条样式属性可以写成一条的情况，然后发出警告，目的在于减少文件大小，提升代码可读性。

1. 当 `margin-left`, `margin-right`, `margin-top`, 和 `margin-bottom` 同时使用的时候会被警告。
2. 当 `padding-left`, `padding-right`, `padding-top`, 和 `padding-bottom` 同时使用的时候会被警告。

举个栗子，下面的代码会被警告：

``` css
.mybox {
    margin-left: 10px;
    margin-right: 10px;
    margin-top: 20px;
    margin-bottom: 30px;
}

.mybox {
    padding-left: 10px;
    padding-right: 10px;
    padding-top: 20px;
    padding-bottom: 30px;
}
```

而下面的代码将不会被警告：

``` css
/* only two margin properties*/
.mybox {
    margin-left: 10px;
    margin-right: 10px;

}

/* only two padding properties */
.mybox {
    padding-right: 10px;
    padding-top: 20px;
}
```



3.9 [Disallow duplicate background images](https://github.com/CSSLint/csslint/wiki/Disallow-duplicate-background-images)

##### 规则详情

规则ID:`duplicate-background-images`

在写样式表时有一条首要的原则就是用尽量少的代码来完成工作，根据这条原则，在一份CSS文件中，同一url尽量只使用一次。如果有不同的元素同时使用一张图片作为背景图，那么就像这些元素添加一个相同的类名，然后通过`background-position`来控制背景图片位置。

该条规则在检测到样式表内具有相同的url时，会发出警告。

举个栗子，下面的代码将会被警告。

``` css
/* multiple instances of the same URL */
.heart-icon {
    background: url(sprite.png) -16px 0 no-repeat;
}

.task-icon {
    background: url(sprite.png) -32px 0 no-repeat;
}
```

而下面的代码将不会被警告：

``` css
/* single instance of URL */
.icons {
    background: url(sprite.png) no-repeat;
}

.heart-icon {
    background-position: -16px 0;
}

.task-icon {
    background-position: -32px 0;
}
```



#### 四、Maintainability & Duplication

4.1 [Disallow too many floats](https://github.com/CSSLint/csslint/wiki/Disallow-too-many-floats)

规则ID：`floats`

当在一份样式表中使用超过10次`float`时将会发出警告

4.2 [Don't use too many font-size declarations](https://github.com/CSSLint/csslint/wiki/Don%27t-use-too-many-font-size-declarations)

规则ID：`font-size`

当一份样式表中`font-size`使用超过10（包括10次）将会发出警告

4.3 [Disallow IDs in selectors](https://github.com/CSSLint/csslint/wiki/Disallow-IDs-in-selectors)

规则ID：`ids`

该条规则禁止在选择器中使用ID选择器。举个栗子，下面的代码将被视为不合法：

``` css
#mybox {
    display: block;
}

.mybox #go {
    color: red;
}
```

##### 延伸阅读

- [Don't use IDs in CSS selectors?](http://oli.jp/2011/ids/)
- [Don't use ID selectors in CSS](http://screwlewse.com/2010/07/dont-use-id-selectors-in-css/)



4.4 [Disallow !important](https://github.com/CSSLint/csslint/wiki/Disallow-%21important)

规则ID：`important`

该条规则在样式表中使用了`!important`的时候会发出警告,举个栗子，下面的代码将会被警告

``` css
.mybox {
    color: red !important;
}

```



#### 五、Accessibility

5.1 [Disallow outline:none](https://github.com/CSSLint/csslint/wiki/Disallow-outline%3Anone)

规则ID:`outline-none`

该条规则的目的在于使得只能够使用键盘的用户在元素获取焦点是获取到一个视觉提示。正因为如此，当出现以下情况时会发出警告：

1. 当`outline: none·` 或者`outline: 0` 出现在一个不带有`:focus`伪选择器时。
2. 即使`outline:none` 和`outline: 0 `在一个带有`:focus`伪选择器中，但是该选择器规则不包含其他样式属性。

举个栗子，下面的代码将被视为不合法：

``` css
/* no :focus */
a {
    outline: none;
}

/* no :focus */
a {
    outline: 0;
}

/* :focus but missing a replacement treatment */
a:focus {
    outline: 0;
}
```

而下面的代码将被视为合法：

``` css
/* :focus with outline: 0 and provides replacement treatment */
a:focus {
    border: 1px solid red;
    outline: 0;
}
```

##### 延伸阅读

- [Removing the dotted outline](http://css-tricks.com/829-removing-the-dotted-outline/)
- [http://outlinenone.com/](http://outlinenone.com/)



#### 六、OOCSS

6.1 [Disallow qualified headings](https://github.com/CSSLint/csslint/wiki/Disallow-qualified-headings)

规则ID:`qualified-headings`

标题元素（`h1 - h6`）应该定义在顶级样式中（译者注：作者意思应该写在选择器最左边），而不应该嵌套在其他元素内定义样式，因为在面向对象CSS样式中，标题元素被视为原生对象，在整个网站中，相同标题元素的样式应该是一样的，这使得标题元素高度可复用，并且保持视觉上的一致性和代码可维护性。举个栗子，下面的代码将会重新定义`.foo`内部的`h1`标签样式，而导致`h1`标签在整个页面中的样式不一致。

``` css
.foo h1 {
    font-size: 110%;
}
```

该规则的目的在于检测标题元素选择器出现在选择器的最右边这种对标题元素样式重写的情况，然后发出警告，举个栗子，下面的代码将会被警告：

``` css
/* qualified heading */
.box h3 {
    font-weight: normal;
}

/* qualified heading */
.item:hover h3 {
    font-weight: bold;
}
```

而下面的代码将被视为合法

``` css
/* Not qualified */
h3 {
    font-weight: normal;
}
```



6.2 [Headings should only be defined once](https://github.com/CSSLint/csslint/wiki/Headings-should-only-be-defined-once)

规则ID:`unique-headings`

该条规则的目的用于消除重复定义标题样式，当向同一个标题（h1 - h6）定义多个样式规则时会报错。

举个栗子：下面的代码将报错：

``` css
/* Two rules for h3 */
h3 {
    font-weight: normal;
}

.box h3 {
    font-weight: bold;
}
```

而下面的样式不会报错

``` css
/* :hover doesn't count */
h3 {
    font-weight: normal;
}

h3:hover {
    font-weight: bold;
}
```

