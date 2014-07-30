Deeeploy
========

Deployment role capistrano like for PHP with composer projects. The role deploy a git project to a project path on a server under `releases`. And make a symink of the latest release to a `current` path. It also support shared files/folders and configuration files with jinja2 variables (named .dist).

Requirements
------------

You need a `shared` folder on the server you want to deploy. With the exact structure of shared files/folers you want (ie: foo/bar => shred/foo/bar).

Role Variables
--------------

The next variables are mandatory:

* `deeeploy_repo_url`
* `deeeploy_project_path`

The next variables are optional:

* `deeeploy_branch` (default master)
* `deeeploy_config_files` (default [])
* `deeeploy_shared_files` (default [])
* `deeeploy_clean_files` (default [".git"])

You can also set a `deeeploy_newrelic_token` and a `deeeploy_newrelic_app_id` to trigger a deploy event on your newrelic dashboard.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
- hosts: webservers
  roles:
     - { role: Xotelia.Deeeploy, deeeploy_repo_url: git@github.com:foo/bar.git, deeeploy_project_path: /foo/bar }
```

License
-------

Deeeploy is licensed under the [MIT license](LICENSE).
