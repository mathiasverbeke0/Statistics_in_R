# Tests of independence
## Fisher's exact test of independence
The most common use of Fisher's exact test is for 2×2 tables. Fisher's exact test is more accurate than the chi-square test or G–test of independence when the expected numbers are small. Fisher's exact test is recommended when the total sample size is less than 1000, and the chi-square or G–test for larger sample sizes. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/fishers.html).

### Example

||Cured|Not Cured|
| :---: | :---: | :---: |
|Drug X|45|55|
|Drug Y|19|89|

H0: The two variables are independent.<br>
H1: The two variables are not independent.

### Implementation in R
```r
data <- matrix(c(45, 55, 19, 89), nrow = 2, ncol = 2, byrow = TRUE)
colnames(data) <- c("cured", "not cured")
rownames(data) <- c("drug X", "drug Y")

result <- fisher.test(data)

result$p.value
```

## Chi-square test of independence
Use the chi-square test of independence when you have two nominal variables and you want to see whether the proportions of one variable are different for different values of the other variable. Use it when the sample size is large. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/chiind.html).

### Example
|| No severe reaction | Severe reaction |
|:---:|:---:|:---:|
|Thigh|4758|30|
|Arm|8840|76|

H0: The two variables are independent.<br>
H1: The two variables are not independent.

### Implementation in R
```r
data <- matrix(c(4758, 30, 8840, 76), nrow = 2, ncol = 2, byrow = TRUE)
colnames(data) <- c("nosevere", "severe")
rownames(data) <- c("thigh", "arm")

result <- chisq.test(data, correct = F)

result$p.value
```

## G-test of independence
Use the G–test of independence when you have two nominal variables and you want to see whether the proportions of one variable are different for different values of the other variable. Use it when the sample size is large. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/gtestind.html).

### Example
|| No severe reaction | Severe reaction |
|:---:|:---:|:---:|
|Thigh|4758|30|
|Arm|8840|76|

H0: The two variables are independent.<br>
H1: The two variables are not independent.

### Implementation in R
```r
library(DescTools)

data <- matrix(c(4758, 30, 8840, 76), nrow = 2, ncol = 2, byrow = TRUE)
colnames(data) <- c("nosevere", "severe")
rownames(data) <- c("thigh", "arm")

result <- GTest(data)
result$p.value
```

## Cochran-Mantel-Haenszel test
Use the Cochran–Mantel–Haenszel test when you have data from 2×2 tables that you've repeated at different times or locations (= repeated tests of independence). It will tell you whether you have a consistent difference in proportions across the repeats.

### Example
| Location  | Allele | Marine | Estuarine |
| :-------: | :----: | :----: | :-------: |
| Tillamook | 94     | 56     | 69        |
|           | non-94 | 40     | 77        |
| Yaquina   | 94     | 61     | 257       |
|           | non-94 | 57     | 301       |
| Alsea     | 94     | 73     | 65        |
|           | non-94 | 71     | 79        |
| Umpqua    | 94     | 71     | 48        |
|           | non-94 | 55     | 48        |


H0: The two variables (allele and water) are independent.<br>
H1: The two variables (allele and water) are not independent.

### Implementation in R
```r
study_array <- array(c(56,40,69,77,
                        61,57,257,301,
                        73,71,65,79, 
                        71,55,48,48),
                      dim = c(2, 2, 4), # 2x2 for 4 studies
                      dimnames = list(
                        Allele = c("94", "non-94"),
                        Water = c("Marine", "Estuarine"),
                        Location = c("Tillamook", "Yaquina", "Alsea", "Umpqua")))

result <- mantelhaen.test(study_array)
result$p.value
```



# Tests of goodness-of-fit
## Exact test of goodness-of-fit
You use the exact test of goodness-of-fit when you have one nominal variable, you want to see whether the number of observations in each category fits a theoretical expectation, and the sample size is small. The most common use is a nominal variable with only two values (such as male or female, left or right, green or yellow), in which case the test may be called the exact binomial test. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/exactgof.html).

### Example
|| Homotypic matings | Heterotypic matings |
|:---:|:---:|:---:|
|Observed|140|106|
|Expected frequency|0.5|0.5|

H0: The observed data follow the hypothesized distribution.<br>
H1: The observed data do not follow the hypothesized distribution.

### Implementation in R
```r
result <- binom.test(c(140,106), p = 0.5) # with p the expected probability of homotypic matings
result$p.value
```

