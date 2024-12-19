# Twitter Spaces - Liquid Devs AMA

Date: 2024-12-18

Recording: [Twitter Spaces - Liquid Devs AMA](https://x.com/i/spaces/1mnxeAQQdwQxX)

## Hosts

- [Shopify Devs](https://twitter.com/ShopifyDevs)
- [Ben Sehl](https://x.com/benjaminsehl)
- [Max Stoiber](https://x.com/mxstbr)
- [James Meng](https://x.com/jmengio)

## Notes

### Key Statistics & Specific Updates

- Theme tools: Releasing approximately 4 new improvements per day through November
- VS Code improvements: 582 lines of release notes since last edition
- CLI Performance: Push/pull commands now 5-10x faster depending on bandwidth
- Sitemap API: Now supports catalogs with millions of products (previously limited)

### Notable Q&A

#### Q: What scenarios benefit from instant dev server startup?

A: Benefits daily development flow with:

- Almost instant dev server startup
- Hot module reloading
- More seamless development experience
- Background work offloading

#### Q: How does the new VS Code extension handle errors and improvements?

A: Key features include:

- Automatic snippet renaming across all render calls
- Theme block schema validation
- Metafield auto-completion pulling from store definitions
- Image URL auto-completion
- Content for tag suggestions
- Validation for theme blocks with specific field requirements

#### Q: What's happening with the AJAX API documentation?

A: Current status:

- Documentation needs updating
- Built on older REST API
- Missing newer features (shipping rates updates)
- Team acknowledges need for improvement
- Planning comprehensive documentation overhaul
- Considering reactivity improvements for cart workflows

#### Q: How does Theme Blocks nesting and performance work?

A: Considerations include:

- 8 levels of nesting support
- Focus on reusable blocks across themes
- Recommendation to use presets for merchant ease
- Performance monitoring through upcoming developer tools
- Balance between flexibility and ease of use
- Emphasis on semantic grouping rather than artificial organization (avoiding emoji usage for related items)

#### CLI Authentication

- New browserless console authentication
- Text-prompt based authentication
- Eliminates need for browser-based storefront password entry
- Maintains development flow continuity

#### GraphQL API Updates

- Batching Strategy for Theme Files:

  - Required upload order:
    - Liquid blocks
    - Liquid sections
    - JSON sections
    - JSON templates
    - Contextualized JSON files
  - Config files (settings, data, schema)

#### Legacy Support

- Legacy flag available for older CLI functionality
- Maintaining backward compatibility
- Focus on supporting existing implementations while introducing new features