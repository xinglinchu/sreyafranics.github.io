# Attention is not Explanation (Jain and Wallace)

[Link](https://arxiv.org/abs/1902.10186)

# Key Contributions:
Li et al. (2016) summarized this commonly held view in NLP: “Attention provides an important way to explain the workings of neural models".

Assuming attention provides an explanation for model predictions, we might expect the following properties to hold. 

(i) Attention weights should correlate with feature importance measures (e.g., gradient-based measures); 

(ii) Alternative (or counterfactual) attention weight configurations ought to yield corresponding changes in prediction (and if they do not then are equally plausible as explanations). 

The authors show that NEITHER property is consistently observed by standard attention mechanisms in the context of *text classification*, *question answering (QA)*, and *Natural Language Inference (NLI)* tasks.

**Shocking** It is very often possible to construct ‘adversarial’ attention distributions that yield effectively equivalent predictions as when using the originally induced attention weights, despite attending to entirely different input features. Even more strikingly, randomly permuting attention weights often induces only minimal changes in output.
