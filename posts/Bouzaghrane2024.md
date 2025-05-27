# Human mobility reshaped? Deciphering the impacts of the Covid-19 pandemic on activity patterns, spatial habits, and schedule habits

## Introduction

Human mobility is regular and predictable due to internal and external constraints. In the wake of COVID-19 pandemic, human behavior underwent significant shifts. Major changes are large scale adoption of telecommuting by employers and the increase in e-commerce adoptions by consumers. This reflects relaxation of spatio-temporal constraints. For example, employer can choose their preferred work environment, and could have more flexibility in their schedules.

There has been many researches studying impacts of pandemic on human mobility. But research on broader,long term impacts are needed, and impacts on relaxation of spatio=temporal constraints on schedule habits remains missing. So in this research, they used panel dataset to explore impact of pandemic and its associated relaxaiton of spario-temporal activity constrains on multiple dimensions of mobility behavior. They used used metrics characterizing human activity patterns, such as frequency of travel, radius of gyration, dwell-time, dwell-tim, trip timing, spatial exploration, and spatial diversity. 

## Data and methods
### Data
Thy used tracking data from a panel of U.S. smartphone users. This data infers individual check-ins at points of interest (POI). Each individual check-in at a POI includes infomration about panelit's arrival and departure times, category of the location visited, distance and time traveled, distance of POI to individual's home and work locations. They also have individual's personal informations such as gender and age. 

For accurate analysis, they undertook some preprocessing. They aggregated each POIs into geopgraphical locations based solely on their spatial proximity. They selected only individuals observed for long period of time with small change in tracking coverage over time. 

### Framework and metrics

Hypothesisi in thie research is that relaxation of spatio-temporal constraints following pandemic have borader influence on mobioity behavior, affecting not just activity patterns, but also spatial and schedule habits. Tney proposed a new metric capturing regularity of individual schedules over time, while controlling for day-of-week charactersitics. Below of explanation of metrics they used:

Activity patterns got impact throughout the pandemic. Travel frequency, travel distance, stay duration, trip timing are examples. For travel distance, they used radius of gyration. 

Spatial habits might also get changed if spatio-temporal activity constraints are relaxed. Individuals balance exploring new places with revisiting known ones, or there might be higher heterogeneity of use of geographical space. They used spatial exploration and spatial entropy.

Schedule habits would also change, since people will be less habitual in their schedules from week to week. They used cosine similarity to calculated similarity between any pair of dailiy schedules.
