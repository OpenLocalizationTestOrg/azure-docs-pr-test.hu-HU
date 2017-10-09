---
title: "aaaAzure AD v2 Android bevezetés - teszt |} Microsoft Docs"
description: "Android-alkalmazás hogyan szereznie egy hozzáférési jogkivonatot és hívható meg Microsoft Graph API-val vagy a hozzáférési jogkivonatok az Azure Active Directory v2 végpont igénylő API-k"
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
ms.openlocfilehash: 499f32b46fd44cca0e52179bced49b311135d8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="4a10d-103">Tesztelheti a kódját</span><span class="sxs-lookup"><span data-stu-id="4a10d-103">Test your code</span></span>

1. <span data-ttu-id="4a10d-104">A kód tooyour/emulátor eszköz telepítése.</span><span class="sxs-lookup"><span data-stu-id="4a10d-104">Deploy your code tooyour device/emulator.</span></span>
2. <span data-ttu-id="4a10d-105">Amikor készen áll a tootest, használja a Microsoft Azure Active Directory (szervezeti fiók) vagy a Microsoft Account (live.com, outlook.com) fiók toosign a.</span><span class="sxs-lookup"><span data-stu-id="4a10d-105">When you're ready tootest, use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account toosign in.</span></span> 

<span data-ttu-id="4a10d-106">![A minta képernyőkép](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span><span class="sxs-lookup"><span data-stu-id="4a10d-106">![Sample screen shot](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span></span><br/><br/><span data-ttu-id="4a10d-107">
![Bejelentkezés](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span><span class="sxs-lookup"><span data-stu-id="4a10d-107">
![Sign-in](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span></span>

### <a name="consent"></a><span data-ttu-id="4a10d-108">Hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="4a10d-108">Consent</span></span>
<span data-ttu-id="4a10d-109">hello első alkalommal a felhasználó bejelentkezik tooyour alkalmazás, azok számára jelenik meg a hozzájárulási képernyő hasonló toohello az alábbi, ahol tooexplicitly, fogadja el:</span><span class="sxs-lookup"><span data-stu-id="4a10d-109">hello first time a user signs in tooyour application, they will be presented with a consent screen similar toohello below, where they need tooexplicitly accept:</span></span> 

![Hozzájárulás](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a><span data-ttu-id="4a10d-111">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="4a10d-111">Expected results</span></span>
<span data-ttu-id="4a10d-112">Megjelenik egy hívás tooMicrosoft Graph API hello eredménye "me" végpont használt tootooobtain hello felhasználói profil – https://graph.microsoft.com/v1.0/me.</span><span class="sxs-lookup"><span data-stu-id="4a10d-112">You should see hello results of a call tooMicrosoft Graph API ‘me’ endpoint used tootooobtain hello user profile - https://graph.microsoft.com/v1.0/me.</span></span> <span data-ttu-id="4a10d-113">Általános Microsoft Graph-végpontok listáját lásd: a [cikk](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span><span class="sxs-lookup"><span data-stu-id="4a10d-113">For a list of common Microsoft Graph endpoints, please see this [article](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="4a10d-114">További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették</span><span class="sxs-lookup"><span data-stu-id="4a10d-114">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="4a10d-115">hello Microsoft Graph API szükséges hello `user.read` tooread hello profil hatókörének.</span><span class="sxs-lookup"><span data-stu-id="4a10d-115">hello Microsoft Graph API requires hello `user.read` scope tooread hello user's profile.</span></span> <span data-ttu-id="4a10d-116">Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="4a10d-116">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="4a10d-117">Néhány más Microsoft Graph API, továbbá egyéni API-k, a háttér-kiszolgáló további hatókörökkel lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="4a10d-117">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="4a10d-118">Például a Microsoft Graph-hatókör hello `Calendars.Read` van szükség toolist hello felhasználók naptáraiban.</span><span class="sxs-lookup"><span data-stu-id="4a10d-118">For example, for Microsoft Graph, hello scope `Calendars.Read` is required toolist hello user’s calendars.</span></span> <span data-ttu-id="4a10d-119">Sorrendben tooaccess hello kontextusban az alkalmazás felhasználó naptár, tooadd hello kell `Calendars.Read` delegált engedéllyel toohello alkalmazás regisztrációs adatait, és adja meg az hello `Calendars.Read` hatókör toohello `acquireTokenSilentAsync` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="4a10d-119">In order tooaccess hello user’s calendar in a context of an application, you need tooadd hello `Calendars.Read` delegated permission toohello application registration’s information and then add hello `Calendars.Read` scope toohello `acquireTokenSilentAsync` call.</span></span> <span data-ttu-id="4a10d-120">hello felhasználói további hozzájárulásokat azoktól hello hatókörök számának növelésével kérheti.</span><span class="sxs-lookup"><span data-stu-id="4a10d-120">hello user may be prompted for additional consents as you increase hello number of scopes.</span></span>

<!--end-collapse-->
