---
layout: post
title: "Feature Maps"
date: 2026-07-23
categories: [nlp, quantum-ml]
---


<!-- 
Feature Maps is in learning representations( what representation is useful.)
Feature Representations
Feature Transformations(for dim exp and dim red)
Representation Learning and Feature Maps

For a tutorial on classical and quantum machine learning, I'd recommend:
## Feature Maps and Feature Transformations 
-->


<!-- 
Definition
Feature expansion (lifting)
Dimensionality reduction (feature extraction)
Classical vs. quantum feature maps
limits related to representation ??? 
relationship between representation and feature map 
-->


# feature transformation 
when we need it ?
does it always happens by a function(called feature map) or there are other way to transform features ?  
do we choose the transformation fuction or the result first orit s all drived by the input(problem) ?? 
Feature map = the function ϕ.
Representation = the result z=ϕ(x).
The question is representation vs  similarities(is thereothers?? ) or it s transformation vs similarities and who define the question about what against what or what are we in front of is it the precison boundries or the computation boundries or capacity boundries ???  


# Feature Maps 
- **Feature map** \(\phi\): the function that performs the embedding.
  - **Classical:** typically either **deterministic** or **learned**.
  - **Quantum:** implemented as a state-preparation circuit,
    \[
    x \mapsto |\phi(x)\rangle.
    \]




## 2 types of feature transformation: 
  there are 2 types of feature transformations 
### Feature Expansion (Lifting) 
the kernel trick here 

### Dimensionality Reduction (Feature Extraction) 


2 different ways to impliment those transformations
### Classical Feature Maps



### Quantum Feature Maps






<!---

we can organize by: 
learning objectives if we want to answer the question :"What are we trying to learn?" 


Machine Learning
│
├── Learn Parameters
│     ├── Linear models
│     ├── Neural networks
│     └── SVM
│
├── Learn Representations
│     ├── Feature maps
│     ├── Embeddings
│     ├── Latent spaces
│     └── Autoencoders
│
├── Learn Similarity
│     ├── Kernels
│     └── Metric learning
│
├── Learn Data Distributions
│     ├── Gaussian models
│     ├── VAEs
│     └── GANs
│
└── Learn Policies
      └── Reinforcement Learning 

# Learrning Objectives: 
What are we trying to learn?
Learning can refer to many defferent objectives:
learning parameters (θ),
learning representations (z),
learning feature maps (ϕ),
learning latent variables,
learning kernels,
learning metrics,
learning structures,
learning policies, and more.

or 
Mechanism of learnig if it s about optimization("How is learning performed?")
or 
representation space(going from Kernel methods to QML)
answers: "what representation the model uses"

Input Space
        │
        ▼
Feature Space
        │
        ├── Classical feature maps
        ├── Kernel feature maps
        └── Quantum feature maps
        │
        ▼
Latent Space
        │
        ├── PCA
        ├── Autoencoders
        └── Embeddings 

# Mechanism of Learnings :
optimizaton is a mechanism of learning (mathematical rule is chain rule) 

or by learning paradigms:
This answers "where the supervision comes from", not what is learned.
Machine Learning
│
├── Supervised
├── Unsupervised
├── Self-supervised
├── Semi-supervised
└── Reinforcement
--->




