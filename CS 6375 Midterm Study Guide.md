# CS 6375 Midterm Study Guide

Oct 15, 2026 · @Nazia Shehnaz Joynab

The midterm is on Oct 20, 2026 at 8:30am in class: multiple choice plus long answer, one double-sided cheat sheet, basic calculator. Each section explains one topic in plain language, then gives short-answer questions and MCQs. Answers sit at the end of each section so you can cover them while you practice.

## 1. ML basics: flavors of learning and the supervised setup

Machine learning means a program learns a rule from examples instead of you writing the rule by hand. The lecture's warm-up: try writing a rule that tells a handwritten 3 from an 8. Every rule you write has a counterexample, so you let the computer find the rule from labeled examples.

### Explanation

**Mitchell's definition.** A program learns from experience E on a task T, measured by performance P, if its performance on T (as measured by P) improves with E. The exam can ask you to name T, P and E for any example:

| Example | Task T | Performance P | Experience E |
| --- | --- | --- | --- |
| Spam filter | label email spam or legitimate | probability of misclassifying | labeled emails |
| Movie ratings | predict how much a user likes a film | gap between prediction and true rating | previous ratings |
| Clustering | split data into distinct groups | closeness to the underlying structure | the data points |
| Language model | predict the next token | perplexity | large text datasets |
| Aircraft control | keep flying, steer to a goal | reward for reaching the goal quickly | state/action/reward from earlier flights |

**The three flavors.**

- **Supervised learning**: examples come with answers (labels). Collect {data, labels} as a training set, train a model to match them, then test on new data. Examples: indoor/outdoor images, spam detection, house prices.
- **Unsupervised learning**: no answers. The goal is to find something: structure or hidden information. Examples from the lecture: clustering, generative image models (StyleGAN), language models.
- **Reinforcement learning**: an agent acts in an environment and receives rewards. It trains a policy to maximize reward, then is deployed in a new environment. Examples: Atari games, flying an aircraft.

**Formal supervised setup.** Four objects:

1. Instances X: the set of possible inputs.
2. An unknown target function f: X → Y that produces the true label.
3. A hypothesis class H: the set of models you are willing to consider (for example all linear functions).
4. A training set {(x⁽¹⁾, y⁽¹⁾), …, (x⁽ⁿ⁾, y⁽ⁿ⁾)} where y⁽ⁱ⁾ = f(x⁽ⁱ⁾).

The goal is a model h in H that approximates f as well as possible.

**Hypothesis class versus hypothesis.** The class is the blueprint, such as "linear functions f(x) = wᵀx + b". The hypothesis (or model) is one specific member that training picked, such as w = (2, −1), b = 0.5. Most people just say "model" for the second.

**Hyperparameters versus parameters.** Parameters (weights) are learned by training. Hyperparameters (k in kNN, tree depth, learning rate, λ) are chosen by you, usually with a validation set.

**Feature types (input space).**

| Type | Meaning | Example |
| --- | --- | --- |
| nominal (incl. Boolean) | no ordering | color ∈ {red, blue, green}, awarded ∈ {True, False} |
| ordinal | totally ordered | size ∈ {small, medium, large} |
| numeric | real-valued | height ∈ \[0, 100\] inches |
| hierarchical | partially ordered in a tree | shape: polygon → square, triangle; closed → circle |

If all features are numeric, each instance is a point in d-dimensional space, where d is the number of features. A 32×32 RGB image has 3 × 32 × 32 = 3072 features. A 28×28 grayscale image has 28 × 28 = 784. Grayscale has one channel, color has three.

**Output space.** Discrete labels mean classification (binary or multiclass). Continuous labels mean regression.

**Empirical risk minimization (ERM).** Pick the hypothesis with the smallest average loss on the training set:

```
h* = argmin over h in H of (1/n) Σᵢ L( h(x⁽ⁱ⁾), y⁽ⁱ⁾ )
```

ERM tells you how well a model fits the training data. That is not the real goal. The real goal is generalization: doing well on future, unseen data. The theory assumes the training instances are i.i.d. (independent and identically distributed). Time series break this because each point depends on earlier ones.

**Batch versus online learning.** Batch: you receive all instances at once. Online: instances arrive one after another and you update the model as they come.

**The supervised pipeline.**

1. Setup: get data, clean it (drop irrelevant features, standardize), choose a model class.
2. Train: choose hyperparameters, fit using training data and labels only.
3. Validate (optional): use a validation set as a mini test set; if poor, adjust hyperparameters and retrain.
4. Test: pick a suitable metric, measure error on the test set, then deploy.

### Common traps

- "Supervised learning aims for the highest training accuracy." False. It aims to generalize.
- "Time-series points are i.i.d." False. They depend on earlier points.
- "Hypothesis class = the trained model." False. The class is the family, the hypothesis is the trained member.
- Language models are listed under unsupervised learning in this course, even though they predict a next token.

### Practice: short answer

1. Name the task T, performance measure P and experience E for handwritten-digit recognition.
2. Explain the difference between a hypothesis class and a hypothesis, using linear models as the example.
3. A dataset has these features: color ∈ {red, blue}, size ∈ {S, M, L}, weight in kg, and shape ∈ {square, triangle, circle} organized under "polygon" and "closed". State the feature type of each.
4. Why is minimizing training error not the main objective of supervised learning? Name the assumption that theory about generalization usually needs, and one data type where it fails.

### Practice: MCQ

**Q1.** Which of these is NOT a supervised learning task?

- A. Grouping customers into segments with k-means, with no labels
- B. Predicting house prices from past sales
- C. Detecting spam email from labeled messages
- D. Recognizing handwritten digits from labeled images

**Q2.** After flattening, what is the feature dimension of one 28 × 28 grayscale image?

- A. 2352
- B. 784
- C. 256
- D. 28

**Q3.** Which feature is ordinal?

- A. Temperature in °C
- B. Eye color ∈ {brown, blue, green}
- C. Has\_pet ∈ {True, False}
- D. T-shirt size ∈ {S, M, L, XL}

**Q4.** In kNN, which of the following is a hyperparameter?

- A. The training labels
- B. The distance between two points
- C. The value of k
- D. The predicted label for a test point

**Q5.** Are these statements true? (i) Instances in a time series are i.i.d. (ii) The primary goal of supervised learning is to generalize to unseen data.

- A. False, False
- B. False, True
- C. True, False
- D. True, True

**Q6.** Predicting a patient's blood pressure (a real number) from other measurements is an example of:

- A. Binary classification
- B. Clustering
- C. Multiclass classification
- D. Regression

### Answers

**Short answer**

1. T: classify an image of a digit into 0–9. P: probability of misclassifying (error rate). E: labeled digit images (such as MNIST).
2. The class is the blueprint, for example all functions f(x) = wᵀx + b. A hypothesis is the single function you get after training, with specific numbers filled in, such as w = (2, −1), b = 0.5.
3. color: nominal. size: ordinal (S < M < L). weight: numeric. shape: hierarchical.
4. Because the model will be used on new data, and a model can fit the training set perfectly (by memorizing) yet do badly on new data. Theory usually assumes the training instances are i.i.d. from the same distribution as future data. Time series violate it because points depend on earlier points.

**MCQ**

1. A. Clustering has no labels, so it is unsupervised.
2. B. 28 × 28 × 1 = 784.
3. D. Sizes have a natural total order. Eye color and Boolean are nominal. Temperature is numeric.
4. C. k is chosen by you, not learned from data. Labels are data, and the prediction is an output.
5. B. (i) is false because time-series points depend on each other. (ii) is true.
6. D. A continuous output means regression.

## 2. k-Nearest Neighbors (kNN)

kNN predicts a label by looking at the labels of the training points closest to the query. The idea is that nearby feature vectors tend to share a label. There is no training step: the "model" is the stored training set, which is why kNN is called instance-based and non-parametric.

### Explanation

**The algorithm.**

- **1-NN**: find the single nearest training point and return its label. The decision regions form a Voronoi diagram: each cell holds the points that are closer to one training point than to any other.
- **kNN classification**: find the k nearest training points and return the plurality (most common) class.
- **kNN regression**: find the k nearest training points and return the mean of their labels.
- k is a hyperparameter you choose.

```
classification:  ŷ = argmax over y of  Σ (i = 1..k)  1( y = y⁽ⁱ⁾ )
regression:      ŷ = (1/k) Σ (i = 1..k)  y⁽ⁱ⁾
```

**Distances.** "Nearby" needs a distance:

```
Hamming (discrete features):  d = number of features where x and x' differ
Euclidean (L2):               d = ( Σₐ (xₐ − x'ₐ)² )^(1/2)
Manhattan (L1):               d = Σₐ | xₐ − x'ₐ |
```

For mixed feature types, add the per-feature distances, and normalize each so no feature dominates (for example scale each to a maximum of 1).

**Standardization.** Distances are sensitive to scale: a feature measured in the tens of thousands drowns out one measured in tens. The recipe:

1. On the training set, compute each feature's mean μₐ and standard deviation σₐ, where σₐ = ( (1/n) Σ (xₐ − μₐ)² )^(1/2).
2. Replace each value with x̃ₐ = (xₐ − μₐ) / σₐ.
3. Do the same for test points, using the training μ and σ (never statistics from the test set).

Example: ages 20, 30, 40, 50, 60 have μ = 40 and σ = √200 ≈ 14.14. The age 60 becomes (60 − 40) / 14.14 ≈ 1.41.

**Choosing k.**

- Small k (k = 1): jagged boundary, follows noise, training error is zero (unless identical points have different labels). Low bias, high variance, overfits.
- Large k: smooth boundary, averages over many points. At k = n it predicts the majority class everywhere. High bias, low variance, underfits.
- So "large k leads to overfitting" is false. Pick k with a validation set or cross-validation.

**Weighted kNN.** Let closer neighbors count more. Use any weight that shrinks as distance grows: w = 1/d², w = exp(−d), or w = 1 (which is plain kNN). Classification adds up the weights per class; regression uses ŷ = Σ wᵢ yᵢ / Σ wᵢ.

**Strengths and weaknesses.**

| Strengths | Weaknesses |
| --- | --- |
| Easy to explain a prediction ("these are the neighbors") | Sensitive to irrelevant and correlated features |
| Simple to implement | Prediction is expensive: compare against every training point |
| No training | No compact model to interpret |
| Often good in practice | Needs standardization and a sensible distance |

An irrelevant feature hurts because it adds distance that has nothing to do with the label, so the nearest neighbors stop being the same-class points.

### Worked example 1 (lecture quiz): 3-NN classification

Training points: (0,0)+, (1,0)+, (2,0)+, (2,2)+, (0,1)−, (0,2)−, (1,2)−, (3,1)−. Use Euclidean distance and k = 3.

```
Query A = (1,1): distances
  (0,1) = 1   label −
  (1,0) = 1   label +
  (1,2) = 1   label −
  next closest is (0,0) at √2 ≈ 1.41, so it is not in the top 3
  votes: −, +, −  →  A = −

Query B = (2,1): distances
  (2,0) = 1   label +
  (2,2) = 1   label +
  (3,1) = 1   label −
  votes: +, +, −  →  B = +
```

### Worked example 2: regression and weighting

Training (x, y): (1, 10), (4, 20), (6, 30), (10, 50). Predict at x = 5 with k = 3.

```
distances to 5:  1→4,  4→1,  6→1,  10→5
the 3 nearest:   x = 4 (y 20), x = 6 (y 30), x = 1 (y 10)
plain mean:      (20 + 30 + 10) / 3 = 20
weights 1/d:     1, 1, 1/4
weighted mean:   (20·1 + 30·1 + 10·0.25) / (1 + 1 + 0.25) = 52.5 / 2.25 ≈ 23.33
```

The weighted answer is higher because the far point (y = 10) counts for less.

### Common traps

- Standardizing the test set with the test set's own mean and standard deviation. Use the training statistics.
- Believing large k overfits. It is small k that overfits.
- A weight that grows with distance (such as w = d or w = d^(1/2)) is not appropriate, because far points would count more.
- kNN has hyperparameters (k, the distance, the weights) even though it is non-parametric.

### Practice: short answer

1. Describe the kNN prediction procedure for classification and for regression. What happens during "training"?
2. Income runs from 0 to 200,000 and age from 20 to 60. Which feature dominates Euclidean distance if you do nothing? How do you fix it, and which mean and standard deviation do you use for a test point?
3. Explain how bias and variance change as k grows, and state the training error of 1-NN.
4. Give two strengths and two weaknesses of kNN.
5. Training points: (0,0)−, (1,0)+, (0,1)+, (3,3)−, (4,3)−. Using Euclidean distance, predict the label of the query (1,1) with k = 1, k = 3 and k = 5.

### Practice: MCQ

**Q1.** How should you standardize features for kNN?

- A. Standardization is never needed for kNN
- B. Compute the mean and standard deviation on the test set and use them for test points
- C. Standardize the training set only
- D. Compute the mean and standard deviation on the training set and apply them to both training and test points

**Q2.** Which weight is NOT appropriate for distance-weighted kNN, where d is the distance to the neighbor?

- A. w = d²
- B. w = 1/d
- C. w = exp(−d)
- D. w = 1

**Q3.** The decision regions of 1-NN are:

- A. A single straight line
- B. Circles around each class mean
- C. A Voronoi diagram, one cell per training point
- D. Axis-parallel rectangles

**Q4.** As k grows from 1 toward n, which statement is correct?

- A. Bias decreases and variance increases
- B. The boundary becomes smoother, and at k = n every query gets the majority class
- C. Training error always decreases
- D. Prediction becomes slower and slower

**Q5.** Which is a weakness of kNN?

- A. It needs a long training phase
- B. It cannot do regression
- C. It cannot handle more than two classes
- D. It is sensitive to irrelevant features

**Q6.** In 3-NN regression the three nearest neighbors have labels 10, 20 and 30. The prediction is:

- A. 20
- B. 10
- C. 30
- D. 60

**Q7.** What is the Hamming distance between (red, S, round) and (red, M, square)?

- A. 0
- B. 1
- C. 2
- D. 3

### Answers

**Short answer**

1. Classification: compute the distance from the query to every training point, take the k closest, return the most common label. Regression: same neighbors, return the mean of their labels. Training just stores the data.
2. Income dominates because its numbers are thousands of times larger. Standardize each feature as (x − μ)/σ. For a test point use the μ and σ computed on the training set.
3. Small k: low bias, high variance (overfit). Large k: high bias, low variance (underfit). 1-NN has zero training error, because each training point is its own nearest neighbor.
4. Strengths: easy to explain, no training, simple, often good. Weaknesses: sensitive to irrelevant and correlated features, expensive at prediction time, no interpretable model.
5. Distances from (1,1): (1,0) = 1, (0,1) = 1, (0,0) = √2 ≈ 1.41, (3,3) = √8 ≈ 2.83, (4,3) = √13 ≈ 3.61. k = 1: the two nearest are tied at 1 and both are +, so +. k = 3: +, +, − gives +. k = 5: +, +, −, −, − gives −. The prediction changes with k.

**MCQ**

1. D. Always use training statistics.
2. A. w = d² grows with distance, so far points would count more. The others shrink with distance, and w = 1 is plain kNN.
3. C. Each cell holds the points closest to one training point. Rectangles describe decision trees.
4. B. Larger k smooths the boundary. A is the wrong direction, C is wrong because training error generally rises with k (1-NN has zero), and D is wrong because prediction cost is dominated by computing distances to all training points, which does not depend on k.
5. D. A is false (kNN has no training phase), B is false (kNN does regression), and C is false (kNN handles many classes).
6. A. The mean of 10, 20, 30 is 20.
7. C. Shape and size differ at two positions: S vs M and round vs square, so the distance is 2.

## 3. Decision trees and information gain

A decision tree asks one question about one feature at each internal node, and each leaf gives the answer (a class, or a number for regression). To classify, walk from the root to a leaf. Trees are learned greedily: at each node, choose the split that removes the most uncertainty, measured by entropy.

### Explanation

**Structure.** An internal node tests a feature: a value for categorical features, or a threshold for continuous ones (x₂ > 4). A leaf holds the prediction. In the lecture's play-tennis tree the instance (Sunny, Normal, Strong) ends at a "yes" leaf.

**Geometry.** A tree splits the feature space into axis-parallel rectangles. Each leaf is one rectangle. A diagonal boundary needs many small steps.

**What it can represent.** Any Boolean function. For Boolean features:

- Y = X₂ AND X₅: test X₂. If X₂ = false, output false. If true, test X₅ and output its value.
- Y = X₂ OR X₅: test X₂. If X₂ = true, output true. If false, test X₅ and output its value.
- Y = (X₂ AND X₅) OR (X₃ AND X₁) needs more nodes, because the two conjunctions are tested in separate branches.

The tree for a function is not unique, and smaller trees are preferred: they cost less to store and to run, and simpler models usually generalize better. Finding the smallest tree that fits the training set is NP-hard (Hyafil and Rivest, 1976), so we use a greedy heuristic based on information theory.

**Entropy** measures uncertainty. Class probabilities are the class proportions in the node.

```
H(Y) = − Σᵧ P(y) log₂ P(y)

pure node (all one class):      H = 0
50/50 binary node:              H = 1 bit
```

**Conditional entropy** is the uncertainty left after you know the feature. It is the average of the child entropies, weighted by how much data goes to each child:

```
H(Y | X) = Σₓ P(X = x) · H(Y | X = x)
```

(Sanity check from the lecture notes: if Y = X, every H(Y | X = x) is 0.)

**Information gain** of splitting on X, and the rule: pick the feature with the largest gain.

```
InfoGain(D, X) = H(Y) − H(Y | X)
```

**Learning loop.** Start with all training data at the root. If a stopping rule holds, make a leaf. Otherwise pick the split with the largest information gain, divide the data, and repeat on each child.

**Stopping rules.** Stop when all instances in the node share one class, or when there are no candidate splits left. You can also stop earlier (small node, one class dominates, or a statistical test says the split is not justified). You should not keep splitting down to one data point per leaf, because that memorizes noise. A feature can be used again deeper in the tree (continuous features can be split again at a different threshold). You do not split on every feature.

**Variations.** Probability estimation trees: leaves report class probabilities, for example P(pos) = 0.8. Regression trees: leaves hold the average label, or a linear function. Ensembles: random forests and XGBoost.

**Avoiding overfitting.**

1. During training: use two-way instead of multi-way splits, and stop when a statistical test says further splitting is not justified.
2. Post-pruning: grow a big tree, then for each node measure how pruning it changes accuracy on a tuning (validation) set, and greedily remove the node whose removal helps most.

Why not prune with the training set? The full tree always fits the training data best, so training accuracy would never favor pruning. Why not the test set? Looking at it while learning is cheating, and your final accuracy estimate would no longer be unbiased.

**Other facts.** Trees are human-readable when small (not when huge or in ensembles). They are insensitive to monotone transformations of numeric features such as log or scaling, because only the ordering of values matters for thresholds, so standardization is not required.

### Worked example 1 (lecture): which split is better?

Data D has 9 positive and 5 negative examples.

```
H(D) = −(9/14) log₂(9/14) − (5/14) log₂(5/14)
     = 0.643 · 0.637 + 0.357 · 1.485 = 0.940

Split on Humidity:
  high   [3+, 4−]   H = 0.985      weight 7/14
  normal [6+, 1−]   H = 0.592      weight 7/14
  H(Y | Humidity) = 0.5 · 0.985 + 0.5 · 0.592 = 0.789
  InfoGain = 0.940 − 0.789 = 0.151

Split on Wind:
  weak   [6+, 2−]   H = 0.811      weight 8/14
  strong [3+, 3−]   H = 1.000      weight 6/14
  H(Y | Wind) = (8/14) · 0.811 + (6/14) · 1.0 = 0.892
  InfoGain = 0.940 − 0.892 = 0.048
```

Humidity wins (0.151 > 0.048), so it becomes the root.

### Worked example 2: a split that gains nothing

A node has 10 examples, \[6+, 4−\], so H = 0.971.

```
Split A:  left [4+, 0−] (4 examples, H = 0)   right [2+, 4−] (6 examples, H = 0.918)
          H(Y | A) = 0.4 · 0 + 0.6 · 0.918 = 0.551      InfoGain = 0.971 − 0.551 = 0.420

Split B:  left [3+, 2−] (5 examples, H = 0.971)   right [3+, 2−] (5 examples, H = 0.971)
          H(Y | B) = 0.971                                 InfoGain = 0
```

Split B leaves the class mix unchanged in both children, so you learn nothing. Split A is clearly better.

### Common traps

