---
title: "Végfelhasználói hitelesítési: Data Lake Store az Azure Active Directoryhoz |} Microsoft Docs"
description: "Megtudhatja, hogyan végfelhasználói hitelesítés az Azure Active Directory használatával a Data Lake Store elérése"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ec586ecd-1b42-459e-b600-fadbb7b80a9b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: c20f5c39b00992d801909c8e5de292f3c2f12673
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a><span data-ttu-id="035a7-103">A Data Lake Store az Azure Active Directoryval végfelhasználói hitelesítés</span><span class="sxs-lookup"><span data-stu-id="035a7-103">End-user authentication with Data Lake Store using Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="035a7-104">Szolgáltatások közötti hitelesítés</span><span class="sxs-lookup"><span data-stu-id="035a7-104">Service-to-service authentication</span></span>](data-lake-store-authenticate-using-active-directory.md)
> * [<span data-ttu-id="035a7-105">Végfelhasználói hitelesítés</span><span class="sxs-lookup"><span data-stu-id="035a7-105">End-user authentication</span></span>](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

<span data-ttu-id="035a7-106">Azure Data Lake Store az Azure Active Directory-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="035a7-106">Azure Data Lake Store uses Azure Active Directory for authentication.</span></span> <span data-ttu-id="035a7-107">Egy alkalmazás, amely az Azure Data Lake Store vagy az Azure Data Lake Analytics szerzői, mielőtt először határozza meg, hogyan szeretné az Azure Active Directoryval (Azure AD) az alkalmazás hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="035a7-107">Before authoring an application that works with Azure Data Lake Store or Azure Data Lake Analytics, you must first decide how you would like to authenticate your application with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="035a7-108">A két fő elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="035a7-108">The two main options available are:</span></span>

* <span data-ttu-id="035a7-109">Végfelhasználói hitelesítési (Ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="035a7-109">End-user authentication (this article)</span></span>
* <span data-ttu-id="035a7-110">Szolgáltatások közötti hitelesítés</span><span class="sxs-lookup"><span data-stu-id="035a7-110">Service-to-service authentication</span></span>

<span data-ttu-id="035a7-111">Mindkét ezeket a beállításokat az alkalmazás OAuth 2.0-tokenhez, lekérdezi az összes kérelmet az Azure Data Lake Store vagy az Azure Data Lake Analytics csatolva alatt megadott eredményez.</span><span class="sxs-lookup"><span data-stu-id="035a7-111">Both these options result in your application being provided with an OAuth 2.0 token, which gets attached to each request made to Azure Data Lake Store or Azure Data Lake Analytics.</span></span>

<span data-ttu-id="035a7-112">Ez a cikk megbeszélések arról, hogyan hozzon létre egy **végfelhasználói hitelesítéshez natív Azure AD-alkalmazást**.</span><span class="sxs-lookup"><span data-stu-id="035a7-112">This article talks about how create an **Azure AD native application for end-user authentication**.</span></span> <span data-ttu-id="035a7-113">További információ a szolgáltatások közötti hitelesítéshez az Azure AD-alkalmazás konfigurációja [szolgáltatások közötti hitelesítés az Azure Active Directory használatával a Data Lake Store](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="035a7-113">For instructions on Azure AD application configuration for service-to-service authentication see [Service-to-service authentication with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="035a7-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="035a7-114">Prerequisites</span></span>
* <span data-ttu-id="035a7-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="035a7-115">An Azure subscription.</span></span> <span data-ttu-id="035a7-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="035a7-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="035a7-117">Az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="035a7-117">Your subscription ID.</span></span> <span data-ttu-id="035a7-118">Az Azure portálról le tudja kérni.</span><span class="sxs-lookup"><span data-stu-id="035a7-118">You can retrieve it from the Azure Portal.</span></span> <span data-ttu-id="035a7-119">Például érhető el a Data Lake Store-fiók paneljén.</span><span class="sxs-lookup"><span data-stu-id="035a7-119">For example, it is available from the Data Lake Store account blade.</span></span>
  
    ![Előfizetés-azonosító lekérése](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* <span data-ttu-id="035a7-121">Az Azure AD-tartomány neve.</span><span class="sxs-lookup"><span data-stu-id="035a7-121">Your Azure AD domain name.</span></span> <span data-ttu-id="035a7-122">Azt az Azure portál jobb felső sarkában az egér rámutató által kérheti le.</span><span class="sxs-lookup"><span data-stu-id="035a7-122">You can retrieve it by hovering the mouse in the top-right corner of the Azure Portal.</span></span> <span data-ttu-id="035a7-123">Az alábbi képernyőképen látható, a tartománynév nem **contoso.onmicrosoft.com**, a szögletes zárójelben GUID pedig a bérlői.</span><span class="sxs-lookup"><span data-stu-id="035a7-123">From the screenshot below, the domain name is **contoso.onmicrosoft.com**, and the GUID within brackets is the tenant ID.</span></span> 
  
    ![Az AAD tartományi beolvasása](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a><span data-ttu-id="035a7-125">Végfelhasználói hitelesítés</span><span class="sxs-lookup"><span data-stu-id="035a7-125">End-user authentication</span></span>
<span data-ttu-id="035a7-126">Ez az az ajánlott módszer, ha azt szeretné, hogy a felhasználó bejelentkezni az alkalmazás az Azure AD használatával.</span><span class="sxs-lookup"><span data-stu-id="035a7-126">This is the recommended approach if you want an end-user to log in to your application via Azure AD.</span></span> <span data-ttu-id="035a7-127">Az alkalmazás tudnak hitelesítésszolgáltatóéval azonos szintű hozzáférés, a végfelhasználónak, a rendszer az Azure-erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="035a7-127">Your application will be able to access Azure resources with the same level of access as the end-user that logged in.</span></span> <span data-ttu-id="035a7-128">Adja meg a hitelesítő adatokat rendszeres időközönként ahhoz, hogy az alkalmazás fér a végfelhasználó használni kell.</span><span class="sxs-lookup"><span data-stu-id="035a7-128">Your end-user will need to provide their credentials periodically in order for your application to maintain access.</span></span>

<span data-ttu-id="035a7-129">Az eredmény, hogy a végfelhasználó jelentkezzen be, hogy az alkalmazás olyan hozzáférési jogkivonatot, és egy frissítési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="035a7-129">The result of having the end-user log in is that your application is given an access token and a refresh token.</span></span> <span data-ttu-id="035a7-130">A hozzáférési jogkivonatot minden Data Lake Store vagy a Data Lake Analytics kérelmet lekérdezi kapcsolni, és alapértelmezés szerint egy órán keresztül érvényes legyen.</span><span class="sxs-lookup"><span data-stu-id="035a7-130">The access token gets attached to each request made to Data Lake Store or Data Lake Analytics, and it is valid for one hour by default.</span></span> <span data-ttu-id="035a7-131">A frissítési jogkivonat segítségével szerezzen be egy új jogkivonatot, és alapértelmezés szerint legfeljebb két hétig érvényes rendszeresen használatakor.</span><span class="sxs-lookup"><span data-stu-id="035a7-131">The refresh token can be used to obtain a new access token, and it is valid for up to two weeks by default, if used regularly.</span></span> <span data-ttu-id="035a7-132">Két különböző szempontok a végfelhasználói bejelentkezésnél is használhatja.</span><span class="sxs-lookup"><span data-stu-id="035a7-132">You can use two different approaches for end-user log in.</span></span>

### <a name="using-the-oauth-20-pop-up"></a><span data-ttu-id="035a7-133">Az OAuth 2.0 előugró használatával</span><span class="sxs-lookup"><span data-stu-id="035a7-133">Using the OAuth 2.0 pop-up</span></span>
<span data-ttu-id="035a7-134">Az alkalmazás is elindíthatja az OAuth 2.0 hitelesítési előugró ablakban, amelyben a végfelhasználói megadhatják hitelesítő adataikat.</span><span class="sxs-lookup"><span data-stu-id="035a7-134">Your application can trigger an OAuth 2.0 authorization pop-up, in which the end-user can enter their credentials.</span></span> <span data-ttu-id="035a7-135">Az előugró ablak az Azure AD-kéttényezős hitelesítést (2FA) folyamat során is működik, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="035a7-135">This pop-up also works with the Azure AD Two-factor Authentication (2FA) process, if required.</span></span> 

> [!NOTE]
> <span data-ttu-id="035a7-136">Ez a metódus még nem támogatott az az Azure AD Authentication Library (ADAL) Python vagy Java.</span><span class="sxs-lookup"><span data-stu-id="035a7-136">This method is not yet supported in the Azure AD Authentication Library (ADAL) for Python or Java.</span></span>
> 
> 

### <a name="directly-passing-in-user-credentials"></a><span data-ttu-id="035a7-137">Felhasználói hitelesítő adatok közvetlenül benyújtása</span><span class="sxs-lookup"><span data-stu-id="035a7-137">Directly passing in user credentials</span></span>
<span data-ttu-id="035a7-138">Az alkalmazás közvetlenül adni a hitelesítő adatokat az Azure ad Szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="035a7-138">Your application can directly provide user credentials to Azure AD.</span></span> <span data-ttu-id="035a7-139">Ez a módszer csak használható a szervezeti azonosító felhasználói fiókok; Ez nem kompatibilis a személyes / a "live ID" felhasználói fiókok, beleértve a végződése @outlook.com vagy @live.com.</span><span class="sxs-lookup"><span data-stu-id="035a7-139">This method only works with organizational ID user accounts; it is not compatible with personal / “live ID” user accounts, including those ending in @outlook.com or @live.com.</span></span> <span data-ttu-id="035a7-140">Továbbá ez a metódus nem található felhasználói fiókok Azure AD-kéttényezős hitelesítést (2FA) igénylő kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="035a7-140">Furthermore, this method is not compatible with user accounts that require Azure AD Two-factor Authentication (2FA).</span></span>

### <a name="what-do-i-need-to-use-this-approach"></a><span data-ttu-id="035a7-141">Mit kell ezt a módszert használja?</span><span class="sxs-lookup"><span data-stu-id="035a7-141">What do I need to use this approach?</span></span>
* <span data-ttu-id="035a7-142">Az Azure AD-tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="035a7-142">Azure AD domain name.</span></span> <span data-ttu-id="035a7-143">Ez már szerepel-e ez a cikk az előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="035a7-143">This is already listed in the prerequisite of this article.</span></span>
* <span data-ttu-id="035a7-144">Az Azure AD **natív alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="035a7-144">Azure AD **native application**</span></span>
* <span data-ttu-id="035a7-145">Az Azure AD natív alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="035a7-145">Application ID for the Azure AD native application</span></span>
* <span data-ttu-id="035a7-146">Az Azure AD natív alkalmazás átirányítási URI</span><span class="sxs-lookup"><span data-stu-id="035a7-146">Redirect URI for the Azure AD native application</span></span>
* <span data-ttu-id="035a7-147">Delegált engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="035a7-147">Set delegated permissions</span></span>


## <a name="step-1-create-an-active-directory-native-application"></a><span data-ttu-id="035a7-148">1. lépés: Az Active Directory natív alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="035a7-148">Step 1: Create an Active Directory native application</span></span>

<span data-ttu-id="035a7-149">Hozzon létre, és a végfelhasználói hitelesítéshez az Azure AD natív alkalmazások konfigurálása az Azure Data Lake Store az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="035a7-149">Create and configure an Azure AD native application for end-user authentication with Azure Data Lake Store using Azure Active Directory.</span></span> <span data-ttu-id="035a7-150">Útmutatásért lásd: [hozzon létre egy Azure AD-alkalmazást](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="035a7-150">For instructions, see [Create an Azure AD application](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

<span data-ttu-id="035a7-151">Közben megadott utasítások a fenti hivatkozásra, gondoskodjon róla, hogy **natív** alkalmazás típusra, az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="035a7-151">While following the instructions at the above link, make sure you select **Native** for application type, as shown in the screenshot below.</span></span>

<span data-ttu-id="035a7-152">![A webalkalmazás létrehozása](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "hozzon létre natív alkalmazás")</span><span class="sxs-lookup"><span data-stu-id="035a7-152">![Create web app](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Create native app")</span></span>

## <a name="step-2-get-application-id-and-redirect-uri"></a><span data-ttu-id="035a7-153">2. lépés: Alkalmazásazonosító beszerzése és átirányítási URI</span><span class="sxs-lookup"><span data-stu-id="035a7-153">Step 2: Get application id and redirect URI</span></span>

<span data-ttu-id="035a7-154">Lásd: [Alkalmazásazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) beolvasni az Azure AD-natív alkalmazás az alkalmazás azonosítóját (más néven az ügyfél-Azonosítót a klasszikus Azure portálon).</span><span class="sxs-lookup"><span data-stu-id="035a7-154">See [Get the application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) to retrieve the application id (also called the client ID in the Azure classic portal) of the Azure AD native application.</span></span>

<span data-ttu-id="035a7-155">Az átirányítási URI-t lekéréséhez kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="035a7-155">To retrieve the redirect URI, follow the steps below.</span></span>

1. <span data-ttu-id="035a7-156">Az Azure portálon, válassza ki a **Azure Active Directory**, kattintson a **App regisztrációk**, majd található, és kattintson az imént létrehozott natív Azure AD-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="035a7-156">From the Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click the Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="035a7-157">Az a **beállítások** az alkalmazás paneljén kattintson **átirányítási URI-azonosítók**.</span><span class="sxs-lookup"><span data-stu-id="035a7-157">From the **Settings** blade for the application, click **Redirect URIs**.</span></span>

    ![Get-átirányítási URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. <span data-ttu-id="035a7-159">Másolja a megjelenített érték.</span><span class="sxs-lookup"><span data-stu-id="035a7-159">Copy the value displayed.</span></span>


## <a name="step-3-set-permissions"></a><span data-ttu-id="035a7-160">3. lépés: Engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="035a7-160">Step 3: Set permissions</span></span>

1. <span data-ttu-id="035a7-161">Az Azure portálon, válassza ki a **Azure Active Directory**, kattintson a **App regisztrációk**, majd található, és kattintson az imént létrehozott natív Azure AD-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="035a7-161">From the Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click the Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="035a7-162">Az a **beállítások** az alkalmazás paneljén kattintson **szükséges engedélyek**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="035a7-162">From the **Settings** blade for the application, click **Required permissions**, and then click **Add**.</span></span>

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. <span data-ttu-id="035a7-164">A a **API-hozzáférés hozzáadása** panelen kattintson a **API kiválasztása**, kattintson a **Azure Data Lake**, és kattintson a **válassza**.</span><span class="sxs-lookup"><span data-stu-id="035a7-164">In the **Add API Access** blade, click **Select an API**, click **Azure Data Lake**, and then click **Select**.</span></span>

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  <span data-ttu-id="035a7-166">Az a **API-hozzáférés hozzáadása** panelen kattintson **engedélyként válassza**, jelölje be a jelölőnégyzetet, hogy a **teljes hozzáférés a Data Lake store**, és kattintson a **kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="035a7-166">In the **Add API Access** blade, click **Select permissions**, select the check box to give **Full access to Data Lake Store**, and then click **Select**.</span></span>

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    <span data-ttu-id="035a7-168">Kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="035a7-168">Click **Done**.</span></span>

5. <span data-ttu-id="035a7-169">Ismételje meg az utolsó két lépést vonatkozó engedélyeket **Windows Azure szolgáltatásfelügyeleti API** is.</span><span class="sxs-lookup"><span data-stu-id="035a7-169">Repeat the last two steps to grant permissions for **Windows Azure Service Management API** as well.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="035a7-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="035a7-170">Next steps</span></span>
<span data-ttu-id="035a7-171">Ebben a cikkben egy natív Azure AD-alkalmazást létrehozni, és a szükséges információkat az ügyfél-alkalmazások használata a .NET SDK-val, a Java SDK-t, a REST API-t, a stb szerzői összegyűjtött. Most már folytathatja, az alábbi cikkek tartalmazzák, amely először a Data Lake Store hitelesítéséhez, és végezze el más műveleteket végez a tároló az Azure AD-webes alkalmazás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="035a7-171">In this article you created an Azure AD native application and gathered the information you need in your client applications that you author using .NET SDK, Java SDK, REST API, etc. You can now proceed to the following articles that talk about how to use the Azure AD web application to first authenticate with Data Lake Store and then perform other operations on the store.</span></span>

* [<span data-ttu-id="035a7-172">Az Azure Data Lake Store használatának első lépései a .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="035a7-172">Get started with Azure Data Lake Store using .NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
* [<span data-ttu-id="035a7-173">Az Azure Data Lake Store Java SDK használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="035a7-173">Get started with Azure Data Lake Store using Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
* [<span data-ttu-id="035a7-174">Ismerkedés az Azure Data Lake Store REST API használatával</span><span class="sxs-lookup"><span data-stu-id="035a7-174">Get started with Azure Data Lake Store using REST API</span></span>](data-lake-store-get-started-rest-api.md)

