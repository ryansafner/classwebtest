# Course Websites

This repository contains the code for generating [classtest.ryansafner.com](https://classtest.ryansafner.com/). This is a minimal template for making my course websites, e.g. [metricsf19.courses.ryansafner.com](http://metricsf19.courses.ryansafner.com) and [microf19.courses.ryansafner.com](http://microf19.courses.ryansafner.com), and is designed to be ported to make other course websites quickly.

This is heavily borrowed from the beautiful course websites of [Andrew Heiss](https://andrewheiss.com), including his [`ath-tufte-hugo_18-19` theme](https://github.com/andrewheiss/ath-tufte-hugo_18-19).

# Instructions for Duplicating

Instructions based on Alison Hill's [excellent guide](https://alison.rbind.io/post/2017-06-12-up-and-running-with-blogdown/) to setting up a website using blogdown. I assume you will use [Netlify](http://netlify.com) to deploy your website automatically from a GitHub repository, and then via your own hosting service, link your Netlify account to your hosted domain. 

You will need a git client, GitHub account, and link your GitHub account to `R Studio`. Follow Jenny Bryan's [excellent guide](http://happygitwithr.com). 

1. Create a new repository on [github](http://github.com)
2. In your new repository, click the green **Clone or Download** button and copy the link
3. On your computer, navigate to a directory/folder where you will want to create a *new* directory/folder containing your website files. Once here, open a terminal/command prompt and type in `git clone the_link_from_step2` 
4. In `R Studio`, install `blogdown` (`install.packages("blogdown")`) if you don't already have it. 
5. Install Hugo via `blogdown::install_hugo()`
6. Start a `New Project` in `R Studio` from `Existing Directory` and then point it to the directory from Step 3. 
7. Edit your `*gitignore` file by adding a line with: `public/` so that it will work with Netlify ([here's why](https://bookdown.org/yihui/blogdown/version-control.html))
8. Build your website with ` blogdown::new_site()` in the `R Studio` console
9. Copy/paste all the following files and folders from this repo into your new folder: `content/`, `data/`, `lib/`, `pandoc/`, `public/`, `static/`, `themes/`, `config.yaml`, `index.Rmd` (optional: `README.md` and `favicon.ico`)
    - That is, DON'T copy the `.git` folder, `.Rproj.user` folder, `.gitignore`, `.RData`, `.Rhistory`, `classwebtest.Rproj`, and `index.html` into the new directory
10. Edit and build site at will, commit and push to GitHub, and you're at it! 

# Additional Needs

This structure and theme seems to have a lot of additional bells and whistles beyond a typical Hugo site. Mainly, it seems to be `pandoc` templates for sidenotes (a la Tufte) and citations. This part may require you to go down the rabbit hole and hack your computer just a little bit. 

- Ensure you have the latest version of [pandoc installed](https://pandoc.org/installing.html)
- Install [Pandoc sidenote](https://github.com/jez/pandoc-sidenote) (for converting footnotes to sidenotes).
    - I use a Mac, so I use `brew install jez/formulae/pandoc-sidenote` in a terminal
        - By the way, for Macs, you should have [homebrew](https://docs.brew.sh/Installation) installed, which requires command line tools in XCode (download XCode on the Mac App Store, and then install with `xcode-select --install` in a terminal)

# Major Moving Parts

- `content` folder contains the following folders for making individual class-meeting-based webpages, each file should begin with `##` for proper ordering
    - main level pages (that are on nav-bar): `syllabus`, `schedule`, `reference`, etc. 
    - `assignment`: contains individual files for individual assignment pages (e.g. problem sets, exams)
    - `class`: contains individual files for individual class-meeting pages
    - `reading`: contains individual files for describing individual class-meeting reading requirements
    - `post`: if this were to ever contain a blog, blog posts would go here, etc
- `data` folder contains 
    - `lessons.yml`: a key file that populates a table of class meetings on the `schedule` page. Links class meetings (rows of table) icons to individual assignment pages, class pages, and reading pages for each meeting
- `static` folder used for hosting files (e.g. datasets, handouts, images, etc)
    - `bib/references.bib` is where the references for citations (e.g. on syllabus, readings, etc) are stored
- `public` used for Netlify serving
- other folders that are necessary, but should not be modified:
    - `pandoc` stores templates needed for pages
    - `themes` contains the theme files, here called `ath-tufte-hugo_18-19` based on Andrew Heiss' [`ath-tufte-hugo_18-19` theme](https://github.com/andrewheiss/ath-tufte-hugo_18-19)


# Adding Content

- Changing navbar items: edit in `config.yaml` (main directory)
-

# Adding Sections to Schedule

1. Adding a new column (course item, such as "Homework" or "Class Notes" or "Slides") to the table schedule page, go to `schedule.html` in (`themes/ath-tufte-hugo_18-19/layouts/shortcodes/)` and add the following code (Substitute your actual name for my placeholder, `THING`, below)
        - Note if you'd like to use a different [font awesome icon](https://fontawesome.com/icons?d=gallery) (currently, `fas fa-university` is used, copy/paste the html code wherever you see `<i class="fas fa-university fa-lg"></i>`)
        - Essentially, the code below says "if there is a created `THING` page in the `/THING` folder for this class meeting, use `ICON`, otherwise, use `ICON` in the lighter color `f1f1f1`" .. the creation of `THING`s in the `/THING` folder is what Step 2 below is about. 

```
{{- if .THING }}
<td align="center" style="width:10%;text-align:center"><a href="{{ .Site.baseurl }}/THING/{{ .THING }}/">
    <i class="fas fa-university fa-lg"></i></a></td>
{{- else }}
<td align="center" style="width:10%;text-align:center"><font color="f1f1f1">
    <i class="fas fa-university fa-lg"></i></font></td>
{{- end }}
```

2. On `lessons.yaml`, add each individual element for each class as needed, e.g. `THING: "01-THING"`
3. Place individual `01-THING.Rmd` files into a `THING` folder under `content` to be linked to
    - The one exception is for Xaringan slides, which need to go under `static` to render properly, see below 

# Hosting Xaringan Slides and Adding to Schedule Page

- Following [Tim Mastny's Blog Post](https://timmastny.rbind.io/blog/embed-slides-knitr-blogdown/), create a `slides` folder under `static` folder
- Place all Xaringan files in that folder (e.g custom css files)
- Knit the Xaringan `.Rmd` files to produce an html file (output) pulling css and other libs from this folder
- Based on my Schedule page (see adding sections to schedule section), I have a link going to `/slides/` in the static folder for each class