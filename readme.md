NEMAC Drupal Project
----------------------

This project provides scaffolding to build a containerized Drupal webserver.


### Directory structure

* **.ebextensions/** - Files run when deploying the application to Elastic Beanstalk
* **docker/** - Files used to build the docker image which runs the webserver.
* **docker/apache2/** - Apache2 config files.
* **docker/my_init.d/** - Scripts to run when the container first starts.
* **web/** - The webroot in which Drupal is installed.
* **docker-compose.yml/** - Config for running development environment locally.
* **Dockerrun.aws.json/** - Config for running staging and production environments on Elastic Beanstalk.
* **composer.json/** - Dependencies to install via composer.
* **composer.lock/** - Dependencies resolved via composer. Commit this to ensure that Composer doesn't automatically upgrade dependencies in staging/production environments. Run `composer update` in local development environment to rebuild.

## Local Development

### Setup

### Usage


### Utility Commands
Run `source utils.sh` to load utility commands.

* **drun** - Runs the local development environment. (usually preceded by `image-build`)
* **dps** - Lists running docker containers.
* **dlog** - Shows log output from current running app container.
* **dbash** - Interactive terminal in running app container.
* **build-image** - Build the docker image locally.
* **push-image** - Pushes the docker image to the image repository with the provided tag.

### Configuration
The below environment variables may be configured in `docker-compose.yml`(for dev environment), `Dockerrun.aws.json`(for staging and production), and/or via the Elastic Beanstalk environment configuration. Elastic Beanstalk environment variables overwrite those defined in `Dockerrun.aws.json`.

 Name | Type | Description
 :--- | :--- | :---
PROJECT_HOSTNAME | Hostname | The primary hostname for this environment. Ex: 'some.project.com'
PROJECT_CNAMES | Hostnames | Additional hostnames for which https certificates should be registered (comma-seperated).
ADMIN_EMAIL | Email | The email used to register ssl certs. May receive ssl cert expiry notices.
SECRETS_STORE | S3 bucket/prefix | The S3 bucket and prefix in which the secrets for this environment are stored. Ex: 's3://nemac-secrets/staging-some-project-com/')
ASSET_STORE | S3 bucket/prefix | The S3 bucket and prefix in which the assets for this environment are stored. Ex: 's3://nemac-assets-us-east-1/some-project-com/')
ENABLE_HTTPS | true\|false | Enables https and certbot.
FORCE_HTTPS | true\|false | Enables http redirect (301) to https and turns on HSTS.
ENABLE_DEBUGGING | true\|false | Enables error printing, xdebug, and more. Do not enable in production. See: [debugging](#debugging).
XDEBUG_REMOTE_HOST|IP|IP that XDEBUG should connect to. For dev environment this will be the ip of the docker-machine, for staging it will be `127.0.0.1` and ssh port forwarding should be used. Do not enable in production.

### Gotchas

- If you encounter errors about files not existing in `/vendor`, destroy it, then re-run `drun`. Do not attempt to troubleshoot permissions.
