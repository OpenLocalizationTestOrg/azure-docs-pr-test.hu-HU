---
title: "Egy meglévő egyéni SSL-tanúsítvány kötését az Azure Web Apps |} Microsoft Docs"
description: "Ismerkedjen meg az egyéni SSL-tanúsítványt kötni a webalkalmazást, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazás."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 15c31ae5451a31dff2df08047ee43e75edacc127
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a><span data-ttu-id="185b5-103">Egy meglévő egyéni SSL-tanúsítvány kötését az Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="185b5-103">Bind an existing custom SSL certificate to Azure Web Apps</span></span>

<span data-ttu-id="185b5-104">Az Azure Web Apps jól skálázható, önálló javítási a webhelyszolgáltató biztosít.</span><span class="sxs-lookup"><span data-stu-id="185b5-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="185b5-105">Az oktatóanyag bemutatja, hogyan lehet kötést létrehozni egy egyéni SSL-tanúsítványt a megbízható hitelesítésszolgáltatótól származó megvásárolt [Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="185b5-105">This tutorial shows you how to bind a custom SSL certificate that you purchased from a trusted certificate authority to [Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="185b5-106">Amikor végzett, akkor képes lesz a webalkalmazás a HTTPS-végpont az egyéni DNS-tartomány eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="185b5-106">When you're finished, you'll be able to access your web app at the HTTPS endpoint of your custom DNS domain.</span></span>

