---
title: "Az Azure AD v2 Android első lépések – tesztelése |} Microsoft Docs"
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
ms.openlocfilehash: 6df64f4820f8409bd8897d5ac24f81bffeeef102
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a><span data-ttu-id="7cc19-103">Tesztelheti a kódját</span><span class="sxs-lookup"><span data-stu-id="7cc19-103">Test your code</span></span>

1. <span data-ttu-id="7cc19-104">Az eszköz/emulátor telepíteni a kódot.</span><span class="sxs-lookup"><span data-stu-id="7cc19-104">Deploy your code to your device/emulator.</span></span>
2. <span data-ttu-id="7cc19-105">Ha elkészült, teszteléséhez, a Microsoft Azure Active Directory (szervezeti fiók) vagy a Microsoft Account (live.com, outlook.com) fiókkal való bejelentkezéshez használt.</span><span class="sxs-lookup"><span data-stu-id="7cc19-105">When you're ready to test, use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account to sign in.</span></span> 

<span data-ttu-id="7cc19-106">![A minta képernyőkép](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span><span class="sxs-lookup"><span data-stu-id="7cc19-106">![Sample screen shot](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span></span><br/><br/><span data-ttu-id="7cc19-107">
![Bejelentkezés](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span><span class="sxs-lookup"><span data-stu-id="7cc19-107">
![Sign-in](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span></span>

### <a name="consent"></a><span data-ttu-id="7cc19-108">Hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="7cc19-108">Consent</span></span>
<span data-ttu-id="7cc19-109">Az első alkalommal a felhasználó bejelentkezik az alkalmazáshoz fog jelenni számukra hasonló hozzájárulási képernyő a alatt, ahol azok kell explicit módon fogadja el:</span><span class="sxs-lookup"><span data-stu-id="7cc19-109">The first time a user signs in to your application, they will be presented with a consent screen similar to the below, where they need to explicitly accept:</span></span> 

![Hozzájárulás](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a><span data-ttu-id="7cc19-111">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="7cc19-111">Expected results</span></span>
<span data-ttu-id="7cc19-112">Az eredmények Microsoft Graph API hívása a "me" kell megtekintéséhez az beszerzése a felhasználói profil – https://graph.microsoft.com/v1.0/me használt végpontnak.</span><span class="sxs-lookup"><span data-stu-id="7cc19-112">You should see the results of a call to Microsoft Graph API ‘me’ endpoint used to to obtain the user profile - https://graph.microsoft.com/v1.0/me.</span></span> <span data-ttu-id="7cc19-113">Általános Microsoft Graph-végpontok listáját lásd: a [cikk](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span><span class="sxs-lookup"><span data-stu-id="7cc19-113">For a list of common Microsoft Graph endpoints, please see this [article](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="7cc19-114">További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették</span><span class="sxs-lookup"><span data-stu-id="7cc19-114">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="7cc19-115">A Microsoft Graph API megköveteli a `user.read` hatókörrel, hogy a felhasználói profil olvasása.</span><span class="sxs-lookup"><span data-stu-id="7cc19-115">The Microsoft Graph API requires the `user.read` scope to read the user's profile.</span></span> <span data-ttu-id="7cc19-116">Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="7cc19-116">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="7cc19-117">Néhány más Microsoft Graph API, továbbá egyéni API-k, a háttér-kiszolgáló további hatókörökkel lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="7cc19-117">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="7cc19-118">Például a Microsoft Graph-hatóköre `Calendars.Read` listát a felhasználók naptáraiban szükséges.</span><span class="sxs-lookup"><span data-stu-id="7cc19-118">For example, for Microsoft Graph, the scope `Calendars.Read` is required to list the user’s calendars.</span></span> <span data-ttu-id="7cc19-119">A felhasználó naptár kontextusban az alkalmazás eléréséhez, hozzá kell adnia a `Calendars.Read` jogosultságot a az alkalmazás regisztrációs adatait, majd adja hozzá a `Calendars.Read` a hatókör a `acquireTokenSilentAsync` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="7cc19-119">In order to access the user’s calendar in a context of an application, you need to add the `Calendars.Read` delegated permission to the application registration’s information and then add the `Calendars.Read` scope to the `acquireTokenSilentAsync` call.</span></span> <span data-ttu-id="7cc19-120">A felhasználó kérheti további hozzájárulásokat azoktól a hatókörök számának növelésével.</span><span class="sxs-lookup"><span data-stu-id="7cc19-120">The user may be prompted for additional consents as you increase the number of scopes.</span></span>

<!--end-collapse-->
