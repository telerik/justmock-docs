---
title: License Activation Errors and Warnings
page_title: License Activation Errors and Warnings | JustMock Documentation
description: Learn what can cause an invalid license for Telerik JustMock, learn what are the common warnings and errors, and learn how to solve them.
slug: justmock/licensing/license-errors-warnings
tags: license, activate, download, error, warning
position: 4
---

# License Activation Errors and Warnings

Starting with the Q2 2025 release, Telerik JustMock will display specific warnings and errors if used without a valid license or with an invalid one. This article explains what defines an invalid license, the possible reasons behind it, and the related warnings and errors you might encounter.

## Invalid License

An invalid license can be caused by any of the following:

- Using an expired subscription license—subscription licenses expire at the end of the subscription term.
- Using a perpetual license for product versions released outside the validity period of your license.
- Using an expired trial license.
- A missing license for Telerik JustMock.
- Not installing a license key in your application.
- Not updating the license key after renewing your Telerik JustMock license.

## License Warnings and Errors

When working on a project with Telerik JustMock and encountering an expired or missing license, the test execution will display the following errors:

| Error Message                                   | Message Code | Solution                                                              |
|-------------------------------------------------|--------------|-----------------------------------------------------------------------|
| `Unable to locate licenses for all products` | TKL004 | Your license is not valid for all Telerik and Kendo products added to your project. If you have already purchased the required license, then [update your license key]({%slug justmock/licensing/set-up-your-license%}#updating-your-license-key). |
| `Your subscription has expired.` | TKL103; TKL104 | Renew your subscription and [download a new license key]({%slug justmock/licensing/set-up-your-license%}#downloading-the-license-key). |
| `Your current license has expired.` | TKL102 | You are using a product version released outside the validity period of your perpetual license. To resolve this issue, you can either: |
|                                        | | - Renew your license, then download a new license key and use it to activate the controls. |
|                                        | | - Downgrade to a product version included in your perpetual license as indicated in the message. |
| `Your trial expired.`        | TKL105 | Purchase a commercial license to continue using the product. |
| `Telerik JustMock is not listed in your current license file.` | TKL101 | Review the purchase options for the listed products. |

## See Also

* [Setting Up Your License Key]({%slug justmock/licensing/set-up-your-license%})
* [Adding the License Key to CI Services]({%slug justmock/licensing/add-license-to-ci-cd%})
* [Frequently Asked Questions about Your Telerik JustMock License Key]({%slug justmock/licensing/licensing-faq%})
