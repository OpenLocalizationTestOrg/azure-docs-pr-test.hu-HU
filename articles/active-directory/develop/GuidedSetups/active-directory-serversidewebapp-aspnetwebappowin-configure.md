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
ms.openlocfilehash: e666be4622ad30aaa1e12e49ae56bbe1e129b2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="65748-103">Hozzon létre egy alkalmazást (Express)</span><span class="sxs-lookup"><span data-stu-id="65748-103">Create an application (Express)</span></span>
<span data-ttu-id="65748-104">Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:</span><span class="sxs-lookup"><span data-stu-id="65748-104">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="65748-105">Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span><span class="sxs-lookup"><span data-stu-id="65748-105">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)</span></span>
2.  <span data-ttu-id="65748-106">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="65748-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="65748-107">Ellenőrizze, hogy az interaktív telepítés hello beállítás be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="65748-107">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="65748-108">Hajtsa végre a hello utasításokat tooadd egy átirányítási URL-cím tooyour alkalmazás</span><span class="sxs-lookup"><span data-stu-id="65748-108">Follow hello instructions tooadd a Redirect URL tooyour application</span></span>

## <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="65748-109">Adja hozzá a regisztrációs adatokat tooyour megoldás (speciális)</span><span class="sxs-lookup"><span data-stu-id="65748-109">Add your application registration information tooyour solution (Advanced)</span></span>
<span data-ttu-id="65748-110">Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:</span><span class="sxs-lookup"><span data-stu-id="65748-110">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="65748-111">Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app) tooregister alkalmazás</span><span class="sxs-lookup"><span data-stu-id="65748-111">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="65748-112">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="65748-112">Enter a name for your application and your email</span></span> 
3.  <span data-ttu-id="65748-113">Győződjön meg arról, hogy az interaktív telepítés hello beállítás nincs bejelölve</span><span class="sxs-lookup"><span data-stu-id="65748-113">Make sure hello option for Guided Setup is unchecked</span></span>
4.  <span data-ttu-id="65748-114">Kattintson a `Add Platform`, majd jelölje be`Web`</span><span class="sxs-lookup"><span data-stu-id="65748-114">Click `Add Platform`, then select `Web`</span></span>
5.  <span data-ttu-id="65748-115">Lépjen vissza tooVisual Studio, és a Megoldáskezelőben, válassza ki a hello projektet, és (Ha nem látja a Tulajdonságok ablak az F4) hello tulajdonságai ablakban tekintse meg</span><span class="sxs-lookup"><span data-stu-id="65748-115">Go back tooVisual Studio and, in Solution Explorer, select hello project and look at hello Properties window (if you don’t see a Properties window, press F4)</span></span>
6.  <span data-ttu-id="65748-116">SSL engedélyezése túl módosítása`True`</span><span class="sxs-lookup"><span data-stu-id="65748-116">Change SSL Enabled too`True`</span></span>
7.  <span data-ttu-id="65748-117">Hello SSL URL-cím másolása, és adja hozzá az URL-toohello listája az átirányítási URL-átirányítási URL-t hello regisztrációs portál közül:</span><span class="sxs-lookup"><span data-stu-id="65748-117">Copy hello SSL URL and add this URL toohello list of Redirect URLs in hello Registration Portal’s list of Redirect URLs:</span></span><br/><br/>![Projekt tulajdonságai](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  <span data-ttu-id="65748-119">Adja hozzá a hello következő `web.config` hello gyökérmappájában hello szakaszban található `configuration\appSettings`:</span><span class="sxs-lookup"><span data-stu-id="65748-119">Add hello following in `web.config` located in hello root folder under hello section `configuration\appSettings`:</span></span>

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
<span data-ttu-id="65748-120">Cserélje le `ClientId` a hello csak regisztrált alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="65748-120">Replace `ClientId` with hello Application Id you just registered</span></span>
</li>
<li>
<span data-ttu-id="65748-121">Cserélje le `redirectUri` a hello SSL URL-címet, a projekt</span><span class="sxs-lookup"><span data-stu-id="65748-121">Replace `redirectUri` with hello SSL URL of your project</span></span>
</li>
</ol>
<!-- End Docs -->
