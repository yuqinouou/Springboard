# How Good was a Question Asked?
### A _StackOverflow_ Content Quality Study Proposal 

Yuqin Wei  
2019/01/15   

### Background
Stack Overflow features questions and answers on a wide range of topics in computer programming. Questions on Stack Overflow were often among the first search results from Google, and it has been one of the most popular resources for programmers trying to solve a common problem. Large volume of user-generated data builds up the community and leads to its success.  

But on the other hand, since all contents are user-generated, the quality of part of the posts can be questionable. This can be harmful to the whole community in a lot of ways. It creates dummy contents that are both a waste of resource and a distraction to people from more useful information. Some posts may provide wrong and mis-leading information, damage the intergrity and reputation of the community. Some comments are purely people judging and hating each other and being irrelevant to topics. There can also be questions that could be valueable, but not properly asked or presented, and got no people's attention.

This is to propose an investigation aiming at "questions", and to explore the association between the quality of a question with other features. The goal is to identify potential reasons why some questions are 'bad' and not being appreciated, and to make suggestions to improve the system. 

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
	- See Next Section.	
- What are your deliverables? Typically, this would include code, along with a paper and/or a slide deck.  
	+ Jupyter notebooks for interim and final analysis.
	+ Final report.

### Study Plan
1. Data Extraction. 
	+ Setup database in MSSQLServer and load in StackOverflow database images.
	+ Query relevant tables for analysis, convert and stored in CSV. 
2. Data ETL and EDA.
	+ Extract and investigate potential question quality measures:
		- time to first answer
		- time to first accepted answer
		- time adjusted score
		- time adjusted answer counts
		- time adjusted comment counts
		- time adjusted favorate counts
	+ Extract and investigate potential features predicts quality measures:
		- day of the week
		- tags/categories
		- number of tags
		- title word count
		- body word count
		- internal link to other questions
		- use of external link
		- use of formula
		- use of code chunk
		- Image files
		- Use of polite words - 'Please', 'Thank you'
		- (More)
	+ Invesigate association between features and quality measures.
	+ Display any other patterns identified from data and come up with a reasonable story.
3. Statistical Reasoning.
	+ Perform basic statistical tests to check hypothesis made. 
4. Predictive Modeling.
	+ Perform machine learning models to build the association between features and outcomes.
	+ Extract feature coefficients or feature importance measure to select important factors.
	+ Run prediction. Identify poor quality posts and make suggestions.
5. Conduct final report.

