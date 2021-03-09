---
title: "Transfer learning"
excerpt: "Training a student VGG-16 neural network with a teacher AlexNet"
collection: projects
---

[Code repository](https://github.com/EricHe98/Teacher-Student-Training/blob/master/presentation.pdf)

[Presentation](https://github.com/EricHe98/Teacher-Student-Training)

In the summer of 2017, I worked for [Gyrfalcon Technology](https://www.gyrfalcontech.ai/), one of the many companies designing chips optimized for neural network computations. We were a stealth-mode startup at the time, and the space was pretty hot, so the goal was to rush out a proof-of-concept chip into production, win the race for funding and try to get some of the other startups to drop out of the industry. 

One of the architectural decisions made for this first iteration was to restrict the chip to only one neural architecture (this chip went on to become our first [Lightspeeur](https://www.gyrfalcontech.ai/solutions/2801s/) product). Anyone with even a basic understanding of deep learning can recognize this to be quite a strong constraint; if someone, for example, was using a LeNet architecture and we only supported AlexNet models on our chip, there would be no way to directly port their model onto our hardware. 

Transfer learning was a new and relatively underexplored research topic at the time (back then, it went by many names - we called it teacher-student training, Hinton called it model distillation), and there was a lot of interest at the company in exploring its boundaries: if we could get a general neural architecture to learn another model's decision surface, then having only one possible architecture was completely okay. I was tasked with exploring the possibility of taking a trained model (say, from a client) and getting our selected model to try replicating that model's predictions. 

VGG-16, to date one of the largest (and most inefficient) CNN architectures ever, was our selected student model of choice; the idea was that since this architecture was so big, it should be able to learn anything. I was able to get teacher-student training examples working for small datasets and models, like LeNet and the CIFAR-10 CNN architectures presented in the CIFAR-10 dataset, but unfortunately could not find success on the actual VGG-16 architecture of interest. In my opinion it was because VGG-16 was too bulky to learn anything without training one layer at a time and crossing your fingers. You can see my presentation [here](https://github.com/EricHe98/Teacher-Student-Training/blob/master/presentation.pdf); I think despite its lack of decoration it is one of my better ones.

I worked on this project using the Caffe framework, as TensorFlow and PyTorch were still considered untested technologies by my seniors. As Caffe is deprecated today the code is similarly useless, but I still have the repository [here](https://github.com/EricHe98/Teacher-Student-Training).