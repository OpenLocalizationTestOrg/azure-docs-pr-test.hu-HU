---
title: "Végfelhasználói hitelesítési: Data Lake Store az Azure Active Directoryhoz |} Microsoft Docs"
description: "Megtudhatja, hogyan tooachieve végfelhasználói hitelesítési a Data Lake Store az Azure Active Directoryval"
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
ms.openlocfilehash: fd58f4f2d8fc915b8bc51d9e5b040d2cee34047e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a><span data-ttu-id="fd07f-103">A Data Lake Store az Azure Active Directoryval végfelhasználói hitelesítés</span><span class="sxs-lookup"><span data-stu-id="fd07f-103">End-user authentication with Data Lake Store using Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fd07f-104">Szolgáltatások közötti hitelesítés</span><span class="sxs-lookup"><span data-stu-id="fd07f-104">Service-to-service authentication</span></span>](data-lake-store-authenticate-using-active-directory.md)
> * [<span data-ttu-id="fd07f-105">Végfelhasználói hitelesítés</span><span class="sxs-lookup"><span data-stu-id="fd07f-105">End-user authentication</span></span>](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

<span data-ttu-id="fd07f-106">Azure Data Lake Store az Azure Active Directory-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="fd07f-106">Azure Data Lake Store uses Azure Active Directory for authentication.</span></span> <span data-ttu-id="fd07f-107">Egy alkalmazás, amely az Azure Data Lake Store vagy az Azure Data Lake Analytics szerzői előtt döntse el, először hogyan szeretné tooauthenticate az alkalmazás Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fd07f-107">Before authoring an application that works with Azure Data Lake Store or Azure Data Lake Analytics, you must first decide how you would like tooauthenticate your application with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="fd07f-108">hello két fő elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="fd07f-108">hello two main options available are:</span></span>

* <span data-ttu-id="fd07f-109">Végfelhasználói hitelesítési (Ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="fd07f-109">End-user authentication (this article)</span></span>
* <span data-ttu-id="fd07f-110">Szolgáltatások közötti hitelesítés</span><span class="sxs-lookup"><span data-stu-id="fd07f-110">Service-to-service authentication</span></span>

<span data-ttu-id="fd07f-111">Mindkét ezeket a beállításokat az alkalmazás OAuth 2.0-tokenhez, amely lekérdezi a csatolt tooeach kérelmet tooAzure Data Lake Store vagy az Azure Data Lake Analytics alatt megadott eredményez.</span><span class="sxs-lookup"><span data-stu-id="fd07f-111">Both these options result in your application being provided with an OAuth 2.0 token, which gets attached tooeach request made tooAzure Data Lake Store or Azure Data Lake Analytics.</span></span>

<span data-ttu-id="fd07f-112">Ez a cikk megbeszélések arról, hogyan hozzon létre egy **végfelhasználói hitelesítéshez natív Azure AD-alkalmazást**.</span><span class="sxs-lookup"><span data-stu-id="fd07f-112">This article talks about how create an **Azure AD native application for end-user authentication**.</span></span> <span data-ttu-id="fd07f-113">További információ a szolgáltatások közötti hitelesítéshez az Azure AD-alkalmazás konfigurációja [szolgáltatások közötti hitelesítés az Azure Active Directory használatával a Data Lake Store](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="fd07f-113">For instructions on Azure AD application configuration for service-to-service authentication see [Service-to-service authentication with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd07f-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fd07f-114">Prerequisites</span></span>
* <span data-ttu-id="fd07f-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="fd07f-115">An Azure subscription.</span></span> <span data-ttu-id="fd07f-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fd07f-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="fd07f-117">Az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="fd07f-117">Your subscription ID.</span></span> <span data-ttu-id="fd07f-118">Az Azure portál hello hozzáférhet.</span><span class="sxs-lookup"><span data-stu-id="fd07f-118">You can retrieve it from hello Azure Portal.</span></span> <span data-ttu-id="fd07f-119">Például érhető el a Data Lake Store-fiók panelen hello.</span><span class="sxs-lookup"><span data-stu-id="fd07f-119">For example, it is available from hello Data Lake Store account blade.</span></span>
  
    ![Előfizetés-azonosító lekérése](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* <span data-ttu-id="fd07f-121">Az Azure AD-tartomány neve.</span><span class="sxs-lookup"><span data-stu-id="fd07f-121">Your Azure AD domain name.</span></span> <span data-ttu-id="fd07f-122">Ez által rámutató hello egér hello Azure portál jobb felső sarkában hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="fd07f-122">You can retrieve it by hovering hello mouse in hello top-right corner of hello Azure Portal.</span></span> <span data-ttu-id="fd07f-123">Az alábbi hello képernyőképet, hello tartománynév megadása **contoso.onmicrosoft.com**, és a szögletes zárójelben GUID hello az hello bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fd07f-123">From hello screenshot below, hello domain name is **contoso.onmicrosoft.com**, and hello GUID within brackets is hello tenant ID.</span></span> 
  
    ![Az AAD tartományi beolvasása](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a><span data-ttu-id="fd07f-125">Végfelhasználói hitelesítés</span><span class="sxs-lookup"><span data-stu-id="fd07f-125">End-user authentication</span></span>
<span data-ttu-id="fd07f-126">Ez az ajánlott megközelítést alkalmazva, ha azt szeretné, hogy egy végfelhasználói toolog az Azure AD alkalmazás tooyour hello.</span><span class="sxs-lookup"><span data-stu-id="fd07f-126">This is hello recommended approach if you want an end-user toolog in tooyour application via Azure AD.</span></span> <span data-ttu-id="fd07f-127">Az alkalmazás képes tooaccess Azure-erőforrások hello lesz, a rendszer a hello végfelhasználói hozzáférési azonos szinten.</span><span class="sxs-lookup"><span data-stu-id="fd07f-127">Your application will be able tooaccess Azure resources with hello same level of access as hello end-user that logged in.</span></span> <span data-ttu-id="fd07f-128">A végfelhasználói kell tooprovide rendszeres időközönként ahhoz, hogy az alkalmazás toomaintain hozzáférést a hitelesítő adataikat.</span><span class="sxs-lookup"><span data-stu-id="fd07f-128">Your end-user will need tooprovide their credentials periodically in order for your application toomaintain access.</span></span>

<span data-ttu-id="fd07f-129">hello rendelkezik-e jelentkezni hello végfelhasználói eredménye, hogy olyan hozzáférési jogkivonatot és egy frissítési adta-e az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fd07f-129">hello result of having hello end-user log in is that your application is given an access token and a refresh token.</span></span> <span data-ttu-id="fd07f-130">hello hozzáférési jogkivonat beolvasása tooData Lake Store-ból vagy a Data Lake Analytics csatolt tooeach kérelmet, és alapértelmezés szerint egy órán keresztül érvényes legyen.</span><span class="sxs-lookup"><span data-stu-id="fd07f-130">hello access token gets attached tooeach request made tooData Lake Store or Data Lake Analytics, and it is valid for one hour by default.</span></span> <span data-ttu-id="fd07f-131">hello frissítési jogkivonat lehet egy új jogkivonatot, és esetén érvényes mentése tootwo hét alapértelmezés szerint ha rendszeresen használt használt tooobtain.</span><span class="sxs-lookup"><span data-stu-id="fd07f-131">hello refresh token can be used tooobtain a new access token, and it is valid for up tootwo weeks by default, if used regularly.</span></span> <span data-ttu-id="fd07f-132">Két különböző szempontok a végfelhasználói bejelentkezésnél is használhatja.</span><span class="sxs-lookup"><span data-stu-id="fd07f-132">You can use two different approaches for end-user log in.</span></span>

### <a name="using-hello-oauth-20-pop-up"></a><span data-ttu-id="fd07f-133">Előugró hello OAuth 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="fd07f-133">Using hello OAuth 2.0 pop-up</span></span>
<span data-ttu-id="fd07f-134">Az alkalmazás is elindíthatja az OAuth 2.0 hitelesítési előugró ablakban mely hello a végfelhasználó adja meg a hitelesítő adataikat.</span><span class="sxs-lookup"><span data-stu-id="fd07f-134">Your application can trigger an OAuth 2.0 authorization pop-up, in which hello end-user can enter their credentials.</span></span> <span data-ttu-id="fd07f-135">Az előugró ablak hello Azure AD-kéttényezős hitelesítést (2FA) folyamat is működik, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="fd07f-135">This pop-up also works with hello Azure AD Two-factor Authentication (2FA) process, if required.</span></span> 

> [!NOTE]
> <span data-ttu-id="fd07f-136">Ez a metódus még nem támogatott az Azure AD Authentication Library (ADAL) hello Python vagy Java.</span><span class="sxs-lookup"><span data-stu-id="fd07f-136">This method is not yet supported in hello Azure AD Authentication Library (ADAL) for Python or Java.</span></span>
> 
> 

### <a name="directly-passing-in-user-credentials"></a><span data-ttu-id="fd07f-137">Felhasználói hitelesítő adatok közvetlenül benyújtása</span><span class="sxs-lookup"><span data-stu-id="fd07f-137">Directly passing in user credentials</span></span>
<span data-ttu-id="fd07f-138">Az alkalmazás közvetlenül képes biztosítani a felhasználói hitelesítő adatok tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="fd07f-138">Your application can directly provide user credentials tooAzure AD.</span></span> <span data-ttu-id="fd07f-139">Ez a módszer csak használható a szervezeti azonosító felhasználói fiókok; Ez nem kompatibilis a személyes / a "live ID" felhasználói fiókok, beleértve a végződése @outlook.com vagy @live.com. Továbbá ez a metódus nem található felhasználói fiókok Azure AD-kéttényezős hitelesítést (2FA) igénylő kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="fd07f-139">This method only works with organizational ID user accounts; it is not compatible with personal / “live ID” user accounts, including those ending in @outlook.com or @live.com. Furthermore, this method is not compatible with user accounts that require Azure AD Two-factor Authentication (2FA).</span></span>

### <a name="what-do-i-need-toouse-this-approach"></a><span data-ttu-id="fd07f-140">Mire van szükségem az toouse ezt a módszert használja?</span><span class="sxs-lookup"><span data-stu-id="fd07f-140">What do I need toouse this approach?</span></span>
* <span data-ttu-id="fd07f-141">Az Azure AD-tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="fd07f-141">Azure AD domain name.</span></span> <span data-ttu-id="fd07f-142">Ez már szerepel-e ez a cikk hello előfeltétel.</span><span class="sxs-lookup"><span data-stu-id="fd07f-142">This is already listed in hello prerequisite of this article.</span></span>
* <span data-ttu-id="fd07f-143">Az Azure AD **natív alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="fd07f-143">Azure AD **native application**</span></span>
* <span data-ttu-id="fd07f-144">Az Azure AD hello natív alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="fd07f-144">Application ID for hello Azure AD native application</span></span>
* <span data-ttu-id="fd07f-145">A hello natív Azure AD-alkalmazás átirányítási URI</span><span class="sxs-lookup"><span data-stu-id="fd07f-145">Redirect URI for hello Azure AD native application</span></span>
* <span data-ttu-id="fd07f-146">Delegált engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="fd07f-146">Set delegated permissions</span></span>


## <a name="step-1-create-an-active-directory-native-application"></a><span data-ttu-id="fd07f-147">1. lépés: Az Active Directory natív alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd07f-147">Step 1: Create an Active Directory native application</span></span>

<span data-ttu-id="fd07f-148">Hozzon létre, és a végfelhasználói hitelesítéshez az Azure AD natív alkalmazások konfigurálása az Azure Data Lake Store az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="fd07f-148">Create and configure an Azure AD native application for end-user authentication with Azure Data Lake Store using Azure Active Directory.</span></span> <span data-ttu-id="fd07f-149">Útmutatásért lásd: [hozzon létre egy Azure AD-alkalmazást](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fd07f-149">For instructions, see [Create an Azure AD application](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

<span data-ttu-id="fd07f-150">Közben a következő hello található utasítások segítségével: hello hivatkozás felett, gondoskodjon róla, hogy **natív** alkalmazás típusra, az alábbi hello képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="fd07f-150">While following hello instructions at hello above link, make sure you select **Native** for application type, as shown in hello screenshot below.</span></span>

<span data-ttu-id="fd07f-151">![A webalkalmazás létrehozása](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "hozzon létre natív alkalmazás")</span><span class="sxs-lookup"><span data-stu-id="fd07f-151">![Create web app](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Create native app")</span></span>

## <a name="step-2-get-application-id-and-redirect-uri"></a><span data-ttu-id="fd07f-152">2. lépés: Alkalmazásazonosító beszerzése és átirányítási URI</span><span class="sxs-lookup"><span data-stu-id="fd07f-152">Step 2: Get application id and redirect URI</span></span>

<span data-ttu-id="fd07f-153">Lásd: [hello Alkalmazásazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) tooretrieve hello alkalmazásazonosítót (más néven hello ügyfél-azonosító hello a klasszikus Azure portálon) natív hello Azure AD-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fd07f-153">See [Get hello application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) tooretrieve hello application id (also called hello client ID in hello Azure classic portal) of hello Azure AD native application.</span></span>

<span data-ttu-id="fd07f-154">tooretrieve hello átirányítási URI-címe, kövesse az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="fd07f-154">tooretrieve hello redirect URI, follow hello steps below.</span></span>

1. <span data-ttu-id="fd07f-155">Hello Azure portált, válassza ki **Azure Active Directory**, kattintson a **App regisztrációk**, majd található, és kattintson az imént létrehozott hello Azure AD-natív alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fd07f-155">From hello Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click hello Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="fd07f-156">A hello **beállítások** hello alkalmazás paneljén kattintson **átirányítási URI-azonosítók**.</span><span class="sxs-lookup"><span data-stu-id="fd07f-156">From hello **Settings** blade for hello application, click **Redirect URIs**.</span></span>

    ![Get-átirányítási URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. <span data-ttu-id="fd07f-158">Másolja a megjelenített hello érték.</span><span class="sxs-lookup"><span data-stu-id="fd07f-158">Copy hello value displayed.</span></span>


## <a name="step-3-set-permissions"></a><span data-ttu-id="fd07f-159">3. lépés: Engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="fd07f-159">Step 3: Set permissions</span></span>

1. <span data-ttu-id="fd07f-160">Hello Azure portált, válassza ki **Azure Active Directory**, kattintson a **App regisztrációk**, majd található, és kattintson az imént létrehozott hello Azure AD-natív alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fd07f-160">From hello Azure Portal, select **Azure Active Directory**, click **App registrations**, and then find and click hello Azure AD native application that you just created.</span></span>

2. <span data-ttu-id="fd07f-161">A hello **beállítások** hello alkalmazás paneljén kattintson **szükséges engedélyek**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fd07f-161">From hello **Settings** blade for hello application, click **Required permissions**, and then click **Add**.</span></span>

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. <span data-ttu-id="fd07f-163">A hello **API-hozzáférés hozzáadása** panelen kattintson **API kiválasztása**, kattintson a **Azure Data Lake**, és kattintson a **kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="fd07f-163">In hello **Add API Access** blade, click **Select an API**, click **Azure Data Lake**, and then click **Select**.</span></span>

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  <span data-ttu-id="fd07f-165">Hello a **API-hozzáférés hozzáadása** panelen kattintson **engedélyként válassza**, válasszon hello jelölőnégyzetet toogive **teljes körű hozzáférés az tooData Lake Store**, és kattintson a **kiválasztása **.</span><span class="sxs-lookup"><span data-stu-id="fd07f-165">In hello **Add API Access** blade, click **Select permissions**, select hello check box toogive **Full access tooData Lake Store**, and then click **Select**.</span></span>

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    <span data-ttu-id="fd07f-167">Kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="fd07f-167">Click **Done**.</span></span>

5. <span data-ttu-id="fd07f-168">Ismétlődő hello utolsó két lépést toogrant engedélyeinek **Windows Azure szolgáltatásfelügyeleti API** is.</span><span class="sxs-lookup"><span data-stu-id="fd07f-168">Repeat hello last two steps toogrant permissions for **Windows Azure Service Management API** as well.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="fd07f-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd07f-169">Next steps</span></span>
<span data-ttu-id="fd07f-170">Ez a cikk egy natív Azure AD-alkalmazást létrehozni, és hello információ áll rendelkezésre az ügyfél alkalmazások használata a .NET SDK-val, a Java SDK-t, a REST API-t, a stb szerzői van szüksége. Most már folytathatja a szolgáltatással kapcsolatban, hogyan toouse hello Azure AD webes alkalmazás toofirst a Data Lake Store hitelesítést, és hajtsa végre az egyéb műveletek hello tárolójában lévő cikkek a következő toohello.</span><span class="sxs-lookup"><span data-stu-id="fd07f-170">In this article you created an Azure AD native application and gathered hello information you need in your client applications that you author using .NET SDK, Java SDK, REST API, etc. You can now proceed toohello following articles that talk about how toouse hello Azure AD web application toofirst authenticate with Data Lake Store and then perform other operations on hello store.</span></span>

* [<span data-ttu-id="fd07f-171">Az Azure Data Lake Store használatának első lépései a .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="fd07f-171">Get started with Azure Data Lake Store using .NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
* [<span data-ttu-id="fd07f-172">Az Azure Data Lake Store Java SDK használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="fd07f-172">Get started with Azure Data Lake Store using Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
* [<span data-ttu-id="fd07f-173">Ismerkedés az Azure Data Lake Store REST API használatával</span><span class="sxs-lookup"><span data-stu-id="fd07f-173">Get started with Azure Data Lake Store using REST API</span></span>](data-lake-store-get-started-rest-api.md)

