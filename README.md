# Deeeploy

Ansible deployment role for PHP projects :

* clone a git repo on a server in `releases\{timestamp}` directory,
* use Composer to load dependencies,
* update config files using Jinja2 variables (`.dist`),
* create symlinks to share files/folders beetween releases,
* delete irrelevant files,
* make a `current` symlink to the latest release,
* notify New Relic and Slack.

## Structure

The structure on the remote server looks like :

```shell
$ tree -L 3 /home/your-name
/home/your-name
└── your-app
    ├── current -> /home/your-name/your-app/releases/20150701144412
    ├── releases
    │   ├── 20150623111930
    │   ├── 20150625091111
    │   ├── 20150630141139
    │   ├── 20150630141346
    │   ├── 20150630144820
    │   ├── 20150701135925
    │   └── 20150701144412
    └── shared
        ├── app
        └── web
```

## Variables

The next variables are mandatory :

* `deeeploy_repo_url` : your git repo to deploy,
* `deeeploy_project_path` : path on your server.

The next variables are optional :

* `deeeploy_branch` : branch to deploy (default `master`),
* `deeeploy_config_files` : config files to replace with parameters value (default `[]`),
* `deeeploy_shared_files` : files to shared between releases (default `[]`),
* `deeeploy_clean_files` : irrelevant files to delete after deployment (default `[".git"]`),
* `deeeploy_version_filename` : file containing deployed hash (default `VERSION`),
* `deeeploy_custom_shell_script` : custom shell script to execute,
* `deeeploy_composer_env` : Composer option : `dev` or `prod` (default `dev`).

To use notifications :

* set a `deeeploy_newrelic_token` and a `deeeploy_newrelic_app_id` to trigger a deploy event on [New Relic](http://newrelic.com/).
* set a `deeeploy_slack_token` and a `deeeploy_slack_channel` to trigger a deploy notification on [Slack](https://slack.com/).

## Example Playbook

```
---
- hosts: all
  roles:
    -
        role: deeeploy
        deeeploy_repo_url: "git@github.com:You/YourProject.git"
        deeeploy_project_path: /home/your-name/your-app
        deeeploy_config_files:
          - 'app/config/parameters.yml'
        deeeploy_shared_files:
          - "app/cache"
          - "app/logs"
        deeeploy_clean_files:
          - '.git'
          - 'Vagrantfile.dist'
        deeeploy_version_filename: "web/VERSION"
        deeeploy_custom_shell_script: "scripts/deploy-custom.sh"
        deeeploy_newrelic_token: "As606c9f06706f6da0fz2448559f969e7b355f990x8d78d"
        deeeploy_newrelic_app_id: "7635473"
        deeeploy_slack_token: "R0796HXRX/B0745RDUS/DzJaLzBtzH8ctXklYJxoxItM"
        deeeploy_slack_channel: "#deploy"
```

## Shared files

To shared files/folders beetween releases, you need a dedicated folder on the remote server :

* it has to be named `shared`,
* it has to be at the root of `deeeploy_project_path`,
* it has to have the exact same structure of files/folders you want to shared (`foo/bar` => `shared/foo/bar`).

## License

Licensed under the [MIT license](LICENSE).
