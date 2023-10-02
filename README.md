# Exploratory-Data-Analysis1-
Exploratory Data Analysis | Dataset Global Terrorism 1970-2017 | The Sparks Foundation
#Importing Libraries
library(tidyverse)
library(flexdashboard)
library(highcharter)
library(gt)
library(htmltools)
library(viridisLite)
library(readr)
library(dplyr)
```

```{r}
#Importing Data
data <- read_csv("globalterrorismdb_0718dist.csv")
```

```{r}
#Exploring Data Analysis
str(data)

summary(data)

colSums(is.na(data))

terrorism_data <- data[, c("iyear", "imonth", "iday", "country_txt", "region_txt", "latitude", "longitude", "attacktype1_txt", "targtype1_txt", "gname", "nkill", "nwound")]
```

```{r}
# Handling missing values
terrorism_data[is.na(terrorism_data)] <- 0
hot_zones <- terrorism_data %>%
  group_by(country_txt) %>%
  summarize(total_attacks = n()) %>%
  arrange(desc(total_attacks))
```

```{r}
# Explore correlations
cor(terrorism_data[, c("nkill", "nwound")])
print(cor)
```

```{r}
# Print hot zones
print(hot_zones)
library(ggplot2)

ggplot(hot_zones, aes(x = reorder(country_txt, total_attacks), y = total_attacks)) +
  geom_bar(stat = "identity") +
  xlab("Country") +
  ylab("Total Attacks") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
```

```{r}
# Visualize attack types
ggplot(terrorism_data, aes(x = attacktype1_txt)) +
  geom_bar(fill = "red") +
  theme(axis.text.x = element_text(angle = 45, vjust = 0.5, hjust=1))
```
