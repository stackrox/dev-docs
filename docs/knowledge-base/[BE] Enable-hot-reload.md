# Enable Hot Reload
When working on `sensor` or `central` locally, you can minimize the time needed for redeployments.

```bash
make image  # or use a CI built image

# This will persist the database across restarts
export STORAGE=pvc

# Next up, deploy StackRox, e.g. by invoking 
./deploy/k8s/deploy-local.sh

# then, enable hot reload for either central
./dev-tools/enable-hotreload.sh central
# ... or for sensor
./dev-tools/enable-hotreload.sh
```

This script will mount the compiled binary directly into the corresponding pod, technically enabling hot reloads.
Now you can make your changes to sensor or central and use `make fast-central` or `make fast-sensor` to build and reload either application.  
