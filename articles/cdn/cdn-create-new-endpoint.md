---
title: "Az Azure CDN használatának első lépései | Microsoft Docs"
description: "Ez a témakör azt mutatja be, hogyan engedélyezhető az Azure Content Delivery Network (CDN). Az oktatóanyag végigvezeti Önt egy új CDN-profil és -végpont létrehozásán."
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
ms.openlocfilehash: d263e911d0d0b3cdc1e48e300a3c8a0994b38c39
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-cdn"></a><span data-ttu-id="0a58c-104">Az Azure CDN használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="0a58c-104">Getting started with Azure CDN</span></span>
<span data-ttu-id="0a58c-105">Ez a témakör egy új CDN-profil és -végpont létrehozásán keresztül vezeti Önt végig az Azure CDN aktiválásán.</span><span class="sxs-lookup"><span data-stu-id="0a58c-105">This topic walks through enabling Azure CDN by creating a new CDN profile and endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a58c-106">A CDN működésének bemutatásáért, valamint az általa nyújtott szolgáltatások listájáért tekintse meg a [Tartalomkézbesítési hálózat (CDN) – áttekintés](cdn-overview.md) című dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="0a58c-106">For an introduction to how CDN works, as well as a list of features, see the [CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="0a58c-107">Új CDN-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a58c-107">Create a new CDN profile</span></span>
<span data-ttu-id="0a58c-108">A CDN-profil CDN-végpontok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="0a58c-108">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="0a58c-109">Minden profil egy vagy több CDN-végpontot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0a58c-109">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="0a58c-110">Előfordulhat, hogy több profil használatával rendszerezni szeretné a CDN-végpontokat internetes tartomány, webalkalmazás vagy más feltétel alapján.</span><span class="sxs-lookup"><span data-stu-id="0a58c-110">You may wish to use multiple profiles to organize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!NOTE]
> <span data-ttu-id="0a58c-111">Alapértelmezés szerint egy Azure-előfizetéssel nyolc CDN-profil használható.</span><span class="sxs-lookup"><span data-stu-id="0a58c-111">By default, a single Azure subscription is limited to eight CDN profiles.</span></span> <span data-ttu-id="0a58c-112">Az egyes CDN-profilokban legfeljebb tíz CDN-végpont lehet.</span><span class="sxs-lookup"><span data-stu-id="0a58c-112">Each CDN profile is limited to ten CDN endpoints.</span></span>
> 
> <span data-ttu-id="0a58c-113">A CDN-díjszabást a CDN-profilok szintjén alkalmazzuk.</span><span class="sxs-lookup"><span data-stu-id="0a58c-113">CDN pricing is applied at the CDN profile level.</span></span> <span data-ttu-id="0a58c-114">Ha több Azure CDN-tarifacsomagot szeretne vegyesen használni, több CDN-profilra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="0a58c-114">If you wish to use a mix of Azure CDN pricing tiers, you will need multiple CDN profiles.</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="0a58c-115">Új CDN-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a58c-115">Create a new CDN endpoint</span></span>
<span data-ttu-id="0a58c-116">**Új CDN-végpont létrehozása**</span><span class="sxs-lookup"><span data-stu-id="0a58c-116">**To create a new CDN endpoint**</span></span>

