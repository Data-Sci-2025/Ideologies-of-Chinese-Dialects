# Progress Report 2_Data
Zihan Gao

## Progress report 2

## Zihan Gao

## Update version Nov 12

***Please find the original version at the bottom***

### Project Title

Dialects reduce attractiveness?: A Study of Language Attitudes and
Stereotypes toward Chinese Dialects on Social Media

### Data Type

New continuing: code in this file builds on the one in Progress
report_Oct 28.

### Update of code

In the previous progress report, I managed to extract the titles of
videos on the first page of `bilibili.com`. During the past week, I
worked on figuring out how to turn pages and extract the number of views
for each video. However, I wasn’t able to make it work. Because the
website is dynamic, the code always returned `character(0)`. Therefore,
I tried sorting the results by view count and extracting only the titles
from the first page. With help from ChatGPT, this approach worked. The
code is shown below.

#### Acknowledgement of ChatGPT

The textbook suggested using Chromote for dynamic pages, but it did not
go into detail. Therefore, I asked ChatGPT to generate possible code
using `Chromote` and `rvest`. Some of the code produced blank columns,
or returned `character(0)`, or returned `NA`. I was able to fix some
with Dan’s help. I will provide a record of my conversation with ChatGPT
in a separate file. (I have not uploaded it yet. I did not write the
conversation down before today, but I can still find it on the webpage.
I need some time to organize it. I will make sure to upload it during
Thanksgiving!)

``` r
library(rvest)
library(chromote)
library(dplyr)
```


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

``` r
library(writexl)
library(readxl)
```

#### Extract data from keyword *Cantonese*

``` r
# create Chromote session
b <- ChromoteSession$new()

# name keyword (Cantonese), and rank videos by views
keyword <- "粤语" # Cantonese
url <- paste0(
  "https://search.bilibili.com/all?keyword=",
  URLencode(keyword),
  "&order=click"  # rank by views
)

# open the page
b$Page$navigate(url)

# wait for the page to load (6 seconds)
Sys.sleep(6)

# get the HTML page
html_text <- b$Runtime$evaluate("document.documentElement.outerHTML")$result$value

# read the HTML
html <- read_html(html_text)

# close the window
b$close()
```

##### Extract the titles, name of uploaders, date when the video was uploaded, view counts, and URLs

``` r
# select all video cards (card = video entry)
cards <- html_elements(html, ".bili-video-card")

# extract video titles 
titles <- cards |>
  html_element(".bili-video-card__info--tit") |> 
  html_text2()

# extract uploaders (authors of each video)
uploader <- cards |> 
  html_element(".bili-video-card__info--author") |> 
  html_text2()

# extract upload dates
dates  <- cards |> 
  html_element(".bili-video-card__info--date") |> 
  html_text2()

# extract view counts
views <- cards |> 
  html_element(".bili-video-card__stats--item") |>
  html_element("span:first-of-type") |> 
  html_text2()

# extract video URLs
URL <- cards |> 
  html_elements("a[href*='bilibili.com/video']") |> 
  html_attr("href")

# build a dataframe
df <- data.frame(
  title = titles,
  uploader = uploader,
  date  = dates,
  views = views,
  URL = URL
)

# write the dataframe to an Excel file
# I wanted to manually add a new column, so saving to an .xlsx file works better than a .csv file.
write_xlsx(df, path = "preliminary_data/Cantonese_videos2.xlsx")
```

Then I repeated the same process 4 times for other four
dialects–Jiangsu, Shandong, Zhejiang, and Sichuan. The code is in the
section – *Data extraction for the other four dialects*.

#### Combine the files into one

``` r
# read in 5 excel files
Guangdong <- readxl::read_excel("preliminary_data/Cantonese_videos2.xlsx")
Jiangsu <- readxl::read_excel("preliminary_data/Jiangsu_videos.xlsx")
Shandong <- readxl::read_excel("preliminary_data/Shandong_videos.xlsx")
Zhejiang <- readxl::read_excel("preliminary_data/Zhejiang_videos.xlsx")
Sichuan <- readxl::read_excel("preliminary_data/Sichuan_videos.xlsx")

# combine them into one dataframe
top5 <- bind_rows(Guangdong, Jiangsu, Shandong, Zhejiang, Sichuan)

# write the dataframe to an Excel file
# I was about to clean the data but I am too tired now. I will just save them for now.
write_xlsx(top5, path = "preliminary_data/top5.xlsx")
```

