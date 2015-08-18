```copyright
Copyright © 2012-2014 Ejwa Software. All rights reserved.

This program comes with ABSOLUTELY NO WARRANTY.
This is free software, and you are welcome to redistribute it under
certain conditions; see the accompanying LICENSE.txt file for further details.

For questions regarding gitinspector you can contact the current maintainer
in charge at gitinspector@ejwa.se.

To run gitinspector; please start it via the gitinspector/gitinspector.py
script. Use the -h or --help flags to get help about available options.

It is also possible to set gitinspector options using the "git config"
command. Refer to the project page at http://gitinspector.googlecode.com
for more information.
```

---- docs copied ----


[Source](https://code.google.com/p/gitinspector/wiki/Documentation "Permalink to Documentation - gitinspector - Documentation covering gitinspector, it's options and how to use them. - The statistical analysis tool for git repositories.")

# Documentation - gitinspector - Documentation covering gitinspector, it's options and how to use them. - The statistical analysis tool for git repositories.

* Git executable defined somewhere in your PATH, otherwise gitinspector will not be able to run git.
* Python 2.6+.

The program can be executed using two different methods.

1. By _changing directory_ to a GIT repository and running the command

        gitinspector.py [OPTION]...

.
2. By running the command

        gitinspector.py [OPTION]... [GIT REPOSITORY]

The gitinspector script itself accepts a number of options that modifies it's behaviour and how statistics are generated. It is possible to get a brief explanation of all available options by supplying **-h** or **\--help** when running the program. Boolean arguments can only be given to long options.

The available options are:

| cmd| info | more info |
| ------------- | ----------- |----------- |
|  **-c, --checkout-missing[=BOOL]**  |  Deprecated since 0.3.2  |  By default, gitinspector will just print out missing files in a list and skip the inclusion of certain statistics from those files. With this option, gitinspector will try to checkout any missing files before computing statistics.  |
|  **-f, --file-types=EXTENSIONS**  |   |  A comma separated list of file extensions to include when computing statistics. The default file extensions used are: _java,c,cpp,h,hpp,py,glsl,rb,js,sql_.  |
|  **-F, --format=FORMAT**  |  New since 0.2.0  |  Defines in which format output should be generated; the default format is _'text'_ and the available formats are: _html,htmlembedded,text,xml_.
Please refer to the section #Supported_output_formats__(new_since_0.2.0) for more information about the output formats.|
|  **\--grading[=BOOL]**  |  New since 0.2.0  |  Show statistics and information in a way that is formatted for grading of student project; this is the same as supplying -HlmrTw.  |
|  **-H, --hard[=BOOL]**  |   |  Track rows and look for duplicates harder. In essence, this passes additional -C (and -M) options to the git executable.  |
|  **-l, --list-file-types[=BOOL]**  |   |  List all the file extensions available in the current branch of the repository. This can be very useful when you want to check what extensions are available in the repository. These can later be passed to gitinspector using the **-f** or **\--file-types** options.  |
|  **-L, --localize-output[=BOOL]**  |  New since 0.3.0  |  By default; the generated statistics are always in English. This flag localizes the generated output to the selected system language if a translation is available.  |
|  **-m, --metrics[=BOOL]**  |   |  Include checks for certain metrics during the analysis of commits. Currently very simple, but will be expanded.  |
|  **-r, --responsibilities[=BOOL]**  |   |  Show which files the different authors seem most responsible for.  |
|  **\--since=DATE**  |  New since 0.2.1  |  Show statistics for commits more recent than a specific date. The date format is in approxidate; the same as the format used in git itself.  |
|  **-T, --timeline[=BOOL]**  |   |  Show commit timeline, including author names. the timeline shows a statistical analysis where the amount of changes is calculated in relation to all other changes made by all other authors a specific month. Using the **-w** option, it is possible to get this output specified in weeks instead.  |
|  **\--until=DATE**  |  New since 0.2.1  |  Show statistics for commits older than a specific date. The date format is in approxidate; the same as the format used in git itself.  |
|  **\--tda367**  |  Deprecated since 0.2.0  |  Show statistics and information in a way that is formatted for the course TDA367/DIT211; this parameter equals -lmrTw.  |
|  **-w, --weeks[=BOOL]**  |   |  Show all statistical information in weeks instead of in months.  |
|  **-x, --exclude=PATTERN**  |   |  An exclusion pattern describing file paths, file names, authors or emails that should be excluded from the statistics. Works in the same manner as an inverted grep, meaning that any matched file names will be excluded from the generated statistics. Can be specified multiple times. Please refer to the section #Filtering for more information on how to fully utilize filtering.

New since 0.3.0: Support for regular expressions.  
New since 0.3.2: Support for filtering out commits from authors with a specific name or email.


| command | what it does |
| ------------- | ----------- |
|  **-h, --help**  |   Display this help and exit.  |
|  **\--version**  |   Output version information and exit.  |


There are support for multiple output formats in gitinspector. They can be selected using the -F/--format flags when running the main gitinspector script.

## Text (Plain Text)

Plain text with some very simple ANSI formatting, suitable for console output. This is the format chosen by default by gitinspector.

## HTML

HTML with external links. The generated HTML page links to some external resources; such as the JavaScript library JQuery. It requires an active internet connection to properly function. This output format will most likely also link to additional external resources in the future.

## HTMLEmbedded (new since 0.3.0)

HTML with no external links. Similar to the HTML output format, but requires no active internet connection. As a consequence; the generated pages are bigger (as certain scripts have to be embedded into the generated output).

## XML

XML suitable for machine consumption. If you want to parse the output generated by gitinspector in a script or application of your own; this is the format you should choose.

gitinspector offers several different ways of filtering out unwanted information from the generated statistics. As of version 0.3.2, the following filtering is supported:

```gitinspector -x myfile # filter out and exclude statistics from all files (or paths) with the string "myfile"```


```gitinspector -x file:myfile # filter out and exclude statistics from all files (or paths) with the string "myfile"```


```gitinspector -x author:John # filter out and exclude statistics from all authors containing the string "John"```


```gitinspector -x email:@gmail.com # filter out and exclude statistics from all authors with a gmail account```


## Mixing several filters together

gitinspector lets you add multiple filtering rules by simply specifying the -x options several times or by separating each filtering rule with a comma;

```gitinspector -x author:John -x email:@gmail.com```


```gitinspector -x author:John,email:@gmail.com```


## Using regular expressions

Sometimes, sub-string matching (as described above) is simply not enough. Therefore, gitinspector let's you specify regular expressions as filtering rules. This makes filtering much more flexible. Here are some examples:
     
    
```gitinspector -x "author:^(?!(John Smith))" # only show statistics from author "John Smith"```
    

```gitinspector -x "author:^(?!([A-C]))" # only show statistics from authors starting with the letters A/B/C```


```gitinspector -x "email:.com$" # filter out statistics from all email addresses ending with ".com"```


Options in gitinspector can be set using git config. Consequently, it is possible to configure gitinspector behavior globally (in all git repositories) or locally (in a specific git repository). It also means that settings will be permanently stored. All the long options that can be given to gitinspector can also be configure via git config (and take the same arguments).

To configure how gitinspector should behave in all git repositories, execute the following git command:

    git config --global inspector.**option** **setting**

To configure how gitinspector should behave in a specific git repository, execute the following git command (with the current directory standing inside the repository in question):

    git config inspector.**option** **setting**

If you want to run gitinspector under Windows, there are a few things to consider. Refer to the following page for more information:

<http: docs.python.org="" using="" windows.html="">  </http:>
