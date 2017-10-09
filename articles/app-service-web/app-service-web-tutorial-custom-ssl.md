---
title: "aaaBind egy meglévő egyéni SSL-tanúsítvány tooAzure webalkalmazások |} Microsoft Docs"
description: "Ismerje meg tootoobind, egy egyéni SSL tanúsítvány tooyour webalkalmazás, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazásba."
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
ms.openlocfilehash: 3503ba9f96c8ea8d18451e8bf9a9b441797ef44d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-tooazure-web-apps"></a><span data-ttu-id="ee6ef-103">Egy meglévő egyéni SSL tanúsítvány tooAzure webalkalmazások kötése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-103">Bind an existing custom SSL certificate tooAzure Web Apps</span></span>

<span data-ttu-id="ee6ef-104">Az Azure Web Apps jól skálázható, önálló javítási a webhelyszolgáltató biztosít.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="ee6ef-105">Az oktatóanyag bemutatja, hogyan toobind egyéni SSL tanúsítvány, amikor egy megbízható hitelesítésszolgáltatótól származó túl vásárolt[Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-105">This tutorial shows you how toobind a custom SSL certificate that you purchased from a trusted certificate authority too[Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="ee6ef-106">Amikor végzett, be fog tudni tooaccess: a webalkalmazás HTTPS-végpont hello az egyéni DNS-tartományt.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-106">When you're finished, you'll be able tooaccess your web app at hello HTTPS endpoint of your custom DNS domain.</span></span>

![Egyéni SSL-tanúsítvánnyal webalkalmazás](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

<span data-ttu-id="ee6ef-108">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="ee6ef-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee6ef-109">Frissítse az alkalmazás tarifacsomag kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ee6ef-109">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="ee6ef-110">Az egyéni SSL-tanúsítvány tooApp szolgáltatás kötése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-110">Bind your custom SSL certificate tooApp Service</span></span>
> * <span data-ttu-id="ee6ef-111">Az alkalmazás HTTPS kényszerítése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-111">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="ee6ef-112">SSL-tanúsítvány kötés parancsfájlokkal automatizálhatja</span><span class="sxs-lookup"><span data-stu-id="ee6ef-112">Automate SSL certificate binding with scripts</span></span>

> [!NOTE]
> <span data-ttu-id="ee6ef-113">Tooget egy egyéni SSL-tanúsítványra van szükség, ha közvetlenül lekérni egy-egy hello Azure-portálon, és kösse tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-113">If you need tooget a custom SSL certificate, you can get one in hello Azure portal directly and bind it tooyour web app.</span></span> <span data-ttu-id="ee6ef-114">Hajtsa végre a hello [App Service-tanúsítványokkal oktatóanyag](web-sites-purchase-ssl-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-114">Follow hello [App Service Certificates tutorial](web-sites-purchase-ssl-web-site.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee6ef-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ee6ef-115">Prerequisites</span></span>

<span data-ttu-id="ee6ef-116">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="ee6ef-116">toocomplete this tutorial:</span></span>

- [<span data-ttu-id="ee6ef-117">Az App Service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee6ef-117">Create an App Service app</span></span>](/azure/app-service/)
- [<span data-ttu-id="ee6ef-118">Egy egyéni DNS nevét tooyour webalkalmazás leképezése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-118">Map a custom DNS name tooyour web app</span></span>](app-service-web-tutorial-custom-domain.md)
- <span data-ttu-id="ee6ef-119">Szerezzen be egy megbízható hitelesítésszolgáltatótól származó SSL-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="ee6ef-119">Acquire an SSL certificate from a trusted certificate authority</span></span>

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a><span data-ttu-id="ee6ef-120">Az SSL-tanúsítványra vonatkozó követelményekről</span><span class="sxs-lookup"><span data-stu-id="ee6ef-120">Requirements for your SSL certificate</span></span>

<span data-ttu-id="ee6ef-121">toouse egy tanúsítványt az App Service-ben, hello tanúsítvány összes hello követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="ee6ef-121">toouse a certificate in App Service, hello certificate must meet all hello following requirements:</span></span>

* <span data-ttu-id="ee6ef-122">Megbízható hitelesítésszolgáltató által aláírt</span><span class="sxs-lookup"><span data-stu-id="ee6ef-122">Signed by a trusted certificate authority</span></span>
* <span data-ttu-id="ee6ef-123">Jelszóval védett PFX-fájlba exportált</span><span class="sxs-lookup"><span data-stu-id="ee6ef-123">Exported as a password-protected PFX file</span></span>
* <span data-ttu-id="ee6ef-124">Titkos kulcs legalább 2048 bit hosszúságú tartalmazza</span><span class="sxs-lookup"><span data-stu-id="ee6ef-124">Contains private key at least 2048 bits long</span></span>
* <span data-ttu-id="ee6ef-125">Hello tanúsítványláncában szereplő összes köztes tanúsítvány tartalmazza</span><span class="sxs-lookup"><span data-stu-id="ee6ef-125">Contains all intermediate certificates in hello certificate chain</span></span>

> [!NOTE]
> <span data-ttu-id="ee6ef-126">**Elliptikus görbe alapú titkosítást (ECC) tanúsítványok** App Service képes együttműködni, de ez a cikk nem vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-126">**Elliptic Curve Cryptography (ECC) certificates** can work with App Service but are not covered by this article.</span></span> <span data-ttu-id="ee6ef-127">A hello szövegéről toocreate ECC tanúsítványokat a hitelesítésszolgáltató működik.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-127">Work with your certificate authority on hello exact steps toocreate ECC certificates.</span></span>

## <a name="prepare-your-web-app"></a><span data-ttu-id="ee6ef-128">A webes alkalmazás előkészítése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-128">Prepare your web app</span></span>

<span data-ttu-id="ee6ef-129">toobind egyéni SSL-tanúsítvány tooyour web app alkalmazásban a [App Service-csomag](https://azure.microsoft.com/pricing/details/app-service/) hello kell **alapvető**, **szabványos**, vagy **prémium** réteg.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-129">toobind a custom SSL certificate tooyour web app, your [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be in hello **Basic**, **Standard**, or **Premium** tier.</span></span> <span data-ttu-id="ee6ef-130">Ezt a lépést akkor győződjön meg arról, hogy a webalkalmazás a hello támogatott IP-címek.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-130">In this step, you make sure that your web app is in hello supported pricing tier.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="ee6ef-131">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="ee6ef-131">Log in tooAzure</span></span>

<span data-ttu-id="ee6ef-132">Nyissa meg hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-132">Open hello [Azure portal](https://portal.azure.com).</span></span>

### <a name="navigate-tooyour-web-app"></a><span data-ttu-id="ee6ef-133">Keresse meg a tooyour webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="ee6ef-133">Navigate tooyour web app</span></span>

<span data-ttu-id="ee6ef-134">Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson a webalkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-134">From hello left menu, click **App Services**, and then click hello name of your web app.</span></span>

![Webalkalmazás kiválasztása](./media/app-service-web-tutorial-custom-ssl/select-app.png)

<span data-ttu-id="ee6ef-136">Hello kezelése lapon webalkalmazás rendelkezik ki.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-136">You have landed in hello management page of your web app.</span></span>  

### <a name="check-hello-pricing-tier"></a><span data-ttu-id="ee6ef-137">IP-címek hello ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-137">Check hello pricing tier</span></span>

<span data-ttu-id="ee6ef-138">A weblap app hello bal oldali navigációs, görgessen toohello **beállítások** válassza ki azt **vertikális felskálázás (App Service-csomag)**.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-138">In hello left-hand navigation of your web app page, scroll toohello **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Méretezett menü](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

<span data-ttu-id="ee6ef-140">Ellenőrizze, hogy a webalkalmazás nem hello toomake **szabad** vagy **megosztott** réteg.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-140">Check toomake sure that your web app is not in hello **Free** or **Shared** tier.</span></span> <span data-ttu-id="ee6ef-141">A webalkalmazás jelenlegi rétegtől ki van jelölve, egy kékkel által.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-141">Your web app's current tier is highlighted by a dark blue box.</span></span>

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

<span data-ttu-id="ee6ef-143">Egyéni SSL nem támogatja a hello **szabad** vagy **megosztott** réteg.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-143">Custom SSL is not supported in hello **Free** or **Shared** tier.</span></span> <span data-ttu-id="ee6ef-144">Ha be kell tooscale, kövesse hello hello következő szakaszban található lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-144">If you need tooscale up, follow hello steps in hello next section.</span></span> <span data-ttu-id="ee6ef-145">Ellenkező esetben zárja be a hello **válasszon tarifacsomagot** lapon, és hagyja ki túl[töltse fel, és az SSL-tanúsítvány kötését](#upload).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-145">Otherwise, close hello **Choose your pricing tier** page and skip too[Upload and bind your SSL certificate](#upload).</span></span>

### <a name="scale-up-your-app-service-plan"></a><span data-ttu-id="ee6ef-146">Vertikális felskálázás App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="ee6ef-146">Scale up your App Service plan</span></span>

<span data-ttu-id="ee6ef-147">Válasszon ki egy hello **alapvető**, **szabványos**, vagy **prémium** rétegek.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-147">Select one of hello **Basic**, **Standard**, or **Premium** tiers.</span></span>

<span data-ttu-id="ee6ef-148">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-148">Click **Select**.</span></span>

![Tarifacsomag kiválasztása](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

<span data-ttu-id="ee6ef-150">Amikor megjelenik a következő értesítési hello, hello skálázási művelet be nem fejeződött.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-150">When you see hello following notification, hello scale operation is complete.</span></span>

![Vertikális felskálázás értesítés](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a><span data-ttu-id="ee6ef-152">Az SSL-tanúsítvány kötése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-152">Bind your SSL certificate</span></span>

<span data-ttu-id="ee6ef-153">Akkor van kész tooupload az SSL-tanúsítvány tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-153">You are ready tooupload your SSL certificate tooyour web app.</span></span>

### <a name="merge-intermediate-certificates"></a><span data-ttu-id="ee6ef-154">Köztes tanúsítványok egyesítése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-154">Merge intermediate certificates</span></span>

<span data-ttu-id="ee6ef-155">Ha a hitelesítésszolgáltató lehetővé teszi több tanúsítvány hello tanúsítványlánc, toomerge hello tanúsítványok sorrendben kell.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-155">If your certificate authority gives you multiple certificates in hello certificate chain, you need toomerge hello certificates in order.</span></span> 

<span data-ttu-id="ee6ef-156">toodo minden nyitott Ez a tanúsítvány is egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-156">toodo this, open each certificate you received in a text editor.</span></span> 

<span data-ttu-id="ee6ef-157">Hello egyesített tanúsítvány nevű fájl létrehozása _mergedcertificate.crt_.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-157">Create a file for hello merged certificate, called _mergedcertificate.crt_.</span></span> <span data-ttu-id="ee6ef-158">Egy szövegszerkesztőben másolja az egyes tanúsítványok hello tartalmat a fájlba.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-158">In a text editor, copy hello content of each certificate into this file.</span></span> <span data-ttu-id="ee6ef-159">a tanúsítványok hello sorrendjét a következő sablon hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="ee6ef-159">hello order of your certificates should look like hello following template:</span></span>

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

### <a name="export-certificate-toopfx"></a><span data-ttu-id="ee6ef-160">Tanúsítvány tooPFX exportálása</span><span class="sxs-lookup"><span data-stu-id="ee6ef-160">Export certificate tooPFX</span></span>

<span data-ttu-id="ee6ef-161">Exportálja az egyesített SSL-tanúsítvány hello titkos kulccsal, a tanúsítvány kérelmezéséhez során létrejött.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-161">Export your merged SSL certificate with hello private key that your certificate request was generated with.</span></span>

<span data-ttu-id="ee6ef-162">Ha a tanúsítvány kérelmezéséhez OpenSSL használatával jön létre, majd hozott létre a titkos kulcs fájlja.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-162">If you generated your certificate request using OpenSSL, then you have created a private key file.</span></span> <span data-ttu-id="ee6ef-163">tooexport a tanúsítvány tooPFX, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-163">tooexport your certificate tooPFX, run hello following command.</span></span> <span data-ttu-id="ee6ef-164">Cserélje le a helyőrzőket hello  _&lt;titkos kulcsfájl >_ és  _&lt;egyesített-tanúsítványfájl >_.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-164">Replace hello placeholders _&lt;private-key-file>_ and _&lt;merged-certificate-file>_.</span></span>

```
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

<span data-ttu-id="ee6ef-165">Amikor a rendszer kéri, adja meg az exportálási jelszó.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-165">When prompted, define an export password.</span></span> <span data-ttu-id="ee6ef-166">Ezt a jelszót fog használni, az SSL tanúsítvány tooApp szolgáltatás később feltöltésekor.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-166">You'll use this password when uploading your SSL certificate tooApp Service later.</span></span>

<span data-ttu-id="ee6ef-167">Ha az IIS vagy _Certreq.exe_ toogenerate a tanúsítványkérelmet, a telepítés hello tanúsítvány tooyour helyi számítógép, majd [hello tanúsítvány tooPFX exportálása](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-167">If you used IIS or _Certreq.exe_ toogenerate your certificate request, install hello certificate tooyour local machine, and then [export hello certificate tooPFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span></span>

### <a name="upload-your-ssl-certificate"></a><span data-ttu-id="ee6ef-168">Az SSL-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-168">Upload your SSL certificate</span></span>

<span data-ttu-id="ee6ef-169">tooupload az SSL-tanúsítvány, kattintson a **SSL-tanúsítványok** a bal oldali navigációs webalkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-169">tooupload your SSL certificate, click **SSL certificates** in hello left navigation of your web app.</span></span>

<span data-ttu-id="ee6ef-170">Kattintson a **-tanúsítvány feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-170">Click **Upload Certificate**.</span></span>

<span data-ttu-id="ee6ef-171">A **PFX-tanúsítványfájlt**, válassza ki a PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-171">In **PFX Certificate File**, select your PFX file.</span></span> <span data-ttu-id="ee6ef-172">A **tanúsítványjelszavas**, típus hello hello PFX-fájl exportálása során létrehozott jelszót.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-172">In **Certificate password**, type hello password that you created when you exported hello PFX file.</span></span>

<span data-ttu-id="ee6ef-173">Kattintson a **Feltöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-173">Click **Upload**.</span></span>

![Tanúsítvány feltöltése](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

<span data-ttu-id="ee6ef-175">Megjelenik az App Service befejezése után a tanúsítvány feltöltése hello **SSL-tanúsítványok** lap.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-175">When App Service finishes uploading your certificate, it appears in hello **SSL certificates** page.</span></span>

![A tanúsítvány feltöltése](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a><span data-ttu-id="ee6ef-177">Az SSL-tanúsítvány kötése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-177">Bind your SSL certificate</span></span>

<span data-ttu-id="ee6ef-178">A hello **SSL-kötések** kattintson **kötés hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-178">In hello **SSL bindings** section, click **Add binding**.</span></span>

<span data-ttu-id="ee6ef-179">A hello **hozzá SSL-kötés** lapján hello legördülő listák megnyílásának tooselect hello tartomány neve toosecure, valamint hello tanúsítvány toouse.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-179">In hello **Add SSL Binding** page, use hello dropdowns tooselect hello domain name toosecure, and hello certificate toouse.</span></span>

> [!NOTE]
> <span data-ttu-id="ee6ef-180">Ha a tanúsítvány feltöltött, de nem látja a hello hello tartomány neve **állomásnév** legördülő menüből, próbálja meg frissíteni hello böngésző lapot.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-180">If you have uploaded your certificate but don't see hello domain name(s) in hello **Hostname** dropdown, try refreshing hello browser page.</span></span>
>
>

<span data-ttu-id="ee6ef-181">A **SSL típus**, jelölje be e toouse  **[kiszolgálónév jelzése (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  vagy IP-alapú SSL.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-181">In **SSL Type**, select whether toouse **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP-based SSL.</span></span>

- <span data-ttu-id="ee6ef-182">**Az SNI-alapú SSL** -több SNI-alapú SSL-kötések adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-182">**SNI-based SSL** - Multiple SNI-based SSL bindings may be added.</span></span> <span data-ttu-id="ee6ef-183">Ez a beállítás lehetővé teszi több SSL-tanúsítványok toosecure hello a több tartományt ugyanazon IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-183">This option allows multiple SSL certificates toosecure multiple domains on hello same IP address.</span></span> <span data-ttu-id="ee6ef-184">(Beleértve az Internet Explorer, Chrome, Firefox és Opera) a legtöbb modern böngésző támogatja az SNI (átfogóbb böngésző információt, [kiszolgálónév jelzése](http://wikipedia.org/wiki/Server_Name_Indication)).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-184">Most modern browsers (including Internet Explorer, Chrome, Firefox, and Opera) support SNI (find more comprehensive browser support information at [Server Name Indication](http://wikipedia.org/wiki/Server_Name_Indication)).</span></span>
- <span data-ttu-id="ee6ef-185">**IP-alapú SSL** -csak egy IP-alapú SSL-kötés adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-185">**IP-based SSL** - Only one IP-based SSL binding may be added.</span></span> <span data-ttu-id="ee6ef-186">Ez a beállítás lehetővé teszi, hogy csak egy SSL-tanúsítvány toosecure egy dedikált nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-186">This option allows only one SSL certificate toosecure a dedicated public IP address.</span></span> <span data-ttu-id="ee6ef-187">toosecure több tartomány, biztosítania kell őket minden használatával hello egy SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-187">toosecure multiple domains, you must secure them all using hello same SSL certificate.</span></span> <span data-ttu-id="ee6ef-188">Ez a lehetőség hello hagyományos SSL-kötés.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-188">This is hello traditional option for SSL binding.</span></span>

<span data-ttu-id="ee6ef-189">Kattintson a **kötés hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-189">Click **Add Binding**.</span></span>

![SSL-tanúsítvány kötésének létrehozása](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

<span data-ttu-id="ee6ef-191">Megjelenik az App Service befejezése után a tanúsítvány feltöltése hello **SSL-kötések** szakaszok.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-191">When App Service finishes uploading your certificate, it appears in hello **SSL bindings** sections.</span></span>

![A kötött tanúsítvány tooweb alkalmazás](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a><span data-ttu-id="ee6ef-193">Adja meg újból egy rekordot az IP-SSL</span><span class="sxs-lookup"><span data-stu-id="ee6ef-193">Remap A record for IP SSL</span></span>

<span data-ttu-id="ee6ef-194">Ha a webalkalmazáson belüli IP-alapú SSL nem adja meg, hagyja ki túl[teszt a HTTPS-t az egyéni tartomány](#test).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-194">If you don't use IP-based SSL in your web app, skip too[Test HTTPS for your custom domain](#test).</span></span>

<span data-ttu-id="ee6ef-195">Alapértelmezés szerint a webalkalmazás egy megosztott nyilvános IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-195">By default, your web app uses a shared public IP address.</span></span> <span data-ttu-id="ee6ef-196">IP-alapú SSL tanúsítvány kötése, az App Service létrehoz egy új, dedikált IP-címet a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-196">When you bind a certificate with IP-based SSL, App Service creates a new, dedicated IP address for your web app.</span></span>

<span data-ttu-id="ee6ef-197">Ha egy A rekord tooyour webalkalmazás leképezett, a tartomány rendszerleíró adatbázis frissítése az új, dedikált IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-197">If you have mapped an A record tooyour web app, update your domain registry with this new, dedicated IP address.</span></span>

<span data-ttu-id="ee6ef-198">A webalkalmazás **egyéni tartomány** lap tartalmát hello új, dedikált IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-198">Your web app's **Custom domain** page is updated with hello new, dedicated IP address.</span></span> <span data-ttu-id="ee6ef-199">[Az IP-cím másolása](app-service-web-tutorial-custom-domain.md#info), majd [újramegfeleltetése hello rekord](app-service-web-tutorial-custom-domain.md#map-an-a-record) toothis új IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-199">[Copy this IP address](app-service-web-tutorial-custom-domain.md#info), then [remap hello A record](app-service-web-tutorial-custom-domain.md#map-an-a-record) toothis new IP address.</span></span>

<a name="test"></a>

## <a name="test-https"></a><span data-ttu-id="ee6ef-200">Teszt HTTPS</span><span class="sxs-lookup"><span data-stu-id="ee6ef-200">Test HTTPS</span></span>

<span data-ttu-id="ee6ef-201">Most már elhagyta toodo csak a toomake meg arról, hogy HTTPS működik-e az egyéni tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-201">All that's left toodo now is toomake sure that HTTPS works for your custom domain.</span></span> <span data-ttu-id="ee6ef-202">A különböző böngészők, tallózással keresse meg túl`https://<your.custom.domain>` toosee, amely kész a webalkalmazás működik.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-202">In various browsers, browse too`https://<your.custom.domain>` toosee that it serves up your web app.</span></span>

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> <span data-ttu-id="ee6ef-204">Ha a webes alkalmazást ad meg a tanúsítvány érvényesítési hibákat, valószínűleg egy önaláírt tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-204">If your web app gives you certificate validation errors, you're probably using a self-signed certificate.</span></span>
>
> <span data-ttu-id="ee6ef-205">Ha ez nem hello eset, akkor előfordulhat, hogy kimaradt köztes tanúsítványa a toohello PFX-tanúsítványfájlt exportálásakor.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-205">If that's not hello case, you may have left out intermediate certificates when you export your certificate toohello PFX file.</span></span>

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a><span data-ttu-id="ee6ef-206">HTTPS kényszerítése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-206">Enforce HTTPS</span></span>

<span data-ttu-id="ee6ef-207">Alkalmazás szolgáltatásnak nincs *nem* kényszerítése a HTTPS-t, így bárki, aki továbbra is elérheti a HTTP-n keresztül webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-207">App Service does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="ee6ef-208">a webalkalmazás HTTPS tooenforce átdolgozás szabály megadása a hello _web.config_ fájl a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-208">tooenforce HTTPS for your web app, define a rewrite rule in hello _web.config_ file for your web app.</span></span> <span data-ttu-id="ee6ef-209">App Service ezt a fájlt, függetlenül a webalkalmazás hello nyelvi keretrendszert használja.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-209">App Service uses this file, regardless of hello language framework of your web app.</span></span>

> [!NOTE]
> <span data-ttu-id="ee6ef-210">Kérelmek átirányítása nyelvspecifikus van.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-210">There is language-specific redirection of requests.</span></span> <span data-ttu-id="ee6ef-211">ASP.NET MVC használható hello [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) helyett hello módosítsa úgy a szabály a szűrő _web.config_.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-211">ASP.NET MVC can use hello [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter instead of hello rewrite rule in _web.config_.</span></span>

<span data-ttu-id="ee6ef-212">Ha a .NET-fejlesztők, ezzel a fájllal viszonylag tisztában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-212">If you're a .NET developer, you should be relatively familiar with this file.</span></span> <span data-ttu-id="ee6ef-213">A megoldás hello gyökérkönyvtárában van.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-213">It is in hello root of your solution.</span></span>

<span data-ttu-id="ee6ef-214">Azt is megteheti Ha a PHP, Node.js, Python vagy Java fejleszt, esély van jelenleg ezt a fájlt az Ön nevében létrehozott App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-214">Alternatively, if you develop with PHP, Node.js, Python, or Java, there is a chance we generated this file on your behalf in App Service.</span></span>

<span data-ttu-id="ee6ef-215">Csatlakozás tooyour web app FTP végpont: hello utasításokat követve [telepítheti az alkalmazást tooAzure használatával az FTP/S App Service](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-215">Connect tooyour web app's FTP endpoint by following hello instructions at [Deploy your app tooAzure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="ee6ef-216">Ez a fájl megtalálható _/home/site/wwwroot_.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-216">This file should be located in _/home/site/wwwroot_.</span></span> <span data-ttu-id="ee6ef-217">Ha nem, hozzon létre egy _web.config_ fájl a mappában, amelynek a következő XML hello:</span><span class="sxs-lookup"><span data-stu-id="ee6ef-217">If not, create a _web.config_ file in this folder with hello following XML:</span></span>

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

<span data-ttu-id="ee6ef-218">Egy létező _web.config_ fájlt, másolja a hello teljes `<rule>` az elem a _web.config_tartozó `configuration/system.webServer/rewrite/rules` elemet.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-218">For an existing _web.config_ file, copy hello entire `<rule>` element into your _web.config_'s `configuration/system.webServer/rewrite/rules` element.</span></span> <span data-ttu-id="ee6ef-219">Ha nincsenek más `<rule>` elemeinek a _web.config_, másolt sor hello `<rule>` elem előtt más hello `<rule>` elemek.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-219">If there are other `<rule>` elements in your _web.config_, place hello copied `<rule>` element before hello other `<rule>` elements.</span></span>

<span data-ttu-id="ee6ef-220">Ez a szabály olyan HTTP 301 (Állandó átirányítás) toohello HTTPS protokollt adja vissza, amikor hello felhasználó esetén egy HTTP-kérelem tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-220">This rule returns an HTTP 301 (permanent redirect) toohello HTTPS protocol whenever hello user makes an HTTP request tooyour web app.</span></span> <span data-ttu-id="ee6ef-221">Például az átirányítja a `http://contoso.com` túl`https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-221">For example, it redirects from `http://contoso.com` too`https://contoso.com`.</span></span>

<span data-ttu-id="ee6ef-222">Hello IIS URL-újraíró modult további információkért lásd: hello [URL-újraíró](http://www.iis.net/downloads/microsoft/url-rewrite) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-222">For more information on hello IIS URL Rewrite module, see hello [URL Rewrite](http://www.iis.net/downloads/microsoft/url-rewrite) documentation.</span></span>

## <a name="enforce-https-for-web-apps-on-linux"></a><span data-ttu-id="ee6ef-223">A Web Apps Linux HTTPS kényszerítése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-223">Enforce HTTPS for Web Apps on Linux</span></span>

<span data-ttu-id="ee6ef-224">Linux alkalmazás-szolgáltatásnak nincs *nem* kényszerítése a HTTPS-t, így bárki, aki továbbra is elérheti a HTTP-n keresztül webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-224">App Service on Linux does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="ee6ef-225">a webalkalmazás HTTPS tooenforce átdolgozás szabály megadása a hello _.htaccess_ fájl a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-225">tooenforce HTTPS for your web app, define a rewrite rule in hello _.htaccess_ file for your web app.</span></span> 

<span data-ttu-id="ee6ef-226">Csatlakozás tooyour web app FTP végpont: hello utasításokat követve [telepítheti az alkalmazást tooAzure használatával az FTP/S App Service](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-226">Connect tooyour web app's FTP endpoint by following hello instructions at [Deploy your app tooAzure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="ee6ef-227">A _/home/site/wwwroot_, hozzon létre egy _.htaccess_ hello kód a következő fájlt:</span><span class="sxs-lookup"><span data-stu-id="ee6ef-227">In _/home/site/wwwroot_, create an _.htaccess_ file with hello following code:</span></span>

```
RewriteEngine On
RewriteCond %{HTTP:X-ARR-SSL} ^$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

<span data-ttu-id="ee6ef-228">Ez a szabály olyan HTTP 301 (Állandó átirányítás) toohello HTTPS protokollt adja vissza, amikor hello felhasználó esetén egy HTTP-kérelem tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-228">This rule returns an HTTP 301 (permanent redirect) toohello HTTPS protocol whenever hello user makes an HTTP request tooyour web app.</span></span> <span data-ttu-id="ee6ef-229">Például az átirányítja a `http://contoso.com` túl`https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-229">For example, it redirects from `http://contoso.com` too`https://contoso.com`.</span></span>

## <a name="automate-with-scripts"></a><span data-ttu-id="ee6ef-230">Parancsfájlok automatizálásához</span><span class="sxs-lookup"><span data-stu-id="ee6ef-230">Automate with scripts</span></span>

<span data-ttu-id="ee6ef-231">Automatizálható SSL-kötések parancsfájlok, a webalkalmazás hello segítségével [Azure CLI](/cli/azure/install-azure-cli) vagy [Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ee6ef-231">You can automate SSL bindings for your web app with scripts, using hello [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="azure-cli"></a><span data-ttu-id="ee6ef-232">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ee6ef-232">Azure CLI</span></span>

<span data-ttu-id="ee6ef-233">a következő parancs hello exportált PFX-fájlt feltölti, valamint lekéri a hello ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-233">hello following command uploads an exported PFX file and gets hello thumbprint.</span></span>

```bash
thumbprint=$(az appservice web config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

<span data-ttu-id="ee6ef-234">hello alábbi parancs hozzáadja az SNI-alapú SSL-kötést, hello ujjlenyomata hello előző parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-234">hello following command adds an SNI-based SSL binding, using hello thumbprint from hello previous command.</span></span>

```bash
az appservice web config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a><span data-ttu-id="ee6ef-235">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ee6ef-235">Azure PowerShell</span></span>

<span data-ttu-id="ee6ef-236">hello következő parancsot az exportált PFX-fájlt feltölti és hozzáadja az SNI-alapú SSL-kötést.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-236">hello following command uploads an exported PFX file and adds an SNI-based SSL binding.</span></span>

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="next-steps"></a><span data-ttu-id="ee6ef-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee6ef-237">Next steps</span></span>

<span data-ttu-id="ee6ef-238">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="ee6ef-238">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee6ef-239">Frissítse az alkalmazás tarifacsomag kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ee6ef-239">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="ee6ef-240">Az egyéni SSL-tanúsítvány tooApp szolgáltatás kötése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-240">Bind your custom SSL certificate tooApp Service</span></span>
> * <span data-ttu-id="ee6ef-241">Az alkalmazás HTTPS kényszerítése</span><span class="sxs-lookup"><span data-stu-id="ee6ef-241">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="ee6ef-242">SSL-tanúsítvány kötés parancsfájlokkal automatizálhatja</span><span class="sxs-lookup"><span data-stu-id="ee6ef-242">Automate SSL certificate binding with scripts</span></span>

<span data-ttu-id="ee6ef-243">Hogyan előzetes toohello következő útmutató toolearn toouse Azure Content Delivery Network.</span><span class="sxs-lookup"><span data-stu-id="ee6ef-243">Advance toohello next tutorial toolearn how toouse Azure Content Delivery Network.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ee6ef-244">Adja hozzá a Content Delivery Network (CDN) tooan Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ee6ef-244">Add a Content Delivery Network (CDN) tooan Azure App Service</span></span>](app-service-web-tutorial-content-delivery-network.md)
