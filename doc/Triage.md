# Test Triage at Adoptium

## TRSS Parser for Release Builds

We've created a NodeJS module to parse TRSS output, convert it to Markdown so folks can paste that summary into a GitHub issue.

```bash
npm install -g trss-parser
trss-parser <parent id of TRSS pipeline run>
```

## Triage Guidance

There are many different test jobs running at the adoptium project.  No matter which test jobs are being triaged, there is a straight-forward pattern to follow.

### Categorize the test failure (based on test output) into 1 of 3 general types

- infra problem (machine/network)
- test problem
- product problem

### Check for an existing issue

- if issue already exists for the failure, annotate with additional information if needed.  For example, if you are investigating failures in openjdk tests, look to see if its already reported in JBS, as per instructions in [Guidance for creating OpenJDK bugs](https://github.com/adoptium/aqa-tests/wiki/Guidance-for-Creating-OpenJDK-Test-Defects).

### Raise an issue if no issue exists

- infra issue - raise an issue in [openjdk-infrastructure](https://github.com/adoptium/infrastructure/issues)
- test issue - ideally, there are enough details to determine which test repo to raise an issue in ond of the test repositories from which test material is pulled (OpenJDK, [aqa-systemtest](https://github.com/adoptium/aqa-systemtest/issues), [aqa-tests](https://github.com/adoptium/aqa-tests/issues) or any of the various 3rd party application suites).  If in doubt, ask some questions in the [#testing channel](https://adoptium.slack.com/archives/C5219G28G) and/or raise in [aqa-tests](https://github.com/adoptium/aqa-tests/issues) where it will get routed to proper repo
  - OpenJDK test issues - see [Guidance for creating OpenJDK bugs](https://github.com/adoptium/aqa-tests/wiki/Guidance-for-Creating-OpenJDK-Test-Defects)
  - Additional guidance for external tests - [Triage Rules for Application tests](https://github.com/adoptium/aqa-tests/tree/master/external#triage-rules)
- product issue - additional steps may be necessary, before raising an issue
  - rerun the test - locally or using a Grinder: see [How to Run a Grinder wiki](https://github.com/adoptium/aqa-tests/wiki/How-to-Run-a-Grinder-Build-on-Jenkins)
  - determine if the problem is occurs in other jdk versions, implementations and on other platforms
  - if only observed against 1 implementation, raise an issue against that implementation in the correct upstream repo (typically either OpenJDK or [Eclipse OpenJ9](https://github.com/eclipse-openj9/openj9/issues) projects).

### Exclude the test

- exclude in the most minimal way possible, if failing only on 1 platform, version or implementation, only exclude for that instance
- put the full link to the associated open issue into the exclude file
- exclude files vary depending on what test group you are triaging, refer to the README files in the aqa-tests subdirectories for more details
  - for openjdk tests, see [Exclude an openjdk test](https://github.com/adoptium/aqa-tests/tree/master/openjdk#exclude-a-testcase)
  - for other tests (like system, external and perf tests), tests are typically disabled via the associated playlist.xml (see [example playlist](https://github.com/adoptium/aqa-tests/blob/master/external/example-test/playlist.xml)) file either by using `<platformRequirements>^os.win</platformRequirements>` for permanent exclusion based on platform, or `<disable>` tag for temporary exclusion.

![Common Triage Paths](./diagrams/commonTriagePaths.png)
