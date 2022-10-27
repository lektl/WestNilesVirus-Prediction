# Project 4 : West Nile Virus Prediction

Emma|Emi|Melissa|Tiek Leong


## Background
West Nile virus (WNV) (Flaviviridae: Flavivirus) is an arbovirus circulating among mosquitoes, which serve as the vectors, and wild birds, which serve as the main reservoir hosts. WNV raises public health concerns for its ability to infect humans, which are considered dead-end hosts due to our inability to develop a sufficient viremia to infect mosquitoes. So, environmental surveillance, particularly surveillance based on mosquito sampling, can provide early detection of the virus circulation before the onset of the disease in humans.
WMV is the leading cause of mosquito-borne disease in the continental United States. It is most commonly spread to people by the bite of an infected mosquito. Cases of WNV occur during mosquito season, which starts in the summer and continues through fall. There are no vaccines to prevent or medications to treat WNV in people. Fortunately, most people infected with WNV do not feel sick. About 1 in 5 people who are infected develop a fever and other symptoms. About 1 out of 150 infected people develop a serious, sometimes fatal, illness. (source)
There is no vaccine to prevent WNV infection. The best way to prevent WNV is to protect yourself from mosquito bites. Use insect repellent, wear long-sleeved shirts and pants, treat clothing and gear, and take steps to control mosquitoes indoors and outdoors.
So, every year, from late-May to early-October, public health workers in Chicago setup mosquito traps scattered across the city. Every week from Monday through Wednesday, these traps collect mosquitoes, and the mosquitoes are tested for the presence of West Nile virus before the end of the week. The test results include the number of mosquitoes, the mosquitoes species, and whether or not West Nile virus is present in the cohort.

## Introduction
West Nile virus is most commonly spread to humans through infected mosquitos. Around 20% of people who become infected with the virus develop symptoms ranging from a persistent fever, to serious neurological illnesses that can result in death. In 2002, the first human cases of West Nile virus were reported in Chicago. By 2004 the City of Chicago and the Chicago Department of Public Health (CDPH) had established a comprehensive surveillance and control program that is still in effect today. Every week from late spring through the fall, mosquitos in traps across the city are tested for the virus. The results of these tests influence when and where the city will spray airborne pesticides to control adult mosquito populations.

## Problem Statement
Given weather, location, testing, and spraying data, we are tasked to predict when and where different species of mosquitos will test positive for West Nile virus. We want to come up with an accurate method of predicting outbreaks of West Nile virus in mosquitos so as to help the City of Chicago and CPHD  allocate resources more efficiently and effectively to prevent the transmission of this potentially deadly virus. We aim to predict traps with higher likelihood of West Nile Virus for targeted action.

## Data Dictionary

| Data|Description|
| ---| ---|
|[train](../dataset/train.csv)| 10,506 raw records data of traps tested in years 2007, 2009, 2011, and 2013.|
|[test](../dataset/test.csv)| 116,293 raw data of traps tested in years 2008, 2010, 2012, and 2014.|
|[weather](../dataset/weather.csv)| 2,944 raw data of weather conditions collected from 2 weather stations from 2007 to 2014|
| [spray](../dataset/spray.csv)| 14,835 raw data of spraying performed in 2011 and 2013|

Data dictionary of the new features;

| Feature|Type|Description|
| ---| ---| ---|
| species| int| convert to ordinal feature based on occurance of the species. 3 for the species with the highest occurance and 0 for other species.|
| wet_dry| int|1 if there was an event of either torrential storm, rain, drizzle or shower and = 0 if otherwise|


## Preprocessing

1. Dummfy the `trap` feature which is categorical by nature.
2. Apply SMOTE on train data to handle severe data imbalance where 95% of the records were  without WNV and 5% detected with WNV.  
3. Apply standard scaling on train data.


## Model Evaluation

