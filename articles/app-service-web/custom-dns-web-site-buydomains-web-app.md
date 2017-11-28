---
title: "egy egyéni tartománynevet, az Azure Web Apps aaaBuy"
description: "Ismerje meg, hogyan toobuy egyéni tartományt adjon egy webalkalmazást az Azure App Service-ben."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 2ff61a3f27020516c917fe105ece99eb2a5754b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a><span data-ttu-id="4e9a3-103">Egy egyéni tartománynevet, az Azure Web Apps megvásárlása</span><span class="sxs-lookup"><span data-stu-id="4e9a3-103">Buy a custom domain name for Azure Web Apps</span></span>

<span data-ttu-id="4e9a3-104">App Service (előzetes verzió) tartományai közvetlenül az Azure-ban kezelt a legfelső szintű tartományoknak.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-104">App Service domains (preview) are top-level domains that are managed directly in Azure.</span></span> <span data-ttu-id="4e9a3-105">Teszik könnyen toomanage egyéni tartományok [Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-105">They make it easy toomanage custom domains for [Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="4e9a3-106">Az oktatóanyag bemutatja, hogyan toobuy egy App Service-tartományhoz, és rendelje hozzá a DNS neve tooAzure webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-106">This tutorial shows you how toobuy an App Service domain and assign DNS names tooAzure Web Apps.</span></span>

<span data-ttu-id="4e9a3-107">Ez a cikk az Azure App Service-(Web Apps, az API Apps, Mobile Apps, a Logic Apps) van.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-107">This article is for Azure App Service (Web Apps, API Apps, Mobile Apps, Logic Apps).</span></span> <span data-ttu-id="4e9a3-108">Azure virtuális gép és az Azure Storage: [rendelje hozzá az App Service tartomány tooAzure VM vagy az Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-108">For Azure VM or Azure Storage, see [Assign App Service domain tooAzure VM or Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span></span> <span data-ttu-id="4e9a3-109">Cloud Services, lásd: [egy egyéni tartománynevet, az Azure-felhőszolgáltatás konfigurálása](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-109">For Cloud Services, see [Configuring a custom domain name for an Azure cloud service](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e9a3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4e9a3-110">Prerequisites</span></span>

<span data-ttu-id="4e9a3-111">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="4e9a3-111">toocomplete this tutorial:</span></span>

* <span data-ttu-id="4e9a3-112">[App Service-alkalmazás létrehozása](/azure/app-service/), vagy egy másik az oktatóanyaghoz létrehozott alkalmazást használni.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-112">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>

## <a name="prepare-hello-app"></a><span data-ttu-id="4e9a3-113">Hello alkalmazás előkészítése</span><span class="sxs-lookup"><span data-stu-id="4e9a3-113">Prepare hello app</span></span>

<span data-ttu-id="4e9a3-114">egyéni tartományok toouse az Azure Web Apps, a webes alkalmazás által [App Service-csomag](https://azure.microsoft.com/pricing/details/app-service/) fizetős rétegben kell lennie (**megosztott**, **alapvető**, **szabványos**, vagy **Prémium**).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-114">toouse custom domains in Azure Web Apps, your web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="4e9a3-115">Ebben a lépésben biztosíthatja, hogy hello webalkalmazás a hello támogatott IP-címek.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-115">In this step, you make sure that hello web app is in hello supported pricing tier.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="4e9a3-116">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="4e9a3-116">Sign in tooAzure</span></span>

<span data-ttu-id="4e9a3-117">Nyissa meg hello [Azure-portálon](https://portal.azure.com) és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-117">Open hello [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-toohello-app-in-hello-azure-portal"></a><span data-ttu-id="4e9a3-118">Keresse meg az alkalmazás toohello hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4e9a3-118">Navigate toohello app in hello Azure portal</span></span>

<span data-ttu-id="4e9a3-119">Hello bal oldali menüben válassza ki a **alkalmazásszolgáltatások**, majd válassza ki a hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-119">From hello left menu, select **App Services**, and then select hello name of hello app.</span></span>

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="4e9a3-121">Az App Service alkalmazás hello hello felügyeleti oldal akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-121">You see hello management page of hello App Service app.</span></span>  

### <a name="check-hello-pricing-tier"></a><span data-ttu-id="4e9a3-122">IP-címek hello ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4e9a3-122">Check hello pricing tier</span></span>

<span data-ttu-id="4e9a3-123">A bal oldali navigációs hello app lap hello, görgessen a toohello **beállítások** válassza ki azt **vertikális felskálázás (App Service-csomag)**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-123">In hello left navigation of hello app page, scroll toohello **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Méretezett menü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="4e9a3-125">hello alkalmazás jelenlegi rétegtől kék szegély ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-125">hello app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="4e9a3-126">Ellenőrizze, hogy hello alkalmazáshoz nincs hello toomake **szabad** réteg.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-126">Check toomake sure that hello app is not in hello **Free** tier.</span></span> <span data-ttu-id="4e9a3-127">Hello nem támogatja az egyéni DNS **szabad** réteg.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-127">Custom DNS is not supported in hello **Free** tier.</span></span> 

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="4e9a3-129">Ha hello App Service-csomag nincs **szabad**zárja be, hello **válasszon tarifacsomagot** lapon, és hagyja ki túl[vásárlás hello tartomány](#buy-the-domain).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-129">If hello App Service plan is not **Free**, close hello **Choose your pricing tier** page and skip too[Buy hello domain](#buy-the-domain).</span></span>

### <a name="scale-up-hello-app-service-plan"></a><span data-ttu-id="4e9a3-130">Vertikális felskálázás hello App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="4e9a3-130">Scale up hello App Service plan</span></span>

<span data-ttu-id="4e9a3-131">Válassza ki valamelyik hello nem szabad rétegek (**megosztott**, **alapvető**, **szabványos**, vagy **prémium**).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-131">Select any of hello non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="4e9a3-132">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-132">Click **Select**.</span></span>

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="4e9a3-134">Amikor megjelenik a következő értesítési hello, hello skálázási művelet be nem fejeződött.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-134">When you see hello following notification, hello scale operation is complete.</span></span>

![Skálázási művelet megerősítése](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-hello-domain"></a><span data-ttu-id="4e9a3-136">Hello tartomány vásárlása</span><span class="sxs-lookup"><span data-stu-id="4e9a3-136">Buy hello domain</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="4e9a3-137">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="4e9a3-137">Sign in tooAzure</span></span>
<span data-ttu-id="4e9a3-138">Nyissa meg hello [Azure-portálon](https://portal.azure.com/) és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-138">Open hello [Azure portal](https://portal.azure.com/) and sign in with your Azure account.</span></span>

### <a name="launch-buy-domains"></a><span data-ttu-id="4e9a3-139">Indítsa el a vásárlás tartományok</span><span class="sxs-lookup"><span data-stu-id="4e9a3-139">Launch Buy domains</span></span>
<span data-ttu-id="4e9a3-140">A hello **Web Apps** fülre, kattintson a webalkalmazás, jelölje be a hello neve **beállítások**, majd válassza ki **egyéni tartományok**</span><span class="sxs-lookup"><span data-stu-id="4e9a3-140">In hello **Web Apps** tab, click hello name of your web app, select **Settings**, and then select **Custom domains**</span></span>
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="4e9a3-141">A hello **egyéni tartományok** kattintson **tartományok megvásárlása**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-141">In hello **Custom domains** page, click **Buy domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-hello-domain-purchase"></a><span data-ttu-id="4e9a3-142">Hello tartomány beszerzési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4e9a3-142">Configure hello domain purchase</span></span>

<span data-ttu-id="4e9a3-143">A hello **App Service tartomány** lap hello **keresési tartomány** mezőbe, a típus hello tartománynevet toobuy, és írja be `Enter`.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-143">In hello **App Service Domain** page, in hello **Search for domain** box, type hello domain name you want toobuy and type `Enter`.</span></span> <span data-ttu-id="4e9a3-144">hello javasolt elérhető tartományok látható hello szövegmező alatt.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-144">hello suggested available domains are shown just below hello text box.</span></span> <span data-ttu-id="4e9a3-145">Válassza ki a kívánt toobuy egy vagy több tartományt.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-145">Select one or more domains you want toobuy.</span></span> 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

<span data-ttu-id="4e9a3-146">Kattintson a hello **elérhetőségi adatai** hello tartomány kapcsolattartási adatokat űrlap kitöltése és.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-146">Click hello **Contact Information** and fill out hello domain's contact information form.</span></span> <span data-ttu-id="4e9a3-147">Ha befejezte, kattintson a **OK** tooreturn toohello App Service tartományban lap.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-147">When finished, click **OK** tooreturn toohello App Service Domain page.</span></span>
   
<span data-ttu-id="4e9a3-148">Fontos, hogy a lehető legtöbb pontossággal minden kötelező mezőt kitöltötte.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-148">It is important that you fill out all required fields with as much accuracy as possible.</span></span> <span data-ttu-id="4e9a3-149">Helytelen adatok elérhetőségét hiba toopurchase tartományok eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-149">Incorrect data for contact information can result in failure toopurchase domains.</span></span> 

<span data-ttu-id="4e9a3-150">Ezután válassza ki a tartomány hello szükséges beállításokat.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-150">Next, select hello desired options for your domain.</span></span> <span data-ttu-id="4e9a3-151">Tekintse meg a következő táblázat ismereteket szeretnének elsajátítani hello:</span><span class="sxs-lookup"><span data-stu-id="4e9a3-151">See hello following table for explanations:</span></span>

| <span data-ttu-id="4e9a3-152">Beállítás</span><span class="sxs-lookup"><span data-stu-id="4e9a3-152">Setting</span></span> | <span data-ttu-id="4e9a3-153">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="4e9a3-153">Suggested Value</span></span> | <span data-ttu-id="4e9a3-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="4e9a3-154">Description</span></span> |
|-|-|-|
|<span data-ttu-id="4e9a3-155">Automatikus megújításához</span><span class="sxs-lookup"><span data-stu-id="4e9a3-155">Auto renew</span></span> | <span data-ttu-id="4e9a3-156">**Bekapcsolás**</span><span class="sxs-lookup"><span data-stu-id="4e9a3-156">**Enable**</span></span> | <span data-ttu-id="4e9a3-157">Évente automatikusan megújítja a szolgáltatás alkalmazástartományban.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-157">Renews your App Service Domain automatically every year.</span></span> <span data-ttu-id="4e9a3-158">A hitelkártya díjfizetéssel ugyanazon beszerzés ár hello a megújítási hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-158">Your credit card is charged hello same purchase price at hello time of renewal.</span></span> |
|<span data-ttu-id="4e9a3-159">Adatvédelem</span><span class="sxs-lookup"><span data-stu-id="4e9a3-159">Privacy protection</span></span> | <span data-ttu-id="4e9a3-160">Bekapcsolás</span><span class="sxs-lookup"><span data-stu-id="4e9a3-160">Enable</span></span> | <span data-ttu-id="4e9a3-161">Részt vevő túl "Adatvédelem", amely hello vételár szerepel _szabad_ (kivéve a beállításjegyzékhez nem támogatják a adatvédelmet, mint például a legfelső szintű tartományoknak _. co.in_, _. Co.uk_, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-161">Opt in too"Privacy protection", which is included in hello purchase price _for free_ (except for top-level domains whose registry do not support privacy protection, such as _.co.in_, _.co.uk_, and so on).</span></span> |
| <span data-ttu-id="4e9a3-162">Rendelje hozzá az alapértelmezett állomásnevek</span><span class="sxs-lookup"><span data-stu-id="4e9a3-162">Assign default hostnames</span></span> | <span data-ttu-id="4e9a3-163">**www** és**@**</span><span class="sxs-lookup"><span data-stu-id="4e9a3-163">**www** and **@**</span></span> | <span data-ttu-id="4e9a3-164">SELECT hello állomásnévkötéseket, szükséges, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-164">Select hello desired hostname bindings, if desired.</span></span> <span data-ttu-id="4e9a3-165">Hello beszerzési műveletek befejeződése után a webalkalmazás a kijelölt hello állomásnevek érhető el.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-165">When hello domain purchase operation is complete, your web app can be accessed at hello selected hostnames.</span></span> <span data-ttu-id="4e9a3-166">Ha hello webalkalmazás mögött található [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), hello beállítás tooassign hello gyökértartomány nem jelenik meg (@), mivel a Traffic Manager nem támogatja A rekordok.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-166">If hello web app is behind [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), you don't see hello option tooassign hello root domain (@), because Traffic Manager does not support A records.</span></span> <span data-ttu-id="4e9a3-167">Módosításokat végezheti el toohello állomásnév hozzárendelések hello tartomány vásárlás befejezése után.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-167">You can make changes toohello hostname assignments after hello domain purchase completes.</span></span> |

### <a name="accept-terms-and-purchase"></a><span data-ttu-id="4e9a3-168">Fogadja el a feltételeket és a vásárlások</span><span class="sxs-lookup"><span data-stu-id="4e9a3-168">Accept terms and purchase</span></span>

<span data-ttu-id="4e9a3-169">Kattintson a **jogi feltételeket** tooreview hello feltételeit és hello díjakat, majd kattintson **megvásárlása**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-169">Click **Legal Terms** tooreview hello terms and hello charges, then click **Buy**.</span></span>

> [!NOTE]
> <span data-ttu-id="4e9a3-170">App Service-tartományok Azure DNS toohost hello tartományt használ.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-170">App Service Domains use Azure DNS toohost hello domains.</span></span> <span data-ttu-id="4e9a3-171">Továbbá tartományi Regisztr toohello, hálózathasználati költségeket az Azure DNS alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-171">In addition toohello domain registration fee, usage charges for Azure DNS apply.</span></span> <span data-ttu-id="4e9a3-172">További információ: [Azure DNS árképzési](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-172">For information, see [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>
>
>

<span data-ttu-id="4e9a3-173">Vissza a hello **App Service tartomány** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-173">Back in hello **App Service Domain** page, click **OK**.</span></span> <span data-ttu-id="4e9a3-174">Amíg hello művelet van folyamatban, a következő értesítések hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="4e9a3-174">While hello operation is in progress, you see hello following notifications:</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-hello-hostnames"></a><span data-ttu-id="4e9a3-175">Hello állomásnevek tesztelése</span><span class="sxs-lookup"><span data-stu-id="4e9a3-175">Test hello hostnames</span></span>

<span data-ttu-id="4e9a3-176">Alapértelmezett állomásnevek tooyour webalkalmazás hozzárendelt, is láthatja az összes kijelölt állomásnév sikeres értesítést.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-176">If you have assigned default hostnames tooyour web app, you also see a success notification for each selected hostname.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

<span data-ttu-id="4e9a3-177">Is láthatja a hello hello kijelölt állomásnevek **egyéni tartományok** lap hello **Hostnames** szakasz.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-177">You also see hello selected hostnames in hello **Custom domains** page, in hello **Hostnames** section.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

<span data-ttu-id="4e9a3-178">tootest hello állomásnevek, keresse meg a felsorolt toohello állomásnevek hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-178">tootest hello hostnames, navigate toohello listed hostnames in hello browser.</span></span> <span data-ttu-id="4e9a3-179">A hello hello példát megelőző képernyőképe, próbálja ki a navigáció too_kontoso.net_ és _www.kontoso.net_.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-179">In hello example in hello preceding screenshot, try navigating too_kontoso.net_ and _www.kontoso.net_.</span></span>

## <a name="assign-hostnames-tooweb-app"></a><span data-ttu-id="4e9a3-180">Rendelje hozzá a gazdagépneveknek tooweb alkalmazás</span><span class="sxs-lookup"><span data-stu-id="4e9a3-180">Assign hostnames tooweb app</span></span>

<span data-ttu-id="4e9a3-181">Ha úgy dönt, hogy nem tooassign egy vagy több alapértelmezett állomásnevek tooyour webalkalmazás során hello folyamat vásárolhat, vagy ha az állomásnév tooassign nem szerepel a listában, bármikor rendelhet egy állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-181">If you choose not tooassign one or more default hostnames tooyour web app during hello purchase process, or if you need tooassign a hostname not listed, you can assign a hostname at anytime.</span></span>

<span data-ttu-id="4e9a3-182">A hello szolgáltatás alkalmazástartományban tooany állomásnevek más webes alkalmazás is hozzárendelhetők.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-182">You can also assign hostnames in hello App Service Domain tooany other web app.</span></span> <span data-ttu-id="4e9a3-183">hello függ, hogy hello szolgáltatás alkalmazástartományban és hello webalkalmazás tartozik toohello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-183">hello steps depend on whether hello App Service Domain and hello web app belong toohello same subscription.</span></span>

- <span data-ttu-id="4e9a3-184">Másik előfizetést: egyéni DNS-rekordok webalkalmazásból hello szolgáltatás alkalmazástartományban toohello például egy beszerzett kívülről tartomány hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-184">Different subscription: Map custom DNS records from hello App Service Domain toohello web app like an externally purchased domain.</span></span> <span data-ttu-id="4e9a3-185">Információ hozzáadása egyéni DNS-nevek tooan App Service-tartomány, a következő témakörben: [egyéni DNS-rekordok kezelése](#custom).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-185">For information on adding custom DNS names tooan App Service Domain, see [Manage custom DNS records](#custom).</span></span> <span data-ttu-id="4e9a3-186">egy külső megvásárolt tartomány tooa webalkalmazás toomap lásd [leképezése egy meglévő egyéni DNS nevét tooAzure webalkalmazások](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-186">toomap an external purchased domain tooa web app, see [Map an existing custom DNS name tooAzure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span> 
- <span data-ttu-id="4e9a3-187">Ugyanahhoz az előfizetéshez: használata hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-187">Same subscription: Use hello following steps.</span></span>

### <a name="launch-add-hostname"></a><span data-ttu-id="4e9a3-188">Indítsa el az állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4e9a3-188">Launch add hostname</span></span>
<span data-ttu-id="4e9a3-189">A hello **alkalmazásszolgáltatások** lapra, válassza hello nevét, amelyet tooassign állomásnevek, válassza ki a webalkalmazás **beállítások**, majd válassza ki **egyéni tartományok**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-189">In hello **App Services** page, select hello name of your web app that you want tooassign hostnames to, select **Settings**, and then select **Custom domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="4e9a3-190">Győződjön meg arról, hogy a megvásárolt tartomány szerepel hello **App Service tartományok** szakaszában, de nem adja meg azt.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-190">Make sure that your purchased domain is listed in hello **App Service Domains** section, but don't select it.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> <span data-ttu-id="4e9a3-191">App Service tartományai ugyanahhoz az előfizetéshez hello web app alkalmazásban látható hello **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-191">All App Service Domains in hello same subscription are shown in hello web app's **Custom domains** page.</span></span> <span data-ttu-id="4e9a3-192">Ha a tartomány hello web app előfizetésben, de nem látható a hello web app **egyéni tartományok** lapján próbálja megnyitni hello **egyéni tartományok** lapon vagy hello weblap frissítése.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-192">If your domain is in hello web app's subscription, but you cannot see it in hello web app's **Custom domains** page, try reopening hello **Custom domains** page or refresh hello webpage.</span></span> <span data-ttu-id="4e9a3-193">Ellenőrizze a hello értesítési Csengő hello Azure-portálon hello tetején folyamat és a létrehozási hibák is.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-193">Also, check hello notification bell at hello top of hello Azure portal for progress or creation failures.</span></span>
>
>

<span data-ttu-id="4e9a3-194">Válassza ki **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-194">Select **Add hostname**.</span></span>

### <a name="configure-hostname"></a><span data-ttu-id="4e9a3-195">Állomásnév konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4e9a3-195">Configure hostname</span></span>
<span data-ttu-id="4e9a3-196">A hello **állomásnév hozzáadása** párbeszédpanelen hello teljesen minősített típusnév az App Service tartomány vagy bármely altartomány.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-196">In hello **Add hostname** dialog, type hello fully qualified domain name of your App Service Domain or any subdomain.</span></span> <span data-ttu-id="4e9a3-197">Példa:</span><span class="sxs-lookup"><span data-stu-id="4e9a3-197">For example:</span></span>

- <span data-ttu-id="4e9a3-198">kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="4e9a3-198">kontoso.net</span></span>
- <span data-ttu-id="4e9a3-199">www.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="4e9a3-199">www.kontoso.net</span></span>
- <span data-ttu-id="4e9a3-200">ABC.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="4e9a3-200">abc.kontoso.net</span></span>

<span data-ttu-id="4e9a3-201">Ha elkészült, válassza ki a **ellenőrzése**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-201">When finished, select **Validate**.</span></span> <span data-ttu-id="4e9a3-202">hello állomásnév rekordtípus automatikusan ki van jelölve, az Ön.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-202">hello hostname record type is automatically selected for you.</span></span>

<span data-ttu-id="4e9a3-203">Válassza ki **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-203">Select **Add hostname**.</span></span>

<span data-ttu-id="4e9a3-204">Hello művelet befejeződése után a hozzárendelt állomásnév hello sikeres értesítést láthatja.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-204">When hello operation is complete, you see a success notification for hello assigned hostname.</span></span>  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a><span data-ttu-id="4e9a3-205">Zárja be az állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4e9a3-205">Close add hostname</span></span>
<span data-ttu-id="4e9a3-206">A hello **állomásnév hozzáadása** lapon adjon meg semmilyen más állomásnév tooyour webalkalmazás, tetszés szerint,.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-206">In hello **Add hostname** page, assign any other hostname tooyour web app, as desired.</span></span> <span data-ttu-id="4e9a3-207">Ha elkészült, zárja be a hello **állomásnév hozzáadása** lap.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-207">When finished, close hello **Add hostname** page.</span></span>

<span data-ttu-id="4e9a3-208">Ekkor megjelenik az alkalmazás újonnan hozzárendelt hello hostname(s) **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-208">You should now see hello newly assigned hostname(s) in your app's **Custom domains** page.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-hello-hostnames"></a><span data-ttu-id="4e9a3-209">Hello állomásnevek tesztelése</span><span class="sxs-lookup"><span data-stu-id="4e9a3-209">Test hello hostnames</span></span>

<span data-ttu-id="4e9a3-210">Keresse meg a felsorolt toohello állomásnevek hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-210">Navigate toohello listed hostnames in hello browser.</span></span> <span data-ttu-id="4e9a3-211">Hello megelőző képernyőfelvétel a hello példában too_abc.kontoso.net_ Navigálás próbálja.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-211">In hello example in hello preceding screenshot, try navigating too_abc.kontoso.net_.</span></span>

<a name="custom" />

## <a name="manage-custom-dns-records"></a><span data-ttu-id="4e9a3-212">Egyéni DNS-rekordok kezelése</span><span class="sxs-lookup"><span data-stu-id="4e9a3-212">Manage custom DNS records</span></span>

<span data-ttu-id="4e9a3-213">Az Azure DNS-rekordok egy App Service-tartomány segítségével felügyelhetők [Azure DNS](https://azure.microsoft.com/services/dns/).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-213">In Azure, DNS records for an App Service Domain are managed using [Azure DNS](https://azure.microsoft.com/services/dns/).</span></span> <span data-ttu-id="4e9a3-214">Hozzáadhat, távolítsa el, és DNS-rekordok frissítése, akárcsak a beszerzett kívülről tartományban.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-214">You can add, remove, and update DNS records, just like for an externally purchased domain.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="4e9a3-215">Nyissa meg az App Service-tartomány</span><span class="sxs-lookup"><span data-stu-id="4e9a3-215">Open App Service Domain</span></span>

<span data-ttu-id="4e9a3-216">Hello Azure-portálon, hello bal oldali menüben válassza ki **több szolgáltatások** > **App Service tartományok**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-216">In hello Azure portal, from hello left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="4e9a3-217">Válassza ki a hello tartomány toomanage.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-217">Select hello domain toomanage.</span></span> 

### <a name="access-dns-zone"></a><span data-ttu-id="4e9a3-218">Hozzáférés DNS-zóna</span><span class="sxs-lookup"><span data-stu-id="4e9a3-218">Access DNS zone</span></span>

<span data-ttu-id="4e9a3-219">Hello tartomány bal oldali menüben válasszon ki **DNS-zóna**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-219">In hello domain's left menu, select **DNS zone**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

<span data-ttu-id="4e9a3-220">Ez a művelet megnyitja hello [DNS-zóna](../dns/dns-zones-records.md) az alkalmazástartomány szolgáltatás az Azure DNS oldalán.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-220">This action opens hello [DNS zone](../dns/dns-zones-records.md) page of your App Service Domain in Azure DNS.</span></span> <span data-ttu-id="4e9a3-221">Információ tooedit DNS-rekordokat, lásd: [hogyan toomanage DNS-zónák az hello Azure-portálon](../dns/dns-operations-dnszones-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4e9a3-221">For information on how tooedit DNS records, see [How toomanage DNS Zones in hello Azure portal](../dns/dns-operations-dnszones-portal.md).</span></span>

## <a name="cancel-purchase-delete-domain"></a><span data-ttu-id="4e9a3-222">Szakítsa meg a beszerzési (delete tartomány)</span><span class="sxs-lookup"><span data-stu-id="4e9a3-222">Cancel purchase (delete domain)</span></span>

<span data-ttu-id="4e9a3-223">App Service tartomány hello vásárol, után öt nap toocancel teljes visszatérítést vásárlását.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-223">After you purchase hello App Service Domain, you have five days toocancel your purchase for a full refund.</span></span> <span data-ttu-id="4e9a3-224">Öt nap után törölheti a hello szolgáltatás alkalmazástartományban, de nem visszatérítést.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-224">After five days, you can delete hello App Service Domain, but cannot receive a refund.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="4e9a3-225">Nyissa meg az App Service-tartomány</span><span class="sxs-lookup"><span data-stu-id="4e9a3-225">Open App Service Domain</span></span>

<span data-ttu-id="4e9a3-226">Hello Azure-portálon, hello bal oldali menüben válassza ki **több szolgáltatások** > **App Service tartományok**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-226">In hello Azure portal, from hello left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="4e9a3-227">Válassza ki a hello tartomány tooyou kívánt toocancel vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-227">Select hello domain tooyou want toocancel or delete.</span></span> 

### <a name="delete-hostname-bindings"></a><span data-ttu-id="4e9a3-228">Törölje az állomásnévkötéseket</span><span class="sxs-lookup"><span data-stu-id="4e9a3-228">Delete hostname bindings</span></span>

<span data-ttu-id="4e9a3-229">Hello tartomány bal oldali menüben válasszon ki **állomásnévkötéseket**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-229">In hello domain's left menu, select **Hostname bindings**.</span></span> <span data-ttu-id="4e9a3-230">az itt felsorolt hello állomásnévkötéseket az összes Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-230">hello hostname bindings from all Azure services are listed here.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

<span data-ttu-id="4e9a3-231">Csak az összes állomásnévkötéseket törlése hello szolgáltatás alkalmazástartományban nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-231">You cannot delete hello App Service Domain until all hostname bindings are deleted.</span></span>

<span data-ttu-id="4e9a3-232">Minden egyes állomásnév kötés törlése kiválasztásával **...**   >  **Törlése**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-232">Delete each hostname binding by selecting **...** > **Delete**.</span></span> <span data-ttu-id="4e9a3-233">Miután minden hello kötés törölte, válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-233">After all hello bindings are deleted, select **Save**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a><span data-ttu-id="4e9a3-234">Mégse vagy törlése</span><span class="sxs-lookup"><span data-stu-id="4e9a3-234">Cancel or delete</span></span>

<span data-ttu-id="4e9a3-235">Hello tartomány bal oldali menüben válasszon ki **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-235">In hello domain's left menu, select **Overview**.</span></span> 

<span data-ttu-id="4e9a3-236">Ha hello megszakítási idő vásárolt hello tartományban nem telt meg, válassza ki a **szakítsa meg a beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-236">If hello cancellation period on hello purchased domain has not elapsed, select **Cancel purchase**.</span></span> <span data-ttu-id="4e9a3-237">Ellenkező esetben látni egy **törlése** gombra kattint, helyette.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-237">Otherwise, you see a **Delete** button instead.</span></span> <span data-ttu-id="4e9a3-238">visszatérítés, jelölje be nélkül toodelete hello tartomány **törlése**.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-238">toodelete hello domain without a refund, select **Delete**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

<span data-ttu-id="4e9a3-239">Válassza ki **OK** tooconfirm hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-239">Select **OK** tooconfirm hello operation.</span></span> <span data-ttu-id="4e9a3-240">Ha nem szeretné tooproceed, kattintson bárhová kívül hello megerősítő párbeszédpanele.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-240">If you don't want tooproceed, click anywhere outside of hello confirmation dialog.</span></span>

<span data-ttu-id="4e9a3-241">Hello művelet befejezése után hello tartomány-e az előfizetésből kiadott és bárki toopurchase újra.</span><span class="sxs-lookup"><span data-stu-id="4e9a3-241">After hello operation is complete, hello domain is released from your subscription and available for anyone toopurchase again.</span></span> 
