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
1. Download Docker Desktop: Browse to [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac/) and click on "Mac with Intel chip" if you have an Intel Mac or "Mac with Apple chip" if you have an M1 laptop.
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
    (...)
    ```
*Note: Make sure you assign enough memory to the docker engine. Otherwise you may have problem compiling the project or launching a local deployment. 8 GB should be ok at the time of writing (10/2021).*

### Kubernetes

This is only required if you want to run StackRox locally. You may also use [crc](https://developers.redhat.com/products/codeready-containers/overview) (Code Ready Containers) to run OpenShift locally if you have an Intel Mac (crc is not supported for M1 as of 11/2021).

1. Click on the Docker icon in the menu bar and select Preferences
2. In the Kubernetes tab, select "Enable Kubernetes" and press "Apply & Restart"
3. Once it's done restarting and setting up, run (should show all system pods):  
    ```
    $ kubectl get pods -A
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


### Note for M1 users

In Intel Macs homebrew may install bins, header files and libs to `/usr/local`. However the [default install](https://docs.brew.sh/Installation) for Homebrew on M1 is `/opt/homebrew`. You can verify by running `brew --prefix` in the terminal. 

Homebrew will automatically add (or suggest you add) `/opt/homebrew/bin` and `/opt/homebrew/sbin` to your PATH so apps installed via brew can be found easily. However, if you ever need to compile or link with headers or libs provided by something installed via a brew formulae, then you will need to set your CFLAGS, LDFLAGS and other such flags manually. It seems that current compilers and linkers don't look in that path. Includes can be found in `$(brew --prefix)/include` and libs in `$(brew --prefix)/lib`. You may also need to create a symlink to libs in `/usr/local/lib` for certain dynamic libs (e.g. rocksdb - see below). 

If you find a better way of dealing with this, please update this section.

## Install Productivity Tools and Bash-completion

1. Launch the Terminal app found in /Applications/Utilities. 
2. Run the commands to install productivity tools like this:  
    ```
    $ brew install jq bash-completion wget coreutils
    ```
3. Add bash completion to your `~/.bash_profile`: (if you're setting up a new computer, it's likely it won't exist, so create a new file)
    ```bash
    if [ -f $(brew --prefix)/etc/bash_completion ]; then
      . $(brew --prefix)/etc/bash_completion
    fi
    ```

## Install Go

1. Download Go for MacOS via Homebrew. (Please see [here](https://github.com/stackrox/stackrox/blob/master/EXPECTED_GO_VERSION) for the latest version used at StackRox that you should download and [here](https://formulae.brew.sh/formula/go) for the possible Homebrew formulae).
    ```
    # This installs the latest version of Go
    brew install go
     
    # If the version of Go used in the rox repo is not the latest, install the prior version like this, for example:
    brew install go@1.16
    ```

### Verify

Run the command to inspect the version of Go that was just installed. It should be what you expect:
    ```
    $ go version
    
    go version go1.xx darwin/amd64. # This should reflect the version you just installed
    ```

## Java related considerations

One of the places where the codebase relies on Java is the end-to-end tests (qa-tests-backend). Technically, the tests are written in Groovy, but require a java VM. The groovy daemon (gradle) is not always compatible with the latest delivered Java version, so you may want to install an older jdk and reference it in your environment
```
$ brew install openjdk@11
 
# In the shell configuration file (.zshrc , /etc/profile , ...)
export JAVA_HOME=/usr/local/Cellar/openjdk@11/11.0.12
```

## Install RocksDB
Currently we require v6.15.5 of rocksdb. The tl;dr is that the cgo library we use hasn't been updated to account for backwards incompatible changes made to the C++ library. All of the steps below are to compile and install the 6.15.5 version of rocksdb.

### Intel-Based Mac
```
# Clone the RocksDB repo somewhere on your machine. For example in ~/go/src/github.com/facebook
git clone https://github.com/facebook/rocksdb.git
cd rocksdb
git checkout v6.15.5

# Now install the required dependencies
brew install snappy
brew install lz4
brew install zstd

make shared_lib
make install-shared
```

### M1 Macs

First clone and get the RocksDB repo somewhere on your machine and get the v6.15.5 branch. For example in `~/go/src/github.com/facebook`:
```
# Clone the RocksDB repo somewhere on your machine. For example in ~/go/src/github.com/facebook
git clone https://github.com/facebook/rocksdb.git
cd rocksdb
git checkout v6.15.5

# Now install the required dependencies
brew install snappy
brew install lz4
brew install zstd

# as described in the homebrew section, symlink dylib over to /usr/local/lib
sudo ln -sfn $(brew --prefix)/lib/libsnappy.dylib /usr/local/lib
sudo ln -sfn $(brew --prefix)/lib/liblz4.dylib /usr/local/lib
sudo ln -sfn $(brew --prefix)/lib/libzstd.dylib /usr/local/lib
```

Compile and install the rocksdb library
```
# Apply patch to be able to build on arm64
curl -L https://github.com/facebook/rocksdb/pull/7714.patch | git apply -
 
CFLAGS="-I$(brew --prefix)/include" LDFLAGS="-L$(brew --prefix)/lib" make shared_lib

# if the above doesn't work you may have to allow a few warnings by
# CXXFLAGS="-Wno-error=deprecated-copy -Wno-error=unused-but-set-variable"

make install-shared
# if the above doesn't work run install-shared with sudo
```



### Verify
If this worked (Intel or M1) properly, you'll see something like the following:
```
$ ls -l /usr/local/lib/ | grep rocksdb
-rwxr-xr-x    1 ross  admin  8700976 Apr 21 22:51 librocksdb.6.15.5.dylib
lrwxr-xr-x    1 ross  admin       23 Apr 21 22:51 librocksdb.6.15.dylib -> librocksdb.6.15.5.dylib
lrwxr-xr-x    1 ross  admin       23 Apr 21 22:51 librocksdb.6.dylib -> librocksdb.6.15.5.dylib
lrwxr-xr-x    1 ross  admin       23 Apr 21 22:51 librocksdb.dylib -> librocksdb.6.15.5.dylib
```

### (Optional but recommended)

After you have cloned and setup the StackRox repo, try running a test that depends on rocksdb:  
```
cd $GOPATH/src/github.com/stackrox/stackrox
# runs an arbitrary test that depends on rocksdb. -count=1 forces it to not use cached results
go test github.com/stackrox/rox/central/vulnerabilityrequest/manager -count=1
 
# should get result that says "ok".
```

### Homebrew (may not work)

Homebrew-Core does not track the version of RocksDB we use. Because of this, we maintain our own tap of RocksDB. This tap will be installed at `/usr/local/Homebrew/Library/Taps/stackrox/homebrew-rocksdb`. This is the Git repository for the tap.
```
$ brew tap stackrox/rocksdb
$ brew install stackrox/rocksdb/rocksdb@6.15.5
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
