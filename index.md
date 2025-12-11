---
title: "How Economic Resources Shape Educational Outcomes: SES Predictors of Exam Performance"
description: "Ruby Ahlvin · Data Science Final Project<br>Partner Project by Nick Twum"
--- 

## Introduction
Understanding how socioeconomic status (SES) shapes educational outcomes is a central question in both psychology and education research. While large datasets reveal broad trends, micro-level student data can help to gain an understanding of how individual and family characteristics shape academic performance.

To explore this relationship, my project examines whether socioeconomic status predicts exam performance, using a sample with over 6,000 high school students from the Student Performance Factors dataset on Kaggle (Nguyen, 2024). This dataset includes a variety of demographic and socioeconomic variables, such as Parental Education Level, Family Income, Access to Resources, Internet Access, and Exam Score. However, the origin of this dataset can raise some concerns. Because Kaggle datasets are often user-generated and lack documentation about data collection methods, the accuracy and generalizability of this dataset is not guaranteed. With this in mind, the findings of this project should be interpreted with caution. Using the available variables, my project aimed to answer the following research questions: 

1. Does socioeconomic status predict academic performance (Exam Score)?
2. Does parental education alone meaningfully predict exam performance?
3. Does a multi-factor SES model provide stronger predictive power than parental education alone? 
4. Can a composite SES index summarize multiple SES components effectively?

My partner’s project approaches similar questions using a machine-learning model. Instead of focusing solely on SES, his model examines how every variable in the dataset contributes to predicting exam scores. Together, our projects analyze the same dataset from complementary angles, with my project using traditional regression modeling to isolate SES effects, and my partner using machine-learning methods to compare the relative importance of multiple predictors. 

To answer my research questions, I constructed three regression models. All three models looked at the outcome variable of Exam Score. Model 1 included Parental Education Level only, Model 2 incorporated multiple SES variables (Parental Education Level, Family Income, Access to Resources, and Internet Access), and Model 3 used a composite SES index created from the variables in Model 2. 

---

## Methods and Results
The dataset used for this project was the Student Performance Factors dataset available on Kaggle. It contains data on 6,590 high school students, with 20 recorded variables relating to individual information on socioeconomic status (SES), study habits, and academic outcomes. The primary outcome variable I examined was Exam Score, which was treated as a continuous measure of academic performance. I explored several SES predictor variables: Parental Education Level (High School, College, Postgraduate), Family Income (Low, Medium, High), Access to Resources (Low, Medium, High), and Internet Access (Yes/No). 

Although the dataset is extensive and organized, it has potential limitations. Kaggle datasets are often uploaded by their users, and the data could potentially be synthetic or simulated. This dataset provides no documentation about how it was collected, what population it represents, or whether the variables reflect real measurements. Several SES indicators are broad and subjective. For example, Access to Resources or Family Income are recorded as Low, Medium, or High, which makes it unclear how these values were assessed or what metrics were used to decide the cutoff of each of these groups. Additionally, key demographic variables (such as age, race/ethnicity, or grade) are missing, which limits the interpretability and generalizability of the findings. These concerns suggest that the analysis should primarily be considered as exploratory rather than reflective of any real-world student population.

Several data preparation steps were conducted prior to analysis. First, any missing data was identified in the dataset. Three variables were found to contain missing values, only one of which was relevant to my analysis, Parental Education Level. This variable had 90 missing values, which were excluded because the students with missing data did not differ meaningfully in Exam Score from those with complete data, and the remaining sample size was sufficiently large after their removal. Next, SES variables were converted into ordered categorical types to preserve the meaningful ranking of each of these variables, preventing Pandas from potentially applying incorrect alphabetical ordering. Using the new ordered categories, numeric versions of each of these variables were then created. To create a composite measure of overall socioeconomic status, the four predictor SES variables were each scaled from 0-1 and averaged to produce an SES index for each student. This index represents a continuous measure of overall SES, with larger values corresponding to more advantaged socioeconomic circumstances. 

