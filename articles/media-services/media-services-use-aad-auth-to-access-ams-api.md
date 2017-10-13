---
title: "Azure Active Directory-hitelesítés az Azure Media Services API aaaAccess |} Microsoft Docs"
description: "További információk a fogalmakat és lépéseket tootake toouse Azure Active Directory (Azure AD) tooauthenticate hozzáférés toohello Azure Media Services API."
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
ms.openlocfilehash: bb8f75f39100dc37098260c24ab4a199ef689d46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-hello-azure-media-services-api-with-azure-ad-authentication"></a><span data-ttu-id="1b606-103">Hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1b606-103">Access hello Azure Media Services API with Azure AD authentication</span></span>
 
<span data-ttu-id="1b606-104">Azure Media Services API hello RESTful API.</span><span class="sxs-lookup"><span data-stu-id="1b606-104">hello Azure Media Services API is a RESTful API.</span></span> <span data-ttu-id="1b606-105">Használhatja tooperform media erőforrásokon végrehajtott műveletek a REST API használatával, vagy elérhető ügyfél SDK-k használatával.</span><span class="sxs-lookup"><span data-stu-id="1b606-105">You can use it tooperform operations on media resources by using a REST API or by using available client SDKs.</span></span> <span data-ttu-id="1b606-106">Az Azure Media Services egy Media Services-ügyfél SDK kínál a Microsoft .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="1b606-106">Azure Media Services offers a Media Services client SDK for Microsoft .NET.</span></span> <span data-ttu-id="1b606-107">toobe tooaccess Media Services erőforrások és a Media Services API hello engedélyezett, akkor először hitelesíteni kell.</span><span class="sxs-lookup"><span data-stu-id="1b606-107">toobe authorized tooaccess Media Services resources and hello Media Services API, you must first be authenticated.</span></span> 

