# oci-prerequisites

This README describes the steps to setup your environment so it can run Terraform modules on OCI.

## Install Terraform

First we need to install Terraform.  Instructions on that are [here](https://www.terraform.io/intro/getting-started/install.html).  You can test that the install was successful by running the command:

    terraform

You should see something like:

![](./images/1%20-%20terraform.png)

## Install the OCI Provider

Next you're going to need to install the [Terraform Provider for Oracle Cloud Infrastructure](https://github.com/oracle/terraform-provider-baremetal/blob/master/README.md).  I'm on a Mac, so I downloaded a copy of the binary, `darwin_amd64.tar.gz` from [here](https://github.com/oracle/terraform-provider-oci/releases) and put it in a new plugins directory.  To do that, I ran the follow commands:

    tar -xvf darwin_amd64.tar.gz
    cd ~/.terraform.d
    mkdir plugins
    cd plugins
    mv ~/Downloads/darwin_amd64/terraform-provider-oci_v2.2.0 ./
    ls

That gave this output:

![](./images/2%20-%20provider.png)

## Setup Keys
Create an SSH keypair for connecting to VM instances by follow [these instructions](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Tasks/creatingkeys.htm).

You really just need to do this:

    ssh-keygen -t rsa -N "" -b 2048 -f ~/.ssh/oci

Now, create a key for OCI API access by following the instructions [here](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/apisigningkey.htm).  Basically, you need to run these commands:

    mkdir ~/.oci
    openssl genrsa -out ~/.oci/oci_api_key.pem 2048
    openssl rsa -pubout -in ~/.oci/oci_api_key.pem -out ~/.oci/oci_api_key_public.pem
    cat ~/.oci/oci_api_key_public.pem | pbcopy

When complete, open a web browser to the console [here](https://console.us-phoenix-1.oraclecloud.com/a/identity/users).  Then select your user and click "Add Public Key."

![](./images/3%20-%20console.png)

## Setup Environment Variables
Now, let's take a look at the `env-vars` file.

![](./images/2%20-%20provider.png)

It pulls values from the keys you created in the earlier steps.  You'll need to update...

 update the `env-vars` file.  It's going tr with the keys you created in the last step.  When you've set all the variables, source the file with the command:

    source env-vars

With that, you're all ready to start running Terraform commands!
