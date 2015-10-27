---
layout: post
title: The Rules of the Game
permalink: the-rules-of-the-game
tags: ai,rules,grammer
---

Here's an idea.

Let's say you have lots of games of chess written in traditional notation. Now simulate those games and pipe the pixels into an [unsupervised] AI. The goal is not to learn how to beat the best players but just to learn how to play the game well enough to explain it to another.

In a way, this is like a child watching people play and figuring out all the rules. This is harder for a number of reasons. Two of which are that you have just the board and no language communication. And that's assuming that we've previously taught the AI what are squares, pieces, and the notion of a 1-1 game.

How would you do this? Has it been done? There is a famous [paper](https://www.cs.toronto.edu/~vmnih/docs/dqn.pdf) by the team at DeepMind exploring how to train computers to beat Atari games. For each game, the AI was given video pixel input, the controls, and the rewards / losses (no different than the input and output humans get). With no human-assisted feature selection at all, the system did astonighsly well and surpassed a human expert for three out of the seven games played.

What I'm prorposing is different. With the Atari games, the machine didn't have to explain to someone else what it was doing. It just needed to get a better score. Another way to put it is that it couldn't now go and teach another how to play any of these games. How could it? It doesn't even have the means to communicate except through play.

Returning to chess, what is a way to test if someone understands the rules? If it was a kid, we'd ask her to explain how pieces move, how the game is set up, and what ends the game. Once we'd established that, we'd play a game or two to see if she understood everything in combination.

Is it sufficient to just play the game? No. That's no different than writing a textbook by combining words in a correct grammatical structure. It's very hard to train machines to do that correctly but we're getting very good today at teaching computers to pull this off without them being able to teach the rules of grammar[^n].

There are tests that seem good but break down. One is to leave out some possibilities for pieces and see if the computer picks it up. Say it never sees a rook moving from its original space to the opposite end of its base. How would it know that it could do that? It would be strange to think that it couldn't if it understood that rooks move in a horizontal fashion and can go up to any number of spaces. And yet it would be strange to think it *could* do that given that it has no probabilistic basis for that move. It would be similar to saying that the rook can jump over another one. It happens often enough with another piece (the knight) that that might be in the space of possibilities.

This might be too unnatural and hard. We as humans don't even do this - we use language to help guide how we learn games. DeepMind gave the equivalent of the piece movements to the AI (the controls). We could do that. We could also describe the space of moves - horizontal / vertical / diagonal / L-shaped.

I'd be much more interested in imbuing a system with some faculties for asking questions. Then, when it found out a rook could move five spaces horizontally, it could ask us, "can this piece move six spaces as well?"

That would be interesting.

[^n]: This is a non-trivial requirement on the machine. K-12 schools in America don't even require that students know grammar well enough to teach it to others. They just need to pass tests that determine they grasp the faculties. These tests are often times multiple choice but also written. Training a computer to perform well on that is much easier than what I'm suggesting.