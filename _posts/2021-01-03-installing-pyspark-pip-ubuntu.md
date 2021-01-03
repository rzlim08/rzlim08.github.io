---
layout: post
title: "Installing Pyspark with pip on Ubuntu"
categories: Python Spark
---
I've been playing around with pyspark lately, and early on ran into some issues that I didn't find easily documented anywhere. 

The thing that took me the longest to figure out, was where the pyspark-env.sh file was supposed to go. I guess because I used pip to install pyspark into a virtual environment, it wasn't immediately obvious, but with a bit of digging, I found that it was in the venv/lib/<python>/site-packages/pyspark/conf folder. This wasn't automatically created for me, so I had to make it myself. 

After that I manually created the file `pyspark-env.sh`. 

1. Pyspark kept attaching to an external ip interface

For some reason, the default configuration of Pyspark kept attaching to my public ip. This isn't great for a host of security reasons, but I fixed it by adding `SPARK_LOCAL_IP="localhost"` to my pyspark-env.sh 

2. "WARNING: An illegal reflective access operation has occurred" 

I kept getting the above warning upon logging in. Apparently this is because the Apache Arrow version is incompatible with java 11. One way to fix it is to add certain configurations to the program, but I fixed it by downgrading to Java 8 using `JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/`.

3. Opening pyspark in Jupyter

Especially when I'm just testing out a tool, I like to have the immediacy of a jupyter environment. There were a couple different things here though, that weren't expected. I couldn't simply install jupyter and add the virtual environment to the kernelspec. In order to run PySpark with Jupyter, you must configure pyspark to open jupyter, rather than the command line. 

First, run the command:
 
`export PYSPARK_DRIVER_PYTHON='jupyter' && export PYSPARK_DRIVER_PYTHON_OPTS="notebook --NotebookApp.open_browser=False"`

This tell pyspark to open the environment in jupyter. Unfortunately, this results in the error `Exception: Jupyter command jupyter-/home/roy/learning/pyspark/venv/bin/find_spark_home.py not found.
`. This error seems to be due to a bug in the code to find the "SPARK_HOME" directory. The easiest way to solve for me is to manually set the SPARK_HOME directory, but one may want to find a more permanent solution. So I set SPARK_HOME with: `export SPARK_HOME=<PATH_TO_VENV>/lib/python3.6/site-packages/pyspark/`, which was where my personal SPARK_HOME directory was. After that, running `pyspark` opened a jupyter environment. 

From there the things to know are that the variables `spark` and `sc` are automatically added into the python environment to handle calls to the SparkSession and SparkContext respectively. So running the following should work:

```
txt = sc.textFile('/usr/share/doc/python/copyright')
txt.count()
filtered_txt = txt.filter(lambda x: "python" in x.lower())
filtered_txt.count()
```

From there I was pretty much good to go.

Hope that helps someone else out with using pyspark. 
