---
title: "Hitelesítési és engedélyezési egy egyéni alkalmazás, amely a Azure idő adatsorozat Insights API konfigurálása |} Microsoft Docs"
description: "Ez az oktatóanyag azt ismerteti, hogyan konfigurálhatja a hitelesítési és engedélyezési, amely a Azure idő adatsorozat Insights API-egyéni alkalmazás"
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
ms.openlocfilehash: 4dd4865dc556e09a31d2cb7a32768aeb19ba9900
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a><span data-ttu-id="49a9d-103">Hitelesítési és engedélyezési Azure idő adatsorozat Insights API-hoz.</span><span class="sxs-lookup"><span data-stu-id="49a9d-103">Authentication and authorization for Azure Time Series Insights API</span></span>

<span data-ttu-id="49a9d-104">Ez a cikk ismerteti, amely a Azure idő adatsorozat Insights API-egyéni alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="49a9d-104">This article explains how to configure a custom application that calls the Azure Time Series Insights API.</span></span>

## <a name="service-principal"></a><span data-ttu-id="49a9d-105">Egyszerű szolgáltatásnév</span><span class="sxs-lookup"><span data-stu-id="49a9d-105">Service principal</span></span>

<span data-ttu-id="49a9d-106">Ez a szakasz ismerteti a idő adatsorozat Hirdetéselemző API-t nevében az alkalmazás eléréséhez alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="49a9d-106">This section explains how to configure an application to access the Time Series Insights API on behalf of the application.</span></span> <span data-ttu-id="49a9d-107">Az alkalmazás ezután kérdezhet le adatokat, vagy hivatkozási adatok közzététele a idő adatsorozat Insights környezetben alkalmazás hitelesítő adatait, és nem a felhasználó hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="49a9d-107">The application can then query data or publish reference data in the Time Series Insights environment with application credentials and not the user credentials.</span></span>

<span data-ttu-id="49a9d-108">Ha egy alkalmazást, amelyet a hozzáférés idejének adatsorozat Insights, állítson be egy Azure Active Directory-alkalmazás, és rendelje hozzá az adat-hozzáférési házirendjeit a idő adatsorozat Insights környezetben.</span><span class="sxs-lookup"><span data-stu-id="49a9d-108">When you have an application that needs to access Time Series Insights, you must set up an Azure Active Directory application and assign the data access policies in the Time Series Insights environment.</span></span> <span data-ttu-id="49a9d-109">Ez a megközelítés célszerű a saját credentials az alkalmazást futtató, mert:</span><span class="sxs-lookup"><span data-stu-id="49a9d-109">This approach is preferable to running the app under your own credentials because:</span></span>

* <span data-ttu-id="49a9d-110">Engedélyeket rendelhet a app identitása eltér a saját engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="49a9d-110">You can assign permissions to the app identity that are different from your own permissions.</span></span> <span data-ttu-id="49a9d-111">Ezek az engedélyek általában korlátozódik, hogy mit az alkalmazás kell tennie.</span><span class="sxs-lookup"><span data-stu-id="49a9d-111">Typically, these permissions are restricted to exactly what the app needs to do.</span></span> <span data-ttu-id="49a9d-112">Például engedélyezheti az alkalmazásnak, hogy csak olvasható adatokat egy adott idő adatsorozat Insights környezetben.</span><span class="sxs-lookup"><span data-stu-id="49a9d-112">For example, you can allow the app to only read data in a particular Time Series Insights environment.</span></span>
* <span data-ttu-id="49a9d-113">Nem kell módosítani az alkalmazás hitelesítő adatokat, ha az Ön feladatkörei módosítása.</span><span class="sxs-lookup"><span data-stu-id="49a9d-113">You don't have to change the app's credentials if your responsibilities change.</span></span>
* <span data-ttu-id="49a9d-114">Egy tanúsítvány vagy egy alkalmazás kulcs segítségével automatizálhatja a hitelesítés, amikor egy felügyelet nélküli parancsfájllal futtatja.</span><span class="sxs-lookup"><span data-stu-id="49a9d-114">You can use a certificate or an application key to automate authentication when you're running an unattended script.</span></span>

<span data-ttu-id="49a9d-115">Ez a cikk bemutatja, hogyan hajtsa végre ezeket a lépéseket az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="49a9d-115">This article shows you how to perform those steps through the Azure portal.</span></span> <span data-ttu-id="49a9d-116">A single-bérlői alkalmazások, ahol az alkalmazás futtatásához csak egy szervezet célja összpontosít.</span><span class="sxs-lookup"><span data-stu-id="49a9d-116">It focuses on a single-tenant application where the application is intended to run in only one organization.</span></span> <span data-ttu-id="49a9d-117">Single-bérlő alkalmazásokat a szervezet futó üzleti alkalmazásokhoz általában használ.</span><span class="sxs-lookup"><span data-stu-id="49a9d-117">You typically use single-tenant applications for line-of-business applications that run in your organization.</span></span>

<span data-ttu-id="49a9d-118">A telepítő folyamat három magas szintű lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="49a9d-118">The setup flow consists of three high-level steps:</span></span>

1. <span data-ttu-id="49a9d-119">Alkalmazás létrehozása az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="49a9d-119">Create an application in Azure Active Directory.</span></span>
2. <span data-ttu-id="49a9d-120">Engedélyezi az alkalmazásnak a idő adatsorozat Insights környezet eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="49a9d-120">Authorize this application to access the Time Series Insights environment.</span></span>
3. <span data-ttu-id="49a9d-121">Az alkalmazás Azonosítóját és kulcsát használja a jogkivonatot a `"https://api.timeseries.azure.com/"` célközönségtől vagy erőforrástól.</span><span class="sxs-lookup"><span data-stu-id="49a9d-121">Use the application ID and key to acquire a token to the `"https://api.timeseries.azure.com/"` audience or resource.</span></span> <span data-ttu-id="49a9d-122">A jogkivonat az idő adatsorozat Insights API majd használható.</span><span class="sxs-lookup"><span data-stu-id="49a9d-122">The token can then be used to call the Time Series Insights API.</span></span>

