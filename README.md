This plugin is no longer in Marketplace and is not very well maintained. 

See: https://community.developer.atlassian.com/t/eula-and-data-center-for-free-apps/26062

You may want to use this one: https://github.com/sford/yet-another-commit-checker

---

# Simple Bitbucket Commit Checker [![Build Status](https://travis-ci.org/tomasbjerre/simple-bitbucket-commit-checker.svg?branch=master)](https://travis-ci.org/tomasbjerre/simple-bitbucket-commit-checker)
Simple, and easy to use, commit checker for Atlassian Bitbucket. There are many commit checkers out there. This plugin aims at being simple and user friendly. The admin GUI ([here](https://raw.githubusercontent.com/tomasbjerre/simple-bitbucket-commit-checker/master/sandbox/admin_upper.png) and [here](https://raw.githubusercontent.com/tomasbjerre/simple-bitbucket-commit-checker/master/sandbox/admin_lower.png)) allows the Bitbucket administrator to add custom messages for each rejection reason. [Here](https://github.com/tomasbjerre/simple-bitbucket-commit-checker/blob/master/src/test/resources/testProdThatRejectResponseLooksGood.txt) is a sample reject message and [here](https://github.com/tomasbjerre/simple-bitbucket-commit-checker/blob/master/src/test/resources/testProdThatSuccessResponseLooksGood.txt) is a sample accept message. [This](https://raw.githubusercontent.com/tomasbjerre/simple-bitbucket-commit-checker/master/sandbox/config_and_reject.png) is a screenshot of a push being rejected.

Available in [Atlassian Marketplace](https://marketplace.atlassian.com/plugins/se.bjurr.sscc.sscc).

## Features
* Check author in commit
  * email is same as users email in Bitbucket.
  * email is same as any users email in Bitbucket.
  * name is same as users name in Bitbucket.
  * name is same as any users name in Bitbucket.
  * name is same as users user slug in Bitbucket.
  * name is same as any users user slug in Bitbucket.
* Check that author name, slug, and/or email, in commit exists for any user in Bitbucket.
* Check committer in commit.
  * email is same as users email in Bitbucket.
  * name is same as users name in Bitbucket.
  * name is same as users user slug in Bitbucket.
* Optionally check author email against regular expression instead of equality to email in Bitbucket. Like:
  * ^${BITBUCKET_USER}@.*
  * ^[^@]*@company.domain$
* Check JQL query. Can be used to check that any JIRA is in a specific state. There is an extra variable, ${REGEXP}, available for use in the query.
  * Example: issue = ${REGEXP} AND status = "In Progress" AND assignee in ("${BITBUCKET_USER}")
* Simple configuration of rules that must apply to commit messages. Organized in groups.
  * A group can be used for matching, for example, issues. It can state that "at least one", "all of" or "none" of the issues can be mentioned in the commit messages.
  * Rules are added to the group. A rule can, for example, define Jira as a regular expression and the name "Jira".
  * If a group matches a commit, it can reject it or just show a message to the comitter.
* Check only branches matching a regular expression.
* Check that branch name matches specific regexp.
  * You may use variables here to make sure user *tomas* can only commit to `dev/tomas/.+` with a regexp like `refs/heads/dev/${BITBUCKET_USER_SLUG}/.+`.
* Exclude merge commits.
* Optionally check annotated tag commits.
* Check commits in pull requests
* Show a general reject message.
* Show a general accept message.
* Optionally accept all commits from [service users](https://developer.atlassian.com/static/javadoc/bitbucket-server/4.0.3/api/reference/com/atlassian/bitbucket/user/UserType.html).
* Optionally accept all commits if pattern matches the username. Checks will be ignored for user if pattern matches its name, like ^BATCH.* will ignore users with name starting with BATCH.
* Dry run mode, where all commits are accepted. But verification results are shown.
* Supporting variables to be used in error messages and checks.
  * BITBUCKET_EMAIL, Email of user in Bitbucket.
  * BITBUCKET_NAME Name of user in Bitbucket.
  * BITBUCKET_USER, User display name of user in Bitbucket.
  * BITBUCKET_USER_SLUG, User slug (username) of user in Bitbucket.
  * COMMITTER_EMAIL, Email of committer name.
  * COMMITTER_NAME, Name of committer email.
  * AUTHOR_EMAIL, Email of author email.
  * AUTHOR_NAME, Name of author name.

The plugin is configured on repository level, where all hooks are configured. If you want to configure it once for all repos, have a look at [Settings Synchronizer for Bitbucket](https://github.com/tomasbjerre/settings-synchronizer-for-bitbucket-plugin).

## Design goals
The included features should:

* Be easy to configure as an administrator of Bitbucket. Any rejection reason delivered to the committer should be configurable.
* Be easy to use as a committer. With user friendly rejection messages, that clearly explains what was wrong and how to fix it.

## Developer instructions
You will need Atlas SDK to compile the code.

https://developer.atlassian.com/docs/getting-started/set-up-the-atlassian-plugin-sdk-and-build-a-project

You can generate Eclipse project:
```
atlas-compile eclipse:eclipse
```

Package the plugin:
```
atlas-package
```

Run Bitbucket, with the plugin, on localhost:
```
export MAVEN_OPTS=-Dplugin.resource.directories=`pwd`/src/main/resources
atlas-mvn bitbucket:run
```

You can also remote debug on port 5005 with:
```
atlas-debug
```

Make a release:

https://developer.atlassian.com/docs/common-coding-tasks/development-cycle/packaging-and-releasing-your-plugin
```
atlas-mvn release:prepare
atlas-mvn release:perform
```
