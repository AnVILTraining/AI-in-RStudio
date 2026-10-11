


# Introduction

## Welcome

Welcome to gander-assisted data analysis in RStudio on AnVIL. This course shows you how to utilize Large Language Models (LLMs) inside your RStudio environment on AnVIL.

## Motivation

Adding an AI assistant normally means exposing your data and code to a third-party API provider. On **AnVIL**, you can run a model locally on your own provisioned cloud instance using **Ollama**, connect to it in R using **ellmer**, and leverage **gander** as an in-session coding partner. This approach ensures complete data privacy and no sensitive information leaving your environment.

## Target Audience  

The course is intended for ...

## Curriculum  

Chapter 1: **Introduction**.

Chapter 2: **Run Local Models. Provision a GPU-accelerated RStudio environment on AnVIL, launch a local **Ollama** server, and download the `qwen3-coder:30b` model.

Chapter 3: **Explore with gander**. Configure `gander`, set up keyboard shortcuts, inspect data workflows using gander_peek(), and import the airway dataset from Google Cloud Storage.

Chapter 4: **Run DESeq2**. Filter low counts, execute differential expression analysis, and visualize biological results.

## Abstract

This workshop puts an open-source large language model directly inside RStudio on AnVIL, running on the same cloud instance as your analysis. There are no paid APIs to configure, no keys to leak, and no data leaving your environment.

Using **gander** an RStudio assistant, your session context - active environment objects, data frame column structures, and surrounding code - is passed seamlessly to an **ellmer** chat connected locally via **Ollama**. The workflow uses the ‘airway’ dataset (smooth muscle cells, four cell lines, treated with dexamethasone or untreated). 

**Keywords**: AI-assisted data analysis, gander, ellmer, Ollama, AnVIL, large language models, reproducible research, RNA-seq, DESeq2, genomic data science education

