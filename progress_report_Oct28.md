# data
Zihan Gao

## Progress report 1

## Zihan Gao

## Oct 28, 2025

### My study

#### Title

Dialects reduce attractiveness?: A Study of Language Attitudes and
Stereotypes toward Chinese Dialects on Social Media

#### Background and goal

Some Chinese varieties are considered pleasant-sounding, and people hope
to learn them, while others are perceived as “rustic” or “funny.”
Therefore, this study aims to explore how different Chinese varieties
and accents are perceived on social media.

#### Plan

I plan to analyze videos and comments on Bilibili
(https://www.bilibili.com/), a video website featuring danmu — flying
comments overlaid on the video.

##### Step 1

My first step is to analyze: - the number of videos related to each
dialect - the number of views of each video - the number of likes of
each video - the keywords associated with each dialect or variety (e.g.,
some dialects are connected with keywords like “funny,” while others are
associated with “I wanna learn this dialect”).

##### Step 2

My second step is to extract comments under the top 42 (i.e.,
first-page) most-reviewed videos for each dialect and rate the
positivity of the comments.

### The current progress

Because I did not receive any response from the API request, I started
working on extracting data directly from the website. I tried applying
the code from Chapter 24 Web Scraping, but it didn’t work for my case,
so I asked ChatGPT for help. I tried to ask for specific questions about
rvest. ChatGPT provided several possible solutions. Some of them worked,
and some didn’t. The following is what worked.

``` r
# load packages
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
    ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ✔ purrr     1.1.0     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```


    Attaching package: 'rvest'

    The following object is masked from 'package:readr':

        guess_encoding

``` r
library(dplyr)

# name keyword (Cantonese)
keyword <- "粤语"
url <- paste0("https://search.bilibili.com/all?keyword=", URLencode(keyword))

# read the website page
html <- read_html(url)

# read titles
titles <- html |> 
  html_elements(".bili-video-card__info--tit") |>
  html_text2()

titles
```

     [1] "超级棒的粤语教程"                                                                                                     
     [2] "【30分钟合集】“那些只打高端局的粤语歌曲”"                                                                             
     [3] "超高校级の美少女粤语唱见"                                                                                             
     [4] "【私皮粤v】被甜晕啦"                                                                                                  
     [5] "【Playlist】那些只打高端局的粤语歌 ｜ 港乐｜ 学习 ｜ 通勤 ｜ 写作业 ｜ 运动 ｜ 开车 ｜ 歌单 ｜ 合集"                  
     [6] "40天零基础学会粤语"                                                                                                   
     [7] "【无损音质100首】粤语经典风云榜TOP100 聆听港风灵魂的旋律盛宴"                                                         
     [8] "【边睡边记】粤语核心300单词︱实用高频词"                                                                              
     [9] "30min权威粤语歌曲合集. |（洗澡&开车&写作业&通勤&小说）"                                                               
    [10] "【YouTube上最好的粤语教学】高效学粤语（8集全）"                                                                       
    [11] "粤语快速入门 粤语零基础"                                                                                              
    [12] "《粤语拼音全讲解》密密带你系统学习粤语拼音"                                                                           
    [13] "【自制粤语正字字幕】TVB粤语多啦A夢 【黄昕瑜版/720P】不定期更新"                                                       
    [14] "“何事落到这收场”【粤语emo合集】"                                                                                      
    [15] "“那些超好听的宝藏粤语歌”"                                                                                             
    [16] "广东话配音有多绝！广东人笑疯了哈哈哈哈哈哈哈哈哈哈"                                                                   
    [17] "【Playlist.粤语歌单】“这恨海情天的爱恋，还是和粤语最适配”｜酸涩/伤感/氛围｜自习独处放松|私藏歌单"                     
    [18] "「Playlist｜粤语歌单」熟悉的粤语旋律，一响起就把人拉回，再也回不去的那年。"                                           
    [19] "潜行狙击 (2011) 粤语版 30集全"                                                                                        
    [20] "香港綜藝：攻你上大學"                                                                                                 
    [21] "每天学点粤语｜轻松从粤语小白进阶为粤语达人【完结】"                                                                   
    [22] "经典老歌专场〔粤语老歌带你飞〕"                                                                                       
    [23] "好听的十首粤语歌曲珍藏版"                                                                                             
    [24] "56分钟经典粤语歌曲合集"                                                                                               
    [25] "【4K】 粤语小猪佩奇 合集 共410集"                                                                                     
    [26] "喜爱夜蒲3粤语全集"                                                                                                    
    [27] "【无损音乐】盘点华语超好听的粤语经典合集"                                                                             
    [28] "𝐏𝐥𝐚𝐲𝐥𝐢𝐬𝐭 º 粤语歌单🎵秋天该很好，你若尚在场🍂秋日限定 释怀 回忆 港乐"                                                 
    [29] "那些传唱度过亿的100首粤语神曲，真的抑制不住DNA了！"                                                                   
    [30] "【粤语经典合集】开口即王炸的粤语神曲"                                                                                 
    [31] "每天学点粤语｜轻松从粤语小白进阶为粤语达人【完结】"                                                                   
    [32] "粤语天花板 痴情粤语除了你，我心里什么都没有了#粤语歌 #经典粤语 #经典歌曲 #歌单种草计划 #音乐分享4"                    
    [33] "学粤语学广东话最常用句子200句（第1集）粤语学习｜粤语教学｜学习粤语｜Cantonese Lesson"                                 
    [34] "鸭王-粤语"                                                                                                            
    [35] "［30分钟粤语串烧］90后不得不听的广东歌（1）"                                                                          
    [36] "【粤语合集】“你总以为机会无限，所以从不珍惜眼前人”"                                                                   
    [37] "（懷緬歡喜哥）使命徒步行者1 粵語"                                                                                     
    [38] "2025热门粤语金曲榜TOP10《晚风心里吹》《富士山下》《不负众望》《吴哥窟》《处处吻》《喜欢你》《月半小夜曲》《光辉岁月》"
    [39] "【PLAYLIST】｜宝藏粤语歌单｜港乐｜适合Citywalk/通勤/散步/放松时听的粤语歌单"                                          
    [40] "小玉的万圣节派对 | 粤语"                                                                                              
    [41] "B站最权威的少御"                                                                                                      
    [42] "香港電台【有種人生】"                                                                                                 

**I have a lot of questions about the code above. It only produces the
correct output when I leave it unchanged; whenever I modify anything, it
fails to give the right result.**

#### one step further (did not work)

The code above was able to extract 42 titles with the keyword
“Cantonese” from the first page. When I failed to navigate to later
pages. Some code returned NULL, and others did not run at all.

``` r
# Again, the following code was generated by ChatGPT.
# This one does not work
keyword <- "粤语"
pages <- 1:3  # 比如抓前三页
all_titles <- c()

for (p in pages) {
  url <- paste0("https://search.bilibili.com/all?keyword=", URLencode(keyword), "&page=", p)
  html <- read_html(url)
  titles <- html %>%
    html_elements(".bili-video-card__info--tit") %>%
    html_text2()
  all_titles <- c(all_titles, titles)
}
all_titles
```

#### another step further (did not work)

I also tried to extract view counts and like counts, but I haven’t
figured that out yet.

### Next steps

- figure out how to turn pages
- get view counts and like counts
- get comment counts (it seems harder)
- collect data for other dialects
- write the results to a CSV file
- extract keywords (how should I tokenize them as there is no space in
  Chinese)

***The following code contains both what I wrote based on the chapter,
and what ChatGPT gave me. None of them worked.***

``` r
# read the HTML into R
# html <- read_html("https://www.bilibili.com/")
```

``` r
# page_title <- html |> 
#   html_element("title") |> 
#   html_text2()
```

``` r
# cantonese <- read_html("https://search.bilibili.com/all?keyword=粤语")

# cantonese <- read_html("https://search.bilibili.com/all?keyword=%E7%B2%A4%E8%AF%AD&from_source=webtop_search&spm_id_from=333.1007&search_source=5")
```

``` r
# titles <- cantonese %>%
#   html_elements(".bili-video-card__info--tit") %>%
#   html_text2()
```

``` r
# library(httr)
# library(jsonlite)

# keyword <- "粤语"
# url <- paste0(
#   "https://api.bilibili.com/x/web-interface/search/all/v2?",
#   "keyword=", URLencode(keyword),
#   "&pn=1&ps=20"
# )

# res <- GET(url, user_agent("Mozilla/5.0"))  # 模拟浏览器
# json <- fromJSON(content(res, "text", encoding = "UTF-8"))

# str(json, max.level = 3)  # 查看数据结构
```

``` r
# library(httr)
# url <- "https://api.bilibili.com/x/web-interface/search/all/v2?keyword=粤语&pn=1&ps=20"

# res <- GET(
#   url,
#   user_agent("Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36"),
#   add_headers(
#     Referer = "https://www.bilibili.com/",
#     Accept = "application/json"
#   )
# )

# content(res, "text", encoding = "UTF-8")
```
