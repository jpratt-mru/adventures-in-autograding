# The Checklist

## Before You Begin

Make sure you have the following ready before you begin:

- a directory to do all the work to follow (we'll call this the **workdir** from hereon in)
- a **public** GitHub repo to host the zip files mentioned later in the steps to follow

## The Steps

Here are the steps you can follow to get an assessment ready. You should follow them in the order provided.

- [ ] pull down the [current autograded template](https://github.com/jpratt-mru/starting-autograded-template/settings) into the workdir

  > **why?**
  >
  > This template has the structure and settings files necessary for the autograding process to work. You _don't_ want to recreate this by hand - trust me.

- [ ] I suggest you rename the template directory to `[name of assessment]-template` (for example `kerninator-drill-template`).
- [ ] remove the `.git` directory from the template

  > **why?**
  >
  > You're going to create a **new** repository here in the next step, so we want to blow away the old one.

- [ ] `git init` the template directory
- [ ] copy the starting source files and test files for your project into `src/main` and `src/test`, respectively
- [ ] copy any needed libraries into `lib` (for example, if you're using assertj, you'd want to put the `assertj-core.jar` file here)
- [ ] copy your assessment's README.md into the template directory, overwriting the one that's there
- [ ] if you're feeling particularly tidy, you can kill all the `.keep` files (in `lib`, `src/main`, and `src/test`)
- [ ] edit the `.project` file so that it has the desired name in the `<name>PROJECT-NAME-HERE</name>` section

  > **why?**
  >
  > this is the project name Eclipse will use when student's import your project...if you don't create a unique name here, then the second time a student imports a project, they'll get a confusing error from Eclipse, which will complain about a project with the same name already existing. Confusion bad.

- [ ] if you have any special libs in use, you've got some extra work to do:
  - [ ] make sure they're all added to the build path in Eclipse; this will alter the `.classpath` file. If you forget to do this, your project will be hella-confusing to students, because any references to the libraries will be broken and they won't know how to fix that!
  - [ ] the `Generate test report` of the `submit.yml` file will need to be modified as well: tweak the `--class-path` section to include **explicit** references to each lib - you **can't** use wildcards here!
- [ ] create the zip file needed for the GitHub Action that "marks" things. Here's what you do:
  - [ ] create a directory (which we'll call the **zipdir**) in the workdir to hold the contents of the zip
  - [ ] place copies of the `lib`, `config`, and `src` directories from the template directory into the zipdir. **If you have any solution code in the src directory, make sure you delete it, 'cause the zip's going in a public repo!**
  - [ ] copy the most recent version of the cisummarizer-all.jar from [the cisummarizer repo](https://github.com/jpratt-mru/cisummarizer) into the zipdir
  - [ ] zip the `cisummarizer-all.jar`, `config` folder, `lib` folder, and `src` folders into a zip file. **Make sure that the zip file just contains these items and NOT a folder that contains these items!**
- [ ] upload the zip file to the public repo mentioned in [Before You Begin](#before-you-begin)
- [ ] grab the **raw link** to the zip file in the public repo and update the two lines (they're right next to each other) in `submit.yml` that refer to the zip file.
- [ ] at this point, the template directory is good to go, so add, commit, and push to a private repo in the organization where you're hosting all the GitHub Classroom repos for the current semester
- [ ] after the push is complete, mark the repo as a template in the repo's settings
- [ ] if you mean for Arty to be involved, you might find it useful to confirm that he's been installed to your org
- [ ] go to your desired GitHub Classroom classroom and add a new assignment with the desired options