# Improving the clinical workflow with federated learning

> https://www.scaleoutsystems.com/post/improving-the-clinical-workflow-with-federated-learning

Scaleout has teamed up with partners such as Philips Medical Systems, Barco, Raysearch Laboratories, and several others to try to make things better.

In the ASSIST project we want to create new technology that will make it easier for doctors to do their job. This might mean automating some of the things doctors currently have to do manually. By doing this, doctors can focus more on the patient and less on the computer screen.

The project also hopes to improve the imaging systems used in therapy, so patients get better outcomes. And, of course, they want to make things less expensive, too.

### Saving time in the clinical workflow

Deep learning is changing a lot of areas of research. In healthcare, it can help doctors and nurses save a lot of time when they're working with patients. For instance, when planning radiotherapy treatment, doctors have to do a few things:

1.  identify the tumor they want to kill with radiation,
2.  identify other organs that need to be protected from radiation, and
3.  figure out the best way to treat the tumor without hurting the other organs.

Deep learning can help speed up each of these steps by cutting down the time needed by up to 60 minutes. And that is a huge improvement in the treatment process.

However, it takes a lot of data to train the AI models that use deep learning. These models can have millions of different settings that need to be programmed. When doctors need to share medical images with each other, it becomes difficult. There are rules and laws that protect people's privacy, like GDPR. It's also hard to get enough images of rare diseases.

One way to solve this problem is with federated learning.

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/65bbe70d5a4838ee2bf585a8_64253e9ddf84715da6af16b5_assist.jpeg)

Here's how it works: each hospital keeps their own data, and uses it to teach their own AI model. This model can learn to identify things in the images, like where a tumor is. Then, the hospitals send their models to a central place, called a "combiner," which puts all the updates together. The combiner uses the new, combined model to teach all the hospitals' models at once. That way, everyone can benefit from each other's work without breaking any privacy rules.

Learn more about the ASSIST project here: [https://itea4.org/project/assist.html](https://itea4.org/project/assist.html)
