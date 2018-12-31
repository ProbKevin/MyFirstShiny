# A simple query system of Stat class

学统计的人搞出系统化的交互式数据可视化，语言不通是硬伤（对不起，只会R :see_no_evil:），Shiny为像我这样只会R不懂CSS的人提供了很大便利:relieved:23333...
关于入门Shiny呢，最好的学习路径还是[官方Tutorial](http://shiny.rstudio.com/tutorial/)
1. 首先建立一个排名查询函数
```r
## helper.r
getscore=function(phonenum){
  library(readxl)
  score=read_excel('stat5.xlsx',sheet = 2)
  newscore=score[1:6]
  return(outcome=newscore[which(score$key==phonenum),])
}
```
2. 搭建Shiny
```r
library(shiny)
## Shiny外观
ui <- fluidPage(
   titlePanel("STAT Class 2015 Rank"),
   sidebarLayout(
      sidebarPanel(
         textInput('text',h3('Phone Number'),value='Input your phone number')
      ),
      mainPanel(
        textOutput("ID"),
        textOutput('Name'),
        textOutput('SumScore'),
        textOutput('SumCredit'),
        textOutput('AvgScore'),
        textOutput('Rank')
        
      )
   )
)
source('helper.R')
## 输出函数
server <- function(input, output) {
   output$ID=renderText({
     paste('ID:',getscore(input$text)[1])
   })
   output$Name=renderText({
     paste('Name:',getscore(input$text)[2])
   })
   output$SumScore=renderText({
     paste('SumScore:',getscore(input$text)[3])
   })
   output$SumCredit=renderText({
     paste('SumCredit:',getscore(input$text)[4])
   })
   output$AvgScore=renderText({
     paste('AvgScore:',getscore(input$text)[6])
   })
   output$Rank=renderText({
     paste('Rank:',getscore(input$text)[5])
   })
}
```
3. 借助shinyapps.io发布Shiny App
可参考：[shinyapps.io user guide](https://docs.rstudio.com/shinyapps.io/index.html)，超详细~
```r
rsconnect::setAccountInfo(name="<ACCOUNT>", token="<TOKEN>", secret="<SECRET>")
```
