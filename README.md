# Homebrew Command Not Found

This project reproduces Ubuntu’s `command-not-found` for Homebrew users on
macOS.

[![GitHub Actions](https://github.com/Homebrew/homebrew-command-not-found/workflows/GitHub%20Actions%20CI/badge.svg)](https://github.com/Homebrew/homebrew-command-not-found/actions)

On Ubuntu, when you try to use a command that doesn’t exist locally but is
available through a package, Bash will suggest you a command to install it.
Using this script, you can replicate this feature on macOS:

```bash
# on Ubuntu
$ when
The program 'when' is currently not installed.  You can install it by typing:
sudo apt-get install when

# on macOS with Homebrew
$ when
The program 'when' is currently not installed. You can install it by typing:
  brew install when
```

Over 5000 formulae are supported, representing more than 17000 different commands
(100% of the main Homebrew repo).

## Install

First, tap this repository:

```bash
brew tap homebrew/command-not-found
```

* **Bash and Zsh**: Add the following line to your `~/.bash_profile` (bash) or `~/.zshrc` (zsh):

    ```bash
    HB_CNF_HANDLER="$(brew --prefix)/Homebrew/Library/Taps/homebrew/homebrew-command-not-found/handler.sh"
    if [ -f "$HB_CNF_HANDLER" ]; then
	source "$HB_CNF_HANDLER";
    fi
    ```

* **Fish**: Add the following line to your `~/.config/fish/config.fish`:

    ```fish
    set HB_CNF_HANDLER (brew --prefix)"/Homebrew/Library/Taps/homebrew/homebrew-command-not-found/handler.fish"
    if test -f $HB_CNF_HANDLER
       source $HB_CNF_HANDLER
    end
    ```

## Requirements

This tool requires one of the following:

* [Zsh](https://www.zsh.org) (the default on macOS Catalina and above)
* [Bash](https://www.gnu.org/software/bash/) (version 4 and higher)
* [Fish](https://fishshell.com)

## How it works

When you tap the repo you’ll get two more `brew` commands: `brew which-formula`
and `brew which-update`. The first one uses a database file which gives you the
formula you have to install in order to get a specific command. The file is
generated by the second command by crawling all installed formulae and
collecting their binaries. Having this as a tap means you get an up-to-date
binaries database each time you run `brew update`.

The `handler.sh` script defines a `command_not_found_handle` function which is
used by Bash when you try a command that doesn’t exist. The function calls
`brew which-formula` on your command, and if it finds a match it’ll print it to
you. If not, you’ll get an error as expected.
