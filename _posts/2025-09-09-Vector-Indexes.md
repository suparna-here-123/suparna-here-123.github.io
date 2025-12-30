---
title: A Deep Dive into Vector Indexes
tags: [Deep Dive, Tech Blog, Article]
style: fill
color: success
description: Vector indexes, inner workings and my questions :P
---

### Questions I had...

1) Is it okay to add vectors with dimensions originally less than the required dimensions of an index, by padding with zeros? <br><br>
Ans :  Mathematically, increasing the dimensions of a vector by padding with zeros does not change the magnitude of the vector(retains values as is) and can be compared with other equal-dimension vectors using a similarity measure like cosine similiarity. Though this checks out mathematically, the meaning that the zeros give the vector is... just garbage. Embedding models have a fixed output dimension because each one denotes some valuable feature of the input text - this can reduce  accuracy of the search results as the similarity is based on some garbage values.

2) Difference between clusters and namespaces <br><br>
Ans : I am using the Pinecone vector database as a reference and the concept of <i>namespace</i> in it refers to a <i>logical partitioning</i> of vectors within the index, and can be <b>managed by the user</b> to logically organize the vectors based on what they represent so that searches can target a specific namespaces and speed up the process. <br>
Clusters are <b>managed by the vector database</b> , i.e. for internal operations like searching and upserting. From my understanding and implementation, namespaces are clustered to speed up searches within that namespace. 

3) Pinecone's `index.search()` API call has namespace as a required argument - why? Why is there no provision to search across all vectors in the index? <br><br>
Ans : Because a vector database is not meant to search the whole vector space. These vectors are not random groupings of numbers given on a math test - they have meaning, which the user knows beforehand when the index is created and while passing in a query. So why not ask the user to create the logical partitions first and <i>then</i> speed up query within it, right? An example would be an index for an e-commerce website's different entities, each in it's own namespace - like user preferences and product catalog descriptions. If the query is "Find users with similar shopping patterns as Suparna", just query the user-namespace. If the query is "Marvel-themed table lamp", just direct the query to the product-namespace. But if you really need to search the whole index, all vectors should be stored in the same namespace, but this obviously slows down searches.