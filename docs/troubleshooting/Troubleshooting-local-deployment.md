# Troubleshooting local deployment of StackRox
If you run the StackRox software locally, either with the front-end script `yarn deploy-local` or `deploy/k8s/deploy-local.sh`, then you may run into a situation where it fails to fully deploy.

## Debugging Tips
If your local deployment run ends in an error like the following,
```
Waiting for Central to respond...Exceeded deadline waiting for Central.
Error from server (BadRequest): pod central-7c84c44b6-h6rq4 does not have a host assigned
error Command failed with exit code 1.
```
check to see whether you have a "zombie" pod still running.
```
kubectl -n stackrox get pods
```
 

If there is an old central with a status of Terminating, kill it with: 

```
kubectl -n stackrox delete po --force --grace-period=0 <pod_name>
```
where `<pod_name>` is the name of the “zombie” pod.


**Note**: to redeploy a newer version of StackRox, currently the easiest way is by deleting the whole stackrox namespace via `kubectl delete ns stackrox`, and repeating the steps under [Using Local StackRox Deployment and Docker for Mac](https://github.com/stackrox/rox/tree/master/ui#using-local-stackrox-deployment-and-docker-for-mac) in the front-end README.
