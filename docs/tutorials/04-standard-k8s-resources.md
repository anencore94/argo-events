# Trigger Standard K8s Resources

In the previous sections, you saw how to trigger the Argo workflows. In this
tutorial, you will see how to trigger Pod and Deployment.

**Note:** You can trigger any standard Kubernetes object.

Having the ability to trigger standard Kubernetes resources is quite powerful as
provides an avenue to set up pipelines for existing workloads.

## Prerequisites

1.  Make sure that the service account used by the Sensor has necessary
    permissions to create the Kubernetes resource of your choice. We use
    `k8s-resource-sa` for below examples, it should be bound to a Role like
    following.

        apiVersion: rbac.authorization.k8s.io/v1
        kind: Role
        metadata:
          name: create-deploy-pod-role
        rules:
          - apiGroups:
              - ""
            resources:
              - pods
            verbs:
              - create
          - apiGroups:
              - apps
            resources:
              - deployments
            verbs:
              - create

2.  The `Webhook` event-source is already set up.

## Pod

1.  Create a sensor with K8s trigger.

         kubectl -n argo-events apply -f https://raw.githubusercontent.com/argoproj/argo-events/stable/examples/tutorials/04-standard-k8s-resources/sensor-pod.yaml

2.  Use either Curl or Postman to send a post request to the
    `http://localhost:12000/example`.

        curl -d '{"message":"ok"}' -H "Content-Type: application/json" -X POST http://localhost:12000/example

3.  Now, you should see a pod being created.

        kubectl -n argo-events get po

Output

        _________________________________________
        / {"context":{"type":"webhook","specVersi \
        | on":"0.3","source":"webhook","e |
        | ventID":"30306463666539362d346666642d34 |
        | 3336332d383861312d336538363333613564313 |
        | 932","time":"2020-01-11T21:23:07.682961 |
        | Z","dataContentType":"application/json" |
        | ,"subject":"example"},"data":"eyJoZWFkZ |
        | XIiOnsiQWNjZXB0IjpbIiovKiJdLCJDb250ZW50 |
        | LUxlbmd0aCI6WyIxOSJdLCJDb250ZW50LVR5cGU |
        | iOlsiYXBwbGljYXRpb24vanNvbiJdLCJVc2VyLU |
        | FnZW50IjpbImN1cmwvNy41NC4wIl19LCJib2R5I |
        \ jp7Im1lc3NhZ2UiOiJoZXkhISJ9fQ=="}       /
        -----------------------------------------
          \
           \
            \
                          ##        .
                    ## ## ##       ==
                 ## ## ## ##      ===
             /""""""""""""""""___/ ===
        ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
             \______ o          __/
              \    \        __/
                \____\______/

## Deployment

1.  Lets create a sensor with a K8s deployment as trigger.

        kubectl -n argo-events apply -f https://raw.githubusercontent.com/argoproj/argo-events/stable/examples/tutorials/04-standard-k8s-resources/sensor-deployment.yaml

2.  Use either Curl or Postman to send a post request to the
    `http://localhost:12000/example`.

        curl -d '{"message":"ok"}' -H "Content-Type: application/json" -X POST http://localhost:12000/example

3.  Now, you should see a deployment being created. Get the corresponding pod.

        kubectl -n argo-events get deployments

Output

        _________________________________________
        / {"context":{"type":"webhook","specVersi \
        | on":"0.3","source":"webhook","e |
        | ventID":"30306463666539362d346666642d34 |
        | 3336332d383861312d336538363333613564313 |
        | 932","time":"2020-01-11T21:23:07.682961 |
        | Z","dataContentType":"application/json" |
        | ,"subject":"example"},"data":"eyJoZWFkZ |
        | XIiOnsiQWNjZXB0IjpbIiovKiJdLCJDb250ZW50 |
        | LUxlbmd0aCI6WyIxOSJdLCJDb250ZW50LVR5cGU |
        | iOlsiYXBwbGljYXRpb24vanNvbiJdLCJVc2VyLU |
        | FnZW50IjpbImN1cmwvNy41NC4wIl19LCJib2R5I |
        \ jp7Im1lc3NhZ2UiOiJoZXkhISJ9fQ=="}       /
        -----------------------------------------
          \
           \
            \
                          ##        .
                    ## ## ##       ==
                 ## ## ## ##      ===
             /""""""""""""""""___/ ===
        ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
             \______ o          __/
              \    \        __/
                \____\______/
