---
title: "aaaConfigure hitelesítési és engedélyezési egy egyéni alkalmazás-t meghívó hello Azure idő adatsorozat Hirdetéselemző API-t |} Microsoft Docs"
description: "Ez az oktatóanyag azt ismerteti, hogyan tooconfigure hitelesítési és engedélyezési egy egyéni alkalmazás-t meghívó hello Azure idő adatsorozat Hirdetéselemző API-t"
keywords: 
services: time-series-insights
documentationcenter: 
author: dmdenmsft
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/24/2017
ms.author: dmden
ms.openlocfilehash: 5043468bfc2af3c0d27e8602508d92ba2848409e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a><span data-ttu-id="c2b50-103">Hitelesítési és engedélyezési Azure idő adatsorozat Insights API-hoz.</span><span class="sxs-lookup"><span data-stu-id="c2b50-103">Authentication and authorization for Azure Time Series Insights API</span></span>

<span data-ttu-id="c2b50-104">Ez a cikk azt ismerteti, hogyan tooconfigure egy egyéni alkalmazást, amely behívja hello Azure idő adatsorozat Hirdetéselemző API-t.</span><span class="sxs-lookup"><span data-stu-id="c2b50-104">This article explains how tooconfigure a custom application that calls hello Azure Time Series Insights API.</span></span>

## <a name="service-principal"></a><span data-ttu-id="c2b50-105">Egyszerű szolgáltatásnév</span><span class="sxs-lookup"><span data-stu-id="c2b50-105">Service principal</span></span>

<span data-ttu-id="c2b50-106">Ez a szakasz ismerteti, hogyan tooconfigure egy alkalmazás tooaccess hello hello alkalmazás nevében idő adatsorozat Hirdetéselemző API-t.</span><span class="sxs-lookup"><span data-stu-id="c2b50-106">This section explains how tooconfigure an application tooaccess hello Time Series Insights API on behalf of hello application.</span></span> <span data-ttu-id="c2b50-107">hello alkalmazás majd kérdezhet le adatokat, vagy hello idő adatsorozat Insights környezetben alkalmazás hitelesítő adatait, és nem a hello felhasználói hitelesítő adatokat a referenciaadatok közzététele.</span><span class="sxs-lookup"><span data-stu-id="c2b50-107">hello application can then query data or publish reference data in hello Time Series Insights environment with application credentials and not hello user credentials.</span></span>

<span data-ttu-id="c2b50-108">Ha egy alkalmazás, amelyet a tooaccess idő adatsorozat Insights, állítson be egy Azure Active Directory-alkalmazás, és rendelje hozzá a hello adat-hozzáférési házirendjeit hello idő adatsorozat Insights környezetben.</span><span class="sxs-lookup"><span data-stu-id="c2b50-108">When you have an application that needs tooaccess Time Series Insights, you must set up an Azure Active Directory application and assign hello data access policies in hello Time Series Insights environment.</span></span> <span data-ttu-id="c2b50-109">Ez a módszer előnyösebb toorunning hello alkalmazás a saját credentials azért, mert:</span><span class="sxs-lookup"><span data-stu-id="c2b50-109">This approach is preferable toorunning hello app under your own credentials because:</span></span>

* <span data-ttu-id="c2b50-110">Engedélyeket rendelhet toohello identitását, amelyek eltérnek a saját engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="c2b50-110">You can assign permissions toohello app identity that are different from your own permissions.</span></span> <span data-ttu-id="c2b50-111">Ezek az engedélyek általában korlátozott tooexactly milyen hello alkalmazásnak kell toodo.</span><span class="sxs-lookup"><span data-stu-id="c2b50-111">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span> <span data-ttu-id="c2b50-112">Engedélyezheti például hello app tooonly egy adott idő adatsorozat Insights környezetben adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="c2b50-112">For example, you can allow hello app tooonly read data in a particular Time Series Insights environment.</span></span>
* <span data-ttu-id="c2b50-113">Ha módosítja az Ön feladatkörei nincs toochange hello alkalmazás hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c2b50-113">You don't have toochange hello app's credentials if your responsibilities change.</span></span>
* <span data-ttu-id="c2b50-114">Amikor egy felügyelet nélküli parancsfájllal futtatja használhatja a tanúsítványt, vagy az alkalmazás fő tooautomate hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="c2b50-114">You can use a certificate or an application key tooautomate authentication when you're running an unattended script.</span></span>

