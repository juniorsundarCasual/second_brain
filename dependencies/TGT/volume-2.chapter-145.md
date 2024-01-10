---
id: 1lNY8RUSbAf1HeX99f293
title: Chapter 145 - 4EA (Part Two)
desc: ''
updated: 1647004756459
created: 1644914058885
---

I use LaTeX conventions to define the mathematic formulae here. It may not make sense on your mobile device or computer. You can find the properly formatted derivation at the Discord Server.

____

Rome wasn't built in a day. Guy recognised that building up to the topics of Fourier Series and Transforms would take quite a bit of time, even with Marie's enthusiasm and receptivity to the rather arcane mathematical concepts leading up to it. All that said, Guy had seriously underestimated Marie's aptitude. What Guy assumed would take him at least a few weeks was accomplished in just one.

Guy and Marie were alone in the classroom. After Markus' incisive remarks the last time, Marie had become more conscious about intruding into Guy's personal time with technical problems. She did, however, poke in once in a while to play a game or mess around with Markus who admittedly took his post as a Disciple way too seriously.

The girl was to maths like fish in water. Guy breezed through integrals and most other prerequisites, most of which Marie covered on her own with just a textbook. Needless to say, Guy was impressed and proud. Marie was a veritable mathematical genius, along the same veins as his elder brother, who she beat only because of the benefits being a mage provided.

"Today we will complete our rather hurried journey towards learning the Fourier Series and Transform. Before we begin, do you have any questions about what we've covered till now?"

Marie shook her head in response. "I'm all good, Teacher Larks. We can begin."

With that, Marie entered a meditative state and Guy did the same to enter The Church through his core. Once the two had reentered through the dark screen that descended over the dais, Guy's voice spoke up.

"At its core, the Fourier Series harkens back to the same process as the Taylor Series, in that it is an infinite summation of functions to approximate another function. While the Taylor Series achieved this using polynomials functions, the Fourier Series does so using sinusoids. However, the limitation is that the function being approximated must be periodic, as in it should have a repeating pattern."

As Guy said so, a Cartesian plane materialised in front of Marie. On it, a simple square function was drawn: one that has a value of negative one between $t=-\pi$ and $t=0$, and a value of one between $t=0$ and $t=\pi$.

"Notice here that the function is discontinuous. You should have guessed by now that it is impossible to approximate this function using the Taylor Series as it isn't differentiable. However, that is not the case with the Fourier Series. AGAIN, as long as the function is periodic in nature, it can be decomposed."

Guy stretched his hand, causing the single period of the step function to stretch out infinitely in both directions.

"Let us revert to the analogy between complex numbers and periodic functions."

A circle with a radius greater than one took form at the origin. On its circumference, lay a red point. The circle started to rotate and roll forwards, and as it did so the red dot started to trace out a line that closely represented a sine function with the same period as the square wave.

Once the circle disappeared from view, the drawn line faded lightly and the circle returned to its origin. This time, at the location of the red point on the circle's circumference there was another circle with a much smaller radius. The red point translated to the circumference of this smaller circle.

As the two conjoined circles rolled forward and traced a line, Marie noticed that the two had varying frequencies of rotation as well. The smaller one rotated faster than the larger one. The traced line also had a warped shape with a small dip at the peak and trough of the previous (faded) sine wave.

This process kept repeating, with more and more circles joining the ranks, each with a different radius and frequency. As more were added, the traced line started to get closer and closer to the square wave that had faded into the background.

"This is the concept behind the Fourier Series. The mathematical definition is as follows:"

$$
f(x)=\sum_{n=-\infty}^{\infty}A_ne^{i\frac{2\pi nx}{L}}
$$

"where,"

$$
A_n = \frac{1}{L} \int_{-\frac{L}{2}}^{\frac{L}{2}} f(x)e^{-i\frac{2\pi nx}{L}}dx
$$

"Here, $L$ refers to the period of the function."

Guy paused to let the information sink in with Marie. The girl gazed ferociously at the formulae, internalising it to the best of her abilities.

"Why don't we try some examples?" Guy suggested, to which Marie nodded lightly.

The two retreated from The Church and Guy produced a few questions curated from the many readily available university-level textbooks in the RoK. The girl had some difficulty getting started but, unsurprisingly, she quickly assimilated the knowledge and solved the rest of them at record speed. Guy suspected that not even actual university students from Earth could breeze through those questions with the same level of ease as she did.

"I'm ready to move on, Teacher Larks," Marie declared as she stacked the answer sheets to the side.

"Very well."

Upon returning to the virtual space in The Church once again, Guy pointed out that, "The next section will be fairly congested. We need to look into Fourier Transforms as applied to Continuous and Discrete-time functions. However, what will be of use to you is the Fast Fourier Transform that can be applied to sampled data. I cannot teach that to you very well as it is also a bit beyond my understanding. Though I can provide you with resources that will be of assistance."

