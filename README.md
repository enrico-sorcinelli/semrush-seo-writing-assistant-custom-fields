# SEO Writing Assistant SEMrush Custom Fields

The [SEMrush SEO Writing Assistant](https://wordpress.org/plugins/semrush-seo-writing-assistant/) plugin read only from post `title` and post `content` elements for the real time check.

This lightweight plugin allows you to use an arbitrary value for that check in addition to those values.

So, if you are using some kind of a page builders, field managers and so on, you have only to define a constant in your 
_wp_config.php_ file and eventually to write a little bit of JavaScript code in order to specify which fields.

Note that the plugin don't replace _SEMrush SEO Writing Assistant_ but is intended to be used together.

# Installation  

This section describes how to install the plugin and get it working.

1. Upload the plugin files to the `/wp-content/plugins/seo-writing-assistant-semrush-custom-fields` directory, or install the plugin through the WordPress _Plugins_ screen directly.
1. Activate the plugin through the _Plugins_ screen in WordPress.

# Usage

Once the plugin is activated you can configure it by defining following constants in your _wp-config.php_:

* `SWA_SEMRUSH_CUSTOM_FIELDS_PLUGIN_AUTOENABLE`

Automatically enable use of additional text value for SEMrush check. The default value is `false`.
This is the only configuration you have to do in order to have the plugin feature working.
For example:

```php
define( 'SWA_SEMRUSH_CUSTOM_FIELDS_PLUGIN_AUTOENABLE', true );
```
* `SWA_SEMRUSH_CUSTOM_FIELDS_PLUGIN_INTERVAL`

Allows to change the interval between automatic updates of the text used by SEMrush checks.
The default value is `5` seconds.For example:

```php
define( 'SWA_SEMRUSH_CUSTOM_FIELDS_PLUGIN_INTERVAL', 10 );
```
You can alernatavely control the plugins programmatically, by using 
`semrush_seo_writing_assistant_post_types` (optional) filter and `SeoWritingAssistantSEMrushCustomFields` JavaScript object (see below).

# API

## WordPress Hooks

### `swa_semrush_custom_fields_settings`

Filters plugin settings values.

```php
apply_filters( 'swa_semrush_custom_fields_settings', array $settings )
```

### `semrush_seo_writing_assistant_post_types`

Filters post types where to enable plugin features. Default to `array( 'post', 'page', 'product' )`.

```php
apply_filters( 'semrush_seo_writing_assistant_post_types', array $post_types )
```

The filter has the same name as the one used by _SEMrush SEO Writing Assistant_ plugin (since version 1.0.4) used for the same purpose.

## JavaScript Event

### `seo-writing-assistant-semrush`

This event allows you to update the text for SEMrush analysis.
The following example will update text for SEMrush check every 5 seconds, using `excerpt` and `my_custom_fields` custom field values (working both with block and classic editors):

```javascript
jQuery( document ).ready( function() {
	var swa = new SeoWritingAssistantSEMrushCustomFields( { interval: 5 } );
	jQuery( document ).on( 'seo-writing-assistant-semrush', function( event, data ) {
		data.html += jQuery( '#excerpt' ).val() + "\n"
			+ jQuery( '.editor-post-excerpt__textarea textarea').val() + "\n"
			+ jQuery( '#my_custom_fields' ).val();
	} )
} );
```
Note that if you have previously defined constant `SWA_SEMRUSH_CUSTOM_FIELDS_PLUGIN_AUTOENABLE` to `true`, 
you don't have to create new `SeoWritingAssistantSEMrushCustomFields` object.

## JavaScript API

### `setHtml()`

The following example will update programmatically the text used by SEMrush check with an arbitrary value:

```javascript
jQuery( document ).ready( function() {
	var swa = new SeoWritingAssistantSEMrushCustomFields( { interval: 0 } );
	swa.setHtml( 'foo baz bar' );
} );
```
## Advanced Custom Fields

The plugin checks if _Advanced Custom Fields_ is active in your WordPress instance and adds a custom setting to each field 
(only for `input` within _text_, _radio_ and _checkbox_ type, `textarea` and `select` elements) allowing you to add his value 
to SEMrush check without having to write any additional JavaScript code.

## Autodiscovery

The plugin also automatically add to text used by SEMrush checks, the values of all HTML elements
with `swa-scf` class or `data-swa-scf` attribute.
This will applied only for `input` (within _text_, _radio_ and _checkbox_ type), `textarea`, and `select` elements.

# Screenshots 

## Advanced Custom Fields settings

The Advanced Custom Fields custom setting.

![Advanced Custom Fields custom setting](https://raw.githubusercontent.com/enrico-sorcinelli/seo-writing-assistant-semrush-custom-fields/master/assets-wp/screenshot-1.png)

# Frequently Asked Questions

## Does it work with Gutenberg / Block editor?

Yes.

# License: GPLv2

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.