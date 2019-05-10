# ECON 480: Econometrics

This repository contains the code for generating [classtest.ryansafner.com](https://classtest.ryansafner.com/).

This is heavily borrowed from the beautiful course websites of [Andrew Heiss](https://andrewheiss.com), including his [`ath-tufte-hugo_18-19` theme](https://github.com/andrewheiss/ath-tufte-hugo_18-19).

# Instructions for Duplicating

1. Create a new repository on [github](http://github.com)
2. On your new repository, click the green **Clone or Download** button and copy the link
3. On your computer, navigate to a directory/folder where you will want to create a *new* directory/folder containing your website files. Once here, open a terminal/command prompt and type in `git clone thelinkyoucopiedtoyourrepositoryinstep2` 
4. 


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
- `public` used for Netlify serving
- other folders that are necessary, but should not be modified:
    - `pandoc` stores templates needed for pages
    - `themes` contains the theme files, here called `ath-tufte-hugo_18-19` based on Andrew Heiss' [`ath-tufte-hugo_18-19` theme](https://github.com/andrewheiss/ath-tufte-hugo_18-19)

# Adding Content

- Changing navbar items: edit in `config.yaml` (main directory)
-