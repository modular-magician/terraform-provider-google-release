---
# ----------------------------------------------------------------------------
#
#     ***     AUTO GENERATED CODE    ***    Type: MMv1     ***
#
# ----------------------------------------------------------------------------
#
#     This file is automatically generated by Magic Modules and manual
#     changes will be clobbered when the file is regenerated.
#
#     Please read more about how to change this file in
#     .github/CONTRIBUTING.md.
#
# ----------------------------------------------------------------------------
subcategory: "AlloyDB"
page_title: "Google: google_alloydb_cluster"
description: |-
  A managed alloydb cluster.
---

# google\_alloydb\_cluster

A managed alloydb cluster.

~> **Warning:** This resource is in beta, and should be used with the terraform-provider-google-beta provider.
See [Provider Versions](https://terraform.io/docs/providers/google/guides/provider_versions.html) for more details on beta resources.

To get more information about Cluster, see:

* [API documentation](https://cloud.google.com/alloydb/docs/reference/rest/v1beta/projects.locations.clusters/create)
* How-to Guides
    * [AlloyDB](https://cloud.google.com/alloydb/docs/)

~> **Warning:** All arguments including `initial_user.password` will be stored in the raw
state as plain-text. [Read more about sensitive data in state](https://www.terraform.io/language/state/sensitive-data).

<div class = "oics-button" style="float: right; margin: 0 0 -15px">
  <a href="https://console.cloud.google.com/cloudshell/open?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fterraform-google-modules%2Fdocs-examples.git&cloudshell_working_dir=alloydb_cluster_basic&cloudshell_image=gcr.io%2Fgraphite-cloud-shell-images%2Fterraform%3Alatest&open_in_editor=main.tf&cloudshell_print=.%2Fmotd&cloudshell_tutorial=.%2Ftutorial.md" target="_blank">
    <img alt="Open in Cloud Shell" src="//gstatic.com/cloudssh/images/open-btn.svg" style="max-height: 44px; margin: 32px auto; max-width: 100%;">
  </a>
</div>
## Example Usage - Alloydb Cluster Basic


```hcl
resource "google_alloydb_cluster" "default" {
  provider   = google-beta
  cluster_id = "alloydb-cluster"
  location   = "us-central1"
  network    = "projects/${data.google_project.project.number}/global/networks/${google_compute_network.default.name}"
}

data "google_project" "project" {
  provider = google-beta
}

resource "google_compute_network" "default" {
  provider = google-beta
  name = "alloydb-cluster"
}
```
<div class = "oics-button" style="float: right; margin: 0 0 -15px">
  <a href="https://console.cloud.google.com/cloudshell/open?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fterraform-google-modules%2Fdocs-examples.git&cloudshell_working_dir=alloydb_cluster_full&cloudshell_image=gcr.io%2Fgraphite-cloud-shell-images%2Fterraform%3Alatest&open_in_editor=main.tf&cloudshell_print=.%2Fmotd&cloudshell_tutorial=.%2Ftutorial.md" target="_blank">
    <img alt="Open in Cloud Shell" src="//gstatic.com/cloudssh/images/open-btn.svg" style="max-height: 44px; margin: 32px auto; max-width: 100%;">
  </a>
</div>
## Example Usage - Alloydb Cluster Full


```hcl
resource "google_alloydb_cluster" "full" {
  provider     = google-beta
  cluster_id   = "alloydb-cluster-full"
  location     = "us-central1"
  network      = "projects/${data.google_project.project.number}/global/networks/${google_compute_network.default.name}"

  initial_user {
    user     = "alloydb-cluster-full"
    password = "alloydb-cluster-full"
  }

  automated_backup_policy {
    location      = "us-central1"
    backup_window = "1800s"
    enabled       = true

    weekly_schedule {
      days_of_week = ["MONDAY"]

      start_times {
        hours   = 23
        minutes = 0
        seconds = 0
        nanos   = 0
      }
    }

    quantity_based_retention {
      count = 1
    }

    labels = {
      test = "alloydb-cluster-full"
    }
  }

  labels = {
    test = "alloydb-cluster-full"
  }
}

data "google_project" "project" {
  provider = google-beta
}

resource "google_compute_network" "default" {
  provider = google-beta
  name = "alloydb-cluster-full"
}
```

## Argument Reference

The following arguments are supported:


* `network` -
  (Required)
  The relative resource name of the VPC network on which the instance can be accessed. It is specified in the following form:
  "projects/{projectNumber}/global/networks/{network_id}".

* `cluster_id` -
  (Required)
  The ID of the alloydb cluster.


- - -


* `labels` -
  (Optional)
  User-defined labels for the alloydb cluster.

* `display_name` -
  (Optional)
  User-settable and human-readable display name for the Cluster.

* `initial_user` -
  (Optional)
  Initial user to setup during cluster creation.
  Structure is [documented below](#nested_initial_user).

* `automated_backup_policy` -
  (Optional)
  The automated backup policy for this cluster.
  If no policy is provided then the default policy will be used. The default policy takes one backup a day, has a backup window of 1 hour, and retains backups for 14 days.
  Structure is [documented below](#nested_automated_backup_policy).

* `location` -
  (Optional)
  The location where the alloydb cluster should reside.

* `project` - (Optional) The ID of the project in which the resource belongs.
    If it is not provided, the provider project is used.


<a name="nested_initial_user"></a>The `initial_user` block supports:

* `user` -
  (Optional)
  The database username.

* `password` -
  (Required)
  The initial password for the user.
  **Note**: This property is sensitive and will not be displayed in the plan.

<a name="nested_automated_backup_policy"></a>The `automated_backup_policy` block supports:

* `backup_window` -
  (Optional)
  The length of the time window during which a backup can be taken. If a backup does not succeed within this time window, it will be canceled and considered failed.
  The backup window must be at least 5 minutes long. There is no upper bound on the window. If not set, it will default to 1 hour.
  A duration in seconds with up to nine fractional digits, terminated by 's'. Example: "3.5s".

* `location` -
  (Optional)
  The location where the backup will be stored. Currently, the only supported option is to store the backup in the same region as the cluster.

* `labels` -
  (Optional)
  Labels to apply to backups created using this configuration.

* `weekly_schedule` -
  (Required)
  Weekly schedule for the Backup.
  Structure is [documented below](#nested_weekly_schedule).

* `time_based_retention` -
  (Optional)
  Time-based Backup retention policy.
  Structure is [documented below](#nested_time_based_retention).

* `quantity_based_retention` -
  (Optional)
  Quantity-based Backup retention policy to retain recent backups.
  Structure is [documented below](#nested_quantity_based_retention).

* `enabled` -
  (Optional)
  Whether automated automated backups are enabled.


<a name="nested_weekly_schedule"></a>The `weekly_schedule` block supports:

* `days_of_week` -
  (Optional)
  The days of the week to perform a backup. At least one day of the week must be provided.
  Each value may be one of `MONDAY`, `TUESDAY`, `WEDNESDAY`, `THURSDAY`, `FRIDAY`, `SATURDAY`, and `SUNDAY`.

* `start_times` -
  (Required)
  The times during the day to start a backup. At least one start time must be provided. The start times are assumed to be in UTC and to be an exact hour (e.g., 04:00:00).
  Structure is [documented below](#nested_start_times).


<a name="nested_start_times"></a>The `start_times` block supports:

* `hours` -
  (Optional)
  Hours of day in 24 hour format. Should be from 0 to 23. An API may choose to allow the value "24:00:00" for scenarios like business closing time.

* `minutes` -
  (Optional)
  Minutes of hour of day. Must be from 0 to 59.

* `seconds` -
  (Optional)
  Seconds of minutes of the time. Must normally be from 0 to 59. An API may allow the value 60 if it allows leap-seconds.

* `nanos` -
  (Optional)
  Fractions of seconds in nanoseconds. Must be from 0 to 999,999,999.

<a name="nested_time_based_retention"></a>The `time_based_retention` block supports:

* `retention_period` -
  (Optional)
  The retention period.
  A duration in seconds with up to nine fractional digits, terminated by 's'. Example: "3.5s".

<a name="nested_quantity_based_retention"></a>The `quantity_based_retention` block supports:

* `count` -
  (Optional)
  The number of backups to retain.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:

* `id` - an identifier for the resource with format `projects/{{project}}/locations/{{location}}/clusters/{{cluster_id}}`

* `name` -
  The name of the cluster resource.

* `uid` -
  The system-generated UID of the resource.

* `database_version` -
  The database engine major version. This is an output-only field and it's populated at the Cluster creation time. This field cannot be changed after cluster creation.

* `backup_source` -
  Cluster created from backup.
  Structure is [documented below](#nested_backup_source).

* `migration_source` -
  Cluster created via DMS migration.
  Structure is [documented below](#nested_migration_source).


<a name="nested_backup_source"></a>The `backup_source` block contains:

* `backup_name` -
  (Optional)
  The name of the backup resource.

<a name="nested_migration_source"></a>The `migration_source` block contains:

* `host_port` -
  (Optional)
  The host and port of the on-premises instance in host:port format

* `reference_id` -
  (Optional)
  Place holder for the external source identifier(e.g DMS job name) that created the cluster.

* `source_type` -
  (Optional)
  Type of migration source.

## Timeouts

This resource provides the following
[Timeouts](/docs/configuration/resources.html#timeouts) configuration options:

- `create` - Default is 10 minutes.
- `update` - Default is 10 minutes.
- `delete` - Default is 10 minutes.

## Import


Cluster can be imported using any of these accepted formats:

```
$ terraform import google_alloydb_cluster.default projects/{{project}}/locations/{{location}}/clusters/{{cluster_id}}
$ terraform import google_alloydb_cluster.default {{project}}/{{location}}/{{cluster_id}}
$ terraform import google_alloydb_cluster.default {{location}}/{{cluster_id}}
$ terraform import google_alloydb_cluster.default {{cluster_id}}
```

## User Project Overrides

This resource supports [User Project Overrides](https://www.terraform.io/docs/providers/google/guides/provider_reference.html#user_project_override).