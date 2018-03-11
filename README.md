# Parameter Rando

## Periodically insert a random value into AWS Systems Manager Parameter Store

Parameter Rando will periodically put a random value into an 
[AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html)
key at a configurable rate. It can be set up via the deployment of a 
[CloudFormation](https://aws.amazon.com/cloudformation/) template.

### Huh? How is this even remotely helpful?
It can be helpful for a CloudFormation stack to receive a changed value every 
time it is updated. Example use cases for this include triggering an update to a 
[custom resource](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html),
triggering a [cfn-hup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-hup.html) 
update, etc. This is usually accomplished by establishing some procedure by 
which a parameter in the stack is changed at every update (e.g. via the 
CloudFormation console).

CloudFormation stacks with parameters of type 
```AWS::SSM::Parameter::Value<String>```
will automatically resolve new values from Systems Manager Parameter Store when
a stack is updated. This feature can be utilized to automatically provide a
frequently changing value to your stack.

There may be other use cases (let me know!), but those decribed above were the
primary motivation for this project.

### Installation
1. Create a new CloudFormation with the template at cloud-formation.yml.
2. Profit.

### Usage
1. Place a parameter of type ```AWS::SSM::Parameter::Value<String>``` in your 
   CloudFormation template. This parameter should references the Parameter Rando
   key in Parameter Store (this is defined when you install Parameter Rando, 
   and defaults to ```parameter-rando```)
2. Use that parameter wherever you need a value that will frequently change.
   e.g. as a custom resource property or metadata value
3. The resolved value will automatically change every time you update your
   stack, up to the rate defined when you set up Parameter Rando (minimum of
   1 minute)
