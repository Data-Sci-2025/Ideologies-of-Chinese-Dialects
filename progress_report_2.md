
## Progress report 2
## Zihan Gao
## Nov 12, 2025 (updated)
## Nov 11, 2025 (original)

### Sharing scheme for the “found” portion of your data
My dataset includes video titles, name of uploaders, date of publication, number of views, and URLs. All information is publicly available on Bilibili. Because Bilibili’s terms of service do not allow redistribution of raw content, I will not upload the full dataset. Instead, I will share a file that includes metadata I extracted from the first page, as well as the video type, which I manually coded for each video. I will provide the code I used for data collection. This ensures reproducibility while not violating copyright and platform rules.

### Licensing decision and justification
I have chosen the CC-BY-4.0 (Creative Commons Attribution 4.0) license for my project. I chose this license because my original data is extracted from an publically accessible website page, and I allow other people to use and modify my data and code as long as they give proper credit.

### Data Type
New continuing: the new code builds on the one in Progress report_Oct 28. 

### Folder data_temporary_Nov11 (written on Nov 12, 2025)
I have tried another code and was able to extract the titles of the 42 most frequently viewed videos for all five dialects. They are in the folder `data_temporary_Nov11`. 

However, this morning, I updated the code to include the uploader’s name and the publication date for Cantonese. I haven't had a chance to update the code for the other dialects because I was manualing entering view counts.

### Folder `preliminary_data` (updated on Nov 13, 2025) 
- I have successfully extracted the view counts with Dan's help. I also worked on extracting the URLs, and it partially worked. "Partically worked" means the data was extracted, but the entries were duplicated. Therefore, I will need to fix the code in the coming days. 
- I have exported an excel file for each dialect, and have combined them into one excel file.
- I also need to manually code the type of each video based on its content.

I apologize that the data collection is still not complete. I think the most challenging part of my project is the data collection, and I am almost there.