|Classifier|Train score|Test score|Generalisation|Sensitivity|Specificity|Precision|F1 score|Test ROC AUC|Runtime|
|---|---|---|---|---|---|---|---|---|---|
|Logistic Regression|0.8089|0.7408|9.19%|0.6404|0.7464|0.1237|0.2074|0.773|13min 33s|
|Decision Tree|0.9117|0.8927|2.13%|0.3333|0.924|0.1969|0.2476|0.802|11.2s|
|Random Forest|0.9641|0.9387|2.71%|0.2544|0.9769|0.3816|0.3053|0.8703|32min 42s|
|Ada Boost|0.8926|0.8537|4.56%|0.5789|0.8691|0.1982|0.2953|0.8435|1min 33s|
|Gradient Boost|0.9693|0.9392|3.21%|0.1228|0.9848|0.3111|0.1761|0.8693|5min 26s|
|XGBoost|0.9671|0.9392|2.98%|0.114|0.9853|0.3023|0.1656|0.8689|8min 37s|

The models were created using Logistic Regression, Decision Trees, Random Forest, Ada Boost, Gradient Boost and XG Boost. Whilst, all models perform better than the baseline model which has an AUC_ROC score of 0.69. the logistic regression model does not generalise well at 9.19% and will not be considered.

Since the data is serverely imbalance, we will evaluate the models based on the f1 score. Among the remaining 5 models, the Random Forest and Adaboost models have higher F1 score, with Random Forest model having the higest AUC-ROC score which is better that classifying traps with or without WNV. As such, the Random Forest model is selected as the final model.

![](../image/AUC_ROC.png)
![](../image/confusionmatrix.png)


## Cost Benefit Analysis

Per the [surveillance newsletter] (https://www.chicago.gov/content/dam/city/depts/cdph/statistics_and_reports/CDInfo_2013_JULY_WNV.pdf) from the CDPH, adulticide (Zenivex ®) was used on 11 occasions in July through late August. 
  
We evaluate the cost using the same insecticide and spray frequency.

| |Whole of Chicago|Target Traps|
| ---| ---|---|
| Number of Traps| NA|27|
| Area (acres)    | 145,300 |69,120* | 
| Cost (USD / acre)| 67¢ |67¢|
| Spray Frequency | 11    | 11|   
| Total (USD)|$1,070,861|$509,414|

Cost saving: $561,447 (52%)

'* Mosquitoes can fly up to 2 miles from their palce of birth. 4sq mile = 2560 acres

## Conclusion

Our model has identified 27 traps for targeted action and with the cost-benefit analysis performed, significant cost savings could be achieved. 

However, the current analysis is oversimplified and generalised which may not be an accurate reflection of the projected spendings/ cost incurred. In addition, other factors such as population density is not considered to evaluate the cost and effort of spraying in low density neighbourhood.

## Recommendation

Explore other efforts such as:

1. Introducing male wolbachia mosquitoes to mate with virus carrying female Culex mosquitoes to prevent eggs from hatching hence reducing the likelihood of new mosquitoes carrying West Nile Virus.
2. Looking into stagnant water bodies that could potentially be a favourable mosquitoes breeding ground. 
3. Educating people on preventive methods to reduce the breeding of mosquitoes




## Team
Lek Tiek Leong, Emma Goh, Emi Chua, Melissa Ng


## References
1. [https://www.cdc.gov/mosquitoes/about/life-cycles/culex.html](https://www.cdc.gov/mosquitoes/about/life-cycles/culex.html)
2. [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4342965] (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4342965)
2. [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3945683] (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3945683)
3. [https://www.chicago.gov/content/dam/city/depts/cdph/statistics_and_reports/CDInfo_2013_JULY_WNV.pdf] (https://www.chicago.gov/content/dam/city/depts/cdph/statistics_and_reports/CDInfo_2013_JULY_WNV.pdf)
4. [https://www.cdph.ca.gov/Programs/CCDPHP/DEODC/OHB/OPIPP/CDPH%20Document%20Library/mosquitocontrol1.pdf] (https://www.cdph.ca.gov/Programs/CCDPHP/DEODC/OHB/OPIPP/CDPH%20Document%20Library/mosquitocontrol1.pdf)