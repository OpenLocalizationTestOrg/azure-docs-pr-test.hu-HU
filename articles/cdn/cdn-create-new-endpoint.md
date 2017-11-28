---
title: "aaaGetting Azure CDN használatbavételének |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooenable hello Azure Content Delivery Network (CDN). hello az oktatóanyag végigvezeti egy új CDN-profil és -végpont hello létrehozása."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0ce9802bfd7b60e70a9a62330f5593fc17ea07d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-cdn"></a><span data-ttu-id="86996-104">Az Azure CDN használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="86996-104">Getting started with Azure CDN</span></span>
<span data-ttu-id="86996-105">Ez a témakör egy új CDN-profil és -végpont létrehozásán keresztül vezeti Önt végig az Azure CDN aktiválásán.</span><span class="sxs-lookup"><span data-stu-id="86996-105">This topic walks through enabling Azure CDN by creating a new CDN profile and endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86996-106">Egy bevezető toohow CDN működik, valamint a szolgáltatások listáját, lásd: hello [CDN áttekintésével](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="86996-106">For an introduction toohow CDN works, as well as a list of features, see hello [CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="86996-107">Új CDN-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="86996-107">Create a new CDN profile</span></span>
<span data-ttu-id="86996-108">A CDN-profil CDN-végpontok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="86996-108">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="86996-109">Minden profil egy vagy több CDN-végpontot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="86996-109">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="86996-110">Kezdésként érdemes lehet toouse több profilok tooorganize a CDN-végpontokat internetes tartomány, webalkalmazás vagy más feltétel alapján.</span><span class="sxs-lookup"><span data-stu-id="86996-110">You may wish toouse multiple profiles tooorganize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!NOTE]
> <span data-ttu-id="86996-111">Alapértelmezés szerint egy Azure-előfizetéssel korlátozott tooeight CDN-profil.</span><span class="sxs-lookup"><span data-stu-id="86996-111">By default, a single Azure subscription is limited tooeight CDN profiles.</span></span> <span data-ttu-id="86996-112">Minden egyes CDN-profil CDN-végpontok korlátozott tooten.</span><span class="sxs-lookup"><span data-stu-id="86996-112">Each CDN profile is limited tooten CDN endpoints.</span></span>
> 
> <span data-ttu-id="86996-113">A CDN-díjszabást hello CDN profil szinten alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="86996-113">CDN pricing is applied at hello CDN profile level.</span></span> <span data-ttu-id="86996-114">Ha a toouse Azure CDN vegyesen tarifacsomagok, szüksége lesz a több CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="86996-114">If you wish toouse a mix of Azure CDN pricing tiers, you will need multiple CDN profiles.</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="86996-115">Új CDN-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="86996-115">Create a new CDN endpoint</span></span>
<span data-ttu-id="86996-116">**új CDN-végpont toocreate**</span><span class="sxs-lookup"><span data-stu-id="86996-116">**toocreate a new CDN endpoint**</span></span>

