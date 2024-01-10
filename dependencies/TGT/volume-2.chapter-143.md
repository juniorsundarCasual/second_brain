---
id: W0sCScwLJyXnDlZSUMg3K
title: Chapter 143 - Series Representation of Functions
desc: ''
updated: 1646760475063
created: 1644314995723
---

A/N: I use LaTeX conventions to define the mathematic formulae here. It may not make sense on your mobile device or computer. You can find the properly formatted derivation at the Discord Server.

____

"You think of me too highly, Marie," Guy said while shaking his head with a cynical smile. "How do I help you if I can't even see these waves of fate?"

"I just need an idea. An inspiration. A direction. Anything," Marie emphasised. "Anything at all."

Guy released an audible sigh. Honestly, he was a little conflicted about the current predicament. Many thoughts were circling in his mind questioning the feasibility of his intervention. Usually, in a Master-Disciple relationship, there was little space for a third party to contribute especially when it came to details regarding core cultivation techniques, such as The Heavenly Eye. He realised that he was already pushing on a rather precarious boundary by helping Marie that time when she tried to sense the waves of fate. Although he wasn't reprimanded or warned by Krish, Guy had recognised internally that that would be his first and maybe last time.

Yet, here they were again.

"Am I allowed to interfere? Mage Nara?" Guy asked out loud.

'Go ahead,' response was received immediately in his mind through Mana Transmission. 'She expects more than what I can provide her. You're the only one who can satiate her boundless curiosity.'

Guy shrugged affirmatively.

"Your Master has acquiesced," Guy informed Marie, to which the girl subconsciously unwound her tense fists.

"Before I can offer anything, I need some insight into these waves of fate. How do they look? What do they feel like?" Guy then emphasised the last point by adding, "Be as descriptive as possible."

Marie sat up straight and orated her insights and perspective on the topic. Along the process, Markus and Guy chimed in with pertinent questions that augmented their understanding. Following an information-packed half an hour, Marie closed her explanation with a loud, satisfied exhale.

"That just about covers everything," she concluded.

"So it literally IS a wave," Markus hummed.

"It MAY be just the way Marie and Mage Nara perceive it. After all, biases can be introduced by simply naming something a particular way," Guy countered. "For instance, if we label them as strings of fate maybe - MAYBE - they might be perceived differently."

"Nonetheless," Guy quickly added. "The fact that they're observed in this fashion does make our life a little easier. After all, waves are common in physics and are how energy can be transmitted."

Marie and Markus' eyes sparkled as they prepared for Guy's insight, but their instructor immediately caught himself and declared, "Let's continue this in the classroom. I need my chalkboards. Oh, and call in Jean as well! Even if she isn't interested in this, it may be of use to her."

____

Guy and his three students were now at the classroom which had undergone a pleasant overhaul. It was still open-aire - partly due to Guy's insistence on it being conducive to learning - however, the floor was properly built flat with an elevation. There was also a small arena nearby that could be used to practice magic. The classroom as a whole was now more detached from the orphanage. In fact, it looked closer to the newly built schools Guy would work at during his travel, albeit with far more robust foundation and facilities.

"Let me define the problem statement once more," Guy started. "If we redefine the waves of fate as a signal, then the received signal is a superposition of multiple signals each corresponding to a different message or vision. These component signals are differentiated by their magnitude, which should correspond to the likelihood of their occurrence. Am I correct?"

Marie affirmed with a nod.

"Then we require to be able to separate these component signals from the summation and only tune in to the one with the highest likelihood," Guy added.

"This problem actually has a mathematical solution," right as Guy finished this statement, Marie's eyes gained a spark beyond measure that emanated visible excitement. She was enthralled to know that her predicament could be solved by her favourite subject in the world.

"It is a high-level topic, so be warned that you may not comprehend it at first. Furthermore, it isn't a requirement for everyone present to learn it," Guy highlighted. "By that, I am referring to you, Jean. Once you hear it out, if you feel that it is irrelevant or uninteresting to you, you don't have to learn it."

After taking a short pause for dramatic effect, Guy declared, "What if I told you that all periodic functions (or waves) can be defined as a summation of sinusoids?"

His students frowned as they pondered on that statement. They knew what functions were: they were relations that connected input to output given that a single input could only produce one output (many inputs can produce the same output, but the reverse is not applicable). The inputs to these functions are defined as independent variables, which is usually time when modelling most real-life phenomena. A function can be continuous, in that there must be an output for every possible input on the real number line, or have discontinuities. A function can be differentiable across all inputs, in that the change in slope of the function does not change too flagrantly at a given point, or not. 

