# Pruning in neural network
Weight/Unit pruning in Tensorflow

Given a layer of a neural network ReLU(xW) are two well-known ways to prune it:

  - Weight pruning: Set individual weights in the weight matrix to zero. This corresponds to deleting connections as in the figure above. Here, to achieve sparsity of k% I rank the individual weights in weight matrix W according to their magnitude (absolute value) , and then set to zero the smallest k%.

  - Unit/Neuron pruning: Set entire columns to zero in the weight matrix to zero, in effect deleting the corresponding output neuron. Here to achieve sparsity of k% we rank the the columns of a weight matrix according to their L2-norm and delete the smallest k%.
  
###### References
[EXPLOITING SPARSENESS IN DEEP NEURAL NETWORKS FOR LARGE VOCABULARY SPEECH RECOGNITION](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=6288897&tag=1)

## Conclusion
After implementing the weight pruning and neuron pruning methods, I noticed that to have good performance after pruning the final distributions of the weights plays a crucial role. The overall dependence of final distribution is on the learning rate. The initial learning rate used was 1e-5. In this case, the final accuracy, after training over 50 epochs was ~98%. But, I can distinguish that the distributions of the weights had a flat top. In this case, the test accuracy dropped very early (smaller values of k) in the case of weight as well as unit pruning. The learning rate was then increased to 0.001 and the final accuracy after training was ~98%. In this case it was noticed that the distributions of the weight matrices were proper gaussians. In this case the the accuracies didn't drop for early values of k (accuracies did not degrade rapidly with increasing k) and the performance was much better.

It was also noticed that the test accuracy initially increases a little bit in weight pruning. This shows that the network overfits the training set, which depicts that pruning has a regularizing effect and it decreases the generalization error. Though fundamentally different, L1 regularization also leads to a sparse solution. I believe the main intuition behind pruning is that weights with small magnitudes do not have much effect on the outputs. The weight distribution above also shows that the majority of the weights have extremely small values hence zeroing them does not
have much effect. The above referenced paper shows that ~70% of the weights of the neural network are small and can be removed.

As per the experiemnt, I see unit weight pruning performance much better than unit pruning, in unit pruning we sets the entire columns to zero, which results in zeroing out of the output neuron. We are necessarily removing conclusive features which are necessary for classification. First, we remove those features whose performance does not decrease i.e., features which are not useful. As k increases, we keep on removing useful features and performance degrades more quickly. As we see that the cloumn norms have lesser values with lower magnitudes hence its performance degrades faster. Apart from this, we believe that the network learns redundant features, thus removing some features (or weights) won't have any effect. Although, since we are removing features based on magnitude, this may be one of those things that fairly contributed to achieve the observed result.

Here we see that the dense evaluation is slower than sparse evaluation for both weight and unit/neuron pruning. As we see that increasing the sparsity decreases evaluation time of sparse evaluation.
