# Cost-Optimization
Cost-optimization aka bread n butter for a devops engineer🧑‍💻

Pre-text :
Why we moved to cloud from On-prem: 
1. Overhead/lower maintenance/cost optimisation
2. Enhanced Security

In this demo cost-optimization is done using one such use case

## AWS Cloud Cost Optimization - Identifying Stale Resources
Identifying Stale EBS Snapshots
In this example, we'll create a Lambda function that identifies EBS snapshots that are no longer associated with any active EC2 instance and deletes them to save on storage costs.
Description:
The Lambda function fetches all EBS snapshots owned by the same account ('self') and also retrieves a list of active EC2 instances (running and stopped). For each snapshot, it checks if the associated volume (if exists) is not associated with any active instance. If it finds a stale snapshot, it deletes it, effectively optimizing storage costs.

## So to dhe use case in detail :
Say a dev has created a EC2 instance and attached a volume to it with multiple backups of the volume. Backup was  created as it contains sensitive data. The dev has taken backup(basically snapshot) each day at 10:00 am UTC but as a Devops/Cloud Engineer you have come across this EC2 instance where either say both EC2 + volume is deleted but the backups is still there as stale resource or another scenario where only EC2 instance is deleted but the volume + backup is still there. So moral of the story is we moved from on-prem to cloud to reduce cost but here the cost is becoming higher due to this stale resources. (PS : you can say what if the dev had taken the backup intentionally for some purpose then as devops/cloud engineer we can define a timeline say if the backup has not been used over 6 months we can delete it. FYI : Devops/Engineer can either send a notification a we saw through SNS(Simple notification service) or directly delete. In our case we will delete it.

##Improvements that can be made :
1.  Add a if statement in ebs_stale_snapshosts.py stating if the snapshot has not been used in last 30 days delete it
2.  Integrate the lambda function with cloud watch by creating a cloud watch rule by creating a scheduler within amazon event bridge(acting as a bridge between cloud watch and lambda function which will trigger the ebs_stale_snapshosts.py at a particular time everyday. 
