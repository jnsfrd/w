# Enhancing semiconductor component placement with federated learning

> https://www.scaleoutsystems.com/post/enhancing-semiconductor-component-placement-with-federated-learning

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/66cd75cd4ca6e352a6598d48_66cd759fad7a660cf5774be0_pnp%2520(1).webp)

Precision is crucial in the field of semiconductor manufacturing. Mycronic, a leader in this domain, is exploring cutting-edge approaches to enhance their Pick and Place (PnP) machines, which are responsible for placing semiconductor components on circuit boards. Access to a plentiful AI model-training data set is essential for the ML algorithm supporting the precision of the PnP machines to perform well. However, the data sets needed for AI-model training are sensitive and to a large extent owned by Myronic’s customers.

![Image 2](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/66c450ca28e54c3ecbaee666_66c45030322bfa1968082c9a_Screenshot%25202024-08-20%2520at%252010.12.51.jpeg)

In order to respect the integrity of the customer-owned-data, the option of training a model across all datasets is not viable using standard ML model training which relies on centralizing all data to one location, outside the control of local data owners. By addressing privacy and security concerns using federated Machine learning, Mycronic is looking to increase the available training data and strengthen their Pick and Place machines further.

A recent master’s thesis study by Juan Albahaca has shed light on how federated learning can solve for secure and privacy preserving ML and thereby access to essential, value creating data sets for model training. Learn more about the project and findings in the following interview, featuring the thesis author Juan Albahaca.

**Interviewer**: What was the objective of this thesis work?  
**Juan**: The objective of this thesis was to develop and evaluate federated machine learning architectures for binary image classification of semiconductor components, particularly focusing on enhancing the model training on Pick and Place (PnP) machines without the need for centralizing sensitive data.  
**Interviewer**: Please tell us a bit more about the problem you are solving?  
**Juan**: The problem addressed in this thesis is the challenge of training machine learning models on sensitive data from different sources, specifically in industrial settings like Mycronic's Pick and Place machines. Centralizing this data poses privacy risks and can degrade model performance due to data heterogeneity.  
**Interviewer**: Interesting. In what way could federated learning solve this problem?  
**Juan**: Federated learning mitigates the need for centralizing data by allowing the model to be trained directly on devices where the data is generated. This approach preserves data privacy and improves the model's ability to generalize across diverse data sources, thus enhancing performance in real-world scenarios.  
**Interviewer**: Can you describe the specific approach you took?  
**Juan**: The approach involved developing a federated learning framework using the FEDn platform, evaluating different convolutional neural network (CNN) architectures, and conducting experiments to assess the performance of various federated learning algorithms under conditions of data heterogeneity.  
**Interviewer**: You mentioned you’ve been using the FEDn framework for federated learning experimentation? What was your experience?  
**Juan**: My experience with the FEDn framework was positive. It proved to be robust and effective in managing the federated learning process across multiple clients. The framework facilitated smooth experimentation and provided essential tools for evaluating federated learning algorithms in a controlled environment.  
**Interviewer**: What are the most important conclusions from your thesis work and what are the next steps?  
**Juan:** The key conclusions from the thesis are that federated learning is a viable solution for privacy-preserving model training in industrial applications, and that the performance of federated learning can be significantly affected by data heterogeneity. The next steps involve further refining the algorithms to better handle diverse data distributions and scaling the framework to accommodate larger and more complex datasets.

‍  
We would like to extend our sincere thanks to Juan Albahaca for his valuable contributions and insights through his master’s thesis. His work has provided us with a deeper understanding of how federated learning can be applied to advance our technology. We look forward to seeing how his research will continue to influence the field.

![Image 3](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/66c4927f2c0e6d48efe47a0c_66c4926a092597c28615cd6d_juan%2520(1).jpeg)

Juan Albahaca

Thesis: https://www.diva-portal.org/smash/get/diva2:1864216/FULLTEXT01.pdf

‍
