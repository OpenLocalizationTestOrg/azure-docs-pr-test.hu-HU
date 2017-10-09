---
title: "aaaAzure AD v2 ASP.NET Web Server bevezetés - Config |} Microsoft Docs"
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
ms.openlocfilehash: badc47e131290a56a507592f944a0fc7093260a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="configure-your-aspnet-web-app-with-hello-applications-registration-information"></a><span data-ttu-id="5f722-103">Konfigurálja az ASP.NET webalkalmazás hello alkalmazás regisztrációs adatai</span><span class="sxs-lookup"><span data-stu-id="5f722-103">Configure your ASP.NET Web App with hello application's registration information</span></span>

<span data-ttu-id="5f722-104">Ebben a lépésben a projekt toouse SSL konfigurálása, és az alkalmazás regisztrációs adatokat hello SSL URL-cím tooconfigure használhatja.</span><span class="sxs-lookup"><span data-stu-id="5f722-104">In this step, you will configure your project toouse SSL, and then use hello SSL URL tooconfigure your application’s registration information.</span></span> <span data-ttu-id="5f722-105">Ezt követően hello alkalmazás hozzáadása "regisztrációs információ tooyour megoldás a *web.config*.</span><span class="sxs-lookup"><span data-stu-id="5f722-105">After this, add hello application’ registration information tooyour solution via *web.config*.</span></span>

1.  <span data-ttu-id="5f722-106">A Megoldáskezelőben, válassza ki a hello projekt, és nézze meg hello `Properties` (Ha nem látja a Tulajdonságok ablak az F4) ablak</span><span class="sxs-lookup"><span data-stu-id="5f722-106">In Solution Explorer, select hello project and look at hello `Properties` window (if you don’t see a Properties window, press F4)</span></span>
2.  <span data-ttu-id="5f722-107">Változás `SSL Enabled` túl`True`</span><span class="sxs-lookup"><span data-stu-id="5f722-107">Change `SSL Enabled` too`True`</span></span>
3.  <span data-ttu-id="5f722-108">Másolja a hello értéket `SSL URL` fent és beillesztheti hello `Redirect URL` hello oldal tetején lévő mezőbe, majd kattintson az *frissítés*:</span><span class="sxs-lookup"><span data-stu-id="5f722-108">Copy hello value from `SSL URL` above and paste it in hello `Redirect URL` field on hello top of this page, then click *Update*:</span></span><br/><br/><span data-ttu-id="5f722-109">![Projekt tulajdonságai](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span><span class="sxs-lookup"><span data-stu-id="5f722-109">![Project properties](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)</span></span><br />
4.  <span data-ttu-id="5f722-110">Adja hozzá a hello következő `web.config` szakaszban legfelső mappában található fájl `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="5f722-110">Add hello following in `web.config` file located in root’s folder, under section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="[Enter hello application Id here]" />
<add key="RedirectUri" value="[Enter hello Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
