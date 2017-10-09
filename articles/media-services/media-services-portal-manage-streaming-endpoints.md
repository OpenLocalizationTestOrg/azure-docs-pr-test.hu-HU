---
title: "adatfolyam-végpontok az Azure-portálon hello aaaManage |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toomanage streamvégpontok a hello Azure-portálon."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: dfa9352894d37edb317a6334d7f109419deb362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-streaming-endpoints-with-hello-azure-portal"></a><span data-ttu-id="7a99e-103">Adatfolyam-végpontokat kezelheti a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7a99e-103">Manage streaming endpoints with hello Azure portal</span></span>

<span data-ttu-id="7a99e-104">Ez a témakör bemutatja, hogyan toouse hello Azure portál toomanage streamvégpontok.</span><span class="sxs-lookup"><span data-stu-id="7a99e-104">This topic shows  how toouse hello Azure portal toomanage streaming endpoints.</span></span> 

>[!NOTE]
><span data-ttu-id="7a99e-105">Győződjön meg arról, hogy tooreview hello [áttekintése](media-services-streaming-endpoints-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="7a99e-105">Make sure tooreview hello [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> 

<span data-ttu-id="7a99e-106">Hogyan tooscale hello streamvégpont kapcsolatos információkért lásd: [ez](media-services-portal-scale-streaming-endpoints.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="7a99e-106">For information about how tooscale hello streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="start-managing-streaming-endpoints"></a><span data-ttu-id="7a99e-107">Adatfolyam-továbbítási végpontok kezelése – első lépések</span><span class="sxs-lookup"><span data-stu-id="7a99e-107">Start managing streaming endpoints</span></span> 

<span data-ttu-id="7a99e-108">a fiók streamvégpontok toostart hello követően.</span><span class="sxs-lookup"><span data-stu-id="7a99e-108">toostart managing streaming endpoints for your account, do hello following.</span></span>

1. <span data-ttu-id="7a99e-109">A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="7a99e-109">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="7a99e-110">A hello **beállítások** panelen válassza **adatfolyam-végpontok**.</span><span class="sxs-lookup"><span data-stu-id="7a99e-110">In hello **Settings** blade, select **Streaming endpoints**.</span></span>
   
    ![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> <span data-ttu-id="7a99e-112">Csak számlázása, amikor a Streamvégpontot futó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="7a99e-112">You are only billed when your Streaming Endpoint is in running state.</span></span>

## <a name="adddelete-a-streaming-endpoint"></a><span data-ttu-id="7a99e-113">A streamvégpont hozzáadása vagy törlése</span><span class="sxs-lookup"><span data-stu-id="7a99e-113">Add/delete a streaming endpoint</span></span>

>[!NOTE]
><span data-ttu-id="7a99e-114">hello alapértelmezett streamvégpontból nem lehet törölni.</span><span class="sxs-lookup"><span data-stu-id="7a99e-114">hello default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="7a99e-115">adatfolyam-végpontot a tooadd vagy törlése hello Azure-portálon, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="7a99e-115">tooadd/delete streaming endpoint using hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="7a99e-116">a streamvégpont tooadd hello kattintson **+ végpont** hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="7a99e-116">tooadd a streaming endpoint, click hello **+ Endpoint** at hello top of hello page.</span></span> 

    <span data-ttu-id="7a99e-117">Érdemes lehet több adatfolyam-végpontot, ha azt tervezi, hogy toohave különböző tartalomtovábbító vagy a CDN és a közvetlen hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="7a99e-117">You might want multiple Streaming Endpoints if you plan toohave different CDNs or a CDN and direct access.</span></span>

2. <span data-ttu-id="7a99e-118">toodelete streamvégpontra, nyomja le az ENTER **törlése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a99e-118">toodelete a streaming endpoint, press **Delete** button.</span></span>      
3. <span data-ttu-id="7a99e-119">Kattintson a hello **Start** gomb toostart hello streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="7a99e-119">Click hello **Start** button toostart hello streaming endpoint.</span></span>
   
    ![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <span data-ttu-id="7a99e-121"><a id="configure_streaming_endpoints"></a>Hello Streaming Endpoint konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a99e-121"><a id="configure_streaming_endpoints"></a>Configuring hello Streaming Endpoint</span></span>
<span data-ttu-id="7a99e-122">Adatfolyam-továbbítási végpontra lehetővé teszi tooconfigure hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="7a99e-122">Streaming Endpoint enables you tooconfigure hello following properties:</span></span>

* <span data-ttu-id="7a99e-123">Hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="7a99e-123">Access control</span></span>
* <span data-ttu-id="7a99e-124">Gyorsítótár-vezérlő</span><span class="sxs-lookup"><span data-stu-id="7a99e-124">Cache control</span></span>
* <span data-ttu-id="7a99e-125">Kereszt-hely hozzáférési házirendek</span><span class="sxs-lookup"><span data-stu-id="7a99e-125">Cross site access policies</span></span>

<span data-ttu-id="7a99e-126">Ezek a Tulajdonságok kapcsolatos részletes információkért lásd: [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="7a99e-126">For detailed information about these properties, see [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="7a99e-127">A streamvégpont hello következő tevékenységek végrehajtásával konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="7a99e-127">You can configure streaming endpoint by doing hello following:</span></span>

1. <span data-ttu-id="7a99e-128">Válassza ki a hello tooconfigure kívánt streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="7a99e-128">Select hello streaming endpoint you want tooconfigure.</span></span>
2. <span data-ttu-id="7a99e-129">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7a99e-129">Click **Settings**.</span></span>

<span data-ttu-id="7a99e-130">A következő hello mezők rövid leírását.</span><span class="sxs-lookup"><span data-stu-id="7a99e-130">A brief description of hello fields follows.</span></span>

![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. <span data-ttu-id="7a99e-132">A gyorsítótár maximális házirend: használt tooconfigure gyorsítótár élettartamát az eszközök kiszolgált keresztül a streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="7a99e-132">Maximum cache policy: used tooconfigure cache lifetime for assets served through this streaming endpoint.</span></span> <span data-ttu-id="7a99e-133">Ha nincs megadva érték, hello alapértelmezett szolgál.</span><span class="sxs-lookup"><span data-stu-id="7a99e-133">If no value is set, hello default is used.</span></span> <span data-ttu-id="7a99e-134">hello alapértelmezett értékeket közvetlenül az Azure storage-ban is definiálhatók.</span><span class="sxs-lookup"><span data-stu-id="7a99e-134">hello default values can also be defined directly in Azure storage.</span></span> <span data-ttu-id="7a99e-135">Adatfolyam-továbbítási végpontra hello Azure CDN engedélyezett, ha nem kellene hello gyorsítótár házirend érték tooless mint 600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="7a99e-135">If Azure CDN is enabled for hello streaming endpoint, you should not set hello cache policy value tooless than 600 seconds.</span></span>  
2. <span data-ttu-id="7a99e-136">Engedélyezett IP-címek: használt toospecify IP-címeket, akkor engedélyezni tooconnect toohello közzétett streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="7a99e-136">Allowed IP addresses: used toospecify IP addresses that would be allowed tooconnect toohello published streaming endpoint.</span></span> <span data-ttu-id="7a99e-137">Ha nincs megadva IP-címeket, IP-címeket tud tooconnect lesz.</span><span class="sxs-lookup"><span data-stu-id="7a99e-137">If no IP addresses specified, any IP address would be able tooconnect.</span></span> <span data-ttu-id="7a99e-138">IP-címeket adhat meg egyetlen IP-címet (például 10.0.0.1), egy IP-címet és egy CIDR alhálózati maszk (például "10.0.0.1/22") IP-címtartomány vagy egy IP-címtartomány IP-címet és egy pontozott decimális alhálózati maszk segítségével (például "10.0.0.1(255.255.255.0)').</span><span class="sxs-lookup"><span data-stu-id="7a99e-138">IP addresses can be specified as either a single IP address (for example, '10.0.0.1'), an IP range using an IP address and a CIDR subnet mask (for example, '10.0.0.1/22'), or an IP range using IP address and a dotted decimal subnet mask (for example, '10.0.0.1(255.255.255.0)').</span></span>
3. <span data-ttu-id="7a99e-139">Akamai aláírás fejléc hitelesítési konfigurációját: használt toospecify aláírás fejléc hitelesítési kérelem Akamai kiszolgálókról konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="7a99e-139">Configuration for Akamai signature header authentication: used toospecify how signature header authentication request from Akamai servers is configured.</span></span> <span data-ttu-id="7a99e-140">Lejáratának UTC formátumban van.</span><span class="sxs-lookup"><span data-stu-id="7a99e-140">Expiration is in UTC.</span></span>

## <a name="scale-your-premium-streaming-endpoint"></a><span data-ttu-id="7a99e-141">Az adatfolyam-továbbítási végpontra prémium méretezése</span><span class="sxs-lookup"><span data-stu-id="7a99e-141">Scale your Premium streaming endpoint</span></span>

<span data-ttu-id="7a99e-142">További információ [ebben](media-services-portal-scale-streaming-endpoints.md) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="7a99e-142">For more information, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <span data-ttu-id="7a99e-143"><a id="enable_cdn"></a>Azure CDN-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7a99e-143"><a id="enable_cdn"></a>Enable Azure CDN integration</span></span>

<span data-ttu-id="7a99e-144">Amikor létrehoz egy új fiókot, alapértelmezett végpont Azure CDN adatfolyam-integráció alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="7a99e-144">When you create a new account, default Streaming Endpoint Azure CDN integration is enabled by default.</span></span>

<span data-ttu-id="7a99e-145">Ha később szeretné toodisable/enable hello CDN, az adatfolyam-továbbítási végpontjának hello kell **leállt** állapotát.</span><span class="sxs-lookup"><span data-stu-id="7a99e-145">If you later want toodisable/enable hello CDN, your streaming endpoint must be in hello **stopped** state.</span></span> <span data-ttu-id="7a99e-146">Hello Azure CDN integrációs tooget engedélyezve van és aktív hello módosítások toobe too2 óra között az összes CDN POP hello eltarthat.</span><span class="sxs-lookup"><span data-stu-id="7a99e-146">It could take up too2 hours for hello Azure CDN integration tooget enabled and for hello changes toobe active across all hello CDN POPs.</span></span> <span data-ttu-id="7a99e-147">Azonban Ön indítsa el a streamelési végpont és működésének felfüggesztése nélkül adatfolyam adatfolyam-továbbítási végpontra hello és hello integrációs végrehajtása után hello adatfolyam kézbesíti a rendszer a hello CDN.</span><span class="sxs-lookup"><span data-stu-id="7a99e-147">However, your can start your streaming endpoint and stream without interruptions from hello streaming endpoint and once hello integration is complete, hello stream will be delivered from hello CDN.</span></span> <span data-ttu-id="7a99e-148">Hello időszak kiépítése során a streamvégpontján lesz **indítása** állapotát, és előfordulhat, hogy tekintse át az degredad teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="7a99e-148">During hello provisioning period your streaming endpoint will be in **starting** state and you might observe degredad performance.</span></span>

<span data-ttu-id="7a99e-149">A minden hello az Azure data központok execpt Kína és szövetségi kormányzati régiók engedélyezve van a CDN-integráció.</span><span class="sxs-lookup"><span data-stu-id="7a99e-149">CDN integration is enabled in all hello Azure data centers execpt China and Federal Goverment regions.</span></span>

<span data-ttu-id="7a99e-150">Ha engedélyezve van, hello **hozzáférés-vezérlés**, **egyéni állomásnév** és **Akamai aláírás hitelesítési** beállítások lekérdezi engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="7a99e-150">Once it is enabled, hello **Access Control**, **Custom hostname** and **Akamai Signature authentication** configuration gets disabled.</span></span>
 
> [!IMPORTANT]
> <span data-ttu-id="7a99e-151">Azure Media Services-integráció az Azure CDN van megvalósítva a **Azure CDN Verizon** standard adatfolyam-végpontok.</span><span class="sxs-lookup"><span data-stu-id="7a99e-151">Azure Media Services integration with Azure CDN is implemented on **Azure CDN from Verizon** for standard streaming endpoints.</span></span> <span data-ttu-id="7a99e-152">Prémium szintű adatfolyam-végpontok beállítható, hogy az összes **árképzési szinteket, és szolgáltatók Azure CDN**.</span><span class="sxs-lookup"><span data-stu-id="7a99e-152">Premium streaming endpoints can be configured using all **Azure CDN pricing tiers and providers**.</span></span> <span data-ttu-id="7a99e-153">Azure CDN funkciókkal kapcsolatos további információkért lásd: hello [CDN áttekintésével](../cdn/cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7a99e-153">For more information about Azure CDN features, see hello [CDN overview](../cdn/cdn-overview.md).</span></span>
 
### <a name="additional-considerations"></a><span data-ttu-id="7a99e-154">Néhány fontos megjegyzés</span><span class="sxs-lookup"><span data-stu-id="7a99e-154">Additional considerations</span></span>

* <span data-ttu-id="7a99e-155">CDN engedélyezve van a streamvégpontján, amikor az ügyfelek tartalmat nem lehet közvetlenül hello forrásból igényelnek.</span><span class="sxs-lookup"><span data-stu-id="7a99e-155">When CDN is enabled for a streaming endpoint, clients cannot request content directly from hello origin.</span></span> <span data-ttu-id="7a99e-156">Ha hello képességét tootest kell a tartalmat, vagy a CDN nélkül, egy másik streamvégpontra, amely nem engedélyezett CDN is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="7a99e-156">If you need hello ability tootest your content with or without CDN, you can create another streaming endpoint that isn't CDN enabled.</span></span>
* <span data-ttu-id="7a99e-157">A folyamatos átviteli végpont állomásnév marad hello azonos CDN engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="7a99e-157">Your streaming endpoint hostname remains hello same after enabling CDN.</span></span> <span data-ttu-id="7a99e-158">Nincs szükség a toomake bármely módosítások tooyour media services munkafolyamat CDN engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="7a99e-158">You don’t need toomake any changes tooyour media services workflow after CDN is enabled.</span></span> <span data-ttu-id="7a99e-159">Például ha a folyamatos átviteli végpont állomásnevéhez strasbourg.streaming.mediaservices.windows.net, miután engedélyezte a CDN, használatos hello pontos azonos állomásnévvel.</span><span class="sxs-lookup"><span data-stu-id="7a99e-159">For example, if your streaming endpoint hostname is strasbourg.streaming.mediaservices.windows.net, after enabling CDN, hello exact same hostname is used.</span></span>
* <span data-ttu-id="7a99e-160">Az új streamvégpontok engedélyezéséhez CDN egyszerűen egy új végpont; létrehozása meglévő streamvégpontok kell toofirst stop hello végpont és engedélyezését vagy letiltását hello CDN.</span><span class="sxs-lookup"><span data-stu-id="7a99e-160">For new streaming endpoints, you can enable CDN simply by creating a new endpoint; for existing streaming endpoints, you need toofirst stop hello endpoint and then enable/disable hello CDN.</span></span>
* <span data-ttu-id="7a99e-161">Adatfolyam-továbbítási végpontra standard csak használatával konfigurálható **Verizon standard szintű CDN-szolgáltató** az Azure felügyeleti portáljáról.</span><span class="sxs-lookup"><span data-stu-id="7a99e-161">Standard streaming endpoint can only be configured using **Verizon Standard CDN provider** using Azure management portal.</span></span> <span data-ttu-id="7a99e-162">Azonban más Azure CDN szolgáltatók REST API-k használatával engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="7a99e-162">However, you can enable other Azure CDN providers using REST APIs.</span></span>

## <a name="configure-cdn-profile"></a><span data-ttu-id="7a99e-163">CDN-profil konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a99e-163">Configure CDN profile</span></span>

<span data-ttu-id="7a99e-164">CDN-profil hello hello kiválasztásával konfigurálhatja **kezelése CDN** hello elejéről gombra.</span><span class="sxs-lookup"><span data-stu-id="7a99e-164">You can configure hello CDN profile by selecting hello **Manage CDN** button from hello top.</span></span>

![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a><span data-ttu-id="7a99e-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a99e-166">Next steps</span></span>
<span data-ttu-id="7a99e-167">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="7a99e-167">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7a99e-168">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="7a99e-168">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

