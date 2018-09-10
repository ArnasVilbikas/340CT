
# Advanced Git

Each week you will be expected to complete a series of lab activities. You will be required to reflect on these in your assignment so make sure you keep records of what you have done.

You should refer to [this week's presentation](https://drive.google.com/open?id=1JmtlJWZy5Y5pFhDoggkLzaCSrbSa3plBh105nZtU2qA).

## 1 What Will You Be Doing?

In this third sprint you will be adopting some additional agile concepts. The focus in this sprint is to improve the overall quality of your code-base:

1. Your team will add protection to the `master` branch to prevent developers from directly pushing to it.
2. You will be running the non-functional testing that you covered in the last worksheet to ensure you are writing high-quality code.
3. You will develop each feature  in its own _feature branch_.
4. You will be implementing **pull requests** to monitor and check the code being merged into the master branch.
5. You will rebase code into your feature branches to make it easier to merge on completion.
6. You will be tagging your releases.
7. And finally you will be implementing **Git Hooks** to run tests before committing and before pushing.

## 2 Before the Sprint

Unlike the previous sprints there are a couple of tasks you need to carry out this week before starting.

### 2.1 Update The Ignored Files List

Before starting the sprint, review the contents of the GitLab repository to identify any files that should not be in the repository. These might include:

1. Binary files
2. Third-party modules and libraries
3. Editor setting files
4. Local settings

If you find a file it can be removed by running the following command:

```
$ git rm --cached file1 file2
```

Once this has been done you will need to:

1. Add the names of the files and directories to your `.gitignore` file (otherwise they will be added to the next commit!
2. Use the `git status` command to make sure Git is not picking up the files.
3. Commit the changes and push.
4. Make sure the file(s) are no longer on the GitLab repository.

### 2.2 Configure Team Permissions

The first step is to make sure that everyone in the team has been assigned the correct permission levels.

1. One person in each _sub-team_ (eg, API, iOS, etc.) should be the designated **Code Owner**.
2. There are four permission levels: Guest, Developer, Reporter, Master. Everyone in the team should have developer permission.
3. The designated **Code Owner** for each repository should be given **Master** permissions.

### 2.3 Protected Branches

In your first sprint you all had full access to the _Master Branch_ meaning anyone in the team could commit to it and merge branches into it. As you quickly discovered this caused a lot of problems. In this sprint your team will be making the master branch into a **protected branch**, restricting who can interact with it and how.

1. In your GitLab repositories go to `Settings > Repository` and expand the **Protected Branches** section.
2. In the **Branch** dropdown list choose `master`.
3. In the **Allowed to merge** dropdown list choose 'Masters', this will prevent except _code owners) from merging any code into this branch.
4. In the **Allowed to push** dropdown list make sure that you choose `No one`, we don't want any code to be pushed directly into this branch.

### 2.4 Configuring the Non-Functional Tests

In the previous week you implemented a suite of non-functional tests. Before starting this sprint make sure these are:

1. Installed and configured correctly.
2. Run correctly (note the output you should be expecting).


### 2.5 Hooks

The value generated by your team lies in the quality code they produce so you should take time to ensure this is securely stored on your remote repository (GitHub). The first step is to ensure each person in the team only has the minimum permissions needed to do their job. You have already added some security by configuring _protected branches_ in the last lab. We will now improve this.

In the **Code Quality** worksheet you created a range of automated tests to check both _functional_ and _non-functional_ requirements. Many of these, such as the _linter_ returned a `0` on success and a non-zero if the test failed. Until now we have triggered these tests manually but, using **git hooks** these can be triggered in response to specific events.

#### 2.5.1 Pre-Commit

Let's use a **Git Hook** to run checks on our code before allowing us to commit. In this example git will reject any code that fails the linting test.

Use the terminal to create a new file in the `.git/hooks/` directory called `pre-commit`.

```shell
$ nano .git/hooks/pre-commit
```

You should now write a **shell script** to run the linter for your chosen language. You should already have a suitable script. Remember to include a [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) line to identify the _script executable_, typically on a *nix system this will be `#!/bin/sh` if the script is a _shell script_.

Finally you need to set the script as executable.

```shell
$ chmod +x .git/hooks/pre-commit
```

Lets see if this works:

1. Introduce a linting error (warnings won't work).
2. Manually trigger the linter to check it is being picked up properly.
3. Try staging and committing, it should be rejected.
4. Fix the linting error.
5. Try staging and committing again, it should work this time.

By setting up a number of tests in git hooks you can automatically monitor the code quality _before_ it is committed. The downside is that the more tests you include, the slower the commit process.

#### 2.5.2 Pre-Push

Rather than having a lot of tests that run at the commit stage you can also have tests that are triggered before the code is pushed (the push is rejected if they fail). The trick is to decide which tests should be `pre-commit` and which should be `pre-push`. This should be agreed by the team and the hooks set up before work starts.

Have a go at setting up a `pre-push` hook. You can choose whatever test or tests you want to include in this.


## 3 Conducting the Sprint

In this third sprint you will be adopting some additional agile concepts on top of all the skills you have already been using:

1. You will be implementing non-functional testing to improve code quality.
2. You will be implementing **pull requests** to monitor and check the code being merged into the master branch.
3. You will rebase code into your feature branches to make it easier to merge on completion.
4. You will be tagging your releases.
5. And finally you will be implementing **Git Hooks** to run tests before committing and after pushing.

### 3.1 Daily Standup Meeting

Your development team will still need to carry out a **Daily Standup meeting** every morning. Before this meeting, the _Scrum Master_ should:

1. Check the _Kanban board_ is up to date.
2. add up the hours for all the tasks remaining incomplete on the Kanban board and using this to update the _Burndown Chart_.

The Scrum Master needs to make sure everyone is engaged in the process. Adopt the following policy:

1. Everyone should be there at the agreed time. Anyone delayed must phone the Scrum Master and the meeting postponed until they are there.
2. Everyone should be standing around the information radiators (whiteboard/flipchart).
3. Phones stay in pockets. meeting paused if anyone uses a phone (they are not focussed).
4. Laptops only to be used to demonstrate functionality.

During the meeting:

1. The Scrum Master reviews the burndown chart and tells the team whether they are ahead or behind schedule:
2. Now each member:
    1. explains what they have achieved since the last daily standup meeting, running the  **acceptance test suite** and **unit test suite** to demonstrate this.
    2. uses the Kanban board to identify the tasks they will work on until the next meeting (tomorrow), flags with the team responsible and moves these forward on the board.
    3. Describes any technical challenges that are holding back development work.

If any problems were identified during the standup these will need to be resolved by the appropriate team immediately **after** the daily standup. Make sure the resolution is explained to the _Scrum Master_ before continuing work.

Now each team have tasks assigned and will need to implement these before the next daily standup.

### 3.2 The Development Process

This is the same as in the previous sprint with the following **additions**:

#### 3.2.1 Creating a Pull/Merge Requests

This should be carried out only if the feature is complete and all the automated tests (functional and non-functional) pass.

1. Click on the **Merge Requests** tab.
2. Click on the **New merge request** button.
3. The _source branch_ is the feature branch and the _target branch_ should be the master branch.
4. Click on the **Compare branches and continue** button.
5. Review the changes at the bottom of the next screen.
6. Add a title and description to the merge request, this should explain the work that has been done.
7. Click on **Submit merge request**

### 3.2.2 Approving a Pull/Merge Request

All requests will need to be reviewed by the **Code Owner**.

1. The number of merge requests needing approval are shown on the **Merge Requests** tab.
2. Review the changes:
    1. Pull the branch.
    2. Review the changes (and run tests).
3. Check the **Remove source branch** box.
4. Click on the **Merge** button.

If the code is not ready for merging you should add a comment and send it back to the development team. If the code is far from ready you can **close** the merge request.

#### 3.2.3 Rebasing

Rebasing allows you to unplug the base of your feature branch and replug it further down the commit tree. This allows you to integrate changes to the master branch in your feature branch in a clean way. You should carry this out on any long-running branch and it should be carried out if code has been merged to the master branch after this branch was created.

Take a few moments to review the structure of your git repository. Open the commit graph in GitLab `Repository > Graph` or use the `git log` command to get a visual representation.

```shell
$ git log --pretty=format:"[%cn] %h %s (%cr)" --graph
```

You can navigate forward a screen using &lt;space&gt;, back a screen using `w` and quit using `q`.

You should identify any branches and either:

- If the feature is complete _delete the branch_.
- If the feature is not complete, do a rebase to make sure the feature branch contains the latest code from `master`. Check out the feature branch and `git rebase master`.

This should make your git history much easier to understand. By rebasing your feature branches they will become far easier to merge back in to the `master` branch once the feature is complete.

## 4 After Completing a Story

Once a story has been completed you will need to create a software **release** by adding a **git tag**.

These are used to mark the code snapshots corresponding to the software releases, this is in the form of 3 numbers separated by periods (.) such as `v1.12.2`. There are [naming conventions](https://semver.org) you should use:

1. The first number is the **MAJOR**. If the product is still in beta this should be `0`. It only changes if there are major changes to the software.
2. The second number is the **MINOR**. It should be incremented for each user story completed.
3. The third number records the **PATCH** and is incremented whenever a bug is fixed.

- Go through the code in your master branch and note the 7 character _commit hash_ of the first working version of your code. After creating a tag it needs to be pushed to the remote:

```shell
$ git tag -a v0.1.12 -m 'Describe the changes clearly,
you can use multiple lines'
$ git push origin v0.1
```

These can be seen on GitLab under the `Repository > Tags` section which gives you the opportunity to download the code at that point.

Repeat the operation for subsequent working versions of your code (typically as you complete each user story). If the release contains the **Minimum Viable Product** (MVP) the version should increment to `v1.0`.