# Semantically Equivalent Adversarial Rules for Debugging NLP Models (Ribeiro et al.)
[Paper](http://aclweb.org/anthology/P18-1079) and [Code](https://github.com/marcotcr/sears)

Complex models for RC and VQA are prone to brittleness; different ways of phrasing the same sentence often cause the model to output different predictions. This is called *oversensitivity*. In another way, adding a new word can have no semantic impact at all (known as *overstability*). 

<img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/sear-main.png" alt="drawing" width="400"/> 
It is possible to fool the model by adversarially changing a single character (c), but at the cost of making the question nonsensical. A **Semantically Equivalent Adversary** (d) results in an incorrect answer while preserving semantics.

## Main Contributions
1. Inspired by adversarial examples for images, they introduce semantically equivalent adversaries (SEAs) – text inputs that are perturbed in semantics-preserving ways, but induce changes in a black box model’s predictions. These are representative of real world scenarios compared to malicious attacks like misspellings, scrambling, removing words, etc.
2. Derivation of some semantics-preserving rules - semantically equivalent adversarial rules (SEARs), which are more globally applicable rather than perturbations to individual instances.
3. SEAs and SEARs, designed to unveil local and global oversensitivity bugs in NLP models.
4. Paraphrase generation techniques are used for to generate SEAs (model-agnostic). Next SEAs are generalized into semantically equivalent rules (SEARs). 

## Definition
Consider a black-box model <a href="https://www.codecogs.com/eqnedit.php?latex=$f$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$f$" title="$f$" /></a> that takes a sentence <a href="https://www.codecogs.com/eqnedit.php?latex=$x$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$x$" title="$x$" /></a> and produces <a href="https://www.codecogs.com/eqnedit.php?latex=$f(x)$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$f(x)$" title="$f(x)$" /></a>. A semantically equivalent adversary (SEA) is one that changes model prediction. SemEq(<a href="https://www.codecogs.com/eqnedit.php?latex=$x$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$x$" title="$x$" /></a>,<a href="https://www.codecogs.com/eqnedit.php?latex=$x^'$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$x^'$" title="$x^'$" /></a>) measures the semantic equivalence of its arguments (0 to 1). The following equation defines SEA:

<img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/sear-sea.png" alt="drawing" width="400"/>

To generate paraphrases, the approach by [1] is followed : A sentence <a href="https://www.codecogs.com/eqnedit.php?latex=$x$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$x$" title="$x$" /></a> is translated into multiple pivot languages then back-translating back to the original language. The score for a particular paraphrase is computed by the heuristic above. 

## Generating SEARs

<img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/sear-gen.png" alt="drawing" width="400"/>

This algorithm says once SEAs has been generated for all instances, they are generalized into certain candidate rules. Like (*What color* -> *Which color*), (*What NOUN* -> *Which NOUN*) and (*WP color* -> *Which color*). Once the candidate rules are proposed, they are filtered based on thresholds on **semantic equivalence**. After the filtration step, a submodular optimization is performed to increase **coverage** of rules while avoiding **redundancy**.

### Example rules
<img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/sear-rules.png" alt="drawing" width="600"/>

### Example SEARs in different problem settings
<img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/sear-mc.png" alt="drawing" width="280"/> <img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/sear-vqa.png" alt="drawing" width="280"/> <img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/sear-sent.png" alt="drawing" width="280"/>

In Table 1, The rule (*What VBZ* → *What’s*) shows that the model is fragile with respect to contractions (flips 2% of all correctly predicted instances on the validation data). The second rule uncovers a bug with respect to simple question rephrasing, while the third and fourth rules show that the model is not robust to a more conversational style of asking questions.

In Table 2, Changes induced by these four rules flip more than 10% of the predictions in the validation data, which is of critical concern if the model is being evaluated for production.

In Table 3, Surprisingly, many of its predictions change for perturbations that have no sentiment connotations, even in the presence of polarity-laden words.

## Observations
1. *Can humans generate good adversaries?* Humans generate paraphrases differently than this method: the average character edit distance of SEAs is 6.2 for VQA and 9.0 for Sentiment, while for humans it is 18.1 and 43.3, respectively. Also humans can generate adversaries that: (1) make use of the visual context in VQA, which SEAR method does not, and (2) significantly change the sentence structure, unlike the SEAR method.

2. *Can experts find high-impact bugs?* Discovering these bugs is hard for humans (even experts) without SEARs: not only do they need to imagine rules that maintain semantic equivalence, they must also discover the model’s weak spots. 

3. *Fixing bugs using SEARs* Once impactful bugs are identified, a simple data augmentation procedure is used: applying SEARs to the training data, and retraining the model on the original training augmented with the generated examples.

## Limitations and Future Work
1. *Semantic scoring errors* : Paraphrasing is still an active area of research, and thus the proposed semantic scorer is sometimes incorrect in evaluating rules for semantic equivalence.

2. *Paraphrasing* : Paraphrase models based on neural machine translation are biased towards maintaining the sentence structure, and thus do not produce certain adversaries which humans can produce.

3. *Better bug fixing* : Developing more effective ways of leveraging the expert’s time to close the loop, and facilitating more interactive collaboration between humans and SEARs are exciting areas for future work.

## References
1. Mirella Lapata, Rico Sennrich, and Jonathan Mallinson.2017. Paraphrasing revisited with neural machine translation. In EACL.
