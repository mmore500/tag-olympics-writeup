Thank you for your detailed review and feedback, which has been greatly helpful in improving the quality of our manuscript.

We have returned our response to reviewer comments in plain text format.
However, a nicer-to-read rendered version of our responses to your feedback may be viewed at the following url,

https://github.com/mmore500/tag-olympics-writeup/blob/master/review/reviewer-3.md

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

> The idea of using tags in evolving systems has been around for a long time, under various names, and is common in sub-fields of GP such as gene regulatory models - so it's definitely something that could use more investigation and understanding.
> This paper reports a well-considered analysis of the role of metrics within binary tag matching, beginning with a geometric analysis of the tag spaces under different distance metrics, then under the action of a simple mutation operator, and finally under selection.
>
> It would good to add some justification for focusing on binary-valued, rather than, for instance, real-valued tags.
> Though I appreciate there's only so much you can cover in one study, I'd be interested to hear if you have any thoughts on the role of tag alphabet, e.g. whether you'd expect any differences in terms of evolvability for binary vs multi-valued vs continuous.

This is a good point, and something that will certainly be important to explore in the future.
We added a supplementary section to offer some thoughts on this subject.

```diff
+For tractability and consistency, this work exclusively considers strings composed from the binary alphabet $\{0, 1\}$.
+However, we expect that most geometric, variational, and evolutionary properties of the metrics studied are not fundmaentally tied to the particular use of the binary alphabet.
+
+We suspect that the surveyed integer metrics under the existing bitstring representation should behave effectively indistinguishably from a continuous-valued (i.e., floating point) representation.
+Due to the uniformification process performed, both would be effectively rescaled to the range $\[0, 1\]$.
+With a precision of $1/2^{32}$ --- tighter than $10^{-9}$ --- the 32-bit tags used should exhibit near-undetectable granularity, especially given the relatively small pools of query and operand tags used in our experiments.
+
+However, it is important to note that the bit flip mutation operator used in our experiments induces a roughly exponential distribution of mutation effect size, which might otherwise be an unusual choice when working with a continuous-valued tag system.
+We unpack this issue in greater detail in Section \ref{sec:variational}.
+
+Alternate alphabet choice would have a minimal effect on the streak metric.
+Imagine, for example, using a four-valued alphabet instead of the existing two-valued binary alphabet.
+Any character in that four-valued alphabet could be encoded by a pair of binary digits.
+So, the existing bitstring representation for tags could be preserved and adjustment instead made to the match distance metric to count only entirely-matching (or mismatching) pairs of bits as contributing to a streak.
+The significance of this effect would depend on typical streak length and, of course, for large alphabets this truncation effect would eventually become overwhelming.
+
+Increased alphabet size might have a more nuanced effect on the Hamming metric.
+Under the binary alphabet, every mutation affects a tag's match distances to all other tags --- no mutation is neutral.
+However, with a larger alphabet size this would no longer be the case.
+As with the streak metric, increased alphabet size would introduce effects from coarsened granularity, with the magnitude of these effects eventually becoming overwhelming under large alphabets.

+We do not fully explore the possibilities introduced by alternate tag-matching representations in this work, so a detailed and rigorous understanding of this topic remains an avenue for future research.
```

We added an aside in the Metrics section to note other possible tag representations and refer interested readers to this new section in the supplement for more detail.

> The introduction to the metrics would benefit from some concrete examples, and I think more examples in general would help engage readers who are less familiar with tag systems.

We added a brief worked example of each tag metric's distance computation.

> A line plot might work better for Fig 6.
> It's hard to see and compare the trends at the moment.

We have replaced the figure in the main text with a line plot.

> Although I see you've included the full results in supplementary material, it would be worth mentioning the optimal mutation rates in Section 5.1 (as you do in Section 5.2), since these rates contain relevant information.
> Perhaps you could also comment on the fact that the optimal mutation rate for different metrics can differ by an order of magnitude, and maybe say something about the relative importance of optimising this hyperparameter.

We added a table with the best-performing mutation rates for each metric on the different target-matching problems and added a note summarizing across the surveyed metrics and emphasizing variability in optimal mutation rates between metrics

```diff
+ Best-performing mutation rates varied greatly, ranging from as low as 0.75 expected mutations per genome for the hash metric to as high as 8.0 for the integer metrics.
+ The hamming and streak metrics intermediate best-performing mutation rates between 1.5 and 4.0 expected mutations per genome.
```

> Was there any dependence between the choice of seed program and the performance of the metrics?
> For instance, the optimal balance between exploration and exploitation may be determined in part by program size.

