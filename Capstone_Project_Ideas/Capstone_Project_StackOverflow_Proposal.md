# [DRAFT] Stack Overflow Content Quality Investigation: How Good was a Question Asked?  
#### Springboard Capstone Project Proposal  
##### _Yuqin Wei_  
##### _2018/11/28_  

### Background
Stack Overflow was used by millions of programmers on daily basis. Over the last 10 years, it provides a platform for 
programmers to share knowledge by posting questions and answers to programming topics. Large volume of user-generated 
data builds up the community and leads to its success.  
There are definitely hidden problems. Since all contents are 100% from user input, the quality of 
some posts can be questionable. This can be harmful to the whole community in a lot of ways. It creates
dummy information that is both a waste of resource and a distract to people from more useful information. Some posts
may be wrong and mis-leading. Some comments are purely people judging and hating each other and is totally irrelevant to topics.
The user rating system reflects the quality of contents. It provides a good measure of content quality from a collective of reviews - 
assuming the community is positive and on average have good judgements. With all those merits, it still requires time to accumulate 
a good number of reviews to fairly reflect the quality. In addition, the scores are heavily associated with the level of activity for
that topic, thus it is not fairly measuring quality of posts across different area.  
This report will aim at one specific type of posts - _"questions"_, and will try to explore the association between the quality of
a question with other features.  

### Study Questions
- What is the problem you want to solve?  
> There exist terrible posts on _Stack Overflow_. We would need to identify them.
- Who is your client and why do they care about this problem? In other words, what will your client DO or DECIDE based on your analysis that they wouldn’t have otherwise?
> The client is _Stack Overflow_. They care about this problem since bad posts do damage to the community and waste resource.
They can improve the quality of posts by closing bad posts and pushing good posts to users. They can also construct a guideline for 
content quality, given insights from data.
- What data are you going to use for this? How will you acquire this data?
> Stack Overflow shares open source data through [stackexchange](https://data.stackexchange.com/). Google Big Query provides a free data
repository but you have to use Google BigQuery to pull the data with limited quota. 
Credits to Brent Ozar who converts xml format to SQL Server database that is downloadable [here](https://www.brentozar.com/archive/2015/10/how-to-download-the-stack-overflow-database-via-bittorrent/).
- In brief, outline your approach to solving this problem (knowing that this might change later).
> 1. set up the local data server and pull relevant tables for investigations in python. 
> 2. perform data wrangling and EDA on numerical features.
> 3. perform some basic NLP on post titles and body text.
> 4. perform models to predict quality of posts
> 5. refine reports
- What are your deliverables? Typically, this would include code, along with a paper and/or a slide deck.
> A markdown report with codes, reports and data visualization.