<span data-ttu-id="c2b50-115">Ez a cikk bemutatja, hogyan azokat lépésről-lépésre tooperform hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c2b50-115">This article shows you how tooperform those steps through hello Azure portal.</span></span> <span data-ttu-id="c2b50-116">A single-bérlői alkalmazások hello alkalmazás esetén csak egy szervezet tervezett toorun összpontosít.</span><span class="sxs-lookup"><span data-stu-id="c2b50-116">It focuses on a single-tenant application where hello application is intended toorun in only one organization.</span></span> <span data-ttu-id="c2b50-117">Single-bérlő alkalmazásokat a szervezet futó üzleti alkalmazásokhoz általában használ.</span><span class="sxs-lookup"><span data-stu-id="c2b50-117">You typically use single-tenant applications for line-of-business applications that run in your organization.</span></span>

<span data-ttu-id="c2b50-118">hello telepítő folyamat három magas szintű lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="c2b50-118">hello setup flow consists of three high-level steps:</span></span>

1. <span data-ttu-id="c2b50-119">Alkalmazás létrehozása az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="c2b50-119">Create an application in Azure Active Directory.</span></span>
2. <span data-ttu-id="c2b50-120">Az alkalmazás tooaccess hello idő adatsorozat Insights környezet engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="c2b50-120">Authorize this application tooaccess hello Time Series Insights environment.</span></span>
3. <span data-ttu-id="c2b50-121">Hello Alkalmazásazonosítót és a kulcs tooacquire token toohello `"https://api.timeseries.azure.com/"` célközönségtől vagy erőforrástól.</span><span class="sxs-lookup"><span data-stu-id="c2b50-121">Use hello application ID and key tooacquire a token toohello `"https://api.timeseries.azure.com/"` audience or resource.</span></span> <span data-ttu-id="c2b50-122">hello token majd lehet használt toocall hello idő adatsorozat Hirdetéselemző API-t.</span><span class="sxs-lookup"><span data-stu-id="c2b50-122">hello token can then be used toocall hello Time Series Insights API.</span></span>

<span data-ttu-id="c2b50-123">Az alábbiakban a részletes lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="c2b50-123">Here are hello detailed steps:</span></span>

