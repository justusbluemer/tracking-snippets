# Consent parsing for the "GDPR Legal Cookie" Shopify Plugin

The "GDPR Legal Cookie" Shopify Plugin is designed to enable consent or denial thereof for every individual cookie.
With tag management though, consent is usually handled on a per-3rd-party tool. For example, you'd either handle consent for "Facebook" rather than for `_fbp` and `_fbc` cookies separately.

The following code abstracts this complexity and produces an easy to handle object that provides a boolean (`true` or `false`) value for every "provider" found in the plugin's window.GDPR_LC object.

## Example result

```javascript
{
    "shopify": true,
    "cloudflare": true,
    "gdpr_legal_cookie": true,
    "google_analytics": true,
    "facebook": true,
    "criteo": true
}
```

The code below is intended to be used as a Custom JavaScript Variable in Google Tag Manager, but can likely be used in other tag management systems as well.

```javascript
/**
 * Converts the consent status from the "GDPR Legal Cookie" Shopify
 * plugin into an easy-to-handle format for Google Tag Manager and other
 * downstream services.
 * The plugin is centered around individual cookies, not providers or
 * 3rd party tools which makes it difficult to use the consent status to
 * block or execute code with just the info in window.GDPR_LC
 */
function (){
    var providers = {};
    var cookie;

    if (
        window.GDPR_LC &&
        window.GDPR_LC.Cookies &&
        window.GDPR_LC.Cookies.list
    ) {
        // Iterate all cookies
        for (cookieName in window.GDPR_LC.Cookies.list) {
            cookie = window.GDPR_LC.Cookies.list[cookieName];
            // Consolidate the "providers" of all cookies into a single object
            // A provider will by considered consented-to if all its cookies are
            // accepted (true value). Even if only one of its cookies are not accepted, the
            // provider is considered denied (false value).
            if (cookie.provider && cookie.provider != "") {
                if (typeof providers[cookie.provider] === "undefined") {
                    providers[cookie.provider] = cookie.userSetting || false;
                }
            }
        }

        // Convert providers object key to lowercase and all non alphanumerical characters into underscores
        var providerKeys = Object.keys(providers);
        for (var i = 0; i < providerKeys.length; i++) {
            var key = providerKeys[i];
            providers[key.toLowerCase().replace(/[^a-z0-9]/g, "_")] = providers[key];
            delete providers[key];
        }
    }

    return providers;
};
```
