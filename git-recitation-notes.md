# Git Lunch and Learn Notes #

ME: Talk about the approach to this lunch and learn

- What is git?
    - Git is a tool to track your code and its history of changes
    - It uses a fully decentralized model
        - You can make changes independent of other developers
        - You can use all features of git, regardless of network connectivity
    - It makes experimenting with new features and bug fixes easy
        - Branches and their flexibility makes it easy to experiment
    - It helps to protect you from yourself or others in a collaborative environment.
    - It's important to understand that git tracks _changes_, not files.

## Making a Repository ##

- A "repository" is all the folders and files you want git to track
- Git maintains a record of all this information in the `.git` folder, at the top-most folder (the root) of the repository.

```
$ mkdir my_repository
>>>
$ cd my_repository
>>>
$ git init
>>> Initialized empty Git repository in my_repository/.git/
```

- We made a directory called `my_repository`, and told git to start tracking changes inside of it

## Making a Worksp


- <http://images.osteele.com/2008/git-transport.png>

## Command Showcase ##

- Checkout a specific file from a specific commit:

```
$ git checkout