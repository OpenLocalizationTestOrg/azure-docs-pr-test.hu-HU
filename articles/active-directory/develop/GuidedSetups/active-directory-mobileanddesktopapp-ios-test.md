---
title: Az Azure AD v2 iOS Getting Started - teszt |} Microsoft Docs
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
ms.openlocfilehash: 4a88096d2b0a23708acdbc1798eac528599b4f71
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a><span data-ttu-id="a8b38-103">Lekérdezi a Microsoft Graph API az iOS-alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="a8b38-103">Test querying the Microsoft Graph API from your iOS application</span></span>

<span data-ttu-id="a8b38-104">Nyomja le az `Command`  +  `R` a kód-szimulátorban történő futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a8b38-104">Press `Command` + `R` to run the code in the simulator.</span></span>

![A minta képernyőkép](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

<span data-ttu-id="a8b38-106">Ha elkészült, teszteléséhez, koppintson *"Hívható meg Microsoft Graph API"* és a felhasználónév és a jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="a8b38-106">When you're ready to test, tap *‘Call Microsoft Graph API’* and you will be prompted to type your username and password.</span></span>

### <a name="consent"></a><span data-ttu-id="a8b38-107">Hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="a8b38-107">Consent</span></span>
<span data-ttu-id="a8b38-108">Az első alkalommal bejelentkezik az alkalmazás választhat hasonló hozzájárulási képernyő a alatt, ahol kell explicit módon fogadja el:</span><span class="sxs-lookup"><span data-stu-id="a8b38-108">The first time you sign in to your application, you will be presented with a consent screen similar to the below, where you need to explicitly accept:</span></span>

![Hozzájárulás képernyő](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="a8b38-110">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="a8b38-110">Expected results</span></span>
<span data-ttu-id="a8b38-111">Felhasználói profil által visszaadott adatok a Microsoft Graph API-hívás kell megjelennie a *naplózás* szakasz.</span><span class="sxs-lookup"><span data-stu-id="a8b38-111">You should see user profile information returned by the Microsoft Graph API call in the *Logging* section.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="a8b38-112">További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették</span><span class="sxs-lookup"><span data-stu-id="a8b38-112">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="a8b38-113">A Microsoft Graph API megköveteli a `user.read` hatókörrel, hogy a felhasználói profil olvasása.</span><span class="sxs-lookup"><span data-stu-id="a8b38-113">The Microsoft Graph API requires the `user.read` scope to read the user's profile.</span></span> <span data-ttu-id="a8b38-114">Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="a8b38-114">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="a8b38-115">Néhány más Microsoft Graph API, továbbá egyéni API-k, a háttér-kiszolgáló további hatókörökkel lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="a8b38-115">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="a8b38-116">Például a Microsoft Graph-hatóköre `Calendars.Read` listát a felhasználók naptáraiban szükséges.</span><span class="sxs-lookup"><span data-stu-id="a8b38-116">For example, for Microsoft Graph, the scope `Calendars.Read` is required to list the user’s calendars.</span></span> <span data-ttu-id="a8b38-117">A felhasználó naptár kontextusban az alkalmazás eléréséhez, hozzá kell adnia a `Calendars.Read` jogosultságot a az alkalmazás regisztrációs adatait, majd adja hozzá a `Calendars.Read` a hatókör a `acquireTokenSilent` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="a8b38-117">In order to access the user’s calendar in a context of an application, you need to add the `Calendars.Read` delegated permission to the application registration’s information and then add the `Calendars.Read` scope to the `acquireTokenSilent` call.</span></span> <span data-ttu-id="a8b38-118">A felhasználó kérheti további hozzájárulásokat azoktól a hatókörök számának növelésével.</span><span class="sxs-lookup"><span data-stu-id="a8b38-118">The user may be prompted for additional consents as you increase the number of scopes.</span></span>

<!--end-collapse-->



