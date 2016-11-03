# CFML Package for Sublime Text 3

This package provides CFML syntax highlighting as well as function and tag completions. It also has a number of other features, please see below for more information.

You can install this package via [Package Control](https://packagecontrol.io/). Please restart Sublime Text after installation.

Manual installation is also possible by downloading the repository and placing it in a folder within your Sublime packages folder. (See below for more information.)

*Please ensure that you are running at least build 3103 of Sublime Text 3. As of v0.18.0 this package only uses `sublime-syntax` syntax highlighting.*

### Acknowledgements

This package was developed from the following packages:

* https://github.com/SublimeText/ColdFusion
* https://github.com/sublimehq/Packages (especially the HTML and JavaScript syntaxes)
* https://github.com/Benvie/JavaScriptNext.tmLanguage

The current CFScript syntax is based on Will Bond's ([@wbond](https://github.com/wbond)) JavaScript syntax rewrite:

* https://github.com/sublimehq/Packages/blob/master/JavaScript/JavaScript.sublime-syntax

Special thanks to [@foundeo](https://github.com/foundeo) and [cfdocs.org](http://cfdocs.org), from which this package gets its function and tag data. Also, thanks to [@mjhagen](https://github.com/mjhagen) who helped me get this package off the ground.

### Table of Contents

- [Completions](#completions)
- [Inline Documentation](#inline-documentation)
- [A Note about Settings](#a-note-about-settings)
- [Default Key Bindings](#default-key-bindings)
- [CFC Indexing and Dot Paths](#cfc-indexing-and-dot-paths)
- [Custom Tags](#custom-tags)
- [Code Formatting](#code-formatting)
- [Custom Coloring for CFML Tags](#custom-coloring-for-cfml-tags)
- [Controller/View Toggle](#controllerview-toggle)
- [CommandBox](#commandbox)
- [TestBox](#testbox)
- [Framework One](#framework-one)
- [Manual Installation](#manual-installation)

### Completions

Completions are included for tags and tag attributes, as well for built-in functions and member functions. Completions are also available for `Application.cfc` settings and methods.

There are three completions styles for built-in functions. This is controlled by a package setting, `cfml_bif_completions`, which can be set to one of three options: `basic`, `required`, or `full`.  Setting this to `basic` will cause only the function name to be completed, with no function parameters inserted. `required`, which is the default setting, includes required parameters in the completion, but only their names, and no types. Setting this to `full` results in all parameters being included in the completion, as well as the parameters types.

When entering a function call parameter for a built-in function a pop-up window with documentation from [cfdocs.org](http://cfdocs.org) for that parameter is shown. This can be disabled by setting `cfml_completion_docs` to `false` in your user package settings.

### Inline Documentation

<kbd>F1</kbd> is mapped to an inline documentation command that provides an inline documentation popup based on the cursor position.

*You can always override the default key binding in your user key bindings file.*

If the documentation command is run when the cursor is within a built-in function or tag it will load the [cfdocs.org](http://cfdocs.org) documentation for that function or tag. Thus, having the cursor anywhere within `dateFormat(myDate, "yyyy-mm-dd")` and pressing <kbd>F1</kbd> (by default) will trigger a popup displaying the documentation for `dateFormat`. Similarly, having the cursor anywhere within `<cfinclude template="myOtherTemplate.cfm">` and pressing <kbd>F1</kbd> will trigger the display of the documention for `cfinclude`. Inline documentation is also available for `Application.cfc` settings and methods.

*If you have a copy of the cfdocs.org GitHub repository on your file system, there is a package setting, `"cfdocs_path"`, which can be set to the data directory of that repository (e.g. `"cfdocs_path": "C:/github/cfdocs/data/en/"`). If this is set, the cfdocs.org documentation will be loaded from the file system instead of being fetched via HTTP request. (See below for more information on package settings.)*

### A Note about Settings

There are several settings files that you should be aware of, as they control much of the functionality of the package. It is important to know how to access them. There are two ways to access each file, the first being through the menus, and the second being via the command palette, which you open with <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> on Windows (<kbd>CMD</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> on a Mac).

**Package Settings**

You can access the default package settings from the menu under `Package Settings -> CFML -> Package Settings - Default` or via the command palette: `CFML: Package Settings - Default`. To override any of these settings, use the user package settings file. This can be accessed under the menu `Package Settings -> CFML -> Package Settings - User` or via the command palette: `CFML: Package Settings - User`.

**Project Settings**

Some settings are also (or only) able to be set on a per project basis. These settings control the behavior of the package for a given project. You can access and edit project files from the menu under `Project -> Edit Project` or via the command palette: `Project: Edit Project`. (These will only be accessible after you have saved a project: `Project -> Save Project As` or `Project: Save As`.)

**Bindings**

The default keyboard and mouse binding files are available in the menu under `Package Settings -> CFML -> Key Bindings - Default` and `Package Settings -> CFML -> Mouse Bindings - Default`. Or via the command palette at `CFML: Key Bindings - Default` and `CFML: Mouse Bindings - Default`. The user key and mouse  binding files are available in the same locations.

### Default Key Bindings

In tag attributes, script strings, and between `cfoutput` tags, pressing `#` when text is selected will wrap the currently selected text `#selected#`.

<kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>D</kbd> will output a `<cfdump>` tag or `writeDump()/dump()` function in CFML script, wrapping any currently selected text.

<kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>O</kbd> will output a `<cfoutput>` tag or `writeOutput()` function in CFML script, wrapping any currently selected text.

<kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>A</kbd> will output a `<cfabort>` tag or `abort;` in CFML script.

If Sublime Text's `auto_close_tags` setting is true, when a closing tag's `/` has been pressed, the closing tag will be completed. There are two package settings that control which CFML and HTML tags should not be closed: `cfml_non_closing_tags` and `html_non_closing_tags`.

If the package setting `cfml_auto_insert_closing_tag` is set to `true` (by default it is `false`), when `>` is pressed in a tag, the corresponding closing tag will be automatically inserted after the cursor position. The package setting `cfml_non_closing_tags` controls which CFML tags will not get a closing tag auto inserted.

The package setting `cfml_between_tag_pair` controls the behavior of the editor when <kbd>ENTER</kbd> is pressed while the cursor is between a CFML tag pair (e.g. `<cfoutput>|</cfoutput>`). By default only a single new line is inserted. This can be changed to have an extra new line auto inserted between the tag pair (with an optional indent), and the cursor placed there. See the default package settings file for more information.

### CFC Indexing and Dot Paths

There are two settings that are set on a per project basis in a project file, `cfc_folders` and `mappings`. Here is an example (a real project file will have other settings in it as well):

```
{
    "mappings": [
        {"mapping": "/framework", "path": "C:/myprojects/projectname/framework"},
        {"mapping": "/model", "path": "C:/myprojects/projectname/app/model"}
    ],
    "cfc_folders": [
        {
            "path": "C:/myprojects/projectname/app/model",
            "variable_names": ["{cfc}", "{cfc}{cfc_folder_singularized}"],
            "accessors": false
        }
    ]
}
```
*__Note__: the paths specified above can either be absolute paths, or relative paths. If they are relative paths, they will be calculated relative to the location of the `.sublime-project` file.*

The specified mappings are used to resolve file paths to dot paths when right clicking on a CFC file in the sidebar and selecting `Copy CFC Dotted Path`. If more than one mapping matches, you will be given a choice, and if none match, it will fall back to using the base folder of the project as a dot path root. The mappings are also used to resolve dot paths specified in the `extends` attribute in a component, the `createObject` function, and when using the `new` operator to instantiate a component (e.g. `new dot.path.to.component()`). If the <kbd>F1</kbd> command is used when the cursor is in any of these, you will get a popup with the file path for the component (plus more information about the component if it is one that has been indexed). You can click on the file path to jump to the component file. You can also <kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Left Click</kbd> (<kbd>CMD</kbd>+<kbd>ALT</kbd>+<kbd>Left Click</kbd> on a Mac) on any of these to jump straight to the component file without going through the popup. In addition, these same commands will work in any quoted string that is used as a function call argument if it contains a dot path that can be resolved.

*You can see the structure of this mouse binding in the `Mouse Bindings - Default` file. Use the `Mouse Bindings - User` file to create your own.*

*__Important__: mouse bindings are global throughout Sublime Text, so use caution to ensure you don't override a default Sublime Text mouse binding (such as <kbd>CTRL</kbd>+<kbd>Left Click</kbd> which adds a new cursor at the position clicked).*

The `cfc_folders` array contains objects with one required key: `path`. Folders specified here are recursively searched for CFC files and those files are indexed. The `variable_names` setting is an array of strings, where each string specifies a variable name to associate each indexed CFC with. The names are determined via substitution as follows:

| Key                       | Substitution                                                |
|:--------------------------|:------------------------------------------------------------|
| {cfc}                     | CFC name                                                    |
| {entityname}              | entity name of CFC if it has one, otherwise CFC name        |
| {cfc_folder}              | name of folder containing the CFC                           |
| {cfc_folder_singularized} | name of folder containing the CFC with trailing 's' removed |

Any other text in the string will be left in place - this allows for specifying a prefix or suffix to use in the variable names. Then, if that variable name is typed in your code and followed with a <kbd>.</kbd>, method completions from the corresponding CFC will be offered. A variable name can contain periods in it (e.g. `application.services.{cfc}`). You can also use the <kbd>F1</kbd> documentation command on these variable names as well as their methods calls to view documention regarding the CFC and its methods. (Note: the <kbd>F1</kbd> command also works on property names, if they match a cfc variable name, to work with DI frameworks like DI/1 that inject beans into properties of the same name.) In addition, you can use <kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Left Click</kbd> to jump to the CFC or to the specific method in the CFC.

By default, implicit accessors are included in the completions.The `accessors` key, if present and set to false, will remove implicit property accessors from the completions offered (explicit accessors will always be included).

These two settings work together, such that when a base CFC as well as another CFC that extends it are both indexed, and a mapping is specified such that the dot path in the `extends` can be resolved, the documentation and completions for the child CFC will include properties and methods from the base CFC. In addition, the dot paths of all indexed CFCs will be offered as completions in the `extends` attribute, the `createObject` function (where the type is `component`), as well as when using `new` to instantiate a CFC.

Further, when an instantiated component has been assigned to a variable (e.g. `myVar = new path.to.mycfc()`), and that component has been indexed and a mapping specified for it, then method completions for that component will be offered when a `.` is typed after that variable name. The <kbd>F1</kbd> documentation command and the <kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Left Click</kbd> jump to CFC command will also work on that variable name. (This behavior can be disabled by setting `instantiated_component_completions` to `false` in your user package settings.)

*__Note__: completions from indexed components are available in three styles matching the completion styles for built-in functions. This is controlled by the `cfml_cfc_completions` package setting. Also, when entering parameters for indexed component method calls, a pop up window with information regarding that parameter is shown.*

**Inject Property Command**

<kbd>SHIFT</kbd>+<kbd>ALT</kbd>+<kbd>D</kbd> is bound to command that will insert a property into a component. If it is run while the cursor is on one of the above mentioned component variable names or method calls, it will insert a property with its name set to the component variable name. Otherwise, it will present a list of all component variable names indexed in the given project, allowing one to be selected from the list and inserted. The default property templates used by the insert command can be found in the default package settings under the `di_property` key. These can be overriden in the user package settings, or on a per project basis by adding the same settings to a project settings file. By default, after a new property has been inserted all properties in the component will be sorted by name; this can be turned off by setting the `sort_properties` key in the `di_property` object to false.

### Custom Tags

Support is offered on a per project basis for custom tags as used with a `<cfimport>` tag and a custom tag prefix. Here is an example project file (other settings have been removed):

```
{
    "custom_tag_folders":
    [
        {
            "path": "C:/myprojects/projectname/customtags/page",
            "prefix": "page"
        }
    ]
}
```

*__Note__: the path specified above can either be an absolute path, or a relative path. If it is a relative path, it will be calculated relative to the location of `.sublime-project` file.*

Once this is done, `page` will be offered as a custom tag prefix completion, and then after entering `<page:` a list of tags in the custom tag folder will be offered. After a tag has been entered, completions for every `attributes` scope variable found in that custom tag file will be offered.

You can use the <kbd>F1</kbd> documentation command to get some minimal documentation about that custom tag, and <kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Left Click</kbd> can be used to jump to the custom tag file.

The indexer will search for `thistag.executionmode is/eq/== "end"/'end'` (case insensitive) or `<cfcase value="end"/'end'>` (case insensitive) in custom tag files in order to determine whether or not to auto close the custom tag.

### Code Formatting

This package includes several CFScript code formatting commands. You can view the default settings for these commands via the menu: `Package Settings -> CFML -> Format Settings - Default`, or from the command palette via `CFML: Format Settings - Default`. Please view these settings to see the various formatting options available. These settings can be overridden in your user format settings file, which can be found in the same locations.

There are two key bindings included that work with these commands: <kbd>SHIFT</kbd>+<kbd>ALT</kbd>+<kbd>F</kbd> and <kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>SHIFT</kbd>+<kbd>F</kbd> (<kbd>CMD</kbd>+<kbd>ALT</kbd>+<kbd>SHIFT</kbd>+<kbd>F</kbd> on a Mac) The first executes a default set of commands (specified in the settings file (`cfml_format.sublime-settings`), while the second calls up a menu from which formatting commands can be selected. The <kbd>SHIFT</kbd>+<kbd>ALT</kbd>+<kbd>F</kbd> command formats only the current method when in a component - though if any text is selected it formats that text, and in `.cfm` files it formats all CFScript in the file. To format an entire component the `Format Full Component` menu command can be used. All of the individual command options in the menu format the whole file unless text is selected.

*PLEASE NOTE:*
The reason formatting is limited by default to the current method in components is that the formatting is CPU intensive and is currently run on the main thread, meaning Sublime Text will "freeze" while formatting is ongoing. On small blocks of code, this should be very quick, but on a large file it can take some time.

### Custom Coloring for CFML Tags

Unless you are using a specialized color scheme, CFML and HTML tags will receive the same coloring. This can make it a bit harder to distinguish between the two types of tags when embedding CFML tags in HTML. This package has a command you can run from the command palette that will inject custom colors for CFML tags into your current color scheme (or remove them if they are already there). Press <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> to bring up the command palette (<kbd>CMD</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> on a Mac) and run `CFML: Toggle Color Scheme Styles`. You can edit the styles that will be injected via the user settings for this package.

*Caveat:* This feature works by either overriding or modifying your active color scheme file. Because of this, it may not work well with other packages that also modify the active color scheme in some way. Also, if the package containing your active color scheme is updated, it is likely that you will need to toggle the custom tag coloring off and then on again to pick up any changes.

### Controller/View Toggle

CFML MVC frameworks typically have the convention that a `controllers` and a `views` folder are contained in the same parent directory, and that controller names and methods correspond to view folder and file names. <kbd>CTRL</kbd>+<kbd>F1</kbd> (<kbd>CMD</kbd>+<kbd>F1</kbd> on a Mac) is mapped to a toggle command that will jump the editor back and forth between a view file and the controller method that corresponds to it. The settings which determine which folder names are regarded as controller and view folders are contained in the package settings file. By default, `controllers` and `handlers` are treated as controller folders, and `views` as the views folder. Alternate folder names can be specifed in the user package settings file.

### CommandBox

CommandBox (https://www.ortussolutions.com/products/commandbox) has been added as the default CFML build system. This simply means that running the build command (<kbd>F7</kbd> on Windows) on a CFML file will run `box ${filename}` as a shell command in the directory of the file and output the result in a pane within Sublime Text (it can also be selected from the build system menu available via <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>B</kbd>). For this to work, CommandBox needs to be installed, and the CommandBox `box` binary needs to be available system wide, so that `box` can be run in any directory (see https://ortus.gitbooks.io/commandbox-documentation/content/setup/installation.html).

### TestBox

TestBox (https://www.ortussolutions.com/products/testbox) completions and inline documentation are available for `BaseSpec` components. They are enabled by default, but can be disabled globally by adding `"testbox_enabled": false` to your CFML user package settings, or on a per project basis by adding the same setting to a project settings file. The completions and documentation are offered in any CFC that is contained under a folder named `tests` (alternate folders can be specified in the settings), as well as in any CFC that extends `testbox.system.BaseSpec`.

There are three build system variants for running TestBox tests from within Sublime Text. The first is for running all of a project's tests, which can be used no matter what project files are open, so long as the active file is a CFML one. The next variant is for running all of the tests in the directory of the currently active file, while the last variant runs the tests in the currently active test CFC only. <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>B</kbd> calls up the build system menu, from which these options can be selected. Once one of the variants has been run, <kbd>CTRL</kbd>+<kbd>B</kbd> can used to run the last selected option again, without having to go through the menu.

Several settings need to be set in a project in order for these build systems to function. The first setting is `testbox_runner_url`, which should be the URL to a TestBox runner which runs tests and returns results in JSON. This needs to be setup so that a dot delimited directory of tests to run can be appended to it. An example URL that does this might be the following: `http://localhost:8888/testbox/system/Testbox.cfc?method=runremote&reporter=json&directory=`. The second setting is `testbox_default_directory`, which is the dot delimited directory that will be appended to the `testbox_runner_url` when the build variant for running all of a project's tests is selected. The last setting is `testbox_tests_root`, which is the root directory path that is used to determine the dot delimited path to a test directory when running the tests in a particular folder or file. (This could be the path to your project's webroot if your tests are contained under the webroot, but might not be if mappings are used.)

Results are displayed in a results pane. If there are errors or failures you can step backwards and forwards through the file stack traces via <kbd>F4</kbd> and <kbd>SHIFT</kbd>+<kbd>F4</kbd>. As each file is selected, Sublime Text opens that file and jumps to the line indicated. You can also double click on any file, and Sublime Text will open it for you.  Since the path to your files might look different to Sublime Text and the CFML application server (for example, if the server is running on a virtual machine), there is one more setting that maps the path as it appears to the server to the path as it appears to Sublime Text. Use `testbox_results.server_root` and `testbox_results.sublime_root` to specify the respective root paths to your project, so that Sublime Text can accurately open the files in stack traces.

All of the settings for TestBox can be seen in the default package settings.

### Framework One

Framework One (https://github.com/framework-one/fw1) function completions and `variables.framework` setting completions are available. They are disabled by default, but can be enabled globally by adding `"fw1_enabled": true` to your CFML user package settings, or on a per project basis by adding the same setting to a project settings file. (Project based settings will override global settings. The default package settings for Framework One can be viewed in the CFML default package settings file.) The completions are offered in `Application.cfc` as well as in Framework One controller, view and layout files. (The folder names can be specified in the settings.) In controllers, Framework One method completions are offered after typing `framework.` and `fw.`.

### Manual Installation

Locate your Sublime Text 3 packages directory. This can be easily done by opening the command palette in Sublime Text (<kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> on Windows, <kbd>CMD</kbd>+<kbd>SHIFT</kbd>+<kbd>P</kbd> on a Mac), and running `Preferences: Browse Packages`.

On Windows it will typically be something like this:
`C:\Users\Username\AppData\Roaming\Sublime Text 3\Packages\`

On a Mac it will be something like this:
`/Users/Username/Library/Application Support/Sublime Text 3/Packages/`

#### Via Git

Open Terminal or Command Prompt and cd into your packages directory, then run:

    git clone https://github.com/jcberquist/sublimetext-cfml.git ./CFML

The specified `./CFML` subdirectory is optional and if it is not included the package will be cloned into `sublimetext-cfml`.

That's it, restart Sublime Text 3 - if you want to update the code to match this repo, simply run the following inside the folder where you installed the package:

    git pull origin master

#### Via ZIP Download

Use the `Download ZIP` option to download a zip of the repository to your computer. Unzip this, and copy the repository folder into your Sublime Text packages directory. You can leave the folder name as is, or rename it (e.g. to `CFML`).
