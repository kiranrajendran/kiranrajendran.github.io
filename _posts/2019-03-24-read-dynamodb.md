---
date: 2019-03-24 12:00:00
layout: post
title: Querying AWS DynamoDB with Pentaho
subtitle: User Defined Java Class to call AWS SDK
description: Read DynamoDB tables with Pentaho
image: https://res.cloudinary.com/dvexqgcid/image/upload/c_scale,w_680/v1584930993/krghio/DynamoDB_Blog_Header_2_jktsnd.png
optimized_image: https://res.cloudinary.com/dvexqgcid/image/upload/c_scale,w_380/v1584930993/krghio/DynamoDB_Blog_Header_2_jktsnd.png
category: data
tags:
  - pentaho
  - aws
  - java
  - kettle
author: kiranrajendran
---
A quick example on how to use a User Defined Java Class (UDJC) to query Amazon DynamoDB with Pentaho 8.2.x (probably works in other versions as well).

The UDJC uses the local machines `DefaultAWSCredentialsProviderChain` to find authentication credentials (environment variables, java properties, profile, etc.).

There is opportunity to create a single request with multiple `Items` and send them in a <a href=" https://docs.aws.amazon.com/cli/latest/reference/dynamodb/batch-get-item.html" target="_blank">single batch</a>.

The transformation can be used to grab configuration information for an execution e.g. read server properties for a project/environment/tenant and set them at top of your ETL cycle. 

![alt text](/assets/post-images/2019-03-24-read-dynamodb-001.png "Image 001")

<a href="https://github.com/kiranrajendran/krghio/tree/master/dynamho" target="_blank">Click here for sample put/get to DynamoDB from AWS CLI</a>  
<a href="https://github.com/kiranrajendran/krghio/blob/master/dynamho/t_read_dynamodb.ktr" target="_blank">Click here for transformation</a>  
<a href="https://github.com/kiranrajendran/krghio/blob/master/dynamho/udjc.java" target="_blank">Click here for UDJC</a>  