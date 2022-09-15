Thank you for your detailed review and feedback, which has been greatly helpful in improving the quality of our manuscript.

We have returned our response to reviewer comments in plain text format.
However, a nicer-to-read rendered version of our responses to your feedback may be viewed at the following url,

https://github.com/mmore500/tag-olympics-writeup/blob/master/review/reviewer-1.md

We are taking advantage of the git version control system to track and report changes we make in response to your comments.

In some of our responses, we include "diffs" that show the exact changes we made.
Here is an example diff showing a line "deleted text" being replaced by "inserted text."
Note the leading "-" in front of deleted text and the leading "+" in front of inserted text.

```diff
--- a/source.tex
+++ b/source.tex
@@ -1,3 +1,3 @@
 unchanged text
-deleted text
+inserted text
 more unchanged text
```

These diffs are syntax highlighted for easy skimmability on the rendered version of our notes linked above.
We also provide some links so that diffs may be viewed directly on GitHub.

> This paper explores the properties of different tag matching metrics that have been used for inexact matching in evolutionary and artificial life systems.
> These properties are considered theoretically, empirically with consideration to geometry and variation, and empirically within evolutionary systems.
>
> I really like the premise of this paper and think that it could make an important contribution to the understanding of how tag matching metrics affect the use of inexact matching.
> However, I have 4 major concerns that I believe must be addressed before publication, discussed in detail below.
> So, while I cannot recommend acceptance at this time, I think some revisions, moderate changes, and additional experiments could make this an excellent contribution to this journal.
>
> Major Concerns
> ==============
>
> The one thing I really wanted to see in this paper was an analysis of in what situations you would want to use each distance metric, but that is unfortunately missing.
> What qualities of problem space, tag mutation, etc. would warrant a recommendation of one over another?
> Are any flat-out bad ideas?
> What do your different experiments illuminate about when matching schemes would be useful?
> I want to see more actionable direction that can be used to improve systems that use tags.
> I would expect to see this analysis in the Discussion section; however, I found that section to be most lacking.
> The first 7 paragraphs are more or less a recap of the results section.
> That is not useful analysis, does not belong in a Discussion section, and should likely be cut entirely.
> The last paragraph begins some useful discussion, but I would like this taken much further in the directions I outlined above.

We agree that practical recommendations are an important point to expand on.
We added a subsection to the end our discussion in order to speak to this point more directly.

```diff
+\subsection{Practical Recommendations}
+
+Our results highlight the dependence of tag-matching metrics' performance on  problem domain.
+So, consideration of properties of the problem domain at hand should drive the decision of which tag-matching metric to use in a particular system.
+
+A major practical advantage of integer-based metrics is the possibility for log time lookup of operands (i.e., via binary search).
+However, the integer metrics performed competitively only within a particular class of problem domains.
+In all high-constraint problem domains, the integer metrics performed poorly.
+Among low-constraint problem domains, the integer metrics only performed well on the toy graph-matching task --- they did not perform well on the low-constraint GP changing signals task.
+At present, it is unclear what problem domain property stymied the integer metrics in the low-constraint GP changing signals task --- the potential for the operand set to grow over time (e.g., duplication and divergence), fitness landscape ruggedness (e.g., epistasis), or something else.
+
+However, within the low-constraint, fixed-size, smooth fitness landscape problem domain where integer metrics performed well, the hash metric actually performed slightly better.
+So, under these conditions, the hash metric may be preferable when log time lookup is not critical.
+
+Outside of low-constraint, fixed-size, smooth fitness landscape problem domains, the Hamming and streak metrics perform best.
+On the low-constraint, fixed-size, smooth fitness landscape tested, both evolved full solutions, although the Hamming metric was slightly slower than the streak and integer metrics.
+In particular, the Hamming and streak metrics performed best in our GP tests.
+So, both metrics appear to be reasonable choices in most cases.
+
+Choosing between the two will likely depend on implementation considerations: the streak metric facilitated faster adaptive evolution in some experiments, but is more computationally expensive to calculate than the Hamming metric.
+Future work should investigate whether a simplified version of the streak metric --- for example, ignoring mismatching streak length and only considering matching streak length --- suffices to capture its properties.
```


