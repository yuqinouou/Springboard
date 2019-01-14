# Capstone Project Data Wrangling

This document provide a quick summary of data wrangling steps took to import StackOverflow database for the capstone project.

1.  Download Raw Data File.  
  https://www.brentozar.com/archive/2015/10/how-to-download-the-stack-overflow-database-via-bittorrent/
  I downloaded the 137GB SQL Server 2008 file with Bittorrent that includes all historical posts up-to December 2017.
2.  Setup SQL Server in Docker. Run SQL Query and get tables I need for analysis. Here are more detailed documents.
  https://github.com/yuqinouou/Springboard/blob/master/Capstone_Project/doc/Database_Setup.md
  https://github.com/yuqinouou/Springboard/blob/master/Capstone_Project/notebook/StackOverflow_data_processing.ipynb
3. Loading Pre-processed tables and perform more data cleaning and wrangling.
  + Since `Posts` table is very large (>100G), split into questions table and answers table. Remove columns that's not going to be helpful.
  + Further split tables into text and info tables. Only keep `title` and `body` in text table since the volume of text data is huge.
  + Format time variables to datetime.
  + Replace missing total answer counts to zero.
  + To identify time to first answer, merge question and answer table by question.id and answer.parent_id. To identify time to first accepted answer, merge question and answer tables by question.accepted_id and answer.id. Remove error entries with question.creationtime > answer.creationtime.
  + To identify user location, merge users table with question table by user.id and question.creatorid. Since 80% of users haven't don't have location data, perform inner join and work on non-missing samples. Manually perform data cleaning on location string to standardize 'States', 'Cities' and country names in local language. Further aggregate to region level to perform analysis. Dropped all small categories.
  + Processed `tags` from an array of long string to an array of lists. Convert top 100 frequent tags into indicater variable in a (15m * 100) sparse matrix and calculated cosine distances between tags. Run a knn clustering algorithm to collaspe tags into 19 categories, such as ['css', 'html', 'html5']
  + (Unresolved) Identified a trend where rate of questions being answered dropped significantly around 2017/1/1, potentially a website policy change of a system update.
  + Plan to perform NLP on title and body.
