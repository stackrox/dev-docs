# Connecting your UI to a large-scale cluster 

If you need to test UI changes against a large-scale environment (lots
of deployments, lots of violations, lots of images, lots of CVEs, etc.),
follow these steps.

## Terminal Tab 1

For all these commands, your CWD should be the `ui/` directory of the
Rox repo

1.  Create a cluster (e.g. on OpenShift or GKE) with an initial
    node count of 5.

2.  Connect to that remote cluster with `yarn connect <resource-name>`.

3.  Make sure you have exported the `MAIN_IMAGE_TAG` you want to deploy,
    or make sure your master branch is clean.

4.  `export STORAGE=pvc`

5.  `export STORAGE_CLASS=faster`

6.  `yarn deploy`

7.  `kubectl -n stackrox set env deploy/sensor MUTEX_WATCHDOG_TIMEOUT_SECS=0`

8.  `kubectl -n stackrox set env deploy/sensor ROX_FAKE_KUBERNETES_WORKLOAD=xlarge`

9.  `kubectl -n stackrox patch deploy/sensor -p '{"spec":{"template":{"spec":{"containers":[{"name":"sensor","resources":{"requests":{"memory":"3Gi","cpu":"2"},"limits":{"memory":"12Gi","cpu":"4"}}}]}}}}'`

10. `kubectl -n stackrox set env deploy/central MUTEX_WATCHDOG_TIMEOUT_SECS=0 ROX_ROCKSDB=true`

11. `kubectl -n stackrox patch deploy/central -p '{"spec":{"template":{"spec":{"containers":[{"name":"central","resources":{"requests":{"memory":"3Gi","cpu":"2"},"limits":{"memory":"12Gi","cpu":"4"}}}]}}}}'`

12. `killall kubectl`

13. `yarn forward`

## Terminal Tab 2

Now, in another terminal tab, also with `ui/` as the CWD, run

1.   `yarn start`

It will take a few minutes, but you should start to see the counts
slowly climb. Scaling is done when the deployments reach around 10,000.
