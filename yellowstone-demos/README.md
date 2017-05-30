# PySpark for Climate

## Background

> Climate and Weather directly impact all aspects of society. Understanding these processeses provide important information for policy - and decision - makers. To understand the average climate conditions and extreme weather events, Earth science and climate change researchers need data from climate models and observations. As such in the typical work flow of climate change and atmospheric research, much time is wasted waiting to reformat and regrid data to homogeneous formats. Additionally, because of the data volume, in order to compute metrics, perform analysis, and visualize data / generate plots, multi-stage processes with repeated I/O are used - the root cause of performance bottlenecks and the resulting user’s frustrations related to time inefficiencies.
**[NASA-SciSpark project](https://scispark.jpl.nasa.gov/about.html)**

<a name="footnote1">1</a>: Spark is a cluster computing paradigm based on the MapReduce paradigm that has garnered many scientific analysis workflows and are very well suited for Spark.  As a result of this lack of great deal of interest for its power and ease of use in analyzing “big data” in the commercial and computer science sectors.  In much of the scientific sector, however --- and specifically in the atmospheric and oceanic sciences --- Spark has not captured the interest of scientists for analyzing their data, even though their datasets may be larger than many commercial datasets interest, there are very few platforms on which scientists can experiment with and learn about using Hadoop and/or Spark for their scientific research.  Additionally, there are very few resources to teach and educate scientists on how or why to use Hadoop or Spark for their analysis.


## Goal

**PySpark for Big Atmospheric & Oceanic Data Analysis** is a [CISL/SIParCS research project](https://www2.cisl.ucar.edu/siparcs) that seeks to explore the realm of distributed parallel computing on NCAR's Yellowstone and Cheyenne supercomputers by taking advantage of: 

- Apache Spark's potential to offer speed-up and advancements of nearly 1000x in-memory

- The increasing growing community around Spark

- Spark's notion of Resilient Distributed Datasets(RDDs). RDDs represent immutable dataset that can be: 
  - reused across multi-stage operations.
  - partitioned across multiple machines.
  - automatically reconstructed if a partition is lost.

to address the pain points that scientists and researchers endure during model evaluation processes. 

Examples of these pain points include:

 - Temporal and Zonal averaging of data
 - Computation of climatologies
 - Pre-processing of CMIP data such as:
   - Regridding 
   - Variable clustering (min/max)
   - calendar harmonizing