![Egyéni SSL-tanúsítvánnyal webalkalmazás](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

<span data-ttu-id="185b5-108">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="185b5-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="185b5-109">Frissítse az alkalmazás tarifacsomag kiválasztása</span><span class="sxs-lookup"><span data-stu-id="185b5-109">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="185b5-110">Az egyéni SSL-tanúsítvány kötését az App Service</span><span class="sxs-lookup"><span data-stu-id="185b5-110">Bind your custom SSL certificate to App Service</span></span>
> * <span data-ttu-id="185b5-111">Az alkalmazás HTTPS kényszerítése</span><span class="sxs-lookup"><span data-stu-id="185b5-111">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="185b5-112">SSL-tanúsítvány kötés parancsfájlokkal automatizálhatja</span><span class="sxs-lookup"><span data-stu-id="185b5-112">Automate SSL certificate binding with scripts</span></span>

> [!NOTE]
> <span data-ttu-id="185b5-113">Ha egy egyéni SSL-tanúsítvány beszerzése van szüksége, közvetlenül lekérni egy Azure-portálon, és kösse a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="185b5-113">If you need to get a custom SSL certificate, you can get one in the Azure portal directly and bind it to your web app.</span></span> <span data-ttu-id="185b5-114">Kövesse a [App Service-tanúsítványokkal oktatóanyag](web-sites-purchase-ssl-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="185b5-114">Follow the [App Service Certificates tutorial](web-sites-purchase-ssl-web-site.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="185b5-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="185b5-115">Prerequisites</span></span>

<span data-ttu-id="185b5-116">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="185b5-116">To complete this tutorial:</span></span>

- [<span data-ttu-id="185b5-117">Az App Service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="185b5-117">Create an App Service app</span></span>](/azure/app-service/)
- [<span data-ttu-id="185b5-118">Egy egyéni DNS-nevet a webalkalmazás hozzárendelését</span><span class="sxs-lookup"><span data-stu-id="185b5-118">Map a custom DNS name to your web app</span></span>](app-service-web-tutorial-custom-domain.md)
- <span data-ttu-id="185b5-119">Szerezzen be egy megbízható hitelesítésszolgáltatótól származó SSL-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="185b5-119">Acquire an SSL certificate from a trusted certificate authority</span></span>

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a><span data-ttu-id="185b5-120">Az SSL-tanúsítványra vonatkozó követelményekről</span><span class="sxs-lookup"><span data-stu-id="185b5-120">Requirements for your SSL certificate</span></span>

<span data-ttu-id="185b5-121">Az App Service tanúsítványt használ, a tanúsítványt a következő követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="185b5-121">To use a certificate in App Service, the certificate must meet all the following requirements:</span></span>

* <span data-ttu-id="185b5-122">Megbízható hitelesítésszolgáltató által aláírt</span><span class="sxs-lookup"><span data-stu-id="185b5-122">Signed by a trusted certificate authority</span></span>
* <span data-ttu-id="185b5-123">Jelszóval védett PFX-fájlba exportált</span><span class="sxs-lookup"><span data-stu-id="185b5-123">Exported as a password-protected PFX file</span></span>
* <span data-ttu-id="185b5-124">Titkos kulcs legalább 2048 bit hosszúságú tartalmazza</span><span class="sxs-lookup"><span data-stu-id="185b5-124">Contains private key at least 2048 bits long</span></span>
* <span data-ttu-id="185b5-125">A tanúsítványlánc összes köztes tanúsítvány tartalmazza</span><span class="sxs-lookup"><span data-stu-id="185b5-125">Contains all intermediate certificates in the certificate chain</span></span>

> [!NOTE]
> <span data-ttu-id="185b5-126">**Elliptikus görbe alapú titkosítást (ECC) tanúsítványok** App Service képes együttműködni, de ez a cikk nem vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="185b5-126">**Elliptic Curve Cryptography (ECC) certificates** can work with App Service but are not covered by this article.</span></span> <span data-ttu-id="185b5-127">A hitelesítésszolgáltató használni a pontos lépések az ECC-tanúsítványokkal létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="185b5-127">Work with your certificate authority on the exact steps to create ECC certificates.</span></span>

## <a name="prepare-your-web-app"></a><span data-ttu-id="185b5-128">A webes alkalmazás előkészítése</span><span class="sxs-lookup"><span data-stu-id="185b5-128">Prepare your web app</span></span>

<span data-ttu-id="185b5-129">Egy egyéni SSL-tanúsítványt kötni a webalkalmazás a [App Service-csomag](https://azure.microsoft.com/pricing/details/app-service/) szerepelnie kell a **alapvető**, **szabványos**, vagy **prémium** réteg.</span><span class="sxs-lookup"><span data-stu-id="185b5-129">To bind a custom SSL certificate to your web app, your [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be in the **Basic**, **Standard**, or **Premium** tier.</span></span> <span data-ttu-id="185b5-130">Ezt a lépést akkor győződjön meg arról, hogy a webalkalmazás van a támogatott az IP-címek.</span><span class="sxs-lookup"><span data-stu-id="185b5-130">In this step, you make sure that your web app is in the supported pricing tier.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="185b5-131">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="185b5-131">Log in to Azure</span></span>

<span data-ttu-id="185b5-132">Nyissa meg az [Azure portált](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="185b5-132">Open the [Azure portal](https://portal.azure.com).</span></span>

### <a name="navigate-to-your-web-app"></a><span data-ttu-id="185b5-133">Keresse meg a webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="185b5-133">Navigate to your web app</span></span>

<span data-ttu-id="185b5-134">Kattintson a bal oldali menü **alkalmazásszolgáltatások**, majd kattintson a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="185b5-134">From the left menu, click **App Services**, and then click the name of your web app.</span></span>

![Webalkalmazás kiválasztása](./media/app-service-web-tutorial-custom-ssl/select-app.png)

<span data-ttu-id="185b5-136">A webalkalmazás kezelése lapon rendelkezik ki.</span><span class="sxs-lookup"><span data-stu-id="185b5-136">You have landed in the management page of your web app.</span></span>  

### <a name="check-the-pricing-tier"></a><span data-ttu-id="185b5-137">Ellenőrizze az árképzési szint</span><span class="sxs-lookup"><span data-stu-id="185b5-137">Check the pricing tier</span></span>

<span data-ttu-id="185b5-138">A bal oldali navigációs sáv a weblap app, görgessen a **beállítások** válassza ki azt **vertikális felskálázás (App Service-csomag)**.</span><span class="sxs-lookup"><span data-stu-id="185b5-138">In the left-hand navigation of your web app page, scroll to the **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Méretezett menü](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

<span data-ttu-id="185b5-140">Győződjön meg arról, hogy a webalkalmazás nem szerepel a **szabad** vagy **megosztott** réteg.</span><span class="sxs-lookup"><span data-stu-id="185b5-140">Check to make sure that your web app is not in the **Free** or **Shared** tier.</span></span> <span data-ttu-id="185b5-141">A webalkalmazás jelenlegi rétegtől ki van jelölve, egy kékkel által.</span><span class="sxs-lookup"><span data-stu-id="185b5-141">Your web app's current tier is highlighted by a dark blue box.</span></span>

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

<span data-ttu-id="185b5-143">Egyéni SSL nem támogatja a **szabad** vagy **megosztott** réteg.</span><span class="sxs-lookup"><span data-stu-id="185b5-143">Custom SSL is not supported in the **Free** or **Shared** tier.</span></span> <span data-ttu-id="185b5-144">Ha vertikális felskálázás van szüksége, kövesse a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="185b5-144">If you need to scale up, follow the steps in the next section.</span></span> <span data-ttu-id="185b5-145">Ellenkező esetben zárja be a **válasszon tarifacsomagot** lapon, és folytassa a [töltse fel, és az SSL-tanúsítvány kötését](#upload).</span><span class="sxs-lookup"><span data-stu-id="185b5-145">Otherwise, close the **Choose your pricing tier** page and skip to [Upload and bind your SSL certificate](#upload).</span></span>

### <a name="scale-up-your-app-service-plan"></a><span data-ttu-id="185b5-146">Vertikális felskálázás App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="185b5-146">Scale up your App Service plan</span></span>

<span data-ttu-id="185b5-147">Válasszon egyet a a **alapvető**, **szabványos**, vagy **prémium** rétegek.</span><span class="sxs-lookup"><span data-stu-id="185b5-147">Select one of the **Basic**, **Standard**, or **Premium** tiers.</span></span>

<span data-ttu-id="185b5-148">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="185b5-148">Click **Select**.</span></span>

![Tarifacsomag kiválasztása](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

<span data-ttu-id="185b5-150">Amikor megjelenik a következő értesítés, a méretezési művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="185b5-150">When you see the following notification, the scale operation is complete.</span></span>

![Vertikális felskálázás értesítés](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a><span data-ttu-id="185b5-152">Az SSL-tanúsítvány kötése</span><span class="sxs-lookup"><span data-stu-id="185b5-152">Bind your SSL certificate</span></span>

<span data-ttu-id="185b5-153">Készen áll az SSL-tanúsítvány feltöltése a webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="185b5-153">You are ready to upload your SSL certificate to your web app.</span></span>

### <a name="merge-intermediate-certificates"></a><span data-ttu-id="185b5-154">Köztes tanúsítványok egyesítése</span><span class="sxs-lookup"><span data-stu-id="185b5-154">Merge intermediate certificates</span></span>

<span data-ttu-id="185b5-155">Ha a hitelesítésszolgáltató lehetővé teszi több tanúsítvány a tanúsítványlánc, kell egyesíteni, a tanúsítványok sorrendben.</span><span class="sxs-lookup"><span data-stu-id="185b5-155">If your certificate authority gives you multiple certificates in the certificate chain, you need to merge the certificates in order.</span></span> 

<span data-ttu-id="185b5-156">Ehhez nyissa meg a minden tanúsítvány kapott egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="185b5-156">To do this, open each certificate you received in a text editor.</span></span> 

<span data-ttu-id="185b5-157">Az egyesített tanúsítvány, nevű fájl létrehozása _mergedcertificate.crt_.</span><span class="sxs-lookup"><span data-stu-id="185b5-157">Create a file for the merged certificate, called _mergedcertificate.crt_.</span></span> <span data-ttu-id="185b5-158">Egy szövegszerkesztőben másolja az egyes tanúsítványok tartalmat a fájlba.</span><span class="sxs-lookup"><span data-stu-id="185b5-158">In a text editor, copy the content of each certificate into this file.</span></span> <span data-ttu-id="185b5-159">A tanúsítványok sorrendje a következő sablon kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="185b5-159">The order of your certificates should look like the following template:</span></span>

```
-----BEGIN CERTIFICATE-----
<your Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a><span data-ttu-id="185b5-160">Tanúsítvány exportálása PFX</span><span class="sxs-lookup"><span data-stu-id="185b5-160">Export certificate to PFX</span></span>

<span data-ttu-id="185b5-161">Az egyesített SSL-tanúsítvány exportálása a titkos kulccsal, a tanúsítvány kérelmezéséhez során létrejött.</span><span class="sxs-lookup"><span data-stu-id="185b5-161">Export your merged SSL certificate with the private key that your certificate request was generated with.</span></span>

<span data-ttu-id="185b5-162">Ha a tanúsítvány kérelmezéséhez OpenSSL használatával jön létre, majd hozott létre a titkos kulcs fájlja.</span><span class="sxs-lookup"><span data-stu-id="185b5-162">If you generated your certificate request using OpenSSL, then you have created a private key file.</span></span> <span data-ttu-id="185b5-163">Exportálja a tanúsítványt PFX, futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="185b5-163">To export your certificate to PFX, run the following command.</span></span> <span data-ttu-id="185b5-164">Cserélje le a helyőrzőket  _&lt;titkos kulcsfájl >_ és  _&lt;egyesített-tanúsítványfájl >_.</span><span class="sxs-lookup"><span data-stu-id="185b5-164">Replace the placeholders _&lt;private-key-file>_ and _&lt;merged-certificate-file>_.</span></span>

```
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

<span data-ttu-id="185b5-165">Amikor a rendszer kéri, adja meg az exportálási jelszó.</span><span class="sxs-lookup"><span data-stu-id="185b5-165">When prompted, define an export password.</span></span> <span data-ttu-id="185b5-166">Ezt a jelszót fog használni, amikor az SSL-tanúsítvány feltöltése az App Service később.</span><span class="sxs-lookup"><span data-stu-id="185b5-166">You'll use this password when uploading your SSL certificate to App Service later.</span></span>

<span data-ttu-id="185b5-167">Ha az IIS vagy _Certreq.exe_ létrehozni a tanúsítványkérelmet, telepítse a tanúsítványt a helyi számítógépen, majd [exportálja a tanúsítványt PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span><span class="sxs-lookup"><span data-stu-id="185b5-167">If you used IIS or _Certreq.exe_ to generate your certificate request, install the certificate to your local machine, and then [export the certificate to PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span></span>

### <a name="upload-your-ssl-certificate"></a><span data-ttu-id="185b5-168">Az SSL-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="185b5-168">Upload your SSL certificate</span></span>

<span data-ttu-id="185b5-169">Az SSL-tanúsítvány feltöltése, kattintson a **SSL-tanúsítványok** a bal oldali navigációs webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="185b5-169">To upload your SSL certificate, click **SSL certificates** in the left navigation of your web app.</span></span>

<span data-ttu-id="185b5-170">Kattintson a **-tanúsítvány feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="185b5-170">Click **Upload Certificate**.</span></span>

<span data-ttu-id="185b5-171">A **PFX-tanúsítványfájlt**, válassza ki a PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="185b5-171">In **PFX Certificate File**, select your PFX file.</span></span> <span data-ttu-id="185b5-172">A **tanúsítványjelszavas**, írja be a PFX-fájl exportálása során létrehozott jelszót.</span><span class="sxs-lookup"><span data-stu-id="185b5-172">In **Certificate password**, type the password that you created when you exported the PFX file.</span></span>

<span data-ttu-id="185b5-173">Kattintson a **Feltöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="185b5-173">Click **Upload**.</span></span>

![Tanúsítvány feltöltése](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

<span data-ttu-id="185b5-175">App Service befejezése után a tanúsítvány feltöltése megjelenik a **SSL-tanúsítványok** lap.</span><span class="sxs-lookup"><span data-stu-id="185b5-175">When App Service finishes uploading your certificate, it appears in the **SSL certificates** page.</span></span>

![A tanúsítvány feltöltése](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a><span data-ttu-id="185b5-177">Az SSL-tanúsítvány kötése</span><span class="sxs-lookup"><span data-stu-id="185b5-177">Bind your SSL certificate</span></span>

<span data-ttu-id="185b5-178">Az a **SSL-kötések** kattintson **kötés hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="185b5-178">In the **SSL bindings** section, click **Add binding**.</span></span>

<span data-ttu-id="185b5-179">Az a **hozzá SSL-kötés** lapon, a legördülő lista, és válassza ki a tartomány nevét biztonságos, valamint a használni kívánt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="185b5-179">In the **Add SSL Binding** page, use the dropdowns to select the domain name to secure, and the certificate to use.</span></span>

> [!NOTE]
> <span data-ttu-id="185b5-180">Ha a tanúsítvány feltöltött, de nem jelenik meg a tartomány nevét a **állomásnév** legördülő menüből, próbálja meg frissíteni a böngésző lapot.</span><span class="sxs-lookup"><span data-stu-id="185b5-180">If you have uploaded your certificate but don't see the domain name(s) in the **Hostname** dropdown, try refreshing the browser page.</span></span>
>
>

<span data-ttu-id="185b5-181">A **SSL típus**, válassza ki, hogy használja-e  **[kiszolgálónév jelzése (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  vagy IP-alapú SSL.</span><span class="sxs-lookup"><span data-stu-id="185b5-181">In **SSL Type**, select whether to use **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP-based SSL.</span></span>

- <span data-ttu-id="185b5-182">**Az SNI-alapú SSL** -több SNI-alapú SSL-kötések adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="185b5-182">**SNI-based SSL** - Multiple SNI-based SSL bindings may be added.</span></span> <span data-ttu-id="185b5-183">Ez a beállítás lehetővé teszi, hogy a biztonságos több tartományt a azonos IP-címhez több SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="185b5-183">This option allows multiple SSL certificates to secure multiple domains on the same IP address.</span></span> <span data-ttu-id="185b5-184">(Beleértve az Internet Explorer, Chrome, Firefox és Opera) a legtöbb modern böngésző támogatja az SNI (átfogóbb böngésző információt, [kiszolgálónév jelzése](http://wikipedia.org/wiki/Server_Name_Indication)).</span><span class="sxs-lookup"><span data-stu-id="185b5-184">Most modern browsers (including Internet Explorer, Chrome, Firefox, and Opera) support SNI (find more comprehensive browser support information at [Server Name Indication](http://wikipedia.org/wiki/Server_Name_Indication)).</span></span>
- <span data-ttu-id="185b5-185">**IP-alapú SSL** -csak egy IP-alapú SSL-kötés adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="185b5-185">**IP-based SSL** - Only one IP-based SSL binding may be added.</span></span> <span data-ttu-id="185b5-186">Ez a beállítás lehetővé teszi, hogy a biztonságos egy dedikált nyilvános IP-cím csak egyetlen SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="185b5-186">This option allows only one SSL certificate to secure a dedicated public IP address.</span></span> <span data-ttu-id="185b5-187">Több tartomány biztonságos, biztosítania kell őket az összes SSL-tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="185b5-187">To secure multiple domains, you must secure them all using the same SSL certificate.</span></span> <span data-ttu-id="185b5-188">Ez a lehetőség a hagyományos SSL-kötés.</span><span class="sxs-lookup"><span data-stu-id="185b5-188">This is the traditional option for SSL binding.</span></span>

<span data-ttu-id="185b5-189">Kattintson a **kötés hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="185b5-189">Click **Add Binding**.</span></span>

![SSL-tanúsítvány kötésének létrehozása](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

<span data-ttu-id="185b5-191">App Service befejezése után a tanúsítvány feltöltése megjelenik a **SSL-kötések** szakaszok.</span><span class="sxs-lookup"><span data-stu-id="185b5-191">When App Service finishes uploading your certificate, it appears in the **SSL bindings** sections.</span></span>

![A webes alkalmazás kötött tanúsítvány](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a><span data-ttu-id="185b5-193">Adja meg újból egy rekordot az IP-SSL</span><span class="sxs-lookup"><span data-stu-id="185b5-193">Remap A record for IP SSL</span></span>

<span data-ttu-id="185b5-194">Ha IP-alapú SSL nem adja meg a web app alkalmazásban, folytassa a [teszt a HTTPS-t az egyéni tartomány](#test).</span><span class="sxs-lookup"><span data-stu-id="185b5-194">If you don't use IP-based SSL in your web app, skip to [Test HTTPS for your custom domain](#test).</span></span>

<span data-ttu-id="185b5-195">Alapértelmezés szerint a webalkalmazás egy megosztott nyilvános IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="185b5-195">By default, your web app uses a shared public IP address.</span></span> <span data-ttu-id="185b5-196">IP-alapú SSL tanúsítvány kötése, az App Service létrehoz egy új, dedikált IP-címet a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="185b5-196">When you bind a certificate with IP-based SSL, App Service creates a new, dedicated IP address for your web app.</span></span>

<span data-ttu-id="185b5-197">Ha a webes alkalmazás egy A rekordot van leképezve, a tartomány rendszerleíró adatbázis frissítése az új, dedikált IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="185b5-197">If you have mapped an A record to your web app, update your domain registry with this new, dedicated IP address.</span></span>

<span data-ttu-id="185b5-198">A webalkalmazás **egyéni tartomány** lap tartalmát az új, dedikált IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="185b5-198">Your web app's **Custom domain** page is updated with the new, dedicated IP address.</span></span> <span data-ttu-id="185b5-199">[Az IP-cím másolása](app-service-web-tutorial-custom-domain.md#info), majd [megfelelteti az A rekord](app-service-web-tutorial-custom-domain.md#map-an-a-record) ezt új IP-címet.</span><span class="sxs-lookup"><span data-stu-id="185b5-199">[Copy this IP address](app-service-web-tutorial-custom-domain.md#info), then [remap the A record](app-service-web-tutorial-custom-domain.md#map-an-a-record) to this new IP address.</span></span>

<a name="test"></a>

## <a name="test-https"></a><span data-ttu-id="185b5-200">Teszt HTTPS</span><span class="sxs-lookup"><span data-stu-id="185b5-200">Test HTTPS</span></span>

<span data-ttu-id="185b5-201">Ehhez marad most csak a győződjön meg arról, hogy HTTPS működik-e az egyéni tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="185b5-201">All that's left to do now is to make sure that HTTPS works for your custom domain.</span></span> <span data-ttu-id="185b5-202">A különböző böngészők, keresse meg a `https://<your.custom.domain>` megtekintéséhez, hogy kész a webalkalmazás működik.</span><span class="sxs-lookup"><span data-stu-id="185b5-202">In various browsers, browse to `https://<your.custom.domain>` to see that it serves up your web app.</span></span>

![Az Azure alkalmazásba portálnavigációjával](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> <span data-ttu-id="185b5-204">Ha a webes alkalmazást ad meg a tanúsítvány érvényesítési hibákat, valószínűleg egy önaláírt tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="185b5-204">If your web app gives you certificate validation errors, you're probably using a self-signed certificate.</span></span>
>
> <span data-ttu-id="185b5-205">Ha nem, a helyzet, előfordulhat, hogy kilépett azokat a köztes tanúsítványokat amikor exportálta a tanúsítványt a PFX-fájlba.</span><span class="sxs-lookup"><span data-stu-id="185b5-205">If that's not the case, you may have left out intermediate certificates when you export your certificate to the PFX file.</span></span>

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a><span data-ttu-id="185b5-206">HTTPS kényszerítése</span><span class="sxs-lookup"><span data-stu-id="185b5-206">Enforce HTTPS</span></span>

<span data-ttu-id="185b5-207">Alkalmazás szolgáltatásnak nincs *nem* kényszerítése a HTTPS-t, így bárki, aki továbbra is elérheti a HTTP-n keresztül webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="185b5-207">App Service does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="185b5-208">A webalkalmazás HTTPS kényszerítéséhez a átdolgozás szabály megadása a _web.config_ fájl a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="185b5-208">To enforce HTTPS for your web app, define a rewrite rule in the _web.config_ file for your web app.</span></span> <span data-ttu-id="185b5-209">App Service használja ezt a fájlt, a nyelvi keretrendszerébe webalkalmazás függetlenül.</span><span class="sxs-lookup"><span data-stu-id="185b5-209">App Service uses this file, regardless of the language framework of your web app.</span></span>

> [!NOTE]
> <span data-ttu-id="185b5-210">Kérelmek átirányítása nyelvspecifikus van.</span><span class="sxs-lookup"><span data-stu-id="185b5-210">There is language-specific redirection of requests.</span></span> <span data-ttu-id="185b5-211">ASP.NET MVC használhatja a [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) helyett a módosítsa úgy a szabály a szűrő _web.config_.</span><span class="sxs-lookup"><span data-stu-id="185b5-211">ASP.NET MVC can use the [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter instead of the rewrite rule in _web.config_.</span></span>

<span data-ttu-id="185b5-212">Ha a .NET-fejlesztők, ezzel a fájllal viszonylag tisztában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="185b5-212">If you're a .NET developer, you should be relatively familiar with this file.</span></span> <span data-ttu-id="185b5-213">A megoldás gyökérkönyvtárában van.</span><span class="sxs-lookup"><span data-stu-id="185b5-213">It is in the root of your solution.</span></span>

<span data-ttu-id="185b5-214">Azt is megteheti Ha a PHP, Node.js, Python vagy Java fejleszt, esély van jelenleg ezt a fájlt az Ön nevében létrehozott App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="185b5-214">Alternatively, if you develop with PHP, Node.js, Python, or Java, there is a chance we generated this file on your behalf in App Service.</span></span>

<span data-ttu-id="185b5-215">A webalkalmazás FTP végponthoz kapcsolódni a megadott utasítások szerint [telepítse az alkalmazást az Azure App Service segítségével FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="185b5-215">Connect to your web app's FTP endpoint by following the instructions at [Deploy your app to Azure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="185b5-216">Ez a fájl megtalálható _/home/site/wwwroot_.</span><span class="sxs-lookup"><span data-stu-id="185b5-216">This file should be located in _/home/site/wwwroot_.</span></span> <span data-ttu-id="185b5-217">Ha nem, hozzon létre egy _web.config_ ebben a mappában van a következő XML-fájlt:</span><span class="sxs-lookup"><span data-stu-id="185b5-217">If not, create a _web.config_ file in this folder with the following XML:</span></span>

```xml   
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <!-- BEGIN rule ELEMENT FOR HTTPS REDIRECT -->
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
        <!-- END rule ELEMENT FOR HTTPS REDIRECT -->
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

<span data-ttu-id="185b5-218">Egy létező _web.config_ fájlt, másolja a teljes `<rule>` az elem a _web.config_tartozó `configuration/system.webServer/rewrite/rules` elemet.</span><span class="sxs-lookup"><span data-stu-id="185b5-218">For an existing _web.config_ file, copy the entire `<rule>` element into your _web.config_'s `configuration/system.webServer/rewrite/rules` element.</span></span> <span data-ttu-id="185b5-219">Ha nincsenek más `<rule>` elemeinek a _web.config_, helyezze a másolt `<rule>` elem előtt a másik `<rule>` elemek.</span><span class="sxs-lookup"><span data-stu-id="185b5-219">If there are other `<rule>` elements in your _web.config_, place the copied `<rule>` element before the other `<rule>` elements.</span></span>

<span data-ttu-id="185b5-220">Ez a szabály egy HTTP 301 (Állandó átirányítás) és a HTTPS protokoll ad vissza, amikor a felhasználó egy HTTP-kérelem esetén a webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="185b5-220">This rule returns an HTTP 301 (permanent redirect) to the HTTPS protocol whenever the user makes an HTTP request to your web app.</span></span> <span data-ttu-id="185b5-221">Például az átirányítja a `http://contoso.com` való `https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="185b5-221">For example, it redirects from `http://contoso.com` to `https://contoso.com`.</span></span>

<span data-ttu-id="185b5-222">Az IIS URL-újraíró modult további információkért tekintse meg a [URL-újraíró](http://www.iis.net/downloads/microsoft/url-rewrite) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="185b5-222">For more information on the IIS URL Rewrite module, see the [URL Rewrite](http://www.iis.net/downloads/microsoft/url-rewrite) documentation.</span></span>

## <a name="enforce-https-for-web-apps-on-linux"></a><span data-ttu-id="185b5-223">A Web Apps Linux HTTPS kényszerítése</span><span class="sxs-lookup"><span data-stu-id="185b5-223">Enforce HTTPS for Web Apps on Linux</span></span>

<span data-ttu-id="185b5-224">Linux alkalmazás-szolgáltatásnak nincs *nem* kényszerítése a HTTPS-t, így bárki, aki továbbra is elérheti a HTTP-n keresztül webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="185b5-224">App Service on Linux does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="185b5-225">A webalkalmazás HTTPS kényszerítéséhez a átdolgozás szabály megadása a _.htaccess_ fájl a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="185b5-225">To enforce HTTPS for your web app, define a rewrite rule in the _.htaccess_ file for your web app.</span></span> 

<span data-ttu-id="185b5-226">A webalkalmazás FTP végponthoz kapcsolódni a megadott utasítások szerint [telepítse az alkalmazást az Azure App Service segítségével FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="185b5-226">Connect to your web app's FTP endpoint by following the instructions at [Deploy your app to Azure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="185b5-227">A _/home/site/wwwroot_, hozzon létre egy _.htaccess_ fájl a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="185b5-227">In _/home/site/wwwroot_, create an _.htaccess_ file with the following code:</span></span>

```
RewriteEngine On
RewriteCond %{HTTP:X-ARR-SSL} ^$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

<span data-ttu-id="185b5-228">Ez a szabály egy HTTP 301 (Állandó átirányítás) és a HTTPS protokoll ad vissza, amikor a felhasználó egy HTTP-kérelem esetén a webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="185b5-228">This rule returns an HTTP 301 (permanent redirect) to the HTTPS protocol whenever the user makes an HTTP request to your web app.</span></span> <span data-ttu-id="185b5-229">Például az átirányítja a `http://contoso.com` való `https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="185b5-229">For example, it redirects from `http://contoso.com` to `https://contoso.com`.</span></span>

## <a name="automate-with-scripts"></a><span data-ttu-id="185b5-230">Parancsfájlok automatizálásához</span><span class="sxs-lookup"><span data-stu-id="185b5-230">Automate with scripts</span></span>

<span data-ttu-id="185b5-231">Automatizálható SSL-kötések a webalkalmazás, parancsfájlok, használja a [Azure CLI](/cli/azure/install-azure-cli) vagy [Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="185b5-231">You can automate SSL bindings for your web app with scripts, using the [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="azure-cli"></a><span data-ttu-id="185b5-232">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="185b5-232">Azure CLI</span></span>

<span data-ttu-id="185b5-233">A következő parancsot az exportált PFX-fájlt feltölti, és az ujjlenyomat beolvasása.</span><span class="sxs-lookup"><span data-stu-id="185b5-233">The following command uploads an exported PFX file and gets the thumbprint.</span></span>

```bash
thumbprint=$(az appservice web config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

<span data-ttu-id="185b5-234">Az alábbi parancs hozzáadja az SNI-alapú SSL-kötést, az előző parancs ujjlenyomatának használatával.</span><span class="sxs-lookup"><span data-stu-id="185b5-234">The following command adds an SNI-based SSL binding, using the thumbprint from the previous command.</span></span>

```bash
az appservice web config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a><span data-ttu-id="185b5-235">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="185b5-235">Azure PowerShell</span></span>

<span data-ttu-id="185b5-236">A következő parancsot az exportált PFX-fájlt feltölti, és hozzáadja az SNI-alapú SSL-kötést.</span><span class="sxs-lookup"><span data-stu-id="185b5-236">The following command uploads an exported PFX file and adds an SNI-based SSL binding.</span></span>

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="next-steps"></a><span data-ttu-id="185b5-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="185b5-237">Next steps</span></span>

<span data-ttu-id="185b5-238">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="185b5-238">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="185b5-239">Frissítse az alkalmazás tarifacsomag kiválasztása</span><span class="sxs-lookup"><span data-stu-id="185b5-239">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="185b5-240">Az egyéni SSL-tanúsítvány kötését az App Service</span><span class="sxs-lookup"><span data-stu-id="185b5-240">Bind your custom SSL certificate to App Service</span></span>
> * <span data-ttu-id="185b5-241">Az alkalmazás HTTPS kényszerítése</span><span class="sxs-lookup"><span data-stu-id="185b5-241">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="185b5-242">SSL-tanúsítvány kötés parancsfájlokkal automatizálhatja</span><span class="sxs-lookup"><span data-stu-id="185b5-242">Automate SSL certificate binding with scripts</span></span>

<span data-ttu-id="185b5-243">A következő oktatóanyag megtudhatja, hogyan használható az Azure Content Delivery Network továbblépés.</span><span class="sxs-lookup"><span data-stu-id="185b5-243">Advance to the next tutorial to learn how to use Azure Content Delivery Network.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="185b5-244">A Content Delivery Network (CDN) hozzáadása az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="185b5-244">Add a Content Delivery Network (CDN) to an Azure App Service</span></span>](app-service-web-tutorial-content-delivery-network.md)
