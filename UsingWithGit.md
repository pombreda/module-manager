# Introduction #

Using the `deploy` command bypasses any svn-specific functionality. As such, modman can be used with any SCM or no SCM at all.

## Git ##

Since version 1.1.0, modman supports git natively using pull and submodule update --init commands internally.

## Others ##

Other SCMs can likely be integrated easily but only git and subversion are currently supported natively (with modman's update and update-all commands).