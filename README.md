#### For Laravel 5 and below, use the [Original Repo](https://github.com/JeroenNoten/Laravel-Prerender)

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/2d14c369b0844890afe990d50bd80975)](https://app.codacy.com/manual/stanleyloren/laravel-prerender?utm_source=github.com&utm_medium=referral&utm_content=stannlee/laravel-prerender&utm_campaign=Badge_Grade_Dashboard)

=========================== 

Google, Facebook, Twitter, Yahoo, and Bing are constantly trying to view your website... but they don't execute javascript. That's why Prerender was built. Prerender is perfect for AngularJS SEO, BackboneJS SEO, EmberJS SEO, and any other javascript framework.

This middleware intercepts requests to your Laravel website or application from crawlers, and then makes a call to the (external) Prerender Service to get the static HTML instead of the javascript for that page.
## Installation

Require this package run: `composer require stannlee/laravel-prerender`

The package registers it's service provider

If you want to make use of the prerender.io service, add the following to your `.env` file:

    PRERENDER_TOKEN=yoursecrettoken

If you are using a self-hosted service, add the server address in the `.env` file.

    PRERENDER_URL=http://example.com:port

You can disable the service by adding the following to your `.env` file:

    PRERENDER_ENABLE=false

This may be useful for your local development environment.

## How it works
1. The middleware checks to make sure we should show a prerendered page
	1. The middleware checks if the request is from a crawler (`_escaped_fragment_` or agent string)
	2. The middleware checks to make sure we aren't requesting a resource (js, css, etc...)
	3. (optional) The middleware checks to make sure the url is in the whitelist
	4. (optional) The middleware checks to make sure the url isn't in the blacklist
2. The middleware makes a `GET` request to the [prerender service](https://github.com/prerender/prerender) (phantomjs server) for the page's prerendered HTML
3. Return that HTML to the crawler

# Customization

To customize the whitelist and the blacklist, you first have to publish the configuration file:

    $ php artisan vendor:publish

### Whitelist

Whitelist paths or patterns. You can use asterix syntax.
If a whitelist is supplied, only url's containing a whitelist path will be prerendered.
An empty array means that all URIs will pass this filter.
Note that this is the full request URI, so including starting slash and query parameter string.

```php
// prerender.php:
'whitelist' => [
    '/frontend/*' // only prerender pages starting with '/frontend/'
],
```

### Blacklist

Blacklist paths to exclude. You can use asterix syntax.
If a blacklist is supplied, all url's will be prerendered except ones containing a blacklist path.
By default, a set of asset extentions are included (this is actually only necessary when you dynamically provide assets via routes).
Note that this is the full request URI, so including starting slash and query parameter string.

```php
// prerender.php:
'blacklist' => [
    '/api/*' // do not prerender pages starting with '/api/'
],
```

# Credits to the Orginal Creator [Jeroen Noten](https://github.com/JeroenNoten). 
