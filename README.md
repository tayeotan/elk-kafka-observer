# Centralized Logging & Monitoring System

## Introduction

This project sets up a centralized logging pipeline for a demo application using **`Filebeat`** → **`Kafka`** → **`Logstash`** → **`Elasticsearch`** → **`Kibana`**, all running in Docker containers on localhost. It demonstrates how to collect, transport, process, and visualize application logs in a way that scales to production systems.

---
## Table of Contents
1. Project Overview
2. Architecture
3. Tools & Service
4. Features & Keynotes
5. Future Improvements

---
## Project Overview

The goal of this project is to build a fully containerized centralized logging stack for a sample application that writes structured logs to a file. Logs are shipped with Filebeat, sent through Kafka as an event bus, processed by Logstash, stored in Elasticsearch, and explored in Kibana dashboards.

The main objectives are:
* Collect application logs from a Dockerized app in a consistent format.
* Use Kafka as a reliable transport layer between log shippers and consumers.
* Parse, enrich, and index logs into Elasticsearch for fast querying.
* Build Kibana searches and dashboards to analyze INFO/WARNING/ERROR events.

---
## Architecture

* `app.py`:
* `filebeat.yml`:
* `logstash.conf`:
* `docker-compose.yml`:
* `app.log`:

---
## Tools & Services

* Docker / Docker Compose – Orchestration for all containers.
* Demo App (Python or similar) – Writes synthetic INFO/WARNING/ERROR logs to /logs/app.log.​
* Filebeat 8.x – Ships log file entries to Kafka, using ECS-compatible fields.
* Apache Kafka – Acts as a distributed log/event bus for log messages.
* Logstash – Reads from Kafka, parses messages, and outputs to Elasticsearch.​
* Elasticsearch 8.x – Stores and indexes log documents into logs-YYYY.MM.DD indices.
* Kibana 8.x – UI for search, dashboards, and visualization of logs.

---
## Features & Keynotes

* End‑to‑end centralized logging pipeline
  * Logs collected from a containerized application and shipped via Filebeat.
  * Kafka used as a decoupling layer between producers and consumers.
* Structured, queryable logs
  * Logs enriched with ECS fields: @timestamp, service, environment, host.name, agent.*, etc.
  * Custom fields extracted: level (INFO/WARNING/ERROR) and msg (log message text).
* Kafka verification
  * Used docker exec and kafka-console-consumer inside the Kafka container to read raw events from the app-logs topic.
  * Observed continuous stream of JSON messages matching the app log patterns and offsets.
* Elasticsearch indexing
  * Documents stored in indices like logs-2026.02.06 with correctly parsed fields and timestamps.
* Kibana analytics
  * Data view logs-* with @timestamp as the time field
  * Saved search demo-app-errors filtering on level: "ERROR"
  * Dashboard centralized-logging-dashboard embedding the saved search for quick error monitoring.

---
## Future Improvements

* Add visualizations:
  * Bar chart: count of logs by level over time.
  * Pie chart: distribution of log levels or services.
* Implement alerts in Kibana for high error rates or repeated database failures.
* Add log parsing for more fields, such as user_id or latency values, using Logstash grok patterns.
* Secure the stack with TLS and user authentication for non‑local deployments.​
* Deploy the stack to a cloud environment (e.g., Docker Swarm, Kubernetes, or managed Elastic Cloud) for scalability.
