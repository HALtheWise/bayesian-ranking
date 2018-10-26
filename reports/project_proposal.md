# Bayesian Judging
## Eric Miller, Olin College

Inspired by the judging system used at many hackathons including HackMIT and HackHarvard, I intend to create bayesian algorithms that estimate the global ordering of teams based on pairwise comparisons performed by judges. By applying Bayesian methods, I hope to be able to answer probabilistic questions about the result (for example, declaring a tie when there is insufficient data to distinguish between two teams), as well as allow more expressive input data formats, like allowing judges to rate one team as "much better" or "slightly better" than another, or allowing a mixture of absolute-scale judging and relative judging.

## Prior Art

There has been a lot of work done with using [pairwise comparisons](https://en.wikipedia.org/wiki/Pairwise_comparison)
as the basis for judging and ranking systems, most notably the work done by Anisha Thalye for HackMIT. This system has gone through multiple versions, and is documented 
[here](https://www.anishathalye.com/2015/03/07/designing-a-better-judging-system/)
[here](https://www.anishathalye.com/2015/11/09/implementing-a-scalable-judging-system/)
and [here](https://www.anishathalye.com/2016/09/19/gavel-an-expo-judging-system/), with source code [here](https://github.com/anishathalye/gavel).

This system is vaguely Bayesian, and is based on an optimization approach to the problem of finding the maximum likelihood ranking of teams given a probability model for the likelihood of a given comparison result given the difference in quality between the teams. Originally, the system used Thurstone's approach of assuming that each team is represented by some probability distribution, and each judge reports on the difference between one random draw from each team's distribution. I want to extend this model with an additional parameterized distribution per judge, capturing the bias and variance associated with their measurements. 

## Goals

I want to implement a system inspired by the algorithms above, [Thurstone's model of comparative judgement](https://www.anishathalye.com/media/2015/03/07/thurstone1927.pdf), and hierarchical Bayesian modelling, that estimates the full distribution of posterior expectations for team quality rather than just the maximum likelihood and offers significantly more flexibility in supporting mixed judging types and more sophisticated distribution shapes than the optimization approach currently used. That will probably require using MCMC algorithms because the number of latent variables is quite large (hundreds).

## Approach

I have been able to find two datasets from actual hackathons, a [smaller one](https://www.anishathalye.com/media/2015/03/07/blueprint-rookie-data.txt)
 containing 235 votes for 37 teams and a [larger one](https://www.anishathalye.com/media/2015/11/09/judging-data.csv)
 with 108 judges, 195 teams, and 1351 votes. These datasets will be the primary focus of the analysis, starting with the smaller one. It may prove necessary to build tools to synthesize new data to investigate scaling better, but having these as a starting point is critical. 

The first step will be to build the infrastructure in PyMC and with grid algorithms to represent the likelihood of a given comparison with the simple model used in the original algorithm, then extend the model with additional parameters for judge reliability and more expressive distributions.