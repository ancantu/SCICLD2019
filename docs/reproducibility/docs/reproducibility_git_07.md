## Collaborating with Others

A public platform like GitHub makes it easier than ever to collaborate with others on the content of a repository. You can have as many local copies of a repository as you want, but there is only one "origin" repository - the repository hosted on GitHub. Other repositories may fall behind the origin, or have changes that are ahead of the origin. A common model for juggling multiple repositories where separate individuals are working on different features is the [GitFlow model](https://datasift.github.io/gitflow/IntroducingGitFlow.html):

![GitFlow](./fig/GitFlowMasterBranch.png)

Some important definitions (most can easily be managed right in the GitHub web interface):

#### Fork

A fork is a personal copy of another user's repository that lives on your account. Forks allow you to freely make changes to a project without affecting the original. Forks remain attached to the original, allowing you to submit a pull request to the original's author to update with your changes. You can also keep your fork up to date by pulling in updates from the original.

#### Branch

A branch is a parallel version of a repository. It is contained within the repository, but does not affect the primary or master branch allowing you to work freely without disrupting the "live" version. When you've made the changes you want to make, you can merge your branch back into the master branch to publish your changes. For more information, see [About branches](https://help.github.com/articles/about-branches).

#### Tag

 Git has the ability to tag specific points in history as being important. Typically people use this functionality to mark release points (v1.0, and so on).

#### Pull request

Pull requests are proposed changes to a repository submitted by a user and accepted or rejected by a repository's collaborators. Like issues, pull requests each have their own discussion forum. For more information, see [About pull requests](https://help.github.com/articles/about-pull-requests).

[GitHub Glossary](https://help.github.com/articles/github-glossary/)

[More on different git workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)

## Other Considerations

Most repos will also contain a few standard files in the top directory, including:

`README.md`: The landing page of your repository on GitHub will display the contents of README.md, if it exists. This is a good place to describe your project and list the appropriate citations.

`LICENSE.txt`: See if your repository needs a license [here](https://help.github.com/articles/licensing-a-repository/)


Previous: [Link a Local Repository to GitHub](reproducibility_git_06.md) | Next: [Build an App with a Github-Docker Integration](reproducibility_git_08.md) | Top: [Course Overview](../reproducibility.md)
