---
layout: post
title: Generate \( (\omega,\varepsilon) \\\)-reducts over universe of weighted objects
tags: 
- reducts
- samples
- weights
---
{% highlight csharp %}
//load benchmark data
var data = Data.Benchmark.Factory.Zoo();

//set object weights using r(u) weighting scheme
data.SetWeights(new WeightGeneratorRelative(data).Weights);

//split data into training and testing sets
DecisionTable train, test;
var splitter = new DataSplitterRatio(data, 0.8);
splitter.Split(out train, out test);

//set parameters for reduct factory
var parm = new Args();
parm.SetParameter(ReductFactoryOptions.DecisionTable, train);
parm.SetParameter(ReductFactoryOptions.ReductType, ReductTypes.ApproximateDecisionReduct);
parm.SetParameter(ReductFactoryOptions.FMeasure, (FMeasure)FMeasures.MajorityWeighted);
parm.SetParameter(ReductFactoryOptions.Epsilon, 0.05);

//compute reducts
var reductGenerator = ReductFactory.GetReductGenerator(parm);
var reducts = reductGenerator.GetReducts();

//select 10 reducts with least number of attributes
var bestReduct = reducts.OrderBy(r => r.Attributes.Count).Take(10);

//create decision rules based on reducts
var decisionRules = new ReductDecisionRules(bestReducts);

//when test instance is not recognized
//set output as unclassified
decisionRules.DefaultOutput = null;

//classify test data
var result = Classifier.DefaultClassifer.Classify(decisionRules, test);

//output accuracy and coverage
Console.WriteLine("Accuracy: {0}", result.Accuracy);
{% endhighlight %}