- Forgetting the weights: conditional entropy is the size-weighted average of child entropies, not a plain average.
- Using log base 10 or ln. The lecture uses log base 2.
- Thinking a 100% training-accuracy tree is a good tree. It is usually overfit.
- Saying a feature can be used only once per tree, or that you must split on all features, or that leaves must hold a single point. All three are false.
- Standardizing features "for the tree". It does not change the result.

### Practice: short answer

1. Write the entropy formula. What is the entropy of a \[5+, 5−\] node and of an \[8+, 0−\] node?
2. A node has 8 examples, \[4+, 4−\]. Split X gives children \[3+, 1−\] and \[1+, 3−\]. Split Y gives \[2+, 2−\] and \[2+, 2−\]. Compute the information gain of each and say which to use.
3. Describe the decision tree for Y = X₂ OR X₅.
4. Give two ways to limit overfitting in decision trees. Why can neither the training set nor the test set be used to decide which nodes to prune?
5. Why do decision trees not need feature standardization?
6. Why do we use greedy information gain instead of searching for the smallest tree?

### Practice: MCQ

**Q1.** The entropy of a node with 5 positive and 5 negative examples is:

- A. 1
- B. 0
- C. 0.5
- D. 2

**Q2.** Which node has the lowest entropy?

- A. \[5+, 5−\]
- B. \[10+, 0−\]
- C. \[8+, 2−\]
- D. \[6+, 4−\]

**Q3.** Information gain of splitting on feature X is:

- A. H(Y) + H(Y | X)
- B. H(Y | X) − H(Y)
- C. H(Y) · H(Y | X)
- D. H(Y) − H(Y | X)

**Q4.** Decision trees partition the feature space into:

- A. Circles
- B. Diagonal half-planes
- C. Axis-parallel rectangles
- D. A Voronoi diagram

**Q5.** Which statement about growing a tree is true?

- A. A feature can never be reused after one split on it
- B. You should keep splitting until each leaf has one example
- C. You should split on every feature
- D. You can stop when all examples in a node share the same class

**Q6.** Which change to a numeric feature will NOT change the learned tree?

- A. Shuffling the feature's values among the rows
- B. Replacing x by log(x), for x > 0
- C. Deleting the feature
- D. Adding random noise to the feature

**Q7.** Learning the smallest decision tree that is consistent with the training data is:

- A. NP-hard
- B. Solved exactly by greedy information gain
- C. Impossible to define
- D. Solvable by gradient descent

### Answers

**Short answer**

1. H(Y) = − Σ P(y) log₂ P(y). \[5+, 5−\]: 1 bit. \[8+, 0−\]: 0.
2. H(parent) = 1. H(\[3+, 1−\]) = H(\[1+, 3−\]) = 0.811. Split X: H(Y | X) = 0.5 · 0.811 + 0.5 · 0.811 = 0.811, so InfoGain = 1 − 0.811 = 0.189. Split Y: both children are 50/50 with H = 1, so InfoGain = 0. Use X.
3. Root tests X₂. If X₂ is true, leaf = true. If X₂ is false, test X₅: true gives a true leaf, false gives a false leaf.
4. Two ways: use two-way splits plus a statistical stopping test, or post-prune using a tuning set. The training set cannot be used because the full tree always fits it best, so it would never favor pruning. The test set cannot be used because it must stay untouched to give an unbiased estimate of future accuracy.
5. A tree compares a feature only against thresholds, so only the ordering of the values matters. Any monotone rescaling gives the same splits.
6. Finding the smallest consistent tree is NP-hard, so we pick each split greedily by information gain.

**MCQ**

1. A. A 50/50 split is maximum uncertainty for two classes, 1 bit.
2. B. A pure node has entropy 0.
3. D. Gain is the entropy before the split minus the expected entropy after.
4. C. Each leaf is a rectangle with sides parallel to the axes.
5. D. A, B and C are all false. A feature can be reused, and you do not split to one example per leaf (overfitting) or on every feature.
6. B. Log is monotone, so the ordering of the values is unchanged. Shuffling, deleting and adding noise change which examples fall where.
7. A. NP-hard. That is why we use a greedy heuristic.

## 4. Overfitting and validation

Training accuracy says little about future accuracy, so you estimate performance on data the model has never seen. A held-out test set gives the final estimate, a validation set is for tuning, and cross-validation squeezes more out of a small dataset.

### Explanation

**Overfitting.** Write error\_D(h) for the error on the training data and error\_P(h) for the error on the whole data distribution. Model h overfits if error\_D(h) is low but error\_P(h) is high.

**Why it happens.**

- The model is too complex (too many parameters, such as a very deep tree or a degree-9 polynomial).
- There is too little data to pin down the parameters.
- The model fits noise or chance patterns. Even with noise-free data, a small training set can contain features that correlate with the label by luck. Lecture example: a tree that splits on X3 gets 100% training accuracy but 50% test accuracy, while the one-leaf tree "always predict true" gets 66% on both.
- Noisy labels force extra branches: the lecture shows a tree growing X3 and X4 branches just to fit one noisy training row.

**What the curves look like.** As a tree grows or the polynomial degree k rises, training error falls steadily. Test error usually falls first and then rises. Generalization error is therefore not monotone in model complexity. (Modern deep networks add a twist, see section 6.) More training data helps reduce overfitting. "No noise means no overfitting" is false.

**Test set.** A set held aside during learning, used once to get an unbiased estimate of accuracy. Rule: do not train on the test set, and do not let test labels influence the model in any way (including hyperparameter choices).

**Validation (tuning) set.** Take part of the training data and set it aside. Use it to choose hyperparameters, decide when to stop training, or prune a tree. Then train again on all the training data (not the test data) with the best hyperparameters and test once.

```
all data = train  +  tuning (validation)  +  test
```

**Learning curve.** Accuracy as a function of the training set size. To draw one: for each size s, randomly pick s training instances, train, evaluate on the test set, and plot (s, accuracy). Repeat several times and add error bars. The sample size on the curve is the training set size, not the test set size.

**Confidence interval for error.** Suppose a model makes r errors on a test set of n examples. The estimate is error\_S(h) = r / n. With C% probability the true error lies in:

```
error_S(h)  ±  z_C · √( error_S(h) · (1 − error_S(h)) / n )          (z_C = 1.96 for 95%)
```

A larger test set gives a narrower interval, so a more reliable estimate. It is the test set size, not the training set size, that controls how reliable the accuracy estimate is.

**Why one train/test split is not enough.**

1. With limited data you must trade off: a bigger test set gives a more reliable estimate, but a bigger training set better represents what you could learn from all the data.
2. One split cannot show how sensitive the result is to which examples ended up in training.

**Random resampling.** Repeat random train/test splits and average.

**Stratified sampling.** When splitting, keep the class proportions the same in each set: group the instances by class, then sample from each class in proportion. This matters most when classes are imbalanced.

**k-fold cross-validation.** Split the data into k equal folds. In round i, train on the other k − 1 folds and test on fold i. Combine the k results.

- 10 folds is common; use fewer if training is slow.
- Leave-one-out: k equals the number of instances (N folds, N models).
- Stratified CV uses stratified sampling to build the folds.
- CV uses data efficiently for testing, but it evaluates a learning method with its specific choices, not one individual trained model.
- CV can also be used for tuning: pick the hyperparameter with the best CV score, then retrain on all training data.

### Worked example 1: 5-fold cross-validation

100 instances, 5 folds of 20.

```
round   train on        test on   correct
  1     s2 s3 s4 s5     s1        11 / 20
  2     s1 s3 s4 s5     s2        17 / 20
  3     s1 s2 s4 s5     s3        16 / 20
  4     s1 s2 s3 s5     s4        13 / 20
  5     s1 s2 s3 s4     s5        16 / 20

accuracy = (11 + 17 + 16 + 13 + 16) / 100 = 73 / 100 = 73%
```

Each model trains on 80 instances and each instance is tested exactly once.

### Worked example 2: choosing k for kNN

```
5-fold CV accuracy on the training data
  k = 1: 0.71     k = 3: 0.76     k = 5: 0.78     k = 7: 0.75

pick k = 5 → retrain on all training data with k = 5 → evaluate once on the test set
```

### Worked example 3: confidence interval

20 errors on 100 test examples gives error = 0.20.

```
standard error = √(0.20 · 0.80 / 100) = √0.0016 = 0.04
95% interval   = 0.20 ± 1.96 · 0.04 = 0.20 ± 0.078, i.e. about 0.12 to 0.28
```

With 400 test examples and the same 0.20 error rate the standard error halves to 0.02, giving 0.20 ± 0.039.

### Common traps

- Choosing hyperparameters by looking at test accuracy. That makes the test estimate optimistic.
- Reading the learning curve's x-axis as the test set size. It is the training set size.
- Thinking cross-validation scores one specific trained model. It scores the method.
- Believing noise is the only cause of overfitting.

### Practice: short answer

1. Define overfitting using error\_D(h) and error\_P(h), and give two causes.
2. Why must the test set not be used to choose hyperparameters? What do you use instead, and what do you do after choosing?
3. A model makes 30 errors on 200 test examples. Give the estimated error and its 95% confidence interval (z = 1.96).
4. 5-fold cross-validation on 150 instances gives correct counts 24, 27, 21, 25 and 23 on the five folds. What is the CV accuracy, and how many instances does each model train on?
5. What is stratified sampling and when does it matter?
6. How do you build a learning curve, and what is on its x-axis?

### Practice: MCQ

**Q1.** Which statement is true?

- A. If the data has no noise, overfitting cannot happen
- B. Overfitting can improve generalization
- C. Generalization error always decreases as model complexity grows
- D. More training data can help prevent overfitting

**Q2.** In leave-one-out cross-validation on a dataset of N instances, how many models are trained?

- A. N
- B. 1
- C. 10
- D. N − 1

**Q3.** Cross-validation evaluates:

- A. One specific trained hypothesis
- B. The test set
- C. A learning method together with its specific choices
- D. Only the validation set

**Q4.** The x-axis of a learning curve is usually:

- A. Test set size
- B. Training set size
- C. Number of features
- D. Value of the learning rate

**Q5.** Which change makes a 95% confidence interval for test error narrower?

- A. A smaller test set
- B. A higher confidence level
- C. An error rate closer to 0.5
- D. A larger test set

**Q6.** Stratified sampling is used to:

- A. Keep class proportions the same in each split
- B. Make folds of different sizes
- C. Remove outliers
- D. Increase the number of features

**Q7.** With 5-fold CV on 100 instances, each model is trained on how many instances?

- A. 20
- B. 80
- C. 99
- D. 100

**Q8.** After using a validation set to pick the best hyperparameters, what is the standard next step?

- A. Report the validation accuracy as the final result
- B. Keep tuning on the test set
- C. Train again on all the training data with those hyperparameters and evaluate once on the test set
- D. Discard the validation set and the model

### Answers

**Short answer**

1. h overfits if error\_D(h) is low but error\_P(h) is high. Causes: model too complex, too little data, noisy labels or features, chance correlations in a small sample.
2. Using test labels in tuning leaks information about the test set, so the final accuracy estimate is optimistic. Use a validation (tuning) set carved from the training data, or cross-validation. After choosing, retrain on all the training data and evaluate once on the test set.
3. error = 30 / 200 = 0.15. Standard error = √(0.15 · 0.85 / 200) = √0.0006375 ≈ 0.0253. Interval = 0.15 ± 1.96 · 0.0253 ≈ 0.15 ± 0.050, so about 0.10 to 0.20.
4. Accuracy = (24 + 27 + 21 + 25 + 23) / 150 = 120 / 150 = 0.80. Each model trains on 4 folds of 30, so 120 instances.
5. Group the instances by class, then sample from each class in proportion, so each split keeps the full dataset's class proportions. It matters when classes are imbalanced, because a random split could leave a rare class almost absent from a fold.
6. For each training size s: randomly select s training instances, train a model, evaluate on the test set, and record the accuracy (repeat and average). The x-axis is the training set size.

**MCQ**

1. D. Noise-free data can still overfit through chance correlations. Overfitting hurts generalization, and the curve is not monotone.
2. A. One model per instance, each trained on N − 1 instances and tested on the one left out.
3. C. CV scores the method, not an individual hypothesis.
4. B. Training set size.
5. D. The width shrinks like 1/√n. A smaller test set or a higher confidence level widens it, and an error rate nearer 0.5 raises √(p(1 − p)).
6. A. Stratification preserves class proportions.
7. B. 4 of 5 folds, so 80.
8. C. Test data stays untouched until the very end.

## 5. Evaluation metrics: confusion matrix, precision/recall, ROC

Accuracy hides what kind of mistakes a model makes. The confusion matrix breaks mistakes into false positives and false negatives, and metrics such as precision, recall, F1 and ROC curves let you judge a model by the cost of each kind of mistake.

### Explanation

**Confusion matrix (two classes).** The lecture puts predicted class on the rows and actual class on the columns:

|  | actual positive | actual negative |
| --- | --- | --- |
| predicted positive | TP (true positive) | FP (false positive) |
| predicted negative | FN (false negative) | TN (true negative) |

**Formulas to memorize.**

```
accuracy            = (TP + TN) / (TP + FP + FN + TN)
error               = 1 − accuracy = (FP + FN) / total
recall = TPR        = TP / (TP + FN)        = TP / actual positives
precision (PPV)     = TP / (TP + FP)        = TP / predicted positives
false positive rate = FP / (FP + TN)        = FP / actual negatives
F1                  = 2 · precision · recall / (precision + recall)
```

A quick way to keep them straight: precision asks "of the ones I called positive, how many were right?". Recall asks "of the real positives, how many did I find?". Both TPR and FPR divide by a column total (actual class); precision divides by a row total (predicted class). F1 is the harmonic mean of precision and recall (the slide words it as "twice the harmonic mean"; either way, use 2PR/(P + R)). F1 is low if either precision or recall is low.

**When accuracy is not enough.**

- Class skew: if 97% of instances are negative, a model that always says "negative" scores 97% accuracy and finds nothing. A 98% accuracy is then not impressive.
- Different costs: in a medical test, a false positive costs an extra test, but a false negative means a disease goes untreated. You want to weigh them differently.

**Thresholds.** Many classifiers output a probability Pr(Y = True | x). You turn it into a label with a threshold c: predict True if Pr(Y = True | x) ≥ c. Taking the most likely class (c = 0.5) is not optimal when FP and FN costs differ. Raising c makes fewer positive predictions: TPR and FPR both go down, precision tends to rise, recall falls. Lowering c does the opposite.

**ROC curve.** Plot TPR (y-axis) against FPR (x-axis) as the threshold sweeps from high to low.

- Ideal point: (0, 1), top-left. Worst point: (1, 0).
- Random guessing: the diagonal from (0, 0) to (1, 1).
- Increasing the threshold moves you down along the curve toward (0, 0).
- The best operating point depends on the relative cost of FN and FP.
- Summarize with the area under the curve (AUC): 1 is perfect, 0.5 is random guessing. AUC is the probability that a randomly chosen positive gets a higher score than a randomly chosen negative.
- ROC curves do not change if you change the proportion of positives and negatives in the test set, because TPR and FPR are each computed within one class.

**Precision/recall (PR) curve.** Plot precision against recall as the threshold varies. The ideal point is (1, 1). The baseline precision of a useless model equals the fraction of positives in the data. PR curves show the fraction of predictions that are false positives, and they are well suited to tasks with many negatives.

Both curves assume binary classification and show performance at many confidence levels.

### Worked example 1: all the metrics from a matrix

TP = 40, FP = 10, FN = 20, TN = 130 (200 examples, 60 actual positives, 140 actual negatives).

```
accuracy  = (40 + 130) / 200 = 0.85
precision = 40 / (40 + 10)   = 0.80
recall    = 40 / (40 + 20)   = 0.667
FPR       = 10 / (10 + 130)  = 0.071
F1        = 2 · 0.80 · 0.667 / (0.80 + 0.667) = 0.727
```

### Worked example 2: accuracy that fools you

1000 patients, 10 are sick. A model predicts "healthy" for everyone: TP = 0, FP = 0, FN = 10, TN = 990.

```
accuracy = 990 / 1000 = 0.99      recall = 0 / 10 = 0      F1 = 0
```

Accuracy is 99% and the model never finds a sick patient.

### Worked example 3 (lecture): building an ROC curve

Ten test examples with the model's confidence of being positive. 5 are actually positive (+), 5 negative (−). Sort by confidence, then lower the threshold one example at a time:

```
example  confidence  true class   after including it:  TPR     FPR
  Ex9      0.99         +                             1/5     0/5
  Ex7      0.98         +                             2/5     0/5
  Ex1      0.72         −                             2/5     1/5
  Ex2      0.70         +                             3/5     1/5
  Ex6      0.65         +                             4/5     1/5
  Ex10     0.51         −                             4/5     2/5
  Ex3      0.39         −                             4/5     3/5
  Ex5      0.24         +                             5/5     3/5
  Ex4      0.11         −                             5/5     4/5
  Ex8      0.01         −                             5/5     5/5
```

Each positive moves the curve up by 1/5, each negative moves it right by 1/5. The AUC counts the positive/negative pairs where the positive scores higher: Ex9 and Ex7 beat all 5 negatives, Ex2 and Ex6 beat 4, Ex5 beats 2, so 5 + 5 + 4 + 4 + 2 = 20 of 25 pairs, and AUC = 0.80.

### Common traps

- Swapping precision and recall. Precision divides by TP + FP, recall by TP + FN.
- Dividing FPR by the number of predicted positives. It divides by actual negatives.
- Forgetting that accuracy ignores the cost of each error type.
- Moving the wrong way along the ROC curve. A higher threshold moves toward (0, 0), a lower one toward (1, 1).
- Checking which axis is which. ROC is TPR versus FPR. PR is precision versus recall.

### Practice: short answer

1. TP = 30, FP = 20, FN = 10, TN = 140. Compute accuracy, precision, recall, FPR and F1.
2. 97% of the instances are negative and a model reports 98% accuracy. Why is that not good evidence?
3. In a cancer screening test, a missed case is far worse than a false alarm. Should you favor higher recall or higher precision? Which way do you move the threshold?
4. Describe how to build an ROC curve from a list of scores and true labels.
5. What does the diagonal of an ROC plot represent? What is the AUC of a perfect classifier and of random guessing?
6. When would you prefer a precision/recall curve over an ROC curve?

### Practice: MCQ

**Q1.** TP = 50, FP = 10, FN = 40, TN = 900. The recall is closest to:

- A. 0.83
- B. 0.95
- C. 0.56
- D. 0.05

**Q2.** With heavy class imbalance, which metric is the least informative on its own?

- A. Accuracy
- B. Precision
- C. Recall
- D. F1

**Q3.** On an ROC plot (x = FPR, y = TPR), which points are the best and worst possible?

- A. Best (1, 1), worst (0, 0)
- B. Best (1, 0), worst (0, 1)
- C. Best (0, 0), worst (1, 1)
- D. Best (0, 1), worst (1, 0)

**Q4.** If you raise the classification threshold c, what usually happens?

- A. TPR and FPR both increase
- B. TPR and FPR both decrease
- C. FPR increases and TPR decreases
- D. Nothing changes

**Q5.** Which statement about ROC curves is true?

- A. The curve does not change if the class proportions in the test set change
- B. They plot precision against recall
- C. A perfect classifier passes through (1, 0)
- D. They apply only to regression

**Q6.** A model has precision 0.5 and recall 0.75. Its F1 is:

- A. 0.625
- B. 0.5
- C. 0.75
- D. 0.6

**Q7.** The AUC of a classifier that guesses at random is:

- A. 0
- B. 0.25
- C. 0.5
- D. 1

**Q8.** For a screening test where a missed disease is much worse than a false alarm, you should:

- A. Raise the threshold to increase precision
- B. Lower the threshold to increase recall, accepting more false positives
- C. Maximize accuracy and ignore the error types
- D. Report only precision

### Answers

**Short answer**

1. Total = 200. Accuracy = (30 + 140) / 200 = 0.85. Precision = 30 / 50 = 0.60. Recall = 30 / 40 = 0.75. FPR = 20 / 160 = 0.125. F1 = 2 · 0.60 · 0.75 / 1.35 = 0.667.
2. A model that always predicts negative already gets 97%, so 98% is barely better than doing nothing. Check recall and precision on the positive class.
3. Favor recall (catch every real case). Lower the threshold so more people are flagged, accepting extra false positives. On the ROC curve you move up and to the right.
4. Sort the examples by score from high to low. Start with the threshold above every score (TPR = 0, FPR = 0). Lower it one example at a time. After each example, compute TPR = TP / actual positives and FPR = FP / actual negatives and plot the point. Equivalently, each positive moves the path up by 1 / (number of positives) and each negative moves it right by 1 / (number of negatives).
5. The diagonal is random guessing (TPR = FPR). Perfect AUC is 1.0, random is 0.5.
6. When there are many more negatives than positives, or you care about how many predicted positives are wrong. FPR can look tiny because the negative count is huge, while precision exposes the false positives.

