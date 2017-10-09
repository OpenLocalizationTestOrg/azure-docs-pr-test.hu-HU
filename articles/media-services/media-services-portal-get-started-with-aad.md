---
title: "aaaGet használatába az Azure AD-alapú hitelesítés hello Azure-portál használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure portál tooaccess Azure Active Directory (Azure AD) hitelesítési tooconsume hello Azure Media Services API."
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
ms.openlocfilehash: 497ad1806b3fd1262802adefb6e12b65ee9f2d61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-hello-azure-portal"></a><span data-ttu-id="8f84a-103">Ismerkedés az Azure AD-alapú hitelesítés hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="8f84a-103">Get started with Azure AD authentication by using hello Azure portal</span></span>

<span data-ttu-id="8f84a-104">Ismerje meg, hogyan toouse hello Azure portál tooaccess Azure Active Directory (Azure AD) hitelesítési tooaccess hello Azure Media Services API.</span><span class="sxs-lookup"><span data-stu-id="8f84a-104">Learn how toouse hello Azure portal tooaccess Azure Active Directory (Azure AD) authentication tooaccess hello Azure Media Services API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f84a-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f84a-105">Prerequisites</span></span>

- <span data-ttu-id="8f84a-106">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="8f84a-106">An Azure account.</span></span> <span data-ttu-id="8f84a-107">Ha nincs fiókja, kezdje egy [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8f84a-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="8f84a-108">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="8f84a-108">A Media Services account.</span></span> <span data-ttu-id="8f84a-109">További információkért lásd: [Azure Media Services-fiók létrehozása hello Azure-portál használatával](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="8f84a-109">For more information, see [Create an Azure Media Services account by using hello Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="8f84a-110">Ellenőrizze, hogy tekintse át a hello [eléréséhez Azure Media Services API-t az Azure AD hitelesítési áttekintés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="8f84a-110">Make sure you review hello [Accessing Azure Media Services API with Azure AD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="8f84a-111">Az Azure AD-alapú hitelesítés használatakor az Azure Media Services hitelesítési két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="8f84a-111">When you use Azure AD authentication with Azure Media Services, you have two authentication options:</span></span>

- <span data-ttu-id="8f84a-112">**Felhasználó hitelesítése**.</span><span class="sxs-lookup"><span data-stu-id="8f84a-112">**User authentication**.</span></span> <span data-ttu-id="8f84a-113">Egy Media Services-erőforrásokkal hello app toointeract használó felhasználó hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8f84a-113">Authenticate a person who is using hello app toointeract with Media Services resources.</span></span> <span data-ttu-id="8f84a-114">hello interaktív alkalmazás először kell hello felhasználók hitelesítő adatainak kéréséhez.</span><span class="sxs-lookup"><span data-stu-id="8f84a-114">hello interactive application should first prompt hello user for credentials.</span></span> <span data-ttu-id="8f84a-115">Például egy engedéllyel rendelkező felhasználók toomonitor kódolási feladatok által használt felügyeleti konzol alkalmazás vagy élő adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="8f84a-115">An example is a management console app used by authorized users toomonitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="8f84a-116">**Szolgáltatás egyszerű hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="8f84a-116">**Service principal authentication**.</span></span> <span data-ttu-id="8f84a-117">A szolgáltatás hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8f84a-117">Authenticate a service.</span></span> <span data-ttu-id="8f84a-118">Ezt a hitelesítési módszert gyakran használó alkalmazások olyan alkalmazások, amelyeket démon szolgáltatások, a középső rétegbeli szolgáltatások vagy az ütemezett feladatok futtatása: webalkalmazások, függvény alkalmazások, a logic apps, API-k vagy egy mikroszolgáltatási.</span><span class="sxs-lookup"><span data-stu-id="8f84a-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs: web apps, function apps, logic apps, APIs, or a microservice.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f84a-119">Jelenleg a Media Services hello Azure Access Control service hitelesítési modellt támogatja.</span><span class="sxs-lookup"><span data-stu-id="8f84a-119">Currently, Media Services supports hello Azure Access Control service authentication model.</span></span> <span data-ttu-id="8f84a-120">Azonban a 2018. június 1 elavulttá válik hozzáférés-vezérlés engedélyt.</span><span class="sxs-lookup"><span data-stu-id="8f84a-120">However, Access Control authorization will be deprecated on June 1, 2018.</span></span> <span data-ttu-id="8f84a-121">Azt javasoljuk, hogy az áttelepített toohello az Azure AD hitelesítési modell lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="8f84a-121">We recommend that you migrate toohello Azure AD authentication model as soon as possible.</span></span>

## <a name="select-hello-authentication-method"></a><span data-ttu-id="8f84a-122">Hello hitelesítési módszer kiválasztása</span><span class="sxs-lookup"><span data-stu-id="8f84a-122">Select hello authentication method</span></span>

1. <span data-ttu-id="8f84a-123">A hello [Azure-portálon](https://portal.azure.com/), válassza ki a Media Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="8f84a-123">In hello [Azure portal](https://portal.azure.com/), select your Media Services account.</span></span>
2. <span data-ttu-id="8f84a-124">Adja meg, hogyan tooconnect toohello Media Services API.</span><span class="sxs-lookup"><span data-stu-id="8f84a-124">Select how tooconnect toohello Media Services API.</span></span>

    ![Válassza ki a csatlakozási módszer lapja](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a><span data-ttu-id="8f84a-126">Felhasználóhitelesítés</span><span class="sxs-lookup"><span data-stu-id="8f84a-126">User authentication</span></span>

<span data-ttu-id="8f84a-127">tooconnect toohello Media Services API használatával hello felhasználói hitelesítést választja, hello ügyfélalkalmazás kell toorequest az Azure AD jogkivonatban hello rendelkezik a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="8f84a-127">tooconnect toohello Media Services API by using hello user authentication option, hello client app needs toorequest an Azure AD token that has hello following parameters:</span></span>  

* <span data-ttu-id="8f84a-128">Azure AD-bérlő végpont</span><span class="sxs-lookup"><span data-stu-id="8f84a-128">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="8f84a-129">A Media Services erőforrás URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="8f84a-129">Media Services resource URI</span></span>
* <span data-ttu-id="8f84a-130">A Media Services (natív) alkalmazás ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="8f84a-130">Media Services (native) application client ID</span></span> 
* <span data-ttu-id="8f84a-131">Media Services (natív) alkalmazás átirányítási URI</span><span class="sxs-lookup"><span data-stu-id="8f84a-131">Media Services (native) application redirect URI</span></span> 
* <span data-ttu-id="8f84a-132">Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="8f84a-132">Resource URI for REST Media Services</span></span>

<span data-ttu-id="8f84a-133">Ezek a paraméterek a hello kaphat hello értékek **Media Services API és a felhasználói hitelesítés** lap.</span><span class="sxs-lookup"><span data-stu-id="8f84a-133">You can get hello values for these parameters on hello **Media Services API with user authentication** page.</span></span> 

![Csatlakozás a felhasználói hitelesítés lap](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

<span data-ttu-id="8f84a-135">Ha toohello Media Services API hello Media Services Microsoft .NET SDK használatával, a hello szükséges értékei elérhető tooyou hello SDK részeként.</span><span class="sxs-lookup"><span data-stu-id="8f84a-135">If you connect toohello Media Services API by using hello Media Services Microsoft .NET SDK, hello required values are available tooyou as part of hello SDK.</span></span> <span data-ttu-id="8f84a-136">További információkért lásd: [használja az Azure AD hitelesítési tooaccess hello Azure Media Services API a .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="8f84a-136">For more information, see [Use Azure AD authentication tooaccess hello Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="8f84a-137">Ha ügyfél-hello Media Services .NET SDK nem használ, manuálisan kell létrehoznia az Azure AD-jogkivonatkérelem taglaltak hello paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="8f84a-137">If you're not using hello Media Services .NET client SDK, you must manually create an Azure AD token request by using hello parameters discussed earlier.</span></span> <span data-ttu-id="8f84a-138">További információkért lásd: [hogyan toouse hello Azure AD Authentication Library tooget hello Azure AD-jogkivonat](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="8f84a-138">For more information, see [How toouse hello Azure AD Authentication Library tooget hello Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

## <a name="service-principal-authentication"></a><span data-ttu-id="8f84a-139">Egyszerű szolgáltatásnév hitelesítése</span><span class="sxs-lookup"><span data-stu-id="8f84a-139">Service principal authentication</span></span>

<span data-ttu-id="8f84a-140">tooconnect toohello Media Services API használatával hello szolgáltatás egyszerű beállítás, a középső rétegbeli (webes API-t vagy webalkalmazás) kell toorequest az Azure AD jogkivonatban hello rendelkezik a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="8f84a-140">tooconnect toohello Media Services API by using hello service principal option, your middle-tier app (web API or web application) needs toorequest an Azure AD token that has hello following parameters:</span></span>  

* <span data-ttu-id="8f84a-141">Azure AD-bérlő végpont</span><span class="sxs-lookup"><span data-stu-id="8f84a-141">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="8f84a-142">A Media Services erőforrás URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="8f84a-142">Media Services resource URI</span></span> 
* <span data-ttu-id="8f84a-143">Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="8f84a-143">Resource URI for REST Media Services</span></span>
* <span data-ttu-id="8f84a-144">Az Azure AD alkalmazás értékek: hello **ügyfél-azonosító** és **ügyfélkulcs**</span><span class="sxs-lookup"><span data-stu-id="8f84a-144">Azure AD application values: hello **client ID** and **client secret**</span></span>

<span data-ttu-id="8f84a-145">Ezek a paraméterek a hello kaphat hello értékek **tooMedia szolgáltatások API kapcsolattartásnak szolgáltatás egyszerű** lap.</span><span class="sxs-lookup"><span data-stu-id="8f84a-145">You can get hello values for these parameters on hello **Connect tooMedia Services API with service principal** page.</span></span> <span data-ttu-id="8f84a-146">Használja az oldal toocreate egy új Azure AD-alkalmazást vagy tooselect egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="8f84a-146">Use this page toocreate a new Azure AD application or tooselect an existing one.</span></span> <span data-ttu-id="8f84a-147">Miután kiválasztotta a hello Azure AD-alkalmazáshoz, hello ügyfél-azonosítót (Alkalmazásazonosító), és hello ügyfél titkos (kulcs) értékek generálásához.</span><span class="sxs-lookup"><span data-stu-id="8f84a-147">After you select hello Azure AD app, you can get hello client ID (Application ID) and generate hello client secret (key) values.</span></span> 

![Csatlakozás szolgáltatás egyszerű lap](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

<span data-ttu-id="8f84a-149">Ha hello **egyszerű** panel nyílik meg, a kiválasztott hello első Azure AD alkalmazás, amely megfelel a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="8f84a-149">When hello **Service Principal** blade opens, hello first Azure AD application that meets hello following criteria is selected:</span></span>

- <span data-ttu-id="8f84a-150">Egy regisztrált Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8f84a-150">It is a registered Azure AD application.</span></span>
- <span data-ttu-id="8f84a-151">Hello fiók közreműködői vagy Owner Role-Based hozzáférés-vezérlés engedéllyel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8f84a-151">It has Contributor or Owner Role-Based Access Control permissions on hello account.</span></span>

<span data-ttu-id="8f84a-152">Miután hoz létre, vagy válasszon ki egy Azure AD-alkalmazást, hozzon létre és másolja át egy ügyfélkulcsot (kulcs), és hello ügyfél-azonosító (Alkalmazásazonosító).</span><span class="sxs-lookup"><span data-stu-id="8f84a-152">After you create or select an Azure AD app, you can create and copy a client secret (key) and hello client ID (Application ID).</span></span> <span data-ttu-id="8f84a-153">hello ügyfél titkos kulcs és az ügyfél-azonosító szükséges tooget hello hozzáférési jogkivonat ebben a forgatókönyvben is.</span><span class="sxs-lookup"><span data-stu-id="8f84a-153">hello client secret and client ID are required tooget hello access token in this scenario.</span></span>

<span data-ttu-id="8f84a-154">Ha nem rendelkezik engedélyekkel toocreate Azure AD alkalmazásaiban abban a tartományban, hello Azure AD alkalmazás vezérlők hello panelről nem láthatók, és megjelenik egy figyelmeztető üzenet.</span><span class="sxs-lookup"><span data-stu-id="8f84a-154">If you don't have permissions toocreate Azure AD apps in your domain, hello Azure AD app controls of hello blade are not shown, and a warning message is displayed.</span></span>

<span data-ttu-id="8f84a-155">Ha toohello Media Services API hello Media Services .NET SDK használatával, lásd: [használja az Azure AD hitelesítési tooaccess hello Azure Media Services API a .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="8f84a-155">If you connect toohello Media Services API by using hello Media Services .NET SDK, see [Use Azure AD authentication tooaccess hello Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="8f84a-156">Ha nem használ hello Media Services .NET ügyfél SDK, manuálisan kell létrehoznia egy Azure AD-jogkivonatkérelem, azt a korábbiakban említettük hello paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="8f84a-156">If you are not using hello Media Services .NET client SDK, you must manually create an Azure AD token request using hello parameters discussed earlier.</span></span> <span data-ttu-id="8f84a-157">További információkért lásd: [hogyan toouse hello Azure AD Authentication Library tooget hello Azure AD-jogkivonat](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="8f84a-157">For more information, see [How toouse hello Azure AD Authentication Library tooget hello Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

### <a name="get-hello-client-id-and-client-secret"></a><span data-ttu-id="8f84a-158">Hello ügyfél azonosító és az ügyfél titkos kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="8f84a-158">Get hello client ID and client secret</span></span>

<span data-ttu-id="8f84a-159">Miután kiválasztott egy meglévő Azure AD-alkalmazáshoz vagy select hello beállítás toocreate egy új, a következő gombok hello jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="8f84a-159">After you select an existing Azure AD app or select hello option toocreate a new one, hello following buttons appear:</span></span>

![Engedélyek és kezelés alkalmazás gomb kezelése](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

<span data-ttu-id="8f84a-161">tooopen hello Azure AD-alkalmazás paneljén kattintson **alkalmazás kezeléséhez**.</span><span class="sxs-lookup"><span data-stu-id="8f84a-161">tooopen hello Azure AD application blade, click **Manage application**.</span></span> <span data-ttu-id="8f84a-162">A hello **alkalmazás kezeléséhez** panelen kaphat a hello alkalmazás ügyfél-Azonosítóját (Alkalmazásazonosító).</span><span class="sxs-lookup"><span data-stu-id="8f84a-162">On hello **Manage application** blade, you can get hello app's client ID (Application ID).</span></span> <span data-ttu-id="8f84a-163">a titkos ügyfélkulcsot (kulcs), jelölje be toogenerate **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="8f84a-163">toogenerate a client secret (key), select **Keys**.</span></span>

![Alkalmazás panel kulcsok beállítás kezelése](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-hello-application"></a><span data-ttu-id="8f84a-165">Engedélyek és hello alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="8f84a-165">Manage permissions and hello application</span></span>

<span data-ttu-id="8f84a-166">Miután kiválasztotta a hello Azure AD-alkalmazást, kezelheti a hello alkalmazás és az engedélyek.</span><span class="sxs-lookup"><span data-stu-id="8f84a-166">After you select hello Azure AD application, you can manage hello application and permissions.</span></span> <span data-ttu-id="8f84a-167">tooset mentése az Azure AD alkalmazás tooaccess más alkalmazásokat, kattintson a **kezeli az engedélyeket**.</span><span class="sxs-lookup"><span data-stu-id="8f84a-167">tooset up your Azure AD application tooaccess other applications, click **Manage permissions**.</span></span> <span data-ttu-id="8f84a-168">A felügyeleti feladatokat, például megváltoztatni a kulcsok és a válasz URL-címek vagy tooedit hello alkalmazás jegyzékfájlja, kattintson a **alkalmazás kezeléséhez**.</span><span class="sxs-lookup"><span data-stu-id="8f84a-168">For management tasks, such as changing keys and reply URLs, or tooedit hello application’s manifest, click **Manage application**.</span></span>

### <a name="edit-hello-apps-settings-or-manifest"></a><span data-ttu-id="8f84a-169">Hello alkalmazás beállításait vagy manifest</span><span class="sxs-lookup"><span data-stu-id="8f84a-169">Edit hello app's settings or manifest</span></span>

<span data-ttu-id="8f84a-170">tooedit hello alkalmazás beállításai vagy jegyzékfájl, kattintson a **alkalmazás kezeléséhez**.</span><span class="sxs-lookup"><span data-stu-id="8f84a-170">tooedit hello app's settings or manifest, click **Manage application**.</span></span>

![Alkalmazások lap kezelése](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a><span data-ttu-id="8f84a-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f84a-172">Next steps</span></span>

<span data-ttu-id="8f84a-173">Ismerkedés a [fájlok feltöltése a tooyour fiók](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="8f84a-173">Get started with [uploading files tooyour account](media-services-portal-upload-files.md).</span></span>