<span data-ttu-id="49a9d-123">Részletes lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="49a9d-123">Here are the detailed steps:</span></span>

1. <span data-ttu-id="49a9d-124">Válassza ki az Azure-portálon **Azure Active Directory** > **App regisztrációk** > **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="49a9d-124">In the Azure portal, select **Azure Active Directory** > **App registrations** > **New application registration**.</span></span>

   ![Új alkalmazás regisztrálása az Azure Active Directoryban](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. <span data-ttu-id="49a9d-126">Nevezze el az alkalmazást, jelölje ki a kell **webalkalmazás / API**, válassza ki a bármilyen érvényes URI-JÁNAK **bejelentkezési URL-cím**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="49a9d-126">Give the application a name, select the type to be **Web app / API**, select any valid URI for **Sign-on URL**, and click **Create**.</span></span>

   ![Az alkalmazás létrehozása az Azure Active Directoryban](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. <span data-ttu-id="49a9d-128">Válassza ki az újonnan létrehozott alkalmazást, és másolja az alkalmazás Azonosítóját a kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="49a9d-128">Select your newly created application and copy its application ID to your favorite text editor.</span></span>

   ![Másolja át az Alkalmazásazonosítót](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. <span data-ttu-id="49a9d-130">Válassza ki **kulcsok**, adja meg a kulcs nevét, válassza ki a lejárati, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="49a9d-130">Select **Keys**, enter the key name, select the expiration, and click **Save**.</span></span>

   ![Kulcsok – alkalmazás választása](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Adja meg a kulcs nevét és a lejárati, és kattintson a Mentés gombra](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. <span data-ttu-id="49a9d-133">A kulcs másolása kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="49a9d-133">Copy the key to your favorite text editor.</span></span>

   ![Az alkalmazás kulcs másolása](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. <span data-ttu-id="49a9d-135">Az idő adatsorozat Insights környezetben, válassza a **adat-hozzáférési házirendjeit** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="49a9d-135">For the Time Series Insights environment, select **Data Access Policies** and click **Add**.</span></span>

   ![Új adatok házirend hozzáadása a idő adatsorozat Insights környezet](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. <span data-ttu-id="49a9d-137">Az a **felhasználó kiválasztása** párbeszédpanelen illessze be az alkalmazás neve (a 2. lépés) vagy alkalmazás-azonosító (a 3. lépés).</span><span class="sxs-lookup"><span data-stu-id="49a9d-137">In the **Select User** dialog box, paste the application name (from step 2) or application ID (from step 3).</span></span>

   ![Alkalmazások keresésére a felhasználó kiválasztása párbeszédpanel](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. <span data-ttu-id="49a9d-139">Válassza ki a szerepkört (**olvasó** a lekérdezésre adatok, **közreműködő** az adatok lekérdezése és referenciaadatok módosítása), majd **Ok**.</span><span class="sxs-lookup"><span data-stu-id="49a9d-139">Select the role (**Reader** for querying data, **Contributor** for querying data and changing reference data) and click **Ok**.</span></span>

   ![Válassza ki az olvasó vagy közreműködői szerepkör kiválasztása párbeszédpanel](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. <span data-ttu-id="49a9d-141">A szabályzat mentése kattintva **Ok**.</span><span class="sxs-lookup"><span data-stu-id="49a9d-141">Save the policy by clicking **Ok**.</span></span>

10. <span data-ttu-id="49a9d-142">Az alkalmazás nevében jogkivonat használja az alkalmazás-azonosító (a 3. lépés) és az alkalmazás kulcsot (az 5. lépés).</span><span class="sxs-lookup"><span data-stu-id="49a9d-142">Use the application ID (from step 3) and application key (from step 5) to acquire the token on behalf of the application.</span></span> <span data-ttu-id="49a9d-143">A token majd adhatók át a a `Authorization` fejléc, ha az alkalmazás meghívja a idő adatsorozat Hirdetéselemző API-t.</span><span class="sxs-lookup"><span data-stu-id="49a9d-143">The token can then be passed in the `Authorization` header when the application calls the Time Series Insights API.</span></span>

    <span data-ttu-id="49a9d-144">Használata C#, a következő kód segítségével nevében az alkalmazás jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="49a9d-144">If you're using C#, you can use the following code to acquire the token on behalf of the application.</span></span> <span data-ttu-id="49a9d-145">Egy teljes mintát, lásd: [adatait használó C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="49a9d-145">For a complete sample, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

## <a name="next-steps"></a><span data-ttu-id="49a9d-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49a9d-146">Next steps</span></span>

<span data-ttu-id="49a9d-147">Használja az alkalmazás Azonosítóját és kulcsát az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="49a9d-147">Use the application ID and key in your application.</span></span> <span data-ttu-id="49a9d-148">Az idő adatsorozat Hirdetéselemző API-t behívó kód a minta, lásd: [adatait használó C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="49a9d-148">For sample code that calls the Time Series Insights API, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="49a9d-149">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="49a9d-149">See also</span></span>

* <span data-ttu-id="49a9d-150">[Lekérdezési API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) a teljes lekérdezés API-referencia</span><span class="sxs-lookup"><span data-stu-id="49a9d-150">[Query API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) for the full Query API reference</span></span>
* [<span data-ttu-id="49a9d-151">Egy egyszerű szolgáltatás létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="49a9d-151">Create a service principal in the Azure portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