**MCQ**

1. C. Recall = 50 / (50 + 40) = 0.556.
2. A. A model that always predicts the majority class gets high accuracy.
3. D. Perfect: every positive found (TPR = 1) and no false alarms (FPR = 0).
4. B. Fewer positive predictions, so both rates drop.
5. A. TPR and FPR are each computed within one class. B describes PR curves.
6. D. 2 · 0.5 · 0.75 / (0.5 + 0.75) = 0.75 / 1.25 = 0.6.
7. C. Random guessing is the diagonal, with area 0.5.
8. B. Lower the threshold: more recall, more false positives.

## 6. Bias, variance and double descent

Error on new data has three sources: bias (the model family cannot capture the truth), variance (the fitted model changes a lot from one training sample to another) and noise (randomness in the labels that nobody can remove). More expressive models lower bias but raise variance, so the best model sits in between.

### Explanation

Imagine drawing many different training sets from the same source and fitting the same type of model to each one.

- **Bias**: how far the average of all those fitted models is from the true function. It is a systematic error caused by a model that is too simple, for example a straight line through a wavy curve. High bias means underfitting.
- **Variance**: how much the individual fitted models scatter around that average. It is caused by a model that is flexible enough to chase the quirks and noise of its particular training set. High variance means overfitting.
- **Noise**: randomness in y even if you knew the true function. It is irreducible.

For squared error in regression the pieces add up:

```
expected error = bias² + variance + noise
```

**Lecture example: fitting polynomials.**

| Model | Bias | Variance | Behavior |
| --- | --- | --- | --- |
| 1st-order polynomial (line) | high | low | poor approximation, but stable from sample to sample |
| 50th-order polynomial | low | high | fits well on average, but swings wildly with each sample |
| 4th-order polynomial | moderate | moderate | good trade-off |

**Capacity versus error.** As capacity (flexibility) grows, training error keeps falling. Test error is U-shaped: on the left is the underfitting zone (high bias), on the right the overfitting zone (high variance), and the minimum marks the optimal capacity. More expressive learners reduce bias but increase variance. The idea extends from regression to classification.

**Diagnosing from the errors.**

| Training error | Validation error | Diagnosis | What to try |
| --- | --- | --- | --- |
| high | high (similar) | high bias, underfitting | more features, polynomial terms, a bigger model, weaker regularization |
| very low | much higher | high variance, overfitting | more data, simpler model, regularization, early stopping, dropout, augmentation |
| low | low (similar) | good fit | stop and test |

More data mainly cuts variance. It does not fix a model that is too simple.

**Connections you may be asked about.**

- kNN: small k is low bias and high variance. Large k is high bias and low variance.
- Regularization: stronger λ raises bias and lowers variance.
- Decision trees: a deeper tree has lower bias and higher variance.

**Double descent (extra material).** Classic theory predicts the U-shape. Modern heavily over-parameterized models (such as deep networks) can show a second drop: test error falls, then rises as the model reaches the point where it can just barely fit the training data, then falls again as the model keeps growing. Bigger models and more data can sometimes even hurt. The classical bias-variance picture is still the foundation, but it is not the complete story.

### Worked example: read the diagnosis

```
Model A:  train error 30%   validation error 32%    → both high, close together → underfitting (high bias)
Model B:  train error  2%   validation error 25%    → big gap                  → overfitting (high variance)
Model C:  train error  5%   validation error  6%    → both low, close          → good fit
```

For A, adding more training data will not help much, so make the model richer. For B, more data or stronger regularization helps.

### Common traps

- Judging a model from training error alone. Compare training and validation error.
- Thinking more data fixes high bias. It does not.
- Thinking noise can be modeled away. It is irreducible.
- Forgetting that large k in kNN means high bias, not high variance.
- Believing the U-shape always holds. Double descent shows exceptions for very large models.

### Practice: short answer

1. Explain bias, variance and noise in your own words, and write the decomposition of expected squared error.
2. For polynomial regression of degree 1, 4 and 50, state the bias and variance and say which underfits and which overfits.
3. A model has 2% training error and 25% validation error. Diagnose it and give two remedies. What about a model with 30% training and 32% validation error?
4. Why does more training data reduce variance but do little for bias?
5. How does the choice of k in kNN move you along the bias-variance trade-off?
6. Describe double descent in two sentences.

### Practice: MCQ

**Q1.** Which model most likely has high bias and low variance?

- A. A degree-50 polynomial
- B. 1-NN
- C. A straight line fit to clearly curved data
- D. A decision tree grown until every leaf is pure

**Q2.** Training error is 1% and test error is 20%. This most likely indicates:

- A. High variance (overfitting)
- B. High bias (underfitting)
- C. High noise only
- D. A perfect model

**Q3.** Which part of the expected error cannot be reduced by any choice of model?

- A. Bias
- B. Variance
- C. Training error
- D. Noise

**Q4.** For squared error, expected prediction error equals:

- A. bias + variance
- B. bias² + variance + noise
- C. bias × variance
- D. noise only

**Q5.** Increasing the regularization strength λ generally:

- A. Increases bias and decreases variance
- B. Decreases bias and increases variance
- C. Increases both
- D. Changes neither

**Q6.** Both training and validation error are high. Which step is most likely to help?

- A. Collect more training data of the same kind
- B. Increase regularization
- C. Add polynomial features or use a larger model
- D. Stop training earlier

**Q7.** Double descent refers to:

- A. Training error rising twice
- B. A model being trained twice
- C. Gradient descent run on two losses
- D. Test error that falls, rises near the point where the model just fits the training data, then falls again as capacity keeps growing

### Answers

**Short answer**

1. Bias: the gap between the average fitted model and the truth, from a model too simple. Variance: how much fitted models differ between training samples, from a model too flexible. Noise: randomness in the labels that no model can predict. Expected error = bias² + variance + noise.
2. Degree 1: high bias, low variance, underfits. Degree 50: low bias, high variance, overfits. Degree 4: a good balance.
3. 2% vs 25% is a large gap, so it is overfitting (high variance). Remedies: more data, a simpler model, regularization, early stopping. 30% vs 32% is high error on both, so it is underfitting (high bias). Remedies: richer features, polynomial terms, a bigger model, weaker regularization.
4. With more samples the fitted model depends less on any one sample, so predictions vary less. A model that is too simple still cannot represent the truth, however much data it sees.
5. Small k: flexible, low bias, high variance. Large k: smooth, high bias, low variance. Choose k by validation or cross-validation.
6. As model capacity grows, test error first falls, then rises around the point where the model can just fit the training data, and then falls again for even larger models. The classic single U-shape is not the full picture for very large models.

**MCQ**

1. C. A line is too simple for curved data, so it is biased but stable. The others are flexible, high-variance models.
2. A. A big gap between training and test error means high variance.
3. D. Noise is irreducible.
4. B. Bias is squared in the decomposition, and noise is added.
5. A. Regularization restricts the model, trading variance for bias.
6. C. Both errors high means underfitting. More data of the same kind or more regularization will not fix it, and early stopping makes it worse.
7. D. The test-error curve has two descents with a bump in between.

## 7. Linear and polynomial regression

Linear regression fits a line (or a hyperplane in many dimensions) by minimizing the average squared error on the training data. Learning means finding the parameters, and gradient descent is a general way to do it.

### Explanation

**Parametric versus non-parametric.**

- Parametric: the hypotheses are indexed by a parameter vector, and learning means finding the parameters that best approximate the target. Examples: linear models, neural networks.
- Non-parametric: instance-based methods (kNN) and decision trees. They can still have hyperparameters, such as k.

**Hypothesis class.** One feature: f(x) = ax + b. Many features, x in ℝⁿ: f(x) = aᵀx + b = a₁x₁ + … + aₙxₙ + b. There are infinitely many such functions, one per choice of the real-valued parameters.

**Loss and objective.** For M training points, the average squared loss is:

```
L(a, b) = (1/M) Σₘ ( a·x⁽ᵐ⁾ + b − y⁽ᵐ⁾ )²
```

The best linear hypothesis minimizes L over a and b.

**Two ways to minimize it.**

1. Set the derivatives to zero and solve. Linear regression has a closed-form solution.
2. Gradient descent: more general, and the approach that carries over to other losses and to neural networks.

**Gradients (chain rule).** For one feature:

```
∂L/∂a = (1/M) Σₘ 2 ( a·x⁽ᵐ⁾ + b − y⁽ᵐ⁾ ) · x⁽ᵐ⁾
∂L/∂b = (1/M) Σₘ 2 ( a·x⁽ᵐ⁾ + b − y⁽ᵐ⁾ )
```

In n dimensions it is the same with vectors: ∇ₐL = (1/M) Σₘ 2 ( aᵀx⁽ᵐ⁾ + b − y⁽ᵐ⁾ ) x⁽ᵐ⁾. The squared loss of a linear model is smooth and strongly convex, so gradient descent converges quickly to the global minimum.

**Regression metrics.**

```
MSE  = (1/M) Σ ( f(x) − y )²            units of y squared
RMSE = √MSE                               same units as y
MAE  = (1/M) Σ | f(x) − y |              average absolute error
R²   = 1 − Σ (y − f(x))² / Σ (y − ȳ)²   fraction of the variance explained
```

Squared error punishes large mistakes more than MAE does. The lecture notes that metrics are often computed on training data, but you should use held-out data or cross-validation.

**Polynomial regression.** Enlarge the hypothesis class with powers of x: aₖxᵏ + … + a₁x + a₀. It is nonlinear in x but still linear in the parameters, so you treat x, x², …, xᵏ as features and use the same loss and gradient descent. A degree-k polynomial in one variable has k + 1 parameters.

**Bigger class, lower training loss, not always better.** A larger hypothesis class never raises the training loss, but that does not mean better predictions. In the lecture's figure k = 1 underfits, k = 3 fits well and k = 9 overfits. As k grows, training error decreases monotonically while test error falls and then rises. Overfitting happens when the model is too complex, when there is too little data to estimate its parameters, and when it captures noise. Prefer the simplest hypothesis consistent with the data, and report the loss on held-out data.

If a linear model underfits, add features or add polynomial terms.

### Worked example 1: one gradient descent step

Data (x, y): (1, 2) and (2, 4). Start at a = 0, b = 0, step size α = 0.1, M = 2.

```
predictions: 0 and 0        residuals (f − y): −2 and −4
loss before = (4 + 16) / 2 = 10

∂L/∂a = (1/2) [ 2(−2)(1) + 2(−4)(2) ] = (1/2)(−4 − 16) = −10
∂L/∂b = (1/2) [ 2(−2)    + 2(−4)    ] = (1/2)(−4 − 8)  = −6

a_new = 0 − 0.1 · (−10) = 1.0
b_new = 0 − 0.1 · (−6)  = 0.6

new predictions: 1·1 + 0.6 = 1.6 and 1·2 + 0.6 = 2.6
residuals: −0.4 and −1.4     loss after = (0.16 + 1.96) / 2 = 1.06
```

The loss fell from 10 to 1.06. The exact best fit here is a = 2, b = 0 with loss 0, and more steps move toward it.

### Worked example 2: the metrics

True y = \[3, 5, 7\], predictions = \[2, 5, 9\], so the errors f − y are \[−1, 0, 2\].

```
MSE  = (1 + 0 + 4) / 3 = 1.667        RMSE = √1.667 = 1.29
MAE  = (1 + 0 + 2) / 3 = 1.0
R²   = 1 − 5 / 8 = 0.375             (mean of y is 5, Σ (y − ȳ)² = 4 + 0 + 4 = 8, Σ (y − f)² = 5)
```

### Common traps

- Calling a polynomial model "nonlinear regression" in the sense of the training method. It is linear in the parameters, so the same machinery applies.
- Quoting MSE as if it were in the units of y. RMSE has those units.
- Treating lower training loss as proof of a better model.
- Forgetting that kNN and decision trees are non-parametric even though they have hyperparameters.

### Practice: short answer

1. Define parametric and non-parametric learning, and classify kNN, decision trees, linear regression and neural networks.
2. Write the average squared loss for one-feature linear regression, and its gradients with respect to a and b.
3. Data (1, 1) and (3, 3). Start from a = 0, b = 0 with α = 0.1. Do one gradient descent step and compute the loss before and after.
4. For y = \[2, 4, 6, 8\] and predictions \[3, 4, 5, 10\], compute MSE, RMSE, MAE and R².
5. Why does a degree-9 polynomial always reach training error at most that of a degree-3 polynomial, and why is that not necessarily better?
6. Your linear model underfits the data. Name two things you can try.

### Practice: MCQ

**Q1.** Which of these is a parametric model?

- A. Linear regression
- B. kNN
- C. A decision tree
- D. A lookup table of the training set

**Q2.** A polynomial of degree k in one variable has how many parameters?

- A. k
- B. k + 1
- C. 2k
- D. k²

**Q3.** As the polynomial degree k increases, the training error:

- A. Increases
- B. Stays constant
- C. Never increases (it decreases or stays the same)
- D. Is U-shaped

**Q4.** Linear regression typically minimizes which loss?

- A. Hinge loss
- B. Zero/one loss
- C. Cross-entropy
- D. Squared loss

**Q5.** The RMSE of a regression model is measured in:

- A. Squared units of y
- B. The same units as y
- C. No units
- D. Units of the features

**Q6.** One training point (x = 2, y = 5), model f(x) = a·x + b with a = 1, b = 1, loss (f(x) − y)². What is ∂L/∂a?

- A. −4
- B. 8
- C. −8
- D. 4

**Q7.** A linear regression model underfits. What is a sensible next step?

- A. Add more features or polynomial terms
- B. Remove features
- C. Increase regularization
- D. Train on less data

### Answers

**Short answer**

1. Parametric: hypotheses are indexed by parameters and learning finds the best parameters (linear regression, neural networks). Non-parametric: no fixed-size parameter vector, the model is built from the data (kNN, decision trees). Non-parametric methods can still have hyperparameters such as k.
2. L(a, b) = (1/M) Σ (a x + b − y)². ∂L/∂a = (1/M) Σ 2 (a x + b − y) x. ∂L/∂b = (1/M) Σ 2 (a x + b − y).
3. Residuals f − y: −1 and −3. Loss before = (1 + 9) / 2 = 5. ∂L/∂a = (1/2)\[2(−1)(1) + 2(−3)(3)\] = (1/2)(−2 − 18) = −10. ∂L/∂b = (1/2)\[2(−1) + 2(−3)\] = (1/2)(−8) = −4. New a = 0 − 0.1(−10) = 1.0, new b = 0 − 0.1(−4) = 0.4. New predictions 1.4 and 3.4, residuals 0.4 and 0.4, loss after = (0.16 + 0.16) / 2 = 0.16.
4. Errors \[+1, 0, −1, +2\]. MSE = (1 + 0 + 1 + 4) / 4 = 1.5. RMSE = √1.5 ≈ 1.22. MAE = (1 + 0 + 1 + 2) / 4 = 1.0. Mean of y is 5, Σ (y − ȳ)² = 9 + 1 + 1 + 9 = 20, Σ (y − f)² = 6, so R² = 1 − 6/20 = 0.70.
5. Every degree-3 polynomial is also a degree-9 polynomial (set the higher coefficients to 0), so the larger class can only match or beat it on the training data. But a degree-9 fit can chase noise and do worse on new data.
6. Add more features, add polynomial terms, use a more expressive model, or reduce regularization.

**MCQ**

1. A. kNN, decision trees and lookup tables are non-parametric.
2. B. Coefficients a₀ through aₖ.
3. C. A bigger class contains the smaller one, so training loss cannot go up. Test error is the one that can be U-shaped.
4. D. Squared loss. Hinge is for perceptron/SVM, cross-entropy for logistic regression and classifiers.
5. B. Taking the square root undoes the squaring.
6. C. f(2) = 1·2 + 1 = 3, so the residual is 3 − 5 = −2, and ∂L/∂a = 2 · (−2) · x = 2 · (−2) · 2 = −8.
7. A. Underfitting means the model is too simple, so make it richer.

## 8. Logistic regression

Logistic regression is a linear classifier that outputs a probability. It computes a linear score z = θᵀx, squashes it with the sigmoid, and trains by minimizing the negative log-likelihood with gradient descent. The name says regression, but it is a classifier.

### Explanation

**Why not just the perceptron?** The perceptron's step function has three problems: the output is a hard 0 or 1 (no sense of how close to the boundary), there is no measure of confidence, and the slope is 0 everywhere except at the jump, so the loss gives no useful gradient. Logistic regression swaps the step for a smooth "soft step", the sigmoid.

**The sigmoid.**

```
σ(z) = 1 / (1 + e^(−z)) = e^z / (1 + e^z)

σ(0) = 0.5      σ(z) → 1 as z → +∞      σ(z) → 0 as z → −∞
σ(−z) = 1 − σ(z)      σ'(z) = σ(z)(1 − σ(z))      (largest slope 0.25 at z = 0)
```

**The model.** Fold the bias into θ by adding a constant feature 1 to x. Then:

```
z = θᵀx
Pθ(y = 1 | x) = σ(θᵀx) = 1 / (1 + exp(−θᵀx))
Pθ(y = 0 | x) = 1 − σ(θᵀx)
```

- θᵀx > 0 gives P(y = 1 | x) > 0.5, so predict 1.
- θᵀx < 0 gives P(y = 1 | x) < 0.5, so predict 0.
- θᵀx = 0 is the decision boundary, a straight line or hyperplane, where P = 0.5.
- The farther θᵀx is from 0, the more confident the prediction.

**Training.** Given i.i.d. training data, minimize the average negative log-likelihood (NLL), also called cross-entropy:

```
min over θ of   − (1/n) Σᵢ log Pθ( y⁽ⁱ⁾ | x⁽ⁱ⁾ )

for y in {0, 1}, one example's loss, with ŷ = σ(θᵀx):
    ℓ = − y log ŷ − (1 − y) log(1 − ŷ)
```

If the true label is 1 the loss is −log ŷ: nearly 0 when ŷ is near 1, and huge when ŷ is near 0. At test time the model outputs label probabilities.

**Gradient and update.** For one example, ∂ℓ/∂θ = (ŷ − y) x (derived step by step in section 13). The gradient descent step is θ ← θ − α (ŷ − y) x.

**Perceptron versus logistic regression.**

|  | Perceptron | Logistic regression |
| --- | --- | --- |
| Activation | step (hard 0/1 or ±1) | sigmoid (probability) |
| Output | a label, no confidence | P(y = 1 given x) |
| Loss | perceptron / hinge loss | negative log-likelihood |
| Update | only on mistakes, w ← w + y x | every example, θ ← θ − α (ŷ − y) x |
| Gradient | zero almost everywhere | smooth everywhere |
| Decision boundary | linear | linear |

**Maximum likelihood (extra).** Choose the parameter that makes the observed data most likely. Coin example: k heads in n flips with P(heads) = θ.

```
likelihood        L(θ) = θᵏ (1 − θ)^(n − k)
log-likelihood    k log θ + (n − k) log(1 − θ)
set derivative to 0:   k/θ − (n − k)/(1 − θ) = 0      →      θ̂ = k / n
```

Maximizing the log-likelihood is the same as minimizing the NLL.

**Beyond two classes.** Use one weight vector per class and a softmax:

```
Pθ(y = i | x) = exp( (θⁱ)ᵀ x ) / Σⱼ exp( (θʲ)ᵀ x )          for classes i = 1..k
```

The training objective is the same NLL, just with more weight vectors.

### Worked example 1: predict, score, update

Let θ = (2, −1) and x = (1, 3).

```
z = 2·1 + (−1)·3 = −1
ŷ = σ(−1) = 1 / (1 + e) = 0.269           → predict class 0 (P(y = 1) is only 27%)

loss if the true label is 1:   −log(0.269) = 1.313
loss if the true label is 0:   −log(1 − 0.269) = −log(0.731) = 0.313

Suppose the true label is y = 1 and α = 0.5:
  gradient = (ŷ − y) x = (0.269 − 1)(1, 3) = (−0.731, −2.193)
  θ_new  = (2, −1) − 0.5 · (−0.731, −2.193) = (2.366, 0.097)
  new z   = 2.366·1 + 0.097·3 = 2.655  →  ŷ = σ(2.655) = 0.934
```

After one step the model went from 27% to 93% confidence on this example, and the loss dropped from 1.313 to about 0.068.

### Worked example 2: softmax

Scores (logits) z = \[2, 1, 0\].

```
exp(2) = 7.389     exp(1) = 2.718     exp(0) = 1.000      sum = 11.107
probabilities = [7.389, 2.718, 1.000] / 11.107 = [0.665, 0.245, 0.090]
```

