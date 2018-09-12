---
author: admin
categories:
- Learning Machine Learning
comments: true
date: 2017-05-28T11:03:39Z
layouts: post
link: http://chiwbaka.com/generating-jules-verne-novels-torch-rnn/
slug: generating-jules-verne-novels-torch-rnn
title: Generating Jules Verne novels with Torch-RNN
wordpress_id: 134
---

Torch-RNN is a rewrite of Andrej Karpathy's char-rnn. You train it on some text, and then it can generate 'similar' text. I loved reading Jules Verne novels, so being able to just crank out some new novels whenever I feel like it sounds like a great idea right?

The hardest part was setting the environment up.

The text preprocessor was written in Python 2, and you'd think "hey it's just wrangling some text what requirements does it need". And it pulled in Cython, which numpy requires. Compiling Cython is always a bitch. I hate wading through compile scripts that I didn't write myself that break.

The NN model itself is written in Lua and needs something called LuaJIT, which sounds like a faster variant of a Lua interpreter. Whatever, not interested in learning the language. Setting that up required a lot of compiling too.

In the end I managed to get everything setup, and realized that I couldn't run the neural network on my GPU because everybody only writes for CUDA (thanks guys) and I have a Radeon HD 6870 (note: OpenCL won't work out of the box with the Radeon Crimson 16.x beta drivers, the last ones to be released for Barts. You need Catalyst 15.7.1 WHQL for proper OpenCL 1.2 support).

Anyway. I took From the Earth to the Moon, Eight Hundred Leagues on the Amazon (I wanna read that!), and. The Secret of the Island (I just found out that this was written by someone else, and only translated by Verne!) and put them all in one huge  text file.

Training the neural network took all of my CPU. Since I was running in the Bash shell for Windows 10 there was no way I could've gotten it to run on the GPU anyway.

After a day or two of training (and about 20K iterations) the virtual Jules Verne spat this out:





<blockquote>& thon tole, by seemed profufess and metal stations requirements of Judge Ribeiro, which degrees.
They have been descended some caber.

“Imlet in “the struggled a pressure, we must attracted by Recthman. That is more principants of the amazing,
nothing the course were so
craw of the same peculiable perpetibilitions of the
life of Judge Jarriquez frave to given
arrive
to violence.  On the forests, with the _Tapperto_”_

The companion, and
it like a close stretter had the traveler considerable than to the earth where the
quality.

On thick the Gun Club; what they disarsed of the idea of twenty-keeping them.  But to this day as to me, fix _“jussiba!”

“Aboats of do.
During the darkment is recovertaken a despaited that public Street!"

The long poblen the: dembnoit at langual paralle _ther of the Amazon?"

"What a compressed to Project Gutenberg-textected to this apparent for hourbascure
was no doubt, when he was with in its finished, “there would return
with all, or rather journey.

The step of the document seen to ask supering and have been liness; it would true. Not
the diamobas writable intomarier of
great a previous; after less mean them.

And which simple with more than the villal
work of the projectile would have indeed the
topped
the moon?
Work of the pounds
to proceed, at
them in at over Joam Dacosta dadled for
the refund of Sateltences comply up the certain soon in the right of nigmon.
The
Sound the projectile, and without retrew of the best
conquernts misceldt.

He as reply, the loud, the two gran!  Unifer--“it is inches that the mass approach
it, and we branches--that I cabinged that a
comes ank, low.
They reprisonation of the metal plantant mashed
which his destity proper profession of a large feeting his none-wall of the gas, free cupiness, through
frittle for the
jokes Donselver, putting scars of carrying an edgars a fear one of the
Rodroats
as soverlocks at the Chaboy, you, she not have the syriy the coupo</blockquote>



Clearly it seems I should've just removed the Project Gutenberg prefaces from the training text.

Anyway it's doing kinda well for a neural network that doesn't understand English, and besides, doesn't even understand the concepts behind the words.
