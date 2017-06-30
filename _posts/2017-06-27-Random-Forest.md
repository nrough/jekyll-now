---
layout: post
title: Random Forest Based on C4.5 Decision Trees
tags:
  - samples
  - ensembles
published: true
---
{% highlight csharp linenos %}
//load data from a CSV file
var data = DecisionTable.Load("german.data", FileFormat.CSV);

DecisionTable train, test;
var splitter = new DataSplitterRatio(data, 0.8);
splitter.Split(out train, out test);

//Initialize and Learn Random Forest
var forest = new DecisionForestRandom<DecisionTreeC45>();
forest.Size = 500;
forest.Learn(train, train.SelectAttributeIds(a => a.IsStandard).ToArray());

//Validate on test data set
var result = Classifier.Default.Classify(forest, test);

//Output the results
Console.WriteLine(result);
{% endhighlight %}
