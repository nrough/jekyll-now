---
layout: post
title: Compute Generalized Majority Decision Reducts
tags: 
- reducts
- samples
- generalized-decision
---

{% highlight csharp %}
//load training data set
var train = Data.Benchmark.Factory.Dna();

//setup reduct factory parameters
Args parms = new Args();
parms.SetParameter(ReductFactoryOptions.DecisionTable, train);
parms.SetParameter(ReductFactoryOptions.ReductType,
	ReductTypes.GeneralizedMajorityDecision);
parms.SetParameter(ReductFactoryOptions.WeightGenerator,
	new WeightGeneratorMajority(train));
parms.SetParameter(ReductFactoryOptions.Epsilon, 0.05);
parms.SetParameter(ReductFactoryOptions.PermutationCollection,
	new PermutationCollection(10,
	train.SelectAttributeIds(a => a.IsStandard)
		.ToArray()));

//generate reducts
var reductGenerator = ReductFactory.GetReductGenerator(parms);
var reducts = reductGenerator.GetReducts();
{% endhighlight %}
