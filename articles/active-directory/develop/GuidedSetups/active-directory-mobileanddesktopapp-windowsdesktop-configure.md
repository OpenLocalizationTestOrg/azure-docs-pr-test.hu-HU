---
title: "aaaAzure AD v2 Windows asztali bevezetés - Config |} Microsoft Docs"
description: "A Windows asztali .NET (XAML) alkalmazások hogyan szereznie egy hozzáférési jogkivonatot, és a hívja az API-k védi az Azure Active Directory v2 végpont. | A Microsoft Azure |} A Microsoft Azure"
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
ms.openlocfilehash: 39c257e3e0cb09491f6fe005877601bd46824d12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="4206a-104">Hozzon létre egy alkalmazást (Express)</span><span class="sxs-lookup"><span data-stu-id="4206a-104">Create an application (Express)</span></span>
<span data-ttu-id="4206a-105">Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:</span><span class="sxs-lookup"><span data-stu-id="4206a-105">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="4206a-106">Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span><span class="sxs-lookup"><span data-stu-id="4206a-106">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span></span>
2.  <span data-ttu-id="4206a-107">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="4206a-107">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="4206a-108">Ellenőrizze, hogy az interaktív telepítés hello beállítás be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="4206a-108">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="4206a-109">Hajtsa végre a hello utasításokat tooobtain hello Alkalmazásazonosító, és illessze be a kódot</span><span class="sxs-lookup"><span data-stu-id="4206a-109">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="4206a-110">Adja hozzá a regisztrációs adatokat tooyour megoldás (speciális)</span><span class="sxs-lookup"><span data-stu-id="4206a-110">Add your application registration information tooyour solution (Advanced)</span></span>
<span data-ttu-id="4206a-111">Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:</span><span class="sxs-lookup"><span data-stu-id="4206a-111">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="4206a-112">Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app) tooregister alkalmazás</span><span class="sxs-lookup"><span data-stu-id="4206a-112">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="4206a-113">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="4206a-113">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="4206a-114">Győződjön meg arról, hogy az interaktív telepítés hello beállítás nincs bejelölve</span><span class="sxs-lookup"><span data-stu-id="4206a-114">Make sure hello option for Guided Setup is unchecked</span></span>
4. <span data-ttu-id="4206a-115">Kattintson a `Add Platform`, majd jelölje be `Native Application` , majd kattintson a Mentés</span><span class="sxs-lookup"><span data-stu-id="4206a-115">Click `Add Platform`, then select `Native Application` and hit Save</span></span>
5. <span data-ttu-id="4206a-116">Másoljon át hello GUID azonosítója, lépjen vissza tooVisual Studio, a nyílt `App.xaml.cs` , és cserélje le `your_client_id_here` az imént regisztrált Alkalmazásazonosító hello:</span><span class="sxs-lookup"><span data-stu-id="4206a-116">Copy hello GUID in Application ID, go back tooVisual Studio, open `App.xaml.cs` and replace `your_client_id_here` with hello Application ID you just registered:</span></span>

```csharp
private static string ClientId = "your_application_id_here";
```