Model 1 assessed whether Parental Education Level alone predicted exam performance. Parental education was a significant predictor of Exam Score ( _p_ < .001), showing that students with parents who completed college or postgraduate school scored higher on average than students whose parents completed only high school. However, the model explained only 1.1% of variance (\( R^2 \) = .011), indicating that parental education is a meaningful predictor, but insufficient to represent SES on its own. 

**Table 1: Regression Output for Model 1**

| Index            | Coefficient | Std Error | t-value  | p-value | CI Lower | CI Upper |
|------------------|-------------|-----------|----------|---------|----------|----------|
| Intercept        |    66.87    |   0.065   | 1032.327 |   0.0   |  66.743  |  66.997  |
| Parent Education |    1.043    |   0.123   |   8.467  |   0.0   |   0.801  |   1.284  |

**Table 1.** _This table reports the estimated regression coefficient and other relevant statistics for Parental Education Level predicting Exam Score. While statistically significant, the effect size is small, and the model fit is weak, indicating that parental education alone explains only a small amount of variance in exam performance._ 

Model 2 included four SES variables: Parental Education Level, Family Income, Internet Access, and Access to Resources. The model showed that all SES variables were significant predictors of academic achievement (all _p_ < .001) in the expected direction. Higher family income was associated with higher exam scores, internet access predicted higher performance, greater access to resources was associated with higher exam scores, and parental education remained a significant predictor even with other SES variables included. Additionally, this model had substantially higher explanatory power than Model 1 (\( R^2 \) = .052). This suggests that SES is multifaceted and is better captured when multiple indicators are considered rather than parental education alone. 

**Table 2: Regression Output for Model 2**

| Index                      | Coefficient | Std Error | t-value | p-value | CI Lower | CI Upper |
|----------------------------|-------------|-----------|---------|---------|----------|----------|
| Intercept                  |    64.692   |   0.208   | 310.559 |    0    |  64.284  |   65.1   |
| Family Income (Med)        |    0.491    |   0.105   |  4.684  |    0    |   0.285  |   0.696  |
| Family Income (High)       |    0.988    |    0.13   |  7.582  |    0    |   0.733  |   1.243  |
| Internet Access (Yes)      |    0.814    |   0.178   |  4.585  |    0    |   0.466  |   1.163  |
| Access to Resources (Med)  |    0.927    |   0.125   |  7.436  |    0    |   0.682  |   1.171  |
| Access to Resources (High) |    1.902    |   0.136   |  13.974 |    0    |   1.635  |   2.169  |
| Parent Education           |    1.054    |   0.121   |  8.734  |    0    |   0.817  |   1.291  |

**Table 2.** _This table reports regression estimates for four SES predictors. Each SES variable significantly predicts Exam Score, with higher SES (income, resources, internet access, or parental education) being associated with higher exam scores. The model shows improved explanatory power compared to Model 1, revealing that SES is better captured when multiple indicators are considered together._


Finally, Model 3 assessed whether a single composite SES index predicted exam performance. The SES index was created by scaling and averaging the four SES predictor variables previously used in Model 2. The SES index significantly predicted Exam Score (_p_ < .001), with higher SES associated with higher academic performance. The model explained 4.6% (\( R^2 \) = .046) of the variance, slightly less than the multi-factor model, but higher than parental education alone. This indicates that while the SES index provides a cleaner and simpler predictor, averaging multiple SES variables reduces variability, slightly reducing predictive power.  

**Table 3: Regression Output for Model 3**

| Index            | Coefficient | Std Error | t-value | p-value | CI Lower | CI Upper |
|------------------|-------------|-----------|---------|---------|----------|----------|
| Intercept        |    64.542   |   0.159   | 406.181 |    0    |   64.23  |  64.853  |
| Parent Education |    4.856    |   0.273   |  17.77  |    0    |   4.32   |   5.392  |

