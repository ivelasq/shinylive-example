# Making a webpage with Shinylive using R

1. Create a new Quarto `.qmd` doc in a folder.

2. Install the shinylive package.

```r
install.packages("shinylive")
```

3. Add the Quarto extension in your folder.

```{.bash filename="Terminal"}
quarto add quarto-ext/shinylive
```

4. Put this in the YAML header of your Quarto doc.

```
filters:
  - shinylive
```

5. Create a `shinylive-r` block with your Shiny code in it. 

- Add a code chunk option, `#| standalone: true`.
- You can control the height of the panel by setting `viewerHeight`.

````
```{shinylive-r}
#| standalone: true

library(shiny)
library(bslib)

ui <- page_sidebar(
  # App title ----
  title = "Hello Shiny!",
  # Sidebar panel for inputs ----
  sidebar = sidebar(
    # Input: Slider for the number of bins ----
    sliderInput(
      inputId = "bins",
      label = "Number of bins:",
      min = 1,
      max = 50,
      value = 30
    )
  ),
  # Output: Histogram ----
  plotOutput(outputId = "distPlot")
)

# Define server logic required to draw a histogram ----
server <- function(input, output) {

  # Histogram of the Old Faithful Geyser Data ----
  # with requested number of bins
  # This expression that generates a histogram is wrapped in a call
  # to renderPlot to indicate that:
  #
  # 1. It is "reactive" and therefore should be automatically
  #    re-executed when inputs (input$bins) change
  # 2. Its output type is a plot
  output$distPlot <- renderPlot({

    x    <- faithful$waiting
    bins <- seq(min(x), max(x), length.out = input$bins + 1)

    hist(x, breaks = bins, col = "#007bc2", border = "white",
         xlab = "Waiting time to next eruption (in mins)",
         main = "Histogram of waiting times")

    })

}

shinyApp(ui = ui, server = server)

```
````

6. Render your document.