Guy then started his explanation of the Fourier Transform. By definition, a mathematical transform aims to shift the domain of a function from one to another to make it easier to solve certain problems. A basic example is logarithms. Logarithms are used to transform exponential functions and solve problems where the exponents are unknown since these cannot be determined in the exponential space. In the case of the Fourier Transform, this conversion is made from the time domain to the frequency domain.

Fourier Transforms, and their subset the Laplace Transform, can be used to solve differential equations. A differential equation in the time domain becomes a polynomial equation in the frequency domain which is a lot easier to solve.

After explaining the concept, Guy revealed the formula to perform the transform for continuous-time functions.

$$
F(\omega)=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{\infty} f(t)e^{-i\omega t}dt
$$
$$
f(t) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^{\infty} F(\omega)e^{i\omega t}d\omega
$$

"This is genius!" Marie exclaimed in awe. She had already applied this transform on the very same square function from before and obtained a graph that indicated the same frequency and magnitude values as what she had obtained by performing the Fourier Series decomposition.

"Though I can see how this won't be directly applicable to my problem as of yet," she then added.

Guy nodded and continued, extending into discrete-time Fourier Transforms and touching upon discrete Fourier Transforms [A/N]. By the time he finished, Marie had entered a light trance. She hurriedly grabbed the book Guy produced with the details of the Fast Fourier Transform and immediately plopped down on her seat, ready to devour its contents in earnest.

Right then, Guy started to feel giddy from within. His inner mana was growing increasingly turbulent, far greater than before. He quickly settled down and started to circulate his mana through Yoga.

Guy was so thoroughly absorbed with himself at this point that he didn't notice Marie undergoing a profound change as well. If observed carefully, Guy would have realised that it was something similar to what Markus had gone through.

Marie's mind was rapidly beginning to churn through functions as she carefully devised the algorithm that could interpret the waves of fate. Applying the Fast Fourier Transform was the answer to her problems, she could feel that she was close!

A large whirlwind of mana started to form above the Teacher and Student, the former being markedly larger than the latter. Guy's core expanded as more and more mana started to circulate and nourish it, as well as his bones and internal organs. This was Guy stepping into the Internal stage of the Foundation Establishment realm. Just like before, a large quantity of pungent waste was emitted through Guy's pores in the form of a black vapour. This time, though, his clothes hadn't disintegrated as Guy had the presence of mind to quickly dissipate the heat while forming a protective layer around himself. He was NOT planning to become nude again!

Guy's cultivation advancement did not show signs of slowing down though. This was because although Marie had safely stepped into the Late stage of Mana Condensation realm, her core was still undergoing a profound change.

At this point, Marie closed her eyes and observed her environment using her mana sense. She quickly tuned to the channel that corresponded to the waves of fate.

She reached into her pockets and retrieved a copper coin. The copper denominations circulating in the Solar Empire had an etching of the symbol of the Emperor's clan, a sun with undulating rays, on one face and the seal of the authority that minted it on the other. When tossing a coin, the sun would represent heads and the minting authority's seal would be the tail's side.

Marie quickly tossed the coin high up into the air and projected her question.

"Will the copper coin land heads or tails?"

The waves of fate emitted from her and a response was received. Marie directed the waves through her open palms and directed it through her mana channels into her brains. Her mind was prepared with the Fast Fourier Transform function loaded in place. Right as the waves interacted with the abstract function in her mind, a profound reaction took place that confused even Krish who was diligently observing the situation from his cottage.

Marie immediately obtained a spectral analysis of the wave and siphoned it through a filter in her mind that isolated the frequency with the greatest amplitude, ejecting the rest from her body.

The isolated wave was received, and unlike before, Marie wasn't assaulted by white noise but an actual vision that, though short, clearly showed her copper coin landing heads up.

A minor explosion took place within Marie's core as she underwent perfected resonance. Her advancement started to teeter off, stopping just short of breaking into Foundation Establishment, exactly like Markus.

Lo and behold, the moment Marie reverted to her regular vision, the copper coin descended. It bounced on the ground three times, before rolling on its sides and spinning out with the sun facing upwards.

Marie did not pause and repeated the exercise again. And again. And again! She did it fifty times in total.

With each repetition, Marie's advancement started to slow down, and so did Guy's, until both their cultivation settled to a stable halt.

"I did it, Teacher Larks!" Marie beamed with a striking smile. She rushed forward and hugged Guy tightly. However, her grasp started to slowly slip as fatigue attacked her. Evidently, utilising this ability to divine the outcome of a coin toss fifty times had exhausted her.

Guy shook his head in defeat and quickly levitated Marie away from him to avoid getting the stink of his ejected waste on her.

He looked around cautiously before sneaking back into the orphanage.

"I hope Grace isn't home. If she sees you unconscious again..." Guy stroked his neck and dabbed the sweat forming on his forehead, as his mind conjured a frightening image of Grace choking him feverously.

"Nope! Can't have that..."

____

A/N: DTFT and DFT are two different things. I am not going to exhaust you with this as it isn't really pertinent to the story. But I urge you to look up videos on YouTube if you are interested.

____

**Next**
* [[volume-2.chapter-146]]