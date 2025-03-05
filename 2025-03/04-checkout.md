# Twitter Spaces - PCI v4 Compliance & Shopify Checkout

Date: 2025-03-04

Recording: [Twitter Spaces - PCI v4 Compliance & Shopify Checkout](https://x.com/i/spaces/1RDGlzjynYoxL)

## Hosts

- [Ilya Grigorik](https://x.com/igrigorik)
- [Mani](https://x.com/manishtomar)
- [Weston Ruter](https://x.com/WestonRuter)

## Key Topics Covered

### PCI v4 Overview

00:00 - Introduction and host backgrounds
05:30 - What is PCI v4 and anti-skimming provisions
10:15 - Shopify's checkout philosophy: conversion, compliance, and composability
15:45 - Technical implementation of sandboxing for security

### Sandboxing & Security Architecture

20:00 - Remote DOM and isolation techniques
25:30 - First-party vs third-party code separation
30:45 - Data access and scope controls
35:20 - Balancing customization with security requirements

### Extension Development

40:10 - Building UI extensions in a secure environment
45:30 - Developer experience and debugging
50:15 - Q&A on communicating compliance to merchants
55:40 - Cross-channel strategy and compliance

## Detailed Key Takeaways

### 1. PCI v4 Focuses on Comprehensive Script Protection

The new PCI v4 standard significantly expands beyond previous versions by addressing the entire checkout environment, not just the payment fields. Key requirements include:

- **Complete Script Inventory**: Merchants must maintain a comprehensive inventory of all scripts running on checkout pages
- **Business Justification**: Each script must have a documented business or technical justification
- **Authorization Mechanisms**: Systems must be in place to ensure only authorized scripts are loaded
- **Integrity Verification**: Scripts must be protected against unauthorized modifications through integrity checks
- **Anti-Skimming Provisions**: Specifically designed to prevent malicious code from capturing sensitive information
- **Beyond iframe Protection**: Previous standards focused on isolating payment fields in iframes, but v4 recognizes that a compromised parent page remains vulnerable

This represents a significant shift from PCI v3, acknowledging that protecting only the payment fields is insufficient when the entire checkout environment could be compromised.

### 2. Shopify's Architecture Creates Complete Isolation

Shopify's approach to PCI v4 compliance centers on a robust sandboxing architecture:

- **Remote DOM Technology**: An open-source library that allows controlled communication between isolated environments
- **Complete Separation of Concerns**: First-party code (Shopify-controlled) runs in the main thread while third-party code runs in isolated environments
- **Controlled Bridge Communication**: All interactions between sandboxed code and the parent page go through a strictly controlled bridge
- **Web Workers and iframes**: Leverages native browser isolation features to create secure execution environments
- **Runtime Guarantees vs. Retroactive Auditing**: Provides active protection during execution rather than after-the-fact verification
- **CSP and SRI Implementation**: Content Security Policy and Sub-Resource Integrity are managed by Shopify, not individual developers
- **Performance Optimization**: Edge caching and other techniques to minimize the performance impact of these security measures

This architecture creates a fundamental separation that prevents third-party code from directly accessing sensitive areas of the checkout experience.

### 3. Extension Model Enables Secure Customization

Shopify's extension framework provides three primary ways to customize checkout while maintaining security:

- **Functions**: Server-side extensions that customize backend business logic
  - Run on Shopify's servers with strict security controls
  - Allow customization of pricing, discounting, shipping calculations, etc.
  - Maintain full PCI compliance since they never run in the browser

- **UI Extensions**: Client-side extensions that modify the checkout interface
  - Run in isolated sandboxes separate from the main checkout
  - Use Remote DOM to safely project UI elements into the parent page
  - Access controlled data through explicit APIs with permission scopes
  - Leverage Shopify's component library for consistent styling and accessibility

- **Web Pixels**: Extensions for analytics and tracking
  - Run in web workers to prevent performance impact on the main thread
  - Access standardized events through a pub/sub event bus
  - Cannot directly access sensitive DOM elements or user inputs
  - Provide tracking capabilities without compromising security

This model allows for extensive customization while maintaining strict boundaries that prevent security vulnerabilities.

### 4. Developer Experience Remains Straightforward

Despite the complex security architecture, Shopify has worked to maintain a simple developer experience:

- **Familiar Development Patterns**: Developers can write standard React/JavaScript code
- **Security Abstraction**: PCI compliance details are handled by the platform, not individual developers
- **Standard Debugging Tools**: Console logs, breakpoints, and other typical tools work as expected
- **Automatic Inventory and Integrity**: Shopify handles script inventory and integrity verification
- **Permission-Based Data Access**: Clear API boundaries with explicit permission requirements
- **Branding API**: Consistent styling across native and custom components
- **Accessibility Enforcement**: Built-in checks to maintain WCAG compliance

This approach means developers can focus on building functionality without becoming security experts, while merchants gain the assurance that extensions maintain compliance with regulatory requirements.

### 5. Cross-Channel Strategy Ensures Consistent Compliance

Shopify's approach to cross-channel commerce maintains compliance across different platforms:

- **Web Checkout Foundation**: All checkout experiences are powered by the same compliant web foundation
- **Checkout Sheet Kit**: Mobile native experiences use a web-view based approach that inherits compliance
- **Channel Partner Integrations**: Platforms like Roblox use the same secure web checkout with appropriate styling
- **POS Considerations**: Special attention to point-of-sale experiences that have unique requirements
- **Unified Compliance Strategy**: One implementation that powers all sales channels
- **Consistent Buyer Experience**: Maintains brand consistency while ensuring security across channels

This strategy allows Shopify to maintain compliance across an expanding ecosystem of sales channels without duplicating security efforts.

### 6. Comprehensive Compliance Beyond PCI

Shopify's approach addresses multiple compliance requirements simultaneously:

- **Web Accessibility (WCAG)**: Component library enforces accessibility standards
- **Data Privacy (GDPR, CCPA)**: Controlled data access prevents unauthorized collection
- **E-Privacy and Cookie Consent**: Built-in mechanisms for proper consent management
- **Performance Standards**: Ongoing optimization despite security overhead
- **Identity Management**: Integration with identity providers like Shop Pay while maintaining merchant branding
- **Regulatory Evolution**: Architecture designed to adapt to changing requirements

This holistic approach means merchants benefit from a solution that addresses the full spectrum of compliance requirements, not just PCI.

## Resources Mentioned

- Remote DOM (GitHub: Shopify/remote-dom)
- Shopify Developer Blog for more on PCI compliance
- Checkout SDK for native mobile applications

## Full unedited transcript

Hey, folks. 
Welcome to today's panel on PCI v4. 
Now, as people trickle in, I know what you're thinking. 
PCI compliance? 
How did I get lucky enough to attend this party, right? 
But don't worry, my friends. 
I have Manny and Weston with me here today. 
And we'll try to keep things lighter. 
So first things first, let's do a bit of introductions. 
First, a bit about myself. 
I'm Ilya Grigorik. 
I'm a distinguished engineer at Shopify. 
And my work has been our storefront platform, APIs, 
underlying infrastructure, the power of Shopify services, 
and of course, Checkout, where I've 
had the pleasure of working alongside Manny and Weston 
on PCI and many other topics. 
Manny, over to you. 
Hey, everyone. 
Glad to be here with all of you. 
Haven't done one of these in a while. 
We've done a ton of work in the past few years 
in the world of Checkout. 
And I've been leading our Checkout product 
for a few years now. 
I think we're coming up on a little bit past four years. 
And so I have all sorts of teams that cut across this area. 
And Ilya and I have partnered on many topics together. 
And it's one of a few areas of Shopify 
that I'm focused on, including wholesale and B2B tooling, 
our draft orders, and some other stuff that 
is exciting and coming soon. 
So I'm really glad to be here with everyone. 
I have an honest AMA. 
Weston, I'll pass to you. 
Yeah, absolutely. 
Thank you. 
First x-spaces for me. 
I'm Weston. 
I joined Shopify about a year ago. 
And I'm on the Checkout engineering team. 
Most of the past year here for me 
has actually been working on PCI v4, 
understanding the requirements, ensuring that Checkout 
is going to be good to go, is safe and secure as can be. 
It's been exciting in a way, right? 
Compliance is never the most fun thing in the world. 
But it's super important for us, for our merchants, for buyers. 
And yeah, it's been a big focus of mine. 
So happy to chat about PCI and dig 
into some of the details that made it difficult to implement. 
Awesome. 
Thank you both. 
One fun fact about Mani, he likes to make-- 
or he has a great repertoire, I should say, 
of grown-worthy dad jokes that he likes to share internally. 
And I'm going to try my best to share some as we go along, 
just to keep things fun. 
Not dad jokes, but focusing on PCI. 
So I'll start with one. 
Curious. 
What's the PCI auto's favorite dance move? 
The two-factor shuffle. 
[LAUGHTER] 
A two-factor shuffle, God. 
That is grown-worthy indeed. 
Yes, yes. 
That'll be the tone for today. 
All right, so before we get into the actual Q&A, 
we have quite a few questions, good questions, 
that were submitted before the event. 
I want to set the stage a little bit 
and just talk about how we think about PCI before Shopify 
and some of the underlying technology. 
So let's start at the top, which is, what is PCI before? 
And the short answer is, it's many things. 
But for the purpose of our conversation, 
we'll focus on the anti-skimming provisions, which are arguably 
also the most far-reaching and impactful 
as far as implementation goes. 
So the next question then is, what is skimming? 
And skimming is the stealing of sensitive information 
from websites. 
So if you're in checkout, this is typically 
done by placing some malicious code on the checkout page, 
which can then capture the information and exfiltrate it 
somewhere else. 
So this can be achieved through a supply chain attack, 
or maybe a compromised script, or an XSS vulnerability 
on site where malicious code can be injected and then abused 
to exfiltrate this information. 
So then the question is, why now? 
And that will be a good one, because PCI v3 
focused on protecting the sensitive payment 
data, which, as an industry, we solve through iframe 
isolation. 
So those are not familiar with the technical underpinnings 
of that. 
When content is embedded in iframe, all of the inputs-- 
keyboard and mouse-- are obfuscated from the embedder. 
So that allows payment information 
to be isolated from the top frame. 
But it turns out that that's not sufficient, 
because what happens if the parent page is compromised? 
So if you've embedded the secure element, 
what stops the parent page from, let's say, 
replacing it with a compromised version 
or simply interrupting some of the events? 
And then finally, what are the proposed remedies? 
This will be kind of the meat and potatoes 
of our conversation today. 
The PCI v4 specification has a section. 
I think it's 6.43. 
And it outlines a number of requirements. 
For example, that you have to maintain 
an inventory of all scripts, that you 
have to provide a mechanism to authorize 
the scripts that are loaded, and then finally, 
ensure the integrity of the script. 
And all combined, this gives you strong assurances 
about protection provided to the buyer, which, as a consumer, 
I think is fantastic. 
As a developer, these can be quite a headache. 
And we'll get into the solutioning 
of how we tackle such Shopify and why 
it's a challenge for many others as well. 
But before we get into the technical discussion, 
Mani, I'm curious, how does PCI compliance 
fit into the broader landscape of checkout? 
Yeah, so when we think about checkout, 
the idea is that any business that 
wants to run on Shopify, going from, let's say, 
even pre-sales, before they've had 
any idea of what they're going to sell, all the way to IPO, 
and then beyond that to any enterprise scale, 
should be able to do the whole thing on one stack. 
And they should be able to do this not just 
for their online commerce, but also for retail, for wholesale, 
and all aspects of commerce that matter to their unified 
commerce and omnichannel business. 
And then when we were looking at redesigning 
many parts of our checkout about five years ago, 
and then we started building parts that some of you 
on the call might be familiar with about three years ago, 
we came up with this North Star that 
said there are three Cs to checkout-- 
conversion, compliance, and composability. 
So you think about conversion being that simply being 
on Shopify should guarantee that you have higher odds of success 
out of the box, and we'll be the ones who are obsessively 
figuring out how to get every ounce of conversion lift 
and to build the most elegant buyer experience you 
could buy on the internet. 
From a compliance perspective, we 
want to say that the burden of meeting 
whatever regulatory requirements are around the world, 
they can really weigh down small businesses. 
That means that entrepreneurs can't start businesses 
as easily, but it also impacts large businesses 
that have to have entire teams worrying about these problems. 
So Shopify should drastically reduce this overhead, 
and that applies in many areas. 
PCI is one. 
Obviously, we've mentioned a bunch of times here today, 
but you can think of GDPR, CCPA, e-privacy, WCAG, PCI DSS, 
acronym soup, whatever it might be. 
We should be doing heavy lifting here. 
And then that brings us to composability, 
because while we think we have an amazing, the world's best, 
actually, checkout out of the box, 
as your business begins to scale and you become 
more sophisticated and the amount of GMV you're moving 
goes up, usually that's correlated 
with needing to make some set of customizations that 
are bespoke to your business or that are add-ons that maybe 
an ecosystem of developers have been able to standardize 
and bring solutions to market for. 
For example, upsells or loyalty or what have you. 
And you should be able to make those customizations 
without sacrificing either conversion or compliance. 
So you want it to be easy, secure, upgrade safe, 
what have you. 
And altogether, when you take a look at these three Cs 
as being part of that North Star, 
it begins to kind of reveal why we've 
taken certain technical decisions 
over the past several years and the advent of Shopify 
extensions, which led to checkout extensions, 
the advent of functions, the advent of web pixels, 
and all of these things coming together 
to enable getting the conversion without sacrificing 
the compliance while always being composable in checkout. 
So that's a bit of a back story of how we got here, 
and we're going to talk more about it as we go. 
That's really helpful. 
I like the framing of a broader landscape of requirements. 
As you said, it's not just PCI. 
There's an acronym swoop of regulations and regulatory 
bazaar that keeps changing. 
So it's about finding a solution that 
can work across all of these. 
Weston, I'm curious. 
You've been in the trenches of working with many teams 
in checkout and across Shopify to tighten up all the hatches 
and implement the right protections. 
And if you were to sit down with a fellow developer who's 
not thought about PCI before, how would you explain or guide 
them on how to navigate the space? 
Yeah, I think it's a good question. 
And that's actually a question a lot of folks 
had asked when we started the project, even internally. 
Like, what are all the details around PCI before, 
and what changed? 
Because as you mentioned earlier, 
used to just be if your card fields were in an iframe, 
you were pretty good, right? 
You'd restrict that sandbox where they entered credit card 
details, the buyer, and you felt pretty secure. 
One example I actually like to give to start with 
is I've been in the checkout and e-commerce space for a bit now. 
And a lot of folks had told me, well, 
it seems very secure to isolate card details. 
Like, what could happen? 
And one thing I saw on a number of merchant sites 
when I was working elsewhere was this ability 
for malicious actors to open that merchant site in a pop-up. 
So tricking a user to clicking a button 
and opening a merchant site in a pop-up. 
And then they would launch attacks 
to not even just replace the card field. 
Sometimes they would be intelligent 
and see if your post messaging in and out of that iframe 
had any configurations about sort of endpoints 
where you might send fetch requests with card details. 
And these attackers get really creative 
and can really sort of abuse any ability 
to get on the merchant's top level page. 
So PCI is exciting in that we can really restrict 
all of those avenues, replacing card fields, 
communicating with card fields entirely. 
So I always led with that example, 
why wasn't card fields in an iframe sufficient? 
But after that, it gets really tricky 
when you look at the landscape of checkout pages in general. 
A lot of times you have web pixels, 
just tracking users so you can monitor 
what buyers are doing and better understand 
how they're interacting with your page. 
A lot of times you have third-party wallets 
to accept payments outside of credit cards. 
And all of these things represent scripts 
you don't control potentially running on your page 
on top of the need to lock down the scripts you do control. 
So I think Shopify, when I got started, 
was in a really good spot with extensions, for instance. 
I was initially really concerned, 
how would we still have customizability 
if we had to have, know what's running, 
know the contents of every script running. 
And looking at extensions was really a breath of fresh air 
because they were sandboxed. 
They didn't have access to the merchant page. 
So that was a great relief and kind of speaks 
to Mani's North Stars really working in our favor. 
So that was great. 
What we needed to really do then was look at our scripts 
pretty heavily and say, 
how can we develop this robust inventory? 
Because checkout's made up of quite a few different scripts 
and we have a lot of optimizations for how we load those 
and when we load those. 
So we were able to build some new build tooling 
to look at what scripts we could possibly generate. 
And annotate those with why we might load them on the page, 
which was a unique challenge, 
but we were able to sort of solve that 
with our build tooling. 
And then for integrity, that was also quite interesting 
because we make use of import maps pretty heavily 
because we want to optimize how we load everything 
and use the platform as much as possible. 
So we were able to contribute back to browsers 
to add integrity support to import maps 
and start generating those and finding ways 
to apply that across any script we might load. 
So it was really a lot of looking at 
removing third-party scripts from the page entirely, 
such as wallets or any sort of pixels 
that might be left over that weren't already covered 
by our work in extensions and our sandboxing work 
that had already existed. 
And applying that same practice that we were already doing 
was very helpful for things such as wallets. 
Adding inventory, adding integrity 
was pretty critical as well. 
So that took a lot of effort to do on our end. 
And then for just ensuring we knew 
what origins were being loaded, 
really just kind of looking at our CSP policies 
and making sure we were applying best practices there. 
That ended up being a little bit more fluid 
in the grand scheme of things, getting our CSP locked down. 
But certainly something that you don't want to overlook 
on your checkout pages. 
- Yeah, the one thing I'll add, Weston, is that, 
oh, by the way, for those who may not be familiar, 
CSP is Content Security Policy. 
But the one thing I'll add is a very nice side effect 
of the extensions-based model 
for being able to customize the checkout 
and loading all of these third-party scripts 
and customization is that it automatically 
begins to inventory what are all of the apps being loaded, 
what extensions they have, 
what the data access of those extensions is, 
which also helps with understanding and chronicling 
what is the purpose of that app and customization. 
Which, when you look at that requirement 6.4.3 
and this idea that scripts need to be inventoried 
with business or technical justification, 
and that the methods are implemented 
to confirm that all scripts are authorized, 
well, the authorization is installing the app 
and approving it to be placed inside the checkout, 
as well as the fact that Shopify is loading 
a versioned extension, right, as a block inside of checkout. 
And all of these ended up being really good paradigms 
for when the updates on version four of DSS was written. 
And we were pleasantly surprised 
that many decisions we'd made 
about why customization should be delivered by apps, 
the simplicity that brings with install, uninstall, 
traceability, data access, scope rights, 
all these things together, 
it just, it ended up being a really big win. 
- Yeah, it's crazy to think about too, 
when I was reading PCI v4 prior to coming to Shopify, 
I had this concern, having worked on checkout pages, 
I would throw a couple scripts on the page 
to accept payments, 
like I might want to throw a wallet on the page, 
and I would just throw a script on that page. 
And I didn't really put a lot of thought 
into how do I make sure that the script has strict integrity, 
so I know the contents of it at all time, 
'cause so many scripts online are evergreen, 
or how do I handle version changes over time? 
So it is kind of really refreshing 
to see that there's a paradigm that exists internally. 
And kind of compare that to what I was doing 
before coming to Shopify, 
which was collecting different third-party scripts 
effectively to accomplish an end goal on checkout, 
which, as we know, is not the most secure posturing. 
- So you guys did a good job of setting the stage here, 
and it's not often that I hear PCI and exciting 
uttered in the same sentence, 
but it is an exciting space, 
and let's just be clear, 
maybe for those who are not familiar 
with the Shopify platform, 
that everything we're talking about here 
is a solved problem for our merchants. 
So as a merchant on Shopify, using Shopify checkout, 
effectively, you have the right guarantees 
to be PCI before compliant. 
You do not have to put in any additional work. 
So everything we're talking about here 
is the pain and the suffering that we've gone through 
along this journey, 
which was not strictly motivated by PCI, 
but as Mani has outlined, chasing the North Star. 
It is about performance, accessibility, 
providing the landscape or infrastructure 
to provide regulatory compliance, 
and all of the other things. 
And PCI plays into that same architecture. 
So now let's maybe dive into some of the questions, 
'cause I think we'll start unpacking 
some of the statements that you guys were making. 
I'll do my best to moderate. 
We have a number of questions that were submitted up front, 
and we'll also welcome live questions. 
As a quick disclaimer there, 
please let's keep it focused on PCI-related topics 
and keep it brief. 
And if we don't get to your question, 
please post it on Next, 
and we'll do our best to triage and follow up 
after we sign off. 
So with that out of the way, 
let me maybe queue up the first question 
that we had submitted beforehand, 
which is, Shopify's checkout architecture 
relies heavily on sandboxing. 
Could you elaborate on the specific technologies used 
and how they contribute to performance and security? 
Weston, maybe you can take a first crack at this one. 
- Yeah, absolutely. 
So we have a number of things in play 
to get sandboxing right, 
'cause it's not an easy thing to do. 
One of the premier tools in our tool belt, so to speak, 
is a library called Remote DOM 
that we actually have open source. 
And what that library does is it makes it easy 
to run logic in a sandboxed iframe or a web worker 
and communicate to and from it securely 
from the parent page. 
So if we think about the checkout page 
and maybe let's say your card fields components, 
you can easily talk to and from them 
without worrying about leaking too much information 
from inside the secure sandbox 
and also prevent that secure sandbox 
from any code inside of it from escaping the frame 
and executing logic on the top level page. 
So Remote DOM and its various sub packages 
have been huge for implementing 
our sandboxing strategy in PCI. 
Under the hood, that's using the native browser features 
of native web features of iframes, of course, right? 
Iframes are sort of that most obvious solution 
for sandboxing, but also we do make heavy use of web workers 
so we can take logic off of the main thread, 
securely, but also performantly execute that logic. 
And I think those are sort of the three key pieces 
in our equation there. 
- Yeah, I think that's a great way. 
Also, just a little bit of context. 
So Remote DOM is an open source library 
that we've also released. 
So if you go at GitHub under Shopify Remote DOM, 
you can actually find it and use it 
in your own applications. 
And effectively what it allows you to do 
is to take a tree of DOM elements, 
which you can create in the sandbox environment 
as Russell has described, 
and then propagate them into the parent page. 
And the reason this is crucial and important, 
I just want to take a step back here. 
Checkout pages are probably some of the most 
heavily instrumented pages on the internet 
because they are so crucial to the business. 
This is where money changes hands. 
This is where much of the A/B testing happens, 
all the conversion tracking, all of the behavioral analysis. 
It is imperative for many businesses 
to measure and understand what is happening in Checkout. 
And that in itself actually creates the challenge 
that PCI v4 is running into, 
which is in order to accomplish all those things, 
you need a lot of capabilities and scripts specifically. 
So it's not atypical to find dozens of scripts 
and third-party vendors all trying to execute 
in that top level page. 
And at Shopify, we've taken kind of a hard stance on this. 
And we said that we cannot trust third-party code. 
So we can partition the problems, 
the first-party code and third-party code. 
For the first-party code, 
we have all of our audit mechanisms in place, 
integrity, verification, and all the rest. 
And we can load that safely into the top level page, 
and we can make strong guarantees 
about the behavior of that. 
The moment you drop in a third-party script 
or let alone a tag manager, 
which is very popular because you want to, 
the developer team wants to make the life simpler 
for the rest of the organization. 
So they install a tag manager 
and basically hand over the keys, 
and then who knows what's being injected 
or what the partner might be loading 
when they load their own script. 
Once you invite third-party content 
into that same JavaScript environment, 
all bets are off as to integrity of that whole environment. 
So because of that, we've deliberately 
and very firmly separated all third-party content 
into these isolated workers and items 
where all third-party code executes in their own sandbox. 
And we control the bridge. 
This is the most important part 
where we accept, respectively, commands and DOM directives, 
and we propagate them into the top level page. 
We can sanitize and make sure that those are secure 
and allowed operations, 
and that allows us to maintain this integrity guarantee. 
So if you have not seen Remote DOM before, 
I encourage you to play with it. 
As I said, it's open source and a really great tool. 
All right. 
- Ilya, the thing I would just pair with that 
is an important part of the paradigm 
is that not only does Remote DOM cause all this sandboxing, 
and the postMessage bridge that we use 
for communicating with the parent, 
but all data that is ingress/egress from the extension 
is also powered by APIs specifically written by Shopify, 
which then has all of our data access scope rights 
and all of our PII and privacy protecting mechanisms 
all built into it, 
so that you do not end up in a place 
that even when the web worker is executing code 
that's been written by the app and delivered by the app, 
that it's accessing any information 
that it should not be allowed to, 
even if it's available on the API, 
because they had to have been requesting that scope 
and having that approved 
at the time of app install by the merchant. 
So it just goes even further with putting controls 
around information exchange and data accessibility, 
which then we believe builds 
a far more secure checkout environment. 
- Yeah, that's well said. 
And I think there's many layers to this. 
As you said, there's governance or what data is exposed. 
There's also assurances of Shopify provides a UI library 
that is tested, optimized, tested for accessibility. 
So ensuring that you use the right elements, 
guarantees right buyer experience. 
There's the performance side of it as well, 
because we can make certain guarantees 
about third-party scripts 
not capturing the main thread of the browser, 
so the checkout stays responsive. 
There's a host of additional benefits, 
but it's not easy to achieve. 
And let's be clear about that, right? 
Getting third-party content into a work worker 
technically is not that hard, 
but getting the right code in the worker 
to execute and use the right APIs, 
get access to the right data. 
That's the tricky part 
that we've been on a long journey to implement, 
and we're there and we're harvesting the benefits 
of that architecture and approach. 
So I think this actually leads us to the next question, 
which is really good and related to the above, 
which is with the shift towards stricter security measures, 
how does Shopify balance the need 
for a customizable checkout experience 
with the requirements of highly secure environment? 
So we've put all of these restrictions in place, 
but does that mean that I can no longer 
make the checkout my own? 
- Vani, do you want to take this one first? 
- Yeah, the entire idea of the composability 
was we have to bring to life what we believe 
that the merchant community requires. 
So it would be kind of foolish to build a platform 
that had all of this security and the guarantees 
and the compliance and converts well and all that, 
but you really couldn't make it bend left or right 
to achieve your outcomes. 
Like we know a common thing that merchants do 
in regulated industries, such as tobacco and alcohol, 
might be age verification. 
Well, what is the mechanism by which age verification 
using validations and using third-party systems 
that are able to use a government-issued ID and such 
in order to be able to integrate that 
at the point of purchase online? 
How would you achieve that? 
And if we were not able to do so, 
then is our platform actually the one that allows 
for merchants of any scale to be able to operate on it? 
Another popular one that you obviously spend 
advertising dollars, you want your return on ad spend 
to be as high as possible. 
You have a good established workflow that you've A/B tested 
that gets the buyer from an ad to a PDP 
and then on to checkout. 
But you also have good evidence to suggest 
that thoughtful upsells are going to enable you 
to expand your average order value. 
Well, should that be possible or not? 
In Shopify, previously we had built 
what's called the post-purchase extension 
that is very actively used for upsell offers 
that come after the purchase, 
but there's also plenty of evidence to suggest 
that for specific merchants in specific workflows, 
doing that in checkout also makes sense. 
So certainly the idea here was we need to be opinionated 
about the way to bring composability 
and customizability to life, 
but that is part of the three pillars of success here. 
Without it, you would not build the checkout 
that the internet needs or deserves. 
Yeah, so maybe building on that. 
There are three key extension primitives that we provide. 
There's functions, which allow you to customize 
the backend logic of how Shopify Engine works. 
So for example, if you have a custom discounting engine, 
you could bring your own logic 
and replace some of the built-in behavior in Shopify. 
So that's executed by Shopify on Shopify servers 
in the backend. 
There's the UI extensions that you've just described, 
which allow you to provide different elements, 
inject new capabilities into checkout, 
and this is done via remote DOM in a secure way. 
And then finally, pixels, which you alluded to. 
So conversion tracking 
and all of the behavioral data is critical. 
How do you instrument that? 
That is also powered by Web Workers. 
And as you said, a key feature there is this event bus. 
So there's a Pub/Sub where you can collect events 
that are provided by Shopify. 
There's a standard library of events 
that cover a lot of the activities, 
like anything from interacting with elements 
to conversion events, 
and also publish and emit custom events. 
So as an extension developer, 
if you built a UI extension, 
you can also emit events that others can subscribe. 
And the benefit of having this bridge in between 
and a Pub/Sub protocol is that 
you have composable behavior between these applications. 
So if you install an app, an analytics app, 
it can just subscribe to a star for all the events 
and get the UI events from another custom app 
that is providing a UI customization. 
So all of that works together in a secure way. 
- Yeah, let's actually linger there for a minute 
because there's some really interesting use cases here. 
For example, we did a lot of work to figure out how 
any app that required additional DOM events 
for the purposes of, for example, 
recording screen capture behavior, 
the most common thing that the market would say is, 
oh, let us have access to the unsandboxed DOM 
and we can listen to all the DOM events ourselves. 
And we said, no, we're going to find a pattern 
by which we're able to emit sampling 
that gives you enough data that you're able to do the thing 
that you need inside of your app. 
And then there could be a class of apps 
who are specifically for this purpose 
that through governance are getting access 
to the additional scope and are able to be public. 
Examples of this are like Lucky Orange or Noibu 
or a few others. 
And that was something that we deliberately went 
and designed as part of the same platform 
of these standard events that we're publishing 
over this Pub/Sub. 
And then there's one other thing 
I think you forgot to mention earlier 
that I think brings the whole thing together, 
which is the branding API. 
And the idea that the way you style the checkout 
is through a defined schema that we provide 
that allows for us to then uniformly apply 
all of those customizations 
to both the native checkout of Shopify, 
which is built with the same 32 component library 
that we offer for all app developers to use. 
And so the theming and styling of the native components 
will be identical to those that were written 
inside of a custom app, maybe bespoke to that merchant, 
which are also identical to any customizations 
delivered by a third-party app in our app store, 
otherwise known as a public app. 
And this is the first time ever 
that we're able to use these platform capabilities 
to bring to life full contiguous experience for the buyer, 
no matter how the customizations or native features 
were combined together, 
all the security and performance and everything else aside, 
it brings two other elements of, 
one is compliance, 
and then the other one is maximum compatibility. 
So on compliance, so WCAG, 
which is Web Accessibility Guidelines, 
you can think about all these different versions of that 
with insufficient color contrast. 
It could be keyboard and accessibility, 
missing things like alternate text on images. 
It could be not having form labels properly filled out. 
And this can happen not just with the native code 
that you expect to be perfect from Shopify, 
but in any customization done either, 
again, by a customer public app. 
And we're now able to make all that uniform 
and then theme it and auto adjust 
in places where things are not quite right. 
For example, the color contrast is a great example 
for background versus foreground or header versus subtext 
and ideas like this that are now baked 
into this branding API. 
And the reason that this all comes together 
in a really beautiful way is that the world 
of online shopping is moving more and more 
to a world where identity ends up powering 
an accelerated experience. 
And of course, there are many identity providers 
and wallets out there, and one of them is ShopPay. 
And ShopPay is able to automatically, 
because it's an identity provider at the top, 
and then underneath what sits there 
is the checkout of Shopify, 
we're able to apply all of the branding of the merchant 
across native first and third party code 
inside of a ShopPay experience, 
where the buyer gets the beauty of that one-click checkout 
and the recognition without the merchant having 
to sacrifice how they show up in front of that buyer, 
which is a pretty magical experience 
that we think is fairly unique to what we've built. 
And again, it's an emergent property 
of these principles that we've used that, again, 
one of the pillars was compliance 
and ensuring that e-privacy compliance 
would work even in ShopPay, 
and WCAG compliance would work even 
with your customization in ShopPay, so on and so forth. 
So just thought I'd share a bit about that history as well. 
- Yeah, that's really helpful. 
Just as a reminder, folks, 
if you have any questions, please raise your hand 
and we'll bring you up to the stage. 
In the meantime, as folks are thinking of the questions, 
Weston, I'm curious, let's take a turn 
into kind of a more technical implementation. 
So we've described the landscape 
and the properties of the extensions, 
but let's say if the two of us are actually, 
we wanna build a UI extension. 
How does that process work? 
What tools do I have at my fingertips? 
How do I debug? 
If my extension is running in a worker, 
what does that mean in terms 
of my end-to-end development experience? 
And then what about that alphabet soup 
of content security policy, sub-resource integrity, 
and all of the other things? 
Do I need to concern myself with those 
and how do I ensure that my extension is compliant? 
- Yeah, I think that's a good question. 
And I kind of joke like the headache 
of alphabet soup becomes my problem 
and our problem here at Shopify, right? 
So when you're building your extension, 
you're probably not really thinking about CSP or SRI 
or any other of those acronyms, right? 
Because we basically built it in such a way 
where we can kind of abstract that away from you. 
So it'd be interesting to explore 
like how did we make that happen? 
But, you know, kind of looking at how do you start building 
your extension and then into debugging 
and what makes it secure? 
So you start with setting up that Tomo file 
to describe your extension, 
what capabilities it might need from us 
so that we can give you the right API access you need. 
And then you can just write your code using our UI components 
interacting with the APIs we've exposed. 
And you get that flexibility to write code 
as if it's just running on the page. 
So, you know, one thing that I would say 
looking at some of even our examples 
or our tutorials for building extensions, 
like the pre-purchase offer, 
I think we have a good tutorial there. 
That was one of the first ones I looked at 
when I started here. 
It just looks like plain React code, 
you know, a couple imports from Shopify, 
but, you know, those are our components 
and our APIs we're giving you. 
So those imports should make your life easier. 
But once you get into the meat and potatoes of the component 
it looks like JavaScript I would write 
in an application I owned. 
And then in terms of thinking about, 
so now what do I do to be PCI compliant 
or any other sort of compliance? 
I'm going to focus on PCI 
'cause that's where my head's been the last 11 months, 
but there's really not much for you to do at that point 
because we're sandboxing that extension you wrote. 
So we're making sure it doesn't have access 
to the parent page. 
We have that inventory of the code you wrote, 
the contents of the code you wrote. 
So like from an auditing perspective, 
if someone were to come in and ask, 
how is this extension compliant? 
We can answer questions like that and say, 
you know, we know what it is, 
we know we control loading it 
and it really just sort of takes the burden away. 
So because it's in a sandbox, 
we don't have to worry so much about 
what it's doing from the parent page perspective 
because it can't access the parent page. 
So I really think it goes really well. 
So thinking about the three-letter acronyms, 
you don't really have to think about it 
because you can just write code 
that you've already been writing. 
We'll take care of the rest. 
From a debuggability standpoint, 
I think there might be questions such as, 
how are we sandboxing you, 
but what does that mean for your debugging story? 
So if we're taking away access to the top-level page, 
we have this lockdown checkout environment. 
Does that impact you from seeing what your app is doing 
if you have a bug? 
I personally don't believe so 
because what you can still do is, 
again, similar to what you've done 
in any app you've written before. 
You can start at console logs, 
the most basic primitive, right? 
You can add console logs 
and see the logs of what's happening to debug. 
That's typically what I end up doing as a first resort, 
to be quite honest. 
You can still set breakpoints in your code 
and step through breakpoints, 
and basically just using the tools available in the browser 
that you already had available to you 
if you were writing any other app. 
So to me, writing an extension feels 
just like writing regular code, so to speak, right? 
- Yep, that makes a lot of sense. 
So just to reiterate, 
I think that the key thing I think we want folks 
to understand is that PCI 84 really talks 
about the compliance at the parent page 
and protecting the parent page, the integrity of that. 
And by putting all the content into an isolated sandbox, 
we're effectively isolating all of the third-party, 
the implications of third-party code 
and the fact that we don't know what it does 
into this contained environment, 
which can then execute whatever it needs. 
And we know that we control the bridge 
that interacts with the parent page. 
So that's where this integrity protection 
really comes into play. 
Now, I have follow-up questions on what you just said, 
but I think we actually have a live question coming up. 
So why don't we bring Younes on stage? 
Can you please introduce yourself and then fire away? 
- Hello, guys, how are you? 
Younes from Outremont. 
We are a consultant firm. 
We work with Shopify and a lot of brands, mainly in France. 
And yeah, I have mainly two questions, 
but before I get to the questions, 
like the other day I was prototyping a checkout UI extension 
and I stumbled upon like a hook 
like that says you ship an address 
and then it didn't work for me. 
So when I checked, like it took like some few minutes 
to understand why it didn't work. 
And then when I checked the code, 
I realized that I didn't have the permissions 
to access the data on the checkout. 
And I was like, oh, this is a really, really good 
because like you can not access customer data 
unless the, well, obviously like what Mani said, 
unless the customer, the merchant here approves it 
via like the permissions mobile when installing the app. 
And I think that's like, that is something 
that is really good to see, especially in the checkouts 
where like data is extremely sensitive 
and that anyone could access. 
The merchant should obviously give the permissions. 
And it's also like really good to see 
how extensions overall evolved in extensibility 
in the checkout evolved in the last like two years. 
Like things are being opened more and more. 
And now we have like components 
that we never imagined we would have like two years ago, 
like QR codes, chat, et cetera. 
That's all amazing. 
I have a question about compliance. 
My first question is how, like, 
if a merchant asks us if the components we add 
at checkout is compliant, 
how would we communicate our compliance to a merchant? 
Like the merchant would obviously know 
that Shopify's checkout is compliant, 
but his main question would be like the code, 
the extra code you add to my checkout 
or the scripts you add to my checkout, are they compliant? 
How could we communicate this to the merchant? 
Yeah, that's basically my question. 
- Yeah, that's a very good question 
to which the answer is probably gonna be nuanced. 
So maybe the three of us will take stabs at. 
It really depends. 
By the way, Eunice, thanks so much for the kind words 
and always being in the ecosystem 
and jumping onto threads and giving us feedback. 
I appreciate it. 
The answer is gonna always be around 
what is the nature of the change being made? 
So for example, are you able to make breaking 
web accessibility changes? 
Yes, it is possible. 
The system is not perfect at being able to detect 
every single place where the WCAG 2.1 AA guidelines 
are guaranteed to have been compliant. 
They are from our own custom code 
and everything we're able to detect and enforce 
via the component library and the branding API. 
But I think there are still ways 
that you could set up color schemes 
or you could set up a changing of font sizes and such 
that can break those guidelines. 
So the answer has to be mostly of like, 
we're using the components, we're using the branding API, 
and we're not doing anything that is in our experience 
so out of the ordinary that it would require 
some independent assessment of the execution 
still being compliant or not. 
So I think for the vast, vast, vast majority of cases, 
that should be a pretty straightforward conversation. 
When it comes to PCI, the answer ends up being yes. 
Shopify has done all the isolation here. 
Now, in PCI, and this is where I can't remember 
what the form looks like 
for some of the self-attestation portions, 
but there is still gonna be, if you remember 
from the requirement that says scripts are inventoried 
with business or technical justification, 
you may still need a list 
that justifies why every app was installed. 
Even if that feels self-evident 
or if you were to be audited, 
you would be able to explain it 
because you have the full inventory of every app installed 
or installed historically, 
you may still need some additional documentation 
that justifies why every app was installed. 
So are we able to get your work down to zero? 
Probably not. 
But is your work gonna be substantially lower? 
Absolutely. 
And we could take it from there, 
like e-privacy. 
Well, Shopify has recommended behaviors 
that are out of the box, 
but then they're also configurable, 
that if a merchant for any reason decides 
that they understand something 
about their legal requirements better than we do, 
then we allow them to configure it 
such that the requirements of cookie consent or whatever 
are adhering to what the merchant believes is correct. 
What we're doing is our best knowledge 
with our legal team and our research 
of what the various jurisdictions around the world 
should be allowing. 
Same goes with things like marketing consent, 
collecting email or... 
So we offer collecting email, 
what's it called, like a acceptance form by default. 
And then you also have the ability to do SMS 
and you can have apps that power SMS or email 
and it all feeds back down into our customer API 
that ensures that we've captured the right consents. 
And so again, out of the box, yes, compliant, 
but is it possible to come out of that compliance 
based off configuration or customization? 
The answer is also yes. 
And then that requires like extra rigor 
to figure out what is the risk being posed to the business. 
So I'll stop there 
and that's more of like a product perspective on it. 
I can let Ilya or Weston jump in 
if they have another angle that's worthwhile. 
And then Eunice, you tell us 
if that answers the spirit of your question. 
- Yeah, I wanted to dive in a little bit more 
on the PCI side specifically. 
Yeah, so one of the benefits to extensions 
when you think about, 
am I gonna still be PCI compliant when I install this 
is that there are these applications you're writing, 
they're not run on the top level payment page. 
So PCI is very concerned about what scripts are running 
in your hosted field sandbox 
where you're accepting those raw credit card details 
like your FPAD. 
And then on the parent page 
when you're actually taking what scripts are running 
that could maybe replace your card fields iframe 
or listen in on what the user's typing into it. 
Well, we sandbox that off the parent page. 
So the execution context isn't the parent page 
and we've restricted how it communicates 
to and from the parent page. 
So it doesn't, from a PCI standpoint 
when the auditor is saying, 
effectively I'm looking at all of these requirements 
and asking myself, can this site be attacked 
such that I can skim credit card details? 
Well, you'd say these extensions are sandbox. 
They can't do anything on the parent page 
that isn't controlled by Shopify, that isn't gated. 
So it really does tell a good story from PCI perspective 
that these aren't running on the payment page 
and can de-scope them heavily. 
I think Mani's point makes a lot of sense though 
is that it is always beneficial 
to understand why you're running things 
on your page in general. 
And that's sort of icing on the top to those auditors 
or when filling out any requirements for PCI saying, 
I also know the extensions I'm installing 
even though they are sandboxed. 
And it is easy to provide justification at that point 
because of our app store 
and the ability for you to install them 
and monitor what you're installing and all that. 
- Yeah. 
Just to add to all of everything that's been said here, 
a key distinction I want to also describe 
about the architecture that we're describing here 
is that we're providing a strict runtime guarantee 
about integrity as opposed to a retroactive auditing 
of, hey, has anything changed? 
Has an unauthorized script made it onto the page? 
Is the integrity invalidated of a particular script? 
So as Weston has said, 
by moving the content of your extension into a sandbox, 
it allows us to move that entire liability 
and the blast radius into its own contained environment. 
And because we control the bridge in between, 
we can make strong assurances about this. 
So this actually simplifies a lot 
in terms of the blast radius that we need to think about. 
If you are operating in a world 
where you're deploying third-party scripts 
directly in the top parent page, 
that gets really complicated. 
That you then have to, as a merchant or as owner of Checkout, 
work with every provider 
and figure out what are their assurances. 
How do they protect the authentication 
and integrity of their scripts? 
Have a change management process 
and all the rest. 
So that gets really complicated really fast, 
which is why the architecture 
that we're describing here at Shopify, 
I think is likely to become the default in the future 
for other providers and other Checkouts as well. 
But it's not an easy lift. 
- Yeah, I would say- 
- Also, I wanna use this opportunity. 
- I just wanted to add that communication 
with other third-party vendors. 
I mean, that was a lot of non-trivial work. 
And I think things that hopefully, 
we've done a lot of homework 
and chatted with a lot of third parties about this. 
And hopefully that can benefit the broader ecosystem. 
But yeah, there's a lot of work communicating 
with your vendors around, 
how are you providing the integrity? 
What is your list of known origins? 
If you're not gonna sandbox them, 
you have to answer a lot of deep questions 
about what they're doing on your page. 
And that's just exceedingly difficult to do 
when you're not the author of the code. 
- Yeah, I mean, all that makes sense. 
Thanks for the response. 
My other question is mostly about, 
what's your strategy about new sales channels? 
Like when you guys opened to native mobile apps, 
you introduced Checkout Cheat Kit, which is amazing. 
Like we worked with Immersion to implement it 
and they reduced so much like tech depth 
from their custom like Checkout, 
which they manage using like Swift 
and all these like native libraries to build it out. 
And like, it's just cross-channel. 
And my question would be, 
what would your strategy be like to stay compliant 
across all different sales channels? 
Because if you go today in POS, 
we still have like a payment widget 
that says enter your credit cards 
and it's not Shopify's Checkouts. 
It's like, I think it's unembedded, 
unembedded like credit card input. 
I'm not sure, but you still can like get payments 
through POS from using like a credit cards form, 
but it's not Shopify's extensibility Checkouts. 
And also there is new channels that you guys are expanding 
to like Roblox, for example. 
And how would the Checkouts experience be 
in those new channels? 
And how does that impact compliance 
and also like user experience? 
- Yeah, I'll take this one and then we'll jump to, 
I think we have one other person 
who was looking to ask a question before we run out of time. 
So the magic of this is that the web Checkout 
is the one that powers all of the compliant experiences. 
So the Roblox example you brought up, for example, 
is us having a web view-based integration with them 
that they run on mobile and also on web 
that has some specific patterns 
to working with that kind of channel partner 
for attribution and such, 
but otherwise is powered by the same thing 
that's powering the online store. 
The Checkout Cheat Kit is the same story. 
There is a skin on top and there is some additional 
message passing that's possible in the bridge 
within the sheet that's popped up, 
but otherwise it is underneath an idiomatic skinning 
of the web Checkout. 
And what this allows us to do is standardize 
on building the tech stack once 
that allows for all of this compliance 
and then deploying it to all the places 
and all the channels where the merchant needs it. 
So that's the secret, Eunice. 
Okay, so just want to make sure we're able to ask 
that one last question, although I don't see, 
I think it was Takish who was on having a hand raised, 
but maybe you're not on downstage. 
So Eli, I'll hand over back to you. 
- Sounds great. 
Takish, please put up your hand 
if we haven't answered your question already. 
I want to use the last couple of minutes 
maybe to also talk briefly about, 
you know, we described all the great things 
that this provides, but one topic that's near 
and dear to our hearts is of course performance. 
And there is a cost to all of the perimeters 
that we're talking about, sandboxes and isolation. 
Weston, maybe you can talk a little bit 
about how we've approached that problem 
and first of all, let me just follow the question. 
Is there a performance cost? 
Is there a degradation because of all of this technology 
that we've put in place? 
- Yeah, you know, I think about not so much extensions, 
but some of the work we've done to sandbox 
other third party codes, such as the wallets in checkout. 
And there's absolutely a potential performance implication 
when sandboxing. 
You know, for instance, you might have to make 
an extra network hop to fetch that sandbox iframe. 
And that's going to add latency into the equation. 
One of the things we did in that realm was, you know, 
we really looked at our rendering story 
and our serving story to make sure that we are serving up 
the sandbox from buyer points of presence. 
So basically using edge caching and edge CDN providers 
to ensure that, you know, if a buyer is in, you know, 
let's say a remote region of the world 
that they're not having to hop all the way 
to some singular data center to get the contents 
of that iframe, slowing down our ability to load, 
let's say a wallet, such as like PayPal, for example. 
So that was a big piece of outside of extensions 
that we had to do in order to ensure that there is 
no performance degradation or minimal degradation 
when we sandbox some necessary third party code 
that we might use on checkout. 
When I think about, you know, providing those 
integrity guarantees through sub-resource integrity, 
you know, I don't really see a big performance cost on that 
or applying those content security policy headers 
that it ends up being a little bit more straightforward. 
It's about getting your data and ducks in a row there. 
But certainly sandboxing third party code 
is the first thing that comes to mind 
when I think about being cautious around performance 
when trying to meet PCI compliance. 
So. 
- Yeah, that's well said. 
And I think one of the points of professional pride 
that I like about Shopify, that at every edition, 
I think at every edition, we have a statement 
about how checkout and cards are getting faster 
every six months on repeat. 
So while the parameters that we're talking about here 
certainly do add overhead, we've been harvesting 
what performance optimizations along the way 
and across all the different layers. 
So the net effect is that the checkout 
is still getting faster. 
So we're able to negate some of the costs. 
And further, we've been doing a lot of work 
with the browser developers and browser agents 
to optimize and reduce this overhead. 
So instead of, for example, instead of relying 
on client side logic to collect hashes 
and do all of this in the client, 
we've been upstreaming patches into Chromium 
and WebKit to get this implemented natively in a browser 
such that not just Shopify, but the web community at large 
can benefit from these optimizations 
and get the right guarantees in place 
without imposing heavy costs of yet more JavaScript 
having to execute on every single page load across the web. 
So if you wanna learn more about some of that work, 
we have a blog post on our developer blog 
about these app compliance, some of this unboxing and links. 
So please take a look at that. 
And I think with that, 
we're actually coming to the end of our AMA. 
So thank you to everyone who's tuned in 
and shared questions. 
If I know that we didn't address all of them, 
I see a few queued up. 
So we'll make sure to follow up async and address those. 
And in closing, if you haven't already, 
please do follow Shopify devs to stay up to date 
on all things related to PCI 
and other exciting things happening in Shopify. 
And we'll see you next time. 
- Thanks folks. 
And thank you, Manny and Weston for joining us today. 
- Yeah, thank you. 
- Yeah, it was a pleasure. 
(beeps) 
(beeps) 
(beeps) 
