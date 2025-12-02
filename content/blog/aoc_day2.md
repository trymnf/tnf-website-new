+++
title = "Advent of Code Using Tidyverse"
date = "2025-12-02T10:49:39+01:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["advent of code", "R"]
+++

When I find the time, I like to try solving the [Advent of
Code](https://adventofcode.com/2025/) puzzles. I prefer to take a "tidy," data-like approach wherever possible (because that's all I know). Let's see how
far that gets me this year.

[Day 2](https://adventofcode.com/2025/day/2) asks us to find numbers that contain patterns (like `99` or `1188511885`) within ranges of numbers. This task is *very* friendly to a tidyverse approach. We need to do some data wrangling to get everything in the right format, then it's just a matter of a simple regex to find what we need.  

The `separate` functions from tidyr and a little regex made today's solution clean and straightforward.


```R
library(tidyverse)

input <- "11-22,95-115,998-1012,1188511880-1188511890,222220-222224,1698522-1698528,446443-446449,38593856-38593862,565653-565659,824824821-824824827,2121212118-2121212124"

data <- tibble(input = input) |>
  separate_longer_delim(input, ",") |>
  separate(input, into = c("start", "end"), sep = "-") |>
  rowwise() |>
  mutate(range = list(start:end)) |>
  unnest_longer(range)

data
```

    # A tibble: 106 × 3
       start end   range
       <chr> <chr> <int>
     1 11    22       11
     2 11    22       12
     3 11    22       13
     4 11    22       14
     5 11    22       15
     6 11    22       16
     7 11    22       17
     8 11    22       18
     9 11    22       19
    10 11    22       20
    # ℹ 96 more rows

`range` here gives us all the values in all the ranges. Now it's just a matter of finding the repetitions using regex, and adding them all together. 


```R
 data |>
  filter(str_detect(range, "^([0-9]+)\\1$")) |>
  summarise(sum(range))
```


    # A tibble: 1 × 1
      `sum(range)`
             <int>
    1   1227775554


For part 2, all I needed to add was a `+` to
the regex to allow for more than one repetition of the pattern.

```R
# Part 2

tibble(input = input) |>
  separate_longer_delim(input, ",") |>
  separate(input, into = c("start", "end"), sep = "-") |>
  rowwise() |>
  mutate(range = list(start:end)) |>
  unnest_longer(range) |>
  # adding a plus here for unlimited repeats
  filter(str_detect(range, "^([0-9]+)\\1+$")) |>
  summarise(sum(range))
```


    # A tibble: 1 × 1
      `sum(range)`
             <dbl>
    1   4174379265