1. <span data-ttu-id="86996-117">A hello [Azure Portal](https://portal.azure.com), keresse meg a tooyour CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="86996-117">In hello [Azure Portal](https://portal.azure.com), navigate tooyour CDN profile.</span></span>  <span data-ttu-id="86996-118">Előfordulhat, hogy rendelkezik rögzítette azt toohello irányítópult hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="86996-118">You may have pinned it toohello dashboard in hello previous step.</span></span>  <span data-ttu-id="86996-119">Ha nem, akkor megtalálja kattintva **Tallózás**, majd **CDN-profilra**, és kattintson a hello-profil tervezi tooadd a végponthoz.</span><span class="sxs-lookup"><span data-stu-id="86996-119">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on hello profile you plan tooadd your endpoint to.</span></span>
   
    <span data-ttu-id="86996-120">CDN-profil panelje hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="86996-120">hello CDN profile blade appears.</span></span>
   
    ![CDN-profil][cdn-profile-settings]
2. <span data-ttu-id="86996-122">Kattintson a hello **végpont hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="86996-122">Click hello **Add Endpoint** button.</span></span>
   
    ![Végpont hozzáadása gomb][cdn-new-endpoint-button]
   
    <span data-ttu-id="86996-124">Hello **végpont hozzáadása** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="86996-124">hello **Add an endpoint** blade appears.</span></span>
   
    ![Végpont hozzáadása panel][cdn-add-endpoint]
3. <span data-ttu-id="86996-126">Adja meg a CDN-végpont kívánt nevét a **Név** mezőben.</span><span class="sxs-lookup"><span data-stu-id="86996-126">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="86996-127">Ez a név a gyorsítótárazott erőforrások hello tartomány lesz használt tooaccess `<endpointname>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="86996-127">This name will be used tooaccess your cached resources at hello domain `<endpointname>.azureedge.net`.</span></span>
4. <span data-ttu-id="86996-128">A hello **forrása típusa** legördülő menüben válassza ki a forrástípust.</span><span class="sxs-lookup"><span data-stu-id="86996-128">In hello **Origin type** dropdown, select your origin type.</span></span>  <span data-ttu-id="86996-129">Azure Storage-fiók esetén válassza a **Storage**, Azure Cloud Service szolgáltatás esetén a **Cloud Service**, Azure Web Apps esetén a **Webalkalmazás**, az összes többi nyilvánosan elérhető (az Azure rendszerben vagy máshol tárolt) webkiszolgáló-forrás esetén pedig az **Egyéni forrás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="86996-129">Select **Storage** for an Azure Storage account, **Cloud service** for an Azure Cloud Service, **Web App** for an Azure Web App, or **Custom origin** for any other publicly accessible web server origin (hosted in Azure or elsewhere).</span></span>
   
    ![A CDN-forrása típusa](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. <span data-ttu-id="86996-131">A hello **forrásállomásnév** legördülő menüben válasszon ki vagy írja be a forrástartományt.</span><span class="sxs-lookup"><span data-stu-id="86996-131">In hello **Origin hostname** dropdown, select or type your origin domain.</span></span>  <span data-ttu-id="86996-132">hello legördülő lista felsorolja az összes rendelkezésre álló források a 4. lépésben megadott hello típusú.</span><span class="sxs-lookup"><span data-stu-id="86996-132">hello dropdown will list all available origins of hello type you specified in step 4.</span></span>  <span data-ttu-id="86996-133">Ha a kiválasztott *egyéni forrás* , a **forrása típusa**, akkor be kell írnia az egyéni forrás hello tartományban.</span><span class="sxs-lookup"><span data-stu-id="86996-133">If you selected *Custom origin* as your **Origin type**, you will type in hello domain of your custom origin.</span></span>
6. <span data-ttu-id="86996-134">A hello **forrás elérési útvonalának** szöveg mezőbe írja be a kívánt toocache, vagy hagyja üresen tooallow gyorsítótár 5. lépésben megadott hello tartomány bármely erőforrásának hello elérési toohello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="86996-134">In hello **Origin path** text box, enter hello path toohello resources you want toocache, or leave blank tooallow cache any resource at hello domain you specified in step 5.</span></span>
7. <span data-ttu-id="86996-135">A hello **forrás állomásfejlécét**adja meg a kívánt minden egyes kérelemmel CDN toosend hello hello állomásfejléc, vagy hagyja hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="86996-135">In hello **Origin host header**, enter hello host header you want hello CDN toosend with each request, or leave hello default.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="86996-136">Bizonyos típusú források – például az Azure Storage és a Web Apps hello host fejléc toomatch hello hello eredeti tartomány szükséges.</span><span class="sxs-lookup"><span data-stu-id="86996-136">Some types of origins, such as Azure Storage and Web Apps, require hello host header toomatch hello domain of hello origin.</span></span> <span data-ttu-id="86996-137">Ha nincs forrás a tartománytól eltérő állomásfejléc használatát nem igényli, hagyja hello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="86996-137">Unless you have an origin that requires a host header different from its domain, you should leave hello default value.</span></span>
   > 
   > 
8. <span data-ttu-id="86996-138">A **protokoll** és **forrásport**adja meg a hello protokollok és portok használt tooaccess az erőforrások hello származási helyen.</span><span class="sxs-lookup"><span data-stu-id="86996-138">For **Protocol** and **Origin port**, specify hello protocols and ports used tooaccess your resources at hello origin.</span></span>  <span data-ttu-id="86996-139">Legalább egy protokollt (a HTTP vagy a HTTPS protokollt) ki kell választani.</span><span class="sxs-lookup"><span data-stu-id="86996-139">At least one protocol (HTTP or HTTPS) must be selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="86996-140">Hello **forrásport** csak érinti a milyen port hello végpont hello forrásból tooretrieve információkat használja.</span><span class="sxs-lookup"><span data-stu-id="86996-140">hello **Origin port** only affects what port hello endpoint uses tooretrieve information from hello origin.</span></span>  <span data-ttu-id="86996-141">hello maga végpont csak akkor elérhető tooend ügyfelek hello alapértelmezett HTTP és HTTPS-portok (80-as és 443-as), függetlenül attól, hello **forrásport**.</span><span class="sxs-lookup"><span data-stu-id="86996-141">hello endpoint itself will only be available tooend clients on hello default HTTP and HTTPS ports (80 and 443), regardless of hello **Origin port**.</span></span>  
   > 
   > <span data-ttu-id="86996-142">**Akamai Azure CDN** végpontok nem teszik lehetővé az hello teljes TCP-porttartomány esetén a források számára.</span><span class="sxs-lookup"><span data-stu-id="86996-142">**Azure CDN from Akamai** endpoints do not allow hello full TCP port range for origins.</span></span>  <span data-ttu-id="86996-143">A nem engedélyezett forrásportok listáját lást: [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx) (Az Akamai Azure CDN engedélyezett forrásportjai).</span><span class="sxs-lookup"><span data-stu-id="86996-143">For a list of origin ports that are not allowed, see [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx).</span></span>  
   > 
   > <span data-ttu-id="86996-144">CDN elérése HTTPS-kapcsolaton keresztül tartalmat rendelkezik a következő korlátozások hello:</span><span class="sxs-lookup"><span data-stu-id="86996-144">Accessing CDN content using HTTPS has hello following constraints:</span></span>
   > 
   > * <span data-ttu-id="86996-145">Hello hello CDN által biztosított SSL-tanúsítványt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="86996-145">You must use hello SSL certificate provided by hello CDN.</span></span> <span data-ttu-id="86996-146">A rendszer nem támogatja a harmadik féltől származó tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="86996-146">Third party certificates are not supported.</span></span>
   > * <span data-ttu-id="86996-147">Hello CDN által biztosított tartományt kell használnia (`<endpointname>.azureedge.net`) tooaccess HTTPS-tartalom.</span><span class="sxs-lookup"><span data-stu-id="86996-147">You must use hello CDN-provided domain (`<endpointname>.azureedge.net`) tooaccess HTTPS content.</span></span> <span data-ttu-id="86996-148">HTTPS-támogatás nem érhető el egyéni tartománynevek (CNAME), mert hello CDN nem támogatja az egyéni tanúsítványokat jelenleg.</span><span class="sxs-lookup"><span data-stu-id="86996-148">HTTPS support is not available for custom domain names (CNAMEs) since hello CDN does not support custom certificates at this time.</span></span>
   > 
   > 
9. <span data-ttu-id="86996-149">Kattintson a hello **Hozzáadás** gomb toocreate hello új végpont.</span><span class="sxs-lookup"><span data-stu-id="86996-149">Click hello **Add** button toocreate hello new endpoint.</span></span>
10. <span data-ttu-id="86996-150">Hello végpont létrehozását követően hello-profil végpontjainak listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="86996-150">Once hello endpoint is created, it appears in a list of endpoints for hello profile.</span></span> <span data-ttu-id="86996-151">hello listanézetben látható, hello URL-cím toouse tooaccess gyorsítótárazott tartalmat, valamint hello forrástartományt.</span><span class="sxs-lookup"><span data-stu-id="86996-151">hello list view shows hello URL toouse tooaccess cached content, as well as hello origin domain.</span></span>
    
    ![CDN-végpont][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > <span data-ttu-id="86996-153">hello végpont nem azonnal elérhetővé válik, használatra hello regisztrációs toopropagate keresztül hello CDN időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="86996-153">hello endpoint will not immediately be available for use, as it takes time for hello registration toopropagate through hello CDN.</span></span>  <span data-ttu-id="86996-154">Az <b>Akamai Azure CDN</b> típusú profilok propagálása általában egy percen belül befejeződik.</span><span class="sxs-lookup"><span data-stu-id="86996-154">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="86996-155">A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="86996-155">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
    > 
    > <span data-ttu-id="86996-156">Próbálja meg toouse hello CDN-tartománynevet, mielőtt hello végpont-konfiguráció propagálása toohello POP felhasználók HTTP 404-es válaszkódot kapnak.</span><span class="sxs-lookup"><span data-stu-id="86996-156">Users who try toouse hello CDN domain name before hello endpoint configuration has propagated toohello POPs will receive HTTP 404 response codes.</span></span>  <span data-ttu-id="86996-157">Ha a végpont létrehozása óta már több óra is eltelt, mégis 404-es válaszkódot kap, olvassa el a [404-es állapotot visszaadó CDN-végpontok hibaelhárítása](cdn-troubleshoot-endpoint.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="86996-157">If it's been several hours since you created your endpoint and you're still receiving 404 responses, please see [Troubleshooting CDN endpoints returning 404 statuses](cdn-troubleshoot-endpoint.md).</span></span>
    > 
    > 

## <a name="see-also"></a><span data-ttu-id="86996-158">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="86996-158">See Also</span></span>
* [<span data-ttu-id="86996-159">Lekérdezési karakterláncot tartalmazó kérelmek gyorsítótárazási viselkedésének vezérlése</span><span class="sxs-lookup"><span data-stu-id="86996-159">Controlling caching behavior of requests with query strings</span></span>](cdn-query-string.md)
* [<span data-ttu-id="86996-160">Hogyan tooMap CDN tartalom tooa egyéni tartományhoz</span><span class="sxs-lookup"><span data-stu-id="86996-160">How tooMap CDN Content tooa Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="86996-161">Eszközök előzetes betöltése Azure CDN-végponton</span><span class="sxs-lookup"><span data-stu-id="86996-161">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="86996-162">CDN-végpont végleges törlése</span><span class="sxs-lookup"><span data-stu-id="86996-162">Purge an Azure CDN Endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="86996-163">404-es állapotot visszaadó CDN-végpontok hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="86996-163">Troubleshooting CDN endpoints returning 404 statuses</span></span>](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
