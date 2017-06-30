---
layout: post
title: Decision Table Discretization
tags:
  - samples
  - discretization
published: true
---

{% highlight csharp linenos %}
var data = Data.Benchmark.Factory.Vehicle();

DecisionTable train, test;
var splitter = new DataSplitterRatio(data, 0.8);
splitter.Split(out train, out test);

var tableDiscretizer = new TableDiscretizer(
	new IDiscretizer[]
	{
		//try to discretize using Fayyad MDL Criterion
		new DiscretizeFayyad(),

		//in case Fayyad MDL is to strict
		//use standard entropy and 5 buckets
		new DiscretizeEntropy(5)
});

tableDiscretizer.FieldsToDiscretize = train
	.SelectAttributeIds(a => a.IsStandard && a.CanDiscretize());

var filter = new DiscretizeFilter();
filter.TableDiscretizer = tableDiscretizer;
filter.Compute(train);

foreach(int attributeId in tableDiscretizer.FieldsToDiscretize)
{
	var fieldDiscretizer = filter
		.GetAttributeDiscretizer(attributeId);

	Console.WriteLine("Attribute {0} was discretized with {1}",
		attributeId, fieldDiscretizer.GetType().Name);
	Console.WriteLine("Computed Cuts: {0}",
		fieldDiscretizer.Cuts.ToStr());
}

var trainDisc = filter.Apply(train);
var testDisc = filter.Apply(test);
{% endhighlight %}
