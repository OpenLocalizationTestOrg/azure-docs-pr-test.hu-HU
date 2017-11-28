---
title: "Egy meglévő egyéni DNS-névvel hozzárendelése az Azure Web Apps |} Microsoft Docs"
description: "Útmutató egy webalkalmazást, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazás hozzáadása egy meglévő egyéni DNS-tartománynevet (személyes tartománnyal)."
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
ms.openlocfilehash: 973cda462e8d258cc848e1036891c7f8af043102
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a><span data-ttu-id="d1bdf-103">Egy meglévő egyéni DNS-névvel hozzárendelése az Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="d1bdf-103">Map an existing custom DNS name to Azure Web Apps</span></span>

<span data-ttu-id="d1bdf-104">Az [Azure Web Apps](app-service-web-overview.md) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-104">[Azure Web Apps](app-service-web-overview.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="d1bdf-105">Az oktatóanyag bemutatja, hogyan képezheti Azure Web Apps egy meglévő egyéni DNS-névvel.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-105">This tutorial shows you how to map an existing custom DNS name to Azure Web Apps.</span></span>

![Az Azure alkalmazásba portálnavigációjával](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

<span data-ttu-id="d1bdf-107">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="d1bdf-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d1bdf-108">Altartomány leképezése (például `www.contoso.com`) egy olyan CNAME rekordot használatával</span><span class="sxs-lookup"><span data-stu-id="d1bdf-108">Map a subdomain (for example, `www.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="d1bdf-109">Egy legfelső szintű tartomány leképezése (például `contoso.com`) az A rekord használatával</span><span class="sxs-lookup"><span data-stu-id="d1bdf-109">Map a root domain (for example, `contoso.com`) by using an A record</span></span>
> * <span data-ttu-id="d1bdf-110">Rendelni egy helyettesítő tartományt (például `*.contoso.com`) egy olyan CNAME rekordot használatával</span><span class="sxs-lookup"><span data-stu-id="d1bdf-110">Map a wildcard domain (for example, `*.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="d1bdf-111">Tartomány leképezése parancsfájlokkal automatizálhatja</span><span class="sxs-lookup"><span data-stu-id="d1bdf-111">Automate domain mapping with scripts</span></span>

<span data-ttu-id="d1bdf-112">Választhat egy **CNAME rekord** vagy egy **rekord** App Service egy egyéni DNS-név hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-112">You can use either a **CNAME record** or an **A record** to map a custom DNS name to App Service.</span></span> 

> [!NOTE]
> <span data-ttu-id="d1bdf-113">Azt javasoljuk, hogy használ egy olyan CNAME REKORDOT a legfelső szintű tartomány kivételével az összes egyéni DNS-nevek (például `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-113">We recommend that you use a CNAME for all custom DNS names except a root domain (for example, `contoso.com`).</span></span>

<span data-ttu-id="d1bdf-114">Az App Service egy élő hely és a DNS-tartománynév áttelepítéséhez lásd: [egy aktív DNS-név áttelepítése az Azure App Service](app-service-custom-domain-name-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-114">To migrate a live site and its DNS domain name to App Service, see [Migrate an active DNS name to Azure App Service](app-service-custom-domain-name-migrate.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1bdf-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d1bdf-115">Prerequisites</span></span>

<span data-ttu-id="d1bdf-116">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="d1bdf-116">To complete this tutorial:</span></span>

* <span data-ttu-id="d1bdf-117">[App Service-alkalmazás létrehozása](/azure/app-service/), vagy egy másik az oktatóanyaghoz létrehozott alkalmazást használni.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-117">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>
* <span data-ttu-id="d1bdf-118">Vásároljon egy tartomány nevét, és ellenőrizze, elérhető lesz a tartomány-szolgáltató (például a GoDaddy) a DNS-beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-118">Purchase a domain name and make sure you have access to the DNS registry for your domain provider (such as GoDaddy).</span></span>

  <span data-ttu-id="d1bdf-119">Ahhoz például, hogy vegye fel a DNS-bejegyzéseket `contoso.com` és `www.contoso.com`, be kell tudnia a DNS-beállításainak konfigurálásához a `contoso.com` gyökértartomány.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-119">For example, to add DNS entries for `contoso.com` and `www.contoso.com`, you must be able to configure the DNS settings for the `contoso.com` root domain.</span></span>

  > [!NOTE]
  > <span data-ttu-id="d1bdf-120">Ha még nem rendelkezik egy meglévő tartomány nevét, érdemes lehet [egy tartományhoz az Azure portál használatával megvásárlásáról](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-120">If you don't have an existing domain name, consider [purchasing a domain using the Azure portal](custom-dns-web-site-buydomains-web-app.md).</span></span> 

## <a name="prepare-the-app"></a><span data-ttu-id="d1bdf-121">Az alkalmazás előkészítése</span><span class="sxs-lookup"><span data-stu-id="d1bdf-121">Prepare the app</span></span>

<span data-ttu-id="d1bdf-122">Egy egyéni DNS-név hozzárendelése egy webes alkalmazás, a webes alkalmazás [App Service-csomag](https://azure.microsoft.com/pricing/details/app-service/) fizetős rétegben kell lennie (**megosztott**, **alapvető**, **szabványos**, vagy  **Prémium szintű**).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-122">To map a custom DNS name to a web app, the web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="d1bdf-123">Ezt a lépést akkor győződjön meg arról, hogy az App Service alkalmazás van a támogatott az IP-címek.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-123">In this step, you make sure that the App Service app is in the supported pricing tier.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="d1bdf-124">Bejelentkezés az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="d1bdf-124">Sign in to Azure</span></span>

<span data-ttu-id="d1bdf-125">Nyissa meg a [Azure-portálon](https://portal.azure.com) és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-125">Open the [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-to-the-app-in-the-azure-portal"></a><span data-ttu-id="d1bdf-126">Keresse meg az alkalmazás az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d1bdf-126">Navigate to the app in the Azure portal</span></span>

<span data-ttu-id="d1bdf-127">A bal oldali menüben válassza ki a **alkalmazásszolgáltatások**, majd válassza ki az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-127">From the left menu, select **App Services**, and then select the name of the app.</span></span>

![Az Azure alkalmazásba portálnavigációjával](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="d1bdf-129">Az App Service alkalmazás a felügyeleti oldal akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-129">You see the management page of the App Service app.</span></span>  

<a name="checkpricing"></a>

### <a name="check-the-pricing-tier"></a><span data-ttu-id="d1bdf-130">Ellenőrizze az árképzési szint</span><span class="sxs-lookup"><span data-stu-id="d1bdf-130">Check the pricing tier</span></span>

<span data-ttu-id="d1bdf-131">Az alkalmazás oldal bal oldali navigációs, görgessen a **beállítások** válassza ki azt **vertikális felskálázás (App Service-csomag)**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-131">In the left navigation of the app page, scroll to the **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Méretezett menü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="d1bdf-133">Az alkalmazás jelenlegi rétegtől kék szegély ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-133">The app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="d1bdf-134">Győződjön meg arról, hogy az alkalmazás nem szerepel a **szabad** réteg.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-134">Check to make sure that the app is not in the **Free** tier.</span></span> <span data-ttu-id="d1bdf-135">Nem támogatja az egyéni DNS a **szabad** réteg.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-135">Custom DNS is not supported in the **Free** tier.</span></span> 

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="d1bdf-137">Ha az App Service-csomag **szabad**, zárja be a **válasszon tarifacsomagot** lapon, és folytassa a [képezi le egy olyan CNAME rekordot](#cname).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-137">If the App Service plan is not **Free**, close the **Choose your pricing tier** page and skip to [Map a CNAME record](#cname).</span></span>

<a name="scaleup"></a>

### <a name="scale-up-the-app-service-plan"></a><span data-ttu-id="d1bdf-138">Vertikális felskálázás az App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="d1bdf-138">Scale up the App Service plan</span></span>

<span data-ttu-id="d1bdf-139">Válassza ki valamelyik nem szabad rétegek (**megosztott**, **alapvető**, **szabványos**, vagy **prémium**).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-139">Select any of the non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="d1bdf-140">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-140">Click **Select**.</span></span>

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="d1bdf-142">Amikor megjelenik a következő értesítés, a méretezési művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-142">When you see the following notification, the scale operation is complete.</span></span>

![Skálázási művelet megerősítése](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a><span data-ttu-id="d1bdf-144">A CNAME rekord leképezése</span><span class="sxs-lookup"><span data-stu-id="d1bdf-144">Map a CNAME record</span></span>

<span data-ttu-id="d1bdf-145">Az oktatóanyag példában hozzá egy CNAME rekordot a `www` altartomány (például `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-145">In the tutorial example, you add a CNAME record for the `www` subdomain (for example, `www.contoso.com`).</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a><span data-ttu-id="d1bdf-146">A CNAME rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="d1bdf-146">Create the CNAME record</span></span>

<span data-ttu-id="d1bdf-147">Adjon hozzá egy CNAME rekordot altartomány hozzárendelése az alkalmazás alapértelmezett állomásnév (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-147">Add a CNAME record to map a subdomain to the app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="d1bdf-148">Az a `www.contoso.com` tartomány például egy CNAME rekordot, amely rendeli hozzá `www` való `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-148">For the `www.contoso.com` domain example, add a CNAME record that maps the name `www` to `<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="d1bdf-149">Miután hozzáadta a CNAME REKORDOT, a DNS-rekordok lapon néz ki a következő példa:</span><span class="sxs-lookup"><span data-stu-id="d1bdf-149">After you add the CNAME, the DNS records page looks like the following example:</span></span>

![Az Azure alkalmazásba portálnavigációjával](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a><span data-ttu-id="d1bdf-151">Az Azure-ban a CNAME rekord hozzárendelésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d1bdf-151">Enable the CNAME record mapping in Azure</span></span>

<span data-ttu-id="d1bdf-152">Az alkalmazás oldal az Azure-portálon a bal oldali navigációs, válassza ki a **egyéni tartományok**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-152">In the left navigation of the app page in the Azure portal, select **Custom domains**.</span></span> 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="d1bdf-154">Az a **egyéni tartományok** lap az alkalmazáshoz, adja hozzá a teljes egyéni DNS-nevet (`www.contoso.com`) listájához.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-154">In the **Custom domains** page of the app, add the fully qualified custom DNS name (`www.contoso.com`) to the list.</span></span>

<span data-ttu-id="d1bdf-155">Válassza ki a  **+**  melletti ikon **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-155">Select the **+** icon next to **Add hostname**.</span></span>

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="d1bdf-157">Írja be a teljesen minősített tartománynevét, egy CNAME rekordot, például a hozzáadott `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-157">Type the fully qualified domain name that you added a CNAME record for, such as `www.contoso.com`.</span></span> 

<span data-ttu-id="d1bdf-158">Válassza ki **érvényesítése**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-158">Select **Validate**.</span></span>

<span data-ttu-id="d1bdf-159">A **állomásnév hozzáadása** gomb aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-159">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="d1bdf-160">Győződjön meg arról, hogy **állomásnév rekordtípus** értéke **CNAME (www.example.com vagy bármely altartomány)**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-160">Make sure that **Hostname record type** is set to **CNAME (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="d1bdf-161">Válassza ki **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-161">Select **Add hostname**.</span></span>

![DNS-név hozzáadása az alkalmazáshoz](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="d1bdf-163">Ez eltarthat egy ideig, megjelennek az alkalmazás új gazdagép **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-163">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="d1bdf-164">Próbálja meg frissíteni a böngészőt az adatok frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-164">Try refreshing the browser to update the data.</span></span>

![CNAME rekord hozzáadva](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="d1bdf-166">Ha nem talált meg egy lépést, vagy elgépelte valahol korábban, egy ellenőrzési hiba a lap alján látható.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-166">If you missed a step or made a typo somewhere earlier, you see a verification error at the bottom of the page.</span></span>

![Ellenőrzési hiba](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a><span data-ttu-id="d1bdf-168">Az A rekord leképezése</span><span class="sxs-lookup"><span data-stu-id="d1bdf-168">Map an A record</span></span>

<span data-ttu-id="d1bdf-169">Az oktatóanyag példában a legfelső szintű tartomány az A rekord hozzáadása (például `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-169">In the tutorial example, you add an A record for the root domain (for example, `contoso.com`).</span></span> 

<a name="info"></a>

### <a name="copy-the-apps-ip-address"></a><span data-ttu-id="d1bdf-170">Az alkalmazás IP-cím másolása</span><span class="sxs-lookup"><span data-stu-id="d1bdf-170">Copy the app's IP address</span></span>

<span data-ttu-id="d1bdf-171">Képezi rekorddal, az alkalmazás külső IP-cím szükséges.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-171">To map an A record, you need the app's external IP address.</span></span> <span data-ttu-id="d1bdf-172">Az IP-címet az alkalmazásban található **egyéni tartományok** oldal az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-172">You can find this IP address in the app's **Custom domains** page in the Azure portal.</span></span>

<span data-ttu-id="d1bdf-173">Az alkalmazás oldal az Azure-portálon a bal oldali navigációs, válassza ki a **egyéni tartományok**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-173">In the left navigation of the app page in the Azure portal, select **Custom domains**.</span></span> 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="d1bdf-175">Az a **egyéni tartományok** lapon, az alkalmazás IP-cím másolásához.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-175">In the **Custom domains** page, copy the app's IP address.</span></span>

![Az Azure alkalmazásba portálnavigációjával](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-a-record"></a><span data-ttu-id="d1bdf-177">Az A rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="d1bdf-177">Create the A record</span></span>

<span data-ttu-id="d1bdf-178">Az A rekord leképezése egy alkalmazást, az App Service szükséges **két** DNS-rekordokat:</span><span class="sxs-lookup"><span data-stu-id="d1bdf-178">To map an A record to an app, App Service requires **two** DNS records:</span></span>

- <span data-ttu-id="d1bdf-179">Egy **A** rekord az alkalmazás IP-cím hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-179">An **A** record to map to the app's IP address.</span></span>
- <span data-ttu-id="d1bdf-180">A **TXT** rekord leképezése az alkalmazás alapértelmezett állomásnév `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-180">A **TXT** record to map to the app's default hostname `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="d1bdf-181">App Service a rekord csak időben konfigurációs, győződjön meg arról, hogy Ön a tulajdonosa az egyéni tartomány használja.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-181">App Service uses this record only at configuration time, to verify that you own the custom domain.</span></span> <span data-ttu-id="d1bdf-182">Miután az egyéni tartomány érvényesítve, és az App Service-ben konfigurált, törölheti a TXT-rekord.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-182">After your custom domain is validated and configured in App Service, you can delete this TXT record.</span></span> 

<span data-ttu-id="d1bdf-183">Az a `contoso.com` tartomány például az alábbi táblázat szerint és TXT-rekordok létrehozása (`@` a gyökértartomány általában jelöli).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-183">For the `contoso.com` domain example, create the A and TXT records according to the following table (`@` typically represents the root domain).</span></span> 

| <span data-ttu-id="d1bdf-184">Bejegyzéstípus</span><span class="sxs-lookup"><span data-stu-id="d1bdf-184">Record type</span></span> | <span data-ttu-id="d1bdf-185">Gazdagép</span><span class="sxs-lookup"><span data-stu-id="d1bdf-185">Host</span></span> | <span data-ttu-id="d1bdf-186">Érték</span><span class="sxs-lookup"><span data-stu-id="d1bdf-186">Value</span></span> |
| - | - | - |
| <span data-ttu-id="d1bdf-187">A</span><span class="sxs-lookup"><span data-stu-id="d1bdf-187">A</span></span> | `@` | <span data-ttu-id="d1bdf-188">IP-cím a [másolja az alkalmazás IP-cím](#info)</span><span class="sxs-lookup"><span data-stu-id="d1bdf-188">IP address from [Copy the app's IP address](#info)</span></span> |
| <span data-ttu-id="d1bdf-189">TXT</span><span class="sxs-lookup"><span data-stu-id="d1bdf-189">TXT</span></span> | `@` | `<app_name>.azurewebsites.net` |

<span data-ttu-id="d1bdf-190">A rekordok kerülnek, ha a DNS-rekordok lapon néz ki a következő példa:</span><span class="sxs-lookup"><span data-stu-id="d1bdf-190">When the records are added, the DNS records page looks like the following example:</span></span>

![DNS-rekordok lap](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-the-a-record-mapping-in-the-app"></a><span data-ttu-id="d1bdf-192">Az alkalmazásban, az A rekord hozzárendelésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d1bdf-192">Enable the A record mapping in the app</span></span>

<span data-ttu-id="d1bdf-193">Vissza az alkalmazásban lévő **egyéni tartományok** oldalra az Azure portálon, adja hozzá a teljesen minősített egyéni DNS-nevét (például `contoso.com`) listájához.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-193">Back in the app's **Custom domains** page in the Azure portal, add the fully qualified custom DNS name (for example, `contoso.com`) to the list.</span></span>

<span data-ttu-id="d1bdf-194">Válassza ki a  **+**  melletti ikon **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-194">Select the **+** icon next to **Add hostname**.</span></span>

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

<span data-ttu-id="d1bdf-196">Írja be a teljesen minősített tartománynevét, az A rekordot, például a konfigurált `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-196">Type the fully qualified domain name that you configured the A record for, such as `contoso.com`.</span></span>

<span data-ttu-id="d1bdf-197">Válassza ki **érvényesítése**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-197">Select **Validate**.</span></span>

<span data-ttu-id="d1bdf-198">A **állomásnév hozzáadása** gomb aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-198">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="d1bdf-199">Győződjön meg arról, hogy **állomásnév rekordtípus** értéke **egy olyan rekordot (example.com)**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-199">Make sure that **Hostname record type** is set to **A record (example.com)**.</span></span>

<span data-ttu-id="d1bdf-200">Válassza ki **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-200">Select **Add hostname**.</span></span>

![DNS-név hozzáadása az alkalmazáshoz](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

<span data-ttu-id="d1bdf-202">Ez eltarthat egy ideig, megjelennek az alkalmazás új gazdagép **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-202">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="d1bdf-203">Próbálja meg frissíteni a böngészőt az adatok frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-203">Try refreshing the browser to update the data.</span></span>

![Egy hozzáadott bejegyzés](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

<span data-ttu-id="d1bdf-205">Ha nem talált meg egy lépést, vagy elgépelte valahol korábban, egy ellenőrzési hiba a lap alján látható.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-205">If you missed a step or made a typo somewhere earlier, you see a verification error at the bottom of the page.</span></span>

![Ellenőrzési hiba](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a><span data-ttu-id="d1bdf-207">A helyettesítő tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="d1bdf-207">Map a wildcard domain</span></span>

<span data-ttu-id="d1bdf-208">Az oktatóanyag példában leképez egy [helyettesítő DNS-név](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (például `*.contoso.com`) az App Service alkalmazásba adja hozzá egy CNAME rekordot.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-208">In the tutorial example, you map a [wildcard DNS name](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (for example, `*.contoso.com`) to the App Service app by adding a CNAME record.</span></span> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a><span data-ttu-id="d1bdf-209">A CNAME rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="d1bdf-209">Create the CNAME record</span></span>

<span data-ttu-id="d1bdf-210">Adjon hozzá egy CNAME rekordot egy helyettesítő karakteres nevet hozzárendelése az alkalmazás alapértelmezett állomásnév (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-210">Add a CNAME record to map a wildcard name to the app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="d1bdf-211">Az a `*.contoso.com` tartomány például a CNAME rekord felelteti meg a név `*` való `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-211">For the `*.contoso.com` domain example, the CNAME record will map the name `*` to `<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="d1bdf-212">Amikor a CNAME ad hozzá, a DNS-rekordok lapon néz ki a következő példa:</span><span class="sxs-lookup"><span data-stu-id="d1bdf-212">When the CNAME is added, the DNS records page looks like the following example:</span></span>

![Az Azure alkalmazásba portálnavigációjával](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-the-cname-record-mapping-in-the-app"></a><span data-ttu-id="d1bdf-214">Az alkalmazásban a CNAME rekord hozzárendelésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d1bdf-214">Enable the CNAME record mapping in the app</span></span>

<span data-ttu-id="d1bdf-215">Mostantól hozzáadhatja azok bármely altartomány, amely megfelel a helyettesítő karakteres nevének az alkalmazáshoz (például `sub1.contoso.com` és `sub2.contoso.com` megfelelő `*.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-215">You can now add any subdomain that matches the wildcard name to the app (for example, `sub1.contoso.com` and `sub2.contoso.com` match `*.contoso.com`).</span></span> 

<span data-ttu-id="d1bdf-216">Az alkalmazás oldal az Azure-portálon a bal oldali navigációs, válassza ki a **egyéni tartományok**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-216">In the left navigation of the app page in the Azure portal, select **Custom domains**.</span></span> 

![Az egyéni tartomány menü](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="d1bdf-218">Válassza ki a  **+**  melletti ikon **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-218">Select the **+** icon next to **Add hostname**.</span></span>

![Adja hozzá a gazdagép neve](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="d1bdf-220">Írjon be egy teljesen minősített tartománynevet, amely megfelel a helyettesítő tartomány (például `sub1.contoso.com`), majd válassza ki **ellenőrzése**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-220">Type a fully qualified domain name that matches the wildcard domain (for example, `sub1.contoso.com`), and then select **Validate**.</span></span>

<span data-ttu-id="d1bdf-221">A **állomásnév hozzáadása** gomb aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-221">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="d1bdf-222">Győződjön meg arról, hogy **állomásnév rekordtípus** értéke **CNAME rekord (www.example.com vagy bármely altartomány)**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-222">Make sure that **Hostname record type** is set to **CNAME record (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="d1bdf-223">Válassza ki **állomásnév hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-223">Select **Add hostname**.</span></span>

![DNS-név hozzáadása az alkalmazáshoz](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

<span data-ttu-id="d1bdf-225">Ez eltarthat egy ideig, megjelennek az alkalmazás új gazdagép **egyéni tartományok** lap.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-225">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="d1bdf-226">Próbálja meg frissíteni a böngészőt az adatok frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-226">Try refreshing the browser to update the data.</span></span>

<span data-ttu-id="d1bdf-227">Válassza ki a  **+**  ikonra újra egy másik állomásnév, amely megfelel a helyettesítő tartomány hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-227">Select the **+** icon again to add another hostname that matches the wildcard domain.</span></span> <span data-ttu-id="d1bdf-228">Adja hozzá például `sub2.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-228">For example, add `sub2.contoso.com`.</span></span>

![CNAME rekord hozzáadva](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a><span data-ttu-id="d1bdf-230">A böngészőben tesztelése</span><span class="sxs-lookup"><span data-stu-id="d1bdf-230">Test in browser</span></span>

<span data-ttu-id="d1bdf-231">Keresse meg a korábban konfigurált DNS-nevek (például `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, és `sub2.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-231">Browse to the DNS name(s) that you configured earlier (for example, `contoso.com`,  `www.contoso.com`, `sub1.contoso.com`, and `sub2.contoso.com`).</span></span>

![Az Azure alkalmazásba portálnavigációjával](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a><span data-ttu-id="d1bdf-233">Parancsfájlok automatizálásához</span><span class="sxs-lookup"><span data-stu-id="d1bdf-233">Automate with scripts</span></span>

<span data-ttu-id="d1bdf-234">Automatizálható a parancsfájlok, az egyéni tartományok felügyeleti használatával a [Azure CLI](/cli/azure/install-azure-cli) vagy [Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-234">You can automate management of custom domains with scripts, using the [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="azure-cli"></a><span data-ttu-id="d1bdf-235">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d1bdf-235">Azure CLI</span></span> 

<span data-ttu-id="d1bdf-236">A következő parancsot egy konfigurált egyéni DNS-nevet ad hozzá egy App Service-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-236">The following command adds a configured custom DNS name to an App Service app.</span></span> 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

<span data-ttu-id="d1bdf-237">További információkért lásd: [egy egyéni tartomány leképezése a webes alkalmazás](scripts/app-service-cli-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-237">For more information, see [Map a custom domain to a web app](scripts/app-service-cli-configure-custom-domain.md).</span></span> 

### <a name="azure-powershell"></a><span data-ttu-id="d1bdf-238">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1bdf-238">Azure PowerShell</span></span> 

<span data-ttu-id="d1bdf-239">A következő parancsot egy konfigurált egyéni DNS-nevet ad hozzá egy App Service-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-239">The following command adds a configured custom DNS name to an App Service app.</span></span> 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

<span data-ttu-id="d1bdf-240">További információkért lásd: [rendelje hozzá az egyéni tartománynév a webes alkalmazás](scripts/app-service-powershell-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="d1bdf-240">For more information, see [Assign a custom domain to a web app](scripts/app-service-powershell-configure-custom-domain.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1bdf-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1bdf-241">Next steps</span></span>

<span data-ttu-id="d1bdf-242">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="d1bdf-242">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d1bdf-243">A CNAME rekord használatával altartomány térkép</span><span class="sxs-lookup"><span data-stu-id="d1bdf-243">Map a subdomain by using a CNAME record</span></span>
> * <span data-ttu-id="d1bdf-244">Az A rekord használatával a legfelső szintű tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="d1bdf-244">Map a root domain by using an A record</span></span>
> * <span data-ttu-id="d1bdf-245">Térkép egy helyettesítő tartomány CNAME rekord használatával</span><span class="sxs-lookup"><span data-stu-id="d1bdf-245">Map a wildcard domain by using a CNAME record</span></span>
> * <span data-ttu-id="d1bdf-246">Tartomány leképezése parancsfájlokkal automatizálhatja</span><span class="sxs-lookup"><span data-stu-id="d1bdf-246">Automate domain mapping with scripts</span></span>

<span data-ttu-id="d1bdf-247">Továbblépés a következő oktatóanyag áttekintésével megismerheti, hogyan lehet kötést létrehozni egy egyéni SSL-tanúsítványt egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d1bdf-247">Advance to the next tutorial to learn how to bind a custom SSL certificate to a web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d1bdf-248">Egy meglévő egyéni SSL-tanúsítvány kötését az Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="d1bdf-248">Bind an existing custom SSL certificate to Azure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
