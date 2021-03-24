---
layout: classic-docs
title: "Arm resources"
short-title: "Using Arm resources on CircleCI"
description: "Using Arm resources on CircleCI"
version:
- Cloud
---

# Overview

This document will walk you through the setup steps required to use an Arm
resource on CircleCI. Arm resources are not available on CircleCI Server 1.x or
2.x. 

<div class="alert alert-info" role="alert">
  <b>Note:</b> If you are interested in using Arm resources, fill out this <a href="https://circleci.com/contact-us"><b>contact form</b></a>.
</div>

CircleCI offers multiple kinds of environments for you to run jobs in. In your
CircleCI `config.yml` file you can choose the right environment for your job using the
[`resource_class`]({{site.baseurl}}/2.0/configuration-reference/#resource_class)
key. CircleCI offers two Arm resources as part of the [`machine` executor]({{site.baseurl}}/2.0/configuration-reference/#machine-executor-linux):

* `arm.medium` - `arm64` architecture, 2 vCPU, 8GB RAM
* `arm.large` - `arm64` architecture, 4 vCPU, 16GB RAM

Which are available under these images:

* `ubuntu-2004:202101-01` - most recent, recommended for all users
* `ubuntu-2004:202011-01` - deprecated as of Feb 3, 2021

As these are `machine` executor resources, each class is a dedicated VM that is created specifically for your job and subsequently taken down after the job has finished running.

## Pricing and availability

The following Arm resource class is available to all CircleCI customers:

| Resource class name | Specs             | Requisite Plan                   |
|---------------------|-------------------|----------------------------------|
| `arm.medium`        | 2 vCPUs, 8GB RAM  | Free, Performance, Scale, Custom |
| `arm.large`         | 4 vCPUs, 16GB RAM | Performance, Scale               |
{: class="table table-striped"}

For pricing and availability check out our [Pricing](https://circleci.com/pricing/) page.

At this moment, Arm resources are only available on our cloud offering. If you
are a CircleCI Server customer and are looking to try Arm resources, consider
creating a CircleCI Cloud account, or contact your Customer Success Manager to
request Arm on Server.

## Using Arm resources

Update your `.circleci/config.yml` file to use Arm resources. Consider the example config:

```yaml
# .circleci/config.yml
version: 2.1

jobs:
  build-medium:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - run: uname -a
      - run: echo "Hello, Arm!"

  build-large:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.large
    steps:
      - run: uname -a
      - run: echo "Hello, Arm!"

workflows:
  build:
    jobs:
      - build-medium
      - build-large
```

Please note that it is indeed possible to mix various resources in the same
configuration (and even the same workflow).

## Limitations

* Some orbs that include an executable may **not** be compatible with Arm at
  this moment. If you run into issues with orbs on Arm, please [open an
  issue](https://github.com/CircleCI-Public/arm-preview-docs/issues).
* We currently do not provide support for 32-bit Arm architectures. Only 64-bit
  `arm64` architectures are supported at this time.
* There may be up to 2 minutes of spin-up time before your job actually starts
  running. This time will decrease as more customers start using Arm resources.
* If there is software you require that is not available in the image, please
  [open an issue](https://github.com/CircleCI-Public/arm-preview-docs/issues) to
  let us know.

## Arm FAQs 

This section answers frequently asked questions regarding CircleCI Arm support.

toc::[]

=== How do I get access to the Arm preview?

To receive access to the CircleCI Arm preview, complete the [Arm Program Request Form](https://form.asana.com/?k=S8EKGU3o66ld_qYXsdOQww&d=5374345383152).

=== What Arm architectures are currently supported?

CircleCI currently only supports Arm 64-bit architectures.

=== What Arm resources are available in the CircleCI preview?

There are two Arm resources available:

* arm.medium - arm64 architecture, 2 vCPU, 8GB RAM
* arm.large - arm64 architecture, 4 vCPU, 16GB RAM

=== Can I use an 'arm.large' resource if I am on the Free Plan?

No, you must be on a Performance or Scale plan in order to use an 'arm.large' resource. Contact your CircleCI Support Representative for information on how to sign up for a Performance or Scale Plan.

=== Can I use orbs with Arm?

Yes, you can use orbs with Arm, with the exception of Orbs that have executables.