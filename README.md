# Daisycon Conversion Tag Server-side (Synergy/Hybrid)

This is Daisycon’s official server-side conversion tag template for Google Tag Manager (GTM). It runs in a server container and can work with the client-side Synergy Tag to support hybrid tracking.

Together, these tags improve the accuracy of affiliate reporting when browser limitations would otherwise block tracking data.

## What it does

The tag sends conversion details directly from your server to Daisycon. It supports both basket-level and product-level conversion tracking.

* **Tracks conversions reliably**: Sends campaign and transaction data from your server to Daisycon.
* **Supports hybrid tracking**: Connects server-side conversions to browser-side clicks through the Synergy Tag.
* **Supports product-level tracking**: Builds Daisycon product lines from GA4 ecommerce data, UA Enhanced Ecommerce data, or a Daisycon product array.
* **Handles incomplete product data**: Either fails the tag or sends a basket-level fallback conversion, depending on the selected fallback behavior.

## Tracking modes

### Standard transaction tracking

Sends one basket-level conversion using the transaction values configured in the tag.

### Product-level tracking

Product-level tracking supports three data sources:

* **GA4 ecommerce data**: Reads products from `ecommerce.items` or `items` in the server-side event data.
* **UA Enhanced Ecommerce data**: Reads products from `ecommerce.purchase.products` or `products`.
* **Daisycon product array**: Reads a GTM variable containing product objects with the supported keys `a`, `r`, `qty`, `sku`, `cc`, `pn`, `iv`, and `e1` through `e5`.

For GA4 and UA data, the tag reads the transaction ID, currency, and coupon from event data when available and falls back to the corresponding configured tag values.

Each valid product is sent as a Daisycon `p[]` entry. A product-level conversion requires a campaign ID, transaction ID, and at least one valid product.

### GA4 price and tax

For GA4 products, `items[].price` is treated as the unit product price and multiplied by `quantity`. Supply the price excluding tax. GA4’s transaction-level `tax` field is not allocated across products or used to calculate Daisycon product amounts.

If you need explicit control over the Daisycon values, you can add `daisycon_a` or `a` for the product amount and `daisycon_r` or `r` for the product revenue. These values take precedence over the calculated `price × quantity` amount.

### Product commission codes

Commission codes can come from each product’s standard category or commission-code field, from another product property selected in the tag, or from the tag-level commission code for every product. The tag-level value is also used as a fallback when a product-level value is empty.

### Fallback behavior

When product-level tracking cannot build any valid products, choose whether the tag should:

* fail without sending a conversion; or
* send a standard basket-level conversion instead.

Fallback conversions include a marker in the advertiser description so they can be identified downstream.

## Setup and configuration

Use these guides to configure the server-side tag and connect it to the client-side Synergy Tag:

* [How to set up the Daisycon Server-side Conversion Tag](https://faq-advertiser.daisycon.com/hc/en-us/articles/7046670401820)
* [How to set up Hybrid Tracking with the Daisycon Synergy Tag](https://faq-advertiser.daisycon.com/hc/en-us/articles/20622723791772)
