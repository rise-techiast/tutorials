# Basic GitHub Commands
This is a list of basic GitHub commands following the standard GIT workflow.

## Commands

List your existing remotes:
```sh
git remote -v
```
Clone a repository using SSH:
```sh
git clone git@github.com:$YOUR_ACCOUNT/$PATH_TO_REPOSITORY
```
Stage the file for commit from your local repository:
```sh
## To add the current directory:
git add .

## To add all sub-directories:
git add --all
```
Create a commit with an update message:
```
git commit -m "$UPDATE_MESSAGE"
```
Push commits made on your local branch to a remote repository:
```sh
git push $REMOTE $BRANCH
```

## Reference
* _Changing a remote's URL_, GitHub Help, viewed 17 Feb 2020, <<https://help.github.com/en/github/using-git/changing-a-remotes-url#switching-remote-urls-from-https-to-ssh>>.
* _Cloning a repository from GitHub_, GitHub Help, viewed 17 Feb 2020, <<https://help.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository>>.
* _Adding a file to a repository using the command line_, GitHub Help, viewed 17 Feb 2020, <<https://help.github.com/en/github/managing-files-in-a-repository/adding-a-file-to-a-repository-using-the-command-line>>.
* _Creating a commit with multiple authors_, GitHub Help, viewed 17 Feb 2020, <<https://help.github.com/en/github/committing-changes-to-your-project/creating-a-commit-with-multiple-authors>>.
* _Pushing commits to a remote repository_, GitHub Help, viewed 17 Feb 2020, <<https://help.github.com/en/github/using-git/pushing-commits-to-a-remote-repository>>.
