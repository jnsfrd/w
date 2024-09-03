# Email spam detection with FEDn and Hugging Face

Our new example project demonstrates how one can make use of the popular Hugging Face ‘Transformers’ library in FEDn. In this example, a pre-trained BERT-tiny model \[1\] from Hugging Face is fine-tuned to perform spam detection on the Enron spam email dataset \[2\]. Check out the example in FEDn at:

[https://github.com/scaleoutsystems/fedn/tree/master/examples/huggingface](https://github.com/scaleoutsystems/fedn/tree/master/examples/huggingface)

Email communication often contains personal and sensitive information, and privacy regulations may prevent the collection of data in a central storage for model training. Federated learning is a privacy preserving machine learning technique that enables the training of models on decentralized data sources. Fine-tuning large language models (LLMs) on various data sources enhances both accuracy and generalizability. In the example we provide, the Enron email spam dataset is split among two clients. The BERT-tiny model is fine-tuned on the client data using federated learning to predict whether an email is spam or not.

To run the example, follow the steps described at:

[https://github.com/scaleoutsystems/fedn/tree/master/examples/huggingface](https://github.com/scaleoutsystems/fedn/tree/master/examples/huggingface)

FEDn studio visualizes the training progress by plotting test loss and accuracy, as shown in the plot below.  After running the example for only a few rounds in FEDn studio, the BERT-tiny model, fine-tuned via federated learning, is able to detect spam emails on the test dataset with high accuracy (~99%). Run the example yourself or adapt it to your own use-case!

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/66475d9ff957330351a79244_S5ir34pXWTv_PcPrqzl3ZosO1dcZ77ml3bHsKQZbvCHE2tn3P7JOX3TSZIcWIpNKwLuBhjyw3J5KicyQEuIvMImlwfdaMlX1HnYVC3KU0xTEsxkBO9ptLG-mmyYreEGmhXrZgfU85g7piHodSNEksZI.png)

\[1\] I. Turc, M.-W. Chang, K. Lee, and K. Toutanova, "Well-Read Students Learn Better: On the Importance of Pre-training Compact Models." arXiv preprint arXiv:1908.08962v2, 2019.

\[2\] V. Metsis, I. Androutsopoulos and G. Paliouras, "Spam Filtering with Naive Bayes - Which Naive Bayes?". Proceedings of the 3rd Conference on Email and Anti-Spam (CEAS 2006), Mountain View, CA, USA, 2006.
