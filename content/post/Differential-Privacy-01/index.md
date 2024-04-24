---
title: Differential Privacy - An Overview
subtitle: In this blog post, we'll get into the world of Differential Privacy, a crucial concept in Privacy Preserving. We'll break down complex concepts into simple, easy-to-understand language, exploring key concepts such as Local and Global Differential Privacy, noise injection, and more. 

links:
  - icon: twitter
    icon_pack: fab
    name: Reach me out!
    url: https://twitter.com/amdjedbensalah


# Summary for listings and search engines
summary: Discover the fundamentals of Differential Privacy, a crucial concept in Privacy Preserving. Learn about Local and Global Differential Privacy, noise injection, and more. Perfect for beginners and experts alike, this guide demystifies the world of DP and its applications.

# Link this post with a project
projects: []

# Date published
date: '2024-04-24T00:00:00Z'

# Date updated
lastmod: '2024-04-24T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: true

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
#image:
#  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
#  focal_point: ''
#  placement: 2
#  preview_only: false

authors:
  - admin

tags:
  - Privacy-Preserving series
  - Differential Privacy


categories:
  - Privacy-Preserving series
  - Blog
---



## 📋  TL;DR

* 👉 **What is Differential Privacy?**: A mathematical framework to provide strong privacy when releasing information derived from a dataset.
* 💡**How to achieve DP**: Add noise to the data or results, limit privacy loss by adding noise, random sampling, composition, and more.
* 📚 **Types of DP**: Local (LDP) and Global (GDP) Differential Privacy, with LDP protecting data at the source and GDP relying on a trusted central aggregator.
* 📚 **Shuffler DP**: A variant of LDP that adds an additional layer of privacy by shuffling the noised data.
* 💡 **Bounded DP**: Achieved through techniques like data perturbation or noise injection, adding random noise to ensure the data analyst cannot accurately infer information about any individual.
* 💡 **Unbounded DP**: Does not put a limit on the amount of information that can be inferred about an individual.

#### Differential Privacy Overview
*To explain DP In simple terms*, we can say that it's an approach that allows organizations to perform useful analysis on sensitive data without compromising the privacy of individuals represented in that data.

DP is a mathematical framework designed to provide strong privacy when releasing information derived from a dataset.
It works on achieving this by ensuring that the removal/addition of a single instance doesn't effect the outcome of the query or any analysis, so by this we can hide the presence or absence of any particular individual's data.

>This is done by adding random noise to the data OR the results of queries to the data.

##### Now that we know the formal definition, how do we achieve it?

**We can limit Privacy loss by:**
1. **Adding noise**, we can imagine it as an image the more you add,  the more blurrier it gets, the less you can tell.
2. **Random sampling**, if we randomly sample a fraction of `q` of the data, rather than the **entire** data, then an (ε1, δ1)-differentially-private mechanism M, becomes (qε1,q δ1)-differentially-private.
3. **Composition in DP**, came out after figuring out that asking too much **relevant** questions, will end up in revealing information about individuals.
	1. if we run an (ε1, δ1) diff private mechanism followed by an (ε2, δ2)-differentially private, The whole thing is (ε1 + ε2, δ1 + δ2)-differentially private.
4. More about composition: 
	1. **Sequential composition**: this form applies when a sequence of differentially private operations are performed one after the other **on the same dataset**. if each individual operation satisfies DP with a certain ε parameter, then the overall ε for the sequence is the sum.
		- if we have n operations, the overall ε for the sequence is bounded by: ε = ε_1 + ε_2 + ..... + ε_n).
	2. **Parallel composition**: This form applies when multiple independent DP operations are performed **in parallel** on the same dataset. if each individual op satisfies DP with a certain parameter ε, then the overall ε for the parallel ops can be bounded by taking the maximum ε values.
		- if we have n operations with ε_1 + ε_2 + ..... + ε_n, then the overall ε = max( ε_1 + ε_2 + ..... + ε_n) 

#### Instantiations of Differential Privacy
There are two main types of differential privacy instantiation: local and global.
##### 1. Local Differential Privacy (LDP)
Each individual perturbs their own data before sending it to the data collector/aggregator. This means the raw data is privatized at the source providing a strong guarantee that the data never leaves the user's end in an un-noised form.
It is used in scenarios where users are more concerned about their privacy and want to protect it before sharing to the data collector.
- **Trust Model**: LDP does not require users to trust a central aggregator with their raw data, as privacy is maintained at the local level
##### 2. Central/Global Differential Privacy (CDP)
A model that is based on the following approach: It's the data collector responsibility to add noise to the data (or the results of the queries), this approach assumes that the aggregator is trustworthy to handle the raw data and only release noised outcome.
- **Trust Model**: GDP assumes a trusted central server that collects raw data and provides privacy guarantees by releasing only noisy results or statistics[


##### 3. Shuffler DP:
A variant of LDP, which adds an additional layer of privacy to the **noised** data by shuffling it before it's aggregated.
this helps to further disassociate/split individual data points from their owner.


Both Local & Global DP have their own advantages, inconvenient and use cases.
For example LDP, is used in scenarios where users are more concerned about their privacy and want to protect it before sharing to the data collector. 
While Global DP is more suitable for scenarios where a central authority/entity collects and analyzes data from different sources such as in medical research or census data analysis.

#### Bounded DP vs Unbounded DP
##### Bounded DP:
Given two datasets D and D', D' can be obtained by changing one record from D, while the hamming distance between datasets (is always) = 1, and the cardinality is the same.
- The sensitivity of the query is known and is bounded.
- **The sensitivity of a query**: is a defined as the maximum change in the output of the function that can be caused by changing a single element in the dataset, IT DETERMINES as well how much noise needs to be added to the function's output to maintain the privacy of indiv.
    - For example, consider a dataset of n individuals, where each individual has a **binary attribute (0 or 1)**, A query function that calculates the AVG of the attributes, has a sensitivity of 1/n (since we're calculating just 1s, so it's inversely proportional to the number of individuals (n)), in the case of a query function that calculates the sum, the adding or removing an indiv, can be at most 1 (so the sensitivity is 1).
- Bounded DP is typically achieved through techniques such as data perturbation or noise injection, which add random noise to the data in a way that ensures that the data analyst cannot accurately infer information about any individual.

##### Unbounded DP:
Given two datasets D and D', D' can be obtained by adding/removing one record from D, while the Hamming distance between is 1, but the cardinality is different, (removing one record: D' = D + 1), (adding one record: D' = D + 1).
- Doesn't put a limit on the amount of info that can be inferred about an individual.

---
#### ✅ Takeaway:
##### 1. Privacy Guarantee
* Bounded DP provides a stronger privacy guarantee than unbounded DP, as it ensures that the data analyst cannot learn too much about any individual in the dataset.
* Unbounded DP, on the other hand, relies on privacy loss accounting (spending too much on privacy) to ensure the overall privacy of the dataset within acceptable bounds.

##### 2. Implementation
* Bounded DP is typically easier to implement than unbounded DP, as it requires fewer mathematical computations.
* Unbounded DP, on the other hand, requires more complex mathematical computations to ensure that the privacy risk is within acceptable bounds.

##### 3. Applications
* Unbounded DP is typically used in applications where data privacy is important but not critical.
* Bounded DP, where data privacy is utmost importance, such as in medical research, etc.

---

Feel free to reach out to me if you spot any errors or have questions about the concepts discussed - I'm always here to help and improve!


