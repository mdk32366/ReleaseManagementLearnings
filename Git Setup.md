#### [Setting up Git](https://help.github.com/en/articles/set-up-git#setting-up-git)

Reference: [Git Configuration](https://git-scm.com/book/en/Customizing-Git-Git-Configuration) from the Pro Git book.

[Download and install the latest version of Git.](https://git-scm.com/downloads)

##### Set your username in Git.
Git uses a username to associate commits with an identity. The Git username is not the same as your GitHub username.

You can change the name that is associated with your Git commits using the git config command. The new name you set will be visible in any future commits you push to GitHub from the command line. If you'd like to keep your real name private, you can use any text as your Git username.

Changing the name associated with your Git commits using git config will only affect future commits and will not change the name used for past commits.

Setting your Git username for every repository on your computer
Open Git Bash.

Set a Git username:

    $ git config --global user.name "Mona Lisa"
Confirm that you have set the Git username correctly:

    $ git config --global user.name
    > Mona Lisa

Setting your Git username for a single repository
Open Git Bash.

Change the current working directory to the local repository where you want to configure the name that is associated with your Git commits.

Set a Git username:

    $ git config user.name "Mona Lisa"
Confirm that you have set the Git username correctly:

    $ git config user.name
    > Mona Lisa

Further reading:

[Setting your commit email address in Git](https://help.github.com/en/articles/setting-your-commit-email-address-in-git)

[Git Configuration from the Pro Git book](https://git-scm.com/book/en/Customizing-Git-Git-Configuration)

##### [Set your commit email address in Git.](https://help.github.com/en/articles/setting-your-commit-email-address-in-git)

GitHub uses the email address set in your local Git configuration to associate commits pushed from the command line with your GitHub account.

You can use the git config command to change the email address you associate with your Git commits. The new email address you set will be visible in any future commits you push to GitHub from the command line. Any commits you made prior to changing your commit email address are still associated with your previous email address.

For more information on commit email addresses, including your GitHub-provided noreply email address, see "About commit email addresses."

Setting your email address for every repository on your computer
Open Git Bash.

Set an email address in Git. You can use your GitHub-provided no-reply email address or any email address.

    $ git config --global user.email "email@example.com"
Confirm that you have set the email address correctly in Git:

    $ git config --global user.email
    email@example.com
Add the email address to your GitHub account by setting your commit email address on GitHub, so that your commits are attributed to you and appear in your contributions graph.

Setting your email address for a single repository
You can change the email address associated with commits you make in a single repository. This will override your global Git config settings in this one repository, but will not affect any other repositories.

Open Git Bash.

Change the current working directory to the local repository where you want to configure the email address that you associate with your Git commits.

Set an email address in Git. You can use your GitHub-provided no-reply email address or any email address.

    $ git config user.email "email@example.com"
Confirm that you have set the email address correctly in Git:

    $ git config user.email
    email@example.com
Add the email address to your GitHub account by setting your commit email address on GitHub, so that your commits are attributed to you and appear in your contributions graph.

Further reading:

[About commit email addresses](https://help.github.com/en/articles/about-commit-email-addresses)

[Setting your commit email address on GitHub](https://help.github.com/en/articles/setting-your-commit-email-address-on-github)

[Git Configuration](https://git-scm.com/book/en/Customizing-Git-Git-Configuration) from the Pro Git book
