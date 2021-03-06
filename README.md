<p align="center">
  <img src=".github/images/qq-icon-md.png">
</p>

[![Build status][circleci-badge]][circleci-badge-url]
[![Code style][prettier-badge]][prettier-badge-url]

# Odin

Odin, or Wōtan, is a Norse god who directs the valkyries and is the guardian of Valhalla. Half of those who perish in combat are welcomed by him into his majestic halls where they prepare for the final battle against Fenrir during the events of Ragnarök.

# What it does

This serverless application periodically checks the status of CloudFormation stacks in your AWS environment and sends the stale ones to Valhalla.

Odin currently determines that a stack is stale and should be removed when it doesn't fit any of the following three criteria:

1.  **Tagging:** If a stack is tagged with the stage 'Production' or 'Automation' (or either's derivatives, like 'Prod' or 'Auto') then it is ineligible for deletion
1.  **Status:** If a stack's status is in a failure or in-progress state then it is ineligible for deletion
1.  **Age:** If a stack has been updated recently then it is ineligible for deletion

If a stack doesn't match one of the above criteria then it is removed.

# How to use it

Odin is built with the [Serverless Framework](https://serverless.com/) - see their documentation for more about the tool and how to use it.

To change the frequency with which Odin runs and the settings used to determine whether a stack is eligible for deletion modify the values in [odin.yml](https://github.com/manwaring/odin/blob/master/odin.yml) and redeploy the application.

# Limitations

Unlike his Norse namesake Odin is unable to practice magic and cannot remove stacks that aren't self-removing.

A common example of this situation is a stack which creates an S3 bucket that is later filled with files. If the stack doesn't contain the resources necessary to empty and then delete this bucket any stack delete commands (whether triggered by Odin, manually, or any other means) will end with the stack in an error state. Don't treat your stacks this way - let them enter Valhalla with honor!

For this specific scenario you can use a tool like the `empty-s3-bucket` service found in [Lambda Utilities](https://github.com/manwaring/lambda-utilities) in combination with a custom CloudFormation resource to empty buckets when a stack is deleted. See the infrastructure section of [Santa Swap UI](https://github.com/santaswap/ui) for a real-world example of how this can be achieved within a stack.

Another approach is to let Odin empty your S3 buckets for you prior to calling the delete stack command. This approach requires you to output the bucket name in your CloudFormation stack, and then add the output key name to [odin.yml](https://github.com/manwaring/odin/blob/master/odin.yml). By default Odin will empty the deployment buckets created by the [Serverless Framework](https://serverless.com/).

# Architecture overview

![odin - architecture overview](https://cloud.githubusercontent.com/assets/2955468/24622720/f24c75a4-1873-11e7-9e09-b83a1425c196.png)

<!-- Badge links -->

[circleci-badge]: https://circleci.com/gh/Argo-DigitalVentures/odin/tree/master.svg?style=shield&circle-token=34af583571b13525df7f6b7d5481d29bd17225aa
[circleci-badge-url]: https://circleci.com/gh/Argo-DigitalVentures/odin
[prettier-badge]: https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square
[prettier-badge-url]: #badge
