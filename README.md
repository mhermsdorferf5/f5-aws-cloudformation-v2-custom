# F5 BIG-IP AWS CloudFormation Templates

[![Releases](https://img.shields.io/github/release/f5networks/f5-aws-cloudformation-v2.svg)](https://github.com/f5networks/f5-aws-cloudformation-v2/releases)
[![Issues](https://img.shields.io/github/issues/f5networks/f5-aws-cloudformation-v2.svg)](https://github.com/f5networks/f5-aws-cloudformation-v2/issues)

## F5 BIG-IP AWS CloudFormation 2.0

## TLDR:

### Prep AWS Environment

```bash
export REGION=us-west-2
aws ec2 import-key-pair \
  --region ${REGION} \
  --key-name hermsdorfer-key-pair \
  --public-key-material fileb:///home/mhermsdorfer/.ssh/id_rsa.pub
```

### Encode & Create Secrets with pkcs12 file & phassphrase

```bash
export pkcs12Passphrase=`echo -en "TestSecretPhassphrase"| base64`
export pkcs12File=`base64 -w0 testCert.pfx`
aws secretsmanager create-secret \
    --name prod/hermapp/pkcs12_cert \
    --region ${REGION} \
    --description "pkcs12 file, base64 encoded & stored as aws secret." \
    --secret-string "${pkcs12File}"
aws secretsmanager create-secret \
    --name prod/hermapp/pkcs12_passphrase \
    --region ${REGION} \
    --description "pkcs12 passphrase, base64 encoded & stored as aws secret." \
    --secret-string "${pkcs12Passphrase}"
```

### Deploy CloudFormation Template:

```bash
aws cloudformation create-stack --region ${REGION} --stack-name hermTestStack \
  --template-body "`cat ./examples/autoscale/payg/autoscale.yaml`" \
  --parameters "`cat ./examples/autoscale/payg/autoscale-parameters.json`" \
  --capabilities CAPABILITY_NAMED_IAM
```


## Introduction

Welcome to the GitHub repository for F5's CloudFormation Templates v2. All of the templates in this repository have been developed by F5 Networks engineers. This repository contains one main directory: *examples*.

- **examples**<br>
  These are our next-generation Cloud Solutions Templates 2.0 (CST2), which have been designed to improve the user experience with fewer templates, simplify full-stack deployments, enable customization via a new modular nested/linked architecture, and more. The example templates in this directory have been tested and verified to work as-is and are intended to provide reference deployments of F5 BIG-IP Virtual Editions. 



## Template Information

Descriptions for each template are contained at the top of each template in the *Description* key.
For additional information, including how the templates are generated or assistance in deploying a template, see the README.md file in the individual template directory.

To get started, first checkout the /examples folder.

## Getting Help

Due to the heavy customization requirements of external cloud resources and BIG-IP configurations in these solutions, F5 does not provide technical support for deploying, customizing, or troubleshooting the templates themselves. However, the various underlying products and components used (for example: [F5 BIG-IP Virtual Edition](https://clouddocs.f5.com/cloud/public/v1/), [F5 BIG-IP Runtime Init](https://github.com/F5Networks/f5-bigip-runtime-init), [F5 Automation Toolchain](https://www.f5.com/pdf/products/automation-toolchain-overview.pdf) extensions, and [Cloud Failover Extension (CFE)](https://clouddocs.f5.com/products/extensions/f5-cloud-failover/latest/)) in the solutions located here are F5-supported and capable of being deployed with other orchestration tools. Problems found with the templates deployed as-is should be reported with a GitHub issue. Read more about [Support Policies](https://www.f5.com/company/policies/support-policies).


For help with authoring and support for custom CST2 templates, we recommend engaging F5 Professional Services (PS).


### Filing Issues

If you find an issue, we would love to hear about it.

- Use the **[Issues](https://github.com/F5Networks/f5-aws-cloudformation-v2/issues)** link on the GitHub menu bar in this repository for items such as enhancement, feature requests, and bug fixes. Tell us as much as you can about what you found and how you found it.


---


### Copyright

Copyright 2014-2023 F5 Networks Inc.

### License

#### Apache V2.0

Licensed under the Apache License, Version 2.0 (the "License"); you may not use
this file except in compliance with the License. You may obtain a copy of the
License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and limitations
under the License.

#### Contributor License Agreement

Individuals or business entities who contribute to this project must have
completed and submitted the F5 Contributor License Agreement.