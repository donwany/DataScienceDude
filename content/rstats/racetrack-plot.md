Title: Racetrack Plot
Slug: racetrack-plot
Summary: Racetrack Plot
Date: 2016-05-01 12:00
Category: R Stats
Tags: Data Visualization
Authors: Chris Albon


Original source: http://stackoverflow.com/questions/15751442/making-a-circular-barplot-with-a-hollow-center-aka-race-track-plot

Want to learn more? I recommend working through: [R for Data Science](http://amzn.to/2myxnhi), [R Cookbook](http://amzn.to/2lF6hkb), and [R Graphics Cookbook](http://amzn.to/2m0fcPL).


```R
# load the ggplot2 library
library(ggplot2)
```


```R
# create some simulated data
Category <- c("Electronics", "Appliances", "Books", "Music", "Clothing",
              "Cars", "Food/Beverages", "Personal Hygiene",
              "Personal Health/OTC", "Hair Care")
Percent <- c(81, 77, 70, 69, 69, 68, 62, 62, 61, 60)
internetImportance <- data.frame(Category,Percent)
```

## Create a dataframe that contains the legend information


```R
# create a variable called len that equals 4
len <- 4
```


```R
# create a data frame with three elements, Category, Percent, and Category2. this is all blank data so that the middle of the racetrack is blank
df2 <- data.frame(Category = letters[1:len], Percent = rep(0, len),
                  Category2 = rep("", len))
```


```R
# create a new Category2 variable in the internetImportance df that contains the category name, a dash, the importance percent, and a percent sign
internetImportance$Category2 <-
  paste0(internetImportance$Category," - ",internetImportance$Percent,"%")
```


```R
# append number to category name
internetImportance <- rbind(internetImportance, df2)
```


```R
# set factor so it will plot in descending order
internetImportance$Category <-
  factor(internetImportance$Category,
         levels=rev(internetImportance$Category))
```

## Plot


```R
# great the ggplot data
ggplot(internetImportance, aes(x = Category, y = Percent,
                               fill = Category2)) +
  # create a bar plot
  geom_bar(width = 0.9, stat="identity") +
  scale_fill_brewer(palette="Set3") +
  # curve the bar chart
  coord_polar(theta = "y") +
  # remove the x and y axis labels
  xlab("") + ylab("") +
  # let the "circumference" of the racetrack to 100
  ylim(c(0,100)) +
  # add a title at the top of the chart
  ggtitle("Top Product Categories Influenced by Internet") +
  # add the legend text
  geom_text(data = internetImportance, hjust = 1, size = 3,
            aes(x = Category, y = 0, label = Category2)) +
  # add the text in the middle of the racetrack
  geom_text(label="GLOBAL", x=.5, y=.5, size=4) +
  # use the minimal theme to make is pretty
  theme_minimal() +
  # remove the external legend, and remove all the extra elements
  theme(legend.position = "none",
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        axis.line = element_blank(),
        axis.text.y = element_blank(),
        axis.text.x = element_blank(),
        axis.ticks = element_blank())
```









![png]({filename}/images/racetrack-plot_files/racetrack-plot_10_1.png)
