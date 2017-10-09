---
title: "egy aktív DNS aaaMigrate tooAzure App Service name |} Microsoft Docs"
description: "Ismerje meg, hogyan toomigrate egy egyéni DNS-tartománynév, amely már hozzá van rendelve a tooa élő hely tooAzure App Service állásidő nélkül."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: cephalin
ms.openlocfilehash: fbc4cc30dcb87efb2e867cb65c5404b667661e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-an-active-dns-name-tooazure-app-service"></a><span data-ttu-id="341d6-103">Telepítse át egy aktív DNS-név tooAzure App Service</span><span class="sxs-lookup"><span data-stu-id="341d6-103">Migrate an active DNS name tooAzure App Service</span></span>

<span data-ttu-id="341d6-104">Ez a cikk bemutatja, hogyan toomigrate egy aktív DNS neve túl[Azure App Service](../app-service/app-service-value-prop-what-is.md) állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="341d6-104">This article shows you how toomigrate an active DNS name too[Azure App Service](../app-service/app-service-value-prop-what-is.md) without any downtime.</span></span>

<span data-ttu-id="341d6-105">Egy élő hely és a DNS tartomány neve tooApp szolgáltatás áttelepítésekor, hogy a DNS-név már kiszolgál élő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="341d6-105">When you migrate a live site and its DNS domain name tooApp Service, that DNS name is already serving live traffic.</span></span> <span data-ttu-id="341d6-106">DNS-feloldás az állásidő elkerülhetők hello az áttelepítés során megelőző jelleggel kötés hello aktív DNS nevét tooyour App Service-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="341d6-106">You can avoid downtime in DNS resolution during hello migration by binding hello active DNS name tooyour App Service app preemptively.</span></span>

