# GitHub Tag Commands
A list of GitHub commands in regards to Tags.

#### Checkout a tag
```sh
git checkout $TAG_NAME
```
#### List all tags
```sh
git tag
```
#### Create a tag
Create a new tag:
```sh
git tag $TAG_NAME
```
Create a new tag with a message:
```sh
git tag -a $TAG_NAME -m "$MESSAGE"
```
#### Push a tag
Push your created tags:
```sh
git push --tags
```
#### Delete a Tag
Delete a local tag:
```sh
git tag -d $TAG_NAME
```
Delete a remote tag:
```sh
git push origin --delete $TAG_NAME
```
## References
* _2.6 Git Basics - Tagging_, Git, viewed 17 Feb 2020, <<https://git-scm.com/book/en/v2/Git-Basics-Tagging>>.