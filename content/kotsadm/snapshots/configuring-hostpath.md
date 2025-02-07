---
date: 2021-03-03
linktitle: "Configuring a Host Path"
weight: 11
title: Configuring a Host Path
---

> Introduced in KOTS v1.33.0

The steps described on this page are only necessary if you wish to configure a Host Path destination as your KOTS Snapshots [storage destination](/kotsadm/snapshots/storage-destinations/). 

**Important note:** Make sure that the host path exists and is writable by the user:group 1001:1001 on all the nodes in the cluster.

## Embedded Clusters

Embedded clusters set up using installers that include the [Velero](https://kurl.sh/docs/add-ons/velero) addon are configured by default to store snapshots internally in the cluster.
There are two ways to change this configuration to use a Host Path:

* Using the KOTS CLI [velero configure-hostpath](/kots-cli/velero/configure-hostpath/) command.
* Using the Admin Console (Check screenshots below):

First, head to the "Snapshots" tab.
From there, head to the "Settings and Schedule" tab and choose the "Host Path" dropdown option.

![Snapshot Destination Dropdown Host Path](/images/snapshot-destination-dropdown-hostpath.png)

Enter the path to the directory on the node and click "Update storage settings".
This step might take a couple of minutes so please be patient.

![Snapshot Destination Host Path Fields](/images/snapshot-destination-hostpath-field.png)

## Existing Clusters

**Note:** If Velero is already installed in the cluster, you can follow the same instructions mentioned in the [Embedded Clusters](/kotsadm/snapshots/configuring-hostpath/#embedded-clusters) section.

If Velero is not yet installed in the cluster, then the first step would be to set up and deploy the necessary components that are going to be used to install and set up Velero with the provided host path.
This can be done in two ways:

### Using the KOTS CLI

The [velero configure-hostpath](/kots-cli/velero/configure-hostpath/) CLI command can be used to configure a host path for either online or airgapped installations.
After this command has run and completed successfully, it will detect if Velero is not installed and print out specific instructions on how to install and set up Velero.

**Online Installations**

```bash
kubectl kots velero configure-hostpath --hostpath /path/to/directory --namespace <namespace>
```

**Airgapped Installations**

```bash
kubectl kots velero configure-hostpath \
  --hostpath /path/to/directory \
  --namespace <namespace> \
  --kotsadm-registry private.registry.host \
  --kotsadm-namespace application-name \
  --registry-username ro-username \
  --registry-password ro-password
```

### Using the Admin Console

First, head to the “Snapshots” tab.
From there, head to the “Settings and Schedule” tab.
Then, you'll be presented with a dialog which contains instructions for setting up Velero with different providers.
Click on the "Host Path" provider option (check screenshot below).

![Snapshot Provider Host Path](/images/snapshot-provider-hostpath.png)

Then, you'll be presented with another dialog for configuring the host path.
Enter the path to the directory on the node and click "Configure" (check screenshot below).

![Snapshot Provider Host Path Fields](/images/snapshot-provider-hostpath-field.png)

This step might take a few minutes, so please be patient.
Once the configuration is successful, you'll be presented with a different dialog which contains a CLI command that will print out instructions on how to set up Velero with the deployed host path configuration/components (check screenshot below).

![Snapshot Provider File System Next Steps](/images/snapshot-provider-fs-next-steps.png)

After following the instructions from the above CLI command, and Velero has been installed successfully, you can go back to the Admin Console and either click on the "Check for Velero" button to retry detecting Velero, or simply refresh the page.
