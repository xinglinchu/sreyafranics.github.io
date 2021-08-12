# Rationalizing Neural Predictions (Lei et al.)

[Paper](https://people.csail.mit.edu/taolei/papers/emnlp16_rationale.pdf) and [Code](https://github.com/taolei87/rcnn)

<img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/rat-example.png" alt="drawing" width="400"/>

# Main Contributions

1. Rationale generation from input text. Rationales satisfy two key properties : (a) selected words represent short and coherent pieces of text, (b) the selected words must alone suffice for prediction as substitute for the original text. In the figure above *a very pleasant ruby red-amber color* is the rationale for the underlying decision.

2. Assumption : The model with rationales is trained on the same data as the original neural models, without access to additional rationale annotations. 

3. Two modular components : (a) **Generator** : specifies a distribution over possible rationales (extracted text). (b) **Encoder** : Maps any such text to task specific target values. They are trained jointly to minimize a cost function that favors short, concise rationales while enforcing that the rationales alone suffice for accurate prediction.

4. Evaluation of the method done on the beer review corpus (Multi-aspect Sentiment Analysis) and the problem of retrieving similar questions.

# Cost function

<img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/rat-loss.png" alt="drawing" width="400"/>

<img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/rat-reg.png" alt="drawing" width="400"/>

<img src="https://github.com/anirbanl/anirbanl.github.io/blob/master/img/notes/rat-cost.png" alt="drawing" width="400"/>
