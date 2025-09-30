---
title: "Record-Breaking Record Breaking: How often should we expect to see record-breaking temperatures?"
---

## Tutorial Objectives and Summary

In this tutorial, we will go over how to use simulation to corroborate statistical conjectures/hypotheses. We will learn how to use loops and random number generation in python in order to produce usable example data from which we can draw conclusions.

## Topic of study

Have you ever looked at the weather report and seen that today is the hottest (or coldest) recorded temperature for that day, for a specific city? Since there are 365 days of the year, and thousands of cities that measure weather data, we shouldn't be surprised when these records are broken. However, the question arises: How _often_ should we expect these records to be broken? If we had record breaking high or low temperature every day for a whole year, that would clearly be cause for concern. In addition, does us having 30 years of historical data vs 100 make a difference in how often we should see records being broken? We can try to work this out mathematically, and then check our work using simulation!

![](icicles.jpg){width="750"}

## Calculating the expected record breaking, mathematically

For any given day of the year, we should expect the high and low temperature of that day to fall within certainly logical bounds. In Provo, for example, it would not make sense (and would be very concerning) for us to see a high temperature of 20F in the middle of July. We also would not expect the low temperature to be 100F in December! Despite the fact that someone like you or I cannot predict the exact temperature of a given day, it makes sense to assume that the high/low temperature of a given day would follow a normal distribution, or something similar to that.

### Starting from the normal distribution

For the sake of thinking through the problem mathematically, let's say we're talking about a specific day in Provo, Utah, such as September 27th. This year, the high was 84 degrees Fahrenheit. If we assume that a collection of all historical high temperatures on September 29th in Provo, Utah follow a normal distribution, then there is some inherent mean and standard deviation that this data will fall into. Fortunately for our math, the exact values of the mean and standard deviation don't matter! This is because if we assume every data point comes from this same distribution, a value that is two standard deviations above the mean will always be larger than a value that is only one standard deviation above the mean, no matter what those values actually are.

### Generating the expected frequency from assumptions about historical data

As it turns out, doing the initial calculations of frequency become extremely easy when we make the assumption that each day's high temperature has its own inherent distribution. For a given city, let's say we have 50 years of historical data. That means, that for a specific day, we will have 50 daily high temperatures from which we would determine if this year's was the highest. If all 50 come from the exact same distribution, then we can say that each of the 50 data points is equally as likely to be the highest! Without doing any complex math at all, we have a hypothesis that with 50 years of historical data, we would expect 1/50 days this year to have a record-breaking high temperature!

## Creating a simulation

The heart and soul of any statistical simulation is repetition! We want to create code that can be run hundreds of times, recording our simulated data for every iteration. The code below shows how we can simulate a random number pulled from a normal distribution with mean 0 and standard deviation 1:

```{python}
newtemp = np.random.normal(0,1)
```

Next, let's write a loop around this to run this 50 times, adding in a variable that dynamically stores the highest number generated so far:

```{python}
highest_temp = -100
for i in range(50):
        newtemp = np.random.normal(0,1)
        if newtemp > highest_temp:
            highest_temp = newtemp
```

Remember, we're trying to determine if a given temperature was a record, so let's initialize a list, and append a 1 for every year that broke the highest temperature record and a 0 for every year that didn't

```{python}
temp_record = []
highest_temp = -100
for i in range(50):
        newtemp = np.random.normal(0,1)
        if newtemp > highest_temp:
            temp_record.append(1)
            highest_temp = newtemp
        else:
            temp_record.append(0)
```

Now, our resulting temp_record list gives us a sequence of 1's and 0's representing when we saw record breaking temperatures in our simulation. However, we still haven't reached a point where we can make any kind of conclusions about frequency, since the most recent year is only represented once! If we were to run this same chunk of code 10,000 times, and record the results from each one of these 10,000 50-year periods, we could see how frequent records would be each year. In order to keep these lists of 50 in order and calculate the frequency for each year, we will make use of the numpy package, which makes array math much easier to do:

```{python}
import numpy as np

#initialize an array for summing the quantity of records over time
num_record = np.zeros(50)

for j in range(10000):
    temp_record = []
    highest_temp = -100
    for i in range(50):
        newtemp = np.random.normal(0,1)
        if newtemp > highest_temp:
            temp_record.append(1)
            highest_temp = newtemp
        else:
            temp_record.append(0)
    num_record += np.array(temp_record)

#divide the totals by 10000 to get the proportion of records that broke the record
freq_record = num_record / 10000
print(freq_record)
```

Run this code on your own machine! You'll see that the last value in freq_record is pretty close to 0.02, which is what we expected it to be! Since we have data for each of the other years, we can also see that our mathematical predictions line up almost exactly with the results of our simulation. At the 10-year mark, we should see a frequency close to 0.10, and at the 25 year mark, we should see a frequency close to 0.04.

## Conclusion

In conclusion, creating a simple simulation can be very helpful in providing evidence towards a hypothesis. We were able to use our assumptions to create a simulation in python that reflected our knowledge of our subject matter. I would now like you to do one of two things:

1\) Use our results as the null hypothesis in an observational study of the historical data of your own hometown! Look up how many high (or low) temperature records were broken in the last year, and see if that number is statistically significantly different from our hypothetical number.

2\) Create your own simulation to model some aspect of your life you would like to investigate! The most comfortable you become in creating simulations, the better able you will be to put your hypotheses to the test.