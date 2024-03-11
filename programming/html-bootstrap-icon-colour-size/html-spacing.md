---
description: margin or padding
---

# Html spacing

The classes are named using the format `{property}{sides}-{size}` for `xs` and \
`{`<mark style="background-color:blue;">**`property`**</mark>`}{`**`sides`**`}-{`**`breakpoint`**`}-{`**`size`**`}` for **`sm`, `md`, `lg`, and `xl`**.

\
&#x20;   **example:**\
&#x20;        class="mb-2"\
&#x20;        class="px-0"



Where _<mark style="background-color:blue;">**property**</mark>_ is one of:

* `m` - for classes that set <mark style="color:red;">**`margin`**</mark>
* `p` - for classes that set <mark style="color:green;">**`padding`**</mark>

Where _<mark style="color:blue;background-color:orange;">**sides**</mark>_ is one of:

* `t` - for classes that set `margin-top` or `padding-top`
* `b` - for classes that set `margin-bottom` or `padding-bottom`
* `l` - for classes that set `margin-left` or `padding-left`
* `r` - for classes that set `margin-right` or `padding-right`
* `x` - for classes that set both `*-left` and `*-right`
* `y` - for classes that set both `*-top` and `*-bottom`
* blank - for classes that set a `margin` or `padding` on all 4 sides of the element

Where _<mark style="color:green;background-color:blue;">**size**</mark>_ is one of:

* `0` - for classes that eliminate the `margin` or `padding` by setting it to `0`
* `1` - (by default) for classes that set the `margin` or `padding` to `$spacer * .25`
* `2` - (by default) for classes that set the `margin` or `padding` to `$spacer * .5`
* `3` - (by default) for classes that set the `margin` or `padding` to `$spacer`
* `4` - (by default) for classes that set the `margin` or `padding` to `$spacer * 1.5`
* `5` - (by default) for classes that set the `margin` or `padding` to `$spacer * 3`
* `auto` - for classes that set the `margin` to auto

