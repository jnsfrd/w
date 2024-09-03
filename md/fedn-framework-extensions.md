# FEDn Framework Extensions

In Federated Learning, model updates computed by clients on private, local data are combined by a server into a global model. Since the computation is inherently distributed, there is a need to move the model updates to the server. The format of the model is dictated by the specific ML framework used, e.g. PyTorch. Moving an object (in this case a model update) from its memory representation at the client to its memory representation at the server is a process that is referred to as _marshaling_.

Once model updates from multiple clients are available on the server, they need to be aggregated. In federated learning, there are many possible aggregations schemes. The most commonly used is called Federated Averaging (FedAvg), where a weighted average of the model parameters is computed. More advanced alternatives include Federated Adam (FedAdam), where a pseudo-gradient is computed and the global model is updated using so-called server side momentum. Many different variants of global aggregation schemes can be derived by combining a given local optimization strategy (at clients) with a server side strategy. A good introduction to this topic is provided in the paper [https://arxiv.org/abs/2003.00295](https://arxiv.org/abs/2003.00295) by Reddi et al.

It is relatively straightforward to write a simulation code for federated learning, however for a production grade system, care should be taken to make sure that both marshaling, model transfers (for example via streaming) and aggregation is scalable, robust and secure. At the same time, there is a need to allow data scientists a user-friendly way to extend the framework with custom aggregators and support for new ML frameworks. In FEDn we enable this with a plugin architecture with clearly defined framework extension points.

In what follows, we elaborate on the computational model for aggregation, and the steps taken by a developer to extend and configure the framework.

Aggregators 
------------

Aggregators handle combinations of model updates received by the combiner into a combiner-level global model. During a training session, the combiners will instantiate an Aggregator and use it to process the incoming model updates from clients.

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/65bbe79db0c5c6166a7b1fe5_65b8b12acd69edac4defd8af_unnamed%2520(1).png)

The above figure illustrates the overall flow. When a client completes a model update, the model parameters are streamed to the combiner, and a model update message is sent. The model parameters are written to file on disk, and the model update message is passed to a callback function, on\_model\_update. The callback function validates the model update, and if successful, puts the update message on an aggregation queue. The model parameters are written to disk at a configurable storage location at the combiner. This is done to avoid exhausting RAM memory at the combiner. As multiple clients send updates, the aggregation queue builds up, and when a certain criteria is met, another method, combine\_models, starts processing the queue, aggregating models according to the specifics of the scheme (FedAvg, FedAdam, etc).

The user can configure several parameters that guide general behavior of the aggregation flow:

*   Round timeout: The maximal time the combiner waits before processing the update queue.
*   Buffer size: The maximal allowed length of the queue before processing it.
*   Whether to retain or delete model update files after they have been processed (default is to delete them)

A developer can extend FEDn with his/her own Aggregator(s) by implementing the interface specified in fedn.network.combiners.aggregators.aggregatorbase.AggregatorBase. The developer implements two following methods:

*   on\_model\_update (optional)
*   combine\_models

#### on\_model\_update

The on\_model\_callback has access to the complete model update including the metadata passed on  by the clients (as specified in the training entrypoint, see compute package).  The base class implements a default callback that checks that all metadata assumed by the aggregation algorithms FedAvg and FedAdam is present in the metadata. However, the callback could also be used to implement custom preprocessing and additional checks including strategies to filter out updates that are suspected to be corrupted or malicious.

#### combine\_models

This method is responsible for processing the model update queue and in doing so produce an aggregated model. This is the main extension point where the numerical detail of the aggregation scheme is implemented. The best way to understand how to implement this methods is to study the already implemented algorithms:

*   fedn.network.combiners.aggregators.fedavg
*   fedn.network.combiners.aggregators.fedopt

To add an aggregator plugin “myaggregator”, the developer implements the interface and places a file called ‘myaggregator.py’ in the folder ‘fedn.network.combiner.aggregators’.

Model marshaling - Helpers
--------------------------

In federated learning, model updates need to be serialized and deserialized in order to be transferred between clients and server / combiner. There is also a need to write and load models to/from disk, for example to transiently store updates during training rounds. Furthermore, aggregation algorithms need to perform a range of numerical operations on the model updates (addition, multiplication, etc). Since different ML frameworks (TF, Torch, etc) have different internal ways to represent model parameters, there is a need to inform the framework how to handle models of a given type. In FEDn, this compatibility layer is the task of Helpers.

A helper is defined by the interface in fedn.utils.helpers.HelperBase. By implementing a helper plugin, a developer can extend the framework with support for new ML frameworks and numerical operations.

FEDn ships with a default helper implementation, numpyhelper. This helper relies on the assumption that the model update is made up of parameters represented by a list of numpy.ndarray arrays. Since most ML frameworks have good numpy support it should in most cases be sufficient to use this helper. Both TF/Keras and PyTorch models can be readily serialized in this way. To add a helper plugin “myhelper” you implement the interface and place a file called ‘myhelper.py’ in the folder fedn.utils.helpers.plugins. See the Keras and PyTorch quickstart examples and fedn.utils.helpers.plugins.numpyhelper for further details.

‍
