gister
======

`gister` is a command line tool for managing GitHub gists.

Based on [gist.rb][gist] by [@defunkt][defunkt], this tool helps you to manage a local copy of your gists.

After publishing files to gist.github.com, this tool will:

- automatically clone the gist repository to local
- index the content of your gist for code search
- fetch meta info (e.g. description, url) of the gist from GitHub and add them to `gists.list` 

[gist]: https://github.com/defunkt/gist
[defunkt]: https://github.com/defunkt

You can also use `gisetr` to sync your gists between gist.github.com and your machine.


Dependencies
------------


- curl
- git
- [gist.rb][gist]
- [csearch](https://code.google.com/p/codesearch/)
- [jq](http://stedolan.github.io/jq/)

For Linux, BSD, etc, you also need `xclip` or `xsel`.
For Cygwin, you need putclip/getclip provided by cygutils-extra.
(Mas OS X users should be fine with the preinstalled pbcopy/pbpaste.)

Note: `xsel` users should use `gist.rb` v4.1.2+, since there is [a bug bitting xsel users in previous versions][151].

[151]: https://github.com/defunkt/gist/pull/151


Install
-------

### fetch the source

    git clone https://github.com/weakish/gister.git
    cd gister
    sudo rake install

If you want to install `gister` to a place other than `/usr`, you can use `sudo rake install[prefix]` instead, e.g. `sudo rake install[/usr/local]`.

Arch users can install via [AUR](https://aur.archlinux.org/packages/gister/), thanks to [weiLiangcan](https://github.com/wenLiangcan). 


Usage
-----

### init

For the first time, you need to run `gister init` to associate your GitHub account and configure the directory to store local copies of your gists.

After that, you may run `gister sync` to fetch all your gists to local.

Warn: `sync` can only fetch up to 10 million gists for you. If you have more than 10 million gists, you need to modify the source of `gister` yourself.


### publish

Whenever you want to publish a gist, just use

    gister description file.txt ...

This will create the gist with the provided description, clone the gist repo, put the gistid to clipborad, and open the url in your `x-www-browser`.

Note: you must provide gist description, otherwise `gister` will fail.

Hint: `gister` will pass all arguments to gist as `gist -c -o -d description ...`, so you can use other options that gist understands, e.g. `gister descrption -P` will work.

If you've edited your gists at `gist.github.com` or local machine, without pull/push changesets, you can sync all your gists via `gister sync`.

If you've deleted your gists at `gist.github.com`, after `gister sync`, the directories of deleted gists at your local machine will be marked with a prefix `_`.

### search

Search all of your gists:

    gister search regexp


### migrate

From version 1.0.0, `gister` uses a different storage structure.
If you have used `gister <1.0.0`, then you need to run this command to migrate:

    gister migrate


Storage
-------

    /path/to/your/gists
    |-- gists.list  # a list of all your gists (including meta info) 
    |-- repo # git repositories of your gists
    |-- tree # working directory of your gist repositories
        |-- 123456 # an example of gist
        |-- _123567890 # an example of gist which you have deleted on gist.github.com
        |-- ...
    `-- .csearchindex # code search index


Contributing
------------

Fork and/or send pull requests or issues on github: https://github.com/weakish/gister
