# Product Update November

> https://www.scaleoutsystems.com/post/product-update-november

Scaleout Studio
---------------

In this latest update of the Scaleout Studio platform, we are excited to introduce several enhancements in Scaleout Studio. The update features a revamped user interface, making the setup and management of Federated Learning Networks more intuitive. Key new functionalities include simplified client addition process with direct configuration options, and enhanced training session management. Users can now easily monitor session progress and outcomes, with a clear view of the generated models. These improvements aim to provide a more efficient and user-friendly experience in managing Federated Learning projects.

FEDn
----

The latest product release introduces a series of enhancements and bug fixes. The main feature of this release is the new REST-API and the APIClient. Additionally, the documentation has been significantly updated to provide users with a more comprehensive understanding of the software.

#### New Features and Improvements

*   ‍**Flush Model Update Queues at New Session + Buffer Size Configuration (Feature/SK-505):** We've updated the global round logic to flush the combiner aggregation queues at the start of every new session, ensuring a clean state for each session. Additionally, the 'buffer size' is now configurable, offering greater flexibility in managing stragglers at time of aggregation.**‍**
*   **Refactor REST and Add APIClient (Feature/SK-523)**: This release introduces a significant refactor of our REST-API. We've implemented a separate service called 'api-server'. Additionally, an APIClient is now available. With this client, users can query the statestore and send requests to the controller, enhancing functionalities like uploading and downloading packages, listing clients, validations, and start sessions without using the dashboard.

#### Bugfixes

*   ‍**Fix Printout at Model Staging at Combiner (Bugfix/SK-548):** A minor fix has been made to enhance the print logs for model staging at the combiner, improving the clarity of information during model processing.

#### Additional Features

*   ‍**Statestore Initialization from API-Server Lacks Proper Setup (Feature/SK-545):** We addressed an issue where the statestore wasn't properly configured when running the api-server without the dashboard. The "default" argument for the MongoStatestore constructor has been removed, aligning with our practice of using JSON to start session endpoints rather than file-based control settings.
*   ‍**Add Pagination Option to REST-API (Feature/SK-553):**  We've extended the API server to accept 'limit' and 'skip' parameters, enabling efficient pagination. This feature makes it easier to handle large datasets and improves user experience by breaking down data into manageable portions.
*   ‍**Global Model Not Created if Combiner Terminates Based on Timeout (Feature/SK-521):** We've rewritten some of the polling in the controller to be more robust. This increases the system's resilience and eliminates the risk of race conditions, especially when the combiner times out. This ensures that the global model is consistently created even in timeout scenarios.**‍**
*   **Add Metadata to gRPC Calls for Python Clients (Feature/SK-574):** A new function has been introduced, allowing Python clients to add metadata (headers) to their gRPC stub calls. This addition enhances the functionality and security for Python clients interacting with our system via gRPC.

#### Documentation Improvements

We've made major enhancements to our [documentation](https://fedn.readthedocs.io/en/latest/), including updated guides, expanded API documentation, and enriched technical details. These improvements are designed to provide users with a clearer and more comprehensive understanding of the software.

Looking ahead, our upcoming releases promise exciting advancements that will further enhance the capabilities and reach of our software. A major development will be related to training of machine learning models that is set to significantly broaden the scope and flexibility of model training and deployment in the Federated Learning (FL) network. More updates about this later. Moreover, we're expanding our client support with the introduction of an Android client, developed in Kotlin, and a C++ client. Their inclusion will undoubtedly cater to a wider range of developers and applications, providing more robust and versatile solutions for different programming environments.

Furthermore, an important leap forward will be the capability to run all services natively on Windows. This enhancement is poised to significantly streamline operations for Windows users, offering a seamless and more integrated experience. With these forthcoming features, we continue to demonstrate our dedication to innovation, aiming to cater to the evolving needs of our diverse user base and to remain at the forefront of technological advancement in our field.

These upcoming features underscore our commitment to continuous innovation and our focus on meeting the diverse needs of our user community.