1. <span data-ttu-id="c2b50-124">Hello Azure-portálon, válassza ki **Azure Active Directory** > **App regisztrációk** > **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="c2b50-124">In hello Azure portal, select **Azure Active Directory** > **App registrations** > **New application registration**.</span></span>

   ![Új alkalmazás regisztrálása az Azure Active Directoryban](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. <span data-ttu-id="c2b50-126">Hello alkalmazás biztosítják a nevét, jelölje be hello típus toobe **webalkalmazás / API**, válassza ki a bármilyen érvényes URI-JÁNAK **bejelentkezési URL-cím**, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c2b50-126">Give hello application a name, select hello type toobe **Web app / API**, select any valid URI for **Sign-on URL**, and click **Create**.</span></span>

   ![Hello alkalmazás létrehozása az Azure Active Directoryban](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. <span data-ttu-id="c2b50-128">Válassza ki az újonnan létrehozott alkalmazást, és másolja az alkalmazás azonosítója tooyour kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="c2b50-128">Select your newly created application and copy its application ID tooyour favorite text editor.</span></span>

   ![Másolja a hello Alkalmazásazonosító](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. <span data-ttu-id="c2b50-130">Válassza ki **kulcsok**, hello kulcs nevét, jelölje be hello lejárati, adja meg, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c2b50-130">Select **Keys**, enter hello key name, select hello expiration, and click **Save**.</span></span>

   ![Kulcsok – alkalmazás választása](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Adja meg a hello kulcs nevét és a lejárati, és kattintson a Mentés gombra](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. <span data-ttu-id="c2b50-133">Másolja a hello kulcs tooyour kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="c2b50-133">Copy hello key tooyour favorite text editor.</span></span>

   ![Hello alkalmazás kulcs másolása](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. <span data-ttu-id="c2b50-135">Hello idő adatsorozat Insights környezet, válassza a **adat-hozzáférési házirendjeit** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c2b50-135">For hello Time Series Insights environment, select **Data Access Policies** and click **Add**.</span></span>

   ![Adja hozzá az új adatok hozzáférési házirend toohello idő adatsorozat Insights környezet](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. <span data-ttu-id="c2b50-137">A hello **felhasználó kiválasztása** párbeszédpanelen Beillesztés hello alkalmazás neve (a 2. lépés) vagy alkalmazás-azonosító (a 3. lépés).</span><span class="sxs-lookup"><span data-stu-id="c2b50-137">In hello **Select User** dialog box, paste hello application name (from step 2) or application ID (from step 3).</span></span>

   ![Alkalmazások keresésére hello felhasználó kiválasztása párbeszédpanel](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. <span data-ttu-id="c2b50-139">Jelölje be hello szerepkör (**olvasó** a lekérdezésre adatok, **közreműködő** az adatok lekérdezése és referenciaadatok módosítása), és kattintson a **Ok**.</span><span class="sxs-lookup"><span data-stu-id="c2b50-139">Select hello role (**Reader** for querying data, **Contributor** for querying data and changing reference data) and click **Ok**.</span></span>

   ![Válassza ki az olvasó és közreműködő hello szerepkör kiválasztása párbeszédpanel](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. <span data-ttu-id="c2b50-141">Hello szabályzat mentése kattintva **Ok**.</span><span class="sxs-lookup"><span data-stu-id="c2b50-141">Save hello policy by clicking **Ok**.</span></span>

10. <span data-ttu-id="c2b50-142">Hello Alkalmazásazonosítót használja (a 3. lépés) és az alkalmazás (az 5. lépés) kulcs tooacquire hello token hello alkalmazás nevében.</span><span class="sxs-lookup"><span data-stu-id="c2b50-142">Use hello application ID (from step 3) and application key (from step 5) tooacquire hello token on behalf of hello application.</span></span> <span data-ttu-id="c2b50-143">hello token majd adhatók át a hello `Authorization` Ha hello alkalmazás hívások hello idő adatsorozat Hirdetéselemző API-t.</span><span class="sxs-lookup"><span data-stu-id="c2b50-143">hello token can then be passed in hello `Authorization` header when hello application calls hello Time Series Insights API.</span></span>

    <span data-ttu-id="c2b50-144">Használata C#, a következő kód tooacquire hello token hello alkalmazás nevében hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c2b50-144">If you're using C#, you can use hello following code tooacquire hello token on behalf of hello application.</span></span> <span data-ttu-id="c2b50-145">Egy teljes mintát, lásd: [adatait használó C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="c2b50-145">For a complete sample, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set hello resource URI toohello Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of hello application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

## <a name="next-steps"></a><span data-ttu-id="c2b50-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2b50-146">Next steps</span></span>

<span data-ttu-id="c2b50-147">Használja a hello alkalmazás Azonosítóját és kulcsát az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c2b50-147">Use hello application ID and key in your application.</span></span> <span data-ttu-id="c2b50-148">Hello idő adatsorozat Hirdetéselemző API-t behívó kód a minta, lásd: [adatait használó C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="c2b50-148">For sample code that calls hello Time Series Insights API, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="c2b50-149">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c2b50-149">See also</span></span>

* <span data-ttu-id="c2b50-150">[Lekérdezési API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) a hello teljes lekérdezés API-referencia</span><span class="sxs-lookup"><span data-stu-id="c2b50-150">[Query API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) for hello full Query API reference</span></span>
* [<span data-ttu-id="c2b50-151">Hello Azure-portálon található egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2b50-151">Create a service principal in hello Azure portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
