---
layout: default
title: Access BigQuery in another project
parent: Cloud Build
grand_parent: Google Cloud Platform
nav_order: 1
description: How to make Cloud Build in one project access BigQuery in another project using service account and IAM
---

# How to make Cloud Build in one project access BigQuery in another project

## Overview

We have the following configuration:

- Cloud Build in one GCP project (`cloud_build_project`)
- BigQuery dataset in another GCP project (`bq_project`)

Let's assume that the BigQuery dataset name is the same as the GCP project name — `bq_project`.

We would like to achieve the following:

- Make it possible to perform some queries against the BigQuery dataset in `bq_project` using the Cloud Build steps from `cloud_build_project`

### Sample `cloudbuild.yaml`

We are going to use this sample Cloud Build steps file to check if we have the access to BigQuery dataset in another project:

```yaml
steps:
- name: 'google/cloud-sdk:alpine'
  entrypoint: 'bash'
  args:
  - '-eEuo'
  - 'pipefail'
  - '-c'
  - |-
    bq ls -p
    bq ls 'bq_project:'
```

This Cloud Build configuration performs the following:

- `bq ls -p` show all projects
- `bq ls 'bq_project:'` lists the tables in the named dataset `bq_project` (mind the trailing colon `:`, it indicates a dataset).

If we execute this build configuration the output of the above commands would be similar to the following (some output skipped for brevity):

```
# Make sure you're on the right project
$ gcloud config set project cloud_build_project
$ gcloud builds submit --config cloudbuild.yaml
...
       projectId        friendlyName  
 --------------------- -------------- 
  cloud_build_project   Cloud Build Project
...
```

As you can see, we only got the output for the first command — `bq ls -p`, for the second one we got no output.

To make it access the BigQuery dataset in another project, we need to add Cloud Build service account from `cloud_build_project` as a user to `bq_project`, and grant the necessary permissions.

## Get Cloud Build service accont email

First of all we need to get Cloud Build service account email.

Naviage to **Cloud Build** > **Settings** page. There you can see something like the following:

> Service account email: xxxxxxxxxxxx@cloudbuild.gserviceaccount.com 

Copy that email address, we are going to use in in the next step.

## Create a new IAM user

Now, when you have Cloud Builde service account email copied, let's create a new IAM user in `bq_project`.

Using the top menu, switch from one project (`cloud_build_project`) to another (`bq_project`).

Naviagte to **IAM & Admin** > **IAM**

Click **Add** button.

Paste the copied Cloud Build service account email into a **New memebers** field.

From the **Select role** drop-down menu choose **BigQuery Metadata Viewer**.

Click **Save** button.

## Check results

Some output skipped for brevity:

```
# Make sure you're on the right project
$ gcloud config set project cloud_build_project
$ gcloud builds submit --config cloudbuild.yaml
...
       projectId        friendlyName  
 --------------------- -------------- 
  cloud_build_project   Cloud Build Project
  bq_project            BigQuery Project
  datasetId  
 -----------
  my_table1
  my_table2
...
```

As you can see, Cloud Build now has access to both projects and can list tables from the other project BigQuery dataset.
