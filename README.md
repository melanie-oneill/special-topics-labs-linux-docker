# Special Topics Linux/Docker Labs

## Objective

For this lab, you will learn a little more about writing and executing shell scripts, and using docker.  You will also gain a little experience with [Docker Compose](https://docs.docker.com/compose/).  Frequently your project will require more than one service to function (e.g. an application server, load balancer, and database server).  

Docker Compose lets you specify the docker images you want to run (with configuration for each service) in a YAML file and will launch all of the containers and link them together.  Starting your complex application is then as simple as invoking `docker-compose -f <my-compose-specification>.yml up` to start your entire application.

## Prerequisites

* Install ``docker-compose``:
1. Run the following command: ``sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose``

If all goes well, you should see something like:
```
[CORP\jschmersal1@a-1qu102vu4ll0k lab]$ sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   617    0   617    0     0   7810      0 --:--:-- --:--:-- --:--:--  7810
100 11.2M  100 11.2M    0     0  50.6M      0 --:--:-- --:--:-- --:--:--  107M

```
1. Next, make docker-compose executable by running: ``sudo chmod +x /usr/local/bin/docker-compose``
1. Verify docker-compose is installed correctly by running: ``docker-compose --version``.  Your output should look similar to:
```
[CORP\jschmersal1@a-1qu102vu4ll0k lab]$ docker-compose --version
docker-compose version 1.23.2, build 1110ad01

```


## Getting Started:

1. Copy the starter code from here into a new, private repository in your personal GitHub account using [these instructions](https://github.com/jeff-anderson-cscc/submitting-assignments-lab#copy-the-starter-code-into-a-new-private-repository-in-your-personal-github-account) substituting this repository URL ``https://github.com/jschmersal-cscc/special-topics-labs-linux-docker.git`` for the one referenced in that document
2. Create a new branch for your code changes as described in [these instructions](https://github.com/jeff-anderson-cscc/submitting-assignments-lab#before-you-start-coding)



## Completing the Assignment

1. The goal of this assignment is to create a shell script that controls an application (GitLab) with docker-compose.  GitLab is a git repository server, similar to GitHub, but can be self-hosted.  
    1. Your shell script should be called ``gitlab-ctl.sh`` and it should be in the ``bin`` subdirectory of this repo
    1. Your script should accept the following options (*Hint* [getopts](http://pubs.opengroup.org/onlinepubs/9699919799.2018edition/utilities/getopts.html) is a bash built-in function that helps you process command-line arguments.  You can find a simple tutorial [here](http://pubs.opengroup.org/onlinepubs/9699919799.2018edition/utilities/getopts.html)).
        * ``-h`` - (optional argument) display help and exit with a status code 0.  The help should look similar to what you see when you pass ``--help`` to standard commands (e.g. ``grep --help``).  *Hint*: It's frequently helpful to create a ``usage`` function that prints out the correct command usage.
        * ``-d <directory>`` - (optional argument) uses the specified directory as the directory to provide postgres for storing database content.  If the ``-d`` parameter isn't used the default location should be ``/tmp/gitlab-data``
        * ``-p <port>`` - (optional argument) specifies the port GitLab should listen on when starting
    1. The final argument should be ``<command>``.  It is required and must be one of ``start``, ``stop``, or ``destroy``.  ``start`` will start your GitLab instance.  ``stop`` will stop the instance, but not remove the container.  ``destroy`` will stop any running instance and remove associated containers.  Thus, the simplest form of running your script to start GitLab should be ``gitlab-ctl.sh start``.
    1. Your script _must_ used ``docker-compose`` and an associated compose YAML file (it is fine to place it in the ``bin/`` directory) to start and stop your GitLab instances.  Your compose _must_ create postgresql and redis services to hook your GitLab instance up to (i.e. you can't run an "all-in-one" GitLab instance.   *Hint* There are lots of resources on the web to help you here.  Use them.  A nice example to work from might be [https://developer.ibm.com/code/2017/07/13/step-step-guide-running-gitlab-ce-docker/]. It will be _much_ easier to start with their compose file. 
    1. You should test that your script works by logging in to GitLab, and of course you should test all of the parameters.  The hostname to put in the url should be ``localhost``, so you should be able to point your browser to [http://localhost] if you start your GitLab with your default settings.
    1. You will need to volume mount the value passed as the ``-d`` parameter (defaulted to ``/tmp/gitlab-data`` to your postgresql instance. 
    1. You should run your service in the background (i.e. ``Detached mode``).  To see how to do this consult ``docker-compose up --help``.

## Hints
1. Your reading assignments include overviews of writing shell scripts and docker compose.  I suggest you read through them to get a good intro to both.
1. The [https://developer.ibm.com/code/2017/07/13/step-step-guide-running-gitlab-ce-docker/] site talks about Kubernetes, but you may safely ignore it.  We won't be using Kubernetes in this lab
1. The [docker-compose.yml](https://github.com/IBM/Kubernetes-container-service-GitLab-sample/blob/master/docker-compose.yml) file specified in that IBM blog entry is a great place to start.  Note that it starts GitLab on port 30080 (`- "30080:30080"`), so you might need to fix it up.  You can specify an environment variable in the yml file as long as you set it in your script before running `docker-compose`.
1. I would suggest you develop your script in multiple phases, to make it easier.  For example:
    1. Download the `docker-compose.yml` and run various `docker-compose` invocations (`docker-compose up`, `docker-compose stop`, etc.) to become familiar with running docker-compose.
    2. Have your shell script invoke `docker-compose`.  You can start by hardcoding the `docker-compose up` and then use a command line argument to choose up/down/stop/etc.
    3. Finally, add in the command line arguments you need to support.
1. Note that with `docker-compose` you need not specify the yml file you want to use, but you can.  You can run `docker-compose -f <location-of-my-yml-file>.yml up` or `docker-compose up`.  If you don't specify the YML file, docker-compose will look in the current directory for a file named `docker-compose.yml`.  If it doesn't find it, it will complain.  When testing your lab, I will run the command from your `bin/` directory so you don't have to deal with Linux paths if you don't want to.
1. Note that your `gitlab-ctl.sh` script takes "start", but `docker-compose` takes "up".  

## Submitting Your Work

1. Create a pull request for your branch using [these instructions](https://github.com/jeff-anderson-cscc/submitting-assignments-lab#once-you-are-ready-to-submit-your-work-for-grading)
1. Submit the assignment in Blackboard as described in [these instructions](https://github.com/jeff-anderson-cscc/submitting-assignments-lab#once-your-pull-request-is-created-and-i-am-added-as-a-reviewer)

__NOTE: I will provide feedback via. comments in your pull request.__
If you need to amend your work after you issue your initial pull request:

1. Commit your updates
1. Push your changes to gitHub
1. Verify the new commits were automatically added to your open pull request
