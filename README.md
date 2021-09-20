# kubectl-scan

Simple Kubernetes image scan using trivy.

## Disclaimer

This work is at a very early stage. Use it with caution. Merge request are welcomed!

It relies heavily on trivy (https://github.com/aquasecurity/trivy) and we are using the image from bitnami
(https://bitnami.com/).

## Prerequisites

A working cluster and kubectl correctly configured.

## Installation

Retrieve the kubectl-scan script:

    wget https://raw.githubusercontent.com/AIOS-SH/kubectl-scan/main/kubectl-scan

Set execution on it:

    chmod +x kubectl-scan

Then put it somewhere in the user path (eg: /usr/local/bin or ~/bin)

    cp kubectl-scan ~/bin

Check the configuration:

    kubectl scan --help

This command will display the help:

    Usage: kubectl scan
          [-i|--init-containers] [--trivy-image my/image]
          [--pod-annotations annotations]
          <KUBECTL_OPTIONS> -- <TRIVY_OPTIONS>

    With:
    -i --init-containers  Scan init containers
    --trivy-image      Trivy image reference (default=bitnami/trivy:latest)
    --pod-annotations   Add this annotations (default=sidecar.istio.io/inject=false)
    
    You can use regular kubectl options to handle namespace or kubeconfig.
    To send options to trivy, put -- then set the needed parameters (default options:  --no-progress).

## Run it

To launch a simple scan with trivy against all pods on default namespace, launch the following command:

    kubectl scan

To scan kube-system, use the following command:

    kubectl scan -n kube-system

In fact, you can use any kubectl options.

To change the trivy options, use '--' sequence and paste options. Here an example to scan kube-system namespace and
fail only on CRITICAL security issue and ignoring unfixed errors:

    kubectl scan -n kube-system -- --severity CRITICAL --ignore-unfixed

## Known limitations

It's not possible to run the scan on the whole cluster. You can only run it on a given namespace.

The script support only one pullImageSecrets. If you are using multiple secrets, the script will fail to scan protected
images.