### Future steps

- Fixing the URL
  - Now each URL reduplicates twice.
- Data cleaning
  - split Y, M, D
- Coding videos based on their content (e.g., music, humor, mocking,
  etc.)

# The end

**Note for the reviewer: The useful information is provided above. Below
is the duplicated code for the other four dialects and the previous
version.**

### Data extraction for the other four dialects

#### Jiangsu dialect

``` r
# create Chromote session
b <- ChromoteSession$new()

# name keyword (Jiangsu dialect), and rank videos by views
keyword <- "江苏话" # Jiangsu dialect
url <- paste0(
  "https://search.bilibili.com/all?keyword=",
  URLencode(keyword),
  "&order=click"  # rank by views
)

# open the page
b$Page$navigate(url)

# wait for the page to load (6 seconds)
Sys.sleep(6)

# get the HTML page
html_text <- b$Runtime$evaluate("document.documentElement.outerHTML")$result$value

# read the HTML
html <- read_html(html_text)

# close the window
b$close()
```

``` r
# select all video cards (card = video entry)
cards <- html_elements(html, ".bili-video-card")

# extract video titles 
titles <- cards |>
  html_element(".bili-video-card__info--tit") |> 
  html_text2()

# extract uploaders (authors of each video)
uploader <- cards |> 
  html_element(".bili-video-card__info--author") |> 
  html_text2()

# extract upload dates
dates  <- cards |> 
  html_element(".bili-video-card__info--date") |> 
  html_text2()

# extract view counts
views <- cards |> 
  html_element(".bili-video-card__stats--item") |>
  html_element("span:first-of-type") |> 
  html_text2()

# extract video URLs
URL <- cards |> 
  html_elements("a[href*='bilibili.com/video']") |> 
  html_attr("href")

# build a dataframe
df <- data.frame(
  title = titles,
  uploader = uploader,
  date  = dates,
  views = views,
  URL = URL
)

# write the dataframe to an Excel file
# I wanted to manually add a new column, so saving to an .xlsx file works better than a .csv file.
write_xlsx(df, path = "preliminary_data/Jiangsu_videos.xlsx")
```

#### Shandong dialect

``` r
# create Chromote session
b <- ChromoteSession$new()

# name keyword (Shandong dialect), and rank videos by views
keyword <- "山东话" # Shandong dialect
url <- paste0(
  "https://search.bilibili.com/all?keyword=",
  URLencode(keyword),
  "&order=click"  # rank by views
)

# open the page
b$Page$navigate(url)

# wait for the page to load (6 seconds)
Sys.sleep(6)

# get the HTML page
html_text <- b$Runtime$evaluate("document.documentElement.outerHTML")$result$value

# read the HTML
html <- read_html(html_text)

# close the window
b$close()
```

``` r
# select all video cards (card = video entry)
cards <- html_elements(html, ".bili-video-card")

# extract video titles 
titles <- cards |>
  html_element(".bili-video-card__info--tit") |> 
  html_text2()

# extract uploaders (authors of each video)
uploader <- cards |> 
  html_element(".bili-video-card__info--author") |> 
  html_text2()

# extract upload dates
dates  <- cards |> 
  html_element(".bili-video-card__info--date") |> 
  html_text2()

# extract view counts
views <- cards |> 
  html_element(".bili-video-card__stats--item") |>
  html_element("span:first-of-type") |> 
  html_text2()

# extract video URLs
URL <- cards |> 
  html_elements("a[href*='bilibili.com/video']") |> 
  html_attr("href")

# build a dataframe
df <- data.frame(
  title = titles,
  uploader = uploader,
  date  = dates,
  views = views,
  URL = URL
)

# write the dataframe to an Excel file
# I wanted to manually add a new column, so saving to an .xlsx file works better than a .csv file.
write_xlsx(df, path = "preliminary_data/Shandong_videos.xlsx")
```

