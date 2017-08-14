+++
title = "What is Chemistry?"
date = "2017-04-21T13:47:08+02:00"
tags = ["Scraping", "Ridge Regression"]
categories = ["Machine Learning"]
banner = "img/banners/pic3.jpeg"
+++


Playoffs in the NBA just started, and I hear reporters on the news talking about chemistry _all the time_. However, what exactly is chemistry? What determines if teammates have good chemistry? 

I decide to google "NBA Chemistry", and the consensus online is "chemistry", is actually really hard to define, because it is not quantified yet. So, how do you measure chemistry, when it could mean a million different variables. I decided to take a stab at it by using machine learning methods.

 
 My first step was to find and scrape data that relied on player to player interaction. I couldn't just scrape any stats data, I had to find data that showed how well two different teammates played _together_ while on the floor at the same time. I used **Beautiful Soup** and **Selenium** to scrape and piece the data together into a **Pandas** Dataframe.
 
 In order to get all the links off a site, I had to use enumerate and a for loop
   
```python
def scrape_all_players(driver):
    for idc, xyz in enumerate(site_list()):
        main_url = xyz
        driver.get(main_url)
        player_list_alphabet = driver.find_elements_by_xpath('//th//a')
```

On my first day of trying to scrape, the website temporarily banned me for 24 hours for scraping. I had to change my scraping parameters to trick the site into thinking I was a human. 

After scraping thousands of rows, and merging different sources, it was now time to manipulate my data into something that I could use machine learning on. 

My first step was to look at multicollinearity because I knew many of the variables would be correlated. I made a heatmap using **seaborn** to show this.

![w2heatmap](/img/heatmapweek2.png)

I tried 3 types(**linearcv**, **lassocv** and **ridgecv**) of regression formulas using **sklearn**. Ridge seemed to work the best because it set a penalty on large coefficients.

My R-Squared was significantly better and I plotted using **Plotly**(interactive chart) actual Win Percentage vs Predicted


![w2heatmap](/img/predictplotweek2.png)

All the outliers in the right were the Golden State Warriors, which I expected because they have been amazing. One interesting point I did find was that the uppermost right point was Javale McGee. This surprised me because his stats don't pop out. His stats are nothing special. However, According to the data he is a phenomenal teammate. Attributes that Javale Mcgee are good at are not represented in traditional NBA stats. NBA stats don't account for hustle, quick on defense, etc. 

Overall, I thought my project did a great job of showing how team chemistry can predict wins and the plots I made, showed how team chemistry contributes to those wins. 

Also, if anyone would like to see the data and one of the plots, I made a quick dashboard using **Shiny**, **DT**,**Plotly** and **R**. Go check it out at [Project Luther Dashboard](https://mcarrie30.shinyapps.io/project_benson/)!

![shinyweek2](/img/shinyweek2.png)



