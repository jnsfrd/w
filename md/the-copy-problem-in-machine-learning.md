# The copy-problem in machine learning

### The copy-problem in machine learning

In traditional machine learning, data is often collected in a central location and used to train a model. This process typically involves making multiple copies of the data, which are stored on various servers or cloud platforms. These copies are then used to train the model, and the resulting model may also be copied and shared with multiple parties. The _copy problem_ arises when sensitive data is inadvertently copied, stored, or transmitted to unauthorized parties, or when copies of the data are not properly secured. For example, if a copy of the data is transmitted over an insecure network, it could be intercepted by malicious actors. But even if we disregard deliberate, malicious actions, over time we might lose control of who has access to copies of data. All in all, it is good security practice to adhere to [_data minimization principles_](https://www.scaleoutsystems.com/post/compliant-machine-learning) and seek to avoid unnecessarily moving or disclosing data.

### How are we addressing the copy problem? 

The copy-problem is at the core of what we do at Scaleout:

1.  Data used for machine learning is often spread out at different locations in the Enterprise. Centralizing this data for machine learning introduces both security and compliance risks. For this reason our platform is built with federated machine learning (FL) as a first-class citizen. FL is a relatively new approach to machine learning and allows training and fine-tuning of models over decentralized data sources. In this way we avoid copying and moving data.

2.  We develop MLSecOps solutions that bring essential tools for hosting ML-models and AI-apps on-premise or in a (virtual) private cloud. This allows you to avoid unnecessarily moving training or inference-data to third-party cloud services outside of your organization. Our approach is to integrate and operate best-of-breed open source MLOps tools deployed to Kubernetes, adding a layer of security features and granular access control for model artifacts.      

‍

Taken together, this provides the necessary technology to “bring machine learning to data instead of data to machine learning”.

There are traditional approaches to protecting your input data while copying and centralizing it, most prominently data anonymization. This can require quite extensive work, and there is a risk that the utility of the data is reduced. Federated machine learning offers a pragmatic (and in our opinion quite elegant”!) alternative, by simply leaving the data where it is. As always,  there is no free lunch - there are technical challenges with building a production-grade federated learning system (we have written about that in [a previous post](https://www.scaleoutsystems.com/post/our-approach-to-scalable-federated-learning)). In the end, you can of course combine federated learning with more traditional approaches, stacking layers of security to improve data protection.

### Privacy enhancing technologies

Taking a broader perspective, Federated machine learning sits in the wider context of privacy-enhancing technologies (PETs). In addition to FL, privacy-preserving AI involves a range of approaches, including differential privacy, homomorphic encryption, and synthetic data generation. These are all complementary technologies, each offering to address different aspects of privacy and security in the end-to-end machine learning information flow. But while the copy-problem is relatively intuitive, other challenges in AI security require a bit deeper look at the concepts of input privacy and output privacy.

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/65bbc58cef3ef444eb53153e_644a2aa2b4998347e16dd2a1_unnamed.png)

### Input privacy vs output privacy

If an unauthorized party gains access to the input data during the process of computation, this is a breach of **_input privacy_**_._  In our context, this translates to an attacker gaining access to the training data (or its sensitive properties) during the forward computational process of creating the machine learning model. This also applies to the data prompted to the finished model to make predictions. Federated machine learning fundamentally deals with input privacy, excelling at the scenario where training data is spread over different devices or locations.

If an attacker is able to reverse-engineer private data from the output of a computation, this is a breach of **_output privacy_**_._ In our setting this translates to attempts at inferring training data (or properties of that data) from model output. This is a potential threat to any machine learning system since it in theory only requires knowledge about the model structure and access to either the model itself or an hosted API for making predictions. The most fundamental layer of protection against reverse-engineering attempts is thus to make sure to control access to model endpoints to as large extend as possible. However, advanced techniques such as differential privacy can add another layer of protection, at the expense of degradation of model performance.

In future posts, we will dig deeper into security challenges for machine learning systems, with a particular focus on the extension of secure MLOps to the decentralized setting which federated learning represents.