The probabilities are positive and sum to 1. The largest score gets the largest probability.

### Common traps

- Calling it regression for continuous targets. It is classification.
- Treating σ(θᵀx) as the label. It is P(y = 1 | x). You still threshold it.
- Putting the boundary at ŷ = 0 or 1. It is at θᵀx = 0, where ŷ = 0.5.
- Using squared error as the loss. The loss here is NLL / cross-entropy.
- Mixing label conventions: cross-entropy above uses y in {0, 1}, the perceptron uses y in {−1, +1}.

### Practice: short answer

1. Give three problems of the perceptron's step function that the sigmoid fixes.
2. Write P(y = 1 | x) for logistic regression and the training objective.
3. θ = (1, 2) and x = (1, 0.5) (the first entry of x is the constant bias feature). Compute z, P(y = 1 | x), the predicted class, and the loss if the true label is 1 and if it is 0. (Use σ(2) ≈ 0.881.)
4. You flip a coin 10 times and see 7 heads. Write the log-likelihood and find the maximum likelihood estimate of P(heads).
5. What does P(y = 1 | x) approach when θᵀx is a large positive number, exactly 0, and a large negative number?
6. Explain how a logistic regression update differs from a perceptron update.
7. Compute the softmax of \[2, 1, 0\].

### Practice: MCQ

**Q1.** σ(0) equals:

- A. 0
- B. 1
- C. 0.5
- D. 0.25

**Q2.** The decision boundary of logistic regression is the set of points where:

- A. θᵀx = 0
- B. θᵀx = 1
- C. P(y = 1 | x) = 1
- D. x = 0

**Q3.** Logistic regression is trained by minimizing:

- A. Zero/one loss
- B. Hinge loss
- C. Squared error
- D. Negative log-likelihood (cross-entropy)

**Q4.** Which statement is true?

- A. Logistic regression predicts continuous values
- B. It outputs a probability and is trained by gradient descent on the NLL
- C. A single logistic regression unit can represent XOR
- D. Its loss is the hinge loss

**Q5.** For one example with ŷ = σ(θᵀx) and the cross-entropy loss, the gradient with respect to θ is:

- A. (ŷ − y) x
- B. (y − ŷ) x
- C. ŷ (1 − ŷ) x
- D. (ŷ − y)

**Q6.** A coin lands heads 6 times in 8 flips. The maximum likelihood estimate of P(heads) is:

- A. 0.5
- B. 0.6
- C. 0.75
- D. 0.8

**Q7.** When θᵀx is a large negative number, P(y = 1 | x) is close to:

- A. 0
- B. 0.5
- C. 1
- D. −1

**Q8.** Multiclass logistic regression with k classes uses:

- A. One shared weight vector for all classes
- B. One weight vector per class, combined with a softmax
- C. A step function on each class
- D. k separate decision trees

### Answers

**Short answer**

1. The step gives a hard 0/1 with no sense of distance to the boundary, gives no confidence, and has zero gradient almost everywhere so it is hard to optimize. The sigmoid is smooth, outputs a probability, and has a useful gradient.
2. P(y = 1 | x) = σ(θᵀx) = 1 / (1 + exp(−θᵀx)). Objective: minimize −(1/n) Σ log Pθ(y⁽ⁱ⁾ | x⁽ⁱ⁾).
3. z = 1·1 + 2·0.5 = 2. P(y = 1 | x) = σ(2) ≈ 0.881, so predict class 1. Loss if y = 1: −log(0.881) ≈ 0.127. Loss if y = 0: −log(1 − 0.881) = −log(0.119) ≈ 2.13.
4. Log-likelihood = 7 log θ + 3 log(1 − θ). Derivative: 7/θ − 3/(1 − θ) = 0, so 7(1 − θ) = 3θ, giving θ̂ = 7/10 = 0.7.
5. Close to 1; exactly 0.5; close to 0.
6. A perceptron updates only when it makes a mistake, with w ← w + y x (labels ±1). Logistic regression updates on every example by θ ← θ − α (ŷ − y) x, so the step is bigger the more wrong the predicted probability is.
7. &#91;0.665, 0.245, 0.090\].

**MCQ**

1. C. e⁰ = 1, so 1 / (1 + 1) = 0.5.
2. A. θᵀx = 0 gives σ = 0.5.
3. D. NLL, also called cross-entropy.
4. B. A is wrong (it classifies), C is wrong (a single linear unit cannot do XOR), and D describes the perceptron/SVM family.
5. A. (ŷ − y) x. B has the wrong sign.
6. C. θ̂ = k / n = 6 / 8 = 0.75.
7. A. σ of a large negative number is near 0.
8. B. One weight vector per class plus a softmax.

## 9. Gradient descent and SGD

Gradient descent is how nearly every model in this course gets trained: look at which way the loss goes up, step the opposite way, repeat. Plain GD uses every training example for each step, SGD uses one example (or a small minibatch), and the step size decides whether any of it works.

### Explanation

**The update rule.** Pick a starting point x⁽⁰⁾, then repeat until convergence:

```
x⁽ᵗ⁺¹⁾ = x⁽ᵗ⁾ − αₜ ∇f( x⁽ᵗ⁾ )

x⁽ᵗ⁺¹⁾  updated parameters
x⁽ᵗ⁾    current parameters
αₜ      step size (learning rate): how far to move
∇f      gradient of the loss at the current parameters
```

The gradient points in the direction the loss increases fastest, so we subtract it. Where the gradient is 0 the algorithm stops moving (a fixed point).

**Convexity.** f is convex if the straight line between any two points on its graph lies on or above the graph:

```
f( t·x + (1 − t)·y )  ≤  t·f(x) + (1 − t)·f(y)        for all x, y and t in [0, 1]
```

- If f is convex, any fixed point of gradient descent (gradient = 0) is a global minimum.
- If f is not convex, GD may only reach a local optimum.
- The linear regression loss and the logistic regression NLL are convex. Neural network losses are not.
- Linear regression is also smooth and strongly convex, which is why GD converges quickly on it.

**Step size matters.** Take f(x) = x², so ∇f = 2x. The update becomes x ← x − α·2x = (1 − 2α) x, so every step multiplies x by the same factor (1 − 2α).

| Step size α | Factor 1 − 2α | Behavior |
| --- | --- | --- |
| 0.2 (small) | 0.6 | Converges smoothly but slowly |
| 0.5 | 0 | Lands on the minimum in one step |
| 0.8 | −0.6 | Bounces across the minimum but converges |
| 0.9 | −0.8 | Converges, but with slow oscillation |
| 1.0 | −1 | Bounces between x₀ and −x₀ forever |
| above 1 | below −1 | Diverges (each bounce is bigger) |

Too small wastes time, too large overshoots or blows up. Convergence needs |1 − 2α| < 1, that is 0 < α < 1, for this particular function.

**The cost of plain GD.** With the empirical risk (1/n) Σᵢ lossᵢ(θ), every step needs the gradient of all n examples. For ImageNet with about 10⁶ samples, 100 iterations is about 10⁸ gradient computations.

**SGD (stochastic gradient descent).**

```
GD:    θ ← θ − α · (1/n) Σᵢ ∇lossᵢ(θ)          (sum over all n examples, every step)
SGD:   pick a uniformly from 1..n,   θ ← θ − α · ∇lossₐ(θ)     (no sum: one example)
```

- The random pick is the "stochastic" part.
- In expectation the SGD gradient equals the GD gradient, so each step is correct on average but noisy.
- Each step is about n times cheaper, so you can take many more of them.
- In practice we use **minibatches** (say 32 or 128 examples) to use GPU parallelism and to reduce the noise.
- One **epoch** is one pass over all training data. One **iteration** is one parameter update.

|  | Examples per update | Cost per update | Noise | Updates per epoch |
| --- | --- | --- | --- | --- |
| Full-batch GD | all n | highest | none | 1 |
| Minibatch SGD | B (e.g. 50) | medium | some | n / B |
| SGD | 1 | lowest | most | n |

**Neural-network training loop (slides).** Initialize the weights, then until a stopping criterion is met, for each training point: (1) forward pass to get ŷ = fₜ(x), (2) compute the loss, for example NLL, (3) backward pass to get ∇ₜ loss, (4) update w ← w − α ∇ₜ loss.

### Worked example 1: the slide example, f(x) = x², α = 0.8, x₀ = −4

```
x₁ = −4 − 0.8·2·(−4) = −4 + 6.4  =  2.4
x₂ =  2.4 − 0.8·2·(2.4) =  2.4 − 3.84 = −1.44
x₃ = −1.44 − 0.8·2·(−1.44) = −1.44 + 2.304 = 0.864
x₄ = −0.5184      x₅ = 0.31104      ...      x₃₀ ≈ −8.8×10⁻⁷
```

Each step multiplies by 1 − 2·0.8 = −0.6, so the sign flips every step and the size shrinks by 40%.

### Worked example 2: GD on a tiny regression

Data (x, y): (1, 2) and (2, 4). Model ŷ = a·x (no bias). Loss L(a) = (1/2) Σ (a·x − y)². Gradient: dL/da = (1/2) Σ 2(a·x − y) x. Start at a = 0 with α = 0.1.

```
step 0:  a = 0     loss = (4 + 16)/2 = 10      gradient = (−2·1) + (−4·2) = −10
         a ← 0 − 0.1·(−10) = 1.0
step 1:  a = 1.0   loss = (1 + 4)/2 = 2.5       gradient = (−1·1) + (−2·2) = −5
         a ← 1.0 − 0.1·(−5) = 1.5
step 2:  a = 1.5   loss = (0.25 + 1)/2 = 0.625
```

The true best value is a = 2. The distance to it halves each step (2 → 1 → 0.5), so the loss quarters each step (10 → 2.5 → 0.625).

### Worked example 3: SGD gradient versus GD gradient

Same data and a = 0. Per-example gradients of (a·x − y)²:

```
example (1, 2):   2(0·1 − 2)·1 = −4
example (2, 4):   2(0·2 − 4)·2 = −16
average of the two = −10   ← exactly the full GD gradient from example 2
```

SGD would use −4 or −16 (each picked with probability 1/2). Both point the right way here, but the sizes differ. That is the noise. On average the step equals the GD step.

### Common traps

- Adding the gradient instead of subtracting it. That climbs the loss.
- Assuming bigger α is faster. Past the limit it diverges (for x², α above 1).
- Assuming SGD decreases the loss every step. It does not; it is noisy and only decreases it on average.
- Assuming GD finds the global minimum for any model. That needs convexity. Neural networks can get stuck at local optima.
- Mixing up epoch (one pass over the data) and iteration (one update).
- Forgetting that SGD has no sum: it uses only the sampled example's gradient.

### Practice: short answer

1. Write the gradient descent update rule and say what each part means. Why is there a minus sign?
2. For f(x) = x² with α = 0.8 and x₀ = −4, compute x₁, x₂, x₃.
3. For f(x) = x², write x⁽ᵗ⁺¹⁾ in terms of x⁽ᵗ⁾ and α. For which α does GD converge? What happens at α = 0.5, α = 1 and α = 1.2?
4. Data (1, 2) and (2, 4), model ŷ = a·x, loss (1/2) Σ (a·x − y)². Starting from a = 0 with α = 0.1, compute the gradient, the new a, and the loss before and after one step.
5. Why do we use SGD or minibatch SGD instead of full-batch GD? What is the downside?
6. State what "convex" means and why it matters for gradient descent.
7. You have n = 1000 training examples and a minibatch size of 50. How many parameter updates happen in 10 epochs? How many would plain SGD (one example per update) take?

### Practice: MCQ

**Q1.** The gradient descent update rule is:

- A. x ← x + α ∇f(x)
- B. x ← x − α ∇f(x)
- C. x ← x − ∇f(x) / α
- D. x ← α x − ∇f(x)

**Q2.** For f(x) = x², α = 0.25 and x₀ = 8, the value of x₁ is:

- A. 0
- B. 6
- C. 7.5
- D. 4

**Q3.** For f(x) = x² with α = 1, starting from x₀ = 3, GD:

- A. Bounces between 3 and −3 forever
- B. Reaches 0 in one step
- C. Converges smoothly to 0
- D. Diverges to infinity

**Q4.** Which statement about SGD is true?

- A. Each step uses the gradient summed over all training examples
- B. It is guaranteed to lower the loss at every step
- C. In expectation, its gradient equals the full GD gradient
- D. It can only be used with convex loss functions

**Q5.** If f is convex and differentiable, and GD reaches a point where ∇f = 0, that point is:

- A. Only a local minimum
- B. A global minimum
- C. A saddle point
- D. A maximum

**Q6.** A step size that is far too small will most likely cause:

- A. Divergence
- B. Oscillation around the minimum
- C. Very slow convergence
- D. Overfitting

**Q7.** With n = 2000 training examples and a minibatch size of 100, the number of updates in one epoch is:

- A. 1
- B. 100
- C. 20
- D. 2000

**Q8.** The main reason to use minibatches instead of single-example SGD is:

- A. The single-example gradient is biased
- B. Minibatches guarantee a global minimum on non-convex losses
- C. Single examples cannot be used for classification
- D. They use parallel hardware (GPUs) well and give a less noisy gradient

### Answers

**Short answer**

1. x⁽ᵗ⁺¹⁾ = x⁽ᵗ⁾ − αₜ ∇f(x⁽ᵗ⁾). Updated parameters equal current parameters minus (step size times the loss gradient at the current parameters). The gradient points uphill, so we subtract it to go downhill and reduce the loss.
2. x₁ = −4 − 0.8·2·(−4) = 2.4. x₂ = 2.4 − 0.8·2·2.4 = −1.44. x₃ = −1.44 − 0.8·2·(−1.44) = 0.864.
3. x⁽ᵗ⁺¹⁾ = (1 − 2α) x⁽ᵗ⁾. It converges when |1 − 2α| < 1, so 0 < α < 1. At α = 0.5 the factor is 0 and it lands on 0 in one step. At α = 1 the factor is −1 and x flips sign forever. At α = 1.2 the factor is −1.4, so |x| grows each step and it diverges.
4. Gradient = (−2·1) + (−4·2) = −10 (the (1/2) cancels the 2 in 2(a·x − y)). New a = 0 − 0.1·(−10) = 1.0. Loss before = (4 + 16)/2 = 10. Loss after = ((1−2)² + (2−4)²)/2 = (1 + 4)/2 = 2.5.
5. Full-batch GD needs the gradient of all n examples for every step, which is too slow for large n (ImageNet: about 10⁸ gradient computations for 100 iterations). SGD uses one example, or a minibatch, per step, so each step is far cheaper and GPUs can parallelize minibatches. The downside is noise: each step is only correct on average, so the loss bounces around instead of decreasing smoothly.
6. f is convex if f(tx + (1−t)y) ≤ t f(x) + (1−t) f(y) for all x, y and t in \[0, 1\] (the chord lies above the curve). For a convex f, any point where GD stops (gradient 0) is a global minimum. Without convexity, GD may stop at a local optimum.
7. Minibatch: 1000/50 = 20 updates per epoch, so 200 updates in 10 epochs. Plain SGD: 1000 updates per epoch, so 10,000 updates.

**MCQ**

1. B. Subtract the scaled gradient.
2. D. x₁ = 8 − 0.25·2·8 = 8 − 4 = 4.
3. A. x₁ = 3 − 1·2·3 = −3, then 3, then −3, and so on.
4. C. A is full-batch GD, B is false because SGD is noisy, and D is false because SGD is used on non-convex networks.
5. B. For a convex function, gradient zero means global minimum.
6. C. A tiny step size makes progress slowly. Divergence and oscillation come from a step size that is too large.
7. C. 2000 / 100 = 20.
8. D. The single-example gradient is unbiased (A is wrong), nothing guarantees a global minimum on non-convex losses (B is wrong), and SGD works for classification (C is wrong).

## 10. Regularization and practical training

Regularization means any method that prevents overfitting or helps optimization. The common ideas are a penalty on large weights (ridge, lasso), more varied data (augmentation, noise), stopping before the model memorizes the training set (early stopping), and tricks for neural nets (dropout, batch normalization).

### Explanation

**1. Penalty-based regularization.** Add a cost for large parameters to the training objective:

```
argmin over θ of   (1/n) Σᵢ lossᵢ(θ)  +  λ R(θ)

Ridge (L2):   R(θ) = ½ ‖θ‖₂² = ½ (θ₁² + θ₂² + ...)      weight decay, makes parameters small
Lasso (L1):   R(θ) = ‖θ‖₁ = |θ₁| + |θ₂| + ...           sparsity, eliminates weak features
```

λ ≥ 0 controls the strength. λ = 0 is no regularization. As λ grows, the model is pushed toward simpler solutions, so bias goes up and variance goes down. Too large a λ underfits. Choose λ on a validation set (or by cross-validation), never on the test set.

**Why ridge is called weight decay.** The gradient of λ · ½ ‖w‖₂² is λ w, so the update becomes:

```
w ← w − α ( ∇loss + λ w )  =  (1 − αλ) w  −  α ∇loss
```

Before the usual gradient step, every weight is multiplied by (1 − αλ), a number slightly under 1, so it shrinks by a fixed percentage. For lasso, the gradient of λ |w| is λ · sign(w), so each weight is pulled toward 0 by a fixed amount αλ per step. A fixed pull is strong enough to hit exactly 0 for small weights, which is why lasso gives sparse solutions while ridge only makes weights small.

|  | Ridge (L2) | Lasso (L1) |
| --- | --- | --- |
| Penalty | ½ ‖θ‖₂² | ‖θ‖₁ |
| Shrinkage per step | proportional to the weight | constant amount toward 0 |
| Weights end up | small, rarely exactly 0 | many exactly 0 (sparse) |
| Good for | many weakly useful features; spreads weight evenly | feature selection |

**2. Data augmentation.** Transform existing examples and add them as new samples, always keeping the label the same. This builds invariances into the model (a slightly rotated cat is still a cat).

- Images: crop and zoom, color changes (contrast, brightness), rotation, translation, stretch, shear. Combine them.
- Text: substitution ("great day" becomes "wonderful day", using a thesaurus or a model), back-translation (translate to another language and back).
- Which transformations work best is decided by cross-validation.

**3. Adding noise.** Many solutions can separate the training data. Adding some noise to the inputs favors the solution with the larger margin (higher confidence), for example w₂ instead of w₁ or w₃ in the slide figure. Too much noise hurts: points cross the decision boundary and the labels become wrong.

**4. Early stopping.** Do not train to the smallest possible training error. Stopping early limits how far the weights can move from their initial values. Recipe:

- Compute the validation error during training.
- Every time it improves, store a copy of the weights.
- When it has not improved for some time, stop.
- Return the stored copy (the best weights, not the last ones).

**5. Dropout.** In each update step, randomly drop units: sample a fresh binary mask over the input and hidden units, multiply it into the units, and update as usual. At test time use all of the weights. Typical drop probabilities: 0.2 for inputs and 0.5 for hidden units. It stops units from relying on each other and acts like averaging many thinned networks. (Implementations also rescale, for example by 1/(1−p), so the average activation matches between training and test; the slides do not go into that.)

**6. Batch normalization.** Recall that standardizing input features, (xᵢ − μᵢ)/σᵢ with statistics from the training data, helps training. Batch norm does the same to the outputs h\_L of each layer before they go into layer L+1, with statistics computed on each batch. It is supposed to help optimization, but the mechanism is poorly understood.

**Summary from the slides.** Linear models are dominated by well-understood penalty methods (ridge, lasso). Neural network regularization is less understood: weight decay and augmentation come from linear models, dropout and batch norm are specific to deep nets, and the choices are usually made by trial and error plus validation.

### Worked example 1: the penalty term and one ridge step

Let w = (3, −4), λ = 0.1, α = 0.1.

```
ridge penalty:  λ · ½ (9 + 16) = 0.1 · 12.5 = 1.25
lasso penalty:  λ · (|3| + |−4|) = 0.1 · 7 = 0.70

If the plain loss is 0.8, the ridge objective is 0.8 + 1.25 = 2.05.

Suppose the data-loss gradient is g = (0.5, 0.5).
ridge gradient = g + λ w = (0.5 + 0.3, 0.5 − 0.4) = (0.8, 0.1)
new w = (3 − 0.1·0.8, −4 − 0.1·0.1) = (2.92, −4.01)

Same thing in weight-decay form:
(1 − αλ) w − α g = 0.99·(3, −4) − 0.1·(0.5, 0.5) = (2.97 − 0.05, −3.96 − 0.05) = (2.92, −4.01)
```

### Worked example 2: ridge shrinks proportionally, lasso by a fixed amount

Take w = (4, −2), α = 0.1, λ = 0.5, and pretend the data gradient is 0 so only the penalty acts.

