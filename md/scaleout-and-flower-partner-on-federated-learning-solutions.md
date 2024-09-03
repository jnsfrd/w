# Scaleout and Flower partner on federated learning solutions

> https://www.scaleoutsystems.com/post/scaleout-and-flower-partner-on-federated-learning-solutions

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/662636c554a2466a4d69297b_Frame%20122%20(1).jpg)

Scaleout, the company behind the enterprise-grade FEDn federated learning platform, and Flower, the popular open-source federated learning framework, today announced a strategic collaboration. This partnership is to enable developers to seamlessly run Flower federated learning projects on FEDn, including FEDn Studio, Scaleout's managed federated learning service (SaaS or on-premise).

Federated learning unlocks the ability to train machine learning models across decentralized data sources while preserving data privacy. Flower has emerged as a pioneering framework, providing an easy-to-use platform and mature ecosystem for federated learning research and deployment. By collaborating with Scaleout, Flower users will gain access to FEDn's robust production capabilities for secure, scalable execution of their federated systems.

As a first step, in the recent FEDn 0.9.0 release, users can run many existing Flower ClientApps on FEDn clients. This is critical towards enabling full unmodified Flower projects to run on FEDn.

Compatible Flower ClientApps will benefit from:

*   Client Identity Management: FEDn Studio provides role-based access control (RBAC) for clients using token-based authentication (JWT).
*   Scalable Federated Training: Flower ClientApps that use one of the FEDn-supported aggregation schemes will benefit from FEDn's server-based architecture that efficiently coordinates large numbers of federated clients and handles asynchronous and intermittent client connections.
*   Monitoring and Management: Get real-time visibility into the federated training progress of your Flower ClientApp, with comprehensive logging, distributed tracing, and access to validation metrics through APIs and dashboards.

Key goals for the next stages in this collaboration include:

*   Run full Flower projects in FEDn: We want to enable developers to take their existing Flower project consisting of ServerApp and ClientApp and run it as-is in the FEDn Studio environment without any modifications required.
*   Privacy and security: Flower’s rich ecosystem of modular secure aggregation and differential privacy implementations will be able to run seamlessly on FEDn deployments.
*   Arbitrary Aggregation and Client Selection Protocols: FEDn users will benefit from fully customizable client selection and aggregation protocols, many of them provided as built-in strategies in Flower.

"We're excited to partner with Flower and provide their vibrant community with a seamless path to production deployments on FEDn," said Andreas Hellander, CEO. "Combining Flower's best-in-class developer experience and their rich library of models and examples with FEDn's enterprise-grade federated learning platform will unlock new opportunities across industries. By collaborating we can accelerate real-world adoption of privacy-enhancing technologies."

"Collaborating with Scaleout allows Flower users to take full advantage of the managed platform's robust security, scalability and monitoring capabilities," said Daniel J. Beutel, CEO. "Our hope is that this integration will be helpful to the Flower community as they advance federated learning methodology. Importantly, by introducing compatibility between FL frameworks we reduce fragmentation in the FL landscape, something that limits the rate of adoption and innovation.”

Developers can start using the integrated FEDn-Flower solution today by taking the tutorial: [Flower ClientApps in FEDn](https://github.com/scaleoutsystems/fedn/tree/master/examples/flower-client)

The FEDn 0.9.0 release enables partial compatibility for the client-side model definitions (Flower ClientApp). In future work the Flower and FEDn teams aspire to extend the compatibility layer to also support server-side features in the Flower ServerApp, a necessary next step to enable full Flower projects on FEDn.

More information on FEDn is available at: [https://www.scaleoutsystems.com/](https://www.scaleoutsystems.com/)

More information on Flower is available at: [https://flower.ai/](https://flower.ai/)
