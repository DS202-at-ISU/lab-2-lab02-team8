
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#1

## Owen Kim

### Data Exploration

Inspecting relationships between year built and sale price<br>

1.  There are various variables related to housing, like

- Sale Price
- Total Living Area
- Year Built
- Neighborhood
- Bedrooms
- Lot Area

2.  I will be inspecting relationships between year built and sale
    price.

3 & 4.

    > range(ames$YearBuilt, na.rm = TRUE)
    [1]    0 2022 # there is a dirty data with YearBuilt = 0 messing up the range

<br>

    > range(ames$`Sale Price`, na.rm = TRUE)
    [1]        0 20500000 # also a dirty data

<br>

    #plot line chart
    ggplot(ames, aes(x = `YearBuilt`, y = `Sale Price`)) +
      geom_line()

![](images/dirty-sale-vs-year2.png)

    # clean x and plot again
    ames_clean <- ames %>%
      filter(`YearBuilt` >= 1880, !is.na(`YearBuilt`), !is.na(`Sale Price`))

    ggplot(ames_clean, aes(x = `YearBuilt`, y = `Sale Price`)) +
       geom_point() +
       geom_smooth(method = "loess") +
       labs(title = "Sale Price vs Year Built",
            x = "Year Built",
            y = "Sale Price")

![](images/dirty-sale-vs-year.png)

    # remove y outliers and plot again
    Q1 <- quantile(ames_clean$`Sale Price`, 0.25)
    Q3 <- quantile(ames_clean$`Sale Price`, 0.75)
    IQR <- Q3 - Q1

    ames_clean <- ames_clean %>%
      filter(`Sale Price` > (Q1 - 1.5 * IQR) & `Sale Price` < (Q3 + 1.5 * IQR))
      
    ggplot(ames_clean, aes(x = `YearBuilt`, y = `Sale Price`)) +
       geom_point() +
       geom_smooth(method = "loess") +
       labs(title = "Sale Price vs Year Built",
            x = "Year Built",
            y = "Sale Price")

![](images/sale-price-vs-year-built-cleaned.png) <br> Fair to conclude
that house price increased over time, and newly built houses are worth
more, house price started increasing more rapidly starting 1980.

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.
