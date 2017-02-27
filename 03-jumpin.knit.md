# Jump In! {#jumpin}


```
## Loading required package: jpeg
```

```
## Loading required package: BayesFactor
```

```
## Loading required package: coda
```

```
## Loading required package: Matrix
```

```
## ************
## Welcome to BayesFactor 0.9.12-2. If you have questions, please contact Richard Morey (richarddmorey@gmail.com).
## 
## Type BFManual() to open the manual.
## ************
```

```
## yarrr v0.1.4. Citation info at citation('yarrr'). Package guide at yarrr.guide()
```

```
## Email me at Nathaniel.D.Phillips.is@gmail.com
```



<div class="figure" style="text-align: center">
<img src="images/pirateswimming.jpg" alt="Despite what you might find at family friendly waterparks -- this is NOT how real pirate swimming lessons look." width="75%" />
<p class="caption">(\#fig:unnamed-chunk-2)Despite what you might find at family friendly waterparks -- this is NOT how real pirate swimming lessons look.</p>
</div>


What's the first exercise on the first day of pirate swimming lessons? While it would be cute if they all had little inflatable pirate ships to swim around in -- unfortunately this is not the case. Instead, those baby pirates take a walk off their baby planks so they can get a taste of what they're in for. Turns out, learning R is the same way. Let's jump in. In this chapter, you'll see how easy it is to calculate basic statistics and create plots in R. Don't worry if the code you're running doesn't make immediate sense -- just marvel at how easy it is to do this in R!

In this section, we'll analyze a dataset called...wait for it...pirates! The dataset contains data from a survey of 1,000 pirates. The data is contained in the `yarrr` package, so make sure you've installed and loaded the package:


```r
# Install the yarrr package
install.packages('yarrr')

# Load the package
library(yarrr)
```

## Exploring data

Next, we'll look at the help menu for the pirates dataset using the question mark `?pirates`. When you run this, you should see a small help window open up in RStudio that gives you some information about the dataset.


```r
?pirates
```


First, let's take a look at the first few rows of the dataset using the `head()` function. This will show you the first few rows of the data.


```r
# Look at the first few rows of the data
head(pirates)
##   id    sex age height weight headband college tattoos tchests parrots
## 1  1   male  28 173.11   70.5      yes   JSSFP       9       0       0
## 2  2   male  31 209.25  105.6      yes   JSSFP       9      11       0
## 3  3   male  26 169.95   77.1      yes    CCCC      10      10       1
## 4  4 female  31 144.29   58.5       no   JSSFP       2       0       2
## 5  5 female  41 157.85   58.4      yes   JSSFP       9       6       4
## 6  6   male  26 190.20   85.4      yes    CCCC       7      19       0
##   favorite.pirate sword.type eyepatch sword.time beard.length
## 1    Jack Sparrow    cutlass        1       0.58           16
## 2    Jack Sparrow    cutlass        0       1.11           21
## 3    Jack Sparrow    cutlass        1       1.44           19
## 4    Jack Sparrow   scimitar        1      36.11            2
## 5            Hook    cutlass        1       0.11            0
## 6    Jack Sparrow    cutlass        1       0.59           17
##             fav.pixar grogg
## 1      Monsters, Inc.    11
## 2              WALL-E     9
## 3          Inside Out     7
## 4          Inside Out     9
## 5          Inside Out    14
## 6 Monsters University     7
```

You can look at the names of the columns in the dataset with the `names()` function


```r
# What are the names of the columns?
names(pirates)
##  [1] "id"              "sex"             "age"            
##  [4] "height"          "weight"          "headband"       
##  [7] "college"         "tattoos"         "tchests"        
## [10] "parrots"         "favorite.pirate" "sword.type"     
## [13] "eyepatch"        "sword.time"      "beard.length"   
## [16] "fav.pixar"       "grogg"
```

Finally, you can also view the entire dataset in a separate window using the `View()` function:


```r
# View the entire dataset in a new window
View(pirates)
```


## Descriptive statistics

Now let's calculate some basic statistics on the entire dataset. We'll calculate the mean age, maximum height, and number of pirates of each sex:


```r
# What is the mean age?
mean(pirates$age)
## [1] 27.36

# What was the tallest pirate?
max(pirates$height)
## [1] 209.25

# How many pirates are there of each sex?
table(pirates$sex)
## 
## female   male  other 
##    464    490     46
```

Now, let's calculate statistics for different groups of pirates. For example, the following code will use the `aggregate()` function to calculate the mean age of pirates, separately for each sex.


```r
# Calculate the mean age, separately for each sex
aggregate(formula = age ~ sex,
          data = pirates,
          FUN = mean)
##      sex      age
## 1 female 29.92241
## 2   male 24.96735
## 3  other 27.00000
```


## Plotting

Cool stuff, now let's make a plot! We'll plot the relationship between pirate's height and weight using the `plot()` function


```r
# Create scatterplot
plot(x = pirates$height,        # X coordinates
     y = pirates$weight)        # y-coordinates
```

<img src="03-jumpin_files/figure-html/unnamed-chunk-10-1.png" width="672" />

Now let's make a fancier version of the same plot by adding some customization 


```r
# Create scatterplot
plot(x = pirates$height,        # X coordinates
     y = pirates$weight,        # y-coordinates
     main = 'My first scatterplot of pirate data!',
     xlab = 'Height (in cm)',   # x-axis label
     ylab = 'Weight (in kg)',   # y-axis label
     pch = 16,                  # Filled circles
     col = gray(.0, .1))        # Transparent gray
```

<img src="03-jumpin_files/figure-html/unnamed-chunk-11-1.png" width="672" />

Now let's make it even better by adding gridlines and a blue regression line to measure the strength of the relationship.


```r
# Create scatterplot
plot(x = pirates$height,        # X coordinates
     y = pirates$weight,        # y-coordinates
     main = 'My first scatterplot of pirate data!',
     xlab = 'Height (in cm)',   # x-axis label
     ylab = 'Weight (in kg)',   # y-axis label
     pch = 16,                  # Filled circles
     col = gray(.0, .1))        # Transparent gray

grid()        # Add gridlines

# Create a linear regression model
model <- lm(formula = weight ~ height, 
            data = pirates)

abline(model, col = 'blue')      # Add regression to plot
```

<img src="03-jumpin_files/figure-html/unnamed-chunk-12-1.png" width="672" />


Scatterplots are great for showing the relationship between two continuous variables, but what if your independent variable is not continuous? In this case, pirateplots are a good option. Let's create a pirateplot using the `pirateplot()` function to show the distribution of pirate's age based on their favorite sword:


```r
pirateplot(formula = age ~ sword.type, 
           data = pirates,
           main = "Pirateplot of ages by favorite sword")
```

<img src="03-jumpin_files/figure-html/unnamed-chunk-13-1.png" width="672" />

Now let's make another pirateplot showing the relationship between sex and height using a different plotting theme and the `"pony"` color palette:


```r
pirateplot(formula = height ~ sex,               # Plot weight as a function of sex
           data = pirates,                       
           main = "Pirateplot of height by sex",
           pal = "pony",                         # Use the info color palette
           theme = 3)                            # Use theme 3
```

<img src="03-jumpin_files/figure-html/unnamed-chunk-14-1.png" width="672" />

The `"pony"` palette is contained in the `piratepal()` function. Let's see where the `"pony"` palette comes from...


```r
# Show me the pony palette!
piratepal(palette = "pony",
          plot.result = TRUE,   # Plot the result
          trans = .1)           # Slightly transparent
```

<img src="03-jumpin_files/figure-html/unnamed-chunk-15-1.png" width="672" />


## Hypothesis tests

Now, let's do some basic hypothesis tests. First, let's conduct a two-sample t-test to see if there is a significant difference between the ages of pirates who do wear a headband, and those who do not:


```r
# Age by headband t-test
t.test(formula = age ~ headband,
       data = pirates,
       alternative = 'two.sided')
## 
## 	Welch Two Sample t-test
## 
## data:  age by headband
## t = 0.35135, df = 135.47, p-value = 0.7259
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -1.030754  1.476126
## sample estimates:
##  mean in group no mean in group yes 
##          27.55752          27.33484
```

With a p-value of 0.7259, we don't have sufficient evidence say there is a difference in the men age of pirates who wear headbands and those that do not.

Next, let's test if there a significant correlation between a pirate's height and weight using the `cor.test()` function:


```r
cor.test(formula = ~ height + weight,
         data = pirates)
## 
## 	Pearson's product-moment correlation
## 
## data:  height and weight
## t = 81.161, df = 998, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.9232371 0.9396050
## sample estimates:
##       cor 
## 0.9318938
```

We got a p-value of `p < 2.2e-16`, that's scientific notation for `p < .00000000000000016` -- which is pretty much 0. Thus, we'd conclude that there is a significant (positive) relationship between a pirate's height and weight.

Now, let's do an ANOVA testing if there is a difference between the number of tattoos pirates have based on their favorite sword


```r
# Create tattoos model
tat.sword.lm <- lm(formula = tattoos ~ sword.type,
                   data = pirates)

# Get ANOVA table
anova(tat.sword.lm)
## Analysis of Variance Table
## 
## Response: tattoos
##             Df Sum Sq Mean Sq F value    Pr(>F)    
## sword.type   3 1587.8  529.28  54.106 < 2.2e-16 ***
## Residuals  996 9743.1    9.78                      
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Sure enough, we see another very small p-value of `p < 2.2e-16`, suggesting that the number of tattoos pirate's have are different based on their favorite sword.

## Regression

Finally, let's run a regression analysis to see if a pirate's age, weight, and number of tattoos (s)he has predicts how many treasure chests he/she's found:


```r
# Create a linear regression model: DV = tchests, IV = age, weight, tattoos
tchests.model <- lm(formula = tchests ~ age + weight + tattoos,
                    data = pirates)

# Show summary statistics
summary(tchests.model)
## 
## Call:
## lm(formula = tchests ~ age + weight + tattoos, data = pirates)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -33.302 -15.832  -6.860   8.407 119.966 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  5.19084    7.18437   0.723     0.47    
## age          0.78177    0.13438   5.818 8.03e-09 ***
## weight      -0.09013    0.07183  -1.255     0.21    
## tattoos      0.25398    0.22550   1.126     0.26    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 23.99 on 996 degrees of freedom
## Multiple R-squared:  0.04056,	Adjusted R-squared:  0.03767 
## F-statistic: 14.04 on 3 and 996 DF,  p-value: 5.751e-09
```

It looks like the only significant predictor of the number of treasure chests that a pirate has found is his/her age. There does not seem to be significant effect of weight or tattoos.

## Bayesian Statistics

Now, let's repeat some of our previous analyses with Bayesian versions. First we'll install and load the \texttt{BayesFactor} package which contains the Bayesian statistics functions we'll use:


```r
# Install and load the BayesFactor package
install.packages('BayesFactor')
library(BayesFactor)
```

Now that the packages is installed and loaded, we're good to go! Let's do a Bayesian version of our earlier t-test asking if pirates who wear a headband are older or younger than those who do not.



