---
layout: post
title: Short post: How to assign within a loop
comments: true
---


You probably heard that you shouldn't use loops in R a thousand times by now, but there are some times you can't avoid it. I recently *had* to implement a loop to generate a new variable in a dataset; the problem, how do I create the new variable and then put it back in a data.frame? Use `assign`

code example:

{% highlight r %}

# create an empty dataset
df <- data.frame(obs = 1:10)

# create a list of values (strings) to iterate through
clist <- list("a", "b", "c")

# here comes the loop
for (i in clist) {
  # in each loop assign the values to a variable matching
  # the string corresponding to the actual iteration
  assign(i, rnorm(10))

# use mget to put the vector back in the dataframe
df <- data.frame(df, mget(i))
}

{% enhighlight %}

There must be a million ways of doing this in a more efficient way, but this works great for a smallish dataset.

The result:

    df
       obs          a           b           c
    1    1 -0.6043081 -0.55737494  0.55271123
    2    2  1.7190841  0.76963270 -1.20019177
    3    3 -0.6397346 -1.01280031 -0.88938821
    4    4  1.2044493  0.53935314  0.98288657
    5    5  1.5952527  1.37572210  0.18416401
    6    6  1.6863256  1.57793910  0.52359373
    7    7  0.4624379  1.97033926  0.09766545
    8    8 -0.3982681 -2.72841091 -0.91980220
    9    9  1.3184167 -1.48644095 -0.07343118
    10  10  1.2830817  0.05275999  1.92037378


Have a great weekend!