1. <span data-ttu-id="0a58c-117">Az [Azure portálon](https://portal.azure.com) lépjen a CDN-profilra.</span><span class="sxs-lookup"><span data-stu-id="0a58c-117">In the [Azure Portal](https://portal.azure.com), navigate to your CDN profile.</span></span>  <span data-ttu-id="0a58c-118">Lehetséges, hogy a profilt az előző lépésben az irányítópulton rögzítette.</span><span class="sxs-lookup"><span data-stu-id="0a58c-118">You may have pinned it to the dashboard in the previous step.</span></span>  <span data-ttu-id="0a58c-119">Amennyiben nem rögzítette, kattintson **Tallózás** gombra, majd a **CDN-profilok** elemre, végül kattintson arra a profilra, amelyet hozzá szeretne adni a végponthoz.</span><span class="sxs-lookup"><span data-stu-id="0a58c-119">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on the profile you plan to add your endpoint to.</span></span>
   
    <span data-ttu-id="0a58c-120">Megjelenik a CDN-profil panelje.</span><span class="sxs-lookup"><span data-stu-id="0a58c-120">The CDN profile blade appears.</span></span>
   
    ![CDN-profil][cdn-profile-settings]
2. <span data-ttu-id="0a58c-122">Kattintson a **Végpont hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a58c-122">Click the **Add Endpoint** button.</span></span>
   
    ![Végpont hozzáadása gomb][cdn-new-endpoint-button]
   
    <span data-ttu-id="0a58c-124">Megjelenik a **Végpont hozzáadása** panel.</span><span class="sxs-lookup"><span data-stu-id="0a58c-124">The **Add an endpoint** blade appears.</span></span>
   
    ![Végpont hozzáadása panel][cdn-add-endpoint]
3. <span data-ttu-id="0a58c-126">Adja meg a CDN-végpont kívánt nevét a **Név** mezőben.</span><span class="sxs-lookup"><span data-stu-id="0a58c-126">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="0a58c-127">A rendszer ezt a nevet fogja használni a gyorsítótárazott erőforrások eléréséhez a `<endpointname>.azureedge.net` tartományban.</span><span class="sxs-lookup"><span data-stu-id="0a58c-127">This name will be used to access your cached resources at the domain `<endpointname>.azureedge.net`.</span></span>
4. <span data-ttu-id="0a58c-128">A **Forrása típusa** legördülő menüben válassza ki a forrástípust.</span><span class="sxs-lookup"><span data-stu-id="0a58c-128">In the **Origin type** dropdown, select your origin type.</span></span>  <span data-ttu-id="0a58c-129">Azure Storage-fiók esetén válassza a **Storage**, Azure Cloud Service szolgáltatás esetén a **Cloud Service**, Azure Web Apps esetén a **Webalkalmazás**, az összes többi nyilvánosan elérhető (az Azure rendszerben vagy máshol tárolt) webkiszolgáló-forrás esetén pedig az **Egyéni forrás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0a58c-129">Select **Storage** for an Azure Storage account, **Cloud service** for an Azure Cloud Service, **Web App** for an Azure Web App, or **Custom origin** for any other publicly accessible web server origin (hosted in Azure or elsewhere).</span></span>
   
    ![A CDN-forrása típusa](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. <span data-ttu-id="0a58c-131">A **Forrás állomásneve** legördülő menüben válassza ki vagy adja meg a forrástartományt.</span><span class="sxs-lookup"><span data-stu-id="0a58c-131">In the **Origin hostname** dropdown, select or type your origin domain.</span></span>  <span data-ttu-id="0a58c-132">A legördülő menüben szerepelni fog a 4. lépésben megadott típusú összes rendelkezésre álló forrás.</span><span class="sxs-lookup"><span data-stu-id="0a58c-132">The dropdown will list all available origins of the type you specified in step 4.</span></span>  <span data-ttu-id="0a58c-133">Ha a **Forrása típusa** számára az *Egyéni forrás* beállítást választotta ki, akkor be kell írnia az egyéni forrás tartományát.</span><span class="sxs-lookup"><span data-stu-id="0a58c-133">If you selected *Custom origin* as your **Origin type**, you will type in the domain of your custom origin.</span></span>
6. <span data-ttu-id="0a58c-134">A **Forrás elérési útvonala** szövegmezőbe írja be a gyorsítótárazni kívánt erőforrások elérési útját. A mezőt üresen is hagyhatja, ha engedélyezni szeretné az 5. lépésben megadott tartomány bármely erőforrásának gyorsítótárazását.</span><span class="sxs-lookup"><span data-stu-id="0a58c-134">In the **Origin path** text box, enter the path to the resources you want to cache, or leave blank to allow cache any resource at the domain you specified in step 5.</span></span>
7. <span data-ttu-id="0a58c-135">A **Forrás állomásfejléce** mezőben adja meg azt az állomásfejlécet, amelyet a CDN-nek minden egyes kérelemmel el kell küldenie; vagy hagyja meg az alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="0a58c-135">In the **Origin host header**, enter the host header you want the CDN to send with each request, or leave the default.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="0a58c-136">Bizonyos típusú források – például az Azure Storage és a Web Apps – esetén az állomásfejlécnek egyeznie kell a forrás tartományával.</span><span class="sxs-lookup"><span data-stu-id="0a58c-136">Some types of origins, such as Azure Storage and Web Apps, require the host header to match the domain of the origin.</span></span> <span data-ttu-id="0a58c-137">Hacsak a forrás a tartománytól eltérő állomásfejléc használatát nem igényli, hagyja meg az alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="0a58c-137">Unless you have an origin that requires a host header different from its domain, you should leave the default value.</span></span>
   > 
   > 
8. <span data-ttu-id="0a58c-138">A **Protokoll** és a **Forrásport** mezőben adja meg a forráson az erőforrások eléréséhez használt protokollokat és portokat.</span><span class="sxs-lookup"><span data-stu-id="0a58c-138">For **Protocol** and **Origin port**, specify the protocols and ports used to access your resources at the origin.</span></span>  <span data-ttu-id="0a58c-139">Legalább egy protokollt (a HTTP vagy a HTTPS protokollt) ki kell választani.</span><span class="sxs-lookup"><span data-stu-id="0a58c-139">At least one protocol (HTTP or HTTPS) must be selected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0a58c-140">A **Forrásport** értéke csak azt befolyásolja, hogy a végpont melyik portot használja az információk forrásról való lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="0a58c-140">The **Origin port** only affects what port the endpoint uses to retrieve information from the origin.</span></span>  <span data-ttu-id="0a58c-141">Magát a végpontot a végügyfelek – a **Forrásport** értékétől függetlenül – csak az alapértelmezett HTTP- és HTTPS-porton (azaz a 80-as és a 443-as porton) keresztül érik el.</span><span class="sxs-lookup"><span data-stu-id="0a58c-141">The endpoint itself will only be available to end clients on the default HTTP and HTTPS ports (80 and 443), regardless of the **Origin port**.</span></span>  
   > 
   > <span data-ttu-id="0a58c-142">Az **Akamai Azure CDN** típusú végpontok esetén a források számára nem áll rendelkezésre a teljes TCP-porttartomány.</span><span class="sxs-lookup"><span data-stu-id="0a58c-142">**Azure CDN from Akamai** endpoints do not allow the full TCP port range for origins.</span></span>  <span data-ttu-id="0a58c-143">A nem engedélyezett forrásportok listáját lást: [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx) (Az Akamai Azure CDN engedélyezett forrásportjai).</span><span class="sxs-lookup"><span data-stu-id="0a58c-143">For a list of origin ports that are not allowed, see [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx).</span></span>  
   > 
   > <span data-ttu-id="0a58c-144">A CDN-tartalom HTTPS-kapcsolaton keresztüli elérésére a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="0a58c-144">Accessing CDN content using HTTPS has the following constraints:</span></span>
   > 
   > * <span data-ttu-id="0a58c-145">A CDN által biztosított SSL-tanúsítványt kell használni.</span><span class="sxs-lookup"><span data-stu-id="0a58c-145">You must use the SSL certificate provided by the CDN.</span></span> <span data-ttu-id="0a58c-146">A rendszer nem támogatja a harmadik féltől származó tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="0a58c-146">Third party certificates are not supported.</span></span>
   > * <span data-ttu-id="0a58c-147">A HTTPS-tartalom eléréséhez a CDN által biztosított tartományt (`<endpointname>.azureedge.net`) kell használni.</span><span class="sxs-lookup"><span data-stu-id="0a58c-147">You must use the CDN-provided domain (`<endpointname>.azureedge.net`) to access HTTPS content.</span></span> <span data-ttu-id="0a58c-148">A HTTPS nem támogatott az egyéni tartománynevek (CNAME) esetén, mivel a CDN jelenleg nem támogatja az egyéni tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="0a58c-148">HTTPS support is not available for custom domain names (CNAMEs) since the CDN does not support custom certificates at this time.</span></span>
   > 
   > 
9. <span data-ttu-id="0a58c-149">Az új végpont létrehozásához kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a58c-149">Click the **Add** button to create the new endpoint.</span></span>
10. <span data-ttu-id="0a58c-150">A végpont a létrehozás után megjelenik a profil végpontjainak listájában.</span><span class="sxs-lookup"><span data-stu-id="0a58c-150">Once the endpoint is created, it appears in a list of endpoints for the profile.</span></span> <span data-ttu-id="0a58c-151">A listanézetben látható a gyorsítótárazott tartalom eléréséhez használható URL-cím, valamint a forrástartomány is.</span><span class="sxs-lookup"><span data-stu-id="0a58c-151">The list view shows the URL to use to access cached content, as well as the origin domain.</span></span>
    
    ![CDN-végpont][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > <span data-ttu-id="0a58c-153">A végpont nem vehető használatba azonnal, mivel némi időre van szükség a regisztráció CDN-rendszeren belüli propagálásához.</span><span class="sxs-lookup"><span data-stu-id="0a58c-153">The endpoint will not immediately be available for use, as it takes time for the registration to propagate through the CDN.</span></span>  <span data-ttu-id="0a58c-154">Az <b>Akamai Azure CDN</b> típusú profilok propagálása általában egy percen belül befejeződik.</span><span class="sxs-lookup"><span data-stu-id="0a58c-154">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="0a58c-155">A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="0a58c-155">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
    > 
    > <span data-ttu-id="0a58c-156">A felhasználók 404-es HTTP-válaszkódot kapnak, ha a CDN-tartománynevet azelőtt próbálják meg használni, mielőtt végbement volna a végpont-konfiguráció POP-kre történő propagálása.</span><span class="sxs-lookup"><span data-stu-id="0a58c-156">Users who try to use the CDN domain name before the endpoint configuration has propagated to the POPs will receive HTTP 404 response codes.</span></span>  <span data-ttu-id="0a58c-157">Ha a végpont létrehozása óta már több óra is eltelt, mégis 404-es válaszkódot kap, olvassa el a [404-es állapotot visszaadó CDN-végpontok hibaelhárítása](cdn-troubleshoot-endpoint.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="0a58c-157">If it's been several hours since you created your endpoint and you're still receiving 404 responses, please see [Troubleshooting CDN endpoints returning 404 statuses](cdn-troubleshoot-endpoint.md).</span></span>
    > 
    > 

## <a name="see-also"></a><span data-ttu-id="0a58c-158">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0a58c-158">See Also</span></span>
* [<span data-ttu-id="0a58c-159">Lekérdezési karakterláncot tartalmazó kérelmek gyorsítótárazási viselkedésének vezérlése</span><span class="sxs-lookup"><span data-stu-id="0a58c-159">Controlling caching behavior of requests with query strings</span></span>](cdn-query-string.md)
* [<span data-ttu-id="0a58c-160">CDN-tartalom leképezése egyéni tartományra</span><span class="sxs-lookup"><span data-stu-id="0a58c-160">How to Map CDN Content to a Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="0a58c-161">Eszközök előzetes betöltése Azure CDN-végponton</span><span class="sxs-lookup"><span data-stu-id="0a58c-161">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="0a58c-162">CDN-végpont végleges törlése</span><span class="sxs-lookup"><span data-stu-id="0a58c-162">Purge an Azure CDN Endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="0a58c-163">404-es állapotot visszaadó CDN-végpontok hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="0a58c-163">Troubleshooting CDN endpoints returning 404 statuses</span></span>](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
