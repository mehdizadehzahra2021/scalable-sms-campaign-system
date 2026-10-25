Scalable Bulk SMS Campaign System

Overview

This project presents a system architecture design for a bulk SMS campaign platform capable of managing campaigns targeting up to one million recipients.

The primary objective is to design a scalable and resilient message-processing pipeline that supports asynchronous processing, parallel workers, delivery-status tracking, and failure recovery.

Architecture

The proposed architecture includes the following components:

Campaign UI: Interface for creating and managing SMS campaigns.
Campaign API: Validates campaign requests and coordinates campaign execution.
PostgreSQL: Stores campaign metadata, recipient records, and message statuses.
Campaign Preparation Queue: Decouples campaign creation from recipient preparation.
Audience Worker: Processes recipient lists and prepares messages for delivery.
Message Queue: Buffers pending messages and distributes work to multiple workers.
SMS Workers: Process queued messages and communicate with the SMS provider.
SMS Provider: External service responsible for delivering SMS messages.
Reporting API and Dashboard: Provide campaign progress and delivery-status visibility.
Key Design Considerations

Scalability

Asynchronous queues and multiple SMS workers allow message processing to scale independently from campaign creation.

Reliability and Failure Recovery

Worker failures are handled through message acknowledgments, lease expiration, and requeuing of unfinished work.

Ambiguous Delivery Outcomes

If a provider request times out and the delivery outcome is unknown, the message is marked as UNKNOWN until it can be reconciled through provider callbacks, status APIs, or delivery reports.

Observability

Campaign and message statuses provide visibility into processing progress, failures, and delivery outcomes.

Architecture Flow

An operator creates an SMS campaign.
The Campaign API validates and stores the campaign.
Recipient preparation runs asynchronously.
Prepared messages are placed in the message queue.
Multiple SMS workers process messages concurrently.
The SMS provider handles message delivery.
Delivery statuses are reconciled and stored.
The reporting layer exposes campaign progress and results.
Current Status

Status: Architecture design

This repository documents the proposed architecture and its reliability considerations. Production implementation, load testing, and performance benchmarking are separate future steps.

Future Improvements

Implement the APIs and background workers.
Add queue monitoring and operational metrics.
Introduce provider rate limiting and retry policies.
Test worker recovery and ambiguous delivery outcomes.
Perform load testing against a defined throughput target.
Disclaimer

The one-million-recipient figure is the intended campaign scale, not a verified performance benchmark.

