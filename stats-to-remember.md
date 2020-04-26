CloudWatch
  - CloudWatch retains metric data as for data points follows:
    - period < 60 secs data points -> available for 3 hours
    - period of 1 min -> 15 days
    - period of 5 min -> 63 days (default)
    - period of 1 hr -> 15 months
  - Enable detailed monitoring for less than 5 mins ( standard monitoring )

  - CloudWatch default monitoring metrics
    - CPU Utilization
    - Network Utilization
    - Disk i/O
  - CloudWatch agent
    - Memory utilization
    - Disk swap utilization
    - Disk space utilization
    - Page file utilization
    - Log collection

Glacier
- Glacier provides three data access tiers:
  - Expedited – data accessed is typically made available within 1–5 minutes.
  - Standard – data accessed is typically made available within 3–5 hours.
  - Bulk – data accessed is typically made available within 5–12 hours.
