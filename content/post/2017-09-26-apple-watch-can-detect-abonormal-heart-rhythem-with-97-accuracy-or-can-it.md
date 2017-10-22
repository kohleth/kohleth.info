---
title: Apple watch can detect abnormal heart rhythm with 97% accuracy ... or can it?
author: Kohleth Chia
date: '2017-09-26'
categories:
  - machine learning
slug: apple-watch-can-detect-abnormal-heart-rhythm-with-97-accuracy-or-can-it
---

Earlier this year I came across <a href="//www.macrumors.com/2017/05/11/apple-watch-abnormal-heart-rhythm-detection/" target="_blank" rel="noopener noreferrer">this article</a> which reports a company has developed a deep learning model to predict Abnormal Heart Rhythm (precisely, atrial fibrillation) with 97% Accuracy.

So, I traced the source and got to <a href="//blog.cardiogr.am/applying-artificial-intelligence-in-medicine-our-early-results-78bfe7605d32" target="_blank" rel="noopener noreferrer">this blog</a>. And then I saw that the 97% is actually an AUC of 0.97, not accuracy in the commonly understood sense.

But the real interesting thing is, the blog provides all the data to do the standard Bayes theorem calculation.

From the blog post, I figured:
<ul>
	<li>Pr(atrial fibrillation) = 3.2% (= 200 / 6158, making some assumptions here.)</li>
	<li>Sensitivity = 98.04%</li>
	<li>Specificity = 90.2%</li>
</ul>
Using the standard Bayes formula, we get Pr(atrial fibrillation |the App says so) to be about 25%.

This 25% gives a very different sense of 'accuracy' to what 97% would have indicated. (although both are not accuracy measure).

But then, if we take a step back, I think all these metrics that are based on predicted class is less useful in the medical diagnoses setting, where the goal is to *assist* clinician in making decision, rather than actually making the decision. Therefore, instead of just a binary prediction (yes/no), it is more useful to return the predicted probability, and even better if the model can explain itself, for this way the clinicians will have more information to help them decide on the diagnosis.

So, as more and more medical researchers are applying machine learning to their work, it is worth keeping an eye out for these things.

(having said all these, I am still quite positive about the utility of wearable toy...I mean tech)
