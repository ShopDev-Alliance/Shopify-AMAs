# Building Shopify Apps with AI: Partner Insights X Space

**Date**: 2026-03-12

**Platform**: Twitter(X) Space

**Host**: [Liam Griffin](https://x.com/liam_at_shopify) (Developer Advocate)

**Co-Host**: [Prakhar](https://x.com/PrakharS96) (FoxSell)

**Guests**:
* **[Henry Johnson](https://x.com/TalktoHenryJ)**: iziGift: Gift Cards & Rewards
* **[Farid Movsumov](https://x.com/faridmovsumov)**: Craftshift apps
* **[Alex Romm](https://x.com/saasromm)**: Epsifund
* **[Sandesh](https://x.com/heysandy801)**: Stoq
* **[Kurt Elster](https://x.com/kurtinc)**: Ethercycle
* **[Tuki](https://x.com/tamir_eden)**: 40RTY.ai 

## TL;DR

Shopify partners are undergoing a massive productivity shift by moving from traditional IDEs like Cursor to AI-native terminal workflows using Claude Code. Key strategies involve using Model Context Protocol (MCP) servers for real-time API accuracy and creating autonomous pipelines that manage everything from merchant feature requests to automated code reviews. The developer's role is evolving from writing code to architecting systems and performing final "human-in-the-loop" sanity checks.

---

## 1. The Migration to Claude Code

* **Direct API and Cost**: Developers are shifting from Cursor to **Claude Code** to avoid the "margin" added to API calls by third-party IDEs.
* **Increased Productivity**: Users report being up to five times more productive in a terminal-first AI environment compared to previous setups.
* **Hybrid Workflows**: Some developers still use Cursor as a visual editor for reading code while utilizing Claude Code for heavy lifting and terminal-based execution.

## 2. Managing AI Context with Documentation

* **Instruction Files**: Partners use `.md` files (like `CLAUDE.md`) to provide persistent system-level context and project rules to the AI.
* **Teaching Polaris**: Because LLMs often lack training on the newest **Polaris web components**, developers create custom documentation files to prevent the AI from hallucinating old patterns like "S-Cards".
* **Technical Specs**: Context files are used to specify requirements such as Node.js version 20, NPM usage, and specific directory structures for backend, frontend, and theme extensions.

## 3. Autonomous Pipelines and "Engine Rooms"

* **Automated Feature Building**: Alex Rom detailed a pipeline where an AI support agent receives merchant requests, logs them to Trello, and a development agent writes the code while an independent "Reviewer" agent suggests improvements.
* **The "Engine Room" Dashboard**: Sandesh (Stock) built a dashboard that aggregates data from Slack, PostHog, and merchant calls to provide real-time sentiment analysis and help decide which features to build next.
* **Operational AI**: AI is being used for "grunt work" like analyzing thousands of merchant conversations to identify requested features without manual review.

## 4. Advanced Tooling and MCPs

* **Shopify Dev MCP**: The community emphasizes using the **Shopify Dev MCP server** to ensure the AI has the most up-to-date information on Shopify APIs and GraphQL schemas.
* **Devin for Triage**: Engineering teams use Devin to analyze logs from New Relic and PostHog to generate bug reports, saving hours of manual debugging.
* **Chrome DevTools MCP**: Tuki recommended using the Google Chrome DevTools MCP to allow AI agents to open a browser and debug complex DOM event conflicts in real-time.
* **OpenClaw**: This tool is used as an autonomous wrapper to log into Slack or GitHub and analyze the implementation cost of items in the roadmap.

## 5. The Architect's New Role

* **Code as a Commodity**: Developers are choosing to "not write a single line of code" manually, focusing instead on system architecture and prompting.
* **Platform Knowledge**: Success now depends on understanding Shopify’s specific capabilities (like Functions and POS extensions) to guide AI agents effectively.
* **Toil Reduction**: The end goal of these tools is to remove "toil"—tasks like writing complex Regex or boilerplate code—so small teams can focus on high-level product strategy.

---

## Full unedited transcript

Liam Griffin
Hello everyone, thanks for joining. This is definitely an overdue X-space I feel. I think a lot of the folks in the Shopify partner community have been chatting about AI and the different ways that they've been using it independently. So yeah, I think it's a really good idea that we hop on here and chat about it a little bit more. So I'm going to just give a minute or two for everyone to join. So get comfortable we'll start our our Main shot in a couple minutes We'll definitely make sure there's opportunity for everyone to contribute. I know there's so many really cool use cases being explored. I really want to make sure that we get everyone chatting about it.

Liam Griffin
I hope my co-host per car is here. Oh, here you are. Just sent the co-host invite.

Speaker 3
I should have prepared some fun music to play in the background.

Liam Griffin
Cool. I think we're going to kick it off. I know it's people will go in and out and that's good. But yeah, first off, thanks for joining everyone. My name is Liam. I am joining here for Ireland. Procar, just sent an invite to speak. Yeah, I'm hosting this with Procar. Hopefully I can set him up with, I just send the invite to speak. So hopefully you'll be able to.

Prakhar
- Hey, thank you so much. - All right. - Sorry for the technical issues. Can you hear me?

Liam Griffin
- Yeah, loud and clear. Hopefully all have no technical issues on my side. There's actually a big storm happening outside. So if I drop off at any point per car, you'll be able to take over for me.

Prakhar
Oh yeah. - I think I have great people to partner up here.

Liam Griffin
Awesome. So yeah, thanks everyone for joining us today. We're going to be chatting all about AI and all the amazing, cool, fun capabilities that are available to us. And I guess like this all came from a tweet that I saw Prokard shared a couple, maybe like a week ago, talking about his experiences with open claw and all the different potential uses from a partner, Shopify partner perspective. So I thought it was just a really good time to maybe gather everyone together, talk about all of the different things that are available. And yeah, maybe, Procar, if you want to kick us off, maybe you can tell us a little bit about how you guys at Fox So are using AI in your workflows. You were telling me before about how the Dev MCP specifically was able to unlock, being able to build POS extensions. So maybe that's a good place to kick off. Also, it's great to see all the familiar faces in here. There's definitely a couple of people here that we'll want to hear from folks like Fred, who we've got Jake in here, we've Sandesh, Spencer, Alex is here. Alex, I wanna hear about the really cool pipeline that you have for automating support to building features. So yeah, maybe, yeah, Procaro, kick us off and then we'll start bringing people in. I might call on a couple people to chat.

Prakhar
So yeah, here we go. Yeah, absolutely. I think we are heavily invested in this ecosystem now. I think this is the way forward in the future. And we started off with using Shopify Dev NCP to just think about building few stuff. One of the most requested feature for us was enabling like make customizable bundles on POS. And we just was going through and just testing the waters of how we can go about it. And we just asked DevNCP. We just integrated with DevNCP, asked Cloud Code to just build up the skeleton. And it actually built a lot of the working stuff with a lot of different use cases that we had in mind. And we just had to tweak a lot of things. So it was actually an insane experience. It would have taken us about a week or so to build all of the use cases that we had in mind with the customizable bundles, but it took us literally like few days or one or two days to test everything and to push it to production and people started using it. So that was like a major shift in how we started using it, especially in the Shopify development ecosystem. And now we've started deep diving into open claw and agents and automating our supports, escalation, task management, PR reviews, those kind of stuff. So yeah, it's a pretty, pretty amazing world we are living in.

Liam Griffin
- Awesome, yeah, I love it. I think what's really cool is just everything around like the speed to like, like having an idea and actually seeing that like in real life, being available to merchants and customers is like, it's just so cool that that rate of development is really, really quick. Yeah, who's here? I hope it's okay to maybe call in Henry, just 'cause I've been watching all of his experiences with getting started on Cloud Code. Henry, I'm going to send you an invite to speak if you want to have a chat. I know it's been, for me, very interesting watching the migrating to Cloud Code and all of the different experiences, some good and some bad. But maybe he doesn't have my invite. That's okay. Maybe, Procar, while we're waiting for Henry, can you tell us a little bit about like working with open

Prakhar
claw just as like a side tangent? Yeah, I think we've been deep diving into open claw a lot. We have set up like multiple.

Henry Johnson
I'm on but I'm getting like, I'm getting like both of you. It's like, it's like delayed.

Liam Griffin
Oh, no. Well, Henry, do you want to hop on?

Henry Johnson
Yeah, I'll be, I'll be right back. I'm going to hop off and hop back on because it's like,

Liam Griffin
Oh, no worries, no worries.

Prakhar
Yeah, so I was just mentioning the open class setup that we have. We are trying to do a lot on the internal support and external support sides on the open class side of things on setting up the agents, RPR reviews, our code changes. So essentially what we have to do is we just have to operate everything on Slack and do a lot of testing. But eventually it has really increased the productivity of our complete team because everyone have access to these agents. They are on our Slack as any IT mates. So that has been pretty amazing learning. It comes with their own challenges, but it's like amazing.

Liam Griffin
Is there any concerns around security or permissions that you have? And like, how are you like mitigating any of those risks?

Prakhar
So right now what we've done is we have taken a complete different dedicated server and we have given access, role based access to our Open Claw agent. So they have their own emails, everything. So in case anything happens, we just have to rework access to their emails, all of those things. And we are constantly updating our Open Claws to mitigate all the securities that are coming in and are getting patched. So that is the measure that we are taking there.

Liam Griffin
Yeah, it's definitely like a tricky space to navigate everything around security because with really great powers and really, yeah, there's so much you can do that there is also the potential for things to potentially go wrong as well.

Prakhar
Exactly. And do you want to go next if you're here now?

Henry Johnson
Hey, yeah, I'm here. I don't know. I don't have a ton to add. I'm really just getting started with Cloud Code. So I'd imagine that a lot of people have more experience with this stuff than me. But I am really excited about Cloud Code. I've been using-- Oh, my mic muted for some reason. I've just been using it and Opus 4.6, 4.5, or all the other LMs before. But I read a post by Trung. I don't know if anybody knows what Trung is, but he does a bunch of tech stuff. And he just essentially was saying how you're getting ripped off by using cursor because they add their own margin to the API calls. So I was like, all right, let me try Cloud Code. And you just get so much more usage of these models if you go directly through Cloud. So that's why I started using it. And I was able to push two new features in the last day, which I could have done through Cursor, obviously, but it just used so much less of my API usage. So that's exciting. And now I'm learning more about using the other aspects of Cloud Code, like the Cloud MD file. I see that it can do autonomous agents and things like that. I don't know a lot about it yet, but I'm looking forward to like diving into that and the skills and such.

Liam Griffin
- Oh, well, we'll keep posting on X. It's been really fun to watch your journey. And I've actually only relatively recently started, like moved over to Cloud Code. And I didn't expect there to be a massive difference from what I was using previously, because I was thinking like, So it's all the same like language models at the end of the day that being used. But it was actually such a night and day difference. Yeah, like five times more productive. I should have made the change a long time ago.

Henry Johnson
- Yeah, I'd agree with that. The one thing I'd say that was interesting is like, I still use cursor as like my IDE and then I use cloud code on the side of it. 'Cause I was trying to figure out like how to read and make sure that the code seemed good, right? because if you're just using the terminal, it's hard to know exactly where things are happening. If it's simple, it's easy, but following the code, if you're not an ID, makes it a little tough. So that would be my one thing. And I got a bunch of advice from a bunch of people in here, too, so I see you have my Fareed up, and he gave me some good advice. I'll let you guys get going.

Liam Griffin
- Fareed, are you up for chatting? I think I sent you a invite to speak, if you want to join too. Also, be--

Farid Mosumov
- Hello, Liam, can you hear me?

Liam Griffin
- Yeah, I can hear you.

Farid Mosumov
- Yeah, yeah, I'm also using a lot of cloud code actually. So that's for me also really cool thing at the moment, like for both for development and marketing actually, I'm using that a lot. We can also maybe touch a little bit how we are using it for marketing, But for me, it was like kind of no brainer to start cloud code because first I started with cursor, but I couldn't get used to ID actually like because it's a bit different. I'm normally using PHP storm and that was kind of what I used to and more comfortable with is that. but then switching ID was really difficult. And, but because I'm already very comfortable with terminal, cloud code was no brainer. As I saw it, I say, okay, that's the tool that I'm going to use from now on. And yeah, I think it's very important to create some also docs for it. Like every time I'm starting some tasks to not give every time like all the context from scratch, It's very important to create some MD files. Like for example, I have like Polaris.md because now with Polaris web components, AI doesn't know it very well. Like they are not trained on that. Probably they are trained more on old Polaris. And now when they see that, they don't understand. For example, it will try to write something like S-Cards, but there is no S-Card element in Polaris. So I would create that and then explain, like always visit this documentation before using some elements or like check MCP servers. Also MCP server is really helpful, I think. That's also helping me a lot. So when AI checking MCP servers, cloud will check it automatically. You don't need to even say that, but you can give hint that just check documentation and it will check that. So I think that's very important to create these MD files for these kind of steps.

Liam Griffin
- And just on the MCP service as well, I hope everyone on this call is using the Shopify Dev MCP. If not, make sure, like just even right now, just set it up. It's super valuable. It's something that like we're updating all the time. So you can be quite confident that anything that's coming from the Dev MCP that's related to our APIs is going to have the most up to date information.

Farid Mosumov
- Yeah, yeah, and do you want me also to touch how I'm using it for marketing a little bit as well?

Liam Griffin
- Yeah, definitely. I've definitely been following what you've been posting about particularly around planning and stuff. So do you use it when you're doing marketing? Is it planning marketing campaigns? What does that look like?

Farid Mosumov
No, actually what I did recently with this new project, I actually created all documentation because it's connected to repository and it knows about all the features available in the app. So I say it like analyze all the features, everything that what exists and creates FAQ pages, like basically all kind of documentation about one by one by features. And that's also will help discoverability app from AI perspective because people now searching a lot with LLMs and LLMs searching for this kind of documentation where they actually check like what this app is doing for example and I think that can be really helpful just to connect app repository and then listing the all the features all the FAQ what can be done with this app what settings exist what can be changed and people basically like for example we have one section where people can write custom CSS. And then you can easily check this documentation scan because you know all the features, all the classes available, it can create great documentation. And then people just can give it to LLM and then write custom CSS for our app. That's also this kind of flexibility. I think great content can be created. And also I just connected it to my WordPress blog, actually, you can just create API key from your WordPress account and give it to AI, Cloud, and then it will directly, again, it knows about your repository and it can directly create blog posts from your repository, directly based on your features, and that's also very helpful.

Liam Griffin
So you're giving it write access?

Farid Mosumov
Yeah, yeah. And it's right. I kind of now gradually started trusting Cloud more and more because with the version 4.5 it's get smarter and in general Cloud is security research company in the first place like on Trophyq this company is their first thing they are actually the focusing is security and I think their security features are really well like I think it's It's easier to trust them when doing this kind of stuff and it's always asking also do you want to do this. Yes, like I will not never do like dangerous this skip permissions this I'm still not doing, but I will always track and see like what it's doing. is just sending API request to write post, okay. And it's always publishing it draft and asking me to review. So it's kind of also not directly publishing it live as well.

Liam Griffin
- Yeah, that's good. I think definitely hear you on the security aspect. And yeah, it's good that permissions are being asked. Sometimes I feel like it's asking permissions a little bit too much, but maybe better too much than too little.

Farid Mosumov
- Yeah, I agree. - Cool.

Liam Griffin
I'm wondering, Alex Rom, are you, I sent you permissions to talk if you wanna chat. I know you got some interesting stuff that you've been working on.

Alex Romm
- Hey, thanks Liam, can you guys hear me?

Liam Griffin
- Yeah, sounds great.

Alex Romm
- Cool, yeah, absolutely. Yeah, thanks Liam, and you did mention the pipeline that we've built for software developments. And the way it works, I'm happy to describe and then perhaps a couple of points that have been mentioned by other people, such as using CloudMZ file and also using Shopify MCP. I can touch upon those elements too, because we actually did not sort of initially use Shopify MCP. And when we started, we've definitely gonna see an improvement. So the pipeline is pretty simple. So we initially launched their customer support agents using OpenAI, ChargePC for all back in the day. And we've also given it a bunch of MCPs, essentially access to the code base, access to the database, which was tricky and initially quite scary. It did expose some interesting data points which we had to work on to make sure that this graph properly secured. I'm obviously a knowledge base and even screenshots of the onboarding of the app, which actually are the customer support agents worked with quite nicely and actually been able to sort of help the customers with. And then we've seen that there is almost a constant sort of stream of our merchants coming and asking about the feature in a new feature that they want to be built. So we just initially piling them into Trello boards trying to sort of prioritize them manually. And then we just decided, okay, let's just go ahead and try and sort of automate it. So what happens now are the feature requests will be sent to the Trello boards via API from the customer support agents and then the development agents, which is on cloud code and would actually take their sort of incoming or description, which would be quite short and it would write more details, explanation of what needs to be built. Then it would move it into the next card. And then we essentially wanted the human to have a look and see what a description is good quality. We did notice a few problems with that, where certain important bits would not be picked up. So we still kind of want the human in a loop at this stage. But once the human is happy, it would sort of move to the next card, the next column. And from there, development agent would take it out, start coding. And once the code is written, it would then allow the session to move it to the next cart. And we're using the reviewer, which is kind of independent instance. We're using OpenAI for that. Would come and start sort of writing reviews, comments. And then at the same time, Claude would be effectively checking those comments and addressing them. All kind of happens in real time. And once they're all addressed, it would then sort of move it to the next stage, which is again kind of ready for the merge. And then again, we want human to check what's been done because we did notice obviously it's not always perfect. We do want to make sure that the code is always checked before it's merged into the main. And then after that, it's basically our merging. And see if it's auto deployments, and it just kind of goes into production. If it's something super simple, most of the time it works without too many problems. If it's something a bit more complex, we did initially notice that it would, for instance, go and start sort of writing code in free repositories. We've got backend, frontend, and the theme app extension. Whereas in reality, sometimes you're like, oh, well, actually, I don't actually need code anywhere, except for the theme app extension. And when we add a Shopify MCP, it basically stopped doing that. It has, I think, a much more clear understanding of how things should work. And obviously, we also developed the cloud and Z file a bit more, we've been adding limitations, restrictions into that. I know people are arguing to what extent it should be complex or it should be simple, but so far, it just keeps growing for us. I guess it will be a point where we need to stop sort of, you know, feeding it more information. But yeah, ultimately, that's kind of what we've got currently and we've been quite happy with the way it's been working.

Prakhar
- That's really cool, well done. - Actually, I've actually seen the updates that Alex sends for the new app and the update. That's, have been amazing. But just before anything else, I just want to say, If anybody does have questions for the speakers or in general, just feel free to post them in comments and we can take that.

Liam Griffin
>> Yeah, 100%.

Henry Johnson
>> Can I ask one since just Alex, has he just spoken about it? On the MD file, what are you like putting in the cloud MD file? Sorry, again, I'm newer to cloud and maybe somebody else listening is too. Maybe it could be helpful. I'm just trying to understand how that works and what you're using that for.

Alex Romm
- Yeah, absolutely. So essentially what we've said, what kind of text that we are using, Shopify, CLI, essentially version of X1 Z, and a theme app extension for the front end, it's liquid templates, vanilla JavaScript, CSS. Okay, what we're using for the backends and we actually put the link to the backends, the URL. Also specify the package manager, essentially saying here's NPM, Node.js version 20, and the CI/CD that is currently being used. And then we've put some sort of commands that we kind of expected to run. And I think you need those commands in order to make sure that, for instance, you know, the connection to Shopify MCP works and, you know, it's gonna deploy node on your ARM server for it to access it. Also put a very simple product structure, essentially, you know, the high level and then kind of a few branches underneath. Again, just to describe the way the kind of structure looks like. What else is there? Put a few conventions. What else? Yeah, I mean CSS and liquid templates. Also with, you know, a few details. any API integration, also whatever we currently have. And the config, just explaining, here's the Shopify app config, Tombal files, importance, extension config, needs to be used. Here is the Dev Store that you can actually use to test and see how it works in the API version, Shopify API version, that's pretty much it.

Henry Johnson
- Makes sense, Patrick, thanks.

Liam Griffin
- That's really, really cool. And I love hearing as well that you noticed like a big improvement in terms of what you're saying about like scoping changes to just the places where it made the most sense to make those changes in the code base when you used the Dev MCP. I was also wondering like, have you made any other like changes improvements? Like do you tweak maybe like the using different models in different places maybe to like keep costs low? Like are you like tweaking the whole system as you like watch it operate?

Alex Romm
- Yeah, I guess we're not that progressed yet. We obviously did try a few models when we started, but so far we haven't really kind of gone to the stage of playing around with them enough, but that needs to be done absolutely. And I think that's the right course selection.

Liam Griffin
- Cool, yeah, no, but please do keep us posted on the evolution of this. I think it'll be like an amazing workflow and it's really cool to just see, being able to, like a merchant, making a feature request and that actually being developed in an automated way and then being able to go back to that merchant and say like, hey, that feature that you asked for is available now. I think that's just amazing.

Alex Romm
- Yeah, still need to close the loop and improve a few things, but yeah, ultimately we'll definitely keep you guys posted on that one.

Liam Griffin
- Actually, one last thing, like how do you, like do you set up rules for what would be examples of an out of scope request? Like how do you stop, how do you prevent something that you don't want to do?

Alex Romm
- Yeah, I mean, apart from what we've put into the Cloud MD file, there's nothing really, We obviously have those two intervention stages for the human where we're kind of trying to catch, you know, the issues. So a couple of times we've noticed at the stage of description, it would sort of start saying something which isn't totally relevant to what needs to be built. And we would try and cut it off and say, oh, no, actually, that that needs to go out of the description. So there's not really an automated way so far to, at least we haven't done it, but that's probably something we need to think of. And as we work more and more and more and more kind of features that develop this way, we'll probably need to put something in place.

Liam Griffin
Yeah, well, it's also maybe a good position for a human to be in the loop at that point as well to be like a sanity check.

Alex Romm
Yeah, yeah, no we still feel it's probably required Especially for more complex stuff and and in general, you know, there is definitely some sort of way to optimize things Initially, we had a problem when reviewer was sort of doing reviews and putting them in and as comments The court sort of would come back and start sort of working on them and they would just go into the infinite loop of just generating more and more comments. And obviously you spend a lot of tokens and it's just not helpful. But we've managed to kind of eliminate that problem. Again, by kind of giving it some limitations. But still, I think that that is probably something that can happen and something to be aware of.

Liam Griffin
- Yeah, yeah, no, it's such a really cool way of working and such a great work floats. Yeah, just keep us posted on how this evolves. It'll be really cool to see. Sandesh, I sent you an invite to speak if you want to chat about what you guys are doing on stock. I also see you're Asmus, you're in here too. I also sent you an invite to speak. Would love to hear your experiences on the theme side of things and working with Liquid.

Sandesh
- Yeah, hello guys, can you hear me?

Liam Griffin
- Yeah.

Sandesh
- Yeah. - Oh, awesome. - Yeah, I think we haven't been using, I think just like everyone else, we've been using AI to build a lot of our features. And I heard you guys talking about Cloud Code, like very similar, we switched from cursor to using the Cloud Code extension inside cursor, and it is a big difference. I'm actually using AI a lot more for our operations in general, 'cause like one of the hardest things that we have had to deal with as we're growing and scaling up and dealing with lots of apps is trying to understand like what we should think about and what we should work on next. There's, you know, like there is LCP to think about. There is our install funnels, like our merchants actually staying on. There is information in conversations with merchants, like they're chatting to our support agents and we need to know whether they're asking for something That's interesting, something we should build. There's information from post-hog about our events. We do calls with merchants and there's feedback. So there's a lot of information and it's kind of hard to get every person to put things in Slack and then eventually track it. So we built an internal dashboard we call the engine room. Kind of the engine room is basically like a reference to a ship and the bottom most level is the team that's actually making the ship running. So the engine room's job is to get all of that info from Slack, from post-hoc, from calls, from our database, and to just give me a surrounding summary so I can just keep tabs on everything. And we've integrated a Slack bot into it. We've integrated all the APIs you can think of. And it's basically trying to give me a real-time view everything that we're doing across the board, be it engineering escalations, water merchants up to all of that. So that's what we've been using AI for the most. And I think the hardest part in general of Shopify app, once you get past the point where you are thinking about what features is, how do you keep this running? So it's a really good way to operate your business in the same way. I would show my screen if I could here, but it as like a dashboard with all the different tabs possible, but it's just making API calls to get all the information and just give me a running summary at any point. One of my favorite features about it is like sentiment analysis, like real time sentiment analysis. I'm sure everybody knows that like reviews and not having negative reviews matters a lot. So we're just analyzing sentiment of like what merchants are telling us in real time. So we know that it does escalation. We should just jump in and like try that will actually be escalated or to help them in some way. So yeah, that's pretty much how we've been using it.

Liam Griffin
- And that's awesome. I think almost like the most tempting area to focus on is like the, like actually building and writing code, but it's like there's so much potential there in terms of what you're talking about of, you know, measuring sentiment, getting like research from what you, the activity that your existing clients are doing, what are they asking for? And then like using that to make like really informed decisions and like previously, like being able to collect and distill all of that information would have been just like so time intensive, but now it's like you can get it so quickly. And like you said, now you've like a dashboard with all of that. So it's just at your fingertips all the time.

Sandesh
- Yeah. And I know Devon doesn't get a lot of love generally, but if you're running at scale, I think Devon is also a really great tool to integrate into Slack. So for example, we often get escalations from our team to engineering about some issues. It could be some bugs. It takes a lot of time for an engineer to debug and figure out what the issue could be. You'd usually spend half an hour to an hour or maybe two. Nowadays, what we do is we just say, @devin, look into this and give us a report. And it has access to all the MCPs, like our bug reporting tool, our New Relic for logs. It has access to Post-Hog. It has access to even data from our dashboard or from our database. And it can give us a report of what exactly happened. So we can then focus our energy on solving the exact issue quickly, instead of having to spend time triaging and understanding where it could have started from. And that has been a huge unlock because it's not something that you would, an engineer doesn't have to pause everything and think about what how to resolve this. They can just say, "Add Dev and investigate this "and tell me why it's happening." And if you get a report, then they know exactly what to do next. So that, again, is from an operational standpoint, it's really helped us stay on top of things that would have taken the fine on of working.

Liam Griffin
- Yeah, but that does sound like a huge productivity win. And definitely I'm neglected looking into Devin a lot. You kind of have to, from my experience, you kind of have to pick one or two and just kind of double down on those. But no, Devin is definitely on my conscious mind to check out. Kurt, we'll throw the mic over to you. I think you had a question that you wanted to ask.

Kurt Elster
- Yeah, yes, except, well, Liam, could you hear me? Oh, I got the right headphones on.

Liam Griffin
- Again, we can hear you loud and clear.

Kurt Elster
- All right, good, okay. So I've been using Claude code for about six weeks now. So relative beginner and starting small than working my way up through projects and giving it administrative control of home servers in my house was my test case for it before I unleashed this in a production environment. - Sounds safe. - Yeah, my app development partner, Carl Meisterheim, already using Clawd for some development on our app together. But you know, I'm trying to get better and understand it. Anyway, I recently found frameworks for it. Like, oh my God, there's so many guidelines on like, here's the right way to use Clawd code. Of course, very quickly I found like, it just, is any LLM, it is so dependent on quality of prompting. And so these, I tried Get Shit Done, GSD, which is the spec driven development protocol. That thing's really cool. By the time I'd like found it, used it, tried it, tweeted about it, 30 on V2. So I was curious, like, are there, are people rolling their own frameworks? Are there existing frameworks? Like, what is that initial prompting spec approach look like?

Liam Griffin
- That is a great question, especially feel you on the sense that everything is changing so quickly. So when you find something that feels solid and reliable, it already feels like there's multiple versions of iterations that are exceeding that. Yeah, who wants to chat about frameworks and what their recommendation is?

Henry Johnson
- Oh, Kurt, can I just clarify? Are you saying like prompting to like build features and things of that nature like asking it to to do things for you? No, you're referring to yes I guess I'll start the car. So like you're about to go I'm gonna start because I don't have a ton, but I just use whisper I don't know if anybody uses whisper flow. I think it's called and I just give it as much context as I can I don't have like a specific framework in terms of like how I ask things When I was using cursor, I had like cursor files that it would always look at before Moving on it sounds like that's what the Claude MD files do So I'm probably going to follow a similar path so that I don't have to follow like a framework in terms of like the way I position my prompt, but maybe there is more there than I'm missing

Kurt Elster
And by whisper flow, you mean the voice to text transcription. Yes, exactly Yeah, I tried that, uh, you know, I use it at home where it could be alone in an office but in a my office office in a shared space it just isn't practical it does hear you if you whisper by the way you know I have it I suppose that if you know

Tuki
that's why they call it whisper but I haven't tried it we started to use I can hear you guys yes yeah oh so about frameworks we recently added superpowers to Claude code I just add a repository to their comments here we got great feedback from the developers with it. It really helps them plan a task and have a lot of autonomous coding later on. So they iterate on the planning quite roughly and then it gets a very good result at the end. You should check it out.

Kurt Elster
Yeah, I just love this up. I'm looking at superpowers. Yes, similar, really similar concept to what I was doing with, or to like what Git Sh*t done uses, where it helps you like write the spec, it creates phases and milestones in documentation. And really what it's doing is like solving that context problem. Because what I found with development is like, you know, you get what you want, and then right around the third revision, things fall apart because of context rot. Like it is, every time it is compacted the conversation, you're losing something. And so that the get shit done approach really like try breaks the phases and stages but tries to work within the context limit. So at the end of one phase, like you check it, verify it, you know, a human's in the loop and then you go, you know, clear context window and then you tell it start phase two, reach documentation starts again.

Tuki
Correct. We also had a lot of, I would say, like the PRD and architecture around every task with like the full system, current state. And so far we've managed to deploy a few very big features, including even a small refactor in the backend in an API level that we had that went out quite fast and very successfully.

Henry Johnson
Yeah, I was going to say that I actually always ask you to build me like a roadmap in terms of how I'm going to put out a feature, especially if it's like more than like something super simple. I'll say to break the roadmap up into sprints and they each have to be independently deployable and independently valuable. It's valuable where I'm able to negotiate with a little bit more, but I want to be able to deploy things into production independently as I do those prints so that I can move forward. Having it in that MD file means that I can clear the context window and get back to it. It sounds like that's what superpowers are doing, so I definitely look into it.

Kurt Elster
Yeah. Yeah, because two years ago when all of us were starting to use LLMs for the first time, very quickly discover like, oh, it's dependent on clear context to whatever you're trying to do. You slam into that context window very quickly and then it gets dumb very fast. And it is just so dependent on input equals output, garbage in, garbage out. And so moving to phase-driven, very specs development, man, that seems to be, like this phased approach seems to be the unlock. And I only stumbled on it when trying to troubleshoot stuff on these home servers, like Plex Media Server, various stuff, home automation, where I give it a problem and you can tell what it like by default, it's just going into a loop and it's just going to try stuff over and over and over until it gets it versus with a simple problem, maybe that's okay. But then when I start telling it like, "Okay, here's the problem. I want you to propose a troubleshooting plan." And then it gives it to me. Okay, that makes sense. Go, which two years ago we found one of the early prompt acts was you would add the phrase, think step by step, before they had thinking models and that would help. A lot of the stuff that was table stakes for a, suppose a prompt engineer two years ago, now I'm like, oh, I'm rediscovering this now that I'm applying it to these much bigger problems. Just code is so much more context.

Liam Griffin
Yeah, it's really evolved in such a short time. And yeah, I think part of having these kind of conversations here is that we're able to share like what's working, what's different, like even just in terms of like prompt engineering, how we organize projects and stuff. - Tuki, I might have another question here for you. Someone is asking, "Medi was asking, "do you think future Shopify apps will be built mostly "by AI agents or will developers still need "to architect the core systems?" And maybe like what you're doing with Open Claw, like you might have a little bit of context on what this looks like.

Tuki
- Cool, so I think it's a big question everyone is thinking about how much developers would be needed in the future. I think the developer role is changing tremendously in the past year and also would change looking forward. I think it's more of being good developers, I would say, historically with two types of people, like ones who are very deep in the understanding of how the framework work or the coding language that you use. And the other kind were very like system architecture and engineering experts. And I think that today the LLMs are solving more of the deep like coding problems. They might not be as good as like highly knowledgeable developers, but the fact that they can write and implement so fast and nowadays doing a refactor is not as bad as it was in the past. Like I can take the whole API and just rewrite it in a day, makes the coding patterns less important, but this overall system architecture very much important in my opinion. So I think the developers, and I like obviously show up if I have developers would need to have like a more high level thinking about which APIs exist, what are the obviously product use cases that they want to solve. and how are things going to build in the workflow for the operators and have a more understanding of what the product is and what tech can solve. And then let agents write it for them. So my current app 40, I started it six months ago. Basically, when we started it, we took a decision to not try to single line of code. We're a group of very expert developers, I would say. Like we do it for quite a long time. And we understood that if we write code, we're actually delaying the execution. So if we learn how to use framework good enough and how to prompt good enough and understanding how to leverage LLNs where they operate good or not, and put a guardrails around them, that would help us execute as fast as we can. And this is what we do. And it's evolving. So frameworks are evolving and LLM versions are evolving. Today, I think Cloud Code with Opus 4.6 and the frameworks around it can do quite amazing stuff almost autonomously with the right guidance. So we see quite a successful there. OpenClo is like the new toy in town. So we started playing with it. So OpenClo is the open source, I would say, autonomous agent that can run on any computer or Mac computer and actually use every interface or tool that humans can do. We deployed OpenClo in two select channels. One is for bugs, bug reportings in our app. And the second one is with feature requests. And for now, what OpenCloud does, it has full access to the repository. It doesn't write the code yet, but it checks any request about a bug or a feature request. It loads the backlog and the roadmap that we have existing. And it checks what is the feature request, for example, how far is it from the current state of the system? If it's something that is easy to implement or not, what will be the refactor or the implementation cost. And if it's planned in the future of the roadmap, it marks it, it ties it to the future item that you have in the roadmap. And it helps us gain visibility, like which items in the roadmap are more important than others. So if you have anything like any system that manages feature request from users right now, I highly suggest you try having an LLM, taking those feature presence seeing where they fit in your product roadmap and how things should change to include them as well. Just for an example.

Kurt Elster
I have open claw questions because I haven't used it yet. I'm open claw curious. So like claw code, I get to benefit from being subsidized by Anthropic, right? You know, like pay 200 bucks for the Max Plan and essentially, you know, for practical purposes, limited usage for me anyway. You know, I've only managed to hit the limit once. Open, Claw, we have to use the API, right?

Tuki
Yeah, OpenClaw is more of the wrapper. You can use any LLM you want. We just give it access to Cloud Code with a max account, so it has a full Cloud Code instance of its own. And then it can use it as much as it wants. You can use it API as well.

Kurt Elster
just you and on the computer. All right, that that part I didn't know that it could use cloud code like use my plan as opposed to use the API.

Prakhar
You could use your subscription as well.

Kurt Elster
Yeah, all right, that part that I I couldn't get a straight answer on so good to have that confirmed.

Henry Johnson
If I'm not mistaken, I don't think you're supposed to though. I think you can I think you can get like canceled on your.

Liam Griffin
I think people were getting rate limited. - Yeah, we're using the API gate.

Kurt Elster
(laughs) - So what do you, like, in that--

Tuki
- No, no, but for us OpenClaw is not writing the code, so it's most using for certain in the repository and analyzing the repository, so it doesn't really, it's not, it doesn't use so much, it doesn't use it, do so much usage on Cloud Code,

Kurt Elster
eventually. - Okay. But why use, like, why use OpenClaw for this versus, you know, roll your own? You're clearly sophisticated enough, or use a different platform, like NADAD or anything else?

Tuki
- So one thing OpenCloud is doing very good. It can open any document from your Google Sheets or going into your Slack, login type messages, or use the Slack API, obviously. It can log into your GitHub and do searches there. So it's very much on tunnel. So because we weren't sure what will be the, I would say the tools needed to assess which feature request is relevant and where to put it in the backlog and how to view the implementation effort. We just used openclaw. It worked very nice and we just kept it for now.

Prakhar
Okay. And also just to add to that, I feel initially we were using like a lot of different tools for different types of use cases. So there was bugbot who reviews our code. Then there was AI support agent that was doing support for us basically when we were asleep. Of course. So now with open claw and having multiple agents with their own context, with their own training and specifically for your use case, it's much more cost efficient, much more, it's more productive for us and giving like very high output as well. So that has been like really well.

Liam Griffin
I guess also going back to like that original question on developers being replaced. Um, that's that sort of concept. Uh, and it sounds like from what you were saying, the, the value of a Shopify developers moving more towards, uh, being like really aware of the platform and the potential for building on top of it versus the actual skill of coding. And it's more around how do we take a concept, spec it out, and then actually execute that. And also foreseeing what are the things that we should build that will make a really big impact for merchants. And also, I mean, the platform is constantly evolving too. Like there's also-- there's so many like new APIs and new capabilities being released that a big role for the partner ecosystem will be interpreting what's possible from the new APIs and capabilities.

Tuki
100% I think that Shopify developers, the more of being a developer for them is understanding the platform itself and the capabilities and how things work. So people who have good understanding of our products and listing the collection or even like different functions and discount and all the features that Shopify offers and how they can use like combination of those four creative solutions. that's I think where the magic usually happens. What we did recently, like the entire app that we built in the past six months was built on top capabilities that were added. I think in a year ago, in May 2025, I don't remember the additions for maybe Liam can help me, but the last additions in Toronto, like it was released there, the stuff on MCP, we started from it and we built a lot of our app. around it and the world from there. So gaining knowledge about what is coming and how the market would want to use it, that's I think the best place for developers to hang around these days.

Liam Griffin
Yeah, I think it was Gil Greenberg quote that always sticks in my mind is, you know, try to always swing with the tide. There's a question here from Harry Prennish that I guess could be open for anyone to answer. And he's asking, as developers, we spend a significant amount of time resolving DOM event conflicts. Is there a way to leverage AI to automate this? Specifically, can AI analyze a website's code to pinpoint exactly, for example, which event listeners are causing interference? Has anyone got experience in that side of things? I also see Harshdeep has just joined.

Tuki
Maybe a suggestion is using the Google Chrome DevTools MCP with Claude or Cursor and let it debug it from there because then you can have full console visibility and the network and everything and potentially can add logs and traces and then find the bugs. So the Google Chrome Devs tools is quite, it's like it runs the browser and it can, it connects the agent for many, any agent you work with and it can get an influence ability about how the website is running in real time in the dorm and everything. So that might be the path, I guess.

Liam Griffin
Can it do like browser testing or is it more like analyzing The like what's being rendered on the front end

Tuki
It's completely turning the like like puppeteer like it. It's not headless. It's with the browser it opens the browser and it Loads the URL and can get to the Google the dev tools inside the Google Chrome. And again, like everything you can do as a developer on the dev tools, the agent can do and see all the network and all activity. So yeah, everything, it's fully loadable also.

Liam Griffin
- Awesome. Yeah, I think that sounds like it could be a solid approach to use and not use case for sure. I know we're coming up to the last five or so minutes. If anyone would like to chat, chime in with any insights or questions that they have, let me know here. Yeah, but really great conversation so far. - Yep. - Harshdeep, I also gave you a speaker permissions if you want to chime in about anything.

Kurt Elster
>> Volunteer, the harsh deeps the man.

Liam Griffin
>> Hell yeah.

Liam Griffin
One thing we haven't really talked much around, which is kind of related to the last point is, and I'm not sure if your résumés is still here, but using AI in the context of team development, or store customization. I remember in the beginning, well, like beginning like a year ago, chat GPT models were not really great with liquid because I think it was kind of relatively niche, but it's definitely come along a long way and the DevNCP for the Shopify DevNCP also has a lot of support for liquid. So I imagine that's a lot better. I just don't have a lot of experiences. would be cool if someone here has experiences on that side. I can imagine it would be especially interesting for folks in the agency world. I think Harsh Deep is in a meeting. He's just tuning in in the background. So thanks for showing up at least.

Alex Romm
- I mean, Liam, not sure to what extent it would be relevant, but we had a few simple feature requests that did not really make much sense for us to implement within the app. What we ended up doing is building those, you know, I think there were two or three of those so far over the past few months, And we've done it with the help of Sidekick, which isn't always perfect, but then it's obviously just the theme, you know, customization effectively. But, you know, somebody's saying, I really want the vertical slider, and it needs to sit on the right-hand side of my blog page. And instead of building that within the app, which doesn't really sort of comply with, you know, what we're trying to do more broadly, but in the app, which is, you know, went ahead and said, well, why don't we try? And it worked. So I think those kind of simple things that we've done with the Sidekick, that was our kind of experience of messing around with the theme so far.

Liam Griffin
- Yeah, it's a good call. Yeah, Sidekick is definitely like super effective for those smaller customizations. And yeah, I can imagine for, you know, for team up extensions, there will be like a really good use case for that as well. Going through just a couple of things questions here that could be interesting. Dennis from. Check out is asking what are people's end goals? I think that's a really probably a nice question for us to wind down on. So what are people's end goals for for using all this tech? Is it to get more free time? Is it to be able to spend more time digging down into what you enjoy? Yeah, what is the end goal for people? Would love to hear all the speakers' impressions.

Prakhar
- I think for us it's been kind of an addiction. So I'm not able to yet find what the end goal is because once you get into this AI deep space, it feels like I don't wanna get out until that cloud code limits gets expired. I'm not able to find out that end goal as of now. But maybe somebody else.

Liam Griffin
Maybe the end goal is that there never has to be an end goal.

Sandesh
For us, it is to like pretty much get rid of anything to do with any pain of like operating a business and gathering information on like what to do next. Like I don't try to use our AI to like build more features for merchants. I think that's a slippery slope and the product eventually just doesn't work great. So we're just using it to do more as a small team and get more time back. Like I don't want to have to go through lots of reports and data to figure out what we should be looking at, but I should just be able to see it easily. And same thing for everybody else in our team too. Like our merchants success lead should not have to review every conversation. They should just get that information to them so then they can take the next step. So yeah, the general idea is do less grunt work ourselves.

Liam Griffin
- Yeah, I love that there's definitely so much opportunity for removing toil, especially like those smaller teams. So like that's so cool to hear.

Farid Mosumov
- Yeah, and remember this when we will try to write some regex expressions where we will spend hours maybe trying to write some like basic code sometimes in StackOrflow trying to find the answer like now all this boring stuff you don't have to spend time with that kind of things. It's like feels basic now to writing regex is so easy with AI. For example, we don't even, I don't even remember now when I entered last time StackOrflow. I was big fan actually was having like high points also on stack or flow like now I'm not even visiting it and learning is much easier like you can ask deeper questions normally you don't have access to that kind of person which you can ask these deep questions about programming like why we are doing this what's advantage disadvantage like we are also learning about I think and focusing on what we love actually. I love building products. I don't like writing like spending time is writing regex but I like the end result of what actually being the end result of writing that regex like what kind of feature is being served to merchants in the end and that's what we love and I think it makes job less stressful like because you are spending less time is doing pouring stuff and it's more enjoying stuff.

Liam Griffin
It empowers you to focus on the aspects of the job that you enjoy the most and then outsource the more annoying things. So yeah, it's a really great end goal. Any other end goals before we wind down?

Prakhar
I think one of the great things of using AI and product and coding becoming commoditized is that we are able to do more and more personalized service for our customers. So because building is become so easy so you can actually get it to a lot of personal requests and then the operational stuff like Sandesh mentions is becoming more interesting and more easier. You can take a look at the reports and actually I would find things that you won't even look at. So from all sides of things, technical and non-technical, it's just becoming more and more easier for us to provide that white glove service to our personalized service to more and more customers. So that is also one thing that we've experienced in ourselves and that has also been working pretty well.

Liam Griffin
Yeah, that's awesome. At the end, it's the customer that gets the benefit from just an overall improved service. And yeah, hopefully they'll keep coming back and keep continue using your services and apps. So yeah, thanks everyone for joining. I think we'll wind down there, but this has been a really great conversation. I think we might even make this a more regular thing. We'll see, maybe we might do this in another couple of weeks. Be really cool to check in and see what else we've learned. And I'm sure there'll be whatever new huge shifts in tooling and AI in the next few weeks. That feels like the pace of change that we've come to expect. But yeah, thanks everyone for tuning in. Let's keep the conversation going on X and yeah, let's schedule another one of these very soon.

Farid Mosumov
You know so much for dining. Thank you.

Liam Griffin
- Thanks everyone for joining.

Liam Griffin
- Thanks guys, cheers. - Thanks folks.