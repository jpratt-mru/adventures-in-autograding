# Adventures in Autograding

## A Scenario

Student X is ready to get started on a 1502 drill.

1. They log into Blackboard, go to the list of available drill links, and click on the desired one.
   - **result**: a GitHub repo containing a copy of the starting code for the drill is created by GitHub Classroom and the student is given non-admin access to this repo.
1. The student clones the repo to their own machine. Perhaps because they're not quite comfortable with Git commands yet, they decide to download a zip file and unzip it into a convenient location.
   - **result**: the student now has a folder containing all the folders and files they need to start working.
1. The student uses the Import command in Eclipse with the downloaded folder.
   - **result**: Eclipse uses the various files and folders in the repo to create a working Eclipse project. The project uses the configuration files in the repo to initialize any necessary Eclipse plugins.
1. The student begins coding a solution (after thinking long and carefully about the problem first, naturally).
   - **result**: as the student saves their work, the Infinitest Eclipse plugin gives them immediate feedback about whether the unit tests provided with the project are passing or failing. Also, static code analysis tools like Checkstyle and PMD also display any issues after every source file save.
1. The student continues
