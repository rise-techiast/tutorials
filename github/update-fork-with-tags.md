# Update a GIT Fork with Tags

Clone your forked repository:
```sh
git clone git@github.com:$YOUR_ACCOUNT/$PATH_TO_FORKED_REPOSITORY
cd $FORKED_REPOSITORY
```
Fetch changes of the original or upstream repository:
```sh
git remote add upstream $HTTPS_PATH_TO_UPSTREAM_DIRECTORY
git pull --rebase --all
git fetch upstream
git rebase upstream/master
```
Update the forked repository:
```sh
git pull --rebase --all
git push
git push --tags
```

## References
* _Fork a repo_, GitHub Help, viewed 17 Feb 2020, <<https://help.github.com/en/enterprise/2.13/user/articles/fork-a-repo>>.