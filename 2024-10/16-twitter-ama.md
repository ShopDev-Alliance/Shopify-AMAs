# Twitter Spaces - Liquid Devs AMA

Date: 2024-10-16

Recording: [Twitter Spaces - Liquid Devs AMA](https://x.com/shopifydevs/status/1844408172007784701?s=46)

## Hosts

- [Liquid Devs](https://twitter.com/liquid_devs)
- [Ben Sehl](https://twitter.com/benjaminsehl)
- [Max Stoiber](https://twitter.com/mxstbr)
- [Clauderic Demers](https://twitter.com/clauderic_d)

## Notes

### Developer tooling

Goals are incorporating nice things like vite would be the goal and React-like things. It's not Javascript runtime. Slate and other attempts to bring in tooling to Javascript library level tooling is something they want to move towards.

There aren't specific answers on how to move to more of a Javascript friendly way to approach handling Liquid.

### AI Assisant for Liquid Specifically?

_Background - .dev assistant in docs is pretty helpful for pulling GraphQL queries_

Not something that has been worked on or previously considered. Certainly something that could be looked at or considered. Team has been using Cursor quite a bit, along with some Copilot as well.

### Plans to improve reactivity around Liquid?

Anything built will have to be optional and/or opt-in due to concerns for legacy builds. Any instance where there has to be a `useEffect` equivalent we've lost.

Good example would be how [Section Rendering API](https://shopify.dev/docs/api/ajax/section-rendering) works at the moment and making this something that could be accessible directly in Liquid. This would be a good example of something that could be improved over the existing snagging and replacing everything with Javascript.

[HTMX](https://htmx.org/) and [Hotwire](https://hotwired.dev/) can all be really good examples of how this could be done to draw inspiration.

Everything will be coming in the form of RFCs (request for comments) initially. Nothing will be rolled out without discussion ahead of time to make sure it's properly meeting use-cases.

#### Additional notes

Rich Harris' talk referenced [Rethinking Reactivity](https://www.youtube.com/watch?v=AdNJ3fydeao) maybe?

SSR (server side rendering) is super important for ecommerce. Yes, use Javascript to enhance the page but make sure the page still works without Javascript is an important learning from working on Hydrogen.

### Future of Liquid API

Taking a look at arrays specifically. A more powerful set of array filters is somethign that's asked regularly. Lookikng at how if statements and expressions are done are what the team is looking at initially to start making improvements.

Considering the line between a _programming language_ and a _templating language_. It's a fine line, but one the team has at the forefront.

Liquid is the entrance for many merchants to start building their storefronts. Liquid will always be the main entrance point.

### Liquid and integrating with apps

The connection between themes and apps has been difficult to manage. The web pixel api could be a potential avenue here.

Standards are the way to do this. [CSS variables](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) are huge opportunity from a stylistic standpoint. Standard event names, like with the [web pixels API](https://shopify.dev/docs/api/web-pixels-api), could be potential considerations as well.

### Currency conversion in Liquid

No current plans or considerations for this. Currency conversion is not something that is currently on the roadmap or surfaceable directly.

The issue right now is really evident with Cart Transform API and using for things like bundles (merchants can set their own price). There currently is not a way to run it through currency rules to get the proper conversion.

Not a known use-case or a way to do this currently.

### Collection filtering limited to 5,000 products

There isn't a good answer here. With the increased variant support, this is something that will require more research.

The team encourages folks to go ahead and reach out directly and folks don't have to wait to catch them on AMAs. Available to chat 1:1 for these sorts of questions to help understand use-cases.