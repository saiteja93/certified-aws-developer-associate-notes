# ASG: Auto Scaling Group

In real-life, the load on your websites and applications can change. You can create and get rid of servers very quickly

The goal of an Auto Scaling Group (ASG) is to:
* Scale out (add EC2 Instances) to match an increased load
* Scale in (remove EC2 Instances) to match a decreased load
* Ensure we have a minimum and a maximum number of machines running
* **Automatically register new instances to a load balancer**

#### ASGs have the following attributes
* A launch configuration
    * AMI + Instance Type
    * EC2 User Data
    * EBS Volumes
    * Security Groups
    * SSH Key Pair
* Min Size / Max Size / Initial Capacity / Desired Capacity
* Network + Subnets Information
* Load Balancer Information
* Scaling Policies

#### Auto Scaling Alarms
* It is possible to scale an ASG based on CloudWatch alarms
* An alarm monitors a metric (such as Average CPU)
* Metrics are computed for the overall ASG instances
* Based on the alarm:
    * We can create a scale-out policies (increase the number of instances)
    * We can create a scale-in policies (decrease the number of instances)

#### New Auto Scaling Rules
* It is now possible to define “better” auto scaling rules that are directly managed by EC2
    * Target Average CPU Usage
    * Number of requests on the ELB per instance
    * Average Network In
    * Average Network Out
* These rules are easier to set up and can make more sense

#### Auto Scaling Custom Metric
* We can auto scale based on a custom metric (ex: number of connected users)
* 1. Send custom metrics from an application on EC2 to CloudWatch (PutMetric API)
* 2. Create a CloudWatch alarm to react to low / high values
* 3. Use the CloudWatch Alarm as the scaling policy for ASG

#### Auto Scaling Groups - Scaling Policies
*   Target Tracking Scaling - Easy/simple. Eg - I want the avg ASG CPU to stay at 40%. Scale out if more, scale in if less.
*   Simple/Step Scaling - 
        * Has better control on how many units you add and remove
        * Eg: When CloudWatch alarm is triggered (example CPU > 70%), add 2 units
*   Scheduled Actions:
        * Anticipate scaling based on usage patterns during the week.

#### ASG - Scaling Cool Downs
* Ensures that ASG doesn't launch or terminate instances before the previous scaling activity takes effect. Kind of settle down before any future scaling effect takes place.
* There is a default cool down period. But you can set a Scaling specific cooldown policy. Scaling specific cooldown policy overrides the default.
* If application is scaling up and scaling down multiple times, modify the cool down timers and the CloudWatch Alarm period that triggers the scale in. 

#### ASG Summary
* Scaling policies can be on CPU, Network… and can even be on custom metrics or based on a schedule (if you know your visitors patterns)
* ASGs use Launch configurations or Launch Templates (newer)
* To update an ASG, you need to provide a newer version of Launch configurations or Launch Templates (allow you to use Spot fleet of instances).
* IAM roles attached to an ASG will get assigned to EC2 instances
* ASG are free. You pay for the underlying resources being launched
* Having instances under an ASG means that if they get terminated for whatever reason, the ASG will restart them. Extra safety
* ASG can terminate instances marked as unhealthy by an LB (and hence replace them)