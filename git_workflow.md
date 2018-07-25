# GIT Workflow

1.  On master, pull from origin master
2.  Create a feature branch (feature/feature_name)
3.  Update your branch with the latest changes from master

a. git checkout master
b. git pull origin master
c. git checkout feature/feature_name (use git branch as needed)
d. git merge master (merge master into the feature branch)
e. resolve conflicts and commit changes

4.  Push the feature branch to github

- git push origin feature/feature_name

5.  Create a pull request on GitHub

- you should not merge your own pull request

6.  checkout to master and pull again from origin master
