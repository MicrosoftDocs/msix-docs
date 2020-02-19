---
title: Using custom properties for app extensions
description: Learn how to use Using Custom Properties for AppExtensions
ms.date: 02/06/2020
ms.topic: article
keywords: windows 10, msix, uwp, extensions
ms.localizationpriority: medium
---

# Using custom properties for app extensions

App extension properties are a custom metadata field provided by the app extension developer in their app manifest. App extension hosts can read this metadata when examining app extensions without having to load any content from the app extension.

Properties are an optional but very useful feature. There are a wide range of possible metadata that you may require when creating an app extension host platform, such as version, capabilities, lists of supported file types or other data that is helpful to know prior to loading an app extension. Such information may even determine how you load an app extension. Rather than trying to predict all of the fields you might need, app extension properties provide an open canvas to define exactly what you need in however manner best suited for your app.

## Advantages of app extension properties

There two important reasons why you should take advantage of app extension properties:

* It's an easy way and safe way to store basic or important metadata about your app extension platform without having to put that information into files. You can use it for important concepts like versioning, permissions, and as enforcement. For example, your app could insist that a `version` field is defined and present in the properties, and have different loading behavior based on that value.

* Because the information is stored in the app manifest, it can be indexed and made available via an API in the future. That means you can bubble it up for display to the user, or have more refined app extension searches for specific properties without having to deploy and load the extension first.

## How to declare properties

Properties are declared in the Package.appxmanifest file in your package. They can be read at runtime by the extension host as a property set. Because the host defines the platform, it is up to the host to communicate to the extension developers about which properties are available and should be put into the `AppExtension` declaration.

> [!NOTE]
> The manifest designer in Visual Studio does not support the ability to define properties. You must edit the Package.appxmanifest directly to define properties.

To declare properties, put them in the `<uap3:Properties/>` element under your `<uap3:AppExtension>` declaration. Here is a sample `<uap3:AppExtension>` declaration for Microsoft Edge that uses properties supported by Edge.

```xml
<uap3:AppExtension Name="com.microsoft.edge.extension" Id="FirstExtension" PublicFolder="Extension" DisplayName="MyExtension">
  <uap3:Properties>
    <Capabilities>
      <Capability Name="websiteContent" />
      <Capability Name="websiteInfo" />
      <Capability Name="browserWebRequest" />
      <Capability Name="browserStorage" />
    </Capabilities>
  </uap3:Properties>
</uap3:AppExtension>
```

Edge has defined a known property value of `Capabilities` with a list of extension capabilities declared. As a host, you can support whatever properties you want in your app extensions. Since these are a property set, you can also have nested properties. Its a good idea to have a root version property that you can use in case you change formats of your extensions in the future. We intentionally did not put `Version` as an attribute of app extensions so you would not be artificially confined to using our versioning semantics. Instead, we created properties where version could be one of many custom-defined attributes, in whatever way and format you want, and processed however you want.

## How to use properties

Suppose you have a simple property in an app extensions that describes a version, such as the following.

```xml
<uap3:Properties>
    <Version>1.0.0.0</Version>
</uap3:Properties>
```

To get this data at runtime, just call [GetExtensionPropertiesAsync()](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions.appextension.getextensionpropertiesasync) on the app extensions.

```csharp
string extensionVersion = "Unknown";
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;
if (properties != null)
{
    if (properties.ContainsKey("Version"))
    {
        PropertySet versionProperty = properties["Version"] as PropertySet;
        extensionVersion = versionProperty["#text"].ToString();
    }
}
```
