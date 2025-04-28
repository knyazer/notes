### Notions of robustness in machine learning

The issue with current models is that they are capable of perfectly partitioning a randomly labeled dataset. As in, if we train a model on imagenet, but shuffle labels, the model will still reach ~100% accuracy on the training set (and, trivially, ~0.1% accuracy on the test set)

The ultimate aim of the "inductive biases" research is to make sure that the model with shuffled labels **doesn't** learn anything in such a scenario, we wish for performance on the train set to match the performance on the test set. This can also be viewed as minimizing generalization gap.

The point of robustness is different, though similar.

We wish to make models that generalize, but instead of using "biases", the aim is to use different data-generating process assumption.

Thus, different notions of robustness arise. The most trivial ones are:
* Robustness to adversarial dataset corruption
* Robustness to iid dataset corruption

Dataset corruption is when our training dataset is "corrupted" by either an adversary or an iid process, which either shuffles the labels or generates totally new image-label pairs. We will talk about shuffling-labels corruption. 

Adversarial corruption is when the worst-possible scenario is chosen: the labels are chosen such that the learning algorithm is confused as much as possible. One can see that dealing with adversarial corruption of 51% (51% of labels are modified) is impossible: an adversary can make our algorithm behave "arbitrarily" even if we use the "best" method out there. The adversary could just change the label of 51% of data of each class to a different class, and since label are assigned to our taste ("doplhin" could be label 32 as well as 874), there is no way to distinguish 51% corruption of 32 with 874 from a 49% corruption of 874 with 32.

While robustness to adversarial corruption is useful, it is not extremely useful: we believe a more useful robustness framework is an "expected" robustness framework. Assume that the "true" data-generating process is corrupted by some iid modification, we wish to have a high chance of our model performing very well on the true data-generating process. 

Definition of "robustness to iid corruption". We say that a learning method is robust to iid corruption of \alpha% with probability of \beta% if given a corrupted dataset D, such that \alpha% of the labels in D are replaced according to an unknown distribution C, there is a probability of \beta% for our method to converge to the same solution as if trained on D without the corrupted part.

What this means is simple: we wish for our algorithm to "reject" certain samples that look too "unrealistic". This goal is exceptionally similar to the goal stated in the beginning: such a method should reject **most** of the samples from a randomly labelled dataset, thus leading to a pretty low accuracy on the training set. While "adversarial" robustness formulation allows us to drop the training accuracy to at most 51% (as in, the limit of performance of any variant of our method; infinitely-wide neural network, for example), the iid robustness allows us to drop performance to _almost_ 0.

One of the problems with the formulation we are using is the "probability of \beta% ...": we are trying to estimate this probability over all possible corruptions, which requires a prior on corruptions.

### Deep learning implications

When we have a network that is trained to e.g. classify images, what does it mean to be robust?

Our intuition is simple: the "robustness" is a tradeoff between an expected generalizibility of the solution, and the "performance" of the solution on the train set. A very simple example: if our dataset has 99% entities classified as "A" and 1% of entities classified as "B", should our method classify everything as "A"? The answer depends on a particular application.

Returning "A" always is an _extremely_ generalizable solution: the solomoff induction prior for such a solution is incomprehensibly higher than actually classifying stuff. Sometimes we might wish to always answer "A", and sometimes "B".

There is an argument against such a model: if we believe the best model is to classify everything as A, we won't train our model to do that; we will just make a function that always returns 1. However, we dismiss this argument as a survivorship bias. 

Anyway, the proxy for solomonoff induction prior could be wideness of the optimum found by gradient descent. The wider the optimum - the higher the prior is.


### Revisiting distributional robustness

The problem with distributional robustness is that we rarely know how the "noise" will look. In these cases "adversarial" robustness is the best bet: we wish to deal with the worst possible corruption. If the shape of the corruption is known, or at least can be approximated, the combination of the two could be the best. For example, with image datasets we expect to have most of the errors come from "confusions": in imagenet it is sometimes the case that images of two classes are present in an image, but the assigned class is only one of the two.

### Designing for adversarial robustness

Alright. Now, we hopefully agree that "adversarial" robustness is a more universal framework. How to quantify it? In robust statistics, one of the most common measures are "breakdown points": how much data the adversary needs to alter to "break" the algorithm; to modify the "performance" on the test set to be any one it wishes. If we are talking about label shuffle, it is not trivial whether current methods are robust. For adding whole new samples, it is trivial that current methods are "extremely" not robust: we need to add a copy of test set element with a desired label to achieve any desired level of performance on the test set. This is not extremely useful criterion nonetheless.

There are several aims people can have with their methods: they might wish to defend against distributional shifts (as in, defend against "low performance on all classes"), defend against discrimination (defend against always missclassying a particular class), or defend against missclassifying a particular sample (privacy perspective).

Here we will start by considering distributional shifts: the adversary aim is to lower performance against an iid (unknown to the adversary) sample from the true data generating process. And our aim is to defend against it. Adversary can add samples to our training set, but cannot delete them. Adversary strength of 5% corresponds to adversary adding 5% (input, output) pairs to the training set. Trivially, modern methods of training neural networks are not robust even to epsilon adversary: one has just make an infinite-loss sample. This is easy in regression problems (and can be trivially resolved by using an M-estimator instead of an MLE). I might make it a full section, but I am more interested in classification: it is not trivial to come up with a (input, output) pair for a classification task that would have an infinite loss. So, the question of how much corruption is needed to break down an algorithm is interesting.

One possible way, if one knows the weights of the model when it accesses our "adversarial" sample, we can make the loss near-infinite, by matching the activations exceptionally well. However, the SGD samples data i.i.d. so there is no way to do it. One trivial way to look at what is possible, is to run a meta-optimization procedure: optimize an (input, output) pair such that test performance is as low as possible in expectation. The fun part is to also check if it works with lower model capacity: I imagine a plot, with "model capacity" on one axis, and "test performance" as color, and "number of adversarial samples" on another axis. We believe that highly expressive models will be able to pass such a test: performing well both on test and train, but such samples spend a "bunch" of model capacity. So making the most "distinct" (input, output) pairs might be an interesting way.

Nelson 2008 shows that altering 1% of labels on spam filter emails can make them totally useless.

Bennouna 2023 studies talks about "evasion" and "poisoning" corruptions; we don't believe in "evasion" corruption, and focus on "poisoning". 

Fun note: modern robust methods are totally unusable :)) they are too slow, have too low perf, etc etc
