# Project 1: Splunk Data Onboarding

## Objective
Demonstrate how a Splunk Universal Forwarder collects Linux system logs and forwards them to a Splunk Indexer.

## Architecture
Linux Server → Universal Forwarder → Splunk Indexer → Search Head

## Components
- Splunk Universal Forwarder
- Splunk Enterprise Indexer
- Splunk Search Head
- Linux system logs

## Configuration Files
- inputs.conf — Defines which logs to collect.
- outputs.conf — Defines where to forward the data.
- props.conf — Controls event parsing and timestamp extraction.
- transforms.conf — Defines transformations such as masking and filtering.

## Verification
Use SPL searches to confirm that events are being indexed correctly.

## Troubleshooting
- Check Universal Forwarder status.
- Verify inputs.conf and outputs.conf.
- Check network connectivity.
- Review Splunk internal logs.
- Use btool to verify active configurations.

## Project Status
Documentation and sample configurations in progress.

All examples use synthetic data and sample configurations.
