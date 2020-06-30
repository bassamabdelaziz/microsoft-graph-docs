---
title: "Customize the profile card using the profile API in Microsoft Graph (preview)"
description: "This article describes how you can customize the profile card by making additional attributes visible, or adding custom attributes."
author: "PollyNincevic"
localization_priority: Priority
ms.prod: "users"
ms.custom: scenarios:getting-started
---

# Add additional properties to the profile card using the profile API in Microsoft Graph (preview)

On the [profile card](https://support.office.com/article/profile-cards-in-office-365-e80f931f-5fc4-4a59-ba6e-c1e35a85b501) in Microsoft 365, you can find information about users that is stored and maintained by your organization, for example **Job title** or **Office location**.

Use the [profileCardProperty] (/graph/api/resources/profilecardproperty?view=graph-rest-beta) resource to show additional properties from Azure AD on profile cards for an organization, by:

- Making additional attributes visible

- Adding custom attributes

Additional properties will display in the **Contact** section of the profile card in Microsoft 365.

Operations on the **profileCardProperty** resource that use delegated permissions requires the signed-in user to have a tenant administrator or global administrator role.

## Make additional attributes visible

You can make the following attributes from Azure Active Directory (Azure AD) visible on users' profile cards; these attributes are not case sensitive:

- `UserPrincipalName`
- `Fax`
- `StreetAddress`
- `PostalCode`
- `StateOrProvince`
- `Alias`

The following table shows how the Azure AD attributes correspond with properties of the Microsoft Graph [user](/graph/api/resources/user?view=graph-rest-beta) entity.

|Attribute string|Microsoft Graph property|
|:---------------|:----------|
|UserPrincipalName|userPrincipalName |
|Fax|faxNumber|
|StreetAddress|streetAddress|
|PostalCode|postalCode|
|StateOrProvince|state
|Alias|mailNickname

You can add any of these attributes to the profile card by configuring your tenant settings in Microsoft Graph. When you make additional attributes visible, you must use the property names for `en-us`. You don't have to add localized values. The additional properties will automatically be shown in the language settings that the user has specified for Microsoft 365.

> [!IMPORTANT]
> When adding an attribute to profile card, it takes up to 24 hours for the addition to be displayed.

## Example

An example is to display **Alias** on the profile card:

```http
        POST https://graph.microsoft.com/beta/organization/{tenantid}/settings/profileCardProperties
        Content-Type: application/json

        {
        "directoryPropertyName": "Alias"
        }
```

If successful, the response returns a `200 OK` response code and a **profileCardProperty** object in the response body. The value for the `Alias" attribute  would be displayed on a user's profile card.  

```http
HTTP/1.1 200 OK
Content-type: application/json

            {
              "directoryPropertyName": "Alias",
              "annotations": []
            }
```

## Adding custom attributes

You can add any of the [15 custom attributes](/graph/api/resources/onpremisesextensionattributes?view=graph-rest-beta) Azure AD to users' profile cards by configuring your tenant settings in Microsoft Graph. You can add one **profileCardProperty** resource at a time.

It takes up to 24 hours for the changes to show on profile cards.

Custom properties are not searchable and can't be used to search for people across Microsoft apps and services.

The following table shows how the Azure AD custom attribute names correspond to the supported values for the **directoryPropertyName** property of the [profileCardProperty] (/graph/api/resources/profilecardproperty?view=graph-rest-beta) entity; these Azure AD custom attribute names are not case sensitive.

|Attribute string|profileCardProperty value|
|:---------------|:----------|
|customAttribute1| extensionAttribute1 |
|customAttribute2| extensionAttribute2 |
|customAttribute3| extensionAttribute3 |
|customAttribute4| extensionAttribute4 |
|customAttribute5| extensionAttribute5 |
|customAttribute6| extensionAttribute6 |
|customAttribute7| extensionAttribute7 |
|customAttribute8| extensionAttribute8 |
|customAttribute9| extensionAttribute9 |
|customAttribute10| extensionAttribute10 |
|customAttribute11| extensionAttribute11 |
|customAttribute12| extensionAttribute12 |
|customAttribute13| extensionAttribute13 |
|customAttribute14| extensionAttribute14 |
|customAttribute15| extensionAttribute15 |

## Example

The following example adds the first Azure AD custom attribute to the profile card, using the display name **Cost center**. For users that have set their language settings to German-Austria, the display name will be **Kostenstelle**.

```http
POST https://graph.microsoft.com/beta/organization/{tenantid}/settings/profileCardProperties
Content-Type: application/json

        {
        "directoryPropertyName": "customAttribute1",
        "annotations": [
          {
                "displayName": "Cost center",
                "localizations": [
                    {
                        "languageTag": "de-at",
                        "displayName": "Kostenstelle"
                    }
                  ]
                }
              ]
            }
  ```

Enter the language code in the form *ll-cc*, where *ll* is the language code, and *cc* the country code. For example, for German–Austria, enter the country code de-at.
If a language is not supported, the property name will be shown with the default value.  

If successful, the response returns a `200 OK` response code and a **profileCardProperty** object in the response body. In this example you can assume that the profile card displays **Kostenstelle** for all users that have set their language settings to German-Austria on the profile card. For all other users, **Cost center** will be displayed on the profile card.

```http
HTTP/1.1 200 OK
Content-type: application/json

            {
              "directoryPropertyName": "customAttribute1",
              "annotations": [
                {
                  "displayName": "Cost center",
                  "localizations": [
                      {
                          "languageTag": "de-at",
                          "displayName": "Kostenstelle"
                      }
                    ]
                  }
                ]
              }
```

It takes up to 24 hours for the changes to show on profile cards.

> [!NOTE]
> Custom properties are not searchable and can't be used to search for people across Microsoft apps and services.

## See also

[Find your Microsoft 365 tenant ID](https://docs.microsoft.com/onedrive/find-your-office-365-tenant-id)

[onPremisesExtensionAttributes resource type](/graph/api/resources/onpremisesextensionattributes?view=graph-rest-beta))

[User resource type](/graph/api/resources/user?view=graph-rest-beta)

[Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)

[Get profileCardProperty](/graph/api/profilecardproperty-get?view=graph-rest-beta)