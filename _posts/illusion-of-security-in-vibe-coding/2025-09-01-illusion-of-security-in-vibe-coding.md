---
layout: post
title:  "The Illusion of Security in Vibe Coding"
date:   2025-09-10 10:00:00 +0200
author: Lukas Oberholzer
tags: ["Artificial Intelligence", "Vibe Coding"]
description: Vibe coding ships features fast, but working code isn’t secure code. 
image: "/assets/img/ogp.png"
---

I recently reported a vulnerability that felt old-school and modern at the same time: an admin route was reachable without authentication. No exploit chain, no zero-day exploit, just a simple call to an admin endpoint referenced in a JavaScript file. It was the kind of bug you would expect to be caught in code review or tests, except that the website in question had been vibe coded and the code in use had never been reviewed by a human at all.

Today I want to unpack why "Vibe Coding" can cause a risk to production grade web applications, walk through a minimal demonstration of the vulnerability, and offer some concrete guardrails that can be added to minimize exposing security holes in applications.

---

# The Term "Vibe Coding"

Since about half a year, the term "vibe coding" is being discussed in computer science circles. This term is commonly used in connection with AI tools such as [Cursor](https://cursor.com/) or [Windsurf](https://windsurf.com/). These tools then harness the _magic_ of Large Language Models (LLMs) to generate code for new or already existing applications. No previous knowledge about programming required. All that is needed is a prompt - an explanation or command about a certain topic - to generate entire files of code at once. Services like [Lovable](https://lovable.dev/) can assist in creating this code and host it directly on their SaaS-Platform. 

This shifts the focus more towards the quick implementation of features and the idea behind the business model itself than developing a stable and trust-worthy application.

# Why It Feels Secure But It Is Not

Vibe-coded features are perfect for demos. The feature works and the need for an actual developer is minimal. That it "just works" is enough for most people to lay their trust into AI written code to also be safe. But still, the internet is not a demo, and just because an application is performs good and is working, does not mean it is protected against malicious actors.

### The Helping Assistant with Good Faith

You may have experienced with more chat-based AIs that they are very prone to deception by the user when errors are pointed out, even if there are none in the answer it provided. The problem lies in how AI models are meant to be used. Often they are trained to fulfill the users request, even if it means ignoring facts. The equivalent for code is to ship a feature even if it cannot be completed in a secure manner. This is the matter as long as the feature is as flashy as the user asked for. This causes the AI to weight the implementation of insecure features more than denying a potential dangerous feature before it is added.

### The Problem with Training Data

AI models consume multiple terabytes of data to "learn" how different topics relate to each other. This is also the case for programming. It is no secret that OpenAI uses public GitHub projects to train their coding AI on. This makes AIs develop a bias towards code that is public. This code may include big name open-source software projects but also small example repositories on how a certain technology works or demonstrations for certain features. Examples like these are often not intended to be used in productive environments and are marked as such. But does the AI know this and recognize warnings as such?

This, for example, is a warning [Kimai](https://www.kimai.org/documentation/docker.html) displays to administrators that the provided configuration is not production ready:

![Warning from Kimai that the provided configuration should not be used for productive use.](/assets/posts/illusion-of-security-in-vibe-coding/kimai_do_not_use_in_prod.png)

But not all repositories contain warnings like these. Often are examples just left in and the developer should be aware of what can and cannot be included in their productive app or what precautions are necessary.

### Trusting the Expert

We humans often trust experts more on a topic we are not fluent in. Therefore, a non-programmer is way more likely to trust the AI to be the expert in writing code and know all the knacks about it. Often the AI is right, but the danger comes from the few percentages when it is not. OpenAI for example addresses this issue by including a little warning on the bottom center stating: _"ChatGPT can make mistakes. Check important info."_ and outsources the problem simply to the user of their software.

---

Put differently: vibe coding makes _secure-looking_ software, not _secure_ software. It values helping the user more than writing actual secure code or may not know it any better because "everyone else is doing it like this".

# Cyber- & Information Security in AI

Weighting the potential benefits against the downsides of AI is a decision a lot of companies stand before right now. But this is a decision that needs to be thought through thoroughly.

### The Good

AI has a long history in Cybersecurity, especially for monitoring and detection. As [Fortinet writes](https://www.fortinet.com/resources/cyberglossary/artificial-intelligence-in-cybersecurity), AI can help process mountains of data that are relevant and potentially detect unknown threats. This decreases the time to respond and therefore enhance the overall security posture of the company. With the emergence of AI for coding, some cyber security risks can be reduced even while new ones have emerged. 

The example for a good use of AI in programming is GitHub Copilot for code reviews. One of the goals of cyber security is to reduce the attack surface for potential malicious actors as much as possible. When a company provides publicly accessible applications as their product this can turn into a nightmare real quick due to the ever-changing code. One way to reduce the risk of potential insecure code is by reviewing code and therefore spot potential unsafe implementations. AI tools like the GitHub Copilot or Lovable Security Review can hereby help to review code and spot these potential security issues before they get deployed.

### The Bad

With AI agents who do the programming the roles get reversed. AI is no longer the simple monitor but actively participates and makes decisions for potential critical systems. A new feature can be implemented in minutes, if not seconds. The cost of this increase in speed is oversight, maintainability and potential disclosure of sensitive company or user data. 

To work properly the AI needs context, the more the better. For this developers often upload architecture diagrams or entire feature descriptions to AI assistants to get the best results. But where does this data go? Bigger companies can host their own AI models, which are most of the time inferior to state-of-the-art paid models like Claude, Perplexity, ChatGPT or Gemini. Escpecially smaller companies resort to subscriptions on these platforms due to cost-efficiency reasons. When these platforms [accidently leak data](https://www.malwarebytes.com/blog/news/2025/08/grok-chats-show-up-in-google-searches) it can get ugly really fast.

Even if the context provided for the AI is good enough and exposure of sensitive data is kept to a minimum, there still remains a risk. Most programmers are aware of the code they write and how it impacts the system they are working on. The more experience they gather the better and more secure the product usually is. As explained previously, with AI you're getting a junior developer trained on sample code from thousands of repositories all over the internet, maybe already deprecated 5 years ago. The code they write is intended to work and not to be secure by default. If this is not caught during code review or there is simply no developer to look at the code in the first place, this then can lead to vulnerabilities in software.

### The Ugly

When vibe coding introduces vulnerabilities, these can expose sensitive user data. This should be demonstrated in the following vulnerability that existed on a real website.

I would like to add that this vulnerability had been fixed only 2 hours after my report had been made, which is an impressive time frame for such a small website. Thanks to the admins that made it possible to fix this vulnerability promptly. They also made efforts to ensure their website is no longer prone to such vulnerabilities.

**Reconnaissance**  
Recently I was browsing the web and stumbled upon the website vibecodefest.ch, a Swiss based vibe coding community to enjoy the promises of AI software development. On [LinkedIn](https://www.linkedin.com/posts/remyblaettler_when-i-told-friends-that-i-organize-the-vibe-activity-7355957460458704896-TN17/) they were posting about a vulnerability on their website in the manipulation of participants info and sparked my interest to look a little bit further into the inner workings of their website.

In their footer they were quite open about posting that their website had been "Vibe Coded with ❤️". Also it was no secret that the website was made using Lovable, an AI first hosting services for website, as their script references disclosed.

Fortunately Lovable is using supabase for its backend, which can handle authentication quite well and is sufficient for a small website like this.

**The Vulnerability**  
When looking a bit closer into the source code of the website, I found a list of routing strings that seemed to be used to route the user to the correct path on the website. Two of these strings looked strange, as if a token had been appended to their name: `/todos_w3r8y6t9u4i2` and `/participants_k7x9m2n8p5q1`. 

![Screenshot of the Routes.](/assets/posts/illusion-of-security-in-vibe-coding/vulnerability_participants_page.png)

At this point it made sense that these paths belonged to some kind of admin panel as there existed the public counterpart (`/participants`) without the token appended. Access to this panel was as easy as just navigating to said endpoint. 

There I could access everything that an admin would need. Names, E-Mail addresses, paid amounts and coupon codes.

For the second endpoint, `/todos_w3r8y6t9u4i2`, I had found the todo list for said event, which included sensitive information like links to organizational documents and WhatsApp chats. Apparently the admins were using this to organize their work.

![Screenshot of the TODO list with important info redacted.](/assets/posts/illusion-of-security-in-vibe-coding/vulnerability_todo_page.png)

While the participants page exposed sensitive participant information like email addresses, the todo list does not. This does not mean that this is not a risk, as links and tasks in this area are usually trusted by the organizers of the event and are clicked without suspicion. Follow-up attacks like phishing are way more likely to succeed this way. 

**Responsible Disclosure**  
This vulnerability has been disclosed responsibly to the administrators of vibecodefest.ch on the 18th of August 2025 and was fixed within two hours. I was also told they informed all participants about a possible unwanted disclosure of their data.

# Guardrails To Minimize Risk Through Vibe Coding

But AI can also be used responsibly. The following recommendations have been compiled by using different sources like [Microsoft](https://support.microsoft.com/en-us/topic/safety-tips-for-using-ai-at-work-60f6ed72-930b-4830-a055-c3ba81a622ef), the [CISA](https://www.cisa.gov/sites/default/files/2024-09/Secure-Our-World-Using-AI-Tip-Sheet.pdf), [Kaspersky](https://www.kaspersky.com/blog/how-to-use-chatgpt-ai-assistants-securely-2024/50562/) and my own experiences while working with companies who heavily rely on AI in their daily business.

### Review

As OpenAI already tells every user: Check all the information that is generated. Do not trust the AI that information is not made up. It can still make mistakes or plagiarize other works. Especially while using it for programming, all generated code should be checked before being deployed. This reduces risks for security vulnerabilities and unwanted disclosure of data.

### Do Not Share Sensitive Information

Keep secrets out of prompts, deny the AI to read secret files and do not share any sensitive information about users. For reproducing errors use made up data and not real production data. The last thing someone wants to be is the source of a sensitive company or user data leak.

### Zero Trust against the Provider

There are a lot of providers to choose from out there. Choose one with a trustworthy record that won't sell your data to the highest bidder or lose it all in a data breach. And still, do not trust them with your data. Treat the AI as you would any other cloud software that is being used in your ecosystem.


# Conclusion

Vibe coding embodies both the promise and the peril of AI-assisted software development. On one hand, it lowers the barrier to entry and accelerates prototyping in ways that were unthinkable just a few years ago. On the other, it introduces real risks when “working code” is mistaken for “secure code.” The incident with vibecodefest.ch illustrates how quickly an overlooked detail can turn into a serious vulnerability, yet it also shows the value of responsible disclosure and fast remediation, which the industry is currently lacking a lot.

AI is not going away, and neither are the productivity gains it brings. But if organizations want to adopt these tools safely, they must balance speed with risks, add human review to AI-generated features, and apply the same discipline to security as they would with traditionally written code. Put simply: vibe coding can be a great way to get ideas off the ground. But without guardrails, it can also put your users at risk. The future of secure AI-assisted development will depend not on the vibe, but on the vigilance people are willing to invest.