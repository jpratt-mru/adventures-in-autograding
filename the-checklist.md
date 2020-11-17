# The Checklist

## Preamble

This covers the things that need to be done for The System to work. Once you go through this process a few times, please think about making your own version of this to suit your workflow. Maybe you want to reorder some steps? Stress some? Delete others? Automate portions?

## WAY Before You Begin

- [ ] create a GitHub organization for the course section you're teaching. This org will house all your coursework (including student work) for the semester.
  
  - [ ] if you want to use [Arty](#arty.md) to move all submissions to a centralized location, then 
  
    - [ ] [install him](installing-arty.md) in your org

    - [ ] create a **private** GitHub repo in this org called `submissions`

  - [ ] create a **public** GitHub repo in this org to host the zip files mentioned later

- [ ] make sure all students have GitHub accounts (they don't have to name them their MRU username)

## Before You Begin

Make sure you have the following ready before you begin the process of making an assessment useable by The System:

- [ ] a copy of a working assessment Eclipse project (we'll call this the **assessment template** ) created from the [master template](https://github.com/jpratt-mru/starting-autograded-template.git)<sup id="f1">[1](#footnote-1)</sup>

  - [ ] confirm that the assessment template has any necessary libraries in its `lib` directory (for example, I use assertj for my unit tests, so I make sure the needed `assertj-core.jar` is there)

  - [ ] confirm that you are using JUnit 5 unit tests<sup id="f1a">[1a](#footnote-1a)</sup>
  
  - [ ] if you have specific PMD or Checkstyle rules you want in place, make sure they are set up as **project-level** rules in Eclipse 
   
- [ ] a directory to do all the work to follow (we'll call this the **workdir** from hereon in)

## The Steps

Here are the steps you can follow to create the GitHub Classroom repository that students will use. 

- [ ] move the assessment template directory into the workdir and rename it `[name of assessment]-template` (for example `kerninator-drill-template`)

- [ ] remove any existing `.git` directory from the assessment template <sup id="f2">[2](#footnote-2)</sup>

- [ ] `git init` the assessment template **directory**

- [ ] create the zip file needed for the GitHub Action that "marks" things:
  
  - [ ] create a directory (which we'll call the **zipdir**) in the workdir to hold the contents of the zip
  
  - [ ] place copies of the `lib`, `config`, and `src` directories from the assessment template into the zipdir. **If you have any solution code in the src directory, make sure you delete it, 'cause the zip's going in a public repo!**
  
  - [ ] copy the most recent version of the cisummarizer-all.jar from [the cisummarizer repo](https://github.com/jpratt-mru/cisummarizer) into the zipdir
  
  - [ ] zip the `cisummarizer-all.jar`, `config` folder, `lib` folder, and `src` folders into a zip file. **Make sure that the zip file just contains these items and NOT a folder that contains these items!**

- [ ] upload the zip file to the public repo mentioned in [Before You Begin](#way-before-you-begin)

- [ ] grab the **raw link** to the zip file in the public repo and update the **"REPLACE ZIP LOCATION"** entry in `.github/workflows/submit.yml` with that URL

- [ ] edit the `.project` file so that it has the desired name in the `<name>PROJECT-NAME-HERE</name>` section<sup id="f3">[3](#footnote-3)</sup>

- [ ] if you have any special libs in use, you've got some extra work to do:
  - [ ] make sure they're all added to the build path in Eclipse; this will alter the `.classpath` file<sup id="f4">[4](#footnote-4)</sup>

  - [ ] the `Generate test report` of the `submit.yml` file will need to be modified as well: tweak the `--class-path` section to include **explicit** references to each lib - you **can't** use wildcards here!

- [ ] at this point, the assessment template is good to go, so: add, commit, and push to a private repo in the organization where you're hosting all the GitHub Classroom repos for the current semester

  - [ ] create a private GitHub repo in your organization for this assessment template

  - [ ] do the usual add/commit/push hoedown to get the local repo up to the GitHub one

  - [ ] **mark the repo as a template in the repo's settings!!!**<sup id="f5">[5](#footnote-5)</sup>

- [ ] go to your desired GitHub Classroom classroom and add a new assignment with the desired options that accesses the GitHub repo you made for the assessment template
 
<hr/>
 
 ### FOOTNOTES
 
<a id="footnote-1">1</a> This template has the structure and settings files necessary for the autograding process to work. You _don't_ want to recreate this by hand - trust me. [↩](#f1)

<a id="footnote-1a">1a</a> The cisummarizer only parses the results of JUnit 5 tests, so the summary report will incorrectly report test results if you use JUnit 4. [↩](#f1a)

<a id="footnote-2">2</a> You're going to create a **new** repository here in the next step, so we want to blow away the old one. [↩](#f2)

<a id="footnote-3">3</a> This is the project name Eclipse will use when student's import your project...if you don't create a unique name here, then the second time a student imports a project, they'll get a confusing error from Eclipse, which will complain about a project with the same name already existing. Confusion bad. [↩](#f3)

 <a id="footnote-4">4</a> If you forget to do this, your project will be hella-confusing to students, because any references to the libraries will be broken and they won't know how to fix that! [↩](#f4)
 
 <a id="footnote-5">5</a> If you forget to do this now, things get fuglier when you make the assessment in GitHub Classroom, so do it now and breathe easier later. [↩](#f4)