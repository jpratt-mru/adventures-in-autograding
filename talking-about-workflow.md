# GitHub Workflow Example

Here's the current workflow file I have; let's talk about it:

```yaml
name: Build this sucker!

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    continue-on-error: true

    steps:
      - name: Checkout thy repository
        uses: actions/checkout@v2

      - name: Set up Java14, my good fellow
        uses: actions/setup-java@v1
        with:
          java-version: 14

      - name: wget standard test files and such
        run: |
          rm -rf ./lib
          rm -rf ./config
          rm -rf ./src/test
          wget "https://github.com/MRU-CSIS-1501-DEV-PLAYGROUND/friend-list-libs-and-tests/raw/master/friend-list-libs-and-tests.zip" -P /tmp
          unzip /tmp/friend-list-libs-and-tests.zip -d /tmp/defaults
          mv /tmp/defaults/src/test ./src/test
          mv /tmp/defaults/config ./config
          mv /tmp/defaults/lib ./lib
          cp /tmp/defaults/cisummarizer-all.jar ./
          mkdir results
          mkdir reports

      - name: Compile that source code like a bandit
        run: |
          javac -d bin -cp "src/test:src/main:lib/*" src/test/*.java src/main/*.java 2> results/compilation-results.txt
          cat results/compilation-results.txt

      - name: Run the PMD, posthaste
        if: ${{ always() }}
        run: |
          wget "https://github.com/pmd/pmd/releases/download/pmd_releases%2F6.26.0/pmd-bin-6.26.0.zip" -P /tmp
          unzip /tmp/pmd-bin-6.26.0.zip -d /tmp
          mv /tmp/pmd-bin-6.26.0 $HOME/pmd
          chmod +x $HOME/pmd/bin/run.sh
          $HOME/pmd/bin/run.sh pmd -d ./src/main -R config/pmd/comp1501-pmd-rules.xml -f xml -r results/pmd-results.xml
          $HOME/pmd/bin/run.sh pmd -d ./src/main -R config/pmd/comp1501-pmd-rules.xml -f textcolor

      - name: Check all the styles
        if: ${{ always() }}
        run: |
          wget "https://github.com/checkstyle/checkstyle/releases/download/checkstyle-8.34/checkstyle-8.34-all.jar" -P /tmp
          java -Dconfig_loc="config/checkstyle" -jar /tmp/checkstyle-8.34-all.jar -c config/checkstyle/1501-checks.xml src/main/*.java
          java -Dconfig_loc="config/checkstyle" -jar /tmp/checkstyle-8.34-all.jar -c config/checkstyle/1501-checks.xml src/main/*.java -f xml -o results/checkstyle-results.xml

      - name: Testing the heck outta the codes
        if: ${{ always() }}
        run: |
          wget "https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.6.2/junit-platform-console-standalone-1.6.2.jar" -P /tmp
          java -jar /tmp/junit-platform-console-standalone-1.6.2.jar --class-path "bin:lib/assertj-core-3.16.1.jar:lib/system-lambda-1.0.0.jar" --disable-banner --scan-class-path --reports-dir=results
          java -jar /tmp/junit-platform-console-standalone-1.6.2.jar --class-path "bin:lib/assertj-core-3.16.1.jar:lib/system-lambda-1.0.0.jar" --disable-banner --scan-class-path --details=tree

      - name: Figuring out what this all meant
        if: ${{ always() }}
        run: |
          ls -al ./results
          mv ./results/TEST-junit-jupiter.xml ./results/junit-results.xml
          rm -f ./results/TEST-junit-vintage.xml
          ls -al ./results
          java -jar ./cisummarizer-all.jar $GITHUB_REPOSITORY
          cat reports/summary-report.txt

      - name: Save the results, just because
        if: ${{ always() }}
        uses: actions/upload-artifact@v1
        with:
          name: report
          path: reports/

      - name: Create an issue from the result. Despair? Jubilation?
        if: ${{ always() }}
        uses: peter-evans/create-issue-from-file@v2
        with:
          title: A summary was created
          content-filepath: ./reports/summary-report.txt
          labels: report
```

## line-by-line notes

I won't go over **every** line (you can dig https://docs.github.com/en/actions/getting-started-with-github-actions to learn more about workflows and actions...it's pretty good, as documentation goes).

- `name`
  - feel free to give it a more appropriate name
  - shows up here:
    - ![Alt text](images/name-capture.png)
- `on > workflow_dispatch:`
  - use this if you want users to trigger an action manually instead of automatically
  - at first, I had `on: push`...I thought this made the most sense - every time someone pushed to the repo, you'd want to run the actions, right? Wrong. For one, when GitHub Classroom does its thing, there are **three** pushes done by the creator of the template repo! So each of those pushes triggers the workflow! So not only is the poor student spammed with three extra notifications out of the gate...but that's three times as many minutes counted to the organization's Action minutes quota! AND if you have a student with a (typically admirable) "commit and push often" habit, that's even **more** quota being burned.
- `continue-on-error: true`
  - without this, the workflow will use the default value of false and the minute something goes wrong (like compilation fails or a PMD violation is found, etc), the workflow stops - but we don't want this, since we want to generate a summary report about all the problems!
- `checkout...`
  - very common GitHub action that pulls the current repo down onto the virtual machine - you need this!!!
- `set up Java....`
  - you can choose different Java versions to put on the virtual machine...very convenient!
- `wget standard test files and such` block
  > **pro tip**: when you do a `run` in an action, you can chain a whole bunch of things together if you start out with a pipe `|` on its own on one line followed by all the individual commands following one per line
  - this whole blocks job in life is to make sure that
    - any external libs needed for compilation,
    - Checkstyle and PMD configuration files, and
    - unit tests
      are copied into the workflow's virtual machine so that these important files are guaranteed to be in the project directory when the rest of the actions do their thing
  - this is partially done out of paranoia - if a student wanted to, they could play with the configs and tests to change the result of the upcoming checks!
  - this step also lets instructors bring in **further tests** - so they can give _some_ tests in the project template and then test with _additional_ ones as well
    > this is a bit of labour that needs to go into making this work, because you have to make a zip file of the necessary folders, place this zip file in its own **public** repo (otherwise the wget will fail...or at least require some extra work to handle authentication to get into a private repo!), and make sure that if any changes are made to lib/config/test, that everything gets updated!
  - this step also makes the `results` directory (which holds the raw results from compilation, checkstyle validation, pmd validation, and unit test reports) and the `reports` directory, which holds the summary report generated by the [cisummarizer tool](https://github.com/jpratt-mru/cisummarizer).
- `compile that source code....`
  -
