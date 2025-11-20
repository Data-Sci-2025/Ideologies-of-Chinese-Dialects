# Progress Report 3 Code
Zihan
2025-11-18

**Code: Existing**

In this file, I updated some code based on the previous one in Progress
Report 2. Then I added a new chunk.

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

### Updated code with URLs extracted

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

# for some reasons, there are two same results (URL) for each video, so it generated 84 results instead of 42.

# remove the reduplicated ones
URL.df <- data.frame(URL) |> 
  distinct()

# build a dataframe
df <- data.frame(
  title = titles,
  uploader = uploader,
  date  = dates,
  views = views,
  URL = unique(URL)
)

# write the dataframe to a .csv file
write_csv(df, "preliminary_data/Cantonese.csv")
```

## Code to visit websites that require authentication

**the following code runs, but it does not work–I still am not logged
in.**

``` r
library(chromote)
b <- ChromoteSession$new()
b$view()
b$go_to("https://www.bilibili.com/")
```

``` r
cookies <- b$Network$getCookies()
str(cookies)
saveRDS(cookies, "cookies.rds")
```

``` r
library(chromote)
b <- ChromoteSession$new()
b$view()
cookies <- readRDS("cookies.rds")
b$Network$setCookies(cookies = cookies$cookies)
# Navigate to the app that requires a login
b$go_to("https://www.bilibili.com/")
b$screenshot()
```
