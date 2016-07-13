---
title: Introduction to Git
date: '2016-07-11 09:16:00'
layout: post
---
## What is Git?

Git is a version control system (VCS) which exists as a distributed tree based repository.  It's used for storing and versioning documents and code, and allows you to at any time retrieve any of the files from any of the current or past versions of the repository.  It also has features like file locking to ensure that only one person at a time can make edits to a file, automatic merging for when two users make changes to the same file at the same time but don't conflict with each other, and assisted merging for when two users make changes to the same part of the same file at the same time.  Most importantly Git is useful for never losing your work by not only backing it up in the cloud but by also ensuring that you can always revert back to a pervious version on a per-file/folder/repository basis.

## How to install Git

If you're working with [Github](http://www.github.com), which you probably are, and you don't want to learn command line interfaces, which you probably don't, then you should just simplify your life and download [Github Desktop](https://desktop.github.com/).  It's available for both Windows and OSX.

If you're on Linux, or would rather learn how to use git from the command line (which is generally faster once you get used to it), then you should simply use Git.  [Here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) is a good link which gives the basic instructions for installing Git under Linux, OSX, and Windows.

## How to use Git

Most of the time you're using Git, you won't notice it.  That's because most of the time you're simply working as usual, occasionally commiting your changes and pushing them into the cloud to back them up.  However, before you can do any of that, you must have a Git repository.

### Creating a repository

The easiest way to make and host a backed-up accessible git repository is through [Github](http://github.com).  There (as a registered user, free account) you can create your own repositories which are hosted at _http://github.com/${user name}/${repository name}_.  If you have are part of an organization (or create one), you can also create a repository under the organization which will be hosted at _http://github.com/${organization name}/${repository name}_.  For this team, you should be a part of and use the VADL organization, so create any related repositories (that your team members will be using as well) in the VADL organization.  Creating a repository is as easy as pressing the button that says _New Repository_ and giving it a name.  Generally you do want to click the box that says _Initialize this repository with a README_, since that will allow you to immediately start working on it.

### _Cloning_ a repository

Once you've created a repository (or if someone else already has, and you've been given access by them or through a shared organization), you can **clone** the repository onto your computer, which downloads the entire repository (and its history), compressed of course, into a folder of your choosing.  

#### Github Desktop

In Github Desktop, this is as simple as clicking in the upper left to clone a new repository and selecting one of the available repositories that it shows you (those are all the repositories to which you have write access).

#### Git console

To clone a repository (whether or not you have write access) in git console you have two options, shown here as examples:

```bash
$ git clone git@github.com:${user/organization name}/${repository name} ${optional folder name}
```

and

```bash
$ git clone https://github.com/${user/organization name}/${repository name} ${optional folder name}
```

e.g. 

```bash
git clone git@github.com:vadl/agse
git clone git@github.com:vadl/agse AGSE_REPO
git clone https://github.com/vadl/agse
git clone https://github.com/vadl/agse.git
git clone https://github.com/vadl/agse AGSE_REPO
```

When using the `git@<url>` syntax, it will use any configured ssh keys, otherwise it will ask you for login information.

### Working as usual

Once you've cloned the repository, you can add files and edit existing files as you want, without having to use any interface or anything.  Simply do your work as you normally do.

### _Commiting_ your work locally

Every so often (generally when you've fininshed editing a file, or reached a stopping point you should commit your work so that it becomes part of the history of the repository.  You generally want incremental changes as opposed to doing a lot of work and committing everything at once.  The reason for this is in case you need to roll back (revert) some of the edits or in case you run into a merge conflict (e.g. if you and someone else touched the same file at the same time).

When you're ready to commit you need to do three things:

* Update your local copy of the repository, this ensures that you've gotten any changes others have pushed to the repository since the last time you pulled or since when you cloned the repository:

  ```bash
  $ git pull
  ```
  
  Or in Github Desktop you simply _sync_.

* Tell git which files you've changed and want to commit:

  ```bash
  $ git add <files to add>
  ```
  
  Note: you can git add multiple times.  On the console, use `git status` to have it tell you what you've committed and what you've changed but haven't committed, and what you've added but not told git to version.
  
  If you're on Github Desktop, simply switch to the changes tab and select the files you want to add.
  
* Write a commit message and perform the commit:

  ```bash
  $ git commit -m "<commit message here>"
  ```
  
  Or if you're on Github Desktop, just write the message in the text box when you add the files.

### _Pushing_ your work to the cloud

After you've edited files, pulled updates, add committed your changes you need to push those changes to the cloud.

```bash
git push
```

or press the _sync_ button in Github Desktop.

### _Pulling_ others' work from the cloud

As stated above, pulling every so often to update your local copy of the repository is paramount to ensuring consistency.

```bash
$ git pull
```

## Going from here

## Further references

* [Git Guide](http://rogerdudler.github.io/git-guide/)
* [Using Github](https://guides.github.com/)
* [More information](https://www.mutuallyhuman.com/blog/2012/06/22/a-git-walkthrough/)