---
title: "egy meglévő egyéni DNS aaaMap tooAzure webalkalmazások name |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd egy meglévő egyéni DNS-tartomány (kreatív tartomány) tooa webalkalmazás, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazás neve."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2c4eea3c56c758ca11355554321ffa52dd2c6b9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="map-an-existing-custom-dns-name-tooazure-web-apps"></a><span data-ttu-id="c39ee-103">Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése</span><span class="sxs-lookup"><span data-stu-id="c39ee-103">Map an existing custom DNS name tooAzure Web Apps</span></span>

<span data-ttu-id="c39ee-104">Az [Azure Web Apps](app-service-web-overview.md) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c39ee-104">[Azure Web Apps](app-service-web-overview.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="c39ee-105">Az oktatóanyag bemutatja, hogyan toomap egy meglévő egyéni DNS name tooAzure webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c39ee-105">This tutorial shows you how toomap an existing custom DNS name tooAzure Web Apps.</span></span>

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

<span data-ttu-id="c39ee-107">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="c39ee-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c39ee-108">Altartomány leképezése (például `www.contoso.com`) egy olyan CNAME rekordot használatával</span><span class="sxs-lookup"><span data-stu-id="c39ee-108">Map a subdomain (for example, `www.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="c39ee-109">Egy legfelső szintű tartomány leképezése (például `contoso.com`) az A rekord használatával</span><span class="sxs-lookup"><span data-stu-id="c39ee-109">Map a root domain (for example, `contoso.com`) by using an A record</span></span>
> * <span data-ttu-id="c39ee-110">Rendelni egy helyettesítő tartományt (például `*.contoso.com`) egy olyan CNAME rekordot használatával</span><span class="sxs-lookup"><span data-stu-id="c39ee-110">Map a wildcard domain (for example, `*.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="c39ee-111">Tartomány leképezése parancsfájlokkal automatizálhatja</span><span class="sxs-lookup"><span data-stu-id="c39ee-111">Automate domain mapping with scripts</span></span>

<span data-ttu-id="c39ee-112">Választhat egy **CNAME rekord** vagy egy **rekord** toomap egy egyéni DNS tooApp szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="c39ee-112">You can use either a **CNAME record** or an **A record** toomap a custom DNS name tooApp Service.</span></span> 

> [!NOTE]
> <span data-ttu-id="c39ee-113">Azt javasoljuk, hogy használ egy olyan CNAME REKORDOT a legfelső szintű tartomány kivételével az összes egyéni DNS-nevek (például `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="c39ee-113">We recommend that you use a CNAME for all custom DNS names except a root domain (for example, `contoso.com`).</span></span>

<span data-ttu-id="c39ee-114">toomigrate élő webhelyet, és a DNS tartomány neve tooApp szolgáltatás, lásd: [áttelepítése egy aktív DNS-név tooAzure App Service](app-service-custom-domain-name-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="c39ee-114">toomigrate a live site and its DNS domain name tooApp Service, see [Migrate an active DNS name tooAzure App Service](app-service-custom-domain-name-migrate.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c39ee-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c39ee-115">Prerequisites</span></span>

<span data-ttu-id="c39ee-116">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="c39ee-116">toocomplete this tutorial:</span></span>

* <span data-ttu-id="c39ee-117">[App Service-alkalmazás létrehozása](/azure/app-service/), vagy egy másik az oktatóanyaghoz létrehozott alkalmazást használni.</span><span class="sxs-lookup"><span data-stu-id="c39ee-117">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>
* <span data-ttu-id="c39ee-118">Vásároljon egy tartomány nevét, és ellenőrizze, hogy hozzáférési toohello DNS beállításjegyzék a tartomány-szolgáltató (például a GoDaddy).</span><span class="sxs-lookup"><span data-stu-id="c39ee-118">Purchase a domain name and make sure you have access toohello DNS registry for your domain provider (such as GoDaddy).</span></span>

  <span data-ttu-id="c39ee-119">Például tooadd DNS-bejegyzéseket `contoso.com` és `www.contoso.com`, képes tooconfigure hello DNS-beállításainak hello kell `contoso.com` gyökértartomány.</span><span class="sxs-lookup"><span data-stu-id="c39ee-119">For example, tooadd DNS entries for `contoso.com` and `www.contoso.com`, you must be able tooconfigure hello DNS settings for hello `contoso.com` root domain.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c39ee-120">Ha még nem rendelkezik egy meglévő tartomány nevét, érdemes lehet [megvásárlásáról a tartomány használatával hello Azure-portálon](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="c39ee-120">If you don't have an existing domain name, consider [purchasing a domain using hello Azure portal](custom-dns-web-site-buydomains-web-app.md).</span></span> 

## <a name="prepare-hello-app"></a><span data-ttu-id="c39ee-121">Hello alkalmazás előkészítése</span><span class="sxs-lookup"><span data-stu-id="c39ee-121">Prepare hello app</span></span>

<span data-ttu-id="c39ee-122">egy egyéni DNS neve tooa webalkalmazás, webes alkalmazás hello toomap [App Service-csomag](https://azure.microsoft.com/pricing/details/app-service/) fizetős rétegben kell lennie (**megosztott**, **alapvető**, **szabványos**, vagy  **Prémium szintű**).</span><span class="sxs-lookup"><span data-stu-id="c39ee-122">toomap a custom DNS name tooa web app, hello web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="c39ee-123">Ebben a lépésben biztosíthatja, hogy az App Service alkalmazás van hello hello támogatott tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="c39ee-123">In this step, you make sure that hello App Service app is in hello supported pricing tier.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="c39ee-124">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="c39ee-124">Sign in tooAzure</span></span>

<span data-ttu-id="c39ee-125">Nyissa meg hello [Azure-portálon](https://portal.azure.com) és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c39ee-125">Open hello [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-toohello-app-in-hello-azure-portal"></a><span data-ttu-id="c39ee-126">Keresse meg az alkalmazás toohello hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c39ee-126">Navigate toohello app in hello Azure portal</span></span>

<span data-ttu-id="c39ee-127">Hello bal oldali menüben válassza ki a **alkalmazásszolgáltatások**, majd válassza ki a hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="c39ee-127">From hello left menu, select **App Services**, and then select hello name of hello app.</span></span>

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="c39ee-129">Az App Service alkalmazás hello hello felügyeleti oldal akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c39ee-129">You see hello management page of hello App Service app.</span></span>  

<a name="checkpricing"></a>

### <a name="check-hello-pricing-tier"></a><span data-ttu-id="c39ee-130">IP-címek hello ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c39ee-130">Check hello pricing tier</span></span>

<span data-ttu-id="c39ee-131">A bal oldali navigációs hello app lap hello, görgessen a toohello **beállítások** válassza ki azt **vertikális felskálázás (App Service-csomag)**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-131">In hello left navigation of hello app page, scroll toohello **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Méretezett menü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="c39ee-133">hello alkalmazás jelenlegi rétegtől kék szegély ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="c39ee-133">hello app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="c39ee-134">Ellenőrizze, hogy hello alkalmazáshoz nincs hello toomake **szabad** réteg.</span><span class="sxs-lookup"><span data-stu-id="c39ee-134">Check toomake sure that hello app is not in hello **Free** tier.</span></span> <span data-ttu-id="c39ee-135">Hello nem támogatja az egyéni DNS **szabad** réteg.</span><span class="sxs-lookup"><span data-stu-id="c39ee-135">Custom DNS is not supported in hello **Free** tier.</span></span> 

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="c39ee-137">Ha hello App Service-csomag nincs **szabad**zárja be, hello **válasszon tarifacsomagot** lapon, és hagyja ki túl[képezi le egy olyan CNAME rekordot](#cname).</span><span class="sxs-lookup"><span data-stu-id="c39ee-137">If hello App Service plan is not **Free**, close hello **Choose your pricing tier** page and skip too[Map a CNAME record](#cname).</span></span>

<a name="scaleup"></a>

### <a name="scale-up-hello-app-service-plan"></a><span data-ttu-id="c39ee-138">Vertikális felskálázás hello App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="c39ee-138">Scale up hello App Service plan</span></span>

<span data-ttu-id="c39ee-139">Válassza ki valamelyik hello nem szabad rétegek (**megosztott**, **alapvető**, **szabványos**, vagy **prémium**).</span><span class="sxs-lookup"><span data-stu-id="c39ee-139">Select any of hello non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="c39ee-140">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c39ee-140">Click **Select**.</span></span>

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="c39ee-142">Amikor megjelenik a következő értesítési hello, hello skálázási művelet be nem fejeződött.</span><span class="sxs-lookup"><span data-stu-id="c39ee-142">When you see hello following notification, hello scale operation is complete.</span></span>

![Skálázási művelet megerősítése](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a><span data-ttu-id="c39ee-144">A CNAME rekord leképezése</span><span class="sxs-lookup"><span data-stu-id="c39ee-144">Map a CNAME record</span></span>

<span data-ttu-id="c39ee-145">Hello oktatóanyag példában hello CNAME rekord hozzáadása `www` altartomány (például `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="c39ee-145">In hello tutorial example, you add a CNAME record for hello `www` subdomain (for example, `www.contoso.com`).</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a><span data-ttu-id="c39ee-146">Hello CNAME rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="c39ee-146">Create hello CNAME record</span></span>

<span data-ttu-id="c39ee-147">Adja hozzá egy CNAME rekord toomap egy altartomány toohello alkalmazás alapértelmezett állomásnév (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="c39ee-147">Add a CNAME record toomap a subdomain toohello app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="c39ee-148">A hello `www.contoso.com` tartomány például adjon hozzá egy CNAME rekordot, amely leképezhető hello neve `www` túl`<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c39ee-148">For hello `www.contoso.com` domain example, add a CNAME record that maps hello name `www` too`<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="c39ee-149">Miután hozzáadta a hello CNAME, hello DNS-rekordok oldalon néz ki a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="c39ee-149">After you add hello CNAME, hello DNS records page looks like hello following example:</span></span>

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-hello-cname-record-mapping-in-azure"></a><span data-ttu-id="c39ee-151">Engedélyezett hello CNAME rekord-hozzárendelést az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c39ee-151">Enable hello CNAME record mapping in Azure</span></span>

<span data-ttu-id="c39ee-152">Hello hello app lap navigációs maradtak hello Azure-portálon, válassza ki **egyéni tartományok**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-152">In hello left navigation of hello app page in hello Azure portal, select **Custom domains**.</span></span> 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="c39ee-154">A hello **egyéni tartományok** lap hello alkalmazás hozzáadása a hello teljesen minősített egyéni DNS-nevét (`www.contoso.com`) toohello listája.</span><span class="sxs-lookup"><span data-stu-id="c39ee-154">In hello **Custom domains** page of hello app, add hello fully qualified custom DNS name (`www.contoso.com`) toohello list.</span></span>

<span data-ttu-id="c39ee-155">Jelölje be hello  **+**  ikon következő túl**állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-155">Select hello **+** icon next too**Add hostname**.</span></span>

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="c39ee-157">Típus hello teljesen minősített tartománynevét egy CNAME rekordot, például a hozzáadott `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="c39ee-157">Type hello fully qualified domain name that you added a CNAME record for, such as `www.contoso.com`.</span></span> 

<span data-ttu-id="c39ee-158">Válassza ki **érvényesítése**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-158">Select **Validate**.</span></span>

<span data-ttu-id="c39ee-159">Hello **állomásnév hozzáadása** gomb aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="c39ee-159">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="c39ee-160">Győződjön meg arról, hogy **állomásnév rekordtípus** értéke túl**CNAME (www.example.com vagy bármely altartomány)**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-160">Make sure that **Hostname record type** is set too**CNAME (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="c39ee-161">Válassza ki **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-161">Select **Add hostname**.</span></span>

![DNS-név toohello alkalmazás hozzáadása](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="c39ee-163">Hello hello app kerülnek új állomásnév toobe némi időbe telhet **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="c39ee-163">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="c39ee-164">Próbálja meg frissíteni a hello böngésző tooupdate hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="c39ee-164">Try refreshing hello browser tooupdate hello data.</span></span>

![CNAME rekord hozzáadva](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="c39ee-166">Ha nem talált meg egy lépést, vagy elgépelte valahol korábban, egy ellenőrzési hiba hello hello lap alsó részén látható.</span><span class="sxs-lookup"><span data-stu-id="c39ee-166">If you missed a step or made a typo somewhere earlier, you see a verification error at hello bottom of hello page.</span></span>

![Ellenőrzési hiba](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a><span data-ttu-id="c39ee-168">Az A rekord leképezése</span><span class="sxs-lookup"><span data-stu-id="c39ee-168">Map an A record</span></span>

<span data-ttu-id="c39ee-169">Hello oktatóanyag példában hello legfelső szintű tartomány az A rekord hozzáadása (például `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="c39ee-169">In hello tutorial example, you add an A record for hello root domain (for example, `contoso.com`).</span></span> 

<a name="info"></a>

### <a name="copy-hello-apps-ip-address"></a><span data-ttu-id="c39ee-170">Hello app IP-cím másolása</span><span class="sxs-lookup"><span data-stu-id="c39ee-170">Copy hello app's IP address</span></span>

<span data-ttu-id="c39ee-171">toomap egy A rekordot kell hello app külső IP-címet.</span><span class="sxs-lookup"><span data-stu-id="c39ee-171">toomap an A record, you need hello app's external IP address.</span></span> <span data-ttu-id="c39ee-172">Az IP-cím hello alkalmazásban található **egyéni tartományok** hello Azure portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="c39ee-172">You can find this IP address in hello app's **Custom domains** page in hello Azure portal.</span></span>

<span data-ttu-id="c39ee-173">Hello hello app lap navigációs maradtak hello Azure-portálon, válassza ki **egyéni tartományok**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-173">In hello left navigation of hello app page in hello Azure portal, select **Custom domains**.</span></span> 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="c39ee-175">A hello **egyéni tartományok** lapján hello app IP-cím másolásához.</span><span class="sxs-lookup"><span data-stu-id="c39ee-175">In hello **Custom domains** page, copy hello app's IP address.</span></span>

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-a-record"></a><span data-ttu-id="c39ee-177">Hello A rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="c39ee-177">Create hello A record</span></span>

<span data-ttu-id="c39ee-178">toomap egy A rekord tooan alkalmazás, az App Service csak **két** DNS-rekordokat:</span><span class="sxs-lookup"><span data-stu-id="c39ee-178">toomap an A record tooan app, App Service requires **two** DNS records:</span></span>

- <span data-ttu-id="c39ee-179">Egy **A** toomap toohello app IP-cím rögzítése.</span><span class="sxs-lookup"><span data-stu-id="c39ee-179">An **A** record toomap toohello app's IP address.</span></span>
- <span data-ttu-id="c39ee-180">A **TXT** toomap toohello alkalmazás alapértelmezett állomásnév rögzítse `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c39ee-180">A **TXT** record toomap toohello app's default hostname `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="c39ee-181">App Service használja ezt a rekordot csak egyidejűleg konfigurációs tooverify, hogy Ön a tulajdonosa hello egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="c39ee-181">App Service uses this record only at configuration time, tooverify that you own hello custom domain.</span></span> <span data-ttu-id="c39ee-182">Miután az egyéni tartomány érvényesítve, és az App Service-ben konfigurált, törölheti a TXT-rekord.</span><span class="sxs-lookup"><span data-stu-id="c39ee-182">After your custom domain is validated and configured in App Service, you can delete this TXT record.</span></span> 

<span data-ttu-id="c39ee-183">A hello `contoso.com` tartomány például hello és TXT-rekordok megfelelően a következő táblázat toohello létrehozása (`@` általában jelöli hello gyökértartomány).</span><span class="sxs-lookup"><span data-stu-id="c39ee-183">For hello `contoso.com` domain example, create hello A and TXT records according toohello following table (`@` typically represents hello root domain).</span></span> 

| <span data-ttu-id="c39ee-184">Bejegyzéstípus</span><span class="sxs-lookup"><span data-stu-id="c39ee-184">Record type</span></span> | <span data-ttu-id="c39ee-185">Gazdagép</span><span class="sxs-lookup"><span data-stu-id="c39ee-185">Host</span></span> | <span data-ttu-id="c39ee-186">Érték</span><span class="sxs-lookup"><span data-stu-id="c39ee-186">Value</span></span> |
| - | - | - |
| <span data-ttu-id="c39ee-187">A</span><span class="sxs-lookup"><span data-stu-id="c39ee-187">A</span></span> | `@` | <span data-ttu-id="c39ee-188">IP-cím a [másolási hello app IP-cím](#info)</span><span class="sxs-lookup"><span data-stu-id="c39ee-188">IP address from [Copy hello app's IP address](#info)</span></span> |
| <span data-ttu-id="c39ee-189">TXT</span><span class="sxs-lookup"><span data-stu-id="c39ee-189">TXT</span></span> | `@` | `<app_name>.azurewebsites.net` |

<span data-ttu-id="c39ee-190">Hello bejegyzések felvételekor hello DNS-rekordok oldalon néz ki a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="c39ee-190">When hello records are added, hello DNS records page looks like hello following example:</span></span>

![DNS-rekordok lap](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-hello-a-record-mapping-in-hello-app"></a><span data-ttu-id="c39ee-192">Egy rekord leképezése hello alkalmazásban hello engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c39ee-192">Enable hello A record mapping in hello app</span></span>

<span data-ttu-id="c39ee-193">Vissza a hello app **egyéni tartományok** hello Azure-portálon lapján hozzáadása hello teljesen minősített egyéni DNS-nevét (például `contoso.com`) toohello listája.</span><span class="sxs-lookup"><span data-stu-id="c39ee-193">Back in hello app's **Custom domains** page in hello Azure portal, add hello fully qualified custom DNS name (for example, `contoso.com`) toohello list.</span></span>

<span data-ttu-id="c39ee-194">Jelölje be hello  **+**  ikon következő túl**állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-194">Select hello **+** icon next too**Add hostname**.</span></span>

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

<span data-ttu-id="c39ee-196">Típus hello teljesen minősített tartománynevét hello A rekord, például a konfigurált `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="c39ee-196">Type hello fully qualified domain name that you configured hello A record for, such as `contoso.com`.</span></span>

<span data-ttu-id="c39ee-197">Válassza ki **érvényesítése**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-197">Select **Validate**.</span></span>

<span data-ttu-id="c39ee-198">Hello **állomásnév hozzáadása** gomb aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="c39ee-198">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="c39ee-199">Győződjön meg arról, hogy **állomásnév rekordtípus** értéke túl**egy olyan rekordot (example.com)**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-199">Make sure that **Hostname record type** is set too**A record (example.com)**.</span></span>

<span data-ttu-id="c39ee-200">Válassza ki **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-200">Select **Add hostname**.</span></span>

![DNS-név toohello alkalmazás hozzáadása](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

<span data-ttu-id="c39ee-202">Hello hello app kerülnek új állomásnév toobe némi időbe telhet **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="c39ee-202">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="c39ee-203">Próbálja meg frissíteni a hello böngésző tooupdate hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="c39ee-203">Try refreshing hello browser tooupdate hello data.</span></span>

![Egy hozzáadott bejegyzés](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

<span data-ttu-id="c39ee-205">Ha nem talált meg egy lépést, vagy elgépelte valahol korábban, egy ellenőrzési hiba hello hello lap alsó részén látható.</span><span class="sxs-lookup"><span data-stu-id="c39ee-205">If you missed a step or made a typo somewhere earlier, you see a verification error at hello bottom of hello page.</span></span>

![Ellenőrzési hiba](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a><span data-ttu-id="c39ee-207">A helyettesítő tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="c39ee-207">Map a wildcard domain</span></span>

<span data-ttu-id="c39ee-208">Hello oktatóanyag példában leképez egy [helyettesítő DNS-név](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (például `*.contoso.com`) toohello App Service alkalmazáshoz egy olyan CNAME rekordot hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="c39ee-208">In hello tutorial example, you map a [wildcard DNS name](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (for example, `*.contoso.com`) toohello App Service app by adding a CNAME record.</span></span> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a><span data-ttu-id="c39ee-209">Hello CNAME rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="c39ee-209">Create hello CNAME record</span></span>

<span data-ttu-id="c39ee-210">Adja hozzá egy CNAME rekord toomap egy helyettesítő karakteres nevet toohello alkalmazás alapértelmezett állomásnév (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="c39ee-210">Add a CNAME record toomap a wildcard name toohello app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="c39ee-211">A hello `*.contoso.com` tartomány például hello CNAME rekord leképezése hello neve `*` túl`<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c39ee-211">For hello `*.contoso.com` domain example, hello CNAME record will map hello name `*` too`<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="c39ee-212">Hello CNAME felvételekor hello DNS-rekordok oldalon néz ki a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="c39ee-212">When hello CNAME is added, hello DNS records page looks like hello following example:</span></span>

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-hello-cname-record-mapping-in-hello-app"></a><span data-ttu-id="c39ee-214">Hello CNAME rekord hozzárendelésének hello alkalmazásban engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c39ee-214">Enable hello CNAME record mapping in hello app</span></span>

<span data-ttu-id="c39ee-215">Mostantól hozzáadhatja azok bármely altartomány megfelelő hello helyettesítő neve toohello app (például `sub1.contoso.com` és `sub2.contoso.com` megfelelő `*.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="c39ee-215">You can now add any subdomain that matches hello wildcard name toohello app (for example, `sub1.contoso.com` and `sub2.contoso.com` match `*.contoso.com`).</span></span> 

<span data-ttu-id="c39ee-216">Hello hello app lap navigációs maradtak hello Azure-portálon, válassza ki **egyéni tartományok**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-216">In hello left navigation of hello app page in hello Azure portal, select **Custom domains**.</span></span> 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="c39ee-218">Jelölje be hello  **+**  ikon következő túl**állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-218">Select hello **+** icon next too**Add hostname**.</span></span>

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="c39ee-220">Írjon be egy teljesen minősített tartománynevet, amely megfelel a hello helyettesítő tartomány (például `sub1.contoso.com`), majd válassza ki **ellenőrzése**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-220">Type a fully qualified domain name that matches hello wildcard domain (for example, `sub1.contoso.com`), and then select **Validate**.</span></span>

<span data-ttu-id="c39ee-221">Hello **állomásnév hozzáadása** gomb aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="c39ee-221">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="c39ee-222">Győződjön meg arról, hogy **állomásnév rekordtípus** értéke túl**CNAME rekord (www.example.com vagy bármely altartomány)**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-222">Make sure that **Hostname record type** is set too**CNAME record (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="c39ee-223">Válassza ki **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c39ee-223">Select **Add hostname**.</span></span>

![DNS-név toohello alkalmazás hozzáadása](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

<span data-ttu-id="c39ee-225">Hello hello app kerülnek új állomásnév toobe némi időbe telhet **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="c39ee-225">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="c39ee-226">Próbálja meg frissíteni a hello böngésző tooupdate hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="c39ee-226">Try refreshing hello browser tooupdate hello data.</span></span>

<span data-ttu-id="c39ee-227">Jelölje be hello  **+**  ikon újra tooadd egy másik állomásnév, amely megfelel a hello helyettesítő tartomány.</span><span class="sxs-lookup"><span data-stu-id="c39ee-227">Select hello **+** icon again tooadd another hostname that matches hello wildcard domain.</span></span> <span data-ttu-id="c39ee-228">Adja hozzá például `sub2.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="c39ee-228">For example, add `sub2.contoso.com`.</span></span>

![CNAME rekord hozzáadva](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a><span data-ttu-id="c39ee-230">A böngészőben tesztelése</span><span class="sxs-lookup"><span data-stu-id="c39ee-230">Test in browser</span></span>

<span data-ttu-id="c39ee-231">Keresse meg a korábban megadott értékektől toohello DNS-nevek (például `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, és `sub2.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="c39ee-231">Browse toohello DNS name(s) that you configured earlier (for example, `contoso.com`,  `www.contoso.com`, `sub1.contoso.com`, and `sub2.contoso.com`).</span></span>

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a><span data-ttu-id="c39ee-233">Parancsfájlok automatizálásához</span><span class="sxs-lookup"><span data-stu-id="c39ee-233">Automate with scripts</span></span>

<span data-ttu-id="c39ee-234">Automatizálható a parancsfájlok, az egyéni tartományok felügyeleti hello segítségével [Azure CLI](/cli/azure/install-azure-cli) vagy [Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c39ee-234">You can automate management of custom domains with scripts, using hello [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="azure-cli"></a><span data-ttu-id="c39ee-235">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c39ee-235">Azure CLI</span></span> 

<span data-ttu-id="c39ee-236">a következő parancs hello ad hozzá egy konfigurált egyéni DNS nevét tooan App Service-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c39ee-236">hello following command adds a configured custom DNS name tooan App Service app.</span></span> 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

<span data-ttu-id="c39ee-237">További információkért lásd: [képezi le egy egyéni tartomány tooa webalkalmazás](scripts/app-service-cli-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="c39ee-237">For more information, see [Map a custom domain tooa web app](scripts/app-service-cli-configure-custom-domain.md).</span></span> 

### <a name="azure-powershell"></a><span data-ttu-id="c39ee-238">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c39ee-238">Azure PowerShell</span></span> 

<span data-ttu-id="c39ee-239">a következő parancs hello ad hozzá egy konfigurált egyéni DNS nevét tooan App Service-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c39ee-239">hello following command adds a configured custom DNS name tooan App Service app.</span></span> 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

<span data-ttu-id="c39ee-240">További információkért lásd: [hozzárendelése egyéni tartományhoz tooa webalkalmazás](scripts/app-service-powershell-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="c39ee-240">For more information, see [Assign a custom domain tooa web app](scripts/app-service-powershell-configure-custom-domain.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c39ee-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c39ee-241">Next steps</span></span>

<span data-ttu-id="c39ee-242">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="c39ee-242">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c39ee-243">A CNAME rekord használatával altartomány térkép</span><span class="sxs-lookup"><span data-stu-id="c39ee-243">Map a subdomain by using a CNAME record</span></span>
> * <span data-ttu-id="c39ee-244">Az A rekord használatával a legfelső szintű tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="c39ee-244">Map a root domain by using an A record</span></span>
> * <span data-ttu-id="c39ee-245">Térkép egy helyettesítő tartomány CNAME rekord használatával</span><span class="sxs-lookup"><span data-stu-id="c39ee-245">Map a wildcard domain by using a CNAME record</span></span>
> * <span data-ttu-id="c39ee-246">Tartomány leképezése parancsfájlokkal automatizálhatja</span><span class="sxs-lookup"><span data-stu-id="c39ee-246">Automate domain mapping with scripts</span></span>

<span data-ttu-id="c39ee-247">Előzetes toohello következő útmutató toolearn hogyan toobind egyéni SSL tanúsítvány tooa webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c39ee-247">Advance toohello next tutorial toolearn how toobind a custom SSL certificate tooa web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c39ee-248">Egy meglévő egyéni SSL tanúsítvány tooAzure webalkalmazások kötése</span><span class="sxs-lookup"><span data-stu-id="c39ee-248">Bind an existing custom SSL certificate tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
