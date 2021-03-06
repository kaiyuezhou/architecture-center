---
title: Choosing a search data store
description: Compare Azure search data store technologies that create and store specialized indexes for searching free-form text.
author: zoinerTejada
ms.date: 02/12/2018
ms.topic: conceptual
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom:
  - guide
---

# Choosing a search data store in Azure

This article compares technology choices for search data stores in Azure. A search data store is used to create and store specialized indexes for performing searches on free-form text. The text that is indexed may reside in a separate data store, such as blob storage. An application submits a query to the search data store, and the result is a list of matching documents. For more information about this scenario, see [Processing free-form text for search](../scenarios/search.md).

<!-- markdownlint-disable MD026 -->

## What are your options when choosing a search data store?

<!-- markdownlint-enable MD026 -->

In Azure, all of the following data stores will meet the core requirements for search against free-form text data by providing a search index:

- [Azure Search](/azure/search/search-what-is-azure-search)
  - using [CosmosDb](/azure/search/search-howto-index-cosmosdb)
  - using [Azure SQL Database](/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers)
- [Elasticsearch](https://azuremarketplace.microsoft.com/marketplace/apps/elastic.elasticsearch_service?tab=Overview)
- [HDInsight with Solr](/azure/hdinsight/hdinsight-hadoop-solr-install-linux)
- [Azure SQL Database with full text search](/sql/relational-databases/search/full-text-search)

## Key selection criteria

For search scenarios, begin choosing the appropriate search data store for your needs by answering these questions:

- Do you want a managed service rather than managing your own servers?

- Can you specify your index schema at design time? If not, choose an option that supports updateable schemas.

- Do you need an index only for full-text search, or do you also need rapid aggregation of numeric data and other analytics? If you need functionality beyond full-text search, consider options that support additional analytics.

- Do you need a search index for log analytics, with support for log collection, aggregation, and visualizations on indexed data? If so, consider Elasticsearch, which is part of a log analytics stack.

- Do you need to index data in common document formats such as PDF, Word, PowerPoint, and Excel? If yes, choose an option that provides document indexers.

- Does your database have specific security needs? If yes, consider the security features listed below.

## Capability matrix

The following tables summarize the key differences in capabilities.

### General capabilities

| Capability | Azure Search | Elasticsearch | HDInsight with Solr | SQL Database |
| --- | --- | --- | --- | --- |
| Is managed service | Yes | No | Yes | Yes |  
| REST API | Yes | Yes | Yes | No |
| Programmability | .NET, Java, Python | Java | Java | T-SQL |
| Document indexers for common file types (PDF, DOCX, TXT, and so on) | Yes | No | Yes | No |

### Manageability capabilities

| Capability | Azure Search | Elasticsearch | HDInsight with Solr | SQL Database |
| --- | --- | --- | --- | --- |
| Updateable schema | No | Yes | Yes | Yes |
| Supports scale out  | Yes | Yes | Yes | No |

### Analytic workload capabilities

| Capability | Azure Search | Elasticsearch | HDInsight with Solr | SQL Database |
| --- | --- | --- | --- | --- |
| Supports analytics beyond full text search | No | Yes | Yes | Yes |
| Part of a log analytics stack | No | Yes (ELK) |  No | No |
| Supports semantic search | Yes (find similar documents only) | Yes | Yes | Yes |

### Security capabilities

| Capability | Azure Search | Elasticsearch | HDInsight with Solr | SQL Database |
| --- | --- | --- | --- | --- |
| Row-level security | Partial (requires application query to filter by group id) | Partial (requires application query to filter by group id) | Yes | Yes |
| Transparent data encryption | Yes | No | No | Yes |  
| Restrict access to specific IP addresses | Yes | Yes | Yes | Yes |
| Restrict access to allow virtual network access only | Yes | Yes | Yes | Yes |  
| Active Directory authentication (integrated authentication) | No | No | No | Yes |

## Performance Analysis

The file [movies.json](https://raw.githubusercontent.com/prust/wikipedia-movie-data/master/movies.json) contains the data used in this section to compare the performance of the search technologies previously mentioned. The dataset weights around 5 Mb and includes almost 27000 documents.

Notice that the following figures are only for guidance and were obtained using a default deployment, without any manual tuning or proper escalation.

Performance for Elasticsearch and HDInsight with Solr, which are based in Lucene engine, should be similar to Azure Search.

### Performance Comparison

| Capability | Azure Search & CosmosDB | Azure Search & SQL Database | Azure SQL Database Full Text Search |
| --- | --- | --- | --- |
| Indexation | ~1 min  | ~45 secs | ~5 mins |
| Search | ~0.2 secs | ~0.2 secs | ~0.5 secs |

## See also

[Processing free-form text for search](../scenarios/search.md)