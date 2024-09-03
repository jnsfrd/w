# Announcing Scaleout Studio

_Our goal at Scaleout is to contribute to secure and privacy-preserving AI in the industry. A current focus is to take federated machine learning (FL) to production. FL allows development of machine learning models over multiple geographically distributed data sources without moving data. This improves_ [_data-access and security_](https://www.scaleoutsystems.com/post/the-copy-problem-in-machine-learning) _for a range of use-cases including predictive maintenance in vehicle fleets, energy optimization for smart buildings, and i_[_mproved analysis of medical images_](https://www.scaleoutsystems.com/post/improving-the-clinical-workflow-with-federated-learning)_._ To this end we have built the [federated learning framework FEDn](https://www.scaleoutsystems.com/post/our-approach-to-scalable-federated-learning).

We're excited to announce Scaleout Studio -  a key addition to our software suite. With Studio we offer a secure and privacy-enhancing ML platform that is simple to install, whether on-site or on a private cloud. It aids in combining privacy-enhancing technologies like FEDn with top open source MLOps (Machine Learning Operations) tools such as Jupyter, MLFlow, Tensorflow Serving, and TorchServe. We envision Studio empowering teams with essential tools for successful machine learning projects, with a clear path to effective use of privacy-enhancing technologies.

MLOps + FL = Studio
-------------------

As MLOps and privacy-centric solutions accelerate in development, our design philosophy for Studio has been deliberate. We've steered clear from the task of re-inventing MLOps - a domain already well-provisioned with superb tools available for both standalone users as well as enterprises. However, there’s currently no platform which incorporates Federated Learning (FL) with existing open source MLOps toolchains. The Studio platform enables you and your team to adopt production-grade FL while integrating with already known ML tools, such as MLFlow, Jupyter, Tensorflow/Pytorch Serving etc. Our focus moving forward will be on expanding unique capabilities for increasing security and privacy.

Current features of Studio
--------------------------

*   Data scientists can collaborate while maintaining stringent security and detailed access control for all apps and model artifacts. Project-based multitenancy on top of leading open source MLOps tools and FEDn. Segment managed services for Jupyter, MLFlow and model serving into project workspaces.
*   Host models and publish model API endpoints into a “model intranet”. Control access to model endpoints with token-based authentication on a personal, project or public level. In security and privacy context, it is essential with access control for model endpoints to reduce risk for model inversion and membership inference attacks. Studio makes this task easy.
*   Managed federated learning with FEDn. Scale model training to private, decentralized data without the complexity of deploying a production-grade distributed system yourself.

Studio's architecture allows for tool integration via plugins. This flexibility accommodates the changing demands of a project and ensures swift expansion of the platform as more privacy enhancing technologies reach production readiness.

Studio is designed to be cloud-agnostic, ensuring it can be deployed on any Kubernetes cluster (version 1.24 or above). In contrast with many MLOps platforms, Studio abstains from using Custom Resource Definitions in Kubernetes, meaning we leave the Kubernetes API untouched.

Demonstration
-------------

Demo of Studio’s current capabilities by Scaleout Head of Engineering Fredrik Wrede.

Trialing Studio
---------------

If you think Studio could help you solve your machine learning challenges and want to explore it in more depth, you are most welcome to trial our public demo-deployment: [https://www.scaleoutsystems.com/platform](https://www.scaleoutsystems.com/platform).

If you are interested in a private demo / Q&A-session with our engineering team to learn more about on-premise and private cloud deployment alternatives, contact us at [contact@scaleoutsystems.com](mailto:ebba@scaleoutsystems.com).
