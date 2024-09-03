# Product update January

The FEDn version 0.7.0 introduces significant enhancements including the integration of Python's logging framework for client-side logging, compatibility improvements for Windows users, and substantial refactoring of the gRPC server for better scalability and efficiency. This update brings a more robust and user-friendly experience to our users, particularly in managing client-server interactions and data handling.

See release details here: https://github.com/scaleoutsystems/fedn/releases/tag/v0.7.0

Learn more about FEDn here: https://github.com/scaleoutsystems/fedn

FEDn
----

This new release of FEDn includes major updates aimed at improving user experience and system efficiency. Key features of this release include the use of Python's logging framework for client-side activities, enabling more detailed and customizable logging. Default logging is set to INFO level, but can be adjusted to DEBUG for more in-depth analysis.

Windows Compatibility
---------------------

A significant improvement in this release is the Windows compatibility of the Dispatcher. This update resolves previous limitations by adapting the command execution process for Windows environments, enhancing the platform's versatility across different operating systems.

gRPC Server Refactor
--------------------

We have streamlined the gRPC server in the combiner to reduce redundancy and improve clarity. Logging has been enhanced for better understanding and management of server activities. The update also includes the replacement of the Report endpoint with ListActiveClients for a more efficient and accurate tracking of client status. Additionally, client status management has been revised for increased scalability, including bulk updates to the statestore database and the option to use ListActiveClients for real-time updates.

Refactor of Common Module
-------------------------

The 'common' module has undergone significant refactoring. It now offers clearer organization, with statestore implementation and interfaces to S3 storage moved to the network module. We have removed the 'tracer' and integrated its functionality into the 'statestore', alongside a thorough cleanup of the S3 interface and the addition of complete docstrings for enhanced understanding and usability.

Compute Package Registry
------------------------

A new feature in this release is the compute package registry, allowing users to set the active model and compute package. The compute package file name is generated before storage in S3, with the original file name retained in Mongo for user interface purposes.

Conclusion

These updates mark a step forward in the FEDn platform's development, offering our users a more efficient, flexible, and user-friendly experience. We are committed to continuous improvement and look forward to bringing more advancements in the future.

‍

‍
