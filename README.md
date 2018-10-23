# Node Cleanser

Remove `node_modules` from your inactive projects.

## Install

To-Do: add install instructions

## Usage


#### Passing project root directory

`nclean` Looks for npm projects in provided directory. For example, `Documents/projects` is place where you have all your projects.


1. Using Current working directory as entry - simply change directory to `Documents/projects` and run:

`nclean`

2. Provide project directory:

`nclean ~/Documents/projects`


#### Ignore projects

By passing `--ignore` or `-i` argument, you can exclude project(s) from from `nclean`

`nclean -i betaProject "My super project"`


