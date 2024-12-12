# Twitter Spaces - Checkout Extensions and Functions

Date: 2024-11-21

Recording: [Twitter Spaces - Checkout Extensions and Functions](https://x.com/ShopifyDevs/status/1859642510244708596)

## Hosts

- [Shopify Devs](https://twitter.com/ShopifyDevs)
- [Nick Wesselman](https://x.com/nick_wesselman)
- [Gil Greenberg](https://x.com/gilgNYC)
- [David Cameron](https://x.com/dave_cameron)

## Notes

## Recent Updates & Improvements

- Increased limit from 5 to 25 for automatic discount functions
- 40% performance improvement for JavaScript functions through improved JSON parsing
- New logging capabilities and error tracking in development flows
- Dynamic scaling of input limits for carts over 200 items

## Q&A Highlights

### What are the current function limits per app/store?

- Up to 100 functions can be associated with a particular app
- Previously 5, now 25 automatic discounts can be powered by functions
- Payment and delivery customization currently limited to 5 active customizations
- Limits vary by API type, check documentation for specific limits

### How can we take advantage of the new JavaScript optimizations?

- Update to latest version of Shopify CLI
- Update NPM package to version 1.0.2
- No changes needed to existing code
- Deploy app after making updates
- Approximately 40% improvement in performance

### Can gift cards be hidden in checkout now?

- Yes, treat gift cards like any other payment method
- Use the unstable API or wait for 2025-01 stable release
- Return gift cards payment method name in the hide operation
- Functions similarly to hiding other payment methods

### What's the status of functions working across channels (POS, etc.)?

- Working on unifying experiences across platforms
- Different contexts (online vs in-person) require different approaches
- Need to consider staff member control levels
- Planning to add more control over where functions execute
- No specific timelines shared

### How does scaling work for larger carts?

- Dynamic scaling triggers beyond 200 line items
- Input size limits being evaluated for potential adjustments
- Platform regularly tested through flash sales
- Confident in BFCM readiness

## Technical Discussions

### JavaScript vs Rust Functions

- JavaScript is good for prototyping and simpler logic
- Rust performs better for complex operations and B2B use cases
- AI tools can help with JavaScript to Rust migration

### Function Limitations & Considerations

- Functions must remain deterministic (no random numbers or direct time checks)
- Input size limits may need tweaking
- Working on improving date/time handling capabilities

## Future Development Areas

- Considering carrier service function capabilities
- Looking into segmentation features for functions
- Improving function filtering and logging capabilities
- Working on expanding payment and delivery customization limits

## Developer Experience

- Improved error capturing and development flows
- Better documentation and stable APIs
- Need to update to latest CLI version for newest improvements
- Growing focus on cross-channel compatibility