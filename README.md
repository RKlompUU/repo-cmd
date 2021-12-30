# repo-cmd

Repo-cmd provides a simple framework for any scripting logic related to a git
repository, that should stay outside of the repository. For example to provide
secrets, or to specify some scripting logic that is still half-baked / in some
way not yet ready to be shared with fellow programmers.

Of course, one way to achieve this is to simply store a script inside the
related git repo, together with an entry in the .gitignore file. However, in
my experience this turned out to insufficiently stimulate consistency between
different repos.

Additionally, there turned out to be a few features that a framework could
quite easily provide:
- print out available commands to run
- provide a convenient note taking CLI
- establish a sane skeleton script for new repos

I've personally been using this framework for 2 years before having opened it
up in this public Github repo, and it definitely helped me write things down
into scripts sooner/more than I had done before. The barriers to do this are
lowered simply because of the CLI that repo-cmd elicits. One warning regarding
this however; it becomes too easy/inviting to write script logic outside of
git repos, pay attention that logic that is ready to be shared should arrive
in the git repo itself.
