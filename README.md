# Web Crawl Analysis Project

## Overview

This project is a comprehensive analysis of web crawl data using PySpark and various NLP techniques. The aim is to extract meaningful insights from the web crawl data, perform sentiment analysis, and identify dominant topics within the content. The analysis is conducted on a sample dataset obtained from the Common Crawl repository, leveraging AWS S3 for data storage and PySpark for processing.

## Key Features

- **Data Extraction**: Read and parse WARC files from AWS S3.
- **Text Extraction**: Extract textual content from HTML using BeautifulSoup.
- **Sentiment Analysis**: Perform sentiment analysis using TextBlob to classify text as positive, neutral, or negative.
- **Topic Modeling**: Use Latent Dirichlet Allocation (LDA) to identify dominant topics in the text data.
- **Data Visualization**: Visualize the distribution of topics within the dataset.

## Technologies Used

- **AWS S3**: For storing and accessing web crawl data.
- **PySpark**: For distributed data processing and machine learning.
- **BeautifulSoup**: For parsing HTML content.
- **TextBlob**: For sentiment analysis.
- **NLTK**: For text preprocessing and lemmatization.
- **Matplotlib**: For plotting topic distributions.

## Setup Instructions

1. **AWS Credentials**: Ensure you have your AWS credentials (access key and secret access key) ready for accessing S3.
2. **Spark Session**: Initialize a Spark session with appropriate configurations.
3. **Dependencies**: Install required Python libraries:
   ```bash
   pip install boto3 warcio beautifulsoup4 pyspark textblob nltk matplotlib
   ```

## Running the Analysis

1. **Initialize Spark Session**: Start a Spark session with adequate memory and result size configurations.
   ```python
   from pyspark.sql import SparkSession
   
   spark = SparkSession.builder \
       .appName("WebCrawlAnalysis") \
       .config("spark.driver.memory", "8g") \
       .config("spark.executor.memory", "8g") \
       .config("spark.driver.maxResultSize", "4g") \
       .getOrCreate()
   ```

2. **Read WARC Files**: Read web crawl data from S3 using the provided AWS credentials and sample key.
   ```python
   import boto3
   from io import BytesIO
   from warcio.archiveiterator import ArchiveIterator
   
   s3 = boto3.client('s3', aws_access_key_id='YOUR_AWS_ACCESS_KEY_ID', aws_secret_access_key='YOUR_AWS_SECRET_ACCESS_KEY')
   bucket = 'commoncrawl'
   sample_key = 'crawl-data/CC-MAIN-2013-20/segments/1368696381249/warc/CC-MAIN-20130516092621-00000-ip-10-60-113-184.ec2.internal.warc.gz'
   ```

3. **Text Extraction and Sentiment Analysis**: Extract text from HTML, perform sentiment analysis, and clean the text.
   ```python
   from bs4 import BeautifulSoup
   from pyspark.sql.functions import udf, col, split, size
   from pyspark.sql.types import StringType
   from textblob import TextBlob
   import nltk
   nltk.download('wordnet')
   nltk.download('omw-1.4')
   ```

4. **Topic Modeling**: Use CountVectorizer, IDF, and LDA for topic modeling.
   ```python
   from pyspark.ml.feature import CountVectorizer, IDF, Tokenizer, StopWordsRemover
   from pyspark.ml.clustering import LDA
   ```

5. **Visualize Topic Distribution**: Plot the distribution of topics in the dataset.
   ```python
   import matplotlib.pyplot as plt
   import numpy as np
   ```

## Results

- **Sentiment Analysis**: Provides a distribution of sentiments (positive, neutral, negative) across the dataset.
- **Topic Modeling**: Identifies and names the dominant topics within the dataset, such as Technology, Music and Entertainment, Deals and Accounts, and Sports.
- **Data Visualization**: Displays a bar chart showing the proportion of each topic within the dataset.

## Conclusion

This project showcases the ability to handle large-scale web crawl data, perform detailed text analysis, and derive meaningful insights using advanced NLP techniques. The integration of various tools and libraries demonstrates a robust approach to data engineering and analysis.

