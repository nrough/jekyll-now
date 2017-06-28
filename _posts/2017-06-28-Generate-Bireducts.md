---
layout: post
title:  Generate bireducts using class hierarchy
tags: reducts, bireducts, samples
---

{% highlight csharp %}
//load training data set
var train = Data.Benchmark.Factory.Dna();

//generate 100 permutations based on attributes and objects
var permGenerator = new PermutationGeneratorAttributeObject(train, 0.5);
var permutations = permGenerator.Generate(100);

//setup gamma-bireduct generator
//generate bireducts based on permutations
var bireductGammaGenerator = new BireductGammaGenerator();
bireductGammaGenerator.DecisionTable = train;
bireductGammaGenerator.Permutations = permutations;
var bireducts = bireductGammaGenerator.GetReducts();

//for each bireduct show its attributes and supported objects
foreach (var bireduct in bireducts)
{
	Console.WriteLine(
		bireduct.Attributes.ToArray().ToStr());
	
	Console.WriteLine(
		bireduct.SupportedObjects.ToArray().ToStr());
}
{% endhighlight %}
