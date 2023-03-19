# 5044-Robust-Explanation-for-Free-or-At-the-Cost-of-Faithfulness
This repo contains the manuscript for ICML 2023 submission *Robust Explanation for Free or At the Cost of Faithfulness*. 

## Revison Summary
We thank all reviewers for their interesting questions, insightful comments, and helpful suggestions.   

We have made several revisions to our manuscript(see main.pdf) in response to reviewer feedback. We have marked the additions in blue for the convenience of the reviewers. Notably, we have added new content to the following sections:

* In the last paragraph of Section 4.1, we have emphasized the connection between Theorem 4.1 and robust attribution by regularization.
* In Section 4.2, we have highlighted the implications of Theorem 4.3 and shown how it unifies and generalizes prior works. We believe that Theorem 4.3 could encourage the development of more robust explanation methods.
* In Section 5.1, we have summarized the intuition behind why a robustness-faithfulness trade-off exists.
* In Remark 5.1, we have provided additional emphasis, as suggested by Reviewer X33x, on how faithfulness should be computed in practice for different data types.
* We have included the tiny Swin Transformer as a backbone model and provided additional results on Swin Transformer in Table 1 and Figure 4-6, as requested by Reviewer X33x.
* In the last paragraph of Section 6.2, we have clarified our conclusion drawn from Figure 4 and Table 1, as pointed out by Reviewer UjeD.
* We have added more derivations of the local Lipschitz of LIME in Appendix C.4.2, in response to Reviewer UjeD's comments.

## Some Important Questions
### Robust models have robust explanations

While it's been shown in previous works that explanations for robust models are generally more robust than those for non-robust models, we take a systematic approach to relate the local Lipschitz of six post hoc explanation methods to the local Lipschitz and smoothness of the explained model. To our knowledge, most of these relationships have not been explored in prior research. Our analysis reveals that if we already have a robust model, we need not worry about explanation robustness, as robust explanations can be obtained "for free." Many efforts have been dedicated to developing robust attribution methods, often achieved by adding an explanation regularization term to the loss function and retraining the model (as mentioned in lines 53-57). Our findings suggest that these methods may be equivalent to training a robust model with a smaller local Lipschitz. Theorem 4.1 sheds light on the underlying mechanisms behind their success.

### Achieving robust explanations by smoothing
While previous work has discussed similar results, our findings provide a more general and unifying result that builds upon previous research. 

Our study proves in Theorem 4.3 that any distribution that satisfies the conditions specified in the theorem is a promising candidate for smoothing. By composing the function $f$ with a kernel induced by this distribution, we can obtain more robust attributions. Our results go beyond the previous studies by providing theoretical justification for UniGrad and SmoothGrad, which were not supported by theoretical proofs in their papers. Our study may encourage researchers to explore other distributions and develop new smoothing techniques such as XXGrad in the future.

Moreover, while SmoothGrad is a gradient explanation on $f\_\sigma(x) = \mathbb{E}_{\epsilon \sim \mathcal{N}(0, \sigma^2 I)}[f(x + \epsilon)]$, we can also compute explanations on $f\_\sigma$ with other methods such as GI, IG, LIME, and SHAP. Our findings demonstrate that explanations returned by these methods are more robust. Theorem 4.3 supports this argument for uniform distribution, Gaussian distribution and other appropriate distributions as well. Therefore, our study may inspire future work to design other explanation methods such as UniformLIME, GaussianLIME, UniformSHAP, GaussianSHAP, and so on, to improve explanation robustness for any model and any explanation method.

### Why trade-off exists

Firstly, our intuition is that *over-smoothing may lead to the loss of information*. For instance, consider a model $f$ that reasons differently on two similar inputs $x$ and $x^\prime$, but outputs the same prediction (see Figure 1 [1] for an example). Suppose $f$ utilizes feature $i$ of $x$ and feature $j\neq i$ of $x^\prime$ to make predictions. A faithful explanation method $\phi$ should produce explanations $\phi(x)$ and $\phi(x^\prime)$ that reveal this difference. However, if $\phi$ indeed does so, then $\phi(\mathbf{x})$ and $\phi(x^\prime)$ must be significantly different, indicating that this explanation method is not robust. In contrast, consider another explanation method $\rho$ that is regarded as robust. Then, $\rho(x)$ and $\rho(x^\prime)$ are similar, implying that $\rho$ weights feature $i$ and $j$ similarly on $x$ and $x^\prime$. However, the notable difference in the reasoning of $f$ on $x$ and $x^\prime$ becomes negligible, indicating that $\rho$ is not faithful.

Secondly, another perspective is that an explanation method that includes a great deal of noise is quite non-robust and also unfaithful since it reveals little information about the reasoning process of the model. Conversely, an explanation method that produces nearly constant explanations is highly robust, but both unfaithful and uninformative. Thus, a faithful explanation method is neither the most robust nor the least robust one. The robustness of a faithful explanation method should ***match*** the robustness of the model it explains. When $f$ is not robust on $x$, we should not expect a faithful explanation method $\phi$ to output a robust explanation on $x$. Therefore, we may lose faithfulness due to an over-robust explanation method. **Furthermore, we may potentially gain faithfulness by eliminating noise in explanations, thereby rendering them more robust.** 

The trade-off between robustness and faithfulness also applies to robust models. Our theoretical results do not assume $f$ to be non-robust, and hence, this trade-off holds for both robust and non-robust models. Our intuition suggests that over-smoothing explanations results in information loss, leading to a decrease in faithfulness, as well as an inability to reveal heterogeneous information. Therefore, the statements made above hold for both non-robust and robust models.


### Reference
[1] Chirag Agarwal et.al., Rethinking Stability for Attribution-based Explanations, ICLR 2022 Workshop PAIR^2Struct 
