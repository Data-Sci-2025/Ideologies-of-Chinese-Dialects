
## Progress report 2
## Zihan Gao
## Nov 11, 2025

### Sharing scheme for the “found” portion of your data
My dataset includes video titles, name of uploaders, date, number of views that are publicly available on Bilibili. Because Bilibili’s terms of service do not allow redistribution of raw content, I will not upload the full dataset. Instead, I will share a file that includes metadata I extracted from the first page, as well as the video type, which I manually coded for each video. I will provide the code I used for data collection. This ensures reproducibility while not violating copyright and platform rules.

### Licensing decision and justification
I have chosen the CC-BY-4.0 (Creative Commons Attribution 4.0) license for my project. I chose this license because my original data is extracted from an publically accessible website page, and I allow other people to use and modify my data and code as long as they give proper credit.

### Data Type
- Existing, but I have added/changed a lot.

### Folder data_temporary_Nov11
I have tried another code and was able to extract the titles of the 42 most frequently viewed videos for all five dialects. They are in the folder `data_temporary_Nov11`. 

However, this morning, I updated the code to include the uploader’s name and the publication date for Cantonese. I haven't had a chance to update the code for the other dialects because I was manualing entering view counts.