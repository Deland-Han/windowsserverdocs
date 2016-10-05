
---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Add Home Link 
description:
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/31/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.service: active-directory
ms.technology: identity-adfs
---
# Add Home Link 

>Applies To: Windows Server 2016, Windows Server 2012 R2

To add the home link that is displayed on the sign\-in page, use the following Windows PowerShell cmdlet and syntax. 


![](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> The `linkText` parameter in this cmdlet is not required unless you use another value than the default, which is *Home*. The advantage of using the default is that they are localized to all client locales. after the sign\-in page is customized, the customization takes precedence; therefore, you should customize for all languages that you want to support.

## Additional references 
[AD FS User Sign-in Customization](AD-FS-user-sign-in-customization.md)  