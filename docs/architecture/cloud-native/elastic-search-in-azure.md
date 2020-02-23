---
title: 云本机应用程序中的 Elasticsearch
description: 了解如何将弹性搜索功能添加到云本机应用程序。
author: robvet
ms.date: 01/22/2020
ms.openlocfilehash: 6ea237eddc89a8c6843d6b34b05b1b71515a99b6
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/28/2020
ms.locfileid: "76795037"
---
# <a name="elasticsearch-in-a-cloud-native-app"></a><span data-ttu-id="a9040-103">云本机应用中的 Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="a9040-103">Elasticsearch in a cloud-native app</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="a9040-104">Elasticsearch 是一种分布式搜索和分析系统，可跨不同类型的数据实现复杂的搜索功能。</span><span class="sxs-lookup"><span data-stu-id="a9040-104">Elasticsearch is a distributed search and analytics system that enables complex search capabilities across diverse types of data.</span></span> <span data-ttu-id="a9040-105">它是开源的，广泛流行。</span><span class="sxs-lookup"><span data-stu-id="a9040-105">It's open source and widely popular.</span></span> <span data-ttu-id="a9040-106">考虑以下公司如何将 Elasticsearch 集成到其应用程序中：</span><span class="sxs-lookup"><span data-stu-id="a9040-106">Consider how the following companies integrate Elasticsearch into their application:</span></span>