A sinusoid, when defined in mathematics and physics, refers to the types of functions that repeat periodically or generally resemble sine or cosine functions. Sinusoids have a fixed maximum and minimum value that they can assume regardless of the independent variable. It is because of this quirk that Markus, Jean and Marie found it difficult to fathom that any periodic function in existence could be modelled as summations of sinusoids.

"I know what you're thinking. What if there are discontinuities in the periodic function? What if the function isn't always differentiable? Sinusoids are technically continuous and differentiable" Guy fired. "Well, the complete definition is that a function can be theoretically defined as an INFINITE weighted summation of harmonically related sinusoids. The key is infinite!"

"If that were the case, then it would be possible to define the received wave of fate- let's call it a signal of fate to match terminology- we can define them as the individual summations of the signals that comprise them, right?" Marie chimed in, immediately grasping at the crux of Guy's idea. "Right! Then we only need to look at the signal with the greatest weight and that would be the event of the highest probability!"

"You seem to have absorbed the idea quite readily, Marie," Guy commented. "Does it make sense to you?"

"It's a proposition," Marie responded. "If we take the proposition to be true, THEN the solution seems apt. If we begin with that assumption, then the problem can be restructured as proof instead of exploration. I find it easier to prove or disprove a theory than exploring without direction."

"But I don't quite get how such series can be defined," Marie pointed out.

"There is actually an analogous serial decomposition of functions known as Taylor Series. In that, a function - not necessarily periodic, but one that is differentiable - is broken down into an infinite weighted summation of polynomial functions," Guy proposed.

"Note that this serial representation is only an approximation if taken to a finite sum. And it is centred around a point. Let us assume that it can be written as follows:"

Guy picked up a chalk and started writing on the board.

$$
f(x) = a_0 + a_1(x-x_0) + a_2(x-x_0)^2 + a_3(x-x_0)^3 + \dots
$$

"Here, the a-sub coefficients are unique constants and $x_0$ is the point around which the approximation is being made. If you remember from before, $f(x)$ is how we define a function depending on the independent variable '$x$'. Our task now is to determine what these a-sub coefficients might be. So first, if we take the value of the function at the point along which it is approximated:"

$$
f(x_0) = a_0 + a_1(x_0-x_0) + a_2(x_0-x_0)^2 + a_3(x_0-x_0)^3 + \dots
$$
$$
f(x_0) = a_0
$$

"For the successive coefficients, we take derivatives of the original function:"

$$
f'(x) = a_1 + 2a_2(x-x_0) + 3a_3(x-x_0)^2 + 4a_4(x-x_0)^3 + \dots
$$
$$
f'(x_0) = a_1
$$

Guy then proceeded to repeat the processes and wrote:

$$
\frac{1}{2}f''(x_0) = a_2
$$
$$
\frac{1}{6}f'''(x_0) = a_3
$$
$$
\dots
$$
$$
\frac{1}{k!}f^{k}(x_0) = a_k
$$

"Therefore, the Taylor Series representation of any function can be written as:"

$$
f(x) = \sum_{k=0}^{\infty}\frac{1}{k!}f^{k}(x_0)(x-x_0)^k
$$

"Does it make sense?" Guy called out and observed the faces of his students. He noticed Marie and Markus nodding, but Jean had an unusually vacant expression on her face.

"Teacher Larks," Jean volunteered while raising her hands. "I'm lost."

"Where did I lose you?"

"It's the differentiation. I'm kind of struggling with the first principle of differentiation and limits in general."

Guy nodded while scratching his chin, "Right! I forgot that we are still going through the limits method of differentiating functions. It's okay then. You don't have to sit in, we can cover this at our own pace at a later date."

Jean affirmed audibly before packing up her things to leave.

Guy sighed as he observed Jean's retreating figure, "I was too eager to share that I falsely evaluated the extent of her knowledge. I must have wasted her time..."

"Master," Markus called out while raising his hand. "I'm having some difficulty picturing this."

"Hmm," Guy pondered. At this point, he would very much enjoy having a laptop of some kind to present the effect of the series visually. Suddenly, an idea flashed across his mind.

"You two, enter a meditative state. Quickly!" Guy instructed as he himself seamlessly entered his core. With familiarity, he passed through the clear pool to access The Church and pulled his students inside.

"Look ahead. I will show you a simulation that might be useful to you," Guy clarified as the black screen descended above the dais.

____

**Next**
* [[volume-2.chapter-144]]