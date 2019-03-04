# Special Topics Linux/Docker Labs

## Objective

For this lab, you will learn a little more about writing and executing shell scripts, and using docker.

## Prerequisites

* Create a "labs" directory if you don't already have one by opening a terminal
and typing:
``mkdir ~/labs``

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

1. [Fork](https://help.github.com/articles/fork-a-repo/) this repository (__don't__ "Clone or download" directly from my repository)
1. Open a terminal and change to your labs directory:
``cd ~/labs``
1. After the repo has been forked to your GitHub account, verify you are viewing your fork, then click on "Clone or download"
1. Click on the clipboard icon in the dropdown that appears to copy the URL
1. Go back to your terminal and type: ``git clone `` (with a trailing space)
1. In the terminal window, choose "Edit" -> "Paste" which should cause the repository URL to be appended to the ``git clone`` command
1. Press Enter

If all goes well you should see something like:
```
Cloning into 'special-topics-labs-linux-docker'...
remote: Enumerating objects: 31, done.
...
```

## Completing the Assignment

1. The goal of this assignment is to create a shell script that controls an application (GitLab) with docker-compose.  GitLab is a git repository server, similar to GitHub, but can be self-hosted.  
    1. Your shell script should be called ``gitlab-ctl.sh`` and it should be in the ``bin`` subdirectory of this lab
    1. Your script should accept the following options (*Hint* [getopts](http://pubs.opengroup.org/onlinepubs/9699919799.2018edition/utilities/getopts.html) is a bash built-in function that helps you process command-line arguments.  You can find a simple tutorial [here](http://pubs.opengroup.org/onlinepubs/9699919799.2018edition/utilities/getopts.html)).
        * ``-h`` - (optional argument) display help and exit with a status code 0.  The help should look similar to what you see when you pass ``--help`` to standard commands (e.g. ``grep --help``).  *Hint*: It's frequently helpful to create a ``usage`` function that prints out the correct command usage.
        * ``-d <directory>`` - (optional argument) uses the specified directory as the directory to provide postgres for storing database content.  If the ``-d`` parameter isn't used the default location should be ``/tmp/gitlab-data``
        * ``-p <port>`` - (optional argument) specifies the port GitLab should listen on when starting
    1. The final argument should be ``<command>``.  It is required and must be one of ``start``, ``stop``, or ``destroy``.  ``start`` will start your GitLab instance.  ``stop`` will stop the instance, but not remove the container.  ``destroy`` will stop any running instance and remove associated containers.  Thus, the simplest form of running your script to start GitLab should be ``gitlab-ctl.sh start``.
    1. Your script _must_ used ``docker-compose`` and an associated compose YAML file (it is fine to place it in the ``bin/`` directory) to start and stop your GitLab instances.  Your compose _must_ create postgresql and redis services to hook your GitLab instance up to (i.e. you can't run an "all-in-one" GitLab instance.   *Hint* There are lots of resources on the web to help you here.  Use them.  A nice example to work from might be [https://developer.ibm.com/code/2017/07/13/step-step-guide-running-gitlab-ce-docker/]. It will be _much_ easier to start with their compose file. 
    1. You should test that your script works by logging in to GitLab, and of course you should test all of the parameters.  The hostname to put in the url should be ``localhost``, so you should be able to point your browser to [http://localhost] if you start your GitLab with your default settings.
    1. You will need to volume mount the value passed as the ``-d`` parameter (defaulted to ``/tmp/gitlab-data`` to your postgresql instance. 
    1. You should run your service in the background (i.e. ``Detached mode``).  To see how to do this consult ``docker-compose up --help``.

## Submitting Your Work

Create a [pull request](https://help.github.com/articles/creating-a-pull-request/) to submit your work for grading:

1. Open a terminal
1. Change to the main directory for these labs: ``cd ~/labs/special-topics-labs-linux-docker``
1. [Commit your changes](https://help.github.com/en/articles/adding-a-file-to-a-repository-using-the-command-line)
1. Push your changes to GitHub: ``git push origin master`` providing your GitHub login credentials if prompted to do so
1. Navigate to your copy of the repository in GitHub
1. Click "New Pull Request"
1. From the "Comparing changes" view, click "Create pull request"
1. Enter a proper title and comments in the "Open a pull request" view
1. Click "Create pull request"
1. Verify your pull request is pending in the [main repository pulls](https://github.com/jschmersal-cscc/special-topics-labs-linux-docker/pulls) list

__NOTE: I will provide feedback via. comments in your pull request.__
If you need to amend your work after you issue your initial pull request:

1. Commit your updates
1. Push your changes to gitHub
1. Verify the new commits were automatically added to your open pull request
