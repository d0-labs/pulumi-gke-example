# Pulumi GKE Sample

The purpose of this repo is to use Pulumi to create a GKE cluster in Google Cloud.

It is based on the instructions found [here](https://www.pulumi.com/docs/tutorials/gcp/gcp-py-gke/).

> **NOTE:** The Pulumi docs are based on the Terraform docs, so that comes in very handy

## Getting Started

1. Install Pulumi:

    ```bash
    brew install pulumi
    ```

2. Set up the [Python virtual env](https://docs.python-guide.org/dev/virtualenvs/#virtualenvironments-ref):

    ```bash
    pip install --upgrade --force-reinstall virtualenv
    virtualenv venv
    virtualenv -p venv/bin/python
    source  venv/bin/activate
    python -m pip install --upgrade pip
    pip install --upgrade --force-reinstall -r requirements.txt
    ```

3. Create the new Pulumi stack using the Pulumi service:

    This example assumes that you're using `app.pulumi.com` to store your stack. As a pre-requisite, you'll get set up [here](https://app.pulumi.com). The service is free for personal use.

    ```bash
    pulumi stack init dev
    ```

    This will create a new file named `Pulumi.dev.yaml`.

4. Set up Pulumi configs for Google Cloud

    ```bash
    pulumi config set gcp:project <your_project_name>
    pulumi config set gcp:region <region>
    pulumi config set gcp:zone <zone>
    ```

    These will be added to `Pulumi.dev.yaml`

5. Set up the gcloud service account:

    This section assumes that you already have a [GCP service account](https://www.pulumi.com/docs/intro/cloud-providers/gcp/service-account/) set up:

    ```bash
    # Login to gcloud using the service account
    gcloud auth activate-service-account <service_account_name>@<gcp_project_name>.iam.gserviceaccount.com --key-file=<service_account_key>.json

    # Set the GOOGLE_CREDENTIALS environment variable - Pulumi uses this for service account auth
    export GOOGLE_CREDENTIALS=$(cat <service_account_key>.json)
    ```
    References:
    * [Pulumi - Setup GCP Service Account](https://www.pulumi.com/docs/intro/cloud-providers/gcp/service-account/)
    * [Pulumi - Google Cloud (GCP) Setup](https://www.pulumi.com/docs/intro/cloud-providers/gcp/setup/#google-cloud-platform-gcp-setup)

6. Run Pulumi to provision/destroy your infrastructure

    ```bash
    # Create cluster
    pulumi up

    # Preview changes
    pulumi preview

    # Destroy cluster
    pulumi destroy

    # Destroy the stack
    pulumi stack rm
    ```

# References

* [Pulumi CLI reference](https://www.pulumi.com/docs/reference/cli/)
* [Pulumi GKE cluster creation](https://www.pulumi.com/docs/reference/pkg/python/pulumi_gcp/container/#pulumi_gcp.container.Cluster)
* [Terraform GKE cluster creation](https://www.terraform.io/docs/providers/google/r/container_cluster.html)
