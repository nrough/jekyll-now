---
layout: post
title: Boosting \`\\\( (\omega,\varepsilon) \\\)\`-decision reduct based classifier
tags: 
- reducts
- samples
- ensembles
---

{% highlight csharp %}
//load training and testing DNA (spieces) data sets
var train = Data.Benchmark.Factory.Dna();
var test = Data.Benchmark.Factory.DnaTest();

//set weights
var weightGen = new WeightGeneratorConstant(train, 1.0 / (double)train.NumberOfRecords);
train.SetWeights(weightGen.Weights);

//create parameters for reduct factory
var parm = new Args();
parm.SetParameter(ReductFactoryOptions.ReductType, ReductTypes.ApproximateDecisionReduct);
parm.SetParameter(ReductFactoryOptions.FMeasure, (FMeasure)FMeasures.MajorityWeighted);
parm.SetParameter(ReductFactoryOptions.Epsilon, 0.05);
parm.SetParameter(ReductFactoryOptions.NumberOfReducts, 100);
parm.SetParameter(ReductFactoryOptions.ReductComparer, ReductRuleNumberComparer.Default);
parm.SetParameter(ReductFactoryOptions.SelectTopReducts, 1);

//create weak classifier prototype
var prototype = new ReductDecisionRules();
prototype.ReductGeneratorArgs = parm;

//create ada boost ensemble
var adaBoost = new AdaBoost<ReductDecisionRules>(prototype);
adaBoost.Learn(train, train.SelectAttributeIds(a => a.IsStandard).ToArray());

//classify test data set
var result = Classifier.Default.Classify(adaBoost, test);

//print result header & result
Console.WriteLine(ClassificationResult.TableHeader());
Console.WriteLine(result);
{% endhighlight %}