**Table 3.** _This table reports the regression results using the SES Index as a single composite predictor. The SES Index significantly predicted Exam Score and offers a simpler way to summarize socioeconomic advantage, though it sacrifices some explanatory precision compared to the multi-factor model._


---

# Results Summary & Embedded Visualizations

Below are the interactive Plotly visualizations associated with the project. These figures render only on the GitHub Pages site and not inside the main GitHub repository.

---

## Figure 1. Mean Exam Score by Parental Education Level

Figure 1 shows the mean Exam Score for each Parental Education Level, along with 95% confidence intervals. The figure shows a pattern of clear upward trend in average performance when increasing Parental Education Level. Students whose parents completed only high school scored the lowest on average, followed by those with college-educated parents, and students with postgraduate-educated parents achieved the highest mean exam scores. Although the differences are small, the upward pattern suggests that higher parental education is associated with higher academic performance. The confidence intervals around each point are narrow, indicating that the mean estimates are precise given the large sample size. 

<iframe src="mean_exam_score_plot.html" width="800" height="500"></iframe>

**Figure 1.** _Mean Exam Score by Parental Education Level, with 95% confidence intervals representing uncertainty around each mean estimate. Mean exam performance increases gradually across Parental Education Level. Students whose parents completed postgraduate education scored the highest on average, followed by students with college-educated parents, and then by students with high school-educated parents._ 

---

## Figure 2. SES Index vs Exam Score

Figure 2 shows a scatterplot with a regression line of best fit to model the relationship between the composite SES index and exam performance. The figure revealed a clear positive relationship between the SES index and exam performance, reflecting that students with higher SES index values had generally higher exam scores. 

<iframe src="ses_index_vs_exam_score2.html" width="800" height="500"></iframe>

**Figure 2.** _Scatterplot of SES Index predicting Exam Score with line of best fit. A positive correlation can be seen between the SES index and Exam Score, indicating that students with a higher SES index generally had higher Exam Scores._


---

## Conclusion
This project examined whether different factors of socioeconomic status predict exam performance in high school students. Across all three regression models, SES proved to be a meaningful predictor of academic achievement. Model 1 showed that parental education alone was significant but a relatively weak predictor. Model 2 revealed that a multi-factor SES model offered the strongest predictive power, reinforcing the idea that SES is multidimensional. Model 3 applied a composite SES index to summarize multiple SES components into a single measure.  Although Model 3 was slightly less precise than the full multi-factor model, it still effectively captured the overall relationship between SES and exam performance. 

These findings align with my partner’s machine learning analysis, which similarly found that SES variables, while not the strongest predictors in the dataset, meaningfully contribute to exam performance in combination with other factors. Together, our approaches highlight how traditional regression and machine learning modeling can complement each other in understanding the predictors of academic outcomes. 

Several limitations should be considered when interpreting these results. First, because the dataset is observational, the analyses cannot establish causal relationships between SES variables and performance. In addition, the SES indicators were broad and most likely self-reported, which may introduce bias or reduce measurement accuracy. Exam Score also only captures one element of academic achievement, and may not be fully representative of students’ academic abilities or experiences. Finally, the origin and undocumented nature of the Kaggle dataset raise questions about the reliability and generalizability of the data and results. 

Future research could address these limitations by incorporating additional SES indicators, more detailed demographic information, and multiple measures of academic performance. Further work could also explore whether SES predicts non-academic outcomes, such as mental health or school engagement, to determine whether the influence of socioeconomic resources extends beyond academic settings. Lastly, applying the SES index, or a similar composite measure, to other populations or international datasets could help assess the validity and generalizability of these findings across different social and cultural contexts. 



---

## References

Nguyen, T. (2024). *Student Performance Factors* [Dataset]. Kaggle.  
<https://www.kaggle.com/datasets/lainguyn123/student-performance-factors>

---
