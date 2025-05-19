---
title: Adding Your License Key to CI Services
page_title: Adding Your License Key to CI Services | JustMock Documentation
description: Learn how to activate the Telerik JustMock by downloading and setting up your Telerik components license key for use in CI/CD environments.
slug: justmock/licensing/add-license-to-ci-cd
tags: license, activate, download, ci, cd, environment
position: 2
---

# Adding Your License Key to CI/CD Services

This article describes how to set up and activate your Telerik JustMock [license key]({%slug justmock/licensing/set-up-your-license%}) across a few popular CI/CD services by using environment variables.

The license activation process in a CI/CD environment involves the following steps:

1. [Download]({%slug justmock/licensing/set-up-your-license%}) a license key from your Telerik account.

1. [Create an environment variable](#creating-an-environment-variable) named `TELERIK_LICENSE` and add your Telerik JustMock license key as a value.

## Creating an Environment Variable

The recommended approach for providing your license key is to use environment variables. Each CI/CD platform has a different process for setting environment variables and this article lists only some of the most popular examples.

### Azure Pipelines

1. Create a new [user-defined variable](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#user-defined-variables) named `TELERIK_LICENSE`.

1. Paste the contents of the license key file as a value.

### GitHub Actions

1. Create a new [Repository Secret](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository) or an [Organization Secret](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-an-organization).

1. Set the name of the secret to `TELERIK_LICENSE` and paste the contents of the license file as a value.

1. Update the build step that executes the tests. For example, in a GitHub Actions workflow, you can include the following step in your YAML configuration:

{% raw %}
```yaml
- name: Run Tests with Telerik JustMock
    env:
        TELERIK_LICENSE: ${{ secrets.TELERIK_LICENSE }}
    run: |
        # Your test execution command goes here
```
{% endraw %}

## See Also

* [Setting Up Your License Key]({%slug justmock/licensing/set-up-your-license%})
* [License Activation Errors and Warnings]({%slug justmock/licensing/license-errors-warnings%})
* [Frequently Asked Questions about Your Telerik JustMock License Key]({%slug justmock/licensing/licensing-faq%})
