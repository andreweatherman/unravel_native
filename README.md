# DataTutor: Data Science Code Comprehension Tools.

<!-- badges: start -->
[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-blue.svg)](https://www.tidyverse.org/lifecycle/#experimental)
<!-- badges: end -->

A project that uses Shiny interactive apps for facilitating Data Science code comprehension using R.

**NOTE:** The package is early on in its lifecycle and is still undergoing development. But, if you are ever so curious, you can install it with:

```r
devtools::install_github('nischalshrestha/DataTutor')
```

# Unravel

Unravel is the first tool I am developing in this project designed to help data scientists understand and explore tidyverse R code which makes use of the fluent interface (function composition). You can read about
the tool in my [paper](https://dl.acm.org/doi/10.1145/3472749.3474744) which covers its motivation, design, and results of a user study. Optionally, you can 
watch the [talk](https://youtu.be/wJ77e39XVEs) I gave on the tool at UIST 2021.

## Usage

### RStudio

You can unravel `dplyr` or `tidyr` (pivoting) code in a Shiny app to explore the intermediate data and data change states (no change, visible change, internal change, error). It will also allow you to perform structural edits to the code via toggles (think comment/uncomment), and reordering lines with drag and drop interaction. 

The easiest way to use Unravel is through the Addin. Highlight the tidyverse code you want to unravel, then go to Addins -> Unravel code.

You can also invoke it programmatically using the `unravel` function by wrapping or piping your code to the function:

```r
# wrapped
DataTutor::unravel(
  mtcars %>%
    group_by(cyl) %>% 
    summarise(mean_mpg = mean(mpg))
)
# piped
mtcars %>%
  group_by(cyl) %>% 
  summarise(mean_mpg = mean(mpg)) %>%
  DataTutor::unravel()
```

![](man/figures/example.png)

This will open up the app on the Viewer pane in RStudio by default. But, if you want to respect your currently chosen browser window, you can add a `viewer = FALSE`:

```r
mtcars %>%
  group_by(cyl) %>% 
  summarise(mean_mpg = mean(mpg)) %>%
  DataTutor::unravel(viewer = FALSE)
```

Keep in mind, it's very early so trying things on your own code/data might not work.

### What verbs have summaries?

Currently, any `dplyr`/`tidyr` piped code working on single tables will work execution-wise, but only a handful of the functions in each package has explicit support for summaries / has been tested. The summaries are generated by an [extension](https://github.com/nischalshrestha/tidylog) package of the amazing original [tidylog](https://github.com/elbersb/tidylog) package.

In the extension, I have added some enhancements (like data shape summary for every verb and rephrasing summaries) and is specially designed to work with `DataTutor` so that I can access the messages in a convenient cache. All verbs supported by `tidylog` besides `join`s will work and some more I added like `arrange`, `rowwise`.