## Chi Square test of goodness-of-fit
You use the chi-square test of goodness-of-fit when you have one nominal variable, you want to see whether the number of observations in each category fits a theoretical expectation, and the sample size is large. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/chigof.html).

### Example
#### Test 1 
European crossbills have the tip of the upper bill either on the right or left side of the lower bill. We want to test if the chances of having the tip of the upper bill on the right or on the left are equal. We made the following observations: 1752 right-billed, 1895 left-billed.

#### Test 2
In a forest, the tree species composition is 54% Douglas fir, 40% ponderosa pine, 5% grand fir, and 1% western larch. A total of 156 birds were observed and their nesting locations were recorded as follows: 70 observations (45% of the total) in Douglas fir, 79 (51%) in ponderosa pine, 3 (2%) in grand fir, and 4 (3%) in western larch. We will test if the birds favor a tree (with this test, it will not be clear which tree).

### Implementation in R
#### Test 1
```r
chisq.test(c(1752,1895), correct = FALSE)
```

#### Test 2
```r
chisq.test(c(70,79,3,4), p = c(0.54,0.40,0.05,0.01), correct = FALSE)
```

## G-test of goodness-of-fit
You use the G–test of goodness-of-fit (also known as the likelihood ratio test, the log-likelihood ratio test, or the G2 test) when you have one nominal variable, you want to see whether the number of observations in each category fits a theoretical expectation, and the sample size is large. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/gtestgof.html).

### Example
Refer to the datasets in the Chi Square test of goodness-of-fit

### Implementation in R
#### Test 1
```r
GTest(c(1752,1895))
```

#### Test2
```r
GTest(c(70,79,3,4), p = c(0.54,0.40,0.05,0.01))
```

# T-tests
## One-sample T-test
Use Student's t–test for one sample when you have one measurement variable and a theoretical expectation of what the mean should be under the null hypothesis. It tests whether the mean of the measurement variable is different from the null expectation. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/onesamplettest.html).

### Example
We have a sample of test scores with 20 observations: 71, 73, 72, 68, 70, 69, 72, 70, 74, 70, 71, 75, 70, 69, 72, 73, 70, 72, 71 and 70. 
#### Test 1: Two-sided
We want to test whether the mean test score is significantly different from 70.
H0: μ1 = 70<br>
H1: μ1 ≠ 70

#### Test 2: Single-sided
Now we want to test whether the mean test score is significantly smaller than 70.
H0: μ1 ≥ 70<br>
H1: μ1 < 70

### Implementation in R
#### Test 1
```r
scores <- c(71, 73, 72, 68, 70, 69, 72, 70, 74, 70, 71, 75, 70, 69, 72, 73, 70, 72, 71, 70)
result <- t.test(scores, mu = 70)
restult$p.value
```
#### Test 2
```r 
scores <- c(71, 73, 72, 68, 70, 69, 72, 70, 74, 70, 71, 75, 70, 69, 72, 73, 70, 72, 71, 70)
result <- t.test(scores, mu = 70, alternative = "less")
result$p.value
```

## Two-sample T-test
Use Student's t–test for two samples when you have one measurement variable and one nominal variable, and the nominal variable has only two values. It tests whether the means of the measurement variable are different in the two groups. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/twosamplettest.html).

### Example
| Treatment | Control |
| :---: | :---: |
| 10 | 8 |
| 12 | 7 |
| 14 | 9 |
| 11 | 6 |
| 13 | 10 |

#### Test 1: Two-sided
We want to test whether or not the means of the two groups are the same.
H0: μT = μC<br>
H1: μT ≠ μC

#### Test 2: Single-sided
Now we want to test if the mean of the treatment group is greater than the mean of the control group.
H0: μT ≤ μC<br>
H1: μT > μC

### Implementation in R
#### Variance check
First check whether the variances are equal or not with the F-test.
H0: σ_1^2 = σ_2^2<br>
H1: σ_1^2 ≠ σ_2^2

```r
treatment <- c(10, 12, 14, 11, 13)
control <- c(8, 7, 9, 6, 10)
result <- var.test(treatment, control, alternative = "two.sided")
result$p.value
```
Set the var.equal option to F if the p.value is below the threshold, otherwise set it to T.

#### Test 1
```r
treatment <- c(10, 12, 14, 11, 13)
control <- c(8, 7, 9, 6, 10)
result <- t.test(treatment, control, var.equal = T, paired = F)
result$p.value
```
#### Test 2
```r 
treatment <- c(10, 12, 14, 11, 13)
control <- c(8, 7, 9, 6, 10)
result <- t.test(treatment, control, var.equal = T, paired = F, alternative = "greater") # Note that the treatment variable must be given first in this case, because you want to test if the mean of the treatment group is greater than the mean of the control group!
result$p.value
```

