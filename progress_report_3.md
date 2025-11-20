
## Progress report 3
### Zihan Gao
### Nov 20

After the last report, I successfully extracted the URLs for the videos on the first page.

However, after talking with Dan during office hours on Monday, I realized that I need to extract some additional data:
- titles containing the keywords
- comments under the videos

However, several problems came up while I was extracting the data. For the titles, the website’s default search setting is to look for the keyword in both the tags and the titles. I did managed to search titles that contained the keyword, but whenever I ranked the videos by views, the results went back to the default setting.

My current solution is to figure out how to turn pages, so that I can extract more video titles from the following pages, and then select the ones that contain the keyword.

For the comments, the website only allowed credentialed users to view the comments. I tried the code under “Websites that require authentication.” It ran, but I still could not log in. There is always a verification code required for login, and I am not sure if that caused the issue.

Final note: My Wednesdays are very hectic. I only worked on these two unresolved problems last night, and I will work on the Chinese tokenizer tonight.