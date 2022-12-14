DS 202 Final Project
================

<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

This repository serves as a starter repo for your final project, and
this Rmd is supposed to serve as a starter file for your project report.

## Part I: Repo Structure

The structure sketched out below is an idea of what your repository
might look like. You can use it as a starting base and change according
to your needs. But think about the changes that you make!

    -- code
    |   |   -- any R scripts you need but don't want to include directly in the write-up
    -- data
    |   |   -- csv files (cleaned data)
    -- data-raw
    |   |   -- raw data files 
    |   |   -- data description files, origin
    |   |   -- Codebook
    -- final-project.Rmd
    -- images  # only images that are not created by the Rmd
    -- LICENSE
    -- README.md
    -- README.Rmd
    -- README_files # folder with files created during the knitting process

## Part II: Project report

# Title of your project

Authors: Chuxiao Yu, Feifan Cao, Haoran Wang, Wenzheng Wang

## Abstract (TL;DR)

An abstract is a quick summary of your work. Ideally it should motivate
someone to read the rest of the paper. Include one sentence each on

-   what is the project about?
-   what is the motivation for doing it?
-   what data is your work based on? and where does it come from? = what
    are your main findings? (one sentence each)

# Intro/Background/Motivation

What is the topic of your project, why is it relevant?

At the end of the Intro, write a sentence describing what each of the
(result) sections is about, e.g. in section \[Results 1\] we show the
relationship between XXX and YYY, section \[Results 2\] also considers
the effect of variable ZZZ. … Finally we conclude with a quick summary
of our findings and potential follow-up work in section
[Conclusions](#conclusions).

Somewhere at the beginning of your project, include a code chunk that
includes all of the R packages you are using throughout. In this
document, the setup code chunk is called `setup` (see line 8) Also make
sure to set defaults for the code chunks - like should they be visible?
(probably not: echo=FALSE). Do you want to automatically include
warnings? (probably yes, for creating the Rmd, to make sure that all
warnings are accounted for)

Our project is about researching the graduation rates related to
different graduation years, different district, and different genders.
The graduation rate not only gives people insight into how many students
are finishing their degrees in a timely manner once they enroll, it is
also a transparent metric that holds a school accountable and helps
meassure the quality of the school.

# Quick Data Summary

`{r}{r setup, include=FALSE} #graduation rate data df=read.csv("4-Year_Graduation_Rates_in_Iowa_by_Cohort_and_Public_School_District.csv") dim(df) variable.names(df) length(unique(df$District))  length(unique(df$District.Name)) range(df$Graduating.Class) gr=na.omit(df) head(gr) dim(gr) #graduation intention data df3 <- read.csv("graduate_intention.csv") df3[df3 == ""] <- NA df3[df3 == "N/A"] <- NA gi=na.omit(df3) variable.names(gi) str(gi) gi$County=as.character(gi$County) gi$District=as.character(gi$District) gi$Diploma.Count=as.numeric(gi$Diploma.Count) gi$Private.4.Year.College=as.numeric(gi$Private.4.Year.College) gi$Public.4.Year.College=as.numeric(gi$Public.4.Year.College) gi$Private.2.Year.College=as.numeric(gi$Private.2.Year.College) gi$Community.College=as.numeric(gi$Community.College) gi$Other.Training=as.numeric(gi$Other.Training) gi$Employment=as.numeric(gi$Employment) gi$Homemaker=as.numeric(gi$Homemaker) gi$Military=as.numeric(gi$Military) gi$Unknown=as.numeric(gi$Unknown) gi$Graduation.Year=as.character(gi$Graduation.Year) str(gi) gi=na.omit(gi) dim(gi)`

# Results

Each line of exploration is supposed to be featured in one of the
Results sections. Make sure to change to more interesting section
headers!

\<\<\<\<\<\<\< HEAD ## Results 1-results after analyzing graduation rate
data

\`\`\`{r}{r setup, include=FALSE} #graduation rate vs district gr %>%
group_by(District) %>% summarize(Graduation.Rate= mean(Graduation.Rate))
%>% mutate(District= reorder(District, -Graduation.Rate, mean)) %>%
ggplot(aes(District, weight=Graduation.Rate)) + geom_bar() #graduation
rate summary by district name gr%>% group_by(District.Name)%>%
summarize(avg.gr=mean(gr$Graduation.Rate)) #graduation rate vs graduating class gr$Graduating.Class=as.character(gr\$Graduating.Class)
gr %>% group_by(Graduating.Class) %>%
ggplot(aes(Graduating.Class,y=Graduation.Rate))+geom_boxplot() #ames
graduation rate ames=gr%>%filter(District.Name==“Ames”)%>%
group_by(Graduating.Class)

ames%>%ggplot(aes(x=Graduating.Class, y=Total.Cohort,
fill=Graduation.Rate))+geom_bar(stat=“identity”)

ggplot(ames, aes(x=Graduating.Class)) + geom_bar( aes(y=Total.Cohort),
stat=“identity”, size=.1, fill=“plum”, alpha=.4)+
geom_point(aes(y=(Graduation.Rate-91)\*50), size=2) +
scale_y\_continuous( name = “Total.Cohort”, sec.axis = sec_axis(
trans=\~./50+91, name=“Graduation.Rate”) ) +theme_ipsum()



    For graduation rate vs district: based on the output graph, it is too concentrated to tell if there is any relationship between district and graduation rate.

    For graduation rate vs graduating class: Graduation class do have a little effect on the graduation but not significant. The median graduation and 75% quantile is not affecting by graduation year but the 25% quantile has a increasing trend from graduation class 2011-2016.

    For Ames graduation rate: both total cohort and graduation class are not factors that affects graduation rate


    ## Results 2

    ## Results 3-results after analyzing graduation intention

    ```{r}{r setup, include=FALSE}
    #summarize by graduation year
    gi%>%group_by(Graduation.Year)%>%
      summarize(
        sum.Diploma.Count=sum(Diploma.Count),
        sum.Private.4.Year.College=sum(Private.4.Year.College),
        sum.Public.4.Year.College=sum(Public.4.Year.College),
        sum.Private.2.Year.College=sum(Private.2.Year.College),
        sum.Community.College=sum(Community.College),
        sum.Other.Training=sum(Other.Training),
        sum.Employment=sum(Employment),
        sum.Homemaker=sum(Homemaker),
        sum.Military=sum(Military),
        sum.Unknown=sum(Unknown)
      )
    #graduation year vs diploma count
    ggplot(gi,aes(x=Graduation.Year, y=Diploma.Count))+geom_boxplot()
    #county vs diploma count
    gi %>%
      group_by(County) %>%
      ggplot(aes(x=County,y=Diploma.Count, color=Graduation.Year))+geom_bar(stat="identity")
    gi %>%
      group_by(County) %>%
      summarize(
        sum.diploma=sum(Diploma.Count))
        
    #pie charts
    ames2=gi%>%filter(District.Name=="Ames")%>%group_by(Graduation.Year)
    #2012
    slices12=c(64, 150, 63, 3, 1, 12, 1, 2, 18)
    lbls12=c("Private.4.Year.College", "Public.4.Year.College", "Private.2.Year.College", "Community.College", "Other.Training","Employment", "Homemaker",  "Military", "Unknown" )
    pie(slices12, labels = lbls12, main="Pie Chart of Graduates Intention")

    #2013
    slices13=c(49, 154, 76, 2, 7, 16, 1, 4, 0)
    lbls13=c("Private.4.Year.College", "Public.4.Year.College", "Private.2.Year.College", "Community.College", "Other.Training","Employment", "Homemaker",  "Military", "Unknown" )
    pie(slices13, labels = lbls13, main="Pie Chart of Graduates Intention")

    #2014
    slices14=c(54,157,54,4,3,20,0,8,0)
    lbls14=c("Private.4.Year.College", "Public.4.Year.College", "Private.2.Year.College", "Community.College", "Other.Training","Employment", "Homemaker",  "Military", "Unknown" )
    pie(slices14, labels = lbls14, main="Pie Chart of Graduates Intention")

    #2015
    slices15=c(36,159,48,1,5,29,2,0,1)
    lbls15=c("Private.4.Year.College", "Public.4.Year.College", "Private.2.Year.College", "Community.College", "Other.Training","Employment", "Homemaker",  "Military", "Unknown" )
    pie(slices15, labels = lbls15, main="Pie Chart of Graduates Intention")

    #2017
    slices17=c(35,137,79,0,5,32,0,4,13)
    lbls17=c("Private.4.Year.College", "Public.4.Year.College", "Private.2.Year.College", "Community.College", "Other.Training","Employment", "Homemaker",  "Military", "Unknown" )
    pie(slices17, labels = lbls17, main="Pie Chart of Graduates Intention")

Summarize by graduation year: Most students will go to
college(Private.4.Year.College, Public.4.Year.College,
Private.2.Year.College, Community.College are considered college) after
graduate. Employment is a popular choice for those who are not going to
college.

Graduation year vs diploma count: Graduation year is not a factor that
affects diploma count

County vs diploma count: County is a factor that affect diploma count.
County 77 has the most diploma in year 2012-2018.This does not mean
County 77 has the highest graduation rate because we don’t know how many
`Total.Cohort` (as it called in the graduation data) in this county

Pie charts: most graduates would like to go to college.4 year public
college is the most popular choice, about half of graduates will go to 4
year public college every year.

# Conclusions

Give a quick summary of your work. Here is the place to be a bit
critical and discuss potential limitations. Add a sentence on what else
you would have liked to include in your data exploration if you had more
time or more members in your team.

## Data source

Where does the data come from, who owns the data? Where are all the
scripts that you need to clean the data?

## References

List all resources you used.