## Paired T-test
Use the paired t–test when you have one measurement variable and two nominal variables, one of the nominal variables has only two values, and you only have one observation for each combination of the nominal variables; in other words, you have multiple pairs of observations. It tests whether the mean difference in the pairs is different from 0. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/pairedttest.html).

### Example
| Bird | Yellowness index (typical feather)| Yellowness index (odd feather)|
|:---:|:---:|:---:|
| A    | -0.255          | -0.324          |
| B    | -0.213          | -0.185          |
| C    | -0.19           | -0.299          |
| D    | -0.185          | -0.144          |
| E    | -0.045          | -0.027          |
| F    | -0.025          | -0.039          |
| G    | -0.015          | -0.264          |
| H    | 0.003           | -0.077          |
| I    | 0.015           | -0.017          |
| J    | 0.02            | -0.169          |
| K    | 0.023           | -0.096          |
| L    | 0.04            | -0.33           |
| M    | 0.04            | -0.346          |
| N    | 0.05            | -0.191          |
| O    | 0.055           | -0.128          |
| P    | 0.058           | -0.182          |

H0: μ1 = μ2<br>
H1: μ1 ≠ μ2

NOTE: You can also do single-sided tests here. It is performed in the same way as mentioned in the Two-sample T-test section. 

### Implementation in R
```r 
Typical <- c(-0.255, -0.213, -0.19, -0.185, -0.045, -0.025, -0.015, 0.003, 0.015, 0.02, 0.023, 0.04, 0.04, 0.05, 0.055, 0.058)
Odd <- c(-0.324, -0.185, -0.299, -0.144, -0.027, -0.039, -0.264, -0.077, -0.017, -0.169, -0.096, -0.33, -0.346, -0.191, -0.128, -0.182)
control <- c(8, 7, 9, 6, 10)
result <- t.test(Typical, Odd, paired = TRUE)
result$p.value
```

## Wilcoxon signed-rank test
Use the Wilcoxon signed-rank test when you'd like to use the paired t–test, but the differences are severely non-normally distributed. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/wilcoxonsignedrank.html).

### Example
| Clone         | August | November | August−November |
|---------------|--------|----------|----------------|
| Columbia River | 18.3   | 12.7     | -5.6           |
| Fritzi Pauley  | 13.3   | 11.1     | -2.2           |
| Hazendans      | 16.5   | 15.3     | -1.2           |
| Primo          | 12.6   | 12.7     | 0.1            |
| Raspalje       | 9.5    | 10.5     | 1.0            |
| Hoogvorst      | 13.6   | 15.6     | 2.0            |
| Balsam Spire   | 8.1    | 11.2     | 3.1            |
| Gibecq         | 8.9    | 14.2     | 5.3            |
| Beaupre        | 10.0   | 16.3     | 6.3            |
| Unal           | 8.3    | 15.5     | 7.2            |
| Trichobel      | 7.9    | 19.9     | 12.0           |
| Gaver          | 8.1    | 20.4     | 12.3           |
| Wolterson      | 13.4   | 36.8     | 23.4           |

### Implementation in R
```r
August <- c(18.3, 13.3, 16.5, 12.6, 9.5, 13.6, 8.1, 8.9, 10.0, 8.3, 7.9, 8.1, 13.4)
November <- c(12.7, 11.1, 15.3, 12.7, 10.5, 15.6, 11.2, 14.2, 16.3, 15.5, 19.9, 20.4, 36.8)
August.November <- November - August

densAugust <- density(August)
densNovember <- density(November)
densAugust.November <- density(August.November)

# Visualize whether or not the data is normally distributed
par(mfrow = c(2,2))

hist(x = August, prob = T)
lines(distAugust)

hist(x = November, prob = T)
lines(distNovember)

hist(x = August.November, prob = T)
lines(August.November)

boxplot(August, November, names = c('August', 'November'), main = 'Boxplot')

# Statistical test
wilcox.test(August, November, paired = TRUE)
```

# ANOVA
## One-way ANOVA
Use one-way anova when you have one nominal variable and one measurement variable; the nominal variable divides the measurements into two or more groups. It tests whether the means of the measurement variable are the same for the different groups. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/onewayanova.html).

