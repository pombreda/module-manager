Modman can be run without a VCS as of 1.0.3 by using the _**deploy**_ command.

The modman file format is no different. The only difference is that rather than using modman checkout to create your repository, you copy the file to the same location manually. That location is relative to the project root: **.modman/<module name>**

Example:

Files located under /home/colin/test
```
testdir/
testdir/file
testfile
modman
```

Deploying /home/colin/test
```
cd /var/www/test
modman init
cp -r /home/colin/test .modman/test
modman test deploy
```