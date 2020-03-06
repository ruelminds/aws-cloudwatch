cloudwatch-mon-scripts-python
=============================

[![PyPI version](https://badge.fury.io/py/cloudwatchmon.svg)](https://badge.fury.io/py/cloudwatchmon)

Linux monitoring scripts for the [AWS CloudWatch Service](https://aws.amazon.com/de/cloudwatch/).

Couple of additional features (compared the original AWS monitoring scripts):

- Memory monitoring incl. buffers
- Load monitoring (overall and per CPU core)
- Monitoring of disk inode usage
- Process monitoring
- Fewer dependencies
- Simpler installation


Requirements
------------

- Python 2 (>= 2.6) or Python 3 (>= 3.3)
- Boto >= 2.33.0


Installation
------------

Optionally create a virtual environment and activate it. Then just run
`pip install cloudwatchmon`. Install the scripts in `/usr/local/bin` folder.

For script usage, run:

    mon-put-instance-stats.py --help


Examples
--------

To perform a simple test run without posting data to Amazon CloudWatch:

    mon-put-instance-stats.py --mem-util --verify --verbose

Report memory and disk space utilization to Amazon CloudWatch:

    mon-put-instance-stats.py --mem-util --disk-space-util --disk-path=/

To get utilization statistics for the last 12 hours:

    mon-get-instance-stats.py --recent-hours=12


Configuration
-------------

To allow an EC2 instance to read and post metric data to Amazon CloudWatch,
this IAM policy is required:

```json
{
  "Statement": [
    {
      "Action": [
        "cloudwatch:ListMetrics",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:PutMetricData",
        "autoscaling:DescribeAutoScalingInstances"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

If the policy is configured via an IAM role that is assigned to the EC2
server this script runs on, you're done.

Otherwise you can configure the policy for a user account and export
the credentials before running the script:

```sh
export AWS_ACCESS_KEY_ID=[Your AWS Access Key ID]
export AWS_SECRET_ACCESS_KEY=[Your AWS Secret Access Key]
```

Third option is to create a _~/.boto_ file with this content:

```
[Credentials]
aws_access_key_id = Your AWS Access Key ID
aws_secret_access_key = Your AWS Secret Access Key
```

