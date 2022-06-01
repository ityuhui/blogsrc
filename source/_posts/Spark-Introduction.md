title: Introduction to Apache Spark

date: 2020-06-21 10:59:22

categories:
- 大数据和分布式计算

tags:
- spark

---

## 1 Introduction

Apache spark is a batch computing framework. It is used to replace MapReduce in Hadoop. It can be deployed on Apache Yarn or Mesos, Kubernetes.

<!--more-->

## 1.1 Compare

### 1.1.1 Flink

Flink is a stream computing framework. It is used for millisecond-level computing

## 2 Use Case

### 2.1 Interactive anylysis with the Spark shell 


### 2.2 Self-contain applications written with Spark API

## 3 Programming Guides

### 3.1 RDD

Core and old API

### 3.2 Spark SQL, Datasets, DataFrames

#### Term.
- Dataset is the new interface added in Spark1.6
- DataFrame is a Dataset organized into named columns. It is equivalent to a table in a relational database.

#### Getting Started

1. Create SparkSession
2. Create DataFrames

### 3.3 Structured Streaming 

Using Datasets and DataFrame, newer API than DStreams

### 3.4 Spark Streaming using DStreams 

Using DStreams, old stream API

### 3.5 MLlib

maching learning algorithms

### 3.6 GraphX

graphs computing
