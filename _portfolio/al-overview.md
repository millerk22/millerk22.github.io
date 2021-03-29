---
title: "Active Learning Overview"
excerpt: "I provide my thoughts and main takeaways for active learning."
collection: 
---


[Click Here](https://two-moons-app.herokuapp.com/) for my Active Learning Demo that visually presents the active learning process, highlighting my ["Model Change" criterion](https://arxiv.org/pdf/2007.11126.pdf).


Many machine learning algorithms require a plethora of labeled data in order to be accurate in practice. For example, most deep learning neural network architectures requires many input and output pairings in order to properly adjust model parameters to accurately reflect the training data and hopefully generalize to testing data. However, labeled data in the real world is often difficult to acquire; e.g. properly labeling (i.e. classifying) images of lung Xrays requires expert supervision that is costly both in time and money. While it is relatively easy to obtain a large amount of inputs (e.g. lung Xrays), it is costly to accurately obtain a sufficient amount of outputs (labels) from human labeling efforts (e.g. doctor hand classifying the lung Xrays).

Active learning seeks to alleviate this problem by **selecting "important" inputs to use your limited resources for human labeling**. While there are different active learning paradigms, my research focuses on pool-based active learning, wherein our active learner has access to a "pool" (database) of inputs X from which it can select a subset Q which we will ask the human (oracle) to label. These labelings are fed into our underlying machine learning classifier, wherein we update the classifier (model retraining) and then select another subset of query points. The underlying machine learning classifiers that I work with are graph-based semi-supervised learning methods that leverage the geometric relationships between the labeled and unlabeled inputs via the use of a similarity graph in order to infer the labeling information on the unlabeled inputs. 

The main question for us in this active learning paradigm is **how to select that subset Q of query points?** In general, we want to select points that will be beneficial for our resulting underlying machine learning classifier. To make this a bit more precise, there are two properties of our active learning query choices that we would like to balance:
* **Exploration** : We want to properly "explore" the inherent geometric/clustering structure of our dataset's inputs. These points will be *representative* of our input data.
* **Exploitation** : We want to adequately "exploit" the classification structure that we have learned from our previously observed labeled data. These points will be *informative* for refining the decision boundary between our classes. 

Generally, active learning methods favor one or the other of these properties. I want to create methods that naturally combine and balance the desiderata of exploration and exploitation. 

As a toy example, consider the "Two Moons" dataset, shown below in Figure 1, wherein the two classes are the distinct moons. *Representative* points would reflect the underlying moon structure (think of a mesh of points that are somewhat evenly spaced over the input data). *Informative* points then would lie in the middle region, where points of the two moons lie close to eachother (think of points that are close to the easily observed decision boundary that would separate the top moon from the bottom moon). **A "good" active learner will generally balance these properties by first exploring the clustering structure and then exploiting the decision boundary information of its observed labeled data.** 

![tm-base.JPG]({{site.baseurl}}/images/tm-base.png){:style="margin: auto; display: block; width: 25%"}

See the following [demo for visualizing this active learning process](https://two-moons-app.herokuapp.com/), where I show how my ["Model Change"](https://arxiv.org/pdf/2007.11126.pdf) criterion progresses through the successive iterations of the active learning process. 

## What Can Go Wrong?

Let's see what happens when we don't properly balance these two properties. Consider another toy dataset, the Checkerboard dataset - 2,000 points sampled uniformly from the unit square and divided into a 4 x 4 grid, where the classification of the boxes follows a checkerboard pattern (see Figure 2).

![cb-base.JPG]({{site.baseurl}}/images/cb-base.png){:style="margin: auto; display: block; width: 25%"}

In order for an active learner to be successful on this dataset, it must **explore** each of the regions of the square to identify all of the boxes as well as **exploit** labeling information to refine the underlying model classifier's decision boundary to fit to the grid pattern. An active learner that only does one without the other leads to suboptimal results:
* If one only explores, the refinement of the underlying classifier's decision boundary to fit the grid like pattern will take a long time. Remember we want to improve the accuracy of our underlying classifier as __efficiently as possible, not wasting query choices__.
* If one only exploits, then we can incur sampling bias too soon by exploiting the underlying classifier's decision boundary that might be highly inaccurate. For example, we could all together miss one of the boxes in the grid if we do not properly explore the input data's structure. 

Below in Figure 3 are some results from our paper [Efficient Graph-Based Active Learning with Probit Likelihood Via Gaussian Approximations](https://arxiv.org/pdf/2007.11126.pdf) that demonstrate this. The yellow stars in the plots are the 200 active learning query choices of different methods. In plots (a) and (f), we see that the corresponding methods did not fully explore the input data's structure, and have suboptimal classification results. In plots (d) and (e), we observe that the corresponding methods chose an almost uniform mesh of points from the input data to query. These methods did well, but ultimately did not exploit the labeling information and hence led to suboptimal performance. Finally, in plots (b) and (c), we observe that our proposed methods balanced both exploration and exploitation to achieve the best performance -- there are query choices in all the boxes (exploration) as well as a certain concentration of choices along the boundaries between boxes of opposing classes (exploitation). 

![al-comp.png]({{site.baseurl}}/images/al-comp.png){:style="margin: auto; display: block; width: 80%"}
