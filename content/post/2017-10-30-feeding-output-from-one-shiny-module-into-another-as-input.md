---
title: Feeding output from one Shiny module into another as input
author: Kohleth Chia
date: '2017-10-30'
slug: feeding-output-from-one-shiny-module-into-another-as-input
categories:
  - r
tags:
  - shiny
---

Shiny has introduced the concept of <a href="//shiny.rstudio.com/articles/modules.html" target="_blank">modules</a> which lets you break up complex shiny code into small, isolated (self-contained), re-usable chunk.

The idea is great but the implementation can be a bit different to learn. One issue that I had earlier is how to, from a first module, which is a renderUI object, return a reactive value which is then fed as input into another renderUI object.

And just like many other coding problem, the best learning resource is often just a working example.

Below is a simple example. First, there is a selectize list. Once you have selected a value from the list, a set of radio buttons are then reactively shown, and once a value is picked, a plot is reactively drawn.

```
library(shiny)

## define modules -------------------------------

myselectInput=function(id){
  ns=NS(id)
  uiOutput(ns("selectize"))
}

myselect=function(input,output,session){
  x=levels(CO2$Type)
  ns=session$ns
  output$selectize=renderUI({
    selectizeInput(inputId = ns("type"),
                   label="pick a type",
                   choices=c("Pick a plant"="",x))
  })
  return(reactive(input$type))
}

myradioInput=function(id){
  ns=NS(id)
  uiOutput(ns("radio"))
}

myradio=function(input,output,session,.type){

  ns=session$ns
  output$radio=renderUI({
    req(.type())
    x=levels(subset(CO2,Type==.type())$Plant)
    radioButtons(inputId=ns("plant"),
                            label="Pick a plant",
                            choices=x,selected=character(0))
  })
  
  return(reactive(input$plant))
}

## main Shiny files-------------------------

ui <- fluidPage(
  myselectInput("demo"),
  myradioInput("demo"),
  plotOutput("plot")
)

server <- function(input, output, session) { 
  type=callModule(myselect,"demo")
  plant=callModule(myradio,"demo",type)
  output$plot=renderPlot({
    req(plant())
    req(type())
    d=subset(CO2,Plant==plant())
    plot(conc~uptake,data=d,main=paste(type(),plant(),sep=": "))
  })
} 

# Run the application 
shinyApp(ui = ui, server = server)
```

Feel free to try out the code above. Below is an animated screenshot.
![demo](/post_imgs/shinyModuleIO.gif)