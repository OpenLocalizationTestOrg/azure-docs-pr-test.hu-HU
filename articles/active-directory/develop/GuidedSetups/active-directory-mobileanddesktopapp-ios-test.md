---
title: aaaAzure AD v2 iOS Getting Started - teszt |} Microsoft Docs
description: "Hogyan iOS (Swift) alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 98c73eddabf9664feb19ac6878e9d7315b9aa79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-querying-hello-microsoft-graph-api-from-your-ios-application"></a><span data-ttu-id="19abc-103">Az iOS-alkalmazásból lekérdező hello Microsoft Graph API tesztelése</span><span class="sxs-lookup"><span data-stu-id="19abc-103">Test querying hello Microsoft Graph API from your iOS application</span></span>

<span data-ttu-id="19abc-104">Nyomja le az `Command`  +  `R` toorun hello kód hello szimulátor.</span><span class="sxs-lookup"><span data-stu-id="19abc-104">Press `Command` + `R` toorun hello code in hello simulator.</span></span>

![A minta képernyőkép](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

<span data-ttu-id="19abc-106">Amikor készen áll a tootest, koppintson *"Hívható meg Microsoft Graph API"* és rákérdezéses tootype lesz a felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="19abc-106">When you're ready tootest, tap *‘Call Microsoft Graph API’* and you will be prompted tootype your username and password.</span></span>

### <a name="consent"></a><span data-ttu-id="19abc-107">Hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="19abc-107">Consent</span></span>
<span data-ttu-id="19abc-108">hello első bejelentkezéskor tooyour alkalmazást, akkor számára jelenik meg a hozzájárulási képernyő hasonló toohello az alábbi, amennyiben szükséges tooexplicitly, fogadja el:</span><span class="sxs-lookup"><span data-stu-id="19abc-108">hello first time you sign in tooyour application, you will be presented with a consent screen similar toohello below, where you need tooexplicitly accept:</span></span>

![Hozzájárulás képernyő](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="19abc-110">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="19abc-110">Expected results</span></span>
<span data-ttu-id="19abc-111">Felhasználói profil adatait a hello hello Microsoft Graph API-hívás által visszaadott kell megjelennie *naplózás* szakasz.</span><span class="sxs-lookup"><span data-stu-id="19abc-111">You should see user profile information returned by hello Microsoft Graph API call in hello *Logging* section.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="19abc-112">További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették</span><span class="sxs-lookup"><span data-stu-id="19abc-112">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="19abc-113">hello Microsoft Graph API szükséges hello `user.read` tooread hello profil hatókörének.</span><span class="sxs-lookup"><span data-stu-id="19abc-113">hello Microsoft Graph API requires hello `user.read` scope tooread hello user's profile.</span></span> <span data-ttu-id="19abc-114">Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="19abc-114">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="19abc-115">Néhány más Microsoft Graph API, továbbá egyéni API-k, a háttér-kiszolgáló további hatókörökkel lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="19abc-115">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="19abc-116">Például a Microsoft Graph-hatókör hello `Calendars.Read` van szükség toolist hello felhasználók naptáraiban.</span><span class="sxs-lookup"><span data-stu-id="19abc-116">For example, for Microsoft Graph, hello scope `Calendars.Read` is required toolist hello user’s calendars.</span></span> <span data-ttu-id="19abc-117">Sorrendben tooaccess hello kontextusban az alkalmazás felhasználó naptár, tooadd hello kell `Calendars.Read` delegált engedéllyel toohello alkalmazás regisztrációs adatait, és adja meg az hello `Calendars.Read` hatókör toohello `acquireTokenSilent` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="19abc-117">In order tooaccess hello user’s calendar in a context of an application, you need tooadd hello `Calendars.Read` delegated permission toohello application registration’s information and then add hello `Calendars.Read` scope toohello `acquireTokenSilent` call.</span></span> <span data-ttu-id="19abc-118">hello felhasználói további hozzájárulásokat azoktól hello hatókörök számának növelésével kérheti.</span><span class="sxs-lookup"><span data-stu-id="19abc-118">hello user may be prompted for additional consents as you increase hello number of scopes.</span></span>

<!--end-collapse-->



