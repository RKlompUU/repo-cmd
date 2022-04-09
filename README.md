# repo-cmd

Repo-cmd provides a simple framework for any scripting logic related to a git
repository, that should stay outside of the repository. For example, to provide
secrets, or to specify some scripting logic that is still half-baked / in some
way not yet ready to be shared with fellow programmers.

Of course, one way to achieve this is to simply store a script inside the
related git repo, together with an entry in the .gitignore file. However, in
my experience this turned out to insufficiently stimulate consistency between
different repos.

Additionally, there turned out to be a few features that a framework could
quite easily provide:
- print out available commands to run
- provide a convenient note taking CLI (see "Notes feature" section for more info)
- establish a sane skeleton script for new repos

I've personally been using this framework for 2 years before having opened it
up in this public Github repo, and it definitely helped me write things down
into scripts sooner/more than I had done before. The barriers to do this are
lowered simply because of the CLI that repo-cmd elicits. One warning regarding
this however; it becomes too easy/inviting to write script logic outside of
git repos, pay attention that logic that is ready to be shared should arrive
in the git repo itself.

# Installation

Simply execute the `install` bash script. This will place the repo-cmd in /usr/bin/. It will additionally install a bash_completion script inside either $BASH_COMPLETION_USER_DIR or if this is not set in ~/.local/share/bash_completion/.

For bash completions to work, make sure you've installed (bash-completion)[https://github.com/scop/bash-completion].

# Example usage

In one of your git projects, add a command (with `repo-cmd edit`) that launches
a Dockerized postgres database:
```
testdb)
    docker run \
        -p 5432:5432 \
        -e POSTGRES_PASSWORD=somePassword \
        -e POSTGRES_USER=someUser \
        -e POSTGRES_DB=someDatabase \
        postgres
    ;;
```
(Inside the `case`).

And then add another command that connects to this database with `psql`:
```
db)
    PGHOST=localhost \
        PGPORT=5432 \
        PGUSER=someUser \
        PGPASSWORD=somePassword \
        PGDATABASE=someDatabase \
        psql
    ;;
```

With these commands defined, it's now possible to start a temporary database
inside a terminal that is somewhere inside your git project with
`repo-cmd testdb`. With `repo-cmd db` you can then connect to this database in
another terminal session.

Anything goes. Maybe you want to write some convenience wrapper around API
calls you're performing with something like `curl` or `httpie`, eg:
```
login)
    set -u
    username=$2
    password=$3

    response=`http POST https://your_api.com/login <<EOF
{
    "username": "$username",
    "password": "$password"
}
EOF`
    echo "$response" | jq -r '.token'
    ;;
cart-add)
    set -u
    item_id=$2

    token=`repo-cmd login someUser somePassword`
    http https://your_api.com/cart/add/$item_id Authorization:"Bearer $token"
    ;;
```

## notes

`repo-cmd` always first changes directory to the git project root directory.
So, at entrypoint of all of your custom commands, $PWD will always be that
git root directory.

# Notes feature

`repo-cmd` additionally provides a way to make notes in 1 file per extension
per git project, by executing `repo-cmd notes.<extension>`.

For example, I use this a lot to write and store one-off SQL queries with
`repo-cmd notes.sql`, and for some projects I may use `repo-cmd notes.txt` to
store findings.