- <span data-ttu-id="a9040-107">用于全文和增量（键入时搜索）的[维基百科](https://blog.wikimedia.org/2014/01/06/wikimedia-moving-to-elasticsearch/)。</span><span class="sxs-lookup"><span data-stu-id="a9040-107">[Wikipedia](https://blog.wikimedia.org/2014/01/06/wikimedia-moving-to-elasticsearch/) for full-text and incremental (search as you type) searching.</span></span>
- <span data-ttu-id="a9040-108">[GitHub](https://www.elastic.co/customers/github)用于索引和公开超过8000000的代码存储库。</span><span class="sxs-lookup"><span data-stu-id="a9040-108">[GitHub](https://www.elastic.co/customers/github) to index and expose over 8 million code repositories.</span></span>  
- <span data-ttu-id="a9040-109">用于使容器库可发现的[Docker](https://www.elastic.co/customers/docker) 。</span><span class="sxs-lookup"><span data-stu-id="a9040-109">[Docker](https://www.elastic.co/customers/docker) for making its container library discoverable.</span></span>

<span data-ttu-id="a9040-110">Elasticsearch 基于[Apache Lucene](https://lucene.apache.org/core/)全文搜索引擎。</span><span class="sxs-lookup"><span data-stu-id="a9040-110">Elasticsearch is built on top of the [Apache Lucene](https://lucene.apache.org/core/) full-text search engine.</span></span> <span data-ttu-id="a9040-111">Lucene 提供高性能的文档索引和查询。</span><span class="sxs-lookup"><span data-stu-id="a9040-111">Lucene provides high-performance document indexing and querying.</span></span> <span data-ttu-id="a9040-112">它使用反转索引方案对数据进行索引，而不是将页映射到关键字，而是将关键字映射到页面，就像本书末尾的术语表。</span><span class="sxs-lookup"><span data-stu-id="a9040-112">It indexes data with an inverted indexing scheme – instead of mapping pages to keywords, it maps keywords to pages just like a glossary at the end of a book.</span></span> <span data-ttu-id="a9040-113">Lucene 具有强大的查询语法功能，可以通过以下方式查询数据：</span><span class="sxs-lookup"><span data-stu-id="a9040-113">Lucene has powerful query syntax capabilities and can query data by:</span></span>

- <span data-ttu-id="a9040-114">字词（完整单词）</span><span class="sxs-lookup"><span data-stu-id="a9040-114">Term (a full word)</span></span> 
- <span data-ttu-id="a9040-115">前缀（开头-带有 word）</span><span class="sxs-lookup"><span data-stu-id="a9040-115">Prefix (starts-with word)</span></span>
- <span data-ttu-id="a9040-116">通配符（使用 "\*" 或 "？" 筛选器）</span><span class="sxs-lookup"><span data-stu-id="a9040-116">Wildcard (using "\*" or "?" filters)</span></span>
- <span data-ttu-id="a9040-117">短语（文档中的文本序列）</span><span class="sxs-lookup"><span data-stu-id="a9040-117">Phrase (a sequence of text in a document)</span></span>
- <span data-ttu-id="a9040-118">布尔值（复杂搜索合并查询）</span><span class="sxs-lookup"><span data-stu-id="a9040-118">Boolean value (complex searches combining queries)</span></span>

<span data-ttu-id="a9040-119">虽然 Lucene 为搜索提供低级别的管道，但 Elasticsearch 提供了位于 Lucene 顶层的服务器。</span><span class="sxs-lookup"><span data-stu-id="a9040-119">While Lucene provides low-level plumbing for searching, Elasticsearch provides the server that sits on top of Lucene.</span></span> <span data-ttu-id="a9040-120">Elasticsearch 添加了更高级别的功能以简化工作 Lucene，其中包括用于访问 Lucene 的索引和搜索功能的 RESTful API。</span><span class="sxs-lookup"><span data-stu-id="a9040-120">Elasticsearch adds higher-level functionality to simplify working Lucene, including a RESTful API to access Lucene’s indexing and searching functionality.</span></span> <span data-ttu-id="a9040-121">它还提供了一个能够提供巨大的可伸缩性、容错和高可用性的分布式基础结构。</span><span class="sxs-lookup"><span data-stu-id="a9040-121">It also provides a distributed infrastructure capable of massive scalability, fault tolerance, and high availability.</span></span>

<span data-ttu-id="a9040-122">对于更大的具有复杂搜索要求的云本机应用程序，Elasticsearch 可用作 Azure 中的托管服务。</span><span class="sxs-lookup"><span data-stu-id="a9040-122">For larger cloud-native applications with complex search requirements, Elasticsearch is available as managed service in Azure.</span></span> <span data-ttu-id="a9040-123">Microsoft Azure 市场提供了预先配置的模板，开发人员可以使用这些模板在 Azure 上部署 Elasticsearch 群集。</span><span class="sxs-lookup"><span data-stu-id="a9040-123">The Microsoft Azure Marketplace features preconfigured templates which developers can use to deploy an Elasticsearch cluster on Azure.</span></span>

<span data-ttu-id="a9040-124">在 Microsoft Azure 市场中，开发人员可以使用生成的预配置模板在 Azure 上快速部署 Elasticsearch 群集。</span><span class="sxs-lookup"><span data-stu-id="a9040-124">From the Microsoft Azure Marketplace, developers can use preconfigured templates built to quickly deploy an Elasticsearch cluster on Azure.</span></span> <span data-ttu-id="a9040-125">使用 Azure 托管产品，最多可部署50个数据节点、20个协调节点和三个专用主节点。</span><span class="sxs-lookup"><span data-stu-id="a9040-125">Using the Azure-managed offering, you can deploy up to 50 data nodes, 20 coordinating nodes, and three dedicated master nodes.</span></span>

## <a name="summary"></a><span data-ttu-id="a9040-126">摘要</span><span class="sxs-lookup"><span data-stu-id="a9040-126">Summary</span></span>

<span data-ttu-id="a9040-127">本章详细介绍了云本机系统中的数据。</span><span class="sxs-lookup"><span data-stu-id="a9040-127">This chapter presented a detailed look at data in cloud-native systems.</span></span> <span data-ttu-id="a9040-128">我们在云本机系统中的数据存储模式与整体应用程序中的数据存储进行了对比。</span><span class="sxs-lookup"><span data-stu-id="a9040-128">We started by contrasting data storage in monolithic applications with data storage patterns in cloud-native systems.</span></span> <span data-ttu-id="a9040-129">我们查看了在云本机系统中实现的数据模式，其中包括跨服务查询、分布式事务和用于处理大容量系统的模式。</span><span class="sxs-lookup"><span data-stu-id="a9040-129">We looked at data patterns implemented in cloud-native systems, including cross-service queries, distributed transactions, and patterns to deal with high-volume systems.</span></span> <span data-ttu-id="a9040-130">我们将 SQL 与 NoSQL 数据相对应。</span><span class="sxs-lookup"><span data-stu-id="a9040-130">We contrasted SQL with NoSQL data.</span></span> <span data-ttu-id="a9040-131">我们查看了 Azure 中的可用数据存储选项，其中包括 Microsoft 中心和开源选项。</span><span class="sxs-lookup"><span data-stu-id="a9040-131">We looked at data storage options available in Azure that include both Microsoft-centric and open-source options.</span></span> <span data-ttu-id="a9040-132">最后，我们在云本机应用程序中讨论了缓存和 Elasticsearch。</span><span class="sxs-lookup"><span data-stu-id="a9040-132">Finally, we discussed caching and Elasticsearch in a cloud-native application.</span></span>

### <a name="references"></a><span data-ttu-id="a9040-133">引用</span><span class="sxs-lookup"><span data-stu-id="a9040-133">References</span></span>

- [<span data-ttu-id="a9040-134">命令和查询责任分离（CQRS）模式</span><span class="sxs-lookup"><span data-stu-id="a9040-134">Command and Query Responsibility Segregation (CQRS) pattern</span></span>](https://docs.microsoft.com/azure/architecture/patterns/cqrs)

- [<span data-ttu-id="a9040-135">事件溯源模式</span><span class="sxs-lookup"><span data-stu-id="a9040-135">Event Sourcing pattern</span></span>](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)

- [<span data-ttu-id="a9040-136">Rdbms 和 NoSQL 数据库：概述</span><span class="sxs-lookup"><span data-stu-id="a9040-136">RDBMSs vs. NoSQL Databases: Overview</span></span>](https://maxivak.com/rdbms-vs-nosql-databases/)

- [<span data-ttu-id="a9040-137">为什么在 CAP 定理中不能承受 RDBMS 分区，为什么它可用？</span><span class="sxs-lookup"><span data-stu-id="a9040-137">Why isn't RDBMS Partition Tolerant in CAP Theorem and why is it Available?</span></span>](https://stackoverflow.com/questions/36404765/why-isnt-rdbms-partition-tolerant-in-cap-theorem-and-why-is-it-available)

- [<span data-ttu-id="a9040-138">具体化视图</span><span class="sxs-lookup"><span data-stu-id="a9040-138">Materialized View</span></span>](https://docs.microsoft.com/azure/architecture/patterns/materialized-view)

- [<span data-ttu-id="a9040-139">你确实需要知道开源数据库</span><span class="sxs-lookup"><span data-stu-id="a9040-139">All you really need to know about open source databases</span></span>](https://www.ibm.com/blogs/systems/all-you-really-need-to-know-about-open-source-databases/)

- [<span data-ttu-id="a9040-140">补偿事务模式</span><span class="sxs-lookup"><span data-stu-id="a9040-140">Compensating Transaction pattern</span></span>](https://docs.microsoft.com/azure/architecture/patterns/compensating-transaction)

- [<span data-ttu-id="a9040-141">Saga 模式</span><span class="sxs-lookup"><span data-stu-id="a9040-141">Saga Pattern</span></span>](https://microservices.io/patterns/data/saga.html)

- [<span data-ttu-id="a9040-142">Saga 模式 |如何使用微服务实现业务事务</span><span class="sxs-lookup"><span data-stu-id="a9040-142">Saga Patterns | How to implement business transactions using microservices</span></span>](https://blog.couchbase.com/saga-pattern-implement-business-transactions-using-microservices-part/)

- [<span data-ttu-id="a9040-143">补偿事务模式</span><span class="sxs-lookup"><span data-stu-id="a9040-143">Compensating Transaction pattern</span></span>](https://docs.microsoft.com/azure/architecture/patterns/compensating-transaction)

- [<span data-ttu-id="a9040-144">在9球后面：说明 Cosmos DB 一致性级别</span><span class="sxs-lookup"><span data-stu-id="a9040-144">Getting Behind the 9-Ball: Cosmos DB Consistency Levels Explained</span></span>](https://blog.jeremylikness.com/blog/2018-03-23_getting-behind-the-9ball-cosmosdb-consistency-levels/)

- [<span data-ttu-id="a9040-145">探索不同类型的 NoSQL 数据库第 II 部分</span><span class="sxs-lookup"><span data-stu-id="a9040-145">Exploring the different types of NoSQL Databases Part II</span></span>](https://www.3pillarglobal.com/insights/exploring-the-different-types-of-nosql-databases)

- [<span data-ttu-id="a9040-146">在 RDBMS、NoSQL 和 NewSQL 数据库上。与 John Ryan 访谈</span><span class="sxs-lookup"><span data-stu-id="a9040-146">On RDBMS, NoSQL and NewSQL databases. Interview with John Ryan</span></span>](http://www.odbms.org/blog/2018/03/on-rdbms-nosql-and-newsql-databases-interview-with-john-ryan/)
  
- [<span data-ttu-id="a9040-147">SQL vs NoSQL vs NewSQL：完全比较</span><span class="sxs-lookup"><span data-stu-id="a9040-147">SQL vs NoSQL vs NewSQL: The Full Comparison</span></span>](https://www.xenonstack.com/blog/sql-vs-nosql-vs-newsql/)

- [<span data-ttu-id="a9040-148">破折号： Kubernetes 的四个属性-本机数据库</span><span class="sxs-lookup"><span data-stu-id="a9040-148">DASH: Four Properties of Kubernetes-Native Databases</span></span>](https://thenewstack.io/dash-four-properties-of-kubernetes-native-databases/)

- [<span data-ttu-id="a9040-149">CockroachDB</span><span class="sxs-lookup"><span data-stu-id="a9040-149">CockroachDB</span></span>](https://www.cockroachlabs.com/)

- [<span data-ttu-id="a9040-150">TiDB</span><span class="sxs-lookup"><span data-stu-id="a9040-150">TiDB</span></span>](https://pingcap.com/en/)

- [<span data-ttu-id="a9040-151">YugabyteDB</span><span class="sxs-lookup"><span data-stu-id="a9040-151">YugabyteDB</span></span>](https://www.yugabyte.com/)

- [<span data-ttu-id="a9040-152">Vitess</span><span class="sxs-lookup"><span data-stu-id="a9040-152">Vitess</span></span>](https://vitess.io/)

- [<span data-ttu-id="a9040-153">Elasticsearch：权威指南</span><span class="sxs-lookup"><span data-stu-id="a9040-153">Elasticsearch: The Definitive Guide</span></span>](http://shop.oreilly.com/product/0636920028505.do)
  
- [<span data-ttu-id="a9040-154">Apache Lucene 简介</span><span class="sxs-lookup"><span data-stu-id="a9040-154">Introduction to Apache Lucene</span></span>](https://www.baeldung.com/lucene)

>[!div class="step-by-step"]
><span data-ttu-id="a9040-155">[上一页](azure-caching.md)
>[下一页](resiliency.md)</span><span class="sxs-lookup"><span data-stu-id="a9040-155">[Previous](azure-caching.md)
[Next](resiliency.md)</span></span> <!-- Next Chapter -->