```
ridge  (multiply by 1 − 0.05 = 0.95):
  step 1: (3.8, −1.9)     step 2: (3.61, −1.805)     step 3: (3.43, −1.71)
  the small weight shrinks less in absolute terms and never reaches 0 exactly

lasso  (move 0.05 toward 0):
  step 1: (3.95, −1.95)   step 2: (3.9, −1.9)       step 3: (3.85, −1.85)
  every weight loses the same 0.05 per step, so small weights reach 0 first
```

### Worked example 3: choosing λ

| λ | Training error | Validation error |
| --- | --- | --- |
| 0 | 1% | 18% |
| 0.01 | 3% | 11% |
| 0.1 | 6% | 9% |
| 1 | 15% | 17% |
| 10 | 30% | 31% |

At λ = 0 the model overfits (tiny training error, big gap). At λ = 10 it underfits (both errors high). Choose λ = 0.1, the lowest validation error. The test set stays untouched until the end.

### Worked example 4: early stopping

Validation error by epoch: 0.40, 0.30, 0.22, 0.20, 0.21, 0.23, 0.26 (training error keeps falling). Stop if there is no improvement for 2 epochs.

```
epoch 4: 0.20 is the best so far, store the weights
epoch 5: 0.21 no improvement (1 in a row)
epoch 6: 0.23 no improvement (2 in a row)  → stop after epoch 6, return the epoch 4 weights
```

### Worked example 5: dropout and batch norm

```
Dropout: hidden activations h = (2, 4, 1, 3), sampled mask (1, 0, 1, 0) → (2, 0, 1, 0)
         units 2 and 4 are skipped in this step and get a new random mask in the next one

Batch norm: a unit's outputs on a batch are 2, 4, 6
         mean = 4,  std = sqrt(((−2)² + 0² + 2²)/3) = sqrt(2.667) = 1.633
         standardized = (−2/1.633, 0, 2/1.633) = (−1.22, 0, 1.22)
```

### Common traps

- Believing ridge sets weights exactly to 0. Lasso does that. Ridge only shrinks.
- Tuning λ on the test set. Use validation or cross-validation.
- Thinking more regularization is always better. Large λ underfits (high bias).
- Dropping units at test time. Training uses random masks, test time uses all weights.
- Augmentations that change the label, such as rotating a 6 by 180° so it looks like a 9.
- Returning the final weights from early stopping. Return the stored best-validation copy.
- Mixing up input standardization (training-set statistics, done once) with batch norm (statistics from each batch, applied to layer outputs).

### Practice: short answer

1. What is regularization? Write the penalized training objective.
2. Write the ridge and lasso penalties. Which one produces sparse weights and why?
3. w = (3, −4), λ = 0.1, α = 0.1 and the data-loss gradient is g = (0.5, 0.5). Compute one ridge update.
4. In the λ table above, which λ would you pick and why? What happens to bias and variance as λ increases?
5. Validation error by epoch is 0.40, 0.30, 0.22, 0.20, 0.21, 0.23, 0.26, with patience 2. When does training stop, and whose weights do you return?
6. Explain how dropout works during training and at test time. Give typical drop probabilities.
7. What is data augmentation, what rule must the transformations follow, and how do you decide which ones to use?
8. What does batch normalization do? Standardize the batch (2, 4, 6).

### Practice: MCQ

**Q1.** Which regularizer tends to set the weights of weak features exactly to zero?

- A. Ridge (L2)
- B. Dropout
- C. Lasso (L1)
- D. Batch normalization

**Q2.** The ridge penalty added to the loss is:

- A. λ · ½ ‖θ‖₂²
- B. λ ‖θ‖₁
- C. λ times the number of nonzero weights
- D. λ times the largest weight

**Q3.** In ridge regression, the update multiplies each weight by (1 − αλ) before the gradient step. This is called:

- A. Sparsity
- B. Early stopping
- C. Batch normalization
- D. Weight decay

**Q4.** Increasing λ in ridge regression generally:

- A. Lowers bias and raises variance
- B. Raises bias and lowers variance
- C. Lowers both bias and variance
- D. Raises both bias and variance

**Q5.** At test time, dropout:

- A. Still drops units at random
- B. Drops only the input units
- C. Uses all of the units and weights
- D. Is replaced by early stopping

**Q6.** With early stopping you should return:

- A. The stored weights from the epoch with the lowest validation error
- B. The weights from the final epoch
- C. The weights from the epoch with the lowest training error
- D. Freshly randomized weights

**Q7.** A valid data augmentation for training a classifier must:

- A. Change the label of the example
- B. Reduce the size of the dataset
- C. Use test images only
- D. Change the input while keeping the label the same

**Q8.** Batch normalization standardizes:

- A. The labels
- B. The outputs of each layer, using statistics computed on each batch
- C. Only the input features, once, using test-set statistics
- D. The loss function

### Answers

**Short answer**

1. Regularization is any method that prevents overfitting or helps optimization. Penalized objective: argmin over θ of (1/n) Σᵢ lossᵢ(θ) + λ R(θ).
2. Ridge: R(θ) = ½ ‖θ‖₂². Lasso: R(θ) = ‖θ‖₁. Lasso gives sparse weights. Its gradient pulls every weight toward 0 by a constant amount αλ per step, so small weights reach exactly 0. Ridge pulls in proportion to the weight, so weights get small but rarely exactly 0.
3. Ridge gradient = g + λ w = (0.5 + 0.3, 0.5 − 0.4) = (0.8, 0.1). New w = (3 − 0.08, −4 − 0.01) = (2.92, −4.01).
4. Pick λ = 0.1, which has the lowest validation error (9%). λ = 0 overfits and λ = 10 underfits. As λ increases, bias goes up and variance goes down.
5. The best validation error is 0.20 at epoch 4. Epochs 5 and 6 do not improve, so training stops after epoch 6 and the stored epoch 4 weights are returned.
6. In each training step, sample a new random binary mask over the input and hidden units, multiply it into the units, and update as usual. At test time use all weights. Typical drop probabilities: 0.2 for inputs and 0.5 for hidden units.
7. Augmentation transforms existing examples (crop, color change, rotation; for text, substitution or back-translation) and adds them as new samples to build in invariances. The label must stay the same. Choose transformations by cross-validation.
8. It standardizes the outputs of each layer before they enter the next layer, using the mean and standard deviation of the current batch. For (2, 4, 6): mean 4, std 1.633, result (−1.22, 0, 1.22).

**MCQ**

1. C. L1 gives sparsity. Ridge only shrinks weights.
2. A. The ridge penalty is λ times ½ of the squared L2 norm. B is lasso.
3. D. Weight decay.
4. B. More regularization means a simpler model: more bias, less variance.
5. C. Test time uses all weights.
6. A. Return the best-validation copy, not the last weights.
7. D. Augmentation changes the input but keeps the label.
8. B. Batch norm standardizes layer outputs using per-batch statistics.

## 11. Perceptron

The perceptron is the simplest neural network: one linear score wᵀx passed through a hard threshold. It learns by looking at examples one at a time and nudging w only when it gets one wrong. It works perfectly when the classes can be separated by a straight line, and it cannot do XOR.

### Explanation

**The model.** Labels are −1 and +1. Fold the bias into w by adding a constant feature 1 to x.

```
score = wᵀx          prediction = +1 if wᵀx > 0, else −1
decision boundary:  wᵀx = 0   (a line in 2-D, a hyperplane in general)
```

It is a linear transformation followed by an activation function (here a step). Early history: McCulloch and Pitts 1943, Rosenblatt 1959, Widrow and Hoff 1960.

**When is the perceptron correct?** When the signs of the score and the label match: y wᵀx > 0. You can also demand a margin: y wᵀx ≥ c, a notion of robustness (how easy the example is).

**Training algorithm.**

```
initialize w = (0, ..., 0)
repeat: pick a datapoint i (randomly or cyclically)
    if y⁽ⁱ⁾ wᵀx⁽ⁱ⁾ is too small (a mistake):   w ← w + y⁽ⁱ⁾ x⁽ⁱ⁾
    else:                                          w unchanged
```

- The slide writes the mistake test as y wᵀx < c. With w = 0 and c = 0 the score is exactly 0, so nothing would ever update. The convention is that a score of exactly 0 counts as a mistake (y wᵀx ≤ 0). The worked example below uses that.
- A positive example (y = +1) that was missed gets x added to w, which tilts the boundary toward it. A negative example that was missed gets x subtracted.
- One pass over all the data is an epoch. Stop when an epoch has no mistakes.

**Why the update helps.** After an update on (x, y), the new score on that same example is:

```
w_newᵀ x = (w + y x)ᵀ x = wᵀx + y ‖x‖²
```

So y · score goes up by exactly ‖x‖². One update moves that example toward the correct side (it may or may not fix it in one step).

**Perceptron as SGD.** The update looks like an SGD step w ← w − α ∇L. We need a loss whose gradient is 0 when we are right and −y x on a mistake. The hinge loss L = max(0, c − y wᵀx) does this, and with α = 1 the step is w ← w + y x.

| y · score | −2 | −0.5 | 0 | 0.5 | 1 | 3 |
| --- | --- | --- | --- | --- | --- | --- |
| hinge loss with c = 1 | 3 | 1.5 | 1 | 0.5 | 0 | 0 |

The loss is large for confident wrong answers and exactly 0 once the example is on the correct side with margin c.

**Representational power: only linearly separable concepts.**

| x1 | x2 | AND | OR | XOR |
| --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 |

- AND is separable: x1 + x2 − 1.5 > 0 only at (1, 1).
- OR is separable: x1 + x2 − 0.5 > 0 except at (0, 0).
- XOR is not separable. Suppose w1 x1 + w2 x2 + b > 0 means class 1. Then (0,1) and (1,0) give w2 + b > 0 and w1 + b > 0, so w1 + w2 + 2b > 0. But (0,0) in class 0 gives b ≤ 0, and (1,1) in class 0 gives w1 + w2 + b ≤ 0, so w1 + w2 + 2b ≤ 0. Contradiction.
- Minsky's 1969 book pointed this out and a long "AI winter" with a halt in funding followed. Multi-layer networks (next sections) fix it.

**Drawbacks.**

- No convergence guarantee if the data are not linearly separable.
- It can overfit. Many perfect classifiers can exist and the algorithm has no mechanism for choosing between them.

**Mistake bound (extra).** How many mistakes can the perceptron make on separable data?

```
D(S) = max over examples of ‖x‖            (data diameter)
γ(S) = max over unit-norm w of the smallest distance from any point to the hyperplane
       (the data margin; distance = |xᵀw| / ‖w‖)

Mistake bound (c = 0):   total mistakes ≤ ( D(S) / γ(S) )²
```

- A small margin γ means the classes are close together and the problem is harder, so the bound is larger.
- Proof idea in two lines. Each mistake raises ‖w‖² by at most D², so after m mistakes ‖w‖ ≤ D√m. Each mistake also raises w*ᵀw by at least γ (w* is the max-margin unit vector), so ‖w‖ ≥ mγ. Together mγ ≤ D√m, so m ≤ (D/γ)².
- Implication: cycling over the data until the number of mistakes stops changing gives a perfect separator. It says nothing about generalization.

### Worked example 1: run the perceptron

Data (x, y): (2, 0) with +1, (0, 2) with −1, (1, 1) with +1, (−1, −1) with −1. Start with w = (0, 0), cycle in this order, mistake if y · score ≤ 0.

```
Epoch 1
  (2, 0),  y = +1: score = 0                      mistake → w = (0,0) + (2,0)  = (2, 0)
  (0, 2),  y = −1: score = 0                      mistake → w = (2,0) − (0,2)  = (2, −2)
  (1, 1),  y = +1: score = 2−2 = 0                mistake → w = (2,−2) + (1,1) = (3, −1)
  (−1,−1), y = −1: score = −3+1 = −2, y·score = 2   correct
Epoch 2
  (2, 0): score 6 ok.   (0, 2): score −2, y·score = 2 ok.
  (1, 1): score 2 ok.   (−1,−1): score −2, y·score = 2 ok.   no mistakes → stop

Final w = (3, −1), total 3 mistakes. Decision boundary: 3 x1 − x2 = 0.
```

### Worked example 2: how one update changes the score

w = (−1, −1), x = (2, 1), y = +1.

```
score = −2 − 1 = −3          y·score = −3 → mistake
new w = (−1,−1) + (2,1) = (1, 0)
new score = 2·1 + 1·0 = 2     change = 2 − (−3) = 5 = ‖x‖² = 4 + 1  ✓
```

### Worked example 3: mistake bound for example 1

The largest point norm is D = √(0² + 2²) = 2. The unit vector w = (3, −1)/√10 separates the data. The smallest of its distances to the points is 2/√10 ≈ 0.632 (for the points (0,2), (1,1), (−1,−1)), so γ(S) ≥ 0.632.

```
bound ≤ (2 / 0.632)² = 10
actual mistakes = 3 ≤ 10  ✓
```

The true max-margin separator gives an even smaller bound, and the actual count can be much lower than the bound.

### Common traps

- Forgetting that a score of exactly 0 counts as a mistake when starting from w = 0.
- Updating when the example is already correct. The perceptron changes w only on mistakes.
- Mixing label conventions. The perceptron uses −1/+1 so that y wᵀx > 0 means correct. Logistic regression cross-entropy uses 0/1.
- Forgetting the sign: a missed negative example gets x subtracted (w − x), because y = −1.
- Expecting convergence on non-separable data. There is none.
- Reading the mistake bound as a generalization guarantee. It only bounds training mistakes.
- Saying a single perceptron can compute XOR. It cannot.

### Practice: short answer

1. Write the perceptron's prediction rule and the condition for an example to be classified correctly.
2. State the perceptron training algorithm, including what happens on a mistake and what happens otherwise.
3. Run the perceptron from w = (0, 0), cycling in the given order, counting a score of exactly 0 as a mistake. Data: (1, 2) with +1, (2, −1) with −1, (−1, 1) with +1, (−2, −2) with −1. Give the final w and the number of mistakes.
4. Which of AND, OR and XOR can a single perceptron compute, and why can it not compute the others?
5. w = (−1, −1), x = (2, 1), y = +1. Do one perceptron update. By how much does the score on x change, and why?
6. State the mistake bound. If D = 5 and γ = 0.5, what is the bound? What happens to the bound if the margin doubles?
7. Give two drawbacks of the perceptron and one difference between the perceptron and logistic regression.
8. With c = 1, compute the hinge loss when y · score equals −0.5, 0.5 and 2. How is the perceptron update related to this loss?

### Practice: MCQ

**Q1.** On a mistake with example (x, y), the perceptron update is:

- A. w ← w − y x
- B. w ← w + y x
- C. w ← w + α (ŷ − y) x
- D. w ← y w

**Q2.** A single perceptron can compute:

- A. XOR
- B. Any Boolean function
- C. Only continuous functions
- D. AND and OR

**Q3.** Statement A: a perceptron is guaranteed to perfectly learn a linearly separable function in a finite number of training steps. Statement B: a single perceptron can compute XOR. Which is correct?

- A. A is true, B is false
- B. Both are true
- C. Both are false
- D. B is true, A is false

**Q4.** If the training data are not linearly separable, the perceptron algorithm:

- A. Converges after one epoch
- B. Converges to the maximum-margin separator
- C. Has no convergence guarantee and may keep updating forever
- D. Sets all weights to zero

**Q5.** The perceptron mistake bound on separable data is:

- A. D / γ
- B. γ / D
- C. (γ / D)²
- D. (D / γ)²

**Q6.** If the margin γ doubles and D stays the same, the mistake bound:

- A. Doubles
- B. Becomes one quarter of its value
- C. Halves
- D. Quadruples

**Q7.** After a perceptron update on (x, y), the quantity y · wᵀx for that same example:

- A. Increases by ‖x‖²
- B. Decreases by ‖x‖²
- C. Stays the same
- D. Becomes 0

**Q8.** The perceptron update can be viewed as an SGD step (with α = 1) on which loss?

- A. Squared error
- B. Negative log-likelihood
- C. A hinge-type loss, max(0, c − y wᵀx)
- D. L1 penalty

### Answers

**Short answer**

1. Predict +1 if wᵀx > 0, else −1. An example is correct when y wᵀx > 0 (sign of score matches the label). A margin version requires y wᵀx ≥ c.
2. Initialize w = 0. Repeatedly pick an example (random or cyclic). If it is a mistake (y wᵀx below the threshold, with 0 counted as a mistake), set w ← w + y x. Otherwise leave w unchanged. Stop when a full pass has no mistakes.
3. Start w = (0,0). (1,2), +1: score 0, mistake, w = (1,2). (2,−1), −1: score 2 − 2 = 0, mistake, w = (1,2) − (2,−1) = (−1, 3). (−1,1), +1: score 1 + 3 = 4, ok. (−2,−2), −1: score 2 − 6 = −4, y·score = 4, ok. Epoch 2: scores 5, −5, 4, −4, all correct. Final w = (−1, 3), 2 mistakes.
4. AND and OR are linearly separable, so one perceptron can compute them. XOR is not: no single line separates {(0,1), (1,0)} from {(0,0), (1,1)}.
5. Score = −3, a mistake. New w = (−1,−1) + (2,1) = (1, 0), new score = 2. The score changes by +5 = y‖x‖² = ‖x‖² (here y = +1 and ‖x‖² = 5), because (w + y x)ᵀx = wᵀx + y‖x‖².
6. Mistakes ≤ (D/γ)². With D = 5 and γ = 0.5: (5/0.5)² = 100. Doubling the margin divides the bound by 4.
7. Drawbacks: no convergence on non-separable data; can overfit because it does not choose among the many perfect classifiers. Difference: the perceptron uses a hard step and gives only a label and updates only on mistakes, while logistic regression uses a sigmoid, outputs a probability, and updates on every example with the NLL gradient.
8. Hinge loss = max(0, 1 − y·score): for −0.5 it is 1.5, for 0.5 it is 0.5, for 2 it is 0. The gradient with respect to w is 0 when the loss is 0 and −y x otherwise, so an SGD step with α = 1 gives w ← w + y x, the perceptron update.

**MCQ**

1. B. Add y x, so a missed negative example is subtracted.
2. D. AND and OR are linearly separable. XOR is not.
3. A. Separable data gives a finite number of mistakes, so statement A is true. A single perceptron cannot do XOR, so B is false.
4. C. There is no guarantee, and on non-separable data updates can continue forever.
5. D. (D / γ)².
6. B. γ appears squared in the denominator, so doubling γ divides the bound by 4.
7. A. The new score is wᵀx + y‖x‖², so y · score rises by ‖x‖² (since y² = 1).
8. C. A hinge-type loss. Its gradient is −y x on a mistake and 0 otherwise.

## 12. Multilayer perceptrons

A multilayer perceptron (MLP) stacks layers of neurons. Each neuron takes a weighted sum of the previous layer, adds a bias, and applies a non-linear activation. Stacking layers lets the network represent things a single perceptron cannot, such as XOR. The last layer is chosen to match the task: linear for regression, sigmoid for two classes, softmax for many classes.

### Explanation

**Why stack?** A single perceptron only represents linearly separable concepts. Gluing many perceptron-like units together gives a neural network that can represent more.

**Layers and notation.** An (L+1)-layer network has an input x = h⁰, hidden layers h¹, h², ..., hᴸ, and an output y = hᴸ⁺¹. Every unit in a layer is connected to every unit in the previous layer (fully connected).

```
one neuron:   a = r( wᵀ x + b )          r = activation function
hidden layer: hⱼ = r( Σᵢ xᵢ wⱼᵢ⁽¹⁾ + bⱼ )     one value per neuron, passed to the next layer

Example (the slides): 2 inputs, 3 sigmoid hidden units, 1 sigmoid output, depth 2
  hⱼ = σ( x₁ wⱼ₁⁽¹⁾ + x₂ wⱼ₂⁽¹⁾ + bⱼ )          j = 1, 2, 3
  ŷ  = σ( h₁ w₁⁽²⁾ + h₂ w₂⁽²⁾ + h₃ w₃⁽²⁾ + b' )
```

- Number of input neurons = number of features. For MNIST (28 × 28 grayscale images) that is 784.
- Number of output neurons = number of classes (10 digits for MNIST), or 1 for regression or binary classification.

**Why we need a non-linear activation.** If every activation were linear, the network collapses into one linear map: W₂ (W₁ x) = (W₂ W₁) x. Composition of linear functions is no more expressive than a single linear function. For W₁ = \[\[1, 2\], \[0, 1\]\] and W₂ = \[\[3, −1\]\], the product is \[\[3, 5\]\], a single layer.

**Activation functions.**

