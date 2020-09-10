---
layout: post
title: Game Theory & Dynamics
permalink: game-theory-compilation
tags: research
---

# What is this?

Myself, along with Roberta Raileanu, Carles Domingo, and Samy Jelassi, documented
a chunk of recent and older literature in game theory meets machine learning wrt dynamics and decomposition.
The motivation for this was the Alpha-Rank and Open-Ended Learning papers from DeepMind, large expositions that suggested there were deeper wells underlying the field that we should explore.
We have tried to be strategic about what we read with an eye towards uncovering research directions ripe for exploring today, especially alongside today’s stronger function approximators.
However, there was a lot to cover, and you might very well look below and say not enough.

**NOTE**: Papers with an R have been read but not yet added here, and papers with a U have not yet been read but have been added for relevance. If you would like to contribute those and have your name added to this doc, please contact me.

## Overarching Questions / Research Ideas

1. The dominant model in the dynamics literature is the Ising model from physics, which uses the Fermi (logistic) distribution as the underlying probability of whether agent A adapts agent B’s strategy. Everyone who used this approach, including Alpha-Rank, modulo their motivation for the sole parameter, take this as a given following Blume’s [Statistical Mechanics of Strategic Interactions](https://docs.google.com/document/d/1k86ZeTtAL7Rs6VEbLYzk9tlRYuJ6w3roC53OJ8kZdZ4/edit#). This is unfortunate because Blume himself concluded by saying that "... it seems likely that the behavior of [these processes] is very sensitive to the [model used]. Changing this will lead to different kinds of infinite particle systems. This is good news since the stochastic Ising models and their generalizations that have driven much of the analysis here are hard to work with …"
  - What happens when we **use a different model from the Fermi distribution**? It appears that any Random Utility Model (see [below](#openaccesstexts)) will suffice for the assumptions. It might very well be the case that nothing of significance changes and this is why this isn’t mentioned in the field, but that is dubious.
  - For example, can we **incorporate some prior or model into the setup s.t it coincides with other solution concepts** such as Pareto-optimal? Blume showed that the stationary distributions in the limit of B[eta] → inf does not necessarily reach a Pareto-optimal setup.
  - [Traulsen 06](#traulsen06) comments that “... for games exhibiting a mixed equilibrium, the fingerprint of the [mixed equilibrium] reflects itself into the rate of fixation of one of the absorbing states… even when fixation is certain, the time to reach it may be arbitrarily long.” **Will another model expedite that time and/or bound it**?
  
2. Blume works in a model of agents on a 2d space. One of his assumptions is *Bounded Rationality* - only the instantaneous rewards are considered and not the expected NPV. He considers this the right assumption because he is working in a world with local interactions that determine global interactions. This follows from the agent not knowing information about their neighbors’ neighbors (or some # of neighbors out), and so by consequence the dynamics of learning an NPV is exceptionally difficult. This is intuitively described in [Claussen](#claussen08): “Life typically is organized as a covering of the surface of earth, in competition for sunlight, solid ground, or hunting territory. So it is natural to investigate evolutionary dynamics of individuals located in a two-dimensional space, where collective phenomena can emerge.”
    1. What if we **relax this constraint and allow for 2nd order effects or bayesian approaches wrt strategy** in other parts of the `world'? To some extent, humans are doing this and it seems natural for agents to consider it as well. This also applies to the cyclic models described in [Claussen](#claussen08).
3. There is an implication behind Lock-In ([Blume](#blume92)) and the Small-Mutation Limit ([Fudenberg](fudenberg04)) that when agents (which are species) are learning, all other agents have been held constant. In Lock-In, this follows from the poisson clocks being misaligned; In Small-Mutation Limit, this follows from there being only one mutant at a time. Can we utilize this more effectively towards **addressing the problem of non-Markovian learning in multi-agent**?
4. The obvious question is what happens when you use Alpha-Rank during training. Can you guide the models towards a better strategy by **incorporating Alpha-Rank into the pipeline** to choose the agents and/or to have the agents play against each other. A natural comparison would be against MaxEnt Nash.
5. **Are there certain points in the alpha sweep that are closer to Nash or Pareto optimality**? Is there something deterministic about that?

## Dynamics and Game Theory:

1. <a name="blume92">Statistical</a> [Mechanics of Strategic Interactions](http://www.dklevine.com/archive/refs4488.pdf)
    1. **Setting**: Blume - 1992
    2. **Questions**:
        1. Given the concluding thought, what happens when we use other models besides the Ising one (it seems like any RUM will be viable)? What happens when we change the payoffs, e.g. basing them off of more than just the instantaneous configuration?
        2. What happens if we let the player choose a mixed strategy instead of forcing them to choose only amongst pure strategies? See the comment on xiii.
        3. The motivation behind Lock-in is similar to the small-mutation limit assumption and the implication is that agents (which are species) are not learning in the vicinity of other learning agents but instead we have held those constant. How can we utilize this more effectively in “multi”-agent learning?
        4. Can we utilize Candogan’s Decomposition along with the non-changing game dynamics in in xviii to better address some class of games? (This is hazy.)
        5. How can we compel the dynamics to drive towards Pareto-optimal equilibria? This is not guaranteed.
    3. **Succeeds**:
        1. [Random Orderings](#random-orderings)
        2. [Random Utility Models](#openaccesstexts)
    4. **Summary**: This is the main paper upon which the entire field started using the Fermi Distribution as its go to model. It is absolutely packed with lots of interesting directions, commentary, and investigations.
    5. **Further Details**:
        1. Interested in the phenomnenon of decentralization through local competition. “A trivial example is the problem of determining which side of the road we drive on. Each driver will interact with only a small fraction of the set of all drivers and most often with those who live or work nearest him. Nonetheless a convention is established.”
        2. At large cares about dynamic models of strategic interaction in a population where direction interaction is local but indirection interaction is global. One view of this work is that it describes a population of players, each of whom adapts to teh environment in which eh plays. Reflexive: “Players adapt to the environment, which in turn adapts to them.”
        3. Similar recent work has largely “traded off rationality of the individual for the emergence of collective order in the population.” … “One looks for the emergence of Nash-like play in the aggregate rather than at the level of the individual player.”
        4. Assumptions:
            1. Players do not instantaneously react to their environment. Having made a decision, there is a lock-in period.
            2. Players are myopic in their decision making. They respond to instantaneous reward rather than to an NPV.
        5. Two Strategy-Revision Processes considered:
            1. *Best-response dynamics*: Each player maximizes instantaneous payoff flow at each revision opportunity.
            2. *Stochastic-choice dynamics*: At each revision opportunity, players choose strategies stochastically dependent upon the payoffs of each value.
        6. Focuses on stochastic-choice dynamics, where he uses the Ising model for log-linear strategy revision process.
        7. Main results address the question of which Nash equilibria emerge in the asymptotic behavior of stochastic strategy revision processes, which are small perturbations of best-responses processes.
        8. Model:
            1. 2^d lattice where every site is a player.
            2. Neighborhood of a site is finite.
            3. Instantaneous payoff flow from palying strategy w is sum of instantaneous payoffs received from playing w against each of the neighbors.
            4. Strategy revision process is that every player has iid poisson clock w/ mean 1 and the player can change their strategy only when it goes off.
        9. Best-response revision: players choose equiprobably from among strategies that give the highest payoff flow given current neighbor configuration.
        10. Stochastic-choice revision:
            1. There are two sources of random choice behavior - mistakes and unpredictable experimentation.
            2. Say {p_s^br} are the best response probabilities and {q_s} is another system of selection probabilities where {q_s} is a completely mixed distribution on the strategies w.
            3. Then p_s(w|phi) = B/(1+B)*p_s^br(w|phi) + 1/(1+B)q_s(w|phi) is a perturbation of best-response and using it as the stochastic-choice rule is called a *Perturbed Best-Response Rule.*
            4. Further, assume that q_s is shift-invariant and that for all s, w, phi, q_s(w|phi) > 0.
        11. Motivation for using the familiar Fermi distribution comes from here:
            1. “Another source of stochastic choice is as the outcome of a rational stochastic-choice theory. One such theory is the Random Utility Model - the return to any action is random and varies across each player’s choice opportunities.”
            2. An important RUM is the log-linear model, where the log odds of the player at site s choosing strategy v to strategy w are proportional to some function of the associated payoffs.
            3. *We compute those payoffs for the current configuration*.
            4. log(p_s(v|phi)/p_s(w|phi)) = B * \sum_{t \in V_s} G(v, phi(t)) - G(w, phi(t)) leads to p_s(v|phi) = (\sum_{w \in W} exp(B \sum_{t \in V_s} G(**w**, phi(t)) - G(**v**, phi(t))))^(-1)
            5. The constant B parameterized the sensitivity of choice. When B=0, the distribution puts equal weight on all choices. When B → inf, the distribution converges to one that puts equal weight on all best responses and 0 otherwise. Thus, best-response revision arises as a limiting case of the stochastic choice model.
            6. These stochastic choice models originated in Thurstone’s study of comparative judgement and characterization of these models as RUMs was first established in [[Random Orderings]](random-orderings).
        12. Problem: The class of perturbed Best-Response Rules does not contain the above log-linear rules. This is because as B → inf, the additive perturbation {q_s} changes.
        13. Note that with both best-response and stochastic choice rules, the objects of choice are pure strategies, not mixed strategies! The player must choose a pure strategy and not a distribution, and when he observes neighbors, he sees pure strategy choices. Even so, the players confront distributions of actions because a typical moment in time, the configuration of choices will be polymorphic in that many choices will be represented in the population. The players care about the distribution of choice in his neighborhood, which may very well be mixed.
        14. Both BR and SC have:
            1. *Lock-In*: Players are committed to their choices for some period determined by the alarm clock. This could feasibly also be achieved with decision costs. The importance of this is that it’s unlikely more than one player is changing strategies.
            2. *Bounded Rationality*: Only the present rewards to each choice are considered, and not the expected NPV. Full rationality would entail forming beliefs about future play of neighbors and maximizing the NPV.
                1. Claim: For Full Rationality to be Markovian, it requires that information before t to be irrelevant at t.
                2. Me: Why can’t we include history as part of the state?
                3. His answer is that that is unrealistic because you don’t know information about your neighbors’ neighbors (or some # of neighbors out), thus the dynamics of learning is immediately hard to understand. You can only get this through interactions with you rneighbors.
                4. Me: What if you could talk with them though?
                5. Him: Markov play arises when each player’s discount factor is small relative to mean waiting time between poisson alarm rings. Here, future play is irrelevant and full rationality **is** bounded rationality → maximize instantaneous payoff flow, i.e. simple best response.
        15. Goal: Relate stationary distributions of strategy revision processes to equilibrium concepts for noncooperative games.
        16. One result that is immediately clear is that stationary distributions for BR dynamics places all mass on iteratively undominated strategies.
        17. Another result that is less clear is that if a game G has the unique best-response property, then its current distribution is a stationary distribution iff <expression om page 399> = 0. Furhter, for any SC and for all epsilon > 0, there is a b s.t. For B > b, that <expression> is at most epsilon from 0. This implies that if G has the UBR property, then we are ~ at an epsilon-Nash.
        18. The selection probabilities for a given game matrix G are unchanged if the matrix is perturbed s.t. The difference between any two elements in the same column are unchanged.
        19. For a given G, with a space of two strategies, we can convert the SC rule approach into a log-linear choice model. See p. 400 for the corresponding flip probabilities.
        20. *Gibbs State*: Equilibrium concept extensively employed in physics. Here, it’s a stationary distribution for the stochastic strategy revision process with a special property → The spatial distribution of equilibrium play is given by the system of conditional probabilities {p_s}: u is Gibbs state for {p_s} iff u(n(s) = w | n(-s) = phi(-s)) = p_s(w|phi). Five properties for 2-strategy games:
            1. Gibbs States exist.
            2. They are stationary distributions.
            3. All translation-invariant stationary distributions are Gibbs states.
            4. If lattice dimension is {1, 2}, every stationary distribution is a Gibbs state.
            5. Set of Gibbs states varies hemi-continuously with B.
        21. When B → inf, probability of choice given configuration of all other players is highest for best response choice. This converges to palying Best Response process and leads to a connection b/w Gibbs states and Nash.
            1. If distribution u on X is weak limit of sequence of Gibbs states {u_B} as B → infinity for a log-linear strategy revision process with matrix G, then u is a stationary Nash distro for best-response strategy revision.
        22. See section 5.3 for examples of Gibbs states and asymptotics in games.
        23. “With stochastic-choice revision, coordination emerges as limit behavior of population over time given high enough B”.
            1. Note that B → inf does not imply that the selected equilibrium is always Pareto-optimal.
        24. Uses coupling to show that perturbed best-response has similar asymptotic-B results to log-linear strategy revisions.
        25. Difficulties arise when there are more than two strateiges. The Gibbs state equation doesn’t hold because it isn’t true that arbitrarily specified collection of conditional probability distribtuions derive from a common joint distribution. The Gibbs state distros may not even exist when > 2 strategies.
        26. <There are a lot more results here that build on each other>
        27. Connection to potential games:
            1. Matrices G and G’ are strongly best-response equivalent if there exist numbers a > 0 and B_j s.t. G’(i, j) = aG(i, j) + B_j.
            2. The matrix G has a potential if G is strongly best-response equivalent to a matrix G’ = B + A, where B is symmetric with 0 column sums, A has column sums equal to 0, and all elements of any row of A are equal. All symmetric 2x2 games have potentials, but most symmetric NxN games do not.
            3. The log-linear revision process w parameter B has a Gibbs state u iff payoff matrix G has a potential.
        28. One of the concluding thoughts: “I have assumed that the value of an alternative to any player is the sum of the values of the simultaneous interactions with neighbors. The matching models in EGT do not work this way; there, the value of an alternative depends upon the current match. It seems likely that the behavior of strategy revision processes is very sensitive to the interaction technology. Changing this will lead to different kinds of infinite particle systems. This is good news since the stochastic Ising models and their generalizations that have driven much of the analysis here are hard to work with. Other appealing matching technologies lead to percolation processes and processes akin to the voter model, which are easier to grasp...”
2. <a name="fudenberg04">Imitation</a> [Processes with Small Mutations](https://www.sciencedirect.com/science/article/pii/S0022053105001080)
    1. **Setting**: Fudenberg, Imhof - 2004
    2. **Questions**:
        1. What happens when you introduce the Mixed Nash as a strategy option?
    3. **Succeeds**:
        1. 15: Emergence of … (Moran Process)
    4. **Summary**: This paper characterize the limit, as the mutation rate goes to zero, of the invariant distributions of the perturbed chains, where the chains have transition matrices D s.t. D_ij = probability that in a population of i-players a single j-mutant occurs times the probability that this mutant takes over the whole population so that only j-players remain. They show that they only need to do this through linear means (prior work required more machinery).
    5. **Further Details**:
        1. In our model, it is impossible for agents to adopt a strategy that is not currently in use, so that any “homogenous” state where all agents use the same strategy is absorbing.
        2. Moreover, all of the interior states are transient so that the dynamics converge to one of the homogenous states.
        3. Some of the homogenous states are “more persistent” than others in that they are not all equally robust to mutation or experimentation. We can formalize this by assuming a small mutation term that makes the system ergodic and then analyzing the limit as that term → 0.
        4. The resulting system spends almost all of its time at the absorbing states of the underlying no-mutation process.
        5. Under our conditions, the details of the dynamics at strictly interiro points have no additional impact on the limit distribution. That is, any two processes that meet our condtiions and also behave the same way along “edges” where only two strategies are present must lead to the same limit distribution.
        6. Assumptions:
            1. If at any time a strategy is not played, it will remain absent, implying that for each i the homogenous state in which all players use strategy i is absorbing.
            2. All states in S \ {s_1, …, s_K} are transient where states in S represent the number of agents playing each strategy, i.e. at any state where at least two strategies are being used, there is a positive probability of transitioning to a state where the number of agents using hte most popular strategy is increased.
            3. Let s_i/j denote ~ pure state where all players use i except for one, which uses j. Then, for i in [1, K], lim_{eps → 0} p_eps(s_i, s_i/j)/eps = u_ij >= 0 for i != j
            4. For i in [1, K], lim_{eps → 0} p_eps(s_i, s)/eps = 0 for all s in S \ {s_i, s_i/1, …, s_i/i-1, s_i/i+1, …, s_i/K}
            5. Matrix D[elta] is s.t. D_ij = probability that in a population of i-players a single j-mutant occurs times the probability that this mutant takes over the whole population so that only j-players remain: There is a unique k-vector lambda s.t. lambda is an eigenvector for D, <lambda, 1> = 1, and lambda >= 0.
                1. Simpler version: For all pairs of different strategies i, j: u_ij > 0 and p_ij > 0.
        7. Goal: Characterize the limit, as the mutation rate goes to zero, of the invariant distributions of the perturbed chains, where the chains have transition matrices D.
            1. Theorem 1 says that this limit distribution is the unique invariant distribution of Assumption 5 and is thus solvable via system of K linear inequalities.
        8. They look at the frequency-dependent Moran process from Fudenberg 2004 / Nowak 2004 where absorption probability p_ji = (1 + \sum_{k=1 → N-1} \prod_{l=1 → k} g_l(i, j) / f_l(i, j))^(-1) and find that the resulting chain converges to vastly different numbers depending on mutation chance for each strategy (“mutations correspond to mistakes and players of strategy 2 are less likely to make mistakes”) and to number of players.
        9. There is a bunch more in this paper where they relax some restrictions in the beginning sections.
3. [Stochastic][#traulsen06] [Dynamics of Invasion and Fixation](http://atpgroup.org/jmp/PDFs/061.pdf):
    1. **Setting**: Traulsen, Nowak, Pacheco - 2006
    2. **Questions**:
        1. Everything in this paper utilizes the Fermi Distribution. What if we changed that to another model? The only impetus for using it was that it satisfied having a quality of evolutionary selection that was missing in the recent literature.
        2. The paper ends by saying “... for games exhibiting a mixed equilibrium, the fingerprint of the [mixed equilibirum] reflects itself into the rate of fixation of *one* of the absorbing states… even when fixation is certain, the time to reach it may become arbitrarily long.” Will another model expedite that time and make it bounded?
    3. **Succeeds**:
        1. 15: Emergence of Cooperation ...
        2. 13: Statistical Mechanics …
        3. 12: Game Theory and Physics
    4. **Summary**: This paper introduces the *Pairwise Comparison* evolutionary process to account for the difference in behavior between populations that are finite and those that are infinite. The main idea is that in a finite population, the only stable evolutionary outcome corresponds to all individuals sharing a common trait and the probability of fixation is a nontrivial function of the initial conditions and the intensity of selection.
    5. **Further Details**:
        1. Deterministic Replicator Dynamacs - in absence of mutations, and when populations are infinite, homogenous, and well-mixed, so that there is an equal chance of interaction among the individuals, the mean-field description of a strategy changes according to the relative payoff, i.e. dx/dt = x * (pi_x - <pi>).
        2. Introduces a temperature of selection to evolutionary game dynamics of finite populations that controls the balance between selection and random drift in the population. Controlling this temperature adjusts for many kinds of cultural dynamics.
        3. Introduces the *Pairwise Comparison* evolutionary process, which uses Fermi distribution and provides a closed analytical expression for the fixation probability.
        4. Consider the social dilemma games defined by (C, D) x (C, D), with CxC=R, CxD=S, DxC=T, DxD=P. Then Prisoner’s Dilemma (PD) is T>R>P>S, Snowdrift (SD) is T>R>S>P, and Staghunt (SH) is R>T>P>S.
        5. Under Replicator Dynamics:
            1. Only stable equilibrium is Defect for PD.
            2. There is a stable coexistence of cooperators and defectors at cooperator frequency x* = (S-P)/(T+S-R-P).
            3. There is an unstable interior fixed point at x*. Frequencies above (below) this point evolve toward 100% (0%) cooperation.
        6. What is the impetus for developing the new Pairwise Comparator approach?
            1. The normal process is that two individuals are chosen randomly for the population with payoffs pi_i1, pi_i2.
            2. Under imitation dynamics, the fitter replaces the less with p=1.
            3. Under replicator dynamics, selection also always favors the fitter.
            4. But that’s not how evolutionary changes actually happen!
            5. This can have decisive effects in small populations.
        7. To address this, do *Pairwise Comparison*: set the probability of having i1 replace i2 as p_{pi_p1, pi_p2} = (1 + exp(-B(pi_p1 - pi_p2)))^(-1).
            1. The inverse temperature B >= 0 controls the intensity of selection.
            2. As B → inf, this becomes imitation dynamics. For B << 1, this reduces exactly to the frequency-dependent Moran process (See 14: Emergence of Cooperation)
        8. This defines a finite state Markov Process w/ Pr_j{j +- 1} being the probability of transitioning from j cooperators to j +- 1 cooperators. This probability is j/N * (N-j)/N * p_{pi_C, pi_D} for j+1 and we reverse the pi_C, pi_D for j-1.
        9. Can approximate this with a stochastic diff eq, yielding dx/dt = x(1-x)tanh(B/2 * (pi_C - pi_D)) + sqrt(x*(1-x)*w/N) where w is Gaussian white noise with variance 1 and x = i/N is the fraction of cooperators.
        10. The remaining part of the paper exhibit results that are dependent on the Fermi process. The most interesting is with SD:
            1. Replicator dynmiacs predicts that any intiial condition will lead to the mixed equilibrium.
            2. This is not possible in a finite population where the system will end up in one of the absorbing states (0 or N cooperators).
            3. The approach described here highlights this and addresses it wrt the resulting fixation probabilities.
4. U [Statistical Mechanics of Best Response Strategy Revision](https://pdfs.semanticscholar.org/16d1/21c0491e71d8cbc7542b07bfb6a15d181363.pdf)
5. The Mechanics of n-Player Differentiable Games:
    1. **Setting**: Balduzzi et al. - 2018
    2. **Questions**:
        1. The paper largely ignores previous developments. It does cite papers from the RL community and from the game theory community, but it lacks specific references to optimization papers. It would be good to find a clear link with previous work (see also Questions in the Stable Opponent Shaping...).
        2. On the one hand, the paper proposes a set of five desiderata that an update should satisfy for an n-player zero sum game. On the other hand, it proposes an update called SGA that is shown to satisfy the five desiderata (see Further Details below). It seems as if the desiderata were chosen such that SGA fulfills them, because they do not imply any type of convergence and can be seen as “arbitrary”. Proving local or global results of convergence of SGA would be a possible direction in the theory side.
    3. **Succeeds**:
        1. Facchinei and Kanzow (Annals of OR, 2010): Hessian stability is discussed extensively in Section 5 (although this paper is not cited)
        2. The other papers cited are not really in the same setting as this one.
    4. **Summary**: This paper serves as an introduction to n-player differentiable games: it defines solution concepts (local Nash equilibrium, stable fixed point), potential and Hamiltonian games, it states the decomposition of the Hessian. Successful updates are provided for the particular cases of potential and Hamiltonian games. A set of five desiderata are proposed for updates in the general case. Symplectic gradient adjustment (SGA) is defined and shown to fulfill the five desiderata.
    5. **Further Details**:
        1. We deal with games with $n$ players, which we refer to as integers between 1 and $n$. Each player $i$ controls parameters $\mathbf{w}_i \in \mathbb{R}^{d_i}$, such that we can express all the parameters of the game as $\mathbf{w} = (\mathbf{w}_1,\mathbf{w}_2,...\mathbf{w}_n) \in \mathbf{R}^d$. Here, $d = \sum_{i=1}^n d_i$. Each player has a loss function $l_i : \mathbb{R}^d \to \mathbb{R}$ which has as input the vector of parameters $\mathbf{w}$. As in regular game theory, the player seeks to achieve the minimum value of $l_i$ possible but is only able to control $\mathbf{w}_i$. We will write $\mathbf{w} = (\mathbf{w}_i,\mathbf{w}_{-i})$, where $\mathbf{w}_{-i}$ refers to all the parameters different from $\mathbf{w}_i$.
        2. We define the simultaneous gradient $\xi$ as the concatenation of the gradients of losses l_i with respect to parameters w_i, and the Hessian of the game $H$ as the Jacobian of the simultaneous gradient.
        3. A pure Nash equilibrium is a vector $\mathbf{w}^{\star}$ such that for all $i$ and all $\mathbf{w}_i' \in \mathbb{R}^{d_i}$, $l_i(\mathbf{w}^{\star}_i,\mathbf{w}^{\star}_{-i}) \leq l_i(\mathbf{w}_i',\mathbf{w}^{\star}_{-i})$.
        4. $\mathbf{w}^{\star}$ is a local pure Nash equilibrium if for all $i$ there exists a neighborhood $U_i$ of $\mathbf{w}^{\star}_i$ such that for all $\mathbf{w}_i' \in U_i$, $l_i(\mathbf{w}^{\star}_i,\mathbf{w}^{\star}_{-i}) \leq l_i(\mathbf{w}_i',\mathbf{w}^{\star}_{-i})$.
        5. $\hat{\mathbf{w}}$ is a fixed point if $\xi(\hat{\mathbf{w}}) = 0$. A fixed point $\hat{\mathbf{w}}$ is stable if $H(\hat{\mathbf{w}})$ is positive semidefinite.
        6. Potential games are those for which the Hessian $H$ is symmetric at all points. Equivalently, a game is potential if there exists a function $\phi: \mathbb{R}^n \to \mathbb{R}$ such that for all $i$, $w_i'$, $w_i''$, $w_{-i}$, $\phi(w_i',w_{-i}) - \phi(w_i'',w_{-i}) = l_i(w_i',w_{-i}) - l_i(w_i'',w_{-i})$. Simultaneous gradient descent dynamics for potential games is actually gradient descent on the potential $\phi$ and local minima of $\phi$ are stable fixed points.
        7. Hamiltonian games are those for which the Hessian is antisymmetric. For Hamiltonian games, we can define a Hamiltonian function $\mathcal{H}(\mathbf{w})=\frac{1}{2}||\xi(\mathbf{w})||^2$. Simultaneous gradient descent dynamics preserve the Hamiltonian function. And if $H(\mathbf{w})$ is invertible everywhere and $\lim_{||\mathbf{w}|| \to \infty} \mathcal{H}(\mathbf{w}) = \infty$, gradient descent on $\mathcal{H}$ converges to a Nash equilibrium.
        8. Let $\xi_{\lambda}$ be a certain update, that is, the discrete dynamics are be given by $\mathbf{w}^{k+1}=\mathbf{w}^k - \alpha \xi_{\lambda}(\mathbf{w}^{k})$. Five desiderata that "seem" reasonable to demand to $\xi_{\lambda}$ are proposed:
6. Compatible with game dynamics: $\langle \xi_{\lambda}, \xi \rangle = \alpha_1 ||\xi||^2$.
7. Compatible with potential dynamics. If the game is potential, then $\langle \xi_{\lambda}, \nabla \phi \rangle = \alpha_2 ||\nabla \phi||^2$
8. Compatible with Hamiltonian dynamics. If the game is Hamiltonian, then $\langle \xi_{\lambda}, \nabla \mathcal{H} \rangle = \alpha_3 ||\nabla \mathcal{H}||^2$
9. Attracted to stable fixed points. In neighborhoods where $S = \frac{H+H^T}{2} \succ 0$, $\theta(\xi_{\lambda},\nabla \mathcal{H}) \leq \theta(\xi,\nabla \mathcal{H})$.
10. Repelled by unstable fixed points. In neighborhoods where $S \prec 0$, $\theta(\xi_{\lambda},\nabla \mathcal{H}) \geq \theta(\xi,\nabla \mathcal{H})$

> where $\alpha_1, \alpha_2, \alpha_3 > 0$ and $\theta$ is the angle between vectors.

1. The symplectic gradient adjustment (SGA) is given by

> $\xi_{\lambda} = \xi + \lambda A^T \xi$ where $A = \frac{H-H^T}{2}$. With an appropriate choice of $\lambda$, SGA satisfies the five desiderata with $\alpha_1 = \alpha_2 = 1$ and $\alpha_3 = \lambda$. Specifically, we need to choose $\lambda$ such that $\text{sign}(\lambda) = \text{sign} \left(\langle \xi, \nabla \mathcal{H} \rangle \langle A^T \xi, \nabla \mathcal{H} \rangle\right)$

1. Stable Opponent Shaping in Differentiable Games:
    1. **Setting**: Letcher et al. - 2019
    2. **Questions**:
        1. As “Mechanics”, this paper ignores other previous developments on the subject. An interesting question would be how LookAhead, LOLA and SOS relate to extragradient algorithms. Can results on the convergence for extragradient algorithms shed light on convergence properties of LookAhead, LOLA and SOS?
        2. Another issue with this paper is the proof of local convergence for LookAhead (which also underlies the proof of local convergence for SOS). There are some doubts about the way they use first order sufficient conditions for convergence. It would be a good idea to go over that and fix it.
        3. LOLA and SOS outperform SGA (from the Mechanics paper) and LookAhead according to the experiments provided in this paper. However, they do not provide any kind of theoretical explanation why that should be the case. It would be worth to understand this in more detail.
    3. **Succeeds**:
        1. 5. The Mechanics of n-player Differentiable Games (Balduzzi et al. - 2018)
        2. Learning with Opponent-Learning Awareness (Foerster et al. - 2017)
    4. **Summary**: This paper proposes SOS and it shows that it is a stable alternative to LOLA, which is an update presented in Foerster et al. - 2017 that performs well experimentally.
    5. **Further Details**:
        1. In LOLA (Foerster et al., 2018), each player computes the gradient of its loss function supposing that all the other players have already moved. That is, the new objective function is $l_i(\mathbf{w}_1+\Delta \mathbf{w}_1,...,\mathbf{w}_i,...,\mathbf{w}_n+\Delta \mathbf{w}_n) \approx l_i(\mathbf{w}_1,...,\mathbf{w}_i,...,\mathbf{w}_n) + \sum_{j\neq i} (\nabla_j l_i(\mathbf{w}))^T\Delta \mathbf{w}_j$.
        2. Hence, the update for LOLA is $\nabla_i l_i - \alpha \left(\sum_{j\neq i} (\nabla_{ji} l_i(\mathbf{w}))^T\nabla_j l_j + (\nabla_{ji} l_j)^T \nabla_{j} l_i(\mathbf{w})\right)$.
        3. If we remove the second terms of LOLA, we get an update called LookAhead. The expression for LookAhead is $\nabla_i l_i - \alpha \left(\sum_{j\neq i} (\nabla_{ji} l_i(\mathbf{w}))^T\nabla_j l_j \right)$.
        4. The fixed points for the LookAhead dynamics are the same as the fixed points for the game (those points at which the simultaneous gradient is zero). However, the fixed points for LOLA are not the same as the fixed points for the game.
        5. Stable Opponent Shaping (SOS) is a modification of LOLA such that its fixed points are the same ones as the fixed points of the game. The SOS update is $\nabla_i l_i - \alpha \left(\sum_{j\neq i} (\nabla_{ji} l_i(\mathbf{w}))^T\nabla_j l_j + p(\mathbf{w}) (\nabla_{ji} l_j)^T \nabla_{j} l_i(\mathbf{w})\right)$. $p(\mathbf{w})$ is a factor with a specific expression that takes value 0 on the fixed points of the game.
        6. The main results claimed in this paper are:
2. LookAhead and SOS converge locally to stable fixed points for $\alpha$ small enough if their Hessian $H$ is invertible.
3. If SOS converges to $\hat{\mathbf{w}}$ and $\alpha > 0$ is small then $\hat{\mathbf{w}}$ is a fixed point of the game. That is, SOS actually preserves the fixed points of the game and hence is better than LOLA in that sense.
4. SOS (and LookAhead) locally avoid strict saddles almost surely, for $\alpha > 0$ sufficiently small.
5. <a name="claussen08">Discrete</a> [stochastic processes, replicator and Fokker-Planck equations of coevolutionary dynamics in finite and infinite populations](https://arxiv.org/abs/0803.2443)
    1. **Setting**: Claussen - 2008
    2. **Questions**:
        1. In the cycling games like Matching Pennies, is it beneficial to jump ahead of the cycling by predicting 2nd order effects and strategy changes?
    3. **Succeeds**:
        1. [Nowak 04](#nowak04)
        2. [Statistical Mechanics](#blume92)
        3. [Traulsen 05](#traulsen05)
        4. [Stochastic Dynamics](#traulsen06)
        5. [Imitation Dynamics](#sophisticated)
    4. **Summary**: This is a tutorial review about coevolutionary dynamics in finite populations.The main takeaway is Table 1 (reproduced below) comparing Moran to Local to Fermi processes.
    5. **Further Details**:
        1. This is a tutorial review formulating coevolutionary dynamics in finite populations within the EGT framework. There is a bunch of preliminary buildup before we get to EGT.
        2. Evolutionary Stable Strategy: A population in which a single mutant (changing to any of the possible strategies or genotypes) cannot invade the population. In infinite populations, these appear as stable fixed points. In finite populations, this is more complicated (references given).
        3. Motivation for using 2d space models: “Life typically is organized as a covering of the surface of earth, in competition for sunglight, solid ground, or hunting territory. So it is natural to investigate evolutionary dynamics of individuals located in a two-dimensioanl space, where collective phenomena can emerge.”
        4. Many processes considered, all in the Polya Urn mold:
            1. Fisher-Wright Process: At each time step, all individuals reproduce with probabilities proportional to their fitness, until the same population size N is reached. When all individuals have identical fitness, the discrete stochastic process describing the evolution resembles a random genetic drift.
            2. Discrete stochastic processes: Moran and Local update types fall into this category where there is a transition probability T+- that the number of individuals i (in the first strategy) increase or decrease by one.
            3. Moran Process: In original form, it is not frequency dependent. Here, in each time step an individual is selected for reproduction and subsequently a random selected individual dies. [Nowak 04](#nowak04) investigated a frequency-dependent Moran process where selection for reproduction is proportional to the payoff comapred to the average payoff. The probability per time step that a copy of an A agent is newborn then is (p+)*(i/N).
            4. Local Update / Imitation Process: One individual selected for reproduction, another for death; Strategy of individual is changed or kept with probabilities that depend linearly on the difference of the payoffs (global) of the two interacting individuals. This is from [Traulsen 05](#traulsen05) but is closely related to Pairwise Competition from [Stochastic Dynamics](#traulsen06) from Traulsen and [Imitation Dynamics](#sophisticated).
            5. Nonlinear Response: An example is the Local Fermi Process described in Blume. Briefly mentions the link here to Boltzmann factors and heatbaths.
            6. <img src="{{site.baseurl}}/assets/gametheory.png">
            7. Then descirbes how this all connects to the Fokker-Planck equation and Ito calculus with the resulting limits shown as described in the table above.
            8. Shows a cyclic NFG introduced by Dawkins, which was later simplified to Matching Pennies. Leaves with a cliffhanger about how the mdoel that leads to the cycling relies on several assumptions, but doesn’t *really* consider what happens as they change.
6. U <a name="traulsen05">Coevolutionary</a> [Dynamics: From Finite to Infinite Populations](https://arxiv.org/abs/cond-mat/0409655)
7. U <a name="sophisticated">Sophisticated</a> [Imitation in Cyclic Games](https://pdfs.semanticscholar.org/2603/e37cf5ca4d3b01d647cef39922a63a705c49.pdf)
8. U <a name="random-orderings">Random</a> [Orderings and Stochastic Theories of Responses](https://cowles.yale.edu/sites/default/files/files/pub/d00/d0066.pdf)
9. U [Evolutionary Dynamics of Multi-Agent Learning: A Survey](https://www.jair.org/index.php/jair/article/view/10952/26090)
10. U [An Evolutionary Dynamical Analysis of Multi-Agent Learning in Iterated Games](https://link.springer.com/content/pdf/10.1007%2Fs10458-005-3783-9.pdf)
11. U [Evolutionary game theory and multi-agent reinforcement learning](https://www.researchgate.net/publication/220254307_Evolutionary_game_theory_and_multi-agent_reinforcement_learning)
12. U [An evolutionary game theoretic perspective on learning in multi-agent systems](https://www.jstor.org/stable/20118420?seq=1#metadata_info_tab_contents)
13. U [What evolutionary game theory tells us about multiagent learning](https://www.sciencedirect.com/science/article/pii/S0004370207000082)
14. U [Theoretical Advantages of Lenient Learners: An Evolutionary Game Theoretic Perspective](http://www.jmlr.org/papers/volume9/panait08a/panait08a.pdf)

## Decomposing the Gamespace:

1. U Decomposition of Games: Some Strategic Considerations
2. R [Flows and Decompositions of Games](https://arxiv.org/abs/1005.2405)
3. U Decompositions and Potentials for Normal Form Games
4. U A Cooperative Value for Bayesian Games
5. [Real World Games Look Like Spinning Tops](https://arxiv.org/abs/2004.09468)
    1. **Setting:** Czarnecki et al. - 2020
    2. **Questions:**
        1. TODO
    3. **Succeeds:** Balduzzi et al. 2019
    4. **Summary:** They define a game class called “Games of Skill” that provides a structure to / decomposes any real world (zero-sum symmetric) game and is composed of two axis - transitive dimension- y-axis (skill level of strategies) and the non-transitive dimension- x-axis (cyclic strategies). High level visualization gives an image of a spinning top with the highly skilled agents appearing at the top (of y-axis). As the width of the top increases while traversing down the transitive dimension (medium-low skill strategies), the number of cyclic strategies increases (x-axis) for the same skill level. Further they provide a connection with population based methods, their necessity, and how this proposed structure can help in learning.
    5. **Further Details:**
        1. The authors take inspiration from advances in AI methods which exploit structural properties in different domains to perform better at a given task. How do we take advantage of this in real world games?
        2. Games of Skill: requires 2 main concepts to be fulfilled-
            1. Games should have a notion of progress (transitive skill component) not like RPS. Agent specific learning handles this.
            2. Availability of diverse game states. Eg: Different opening moves in (Go, Chess) make even similar skill players fail against each other (non transitive component). Multi-agent methods used to deal with this non-transitivity.
        3. Preliminaries: Convert all games to a finite normal-form game (even extensive form ones) and build the win/loss/draw payoff from the game payoff.
        4. Proposition 1: Every real world game can be shown to be n-bit communicative. I.e. a given player transmits n bits of information before the game reaches an outcome - 2^n such strategies exist. Go has n=1000.
        5. Proposition 2: Divide the proposed game geometry (along the transitive dimension) into layers. The set of strategies $$\prod$$ can be divided into k disjoint layers C_k with increasing skill level.
        6. Proposition 3: at each iteration, for a given $$\prod$$, replace any strategy in $$\prod$$ with a new one that defeats them all. This fixed size population fictitious play will converge if $$\|\prod\|$$ is at least the number of strategies present in the topmost (most skilled layer). It also shows that the more the complex multi-agent algorithm is (Alphastar, OpenAI Five), the more specific agent priors we need (i.e. need to cover more strategies from the top).
        7. Layers are intractable so use clustering. Iterative procedure: First find a Nash equilibrium over the set of pure strategies $$\prod$$ and form a cluster. Then take the remaining strategies and repeat the process. The clusters are created with decreasing skill level. Nash clustering also satisfies RPP identity for clusters the same way it does for strategies (Balduzzi et al. 2019)
            1. “In fully transitive games, Nash clustering creates one cluster per strategy, and in games that are fully cyclic (e.g. RPS) it creates a single cluster for the entire strategy set.”
        8. Theorem: A diverse population that spans an entire cluster guarantees transitive improvements. A strategy \pi that beats all strategies in a cluster C_i will be at a higher skill level. No need to check with any strategy in lower levels than C_i.

## Applications involving Dynamics in Game Theory:

1. <a name="nowak04">Emergence</a> [of Cooperation and Evolutionary Stability in Finite Populations](https://dash.harvard.edu/bitstream/handle/1/3196331/fudenberg_emergence.pdf)
    1. **Setting**: Nowak, Sasaki, Taylor, Fudenberg - 2004
    2. **Questions**:
        1. <Todo>
    3. **Succeeds**:
    4. **Summary**: They use the Moran process to show that “... in finite populations, natural selection can favor the invasion and replacement of AD by a reciprocal strategy when starting from a single individual using that strategy. No other mechanism is required.” Further analysis leads to conditions specifying whether a strategy is protected against invasion / replacement.
    5. **Further Details**:
        1. “The problem of cooperation is that defection is evolutionarily stable”. How do strategies like TFT arise in the first place?
        2. They show that a single cooperator using a reciprocal strategy can invade a population of defectors in an NFG game.
        3. Mentions that Deterministic Replicator Dynamics of inf populations admit an unstable equilibrium at a frequency of TFT given by x* = (d - b)/(a + d - b - c). If frequency is higher (lower), then it invades (is eliminated).
        4. Let’s use a stochastic approach instead:
            1. Stochastic process in finite population of size N.
            2. Choose random idividual for reproduction at each time step proportional to fitness.
            3. The offspring replaces another randomly chosen individual (holding N constant).
        5. We can calculate the probability p that a single strat A can invade and takeover a population of B players. For a neutral mutant, this is 1/N. If p>1/N, then selection favors A over B.
        6. We show that this process favors TFT over AD.
        7. Observe that the population size matters:
            1. In very small populations, helping a competitor leads to significant disadvantage.
            2. In very large populations the selection against TFT at low frequencies is too strong.
            3. Intermediate size populations are optimial for initiation cooperation.
        8. While the exact expression for p is complicated, the following simple theorem holds:
            1. For pop size N and weak selection (small w), selection favors TFT over AD if a(N-2) + b(2N-1) > c(N+1) + d(2N-4).
            2. When N=2, this reduces to b > c.
            3. For N → inf, we obtain a + 2b > c + 2d, equivalent to x* < ⅓.
        9. We define Evolutionary Stable Strategy B as:
            1. Selection opposes A invading B, i.e. a single mutant A in a pop of B players has a lower fitness.
            2. Selection opposes A replacing B, i.e. fixation prob p < 1/N.
2. [Emergence of Fairness in Repeated Group Interactions](https://link.aps.org/supplemental/10.1103/PhysRevLett.108.158104)
    1. **Setting**: Segbroeck, Pacheco, Lenaerts, Santos - 2012
    2. **Questions**:
        1. Are there further explorations worth considering wrt the ending question - “[this choice] results from a detailed competition between the capacity to avoid continuous exploitation and the generosity of contributing in groups which are only partially cooperative”?
    3. **Succeeds**:
        1. 18: Stochastic Dynamics …
        2. 15: Emergence of Cooperation …
        3. 14: Imitation Processes ...
    4. **Summary**: Consider a model where a strategy is governed by the perceived fairness of the other players and there is an associated investment pool from which to contribute and receive. What happens with this model in the limit of evolutionary dynamics?
    5. **Further Details**:
        1. How did evolution succeed in shaping such cooperative beings if the temptation to free-ride on the benefits produced by others is ubiquitous?
        2. Strategy R_m is “contribute/cooperate in first round; then continue to do so if >= m players contributed the prior round”. So m is the threshold of what this strategy considers fair.”
        3. Assume infinite population with fraction playing R_m and everyone else playing Always Defect (AD)
        4. Replicator equation for Behavioral Dynamics is dx/dt = x*(1-x)*(f_Rm - f_AD), where f_k is fitness of k given by payoff derived from group interactions.
        5. Solving this gives two interior fixed points x*_l and x*_r, former unstable and associated with coordination, the latter stable and associated with coexistence.
        6. Now assume finite population of size Z and do AD plus the N different R_m strategies over time.
        7. Evolve strategies according to stochastic mutation selection process that is the same as the prior work:
            1. Select random individual A.
            2. With probability u, A suffers a mutation and adopts a strategy from the N+1 strategies randomly.
            3. With probability 1-u, pick random individual C and have A adopt C’s strategy with probability (1+exp(B(f_A - f_C)))^(-1).
        8. They cite B as measuring the strength of the fitness contribution, i.e. Intensity of Selection.
        9. Do the Markov Chain limit population analysis seen in prior works.
        10. The most relevant number for understanding resulting fixation probabilities is the game-dependent value F associated with group investment.
        11. Population oscillates continuously between fairness and defection: “We found that evolution selects for a moderate, yet prevalent, concept of fairness in the population. This choice results from a detailed competition between the capacity to avoid continuous exploitation and the generosity of contributing in groups which are only partially cooperative.”
3. [Co-Evolution of Pre-Play Signaling and Cooperation](https://www.sciencedirect.com/science/article/pii/S0022519311000087)
    1. **Setting**: Santos, Pacheco, Skyrms - 2011
    2. **Questions**:
        1. Is there a bound to how effective this is, i.e. what happens if we just keep increasing the number of meaningless signals indefinitely?
        2. What happens with signals that are meaningful? Do they provide any *more* advantage?
    3. **Succeeds**:
        1. 18: Stochastic Dynamics …
        2. 15: Emergence of Cooperation …
        3. 14: Imitation Processes ...
    4. **Summary**: Use the Fermi distribution from 18 and the small-mutation limit from 14 to govern how agents live and die. Then model communication by asking what happens if there are meaningless pre-play signals. Analyze the resulting Markov chain and find that more signals leads to higher prevalence of cooperation. This is interesting because we haven’t changed much but yet everything about the resulting dynamics are different.
    5. **Further Details**:
        1. Costless pre-play communication with no pre-existing meaning (defined as “Cheap Talk”) can destabilize a non-cooperative equilibrium.
        2. It can also create new equilibria and change relatize size of the basins of attraction.
        3. Uses Fermi Distribution to model probability of imitation and uses 18’s reasoning as the guiding motivation.
            1. The B is noise associated with errors in decision modeling*.*
            2. High B → Pure imitation dynamics (cultural evolution studies).
            3. B → 0 looks like random drift.
        4. Uses the small-mutation limit introduced in 14. Note how similar this is to the DeepMind Alpha-Rank language: “In the absence of mutations, the end states of evolution are inevitably monomorphic, as a result of the stochastic nature of the evolutionary dynamics and update rule(s). By introducing a small probability of mutation, the population will either wipe out the mutant or witness its fixation. Hence, in the small-mutation limit, the mutant will fixate or will become extinct long before the occurrence of another mutation and, for this reason, the population will spend all of its time with a maximum of two strategies present simultaneously.”
        5. With k players playing A and n-k playing B, we have pi_A(k) = k/n * p_A,A + ((n-k)/n)*P_A,B → Markov chain where each state is a monomorphic end-state associated with a given strategy and transitions are fixation probabilities of a single mutant of one strategy in a population of individuals of another strategy.
        6. Resulting stationary distribution characterizes the average time the population spends in each of the states.
        7. Introduce signaling stage before each interatction where individuals can send “0” or “1”. This is an 8-stage Markov Chain a la the above.
        8. Full cooperation now occurs 80% of the time in Stag-Hunt.
        9. Increasing the # of signals favors prevalence of cooperation. This holds for PD as well.
4. [Evolutionary Prisoner’s Dilemma Game on a Square Lattice](https://arxiv.org/abs/cond-mat/9710096)
    1. **Setting**: Szabo, Toke - 1997
    2. **Questions**:
        1. The paper mentions that we cannot create a new strategy under their model. Today, we are doing that with gradient descent based models learning and then using the small mutation assumption to assume all is good. Is this satisfying? What else can we do to create a new strategy that is capable of spreading?
    3. **Succeeds**:
        1. 28: Spatial Dilemma of Evolution
    4. **Summary**: This paper takes a model spearheaded by Nowak and applies the Isling notion from physics to it, which is what suggests using the Fermi distribution. They describe the intensity parameter as characterizing the noise introduced to permit irrational choices.
    5. **Further Details**:
        1. The emergence of uniform cooperation becomes easier when players interact much more with their neighbors than those far away. This survival of cooperators is true even when we do not use any kind of elaborate strategies like TFT.
        2. Follows Nowak and May with a spatial evolutionary PD game. Outcome there depends on the initial configuration and rescaled payoff matrix described by parameter B characterizing temptation to defect. The results are nonlinear function of B.
        3. Their model is on a square lattice with two strategies C and D. On this simplification, they use Ising Formalism from statistical physics, which encourages using the Fermi distribution to adopt another’s strategy: (1+exp(-(E_y - E_x)/K))^(-1), where E_Y is the neighbor’s payoff and E_x is my payoff and *K characterizes the noise introduced to permit irrational choices*.
        4. They consider models with both immediate neighbors as well as secondary neighbors.
        5. The two absorbing states are “independent of time because evolutionary rule cannot create a new strategy which can spread under advantageous conditions.”
5. [Alpha-Rank](https://arxiv.org/abs/1903.01373)
    1. **Setting**: Omidshafiei, Papadimitriou, Piliouras, Tuyls, … - 2019
    2. **Questions**:
        1. What happens when you change the underlying RUM from Fermi Distribution to something else? Their reason for using it was that everyone else uses it. So what happens when you push on that?
        2. The obvious one is what happens when you use this during training. Can you guide the models towards something better by incorporating this into the pipeline?
        3. How does this account for more complex strategies or ensembles of strategies? What if you meta-metaed by going one up the strategy rung and picking from amongst the meta-agents?
        4. What happens if the payoffs try to take into account future changes in the game / dynamics instead of being instantaneous?
        5. Are there certain points in the alpha sweep that are closer to Nash than others? Is there something deterministic about that?
    3. **Succeeds**:
        1. [Claussen: Discrete stochastic processes](#claussen08)
        2. Nowak: Evolutionary dynamics of biological games
        3. Traulsen: Staochasticisty and evolutionary stability
        4. Papadimitriou: From Nash equilibria to chain recurrent sets
        5. Traulsen: Stochastic dynamics of invasion …
        6. Santos: Co-evolution of pre-play …
        7. Segbroeck: Emergence of fairness in repeated…
        8. Tuyls: A Generalised Method for Empirical Game Theoretic Analysis.
    4. **Summary**:
    5. **Further Details**:
        1. Algorithm presented is alpha-Rank, a methodology for evaluating and ranking agents in large-scale multi-agent settings grounded in dynamics.
        2. See Fig 1 for an overview of the paper in 9 sections. At a high level, the ideas presented build off of game-theoretic solution concepts and evolutionary dynamics concepts.
        3. Considers the ideas behind alpha-rank, in particular Markov-Conley Chains (“MCC”), as the basis for a dynamic solution concept analogously to Nash as a static syolution concept.
        4. The associated alpha-rank method runs in polynomial time wrt the total number of pure strategy profiles.
        5. “... We show that by introducing a perturbed variant [of MCCs] that corresponds to a generatlized multi-population discrete-time dynamical model, the underlying Markov chain containing them becomes irreducible and yields a unique stationary distribution. The ordering of the strategies of agents in this distribution gives rise to alpha-rank. It automatically produces a ranking over agents as well as filters out transient agents.”
        6. Meta-Game: Simplied model of an underlying multi-agent system, e.g. an RTS, auction, or football match.
        7. Meta-Strategy: Learning agents, e.g. different variants of AG agents or agents at different times in the learning process.
        8. Meta-payoffs: Payoffs quantifying the outcomes when palyers play profiles over the set of meta-strategies. Calculated from empirical interactions among the agents, e.g. win/losses in Go.
        9. How many such interactions are necessary to have a good approximation of the underlying meta-game?
        10. Mentions Replicator Dynamics and how they are not scalable (arguably because they use continuous time dynamics).
        11. Cite the small-mutation rate theorem and other prior work to do evolutionary dynamics as a Markov Chain. “Prior works have conducted this discrete-time Markov chain analysis in the context of pair-wise interaction games with symmetric payoffs. We, however, formulate a generalized model that also applies to K-wise interaction games with asymmetric payoffs.”
        12. At each timestep, one individual from each population is sampled uniformly and the K resulting individuals play a game. From this, we get a fitness of each individual playing a given strategy. Note that individuals from the same population never directly interact.
        13. Randomly sample two individuals in population k and consider the probability that the one playing strategy T copies the other individual’s strategy S. The probability of copying governs the dynamics of the model. They use the logistic selection function (aka Fermi distribution) with reasoning being its extensive use in the single-population literature.
        14. Based on this, we build the Markov chain over the set of strategy profiles where each state corresponds to one of the strategy profiles where each population concludes as monomorphic.
        15. Following the literature, we get that the fixation probability p_{sig, tau}^k(s^-k) of a single mutant with strategy tau in a population k of m-1 individuals playing sig is the following (p. 7):
            1. 1/m if f^k(tau, s^-k) = f^k(sig, s^-k)
            2. (1 - exp(-alpha * (f^k(tau, s^-k) - f^k(sig, s^-k)))) / (1 - exp(m * -alpha * (f^k(tau, s^-k) - f^k(sig, s^-k)))) if f^k(tau, s^-k) != f^k(sig, s^-k)
            3. The above corresponds to an m-step transition in the Markov chain correspodning to P(tau → sig, s^-k).
        16. For any given monomorphic strategy profile, there are a total of V = \sum_k (|S^k| - 1) valid transitions to a subsequent profile where only a single population has changed its strategy. Let nu = V^-1, then nu*p_{sig, tau}^k(s^-k) is the probability that the joint population state transitions from (sig, s^-k) → (tau, s^-k) after the occurence of a single mutation in population k.
        17. This then yields the Markov transition matrix C s.t. C_ij =
            1. nu * p_{s_i^k, s_j^k}^k(s_i^-k) if there exists k s.t. s_i^k != s_j^k and s_i^-k = s_j^-k.
            2. 1 - \sum_{j != i} C_ij if s_i = s_j
            3. 0 otherwise
        18. “In the large-population limit, the macro-dynamics model is equivlanet to the mciro-dynamics model over hte edges of the strategy simplex. The limiting model is a variant of the replica dynamics with the caveat that the Fermi function takes the place of the usual fitness terms.
        19. Examples (albeit these are not new):
            1. RPS: The resulting stationary distribution gives ⅓ mass to each of R, P, S. We also get a cycle in the graph as expected.
            2. Battle of the Sexes: The stationary distribution gives ½ mass to each of (O, O) and (M, M) as expected. There are no cycles, only absorbing states.
        20. The fact that dynamics converges to Nash in some instances is an articfact of the 2-dim nature of 2x2 games (cited as a consequence of the Poincare-Bendixson theorem). But the dynamics of even 2p games with >2 strategies can be chaotic.
        21. That leads to the goal of Markov Conley Chains: Identify what dynamics converges to, i.e the non-trivial irreducible behaviors of a system and consequently provide a new solution concept.
        22. They then take a detour where they rehash much of what was discussed in Papadimitriou: From Nash equilibria to chain recurrent sets.
            1. “Every game is a potential game if only we change our solution concept from equilibria to chain recurrent sets.” Seems strong.
            2. “The structure of the transition probabilities [introduced in Def 2.4.23] renders the MCCs invariant under arbitrary positive affine transformations of the payoffs, i.e. the resulting theoretical and empirical insights are insensitive to such transformations.”
        23. “The key idea behind the ordering of agents we wish to compute is that the evolutionary fitness/performance of a specific strategy should be reflected by how often it is being chosen by the system/evolution.”
        24. Problem: MCCs are efficient to compute, however games may have many of them. How do you choose amongst these?
            1. Solution:Introduce noise and study stochastic version s.t. That overall chain is irreducible → has unique stationary distribution.
            2. Practice: Choose an agent k at random and, if it is playing strategy s_i^k, choose one of its strategies s_j^k at random. Set the new system state to be e(s^k, s^-k) + (1-e)(s_j^k, s^-k). For a particularly large choice of alpha, thiese dynamics correspond closely to earlier model.
            3. In the limit of alpha, the Markov chain associated with the earlier model coincides with the MCC above.
        25. Key Result: To resolve hte MCC selection problem, we require a perturbed model, but one with a large enough ranking-intensity alpha s.t. It approximates an MCC, but small enough that the MCCs remain connected.
            1. This implies the need for a sweep over the ranking-intensity parameter alpha, which is the only parameter.
        26. Alpha-Rank Method:
            1. Construct Meta-game payoff table M^k for each population k from the data / simulations.
            2. Compute transition matrix C (outlined in 2.1.4) with a sufficiently large alpha.
            3. Compute stationary distribution pi.
            4. Compute agent rankings corresponding to ordered masses of pi.
        27. “Our approach is descriptive rather than prescriptive in the sence that it provides useful information regarding the evolutionary non-transient strategies and higlights remaining strategies one might play, but does not prescribe the strategies to play.”
        28. Both Nash and MCCs share the foundation of a best response (simultaneous best response for Nash and sink components of best response graph for MCCs), so where do they coincide? They note that every better response sequence converges to both a pure Nash equilibrium and coincides with an MCC.
            1. Observe that in the biased RPS game, these Nash and MCC distribtuions have different results. Nash gives (1/16, ⅝, 5/16) whereas MCC gives (⅓, ⅓, ⅓).
            2. There, the ranking-intensity sweep shows that it starts at even and ends at even for too high alpha. The in-between range is ~5e-3 to 10. There, scissors dips really low and reaches 0.1 at its nadir, while rock increases some at the beginning and then dips to ~.18 at its nadir (never below scissors), and paper otherwise jumps up a lot and reaches ~.73 at its apex.
        29. “The strategies chosen by our approach are those favored by evolutionary selection as opposed to the Nash strategies.”
        30. Briefly describes how sparse is C. For a 6-wise interaction game where agents in each population choose among 4 strategies, it is 99.5% sparse.
        31. This means that the stationary distribution can be computed in cubic-time in terms of the number of pure strategy profiles.
        32. They then have a cool series of experiments using AG, AG0, Mujoco Soccer, Kuhn Poker, and Leduc Poker, with different trained agents, etc. Some takeaways:
            1. The AG0 results are not obvious and result in agents not all the way at the end of training getting support.
            2. You can use the resulting Markov Chain to show cycles amongst competitors.
            3. The Maxent supports and the alpha-rank supports are correlated pretty darn strongly but defintiely not perfectly. See Fig 11 for this.

## Team Games:

1. R [Learning Dynamics and the Co-Evolution of Competing Sexual Species](https://arxiv.org/abs/1711.06879)
2. U [The Duality Gap for Two-Team Zero-Sum Games](http://drops.dagstuhl.de/opus/volltexte/2017/8142/pdf/LIPIcs-ITCS-2017-56.pdf)
3. R [Team-Maxmin Equilibria](http://robotics.stanford.edu/~koller/Papers/vonStengel+Koller:GEB97.pdf)
4. U [Team-Maxmin Equilibria: Efficiency Bounds and Algorithms](https://arxiv.org/pdf/1611.06134.pdf)

## Nash in the context of Dynamics and Learning:

1. R [From Nash equilibria to chain recurrent sets](https://www.semanticscholar.org/paper/From-Nash-Equilibria-to-Chain-Recurrent-Sets%3A-An-Papadimitriou-Piliouras/91b1271aa1f675d8ab6a51b581d722a7e7b69382)
2. [Beyond the Nash equilibrium barrier](http://users.cms.caltech.edu/~katrina/papers/nashbarrier.pdf)
    1. **Setting:** Wagner ‘13
    2. **Questions:**
        1. Is this a very particular / rare type of game or is it common?
        2. Can we generalize the result to a larger class of games?
        3. Are there more realistic scenarios that are based on this game and for which “typical” learning dynamics lead to something “better” than Nash equilibrium?
        4. What can we say about the dynamics and convergence of other learning algorithms (especially those currently used in practice -- how much do they deviate from the replicator dynamics) for this game?
    3. **Succeeds:**
        1. Gaunersdorfer ‘95: Fictitious Play, Shapley polygons, and the replicator equation
        2. Freund & Schapire ‘99: Adaptive game playing using multiplicative weights
        3. Hart & Mas-Collel ‘00: A simple adaptive procedure leading to correlated equilibrium
        4. Blum & Mansour ‘07: Learning, regret minimization and equilibria
        5. Blum et al ‘08: Regret minimization and price of total anarchy
        6. Daskalakis ‘10: On learning algorithms for Nash equilibria
        7. Piliouras ‘10: A learning theoretic approach to game theory
    4. **Summary:**
        1. For some games (with unnatural equilibria that are hard to find or coordinate on), natural learning dynamics such as online-learning algorithms can lead to outcomes that are much better than any equilibrium (indicating that merely looking at static limit points of dynamical systems for understanding behavior of selfish behavior is limiting)
        2. Study a game with unique Nash equilibrium but where natural learning dynamics exhibit non-convergent cycling behavior rather than converging to this equilibrium
        3. Show that the outcome of this learning process is optimal and has much better social welfare than the unique Nash equilibrium
        4. Show that (in some cases at least) the natural learning processes have the potential to significantly outperform equilibrium based analysis
    5. **Further Details:**
        1. Asymmetric Cyclic Matching Pennies: 3 players (1,2,3) with two strategies each H and T. the utility of i depends only on his action and the action of player i - 1
            1. Payoff Matrix [0, 1; M, 0] for player i and i - 1 assuming strategies H and T
            2. If i plays same strategi with i - 1 i gets 0
            3. If i plays H and i - 1 plays T then i receives 1
            4. If i plays T and i - 1plays H then i receives M >= 1
            5. The game has a unique Nash for all players to mix between H and T -- with payoff M / (M + 1) < 1 for each player
        2. Replicator dynamics, the continuum limit of the multiplicative weights update process as the multiplicative factor approaches 1 and time is renormalized
        3. The flow lines of the differential equations of the replicator dynamics on this game do not converge to a set of fixed points
        4. They show that the social welfare of the players approaches M + 1 (optimum of the game) which is significantly higher than the total welfare at the unique Nash, which is < 3
        5. They write the system of differential equations that describe the dynamics of the replicator dynamics (learning update rule) on the Asymmetric Cyclic Matching Pennies game
        6. They express the social welfare for a mixed strategy profile
        7. They show that the optimum social welfare is M + 1 (M/3 times larger than that of the Nash) and can be achieved via a correlated equilibrium
        8. They show that the trajectory of the replicator dynamic converges to the faces (boundary) of the unit cube unless started on the diagonal (all players taking same action at t = 0)
        9. They show that the dynamics converge to the 6-cycle of best responses connecting the 6 pure strategies with the max social welfare of M + 1
        10. They show that the maximum of a player’s payoff converges to 1 and the minimum to 0 as t -> inf
3. [On learning algorithms for Nash equilibria](https://people.csail.mit.edu/costis/learning.pdf)
    1. **Setting:** Daskalakis et al
    2. **Questions:**
    3. **Succeeds:**
        1. Freund & Schapire ‘99: Adaptive game playing using multiplicative weights
        2. Hart & Mas-Collel ‘00: A simple adaptive procedure leading to correlated equilibrium
        3. Zinkevich ‘04: Theoretical guarantees for algorithms in multi-agent settings
        4. Blum et al ‘08: Regret minimization and price of total anarchy
    4. **Summary:**
        1. Consider the dynamics of the standard multiplicative weights update learning algorithms (known to converge to Nash in zero-sum games)
        2. Show that fictitious play does not converge in general games (using a 3x3 game defined by Shapley)
        3. Show that a variety of settings the multiplicative updates algo fails to find the unique Nash
    5. **Further Details:**
        1. For 3 different learning algorithms (with multiplicative weight updates i.e. the weights are multiplied by an exponential in the observed utility), they show that they do not converge to the unique Nash on Shapley’s 3x3 game
        2. They show that the learning dynamics lead the players’ cumulative distributions away from the equilibrium exponentially quickly
        3. The cumulative distribution means the time average of the private distributions played up to some time step
        4. Distribution Payoff Setting:
            1. Each player receives a vector fo the expected payoffs that each of his strategies would receive given the distribution of the other player
        5. Stochastic Setting
        6. Multi armed setting
        7. The game has row and column payoffs given by
            1. R = [0 1 2; 2 0 1; 1 2 0]; C = [0 2 1; 1 0 2; 2 1 0]
        8. The proofs are based on potential functions
4. [The explanatory relevance of Nash equilibrium](https://www.jstor.org/stable/10.1086/673731?seq=1#metadata_info_tab_contents)
    1. **Setting:** Kleinberg et al
    2. **Questions:**
        1. Can it be that the learning algorithm is too simple and so the update rule cannot exploit out-of-equilibrium behavior or order to drive the system to an equilibrium?
        2. How do the dynamics change when using different learning algorithms?
        3. Can we design an update rule that will lead to equilibrium in this case? Is it desirable to do so?
    3. **Succeeds:**
        1. Skyrms ‘92: Chaos and the explanatory significance of equilibrium: strange attractors in evolutionary game dynamics
        2. Sandholm ‘10:Population games and evolutionary dynamics
    4. **Summary:**
        1. Show that even in two-strategy games with a single equilibrium, a family of imitative learning dynamics does not lead to equilibrium
        2. They show that when the myopic rationality parameter (beta) is low the players are guaranteed to converge to the equilibrium (just like in the low rationality replicator dynamics)
        3. When beta is increased, the players become more myopically rational. The quick speed at which rational agents adopt a best response causes the population to overshoot the equilibrium, leading to periodic and chaotic attractors
        4. The system is very sensitive to initial conditions so it is very hard to make accurate predictions about the system’s future state
        5. Surprisingly increased myopic rationality does not result in convergence to equilibriums but rather is the cause of chaotic behavior and persistent out of equilibria play
    5. **Further Details:**
        1. Cressman 2003 shows that in some types of games standard continuous time evolutionary and learning dynamics are guaranteed to lead to Nash equilibriums, but these theorems cover only some special cases
        2. Skyrms 1992, Schanbl 1991 show that in some games with four strategies the continuous-time replicator dynamic exhibits chaotic behavior in which orbits converge to strange attractors and Hamiltonian chaos is seen in replicator dynamic in both 4-dim RPS games (Sato et al 2002) and 52-dim zero-sum signaling games (Wagner 2012)
        3. In this paper they show that such non-convergence results arise in the simplest possible case: ordinary two-strategy games with a single equilibrium.
        4. Discrete-time imitative logit dynamics (introduced by Cabralese and Sobel 1992) exhibit chaotic behavior in such games → the limits points of the discrete time i-logit dynamic are “rationalizable strategies”
            1. Parametrized by a “myopic rationality” term that influences the probability with which agents choose to imitate another player with a higher payoff
            2. For low values of this parameter, the dynamics resemble the not-so-rational replicator dynamics
            3. For high values of this parameter the dynamics resemble the highly rational best-response dynamics
            4. Allows the study of a range of boundedly rational players
        5. The dynamics follow the period-doubling route to chaos as the myopic rationality parameter is varied from low to high
        6. The route from low rationality learning to high myopic rationality involves a journey through regimes of complicated limit cycles and chaotic attractors
        7. The dynamics also undergo the period doubling route to chaos as the payoffs of the game are varied
        8. The discrete-time imitative logit dynamic: assume a pool of agents are matched at random to play a two-person game and x_i is the frequency of agents. Each agent is offered the chance to revise her current choice of strategy at a time interval with a rate R exponential distribution. The agents will change their strategy according to the system’s conditional switch rates rho_ij which give the rate at which agents currently using strategy i switch to strategy j. This rate can depend on the current population state as well as the expected payoffs for each strategy type. (stochastic game dynamic)
        9. One can look at the deterministic process that follows the expected motion of the stochastic dynamic → the mean dynamic provides a good approx of the stochastic process assuming the population is sufficiently large
        10. dx_i/dt = expected inflow - expected outflow = \sum x_j rho_ji - x_i \sum rho_ij
        11. If the agents choose a player at random and then imitate that player’s strategy with probability proportional to the difference between that players expected payoff and her own expected pay, then it will have the standard continuous-time replicator dynamic as its mean dynamic. One version of this protocol assumes that an agent will keep randomly picking other players until they choose to imitate one of them
        12. rho_ij = x_j w(pi_j) / sum (x_k w(pi_k))
        13. If w(pi_j) = exp(beta pi_j) -- continuous time imitative logit (i-logit) dynamic
        14. Beta -- myopic rationality parameter
        15. They analyze the system using orbit diagrams (showing the states that occur in the long run as the map is iterated). In this case, they exhibit period-doubling route to chaos typical of 1D maps. that the system has positive Lyapunov exponents for some values of beta so the system does indeed exhibit deterministic chaos
5. R [Multi-Agent Learning in Network ZS Games is a Hamiltonian System](https://arxiv.org/abs/1903.01720)

## DeepMind (Otherwise Uncategorized):

### [Re-evaluating Evaluation](https://papers.nips.cc/paper/7588-re-evaluating-evaluation.pdf)

1. **Setting**: Balduzzi et al - NeurIPS 2018
2. **Questions**:
    1. No clear open question from this paper; the only vague open question that is raised here is: can we build other evaluations metrics that satisfy invariance to redundancy and robust to small perturbations? Are there other properties that metrics should satisfy beyond these two criteria?
    2. **Summary**: This paper aims at designing evaluation metrics that work with cyclic games (1st part of the paper) and that are invariant to redundancy, robust to noise and interpretable for zero-sum games (2nd part of the paper).
    3. Succeeds:
    4. **Further Details**:
        1. Goal: treat tasks and agents symmetrically.
        2. AvA: task is to beat another agent. Performance in general quantified using Elo ratings. But Elo is meaningless:
            1. in cyclic games (because beating one agent doesn’t mean that you beat everyone else).
            2. Elo rating is not robust to redundancy: we can inflate an agent’s Elo rating by instianting many copies of an agent it beats.
        3. Design of an Elo rating (mElo) that handles cyclic interactions.
        4. Introduction of another evaluation metric: Nash averaging.
            1. It is not sensitive to redundant tasks and agents.
            2. Idea: play a meta-game on evaluation data. This meta-game has a unique max entropy Nash. This Nash automatically adapts to the presence of redundancy.
        5. Elo rating: n agents play a series of pairwise matches against each other. Elo assigns a rating r_i to player i based on their wins and losses. What matters is the difference between Elo ratings: this is the quantity that is used for predicting wins and losses. The prediction is made used probabilities computed from these differences. Unfortunately, this method can fail in cyclic games.
        6. Definition of mElo: starting from the definition of Elo but adding an approximation to the cyclic component of the matrix of win-loss probabilities (Elo probabilities). Adding this cyclic component leads to good results in cyclic games.
        7. Desiderata for a good evaluation method:
            1. Invariant: adding redundant copies of an agent/task doesn’t make any difference.
            2. Continuous: robust to small changes in the data.
            3. Interpretable
        8. Definition of Nash averaging: a method that satisfies the desiderata 1, 2 and 3 mentioned above (for zero-sum games).
            1. Given a matrix game, play a meta-game. Meta-players of this meta-game pick teams of agents of the matrix game.
            2. Problem with general games: Nash equilibria are not unique.
            3. For zero-sum games, natural choice of Nash: max entropy Nash.
            4. Nash averaging method: finding the maxent Nash equilibrium p and multiply the payoff matrix A by p.
        9. Application to agents of Atari: under Nash averaging, human performance ties with the best agents.
        10. Conclusion:
            1. Nash averaging is not always the best tool: its results heavily depend on the data. Blind to differences between environments that do not make a difference to agents.
            2. Nash evaluation: another method not discussed in the paper that is also adjusting to redundant data. The advantage is that it is maximally inclusive: it only benefits from including as many tasks/agents as possible (this part is a little bit obscure, they don’t give more details…).

### [Open-Ended Learning in Symmetric Zero-Sum Games](https://arxiv.org/abs/1901.08106)

1. **Setting**: Balduzzi et al - 2019
2. **Questions**:
    1. How to extend self-play algorithm to games that have a transitive and non-transitive component?
    2. A further understanding of the two metrics “population performance” and “effective diversity” developed in the paper? What metric is the best for games? Are they invariant to redundancy and robust small perturbations? It may be possible to make a link here with the re-evaluating evaluation paper. Can we use these metrics in other settings in multi-agent learning?
    3. Computing gamescapes seems to be a computational burden: is it possible to modify them in such a way that they are easier to handle? Gamescapes are important in the paper because it is shown that growing them enables to find the Nash population.
    4. I think that there is a window for improving their algorithms: PSOR_N fails when the Nash equilibrium is contained in the empirical gamescape. PSOR_rN can be computationally heavy and they don’t show that the effective diversity is maximized for instance.
    5. Succeeds:
        1. Silver ‘18: A general reinforcement learning algorithm that masters chess, shogi, and Go through self-play
        2. Bansal ‘18: Emergent complexity via multi-agent competition
        3. Tesauro ‘95: Temporal difference learning and TD-Gammon
    6. **Summary**: This paper presents a way to extend the self-play algorithm that has been successful for monotonic games to nontransitive games.
    7. **Further Details**:
        1. Setting: FFG functional space representing zero-sum games.
        2. Decomposition of FFG = transitive + cyclic components
        3. Presence of cyclic components require more sophisticated algorithms for designing opponents.
        4. Learning in transitive games: train against a fixed opponent.
        5. Learning in monotonic games: self-play algorithm construct a sequence of objectives to optimize. Assumes that local improvements (v_{t+1} beats v_t) imply global ones (v_{t+1} beats v_1,..,v_t). Of course, this assumption doesn’t hold for nontransitive games where improving against one agent doesn’t guarantee improvements against others.
        6. Instead of looking for a single dominant agent (that may not exist), look for a population that embodies a complete understanding of the strategic dimensions of the game (this part is not very clear in the text).
        7. Gamescapes = geometrical representation of all possible strategies in FFGs.
        8. Functional Gamescapes are intractable.
        9. In practice, it is easier to work with empirical gamescapes: the set of convex mixtures of rows of the evaluation matrix (the evaluation matrix is a matrix where each entry corresponds to the evaluation of the function of the game in a pair of agents). Intuitively, it encodes all the ways agents are actually observed to interact respectively.
        10. By defining the population performance, they show that growing the polytope spanned by a population improves its performance against any other population.
        11. Effective diversity: quantifies how the best agents (agents with support in the Nash) exploit each other.
        12. Goal of the algorithms: construct objectives that when trained against, produce new effective agents (create some transitivity in non-transitive games).
        13. PSOR_N:
            1. Idea of the algorithm: generate new agents that are approximate best responses to the Nash mixture (substitute for the notion of best agent).
            2. Step 1: find the Nash mixture (notion of best agent) for the current population (from the evaluation matrix).
            3. Step 2: Ask an oracle to return an agent that does better than the Nash mixture).
            4. Step 3: Add this latter agent to your population.
            5. Remark that this algorithm improves the metric of population performance.
        14. PSOR_rN:
            1. Idea of the algorithm: each agent in the Nash trained against the agents in the Nash that it beats or ties “amplify your strengths and ignore your weaknesses”.
            2. Step 1: find the Nash mixture (notion of best agent) for the current population (from the evaluation matrix).
            3. For each agent v_t in the Nash, ask to an oracle to return an agent that beats all the agents that v_t beats in the Nash.
            4. Integrate all these agents returned by the latter for loop to your population.
            5. It can be proved that this algorithm build an effective diverse objective.
3. [Real World Games Look Like Spinning Tops](https://arxiv.org/abs/2004.09468):
    1. **Setting**: Czarnecki 2020
    2. **Questions**:
    3. **Summary**: Games of Skill have a common structure that looks like a spinning top. At the very bottom are agents that try to lose. Above that are different levels. Within each level the agents exhibit non-transitivity. Across levels the agents exhibit transitivity with all agents at a higher level defeating agents at lower levels. The top structure is because we can define the width of a level according to the size of its largest cycle. This level occurs somewhere in the middle by definition because of the existence of a Nash. As the levels increase from this widest level, they gradually become stronger until they approach the level with the Nash equilibria.
    4. **Succeeds**:
        1. Balduzzi ‘19: Open-ended Learning in Symmetric Zero-sum Games
        2. Balduzzi ‘18: Reevaluating Evaluation
        3. Vinyals ‘19: [AlphaStar](https://www.nature.com/articles/s41586-019-1724-z)
    5. **Further details**:
        1. Games of Skill are defined by progress, hence as you get better there should be a new rung of skill achievable. In particular, at the higher rungs of game play, the outcome should rely on skill and not game style.
        2. There is a large section of the paper that deals with defining games and their complexity in terms of n-bit communication. This is done so as to show that there can be very long cycles but isn’t super important o understanding what’s going on in the paper.
        3. The key points start in section 5 which shows that games of skill can be factorized into the layers.
        4. Further, if you have a minimum of agents and employ an algorithm that is capable of generating better agents, then you will rise up to the top of the top. Both of those are necessary.
        5. With better algorithms come a lesser importance on the population count because those algorithms build in priors that let the agents start at a higher position on the top (towrads the Nash).
        6. This higher position means that they don’t need the |L_z| count necessary to get over the largest cycle defined by the fat layer.
        7. Nash Clustering is helpful because it lets us get a monotonic ordering with respect to the relative population performance. This lets us be guaranteed that we’ll improve.
        8. Why? Because if we have a full cluster, then the next agent will best this *entire* cluster and thus be a jump into the next level. This is motivation for getting more diversity in the population so as to increase the coverage of the cluster.
        9. It’s a similar argument to Open Ended Learning but here it’s using all games of skill.
        10. In AlphaStar, this leads to using explotiers to expand the current nash and cover the non-transitivies.
        11. You can flip this around to single-agent domains and seek diversity of the environment.
        12. See the graphs in Table 1 for pictorial demos of the Spinning Tops.
4. R Malthusian Reinforcement Learning
5. A Generalized Method for Empirical Game Theoretic Analysis

### 

## Uncategorized

1. [Game Theory and Physics](https://www.math.ubc.ca/~hauert/publications/reprints/hauert_ajp05.pdf)
    1. **Setting:** Hauert 2004
    2. **Questions:**
        1. How are phase transitions and the level of cooperation affected in more general graphs?
        2. What if the structure of the graph changes with time or the agents’ positions aren’t fixed but rather can move around?
        3. What if the space is continuous?
        4. What if the agents can choose the agents they want to play with from some subset of the population?
        5. What if the agents can have different numbers of neighbors they interact with (e.g. diluted lattices with vacant sites)?
    3. **Succeeds:**
        1. Axelrod ‘84: The Evolution of Cooperation
        2. Lindgren & Nordahl ‘94: Evolutionary dynamics of spatial games
        3. Szabo & Toke ‘98: ‘‘Evolutionary prisoner’s dilemma game on a square lattice”’
        4. Szabo & Hauert ‘02: Phase transitions and volunteering in spatial public goods games
        5. Szabo & Hauert ‘02: Evolutionary Prisoner’s Dilemma Games with voluntary participation
        6. Semann et al ‘03: Volunteering leads to RPS dynamics in public goods game (Nature)
    4. **Summary:**
        1. Consider spatially structured populations where individuals interact and compete only within a limited neighborhood in an attempt to explain cooperation in prisoner’s dilemmas type games (which both classical and evolutionary game theory fail to do)
        2. Evolutionary game theory in structured populations reveals critical phase transitions that fall into the universality class of directed percolation on square lattices and mean-field-type transitions on regular small world networks and random regular graphs
        3. By limiting the interactions to local regions, we can enable cooperators to form clusters and thus individuals along the boundary can outweigh their losses against defectors by gains from interactions within the cluster
        4. They also consider voluntary participation (in which each agent has the option to not interact with other players) and they connect the results to those in condensed matter physics
    5. **Further Details:**
        1. Spatially structured populations:
            1. Spatially structured populations: confines players to lattice sites or to the nodes of an arbitrary graph
            2. The performance Px of a player at site x is determined by the payoffs accumulated in its interactions with its neighbors
            3. EGT infinite population starts with a percentage rho of cooperators and 1 - rho of defectors
            4. PD setting: cooperation yields benefit b to the co-player at a cost c (b > c)
            5. R = c / (b - c) -- ration of the costs of cooperation to the net benefit of cooperation
            6. A player x adopts the strategy of y with a probability proportional to the difference in performance Py - Px (if positive).
            7. They assume the transition probability is given by
                1. W(x <-- y) = [1 + exp( - (Py - Px) / k )]^{-1}
                2. K incorporates the uncertainties in the strategy adoption (variation of payoffs or mistakes in decision making)
                3. Glauber dynamics for kinetic Ising model where strategies translate to spin up and down, Py - Px change of energy when flipping the spin at x and k the temperature
                4. This dynamics drives the system toward the equilibrium state for the temperature k
                5. However, GT is different in that strategy changes occur only along boundaries which separate domains of different strategies (since agents max their individual payoffs regardless of potential losses for the population)
                6. Spatial games always have absorbing states where all members follow the same strategy
        2. Square lattices
            1. Each individual is confined to a lattice site and interacts and competes only with its four nearest neighbors
            2. Cooperators persist if r small (in contrast with well-mixed populations i.e. no structure where all agents interact with each other)
            3. For critical value of r, r_c → cooperators vanish because the benefits of spatial clustering are no longer sufficient to offset the losses along the boundary (similar phenomena observed in biological evolution of cooperation)
            4. r < r_c: persistent levels of cooperation; r> r_c: homogenous states of defection (phase transition!)
        3. Random regular graphs and regular small world networks
            1. Regular graphs: each individual has the same number of connection links to other individuals (all individuals have the same connectivity)
            2. Random regular graph (RRG): each agent has the same number of other agents with whom it interacts (same connectivity) but those agents are not limited to immediate neighborhood. Instead, they are randomly drawn from the entire population (spatial distances weakly affect interactions). So you take some nodes and set the connectivity C and then for each node (agent) you randomly pick C nodes with whom will interact. You keep doing this until all nodes have the same connectivity. But now you will have some long-range interactions instead of only local interactions
            3. Small world networks: high local connectedness and a few long-range connections that result in short average path lengths between any two nodes
            4. Regular small world networks (RSW): each individual keeps the same number of connections
        4. Conclusions
            1. Cooperation is sensitive to the topological features of the underlying population structure
            2. Cooperators perform significantly better on RRG than on square lattices
            3. Their performance on RSW lies between these two extremes
            4. The substitution of long-range connections for local ones benefits cooperation
            5. RRG lead to better chances for cooperators compared to regular lattices
            6. On the square lattice cooperators vanish according to a power law
            7. In RRG and SW cooperators vanish linearly with r -- consequence of the mean field type transitions
        5. Voluntary participation
            1. A loner can chose to not interact with other players
            2. Payoff of a risk-averse loner is constant → better off than a pair of defectors but worse off than a pair of cooperators
            3. The three strategies of C, D, and L lead to RPS type cyclic dominance
            4. In unstructured well mixed populations cooperative behavior vanishes so the absorbing homogenous state with all loners occurs
            5. This outcome changes when spatial structure and local clustering are introduced
            6. Square Lattices:
                1. Cooperation survives for any value of r (in contrast with compulsory interactions where they only persist for below some critical value r < r_c)
                2. Two different dynamical regimes: r < r_c1 (large benefits and small costs): loners vanish; r > r_c1, loners are vital for the existence of cooperators so all 3 strategies coexist in dynamical equilibrium
                3. Extinction of longers belongs to the directed percolation universality class
            7. RRG and RSW
                1. RSW: similar dynamics to SL with r > r_c1 loners extinct and r> r_c1 all 3 strategies. But for r > r_c2 persistent global periodic oscillations occur → global synchronization
2. R [The effect of constrained communication and limited information in governing a common resource](https://repository.asu.edu/attachments/144346/content/Janssen_Lee_Tyson_473-3763-1-PB.pdf)
3. U [Spatial Dilemma of Evolution](https://www.researchgate.net/publication/243764392_The_spatial_dilemma_of_evolution)
4. <a name="openaccesstexts">OpenAccessTexts</a>: [Random Utility Models](http://www.openaccesstexts.org/pdf/Quant_Chapter_13_rum.pdf)
    1. **Setting**: Hofacker - 2007. Textbook called “Mathematical Marketing”.
    2. **Questions**:
        1. Have RUMs been investigated in the literature for dynamic models?
        2. What are RUMs outside of logit-based ones?
    3. **Succeeds**:
    4. **Summary**: This chapter from a textbook explains a cross-section of logit-based RUMs.
    5. **Further Details**:
        1. Random Utility Models (RUMs) are widely applied in marketing, frequently to CPGs.
        2. Assumptions:
            1. Choice is a discrete event. It’s all-or-nothing.
            2. Utility towards a brand varies acorss individuals as a random variable.
            3. Individuals choose the brand with the highest utility.
            4. **This document focuses on logit RUMs**
            5. Gives motivation for the logistic model:
                1. OLS can produce predicted values outside of [0, 1], i.e. ones that are not logically consistent choice probabilities.
                2. It could also produce values that are not sum constrainted to 1.
                3. There is resulting heteroskedasticity of error variance.
                4. A way to solve these comes from p_ij (probability of person i picking the jth option) = (1+exp(u_i))^-1 when there are two options and exp(u_ij) / \sum_{choices k} exp(u_ik) when there are more.
                5. Can linearize to get familiar logistic regression.
            6. Leads to multonomial logit model (MLM) of p_ij = exp(x_ij * B_j) / \sum_{choices k} exp(x_ik)B_k
            7. Section 13.7 details a variety of other models, but all following that p_ij = a_ij / (\sum a_ik):
                1. *MLM*: we have a_ij = exp(\sum_k x_ijk*B_k). Intuitively, this assumes that there is a B coefficient for each of the k marketing instruments, but these are constant across the brands.
                2. *Multiplicative Competitive Interaction*: a_ij = prod_k (x_ijk ^ B_k). This follows in the footsteps of the Cobb-Douglas equation.
                3. *Differential Effects Models*: This can be applied to the MNL and the MCI. Here, we add a subscript for the B_k → B_jk.
                4. *Fully Extended Models*: Also can be applied to the others. Now $$a_ij = \exp(\sum_m \sum_k x_{imk} * B_{mjk})$$ or similarly $$a_ij = \prod_m \prod_k x_{imk}^{B_{mjk}}$$
5. U [Generalized Random Utility Model](http://tzyy-ling.ukn.edu.tw/seminar4/%E5%B0%88%E8%A8%8E%E5%A0%B1%E5%91%8A%E5%8F%83%E8%80%83%E8%B3%87%E6%96%99/%E7%AC%AC%E4%B8%80%E6%AC%A1/%E9%99%B3%E6%99%AF%E5%85%83/Generalized%20random%20utility%20model.pdf)



