# Getting Started on Darwin


## Change default shell to bash(Optional)
The latest Macbook comes with zsh as default shell. You may change it to bash if you would like to.

1. Launch the Terminal app found in /Applications/Utilities. 
2. Change default shell to bash  
    ```
    % chsh -s /bin/bash
    ```
3. You’ll be prompt to enter your password. Then, close the terminal and reopen it. The terminal will come back with bash prompt "$".


## Install XCode
1. Open the App Store and install XCode.


## Install the XCode Command Line Tools
1. Launch the Terminal app found in /Applications/Utilities. 
2. Run the command to install the XCode Command Line Tools like this:  
    ```
    $ xcode-select --install
    ```

### Verify
1. Run this command:  
    ```
    $ gcc
    ```
2. Verify you see output like the following:  
    ```
    clang: error: no input files
    ```


## Install Docker Desktop for Mac
1. Download Docker Desktop: Browse to [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac/) and click on [Get Docker for Mac (stable)](https://desktop.docker.com/mac/stable/Docker.dmg).
2. Install Docker for Mac by double-clicking on the installer (Docker.dmg) when it's done downloading.
3. Launch the Docker for Mac found in /Applications.

### Verify
1. Launch the Terminal app found in /Applications/Utilities.
2. Run this command:  
    ```
    $ docker run --rm hello-world
    ```
3. Verify you see output like the following:  
    ```
    (...)
    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    (...)
    ```


## Install Homebrew

1. Launch the Terminal app found in /Applications/Utilities. 
2. Run the command to install Homebrew, like this:  
    ```
    $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
    ```

### Verify
1. Run this command from within the terminal you just opened:  
    ```
    $ brew info
    ```
2. Verify you see output like the following:  
    ```
    16 kegs, 22,408 files, 336.6M
    ```


## Install Productivity Tools and Bash-completion

1. Launch the Terminal app found in /Applications/Utilities. 
2. Run the commands to install productivity tools like this:  
    ```
    $ brew install jq bash-completion wget
    ```
3. Add bash completion to your `~/.bash_profile`: (if you're setting up a new computer, it's likely it won't exist, so create a new file)
    ```bash
    if [ -f $(brew --prefix)/etc/bash_completion ]; then
      . $(brew --prefix)/etc/bash_completion
    fi
    ```

## Install Go

1. Download Go for MacOS via Homebrew. (Please see [here](https://github.com/stackrox/rox/blob/master/EXPECTED_GO_VERSION) for the latest version used at StackRox that you should download and [here](https://formulae.brew.sh/formula/go) for the possible Homebrew formulae).
    # This installs the latest version of Go
    brew install go
     
    # If the version of Go used in the rox repo is not the latest, install the prior version like this, for example:
    brew install go@1.16

### Verify

1. Run the command to inspect the version of Go that was just installed. It should be what you expect:
    ```
    $ go version
    
    go version go1.xx darwin/amd64. # This should reflect the version you just installed
    ```


## Install RocksDB
    ```
    $ brew install rocksdb
    ```

## Install Helm
    ```
    $ brew install helm
    ```

### Verify

1. Run the command to inspect the version of Helm that was just installed
    ```
    $ helm version
    
    version.BuildInfo{Version:"v3.4.2", (...)}
    ```


## Install Workflow Scripts

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
