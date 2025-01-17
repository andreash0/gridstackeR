
<!-- README.md is generated from README.Rmd. Please edit that file -->

# gridstackR

The gridstackR package allows users to easily create Dashboards with
[gridstack.js](https://gridstackjs.com/) functionalities

<img src='man/figures/healthdown_example.gif'/>

## Installation

You can install the development version from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("petergandenberger/gridstackeR")
```

## Usage

In the `ui.R` file, create a grid using `grid_stack(...)` and place
grid-stack-items inside using `grid_stack_item(...)`.

Specify options like height, width, x-, y-position aswell. Check the
[gridstack.js
documentation](https://github.com/gridstack/gridstack.js/tree/master/doc#item-options)
for a full list of options.

The `ui.R` file might contain something like the following.

``` r
grid_stack(
  grid_stack_item(
        h = 4, w = 4, id = "plot_container", style = "overflow:hidden",
        box(
          title = "Histogram", status = "primary", solidHeader = TRUE,  width = 12, height = "100%",
          plotOutput("plot", height = "auto")
        )
      )
```

## Dynamic figure height

Plots inside the `grid-stack-items` might not change their height
themselves.

### Setting the height dynamically using callbacks

The following example shows how the height of the plot can be set
dynamically using the `<id>_height` callback

Note: the `plot_container_height` references the height of the
`id = "plot_container"` created in the `ui.R` example above.

``` r
output$plot <- renderPlot({
    x    <- faithful$waiting
    bins <- seq(min(x), max(x), length.out = input$slider + 1)

    hist(x, breaks = bins, col = "#75AADB", border = "white",
         xlab = "Waiting time to next eruption (in mins)",
         main = "Histogram of waiting times")

  },
  # set the height according to the container height (minus the margins)
  height = function() {max(input$plot_container_height - 80, 150)}
  )
```

### Setting the height for [DT::dataTableOutput](https://rstudio.github.io/DT/)

The height for a `DT::dataTableOutput` can be set as in the following
example.

ui.R

``` r
grid_stack_item(
        w = 4, h = 10, x = 0, y = 0, id =  "c_table",
        DT::dataTableOutput("mytable")
      )
```

server.R

``` r
output$mytable <- DT::renderDataTable({
    DT::datatable(mtcars, options = list(
      # set the height according to the container height (minus the margins)
      scrollY = max(input$c_table_height, 200) - 110, paging = FALSE
      )
    )
  })
```

### Setting the height for [echarts4r](https://github.com/JohnCoene/echarts4r)

The height for a `echarts4r::echarts4rOutput` can easily be set using
the `height="100%"` option.

ui.R

``` r
grid_stack_item(
        w = 5, h = 5, x = 7, y = 0, id = "c_plot",
        echarts4rOutput(outputId =  "plot", height = "100%")
      )
```
