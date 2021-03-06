Potential Time Series Visualizations
================

``` r
#Tidying different data sets

new_join <- read_csv("new_join.csv")

new_new_join <- gather(new_join, key = type, value = count, Islamophobia, Anti_Muslim) #Creating "type" variable to plot both sets of data in one visualization

weeks <- new_new_join %>%  #Grouping by week
  mutate(week = (year(newdate) - year(min(newdate)))*52 + 
                 week(newdate) - week(min(newdate)),
               week2 = (as.numeric(newdate) %/% 7) 
         - (as.numeric(min(newdate)) %/% 7)) %>%
  arrange(newdate)

join_2011 <- filter(weeks, Year == "2011") #Filtering by year from grouped weeks data
join_2012 <- filter(weeks, Year == "2012")
join_2013 <- filter(weeks, Year == "2013")


new_join_month1 <- join_2011 %>% mutate(Month = month(newdate))
new_join_month2 <- join_2012 %>% mutate(Month = month(newdate))
new_join_month3 <- join_2013 %>% mutate(Month = month(newdate))
```

### Bar chart grouped by WEEK and FLIPPED

``` r
#Bar chart with coordinates flipped, by week

ggplot(data = join_2011, aes(x=week, y=count, fill=type), group=type) + 
  geom_bar(stat = "identity", position = "identity") +
coord_flip() +
  theme_minimal() +
  labs(title = "Rate of Islamophobia in National Media 
       and the Rate of Anti-Muslim Hate Crimes 
       in the United States, 2011", x = "Week (1-52) of 2011", y = "Count") + 
  scale_fill_discrete(name="Legend")
```

<img src="Time_Series_files/figure-markdown_github/flip-1.png" width="1000px" />

``` r
ggplot(data = join_2012, aes(x=week, y=count, fill=type), group=type) + 
  geom_bar(stat = "identity", position = "identity") +
  coord_flip() +
  theme_minimal() +
  labs(title = "Rate of Islamophobia in National Media 
       and the Rate of Anti-Muslim Hate Crimes 
       in the United States, 2012", x = "Week (1-52) of 2012", y = "Count") + 
  scale_fill_discrete(name="Legend")
```

<img src="Time_Series_files/figure-markdown_github/flip-2.png" width="1000px" />

``` r
ggplot(data = join_2013, aes(x=week, y=count, fill=type), group=type) + 
  geom_bar(stat = "identity", position = "identity") +
  coord_flip() +
  theme_minimal() +
  labs(title = "Rate of Islamophobia in National Media 
       and the Rate of Anti-Muslim Hate Crimes 
       in the United States, 2013", x = "Week (1-52) of 2013", y = "Count") + 
  scale_fill_discrete(name="Legend")
```

<img src="Time_Series_files/figure-markdown_github/flip-3.png" width="1000px" />

### Bar chart grouped by WEEK

``` r
#Regular bar chart, grouped by week

ggplot(data = join_2011, aes(x=week, y=count, fill=type), group=type) + 
  geom_bar(stat = "identity", position = "identity") +
  theme_minimal() +
  labs(title = "Rate of Islamophobia in National Media 
       and the Rate of Anti-Muslim Hate Crimes 
       in the United States, 2011", x = "Week (1-52) of 2011", y = "Count") + 
  scale_fill_discrete(name="Legend")
```

<img src="Time_Series_files/figure-markdown_github/noflip-1.png" width="1000px" />

``` r
ggplot(data = join_2012, aes(x=week, y=count, fill=type), group=type) + 
  geom_bar(stat = "identity", position = "identity") +
  theme_minimal() +
  labs(title = "Rate of Islamophobia in National Media 
       and the Rate of Anti-Muslim Hate Crimes 
       in the United States, 2012", x = "Week (1-52) of 2012", y = "Count") + 
  scale_fill_discrete(name="Legend")
```

<img src="Time_Series_files/figure-markdown_github/noflip-2.png" width="1000px" />

``` r
ggplot(data = join_2013, aes(x=week, y=count, fill=type), group=type) + 
  geom_bar(stat = "identity", position = "identity") +
  theme_minimal() +
  labs(title = "Rate of Islamophobia in National Media 
       and the Rate of Anti-Muslim Hate Crimes 
       in the United States, 2013", x = "Week (1-52) of 2013", y = "Count") + 
  scale_fill_discrete(name="Legend")
```

<img src="Time_Series_files/figure-markdown_github/noflip-3.png" width="1000px" />

### Bar chart grouped by MONTH

