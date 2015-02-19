# Ansible role for GitLab

This role installs and configures GitLab on Ubuntu or other Debian-based distro that uses Upstart \*.

_\* If you’re using Gentoo, then switch to branch [master](https://github.com/jirutka/ansible-role-gitlab/tree/master). Note: I know how to support more distros in one role, but I don’t want to._


## Directory structure

This roles doesn’t follows bad-practices from the official GitLab installation guide about keeping _everything_ inside `/home/git`.
I’ve changed the structure to be more compliant with ([FHS](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)).
However, it’s not written in stone, most of the paths can be simply customized using variables (see [defaults/main.yml](defaults/main.yml)).

    /
    |-- etc
    |   |-- logrotate.d
    |   |   `-- gitlab
    |   |-- gitlab  # site specific configuration (symlinked to /opt/gitlab/config)
    |   |   |-- database.yml
    |   |   |-- gitlab_shell.secret
    |   |   |-- gitlab-shell.yml
    |   |   |-- gitlab.yml
    |   |   |-- resque.yml
    |   |   `-- unicorn.rb
    |   `-- init  # configs for upstart jobs/services
    |       |-- gitlab.conf          # master config for GitLab that controls the ones below
    |       |-- gitlab-sidekiq.conf  # config for background processes (Sidekiq)
    |       `-- gitlab-web.conf      # config for web UI
    |-- opt
    |   |-- gitlab  # cloned gitlab sources
    |   |   `-- public  # static files served by a web server
    |   `-- gitlab-shell
    |       `-- hooks
    |-- run
    |   `-- gitlab
    |       |-- gitlab.sock
    |       `-- unicorn.pid
    `-- var
        |-- lib
        |   `-- git  # home directory of the git user
        |       |-- .ssh
        |       |   `-- authorized_keys  # managed by gitlab-shell
        |       |-- repo_satellites  # checked out repositories for merge requests and editing from web UI
        |       |-- repositories     # bare repositories (SHOULD BE BACKED UP!)
        |       `-- .gitconfig
        |-- log
        |   `-- gitlab  # both gitlab and gitlab-shell logs
        `-- tmp
            `-- gitlab  # temporary files
                |-- cache     # assets cache
                `-- download  # repository downloads directory


## License

This project is licensed under [MIT license](http://opensource.org/licenses/MIT).
