# # The path to compliant machine learning

> https://www.scaleoutsystems.com/post/compliant-machine-learning

Food for Thought for Enterprise Data Privacy Officers
-----------------------------------------------------

In today's world, with over 150 countries having their own data protection laws, global data governance has become a significant challenge for enterprises. Moreover, the stakes are getting higher, with recent GDPR fines demonstrating that regulators' patience for non-compliance is dwindling.

In this article, we explore a recommended path towards responsible, compliant machine learning on regulated data, which involves applying data minimization principles and relevant privacy-enhancing technologies (PETs) to your ML systems.

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/65bbe58ab205f824baded193_64271512fe83fe7b76ebfa1e_fines.jpeg)

Cumulative GDPR fines (July 2018 - Dec 2022)

### Data Minimization and Federated Learning
Data minimization is a fundamental privacy principle found in privacy laws worldwide, including GDPR. It means that only relevant, adequate, and necessary personal data should be collected and kept for as little time as possible. Federated learning, an approach for training machine learning models in distributed settings, aligns well with GDPR's principles of data minimization. By avoiding unnecessary data duplication, federated learning enables the principle of data being limited to what is necessary and can help ensure that the data is used solely for its intended purpose.

Google is an early adopter of data minimization principles and federated learning, using it to train their ML model for next-word prediction in virtual keyboards for smartphones. In this example, Google adheres to data minimization principles by

1.  collecting only a minimal amount of personal data,
2.  training the ML model locally on the user's device using federated machine learning, and
3.  immediately deleting the data after model training.

By applying privacy-enhancing technologies to this service, which requires ML trained on personal data, Google can provide a high-value service to Android users while respecting their data privacy.

The recommended path towards responsible, compliant machine learning on regulated data involves applying data minimization principles and relevant PETs, such as federated learning. Enterprises embracing this approach can enable valuable tools and services based on data insights and machine learning on privacy-regulated data while respecting data privacy laws.