<span data-ttu-id="1b606-108">Media Services szolgáltatásban [Azure Active Directory (Azure AD)-alapú hitelesítés](../active-directory/active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-108">Media Services supports [Azure Active Directory (Azure AD)-based authentication](../active-directory/active-directory-whatis.md).</span></span> <span data-ttu-id="1b606-109">Azure Media REST-szolgáltatást hello megköveteli, hogy hello felhasználó vagy alkalmazás, amely lehetővé teszi a REST API-kérés hello vagy hello **közreműködő** vagy **tulajdonos** szerepkör tooaccess hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1b606-109">hello Azure Media REST service requires that hello user or application that makes hello REST API requests have either hello **Contributor** or **Owner** role tooaccess hello resources.</span></span> <span data-ttu-id="1b606-110">További információkért lásd: [Ismerkedés a szerepköralapú hozzáférés-vezérlés az Azure-portálon hello](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-110">For more information, see [Get started with Role-Based Access Control in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="1b606-111">Jelenleg a Media Services hello Azure Access Control service hitelesítési modellt támogatja.</span><span class="sxs-lookup"><span data-stu-id="1b606-111">Currently, Media Services supports hello Azure Access Control service authentication model.</span></span> <span data-ttu-id="1b606-112">Azonban a 2018. június 1 elavulttá válik hozzáférés-vezérlés engedélyt.</span><span class="sxs-lookup"><span data-stu-id="1b606-112">However, Access Control authorization will be deprecated on June 1, 2018.</span></span> <span data-ttu-id="1b606-113">Azt javasoljuk, hogy az áttelepített toohello az Azure AD hitelesítési modell lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="1b606-113">We recommend that you migrate toohello Azure AD authentication model as soon as possible.</span></span>

<span data-ttu-id="1b606-114">Ez a dokumentum áttekintést hogyan tooaccess hello REST vagy a .NET API-k használatával Media Services API.</span><span class="sxs-lookup"><span data-stu-id="1b606-114">This document gives an overview of how tooaccess hello Media Services API by using REST or .NET APIs.</span></span>

## <a name="access-control"></a><span data-ttu-id="1b606-115">Hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="1b606-115">Access control</span></span>

<span data-ttu-id="1b606-116">Hello Azure Media REST kérelem toosucceed hello hívó felhasználói hello tooaccess próbál Media Services-fiók közreműködői vagy a tulajdonos szerepkört kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1b606-116">For hello Azure Media REST request toosucceed, hello calling user must have a Contributor or Owner role for hello Media Services account it is trying tooaccess.</span></span>  
<span data-ttu-id="1b606-117">Csak hello tulajdonos szerepkörrel rendelkező felhasználó media erőforrás (fiók) hozzáférési toonew felhasználók vagy alkalmazások biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="1b606-117">Only a user with hello Owner role can give media resource (account) access toonew users or apps.</span></span> <span data-ttu-id="1b606-118">hello közreműködői szerepkör elér csak hello média-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="1b606-118">hello Contributor role can access only hello media resource.</span></span>
<span data-ttu-id="1b606-119">Jogosulatlan kérelem sikertelen lesz, a 401-es állapotkóddal.</span><span class="sxs-lookup"><span data-stu-id="1b606-119">Unauthorized requests fail, with status code of 401.</span></span> <span data-ttu-id="1b606-120">Ha hibaüzenet jelenik meg, ellenőrizze, hogy a felhasználó van-e a hello közreműködő vagy a tulajdonos szerepkörrel hello felhasználói Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="1b606-120">If you see this error code, check whether your user has hello Contributor or Owner role assigned for hello user's Media Services account.</span></span> <span data-ttu-id="1b606-121">Az Azure-portálon hello ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="1b606-121">You can check this in hello Azure portal.</span></span> <span data-ttu-id="1b606-122">Keresse meg az adathordozó-fiókját, és kattintson a hello **hozzáférés-vezérlés** fülre.</span><span class="sxs-lookup"><span data-stu-id="1b606-122">Search for your media account, and then click hello **Access control** tab.</span></span> 

![Hozzáférés-vezérlési lap](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a><span data-ttu-id="1b606-124">Típusú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1b606-124">Types of authentication</span></span> 
 
<span data-ttu-id="1b606-125">Az Azure AD-alapú hitelesítés használatakor az Azure Media Services hitelesítési két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="1b606-125">When you use Azure AD authentication with Azure Media Services, you have two authentication options:</span></span>

- <span data-ttu-id="1b606-126">**Felhasználó hitelesítése**.</span><span class="sxs-lookup"><span data-stu-id="1b606-126">**User authentication**.</span></span> <span data-ttu-id="1b606-127">Egy Media Services-erőforrásokkal hello app toointeract használó felhasználó hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1b606-127">Authenticate a person who is using hello app toointeract with Media Services resources.</span></span> <span data-ttu-id="1b606-128">hello interaktív alkalmazás először kell hello felhasználók hello felhasználó hitelesítő adatainak kéréséhez.</span><span class="sxs-lookup"><span data-stu-id="1b606-128">hello interactive application should first prompt hello user for hello user's credentials.</span></span> <span data-ttu-id="1b606-129">Például egy engedéllyel rendelkező felhasználók toomonitor kódolási feladatok által használt felügyeleti konzol alkalmazás vagy élő adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="1b606-129">An example is a management console app used by authorized users toomonitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="1b606-130">**Szolgáltatás egyszerű hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="1b606-130">**Service principal authentication**.</span></span> <span data-ttu-id="1b606-131">A szolgáltatás hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1b606-131">Authenticate a service.</span></span> <span data-ttu-id="1b606-132">Ezt a hitelesítési módszert gyakran használó alkalmazások olyan alkalmazások, démon szolgáltatások, a középső rétegbeli szolgáltatások vagy az ütemezett feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1b606-132">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs.</span></span> <span data-ttu-id="1b606-133">Többek között a webalkalmazások, függvény alkalmazások, a logic apps, API és mikroszolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1b606-133">Examples are web apps, function apps, logic apps, API, and microservices.</span></span>

### <a name="user-authentication"></a><span data-ttu-id="1b606-134">Felhasználóhitelesítés</span><span class="sxs-lookup"><span data-stu-id="1b606-134">User authentication</span></span> 

<span data-ttu-id="1b606-135">Hello felhasználói hitelesítési módszert kell használó alkalmazások: a felügyeleti vagy a natív alkalmazások figyeléséről: mobilalkalmazások, a Windows-alkalmazások és a konzol alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="1b606-135">Applications that should use hello user authentication method are management or monitoring native apps: mobile apps, Windows apps, and Console applications.</span></span> <span data-ttu-id="1b606-136">Az ilyen típusú megoldást akkor hasznos, ha azt szeretné, hogy valamelyik a következő forgatókönyvek hello hello szolgáltatásban emberi beavatkozást igényel:</span><span class="sxs-lookup"><span data-stu-id="1b606-136">This type of solution is useful when you want human interaction with hello service in one of hello following scenarios:</span></span>

- <span data-ttu-id="1b606-137">Figyelési irányítópult a kódolási feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="1b606-137">Monitoring dashboard for your encoding jobs.</span></span>
- <span data-ttu-id="1b606-138">Az élő adatfolyamok a figyelési irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="1b606-138">Monitoring dashboard for your live streams.</span></span>
- <span data-ttu-id="1b606-139">Az asztali és mobil felhasználók tooadminister erőforrások a Media Services-fiók alkalmazás kezelése.</span><span class="sxs-lookup"><span data-stu-id="1b606-139">Management application for desktop or mobile users tooadminister resources in a Media Services account.</span></span>

> [!NOTE]
> <span data-ttu-id="1b606-140">Ezt a hitelesítési módszert felhasználók felé néző alkalmazásokban kell nem használhatók.</span><span class="sxs-lookup"><span data-stu-id="1b606-140">This authentication method should not be used for consumer-facing applications.</span></span> 

<span data-ttu-id="1b606-141">Egy natív alkalmazás kell először szerezni az Azure ad-hozzáférési tokent, és használja azt meg, hogy a HTTP kérelmeket toohello Media Services REST API-t.</span><span class="sxs-lookup"><span data-stu-id="1b606-141">A native application must first acquire an access token from Azure AD, and then use it when you make HTTP requests toohello Media Services REST API.</span></span> <span data-ttu-id="1b606-142">Adja hozzá a hello access token toohello kérelemfejlécet.</span><span class="sxs-lookup"><span data-stu-id="1b606-142">Add hello access token toohello request header.</span></span> 

<span data-ttu-id="1b606-143">a következő diagram hello egy interaktív alkalmazások hitelesítési folyamat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="1b606-143">hello following diagram shows a typical interactive application authentication flow:</span></span> 

![Natív alkalmazások diagramja](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

<span data-ttu-id="1b606-145">A fenti diagram hello hello számok jelölik hello folyamata hello kérelmek időrendi sorrendben.</span><span class="sxs-lookup"><span data-stu-id="1b606-145">In hello preceding diagram, hello numbers represent hello flow of hello requests in chronological order.</span></span>

> [!NOTE]
> <span data-ttu-id="1b606-146">Hello felhasználói hitelesítési módszer használata esetén minden alkalmazás megosztása hello natív alkalmazás ügyfél-azonosító ugyanazon (alapértelmezett) és a natív alkalmazás átirányítási URI-címe.</span><span class="sxs-lookup"><span data-stu-id="1b606-146">When you use hello user authentication method, all apps share hello same (default) native application client ID and native application redirect URI.</span></span> 

1. <span data-ttu-id="1b606-147">A felhasználók hitelesítő adatainak kéréséhez.</span><span class="sxs-lookup"><span data-stu-id="1b606-147">Prompt a user for credentials.</span></span>
2. <span data-ttu-id="1b606-148">Egy Azure AD hozzáférési jogkivonat a következő paraméterek hello. kérelem:</span><span class="sxs-lookup"><span data-stu-id="1b606-148">Request an Azure AD access token with hello following parameters:</span></span>  

    * <span data-ttu-id="1b606-149">Azure AD-bérlő végpont.</span><span class="sxs-lookup"><span data-stu-id="1b606-149">Azure AD tenant endpoint.</span></span>

        <span data-ttu-id="1b606-150">hello bérlői adatait lekérhetők hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1b606-150">hello tenant information can be retrieved from hello Azure portal.</span></span> <span data-ttu-id="1b606-151">Vigye a kurzort keresztül hello bejelentkezett felhasználó neve hello hello jobb felső sarokban található.</span><span class="sxs-lookup"><span data-stu-id="1b606-151">Place your cursor over hello name of hello signed-in user in hello top right corner.</span></span>
    * <span data-ttu-id="1b606-152">A Media Services erőforrás URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1b606-152">Media Services resource URI.</span></span> 

        <span data-ttu-id="1b606-153">Ezt az URI van hello azonos hello a Media Services-fiókok Azure ugyanabban a környezetben (például https://rest.media.azure.net).</span><span class="sxs-lookup"><span data-stu-id="1b606-153">This URI is hello same for Media Services accounts that are in hello same Azure environment (for example, https://rest.media.azure.net).</span></span>

    * <span data-ttu-id="1b606-154">A Media Services (natív) alkalmazás ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="1b606-154">Media Services (native) application client ID.</span></span>
    * <span data-ttu-id="1b606-155">Media Services (natív) alkalmazás átirányítási URI-címe.</span><span class="sxs-lookup"><span data-stu-id="1b606-155">Media Services (native) application redirect URI.</span></span>
    * <span data-ttu-id="1b606-156">Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="1b606-156">Resource URI for REST Media Services.</span></span>
        
        <span data-ttu-id="1b606-157">hello URI hello REST API-végpont (például https://test03.restv2.westus.media.azure.net/api/) jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b606-157">hello URI represents hello REST API endpoint (for example, https://test03.restv2.westus.media.azure.net/api/).</span></span>

    <span data-ttu-id="1b606-158">Ezek a paraméterek értékeit tooget lásd [hello Azure portál tooaccess az Azure AD hitelesítési beállítások](media-services-portal-get-started-with-aad.md) hello felhasználói hitelesítés beállítás használatával.</span><span class="sxs-lookup"><span data-stu-id="1b606-158">tooget values for these parameters, see [Use hello Azure portal tooaccess Azure AD authentication settings](media-services-portal-get-started-with-aad.md) using hello user authentication option.</span></span>

3. <span data-ttu-id="1b606-159">hello Azure AD hozzáférési jogkivonat toohello ügyfél küldött.</span><span class="sxs-lookup"><span data-stu-id="1b606-159">hello Azure AD access token is sent toohello client.</span></span>
4. <span data-ttu-id="1b606-160">hello ügyfél küldi hello Azure AD hozzáférési jogkivonatok kérelem toohello Azure Media REST API-t.</span><span class="sxs-lookup"><span data-stu-id="1b606-160">hello client sends a request toohello Azure Media REST API with hello Azure AD access token.</span></span>
5. <span data-ttu-id="1b606-161">hello ügyfél vissza hello adatok lekérése a Media Services.</span><span class="sxs-lookup"><span data-stu-id="1b606-161">hello client gets back hello data from Media Services.</span></span>

<span data-ttu-id="1b606-162">További információ a hogyan toouse az Azure AD hitelesítési toocommunicate többi hello ügyfél-Media Services .NET SDK használatával kér: [használja az Azure AD hitelesítési tooaccess hello Media Services API a .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-162">For information about how toouse Azure AD authentication toocommunicate with REST requests by using hello Media Services .NET client SDK, see [Use Azure AD authentication tooaccess hello Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span> 

<span data-ttu-id="1b606-163">Ha nem használ hello Media Services .NET ügyfél SDK, manuálisan kell létrehoznia az Azure AD hozzáférési jogkivonat kérelem a 2. lépésben leírt hello paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="1b606-163">If you are not using hello Media Services .NET client SDK, you must manually create an Azure AD access token request by using hello parameters described in step 2.</span></span> <span data-ttu-id="1b606-164">További információkért lásd: [hogyan toouse hello Azure AD Authentication Library tooget hello Azure AD-jogkivonat](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-164">For more information, see [How toouse hello Azure AD Authentication Library tooget hello Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

### <a name="service-principal-authentication"></a><span data-ttu-id="1b606-165">Egyszerű szolgáltatásnév hitelesítése</span><span class="sxs-lookup"><span data-stu-id="1b606-165">Service principal authentication</span></span>

<span data-ttu-id="1b606-166">Ezt a hitelesítési módszert gyakran használó alkalmazások olyan alkalmazások, amelyeket a középső rétegbeli szolgáltatások és az ütemezett feladatok futtatásához: web apps, függvény alkalmazások, a logic apps, API-k és mikroszolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1b606-166">Applications that commonly use this authentication method are apps that run middle-tier services and scheduled jobs: web apps, function apps, logic apps, APIs, and microservices.</span></span> <span data-ttu-id="1b606-167">Ezt a hitelesítési módszert is alkalmas lehet, hogy ki toouse a szolgáltatás fiók toomanage erőforrásainak interaktív alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="1b606-167">This authentication method also is suitable for interactive applications in which you might want toouse a service account toomanage resources.</span></span>

<span data-ttu-id="1b606-168">Hello szolgáltatás egyszerű hitelesítési módszer toobuild fogyasztói forgatókönyveket használatakor hitelesítési általában kell kezelni a középső réteg hello (néhány API-n) keresztül, és nem közvetlenül a hordozható vagy asztali alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1b606-168">When you use hello service principal authentication method toobuild consumer scenarios, authentication typically is handled in hello middle tier (through some API) and not directly in a mobile or desktop application.</span></span> 

<span data-ttu-id="1b606-169">toouse ezzel a módszerrel hozzon létre egy Azure AD-alkalmazást, és a saját bérlőt az egyszerű szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1b606-169">toouse this method, create an Azure AD application and service principal in its own tenant.</span></span> <span data-ttu-id="1b606-170">Hello alkalmazás létrehozása után adjon hello app közreműködő vagy a tulajdonos szerepkör hozzáférés toohello Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="1b606-170">After you create hello application, give hello app Contributor or Owner role access toohello Media Services account.</span></span> <span data-ttu-id="1b606-171">Ehhez a hello Azure-portálon az Azure parancssori felület használatával, vagy egy PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="1b606-171">You can do this in hello Azure portal, by using Azure CLI, or with a PowerShell script.</span></span> <span data-ttu-id="1b606-172">Egy Azure AD-alkalmazást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="1b606-172">You also can use an existing Azure AD application.</span></span> <span data-ttu-id="1b606-173">Regisztrálhat és kezeli az Azure AD alkalmazás- és egyszerű szolgáltatás [hello Azure-portálon a](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-173">You can register and manage your Azure AD app and service principal [in hello Azure portal](media-services-portal-get-started-with-aad.md).</span></span> <span data-ttu-id="1b606-174">Is ehhez a [Azure CLI 2.0](media-services-use-aad-auth-to-access-ams-api.md) vagy [PowerShell](media-services-powershell-create-and-configure-aad-app.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-174">You also can do this by using [Azure CLI 2.0](media-services-use-aad-auth-to-access-ams-api.md) or [PowerShell](media-services-powershell-create-and-configure-aad-app.md).</span></span> 

![Középső rétegbeli alkalmazások](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

<span data-ttu-id="1b606-176">Miután létrehozta az Azure AD-alkalmazást, a következő beállítások hello beolvasása értékek.</span><span class="sxs-lookup"><span data-stu-id="1b606-176">After you create your Azure AD application, you get values for hello following settings.</span></span> <span data-ttu-id="1b606-177">A hitelesítéshez kell ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="1b606-177">You need these values for authentication:</span></span>

- <span data-ttu-id="1b606-178">Ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="1b606-178">Client ID</span></span> 
- <span data-ttu-id="1b606-179">Titkos ügyfélkulcs</span><span class="sxs-lookup"><span data-stu-id="1b606-179">Client secret</span></span> 

<span data-ttu-id="1b606-180">A fenti ábra hello hello számok hello kérelmek időrendben hello áramló mutatják be:</span><span class="sxs-lookup"><span data-stu-id="1b606-180">In hello preceding figure, hello numbers represent hello flow of hello requests in chronological order:</span></span>
    
1. <span data-ttu-id="1b606-181">A középső rétegbeli alkalmazás (webes API-t vagy webalkalmazás) egy Azure AD hozzáférési jogkivonatot, amely rendelkezik a következő paraméterek hello kéri:</span><span class="sxs-lookup"><span data-stu-id="1b606-181">A middle-tier app (web API or web application) requests an Azure AD access token that has hello following parameters:</span></span>  

    * <span data-ttu-id="1b606-182">Azure AD-bérlő végpont.</span><span class="sxs-lookup"><span data-stu-id="1b606-182">Azure AD tenant endpoint.</span></span>

        <span data-ttu-id="1b606-183">hello bérlői adatait lekérhetők hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1b606-183">hello tenant information can be retrieved from hello Azure portal.</span></span> <span data-ttu-id="1b606-184">Vigye a kurzort keresztül hello bejelentkezett felhasználó neve hello hello jobb felső sarokban található.</span><span class="sxs-lookup"><span data-stu-id="1b606-184">Place your cursor over hello name of hello signed-in user in hello top right corner.</span></span>
    * <span data-ttu-id="1b606-185">A Media Services erőforrás URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1b606-185">Media Services resource URI.</span></span> 

        <span data-ttu-id="1b606-186">Ezt az URI van hello azonos hello találhatók a Media Services-fiókok Azure ugyanabban a környezetben (például https://rest.media.azure.net).</span><span class="sxs-lookup"><span data-stu-id="1b606-186">This URI is hello same for Media Services accounts that are located in hello same Azure environment (for example, https://rest.media.azure.net).</span></span>

    * <span data-ttu-id="1b606-187">Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="1b606-187">Resource URI for REST Media Services.</span></span>

        <span data-ttu-id="1b606-188">hello URI hello REST API-végpont (például https://test03.restv2.westus.media.azure.net/api/) jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b606-188">hello URI represents hello REST API endpoint (for example, https://test03.restv2.westus.media.azure.net/api/).</span></span>

    * <span data-ttu-id="1b606-189">Az Azure AD alkalmazás értékek: hello ügyfél-azonosító és a titkos ügyfélkódot.</span><span class="sxs-lookup"><span data-stu-id="1b606-189">Azure AD application values: hello client ID and client secret.</span></span>
    
    <span data-ttu-id="1b606-190">Ezek a paraméterek értékeit tooget lásd [hello Azure portál tooaccess az Azure AD hitelesítési beállítások](media-services-portal-get-started-with-aad.md) hello szolgáltatás egyszerű hitelesítés lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1b606-190">tooget values for these parameters, see [Use hello Azure portal tooaccess Azure AD authentication settings](media-services-portal-get-started-with-aad.md) by using hello service principal authentication option.</span></span>

2. <span data-ttu-id="1b606-191">hello Azure AD hozzáférési jogkivonat toohello középső réteg zajlik.</span><span class="sxs-lookup"><span data-stu-id="1b606-191">hello Azure AD access token is sent toohello middle tier.</span></span>
4. <span data-ttu-id="1b606-192">hello középső réteg küld kérelmet toohello Azure Media REST API hello Azure AD-tokenhez.</span><span class="sxs-lookup"><span data-stu-id="1b606-192">hello middle tier sends request toohello Azure Media REST API with hello Azure AD token.</span></span>
5. <span data-ttu-id="1b606-193">hello középső réteg vissza hello adatok lekérése a Media Services.</span><span class="sxs-lookup"><span data-stu-id="1b606-193">hello middle tier gets back hello data from Media Services.</span></span>

<span data-ttu-id="1b606-194">Hogyan toouse az Azure AD hitelesítési toocommunicate többi kérelmek hello ügyfél-Media Services .NET SDK használatával kapcsolatos további információkért lásd: [használja az Azure AD hitelesítési tooaccess Azure Media Services API a .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-194">For more information about how toouse Azure AD authentication toocommunicate with REST requests by using hello Media Services .NET client SDK, see [Use Azure AD authentication tooaccess Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span> 

<span data-ttu-id="1b606-195">Ha nem használ hello Media Services .NET ügyfél SDK, manuálisan kell létrehoznia az Azure AD-jogkivonatkérelem az 1. lépésben leírt paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="1b606-195">If you are not using hello Media Services .NET client SDK, you must manually create an Azure AD token request by using parameters described in step 1.</span></span> <span data-ttu-id="1b606-196">További információkért lásd: [hogyan toouse hello Azure AD Authentication Library tooget hello Azure AD-jogkivonat](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-196">For more information, see [How toouse hello Azure AD Authentication Library tooget hello Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1b606-197">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="1b606-197">Troubleshooting</span></span>

<span data-ttu-id="1b606-198">Kivétel: "hello távoli kiszolgáló hibát adott vissza: (401) nem engedélyezett."</span><span class="sxs-lookup"><span data-stu-id="1b606-198">Exception: "hello remote server returned an error: (401) Unauthorized."</span></span>

<span data-ttu-id="1b606-199">Megoldás: Hello Media Services REST kérelem toosucceed hello hívó felhasználónak kell hello tooaccess próbál Media Services-fiók közreműködői vagy a tulajdonos szerepet.</span><span class="sxs-lookup"><span data-stu-id="1b606-199">Solution: For hello Media Services REST request toosucceed, hello calling user must be a Contributor or Owner role in hello Media Services account it is trying tooaccess.</span></span> <span data-ttu-id="1b606-200">További információkért lásd: hello [hozzáférés-vezérlés](media-services-use-aad-auth-to-access-ams-api.md#access-control) szakasz.</span><span class="sxs-lookup"><span data-stu-id="1b606-200">For more information, see hello [Access control](media-services-use-aad-auth-to-access-ams-api.md#access-control) section.</span></span>

## <a name="resources"></a><span data-ttu-id="1b606-201">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="1b606-201">Resources</span></span>

<span data-ttu-id="1b606-202">a következő cikkek hello az Azure AD hitelesítési fogalmak áttekintése:</span><span class="sxs-lookup"><span data-stu-id="1b606-202">hello following articles are overviews of Azure AD authentication concepts:</span></span> 

- [<span data-ttu-id="1b606-203">Az Azure AD által érintett hitelesítési forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="1b606-203">Authentication scenarios addressed by Azure AD</span></span>](../active-directory/develop/active-directory-authentication-scenarios.md#basics-of-authentication-in-azure-ad)
- [<span data-ttu-id="1b606-204">Hozzáadása, módosítása vagy az Azure AD alkalmazás eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1b606-204">Add, update, or remove an application in Azure AD</span></span>](../active-directory/develop/active-directory-integrating-applications.md)
- [<span data-ttu-id="1b606-205">Konfigurálja és a szerepköralapú hozzáférés-vezérlés kezelése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1b606-205">Configure and manage Role-Based Access Control by using PowerShell</span></span>](../active-directory/role-based-access-control-manage-access-powershell.md)

## <a name="next-steps"></a><span data-ttu-id="1b606-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b606-206">Next steps</span></span>

* <span data-ttu-id="1b606-207">Hello Azure-portált használja túl[elérni az Azure AD hitelesítési tooconsume Azure Media Services API](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-207">Use hello Azure portal too[access Azure AD authentication tooconsume Azure Media Services API](media-services-portal-get-started-with-aad.md).</span></span>
* <span data-ttu-id="1b606-208">Az Azure AD-hitelesítés használatára túl[elérni az Azure Media Services API a .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="1b606-208">Use Azure AD authentication too[access Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>
