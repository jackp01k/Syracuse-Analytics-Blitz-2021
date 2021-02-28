# 2021 Syracuse Analytics Blitz - University of Oklahoma Presentation

This was a blast to put together. The prompt was received on Thursday, February 18th 2021 and we had a 20 minute presentation on our solution 
due at 8pm CT Thursday, February 25th 2021.

## The Prompt

1. Determine the optimal pass/run ratio for given zones based on down, distance, and game situation.
2. Select two teams from the 2020 NFL season and describe their pass/run ratio in relation to our optimal ratio. Identify why they may have performed differently.
3. For these two teams, identify which zone of the field has the highest percentage of play-action passes, and compare these to the league-average rates. 

## The Solution

After initially attempting logistic regression in order to predict rates, we decided to go with an "observed" approach. To this end, we needed to identify situations in a game and analyze how teams had historically performed in these situations. This led us on a search for the definition of "situation." Once we could identify specific game situations, we would filter the larger play-by-play dataset into plays occuring only in a specific situation, and this would allow us to perform analysis on how teams performed. 

### Defining Situation

We eventually settled on the variables of down, distance, zone, situational time left in game, and win probability. Grouping variables into broader categories, specifically time left and win probability, would allow for larger play datasets to work from. 

### Analyzing Situation

After filtering down the dataset to a particular in-game situation, we were able to find conditional probabilities. Of particular interest here was the win probability added (WPA) given either a run play or a pass play, and the same for expected points added (EPA). Because the situations could produce very small datasets, Bayesian Bootstrapping would be used on these datasets in order to provide expected value estimates. With this, we could then assess the historically-advantageous playcall. Whichever was the better playcall given our play-calling logic, we could then use a random play from that filtered data set and treat it as our "playcall". 

### The Simulation

The simulation that we built was essentially a loop of the previous two sections. To begin a "drive", a random drive-beginning situation would be selected from the play-by-play data provided by nflfastR. This could be a punt, kickoff, blocked field goal, interception, etc. Our simulation would inherit key variables from this starting position, including timeouts remaining, the spread of the game, whether the game was played in a dome, the Vegas spread of the game, and many other key statistics. The reason for this was that we hoped to assess key metrics such as cumulative WPA as well as cumulative EPA. These metrics would go on to tell us how we performed relative to league or team averages. 

### Playcalling Logic

In order to assess the historical statistics we were gathering, we tried three different methods of comparing our pivotal statistic (EPA/WPA) for rushing and passing plays. The first two methods were deterministic comparisons of average WPA or EPA. Whichever playcall had the higher average value between these two would always be the playcall given to the simulation. For the third method, we took full advantage of the bootstrapping technique, and assessed the *probability* that a pass would produce a higher EPA than a run. This took into the variance of success that a playcall has, and ultimately produced the best logic. This would be the model we used for our team-wise comparison.

## Team Case Studies

We chose the Browns and the Cardinals for our case study, mostly because of Baker Mayfield and Kyler Murray (Oklahoma alumni). To assess these teams, we used their 2020 cumulative drive EPA and ran another simulation to see if we could produce a higher drive EPA than their mark. The difference between this simulation and the one before is, instead of using all NFL plays for a given situation, we were only using ones that were called by teams with similar strengths. We take PFF Grade categories to be accurate assessments of a team's strength in that facet, and filtering by all of the offensive ones gives us similar playcalls to those that our team of interest may call. For instance, the 2020 Browns may have the same relative offensive strength as the 2019 Titans. Therefore, a simulated playcall may choose a 2019 Titans play in order to move our simulated team down the field. 

### Results

For both teams, we prescribe higher passing rates across the board. However, for Cleveland, we suggest the exact same zone 5 (10-1 yardline) passing rate as we observed from them in the 2020 season. Therefore, we are led to believe that Cleveland called the optimal passing rate given their relative team strength. For the Cardinals, the simulation prescribed even higher passing rates than we did for Cleveland. This may be due to the fact that their run-blocking grades were particularly poor. 


## A Final Word

This is a major thank you from the newly-founded OU Sports and Data Analytics Club. Prior to this competition we had no formal group, and some of us didn't know of each other outside of Twitter. This competition gave us an opportunity to come together and share knowledge as well as establish a formal club which we are hoping will grow in its popularity here at OU. We look forward to future analytics competitions, and we will always remember the fun we had with our first one: the 2021 Syracuse Analytics Blitz.
