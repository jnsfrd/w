# Cross-device FL with FEDn Part 1

> https://www.scaleoutsystems.com/post/cross-device-fl-with-fedn-part-1

A local development environment for intermittent client connections
-------------------------------------------------------------------

In the realm of cross-device federated machine learning, it's common for clients to intermittently connect and disconnect from the federated learning network. This variability can stem from:

*   **Unreliable connections**: For instance, a vehicle might traverse areas with poor internet coverage, leading to sporadic losses in server connectivity
*   **Application/System requirements**: An example includes a mobile application initiating a process to engage in training rounds for 2 minutes whenever new data becomes available and there is adequate battery life.

In a series of posts featuring code, we will delve into cross-device federated learning using the FEDn framework. More details about the framework can be found here:  
‍[the FEDn framework](http://scaleoutsystems.com/framework).

Our discussion will span the entire spectrum from research to real-world deployment, addressing aspects like:

*   ‍**Software Architecture:** It's crucial to have an architecture that robustly manages intermittent client connections. This entails allowing for the dynamic inclusion and exclusion of clients to and from the aggregation server(s) during ongoing training sessions.**‍**
*   **Scalability:** The system must efficiently support a vast number of clients.**‍**
*   **Aggregation Algorithm Adjustments:** To accommodate significant client diversity, the aggregation algorithm might require adjustments. A critical challenge in this context is the "problem of stragglers" — clients attempting to update the model asynchronously, or "out of round".**‍**
*   **Transition to Real-World Scenarios:** Moving from research and development to practical applications necessitates mechanisms for client authentication and the operational management of federated learning in a production environment.

In this first example, we will set up a local development environment for experimenting with cross-device use-cases on single machines. We will run intermittent clients solving a classification problem using incremental learning. In future posts in the series we will show how to seamlessly add production-grade features and scale it in a production-like setting via FEDn Studio.

### **The scenario: Intermittent clients training a scikit-learn Multi Layer Perceptron**   

The figure below illustrates the scenario.  We will run clients that:

1.  Connect to the server at a random time t\_on
2.  Stay online for training for a fixed period of time (e.g 120 seconds).
3.  Disconnect at time t\_off. 

This completes one cycle in our setup, which we then repeat a configurable number of cycles.

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/66006cb55ec3e645d1c70cbc_async-clients.png)

These clients will collectively solve a classification problem. We assume that each client collects a fresh batch of training points during each cycle. During this time (a random time delay with max 10s) they remain offline from the FL network. They then connect for 120s making themselves available to participate in training on that local data batch. Naturally, you can modify the parameters to your liking. The global model trained as such (using FedAdam) is compared to the alternative that clients instead send their collected data batches to a server.

![Image 2](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/66006cc85ed775a33b08122c_cross-device-experiment.png)

As can be seen, under these intermittent client conditions, FEDn trains a federated model (red) that reaches the same level of performance as the centralized baseline trained on all data on the server (blue), with convergence close to a simulated case where one client (orange) or all clients (green) send their collected data to a central server.

Here we used FedAdam with a fixed learning rate 1e-2 as the server-side aggregator. It is possible that hyperparameter tuning, or adapting the learning rate, could improve convergence further. This was not the focus of this experiment though - the objective was to set up a local experiment environment where clients connect intermittently and validate the robustness of FEDn in this scenario. In future parts of this series we will build on this in different ways as we explore various aspects of cross-device FL.

The code as well as additional experiments and examples for how to use the FEDn Python API to analyze training results can be found in our GitHub repository:  
[https://github.com/scaleoutsystems/fedn](https://github.com/scaleoutsystems/fedn)

### **FEDn - from development to real-world deployments**

A core design goal of FEDn is to meet the requirements of research and development while ensuring a sustainable route to real-world distributed deployments. Specifically, our aim is to assist researchers and developers in transitioning from initial experimentation to secure distributed deployments. In these deployments, elements like token-based client authentication and the managed operations of the server-side in Federated Learning (FL) are facilitated by the framework. Our approach within the framework is to offer a versatile local sandbox, designed to operate efficiently on individual laptops and workstations. This local setup is not merely a simulation of FEDn; it is a genuine distributed setup functioning on localhost. Consequently, projects developed in this manner can be deployed without any code modifications to FEDn Studio—a production version of FEDn that runs on any Kubernetes cluster. Scaleout provides a public instance of Studio as a Software as a Service (SaaS) to streamline the development phase as much as possible.

‍
