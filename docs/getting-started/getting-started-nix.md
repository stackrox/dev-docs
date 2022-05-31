# Getting started with Nix

`Nix` is both a package manager and a programming language, in which derivations for
said package manager are written. While `Nix` was originally developed for `NixOS`, it
supports any Linux distribution as well as macOS. The goal of the `Nix` project is
to provide declarative and reproducible build environments.

[Stackrox-env](https://github.com/stackrox/stackrox-env) provides a development environment,
which includes all necessary tools to contribute to the [Stackrox project](https://github.com/stackrox/stackrox) regardless
of whether Linux or macOS is the underlying operating system. This approach represents an
alternative to the imperative approaches outlined in the [Linux](getting-started-linux.md) and [macOS](getting-started-darwin.md) guides.
The `Nix` environment provides the following advantages over the conventional setup:
* `infrastructure-as-code`: Changes to the environment are tracked by git and must pass a continuous integration pipeline.
* A `git pull` is sufficient to keep the environment in sync.
* The `Nix store` hermetically stores binaries, libraries and all dependencies. No pollution on a system level.
* Rollbacks become trivial by reverting to an older git revision.

## Install Nix

Follow the [instructions](https://nixos.org/download.html#download-nix) to install `Nix` on your operating system.

## Download Stackrox Nix environment

Follow the [instructions](https://github.com/stackrox/stackrox-env) to download the
Stackrox Nix environment. You may either choose to set the environment globally in your system
by [sourcing the environment in your login shell](https://github.com/stackrox/stackrox-env#login-shell), or
source the environment locally using the [Direnv integration](https://github.com/stackrox/stackrox-env#direnv-integration).

## Install workflow scripts (optional)

1. Clone the workflow repository:
    ```bash
    # If you choose to follow the opinionated golang installation of putting everything into a "go" folder:
    $ mkdir -p ~/go/src/github.com/stackrox/
    $ cd ~/go/src/github.com/stackrox/
    $ git clone git@github.com:stackrox/workflow.git
    ```
2. Follow the steps in the [repo's README](https://github.com/stackrox/workflow/blob/master/README.md)

### Verify
1. Run roxhelp in the terminal to verify correct installation.
```
    $ roxhelp

    Usage:
    /Users/ross/go/src/github.com/stackrox/workflow/bin/roxhelp <command_name> (to print help for command <command_name>)
    /Users/ross/go/src/github.com/stackrox/workflow/bin/roxhelp --list-all (to list all available Rox commands)
```
