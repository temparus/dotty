## Dotty is a script for syncing and managing versions of your dotfiles.

### Usage
[![asciicast](https://asciinema.org/a/200410.png)](https://asciinema.org/a/200410)

### Installation:
  Add dotty to your dotfiles' git repository:
  
    cd ~/chosen-dotfiles-folder; [[ -f .git ]] || git init
    git submodule add https://github.com/mvrozanti/dotty; git submodule update --remote dotty

You're done!
  
### Configuration
  Dotty uses a JSON-formatted config located on the dotty repository directory.
  Currently, dotty can create/check with `mkdirs`, `link` or `copy` files/directories, `install` packages and execute shell `commands`.

  It is also capable of automatically pushing your changes to your dotfiles to a repository server like GitHub.

  Most importantly, it can also restore the dotfiles to their respective locations on the target file system. That is, You can take your files *and* your configurations anywhere, while backing it up remotely if desired.


### Features
    - Link-following
    - Wildcards in `copy` paths

### Arguments: 
  
    usage: dotty.py [-h] [--config *dotty*.json] [-f] [-b] [-c] [-r] [-d] [-s] [-e LOCATION]
    optional arguments:
      -h, --help            show this help message and exit
      --config *dotty*.json
                            the JSON file you want to use, it's only required if
                            filename doesn't end in json or doesn't contain dotty
                            in the basename
      -f, --force           [1mdo not prompt user[0m: replace files/folders if
                            they already exist, removing previous directory tree
      -b, --backup          run copy in reverse so that files and directories are
                            backed up to the directory the config file is in
      -c, --clear           clears the config directory before anything, removing
                            all files listed in it
      -r, --restore         restore all elements to system (mkdirs, link, copy,
                            install(install_cmd), commands)
      -d, --dryrun          perform a dry run, outputting what changes would have
                            been made if this argument was removed [TODO]
      -s, --sync            perform action --backup, commits changes and pushes to
                            the dotfiles remote repository (must already be set
                            up) and then --clear
      -e LOCATION, --eject LOCATION
                            run --clear and move config folder to another location
                            (thank hoberto)
### To be implemented:
 Implement dryrun command.

 Check if any file listed in config are missing and warn user before trying to operate on them.

 Implement mutually exclusive arguments.

### Sample configuration:

    {
        "mkdirs": ["~/.vim"],
        
        "link": {
            "<file in dotfiles folder>": "<file in filesystem>",
            "zshrc": "~/.zshrc",
            "emacs/lisp/": "~/.emacs.d/lisp",
            "/mnt/1ADE1465DE134C17/or/another/external/drive": "~/external-drive"
        },

        "copy": {
            "<file in dotfiles folder>": "<file in filesystem>",
            ".vimrc": "~/.vimrc",
            ".config/": "~/.config/*conf"
        },

        "install_cmd": "apt-get install",
        "install": [
            "zsh",
            "firefox",
            "vim"
        ],
            
        "commands": [
            "git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim && vim +PluginInstall +qall"
        ]
    }
