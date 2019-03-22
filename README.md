
<!-- README.md is generated from README.Rmd. Please edit that file -->

# shinyhttr

[![Travis build
status](https://travis-ci.org/curso-r/shinyhttr.svg?branch=master)](https://travis-ci.org/curso-r/shinyhttr)
[![AppVeyor build
status](https://ci.appveyor.com/api/projects/status/github/curso-r/shinyhttr?branch=master&svg=true)](https://ci.appveyor.com/project/curso-r/shinyhttr)
[![CRAN
status](https://www.r-pkg.org/badges/version/shinyhttr)](https://cran.r-project.org/package=shinyhttr)

The goal of shinyhttr is to integrate `httr::progress` with
`shinyWidgets::progressBar`.

In practice, the difference will be

``` r
# from this
httr::GET("http://download.com/large_file.txt", 
          progress())


# to this
httr::GET("http://download.com/large_file.txt", 
          progress(session, id = "my_progress_bar1"))
```

![gif\_progress\_example.gif](man/figures/README-gif_progress_example.gif)

## Installation

From CRAN:

``` r
install.packages("shinyhttr")
```

From github:

``` r
devtools::install_github("curso-r/shinyhttr")
```

## Example

``` r
library(shiny)
library(shinyWidgets)
library(httr)
library(shinyhttr)

ui <- fluidPage(

  sidebarLayout(

    NULL,

    mainPanel(
      actionButton('download', 'Download 100MB file...'),
      tags$p("see R console to compare both progress bars."),
      progressBar(
        id = "pb",
        value = 0,
        title = "",
        display_pct = TRUE
      )
    )
  )
)

server <- function(input, output, session) {
  observeEvent(input$download, {
    GET(
      url = "https://speed.hetzner.de/100MB.bin",
      shinyhttr::progress(session, id = "pb") # <- the magic happens here. progress() now has session and id args
    )
  })
}

shinyApp(ui, server)
```