> I find the "Match Distance/Percentile" plots that appear throughout the paper (including and not limited to Figs. 1, 2c, 3c, etc.) very hard to wrap my mind around, and strongly recommend reconfiguring every one of them.
> Are they essentially cumulative distribution functions on their sides?
> Or are they cumulative histograms, except for some reason the bars are the white part at the bottom and the bars are on the top?
> After reading a bunch of these, I can somewhat interpret them, but would have a hard time explaining precisely what they mean.
> I think I would find histograms (whether cumulative or not, not sure which would be better), easier to read and would show the same trends.
> All of these definitely need to be changed, since they're a core piece of this work and are very difficult to interpret.

Agreed on the shortcomings of the existing distribution plots.
We have replaced these all with histograms and moved the originals, which do show some granular detail not entirely captured by the histograms, to the supplements.

> Early in the paper you mention "In experiments where mutations were applied to tags, individual bits were toggled stochastically at a uniform per-bit rate.", which I immediately thought "that doesn't sound fair to the integer-based metrics."
> You later, at the start of section 4, acknowledge this and state that further experimentation should be made with other mutation operators.
> I think it would be appropriate and within the scope of this paper to at least test 1 mutation operator that would have low locality for the integer representations, comparing it to the bit-flip mutation.
> You suggest adding rounded Gaussian noise, which I think would be appropriate.
> I could imagine other mutations here as well -- I don't think you have to test all of them, but choosing one representative would round out the paper nicely.

We tested the Gaussian noise mutation operator and found that it performed equivalently or worse compared to the bitwise mutation on the graph matching task.

