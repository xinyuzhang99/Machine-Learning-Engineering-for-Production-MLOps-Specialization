# Week 1: Overview of the ML Lifecycle and Deployment

## ==1. Overview==

- **Deployment Example** (Visual Inspection of Phones)

  <img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220713215138338.png" alt="image-20220713215138338" style="zoom:80%;" />

  <img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220713215138338.png" alt="image-20220713215138338" style="zoom:80%;" />

  <An ***edge device*** is configured via local access and also has a port to connect it to the internet and cloud. >

  --> after successfully training a machine learning model on Jupyter Notebook, it's likely that the actual dataset changes because of the environment conditions (lighting...) --> a machine learning engineer needs to adjust the data distribution

- **ML in Production**

  --> in the production process, only 5-10% of code is for ML model

  - <u>The requirements surrounding ML infrastructure</u>

    ![image-20220713220214226](/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220713220214226.png)

    <font color=blue>**Key point: to systematically plan out the life cycle of a machine learning project**</font>

## ==2. Steps of an ML Project==

![IMG_F644553F5F4C-1](/Users/xinyuzhang/Downloads/IMG_F644553F5F4C-1.jpeg)

- **Case Study: Speech Recognition**

  - <u>Step 1: Scoping</u>

    - Decide to work on speech recognition for voice search
    - Decide on key **metrics**: accuracy, latent, throughput (how many queries per second could we handle)
    - Estimate resources and timeline needed

  - <u>Step 2: Data</u>

    - Define data: <font color=blue>**[Make sure to have high quality data]**</font>

      - Is the data labeled consistently?

        <img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220713221717983.png" alt="image-20220713221717983" style="zoom: 25%;" />

        --> choose the first and second translation (the third translation is inconsistent and may be confusing for learning algorithms)

        --> <font color=blue>**Set up a standard at the beginning**</font>

      - How much silence before/after each clip?
      - How to perform volume normalization?

  - <u>Step 3: Modeling</u>

    - Three key inputs: 

      <img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220713222222733.png" alt="image-20220713222222733" style="zoom: 33%;" />

      --> In production, tend to fix the code and change data and hyperparameters instead (maybe download code from GitHub and focus on optimizing data --> efficient and cheaper to get higher accuracy) --> <font color=blue>**Data-centric Approach**</font> 

  - <u>Step 4: Deployment</u>

    <img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220713223955742.png" alt="image-20220713223955742" style="zoom:50%;" />

    - Key challenge: **Concept/Data Drift** when data distribution changes

  - <u>Other Import Discipline:</u>

    - MLOps (Machine Learning Operations): software tools and principles to support progress through the ML project lifecycle

## ==3. Key Challenges ==

- **1. Concept/Data Drift**

  <img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220713225427017.png" alt="image-20220713225427017" style="zoom: 33%;" />

  - Concept drift: the mapping ($x \rightarrow y$) changes

    Data drift: the input data distribution of $x$ changes

  - Data may have changed because of language, time...

    - Gradual change along time
    - Sudden change (due to big change like COVID-19) --> collet new data and retrain systems to adapt to new data distribution

- **2. Software Engineering Issues**

  - <u>Checklist of Questions</u> --> make the appropriate software engineering choices when implementing prediction services
    - **Realtime or Batch** predictions
    - Predictions run on **Cloud** (for large computing resources) **vs. Edge/Browser** (mobile and more controlled...)
    - **Compute resources(CPU/GPU/memory)** --> choose the right software based on resources provided
    - **Latency, throughput(QPS)**
    - **Logging** --> may be useful to log as much of the data as possible for analysis and review + provide more data for retraining the learning algorithm
    - **Security and Privacy**

## ==4. Deployment Patterns==

- **Common Deployment Cases**

  - <u>New product/capability</u>: start with a small amount of traffic and then gradually increase

    - <font color=blue>**[Canary Deployment]**</font>: the practice of making staged releases --> to spot problems early on
      - Roll out a software update to a small part of the users initially, so they may test it and provide feedback. 
      - Monitor system and ramp up traffic gradually

  - <u>Automate/Assist with manual task</u>: with previous knowledge and experience 

    - **<font color=blue>[Shadow Mode] is usually used</font>**: ML shadows the human and ***run in parallel*** + ML system's output ***not used for any decisions*** during this phase

      <img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220713233532974.png" alt="image-20220713233532974" style="zoom:50%;" />

      --> gather data of how the algorithm performs and how that compares to the human judgements

  - <u>Replace previous ML system</u> 

    - <font color=blue>**[Blue-Green Deployment]**</font>: let the router switch over the traffic between old and new versions

      ![image-20220714190821370](/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220714190821370.png)

      - Easy way to enable rollback

  - <font color=red>**Key Ideas:**</font>

    - Gradual ramp up with monitoring --> rather than sending tons of traffic
    - Rollback: if the new algorithm is not working, can revert back to the previous system

- **Import Framework: ==Degrees of Automation==**

  <img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220714191245878.png" alt="image-20220714191245878" style="zoom:50%;" />

## ==5. Monitoring==

- **Monitoring Dashboard**

  --> The most common way to monitor a machine learning system to track performance over time

  ![image-20220714192910455](/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220714192910455.png)

  - Set thresholds for alarms
  - Adapt metrics and thresholds over time

- **Metric Examples to Track**

  - <u>Software Metrics:</u> Memory, compute, latency, throughput, server load
  - <u>Input Metrics (x):</u> Average input length, average input volume, number of missing values (very common with structured data), average image brightness <font color=blue>**[for each component]**</font>
  - <u>Output Metrics (y):</u> number of times return `''/None`; number of times the user redoes search (may misrecognize the user's query the first time around); number of times the user swithes to typing; CTR (click-through rate) <font color=blue>**[spot problems]**</font>

- **Deployment is also Iterative as well as Modeling**

  <img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220714193842038.png" alt="image-20220714193842038" style="zoom: 50%;" />

- **Model Maintenance**
  - <u>Manual Retraining</u> (more common)
  - <u>Automatic Retraining</u> --> happen more in consumer software services

	## ==6. Pipeline Monitoring==

<img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220714223757802.png" alt="image-20220714223757802" style="zoom: 33%;" />

--> the second example has two modules of learning algorithms, and the performance of the first module might affect the performance of the second module

<img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220715002852869.png" alt="image-20220715002852869" style="zoom: 33%;" />

<img src="/Users/xinyuzhang/Library/Application Support/typora-user-images/image-20220715003312001.png" alt="image-20220715003312001" style="zoom:50%;" />