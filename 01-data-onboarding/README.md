# Project 1: E-commerce Apache Web Server Log Onboarding

## About This Project

In this project, I walk through how I would handle a Splunk data onboarding request for an e-commerce company.

I chose this scenario because web server monitoring is a practical example of how Splunk can help application and IT teams investigate errors, understand website traffic, and troubleshoot issues.

My goal is to document the entire onboarding process, from gathering requirements and preparing configurations to validating data and troubleshooting.


**Project status:** Portfolio case study with completed hands-on Splunk ingestion and SPL validation using synthetic Apache logs. Universal Forwarder deployment and end-to-end forwarding are documented as proposed implementation steps and have not been tested in this project.


## Business Request

Let's say I receive a ticket from an application support team asking me to onboard Apache web server logs into Splunk.

The team wants to:

- Monitor website traffic.
- Investigate HTTP 404 and 500 errors.
- Identify unusual activity.
- Analyze traffic trends.
- Build searches, reports, and alerts.

My responsibility would be to understand their requirements, configure data collection, and make sure the logs are searchable in Splunk.

## Step 1: Meeting With the Client

The first thing I would do is meet with the application owner.

Before configuring anything, I want to understand what data I am working with and what the client expects to see in Splunk.

I would collect:

- Server hostname and operating system.
- Log file paths.
- Sample log files in their original format.
- Expected data volume.
- Index and sourcetype requirements.
- Data retention policy.
- Access and permission requirements.
- DEV and PROD environment details.

For this scenario, I am using the following example requirements:

| Setting | Value |
|---|---|
| Data source | Apache HTTP Server |
| Operating system | Linux |
| Access log | `/var/log/httpd/access_log` |
| Error log | `/var/log/httpd/error_log` |
| Collection method | Universal Forwarder |
| Index | `web` |
| Sourcetypes | `access_combined`, `apache_error` |
| Retention | 90 days (example, pending approval) |

I would document everything in a requirements spreadsheet and share it with the client to confirm that we are on the same page.

## Step 2: Planning the Data Flow

For this project, I would use a Splunk Universal Forwarder installed on the Apache server.

The data flow would look like this:

**Apache Web Server → Universal Forwarder → Indexer → Search Head**

The Universal Forwarder collects the log files and sends the events to the Indexer.

The Search Head allows users to search and analyze the indexed data.

## Step 3: Preparing the Configuration

I would prepare a custom Technology Add-on (TA) for the Apache log collection.

My configuration would include:

- **inputs.conf:** I define the Apache log files I want Splunk to monitor, along with the index and sourcetypes.
- **outputs.conf:** I configure where the Universal Forwarder sends the data.
- **props.conf:** I configure timestamp recognition and event parsing based on the log samples.

I would use Deployment Server to distribute the forwarder configuration and the appropriate deployment mechanism for the Indexer-side parsing configuration. 
The configuration files included in this repository are examples of how I would prepare Apache log onboarding in a distributed Splunk environment. They have not been deployed or tested through a Universal Forwarder or Deployment Server as part of this project.

## Step 4: DEV Testing and SPL Validation

Before deploying a new data source to production, I would validate the complete onboarding process in DEV, including Universal Forwarder connectivity and data collection.

For this portfolio project, I completed hands-on testing by manually uploading synthetic Apache logs into Splunk Enterprise and validating the indexed events using SPL.

I would check whether:

- The Universal Forwarder is running.
- The log files are being monitored.
- Events are arriving in the correct index.
- Sourcetypes are assigned correctly.
- Timestamps are extracted properly.
- Events are not being incorrectly merged or truncated.

I would also use SPL to verify the data.

**Check event counts by sourcetype:**

```spl
index=web
| stats count by sourcetype
```

**Check HTTP errors:**

```spl
index=web sourcetype=access_combined
| rex field=_raw "\"\s+(?<http_status>\d{3})\s+"
| where http_status>=400
| stats count by http_status
| sort -count

```

**Example SPL — Review traffic trends (not yet validated):**

```spl
index=web sourcetype=access_combined
| timechart span=1h count
```

I completed hands-on ingestion and SPL validation using synthetic Apache logs in a Splunk Enterprise test environment. I verified event counts, HTTP errors, checkout failures, and Apache backend proxy errors.

The validated SPL searches and test results are available in the `queries/` and `test-results/` folders. Universal Forwarder and Deployment Server testing remain planned implementation steps.


## Step 5: Troubleshooting

If the client tells me that the data is missing, I would start troubleshooting from the source.

First, I would verify that the Universal Forwarder is running and has permission to read the Apache log files.

Next, I would check the active configurations using **btool**, review `outputs.conf`, and confirm connectivity to the receiving Indexer.

I would also review Splunk internal logs for forwarding errors and verify that the index exists and the event timestamps are correct.

My goal would be to identify where the data flow is failing rather than making random configuration changes.

## Step 6: Moving to Production

Once the DEV testing is successful, I would review the results with the application owner.

After receiving approval, I would prepare the production deployment, follow the company's change management process, and verify data ingestion again in PROD.

I would also document the final configuration and any troubleshooting steps for future support.

## Project Files

I organized my project files to show how I approach Apache log onboarding and troubleshooting.

- `inputs.conf` — Apache access and error log collection.
- `outputs.conf` — Universal Forwarder communication with the Indexer.
- `props.conf` — Timestamp extraction and event parsing.
- `sample_access.log` — Synthetic Apache access log examples.
- `sample_error.log` — Synthetic Apache error log examples.
- `queries/` — SPL searches for ingestion validation, HTTP errors, checkout failures, and backend errors.
- `TROUBLESHOOTING.md` — My approach to investigating checkout failures.
- `test-results/` — Completed SPL validation results, investigation findings, and supporting screenshots.

## What I Want to Demonstrate

Through this project, I want to demonstrate my understanding of:

- End-to-end Splunk data onboarding.
- Working with application owners.
- Universal Forwarder configuration.
- Custom Technology Add-ons.
- Data parsing and timestamp extraction.
- Deployment Server and Indexer configuration.
- SPL validation.
- Troubleshooting and production deployment planning.


## Lab Validation and Test Evidence

I validated Apache access and error log ingestion in a Splunk Enterprise test environment.

I executed four SPL searches to verify event ingestion, identify HTTP errors, investigate checkout failures, and analyze Apache backend proxy errors.

All four searches returned the expected results.

The validation results, investigation findings, and Splunk screenshots are documented here:

[View SPL Validation Results and Evidence](test-results/README.md)

