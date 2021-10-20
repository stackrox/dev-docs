# Use remote Central for local front-end dev

Sometimes, you need to develop the front-end StackRox app against a
central Central deployment. This documents covers some common ways that
you may do that.

## Instructions

### Creating a remote StackRox Deployment

1.  **Provision back end cluster** - Provision a cluster of your liking, e.g.
    OpenShift, GKE, etc.

2.  **Connect local machine to cluster** - Your local machine needs to
    be made aware of the cloud cluster you just created.
    Run `yarn connect [cluster-name]` where `[cluster-name]` is the name
    of the cluster you created in the previous step.

3.  **Deploy StackRox** - Deploy a fresh copy of the StackRox software
    to your new cluster. During the deployment process, you may be asked
    for your DockerHub credentials.

    -   Set up a load balancer by setting the env. variable by
        running `export LOAD_BALANCER=lb` (optional, if you want to add
        multiple clusters or just avoid port forwarding)

    -   Set up persistent storage by setting the env. variable by
        running `export STORAGE=pvc` (optional, but if you need to
        bounce central during testing, then your changes will be saved)

    -   Run `yarn deploy`

4.  **Run local server** - Start your local server.

    -   Ensure port forwarding is working by running `yarn forward` (The
        deploy script tries to do this, but it is flakey.)

    -   In another terminal, start the front-end by
        running `yarn start`.

    -   This will open your web browser
        to <a href="https://localhost:3000/" class="external-link">https://localhost:3000</a>

*If your machine goes into sleep mode, you may lose the port forwarding
set up during the deploy step. If this happens, run `yarn forward` to
restart port forwarding.*

### Using an Existing StackRox Deployment with a Local Frontend

If you want to connect your local frontend app to a StackRox deployment
that is already running, you can use one of the following, depending on
whether it has a public IP. 

-   If it has a public IP, export that in the `YARN_START_TARGET` env var and start 
    the front-end by running `export YARN_START_TARGET=<external_IP>; yarn start`

-   If it does not have a public IP or a demo URL, you can use steps 2
    and 4 from the section above, **Using Remote StackRox Deployment**.

    1.  Run `yarn connect [cluster-name]` where `[cluster-name]` is the
        name found in the existing cluster’s details in Infra.

    2.  Run `yarn forward` in one terminal, and `yarn start` in another.
