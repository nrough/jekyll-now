---
layout: post
title: Compute Generalized Majority \( (m^{\varepsilon},\cap) \\\)-Reducts with Exceptions
tags: 
- reducts
- samples
- generalized-decision
- exceptions
---

{% highlight csharp %}
//load training and test data sets
var train = Data.Benchmark.Factory.Dna();
var test = Data.Benchmark.Factory.DnaTest();

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
parms.SetParameter(ReductFactoryOptions.UseExceptionRules, true);

//generate reducts with exceptions
var reductGenerator = ReductFactory.GetReductGenerator(parms);
var reducts = reductGenerator.GetReducts();

foreach (var reduct in reducts) {
	var r = reduct as ReductWithExceptions;
	foreach (var exception in r.Exceptions) {
		Console.WriteLine(exception.Attributes
			.ToArray().ToStr());
		Console.WriteLine(exception.SupportedObjects
			.ToArray().ToStr());
	}
}

var rules = new ReductDecisionRules(reducts);
rules.DecisionIdentificationMethod
	= RuleQualityMethods.Confidence;
rules.RuleVotingMethod = RuleQualityMethods.SingleVote;
rules.Learn(train, null);

//classify test data set
var result = Classifier.Default.Classify(rules, test);

//show results
Console.WriteLine(result);
{% endhighlight %}
