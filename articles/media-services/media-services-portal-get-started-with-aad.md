---
title: "Ismerkedés az Azure AD-alapú hitelesítés az Azure portál használatával |} Microsoft Docs"
description: "Útmutató: Azure Active Directory (Azure AD-) hitelesítést használ az Azure Media Services API eléréséhez használja az Azure-portálon."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 829e0759f8aeb8758f6b8895b88b60b66ffb22ed
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a><span data-ttu-id="b6d04-103">Ismerkedés az Azure AD-alapú hitelesítés az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="b6d04-103">Get started with Azure AD authentication by using the Azure portal</span></span>

<span data-ttu-id="b6d04-104">Útmutató az Azure portál Azure Active Directory (Azure AD-) hitelesítés az Azure Media Services API eléréséhez elérésére használhat.</span><span class="sxs-lookup"><span data-stu-id="b6d04-104">Learn how to use the Azure portal to access Azure Active Directory (Azure AD) authentication to access the Azure Media Services API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6d04-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b6d04-105">Prerequisites</span></span>

- <span data-ttu-id="b6d04-106">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="b6d04-106">An Azure account.</span></span> <span data-ttu-id="b6d04-107">Ha nincs fiókja, kezdje egy [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b6d04-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="b6d04-108">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="b6d04-108">A Media Services account.</span></span> <span data-ttu-id="b6d04-109">További információkért lásd: [Azure Media Services-fiók létrehozása az Azure-portál használatával](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="b6d04-109">For more information, see [Create an Azure Media Services account by using the Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="b6d04-110">Győződjön meg arról, hogy tekintse át a [eléréséhez Azure Media Services API-t az Azure AD hitelesítési áttekintés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="b6d04-110">Make sure you review the [Accessing Azure Media Services API with Azure AD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="b6d04-111">Az Azure AD-alapú hitelesítés használatakor az Azure Media Services hitelesítési két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="b6d04-111">When you use Azure AD authentication with Azure Media Services, you have two authentication options:</span></span>

- <span data-ttu-id="b6d04-112">**Felhasználó hitelesítése**.</span><span class="sxs-lookup"><span data-stu-id="b6d04-112">**User authentication**.</span></span> <span data-ttu-id="b6d04-113">Hitelesítés a személy, aki használja az alkalmazás interakciót Media Services-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="b6d04-113">Authenticate a person who is using the app to interact with Media Services resources.</span></span> <span data-ttu-id="b6d04-114">Az interaktív alkalmazás először kell kérni a felhasználót a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b6d04-114">The interactive application should first prompt the user for credentials.</span></span> <span data-ttu-id="b6d04-115">Példa: egy kódolási feladatok figyelése, vagy live streaming jogosult felhasználók által használt felügyeleti konzol alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b6d04-115">An example is a management console app used by authorized users to monitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="b6d04-116">**Szolgáltatás egyszerű hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="b6d04-116">**Service principal authentication**.</span></span> <span data-ttu-id="b6d04-117">A szolgáltatás hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b6d04-117">Authenticate a service.</span></span> <span data-ttu-id="b6d04-118">Ezt a hitelesítési módszert gyakran használó alkalmazások olyan alkalmazások, amelyeket démon szolgáltatások, a középső rétegbeli szolgáltatások vagy az ütemezett feladatok futtatása: webalkalmazások, függvény alkalmazások, a logic apps, API-k vagy egy mikroszolgáltatási.</span><span class="sxs-lookup"><span data-stu-id="b6d04-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs: web apps, function apps, logic apps, APIs, or a microservice.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b6d04-119">A Media Services jelenleg az Azure hozzáférés-vezérlés szolgáltatásmodell-hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="b6d04-119">Currently, Media Services supports the Azure Access Control service authentication model.</span></span> <span data-ttu-id="b6d04-120">Azonban a 2018. június 1 elavulttá válik hozzáférés-vezérlés engedélyt.</span><span class="sxs-lookup"><span data-stu-id="b6d04-120">However, Access Control authorization will be deprecated on June 1, 2018.</span></span> <span data-ttu-id="b6d04-121">Azt javasoljuk, hogy telepítse át az Azure AD hitelesítési modell lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="b6d04-121">We recommend that you migrate to the Azure AD authentication model as soon as possible.</span></span>

## <a name="select-the-authentication-method"></a><span data-ttu-id="b6d04-122">A hitelesítési módszer kiválasztása</span><span class="sxs-lookup"><span data-stu-id="b6d04-122">Select the authentication method</span></span>

1. <span data-ttu-id="b6d04-123">Az a [Azure-portálon](https://portal.azure.com/), válassza ki a Media Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="b6d04-123">In the [Azure portal](https://portal.azure.com/), select your Media Services account.</span></span>
2. <span data-ttu-id="b6d04-124">Válassza ki a Kapcsolódás a Media Services API.</span><span class="sxs-lookup"><span data-stu-id="b6d04-124">Select how to connect to the Media Services API.</span></span>

    ![Válassza ki a csatlakozási módszer lapja](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a><span data-ttu-id="b6d04-126">Felhasználóhitelesítés</span><span class="sxs-lookup"><span data-stu-id="b6d04-126">User authentication</span></span>

<span data-ttu-id="b6d04-127">A Media Services API segítségével csatlakozni a felhasználói hitelesítést választja, az ügyfélalkalmazás van szüksége, kérje az Azure AD jogkivonatban az alábbi paraméterekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="b6d04-127">To connect to the Media Services API by using the user authentication option, the client app needs to request an Azure AD token that has the following parameters:</span></span>  

* <span data-ttu-id="b6d04-128">Azure AD-bérlő végpont</span><span class="sxs-lookup"><span data-stu-id="b6d04-128">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="b6d04-129">A Media Services erőforrás URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="b6d04-129">Media Services resource URI</span></span>
* <span data-ttu-id="b6d04-130">A Media Services (natív) alkalmazás ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="b6d04-130">Media Services (native) application client ID</span></span> 
* <span data-ttu-id="b6d04-131">Media Services (natív) alkalmazás átirányítási URI</span><span class="sxs-lookup"><span data-stu-id="b6d04-131">Media Services (native) application redirect URI</span></span> 
* <span data-ttu-id="b6d04-132">Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="b6d04-132">Resource URI for REST Media Services</span></span>

<span data-ttu-id="b6d04-133">Kaphat az értékeket a paraméterek a **Media Services API és a felhasználói hitelesítés** lap.</span><span class="sxs-lookup"><span data-stu-id="b6d04-133">You can get the values for these parameters on the **Media Services API with user authentication** page.</span></span> 

![Csatlakozás a felhasználói hitelesítés lap](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

<span data-ttu-id="b6d04-135">A Media Services Microsoft .NET SDK használatával kapcsolódik a Media Services API-t, ha a szükséges értékeket rendelkezésére álljanak a SDK részeként.</span><span class="sxs-lookup"><span data-stu-id="b6d04-135">If you connect to the Media Services API by using the Media Services Microsoft .NET SDK, the required values are available to you as part of the SDK.</span></span> <span data-ttu-id="b6d04-136">További információkért lásd: [elérni az Azure Media Services API a .NET hitelesítés használata az Azure AD](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="b6d04-136">For more information, see [Use Azure AD authentication to access the Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="b6d04-137">Ha nem használja a Media Services .NET SDK ügyfél, manuálisan kell létrehoznia az Azure AD-jogkivonatkérelem a korábban tárgyalt paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="b6d04-137">If you're not using the Media Services .NET client SDK, you must manually create an Azure AD token request by using the parameters discussed earlier.</span></span> <span data-ttu-id="b6d04-138">További információkért lásd: [hogyan használható az Azure AD Authentication Library az Azure AD-token beszerzése](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="b6d04-138">For more information, see [How to use the Azure AD Authentication Library to get the Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

## <a name="service-principal-authentication"></a><span data-ttu-id="b6d04-139">Egyszerű szolgáltatásnév hitelesítése</span><span class="sxs-lookup"><span data-stu-id="b6d04-139">Service principal authentication</span></span>

<span data-ttu-id="b6d04-140">Csatlakozás a Media Services API a szolgáltatás egyszerű lehetőséget, a középső rétegbeli (webes API-t vagy webalkalmazás) kell kérjen egy Azure AD-tokent, amely a következő paraméterekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="b6d04-140">To connect to the Media Services API by using the service principal option, your middle-tier app (web API or web application) needs to request an Azure AD token that has the following parameters:</span></span>  

* <span data-ttu-id="b6d04-141">Azure AD-bérlő végpont</span><span class="sxs-lookup"><span data-stu-id="b6d04-141">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="b6d04-142">A Media Services erőforrás URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="b6d04-142">Media Services resource URI</span></span> 
* <span data-ttu-id="b6d04-143">Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="b6d04-143">Resource URI for REST Media Services</span></span>
* <span data-ttu-id="b6d04-144">Az Azure AD alkalmazás értékeket: a **ügyfél-azonosító** és **ügyfélkulcs**</span><span class="sxs-lookup"><span data-stu-id="b6d04-144">Azure AD application values: the **client ID** and **client secret**</span></span>

<span data-ttu-id="b6d04-145">Kaphat az értékeket a paraméterek a **kapcsolódás a Media Services API-t a szolgáltatás egyszerű** lap.</span><span class="sxs-lookup"><span data-stu-id="b6d04-145">You can get the values for these parameters on the **Connect to Media Services API with service principal** page.</span></span> <span data-ttu-id="b6d04-146">Ezen a lapon hozzon létre egy új Azure AD-alkalmazást, vagy válasszon ki egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="b6d04-146">Use this page to create a new Azure AD application or to select an existing one.</span></span> <span data-ttu-id="b6d04-147">Miután kiválasztotta az Azure AD-alkalmazáshoz, az ügyfél-azonosítót (Alkalmazásazonosító), és az ügyfél titkos (kulcs) értékek generálásához.</span><span class="sxs-lookup"><span data-stu-id="b6d04-147">After you select the Azure AD app, you can get the client ID (Application ID) and generate the client secret (key) values.</span></span> 

![Csatlakozás szolgáltatás egyszerű lap](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

<span data-ttu-id="b6d04-149">Ha a **egyszerű** panel nyílik meg, az első Azure AD-alkalmazás, amely megfelel a következő elemet:</span><span class="sxs-lookup"><span data-stu-id="b6d04-149">When the **Service Principal** blade opens, the first Azure AD application that meets the following criteria is selected:</span></span>

- <span data-ttu-id="b6d04-150">Egy regisztrált Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b6d04-150">It is a registered Azure AD application.</span></span>
- <span data-ttu-id="b6d04-151">A fiók közreműködői vagy Owner Role-Based hozzáférés-vezérlés engedéllyel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b6d04-151">It has Contributor or Owner Role-Based Access Control permissions on the account.</span></span>

<span data-ttu-id="b6d04-152">Miután hoz létre, vagy válasszon ki egy Azure AD-alkalmazást, hozhat létre, és másolja át egy ügyfélkulcsot (kulcs) és az ügyfél-azonosító (Alkalmazásazonosító).</span><span class="sxs-lookup"><span data-stu-id="b6d04-152">After you create or select an Azure AD app, you can create and copy a client secret (key) and the client ID (Application ID).</span></span> <span data-ttu-id="b6d04-153">A titkos ügyfélkulcs és ügyfél-azonosító eléréséhez az ebben a forgatókönyvben token szükségesek.</span><span class="sxs-lookup"><span data-stu-id="b6d04-153">The client secret and client ID are required to get the access token in this scenario.</span></span>

<span data-ttu-id="b6d04-154">Ha nem rendelkezik engedélyekkel az Azure AD-alkalmazásai létrehozására a tartományban, a panel az Azure AD alkalmazás vezérlők nem láthatók, és megjelenik egy figyelmeztető üzenet.</span><span class="sxs-lookup"><span data-stu-id="b6d04-154">If you don't have permissions to create Azure AD apps in your domain, the Azure AD app controls of the blade are not shown, and a warning message is displayed.</span></span>

<span data-ttu-id="b6d04-155">Ha a Media Services .NET SDK használatával kapcsolódik a Media Services API-t, tekintse meg [elérni az Azure Media Services API a .NET hitelesítés használata az Azure AD](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="b6d04-155">If you connect to the Media Services API by using the Media Services .NET SDK, see [Use Azure AD authentication to access the Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="b6d04-156">Ha nem használ a Media Services .NET SDK ügyfél, manuálisan kell létrehoznia egy Azure AD-jogkivonatkérelem, a korábban tárgyalt paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="b6d04-156">If you are not using the Media Services .NET client SDK, you must manually create an Azure AD token request using the parameters discussed earlier.</span></span> <span data-ttu-id="b6d04-157">További információkért lásd: [hogyan használható az Azure AD Authentication Library az Azure AD-token beszerzése](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="b6d04-157">For more information, see [How to use the Azure AD Authentication Library to get the Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

### <a name="get-the-client-id-and-client-secret"></a><span data-ttu-id="b6d04-158">Az ügyfél-azonosító és a titkos ügyfélkódot beolvasása</span><span class="sxs-lookup"><span data-stu-id="b6d04-158">Get the client ID and client secret</span></span>

<span data-ttu-id="b6d04-159">Válasszon ki egy meglévő Azure AD-alkalmazást, vagy hozzon létre egy újat választja, a következő gombok jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="b6d04-159">After you select an existing Azure AD app or select the option to create a new one, the following buttons appear:</span></span>

![Engedélyek és kezelés alkalmazás gomb kezelése](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

<span data-ttu-id="b6d04-161">Az Azure AD alkalmazás panel megnyitásához kattintson **alkalmazás kezeléséhez**.</span><span class="sxs-lookup"><span data-stu-id="b6d04-161">To open the Azure AD application blade, click **Manage application**.</span></span> <span data-ttu-id="b6d04-162">Az a **alkalmazás kezeléséhez** panelen kaphat az alkalmazás ügyfél-azonosító (Alkalmazásazonosító).</span><span class="sxs-lookup"><span data-stu-id="b6d04-162">On the **Manage application** blade, you can get the app's client ID (Application ID).</span></span> <span data-ttu-id="b6d04-163">Egy ügyfélkulcsot (kulcs) létrehozásához, jelölje be **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="b6d04-163">To generate a client secret (key), select **Keys**.</span></span>

![Alkalmazás panel kulcsok beállítás kezelése](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-the-application"></a><span data-ttu-id="b6d04-165">Engedélyek és az alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="b6d04-165">Manage permissions and the application</span></span>

<span data-ttu-id="b6d04-166">Miután kiválasztotta az Azure AD-alkalmazás, kezelheti az alkalmazás és az engedélyek.</span><span class="sxs-lookup"><span data-stu-id="b6d04-166">After you select the Azure AD application, you can manage the application and permissions.</span></span> <span data-ttu-id="b6d04-167">Más alkalmazások elérése az Azure AD alkalmazás beállításához kattintson **kezeli az engedélyeket**.</span><span class="sxs-lookup"><span data-stu-id="b6d04-167">To set up your Azure AD application to access other applications, click **Manage permissions**.</span></span> <span data-ttu-id="b6d04-168">A fájlkezelési feladatok, például megváltoztatni a kulcsok és a válasz URL-címek, illetve módosíthatja az alkalmazás jegyzékében, kattintson a **alkalmazás kezeléséhez**.</span><span class="sxs-lookup"><span data-stu-id="b6d04-168">For management tasks, such as changing keys and reply URLs, or to edit the application’s manifest, click **Manage application**.</span></span>

### <a name="edit-the-apps-settings-or-manifest"></a><span data-ttu-id="b6d04-169">Az alkalmazás beállításainak módosítása vagy manifest</span><span class="sxs-lookup"><span data-stu-id="b6d04-169">Edit the app's settings or manifest</span></span>

<span data-ttu-id="b6d04-170">Az alkalmazás beállításai vagy jegyzékfájl szerkesztéséhez kattintson **alkalmazás kezeléséhez**.</span><span class="sxs-lookup"><span data-stu-id="b6d04-170">To edit the app's settings or manifest, click **Manage application**.</span></span>

![Alkalmazások lap kezelése](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a><span data-ttu-id="b6d04-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6d04-172">Next steps</span></span>

<span data-ttu-id="b6d04-173">Ismerkedés a [fájlok feltöltése a fiókjához](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="b6d04-173">Get started with [uploading files to your account](media-services-portal-upload-files.md).</span></span>
