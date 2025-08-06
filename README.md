# Shopify Web-Components intergration oppo.com

## Case - increase conversion

The OPPO E-Store in the Benelux runs on Shopify. As this is not directly intergrated with the oppo.com website, each extra click we lower the amount of conversion we do for customers.

To increase the conversion and have a more unified experience for the customer, we want to embed part of our store on the oppo.com/nl, oppo.com/benl, oppo.com/befr and oppo.com/lu page.

With the recent Shopify Web Component strucuture this will be more simple than ever.

### How to implement

1. **Module import**: Add the web-components module via the script tag. _This is a global tag on the /nl etc. subdirectory._

```html
<script type="module" src="https://cdn.shopify.com/storefront/web-components.js"></script>
```

2. **Market context**:Add token and market context, where country and language might chance based on the oppo.com subdirectory. _This is also a gobal tag based on the subdirectory. Make sure the `country=""` includes the proper country code and the `language=""` the correct language where the country needs to be capitalized and the language needs to be lower case or camel case like `nl-BE`_

```html
<shopify-store store-domain="oppo-nederland.store" country="NL" language="nl"></shopify-store>
```

3. **Cart implementation**: Add the cart icon in the header like oppo.com/cn but link the button to the 'open cart' button.
   The button to open the cart will be:

```html
<button class="open-cart-button" onclick="getElementById('cart').showModal();">Open cart</button>
```

3.1 **Cart recommendations**: We will create a couple of collections which we can use for cart recommendations for certain products. The cart modal looks like this:

```html
<shopify-cart id="cart">
    <!--Translation overrides for each country-->
    <div slot="header"><span class="your_custom_class_to_target_the_header">Winkelwagen</span></div>
    <!-- Override the empty state with translated text -->
    <div slot="empty">Je winkelwagen is leeg</div>
    <!-- Override the checkout button with translated text -->
    <div slot="checkout-button">Naar de winkelwagen</div>
    <!-- Override the apply discount button with translated text -->
    <div slot="apply-discount-button">Korting toevoegen</div>
    <!-- Override the discounts section title with translated text -->
    <div slot="discounts-title">Korting</div>

    <!-- Override the discount apply error message with translated text -->
    <div slot="discount-apply-error">Deze kortingscode kan niet worden toegevoegd.</div>
    <!-- The extension is appended to the cart. -->
    <div slot="extension" class="cart-extension">
        <p class="cart-extension__title">Dit vind je misschien ook wel leuk</p>
        <shopify-context type="collection" handle="korting-accessoires">
            <template>
                <div class="cart-extension__products">
                    <!-- The collection is iterated over and the products are displayed. -->
                    <shopify-list-context type="product" query="collection.products" first="3">
                        <template>
                            <button
                                class="cart-extension__product"
                                onclick="document.querySelector('shopify-cart').addLine(event);"
                                shopify-attr--disabled="!product.selectedOrFirstAvailableVariant.availableForSale"
                            >
                                <shopify-media width="135" height="135" query="product.featuredImage"></shopify-media>
                                <p><span>+</span></p>
                            </button>
                        </template>
                    </shopify-list-context>
                </div>
            </template>
        </shopify-context>
    </div>
</shopify-cart>
```

Within this cart we can add our own styling to match the OPPO.com website. The full documentation is on the [Shopify Web Components Documentation for cart](https://shopify.dev/docs/api/storefront-web-components/components/shopify-cart).

We will be able to provide proper translations for all components and will, once approved also provide collections for upsells.

4. **Add to cart & buy now button**: As the cart is a global item a user will be able to browse the website and add multiple things to their cart. For this to happen we need to add a _add to cart_ button. On top of that we want to be able to give the user the oportunity to buy directly without adding anything to the cart. This can be done via the following code:

```html
<shopify-context type="product" handle="oppo-a5x">
    <template>
        <button
            class="product-modal__buy-button"
            onclick="document.querySelector('shopify-store').buyNow(event)"
            shopify-attr--disabled="!product.selectedOrFirstAvailableVariant.availableForSale"
        >
            Koop nu
        </button>
        <button
            class="product-modal__add-button"
            onclick="getElementById('cart').addLine(event).showModal();"
            shopify-attr--disabled="!product.selectedOrFirstAvailableVariant.availableForSale"
        >
            Toevoegen aan winkelwagen
        </button>
    </template>
</shopify-context>
```


The `intergrations.html` file has a small overiew of the elements that are needed to create this functionallity.

If you have any questions, feel free to contact me at <thom.knepper@oppo-aed.nl>
