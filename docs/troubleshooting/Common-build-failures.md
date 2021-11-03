# Common build failures (and how to fix them)

This page contains some common issues that you might encounter during building the StackRox platform, and how to fix them. When you encounter an issue and after fixing it (either alone or with help from your colleagues), please add it to this page.


## Error fetching dependencies from private repositories
### Symptom
When running make deps or other dependency-related commands like go get, an error similar to the following appears:
```
go: github.com/stackrox/docker-registry-client@v0.0.0-20181115184320-3d98b2b79d1b: git fetch -f origin refs/heads/*:refs/heads/* refs/tags/*:refs/tags/* in /Users/viswa/development/pkg/mod/cache/vcs/224709ea57b0669f3ef68063a82d06a64a371fd0134dc32cb4ad5afcd7a475af: exit status 128:
    fatal: could not read Username for 'https://github.com': terminal prompts disabled
```

### Solution
Git must be instructed to always use SSH to access repositories on github.com. Adjust your global Git config by running the following command:
```
$ git config --global --add url.git@github.com:.insteadof https://github.com/
```


## Error checking if protobuf library is up-to-date
### Symptom
When running make install-dev-tools or similar an error such as this one appears:
```
~/go/src/github.com/stackrox/rox $ make install-dev-tools
+ install-dev-tools
+ Checking if github.com/gogo/protobuf is up-to-date
+ /Users/marcin/go/src/github.com/stackrox/rox/generated/test/test.pb.go
Missing value for flag: -I
```
### Solution
Reinstall dev tools:
```
make reinstall-dev-tools
```

## Error compiling generated protobuf code
### Symptom
When running make image or similar, an error appears:
```
Compiling Go source in ./roxctl to bin/darwin/roxctl
# github.com/stackrox/rox/generated/internalapi/central
generated/internalapi/central/hello.pb.go:26:6: HelmManagedConfigInit redeclared in this block
    previous declaration at generated/internalapi/central/cluster_config.pb.go:26:6
[...]
```

### Solution
Clean the generated source code first with:
```
make clean
```
