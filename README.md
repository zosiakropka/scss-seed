Multiplatform SCSS
==================

Repository providing me with a neat way to create mulitplatform SCSS-based
stylesheets. No CSS is generated by the framework itself. It only decides on
how to render all the variations of the stylesheets. The CSS that would not
be understood anyway is not rendered at all, so that CSS size is fairly
reduced.

1. Structure
------------

Various platform require specific stylesheet files. With this framework it's
easy to create separate CSS files without the code duplication.

For each outputted stylesheet a separate ``<name>.scss`` file is required,
that is not prefixed with ``_`` (underscore) character. Those files should
contain nothing but imports and (optionally) setup constans.

The common part should be stored within a ``_`` (underscore)-prefixed file,
and be imported by all the not-prefixed SCSS files.

There are four platform-specific layout setups, that are ready to inculde out
of the box:

- Fixed layout layout (``@import 'multiplatform/fixed'``),
- Responsive layout (``@import 'multiplatform/responsive'``),
- IE8 layout (``@import 'multiplatform/ie8'``),
- Mobile layout (``@import 'multiplatform/fixed'``).

The great thing is you don't need to decide on a single layout - you may create
one, some or all of them, whatever you require. Your stylesheet will get
generated in a way specific for that setup.

2. Layouts
----------

### 1. Responsive layout

```scss
/*--- scss/responsive.scss ---*/

$desktop-width: 1024px;
// other constants
@import '<multiplatform_path>/responsive';
@import 'common';
```

Responsive layout setup contains no fallback for the platforms not supporting
media queries. The output stylesheet just concludes support for the media
queries.

Output stylesheet might be linked as:

```html
<!--[if (gt IE 8) | (IEMobile)]><!-->
	<link rel="stylesheet" href="css/responsive.css" />
<!--<![endif]-->
```

### 2. Fixed layout

```scss
/*--- scss/fixed.scss ---*/

$desktop-width: 1024px;
// other constants
@import '<multiplatform_path>/fixed';
@import 'common';
```

With the fixed layout all the included CSS is outputted outside of the media
queries. Only the queries that target a viewport with the width of value
``$desktop-width`` are included. The implementation of the fixed-layout CSS is
upon the developer. The only thing to keep in mind is that statements are
interpreted in the very way they are included (unlike when media queries are
evaluated and so that proritized).

To clarify, only ``min-width`` values that are smaller than or equal to
``$desktop-width`` and ``max-width`` values that are bigger than or equal to
``$desktop-width`` are included. Only ``width`` of value ``$desktop-width``
is included.

### 3. IE8 layout

```scss
/*--- scss/ie8.scss ---*/

$desktop-width: 1024px;
// other constants
@import '<multiplatform_path>/ie8';
@import 'common';
```

This layout is nearly the same as the fixed layout, however it provides one
with a rem() fallback function. Read more in the **3.1 @function rem()**
section.

### 3. Mobile layout

```scss
/*--- scss/mobile.scss ---*/

$desktop-width: 1024px;
// other constants
@import '<multiplatform_path>/mobile';
@import 'common';
```

This layout does not create CSS for viewports wider than $desktop-width.
Otherwise it's responsive.

3. Utils
--------

### 1. @function rem()

The framework comes with a handy ``rem()`` function. It allows one to use
``rem`` units and fallback to ``px`` on IE lt 9 platforms.

``$font-base-size`` should be defined proir to using ``rem()`` function. Body
font size should be set to ``$font-base-size`` value. There is no need to
redefine ``$font-base-size`` if it's already been defined in order to setup
Compass vertical rythm.

Example:

```
/*--- usage: ---*/
padding: rem(2) rem(1);

/*--- output for responsive.css, fixed.css, mobile.css ---*/
padding: 2rem 1rem;

/*--- output for ie8.css ---*/
padding: 32px 16px;
```

Media queries
-------------