<span data-ttu-id="341d6-107">Ha nem aggódik DNS-feloldás az állásidő, lásd: [leképezése egy meglévő egyéni DNS nevét tooAzure webalkalmazások](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="341d6-107">If you're not worried about downtime in DNS resolution, see [Map an existing custom DNS name tooAzure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="341d6-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="341d6-108">Prerequisites</span></span>

<span data-ttu-id="341d6-109">Ez az útmutató toocomplete:</span><span class="sxs-lookup"><span data-stu-id="341d6-109">toocomplete this how-to:</span></span>

- <span data-ttu-id="341d6-110">[Győződjön meg arról, hogy az App Service alkalmazás nem ingyenes szint](app-service-web-tutorial-custom-domain.md#checkpricing).</span><span class="sxs-lookup"><span data-stu-id="341d6-110">[Make sure that your App Service app is not in FREE tier](app-service-web-tutorial-custom-domain.md#checkpricing).</span></span>

## <a name="bind-hello-domain-name-preemptively"></a><span data-ttu-id="341d6-111">Hello tartománynév megelőző jelleggel kötése</span><span class="sxs-lookup"><span data-stu-id="341d6-111">Bind hello domain name preemptively</span></span>

<span data-ttu-id="341d6-112">Az egyéni tartománynév megelőző jelleggel kötését, elérheti mindkét hello alábbiakat, mielőtt a DNS-rekordok módosítása:</span><span class="sxs-lookup"><span data-stu-id="341d6-112">When you bind a custom domain preemptively, you accomplish both of hello following before making any changes to your DNS records:</span></span>

- <span data-ttu-id="341d6-113">Ellenőrizze a tartomány tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="341d6-113">Verify domain ownership</span></span>
- <span data-ttu-id="341d6-114">Az alkalmazás hello tartomány nevét engedélyezése</span><span class="sxs-lookup"><span data-stu-id="341d6-114">Enable hello domain name for your app</span></span>

<span data-ttu-id="341d6-115">Végül a való áttelepítéskor a egyéni DNS-nevével hello régi hely toohello App Service-alkalmazást, lesz állásidő nélkül a DNS-feloldás eredményének.</span><span class="sxs-lookup"><span data-stu-id="341d6-115">When you finally migrate your custom DNS name from hello old site toohello App Service app, there will be no downtime in DNS resolution.</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a><span data-ttu-id="341d6-116">Tartomány ellenőrzési rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="341d6-116">Create domain verification record</span></span>

<span data-ttu-id="341d6-117">Jegyezze fel tooverify tartomány tulajdonosa, hozzáadása egy TXT.</span><span class="sxs-lookup"><span data-stu-id="341d6-117">tooverify domain ownership, Add a TXT record.</span></span> <span data-ttu-id="341d6-118">a maps hello TXT-rekord _awverify.&lt; altartomány >_ too_&lt;alkalmazásnév >. azurewebsites.net_.</span><span class="sxs-lookup"><span data-stu-id="341d6-118">hello TXT record maps from _awverify.&lt;subdomain>_ too_&lt;appname>.azurewebsites.net_.</span></span> 

<span data-ttu-id="341d6-119">hello kell TXT-rekord hello toomigrate kívánt DNS-rekord függ.</span><span class="sxs-lookup"><span data-stu-id="341d6-119">hello TXT record you need depends on hello DNS record you want toomigrate.</span></span> <span data-ttu-id="341d6-120">Tekintse meg a következő táblázat hello (`@` általában jelöli hello gyökértartomány):</span><span class="sxs-lookup"><span data-stu-id="341d6-120">For examples, see hello following table (`@` typically represents hello root domain):</span></span>  

| <span data-ttu-id="341d6-121">DNS-rekord példa</span><span class="sxs-lookup"><span data-stu-id="341d6-121">DNS record example</span></span> | <span data-ttu-id="341d6-122">TXT-állomás</span><span class="sxs-lookup"><span data-stu-id="341d6-122">TXT Host</span></span> | <span data-ttu-id="341d6-123">TXT érték</span><span class="sxs-lookup"><span data-stu-id="341d6-123">TXT Value</span></span> |
| - | - | - |
| <span data-ttu-id="341d6-124">@ (root)</span><span class="sxs-lookup"><span data-stu-id="341d6-124">@ (root)</span></span> | <span data-ttu-id="341d6-125">_awverify_</span><span class="sxs-lookup"><span data-stu-id="341d6-125">_awverify_</span></span> | <span data-ttu-id="341d6-126">_&lt;alkalmazásnév >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="341d6-126">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="341d6-127">a webszolgáltatáshoz (rész)</span><span class="sxs-lookup"><span data-stu-id="341d6-127">www (sub)</span></span> | <span data-ttu-id="341d6-128">_awverify.www_</span><span class="sxs-lookup"><span data-stu-id="341d6-128">_awverify.www_</span></span> | <span data-ttu-id="341d6-129">_&lt;alkalmazásnév >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="341d6-129">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="341d6-130">\*(helyettesítő)</span><span class="sxs-lookup"><span data-stu-id="341d6-130">\* (wildcard)</span></span> | <span data-ttu-id="341d6-131">_awverify.\*_</span><span class="sxs-lookup"><span data-stu-id="341d6-131">_awverify.\*_</span></span> | <span data-ttu-id="341d6-132">_&lt;alkalmazásnév >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="341d6-132">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="341d6-133">A DNS-rekordok lapon jegyezze fel hello rekord típusú hello toomigrate kívánt DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="341d6-133">In your DNS records page, note hello record type of hello DNS name you want toomigrate.</span></span> <span data-ttu-id="341d6-134">App Service leképezéseket a CNAME és A rekordok támogatja.</span><span class="sxs-lookup"><span data-stu-id="341d6-134">App Service supports mappings from CNAME and A records.</span></span>

### <a name="enable-hello-domain-for-your-app"></a><span data-ttu-id="341d6-135">Az alkalmazás hello tartomány engedélyezése</span><span class="sxs-lookup"><span data-stu-id="341d6-135">Enable hello domain for your app</span></span>

<span data-ttu-id="341d6-136">A hello [Azure-portálon](https://portal.azure.com), a bal oldali navigációs hello app lap hello, válassza ki **egyéni tartományok**.</span><span class="sxs-lookup"><span data-stu-id="341d6-136">In hello [Azure portal](https://portal.azure.com), in hello left navigation of hello app page, select **Custom domains**.</span></span> 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="341d6-138">A hello **egyéni tartományok** lapra, jelölje be hello  **+**  ikon következő túl**állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="341d6-138">In hello **Custom domains** page, select hello **+** icon next too**Add hostname**.</span></span>

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="341d6-140">Típus hello teljesen minősített tartománynevét hello TXT rekord, például a hozzáadott `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="341d6-140">Type hello fully qualified domain name that you added hello TXT record for, such as `www.contoso.com`.</span></span> <span data-ttu-id="341d6-141">Egy helyettesítő tartomány (például \*. contoso.com), minden DNS-neve, amely megfelel a hello helyettesítő tartomány is használhatja.</span><span class="sxs-lookup"><span data-stu-id="341d6-141">For a wildcard domain (like \*.contoso.com), you can use any DNS name that matches hello wildcard domain.</span></span> 

<span data-ttu-id="341d6-142">Válassza ki **érvényesítése**.</span><span class="sxs-lookup"><span data-stu-id="341d6-142">Select **Validate**.</span></span>

<span data-ttu-id="341d6-143">Hello **állomásnév hozzáadása** gomb aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="341d6-143">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="341d6-144">Győződjön meg arról, hogy **állomásnév rekordtípus** toohello DNS-rekord típusát toomigrate kívánt van beállítva.</span><span class="sxs-lookup"><span data-stu-id="341d6-144">Make sure that **Hostname record type** is set toohello DNS record type you want toomigrate.</span></span>

<span data-ttu-id="341d6-145">Válassza ki **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="341d6-145">Select **Add hostname**.</span></span>

![DNS-név toohello alkalmazás hozzáadása](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="341d6-147">Hello hello app kerülnek új állomásnév toobe némi időbe telhet **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="341d6-147">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="341d6-148">Próbálja meg frissíteni a hello böngésző tooupdate hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="341d6-148">Try refreshing hello browser tooupdate hello data.</span></span>

![CNAME rekord hozzáadva](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="341d6-150">Az egyéni DNS-neve most már engedélyezve van az Azure alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="341d6-150">Your custom DNS name is now enabled in your Azure app.</span></span> 

## <a name="remap-hello-active-dns-name"></a><span data-ttu-id="341d6-151">Adja meg újból hello aktív DNS-név</span><span class="sxs-lookup"><span data-stu-id="341d6-151">Remap hello active DNS name</span></span>

<span data-ttu-id="341d6-152">hello csak dolog bal oldali toodo van ismételt leképezését, az aktív DNS-rekord toopoint tooApp szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="341d6-152">hello only thing left toodo is remapping your active DNS record toopoint tooApp Service.</span></span> <span data-ttu-id="341d6-153">Jobb most, még mindig mutat tooyour régi helyről.</span><span class="sxs-lookup"><span data-stu-id="341d6-153">Right now, it still points tooyour old site.</span></span>

<a name="info"></a>

### <a name="copy-hello-apps-ip-address-a-record-only"></a><span data-ttu-id="341d6-154">Másolja a hello app IP-cím (csak egy a rekordot)</span><span class="sxs-lookup"><span data-stu-id="341d6-154">Copy hello app's IP address (A record only)</span></span>

<span data-ttu-id="341d6-155">Ha egy CNAME rekordot a rendszer újramegfeleltetése, hagyja ki az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="341d6-155">If you are remapping a CNAME record, skip this section.</span></span> 

<span data-ttu-id="341d6-156">tooremap egy A rekordot kell hello App Service alkalmazás külső IP-címet, amely megjelenik-e a hello **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="341d6-156">tooremap an A record, you need hello App Service app's external IP address, which is shown in hello **Custom domains** page.</span></span>

<span data-ttu-id="341d6-157">Bezárás hello **állomásnév hozzáadása** lap választásával **X** hello jobb felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="341d6-157">Close hello **Add hostname** page by selecting **X** in hello upper-right corner.</span></span> 

<span data-ttu-id="341d6-158">A hello **egyéni tartományok** lapján hello app IP-cím másolásához.</span><span class="sxs-lookup"><span data-stu-id="341d6-158">In hello **Custom domains** page, copy hello app's IP address.</span></span>

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-hello-dns-record"></a><span data-ttu-id="341d6-160">Hello DNS-bejegyzésének frissítése</span><span class="sxs-lookup"><span data-stu-id="341d6-160">Update hello DNS record</span></span>

<span data-ttu-id="341d6-161">Hello DNS rekordok lap a tartomány szolgáltató válassza hello DNS-rekord tooremap.</span><span class="sxs-lookup"><span data-stu-id="341d6-161">Back in hello DNS records page of your domain provider, select hello DNS record tooremap.</span></span>

<span data-ttu-id="341d6-162">A hello `contoso.com` : Példa a gyökérkönyvtár, megfeleltetheti hello A vagy CNAME rekord a következő táblázat hello hello példák mint:</span><span class="sxs-lookup"><span data-stu-id="341d6-162">For hello `contoso.com` root domain example, remap hello A or CNAME record like hello examples in hello following table:</span></span> 

| <span data-ttu-id="341d6-163">FQDN-példa</span><span class="sxs-lookup"><span data-stu-id="341d6-163">FQDN example</span></span> | <span data-ttu-id="341d6-164">Bejegyzéstípus</span><span class="sxs-lookup"><span data-stu-id="341d6-164">Record type</span></span> | <span data-ttu-id="341d6-165">Gazdagép</span><span class="sxs-lookup"><span data-stu-id="341d6-165">Host</span></span> | <span data-ttu-id="341d6-166">Érték</span><span class="sxs-lookup"><span data-stu-id="341d6-166">Value</span></span> |
| - | - | - | - |
| <span data-ttu-id="341d6-167">a contoso.com (root)</span><span class="sxs-lookup"><span data-stu-id="341d6-167">contoso.com (root)</span></span> | <span data-ttu-id="341d6-168">A</span><span class="sxs-lookup"><span data-stu-id="341d6-168">A</span></span> | `@` | <span data-ttu-id="341d6-169">IP-cím a [másolási hello app IP-cím](#info)</span><span class="sxs-lookup"><span data-stu-id="341d6-169">IP address from [Copy hello app's IP address](#info)</span></span> |
| <span data-ttu-id="341d6-170">a www.contoso.com (rész)</span><span class="sxs-lookup"><span data-stu-id="341d6-170">www.contoso.com (sub)</span></span> | <span data-ttu-id="341d6-171">CNAME</span><span class="sxs-lookup"><span data-stu-id="341d6-171">CNAME</span></span> | `www` | <span data-ttu-id="341d6-172">_&lt;alkalmazásnév >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="341d6-172">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="341d6-173">\*. contoso.com (helyettesítő)</span><span class="sxs-lookup"><span data-stu-id="341d6-173">\*.contoso.com (wildcard)</span></span> | <span data-ttu-id="341d6-174">CNAME</span><span class="sxs-lookup"><span data-stu-id="341d6-174">CNAME</span></span> | _\*_ | <span data-ttu-id="341d6-175">_&lt;alkalmazásnév >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="341d6-175">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="341d6-176">A beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="341d6-176">Save your settings.</span></span>

<span data-ttu-id="341d6-177">DNS-lekérdezések feloldása tooyour App Service-alkalmazást, akkor fordul elő, DNS-propagálás után azonnal kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="341d6-177">DNS queries should start resolving tooyour App Service app immediately after DNS propagation happens.</span></span>

## <a name="next-steps"></a><span data-ttu-id="341d6-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="341d6-178">Next steps</span></span>

<span data-ttu-id="341d6-179">Ismerje meg, hogyan toobind egyéni SSL tanúsítvány tooApp szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="341d6-179">Learn how toobind a custom SSL certificate tooApp Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="341d6-180">Egy meglévő egyéni SSL tanúsítvány tooAzure webalkalmazások kötése</span><span class="sxs-lookup"><span data-stu-id="341d6-180">Bind an existing custom SSL certificate tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
