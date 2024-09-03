# Our approach to scalable federated learning

> https://www.scaleoutsystems.com/post/our-approach-to-scalable-federated-learning

Scalable federated learning
---------------------------

Our software is user-friendly and agnostic to the ML-framework and language used by clients. It supports high-performance federated training in real distributed settings, making it an ideal tool for developers implementing a wide range of federated learning schemes with ease. Its tiered architecture ensures that it is both robust and scalable, making it well-suited for both research and production use cases.

Overall, Scaleout Studio is a powerful and flexible framework that satisfies the requirements of real-world federated learning use cases.

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/65bbe79e71dd10fb79efa71e_642154f1ce09cd5402659ce1_sol.jpeg)

Our approach is built around a tiered architecture that consists of three logical tiers inspired by the MapReduce programming model. This enables users to implement a variety of federated learning schemes using a familiar and structured design pattern. The framework guarantees that these schemes are highly resilient and scalable.

### Local data and local models

The first tier is made up of geographically distributed clients. Each client is responsible for local model updates and are the only ones that touch the private datasets. This is one of the greatest benefits of federated learning. No data leaves the client site.

At the start of a federated model, a compute package with files and instructions is prepared by the initiating part and uploaded to the Controller. When a client sends a connection request to the discovery service, it receives this compute package from the controller and stages it in its local execution environment.

Clients can join a network by requesting a combiner assignment from the discovery service. The client then connects to its assigned combiner and receives training and validation requests, downloads the global model from the combiner, executes model updates and validations, and streams the results back to the combiner.

### The federated learning network

The second tier is composed of one or many Combiners and a Reducer. The Combiner plays a crucial role in our system. As a stateless server, it coordinates client updates and aggregates model updates from a subset of clients. Importantly, each combiner works independently of all other combiners in the network during a model update round.

This architecture enhances the fault-tolerance of our network. If a combiner becomes unresponsive, it can be easily replaced, and only the model updates from its associated clients for that round are lost. This also ensures that our network is horizontally scalable, as there is no need for communication between combiners.

The work performed by the combiner scales linearly with the number of attached clients, and the total number of clients that our network can support is directly proportional to the number of combiners.

![Image 2](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/65bbe79e71dd10fb79efa71a_6421558f4cf41ff47e3c7c4a_sol2.jpeg)

The number of combiners needed in such a network depends on the number of clients and the size of models. Each combiner is responsible for producing a partial model update in a global round of federated training. The partial models are aggregations of model updates by the combiner’s associated clients.

The reducer is responsible for combining all partial model updates computed by combiners into a single global model update in each round. The reduce operation implements a separate service that pulls partial model updates from the combiners and incrementally aggregates these updates.

The work done by the reducer scales with the number of combiners in a protocol-dependent manner, and it is independent of the number of clients. In the current implementation, it scales linearly.

### The brain of the network

The third tier is made up of two key components - the Controller and the Discovery service. The controller is responsible for coordinating the overall computation done in a global training round, for maintaining a trail of global models (model checkpointing), and to handle global state.

The Discovery service component is responsible for receiving client connection requests and assigning clients to combiners in the network.

During each global round of training, the following sequence of events take place in a typical setup:  
1\. Ask all combiners if they can participate in the round using the update.  
2\. Start a countdown clock and ask all participating combiners to coordinate a partial model update.  
3\. Wait until all combiners report a completed update, or the round times out.  
4\. Check if the round should be considered valid by evaluating the update.  
5\. If the round passes the validation test, ask the reducer to aggregate all combiner partial models into the global model.  
6\. Ask combiners to coordinate model validations.  
7\. Add the global model to the checkpoint.

Global rounds are repeated as needed for model convergence. In this process several steps can be configured to adjust the detailed behavior in a round. For example, the round validity policy can be used to enforce certain requirements on how many combiners need to be successful in a round for the global model to be updated.

Whether a use case involves just a few clients in a data collaboration federation, or thousands of on-device clients, Scaleout federated learning makes it easy to start a secure federated learning network and enable machine learning models to be trained on decentralized data sources, without having to transfer the data to a central location.

If you want to learn more about the scalability features of our software a first step could be to have a look at our research paper [Scalable Federated Machine Learning](https://arxiv.org/pdf/2103.00148.pdf). Or you can just contact us directly to discuss your use case.

‍
