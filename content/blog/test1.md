---
title: "COVID-19 Cases in Spain"
author: "Helen Evangelina"
date: "31/08/2020"
categories: ["R"]
tags: ["R Markdown", "plot", "regression"]
output: 
  blogdown::html_page:
    fig_width: 6
    dev: "svg"
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

```{r loading-packages}
#installing package
#devtools::install_github("joachim-gassen/tidycovid19")
library(tidyverse)
library(tidycovid19)
library(zoo)
library(ggplot2)
library(kableExtra)
library(DT)
library(bookdown)
library(blogdown)
```

```{r loading-data-spain}
covid_data_spain <- download_merged_data(cached = TRUE, silent = TRUE) %>%
  filter(country == "Spain")
```

## Data Description
The data is taken from https://github.com/joachim-gassen/tidycovid19

## Data Story
```{r wrangling-data}
data_wrangled <- covid_data_spain %>% 
  mutate(confirmed_lag = lag(confirmed),
         daily_confirmed = confirmed - confirmed_lag,
         death_lag = lag(deaths),
         daily_death = deaths - death_lag,
         recovered_lag = lag(recovered),
         daily_recovered = recovered - recovered_lag)
```

```{r}
data_table1 <- data_wrangled %>%
  select(date, confirmed, deaths, recovered, daily_confirmed, daily_death, daily_recovered)
```

```{r table1, fig.cap="Table 1"}
DT::datatable(data_table1, options = list(pageLength = 4))
```
The table shown in Table \@ref(table1).

<div class="bg-white border-box" style="position:absolute;right:50px;bottom:50px;width:700px;padding:10px;">