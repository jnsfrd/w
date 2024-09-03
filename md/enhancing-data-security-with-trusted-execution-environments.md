# Enhancing data security with trusted execution environments

TEE Project Summary
-------------------

Introduction
------------

The confidential computing consortium defines a Trusted Execution Environment (TEE) as a hardware-based context which provides a certain level of assurance for code and data confidentiality and integrity ([https://confidentialcomputing.io/whitepaper-01-latest](https://confidentialcomputing.io/whitepaper-01-latest)). In other words, such an environment provides protection against third parties access and corruption of data as well as protection against code corruption. These properties are somewhat similar to alternative approaches such as homomorphic encryption [(Acar et al., 2019)](https://paperpile.com/c/Jt1i3W/gpPd) and secure element [(Kanstren et al., 2015)](https://paperpile.com/c/Jt1i3W/FQea). In homomorphic encryption data is encrypted in a way that makes certain operations possible as if it were in plain format. This should provide data confidentiality and somewhat integrity and programmability, however code integrity and confidentiality is left uncovered. In secure elements, such as Trusted Platform Module, a device is used to securely operate and store keys. Integrity and confidentiality of encryption keys is preserved and integrity of signed code is also ensured. Nevertheless, secure elements primarily focus on protecting keys and lack programmability and generality of TEEs.

The leading TEE implementations are provided by Intel, AMD and ARM. Intel Software Guard Extension (SGX) [(Schunter, 2016)](https://paperpile.com/c/Jt1i3W/wL2V) enables confidentiality and integrity by running the application on a so-called “enclave”. Enclave’s code and data exists solely on processor reserved memory and cannot be accessed by any other peripheral or software. In this way if the host Operative System (OS) or hypervisor gets compromised confidentiality and integrity will still be safeguarded.

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/664c58c378d2168c952eae42__BO9a6-Z9Uq3CR66jgjrrZFb9IQ0O87rJNlFOpZ4u7WKf2W1ihJodsn8G80NQMKla42QYnqbyaQPO9vj7Qxo4CjDTETOjFA76-upJk2Lo5zhQ7DhN41ItINIP7mLZYaEFKLK9Joy5KQQfr8VsxiarIs.png)

Attacks from the OS or from the Hypervisor can only be directed up the stack and cannot reach the enclave.

AMD implements TEEs with a family of technologies named Secure Encrypted Virtualization (SEV), Encrypted State (ES) and Secure Nested Paging (SNP) [(Li et al., 2022)](https://paperpile.com/c/Jt1i3W/HNxu). The implementation is commonly referred to as AMD SEV. Here the TEE is a Virtual Machine (VM) whose memory and state are encrypted with integrity guarantees. Similarly to SGX, a malicious hypervisor (or OS) should not be able access or alter the state or memory of such a VM. Finally, ARM implements TEEs with a technology referred to as TrustZone [(Koutroumpouchos et al., 2021)](https://paperpile.com/c/Jt1i3W/9g7c). Here, protected code and data reside in a “secure world” that can securely communicate with a “normal world”. This distinction is achieved via hardware-level memory management which aims at ensuring that memory leakage from the secure world will not occur. ARM TrustZone and Intel SGX are somewhat similar. The protected application runs outside of the host OS and hypervisor memory space and therefore the surface of attack is drastically reduced. This however comes at the cost of having to develop applications that cannot make use of any host OS system call. The Gramine project [(Tsai et al., 2017)](https://paperpile.com/c/Jt1i3W/VWti), which at the time of writing this document solely supports Intel SGX, is an open source effort that aims at facilitating running enclaved applications by providing a lightweight LibOS (or unikernel) and a set of tools for facilitating secure loading, provisioning and attestation of enclaves. Since the Gramine LibOS implements most of the Linux system calls one should be able to run unmodified applications on Intel SGX with reduced effort. ARM TrustZone provides a reference document for implementing OS, or “trusted firmwares”, for its trusted world and OP-TEE [_(Yang & Lee, 2020)_](https://paperpile.com/c/Jt1i3W/i2Kr) emerged as the open source implementation that collected the largest community. In contrast to the Gramine LibOS, with OP-TEE, and other trusted firmwares, applications need to be recompiled and most likely rewritten in C. Another important difference is that while in Gramine trusted and untrusted environments can run on remote devices, in trusted firmwares the secure and normal world exist on the same device.

For the means of running FEDn in TEEs, Intel SGX and AMD SEV are the most practical solutions as they allow for running the existing Python-based environment on the TEEs. ARM TrustZone is less suited for cloud applications and targets edge use cases by assuming the secure and unsecure world to reside on the same device. While Internet of Things (IoT) use cases are important for Scaleout, it is important to realize that in most cases we can consider an edge device as a trusted environment as they will most likely reside at a customer facility and not on a cloud provider. With such an assumption, it is then possible to secure the weights with SEV or SGX-enabled reducer and combiner. This however does not allow for edge client attestation. Since Intel and AMD are solely focused on cloud use cases and ARM on edge devices some hybrid solution would be needed for making this happen hence we leave it for future exploration.

Use cases
---------

In this section we discuss a number of proofs of concept that we implemented.

Model serving
-------------

Serving Machine Learning (ML) models over gRPC or REST APIs has become a common process for many organizations. State of the art practices advocate the deployment of model serving on cloud environments which promise out-of-the-box resilience and scalability for these services. However, every time we move a mission-critical logic outside of an organization we take a risk. Large amount of data and computation is notably needed to train a successful ML model along with a substantial R&D effort, requiring a considerable investment. Therefore, if a third party gets access to served models with the intent of gaining an unfair advantage over our organization, a major loss may occur. ​​In this first proof of concept we use Gramine LibOS to run our serving API in the enclave and rely on remote secret provisioning to load and decrypt the model from the untrusted environment.

![Image 2](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/664c58c3a91cb7521485dcfc_fJhCkdISqrKpwGp-XZHzxNI3PzoQ2NMw5EH_DKn8Hw1vI0XOi-gGWknSgBVyxwZAZDMJsoiNcuBKEMy0cLcHqCsP76SzLaOrnt6PuzPzy41qhwDNrgMYyXs4pN272t0hD8ff3Wbn1jGxeD4H_rv2Hoo.png)

The plain model is encrypted in a trusted environment and shipped to the untrusted premises. An unmodified serving API runs in the enclave making use of Gramine LibOS. Gramine tools manage secret provisioning and decrypting the model when loading it in the enclave.

Code and documentation for this use case can be found on Scaleout’s GitHub organization: [https://github.com/scaleoutsystems/tee-serve](https://github.com/scaleoutsystems/tee-serve).

Confidential compute package
----------------------------

FEDn leverages “compute packages” to run code-agnostic training routines on its clients. In a FEDn federation an admin uploads a compute package to a central service, referred to as reducer, which then propagates to clients [(Ekmefjord et al., 2022)](https://paperpile.com/c/Jt1i3W/gt7w). If we consider the client hosts as an untrusted environment one can reach a good level of integrity and confidentiality by running the compute package inside an enclave. In this proof of concept, we mock a use case in which reducer and combiner run on a trusted environment (e.g. on prem resources) and where a client runs on remote untrusted resources. Data can be encrypted like in the previous example for making it inaccessible to the untrusted cloud provider.

![Image 3](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/664c58c3de3eb31aa70cf0fd_1EWw2dLvpXIWI7jYNv0W8by-H8Qb-LEhejNmM2578jiMMn6Az_6-W5FhPpdKEXPIV1mCrfY52VkFK07MvnKhaKMRQv_HxwktlJhTYPazY2vXYwnC5d8MK1x5cJsneE_8tETQcPdv0Y_P7h56DirYRNk.png)

Reducer and combiner run on a trusted environment, i.e. on prem. A client can be deployed on an untrusted environment, i.e. public cloud, to obtain extra resources. The compute package accesses data, which can also be encrypted, inside an enclave.

Code and documentation for this use case can be found on Scaleout’s GitHub organization: [https://github.com/scaleoutsystems/tee-mnist](https://github.com/scaleoutsystems/tee-mnist).

Confidential FEDn deployment
----------------------------

A more common use case is to have several organizations connecting their clients to a central FEDn deployment. From the organization's perspective such a deployment, constituted by a reducer and a combiner, may be considered an untrusted environment. By running reducer and combiner in an enclave we can make sure that the hosting provider will not be able to access the weights.

![Image 4](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/664c58c3174c4c14e9521004_-GAnRTONFPUt7PcOacY50TYcEACe-pqm8nokO-sT2yA15vE52Blz7dT-saLo-q6j9cq1seEnsCoWphe5xABRNmuBAg6SAvYLf-5_KvQydz7aJpQzuLUvgerCWsXdASNIlUVgJ7midm4wLvkhkdVGshA.png)

Clients run on a trusted environment, i.e. on prem. Reducer and combiner run on a central untrusted environment, such as public cloud resources. By running reducer and combiner inside an enclave weight updates cannot be seen by the hosting system.

Code and documentation for this use case can be found on Scaleout’s GitHub organization: [https://github.com/scaleoutsystems/tee-gc](https://github.com/scaleoutsystems/tee-gc). In addition an IoT proof of concept was implemented using a confidential FEDn deployment: [https://github.com/scaleoutsystems/tee-iot](https://github.com/scaleoutsystems/tee-iot).

Attestation
-----------

A crucial feature provided by TEEs is the ability to ensure code integrity. In FEDn’s context, this property can be used not only to ensure the integrity of reducer, combiners and clients but also to ensure the integrity of compute packages. In this proof of concept, we use the Intel attestation service, leveraging Gramine tools, to provision a client token only if the identity and integrity of the enclaved service is verified.

![Image 5](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/664c58c4dfe8b7808463e022_ng58peRc8tocWNn9Hr6WyfpD9IKSmQytCNaeGRbaWJJgQl3amXMSPT0keT0RWWlgKUAzieSlwKyRDDPVJP_nCgsmFT2ljcCZq_64SLLTeelS4Crm8NkNi1lVA5-ACbOhZibCZQr57iTi99cjBGwPs5g.png)

The reducer service runs on a trusted environment and it is responsible for generating client auth tokens. A provisioning service, implemented on top of Gramine toolchain, leverages the Intel attestation service to ensure the integrity of the enclaved client. The token is provisioned only if the integrity of the client is verified.

Code and documentation for this proof of concept is available on Scaleout’s GitHub: [https://github.com/scaleoutsystems/tee-attestation](https://github.com/scaleoutsystems/tee-attestation).

Performance tests
-----------------

We performed some preliminary benchmarking of federated learning in TEEs with FEDn using Azure Confidential Computing ([https://azure.microsoft.com/en-us/solutions/confidential-compute](https://azure.microsoft.com/en-us/solutions/confidential-compute/)). The benchmark consists in fitting the MNIST classification dataset [(_Supplemental Information 2: MNIST Dataset and Models for MNIST_, n.d.)](https://paperpile.com/c/Jt1i3W/PBMH) using a deep neural network implemented with the Pytorch C++ API. For details regarding architecture, hyperparameters and training loop please refer to the code: [https://github.com/scaleoutsystems/tee-mnist](https://github.com/scaleoutsystems/tee-mnist). As baseline, we ran the benchmark on a

Standard DC8 v2 (8 vcpus, 32 GiB memory) VM where reducer, combiner and client run natively. On the Standard DC8 v2 we also experimented with running only the compute package on a SGX enclave, running the reducer and combiner in the enclave with native compute package execution and by running the entire stack (reducer, combiner and compute package) in the enclave. Finally, we repeated the experiment running the entire stack with AMD SEV on a Standard DC8as v5 (8 vcpus, 32 GiB memory) VM. Note that when running on a single VM with SEV you need to run all of the services on the TEE.

![Image 6](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/664c58c39ad423c325538f6c_cQYm1_EhlqXE-5QR0kch68Ncf320BdZ41Gkq3OqYY8YyFjnT3fTVTZlAsVZITUU2HZHLcfmTQdTIfBaBju4UYV7Qz4f2nKdqSkN75SMssTgMl6jv5GrCmTgiIydTK4YkL1yQpcDfYkXWTpHFNZNlmlI.png)

The plot shows the wall time measured on the reducer for 30 training rounds. _SGX package_ is the benchmark where we only enclose the compute package in the enclave. _SGX reducer, combiner_ is the use case in which we enclosed the reducer and combiner in the enclave, but we run the compute package natively. _SGX reducer, combiner, package_ is the use case in which we run the whole stack in the enclave. Finally, _SEV reducer, combiner, package_ is the use case in which we run the whole stack in an SEV VM.

Running the compute package in the SGX enclave results in a ~5x performance loss. Surprisingly, running reducer and combiner in the enclave performs slightly better than the baseline. We believe that this is due to the fact that the aggregations performed by reducer and combiner are lightweight and can be performed within the SGX memory constraints. Hence, by running on a lightweight LibOS a slightly better performance can be achieved. This is also reflected when running the whole stack in the enclave where the performance loss is ~4x; lower than in the case in which we only run the compute package in the enclave. Finally, running on AMD SEV results in a ~2x performance loss.

Conclusion and future work
--------------------------

For applications that don’t exceed SGX memory footprint (~90MB) Intel seems to have an edge on performance; AMD SEV seems to perform better otherwise. However, we noticed that even with the help of the Gramine project, a lot of responsibility for setting up the environments correctly falls on the system administrators. This means that running the stack on TEEs becomes more complicated but one also has more control over it.

An interesting research perspective consists of finding efficient ways for running Deep Neural Networks (DNNs) on Intel SGX. DarkneTZ [(Mo et al., 2020)](https://paperpile.com/c/Jt1i3W/VCCC) is a research effort that explored this on ARM Trustzone and the idea behind it is to run only a few layers in the secure world. A similar approach could be tried on SGX along with other existing strategies to train DNNs on small memory footprint scenarios.

References
----------

[Acar, A., Aksu, H., Selcuk Uluagac, A., & Conti, M. (2019). A Survey on Homomorphic Encryption Schemes. In _ACM Computing Surveys_ (Vol. 51, Issue 4, pp. 1–35). https://doi.org/](http://paperpile.com/b/Jt1i3W/gpPd)[10.1145/3214303](http://dx.doi.org/10.1145/3214303)

[Ekmefjord, M., Ait-Mlouk, A., Alawadi, S., Akesson, M., Singh, P., Spjuth, O., Toor, S., & Hellander, A. (2022). Scalable federated machine learning with FEDn. In _2022 22nd IEEE International Symposium on Cluster, Cloud and Internet Computing (CCGrid)_. https://doi.org/](http://paperpile.com/b/Jt1i3W/gt7w)[10.1109/ccgrid54584.2022.00065](http://dx.doi.org/10.1109/ccgrid54584.2022.00065)

[Kanstren, T., Lehtonen, S., & Kukkohovi, H. (2015). Opportunities in Using a Secure Element to Increase Confidence in Cloud Security Monitoring. In _2015 IEEE 8th International Conference on Cloud Computing_. https://doi.org/](http://paperpile.com/b/Jt1i3W/FQea)[10.1109/cloud.2015.159](http://dx.doi.org/10.1109/cloud.2015.159)

[Koutroumpouchos, N., Ntantogian, C., & Xenakis, C. (2021). Building Trust for Smart Connected Devices: The Challenges and Pitfalls of TrustZone. _Sensors_ , _21_(2). https://doi.org/](http://paperpile.com/b/Jt1i3W/9g7c)[10.3390/s21020520](http://dx.doi.org/10.3390/s21020520)

[Li, M., Wilke, L., Wichelmann, J., Eisenbarth, T., Teodorescu, R., & Zhang, Y. (2022). A Systematic Look at Ciphertext Side Channels on AMD SEV-SNP. In _2022 IEEE Symposium on Security and Privacy (SP)_. https://doi.org/](http://paperpile.com/b/Jt1i3W/HNxu)[10.1109/sp46214.2022.9833768](http://dx.doi.org/10.1109/sp46214.2022.9833768)

[Mo, F., Shamsabadi, A. S., Katevas, K., Demetriou, S., Leontiadis, I., Cavallaro, A., & Haddadi, H. (2020). DarkneTZ. In _Proceedings of the 18th International Conference on Mobile Systems, Applications, and Services_. https://doi.org/](http://paperpile.com/b/Jt1i3W/VCCC)[10.1145/3386901.3388946](http://dx.doi.org/10.1145/3386901.3388946)

[Schunter, M. (2016). Intel Software Guard Extensions. In _Proceedings of the 2016 ACM Workshop on Software PROtection_. https://doi.org/](http://paperpile.com/b/Jt1i3W/wL2V)[10.1145/2995306.2995307](http://dx.doi.org/10.1145/2995306.2995307)

[_Supplemental Information 2: MNIST Dataset and Models for MNIST_. (n.d.). https://doi.org/](http://paperpile.com/b/Jt1i3W/PBMH)[10.7717/peerj-cs.436/supp-2](http://dx.doi.org/10.7717/peerj-cs.436/supp-2)

[Tsai, C.-C., Porter, D. E., & Vij, M. (2017). {Graphene-SGX}: A Practical Library {OS} for Unmodified Applications on {SGX}. _2017 USENIX Annual Technical Conference (USENIX ATC 17)_, 645–658.](http://paperpile.com/b/Jt1i3W/VWti)

[Yang, H., & Lee, M. (2020). Demystifying ARM TrustZone TEE Client API using OP-TEE. In _The 9th International Conference on Smart Media and Applications_. https://doi.org/](http://paperpile.com/b/Jt1i3W/i2Kr)[10.1145/3426020.3426113](http://dx.doi.org/10.1145/3426020.3426113)

‍
