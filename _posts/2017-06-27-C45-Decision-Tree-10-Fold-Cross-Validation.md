---
layout: post
title: 10-Fold Coss Validation of C4.5 Decision Tree
tags:
  - cross-validation
  - decision-trees
published: true
---
{% highlight csharp linenos %}
//load data
var data = DecisionTable.Load("data.txt", FileFormat.CSV);

//create 10-fold 25-repeated cross validation
var cv = new CrossValidation(data, 10, 25);

//create C4.5 decision tree and run cv evaluation
var c45 = new DecisionTreeC45();
var result = cv.Run<DecisionTreeC45>(c45);

//output result
Console.WriteLine("Train Error: {0}", result.Error);
{% endhighlight %}
