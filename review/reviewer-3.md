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

> The idea of using tags in evolving systems has been around for a long time, under various names, and is common in sub-fields of GP such as gene regulatory models - so it's definitely something that could use more investigation and understanding.
> This paper reports a well-considered analysis of the role of metrics within binary tag matching, beginning with a geometric analysis of the tag spaces under different distance metrics, then under the action of a simple mutation operator, and finally under selection.
>
> It would good to add some justification for focusing on binary-valued, rather than, for instance, real-valued tags.
> Though I appreciate there's only so much you can cover in one study, I'd be interested to hear if you have any thoughts on the role of tag alphabet, e.g. whether you'd expect any differences in terms of evolvability for binary vs multi-valued vs continuous.
>
> The introduction to the metrics would benefit from some concrete examples, and I think more examples in general would help engage readers who are less familiar with tag systems.
>
> A line plot might work better for Fig 6.
> It's hard to see and compare the trends at the moment.

We have replaced the figure in the main text with a line plot and moved the existing bar plot to the supplement.

> Although I see you've included the full results in supplementary material, it would be worth mentioning the optimal mutation rates in Section 5.1 (as you do in Section 5.2), since these rates contain relevant information.
> Perhaps you could also comment on the fact that the optimal mutation rate for different metrics can differ by an order of magnitude, and maybe say something about the relative importance of optimising this hyperparameter.

We added a table with the best-performing mutation rates for each metric on the different target-matching problems and added a note summarizing across the surveyed metrics and emphasizing variability in optimal mutation rates between metrics

```diff
+ Best-performing mutation rates varied greatly, ranging from as low as 0.75 expected mutations per genome for the hash metric to as high as 8.0 for the integer metrics.
+ The hamming and streak metrics intermediate best-performing mutation rates between 1.5 and 4.0 expected mutations per genome.
```

> Was there any dependence between the choice of seed program and the performance of the metrics?
> For instance, the optimal balance between exploration and exploitation may be determined in part by program size.
>
> Something you've not explored is the relationship between tag representation/metric and operator(s).
> For example, if you use a representation/metric with poor exploration properties, can this be adequately compensated for by using a more explorative operator?
> Or is there some more fundamental benefit to using a particular representation/metric?
>
> You might find the following paper of relevance, since it also explored the evolvability of tag-based representation in GP, and (relevant to my previous point) explored the idea that getting the representation right is more important than getting the operator right: <http://doi.org/10.1016/j.biosystems.2004.05.015>
>
> Given that you've only looked at a small number of problems, it's unclear whether the observations will generalise.
> I suspect, as with most things, there is going to be a degree of No Free Lunch here, i.e. different representations/metrics will work well for different problems.
> Maybe something worth discussing.
> That said, I like your brief point about tuning tag systems in the conclusions; perhaps this could be emphasised a bit more, since you have shown that getting the right setup for a particular problem is important.
>
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
>
> with respect tag-matching criteria => with respect to tag-matching criteria

[Fixed](https://github.com/mmore500/tag-olympics-writeup/commit/a990d2b1463684790ac0e45d12cd19548d6607dd).
