# How Good was a Question Asked?
### A _StackOverflow_ Content Quality Study Proposal 

Yuqin Wei  
2018/11/28   

### Background
Stack Overflow features questions and answers on a wide range of topics in computer programming. Questions on Stack Overflow were often among the first search results from Google, and it has been one of the most popular resources for programmers trying to solve a common problem. Large volume of user-generated data builds up the community and leads to its success.  

But on the other hand, since all contents are from user input, the quality of posts can be questionable. This will be harmful to the whole community in a lot of ways. It creates dummy contents that is both a waste of resource and a distract to people from more useful information. Some posts may provide wrong and mis-leading information. Some comments are purely people judging and hating each other and being irrelevant to topics.  

This is to propose an investigation aiming at "questions", and to explore the association between the quality of a question with other features.  

### Study Questions
- What is the problem you want to solve?  
	+ The goal is to characterize what kind of question posts are "good" questions on _StackOverflow_.
- Who is your client and why do they care about this problem? In other words, what will your client DO or DECIDE based on your analysis that they wouldn’t have otherwise?  
	+ The client is _StackOverflow_. They care about this problem since bad posts can damage the community in multiple ways.
Given a system to identify bad questions, even they might not directly close the post, they can still providing guidelines for users to improve their post quality through a automated system. 
- What data are you going to use for this? How will you acquire this data?  
	+ Stack Overflow shares all historical post data through [stackexchange](https://data.stackexchange.com/). Google provides free access to the data but you have to query through Google BigQuery to pull the data with limited quota.   
Credits to Brent Ozar who converts xml format to SQL Server database that can be downloaded from [here](https://www.brentozar.com/archive/2015/10/how-to-download-the-stack-overflow-database-via-bittorrent/).
- In brief, outline your approach to solving this problem (knowing that this might change later).  
	1. set up the local data server and pull relevant tables for investigations. 
	2. familirize with the data by performing data wrangling and EDA.
		+ Perform data cleaning.
		+ Identify quality measures that describe popularity of a question, such as question score, time to first answer, number of comments received.
		+ Construct features that might be strong predictors for question quality, such as time, tags, keywords, title length, etc.  
		+ Display any other patterns identified from data and come up with a reasonable story.
	3. perform basic statistical tests to prove assumptions made. 
	4. perform models to predict quality of posts.
	5. refine final reports.
- What are your deliverables? Typically, this would include code, along with a paper and/or a slide deck.  
	+ Jupyter notebooks for interim and final reports.