| Activation | Formula | Notes |
| --- | --- | --- |
| Threshold | 1 if z ≥ 0, else 0 | Derivative is 0 everywhere except z = 0, where it is undefined. Hard to optimize, not used in practice |
| Sigmoid | 1 / (1 + e^(−z)) | Output in (0, 1). Flat for large |
| tanh | 2σ(2z) − 1 | Output in (−1, 1), a rescaled sigmoid. Also saturates |
| ReLU | max(0, z) | Cheap, does not saturate for z > 0, helps with the vanishing-gradient problem. Default in modern nets |
| Linear (identity) | z | Useless in hidden layers (network stays linear). Used in the output layer for regression |

From the in-class quiz: the activations typically not used in practice are the threshold function (hard to optimize) and the linear function (no extra expressiveness).

**Output layers.**

| Task | Output | Notes |
| --- | --- | --- |
| Regression | y = wᵀh + b | Linear unit, no nonlinearity |
| Multi-dimensional regression | y = Wᵀh + b | Linear units |
| Binary classification | y = σ(wᵀh + b) | Logistic regression on the hidden features h |
| Multiclass (k classes) | z = Wᵀh + b, y = softmax(z) | k output units, probabilities that sum to 1 |

Softmax: softmax(z)ᵢ = exp(zᵢ) / Σⱼ exp(zⱼ). It turns any vector into a probability vector. If one component dominates, the result is close to a one-hot vector. The slide example: logits (1.3, 5.1, 2.2, 0.7, 1.1) become about (0.02, 0.90, 0.05, 0.01, 0.02).

**Encoding features for a neural net.**

- Nominal features: one-hot. For DNA letters, A = (1,0,0,0), C = (0,1,0,0), G = (0,0,1,0), T = (0,0,0,1).
- Ordinal features: thermometer. small = (1,0,0), medium = (1,1,0), large = (1,1,1). It keeps the order: bigger categories switch on more units.
- Real-valued features: one input unit each, usually scaled or normalized first.

**Counting parameters.** A fully connected layer from n\_in units to n\_out units has n\_in · n\_out weights and n\_out biases. Count connections (weights) only if the question says "connections".

**More hidden units is not automatically better.** A bigger network can fit the training data more closely, but that can mean overfitting. More hidden units does not guarantee better predictions on new inputs.

### Worked example 1: a forward pass with ReLU hidden units

Two inputs, two ReLU hidden units, one sigmoid output.

```
h₁ = ReLU( 1·x₁ − 1·x₂ + 0 )
h₂ = ReLU( 2·x₁ + 0.5·x₂ − 1 )
ŷ  = σ( 1·h₁ + 2·h₂ − 1 )

Input x = (1, 2):
  z₁ = 1 − 2 = −1        h₁ = max(0, −1) = 0      (this neuron is switched off)
  z₂ = 2 + 1 − 1 = 2     h₂ = 2
  output score = 0 + 4 − 1 = 3      ŷ = σ(3) = 0.953

So P(class 1 | x) is about 95%, and we predict class 1.
```

### Worked example 2: a hidden layer computes XOR

Use threshold units. Hidden unit h₁ = \[x₁ + x₂ − 0.5 ≥ 0\] is OR. Hidden unit h₂ = \[x₁ + x₂ − 1.5 ≥ 0\] is AND. Output y = \[h₁ − h₂ − 0.5 ≥ 0\], which means "OR but not AND".

```
(x₁, x₂)   h₁ (OR)   h₂ (AND)   y = XOR
 (0, 0)        0          0           0
 (0, 1)        1          0           1
 (1, 0)        1          0           1
 (1, 1)        1          1           0
```

No single perceptron can do this, but two layers can. Each hidden unit draws one line, and the output unit combines the two lines.

### Worked example 3: counting parameters

```
MNIST network 784 → 100 → 10:
  layer 1: 784·100 weights + 100 biases = 78,500
  layer 2: 100·10  weights + 10 biases  =  1,010
  total = 79,510 parameters

Slide quiz network 5 → 5 → 5 → 1 (input, two hidden layers of 5, one output):
  connections = 5·5 + 5·5 + 5·1 = 25 + 25 + 5 = 55   (weights only)
  with biases add 5 + 5 + 1 = 11, so 66 parameters
```

### Common traps

- Using linear activations in hidden layers. The whole network is then just one linear model.
- Applying a sigmoid in the output layer for regression, or no softmax for multiclass.
- Forgetting the biases when asked for the total number of parameters (or including them when asked for connections).
- Counting layers inconsistently. Check whether the question counts the input layer. In the slide quiz, "3-layer" means three layers of weights: input → hidden → hidden → output.
- Assuming more hidden units always improves generalization.
- Treating an ordinal feature as one-hot. Thermometer encoding keeps the order.

### Practice: short answer

1. Why can a single perceptron not represent XOR, and how does an MLP fix this?
2. Why would a network with only linear activations be no better than linear regression? Show it for W₁ = \[\[1, 2\], \[0, 1\]\] and W₂ = \[\[3, −1\]\].
3. Compute the output of the network in worked example 1 for x = (2, 0).
4. List the output-layer choice (activation and number of units) for: house-price prediction, spam vs not spam, and 10-digit classification.
5. Compute softmax for the logits (2, 1, 0).
6. Encode the nominal feature color ∈ {red, green, blue} and the ordinal feature size ∈ {small, medium, large}. Give vectors for green and for medium.
7. How many parameters (weights plus biases) does a 20 → 50 → 30 → 1 network have?
8. Name two problems with the sigmoid activation in hidden layers and one advantage of ReLU.

### Practice: MCQ

**Q1.** Which activation is typically used in the hidden layers of modern deep networks?

- A. Linear
- B. Threshold
- C. ReLU
- D. Softmax

**Q2.** Why are linear activation functions useless in hidden layers?

- A. They are too slow to compute
- B. A composition of linear functions is still a linear function
- C. They always output 0
- D. Their gradient is infinite

**Q3.** For a 10-class classification problem the output layer should typically have:

- A. 1 sigmoid unit
- B. 1 linear unit
- C. 10 linear units with no normalization
- D. 10 units followed by softmax

**Q4.** A fully connected network has 5 input units, one hidden layer of 4 units, and 1 output unit. The number of weights (without biases) is:

- A. 9
- B. 24
- C. 20
- D. 29

**Q5.** The threshold activation 1{z ≥ 0} is rarely used for training because:

- A. Its output is not bounded
- B. It cannot represent AND
- C. It is too expensive to compute
- D. Its derivative is 0 everywhere except z = 0, so gradient methods get no signal

**Q6.** The ordinal feature small < medium < large is best encoded as:

- A. One-hot: (1,0,0), (0,1,0), (0,0,1)
- B. Thermometer: (1,0,0), (1,1,0), (1,1,1)
- C. A random integer
- D. Dropping the feature

**Q7.** True or false. Statement A: the more hidden units a network has, the better it predicts on new data. Statement B: a 5 → 5 → 5 → 1 network has 55 connections. Which is correct?

- A. Both are true
- B. Both are false
- C. A is true, B is false
- D. A is false, B is true

**Q8.** The output of a regression network should typically be:

- A. Linear: y = wᵀh + b
- B. Sigmoid
- C. Softmax
- D. ReLU followed by softmax

### Answers

**Short answer**

1. XOR is not linearly separable, so no single line (one perceptron) separates its classes. An MLP adds a hidden layer: hidden units such as OR and AND each draw a line, and the output unit combines them (XOR = OR and not AND).
2. The product of linear maps is linear: W₂ (W₁ x) = (W₂ W₁) x. Here W₂ W₁ = \[3·1 + (−1)·0, 3·2 + (−1)·1\] = \[3, 5\], one linear layer, so there is nothing gained from depth.
3. z₁ = 2 − 0 = 2, h₁ = 2. z₂ = 4 + 0 − 1 = 3, h₂ = 3. Output score = 2 + 6 − 1 = 7, ŷ = σ(7) ≈ 0.999.
4. House price: 1 linear unit. Spam vs not spam: 1 sigmoid unit (or 2 units with softmax). 10 digits: 10 units with softmax.
5. exp values 7.389, 2.718, 1.000, sum 11.107. Softmax = (0.665, 0.245, 0.090).
6. One-hot for color: green = (0, 1, 0). Thermometer for size: medium = (1, 1, 0).
7. 20·50 + 50 = 1050. 50·30 + 30 = 1530. 30·1 + 1 = 31. Total = 2611.
8. Sigmoid saturates (flat for large |z|, so gradients vanish) and its slope is at most 0.25, which shrinks gradients through many layers. ReLU does not saturate for z > 0, is cheap, and helps with vanishing gradients.

**MCQ**

1. C. ReLU.
2. B. Composition of linear functions is linear.
3. D. 10 output units with softmax.
4. B. 5·4 + 4·1 = 20 + 4 = 24 weights.
5. D. The derivative is 0 almost everywhere.
6. B. Thermometer encoding keeps the order.
7. D. A is false (more units can overfit), and B is true (25 + 25 + 5 = 55).
8. A. Regression uses a linear output unit.

## 13. Training neural nets and backpropagation

Training a neural network means: pick a loss, find the gradient of the loss with respect to every weight, and take a gradient step. Backpropagation is the part that computes those gradients. It applies the chain rule from the output back toward the input and reuses the numbers saved during the forward pass.

### Explanation

**The training loop (slides).**

```
initialize the weights
until a stopping criterion is met:
    for each training point (x, y):
        1. forward pass:   ŷ = f_w(x)
        2. loss:           L(ŷ, y)          e.g. NLL: −y log ŷ − (1 − y) log(1 − ŷ)
        3. backward pass:  gradient of L with respect to every weight
        4. SGD step:       w ← w − α ∇w L
```

In practice step 4 uses minibatches. Common losses: squared error for regression, NLL (cross-entropy) with a sigmoid for two classes, softmax with NLL for many classes.

**Chain rule.** If l depends on ŷ, ŷ depends on z, and z depends on w, then ∂l/∂w = (∂l/∂ŷ)(∂ŷ/∂z)(∂z/∂w). Multiply the local derivatives along the path from the weight to the loss.

**One neuron, step by step (the slide derivation).** Inputs x₁, x₂, weights w₁₁, w₂₁, and z = w₁₁x₁ + w₂₁x₂, ŷ = σ(z), loss l = −y log ŷ − (1 − y) log(1 − ŷ). Want ∂l/∂w₁₁.

```
∂l/∂ŷ  = −y/ŷ + (1 − y)/(1 − ŷ)  =  (1 − y)/(1 − ŷ) − y/ŷ
∂ŷ/∂z  = σ'(z) = σ(z)(1 − σ(z)) = ŷ(1 − ŷ)
∂z/∂w₁₁ = x₁

multiply:   [ (1 − y)/(1 − ŷ) − y/ŷ ] · ŷ(1 − ŷ) · x₁
          = [ (1 − y)ŷ − y(1 − ŷ) ] · x₁
          = [ ŷ − yŷ − y + yŷ ] · x₁
          = (ŷ − y) x₁
```

So ∂l/∂w = (ŷ − y) x. The gradient is the prediction error times the input. This is the same logistic regression gradient from section 8.

**More layers (slide network).** Two inputs, two sigmoid hidden units a₁₁ and a₁₂, one sigmoid output. Weight w\_ij⁽¹⁾ goes from input i to hidden unit j, and w\_j⁽²⁾ from hidden unit j to the output (biases omitted):

```
z₁₁ = w₁₁⁽¹⁾ x₁ + w₂₁⁽¹⁾ x₂        a₁₁ = σ(z₁₁)         (similarly z₁₂, a₁₂)
ŷ = σ( w₁⁽²⁾ a₁₁ + w₂⁽²⁾ a₁₂ )

∂l/∂a₁₁    = (ŷ − y) w₁⁽²⁾
∂l/∂w₁₁⁽¹⁾ = (∂l/∂a₁₁)(∂a₁₁/∂z₁₁)(∂z₁₁/∂w₁₁⁽¹⁾)
           = (ŷ − y) w₁⁽²⁾ · a₁₁(1 − a₁₁) · x₁
∂l/∂x₁    = (∂l/∂a₁₁)(∂a₁₁/∂x₁) + (∂l/∂a₁₂)(∂a₁₂/∂x₁)      (two paths from x₁ to the loss, add them)
```

**Backpropagation as an algorithm.**

- Forward pass: compute and store every z and every activation.
- Backward pass: start at the output and go back layer by layer.
- At the output with sigmoid and NLL, the error signal is δ\_out = ∂l/∂z\_out = ŷ − y.
- For a weight, its gradient is (error signal of the neuron it feeds) × (value coming in along that weight). Example: ∂l/∂w₁⁽²⁾ = (ŷ − y) a₁₁.
- Pass the error back to an earlier neuron: δ\_j = ( Σ over next-layer neurons k of w\_jk δ\_k ) · r'(z\_j), where r' is the activation's derivative (a(1 − a) for sigmoid, 1 or 0 for ReLU).
- Each intermediate result is computed once and reused for many weights, so it is much cheaper than differentiating each weight separately.
- Modern libraries do this by automatic differentiation (autodiff) on arbitrary computation graphs: go backwards from top to bottom, recursively computing gradients.

**What backprop is and is not (in-class quiz).** Backprop is a gradient-computation algorithm. It is not an optimization algorithm and not a weight-updating algorithm. Optimizers such as SGD use its output to update the weights.

**Practical difficulties.**

- Vanishing gradients: the sigmoid slope is at most 0.25, so multiplying slopes through many layers shrinks the gradient toward 0 (0.25⁵ ≈ 0.001). ReLU (slope 1 for z > 0) helps, and so do residual connections (section 14).
- Random initial weights are likely far from optimal, so initialize cleverly. With many layers there can be numerical difficulties.
- Regularization, a different loss such as cross-entropy, and validation-based tuning all help (section 10).

**Extra (not on the slides): softmax with NLL.** For logits z, probabilities p = softmax(z) and a one-hot true label, ∂l/∂zᵢ = pᵢ − yᵢ. It has the same form as ŷ − y.

### Worked example 1: one neuron

Same numbers as section 8: θ = (2, −1), x = (1, 3), y = 1.

```
z = 2·1 − 1·3 = −1        ŷ = σ(−1) = 0.269
gradient = (ŷ − y) x = (0.269 − 1)·(1, 3) = (−0.731, −2.193)
```

Both entries are negative, so gradient descent increases both weights, which raises the score for this positive example.

### Worked example 2: forward and backward through the 2-2-1 network

Inputs x = (1, −1), label y = 1, no biases.

```
layer 1 weights:  w₁₁ = 0.4,  w₂₁ = 0.2   (into hidden unit 1)
                  w₁₂ = −0.3, w₂₂ = 0.1   (into hidden unit 2)
layer 2 weights:  w₁⁽²⁾ = 0.6,  w₂⁽²⁾ = −0.2

FORWARD
  z₁₁ = 0.4·1 + 0.2·(−1) = 0.2      a₁₁ = σ(0.2)  = 0.5498
  z₁₂ = −0.3·1 + 0.1·(−1) = −0.4    a₁₂ = σ(−0.4) = 0.4013
  z    = 0.6·0.5498 − 0.2·0.4013 = 0.2496       ŷ = σ(0.2496) = 0.5621
  loss = −log(0.5621) = 0.576

BACKWARD
  δ_out = ŷ − y = −0.4379
  layer 2 weights:
    ∂l/∂w₁⁽²⁾ = δ_out · a₁₁ = −0.4379·0.5498 = −0.2408
    ∂l/∂w₂⁽²⁾ = δ_out · a₁₂ = −0.4379·0.4013 = −0.1757
  pass the error to the hidden units:
    ∂l/∂a₁₁ = δ_out · w₁⁽²⁾ = −0.4379·0.6   = −0.2627
    ∂l/∂a₁₂ = δ_out · w₂⁽²⁾ = −0.4379·(−0.2) =  0.0876
  through the sigmoids (slope a(1 − a): 0.2475 and 0.2403):
    δ₁₁ = ∂l/∂z₁₁ = −0.2627·0.2475 = −0.0650
    δ₁₂ = ∂l/∂z₁₂ =  0.0876·0.2403 =  0.0210
  layer 1 weights (error signal × input):
    ∂l/∂w₁₁ = −0.0650·x₁ = −0.0650      ∂l/∂w₂₁ = −0.0650·x₂ = +0.0650
    ∂l/∂w₁₂ =  0.0210·x₁ =  0.0210      ∂l/∂w₂₂ =  0.0210·x₂ = −0.0210

UPDATE with α = 1 (w ← w − α ∂l/∂w)
  w₁₁ = 0.465, w₂₁ = 0.135, w₁₂ = −0.321, w₂₂ = 0.121, w₁⁽²⁾ = 0.841, w₂⁽²⁾ = −0.024

NEW FORWARD PASS
  ŷ = 0.618, loss = 0.482     (was ŷ = 0.562, loss = 0.576)
```

These gradients match a numerical check (nudge each weight by a tiny amount and measure the change in loss). The output weight w₁⁽²⁾ gets a larger gradient than the first-layer weights because the error passes through the sigmoid slopes (about 0.25 each).

### Worked example 3: vanishing gradients

```
A gradient passing back through k sigmoid layers is multiplied by about 0.25 per layer at best:
  k = 1: 0.25     k = 2: 0.0625     k = 5: 0.00098     k = 10: 0.00000095
ReLU has slope 1 where z > 0, so the factor does not shrink.
```

### Worked example 4: softmax gradient

Logits z = (2, 1, 0), true class is the first one (y = (1, 0, 0)).

```
p = (0.665, 0.245, 0.090)      loss = −log(0.665) = 0.408
∂l/∂z = p − y = (−0.335, 0.245, 0.090)
```

The gradient step raises the true class's logit and lowers the other two.

### Common traps

- Dropping the σ'(z) = a(1 − a) factor when going through a hidden sigmoid.
- Using (y − ŷ) and then also subtracting. The gradient is (ŷ − y) and the update subtracts it.
- Saying backprop updates the weights. It only computes gradients; SGD does the update.
- Forgetting that an input or hidden unit feeding several neurons has several paths. Sum the contributions.
- Skipping the forward pass. Backprop needs the stored z and a values.
- Mixing up which weight index goes where. Write down which neuron each weight feeds.
- For ReLU, using a nonzero gradient when z < 0. It is 0 there.

### Practice: short answer

1. List the four steps of the SGD training loop for a neural network. Which are the forward pass and the backward pass?
2. Derive ∂l/∂w₁₁ for a single sigmoid neuron with NLL loss, showing each chain-rule factor.
3. For a single sigmoid neuron with ŷ = 0.8, y = 0, x = (2, −1), θ = (1, 1) and α = 0.5, compute the gradient and the updated θ.
4. What does backprop compute, and what does it not do?
5. In the slide 2-2-1 network, ŷ = 0.7, y = 1, w₁⁽²⁾ = 0.5, a₁₁ = 0.6, x₁ = 2. Compute ∂l/∂w₁⁽²⁾ and ∂l/∂w₁₁⁽¹⁾.
6. Why does ∂l/∂x₁ have two terms in a network with two hidden units?
7. Why do deep sigmoid networks suffer from vanishing gradients? What is the product of five factors of 0.25?
8. Logits are (2, 1, 0) and the true class is the first. What is the gradient of the NLL with respect to the logits?

### Practice: MCQ

**Q1.** Backpropagation is:

- A. An optimization algorithm
- B. A weight-updating algorithm
- C. A regularization method
- D. An algorithm for computing gradients

**Q2.** For a sigmoid output unit with NLL loss, ∂l/∂z equals:

- A. ŷ − y
- B. y − ŷ
- C. ŷ (1 − ŷ)
- D. −y / ŷ

**Q3.** For a single sigmoid neuron with NLL loss, ∂l/∂w equals:

- A. (y − ŷ) x
- B. ŷ (1 − ŷ) x
- C. (ŷ − y) x
- D. (ŷ − y)

**Q4.** In the forward pass of training we:

- A. Compute the gradients
- B. Compute the prediction and the loss, saving intermediate values
- C. Update the weights
- D. Apply dropout at test time

**Q5.** The factor a₁₁(1 − a₁₁) in ∂l/∂w₁₁⁽¹⁾ comes from:

- A. The derivative of the sigmoid activation
- B. The derivative of the NLL loss
- C. The regularization term
- D. The softmax

**Q6.** The main cause of vanishing gradients in deep networks is:

- A. Too many training examples
- B. A learning rate that is too large
- C. Using ReLU activations
- D. Saturating activations whose slopes are small, multiplied across many layers

**Q7.** For a ReLU hidden unit whose input z is negative, the gradient passing through it is:

- A. 1
- B. −1
- C. 0
- D. 0.25