### Example
|   Weight | Group |
| :------: | :---: |
|     4.17 | ctrl  |
|     5.58 | ctrl  |
|     5.18 | ctrl  |
|     6.11 | ctrl  |
|     4.50 | ctrl  |
|     4.61 | ctrl  |
|     5.17 | ctrl  |
|     4.53 | ctrl  |
|     5.33 | ctrl  |
|     5.14 | ctrl  |
|     4.81 | trt1  |
|     4.17 | trt1  |
|     4.41 | trt1  |
|     3.59 | trt1  |
|     5.87 | trt1  |
|     3.83 | trt1  |
|     6.03 | trt1  |
|     4.89 | trt1  |
|     4.32 | trt1  |
|     4.69 | trt1  |
|     6.31 | trt2  |
|     5.12 | trt2  |
|     5.54 | trt2  |
|     5.50 | trt2  |
|     5.37 | trt2  |
|     5.29 | trt2  |
|     4.92 | trt2  |
|     6.15 | trt2  |
|     5.80 | trt2  |
|     5.26 | trt2  |

H0: The means of the measurement variable are the same among the different groups.<br>
H1: The means of the measurement variable are not the same among the different groups.

### Implementation in R
```r
result <- anova(lm(weight ~ group, data = PlantGrowth))
```

## Tukey-Kramer test
Use the Tukey-Kramer test when you have rejected H0 of the one-way ANOVA to look at the data in more detail. 

### Example
Refer to the dataset in the one-way ANOVA section.

### Implemnetation in R
```r
result <- aov(weight ~ group, data = PlantGrowth)
TukeyHSD(result)
```

