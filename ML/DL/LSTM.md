### OVerview
Traditional neural networks do not store information about the previous events. RNN addresses this issue since it uses networks with loops. A loop allows the passing of information from one step of the network to the next step. LSTMs are a special kind of RNN. Sometimes, we only need to look at the recent information to perform the present task, thus in places where there is short-range dependency, i.e. the gap between the relevant information and the place where it is needed is small RNNs can be applied to learn the past information. However, RNNs are unable to learn when there are long-term dependencies, i.e. when the gap is large. The fundamental reasons behind this is

    1. vanishing and exploding gradients: gradients for weight update can shrink exponentially during backpropagation. Conversely,  gradients can also become very large, leading to instability in training.
    2. limited memory capacity: Important information can be lost or diluted as it passes through many time steps.
    3. Non-linear Activation Functions: RNN uses sigmoid and tanh activation functions. These saturate for very large or small input values, leading to a vanishing gradient problem.
    4. Fixed-length memory: RNNs have a fixed-length internal state. Thus, important earlier information can be forgotten or overwritten.
LSTMs solve long-term dependency problems.

### The Core Idea Behind LSTMs
The important thing to note about LSTM is the cell state, the horizontal line running through the top of the diagram. 
![image](https://github.com/user-attachments/assets/907329a9-325e-4d87-a7bb-eefe04823bb6)

Gates are structures which regulate the removal or addition of information to the cell state. They are composed out of a sigmoid neural net layer and a pointwise multiplication operation. There are three gates in LSTM: input date, forget gate and output gate.

![image](https://github.com/user-attachments/assets/589a8cea-ccf3-4926-8664-7a294378365c)