**Q8.** After backprop gives ∇w L, the SGD step is:

- A. w ← w + α ∇w L
- B. w ← w − α ∇w L
- C. w ← ∇w L
- D. w ← w − ∇w L / α

### Answers

**Short answer**

1. (1) Forward pass: compute the prediction ŷ = f\_w(x). (2) Compute the loss L(ŷ, y). (3) Backward pass: compute the gradient of the loss with respect to all weights. (4) SGD step: w ← w − α ∇w L. Steps 1 and 2 are the forward pass, step 3 is the backward pass.
2. ∂l/∂ŷ = (1 − y)/(1 − ŷ) − y/ŷ. ∂ŷ/∂z = ŷ(1 − ŷ). ∂z/∂w₁₁ = x₁. Product = \[(1 − y)ŷ − y(1 − ŷ)\] x₁ = (ŷ − y) x₁.
3. Gradient = (ŷ − y) x = 0.8·(2, −1) = (1.6, −0.8). New θ = (1, 1) − 0.5·(1.6, −0.8) = (1 − 0.8, 1 + 0.4) = (0.2, 1.4).
4. Backprop computes the gradient of the loss with respect to all the weights by applying the chain rule backward through the network. It does not update the weights and it is not an optimizer. SGD or another optimizer uses its output.
5. ∂l/∂w₁⁽²⁾ = (ŷ − y) a₁₁ = (−0.3)(0.6) = −0.18. ∂l/∂w₁₁⁽¹⁾ = (ŷ − y) w₁⁽²⁾ a₁₁(1 − a₁₁) x₁ = (−0.3)(0.5)(0.6)(0.4)(2) = −0.072.
6. x₁ feeds both hidden units, so it affects the loss through two paths (through a₁₁ and through a₁₂). By the chain rule the contributions of the paths add: (∂l/∂a₁₁)(∂a₁₁/∂x₁) + (∂l/∂a₁₂)(∂a₁₂/∂x₁).
7. The sigmoid slope is at most 0.25, so a gradient passing back through many sigmoid layers is multiplied by a number of at most 0.25 per layer and shrinks toward 0. 0.25⁵ = 0.00098, about 0.001. ReLU has slope 1 for z > 0, which avoids this.
8. Softmax is p = (0.665, 0.245, 0.090). The gradient is p − y = (0.665 − 1, 0.245, 0.090) = (−0.335, 0.245, 0.090).

**MCQ**

1. D. Backprop computes gradients. It does not optimize or update.
2. A. ŷ − y.
3. C. (ŷ − y) x. A has the wrong sign, B misses the loss derivative, D drops x.
4. B. Forward pass: prediction and loss, with intermediate values saved for the backward pass.
5. A. It is σ'(z) = a(1 − a), the derivative of the sigmoid hidden activation.
6. D. Saturating activations with small slopes multiplied over many layers.
7. C. ReLU has derivative 0 for negative inputs.
8. B. Subtract the scaled gradient.

## 14. Convolutional neural networks

A CNN is a neural network that uses convolution instead of a general matrix multiply in at least one layer. A small kernel slides over the image and is reused at every position, so a CNN needs far fewer parameters than a fully connected network on images. The exam-friendly parts are the output-size formulas, the parameter count of a conv layer, pooling, and the story from LeNet to AlexNet to ResNet.

### Explanation

**Why not fully connected layers on images?** A fully connected layer connects every input to every output. An RGB image can have 36M floats, so a single hidden layer of 100 neurons already needs about 36M × 100 = 3.6 billion parameters. Convolution fixes this: each output looks only at a small patch, and the same small kernel is reused at every position. CNNs are the default approach for image tasks and are still used even in modern Transformer-based alternatives.

**The convolution operation (as used in the slides).** Slide a kernel (filter) over the input. At each position, multiply the kernel entries with the input patch under it and add them up. The results form the feature map. For a 2 × 2 kernel (w x / y z) over a patch (a b / e f), the output is w·a + x·b + y·e + z·f. The kernel is not flipped.

- Different kernels detect different patterns: edge detection, sharpening, Gaussian blur.
- In a CNN the kernel values are learned by backprop, not designed by hand.

**Padding.** Add rows or columns of zeros around the input.

- Why: (1) keeps edge information, (2) preserves sizes so you can build deep networks (a 32 × 32 image with a 5 × 5 kernel gives 28 × 28 after 1 layer and only 4 × 4 after 7 layers), (3) lets you combine different filter sizes.
- With p\_h padded rows and p\_w padded columns in total, the output is (n\_h − k\_h + p\_h + 1) × (n\_w − k\_w + p\_w + 1).
- Common choice p\_h = k\_h − 1 and p\_w = k\_w − 1, which keeps the size the same at stride 1. For odd k, pad p/2 on each side.

**Stride.** The number of rows or columns the kernel moves each slide. With strides s\_h and s\_w the output shape is:

```
⌊ (n_h − k_h + p_h + s_h) / s_h ⌋  ×  ⌊ (n_w − k_w + p_w + s_w) / s_w ⌋
```

With p = k − 1 this becomes ⌊ (n + s − 1) / s ⌋. Stride 1 is the default; larger strides shrink the output.

**Dilation.** A dilation rate d puts gaps between kernel elements. A k × k kernel with dilation d acts like a (d(k − 1) + 1) × (d(k − 1) + 1) kernel with zeros in the gaps. A 3 × 3 kernel with d = 2 covers a 5 × 5 area, so it sees more context with the same 9 weights. Use that effective size in the output-size formulas.

**Channels.** Color images have several channels (RGB).

- Input is a volume c\_i × n\_h × n\_w (channels × height × width).
- One filter has a kernel for each input channel. You apply them, then sum the results over the c\_i channels to get one output channel.
- With c\_o output channels you have c\_o filters, so c\_i × c\_o kernels in total, and the output is a volume c\_o × m\_h × m\_w.
- Each filter may learn a particular pattern (the first-layer filters of a trained CNN look like Gabor filters, similar to those found in the human visual cortex).

**Conv layer summary.**

```
Input:            c_i × n_h × n_w
Hyperparameters:  number of filters c_o, kernel size k_h × k_w, stride s_h × s_w, dilation d, zero padding p_h × p_w
Output:           c_o × m_h × m_w
Parameters:       k_h × k_w × c_i per filter, total (k_h × k_w × c_i) × c_o      (plus c_o biases)
```

How to pick the hyperparameters: trial and error, hyperparameter tuning or architecture search, or just use an off-the-shelf CNN.

**Pooling.** Another layer type. It slides a window over each channel and summarizes it.

- Max pooling returns the largest value in the window: max(0, 1, 3, 4) = 4.
- Average pooling returns the mean: avg(0, 1, 3, 4) = 2.
- Pooling has padding and stride like conv layers (a common choice is a 2 × 2 window with stride 2, which halves height and width).
- It has no learnable parameters.
- It is applied to each input channel separately, so the number of output channels equals the number of input channels.
- Purpose: reduce spatial size, keep the dominant features, give some translation invariance, reduce overfitting, and enable multi-scale feature extraction.

**Training a CNN.** Same as before: apply a softmax at the end, use a loss (NLL) and backprop with SGD.

**Tasks and architectures.**

- Handwritten digit recognition dates to the 1970s and 1980s. By 2000, an estimated 10–20% of checks in America were processed by software that used CNNs.
- MNIST: 10 classes, 28 × 28 images, centered and scaled, 50,000 training and 10,000 test images.
- LeNet (1989–1999, LeCun et al.): conv → pool → conv → pool → fully connected layers → 10 outputs.
- ImageNet and ILSVRC (2012–2017): thousands of classes, full-resolution images, 14 million images, started 2009 (Deng et al.).
- AlexNet won ImageNet 2012 and was a paradigm shift in computer vision. It is a deeper and bigger LeNet: 1000 classes at the output, larger fully connected layers, more conv layers, more output channels, larger pool size, and larger kernel size and stride for bigger images.
- AlexNet also switched the activations from sigmoid to ReLU (deals with the vanishing gradient problem, since the sigmoid has saturating gradients) and used data augmentation.

**Going deeper: residual connections.** ImageNet error fell as the number of layers grew, but adding too many layers causes optimization problems: vanishing gradients and unstable training. Residual (skip) connections fix this. Instead of learning a transformation of x, the block learns a correction to the identity:

```
output = f(x) + x
```

- If f(x) is near 0 the block just passes x through, so extra layers do no harm.
- The gradient of f(x) + x with respect to x is f'(x) + 1, so the gradient keeps flowing even when f'(x) is tiny.
- ResNet: vastly better performance, no additional parameters (the skip connection is just an addition), records on many benchmarks, and the idea is used in other models including Transformers.

**Why "deep" learning?** Hierarchical feature learning: early layers detect simple patterns (edges), middle layers detect parts of objects, and the last layers detect complete objects.

### Worked example 1: computing a convolution

Input (3 × 4) and kernel (2 × 2) with w = 1, x = 0, y = 2, z = −1. Stride 1, no padding. Output size: (3 − 2 + 0 + 1) × (4 − 2 + 0 + 1) = 2 × 3.

```
input          kernel
1 2 0 1        1  0
3 1 2 0        2 −1
0 2 1 3

position (row 1, col 1): patch (1 2 / 3 1) → 1·1 + 2·0 + 3·2 + 1·(−1) = 6
position (row 1, col 2): patch (2 0 / 1 2) → 2 + 0 + 2 − 2 = 2
position (row 1, col 3): patch (0 1 / 2 0) → 0 + 0 + 4 − 0 = 4
position (row 2, col 1): patch (3 1 / 0 2) → 3 + 0 + 0 − 2 = 1
position (row 2, col 2): patch (1 2 / 2 1) → 1 + 0 + 4 − 1 = 4
position (row 2, col 3): patch (2 0 / 1 3) → 2 + 0 + 2 − 3 = 1

feature map = 6 2 4
              1 4 1
```

### Worked example 2: output sizes

```
Slide quiz: 7×7 input, 3×3 kernel, stride 2, no padding:
  ⌊(7 − 3 + 0 + 2)/2⌋ = ⌊6/2⌋ = 3        → 3×3

32×32 input, 5×5 kernel, stride 1:
  no padding:                (32 − 5 + 0 + 1) = 28          → 28×28
  padding 2 on each side:    (32 − 5 + 4 + 1) = 32          → 32×32 (size preserved)
  stride 2, no padding:      ⌊(32 − 5 + 0 + 2)/2⌋ = ⌊29/2⌋ = 14   → 14×14
  7 layers, no padding:      32 → 28 → 24 → 20 → 16 → 12 → 8 → 4

227×227 input, 11×11 kernel, stride 4, no padding:
  ⌊(227 − 11 + 0 + 4)/4⌋ = ⌊220/4⌋ = 55         → 55×55

7×7 input, 3×3 kernel with dilation 2 (effective 5×5), stride 1, no padding:
  7 − 5 + 0 + 1 = 3                               → 3×3
```

### Worked example 3: parameters and channels

```
RGB input (3×32×32), 6 filters of size 5×5, no padding, stride 1:
  each filter: 5·5·3 = 75 weights   total 75·6 = 450 weights (+ 6 biases = 456)
  output volume: 6 × 28 × 28

For comparison, a fully connected layer from a 28×28 image (784 inputs) to 6·28·28 = 4704 outputs
would need 784 · 4704 = 3,687,936 weights, versus 150 weights for the conv layer with a 5×5 kernel and 6 filters (1 input channel).
```

### Worked example 4: pooling (slide quiz)

```
feature map (4×4)      2×2 windows with stride 2 give a 2×2 output
12 20 30  0
20 12  2  0        average pooling:   (12+20+20+12)/4 = 16     (30+0+2+0)/4 = 8
 0 70  5  2                           (0+70+8+2)/4 = 20        (5+2+90+3)/4 = 25     →  16  8 / 20 25
 8  2 90  3
                   max pooling:       20  30 / 70  90
```

The average pooling result is option B in the slide quiz. Option A is the max pooling result.

### Worked example 5: LeNet shapes and parameters

Setup from the slides (PyTorch): 28 × 28 MNIST image with padding 2 in the first conv so it behaves like LeNet's 32 × 32 input. Each conv is followed by ReLU in that code.

```
input                      1 × 28 × 28
conv1: 6 filters, 5×5, padding 2, stride 1   →  6 × 28 × 28     params 5·5·1·6 + 6      =     156
pool: 2×2, stride 2                          →  6 × 14 × 14     params 0
conv2: 16 filters, 5×5, no padding            → 16 × 10 × 10     params 5·5·6·16 + 16    =   2,416
pool: 2×2, stride 2                          → 16 × 5 × 5       params 0
flatten                                      → 400
fc1: 400 → 120                                                params 400·120 + 120   =  48,120
fc2: 120 → 84                                                 params 120·84 + 84     =  10,164
fc3: 84 → 10 (then softmax)                                   params 84·10 + 10      =     850
total = 61,706 parameters
```

Most of the parameters sit in the fully connected layers, not in the conv layers.

### Worked example 6: residual connection

```
x = 3, f(x) = 0.5   →  block output = f(x) + x = 3.5
If f learns to output 0 (all its weights near 0), the block output is exactly x: an identity map, which is easy to learn.
Gradient with respect to x: f'(x) + 1, so it is at least about 1 when f'(x) is near 0.
```

### Common traps

- Forgetting the stride when computing the output size, or using the floor incorrectly.
- Forgetting that padding is the total added (p\_h = 2 per side means p\_h = 4 in the formula for a 5 × 5 kernel).
- Counting a conv layer's parameters without the number of input channels. It is k\_h × k\_w × c\_i × c\_o.
- Believing pooling has learnable parameters or changes the number of channels. It has none, and channels stay the same.
- Mixing up output size under dilation. Use the effective kernel size d(k − 1) + 1.
- Calling average pooling the max pooling answer (or the reverse) in the slide quiz.
- Saying ResNet adds parameters for the skip connection. It adds none.
- Remembering AlexNet's ReLU as being about speed only. The lecture's reason is the vanishing gradient issue from the sigmoid's saturating gradients.

### Practice: short answer

1. Why are fully connected layers a poor choice for images? Use the numbers from the lecture.
2. Convolve the 4 × 4 input (1 2 0 1 / 3 1 2 0 / 0 2 1 3 / 1 0 2 1) with the kernel (1 0 / 2 −1) using stride 2 and no padding. Give the output size and values.
3. Give the output size for a 32 × 32 input and a 5 × 5 kernel with (a) no padding and stride 1, (b) padding 2 per side and stride 1, (c) no padding and stride 2.
4. A conv layer takes a 3-channel input and has 16 filters of size 5 × 5. How many weights does it have (without biases)? What is the output volume for a 3 × 32 × 32 input with no padding and stride 1?
5. Apply 2 × 2 max pooling with stride 2 to the 4 × 4 map (12 20 30 0 / 20 12 2 0 / 0 70 5 2 / 8 2 90 3). How many learnable parameters does the pooling layer have?
6. What is the effective kernel size of a 3 × 3 kernel with dilation 2? What output size do you get on a 9 × 9 input with stride 1 and no padding?
7. Trace the shapes through LeNet for a 1 × 28 × 28 input as in the slides, up to the first fully connected layer.
8. Name three differences between LeNet and AlexNet, and explain what a residual connection does and why it helps.

### Practice: MCQ

**Q1.** A single-channel 7 × 7 image is convolved with a 3 × 3 kernel, stride 2, no padding. The output size is:

- A. 3 × 3
- B. 7 × 7
- C. 5 × 5
- D. 2 × 2

**Q2.** A conv layer has 3 input channels and 8 filters of size 3 × 3 (no biases). Its number of weights is:

- A. 27
- B. 72
- C. 24
- D. 216

**Q3.** Pooling layers:

- A. Have many learnable parameters
- B. Have no learnable parameters
- C. Change the number of channels
- D. Always use dilated kernels

**Q4.** AlexNet replaced the sigmoid with ReLU mainly to:

- A. Make outputs lie between 0 and 1
- B. Reduce the number of classes
- C. Deal with the vanishing gradient issue (saturating gradients)
- D. Avoid using data augmentation

**Q5.** A residual block outputs:

- A. f(x)
- B. f(x) · x
- C. f(x) − x
- D. f(x) + x

**Q6.** A 32 × 32 input, a 5 × 5 kernel, stride 1 and padding 2 on each side give an output of size:

- A. 32 × 32
- B. 28 × 28
- C. 36 × 36
- D. 30 × 30

**Q7.** A 3 × 3 kernel with dilation rate 2 covers an area of:

- A. 3 × 3
- B. 4 × 4
- C. 5 × 5
- D. 6 × 6

**Q8.** A 4-channel 6 × 6 feature map goes through 2 × 2 max pooling with stride 2. The output shape (channels × height × width) is:

- A. 8 × 3 × 3
- B. 4 × 3 × 3
- C. 1 × 3 × 3
- D. 4 × 6 × 6

### Answers

**Short answer**

1. Every input is connected to every unit, so the parameter count explodes. An RGB image can have about 36M floats, and a hidden layer of only 100 neurons would need about 36M × 100 = 3.6B parameters. A conv layer reuses one small kernel over all positions, so it needs far fewer parameters.
2. Output size with stride 2: ⌊(4 − 2 + 0 + 2)/2⌋ = 2, so 2 × 2. Windows (1 2 / 3 1) → 1 + 0 + 6 − 1 = 6; (0 1 / 2 0) → 0 + 0 + 4 − 0 = 4; (0 2 / 1 0) → 0 + 0 + 2 − 0 = 2; (1 3 / 2 1) → 1 + 0 + 4 − 1 = 4. Output (6 4 / 2 4).
3. (a) 32 − 5 + 1 = 28, so 28 × 28. (b) 32 − 5 + 4 + 1 = 32, so 32 × 32. (c) ⌊(32 − 5 + 2)/2⌋ = ⌊29/2⌋ = 14, so 14 × 14.
4. Each filter has 5·5·3 = 75 weights, so 75·16 = 1200 weights (1216 with biases). Output volume: 16 × 28 × 28.
5. Max pooling gives (20 30 / 70 90). Pooling layers have 0 learnable parameters.
6. Effective size d(k − 1) + 1 = 2·2 + 1 = 5, so 5 × 5. On a 9 × 9 input: 9 − 5 + 1 = 5, so 5 × 5.
7. 1×28×28 → conv1 (6 filters, 5×5, padding 2) → 6×28×28 → pool → 6×14×14 → conv2 (16 filters, 5×5) → 16×10×10 → pool → 16×5×5 → flatten 400 → fc 120.
8. AlexNet is deeper and bigger: 1000 output classes, larger fully connected layers, more conv layers and output channels, larger pool size, and larger kernel size and stride for larger images. Also ReLU instead of sigmoid and data augmentation. A residual connection outputs f(x) + x, so the block learns a correction to the identity. It helps because the gradient is f'(x) + 1 and does not vanish, it adds no parameters, and it makes very deep networks trainable.

**MCQ**

1. A. ⌊(7 − 3 + 0 + 2)/2⌋ = 3, so 3 × 3.
2. D. 3·3·3·8 = 216.
3. B. Pooling has no learnable parameters, and the number of channels is unchanged.
4. C. Vanishing gradients from the sigmoid's saturating gradients.
5. D. f(x) + x.
6. A. 32 − 5 + 4 + 1 = 32.
7. C. d(k − 1) + 1 = 5.
8. B. Channels stay at 4, and 6 × 6 becomes 3 × 3.

## 15. Mixed mock exam

A practice exam in the same style as the real one: multiple choice plus long answer, covering everything. Set a 75-minute timer, use only your cheat sheet and a basic calculator, and write answers before looking at the key. Suggested split: about 25 minutes for Part A and 50 minutes for Part B. Scoring: Part A is 20 points (1 each). Part B is 52 points.

### Part A: MCQ (20 questions)

**Q1.** A spam filter is trained on emails labeled "spam" or "not spam". This is:

- A. Unsupervised learning
- B. Reinforcement learning
- C. Supervised classification
- D. Supervised regression

**Q2.** As k in k-NN grows from 1 to a very large value, the model typically has:

- A. Lower bias and higher variance
- B. Higher bias and lower variance
- C. No change in bias or variance
- D. Higher bias and higher variance

**Q3.** Why standardize features before running k-NN?

- A. So features with large numeric ranges do not dominate the distance
- B. To turn the labels into 0/1
- C. To remove outliers from the training set
- D. To choose k automatically

**Q4.** The information gain of splitting on feature A is:

- A. H(Y | A) − H(Y)
- B. H(Y) · H(A)
- C. The Gini index of A
- D. H(Y) − H(Y | A)

**Q5.** A node has 4 positive and 4 negative examples. Its entropy (base 2) is:

- A. 0
- B. 0.5
- C. 1
- D. 2

