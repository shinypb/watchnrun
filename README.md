# watchnrun
Watch a file (or directory) for changes and run a command in response.

## What
`watchnrun` is a simple script that lets you specify a file or directory to watch, and then runs a script in response to changes when they happen. `watchnrun` will keep watching until you terminate it.

Example output:
````
$ watchnrun example-file example-script.sh 
2019-01-22 18:56:30 -0800 Watching example-file for changes
2019-01-22 18:56:34 -0800 example-file changed; running example-script.sh
I am the script and I am running!
2019-01-22 18:56:37 -0800 example-file changed; running example-script.sh
I am the script and I am running!
````

## Why
This is part of an elaborate [Rube Goldberg machine](https://en.wikipedia.org/wiki/Rube_Goldberg_machine) that I've wired up to get [my Jekyll blog](https://writing.markchristian.org) to rebuild itself whenever I push a change to its repository, without needing to have my web server process have any elevated permissions (which is particularly important on shared hosting like [DreamHost](https://www.dreamhost.com)). I should probably write up the details of that elsewhere in more detail, but the basic idea is:

* A GitHub webhook hits an endpoint on my domain
* That endpoint touches a file in a particular location
* Meanwhile, an instance of `watchnrun` running inside of a detached `screen` process watches that location for changes
* Whenever that file changes, `watchnrun` executes a script that fetches the latest version of my blog's repository from GitHub and runs `jekyll build`.
