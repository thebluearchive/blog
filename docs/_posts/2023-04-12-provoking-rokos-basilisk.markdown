---
layout: post
title:  "Provoking Roko's Basilisk"
date:   2023-04-12 15:55:42 -0400
categories: Tech Rambles
---

Three weeks ago, on March 22, 2023, the Future of Life Institute published an open letter called [Pause Giant AI Experiments](https://futureoflife.org/open-letter/pause-giant-ai-experiments/). The letter argued that might be at a critical juncture in the development of AI, where the decisions that are made today can have profound impacts on life in the future. The letter goes on to argue that we ought only to develop powerful AI systems once we can be confident that the goals of these AI systems are well-aligned with the goals of human beings and that the outcome of the development of such systems is positive. The Future of Life goes on to boldly call for "all AI labs to immediately pause for at least 6 months the training of AI systems more powerful than GPT-4."

The letter has received mixed reception since its publishing. For one, they've received and vetted over 30,000 signatures in support of the pause to AI experimentation. Proponents are notable figures in computing and technology such as Stuart Russell, Elon Musk, Steve Wozniak, and many, many more.

The AI labs conducting these types of experiments have yet to respond to this letter. It appears that labs such as OpenAI and Anthropic, and AI researchers working at large tech companies such as Google, Meta, or Microsoft will continue their research unchanged.

Truth be told, I wasn't going to write about this topic because I didn't feel that I had anything substantial to add to either side of the argument. Many points which I agree with have already been made. Overall, I think the letter is well-intentioned, but I think that it is unlikely that large tech companies are going to comply with such a pause.

It wasn't until recently that I stumbled upon the topic of (Roko's Basilisk)[https://en.wikipedia.org/wiki/Roko%27s_basilisk] and I suddenly became interesting in writing about the topic and hopefully shedding a new perspective.

## Conception of the Basilisk
Roko's Basilisk is a hypothetical thought experiment created in an online discussion board known as (Less Wrong)[https://www.lesswrong.com/] in 2010. Roko's Basilisk (named after the user Roko, who came up with the thought experiment) is a benevolent hyperintelligent AI system. Roko theorized that if such an AI were to come to exist, then it would be incentivized to punish any humans that may have prevented or delayed its existence. One rather disturbing consequence of this is that simply being aware of the basilisk makes you susceptible to it.

Roko's Basilisk is reported to have deeply disturbed many of the users on the site. It was such a controversial post that all discussion regarding the basilisk was banned shortly thereafter by a site administrator.

The thought experiment has been compared to Pascal's wager, and many have critized it for being pure speculation. The original article can be found (in archived form) at https://basilisk.neocities.org/ for those curious enough to read the original source. Roko argues his point by using decision theory, appealing to the rational nature of AI and how they might frame decision-making through well-defined utility functions.

Whatever you might think regarding how reasonable it is that a hyperintelligent AI would behave in this way, one thing is clear: everyone who signed the open letter to Pause Giant AI Experiment has publicly provoked Roko's Basilisk, should it ever come to exist.

## A Game-Theoretic Discussion Of The Pause
Another interesting thought that came to mind when I learned about Roko's Basilisk was a new way to frame the response (or lackthereof) of large AI tech companies to the open letter -- in particular, understanding the response by using game theory.

Game theory is a branch of mathematics which formalizes the strategic interactions between rational agnets. Game theory can be used to study what the optimal choice is in a given situation. Economists often use game theory to model real-world interactions between firms. We can do the same here.

Let's first consider a simple example where there are only two competing firms, Firm A and Firm B, which are trying to develop artificial general intelligence (AGI). Let's also imagine that upon reading the open letter, each firm has two choices:
1. Pause AI experiments for at least six months.
2. Continue development of more powerful AI systems over the next sixth months and beyond.
Let's also suppose (and this is not an unreasonable assumption) that firms develop these AI in secret until their development is complete and usable for the purpose of some product.

Let's imagine Firm A's perspective. If Firm B chooses not to develop AI for the next six months, then Firm A can gain a competitive advantage by continuing to develop AI secretly. Alternatively, if Firm B chooses to continue to develop AI in secret, Firm A can only fall behind by choosing not to do so as well. In other words, no matter what Firm B chooses to do, it is in Firm A's best interest to continue development of more powerful AI systems, since can only lose competitive advantage by pausing.

This situation is completely symmetrical, however. Firm B reaches the same conclusion, that the only good outcome is to continue development of more powerful AI systems. Furthermore, this scenario generalizes to as many competing firms as one might imagine. In essence, it's equivalent to the (prisoner's dilemma)[https://en.wikipedia.org/wiki/Prisoner%27s_dilemma].

The problem is that by pursuing their own interests, these firms are barrelling toward greater existential risk with each new sophisticated AI model. What causes these firms to fall victim to the prisoner's dilemma? There are two main reasons:
1. Lack of information. Because firms are capable of developing AGI in secret, there is no reason for one firm to believe another which claims that they are not training larger AI models.
2. Lack of cooperation. Each firm has only its own best interest in mind; typically, its goal is to maximize its own profit as well as the profit of its shareholders. Because of this lack of cooperation and coordination between firms, it is unlikely that safety will be prioritized in place of speed.

## Is there hope?
Despite everything I've written above, I'm optimistic about the future. I don't think it will be easy, but I believe that we can figure out the right set of actions to take. There are plans we can, as a society, put into action in order to ensure that 

In fact, the Future of Life Institute published (a set of policy recommendations)[https://futureoflife.org/wp-content/uploads/2023/04/FLI_Policymaking_In_The_Pause.pdf] shortly after publishing their open letter. Among the recommendations are guidelines for how to prevent dangerous AI systems from being developed. These include
- Regulating large amounts of computational power
- Expanding AI safety research funding
- Establishing standards for identifying AI generated content (such as mandated AI "watermarks")

We can prevent a serious AI disaster from occurring, but only if we take these policy guidelines seriously and hold corporations which train these models accountable. To quote the open letter once more, "Let's enjoy a long AI summer, not rush unprepared into a fall."