We were also interested by this question, and performed an additional experiment with the target graph-matching system to investigate.
For this additional experiment, we initialized genomes with all-identical tags rather than randomly-generated tags.
This condition increases initial pressure to explore and differentiate.

We found that the integer metrics had significantly faster initial adaptive evolution under these conditions, indicating a greater capacity to generate useful initial variation through mutation.
However, this effect only lasted for about the first 10 generations.
We have added some commentary on this experiment to the text.

> Something you've not explored is the relationship between tag representation/metric and operator(s).
> For example, if you use a representation/metric with poor exploration properties, can this be adequately compensated for by using a more explorative operator?
> Or is there some more fundamental benefit to using a particular representation/metric?

A metric's geometric constraint properties will remain no matter what mutation operator is used.
For example, under the bidirectional integer metric it will be impossible to have tag `A` match closely to tags `a` and `b` while having  tag `B` match closely to `a` but very poorly to `b`.

We have added some commentary to the section on geometric constraint to emphasize this point.
```diff
+Geometric constraint cannot be circumvented by mutation operator design.
+In both the Hamming metric and bidirectional integer metric examples above, the nonexistence of any tag $v$ satisfying the given match distance criteria holds no matter how mutation is performed.
+The mutation operator only affects how tags in a genome move through bitstring space between generations and not how they match to other tags at a particular generation, so is orthogonal to geometric constraint.
```

> You might find the following paper of relevance, since it also explored the evolvability of tag-based representation in GP, and (relevant to my previous point) explored the idea that getting the representation right is more important than getting the operator right: <http://doi.org/10.1016/j.biosystems.2004.05.015>
>
> Given that you've only looked at a small number of problems, it's unclear whether the observations will generalise.
> I suspect, as with most things, there is going to be a degree of No Free Lunch here, i.e. different representations/metrics will work well for different problems.
> Maybe something worth discussing.
> That said, I like your brief point about tuning tag systems in the conclusions; perhaps this could be emphasised a bit more, since you have shown that getting the right setup for a particular problem is important.


We agree that this is an interesting point to expand on.
We added a subsection on practical recommendations to the end of our discussion.

```diff
+\subsection{Practical Recommendations}
+
+Our results highlight the dependence of tag-matching metrics' performance on problem domain.
+So, consideration of properties of the problem domain at hand should drive the decision of which tag-matching metric to use in a particular system.
+
+A major practical advantage of integer-based metrics is the possibility for log time lookup of operands (i.e., via binary search).
+However, the integer metrics performed competitively only within a particular class of problem domains.
+In all high-constraint problem domains, the integer metrics performed poorly.
+Among low-constraint problem domains, the integer metrics only performed well on the toy graph-matching task --- they did not perform well on the low-constraint GP changing signals task.
+At present, it is unclear what problem domain property stymied the integer metrics in the low-constraint GP changing signals task --- the potential for the operand set to grow over time (e.g., duplication and divergence), fitness landscape ruggedness (e.g., epistasis), or something else.
+
+However, within the low-constraint target-matching problem domain where integer metrics performed well (i.e., a low-constraint, fixed-size problem with a smooth fitness landscape), the hash metric actually performed slightly better.
+So, under these conditions, the hash metric may be preferable when log time lookup is not critical.
+
+Outside the low-constraint target-matching problem domain, the Hamming and streak metrics perform significantly better than the integer metrics.
+On the low-constraint target-matching problem, both did evolve full solutions, although the Hamming metric was slightly slower than the streak and integer metrics.
+In particular, the Hamming and streak metrics performed best in our GP tests.
+So, both metrics appear to be reasonable choices in most cases.
+
+Choosing between the two will likely depend on implementation considerations: the streak metric facilitated faster adaptive evolution in some experiments, but is more computationally expensive to calculate than the Hamming metric.
+Future work should investigate whether a simplified version of the streak metric --- for example, ignoring mismatching streak length and only considering matching streak length --- suffices to capture its properties.
```

> Typos etc:
>
> hamming => Hamming

[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/3f2173f756c4218a77704969b07a1606b7609813).

> Fig {2,3,4,...}c caption: ranging form => ranging from

[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/a77a4cc7272d0edc498fbf238f5b09113da50944).

> "neither involved in neither the longest-matching streak nor" - those double-negatives are confusing!

[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/5dfad3f373f5a43318e110fc836c4cfd67f4e212).

> mulit-step => multi-step

[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/f89df168d89463cca77b1cf1bab5361c18840f48).

> "harvested its operand tags placed them": I think there's an "and" missing there.

[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/bfe9cd36c33b4b571da18d695c6da037d472037c).

> with respect tag-matching criteria => with respect to tag-matching criteria

[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/a990d2b1463684790ac0e45d12cd19548d6607dd).
