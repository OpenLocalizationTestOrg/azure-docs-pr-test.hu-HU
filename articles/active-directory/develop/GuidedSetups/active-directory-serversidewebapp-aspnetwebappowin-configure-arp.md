---
title: "Az Azure AD v2 ASP.NET webalkalmazás-kiszolgáló kezdeti lépések - Config |} Microsoft Docs"
description: "A Microsoft bejelentkezés végrehajtási egy ASP.NET-megoldás a hagyományos böngészőalapú webalkalmazás a szabványos OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 8a1650a65e7980f4a13fa4edc7918b0099bb5464
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a><span data-ttu-id="97f94-103">Az alkalmazás regisztrációs adatokat az ASP.NET webalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="97f94-103">Configure your ASP.NET Web App with the application's registration information</span></span>

<span data-ttu-id="97f94-104">Ebben a lépésben konfigurálása az SSL használatát a projekthez, és az SSL URL-cím segítségével konfigurálhatja az alkalmazás regisztrációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="97f94-104">In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information.</span></span> <span data-ttu-id="97f94-105">Ezt követően az alkalmazás hozzáadása "a megoldás a adatainak *web.config*.</span><span class="sxs-lookup"><span data-stu-id="97f94-105">After this, add the application’ registration information to your solution via *web.config*.</span></span>

1.  <span data-ttu-id="97f94-106">A Megoldáskezelőben, válassza ki a projektet, és tekintse meg a `Properties` (Ha nem látja a Tulajdonságok ablak az F4) ablak</span><span class="sxs-lookup"><span data-stu-id="97f94-106">In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)</span></span>
2.  <span data-ttu-id="97f94-107">Változás `SSL Enabled` számára`True`</span><span class="sxs-lookup"><span data-stu-id="97f94-107">Change `SSL Enabled` to `True`</span></span>
3.  <span data-ttu-id="97f94-108">Másolja az értéket `SSL URL` fent és illessze be a `Redirect URL` Ez a lap tetején mezőben, majd kattintson az *frissítés*:</span><span class="sxs-lookup"><span data-stu-id="97f94-108">Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:</span></span><br/><br/><span data-ttu-id="97f94-109">![Projekt tulajdonságai](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span><span class="sxs-lookup"><span data-stu-id="97f94-109">![Project properties](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span></span><br />
4.  <span data-ttu-id="97f94-110">Adja hozzá a következő `web.config` szakaszban legfelső mappában található fájl `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="97f94-110">Add the following in `web.config` file located in root’s folder, under section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