#### Sichuan dialect

``` r
# create Chromote session
b <- ChromoteSession$new()

# name keyword (Sichuan dialect), and rank videos by views
keyword <- "四川话" # Sichuan dialect
url <- paste0(
  "https://search.bilibili.com/all?keyword=",
  URLencode(keyword),
  "&order=click"  # rank by views
)

# open the page
b$Page$navigate(url)

# wait for the page to load (6 seconds)
Sys.sleep(6)

# get the HTML page
html_text <- b$Runtime$evaluate("document.documentElement.outerHTML")$result$value

# read the HTML
html <- read_html(html_text)

# close the window
b$close()
```

``` r
# select all video cards (card = video entry)
cards <- html_elements(html, ".bili-video-card")

# extract video titles 
titles <- cards |>
  html_element(".bili-video-card__info--tit") |> 
  html_text2()

# extract uploaders (authors of each video)
uploader <- cards |> 
  html_element(".bili-video-card__info--author") |> 
  html_text2()

# extract upload dates
dates  <- cards |> 
  html_element(".bili-video-card__info--date") |> 
  html_text2()

# extract view counts
views <- cards |> 
  html_element(".bili-video-card__stats--item") |>
  html_element("span:first-of-type") |> 
  html_text2()

# extract video URLs
URL <- cards |> 
  html_elements("a[href*='bilibili.com/video']") |> 
  html_attr("href")

# build a dataframe
df <- data.frame(
  title = titles,
  uploader = uploader,
  date  = dates,
  views = views,
  URL = URL
)

# write the dataframe to an Excel file
# I wanted to manually add a new column, so saving to an .xlsx file works better than a .csv file.
write_xlsx(df, path = "preliminary_data/Sichuan_videos.xlsx")
```

#### Zhejiang dialect

``` r
# create Chromote session
b <- ChromoteSession$new()

# name keyword (Zhejiang dialect), and rank videos by views
keyword <- "浙江话" # Zhejiang dialect
url <- paste0(
  "https://search.bilibili.com/all?keyword=",
  URLencode(keyword),
  "&order=click"  # rank by views
)

# open the page
b$Page$navigate(url)

# wait for the page to load (6 seconds)
Sys.sleep(6)

# get the HTML page
html_text <- b$Runtime$evaluate("document.documentElement.outerHTML")$result$value

# read the HTML
html <- read_html(html_text)

# close the window
b$close()
```

``` r
# select all video cards (card = video entry)
cards <- html_elements(html, ".bili-video-card")

# extract video titles 
titles <- cards |>
  html_element(".bili-video-card__info--tit") |> 
  html_text2()

# extract uploaders (authors of each video)
uploader <- cards |> 
  html_element(".bili-video-card__info--author") |> 
  html_text2()

# extract upload dates
dates  <- cards |> 
  html_element(".bili-video-card__info--date") |> 
  html_text2()

# extract view counts
views <- cards |> 
  html_element(".bili-video-card__stats--item") |>
  html_element("span:first-of-type") |> 
  html_text2()

# extract video URLs
URL <- cards |> 
  html_elements("a[href*='bilibili.com/video']") |> 
  html_attr("href")

# build a dataframe
df <- data.frame(
  title = titles,
  uploader = uploader,
  date  = dates,
  views = views,
  URL = URL
)

# write the dataframe to an Excel file
# I wanted to manually add a new column, so saving to an .xlsx file works better than a .csv file.
write_xlsx(df, path = "preliminary_data/Zhejiang_videos.xlsx")
```

### Data Type

- Existing, but I have added/changed a lot.
- I also want to note that I asked ChatGPT for a lot of help because I
  really had no idea how to extract data from a dynamic website. I did
  try using the textbook, including the code, packages, and functions
  mentioned there, but most of the current code was written by ChatGPT.
  There are still two lines that I don’t fully understand.

## original version

## Nov 11, 2025

### Notes at the beginning

