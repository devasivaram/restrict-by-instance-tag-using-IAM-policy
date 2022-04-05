# restrict-by-instance-tag-using-IAM-policy

## Description

Created an IAM policy that could list all the instances on AWS console, but which only has the privilege to start, reboot and stop a paricular instance by their name tag.

## Motive

Consider we have two instances with the same name tag "webserver" and project tag set to "Zomato". But the enviornment tag(env) of one instance is set to "prod" and the other is set to "dev". So, what we exacly going to do is creating an IAM user with a custom policy so that this user will have the privilege to see the list of instances on the AWS console, but will only have privilege to stop, start and reboot the instance with env tage set to "prod".

## Steps performed

## Step 1

Created two EC2 instances with the same tag "webserver" and project tag set to "Zomato". We also add an addition tag "env", whose value is set to "prod" for one instance and "dev" for the other instance.

> So, Instance 1:

```sh
Name = websever
project = apache
env = prod
```

> Instance 2:

```sh
Name = webserver
project = apache
env = dev
```

## Step 2

Create an IAM user "test" with acess as "Password - AWS Management Console access". Choose a custom password or an auto generated password for the user.

## Step 3

Now createthe custom IAM policy that enables the IAM user "test" to see the list of all the instances on the AWS console, but will only have privilege to stop, start and reboot the instance with "env" tag set to "prod".

You can create IAM policy from your AWS console [click here]( https://aws.amazon.com/console/ ) >> IAM >> Policies >> Create policy.

So, the required IAM policy is:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
            "ec2:Describe*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:RebootInstances"
            ],
            "Resource": "arn:aws:ec2:ap-south-1:account-id:instance/*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/Name": "webserver",
                    "ec2:ResourceTag/project": "zomato",
                    "ec2:ResourceTag/env": "prod"
                }
            }
        }
    ]
}
```
Now, we need to attach this policy to the IAM user "test".

## Step 4

Now we need to login to the AWS console as the IAM user "test". Here we will be able to see the list of instances. But we will be only able to successfully start, stop and reboot the instance with env tag set to "prod".

But when we try to stop/reboot the instance with "env" tag set to "dev", it will show the privilege error:

## Conclusion

So this is the IAM policy that could exaclty stop, start and reboot an instance based on the tags we have provided for it. Hope this little piece of work helps you out in some way. Thank you and have a great day!

### ⚙️ Connect with Me
<p align="center">
<a href="https://www.instagram.com/dev_anand__/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/dev-anand-477898201/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a>
