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
>
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
>
> The experiment in section 5.1 is very confusingly described.
> After reading it 3 times, I'm still not sure what's going on.
> This needs to be rewritten from scratch to precisely describe the experiment, what genomes are, how they are used, and what they have to do with random bipartite graphs.
>
> Moderate Concerns
> =================
>
> There are a handful of typos and English issues throughout, which need to be tightened up, especially when they say the wrong thing.
>
> The introduction does not do a good job of summarizing the results or discussion of the paper.
>
> Transition from introduction to section 2 is very abrupt. Section 2 could use a paragraph explaining what it will contain.
>
> Section 2.5 and 2.6 discuss "probability of a contiguous k-bit match" and "probability of a match distance". What does "probability" mean here?
> Of any two randomly sampled bit strings?
> Or something else? You need to explain what you mean.
>
> Section 2.5 "probability of a contiguous k-bit match between two bitstrings" -- Does this mean probability of an exact k-bit match, or of at least k-bit match (i.e. k or more contiguous bits match).
> My reading of the math is that it has to be the latter, but this should be stated clearly either way.
>
> All of the experiments in this paper use somewhat arbitrary numbers of samples.
> For example, Figure 1 uses 10,000 random pairs, where others use 5,000.
> A few remarks here:
> - None of these experiments look computationally expensive, so why not use more samples to get finer-grained detail?
> Especially for the normalization process, which is important for all remaining experiments, why not use every one of the 2^32 tags at least once, with its pair a randomly-generated tag?
> Computing the distance between all 2^64 distinct pairs of tags is likely infeasible, but I could imagine computing many more than you do.
> - On the other hand, if you think 10,000 (or 5,000) is "enough" in the sense that adding more data won't change distributions, you should make that case of why this (relatively low) threshold was chosen.
> - Finally, if considering all 2^32 tags is too many, would it be better to pick a smaller tag size like 2^16 so that you could consider all possible pairs?
> I would like to see justification of both the number of samples and the tag size.
>
> I'm not sure that the means in figures 2(c) and 3(c) and 6 tell us anything useful. Hash and Integer look identical, but clearly come from very different distributions, which are much more easily seen in distribution plots. If you want to discuss them compared to theoretical results specifically for Hamming metric in 3.1.3, that seems warranted, but I wouldn't include those plots. Similarly for Fig. 6, you lose all perspective on the distribution of values, which is what's important here, not their means.
>
> In section 3.2 for the dissimilarity constraint, you need to be careful to specify the order of tags, since the integer metric is non-commutative. Specifically, in the description of Fig 3(a), "Then, tags were randomly drawn until a tag S1 with distance to R less than 0.01 was obtained." is not clear whether these distances are d(R, S1) or d(S1, R). Same with the next sentence. If the other way, I would imagine integer's mean match distance would be 0.99, not 0.01. [Note: this decision either way is arbitrary, so your sentence "Then, explaining this counterintuitive result, the distance from the slightly smaller to the slightly larger tag will be small." would read that the distances would be large.]
>
> Section 4.1.1: "For both tightly- and loosely-affiliated tag pairs under the integer and bidirectional integer metrics, most mutations caused very small changes in match distance. These mutations toggle least-significant bits of the tag's integer representation." -- This doesn't make sense to me. Aren't all bits equally likely to be toggled, not least-significant bits most often? I think a better interpretation is that only the first 7 or 8 bits actually make much difference in the integer representation, since those are the only ones that affect whether tags are within 0.01 normalized distance. So, the other 24 bits have very little effect on distance, and 3/4 of changes won't be noticeable.
>
> Later in the same paragraph "Single-step mutations occasionally occured (SIC) that strongly couple loosely affiliated tag pairs or strongly decouple tightly-affiliated tag pairs". -- For the loosely-affiliated tag pairs under bidirectional integer metric, the most significant bit must differ for the pair to even be considered, correct? If so, it seems like flipping that most significant bit is the only way to go from a loose coupling to a strong coupling. I think this section should be rewritten to give a better analysis of what's going on.
>
> Fig. 8 is hard to interpret. Line graphs may be much easier to read here?
>
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
We chose ``uniformification'' and removed all references to normalization.
[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/45e8149d357946a67896fd09c540cfcab641f8db).

> Since Streak and Hamming seem somewhat related, it seems that it would be nice to have them next to each other in the plots.
>
> Subsections such as 3.1.1, 3.1.2, etc. don't discuss methods in the same order as the plots.
> It seems like doing so would be nice and easier to follow.
>
> Fig 2(a) "First, a constraining tag R..." -- you use "target", not "constraining" in the text.
> Be consistent.
>
> Section 3.1.4: "The streak metric retains some geometric constraint in the mean case, but allows for outliers that strongly break similarity constraint." -- Why?
> What do these outliers look like?
> This sounds interesting and I'd like more here.
>
> Section 3.3: "m(A, B) + m(B, C) − m(A, C)" doesn't seem to correspond to Fig. 4(a).
> Likely need to change one or the other.

We updated the figure to clarify correspondence with the text.

> Section 3.3 has no analysis of the integer metrics, and I'd like to see one.
>
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
We were trying to imagine how to invent an "anti" metric and were surprised by its strong performance in some regards.

> "Geometric constraint might prove useful to facilitate modularity by allowing subsets of tag space to correspond to associated functionality" -- this perhaps sounds like homology, maybe you want to label it as such?
>
> I really like Detour Difference -- earlier in the paper I was hoping you'd consider the triangle inequality.
