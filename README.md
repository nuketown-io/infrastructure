# NukeTown - Infrastructure

![Cloud](./images/cloud.png 'Cloud')

Our infrastructure is based on:

- Cloudflare Pages
- Google Cloud Run - Services
- Google Cloud Run - Jobs
- Google Cloud Scheduler
- Goolge Cloud Artifact Registry

## Architecture

### Dashboard, App (ReactJs)

ReactJs applications are published on Cloudflare Pages.
There is a pipeline (on Cloudflare) that builds and publishes the application on every push to the `main` branch.

### API (NodeJs)

Api is published on Google Cloud Run Services.
Every tag pushed to the `main` branch is published (by GitHub Actions) on Google Cloud Artifact Registry and automatically delivered to the API service.

### Jobs (NodeJs)

`dns-cleaner`, `scan-activator`, `endpoints-scanner` are builded (by GitHub Actions) and published on Google Cloud Artifact Registry.
Currently we have a manual process to update the jobs on Google Cloud Run Jobs.

### Google Cloud Scheduler

It's used to schedule the execution of the jobs (dns-cleaner and scan-activator).

## Components

### Scan Activator

It's a job that activates the scan of endpoints, and it's scheduled to run every 5 minutes.
The job parse the endpoints in the database and activates the scan job for the correct region.
So, for example, it there aren't endpoints for the region `europe-central2`, the scan job for that region will not be activated.

#### Regions

![Regions](./images/map_europe.png 'Regions')

Available regions are currently 4:

- Finland
- Warsaw
- Paris
- Madrid

When you create and endpoint you can specify the region where from where to scan it.

### DNS Cleaner

This is a job that simply clear the DNS mapping for the application on Cloudflare (because currently Cloudflare Pages can handle max only 500 mappings).

## Note

About this document:

- is a work in progress, and it's not complete.
- is not a tutorial, it's just a reference for the project.
- is not a guide, it's just a reference for the project.
- wants only to give an idea of the project and inspire other people.