**Q6.** Training error is 1% and validation error is 20%. This most likely means:

- A. Underfitting
- B. Overfitting
- C. A well-fit model
- D. High bias

**Q7.** The test set should be used to:

- A. Tune hyperparameters
- B. Pick the best of several models
- C. Train the final model
- D. Estimate generalization performance once, at the end

**Q8.** In 5-fold cross-validation on 100 examples, each model is trained on:

- A. 80 examples
- B. 20 examples
- C. 100 examples
- D. 99 examples

**Q9.** Precision is:

- A. TP / (TP + FN)
- B. TP / (TP + FP)
- C. TN / (TN + FP)
- D. (TP + TN) / total

**Q10.** For screening a rare disease where missing a sick patient is costly, the most important metric is:

- A. Precision
- B. Accuracy
- C. Recall
- D. Specificity

**Q11.** An ROC curve plots:

- A. Precision against recall
- B. Accuracy against the threshold
- C. FPR against precision
- D. TPR against FPR

**Q12.** Expected squared test error decomposes into:

- A. Bias² + variance + irreducible noise
- B. Bias + variance
- C. Bias × variance
- D. Variance − bias²

**Q13.** For L = (1/M) Σ (a x + b − y)², the derivative ∂L/∂b is:

- A. (2/M) Σ (a x + b − y) x
- B. (2/M) Σ (a x + b − y)
- C. (1/M) Σ (y − a x − b)
- D. 2 (a x + b − y) x

**Q14.** In logistic regression, P(y = 1 | x) = 0.5 exactly when:

- A. θᵀx = 1
- B. x = 0
- C. θᵀx = 0
- D. θ = 1

**Q15.** For f(x) = x² with step size 0.5 starting at x₀ = 6, the next iterate x₁ is:

- A. 3
- B. 6
- C. −6
- D. 0

**Q16.** Which regularizer tends to produce sparse weights?

- A. Lasso (L1)
- B. Ridge (L2)
- C. Dropout
- D. Batch normalization

**Q17.** A single perceptron cannot compute:

- A. AND
- B. XOR
- C. OR
- D. A constant function

**Q18.** The output layer for a 5-class classification network should typically be:

- A. 1 sigmoid unit
- B. 5 linear units with no normalization
- C. 5 units with softmax
- D. 1 ReLU unit

**Q19.** Backpropagation computes:

- A. The weight update
- B. The validation error
- C. The learning rate
- D. The gradient of the loss with respect to the weights

**Q20.** A 28 × 28 image is convolved with a 5 × 5 kernel, stride 1, no padding. The output size is:

- A. 24 × 24
- B. 28 × 28
- C. 26 × 26
- D. 23 × 23

### Part B: Long answer (5 questions)

**L1. k-NN and standardization (10 points).** Training data (feature 1, feature 2, label):

| Point | Feature 1 | Feature 2 | Label |
| --- | --- | --- | --- |
| A | 3 | 200 | + |
| B | 8 | 500 | + |
| C | 1 | 100 | − |
| D | 4 | 400 | − |

Query point q = (8, 400). Use Euclidean distance.

- (a) Using raw features, which point is the 1-nearest neighbor and what label is predicted? (2 points)
- (b) Compute the mean and population standard deviation (divide by n) of each feature from the training data, then standardize the four training points and the query. (3 points)
- (c) With standardized features, give the 1-NN prediction and the 3-NN prediction. (3 points)
- (d) Explain why the answers in (a) and (c) differ and which statistics must be used to standardize the query. (2 points)

**L2. Decision tree (10 points).** Binary features X1, X2 and label Y:

| X1 | X2 | Y |
| --- | --- | --- |
| 1 | 1 | Yes |
| 1 | 1 | Yes |
| 1 | 1 | Yes |
| 0 | 1 | Yes |
| 1 | 0 | No |
| 0 | 0 | Yes |
| 0 | 0 | No |
| 0 | 0 | No |

- (a) Compute H(Y). (2 points)
- (b) Compute the information gain of splitting on X1 and on X2. Which is the root? (4 points)
- (c) Draw the next split below the X2 = 0 branch and give its information gain. (3 points)
- (d) Name one way to reduce overfitting in a tree. (1 point)

**L3. Evaluation (8 points).** A classifier on 200 test examples gives TP = 40, FN = 10, FP = 20, TN = 130.

- (a) Compute accuracy, precision, recall, F1 and FPR. (5 points)
- (b) A classifier that always predicts negative: what are its accuracy and recall? What does this say about accuracy here? (2 points)
- (c) What happens to TPR and FPR if the decision threshold is lowered? (1 point)

**L4. Linear regression and gradient descent (12 points).** Data (x, y): (1, 1), (2, 2), (3, 4). Model ŷ = a x + b, loss L = (1/3) Σ (a x + b − y)². Start at a = 0, b = 0 with step size α = 0.1.

- (a) Compute the initial loss. (2 points)
- (b) Compute ∂L/∂a and ∂L/∂b at the start. (3 points)
- (c) Do one gradient descent step and give the new a and b. (2 points)
- (d) Compute the new loss. Did it decrease? (3 points)
- (e) If the step size were 1.0 instead, what would happen to the loss after one step? What does this tell you about step size? (2 points)

**L5. Neural networks (12 points).**

- (a) A perceptron with w = (1, −1) sees x = (1, 2) with label y = +1. Is it a mistake? Give the updated w and the new score on x. (3 points)
- (b) A single sigmoid neuron has θ = (0.5, −0.5). For x = (2, 1) and y = 0, compute ŷ, the NLL loss, the gradient with respect to θ, and the update with α = 0.5. (4 points)
- (c) A CNN takes a 1 × 28 × 28 input, applies 8 filters of size 3 × 3 (stride 1, no padding) with ReLU, then 2 × 2 max pooling with stride 2, flattens, and applies a fully connected layer to 10 outputs. Give the shape after each stage and the total number of parameters (include biases). (4 points)
- (d) Name one reason deep sigmoid networks are hard to train and one fix. (1 point)

### Answers

**Part A: MCQ**

1. C. Labeled examples and discrete labels: supervised classification.
2. B. Larger k smooths the decision boundary: more bias, less variance.
3. A. Features with large ranges would dominate the distance.
4. D. IG = H(Y) − H(Y | A).
5. C. A 50/50 split has entropy 1 bit.
6. B. Tiny training error with a large validation gap is overfitting.
7. D. The test set gives one final, unbiased estimate. Tuning belongs to the validation set.
8. A. 4 folds of 20 = 80 training examples per model.
9. B. Precision = TP / (TP + FP).
10. C. Recall (sensitivity): catch as many true positives as possible.
11. D. ROC plots TPR against FPR.
12. A. Error = bias² + variance + irreducible noise.
13. B. The derivative with respect to b has no x factor.
14. C. σ(0) = 0.5, so θᵀx = 0.
15. D. x₁ = 6 − 0.5·2·6 = 0.
16. A. Lasso (L1).
17. B. XOR is not linearly separable.
18. C. 5 units with softmax.
19. D. Gradients. SGD (or another optimizer) does the update.
20. A. 28 − 5 + 1 = 24.

**Part B: long answer solutions**

**L1.**

```
(a) Raw distances to q = (8, 400):
      D: sqrt((4−8)² + (400−400)²) = 4.0       label −
      B: sqrt(0 + 100²)                 = 100.0     label +
      A: sqrt(25 + 200²)                 = 200.06    label +
      C: sqrt(49 + 300²)                 = 300.08    label −
    1-NN is D, so the prediction is −.

(b) Feature 1: mean = (3+8+1+4)/4 = 4,   std = sqrt((1+16+9+0)/4) = sqrt(6.5) = 2.55
    Feature 2: mean = (200+500+100+400)/4 = 300,   std = sqrt((10000+40000+40000+10000)/4) = sqrt(25000) = 158.1

    standardized (z = (value − mean) / std):
      A: (−0.39, −0.63)     B: (1.57, 1.26)     C: (−1.18, −1.26)     D: (0, 0.63)
      q: ((8−4)/2.55, (400−300)/158.1) = (1.57, 0.63)

(c) Standardized distances to q:
      B: 0.63 (+)     D: 1.57 (−)     A: 2.33 (+)     C: 3.34 (−)
    1-NN is B, so the prediction is +.
    3-NN is B, D, A with labels +, −, +, so the prediction is + (2 votes to 1).
```

(d) With raw features, feature 2 is on a scale of hundreds, so a difference of 100 in feature 2 outweighs a difference of 4 in feature 1 and the distance is driven almost entirely by feature 2. After standardization both features are measured in standard deviations and count comparably, so the nearest neighbor changes. The mean and standard deviation must come from the training data only. Apply the same values to the query.

**L2.**

```
(a) 5 Yes and 3 No:  H(Y) = −(5/8) log₂(5/8) − (3/8) log₂(3/8) = 0.954

(b) X1 = 1: 4 examples (3 Yes, 1 No)  → H = 0.811
    X1 = 0: 4 examples (2 Yes, 2 No)  → H = 1.000
    H(Y | X1) = 0.5·0.811 + 0.5·1 = 0.906        IG(X1) = 0.954 − 0.906 = 0.049

    X2 = 1: 4 examples (4 Yes, 0 No)  → H = 0
    X2 = 0: 4 examples (1 Yes, 3 No)  → H = 0.811
    H(Y | X2) = 0.5·0 + 0.5·0.811 = 0.406        IG(X2) = 0.954 − 0.406 = 0.549

    The root is X2 (the larger gain). The X2 = 1 branch is a pure Yes leaf.

(c) The X2 = 0 branch holds (1,0,No), (0,0,Yes), (0,0,No), (0,0,No): entropy 0.811. Split on X1:
      X1 = 1: 1 example (No)                    → H = 0
      X1 = 0: 3 examples (1 Yes, 2 No)           → H = 0.918
    IG = 0.811 − (1/4)·0 − (3/4)·0.918 = 0.123
```

(d) Prune the tree using a validation set (remove a subtree if validation accuracy does not get worse), or limit the depth.

**L3.**

```
(a) accuracy  = (40 + 130) / 200 = 0.85
    precision = 40 / (40 + 20)    = 0.667
    recall    = 40 / (40 + 10)    = 0.80
    F1        = 2·0.667·0.8 / (0.667 + 0.8) = 0.727
    FPR       = 20 / (20 + 130)   = 0.133

(b) Always negative: accuracy = 150/200 = 0.75, recall = 0.
```

The classes are imbalanced (150 negatives, 50 positives), so 85% accuracy is not as impressive as it sounds, since the useless always-negative classifier already gets 75%. (c) Lowering the threshold predicts more positives, so TPR and FPR both go up (you move up and to the right along the ROC curve), and precision usually falls.

**L4.**

```
(a) L = ((0−1)² + (0−2)² + (0−4)²)/3 = (1 + 4 + 16)/3 = 7

(b) ∂L/∂a = (2/3) Σ (a x + b − y) x = (2/3)(−1·1 − 2·2 − 4·3) = (2/3)(−17) = −11.33
    ∂L/∂b = (2/3) Σ (a x + b − y)   = (2/3)(−1 − 2 − 4)     = (2/3)(−7)  = −4.67

(c) a = 0 − 0.1·(−11.33) = 1.133        b = 0 − 0.1·(−4.67) = 0.467

(d) predictions: 1.133 + 0.467 = 1.600;  2.267 + 0.467 = 2.733;  3.400 + 0.467 = 3.867
    errors: 0.600, 0.733, −0.133
    L = (0.36 + 0.538 + 0.018)/3 = 0.305       the loss fell from 7 to 0.305

(e) α = 1.0: a = 11.33, b = 4.67. Predictions 16.0, 27.3, 38.7 are far off:
    L = (15² + 25.3² + 34.7²)/3 = 689.5, up from 7.
```

The step overshoots so badly that the loss increases (GD diverges). Step size matters: too large overshoots or diverges, too small is slow.

**L5.**

```
(a) score = 1·1 + (−1)·2 = −1, y · score = −1 ≤ 0  →  mistake.
    new w = (1, −1) + (+1)·(1, 2) = (2, 1)
    new score = 2·1 + 1·2 = 4  (change = +5 = ‖x‖²)

(b) z = 0.5·2 + (−0.5)·1 = 0.5       ŷ = σ(0.5) = 0.622
    loss (y = 0) = −log(1 − 0.622) = −log(0.378) = 0.974
    gradient = (ŷ − y) x = 0.622·(2, 1) = (1.245, 0.622)
    new θ = (0.5, −0.5) − 0.5·(1.245, 0.622) = (−0.122, −0.811)
    (check: new z = −1.056, ŷ = 0.258, loss = 0.298, lower than 0.974)

(c) input                         1 × 28 × 28
    conv 8 filters 3×3 + ReLU      8 × 26 × 26     params 3·3·1·8 + 8 = 80
    2×2 max pool, stride 2         8 × 13 × 13     params 0
    flatten                        1352
    fully connected 1352 → 10      10               params 1352·10 + 10 = 13,530
    total = 80 + 13,530 = 13,610 parameters
```

(d) Sigmoid activations saturate and their slope is at most 0.25, so gradients shrink layer after layer (vanishing gradients). Fixes: ReLU activations or residual connections.

## 16. Cheat sheet and exam-day checklist

You are allowed one double-sided cheat sheet, so use it for formulas and recipes you would otherwise have to recall under pressure. This section gives a ready-to-copy cheat sheet, the logistics from the review slides, the traps to re-read the night before, and a suggested plan for the days left.

### Exam logistics (from the review slides)

- Tuesday Oct 20, in class, starting promptly at 8:30am.
- It covers everything taught so far and is a mix of MCQ and long answer.
- One double-sided cheat sheet. Turn it in with the exam.
- Bring pencils and a basic calculator (no graphing calculator).
- Bring an ID (student ID, driver's license, etc.).
- No cell phones, textbooks, etc.

### Suggested plan for the days left

- Thu Oct 15: sections 1–4 (ML basics, k-NN, decision trees, overfitting and validation). Do the practice questions, then check the answers.
- Fri Oct 16: sections 5–8 (metrics, bias and variance, linear regression, logistic regression).
- Sat Oct 17: sections 9–11 (gradient descent and SGD, regularization, perceptron).
- Sun Oct 18: sections 12–14 (MLPs, backprop, CNNs).
- Mon Oct 19: take the mock exam in section 15 under a 75-minute timer, mark it, then fix your cheat sheet from what you missed.
- Tue Oct 20, morning: read the traps below and your cheat sheet. No new material.

### Cheat-sheet text (copy and shrink)

```
ML BASICS / k-NN / TREES
T, P, E: performance on T (measured by P) improves with experience E.
ERM: choose h in the hypothesis class minimizing (1/n) Σ loss(h(xᵢ), yᵢ). Goal: generalize (test error), data i.i.d.
k-NN: Euclidean d = √Σ(xᵢ − yᵢ)². Standardize z = (x − μ_train)/σ_train. Small k: low bias, high variance. Large k: high bias, low variance.
Entropy H(Y) = −Σ p log₂ p.  H(Y|X) = Σ P(x) H(Y|x).  Info gain = H(Y) − H(Y|X).  Greedy splits; prune with a validation set.

EVALUATION
Train / validation (tune) / test (once at the end). k-fold: train on (k−1)/k. LOOCV: k = n. Stratify for class balance.
Accuracy = (TP+TN)/N    Precision = TP/(TP+FP)    Recall = TPR = TP/(TP+FN)    FPR = FP/(FP+TN)    F1 = 2PR/(P+R)
ROC: TPR vs FPR (AUC). PR curve: precision vs recall.
Error = bias² + variance + irreducible noise. Underfit: high bias. Overfit: high variance.

REGRESSION / LOGISTIC
Linear: ŷ = a x + b.  L = (1/M) Σ (ŷ − y)².  ∂L/∂a = (2/M) Σ (ŷ − y) x.  ∂L/∂b = (2/M) Σ (ŷ − y).
MSE, RMSE = √MSE, MAE = mean |ŷ − y|, R² = 1 − SS_res/SS_tot.
σ(z) = 1/(1+e^−z).  P(y=1|x) = σ(θᵀx).  Boundary: θᵀx = 0.  σ' = σ(1−σ).
NLL = −y log ŷ − (1−y) log(1−ŷ).  Gradient = (ŷ − y) x.  Softmax: e^zᵢ / Σ e^zⱼ.  Coin MLE: θ̂ = k/n.

OPTIMIZATION / REGULARIZATION
GD: x ← x − α ∇f(x).  Convex: any stationary point is a global min.  f = x²: x ← (1 − 2α) x, converges iff 0 < α < 1.
SGD: one example or a minibatch per step, unbiased but noisy.  Epoch = one pass.  Updates per epoch = n / batch size.
Min (1/n) Σ loss + λ R(θ).  Ridge R = ½‖θ‖₂² (weight decay: w ← (1 − αλ) w − α∇loss).  Lasso R = ‖θ‖₁ (sparse).
Bigger λ: more bias, less variance. Pick λ on validation.
Early stopping: return the best-validation weights.  Dropout: random mask in training (0.2 input, 0.5 hidden), all weights at test.
Augmentation: transform and keep the label.  Batch norm: standardize layer outputs with batch statistics.
```

```
PERCEPTRON / NEURAL NETS
Perceptron: ŷ = sign(wᵀx), labels ±1. Mistake if y wᵀx ≤ 0 (0 counts) → w ← w + y x. Score on x changes by y‖x‖².
Only linearly separable (AND, OR yes; XOR no). Mistake bound (D/γ)², D = max ‖x‖, γ = max margin. Hinge: max(0, c − y wᵀx).
Neuron: a = r(wᵀx + b). ReLU = max(0, z). tanh = 2σ(2z) − 1. Linear hidden units collapse to one linear map.
Output: regression linear; binary σ; k classes softmax with k units. One-hot (nominal), thermometer (ordinal).
Parameters of a dense layer: n_in · n_out + n_out (connections = weights only).
Training loop: forward, loss, backward, w ← w − α∇w L.
Backprop (chain rule): sigmoid output + NLL: ∂l/∂z = ŷ − y.  ∂l/∂w = (error signal of the neuron) × (input on that weight).
Hidden: δⱼ = (Σ wⱼₖ δₖ) r'(zⱼ).  Sum over paths. Backprop only computes gradients. Vanishing: 0.25ᵏ for sigmoids.

CNN
Output size = ⌊(n − k + p + s) / s⌋ per dimension (p = total padding; same size at s = 1 if p = k − 1).
Dilation: k_eff = d(k − 1) + 1.
Conv params = (k_h · k_w · c_in) · c_out (+ c_out biases). Output volume c_out × m_h × m_w.
Pooling: no parameters, per channel, channels unchanged. Max = largest in window, average = mean.
LeNet (MNIST): conv, pool, conv, pool, FC. AlexNet: deeper and bigger, ReLU (vanishing gradients), augmentation.
ResNet: output = f(x) + x, no extra parameters, gradient f'(x) + 1.
```

### Traps to re-read the night before

- ML basics: hypothesis class is the set of models, a hypothesis is one model. Data without labels means unsupervised.
- k-NN: standardize with training statistics only. k = 1 overfits, a very large k underfits.
- Trees: base-2 logs, weighted average of child entropies, gain is relative to the parent. Greedy splits are not globally optimal.
- Validation: never tune on the test set. k-fold trains on (k − 1)/k of the data.
- Metrics: check the denominators of precision and recall. Accuracy is misleading when classes are imbalanced.
- Bias and variance: more complex models lower bias and raise variance. Noise cannot be removed.
- Linear regression: the b-gradient has no x. The a-gradient has x. Watch the 2/M.
- Logistic regression: boundary at θᵀx = 0. Gradient (ŷ − y) x. Labels 0/1 here, ±1 for the perceptron.
- GD: subtract the gradient. Too large a step size diverges. SGD is noisy but unbiased.
- Regularization: lasso gives exact zeros, ridge does not. Dropout is off at test time. Early stopping returns the best-validation weights.
- Perceptron: a score of exactly 0 is a mistake. Update only on mistakes. XOR is impossible for one perceptron.
- MLPs: no nonlinearity means no extra power. Count biases unless the question says connections.
- Backprop: keep the σ' = a(1 − a) factor. It computes gradients and does not update weights.
- CNNs: the formula uses total padding. Parameters include input channels. Pooling has no parameters.

### Exam tactics

- Do a first pass through the MCQs and mark any that need computation. Return to them after the easy ones.
- On long answers, write the formula first, then plug in the numbers, then state the final answer. Partial credit follows the steps.
- Keep four decimals in intermediate steps and round at the end.
- Check signs and directions at the end: gradient descent subtracts, loss should go down after a good step, probabilities should sum to 1, output sizes should be whole numbers.
