---
title: "HDSU - Wiki"
layout: wiki
excerpt: "HDSU -- Wiki"
sitemap: false
permalink: /wiki/workflowr/
---


# Workflowr

Workflowr is a great tool for making your analysis and report reproducible,
shareable and organized. Check out [this page](https://github.com/workflowr/workflowr#quick-start) for more information on how 
to start your workflowr project: 

(see [Vignette](https://workflowr.github.io/workflowr/articles/wflow-01-getting-started.html))

See an example [here (report from Katharina Mikulik)](https://katharina782.github.io/HDSU_Herrmann_report)

## 1. Import the library

```
library("workflowr")
```

In case you have never created a Git repository on your computer:

```
wflow_git_config(user.name = "Your Name", user.email = "email@domain")
```

## 2. Start your project

The following command will create a directory called "myproject" that contains all the files and folders you need. 

```
wflow_start("myproject")
```

* `analysis/` should contain all source R Markdown files for your data analysis
* `analysis/index.html` does not contain any code but creates the homepage
* `analysis/_site.yml` is the configuration file, where you can edit your webiste aesthetics

**Be careful NOT to delete `analysis/_site.yml` or `analysis/index.html`!**

* `docs/` contains the HTML files for your website which are built from the R Markdown files in `analysis/` (this directory is empty until you first build your website)
* the additional directories are optional suggestions for for organizing your  project

## 3. Build and publish your website

To build your webiste run the command below. This will build all the HTML files from the Rmarkdown files. These files are not published yet. An unpublished file can only be viewed on the local computer, but is not yet included in the website. To publish your files run the following command.

```
wflow_build()
```

```
wflow_publish()
```

You can add all the files that you want to publish, for example:

```
wflow_publish(c("analysis/index.Rmd", "analysis/about.Rmd", "analysis/license.Rmd"),
              "Publish the initial files for myproject")
```

**Pro Tip:**
If you want to make sure that all figures are displayed correctly and
that you can switch between files in the navigation bar try the following command. This might solve any troubles you run into (especially using cache
can cause problems). 

```
wflow_publish(republish = TRUE, all = TRUE, delete_cache = TRUE)
```

## 4. Push

Your files are automatically commited when you run `wflow_publish()`. Now all
that is left to do is to push everything to GitHub.

```
wflow_git_push( )
```

## More tips:

If you want to play around with the aesthetics of your website check out
this link: https://rstudio4edu.github.io/rstudio4edu-book/rmd-dress.html

To include references you need to create a references.bib file. Check out
this link: https://bookdown.org/yihui/rmarkdown-cookbook/bibliography.html
