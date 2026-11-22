
# ADR 001: Choose Lambda Architecture

## Status

Accepted

Date: 2026-11-22

## Context

Customer: SHP01 — Alpine Outdoor.

Alpine Outdoor focuses on cost and transports ocean freight.

The platform has three main targets:

- Freshness: shipment tracking data should be available within 15 minutes.
- Reliability: at least 55% on-time delivery on every shipping lane.
- Budget: $20 per month.

The October 2026 shipment analysis produced the following results:

| Shipping lane | Shipments | Average delay | On-time rate |
|---|---:|---:|---:|
| Asia-North Europe | 43 | 42.7 hours | 51.2% |
| Transpacific | 36 | 38.3 hours | 52.8% |
| Transatlantic | 41 | 50.0 hours | 58.5% |
| North Europe-Asia | 36 | 47.7 hours | 72.2% |

Asia-North Europe misses the reliability target by 3.8 percentage points, while Transpacific misses it by 2.2 percentage points.

These results show that carrier performance needs monitoring and investigation.

## Options considered

1. **Lambda architecture:** Maintain separate live and batch processing paths. Shipment events can be processed continuously, while daily carrier rate files can be processed in batches.

2. **Kappa architecture:** Use one streaming processing pipeline for all data. Daily rate files would need to be converted into events and processed through the same streaming system.

## Decision

We chose **Lambda architecture** because Alpine Outdoor needs shipment tracking data within 15 minutes, but carrier rate files arrive daily and do not require real-time processing.

Separating live shipment events from batch rate processing allows each workload to use an appropriate processing method.

We will use Pub/Sub and Dataflow for the proposed live path and Cloud Storage with scheduled BigQuery SQL for the batch path.

## Consequences

- **Cost:** The design avoids operating duplicate messaging infrastructure and uses scheduled BigQuery SQL instead of adding Dataproc. However, the $20 monthly budget must be validated against actual service usage.
- **Freshness:** The live pipeline is designed to support the 15-minute target. Batch rate data will be updated on a schedule.
- **Complexity:** Two processing paths must be maintained and monitored.
- **What gets harder:** Maintaining consistency between live and batch data requires additional care. Reprocessing historical data and reconciling results across both paths also add complexity.
