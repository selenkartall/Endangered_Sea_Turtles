# <b>Endangered Species: Sea Turtles</b>
## "Vinir"


_**Team members:**_

- Selen Kartal
- İpek Korkmaz


### **Table of Contents:**
[Project Description](#project-description)  
[Sea Turtles](#sea-turtles)   
[Data Set Description](#data-set-description)    
[Actions In Project Process](#actions-in-project-process)    
[Results](#results)   
[Discuss](#discuss)   
[Conclusion](#conclusion)   
[Refereces](#refereces)

<br>

### **Project Description**

A social problem is any condition or behavior that has negative consequences for large numbers of people and that is generally recognized as a condition or behavior that needs to be addressed. These problems such as environmental pollution, forest fires, endangered animals, murders, … etc. have become increasingly common in the world especially in recent years. While social awareness should increase over the years, unfortunately, the importance and value that given to nature, animals and people decreases inversely. To draw attention to this issue, we have chosen one of these problems and worked on it to raise awareness.

<br>

### **Sea Turtles**

Sea turtles are large, air breathing, ectothermic reptiles that have adapted for life in the sea. They have paddle-shaped flippers instead of feet, streamlined bodies, salt glands, and cannot retract into their shell like a land turtle can.  Sea turtles have ancestors pre-dating the dinosaurs 245 million years ago. Seven species of sea turtles have managed to survive to modern times. All are considered threatened or endangered. Three of these species; the Loggerhead (Caretta caretta), Green (Chelonia mydas), and Leatherback (Dermochelys coriacea) sea turtles nest on Broward County’s beaches, and two of these species; the Hawksbill (Eretmochelys imbricate) and kemp’s Ridley (Lepidochlys kempi) are seen offshore.

Despite that only Caretta carettas come to minds in general. In our project, we have chosen the extinction of these 6 more species of sea turtles. We found a data set that includes observations of sea turtles nesting between 1930 and 2018.

These seven species are:

**The Loggerhead Sea Turtle (Caretta caretta)**, is a species of oceanic turtle distributed throughout the world. The loggerhead sea turtle is found in the Atlantic, Pacific, and Indian Oceans, as well as the Mediterranean Sea. They do not go ashore except nesting period. 

**Green Sea Turtle (Chelonia mydas)**, is easily distinguished from other sea turtles because they have a single pair of prefrontal scales (scales in front of its eyes), rather than two pairs as found on other sea turtles. Mainly stay near the coastline and around islands and live in bays and protected shores, especially in areas with seagrass beds. Rarely are they observed in the open ocean. 

**Leatherback Sea Turtle (Dermochelys coriacea)**, is the only sea turtle that lacks a hard shell. Primarily found in the open ocean, as far north as Alaska and as far south as the southern tip of Africa, though recent satellite tracking research indicates that leatherbacks feed in areas just offshore. Most widely distributed of all sea turtles. Found world wide with the largest north and south range of all the sea turtle species.

**Olive Ridley Sea Turtle (Lepidochelys olivacea)**, are generally found in coastal bays and estuaries, but can be very oceanic over some parts of its range. Nest every year in mass synchronized nestings called arribadas and only the Kemp’s ridley also nests this way. 

**Hawksbill Sea Turtle (Eretmochelys imbricata)**, is one of the smaller sea turtles. Typically found around coastal reefs, rocky areas, estuaries and lagoons. Most tropical of all sea turtles. Tropical and subtropical waters of the Atlantic, Pacific and Indian Oceans. 

**Flatback Sea Turtle (Natator depressus)**, prefers turbid inshore waters, bays, coastal coral reef and grassy shallows. Their range is very limited. It is found only in the waters around Australia and Papua New Guinea in the Pacific. 

**Kemp’s Ridley Sea Turtle (Lepidochelys kempii)**, prefers shallow areas with sandy and muddy bottoms. Kemp’s ridleys nest more often than other species, every 1 to 3 years on average. The Kemp’s ridleys’ nesting is also arribadas. Adults are mostly limited to the Gulf of Mexico. Juveniles range between tropical and temperate coastal areas of the northwest Atlantic Ocean and can be found up and down the east coast of the United States.



As we mentioned before, our goal in this project is to show the decreasement in number of observation of sea turtles nesting and this way to raise awareness about this issue. Also, to discuss the possible reasons of this decreasement in the given years.

<br>

### **Data Set Description**

For our project, we have worked with the data sets that we found from [https://seamap.env.duke.edu/swot](https://seamap.env.duke.edu/swot). (The sea turtle data contributions to SWOT are archived within a specialized SWOT mapping application within the OBIS-SEAMAP system (for the website click [here](http://seamap.env.duke.edu)). It is from Ocean Biodiversity Information System (OBIS) which is a Project under IOS-UNESCO's International Oceanographic Data and Information Exchange (IODE). This dataset contains information about where sea turtles nest in which years.
To access the data, we have to state who we are and why we need it.  Unfortunately, we can not access to nesting count data set due to needing to require permission from each data provider.

The data set we have used contains 5693 rows and 18 columns. Variables and their meanings are:

- **code:** Country code, defined in ISO 3166-1 alpha-2.
- **country:** Country where the sea turtles nested.
- **siteid:** Code of the site.
- **site_name:** Site where the sea turtles nested.
- **compiler:** Name of the community which compiled the nesting information.
- **species:** Scientific name of sea turtle.
- **common_name:** Common name of sea turtle.
- **years_monitored:** Years when nesting is observed.
- **monitoring_effort:** Methods of effort to collect the data.
- **latitude:** latitude of nesting observed.
- **longitude:** longitude of nesting observed.
- **nesting_status:** If their nesting number is counted, Its value is “QUANTIFIED”. If not, its value is “UNQUANTIFIED”.
- **mds:** Minimum Data Standards. Level 1 is the highest quality standards and level 2 is minimum quality standards in the SWOT database.
- **contact1-5:** Contact to community or people who collected the data.

<br>

#### **Actions In Project Process**

After importing the data set, we change the "Unknown"s with NA and remove contact columns.

```{r}
data <- read_csv("data/obis_seamap_swot_site_locations.csv") %>%
        select(code:mds) %>%
        na_if("Unknown")
```

The changes can be seen below. However, there is a problem in the year_monitored column. It is separated by ";". To use the years separately, we have to change this part.

```{r}
head(data)
```

Firstly, we detect columns that have ";".

```{r}
multi_years <- str_detect(data$years_monitored, ";")
```


We separate the rows which have ";" in years_monitered, monitoring_effort, and mds columns with str_split function. In this way, these rows become vectors and these columns become lists.

```{r}
data$years_monitored <- str_split(data$years_monitored, ";")
data$monitoring_effort <- str_split(data$monitoring_effort, ";")
data$mds <- str_split(data$mds, ";")
```


We put the boolean values which are in the multi_years vector to the loop. If bool is FALSE, we will add 1 to row variable. If bool is TRUE, we will unlist years_monitored. Then it will go into the for loop. In the for loop, we will add separately a row which includes the year that we unlist in the years_monitored column. After that, we will add 1 to row variable.

```{r}
row = 1
for (bool in multi_years){
  if(bool){
    years_vector <- unlist(data$years_monitored[row])
    for (a_year in years_vector){
      data <- data %>% add_row(tibble_row(code=data$code[row],
                                          country=data$country[row],
                                          siteid=data$siteid[row],
                                          site_name=data$site_name[row],
                                          compiler=data$compiler[row],
                                          species=data$species[row],
                                          common_name=data$common_name[row],
                                          years_monitored=list(a_year),
                                          monitoring_effort=data$monitoring_effort[row],
                                          latitude=data$latitude[row],
                                          longitude=data$longitude[row],
                                          nesting_status=data$nesting_status[row],
                                          mds=data$mds[row]))
    }
    row=row+1
  }
  else{
    row=row+1
    }
}
```


Because the number of rows in data and the length of multi_years are different, we subtract the length of multi_years from the number of rows in data. We add a number of FALSE, which is equal to the result of the subtraction, to the end of the multi_years vector.

```{r}
dim_dif <- dim(data)[1] - length(multi_years)
multi_years <- append(multi_years, rep(FALSE, times=dim_dif))
```


We remove the rows from data which are still in vector form by using multi_years vector in the previous step.

```{r}
data <- data[!multi_years, ]
```


We convert years_monitored column from list, which has character values, to numeric values. 

```{r}
data <- data %>%
  mutate_at(vars(years_monitored), as.numeric)
str(data$years_monitored)
```


We create factor column variable called species.

```{r}
data <- data %>%
        mutate(species = factor(species, levels=c("Caretta caretta",
                                                  "Chelonia mydas",
                                                  "Dermochelys coriacea",
                                                  "Lepidochelys olivacea",
                                                  "Eretmochelys imbricata",
                                                  "Natator depressus",
                                                  "Lepidochelys kempii")))
```


We count the number of species over the years.

```{r}
yearly_data <- data %>%
              group_by(years_monitored) %>%
              count(species)
```


We create "colors" vector with tmaptools::palette_explorer() to do not write this over and over again. This color palette's name is "Accent" but we need their Hex Code to be fixed colors and to plot the one by one graphs of the species with their previous color.

```{r}
colors = c("Caretta caretta" = "#7FC97F",
           "Chelonia mydas" = "#BEAED4",
           "Dermochelys coriacea" = "#FDC086",
           "Lepidochelys olivacea" = "#E8E46E",
           "Eretmochelys imbricata" = "#386CB0",
           "Natator depressus" = "#f29191",
           "Lepidochelys kempii" = "#999b84")
```


We plot the species' numbers over the years. Firstly, we prefer the bar chart to see total numbers yearly of all the species in one chart. We see from the chart that some species have been seen rarely and some of them have been clustered over the years.

```{r}
barchar <- yearly_data %>%
           ggplot(aes(x = years_monitored, y = n, fill = species)) +
                geom_bar(stat = "identity", position = "stack") +
                labs(y = "Number of Observations",
                    x = ("Observed Years"),
                    title = "Species' numbers over the years",
                    fill = "Species") +
                scale_x_continuous(breaks=c(1930, 1940, 1950, 1960,
                                            1970, 1980, 1990, 2000, 2010, 2020)) +
                theme(plot.title = element_text(face = "bold", size = 15, hjust = 0.5),
                    legend.title = element_text(face = "bold"),
                    axis.title = element_text(face = "bold"),
                    panel.background = element_rect("#Fcfeff")) +
                scale_fill_manual(values = colors)
```
![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/over_all_bar_chart.png)

When we look at the total bar graph, we can see that number of observation peaked at 1192 in 2006 with 7 species. During the 1930-2018 timeline, first apperance was in 1938 and until 1969 there does not exist any kind of species that observed. The observation continued regularly until 2018 except 1972, 1974, and 1975. In 1980, 1978, 1977, 1976, 1973, 1971, 1969, and 1938, only one species has been observed in each year. 

Also, we want to draw a line chart to see how much the species' observation numbers changed separately through the years.

```{r}
linechar <- yearly_data %>%
            ggplot( aes(x=years_monitored, y=n, group=species, color=species)) +
            geom_line() +
            geom_point(shape=19, size=1) +
            labs(y = "Number of Observations", x = ("Observed Years"),
                  title = "Species' numbers over the years") +
            scale_x_continuous(breaks=c(1930, 1940, 1950, 1960,
                                        1970, 1980, 1990, 2000, 2010, 2020)) +
            theme(plot.title = element_text(face = "bold", size = 15, hjust = 0.5),
                  legend.title = element_text(face = "bold"),
                  axis.title = element_text(face = "bold"),
                  panel.background = element_rect("#Fcfeff")) +
            scale_colour_manual("Species", values = colors)
```
![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/over_all_line_chart.png)


We create pie_data to plot pie chart.

```{r}
pie_data <- data %>%
            group_by(species) %>%
            summarize(n = n())
```


We prefer pie chart because it is easy way to see the comparison between species' observation numbers of all time.

```{r}
pie_title <- sprintf("<b>Total observation of species between 1930 and 2018</b>")

pie_data %>%
  plot_ly(labels = ~species, values = ~n, type = 'pie',
          marker = list(colors = colors,
                        line = list(color = '#FFFFFF', width = 1))) %>%
  layout(title = pie_title, font = list(face="bold"),
         xaxis = list(showgrid = FALSE, zeroline = FALSE, showticklabels = FALSE),
         yaxis = list(showgrid = FALSE, zeroline = FALSE, showticklabels = FALSE),
         margin = list(t=50))
```
![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/over_all_pie_chart.png)

From the pie chart, it can be seen that the most observed species from 1930 to 2018 is Chelonia mydas with 27.7%. It is followed by Eretmochelys imbricata with a little difference. The least observed species is Natator depressus with 2.5%.


Now, with the help of our graphs, we will analyze ever species’ numerical results one by one.

We make only Caretta_caretta chart code visible because the rest of the charts' codes are similar. Except, Chelonia_mydas and Lepidochelys_olivacea charts. In addition to these, we write a code that separates the years in order which increases by 10. 

```{r}
Caretta_caretta <- yearly_data %>%
  filter(species == "Caretta caretta") %>%
  ggplot(aes(x=years_monitored, y=n)) +
  geom_bar(stat = "identity", fill="#7FC97F") +
  labs(y = "Number of Observations", x = "Observed Years",
       title = "Observation of Caretta caretta numbers over the years") +
  scale_x_continuous(breaks=c(1980, 1990, 2000, 2010, 2020)) +
  theme(panel.background = element_rect("#Fcfeff"))

ggplotly(Caretta_caretta)
```
![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/Caretta_caretta_bar_chart.png)

Caretta Carettas were seen for the first time in 1979 and never seen in 1980, between 1982-1990 and 1990-1992 at all. Before 1983, they were seen rarely. They started to appear regularly after 1990. Small decreasements and increments can be seen in this time period. The number of observations peaked at 220 in 2006. Also, there exists a huge increment after 2004 and decreasement after 2006. They were observed last in 2018.

![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/Chelonia_mydas_bar_chart.png)

Chelonia Mydases were seen for the first time in 1930 and seen again just one more time in 1938 until 1970. After 1970, we can see the observation more regular in the following years. A great increment can be seen after 2005 and great decreasement after 2007. Also, in 2006, the number of observation peaked with 308. They were observed last in 2017.

![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/Dermochelys_coriacea_bar_chart.png)

Dermochelys coriaceas were seen for the first time in 1979. In the 1980s, they were seen in some years. From 1990, they started to appear regularly. A huge increment can be seen after 1999. Also, there exists a huge decreasement after 2000. The number of observations peaked at 146 in 2006. They were observed last in 2016.

![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/Lepidochelys_olivacea_bar_chart.png)

Lepidochelys olivaceas were seen for the first time in 1969. They were observed in some years until 1992. The process between 1969 and 1992 is not regular. The number of observations peaked at 120 in 2009. There exists a great increment after 2008 and decreasement after 2009. They were observed last in 2016.

![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/Eretmochelyses_imbricata_bar_chart.png)

Eretmochelyses imbricata were seen for the first time in 1979. Between 1979 and 1986, they were only observed in 1985. From 1986, they started to appear regularly. A huge increment can be seen after 2005 and there exists a huge decreasement after 2006. The number of observations peaked at 487 in 2006. They were observed last in 2016.

![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/Natator_depressus_bar_chart.png)

Natator depressuses were seen for the first time in 1986 and for the last time in 2008. In this time line they were only observed in 1987, 1992, 1997, 1999, 2004, 2005, and 2006. There exists a great increment after 2007 and decreasement after 2004. The number of observations peaked at 110 in 2008.

![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/Lepidochelys_kempii_bar_chart.png)

Lepidochelys kempiis were seen for the first time in 1978. They were observed last in 2014. There is not any year such that they were not observed. The observation numbers are pretty regular from 1978 to 2014. There exists a great increment after 2013 and decreasement after 2006. The number of observations peaked at 42 in 2013.


Finally, to obtain an interactive map, we create data_sf which contains coordinate system. That is why data_sf is a special data set called sf.

```{r}
data_sf <- st_as_sf(data,
                 coords = c("longitude", "latitude"),
                 crs = 4326)
```


We plot map with using leaflet() function.

```{r}
pal <- colorFactor(colors, domain = unique(data_sf$species))

labels <-  sprintf("<strong>%s</strong><br/> Site Name: %s",
                   data_sf$species, data_sf$site_name) %>%
          lapply(htmltools::HTML)

data_sf %>%
  leaflet() %>%
  addTiles() %>%
  addCircleMarkers(radius = 3,
                   color = ~pal(species),
                   stroke = FALSE,
                   fillOpacity = 1,
                   label = labels) %>%
  addLegend("bottomright",
            pal=pal,
            values=~species,
            title = 'Species')
  
```
![](https://raw.githubusercontent.com/selenkartall/Endangered_Sea_Turtles/main/images/leaflet.PNG)

We can see from map that some species' ranges are very limited. For example, Natator depressus is found around Australia and Papua New Guinea in the Pacific. Lepidochelys kempii are mostly limited to the Gulf of Mexico and the east coast of the United States. The others do not have a certain range. Also, with this map, we have shown that these locations are similiar with the ones that we mentioned before.

<br>

### **Results**

In our project, we have used several types of graphs to finalize the results. Firstly, we plotted bar graph to see the total numbers of observation over the years clearly. In bar graphs, every single species are stacked one after the other. In this way, the total numbers of observation in each year can be seen. Secondly, we plotted line graph. We chose this graph because it is the ideal one to see every increment and decreasement of observation’s numbers for each species within the years. Thirdly, we plotted pie chart. We used this chart to compare the species’ percentage over the years. Then, we created a map by using leaflet function. In this map, we marked species’ locations. Thus, this map includes distributions of every species’ locations. We can see in which year and where a species is seen and not seen anymore. Also, with these graphs, we can choose to observe the datas of every species’ on their own and not all species in total. Finally, we have created an application which is called Shiny. With our Shiny application, we can choose sea turtle species and year range for better observing. Moreover, in this way, we can compare sea turtles with each other easily. We can see the decreasement in numbers of obseravation of sea turtles.

<br>

### **Discuss**

At the beginning of our project, we have planned to discuss the reasons behind the decreasement of observation numbers. We will share our opinions and the information that we have found about extinction of sea turtles. Before this part, we must say that this discussion part is mostly based on our opinions from our research. 

When we searched the reasons behind this social problem, we faced devastating truths. Unfortunately, almost every reason were related to the human-beings. Sea turtles which maintained their lives for 245 million years, are facing an extinction because of us. Humans hunt them because of their meat, leather, shell, and eggs for collections or ornament. They harvest their nesting habitat by housing developments and coastal lightnings. Also, sea pollution which is created by humans is also undeniably high. There are several kinds of this pollution such as noise, plastic, light, industrial wastes, … etc. According to Deutsche Welle’s news in 2017, in the last 10 years, the pollution in the seas has increased 20 times and plastic production is expected to double in the next 20 years. Besides plastic pollution, noise pollution in the oceans by ship engines, oil rigs, undersea mining, and other man-made sounds are important, too. Lastly, climate change has affected the extinction of sea turtles as well as other living creatures. We found many reasons about extinction of sea turtles but we could not find direct reasons for the decreasing numbers in years and locations.

There might be few ideas to improve our project. For instance, by using the datas after 2018, this project can be updated. Moreover, by collecting the numbers of observation of other sea creatures’ datas, it can be searched that if there is any similarity with decreasement in their numbers between years or locations. If there is, the reasons can be objectified.

<br>

### **Conclusion**

In conclusion, this Project aims to point extinction of sea turtles. To do that first, we have found a data set which contains numbers of observation between 1930 and 2018. After that we have made visualizations so that this data can be understood more clearly by everyone. In this visualizations we plotted bar, line, and pie charts. Also, we created an interactive map to show their nesting locations. We have searched possible reasons for this extinction. Unfortunately, we have seen many factors that are effective in decreasement in numbers. There is some work being done to save them. In the Atlantic, the loggerhead sea turtle and green sea turtle are listed as threatened. The Leatherback, Hawksbill, and Kemp’s Ridley sea turtle species are listed as endangered everywhere. Since 1973, all species of sea turtles are threatened or endangered and protected through Florida Statues, Chapter 370, and by the United States Endangered Species Act of 1973.  Moreover, Indicit conducted studies on sea turtles and marine waste between 2017-2019. In Turkey, WWF carries on a work about endangered species. However it is not enough. If we want to save these endangered animals, we have to do more about them. Everyone should create awareness and do their responsibilities. If we want to save our planet, this is the only choice.

<br>

### **Refereces**


- [hcas.nova.edu](https://hcas.nova.edu/seaturtles/endangered.html)
- [conserveturtles.org](https://www.conserveturtles.org)
- [wikipedia.org](https://tr.wikipedia.org)
- [seamap.env.duke.edu/swot](https://seamap.env.duke.edu/swot)
- [seaturtlestatus.org](https://www.seaturtlestatus.org/)
- [r-graph-gallery.com](https://www.r-graph-gallery.com/)
- [stackoverflow.com](https://stackoverflow.com/)
- [colorhunt.co](https://colorhunt.co/)
- [pau.edu.tr](https://www.pau.edu.tr/dekamer/tr/sayfa/indicit-projesi)
- [trthaber.com](https://www.trthaber.com/haber/bilim-teknoloji/2020-yili-sessiz-okyanus-yili-ilan-edildi-573714.html/amp)
- [amp.dw.com](https://amp.dw.com/tr/okyanuslar%C4%B1n-en-b%C3%BCy%C3%BCk-sorunu-plastik-kirlili%C4%9Fi/a-37610725)
- [wwf.org.tr](https://www.wwf.org.tr/ne_yapiyoruz/doga_koruma/turler/deniz_kaplumbaas/)
- [adfg.alaska.gov](https://www.adfg.alaska.gov/index.cfm?adfg=specialstatus.fedsummary&species=leatherbackseaturtle)
