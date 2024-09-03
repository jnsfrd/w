# Understanding the Scaleout software suite

Federated Learning (FL) is emerging as an integral method for developing and training machine learning models. At Scaleout, we are at the forefront of this evolution, building a comprehensive software suite tailored to federated learning. Our platform consists of two significant components, FEDn and Studio, which together provide a complete solution for your federated learning projects. In this blog post, we take a closer look at the specifics of these components, their interplay, and how they work in unison to offer a robust and efficient platform for federated machine learning.

FEDn: The Federated Learning Backbone
-------------------------------------

The cornerstone of our software suite, FEDn, serves as a System Development Kit (SDK) tailored for scalable federated learning. It is used to implement the core server side logic (including model aggregation) and the client side integrations. It implements functionality to deploy and scale the server side in geographically distributed setups. Developers and ML engineers can use FEDn to build custom federated learning systems and bespoke deployments.

Core Features of FEDn
---------------------

One of the standout features of FEDn is its ability to deploy and scale the server-side in geographically distributed setups. This functionality allows developers and machine learning (ML) engineers to create custom federated learning systems and bespoke deployments, adapting to varying project needs and geographical considerations.

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/65bbe79f895538b8cc3964a0_64997bb8a1e0a719560e88b1_fednarchitecture.png)

**Scalable and Resilient**

FEDn exhibits exceptional scalability and resilience, thanks to its tiered architecture. Multiple aggregation servers, known as combiners, form a network to divide the workload, coordinating clients, and aggregating models. This architecture allows for high performance in various settings, [from thousands of clients in a cross-device environment to large model updates in a cross-silo scenario](https://arxiv.org/abs/2103.00148). Crucially, FEDn has built-in recovery capabilities for all critical components, enhancing system reliability.

**ML-Framework Agnostic**

With FEDn, model updates are treated as black-box computations, meaning it can support any ML model type or framework. This flexibility allows for out-of-the-box support for popular frameworks like Keras and PyTorch, making it a versatile tool for any machine learning project.

**Security**

A key security feature of FEDn is its client protection capabilities, negating the need for clients to expose any ingress ports, thus reducing potential security vulnerabilities.

**Event Tracking and Training Progress**

To ensure transparency and control over the learning process, FEDn logs events in the federation and does real-time tracking of training progress. A flexible API lets the user define validation strategies locally on clients. Data is logged as JSON to MongoDB, enabling users to create custom dashboards and visualizations easily.

**User Interface**

FEDn offers a Flask-based User Interface (UI) that allows users to monitor client model validations in real time. It also facilitates tracking client training time distributions and key performance metrics for clients and combiners, providing a comprehensive view of the system’s operation and performance.

Studio: Enhancing FL with Cloud-Native Features
-----------------------------------------------

Complementing FEDn is Studio, our machine learning platform with inherent support for federated learning. Studio is developed following  cloud native principles  and deploys seamlessly to any standard Kubernetes cluster. Studio bundles commonly used open-source MLOps tools such as MLFlow, Jupyter, Tensorflow Serving and TorchServe.

![Image 2](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/65bbe79f895538b8cc39648a_64997de006d8fb0bc6c3f586_unnameds.png)

Studio's key feature is its ability to provide a managed, production-grade deployment of the server-side of the federated learning system on top of Kubernetes. Studio handles resource management, and introduces project-based multi-tenancy, an essential feature for segregating data and processes across different project environments.

**Security and Monitoring with Studio**

Studio enhances system security and provides advanced monitoring capabilities. Building on FEDn's base, Studio implements additional authorization and authentication mechanisms to ensure control of which  entities can access the system.

Furthermore, Studio includes monitoring tools and components for building customized dashboards. These dashboards offer tailored views of the federated learning system, providing key insights and tracking performance metrics in real-time.

**Flexible Deployment**

Our software suite offers flexibility in both deployment and licensing. With Studio's compatibility with any Kubernetes-supported infrastructure, deployment can take place on a user's own infrastructure, a private or public cloud, or even as a Software as a Service (SaaS) as operated by Scaleout as a trial deployment.

Licensing
---------

In terms of licensing, FEDn adheres to an open source license (Apache2). This approach empowers users to implement federated learning without any lock-in effects. For those seeking additional support, FEDn is also offered under a Scaleout Commercial License, which includes standard terms for enterprise support, warranty, and indemnification.

Studio, on the other hand, is licensed under the Scaleout Commercial License, bringing enterprise-level security features, operations tooling, integrations with MLOps toolchains, and extended user interface/user experience (UI/UX) enhancements to the table.

With Scaleout's software suite, users can leverage either FEDn or Studio, or both, according to their project needs. From implementing the foundational federated learning logic with FEDn to building an all-encompassing, secure, and scalable ML system with Studio, users have full control over their machine learning projects. Scaleout is committed to delivering a software suite that meets your unique needs, providing flexibility, scalability, and reliability for your federated machine learning projects.
