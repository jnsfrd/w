# Output Privacy and Federated Machine Learning

Federated machine learning (FL) has emerged as a promising approach to privacy-preserving machine learning. By removing the need to centralize data, FL improves [input privacy and solves the copy problem in machine learning](https://www.scaleoutsystems.com/post/the-copy-problem-in-machine-learning). Much current research in FL focuses on various aspects of the computational schemes, such as model aggregation, resource allocation, and system heterogeneity. Security and privacy concerns are also critical areas of focus. Safeguarding the integrity of federated machine learning systems requires a nuanced approach, given the distinctive dynamics of model training in a decentralized environment.

When considering security and privacy requirements of federated learning frameworks, we distinguish between input privacy and output privacy. While input privacy pertains to the forward training process, output privacy is concerned with what can be learned about input data from access to the trained model. Output privacy is a concern for any machine learning model whose predictions are made available to end-users. Deliberate attempts of model stealing or breaches of output privacy are often referred to as _reverse engineering attacks_.

### Model Reverse Engineering

Reverse engineering a model involves obtaining its parameters and architectural details when only a black box is accessible. We can broadly distinguish between attempts to “steal a model”, and attempts to “invert a model”. In model stealing the goal is to replicate a model's internals and its predictive ability. _Model inversion attacks_ aim to reconstruct training data or learn properties of that data. Several research studies have demonstrated attacks aimed at retrieving not only the parameters but also the raw data used to train the models \[3, 4, 5\].

An example of model stealing is a _model extraction attack where the_ adversary seeks to replicate machine learning models using repeated queries to a machine learning model. The study \[3\] highlighted that by analyzing the responses obtained from the API of a deep neural network service in a sequence of queries, it became possible to infer the hyperparameters of deep neural networks hosted on prominent platforms like Big ML and Amazon ML on AWS services.

Much of the literature on adversarial machine learning assumes a centralized environment where access to both data and computing are available as one coherence unit. In the case of federated training and inference processes this assumption does not hold. This leads to a complex interplay between input and output privacy in federated environments.

### Output Privacy in the context of federated machine learning

Breaches of output privacy refers to situations where a trained model is compromised, resulting in the leakage of information about the training data. By accessing the model the attacker can try to reverse engineer properties of the input data, or try to reconstruct complete data points in the training data. The reverse engineered information can range from high-level aggregate information, such as the overall distribution of training contributions, to precise specifics about the training data such as an image present in the training dataset. These types of risks are not in any way unique to federated machine learning systems, rather they are a concern for any machine learning model that is made available to end-users either as an API or otherwise.

However, the decentralized nature of federated learning  introduces additional complexity in the training process and it is important to understand the differences between output breaches in centralized and federated environments. For example, studies have shown that attacks with significant impact on centralized models can have limited effects on federated models \[2\].

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/65bbe79eb6a562bc878e16c5_649179e0fbb3a410da3ad92d_ovw.jpeg)

In a centralized case, if a trained model is compromised, the process of reverse engineering can expose not only the distribution of the training data but also the actual data and labels used for model training. In federated learning, it additionally becomes important to determine whether the attacker can gain insight into e.g. the number of clients involved in the training process, their identities, the individual local data distribution, or individual records from participating clients. The leakage of such information in federated settings can have consequences that are not present in centralized training and inference approaches. For example, there can be damage to a participating organization’s reputation.

In technical terms, statistical disclosure, model inversion, and membership inference attacks are the key to address to manage output privacy. The three main attack types are _targeted_, _backdoors,_ and _untargeted_ attacks \[8,9\]. All these attacks are triggered during the training process that is in the domain of input privacy. To mitigate the effect of these attacks on model inference, which is part of output privacy, it is important to understand the connection between input and output privacy, particularly in the context of federated learning. By strengthening input privacy, we can enhance output privacy, impeding the reverse engineering process and safeguarding individual client data.

### Practical feasibility of attacks on federated learning systems

Considering the practical aspects of reverse engineering, it is important to be mindful about the underlying assumptions made in research studies. Often, simplifying assumptions are made about the training setup and/or the machine learning model to allow for theory development and qualitative reasoning. Also, studies tend to be conducted using benchmark datasets (such as MNIST and CIFAR-10). As a consequence, there is a risk that studies exaggerate risks.

Researchers from Google recently reported that there are studies on privacy concerns in federated learning where the presented scenarios are unrealistic. For example, take attack scenarios that assume up to 25% compromised clients in a federation. From a research perspective, this might seem plausible, but is in fact a highly unlikely scenario in practice.  Google reported that one Billion mobile phones use GBoard where federated learning is instrumental, and here 25% compromised clients would mean 250 million compromised mobile phones. If this was the case, the system developers would have much bigger problems than a compromised ML training process(!)

In the cross-silo context, researchers from Nvidia recently looked specifically at _gradient inversion attacks_ on federated learning systems \[10\]. Gradient inversion is a type of attack that could target FL systems, since it leverages potential access to the gradients (or weights) sent between clients and server. The idea is to try to construct a generative model that can synthesize data close to the training input data. The researchers conducted well-designed experiments attempting to use this idea to reverse-engineer chest X-ray images under a range of conditions. They conclude that previous work has likely exaggerated the risks of gradient inversion in FL, assuming that the federated training uses realistic batch sizes, number of training images at each client site, and a reasonable number of local iterations. In addition, several “low-hanging fruits” can be easily incorporated in a FL system to make attacks futile.

More research is needed using realistic machine learning scenarios and production grade FL implementations in order to learn more about practical costs and constraints of different attacks. However, there are several qualitative insights we can draw  from studies on benchmark problems in sandbox settings. For example, we know that the risk of model inversion increases with:

