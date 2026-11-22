
# Alpine Outdoor — Delivery Performance Metrics

**Customer ID:** SHP01  
**Focus:** Cost  
**Freshness target:** 15 minutes  
**Reliability target:** 55% on time on every lane  
**Monthly platform budget:** $20

## Delay and reliability by lane

| Shipping lane | Origin | Destination | Shipments | Average delay (hours) | On-time rate | Target | Status |
|---|---|---|---:|---:|---:|---:|---|
| Asia-North Europe | CNNGB | DEHAM | 43 | 42.7 | 51.2% | 55% | Below target |
| Transpacific | KRPUS | USLGB | 36 | 38.3 | 52.8% | 55% | Below target |
| Transatlantic | NLRTM | USHOU | 41 | 50.0 | 58.5% | 55% | Meets target |
| North Europe-Asia | GBFXT | CNSHA | 36 | 47.7 | 72.2% | 55% | Meets target |

## Key findings

- Asia-North Europe misses the reliability target by 3.8 percentage points.
- Transpacific misses the reliability target by 2.2 percentage points.
- Transatlantic meets the target at 58.5%.
- North Europe-Asia has the highest on-time rate at 72.2%.
- Transatlantic has the highest average delay at 50 hours.

## Recommendation

Prioritise investigating carrier performance on the Asia-North Europe and Transpacific lanes.

Since Alpine Outdoor focuses on cost, carrier negotiations should balance competitive freight rates with the requirement to achieve at least 55% on-time delivery on every lane.

**Data source:** BigQuery tables `freight.bookings` and `freight.schedules`, October 2026 synthetic workshop data.

## Carrier-level investigation

### Asia-North Europe — CNNGB to DEHAM

| Carrier | Shipments | Within 24 hours | On-time rate |
|---|---:|---:|---:|
| KST | 4 | 4 | 100.0% |
| BLC | 9 | 9 | 100.0% |
| ORS | 3 | 2 | 66.7% |
| NWL | 3 | 2 | 66.7% |
| HRZ | 7 | 3 | 42.9% |
| ATL | 17 | 2 | 11.8% |
| **Total** | **43** | **22** | **51.2%** |

### Transpacific — KRPUS to USLGB

| Carrier | Shipments | Within 24 hours | On-time rate |
|---|---:|---:|---:|
| KST | 5 | 4 | 80.0% |
| NWL | 6 | 4 | 66.7% |
| BLC | 9 | 6 | 66.7% |
| ATL | 12 | 5 | 41.7% |
| ORS | 4 | 0 | 0.0% |
| **Total** | **36** | **19** | **52.8%** |

### Investigation priorities

- Investigate ATL on Asia-North Europe, with 17 shipments and 11.8% on-time performance.
- Investigate ORS on Transpacific, with 4 shipments and 0% on-time performance.
- Evaluate KST, BLC and NWL as potential alternatives, considering both rates and reliability.
- Investigate missing arrival data because it affects the measured reliability.

*Note: These are synthetic workshop results. Carrier-level sample sizes vary, so interpret percentages carefully.*
