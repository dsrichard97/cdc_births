<h1 align="center">CDC Births Data Summary</h1>
<p align="center">
  
<p align="center">
  <img src="birthsjpg.jpg" width="300" alt="Sublime's custom image"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python_Version-3.10%2B-blue" title="Python Version">
  <img src="https://img.shields.io/github/last-commit/dsrichard97/otherprojects">
  <img src="https://img.shields.io/badge/Type_of_ML-NLP-orange">
  <img src="https://img.shields.io/badge/STAT-Algorithms-red">
  <img src="https://img.shields.io/badge/SQL-Descriptive Statistics-brown">
  <img src="https://img.shields.io/badge/STAT-RMSE-blue">
  <img src="https://img.shields.io/badge/Python-Pandas-green">
  <a href="https://github.com/ellerbrock/open-source-badges/"><img src="https://badges.frapsoft.com/os/v1/open-source.svg?v=103"></a>
</p> 

<section id="initial-problem">
  <h2>Initial Problem</h2>
  <p>This report investigates various factors influencing maternity across the United States. It presents an initial analysis of data scraped from the CDC's summary reports, accessible through Google BigQuery.</p>
</section>

<section>
  <h2>Table of Contents</h2>
  <ul>
    <li><a href="#initial-problem" target="_parent">Initial Problem</a></li>
    <li><a href="#about-the-data" target="_parent">About The Data</a></li>
    <li><a href="#SQL-Preliminaries" target="_parent">SQL Preliminaries</a></li>
    <li><a href="#Entity_Relationship_Diagram" target="_parent">Entity Relationship Diagram</a></li>
  </ul>
</section>


<section id="about-the-data">
  <h2>About The Data</h2>
  <p>This dataset, sourced from the CDC's National Center for Health Statistics, provides detailed statistics on live births in the United States from 2016 to 2022, including demographic and health information derived from birth certificates. It covers a range of data such as mother's race, age, tobacco use, delivery methods, and congenital anomalies. The dataset ensures privacy by suppressing data representing 1-9 births and is a part of the National Vital Statistics System available on the CDC WONDER Online Database. For more information visit: https://wonder.cdc.gov/wonder/help/Natality-expanded.html</p>
</section>

<section id="SQL-Preliminaries">
  <h2>SQL Preliminaries</h2>
  <p> This report presents an SQL analysis exploring the relationship between live birth rates and anomalies in the United States from 2016 to 2022. The analysis utilizes data from the CDC's National Center for Health Statistics, accessed via Google BigQuery.
    
<p>For detailed information on the datasets used, including variables within each table, please visit the links provided in the table below.</p>
    <table border="1">
        <tr>
            <th>Preliminary SQL Scrub</th>
            <th>Link</th>
        </tr>
        <tr>
            <td>Datasets Details</td>
            <td><a href="https://github.com/dsrichard97/cdc_births/blob/main/datasets.md">Datasets</a></td>
        </tr>
        <tr>
            <td>Questions and Answers</td>
            <td><a href="https://github.com/dsrichard97/cdc_births/blob/main/questions_and_answers.md">Q&A</a></td>
        </tr>
    </table>
    

  ![alt text](picdataset.png)



<section id="Entity_Relationship_Diagram">
  <h2>Entity Relationship Diagram</h2>
  <p>
    The entity diagram shows the relationship of the inner join used to combine both the tables. Header in tables suggest the keys for each.
    <img src="in1.png" width="900" alt="Sublime's custom image"/>
  </p>
</section>


  <section id="quick-glance">
    <h2>Quick glance at the Results</h2>
 <p>
   Distribution
   
<p align="center">
  <img src="distrib.png" width="300" alt="Sublime's custom image"/>
</p>

Correlation Matrix
<p align="center">
  <img src="corr.png" width="300" alt="Sublime's custom image"/>
</p>
Initial Correlation Assessment:
The heatmap of the correlation matrix provides insights into how these variables are related to each other. For instance, a positive correlation between maternal age and birth weight could suggest that older mothers tend to have babies with higher birth weights.

Predictive Modeling:
Used Random Forest model to identify significant predictors.
Found average birth weight, maternal age, pre-pregnancy BMI, and prenatal weeks as critical factors.
- Average Birth Weight (grams): 2429.82. This was the most significant predictor.
- Average Age of Mother: 2290.50. This was a strong predictor, second only to birth weight.
- Average Pre-pregnancy BMI: 2243.89. Another important predictor​​.

ML Algorithm Accuracy:
Compared radial, sigmoid, and polynomial kernel functions in SVM.
Radial kernel function performed best in terms of RMSE, indicating its effectiveness in modeling high-risk pregnancies.

<table>
        <tr>
            <th>Kernel Function</th>
            <th>Root Mean Square Error (RMSE)</th>
        </tr>
        <tr>
            <td>Radial Kernel Function</td>
            <td>8296.19</td>
        </tr>
        <tr>
            <td>Polynomial Kernel Function</td>
            <td>12857.87</td>
        </tr>
        <tr>
            <td>Sigmoid Kernel Function</td>
            <td>16844.82</td>
        </tr>
    </table>
       

<P>
  <section id="limitation">
    <h2>Limitation and what can be Improved</h2
                                             
- Data Scope: Limited to U.S. data; expanding the data set to include global statistics could offer more comprehensive insights.
- Model Diversity: Explore additional machine learning models beyond Random Forest and SVM for potentially better predictions or insights.
- Feature Engineering: Experiment with more complex features or interactions between variables to enhance model performance.
- Real-Time Data Integration: Incorporate real-time data updates for more dynamic and current analyses.
- User Interface: Develop an interactive dashboard for easier access and interpretation of results by non-technical users.


This project offers valuable insights into maternal and infant health, leveraging advanced data analytics and machine learning techniques. Future work could expand the dataset, explore new models, and enhance user interaction to increase the impact and reach of the findings.
   
<P>
  <section id="notebook">
    <h2>Explore the notebook</h2
 <p>
To explore the R notebook file click here: (https://github.com/dsrichard97/cdc_births/blob/main/cdcsummary.pdf).

<P>
  <section id="report">
    <h2>Report and Presentation</h2
 <p>

 ![alt text](tab.png)

 For more information click here: (https://public.tableau.com/views/cdc_snapshot/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link)