## Two-way ANOVA
Use two-way anova when you have one measurement variable and two nominal variables, and each value of one nominal variable is found in combination with each value of the other nominal variable. It tests three null hypotheses: that the means of the measurement variable are equal for different values of the first nominal variable; that the means are equal for different values of the second nominal variable; and that there is no interaction (the effects of one nominal variable don't depend on the value of the other nominal variable). For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/twowayanova.html).

### Example
| Diet    | Temperature | Weight gain |
| :-----: | :---------:| :---------:|
| A       | High       | 2.1        |
| A       | High       | 1.8        |
| A       | High       | 1.9        |
| A       | Low        | 1.2        |
| A       | Low        | 1.4        |
| A       | Low        | 1.1        |
| B       | High       | 2.5        |
| B       | High       | 2.3        |
| B       | High       | 2.6        |
| B       | Low        | 1.8        |
| B       | Low        | 2.0        |
| B       | Low        | 1.7        |
| C       | High       | 2.0        |
| C       | High       | 1.9        |
| C       | High       | 2.2        |
| C       | Low        | 1.5        |
| C       | Low        | 1.7        |
| C       | Low        | 1.3        |

The null hypothesis for the main effect of Factor A (Diet) is that there is no significant difference in the mean weight gain across the three diet groups. The alternative hypothesis is that there is a significant difference in the mean weight gain across the three diet groups.

The null hypothesis for the main effect of Factor B (Temperature) is that there is no significant difference in the mean weight gain across the two temperature levels. The alternative hypothesis is that there is a significant difference in the mean weight gain across the two temperature levels.

The null hypothesis for the interaction effect between Factor A and Factor B is that there is no significant difference in the mean weight gain between the different combinations of Diet and Temperature. The alternative hypothesis is that there is a significant difference in the mean weight gain between at least one combination of Diet and Temperature.

### Implementation in R
```r
df <- data.frame(
  Diet = c("A", "A", "A", "A", "A", "A", "B", "B", "B", "B", "B", "B", "C", "C", "C", "C", "C", "C"),
  Temperature = c("High", "High", "High", "Low", "Low", "Low", "High", "High", "High", "Low", "Low", "Low", "High", "High", "High", "Low", "Low", "Low"),
  Weight_gain = c(2.1, 1.8, 1.9, 1.2, 1.4, 1.1, 2.5, 2.3, 2.6, 1.8, 2.0, 1.7, 2.0, 1.9, 2.2, 1.5, 1.7, 1.3)
)

boxplot(Weight_gain ~ Diet*Temperature,
        data = df,
        xlab = "Diet x Temperature",
        ylab = "Weight gain")


df.model = lm(Weight_gain ~ Diet*Temperature, 
                     data = df)

anova(df.model)
```

## Nested ANOVA
Use nested anova when you have one measurement variable and more than one nominal variable, and the nominal variables are nested (form subgroups within groups). It tests whether there is significant variation in means among groups, among subgroups within groups, etc. For more information, see [the handbook of biological statistics](http://www.biostathandbook.com/nestedanova.html).


|Technician|Brad	|Brad	|Brad	| Janet| Janet| Janet|
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
|Rat |Arnold|	Ben|	Charlie|	Dave|	Eddy|	Frank|
| 	|1.119|	1.045|	0.9873|	1.3883|	1.3952|	1.2574|	
| 	|1.2996|	1.1418|	0.9873|	1.104|	0.9714|	1.0295	|
| 	|1.5407|	1.2569|	0.8714|	1.1581|	1.3972|	1.1941	|
| 	|1.5084|	0.6191|	0.9452|	1.319|	1.5369|	1.0759	|
| 	|1.6181|	1.4823|	1.1186|	1.1803|	1.3727|	1.3249	|
| 	|1.5962|	0.8991|	1.2909|	0.8738|	1.2909|	0.9494	|
| 	|1.2617|	0.8365|	1.1502|	1.387|	1.1874|	1.1041	|
| 	|1.2288|	1.2898|	1.1635|	1.301|	1.1374|	1.1575	|
| 	|1.3471|	1.1821|	1.151|1.3925	|1.0647|	1.294|	
| 	|1.0206|	0.9177|	0.9367|	1.0832|	0.9486|	1.4543|

Not further discussed.


# Ranking
## Kruskal–Wallis test
Use the Kruskal–Wallis test when you have one nominal variable and one ranked variable. It tests whether the mean ranks are the same in all the groups.

## Example
|Dog | Sex | Rank|
|:---: | :---: | :---:|
|Merlino | Male | 1|
|Gastone | Male | 2|
|Pippo | Male | 3|
|Leon | Male | 4|
|Golia | Male | 5|
|Lancillotto | Male | 6|
|Mamy | Female | 7|
|Nanà | Female | 8|
|Isotta | Female | 9|
|Diana | Female | 10|
|Simba | Male | 11|
|Pongo | Male | 12|
|Semola | Male | 13|
|Kimba | Male | 14|
|Morgana | Female | 15|
|Stella | Female | 16|
|Hansel | Male | 17|
|Cucciola | Male | 18|
|Mammolo | Male | 19|
|Dotto | Male | 20|
|Gongolo | Male | 21|
|Gretel | Female | 22|
|Brontolo | Female | 23|
|Eolo | Female | 24|
|Mag | Female | 25|
|Emy | Female | 26|
|Pisola | Female | 27|

H0: The mean ranks of the groups are the same (i.e. there is no correlation between the rank and the sex).<br>
H1: The mean ranks of the groups are not the same (i.e. there is a correlation between the rank and the sex). 
### Implementation in R
```r
data <-  data.frame(
  sex = c("Male", "Male", "Male", "Male", "Male", "Male", "Female", "Female", "Female", "Female", "Male", "Male", "Male", "Male", "Female", "Female", "Male", "Male", "Male", "Male", "Male", "Female", "Female", "Female", "Female", "Female", "Female"),
  rank = c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27)

)

kruskal.test(rank ~ sex, data = data)
```

# Homoscedasticity and heteroscedasticity
## Bartlett’s test
Use Bartlett's test if you have one measurement variable and one nominal variable and want to test if the variances of the measuremants variable are the same amoung the different groups. 

### Example
|   Weight | Group |
| :------: | :---: |
|     4.17 | ctrl  |
|     5.58 | ctrl  |
|     5.18 | ctrl  |
|     6.11 | ctrl  |
|     4.50 | ctrl  |
|     4.61 | ctrl  |
|     5.17 | ctrl  |
|     4.53 | ctrl  |
|     5.33 | ctrl  |
|     5.14 | ctrl  |
|     4.81 | trt1  |
|     4.17 | trt1  |
|     4.41 | trt1  |
|     3.59 | trt1  |
|     5.87 | trt1  |
|     3.83 | trt1  |
|     6.03 | trt1  |
|     4.89 | trt1  |
|     4.32 | trt1  |
|     4.69 | trt1  |
|     6.31 | trt2  |
|     5.12 | trt2  |
|     5.54 | trt2  |
|     5.50 | trt2  |
|     5.37 | trt2  |
|     5.29 | trt2  |
|     4.92 | trt2  |
|     6.15 | trt2  |
|     5.80 | trt2  |
|     5.26 | trt2  |

H0: The variances of the measurement variable are the same among the different groups.<br>
H1: The variances of the measurement variable are not the same among the different groups.

### Implementation in R
```r
bartlett.test(weight ~ group, data = PlantGrowth)
```