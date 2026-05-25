<page>
---
title: Liquid reference
description: >-
  The Liquid reference documents the Liquid tags, filters, and objects that you
  can use to build Shopify themes.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid'
  md: 'https://shopify.dev/docs/api/liquid.md'
---

# Liquid reference

Liquid is a template language created by Shopify. It's available as an [open source project](https://shopify.github.io/liquid/) on GitHub, and is used by many different software projects and companies.

This reference documents the Liquid tags, filters, and objects that you can use to build [Shopify Themes](https://shopify.dev/themes).

## What is a template language?

A template language allows you to create a single template to host static content, and dynamically insert information depending on where the template is rendered. For example, you can create a product template that hosts all of your standard product attributes, such as the product image, title, and price. That template can then dynamically render those attributes with the appropriate content, depending on the current product being viewed.

***

## Variations of Liquid

The variation of Liquid in this reference extends the open-source version of Liquid for use with [Shopify themes](https://shopify.dev/themes). It includes tags, filters, and objects that can be used to render objects specific to Shopify stores and storefront functionality.

Shopify also uses slightly different versions of Liquid to render dynamic content for the following features. These variations aren’t included in this reference.

[Notification templates](https://help.shopify.com/en/manual/orders/notifications/email-variables)

[Shopify Flow](https://help.shopify.com/en/manual/shopify-flow/reference/variables#liquid-variables)

[Order printer templates](https://help.shopify.com/en/manual/fulfillment/managing-orders/printing-orders/shopify-order-printer/liquid-variables-and-filters-reference)

[Packing slip templates](https://help.shopify.com/en/manual/orders/packing-slips-variable-list)

***

## Liquid basics

Liquid is used to dynamically output objects and their properties. You can further modify that output by creating logic with tags, or directly altering it with a filter. Objects and object properties are output using one of six basic data types. Liquid also includes basic logical and comparison operators for use with tags.

[Navigate to - Basics](https://shopify.dev/docs/api/liquid/basics)

##### Code

```liquid
<title>
  {{ page_title }}
</title>
{% if page_description -%}
  <meta name="description" content="{{ page_description | truncate: 150 }}">
{%- endif %}
```

##### Output

```html
<title>
  Health potion
</title>
<meta name="description" content="Are you low on health? Well we've got the potion just for you! Just need a top up? Almost dead? In between? No need to worry because we have a ...">
```

***

## Defining logic with tags

Liquid tags are used to define logic that tells templates what to do.

Tags are wrapped with curly brace percentage delimiters `{% %}`. The text within the delimiters is an instruction, not content to render.

In the example to the right, the `if` tag defines the condition to be met. If `product.available` returns `true`, then the price is displayed. Otherwise, the “sold out” message is shown.

`{% %}`

To nest multiple tags inside one set of delimiters, use the [`liquid`](https://shopify.dev/docs/api/liquid/tags/liquid) tag.

##### Code

```liquid
{% if product.available %}
  Price: $99.99
{% else %}
  Sorry, this product is sold out.
{% endif %}
```

##### Data

```json
{
  "product": {
    "available": true
  }
}
```

##### Output

```html
Price: $99.99
```

### Tags with parameters

Some tags accept parameters: either required or optional. For example, the `for` tag takes an optional `limit` parameter to stop the loop at a specific index.

##### Code

```liquid
{% assign numbers = '1,2,3,4,5' | split: ',' %}

{% for item in numbers limit:2 -%}
  {{ item }}
{% endfor %}
```

##### Output

```html
1
2
```

***

## Modifying output with filters

Liquid filters modify the output of variables and objects.

To filter the output of a tag, use the pipe character `|`, followed by the filter. In this example, `product` is the object, `title` is its property, and `upcase` is the filter.

##### Code

```liquid
{% # product.title -> Health potion %}

{{ product.title | upcase }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
HEALTH POTION
```

### Filters with parameters

Many filters accept parameters that adjust their output. Some parameters are required, others are optional.

##### Code

```liquid
{% # product.title -> Health potion %}

{{ product.title | remove: 'Health' }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
potion
```

### Using multiple filters

Multiple filters can be used on one output. They're applied from left to right.

##### Code

```liquid
{% # product.title -> Health potion %}

{{ product.title | upcase | remove: 'HEALTH' }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
POTION
```

***

## Referencing objects

Liquid objects represent variables that you can use to build your theme. Object types include, but aren't limited to:

* Store resources, such as a collection or product and its properties
* Standard content that is used to power Shopify themes, such as `content_for_header`
* Functional elements that can be used to build interactivity, such as `paginate` and `search`

Objects might represent a single data point, or contain multiple properties. Some products might represent a related object, such as a product in a collection.

`{{ }}`

Double curly brace delimiters denote an output.

### Usage

To output an object, wrap it in curly brace delimiters `{{ }}`.

To output an object's property, use dot notation. This example outputs the `product` object's `title` property.

##### Code

```liquid
{{ product.title }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
Health potion
```

### Object access

Objects can be accessed in three ways:

* **Globally**: Available in any Liquid file, excluding [checkout.liquid](https://shopify.dev/themes/architecture/layouts/checkout-liquid) and [Liquid asset files](https://shopify.dev/themes/architecture#assets)
* **In templates**: Available in specific templates and their sections or blocks. For example, the [`product`](https://shopify.dev/docs/api/liquid/objects/product) object in a [product template](https://shopify.dev/themes/architecture/templates/product)
* **Through parent objects**: Returned as properties of other objects. For example, [`article`](https://shopify.dev/docs/api/liquid/objects/article) objects through [`articles`](https://shopify.dev/docs/api/liquid/objects/articles) or [`blog`](https://shopify.dev/docs/api/liquid/objects/blog)

Check each object's documentation to see how it can be accessed.

### Creating variables

To create your own variables, use variable tags like [`assign`](https://shopify.dev/docs/api/liquid/tags/assign) or [`capture`](https://shopify.dev/docs/api/liquid/tags/capture). Syntactically, Liquid treats variables the same as objects.

##### Code

```liquid
{% assign my_variable = 'My custom string.' %}
{{ my_variable }}
```

##### Output

```html
My custom string.
```

***

## Resources & tools

[Liquid Cheat Sheet\
\
](https://www.shopify.com/partners/shopify-cheat-sheet)

[A simple reference guide to the Liquid language.](https://www.shopify.com/partners/shopify-cheat-sheet)

[Theme Check\
\
](https://shopify.dev/themes/tools/theme-check)

[Command line-based linter for themes. Also comes as an official Visual Studio Code extension.](https://shopify.dev/themes/tools/theme-check)

[Shopify CLI for Themes\
\
](https://shopify.dev/themes/tools/cli)

[A powerful command-line tool for building Shopify themes, and exploring Liquid code in a REPL interface.](https://shopify.dev/themes/tools/cli)

[Open source liquid\
\
](https://github.com/Shopify/liquid)

[Liquid is an open source project on GitHub.](https://github.com/Shopify/liquid)

***

</page>

<page>
---
title: Liquid basics
description: >-
  The basic concepts that you need to effectively interact with Liquid tags,
  filters, and objects.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/basics'
  md: 'https://shopify.dev/docs/api/liquid/basics.md'
---

# Basics

The following are basic concepts that you need to effectively interact with Liquid tags, filters, and objects.

## Object handles

Objects that represent store resources, such as products, collections, articles, and blogs, have handles for identifying an individual resource. The handle is used to build the URL for the resource, or to return properties for the resource.

Other objects like `linklists`, `links`, and `settings` also have handles.

### Creating and modifying handles

Handles are automatically generated based on the resource title. They follow a set of rules:

* Handles are always lowercase
* Whitespace and special characters are replaced with a hyphen `-`.
* If there are multiple consecutive whitespace or special characters, then they're replaced with a single hyphen.
* Whitespace or special characters at the beginning are removed.

Handles need to be unique, so if a duplicate title is used, then the handle is auto-incremented by one. For example, if you had two products called `Potion`, then their handles would be `potion` and `potion-1`.

After a resource has been created, changing the resource title doesn't update the handle.

You can modify a resource's handle within the Shopify admin. This can be done either in the Handle or the Edit website SEO sections, depending on the resource. If you reference resources by their handle, then be sure to update those references when modifying handles.

***

**Note:**

Individual links from `linklists` have handles based on their titles. These handles can't be modified directly. Individual settings, from `settings_schema.json`, sections, or blocks, get their handle from their `id` property.

***

##### Code

```liquid
{{ product.title | handle }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
health-potion
```

### Referencing handles

All objects that have a handle have a `handle` property. For example, you can output a product's handle with `product.handle`. You can reference an object, from within its parent object, by its handle in two ways:

* Square bracket notation `[ ]`: Accepts a handle wrapped in quotes `'`, a Liquid variable, or an object reference.
* Dot notation `.`: Accepts a handle without quotes.

***

**Note:**

Referencing an object by its handle is similar to [referencing array elements by their index](https://shopify.dev/docs/api/liquid/basics#array).

***

##### Code

```liquid
'About us' page URL: {{ pages['about-us'].url }}
Enable product suggestions: {{ settings.predictive_search_enabled }}
```

##### Data

```json
{
  "settings": {
    "predictive_search_enabled": true
  }
}
```

##### Output

```html
'About us' page URL: /pages/about-us
Enable product suggestions: true
```

***

## Logical and comparison operators

Liquid supports basic logical and comparison operators for use with conditional tags: [`case`](https://shopify.dev/docs/api/liquid/tags/case), [`else`](https://shopify.dev/docs/api/liquid/tags/conditional-else), [`if`](https://shopify.dev/docs/api/liquid/tags/if) and [`unless`](https://shopify.dev/docs/api/liquid/tags/unless).

| Operator | Function |
| - | - |
| `==` | equals |
| `!=` | does not equal |
| `>` | greater than |
| `<` | less than |
| `>=` | greater than or equal to |
| `>=` | greater than or equal to |
| `<=` | less than or equal to |
| `or` | Condition A or Condition B |
| `and` | Condition A and Condition B |
| `contains` | Checks for strings in strings or arrays |

#### contains

You can use `contains` to check for the presence of a string within an array, or another string. You can’t use `contains` to check for an object in an array of objects.

##### Code

```liquid
{% if product.tags contains 'healing' %}
  This potion contains restorative properties.
{% endif %}
```

##### Data

```json
{
  "product": {
    "tags": [
      "healing"
    ]
  }
}
```

##### Output

```html
This potion contains restorative properties.
```

### Order of operations

When using more than one operator in a tag, the operators are evaluated from right to left, and you can’t change this order.

***

**Caution:**

Parentheses `()` aren’t valid characters within Liquid tags. If you try to include them to group operators, then your tag won’t be rendered.

***

##### Code

```liquid
{% unless true and false and false or true %}
  This evaluates to false, since Liquid checks tags like this:

  true and (false and (false or true))
  true and (false and true)
  true and false
  false
{% endunless %}
```

##### Output

```html
This evaluates to false, since Liquid checks tags like this:

  true and (false and (false or true))
  true and (false and true)
  true and false
  false
```

***

## Types

Liquid output can be one of six data types.

### string

Any series of characters, wrapped in single or double quotes.

***

**Info:**

You can check whether a string is empty with the `blank` object.

***

### number

Numeric values, including floats and integers.

### boolean

A binary value, either `true` or `false`.

### nil

An undefined value.

Tags or outputs that return `nil` don't print anything to the page. They are also treated as `false`.

***

**Note:**

A string with the characters “nil” is not treated the same as `nil`.

***

### array

A list of variables of any type.

To access all of the items in an array, you can loop through each item in the array using a [`for`](https://shopify.dev/docs/api/liquid/tags/for) or [`tablerow`](https://shopify.dev/docs/api/liquid/tags/tablerow) tag.

You can use square bracket `[ ]` notation to access a specific item in an array. Array indexing starts at zero.

You can’t initialize arrays using only Liquid. You can, however, use the split filter to break a single string into an array of substrings.

### empty

An `empty` object is returned if you try to access an object that is defined, but has no value. For example a page or product that’s been deleted, or a setting with no value.

You can compare an object with `empty` to check whether an object exists before you access any of its attributes.

##### Code

```liquid
{% unless pages.about-us == empty %}
  <h1>{{ page.title }}</h1>
  <div>
    {{ page.content }}
  </div>
{% endunless %}
```

##### Data

```json
{
  "page": {
    "content": "<p>Polina's Potent Potions was started by Polina in 1654.</p>\n<p>We use all-natural locally sourced ingredients for our potions.</p>",
    "title": "About us"
  }
}
```

##### Output

```html
<h1>About us</h1>
  <div>
    <p>Polina's Potent Potions was started by Polina in 1654.</p>
<p>We use all-natural locally sourced ingredients for our potions.</p>
  </div>
```

***

## Truthy and falsy

All data types must return either `true` or `false`. Those which return `true` by default are called truthy. Those that return `false` by default are called falsy.

| Value | Truthy | Falsy |
| - | - | - |
| `true` | | |
| `false` | | |
| `nil` | | |
| `empty string` | | |
| `0` | | |
| `integer` | | |
| `float` | | |
| `array` | | |
| `empty array` | | |
| `page` | | |
| `empty object` | | |

### Example

Because `nil` and `false` are the only falsy values, you need to be careful how you check values in Liquid. A value might not be in the format you expect, but still be truthy.

For example, empty strings are truthy, so you need to check whether they’re empty with `blank`. `EmptyDrop` objects are also truthy, so you need to check whether the object you’re referencing is `empty`.

##### Code

```liquid
{% if settings.featured_potions_title != blank -%}
  {{ settings.featured_potions_title }}
{%- else -%}
  No value for this setting has been selected.
{%- endif %}
{% unless pages.recipes == empty -%}
  {{ pages.recipes.content }}
{%- else -%}
  No page with this handle exists.
{%- endunless %}
```

##### Data

```json
{
  "settings": {
    "featured_potions_title": null
  }
}
```

##### Output

```html
No value for this setting has been selected.
No page with this handle exists.
```

***

## Whitespace control

Even if it doesn't output text, any line of Liquid outputs a line in your rendered content. By including hyphens in your Liquid tag, you can strip any whitespace that your Liquid generates when rendered.

If you want to remove whitespace on only one side of the Liquid tag, then you can include the hyphen on either the opening or closing tag.

##### Code

```liquid
{%- if collection.products.size > 0 -%}
The '{{ collection.title }}' collection contains the following types of products:

{% for type in collection.all_types -%}
  {% unless type == blank -%}
    - {{ type }}
  {%- endunless -%}
{%- endfor %}
{%- endif -%}
```

##### Data

```json
{
  "collection": {
    "all_types": [
      "Health",
      "Health & Beauty",
      "Invisibility",
      "Water"
    ],
    "products": [],
    "title": "Sale potions"
  }
}
```

##### Output

```html
The 'Sale potions' collection contains the following types of products:

- Health
- Health & Beauty
- Invisibility
- Water
```

***

</page>

<page>
---
title: 'Liquid filters: abs'
description: Returns the absolute value of a number.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/abs'
  md: 'https://shopify.dev/docs/api/liquid/filters/abs.md'
---

# abs

```oobleck
number | abs
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Returns the absolute value of a number.

##### Code

```liquid
{{ -3 | abs }}
```

##### Output

```html
3
```

</page>

<page>
---
title: 'Liquid filters: append'
description: Adds a given string to the end of a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/append'
  md: 'https://shopify.dev/docs/api/liquid/filters/append.md'
---

# append

```oobleck
string | append: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Adds a given string to the end of a string.

##### Code

```liquid
{%-  assign path = product.url -%}

{{ request.origin | append: path }}
```

##### Data

```json
{
  "product": {
    "url": "/products/health-potion"
  },
  "request": {
    "origin": "https://polinas-potent-potions.myshopify.com"
  }
}
```

##### Output

```html
https://polinas-potent-potions.myshopify.com/products/health-potion
```

</page>

<page>
---
title: 'Liquid filters: article_img_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for an [article's image](/docs/api/liquid/objects/article#article-image).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/article_img_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/article_img_url.md'
---

# article\_​img\_​url

```oobleck
variable | article_img_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for an [article's image](https://shopify.dev/docs/api/liquid/objects/article#article-image).

**Deprecated:**

The `article_img_url` filter has been replaced by [`image_url`](https://shopify.dev/docs/api/liquid/filters/image_url).

##### Code

```liquid
{{ article.image | article_img_url }}
```

##### Data

```json
{
  "article": {
    "image": "articles/beakers-for-science-with-water.jpg"
  }
}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/articles/beakers-for-science-with-water_small.jpg?v=1654385193
```

### size

```oobleck
image | article_img_url: string
```

By default, the `article_img_url` filter returns the `small` version of the image (100 x 100 px). However, you can specify a [size](https://shopify.dev/docs/api/liquid/filters/img_url#img_url-size).

##### Code

```liquid
{{ article.image | article_img_url: 'large' }}
```

##### Data

```json
{
  "article": {
    "image": "articles/beakers-for-science-with-water.jpg"
  }
}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/articles/beakers-for-science-with-water_large.jpg?v=1654385193
```

</page>

<page>
---
title: 'Liquid filters: asset_img_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for an image in the

  [`assets` directory](/themes/architecture#assets) of a theme.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/asset_img_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/asset_img_url.md'
---

# asset\_​img\_​url

```oobleck
string | asset_img_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for an image in the [`assets` directory](https://shopify.dev/themes/architecture#assets) of a theme.

##### Code

```liquid
{{ 'red-and-black-bramble-berries.jpg' | asset_img_url }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/red-and-black-bramble-berries_small.jpg?v=337
```

### size

```oobleck
image | asset_img_url: string
```

By default, the `asset_img_url` filter returns the `small` version of the image (100 x 100 px). However, you can specify a [size](https://shopify.dev/docs/api/liquid/filters/img_url#img_url-size).

##### Code

```liquid
{{ 'red-and-black-bramble-berries.jpg' | asset_img_url: 'large' }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/red-and-black-bramble-berries_large.jpg?v=337
```

</page>

<page>
---
title: 'Liquid filters: asset_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for a file in the

  [`assets` directory](/themes/architecture#assets) of a theme.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/asset_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/asset_url.md'
---

# asset\_​url

```oobleck
string | asset_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for a file in the [`assets` directory](https://shopify.dev/themes/architecture#assets) of a theme.

##### Code

```liquid
{{ 'cart.js' | asset_url }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/cart.js?v=83971781268232213281663872410
```

</page>

<page>
---
title: 'Liquid filters: at_least'
description: Limits a number to a minimum value.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/at_least'
  md: 'https://shopify.dev/docs/api/liquid/filters/at_least.md'
---

# at\_​least

```oobleck
number | at_least
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Limits a number to a minimum value.

##### Code

```liquid
{{ 4 | at_least: 5 }}
{{ 4 | at_least: 3 }}
```

##### Output

```html
5
4
```

</page>

<page>
---
title: 'Liquid filters: at_most'
description: Limits a number to a maximum value.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/at_most'
  md: 'https://shopify.dev/docs/api/liquid/filters/at_most.md'
---

# at\_​most

```oobleck
number | at_most
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Limits a number to a maximum value.

##### Code

```liquid
{{ 6 | at_most: 5 }}
{{ 4 | at_most: 5 }}
```

##### Output

```html
5
4
```

</page>

<page>
---
title: 'Liquid filters: avatar'
description: 'Generates HTML to render a customer''s avatar, if available.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/avatar'
  md: 'https://shopify.dev/docs/api/liquid/filters/avatar.md'
---

# avatar

```oobleck
customer | avatar
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates HTML to render a customer's avatar, if available.

***

**Tip:** Use with the \<a href="/docs/api/liquid/objects/customer#customer-has\_avatar?">\<code>\<span class="PreventFireFoxApplyingGapToWBR">customer.has\<wbr/>\_avatar?\</span>\</code>\</a> method to determine if the customer has an avatar.

***

```liquid
{{ customer | avatar }}
```

```liquid
{{ customer | avatar }}
```

</page>

<page>
---
title: 'Liquid filters: base64_decode'
description: >-
  Decodes a string in [Base64
  format](https://developer.mozilla.org/en-US/docs/Glossary/Base64).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/base64_decode'
  md: 'https://shopify.dev/docs/api/liquid/filters/base64_decode.md'
---

# base64\_​decode

```oobleck
string | base64_decode
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Decodes a string in [Base64 format](https://developer.mozilla.org/en-US/docs/Glossary/Base64).

##### Code

```liquid
{{ 'b25lIHR3byB0aHJlZQ==' | base64_decode }}
```

##### Output

```html
one two three
```

</page>

<page>
---
title: 'Liquid filters: base64_encode'
description: >-
  Encodes a string to [Base64
  format](https://developer.mozilla.org/en-US/docs/Glossary/Base64).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/base64_encode'
  md: 'https://shopify.dev/docs/api/liquid/filters/base64_encode.md'
---

# base64\_​encode

```oobleck
string | base64_encode
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Encodes a string to [Base64 format](https://developer.mozilla.org/en-US/docs/Glossary/Base64).

##### Code

```liquid
{{ 'one two three' | base64_encode }}
```

##### Output

```html
b25lIHR3byB0aHJlZQ==
```

</page>

<page>
---
title: 'Liquid filters: base64_url_safe_decode'
description: >-
  Decodes a string in URL-safe [Base64
  format](https://developer.mozilla.org/en-US/docs/Glossary/Base64).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/base64_url_safe_decode'
  md: 'https://shopify.dev/docs/api/liquid/filters/base64_url_safe_decode.md'
---

# base64\_​url\_​safe\_​decode

```oobleck
string | base64_url_safe_decode
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Decodes a string in URL-safe [Base64 format](https://developer.mozilla.org/en-US/docs/Glossary/Base64).

##### Code

```liquid
{{ 'b25lIHR3byB0aHJlZQ==' | base64_url_safe_decode }}
```

##### Output

```html
one two three
```

</page>

<page>
---
title: 'Liquid filters: base64_url_safe_encode'
description: >-
  Encodes a string to URL-safe [Base64
  format](https://developer.mozilla.org/en-US/docs/Glossary/Base64).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/base64_url_safe_encode'
  md: 'https://shopify.dev/docs/api/liquid/filters/base64_url_safe_encode.md'
---

# base64\_​url\_​safe\_​encode

```oobleck
string | base64_url_safe_encode
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Encodes a string to URL-safe [Base64 format](https://developer.mozilla.org/en-US/docs/Glossary/Base64).

To produce URL-safe Base64, this filter uses `-` and `_` in place of `+` and `/`.

##### Code

```liquid
{{ 'one two three' | base64_url_safe_encode }}
```

##### Output

```html
b25lIHR3byB0aHJlZQ==
```

</page>

<page>
---
title: 'Liquid filters: blake3'
description: Converts a string into a Blake3 hash.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/blake3'
  md: 'https://shopify.dev/docs/api/liquid/filters/blake3.md'
---

# blake3

```oobleck
string | blake3
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string into a Blake3 hash.

##### Code

```liquid
{{ '' | blake3 }}
```

##### Output

```html
af1349b9f5f9a1a6a0404dea36dcc9499bcb25c9adc112b7cc9a93cae41f3262
```

</page>

<page>
---
title: 'Liquid filters: brightness_difference'
description: >-
  Calculates the [perceived brightness
  difference](https://www.w3.org/WAI/ER/WD-AERT/#color-contrast) between two
  colors.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/brightness_difference'
  md: 'https://shopify.dev/docs/api/liquid/filters/brightness_difference.md'
---

# brightness\_​difference

```oobleck
string | brightness_difference: string
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Calculates the [perceived brightness difference](https://www.w3.org/WAI/ER/WD-AERT/#color-contrast) between two colors.

***

**Tip:** For accessibility best practices, it\&#39;s recommended to have a minimum brightness difference of 125.

***

##### Code

```liquid
{{ '#E800B0' | brightness_difference: '#FECEE9' }}
```

##### Output

```html
134
```

</page>

<page>
---
title: 'Liquid filters: camelize'
description: Converts a string to CamelCase.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/camelize'
  md: 'https://shopify.dev/docs/api/liquid/filters/camelize.md'
---

# camelize

```oobleck
string | camelize
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string to CamelCase.

##### Code

```liquid
{{ 'variable-name' | camelize }}
```

##### Output

```html
VariableName
```

</page>

<page>
---
title: 'Liquid filters: capitalize'
description: Capitalizes the first word in a string and downcases the remaining characters.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/capitalize'
  md: 'https://shopify.dev/docs/api/liquid/filters/capitalize.md'
---

# capitalize

```oobleck
string | capitalize
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Capitalizes the first word in a string and downcases the remaining characters.

##### Code

```liquid
{{ 'this sentence should start with a capitalized word.' | capitalize }}
```

##### Output

```html
This sentence should start with a capitalized word.
```

</page>

<page>
---
title: 'Liquid filters: ceil'
description: Rounds a number up to the nearest integer.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/ceil'
  md: 'https://shopify.dev/docs/api/liquid/filters/ceil.md'
---

# ceil

```oobleck
number | ceil
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Rounds a number up to the nearest integer.

##### Code

```liquid
{{ 1.2 | ceil }}
```

##### Output

```html
2
```

</page>

<page>
---
title: 'Liquid filters: collection_img_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for a [collection's
  image](/docs/api/liquid/objects/collection#collection-image).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/collection_img_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/collection_img_url.md'
---

# collection\_​img\_​url

```oobleck
variable | collection_img_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for a [collection's image](https://shopify.dev/docs/api/liquid/objects/collection#collection-image).

**Deprecated:**

The `collection_img_url` filter has been replaced by [`image_url`](https://shopify.dev/docs/api/liquid/filters/image_url).

##### Code

```liquid
{{ collection.image | collection_img_url }}
```

##### Data

```json
{
  "collection": {
    "image": "collections/sale-written-in-lights.jpg"
  }
}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/collections/sale-written-in-lights.jpg?v=1657654130
```

### The size parameter

```oobleck
image | collection_img_url: string
```

By default, the `collection_img_url` filter returns the `small` version of the image (100 x 100 px). However, you can specify a [size](https://shopify.dev/docs/api/liquid/filters/img_url#img_url-size).

##### Code

```liquid
{{ collection.image | collection_img_url: 'large' }}
```

##### Data

```json
{
  "collection": {
    "image": "collections/sale-written-in-lights.jpg"
  }
}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/collections/sale-written-in-lights_large.jpg?v=1657654130
```

</page>

<page>
---
title: 'Liquid filters: color_brightness'
description: >-
  Calculates the [perceived
  brightness](https://www.w3.org/WAI/ER/WD-AERT/#color-contrast) of a given
  color.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_brightness'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_brightness.md'
---

# color\_​brightness

```oobleck
string | color_brightness
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Calculates the [perceived brightness](https://www.w3.org/WAI/ER/WD-AERT/#color-contrast) of a given color.

##### Code

```liquid
{{ '#EA5AB9' | color_brightness }}
```

##### Output

```html
143.89
```

</page>

<page>
---
title: 'Liquid filters: color_contrast'
description: >-
  Calculates the contrast ratio between two colors and returns the ratio's
  numerator. The ratio's denominator, which isn't

  returned, is always 1. For example, with a contrast ratio of 3.5:1, this
  filter returns 3.5.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_contrast'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_contrast.md'
---

# color\_​contrast

```oobleck
string | color_contrast: string
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Calculates the contrast ratio between two colors and returns the ratio's numerator. The ratio's denominator, which isn't returned, is always 1. For example, with a contrast ratio of 3.5:1, this filter returns 3.5.

The order in which you specify the colors doesn't matter.

***

**Tip:** For accessibility best practices, the \<a href="https://www\.w3.org/WAI/WCAG21/quickref/?versions=2.0#qr-visual-audio-contrast-contrast">WCAG 2.0 level AA\</a> requires a minimum contrast ratio of 4.5:1 for normal text, and 3:1 for large text. \<a href="https://www\.w3.org/WAI/WCAG21/quickref/?versions=2.0#qr-visual-audio-contrast7">Level AAA\</a> requires a minimum contrast ratio of 7:1 for normal text, and 4.5:1 for large text.

***

##### Code

```liquid
{{ '#E800B0' | color_contrast: '#D9D8FF' }}
```

##### Output

```html
3.0
```

</page>

<page>
---
title: 'Liquid filters: color_darken'
description: >-
  Darkens a given color by a specific percentage. The percentage must be between
  0 and 100.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_darken'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_darken.md'
---

# color\_​darken

```oobleck
string | color_darken: number
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Darkens a given color by a specific percentage. The percentage must be between 0 and 100.

##### Code

```liquid
{{ '#EA5AB9' | color_darken: 30 }}
```

##### Output

```html
#98136b
```

</page>

<page>
---
title: 'Liquid filters: color_desaturate'
description: >-
  Desaturates a given color by a specific percentage. The percentage must be
  between 0 and 100.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_desaturate'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_desaturate.md'
---

# color\_​desaturate

```oobleck
string | color_desaturate: number
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Desaturates a given color by a specific percentage. The percentage must be between 0 and 100.

##### Code

```liquid
{{ '#EA5AB9' | color_desaturate: 30 }}
```

##### Output

```html
#ce76b0
```

</page>

<page>
---
title: 'Liquid filters: color_difference'
description: >-
  Calculates the [color
  difference](https://www.w3.org/WAI/ER/WD-AERT/#color-contrast) between two
  colors.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_difference'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_difference.md'
---

# color\_​difference

```oobleck
string | color_difference: string
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Calculates the [color difference](https://www.w3.org/WAI/ER/WD-AERT/#color-contrast) between two colors.

***

**Tip:** For accessibility best practices, it\&#39;s recommended to have a minimum color difference of 500.

***

##### Code

```liquid
{{ '#720955' | color_difference: '#FFF3F9' }}
```

##### Output

```html
539
```

</page>

<page>
---
title: 'Liquid filters: color_extract'
description: Extracts a specific color component from a given color.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_extract'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_extract.md'
---

# color\_​extract

```oobleck
string | color_extract: string
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Extracts a specific color component from a given color.

Accepts the following color components:

* `alpha`
* `red`
* `green`
* `blue`
* `hue`
* `saturation`
* `lightness`

##### Code

```liquid
{{ '#EA5AB9' | color_extract: 'red' }}
```

##### Output

```html
234
```

</page>

<page>
---
title: 'Liquid filters: color_lighten'
description: >-
  Lightens a given color by a specific percentage. The percentage must be
  between 0 and 100.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_lighten'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_lighten.md'
---

# color\_​lighten

```oobleck
string | color_lighten: number
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Lightens a given color by a specific percentage. The percentage must be between 0 and 100.

##### Code

```liquid
{{ '#EA5AB9' | color_lighten: 30 }}
```

##### Output

```html
#fbe2f3
```

</page>

<page>
---
title: 'Liquid filters: color_mix'
description: >-
  Blends two colors together by a specific percentage factor. The percentage
  must be between 0 and 100.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_mix'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_mix.md'
---

# color\_​mix

```oobleck
string | color_mix: string, number
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Blends two colors together by a specific percentage factor. The percentage must be between 0 and 100.

***

**Tip:** A percentage factor of 100 returns the color being filtered. A percentage factor of 0 returns the color supplied to the filter.

***

##### Code

```liquid
{{ '#E800B0' | color_mix: '#00936F', 50 }}
```

##### Output

```html
#744a90
```

If one input has an alpha component, but the other does not, an alpha component of 1.0 will be assumed for the input without an alpha component.

##### Code

```liquid
{{ 'rgba(232, 0, 176, 0.75)' | color_mix: '#00936F', 50 }}
```

##### Output

```html
rgba(116, 74, 144, 0.88)
```

</page>

<page>
---
title: 'Liquid filters: color_modify'
description: Modifies a specific color component of a given color by a specific amount.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_modify'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_modify.md'
---

# color\_​modify

```oobleck
string | color_modify: string, number
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Modifies a specific color component of a given color by a specific amount.

The following table outlines valid color components, and the value range for their modifications:

| Component | Value range |
| - | - |
| * `red`
* `green`
* `blue` | An integer between 0 and 255 |
| `alpha` | A decimal between 0 and 1 |
| `hue` | An integer between 0 and 360 |
| - `saturation`
- `lightness` | An integer between 0 and 100 |

##### Code

```liquid
{{ '#EA5AB9' | color_modify: 'red', 255 }}
```

##### Output

```html
#ff5ab9
```

The format of the modified color depends on the component being modified. For example, if you modify the `alpha` component of a color in hexadecimal format, then the modified color will be in `rgba()` format.

##### Code

```liquid
{{ '#EA5AB9' | color_modify: 'alpha', 0.85 }}
```

##### Output

```html
rgba(234, 90, 185, 0.85)
```

</page>

<page>
---
title: 'Liquid filters: color_saturate'
description: >-
  Saturates a given color by a specific percentage. The percentage must be
  between 0 and 100.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_saturate'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_saturate.md'
---

# color\_​saturate

```oobleck
string | color_saturate: number
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Saturates a given color by a specific percentage. The percentage must be between 0 and 100.

##### Code

```liquid
{{ '#EA5AB9' | color_saturate: 30 }}
```

##### Output

```html
#ff45c0
```

</page>

<page>
---
title: 'Liquid filters: color_to_hex'
description: Converts a CSS color string to hexadecimal format (`hex6`).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_to_hex'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_to_hex.md'
---

# color\_​to\_​hex

```oobleck
string | color_to_hex
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a CSS color string to hexadecimal format (`hex6`).

Because colors are converted to `hex6` format, if a color with an alpha component is provided, then the alpha component is excluded from the output.

##### Code

```liquid
{{ 'rgb(234, 90, 185)' | color_to_hex }}
```

##### Output

```html
#ea5ab9
```

</page>

<page>
---
title: 'Liquid filters: color_to_hsl'
description: Converts a CSS color string to `HSL` format.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_to_hsl'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_to_hsl.md'
---

# color\_​to\_​hsl

```oobleck
string | color_to_hsl
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a CSS color string to `HSL` format.

If a color with an alpha component is provided, the color is converted to `HSLA` format.

##### Code

```liquid
{{ '#EA5AB9' | color_to_hsl }}
```

##### Output

```html
hsl(320, 77%, 64%)
```

</page>

<page>
---
title: 'Liquid filters: color_to_oklch'
description: Converts a CSS color string to `OKLCH` format.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_to_oklch'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_to_oklch.md'
---

# color\_​to\_​oklch

```oobleck
string | color_to_oklch
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a CSS color string to `OKLCH` format.

##### Code

```liquid
{{ '#EA5AB9' | color_to_oklch }}
```

##### Output

```html
oklch(68% 0.2 343 / 1.0)
```

</page>

<page>
---
title: 'Liquid filters: color_to_rgb'
description: Converts a CSS color string to `RGB` format.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/color_to_rgb'
  md: 'https://shopify.dev/docs/api/liquid/filters/color_to_rgb.md'
---

# color\_​to\_​rgb

```oobleck
string | color_to_rgb
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a CSS color string to `RGB` format.

If a color with an alpha component is provided, then the color is converted to `RGBA` format.

##### Code

```liquid
{{ '#EA5AB9' | color_to_rgb }}
```

##### Output

```html
rgb(234, 90, 185)
```

</page>

<page>
---
title: 'Liquid filters: compact'
description: Removes any `nil` items from an array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/compact'
  md: 'https://shopify.dev/docs/api/liquid/filters/compact.md'
---

# compact

```oobleck
array | compact
```

Removes any `nil` items from an array.

##### Code

```liquid
{%- assign original_prices = collection.products | map: 'compare_at_price' -%}

Original prices:

{% for price in original_prices -%}
  - {{ price }}
{%- endfor %}

{%- assign compacted_original_prices = original_prices | compact -%}

Original prices - compacted:

{% for price in compacted_original_prices -%}
  - {{ price }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": "1000000.59"
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": "10.00"
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": "25.00"
      },
      {
        "compare_at_price": "400.00"
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      },
      {
        "compare_at_price": null
      }
    ]
  }
}
```

##### Output

```html
Original prices:

- 
- 
- 
- 
- 100000059
- 
- 
- 
- 1000
- 
- 2500
- 40000
- 
- 
- 
- 
- 
- 
- 

Original prices - compacted:

- 100000059
- 1000
- 2500
- 40000
```

</page>

<page>
---
title: 'Liquid filters: concat'
description: Concatenates (combines) two arrays.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/concat'
  md: 'https://shopify.dev/docs/api/liquid/filters/concat.md'
---

# concat

```oobleck
array | concat: array
```

Concatenates (combines) two arrays.

***

**Note:** The \<code>concat\</code> filter won\&#39;t filter out duplicates. If you want to remove duplicates, then you need to use the \<a href="/docs/api/liquid/filters/uniq">\<code>uniq\</code> filter\</a>.

***

##### Code

```liquid
{%- assign types_and_vendors = collection.all_types | concat: collection.all_vendors -%}

Types and vendors:

{% for item in types_and_vendors -%}
  {%- if item != blank -%}
    - {{ item }}
  {%- endif -%}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "all_types": [
      "",
      "Animals & Pet Supplies",
      "Baking Flavors & Extracts",
      "Container",
      "Cooking & Baking Ingredients",
      "Dried Flowers",
      "Fruits & Vegetables",
      "Gift Cards",
      "Health",
      "Health & Beauty",
      "Invisibility",
      "Love",
      "Music & Sound Recordings",
      "Seasonings & Spices",
      "Water"
    ],
    "all_vendors": [
      "Clover's Apothecary",
      "Polina's Potent Potions",
      "Ted's Apothecary Supply"
    ]
  }
}
```

##### Output

```html
Types and vendors:

- Animals & Pet Supplies
- Baking Flavors & Extracts
- Container
- Cooking & Baking Ingredients
- Dried Flowers
- Fruits & Vegetables
- Gift Cards
- Health
- Health & Beauty
- Invisibility
- Love
- Music & Sound Recordings
- Seasonings & Spices
- Water
- Clover's Apothecary
- Polina's Potent Potions
- Ted's Apothecary Supply
```

</page>

<page>
---
title: 'Liquid filters: currency_selector'
description: >-
  Generates an HTML `<select>` element with an option for each currency
  available on the store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/currency_selector'
  md: 'https://shopify.dev/docs/api/liquid/filters/currency_selector.md'
---

# currency\_​selector

```oobleck
form | currency_selector
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<select>` element with an option for each currency available on the store.

The `currency_selector` filter must be applied to the [`form` object](https://shopify.dev/docs/api/liquid/objects/form) within a [currency form](https://shopify.dev/docs/api/liquid/tags/form#form-currency).

**Deprecated:**

Deprecated without a direct replacement because the [currency form](https://shopify.dev/docs/api/liquid/tags/form#form-currency) has also been deprecated. The currency form was replaced by the [localization form](https://shopify.dev/docs/api/liquid/tags/form#form-localization). Refer to this guide which explains [how to create a country selector](https://shopify.dev/docs/themes/markets/multiple-currencies-languages#implementing-country-and-language-selectors) using the localization form.

##### Code

```liquid
{% form 'currency' %}
  {{ form | currency_selector }}
{% endform %}
```

##### Output

```html
<form method="post" action="/cart/update" id="currency_form" accept-charset="UTF-8" class="shopify-currency-form" enctype="multipart/form-data"><input type="hidden" name="form_type" value="currency" /><input type="hidden" name="utf8" value="✓" /><input type="hidden" name="return_to" value="/services/liquid_rendering/resource" />
  <select name="currency"><option value="AED">AED د.إ</option><option value="AFN">AFN ؋</option><option value="AUD">AUD $</option><option value="CAD" selected="selected">CAD $</option><option value="CHF">CHF CHF</option><option value="CZK">CZK Kč</option><option value="DKK">DKK kr.</option><option value="EUR">EUR €</option><option value="GBP">GBP £</option><option value="HKD">HKD $</option><option value="ILS">ILS ₪</option><option value="JPY">JPY ¥</option><option value="KRW">KRW ₩</option><option value="MYR">MYR RM</option><option value="NZD">NZD $</option><option value="PLN">PLN zł</option><option value="SEK">SEK kr</option><option value="SGD">SGD $</option><option value="USD">USD $</option></select>
</form>
```

### class

```oobleck
form | currency_selector: class: string
```

Specify the `class` attribute of the `<select>` element.

##### Code

```liquid
{% form 'currency' %}
  {{ form | currency_selector: class: 'custom-class' }}
{% endform %}
```

##### Output

```html
<form method="post" action="/cart/update" id="currency_form" accept-charset="UTF-8" class="shopify-currency-form" enctype="multipart/form-data"><input type="hidden" name="form_type" value="currency" /><input type="hidden" name="utf8" value="✓" /><input type="hidden" name="return_to" value="/services/liquid_rendering/resource" />
  <select class="custom-class" name="currency"><option value="AED">AED د.إ</option><option value="AFN">AFN ؋</option><option value="AUD">AUD $</option><option value="CAD" selected="selected">CAD $</option><option value="CHF">CHF CHF</option><option value="CZK">CZK Kč</option><option value="DKK">DKK kr.</option><option value="EUR">EUR €</option><option value="GBP">GBP £</option><option value="HKD">HKD $</option><option value="ILS">ILS ₪</option><option value="JPY">JPY ¥</option><option value="KRW">KRW ₩</option><option value="MYR">MYR RM</option><option value="NZD">NZD $</option><option value="PLN">PLN zł</option><option value="SEK">SEK kr</option><option value="SGD">SGD $</option><option value="USD">USD $</option></select>
</form>
```

### id

```oobleck
form | currency_selector: id: string
```

Specify the `id` attribute of the `<select>` element.

##### Code

```liquid
{% form 'currency' %}
  {{ form | currency_selector: id: 'custom-id' }}
{% endform %}
```

##### Output

```html
<form method="post" action="/cart/update" id="currency_form" accept-charset="UTF-8" class="shopify-currency-form" enctype="multipart/form-data"><input type="hidden" name="form_type" value="currency" /><input type="hidden" name="utf8" value="✓" /><input type="hidden" name="return_to" value="/services/liquid_rendering/resource" />
  <select id="custom-id" name="currency"><option value="AED">AED د.إ</option><option value="AFN">AFN ؋</option><option value="AUD">AUD $</option><option value="CAD" selected="selected">CAD $</option><option value="CHF">CHF CHF</option><option value="CZK">CZK Kč</option><option value="DKK">DKK kr.</option><option value="EUR">EUR €</option><option value="GBP">GBP £</option><option value="HKD">HKD $</option><option value="ILS">ILS ₪</option><option value="JPY">JPY ¥</option><option value="KRW">KRW ₩</option><option value="MYR">MYR RM</option><option value="NZD">NZD $</option><option value="PLN">PLN zł</option><option value="SEK">SEK kr</option><option value="SGD">SGD $</option><option value="USD">USD $</option></select>
</form>
```

</page>

<page>
---
title: 'Liquid filters: customer_login_link'
description: Generates an HTML link to the customer login page.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/customer_login_link'
  md: 'https://shopify.dev/docs/api/liquid/filters/customer_login_link.md'
---

# customer\_​login\_​link

```oobleck
string | customer_login_link
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML link to the customer login page.

##### Code

```liquid
{{ 'Log in' | customer_login_link }}
```

##### Output

```html
<a href="/account/login" id="customer_login_link">Log in</a>
```

</page>

<page>
---
title: 'Liquid filters: customer_logout_link'
description: >-
  Generates an HTML link to log the customer out of their account and redirect
  to the homepage.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/customer_logout_link'
  md: 'https://shopify.dev/docs/api/liquid/filters/customer_logout_link.md'
---

# customer\_​logout\_​link

```oobleck
string | customer_logout_link
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML link to log the customer out of their account and redirect to the homepage.

##### Code

```liquid
{{ 'Log out' | customer_logout_link }}
```

##### Output

```html
<a href="/account/logout" id="customer_logout_link">Log out</a>
```

</page>

<page>
---
title: 'Liquid filters: customer_register_link'
description: Generates an HTML link to the customer registration page.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/customer_register_link'
  md: 'https://shopify.dev/docs/api/liquid/filters/customer_register_link.md'
---

# customer\_​register\_​link

```oobleck
string | customer_register_link
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML link to the customer registration page.

##### Code

```liquid
{{ 'Create an account' | customer_register_link }}
```

##### Output

```html
<a href="/account/register" id="customer_register_link">Create an account</a>
```

</page>

<page>
---
title: 'Liquid filters: date'
description: Converts a timestamp into another date format.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/date'
  md: 'https://shopify.dev/docs/api/liquid/filters/date.md'
---

# date

```oobleck
string | date: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a timestamp into another date format.

The `date` filter accepts the same parameters as Ruby's strftime method for formatting the date. For a list of shorthand formats, refer to the [Ruby documentation](https://ruby-doc.org/core-3.1.1/Time.html#method-i-strftime) or [strftime reference and sandbox](http://www.strfti.me/).

##### Code

```liquid
{{ article.created_at | date: '%B %d, %Y' }}
```

##### Data

```json
{
  "article": {
    "created_at": "2022-04-14 16:56:02 -0400"
  }
}
```

##### Output

```html
April 14, 2022
```

### The current date

You can apply the `date` filter to the keywords `'now'` and `'today'` to output the current timestamp.

***

**Note:** The timestamp will reflect the time that the Liquid was last rendered. Because of this, the timestamp might not be updated for every page view, depending on the context and caching.

***

##### Code

```liquid
{{ 'now' | date: '%B %d, %Y' }}
```

### format

```oobleck
string | date: format: string
```

Specify a locale-aware date format. You can use the following formats:

* `abbreviated_date`
* `basic`
* `date`
* `date_at_time`
* `default`
* `on_date`
* `short` (deprecated)
* `long` (deprecated)

***

**Note:** You can also \<a href="/docs/api/liquid/filters/date-setting-format-options-in-locale-files">define custom formats\</a> in your theme\&#39;s locale files.

***

##### Code

```liquid
{{ article.created_at | date: format: 'abbreviated_date' }}
```

##### Data

```json
{
  "article": {
    "created_at": "2022-04-14 16:56:02 -0400"
  }
}
```

##### Output

```html
Apr 14, 2022
```

### Setting format options in locale files

You can define custom date formats in your [theme's storefront locale files](https://shopify.dev/themes/architecture/locales/storefront-locale-files). These custom formats should be included in a `date_formats` category:

```json
"date_formats": {
  "month_day_year": "%B %d, %Y"
}
```

```json
"date_formats": {
  "month_day_year": "%B %d, %Y"
}
```

##### Code

```liquid
{{ article.created_at | date: format: 'month_day_year' }}
```

##### Data

```json
{
  "article": {
    "created_at": "2022-04-14 16:56:02 -0400"
  }
}
```

##### Output

```html
April 14, 2022
```

</page>

<page>
---
title: 'Liquid filters: default'
description: |-
  Sets a default value for any variable whose value is one of the following:

  - [`empty`](/docs/api/liquid/basics#empty)
  - [`false`](/docs/api/liquid/basics#truthy-and-falsy)
  - [`nil`](/docs/api/liquid/basics#nil)
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/default'
  md: 'https://shopify.dev/docs/api/liquid/filters/default.md'
---

# default

```oobleck
variable | default: variable
```

Sets a default value for any variable whose value is one of the following:

* [`empty`](https://shopify.dev/docs/api/liquid/basics#empty)
* [`false`](https://shopify.dev/docs/api/liquid/basics#truthy-and-falsy)
* [`nil`](https://shopify.dev/docs/api/liquid/basics#nil)

##### Code

```liquid
{{ product.selected_variant.url | default: product.url }}
```

##### Data

```json
{
  "product": {
    "selected_variant": null,
    "url": "/products/health-potion"
  }
}
```

##### Output

```html
/products/health-potion
```

### allow\_false

```oobleck
variable | default: variable, allow_false: boolean
```

By default, the `default` filter's value will be used in place of `false` values. You can use the `allow_false` parameter to allow variables to return `false` instead of the default value.

##### Code

```liquid
{%- assign display_price = false -%}

{{ display_price | default: true, allow_false: true }}
```

##### Output

```html
false
```

</page>

<page>
---
title: 'Liquid filters: default_errors'
description: >-
  Generates default error messages for each possible value of
  [`form.errors`](/docs/themes/liquid/reference/objects/form#form-errors).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/default_errors'
  md: 'https://shopify.dev/docs/api/liquid/filters/default_errors.md'
---

# default\_​errors

```oobleck
string | default_errors
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates default error messages for each possible value of [`form.errors`](https://shopify.dev/docs/themes/liquid/reference/objects/form#form-errors).

</page>

<page>
---
title: 'Liquid filters: default_pagination'
description: >-
  Generates HTML for a set of links for paginated results. Must be applied to
  the [`paginate` object](/docs/api/liquid/objects/paginate).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/default_pagination'
  md: 'https://shopify.dev/docs/api/liquid/filters/default_pagination.md'
---

# default\_​pagination

```oobleck
paginate | default_pagination
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates HTML for a set of links for paginated results. Must be applied to the [`paginate` object](https://shopify.dev/docs/api/liquid/objects/paginate).

##### Code

```liquid
{% paginate collection.products by 2 %}
  {% for product in collection.products %}
    {{- product.title }}
  {% endfor %}

  {{- paginate | default_pagination -}}
{% endpaginate %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      }
    ],
    "products_count": 4
  }
}
```

##### Output

```html
Draught of Immortality
  
Glacier ice
  
<span class="page current">1</span> <span class="page"><a href="/services/liquid_rendering/resource?page=2" title="">2</a></span> <span class="next"><a href="/services/liquid_rendering/resource?page=2" title="">Next &raquo;</a></span>
```

### previous

```oobleck
paginate | default_pagination: previous: string
```

Specify the text for the previous page link.

##### Code

```liquid
{% paginate collection.products by 2 %}
  {% for product in collection.products %}
    {{- product.title }}
  {% endfor %}

  {{- paginate | default_pagination: previous: 'Previous' -}}
{% endpaginate %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      }
    ],
    "products_count": 4
  }
}
```

##### Output

```html
Draught of Immortality
  
Glacier ice
  
<span class="page current">1</span> <span class="page"><a href="/services/liquid_rendering/resource?page=2" title="">2</a></span> <span class="next"><a href="/services/liquid_rendering/resource?page=2" title="">Next &raquo;</a></span>
```

### next

```oobleck
paginate | default_pagination: next: string
```

Specify the text for the next page link.

##### Code

```liquid
{% paginate collection.products by 2 %}
  {% for product in collection.products %}
    {{- product.title }}
  {% endfor %}

  {{- paginate | default_pagination: next: 'Next' -}}
{% endpaginate %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      }
    ],
    "products_count": 4
  }
}
```

##### Output

```html
Draught of Immortality
  
Glacier ice
  
<span class="page current">1</span> <span class="page"><a href="/services/liquid_rendering/resource?page=2" title="">2</a></span> <span class="next"><a href="/services/liquid_rendering/resource?page=2" title="">Next</a></span>
```

### anchor

```oobleck
paginate | default_pagination: anchor: string
```

Specify the anchor to add to the pagination links.

##### Code

```liquid
{% paginate collection.products by 2 %}
  {% for product in collection.products %}
    {{- product.title }}
  {% endfor %}

  <div id="pagination">
    {{- paginate | default_pagination: anchor: 'pagination' -}}
  </div>
{% endpaginate %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      }
    ],
    "products_count": 4
  }
}
```

##### Output

```html
Draught of Immortality
  
Glacier ice
  

  <div id="pagination"><span class="page current">1</span> <span class="page"><a href="/services/liquid_rendering/resource?page=2#pagination" title="">2</a></span> <span class="next"><a href="/services/liquid_rendering/resource?page=2#pagination" title="">Next &raquo;</a></span></div>
```

</page>

<page>
---
title: 'Liquid filters: divided_by'
description: >-
  Divides a number by a given number. The `divided_by` filter produces a result
  of the same type as the divisor. This means if you divide by an integer, the
  result will be an integer, and if you divide by a float, the result will be a
  float.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/divided_by'
  md: 'https://shopify.dev/docs/api/liquid/filters/divided_by.md'
---

# divided\_​by

```oobleck
number | divided_by: number
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Divides a number by a given number. The `divided_by` filter produces a result of the same type as the divisor. This means if you divide by an integer, the result will be an integer, and if you divide by a float, the result will be a float.

##### Code

```liquid
{{ 4 | divided_by: 2 }}

# divisor is an integer
{{ 20 | divided_by: 7 }}

# divisor is a float 
{{ 20 | divided_by: 7.0 }}
```

##### Output

```html
2

# divisor is an integer
2

# divisor is a float 
2.857142857142857
```

</page>

<page>
---
title: 'Liquid filters: downcase'
description: Converts a string to all lowercase characters.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/downcase'
  md: 'https://shopify.dev/docs/api/liquid/filters/downcase.md'
---

# downcase

```oobleck
string | downcase
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string to all lowercase characters.

##### Code

```liquid
{{ product.title | downcase }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
health potion
```

</page>

<page>
---
title: 'Liquid filters: escape'
description: >-
  Escapes special characters in HTML, such as `<>`, `'`, and `&`, and converts
  characters into escape sequences. The filter doesn't effect characters within
  the string that don’t have a corresponding escape sequence.".
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/escape'
  md: 'https://shopify.dev/docs/api/liquid/filters/escape.md'
---

# escape

```oobleck
string | escape
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Escapes special characters in HTML, such as `<>`, `'`, and `&`, and converts characters into escape sequences. The filter doesn't effect characters within the string that don’t have a corresponding escape sequence.".

##### Code

```liquid
{{ '<p>Text to be escaped.</p>' | escape }}
```

##### Output

```html
&lt;p&gt;Text to be escaped.&lt;/p&gt;
```

</page>

<page>
---
title: 'Liquid filters: escape_once'
description: Escapes a string without changing characters that have already been escaped.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/escape_once'
  md: 'https://shopify.dev/docs/api/liquid/filters/escape_once.md'
---

# escape\_​once

```oobleck
string | escape_once
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Escapes a string without changing characters that have already been escaped.

##### Code

```liquid
# applying the escape filter to already escaped text escapes characters in HTML entities:

{{ "&lt;p&gt;Text to be escaped.&lt;/p&gt;" | escape }}

# applying the escape_once filter to already escaped text skips characters in HTML entities:

{{ "&lt;p&gt;Text to be escaped.&lt;/p&gt;" | escape_once }}

# use escape_once to escape strings where a combination of HTML entities and non-escaped characters might be present:

{{ "&lt;p&gt;Text to be escaped.&lt;/p&gt; & some additional text" | escape_once }}
```

</page>

<page>
---
title: 'Liquid filters: external_video_tag'
description: >-
  Generates an HTML `<iframe>` tag containing the player for a given external
  video. The input for the `external_video_tag`

  filter can be either a [`media` object](/docs/api/liquid/objects/media) or
  [`external_video_url`](/docs/api/liquid/filters/external_video_url).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/external_video_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/external_video_tag.md'
---

# external\_​video\_​tag

```oobleck
variable | external_video_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<iframe>` tag containing the player for a given external video. The input for the `external_video_tag` filter can be either a [`media` object](https://shopify.dev/docs/api/liquid/objects/media) or [`external_video_url`](https://shopify.dev/docs/api/liquid/filters/external_video_url).

If [alt text is set on the video](https://help.shopify.com/en/manual/products/product-media/add-alt-text), then it's included in the `title` attribute of the `<iframe>`. If no alt text is set, then the `title` attribute is set to the product title.

##### Code

```liquid
{% for media in product.media %}
  {% if media.media_type == 'external_video' %}
    {% if media.host == 'youtube' %}
      {{ media | external_video_url: color: 'white' | external_video_tag }}
    {% elsif media.host == 'vimeo' %}
      {{ media | external_video_url: loop: '1', muted: '1' | external_video_tag }}
    {% endif %}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": [
      {
        "host": "youtube",
        "media_type": "external_video"
      },
      {
        "host": null,
        "media_type": "video"
      }
    ]
  }
}
```

##### Output

```html
<iframe frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="allowfullscreen" src="https://www.youtube.com/embed/vj01PAffOac?color=white&amp;controls=1&amp;enablejsapi=1&amp;modestbranding=1&amp;origin=https%3A%2F%2Fpolinas-potent-potions.myshopify.com&amp;playsinline=1&amp;rel=0" title="Potion beats"></iframe>
```

### HTML attributes

```oobleck
variable | external_video_tag: attribute: string
```

You can specify [HTML attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attributes) by adding a parameter that matches the attribute name, and the desired value.

##### Code

```liquid
{% for media in product.media %}
  {% if media.media_type == 'external_video' %}
    {% if media.host == 'youtube' %}
      {{ media | external_video_url: color: 'white' | external_video_tag: class:'youtube-video' }}
    {% endif %}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": [
      {
        "host": "youtube",
        "media_type": "external_video"
      },
      {
        "host": null,
        "media_type": "video"
      }
    ]
  }
}
```

##### Output

```html
<iframe frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="allowfullscreen" class="youtube-video" src="https://www.youtube.com/embed/vj01PAffOac?color=white&amp;controls=1&amp;enablejsapi=1&amp;modestbranding=1&amp;origin=https%3A%2F%2Fpolinas-potent-potions.myshopify.com&amp;playsinline=1&amp;rel=0" title="Potion beats"></iframe>
```

</page>

<page>
---
title: 'Liquid filters: external_video_url'
description: >-
  Returns the URL for a given external video. Use this filter to specify
  parameters for the external video player generated

  by the [`external_video_tag`
  filter](/docs/api/liquid/filters/external_video_tag).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/external_video_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/external_video_url.md'
---

# external\_​video\_​url

```oobleck
media | external_video_url: attribute: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the URL for a given external video. Use this filter to specify parameters for the external video player generated by the [`external_video_tag` filter](https://shopify.dev/docs/api/liquid/filters/external_video_tag).

You can specify [YouTube](https://developers.google.com/youtube/player_parameters#Parameters) and [Vimeo](https://vimeo.zendesk.com/hc/en-us/articles/360001494447-Using-Player-Parameters) video parameters by adding a parameter that matches the parameter name, and the desired value.

##### Code

```liquid
{% for media in product.media %}
  {% if media.media_type == 'external_video' %}
    {% if media.host == 'youtube' %}
      {{ media | external_video_url: color: 'white' | external_video_tag }}
    {% elsif media.host == 'vimeo' %}
      {{ media | external_video_url: loop: '1', muted: '1' | external_video_tag }}
    {% endif %}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": [
      {
        "host": "youtube",
        "media_type": "external_video"
      },
      {
        "host": null,
        "media_type": "video"
      }
    ]
  }
}
```

##### Output

```html
<iframe frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="allowfullscreen" src="https://www.youtube.com/embed/vj01PAffOac?color=white&amp;controls=1&amp;enablejsapi=1&amp;modestbranding=1&amp;origin=https%3A%2F%2Fpolinas-potent-potions.myshopify.com&amp;playsinline=1&amp;rel=0" title="Potion beats"></iframe>
```

</page>

<page>
---
title: 'Liquid filters: file_img_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for an image from the

  [Files](https://www.shopify.com/admin/settings/files) page of the Shopify
  admin.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/file_img_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/file_img_url.md'
---

# file\_​img\_​url

```oobleck
string | file_img_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for an image from the [Files](https://www.shopify.com/admin/settings/files) page of the Shopify admin.

##### Code

```liquid
{{ 'potions-header.png' | file_img_url }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/potions-header_small.png?v=4246568442683817558
```

### The size parameter

```oobleck
image | file_img_url: string
```

By default, the `file_img_url` filter returns the `small` version of the image (100 x 100 px). However, you can specify a [size](https://shopify.dev/docs/api/liquid/filters/img_url#img_url-size).

##### Code

```liquid
{{ 'potions-header.png' | file_img_url: 'large' }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/potions-header_large.png?v=4246568442683817558
```

</page>

<page>
---
title: 'Liquid filters: file_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for a file from the

  [Files](https://www.shopify.com/admin/settings/files) page of the Shopify
  admin.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/file_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/file_url.md'
---

# file\_​url

```oobleck
string | file_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for a file from the [Files](https://www.shopify.com/admin/settings/files) page of the Shopify admin.

##### Code

```liquid
{{ 'disclaimer.pdf' | file_url }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/disclaimer.pdf?v=9043651738044769859
```

</page>

<page>
---
title: 'Liquid filters: find'
description: Returns the first item in an array with a specific property value.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/find'
  md: 'https://shopify.dev/docs/api/liquid/filters/find.md'
---

# find

```oobleck
array | find: string, string
```

Returns the first item in an array with a specific property value.

This requires you to provide both the property name and the associated value.

##### Code

```liquid
{% assign product = collection.products | find: 'vendor', "Polina's Potent Potions" %}

{{ product.title }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Charcoal",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Crocodile tears",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Dandelion milk",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Draught of Immortality",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Dried chamomile",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Forest mushroom",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Gift Card",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Glacier ice",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Ground mandrake root",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Health potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Invisibility potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Komodo dragon scale",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Love Potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Mana potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Potion beats",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Potion bottle",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Viper venom",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Whole bloodroot",
        "vendor": "Clover's Apothecary"
      }
    ]
  }
}
```

##### Output

```html
Blue Mountain Flower
```

### Returns `nil` when no items match the specified property value.

##### Code

```liquid
{% assign product = collection.products | find: 'vendor', "Polina's Potions" %}

{{ product.title | default: "No product found" }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      }
    ]
  }
}
```

##### Output

```html
No product found
```

</page>

<page>
---
title: 'Liquid filters: find_index'
description: >-
  Returns the index of the first item in an array with a specific property
  value.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/find_index'
  md: 'https://shopify.dev/docs/api/liquid/filters/find_index.md'
---

# find\_​index

```oobleck
array | find_index: string, string
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Returns the index of the first item in an array with a specific property value.

This requires you to provide both the property name and the associated value.

##### Code

```liquid
{% assign index = collection.products | find_index: 'vendor', "Polina's Potent Potions" %}

{{ index }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      }
    ]
  }
}
```

##### Output

```html
0
```

### Returns `nil` when no items match the specified property value.

##### Code

```liquid
{% assign index = collection.products | find_index: 'vendor', "Polina's Potions" %}

{{ index | default: "No index found" }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      }
    ]
  }
}
```

##### Output

```html
No index found
```

</page>

<page>
---
title: 'Liquid filters: first'
description: Returns the first item in an array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/first'
  md: 'https://shopify.dev/docs/api/liquid/filters/first.md'
---

# first

```oobleck
array | first
```

Returns the first item in an array.

##### Code

```liquid
{%- assign first_product = collection.products | first -%}

{{ first_product.title }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower"
      },
      {
        "title": "Charcoal"
      },
      {
        "title": "Crocodile tears"
      },
      {
        "title": "Dandelion milk"
      },
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Dried chamomile"
      },
      {
        "title": "Forest mushroom"
      },
      {
        "title": "Gift Card"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Ground mandrake root"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      },
      {
        "title": "Komodo dragon scale"
      },
      {
        "title": "Love Potion"
      },
      {
        "title": "Mana potion"
      },
      {
        "title": "Potion beats"
      },
      {
        "title": "Potion bottle"
      },
      {
        "title": "Viper venom"
      },
      {
        "title": "Whole bloodroot"
      }
    ]
  }
}
```

##### Output

```html
Blue Mountain Flower
```

### Dot notation

You can use the `first` filter with dot notation when you need to use it inside a tag or object output.

##### Code

```liquid
{{ collection.products.first.title }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower"
      },
      {
        "title": "Charcoal"
      },
      {
        "title": "Crocodile tears"
      },
      {
        "title": "Dandelion milk"
      },
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Dried chamomile"
      },
      {
        "title": "Forest mushroom"
      },
      {
        "title": "Gift Card"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Ground mandrake root"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      },
      {
        "title": "Komodo dragon scale"
      },
      {
        "title": "Love Potion"
      },
      {
        "title": "Mana potion"
      },
      {
        "title": "Potion beats"
      },
      {
        "title": "Potion bottle"
      },
      {
        "title": "Viper venom"
      },
      {
        "title": "Whole bloodroot"
      }
    ]
  }
}
```

##### Output

```html
Blue Mountain Flower
```

</page>

<page>
---
title: 'Liquid filters: floor'
description: Rounds a number down to the nearest integer.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/floor'
  md: 'https://shopify.dev/docs/api/liquid/filters/floor.md'
---

# floor

```oobleck
number | floor
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Rounds a number down to the nearest integer.

##### Code

```liquid
{{ 1.2 | floor }}
```

##### Output

```html
1
```

</page>

<page>
---
title: 'Liquid filters: font_face'
description: >-
  Generates a CSS [`@font_face`
  declaration](https://developer.mozilla.org/en-US/docs/Web/CSS/%40font-face) to
  load the provided font.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/font_face'
  md: 'https://shopify.dev/docs/api/liquid/filters/font_face.md'
---

# font\_​face

```oobleck
font | font_face
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates a CSS [`@font_face` declaration](https://developer.mozilla.org/en-US/docs/Web/CSS/%40font-face) to load the provided font.

##### Code

```liquid
{{ settings.type_header_font | font_face }}
```

##### Data

```json
{
  "settings": {
    "type_header_font": {}
  }
}
```

##### Output

```html
@font-face {
  font-family: Assistant;
  font-weight: 400;
  font-style: normal;
  src: url("//polinas-potent-potions.myshopify.com/cdn/fonts/assistant/assistant_n4.9120912a469cad1cc292572851508ca49d12e768.woff2") format("woff2"),
       url("//polinas-potent-potions.myshopify.com/cdn/fonts/assistant/assistant_n4.6e9875ce64e0fefcd3f4446b7ec9036b3ddd2985.woff") format("woff");
}
```

### font\_display

```oobleck
font | font_face: font_display: string
```

You can include an optional parameter to specify the [`font_display` property](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display) of the `@font_face` declaration.

##### Code

```liquid
{{ settings.type_header_font | font_face: font_display: 'swap' }}
```

##### Data

```json
{
  "settings": {
    "type_header_font": {}
  }
}
```

##### Output

```html
@font-face {
  font-family: Assistant;
  font-weight: 400;
  font-style: normal;
  font-display: swap;
  src: url("//polinas-potent-potions.myshopify.com/cdn/fonts/assistant/assistant_n4.9120912a469cad1cc292572851508ca49d12e768.woff2") format("woff2"),
       url("//polinas-potent-potions.myshopify.com/cdn/fonts/assistant/assistant_n4.6e9875ce64e0fefcd3f4446b7ec9036b3ddd2985.woff") format("woff");
}
```

</page>

<page>
---
title: 'Liquid filters: font_modify'
description: Modifies a specific property of a given font.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/font_modify'
  md: 'https://shopify.dev/docs/api/liquid/filters/font_modify.md'
---

# font\_​modify

```oobleck
font | font_modify: string, string
```

returns [font](https://shopify.dev/docs/api/liquid/objects/font)

Modifies a specific property of a given font.

The `font_modify` filter requires two parameters. The first indicates which property should be modified and the second is either the new value, or modification amount, for that property.

***

**Tip:** You can access every variant of the chosen font\&#39;s family by using \<a href="/docs/api/liquid/objects/font#font-variants">\<code>font.variants\</code>\</a>. However, you can more easily access specific styles and weights by using the \<code>\<span class="PreventFireFoxApplyingGapToWBR">font\<wbr/>\_modify\</span>\</code> filter.

***

The following table outlines the valid font properties and modification values:

| Property | Modification value | Output |
| - | - | - |
| `style` | `normal` | Returns the normal variant of the same weight, if it exists. |
| `italic` | Returns the italic variant of the same weight, if it exists. | |
| `oblique` | Returns the oblique variant of the same weight, if it exists.Oblique variants are similar to italic variants in appearance. All Shopify fonts have only oblique or italic variants, not both. | |
| `weight` | `100` → `900` | Returns a variant of the same style with the given weight, if it exists. |
| `normal` | Returns a variant of the same style with a weight of `400`, if it exists. | |
| `bold` | Returns a variant of the same style with a weight of `700`, if it exists. | |
| `+100` → `+900` | Returns a variant of the same style with a weight incremented by the given value, if it exists.For example, if a font has a weight of `400`, then using `+100` would return the font with a weight of `500`. | |
| `-100` → `-900` | Returns a variant of the same style with a weight decremented by the given value, if it exists.For example, if a font has a weight of `400`, then using `-100` would return the font with a weight of `300`. | |
| `lighter` | Returns a lighter variant of the same style by applying the rules used by the [CSS `font-weight` property](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight#Meaning_of_relative_weights) and browser [fallback weights](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight#Fallback_weights), if it exists. | |
| `bolder` | Returns a bolder variant of the same style by applying the rules used by the [CSS `font-weight` property](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight#Meaning_of_relative_weights) and browser [fallback weights](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight#Fallback_weights), if it exists. | |

##### Code

```liquid
{%- assign bold_font = settings.type_body_font | font_modify: 'weight', 'bold' -%}

h2 {
  font-weight: {{ bold_font.weight }};
}
```

##### Data

```json
{
  "settings": {
    "type_body_font": {}
  }
}
```

##### Output

```html
h2 {
  font-weight: 700;
}
```

### Non-existent font variants

If the `font_modify` filter tries to create a font variant that doesn't exist, then it returns `nil`. To handle this, you can either assign a fallback value with the [`default` filter](https://shopify.dev/docs/api/liquid/filters/default), or check for `nil` before using the variant.

##### Code

```liquid
{%- assign bold_font = settings.type_body_font | font_modify: 'weight', 'bold' -%}
{%- assign italic_font = settings.type_body_font | font_modify: 'style', 'italic' -%}
{%- assign heavy_font = settings.type_body_font | font_modify: 'weight', '900' | default: bold_font -%}
{%- assign oblique_font = settings.type_body_font | font_modify: 'style', 'oblique' | default: italic_font -%}

h2 {
  font-style: {{ heavy_font.weight }};
}

.italic {
  {% if oblique_font -%}
    font-style: {{ oblique_font.style }};
  {%- else -%}
    font-style: {{ italic_font.style }};
  {%- endif %}
}
```

##### Data

```json
{
  "settings": {
    "type_body_font": {}
  }
}
```

##### Output

```html
h2 {
  font-style: 700;
}

.italic {
  font-style: ;
}
```

</page>

<page>
---
title: 'Liquid filters: font_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for the provided font in `woff2` format.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/font_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/font_url.md'
---

# font\_​url

```oobleck
font | font_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for the provided font in `woff2` format.

##### Code

```liquid
{{ settings.type_header_font | font_url }}
```

##### Data

```json
{
  "settings": {
    "type_header_font": {}
  }
}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/fonts/assistant/assistant_n4.9120912a469cad1cc292572851508ca49d12e768.woff2
```

### woff format

```oobleck
font | font_url: string
```

By default, the `font_url` filter returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for the font in `woff2` format. However, you can also choose `woff` format.

##### Code

```liquid
{{ settings.type_header_font | font_url: 'woff' }}
```

##### Data

```json
{
  "settings": {
    "type_header_font": {}
  }
}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/fonts/assistant/assistant_n4.6e9875ce64e0fefcd3f4446b7ec9036b3ddd2985.woff
```

</page>

<page>
---
title: 'Liquid filters: format_address'
description: >-
  Generates an HTML address display, with each address component ordered
  according to the address's locale.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/format_address'
  md: 'https://shopify.dev/docs/api/liquid/filters/format_address.md'
---

# format\_​address

```oobleck
address | format_address
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML address display, with each address component ordered according to the address's locale.

##### Code

```liquid
{{ shop.address | format_address }}
```

##### Data

```json
{
  "shop": {
    "address": {}
  }
}
```

##### Output

```html
<p>Polina&#39;s Potions, LLC<br>150 Elgin Street<br>8th floor<br>Ottawa ON K2P 1L4<br>Canada</p>
```

## Rendered output

##### Code

```liquid
{{ customer.default_address | format_address }}
```

##### Data

```json
{
  "customer": {
    "default_address": {}
  }
}
```

##### Output

```html
<p>Cornelius Potionmaker<br>12 Phoenix Feather Alley<br>1<br>Calgary AB T1X 0L4<br>Canada</p>
```

## Rendered output

</page>

<page>
---
title: 'Liquid filters: global_asset_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for a global asset.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/global_asset_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/global_asset_url.md'
---

# global\_​asset\_​url

```oobleck
string | global_asset_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for a global asset.

Global assets are kept in a directory on Shopify's server. Using global assets can be faster than loading the resource directly.

Depending on the resource type, you might need to use an additional filter to load the resource. The following table outlines which filter to use for specific resource types.

| Resource type | Additional filter |
| - | - |
| JavaScript (`.js`) | [`script_tag`](https://shopify.dev/docs/api/liquid/filters/script_tag) |
| CSS (`.css`) | [`stylesheet_tag`](https://shopify.dev/docs/api/liquid/filters/stylesheet_tag) |

The following table outlines the available global assets:

| Category | Assets |
| - | - |
| Firebug | - `firebug/firebug.css` - `firebug/firebug.html` - `firebug/firebug.js` - `firebug/firebugx.js` - `firebug/errorIcon.png` - `firebug/infoIcon.png` - `firebug/warningIcon.png` |
| JavaScript libraries | - `controls.js` - `dragdrop.js` - `effects.js` - `ga.js` - `mootools.js` |
| Lightbox | - `lightbox.css` - `lightbox.js` - `lightbox/v1/lightbox.css` - `lightbox/v1/lightbox.js` - `lightbox/v2/lightbox.css` - `lightbox/v2/lightbox.js` - `lightbox/v2/close.gif` - `lightbox/v2/loading.gif` - `lightbox/v2/overlay.png` - `lightbox/v2/zoom-lg.gif` - `lightbox/v204/lightbox.css` - `lightbox/v204/lightbox.js` - `lightbox/v204/bullet.gif` - `lightbox/v204/close.gif` - `lightbox/v204/closelabel.gif` - `lightbox/v204/donatebutton.gif` - `lightbox/v204/downloadicon.gif` - `lightbox/v204/loading.gif` - `lightbox/v204/nextlabel.png` - `lightbox/v204/prevlabel.gif` |
| Prototype | - `prototype.js` - `prototype/1.5/prototype.js` - `prototype/1.6/prototype.js` |
| script.aculo.us | - `scriptaculous/1.8.2/scriptaculous.js` - `scriptaculous/1.8.2/builder.js` - `scriptaculous/1.8.2/controls.js` - `scriptaculous/1.8.2/dragdrop.js` - `scriptaculous/1.8.2/effects.js` - `scriptaculous/1.8.2/slider.js` - `scriptaculous/1.8.2/sound.js` - `scriptaculous/1.8.2/unittest.js` |
| Shopify | - `list-collection.css` - `textile.css` |

##### Code

```liquid
{{ 'lightbox.js' | global_asset_url | script_tag }}

{{ 'lightbox.css' | global_asset_url | stylesheet_tag }}
```

##### Output

```html
<script src="//polinas-potent-potions.myshopify.com/cdn/s/global/lightbox.js" type="text/javascript"></script>

<link href="//polinas-potent-potions.myshopify.com/cdn/s/global/lightbox.css" rel="stylesheet" type="text/css" media="all" />
```

</page>

<page>
---
title: 'Liquid filters: handleize'
description: 'Converts a string into a [handle](/docs/api/liquid/basics#handles).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/handleize'
  md: 'https://shopify.dev/docs/api/liquid/filters/handleize.md'
---

# handleize

```oobleck
string | handleize
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string into a [handle](https://shopify.dev/docs/api/liquid/basics#handles).

***

**Note:** The \<code>handleize\</code> filter has an alias of \<code>handle\</code>.

***

##### Code

```liquid
{{ product.title | handleize }}
{{ product.title | handle }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
health-potion
health-potion
```

</page>

<page>
---
title: 'Liquid filters: has'
description: Tests if any item in an array has a specific property value.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/has'
  md: 'https://shopify.dev/docs/api/liquid/filters/has.md'
---

# has

```oobleck
array | has: string, string
```

returns [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

Tests if any item in an array has a specific property value.

This requires you to provide both the property name and the associated value.

##### Code

```liquid
{% assign has_potent_potions = collection.products | has: 'vendor', "Polina's Potent Potions" %}

{{ has_potent_potions }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      }
    ]
  }
}
```

##### Output

```html
true
```

### Returns `false` when no items match the specified property value.

##### Code

```liquid
{% assign has_potent_potions = collection.products | has: 'vendor', "Polina's Potions" %}

{{ has_potent_potions }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Polina's Potent Potions"
      },
      {
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "vendor": "Clover's Apothecary"
      }
    ]
  }
}
```

##### Output

```html
false
```

</page>

<page>
---
title: 'Liquid filters: hex_to_rgba'
description: >-
  Converts a CSS color string from  hexadecimal format to `RGBA` format.
  Shorthand hexadecimal formatting (`hex3`) is also accepted.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/hex_to_rgba'
  md: 'https://shopify.dev/docs/api/liquid/filters/hex_to_rgba.md'
---

# hex\_​to\_​rgba

```oobleck
string | hex_to_rgba
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a CSS color string from hexadecimal format to `RGBA` format. Shorthand hexadecimal formatting (`hex3`) is also accepted.

**Deprecated:**

The `hex_to_rgba` filter has been replaced by [`color_to_rgb`](https://shopify.dev/docs/api/liquid/filters/color_to_rgb) and [`color_modify`](https://shopify.dev/docs/api/liquid/filters/color_modify).

##### Code

```liquid
{{ '#EA5AB9' | hex_to_rgba }}
```

##### Output

```html
rgba(234,90,185,1)
```

### alpha

```oobleck
string | hex_to_rgba: number
```

The default `alpha` value is 1.0. However, you can specify a decimal value between 0.0 and 1.0.

##### Code

```liquid
{{ '#EA5AB9' | hex_to_rgba: 0.5 }}
```

##### Output

```html
rgba(234,90,185,0.5)
```

</page>

<page>
---
title: 'Liquid filters: highlight'
description: >-
  Wraps all instances of a specific string, within a given string, with an HTML
  `<strong>` tag with a `class` attribute

  of `highlight`.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/highlight'
  md: 'https://shopify.dev/docs/api/liquid/filters/highlight.md'
---

# highlight

```oobleck
string | highlight: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Wraps all instances of a specific string, within a given string, with an HTML `<strong>` tag with a `class` attribute of `highlight`.

##### Code

```liquid
{% for item in search.results %}
  {% if item.object_type == 'product' %}
    {{ item.description | highlight: search.terms }}
  {% else %}
    {{ item.content | highlight: search.terms }}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "search": {
    "results": [
      {
        "description": "",
        "object_type": "product"
      },
      {
        "description": "",
        "object_type": "product"
      },
      {
        "description": "",
        "object_type": "product"
      },
      {
        "description": "",
        "object_type": "product"
      },
      {
        "description": "This is a love potion.",
        "object_type": "product"
      },
      {
        "description": "",
        "object_type": "product"
      }
    ],
    "terms": "love"
  }
}
```

##### Output

```html
This is a <strong class="highlight">love</strong> potion.
```

</page>

<page>
---
title: 'Liquid filters: highlight_active_tag'
description: >-
  Wraps a given tag within the [`collection`
  object](https://shopify.dev/docs/api/liquid/objects/collection) in an HTML
  `<span>` tag, with a `class` attribute of `active`, if the tag is currently
  active. Only

  applies to collection tags.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/highlight_active_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/highlight_active_tag.md'
---

# highlight\_​active\_​tag

```oobleck
string | highlight_active_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Wraps a given tag within the [`collection` object](https://shopify.dev/docs/api/liquid/objects/collection) in an HTML `<span>` tag, with a `class` attribute of `active`, if the tag is currently active. Only applies to collection tags.

##### Code

```liquid
{% for tag in collection.all_tags %}
  {{- tag | highlight_active_tag | link_to_tag: tag }}
{% endfor %}
```

##### Data

```json
{
  "collection": {
    "all_tags": [
      "extra-potent",
      "fresh",
      "healing",
      "ingredients"
    ]
  },
  "template": "collection"
}
```

##### Output

```html
<a href="/services/liquid_rendering/extra-potent" title="Show products matching tag extra-potent"><span class="active">extra-potent</span></a>

<a href="/services/liquid_rendering/fresh" title="Show products matching tag fresh">fresh</a>

<a href="/services/liquid_rendering/healing" title="Show products matching tag healing">healing</a>

<a href="/services/liquid_rendering/ingredients" title="Show products matching tag ingredients">ingredients</a>
```

</page>

<page>
---
title: 'Liquid filters: hmac_sha1'
description: >-
  Converts a string into an SHA-1 hash using a hash message authentication code
  (HMAC).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/hmac_sha1'
  md: 'https://shopify.dev/docs/api/liquid/filters/hmac_sha1.md'
---

# hmac\_​sha1

```oobleck
string | hmac_sha1: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string into an SHA-1 hash using a hash message authentication code (HMAC).

The secret key for the message is supplied as a parameter to the filter.

##### Code

```liquid
{%- assign secret_potion = 'Polyjuice' | hmac_sha1: 'Polina' -%}

My secret potion: {{ secret_potion }}
```

##### Output

```html
My secret potion: 63304203b005ea4bc80546f1c6fdfe252d2062b2
```

</page>

<page>
---
title: 'Liquid filters: hmac_sha256'
description: >-
  Converts a string into an SHA-256 hash using a hash message authentication
  code (HMAC).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/hmac_sha256'
  md: 'https://shopify.dev/docs/api/liquid/filters/hmac_sha256.md'
---

# hmac\_​sha256

```oobleck
string | hmac_sha256: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string into an SHA-256 hash using a hash message authentication code (HMAC).

The secret key for the message is supplied as a parameter to the filter.

##### Code

```liquid
{%- assign secret_potion = 'Polyjuice' | hmac_sha256: 'Polina' -%}

My secret potion: {{ secret_potion }}
```

##### Output

```html
My secret potion: 8e0d5d65cff1242a4af66c8f4a32854fd5fb80edcc8aabe9b302b29c7c71dc20
```

</page>

<page>
---
title: 'Liquid filters: image_tag'
description: >-
  Generates an HTML `<img>` tag for a given
  [`image_url`](/docs/api/liquid/filters/image_url).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/image_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/image_tag.md'
---

# image\_​tag

```oobleck
string | image_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<img>` tag for a given [`image_url`](https://shopify.dev/docs/api/liquid/filters/image_url).

By default, `width` and `height` attributes are added to the `<img>` tag based on the dimensions and aspect ratio from the image URL. However, you can override these attributes with the [width](https://shopify.dev/docs/api/liquid/filters/image_tag#image_tag-width) and [height](https://shopify.dev/docs/api/liquid/filters/image_tag#image_tag-height) parameters. If only one parameter is provided, then only that attribute is added.

##### Code

```liquid
{{ product | image_url: width: 200 | image_tag }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200" alt="Health potion" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200 200w" width="200" height="133">
```

## Rendered output

### Lazy loading

If you don't apply the `preload` attribute to `image_tag`, then `loading` is automatically set to `lazy` for images in sections further down the page. You shouldn't lazy load images above the fold. If the default value doesn't work for your theme, then consider writing your own logic using the `section.index` and `section.location` properties. For more information, refer to the [`section` object](https://shopify.dev/docs/api/liquid/objects/section).

### `image_tag` and focal points

This filter automatically applies a focal point to the image using the `object-position` CSS style, if focal point coordinates are available. You can also access an image's focal point coordinates directly through the [`focal_point`](https://shopify.dev/docs/api/liquid/objects/focal_point) object. [Learn how to set a focal point](https://help.shopify.com/manual/online-store/images/theme-images#set-a-focal-point-on-a-theme-image).

##### Code

```liquid
{{ images['potions-header.png'] | image_url: width: 300 | image_tag }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/potions-header.png?v=1650325393&amp;width=300" alt="" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/potions-header.png?v=1650325393&amp;width=300 300w" width="300" height="173" style="object-position:1.9231% 9.7917%;">
```

## Rendered output

### width

```oobleck
image_url | image_tag: width: number
```

Specify the `width` attribute of the `<img>` tag. You can set the parameter to `nil` to prevent the attribute from being added.

##### Code

```liquid
<!-- With a width -->
{{ product | image_url: width: 400 | image_tag: width: 300 }}

<!-- With the width set to nil -->
{{ product | image_url: width: 400 | image_tag: width: nil }}
```

##### Output

```html
<!-- With a width -->
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=400" alt="Health potion" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=300 300w, //polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=352 352w" width="300">

<!-- With the width set to nil -->
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=400" alt="Health potion" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=352 352w">
```

## Rendered output

### height

```oobleck
image_url | image_tag: height: number
```

Specify the `height` attribute of the `<img>` tag. You can set the parameter to `nil` to prevent the attribute from being added.

##### Code

```liquid
<!-- With a height -->
{{ product | image_url: width: 400 | image_tag: height: 300 }}

<!-- With the height set to nil -->
{{ product | image_url: width: 400 | image_tag: height: nil }}
```

##### Output

```html
<!-- With a height -->
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=400" alt="Health potion" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=352 352w" height="300">

<!-- With the height set to nil -->
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=400" alt="Health potion" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=352 352w">
```

## Rendered output

### sizes

```oobleck
image_url | image_tag: sizes: string
```

Specify source sizes with the [HTML `sizes` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attr-sizes).

##### Code

```liquid
{{ product | image_url: width: 200 | image_tag: sizes: '(min-width:1600px) 960px, (min-width: 750px) calc((100vw - 11.5rem) / 2), calc(100vw - 4rem)' }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200" alt="Health potion" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200 200w" width="200" height="133" sizes="(min-width:1600px) 960px, (min-width: 750px) calc((100vw - 11.5rem) / 2), calc(100vw - 4rem)">
```

## Rendered output

### widths

```oobleck
image_url | image_tag: widths: string
```

By default, Shopify generates a `srcset` with a smart set of default widths up to the maximum defined in the image URL. However, you can create your own set of widths.

##### Code

```liquid
{{ product | image_url: width: 600 | image_tag: widths: '200, 300, 400' }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=600" alt="Health potion" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200 200w, //polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=300 300w, //polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=400 400w" width="600" height="400">
```

## Rendered output

### srcset

```oobleck
image_url | image_tag: srcset: string
```

By default, Shopify generates a `srcset`. However, you can create your own `srcset`. The `srcset` parameter takes precedence over the [`width` parameter](https://shopify.dev/docs/api/liquid/filters/image_tag#image_tag-width). You shouldn't to use the `srcset` parameter unless you want to remove the attribute by setting the parameter to `nil`.

##### Code

```liquid
{{ product | image_url: width: 200 | image_tag: srcset: nil }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200" alt="Health potion" width="200" height="133">
```

## Rendered output

### preload

```oobleck
image_url | image_tag: preload: boolean
```

Specify whether the image should be preloaded.

When `preload` is set to `true`, a resource hint is sent as a [Link HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Link) with a `rel` value of [`preload`](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preload). The Link header also includes `imagesrcset` and `imagesizes` that match the `srcset` and `sizes` attribute of the tag, where present:

```liquid
Link: <IMAGE_URL>; rel=preload; as=image
Link: <IMAGE_URL>; rel=preload; as=image; imagesrcset=ADDITIONAL_IMAGE_URL 352w; imagesizes=40vw
```

```liquid
Link: 
```

This option doesn't affect the HTML img tag directly.

You should use the preload parameter sparingly. For example, consider preloading only above-the-fold images. To learn more about resource hints in Shopify themes, refer to [Performance best practices for Shopify themes](https://shopify.dev/themes/best-practices/performance#preload-key-resources-defer-or-avoid-loading-others).

### alt

```oobleck
image_url | image_tag: alt: string
```

By default, the `alt` attribute of the `<img>` tag is set to the [media alt text](https://help.shopify.com/manual/products/product-media/add-alt-text), or the resource title for article, collection, line item, product, and variant images. However, you can override this default, or set the value if there's no default.

##### Code

```liquid
{{ product | image_url: width: 200 | image_tag: alt: "My image's alt text" }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200" alt="My image&#39;s alt text" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200 200w" width="200" height="133">
```

## Rendered output

### HTML attributes

```oobleck
image_url | image_tag: attribute: string
```

You can specify [HTML attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attributes) by adding a parameter that matches the attribute name, and the desired value.

##### Code

```liquid
{{ product | image_url: width: 200 | image_tag: class: 'custom-class', loading: 'lazy' }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200" alt="Health potion" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&amp;width=200 200w" width="200" height="133" loading="lazy" class="custom-class">
```

## Rendered output

</page>

<page>
---
title: 'Liquid filters: image_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for an image.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/image_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/image_url.md'
---

# image\_​url

```oobleck
variable | image_url: width: number, height: number
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for an image.

You can use the `image_url` filter on the following objects, as well as their `src` property:

* [`article`](https://shopify.dev/docs/api/liquid/objects/article)
* [`collection`](https://shopify.dev/docs/api/liquid/objects/collection)
* [`image`](https://shopify.dev/docs/api/liquid/objects/image)
* [`line_item`](https://shopify.dev/docs/api/liquid/objects/line_item)
* [`product`](https://shopify.dev/docs/api/liquid/objects/product)
* [`variant`](https://shopify.dev/docs/api/liquid/objects/variant)
* [`country`](https://shopify.dev/docs/api/liquid/objects/country)

***

**Caution:** You need to specify either a \<a href="/docs/api/liquid/filters/image\_url#image\_url-width">\<code>width\</code>\</a> or \<a href="/docs/api/liquid/filters/image\_url#image\_url-height">\<code>height\</code>\</a> parameter. If neither are specified, then an error is returned.

***

***

**Note:** Regardless of the specified dimensions, an image can never be resized to be larger than its original dimensions.

***

##### Code

```liquid
{{ product | image_url: width: 450 }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&width=450
```

### width

```oobleck
variable | image_url: width: number
```

Specify the width of the image up to a maximum of `5760px`. If only the width is specified, then the height is automatically calculated based on the image's dimensions.

##### Code

```liquid
{{ product | image_url: width: 450 }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?v=1683744744&width=450
```

### height

```oobleck
variable | image_url: height: number
```

Specify the height of the image up to a maximum of `5760px`. If only the height is specified, then the width is automatically calculated based on the image's dimensions.

##### Code

```liquid
{{ product | image_url: height: 450 }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?height=450&v=1683744744
```

### crop

```oobleck
variable | image_url: crop: string
```

Specify which part of the image to show if the specified dimensions result in an aspect ratio that differs from the original. You can use the following values:

* `top`
* `center`
* `bottom`
* `left`
* `right`
* `region`

The default value is `center`.

When using the `region` crop mode, the starting point for the crop is defined by `crop_left` and `crop_top` and extends to the `crop_width` and `crop_height`. Optionally, to resize the region extracted by the crop, use the `width` and `height` parameters.

***

**Note:** Country flags are SVG images and can only be cropped if a value for \<code>format\</code> is also provided.

***

##### Code

```liquid
{{ product | image_url: width: 400, height: 400, crop: 'bottom' }}

{{ product | image_url: crop: 'region', crop_left: 32, crop_top: 32, crop_width: 512, crop_height: 512 }}

{{ product | image_url: crop: 'region', width: 100, height: 100, crop_left: 32, crop_top: 32, crop_width: 512, crop_height: 512 }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?crop=bottom&height=400&v=1683744744&width=400

//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?crop=region&crop_height=512&crop_left=32&crop_top=32&crop_width=512&v=1683744744

//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?crop=region&crop_height=512&crop_left=32&crop_top=32&crop_width=512&height=100&v=1683744744&width=100
```

### format

```oobleck
variable | image_url: format: string
```

Specify which file format to use for the image. The valid formats are `pjpg` and `jpg`.

It's not practical to convert a lossy image format, like `jpg`, to a lossless image format, like `png`, so Shopify can do only the following conversions:

* `png` to `jpg`
* `png` to `pjpg`
* `jpg` to `pjpg`

***

**Note:** Shopify automatically detects which image formats are supported by the client (e.g. \<code>\<span class="PreventFireFoxApplyingGapToWBR">Web\<wbr/>P\</span>\</code>, \<code>\<span class="PreventFireFoxApplyingGapToWBR">A\<wbr/>V\<wbr/>I\<wbr/>F\</span>\</code>, etc.) and selects a file format for optimal quality and file size. When a format is specified, Shopify takes into account the features (e.g. progressive, alpha channel) of the specified file format when making the final automatic format selection. To learn more, visit \<a href="https://cdn.shopify.com/">https://cdn.shopify.com/\</a>.

***

##### Code

```liquid
{{ product | image_url: width: 450, format: 'pjpg' }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?format=pjpg&v=1683744744&width=450
```

### pad\_color

```oobleck
variable | image_url: pad_color: string
```

Specify a color to pad the image if the specified dimensions result in an aspect ratio that differs from the original. The color must be in hexadecimal format (`hex3` or `hex6`).

##### Code

```liquid
{{ product | image_url: width: 400, height: 400, pad_color: '000' }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new.jpg?height=400&pad_color=000&v=1683744744&width=400
```

</page>

<page>
---
title: 'Liquid filters: img_tag'
description: Generates an HTML `<img>` tag for a given image URL.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/img_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/img_tag.md'
---

# img\_​tag

```oobleck
string | img_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<img>` tag for a given image URL.

You can also use the `img_tag` filter on the following objects:

* [`article`](https://shopify.dev/docs/api/liquid/objects/article)
* [`collection`](https://shopify.dev/docs/api/liquid/objects/collection)
* [`image`](https://shopify.dev/docs/api/liquid/objects/image)
* [`line_item`](https://shopify.dev/docs/api/liquid/objects/line_item)
* [`product`](https://shopify.dev/docs/api/liquid/objects/product)
* [`variant`](https://shopify.dev/docs/api/liquid/objects/variant)

**Deprecated:**

The `img_tag` filter has been replaced by [`image_tag`](https://shopify.dev/docs/api/liquid/filters/image_tag).

##### Code

```liquid
{{ product | img_tag }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_small.jpg?v=1683744744" alt="" />
```

### Optional parameters

```oobleck
variable | img_tag: string, string, string
```

The `img_tag` filter accepts 3 unnamed parameters, separated by commas, to specify the `alt` and `class` attributes, and the [size](https://shopify.dev/docs/api/liquid/filters/img_url#img_url-size) of the image. Because the parameters are read in that order, you must include a value for each parameter before the last parameter you want to specify. If you don't want to include a parameter that precedes one that you do want to include, then you can set the value to an empty string.

***

**Note:** The \<code>size\</code> attribute of the \<code>\<span class="PreventFireFoxApplyingGapToWBR">img\<wbr/>\_tag\</span>\</code> filter can\&#39;t be used in conjunction with the \<a href="/docs/api/liquid/filters/img\_url">\<code>\<span class="PreventFireFoxApplyingGapToWBR">img\<wbr/>\_url\</span>\</code> filter\</a>. If both are used, then the \<code>\<span class="PreventFireFoxApplyingGapToWBR">img\<wbr/>\_url\</span>\</code> filter will override the \<code>size\</code> parameter of the \<code>\<span class="PreventFireFoxApplyingGapToWBR">img\<wbr/>\_tag\</span>\</code> filter.

***

##### Code

```liquid
{{ product | img_tag: 'image alt text', '', '450x450' }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_450x450.jpg?v=1683744744" alt="image alt text" class="" />
```

</page>

<page>
---
title: 'Liquid filters: img_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for an image.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/img_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/img_url.md'
---

# img\_​url

```oobleck
variable | img_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for an image.

You can use the `img_url` filter on the following objects:

* [`article`](https://shopify.dev/docs/api/liquid/objects/article)
* [`collection`](https://shopify.dev/docs/api/liquid/objects/collection)
* [`image`](https://shopify.dev/docs/api/liquid/objects/image)
* [`line_item`](https://shopify.dev/docs/api/liquid/objects/line_item)
* [`product`](https://shopify.dev/docs/api/liquid/objects/product)
* [`variant`](https://shopify.dev/docs/api/liquid/objects/variant)

**Deprecated:**

The `img_url` filter has been replaced by [`image_url`](https://shopify.dev/docs/api/liquid/filters/image_url).

##### Code

```liquid
{{ product | img_url }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_small.jpg?v=1683744744
```

### size

```oobleck
variable | img_url: string
```

The size parameter allows you to specify the dimensions of the image up to a maximum of 5760 x 5760 px. You can specify only the width, only the height, or both, and you can also use the following named sizes:

| Name | Dimensions |
| - | - |
| `pico` | `16x16 px` |
| `icon` | `32x32 px` |
| `thumb` | `50x50 px` |
| `small` | `100x100 px` |
| `compact` | `160x160 px` |
| `medium` | `240x240 px` |
| `large` | `480x480 px` |
| `grande` | `600x600 px` |
| `original` `master` | `1024x1024 px` |

##### Code

```liquid
{{ product | img_url: '480x' }}

{{ product | img_url: 'x480' }}

{{ product | img_url: '480x480' }}

{{ product | img_url: 'large' }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_480x.jpg?v=1683744744

//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_x480.jpg?v=1683744744

//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_480x480.jpg?v=1683744744

//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_large.jpg?v=1683744744
```

### crop

```oobleck
variable | img_url: crop: string
```

The `crop` parameter allows you to specify which part of the image to show if the specified dimensions result in an aspect ratio that differs from the original. You can use the following values:

* `top`
* `center`
* `bottom`
* `left`
* `right`

The default value is `center`.

##### Code

```liquid
{{ product | img_url: crop: 'bottom' }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_small.jpg?v=1683744744
```

### format

```oobleck
variable | img_url: format: string
```

Specify which file format to use for the image. The valid formats are `pjpg` and `jpg`.

It's not practical to convert a lossy image format, like `jpg`, to a lossless image format, like `png`, so this filter does only the following conversions:

* `png` to `jpg`
* `png` to `pjpg`
* `jpg` to `pjpg`

***

**Note:** Shopify automatically detects which image formats are supported by the client (e.g. \<code>\<span class="PreventFireFoxApplyingGapToWBR">Web\<wbr/>P\</span>\</code>, \<code>\<span class="PreventFireFoxApplyingGapToWBR">A\<wbr/>V\<wbr/>I\<wbr/>F\</span>\</code>, etc.) and selects a file format for optimal quality and file size. When a format is specified, Shopify takes into account the features (e.g. progressive, alpha channel) of the specified file format when making the final automatic format selection. To learn more, visit \<a href="https://cdn.shopify.com/">https://cdn.shopify.com/\</a>.

***

##### Code

```liquid
{{ product | img_url: format: 'pjpg' }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_small.jpg?v=1683744744
```

### scale

```oobleck
variable | img_url: scale: number
```

Specify the pixel density of the image. The valid densities are 2 and 3.

##### Code

```liquid
{{ product | img_url: scale: 2 }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_small.jpg?v=1683744744
```

</page>

<page>
---
title: 'Liquid filters: inline_asset_content'
description: >-
  Outputs the content of an asset inline in the template. The asset must be
  either a SVG, JS, or CSS file.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/inline_asset_content'
  md: 'https://shopify.dev/docs/api/liquid/filters/inline_asset_content.md'
---

# inline\_​asset\_​content

```oobleck
asset_name | inline_asset_content
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Outputs the content of an asset inline in the template. The asset must be either a SVG, JS, or CSS file.

***

**Note:** The asset size must be less than 15KB to be inlined.

***

##### Code

```liquid
{{ 'icon.svg' | inline_asset_content }}
```

##### Output

```html
'<svg xmlns="http://www.w3.org/2000/svg"/>'
```

</page>

<page>
---
title: 'Liquid filters: item_count_for_variant'
description: >-
  Returns the total item count for a specified variant in the
  [`cart`](https://shopify.dev/docs/api/liquid/objects/cart) object.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/item_count_for_variant'
  md: 'https://shopify.dev/docs/api/liquid/filters/item_count_for_variant.md'
---

# item\_​count\_​for\_​variant

```oobleck
cart | item_count_for_variant: {variant_id}
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Returns the total item count for a specified variant in the [`cart`](https://shopify.dev/docs/api/liquid/objects/cart) object.

##### Code

```liquid
{{ cart | item_count_for_variant: 39888235757633 }}
```

##### Output

```html
1
```

</page>

<page>
---
title: 'Liquid filters: join'
description: >-
  Combines all of the items in an array into a single string, separated by a
  space.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/join'
  md: 'https://shopify.dev/docs/api/liquid/filters/join.md'
---

# join

```oobleck
array | join
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Combines all of the items in an array into a single string, separated by a space.

##### Code

```liquid
{{ collection.all_tags | join }}
```

##### Data

```json
{
  "collection": {
    "all_tags": [
      "extra-potent",
      "fresh",
      "healing",
      "ingredients"
    ]
  }
}
```

##### Output

```html
extra-potent fresh healing ingredients
```

### Custom separator

```oobleck
array | join: string
```

You can specify a custom separator for the joined items.

##### Code

```liquid
{{ collection.all_tags | join: ', ' }}
```

##### Data

```json
{
  "collection": {
    "all_tags": [
      "extra-potent",
      "fresh",
      "healing",
      "ingredients"
    ]
  }
}
```

##### Output

```html
extra-potent, fresh, healing, ingredients
```

</page>

<page>
---
title: 'Liquid filters: json'
description: 'Converts a string, or object, into JSON format.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/json'
  md: 'https://shopify.dev/docs/api/liquid/filters/json.md'
---

# json

```oobleck
variable | json
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string, or object, into JSON format.

***

**Tip:** When using the JSON output in JavaScript, you don\&#39;t need to wrap it in quotes because the \<code>json\</code> filter includes them. The \<code>json\</code> filter also escapes any quotes inside the output.

***

#### Product inventory

When applied to a [`product` object](https://shopify.dev/docs/api/liquid/objects/product) on any Shopify store created after December 5, 2017, the `json` filter doesn't output values for the `inventory_quantity` and `inventory_policy` properties of any associated [variants](https://shopify.dev/docs/api/liquid/objects/variant). These properties are excluded to help prevent bots and crawlers from retrieving inventory quantities for stores to which they aren't granted access.

If you need inventory information, you can access it through individual variants.

##### Code

```liquid
{{ product | json }}
```

##### Output

```html
{"id":6792602320961,"title":"Crocodile tears","handle":"crocodile-tears","description":"","published_at":"2022-04-22T11:55:58-04:00","created_at":"2022-04-22T11:55:56-04:00","vendor":"Polina's Potent Potions","type":"","tags":["Salty"],"price":5600,"price_min":5600,"price_max":5600,"available":false,"price_varies":false,"compare_at_price":null,"compare_at_price_min":0,"compare_at_price_max":0,"compare_at_price_varies":false,"variants":[{"id":39888242344001,"title":"Default Title","option1":"Default Title","option2":null,"option3":null,"sku":"","requires_shipping":true,"taxable":true,"featured_image":null,"available":false,"name":"Crocodile tears","public_title":null,"options":["Default Title"],"price":5600,"weight":0,"compare_at_price":null,"inventory_management":"shopify","barcode":"","requires_selling_plan":false,"selling_plan_allocations":[],"quantity_rule":{"min":1,"max":null,"increment":1}}],"images":["\/\/polinas-potent-potions.myshopify.com\/cdn\/shop\/products\/amber-beard-oil-bottle.jpg?v=1650642958"],"featured_image":"\/\/polinas-potent-potions.myshopify.com\/cdn\/shop\/products\/amber-beard-oil-bottle.jpg?v=1650642958","options":["Title"],"media":[{"alt":null,"id":21772501975105,"position":1,"preview_image":{"aspect_ratio":1.5,"height":2974,"width":4460,"src":"\/\/polinas-potent-potions.myshopify.com\/cdn\/shop\/products\/amber-beard-oil-bottle.jpg?v=1650642958"},"aspect_ratio":1.5,"height":2974,"media_type":"image","src":"\/\/polinas-potent-potions.myshopify.com\/cdn\/shop\/products\/amber-beard-oil-bottle.jpg?v=1650642958","width":4460}],"requires_selling_plan":false,"selling_plan_groups":[],"content":""}
```

</page>

<page>
---
title: 'Liquid filters: last'
description: Returns the last item in an array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/last'
  md: 'https://shopify.dev/docs/api/liquid/filters/last.md'
---

# last

```oobleck
array | last
```

Returns the last item in an array.

##### Code

```liquid
{%- assign last_product = collection.products | last -%}

{{ last_product.title }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower"
      },
      {
        "title": "Charcoal"
      },
      {
        "title": "Crocodile tears"
      },
      {
        "title": "Dandelion milk"
      },
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Dried chamomile"
      },
      {
        "title": "Forest mushroom"
      },
      {
        "title": "Gift Card"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Ground mandrake root"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      },
      {
        "title": "Komodo dragon scale"
      },
      {
        "title": "Love Potion"
      },
      {
        "title": "Mana potion"
      },
      {
        "title": "Potion beats"
      },
      {
        "title": "Potion bottle"
      },
      {
        "title": "Viper venom"
      },
      {
        "title": "Whole bloodroot"
      }
    ]
  }
}
```

##### Output

```html
Whole bloodroot
```

### Dot notation

You can use the `last` filter with dot notation when you need to use it inside a tag or object output.

##### Code

```liquid
{{ collection.products.last.title }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower"
      },
      {
        "title": "Charcoal"
      },
      {
        "title": "Crocodile tears"
      },
      {
        "title": "Dandelion milk"
      },
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Dried chamomile"
      },
      {
        "title": "Forest mushroom"
      },
      {
        "title": "Gift Card"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Ground mandrake root"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      },
      {
        "title": "Komodo dragon scale"
      },
      {
        "title": "Love Potion"
      },
      {
        "title": "Mana potion"
      },
      {
        "title": "Potion beats"
      },
      {
        "title": "Potion bottle"
      },
      {
        "title": "Viper venom"
      },
      {
        "title": "Whole bloodroot"
      }
    ]
  }
}
```

##### Output

```html
Whole bloodroot
```

</page>

<page>
---
title: 'Liquid filters: line_items_for'
description: >-
  Returns the subset of
  [`cart`](https://shopify.dev/docs/api/liquid/objects/cart) line items that
  include a specified product or variant.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/line_items_for'
  md: 'https://shopify.dev/docs/api/liquid/filters/line_items_for.md'
---

# line\_​items\_​for

```oobleck
cart | line_items_for: object
```

returns array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

Returns the subset of [`cart`](https://shopify.dev/docs/api/liquid/objects/cart) line items that include a specified product or variant.

Accepts the following object types:

* `product`
* `variant`

##### Code

```liquid
{% assign product = all_products['bloodroot-whole'] %}
{% assign line_items = cart | line_items_for: product %}

Total cart quantity for product: {{ line_items | sum: 'quantity' }}
```

##### Data

```json
{
  "all_products": {
    "bloodroot-whole": {}
  }
}
```

##### Output

```html
Total cart quantity for product: 1
```

##### Code

```liquid
{% assign product = all_products['bloodroot-whole'] %}
{% assign variant = product.variants.first %}
{% assign line_items = cart | line_items_for: variant %}

Total cart quantity for variant: {{ line_items | sum: 'quantity' }}
```

##### Data

```json
{
  "all_products": {
    "bloodroot-whole": {
      "variants": []
    }
  },
  "product": {
    "variants": []
  }
}
```

##### Output

```html
Total cart quantity for variant: 1
```

</page>

<page>
---
title: 'Liquid filters: link_to'
description: Generates an HTML `<a>` tag.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/link_to'
  md: 'https://shopify.dev/docs/api/liquid/filters/link_to.md'
---

# link\_​to

```oobleck
string | link_to: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<a>` tag.

##### Code

```liquid
{{ 'Shopify' | link_to: 'https://www.shopify.com' }}
```

##### Output

```html
<a href="https://www.shopify.com" title="" rel="nofollow">Shopify</a>
```

### HTML attributes

```oobleck
string | link_to_type: attribute: string
```

You can specify [HTML attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attributes) by including a parameter that matches the attribute name, and the desired value.

##### Code

```liquid
{{ 'Shopify' | link_to: 'https://www.shopify.com', class: 'link-class' }}
```

##### Output

```html
<a class="link-class" href="https://www.shopify.com" rel="nofollow">Shopify</a>
```

</page>

<page>
---
title: 'Liquid filters: link_to_add_tag'
description: >-
  Generates an HTML `<a>` tag with an `href` attribute linking to the current
  blog or collection, filtered to show

  only articles or products that have a given tag, as well as any currently
  active tags.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/link_to_add_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/link_to_add_tag.md'
---

# link\_​to\_​add\_​tag

```oobleck
string | link_to_add_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<a>` tag with an `href` attribute linking to the current blog or collection, filtered to show only articles or products that have a given tag, as well as any currently active tags.

***

**Tip:** To learn more about filtering by tag, refer to \<a href="/themes/architecture/templates/blog#filter-articles-by-tag">Filter articles by tag\</a> or \<a href="/themes/navigation-search/filtering/tag-filtering">Filter collections by tag\</a>.

***

##### Code

```liquid
{% for tag in collection.all_tags %}
  {%- if current_tags contains tag -%}
    {{ tag }}
  {%- else -%}
    {{ tag | link_to_add_tag: tag }}
  {%- endif -%}
{% endfor %}
```

##### Data

```json
{
  "collection": {
    "all_tags": [
      "extra-potent",
      "fresh",
      "healing",
      "ingredients"
    ]
  },
  "template": "collection"
}
```

##### Output

```html
<a href="/services/liquid_rendering/extra-potent" title="Narrow selection to products matching tag extra-potent">extra-potent</a>

<a href="/services/liquid_rendering/fresh" title="Narrow selection to products matching tag fresh">fresh</a>

<a href="/services/liquid_rendering/healing" title="Narrow selection to products matching tag healing">healing</a>

<a href="/services/liquid_rendering/ingredients" title="Narrow selection to products matching tag ingredients">ingredients</a>
```

</page>

<page>
---
title: 'Liquid filters: link_to_remove_tag'
description: >-
  Generates an HTML `<a>` tag with an `href` attribute linking to the current
  blog or collection, filtered to show

  only articles or products that have any currently active tags, except the
  provided tag.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/link_to_remove_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/link_to_remove_tag.md'
---

# link\_​to\_​remove\_​tag

```oobleck
string | link_to_remove_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<a>` tag with an `href` attribute linking to the current blog or collection, filtered to show only articles or products that have any currently active tags, except the provided tag.

***

**Tip:** To learn more about filtering by tag, refer to \<a href="/themes/architecture/templates/blog#filter-articles-by-tag">Filter articles by tag\</a> or \<a href="/themes/navigation-search/filtering/tag-filtering">Filter collections by tag\</a>.

***

##### Code

```liquid
{% for tag in collection.all_tags %}
  {%- if current_tags contains tag -%}
    {{ tag | link_to_remove_tag: tag }}
  {%- else -%}
    {{ tag | link_to_add_tag: tag }}
  {%- endif -%}
{% endfor %}
```

##### Data

```json
{
  "collection": {
    "all_tags": [
      "extra-potent",
      "fresh",
      "healing",
      "ingredients"
    ]
  },
  "template": "collection"
}
```

##### Output

```html
<a href="/services/liquid_rendering/extra-potent" title="Narrow selection to products matching tag extra-potent">extra-potent</a>

<a href="/services/liquid_rendering/fresh" title="Narrow selection to products matching tag fresh">fresh</a>

<a href="/services/liquid_rendering/healing" title="Narrow selection to products matching tag healing">healing</a>

<a href="/services/liquid_rendering/ingredients" title="Narrow selection to products matching tag ingredients">ingredients</a>
```

</page>

<page>
---
title: 'Liquid filters: link_to_tag'
description: >-
  Generates an HTML `<a>` tag with an `href` attribute linking to the current
  blog or collection, filtered to show

  only articles or products that have a given tag.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/link_to_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/link_to_tag.md'
---

# link\_​to\_​tag

```oobleck
string | link_to_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<a>` tag with an `href` attribute linking to the current blog or collection, filtered to show only articles or products that have a given tag.

***

**Tip:** To learn more about filtering by tag, refer to \<a href="/themes/architecture/templates/blog#filter-articles-by-tag">Filter articles by tag\</a> or \<a href="/themes/navigation-search/filtering/tag-filtering">Filter collections by tag\</a>.

***

##### Code

```liquid
{% for tag in collection.all_tags %}
  {{- tag | link_to_tag: tag }}
{% endfor %}
```

##### Data

```json
{
  "collection": {
    "all_tags": [
      "extra-potent",
      "fresh",
      "healing",
      "ingredients"
    ]
  },
  "template": "collection"
}
```

##### Output

```html
<a href="/services/liquid_rendering/extra-potent" title="Show products matching tag extra-potent">extra-potent</a>

<a href="/services/liquid_rendering/fresh" title="Show products matching tag fresh">fresh</a>

<a href="/services/liquid_rendering/healing" title="Show products matching tag healing">healing</a>

<a href="/services/liquid_rendering/ingredients" title="Show products matching tag ingredients">ingredients</a>
```

</page>

<page>
---
title: 'Liquid filters: link_to_type'
description: >-
  Generates an HTML `<a>` tag with an `href` attribute linking to a [collection
  page](https://shopify.dev/docs/storefronts/themes/architecture/templates/collection)
  that lists all products of the given

  product type.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/link_to_type'
  md: 'https://shopify.dev/docs/api/liquid/filters/link_to_type.md'
---

# link\_​to\_​type

```oobleck
string | link_to_type
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<a>` tag with an `href` attribute linking to a [collection page](https://shopify.dev/docs/storefronts/themes/architecture/templates/collection) that lists all products of the given product type.

##### Code

```liquid
{{ 'Health' | link_to_type }}
```

##### Output

```html
<a href="/collections/types?q=Health" title="Health">Health</a>
```

### HTML attributes

```oobleck
string | link_to_type: attribute: string
```

You can specify [HTML attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attributes) by including a parameter that matches the attribute name, and the desired value.

##### Code

```liquid
{{ 'Health' | link_to_type: class: 'link-class' }}
```

##### Output

```html
<a class="link-class" href="/collections/types?q=Health" title="Health">Health</a>
```

</page>

<page>
---
title: 'Liquid filters: link_to_vendor'
description: >-
  Generates an HTML `<a>` tag with an `href` attribute linking to a [collection
  page](https://shopify.dev/docs/storefronts/themes/architecture/templates/collection)
  that lists all products of a given

  product vendor.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/link_to_vendor'
  md: 'https://shopify.dev/docs/api/liquid/filters/link_to_vendor.md'
---

# link\_​to\_​vendor

```oobleck
string | link_to_vendor
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<a>` tag with an `href` attribute linking to a [collection page](https://shopify.dev/docs/storefronts/themes/architecture/templates/collection) that lists all products of a given product vendor.

##### Code

```liquid
{{ "Polina's Potent Potions" | link_to_vendor }}
```

##### Output

```html
<a href="/collections/vendors?q=Polina%27s%20Potent%20Potions" title="Polina&#39;s Potent Potions">Polina's Potent Potions</a>
```

### HTML attributes

```oobleck
string | link_to_vendor: attribute: string
```

You can specify [HTML attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attributes) by including a parameter that matches the attribute name, and the desired value.

##### Code

```liquid
{{ "Polina's Potent Potions" | link_to_vendor: class: 'link-class' }}
```

##### Output

```html
<a class="link-class" href="/collections/vendors?q=Polina%27s%20Potent%20Potions" title="Polina&#39;s Potent Potions">Polina's Potent Potions</a>
```

</page>

<page>
---
title: 'Liquid filters: login_button'
description: >-
  Generates an HTML Button that enables a customer to either sign in to the
  storefront using their Shop account or follow the shop in the Shop App.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/login_button'
  md: 'https://shopify.dev/docs/api/liquid/filters/login_button.md'
---

# login\_​button

```oobleck
shop | login_button
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML Button that enables a customer to either sign in to the storefront using their Shop account or follow the shop in the Shop App.

***

**Note:** The presence of the \<a href="/docs/api/liquid/objects/shop">shop\</a> object is required for validation purposes only.

***

### action

```oobleck
shop | login_button: action: string
```

Controls the behavior of the button after authentication.

Accepts the following values:

* `default` - Authentication only
* `follow` - Performs a side-effect after authentication which follows the current shop in the Shop app. Requires additional configuration. [Learn more](https://help.shopify.com/manual/online-store/themes/customizing-themes/follow-on-shop)

```liquid
{{ shop | login_button: action: 'follow' }}
```

```liquid
{{ shop | login_button: action: 'follow' }}
```

</page>

<page>
---
title: 'Liquid filters: lstrip'
description: Strips all whitespace from the left of a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/lstrip'
  md: 'https://shopify.dev/docs/api/liquid/filters/lstrip.md'
---

# lstrip

```oobleck
string | lstrip
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Strips all whitespace from the left of a string.

##### Code

```liquid
{%- assign text = '  Some potions create whitespace.      ' -%}

"{{ text }}"
"{{ text | lstrip }}"
```

##### Output

```html
"  Some potions create whitespace.      "
"Some potions create whitespace.      "
```

</page>

<page>
---
title: 'Liquid filters: map'
description: Creates an array of values from a specific property of the items in an array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/map'
  md: 'https://shopify.dev/docs/api/liquid/filters/map.md'
---

# map

```oobleck
array | map: string
```

Creates an array of values from a specific property of the items in an array.

##### Code

```liquid
{%- assign product_titles = collection.products | map: 'title' -%}

{{ product_titles | join: ', ' }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
Draught of Immortality, Glacier ice, Health potion, Invisibility potion
```

</page>

<page>
---
title: 'Liquid filters: md5'
description: >-
  Converts a string into an MD5 hash. MD5 is not considered safe anymore. Please
  use ['blake3'](https://shopify.dev/docs/api/liquid/filters/blake3) instead for
  better security and performance.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/md5'
  md: 'https://shopify.dev/docs/api/liquid/filters/md5.md'
---

# md5

```oobleck
string | md5
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string into an MD5 hash. MD5 is not considered safe anymore. Please use ['blake3'](https://shopify.dev/docs/api/liquid/filters/blake3) instead for better security and performance.

##### Code

```liquid
{{ '' | md5 }}
```

##### Output

```html
d41d8cd98f00b204e9800998ecf8427e
```

</page>

<page>
---
title: 'Liquid filters: media_tag'
description: Generates an appropriate HTML tag for a given media object.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/media_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/media_tag.md'
---

# media\_​tag

```oobleck
media | media_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an appropriate HTML tag for a given media object.

##### Code

```liquid
{% for media in product.media %}
  {{- media | media_tag }}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": []
  }
}
```

##### Output

```html
<iframe frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="allowfullscreen" src="https://www.youtube.com/embed/vj01PAffOac?controls=1&amp;enablejsapi=1&amp;modestbranding=1&amp;origin=https%3A%2F%2Fpolinas-potent-potions.myshopify.com&amp;playsinline=1&amp;rel=0" title="Potion beats"></iframe>

<video playsinline="playsinline" controls="controls" preload="metadata" aria-label="Potion beats" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_small.jpg?v=1655255324"><source src="//polinas-potent-potions.myshopify.com/cdn/shop/videos/c/vp/4edc28a708b7405093a927cebe794f1a/4edc28a708b7405093a927cebe794f1a.HD-1080p-7.2Mbps.mp4?v=0" type="video/mp4"><img src="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_small.jpg?v=1655255324"></video>
```

### image\_size

```oobleck
media | media_tag: image_size: string
```

Specify the dimensions of the media's poster image in pixels.

##### Code

```liquid
{% for media in product.media %}
  {{- media | media_tag: image_size: '400x' }}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": []
  }
}
```

##### Output

```html
<iframe frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="allowfullscreen" image_size="400x" src="https://www.youtube.com/embed/vj01PAffOac?controls=1&amp;enablejsapi=1&amp;modestbranding=1&amp;origin=https%3A%2F%2Fpolinas-potent-potions.myshopify.com&amp;playsinline=1&amp;rel=0" title="Potion beats"></iframe>

<video playsinline="playsinline" controls="controls" preload="metadata" aria-label="Potion beats" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_400x.jpg?v=1655255324"><source src="//polinas-potent-potions.myshopify.com/cdn/shop/videos/c/vp/4edc28a708b7405093a927cebe794f1a/4edc28a708b7405093a927cebe794f1a.HD-1080p-7.2Mbps.mp4?v=0" type="video/mp4"><img src="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_400x.jpg?v=1655255324"></video>
```

</page>

<page>
---
title: 'Liquid filters: metafield_tag'
description: >-
  Generates an HTML element to host the data from a [`metafield`
  object](https://shopify.dev/docs/api/liquid/objects/metafield).

  The type of element that's generated differs depending on the type of
  metafield.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/metafield_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/metafield_tag.md'
---

# metafield\_​tag

```oobleck
metafield | metafield_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML element to host the data from a [`metafield` object](https://shopify.dev/docs/api/liquid/objects/metafield). The type of element that's generated differs depending on the type of metafield.

***

**Note:** The \<code>\<span class="PreventFireFoxApplyingGapToWBR">metafield\<wbr/>\_tag\</span>\</code> filter doesn\&#39;t currently support list metafields other than \<code>\<span class="PreventFireFoxApplyingGapToWBR">list.single\<wbr/>\_line\<wbr/>\_text\<wbr/>\_field\</span>\</code> and \<code>\<span class="PreventFireFoxApplyingGapToWBR">list.metaobject\<wbr/>\_reference\</span>\</code>.

***

### Basic types

Most metafield types return a simple HTML element:

| Type | Element | Attributes |
| - | - | - |
| `boolean` | `<span>` | `class="metafield-boolean"` |
| `collection_reference` | `<a>` | `class="metafield-collection_reference"` |
| `color` | `<span>` | `class="metafield-color"` |
| `date` | `<time>` | `datetime="&lt;the metafield value&gt;"` `class="metafield-date"` Value is localized to the customer |
| `date_time` | `<time>` | `datetime="&lt;the metafield value&gt;"` `class="metafield-date"` Value is localized to the customer |
| `json` | `<script>` | `type="application/json"`\<br>\<br>`class="metafield-json"` |
| `money` | `<span>` | `class="metafield-money"` Value is formatted using the store's [HTML with currency setting](https://help.shopify.com/manual/payments/currency-formatting) |
| `multi_line_text_field` | `<span>` | `class="metafield-multi_line_text_field"` |
| `number_decimal` | `<span>` | `class="metafield-number_decimal"` |
| `number_integer` | `<span>` | `class="metafield-number_integer"` |
| `page_reference` | `<a>` | `class="metafield-page_reference"` |
| `product_reference` | `<a>` | `class="metafield-page_reference"` |
| `rating` | `<span>` | `class="metafield-rating"` |
| `single_line_text_field` | `<span>` | `class="metafield-single_line_text_field"` |
| `url` | `<a>` | `class="metafield-url"` |
| `variant_reference` | `<a>` | `class="metafield-variant_reference"` |
| `rich_text_field` | `<div>` | `class="metafield-rich_text_field"` |

##### Code

```liquid
<!-- boolean -->
{{ product.metafields.information.seasonal | metafield_tag }}

<!-- collection_reference -->
{{ product.metafields.information.related_collection | metafield_tag }}

<!-- color -->
{{ product.metafields.details.potion_color | metafield_tag }}

<!-- date -->
{{ product.metafields.information.expiry | metafield_tag }}

<!-- date_time -->
{{ product.metafields.information.brew_date | metafield_tag }}

<!-- json -->
{{ product.metafields.information.burn_temperature | metafield_tag }}

<!-- money -->
{{ product.metafields.details.price_per_ml | metafield_tag }}

<!-- multi_line_text_field -->
{{ product.metafields.information.shipping | metafield_tag }}

<!-- number_decimal -->
{{ product.metafields.information.salinity | metafield_tag }}

<!-- number_integer -->
{{ product.metafields.information.doses_per_day | metafield_tag }}

<!-- page_reference -->
{{ product.metafields.information.dosage | metafield_tag }}

<!-- product_reference -->
{{ product.metafields.information.related_product | metafield_tag }}

<!-- rating -->
{{ product.metafields.details.rating | metafield_tag }}

<!-- single_line_text_field -->
{{ product.metafields.information.directions | metafield_tag }}

<!-- url -->
{{ product.metafields.information.health | metafield_tag }}

<!-- variant_reference -->
{{ product.metafields.information.best_seller | metafield_tag }}

<!-- rich_text_field -->
{{ product.metafields.information.rich_description | metafield_tag }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
<!-- boolean -->
<span class="metafield-boolean">false</span>

<!-- collection_reference -->
<a href="/collections/sale-potions" class="metafield-collection_reference">Sale potions</a>

<!-- color -->
<span class="metafield-color">#ff0000</span>

<!-- date -->
<time datetime="2040-01-01" class="metafield-date">January 1, 2040</time>

<!-- date_time -->
<time datetime="2022-06-22T13:00:00Z" class="metafield-date_time">Jun 22, 2022, 1:00 pm</time>

<!-- json -->
<script type="application/json" class="metafield-json">{"temperature":"700","unit":"degrees","scale":"Fahrenheit"}</script>

<!-- money -->
<span class="metafield-money">$0.10 CAD</span>

<!-- multi_line_text_field -->
<span class="metafield-multi_line_text_field">All health potions are made to order, so it might take up to 2 weeks before your order can be shipped.<br />
<br />
Thanks for your patience!</span>

<!-- number_decimal -->
<span class="metafield-number_decimal">8.4</span>

<!-- number_integer -->
<span class="metafield-number_integer">3</span>

<!-- page_reference -->
<a href="/pages/potion-dosages" class="metafield-page_reference">Potion dosages</a>

<!-- product_reference -->
<a href="/products/dried-chamomile" class="metafield-product_reference">Dried chamomile</a>

<!-- rating -->
<span class="metafield-rating">4.5</span>

<!-- single_line_text_field -->
<span class="metafield-single_line_text_field">Take with a meal.</span>

<!-- url -->
<a href="https://www.canada.ca/en/health-canada/services/food-nutrition/legislation-guidelines/acts-regulations/canada-food-drugs.html" class="metafield-url">www.canada.ca/en/health-canada/services/food-nutrition/legislation-guidelines/acts-regulations/canada-food-drugs.html</a>

<!-- variant_reference -->
<a href="/products/health-potion?variant=39897499762753" class="metafield-variant_reference">S / Medium</a>

<!-- rich_text_field -->
<div class="metafield-rich_text_field"><h3>Are you low on health? Well we&#39;ve got the potion just for you!</h3><p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p></div>
```

### Complex types

The following metafield types return nested elements, or different elements depending on the metafield contents:

* [`dimension`](https://shopify.dev/docs/api/liquid/filters/metafield_tag#metafield_tag-dimension)
* [`file_reference`](https://shopify.dev/docs/api/liquid/filters/metafield_tag#metafield_tag-file_reference)
* [`list.metaobject_reference`](https://shopify.dev/docs/api/liquid/filters/metafield_tag#metafield_tag-list.metaobject_reference)
* [`list.single_line_text_field`](https://shopify.dev/docs/api/liquid/filters/metafield_tag#metafield_tag-list.single_line_text_field)
* [`metaobject_reference`](https://shopify.dev/docs/api/liquid/filters/metafield_tag#metafield_tag-metaobject_reference)
* [`volume`](https://shopify.dev/docs/api/liquid/filters/metafield_tag#metafield_tag-volume)
* [`weight`](https://shopify.dev/docs/api/liquid/filters/metafield_tag#metafield_tag-weight)

### dimension

Outputs a `<span>` element with the following attribute:

| Attribute | Value |
| - | - |
| `class` | `metafield-dimension` |

The `<span>` element contains the following child elements:

| Child element | HTML element | Attributes |
| - | - | - |
| The dimension value. If it's a decimal with more than two places, then it'll be formatted to have a precision of two with trailing zeros removed. | `<span>` | `class="metafield-dimension_value"` |
| The dimension unit. | `<span>` | `class="metafield-dimension_unit"` |

##### Code

```liquid
{{ product.metafields.details.scale_width | metafield_tag }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
<span class="metafield-dimension"><span class="metafield-dimension_value">3 </span><span class="metafield-dimension_unit">cm</span></span>
```

### file\_reference

The output varies depending on the type of file. There are the following categories of file type:

| File type | Description |
| - | - |
| Image | Images in the format of `jpg`, `png`, `gif`, `heic`, and `webp`. |
| Video | Videos in the format of `mov`, and `mp4`. |
| Other | Any other file type. |

##### Image

Outputs an `<img>` element with the following attributes:

| Attribute | Value |
| - | - |
| `src` | The image's URL. |
| `alt` | The image's alt text. |
| `class` | `metafield-file_reference` |

##### Video

Outputs a `<video>` element with the following attributes:

| Attribute | Value |
| - | - |
| `src` | The video's URL. |
| `poster` | The video's preview image (poster) URL. |
| `playsinline` | N/A - Indicates the video will be played "inline" within the element's playback area. |
| `preload` | `metadata` - Only metadata is pre-fetched before the video is played. |

The `<video>` element contains the following child elements:

| Child element | HTML element | Attributes |
| - | - | - |
| The video's multimedia playlist source, for [HTTP live streaming (HLS)](https://developer.apple.com/streaming/) | `&lt;source&gt;` | `src="&lt;the video's m3u8 source URL>"`\<br>\<br>`type="application/x-mpegURL"` |
| The video's original source | `&lt;source&gt;` | `src="&lt;the video's source URL&gt;"`\<br>\<br>`type="&lt;the video's original source MIME type>"` |
| The video's preview (poster) image | `&lt;img&gt;` | `src="&lt;the video's preview image URL>"` |

##### Other

Outputs an `<a>` element with a link to the file and the following attribute:

| Attribute | Value |
| - | - |
| `class` | `metafield-file_reference` |

The `<a>` element contains an `<img>` element for the file's [preview image](https://shopify.dev/docs/api/liquid/objects/generic_file#generic_file-preview_image) with the following attributes:

| Attribute | Value |
| - | - |
| `src` | The file's preview image URL. |
| `loading` | `lazy` - The image isn't loaded until it's almost in view. |

##### Code

```liquid
<!-- Image -->
{{ product.metafields.information.promo_image | metafield_tag }}

<!-- Video -->
{{ product.metafields.information.promo_video | metafield_tag }}

<!-- Other -->
{{ product.metafields.information.disclaimers | metafield_tag }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
<!-- Image -->
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/potions-header.png?v=1650325393" loading="lazy" class="metafield-file_reference">

<!-- Video -->
<video playsinline="playsinline" preload="metadata" poster="//polinas-potent-potions.myshopify.com/cdn/shop/files/preview_images/4733c31cd9d744f6994f61458fda85e6.thumbnail.0000000_small.jpg?v=1655257099"><source src="//polinas-potent-potions.myshopify.com/cdn/shop/videos/c/vp/4733c31cd9d744f6994f61458fda85e6/4733c31cd9d744f6994f61458fda85e6.HD-1080p-7.2Mbps.mp4?v=0" type="video/mp4"><img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/preview_images/4733c31cd9d744f6994f61458fda85e6.thumbnail.0000000_small.jpg?v=1655257099"></video>

<!-- Other -->
<a href="//polinas-potent-potions.myshopify.com/cdn/shop/files/disclaimer.pdf?v=9043651738044769859" class="metafield-file_reference"><img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/preview_images/document-7f23220eb4be7eeaa6e225738b97d943f22e74367cd2d7544fc3b37fb36acd71.png?v=1653087800" loading="lazy"></a>
```

### list.metaobject\_reference

```oobleck
metafield | metafield_tag: field: string
```

Outputs a `<ul>` element by default with the following attribute:

| Attribute | Value |
| - | - |
| `class` | `metafield-single_line_text_field-array` |

The `<ul>` element contains an `<li>` element for each metaobject in the list with a `class` of `metafield-single_line_text_field`. The required `field` parameter specifies which field should be rendered for each metaobject. The `field` parameter can reference only metafields of type `single_line_text_field`.

To output an `<ol>` element, pass the `list_format` parameter with a value of `ordered`.

##### Code

```liquid
<!-- <ul> element -->
{{ product.metafields.information.ingredients | metafield_tag: field: 'name' }}

<!-- <ol> element -->
{{ product.metafields.information.ingredients | metafield_tag: field: 'name', list_format: 'ordered' }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
<!-- <ul> element -->
<ul class="metafield-single_line_text_field-array"><li class="metafield-single_line_text_field">Spinach</li><li class="metafield-single_line_text_field">Kale</li><li class="metafield-single_line_text_field">Mushrooms</li></ul>

<!-- <ol> element -->
<ol class="metafield-single_line_text_field-array"><li class="metafield-single_line_text_field">Spinach</li><li class="metafield-single_line_text_field">Kale</li><li class="metafield-single_line_text_field">Mushrooms</li></ol>
```

### list.single\_line\_text\_field

Outputs a `<ul>` element by default with the following attribute:

| Attribute | Value |
| - | - |
| `class` | `metafield-single_line_text_field-array` |

The `<ul>` element contains an `<li>` element for each item in the list with a `class` of `metafield-single_line_text_field`.

To output an `<ol>` element, pass the `list_format` parameter with a value of `ordered`.

##### Code

```liquid
<!-- <ul> element -->
{{ product.metafields.information.pickup_locations | metafield_tag }}

<!-- <ol> element -->
{{ product.metafields.information.pickup_locations | metafield_tag: list_format: 'ordered' }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
<!-- <ul> element -->
<ul class="metafield-single_line_text_field-array"><li class="metafield-single_line_text_field">Ottawa</li><li class="metafield-single_line_text_field">Toronto</li><li class="metafield-single_line_text_field">Montreal</li><li class="metafield-single_line_text_field">Vancouver</li></ul>

<!-- <ol> element -->
<ol class="metafield-single_line_text_field-array"><li class="metafield-single_line_text_field">Ottawa</li><li class="metafield-single_line_text_field">Toronto</li><li class="metafield-single_line_text_field">Montreal</li><li class="metafield-single_line_text_field">Vancouver</li></ol>
```

### metaobject\_reference

```oobleck
metafield | metafield_tag: field: string
```

Outputs an HTML element for the metaobject field specified by the required `field` parameter. The `field` parameter can reference only metafields of type `single_line_text_field`.

##### Code

```liquid
{{ product.metafields.information.primary_ingredient | metafield_tag: field: 'name' }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
<span class="metafield-single_line_text_field">Spinach</span>
```

### volume

Outputs a `<span>` element with the following attribute:

| Attribute | Value |
| - | - |
| `class` | `metafield-volume` |

The `<span>` element contains the following child elements:

| Child element | HTML element | Attributes |
| - | - | - |
| The volume value. If it's a decimal with more than two places, then it'll be formatted to have a precision of two with trailing zeros removed. | `<span>` | `class="metafield-volume_value"` |
| The volume unit. | `<span>` | `class="metafield-volume_unit"` |

##### Code

```liquid
{{ product.metafields.details.milk_container_volume | metafield_tag }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
<span class="metafield-volume"><span class="metafield-volume_value">500 </span><span class="metafield-volume_unit">mL</span></span>
```

### weight

Outputs a `<span>` element with the following attribute:

| Attribute | Value |
| - | - |
| `class` | `metafield-weight` |

The `<span>` element contains the following child elements:

| Child element | HTML element | Attributes |
| - | - | - |
| The weight value. If it's a decimal with more than two places, then it'll be formatted to have a precision of two with trailing zeros removed. | `<span>` | `class="metafield-weight_value"` |
| The weight unit. | `<span>` | `class="metafield-weight_unit"` |

##### Code

```liquid
{{ product.metafields.details.chamomile_base_weight | metafield_tag }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
<span class="metafield-weight"><span class="metafield-weight_value">50 </span><span class="metafield-weight_unit">g</span></span>
```

</page>

<page>
---
title: 'Liquid filters: metafield_text'
description: >-
  Generates a text version of the data from a [`metafield`
  object](https://shopify.dev/docs/api/liquid/objects/metafield).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/metafield_text'
  md: 'https://shopify.dev/docs/api/liquid/filters/metafield_text.md'
---

# metafield\_​text

```oobleck
metafield | metafield_text
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates a text version of the data from a [`metafield` object](https://shopify.dev/docs/api/liquid/objects/metafield).

***

**Note:** The \<code>\<span class="PreventFireFoxApplyingGapToWBR">metafield\<wbr/>\_text\</span>\</code> filter doesn\&#39;t currently support list metafields other than \<code>\<span class="PreventFireFoxApplyingGapToWBR">list.single\<wbr/>\_line\<wbr/>\_text\<wbr/>\_field\</span>\</code> and \<code>\<span class="PreventFireFoxApplyingGapToWBR">list.metaobject\<wbr/>\_reference\</span>\</code>.

***

### Basic types

The following outlines the output for each metafield type:

| Metafield type | Output |
| - | - |
| `single_line_text_field` | The metafield text. |
| `multi_line_text_field` | The metafield text. |
| `page_reference` | The page title. |
| `product_reference` | The product title. |
| `collection_reference` | The collection title. |
| `variant_reference` | The variant title. |
| `file_reference` | The file URL. |
| `number_integer` | The number. |
| `number_decimal` | The number. |
| `date` | The date. |
| `date-time` | The date and time. |
| `url` | The URL. |
| `json` | The JSON. |
| `boolean` | The boolean value. |
| `color` | The color value. |
| `weight` | The weight value and unit. If the value is a decimal with more than two places, then it'll be formatted to have a precision of two with trailing zeros removed. |
| `volume` | The volume value and unit. If the value is a decimal with more than two places, then it'll be formatted to have a precision of two with trailing zeros removed. |
| `dimension` | The dimension value and unit. If the value is a decimal with more than two places, then it'll be formatted to have a precision of two with trailing zeros removed. |
| `rating` | The rating value. |
| `list.single_line_text_field` | The metafield values in sentence format. For example, if you had the values `Toronto`, `Ottawa`, and `Vancouver`, then the output would be: `Toronto, Ottawa, and Vancouver` |
| `money` | The money value, formatted using the store's [**HTML with currency** setting](https://help.shopify.com/manual/payments/currency-formatting). |
| `rich_text_field` | The rich text value as simple text. |

##### Code

```liquid
{{ product.metafields.information.dosage | metafield_text }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
Potion dosages
```

### Complex types

The following metafield types produce different output depending on the provided `field` parameter:

* [`list.metaobject_reference`](https://shopify.dev/docs/api/liquid/filters/metafield_text#metafield_text-list.metaobject_reference)
* [`metaobject_reference`](https://shopify.dev/docs/api/liquid/filters/metafield_text#metafield_text-metaobject_reference)

### list.metaobject\_reference

```oobleck
metafield | metafield_text: field: string
```

Outputs the list of metaobjects in sentence format. The required `field` parameter specifies which field should be rendered for each metaobject. The `field` parameter can reference only metafields of type `single_line_text_field`.

##### Code

```liquid
{{ product.metafields.information.ingredients | metafield_text: field: 'name' }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
Spinach, Kale, and Mushrooms
```

### metaobject\_reference

```oobleck
metafield | metafield_text: field: string
```

Outputs the metafield text for the metaobject field specified by the required `field` parameter. The `field` parameter can reference only metafields of type `single_line_text_field`.

##### Code

```liquid
{{ product.metafields.information.primary_ingredient | metafield_tag: field: 'name' }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
<span class="metafield-single_line_text_field">Spinach</span>
```

</page>

<page>
---
title: 'Liquid filters: minus'
description: Subtracts a given number from another number.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/minus'
  md: 'https://shopify.dev/docs/api/liquid/filters/minus.md'
---

# minus

```oobleck
number | minus: number
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Subtracts a given number from another number.

##### Code

```liquid
{{ 4 | minus: 2 }}
```

##### Output

```html
2
```

</page>

<page>
---
title: 'Liquid filters: model_viewer_tag'
description: >-
  Generates a [Google model viewer component](https://modelviewer.dev/) for a
  given 3D model.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/model_viewer_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/model_viewer_tag.md'
---

# model\_​viewer\_​tag

```oobleck
media | model_viewer_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates a [Google model viewer component](https://modelviewer.dev/) for a given 3D model.

##### Code

```liquid
{% for media in product.media %}
  {% if media.media_type == 'model' %}
    {{ media | model_viewer_tag }}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": [
      {
        "media_type": "model"
      }
    ]
  }
}
```

##### Output

```html
<model-viewer src="//polinas-potent-potions.myshopify.com/cdn/shop/3d/models/o/eb9388299ce0557c/WaterBottle.glb?v=0" camera-controls="true" style="--poster-color: transparent;" data-shopify-feature="1.12" alt="Potion bottle" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/WaterBottle_small.jpg?v=1655189057"></model-viewer>
```

### Model viewer attributes

```oobleck
media | model_viewer_tag: attribute: string
```

By default, the model viewer component has the following attributes:

| Attribute | Value |
| - | - |
| `alt` | `[alt-text]` - The media's alt text. |
| `poster` | `[preview-image-url]` - The media's preview image URL. |
| `camera-controls` | N/A - Allows for controls via mouse/touch. |

You can override these attributes, or any [supported model viewer component attributes](https://modelviewer.dev/docs/index.html#stagingandcameras-attributes) by adding a parameter that matches the attribute name, and the desired value.

##### Code

```liquid
{% for media in product.media %}
  {% if media.media_type == 'model' %}
    {{ media | model_viewer_tag: interaction-policy: 'allow-when-focused' }}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": [
      {
        "media_type": "model"
      }
    ]
  }
}
```

##### Output

```html
<model-viewer interaction-policy="allow-when-focused" src="//polinas-potent-potions.myshopify.com/cdn/shop/3d/models/o/eb9388299ce0557c/WaterBottle.glb?v=0" camera-controls="true" style="--poster-color: transparent;" data-shopify-feature="1.12" alt="Potion bottle" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/WaterBottle_small.jpg?v=1655189057"></model-viewer>
```

### image\_size

```oobleck
media | model_viewer_tag: image_size: string
```

Specify the dimensions of the model's poster image in pixels.

##### Code

```liquid
{% for media in product.media %}
  {% if media.media_type == 'model' %}
    {{ media | model_viewer_tag: image_size: '400x' }}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": [
      {
        "media_type": "model"
      }
    ]
  }
}
```

##### Output

```html
<model-viewer src="//polinas-potent-potions.myshopify.com/cdn/shop/3d/models/o/eb9388299ce0557c/WaterBottle.glb?v=0" camera-controls="true" style="--poster-color: transparent;" data-shopify-feature="1.12" alt="Potion bottle" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/WaterBottle_400x.jpg?v=1655189057"></model-viewer>
```

</page>

<page>
---
title: 'Liquid filters: modulo'
description: Returns the remainder of dividing a number by a given number.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/modulo'
  md: 'https://shopify.dev/docs/api/liquid/filters/modulo.md'
---

# modulo

```oobleck
number | modulo: number
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Returns the remainder of dividing a number by a given number.

##### Code

```liquid
{{ 12 | modulo: 5 }}
```

##### Output

```html
2
```

</page>

<page>
---
title: 'Liquid filters: money'
description: >-
  Formats a given price based on the store's [**HTML without currency**
  setting](https://help.shopify.com/manual/payments/currency-formatting).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/money'
  md: 'https://shopify.dev/docs/api/liquid/filters/money.md'
---

# money

```oobleck
number | money
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Formats a given price based on the store's [**HTML without currency** setting](https://help.shopify.com/manual/payments/currency-formatting).

##### Code

```liquid
{{ product.price | money }}
```

##### Data

```json
{
  "product": {
    "price": "10.00"
  }
}
```

##### Output

```html
$10.00
```

</page>

<page>
---
title: 'Liquid filters: money_amount'
description: >-
  Formats a given price as a plain decimal string, without currency symbols,
  thousand separators, or locale formatting.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/money_amount'
  md: 'https://shopify.dev/docs/api/liquid/filters/money_amount.md'
---

# money\_​amount

```oobleck
number | money_amount
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Formats a given price as a plain decimal string, without currency symbols, thousand separators, or locale formatting.

##### Code

```liquid
{{ product.price | money_amount }}
```

##### Data

```json
{
  "product": {
    "price": "10.00"
  }
}
```

##### Output

```html
10.0
```

</page>

<page>
---
title: 'Liquid filters: money_with_currency'
description: >-
  Formats a given price based on the store's [**HTML with currency**
  setting](https://help.shopify.com/manual/payments/currency-formatting).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/money_with_currency'
  md: 'https://shopify.dev/docs/api/liquid/filters/money_with_currency.md'
---

# money\_​with\_​currency

```oobleck
number | money_with_currency
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Formats a given price based on the store's [**HTML with currency** setting](https://help.shopify.com/manual/payments/currency-formatting).

##### Code

```liquid
{{ product.price | money_with_currency }}
```

##### Data

```json
{
  "product": {
    "price": "10.00"
  }
}
```

##### Output

```html
$10.00 CAD
```

</page>

<page>
---
title: 'Liquid filters: money_without_currency'
description: >-
  Formats a given price based on the store's [**HTML without currency**
  setting](https://help.shopify.com/manual/payments/currency-formatting),
  without the currency symbol.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/money_without_currency'
  md: 'https://shopify.dev/docs/api/liquid/filters/money_without_currency.md'
---

# money\_​without\_​currency

```oobleck
number | money_without_currency
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Formats a given price based on the store's [**HTML without currency** setting](https://help.shopify.com/manual/payments/currency-formatting), without the currency symbol.

##### Code

```liquid
{{ product.price | money_without_currency }}
```

##### Data

```json
{
  "product": {
    "price": "10.00"
  }
}
```

##### Output

```html
10.00
```

</page>

<page>
---
title: 'Liquid filters: money_without_trailing_zeros'
description: >-
  Formats a given price based on the store's [**HTML without currency**
  setting](https://help.shopify.com/manual/payments/currency-formatting),
  excluding the decimal separator

  (either `.` or `,`) and trailing zeros.


  If the price has a non-zero decimal value, then the output is the same as the
  [`money` filter](/docs/api/liquid/filters#money).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/money_without_trailing_zeros'
  md: 'https://shopify.dev/docs/api/liquid/filters/money_without_trailing_zeros.md'
---

# money\_​without\_​trailing\_​zeros

```oobleck
number | money_without_trailing_zeros
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Formats a given price based on the store's [**HTML without currency** setting](https://help.shopify.com/manual/payments/currency-formatting), excluding the decimal separator (either `.` or `,`) and trailing zeros.

If the price has a non-zero decimal value, then the output is the same as the [`money` filter](https://shopify.dev/docs/api/liquid/filters#money).

##### Code

```liquid
{{ product.price | money_without_trailing_zeros }}
```

##### Data

```json
{
  "product": {
    "price": "10.00"
  }
}
```

##### Output

```html
$10
```

</page>

<page>
---
title: 'Liquid filters: newline_to_br'
description: Converts newlines (`\n`) in a string to HTML line breaks (`<br>`).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/newline_to_br'
  md: 'https://shopify.dev/docs/api/liquid/filters/newline_to_br.md'
---

# newline\_​to\_​br

```oobleck
string | newline_to_br
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts newlines (`\n`) in a string to HTML line breaks (`<br>`).

##### Code

```liquid
{{ product.description | newline_to_br }}
```

##### Data

```json
{
  "product": {
    "description": "<h3>Are you low on health? Well we've got the potion just for you!</h3>\n<p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p>"
  }
}
```

##### Output

```html
<h3>Are you low on health? Well we've got the potion just for you!</h3><br />
<p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p>
```

</page>

<page>
---
title: 'Liquid filters: payment_button'
description: >-
  Generates an HTML container to host [accelerated checkout
  buttons](https://help.shopify.com/manual/online-store/dynamic-checkout)

  for a product. The `payment_button` filter must be used on the `form` object
  within a [product form](/docs/api/liquid/tags/form#form-product).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/payment_button'
  md: 'https://shopify.dev/docs/api/liquid/filters/payment_button.md'
---

# payment\_​button

```oobleck
form | payment_button
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML container to host [accelerated checkout buttons](https://help.shopify.com/manual/online-store/dynamic-checkout) for a product. The `payment_button` filter must be used on the `form` object within a [product form](https://shopify.dev/docs/api/liquid/tags/form#form-product).

##### Code

```liquid
{% form 'product', product %}
  {{ form | payment_button }}
{% endform %}
```

##### Data

```json
{
  "product": {
    "id": 6786188247105
  }
}
```

##### Output

```html
<form method="post" action="/cart/add" id="product_form_6786188247105" accept-charset="UTF-8" class="shopify-product-form" enctype="multipart/form-data"><input type="hidden" name="form_type" value="product" /><input type="hidden" name="utf8" value="✓" />
  <div data-shopify="payment-button" class="shopify-payment-button"> <shopify-accelerated-checkout recommended="null" fallback="{&quot;supports_subs&quot;:true,&quot;supports_def_opts&quot;:true,&quot;name&quot;:&quot;buy_it_now&quot;,&quot;wallet_params&quot;:{}}" access-token="7be588c245f69602e5db198d53fcfde5" buyer-country="CA" buyer-locale="en" buyer-currency="CAD" variant-params="[{&quot;id&quot;:39897499729985,&quot;requiresShipping&quot;:true},{&quot;id&quot;:39897499762753,&quot;requiresShipping&quot;:true},{&quot;id&quot;:39897499795521,&quot;requiresShipping&quot;:true},{&quot;id&quot;:39897499828289,&quot;requiresShipping&quot;:true},{&quot;id&quot;:39897499861057,&quot;requiresShipping&quot;:true},{&quot;id&quot;:39897499893825,&quot;requiresShipping&quot;:true},{&quot;id&quot;:39897499926593,&quot;requiresShipping&quot;:true},{&quot;id&quot;:39897499959361,&quot;requiresShipping&quot;:true},{&quot;id&quot;:39897499992129,&quot;requiresShipping&quot;:true}]" shop-id="56174706753" enabled-flags="[&quot;ce346acf&quot;]" > <div class="shopify-payment-button__button" role="button" disabled aria-hidden="true" style="background-color: transparent; border: none"> <div class="shopify-payment-button__skeleton">&nbsp;</div> </div> </shopify-accelerated-checkout> <small id="shopify-buyer-consent" class="hidden" aria-hidden="true" data-consent-type="subscription"> This item is a deferred, subscription, or recurring purchase. By continuing, I agree to the <span id="shopify-subscription-policy-button">cancellation policy</span> and authorize you to charge my payment method at the prices, frequency and dates listed on this page until my order is fulfilled or I cancel, if permitted. </small> </div>
<input type="hidden" name="product-id" value="6786188247105" /></form>
```

</page>

<page>
---
title: 'Liquid filters: payment_terms'
description: >-
  Generates the HTML for the [Shop Pay Installments
  banner](/themes/pricing-payments/installments).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/payment_terms'
  md: 'https://shopify.dev/docs/api/liquid/filters/payment_terms.md'
---

# payment\_​terms

```oobleck
form | payment_terms
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates the HTML for the [Shop Pay Installments banner](https://shopify.dev/themes/pricing-payments/installments).

The `payment_terms` filter must be used on the `form` object within a [product form](https://shopify.dev/docs/api/liquid/tags/form#form-product) or [cart form](https://shopify.dev/docs/api/liquid/tags/form#form-cart).

```liquid
{% form 'product', product %}
  {{ form | payment_terms }}
{% endform %}
```

```liquid
{% form 'product', product %}
  {{ form | payment_terms }}
{% endform %}
```

```liquid
{% form 'cart', cart %}
  {{ form | payment_terms }}
{% endform %}
```

```liquid
{% form 'cart', cart %}
  {{ form | payment_terms }}
{% endform %}
```

</page>

<page>
---
title: 'Liquid filters: payment_type_img_url'
description: >-
  Returns the URL for an SVG image of a given [payment
  type](/docs/api/liquid/objects/shop#shop-enabled_payment_types).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/payment_type_img_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/payment_type_img_url.md'
---

# payment\_​type\_​img\_​url

```oobleck
string | payment_type_img_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the URL for an SVG image of a given [payment type](https://shopify.dev/docs/api/liquid/objects/shop#shop-enabled_payment_types).

##### Code

```liquid
{% for type in shop.enabled_payment_types %}
<img src="{{ type | payment_type_img_url }}" width="50" height="50" />
{% endfor %}
```

##### Data

```json
{
  "shop": {
    "enabled_payment_types": [
      "visa",
      "master",
      "american_express",
      "paypal",
      "diners_club",
      "discover"
    ]
  }
}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shopifycloud/storefront/assets/payment_icons/visa-65d650f7.svg" width="50" height="50" />

<img src="//polinas-potent-potions.myshopify.com/cdn/shopifycloud/storefront/assets/payment_icons/master-54b5a7ce.svg" width="50" height="50" />

<img src="//polinas-potent-potions.myshopify.com/cdn/shopifycloud/storefront/assets/payment_icons/american_express-1efdc6a3.svg" width="50" height="50" />

<img src="//polinas-potent-potions.myshopify.com/cdn/shopifycloud/storefront/assets/payment_icons/paypal-a7c68b85.svg" width="50" height="50" />

<img src="//polinas-potent-potions.myshopify.com/cdn/shopifycloud/storefront/assets/payment_icons/diners_club-678e3046.svg" width="50" height="50" />

<img src="//polinas-potent-potions.myshopify.com/cdn/shopifycloud/storefront/assets/payment_icons/discover-59880595.svg" width="50" height="50" />
```

## Rendered output

</page>

<page>
---
title: 'Liquid filters: payment_type_svg_tag'
description: >-
  Generates an HTML `<svg>` tag for a given [payment
  type](/docs/api/liquid/objects/shop#shop-enabled_payment_types).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/payment_type_svg_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/payment_type_svg_tag.md'
---

# payment\_​type\_​svg\_​tag

```oobleck
string | payment_type_svg_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<svg>` tag for a given [payment type](https://shopify.dev/docs/api/liquid/objects/shop#shop-enabled_payment_types).

##### Code

```liquid
{% for type in shop.enabled_payment_types -%}
  {{ type | payment_type_svg_tag }}
{% endfor %}
```

##### Data

```json
{
  "shop": {
    "enabled_payment_types": [
      "visa",
      "master",
      "american_express",
      "paypal",
      "diners_club",
      "discover"
    ]
  }
}
```

##### Output

```html
<svg viewBox="0 0 38 24" xmlns="http://www.w3.org/2000/svg" role="img" width="38" height="24" aria-labelledby="pi-visa"><title id="pi-visa">Visa</title><path opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path fill="#fff" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32"/><path d="M28.3 10.1H28c-.4 1-.7 1.5-1 3h1.9c-.3-1.5-.3-2.2-.6-3zm2.9 5.9h-1.7c-.1 0-.1 0-.2-.1l-.2-.9-.1-.2h-2.4c-.1 0-.2 0-.2.2l-.3.9c0 .1-.1.1-.1.1h-2.1l.2-.5L27 8.7c0-.5.3-.7.8-.7h1.5c.1 0 .2 0 .2.2l1.4 6.5c.1.4.2.7.2 1.1.1.1.1.1.1.2zm-13.4-.3l.4-1.8c.1 0 .2.1.2.1.7.3 1.4.5 2.1.4.2 0 .5-.1.7-.2.5-.2.5-.7.1-1.1-.2-.2-.5-.3-.8-.5-.4-.2-.8-.4-1.1-.7-1.2-1-.8-2.4-.1-3.1.6-.4.9-.8 1.7-.8 1.2 0 2.5 0 3.1.2h.1c-.1.6-.2 1.1-.4 1.7-.5-.2-1-.4-1.5-.4-.3 0-.6 0-.9.1-.2 0-.3.1-.4.2-.2.2-.2.5 0 .7l.5.4c.4.2.8.4 1.1.6.5.3 1 .8 1.1 1.4.2.9-.1 1.7-.9 2.3-.5.4-.7.6-1.4.6-1.4 0-2.5.1-3.4-.2-.1.2-.1.2-.2.1zm-3.5.3c.1-.7.1-.7.2-1 .5-2.2 1-4.5 1.4-6.7.1-.2.1-.3.3-.3H18c-.2 1.2-.4 2.1-.7 3.2-.3 1.5-.6 3-1 4.5 0 .2-.1.2-.3.2M5 8.2c0-.1.2-.2.3-.2h3.4c.5 0 .9.3 1 .8l.9 4.4c0 .1 0 .1.1.2 0-.1.1-.1.1-.1l2.1-5.1c-.1-.1 0-.2.1-.2h2.1c0 .1 0 .1-.1.2l-3.1 7.3c-.1.2-.1.3-.2.4-.1.1-.3 0-.5 0H9.7c-.1 0-.2 0-.2-.2L7.9 9.5c-.2-.2-.5-.5-.9-.6-.6-.3-1.7-.5-1.9-.5L5 8.2z" fill="#142688"/></svg>
<svg viewBox="0 0 38 24" xmlns="http://www.w3.org/2000/svg" role="img" width="38" height="24" aria-labelledby="pi-master"><title id="pi-master">Mastercard</title><path opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path fill="#fff" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32"/><circle fill="#EB001B" cx="15" cy="12" r="7"/><circle fill="#F79E1B" cx="23" cy="12" r="7"/><path fill="#FF5F00" d="M22 12c0-2.4-1.2-4.5-3-5.7-1.8 1.3-3 3.4-3 5.7s1.2 4.5 3 5.7c1.8-1.2 3-3.3 3-5.7z"/></svg>
<svg xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="pi-american_express" viewBox="0 0 38 24" width="38" height="24"><title id="pi-american_express">American Express</title><path fill="#000" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3Z" opacity=".07"/><path fill="#006FCF" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32Z"/><path fill="#FFF" d="M22.012 19.936v-8.421L37 11.528v2.326l-1.732 1.852L37 17.573v2.375h-2.766l-1.47-1.622-1.46 1.628-9.292-.02Z"/><path fill="#006FCF" d="M23.013 19.012v-6.57h5.572v1.513h-3.768v1.028h3.678v1.488h-3.678v1.01h3.768v1.531h-5.572Z"/><path fill="#006FCF" d="m28.557 19.012 3.083-3.289-3.083-3.282h2.386l1.884 2.083 1.89-2.082H37v.051l-3.017 3.23L37 18.92v.093h-2.307l-1.917-2.103-1.898 2.104h-2.321Z"/><path fill="#FFF" d="M22.71 4.04h3.614l1.269 2.881V4.04h4.46l.77 2.159.771-2.159H37v8.421H19l3.71-8.421Z"/><path fill="#006FCF" d="m23.395 4.955-2.916 6.566h2l.55-1.315h2.98l.55 1.315h2.05l-2.904-6.566h-2.31Zm.25 3.777.875-2.09.873 2.09h-1.748Z"/><path fill="#006FCF" d="M28.581 11.52V4.953l2.811.01L32.84 9l1.456-4.046H37v6.565l-1.74.016v-4.51l-1.644 4.494h-1.59L30.35 7.01v4.51h-1.768Z"/></svg>

<svg viewBox="0 0 38 24" xmlns="http://www.w3.org/2000/svg" width="38" height="24" role="img" aria-labelledby="pi-paypal"><title id="pi-paypal">PayPal</title><path opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path fill="#fff" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32"/><path fill="#003087" d="M23.9 8.3c.2-1 0-1.7-.6-2.3-.6-.7-1.7-1-3.1-1h-4.1c-.3 0-.5.2-.6.5L14 15.6c0 .2.1.4.3.4H17l.4-3.4 1.8-2.2 4.7-2.1z"/><path fill="#3086C8" d="M23.9 8.3l-.2.2c-.5 2.8-2.2 3.8-4.6 3.8H18c-.3 0-.5.2-.6.5l-.6 3.9-.2 1c0 .2.1.4.3.4H19c.3 0 .5-.2.5-.4v-.1l.4-2.4v-.1c0-.2.3-.4.5-.4h.3c2.1 0 3.7-.8 4.1-3.2.2-1 .1-1.8-.4-2.4-.1-.5-.3-.7-.5-.8z"/><path fill="#012169" d="M23.3 8.1c-.1-.1-.2-.1-.3-.1-.1 0-.2 0-.3-.1-.3-.1-.7-.1-1.1-.1h-3c-.1 0-.2 0-.2.1-.2.1-.3.2-.3.4l-.7 4.4v.1c0-.3.3-.5.6-.5h1.3c2.5 0 4.1-1 4.6-3.8v-.2c-.1-.1-.3-.2-.5-.2h-.1z"/></svg>
<svg viewBox="0 0 38 24" xmlns="http://www.w3.org/2000/svg" role="img" width="38" height="24" aria-labelledby="pi-diners_club"><title id="pi-diners_club">Diners Club</title><path opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path fill="#fff" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32"/><path d="M12 12v3.7c0 .3-.2.3-.5.2-1.9-.8-3-3.3-2.3-5.4.4-1.1 1.2-2 2.3-2.4.4-.2.5-.1.5.2V12zm2 0V8.3c0-.3 0-.3.3-.2 2.1.8 3.2 3.3 2.4 5.4-.4 1.1-1.2 2-2.3 2.4-.4.2-.4.1-.4-.2V12zm7.2-7H13c3.8 0 6.8 3.1 6.8 7s-3 7-6.8 7h8.2c3.8 0 6.8-3.1 6.8-7s-3-7-6.8-7z" fill="#3086C8"/></svg>
<svg viewBox="0 0 38 24" width="38" height="24" role="img" aria-labelledby="pi-discover" fill="none" xmlns="http://www.w3.org/2000/svg"><title id="pi-discover">Discover</title><path fill="#000" opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32z" fill="#fff"/><path d="M3.57 7.16H2v5.5h1.57c.83 0 1.43-.2 1.96-.63.63-.52 1-1.3 1-2.11-.01-1.63-1.22-2.76-2.96-2.76zm1.26 4.14c-.34.3-.77.44-1.47.44h-.29V8.1h.29c.69 0 1.11.12 1.47.44.37.33.59.84.59 1.37 0 .53-.22 1.06-.59 1.39zm2.19-4.14h1.07v5.5H7.02v-5.5zm3.69 2.11c-.64-.24-.83-.4-.83-.69 0-.35.34-.61.8-.61.32 0 .59.13.86.45l.56-.73c-.46-.4-1.01-.61-1.62-.61-.97 0-1.72.68-1.72 1.58 0 .76.35 1.15 1.35 1.51.42.15.63.25.74.31.21.14.32.34.32.57 0 .45-.35.78-.83.78-.51 0-.92-.26-1.17-.73l-.69.67c.49.73 1.09 1.05 1.9 1.05 1.11 0 1.9-.74 1.9-1.81.02-.89-.35-1.29-1.57-1.74zm1.92.65c0 1.62 1.27 2.87 2.9 2.87.46 0 .86-.09 1.34-.32v-1.26c-.43.43-.81.6-1.29.6-1.08 0-1.85-.78-1.85-1.9 0-1.06.79-1.89 1.8-1.89.51 0 .9.18 1.34.62V7.38c-.47-.24-.86-.34-1.32-.34-1.61 0-2.92 1.28-2.92 2.88zm12.76.94l-1.47-3.7h-1.17l2.33 5.64h.58l2.37-5.64h-1.16l-1.48 3.7zm3.13 1.8h3.04v-.93h-1.97v-1.48h1.9v-.93h-1.9V8.1h1.97v-.94h-3.04v5.5zm7.29-3.87c0-1.03-.71-1.62-1.95-1.62h-1.59v5.5h1.07v-2.21h.14l1.48 2.21h1.32l-1.73-2.32c.81-.17 1.26-.72 1.26-1.56zm-2.16.91h-.31V8.03h.33c.67 0 1.03.28 1.03.82 0 .55-.36.85-1.05.85z" fill="#231F20"/><path d="M20.16 12.86a2.931 2.931 0 100-5.862 2.931 2.931 0 000 5.862z" fill="url(#pi-paint0_linear)"/><path opacity=".65" d="M20.16 12.86a2.931 2.931 0 100-5.862 2.931 2.931 0 000 5.862z" fill="url(#pi-paint1_linear)"/><path d="M36.57 7.506c0-.1-.07-.15-.18-.15h-.16v.48h.12v-.19l.14.19h.14l-.16-.2c.06-.01.1-.06.1-.13zm-.2.07h-.02v-.13h.02c.06 0 .09.02.09.06 0 .05-.03.07-.09.07z" fill="#231F20"/><path d="M36.41 7.176c-.23 0-.42.19-.42.42 0 .23.19.42.42.42.23 0 .42-.19.42-.42 0-.23-.19-.42-.42-.42zm0 .77c-.18 0-.34-.15-.34-.35 0-.19.15-.35.34-.35.18 0 .33.16.33.35 0 .19-.15.35-.33.35z" fill="#231F20"/><path d="M37 12.984S27.09 19.873 8.976 23h26.023a2 2 0 002-1.984l.024-3.02L37 12.985z" fill="#F48120"/><defs><linearGradient id="pi-paint0_linear" x1="21.657" y1="12.275" x2="19.632" y2="9.104" gradientUnits="userSpaceOnUse"><stop stop-color="#F89F20"/><stop offset=".25" stop-color="#F79A20"/><stop offset=".533" stop-color="#F68D20"/><stop offset=".62" stop-color="#F58720"/><stop offset=".723" stop-color="#F48120"/><stop offset="1" stop-color="#F37521"/></linearGradient><linearGradient id="pi-paint1_linear" x1="21.338" y1="12.232" x2="18.378" y2="6.446" gradientUnits="userSpaceOnUse"><stop stop-color="#F58720"/><stop offset=".359" stop-color="#E16F27"/><stop offset=".703" stop-color="#D4602C"/><stop offset=".982" stop-color="#D05B2E"/></linearGradient></defs></svg>
```

## Rendered output

### class

```oobleck
type | payment_type_svg_tag: class: string
```

Specify the `class` attribute of the `<svg>` tag.

##### Code

```liquid
{% for type in shop.enabled_payment_types -%}
  {{ type | payment_type_svg_tag: class: 'custom-class' }}
{% endfor %}
```

##### Data

```json
{
  "shop": {
    "enabled_payment_types": [
      "visa",
      "master",
      "american_express",
      "paypal",
      "diners_club",
      "discover"
    ]
  }
}
```

##### Output

```html
<svg class="custom-class" viewBox="0 0 38 24" xmlns="http://www.w3.org/2000/svg" role="img" width="38" height="24" aria-labelledby="pi-visa"><title id="pi-visa">Visa</title><path opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path fill="#fff" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32"/><path d="M28.3 10.1H28c-.4 1-.7 1.5-1 3h1.9c-.3-1.5-.3-2.2-.6-3zm2.9 5.9h-1.7c-.1 0-.1 0-.2-.1l-.2-.9-.1-.2h-2.4c-.1 0-.2 0-.2.2l-.3.9c0 .1-.1.1-.1.1h-2.1l.2-.5L27 8.7c0-.5.3-.7.8-.7h1.5c.1 0 .2 0 .2.2l1.4 6.5c.1.4.2.7.2 1.1.1.1.1.1.1.2zm-13.4-.3l.4-1.8c.1 0 .2.1.2.1.7.3 1.4.5 2.1.4.2 0 .5-.1.7-.2.5-.2.5-.7.1-1.1-.2-.2-.5-.3-.8-.5-.4-.2-.8-.4-1.1-.7-1.2-1-.8-2.4-.1-3.1.6-.4.9-.8 1.7-.8 1.2 0 2.5 0 3.1.2h.1c-.1.6-.2 1.1-.4 1.7-.5-.2-1-.4-1.5-.4-.3 0-.6 0-.9.1-.2 0-.3.1-.4.2-.2.2-.2.5 0 .7l.5.4c.4.2.8.4 1.1.6.5.3 1 .8 1.1 1.4.2.9-.1 1.7-.9 2.3-.5.4-.7.6-1.4.6-1.4 0-2.5.1-3.4-.2-.1.2-.1.2-.2.1zm-3.5.3c.1-.7.1-.7.2-1 .5-2.2 1-4.5 1.4-6.7.1-.2.1-.3.3-.3H18c-.2 1.2-.4 2.1-.7 3.2-.3 1.5-.6 3-1 4.5 0 .2-.1.2-.3.2M5 8.2c0-.1.2-.2.3-.2h3.4c.5 0 .9.3 1 .8l.9 4.4c0 .1 0 .1.1.2 0-.1.1-.1.1-.1l2.1-5.1c-.1-.1 0-.2.1-.2h2.1c0 .1 0 .1-.1.2l-3.1 7.3c-.1.2-.1.3-.2.4-.1.1-.3 0-.5 0H9.7c-.1 0-.2 0-.2-.2L7.9 9.5c-.2-.2-.5-.5-.9-.6-.6-.3-1.7-.5-1.9-.5L5 8.2z" fill="#142688"/></svg>
<svg class="custom-class" viewBox="0 0 38 24" xmlns="http://www.w3.org/2000/svg" role="img" width="38" height="24" aria-labelledby="pi-master"><title id="pi-master">Mastercard</title><path opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path fill="#fff" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32"/><circle fill="#EB001B" cx="15" cy="12" r="7"/><circle fill="#F79E1B" cx="23" cy="12" r="7"/><path fill="#FF5F00" d="M22 12c0-2.4-1.2-4.5-3-5.7-1.8 1.3-3 3.4-3 5.7s1.2 4.5 3 5.7c1.8-1.2 3-3.3 3-5.7z"/></svg>
<svg class="custom-class" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="pi-american_express" viewBox="0 0 38 24" width="38" height="24"><title id="pi-american_express">American Express</title><path fill="#000" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3Z" opacity=".07"/><path fill="#006FCF" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32Z"/><path fill="#FFF" d="M22.012 19.936v-8.421L37 11.528v2.326l-1.732 1.852L37 17.573v2.375h-2.766l-1.47-1.622-1.46 1.628-9.292-.02Z"/><path fill="#006FCF" d="M23.013 19.012v-6.57h5.572v1.513h-3.768v1.028h3.678v1.488h-3.678v1.01h3.768v1.531h-5.572Z"/><path fill="#006FCF" d="m28.557 19.012 3.083-3.289-3.083-3.282h2.386l1.884 2.083 1.89-2.082H37v.051l-3.017 3.23L37 18.92v.093h-2.307l-1.917-2.103-1.898 2.104h-2.321Z"/><path fill="#FFF" d="M22.71 4.04h3.614l1.269 2.881V4.04h4.46l.77 2.159.771-2.159H37v8.421H19l3.71-8.421Z"/><path fill="#006FCF" d="m23.395 4.955-2.916 6.566h2l.55-1.315h2.98l.55 1.315h2.05l-2.904-6.566h-2.31Zm.25 3.777.875-2.09.873 2.09h-1.748Z"/><path fill="#006FCF" d="M28.581 11.52V4.953l2.811.01L32.84 9l1.456-4.046H37v6.565l-1.74.016v-4.51l-1.644 4.494h-1.59L30.35 7.01v4.51h-1.768Z"/></svg>

<svg class="custom-class" viewBox="0 0 38 24" xmlns="http://www.w3.org/2000/svg" width="38" height="24" role="img" aria-labelledby="pi-paypal"><title id="pi-paypal">PayPal</title><path opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path fill="#fff" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32"/><path fill="#003087" d="M23.9 8.3c.2-1 0-1.7-.6-2.3-.6-.7-1.7-1-3.1-1h-4.1c-.3 0-.5.2-.6.5L14 15.6c0 .2.1.4.3.4H17l.4-3.4 1.8-2.2 4.7-2.1z"/><path fill="#3086C8" d="M23.9 8.3l-.2.2c-.5 2.8-2.2 3.8-4.6 3.8H18c-.3 0-.5.2-.6.5l-.6 3.9-.2 1c0 .2.1.4.3.4H19c.3 0 .5-.2.5-.4v-.1l.4-2.4v-.1c0-.2.3-.4.5-.4h.3c2.1 0 3.7-.8 4.1-3.2.2-1 .1-1.8-.4-2.4-.1-.5-.3-.7-.5-.8z"/><path fill="#012169" d="M23.3 8.1c-.1-.1-.2-.1-.3-.1-.1 0-.2 0-.3-.1-.3-.1-.7-.1-1.1-.1h-3c-.1 0-.2 0-.2.1-.2.1-.3.2-.3.4l-.7 4.4v.1c0-.3.3-.5.6-.5h1.3c2.5 0 4.1-1 4.6-3.8v-.2c-.1-.1-.3-.2-.5-.2h-.1z"/></svg>
<svg class="custom-class" viewBox="0 0 38 24" xmlns="http://www.w3.org/2000/svg" role="img" width="38" height="24" aria-labelledby="pi-diners_club"><title id="pi-diners_club">Diners Club</title><path opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path fill="#fff" d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32"/><path d="M12 12v3.7c0 .3-.2.3-.5.2-1.9-.8-3-3.3-2.3-5.4.4-1.1 1.2-2 2.3-2.4.4-.2.5-.1.5.2V12zm2 0V8.3c0-.3 0-.3.3-.2 2.1.8 3.2 3.3 2.4 5.4-.4 1.1-1.2 2-2.3 2.4-.4.2-.4.1-.4-.2V12zm7.2-7H13c3.8 0 6.8 3.1 6.8 7s-3 7-6.8 7h8.2c3.8 0 6.8-3.1 6.8-7s-3-7-6.8-7z" fill="#3086C8"/></svg>
<svg class="custom-class" viewBox="0 0 38 24" width="38" height="24" role="img" aria-labelledby="pi-discover" fill="none" xmlns="http://www.w3.org/2000/svg"><title id="pi-discover">Discover</title><path fill="#000" opacity=".07" d="M35 0H3C1.3 0 0 1.3 0 3v18c0 1.7 1.4 3 3 3h32c1.7 0 3-1.3 3-3V3c0-1.7-1.4-3-3-3z"/><path d="M35 1c1.1 0 2 .9 2 2v18c0 1.1-.9 2-2 2H3c-1.1 0-2-.9-2-2V3c0-1.1.9-2 2-2h32z" fill="#fff"/><path d="M3.57 7.16H2v5.5h1.57c.83 0 1.43-.2 1.96-.63.63-.52 1-1.3 1-2.11-.01-1.63-1.22-2.76-2.96-2.76zm1.26 4.14c-.34.3-.77.44-1.47.44h-.29V8.1h.29c.69 0 1.11.12 1.47.44.37.33.59.84.59 1.37 0 .53-.22 1.06-.59 1.39zm2.19-4.14h1.07v5.5H7.02v-5.5zm3.69 2.11c-.64-.24-.83-.4-.83-.69 0-.35.34-.61.8-.61.32 0 .59.13.86.45l.56-.73c-.46-.4-1.01-.61-1.62-.61-.97 0-1.72.68-1.72 1.58 0 .76.35 1.15 1.35 1.51.42.15.63.25.74.31.21.14.32.34.32.57 0 .45-.35.78-.83.78-.51 0-.92-.26-1.17-.73l-.69.67c.49.73 1.09 1.05 1.9 1.05 1.11 0 1.9-.74 1.9-1.81.02-.89-.35-1.29-1.57-1.74zm1.92.65c0 1.62 1.27 2.87 2.9 2.87.46 0 .86-.09 1.34-.32v-1.26c-.43.43-.81.6-1.29.6-1.08 0-1.85-.78-1.85-1.9 0-1.06.79-1.89 1.8-1.89.51 0 .9.18 1.34.62V7.38c-.47-.24-.86-.34-1.32-.34-1.61 0-2.92 1.28-2.92 2.88zm12.76.94l-1.47-3.7h-1.17l2.33 5.64h.58l2.37-5.64h-1.16l-1.48 3.7zm3.13 1.8h3.04v-.93h-1.97v-1.48h1.9v-.93h-1.9V8.1h1.97v-.94h-3.04v5.5zm7.29-3.87c0-1.03-.71-1.62-1.95-1.62h-1.59v5.5h1.07v-2.21h.14l1.48 2.21h1.32l-1.73-2.32c.81-.17 1.26-.72 1.26-1.56zm-2.16.91h-.31V8.03h.33c.67 0 1.03.28 1.03.82 0 .55-.36.85-1.05.85z" fill="#231F20"/><path d="M20.16 12.86a2.931 2.931 0 100-5.862 2.931 2.931 0 000 5.862z" fill="url(#pi-paint0_linear)"/><path opacity=".65" d="M20.16 12.86a2.931 2.931 0 100-5.862 2.931 2.931 0 000 5.862z" fill="url(#pi-paint1_linear)"/><path d="M36.57 7.506c0-.1-.07-.15-.18-.15h-.16v.48h.12v-.19l.14.19h.14l-.16-.2c.06-.01.1-.06.1-.13zm-.2.07h-.02v-.13h.02c.06 0 .09.02.09.06 0 .05-.03.07-.09.07z" fill="#231F20"/><path d="M36.41 7.176c-.23 0-.42.19-.42.42 0 .23.19.42.42.42.23 0 .42-.19.42-.42 0-.23-.19-.42-.42-.42zm0 .77c-.18 0-.34-.15-.34-.35 0-.19.15-.35.34-.35.18 0 .33.16.33.35 0 .19-.15.35-.33.35z" fill="#231F20"/><path d="M37 12.984S27.09 19.873 8.976 23h26.023a2 2 0 002-1.984l.024-3.02L37 12.985z" fill="#F48120"/><defs><linearGradient id="pi-paint0_linear" x1="21.657" y1="12.275" x2="19.632" y2="9.104" gradientUnits="userSpaceOnUse"><stop stop-color="#F89F20"/><stop offset=".25" stop-color="#F79A20"/><stop offset=".533" stop-color="#F68D20"/><stop offset=".62" stop-color="#F58720"/><stop offset=".723" stop-color="#F48120"/><stop offset="1" stop-color="#F37521"/></linearGradient><linearGradient id="pi-paint1_linear" x1="21.338" y1="12.232" x2="18.378" y2="6.446" gradientUnits="userSpaceOnUse"><stop stop-color="#F58720"/><stop offset=".359" stop-color="#E16F27"/><stop offset=".703" stop-color="#D4602C"/><stop offset=".982" stop-color="#D05B2E"/></linearGradient></defs></svg>
```

## Rendered output

</page>

<page>
---
title: 'Liquid filters: placeholder_svg_tag'
description: Generates an HTML `<svg>` tag for a given placeholder name.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/placeholder_svg_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/placeholder_svg_tag.md'
---

# placeholder\_​svg\_​tag

```oobleck
string | placeholder_svg_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<svg>` tag for a given placeholder name.

Accepts the following placeholder names:

| Outline illustrations | Color illustrations |
| - | - |
| * `product-1`
* `product-2`
* `product-3`
* `product-4`
* `product-5`
* `product-6`
* `collection-1`
* `collection-2`
* `collection-3`
* `collection-4`
* `collection-5`
* `collection-6`
* `lifestyle-1`
* `lifestyle-2`
* `image` | - `product-apparel-1`
- `product-apparel-2`
- `product-apparel-3`
- `product-apparel-4`
- `collection-apparel-1`
- `collection-apparel-2`
- `collection-apparel-3`
- `collection-apparel-4`
- `hero-apparel-1`
- `hero-apparel-2`
- `hero-apparel-3`
- `blog-apparel-1`
- `blog-apparel-2`
- `blog-apparel-3`
- `detailed-apparel-1` |

##### Code

```liquid
{{ 'collection-1' | placeholder_svg_tag }}
```

##### Output

```html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 525.5 525.5"><path d="M439.9 310.8c-.2.2-.1.5.1.7l13.2 8.7c.1.1.2.1.3.1.2 0 .3-.1.4-.2.2-.2.1-.5-.1-.7l-13.2-8.7c-.3-.2-.6-.1-.7.1z"/><path d="M463.4 235c1.1-9.4-1-18.6-5.1-21.6-1.7-1.2-3.6-1.3-5.4-.3l-.3.3-6.1-9.8-.1-.1-.8-8.1c-.2-1.9-1.7-3.3-3.6-3.3h-33c-1.6-33-14-75.8-44-75.9h-.1c-7.8 0-14.9 3.1-21.1 9.3-12.5 12.5-21 38.1-22.3 66.5h-20.7v-2.5c0-1.5-1.2-2.7-2.7-2.7h-3.8c-1.5 0-2.7 1.2-2.7 2.7v2.5H288c-1.9 0-3.4 1.4-3.6 3.3l-.8 8.4-5.9 9.5c-.1-.1-.3-.3-.5-.3-.8-.2-2.2-.3-3.6.8-.4.3-.7.6-1.1 1.1-8.5 9.5-6.5 32.6-.8 51.2h-34.5c.1-2.1.2-4.6.4-7.3.6-10.3 1.3-23.1.1-30.3-1.7-10.1-8.9-21.5-13.3-26.6-3.9-4.5-9.3-10.8-11.1-12.9 6.2-4 9.6-9.6 10.1-16.6v-.6c.3-3-.4-7.1-2.8-9.7-1.5-1.7-3.4-2.5-5.7-2.5h-39.6c-.3-11.5-6.3-23-19.3-23-4.3 0-8.2 1.7-11.4 4.5l-.2-.1c0 .1-.1.2-.1.4-4.5 4.2-7.4 10.8-7.6 18.3h-34.9c-2.3 0-4.3.8-5.7 2.5-2.3 2.6-3.1 6.7-2.8 9.7v.6c.5 7 3.9 12.6 10.1 16.6-1.9 2.2-7.3 8.4-11.1 12.9-5.4 6.3-11.9 17.3-13.3 26.6-2 12.9-.8 23 .2 32 .9 7.8 1.7 14.6.3 21.6-.8 1.7-1.7 3.6-2.4 5.6-3.2 8.4-4.4 18.9-3.6 23.5.7 3.9 4.3 6.7 8.9 8.3H62.8c-.6 0-1 .4-1 1V389c0 .6.4 1 1 1h59.7c.2.4.4.8.5 1.2 1.1 2.4 2.2 5 3.5 8.2.1.2.2.5.3.7 2.3 5.2 7.5 8.8 13.5 8.8h171.3c6 0 11.2-3.6 13.5-8.8v-.1l.3-.6c1.3-3.2 2.5-5.9 3.5-8.3.2-.4.4-.8.5-1.2H442c.9 0 1.7-.5 2.1-1.3.4-.8.3-1.7-.2-2.4l-8.4-10.8c-3-3.8-7.4-6-12.3-6h-53v-30.5c0-.3-.1-.5-.3-.7 6.3-.4 13.3-1.6 21-4 7.8-2.4 14.7-5.7 20.9-9.5H452c1.7 0 3.4-.7 4.5-2s1.7-3 1.5-4.7l-4.2-42.4c0-.1-.1-.3-.1-.4 5.8-13.2 9.3-27.2 9.7-40.5.1.4.1.3 0 .3zm-9.4-20.2c1.1-.6 2.2-.6 3.2.2 1.9 1.4 3.5 5 4.2 9.7-1.5-1.6-3.8-2-5.7-2.3l-1.5-.3c-1.4-.3-2.2-1-2.5-2.1-.3-1 0-2.2.7-3.3l1 1.6c.2.3.5.5.8.5.2 0 .4 0 .5-.2.5-.3.6-.9.3-1.4l-1.4-2.2c.2-.1.3-.1.4-.2zm-2.8 0c-1.5 1.7-2 3.8-1.5 5.7.5 1.8 1.9 3 4 3.5.5.1 1.1.2 1.6.3 3.1.6 5.1 1.1 5.5 3.8.1.5.5.8.9.8.1 3-.2 6.4-.9 9.8-1.9 8.8-4.6 17.3-8.2 25.5l-5.7-56.1 4.3 6.7zm-50.1-7.5h8.3l3.1 27.6c.1.5-.1.9-.4 1.2-.3.3-.7.5-1.2.5h-11.4c-.5 0-.9-.2-1.2-.5s-.4-.8-.4-1.2l3.2-27.6zm10.2-.4l-.1-.7c-.1-.5-.5-.9-1-.9h-10.1c-.5 0-.9.4-1 .9l-.1.7v-7.7h2.3v.6c0 1.3 1.1 2.4 2.4 2.4h3.2c1.3 0 2.4-1.1 2.4-2.4v-.6h2v7.7zm-49.2-14.7V140c1 .3 2 .5 3.1.5s2.1-.2 3.1-.5v52.2h-6.2zm-32.6 0c1.2-26.6 8.8-50.1 19.9-61.3 2.6-2.6 5.4-4.5 8.4-5.7-1.3 1.6-2.1 3.6-2.1 5.9 0 3.4 1.8 6.3 4.5 8 0 .1-.1.2-.1.4v52.7h-30.6zm-8.2 15.2h8.3l3.1 27.6c.1.5-.1.9-.4 1.2s-.7.5-1.2.5h-11.4c-.5 0-.9-.2-1.2-.5s-.4-.8-.4-1.2l3.2-27.6zm10.2-.4l-.1-.7c-.1-.5-.5-.9-1-.9h-10.1c-.5 0-.9.4-1 .9l-.1.7v-7.7h2.1v.5c0 1.3 1.1 2.4 2.4 2.4h3c1.3 0 2.4-1.1 2.4-2.4v-.6h2.3v7.8zm33.6-83.2c.6 0 1.2 0 1.7.1 3.3.8 5.8 3.7 5.8 7.2 0 4.1-3.3 7.4-7.4 7.4s-7.4-3.3-7.4-7.4c0-3.5 2.4-6.4 5.7-7.2.5-.1 1-.1 1.6-.1zm5 15.3c2.7-1.7 4.4-4.6 4.4-8 0-2.3-.8-4.3-2.1-6 17.4 6.6 27.3 36.7 28.7 67.1h-31v-52.7c.1-.2.1-.3 0-.4zm-24.8-12c5.8-5.8 12.5-8.8 19.7-8.8h.1c31 .1 42.2 48.8 42.2 81.5 0 .2-.2.4-.4.4h-3.2c-.2 0-.4-.2-.4-.4 0-2.1 0-4.1-.1-6.2.1-.1.1-.3.1-.5s-.1-.4-.2-.5c-1.5-34.5-14-68.8-36.1-70.8-.6-.1-1.3-.2-2-.2s-1.4.1-2 .2c-5.5.5-10.6 3.1-15.2 7.6-12.6 12.5-20.7 40.1-20.7 70.3 0 .2-.2.4-.4.4h-3c-.2 0-.4-.2-.4-.4.1-30.8 8.7-59.3 22-72.6zM299 208h-5.3l1.7-13.5h1.8L299 208zm-5.4-16v-2.3c0-.4.3-.7.7-.7h3.8c.4 0 .7.3.7.7v2.5h-5.4c.2-.1.2-.1.2-.2zm-7.1 3.7c.1-.8.8-1.5 1.6-1.5h5.3l-1.9 14.7c0 .3.1.6.2.8.2.2.5.3.8.3h7.6c.3 0 .6-.1.8-.3.2-.2.3-.5.2-.8l-1.9-14.7h22.3c0 1-.1 2-.1 3.1h-3.1c-.6 0-1 .4-1 1v11.8c0 .6.4 1 1 1 .2 0 .4-.1.6-.2l-2.7 23.9c-.1 1 .2 2 .9 2.8.7.8 1.6 1.2 2.7 1.2h11.4c1 0 2-.4 2.7-1.2.7-.8 1-1.8.9-2.8l-2.7-23.9c.2.1.3.2.6.2.6 0 1-.4 1-1v-11.8c0-.6-.4-1-1-1H329.4c0-1 0-2.1.1-3.1h71.9c0 1 .1 2 .1 3h-3.3c-.6 0-1 .4-1 1V210c0 .6.4 1 1 1 .2 0 .4-.1.6-.2l-2.7 23.9c-.1 1 .2 2 .9 2.8.7.8 1.6 1.2 2.7 1.2h11.4c1 0 2-.4 2.7-1.2.7-.8 1-1.8.9-2.8l-2.7-23.9c.2.1.3.2.6.2.6 0 1-.4 1-1v-11.8c0-.6-.4-1-1-1h-3c0-1 0-2-.1-3.1h32.9c.8 0 1.5.6 1.6 1.5l7.3 72.1c-11.7 24.7-30.6 45-52.5 55.3h-66.3c0-.4-.1-.9-.1-1.3-.5-4.8-.9-9.5-1.3-14.1h81.6c.3 0 .5-.2.5-.5s-.2-.5-.5-.5H331c-.6-7.5-1.1-14.8-1.1-22v-15.1c0-1.8-1.5-3.3-3.3-3.3h-22.2v-5.7c0-.6-.4-1-1-1h-17.2c-.6 0-1 .4-1 1v5.7h-5.5l6.8-70.5zm75.6 134.2V325h6.1v5.1c-2.1.1-4.1 0-6.1-.2zm-18.6-4.9h16.6v4.6c-5.7-.7-11.3-2.2-16.6-4.6zm26.7 0h23.6c-7.9 3.1-15.8 4.8-23.6 5.1V325zm-10.1 44.6h-25.3c.1-1.2.1-2.5.1-3.8v-6.2c1.1-1.1 2.1-2.3 3.1-3.6.2-.2.2-.5.2-.8l-1.8-11.2c-.1-.4-.4-.7-.8-.8-.4-.1-.8.1-1 .5-.1.2-.3.5-.4.7-.4-5-.8-9.9-1.2-14.8 5.8 3.7 14.8 7.8 27.3 8.8 0 .1-.1.2-.1.3v30.9zm-81.5 6.8h.7v9.6h-.7v-9.6zm-2 16.3h-10.9v-16.3h4.5v8.9c0 .6.4 1 1 1s1-.4 1-1v-8.9h4.5v16.3zm-101.2 1h10.9v8.7l-5.5 4.4-5.5-4.4v-8.7zm-2-7.8h-.7v-9.6h.7v9.6zm2 1v-10.6h4.5v8.9c0 .6.4 1 1 1s1-.4 1-1v-8.9h4.5v15.3h-10.9v-4.7zm0-30.7h10.9v18.2h-4.5v-1c0-.6-.4-1-1-1s-1 .4-1 1v1h-4.5v-18.2zm12.9 20.2h.7v9.6h-.7v-9.6zm-.4 27.3c.2-.2.4-.5.4-.8v-9.2h1.3c.6 0 1-.4 1-1s-.4-1-1-1h-1.3v-3.8h1.7c.6 0 1-.4 1-1v-11.6c0-.6-.4-1-1-1h-1.7v-4.1c.2.2.4.3.7.3h74.4c.1 0 .2 0 .3-.1v3.8H262c-.6 0-1 .4-1 1v11.6c0 .6.4 1 1 1h1.7v4.8h-1.3c-.6 0-1 .4-1 1s.4 1 1 1h1.3v8.2c0 .3.1.6.4.8l4.3 3.4h-84.8l4.3-3.3zm75.8-17.8h-.7v-9.6h.7v9.6zm2 16.6v-7.7h10.9v7.7l-5.5 4.4-5.4-4.4zm6.5-28.1v-1c0-.6-.4-1-1-1s-1 .4-1 1v1h-4.5v-18.2h10.9v18.2h-4.4zm6.4-18.2h2.8c.6 0 1-.4 1-1s-.4-1-1-1h-20.6c-.6 0-1 .4-1 1s.4 1 1 1h2.8v12.5c-.1 0-.2-.1-.3-.1H189c-.3 0-.6.1-.7.3v-12.8h2.8c.6 0 1-.4 1-1s-.4-1-1-1h-20.6c-.6 0-1 .4-1 1s.4 1 1 1h2.8v12.6c-.1-.1-.3-.1-.5-.1h-37.2c-6.2 0-11.2-5-11.2-11.2v-88c0-.7.6-1.3 1.3-1.3h51.7c2 3.3 6.8 9.6 17.9 17.6l-1.1 1.4c-.2.2-.2.5-.2.7 0 .3.2.5.4.7l4 3.1-.6.8c-.3.4-.3 1.1.2 1.4.2.1.4.2.6.2.3 0 .6-.1.8-.4l.6-.8 4 3.1c.2.1.4.2.6.2.3 0 .6-.1.8-.4l1.1-1.4 4.7 3.6c-.1.1-.2.1-.3.2-.8 1.1-1.2 2.5-1 3.8.2 1.4.9 2.6 2 3.5l48.7 37.3c.9.7 2 1.1 3.2 1.1h.7c1.4-.2 2.6-.9 3.5-2 .2-.2.2-.5.2-.7 21.9 14.6 38.4 24.9 51.4 24.9 1.5 0 3-.2 4.5-.5-2.1 1.9-4.8 3-7.6 3h-37.7v-12.3zM152.6 197v5h-6.5v-5h6.5zm-6.5 6h6.5v3.2h-6.5V203zm7.5 5.2c.6 0 1-.4 1-1V197h6.2v10.2c0 .6.4 1 1 1h2.9c.2 10.1 1.1 18.1 3 24.4h-18.9c1.7-7.8 2.6-16.3 2.2-24.4h2.6zm9.2-2V203h6.5v3.2h-6.5zm6.6-4.2h-6.5v-5h6.5v5zm-1 32.6c.5 1.6 1.1 3 1.8 4.3.2.3.5.6.9.6.2 0 .3 0 .4-.1.5-.2.7-.8.4-1.3-.5-1-1-2.2-1.4-3.4H208v8.6h-25.4c-.3 0-.5.2-.5.5s.2.5.5.5H208v4h-27.1c-.7-.3-3.4-2.6-4.2-3.5-.4-.4-1-.5-1.4-.1-.4.4-.5 1-.1 1.4.4.4 1.3 1.3 2.4 2.2h-34c.6-1.3 1.2-2.6 1.7-4h19.4c.3 0 .5-.2.5-.5s-.2-.5-.5-.5h-19c1-2.7 1.9-5.6 2.6-8.6h20.1zm30.6 25.5h-4.6l1.5-9.5h1.6l1.5 9.5zm-55.4-17h-34.9v-8.6h37.6c-.8 3.1-1.7 6-2.7 8.6zm-34.9 1h34.5c-.6 1.4-1.2 2.8-1.8 4h-32.7v-4zm4.3 6.1h27.3c-.7 1.3-1.5 2.5-2.3 3.6-.3.4-.2 1.1.2 1.4.2.1.4.2.6.2.3 0 .6-.1.8-.4 1-1.4 2-3 2.9-4.8h51.3l-1.7 10.8c0 .3 0 .6.2.8.2.2.5.4.8.4h6.9c.3 0 .6-.1.8-.4.2-.2.3-.5.2-.8l-1.7-10.8h4.8v16h-11.9c-2.5-2.7-3.6-4.5-3.7-4.6-.2-.4-.7-.6-1.1-.4l-10.7 3.1c-.3.1-.5.2-.6.5-.1.2-.2.5-.1.8l.2.6h-8.8v-5.7c0-.6-.4-1-1-1h-17.2c-.6 0-1 .4-1 1v5.7h-22.6c-1.8 0-3.3 1.5-3.3 3.3v15.1c0 5.5-.3 11-.7 16.6h-2.7c-5.4-.4-6.1-2.8-6.1-4.9v-46.1zm207.4 18v85.3c-11.3.5-26.1-9.9-43.2-21.8-.3-.2-.6-.4-.9-.7 1.7-2.3 1.3-5.5-1-7.3l-48.6-37.3c-1.1-.8-2.5-1.2-3.9-1-1.4.2-2.6.9-3.5 2 0 0-.1.1-.1.2l-4.7-3.6 1-1.3c.2-.2.2-.5.2-.7 0-.3-.2-.5-.4-.7l-4-3.1.6-.8c.3-.4.3-1.1-.2-1.4-.4-.3-1.1-.3-1.4.2l-.6.8-4-3.1c-.4-.3-1.1-.3-1.4.2l-1.1 1.4c-3.8-2.5-6.8-5-9-7.2h126.2zm-18.3-2h-15.2v-4.7h15.2v4.7zm25.5 85c-1.4.9-3 1.5-4.6 1.9-.5.1-1 .2-1.6.2v-85.2h4.8c.7 0 1.3.6 1.3 1.3v81.7c0 .1.1.1.1.1zm2.5-29.3c.8 8.1 1.6 16.5 2.2 25.1-.9 1.1-1.8 2-2.7 2.8v-33.2c.1 1.8.3 3.5.5 5.3zm-68.2 15.2c1.7 1.1 3.3 2.2 4.9 3.3-.2.1-.4.2-.5.3-.5.7-1.3 1.1-2.1 1.2-.8.1-1.7-.1-2.4-.6l-2.2-1.7c.2 0 .5-.1.6-.4l1.7-2.1zm-3.3 1c-.2.2-.2.5-.2.7l-7.8-6c.2 0 .5-.1.6-.4l2.4-3.1 6.9-9 2.7-3.5 7.4 5.7-12 15.6zm-80.1-72.2l8.9-2.6c1.3 1.9 5.7 7.7 14.7 13.6l-5.6 7.3c-12.6-9.1-16.8-15.9-18-18.3zm18.4 21.1l3.2 2.5-.5.6-3.2-2.5.5-.6zm4.8 3.7l3.2 2.5-.5.6-3.2-2.5.5-.6zm-3.6-5.3l5.6-7.3 8.1 6.2-5.6 7.3-8.1-6.2zm14.9-2.7l-3.2-2.5.4-.5 3.2 2.5-.4.5zm-4.8-3.7l-3.2-2.5.4-.5 3.2 2.5-.4.5zm5.2 6.5l10.3 7.9-5.7 7.4-10.3-7.9 5.7-7.4zm11.5 6.3l-4.1-3.2.1-.1c.5-.7 1.3-1.1 2.1-1.2.9-.1 1.7.1 2.4.6l1.6 1.2-2.1 2.7zm-12.4 7.7c.1-.1.1-.2.2-.3l4.1 3.2-2.2 2.8-1.5-1.2c-.7-.5-1.1-1.3-1.2-2.1s.1-1.7.6-2.4zm13.4-5.7l2.7-3.5 7.4 5.7-9.6 12.5-2.8 3.6-7.4-5.7 9.7-12.6zm26.7 33.5l-24-18.4 5.7-7.4 24 18.4-5.7 7.4zm6.9-9l-24-18.4 2.1-2.7 24 18.4-2.1 2.7zm-32.1-7.8l24 18.4-1.7 2.3c-.2.2-.2.5-.2.7l-24.2-18.6 2.1-2.8zm44.7 13.3l2 1.5c1.4 1.1 1.7 3.1.6 4.5v.1c-1.5-1.1-3.1-2.2-4.7-3.3l2.1-2.8zm-121.7-57.6v-4.7h15.2v4.7h-15.2zm112.7 69.3l5.7-7.4c2.5 1.7 4.9 3.4 7.3 5.1 19.5 13.7 34.9 24.4 47.3 21.8 4.1-.9 7.6-3.2 10.6-7l.3-.3c.2-.3.4-.5.6-.8l1.3 8.2c-15 19.2-35.7 5.5-73.1-19.6zm15.1-77.8c-5-7.8-7.1-17.4-7.3-25.5.2.4.5.6.9.6.1 0 .2 0 .4-.1.5-.2.8-.8.6-1.3-.8-2 1.6-4.1 4.1-6.4 2.4-2.2 4.8-4.4 4.7-6.9-.1-1.3-.8-2.5-2.2-3.6l3.8-6.1-5 49.3zm-1.5-42.8l-1.4 2.2c-.3.5-.1 1.1.3 1.4.2.1.3.2.5.2.3 0 .7-.2.8-.5l1.3-2.1c.8.7 1.2 1.3 1.2 2 .1 1.6-2 3.5-4.1 5.3-1.8 1.6-3.8 3.4-4.5 5.4.2-5.4 1.3-9.8 2.9-12.2.1-.2.2-.3.3-.5.3-.3.5-.6.8-.8.7-.4 1.3-.5 1.9-.4zm-7.7 17.7c0 1 .1 2 .2 3.1.8 9.6 4 18.5 8.8 25.1l-.5 5.4H274c-3.6-11.1-5.6-23.5-5-33.6zm-46.1-29.3c4.3 5 11.2 15.9 12.8 25.6 1.2 7 .4 20.2-.1 29.9-.2 2.7-.3 5.2-.4 7.3h-29v-16h2.8c.6 0 1-.4 1-1v-15.6c0-.6-.4-1-1-1h-39.2c-1.9-6.1-2.9-14.3-3.1-24.4h3.6c.6 0 1-.4 1-1V197h2.8c16.7 0 29.1-2.3 37.4-6.9 1.7 1.9 7.4 8.5 11.4 13.2zm-10-40.3c1.7 1.8 2.2 4.8 1.9 6.8v.5c-1 12.7-15.2 19.1-42.4 19.1h-28.1c-27.1 0-41.4-6.4-42.4-19.1v-.5c-.2-2.1.3-5 1.9-6.8.5-.6 1.1-1 1.8-1.3H211c.7.3 1.3.7 1.9 1.3zm-39.6-3.3h-6c.7-2.7 2.1-9.2 1.2-15.2 3.3 4.1 4.7 9.8 4.8 15.2zm-7.5-18c2.3 6.5.1 15.6-.6 18h-18.4c-.6-2.3-2.7-10.7-.8-17.2 2.8-2.5 6.2-3.9 10-3.9 4.1-.1 7.3 1.1 9.8 3.1zm-22.4 3.6c-.7 5.8.7 11.8 1.3 14.4h-6c.2-5.7 1.9-10.7 4.7-14.4zm-48.1 27c0-.2 0-.5-.1-.7-.2-2.5.4-6.1 2.3-8.2 1.1-1.2 2.5-1.8 4.3-1.8h2c-.2.2-.5.4-.7.6-1.9 2.1-2.4 5.3-2.2 7.6v.5c1 13.3 15.6 20 43.4 20h28.1c27.7 0 42.3-6.7 43.4-20v-.5c.2-2.3-.3-5.6-2.2-7.6-.2-.2-.4-.4-.7-.6h2c1.7 0 3.2.6 4.3 1.8 1.9 2.1 2.5 5.7 2.3 8.2 0 .2 0 .4-.1.7-1.1 15-17 22.7-47.3 22.7h-31.6c-30.2 0-46.1-7.6-47.2-22.7zm-14.1 88.1c-1-8.8-2.2-18.9-.2-31.5 1.5-9.5 8.5-20.6 12.8-25.6 4-4.7 9.7-11.3 11.4-13.2 8.2 4.6 20.7 6.9 37.4 6.9h1.6v10.2c0 .6.4 1 1 1h3.8c.4 8.1-.5 16.6-2.2 24.4h-39c-.6 0-1 .4-1 1v15.6c0 .6.4 1 1 1h3.3v37H79.5c3-7.4 6.8-12.6 6.9-12.7.3-.4.2-1.1-.2-1.4-.4-.3-1.1-.2-1.4.2-.1.1-1.1 1.6-2.5 3.9.3-5.4-.4-10.8-1.1-16.8zM75.4 311c-.7-4.1.4-14 3.3-21.8H111v7.1c0 4.2 2.7 6.5 8 6.9h2.7c-.4 5.4-.9 10.9-1.5 16.5H94.6c-12.1-.1-18.4-4.6-19.2-8.7zm-11.6 77.1v-66.5H120c-1.4 14.1-2.9 28.7-2.8 44.1 0 10.7 1.8 16 4.5 22.3H63.8zm55.3-22.3c0-15.3 1.4-29.8 2.8-43.9.2-1.8.3-3.5.5-5.2v40.8c0 3.2 1.2 6.2 3.1 8.5v26.2c-.2-.5-.5-1.1-.7-1.6-3.5-8-5.6-12.8-5.7-24.8zm9.5 33.7c-.1-.2-.2-.5-.3-.7-.5-1.4-.8-2.9-.8-4.5v-26.5c2.2 1.8 5.1 2.8 8.1 2.8h37.2c.2 0 .3-.1.5-.1v3.9h-1.7c-.6 0-1 .4-1 1V387c0 .6.4 1 1 1h1.7v3.8H172c-.6 0-1 .4-1 1s.4 1 1 1h1.3v9.2c0 .3.1.6.4.8l4.3 3.4h-37.7c-5.2-.1-9.7-3.2-11.7-7.7zm183 7.6H274l4.3-3.4c.2-.2.4-.5.4-.8v-8.2h1.3c.6 0 1-.4 1-1s-.4-1-1-1h-1.3v-4.8h1.7c.6 0 1-.4 1-1v-11.6c0-.6-.4-1-1-1h-1.7v-3.7h37.7c3 0 5.8-1 8.1-2.8v26.5c0 1.6-.3 3.1-.8 4.5l-.3.6c-2 4.6-6.5 7.7-11.8 7.7zm14.9-15v-26.2c.3-.4.6-.7.8-1.1 0-.1 0-.1.1-.2 1.9-.8 3.7-1.8 5.5-3.2v4.4c0 12-2.2 16.8-5.7 24.8-.3.5-.5 1-.7 1.5zm107.4-15.3l8.4 10.8c.1.1.1.3 0 .3 0 .1-.1.2-.3.2H330.4c2.2-5.1 3.7-9.5 4.2-16.5h88.8c4 0 7.9 1.9 10.5 5.2zm-65.7-37.7v30.5h-6.1V338.5c1.1.1 2.2.1 3.4.1 1 0 2 0 3-.1-.2.2-.3.4-.3.6zm22.1-6.5c-29.7 9.2-48.8.6-57.8-5.5-.1-.7-.1-1.4-.2-2h6.4c9.1 4.8 19 7.2 29.2 7.2h.9c.1 0 .2.1.3.1.1 0 .2 0 .3-.1 9.8-.2 19.9-2.6 29.7-7.3 27.3-12.8 49.4-39.8 59.9-72.2-3 13.8-8.9 27.6-16.9 39.7-9.1 13.8-25.5 32-51.8 40.1zm65.8-14c.1 1.2-.2 2.3-1 3.1-.8.9-1.9 1.3-3 1.3H415c13.4-8.9 22.8-20.2 29-29.5 3.1-4.6 5.8-9.5 8.2-14.5l3.9 39.6z"/><path d="M322.1 233.3h6.5c.3 0 .5-.2.5-.5s-.2-.5-.5-.5h-5.9l2.2-21.3c0-.3-.2-.5-.4-.5-.3 0-.5.2-.5.4l-2.2 21.9c0 .1 0 .3.1.4-.1 0 0 .1.2.1zm79.7.8h8.3c.3 0 .5-.2.5-.5s-.2-.5-.5-.5h-7.8l2.1-22.1c0-.3-.2-.5-.5-.5s-.5.2-.5.5l-2.2 22.6c0 .1 0 .3.1.4.2 0 .4.1.5.1zm-232.3 8.6c.3.1.7.1 1 .1 1.2 0 2.5-.5 3.3-1.4 1-1 1.4-2.3 1.1-3.6-.1-.5-.7-.9-1.2-.8-.5.1-.9.7-.8 1.2.2.8-.3 1.5-.5 1.8-.6.6-1.6.9-2.5.7-.5-.1-1.1.2-1.2.8-.1.5.3 1.1.8 1.2z"/><path d="M171.4 243.4c-.5 0-1 .4-1 1s.4 1 1 1h.2c2.6 0 5-2 5.5-4.5.1-.5-.2-1.1-.8-1.2-.5-.1-1.1.2-1.2.8-.3 1.7-2 3-3.7 2.9zm-32.3 15.8c.3 0 .7 0 1-.1.5-.1.9-.6.8-1.2-.1-.5-.6-.9-1.2-.8-.9.2-1.8-.1-2.5-.7-.3-.3-.7-.9-.5-1.8.1-.5-.2-1.1-.8-1.2-.5-.1-1.1.2-1.2.8-.3 1.3.1 2.6 1.1 3.6.8.9 2 1.4 3.3 1.4z"/><path d="M138 261.9h.2c.6 0 1-.5 1-1 0-.6-.5-1-1-1-1.7.1-3.4-1.3-3.7-2.9-.1-.5-.6-.9-1.2-.8-.5.1-.9.6-.8 1.2.5 2.5 2.9 4.5 5.5 4.5z"/><path d="M131 264.5c.1 0 .2 0 .4-.1 1.2-.4 2.2-1.1 3-2 .4-.4.3-1-.1-1.4-.4-.4-1-.3-1.4.1-.6.7-1.3 1.2-2.2 1.5-.5.2-.8.8-.6 1.3.1.3.5.6.9.6zm33.7 99.2h-26.1c-4.3 0-7.9-3.5-7.9-7.9v-82c0-.3-.2-.5-.5-.5s-.5.2-.5.5v82c0 4.9 4 8.9 8.9 8.9h26.1c.3 0 .5-.2.5-.5s-.2-.5-.5-.5zm91.6 0h-60.6c-.3 0-.5.2-.5.5s.2.5.5.5h60.6c.3 0 .5-.2.5-.5s-.3-.5-.5-.5z"/></svg>
```

## Rendered output

### class

```oobleck
string | placeholder_svg_tag: string
```

Specify the `class` attribute for the `<svg>` tag.

##### Code

```liquid
{{ 'collection-1' | placeholder_svg_tag: 'custom-class' }}
```

##### Output

```html
<svg class="custom-class" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 525.5 525.5"><path d="M439.9 310.8c-.2.2-.1.5.1.7l13.2 8.7c.1.1.2.1.3.1.2 0 .3-.1.4-.2.2-.2.1-.5-.1-.7l-13.2-8.7c-.3-.2-.6-.1-.7.1z"/><path d="M463.4 235c1.1-9.4-1-18.6-5.1-21.6-1.7-1.2-3.6-1.3-5.4-.3l-.3.3-6.1-9.8-.1-.1-.8-8.1c-.2-1.9-1.7-3.3-3.6-3.3h-33c-1.6-33-14-75.8-44-75.9h-.1c-7.8 0-14.9 3.1-21.1 9.3-12.5 12.5-21 38.1-22.3 66.5h-20.7v-2.5c0-1.5-1.2-2.7-2.7-2.7h-3.8c-1.5 0-2.7 1.2-2.7 2.7v2.5H288c-1.9 0-3.4 1.4-3.6 3.3l-.8 8.4-5.9 9.5c-.1-.1-.3-.3-.5-.3-.8-.2-2.2-.3-3.6.8-.4.3-.7.6-1.1 1.1-8.5 9.5-6.5 32.6-.8 51.2h-34.5c.1-2.1.2-4.6.4-7.3.6-10.3 1.3-23.1.1-30.3-1.7-10.1-8.9-21.5-13.3-26.6-3.9-4.5-9.3-10.8-11.1-12.9 6.2-4 9.6-9.6 10.1-16.6v-.6c.3-3-.4-7.1-2.8-9.7-1.5-1.7-3.4-2.5-5.7-2.5h-39.6c-.3-11.5-6.3-23-19.3-23-4.3 0-8.2 1.7-11.4 4.5l-.2-.1c0 .1-.1.2-.1.4-4.5 4.2-7.4 10.8-7.6 18.3h-34.9c-2.3 0-4.3.8-5.7 2.5-2.3 2.6-3.1 6.7-2.8 9.7v.6c.5 7 3.9 12.6 10.1 16.6-1.9 2.2-7.3 8.4-11.1 12.9-5.4 6.3-11.9 17.3-13.3 26.6-2 12.9-.8 23 .2 32 .9 7.8 1.7 14.6.3 21.6-.8 1.7-1.7 3.6-2.4 5.6-3.2 8.4-4.4 18.9-3.6 23.5.7 3.9 4.3 6.7 8.9 8.3H62.8c-.6 0-1 .4-1 1V389c0 .6.4 1 1 1h59.7c.2.4.4.8.5 1.2 1.1 2.4 2.2 5 3.5 8.2.1.2.2.5.3.7 2.3 5.2 7.5 8.8 13.5 8.8h171.3c6 0 11.2-3.6 13.5-8.8v-.1l.3-.6c1.3-3.2 2.5-5.9 3.5-8.3.2-.4.4-.8.5-1.2H442c.9 0 1.7-.5 2.1-1.3.4-.8.3-1.7-.2-2.4l-8.4-10.8c-3-3.8-7.4-6-12.3-6h-53v-30.5c0-.3-.1-.5-.3-.7 6.3-.4 13.3-1.6 21-4 7.8-2.4 14.7-5.7 20.9-9.5H452c1.7 0 3.4-.7 4.5-2s1.7-3 1.5-4.7l-4.2-42.4c0-.1-.1-.3-.1-.4 5.8-13.2 9.3-27.2 9.7-40.5.1.4.1.3 0 .3zm-9.4-20.2c1.1-.6 2.2-.6 3.2.2 1.9 1.4 3.5 5 4.2 9.7-1.5-1.6-3.8-2-5.7-2.3l-1.5-.3c-1.4-.3-2.2-1-2.5-2.1-.3-1 0-2.2.7-3.3l1 1.6c.2.3.5.5.8.5.2 0 .4 0 .5-.2.5-.3.6-.9.3-1.4l-1.4-2.2c.2-.1.3-.1.4-.2zm-2.8 0c-1.5 1.7-2 3.8-1.5 5.7.5 1.8 1.9 3 4 3.5.5.1 1.1.2 1.6.3 3.1.6 5.1 1.1 5.5 3.8.1.5.5.8.9.8.1 3-.2 6.4-.9 9.8-1.9 8.8-4.6 17.3-8.2 25.5l-5.7-56.1 4.3 6.7zm-50.1-7.5h8.3l3.1 27.6c.1.5-.1.9-.4 1.2-.3.3-.7.5-1.2.5h-11.4c-.5 0-.9-.2-1.2-.5s-.4-.8-.4-1.2l3.2-27.6zm10.2-.4l-.1-.7c-.1-.5-.5-.9-1-.9h-10.1c-.5 0-.9.4-1 .9l-.1.7v-7.7h2.3v.6c0 1.3 1.1 2.4 2.4 2.4h3.2c1.3 0 2.4-1.1 2.4-2.4v-.6h2v7.7zm-49.2-14.7V140c1 .3 2 .5 3.1.5s2.1-.2 3.1-.5v52.2h-6.2zm-32.6 0c1.2-26.6 8.8-50.1 19.9-61.3 2.6-2.6 5.4-4.5 8.4-5.7-1.3 1.6-2.1 3.6-2.1 5.9 0 3.4 1.8 6.3 4.5 8 0 .1-.1.2-.1.4v52.7h-30.6zm-8.2 15.2h8.3l3.1 27.6c.1.5-.1.9-.4 1.2s-.7.5-1.2.5h-11.4c-.5 0-.9-.2-1.2-.5s-.4-.8-.4-1.2l3.2-27.6zm10.2-.4l-.1-.7c-.1-.5-.5-.9-1-.9h-10.1c-.5 0-.9.4-1 .9l-.1.7v-7.7h2.1v.5c0 1.3 1.1 2.4 2.4 2.4h3c1.3 0 2.4-1.1 2.4-2.4v-.6h2.3v7.8zm33.6-83.2c.6 0 1.2 0 1.7.1 3.3.8 5.8 3.7 5.8 7.2 0 4.1-3.3 7.4-7.4 7.4s-7.4-3.3-7.4-7.4c0-3.5 2.4-6.4 5.7-7.2.5-.1 1-.1 1.6-.1zm5 15.3c2.7-1.7 4.4-4.6 4.4-8 0-2.3-.8-4.3-2.1-6 17.4 6.6 27.3 36.7 28.7 67.1h-31v-52.7c.1-.2.1-.3 0-.4zm-24.8-12c5.8-5.8 12.5-8.8 19.7-8.8h.1c31 .1 42.2 48.8 42.2 81.5 0 .2-.2.4-.4.4h-3.2c-.2 0-.4-.2-.4-.4 0-2.1 0-4.1-.1-6.2.1-.1.1-.3.1-.5s-.1-.4-.2-.5c-1.5-34.5-14-68.8-36.1-70.8-.6-.1-1.3-.2-2-.2s-1.4.1-2 .2c-5.5.5-10.6 3.1-15.2 7.6-12.6 12.5-20.7 40.1-20.7 70.3 0 .2-.2.4-.4.4h-3c-.2 0-.4-.2-.4-.4.1-30.8 8.7-59.3 22-72.6zM299 208h-5.3l1.7-13.5h1.8L299 208zm-5.4-16v-2.3c0-.4.3-.7.7-.7h3.8c.4 0 .7.3.7.7v2.5h-5.4c.2-.1.2-.1.2-.2zm-7.1 3.7c.1-.8.8-1.5 1.6-1.5h5.3l-1.9 14.7c0 .3.1.6.2.8.2.2.5.3.8.3h7.6c.3 0 .6-.1.8-.3.2-.2.3-.5.2-.8l-1.9-14.7h22.3c0 1-.1 2-.1 3.1h-3.1c-.6 0-1 .4-1 1v11.8c0 .6.4 1 1 1 .2 0 .4-.1.6-.2l-2.7 23.9c-.1 1 .2 2 .9 2.8.7.8 1.6 1.2 2.7 1.2h11.4c1 0 2-.4 2.7-1.2.7-.8 1-1.8.9-2.8l-2.7-23.9c.2.1.3.2.6.2.6 0 1-.4 1-1v-11.8c0-.6-.4-1-1-1H329.4c0-1 0-2.1.1-3.1h71.9c0 1 .1 2 .1 3h-3.3c-.6 0-1 .4-1 1V210c0 .6.4 1 1 1 .2 0 .4-.1.6-.2l-2.7 23.9c-.1 1 .2 2 .9 2.8.7.8 1.6 1.2 2.7 1.2h11.4c1 0 2-.4 2.7-1.2.7-.8 1-1.8.9-2.8l-2.7-23.9c.2.1.3.2.6.2.6 0 1-.4 1-1v-11.8c0-.6-.4-1-1-1h-3c0-1 0-2-.1-3.1h32.9c.8 0 1.5.6 1.6 1.5l7.3 72.1c-11.7 24.7-30.6 45-52.5 55.3h-66.3c0-.4-.1-.9-.1-1.3-.5-4.8-.9-9.5-1.3-14.1h81.6c.3 0 .5-.2.5-.5s-.2-.5-.5-.5H331c-.6-7.5-1.1-14.8-1.1-22v-15.1c0-1.8-1.5-3.3-3.3-3.3h-22.2v-5.7c0-.6-.4-1-1-1h-17.2c-.6 0-1 .4-1 1v5.7h-5.5l6.8-70.5zm75.6 134.2V325h6.1v5.1c-2.1.1-4.1 0-6.1-.2zm-18.6-4.9h16.6v4.6c-5.7-.7-11.3-2.2-16.6-4.6zm26.7 0h23.6c-7.9 3.1-15.8 4.8-23.6 5.1V325zm-10.1 44.6h-25.3c.1-1.2.1-2.5.1-3.8v-6.2c1.1-1.1 2.1-2.3 3.1-3.6.2-.2.2-.5.2-.8l-1.8-11.2c-.1-.4-.4-.7-.8-.8-.4-.1-.8.1-1 .5-.1.2-.3.5-.4.7-.4-5-.8-9.9-1.2-14.8 5.8 3.7 14.8 7.8 27.3 8.8 0 .1-.1.2-.1.3v30.9zm-81.5 6.8h.7v9.6h-.7v-9.6zm-2 16.3h-10.9v-16.3h4.5v8.9c0 .6.4 1 1 1s1-.4 1-1v-8.9h4.5v16.3zm-101.2 1h10.9v8.7l-5.5 4.4-5.5-4.4v-8.7zm-2-7.8h-.7v-9.6h.7v9.6zm2 1v-10.6h4.5v8.9c0 .6.4 1 1 1s1-.4 1-1v-8.9h4.5v15.3h-10.9v-4.7zm0-30.7h10.9v18.2h-4.5v-1c0-.6-.4-1-1-1s-1 .4-1 1v1h-4.5v-18.2zm12.9 20.2h.7v9.6h-.7v-9.6zm-.4 27.3c.2-.2.4-.5.4-.8v-9.2h1.3c.6 0 1-.4 1-1s-.4-1-1-1h-1.3v-3.8h1.7c.6 0 1-.4 1-1v-11.6c0-.6-.4-1-1-1h-1.7v-4.1c.2.2.4.3.7.3h74.4c.1 0 .2 0 .3-.1v3.8H262c-.6 0-1 .4-1 1v11.6c0 .6.4 1 1 1h1.7v4.8h-1.3c-.6 0-1 .4-1 1s.4 1 1 1h1.3v8.2c0 .3.1.6.4.8l4.3 3.4h-84.8l4.3-3.3zm75.8-17.8h-.7v-9.6h.7v9.6zm2 16.6v-7.7h10.9v7.7l-5.5 4.4-5.4-4.4zm6.5-28.1v-1c0-.6-.4-1-1-1s-1 .4-1 1v1h-4.5v-18.2h10.9v18.2h-4.4zm6.4-18.2h2.8c.6 0 1-.4 1-1s-.4-1-1-1h-20.6c-.6 0-1 .4-1 1s.4 1 1 1h2.8v12.5c-.1 0-.2-.1-.3-.1H189c-.3 0-.6.1-.7.3v-12.8h2.8c.6 0 1-.4 1-1s-.4-1-1-1h-20.6c-.6 0-1 .4-1 1s.4 1 1 1h2.8v12.6c-.1-.1-.3-.1-.5-.1h-37.2c-6.2 0-11.2-5-11.2-11.2v-88c0-.7.6-1.3 1.3-1.3h51.7c2 3.3 6.8 9.6 17.9 17.6l-1.1 1.4c-.2.2-.2.5-.2.7 0 .3.2.5.4.7l4 3.1-.6.8c-.3.4-.3 1.1.2 1.4.2.1.4.2.6.2.3 0 .6-.1.8-.4l.6-.8 4 3.1c.2.1.4.2.6.2.3 0 .6-.1.8-.4l1.1-1.4 4.7 3.6c-.1.1-.2.1-.3.2-.8 1.1-1.2 2.5-1 3.8.2 1.4.9 2.6 2 3.5l48.7 37.3c.9.7 2 1.1 3.2 1.1h.7c1.4-.2 2.6-.9 3.5-2 .2-.2.2-.5.2-.7 21.9 14.6 38.4 24.9 51.4 24.9 1.5 0 3-.2 4.5-.5-2.1 1.9-4.8 3-7.6 3h-37.7v-12.3zM152.6 197v5h-6.5v-5h6.5zm-6.5 6h6.5v3.2h-6.5V203zm7.5 5.2c.6 0 1-.4 1-1V197h6.2v10.2c0 .6.4 1 1 1h2.9c.2 10.1 1.1 18.1 3 24.4h-18.9c1.7-7.8 2.6-16.3 2.2-24.4h2.6zm9.2-2V203h6.5v3.2h-6.5zm6.6-4.2h-6.5v-5h6.5v5zm-1 32.6c.5 1.6 1.1 3 1.8 4.3.2.3.5.6.9.6.2 0 .3 0 .4-.1.5-.2.7-.8.4-1.3-.5-1-1-2.2-1.4-3.4H208v8.6h-25.4c-.3 0-.5.2-.5.5s.2.5.5.5H208v4h-27.1c-.7-.3-3.4-2.6-4.2-3.5-.4-.4-1-.5-1.4-.1-.4.4-.5 1-.1 1.4.4.4 1.3 1.3 2.4 2.2h-34c.6-1.3 1.2-2.6 1.7-4h19.4c.3 0 .5-.2.5-.5s-.2-.5-.5-.5h-19c1-2.7 1.9-5.6 2.6-8.6h20.1zm30.6 25.5h-4.6l1.5-9.5h1.6l1.5 9.5zm-55.4-17h-34.9v-8.6h37.6c-.8 3.1-1.7 6-2.7 8.6zm-34.9 1h34.5c-.6 1.4-1.2 2.8-1.8 4h-32.7v-4zm4.3 6.1h27.3c-.7 1.3-1.5 2.5-2.3 3.6-.3.4-.2 1.1.2 1.4.2.1.4.2.6.2.3 0 .6-.1.8-.4 1-1.4 2-3 2.9-4.8h51.3l-1.7 10.8c0 .3 0 .6.2.8.2.2.5.4.8.4h6.9c.3 0 .6-.1.8-.4.2-.2.3-.5.2-.8l-1.7-10.8h4.8v16h-11.9c-2.5-2.7-3.6-4.5-3.7-4.6-.2-.4-.7-.6-1.1-.4l-10.7 3.1c-.3.1-.5.2-.6.5-.1.2-.2.5-.1.8l.2.6h-8.8v-5.7c0-.6-.4-1-1-1h-17.2c-.6 0-1 .4-1 1v5.7h-22.6c-1.8 0-3.3 1.5-3.3 3.3v15.1c0 5.5-.3 11-.7 16.6h-2.7c-5.4-.4-6.1-2.8-6.1-4.9v-46.1zm207.4 18v85.3c-11.3.5-26.1-9.9-43.2-21.8-.3-.2-.6-.4-.9-.7 1.7-2.3 1.3-5.5-1-7.3l-48.6-37.3c-1.1-.8-2.5-1.2-3.9-1-1.4.2-2.6.9-3.5 2 0 0-.1.1-.1.2l-4.7-3.6 1-1.3c.2-.2.2-.5.2-.7 0-.3-.2-.5-.4-.7l-4-3.1.6-.8c.3-.4.3-1.1-.2-1.4-.4-.3-1.1-.3-1.4.2l-.6.8-4-3.1c-.4-.3-1.1-.3-1.4.2l-1.1 1.4c-3.8-2.5-6.8-5-9-7.2h126.2zm-18.3-2h-15.2v-4.7h15.2v4.7zm25.5 85c-1.4.9-3 1.5-4.6 1.9-.5.1-1 .2-1.6.2v-85.2h4.8c.7 0 1.3.6 1.3 1.3v81.7c0 .1.1.1.1.1zm2.5-29.3c.8 8.1 1.6 16.5 2.2 25.1-.9 1.1-1.8 2-2.7 2.8v-33.2c.1 1.8.3 3.5.5 5.3zm-68.2 15.2c1.7 1.1 3.3 2.2 4.9 3.3-.2.1-.4.2-.5.3-.5.7-1.3 1.1-2.1 1.2-.8.1-1.7-.1-2.4-.6l-2.2-1.7c.2 0 .5-.1.6-.4l1.7-2.1zm-3.3 1c-.2.2-.2.5-.2.7l-7.8-6c.2 0 .5-.1.6-.4l2.4-3.1 6.9-9 2.7-3.5 7.4 5.7-12 15.6zm-80.1-72.2l8.9-2.6c1.3 1.9 5.7 7.7 14.7 13.6l-5.6 7.3c-12.6-9.1-16.8-15.9-18-18.3zm18.4 21.1l3.2 2.5-.5.6-3.2-2.5.5-.6zm4.8 3.7l3.2 2.5-.5.6-3.2-2.5.5-.6zm-3.6-5.3l5.6-7.3 8.1 6.2-5.6 7.3-8.1-6.2zm14.9-2.7l-3.2-2.5.4-.5 3.2 2.5-.4.5zm-4.8-3.7l-3.2-2.5.4-.5 3.2 2.5-.4.5zm5.2 6.5l10.3 7.9-5.7 7.4-10.3-7.9 5.7-7.4zm11.5 6.3l-4.1-3.2.1-.1c.5-.7 1.3-1.1 2.1-1.2.9-.1 1.7.1 2.4.6l1.6 1.2-2.1 2.7zm-12.4 7.7c.1-.1.1-.2.2-.3l4.1 3.2-2.2 2.8-1.5-1.2c-.7-.5-1.1-1.3-1.2-2.1s.1-1.7.6-2.4zm13.4-5.7l2.7-3.5 7.4 5.7-9.6 12.5-2.8 3.6-7.4-5.7 9.7-12.6zm26.7 33.5l-24-18.4 5.7-7.4 24 18.4-5.7 7.4zm6.9-9l-24-18.4 2.1-2.7 24 18.4-2.1 2.7zm-32.1-7.8l24 18.4-1.7 2.3c-.2.2-.2.5-.2.7l-24.2-18.6 2.1-2.8zm44.7 13.3l2 1.5c1.4 1.1 1.7 3.1.6 4.5v.1c-1.5-1.1-3.1-2.2-4.7-3.3l2.1-2.8zm-121.7-57.6v-4.7h15.2v4.7h-15.2zm112.7 69.3l5.7-7.4c2.5 1.7 4.9 3.4 7.3 5.1 19.5 13.7 34.9 24.4 47.3 21.8 4.1-.9 7.6-3.2 10.6-7l.3-.3c.2-.3.4-.5.6-.8l1.3 8.2c-15 19.2-35.7 5.5-73.1-19.6zm15.1-77.8c-5-7.8-7.1-17.4-7.3-25.5.2.4.5.6.9.6.1 0 .2 0 .4-.1.5-.2.8-.8.6-1.3-.8-2 1.6-4.1 4.1-6.4 2.4-2.2 4.8-4.4 4.7-6.9-.1-1.3-.8-2.5-2.2-3.6l3.8-6.1-5 49.3zm-1.5-42.8l-1.4 2.2c-.3.5-.1 1.1.3 1.4.2.1.3.2.5.2.3 0 .7-.2.8-.5l1.3-2.1c.8.7 1.2 1.3 1.2 2 .1 1.6-2 3.5-4.1 5.3-1.8 1.6-3.8 3.4-4.5 5.4.2-5.4 1.3-9.8 2.9-12.2.1-.2.2-.3.3-.5.3-.3.5-.6.8-.8.7-.4 1.3-.5 1.9-.4zm-7.7 17.7c0 1 .1 2 .2 3.1.8 9.6 4 18.5 8.8 25.1l-.5 5.4H274c-3.6-11.1-5.6-23.5-5-33.6zm-46.1-29.3c4.3 5 11.2 15.9 12.8 25.6 1.2 7 .4 20.2-.1 29.9-.2 2.7-.3 5.2-.4 7.3h-29v-16h2.8c.6 0 1-.4 1-1v-15.6c0-.6-.4-1-1-1h-39.2c-1.9-6.1-2.9-14.3-3.1-24.4h3.6c.6 0 1-.4 1-1V197h2.8c16.7 0 29.1-2.3 37.4-6.9 1.7 1.9 7.4 8.5 11.4 13.2zm-10-40.3c1.7 1.8 2.2 4.8 1.9 6.8v.5c-1 12.7-15.2 19.1-42.4 19.1h-28.1c-27.1 0-41.4-6.4-42.4-19.1v-.5c-.2-2.1.3-5 1.9-6.8.5-.6 1.1-1 1.8-1.3H211c.7.3 1.3.7 1.9 1.3zm-39.6-3.3h-6c.7-2.7 2.1-9.2 1.2-15.2 3.3 4.1 4.7 9.8 4.8 15.2zm-7.5-18c2.3 6.5.1 15.6-.6 18h-18.4c-.6-2.3-2.7-10.7-.8-17.2 2.8-2.5 6.2-3.9 10-3.9 4.1-.1 7.3 1.1 9.8 3.1zm-22.4 3.6c-.7 5.8.7 11.8 1.3 14.4h-6c.2-5.7 1.9-10.7 4.7-14.4zm-48.1 27c0-.2 0-.5-.1-.7-.2-2.5.4-6.1 2.3-8.2 1.1-1.2 2.5-1.8 4.3-1.8h2c-.2.2-.5.4-.7.6-1.9 2.1-2.4 5.3-2.2 7.6v.5c1 13.3 15.6 20 43.4 20h28.1c27.7 0 42.3-6.7 43.4-20v-.5c.2-2.3-.3-5.6-2.2-7.6-.2-.2-.4-.4-.7-.6h2c1.7 0 3.2.6 4.3 1.8 1.9 2.1 2.5 5.7 2.3 8.2 0 .2 0 .4-.1.7-1.1 15-17 22.7-47.3 22.7h-31.6c-30.2 0-46.1-7.6-47.2-22.7zm-14.1 88.1c-1-8.8-2.2-18.9-.2-31.5 1.5-9.5 8.5-20.6 12.8-25.6 4-4.7 9.7-11.3 11.4-13.2 8.2 4.6 20.7 6.9 37.4 6.9h1.6v10.2c0 .6.4 1 1 1h3.8c.4 8.1-.5 16.6-2.2 24.4h-39c-.6 0-1 .4-1 1v15.6c0 .6.4 1 1 1h3.3v37H79.5c3-7.4 6.8-12.6 6.9-12.7.3-.4.2-1.1-.2-1.4-.4-.3-1.1-.2-1.4.2-.1.1-1.1 1.6-2.5 3.9.3-5.4-.4-10.8-1.1-16.8zM75.4 311c-.7-4.1.4-14 3.3-21.8H111v7.1c0 4.2 2.7 6.5 8 6.9h2.7c-.4 5.4-.9 10.9-1.5 16.5H94.6c-12.1-.1-18.4-4.6-19.2-8.7zm-11.6 77.1v-66.5H120c-1.4 14.1-2.9 28.7-2.8 44.1 0 10.7 1.8 16 4.5 22.3H63.8zm55.3-22.3c0-15.3 1.4-29.8 2.8-43.9.2-1.8.3-3.5.5-5.2v40.8c0 3.2 1.2 6.2 3.1 8.5v26.2c-.2-.5-.5-1.1-.7-1.6-3.5-8-5.6-12.8-5.7-24.8zm9.5 33.7c-.1-.2-.2-.5-.3-.7-.5-1.4-.8-2.9-.8-4.5v-26.5c2.2 1.8 5.1 2.8 8.1 2.8h37.2c.2 0 .3-.1.5-.1v3.9h-1.7c-.6 0-1 .4-1 1V387c0 .6.4 1 1 1h1.7v3.8H172c-.6 0-1 .4-1 1s.4 1 1 1h1.3v9.2c0 .3.1.6.4.8l4.3 3.4h-37.7c-5.2-.1-9.7-3.2-11.7-7.7zm183 7.6H274l4.3-3.4c.2-.2.4-.5.4-.8v-8.2h1.3c.6 0 1-.4 1-1s-.4-1-1-1h-1.3v-4.8h1.7c.6 0 1-.4 1-1v-11.6c0-.6-.4-1-1-1h-1.7v-3.7h37.7c3 0 5.8-1 8.1-2.8v26.5c0 1.6-.3 3.1-.8 4.5l-.3.6c-2 4.6-6.5 7.7-11.8 7.7zm14.9-15v-26.2c.3-.4.6-.7.8-1.1 0-.1 0-.1.1-.2 1.9-.8 3.7-1.8 5.5-3.2v4.4c0 12-2.2 16.8-5.7 24.8-.3.5-.5 1-.7 1.5zm107.4-15.3l8.4 10.8c.1.1.1.3 0 .3 0 .1-.1.2-.3.2H330.4c2.2-5.1 3.7-9.5 4.2-16.5h88.8c4 0 7.9 1.9 10.5 5.2zm-65.7-37.7v30.5h-6.1V338.5c1.1.1 2.2.1 3.4.1 1 0 2 0 3-.1-.2.2-.3.4-.3.6zm22.1-6.5c-29.7 9.2-48.8.6-57.8-5.5-.1-.7-.1-1.4-.2-2h6.4c9.1 4.8 19 7.2 29.2 7.2h.9c.1 0 .2.1.3.1.1 0 .2 0 .3-.1 9.8-.2 19.9-2.6 29.7-7.3 27.3-12.8 49.4-39.8 59.9-72.2-3 13.8-8.9 27.6-16.9 39.7-9.1 13.8-25.5 32-51.8 40.1zm65.8-14c.1 1.2-.2 2.3-1 3.1-.8.9-1.9 1.3-3 1.3H415c13.4-8.9 22.8-20.2 29-29.5 3.1-4.6 5.8-9.5 8.2-14.5l3.9 39.6z"/><path d="M322.1 233.3h6.5c.3 0 .5-.2.5-.5s-.2-.5-.5-.5h-5.9l2.2-21.3c0-.3-.2-.5-.4-.5-.3 0-.5.2-.5.4l-2.2 21.9c0 .1 0 .3.1.4-.1 0 0 .1.2.1zm79.7.8h8.3c.3 0 .5-.2.5-.5s-.2-.5-.5-.5h-7.8l2.1-22.1c0-.3-.2-.5-.5-.5s-.5.2-.5.5l-2.2 22.6c0 .1 0 .3.1.4.2 0 .4.1.5.1zm-232.3 8.6c.3.1.7.1 1 .1 1.2 0 2.5-.5 3.3-1.4 1-1 1.4-2.3 1.1-3.6-.1-.5-.7-.9-1.2-.8-.5.1-.9.7-.8 1.2.2.8-.3 1.5-.5 1.8-.6.6-1.6.9-2.5.7-.5-.1-1.1.2-1.2.8-.1.5.3 1.1.8 1.2z"/><path d="M171.4 243.4c-.5 0-1 .4-1 1s.4 1 1 1h.2c2.6 0 5-2 5.5-4.5.1-.5-.2-1.1-.8-1.2-.5-.1-1.1.2-1.2.8-.3 1.7-2 3-3.7 2.9zm-32.3 15.8c.3 0 .7 0 1-.1.5-.1.9-.6.8-1.2-.1-.5-.6-.9-1.2-.8-.9.2-1.8-.1-2.5-.7-.3-.3-.7-.9-.5-1.8.1-.5-.2-1.1-.8-1.2-.5-.1-1.1.2-1.2.8-.3 1.3.1 2.6 1.1 3.6.8.9 2 1.4 3.3 1.4z"/><path d="M138 261.9h.2c.6 0 1-.5 1-1 0-.6-.5-1-1-1-1.7.1-3.4-1.3-3.7-2.9-.1-.5-.6-.9-1.2-.8-.5.1-.9.6-.8 1.2.5 2.5 2.9 4.5 5.5 4.5z"/><path d="M131 264.5c.1 0 .2 0 .4-.1 1.2-.4 2.2-1.1 3-2 .4-.4.3-1-.1-1.4-.4-.4-1-.3-1.4.1-.6.7-1.3 1.2-2.2 1.5-.5.2-.8.8-.6 1.3.1.3.5.6.9.6zm33.7 99.2h-26.1c-4.3 0-7.9-3.5-7.9-7.9v-82c0-.3-.2-.5-.5-.5s-.5.2-.5.5v82c0 4.9 4 8.9 8.9 8.9h26.1c.3 0 .5-.2.5-.5s-.2-.5-.5-.5zm91.6 0h-60.6c-.3 0-.5.2-.5.5s.2.5.5.5h60.6c.3 0 .5-.2.5-.5s-.3-.5-.5-.5z"/></svg>
```

## Rendered output

</page>

<page>
---
title: 'Liquid filters: pluralize'
description: Outputs the singular or plural version of a string based on a given number.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/pluralize'
  md: 'https://shopify.dev/docs/api/liquid/filters/pluralize.md'
---

# pluralize

```oobleck
number | pluralize: string, string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Outputs the singular or plural version of a string based on a given number.

***

**Caution:** The \<code>pluralize\</code> filter applies English pluralization rules to determine which string to output. You shouldn\&#39;t use this filter on non-English strings because it could lead to incorrect pluralizations.

***

##### Code

```liquid
Cart item count: {{ cart.item_count }} {{ cart.item_count | pluralize: 'item', 'items' }}
```

##### Data

```json
{
  "cart": {
    "item_count": 2
  }
}
```

##### Output

```html
Cart item count: 2 items
```

</page>

<page>
---
title: 'Liquid filters: plus'
description: Adds two numbers.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/plus'
  md: 'https://shopify.dev/docs/api/liquid/filters/plus.md'
---

# plus

```oobleck
number | plus: number
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Adds two numbers.

##### Code

```liquid
{{ 2 | plus: 2 }}
```

##### Output

```html
4
```

</page>

<page>
---
title: 'Liquid filters: preload_tag'
description: >-
  Generates an HTML `<link>` tag with a `rel` attribute of `preload` to
  prioritize loading a given Shopify-hosted asset.

  The asset URL is also added to the [Link
  header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Link)

  with a `rel` attribute of `preload`.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/preload_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/preload_tag.md'
---

# preload\_​tag

```oobleck
string | preload_tag: as: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<link>` tag with a `rel` attribute of `preload` to prioritize loading a given Shopify-hosted asset. The asset URL is also added to the [Link header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Link) with a `rel` attribute of `preload`.

You should use this filter sparingly. For example, consider preloading only resources necessary for rendering above-the-fold content. To learn more about preloading resources, refer to [Performance best practices for Shopify themes](https://shopify.dev/themes/best-practices/performance#preload-key-resources-defer-or-avoid-loading-others).

***

**Tip:** If you want to preload a stylesheet, then use \<a href="/docs/api/liquid/filters/stylesheet\_tag">\<code>\<span class="PreventFireFoxApplyingGapToWBR">stylesheet\<wbr/>\_tag\</span>\</code>\</a>. If you want to preload an image, then use \<a href="/docs/api/liquid/filters/image\_tag">\<code>\<span class="PreventFireFoxApplyingGapToWBR">image\<wbr/>\_tag\</span>\</code>\</a>.

***

The input to this filter must be a URL from one of the following filters:

* [`asset_url`](https://shopify.dev/docs/api/liquid/filters/asset_url)
* [`global_asset_url`](https://shopify.dev/docs/api/liquid/filters/global_asset_url)
* [`shopify_asset_url`](https://shopify.dev/docs/api/liquid/filters/shopify_asset_url)

The `preload_tag` filter also requires an [`as` parameter](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#attr-as) based on the kind of resource being preloaded.

##### Code

```liquid
{{ 'cart.js' | asset_url | preload_tag: as: 'script' }}
```

##### Output

```html
<link href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/cart.js?v=83971781268232213281663872410" as="script" rel="preload">
```

### HTML attributes

```oobleck
string | preload_tag: as: string, attribute: string
```

You can specify [HTML attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#attributes) by adding a parameter that matches the attribute name, and the desired value.

##### Code

```liquid
{{ 'cart.js' | asset_url | preload_tag: as: 'script', type: 'text/javascript' }}
```

##### Output

```html
<link href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/cart.js?v=83971781268232213281663872410" as="script" type="text/javascript" rel="preload">
```

</page>

<page>
---
title: 'Liquid filters: prepend'
description: Adds a given string to the beginning of a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/prepend'
  md: 'https://shopify.dev/docs/api/liquid/filters/prepend.md'
---

# prepend

```oobleck
string | prepend: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Adds a given string to the beginning of a string.

##### Code

```liquid
{%- assign origin = request.origin -%}

{{ product.url | prepend: origin }}
```

##### Data

```json
{
  "product": {
    "url": "/products/health-potion"
  },
  "request": {
    "origin": "https://polinas-potent-potions.myshopify.com"
  }
}
```

##### Output

```html
https://polinas-potent-potions.myshopify.com/products/health-potion
```

</page>

<page>
---
title: 'Liquid filters: product_img_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for a [product image](/docs/api/liquid/objects/product).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/product_img_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/product_img_url.md'
---

# product\_​img\_​url

```oobleck
variable | product_img_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for a [product image](https://shopify.dev/docs/api/liquid/objects/product).

This can be the product's `featured_image` or any image from the `images` array.

**Deprecated:**

The `product_img_url` filter has been replaced by [`image_url`](https://shopify.dev/docs/api/liquid/filters/image_url).

##### Code

```liquid
{{ product.featured_image | product_img_url }}
```

##### Data

```json
{
  "product": {
    "featured_image": "files/science-beakers-blue-light-new.jpg"
  }
}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_small.jpg?v=1683744744
```

### The size parameter

```oobleck
image | product_img_url: string
```

By default, the `product_img_url` filter returns the `small` version of the image (100 x 100 px). However, you can specify a [size](https://shopify.dev/docs/api/liquid/filters/img_url#img_url-size).

##### Code

```liquid
{{ product.images[0] | product_img_url: 'large' }}
```

##### Data

```json
{
  "product": {
    "images": [
      "files/science-beakers-blue-light-new.jpg",
      "products/science-beakers-blue-light.jpg",
      "files/science-beakers-blue-light_9c5badcd-ea54-4ddc-916c-a45c6c67c704.jpg",
      "files/science-beakers-blue-light_40984233-47bf-4b8b-844c-88020e3da712.jpg"
    ]
  }
}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shop/files/science-beakers-blue-light-new_large.jpg?v=1683744744
```

</page>

<page>
---
title: 'Liquid filters: reject'
description: Filters an array to exclude items with a specific property value.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/reject'
  md: 'https://shopify.dev/docs/api/liquid/filters/reject.md'
---

# reject

```oobleck
array | reject: string, string
```

Filters an array to exclude items with a specific property value.

This requires you to provide both the property name and the associated value.

##### Code

```liquid
{% assign polina_products = collection.products | reject: 'vendor', "Polina's Potent Potions" %}

Products from other vendors than Polina's Potent Potions:

{% for product in polina_products -%}
  - {{ product.title }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Charcoal",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Crocodile tears",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Dandelion milk",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Draught of Immortality",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Dried chamomile",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Forest mushroom",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Gift Card",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Glacier ice",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Ground mandrake root",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Health potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Invisibility potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Komodo dragon scale",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Love Potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Mana potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Potion beats",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Potion bottle",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Viper venom",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Whole bloodroot",
        "vendor": "Clover's Apothecary"
      }
    ]
  }
}
```

##### Output

```html
Products from other vendors than Polina's Potent Potions:

- Charcoal
- Dandelion milk
- Dried chamomile
- Forest mushroom
- Glacier ice
- Ground mandrake root
- Komodo dragon scale
- Viper venom
- Whole bloodroot
```

</page>

<page>
---
title: 'Liquid filters: remove'
description: Removes any instance of a substring inside a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/remove'
  md: 'https://shopify.dev/docs/api/liquid/filters/remove.md'
---

# remove

```oobleck
string | remove: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Removes any instance of a substring inside a string.

##### Code

```liquid
{{ "I can't do it!" | remove: "'t" }}
```

##### Output

```html
I can do it!
```

</page>

<page>
---
title: 'Liquid filters: remove_first'
description: Removes the first instance of a substring inside a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/remove_first'
  md: 'https://shopify.dev/docs/api/liquid/filters/remove_first.md'
---

# remove\_​first

```oobleck
string | remove_first: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Removes the first instance of a substring inside a string.

##### Code

```liquid
{{ "I hate it when I accidentally spill my duplication potion accidentally!" | remove_first: ' accidentally' }}
```

##### Output

```html
I hate it when I spill my duplication potion accidentally!
```

</page>

<page>
---
title: 'Liquid filters: remove_last'
description: Removes the last instance of a substring inside a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/remove_last'
  md: 'https://shopify.dev/docs/api/liquid/filters/remove_last.md'
---

# remove\_​last

```oobleck
string | remove_last: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Removes the last instance of a substring inside a string.

##### Code

```liquid
{{ "I hate it when I accidentally spill my duplication potion accidentally!" | remove_last: ' accidentally' }}
```

##### Output

```html
I hate it when I accidentally spill my duplication potion!
```

</page>

<page>
---
title: 'Liquid filters: replace'
description: Replaces any instance of a substring inside a string with a given string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/replace'
  md: 'https://shopify.dev/docs/api/liquid/filters/replace.md'
---

# replace

```oobleck
string | replace: string, string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Replaces any instance of a substring inside a string with a given string.

##### Code

```liquid
{{ product.handle | replace: '-', ' ' }}
```

##### Data

```json
{
  "product": {
    "handle": "komodo-dragon-scale"
  }
}
```

##### Output

```html
komodo dragon scale
```

</page>

<page>
---
title: 'Liquid filters: replace_first'
description: >-
  Replaces the first instance of a substring inside a string with a given
  string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/replace_first'
  md: 'https://shopify.dev/docs/api/liquid/filters/replace_first.md'
---

# replace\_​first

```oobleck
string | replace_first: string, string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Replaces the first instance of a substring inside a string with a given string.

##### Code

```liquid
{{ product.handle | replace_first: '-', ' ' }}
```

##### Data

```json
{
  "product": {
    "handle": "komodo-dragon-scale"
  }
}
```

##### Output

```html
komodo dragon-scale
```

</page>

<page>
---
title: 'Liquid filters: replace_last'
description: Replaces the last instance of a substring inside a string with a given string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/replace_last'
  md: 'https://shopify.dev/docs/api/liquid/filters/replace_last.md'
---

# replace\_​last

```oobleck
string | replace_last: string, string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Replaces the last instance of a substring inside a string with a given string.

##### Code

```liquid
{{ product.handle | replace_last: '-', ' ' }}
```

##### Data

```json
{
  "product": {
    "handle": "komodo-dragon-scale"
  }
}
```

##### Output

```html
komodo-dragon scale
```

</page>

<page>
---
title: 'Liquid filters: reverse'
description: Reverses the order of the items in an array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/reverse'
  md: 'https://shopify.dev/docs/api/liquid/filters/reverse.md'
---

# reverse

```oobleck
array | reverse
```

Reverses the order of the items in an array.

##### Code

```liquid
Original order:
{{ collection.products | map: 'title' | join: ', ' }}

Reverse order:
{{ collection.products | reverse | map: 'title' | join: ', ' }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
Original order:
Draught of Immortality, Glacier ice, Health potion, Invisibility potion

Reverse order:
Invisibility potion, Health potion, Glacier ice, Draught of Immortality
```

### Reversing strings

You can't use the `reverse` filter on strings directly. However, you can use the [`split` filter](https://shopify.dev/docs/api/liquid/filters/split) to create an array of characters in the string, reverse that array, and then use the [`join` filter](https://shopify.dev/docs/api/liquid/filters/join) to combine them again.

##### Code

```liquid
{{ collection.title | split: '' | reverse | join: '' }}
```

##### Data

```json
{
  "collection": {
    "title": "Sale potions"
  }
}
```

##### Output

```html
snoitop elaS
```

</page>

<page>
---
title: 'Liquid filters: round'
description: Rounds a number to the nearest integer.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/round'
  md: 'https://shopify.dev/docs/api/liquid/filters/round.md'
---

# round

```oobleck
number | round
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Rounds a number to the nearest integer.

##### Code

```liquid
{{ 2.7 | round }}
{{ 1.3 | round }}
```

##### Output

```html
3
1
```

### Round to a specific number of decimal places

You can specify a number of decimal places to round to. If you don't specify a number, then the `round` filter rounds to the nearest integer.

##### Code

```liquid
{{ 3.14159 | round: 2 }}
```

##### Output

```html
3.14
```

</page>

<page>
---
title: 'Liquid filters: rstrip'
description: Strips all whitespace from the right of a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/rstrip'
  md: 'https://shopify.dev/docs/api/liquid/filters/rstrip.md'
---

# rstrip

```oobleck
string | rstrip
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Strips all whitespace from the right of a string.

##### Code

```liquid
{%- assign text = '  Some potions create whitespace.      ' -%}

"{{ text }}"
"{{ text | rstrip }}"
```

##### Output

```html
"  Some potions create whitespace.      "
"  Some potions create whitespace."
```

</page>

<page>
---
title: 'Liquid filters: script_tag'
description: >-
  Generates an HTML `<script>` tag for a given resource URL. The tag has a
  `type` attribute of `text/javascript`.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/script_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/script_tag.md'
---

# script\_​tag

```oobleck
string | script_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<script>` tag for a given resource URL. The tag has a `type` attribute of `text/javascript`.

##### Code

```liquid
{{ 'cart.js' | asset_url | script_tag }}
```

##### Output

```html
<script src="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/cart.js?v=83971781268232213281663872410" type="text/javascript"></script>
```

</page>

<page>
---
title: 'Liquid filters: sha1'
description: >-
  Converts a string into an SHA-1 hash. SHA-1 is not considered safe anymore.
  Please use ['blake3'](https://shopify.dev/docs/api/liquid/filters/blake3)
  instead for better security and performance.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/sha1'
  md: 'https://shopify.dev/docs/api/liquid/filters/sha1.md'
---

# sha1

```oobleck
string | sha1: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string into an SHA-1 hash. SHA-1 is not considered safe anymore. Please use ['blake3'](https://shopify.dev/docs/api/liquid/filters/blake3) instead for better security and performance.

##### Code

```liquid
{%- assign secret_potion = 'Polyjuice' | sha1 -%}

My secret potion: {{ secret_potion }}
```

##### Output

```html
My secret potion: bd0ca3935467e5238d7662ada4df899f09b70d5a
```

</page>

<page>
---
title: 'Liquid filters: sha256'
description: >-
  Converts a string into an SHA-256 hash. Please use
  ['blake3'](https://shopify.dev/docs/api/liquid/filters/blake3) instead for
  better security and performance.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/sha256'
  md: 'https://shopify.dev/docs/api/liquid/filters/sha256.md'
---

# sha256

```oobleck
string | sha256: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string into an SHA-256 hash. Please use ['blake3'](https://shopify.dev/docs/api/liquid/filters/blake3) instead for better security and performance.

##### Code

```liquid
{%- assign secret_potion = 'Polyjuice' | sha256 -%}

My secret potion: {{ secret_potion }}
```

##### Output

```html
My secret potion: 44ac1d7a2936e30a5de07082fd65d6fe9b1fb658a1a98bfe65bc5959beac5dd0
```

</page>

<page>
---
title: 'Liquid filters: shopify_asset_url'
description: >-
  Returns the [CDN URL](/themes/best-practices/performance/platform#shopify-cdn)
  for a globally accessible Shopify asset.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/shopify_asset_url'
  md: 'https://shopify.dev/docs/api/liquid/filters/shopify_asset_url.md'
---

# shopify\_​asset\_​url

```oobleck
string | shopify_asset_url
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns the [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for a globally accessible Shopify asset.

The following are the globally accessible Shopify assets:

* `option_selection.js`
* `api.jquery.js`
* `shopify_common.js`
* `customer_area.js`
* `currencies.js`
* `customer.css`

##### Code

```liquid
{{ 'option_selection.js' | shopify_asset_url }}
```

##### Output

```html
//polinas-potent-potions.myshopify.com/cdn/shopifycloud/storefront/assets/themes_support/option_selection-b017cd28.js
```

</page>

<page>
---
title: 'Liquid filters: size'
description: Returns the size of a string or array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/size'
  md: 'https://shopify.dev/docs/api/liquid/filters/size.md'
---

# size

```oobleck
variable | size
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Returns the size of a string or array.

The size of a string is the number of characters that the string includes. The size of an array is the number of items in the array.

##### Code

```liquid
{{ collection.title | size }}
{{ collection.products | size }}
```

##### Data

```json
{
  "collection": {
    "products": [],
    "title": "Sale potions"
  }
}
```

##### Output

```html
12
4
```

### Dot notation

You can use the `size` filter with dot notation when you need to use it inside a tag or object output.

##### Code

```liquid
{% if collection.products.size >= 10 %}
  There are 10 or more products in this collection.
{% else %}
  There are less than 10 products in this collection.
{% endif %}
```

##### Data

```json
{
  "collection": {
    "products": []
  }
}
```

##### Output

```html
There are less than 10 products in this collection.
```

</page>

<page>
---
title: 'Liquid filters: slice'
description: >-
  Returns a substring or series of array items, starting at a given 0-based
  index.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/slice'
  md: 'https://shopify.dev/docs/api/liquid/filters/slice.md'
---

# slice

```oobleck
string | slice
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns a substring or series of array items, starting at a given 0-based index.

By default, the substring has a length of one character, and the array series has one array item. However, you can provide a second parameter to specify the number of characters or array items.

##### Code

```liquid
{{ collection.title | slice: 0 }}
{{ collection.title | slice: 0, 5 }}

{{ collection.all_tags | slice: 1, 2 | join: ', ' }}
```

##### Data

```json
{
  "collection": {
    "all_tags": [
      "Burning",
      "dried",
      "extra-potent",
      "extracts",
      "fresh",
      "healing",
      "ingredients",
      "music",
      "plant",
      "Salty",
      "supplies"
    ],
    "title": "Products"
  }
}
```

##### Output

```html
P
Produ

dried, extra-potent
```

### Negative index

You can supply a negative index which will count from the end of the string.

##### Code

```liquid
{{ collection.title | slice: -3, 3 }}
```

##### Data

```json
{
  "collection": {
    "title": "Products"
  }
}
```

##### Output

```html
cts
```

</page>

<page>
---
title: 'Liquid filters: sort'
description: >-
  Sorts the items in an array in case-sensitive alphabetical, or numerical,
  order.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/sort'
  md: 'https://shopify.dev/docs/api/liquid/filters/sort.md'
---

# sort

```oobleck
array | sort
```

Sorts the items in an array in case-sensitive alphabetical, or numerical, order.

##### Code

```liquid
{% assign tags = collection.all_tags | sort %}

{% for tag in tags -%}
  {{ tag }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "all_tags": [
      "Burning",
      "dried",
      "extra-potent",
      "extracts",
      "fresh",
      "healing",
      "ingredients",
      "music",
      "plant",
      "Salty",
      "supplies"
    ]
  }
}
```

##### Output

```html
Burning
Salty
dried
extra-potent
extracts
fresh
healing
ingredients
music
plant
supplies
```

### Sort by an array item property

```oobleck
array | sort: string
```

You can specify an array item property to sort the array items by. You can sort by any property of the object that you're sorting.

##### Code

```liquid
{% assign products = collection.products | sort: 'price' %}

{% for product in products -%}
  {{ product.title }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "price": "10.00",
        "title": "Blue Mountain Flower"
      },
      {
        "price": "0.00",
        "title": "Charcoal"
      },
      {
        "price": "56.00",
        "title": "Crocodile tears"
      },
      {
        "price": "0.00",
        "title": "Dandelion milk"
      },
      {
        "price": "1000000.00",
        "title": "Draught of Immortality"
      },
      {
        "price": "8.98",
        "title": "Dried chamomile"
      },
      {
        "price": "0.00",
        "title": "Forest mushroom"
      },
      {
        "price": "10.00",
        "title": "Gift Card"
      },
      {
        "price": "0.00",
        "title": "Glacier ice"
      },
      {
        "price": "0.00",
        "title": "Ground mandrake root"
      },
      {
        "price": "10.00",
        "title": "Health potion"
      },
      {
        "price": "250.00",
        "title": "Invisibility potion"
      },
      {
        "price": "0.00",
        "title": "Komodo dragon scale"
      },
      {
        "price": "0.00",
        "title": "Love Potion"
      },
      {
        "price": "10.00",
        "title": "Mana potion"
      },
      {
        "price": "0.00",
        "title": "Potion beats"
      },
      {
        "price": "0.00",
        "title": "Potion bottle"
      },
      {
        "price": "400.00",
        "title": "Viper venom"
      },
      {
        "price": "24.99",
        "title": "Whole bloodroot"
      }
    ]
  }
}
```

##### Output

```html
Charcoal
Dandelion milk
Forest mushroom
Glacier ice
Ground mandrake root
Komodo dragon scale
Love Potion
Potion beats
Potion bottle
Dried chamomile
Blue Mountain Flower
Gift Card
Health potion
Mana potion
Whole bloodroot
Crocodile tears
Invisibility potion
Viper venom
Draught of Immortality
```

</page>

<page>
---
title: 'Liquid filters: sort_by'
description: >-
  Generates a collection URL with the provided `sort_by` parameter appended.

  This filter must be applied to the object property
  [`collection.url`](https://shopify.dev/docs/api/liquid/objects/collection#collection-url).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/sort_by'
  md: 'https://shopify.dev/docs/api/liquid/filters/sort_by.md'
---

# sort\_​by

```oobleck
string | sort_by: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates a collection URL with the provided `sort_by` parameter appended. This filter must be applied to the object property [`collection.url`](https://shopify.dev/docs/api/liquid/objects/collection#collection-url).

Accepts the following values:

* `manual` (as defined in the [collection settings](https://help.shopify.com/manual/products/collections/collection-layout#change-the-sort-order-for-the-products-in-a-collection))
* `best-selling`
* `title-ascending`
* `title-descending`
* `price-ascending`
* `price-descending`
* `created-ascending`
* `created-descending`

***

**Tip:** You can append the \<code>\<span class="PreventFireFoxApplyingGapToWBR">sort\<wbr/>\_by\</span>\</code> filter to the \<a href="/docs/api/liquid/filters/url\_for\_type">\<code>\<span class="PreventFireFoxApplyingGapToWBR">url\<wbr/>\_for\<wbr/>\_type\</span>\</code>\</a> and \<a href="/docs/api/liquid/filters/url\_for\_vendor">\<code>\<span class="PreventFireFoxApplyingGapToWBR">url\<wbr/>\_for\<wbr/>\_vendor\</span>\</code>\</a> filters.

***

##### Code

```liquid
{{ collection.url | sort_by: 'best-selling' }}
```

##### Data

```json
{
  "collection": {
    "url": "/collections/sale-potions"
  }
}
```

##### Output

```html
/collections/sale-potions?sort_by=best-selling
```

</page>

<page>
---
title: 'Liquid filters: sort_natural'
description: Sorts the items in an array in case-insensitive alphabetical order.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/sort_natural'
  md: 'https://shopify.dev/docs/api/liquid/filters/sort_natural.md'
---

# sort\_​natural

```oobleck
array | sort_natural
```

Sorts the items in an array in case-insensitive alphabetical order.

***

**Caution:** You shouldn\&#39;t use the \<code>\<span class="PreventFireFoxApplyingGapToWBR">sort\<wbr/>\_natural\</span>\</code> filter to sort numerical values. When comparing items an array, each item is converted to a string, so sorting on numerical values can lead to unexpected results.

***

##### Code

```liquid
{% assign tags = collection.all_tags | sort_natural %}

{% for tag in tags -%}
  {{ tag }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "all_tags": [
      "Burning",
      "dried",
      "extra-potent",
      "extracts",
      "fresh",
      "healing",
      "ingredients",
      "music",
      "plant",
      "Salty",
      "supplies"
    ]
  }
}
```

##### Output

```html
Burning
dried
extra-potent
extracts
fresh
healing
ingredients
music
plant
Salty
supplies
```

### Sort by an array item property

```oobleck
array | sort_natural: string
```

You can specify an array item property to sort the array items by.

##### Code

```liquid
{% assign products = collection.products | sort_natural: 'title' %}

{% for product in products -%}
  {{ product.title }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower"
      },
      {
        "title": "Charcoal"
      },
      {
        "title": "Crocodile tears"
      },
      {
        "title": "Dandelion milk"
      },
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Dried chamomile"
      },
      {
        "title": "Forest mushroom"
      },
      {
        "title": "Gift Card"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Ground mandrake root"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      },
      {
        "title": "Komodo dragon scale"
      },
      {
        "title": "Love Potion"
      },
      {
        "title": "Mana potion"
      },
      {
        "title": "Potion beats"
      },
      {
        "title": "Potion bottle"
      },
      {
        "title": "Viper venom"
      },
      {
        "title": "Whole bloodroot"
      }
    ]
  }
}
```

##### Output

```html
Blue Mountain Flower
Charcoal
Crocodile tears
Dandelion milk
Draught of Immortality
Dried chamomile
Forest mushroom
Gift Card
Glacier ice
Ground mandrake root
Health potion
Invisibility potion
Komodo dragon scale
Love Potion
Mana potion
Potion beats
Potion bottle
Viper venom
Whole bloodroot
```

</page>

<page>
---
title: 'Liquid filters: split'
description: Splits a string into an array of substrings based on a given separator.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/split'
  md: 'https://shopify.dev/docs/api/liquid/filters/split.md'
---

# split

```oobleck
string | split: string
```

returns array of [string](https://shopify.dev/docs/api/liquid/basics#string)

Splits a string into an array of substrings based on a given separator.

##### Code

```liquid
{%- assign title_words = product.handle | split: '-' -%}

{% for word in title_words -%}
  {{ word }}
{%- endfor %}
```

##### Data

```json
{
  "product": {
    "handle": "health-potion"
  }
}
```

##### Output

```html
health
potion
```

</page>

<page>
---
title: 'Liquid filters: strip'
description: Strips all whitespace from the left and right of a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/strip'
  md: 'https://shopify.dev/docs/api/liquid/filters/strip.md'
---

# strip

```oobleck
string | strip
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Strips all whitespace from the left and right of a string.

##### Code

```liquid
{%- assign text = '  Some potions create whitespace.      ' -%}

"{{ text }}"
"{{ text | strip }}"
```

##### Output

```html
"  Some potions create whitespace.      "
"Some potions create whitespace."
```

</page>

<page>
---
title: 'Liquid filters: strip_html'
description: Strips all HTML tags from a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/strip_html'
  md: 'https://shopify.dev/docs/api/liquid/filters/strip_html.md'
---

# strip\_​html

```oobleck
string | strip_html
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Strips all HTML tags from a string.

##### Code

```liquid
<!-- With HTML -->
{{ product.description }}

<!-- HTML stripped -->
{{ product.description | strip_html }}
```

##### Data

```json
{
  "product": {
    "description": "<h3>Are you low on health? Well we've got the potion just for you!</h3>\n<p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p>"
  }
}
```

##### Output

```html
<!-- With HTML -->
<h3>Are you low on health? Well we've got the potion just for you!</h3>
<p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p>

<!-- HTML stripped -->
Are you low on health? Well we've got the potion just for you!
Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!
```

</page>

<page>
---
title: 'Liquid filters: strip_newlines'
description: Strips all newline characters (line breaks) from a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/strip_newlines'
  md: 'https://shopify.dev/docs/api/liquid/filters/strip_newlines.md'
---

# strip\_​newlines

```oobleck
string | strip_newlines
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Strips all newline characters (line breaks) from a string.

##### Code

```liquid
<!-- With newlines -->
{{ product.description }}

<!-- Newlines stripped -->
{{ product.description | strip_newlines }}
```

##### Data

```json
{
  "product": {
    "description": "<h3>Are you low on health? Well we've got the potion just for you!</h3>\n<p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p>"
  }
}
```

##### Output

```html
<!-- With newlines -->
<h3>Are you low on health? Well we've got the potion just for you!</h3>
<p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p>

<!-- Newlines stripped -->
<h3>Are you low on health? Well we've got the potion just for you!</h3><p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p>
```

</page>

<page>
---
title: 'Liquid filters: structured_data'
description: Converts an object into a schema.org structured data format.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/structured_data'
  md: 'https://shopify.dev/docs/api/liquid/filters/structured_data.md'
---

# structured\_​data

```oobleck
variable | structured_data
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts an object into a schema.org structured data format.

The `structured_data` filter can be used on the [`product`](https://shopify.dev/docs/api/liquid/objects/product) and [`article`](https://shopify.dev/docs/api/liquid/objects/article) objects.

Product objects are output as a [schema.org `Product`](https://schema.org/Product) if they have no variants, and a [`ProductGroup`](https://schema.org/ProductGroup) if they have one or more variants.

Article objects are output as a [schema.org `Article`.](https://schema.org/Article)

##### Code

```liquid
<script type="application/ld+json">
  {{ product | structured_data }}
</script>
```

##### Output

```html
<script type="application/ld+json">
  {"@context":"http:\/\/schema.org\/","@id":"\/products\/crocodile-tears#product","@type":"Product","brand":{"@type":"Brand","name":"Polina's Potent Potions"},"category":"","description":"","image":"https:\/\/polinas-potent-potions.myshopify.com\/cdn\/shop\/products\/amber-beard-oil-bottle.jpg?v=1650642958\u0026width=1920","name":"Crocodile tears","offers":{"@id":"\/products\/crocodile-tears?variant=39888242344001#offer","@type":"Offer","availability":"http:\/\/schema.org\/OutOfStock","price":"56.00","priceCurrency":"CAD","url":"https:\/\/polinas-potent-potions.myshopify.com\/products\/crocodile-tears?variant=39888242344001"},"url":"https:\/\/polinas-potent-potions.myshopify.com\/products\/crocodile-tears"}
</script>
```

</page>

<page>
---
title: 'Liquid filters: stylesheet_tag'
description: >-
  Generates an HTML `<link>` tag for a given resource URL. The tag has the
  following parameters:


  | Attribute | Value |

  | --- | --- |

  | `rel` | `stylesheet` |

  | `type` | `text/css` |

  | `media` | `all` |
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/stylesheet_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/stylesheet_tag.md'
---

# stylesheet\_​tag

```oobleck
string | stylesheet_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<link>` tag for a given resource URL. The tag has the following parameters:

| Attribute | Value |
| - | - |
| `rel` | `stylesheet` |
| `type` | `text/css` |
| `media` | `all` |

##### Code

```liquid
{{ 'base.css' | asset_url | stylesheet_tag }}
```

##### Output

```html
<link href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/base.css?v=88290808517547527771663872409" rel="stylesheet" type="text/css" media="all" />
```

### preload

```oobleck
stylesheet_url | stylesheet_tag: preload: boolean
```

Specify whether the stylesheet should be preloaded.

When `preload` is set to `true`, a resource hint is sent as a [Link header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Link) with a `rel` value of [`preload`](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preload).

```liquid
Link: <STYLESHEET_URL>; rel=preload; as=style
```

```liquid
Link: 
```

This option doesn't affect the HTML link tag directly.

You should use the `preload` parameter sparingly. For example, consider preloading only render-blocking stylesheets that are needed for initial functionality of the page, such as above-the-fold content. To learn more about resource hints in Shopify themes, refer to [Performance best practices for Shopify themes](https://shopify.dev/themes/best-practices/performance#preload-key-resources-defer-or-avoid-loading-others).

</page>

<page>
---
title: 'Liquid filters: sum'
description: Returns the sum of all elements in an array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/sum'
  md: 'https://shopify.dev/docs/api/liquid/filters/sum.md'
---

# sum

```oobleck
array | sum
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Returns the sum of all elements in an array.

##### Code

```liquid
{% assign fibonacci = '0, 1, 1, 2, 3, 5' | split: ', ' %}

{{ fibonacci | sum }}
```

##### Output

```html
12
```

### Sum object property values

```oobleck
array | sum: string
```

For an array of Liquid objects, you can specify a property to sum.

##### Code

```liquid
Total quantity of all items in cart:
{{ cart.items | sum: 'quantity' }}

Subtotal price for all items in cart:
{{ cart.items | sum: 'final_line_price' | money }}
```

##### Data

```json
{
  "cart": {
    "items": [
      {
        "final_line_price": "22.49",
        "quantity": 1
      },
      {
        "final_line_price": "400.00",
        "quantity": 1
      }
    ]
  }
}
```

##### Output

```html
Total quantity of all items in cart:
2

Subtotal price for all items in cart:
$422.49
```

</page>

<page>
---
title: 'Liquid filters: time_tag'
description: Converts a timestamp into an HTML `<time>` tag.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/time_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/time_tag.md'
---

# time\_​tag

```oobleck
string | time_tag: string
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a timestamp into an HTML `<time>` tag.

The `time_tag` filter accepts the same parameters as Ruby's strftime method for formatting the date. For a list of shorthand formats, refer to the [Ruby documentation](https://ruby-doc.org/core-3.1.1/Time.html#method-i-strftime) or [strftime reference and sandbox](http://www.strfti.me/).

##### Code

```liquid
{{ article.created_at | time_tag: '%B %d, %Y' }}
```

##### Data

```json
{
  "article": {
    "created_at": "2022-04-14 16:56:02 -0400"
  }
}
```

##### Output

```html
<time datetime="2022-04-14T20:56:02Z">April 14, 2022</time>
```

### format

```oobleck
string | time_tag: format: string
```

Specify a locale-aware date format. Accepts the following values:

* `abbreviated_date`
* `basic`
* `date`
* `date_at_time`
* `default`
* `on_date`
* `short` (deprecated)
* `long` (deprecated)

***

**Note:** You can also \<a href="/docs/api/liquid/filters/date-setting-format-options-in-locale-files">define custom formats\</a> in your theme\&#39;s locale files.

***

##### Code

```liquid
{{ article.created_at | time_tag: format: 'abbreviated_date' }}
```

##### Data

```json
{
  "article": {
    "created_at": "2022-04-14 16:56:02 -0400"
  }
}
```

##### Output

```html
<time datetime="2022-04-14T20:56:02Z">Apr 14, 2022</time>
```

### datetime

```oobleck
string | time_tag: datetime: string
```

By default, the value of the `datetime` attribute of the `<time>` tag is formatted as `YYYY-MM-DDThh:mm:ssTZD`. However, you can specify a custom format with [strftime shorthand formats](https://ruby-doc.org/core-3.1.2/Time.html#method-i-strftime).

##### Code

```liquid
{{ article.created_at | time_tag: '%B %d, %Y', datetime: '%Y-%m-%d' }}
```

##### Data

```json
{
  "article": {
    "created_at": "2022-04-14 16:56:02 -0400"
  }
}
```

##### Output

```html
<time datetime="2022-04-14">April 14, 2022</time>
```

### Setting format options in locale files

You can define custom date formats in your [theme's storefront locale files](https://shopify.dev/themes/architecture/locales/storefront-locale-files). These custom formats should be included in a `date_formats` category:

```json
"date_formats": {
  "month_day_year": "%B %d, %Y"
}
```

```json
"date_formats": {
  "month_day_year": "%B %d, %Y"
}
```

##### Code

```liquid
{{ article.created_at | time_tag: format: 'month_day_year' }}
```

##### Data

```json
{
  "article": {
    "created_at": "2022-04-14 16:56:02 -0400"
  }
}
```

##### Output

```html
<time datetime="2022-04-14T20:56:02Z">April 14, 2022</time>
```

</page>

<page>
---
title: 'Liquid filters: times'
description: Multiplies a number by a given number.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/times'
  md: 'https://shopify.dev/docs/api/liquid/filters/times.md'
---

# times

```oobleck
number | times: number
```

returns [number](https://shopify.dev/docs/api/liquid/basics#number)

Multiplies a number by a given number.

##### Code

```liquid
{{ 2 | times: 2 }}
```

##### Output

```html
4
```

</page>

<page>
---
title: 'Liquid filters: translate'
description: >-
  Returns a string of translated text for a given translation key from a [locale
  file](/themes/architecture/locales).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/translate'
  md: 'https://shopify.dev/docs/api/liquid/filters/translate.md'
---

# translate

```oobleck
string | t
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Returns a string of translated text for a given translation key from a [locale file](https://shopify.dev/themes/architecture/locales).

The `translate` filter has an alias of `t`, which is more commonly used.

***

**Tip:** To learn more about using the \<code>t\</code> filter, refer to \<a href="/themes/architecture/locales/storefront-locale-files#usage">storefront locale file usage\</a> or \<a href="/themes/architecture/locales/schema-locale-files#usage">schema locale file usage\</a>.

***

### Section locales vs. theme locales

The `t` filter can also reference keys defined in the [`locales` object](https://shopify.dev/themes/architecture/sections/section-schema#locales) of section file's `schema` tag. Content that you put in the `schema` under the `locales` object is only accessible to that section. This is useful if you need to make a standalone section that you want to share between themes.

Content that is global to a theme should be placed in the theme's `locales` directory. For example, you could include the expression "See more" in your `locales` directory to create a single translation. You could then use the translation in a blog post and on the product details page.

***

**Note:** Translations in the section\&#39;s \<code>schema\</code> tag that aren\&#39;t part of the \<code>locales\</code> object are used for merchant-facing text shown in the theme editor. These translations don\&#39;t use the \<code>t\</code> filter.

***

</page>

<page>
---
title: 'Liquid filters: truncate'
description: Truncates a string down to a given number of characters.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/truncate'
  md: 'https://shopify.dev/docs/api/liquid/filters/truncate.md'
---

# truncate

```oobleck
string | truncate: number
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Truncates a string down to a given number of characters.

If the specified number of characters is less than the length of the string, then an ellipsis (`...`) is appended to the truncated string. The ellipsis is included in the character count of the truncated string.

##### Code

```liquid
{{ article.title | truncate: 15 }}
```

##### Data

```json
{
  "article": {
    "title": "How to tell if you're out of invisibility potion"
  }
}
```

##### Output

```html
How to tell ...
```

### Specify a custom ellipsis

```oobleck
string | truncate: number, string
```

You can provide a second parameter to specify a custom ellipsis. If you don't want an ellipsis, then you can supply an empty string.

##### Code

```liquid
{{ article.title | truncate: 15, '--' }}
{{ article.title | truncate: 15, '' }}
```

##### Data

```json
{
  "article": {
    "title": "How to tell if you're out of invisibility potion"
  }
}
```

##### Output

```html
How to tell i--
How to tell if
```

</page>

<page>
---
title: 'Liquid filters: truncatewords'
description: Truncates a string down to a given number of words.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/truncatewords'
  md: 'https://shopify.dev/docs/api/liquid/filters/truncatewords.md'
---

# truncatewords

```oobleck
string | truncatewords: number
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Truncates a string down to a given number of words.

If the specified number of words is less than the number of words in the string, then an ellipsis (`...`) is appended to the truncated string.

***

**Caution:** HTML tags are treated as words, so you should strip any HTML from truncated content. If you don\&#39;t strip HTML, then closing HTML tags can be removed, which can result in unexpected behavior.

***

##### Code

```liquid
{{ article.content | strip_html | truncatewords: 15 }}
```

##### Data

```json
{
  "article": {
    "content": "<p>We've all had this problem before: we peek into the potions vault to determine which potions we are running low on, and the invisibility potion bottle looks completely empty.</p>\n<p>...</p>\n<p> </p>"
  }
}
```

##### Output

```html
We've all had this problem before: we peek into the potions vault to determine which...
```

### Specify a custom ellipsis

```oobleck
string | truncatewords: number, string
```

You can provide a second parameter to specify a custom ellipsis. If you don't want an ellipsis, then you can supply an empty string.

##### Code

```liquid
{{ article.content | strip_html | truncatewords: 15, '--' }}

{{ article.content | strip_html | truncatewords: 15, '' }}
```

##### Data

```json
{
  "article": {
    "content": "<p>We've all had this problem before: we peek into the potions vault to determine which potions we are running low on, and the invisibility potion bottle looks completely empty.</p>\n<p>...</p>\n<p> </p>"
  }
}
```

##### Output

```html
We've all had this problem before: we peek into the potions vault to determine which--

We've all had this problem before: we peek into the potions vault to determine which
```

</page>

<page>
---
title: 'Liquid filters: uniq'
description: Removes any duplicate items in an array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/uniq'
  md: 'https://shopify.dev/docs/api/liquid/filters/uniq.md'
---

# uniq

```oobleck
array | uniq
```

Removes any duplicate items in an array.

##### Code

```liquid
{% assign potion_array = 'invisibility, health, love, health, invisibility' | split: ', ' %}

{{ potion_array | uniq | join: ', ' }}
```

##### Output

```html
invisibility, health, love
```

</page>

<page>
---
title: 'Liquid filters: unit_price_with_measurement'
description: >-
  Formats a given unit price and measurement based on the store's [**HTML
  without currency**
  setting](https://help.shopify.com/manual/payments/currency-formatting).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/unit_price_with_measurement'
  md: 'https://shopify.dev/docs/api/liquid/filters/unit_price_with_measurement.md'
---

# unit\_​price\_​with\_​measurement

```oobleck
number | unit_price_with_measurement: unit_price_measurement
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Formats a given unit price and measurement based on the store's [**HTML without currency** setting](https://help.shopify.com/manual/payments/currency-formatting).

##### Code

```liquid
{%- assign variant = product.variants.first -%}

{{ variant.unit_price | unit_price_with_measurement: variant.unit_price_measurement }}
```

##### Data

```json
{
  "product": {
    "variants": [
      {
        "unit_price": "50.00",
        "unit_price_measurement": "UnitPriceMeasurementDrop"
      },
      {
        "unit_price": "50.00",
        "unit_price_measurement": "UnitPriceMeasurementDrop"
      },
      {
        "unit_price": "25.00",
        "unit_price_measurement": "UnitPriceMeasurementDrop"
      },
      {
        "unit_price": "25.00",
        "unit_price_measurement": "UnitPriceMeasurementDrop"
      }
    ]
  }
}
```

##### Output

```html
$50.00/kg
```

### Formatted unit price

```oobleck
string | unit_price_with_measurement: unit_price_measurement
```

You can specify a formatted unit price using one of the [money filters](https://shopify.dev/docs/api/liquid/filters/payment_button#money-filters).

##### Code

```liquid
{%- assign variant = product.variants.first -%}

{{ variant.unit_price | money_with_currency | unit_price_with_measurement: variant.unit_price_measurement }}
```

##### Data

```json
{
  "product": {
    "variants": [
      {
        "unit_price": "50.00",
        "unit_price_measurement": "UnitPriceMeasurementDrop"
      },
      {
        "unit_price": "50.00",
        "unit_price_measurement": "UnitPriceMeasurementDrop"
      },
      {
        "unit_price": "25.00",
        "unit_price_measurement": "UnitPriceMeasurementDrop"
      },
      {
        "unit_price": "25.00",
        "unit_price_measurement": "UnitPriceMeasurementDrop"
      }
    ]
  }
}
```

##### Output

```html
$50.00 CAD/kg
```

</page>

<page>
---
title: 'Liquid filters: upcase'
description: Converts a string to all uppercase characters.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/upcase'
  md: 'https://shopify.dev/docs/api/liquid/filters/upcase.md'
---

# upcase

```oobleck
string | upcase
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts a string to all uppercase characters.

##### Code

```liquid
{{ product.title | upcase }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
HEALTH POTION
```

</page>

<page>
---
title: 'Liquid filters: url_decode'
description: >-
  Decodes any
  [percent-encoded](https://developer.mozilla.org/en-US/docs/Glossary/percent-encoding)
  characters

  in a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/url_decode'
  md: 'https://shopify.dev/docs/api/liquid/filters/url_decode.md'
---

# url\_​decode

```oobleck
string | url_decode
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Decodes any [percent-encoded](https://developer.mozilla.org/en-US/docs/Glossary/percent-encoding) characters in a string.

##### Code

```liquid
{{ 'test%40test.com' | url_decode }}
```

##### Output

```html
test@test.com
```

</page>

<page>
---
title: 'Liquid filters: url_encode'
description: >-
  Converts any URL-unsafe characters in a string to the

  [percent-encoded](https://developer.mozilla.org/en-US/docs/Glossary/percent-encoding)
  equivalent.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/url_encode'
  md: 'https://shopify.dev/docs/api/liquid/filters/url_encode.md'
---

# url\_​encode

```oobleck
string | url_encode
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Converts any URL-unsafe characters in a string to the [percent-encoded](https://developer.mozilla.org/en-US/docs/Glossary/percent-encoding) equivalent.

***

**Note:** Spaces are converted to a \<code>+\</code> character, instead of a percent-encoded character.

***

##### Code

```liquid
{{ 'test@test.com' | url_encode }}
```

##### Output

```html
test%40test.com
```

</page>

<page>
---
title: 'Liquid filters: url_escape'
description: Escapes any URL-unsafe characters in a string.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/url_escape'
  md: 'https://shopify.dev/docs/api/liquid/filters/url_escape.md'
---

# url\_​escape

```oobleck
string | url_escape
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Escapes any URL-unsafe characters in a string.

##### Code

```liquid
{{ '<p>Health & Love potions</p>' | url_escape }}
```

##### Output

```html
%3Cp%3EHealth%20&%20Love%20potions%3C/p%3E
```

</page>

<page>
---
title: 'Liquid filters: url_for_type'
description: >-
  Generates a URL for a [collection
  page](https://shopify.dev/docs/storefronts/themes/architecture/templates/collection)
  that lists all products of the given product type.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/url_for_type'
  md: 'https://shopify.dev/docs/api/liquid/filters/url_for_type.md'
---

# url\_​for\_​type

```oobleck
string | url_for_type
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates a URL for a [collection page](https://shopify.dev/docs/storefronts/themes/architecture/templates/collection) that lists all products of the given product type.

##### Code

```liquid
{{ 'health' | url_for_type }}
```

##### Output

```html
/collections/types?q=health
```

</page>

<page>
---
title: 'Liquid filters: url_for_vendor'
description: >-
  Generates a URL for a [collection
  page](https://shopify.dev/docs/storefronts/themes/architecture/templates/collection)
  that lists all products from the given product vendor.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/url_for_vendor'
  md: 'https://shopify.dev/docs/api/liquid/filters/url_for_vendor.md'
---

# url\_​for\_​vendor

```oobleck
string | url_for_vendor
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates a URL for a [collection page](https://shopify.dev/docs/storefronts/themes/architecture/templates/collection) that lists all products from the given product vendor.

##### Code

```liquid
{{ "Polina's Potent Potions" | url_for_vendor }}
```

##### Output

```html
/collections/vendors?q=Polina%27s%20Potent%20Potions
```

</page>

<page>
---
title: 'Liquid filters: url_param_escape'
description: Escapes any characters in a string that are unsafe for URL parameters.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/url_param_escape'
  md: 'https://shopify.dev/docs/api/liquid/filters/url_param_escape.md'
---

# url\_​param\_​escape

```oobleck
string | url_param_escape
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Escapes any characters in a string that are unsafe for URL parameters.

The `url_param_escape` filter escapes the same characters as [`url_escape`](https://shopify.dev/docs/api/liquid/filters/url_escape), with the addition of `&`.

##### Code

```liquid
{{ '<p>Health & Love potions</p>' | url_param_escape }}
```

##### Output

```html
%3Cp%3EHealth%20%26%20Love%20potions%3C/p%3E
```

</page>

<page>
---
title: 'Liquid filters: video_tag'
description: Generates an HTML `<video>` tag for a given video.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/video_tag'
  md: 'https://shopify.dev/docs/api/liquid/filters/video_tag.md'
---

# video\_​tag

```oobleck
media | video_tag
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates an HTML `<video>` tag for a given video.

***

**Note:** When \<code>mp4\</code> videos are uploaded, Shopify generates an \<code>m3u8\</code> file as an additional \<a href="/docs/api/liquid/objects/video\_source">\<code>\<span class="PreventFireFoxApplyingGapToWBR">video\<wbr/>\_source\</span>\</code>\</a>. An \<code>m3u8\</code> file enables video players to leverage \<a href="https://developer.apple.com/streaming/">HTTP live streaming (HLS)\</a>, resulting in an optimized video experience based on the user\&#39;s internet connection. If loop is enabled, the HLS source is not used in order to allow progessive download to cache the video.\</p> \<p>If the \<code>m3u8\</code> source isn\&#39;t supported, then the player falls back to the \<code>mp4\</code> source.

***

##### Code

```liquid
{% for media in product.media %}
  {% if media.media_type == 'video' %}
    {{ media | video_tag }}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": [
      {
        "media_type": "external_video"
      },
      {
        "media_type": "video"
      }
    ]
  }
}
```

##### Output

```html
<video playsinline="playsinline" preload="metadata" aria-label="Potion beats" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_small.jpg?v=1655255324"><source src="//polinas-potent-potions.myshopify.com/cdn/shop/videos/c/vp/4edc28a708b7405093a927cebe794f1a/4edc28a708b7405093a927cebe794f1a.HD-1080p-7.2Mbps.mp4?v=0" type="video/mp4"><img src="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_small.jpg?v=1655255324"></video>
```

## Rendered output

### image\_size

```oobleck
media | video_tag: image_size: string
```

Specify the dimensions of the video's poster image in pixels.

##### Code

```liquid
{% for media in product.media %}
  {% if media.media_type == 'video' %}
    {{ media | video_tag: image_size: '400x' }}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": [
      {
        "media_type": "external_video"
      },
      {
        "media_type": "video"
      }
    ]
  }
}
```

##### Output

```html
<video playsinline="playsinline" preload="metadata" aria-label="Potion beats" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_400x.jpg?v=1655255324"><source src="//polinas-potent-potions.myshopify.com/cdn/shop/videos/c/vp/4edc28a708b7405093a927cebe794f1a/4edc28a708b7405093a927cebe794f1a.HD-1080p-7.2Mbps.mp4?v=0" type="video/mp4"><img src="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_400x.jpg?v=1655255324"></video>
```

## Rendered output

### Optional supported HTML5 attributes

```oobleck
media | video_tag: attribute: boolean
```

`video_tag` supports all [HTML5 video attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video#attributes). For example:

| Attribute | Value |
| - | - |
| `autoplay` | Whether to automatically play the video after it’s loaded. Accepted values:`true`,`false` |
| `loop` | Whether to loop the video. Accepted values:`true`,`false` |
| `muted` | Whether to mute the video’s audio. Accepted values:`true`,`false` |
| `controls` | Whether a user can control the video playback. Accepted values:`true`,`false` |

##### Code

```liquid
{% for media in product.media %}
  {% if media.media_type == 'video' %}
    {{ media | video_tag: autoplay: true, loop: true, muted: true, controls: true }}
  {% endif %}
{% endfor %}
```

##### Data

```json
{
  "product": {
    "media": [
      {
        "media_type": "external_video"
      },
      {
        "media_type": "video"
      }
    ]
  }
}
```

##### Output

```html
<video playsinline="playsinline" autoplay="autoplay" loop="loop" muted="muted" controls="controls" preload="metadata" aria-label="Potion beats" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_small.jpg?v=1655255324"><source src="//polinas-potent-potions.myshopify.com/cdn/shop/videos/c/vp/4edc28a708b7405093a927cebe794f1a/4edc28a708b7405093a927cebe794f1a.HD-1080p-7.2Mbps.mp4?v=0" type="video/mp4"><img src="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_small.jpg?v=1655255324"></video>
```

## Rendered output

</page>

<page>
---
title: 'Liquid filters: weight_with_unit'
description: >-
  Generates a formatted weight for a [`variant`
  object](/docs/api/liquid/objects/variant#variant-weight). The weight unit is

  set in the [general settings](https://www.shopify.com/admin/settings/general)
  in the Shopify admin.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/weight_with_unit'
  md: 'https://shopify.dev/docs/api/liquid/filters/weight_with_unit.md'
---

# weight\_​with\_​unit

```oobleck
number | weight_with_unit
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates a formatted weight for a [`variant` object](https://shopify.dev/docs/api/liquid/objects/variant#variant-weight). The weight unit is set in the [general settings](https://www.shopify.com/admin/settings/general) in the Shopify admin.

##### Code

```liquid
{%- assign variant = product.variants.first -%}

{{ variant.weight | weight_with_unit }}
```

##### Data

```json
{
  "product": {
    "variants": [
      {
        "weight": 200
      },
      {
        "weight": 200
      },
      {
        "weight": 400
      },
      {
        "weight": 200
      }
    ]
  }
}
```

##### Output

```html
0.2 kg
```

### Override the default unit

```oobleck
number | weight_with_unit: variable
```

You can specify a unit to override the default from the general settings.

##### Code

```liquid
{%- assign variant = product.variants.first -%}

{{ variant.weight | weight_with_unit: variant.weight_unit }}
```

##### Data

```json
{
  "product": {
    "variants": [
      {
        "weight": 200,
        "weight_unit": "g"
      },
      {
        "weight": 200,
        "weight_unit": "g"
      },
      {
        "weight": 400,
        "weight_unit": "g"
      },
      {
        "weight": 200,
        "weight_unit": "g"
      }
    ]
  }
}
```

##### Output

```html
200 g
```

</page>

<page>
---
title: 'Liquid filters: where'
description: Filters an array to include only items with a specific property value.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/where'
  md: 'https://shopify.dev/docs/api/liquid/filters/where.md'
---

# where

```oobleck
array | where: string, string
```

Filters an array to include only items with a specific property value.

This requires you to provide both the property name and the associated value.

##### Code

```liquid
{% assign polina_products = collection.products | where: 'vendor', "Polina's Potent Potions" %}

Products from Polina's Potent Potions:

{% for product in polina_products -%}
  - {{ product.title }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Charcoal",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Crocodile tears",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Dandelion milk",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Draught of Immortality",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Dried chamomile",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Forest mushroom",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Gift Card",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Glacier ice",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Ground mandrake root",
        "vendor": "Clover's Apothecary"
      },
      {
        "title": "Health potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Invisibility potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Komodo dragon scale",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Love Potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Mana potion",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Potion beats",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Potion bottle",
        "vendor": "Polina's Potent Potions"
      },
      {
        "title": "Viper venom",
        "vendor": "Ted's Apothecary Supply"
      },
      {
        "title": "Whole bloodroot",
        "vendor": "Clover's Apothecary"
      }
    ]
  }
}
```

##### Output

```html
Products from Polina's Potent Potions:

- Blue Mountain Flower
- Crocodile tears
- Draught of Immortality
- Gift Card
- Health potion
- Invisibility potion
- Love Potion
- Mana potion
- Potion beats
- Potion bottle
```

### Filter for boolean properties with a `true` value

You can filter for items that have a `true` value for a boolean property. This requires you to provide only the property name.

##### Code

```liquid
{% assign available_products = collection.products | where: 'available' %}

Available products:

{% for product in available_products -%}
  - {{ product.title }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "available": false,
        "title": "Blue Mountain Flower"
      },
      {
        "available": true,
        "title": "Charcoal"
      },
      {
        "available": false,
        "title": "Crocodile tears"
      },
      {
        "available": false,
        "title": "Dandelion milk"
      },
      {
        "available": true,
        "title": "Draught of Immortality"
      },
      {
        "available": true,
        "title": "Dried chamomile"
      },
      {
        "available": false,
        "title": "Forest mushroom"
      },
      {
        "available": true,
        "title": "Gift Card"
      },
      {
        "available": false,
        "title": "Glacier ice"
      },
      {
        "available": true,
        "title": "Ground mandrake root"
      },
      {
        "available": true,
        "title": "Health potion"
      },
      {
        "available": true,
        "title": "Invisibility potion"
      },
      {
        "available": false,
        "title": "Komodo dragon scale"
      },
      {
        "available": false,
        "title": "Love Potion"
      },
      {
        "available": true,
        "title": "Mana potion"
      },
      {
        "available": true,
        "title": "Potion beats"
      },
      {
        "available": false,
        "title": "Potion bottle"
      },
      {
        "available": true,
        "title": "Viper venom"
      },
      {
        "available": true,
        "title": "Whole bloodroot"
      }
    ]
  }
}
```

##### Output

```html
Available products:

- Charcoal
- Draught of Immortality
- Dried chamomile
- Gift Card
- Ground mandrake root
- Health potion
- Invisibility potion
- Mana potion
- Potion beats
- Viper venom
- Whole bloodroot
```

</page>

<page>
---
title: 'Liquid filters: within'
description: Generates a product URL within the context of the provided collection.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/filters/within'
  md: 'https://shopify.dev/docs/api/liquid/filters/within.md'
---

# within

```oobleck
string | within: collection
```

returns [string](https://shopify.dev/docs/api/liquid/basics#string)

Generates a product URL within the context of the provided collection.

When the collection context is included, you can access the associated [`collection` object](https://shopify.dev/docs/api/liquid/objects/collection) in the [product template](https://shopify.dev/themes/architecture/templates/product).

***

**Caution:** Because a standard product page and a product page in the context of a collection have the same content on separate URLs, you should consider the SEO implications of using the \<code>within\</code> filter.

***

##### Code

```liquid
{%- assign collection_product = collection.products.first -%}

{{ collection_product.url | within: collection }}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "url": "/products/draught-of-immortality"
      },
      {
        "url": "/products/glacier-ice"
      },
      {
        "url": "/products/health-potion"
      },
      {
        "url": "/products/invisibility-potion"
      }
    ]
  }
}
```

##### Output

```html
/collections/sale-potions/products/draught-of-immortality
```

</page>

<page>
---
title: 'Liquid objects: additional_checkout_buttons'
description: >-
  Returns `true` if a store has any payment providers with offsite checkouts,
  such as PayPal Express Checkout.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/additional_checkout_buttons'
  md: 'https://shopify.dev/docs/api/liquid/objects/additional_checkout_buttons.md'
---

# additional\_​checkout\_​buttons

Returns `true` if a store has any payment providers with offsite checkouts, such as PayPal Express Checkout.

Use `additional_checkout_buttons` to check whether these payment providers exist, and [`content_for_additional_checkout_buttons`](https://shopify.dev/docs/api/liquid/objects/content_for_additional_checkout_buttons) to show the associated checkout buttons. To learn more about how to use these objects, refer to [Accelerated checkout](https://shopify.dev/themes/pricing-payments/accelerated-checkout).

```liquid
{% if additional_checkout_buttons %}
  {{ content_for_additional_checkout_buttons }}
{% endif %}
```

```liquid
{% if additional_checkout_buttons %}
  {{ content_for_additional_checkout_buttons }}
{% endif %}
```

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: address'
description: 'An address, such as a customer address or order shipping address.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/address'
  md: 'https://shopify.dev/docs/api/liquid/objects/address.md'
---

# address

An address, such as a customer address or order shipping address.

***

**Tip:** Use the \<a href="/docs/api/liquid/filters/format\_address">\<code>\<span class="PreventFireFoxApplyingGapToWBR">format\<wbr/>\_address\</span>\</code> filter\</a> to output an address according to its locale.

***

## Properties

* * **address1**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The first line of the address.

  * **address2**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The second line of the address.

    If no second line is specified, then `nil` is returned.

  * **city**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The city of the address.

  * **company**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The company of the address.

    If no company is specified, then `nil` is returned.

  * **country**

    [country](https://shopify.dev/docs/api/liquid/objects/country)

  * The country of the address.

  * **country\_​code**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The country of the address in [ISO 3166-1 (alpha 2) format](https://www.iso.org/glossary-for-iso-3166.html).

  * **first\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The first name of the address.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the address.

  * **last\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The last name of the address.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A combination of the first and last names of the address.

  * **phone**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The phone number of the address.

    If no phone number is specified, then `nil` is returned.

  * **province**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The province of the address.

  * **province\_​code**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The province of the address in [ISO 3166-2 (alpha 2) format](https://www.iso.org/glossary-for-iso-3166.html).

    **Note:** The value doesn\&#39;t include the preceding \<a href="https://www\.iso.org/glossary-for-iso-3166.html">ISO 3166-1\</a> country code.

  * **street**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A combination of the first and second lines of the address.

  * **summary**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A summary of the address, including the following properties:

    * First and last name
    * First and second lines
    * City
    * Province
    * Country

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL for the address.

    **Note:** This only applies to customer addresses.

  * **zip**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The zip or postal code of the address.

##### Example

```json
{
  "address1": "150 Elgin Street",
  "address2": "8th floor",
  "city": "Ottawa",
  "company": "Polina&#39;s Potions, LLC",
  "country": {},
  "country_code": "CA",
  "first_name": null,
  "id": 56174706753,
  "last_name": null,
  "name": "",
  "phone": "416-123-1234",
  "province": "Ontario",
  "province_code": "ON",
  "street": "150 Elgin Street, 8th floor",
  "summary": "150 Elgin Street, 8th floor, Ottawa, Ontario, Canada",
  "url": "/account/addresses/56174706753",
  "zip": "K2P 1L4"
}
```

</page>

<page>
---
title: 'Liquid objects: all_country_option_tags'
description: Creates an `<option>` tag for each country.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/all_country_option_tags'
  md: 'https://shopify.dev/docs/api/liquid/objects/all_country_option_tags.md'
---

# all\_​country\_​option\_​tags

Creates an `<option>` tag for each country.

An attribute called `data-provinces` is set for each `<option>`, and contains a JSON-encoded array of the country or region's subregions. If a country doesn't have any subregions, then an empty array is set for its `data-provinces` attribute.

***

**Tip:** To return only the countries and regions included in the store\&#39;s shipping zones, use the \<a href="/docs/api/liquid/objects/country\_option\_tags">\<code>\<span class="PreventFireFoxApplyingGapToWBR">country\<wbr/>\_option\<wbr/>\_tags\</span>\</code> object\</a>.

***

### Directly accessible in

* Global

You can wrap the `all_country_option_tags` object in `<select>` tags to build a country option selector.

```liquid
<select name="country">
  {{ all_country_option_tags }}
</select>
```

```liquid
```

</page>

<page>
---
title: 'Liquid objects: all_products'
description: All of the products on a store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/all_products'
  md: 'https://shopify.dev/docs/api/liquid/objects/all_products.md'
---

# all\_​products

All of the products on a store.

***

**Note:** The \<code>\<span class="PreventFireFoxApplyingGapToWBR">all\<wbr/>\_products\</span>\</code> object has a limit of 20 unique handles per page. If you want more than 20 products, then consider using a collection instead.

***

### Directly accessible in

* Global

You can use `all_products` to access a product by its [handle](https://shopify.dev/docs/api/liquid/basics#handles). This returns the [`product`](https://shopify.dev/docs/api/liquid/objects/product) object for the specified product. If the product isn't found, then `empty` is returned.

##### Code

```liquid
{{ all_products['love-potion'].title }}
```

##### Data

```json
{
  "all_products": {
    "love-potion": {
      "title": "Love Potion"
    }
  }
}
```

##### Output

```html
Love Potion
```

</page>

<page>
---
title: 'Liquid objects: app'
description: >-
  An app. This object is usually used to access app-specific information for use
  with [theme app extensions](/apps/online-store/theme-app-extensions).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/app'
  md: 'https://shopify.dev/docs/api/liquid/objects/app.md'
---

# app

An app. This object is usually used to access app-specific information for use with [theme app extensions](https://shopify.dev/apps/online-store/theme-app-extensions).

## Properties

* * **metafields**

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) that are [owned by the app](https://shopify.dev/apps/metafields/app-owned).

</page>

<page>
---
title: 'Liquid objects: article'
description: >-
  An article, or [blog
  post](https://help.shopify.com/manual/online-store/blogs/writing-blogs), in a
  blog.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/article'
  md: 'https://shopify.dev/docs/api/liquid/objects/article.md'
---

# article

An article, or [blog post](https://help.shopify.com/manual/online-store/blogs/writing-blogs), in a blog.

## Properties

* * **author**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The full name of the author of the article.

  * **comment\_​post\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL where POST requests are sent when creating new comments.

  * **comments**

    array of [comment](https://shopify.dev/docs/api/liquid/objects/comment)

  * The published comments for the article.

    Returns an empty array if comments are disabled.

    **Tip:** Use the \<a href="/docs/api/liquid/tags/paginate">paginate\</a> tag to choose how many comments to show at once, up to a limit of 50.

  * **comments\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of published comments for the article.

  * **comments\_​enabled?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if comments are enabled. Returns `false` if not.

  * **content**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The content of the article.

  * **created\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the article was created.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **excerpt**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The excerpt of the article.

  * **excerpt\_​or\_​content**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Returns the article [excerpt](https://shopify.dev/docs/api/liquid/objects/article#article-excerpt) if it exists. Returns the article [content](https://shopify.dev/docs/api/liquid/objects/article#article-content) if no excerpt exists.

  * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the article.

  * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the article.

  * **image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The featured image for the article.

  * **metafields**

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the article.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **moderated?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the blog that the article belongs to is set to [moderate comments](https://help.shopify.com/manual/online-store/blogs/managing-comments). Returns `false` if not.

  * **published\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the article was published.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **tags**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The tags applied to the article.

    ExampleShow the total tag count

    When looping through `article.tags`, you can print how many times a tag is used with `tag.total_count`. This number shows visitors how many blog posts have been tagged with a particular tag.

    ##### Code

    ```liquid
    {% for tag in article.tags -%}
      {{ tag }} ({{ tag.total_count }})
    {%- endfor %}
    ```

    ##### Data

    ```json
    {
      "article": {
        "tags": [
          "clear potions",
          "potion troubleshooting",
          "tips"
        ]
      }
    }
    ```

    ##### Output

    ```html
    clear potions (1)potion troubleshooting (2)tips (2)
    ```

  * **template\_​suffix**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the [custom template](https://shopify.dev/themes/architecture/templates#alternate-templates) assigned to the article.

    The name doesn't include the `article.` prefix, or the file extension (`.json` or `.liquid`).

    If a custom template isn't assigned to the article, then `nil` is returned.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the article.

  * **updated\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the article was updated.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the article.

  * **user**

    [user](https://shopify.dev/docs/api/liquid/objects/user)

  * The user associated with the author of the article.

##### Example

```json
{
  "author": "Polina Waters",
  "comment_post_url": "/blogs/potion-notions/how-to-tell-if-you-have-run-out-of-invisibility-potion/comments",
  "comments": [],
  "comments_count": 1,
  "comments_enabled?": true,
  "content": "<p>We've all had this problem before: we peek into the potions vault to determine which potions we are running low on, and the invisibility potion bottle looks completely empty.</p>\n<p>...</p>\n<p> </p>",
  "created_at": "2022-04-14 16:56:02 -0400",
  "excerpt": "And where to buy <strong>more</strong>!",
  "excerpt_or_content": "And where to buy <strong>more</strong>!",
  "handle": "potion-notions/how-to-tell-if-you-have-run-out-of-invisibility-potion",
  "id": 556510085185,
  "image": {},
  "metafields": {},
  "moderated?": true,
  "published_at": "2022-04-14 16:56:02 -0400",
  "tags": [],
  "template_suffix": "",
  "title": "How to tell if you're out of invisibility potion",
  "updated_at": "2022-06-04 19:27:33 -0400",
  "url": {},
  "user": {}
}
```

## Templates using article

[Theme architecture](https://shopify.dev/themes/architecture/templates/article)

[article template](https://shopify.dev/themes/architecture/templates/article)

</page>

<page>
---
title: 'Liquid objects: articles'
description: All of the articles across the blogs in the store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/articles'
  md: 'https://shopify.dev/docs/api/liquid/objects/articles.md'
---

# articles

All of the articles across the blogs in the store.

### Directly accessible in

* Global

You can use `articles` to access an article by its [handle](https://shopify.dev/docs/api/liquid/basics#handles).

##### Code

```liquid
{% assign article = articles['potion-notions/new-potions-for-spring'] %}
{{ article.title | link_to: article.url }}
```

##### Output

```html
<a href="/blogs/potion-notions/new-potions-for-spring" title="">New potions for spring</a>
```

</page>

<page>
---
title: 'Liquid objects: block'
description: >-
  The content and settings of a [section
  block](/themes/architecture/sections/section-schema#blocks).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/block'
  md: 'https://shopify.dev/docs/api/liquid/objects/block.md'
---

# block

The content and settings of a [section block](https://shopify.dev/themes/architecture/sections/section-schema#blocks).

Sections and blocks are reusable modules of content that make up [templates](https://shopify.dev/themes/architecture/templates).

You can include a maxiumum of 50 blocks in a section. To learn more about using blocks, refer to the [Building with sections and blocks](https://shopify.dev/docs/themes/best-practices/templates-sections-blocks).

## Properties

* * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the block.

    The ID is dynamically generated by Shopify and is subject to change. You should avoid relying on a literal value of this ID.

  * **settings**

  * The [settings](https://shopify.dev/themes/architecture/sections/section-schema#blocks) of the block.

    To learn about how to access settings, refer to [Access settings](https://shopify.dev/themes/architecture/settings#access-settings). To learn which input settings can be applied to the `type` property within settings, refer to [Input settings](https://shopify.dev/themes/architecture/settings/input-settings).

  * **shopify\_​attributes**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The data attributes for the block for use in the theme editor.

    The theme editor's [JavaScript API](https://shopify.dev/themes/best-practices/editor/integrate-sections-and-blocks#section-and-block-javascript-events) uses the data attributes to identify blocks and listen for events. No value for `block.shopify_attributes` is returned outside the theme editor.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The type of the block.

    The type is a free-form string that's defined in the [block's schema](https://shopify.dev/themes/architecture/sections/section-schema#blocks). You can use the type as an identifier. For example, you might display different markup based on the block type.

##### Example

```json
{
  "id": "column1",
  "settings": "array",
  "shopify_attributes": "data-shopify-editor-block=\"{\"id\":\"column1\",\"type\":\"column\"}\"",
  "type": "column"
}
```

</page>

<page>
---
title: 'Liquid objects: blog'
description: >-
  Information about a specific
  [blog](https://help.shopify.com/manual/online-store/blogs/adding-a-blog) in
  the store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/blog'
  md: 'https://shopify.dev/docs/api/liquid/objects/blog.md'
---

# blog

Information about a specific [blog](https://help.shopify.com/manual/online-store/blogs/adding-a-blog) in the store.

## Properties

* * **all\_​tags**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * All of the tags on the articles in the blog.

    This includes tags of articles that aren't in the current pagination view.

  * **articles**

    array of [article](https://shopify.dev/docs/api/liquid/objects/article)

  * The articles in the blog.

    **Tip:** Use the \<a href="/docs/api/liquid/tags/paginate">paginate\</a> tag to choose how many articles to show per page, up to a limit of 50.

  * **articles\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of articles in the blog. This total doesn't include hidden articles.

  * **comments\_​enabled?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if comments are enabled for the blog. Returns `false` if not.

  * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the blog.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the blog.

  * **metafields**

    array of [metafield](https://shopify.dev/docs/api/liquid/objects/metafield)

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the blog.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **moderated?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the blog is set to [moderate comments](https://help.shopify.com/manual/online-store/blogs/managing-comments). Returns `false` if not.

  * **next\_​article**

    [article](https://shopify.dev/docs/api/liquid/objects/article)

  * The next (older) article in the blog.

    Returns `nil` if there is no next article.

    This property can be used on the [article page](https://shopify.dev/themes/architecture/templates/article) to output `next` links.

  * **previous\_​article**

    [article](https://shopify.dev/docs/api/liquid/objects/article)

  * The previous (newer) article in the blog.

    Returns `nil` if there is no previous article.

    This property can be used on the [article page](https://shopify.dev/themes/architecture/templates/article) to output `previous` links.

  * **tags**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A list of all of the tags on all of the articles in the blog.

    Unlike [`blog.all_tags`](https://shopify.dev/docs/api/liquid/objects/blog#blog-all_tags), this property only returns tags of articles that are in the filtered view.

  * **template\_​suffix**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the [custom template](https://shopify.dev/themes/architecture/templates#alternate-templates) assigned to the blog.

    The name doesn't include the `blog.` prefix, or the file extension (`.json` or `.liquid`).

    If a custom template isn't assigned to the blog, then `nil` is returned.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the blog.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the blog.

##### Example

```json
{
  "all_tags": [],
  "articles": [],
  "articles_count": 3,
  "comments_enabled?": true,
  "handle": "potion-notions",
  "id": 78580613185,
  "metafields": {},
  "moderated?": true,
  "next_article": {},
  "previous_article": {},
  "tags": [],
  "template_suffix": "",
  "title": "Potion Notions",
  "url": "/blogs/potion-notions"
}
```

## Templates using blog

[Theme architecture](https://shopify.dev/themes/architecture/templates/blog)

[blog template](https://shopify.dev/themes/architecture/templates/blog)

[Theme architecture](https://shopify.dev/themes/architecture/templates/article)

[article template](https://shopify.dev/themes/architecture/templates/article)

</page>

<page>
---
title: 'Liquid objects: blogs'
description: All of the blogs in the store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/blogs'
  md: 'https://shopify.dev/docs/api/liquid/objects/blogs.md'
---

# blogs

All of the blogs in the store.

### Directly accessible in

* Global

You can use `blogs` to access a blog by its [handle](https://shopify.dev/docs/api/liquid/basics#handles).

##### Code

```liquid
{% for article in blogs.potion-notions.articles %}
  {{- article.title | link_to: article.url }}
{% endfor %}
```

##### Output

```html
<a href="/blogs/potion-notions/homebrew-start-making-potions-at-home" title="">Homebrew: start making potions at home</a>

<a href="/blogs/potion-notions/new-potions-for-spring" title="">New potions for spring</a>

<a href="/blogs/potion-notions/how-to-tell-if-you-have-run-out-of-invisibility-potion" title="">How to tell if you're out of invisibility potion</a>
```

</page>

<page>
---
title: 'Liquid objects: brand'
description: >-
  The [brand
  assets](https://help.shopify.com/manual/promoting-marketing/managing-brand-assets)
  for the store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/brand'
  md: 'https://shopify.dev/docs/api/liquid/objects/brand.md'
---

# brand

The [brand assets](https://help.shopify.com/manual/promoting-marketing/managing-brand-assets) for the store.

## Properties

* * **colors**

  * The brand's colors.

    To learn about how to access brand colors, refer to [`brand_color`](https://shopify.dev/docs/api/liquid/objects/brand_color).

  * **cover\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The square logo for the brand, resized to 32x32 px.

  * **favicon\_​url**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The square logo for the brand, resized to 32x32 px.

  * **logo**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The default logo for the brand.

  * **metafields**

  * The social links for the brand.

    Social links are stored in [metafields](https://shopify.dev/docs/api/liquid/objects/metafield), and can be accessed using the syntax `shop.brand.metafields.social_links.<platform>.value`.

    | Platforms |
    | - |
    | `facebook` |
    | `pinterest` |
    | `instagram` |
    | `tiktok` |
    | `tumblr` |
    | `snapchat` |
    | `vimeo` |

    ExampleAccess social links

    ##### Code

    ```liquid
    {{ shop.brand.metafields.social_links.twitter.value }}
    {{ shop.brand.metafields.social_links.youtube.value }}
    ```

    ##### Data

    ```json
    {
      "shop": {
        "brand": {
          "metafields": {}
        }
      }
    }
    ```

    ##### Output

    ```html
    https://twitter.com/ShopifyDevs
    https://www.youtube.com/c/shopifydevs
    ```

  * **short\_​description**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A short description of the brand.

  * **slogan**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The slogan for the brand.

  * **square\_​logo**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The square logo for the brand.

##### Example

```json
{
  "colors": {},
  "cover_image": {},
  "favicon_url": {},
  "logo": {},
  "metafields": {},
  "short_description": "Canada's foremost retailer for potions and potion accessories. Try one of our award-winning artisanal potions, or find the supplies to make your own!",
  "slogan": "Save the toil and trouble!",
  "square_logo": {}
}
```

</page>

<page>
---
title: 'Liquid objects: brand_color'
description: >-
  The colors defined as part of a store's [brand
  assets](https://help.shopify.com/manual/promoting-marketing/managing-brand-assets).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/brand_color'
  md: 'https://shopify.dev/docs/api/liquid/objects/brand_color.md'
---

# brand\_​color

The colors defined as part of a store's [brand assets](https://help.shopify.com/manual/promoting-marketing/managing-brand-assets).

### Returned by

* [brand.colors](https://shopify.dev/docs/api/liquid/objects/brand#brand-colors)

To access a brand color, specify the following:

* The brand color group: either `primary` or `secondary`
* The color role: Whether the color is a `background` or `foreground` (contrasting) color
* The 0-based index of the color within the group and role

##### Code

```liquid
{{ shop.brand.colors.primary[0].background }}
{{ shop.brand.colors.primary[0].foreground }}
{{ shop.brand.colors.secondary[0].background }}
{{ shop.brand.colors.secondary[1].background }}
{{ shop.brand.colors.secondary[0].foreground }}
```

##### Data

```json
{
  "shop": {
    "brand": {
      "colors": "BrandDrop::BrandColorsDrop"
    }
  }
}
```

##### Output

```html
#0b101f
#DDE2F1
#101B2E
#95A7D5
#A3DFFD
```

</page>

<page>
---
title: 'Liquid objects: canonical_url'
description: The canonical URL for the current page.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/canonical_url'
  md: 'https://shopify.dev/docs/api/liquid/objects/canonical_url.md'
---

# canonical\_​url

The canonical URL for the current page.

To learn about canonical URLs, refer to [Google's documentation](https://support.google.com/webmasters/answer/139066?hl=en).

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: cart'
description: A customer’s cart.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/cart'
  md: 'https://shopify.dev/docs/api/liquid/objects/cart.md'
---

# cart

A customer’s cart.

## Properties

* * **attributes**

  * Additional attributes entered by the customer with the cart.

    To learn more about capturing cart attributes, refer to the [`cart` template](https://shopify.dev/themes/architecture/templates/cart#support-cart-notes-and-attributes).

    ExampleCapture cart attributes

    To capture a cart attribute, include an HTML input with an attribute of `name="attributes[attribute-name]"` within the cart `<form>`.

    ```liquid
    <label>What do you want engraved on your cauldron?</label>
    <input type="text" name="attributes[cauldron-engraving]" value="{{ cart.attributes.cauldron-engraving }}" />
    ```

    ```liquid
    ```

    ***

    **Tip:** You can add a double underscore \<code>\_\_\</code> prefix to an attribute name to make it private. Private attributes behave like other cart attributes, except that they can\&#39;t be read from Liquid or the Ajax API. You can use them for data that doesn\&#39;t affect the page rendering, which allows for more effective page caching.

    ***

  * **cart\_​level\_​discount\_​applications**

    array of [discount\_application](https://shopify.dev/docs/api/liquid/objects/discount_application)

  * The cart-specific discount applications for the cart.

    ExampleDisplay cart-level discount applications

    ##### Code

    ```liquid
    {% for discount_application in cart.cart_level_discount_applications %}
      Discount name: {{ discount_application.title }}
      Savings: -{{ discount_application.total_allocated_amount | money }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "cart": {
        "cart_level_discount_applications": [
          {
            "title": "Ingredient Sale",
            "total_allocated_amount": "42.24"
          }
        ]
      }
    }
    ```

    ##### Output

    ```html
    Discount name: Ingredient Sale
      Savings: -$42.24
    ```

  * **checkout\_​charge\_​amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The amount that the customer will be charged at checkout in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **currency**

  * The currency of the cart.

    If the store uses multi-currency, then this is the same as the customer's local (presentment) currency. Otherwise, it's the same as the store currency.

    **Tip:** You can output the store\&#39;s available currencies using \<a href="/docs/api/liquid/objects/shop#shop-enabled\_currencies">\<code>\<span class="PreventFireFoxApplyingGapToWBR">shop.enabled\<wbr/>\_currencies\</span>\</code>\</a>.

  * **discount\_​applications**

    array of [discount\_application](https://shopify.dev/docs/api/liquid/objects/discount_application)

  * The discount applications for the cart.

    ExampleDisplay discount applications

    ##### Code

    ```liquid
    {% for discount_application in cart.discount_applications %}
      Discount name: {{ discount_application.title }}
      Savings: -{{ discount_application.total_allocated_amount | money }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "cart": {
        "discount_applications": [
          {
            "title": "Bloodroot discount!",
            "total_allocated_amount": "2.50"
          },
          {
            "title": "Ingredient Sale",
            "total_allocated_amount": "42.24"
          }
        ]
      }
    }
    ```

    ##### Output

    ```html
    Discount name: Bloodroot discount!
      Savings: -$2.50

      Discount name: Ingredient Sale
      Savings: -$42.24
    ```

  * **duties\_​included**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if duties are included in the prices of products in the cart. Returns `false` if not.

  * **empty?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if there are no items in the cart. Return's `false` if there are.

  * **item\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of items in the cart.

  * **items**

    array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

  * The line items in the cart.

  * **items\_​subtotal\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total price of all of the items in the cart in the currency's subunit, after any line item discounts. This doesn't include taxes (unless taxes are included in the prices), cart discounts, or shipping costs.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **note**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Additional information captured with the cart.

    To learn more about capturing cart notes, refer to the [`cart` template](https://shopify.dev/themes/architecture/templates/cart#support-cart-notes-and-attributes).

    ExampleCapture cart notes

    To capture a cart note, include an HTML input such as a `<textarea>` with an attribute of `name="note"` within the cart `<form>`.

    ```liquid
    <label>Gift note:</label>
    <textarea name="note"></textarea>
    ```

    ```liquid
    ```

    ***

    **Note:** There can only be one instance of \<code>{{ cart.note }}\</code> on the cart page. If there are multiple instances, then the one that comes latest in the Document Object Model (DOM) will be submitted with the form.

    ***

  * **original\_​total\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total price of all of the items in the cart in the currency's subunit, before discounts have been applied.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **requires\_​shipping**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if any of the products in the cart require shipping. Returns `false` if not.

  * **taxes\_​included**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if taxes are included in the prices of products in the cart. Returns `false` if not.

    This can be set in a store’s [tax settings](https://www.shopify.com/admin/settings/taxes).

    If the store includes or exclude tax [based on the customer’s country](https://help.shopify.com/manual/taxes/location#include-or-exclude-tax-based-on-your-customers-country), then the value reflects the tax requirements of the customer’s country.

  * **total\_​discount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total amount of all discounts (the amount saved) for the cart in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **total\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total price of all of the items in the cart in the currency's subunit, after discounts have been applied.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **total\_​weight**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total weight of all of the items in the cart in grams.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/weight\_with\_unit">\<code>\<span class="PreventFireFoxApplyingGapToWBR">weight\<wbr/>\_with\<wbr/>\_unit\</span>\</code>\</a> filter to format the weight in \<a href="https://www\.shopify.com/admin/settings/general">the store\&#39;s format\</a>, or override the default unit.

## Deprecated Properties

* * **discounts**

    array of [discount](https://shopify.dev/docs/api/liquid/objects/discount)

    Deprecated

  * The discounts applied to the cart.

    **Deprecated:**

    Deprecated because not all discount types and details are available.

    The `cart.discounts` property has been replaced by [`cart.discount_applications`](https://shopify.dev/docs/api/liquid/objects/cart#cart-discount_applications).

##### Example

```json
{
  "attributes": {},
  "cart_level_discount_applications": [],
  "checkout_charge_amount": "380.25",
  "currency": {},
  "discount_applications": [],
  "discounts": [],
  "duties_included": false,
  "empty?": false,
  "item_count": 2,
  "items": [],
  "items_subtotal_price": "422.49",
  "note": "Hello this is a note",
  "original_total_price": "424.99",
  "requires_shipping": true,
  "taxes_included": false,
  "total_discount": "44.74",
  "total_price": "380.25",
  "total_weight": 0
}
```

## Templates using cart

[Theme architecture](https://shopify.dev/themes/architecture/templates/cart)

[cart template](https://shopify.dev/themes/architecture/templates/cart)

</page>

<page>
---
title: 'Liquid objects: checkout'
description: A customer's checkout.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/checkout'
  md: 'https://shopify.dev/docs/api/liquid/objects/checkout.md'
---

# checkout

A customer's checkout.

***

**Deprecated:** \</p> \<p>The \<code>checkout\</code> object will be deprecated for the Information, Shipping, and Payment pages on August 13, 2024. Merchants who have customized these pages using \<code>checkout.liquid\</code> need to \<a href="https://help.shopify.com/manual/online-store/themes/theme-structure/extend/checkout-migration#migrate-to-checkout-extensibility">upgrade to Checkout Extensibility\</a> before August 13, 2024.\</p> \<p>Learn \<a href="/apps/checkout">how to build checkout extensions\</a> that extend the functionality of Shopify checkout.

***

You can access the `checkout` object on the [**Order status** page](https://help.shopify.com/manual/orders/status-tracking/customize-order-status).

Shopify Plus merchants can access the `checkout` object in the [`checkout.liquid` layout](https://shopify.dev/themes/architecture/layouts/checkout-liquid).

## Properties

* * **applied\_​gift\_​cards**

    array of [gift\_card](https://shopify.dev/docs/api/liquid/objects/gift_card)

  * The gift cards applied to the checkout.

  * **attributes**

  * Additional attributes entered by the customer with the [cart](https://shopify.dev/docs/api/liquid/objects/cart#cart-attributes).

    Shopify Plus merchants that have access to `checkout.liquid` can [capture attributes at checkout](https://shopify.dev/themes/architecture/layouts/checkout-liquid#capture-checkout-attributes).

  * **billing\_​address**

    [address](https://shopify.dev/docs/api/liquid/objects/address)

  * The billing address entered at checkout.

  * **buyer\_​accepts\_​marketing**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the customer checks the email marketing subscription checkbox. Returns `false` if not.

  * **cart\_​level\_​discount\_​applications**

    array of [discount\_application](https://shopify.dev/docs/api/liquid/objects/discount_application)

  * The cart-specific discount applications for the checkout.

  * **currency**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [ISO code](https://www.iso.org/iso-4217-currency-codes.html) of the currency of the checkout.

  * **customer**

    [customer](https://shopify.dev/docs/api/liquid/objects/customer)

  * The customer associated with the checkout.

    **Note:** The \<a href="/docs/api/liquid/objects/customer">\<code>customer\</code> object\</a> is directly accessible globally when a customer is logged in to their account.

  * **discount\_​applications**

    array of [discount\_application](https://shopify.dev/docs/api/liquid/objects/discount_application)

  * The discount applications for the checkout.

  * **discounts\_​amount**

    array of [discount\_application](https://shopify.dev/docs/api/liquid/objects/discount_application)

  * The total amount of the discounts applied to the checkout in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **discounts\_​savings**

    array of [discount\_application](https://shopify.dev/docs/api/liquid/objects/discount_application)

  * The total amount of the discounts applied to the checkout in the currency's subunit, as a negative value.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **email**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The email associated with the checkout.

  * **gift\_​cards\_​amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The amount of the checkout price paid in gift cards.

    The value is output in the customer's local (presentment) currency.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the checkout.

  * **item\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of items in the checkout.

  * **line\_​items**

    array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

  * The line items of the checkout.

  * **line\_​items\_​subtotal\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The sum of the prices of all of the line items of the checkout in the currency's subunit, after any line item discounts. have been applied.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **name**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The name of the checkout.

    This value is the same as [`checkout.id`](https://shopify.dev/docs/api/liquid/objects/checkout#checkout-id) with a `#` prepended to it.

  * **note**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Additional information entered by the customer with the [cart](https://shopify.dev/docs/api/liquid/objects/cart#cart-note).

  * **order**

    [order](https://shopify.dev/docs/api/liquid/objects/order)

  * The order created by the checkout.

    Depending on the payment provider, the order might not have been created when the [**Thank you** page](https://help.shopify.com/en/manual/orders/status-tracking) is first viewed. In this case, `nil` is returned.

    **Note:** The \<code>order\</code> object isn\&#39;t available on the \<strong>Thank you\</strong> page.

  * **order\_​id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the order created by the checkout.

    The value is the same as [`order.id`](https://shopify.dev/docs/api/liquid/objects/order#order-id).

    Depending on the payment provider, the order might not have been created when the [**Order status** page](https://help.shopify.com/en/manual/orders/status-tracking) is first viewed. In this case, `nil` is returned.

  * **order\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the order created by the checkout.

    The value is the same as [`order.name`](https://shopify.dev/docs/api/liquid/objects/order#order-name).

    Depending on the payment provider, the order might not have been created when the [**Order status** page](https://help.shopify.com/en/manual/orders/status-tracking) is first viewed. In this case, `nil` is returned.

  * **order\_​number**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * An integer representation of the name of the order created by the checkout.

    The value is the same as [`order.order_number`](https://shopify.dev/docs/api/liquid/objects/order#order-order_number).

    Depending on the payment provider, the order might not have been created when the [**Order status** page](https://help.shopify.com/en/manual/orders/status-tracking) is first viewed. In this case, `nil` is returned.

  * **requires\_​shipping**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if any of the line items of the checkout require shipping. Returns `false` if not.

  * **shipping\_​address**

    [address](https://shopify.dev/docs/api/liquid/objects/address)

  * The shipping address of the checkout.

  * **shipping\_​method**

    [shipping\_method](https://shopify.dev/docs/api/liquid/objects/shipping_method)

  * The shipping method of the checkout.

  * **shipping\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The shipping price of the checkout in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **tax\_​lines**

    array of [tax\_line](https://shopify.dev/docs/api/liquid/objects/tax_line)

  * The tax lines for the checkout.

  * **tax\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total tax amount of the checkout in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **total\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total price of the checkout in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **transactions**

    array of [transaction](https://shopify.dev/docs/api/liquid/objects/transaction)

  * The transactions of the checkout.

## Deprecated Properties

* * **cancelled**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

    Deprecated

  * Returns `true` if the checkout has been cancelled. Returns `false` if not.

    **Deprecated:**

    Deprecated because `false` is always returned.

  * **discount**

    [discount](https://shopify.dev/docs/api/liquid/objects/discount)

    Deprecated

  * A discount applied to the checkout without being saved.

    **Deprecated:**

    Deprecated because an unsaved discount doesn't exist on the [**Order status** page](https://help.shopify.com/manual/orders/status-tracking).

  * **discounts**

    array of [discount](https://shopify.dev/docs/api/liquid/objects/discount)

    Deprecated

  * The discounts applied to the checkout.

    **Deprecated:**

    Deprecated because not all discount types and details are captured.

    The `checkout.discounts` property has been replaced by [`checkout.discount_applications`](https://shopify.dev/docs/api/liquid/objects/checkout#checkout-discount_applications).

  * **financial\_​status**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

    Deprecated

  * The financial status of the checkout.

    **Deprecated:**

    Deprecated because `nil` is always returned.

  * **fulfilled\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

    Deprecated

  * A timestamp for the fulfullment of the checkout.

    **Deprecated:**

    Deprecated because `nil` is always returned.

  * **fulfilled\_​line\_​items**

    array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

    Deprecated

  * The fulfilled line items from the checkout.

    **Deprecated:**

    Deprecated because the array is always empty.

  * **fulfillment\_​status**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

    Deprecated

  * The fulfillment status of the checkout.

    **Deprecated:**

    Deprecated because `unfulfilled` is always returned.

  * **unavailable\_​line\_​items**

    array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

    Deprecated

  * The unavailable line items of the checkout.

    **Deprecated:**

    Deprecated because the array is always empty.

  * **unfulfilled\_​line\_​items**

    array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

    Deprecated

  * The unfulfilled line items of the checkout.

    **Deprecated:**

    Deprecated because the array is always the same as [`checkout.line_items`](https://shopify.dev/docs/api/liquid/objects/checkout#checkout-line_items).

##### Example

```json
{
  "applied_gift_cards": [],
  "attributes": {},
  "billing_address": {},
  "buyer_accepts_marketing": false,
  "cart_level_discount_applications": [],
  "currency": "CAD",
  "customer": {},
  "discount_applications": [],
  "discounts_amount": 4224,
  "discounts_savings": -4224,
  "email": "cornelius.potionmaker@gmail.com",
  "gift_cards_amount": 0,
  "id": 29944051400769,
  "line_items": [],
  "line_items_subtotal_price": 42249,
  "name": "#29944051400769",
  "note": null,
  "order": null,
  "order_id": null,
  "order_name": "#29944051400769",
  "order_number": "#29944051400769",
  "requires_shipping": true,
  "shipping_address": {},
  "shipping_method": {},
  "shipping_price": 0,
  "tax_lines": [],
  "tax_price": 0,
  "total_price": 38025,
  "transactions": []
}
```

## Templates using checkout

[Theme architecture](https://shopify.dev/themes/architecture/layouts/checkout-liquid)

[checkout template](https://shopify.dev/themes/architecture/layouts/checkout-liquid)

</page>

<page>
---
title: 'Liquid objects: closest'
description: >-
  A drop that holds resources of different types that are the closest to the
  current context
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/closest'
  md: 'https://shopify.dev/docs/api/liquid/objects/closest.md'
---

# closest

A drop that holds resources of different types that are the closest to the current context

This drop is used to hold resources of different types that are the closest to the current context. These resources can be of type `product`, `collection`, `article`, `blog`, `page`, or `metaobject`. The resources of different types within the closest drop can be:

* The currently rendered section or theme block resource setting of the same type
* The currently rendered theme block's ancestor resource setting of the same type
* The currently rendered template resource of the same type
* Assigned via {% content\_for %} tag

***

**Tip:** To learn about how closest drop in theme settings can be used, refer to \<a href="/storefronts/themes/architecture/blocks/theme-blocks/dynamic-sources#accessing-the-closest-resource">Dynamic sources\</a>.

***

## Properties

* * **article**

    [article](https://shopify.dev/docs/api/liquid/objects/article)

  * Closest article resource

    The article resource that is the closest to the current context.

  * **blog**

    [blog](https://shopify.dev/docs/api/liquid/objects/blog)

  * Closest blog resource

    The blog resource that is the closest to the current context.

  * **collection**

    [collection](https://shopify.dev/docs/api/liquid/objects/collection)

  * Closest collection resource

    The collection resource that is the closest to the current context.

  * **metaobject**

    [metaobject](https://shopify.dev/docs/api/liquid/objects/metaobject)

  * Closest metaobject resources

    The metaobject resources that are the closest to the current context.

  * **page**

    [page](https://shopify.dev/docs/api/liquid/objects/page)

  * Closest page resource

    The page resource that is the closest to the current context.

  * **product**

    [product](https://shopify.dev/docs/api/liquid/objects/product)

  * Closest product resource

    The product resource that is the closest to the current context.

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: collection'
description: >-
  A [collection](https://help.shopify.com/manual/products/collections) in a
  store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/collection'
  md: 'https://shopify.dev/docs/api/liquid/objects/collection.md'
---

# collection

A [collection](https://help.shopify.com/manual/products/collections) in a store.

## Properties

* * **all\_​products\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of products in a collection.

    This includes products that have been filtered out of the current view.

    **Tip:** To display the number of products in a filtered collection, use \<a href="/docs/api/liquid/objects/collection#collection-products\_count">\<code>\<span class="PreventFireFoxApplyingGapToWBR">collection.products\<wbr/>\_count\</span>\</code>\</a>.

  * **all\_​tags**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * All of the tags applied to the products in the collection.

    This includes tags for products that have been filtered out of the current view. A maximum of 1,000 tags can be returned.

    **Tip:** To display the tags that are currently applied, use \<a href="/docs/api/liquid/objects/collection#collection-tags">\<code>collection.tags\</code>\</a>.

  * **all\_​types**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * All of the product types in a collection.

    ExampleCreate links to product types

    Use the [`link_to_type`](https://shopify.dev/docs/api/liquid/filters/link_to_type) filter to create links to the product types in a collection.

    ##### Code

    ```liquid
    {% for product_type in collection.all_types -%}
      {{- product_type | link_to_type }}
    {%- endfor %}
    ```

    ##### Data

    ```json
    {
      "collection": {
        "all_types": [
          "Animals & Pet Supplies",
          "Baking Flavors & Extracts",
          "Cooking & Baking Ingredients",
          "Dried Flowers",
          "Fruits & Vegetables",
          "Seasonings & Spices",
          "Water"
        ]
      }
    }
    ```

    ##### Output

    ```html
    <a href="/collections/types?q=Animals%20%26%20Pet%20Supplies" title="Animals &amp; Pet Supplies">Animals & Pet Supplies</a>
    <a href="/collections/types?q=Baking%20Flavors%20%26%20Extracts" title="Baking Flavors &amp; Extracts">Baking Flavors & Extracts</a>
    <a href="/collections/types?q=Cooking%20%26%20Baking%20Ingredients" title="Cooking &amp; Baking Ingredients">Cooking & Baking Ingredients</a>
    <a href="/collections/types?q=Dried%20Flowers" title="Dried Flowers">Dried Flowers</a>
    <a href="/collections/types?q=Fruits%20%26%20Vegetables" title="Fruits &amp; Vegetables">Fruits & Vegetables</a>
    <a href="/collections/types?q=Seasonings%20%26%20Spices" title="Seasonings &amp; Spices">Seasonings & Spices</a>
    <a href="/collections/types?q=Water" title="Water">Water</a>
    ```

  * **all\_​vendors**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * All of the product vendors in a collection.

    ExampleCreate links to vendors

    Use the [`link_to_vendor`](https://shopify.dev/docs/api/liquid/filters/link_to_vendor) filter to create links to the vendors in a collection.

    ##### Code

    ```liquid
    {% for product_vendor in collection.all_vendors %}
      {{- product_vendor | link_to_vendor }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "collection": {
        "all_vendors": [
          "Clover's Apothecary",
          "Polina's Potent Potions",
          "Ted's Apothecary Supply"
        ]
      }
    }
    ```

    ##### Output

    ```html
    <a href="/collections/vendors?q=Clover%27s%20Apothecary" title="Clover&#39;s Apothecary">Clover's Apothecary</a>

    <a href="/collections/vendors?q=Polina%27s%20Potent%20Potions" title="Polina&#39;s Potent Potions">Polina's Potent Potions</a>

    <a href="/collections/vendors?q=Ted%27s%20Apothecary%20Supply" title="Ted&#39;s Apothecary Supply">Ted's Apothecary Supply</a>
    ```

  * **current\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The product type on a product type collection page.

    You can query for products of a certain type at the `/collections/types` URL with a query parameter in the format of `?q=[type]`, where `[type]` is your desired product type.

    **Tip:** The query value is case-insensitive, so \<code>shirts\</code> is equivalent to \<code>Shirts\</code> or \<code>\<span class="PreventFireFoxApplyingGapToWBR">S\<wbr/>H\<wbr/>I\<wbr/>R\<wbr/>T\<wbr/>S\</span>\</code>.

  * **current\_​vendor**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The vendor name on a vendor collection page.

    You can query for products from a certain vendor at the `/collections/vendors` URL with a query parameter in the format of `?q=[vendor]`, where `[vendor]` is your desired product vendor.

    **Tip:** The query value is case-insensitive, so \<code>apparelco\</code> is equivalent to \<code>\<span class="PreventFireFoxApplyingGapToWBR">Apparel\<wbr/>Co\</span>\</code> or \<code>\<span class="PreventFireFoxApplyingGapToWBR">A\<wbr/>P\<wbr/>P\<wbr/>A\<wbr/>R\<wbr/>E\<wbr/>L\<wbr/>C\<wbr/>O\</span>\</code>.

  * **default\_​sort\_​by**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The default sort order of the collection.

    This is set on the collection's page in the Shopify admin.

    | Possible values |
    | - |
    | manual |
    | best-selling |
    | title-ascending |
    | price-ascending |
    | price-descending |
    | created-ascending |
    | created-descending |

  * **description**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The description of the collection.

  * **featured\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The featured image for the collection.

    The default is the [collection image](https://shopify.dev/docs/api/liquid/objects/collection#collection-image). If this image isn't available, then Shopify falls back to the featured image of the first product in the collection. If the first product in the collection doesn't have a featured image, then `nil` is returned.

  * **filters**

    array of [filter](https://shopify.dev/docs/api/liquid/objects/filter)

  * The [storefront filters](https://help.shopify.com/manual/online-store/themes/customizing-themes/storefront-filters) that have been set up on the collection.

    Only filters relevant to the current collection are returned. Filters will be empty for collections that contain over 5000 products.

    To learn about supporting filters in your theme, refer to [Support storefront filtering](https://shopify.dev/themes/navigation-search/filtering/storefront-filtering/support-storefront-filtering).

  * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the collection.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the collection.

  * **image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The image for the collection.

    This image is added on the collection's page in the Shopify admin.

  * **metafields**

    array of [metafield](https://shopify.dev/docs/api/liquid/objects/metafield)

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the collection.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **next\_​product**

    [product](https://shopify.dev/docs/api/liquid/objects/product)

  * The next product in the collection. Returns `nil` if there's no next product.

    This property can be used on the [product page](https://shopify.dev/themes/architecture/templates/product) to output `next` links.

  * **previous\_​product**

    [product](https://shopify.dev/docs/api/liquid/objects/product)

  * The previous product in the collection. Returns `nil` if there's no previous product.

    This property can be used on the [product page](https://shopify.dev/themes/architecture/templates/product) to output `previous` links.

  * **products**

    array of [product](https://shopify.dev/docs/api/liquid/objects/product)

  * All of the products in the collection.

    **Tip:** Use the \<a href="/docs/api/liquid/tags/paginate">paginate\</a> tag to choose how many products to show per page, up to a limit of 50.

  * **products\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of products in the current view of the collection.

  * **published\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the collection was published.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **sort\_​by**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The sort order applied to the collection by the `sort_by` URL parameter.

    If there's no `sort_by` URL parameter, then the value is `nil`.

  * **sort\_​options**

    array of [sort\_option](https://shopify.dev/docs/api/liquid/objects/sort_option)

  * The available sorting options for the collection.

    ExampleOutput the sort options

    ##### Code

    ```liquid
    {%- assign sort_by = collection.sort_by | default: collection.default_sort_by -%}

    <select>
    {%- for option in collection.sort_options %}
      <option
        value="{{ option.value }}"
        {%- if option.value == sort_by %}
          selected="selected"
        {%- endif %}
      >
        {{ option.name }}
      </option>
    {% endfor -%}
    </select>
    ```

    ##### Data

    ```json
    {
      "collection": {
        "default_sort_by": "title-ascending",
        "sort_by": "",
        "sort_options": [
          "CollectionDrop::SortOptionDrop",
          "CollectionDrop::SortOptionDrop",
          "CollectionDrop::SortOptionDrop",
          "CollectionDrop::SortOptionDrop",
          "CollectionDrop::SortOptionDrop",
          "CollectionDrop::SortOptionDrop",
          "CollectionDrop::SortOptionDrop",
          "CollectionDrop::SortOptionDrop",
          "CollectionDrop::SortOptionDrop"
        ]
      }
    }
    ```

    ##### Output

    ```html
    <select>
      <option
        value="manual"
      >
        Featured
      </option>

      <option
        value="most-relevant"
      >
        Most relevant
      </option>

      <option
        value="best-selling"
      >
        Best selling
      </option>

      <option
        value="title-ascending"
          selected="selected"
      >
        Alphabetically, A-Z
      </option>

      <option
        value="title-descending"
      >
        Alphabetically, Z-A
      </option>

      <option
        value="price-ascending"
      >
        Price, low to high
      </option>

      <option
        value="price-descending"
      >
        Price, high to low
      </option>

      <option
        value="created-ascending"
      >
        Date, old to new
      </option>

      <option
        value="created-descending"
      >
        Date, new to old
      </option>
    </select>
    ```

  * **tags**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The tags that are currently applied to the collection.

    This doesn't include tags for products that have been filtered out of the current view. Returns `nil` if no tags have been applied, or all products with tags have been filtered out of the current view.

  * **template\_​suffix**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the [custom template](https://shopify.dev/themes/architecture/templates#alternate-templates) assigned to the collection.

    The name doesn't include the `collection.` prefix, or the file extension (`.json` or `.liquid`).

    If a custom template isn't assigned to the collection, then `nil` is returned.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the collection.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the collection.

##### Example

```json
{
  "all_products_count": 10,
  "all_tags": [
    "Burning",
    "dried",
    "extracts",
    "fresh",
    "ingredients",
    "plant",
    "supplies"
  ],
  "all_types": [
    "Animals & Pet Supplies",
    "Baking Flavors & Extracts",
    "Cooking & Baking Ingredients",
    "Dried Flowers",
    "Fruits & Vegetables",
    "Seasonings & Spices",
    "Water"
  ],
  "all_vendors": [
    "Clover's Apothecary",
    "Polina's Potent Potions",
    "Ted's Apothecary Supply"
  ],
  "current_type": null,
  "current_vendor": null,
  "default_sort_by": "created-ascending",
  "description": "Brew your own potions at home using our fresh, ethically-sourced ingredients.",
  "featured_image": {},
  "filters": {},
  "handle": "ingredients",
  "id": 266168401985,
  "image": {},
  "metafields": {},
  "next_product": null,
  "previous_product": null,
  "products": {},
  "products_count": 1,
  "published_at": "2022-04-19 09:52:18 -0400",
  "sort_by": "",
  "sort_options": [],
  "tags": [
    "Burning"
  ],
  "template_suffix": "eight-products-per-page",
  "title": "Ingredients",
  "url": {}
}
```

## Templates using collection

[Theme architecture](https://shopify.dev/themes/architecture/templates/collection)

[collection template](https://shopify.dev/themes/architecture/templates/collection)

</page>

<page>
---
title: 'Liquid objects: collections'
description: 'All of the [collections](/docs/api/liquid/objects/collection) on a store.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/collections'
  md: 'https://shopify.dev/docs/api/liquid/objects/collections.md'
---

# collections

All of the [collections](https://shopify.dev/docs/api/liquid/objects/collection) on a store.

### Directly accessible in

* Global

### Iterate over the collections

You can iterate over `collections` to build a collection list.

##### Code

```liquid
{% for collection in collections %}
  {{- collection.title | link_to: collection.url }}
{% endfor %}
```

##### Output

```html
<a href="/collections/empty" title="">Empty</a>

<a href="/collections/featured-potions" title="">Featured potions</a>

<a href="/collections/freebies" title="">Freebies</a>

<a href="/collections/frontpage" title="">Home page</a>

<a href="/collections/ingredients" title="">Ingredients</a>

<a href="/collections/potions" title="">Potions</a>

<a href="/collections/sale-potions" title="">Sale potions</a>
```

### Access a specific collection

You can use `collections` to access a collection by its [handle](https://shopify.dev/docs/api/liquid/basics#handles).

##### Code

```liquid
{% for product in collections['sale-potions'].products %}
  {{- product.title | link_to: product.url }}
{% endfor %}
```

##### Output

```html
<a href="/products/draught-of-immortality" title="">Draught of Immortality</a>

<a href="/products/glacier-ice" title="">Glacier ice</a>

<a href="/products/health-potion" title="">Health potion</a>

<a href="/products/invisibility-potion" title="">Invisibility potion</a>
```

</page>

<page>
---
title: 'Liquid objects: color'
description: >-
  A color from a [`color`
  setting](/themes/architecture/settings/input-settings#color).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/color'
  md: 'https://shopify.dev/docs/api/liquid/objects/color.md'
---

# color

A color from a [`color` setting](https://shopify.dev/themes/architecture/settings/input-settings#color).

***

**Tip:** Use \<a href="/docs/api/liquid/filters/color-filters">color filters\</a> to modify or extract properties of a \<code>color\</code> object.

***

## Properties

* * **alpha**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The alpha component of the color, which is a decimal number between 0.0 and 1.0.

  * **blue**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The blue component of the color, which is a number between 0 and 255.

  * **chroma**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The chroma component of the color, which is a decimal number between 0.0 and 0.5.

  * **color\_​space**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The color space of the color. Returns 'srgb' or 'oklch'

  * **green**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The green component of the color, which is a number between 0 and 255.

  * **hue**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The hue component of the color, which is a number between 0 and 360.

  * **lightness**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The lightness component of the color, which is a number between 0 and 100.

  * **oklch**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The lightness, chroma, and hue values of the color, represented as a space-separated string.

  * **oklcha**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The lightness, chroma, hue and alpha values of the color, represented as a space-separated string, with a slash before the alpha channel.

  * **red**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The red component of the color, which is a number between 0 and 255.

  * **rgb**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The red, green, and blue values of the color, represented as a space-separated string.

  * **rgba**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The red, green, blue, and alpha values of the color, represented as a space-separated string, with a slash before the alpha channel.

  * **saturation**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The saturation component of the color, which is a number between 0 and 100.

##### Example

```json
{
  "alpha": 1,
  "blue": 180,
  "chroma": 0.16,
  "color_space": "srgb",
  "green": 79,
  "hue": 227,
  "lightness": 45,
  "oklch": "47% 0.16 268",
  "oklcha": "47% 0.16 268 / 1.0",
  "red": 51,
  "rgb": "51 79 180",
  "rgba": "51 79 180 / 1.0",
  "saturation": 56
}
```

### Referencing color settings directly

When a color setting is referenced directly, the hexidecimal color code is returned.

##### Code

```liquid
{{ settings.colors_accent_2 }}
```

##### Data

```json
{
  "settings": {
    "colors_accent_2": "#334fb4"
  }
}
```

##### Output

```html
#334fb4
```

</page>

<page>
---
title: 'Liquid objects: color_scheme'
description: >-
  A color_scheme from a [`color_scheme`
  setting](/themes/architecture/settings/input-settings#color_scheme).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/color_scheme'
  md: 'https://shopify.dev/docs/api/liquid/objects/color_scheme.md'
---

# color\_​scheme

A color\_scheme from a [`color_scheme` setting](https://shopify.dev/themes/architecture/settings/input-settings#color_scheme).

***

**Tip:** To learn about color scheme groups in themes, refer to \<a href="/themes/architecture/settings/input-settings#color\_scheme\_group">\<code>\<span class="PreventFireFoxApplyingGapToWBR">color\<wbr/>\_scheme\<wbr/>\_group\</span>\</code> setting\</a>.

***

## Properties

* * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the color\_scheme

  * **settings**

  * The [settings](https://shopify.dev/docs/themes/architecture/settings/input-settings#color_scheme_group) of the color\_scheme.

##### Example

```json
{
  "id": "background-2",
  "settings": {}
}
```

### Referencing color\_scheme settings directly

When a color\_scheme setting is referenced directly, the color scheme ID is returned.

##### Code

```liquid
{{ settings.card_color_scheme }}
```

##### Data

```json
{
  "settings": {
    "card_color_scheme": {}
  }
}
```

##### Output

```html
background-2
```

</page>

<page>
---
title: 'Liquid objects: color_scheme_group'
description: >-
  A color_scheme_group from a [`color_scheme_group`
  setting](/themes/architecture/settings/input-settings#color_scheme_group).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/color_scheme_group'
  md: 'https://shopify.dev/docs/api/liquid/objects/color_scheme_group.md'
---

# color\_​scheme\_​group

A color\_scheme\_group from a [`color_scheme_group` setting](https://shopify.dev/themes/architecture/settings/input-settings#color_scheme_group).

***

**Tip:** To learn about color schemes in themes, refer to \<a href="/themes/architecture/settings/input-settings#color\_scheme">\<code>\<span class="PreventFireFoxApplyingGapToWBR">color\<wbr/>\_scheme\</span>\</code> setting\</a>.

***

##### Example

```json
{}
```

### Referencing color\_scheme\_group settings directly

##### Code

```liquid
{% for scheme in settings.color_schemes %}
  .color-{{ scheme.id }} {
    --color-background: {{ scheme.settings.background }};
    --color-text: {{ scheme.settings.text }};
  }
{% endfor %}
```

##### Data

```json
{
  "settings": {
    "color_schemes": {}
  }
}
```

##### Output

```html
.color-background-1 {
    --color-background: #FFFFFF;
    --color-text: #121212;
  }

  .color-background-2 {
    --color-background: #F3F3F3;
    --color-text: #121212;
  }

  .color-inverse {
    --color-background: #121212;
    --color-text: #FFFFFF;
  }

  .color-accent-1 {
    --color-background: #121212;
    --color-text: #FFFFFF;
  }

  .color-accent-2 {
    --color-background: #334FB4;
    --color-text: #FFFFFF;
  }
```

</page>

<page>
---
title: 'Liquid objects: comment'
description: An article comment.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/comment'
  md: 'https://shopify.dev/docs/api/liquid/objects/comment.md'
---

# comment

An article comment.

## Properties

* * **author**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The full name of the author of the comment.

  * **content**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The content of the comment.

  * **created\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the comment was created.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **email**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The email of he author of the comment.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the comment.

  * **status**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The status of the comment. Always returns `published`.

    Outside of the Liquid context, the status of a comment can vary based on spam detection and whether blog comments are moderated. However, only comments with a status of `published` are included in the [`article.comments` array](https://shopify.dev/docs/api/liquid/objects/article#article-comments).

    **Tip:** To learn more about blog comments, visit the \<a href="https://help.shopify.com/manual/online-store/blogs/managing-comments">Shopify Help Center\</a>.

  * **updated\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the status of the comment was last updated.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the article that the comment is associated with, with [`comment.id`](https://shopify.dev/docs/api/liquid/objects/comment#comment-id) appended.

##### Example

```json
{
  "author": "Cornelius",
  "content": "Wow, this is going to save me a fortune in invisibility potion!",
  "created_at": "2022-06-05 19:33:57 -0400",
  "email": "cornelius.potionmaker@gmail.com",
  "id": 129089273921,
  "status": "published",
  "updated_at": "2022-06-05 19:33:57 -0400",
  "url": "/blogs/potion-notions/how-to-tell-if-you-have-run-out-of-invisibility-potion#129089273921"
}
```

</page>

<page>
---
title: 'Liquid objects: company'
description: >-
  A company that a [customer](/docs/api/liquid/objects/customer) is purchasing
  for.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/company'
  md: 'https://shopify.dev/docs/api/liquid/objects/company.md'
---

# company

A company that a [customer](https://shopify.dev/docs/api/liquid/objects/customer) is purchasing for.

To learn about B2B in themes, refer to [Support B2B customers in your theme](https://shopify.dev/themes/pricing-payments/b2b).

## Properties

* * **available\_​locations**

    array of [company\_location](https://shopify.dev/docs/api/liquid/objects/company_location)

  * The company locations that the current customer has access to, or can interact with.

  * **available\_​locations\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of company locations associated with the customer's company.

  * **external\_​id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The external ID of the company.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the company.

  * **metafields**

    array of [metafield](https://shopify.dev/docs/api/liquid/objects/metafield)

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the company.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the company.

##### Example

```json
{
  "available_locations": [],
  "available_locations_count": 1,
  "external_id": null,
  "id": 98369,
  "metafields": {},
  "name": "Cornelius&#39; Custom Concoctions"
}
```

</page>

<page>
---
title: 'Liquid objects: company_address'
description: The address of a company location.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/company_address'
  md: 'https://shopify.dev/docs/api/liquid/objects/company_address.md'
---

# company\_​address

The address of a company location.

To learn about B2B in themes, refer to [Support B2B customers in your theme](https://shopify.dev/themes/pricing-payments/b2b).

## Properties

* * **address1**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The first line of the address.

  * **address2**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The second line of the address.

    If no second line is specified, then `nil` is returned.

  * **attention**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The attention line of the address.

  * **city**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The city of the address.

  * **country**

    [country](https://shopify.dev/docs/api/liquid/objects/country)

  * The country of the address.

  * **country\_​code**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The country of the address in [ISO 3166-1 (alpha 2) format](https://www.iso.org/glossary-for-iso-3166.html).

  * **first\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The first name of the address.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the address.

  * **last\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The last name of the address.

  * **province**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The province of the address.

  * **province\_​code**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The province of the address in [ISO 3166-2 (alpha 2) format](https://www.iso.org/glossary-for-iso-3166.html).

    **Note:** The value doesn\&#39;t include the preceding \<a href="https://www\.iso.org/glossary-for-iso-3166.html">ISO 3166-1\</a> country code.

  * **street**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A combination of the first and second lines of the address.

  * **zip**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The zip or postal code of the address.

##### Example

```json
{
  "address1": "99 Cauldron Lane",
  "address2": "Unit 4B",
  "attention": "Cornelius&#39; Custom Concoctions",
  "city": "Edinburgh",
  "country": {},
  "country_code": "GB",
  "first_name": "Cornelius",
  "id": 65,
  "last_name": "Potionmaker",
  "province": null,
  "province_code": null,
  "street": "99 Cauldron Lane, Unit 4B",
  "zip": "EH95 1AF"
}
```

</page>

<page>
---
title: 'Liquid objects: company_location'
description: >-
  A location of the [company](/docs/api/liquid/objects/company) that a
  [customer](/docs/api/liquid/objects/customer) is purchasing for.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/company_location'
  md: 'https://shopify.dev/docs/api/liquid/objects/company_location.md'
---

# company\_​location

A location of the [company](https://shopify.dev/docs/api/liquid/objects/company) that a [customer](https://shopify.dev/docs/api/liquid/objects/customer) is purchasing for.

To learn about B2B in themes, refer to [Support B2B customers in your theme](https://shopify.dev/themes/pricing-payments/b2b).

## Properties

* * **company**

    [company](https://shopify.dev/docs/api/liquid/objects/company)

  * The company that the location is associated with.

  * **current?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the location is currently selected. Returns `false` if not.

  * **external\_​id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The external ID of the location.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the location.

  * **metafields**

    array of [metafield](https://shopify.dev/docs/api/liquid/objects/metafield)

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the company location.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the location.

  * **shipping\_​address**

    [company\_address](https://shopify.dev/docs/api/liquid/objects/company_address)

  * The address of the location.

  * **store\_​credit\_​account**

    [store\_credit\_account](https://shopify.dev/docs/api/liquid/objects/store_credit_account)

  * The store credit account associated with the company location.

    The account shown will be in the currency associated with the company location’s current context. For example, when browsing a storefront for a company location in the US market, the company location's USD store credit account will be returned. If the company location does not have a USD store credit account `nil` will be returned.

  * **tax\_​registration\_​id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The tax ID of the location.

  * **url\_​to\_​set\_​as\_​current**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL to set the location as the current location for the customer.

##### Example

```json
{
  "company": {},
  "current?": false,
  "external_id": null,
  "id": 98369,
  "metafields": {},
  "name": "99 Cauldron Lane",
  "shipping_address": {},
  "store_credit_account": null,
  "tax_registration_id": null,
  "url_to_set_as_current": "https://polinas-potent-potions.myshopify.com/company_location/update?location_id=98369&return_to=/resource"
}
```

</page>

<page>
---
title: 'Liquid objects: content_for_additional_checkout_buttons'
description: >-
  Returns checkout buttons for any active payment providers with offsite
  checkouts.
api_name: liquid
source_url:
  html: >-
    https://shopify.dev/docs/api/liquid/objects/content_for_additional_checkout_buttons
  md: >-
    https://shopify.dev/docs/api/liquid/objects/content_for_additional_checkout_buttons.md
---

# content\_​for\_​additional\_​checkout\_​buttons

Returns checkout buttons for any active payment providers with offsite checkouts.

Use [`additional_checkout_buttons`](https://shopify.dev/docs/api/liquid/objects/additional_checkout_buttons) to check whether these payment providers exist, and `content_for_additional_checkout_buttons` to show the associated checkout buttons. To learn more about how to use these objects, refer to [Accelerated checkout](https://shopify.dev/themes/pricing-payments/accelerated-checkout).

```liquid
{% if additional_checkout_buttons %}
  {{ content_for_additional_checkout_buttons }}
{% endif %}
```

```liquid
{% if additional_checkout_buttons %}
  {{ content_for_additional_checkout_buttons }}
{% endif %}
```

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: content_for_header'
description: Dynamically returns all scripts required by Shopify.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/content_for_header'
  md: 'https://shopify.dev/docs/api/liquid/objects/content_for_header.md'
---

# content\_​for\_​header

Dynamically returns all scripts required by Shopify.

Include the `content_for_header` object in your [layout files](https://shopify.dev/themes/architecture/layouts) between the `<head>` and `</head>` HTML tags.

You shouldn't try to modify or parse the `content_for_header` object because the contents are subject to change, which can change the behaviour of your code.

***

**Note:** The \<code>\<span class="PreventFireFoxApplyingGapToWBR">content\<wbr/>\_for\<wbr/>\_header\</span>\</code> object is required in \<code>theme.liquid\</code>.

***

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: content_for_index'
description: >-
  Dynamically returns the content of [sections](/themes/architecture/sections)
  to be rendered on the home page.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/content_for_index'
  md: 'https://shopify.dev/docs/api/liquid/objects/content_for_index.md'
---

# content\_​for\_​index

Dynamically returns the content of [sections](https://shopify.dev/themes/architecture/sections) to be rendered on the home page.

If you use a [Liquid index template](https://shopify.dev/themes/architecture/templates/index-template) (`templates/index.liquid`), then you must include `{{ content_for_index }}` in the template. This object can't be used in JSON index templates.

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: content_for_layout'
description: >-
  Dynamically returns content based on the current
  [template](/themes/architecture/templates).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/content_for_layout'
  md: 'https://shopify.dev/docs/api/liquid/objects/content_for_layout.md'
---

# content\_​for\_​layout

Dynamically returns content based on the current [template](https://shopify.dev/themes/architecture/templates).

Include the `content_for_layout` object in your [layout files](https://shopify.dev/themes/architecture/layouts) between the `<body>` and `</body>` HTML tags.

***

**Note:** The \<code>\<span class="PreventFireFoxApplyingGapToWBR">content\<wbr/>\_for\<wbr/>\_layout\</span>\</code> object is required in \<code>theme.liquid\</code>.

***

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: country'
description: A country supported by the store's localization options.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/country'
  md: 'https://shopify.dev/docs/api/liquid/objects/country.md'
---

# country

A country supported by the store's localization options.

To learn how to use the `country` object to offer localization options in your theme, refer to [Support multiple currencies and languages](https://shopify.dev/themes/internationalization/multiple-currencies-languages).

## Properties

* * **available\_​languages**

    array of [shop\_locale](https://shopify.dev/docs/api/liquid/objects/shop_locale)

  * The languages that have been added to the market that this country belongs to.

  * **continent**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The continent that the country is in.

    Possible values are `Africa`, `Asia`, `Central America`, `Europe`, `North America`, `Oceania`, and `South America`.

  * **currency**

    [currency](https://shopify.dev/docs/api/liquid/objects/currency)

  * The currency used in the country.

  * **iso\_​code**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ISO code of the country in [ISO 3166-1 (alpha 2) format](https://www.iso.org/glossary-for-iso-3166.html).

  * **market**

    [market](https://shopify.dev/docs/api/liquid/objects/market)

  * The market that includes this country.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the country.

  * **popular?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the country is popular for this shop. Otherwise, returns `false`. This can be useful for sorting countries in a country selector.

  * **unit\_​system**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The unit system of the country.

    | Possible values |
    | - |
    | imperial |
    | metric |

##### Example

```json
{
  "available_languages": [],
  "continent": "North America",
  "currency": {},
  "iso_code": "CA",
  "market": {},
  "name": "Canada",
  "popular?": false,
  "unit_system": "metric"
}
```

### Referencing the `country` object directly

When the country object is referenced directly, `country.name` is returned.

##### Code

```liquid
{% for country in localization.available_countries -%}
  {{ country }}
{%- endfor %}
```

##### Data

```json
{
  "localization": {
    "available_countries": [
      "Afghanistan",
      "Australia",
      "Austria",
      "Belgium",
      "Canada",
      "Czechia",
      "Denmark",
      "Finland",
      "France",
      "Germany",
      "Hong Kong SAR",
      "Ireland",
      "Israel",
      "Italy",
      "Japan",
      "Malaysia",
      "Netherlands",
      "New Zealand",
      "Norway",
      "Poland",
      "Portugal",
      "Singapore",
      "South Korea",
      "Spain",
      "Sweden",
      "Switzerland",
      "United Arab Emirates",
      "United Kingdom",
      "United States"
    ]
  }
}
```

##### Output

```html
Afghanistan
Australia
Austria
Belgium
Canada
Czechia
Denmark
Finland
France
Germany
Hong Kong SAR
Ireland
Israel
Italy
Japan
Malaysia
Netherlands
New Zealand
Norway
Poland
Portugal
Singapore
South Korea
Spain
Sweden
Switzerland
United Arab Emirates
United Kingdom
United States
```

### Rendering a flag image

When the country object is passed to the [`image_url`](https://shopify.dev/docs/api/liquid/filters#image_url) filter, a [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for that country’s flag is returned. All country’s flags are SVGs, normalized to an aspect ratio of 4:3.

##### Code

```liquid
{{ localization.country | image_url: width: 32 | image_tag }}
```

##### Data

```json
{
  "localization": {
    "country": "Canada"
  }
}
```

##### Output

```html
<img src="//cdn.shopify.com/static/images/flags/ca.svg?width=32" alt="Canada" srcset="//cdn.shopify.com/static/images/flags/ca.svg?width=32 32w" width="32" height="24">
```

</page>

<page>
---
title: 'Liquid objects: country_option_tags'
description: >-
  Creates an `<option>` tag for each country and region that's included in a
  shipping zone on the
  [Shipping](https://www.shopify.com/admin/settings/shipping) page of the
  Shopify admin.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/country_option_tags'
  md: 'https://shopify.dev/docs/api/liquid/objects/country_option_tags.md'
---

# country\_​option\_​tags

Creates an `<option>` tag for each country and region that's included in a shipping zone on the [Shipping](https://www.shopify.com/admin/settings/shipping) page of the Shopify admin.

An attribute called `data-provinces` is set for each `<option>`, and contains a JSON-encoded array of the country or region's subregions. If a country doesn't have any subregions, then an empty array is set for its `data-provinces` attribute.

***

**Tip:** To return all countries and regions included in the store\&#39;s shipping zones, use \<a href="/docs/api/liquid/objects/all\_country\_option\_tags">\<code>\<span class="PreventFireFoxApplyingGapToWBR">all\<wbr/>\_country\<wbr/>\_option\<wbr/>\_tags\</span>\</code>\</a>.

***

### Directly accessible in

* Global

You can wrap the `country_option_tags` object in `<select>` tags to build a country option selector.

##### Code

```liquid
<select name="country">
  {{ country_option_tags }}
</select>
```

##### Output

```html
<select name="country">
  <option value="---" data-provinces="[]">---</option>
<option value="Afghanistan" data-provinces="[]">Afghanistan</option>
<option value="Canada" data-provinces="[[&quot;Alberta&quot;,&quot;Alberta&quot;],[&quot;British Columbia&quot;,&quot;British Columbia&quot;],[&quot;Manitoba&quot;,&quot;Manitoba&quot;],[&quot;New Brunswick&quot;,&quot;New Brunswick&quot;],[&quot;Newfoundland and Labrador&quot;,&quot;Newfoundland and Labrador&quot;],[&quot;Northwest Territories&quot;,&quot;Northwest Territories&quot;],[&quot;Nova Scotia&quot;,&quot;Nova Scotia&quot;],[&quot;Nunavut&quot;,&quot;Nunavut&quot;],[&quot;Ontario&quot;,&quot;Ontario&quot;],[&quot;Prince Edward Island&quot;,&quot;Prince Edward Island&quot;],[&quot;Quebec&quot;,&quot;Quebec&quot;],[&quot;Saskatchewan&quot;,&quot;Saskatchewan&quot;],[&quot;Yukon&quot;,&quot;Yukon&quot;]]">Canada</option>
<option value="United States" data-provinces="[[&quot;Alabama&quot;,&quot;Alabama&quot;],[&quot;Alaska&quot;,&quot;Alaska&quot;],[&quot;American Samoa&quot;,&quot;American Samoa&quot;],[&quot;Arizona&quot;,&quot;Arizona&quot;],[&quot;Arkansas&quot;,&quot;Arkansas&quot;],[&quot;Armed Forces Americas&quot;,&quot;Armed Forces Americas&quot;],[&quot;Armed Forces Europe&quot;,&quot;Armed Forces Europe&quot;],[&quot;Armed Forces Pacific&quot;,&quot;Armed Forces Pacific&quot;],[&quot;California&quot;,&quot;California&quot;],[&quot;Colorado&quot;,&quot;Colorado&quot;],[&quot;Connecticut&quot;,&quot;Connecticut&quot;],[&quot;Delaware&quot;,&quot;Delaware&quot;],[&quot;District of Columbia&quot;,&quot;Washington DC&quot;],[&quot;Federated States of Micronesia&quot;,&quot;Micronesia&quot;],[&quot;Florida&quot;,&quot;Florida&quot;],[&quot;Georgia&quot;,&quot;Georgia&quot;],[&quot;Guam&quot;,&quot;Guam&quot;],[&quot;Hawaii&quot;,&quot;Hawaii&quot;],[&quot;Idaho&quot;,&quot;Idaho&quot;],[&quot;Illinois&quot;,&quot;Illinois&quot;],[&quot;Indiana&quot;,&quot;Indiana&quot;],[&quot;Iowa&quot;,&quot;Iowa&quot;],[&quot;Kansas&quot;,&quot;Kansas&quot;],[&quot;Kentucky&quot;,&quot;Kentucky&quot;],[&quot;Louisiana&quot;,&quot;Louisiana&quot;],[&quot;Maine&quot;,&quot;Maine&quot;],[&quot;Marshall Islands&quot;,&quot;Marshall Islands&quot;],[&quot;Maryland&quot;,&quot;Maryland&quot;],[&quot;Massachusetts&quot;,&quot;Massachusetts&quot;],[&quot;Michigan&quot;,&quot;Michigan&quot;],[&quot;Minnesota&quot;,&quot;Minnesota&quot;],[&quot;Mississippi&quot;,&quot;Mississippi&quot;],[&quot;Missouri&quot;,&quot;Missouri&quot;],[&quot;Montana&quot;,&quot;Montana&quot;],[&quot;Nebraska&quot;,&quot;Nebraska&quot;],[&quot;Nevada&quot;,&quot;Nevada&quot;],[&quot;New Hampshire&quot;,&quot;New Hampshire&quot;],[&quot;New Jersey&quot;,&quot;New Jersey&quot;],[&quot;New Mexico&quot;,&quot;New Mexico&quot;],[&quot;New York&quot;,&quot;New York&quot;],[&quot;North Carolina&quot;,&quot;North Carolina&quot;],[&quot;North Dakota&quot;,&quot;North Dakota&quot;],[&quot;Northern Mariana Islands&quot;,&quot;Northern Mariana Islands&quot;],[&quot;Ohio&quot;,&quot;Ohio&quot;],[&quot;Oklahoma&quot;,&quot;Oklahoma&quot;],[&quot;Oregon&quot;,&quot;Oregon&quot;],[&quot;Palau&quot;,&quot;Palau&quot;],[&quot;Pennsylvania&quot;,&quot;Pennsylvania&quot;],[&quot;Puerto Rico&quot;,&quot;Puerto Rico&quot;],[&quot;Rhode Island&quot;,&quot;Rhode Island&quot;],[&quot;South Carolina&quot;,&quot;South Carolina&quot;],[&quot;South Dakota&quot;,&quot;South Dakota&quot;],[&quot;Tennessee&quot;,&quot;Tennessee&quot;],[&quot;Texas&quot;,&quot;Texas&quot;],[&quot;Utah&quot;,&quot;Utah&quot;],[&quot;Vermont&quot;,&quot;Vermont&quot;],[&quot;Virgin Islands&quot;,&quot;U.S. Virgin Islands&quot;],[&quot;Virginia&quot;,&quot;Virginia&quot;],[&quot;Washington&quot;,&quot;Washington&quot;],[&quot;West Virginia&quot;,&quot;West Virginia&quot;],[&quot;Wisconsin&quot;,&quot;Wisconsin&quot;],[&quot;Wyoming&quot;,&quot;Wyoming&quot;]]">United States</option>
</select>
```

</page>

<page>
---
title: 'Liquid objects: currency'
description: 'Information about a currency, like the ISO code and symbol.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/currency'
  md: 'https://shopify.dev/docs/api/liquid/objects/currency.md'
---

# currency

Information about a currency, like the ISO code and symbol.

## Properties

* * **iso\_​code**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [ISO code](https://www.iso.org/iso-4217-currency-codes.html) of the currency.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the currency.

  * **symbol**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The symbol of the currency.

##### Example

```json
{
  "iso_code": "CAD",
  "name": "Canadian Dollar",
  "symbol": "$"
}
```

</page>

<page>
---
title: 'Liquid objects: current_page'
description: The current page number.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/current_page'
  md: 'https://shopify.dev/docs/api/liquid/objects/current_page.md'
---

# current\_​page

The current page number.

The `current_page` object has a value of 1 for non-paginated resources.

### Directly accessible in

* Global

##### Code

```liquid
{{ page_title }}{% unless current_page == 1 %} - Page {{ current_page }}{% endunless %}
```

##### Output

```html
Ingredients - Page 2
```

</page>

<page>
---
title: 'Liquid objects: current_tags'
description: The currently applied tags.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/current_tags'
  md: 'https://shopify.dev/docs/api/liquid/objects/current_tags.md'
---

# current\_​tags

The currently applied tags.

You can [add tags](https://help.shopify.com/en/manual/shopify-admin/productivity-tools/using-tags) to articles and products. Article tags can be used to [filter a blog page](https://shopify.dev/themes/architecture/templates/blog#filter-articles-by-tag) to show only articles with specific tags. Similarly, product tags can be used to [filter a collection page](https://shopify.dev/themes/navigation-search/filtering/tag-filtering) to show only products with specific tags.

### Directly accessible in

* [blog](https://shopify.dev/themes/architecture/templates/blog)
* [collection](https://shopify.dev/themes/architecture/templates/collection)

## Templates using current\_tags

[Theme architecture](https://shopify.dev/themes/architecture/templates/blog)

[blog template](https://shopify.dev/themes/architecture/templates/blog)

[Theme architecture](https://shopify.dev/themes/architecture/templates/collection)

[collection template](https://shopify.dev/themes/architecture/templates/collection)

</page>

<page>
---
title: 'Liquid objects: customer'
description: 'A [customer](https://help.shopify.com/manual/customers) of the store.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/customer'
  md: 'https://shopify.dev/docs/api/liquid/objects/customer.md'
---

# customer

A [customer](https://help.shopify.com/manual/customers) of the store.

The `customer` object is directly accessible globally when a customer is logged in to their account. It's also defined in the following contexts:

* The [`customers/account` template](https://shopify.dev/themes/architecture/templates/customers-account)
* The [`customers/addresses` template](https://shopify.dev/themes/architecture/templates/customers-addresses)
* The [`customers/order` template](https://shopify.dev/themes/architecture/templates/customers-order)
* When accessing [`checkout.customer`](https://shopify.dev/docs/api/liquid/objects/checkout#checkout-customer)
* When accessing [`gift_card.customer`](https://shopify.dev/docs/api/liquid/objects/gift_card#gift_card-customer)
* When accessing [`order.customer`](https://shopify.dev/docs/api/liquid/objects/order#order-customer)

Outside of the above contexts, if the customer isn't logged into their account, the `customer` object returns `nil`.

## Properties

* * **accepts\_​marketing**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the customer accepts marketing. Returns `false` if not.

  * **addresses**

    array of [address](https://shopify.dev/docs/api/liquid/objects/address)

  * All of the addresses associated with the customer.

    **Tip:** Use the \<a href="/docs/api/liquid/tags/paginate">paginate\</a> tag to choose how many addresses to show at once, up to a limit of 20.

  * **addresses\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of addresses associated with the customer.

  * **b2b?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the customer is a B2B customer. Returns `false` if not.

    To learn about B2B in themes, refer to [Support B2B customers in your theme](https://shopify.dev/themes/pricing-payments/b2b).

  * **company\_​available\_​locations**

    array of [company\_location](https://shopify.dev/docs/api/liquid/objects/company_location)

  * The company locations that the customer has access to, or can interact with.

    To learn about B2B in themes, refer to [Support B2B customers in your theme](https://shopify.dev/themes/pricing-payments/b2b).

    **Tip:** Use the \<a href="/docs/api/liquid/tags/paginate">paginate\</a> tag to choose how many company locations to show at once, up to a limit of 100.

  * **company\_​available\_​locations\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of company locations associated with the customer.

  * **current\_​company**

    [company](https://shopify.dev/docs/api/liquid/objects/company)

  * The company that the customer is purchasing for.

    To learn about B2B in themes, refer to [Support B2B customers in your theme](https://shopify.dev/themes/pricing-payments/b2b).

  * **current\_​location**

    [company\_location](https://shopify.dev/docs/api/liquid/objects/company_location)

  * The currently selected company location.

    To learn about B2B in themes, refer to [Support B2B customers in your theme](https://shopify.dev/themes/pricing-payments/b2b).

  * **default\_​address**

    [address](https://shopify.dev/docs/api/liquid/objects/address)

  * The default address of the customer.

  * **email**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The email of the customer.

  * **first\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The first name of the customer.

  * **has\_​account**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the email associated with the customer is tied to a [customer account](https://help.shopify.com/manual/customers/customer-accounts). Returns `false` if not.

    A customer can complete a checkout without making an account with the store. If the customer doesn't have an account with the store, then `customer.has_account` is `false` at checkout.

    During the checkout process, if the customer has an account with the store and enters an email associated with an account, then `customer.has_account` is `true`. The email is associated with the account regardless of whether the customer has logged into their account.

  * **has\_​avatar?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if an avatar is associated with a customer. Returns `false` if not.

    A customer may have an avatar associated with their account, which can be displayed in the storefront.

    **Tip:** Use with the \<a href="/docs/api/liquid/filters/avatar">\<code>avatar\</code>\</a> filter to render the customer\&#39;s avatar.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the customer.

  * **last\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The last name of the customer.

  * **last\_​order**

    [order](https://shopify.dev/docs/api/liquid/objects/order)

  * The last order placed by the customer, not including test orders.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The full name of the customer.

  * **orders**

    array of [order](https://shopify.dev/docs/api/liquid/objects/order)

  * All of the orders placed by the customer.

    **Tip:** Use the \<a href="/docs/api/liquid/tags/paginate">paginate\</a> tag to choose how many orders to show at once, up to a limit of 20.

  * **orders\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of orders that the customer has placed.

  * **payment\_​methods**

    array of [customer\_payment\_method](https://shopify.dev/docs/api/liquid/objects/customer_payment_method)

  * The customer's saved payment methods.

  * **phone**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The phone number of the customer.

    This phone number is only populated if the customer checks out using a phone number during checkout, opts in to SMS notifications, or if the merchant has manually entered it.

  * **store\_​credit\_​account**

    [store\_credit\_account](https://shopify.dev/docs/api/liquid/objects/store_credit_account)

  * The store credit account associated with the customer.

    The account shown will be in the currency associated with the customer’s current context. For example, if a customer is browsing the storefront in the US market, their USD store credit account will be returned. If they do not have a USD store credit account `nil` will be returned.

  * **tags**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The tags associated with the customer.

  * **tax\_​exempt**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the customer is exempt from taxes. Returns `false` if not.

  * **total\_​spent**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total amount that the customer has spent on all orders in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

##### Example

```json
{
  "accepts_marketing": true,
  "addresses": [],
  "addresses_count": 5,
  "b2b?": false,
  "company_available_locations": [],
  "company_available_locations_count": 1,
  "current_company": {},
  "current_location": null,
  "default_address": {},
  "email": "cornelius.potionmaker@gmail.com",
  "first_name": "Cornelius",
  "has_account": true,
  "has_avatar?": false,
  "id": 5625411010625,
  "last_name": "Potionmaker",
  "last_order": {},
  "name": "Cornelius Potionmaker",
  "orders": [],
  "orders_count": 1,
  "payment_methods": [],
  "phone": "+441314960905",
  "store_credit_account": {},
  "tags": [
    "newsletter"
  ],
  "tax_exempt": false,
  "total_spent": "56.00"
}
```

### Check whether the `customer` object is defined

When using the `customer` object outside of customer-specific templates or objects that specifically return a customer, you should check whether the `customer` object is defined.

##### Code

```liquid
{% if customer %}
  Hello, {{ customer.first_name }}!
{% endif %}
```

##### Data

```json
{
  "customer": {
    "first_name": "Cornelius"
  }
}
```

##### Output

```html
Hello, Cornelius!
```

## Templates using customer

[Theme architecture](https://shopify.dev/themes/architecture/templates/customers-account)

[customers/account template](https://shopify.dev/themes/architecture/templates/customers-account)

[Theme architecture](https://shopify.dev/themes/architecture/templates/customers-addresses)

[customers/addresses template](https://shopify.dev/themes/architecture/templates/customers-addresses)

[Theme architecture](https://shopify.dev/themes/architecture/templates/customers-order)

[customers/order template](https://shopify.dev/themes/architecture/templates/customers-order)

</page>

<page>
---
title: 'Liquid objects: customer_payment_method'
description: A customer's saved payment method.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/customer_payment_method'
  md: 'https://shopify.dev/docs/api/liquid/objects/customer_payment_method.md'
---

# customer\_​payment\_​method

A customer's saved payment method.

A payment method that a customer has saved to their account for reuse (e.g. a credit card).

## Properties

* * **payment\_​instrument\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The instrument type of the payment method (e.g credit\_card).

  * **token**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The identifier for the payment method.

### Returned by

* [customer.payment\_​methods](https://shopify.dev/docs/api/liquid/objects/customer#customer-payment_methods)

</page>

<page>
---
title: 'Liquid objects: discount'
description: 'A discount applied to a cart, line item, or order.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/discount'
  md: 'https://shopify.dev/docs/api/liquid/objects/discount.md'
---

# discount

A discount applied to a cart, line item, or order.

**deprecated:**

Deprecated because not all discount types and details are captured.

The `discount` object has been replaced by the [`discount_allocation`](https://shopify.dev/docs/api/liquid/objects/discount_allocation) and [`discount_application`](https://shopify.dev/docs/api/liquid/objects/discount_application) objects.

## Properties

* * **amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The amount of the discount in the currency's subunit.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/discount#discount-total\_amount">\<code>\<span class="PreventFireFoxApplyingGapToWBR">discount.total\<wbr/>\_amount\</span>\</code>\</a>.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **code**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The customer-facing name of the discount.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/discount#discount-title">\<code>discount.title\</code>\</a>.

  * **savings**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The amount of the discount as a negative value, in the currency's subunit.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/discount#discount-total\_savings">\<code>\<span class="PreventFireFoxApplyingGapToWBR">discount.total\<wbr/>\_savings\</span>\</code>\</a>. The value is output in the customer\&#39;s local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The customer-facing name of the discount.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/discount#discount-code">\<code>discount.code\</code>\</a>.

  * **total\_​amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The amount of the discount in the currency's subunit.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/discount#discount-amount">\<code>discount.amount\</code>\</a>.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **total\_​savings**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The amount of the discount as a negative value, in the currency's subunit.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/discount#discount-savings">\<code>discount.savings\</code>\</a>. The value is output in the customer\&#39;s local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of the discount.

    | Possible values |
    | - |
    | FixedAmountDiscount |
    | PercentageDiscount |
    | ShippingDiscount |

##### Example

```json
{
  "amount": "40.00",
  "code": "DIY",
  "savings": "-40.00",
  "title": "DIY",
  "total_amount": "40.00",
  "total_savings": "-40.00",
  "type": "PercentageDiscount"
}
```

</page>

<page>
---
title: 'Liquid objects: discount_allocation'
description: Information about how a discount affects an item.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/discount_allocation'
  md: 'https://shopify.dev/docs/api/liquid/objects/discount_allocation.md'
---

# discount\_​allocation

Information about how a discount affects an item.

To learn about how to display discounts in your theme, refer to [Discounts](https://shopify.dev/themes/pricing-payments/discounts).

## Properties

* * **amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The amount that the item is discounted by in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **discount\_​application**

    [discount\_application](https://shopify.dev/docs/api/liquid/objects/discount_application)

  * The discount application that applies the discount to the item.

##### Example

```json
{
  "amount": "40.00",
  "discount_application": "DiscountApplicationDrop"
}
```

</page>

<page>
---
title: 'Liquid objects: discount_application'
description: Information about the intent of a discount.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/discount_application'
  md: 'https://shopify.dev/docs/api/liquid/objects/discount_application.md'
---

# discount\_​application

Information about the intent of a discount.

To learn about how to display discounts in your theme, refer to [Discounts](https://shopify.dev/themes/pricing-payments/discounts).

## Properties

* * **target\_​selection**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The selection method for line items or shipping lines to be discounted.

    **Note:** Whether the selection method applies to line items or shipping lines depends on the discount\&#39;s \<a href="/docs/api/liquid/objects/discount\_application#discount\_application-target\_type">target type\</a>.

    | Possible values | Description |
    | - | - |
    | all | The discount applies to all line items or shipping lines. |
    | entitled | The discount applies to a specific set of line items or shipping lines based on some criteria. |
    | explicit | The discount applies to a specific line item or shipping line. |

  * **target\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of item that the discount applies to.

    | Possible values |
    | - |
    | line\_item |
    | shipping\_line |

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The customer-facing name of the discount.

  * **total\_​allocated\_​amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total amount of the discount in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of the discount.

    | Possible values |
    | - |
    | automatic |
    | discount\_code |
    | manual |
    | script |

  * **value**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The value of the discount.

    How this value is interpreted depends on the [value type](https://shopify.dev/docs/api/liquid/objects/discount_application#discount_application-value_type) of the discount. The following table outlines what the value represents for each value type:

    | Value type | Value |
    | - | - |
    | `fixed_amount` | The amount of the discount in the currency's subunit. |
    | `percentage` | The percent amount of the discount. |

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **value\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The value type of the discount.

    | Possible values |
    | - |
    | fixed\_amount |
    | percentage |

##### Example

```json
{
  "target_selection": "explicit",
  "target_type": "line_item",
  "title": "Bloodroot discount!",
  "total_allocated_amount": "2.50",
  "type": "script",
  "value": "2.5",
  "value_type": "fixed_amount"
}
```

</page>

<page>
---
title: 'Liquid objects: external_video'
description: Information about an external video from YouTube or Vimeo.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/external_video'
  md: 'https://shopify.dev/docs/api/liquid/objects/external_video.md'
---

# external\_​video

Information about an external video from YouTube or Vimeo.

***

**Tip:** Use the \<a href="/docs/api/liquid/filters/external\_video\_tag">\<code>\<span class="PreventFireFoxApplyingGapToWBR">external\<wbr/>\_video\<wbr/>\_tag\</span>\</code> filter\</a> to output the video in an HTML \<code>\&lt;iframe\&gt;\</code> tag. Use the \<a href="/docs/api/liquid/filters/external\_video\_url">\<code>\<span class="PreventFireFoxApplyingGapToWBR">external\<wbr/>\_video\<wbr/>\_url\</span>\</code> filter\</a> to specify parameters for the external video player.

***

## Properties

* * **alt**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The alt text of the external video.

  * **aspect\_​ratio**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The aspect ratio of the video as a decimal.

  * **external\_​id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the video from its external source.

  * **host**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The service that hosts the video.

    | Possible values |
    | - |
    | youtube |
    | vimeo |

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the external video.

  * **media\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The media type of the external video. Always returns `external_video`.

    ExampleFilter for media of a specific type

    You can use the `media_type` property with the [`where` filter](https://shopify.dev/docs/api/liquid/filters/where) to filter the [`product.media` array](https://shopify.dev/docs/api/liquid/objects/product#product-media) for all media of a desired type.

    ##### Code

    ```liquid
    {% assign external_videos = product.media | where: 'media_type', 'external_video' %}

    {% for external_video in external_videos %}
      {{- external_video | external_video_tag }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "product": {
        "media": [
          {
            "media_type": "external_video"
          },
          {
            "media_type": "video"
          }
        ]
      }
    }
    ```

    ##### Output

    ```html
    <iframe frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="allowfullscreen" src="https://www.youtube.com/embed/vj01PAffOac?controls=1&amp;enablejsapi=1&amp;modestbranding=1&amp;origin=https%3A%2F%2Fpolinas-potent-potions.myshopify.com&amp;playsinline=1&amp;rel=0" title="Potion beats"></iframe>
    ```

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The position of the external video in the [`product.media`](https://shopify.dev/docs/api/liquid/objects/product#product-media) array.

  * **preview\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * A preview image of the media.

    **Note:** Preview images don\&#39;t have an ID attribute.

##### Example

```json
{
  "alt": "Potion beats",
  "aspect_ratio": "1.77",
  "external_id": "vj01PAffOac",
  "host": "youtube",
  "id": 22015756402753,
  "media_type": "external_video",
  "position": 1,
  "preview_image": {}
}
```

</page>

<page>
---
title: 'Liquid objects: filter'
description: >-
  A [storefront
  filter](https://help.shopify.com/manual/online-store/themes/customizing-themes/storefront-filters).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/filter'
  md: 'https://shopify.dev/docs/api/liquid/objects/filter.md'
---

# filter

A [storefront filter](https://help.shopify.com/manual/online-store/themes/customizing-themes/storefront-filters).

To learn about supporting filters in your theme, refer to [Support storefront filtering](https://shopify.dev/themes/navigation-search/filtering/storefront-filtering/support-storefront-filtering).

## Properties

* * **active\_​values**

    array of [filter\_value](https://shopify.dev/docs/api/liquid/objects/filter_value)

  * The values of the filter that are currently active.

    The array can have values only for `boolean` and `list` type filters.

  * **false\_​value**

    [filter\_value](https://shopify.dev/docs/api/liquid/objects/filter_value)

  * The `false` filter value.

    Returns a value for `boolean` type filters if the unfiltered view has at least one result with the `false` filter value. Otherwise, it returns `nil`.

  * **inactive\_​values**

    array of [filter\_value](https://shopify.dev/docs/api/liquid/objects/filter_value)

  * The values of the filter that are currently inactive.

    The array can have values only for `boolean` and `list` type filters.

  * **label**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The customer-facing label for the filter.

  * **max\_​value**

    [filter\_value](https://shopify.dev/docs/api/liquid/objects/filter_value)

  * The highest filter value.

    Returns a value only for `price_range` type filters. Returns `nil` for other types.

  * **min\_​value**

    [filter\_value](https://shopify.dev/docs/api/liquid/objects/filter_value)

  * The lowest filter value.

    Returns a value only for `price_range` type filters. Returns `nil` for other types.

  * **operator**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The logical operator used by the filter. Returns a value only for `boolean` and `list` type filters. Returns `nil` for other types.

    Example: For a filter named `color` with values `red` and `blue`:

    * If the operator is `AND`, it will filter items that are both red and blue.
    * If the operator is `OR`, it will filter items that are either red or blue or both.

    Filters that support the `AND` operator:

    * Product tags
    * Metafields of type `list.single_line_text_field` and `list.metaobject_reference`

    | Possible values | Description |
    | - | - |
    | AND | Includes products that match all buyer selections. |
    | OR | Includes products that match at least one buyer selection. |

  * **param\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL parameter for the filter. For example, `filter.v.option.color`.

  * **presentation**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * Describes how to present the filter values.

    Returns a value only for `list` type filters. Returns `nil` for other types.

    | Possible values |
    | - |
    | image |
    | swatch |
    | text |

  * **range\_​max**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The highest product price within the collection or search results.

    Returns a value only for `price_range` type filters. Returns `nil` for other types.

  * **true\_​value**

    [filter\_value](https://shopify.dev/docs/api/liquid/objects/filter_value)

  * The `true` filter value.

    Returns a value for `boolean` type filters if the unfiltered view has at least one result with the `true` filter value. Otherwise, it returns `nil`.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of the filter.

    | Possible values |
    | - |
    | boolean |
    | list |
    | price\_range |

  * **url\_​to\_​remove**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The current page URL with the URL parameter related to the filter removed.

  * **values**

    array of [filter\_value](https://shopify.dev/docs/api/liquid/objects/filter_value)

  * The values of the filter.

    The array can have values only for `boolean` and `list` type filters.

### Returned by

* [collection.filters](https://shopify.dev/docs/api/liquid/objects/collection#collection-filters)
* [search.filters](https://shopify.dev/docs/api/liquid/objects/search#search-filters)

</page>

<page>
---
title: 'Liquid objects: filter_value'
description: A specific value of a filter.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/filter_value'
  md: 'https://shopify.dev/docs/api/liquid/objects/filter_value.md'
---

# filter\_​value

A specific value of a filter.

To learn about supporting filters in your theme, refer to [Support storefront filtering](https://shopify.dev/themes/navigation-search/filtering/storefront-filtering/support-storefront-filtering).

## Properties

* * **active**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the value is currently active. Returns `false` if not.

    Can only return `true` for filters of type `boolean` or `list`.

  * **count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of results related to the filter value.

    Returns a value only for `boolean` and `list` type filters. Returns `nil` for `price_range` type filters.

  * **image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The visual representation of the filter value when an image is used.

    Returns an [image](https://shopify.dev/docs/api/liquid/objects/image) drop for the filter value. Requires the [filter presentation](https://shopify.dev/docs/api/liquid/objects/filter#filter-presentation) to be `image` and for an image to be available. Otherwise, returns `nil`.

  * **label**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The customer-facing label for the filter value. For example, `Red` or `Rouge`.

    Returns a value only for `boolean` and `list` type filters. Returns `nil` for `price_range` type filters.

  * **param\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL parameter for the parent filter of the filter value.

    For example, `filter.v.option.color`.

    Filters of type `price_range` include an extra component depending on whether the filter value is for the filter's `min_value` or `max_value`. The following table outlines the URL parameter for each:

    | Value type | URL parameter |
    | - | - |
    | `min_value` | `filter.v.price.gte` |
    | `max_value` | `filter.v.price.lte` |

  * **swatch**

    [swatch](https://shopify.dev/docs/api/liquid/objects/swatch)

  * The visual representation of the filter value when a swatch is used.

    Returns a [swatch](https://shopify.dev/docs/api/liquid/objects/swatch) drop for the filter value. Requires the [filter presentation](https://shopify.dev/docs/api/liquid/objects/filter#filter-presentation) to be `swatch` and saved color or image content for the swatch. Otherwise, returns `nil`.

  * **url\_​to\_​add**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The current page URL with the filter value parameter added.

    **Note:** Any \<a href="/docs/api/liquid/tags/paginate">pagination\</a> URL parameters are removed.

  * **url\_​to\_​remove**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The current page URL with the filter value parameter removed.

    **Note:** Any \<a href="/docs/api/liquid/tags/paginate">pagination\</a> URL parameters are also removed.

  * **value**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The value for the URL parameter. The `value` is paired with the [`param_name`](#filter_value-param_name) property.

    For example, `High` will be used in the URL as `filter.v.option.strength=High`.

## Deprecated Properties

* * **display**

    [filter\_value\_display](https://shopify.dev/docs/api/liquid/objects/filter_value_display)

    Deprecated

  * The visual representation of the filter value.

    Returns a visual representation for the filter value. If no visual representation is available, then `nil` is returned.

    **Deprecated:**

    Deprecated in favor of the [swatch](#swatch) attribute.

### Returned by

* [filter](https://shopify.dev/docs/api/liquid/objects/filter)
* [filter.false\_​value](https://shopify.dev/docs/api/liquid/objects/filter#filter-false_value)
* [filter.true\_​value](https://shopify.dev/docs/api/liquid/objects/filter#filter-true_value)
* [filter.max\_​value](https://shopify.dev/docs/api/liquid/objects/filter#filter-max_value)
* [filter.min\_​value](https://shopify.dev/docs/api/liquid/objects/filter#filter-min_value)

</page>

<page>
---
title: 'Liquid objects: filter_value_display'
description: The visual representation of a filter value.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/filter_value_display'
  md: 'https://shopify.dev/docs/api/liquid/objects/filter_value_display.md'
---

# filter\_​value\_​display

The visual representation of a filter value.

**deprecated:**

Deprecated in favor of the [swatch](https://shopify.dev/docs/api/liquid/objects/swatch) drop.

## Properties

* * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of visual representation.

    | Possible values |
    | - |
    | colors |
    | image |

  * **value**

  * The visual representation.

    Can be a list of [`colors`](https://shopify.dev/docs/api/liquid/objects/color) or an [`image`](https://shopify.dev/docs/api/liquid/objects/image). Refer to the [`type`](#filter_value_display-type) property to determine the type of visual representation.

### Returned by

* [filter\_​value.display](https://shopify.dev/docs/api/liquid/objects/filter_value#filter_value-display)

</page>

<page>
---
title: 'Liquid objects: focal_point'
description: The focal point for an image.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/focal_point'
  md: 'https://shopify.dev/docs/api/liquid/objects/focal_point.md'
---

# focal\_​point

The focal point for an image.

The focal point will remain visible when the image is cropped by the theme. [Learn more about supporting focal points in your theme](https://shopify.dev/themes/architecture/settings/input-settings#image-focal-points).

***

**Tip:** Use the \<a href="/docs/api/liquid/filters/image\_tag">\<code>\<span class="PreventFireFoxApplyingGapToWBR">image\<wbr/>\_tag\</span>\</code>\</a> filter to automatically apply focal point settings to an image on the storefront. This applies the focal point using the \<code>object-position\</code> CSS property.

***

## Properties

* * **x**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The horizontal position of the focal point, as a percent of the image width. Returns `50` if no focal point is set.

  * **y**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The vertical position of the focal point, as a percent of the image height. Returns `50` if no focal point is set.

### Returned by

* [image\_​presentation.focal\_​point](https://shopify.dev/docs/api/liquid/objects/image_presentation#image_presentation-focal_point)

### Referencing the `focal_point` object directly

When a `focal_point` object is referenced directly, the coordinates are returned as a string, in the format `X% Y%`.

##### Code

```liquid
{{ images['potions-header.png'].presentation.focal_point }}
```

##### Output

```html
1.9231% 9.7917%
```

</page>

<page>
---
title: 'Liquid objects: font'
description: >-
  A font from a [`font_picker`
  setting](/themes/architecture/settings/input-settings#font_picker).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/font'
  md: 'https://shopify.dev/docs/api/liquid/objects/font.md'
---

# font

A font from a [`font_picker` setting](https://shopify.dev/themes/architecture/settings/input-settings#font_picker).

You can use the `font` object in Liquid [assets](https://shopify.dev/themes/architecture#assets) or inside a [`style` tag](https://shopify.dev/docs/api/liquid/tags/style) to apply font setting values to theme CSS.

***

**Tip:** Use \<a href="/docs/api/liquid/filters/font-filters">font filters\</a> to modify properties of the \<code>font\</code> object, load the font, or obtain font variants.

***

## Properties

* * **baseline\_​ratio**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The baseline ratio of the font as a decimal.

  * **fallback\_​families**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The fallback families of the font.

  * **family**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The family name of the font.

    **Tip:** If the family name contains non-alphanumeric characters (A-Z, a-z, 0-9, or \&#39;-\&#39;), then it will be wrapped in double quotes.

  * **style**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The style of the font.

  * **system?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the font is a system font. Returns `false` if not.

    **Tip:** You can use this property to determine whether you need to include a corresponding \<a href="/docs/api/liquid/filters/font\_face">font-face\</a> declaration for the font.

  * **variants**

    array of [font](https://shopify.dev/docs/api/liquid/objects/font)

  * The variants in the family of the font.

  * **weight**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The weight of the font.

##### Example

```json
{
  "baseline_ratio": 0.133,
  "fallback_families": "sans-serif",
  "family": "Assistant",
  "style": "normal",
  "system?": false,
  "variants": {},
  "weight": "400"
}
```

</page>

<page>
---
title: 'Liquid objects: forloop'
description: 'Information about a parent [`for` loop](/docs/api/liquid/tags/for).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/forloop'
  md: 'https://shopify.dev/docs/api/liquid/objects/forloop.md'
---

# forloop

Information about a parent [`for` loop](https://shopify.dev/docs/api/liquid/tags/for).

## Properties

* * **first**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current iteration is the first. Returns `false` if not.

  * **index**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current iteration.

  * **index0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current iteration.

  * **last**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current iteration is the last. Returns `false` if not.

  * **length**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of iterations in the loop.

  * **parentloop**

    [forloop](https://shopify.dev/docs/api/liquid/objects/forloop)

  * The parent `forloop` object.

    If the current `for` loop isn't nested inside another `for` loop, then `nil` is returned.

    ExampleUse the `parentloop` property

    ##### Code

    ```liquid
    {% for i in (1..3) -%}
      {% for j in (1..3) -%}
        {{ forloop.parentloop.index }} - {{ forloop.index }}
      {%- endfor %}
    {%- endfor %}
    ```

    ##### Output

    ```html
    1 - 1
    1 - 2
    1 - 3

    2 - 1
    2 - 2
    2 - 3

    3 - 1
    3 - 2
    3 - 3
    ```

  * **rindex**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current iteration, in reverse order.

  * **rindex0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current iteration, in reverse order.

##### Example

```json
{
  "first": true,
  "index": 1,
  "index0": 0,
  "last": false,
  "length": 4,
  "rindex": 3
}
```

### Use the `forloop` object

##### Code

```liquid
{% for page in pages -%}
  {%- if forloop.length > 0 -%}
    {{ page.title }}{% unless forloop.last %}, {% endunless -%}
  {%- endif -%}
{% endfor %}
```

##### Output

```html
About us, Contact, Potion dosages
```

</page>

<page>
---
title: 'Liquid objects: form'
description: >-
  Information about a form created by a [`form`
  tag](/docs/api/liquid/tags/form).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/form'
  md: 'https://shopify.dev/docs/api/liquid/objects/form.md'
---

# form

Information about a form created by a [`form` tag](https://shopify.dev/docs/api/liquid/tags/form).

## Properties

* * **address1**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The first address line associated with the address.

    This property is exclusive to the [`customer_address` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address).

  * **address2**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The second address line associated with the address.

    This property is exclusive to the [`customer_address` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address).

  * **author**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the author of the article comment.

    This property is exclusive to the [`new_comment` form](https://shopify.dev/docs/api/liquid/tags/form#form-new_comment).

  * **body**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The content of the contact submission or article comment.

    This property is exclusive to the [`contact`](https://shopify.dev/docs/api/liquid/tags/form#form-contact) and [`new_comment`](https://shopify.dev/docs/api/liquid/tags/form#form-new_comment) forms.

  * **city**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The city associated with the address.

    This property is exclusive to the [`customer_address` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address).

  * **company**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The company associated with the address.

    This property is exclusive to the [`customer_address` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address).

  * **country**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The country associated with the address.

    This property is exclusive to the [`customer_address` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address).

  * **email**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The email associated with the form.

    This property is exclusive to the following forms:

    * [`contact`](https://shopify.dev/docs/api/liquid/tags/form#form-contact)
    * [`create_customer`](https://shopify.dev/docs/api/liquid/tags/form#form-create_customer)
    * [`customer`](https://shopify.dev/docs/api/liquid/tags/form#form-customer)
    * [`customer_login`](https://shopify.dev/docs/api/liquid/tags/form#form-customer_login)
    * [`new_comment`](https://shopify.dev/docs/api/liquid/tags/form#form-new_comment)
    * [`recover_customer_password`](https://shopify.dev/docs/api/liquid/tags/form#form-recover_customer_password)
    * [`product`](https://shopify.dev/docs/api/liquid/tags/form#form-product)

  * **errors**

    [form\_errors](https://shopify.dev/docs/api/liquid/objects/form_errors)

  * Any errors from the form.

    If there are no errors, then `nil` is returned.

    **Tip:** You can apply the \<a href="/docs/api/liquid/filters/default\_errors">\<code>\<span class="PreventFireFoxApplyingGapToWBR">default\<wbr/>\_errors\</span>\</code> filter\</a> to \<code>form.errors\</code> to output default error messages without having to loop through the array.

  * **first\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The first name associated with the customer or address.

    This property is exclusive to the [`create_customer`](https://shopify.dev/docs/api/liquid/tags/form#form-create_customer) and [`customer_address`](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address) forms.

  * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the form.

  * **last\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The last name associated with the customer or address.

    This property is exclusive to the [`create_customer`](https://shopify.dev/docs/api/liquid/tags/form#form-create_customer) and [`customer_address`](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address) forms.

  * **message**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The personalized message intended for the recipient.

    This property is exclusive to the [`product` form](https://shopify.dev/docs/api/liquid/tags/form#form-product).

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The nickname of the gift card recipient.

    This property is exclusive to the [`product` form](https://shopify.dev/docs/api/liquid/tags/form#form-product).

  * **password\_​needed**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true`.

    This property is exclusive to the [`customer_login` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_login).

  * **phone**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The phone number associated with the address.

    This property is exclusive to the [`customer_address` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address).

  * **posted\_​successfully?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the form was submitted successfully. Returns `false` if there were errors.

    **Note:** The \<a href="/docs/api/liquid/tags/form#form-customer\_address">\<code>\<span class="PreventFireFoxApplyingGapToWBR">customer\<wbr/>\_address\</span>\</code> form\</a> always returns \<code>true\</code>.

  * **province**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The province associated with the address.

    This property is exclusive to the [`customer_address` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address).

  * **set\_​as\_​default\_​checkbox**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Renders an HTML checkbox that can submit the address as the customer's default address.

    This property is exclusive to the [`customer_address` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address).

  * **zip**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The zip or postal code associated with the address.

    This property is exclusive to the [`customer_address` form](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address).

##### Example

```json
{
  "address1": "12 Phoenix Feather Alley",
  "address2": "1",
  "author": null,
  "body": null,
  "city": "Calgary",
  "company": null,
  "country": "Canada",
  "email": null,
  "errors": null,
  "first_name": "Cornelius",
  "id": "new",
  "last_name": "Potionmaker",
  "password_needed?": false,
  "phone": "44 131 496 0905",
  "posted_successfully?": true,
  "province": "Alberta",
  "set_as_default_checkbox": "<input type='checkbox' id='address_default_address_new' name='address[default]' value='1'>",
  "zip": "T1X 0L4"
}
```

</page>

<page>
---
title: 'Liquid objects: form_errors'
description: >-
  The error category strings for errors from a form created by a [`form`
  tag](/docs/api/liquid/tags/form).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/form_errors'
  md: 'https://shopify.dev/docs/api/liquid/objects/form_errors.md'
---

# form\_​errors

The error category strings for errors from a form created by a [`form` tag](https://shopify.dev/docs/api/liquid/tags/form).

The following table outlines the strings that can be returned and the reason that they would be:

| Form property name | Return reason |
| - | - |
| `author` | There were issues with required name fields. |
| `body` | There were issues with required text content fields. |
| `email` | There were issues with required email fields. |
| `form` | There were general issues with the form. |
| `password` | There were issues with required password fields. |

## Properties

* * **messages**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The translated error messages for each value in the `form_errors` array.

    You can access a specific message in the array by using a specific error from the `form_errors` array as a key.

  * **translated\_​fields**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The translated names for each value in the `form_errors` array.

    You can access a specific field in the array by using a specific error from the `form_errors` array as a key.

##### Example

```json
{
  "messages": {},
  "translated_fields": {}
}
```

### Output form errors

You can output the name of the field related to the error, and the error message, by using the error as a key to access the `translated_fields` and `messages` properties.

```liquid
<ul>
  {% for error in form.errors %}
    <li>
      {% if error == 'form' %}
        {{ form.errors.messages[error] }}
      {% else %}
        {{ form.errors.translated_fields[error] }} - {{ form.errors.messages[error] }}
      {% endif %}
    </li>
  {% endfor %}
</ul>
```

```liquid

  {% for error in form.errors %}
    

      {% if error == 'form' %}
        {{ form.errors.messages[error] }}
      {% else %}
        {{ form.errors.translated_fields[error] }} - {{ form.errors.messages[error] }}
      {% endif %}
    

  {% endfor %}
```

</page>

<page>
---
title: 'Liquid objects: fulfillment'
description: >-
  An order [fulfillment](https://help.shopify.com/manual/orders/fulfillment),
  which includes information like the line items

  being fulfilled and shipment tracking.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/fulfillment'
  md: 'https://shopify.dev/docs/api/liquid/objects/fulfillment.md'
---

# fulfillment

An order [fulfillment](https://help.shopify.com/manual/orders/fulfillment), which includes information like the line items being fulfilled and shipment tracking.

## Properties

* * **created\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the fulfillment was created.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **fulfillment\_​line\_​items**

    array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

  * The line items in the fulfillment.

  * **item\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of items in the fulfillment.

  * **tracking\_​company**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the fulfillment service.

  * **tracking\_​number**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The fulfillment's tracking number.

    If there's no tracking number, then `nil` is returned.

  * **tracking\_​numbers**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * An array of the fulfillment's tracking numbers.

  * **tracking\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the fulfillment's tracking number.

    If there's no tracking number, then `nil` is returned.

##### Example

```json
{
  "created_at": "2022-06-15 17:08:30 -0400",
  "fulfillment_line_items": [
    {
      "quantity": 2,
      "line_item": "LineItemDrop"
    },
    {
      "quantity": 1,
      "line_item": "LineItemDrop"
    }
  ],
  "item_count": 3,
  "tracking_company": "Canada Post",
  "tracking_number": "01189998819991197253",
  "tracking_numbers": [
    "01189998819991197253"
  ],
  "tracking_url": "https://www.canadapost-postescanada.ca/track-reperage/en#/search?searchFor=01189998819991197253"
}
```

</page>

<page>
---
title: 'Liquid objects: generic_file'
description: >-
  A file from a `file_reference` type
  [metafield](/docs/api/liquid/objects/metafield) that is neither an image or
  video.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/generic_file'
  md: 'https://shopify.dev/docs/api/liquid/objects/generic_file.md'
---

# generic\_​file

A file from a `file_reference` type [metafield](https://shopify.dev/docs/api/liquid/objects/metafield) that is neither an image or video.

***

**Tip:** To learn about metafield types, refer to \<a href="/apps/metafields/types">Metafield types\</a>.

***

## Properties

* * **alt**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The alt text of the media.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the file.

  * **media\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The media type of the model. Always returns `generic_file`.

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The position of the media in the [`product.media` array](https://shopify.dev/docs/api/liquid/objects/product#product-media).

    If the source is a [`file_reference` metafield](https://shopify.dev/apps/metafields/types), then `nil` is returned.

  * **preview\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * A preview image for the file.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) for the file.

##### Example

```json
{
  "alt": null,
  "id": 21918386454593,
  "media_type": "generic_file",
  "position": null,
  "preview_image": {},
  "url": "//polinas-potent-potions.myshopify.com/cdn/shop/files/disclaimer.pdf?v=9043651738044769859"
}
```

</page>

<page>
---
title: 'Liquid objects: gift_card'
description: >-
  A [gift card](https://help.shopify.com/manual/products/gift-card-products)
  that's been issued to a customer or a recipient.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/gift_card'
  md: 'https://shopify.dev/docs/api/liquid/objects/gift_card.md'
---

# gift\_​card

A [gift card](https://help.shopify.com/manual/products/gift-card-products) that's been issued to a customer or a recipient.

## Properties

* * **balance**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The remaining balance of the gift card in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **code**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The code used to redeem the gift card.

  * **currency**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [ISO code](https://www.iso.org/iso-4217-currency-codes.html) of the currency that the gift card was issued in.

  * **customer**

    [customer](https://shopify.dev/docs/api/liquid/objects/customer)

  * The customer associated with the gift card.

  * **enabled**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the gift card is enabled. Returns `false` if not.

  * **expired**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the gift card is expired. Returns `false` if not.

  * **expires\_​on**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the gift card expires.

    If the gift card never expires, then `nil` is returned.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **initial\_​value**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The initial balance of the gift card in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **last\_​four\_​characters**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The last 4 characters of the code used to redeem the gift card.

  * **message**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The personalized message intended for the recipient.

    If there is no message intended for the recipient, then `nil` is returned.

  * **pass\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL to download the gift card as an Apple Wallet Pass.

  * **product**

    [product](https://shopify.dev/docs/api/liquid/objects/product)

  * The product associated with the gift card.

  * **properties**

  * The [line item properties](https://shopify.dev/docs/api/liquid/objects/line_item#line_item-properties) assigned to the gift card.

    If there aren't any line item properties, then an [`EmptyDrop`](https://shopify.dev/docs/api/liquid/basics#emptydrop) is returned.

  * **qr\_​identifier**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A string used to generate a QR code for the gift card.

  * **recipient**

    [recipient](https://shopify.dev/docs/api/liquid/objects/recipient)

  * The recipient associated with the gift card.

    If there is no recipient associated with the gift card, then `nil` is returned.

  * **send\_​on**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The scheduled date on which the gift card will be sent to the recipient.

    If the gift card does not have a scheduled date, then `nil` is returned.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the date.

  * **template\_​suffix**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the [custom template](https://shopify.dev/themes/architecture/templates#alternate-templates) assigned to the gift card.

    The name doesn't include the `gift_card.` prefix, or the `.liquid` file extension.

    If a custom template isn't assigned to the gift card, then `nil` is returned.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL to view the gift card. This URL is on the `checkout.shopify.com` domain.

    **Tip:** The page at this URL is rendered through the \<a href="/themes/architecture/templates/gift-card-liquid">\<code>\<span class="PreventFireFoxApplyingGapToWBR">gift\<wbr/>\_card.liquid\</span>\</code> template\</a> of the theme.

  * **variant**

    [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The variant associated with the gift card.

    If there is no variant associated with the gift card, then `nil` is returned.

##### Example

```json
{
  "balance": 5000,
  "code": "WCGX 7X97 K9HJ DFR8",
  "currency": "CAD",
  "customer": {},
  "enabled": true,
  "expired": false,
  "expires_on": null,
  "initial_value": 5000,
  "last_four_characters": "DFR8",
  "message": null,
  "send_on": null,
  "pass_url": "https://polinas-potent-potions.myshopify.com/v1/passes/pass.com.shopify.giftcardnext/94af7fbe55d010130df8d8bc4a338d36/",
  "product": {},
  "variant": {},
  "properties": {},
  "qr_identifier": "shopify-giftcard-v1-3TKWJKJBM3X7PBRK",
  "recipient": null,
  "template_suffix": null,
  "url": "https://checkout.shopify.com/gift_cards/56174706753/0011c591fc720d0a51b80cdb694f969e"
}
```

## Templates using gift\_card

[Theme architecture](https://shopify.dev/themes/architecture/templates/gift-card-liquid)

[gift\_card.liquid template](https://shopify.dev/themes/architecture/templates/gift-card-liquid)

</page>

<page>
---
title: 'Liquid objects: group'
description: A group of rules for the `robots.txt` file.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/group'
  md: 'https://shopify.dev/docs/api/liquid/objects/group.md'
---

# group

A group of rules for the `robots.txt` file.

***

**Tip:** You can \<a href="/themes/seo/robots-txt">customize the \<code>robots.txt\</code> file\</a> with the \<a href="/themes/architecture/templates/robots-txt-liquid">\<code>robots.txt.liquid\</code> template\</a>.

***

## Properties

* * **rules**

    array of [rule](https://shopify.dev/docs/api/liquid/objects/rule)

  * The rules in the group.

  * **sitemap**

    [sitemap](https://shopify.dev/docs/api/liquid/objects/sitemap)

  * The sitemap for the group.

    If the group doesn't require a sitemap, then `blank` is returned.

    The sitemap can be accessed at `/sitemap.xml`.

  * **user\_​agent**

    [user\_agent](https://shopify.dev/docs/api/liquid/objects/user_agent)

  * The user agent for the group.

##### Example

```json
{
  "rules": [],
  "sitemap": {},
  "user_agent": {}
}
```

</page>

<page>
---
title: 'Liquid objects: handle'
description: >-
  The [handle](/docs/api/liquid/basics#handles) of the resource associated with
  the current template.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/handle'
  md: 'https://shopify.dev/docs/api/liquid/objects/handle.md'
---

# handle

The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the resource associated with the current template.

The `handle` object will return a value only when the following templates are being viewed:

* [article](https://shopify.dev/themes/architecture/templates/article)
* [blog](https://shopify.dev/themes/architecture/templates/blog)
* [collection](https://shopify.dev/themes/architecture/templates/collection)
* [page](https://shopify.dev/themes/architecture/templates/page)
* [product](https://shopify.dev/themes/architecture/templates/product)

If none of the above templates are being viewed, then `nil` is returned.

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: image'
description: 'An image, such as a product or collection image.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/image'
  md: 'https://shopify.dev/docs/api/liquid/objects/image.md'
---

# image

An image, such as a product or collection image.

To learn about the image formats that Shopify supports, visit the [Shopify Help Center](https://help.shopify.com/manual/online-store/images/theme-images#image-formats).

***

**Tip:** Use the \<a href="/docs/api/liquid/filters/image\_url">\<code>\<span class="PreventFireFoxApplyingGapToWBR">image\<wbr/>\_url\</span>\</code>\</a> and \<a href="/docs/api/liquid/filters/image\_tag">\<code>\<span class="PreventFireFoxApplyingGapToWBR">image\<wbr/>\_tag\</span>\</code>\</a> filters to display images on the storefront.

***

## Properties

* * **alt**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The alt text of the image.

  * **aspect\_​ratio**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The aspect ratio of the image as a decimal.

  * **attached\_​to\_​variant?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the image is associated with a variant. Returns `false` if not.

    The `attached_to_variant?` property is only available for images accessed through the following sources:

    * [`product.featured_image`](https://shopify.dev/docs/api/liquid/objects/product#product-featured_image)
    * [`product.images`](https://shopify.dev/docs/api/liquid/objects/product#product-images)

    If you reference this property on an image from another source, then `nil` is returned.

  * **height**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The height of the image in pixels.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the image.

    If you reference the `id` property for preview images of [`generic_file`](https://shopify.dev/docs/api/liquid/objects/generic_file) or [`media`](https://shopify.dev/docs/api/liquid/objects/media) objects, then `nil` is returned.

  * **media\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The media type of the image. Always returns `image`.

    The `media_type` property is only available for images accessed through the following sources:

    * [`product.media`](https://shopify.dev/docs/api/liquid/objects/product#product-media)
    * [`file_reference` type metafields](https://shopify.dev/apps/metafields/types#supported-types)

    If you reference this property on an image from another source, then `nil` is returned.

    ExampleFilter for media of a specific type

    You can use the `media_type` property with the [`where` filter](https://shopify.dev/docs/api/liquid/filters/where) to filter the [`product.media` array](https://shopify.dev/docs/api/liquid/objects/product#product-media) for all media of a desired type.

    ##### Code

    ```liquid
    {% assign images = product.media | where: 'media_type', 'image' %}

    {% for image in images %}
      {{- image | image_url: width: 300 | image_tag }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "product": {
        "media": [
          "products/oil-dripping-into-jar.jpg"
        ]
      }
    }
    ```

    ##### Output

    ```html
    <img src="//polinas-potent-potions.myshopify.com/cdn/shop/products/oil-dripping-into-jar.jpg?v=1650399519&amp;width=300" alt="Viper venom" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/products/oil-dripping-into-jar.jpg?v=1650399519&amp;width=300 300w" width="300" height="200">
    ```

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The position of the image in the [`product.images`](https://shopify.dev/docs/api/liquid/objects/product#product-images) or [`product.media`](https://shopify.dev/docs/api/liquid/objects/product#product-media) array.

    The `position` property is only available for images that are associated with a product. If you reference this property on an image from another source, then `nil` is returned.

  * **presentation**

    [image\_presentation](https://shopify.dev/docs/api/liquid/objects/image_presentation)

  * The presentation settings for the image.

  * **preview\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * A preview image for the image.

    The `preview_image` property is only available for images accessed through the following sources:

    * [`product.featured_media`](https://shopify.dev/docs/api/liquid/objects/product#product-featured_media)
    * [`product.media`](https://shopify.dev/docs/api/liquid/objects/product#product-media)
    * [`file_reference` type metafields](https://shopify.dev/apps/metafields/types#supported-types)

    If you reference this property on an image from another source, then `nil` is returned.

  * **product\_​id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the product that the image is associated with.

    The `product_id` property is only available for images associated with a product. If you reference this property on an image from another source, then `nil` is returned.

  * **src**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the image.

  * **variants**

    array of [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The product variants that the image is associated with.

    The `variants` property is only available for images accessed through the following sources:

    * [`product.featured_image`](https://shopify.dev/docs/api/liquid/objects#product-featured_image)
    * [`product.images`](https://shopify.dev/docs/api/liquid/objects/product#product-images)

    If you reference this property on an image from another source, then `nil` is returned.

  * **width**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The width of the image in pixels.

##### Example

```json
{
  "alt": "Charcoal",
  "aspect_ratio": 1.5001681802892701,
  "attached_to_variant?": false,
  "height": 2973,
  "id": 29355706875969,
  "position": 1,
  "product_id": 6790277595201,
  "src": {},
  "variants": [],
  "width": 4460
}
```

### Referencing the `image` object directly

When an `image` object is referenced directly, the image's relative URL path is returned.

##### Code

```liquid
{{ product.featured_image }}
```

##### Data

```json
{
  "product": {
    "featured_image": "products/mushrooms-on-a-table.jpg"
  }
}
```

##### Output

```html
products/mushrooms-on-a-table.jpg
```

</page>

<page>
---
title: 'Liquid objects: image_presentation'
description: The presentation settings for an image.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/image_presentation'
  md: 'https://shopify.dev/docs/api/liquid/objects/image_presentation.md'
---

# image\_​presentation

The presentation settings for an image.

## Properties

* * **focal\_​point**

    [focal\_point](https://shopify.dev/docs/api/liquid/objects/focal_point)

  * The focal point for the image.

### Returned by

* [image.presentation](https://shopify.dev/docs/api/liquid/objects/image#image-presentation)

</page>

<page>
---
title: 'Liquid objects: images'
description: >-
  All of the [images](/docs/api/liquid/objects/image) that have been
  [uploaded](https://help.shopify.com/manual/online-store/images/theme-images#upload-images)

  to a store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/images'
  md: 'https://shopify.dev/docs/api/liquid/objects/images.md'
---

# images

All of the [images](https://shopify.dev/docs/api/liquid/objects/image) that have been [uploaded](https://help.shopify.com/manual/online-store/images/theme-images#upload-images) to a store.

### Directly accessible in

* Global

You can access images from the `images` array by their filename.

##### Code

```liquid
{{ images['potions-header.png'] | image_url: width: 300 | image_tag }}
```

##### Output

```html
<img src="//polinas-potent-potions.myshopify.com/cdn/shop/files/potions-header.png?v=1650325393&amp;width=300" alt="" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/files/potions-header.png?v=1650325393&amp;width=300 300w" width="300" height="173" style="object-position:1.9231% 9.7917%;">
```

</page>

<page>
---
title: 'Liquid objects: instructions'
description: The instructions for a nested cart line item.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/instructions'
  md: 'https://shopify.dev/docs/api/liquid/objects/instructions.md'
---

# instructions

The instructions for a nested cart line item.

## Properties

* * **can\_​remove**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Whether the nested cart line item can be removed.

  * **can\_​update\_​quantity**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Whether the nested cart line item quantity can be updated.

### Returned by

* [line\_​item.instructions](https://shopify.dev/docs/api/liquid/objects/line_item#line_item-instructions)

</page>

<page>
---
title: 'Liquid objects: line_item'
description: >-
  A line in a cart, checkout, or order. Each line item represents a product
  variant.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/line_item'
  md: 'https://shopify.dev/docs/api/liquid/objects/line_item.md'
---

# line\_​item

A line in a cart, checkout, or order. Each line item represents a product variant.

## Properties

* * **discount\_​allocations**

    array of [discount\_allocation](https://shopify.dev/docs/api/liquid/objects/discount_allocation)

  * The discount allocations that apply to the line item.

    **Caution:** Not applicable for item component as discounts are applied to the parent line item.

  * **error\_​message**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * An informational error message about the status of the line item in the buyer's chosen language.

    **Note:** This field is applicable for cart line item only and currently available for shops using Checkout Extensibility.

  * **final\_​line\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The combined price, in the currency's subunit, of all of the items in the line item. This includes any line-level discounts.

    The value is equal to `line_item.final_price` multiplied by `line_item.quantity`. It's output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **final\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The price of the line item in the currency's subunit. This includes any line-level discounts.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **fulfillment**

    [fulfillment](https://shopify.dev/docs/api/liquid/objects/fulfillment)

  * The fulfillment of the line item.

  * **fulfillment\_​service**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [fulfillment service](https://help.shopify.com/manual/shipping/understanding-shipping/dropshipping-and-fulfillment-services) for the vartiant associated with the line item. If there's no fulfillment service, then `manual` is returned.

  * **gift\_​card**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the product associated with the line item is a gift card. Returns `false` if not.

  * **grams**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The weight of the line item in the store's [default weight unit](https://help.shopify.com/manual/intro-to-shopify/initial-setup/setup-business-settings#set-or-change-your-stores-default-weight-unit).

    **Tip:** Use this property with the \<a href="/docs/api/liquid/filters/weight\_with\_unit">\<code>\<span class="PreventFireFoxApplyingGapToWBR">weight\<wbr/>\_with\<wbr/>\_unit\</span>\</code> filter\</a> to format the weight.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the line item.

    The ID differs depending on the context. The following table outlines the possible contexts and their associated values:

    | Context | Value |
    | - | - |
    | [`cart.items`](https://shopify.dev/docs/api/liquid/objects/cart#cart-items) | The ID of the line item's variant.\<br>\<br>This ID isn't unique, and can be shared by multiple items with the same variant. |
    | [`checkout.line_items`](https://shopify.dev/docs/api/liquid/objects/checkout#checkout-line_items) | A temporary unique hash generated for the checkout. |
    | [`order.line_items`](https://shopify.dev/docs/api/liquid/objects/order#order-line_items) | A unique integer ID. |

  * **image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The image of the line item.

    The image can come from one of the following sources:

    * The image of the variant associated with the line item
    * The featured image of the product associated with the line item, if there's no variant image

  * **instructions**

    [instructions](https://shopify.dev/docs/api/liquid/objects/instructions)

  * Instructions define behaviours and operations that can be performed on the nested cart line.

    **Note:** This field is applicable for cart line item only.

  * **item\_​components**

    array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

  * The components of a line item.

    **Note:** This field is applicable for cart line item only.

  * **key**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The key of the line item.

    Line item keys are unique identifiers that consist of the following components separated by a colon:

    * The ID of the variant associated with the line item
    * A hash of unique characteristics of the line item.

    Note: Line item keys are not stable identifiers. The line item key will change as characteristics of the line item change. This includes, but is not limited to, properties and discount applications.

  * **line\_​level\_​discount\_​allocations**

    array of [discount\_allocation](https://shopify.dev/docs/api/liquid/objects/discount_allocation)

  * The discount allocations that apply directly to the line item.

    **Caution:** Not applicable for item component as discounts are applied to the parent line item.

  * **line\_​level\_​total\_​discount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total amount of any discounts applied to the line item in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **message**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Information about the discounts that have affected the line item.

    The following table outlines what's returned depending on the number of discounts affecting the line item:

    | Number of discounts | Value |
    | - | - |
    | 0 | `nil` |
    | 1 | The [title](https://shopify.dev/docs/api/liquid/objects/discount_application#discount_application-title) of the discount. |
    | More than 1 | A Shopify generated string noting how many discounts have been applied. |

  * **options\_​with\_​values**

  * The name and value pairs for each option of the variant associated with the line item.

    **Note:** The array is never empty because variants with no options still have a default option. Because of this, you should use \<code>\<span class="PreventFireFoxApplyingGapToWBR">line\<wbr/>\_item.product.has\<wbr/>\_only\<wbr/>\_default\<wbr/>\_variant\</span>\</code> to check whether there\&#39;s any information to output.

    ExampleOutput the option values

    ##### Code

    ```liquid
    {% for item in cart.items %}
    <div class="cart__item">
      <p class="cart__item-title">
        {{ item.title }}
      </p>

      {%- unless item.product.has_only_default_variant %}
      <ul>
        {% for option in item.options_with_values -%}
        <li>{{ option.name }}: {{ option.value }}</li>
        {%- endfor %}
      </ul>
      {% endunless %}
    </div>
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "cart": {
        "items": [
          {
            "product": {
              "has_only_default_variant": true
            },
            "title": "Whole bloodroot"
          },
          {
            "product": {
              "has_only_default_variant": true
            },
            "title": "Viper venom"
          }
        ]
      }
    }
    ```

    ##### Output

    ```html
    <div class="cart__item">
      <p class="cart__item-title">
        Whole bloodroot
      </p>
    </div>

    <div class="cart__item">
      <p class="cart__item-title">
        Viper venom
      </p>
    </div>
    ```

  * **original\_​line\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The combined price of all of the items in a line item in the currency's subunit, before any discounts have been applied.

    The value is equal to `line_item.original_price` multiplied by `line_item.quantity`. It's output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **original\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The price of the line item in the currency's subunit, before discounts have been applied.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **parent\_​relationship**

    [parent\_relationship](https://shopify.dev/docs/api/liquid/objects/parent_relationship)

  * The parent relationship for a nested line item.

    **Note:** This field is applicable for cart line item only.

  * **product**

    [product](https://shopify.dev/docs/api/liquid/objects/product)

  * The product associated with the line item. May be a regular [product](https://shopify.dev/docs/api/liquid/objects/product) or a [remote product](https://shopify.dev/docs/api/liquid/objects/remote_product).

  * **product\_​id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The [ID](https://shopify.dev/docs/api/liquid/objects/product#product-id) of the line item's product.

  * **properties**

  * The properties of the line item.

    You can add, or allow customers to add, custom information to a line item with line item properties.

    Line item properties consist of a name and value pair. They can be captured with the following methods:

    * [A custom input inside a product form](https://shopify.dev/themes/architecture/templates/product#line-item-properties)
    * [The AJAX Cart API](https://shopify.dev/api/ajax/reference/cart#add-line-item-properties)

    **Tip:** To learn about how to display captured properties, refer to \<a href="/themes/architecture/templates/cart#display-line-item-properties">Display line item properties\</a>.

    ExampleCapture line item properties in the product form

    To capture line item properties inside the [product form](https://shopify.dev/docs/api/liquid/tags/form#form-product), you need to include an input, for each property. Each input needs a unique `name` attribute. Use the following format:

    ```
    name="properties[property-name]"
    ```

    The value of the input is captured as the value of the property.

    For example, you can use the following code to capture custom engraving text for a product:

    ```liquid
    {% form 'product', product %}
      ...
      <label for="engravingText">Engraving<label>
      <input type="text" id="engravingText" name="properties[Engraving]">
      ...
    {% endform %}
    ```

    ```liquid
    {% form 'product', product %}
      ...
      
    ```

    ***

    **Tip:** You can add an underscore to the beginning of a property name to hide it from customers at checkout. For example, \<code>\<span class="PreventFireFoxApplyingGapToWBR">properties\[\_hidden\<wbr/>Property\<wbr/>Name]\</span>\</code>.

    ***

  * **quantity**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The quantity of the line item.

  * **requires\_​shipping**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant associated with the line item requires shipping. Returns `false` if not.

  * **selling\_​plan\_​allocation**

    [selling\_plan\_allocation](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation)

  * The selling plan allocation of the line item. If the line item doesn't have a selling plan allocation, then `nil` is returned.

    #### Availability of selling plan information

    The following properties aren't available when referencing selling plan information through an [order's line items](https://shopify.dev/docs/api/liquid/objects/order#order-line_items):

    * [`selling_plan_allocation.compare_at_price`](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation#selling_plan_allocation-compare_at_price)
    * [`selling_plan_allocation.price_adjustments`](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation#selling_plan_allocation-price_adjustments)
    * [`selling_plan_allocation.selling_plan.group_id`](https://shopify.dev/docs/api/liquid/objects/selling_plan#selling_plan-group_id)
    * [`selling_plan_allocation.selling_plan.options`](https://shopify.dev/docs/api/liquid/objects/selling_plan#selling_plan-options)
    * [`selling_plan_allocation.selling_plan.price_adjustments`](https://shopify.dev/docs/api/liquid/objects/selling_plan#selling_plan-price_adjustments)
    * [`selling_plan_allocation.selling_plan_group_id`](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation#selling_plan_allocation-selling_plan_group_id)

    **Tip:** If you need to show selling plan information post-purchase, then you should use \<a href="/docs/api/liquid/objects/selling\_plan#selling\_plan-name">\<code>\<span class="PreventFireFoxApplyingGapToWBR">selling\<wbr/>\_plan.name\</span>\</code>\</a>.

  * **sku**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [sku](https://shopify.dev/docs/api/liquid/objects/variant#variant-sku) of the variant associated with the line item.

  * **successfully\_​fulfilled\_​quantity**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of items from the line item that have been successfully fulfilled.

  * **tax\_​lines**

    array of [tax\_line](https://shopify.dev/docs/api/liquid/objects/tax_line)

  * The tax lines for the line item.

  * **taxable**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if taxes should be charged on the line item. Returns `false` if not.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the line item. The title is a combination of `line_item.product.title` and `line_item.variant.title`, separated by a hyphen.

    In most contexts, the line item title appears in the customer's preferred language. However, in the context of an [order](https://shopify.dev/docs/api/liquid/objects/order), the line item title appears in the language that the customer checked out in. The title can receive an override value from the [Cart Transform API](https://shopify.dev/docs/api/functions/reference/cart-transform#showing-overrides). Overrides take precedence over translations.

    #### Line item title history

    When referencing line item, product, and variant titles in the context of an order, the following changes might result in a different output than you expect:

    * A product or variant being deleted
    * A product or variant title being edited

    When `line_item.title` is referenced for an order, the line item title at the time of the order is returned. However, when `line_item.product.title` and `line_item.variant.title` are referenced, the current value for each title is returned.

  * **unit\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The [unit price](https://help.shopify.com/manual/products/details/product-pricing/unit-pricing#add-unit-prices-to-your-product) of the line item in the currency's subunit.

    The price reflects any discounts that are applied to the line item. The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/unit\_price\_with\_measurement">\<code>\<span class="PreventFireFoxApplyingGapToWBR">unit\<wbr/>\_price\<wbr/>\_with\<wbr/>\_measurement\</span>\</code> filter\</a> with this property and the \<code>\<span class="PreventFireFoxApplyingGapToWBR">line\<wbr/>\_item.unit\<wbr/>\_price\<wbr/>\_measurement\</span>\</code> property to output a formatted unit price with measurement.

    To learn about how to display unit prices in your theme, refer to [Unit pricing](https://shopify.dev/themes/pricing-payments/unit-pricing).

  * **unit\_​price\_​measurement**

    [unit\_price\_measurement](https://shopify.dev/docs/api/liquid/objects/unit_price_measurement)

  * The unit price measurement of the line item.

    To learn about how to display unit prices in your theme, refer to [Unit pricing](https://shopify.dev/themes/pricing-payments/unit-pricing).

    **Tip:** Use the \<a href="/docs/api/liquid/filters/unit\_price\_with\_measurement">\<code>\<span class="PreventFireFoxApplyingGapToWBR">unit\<wbr/>\_price\<wbr/>\_with\<wbr/>\_measurement\</span>\</code> filter\</a> with the \<code>\<span class="PreventFireFoxApplyingGapToWBR">line\<wbr/>\_item.unit\<wbr/>\_price\</span>\</code> property and this property to output a formatted unit price with measurement.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the variant associated with the line item.

  * **url\_​to\_​remove**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A URL to remove the line item from the cart.

    **Tip:** To learn more about how to use this property in your theme, refer to \<a href="/themes/architecture/templates/cart#remove-line-items-from-the-cart">Remove line items from the cart\</a>.

  * **variant**

    [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The variant associated with the line item.

  * **variant\_​id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The [ID](https://shopify.dev/docs/api/liquid/objects/variant#variant-id) of the line item's variant.

  * **vendor**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The vendor of the variant associated with the line item.

## Deprecated Properties

* * **discounts**

    array of [discount](https://shopify.dev/docs/api/liquid/objects/discount)

    Deprecated

  * The discounts applied to the line item.

    **Deprecated:**

    Deprecated because not all discount types and details are available.

    The `line_item.discounts` property has been replaced by [`line_item.discount_allocations`](https://shopify.dev/docs/api/liquid/objects/line_item#line_item-discount_allocations).

  * **line\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

    Deprecated

  * The combined price, in the currency's subunit, of all of the items in a line item. This includes any discounts from [Shopify Scripts](https://help.shopify.com/manual/checkout-settings/script-editor).

    The value is equal to `line_item.price` multiplied by `line_item.quantity`. It's output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

    **Deprecated:**

    Deprecated because discounts from automatic discounts and discount codes aren't included.

    The `line_item.line_price` property has been replaced by [`line_item.final_line_price`](https://shopify.dev/docs/api/liquid/objects/line_item#line_item-final_line_price).

  * **price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

    Deprecated

  * The price of the line item in the currency's subunit. This includes any discounts from [Shopify Scripts](https://help.shopify.com/manual/checkout-settings/script-editor).

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

    **Deprecated:**

    Deprecated because discounts from automatic discounts and discount codes aren't included.

    The `line_item.price` property has been replaced by [`line_item.final_price`](https://shopify.dev/docs/api/liquid/objects/line_item#line_item-final_price).

  * **total\_​discount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

    Deprecated

  * The total amount, in the currency's subunit, of any discounts applied to the line item by [Shopify Scripts](https://help.shopify.com/manual/checkout-settings/script-editor).

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

    **Deprecated:**

    Deprecated because discounts from automatic discounts and discount codes aren't included.

    The `line_item.total_discount` property has been replaced by [`line_item.line_level_total_discount`](https://shopify.dev/docs/api/liquid/objects/line_item#line_item-line_level_total_discount).

##### Example

```json
{
  "discount_allocations": [],
  "discounts": [],
  "error_message": "",
  "final_line_price": "74.97",
  "final_price": "24.99",
  "fulfillment": {},
  "fulfillment_service": "manual",
  "gift_card": false,
  "grams": 0,
  "id": 10974183882817,
  "image": {},
  "instructions": null,
  "item_components": null,
  "key": 10974183882817,
  "line_level_discount_allocations": [],
  "line_level_total_discount": "0.00",
  "line_price": "74.97",
  "message": "",
  "options_with_values": [
    {
      "name": "Title",
      "value": "Default Title"
    }
  ],
  "original_line_price": "74.97",
  "original_price": "24.99",
  "parent_relationship": null,
  "price": "24.99",
  "product": {},
  "product_id": 6792596455489,
  "properties": {},
  "quantity": 3,
  "requires_shipping": true,
  "selling_plan_allocation": null,
  "sku": "",
  "successfully_fulfilled_quantity": 2,
  "tax_lines": [],
  "taxable": true,
  "title": "Bloodroot (whole)",
  "total_discount": "0.00",
  "unit_price": "49.98",
  "unit_price_measurement": {
    "measured_type": "weight",
    "quantity_value": "500.0",
    "quantity_unit": "g",
    "reference_value": 1,
    "reference_unit": "kg"
  },
  "url": {},
  "url_to_remove": null,
  "variant": {},
  "variant_id": 39888235757633,
  "vendor": "Clover's Apothecary"
}
```

</page>

<page>
---
title: 'Liquid objects: link'
description: >-
  A link in a
  [menu](https://help.shopify.com/manual/online-store/menus-and-links/drop-down-menus).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/link'
  md: 'https://shopify.dev/docs/api/liquid/objects/link.md'
---

# link

A link in a [menu](https://help.shopify.com/manual/online-store/menus-and-links/drop-down-menus).

To learn about how to implement navigation in a theme, refer to [Add navigation to your theme](https://shopify.dev/themes/navigation-search/navigation).

## Properties

* * **active**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the link is active. Returns `false` if not.

    A link is considered to be active if the current URL path matches, or contains, the link's [url](https://shopify.dev/docs/api/liquid/objects/link#link-url). For example, if the current URL path is `/blog/potion-notions/new-potions-for-spring`, then the following link URLs would be considered active:

    * `/blog/potion-notions/new-potions-for-spring`
    * `/blog/potion-notions`

    **Tip:** The \<code>link.active\</code> property is useful for menu designs that highlight when top-level navigation categories are being viewed. For example, if a customer is viewing an article from the \&quot;Potion notions\&quot; blog, then the \&quot;Potion notions\&quot; link is highlighted in the menu.

  * **child\_​active**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if a link's child link is active. Returns `false` if not.

    A link is considered to be active if the current URL path matches, or contains, the [URL](https://shopify.dev/docs/api/liquid/objects/link#link-url) of the link.

    For example, if the current URL path is `/blog/potion-notions/new-potions-for-spring`, then the following link URLs would be considered active:

    * `/blog/potion-notions/new-potions-for-spring`
    * `/blog/potion-notions`

  * **child\_​current**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if current URL path matches a link's child link [URL](https://shopify.dev/docs/api/liquid/objects/link#link-url). Returns `false` if not.

    **Note:** URL parameters are ignored when determining a match.\</p> \<p>Product URLs \<a href="/docs/api/liquid/filters/within">within the context of a collection\</a> and standard product URLs are treated the same.

  * **current**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current URL path matches the [URL](https://shopify.dev/docs/api/liquid/objects/link#link-url) of the link. Returns `false` if not.

    **Note:** URL parameters are ignored when determining a match.\</p> \<p>Product URLs \<a href="/docs/api/liquid/filters/within">within the context of a collection\</a> are treated as equal to a standard product URL for the same product.

  * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the link.

  * **levels**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of nested levels under the link.

  * **links**

    array of [link](https://shopify.dev/docs/api/liquid/objects/link)

  * The child links of the link.

    ExampleCheck the number of links

    ##### Code

    ```liquid
    {% for link in linklists.main-menu.links -%}
      {% if link.links.size > 0 -%}
        - {{ link.title }} ({{ link.links.size }} children)<br>
      {%- else -%}
        - {{ link.title }}<br>
      {%- endif %}
    {%- endfor %}
    ```

    ##### Data

    ```json
    {
      "linklists": {
        "main-menu": {
          "links": [
            "LinkDrop",
            "LinkDrop",
            "LinkDrop"
          ]
        }
      }
    }
    ```

    ##### Output

    ```html
    - Home<br>
    - Catalog (2 children)<br>
    - Contact<br>
    ```

  * **object**

  * The object associated with the link.

    The object can be one of the following:

    * [`article`](https://shopify.dev/docs/api/liquid/objects/article)
    * [`blog`](https://shopify.dev/docs/api/liquid/objects/blog)
    * [`collection`](https://shopify.dev/docs/api/liquid/objects/collection)
    * [`metaobject`](docs/api/liquid/objects/metaobject)
    * [`page`](https://shopify.dev/docs/api/liquid/objects/page)
    * [`policy`](https://shopify.dev/docs/api/liquid/objects/policy)
    * [`product`](https://shopify.dev/docs/api/liquid/objects/product)

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the link.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of the link.

    | Possible values | Description |
    | - | - |
    | article\_link | The link points to an article. |
    | blog\_link | The link points to an blog. |
    | catalog\_link | The link points to the [catalog page](https://help.shopify.com/manual/online-store/themes/customizing-themes/change-catalog-page). |
    | collection\_link | The link points to a collection. |
    | collections\_link | The link points to the [collection list page](https://shopify.dev/themes/architecture/templates/list-collections). |
    | customer\_account\_page\_link | The link points to a [customer account page](https://shopify.dev/docs/apps/build/customer-accounts/full-page-extensions). |
    | frontpage\_link | The link points to the home page. |
    | http\_link | The link points to an external web page, or a product type or vendor collection. |
    | metaobject\_link | The link points to a metaobject page. |
    | page\_link | The link points to a [page](https://help.shopify.com/manual/online-store/themes/theme-structure/pages). |
    | policy\_link | The link points to a [policy page](https://help.shopify.com/manual/checkout-settings/refund-privacy-tos#add-links-to-your-policies-within-pages-or-on-social-media). |
    | product\_link | The link points to a product page. |
    | search\_link | The link points to the search page. |

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL of the link.

##### Example

```json
{
  "active": false,
  "child_active": false,
  "child_current": false,
  "current": false,
  "handle": {},
  "levels": 0,
  "links": [],
  "object": {},
  "title": {},
  "type": "page_link",
  "url": "/pages/contact"
}
```

</page>

<page>
---
title: 'Liquid objects: linklist'
description: >-
  A
  [menu](https://help.shopify.com/manual/online-store/menus-and-links/drop-down-menus)
  in a store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/linklist'
  md: 'https://shopify.dev/docs/api/liquid/objects/linklist.md'
---

# linklist

A [menu](https://help.shopify.com/manual/online-store/menus-and-links/drop-down-menus) in a store.

To learn about how to implement navigation in a theme, refer to [Add navigation to your theme](https://shopify.dev/themes/navigation-search/navigation).

## Properties

* * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the menu.

  * **levels**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of nested levels in the menu.

    **Note:** There\&#39;s a maximum of 3 levels.

  * **links**

    array of [link](https://shopify.dev/docs/api/liquid/objects/link)

  * The links in the menu.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the menu.

##### Example

```json
{
  "handle": "main-menu",
  "levels": 2,
  "links": [],
  "title": "Main menu"
}
```

</page>

<page>
---
title: 'Liquid objects: linklists'
description: >-
  All of the
  [menus](https://help.shopify.com/manual/online-store/menus-and-links/drop-down-menus)
  in a store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/linklists'
  md: 'https://shopify.dev/docs/api/liquid/objects/linklists.md'
---

# linklists

All of the [menus](https://help.shopify.com/manual/online-store/menus-and-links/drop-down-menus) in a store.

### Directly accessible in

* Global

You can access a specific menu through the `linklists` object using the menu's [handle](https://shopify.dev/docs/api/liquid/basics#handles).

##### Code

```liquid
<!-- Main menu -->
{% for link in linklists.main-menu.links -%}
  {{ link.title | link_to: link.url }}
{%- endfor %}

<!-- Footer menu -->
{% for link in linklists['footer'].links -%}
  {{ link.title | link_to: link.url }}
{%- endfor %}
```

##### Data

```json
{
  "linklists": {
    "footer": {
      "links": [
        "LinkDrop"
      ]
    },
    "main-menu": {
      "links": [
        "LinkDrop",
        "LinkDrop",
        "LinkDrop"
      ]
    }
  }
}
```

##### Output

```html
<!-- Main menu -->
<a href="/" title="">Home</a>
<a href="/collections/all" title="">Catalog</a>
<a href="/pages/contact" title="">Contact</a>


<!-- Footer menu -->
<a href="/search" title="">Search</a>
```

</page>

<page>
---
title: 'Liquid objects: localization'
description: Information about the countries and languages that are available on a store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/localization'
  md: 'https://shopify.dev/docs/api/liquid/objects/localization.md'
---

# localization

Information about the countries and languages that are available on a store.

The `localization` object can be used in a [localization form](https://shopify.dev/docs/api/liquid/tags/form#form-localization).

To learn about how to offer localization options in your theme, refer to [Support multiple currencies and languages](https://shopify.dev/themes/internationalization/multiple-currencies-languages).

## Properties

* * **available\_​countries**

    array of [country](https://shopify.dev/docs/api/liquid/objects/country)

  * The countries that are available on the store.

  * **available\_​languages**

    array of [shop\_locale](https://shopify.dev/docs/api/liquid/objects/shop_locale)

  * The languages that are available on the store.

  * **country**

    [country](https://shopify.dev/docs/api/liquid/objects/country)

  * The currently selected country on the storefront.

  * **language**

    [shop\_locale](https://shopify.dev/docs/api/liquid/objects/shop_locale)

  * The currently selected language on the storefront.

  * **market**

    [market](https://shopify.dev/docs/api/liquid/objects/market)

  * The currently selected market on the storefront.

##### Example

```json
{
  "available_countries": [],
  "available_languages": [],
  "country": {},
  "language": {},
  "market": {}
}
```

</page>

<page>
---
title: 'Liquid objects: location'
description: 'A store [location](https://help.shopify.com/manual/locations).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/location'
  md: 'https://shopify.dev/docs/api/liquid/objects/location.md'
---

# location

A store [location](https://help.shopify.com/manual/locations).

***

**Tip:** The \<code>location\</code> object is defined only if one or more locations has \<a href="https://help.shopify.com/manual/shipping/setting-up-and-managing-your-shipping/local-methods/local-pickup">local pickup\</a> enabled.

***

## Properties

* * **address**

    [address](https://shopify.dev/docs/api/liquid/objects/address)

  * The location's address.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The location's ID.

  * **latitude**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The latitude of the location's address.

    If the location's address isn't verified, then `nil` is returned.

  * **longitude**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The longitude of the location's address.

    If the location's address isn't verified, then `nil` is returned.

  * **metafields**

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the location.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The location's name.

##### Example

```json
{
  "address": {},
  "id": 62002462785,
  "latitude": 43.6556377,
  "longitude": -79.38681079999999,
  "metafields": {},
  "name": "123 Edward Street"
}
```

</page>

<page>
---
title: 'Liquid objects: market'
description: >-
  A group of one or more regions of the world that a merchant is targeting for
  sales.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/market'
  md: 'https://shopify.dev/docs/api/liquid/objects/market.md'
---

# market

A group of one or more regions of the world that a merchant is targeting for sales.

To learn more about markets, refer to [Shopify Markets](https://shopify.dev/docs/apps/markets). To make sure that visitors interact with the optimal version of a store using Shopify Markets, refer to [Detect and set a visitor's optimal localization](https://shopify.dev/docs/themes/markets/localization-discovery).

## Properties

* * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the market.

  * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the market.

  * **metafields**

    array of [metafield](https://shopify.dev/docs/api/liquid/objects/metafield)

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the market.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

##### Example

```json
{
  "handle": "ca",
  "id": 6157828161,
  "metafields": {}
}
```

</page>

<page>
---
title: 'Liquid objects: measurement'
description: |-
  A measurement from one of the following metafield types:

  - `dimension`
  - `volume`
  - `weight`
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/measurement'
  md: 'https://shopify.dev/docs/api/liquid/objects/measurement.md'
---

# measurement

A measurement from one of the following metafield types:

* `dimension`
* `volume`
* `weight`

***

**Tip:** To learn about metafield types, refer to \<a href="/apps/metafields/types">Metafield types\</a>.

***

## Properties

* * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The measurement type.

    | Possible values |
    | - |
    | dimension |
    | volume |
    | weight |

  * **unit**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The measurement unit.

  * **value**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The measurement value.

##### Example

```json
{
  "type": "volume",
  "unit": "mL",
  "value": "500.0"
}
```

</page>

<page>
---
title: 'Liquid objects: media'
description: |-
  An abstract media object that can represent the following object types:

  - [`image`](/docs/api/liquid/objects/image)
  - [`model`](/docs/api/liquid/objects/model)
  - [`video`](/docs/api/liquid/objects/video)
  - [`external_video`](/docs/api/liquid/objects/external_video)
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/media'
  md: 'https://shopify.dev/docs/api/liquid/objects/media.md'
---

# media

An abstract media object that can represent the following object types:

* [`image`](https://shopify.dev/docs/api/liquid/objects/image)
* [`model`](https://shopify.dev/docs/api/liquid/objects/model)
* [`video`](https://shopify.dev/docs/api/liquid/objects/video)
* [`external_video`](https://shopify.dev/docs/api/liquid/objects/external_video)

The `media` object can be returned by the [`product.media` array](https://shopify.dev/docs/api/liquid/objects/product#product-media) or a [`file_reference` metafield](https://shopify.dev/apps/metafields/types).

You can use [media filters](https://shopify.dev/docs/api/liquid/filters/media-filters) to generate URLs and media displays. To learn about how to use media in your theme, refer to [Support product media](https://shopify.dev/themes/product-merchandising/media/support-media).

***

**Note:** Each media type has unique properties in addition to the general \<code>media\</code> properties. To learn about these additional properties, refer to the reference for each type.

***

## Properties

* * **alt**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The alt text of the media.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the media.

  * **media\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The media type.

    | Possible values |
    | - |
    | image |
    | model |
    | video |
    | external\_video |

    ExampleFilter for media of a specific type

    You can use the `media_type` property with the [`where` filter](https://shopify.dev/docs/api/liquid/filters/where) to filter the [`product.media` array](https://shopify.dev/docs/api/liquid/objects/product#product-media) for all media of a desired type.

    ##### Code

    ```liquid
    {% assign images = product.media | where: 'media_type', 'image' %}

    {% for image in images %}
      {{- image | image_url: width: 300 | image_tag }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "product": {
        "media": [
          "products/oil-dripping-into-jar.jpg"
        ]
      }
    }
    ```

    ##### Output

    ```html
    <img src="//polinas-potent-potions.myshopify.com/cdn/shop/products/oil-dripping-into-jar.jpg?v=1650399519&amp;width=300" alt="Viper venom" srcset="//polinas-potent-potions.myshopify.com/cdn/shop/products/oil-dripping-into-jar.jpg?v=1650399519&amp;width=300 300w" width="300" height="200">
    ```

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The position of the media in the [`product.media` array](https://shopify.dev/docs/api/liquid/objects/product#product-media).

    If the source is a [`file_reference` metafield](https://shopify.dev/apps/metafields/types), then `nil` is returned.

  * **preview\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * A preview image of the media.

    **Note:** Preview images don\&#39;t have an ID attribute.

##### Example

```json
{
  "alt": "Dandelion milk",
  "id": 21772527435841,
  "media_type": "image",
  "position": 1,
  "preview_image": {}
}
```

</page>

<page>
---
title: 'Liquid objects: metafield'
description: 'A [metafield](/apps/metafields) attached to a parent object.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/metafield'
  md: 'https://shopify.dev/docs/api/liquid/objects/metafield.md'
---

# metafield

A [metafield](https://shopify.dev/apps/metafields) attached to a parent object.

To learn about how to access a metafield on a specific object, refer to [Access metafields](https://shopify.dev/docs/api/liquid/objects/metafield#metafield-access-metafields).

Metafields support [multiple data types](https://shopify.dev/apps/metafields/types), which determine the kind of information that's stored in the metafield. You can also output the metafield content in a type-specific format using [metafield filters](https://shopify.dev/docs/api/liquid/filters/metafield-filters).

***

**Note:** You can\&#39;t create metafields in Liquid. Metafields can be created in only the following ways:\</p> \<ul> \<li>\<a href="https://help.shopify.com/manual/metafields">In the Shopify admin\</a>\</li> \<li>\<a href="https://shopify.dev/apps/metafields">Through an app\</a>\</li> \</ul>

***

***

**Note:** Metafields of type \<code>integer\</code>, \<code>\<span class="PreventFireFoxApplyingGapToWBR">json\<wbr/>\_string\</span>\</code>, and \<code>string\</code> are older implementations that don\&#39;t have the properties noted on this page, and aren\&#39;t compatible with metafield filters. To learn more, refer to \<a href="/docs/api/liquid/objects/metafield#metafield-deprecated-metafields">Deprecated metafields\</a>.

***

## Properties

* * **list?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the metafield is a list type. Returns `false` if not.

    **Tip:** To learn about metafield types, refer to \<a href="/apps/metafields/types">Metafield types\</a>.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The [type](https://shopify.dev/apps/metafields/types) of the metafield.

    | Possible values |
    | - |
    | single\_line\_text\_field |
    | multi\_line\_text\_field |
    | rich\_text\_field |
    | product\_reference |
    | collection\_reference |
    | variant\_reference |
    | page\_reference |
    | file\_reference |
    | number\_integer |
    | number\_decimal |
    | date |
    | date\_time |
    | url\_reference |
    | json |
    | boolean |
    | color |
    | weight |
    | volume |
    | dimension |
    | rating |
    | money |

  * **value**

  * The value of the metafield.

    The following table outlines the value format for each metafield type:

    | Type | Returned format |
    | - | - |
    | `single_line_text_field` `multi_line_text_field` | [A string](https://shopify.dev/docs/api/liquid/basics#string) |
    | `rich_text_field` | A field that supports headings, lists, links, bold, and italics |
    | `product_reference` | [A product object](https://shopify.dev/docs/api/liquid/objects/product) |
    | `collection_reference` | [A collection object](https://shopify.dev/docs/api/liquid/objects/collection) |
    | `variant_reference` | [A variant object](https://shopify.dev/docs/api/liquid/objects/variant) |
    | `page_reference` | [A page object](https://shopify.dev/docs/api/liquid/objects/page) |
    | `file_reference` | [A generic\_file object](https://shopify.dev/docs/api/liquid/objects/generic-file) [A media object (images and videos only)](https://shopify.dev/docs/api/liquid/objects/media) |
    | `number_integer` `number_decimal` | [A number](https://shopify.dev/docs/api/liquid/basics#number) |
    | `date` `date_time` | A date string. To format the string, use the [date](https://shopify.dev/docs/api/liquid/filters/date) filter. |
    | `url_reference` | [A url string](https://shopify.dev/docs/api/liquid/basics#string) |
    | `json` | [A JSON object](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON) |
    | `boolean` | [A boolean](https://shopify.dev/docs/api/liquid/basics#boolean) |
    | `color` | [A color object](https://shopify.dev/docs/api/liquid/objects/color) |
    | `weight` `volume` `dimension` | [A measurement object](https://shopify.dev/docs/api/liquid/objects/measurement) |
    | `rating` | [A rating object](https://shopify.dev/docs/api/liquid/objects/rating) |
    | `money` | [A money object, displayed in the customer's local (presentment) currency.](https://shopify.dev/docs/api/liquid/objects/money) |

##### Example

```json
{
  "list?": false,
  "type": "single_line_text_field",
  "value": "Take with a meal."
}
```

### Access metafields

The access path for metafields consists of two layers:

* namespace - A grouping of metafields to prevent conflicts.
* key - The metafield name.

Given this, you can access the metafield object with the following syntax:

```liquid
{{ resource.metafields.namespace.key }}
```

```liquid
{{ resource.metafields.namespace.key }}
```

##### Code

```liquid
Type: {{ product.metafields.information.directions.type }}
Value: {{ product.metafields.information.directions.value }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
Type: single_line_text_field
Value: Take with a meal.
```

### Accessing metafields of type `json`

The `value` property of metafields of type `json` returns a [JSON object](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON). You can access the properties of this object directly in Liquid, either by name or 0-based index. You can also iterate through the properties.

##### Code

```liquid
Temperature: {{ product.metafields.information.burn_temperature.value.temperature }}
Unit: {{ product.metafields.information.burn_temperature.value['unit'] }}

{% for property in product.metafields.information.burn_temperature.value -%}
  {{ property.first | capitalize }}: {{ property.last }}
{%- endfor %}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
Temperature: 700
Unit: degrees

Temperature: 700
Unit: degrees
Scale: Fahrenheit
```

### Accessing metafields of type `list`

The `value` property of metafields of type `list` returns an array. You can iterate through the array to access the values.

##### Code

```liquid
{% for item in product.metafields.information.combine_with.value -%}
  {{ item.product.title }}
{%- endfor %}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
Blue Mountain Flower
Charcoal
```

If the list is of type `single_line_text_field`, then you can access the items in the array directly in Liquid using a 0-based index.

##### Code

```liquid
First item in list: {{ product.metafields.information.pickup_locations.value[0] }}
Last item in list: {{ product.metafields.information.pickup_locations.value.last }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
First item in list: Ottawa
Last item in list: Vancouver
```

### Determining the length of a list metafield

The way that you determine the length of a list metafield depends on its type:

* **[Reference types](https://shopify.dev/docs/apps/custom-data/metafields/types#reference-types)**: Use the `count` property to determine the list length.
* **Non-reference types**: These lists are rendered as arrays. Use the [`size`](https://shopify.dev/docs/api/liquid/filters/size) filter to determine the number of items in the array.

##### Code

```liquid
# list.product_reference
Number of similar products: {{ product.metafields.information.similar_products.value.count }}

# list.single_line_text_field
Number of pickup locations: {{ product.metafields.information.pickup_locations.value.size }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
# list.product_reference
Number of similar products: 2

# list.single_line_text_field
Number of pickup locations: 4
```

### Deprecated metafields

Deprecated metafields are older metafield types with limited functionality. The following metafield types are deprecated:

* `integer`
* `json_string`
* `string`

These metafield types don't have the same metafield object properties mentioned in the previous sections. Instead, they return the metafield value directly.

The following table outlines the value type for each deprecated metafield type:

| Metafield type | Value type |
| - | - |
| `integer` | [An integer](https://shopify.dev/docs/api/liquid/basics#number) |
| `json_string` | [A JSON object](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON) |
| `string` | [A string](https://shopify.dev/docs/api/liquid/basics#string) |

</page>

<page>
---
title: 'Liquid objects: metaobject'
description: >-
  A metaobject entry, which includes the values for a set of
  [fields](/docs/api/liquid/objects#metafield).

  The set is defined by the parent
  [`metaobject_definition`](/docs/api/liquid/objects#metaobject_definition).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/metaobject'
  md: 'https://shopify.dev/docs/api/liquid/objects/metaobject.md'
---

# metaobject

A metaobject entry, which includes the values for a set of [fields](https://shopify.dev/docs/api/liquid/objects#metafield). The set is defined by the parent [`metaobject_definition`](https://shopify.dev/docs/api/liquid/objects#metaobject_definition).

## Properties

* * **system**

    [metaobject\_system](https://shopify.dev/docs/api/liquid/objects/metaobject_system)

  * Basic information about the metaobject. These properties are grouped under the `system` object to avoid collisions between system property names and user-defined metaobject fields.

### Directly accessible in

* [metaobject](https://shopify.dev/themes/architecture/templates/metaobject)

### Returned by

* [metaobjects](https://shopify.dev/docs/api/liquid/objects/metaobjects)

### Access metaobjects individually

The access path for a metaobject consists of two layers:

* type - The type of the parent metaobject definition.
* handle - The unique [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the metaobject.

Given this, you can access a metaobject with the following syntax:

```liquid
{{ metaobjects.type.handle }}
```

```liquid
{{ metaobjects.type.handle }}
```

You can also use square bracket notation:

```liquid
{{ metaobjects['type']['handle'] }}
```

```liquid
{{ metaobjects['type']['handle'] }}
```

A metaobjects's field values can be accessed using the key of the desired field:

```liquid
{{ metaobjects.testimonials.homepage.title }}
{{ metaobjects['highlights']['washable'].image.value }}
```

```liquid
{{ metaobjects.testimonials.homepage.title }}
{{ metaobjects['highlights']['washable'].image.value }}
```

***

**Note:** When the \<a href="/apps/data-extensions/metaobjects/capabilities">\<code>publishable\</code> capability\</a> is enabled, a metaobject can only be accessed if its status is \<code>active\</code>. If its status is \<code>draft\</code>, then the return value is \<code>nil\</code>.

***

### Usage in metaobject templates

Within a metaobject template, the `metaobject` Liquid object represents the metaobject drop being rendered by the template. You can access it directly as `{{ metaobject }}`.

Here's a basic example of accessing a field within the associated metaobject template:

```liquid
{{ metaobject.title.value }}
```

```liquid
{{ metaobject.title.value }}
```

In this example, replace `title` with the key of the field you want to access. This will output the value of that field for the current metaobject.

## Templates using metaobject

[Theme architecture](https://shopify.dev/themes/architecture/templates/metaobject)

[metaobject template](https://shopify.dev/themes/architecture/templates/metaobject)

</page>

<page>
---
title: 'Liquid objects: metaobject_definition'
description: >-
  A `metaobject_definition` defines the structure of a metaobject type for the
  store, which consists of

  a merchant-defined set of [field
  definitions](https://help.shopify.com/en/manual/metafields/metafield-definitions).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/metaobject_definition'
  md: 'https://shopify.dev/docs/api/liquid/objects/metaobject_definition.md'
---

# metaobject\_​definition

A `metaobject_definition` defines the structure of a metaobject type for the store, which consists of a merchant-defined set of [field definitions](https://help.shopify.com/en/manual/metafields/metafield-definitions).

One or more corresponding [`metaobject`](https://shopify.dev/docs/api/liquid/objects#metaobject) objects contain values for the fields specified in the metaobject definition.

***

**Note:** When looping through metaobjects by accessing them using individual handles (e.g., \<code>\<span class="PreventFireFoxApplyingGapToWBR">metaobjects.T\<wbr/>Y\<wbr/>P\<wbr/>E\[handle]\</span>\</code>), you\&#39;re limited to 20 unique handles per page and can\&#39;t use \<a href="/docs/api/liquid/tags/paginate">pagination\</a>. To iterate over more metaobjects, instead use the \<code>values\</code> property, which supports pagination up to 250 entries per page.

***

## Properties

* * **values**

    array of [metaobject](https://shopify.dev/docs/api/liquid/objects/metaobject)

  * The [metaobjects](https://shopify.dev/docs/api/liquid/objects#metaobject) that follow the definition.

  * **values\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of entries for the metaobject definition.

### Loop over entries of a metaobject definition

If a metaobject definition has multiple metaobject entries, then you can loop over them using the `values` property. You can loop over a maximum of 50 entries in a metaobject definition. For example, you can display the field `author` for each metaobject using the following `forloop`:

```liquid
{% for testimonial in metaobjects.testimonials.values %}
  {{ testimonial.author.value }}
{% endfor %}
```

```liquid
{% for testimonial in metaobjects.testimonials.values %}
  {{ testimonial.author.value }}
{% endfor %}
```

***

**Note:** When the \<a href="/apps/data-extensions/metaobjects/capabilities">\<code>publishable\</code> capability\</a> is enabled, loops return only metaobjects with a status of \<code>active\</code>. Metaobjects with a status of \<code>draft\</code> are skipped.

***

</page>

<page>
---
title: 'Liquid objects: metaobject_system'
description: >-
  Basic information about a [`metaobject`](/api/liquid/objects#metaobject).
  These properties are grouped under the `system` object to avoid collisions
  between system property names and user-defined metaobject fields.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/metaobject_system'
  md: 'https://shopify.dev/docs/api/liquid/objects/metaobject_system.md'
---

# metaobject\_​system

Basic information about a [`metaobject`](https://shopify.dev/api/liquid/objects#metaobject). These properties are grouped under the `system` object to avoid collisions between system property names and user-defined metaobject fields.

## Properties

* * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The unique [handle](https://shopify.dev/api/liquid/basics#handles) of the metaobject.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the metaobject.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The type of the metaobject definition.

    This is a free-form string that's defined when the metaobject definition is created.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the metaobject.

    Only set for metaobjects that have the `online_store` capability.

### Returned by

* [metaobject.system](https://shopify.dev/docs/api/liquid/objects/metaobject#metaobject-system)

### Using the `metaobject_system` object

You can access the `metaobject_system` object and its properties through the metaobject's `system` property. You can use the following syntax:

```liquid
{{ metaobjects.testimonials["home_page"].system.id }}
```

```liquid
{{ metaobjects.testimonials["home_page"].system.id }}
```

You can also access `metaobject_system` properties when iterating over a list of metaobjects:

```liquid
{% for metaobject in product.metafields.custom.mixed_metaobject_list.value %}
  {% if metaobject.system.type == "testimonial" %}
    {% render 'testimonial' with metaobject as testimonial  %}
  {% else %}
    {{ metaobject.system.handle }}
  {% endif %}
{% endfor %}
```

```liquid
{% for metaobject in product.metafields.custom.mixed_metaobject_list.value %}
  {% if metaobject.system.type == "testimonial" %}
    {% render 'testimonial' with metaobject as testimonial  %}
  {% else %}
    {{ metaobject.system.handle }}
  {% endif %}
{% endfor %}
```

</page>

<page>
---
title: 'Liquid objects: metaobjects'
description: 'All of the [metaobjects](/docs/api/liquid/objects/metaobject) of the store.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/metaobjects'
  md: 'https://shopify.dev/docs/api/liquid/objects/metaobjects.md'
---

# metaobjects

All of the [metaobjects](https://shopify.dev/docs/api/liquid/objects/metaobject) of the store.

Individual metaobjects can be accessed by specifying their type and handle. For more information, refer to [Access metaobjects individually](https://shopify.dev/docs/api/liquid/objects#metaobject-access-metaobjects-individually).

Additionally, it is possible to iterate over entries from a metaobject definition. For more information, refer to [Loop over entries of a metaobject definition](https://shopify.dev/docs/api/liquid/objects/metaobject_definition#metaobject_definition-loop-over-entries-of-a-metaobject-definition).

Metaobjects are created in the [Content](https://www.shopify.com/admin/content) page of the Shopify admin.

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: model'
description: A 3D model uploaded as product media.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/model'
  md: 'https://shopify.dev/docs/api/liquid/objects/model.md'
---

# model

A 3D model uploaded as product media.

***

**Tip:** Use the \<a href="/docs/api/liquid/filters/model\_viewer\_tag">\<code>\<span class="PreventFireFoxApplyingGapToWBR">model\<wbr/>\_viewer\<wbr/>\_tag\</span>\</code> filter\</a> to output a \<a href="https://modelviewer.dev">Google model viewer component\</a> for the model.

***

## Properties

* * **alt**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The alt text of the model.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the model.

  * **media\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The media type of the model. Always returns `model`.

    ExampleFilter for media of a specific type

    You can use the `media_type` property with the [`where` filter](https://shopify.dev/docs/api/liquid/filters/where) to filter the [`product.media` array](https://shopify.dev/docs/api/liquid/objects/product#product-media) for all media of a desired type.

    ##### Code

    ```liquid
    {% assign models = product.media | where: 'media_type', 'model' %}

    {% for model in models %}
      {{- model | model_viewer_tag }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "product": {
        "media": [
          {
            "media_type": "model"
          }
        ]
      }
    }
    ```

    ##### Output

    ```html
    <model-viewer src="//polinas-potent-potions.myshopify.com/cdn/shop/3d/models/o/eb9388299ce0557c/WaterBottle.glb?v=0" camera-controls="true" style="--poster-color: transparent;" data-shopify-feature="1.12" alt="Potion bottle" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/WaterBottle_small.jpg?v=1655189057"></model-viewer>
    ```

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The position of the model in the [`product.media`](https://shopify.dev/docs/api/liquid/objects/product#product-media) array.

  * **preview\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * A preview image for the model.

  * **sources**

    array of [model\_source](https://shopify.dev/docs/api/liquid/objects/model_source)

  * The source files for the model.

##### Example

```json
{
  "alt": "Potion bottle",
  "id": 22064203137089,
  "media_type": "model",
  "position": 1,
  "preview_image": {},
  "sources": []
}
```

</page>

<page>
---
title: 'Liquid objects: model_source'
description: A model source file.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/model_source'
  md: 'https://shopify.dev/docs/api/liquid/objects/model_source.md'
---

# model\_​source

A model source file.

## Properties

* * **format**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The format of the model source file.

  * **mime\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) of the model source file.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) of the model source file.

##### Example

```json
{
  "format": "glb",
  "mime_type": "model/gltf-binary",
  "url": "//polinas-potent-potions.myshopify.com/cdn/shop/3d/models/o/eb9388299ce0557c/WaterBottle.glb?v=0"
}
```

</page>

<page>
---
title: 'Liquid objects: money'
description: 'A money value, in the the customer''s local (presentment) currency.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/money'
  md: 'https://shopify.dev/docs/api/liquid/objects/money.md'
---

# money

A money value, in the the customer's local (presentment) currency.

***

**Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

***

## Properties

* * **currency**

    [currency](https://shopify.dev/docs/api/liquid/objects/currency)

  * The customer's local (presentment) currency.

##### Example

```json
{
  "currency": {}
}
```

### Referencing money objects directly

When a money object is referenced directly, the money value in cents is returned.

##### Code

```liquid
{{ product.metafields.details.price_per_100g.value }}
```

##### Data

```json
{
  "product": {
    "metafields": {}
  }
}
```

##### Output

```html
1796
```

</page>

<page>
---
title: 'Liquid objects: order'
description: 'An [order](https://help.shopify.com/manual/orders).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/order'
  md: 'https://shopify.dev/docs/api/liquid/objects/order.md'
---

# order

An [order](https://help.shopify.com/manual/orders).

## Properties

* * **attributes**

  * The attributes on the order.

    If there are no attributes on the order, then `nil` is returned.

    **Tip:** Attributes are \<a href="https://shopify.dev/themes/architecture/templates/cart#support-cart-notes-and-attributes">collected with the cart\</a>.

    ExampleOutput the attributes

    ```liquid
    <ul>
      {% for attribute in order.attributes -%}
        <li><strong>{{ attribute.first }}:</strong> {{ attribute.last }}</li>
      {%- endfor %}
    </ul>
    ```

    ```liquid

      {% for attribute in order.attributes -%}
        
    {{ attribute.first }}: {{ attribute.last }}

      {%- endfor %}
    ```

  * **billing\_​address**

    [address](https://shopify.dev/docs/api/liquid/objects/address)

  * The billing address of the order.

  * **cancel\_​reason**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The reason that the order was cancelled.

    | Possible values | Description |
    | - | - |
    | customer | Customer changed/cancelled order |
    | declined | Payment declined |
    | fraud | Fraudulent order |
    | inventory | Items unavailable |
    | staff | Staff error |
    | other | Other |

  * **cancel\_​reason\_​label**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The localized version of the [cancellation reason](https://shopify.dev/docs/api/liquid/objects/order#order-cancel_reason) for the order.

    **Tip:** Use this property to output the cancellation reason on the storefront.

  * **cancelled**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the order was cancelled. Returns `false` if not.

  * **cancelled\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the order was cancelled.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **cart\_​level\_​discount\_​applications**

    array of [discount\_application](https://shopify.dev/docs/api/liquid/objects/discount_application)

  * The discount applications that apply at the order level.

  * **confirmation\_​number**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A randomly generated alpha-numeric identifier for the order that may be shown to the customer instead of the sequential order name. For example, "XPAV284CT", "R50KELTJP" or "35PKUN0UJ". This value isn't guaranteed to be unique.

  * **created\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the order was created.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **customer**

    [customer](https://shopify.dev/docs/api/liquid/objects/customer)

  * The customer that placed the order.

  * **customer\_​order\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the new order details page.

    Customer accounts includes a list of Buyers Orders and an Order Details View. This liquid function exposes a URL to a specific Orders Details in customer accounts. [Setup process for the new order details page](https://help.shopify.com/en/manual/customers/customer-accounts/new-customer-accounts) can be found in the help center.

  * **customer\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the customer to view the order in their account.

  * **discount\_​applications**

    array of [discount\_application](https://shopify.dev/docs/api/liquid/objects/discount_application)

  * All of the discount applications for the order and its line items.

  * **email**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The email that's associated with the order.

    If no email is associated with the order, then `nil` is returned.

  * **financial\_​status**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The order's financial status.

    | Possible values |
    | - |
    | authorized |
    | expired |
    | paid |
    | partially\_paid |
    | partially\_refunded |
    | pending |
    | refunded |
    | unpaid |
    | voided |

  * **financial\_​status\_​label**

  * The localized version of the [financial status](https://shopify.dev/docs/api/liquid/objects/order#order-financial_status) of the order.

    **Tip:** Use this property to output the financial status on the storefront.

  * **fulfillment\_​status**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The fulfillment status of the order.

  * **fulfillment\_​status\_​label**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The localized version of the [fulfillment status](https://shopify.dev/docs/api/liquid/objects/order#order-fulfillment_status) of the order.

    **Tip:** Use this property to output the fulfillment status on the storefront.

    | Possible values |
    | - |
    | complete |
    | fulfilled |
    | partial |
    | restocked |
    | unfulfilled |

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the order.

  * **item\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of items in the order.

  * **line\_​items**

    array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

  * The line items in the order.

  * **line\_​items\_​subtotal\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The sum of the prices of all of the line items in the order in the currency's subunit, after any line item discounts have been applied.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **metafields**

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the order.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the order.

  * **note**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The note on the order.

    If there's no note on the order, then `nil` is returned.

    **Tip:** Notes are \<a href="https://shopify.dev/themes/architecture/templates/cart#support-cart-notes-and-attributes">collected with the cart\</a>.

  * **order\_​number**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The integer representation of the order [name](https://shopify.dev/docs/api/liquid/objects/order#order-name).

  * **order\_​status\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the [**Order status** page](https://help.shopify.com/manual/orders/status-tracking) for the order.

  * **phone**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The phone number associated with the order.

  * **pickup\_​in\_​store?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the order is a store pickup order.

  * **shipping\_​address**

    [address](https://shopify.dev/docs/api/liquid/objects/address)

  * The shipping address of the order.

  * **shipping\_​methods**

    array of [shipping\_method](https://shopify.dev/docs/api/liquid/objects/shipping_method)

  * The shipping methods for the order.

  * **shipping\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The shipping price of the order in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **subtotal\_​line\_​items**

    array of [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

  * The non-tip line items in the order.

    **Note:** These line items are used to calculate the the \<a href="/docs/api/liquid/objects/order#order-subtotal\_price">subtotal price\</a>.

  * **subtotal\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The sum of the prices of the [subtotal line items](https://shopify.dev/docs/api/liquid/objects/order#order-subtotal_line_items) in the currency's subunit, after any line item or cart discounts have been applied.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **tags**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [tags](https://help.shopify.com/manual/shopify-admin/productivity-tools/using-tags) on the order.

    The tags are returned in alphabetical order.

  * **tax\_​lines**

    array of [tax\_line](https://shopify.dev/docs/api/liquid/objects/tax_line)

  * The tax lines on the order.

  * **tax\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total amount of taxes applied to the order in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **total\_​discounts**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total amount of all discounts applied to the order in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **total\_​duties**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The sum of all duties applied to the line items in the order in the currency's subunit.

    If there are no duties, then `nil` is returned. The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **total\_​net\_​amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The net amount of the order in the currency's subunit.

    The amount is calculated after refunds are applied, so is equal to `order.total_price` minus `order.total_refunded_amount`.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **total\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total price of the order in the currency's subunit.

    **Note:** The total price is calculated before refunds are applied. Use \<a href="/docs/api/liquid/objects/order#order-total\_net\_amount">\<code>\<span class="PreventFireFoxApplyingGapToWBR">order.total\<wbr/>\_net\<wbr/>\_amount\</span>\</code>\</a> to output the total minus any refunds.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **total\_​refunded\_​amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total amount that's been refunded from the order in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **transactions**

    array of [transaction](https://shopify.dev/docs/api/liquid/objects/transaction)

  * The transactions of the order.

## Deprecated Properties

* * **discounts**

    [discount](https://shopify.dev/docs/api/liquid/objects/discount)

    Deprecated

  * The discounts on the order.

    **Deprecated:**

    Deprecated because not all discount types and details are captured.

    The `order.discounts` property has been replaced by [`order.discount_applications`](https://shopify.dev/docs/api/liquid/objects/order#order-discount_applications).

##### Example

```json
{
  "attributes": {},
  "billing_address": {},
  "cancel_reason": null,
  "cancel_reason_label": null,
  "cancelled": false,
  "cancelled_at": null,
  "cart_level_discount_applications": [],
  "confirmation_number": "0YMJHPM8U",
  "created_at": "2022-04-29 11:15:46 -0400",
  "customer": {},
  "customer_order_url": "https://shopify.com/56174706753/account/orders/4295688749121?locale=en&region_country=CA&buyer_flags=eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb2xpbmFzLXBvdGVudC1wb3Rpb25zLm15c2hvcGlmeS5jb20iLCJmbGFncyI6W10sImV4cCI6MTc3ODczMjI3MiwibmJmIjoxNzc4MTI3NDcyfQ.8uKYd0IDyCQeQjGqpQigmDN128vceeB5x4K8K5-8_QI",
  "customer_url": "https://polinas-potent-potions.myshopify.com/account/orders/8be02e56c658bcd1f034d28c496fddd9",
  "discount_applications": [],
  "discounts": null,
  "email": "cornelius.potionmaker@gmail.com",
  "financial_status": "paid",
  "financial_status_label": "Paid",
  "fulfillment_status": "partial",
  "fulfillment_status_label": "Partial",
  "id": 4295688749121,
  "item_count": 6,
  "line_items": [],
  "line_items_subtotal_price": "492.93",
  "metafields": {},
  "name": "#1001",
  "note": null,
  "order_number": 1001,
  "order_status_url": "https://polinas-potent-potions.myshopify.com/56174706753/orders/8be02e56c658bcd1f034d28c496fddd9/authenticate?key=4f9baf2b8ebd0f75ec73eb9bac6e4519",
  "phone": null,
  "pickup_in_store?": false,
  "shipping_address": {},
  "shipping_methods": [],
  "shipping_price": "0.00",
  "subtotal_line_items": [],
  "subtotal_price": "492.93",
  "tags": [],
  "tax_lines": [],
  "tax_price": "0.00",
  "total_discounts": "0.00",
  "total_duties": null,
  "total_net_amount": "492.93",
  "total_price": "492.93",
  "total_refunded_amount": "0.00",
  "transactions": []
}
```

## Templates using order

[Theme architecture](https://shopify.dev/themes/architecture/templates/customers-order)

[customers/order template](https://shopify.dev/themes/architecture/templates/customers-order)

</page>

<page>
---
title: 'Liquid objects: page'
description: >-
  A
  [page](https://help.shopify.com/manual/online-store/themes/theme-structure/pages)
  on a store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/page'
  md: 'https://shopify.dev/docs/api/liquid/objects/page.md'
---

# page

A [page](https://help.shopify.com/manual/online-store/themes/theme-structure/pages) on a store.

## Properties

* * **author**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The author of the page.

  * **content**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The content of the page.

  * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the page.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the page.

  * **metafields**

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the page.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **published\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the page was published.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **template\_​suffix**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the [custom template](https://shopify.dev/themes/architecture/templates#alternate-templates) assigned to the page.

    The name doesn't include the `page.` prefix, or the file extension (`.json` or `.liquid`).

    If a custom template isn't assigned to the page, then `nil` is returned.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the page.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the page.

##### Example

```json
{
  "author": null,
  "content": "<p>Polina's Potent Potions was started by Polina in 1654.</p>\n<p>We use all-natural locally sourced ingredients for our potions.</p>",
  "handle": "about-us",
  "id": 83536642113,
  "metafields": {},
  "published_at": "2022-05-04 17:47:03 -0400",
  "template_suffix": "",
  "title": "About us",
  "url": {}
}
```

## Templates using page

[Theme architecture](https://shopify.dev/themes/architecture/templates/page)

[page template](https://shopify.dev/themes/architecture/templates/page)

</page>

<page>
---
title: 'Liquid objects: page_description'
description: The meta description of the current page.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/page_description'
  md: 'https://shopify.dev/docs/api/liquid/objects/page_description.md'
---

# page\_​description

The meta description of the current page.

The `page_description` object can be used to provide a brief description of a page for search engine listings and social media previews.

To learn about where to edit the meta description for a page, visit the [Shopify Help Center](https://help.shopify.com/manual/promoting-marketing/seo/adding-keywords#edit-the-title-and-meta-description-for-a-page).

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: page_image'
description: >-
  An image to be shown in search engine listings and social media previews for
  the current page.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/page_image'
  md: 'https://shopify.dev/docs/api/liquid/objects/page_image.md'
---

# page\_​image

An image to be shown in search engine listings and social media previews for the current page.

The resource's featured image for product and collection pages, and blog posts, is used. For all other pages, or pages where there's no featured image, the [social sharing image](https://help.shopify.com/manual/online-store/images/showing-social-media-thumbnail-images?#setting-the-social-sharing-image-in-your-admin) is used.

### Open Graph fallback tags

The `page_image` object can be used for creating [Open Graph](https://ogp.me/) `og:image` meta tags.

If a theme doesn't include `og:image` tags for a page, then Shopify automatically generates the following tags using the `page_image` object:

* `og:image`
* `og:image:secure_url`
* `og:image:width`
* `og:image:height`

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: page_title'
description: The page title of the current page.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/page_title'
  md: 'https://shopify.dev/docs/api/liquid/objects/page_title.md'
---

# page\_​title

The page title of the current page.

The `page_title` object can be used to specify the title of page for search engine listings and social media previews.

To learn about where to edit the title for a page, visit the [Shopify Help Center](https://help.shopify.com/manual/promoting-marketing/seo/adding-keywords#edit-the-title-and-meta-description-for-a-page).

### Directly accessible in

* Global

</page>

<page>
---
title: 'Liquid objects: pages'
description: 'All of the [pages](/docs/api/liquid/objects/page) on a store.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/pages'
  md: 'https://shopify.dev/docs/api/liquid/objects/pages.md'
---

# pages

All of the [pages](https://shopify.dev/docs/api/liquid/objects/page) on a store.

### Directly accessible in

* Global

You can access a specific page through the `pages` object using the page's [handle](https://shopify.dev/docs/api/liquid/basics#handles).

##### Code

```liquid
{{ pages.contact.title }}
{{ pages['about-us'].title }}
```

##### Output

```html
Contact
About us
```

### Paginate the `pages` object

You can [paginate](https://shopify.dev/docs/api/liquid/tags/paginate) the `pages` object, allowing you to iterate over up to 50 pages at a time.

##### Code

```liquid
{% paginate pages by 2 -%}
  {% for page in pages -%}
    {{ page.title | link_to: page.url }}
  {%- endfor %}

  {{- paginate | default_pagination }}
{%- endpaginate %}
```

##### Output

```html
<a href="/pages/about-us" title="">About us</a>
<a href="/pages/contact" title="">Contact</a>

<span class="page current">1</span> <span class="page"><a href="/services/liquid_rendering/resource?page=2" title="">2</a></span> <span class="next"><a href="/services/liquid_rendering/resource?page=2" title="">Next &raquo;</a></span>
```

</page>

<page>
---
title: 'Liquid objects: paginate'
description: >-
  Information about the pagination inside a set of [`paginate`
  tags](/docs/api/liquid/tags/paginate).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/paginate'
  md: 'https://shopify.dev/docs/api/liquid/objects/paginate.md'
---

# paginate

Information about the pagination inside a set of [`paginate` tags](https://shopify.dev/docs/api/liquid/tags/paginate).

***

**Tip:** Use the \<a href="/docs/api/liquid/filters/default\_pagination">\<code>\<span class="PreventFireFoxApplyingGapToWBR">default\<wbr/>\_pagination\</span>\</code> filter\</a> to output pagination links.

***

## Properties

* * **current\_​offset**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of items on pages previous to the current page.

    For example, if you show 5 items per page and are on page 3, then the value of `paginate.current_offset` is 10.

    Limited to 24,999 see [Pagination Limits](https://shopify.dev/themes/best-practices/performance/platform#pagination-limits) for more information.

  * **current\_​page**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The page number of the current page.

    Limited to 25,000 see [Pagination Limits](https://shopify.dev/themes/best-practices/performance/platform#pagination-limits) for more information.

  * **items**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of items to be paginated.

    For example, if you paginate a collection of 120 products, then the value of `paginate.items` is 120.

    Limited to 25,000 see [Pagination Limits](https://shopify.dev/themes/best-practices/performance/platform#pagination-limits) for more information.

  * **next**

    [part](https://shopify.dev/docs/api/liquid/objects/part)

  * The pagination part to go to the next page.

  * **page\_​param**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL parameter denoting the pagination.

    The default value is `page`.

    If you paginate over an array defined in a setting or a metafield list type, then a unique key is appended to page to allow the paginated list to operate independently from other lists on the page. For example, a paginated list defined in a setting might use the key `page_a9e329dc`.

  * **page\_​size**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of items displayed per page.

    Limited to 250.

  * **pages**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of pages.

    Limited to 25,000 see [Pagination Limits](https://shopify.dev/themes/best-practices/performance/platform#pagination-limits) for more information.

  * **parts**

    array of [part](https://shopify.dev/docs/api/liquid/objects/part)

  * The pagination parts.

    Pagination parts are used to build pagination navigation.

  * **previous**

    [part](https://shopify.dev/docs/api/liquid/objects/part)

  * The pagination part to go to the previous page.

##### Example

```json
{
  "current_offset": 10,
  "current_page": 3,
  "items": 17,
  "next": {},
  "page_param": "page",
  "page_size": 5,
  "pages": 4,
  "parts": [],
  "previous": {}
}
```

</page>

<page>
---
title: 'Liquid objects: parent_relationship'
description: Information about the parent relationship for a nested cart line item.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/parent_relationship'
  md: 'https://shopify.dev/docs/api/liquid/objects/parent_relationship.md'
---

# parent\_​relationship

Information about the parent relationship for a nested cart line item.

## Properties

* * **parent**

    [line\_item](https://shopify.dev/docs/api/liquid/objects/line_item)

  * The parent line item for the nested cart line item.

### Returned by

* [line\_​item.parent\_​relationship](https://shopify.dev/docs/api/liquid/objects/line_item#line_item-parent_relationship)

</page>

<page>
---
title: 'Liquid objects: part'
description: A part in the navigation for pagination.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/part'
  md: 'https://shopify.dev/docs/api/liquid/objects/part.md'
---

# part

A part in the navigation for pagination.

## Properties

* * **is\_​link**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the part is a link. Returns `false` if not.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The page number associated with the part.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL of the part.

    It consists of the current page URL path with the pagination parameter for the current part appended.

##### Example

```json
{
  "is_link": true,
  "title": "2",
  "url": "/collections/all?page=2"
}
```

### Create pagination navigation with `part`

You can create a pagination navigation by iterating over each `part` of a [`paginate` object](https://shopify.dev/docs/api/liquid/objects/paginate).

##### Code

```liquid
{% paginate collection.products by 5 -%}
  {% for part in paginate.parts -%}
    {% if part.is_link -%}
      {{ part.title | link_to: part.url}}
    {%- else -%}
      <span>{{ part.title }}</span>
    {% endif %}
  {%- endfor %}
{%- endpaginate %}
```

##### Data

```json
{
  "collection": {
    "products_count": 19
  }
}
```

##### Output

```html
<span>1</span>
    
<a href="/services/liquid_rendering/resource?page=2" title="">2</a>

<a href="/services/liquid_rendering/resource?page=3" title="">3</a>

<a href="/services/liquid_rendering/resource?page=4" title="">4</a>
```

</page>

<page>
---
title: 'Liquid objects: pending_payment_instruction_input'
description: >-
  Header-value pairs that make up the list of payment information specific to
  the payment method.

  This information can be be used by the customer to complete the transaction
  offline.
api_name: liquid
source_url:
  html: >-
    https://shopify.dev/docs/api/liquid/objects/pending_payment_instruction_input
  md: >-
    https://shopify.dev/docs/api/liquid/objects/pending_payment_instruction_input.md
---

# pending\_​payment\_​instruction\_​input

Header-value pairs that make up the list of payment information specific to the payment method. This information can be be used by the customer to complete the transaction offline.

## Properties

* * **header**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The header of the payment instruction. These are payment method-specific. Example: "Entity" and "Reference" for Multibanco

  * **value**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Contains the corresponding values to the headers of the payment instruction.

### Returned by

* [transaction.buyer\_​pending\_​payment\_​instructions](https://shopify.dev/docs/api/liquid/objects/transaction#transaction-buyer_pending_payment_instructions)

</page>

<page>
---
title: 'Liquid objects: policy'
description: >-
  A [store
  policy](https://help.shopify.com/manual/checkout-settings/refund-privacy-tos),
  such as a privacy or return policy.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/policy'
  md: 'https://shopify.dev/docs/api/liquid/objects/policy.md'
---

# policy

A [store policy](https://help.shopify.com/manual/checkout-settings/refund-privacy-tos), such as a privacy or return policy.

## Properties

* * **body**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The content of the policy.

  * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the policy.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the policy.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the policy.

##### Example

```json
{
  "body": "<p>We have a 30-day return policy, which means you have 30 days after receiving your item to request a return. ...</p>",
  "id": 23805034561,
  "title": "Refund policy",
  "url": "/policies/refund-policy"
}
```

</page>

<page>
---
title: 'Liquid objects: powered_by_link'
description: >-
  Creates an HTML link element that links to a localized version of
  `shopify.com`, based on the locale of the store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/powered_by_link'
  md: 'https://shopify.dev/docs/api/liquid/objects/powered_by_link.md'
---

# powered\_​by\_​link

Creates an HTML link element that links to a localized version of `shopify.com`, based on the locale of the store.

### Directly accessible in

* Global

##### Code

```liquid
{{ powered_by_link }}
```

##### Output

```html
<a target="_blank" rel="nofollow" href="https://www.shopify.com?utm_campaign=poweredby&amp;utm_medium=shopify&amp;utm_source=onlinestore">Powered by Shopify</a>
```

</page>

<page>
---
title: 'Liquid objects: predictive_search'
description: >-
  Information about the results from a predictive search query through the

  [Predictive Search
  API](/api/ajax/reference/predictive-search#get-locale-search-suggest).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/predictive_search'
  md: 'https://shopify.dev/docs/api/liquid/objects/predictive_search.md'
---

# predictive\_​search

Information about the results from a predictive search query through the [Predictive Search API](https://shopify.dev/api/ajax/reference/predictive-search#get-locale-search-suggest).

***

**Note:** The \<code>\<span class="PreventFireFoxApplyingGapToWBR">predictive\<wbr/>\_search\</span>\</code> object returns results only when rendered in a section using the Predictive Search API and the \<a href="/api/section-rendering">Section Rendering API\</a>. To learn about how to include predictive search in your theme, refer to \<a href="/themes/navigation-search/search/predictive-search">Add predictive search to your theme\</a>.

***

## Properties

* * **performed**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` when being referenced inside a section that's been rendered using the Predictive Search API and the Section Rendering API. Returns `false` if not.

  * **resources**

    [predictive\_search\_resources](https://shopify.dev/docs/api/liquid/objects/predictive_search_resources)

  * The resources associated with the query.

    You can check whether any resources of a specific type were returned using the [`size` filter](https://shopify.dev/docs/api/liquid/filters/size).

    ```liquid
    {% if predictive_search.resources.articles.size > 0 %}
      {% for article in predictive_search.resources.articles %}
        {{ article.title }}
      {% endfor %}
    {% endif %}
    ```

    ```liquid
    {% if predictive_search.resources.articles.size > 0 %}
      {% for article in predictive_search.resources.articles %}
        {{ article.title }}
      {% endfor %}
    {% endif %}
    ```

  * **terms**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The entered search terms.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/highlight">\<code>highlight\</code> filter\</a> to highlight the search terms in search results content.

  * **types**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The object types that the search was performed on.

    Searches can be performed on the following object types:

    * [`article`](https://shopify.dev/docs/api/liquid/objects/article)
    * [`collection`](https://shopify.dev/docs/api/liquid/objects/collection)
    * [`page`](https://shopify.dev/docs/api/liquid/objects/page)
    * [`product`](https://shopify.dev/docs/api/liquid/objects/product)

    **Note:** The types are determined by the \<a href="/api/ajax/reference/predictive-search#query-parameters">\<code>type\</code> query parameter\</a>.

##### Example

```json
{
  "performed": true,
  "resources": {},
  "terms": "potion",
  "types": []
}
```

</page>

<page>
---
title: 'Liquid objects: predictive_search_resources'
description: >-
  Contains arrays of objects for each resource type that can be returned by a
  [predictive search
  query](/api/ajax/reference/predictive-search#get-locale-search-suggest).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/predictive_search_resources'
  md: 'https://shopify.dev/docs/api/liquid/objects/predictive_search_resources.md'
---

# predictive\_​search\_​resources

Contains arrays of objects for each resource type that can be returned by a [predictive search query](https://shopify.dev/api/ajax/reference/predictive-search#get-locale-search-suggest).

You can check whether any resources of a specific type were returned using the [`size` filter](https://shopify.dev/docs/api/liquid/filters/size).

```liquid
{% if predictive_search.resources.articles.size > 0 %}
  {% for article in predictive_search.resources.articles %}
    {{ article.title }}
  {% endfor %}
{% endif %}
```

```liquid
{% if predictive_search.resources.articles.size > 0 %}
  {% for article in predictive_search.resources.articles %}
    {{ article.title }}
  {% endfor %}
{% endif %}
```

## Properties

* * **articles**

    array of [article](https://shopify.dev/docs/api/liquid/objects/article)

  * The articles associated with the query.

  * **collections**

    array of [collection](https://shopify.dev/docs/api/liquid/objects/collection)

  * The collections associated with the query.

  * **pages**

    array of [page](https://shopify.dev/docs/api/liquid/objects/page)

  * The pages associated with the query.

  * **products**

    array of [product](https://shopify.dev/docs/api/liquid/objects/product)

  * The products associated with the query.

##### Example

```json
{
  "articles": [],
  "collections": [],
  "pages": [],
  "products": []
}
```

</page>

<page>
---
title: 'Liquid objects: product'
description: 'A [product](https://help.shopify.com/manual/products) in the store.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/product'
  md: 'https://shopify.dev/docs/api/liquid/objects/product.md'
---

# product

A [product](https://help.shopify.com/manual/products) in the store.

## Properties

* * **available**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if at least one of the variants of the product is available. Returns `false` if not.

    For a variant to be available, it needs to meet one of the following criteria:

    * The `variant.inventory_quantity` is greater than 0.
    * The `variant.inventory_policy` is set to `continue`.
    * The `variant.inventory_management` is `nil`.
    * The variant has an associated [delivery profile](https://shopify.dev/docs/apps/selling-strategies/purchase-options/deferred/shipping-delivery/delivery-profiles) with a valid shipping rate.

  * **category**

    [taxonomy\_category](https://shopify.dev/docs/api/liquid/objects/taxonomy_category)

  * The taxonomy category for the product

  * **collections**

    array of [collection](https://shopify.dev/docs/api/liquid/objects/collection)

  * The collections that the product belongs to.

    **Note:** Collections that aren\&#39;t \<a href="https://help.shopify.com/manual/products/collections/make-collections-available">available\</a> on the Online Store sales channel aren\&#39;t included.

  * **compare\_​at\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The lowest **compare at** price of any variants of the product in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **compare\_​at\_​price\_​max**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The highest **compare at** price of any variants of the product in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **compare\_​at\_​price\_​min**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The lowest **compare at** price of any variants of the product in the currency's subunit. This is the same as `product.compare_at_price`.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **compare\_​at\_​price\_​varies**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant **compare at** prices of the product vary. Returns `false` if not.

  * **content**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The description of the product.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/product#product-description">\<code>product.description\</code>\</a>.

  * **created\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the product was created.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **description**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The description of the product.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/product#product-content">\<code>product.content\</code>\</a>.

  * **featured\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The first (featured) image attached to the product.

  * **featured\_​media**

    [media](https://shopify.dev/docs/api/liquid/objects/media)

  * The first (featured) media attached to the product.

    **Note:** In search results or a filtered collection, the \<a href="/docs/api/liquid/objects/media">media\</a> of the most relevant variant is returned. Relevancy is based on search terms and applied filters.

    **Tip:** You can use \<a href="/docs/api/liquid/filters/media-filters">media filters\</a> to output media URLs and displays. To learn about how to include media in your theme, refer to \<a href="/themes/product-merchandising/media/support-media">Support product media\</a>.

  * **first\_​available\_​variant**

    [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The first available variant of the product.

    For a variant to be available, it needs to meet one of the following criteria:

    * The `variant.inventory_quantity` is greater than 0.
    * The `variant.inventory_policy` is set to `continue`.
    * The `variant.inventory_management` is `nil`.

  * **gift\_​card?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the product is a gift card. Returns `false` if not.

  * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the product.

  * **has\_​only\_​default\_​variant**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the product doesn't have any options. Returns `false` if not.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the product.

  * **images**

    array of [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The images attached to the product.

  * **media**

    array of [media](https://shopify.dev/docs/api/liquid/objects/media)

  * The media attached to the product, sorted by the date it was added to the product.

    **Tip:** You can use \<a href="/docs/api/liquid/filters/media-filters">media filters\</a> to output media URLs and displays. To learn about how to include media in your theme, refer to \<a href="/themes/product-merchandising/media/support-media">Support product media\</a>.

  * **metafields**

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the product.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **options**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The option names of the product.

    ExampleOutput the options

    You can use the [`size` filter](https://shopify.dev/docs/api/liquid/filters/size) with dot notation to determine how many options a product has.

    ##### Code

    ```liquid
    {% if product.options.size > 0 -%}
      {% for option in product.options -%}
        - {{ option }}
      {%- endfor %}
    {%- endif %}
    ```

    ##### Data

    ```json
    {
      "product": {
        "options": [
          "Size",
          "Strength"
        ]
      }
    }
    ```

    ##### Output

    ```html
    - Size
    - Strength
    ```

  * **options\_​by\_​name**

  * Allows you to access a specific [product option](https://shopify.dev/docs/api/liquid/objects/product_option) by its name.

    ExampleOutput the values for a specific option

    When accessing a specific option, the name is case-insensitive.

    ##### Code

    ```liquid
    <label>
      Strength
      <select>
        {%- for value in product.options_by_name['strength'].values %}
        <option>{{ value }}</option>
        {%- endfor %}
      </select>
    </label>
    ```

    ##### Data

    ```json
    {
      "product": {
        "options_by_name": {}
      }
    }
    ```

    ##### Output

    ```html
    <label>
      Strength
      <select>
        <option>Low</option>
        <option>Medium</option>
        <option>High</option>
      </select>
    </label>
    ```

  * **options\_​with\_​values**

    array of [product\_option](https://shopify.dev/docs/api/liquid/objects/product_option)

  * The options on the product.

  * **price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The lowest price of any variants of the product in the currency's subunit.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/product#product-price\_min">\<code>\<span class="PreventFireFoxApplyingGapToWBR">product.price\<wbr/>\_min\</span>\</code>\</a>.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **price\_​max**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The highest price of any variants of the product in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **price\_​min**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The lowest price of any variants of the product in the currency's subunit.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/product#product-price">\<code>product.price\</code>\</a>.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **price\_​varies**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the product's variant prices vary. Returns `false` if not.

  * **published\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the product was published.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **quantity\_​price\_​breaks\_​configured?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the product has at least one variant with quantity price breaks in the current customer context. Returns `false` if not.

  * **requires\_​selling\_​plan**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if all of the variants of the product require a selling plan. Returns `false` if not.

    **Note:** A variant requires a selling plan if \<a href="/docs/api/liquid/objects/variant#variant-requires\_selling\_plan">\<code>\<span class="PreventFireFoxApplyingGapToWBR">variant.requires\<wbr/>\_selling\<wbr/>\_plan\</span>\</code>\</a> is \<code>true\</code>.

  * **selected\_​or\_​first\_​available\_​selling\_​plan\_​allocation**

    [selling\_plan\_allocation](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation)

  * The currently selected, or first available, selling plan allocation.

    The following logic is used to determine which selling plan allocation is returned:

    | Selling plan allocation | Return criteria |
    | - | - |
    | The currently selected allocation | Returned if a variant and selling plan are selected. The selected variant is determined by the `variant` URL parameter, and the selected selling plan is determined by the `selling_plan` URL parameter. |
    | The first allocation on the first available variant | Returned if no allocation is currently selected. |
    | The first allocation on the first variant | Returned if no allocation is currently selected, and there are no available variants. |

    If the product doesn't have any selling plans, then `nil` is returned.

  * **selected\_​or\_​first\_​available\_​variant**

    [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The currently selected or first available variant of the product.

    If a variant is selected, it will be returned, regardless of its availability. Otherwise, the first available variant is returned. If no available variant exists, the first variant is returned.

    A selected variant is determined by the following criteria:

    * On product pages, it is based on the variant ID set in the `variant` URL parameter.
    * In search results and filtered collections, it is the most relevant variant based on search terms and applied filters.

    For a variant to be available, it needs to meet one of the following criteria:

    * The `variant.inventory_quantity` is greater than 0.
    * The `variant.inventory_policy` is set to `continue`.
    * The `variant.inventory_management` is `nil`.

  * **selected\_​selling\_​plan**

    [selling\_plan](https://shopify.dev/docs/api/liquid/objects/selling_plan)

  * The currently selected selling plan.

    If no selling plan is selected, then `nil` is returned.

    **Note:** The selected selling plan is determined by the \<code>\<span class="PreventFireFoxApplyingGapToWBR">selling\<wbr/>\_plan\</span>\</code> URL parameter.

  * **selected\_​selling\_​plan\_​allocation**

    [selling\_plan\_allocation](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation)

  * The currently selected selling plan allocation for the currently selected variant.

    If no variant and selling plan are selected, then `nil` is returned.

    **Note:** The selected variant is determined by the \<code>variant\</code> URL parameter, and the selected selling plan is determined by the \<code>\<span class="PreventFireFoxApplyingGapToWBR">selling\<wbr/>\_plan\</span>\</code> URL parameter.

  * **selected\_​variant**

    [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The currently selected variant of the product.

    If no variant is currently selected, then `nil` is returned.

    **Note:** On product pages, it is based on the variant ID set in the \<code>variant\</code> URL parameter. In search results and filtered collections, it is the most relevant variant based on search terms and applied filters.

  * **selling\_​plan\_​groups**

    array of [selling\_plan\_group](https://shopify.dev/docs/api/liquid/objects/selling_plan_group)

  * The selling plan groups that the variants of the product are included in.

  * **tags**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [tags](https://help.shopify.com/manual/shopify-admin/productivity-tools/using-tags) of the product.

    **Note:** The tags are returned in alphabetical order.

  * **template\_​suffix**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the [custom template](https://shopify.dev/themes/architecture/templates#alternate-templates) of the product.

    The name doesn't include the `product.` prefix, or the file extension (`.json` or `.liquid`).

    If a custom template isn't assigned to the product, then `nil` is returned.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the product.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The type of the product.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the product.

    If a product is rendered in search results or a filtered collection, then the URL contains the `variant` parameter of the most relevant variant.

    ```liquid
    /products/gorgeous-wooden-computer?variant=1234567890
    ```

    ```liquid
    /products/gorgeous-wooden-computer?variant=1234567890
    ```

    If a product is rendered as a [product recommendation](https://shopify.dev/docs/themes/product-merchandising/recommendations), then the URL contains tracking parameters:

    ```liquid
    /products/gorgeous-wooden-computer?pr_choice=default&pr_prod_strat=description&pr_rec_pid=13&pr_ref_pid=17&pr_seq=alternating
    ```

    ```liquid
    /products/gorgeous-wooden-computer?pr_choice=default&pr_prod_strat=description&pr_rec_pid=13&pr_ref_pid=17&pr_seq=alternating
    ```

  * **variants**

    array of [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The variants of the product.

    **Note:** Returns a maximum of 250 variants when unpaginated. Tip: Use the \<a href="/docs/api/liquid/tags/paginate">paginate\</a> tag to choose how many variants to show per page, up to a limit of 50.

  * **variants\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of variants for the product.

  * **vendor**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The vendor of the product.

##### Example

```json
{
  "available": true,
  "category": {},
  "collections": [],
  "compare_at_price": "25.00",
  "compare_at_price_max": "25.00",
  "compare_at_price_min": "25.00",
  "compare_at_price_varies": false,
  "content": "<h3>Are you low on health? Well we've got the potion just for you!</h3>\n<p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p>",
  "created_at": "2022-04-13 14:46:16 -0400",
  "description": "<h3>Are you low on health? Well we've got the potion just for you!</h3>\n<p>Just need a top up? Almost dead? In between? No need to worry because we have a range of sizes and strengths!</p>",
  "featured_image": {},
  "featured_media": {},
  "first_available_variant": {},
  "gift_card?": false,
  "handle": "health-potion",
  "has_only_default_variant": false,
  "id": 6786188247105,
  "images": [],
  "media": [],
  "metafields": {},
  "options": [
    "Size",
    "Strength"
  ],
  "options_by_name": {},
  "options_with_values": [],
  "price": "10.00",
  "price_max": "22.00",
  "price_min": "10.00",
  "price_varies": true,
  "published_at": "2022-04-13 14:53:34 -0400",
  "quantity_price_breaks_configured?": false,
  "requires_selling_plan": false,
  "selected_or_first_available_selling_plan_allocation": {},
  "selected_or_first_available_variant": {},
  "selected_selling_plan": null,
  "selected_selling_plan_allocation": null,
  "selected_variant": null,
  "selling_plan_groups": [],
  "tags": [
    "healing"
  ],
  "template_suffix": "",
  "title": "Health potion",
  "type": {},
  "url": {},
  "variants": [],
  "variants_count": 9,
  "vendor": "Polina's Potent Potions"
}
```

## Templates using product

[Theme architecture](https://shopify.dev/themes/architecture/templates/product)

[product template](https://shopify.dev/themes/architecture/templates/product)

</page>

<page>
---
title: 'Liquid objects: product_option'
description: 'A product option, such as size or color.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/product_option'
  md: 'https://shopify.dev/docs/api/liquid/objects/product_option.md'
---

# product\_​option

A product option, such as size or color.

## Properties

* * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the product option.

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the product option in the [`product.options_with_values` array](https://shopify.dev/docs/api/liquid/objects/product#product-options_with_values).

  * **selected\_​value**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The currently selected product option value.

    If no value is currently selected, then the first available variant is returned.

  * **values**

    array of [product\_option\_value](https://shopify.dev/docs/api/liquid/objects/product_option_value)

  * The possible values for the product option.

##### Example

```json
{
  "name": "Size",
  "position": 1,
  "selected_value": {},
  "values": []
}
```

</page>

<page>
---
title: 'Liquid objects: product_option_value'
description: 'A product option value, such as "red" for the option "color".'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/product_option_value'
  md: 'https://shopify.dev/docs/api/liquid/objects/product_option_value.md'
---

# product\_​option\_​value

A product option value, such as "red" for the option "color".

## Properties

* * **available**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Whether or not the option value is available.

    In the context of the selected values for previous options, indicates whether the current option value has any purchaseable combinations in any subsequent options, or whether the current option value is purchaseable if there are no subsequent options. For example, if a product comes in Color/Size/Material and Red/Small/Cotton is selected, `available` will indicate:

    * Color: Whether any variants for the Color option value are available for purchase.
    * Size: Whether any variants for Color:Red and the Size option value are available for purchase.
    * Material: Whether any variants for Color:Red, Size:Small, and the Material option value are available for purchase.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the product option value.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the product option value.

  * **product\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Returns a URL if the option value may be associated with another product, nil otherwise.

    ```liquid
    /products/gorgeous-wooden-computer
    ```

    ```liquid
    /products/gorgeous-wooden-computer
    ```

  * **selected**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Whether or not the option value is selected.

  * **swatch**

    [swatch](https://shopify.dev/docs/api/liquid/objects/swatch)

  * Returns a [swatch](https://shopify.dev/docs/api/liquid/objects/swatch) drop for the product option value. If there is no saved `color` or `image` content for the swatch, then the return value is `nil`.

  * **variant**

    [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The variant associated with this option value combined with the other currently selected option values, if one exists.

    If this option value is selected (`selected == true`), this returns the `selected_or_first_available_variant`.

    If this option value is not selected (`selected == false`), this returns the variant that is associated with the current option value and the other currently selected option values.

    Using optionValue.variant is the recommended way to render product option values availability. For more information, refer to [rendering option value availability.](https://shopify.dev/docs/storefronts/themes/product-merchandising/variants/support-high-variant-products#option-value-availability)

##### Example

```json
{
  "available": true,
  "id": 2070385033281,
  "name": "Bronze",
  "product_url": null,
  "selected": true,
  "swatch": {},
  "variant": {}
}
```

</page>

<page>
---
title: 'Liquid objects: quantity_price_break'
description: The per-unit price of a variant when purchasing the minimum quantity or more.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/quantity_price_break'
  md: 'https://shopify.dev/docs/api/liquid/objects/quantity_price_break.md'
---

# quantity\_​price\_​break

The per-unit price of a variant when purchasing the minimum quantity or more.

## Properties

* * **minimum\_​quantity**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The minimum quantity required to qualify for the price break.

  * **price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The price for the quantity price break once the minimum quantity is met.

    The value is the price in the customer's local (presentment) currency.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

##### Example

```json
{
  "minimum_quantity": "10",
  "price": "20.00"
}
```

</page>

<page>
---
title: 'Liquid objects: quantity_rule'
description: A variant order quantity rule.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/quantity_rule'
  md: 'https://shopify.dev/docs/api/liquid/objects/quantity_rule.md'
---

# quantity\_​rule

A variant order quantity rule.

If no rule exists, then a default value is returned.

This rule can be set as part of a [B2B catalog](https://help.shopify.com/manual/b2b/catalogs/quantity-pricing).

***

**Note:** The default quantity rule is \<code>min=1,max=null,increment=1\</code>.

***

## Properties

* * **increment**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number the order quantity can be incremented by. The default value is `1`.

  * **max**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The maximum order quantity.

    If there is no maximum quantity, then `nil` is returned.

  * **min**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The minimum order quantity. The default value is `1`.

##### Example

```json
{
  "min": 5,
  "max": 100,
  "increment": 5
}
```

### The variant order quantity rule

##### Code

```liquid
{{ product.variants.first.quantity_rule }}
```

##### Data

```json
{
  "product": {
    "variants": [
      {
        "quantity_rule": {
          "min": 1,
          "max": null,
          "increment": 1
        }
      },
      {
        "quantity_rule": {
          "min": 1,
          "max": null,
          "increment": 1
        }
      },
      {
        "quantity_rule": {
          "min": 1,
          "max": null,
          "increment": 1
        }
      },
      {
        "quantity_rule": {
          "min": 1,
          "max": null,
          "increment": 1
        }
      },
      {
        "quantity_rule": {
          "min": 1,
          "max": null,
          "increment": 1
        }
      },
      {
        "quantity_rule": {
          "min": 1,
          "max": null,
          "increment": 1
        }
      },
      {
        "quantity_rule": {
          "min": 1,
          "max": null,
          "increment": 1
        }
      },
      {
        "quantity_rule": {
          "min": 1,
          "max": null,
          "increment": 1
        }
      },
      {
        "quantity_rule": {
          "min": 1,
          "max": null,
          "increment": 1
        }
      }
    ]
  }
}
```

##### Output

```html
{"min"=>1, "max"=>nil, "increment"=>1}
```

</page>

<page>
---
title: 'Liquid objects: rating'
description: 'Information for a [`rating` type](/apps/metafields/types) metafield.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/rating'
  md: 'https://shopify.dev/docs/api/liquid/objects/rating.md'
---

# rating

Information for a [`rating` type](https://shopify.dev/apps/metafields/types) metafield.

***

**Tip:** To learn about metafield types, refer to \<a href="/apps/metafields/types">Metafield types\</a>.

***

## Properties

* * **rating**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The rating value.

  * **scale\_​max**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The maximum value of the rating scale.

  * **scale\_​min**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The minimum value of the rating scale.

##### Example

```json
{
  "rating": "4.5",
  "scale_max": "5.0",
  "scale_min": "0.0"
}
```

</page>

<page>
---
title: 'Liquid objects: recipient'
description: >-
  A recipient that is associated with a [gift
  card](https://help.shopify.com/manual/products/gift-card-products).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/recipient'
  md: 'https://shopify.dev/docs/api/liquid/objects/recipient.md'
---

# recipient

A recipient that is associated with a [gift card](https://help.shopify.com/manual/products/gift-card-products).

## Properties

* * **email**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The email of the recipient.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The full name of the recipient.

  * **nickname**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The nickname of the recipient.

##### Example

```json
{
  "email": "cornelius.potionmaker@gmail.com",
  "name": "Cornelius Potionmaker",
  "nickname": "Cornelius"
}
```

## Templates using recipient

[Theme architecture](https://shopify.dev/themes/architecture/templates/gift-card-liquid)

[gift\_card.liquid template](https://shopify.dev/themes/architecture/templates/gift-card-liquid)

</page>

<page>
---
title: 'Liquid objects: recommendations'
description: >-
  Product recommendations for a specific product based on sales data, product
  descriptions, and collection relationships.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/recommendations'
  md: 'https://shopify.dev/docs/api/liquid/objects/recommendations.md'
---

# recommendations

Product recommendations for a specific product based on sales data, product descriptions, and collection relationships.

Product recommendations become more accurate over time as new orders and product data become available. To learn more about how product recommendations are generated, refer to [Product recommendations](https://shopify.dev/themes/product-merchandising/recommendations).

***

**Note:** The \<code>recommendations\</code> object returns products only when rendered in a section using the \<a href="/api/ajax/reference/product-recommendations">Product Recommendations API\</a> and the \<a href="/api/section-rendering">Section Rendering API\</a>. To learn about how to include product recommendations in your theme, refer to \<a href="/themes/product-merchandising/recommendations/show-product-recommendations">Show product recommendations\</a>.

***

## Properties

* * **intent**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The recommendation intent.

    If `performed?` is `false`, then `nil` is returned.

  * **performed?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` when being referenced inside a section that's been rendered using the Product Recommendations API and the Section Rendering API. Returns `false` if not.

  * **products**

    array of [product](https://shopify.dev/docs/api/liquid/objects/product)

  * The recommended products.

    If `performed?` is `false`, then an [EmptyDrop](https://shopify.dev/docs/api/liquid/basics#emptydrop) is returned.

  * **products\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of recommended products.

    If `performed?` is `false`, then 0 is returned.

##### Example

```json
{
  "products": [],
  "products_count": 4,
  "performed?": true
}
```

</page>

<page>
---
title: 'Liquid objects: remote_details'
description: Information about the remote source from which the object came from.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/remote_details'
  md: 'https://shopify.dev/docs/api/liquid/objects/remote_details.md'
---

# remote\_​details

Information about the remote source from which the object came from.

Remote details can only be accessed on an object that comes from a remote source, such as a product from another store.

## Properties

* * **shop**

    [remote\_shop](https://shopify.dev/docs/api/liquid/objects/remote_shop)

  * Information about the store that the remote object came from.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Provides context on how the remote object was surfaced. Currently the only supported value is "seller", but this may be expanded in the future.

### Returned by

* [remote\_​product](https://shopify.dev/docs/api/liquid/objects/remote_product)
* [remote\_​product.remote\_​details](https://shopify.dev/docs/api/liquid/objects/remote_product#remote_product-remote_details)

</page>

<page>
---
title: 'Liquid objects: remote_product'
description: >-
  A product that comes from a remote source, inheriting all
  [product](/docs/api/liquid/objects/product) functionality and also providing
  additional context about the remote source.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/remote_product'
  md: 'https://shopify.dev/docs/api/liquid/objects/remote_product.md'
---

# remote\_​product

A product that comes from a remote source, inheriting all [product](https://shopify.dev/docs/api/liquid/objects/product) functionality and also providing additional context about the remote source.

## Properties

* * **available**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if at least one of the variants of the product is available. Returns `false` if not.

    For a variant to be available, it needs to meet one of the following criteria:

    * The `variant.inventory_quantity` is greater than 0.
    * The `variant.inventory_policy` is set to `continue`.
    * The `variant.inventory_management` is `nil`.
    * The variant has an associated [delivery profile](https://shopify.dev/docs/apps/selling-strategies/purchase-options/deferred/shipping-delivery/delivery-profiles) with a valid shipping rate.

  * **category**

    [taxonomy\_category](https://shopify.dev/docs/api/liquid/objects/taxonomy_category)

  * The taxonomy category for the product

  * **compare\_​at\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The lowest **compare at** price of any variants of the product in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **compare\_​at\_​price\_​max**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The highest **compare at** price of any variants of the product in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **compare\_​at\_​price\_​min**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The lowest **compare at** price of any variants of the product in the currency's subunit. This is the same as `product.compare_at_price`.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **compare\_​at\_​price\_​varies**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant **compare at** prices of the product vary. Returns `false` if not.

  * **content**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The description of the product.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/product#product-description">\<code>product.description\</code>\</a>.

  * **created\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the product was created.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **description**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The description of the product.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/product#product-content">\<code>product.content\</code>\</a>. The description of remote products is modified to include a link to the remote store\&#39;s shipping and refund policies, if the shop has defined them.

  * **featured\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The first (featured) image attached to the product.

  * **featured\_​media**

    [media](https://shopify.dev/docs/api/liquid/objects/media)

  * The first (featured) media attached to the product.

    **Note:** Depending on rendering context, the featured media of remote products may be modified to include a badge highlighting the remote source.

    **Tip:** You can use \<a href="/docs/api/liquid/filters/media-filters">media filters\</a> to output media URLs and displays. To learn about how to include media in your theme, refer to \<a href="/themes/product-merchandising/media/support-media">Support product media\</a>.

  * **first\_​available\_​variant**

    [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The first available variant of the product.

    For a variant to be available, it needs to meet one of the following criteria:

    * The `variant.inventory_quantity` is greater than 0.
    * The `variant.inventory_policy` is set to `continue`.
    * The `variant.inventory_management` is `nil`.

  * **gift\_​card?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the product is a gift card. Returns `false` if not.

  * **has\_​only\_​default\_​variant**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the product doesn't have any options. Returns `false` if not.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the product.

  * **images**

    array of [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The images attached to the product.

  * **media**

    array of [media](https://shopify.dev/docs/api/liquid/objects/media)

  * The media attached to the product, sorted by the date it was added to the product.

    **Note:** Depending on rendering context, the media of remote products may be modified to include a badge highlighting the remote source.

    **Tip:** You can use \<a href="/docs/api/liquid/filters/media-filters">media filters\</a> to output media URLs and displays. To learn about how to include media in your theme, refer to \<a href="/themes/product-merchandising/media/support-media">Support product media\</a>.

  * **metafields**

  * The metafields applied to the product.

    **Note:** Only standard metafields set by the remote store are included. Custom metafields are not.

  * **options**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The option names of the product.

    ExampleOutput the options

    You can use the [`size` filter](https://shopify.dev/docs/api/liquid/filters/size) with dot notation to determine how many options a product has.

    ##### Code

    ```liquid
    {% if product.options.size > 0 -%}
      {% for option in product.options -%}
        - {{ option }}
      {%- endfor %}
    {%- endif %}
    ```

    ##### Data

    ```json
    {
      "product": {
        "options": [
          "Size",
          "Strength"
        ]
      }
    }
    ```

    ##### Output

    ```html
    - Size
    - Strength
    ```

  * **options\_​by\_​name**

  * Allows you to access a specific [product option](https://shopify.dev/docs/api/liquid/objects/product_option) by its name.

    ExampleOutput the values for a specific option

    When accessing a specific option, the name is case-insensitive.

    ##### Code

    ```liquid
    <label>
      Strength
      <select>
        {%- for value in product.options_by_name['strength'].values %}
        <option>{{ value }}</option>
        {%- endfor %}
      </select>
    </label>
    ```

    ##### Data

    ```json
    {
      "product": {
        "options_by_name": {}
      }
    }
    ```

    ##### Output

    ```html
    <label>
      Strength
      <select>
        <option>Low</option>
        <option>Medium</option>
        <option>High</option>
      </select>
    </label>
    ```

  * **options\_​with\_​values**

    array of [product\_option](https://shopify.dev/docs/api/liquid/objects/product_option)

  * The options on the product.

  * **price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The lowest price of any variants of the product in the currency's subunit.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/product#product-price\_min">\<code>\<span class="PreventFireFoxApplyingGapToWBR">product.price\<wbr/>\_min\</span>\</code>\</a>.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **price\_​max**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The highest price of any variants of the product in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **price\_​min**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The lowest price of any variants of the product in the currency's subunit.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/product#product-price">\<code>product.price\</code>\</a>.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **price\_​varies**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the product's variant prices vary. Returns `false` if not.

  * **published\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp for when the product was published.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **quantity\_​price\_​breaks\_​configured?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the product has at least one variant with quantity price breaks in the current customer context. Returns `false` if not.

  * **remote\_​details**

    [remote\_details](https://shopify.dev/docs/api/liquid/objects/remote_details)

  * Information about the remote source from which the remote product came from.

  * **requires\_​selling\_​plan**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if all of the variants of the product require a selling plan. Returns `false` if not.

    **Note:** A variant requires a selling plan if \<a href="/docs/api/liquid/objects/variant#variant-requires\_selling\_plan">\<code>\<span class="PreventFireFoxApplyingGapToWBR">variant.requires\<wbr/>\_selling\<wbr/>\_plan\</span>\</code>\</a> is \<code>true\</code>.

  * **selected\_​or\_​first\_​available\_​selling\_​plan\_​allocation**

    [selling\_plan\_allocation](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation)

  * The currently selected, or first available, selling plan allocation.

    The following logic is used to determine which selling plan allocation is returned:

    | Selling plan allocation | Return criteria |
    | - | - |
    | The currently selected allocation | Returned if a variant and selling plan are selected. The selected variant is determined by the `variant` URL parameter, and the selected selling plan is determined by the `selling_plan` URL parameter. |
    | The first allocation on the first available variant | Returned if no allocation is currently selected. |
    | The first allocation on the first variant | Returned if no allocation is currently selected, and there are no available variants. |

    If the product doesn't have any selling plans, then `nil` is returned.

  * **selected\_​or\_​first\_​available\_​variant**

    [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The currently selected or first available variant of the product.

    If a variant is selected, it will be returned, regardless of its availability. Otherwise, the first available variant is returned. If no available variant exists, the first variant is returned.

    A selected variant is determined by the following criteria:

    * On product pages, it is based on the variant ID set in the `variant` URL parameter.
    * In search results and filtered collections, it is the most relevant variant based on search terms and applied filters.

    For a variant to be available, it needs to meet one of the following criteria:

    * The `variant.inventory_quantity` is greater than 0.
    * The `variant.inventory_policy` is set to `continue`.
    * The `variant.inventory_management` is `nil`.

  * **selected\_​selling\_​plan**

    [selling\_plan](https://shopify.dev/docs/api/liquid/objects/selling_plan)

  * The currently selected selling plan.

    If no selling plan is selected, then `nil` is returned.

    **Note:** The selected selling plan is determined by the \<code>\<span class="PreventFireFoxApplyingGapToWBR">selling\<wbr/>\_plan\</span>\</code> URL parameter.

  * **selected\_​selling\_​plan\_​allocation**

    [selling\_plan\_allocation](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation)

  * The currently selected selling plan allocation for the currently selected variant.

    If no variant and selling plan are selected, then `nil` is returned.

    **Note:** The selected variant is determined by the \<code>variant\</code> URL parameter, and the selected selling plan is determined by the \<code>\<span class="PreventFireFoxApplyingGapToWBR">selling\<wbr/>\_plan\</span>\</code> URL parameter.

  * **selected\_​variant**

    [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The currently selected variant of the product.

    If no variant is currently selected, then `nil` is returned.

    **Note:** On product pages, it is based on the variant ID set in the \<code>variant\</code> URL parameter. In search results and filtered collections, it is the most relevant variant based on search terms and applied filters.

  * **selling\_​plan\_​groups**

    array of [selling\_plan\_group](https://shopify.dev/docs/api/liquid/objects/selling_plan_group)

  * The selling plan groups that the variants of the product are included in.

  * **template\_​suffix**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the custom template assigned to the product.

    **Note:** Remote products have pre-determined dedicated template names, always prefixed with \&quot;remote.\&quot; This allows them to be managed independently of regular product templates. E.g. \&quot;remote.seller\&quot;

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the product.

    **Note:** In a cart or search suggestions context, the title of a remote product is appended with \&quot;Sold by {store name}\&quot;.

  * **type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The type of the product.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative URL of the product.

    If a product is rendered in search results or a filtered collection, then the URL contains the `variant` parameter of the most relevant variant.

    ```liquid
    /products/gorgeous-wooden-computer?variant=1234567890
    ```

    ```liquid
    /products/gorgeous-wooden-computer?variant=1234567890
    ```

    If a product is rendered as a [product recommendation](https://shopify.dev/docs/themes/product-merchandising/recommendations), then the URL contains tracking parameters:

    ```liquid
    /products/gorgeous-wooden-computer?pr_choice=default&pr_prod_strat=description&pr_rec_pid=13&pr_ref_pid=17&pr_seq=alternating
    ```

    ```liquid
    /products/gorgeous-wooden-computer?pr_choice=default&pr_prod_strat=description&pr_rec_pid=13&pr_ref_pid=17&pr_seq=alternating
    ```

  * **variants**

    array of [variant](https://shopify.dev/docs/api/liquid/objects/variant)

  * The variants of the product.

    **Note:** Returns a maximum of 250 variants when unpaginated. Tip: Use the \<a href="/docs/api/liquid/tags/paginate">paginate\</a> tag to choose how many variants to show per page, up to a limit of 50.

  * **variants\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of variants for the product.

  * **vendor**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The vendor of the product.

### Directly accessible in

* [product](https://shopify.dev/themes/architecture/templates/product)

### Returned by

* [collection.products](https://shopify.dev/docs/api/liquid/objects/collection#collection-products)
* [line\_​item.product](https://shopify.dev/docs/api/liquid/objects/line_item#line_item-product)
* [search.results](https://shopify.dev/docs/api/liquid/objects/search#search-results)
* [variant.product](https://shopify.dev/docs/api/liquid/objects/variant#variant-product)

## Templates using remote\_product

[Theme architecture](https://shopify.dev/themes/architecture/templates/product)

[product template](https://shopify.dev/themes/architecture/templates/product)

</page>

<page>
---
title: 'Liquid objects: remote_shop'
description: Information about a remote store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/remote_shop'
  md: 'https://shopify.dev/docs/api/liquid/objects/remote_shop.md'
---

# remote\_​shop

Information about a remote store.

Remote store information is only present via remote details, if the product comes from a remote source (i.e. a product from another store).

## Properties

* * **brand**

    [brand](https://shopify.dev/docs/api/liquid/objects/brand)

  * The [brand assets](https://help.shopify.com/manual/promoting-marketing/managing-brand-assets) for the remote store.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the remote store.

  * **policies**

    array of [policy](https://shopify.dev/docs/api/liquid/objects/policy)

  * The shipping and refund policies for the remote store.

    The policies are set in the remote store's [Policies settings](https://www.shopify.com/admin/settings/legal).

  * **refund\_​policy**

    [policy](https://shopify.dev/docs/api/liquid/objects/policy)

  * The refund policy for the remote store.

  * **shipping\_​policy**

    [policy](https://shopify.dev/docs/api/liquid/objects/policy)

  * The shipping policy for the remote store.

### Returned by

* [remote\_​product.remote\_​details](https://shopify.dev/docs/api/liquid/objects/remote_product#remote_product-remote_details)
* [remote\_​details.shop](https://shopify.dev/docs/api/liquid/objects/remote_details#remote_details-shop)

</page>

<page>
---
title: 'Liquid objects: request'
description: Information about the current URL and the associated page.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/request'
  md: 'https://shopify.dev/docs/api/liquid/objects/request.md'
---

# request

Information about the current URL and the associated page.

## Properties

* * **design\_​mode**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the request is being made from within the theme editor. Returns `false` if not.

    You can use `request.design_mode` to control theme behavior depending on whether the theme is being viewed in the editor. For example, you can prevent session data from being tracked by tracking scripts in the theme editor.

    **Caution:** You shouldn\&#39;t use \<code>\<span class="PreventFireFoxApplyingGapToWBR">request.design\<wbr/>\_mode\</span>\</code> to change customer-facing functionality. The theme editor preview should match what the merchant\&#39;s customers see on the live store.

  * **host**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The domain that the request is hosted on.

  * **locale**

    [shop\_locale](https://shopify.dev/docs/api/liquid/objects/shop_locale)

  * The locale of the request.

  * **origin**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The protocol and host of the request.

    ExampleCreate a context-aware absolute URL

    You can use `request.origin` with any object, object property, or filter that returns a relative URL to build a context-aware absolute URL.

    ##### Code

    ```liquid
    {{ product.selected_variant.url | default: product.url | prepend: request.origin }}
    ```

    ##### Data

    ```json
    {
      "product": {
        "selected_variant": null,
        "url": "/products/health-potion"
      },
      "request": {
        "origin": "https://polinas-potent-potions.myshopify.com"
      }
    }
    ```

    ##### Output

    ```html
    https://polinas-potent-potions.myshopify.com/products/health-potion
    ```

  * **page\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of page being requested.

    | Possible values |
    | - |
    | 404 |
    | article |
    | blog |
    | captcha |
    | cart |
    | collection |
    | list-collections |
    | customers/account |
    | customers/activate\_account |
    | customers/addresses |
    | customers/login |
    | customers/order |
    | customers/register |
    | customers/reset\_password |
    | gift\_card |
    | index |
    | metaobject |
    | page |
    | password |
    | policy |
    | product |
    | search |

  * **path**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The path of the request.

    **Note:** If the current path is for a page that doesn\&#39;t exist, then \<code>nil\</code> is returned.

  * **visual\_​preview\_​mode**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the request is being made from within the theme editor's visual section preview. Returns `false` if not.

    You can use `request.visual_preview_mode` to control theme behavior depending on whether the theme is being viewed in the editor's visual section preview. For example, you can remove any scripts that interefere with how the section is displayed.

##### Example

```json
{
  "design_mode": false,
  "host": "polinas-potent-potions.myshopify.com",
  "locale": {},
  "origin": "https://polinas-potent-potions.myshopify.com",
  "page_type": "index",
  "path": "/",
  "visual_preview_mode": false
}
```

</page>

<page>
---
title: 'Liquid objects: robots'
description: The default rule groups for the `robots.txt` file.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/robots'
  md: 'https://shopify.dev/docs/api/liquid/objects/robots.md'
---

# robots

The default rule groups for the `robots.txt` file.

***

**Tip:** You can \<a href="/themes/seo/robots-txt">customize the \<code>robots.txt\</code> file\</a> with the \<a href="/themes/architecture/templates/robots-txt-liquid">\<code>robots.txt.liquid\</code> template\</a>.

***

## Properties

* * **default\_​groups**

    array of [group](https://shopify.dev/docs/api/liquid/objects/group)

  * The rule groups.

##### Example

```json
{
  "default_groups": []
}
```

## Templates using robots

[Theme architecture](https://shopify.dev/themes/architecture/templates/robots-txt-liquid)

[robots.txt.liquid template](https://shopify.dev/themes/architecture/templates/robots-txt-liquid)

</page>

<page>
---
title: 'Liquid objects: routes'
description: Allows you to generate standard URLs for the storefront.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/routes'
  md: 'https://shopify.dev/docs/api/liquid/objects/routes.md'
---

# routes

Allows you to generate standard URLs for the storefront.

Using the `routes` object instead of hardcoding URLs helps ensure that your theme supports [multiple languages](https://shopify.dev/themes/internationalization/multiple-currencies-languages), as well as any possible changes in URL format.

## Properties

* * **account\_​addresses\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [account addresses page](https://shopify.dev/themes/architecture/templates/customers-addresses) URL. Redirects to [customer accounts](https://help.shopify.com/en/manual/customers/customer-accounts/new-customer-accounts) when enabled.

  * **account\_​login\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [account login page](https://shopify.dev/themes/architecture/templates/customers-login) URL. Redirects to [customer accounts](https://help.shopify.com/en/manual/customers/customer-accounts/new-customer-accounts) when enabled.

  * **account\_​logout\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL to log a customer out of their account. Redirects to [customer accounts](https://help.shopify.com/en/manual/customers/customer-accounts/new-customer-accounts) when enabled.

  * **account\_​profile\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the [customer accounts](https://help.shopify.com/en/manual/customers/customer-accounts/new-customer-accounts) profile page.

  * **account\_​recover\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [password recovery page](https://shopify.dev/themes/architecture/templates/customers-reset-password) URL. Redirects to [customer accounts](https://help.shopify.com/en/manual/customers/customer-accounts/new-customer-accounts) when enabled.

  * **account\_​register\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [account registration page](https://shopify.dev/themes/architecture/templates/customers-register) URL.

  * **account\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [account page](https://help.shopify.com/manual/customers/customer-accounts) URL. Redirects to [customer accounts](https://help.shopify.com/en/manual/customers/customer-accounts/new-customer-accounts) when enabled.

  * **all\_​products\_​collection\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The all-products collection page URL.

    The all-products collection is automatically generated by Shopify and contains all products in the store.

  * **cart\_​add\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the [`/cart/add` Cart API endpoint](https://shopify.dev/api/ajax/reference/cart#post-locale-cart-add-js).

  * **cart\_​change\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the [`/cart/change` Cart API endpoint](https://shopify.dev/api/ajax/reference/cart#post-locale-cart-change-js).

  * **cart\_​clear\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the [`/cart/clear` Cart API endpoint](https://shopify.dev/api/ajax/reference/cart#post-locale-cart-clear-js).

  * **cart\_​update\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the [`/cart/update` Cart API endpoint](https://shopify.dev/api/ajax/reference/cart#post-locale-cart-update-js).

  * **cart\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [cart page](https://shopify.dev/themes/architecture/templates/cart) URL.

  * **collections\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [collection list page](https://shopify.dev/themes/architecture/templates/list-collections) URL.

  * **predictive\_​search\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [Predictive Search API](https://shopify.dev/api/ajax/reference/predictive-search) URL.

    **Tip:** To learn about how to support predictive search in your theme, refer to \<a href="/themes/navigation-search/search/predictive-search">Add predictive search to your theme\</a>.

  * **product\_​recommendations\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [Product Recommendations API](https://shopify.dev/api/ajax/reference/product-recommendations) URL.

  * **root\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The index (home page) URL.

  * **search\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [search page](https://shopify.dev/themes/architecture/templates/search) URL.

  * **storefront\_​login\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * [Customer accounts](https://help.shopify.com/en/manual/customers/customer-accounts/new-customer-accounts) login page. Redirects to the storefront page the customer was on before visiting the login page.

##### Example

```json
{
  "account_addresses_url": "/account/addresses",
  "account_login_url": "/account/login",
  "account_logout_url": "/account/logout",
  "account_profile_url": "https://shopify.com/56174706753/account/profile?locale=en&region_country=CA&buyer_flags=eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb2xpbmFzLXBvdGVudC1wb3Rpb25zLm15c2hvcGlmeS5jb20iLCJmbGFncyI6W10sImV4cCI6MTc3ODczMjI3MywibmJmIjoxNzc4MTI3NDczfQ.ji08F3ubpXYWaKsSln7jrgLVAbqF66gtsvz9bfFNRnU",
  "account_recover_url": "/account/recover",
  "account_register_url": "/account/register",
  "account_url": "/account",
  "all_products_collection_url": "/collections/all",
  "cart_add_url": "/cart/add",
  "cart_change_url": "/cart/change",
  "cart_clear_url": "/cart/clear",
  "cart_update_url": "/cart/update",
  "cart_url": "/cart",
  "collections_url": "/collections",
  "predictive_search_url": "/search/suggest",
  "product_recommendations_url": "/recommendations/products",
  "root_url": "/",
  "search_url": "/search",
  "storefront_login_url": "/customer_authentication/login?return_to=%2Fservices%2Fliquid_rendering%2Fresource%3Ffast_storefront_renderer%3D1&locale=en&ui_hint=full"
}
```

</page>

<page>
---
title: 'Liquid objects: rule'
description: >-
  A rule for the `robots.txt` file, which tells crawlers which pages can, or
  can't, be accessed.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/rule'
  md: 'https://shopify.dev/docs/api/liquid/objects/rule.md'
---

# rule

A rule for the `robots.txt` file, which tells crawlers which pages can, or can't, be accessed.

A rule consists of a directive, which can be either `Allow` or `Disallow`, and a value of the associated URL path.

For example:

```
Disallow: /policies/
```

You can output a rule directly, instead of referencing each of its properties.

***

**Tip:** You can \<a href="/themes/seo/robots-txt">customize the \<code>robots.txt\</code> file\</a> with the \<a href="/themes/architecture/templates/robots-txt-liquid">\<code>robots.txt.liquid\</code> template\</a>.

***

## Properties

* * **directive**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The directive of the rule.

  * **value**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The value of the rule.

##### Example

```json
{
  "directive": "Disallow",
  "value": "/*preview_script_id*"
}
```

</page>

<page>
---
title: 'Liquid objects: script'
description: >-
  Information about a Shopify Script.

  > Caution:

  > Shopify Scripts will be sunset on August 28, 2025. Migrate your existing
  scripts to [Shopify Functions](/docs/api/functions) before this date.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/script'
  md: 'https://shopify.dev/docs/api/liquid/objects/script.md'
---

# script

Information about a Shopify Script.

***

**Caution:** Shopify Scripts will be sunset on August 28, 2025. Migrate your existing scripts to \<a href="/docs/api/functions">Shopify Functions\</a> before this date.

***

***

**Tip:** To learn more about Shopify Scripts and the Script Editor, visit the \<a href="https://help.shopify.com/manual/checkout-settings/script-editor">Shopify Help Center\</a>.

***

## Properties

* * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the script.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the script.

##### Example

```json
{
  "id": 209584193,
  "name": "10% off Whole bloodroot"
}
```

</page>

<page>
---
title: 'Liquid objects: scripts'
description: >-
  The active scripts, of each script type, on the store.

  > Caution:

  > Shopify Scripts will be sunset on August 28, 2025. Migrate your existing
  scripts to [Shopify Functions](/docs/api/functions) before this date.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/scripts'
  md: 'https://shopify.dev/docs/api/liquid/objects/scripts.md'
---

# scripts

The active scripts, of each script type, on the store.

***

**Caution:** Shopify Scripts will be sunset on August 28, 2025. Migrate your existing scripts to \<a href="/docs/api/functions">Shopify Functions\</a> before this date.

***

There can be only one active script of each type. Currently, the only type accessible in Liquid is `cart_calculate_line_items`.

***

**Tip:** To learn more about Shopify Scripts and the Script Editor, visit the \<a href="https://help.shopify.com/manual/checkout-settings/script-editor">Shopify Help Center\</a>.

***

## Properties

* * **cart\_​calculate\_​line\_​items**

    [script](https://shopify.dev/docs/api/liquid/objects/script)

  * The active line item script.

    If no line item script is currently active, then `nil` is returned.

    ExampleAdvertise the currently active line item script

    ##### Code

    ```liquid
    {% if scripts.cart_calculate_line_items %}
      <p>Don't miss our current sale: <strong>{{ scripts.cart_calculate_line_items.name }}</strong></p>
    {% endif %}
    ```

    ##### Data

    ```json
    {
      "scripts": {
        "cart_calculate_line_items": {
          "name": "10% off Whole bloodroot"
        }
      }
    }
    ```

    ##### Output

    ```html
    <p>Don't miss our current sale: <strong>10% off Whole bloodroot</strong></p>
    ```

##### Example

```json
{
  "cart_calculate_line_items": {}
}
```

</page>

<page>
---
title: 'Liquid objects: search'
description: Information about a storefront search query.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/search'
  md: 'https://shopify.dev/docs/api/liquid/objects/search.md'
---

# search

Information about a storefront search query.

To learn about storefront search and how to include it in your theme, refer to [Storefront search](https://shopify.dev/themes/navigation-search/search).

## Properties

* * **default\_​sort\_​by**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The default sort order of the search results, which is `relevance`.

  * **filters**

    array of [filter](https://shopify.dev/docs/api/liquid/objects/filter)

  * The filters that have been set up on the search page.

    Only filters that are relevant to the current search results are returned. If the search results contain more than 1000 products, then the array will be empty.

    **Tip:** To learn about how to set up filters in the admin, visit the \<a href="https://help.shopify.com/manual/online-store/themes/customizing-themes/storefront-filters">Shopify Help Center\</a>.

  * **performed**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if a search was successfully performed. Returns `false` if not.

  * **results**

  * The search result items.

    An item can be an [`article`](https://shopify.dev/docs/api/liquid/objects/article), a [`page`](https://shopify.dev/docs/api/liquid/objects/page), or a [`product`](https://shopify.dev/docs/api/liquid/objects/product).

    **Tip:** Use the \<a href="/docs/api/liquid/tags/paginate">paginate\</a> tag to choose how many results to show per page, up to a limit of 50.

    ExampleSearch result `object_type`

    Search results have an additional `object_type` property that returns the object type of the result.

    ##### Code

    ```liquid
    {% for item in search.results %}
    <!-- Result {{ forloop.index }}-->
    <h3>
      {{ item.title | link_to: item.url }}
    </h3>

    {% if item.object_type == 'article' -%}
      {%- comment -%}
         'item' is an article
         All article object properties can be accessed.
      {%- endcomment -%}

      {% if item.image -%}
        <div class="result-image">
          <a href="{{ item.url }}" title="{{ item.title | escape }}">
            {{ item | image_url: width: 100 | image_tag }}
           </a>
        </div>
       {% endif %}
    {%- elsif item.object_type == 'page' -%}
      {%- comment -%}
        'item' is a page.
         All page object properties can be accessed.
      {%- endcomment -%}
    {%- else -%}
      {%- comment -%}
         'item' is a product.
         All product object properties can be accessed.
      {%- endcomment -%}

      {%- if item.featured_image -%}
        <div class="result-image">
           <a href="{{ item.url }}" title="{{ item.title | escape }}">
             {{ item.featured_image | image_url: width: 100 | image_tag }}
          </a>
        </div>
      {% endif %}
    {%- endif -%}

    <span>{{ item.content | strip_html | truncatewords: 40 | highlight: search.terms }}</span>
    {% endfor %}
    ```

  * **results\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of results.

  * **sort\_​by**

  * The sort order of the search results. This is determined by the `sort_by` URL parameter.

    If there's no `sort_by` URL parameter, then the value is `nil`.

  * **sort\_​options**

    array of [sort\_option](https://shopify.dev/docs/api/liquid/objects/sort_option)

  * The available sorting options for the search results.

    ExampleOutput the sort options

    ##### Code

    ```liquid
    {%- assign sort_by = search.sort_by | default: search.default_sort_by -%}

    <select>
    {%- for option in search.sort_options %}
      <option
        value="{{ option.value }}"
        {%- if option.value == sort_by %}
          selected="selected"
        {%- endif %}
      >
        {{ option.name }}
      </option>
    {% endfor -%}
    </select>
    ```

  * **terms**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The entered search terms.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/highlight">\<code>highlight\</code> filter\</a> to highlight the search terms in search result content.

  * **types**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The object types that the search was performed on.

    A search can be performed on the following object types:

    * [`article`](https://shopify.dev/docs/api/liquid/objects/article)
    * [`page`](https://shopify.dev/docs/api/liquid/objects/page)
    * [`product`](https://shopify.dev/docs/api/liquid/objects/product)

    **Note:** The types are determined by the \<a href="/api/ajax/reference/predictive-search#query-parameters">\<code>type\</code> query parameter\</a>.

##### Example

```json
{
  "default_sort_by": "relevance",
  "filters": {},
  "performed": true,
  "results": [],
  "results_count": 16,
  "sort_by": "relevance",
  "sort_options": [],
  "terms": "potion",
  "types": [
    "article",
    "page",
    "product"
  ]
}
```

## Templates using search

[Theme architecture](https://shopify.dev/themes/architecture/templates/search)

[search template](https://shopify.dev/themes/architecture/templates/search)

</page>

<page>
---
title: 'Liquid objects: section'
description: The properties and settings of a section.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/section'
  md: 'https://shopify.dev/docs/api/liquid/objects/section.md'
---

# section

The properties and settings of a section.

***

**Tip:** To learn about sections and using them in a theme, refer to \<a href="/themes/architecture/sections">Sections\</a>.

***

## Properties

* * **blocks**

    array of [block](https://shopify.dev/docs/api/liquid/objects/block)

  * The blocks of the section.

  * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the section.

    The ID for sections included through [JSON templates](https://shopify.dev/themes/architecture/templates/json-templates) are dynamically generated by Shopify.

    The ID for static sections is the section file name without the `.liquid` extension. For example, a `header.liquid` section has an ID of `header`.

  * **index**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current section within its location.

    Use this property to adjust section behavior based on its position within its location ([template](https://shopify.dev/docs/themes/architecture/templates), [section group](https://shopify.dev/docs/themes/architecture/section-groups)) and on the page. The `index` starts at 1 within each location.

    An example use case is for programmatically setting `loading="lazy"` for images below the fold based on an index higher than, for example, 3. Note that this is now the default behavior for the [`image_tag` filter](https://shopify.dev/docs/api/liquid/filters#image_tag).

    Only use this for non-display use cases like web performance. Because of various limitations, the `index` property returns `nil` in the following contexts:

    * When rendered as a [static section](https://shopify.dev/docs/themes/architecture/sections#statically-render-a-section)
    * While rendering in the online store editor
    * When using the [Section Rendering API](https://shopify.dev/docs/api/section-rendering)

  * **index0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current section within its location.

    This is the same as the `index` property except that the index starts at 0 instead of 1.

  * **location**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The scope or context of the section (template, section group, or global).

    Sections can have one of four different location types. For sections rendered within a [template](https://shopify.dev/docs/themes/architecture/templates), the location will be `template`. For sections rendered within a [section group](https://shopify.dev/docs/themes/architecture/section-groups), the location will be the section group type, e.g., `header`, `footer`, `custom.<type>`. Sections [rendered statically](https://shopify.dev/docs/themes/architecture/sections#statically-render-a-section) will be `static`. Finally, if you're still using `content_for_index`, then the value will be `content_for_index`.

  * **settings**

  * The [settings](https://shopify.dev/themes/architecture/sections/section-schema#settings) of the section.

    To learn about how to access settings, refer to [Access settings](https://shopify.dev/themes/architecture/settings#access-settings).

##### Example

```json
{
  "blocks": [],
  "id": "template--14453298921537__cart-items",
  "settings": {}
}
```

</page>

<page>
---
title: 'Liquid objects: selling_plan'
description: >-
  Information about the intent of how a specific [selling
  plan](/apps/subscriptions/selling-plans) affects a line item.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/selling_plan'
  md: 'https://shopify.dev/docs/api/liquid/objects/selling_plan.md'
---

# selling\_​plan

Information about the intent of how a specific [selling plan](https://shopify.dev/apps/subscriptions/selling-plans) affects a line item.

To learn about how to support selling plans in your theme, refer to [Purchase options](https://shopify.dev/themes/pricing-payments/purchase-options).

## Properties

* * **checkout\_​charge**

    [selling\_plan\_checkout\_charge](https://shopify.dev/docs/api/liquid/objects/selling_plan_checkout_charge)

  * The checkout charge of the selling plan.

  * **description**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The description of the selling plan.

  * **group\_​id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the [`selling_plan_group`](https://shopify.dev/docs/api/liquid/objects/selling_plan_group) that the selling plan belongs to.

    **Note:** The name is shown at checkout with the line item summary.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the selling plan.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the selling plan.

    **Note:** The name is shown at checkout with the line item summary.

  * **options**

    array of [selling\_plan\_option](https://shopify.dev/docs/api/liquid/objects/selling_plan_option)

  * The selling plan options.

  * **price\_​adjustments**

    array of [selling\_plan\_price\_adjustment](https://shopify.dev/docs/api/liquid/objects/selling_plan_price_adjustment)

  * The selling plan price adjustments.

    The maximum length of the array is two. If the selling plan doesn't create any price adjustments, then the array is empty.

    Each `selling_plan_price_adjustment` maps to a [`selling_plan_allocation_price_adjustment`](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation_price_adjustment) in the [`selling_plan_allocation.price_adjustments` array](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation#selling_plan_allocation-price_adjustments). The `selling_plan.price_adjustments` array contains the intent of the selling plan, and the `selling_plan_allocation.price_adjustments` contains the resulting money amounts.

  * **recurring\_​deliveries**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the selling plan includes multiple deliveries. Returns `false` if not.

  * **selected**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the selling plan is currently selected. Returns `false` if not.

    **Note:** The selected selling plan is determined by the \<code>\<span class="PreventFireFoxApplyingGapToWBR">selling\<wbr/>\_plan\</span>\</code> URL parameter.

##### Example

```json
{
  "checkout_charge": {},
  "description": null,
  "group_id": "e88ff8fdb3c39c89b564859e34542e0b982076d6",
  "id": 2595487809,
  "name": "Deliver every week, 10% off",
  "options": [],
  "price_adjustments": [],
  "recurring_deliveries": true,
  "selected": true
}
```

</page>

<page>
---
title: 'Liquid objects: selling_plan_allocation'
description: >-
  Information about how a specific [selling
  plan](/apps/subscriptions/selling-plans) affects a line item.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation'
  md: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation.md'
---

# selling\_​plan\_​allocation

Information about how a specific [selling plan](https://shopify.dev/apps/subscriptions/selling-plans) affects a line item.

To learn about how to support selling plans in your theme, refer to [Purchase options](https://shopify.dev/themes/pricing-payments/purchase-options).

## Properties

* * **checkout\_​charge\_​amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The amount that the customer will be charged at checkout in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **compare\_​at\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The **compare at** price of the selling plan allocation in the currency's subunit.

    The value of the **compare at** price is the line item's price without the selling plan applied.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **per\_​delivery\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The price for each delivery in the selling plan in the currency's subunit.

    If a selling plan includes multiple deliveries, then the `per_delivery_price` is the `price` divided by the number of deliveries.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The price of the selling plan allocation in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **price\_​adjustments**

    array of [selling\_plan\_allocation\_price\_adjustment](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation_price_adjustment)

  * The selling plan allocation price adjustments.

    The maximum length of the array is two. If the associated selling plan doesn't create any price adjustments, then the array is empty.

    Each `selling_plan_allocation_price_adjustment` maps to a [`selling_plan_price_adjustment`](https://shopify.dev/docs/api/liquid/objects/selling_plan_price_adjustment) in the [`selling_plan.price_adjustments` array](https://shopify.dev/docs/api/liquid/objects/selling_plan#selling_plan-price_adjustments). The `selling_plan.price_adjustments` array contains the intent of the selling plan, and the `selling_plan_allocation.price_adjustments` array contains the resulting money amounts.

  * **remaining\_​balance\_​charge\_​amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The remaining amount for the customer to pay, in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **selling\_​plan**

    [selling\_plan](https://shopify.dev/docs/api/liquid/objects/selling_plan)

  * The selling plan that created the allocation.

  * **selling\_​plan\_​group\_​id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the [`selling_plan_group`](https://shopify.dev/docs/api/liquid/objects/selling_plan_group) that the selling plan of the allocation belongs to.

  * **unit\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The [unit price](https://shopify.dev/docs/api/liquid/objects/variant#variant-unit_price) of the variant associated with the selling plan, in the currency's subunit.

    If the variant doesn't have a unit price, then `nil` is returned.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

### Returned by

* [line\_​item.selling\_​plan\_​allocation](https://shopify.dev/docs/api/liquid/objects/line_item#line_item-selling_plan_allocation)
* [variant.selling\_​plan\_​allocations](https://shopify.dev/docs/api/liquid/objects/variant#variant-selling_plan_allocations)
* [product.selected\_​selling\_​plan\_​allocation](https://shopify.dev/docs/api/liquid/objects/product#product-selected_selling_plan_allocation)
* [product.selected\_​or\_​first\_​available\_​selling\_​plan\_​allocation](https://shopify.dev/docs/api/liquid/objects/product#product-selected_or_first_available_selling_plan_allocation)
* [variant.selected\_​selling\_​plan\_​allocation](https://shopify.dev/docs/api/liquid/objects/variant#variant-selected_selling_plan_allocation)
* [remote\_​product.selected\_​selling\_​plan\_​allocation](https://shopify.dev/docs/api/liquid/objects/remote_product#remote_product-selected_selling_plan_allocation)
* [remote\_​product.selected\_​or\_​first\_​available\_​selling\_​plan\_​allocation](https://shopify.dev/docs/api/liquid/objects/remote_product#remote_product-selected_or_first_available_selling_plan_allocation)

</page>

<page>
---
title: 'Liquid objects: selling_plan_allocation_price_adjustment'
description: >-
  The resulting price from the intent of the associated
  [`selling_plan_price_adjustment`](/docs/api/liquid/objects/selling_plan_price_adjustment).
api_name: liquid
source_url:
  html: >-
    https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation_price_adjustment
  md: >-
    https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation_price_adjustment.md
---

# selling\_​plan\_​allocation\_​price\_​adjustment

The resulting price from the intent of the associated [`selling_plan_price_adjustment`](https://shopify.dev/docs/api/liquid/objects/selling_plan_price_adjustment).

To learn about how to support selling plans in your theme, refer to [Purchase options](https://shopify.dev/themes/pricing-payments/purchase-options).

## Properties

* * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the price adjustment in the [`selling_plan_allocation.price_adjustments` array](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation#selling_plan_allocation-price_adjustments).

  * **price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The price that will be charged for the price adjustment's lifetime, in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

### Returned by

* [selling\_​plan\_​allocation.price\_​adjustments](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation#selling_plan_allocation-price_adjustments)

</page>

<page>
---
title: 'Liquid objects: selling_plan_checkout_charge'
description: >-
  Information about how a specific [selling
  plan](/apps/subscriptions/selling-plans) affects the amount that a

  customer needs to pay for a line item at checkout.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_checkout_charge'
  md: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_checkout_charge.md'
---

# selling\_​plan\_​checkout\_​charge

Information about how a specific [selling plan](https://shopify.dev/apps/subscriptions/selling-plans) affects the amount that a customer needs to pay for a line item at checkout.

To learn about how to support selling plans in your theme, refer to [Purchase options](https://shopify.dev/themes/pricing-payments/purchase-options).

## Properties

* * **value**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The value of the checkout charge.

    How this value is interpreted depends on the [value type](https://shopify.dev/docs/api/liquid/objects/selling_plan_checkout_charge#selling_plan_checkout_charge-value_type) of the checkout charge. The following table outlines what the value represents for each value type:

    | Value type | Value |
    | - | - |
    | `percentage` | The percent amount of the original price that the customer needs to pay. For example, if the value is 50, then the customer needs to pay 50% of the original price. |
    | `price` | The amount that the customer needs to pay in the currency's subunit. |

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **value\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The value type of the checkout charge.

    | Possible values |
    | - |
    | percentage |
    | price |

##### Example

```json
{
  "value": 100,
  "value_type": "percentage"
}
```

</page>

<page>
---
title: 'Liquid objects: selling_plan_group'
description: >-
  Information about a specific group of [selling
  plans](/apps/subscriptions/selling-plans) that include any of a

  product's variants.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_group'
  md: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_group.md'
---

# selling\_​plan\_​group

Information about a specific group of [selling plans](https://shopify.dev/apps/subscriptions/selling-plans) that include any of a product's variants.

Selling plans are grouped based on shared [selling plan option names](https://shopify.dev/docs/api/liquid/objects/selling_plan_option#selling_plan_option-name).

To learn about how to support selling plans in your theme, refer to [Purchase options](https://shopify.dev/themes/pricing-payments/purchase-options).

## Properties

* * **app\_​id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * An optional string provided by an app to identify selling plan groups created by that app.

    If the app doesn't provide a value, then `nil` is returned.

    **Tip:** You can use this property, with the \<a href="/docs/api/liquid/filters/where">\<code>where\</code> filter\</a>, to filter the \<a href="/docs/api/liquid/objects/product#product-selling\_plan\_groups">\<code>\<span class="PreventFireFoxApplyingGapToWBR">product.selling\<wbr/>\_plan\<wbr/>\_groups\</span>\</code> array\</a> for all selling plan groups from a specific app.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the selling plan group.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the selling plan group.

  * **options**

    array of [selling\_plan\_group\_option](https://shopify.dev/docs/api/liquid/objects/selling_plan_group_option)

  * The selling plan group options.

  * **selling\_​plan\_​selected**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the currently selected selling plan is part of the selling plan group. Returns `false` if not.

    **Note:** The selected selling plan is determined by the \<code>\<span class="PreventFireFoxApplyingGapToWBR">selling\<wbr/>\_plan\</span>\</code> URL parameter.

  * **selling\_​plans**

    array of [selling\_plan](https://shopify.dev/docs/api/liquid/objects/selling_plan)

  * The selling plans in the group.

##### Example

```json
{
  "app_id": "gid://shopify/App/66228322305",
  "id": "e88ff8fdb3c39c89b564859e34542e0b982076d6",
  "name": "1 Week(s), 4 Week(s)",
  "options": [],
  "selling_plan_selected": false,
  "selling_plans": []
}
```

</page>

<page>
---
title: 'Liquid objects: selling_plan_group_option'
description: >-
  Information about a specific option in a [selling plan
  group](/docs/api/liquid/objects/selling_plan_group).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_group_option'
  md: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_group_option.md'
---

# selling\_​plan\_​group\_​option

Information about a specific option in a [selling plan group](https://shopify.dev/docs/api/liquid/objects/selling_plan_group).

## Properties

* * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the option.

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the option in the [`selling_plan_group.options` array](https://shopify.dev/docs/api/liquid/objects/selling_plan_group#selling_plan_group-options).

  * **selected\_​value**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The option value of the currently selected selling plan.

    If no selling plan is currently selected, then `nil` is returned.

    **Note:** The selected selling plan is determined by the \<code>\<span class="PreventFireFoxApplyingGapToWBR">selling\<wbr/>\_plan\</span>\</code> URL parameter.

  * **values**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The values of the option.

##### Example

```json
{
  "name": "Delivery frequency",
  "position": 1,
  "selected_value": null,
  "values": [
    "Deliver every week",
    "Deliver every 4 weeks"
  ]
}
```

</page>

<page>
---
title: 'Liquid objects: selling_plan_option'
description: >-
  Information about a selling plan's value for a specific
  [`selling_plan_group_option`](/docs/api/liquid/objects/selling_plan_group_option).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_option'
  md: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_option.md'
---

# selling\_​plan\_​option

Information about a selling plan's value for a specific [`selling_plan_group_option`](https://shopify.dev/docs/api/liquid/objects/selling_plan_group_option).

To learn about how to support selling plans in your theme, refer to [Purchase options](https://shopify.dev/themes/pricing-payments/purchase-options).

## Properties

* * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the associated `selling_plan_group_option`.

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the selling plan option in the associated [`selling_plan_group.options` array](https://shopify.dev/docs/api/liquid/objects/selling_plan_group#selling_plan_group-options).

  * **value**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The value of the selling plan option.

    The value is one of the [`selling_plan_group_option.values`](https://shopify.dev/docs/api/liquid/objects/selling_plan_group_option#selling_plan_group_option-values).

##### Example

```json
{
  "name": "Delivery frequency",
  "position": 1,
  "value": "Deliver every week"
}
```

</page>

<page>
---
title: 'Liquid objects: selling_plan_price_adjustment'
description: >-
  Information about how a selling plan changes the price of a variant for a
  given period of time.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_price_adjustment'
  md: 'https://shopify.dev/docs/api/liquid/objects/selling_plan_price_adjustment.md'
---

# selling\_​plan\_​price\_​adjustment

Information about how a selling plan changes the price of a variant for a given period of time.

To learn about how to support selling plans in your theme, refer to [Purchase options](https://shopify.dev/themes/pricing-payments/purchase-options).

## Properties

* * **order\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of orders that the price adjustment applies to.

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the price adjustment in the [`selling_plan.price_adjustments` array](https://shopify.dev/docs/api/liquid/objects/selling_plan#selling_plan-price_adjustments).

  * **value**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The value of the price adjustment as a decimal.

    How this value is interpreted depends on the [value type](https://shopify.dev/docs/api/liquid/objects/selling_plan_price_adjustment#selling_plan_price_adjustment-value_type) of the price adjustment. The following table outlines what the value represents for each value type:

    | Value type | Value |
    | - | - |
    | `fixed_amount` | The amount that the original price is being adjusted by, in the currency's subunit. |
    | `percentage` | The percent amount that the original price is being adjusted by. |
    | `price` | The adjusted amount in the currency's subunit. |

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **value\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of price adjustment.

    | Possible values |
    | - |
    | percentage |
    | fixed\_amount |
    | price |

##### Example

```json
{
  "order_count": null,
  "position": 1,
  "value": 10,
  "value_type": "percentage"
}
```

</page>

<page>
---
title: 'Liquid objects: settings'
description: >-
  Allows you to access all of the theme's settings from the
  [`settings_schema.json`
  file](/themes/architecture/config/settings-schema-json).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/settings'
  md: 'https://shopify.dev/docs/api/liquid/objects/settings.md'
---

# settings

Allows you to access all of the theme's settings from the [`settings_schema.json` file](https://shopify.dev/themes/architecture/config/settings-schema-json).

***

**Tip:** To learn about the available setting types, refer to \<a href="/themes/architecture/settings/input-settings">Input settings\</a>.

***

### Directly accessible in

* Global

### Reference a setting value

##### Code

```liquid
{% if settings.favicon != blank %}
  <link rel="icon" type="image/png" href="{{ settings.favicon | image_url: width: 32, height: 32 }}">
{% endif %}
```

##### Data

```json
{
  "settings": {
    "favicon": null
  }
}
```

</page>

<page>
---
title: 'Liquid objects: shipping_method'
description: Information about the shipping method for an order.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/shipping_method'
  md: 'https://shopify.dev/docs/api/liquid/objects/shipping_method.md'
---

# shipping\_​method

Information about the shipping method for an order.

## Properties

* * **discount\_​allocations**

    array of [discount\_allocation](https://shopify.dev/docs/api/liquid/objects/discount_allocation)

  * The discount allocations that apply to the shipping method.

  * **handle**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handle](https://shopify.dev/docs/api/liquid/basics#handles) of the shipping method.

    **Note:** The price of the shipping method is appended to handle.

  * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the shipping method.

  * **original\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The price of the shipping method in the currency's subunit, before discounts have been applied.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **price\_​with\_​discounts**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The price of the shipping method in the currency's subunit, after discounts have been applied, including order level discounts.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **tax\_​lines**

    array of [tax\_line](https://shopify.dev/docs/api/liquid/objects/tax_line)

  * The tax lines for the shipping method.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the shipping method.

    In most contexts, the shipping method title appears in the customer's preferred language. However, in the context of an [order](https://shopify.dev/docs/api/liquid/objects/order), the shipping method title appears in the language that the customer checked out in.

## Deprecated Properties

* * **price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

    Deprecated

  * The price of the shipping method in the currency's subunit, after discounts have been applied.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

    **Deprecated:**

    Deprecated because the price did not include order level discounts.

    The `shipping_line.price` property has been replaced by [`shipping_line.price_with_discounts`](https://shopify.dev/docs/api/liquid/objects/shipping_method#shipping_method-price_with_discounts).

##### Example

```json
{
  "handle": "shopify-Standard-0.00",
  "id": "shopify-Standard-0.00",
  "original_price": "0.00",
  "price": "0.00",
  "price_with_discounts": "0.00",
  "tax_lines": [],
  "title": "Standard"
}
```

</page>

<page>
---
title: 'Liquid objects: shop'
description: >-
  Information about the store, such as the store address, the total number of
  products, and various settings.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/shop'
  md: 'https://shopify.dev/docs/api/liquid/objects/shop.md'
---

# shop

Information about the store, such as the store address, the total number of products, and various settings.

## Properties

* * **accepts\_​gift\_​cards**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the store accepts gift cards. Returns `false` if not.

  * **address**

    [address](https://shopify.dev/docs/api/liquid/objects/address)

  * The address of the store.

  * **brand**

    [brand](https://shopify.dev/docs/api/liquid/objects/brand)

  * The [brand assets](https://help.shopify.com/manual/promoting-marketing/managing-brand-assets) for the store.

  * **collections\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of collections in the store.

  * **currency**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The currency of the store.

  * **customer\_​accounts\_​enabled**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the store shows a login link. Returns `false` if not.

  * **customer\_​accounts\_​optional**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if customer accounts are optional to complete checkout. Returns `false` if not.

  * **description**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [description](https://help.shopify.com/manual/online-store/setting-up/preferences#edit-the-title-and-meta-description) of the store.

  * **domain**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The primary domain of the store.

  * **email**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [sender email](https://help.shopify.com/manual/intro-to-shopify/initial-setup/setup-your-email#change-your-sender-email-address) of the store.

  * **enabled\_​currencies**

    array of [currency](https://shopify.dev/docs/api/liquid/objects/currency)

  * The currencies that the store accepts.

    **Tip:** You can get the store\&#39;s currency with \<a href="/docs/api/liquid/objects/shop#shop-currency">\<code>shop.currency\</code>\</a>.

  * **enabled\_​payment\_​types**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The accepted payment types on the store.

    The payment types are based on the store's enabled [payment providers](https://help.shopify.com/manual/payments) and the customer's current region and currency.

    **Tip:** You can output an \<code>svg\</code> logo for each payment type with the \<a href="/docs/api/liquid/filters/payment\_type\_svg\_tag">\<code>\<span class="PreventFireFoxApplyingGapToWBR">payment\<wbr/>\_type\<wbr/>\_svg\<wbr/>\_tag\</span>\</code> filter\</a>. Alternatively, you can get the source URL for each \<code>svg\</code> with the \<a href="/docs/api/liquid/filters/payment\_type\_img\_url">\<code>\<span class="PreventFireFoxApplyingGapToWBR">payment\<wbr/>\_type\<wbr/>\_img\<wbr/>\_url\</span>\</code> filter\</a>.

  * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ID of the store.

  * **metafields**

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the store.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **money\_​format**

    [currency](https://shopify.dev/docs/api/liquid/objects/currency)

  * The money format of the store.

  * **money\_​with\_​currency\_​format**

    [currency](https://shopify.dev/docs/api/liquid/objects/currency)

  * The money format of the store with the currency included.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the store.

  * **password\_​message**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The password page message of the store.

  * **permanent\_​domain**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The `.myshopify.com` domain of the store.

  * **phone**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The phone number of the store.

  * **policies**

    array of [policy](https://shopify.dev/docs/api/liquid/objects/policy)

  * The policies for the store.

    The policies are set in the store's [Policies settings](https://www.shopify.com/admin/settings/legal).

    ExampleOutput the policies

    ##### Code

    ```liquid
    <ul>
    {%- for policy in shop.policies %}
      <li>{{ policy.title }}</li>
    {%- endfor %}
    </ul>
    ```

    ##### Data

    ```json
    {
      "shop": {
        "policies": [
          "<p>We have a 30-day return policy, which means you have 30 days after receiving your item to request a return. ...</p>",
          "<p>This Privacy Policy describes how polinas-potent-potions.myshopify.com (the “Site” or “we”) collects, uses, and discloses your Personal Information when you visit or make a purchase from the Site. ...</p>",
          "<strong>OVERVIEW</strong> <br> This website is operated by Polina's Potent Potions. Throughout the site, the terms “we”, “us” and “our” refer to Polina's Potent Potions. ...",
          "<meta charset=\"utf-8\">\n<p data-mce-fragment=\"1\">All orders are processed within X to X business days (excluding weekends and holidays) after receiving your order confirmation email. You will receive another notification when your order has shipped. ...&nbsp;</p>\n<h3 data-mce-fragment=\"1\"></h3>"
        ]
      }
    }
    ```

    ##### Output

    ```html
    <ul>
      <li>Refund policy</li>
      <li>Privacy policy</li>
      <li>Terms of service</li>
      <li>Shipping policy</li>
    </ul>
    ```

  * **privacy\_​policy**

    [policy](https://shopify.dev/docs/api/liquid/objects/policy)

  * The privacy policy for the store.

  * **products\_​count**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The number of products in the store.

  * **published\_​locales**

    array of [shop\_locale](https://shopify.dev/docs/api/liquid/objects/shop_locale)

  * The locales (languages) that are published on the store.

  * **refund\_​policy**

    [policy](https://shopify.dev/docs/api/liquid/objects/policy)

  * The refund policy for the store.

  * **search\_​types**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The resource types searched for by default when no `type` parameter is specified.

    Search types are `["article", "page", "product"]` by default, and can be configured in the Search & Discovery app. These search types are used when a search query is performed without a `?type=` parameter.

    **Tip:** A search with a \<code>?type=\</code> parameter value not in the \<code>\<span class="PreventFireFoxApplyingGapToWBR">shop.search\<wbr/>\_types\</span>\</code> list will still return results for the specified type, if they exist.

  * **secure\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The full URL of the store, with an `https` protocol.

  * **shipping\_​policy**

    [policy](https://shopify.dev/docs/api/liquid/objects/policy)

  * The shipping policy for the store.

  * **subscription\_​policy**

    [policy](https://shopify.dev/docs/api/liquid/objects/policy)

  * The subscription policy for the store.

  * **terms\_​of\_​service**

    [policy](https://shopify.dev/docs/api/liquid/objects/policy)

  * The terms of service for the store.

  * **types**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * All of the product types in the store.

    ExampleOutput the product types

    ##### Code

    ```liquid
    {% for type in shop.types %}
      {{- type | link_to_type }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "shop": {
        "types": [
          "",
          "Animals & Pet Supplies",
          "Baking Flavors & Extracts",
          "Container",
          "Cooking & Baking Ingredients",
          "Dried Flowers",
          "Fruits & Vegetables",
          "Gift Cards",
          "Health",
          "Health & Beauty",
          "Invisibility",
          "Love",
          "Music & Sound Recordings",
          "Seasonings & Spices",
          "Water"
        ]
      }
    }
    ```

    ##### Output

    ```html
    Unknown Type

    <a href="/collections/types?q=Animals%20%26%20Pet%20Supplies" title="Animals &amp; Pet Supplies">Animals & Pet Supplies</a>

    <a href="/collections/types?q=Baking%20Flavors%20%26%20Extracts" title="Baking Flavors &amp; Extracts">Baking Flavors & Extracts</a>

    <a href="/collections/types?q=Container" title="Container">Container</a>

    <a href="/collections/types?q=Cooking%20%26%20Baking%20Ingredients" title="Cooking &amp; Baking Ingredients">Cooking & Baking Ingredients</a>

    <a href="/collections/types?q=Dried%20Flowers" title="Dried Flowers">Dried Flowers</a>

    <a href="/collections/types?q=Fruits%20%26%20Vegetables" title="Fruits &amp; Vegetables">Fruits & Vegetables</a>

    <a href="/collections/types?q=Gift%20Cards" title="Gift Cards">Gift Cards</a>

    <a href="/collections/types?q=Health" title="Health">Health</a>

    <a href="/collections/types?q=Health%20%26%20Beauty" title="Health &amp; Beauty">Health & Beauty</a>

    <a href="/collections/types?q=Invisibility" title="Invisibility">Invisibility</a>

    <a href="/collections/types?q=Love" title="Love">Love</a>

    <a href="/collections/types?q=Music%20%26%20Sound%20Recordings" title="Music &amp; Sound Recordings">Music & Sound Recordings</a>

    <a href="/collections/types?q=Seasonings%20%26%20Spices" title="Seasonings &amp; Spices">Seasonings & Spices</a>

    <a href="/collections/types?q=Water" title="Water">Water</a>
    ```

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The full URL of the store.

  * **vendors**

    array of [string](https://shopify.dev/docs/api/liquid/basics#string)

  * All of the product vendors for the store.

    ExampleOutput the vendors

    ##### Code

    ```liquid
    {% for vendor in shop.vendors %}
      {{- vendor | link_to_vendor }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "shop": {
        "vendors": [
          "Clover's Apothecary",
          "Polina's Potent Potions",
          "Ted's Apothecary Supply"
        ]
      }
    }
    ```

    ##### Output

    ```html
    <a href="/collections/vendors?q=Clover%27s%20Apothecary" title="Clover&#39;s Apothecary">Clover's Apothecary</a>

    <a href="/collections/vendors?q=Polina%27s%20Potent%20Potions" title="Polina&#39;s Potent Potions">Polina's Potent Potions</a>

    <a href="/collections/vendors?q=Ted%27s%20Apothecary%20Supply" title="Ted&#39;s Apothecary Supply">Ted's Apothecary Supply</a>
    ```

## Deprecated Properties

* * **enabled\_​locales**

    array of [shop\_locale](https://shopify.dev/docs/api/liquid/objects/shop_locale)

    Deprecated

  * The locales (languages) that are published on the store.

    **Deprecated:**

    Deprecated because the name didn't make it clear that the returned locales were published.

    The `shop.enabled_locales` property has been replaced by [`shop.published_locales`](https://shopify.dev/docs/api/liquid/objects/shop#shop-published_locales).

  * **locale**

    [shop\_locale](https://shopify.dev/docs/api/liquid/objects/shop_locale)

    Deprecated

  * The currently active locale (language).

    **Deprecated:**

    Deprecated because this value is contextual to the request and not a property of the shop resource.

    The `shop.locale` property has been replaced by [request.locale](https://shopify.dev/docs/api/liquid/objects/request#request-locale).

  * **metaobjects**

    Deprecated

  * All of the [metaobjects](https://shopify.dev/docs/api/liquid/objects/metaobject) of the store.

    **Deprecated:**

    The `shop.metaobjects` property has been replaced by [`metaobjects`](https://shopify.dev/docs/api/liquid/objects/metaobjects).

  * **taxes\_​included**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

    Deprecated

  * Returns `true` if prices include taxes. Returns `false` if not.

    **Deprecated:**

    Deprecated because whether or not prices have taxes included is dependent on the customer's country.

    The `shop.taxes_included` property has been replaced by [cart.taxes\_included](https://shopify.dev/docs/api/liquid/objects/cart#cart-taxes_included).

##### Example

```json
{
  "accepts_gift_cards": true,
  "address": {},
  "brand": {},
  "collections_count": 7,
  "currency": "CAD",
  "customer_accounts_enabled": true,
  "customer_accounts_optional": true,
  "description": "Canada's foremost retailer for potions and potion accessories. Try one of our award-winning artisanal potions, or find the supplies to make your own!",
  "domain": "polinas-potent-potions.myshopify.com",
  "email": "polinas.potent.potions@gmail.com",
  "enabled_currencies": [],
  "enabled_locales": [],
  "enabled_payment_types": [
    "visa",
    "master",
    "american_express",
    "paypal",
    "diners_club",
    "discover"
  ],
  "id": 56174706753,
  "locale": "en",
  "metafields": {},
  "metaobjects": {},
  "money_format": "${{amount}}",
  "money_with_currency_format": "${{amount}} CAD",
  "name": "Polina&#39;s Potent Potions",
  "password_message": "Our store will be opening when the moon is in the seventh house!!",
  "permanent_domain": "polinas-potent-potions.myshopify.com",
  "phone": "416-123-1234",
  "policies": [],
  "privacy_policy": {},
  "products_count": 19,
  "published_locales": [],
  "refund_policy": {},
  "search_types": [
    "article",
    "page",
    "product"
  ],
  "secure_url": "https://polinas-potent-potions.myshopify.com",
  "shipping_policy": {},
  "subscription_policy": null,
  "taxes_included": false,
  "terms_of_service": {},
  "types": [
    "",
    "Animals & Pet Supplies",
    "Baking Flavors & Extracts",
    "Container",
    "Cooking & Baking Ingredients",
    "Dried Flowers",
    "Fruits & Vegetables",
    "Gift Cards",
    "Health",
    "Health & Beauty",
    "Invisibility",
    "Love",
    "Music & Sound Recordings",
    "Seasonings & Spices",
    "Water"
  ],
  "url": "https://polinas-potent-potions.myshopify.com",
  "vendors": [
    "Clover's Apothecary",
    "Polina's Potent Potions",
    "Ted's Apothecary Supply"
  ]
}
```

</page>

<page>
---
title: 'Liquid objects: shop_locale'
description: A language in a store.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/shop_locale'
  md: 'https://shopify.dev/docs/api/liquid/objects/shop_locale.md'
---

# shop\_​locale

A language in a store.

To learn how to offer localization options in your theme, refer to [Support multiple currencies and languages](https://shopify.dev/themes/internationalization/multiple-currencies-languages).

## Properties

* * **endonym\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the locale in the locale itself.

  * **iso\_​code**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The ISO code of the locale in [IETF language tag format](https://en.wikipedia.org/wiki/IETF_language_tag).

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the locale in the store's primary locale.

  * **primary**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the locale is the store's primary locale. Returns `false` if not.

  * **root\_​url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The relative root URL of the locale.

##### Example

```json
{
  "endonym_name": "English",
  "iso_code": "en",
  "name": "English",
  "primary": true,
  "root_url": "/"
}
```

</page>

<page>
---
title: 'Liquid objects: sitemap'
description: >-
  The sitemap for a specific group in the [`robots.txt`
  file](/themes/architecture/templates/robots-txt-liquid).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/sitemap'
  md: 'https://shopify.dev/docs/api/liquid/objects/sitemap.md'
---

# sitemap

The sitemap for a specific group in the [`robots.txt` file](https://shopify.dev/themes/architecture/templates/robots-txt-liquid).

The sitemap provides information about the pages and content on a site, and the relationships between them, which helps crawlers crawl a site more efficiently.

***

**Tip:** To learn more about sitemaps, refer to \<a href="https://developers.google.com/search/docs/advanced/sitemaps/overview">Google\&#39;s documentation\</a>.

***

The `sitemap` object consists of a `Sitemap` directive, and a value of the URL that the sitemap is hosted at. For example:

```
Sitemap: https://your-store.myshopify.com/sitemap.xml
```

***

**Tip:** You can \<a href="/themes/seo/robots-txt">customize the \<code>robots.txt\</code> file\</a> with the \<a href="/themes/architecture/templates/robots-txt-liquid">\<code>robots.txt.liquid\</code> template\</a>.

***

## Properties

* * **directive**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Returns `Sitemap`.

  * **value**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL that the sitemap is hosted at.

##### Example

```json
{
  "directive": "Sitemap",
  "value": "https://polinas-potent-potions.myshopify.com/sitemap.xml"
}
```

</page>

<page>
---
title: 'Liquid objects: sort_option'
description: A sort option for a collection or search results page.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/sort_option'
  md: 'https://shopify.dev/docs/api/liquid/objects/sort_option.md'
---

# sort\_​option

A sort option for a collection or search results page.

## Properties

* * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The customer-facing name of the sort option.

    The name can be edited by merchants in the [language editor](https://help.shopify.com/manual/online-store/themes/customizing-themes/language/change-wording).

  * **value**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The value of the sort option.

    This value is used when assigning the [`collection.sort_by`](https://shopify.dev/docs/api/liquid/objects/collection#collection-sort_by) and [`search.sort_by`](https://shopify.dev/docs/api/liquid/objects/search#search-sort_by) parameters.

##### Example

```json
{
  "name": "Alphabetically, A-Z",
  "value": "title-ascending"
}
```

</page>

<page>
---
title: 'Liquid objects: store_availability'
description: A variant's inventory information for a physical store location.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/store_availability'
  md: 'https://shopify.dev/docs/api/liquid/objects/store_availability.md'
---

# store\_​availability

A variant's inventory information for a physical store location.

If a location doesn't stock a variant, then there won't be a `store_availability` for that variant and location.

***

**Note:** The \<code>\<span class="PreventFireFoxApplyingGapToWBR">store\<wbr/>\_availability\</span>\</code> object is defined only if one or more locations has \<a href="https://help.shopify.com/manual/shipping/setting-up-and-managing-your-shipping/local-methods/local-pickup">local pickup\</a> enabled.

***

## Properties

* * **available**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant has available inventory at the location. Returns `false` if not.

  * **location**

    [location](https://shopify.dev/docs/api/liquid/objects/location)

  * The location that the variant is stocked at.

  * **pick\_​up\_​enabled**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the location has pickup enabled. Returns `false` if not.

  * **pick\_​up\_​time**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The amount of time that it takes for pickup orders to be ready at the location.

    **Tip:** This value can be configured in the Shopify admin. To learn more, visit the \<a href="https://help.shopify.com/en/manual/sell-in-person/shopify-pos/order-management/local-pickup-for-online-orders#manage-preferences-for-a-local-pickup-location">Shopify Help Center\</a>.

##### Example

```json
{
  "available": true,
  "location": {},
  "pick_up_enabled": true,
  "pick_up_time": "Usually ready in 24 hours"
}
```

</page>

<page>
---
title: 'Liquid objects: store_credit_account'
description: >-
  A [store credit
  account](https://help.shopify.com/en/manual/customers/store-credit) owned by a
  [customer](/docs/api/liquid/objects/customer).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/store_credit_account'
  md: 'https://shopify.dev/docs/api/liquid/objects/store_credit_account.md'
---

# store\_​credit\_​account

A [store credit account](https://help.shopify.com/en/manual/customers/store-credit) owned by a [customer](https://shopify.dev/docs/api/liquid/objects/customer).

## Properties

* * **balance**

    [money](https://shopify.dev/docs/api/liquid/objects/money)

  * The balance of the store credit account in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

##### Example

```json
{
  "balance": {}
}
```

</page>

<page>
---
title: 'Liquid objects: swatch'
description: >-
  Color and image for visual representation.

  Available for [product option
  values](/docs/api/liquid/objects/product_option_value) and [filter
  values](/docs/api/liquid/objects/filter_value).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/swatch'
  md: 'https://shopify.dev/docs/api/liquid/objects/swatch.md'
---

# swatch

Color and image for visual representation. Available for [product option values](https://shopify.dev/docs/api/liquid/objects/product_option_value) and [filter values](https://shopify.dev/docs/api/liquid/objects/filter_value).

## Properties

* * **color**

    [color](https://shopify.dev/docs/api/liquid/objects/color)

  * The swatch color.

  * **image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The swatch image.

##### Example

```json
{
  "color": {},
  "image": {}
}
```

</page>

<page>
---
title: 'Liquid objects: tablerowloop'
description: 'Information about a parent [`tablerow` loop](/docs/api/liquid/tags/tablerow).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/tablerowloop'
  md: 'https://shopify.dev/docs/api/liquid/objects/tablerowloop.md'
---

# tablerowloop

Information about a parent [`tablerow` loop](https://shopify.dev/docs/api/liquid/tags/tablerow).

## Properties

* * **col**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current column.

  * **col\_​first**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current column is the first in the row. Returns `false` if not.

  * **col\_​last**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current column is the last in the row. Returns `false` if not.

  * **col0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current column.

  * **first**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current iteration is the first. Returns `false` if not.

  * **index**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current iteration.

  * **index0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current iteration.

  * **last**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current iteration is the last. Returns `false` if not.

  * **length**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of iterations in the loop.

  * **rindex**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current iteration, in reverse order.

  * **rindex0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current iteration, in reverse order.

  * **row**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of current row.

##### Example

```json
{
  "col": 1,
  "col0": 0,
  "col_first": true,
  "col_last": false,
  "first": true,
  "index": 1,
  "index0": 0,
  "last": false,
  "length": 5,
  "rindex": 5,
  "rindex0": 4,
  "row": 1
}
```

</page>

<page>
---
title: 'Liquid objects: tax_line'
description: Information about a tax line of a checkout or order.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/tax_line'
  md: 'https://shopify.dev/docs/api/liquid/objects/tax_line.md'
---

# tax\_​line

Information about a tax line of a checkout or order.

## Properties

* * **price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The tax amount in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **rate**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The decimal value of the tax rate.

  * **rate\_​percentage**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The decimal value of the tax rate, as a percentage.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The title of the tax.

##### Example

```json
{
  "price": 1901,
  "rate": 0.05,
  "rate_percentage": 5,
  "title": "GST"
}
```

</page>

<page>
---
title: 'Liquid objects: taxonomy_category'
description: The taxonomy category for a product
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/taxonomy_category'
  md: 'https://shopify.dev/docs/api/liquid/objects/taxonomy_category.md'
---

# taxonomy\_​category

The taxonomy category for a product

## Properties

* * **ancestors**

    array of [taxonomy\_category](https://shopify.dev/docs/api/liquid/objects/taxonomy_category)

  * All parent nodes of the current taxonomy category.

  * **gid**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The public node ID for the category, formatted as a Shopify GID.

  * **id**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The public node ID for the category

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The localized category name

##### Example

```json
{
  "ancestors": [],
  "gid": "gid://shopify/TaxonomyCategory/hb-1-9-6",
  "id": "hb-1-9-6",
  "name": "Vitamins & Supplements"
}
```

</page>

<page>
---
title: 'Liquid objects: template'
description: 'Information about the current [template](/docs/themes/architecture/templates).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/template'
  md: 'https://shopify.dev/docs/api/liquid/objects/template.md'
---

# template

Information about the current [template](https://shopify.dev/docs/themes/architecture/templates).

## Properties

* * **directory**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the template's parent directory.

    Returns `nil` if the template's parent directory is `/templates`.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The name of the template's [type](https://shopify.dev/docs/themes/architecture/templates#template-types).

    | Possible values |
    | - |
    | 404 |
    | article |
    | blog |
    | cart |
    | collection |
    | list-collections |
    | customers/account |
    | customers/activate\_account |
    | customers/addresses |
    | customers/login |
    | customers/order |
    | customers/register |
    | customers/reset\_password |
    | gift\_card |
    | index |
    | page |
    | password |
    | product |
    | search |

  * **suffix**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The custom name of an [alternate template](https://shopify.dev/themes/architecture/templates#alternate-templates).

    Returns `nil` if the default template is being used.

##### Example

```json
{
  "directory": null,
  "name": "product",
  "suffix": null
}
```

</page>

<page>
---
title: 'Liquid objects: theme'
description: Information about the current theme.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/theme'
  md: 'https://shopify.dev/docs/api/liquid/objects/theme.md'
---

# theme

Information about the current theme.

**deprecated:**

Deprecated because the values of this object's properties are subject to change, so can't be relied on within the theme.

If you want to link to the theme editor for the published theme, then you can use the URL path `/admin/themes/current/editor`.

While this object is deprecated in Liquid and shouldn't be used, you can still access it through the [REST Admin API](https://shopify.dev/api/admin-rest/current/resources/theme).

## Properties

* * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the theme.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the theme.

  * **role**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The role of the theme.

    | Possible values | Description |
    | - | - |
    | main | The theme is published. Customers see it when they visit the online store. |
    | unpublished | The theme is unpublished. Customers can't see it. |
    | demo | The theme is installed on the store as a demo. The theme can't be published until the merchant buys the full version. |
    | development | The theme is used for development. The theme can't be published, and is temporary. |

##### Example

```json
{
  "id": 124051750977,
  "name": "Dawn",
  "role": "main"
}
```

</page>

<page>
---
title: 'Liquid objects: transaction'
description: A transaction associated with a checkout or order.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/transaction'
  md: 'https://shopify.dev/docs/api/liquid/objects/transaction.md'
---

# transaction

A transaction associated with a checkout or order.

## Properties

* * **amount**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The amount of the transaction in the currency's subunit.

    The amount is in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted amount.

  * **buyer\_​pending\_​payment\_​instructions**

    array of [pending\_payment\_instruction\_input](https://shopify.dev/docs/api/liquid/objects/pending_payment_instruction_input)

  * A list of `pending_payment_instruction_input` header-value pairs, with payment method-specific details. The customer can use these details to complete their purchase offline.

    If the payment method doesn’t support pending payment instructions, then an empty array is returned.

    | Supported payment method | Expected Values |
    | - | - |
    | ShopifyPayments - Multibanco | \[{header="Entity", value="12345"}, {header="Reference", value="999999999"}] |

  * **buyer\_​pending\_​payment\_​notice**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A notice that contains instructions for the customer on how to complete their payment. The messages are specific to the payment method used.

  * **created\_​at**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A timestamp of when the transaction was created.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the timestamp.

  * **gateway**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [handleized](https://shopify.dev/docs/api/liquid/basics#modifying-handles) name of the payment provider used for the transaction.

  * **gateway\_​display\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the payment provider used for the transaction.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the transaction.

  * **kind**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of transaction.

    | Possible values | Description |
    | - | - |
    | authorization | The reserving of money that the customer has agreed to pay. |
    | capture | The transfer of the money that was reserved during the `authorization` step. |
    | sale | A combination of `authorization` and `capture` in one step. |
    | void | The cancellation of a pending `authorization` or `capture`. |
    | refund | The partial, or full, refund of captured funds. |

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the transaction.

  * **payment\_​details**

    [transaction\_payment\_details](https://shopify.dev/docs/api/liquid/objects/transaction_payment_details)

  * The transaction payment details.

  * **receipt**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Information from the payment provider about the payment receipt.

    This includes things like whether the payment was a test, or an authorization code if there was one.

  * **show\_​buyer\_​pending\_​payment\_​instructions?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Whether the transaction is pending, and whether additional customer info is required to process the payment.

  * **status**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The status of the transaction.

    | Possible values |
    | - |
    | success |
    | pending |
    | failure |
    | error |

  * **status\_​label**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The status of the transaction, translated based on the current locale.

##### Example

```json
{
  "amount": "380.25",
  "created_at": "2022-06-15 19:13:14 -0400",
  "gateway": "shopify_payments",
  "gateway_display_name": "Shopify payments",
  "id": 5432242176065,
  "kind": "sale",
  "name": "c29944051400769.",
  "payment_details": {
    "credit_card_number": "•••• •••• •••• 4242",
    "credit_card_company": "Visa",
    "credit_card_last_four_digits": "4242",
    "receiver_info": null
  },
  "receipt": "#☠1☢\n---\nid: pi_3LB5Oh2m9fH5ulsO18aKrXyL\nobject: payment_intent\namount: 38025\namount_capturable: 0\namount_received: 38025\ncanceled_at: \ncancellation_reason: \ncapture_method: automatic\ncharges:\n  object: list\n  data:\n  - id: ch_3LB5Oh2m9fH5ulsO1KncBePo\n    object: charge\n    amount: 38025\n    application_fee: fee_1LB5Oi2m9fH5ulsOrVcBjr4k\n    balance_transaction:\n      id: txn_3LB5Oh2m9fH5ulsO1JtjGSxy\n      object: balance_transaction\n      exchange_rate: \n    captured: true\n    created: 1655334796\n    currency: cad\n    failure_code: \n    failure_message: \n    fraud_details: {}\n    livemode: false\n    metadata:\n      shop_id: '56174706753'\n      shop_name: Polina's Potent Potions\n      transaction_fee_total_amount: '791'\n      transaction_fee_tax_amount: '0'\n      payments_charge_id: '2076986474561'\n      order_transaction_id: '5432242176065'\n      manual_entry: 'false'\n      order_id: c29944051400769.1\n      email: cornelius.potionmaker@gmail.com\n    outcome:\n      network_status: approved_by_network\n      reason: \n      risk_level: normal\n      risk_score: 15\n      seller_message: Payment complete.\n      type: authorized\n    paid: true\n    payment_intent: pi_3LB5Oh2m9fH5ulsO18aKrXyL\n    payment_method: pm_1LB5Oh2m9fH5ulsOk67EqrsK\n    payment_method_details:\n      card:\n        brand: visa\n        checks:\n          address_line1_check: pass\n          address_postal_code_check: pass\n          cvc_check: pass\n        country: US\n        description: Visa Classic\n        ds_transaction_id: \n        exp_month: 1\n        exp_year: 2029\n        fingerprint: KE6OIQsc8EspGDeW\n        funding: credit\n        iin: '424242'\n        installments: \n        issuer: Stripe Payments UK Limited\n        last4: '4242'\n        mandate: \n        moto: \n        network: visa\n        network_token: \n        network_transaction_id: '541168454791087'\n        three_d_secure: \n        wallet: \n      type: card\n    refunded: false\n    source: \n    status: succeeded\n    mit_params:\n      network_transaction_id: '541168454791087'\n  has_more: false\n  total_count: 1\n  url: \"/v1/charges?payment_intent=pi_3LB5Oh2m9fH5ulsO18aKrXyL\"\nconfirmation_method: manual\ncreated: 1655334795\ncurrency: cad\nlast_payment_error: \nlivemode: false\nmetadata:\n  shop_id: '56174706753'\n  shop_name: Polina's Potent Potions\n  transaction_fee_total_amount: '791'\n  transaction_fee_tax_amount: '0'\n  payments_charge_id: '2076986474561'\n  order_transaction_id: '5432242176065'\n  manual_entry: 'false'\n  order_id: c29944051400769.1\n  email: cornelius.potionmaker@gmail.com\nnext_action: \npayment_method: pm_1LB5Oh2m9fH5ulsOk67EqrsK\npayment_method_types:\n- card\nsource: \nstatus: succeeded\n",
  "show_buyer_pending_payment_instructions?": null,
  "status": "success",
  "status_label": "Success"
}
```

</page>

<page>
---
title: 'Liquid objects: transaction_payment_details'
description: Information about the payment methods used for a transaction.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/transaction_payment_details'
  md: 'https://shopify.dev/docs/api/liquid/objects/transaction_payment_details.md'
---

# transaction\_​payment\_​details

Information about the payment methods used for a transaction.

## Properties

* * **credit\_​card\_​company**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the company that issued the credit card used for the transaction.

  * **credit\_​card\_​last\_​four\_​digits**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The last four digits of the credit card number of the credit card used for the transaction.

  * **credit\_​card\_​number**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The credit card number of the credit card used for the transaction.

    All but the last four digits are redacted.

  * **gift\_​card**

    [gift\_card](https://shopify.dev/docs/api/liquid/objects/gift_card)

  * The gift card used for the transaction.

    If no gift card was used, then `nil` is returned.

##### Example

```json
{
  "credit_card_number": "•••• •••• •••• 4242",
  "credit_card_company": "Visa",
  "credit_card_last_four_digits": "4242"
}
```

</page>

<page>
---
title: 'Liquid objects: unit_price_measurement'
description: >-
  Information about how units of a product variant are measured. It's used to
  calculate

  [unit
  prices](https://help.shopify.com/manual/products/details/product-pricing/unit-pricing#add-unit-prices-to-your-product).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/unit_price_measurement'
  md: 'https://shopify.dev/docs/api/liquid/objects/unit_price_measurement.md'
---

# unit\_​price\_​measurement

Information about how units of a product variant are measured. It's used to calculate [unit prices](https://help.shopify.com/manual/products/details/product-pricing/unit-pricing#add-unit-prices-to-your-product).

## Properties

* * **measured\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The type of unit measurement.

    | Possible values |
    | - |
    | volume |
    | weight |
    | length |
    | area |
    | count |

  * **quantity\_​unit**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The unit of measurement used to measure the [`quantity_value`](https://shopify.dev/docs/api/liquid/objects/unit_price_measurement#unit_price_measurement-quantity_value).

  * **quantity\_​value**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The quantity of the unit.

  * **reference\_​unit**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The unit of measurement used to measure the [`reference_value`](https://shopify.dev/docs/api/liquid/objects/unit_price_measurement#unit_price_measurement-reference_value).

  * **reference\_​value**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The quantity of the unit for the base unit price.

##### Example

```json
{
  "measured_type": "weight",
  "quantity_value": "500.0",
  "quantity_unit": "g",
  "reference_value": 1,
  "reference_unit": "kg"
}
```

</page>

<page>
---
title: 'Liquid objects: user'
description: The author of a blog article.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/user'
  md: 'https://shopify.dev/docs/api/liquid/objects/user.md'
---

# user

The author of a blog article.

***

**Tip:** The information returned by the \<code>user\</code> object can be edited on the \<a href="https://www\.shopify.com/admin/settings/account">\<strong>Account\</strong> page\</a> of the Shopify admin.

***

## Properties

* * **account\_​owner**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the author is the account owner of the store. Returns `false` if not.

  * **bio**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The bio associated with the author's account.

    If no bio is specified, then `nil` is returned.

  * **email**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The email associated with the author's account.

  * **first\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The first name associated with the author's account.

  * **homepage**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL for the personal website associated with the author's account.

    If no personal website is specified, then `nil` is returned.

  * **image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The image associated with the author's account.

    If no image is specified, then `nil` is returned.

  * **last\_​name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The last name associated with the author's account.

  * **name**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The first and last name associated with the author's account.

##### Example

```json
{
  "account_owner": false,
  "bio": "Polina got her first cauldron at the tender age of six, and she has been passionate about potions ever since!!",
  "email": "polinas.potent.potions@gmail.com",
  "first_name": "Polina",
  "homepage": null,
  "image": {},
  "last_name": "Waters",
  "name": "Polina Waters"
}
```

</page>

<page>
---
title: 'Liquid objects: user_agent'
description: >-
  The user-agent, which is the name of the crawler, for a specific group in the
  [`robots.txt` file](/themes/architecture/templates/robots-txt-liquid).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/user_agent'
  md: 'https://shopify.dev/docs/api/liquid/objects/user_agent.md'
---

# user\_​agent

The user-agent, which is the name of the crawler, for a specific group in the [`robots.txt` file](https://shopify.dev/themes/architecture/templates/robots-txt-liquid).

The `user_agent` object consists of a `User-agent` directive, and a value of the name of the user-agent. For example:

```
User-agent: *
```

***

**Tip:** You can \<a href="/themes/seo/robots-txt">customize the \<code>robots.txt\</code> file\</a> with the \<a href="/themes/architecture/templates/robots-txt-liquid">\<code>robots.txt.liquid\</code> template\</a>.

***

## Properties

* * **directive**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * Returns `User-agent`.

  * **value**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The name of the user-agent.

##### Example

```json
{
  "directive": "User-agent",
  "value": "*"
}
```

</page>

<page>
---
title: 'Liquid objects: variant'
description: 'A [product variant](https://help.shopify.com/manual/products/variants).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/variant'
  md: 'https://shopify.dev/docs/api/liquid/objects/variant.md'
---

# variant

A [product variant](https://help.shopify.com/manual/products/variants).

## Properties

* * **available**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant is available. Returns `false` if not.

  * **barcode**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The barcode of the variant.

  * **compare\_​at\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The **compare at** price of the variant in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **featured\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The image attached to the variant.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/variant#variant-image">\<code>variant.image\</code>\</a>.

  * **featured\_​media**

    [media](https://shopify.dev/docs/api/liquid/objects/media)

  * The first media object attached to the variant.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the variant.

  * **image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * The image attached to the variant.

    **Note:** This is the same value as \<a href="/docs/api/liquid/objects/variant#variant-featured\_image">\<code>\<span class="PreventFireFoxApplyingGapToWBR">variant.featured\<wbr/>\_image\</span>\</code>\</a>.

  * **incoming**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant has incoming inventory. Returns `false` if not.

    Incoming inventory information is populated by [inventory transfers](https://help.shopify.com/manual/products/inventory/transfers), [purchase orders](https://help.shopify.com/manual/products/inventory/purchase-orders), and [third-party apps](https://shopify.dev/docs/apps/fulfillment/inventory-management-apps).

  * **inventory\_​management**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The inventory management service of the variant.

    If inventory isn't tracked, then `nil` is returned.

  * **inventory\_​policy**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * Whether the variant should continue to be sold when it's out of stock.

    **Tip:** To learn about why merchants might want to continue selling products when they\&#39;re out of stock, visit the \<a href="https://help.shopify.com/manual/products/inventory/getting-started-with-inventory/selling-when-out-of-stock">Shopify Help Center\</a>.

    | Possible values | Description |
    | - | - |
    | continue | Continue selling when the variant is out of stock. |
    | deny | Stop selling when the variant is out of stock. |

  * **inventory\_​quantity**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The inventory quantity of the variant.

    If inventory isn't tracked, then the number of items sold is returned.

  * **matched**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant has been matched by a [storefront filter](https://shopify.dev/themes/navigation-search/filtering/storefront-filtering) or no filters are applied. Returns `false` if it hasn't.

  * **metafields**

  * The [metafields](https://shopify.dev/docs/api/liquid/objects/metafield) applied to the variant.

    **Tip:** To learn about how to create metafields, refer to \<a href="/apps/metafields/manage">Create and manage metafields\</a> or visit the \<a href="https://help.shopify.com/manual/metafields">Shopify Help Center\</a>.

  * **next\_​incoming\_​date**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The arrival date for the next incoming inventory of the variant.

    Incoming inventory information is populated by [inventory transfers](https://help.shopify.com/manual/products/inventory/transfers), [purchase orders](https://help.shopify.com/manual/products/inventory/purchase-orders), and [third-party apps](https://shopify.dev/docs/apps/fulfillment/inventory-management-apps).

    **Tip:** Use the \<a href="/docs/api/liquid/filters/date">\<code>date\</code> filter\</a> to format the date.

  * **options**

    [product\_option\_value](https://shopify.dev/docs/api/liquid/objects/product_option_value)

  * The values of the variant for each [product option](https://shopify.dev/docs/api/liquid/objects/product_option).

    ExampleOutput the options of each variant

    ##### Code

    ```liquid
    {% for variant in product.variants -%}
      {%- capture options -%}
        {% for option in variant.options -%}
          {{ option }}{%- unless forloop.last -%}/{%- endunless -%}
        {%- endfor %}
      {%- endcapture -%}
      
      {{ variant.id }}: {{ options }}
    {%- endfor %}
    ```

    ##### Data

    ```json
    {
      "product": {
        "variants": [
          {
            "id": 39897499729985,
            "options": [
              "S",
              "Low"
            ]
          },
          {
            "id": 39897499762753,
            "options": [
              "S",
              "Medium"
            ]
          },
          {
            "id": 39897499795521,
            "options": [
              "S",
              "High"
            ]
          },
          {
            "id": 39897499828289,
            "options": [
              "M",
              "Low"
            ]
          },
          {
            "id": 39897499861057,
            "options": [
              "M",
              "Medium"
            ]
          },
          {
            "id": 39897499893825,
            "options": [
              "M",
              "High"
            ]
          },
          {
            "id": 39897499926593,
            "options": [
              "L",
              "Low"
            ]
          },
          {
            "id": 39897499959361,
            "options": [
              "L",
              "Medium"
            ]
          },
          {
            "id": 39897499992129,
            "options": [
              "L",
              "High"
            ]
          }
        ]
      }
    }
    ```

    ##### Output

    ```html
    39897499729985: S/Low

    39897499762753: S/Medium

    39897499795521: S/High

    39897499828289: M/Low

    39897499861057: M/Medium

    39897499893825: M/High

    39897499926593: L/Low

    39897499959361: L/Medium

    39897499992129: L/High
    ```

  * **price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The price of the variant in the currency's subunit.

    The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use \<a href="/docs/api/liquid/filters/money-filters">money filters\</a> to output a formatted price.

  * **product**

    [product](https://shopify.dev/docs/api/liquid/objects/product)

  * The parent product of the variant.

  * **quantity\_​price\_​breaks**

    array of [quantity\_price\_break](https://shopify.dev/docs/api/liquid/objects/quantity_price_break)

  * Returns `quantity_price_break` objects for the variant in the current customer context.

  * **quantity\_​price\_​breaks\_​configured?**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant has any quantity price breaks available in the current customer context. Returns `false` if it doesn't.

  * **quantity\_​rule**

    [quantity\_rule](https://shopify.dev/docs/api/liquid/objects/quantity_rule)

  * The quantity rule for the variant.

    If no rule exists, then a default value is returned.

    This rule can be set as part of a [B2B catalog](https://help.shopify.com/manual/b2b/catalogs/quantity-pricing).

    **Note:** The default quantity rule is \<code>min=1,max=null,increment=1\</code>.

  * **requires\_​selling\_​plan**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant's product is set to require a `selling_plan` when being added to the cart. Returns `false` if not.

  * **requires\_​shipping**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant requires shipping. Returns `false` if it doesn't.

  * **selected**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the variant is currently selected. Returns `false` if it's not.

    **Note:** The selected variant is determined by the \<code>variant\</code> URL parameter. This URL parameter is available on product pages URLs only.

  * **selected\_​selling\_​plan\_​allocation**

    [selling\_plan\_allocation](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation)

  * The selected `selling_plan_allocation`.

    If no selling plan is selected, then `nil` is returned.

    **Note:** The selected selling plan is determined by the \<code>\<span class="PreventFireFoxApplyingGapToWBR">selling\<wbr/>\_plan\</span>\</code> URL parameter.

  * **selling\_​plan\_​allocations**

    array of [selling\_plan\_allocation](https://shopify.dev/docs/api/liquid/objects/selling_plan_allocation)

  * The `selling_plan_allocation` objects for the variant.

  * **sku**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The SKU of the variant.

  * **store\_​availabilities**

    array of [store\_availability](https://shopify.dev/docs/api/liquid/objects/store_availability)

  * The store availabilities for the variant.

    The array is defined in only the following cases:

    * `variant.selected` is `true`
    * The variant is the product's first available variant. For example, `product.first_available_variant` or `product.selected_or_first_available_variant`.

  * **taxable**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if taxes should be charged on the variant. Returns `false` if not.

  * **title**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * A concatenation of each variant option, separated by a `/`.

    ExampleThe variant title

    ##### Code

    ```liquid
    {{ product.variants.first.title }}
    ```

    ##### Data

    ```json
    {
      "product": {
        "variants": [
          {
            "title": "S / Low"
          },
          {
            "title": "S / Medium"
          },
          {
            "title": "S / High"
          },
          {
            "title": "M / Low"
          },
          {
            "title": "M / Medium"
          },
          {
            "title": "M / High"
          },
          {
            "title": "L / Low"
          },
          {
            "title": "L / Medium"
          },
          {
            "title": "L / High"
          }
        ]
      }
    }
    ```

    ##### Output

    ```html
    S / Low
    ```

  * **unit\_​price**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The [unit price](https://help.shopify.com/manual/products/details/product-pricing/unit-pricing#add-unit-prices-to-your-product) of the variant in the currency's subunit.

    The price reflects any discounts that are applied to the line item. The value is output in the customer's local (presentment) currency.

    For currencies without subunits, such as JPY and KRW, tenths and hundredths of a unit are appended. For example, 1000 Japanese yen is output as 100000.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/unit\_price\_with\_measurement">\<code>\<span class="PreventFireFoxApplyingGapToWBR">unit\<wbr/>\_price\<wbr/>\_with\<wbr/>\_measurement\</span>\</code> filter\</a> with this property and the \<code>\<span class="PreventFireFoxApplyingGapToWBR">variant.unit\<wbr/>\_price\<wbr/>\_measurement\</span>\</code> property to output a formatted unit price with measurement.

    To learn about how to display unit prices in your theme, refer to [Unit pricing](https://shopify.dev/themes/pricing-payments/unit-pricing).

  * **unit\_​price\_​measurement**

    [unit\_price\_measurement](https://shopify.dev/docs/api/liquid/objects/unit_price_measurement)

  * The unit price measurement of the variant.

    To learn about how to display unit prices in your theme, refer to [Unit pricing](https://shopify.dev/themes/pricing-payments/unit-pricing).

    **Tip:** Use the \<a href="/docs/api/liquid/filters/unit\_price\_with\_measurement">\<code>\<span class="PreventFireFoxApplyingGapToWBR">unit\<wbr/>\_price\<wbr/>\_with\<wbr/>\_measurement\</span>\</code> filter\</a> with the \<code>\<span class="PreventFireFoxApplyingGapToWBR">variant.unit\<wbr/>\_price\</span>\</code> property and this property to output a formatted unit price with measurement.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The URL of the variant.

    Variant URLs use the following structure:

    ```
    /products/[product-handle]?variant=[variant-id]
    ```

  * **weight**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The weight of the variant in grams.

    **Tip:** Use the \<a href="/docs/api/liquid/filters/weight\_with\_unit">\<code>\<span class="PreventFireFoxApplyingGapToWBR">weight\<wbr/>\_with\<wbr/>\_unit\</span>\</code>\</a> filter to format the weight in \<a href="https://www\.shopify.com/admin/settings/general">the store\&#39;s format\</a>.\</p> \<p>Use \<code>\<span class="PreventFireFoxApplyingGapToWBR">variant.weight\<wbr/>\_in\<wbr/>\_unit\</span>\</code> to output the weight in the unit configured on the variant.

  * **weight\_​in\_​unit**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The weight of the variant in the unit specified by `variant.weight_unit`.

    **Tip:** To output this weight, use this property, and the \<code>\<span class="PreventFireFoxApplyingGapToWBR">variant.weight\<wbr/>\_unit\</span>\</code> property, with the \<a href="/docs/api/liquid/filters/weight\_with\_unit">\<code>\<span class="PreventFireFoxApplyingGapToWBR">weight\<wbr/>\_with\<wbr/>\_unit\</span>\</code> filter\</a>.

  * **weight\_​unit**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The unit for the weight of the variant.

    **Tip:** To output the weight of a variant in this unit, use this property, and the \<code>\<span class="PreventFireFoxApplyingGapToWBR">variant.weight\<wbr/>\_in\<wbr/>\_unit\</span>\</code> property, with the \<a href="/docs/api/liquid/filters/weight\_with\_unit">\<code>\<span class="PreventFireFoxApplyingGapToWBR">weight\<wbr/>\_with\<wbr/>\_unit\</span>\</code> filter\</a>.

## Deprecated Properties

* * **option1**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

    Deprecated

  * The value of the variant for the first product option.

    If there's no first product option, then `nil` is returned.

    **Deprecated:**

    Deprecated. Prefer to use [`variant.options`](https://shopify.dev/docs/api/liquid/objects/variant#variant-options) instead.

  * **option2**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

    Deprecated

  * The value of the variant for the second product option.

    If there's no second product option, then `nil` is returned.

    **Deprecated:**

    Deprecated. Prefer to use [`variant.options`](https://shopify.dev/docs/api/liquid/objects/variant#variant-options) instead.

  * **option3**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

    Deprecated

  * The value of the variant for the third product option.

    If there's no third product option, then `nil` is returned.

    **Deprecated:**

    Deprecated. Prefer to use [`variant.options`](https://shopify.dev/docs/api/liquid/objects/variant#variant-options) instead.

##### Example

```json
{
  "available": true,
  "barcode": "",
  "compare_at_price": null,
  "featured_image": null,
  "featured_media": null,
  "id": 39897499729985,
  "image": null,
  "incoming": false,
  "inventory_management": "shopify",
  "inventory_policy": "deny",
  "inventory_quantity": 5,
  "matched": true,
  "metafields": {},
  "next_incoming_date": null,
  "option1": "S",
  "option2": "Low",
  "option3": null,
  "options": [],
  "price": "10.00",
  "product": {},
  "quantity_price_breaks": [],
  "quantity_rule": {},
  "requires_selling_plan": false,
  "requires_shipping": true,
  "selected": false,
  "selected_selling_plan_allocation": null,
  "selling_plan_allocations": [],
  "sku": "",
  "store_availabilities": [],
  "taxable": true,
  "title": "S / Low",
  "unit_price": null,
  "unit_price_measurement": null,
  "url": {},
  "weight": 500,
  "weight_in_unit": 500,
  "weight_unit": "g"
}
```

</page>

<page>
---
title: 'Liquid objects: video'
description: >-
  Information about a video uploaded as [product
  media](/docs/api/liquid/objects/product-media) or a [`file_reference`
  metafield](/apps/metafields/types).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/video'
  md: 'https://shopify.dev/docs/api/liquid/objects/video.md'
---

# video

Information about a video uploaded as [product media](https://shopify.dev/docs/api/liquid/objects/product-media) or a [`file_reference` metafield](https://shopify.dev/apps/metafields/types).

***

**Tip:** Use the \<a href="/docs/api/liquid/filters/video\_tag">\<code>\<span class="PreventFireFoxApplyingGapToWBR">video\<wbr/>\_tag\</span>\</code> filter\</a> to output the video in an HTML \<code>\&lt;video\&gt;\</code> tag.

***

## Properties

* * **alt**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The alt text of the video.

  * **aspect\_​ratio**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The aspect ratio of the video as a decimal.

  * **duration**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The duration of the video in milliseconds.

  * **id**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The ID of the video.

  * **media\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The media type of the model. Always returns `video`.

    ExampleFilter for media of a specific type

    You can use the `media_type` property with the [`where` filter](https://shopify.dev/docs/api/liquid/filters/where) to filter the [`product.media` array](https://shopify.dev/docs/api/liquid/objects/product#product-media) for all media of a desired type.

    ##### Code

    ```liquid
    {% assign videos = product.media | where: 'media_type', 'video' %}

    {% for video in videos %}
      {{- video | video_tag }}
    {% endfor %}
    ```

    ##### Data

    ```json
    {
      "product": {
        "media": [
          {
            "media_type": "external_video"
          },
          {
            "media_type": "video"
          }
        ]
      }
    }
    ```

    ##### Output

    ```html
    <video playsinline="playsinline" preload="metadata" aria-label="Potion beats" poster="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_small.jpg?v=1655255324"><source src="//polinas-potent-potions.myshopify.com/cdn/shop/videos/c/vp/4edc28a708b7405093a927cebe794f1a/4edc28a708b7405093a927cebe794f1a.HD-1080p-7.2Mbps.mp4?v=0" type="video/mp4"><img src="//polinas-potent-potions.myshopify.com/cdn/shop/products/4edc28a708b7405093a927cebe794f1a.thumbnail.0000000_small.jpg?v=1655255324"></video>
    ```

  * **position**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The position of the video in the [`product.media`](https://shopify.dev/docs/api/liquid/objects/product#product-media) array.

  * **preview\_​image**

    [image](https://shopify.dev/docs/api/liquid/objects/image)

  * A preview image for the video.

  * **sources**

    array of [video\_source](https://shopify.dev/docs/api/liquid/objects/video_source)

  * The source files for the video.

##### Example

```json
{
  "alt": "Potion beats",
  "aspect_ratio": 1.779,
  "duration": 34801,
  "id": 22070396551233,
  "media_type": "video",
  "position": 2,
  "preview_image": {},
  "sources": []
}
```

</page>

<page>
---
title: 'Liquid objects: video_source'
description: Information about the source files for a video.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/objects/video_source'
  md: 'https://shopify.dev/docs/api/liquid/objects/video_source.md'
---

# video\_​source

Information about the source files for a video.

## Properties

* * **format**

    [string](https://shopify.dev/docs/api/liquid/basics#string) from a set of values

  * The format of the video source file.

    **Note:** When mp4 videos are uploaded, Shopify generates an m3u8 file as an additional video source. An m3u8 file enables video players to leverage HTTP live streaming (HLS), resulting in an optimized video experience based on the user\&#39;s internet connection.

    | Possible values |
    | - |
    | mov |
    | mp4 |
    | m3u8 |

  * **height**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The height of the video source file.

  * **mime\_​type**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) of the video source file.

  * **url**

    [string](https://shopify.dev/docs/api/liquid/basics#string)

  * The [CDN URL](https://shopify.dev/themes/best-practices/performance/platform#shopify-cdn) of the video source file.

  * **width**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The width of the video source file.

##### Example

```json
{
  "format": "mp4",
  "height": 1080,
  "mime_type": "video/mp4",
  "url": "//polinas-potent-potions.myshopify.com/cdn/shop/videos/c/vp/4edc28a708b7405093a927cebe794f1a/4edc28a708b7405093a927cebe794f1a.HD-1080p-7.2Mbps.mp4?v=0",
  "width": 1920
}
```

</page>

<page>
---
title: 'Liquid tags: assign'
description: Creates a new variable.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/assign'
  md: 'https://shopify.dev/docs/api/liquid/tags/assign.md'
---

# assign

Creates a new variable.

You can create variables of any [basic type](https://shopify.dev/docs/api/liquid/basics#types), [object](https://shopify.dev/docs/api/liquid/objects), or object property.

***

**Caution:** Predefined Liquid objects can be overridden by variables with the same name. To make sure that you can access all Liquid objects, make sure that your variable name doesn\&#39;t match a predefined object\&#39;s name.

***

## Syntax

```oobleckTag
{% assign variable_name = value %}
```

### variable\_name

The name of the variable being created.

### value

The value you want to assign to the variable.

##### Code

```liquid
{%- assign product_title = product.title | upcase -%}

{{ product_title }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
HEALTH POTION
```

</page>

<page>
---
title: 'Liquid tags: break'
description: 'Stops a [`for` loop](/docs/api/liquid/tags/for) from iterating.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/break'
  md: 'https://shopify.dev/docs/api/liquid/tags/break.md'
---

# break

Stops a [`for` loop](https://shopify.dev/docs/api/liquid/tags/for) from iterating.

## Syntax

```oobleckTag
{% break %}
```

##### Code

```liquid
{% for i in (1..5) -%}
  {%- if i == 4 -%}
    {% break %}
  {%- else -%}
    {{ i }}
  {%- endif -%}
{%- endfor %}
```

##### Output

```html
1
2
3
```

</page>

<page>
---
title: 'Liquid tags: capture'
description: Creates a new variable with a string value.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/capture'
  md: 'https://shopify.dev/docs/api/liquid/tags/capture.md'
---

# capture

Creates a new variable with a string value.

You can create complex strings with Liquid logic and variables.

***

**Caution:** Predefined Liquid objects can be overridden by variables with the same name. To make sure that you can access all Liquid objects, make sure that your variable name doesn\&#39;t match a predefined object\&#39;s name.

***

## Syntax

```oobleckTag
{% capture variable %}
  value
{% endcapture %}
```

### variable

The name of the variable being created.

### value

The value you want to assign to the variable.

##### Code

```liquid
{%- assign up_title = product.title | upcase -%}
{%- assign down_title = product.title | downcase -%}
{%- assign show_up_title = true -%}

{%- capture title -%}
  {% if show_up_title -%}
    Upcase title: {{ up_title }}
  {%- else -%}
    Downcase title: {{ down_title }}
  {%- endif %}
{%- endcapture %}

{{ title }}
```

##### Data

```json
{
  "product": {
    "title": "Health potion"
  }
}
```

##### Output

```html
Upcase title: HEALTH POTION
```

</page>

<page>
---
title: 'Liquid tags: case'
description: Renders a specific expression depending on the value of a specific variable.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/case'
  md: 'https://shopify.dev/docs/api/liquid/tags/case.md'
---

# case

Renders a specific expression depending on the value of a specific variable.

## Syntax

```oobleckTag
{% case variable %}
  {% when first_value %}
    first_expression
  {% when second_value %}
    second_expression
  {% else %}
    third_expression
{% endcase %}
```

### variable

The name of the variable you want to base your case statement on.

### first\_value

A specific value to check for.

### second\_value

A specific value to check for.

### first\_expression

An expression to be rendered when the variable's value matches `first_value`.

### second\_expression

An expression to be rendered when the variable's value matches `second_value`.

### third\_expression

An expression to be rendered when the variable's value has no match.

##### Code

```liquid
{% case product.type %}
  {% when 'Health' %}
    This is a health potion.
  {% when 'Love' %}
    This is a love potion.
  {% else %}
    This is a potion.
{% endcase %}
```

##### Data

```json
{
  "product": {
    "type": null
  }
}
```

##### Output

```html
This is a health potion.
```

### Multiple values

## Syntax

```oobleckTag
{% case variable %}
  {% when first_value or second_value or third_value %}
    first_expression
  {% when fourth_value, fifth_value, sixth_value %}
    second_expression
  {% else %}
    third_expression
{% endcase %}
```

A `when` tag can accept multiple values. When multiple values are provided, the expression is returned when the variable matches any of the values inside of the tag. Provide the values as a comma-separated list, or separate them using an `or` operator.

##### Code

```liquid
{% case product.type %}
  {% when 'Love' or 'Luck' %}
    This is a love or luck potion.
  {% when 'Strength','Health' %}
    This is a strength or health potion.
  {% else %}
    This is a potion.
{% endcase %}
```

##### Data

```json
{
  "product": {
    "type": null
  }
}
```

##### Output

```html
This is a strength or health potion.
```

</page>

<page>
---
title: 'Liquid tags: comment'
description: Prevents an expression from being rendered or output.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/comment'
  md: 'https://shopify.dev/docs/api/liquid/tags/comment.md'
---

# comment

Prevents an expression from being rendered or output.

Any text inside `comment` tags won't be output, and any Liquid code will be parsed, but not executed.

## Syntax

```oobleckTag
{% comment %}
  content
{% endcomment %}
```

### content

The content of the comment.

### Inline comments

## Syntax

```oobleckTag
{% # content %}
```

Inline comments prevent an expression inside of a tag `{% %}` from being rendered or output.

You can use inline comment tags to annotate your code, or to temporarily prevent logic in your code from executing.

You can create multi-line inline comments. However, each line in the tag must begin with a `#`, or a syntax error will occur.

##### Code

```liquid
{% # this is a comment %}

{% # for i in (1..3) -%}
  {{ i }}
{% # endfor %}

{%
  ###############################
  # This is a comment
  # across multiple lines
  ###############################
%}
```

### Inline comments inside `liquid` tags

You can use inline comment tags inside [`liquid` tags](https://shopify.dev/docs/api/liquid/tags/liquid). The tag must be used for each line that you want to comment.

##### Code

```liquid
{% liquid
  # this is a comment
  assign topic = 'Learning about comments!'
  echo topic
%}
```

##### Output

```html
Learning about comments!
```

</page>

<page>
---
title: 'Liquid tags: else'
description: >-
  Allows you to specify a default expression to execute when no other condition
  is met.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/conditional-else'
  md: 'https://shopify.dev/docs/api/liquid/tags/conditional-else.md'
---

# else

Allows you to specify a default expression to execute when no other condition is met.

You can use the `else` tag with the following tags:

* [`case`](https://shopify.dev/docs/api/liquid/tags/case)
* [`if`](https://shopify.dev/docs/api/liquid/tags/if)
* [`unless`](https://shopify.dev/docs/api/liquid/tags/unless)

## Syntax

```oobleckTag
{% else %}
  expression
```

### expression

The expression to render if no other condition is met.

##### Code

```liquid
{% if product.available %}
  This product is available!
{% else %}
  This product is sold out!
{% endif %}
```

##### Data

```json
{
  "product": {
    "available": true
  }
}
```

##### Output

```html
This product is available!
```

</page>

<page>
---
title: 'Liquid tags: content_for'
description: >-
  Creates a designated area in your
  [theme](https://shopify.dev/themes/architecture) where blocks can be rendered.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/content_for'
  md: 'https://shopify.dev/docs/api/liquid/tags/content_for.md'
---

# content\_​for

Creates a designated area in your [theme](https://shopify.dev/themes/architecture) where blocks can be rendered.

The `content_for` tag requires a type parameter to differentiate between rendering a number of theme blocks (`'blocks'`) and a single static block (`'block'`).

## Syntax

```oobleckTag
{% content_for 'blocks' %}
{% content_for 'block', type: "slide", id: "slide-1" %}
```

### blocks

## Syntax

```oobleckTag
{% content_for "blocks" %}
```

Creates a designated area that renders theme blocks as configured in the JSON template or section groups, allowing merchants to add, remove, and rearrange blocks using the theme editor. See [theme blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks/theme-blocks) for more information.

### block

## Syntax

```oobleckTag
{% content_for "block", type: "button", id: "static-block-1", color: "red" %}
```

Renders a static theme block of the specified type with the provided ID. You can pass additional arbitrary parameters (such as `color`) that will be accessible within the static block using `{{ color }}`. See [static blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks/theme-blocks/static-blocks) to learn more.

</page>

<page>
---
title: 'Liquid tags: continue'
description: >-
  Causes a [`for` loop](/docs/api/liquid/tags/for) to skip to the next
  iteration.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/continue'
  md: 'https://shopify.dev/docs/api/liquid/tags/continue.md'
---

# continue

Causes a [`for` loop](https://shopify.dev/docs/api/liquid/tags/for) to skip to the next iteration.

## Syntax

```oobleckTag
{% continue %}
```

##### Code

```liquid
{% for i in (1..5) -%}
  {%- if i == 4 -%}
    {% continue %}
  {%- else -%}
    {{ i }}
  {%- endif -%}
{%- endfor %}
```

##### Output

```html
1
2
3
5
```

</page>

<page>
---
title: 'Liquid tags: cycle'
description: >-
  Loops through a group of strings and outputs them one at a time for each
  iteration of a [`for` loop](/docs/api/liquid/tags/for).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/cycle'
  md: 'https://shopify.dev/docs/api/liquid/tags/cycle.md'
---

# cycle

Loops through a group of strings and outputs them one at a time for each iteration of a [`for` loop](https://shopify.dev/docs/api/liquid/tags/for).

The `cycle` tag must be used inside a `for` loop.

***

**Tip:** Use the \<code>cycle\</code> tag to output text in a predictable pattern. For example, to apply odd/even classes to rows in a table.

***

## Syntax

```oobleckTag
{% cycle string, string, ... %}
```

##### Code

```liquid
{% for i in (1..4) -%}
  {% cycle 'one', 'two', 'three' %}
{%- endfor %}
```

##### Output

```html
one
two
three
one
```

### Create unique cycle groups

## Syntax

```oobleckTag
{% cycle string: string, string, ... %}
```

If you include multiple `cycle` tags with the same parameters, in the same template, then each set of tags is treated as the same group. This means that it's possible for a `cycle` tag to output any of the provided strings, instead of always starting at the first string. To account for this, you can specify a group name for each `cycle` tag.

##### Code

```liquid
<!-- Iteration 1 -->
{% for i in (1..4) -%}
  {% cycle 'one', 'two', 'three' %}
{%- endfor %}

<!-- Iteration 2 -->
{% for i in (1..4) -%}
  {% cycle 'one', 'two', 'three' %}
{%- endfor %}

<!-- Iteration 3 -->
{% for i in (1..4) -%}
  {% cycle 'group_1': 'one', 'two', 'three' %}
{%- endfor %}

<!-- Iteration 4 -->
{% for i in (1..4) -%}
  {% cycle 'group_2': 'one', 'two', 'three' %}
{%- endfor %}
```

##### Output

```html
<!-- Iteration 1 -->
one
two
three
one


<!-- Iteration 2 -->
two
three
one
two


<!-- Iteration 3 -->
one
two
three
one


<!-- Iteration 4 -->
one
two
three
one
```

</page>

<page>
---
title: 'Liquid tags: decrement'
description: >-
  Creates a new variable, with a default value of -1, that's decreased by 1 with
  each subsequent call.


  > Caution:

  > Predefined Liquid objects can be overridden by variables with the same name.

  > To make sure that you can access all Liquid objects, make sure that your
  variable name doesn't match a predefined object's name.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/decrement'
  md: 'https://shopify.dev/docs/api/liquid/tags/decrement.md'
---

# decrement

Creates a new variable, with a default value of -1, that's decreased by 1 with each subsequent call.

***

**Caution:** Predefined Liquid objects can be overridden by variables with the same name. To make sure that you can access all Liquid objects, make sure that your variable name doesn\&#39;t match a predefined object\&#39;s name.

***

Variables that are declared with `decrement` are unique to the [layout](https://shopify.dev/themes/architecture/layouts), [template](https://shopify.dev/themes/architecture/templates), or [section](https://shopify.dev/themes/architecture/sections) file that they're created in. However, the variable is shared across [snippets](https://shopify.dev/themes/architecture/snippets) included in the file.

Similarly, variables that are created with `decrement` are independent from those created with [`assign`](https://shopify.dev/docs/api/liquid/tags/assign) and [`capture`](https://shopify.dev/docs/api/liquid/tags/capture). However, `decrement` and [`increment`](https://shopify.dev/docs/api/liquid/tags/increment) share variables.

## Syntax

```oobleckTag
{% decrement variable_name %}
```

### variable\_name

The name of the variable being decremented.

##### Code

```liquid
{% decrement variable %}
{% decrement variable %}
{% decrement variable %}
```

##### Output

```html
-1
-2
-3
```

</page>

<page>
---
title: 'Liquid tags: doc'
description: Documents template elements with annotations.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/doc'
  md: 'https://shopify.dev/docs/api/liquid/tags/doc.md'
---

# doc

Documents template elements with annotations.

The `doc` tag allows developers to include documentation within Liquid templates. Any content inside `doc` tags is not rendered or outputted. Liquid code inside will be parsed but not executed. This facilitates tooling support for features like code completion, linting, and inline documentation.

For detailed documentation syntax and examples, see the [`LiquidDoc` reference](https://shopify.dev/docs/storefronts/themes/tools/liquid-doc).

## Syntax

```oobleckTag
{% doc %}
  Renders a message.


  @param {string} foo - A string value.
  @param {string} [bar] - An optional string value.


  @example
  {% render 'message', foo: 'Hello', bar: 'World' %}
{% enddoc %}
```

</page>

<page>
---
title: 'Liquid tags: echo'
description: Outputs an expression.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/echo'
  md: 'https://shopify.dev/docs/api/liquid/tags/echo.md'
---

# echo

Outputs an expression.

Using the `echo` tag is the same as wrapping an expression in curly brackets (`{{` and `}}`). However, unlike the curly bracket method, you can use the `echo` tag inside [`liquid` tags](https://shopify.dev/docs/api/liquid/tags/liquid).

***

**Tip:** You can use \<a href="/docs/api/liquid/filters">filters\</a> on expressions inside \<code>echo\</code> tags.

***

## Syntax

```oobleckTag
{% liquid
  echo expression
%}
```

### expression

The expression to be output.

##### Code

```liquid
{% echo product.title %}

{% liquid
  echo product.price | money
%}
```

##### Data

```json
{
  "product": {
    "price": "10.00",
    "title": "Health potion"
  }
}
```

##### Output

```html
Health potion

$10.00
```

</page>

<page>
---
title: 'Liquid tags: for'
description: Renders an expression for every item in an array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/for'
  md: 'https://shopify.dev/docs/api/liquid/tags/for.md'
---

# for

Renders an expression for every item in an array.

You can do a maximum of 50 iterations with a `for` loop. If you need to iterate over more than 50 items, then use the [`paginate` tag](https://shopify.dev/docs/api/liquid/tags/paginate) to split the items over multiple pages.

***

**Tip:** Every \<code>for\</code> loop has an associated \<a href="/docs/api/liquid/objects/forloop">\<code>forloop\</code> object\</a> with information about the loop.

***

## Syntax

```oobleckTag
{% for variable in array %}
  expression
{% endfor %}
```

### variable

The current item in the array.

### array

The array to iterate over.

### expression

The expression to render for each iteration.

##### Code

```liquid
{% for product in collection.products -%}
  {{ product.title }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
Draught of Immortality
Glacier ice
Health potion
Invisibility potion
```

## for tag parameters

### limit

## Syntax

```oobleckTag
{% for variable in array limit: number %}
  expression
{% endfor %}
```

You can limit the number of iterations using the `limit` parameter.

***

**Tip:** Limit the amount of data fetched for arrays that can be paginated with the \<code>paginate\</code> tag instead of using the \<code>limit\</code> parameter. Learn more about \<a href="/docs/api/liquid/tags/paginate#paginate-limit-data-fetching">limiting data fetching\</a> for improved server-side performance.

***

##### Code

```liquid
{% for product in collection.products limit: 2 -%}
  {{ product.title }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
Draught of Immortality
Glacier ice
```

### offset

## Syntax

```oobleckTag
{% for variable in array offset: number %}
  expression
{% endfor %}
```

You can specify a 1-based index to start iterating at using the `offset` parameter.

##### Code

```liquid
{% for product in collection.products offset: 2 -%}
  {{ product.title }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
Health potion
Invisibility potion
```

### range

## Syntax

```oobleckTag
{% for variable in (number..number) %}
  expression
{% endfor %}
```

Instead of iterating over specific items in an array, you can specify a numeric range to iterate over.

***

**Note:** You can define the range using both literal and variable values.

***

##### Code

```liquid
{% for i in (1..3) -%}
  {{ i }}
{%- endfor %}

{%- assign lower_limit = 2 -%}
{%- assign upper_limit = 4 -%}

{% for i in (lower_limit..upper_limit) -%}
  {{ i }}
{%- endfor %}
```

##### Output

```html
1
2
3

2
3
4
```

### reversed

## Syntax

```oobleckTag
{% for variable in array reversed %}
  expression
{% endfor %}
```

You can iterate in reverse order using the `reversed` parameter.

##### Code

```liquid
{% for product in collection.products reversed -%}
  {{ product.title }}
{%- endfor %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
Invisibility potion
Health potion
Glacier ice
Draught of Immortality
```

# forloop**object**

Information about a parent [`for` loop](https://shopify.dev/docs/api/liquid/tags/for).

## Properties

* * **first**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current iteration is the first. Returns `false` if not.

  * **index**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current iteration.

  * **index0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current iteration.

  * **last**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current iteration is the last. Returns `false` if not.

  * **length**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of iterations in the loop.

  * **parentloop**

    [forloop](https://shopify.dev/docs/api/liquid/objects/forloop)

  * The parent `forloop` object.

    If the current `for` loop isn't nested inside another `for` loop, then `nil` is returned.

    ExampleUse the `parentloop` property

    ##### Code

    ```liquid
    {% for i in (1..3) -%}
      {% for j in (1..3) -%}
        {{ forloop.parentloop.index }} - {{ forloop.index }}
      {%- endfor %}
    {%- endfor %}
    ```

    ##### Output

    ```html
    1 - 1
    1 - 2
    1 - 3

    2 - 1
    2 - 2
    2 - 3

    3 - 1
    3 - 2
    3 - 3
    ```

  * **rindex**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current iteration, in reverse order.

  * **rindex0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current iteration, in reverse order.

##### Example

```json
{
  "first": true,
  "index": 1,
  "index0": 0,
  "last": false,
  "length": 4,
  "rindex": 3
}
```

### Use the `forloop` object

##### Code

```liquid
{% for page in pages -%}
  {%- if forloop.length > 0 -%}
    {{ page.title }}{% unless forloop.last %}, {% endunless -%}
  {%- endif -%}
{% endfor %}
```

##### Output

```html
About us, Contact, Potion dosages
```

</page>

<page>
---
title: 'Liquid tags: form'
description: >-
  Generates an HTML `<form>` tag, including any required `<input>` tags to
  submit the form to a specific endpoint.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/form'
  md: 'https://shopify.dev/docs/api/liquid/tags/form.md'
---

# form

Generates an HTML `<form>` tag, including any required `<input>` tags to submit the form to a specific endpoint.

Because there are many different form types available in Shopify themes, the `form` tag requires a type. Depending on the form type, an additional parameter might be required. You can specify the following form types:

* [`activate_customer_password`](https://shopify.dev/docs/api/liquid/tags/form#form-activate_customer_password)
* [`cart`](https://shopify.dev/docs/api/liquid/tags/form#form-cart)
* [`contact`](https://shopify.dev/docs/api/liquid/tags/form#form-contact)
* [`create_customer`](https://shopify.dev/docs/api/liquid/tags/form#form-create_customer)
* [`currency`](https://shopify.dev/docs/api/liquid/tags/form#form-currency)
* [`customer`](https://shopify.dev/docs/api/liquid/tags/form#form-customer)
* [`customer_address`](https://shopify.dev/docs/api/liquid/tags/form#form-customer_address)
* [`customer_login`](https://shopify.dev/docs/api/liquid/tags/form#form-customer_login)
* [`guest_login`](https://shopify.dev/docs/api/liquid/tags/form#form-guest_login)
* [`localization`](https://shopify.dev/docs/api/liquid/tags/form#form-localization)
* [`new_comment`](https://shopify.dev/docs/api/liquid/tags/form#form-new_comment)
* [`product`](https://shopify.dev/docs/api/liquid/tags/form#form-product)
* [`recover_customer_password`](https://shopify.dev/docs/api/liquid/tags/form#form-recover_customer_password)
* [`reset_customer_password`](https://shopify.dev/docs/api/liquid/tags/form#form-reset_customer_password)
* [`storefront_password`](https://shopify.dev/docs/api/liquid/tags/form#form-storefront_password)

## Syntax

```oobleckTag
{% form 'form_type' %}
  content
{% endform %}
```

### form\_type

The name of the desired form type

### content

The form contents

### activate\_customer\_password

## Syntax

```oobleckTag
{% form 'activate_customer_password', article %}
  form_content
{% endform %}
```

Generates a form for activating a customer account. To learn more about using this form, and its contents, refer to the [`customers/activate_account` template](https://shopify.dev/themes/architecture/templates/customers-activate-account#content).

##### Code

```liquid
{% form 'activate_customer_password' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/account/activate" accept-charset="UTF-8"><input type="hidden" name="form_type" value="activate_customer_password" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### cart

## Syntax

```oobleckTag
{% form 'cart', cart %}
  form_content
{% endform %}
```

Generates a form for creating a checkout based on the items currently in the cart. The `cart` form requires a [`cart` object](https://shopify.dev/docs/api/liquid/objects/cart) as a parameter. To learn more about using the cart form in your theme, refer to the [`cart` template](https://shopify.dev/themes/architecture/templates/cart#proceed-to-checkout).

##### Code

```liquid
{% form 'cart', cart %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/cart" id="cart_form" accept-charset="UTF-8" class="shopify-cart-form" enctype="multipart/form-data"><input type="hidden" name="form_type" value="cart" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### contact

## Syntax

```oobleckTag
{% form 'contact' %}
  form_content
{% endform %}
```

Generates a form for submitting an email to the merchant. To learn more about using this form in your theme, refer to [Add a contact form to your theme](https://shopify.dev/themes/customer-engagement/add-contact-form).

***

**Tip:** To learn more about the merchant experience of receiving submissions, refer to \<a href="https://help.shopify.com/manual/online-store/themes/customizing-themes/add-contact-page#view-contact-form-submissions">the Shopify Help Center\</a>.

***

##### Code

```liquid
{% form 'contact' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/contact#contact_form" id="contact_form" accept-charset="UTF-8" class="contact-form"><input type="hidden" name="form_type" value="contact" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### create\_customer

## Syntax

```oobleckTag
{% form 'create_customer' %}
  form_content
{% endform %}
```

Generates a form for creating a new customer account. To learn more about using this form, and its contents, refer to the [`customers/register` template](https://shopify.dev/themes/architecture/templates/customers-register#content).

##### Code

```liquid
{% form 'create_customer' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/account" id="create_customer" accept-charset="UTF-8" data-login-with-shop-sign-up="true"><input type="hidden" name="form_type" value="create_customer" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### currency

## Syntax

```oobleckTag
{% form 'currency' %}
  form_content
{% endform %}
```

***

**Deprecated:** The \<code>currency\</code> form is deprecated and has been replaced by the \<a href="/docs/api/liquid/tags/form#form-localization">\<code>localization\</code> form\</a>.

***

Generates a form for customers to select their preferred currency.

***

**Tip:** Use the \<a href="/docs/api/liquid/filters/currency\_selector">\<code>\<span class="PreventFireFoxApplyingGapToWBR">currency\<wbr/>\_selector\</span>\</code> filter\</a> to include a currency selector inside the form.

***

##### Code

```liquid
{% form 'currency' %}
  {{ form | currency_selector }}
{% endform %}
```

##### Output

```html
<form method="post" action="/cart/update" id="currency_form" accept-charset="UTF-8" class="shopify-currency-form" enctype="multipart/form-data"><input type="hidden" name="form_type" value="currency" /><input type="hidden" name="utf8" value="✓" /><input type="hidden" name="return_to" value="/services/liquid_rendering/resource" />
  <select name="currency"><option value="AED">AED د.إ</option><option value="AFN">AFN ؋</option><option value="AUD">AUD $</option><option value="CAD" selected="selected">CAD $</option><option value="CHF">CHF CHF</option><option value="CZK">CZK Kč</option><option value="DKK">DKK kr.</option><option value="EUR">EUR €</option><option value="GBP">GBP £</option><option value="HKD">HKD $</option><option value="ILS">ILS ₪</option><option value="JPY">JPY ¥</option><option value="KRW">KRW ₩</option><option value="MYR">MYR RM</option><option value="NZD">NZD $</option><option value="PLN">PLN zł</option><option value="SEK">SEK kr</option><option value="SGD">SGD $</option><option value="USD">USD $</option></select>
</form>
```

### customer

## Syntax

```oobleckTag
{% form 'customer' %}
  form_content
{% endform %}
```

Generates a form for creating a new customer without registering a new account. This form is useful for collecting customer information when you don't want customers to log in to your store, such as building a list of emails from a newsletter signup.

***

**Tip:** To generate a form that registers a customer account, use the \<a href="/docs/api/liquid/tags/form#form-create\_customer">\<code>\<span class="PreventFireFoxApplyingGapToWBR">create\<wbr/>\_customer\</span>\</code> form\</a>.

***

To learn more about using this form, and its contents, refer to [Email consent](https://shopify.dev/themes/customer-engagement/email-consent#newsletter-sign-up-form).

##### Code

```liquid
{% form 'customer' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/contact#contact_form" id="contact_form" accept-charset="UTF-8" class="contact-form"><input type="hidden" name="form_type" value="customer" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### customer\_address

## Syntax

```oobleckTag
{% form 'customer_address', address_type %}
  form_content
{% endform %}
```

Generates a form for creating a new address on a customer account, or editing an existing one. The `customer_address` form requires a specific parameter, depending on whether a new address is being created or an existing one is being edited:

| Parameter value | Use-case |
| - | - |
| `customer.new_address` | When a new address is being created. |
| `address` | When an existing address is being edited. |

To learn more about using this form, and its contents, refer to the [`customers/addresses` template](https://shopify.dev/themes/architecture/templates/customers-addresses#content).

##### Code

```liquid
{% form 'customer_address', customer.new_address %}
  <!-- form content -->
{% endform %}
```

##### Data

```json
{
  "customer": {
    "new_address": {}
  }
}
```

##### Output

```html
<form method="post" action="/account/addresses" id="address_form_new" accept-charset="UTF-8"><input type="hidden" name="form_type" value="customer_address" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### customer\_login

## Syntax

```oobleckTag
{% form 'customer_login' %}
  form_content
{% endform %}
```

Generates a form for logging into a customer account. To learn more about using this form, and its contents, refer to the [`customers/login` template](https://shopify.dev/themes/architecture/templates/customers-login#the-customer-login-form).

##### Code

```liquid
{% form 'customer_login' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/account/login" id="customer_login" accept-charset="UTF-8" data-login-with-shop-sign-in="true"><input type="hidden" name="form_type" value="customer_login" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### guest\_login

## Syntax

```oobleckTag
{% form 'guest_login' %}
  form_content
{% endform %}
```

Generates a form, for use in the [`customers/login` template](https://shopify.dev/themes/architecture/templates/customers-login), that directs customers back to their checkout session as a guest instead of logging in to an account. To learn more about using this form, and its contents, refer to [Offer guest checkout](https://shopify.dev/themes/architecture/templates/customers-login#offer-guest-checkout).

##### Code

```liquid
{% form 'guest_login' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/account/login" id="customer_login_guest" accept-charset="UTF-8"><input type="hidden" name="form_type" value="guest_login" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
<input type="hidden" name="guest" value="true" /></form>
```

### localization

## Syntax

```oobleckTag
{% form 'localization' %}
  form_content
{% endform %}
```

Generates a form for customers to select their preferred country so that they're shown the appropriate language and currency. The `localization` form can contain one of two selectors:

* A country selector
* A language selector

***

**Note:** The \<code>localization\</code> form replaces the deprecated \<a href="/docs/api/liquid/tags/form#form-currency">\<code>currency\</code> form\</a>.

***

To learn more about using this form, and its contents, refer to [Support multiple currencies and languages](https://shopify.dev/themes/internationalization/multiple-currencies-languages).

##### Code

```liquid
{% form 'localization' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/localization" id="localization_form" accept-charset="UTF-8" class="shopify-localization-form" enctype="multipart/form-data"><input type="hidden" name="form_type" value="localization" /><input type="hidden" name="utf8" value="✓" /><input type="hidden" name="_method" value="put" /><input type="hidden" name="return_to" value="/services/liquid_rendering/resource" />
  <!-- form content -->
</form>
```

### new\_comment

## Syntax

```oobleckTag
{% form 'new_comment', article %}
  form_content
{% endform %}
```

Generates a form for creating a new comment on an article. The `new_comment` form requires an [`article` object](https://shopify.dev/docs/api/liquid/objects/article) as a parameter. To learn more about using this form, and its contents, refer to the [`article` template](https://shopify.dev/themes/architecture/templates/article#the-comment-form).

##### Code

```liquid
{% form 'new_comment', article %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/blogs/potion-notions/how-to-tell-if-you-have-run-out-of-invisibility-potion/comments#comment_form" id="comment_form" accept-charset="UTF-8" class="comment-form"><input type="hidden" name="form_type" value="new_comment" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### product

## Syntax

```oobleckTag
{% form 'product', product %}
  form_content
{% endform %}
```

Generates a form for adding a product variant to the cart. The `product` form requires a [`product` object](https://shopify.dev/docs/api/liquid/objects/product) as a parameter. To learn more about using this form, and its contents, refer to the [`product` template](https://shopify.dev/themes/architecture/templates/product#the-product-form).

##### Code

```liquid
{% form 'product', product %}
  <!-- form content -->
{% endform %}
```

##### Data

```json
{
  "product": {
    "id": 6786188247105
  }
}
```

##### Output

```html
<form method="post" action="/cart/add" id="product_form_6786188247105" accept-charset="UTF-8" class="shopify-product-form" enctype="multipart/form-data"><input type="hidden" name="form_type" value="product" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
<input type="hidden" name="product-id" value="6786188247105" /></form>
```

### recover\_customer\_password

## Syntax

```oobleckTag
{% form 'recover_customer_password' %}
  form_content
{% endform %}
```

Generates a form, for use in the [`customers/login` template](https://shopify.dev/themes/architecture/templates/customers-login), for a customer to recover a lost or forgotten password. To learn more about using this form, and its contents, refer to [Provide a "Forgot your password" option](https://shopify.dev/themes/architecture/templates/customers-login#provide-a-forgot-your-password-option).

##### Code

```liquid
{% form 'recover_customer_password' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/account/recover" accept-charset="UTF-8"><input type="hidden" name="form_type" value="recover_customer_password" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### reset\_customer\_password

## Syntax

```oobleckTag
{% form 'reset_customer_password' %}
  form_content
{% endform %}
```

Generates a form for a customer to reset their password. To learn more about using this form, and its contents, refer to the [`customers/reset_password` template](https://shopify.dev/themes/architecture/templates/customers-reset-password#content).

##### Code

```liquid
{% form 'reset_customer_password' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/account/reset" accept-charset="UTF-8"><input type="hidden" name="form_type" value="reset_customer_password" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

### storefront\_password

## Syntax

```oobleckTag
{% form 'storefront_password' %}
  form_content
{% endform %}
```

Generates a form for entering a password protected storefront. To learn more about using this form, and its contents, refer to the [`password` template](https://shopify.dev/themes/architecture/templates/password#the-password-form).

##### Code

```liquid
{% form 'storefront_password' %}
  <!-- form content -->
{% endform %}
```

##### Output

```html
<form method="post" action="/password" id="login_form" accept-charset="UTF-8" class="storefront-password-form"><input type="hidden" name="form_type" value="storefront_password" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
</form>
```

## form tag parameters

### return\_to

## Syntax

```oobleckTag
{% form 'form_type', return_to: string %}
  content
{% endform %}
```

By default, each form type redirects customers to a specific page after the form submits. For example, the `product` form redirects to the cart page.

The `return_to` parameter allows you to specify a URL to redirect to. This can be done with the following values:

| Value | Description |
| - | - |
| `back` | Redirect back to the same page that the customer was on before submitting the form. |
| A relative path | A specific URL path. For example `/collections/all`. |
| A [`routes` attribute](https://shopify.dev/docs/api/liquid/objects/routes) | For example, `routes.root_url` |

##### Code

```liquid
{% form 'customer_login', return_to: routes.root_url %}
  <!-- form content -->
{% endform %}
```

##### Data

```json
{
  "routes": {
    "root_url": "/"
  }
}
```

##### Output

```html
<form method="post" action="/account/login" id="customer_login" accept-charset="UTF-8" data-login-with-shop-sign-in="true"><input type="hidden" name="form_type" value="customer_login" /><input type="hidden" name="utf8" value="✓" /><input type="hidden" name="return_to" value="/" />
  <!-- form content -->
</form>
```

### HTML attributes

## Syntax

```oobleckTag
{% form 'form_type', attribute: string %}
  content
{% endform %}
```

You can specify [HTML attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attributes) by adding a parameter that matches the attribute name with `data-` prepended, and the desired value.

##### Code

```liquid
{% form "product", product, id: 'custom-id', class: 'custom-class', data-example: '100' %}
  <!-- form content -->
{% endform %}
```

##### Data

```json
{
  "product": {
    "id": 6786188247105
  }
}
```

##### Output

```html
<form method="post" action="/cart/add" id="custom-id" accept-charset="UTF-8" class="custom-class" enctype="multipart/form-data" data-example="100"><input type="hidden" name="form_type" value="product" /><input type="hidden" name="utf8" value="✓" />
  <!-- form content -->
<input type="hidden" name="product-id" value="6786188247105" /></form>
```

</page>

<page>
---
title: 'Liquid tags: if'
description: Renders an expression if a specific condition is `true`.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/if'
  md: 'https://shopify.dev/docs/api/liquid/tags/if.md'
---

# if

Renders an expression if a specific condition is `true`.

## Syntax

```oobleckTag
{% if condition %}
  expression
{% endif %}
```

### condition

The condition to evaluate.

### expression

The expression to render if the condition is met.

##### Code

```liquid
{% if product.compare_at_price > product.price %}
  This product is on sale!
{% endif %}
```

##### Data

```json
{
  "product": {
    "compare_at_price": "10.00",
    "price": "0.00"
  }
}
```

##### Output

```html
This product is on sale!
```

### elsif

You can use the `elsif` tag to check for multiple conditions.

##### Code

```liquid
{% if product.type == 'Love' %}
  This is a love potion!
{% elsif product.type == 'Health' %}
  This is a health potion!
{% endif %}
```

##### Data

```json
{
  "product": {
    "type": null
  }
}
```

##### Output

```html
This is a health potion!
```

</page>

<page>
---
title: 'Liquid tags: include'
description: 'Renders a [snippet](/themes/architecture/snippets).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/include'
  md: 'https://shopify.dev/docs/api/liquid/tags/include.md'
---

# include

Renders a [snippet](https://shopify.dev/themes/architecture/snippets).

Inside the snippet, you can access and alter variables that are [created](https://shopify.dev/docs/api/liquid/tags/variable-tags) outside of the snippet.

**Deprecated:**

Deprecated because the way that variables are handled reduces performance and makes code harder to both read and maintain.

The `include` tag has been replaced by [`render`](https://shopify.dev/docs/api/liquid/tags/render).

## Syntax

```oobleckTag
{% include 'filename' %}
```

### filename

The name of the snippet to render, without the `.liquid` extension.

</page>

<page>
---
title: 'Liquid tags: increment'
description: >-
  Creates a new variable, with a default value of 0, that's increased by 1 with
  each subsequent call.


  > Caution:

  > Predefined Liquid objects can be overridden by variables with the same name.

  > To make sure that you can access all Liquid objects, make sure that your
  variable name doesn't match a predefined object's name.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/increment'
  md: 'https://shopify.dev/docs/api/liquid/tags/increment.md'
---

# increment

Creates a new variable, with a default value of 0, that's increased by 1 with each subsequent call.

***

**Caution:** Predefined Liquid objects can be overridden by variables with the same name. To make sure that you can access all Liquid objects, make sure that your variable name doesn\&#39;t match a predefined object\&#39;s name.

***

Variables that are declared with `increment` are unique to the [layout](https://shopify.dev/themes/architecture/layouts), [template](https://shopify.dev/themes/architecture/templates), or [section](https://shopify.dev/themes/architecture/sections) file that they're created in. However, the variable is shared across [snippets](https://shopify.dev/themes/architecture/snippets) included in the file.

Similarly, variables that are created with `increment` are independent from those created with [`assign`](https://shopify.dev/docs/api/liquid/tags/assign) and [`capture`](https://shopify.dev/docs/api/liquid/tags/capture). However, `increment` and [`decrement`](https://shopify.dev/docs/api/liquid/tags/decrement) share variables.

## Syntax

```oobleckTag
{% increment variable_name %}
```

### variable\_name

The name of the variable being incremented.

##### Code

```liquid
{% increment variable %}
{% increment variable %}
{% increment variable %}
```

##### Output

```html
0
1
2
```

</page>

<page>
---
title: 'Liquid tags: else'
description: >-
  Allows you to specify a default expression to execute when a [`for`
  loop](/docs/api/liquid/tags/for) has zero length.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/iteration-else'
  md: 'https://shopify.dev/docs/api/liquid/tags/iteration-else.md'
---

# else

Allows you to specify a default expression to execute when a [`for` loop](https://shopify.dev/docs/api/liquid/tags/for) has zero length.

## Syntax

```oobleckTag
{% for variable in array %}
  first_expression
{% else %}
  second_expression
{% endfor %}
```

### variable

The current item in the array.

### array

The array to iterate over.

### first\_expression

The expression to render for each iteration.

### second\_expression

The expression to render if the loop has zero length.

##### Code

```liquid
{% for product in collection.products %}
  {{ product.title }}<br>
{% else %}
  There are no products in this collection.
{% endfor %}
```

##### Data

```json
{
  "collection": {
    "products": []
  }
}
```

##### Output

```html
There are no products in this collection.
```

</page>

<page>
---
title: 'Liquid tags: javascript'
description: >-
  JavaScript code included in
  [section](/storefronts/themes/architecture/sections),
  [block](/storefronts/themes/architecture/blocks) and
  [snippet](/storefronts/themes/architecture/snippets) files.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/javascript'
  md: 'https://shopify.dev/docs/api/liquid/tags/javascript.md'
---

# javascript

JavaScript code included in [section](https://shopify.dev/storefronts/themes/architecture/sections), [block](https://shopify.dev/storefronts/themes/architecture/blocks) and [snippet](https://shopify.dev/storefronts/themes/architecture/snippets) files.

Each section, block or snippet can have only one `{% javascript %}` tag.

To learn more about how JavaScript that's defined between the `javascript` tags is loaded and run, refer to the documentation for [javascript tags](https://shopify.dev/storefronts/themes/best-practices/javascript-and-stylesheet-tags#javascript).

***

**Caution:** Liquid isn\&#39;t rendered inside of \<code>{% javascript %}\</code> tags. Including Liquid code can cause syntax errors.

***

## Syntax

```oobleckTag
{% javascript %}
  javascript_code
{% endjavascript %}
```

### javascript\_code

The JavaScript code for the section, block or snippet.

</page>

<page>
---
title: 'Liquid tags: layout'
description: 'Specify which [layout](/themes/architecture/layouts) to use.'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/layout'
  md: 'https://shopify.dev/docs/api/liquid/tags/layout.md'
---

# layout

Specify which [layout](https://shopify.dev/themes/architecture/layouts) to use.

## Syntax

```oobleckTag
{% layout name %}
```

### name

The name of the layout file you want to use, wrapped in quotes, or `none` for no layout.

By default, the `theme.liquid` layout is used. The `layout` tag allows you to specify an alternate layout, or use no layout.

```liquid
{% layout 'full-width' %}
{% layout none %}
```

```liquid
{% layout 'full-width' %}
{% layout none %}
```

</page>

<page>
---
title: 'Liquid tags: liquid'
description: Allows you to have a block of Liquid without delimeters on each tag.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/liquid'
  md: 'https://shopify.dev/docs/api/liquid/tags/liquid.md'
---

# liquid

Allows you to have a block of Liquid without delimeters on each tag.

Because the tags don't have delimeters, each tag needs to be on its own line.

***

**Tip:** Use the \<a href="/docs/api/liquid/tags/echo">\<code>echo\</code> tag\</a> to output an expression inside \<code>liquid\</code> tags.

***

## Syntax

```oobleckTag
{% liquid
  expression
%}
```

### expression

The expression to be rendered inside the `liquid` tag.

##### Code

```liquid
{% liquid
  # Show a message that's customized to the product type

  assign product_type = product.type | downcase
  assign message = ''

  case product_type
    when 'health'
      assign message = 'This is a health potion!'
    when 'love'
      assign message = 'This is a love potion!'
    else
      assign message = 'This is a potion!'
  endcase

  echo message
%}
```

##### Data

```json
{
  "product": {
    "type": null
  }
}
```

##### Output

```html
This is a health potion!
```

</page>

<page>
---
title: 'Liquid tags: paginate'
description: Splits an array's items across multiple pages.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/paginate'
  md: 'https://shopify.dev/docs/api/liquid/tags/paginate.md'
---

# paginate

Splits an array's items across multiple pages.

Because [`for` loops](https://shopify.dev/docs/api/liquid/tags/for) are limited to 50 iterations per page, you need to use the `paginate` tag to iterate over an array that has more than 50 items. The following arrays can be paginated:

* [`article.comments`](https://shopify.dev/docs/api/liquid/objects/article#article-comments)
* [`blog.articles`](https://shopify.dev/docs/api/liquid/objects/blog#blog-articles)
* [`collections`](https://shopify.dev/docs/api/liquid/objects/collections)
* [`collection.products`](https://shopify.dev/docs/api/liquid/objects/collection#collection-products)
* [`customer.addresses`](https://shopify.dev/docs/api/liquid/objects/customer#customer-addresses)
* [`customer.orders`](https://shopify.dev/docs/api/liquid/objects/customer#customer-orders)
* [`metaobject_definition.values`](https://shopify.dev/docs/api/liquid/objects/metaobject_definition#metaobject_definition-values)
* [`pages`](https://shopify.dev/docs/api/liquid/objects/pages)
* [`product.variants`](https://shopify.dev/docs/api/liquid/objects/product#variants)
* [`search.results`](https://shopify.dev/docs/api/liquid/objects/search#search-results)
* [`article_list` settings](https://shopify.dev/themes/architecture/settings/input-settings#article_list)
* [`collection_list` settings](https://shopify.dev/themes/architecture/settings/input-settings#collection_list)
* [`product_list` settings](https://shopify.dev/themes/architecture/settings/input-settings#product_list)

Within the `paginate` tag, you have access to the [`paginate` object](https://shopify.dev/docs/api/liquid/objects/paginate). You can use this object, or the [`default_pagination` filter](https://shopify.dev/docs/api/liquid/filters/default_pagination), to build page navigation.

***

**Note:** The \<code>paginate\</code> tag allows the user to paginate to the 25,000th item in the array and no further. To reach items further in the array the array should be filtered further before paginating. See \<a href="/themes/best-practices/performance/platform#pagination-limits">Pagination Limits\</a> for more information.

***

## Syntax

```oobleckTag
{% paginate array by page_size %}
  {% for item in array %}
    forloop_content
  {% endfor %}
{% endpaginate %}
```

### array

The array to be looped over.

### page\_size

The number of array items to include per page, between 1 and 250.

### item

An item in the array being looped.

### forloop\_content

Content for each loop iteration.

##### Code

```liquid
{% paginate collection.products by 5 %}
  {% for product in collection.products -%}
    {{ product.title }}
  {%- endfor %}

  {{- paginate | default_pagination }}
{% endpaginate %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower"
      },
      {
        "title": "Charcoal"
      },
      {
        "title": "Crocodile tears"
      },
      {
        "title": "Dandelion milk"
      },
      {
        "title": "Draught of Immortality"
      }
    ],
    "products_count": 19
  }
}
```

##### Output

```html
Blue Mountain Flower
Charcoal
Crocodile tears
Dandelion milk
Draught of Immortality

<span class="page current">1</span> <span class="page"><a href="/services/liquid_rendering/resource?page=2" title="">2</a></span> <span class="page"><a href="/services/liquid_rendering/resource?page=3" title="">3</a></span> <span class="page"><a href="/services/liquid_rendering/resource?page=4" title="">4</a></span> <span class="next"><a href="/services/liquid_rendering/resource?page=2" title="">Next &raquo;</a></span>
```

### Paginating setting arrays

To allow the pagination of `article_list`, `collection_list` and `product_list` settings to operate independently from other paginated lists on a page, these lists use a pagination query parameter with a unique key. The key is automatically assigned by the `paginate` tag, and you don't need to reference the key in your code. However, you can access the key using [`paginate.page_param`](https://shopify.dev/docs/api/liquid/objects/paginate#paginate-page_param).

***

**Tip:** To paginate two arrays independently without refreshing the entire page, you can use the \<a href="/docs/api/ajax/section-rendering">Section Rendering API\</a>.

***

### Limit data fetching

The [`limit` parameter](https://shopify.dev/docs/api/liquid/tags/for#for-limit) of the `for` tag controls the number of iterations, but not the amount of information fetched. Using the `paginate` tag with a matching `page_size` can reduce the data queried, leading to faster server response times.

For example, referencing `collection.products` will fetch up to 50 products by default, regardless of the forloop's `limit` parameter. Use `paginate` and set a `page_size` to limit the amount of data fetched, and opt not to display any pagination controls.

More data than requested in a specific section may be returned. Because of this, make sure to include both `paginate` and `limit` when using this technique.

##### Code

```liquid
{% paginate collection.products by 4 %}
  {% for product in collection.products limit: 4 -%}
    {{ product.title }}
  {%- endfor %}
{% endpaginate -%}

<!-- Less performant method -->
{% for product in collection.products limit: 4 -%}
  {{ product.title }}
{%- endfor -%}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower"
      },
      {
        "title": "Charcoal"
      },
      {
        "title": "Crocodile tears"
      },
      {
        "title": "Dandelion milk"
      }
    ],
    "products_count": 19
  }
}
```

##### Output

```html
Blue Mountain Flower
Charcoal
Crocodile tears
Dandelion milk

<!-- Less performant method -->
Blue Mountain Flower
Charcoal
Crocodile tears
Dandelion milk
```

## paginate tag parameters

### window\_size

## Syntax

```oobleckTag
{% paginate collection.products by 3, window_size: 1 %}
```

Set the window size of the pagination. The window size is the number of pages that should be visible in the pagination navigation.

##### Code

```liquid
{% paginate collection.products by 3, window_size: 1 %}
  {% for product in collection.products -%}
    {{ product.title }}
  {%- endfor %}

  {{- paginate | default_pagination }}
{% endpaginate %}
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Blue Mountain Flower"
      },
      {
        "title": "Charcoal"
      },
      {
        "title": "Crocodile tears"
      }
    ],
    "products_count": 19
  }
}
```

##### Output

```html
Blue Mountain Flower
Charcoal
Crocodile tears

<span class="page current">1</span> <span class="deco">&hellip;</span> <span class="page"><a href="/services/liquid_rendering/resource?page=7" title="">7</a></span> <span class="next"><a href="/services/liquid_rendering/resource?page=2" title="">Next &raquo;</a></span>
```

</page>

<page>
---
title: 'Liquid tags: raw'
description: Outputs any Liquid code as text instead of rendering it.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/raw'
  md: 'https://shopify.dev/docs/api/liquid/tags/raw.md'
---

# raw

Outputs any Liquid code as text instead of rendering it.

## Syntax

```oobleckTag
{% raw %}
  expression
{% endraw %}
```

### expression

The expression to be output without being rendered.

##### Code

```liquid
{% raw %}
{{ 2 | plus: 2 }} equals 4.
{% endraw %}
```

##### Output

```html
{{ 2 | plus: 2 }} equals 4.
```

</page>

<page>
---
title: 'Liquid tags: render'
description: >-
  Renders a [snippet](/themes/architecture/snippets) or [app
  block](/themes/architecture/sections/section-schema#render-app-blocks).
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/render'
  md: 'https://shopify.dev/docs/api/liquid/tags/render.md'
---

# render

Renders a [snippet](https://shopify.dev/themes/architecture/snippets) or [app block](https://shopify.dev/themes/architecture/sections/section-schema#render-app-blocks).

Inside snippets and app blocks, you can't directly access variables that are [created](https://shopify.dev/docs/api/liquid/tags/variable-tags) outside of the snippet or app block. However, you can [specify variables as parameters](https://shopify.dev/docs/api/liquid/tags/render#render-passing-variables-to-a-snippet) to pass outside variables to snippets.

While you can't directly access created variables, you can access global objects, as well as any objects that are directly accessible outside the snippet or app block. For example, a snippet or app block inside the [product template](https://shopify.dev/themes/architecture/templates/product) can access the [`product` object](https://shopify.dev/docs/api/liquid/objects/product), and a snippet or app block inside a [section](https://shopify.dev/themes/architecture/sections) can access the [`section` object](https://shopify.dev/docs/api/liquid/objects/section).

Outside a snippet or app block, you can't access variables created inside the snippet or app block.

***

**Note:** When you render a snippet using the \<code>render\</code> tag, you can\&#39;t use the \<a href="/docs/api/liquid/tags/include">\<code>include\</code> tag\</a> inside the snippet.

***

## Syntax

```oobleckTag
{% render 'filename' %}
```

### filename

The name of the snippet to render, without the `.liquid` extension.

## render tag parameters

### for

## Syntax

```oobleckTag
{% render 'filename' for array as item %}
```

You can render a snippet for every item in an array using the `for` parameter. You can also supply an optional `as` parameter to be able to reference the current item in the iteration inside the snippet. Additionally, you can access a [`forloop` object](https://shopify.dev/docs/api/liquid/objects/forloop) for the loop inside the snippet.

### Passing variables to a snippet

## Syntax

```oobleckTag
{% render 'filename', variable: value %}
```

Variables that have been [created](https://shopify.dev/docs/api/liquid/tags/variable-tags) outside of a snippet can be passed to a snippet as parameters on the `render` tag.

***

**Note:** Any changes that are made to a passed variable apply only within the snippet.

***

### with

## Syntax

```oobleckTag
{% render 'filename' with object as name %}
```

You can pass a single object to a snippet using the `with` parameter. You can also supply an optional `as` parameter to specify a custom name to reference the object inside the snippet. If you don't use the `as` parameter to specify a custom name, then you can reference the object using the snippet filename.

</page>

<page>
---
title: 'Liquid tags: section'
description: 'Renders a [section](/themes/architecture/sections).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/section'
  md: 'https://shopify.dev/docs/api/liquid/tags/section.md'
---

# section

Renders a [section](https://shopify.dev/themes/architecture/sections).

Rendering a section with the `section` tag renders a section statically. To learn more about sections and how to use them in your theme, refer to [Render a section](https://shopify.dev/themes/architecture/sections#render-a-section).

## Syntax

```oobleckTag
{% section 'name' %}
```

### name

The name of the section file you want to render.

##### Code

```liquid
{% section 'header' %}
```

##### Data

```json
{
  "cart": {
    "item_count": 2
  },
  "request": {
    "origin": "https://polinas-potent-potions.myshopify.com",
    "page_type": "index"
  },
  "routes": {
    "account_url": "/account",
    "cart_url": "/cart",
    "root_url": "/",
    "search_url": "/search"
  },
  "settings": {
    "accent_icons": "text",
    "cart_type": "notification",
    "inputs_shadow_vertical_offset": 4,
    "predictive_search_enabled": true,
    "social_facebook_link": "",
    "social_instagram_link": "",
    "social_pinterest_link": "",
    "social_snapchat_link": "",
    "social_tiktok_link": "",
    "social_tumblr_link": "",
    "social_twitter_link": "",
    "social_vimeo_link": "",
    "social_youtube_link": ""
  },
  "shop": {
    "customer_accounts_enabled": true,
    "name": "Polina&#39;s Potent Potions"
  }
}
```

##### Output

```html
<div id="shopify-section-header" class="shopify-section section-header"><link rel="stylesheet" href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-list-menu.css?v=151968516119678728991663872413" media="print" onload="this.media='all'">
<link rel="stylesheet" href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-search.css?v=96455689198851321781663872411" media="print" onload="this.media='all'">
<link rel="stylesheet" href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-menu-drawer.css?v=182311192829367774911663872416" media="print" onload="this.media='all'">
<link rel="stylesheet" href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-cart-notification.css?v=183358051719344305851663872409" media="print" onload="this.media='all'">
<link rel="stylesheet" href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-cart-items.css?v=23917223812499722491663872408" media="print" onload="this.media='all'"><link rel="stylesheet" href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-price.css?v=65402837579211014041663872409" media="print" onload="this.media='all'">
  <link rel="stylesheet" href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-loading-overlay.css?v=167310470843593579841663872415" media="print" onload="this.media='all'"><noscript><link href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-list-menu.css?v=151968516119678728991663872413" rel="stylesheet" type="text/css" media="all" /></noscript>
<noscript><link href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-search.css?v=96455689198851321781663872411" rel="stylesheet" type="text/css" media="all" /></noscript>
<noscript><link href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-menu-drawer.css?v=182311192829367774911663872416" rel="stylesheet" type="text/css" media="all" /></noscript>
<noscript><link href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-cart-notification.css?v=183358051719344305851663872409" rel="stylesheet" type="text/css" media="all" /></noscript>
<noscript><link href="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/component-cart-items.css?v=23917223812499722491663872408" rel="stylesheet" type="text/css" media="all" /></noscript>

<style>
  header-drawer {
    justify-self: start;
    margin-left: -1.2rem;
  }

  .header__heading-logo {
    max-width: 90px;
  }

  @media screen and (min-width: 990px) {
    header-drawer {
      display: none;
    }
  }

  .menu-drawer-container {
    display: flex;
  }

  .list-menu {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .list-menu--inline {
    display: inline-flex;
    flex-wrap: wrap;
  }

  summary.list-menu__item {
    padding-right: 2.7rem;
  }

  .list-menu__item {
    display: flex;
    align-items: center;
    line-height: calc(1 + 0.3 / var(--font-body-scale));
  }

  .list-menu__item--link {
    text-decoration: none;
    padding-bottom: 1rem;
    padding-top: 1rem;
    line-height: calc(1 + 0.8 / var(--font-body-scale));
  }

  @media screen and (min-width: 750px) {
    .list-menu__item--link {
      padding-bottom: 0.5rem;
      padding-top: 0.5rem;
    }
  }
</style><style data-shopify>.header {
    padding-top: 10px;
    padding-bottom: 10px;
  }

  .section-header {
    margin-bottom: 0px;
  }

  @media screen and (min-width: 750px) {
    .section-header {
      margin-bottom: 0px;
    }
  }

  @media screen and (min-width: 990px) {
    .header {
      padding-top: 20px;
      padding-bottom: 20px;
    }
  }</style><script src="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/details-disclosure.js?v=153497636716254413831663872415" defer="defer"></script>
<script src="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/details-modal.js?v=4511761896672669691663872416" defer="defer"></script>
<script src="//polinas-potent-potions.myshopify.com/cdn/shop/t/4/assets/cart-notification.js?v=160453272920806432391663872410" defer="defer"></script><svg xmlns="http://www.w3.org/2000/svg" class="hidden">
  <symbol id="icon-search" viewbox="0 0 18 19" fill="none">
    <path fill-rule="evenodd" clip-rule="evenodd" d="M11.03 11.68A5.784 5.784 0 112.85 3.5a5.784 5.784 0 018.18 8.18zm.26 1.12a6.78 6.78 0 11.72-.7l5.4 5.4a.5.5 0 11-.71.7l-5.41-5.4z" fill="currentColor"/>
  </symbol>

  <symbol id="icon-close" class="icon icon-close" fill="none" viewBox="0 0 18 17">
    <path d="M.865 15.978a.5.5 0 00.707.707l7.433-7.431 7.579 7.282a.501.501 0 00.846-.37.5.5 0 00-.153-.351L9.712 8.546l7.417-7.416a.5.5 0 10-.707-.708L8.991 7.853 1.413.573a.5.5 0 10-.693.72l7.563 7.268-7.418 7.417z" fill="currentColor">
  </symbol>
</svg>
<sticky-header class="header-wrapper color-background-1 gradient header-wrapper--border-bottom">
  <header class="header header--middle-left header--mobile-center page-width header--has-menu"><header-drawer data-breakpoint="tablet">
        <details id="Details-menu-drawer-container" class="menu-drawer-container">
          <summary class="header__icon header__icon--menu header__icon--summary link focus-inset" aria-label="Menu">
            <span>
              <svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="presentation" class="icon icon-hamburger" fill="none" viewBox="0 0 18 16">
  <path d="M1 .5a.5.5 0 100 1h15.71a.5.5 0 000-1H1zM.5 8a.5.5 0 01.5-.5h15.71a.5.5 0 010 1H1A.5.5 0 01.5 8zm0 7a.5.5 0 01.5-.5h15.71a.5.5 0 010 1H1a.5.5 0 01-.5-.5z" fill="currentColor">
</svg>

              <svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="presentation" class="icon icon-close" fill="none" viewBox="0 0 18 17">
  <path d="M.865 15.978a.5.5 0 00.707.707l7.433-7.431 7.579 7.282a.501.501 0 00.846-.37.5.5 0 00-.153-.351L9.712 8.546l7.417-7.416a.5.5 0 10-.707-.708L8.991 7.853 1.413.573a.5.5 0 10-.693.72l7.563 7.268-7.418 7.417z" fill="currentColor">
</svg>

            </span>
          </summary>
          <div id="menu-drawer" class="gradient menu-drawer motion-reduce" tabindex="-1">
            <div class="menu-drawer__inner-container">
              <div class="menu-drawer__navigation-container">
                <nav class="menu-drawer__navigation">
                  <ul class="menu-drawer__menu has-submenu list-menu" role="list"><li><a href="/" class="menu-drawer__menu-item list-menu__item link link--text focus-inset">
                            Home
                          </a></li><li><details id="Details-menu-drawer-menu-item-2">
                            <summary class="menu-drawer__menu-item list-menu__item link link--text focus-inset">
                              Catalog
                              <svg viewBox="0 0 14 10" fill="none" aria-hidden="true" focusable="false" role="presentation" class="icon icon-arrow" xmlns="http://www.w3.org/2000/svg">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M8.537.808a.5.5 0 01.817-.162l4 4a.5.5 0 010 .708l-4 4a.5.5 0 11-.708-.708L11.793 5.5H1a.5.5 0 010-1h10.793L8.646 1.354a.5.5 0 01-.109-.546z" fill="currentColor">
</svg>

                              <svg aria-hidden="true" focusable="false" role="presentation" class="icon icon-caret" viewBox="0 0 10 6">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M9.354.646a.5.5 0 00-.708 0L5 4.293 1.354.646a.5.5 0 00-.708.708l4 4a.5.5 0 00.708 0l4-4a.5.5 0 000-.708z" fill="currentColor">
</svg>

                            </summary>
                            <div id="link-catalog" class="menu-drawer__submenu has-submenu gradient motion-reduce" tabindex="-1">
                              <div class="menu-drawer__inner-submenu">
                                <button class="menu-drawer__close-button link link--text focus-inset" aria-expanded="true">
                                  <svg viewBox="0 0 14 10" fill="none" aria-hidden="true" focusable="false" role="presentation" class="icon icon-arrow" xmlns="http://www.w3.org/2000/svg">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M8.537.808a.5.5 0 01.817-.162l4 4a.5.5 0 010 .708l-4 4a.5.5 0 11-.708-.708L11.793 5.5H1a.5.5 0 010-1h10.793L8.646 1.354a.5.5 0 01-.109-.546z" fill="currentColor">
</svg>

                                  Catalog
                                </button>
                                <ul class="menu-drawer__menu list-menu" role="list" tabindex="-1"><li><a href="/collections/potions" class="menu-drawer__menu-item link link--text list-menu__item focus-inset">
                                          Potions
                                        </a></li><li><a href="/collections/ingredients" class="menu-drawer__menu-item link link--text list-menu__item focus-inset">
                                          Ingredients
                                        </a></li></ul>
                              </div>
                            </div>
                          </details></li><li><a href="/pages/contact" class="menu-drawer__menu-item list-menu__item link link--text focus-inset">
                            Contact
                          </a></li></ul>
                </nav>
                <div class="menu-drawer__utility-links"><a href="/account" class="menu-drawer__account link focus-inset h5">
                      <svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="presentation" class="icon icon-account" fill="none" viewBox="0 0 18 19">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M6 4.5a3 3 0 116 0 3 3 0 01-6 0zm3-4a4 4 0 100 8 4 4 0 000-8zm5.58 12.15c1.12.82 1.83 2.24 1.91 4.85H1.51c.08-2.6.79-4.03 1.9-4.85C4.66 11.75 6.5 11.5 9 11.5s4.35.26 5.58 1.15zM9 10.5c-2.5 0-4.65.24-6.17 1.35C1.27 12.98.5 14.93.5 18v.5h17V18c0-3.07-.77-5.02-2.33-6.15-1.52-1.1-3.67-1.35-6.17-1.35z" fill="currentColor">
</svg>

Account</a><ul class="list list-social list-unstyled" role="list"></ul>
                </div>
              </div>
            </div>
          </div>
        </details>
      </header-drawer><h1 class="header__heading"><a href="/" class="header__heading-link link link--text focus-inset"><span class="h2">Polina&#39;s Potent Potions</span></a></h1><nav class="header__inline-menu">
          <ul class="list-menu list-menu--inline" role="list"><li><a href="/" class="header__menu-item list-menu__item link link--text focus-inset">
                    <span>Home</span>
                  </a></li><li><header-menu>
                    <details id="Details-HeaderMenu-2">
                      <summary class="header__menu-item list-menu__item link focus-inset">
                        <span>Catalog</span>
                        <svg aria-hidden="true" focusable="false" role="presentation" class="icon icon-caret" viewBox="0 0 10 6">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M9.354.646a.5.5 0 00-.708 0L5 4.293 1.354.646a.5.5 0 00-.708.708l4 4a.5.5 0 00.708 0l4-4a.5.5 0 000-.708z" fill="currentColor">
</svg>

                      </summary>
                      <ul id="HeaderMenu-MenuList-2" class="header__submenu list-menu list-menu--disclosure gradient caption-large motion-reduce global-settings-popup" role="list" tabindex="-1"><li><a href="/collections/potions" class="header__menu-item list-menu__item link link--text focus-inset caption-large">
                                Potions
                              </a></li><li><a href="/collections/ingredients" class="header__menu-item list-menu__item link link--text focus-inset caption-large">
                                Ingredients
                              </a></li></ul>
                    </details>
                  </header-menu></li><li><a href="/pages/contact" class="header__menu-item list-menu__item link link--text focus-inset">
                    <span>Contact</span>
                  </a></li></ul>
        </nav><div class="header__icons">
      <details-modal class="header__search">
        <details>
          <summary class="header__icon header__icon--search header__icon--summary link focus-inset modal__toggle" aria-haspopup="dialog" aria-label="Search">
            <span>
              <svg class="modal__toggle-open icon icon-search" aria-hidden="true" focusable="false" role="presentation">
                <use href="#icon-search">
              </svg>
              <svg class="modal__toggle-close icon icon-close" aria-hidden="true" focusable="false" role="presentation">
                <use href="#icon-close">
              </svg>
            </span>
          </summary>
          <div class="search-modal modal__content gradient" role="dialog" aria-modal="true" aria-label="Search">
            <div class="modal-overlay"></div>
            <div class="search-modal__content search-modal__content-bottom" tabindex="-1"><predictive-search class="search-modal__form" data-loading-text="Loading..."><form action="/search" method="get" role="search" class="search search-modal__form">
                  <div class="field">
                    <input class="search__input field__input"
                      id="Search-In-Modal"
                      type="search"
                      name="q"
                      value=""
                      placeholder="Search"role="combobox"
                        aria-expanded="false"
                        aria-owns="predictive-search-results-list"
                        aria-controls="predictive-search-results-list"
                        aria-haspopup="listbox"
                        aria-autocomplete="list"
                        autocorrect="off"
                        autocomplete="off"
                        autocapitalize="off"
                        spellcheck="false">
                    <label class="field__label" for="Search-In-Modal">Search</label>
                    <input type="hidden" name="options[prefix]" value="last">
                    <button class="search__button field__button" aria-label="Search">
                      <svg class="icon icon-search" aria-hidden="true" focusable="false" role="presentation">
                        <use href="#icon-search">
                      </svg>
                    </button>
                  </div><div class="predictive-search predictive-search--header" tabindex="-1" data-predictive-search>
                      <div class="predictive-search__loading-state">
                        <svg aria-hidden="true" focusable="false" role="presentation" class="spinner" viewBox="0 0 66 66" xmlns="http://www.w3.org/2000/svg">
                          <circle class="path" fill="none" stroke-width="6" cx="33" cy="33" r="30"></circle>
                        </svg>
                      </div>
                    </div>

                    <span class="predictive-search-status visually-hidden" role="status" aria-hidden="true"></span></form></predictive-search><button type="button" class="search-modal__close-button modal__close-button link link--text focus-inset" aria-label="Close">
                <svg class="icon icon-close" aria-hidden="true" focusable="false" role="presentation">
                  <use href="#icon-close">
                </svg>
              </button>
            </div>
          </div>
        </details>
      </details-modal><a href="/account" class="header__icon header__icon--account link focus-inset small-hide">
          <svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="presentation" class="icon icon-account" fill="none" viewBox="0 0 18 19">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M6 4.5a3 3 0 116 0 3 3 0 01-6 0zm3-4a4 4 0 100 8 4 4 0 000-8zm5.58 12.15c1.12.82 1.83 2.24 1.91 4.85H1.51c.08-2.6.79-4.03 1.9-4.85C4.66 11.75 6.5 11.5 9 11.5s4.35.26 5.58 1.15zM9 10.5c-2.5 0-4.65.24-6.17 1.35C1.27 12.98.5 14.93.5 18v.5h17V18c0-3.07-.77-5.02-2.33-6.15-1.52-1.1-3.67-1.35-6.17-1.35z" fill="currentColor">
</svg>

          <span class="visually-hidden">Account</span>
        </a><a href="/cart" class="header__icon header__icon--cart link focus-inset" id="cart-icon-bubble"><svg class="icon icon-cart" aria-hidden="true" focusable="false" role="presentation" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 40 40" fill="none">
  <path fill="currentColor" fill-rule="evenodd" d="M20.5 6.5a4.75 4.75 0 00-4.75 4.75v.56h-3.16l-.77 11.6a5 5 0 004.99 5.34h7.38a5 5 0 004.99-5.33l-.77-11.6h-3.16v-.57A4.75 4.75 0 0020.5 6.5zm3.75 5.31v-.56a3.75 3.75 0 10-7.5 0v.56h7.5zm-7.5 1h7.5v.56a3.75 3.75 0 11-7.5 0v-.56zm-1 0v.56a4.75 4.75 0 109.5 0v-.56h2.22l.71 10.67a4 4 0 01-3.99 4.27h-7.38a4 4 0 01-4-4.27l.72-10.67h2.22z"/>
</svg>
<span class="visually-hidden">Cart</span><div class="cart-count-bubble"><span aria-hidden="true">2</span><span class="visually-hidden">2 items</span>
          </div></a>
    </div>
  </header>
</sticky-header>

<cart-notification>
  <div class="cart-notification-wrapper page-width">
    <div id="cart-notification" class="cart-notification focus-inset color-background-1 gradient" aria-modal="true" aria-label="Item added to your cart" role="dialog" tabindex="-1">
      <div class="cart-notification__header">
        <h2 class="cart-notification__heading caption-large text-body"><svg class="icon icon-checkmark color-foreground-text" aria-hidden="true" focusable="false" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 12 9" fill="none">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M11.35.643a.5.5 0 01.006.707l-6.77 6.886a.5.5 0 01-.719-.006L.638 4.845a.5.5 0 11.724-.69l2.872 3.011 6.41-6.517a.5.5 0 01.707-.006h-.001z" fill="currentColor"/>
</svg>
Item added to your cart</h2>
        <button type="button" class="cart-notification__close modal__close-button link link--text focus-inset" aria-label="Close">
          <svg class="icon icon-close" aria-hidden="true" focusable="false"><use href="#icon-close"></svg>
        </button>
      </div>
      <div id="cart-notification-product" class="cart-notification-product"></div>
      <div class="cart-notification__links">
        <a href="/cart" id="cart-notification-button" class="button button--secondary button--full-width"></a>
        <form action="/cart" method="post" id="cart-notification-form">
          <button class="button button--primary button--full-width" name="checkout">Check out</button>
        </form>
        <button type="button" class="link button-label">Continue shopping</button>
      </div>
    </div>
  </div>
</cart-notification>
<style data-shopify>
  .cart-notification {
     display: none;
  }
</style>


<script type="application/ld+json">
  {
    "@context": "http://schema.org",
    "@type": "Organization",
    "name": "Polina\u0026#39;s Potent Potions",
    
    "sameAs": [
      "",
      "",
      "",
      "",
      "",
      "",
      "",
      "",
      ""
    ],
    "url": "https:\/\/polinas-potent-potions.myshopify.com"
  }
</script>
  <script type="application/ld+json">
    {
      "@context": "http://schema.org",
      "@type": "WebSite",
      "name": "Polina\u0026#39;s Potent Potions",
      "potentialAction": {
        "@type": "SearchAction",
        "target": "https:\/\/polinas-potent-potions.myshopify.com\/search?q={search_term_string}",
        "query-input": "required name=search_term_string"
      },
      "url": "https:\/\/polinas-potent-potions.myshopify.com"
    }
  </script>
</div>
```

</page>

<page>
---
title: 'Liquid tags: sections'
description: 'Renders a [section group](/themes/architecture/section-groups).'
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/sections'
  md: 'https://shopify.dev/docs/api/liquid/tags/sections.md'
---

# sections

Renders a [section group](https://shopify.dev/themes/architecture/section-groups).

Use this tag to render section groups as part of the theme's [layout](https://shopify.dev/themes/architecture/layouts) content. Place the `sections` tag where you want to render it in the layout.

To learn more about section groups and how to use them in your theme, refer to [Section groups](https://shopify.dev/themes/architecture/section-groups#usage).

## Syntax

```oobleckTag
{% sections 'name' %}
```

### name

The name of the section group file you want to render.

</page>

<page>
---
title: 'Liquid tags: style'
description: Generates an HTML `<style>` tag with an attribute of `data-shopify`.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/style'
  md: 'https://shopify.dev/docs/api/liquid/tags/style.md'
---

# style

Generates an HTML `<style>` tag with an attribute of `data-shopify`.

***

**Note:** If you reference \<a href="/themes/architecture/settings/input-settings#color">color settings\</a> inside \<code>style\</code> tags, then the associated CSS rules will update as the setting is changed in the theme editor, without a page refresh.

***

## Syntax

```oobleckTag
{% style %}
  CSS_rules
{% endstyle %}
```

### CSS\_rules

The desired CSS rules for the `<style>` tag.

##### Code

```liquid
{% style %}
  .h1 {
    color: {{ settings.colors_accent_1 }};
  }
{% endstyle %}
```

##### Data

```json
{
  "settings": {
    "colors_accent_1": "#121212"
  }
}
```

##### Output

```html
<style data-shopify>
  .h1 {
    color: #121212;
  }
</style>
```

</page>

<page>
---
title: 'Liquid tags: stylesheet'
description: >-
  CSS styles included in [section](/storefronts/themes/architecture/sections),
  [block](/storefronts/themes/architecture/blocks), and
  [snippet](/storefronts/themes/architecture/snippets) files.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/stylesheet'
  md: 'https://shopify.dev/docs/api/liquid/tags/stylesheet.md'
---

# stylesheet

CSS styles included in [section](https://shopify.dev/storefronts/themes/architecture/sections), [block](https://shopify.dev/storefronts/themes/architecture/blocks), and [snippet](https://shopify.dev/storefronts/themes/architecture/snippets) files.

Each section, block or snippet can have only one `{% stylesheet %}` tag.

To learn more about how CSS that's defined between the `stylesheet` tags is loaded and run, refer to the documentation for [stylesheet tags](https://shopify.dev/storefronts/themes/best-practices/javascript-and-stylesheet-tags#stylesheet).

***

**Caution:** Liquid isn\&#39;t rendered inside of \<code>{% stylesheet %}\</code> tags. Including Liquid code can cause syntax errors.

***

## Syntax

```oobleckTag
{% stylesheet %}
  css_styles
{% endstylesheet %}
```

### css\_styles

The CSS styles for the section, block or snippet.

</page>

<page>
---
title: 'Liquid tags: tablerow'
description: Generates HTML table rows for every item in an array.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/tablerow'
  md: 'https://shopify.dev/docs/api/liquid/tags/tablerow.md'
---

# tablerow

Generates HTML table rows for every item in an array.

The `tablerow` tag must be wrapped in HTML `<table>` and `</table>` tags.

***

**Tip:** Every \<code>tablerow\</code> loop has an associated \<a href="/docs/api/liquid/objects/tablerowloop">\<code>tablerowloop\</code> object\</a> with information about the loop.

***

## Syntax

```oobleckTag
{% tablerow variable in array %}
  expression
{% endtablerow %}
```

### variable

The current item in the array.

### array

The array to iterate over.

### expression

The expression to render.

##### Code

```liquid
<table>
  {% tablerow product in collection.products %}
    {{ product.title }}
  {% endtablerow %}
</table>
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
<table>
  <tr class="row1">
<td class="col1">
    Draught of Immortality
  </td><td class="col2">
    Glacier ice
  </td><td class="col3">
    Health potion
  </td><td class="col4">
    Invisibility potion
  </td></tr>

</table>
```

## tablerow tag parameters

### cols

## Syntax

```oobleckTag
{% tablerow variable in array cols: number %}
  expression
{% endtablerow %}
```

You can define how many columns the table should have using the `cols` parameter.

##### Code

```liquid
<table>
  {% tablerow product in collection.products cols: 2 %}
    {{ product.title }}
  {% endtablerow %}
</table>
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
<table>
  <tr class="row1">
<td class="col1">
    Draught of Immortality
  </td><td class="col2">
    Glacier ice
  </td></tr>
<tr class="row2"><td class="col1">
    Health potion
  </td><td class="col2">
    Invisibility potion
  </td></tr>

</table>
```

### limit

## Syntax

```oobleckTag
{% tablerow variable in array limit: number %}
  expression
{% endtablerow %}
```

You can limit the number of iterations using the `limit` parameter.

##### Code

```liquid
<table>
  {% tablerow product in collection.products limit: 2 %}
    {{ product.title }}
  {% endtablerow %}
</table>
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
<table>
  <tr class="row1">
<td class="col1">
    Draught of Immortality
  </td><td class="col2">
    Glacier ice
  </td></tr>

</table>
```

### offset

## Syntax

```oobleckTag
{% tablerow variable in array offset: number %}
  expression
{% endtablerow %}
```

You can specify a 1-based index to start iterating at using the `offset` parameter.

##### Code

```liquid
<table>
  {% tablerow product in collection.products offset: 2 %}
    {{ product.title }}
  {% endtablerow %}
</table>
```

##### Data

```json
{
  "collection": {
    "products": [
      {
        "title": "Draught of Immortality"
      },
      {
        "title": "Glacier ice"
      },
      {
        "title": "Health potion"
      },
      {
        "title": "Invisibility potion"
      }
    ]
  }
}
```

##### Output

```html
<table>
  <tr class="row1">
<td class="col1">
    Health potion
  </td><td class="col2">
    Invisibility potion
  </td></tr>

</table>
```

### range

## Syntax

```oobleckTag
{% tablerow variable in (number..number) %}
  expression
{% endtablerow %}
```

Instead of iterating over specific items in an array, you can specify a numeric range to iterate over.

***

**Note:** You can define the range using both literal and variable values.

***

##### Code

```liquid
<table>
  {% tablerow i in (1..3) %}
    {{ i }}
  {% endtablerow %}
</table>

{%- assign lower_limit = 2 -%}
{%- assign upper_limit = 4 -%}

<table>
  {% tablerow i in (lower_limit..upper_limit) %}
    {{ i }}
  {% endtablerow %}
</table>
```

##### Output

```html
<table>
  <tr class="row1">
<td class="col1">
    1
  </td><td class="col2">
    2
  </td><td class="col3">
    3
  </td></tr>

</table><table>
  <tr class="row1">
<td class="col1">
    2
  </td><td class="col2">
    3
  </td><td class="col3">
    4
  </td></tr>

</table>
```

# tablerowloop**object**

Information about a parent [`tablerow` loop](https://shopify.dev/docs/api/liquid/tags/tablerow).

## Properties

* * **col**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current column.

  * **col\_​first**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current column is the first in the row. Returns `false` if not.

  * **col\_​last**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current column is the last in the row. Returns `false` if not.

  * **col0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current column.

  * **first**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current iteration is the first. Returns `false` if not.

  * **index**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current iteration.

  * **index0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current iteration.

  * **last**

    [boolean](https://shopify.dev/docs/api/liquid/basics#boolean)

  * Returns `true` if the current iteration is the last. Returns `false` if not.

  * **length**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The total number of iterations in the loop.

  * **rindex**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of the current iteration, in reverse order.

  * **rindex0**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 0-based index of the current iteration, in reverse order.

  * **row**

    [number](https://shopify.dev/docs/api/liquid/basics#number)

  * The 1-based index of current row.

##### Example

```json
{
  "col": 1,
  "col0": 0,
  "col_first": true,
  "col_last": false,
  "first": true,
  "index": 1,
  "index0": 0,
  "last": false,
  "length": 5,
  "rindex": 5,
  "rindex0": 4,
  "row": 1
}
```

</page>

<page>
---
title: 'Liquid tags: unless'
description: Renders an expression unless a specific condition is `true`.
api_name: liquid
source_url:
  html: 'https://shopify.dev/docs/api/liquid/tags/unless'
  md: 'https://shopify.dev/docs/api/liquid/tags/unless.md'
---

# unless

Renders an expression unless a specific condition is `true`.

***

**Tip:** Similar to the \<a href="/docs/api/liquid/tags/if">\<code>if\</code> tag\</a>, you can use \<code>elsif\</code> to add more conditions to an \<code>unless\</code> tag.

***

## Syntax

```oobleckTag
{% unless condition %}
  expression
{% endunless %}
```

### condition

The condition to evaluate.

### expression

The expression to render unless the condition is met.

##### Code

```liquid
{% unless product.has_only_default_variant %}
  // Variant selection functionality
{% endunless %}
```

##### Data

```json
{
  "product": {
    "has_only_default_variant": false
  }
}
```

##### Output

```html
// Variant selection functionality
```

</page>