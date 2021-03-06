Mean Hatecrime Rates by US Region
================
Theodore Dounias
April 18, 2017

Data
----

This plot is based on uniform FBI Hate Crime data collected across a three year period (2011-13), from willing crime reporting agencies throughout the USA. From that data we extract the total number of hate crimes per region of the United States, and then divide them into crimes targeting Muslims and other crimes. We also include population data from the 2010 country-wide census, procured from the socialexplorer website, in order to control for population of each region.

Rationale
---------

This is by no means a complete analysis, but just an initial showcase of what could be done with the FBI data and a small amount of extra inputs. The way that hatecrime rates are calculated is primitive (mean()), and possibly oversimplified as it is calculated across *region* of the US, and not controlled by, for example, if the area is metropolitan, what the racial makeup of each area is, or other interesting factors like educational level, or mean income. However, we can still see some trends that would be worth discussing. We can first see that some regions have increased rates of hatecrimes both against Muslims and non-Muslims; in the South, for example, an increasing trend in hatecrimes in general, is coupled with a stark decrease in anti-Muslim hate crime rates. These issues beg further analysis.

``` r
#Read Incident Files
incidents_df <- read.csv("C:\\Users\\tdounias\\Desktop\\Reed College\\Spring 2017\\MATH 241\\Repositories\\anti-muslim_rhetoric\\data\\hatecrime_incidents_2011to13.csv")
reporting_df <- read.csv("C:\\Users\\tdounias\\Desktop\\Reed College\\Spring 2017\\MATH 241\\Repositories\\anti-muslim_rhetoric\\data\\hatecrime_reporters_2011to13.csv")
popdata_df <- read.csv("C:\\Users\\tdounias\\Desktop\\Reed College\\Spring 2017\\MATH 241\\Repositories\\anti-muslim_rhetoric\\data\\Population_data.csv")

#Create Dataset with regions and hatecrime numbers per year
viz1 <- incidents_df %>%
  mutate(Year = year(Incident_Date), count = 1) %>%
  group_by(State_Code, Year) %>%
  summarize(No_Non_Muslim = sum(Anti_Muslim == "N"), No_Muslim = sum(Anti_Muslim == "Y"))

region <- reporting_df %>%
  filter(State_Code < 51) %>%
  group_by(State_Code, Master_File_Year, Country_Region) %>%
  summarize()

viz1[, 5] <- region[, 3]

#Handle Population Data
total_pop <- popdata_df %>%
  group_by(Geo_STATE) %>%
  summarize(total_pop = sum(SE_T001_001)) %>%
  rename(State_Code = Geo_STATE)

viz1 <- inner_join(viz1, total_pop, by = "State_Code")

#Create dataframe for visualizing
viz1_1 <- viz1 %>%
  mutate(HC_by_pop_nonM = No_Non_Muslim / total_pop, HC_by_pop_M = No_Muslim / total_pop) %>%
  group_by(Country_Region, Year) %>%
  summarize(Mean_Rate_Muslim = mean(HC_by_pop_M), Mean_Rate_Non_Muslim = mean(HC_by_pop_nonM)) %>%
  gather(key = Type, value = Rate, 3, 4)

viz1_1$Year <- as.factor(viz1_1$Year)

#Plot graphics
ggplot(viz1_1, aes(Year, Rate, col = Type, group = Type)) +
  geom_point() + 
  geom_line() +
  facet_wrap(~Country_Region) +
  scale_y_log10() +
  labs(y = "Mean Rates of Hatecrimes by Population", title = "Mean Hatecrime Rates by US Region")
```

![](HateCrime_Means_Region_files/figure-markdown_github/unnamed-chunk-1-1.png)