*   Very few training data points at a client site
*   Small batch sizes
*   Few local iterations before aggregation

These are all parameters that the use-case owner / developer of the machine learning model  has a great deal of control over. Awareness of the risks here aids in setting up and governing the federated training process.

Also, to succeed with a gradient inversion attack adversary needs to have access to:

*   The local gradient/parameter update(s) from clients
*   The state of the current global model that updates were computed from
*   The model architecture

There are several relatively straightforward measures that can and should be taken to mitigate the risks for leakage of this information outside of the federation.

### Ways to increase security and privacy of (federated) machine learning systems

**Layering FL with other privacy-enhancing technologies.** Approaches like multi-party computations and homomorphic encryption can be used to implement secure aggregation - an additional level of protection against leakage of weights or gradients sent to the aggregator \[7\]. However, challenges include performance and scalability. Differential privacy, where controlled noise is added either during the local training process or when weights are aggregated, can also reduce risk of reverse engineering. A problem here though is a degradation of final model accuracy, so a trade-off between noise and model accuracy needs to be made \[6,10\].

**Regularization.** Another approach to enhance output privacy involves the use of regularization techniques during loss calculation and the aggregation process. Regularization strengthens anonymization without compromising model accuracy. A number of studies have highlighted the benefits of incorporating regularization terms. Recently, T. Wang and the team published a study explaining the impact of regularization to model inversion attacks \[1\].

**Engineering approaches.** In addition to ML-specific solutions, data engineering solutions can also contribute to supporting output privacy. One such solution is the early evaluation of incoming model inference requests. This can involve implementing rate limits, and to effectively scan and identify any unexpected behavior. This allows for the interception of prediction requests that exhibit suspicious patterns. To ensure a secure model-serving environment, the platform should incorporate authentication, authorization, and proxy/token-based access to inference. Furthermore, implementing additional security measures within the CI/CD pipeline can help prevent any unexpected exposure to the model. Specifically for federated learning, one should make sure to use secure, industry strength communication protocols, and enforce identity management for clients. In a future post, we will elaborate further on system security for federated learning.

At [Scaleout](https://www.scaleoutsystems.com/), our primary focus is to provide a comprehensive software platform for production-grade federated learning, capable of catering to both cross-device and cross-silo use-cases at scale. We are actively engaged in developing innovative strategies to address input and output privacy challenges within federated settings. For example, we are currently running a [cybersecurity project funded by the Swedish Innovation Agency](https://www.vinnova.se/en/p/trusted-execution-environments-for-federated-learning/), in which we look into leveraging trusted execution environments for confidential computing and remote attestation. On the application side, we have recently completed a European project, [AICHAIN](https://www.aichain-h2020.eu/): A platform for privacy-preserving Federated Machine Learning using Blockchain to enable Operational Improvements in ATM. In the [ITEA ASSIST](https://itea4.org/project/assist.html) project we are contributing to advancing state-of-the art in brain tumor segmentation. The know-how gained in these industrial research projects benefit all our partners and clients as we integrate results into our software FEDn and Studio.

\---

_1\. T. Wang, Y. Zhang, and R. Jia, “Improving robustness to model inversion attacks via mutual information regularization,” Proceedings of the AAAI Conference on Artificial Intelligence, vol. 35, no. 13, pp. 11 666–11 673, May 2021._

_2\. V. Shejwalkar, A. Houmansadr, P. Kairouz, and D. Ramage, “Back to the drawing board: A critical evaluation of poisoning attacks on production federated learning,” in 2022 IEEE Symposium on Security and Privacy (SP), 2022, pp. 1354–1371._

_3\. F. Tramer, F. Zhang, A. Juels, Michael K. Reiter, T. Ristenpart. “Stealing Machine Learning Models via Prediction {APIs}”. 25th USENIX Security Symposium (USENIX Security 16). 2016. 978-1-931971-32-4_

_4\. Oh, Seong Joon, Bernt Schiele, and Mario Fritz. "Towards reverse-engineering black-box neural networks." Explainable AI: Interpreting, Explaining and Visualizing Deep Learning (2019): 121-144._

_5\. Usynin, Dmitrii, Daniel Rueckert, and Georgios Kaissis. "Beyond gradients: Exploiting adversarial priors in model inversion attacks." arXiv preprint arXiv:2203.00481 (2022)._

_6\. K. Wei et al., "Federated Learning With Differential Privacy: Algorithms and Performance Analysis," in IEEE Transactions on Information Forensics and Security, vol. 15, pp. 3454-3469, 2020, doi: 10.1109/TIFS.2020.2988575._

_7\. Stacey Truex, Nathalie Baracaldo, Ali Anwar, Thomas Steinke, Heiko Ludwig, Rui Zhang, and Yi Zhou. 2019. A Hybrid Approach to Privacy-Preserving Federated Learning. In Proceedings of the 12th ACM Workshop on Artificial Intelligence and Security (AISec'19). Association for Computing Machinery, New York, NY, USA, 1–11._ [_https://doi.org/10.1145/3338501.3357370_](https://doi.org/10.1145/3338501.3357370)

_8\. Bhagoji, Arjun Nitin, et al. "Analyzing federated learning through an adversarial lens." International Conference on Machine Learning. PMLR, 2019._

_9\. Jere, Malhar S., Tyler Farnan, and Farinaz Koushanfar. "A taxonomy of attacks on federated learning." IEEE Security & Privacy 19.2 (2020): 20-28._

_10\. A. Hatamizadeh et al., "Do Gradient Inversion Attacks Make Federated Learning Unsafe?," in IEEE Transactions on Medical Imaging (2023), doi: 10.1109/TMI.2023.3239391._