I wasn’t able to finish the data collection process by noon on November
11. I will list one example and complete the related questions first,
then apply the same procedure to the other four dialects.

### Data Type

- Existing, but I have added/changed a lot.
- I also want to note that I asked ChatGPT for a lot of help because I
  really had no idea how to extract data from a dynamic website. I did
  try using the textbook, including the code, packages, and functions
  mentioned there, but most of the current code was written by ChatGPT.
  There are still two lines that I don’t fully understand.

### My study

#### Title

Dialects reduce attractiveness?: A Study of Language Attitudes and
Stereotypes toward Chinese Dialects on Social Media

### Update of code

In the previous progress report, I managed to extract the titles of
videos on the first page. During the past week, I worked on figuring out
how to turn pages and extract the number of views for each video.
However, I wasn’t able to make it work. Because the website is dynamic,
the code always returned `character(0)`. Therefore, I tried sorting the
results by view count and extracting only the titles from the first
page. With help from ChatGPT, this approach worked. The code is shown
below.

#### Step 1 extract the titles, name of uploaders, and the date when the video was uploaded

``` r
library(rvest)
library(chromote)
library(dplyr)
library(writexl)
```

``` r
# create Chromote session
b <- ChromoteSession$new()

# name keyword (Cantonese), and rank by number of views
keyword <- "粤语"
url <- paste0(
  "https://search.bilibili.com/all?keyword=",
  URLencode(keyword),
  "&order=click"  # rank by views
)

# open the page
b$Page$navigate(url)

# wait time
Sys.sleep(6)

# get the HTML
html_text <- b$Runtime$evaluate("document.documentElement.outerHTML")$result$value
html <- read_html(html_text)

# I actually don't understand this step below
cards <- html_elements(html, ".bili-video-card")

# extract the keyword
titles <- cards |> html_element(".bili-video-card__info--tit") |> html_text2()

# ups    <- cards |> html_element(".bili-video-card__info--owner") |> html_text2()
ups    <- cards |> html_element(".bili-video-card__info--author") |> html_text2()
dates  <- cards |> html_element(".bili-video-card__info--date") |> html_text2()

# views  <- cards |> html_element("span[data-v-37adbc26]") |> 
#   html_text2()
# views  <- cards |> html_element("[attribute$="万"]") |> 
#   html_text2()

dates  <- cards |> html_element(".bili-video-card__stats--item") |>
  html_element("span:first-of-type") |> 
  html_text2()


# I don't understand the code below.
# It seems to give a back up option.
# if (length(ups) == 0) {
#  ups <- cards |> html_element(".bili-video-card__info--author") |> html_text2()
#}
if (length(dates) == 0) {
  dates <- cards |> html_element(".bili-video-card__info--pubdate") |> html_text2()
}

# build the dataframe
df <- data.frame(
  title = titles,
  up    = ups,
  date  = dates,
  views = views,
  stringsAsFactors = FALSE
)

# output
write_xlsx(df, path = "Cantonese_videos2.xlsx")

# close the window
b$close()
```

#### Step 2 input the view count (manually?)

I found the view count in the HTML code, but I wasn’t able to extract
it. I’ll keep working on it a bit more. It should be extractable.

I actually entered the view counts manually, but I accidentally ran the
above code again and the file was overwritten 😭. I’ll work on
extracting it from the HTML language instead of entering manually.

#### Step 3 code the video type

I had also coded different videos based on their content (e.g., music,
humor, mocking, etc.), but unfortunately, I lost that data as well
because the file was overwritten.

#### Step 4 decide the dialects

Another step I completed was deciding which dialects to focus on.
Instead of using linguistically defined Chinese dialects or varieties, I
decided to use provinces. I found data on the GDP of each province and
ranked them from highest to lowest. I will focus on the top five
provinces.

### Step 5 repeat the previous steps

I have tried another code and was able to extract the titles of the 42
most frequently viewed videos for all five dialects. But this morning, I
updated the code to include the uploader’s name and the publication date
for Cantonese. I haven’t had a chance to update the code for the other
dialects because I was manualing entering view counts.