``` r
ggplot(data = new_join_month1, aes(x=Month, y=count, fill=type), group=type) + 
  geom_bar(stat = "identity", position = "identity") +
  theme_minimal() +
  labs(title = "Rate of Islamophobia in National Media 
       and the Rate of Anti-Muslim Hate Crimes 
       in the United States, 2011", x = "Month of 2011", y = "Count") + 
  scale_fill_discrete(name="Legend")
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-1-1.png" width="1000px" />

``` r
ggplot(data = new_join_month2, aes(x=Month, y=count, fill=type), group=type) + 
  geom_bar(stat = "identity", position = "identity") +
  theme_minimal() +
  labs(title = "Rate of Islamophobia in National Media 
       and the Rate of Anti-Muslim Hate Crimes 
       in the United States, 2012", x = "Month of 2012", y = "Count") + 
  scale_fill_discrete(name="Legend")
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-1-2.png" width="1000px" />

``` r
ggplot(data = new_join_month3, aes(x=Month, y=count, fill=type), group=type) + 
  geom_bar(stat = "identity", position = "identity") +
  theme_minimal() +
  labs(title = "Rate of Islamophobia in National Media 
       and the Rate of Anti-Muslim Hate Crimes 
       in the United States, 2013", x = "Month of 2013", y = "Count") + 
  scale_fill_discrete(name="Legend")
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-1-3.png" width="1000px" />

### Line chart grouped by DAY

``` r
#This a line plot grouped by DAY

ggplot(data = join_2011, aes(x=newdate, y=count, col=type, fill=type)) + geom_line() +
  scale_x_date(date_labels = "%b %d") + xlab("") + ylab("Count")
```

<img src="Time_Series_files/figure-markdown_github/line-1.png" width="1000px" />

``` r
ggplot(data = join_2012, aes(x=newdate, y=count, col=type)) + geom_line() +
  scale_x_date(date_labels = "%b %d") + xlab("") + ylab("Count")
```

<img src="Time_Series_files/figure-markdown_github/line-2.png" width="1000px" />

``` r
ggplot(data = join_2013, aes(x=newdate, y=count, col=type)) + geom_line() +
  scale_x_date(date_labels = "%b %d") + xlab("") + ylab("Count")
```

<img src="Time_Series_files/figure-markdown_github/line-3.png" width="1000px" />

### Line chart grouped by WEEK

``` r
#Grouped by WEEK

ggplot(data = join_2011, aes(x=week, y=count, col=type)) + geom_line() 
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-2-1.png" width="1000px" />

``` r
ggplot(data = join_2012, aes(x=week, y=count, col=type)) + geom_line() 
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-2-2.png" width="1000px" />

``` r
ggplot(data = join_2013, aes(x=week, y=count, col=type)) + geom_line() 
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-2-3.png" width="1000px" />

### Filled density plot by DAY per YEAR

``` r
join_2011$type <- as.factor(join_2011$type)
join_2011$type = factor(join_2011$type,levels(join_2011$type)[c(2,1)])

join_2012$type <- as.factor(join_2012$type)
join_2012$type = factor(join_2012$type,levels(join_2012$type)[c(2,1)])

join_2013$type <- as.factor(join_2013$type)
join_2013$type = factor(join_2013$type,levels(join_2013$type)[c(2,1)])

ggplot(data = join_2011, aes(x=newdate, y=count, col=type, fill=type, alpha=.3)) + geom_density(stat = "identity") + theme_minimal() + scale_fill_manual(values=c("#bfd0e0", "#027aca")) + scale_color_manual(values=c("#bfd0e0", "#027aca"))
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-3-1.png" width="1000px" />

``` r
ggplot(data = join_2012, aes(x=newdate, y=count, col=type, fill=type, alpha=.3)) + geom_density(stat = "identity") + theme_minimal() + scale_fill_manual(values=c("#bfd0e0", "#027aca"), name="",
                         labels=c("Islamophobia in media", "Anti-Muslim hate crimes")) + scale_color_manual(values=c("#bfd0e0", "#027aca"), guide=FALSE) + scale_alpha(guide=FALSE)
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-3-2.png" width="1000px" />

``` r
ggplot(data = join_2013, aes(x=newdate, y=count, col=type, fill=type, alpha=.3)) + geom_density(stat = "identity") + theme_minimal() + scale_fill_manual(values=c("#bfd0e0", "#027aca")) + scale_color_manual(values=c("#bfd0e0", "#027aca"))
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-3-3.png" width="1000px" />

### By WEEK for all THREE YEARS

``` r
weeks$type <- as.factor(weeks$type)
weeks$type = factor(weeks$type,levels(weeks$type)[c(2,1)])

ggplot(data = weeks, aes(x=week, y=count, col=type, fill=type, alpha=.3)) + geom_density(stat = "identity") + theme_minimal() + scale_fill_manual(values=c("#bfd0e0", "#027aca")) + scale_color_manual(values=c("#bfd0e0", "#027aca"))
```

<img src="Time_Series_files/figure-markdown_github/unnamed-chunk-4-1.png" width="1000px" />
