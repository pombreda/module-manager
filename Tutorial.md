# Introduction #

Modman allows you to structure your code repository in a very simple way and have the contents mapped to a different location at deployment via symlinks. This has the following benefits:

  * Your project's code can appear to the application to be interspersed with other code that is not checked into your repository.
  * You don't need to use svn:externals at all (although you can). This avoids making multiple commits due to a split repository.
  * Browsing your code in an IDE such as Netbeans is very fast since you and the IDE will see only your project's code, not the other application code.

# How to Use #

Most likely you can start using modman with an existing project by simply adding one file, the **modman** file. This is a simple text file (`*`nix newlines only) which tells the script how to map your repository files into the application root. It should be placed in the root of the project. The syntax rules are simple:

  * Blank lines are ignored
  * Lines beginning with # are ignored
  * All other lines should contain two white-space separated strings

There are three types of directives, the first is a path mapping. The first path is the repository path (relative to the location of the modman file) and the second is the path it should map to when deployed.

### Example ###

Our example module is very simple, it just has a Block, a layout update, a template file and the typical Magento module files. Here is how I chose to structure the project in my repository (I like shallow paths):

```
code/
code/etc/
code/etc/config.xml
code/Block/
code/Block/View.php
design/
design/layout/
design/layout/my.xml
design/template/my/
design/template/my/view.phtml
My_Module.xml
modman
```

In order to properly deploy my module to a Magento installation my modman file would look like this:

```
# My_Module, just an example
code                   app/code/local/My/Module/
design/layout/my.xml   app/design/frontend/my/default/layout/my.xml
design/template/my     app/design/frontend/my/default/template/my
My_Module.xml          app/etc/modules/My_Module.xml
```

Assuming the above is checked into a subversion repository, I can now deploy my module like so:

```
> cd /var/www
> modman init            (this is only done once in the application root)
> modman my checkout <my_svn_url_here>
```

That is all! The project will be checked out (tucked away in `.modman/my/` in case you are curious) and symlinks will be created as needed. Parent directories of the final locations are created on demand so that additional modules can place their code next to yours.

## Handling Multiple Modules ##

Modman allows you to use more than one module in the same application root, so you may choose to put third party code in separate modules, templates in separate modules, etc.. Modman has two methods of facilitating this, either or both of which may be used as needed.

### Multiple Modules ###

Using multiple separate modules will always result in multiple working copies. Simply run `modman <module> <checkout|clone> <url>` for each module where `<module>` is a unique name of your choosing. The working copy will be located at `[ROOT]/.modman/<module>` and you can then update, commit, etc.. by specifying the same module name to modman. E.g., to update the My\_Module module to the latest version:

```
> modman my update
```

If you have lots of modules you can simply run `update-all` to update each one automatically:

```
> modman update-all
```

### Nested Modules ###

Modman also supports nested modules. Using nested modules allows you to have fewer working copies checked out if the source for your modules is all contained within one repository. You can optionally use svn:externals or git submodules to include an external module in your working copy. Then, in your modman file specify the root of the nested module using `@import` like so:

```
# My template
skin               skin/frontend/my/default
design             app/design/frontend/my/default

# Import My_Module
@import            modules/My_Module
```

In cases such as this it is actually important that the `My_Module` module is not checked out first since it contains files which are mapped to directories which are within the above module. Imports are the best way to guarantee link creation order.

See [Standalone](Standalone.md) and ShellCommand for more features.