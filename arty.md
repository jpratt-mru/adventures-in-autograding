# Arty The Miner

[Arty](https://github.com/jpratt-mru/arty-the-miner) is a GitHub App that moves the results of every submission (which is handled by [this GitHub Workflow](https://github.com/jpratt-mru/starting-autograded-template/blob/master/.github/workflows/submit.yml) in the [starting Eclipse template](https://github.com/jpratt-mru/starting-autograded-template)) over to the submissions repo in a given GitHub organization that's been set up for use with a given course section.

Arty is currently running on a [free-tier level Heroku instance](https://dashboard.heroku.com/apps/arty-the-miner); if he ever needs to be used "for real", he'd either need a new home or a tier bump on Heroku.

## Why "Arty The Miner"?

When a GitHub Workflow runs, one thing you can do is save things that were generated inside the virtual machine that was "running the run", so to speak.

Those things are called "artifacts" by GitHub ([see here](https://docs.github.com/en/actions/configuring-and-managing-workflows/persisting-workflow-data-using-artifacts)). Since we want to extract (or _mine_ - it's a bit of a reach, but that's how my noggin noodles) the artifacts out of the submitted repo, well...Arty the Miner.

## What Arty commits for you

Once the submission workflow run has been completed, Arty springs (lurches?) into action and places the zipped artifact and the summary report for the submission into the submissions directory.

> _this repo - which should be private! - must be in the organization that was created for use wiht the GitHub classroom for that was made for that course section_

### Example

OK, let's make this real. I'll use a scenario based on real repos I've used in the past, but just a heads-up: these haven't been set up for use with this submission system - I'm just using these repos to ground this example in reality.

There's an organization - [mru-csis-1501-201904001](https://github.com/mru-csis-1501-201904-001) - I made for my fall 2019 section of 1501.

Every two weeks, I'd run a live lab exam. One of them was [lab.quiz.03](https://github.com/mru-csis-1501-201904-001/lab.quiz.03.official.template).

Say one student - with username jpratt123 - decides to submit their quiz. The quiz finished the submitting process at 1:32:52PM on September 18, 2020.

Then in this case, thanks to Arty's tireless efforts, we'd have the following entries in the submissions repository the instructor set up:

- `submissions/lab-quiz-03/jpratt123/2020-09-18T13-32-52/lab-quiz-03-jpratt123.zip`
- `submissions/lab-quiz-03/jpratt123/2020-09-18T13-32-52/summary-report.txt`
