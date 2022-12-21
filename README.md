# Mystery-College

## Libraries Used

```{r library}
library(tidyverse)
```

## Data Read

```{r data_read}
colleges <- read_csv("https://www.dropbox.com/s/bt5hvctdevhbq6j/colleges.csv?dl=1")
```

## Data Cleaning and Transformations

```{r clean, warning=FALSE}
colleges_clean <- colleges %>%
  select(INSTNM, CITY, STABBR, ZIP, CONTROL, ADM_RATE, SAT_AVG, TUITIONFEE_IN, TUITIONFEE_OUT, UGDS, REGION) 

colleges_clean <- colleges_clean %>%
  filter(CONTROL == 2) 

colleges_clean <- colleges_clean %>%
  mutate(
    TUITIONFEE_IN = as.numeric(TUITIONFEE_IN),
    TUITIONFEE_OUT = as.numeric(TUITIONFEE_OUT),
    SAT_AVG = as.numeric(SAT_AVG),
    ADM_RATE = as.numeric(ADM_RATE)
    ) 

colleges_clean <- colleges_clean %>% 
  mutate(
    CONTROL = as.factor(CONTROL),
    REGION = as.factor(REGION)
    )

colleges_clean <- colleges_clean %>% 
    mutate(TUITION_DIFF = TUITIONFEE_OUT - TUITIONFEE_IN)

colleges_clean <- colleges_clean %>% drop_na()

```

## Mystery College Output

```{r mystery_college}
mystery_college <- colleges_clean %>%
  filter(REGION==1) %>%
  filter(ADM_RATE <= quantile(ADM_RATE, 0.25)) %>%
  filter(TUITION_DIFF==0)%>%
  filter(SAT_AVG %% 2 !=0) %>%
  filter(CITY != 'Boston') %>%
  filter(STABBR != 'NH') %>%
  filter(UGDS >= 4*(3000*ADM_RATE)) %>%
  filter(INSTNM != 'Harvard University', INSTNM != 'The Universtiy of North Carolina at Chapel Hill')

head(mystery_college)
```