We added a supplementary section reporting this result.
We also expanded our footnote aside on the application of bitwise mutation to the integer metrics to mention this further motivation for restricting our scope to consider only bitwise mutation.
[Diff](https://github.com/mmore500/tag-olympics-writeup/commit/ec330fdc8fa87c470cd7ed054ba1cdc0d41becf5).

> The experiment in section 5.1 is very confusingly described.
> After reading it 3 times, I'm still not sure what's going on.
> This needs to be rewritten from scratch to precisely describe the experiment, what genomes are, how they are used, and what they have to do with random bipartite graphs.

We have revised our explanation as follows
```
In this experiment, we explored metrics' ability to facilitate evolution of an arbitrary, fixed pattern of tag-matching connectivity.
Genomes in this experiment consisted solely of a set of 32 tags, partitioned evenly as 16 query tags and 16 operand tags.
We used randomly-generated bipartite graphs as a specification for an arbitrary set of desired operands that queries should match to, with each node in the graph corresponding directly to a bitstring tag in the genome.
Figure \ref{fig:graph_layouts} shows example target graph layouts.

To evaluate the fitness of a genome, we harvested its operand tags placed them into a tag-matching data structure.
This data structure allowed us to match each query tag to its $n$ best-matching operands.
For each query tag, we set the number of collected operand matches $n$ to the outgoing edge count from its corresponding node in the target graph.
So, for example, if a node had three outgoing edges then we would record the three best-matching operands of the corresponding tag.

For the purposes of fitness evaluation, we considered matches within this set equivalent.
So, each query tag had a set of $n$ best-matching operands.
We took that query tag's contribution to fitness as the fraction of its best-matching operands that corresponded to connections on the target biparte graph; that is, the fraction of best-matching operands with corresponding graph nodes connected by an edge to the query's corresponding graph node.

Along these lines, we took overall fitness as the fraction of best-match tag pairs that correctly corresponded to edges in the target graph.

We controlled the degree of tag-matching constraint imposed by the target graph by manipulating:
```

> Moderate Concerns
> =================
>
> There are a handful of typos and English issues throughout, which need to be tightened up, especially when they say the wrong thing.
>
> The introduction does not do a good job of summarizing the results or discussion of the paper.

We embellished the end part of our introduction to directly cover these topics.
```diff
-In this work, we show how tag-matching systems differ with respect to
+In this work, we survey five tag-matching schemes: two based on integer representations, one based on Hamming distance, a "streak" metric based on the maximum length of identical substrings, and a control metric that uses a hashing algorithm to compute completely arbitrary match distances.
+
+We explore how these tag-matching schemes differ with respect to
 \begin{enumerate}
   \item geometric structure that biases or limits the patterns of connectivity that form among queries and operands (Section \ref{sec:geometric}),
   \item variational properties that influence changes to connectivity observed under mutation (Section \ref{sec:variational}), and
   \item evolutionary outcomes such as the rate of adaptive evolution and the quality of evolved solutions (Section \ref{sec:evolutionary}).
 \end{enumerate}
-Understanding the implications of tag-matching rules will directly enable more effective genetic programming practice.
-To a more theoretical bent, such work also provides a foundation for inquiry into the properties and mechanisms of tag-matching systems in nature.
+
+Across several geometric analyses, we found the geometric structure of the integer metrics to be most restrictive, followed by the Hamming and then streak metrics.
+We observed large-effect one step mutations under the integer metrics and streak metrics, but not under the hamming mutation.
+Except for the control hash metric, match affinity decayed most rapidly along mutational walks under the integer metrics.
+Match affinity decayed slowest along mutational walks under the hamming metric.
+
+Evolutionary experiments also showed meaningful differences between tag-matching schemes.
+We found that rigid, one-dimensional geometric structure of the integer metrics impeded satisfaction of multiple simultaneous tag-matching requirements in scenarios where a query tag was required to closely match to more than one operand.
+Integer metrics also fared poorly in genetic programming experiments, even when the selected-for tag-matching scenario only involved matching queries to a single operand.
+Evolutionary conditions in these experiments were configured to emphasize duplication and divergence by restricting sources of variation, namely initial tag variation and ongoing insertion of randomly-generated tags.
+The Hamming and streak metrics generally fared best, with the streak metric outperforming the Hamming metric in some scenarios.
+
+Although the Hamming and streak metrics generally matched or outperformed the integer metrics, confirming the extent to which these findings generalize across tag-matching application domains beyond those surveyed --- particularly with respect to mutation operator used --- necessitates further research.
+Improved understanding of the implications of tag-matching rules will directly enable more effective genetic programming practice.
+To a more theoretical bent, this work also provides a foundation for inquiry into the properties and mechanisms of tag-matching systems in nature.
```

> Transition from introduction to section 2 is very abrupt. Section 2 could use a paragraph explaining what it will contain.

We have added an introductory paragraph to Section 2.
```diff
+Rigorous comparison of disparate tag-matching schemes required careful standardization of tag representation, mutation, and match quality calculation.
+This section summarizes the tag representation, mutation, and match quality calculation procedures used in our experiments and provides formal definition for each of the surveyed tag-matching schemes within that framework.
```

> Section 2.5 and 2.6 discuss "probability of a contiguous k-bit match" and "probability of a match distance". What does "probability" mean here?
> Of any two randomly sampled bit strings?
> Or something else?
> You need to explain what you mean.

[Clarified](https://github.com/mmore500/tag-olympics-writeup/commit/5d9713678e64e6590aa8c10af91e3d52d8fe3386).

> Section 2.5 "probability of a contiguous k-bit match between two bitstrings" -- Does this mean probability of an exact k-bit match, or of at least k-bit match (i.e. k or more contiguous bits match).
> My reading of the math is that it has to be the latter, but this should be stated clearly either way.

[Further clarified](https://github.com/mmore500/tag-olympics-writeup/commit/fb8a3e5bd3f4f59ea9a842eb9b20f22e27105cd4).

> All of the experiments in this paper use somewhat arbitrary numbers of samples.
> For example, Figure 1 uses 10,000 random pairs, where others use 5,000.
> A few remarks here:
> - None of these experiments look computationally expensive, so why not use more samples to get finer-grained detail?
> Especially for the normalization process, which is important for all remaining experiments, why not use every one of the 2^32 tags at least once, with its pair a randomly-generated tag?
> Computing the distance between all 2^64 distinct pairs of tags is likely infeasible, but I could imagine computing many more than you do.
> - On the other hand, if you think 10,000 (or 5,000) is "enough" in the sense that adding more data won't change distributions, you should make that case of why this (relatively low) threshold was chosen.
> - Finally, if considering all 2^32 tags is too many, would it be better to pick a smaller tag size like 2^16 so that you could consider all possible pairs?
> I would like to see justification of both the number of samples and the tag size.

We added notes explaining expected error under the Monte Carlo percentile approximation given the sample size of 10,000 tag pairs.

```diff
+Error in the Monte Carlo approximation of the percentile for any raw match score is distributed binomially.
+With 10,000 samples, absolute error at the 50th percentile can be bounded below 0.01 match distance units with 95\% confidence and below 0.012 match distance units with 99\% confidence.
+Absolute error at the 1st percentile can be bounded below 0.0017 match distance units with 95\% confidence and below 0.0024 match distance units with 99\% confidence.
+(With five independent uniformification processes, 99\% confidence per metric translates to 95\% confidence over all metrics under Bonferroni correction.)
```

The sample size of 5,000 was used for experiments that required data to be written to file for analysis and visualization.
This choice of sample size had some benefits for experiment execution speed and, especially, data post-processing and visualization time (particulary with respect to the original visualizations where each observation is drawn as an indepednent bar).
Another practical factor driving our sample size decision was the ability to store data files in the Git version control system without the use of the Large File Storage (LFS) extension.
(The 10,000 sampled match distances for Monte Carlo percentile approximation were only held in memory over the course of execution of our experiments and never had to actually be written to disk.)

The recreation of distribution figures mentioned above allowed us to add confidence interval bars and bands.
These bootstrapped estimates of uncertainty better justify the sufficiency of sample size in these experiments.

> I'm not sure that the means in figures 2(c) and 3(c) and 6 tell us anything useful. Hash and Integer look identical, but clearly come from very different distributions, which are much more easily seen in distribution plots. If you want to discuss them compared to theoretical results specifically for Hamming metric in 3.1.3, that seems warranted, but I wouldn't include those plots. Similarly for Fig. 6, you lose all perspective on the distribution of values, which is what's important here, not their means.

We agree that contextualizing means with respect to the underlying distributions is critical (particularly when those distributions are multimodal, as some of the distributions in question are).

We have switched bar plots in Figures 2 and 3 for violin plots with mean (and bootstrapped CI) annotated.
We have added a subpanel to Figure 6 that shows distribution of match distance by mutational step as a heat map.

We do believe that mean similarity constraint (Fig 2), dissimilarity constraint (Fig 3), and match distance (Figure 6) provide reasonable and interpretable information, so long as they are considered in context of the underlying distributions.
For example, mean match distance at a mutational step gives a measure of how well you could expect a pair of previously perfectly-matched tags to match after _x_ successive mutations.

> In section 3.2 for the dissimilarity constraint, you need to be careful to specify the order of tags, since the integer metric is non-commutative. Specifically, in the description of Fig 3(a), "Then, tags were randomly drawn until a tag S1 with distance to R less than 0.01 was obtained." is not clear whether these distances are d(R, S1) or d(S1, R). Same with the next sentence. If the other way, I would imagine integer's mean match distance would be 0.99, not 0.01. [Note: this decision either way is arbitrary, so your sentence "Then, explaining this counterintuitive result, the distance from the slightly smaller to the slightly larger tag will be small." would read that the distances would be large.]

We incorporated mathematical notation into this definition (and several others) in order to resolve ambiguity about argument order with respect to metric functions.
[Diff](https://github.com/mmore500/tag-olympics-writeup/commit/b640c1f97201c632977f17f903561a66d4413de0).

> Section 4.1.1: "For both tightly- and loosely-affiliated tag pairs under the integer and bidirectional integer metrics, most mutations caused very small changes in match distance. These mutations toggle least-significant bits of the tag's integer representation." -- This doesn't make sense to me. Aren't all bits equally likely to be toggled, not least-significant bits most often? I think a better interpretation is that only the first 7 or 8 bits actually make much difference in the integer representation, since those are the only ones that affect whether tags are within 0.01 normalized distance. So, the other 24 bits have very little effect on distance, and 3/4 of changes won't be noticeable.

We corrected the second sentence to read "These mutations toggle less-significant" instead of "least-significant."

> Later in the same paragraph "Single-step mutations occasionally occured (SIC) that strongly couple loosely affiliated tag pairs or strongly decouple tightly-affiliated tag pairs". -- For the loosely-affiliated tag pairs under bidirectional integer metric, the most significant bit must differ for the pair to even be considered, correct? If so, it seems like flipping that most significant bit is the only way to go from a loose coupling to a strong coupling. I think this section should be rewritten to give a better analysis of what's going on.

Our use of "stongly: here inadvertently drew a false equivalency to "tightly/loosely."
We did not intend that "strongly" implied a match distance change surpassing a particular threshold (i.e., 0.5).

We have rephrased to use ther term "conspicuously" instead of strongly and added some numerical detail about the percentage of mutations that altered match distance by at least 0.25 match distance units.

```diff
For both tightly- and loosely-affiliated tag pairs under the integer and bidirectional integer metrics, most mutations caused very small changes in match distance.
 These mutations toggle less-significant bits of the tag's integer representation.
 However, under these metrics, a small fraction of mutations affecting more-significant bits of the integer representation have a much stronger effect.
-Single-step mutations occasionally occured that strongly couple loosely-affiliated tag pairs or strongly decouple tightly-affiliated tag pairs.
-In particular, the unidirectional integer metric appears to exhibit more frequent strong decoupling mutations than the bidirectional integer metric, presumably due to its non-commutative quirks.
+Single-step mutations occasionally occurred that conspicuously couple loosely-affiliated tag pairs or conspicuously decouple tightly-affiliated tag pairs.
+For instance, under the integer metric 3.6\% of mutations increased loosely-affiliated match closeness by at least 0.25 units and and 10.6\% decreased tightly-affiliated match closeness by at least 0.25 units.
+Under the bidirectional integer metric, these percentages were 3.3\% and 3.9\%, respectively.
+Notably, the unidirectional integer metric exhibits more frequent strong decoupling mutations than the bidirectional integer metric, presumably due to its non-commutative quirks.
```

> Fig. 8 is hard to interpret. Line graphs may be much easier to read here?

Agreed.
We have redrawn this figure as a line graph.

> Minor concerns
> ==============
>
> Some figures, including Figure 1, appear a long ways before they are mentioned in the text.

[Adjusted](https://github.com/mmore500/tag-olympics-writeup/commit/036383783375b9dcbab3a54b62b223e6dcd92269).

> I believe the definition of the Integer Metric in section 2.3 could be simplified to: `d(t,u) = ((f(u) - f(t)) mod 2^n) / 2^n`, removing the need for a piecewise definition.

We double checked this simplification with a few examples by hand and incorporated it.

> (And if you stick with the piecewise definition, I believe the bottom half is backwards, and should instead be `(f(u) - f(t)) / 2^n`, since otherwise this will be negative.)

Yes, the bottom half of the piecewise definition was backward.

> Finally, if I'm getting the math right, the bidirectional integer metric could also be simplified to: `d(t,u) = min{(f(t) - f(u)) mod 2^n, (f(u) - f(t)) mod 2^n} / 2^n`.
> I find these formulas with mod more intuitive for the "wrapping around" structure of these tag metrics.

Agreed!
Updated the definition.

> 2.4 line 7 - "bintiger" not used anywhere else.

This was a typo.
Corrected.

> You use "normalization" and "uniformification" to describe how you make the distributions more uniform. Pick one.

Agreed.
We chose "uniformification" and removed all references to normalization.
[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/45e8149d357946a67896fd09c540cfcab641f8db).

> Since Streak and Hamming seem somewhat related, it seems that it would be nice to have them next to each other in the plots.

Agreed.
Additionally, we have placed the Hash metric at the beginning of the order because it serves as a control.

> Subsections such as 3.1.1, 3.1.2, etc. don't discuss methods in the same order as the plots.
> It seems like doing so would be nice and easier to follow.

Agreed.
We reorganized these subsections (and subsequent subsections) to match the order of metrics in the plots.
[Diff](https://github.com/mmore500/tag-olympics-writeup/commit/aadb0684e0447d723f6d6afec211b0ee123b9c88).

> Fig 2(a) "First, a constraining tag R..." -- you use "target", not "constraining" in the text.
> Be consistent.

We fixed this inconsistency in this figure and another where it appeared.

> Section 3.1.4: "The streak metric retains some geometric constraint in the mean case, but allows for outliers that strongly break similarity constraint." -- Why?
> What do these outliers look like?
> This sounds interesting and I'd like more here.
>
> Section 3.3: "m(A, B) + m(B, C) − m(A, C)" doesn't seem to correspond to Fig. 4(a).
> Likely need to change one or the other.

We updated the figure to clarify correspondence with the text.

> Section 3.3 has no analysis of the integer metrics, and I'd like to see one.

Added.
```diff
+The integer and bidirectional integer metrics only exhibited shortening detours up to -0.02, which were due to minor stochastic imperfections of the uniformification process.
+As would be expected given their Euclidean basis, shortening detours were otherwise nonexistent for the integer metrics.
```

> Section 4.1 line 36 -- should this be d', not d?

That is correct.
[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/6fda702ed5feb32c7635ba29d4477d3468d65565).

> p. 19 line 4 -- "loosely-affiliated" => "tightly-affiliated"

[Corrected](https://github.com/mmore500/tag-olympics-writeup/commit/ed1760368d70bbf5c0141c018c3126847ebfea76).

> Section 4.2 -- why 65 bit flip mutations? And why only 1,000 replicates for this experiment?

We have clarified both of these points
```diff
@@ -81,10 +81,12 @@ We performed multi-step mutational analyses to characterize the broader mutation
 To conduct these mutational walks, we
 \begin{itemize}
     \item randomly generated a starting tag,
-    \item then sequentially applied 65 randomly-chosen one-step bit flip mutations to that tag (with back mutation allowed),
+    \item then sequentially applied randomly-chosen one-step bit flip mutations to that tag until a mutational saturation threshold,
     \item while recording match distance to the original starting tag at each step along the walk.
 \end{itemize}
-We analyzed 1,000 replicate mutational walks for each metric.
+Back mutation was allowed in these experiments.
+We performed 65 step mutational walks, which allowed us to include a binary order of magnitude past one expected mutation per site.
+We analyzed 1,000 replicate mutational walks for each metric, which was sufficient to distinguish metrics with bootstrapped 95\% confidence intervals

```

> Section 6 line 44 -- "streak" => "hash"

[Fixed.](https://github.com/mmore500/tag-olympics-writeup/commit/a2bb3fa68f2397d08e8755ea2649d08734d12e98)

> Other Comments
> ==============
>
> I really like the inclusion of the hash metric as a control.

Thank you!
We were trying to imagine how to invent an "antipattern" metric and were surprised by its strong performance in some regards.

> "Geometric constraint might prove useful to facilitate modularity by allowing subsets of tag space to correspond to associated functionality" -- this perhaps sounds like homology, maybe you want to label it as such?
>
> I really like Detour Difference -- earlier in the paper I was hoping you'd consider the triangle inequality.
