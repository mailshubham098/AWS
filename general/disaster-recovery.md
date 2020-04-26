Disaster Recovery Overview

- Any event that has a negative impact on a company’s business continuity or finances is a disaster
- Disaster recovery (DR) is about preparing for and recovering from a disaster
- What kind of disaster recovery?
  - On-premise => On-premise: traditional DR, and very expensive
  - On-premise => AWS Cloud: hybrid recovery
  - AWS Cloud Region A => AWS Cloud Region B
- Need to define two terms:
  - RPO: Recovery Point Objective (last checkpoint)
  - RTO: Recovery Time Objective (time to recover after disaster strikes)
-  RPO ->  disaster -> RTO
- time between disaster and RPO => data loss
- time between RTO and disaster time => recovery time

Disaster Recovery Strategies
- Backup and Restore
  - Data backed up regularly and restored on disaster
  - RPO/RTO in Hrs
  - Cost = $
- Pilot Light
  - A small version of the app is always running in the cloud
  - Useful for the critical core (pilot light)
  - Very similar to Backup and Restore
  - Faster than Backup and Restore as critical systems are already up
  - RPO/RTO in 10s of Mins
  - Cost = $$
- Warm Standby
  - Full system is up and running, but at minimum size
  - Upon disaster, we can scale to production load
  - RPO/RTO in  Mins
  - Cost = $$$
- Hot Site / Multi Site Approach
  - Very low RTO (minutes or seconds) – very expensive
  - Full Production Scale is running AWS and On Premise
  - RPO/RTO in real-time/secs
  - Cost = $$$$


Faster RTO
- Hot Site / Multi Site Approach > Warm Standby > Pilot Light > Backup & restore
