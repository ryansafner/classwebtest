# ECON 480: Econometrics

This repository contains the code for generating [classtest.ryansafner.com](https://classtest.ryansafner.com/).

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
9. Copy/paste my barebones course template

# Additional Needs

- Ensure you have the latest version of [pandoc installed](https://pandoc.org/installing.html)
- Install [Pandoc sidenote](https://github.com/jez/pandoc-sidenote) (for converting footnotes to sidenotes, a la Tufte). This might require you to go down the rabbit hole a bit: 
    - I use a Mac, so I use `brew install jez/formulae/pandoc-sidenote`
        - Why, by the way, you should have [homebrew](https://docs.brew.sh/Installation) installed, which requires command line tools in XCode (download XCode on the Mac App Store, and then install with `xcode-select --install` in a terminal)

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

1. Adding a new column (assignment type) to the table schedule page, such as **thing**, go to `schedule.html` in (`themes/ath-tufte-hugo_18-19/layouts/shortcodes/) and add the following code (examplified with **thing**)

```
            {{- if .thing }}
            <td align="center" style="width:10%;text-align:center"><a href="{{ .Site.baseurl }}/thing/{{ .thing }}/">
                <i class="fas fa-university fa-lg"></i></a></td>
            {{- else }}
            <td align="center" style="width:10%;text-align:center"><font color="f1f1f1">
                <i class="fas fa-university fa-lg"></i></font></td>
            {{- end }}

```

2. On `lessons.yaml`, add each individual element for each class as needed, e.g. `thing: "01-thing"`
3. Place individual `01-thing.Rmd` files into a `thing` folder under `content` to be linked to
    - The one exception is for Xaringan slides, which need to go under `static` to render properly, see below 

# Hosting Xaringan Slides and Adding to Schedule Page

- Following [Tim Mastny's Blog Post](https://timmastny.rbind.io/blog/embed-slides-knitr-blogdown/), create a `slides` folder under `static` folder
- Place all Xaringan files in that folder (e.g custom css files)
- Knit the Xaringan `.Rmd` files to produce an html file (output) pulling css and other libs from this folder
- Based on my Schedule page (see adding sections to schedule section), I have a link going to `/slides/` in the static folder for each class