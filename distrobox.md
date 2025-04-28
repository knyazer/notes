### gh setup

* running `gh auth login` - you need to do this only once, and, if rerunning, gh is not gonna tell you that you are already logged in. So use `gh auth status`
* when running `git push` it uses a global credential.helper, and the root system has likely overriden it. So, you should add the correct credential helper by hand (you can keep the old one, it will just be skipped). Run `git config --global credential.helper "/usr/bin/gh auth git-credential` (or whatever the path to the gh executable is)
