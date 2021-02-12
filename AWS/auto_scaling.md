# auto scaling

1.remove order
```
1.select on az with have much instance
2.oldest launch
3.the instance closest next billing hour
4.random select
```

2.types of scaling policies
```
Target tracking scaling: based on a target value for a specific metric
Step scaling: based on a set of scaling adjustments
Simple scaling: based on a single scaling adjustment
Scheduled Scaling: based on a schedule(predictable load changes), not dynamic scaling
```

3. new IAM, how to do?
```
1.設定auto scaling是靠launch configuration.
2.設定之後不能modify
```

4.cooldown period
```
1.It ensures that the Auto Scaling group does not launch or terminate additional EC2 instances before the previous scaling activity takes effect.
2.Its default value is 300 seconds.
3.It is a configurable setting for your Auto Scaling group.
```


# help
```
Category: CSAA – Design High-Performing Architectures
An application is hosted in an Auto Scaling group of EC2 instances. To improve the monitoring process, you have to configure the current capacity to increase or decrease based on a set of scaling adjustments. This should be done by specifying the scaling metrics and threshold values for the CloudWatch alarms that trigger the scaling process.

Which of the following is the most suitable type of scaling policy that you should use?
```
```
Step scaling
```
