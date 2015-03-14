# Shell Command #

Modman allows an update, checkout or deploy to invoke shell commands with the @shell operator. Two environment variables are exported before the command is run:
  * PROJECT  - path to project root
  * MODULE   - path to module root

Example:

```
# Deploy some modules
@import  modules/My_Module
@import  modules/Your_Module

# Clear the cache
@shell   rm -rf $PROJECT/var/cache/* && echo "Cache cleared"
```

The shell command will run with **every** update, not just the first. As such it could also be used to copy files to a location within the root if for some reason symlinks did not get the job done.

```
# These file don't work when symlinked
@shell    rm -rf ../../lib/blah; cp -rf lib/blah ../../lib/blah
```