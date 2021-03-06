# Procedure for creating a new repository

**About this document**

This document describes the recipe to create a new GitHub repository. It's meant for class libraries.


## Introduction

The strict following of this procedure is required in order to maintain consistency and coherence throughout the repositories, along with taking advantage of the build tools, testing and publishing automation.
If in doubt please ask one of the senior team members.


## Creating the repository in GitHub

1. This is basically clicking the create new repository button in GitHub. 

Note: The class libraries repositories are following the patter "**lib-**namespace" most of the remaining repositories "**nf-**some-relevant-name-here". This makes it easier to spot what is what.

2. As we are following the [GitFlow branching model](http://nvie.com/posts/a-successful-git-branching-model/) two branches must be created: `master` and `develop`. 

3. Make sure to create an empty readme.md to make it easier to fork and clone the new repo.


## Adjust the repository settings (part 1)

1. Go to the repository settings and move into _Options_.

2. In the _Features_ section disable Wikis, Issues and Projects.

3. On the _Merge Button_ section disable Allow merge commits. We prefer to have tidy merges on PRs without having to bother contributors to squash commits.


## Setup the CLA

1. Open a browser window in Private Mode (so you can sign-in as `nfbot` and not loose you personal GitHub session).

2. Navigate to the [CLA Assistant](https://cla-assistant.io/).

3. Sign-in with GitHub `nfbot` account.

4. Click the "Configure CLA" button at the top left.

5. Select the newly created repo on the "Choose a repository" drop-down.

6. Select the "nanoFramework-CLA.md" in the next drop-down.

7. Click the "Link" button and agree with the next step.


## Setup AppVeyor

1. Still on that Private browser window, navigate to [AppVeyor](https://ci.appveyor.com/projects).

2. Click "New Project" and select the ewly created repo.

3. After the new project corresponding to the the new repo shows, go to "Settings".

4. Check that the default branch showing is "develop".

5. Check the "Save build cache in Pull Requests" option almost at the bottom.

6. Click "Save".

7. Move into "Environment" and create a new environment variable "MyGetToken" filling in the respective value with the token from MyGet feed.

8. Click "Save".

9. Move into "Badges" and copy the markdown code for master and develop branches. This will be required to add the correct build badges in the repo readme in a moment.


# Prepare the initial commit

1. Fork the repo into your preferred GutHub account and clone it locally.

2. The best option is to copy/past from an existing repo, so you're more efficient doing just that. Mind the name changes tough! Grab the following files:
  - .github\PULL_REQUEST_TEMPLATE.md
  - .gitignore
  - appveyor.yml
  - CODE_OF_CONDUCT.md
  - CONTRIBUTING.md
  - GitVersion.yml
  - install-vsix-appveyor.ps1
  - LICENSE
  - README.md
  - template.vssettings

3. Open "appveyor.yml"
    1. Rename the class library name occurrences with  the new name.
    2. Rename the "release description" occurrences with the new name.

4. Open "GitVersion.yml" and set the _next_ version to the appropriate one. Make sure to follow our version number guidelines. In doubt please ask one of the senior team members.

5. Open "README.md"
    1. Rename the class library name occurrences with  the new name.
    2. Rename the package name for the Nuget badges.
    3. Replace the build status badges with the ones that you've copied from AppVeyor. Make sure that you past the correct ones, don't misplace the develop in the the master.

6. Create a "source" folder that will hold the code files and VS Solutions and projects.

7. Create a "**Nuget.**class-lib-name" folder inside source.

8. Create a "**Nuget.**class-lib-name.DELIVERABLES" folder inside source.

9. Add the VS solutions for the class library and for the Nuget packages. Again it's better to follow an existing one and ask in doubt.


## Adjust the repository settings (part 2)

1. Go to the repository settings in GitHub and move into _Branches_.

2. Go to the rule for "develop" branch and change the following:
  - Enable "Require pull request reviews before merging"
  - Enable "Require status checks to pass before merging" with the options:
    - "Require branches to be up to date before merging"
    - "Status checks: continuous-integration/appveyor/pr" (for develop branch)
    - "Status checks: continuous-integration/appveyor/branch" (for master branch)
    - "Status checks: license/cla" (for develop branch)
