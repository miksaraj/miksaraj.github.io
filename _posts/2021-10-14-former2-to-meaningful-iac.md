---
layout: post
title:  "Existing cloud infrastructure to code, Part 1: From Former2 to Meaningful IaC"
categories: cloud
author: Mikko Rajakangas
---
So you want to start getting your AWS hosted
cloud infrastructure firmly under the control
of your DevOps team. You want to turn your
[manually setup infrastructure into code]({% post_url 2021-09-28-iac-project %}).
How do you get from here to there?<!--excerpt--> Frankly,
turns out it's one of those problems where
StackOverflow, AWS documentation or no other
form of Google-fu seems to hold good enough
of an answer. Most what's written about using
AWS CDK out there deals with starting from
scratch, or at least from CloudFormation
generated infrastructure - which strictly
speaking is already IaC, but you'll just
want to get it more developer friendly.
Because let's face it, CfN is not really
that pleasurable for humans to read nor edit.

Thanks to [Ian McKay](https://github.com/iann0036)
we have great tool for the job of extracting
existing infrastructure into code: [Former2](https://former2.com/).
Now the problem here is that there are some security
considerations to take into account with a
website out there and inputting any form of
AWS credentials in there. I'm sure Ian has
seen to mitigating any possible security issues,
but for those paranoid enough, he does offer
something better - a way that I implore you to
go with: just fork the repo and [host it locally](https://github.com/iann0036/former2/blob/master/HOSTING.md).
It's simple, secure, effective and as a result
(I'll leave the details of how to use Former2
for you to figure out) you'll have your existing
infrastructure extractable as code in a few possible
formats.

If you go with extracting your infrastructure as CDK
code, there is a slight problem that you'll be able
to spot rightaway. What Former2 does is generate
a semi-unordered list of L1 constructs with generic
IDs. Like the EC2 instance L1 construct here:

```ts
const EC2Instance7 = new ec2.CfnInstance(this, 'EC2Instance7', {
            imageId: "ami-c51e3eb6",
            instanceType: "t2.medium",
            keyName: "<KeyName>",
            availabilityZone: EC2Instance8.attrAvailabilityZone,
            tenancy: "default",
            subnetId: EC2Subnet4.ref,
            ebsOptimized: false,
            securityGroupIds: [
                EC2SecurityGroup7.ref
            ],
            sourceDestCheck: true,
            blockDeviceMappings: [
                {
                    deviceName: "/dev/xvda",
                    ebs: {
                        encrypted: false,
                        volumeSize: 100,
                        snapshotId: "<snap-uuid>",
                        volumeType: "gp2",
                        deleteOnTermination: true
                    }
                }
            ],
            userData: "<hashed userData goes here>",
            iamInstanceProfile: IAMRole3.ref,
            tags: [
                {
                    key: "Name",
                    value: "example"
                }
            ],
            hibernationOptions: {
                configured: false
            },
            enclaveOptions: {
                enabled: false
            }
        });
```

There's a alot of stuff going on there. Best
of all, some of the entries generated into
the L1 construct right there are default
values nonetheless, and while they could
be required when defining an L1 construct,
such default values can usually just be
brushed off when defining L2 constructs.
Let's look at the same EC2 instance definition
turned into an L2 construct:

```ts
const ExampleInstance = new Instance(this, 'example', {
            availabilityZone: PrivateSubnet1.availabilityZone,
            blockDevices: [
                {
                    deviceName: '/dev/xvda',
                    volume: BlockDeviceVolume.ebsFromSnapshot('<snap-uuid>')
                } as BlockDevice
            ],
            instanceName: 'example',
            instanceType: InstanceType.of(InstanceClass.T2, InstanceSize.MEDIUM),
            machineImage: MachineImage.genericLinux({ 'eu-west-1a' : 'ami-c51e3eb6' }),
            securityGroup: ExampleSecurityGroup,
            userData: ExampleUserData,
            vpc: ExampleVPC,
            role: ExampleServiceRole
        })
```

Much better, if you ask me. Much, much more
developer friendly in any case. It doesn't
end here, however. So what Former2 does is
it generates one file with everything in your
infrastructure in there, one after another,
with no contextual structure whatsoever.
Frankly, you can't expect such things from
a tool like Former2, but it does lead for
you as a developer having to go through the
whole file and restructure it yourself. The
structure of the file goes something like
follows: first all your EC2 instances, followed
by your ELB constructs, then all EC2 Security
Groups and so on. No discrimination regarding
whether any of the constructs relate to app
or web servers, or certain services, or whatever
is the logical way you like to structure your
stacks.

So what can you do then? The very fun but
time-consuming process of manually going
through the generated L1 constructs and
organizing them into custom Constructs, like
a master ELB construct, or App Servers Construct,
or whatever else you need to have. From those it
is then possible to compile your stacks. Like
a Bastion stack and the App stack. Like this:

```ts
import { Construct } from "@aws-cdk/core"

export class AppServerConstruct extends Construct {
    constructor(scope: Construct, id: string) {
        super(scope, id)

        /* Your L2 and L1 constructs needed for your app servers go here */

    }
}
```

Ok, so we've got this far: we have extracted
our existing infrastructure into a generated
set of L1 constructs, given them meaning and
context well as improved developer friendliness
by turning them into L2 constructs (where possible)
and structuring them into logical parent
constructs. All that's left to do is to **stack
'em up** and deploy (preferably to a sandbox
environment first - please don't test in prod!).
We'll leave that for later (as I'm not there
either, yet).

But the important point here is:
at this point you will have your infrastructure
as logically grouped code and - provided you
assigned meaningful names and IDs - easy to
understand and handle by developers. Ideally,
you could just show the code to your random
junior dev who's familiar with your chosen
programming language, and they'd be able to
get the handle of the infrastructure code
relatively easily. At least much easier than
if your IaC was this:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
...
Resources:
...
  EC2Instance7:
    Type: "AWS::EC2::Instance"
    Properties:
      ImageId: "ami-c51e3eb6"
      InstanceType: "t2.medium"
      KeyName: "<KeyName>"
      AvailabilityZone: !GetAtt EC2Instance8.AvailabilityZone
      Tenancy: "default"
      SubnetId: !Ref EC2Subnet4
      EbsOptimized: false
      SecurityGroupIds:
        - !Ref EC2SecurityGroup7
      SourceDestCheck: true
      BlockDeviceMappings:
        -
          DeviceName: "/dev/xvda"
          Ebs:
            Encrypted: false
            VolumeSize: 100
            SnapshotId: "<snap-uuid>"
            VolumeType: "gp2"
            DeleteOnTermination: true
      UserData: "<hashed userData goes here>"
      IamInstanceProfile: !Ref IAMRole3
      Tags:
        -
          Key: "Name"
          Value: "example"
      HibernationOptions:
        Configured: false
      EnclaveOptions:
        Enabled: false
...
```

That's the same EC2 instance we've been using
as an example defined as part of a CloudFormation
template. A tiny excerpt like that is fine,
but even a simple infrastructure turns into
*more than 2000 lines of YAML* - and that is
no fun at all, if you ask me. I'd rather read
lines of declarative code in a programming
language I am - and everyone in my team is -
familiar with divided into logical parts that
I can interact with **as code**.