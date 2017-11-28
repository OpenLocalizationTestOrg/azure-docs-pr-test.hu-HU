---
title: "aaaAdd az SSL-tanúsítvány tooyour Azure App Service alkalmazás |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd egy SSL tanúsítvány tooyour App Service-alkalmazást."
services: app-service
documentationcenter: .net
author: ahmedelnably
manager: stefsch
editor: cephalin
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: apurvajo
ms.openlocfilehash: f4652794ba745790a073264f6a102c64c73e8db0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a><span data-ttu-id="df502-103">SSL-tanúsítvány vásárlása és konfigurálása saját Azure App Service szolgáltatások számára</span><span class="sxs-lookup"><span data-stu-id="df502-103">Buy and Configure an SSL Certificate for your Azure App Service</span></span>

<span data-ttu-id="df502-104">Ebben az oktatóanyagban az SSL-tanúsítvány megvásárlásával fog biztonságos a webalkalmazás a  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, biztonságos helyen tárolja [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), és való társítás egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="df502-104">In this tutorial, you will secure your web app by purchasing an SSL certificate for your **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, securely storing it in [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), and associating it with a custom domain.</span></span>

## <a name="step-1---log-in-tooazure"></a><span data-ttu-id="df502-105">1. lépés – tooAzure bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="df502-105">Step 1 - Log in tooAzure</span></span>

<span data-ttu-id="df502-106">Jelentkezzen be toohello http://portal.azure.com: az Azure portál</span><span class="sxs-lookup"><span data-stu-id="df502-106">Log in toohello Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---place-an-ssl-certificate-order"></a><span data-ttu-id="df502-107">2. lépés - az SSL-tanúsítvány megrendelés</span><span class="sxs-lookup"><span data-stu-id="df502-107">Step 2 - Place an SSL Certificate order</span></span>

<span data-ttu-id="df502-108">Hozzon létre egy új SSL-tanúsítvány rendelés elhelyezhet [App szolgáltatástanúsítvány](https://portal.azure.com/#create/Microsoft.SSL) a hello **Azure-portálon**.</span><span class="sxs-lookup"><span data-stu-id="df502-108">You can place an SSL Certificate order by creating a new [App Service Certificate](https://portal.azure.com/#create/Microsoft.SSL) In hello **Azure portal**.</span></span>

![Tanúsítvány létrehozása](./media/app-service-web-purchase-ssl-web-site/createssl.png)

<span data-ttu-id="df502-110">Adjon meg egy rövid **neve** SSL-tanúsítványok a tanúsítványok, és írja be a hello **tartománynév**</span><span class="sxs-lookup"><span data-stu-id="df502-110">Enter a friendly **Name** for your SSL certificate and enter hello **Domain Name**</span></span>

> [!NOTE]
> <span data-ttu-id="df502-111">Ez az egyik legfontosabb részeit hello hello vásárlási eljárást.</span><span class="sxs-lookup"><span data-stu-id="df502-111">This is one of hello most critical parts of hello purchase process.</span></span> <span data-ttu-id="df502-112">Győződjön meg arról, hogy javítsa ki a gazdagép nevét (egyéni tartomány), amelyet az ezzel a tanúsítvánnyal tooprotect tooenter.</span><span class="sxs-lookup"><span data-stu-id="df502-112">Make sure tooenter correct host name (custom domain) that you want tooprotect with this certificate.</span></span> <span data-ttu-id="df502-113">**NE** hello állomásnévvel rendelkező Webszolgáltatáshoz append.</span><span class="sxs-lookup"><span data-stu-id="df502-113">**DO NOT** append hello Host name with WWW.</span></span> 
>

<span data-ttu-id="df502-114">Válassza ki a **előfizetés**, **erőforráscsoport**, és **tanúsítvány-SKU**</span><span class="sxs-lookup"><span data-stu-id="df502-114">Select your **Subscription**, **Resource Group**, and **Certificate SKU**</span></span>

> [!WARNING]
> <span data-ttu-id="df502-115">App Service-tanúsítványokkal csak használhatja más alkalmazásszolgáltatások belül hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="df502-115">App Service Certificates can only be used by other App Services within hello same subscription.</span></span>  
>

## <a name="step-3---store-hello-certificate-in-azure-key-vault"></a><span data-ttu-id="df502-116">3. lépés – az Azure Key Vault-tároló hello tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="df502-116">Step 3 - Store hello certificate in Azure Key Vault</span></span>

> [!NOTE]
> <span data-ttu-id="df502-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) Azure szolgáltatás, amely segít a felhőalapú alkalmazások és szolgáltatások által használt titkosítási kulcsok és titkos védelme.</span><span class="sxs-lookup"><span data-stu-id="df502-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) is an Azure service that helps safeguard cryptographic keys and secrets used by cloud applications and services.</span></span>
>

<span data-ttu-id="df502-118">Hello SSL-tanúsítvány a vásárlás befejezése után kell tooopen [App Service-tanúsítványokkal](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) erőforráspanelen.</span><span class="sxs-lookup"><span data-stu-id="df502-118">Once hello SSL Certificate purchase is complete, you need tooopen [App Service Certificates](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) Resource blade.</span></span>

![Helyezze be KV készen toostore képe](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

<span data-ttu-id="df502-120">Megfigyelheti, hogy van-e a tanúsítvány állapotának **"Függőben lévő kiállítási"** , néhány további lépést kell toocomplete ezzel a tanúsítvánnyal megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="df502-120">You will notice that Certificate status is **“Pending Issuance”** as there are few more steps you need toocomplete before you can start using this certificate.</span></span>

<span data-ttu-id="df502-121">Kattintson a **Tanúsítványkonfiguráció** belül tanúsítvány tulajdonságai panelen, majd kattintson a **1. lépés: tároló** toostore Azure Key Vault ezt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="df502-121">Click **Certificate Configuration** inside Certificate Properties blade and Click on **Step 1: Store** toostore this certificate in Azure Key Vault.</span></span>

<span data-ttu-id="df502-122">A **Key Vault állapot** panelen kattintson a **Key Vault tárház** toochoose egy meglévő kulcstároló toostore ezt a tanúsítványt **vagy hozzon létre új Key Vault** toocreate új kulcsot tároló azonos előfizetésbe és erőforráscsoportba csoporton belül.</span><span class="sxs-lookup"><span data-stu-id="df502-122">From **Key Vault Status** Blade, click **Key Vault Repository** toochoose an existing Key Vault toostore this certificate **OR Create New Key Vault** toocreate new Key Vault inside same subscription and resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="df502-123">Az Azure Key Vault tárolja a tanúsítványt a minimális díja van.</span><span class="sxs-lookup"><span data-stu-id="df502-123">Azure Key Vault has minimal charges for storing this certificate.</span></span>
> <span data-ttu-id="df502-124">További információkért lásd:  **[Azure Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/)**.</span><span class="sxs-lookup"><span data-stu-id="df502-124">For more information, see **[Azure Key Vault Pricing Details](https://azure.microsoft.com/pricing/details/key-vault/)**.</span></span>
>

<span data-ttu-id="df502-125">Miután kiválasztotta hello Key Vault tárház toostore ezt a tanúsítványt, hello **tároló** beállítást meg kell jelennie a sikeres.</span><span class="sxs-lookup"><span data-stu-id="df502-125">Once you have selected hello Key Vault Repository toostore this certificate in, hello **Store** option should show success.</span></span>

![Helyezze be a tároló sikeres KV képe](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-hello-domain-ownership"></a><span data-ttu-id="df502-127">4. lépés - hello tartomány tulajdonjog ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="df502-127">Step 4 - Verify hello Domain Ownership</span></span>

> [!NOTE]
> <span data-ttu-id="df502-128">App service-tanúsítványokkal által támogatott tartományok ellenőrzésének három típusa van: tartomány, E-mail, manuális ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="df502-128">There are three types of domain verification supported by App service Certificates: Domain, Mail, Manual Verification.</span></span> <span data-ttu-id="df502-129">Ezek magyarázata hello részletesebben [szakasz speciális](#advanced).</span><span class="sxs-lookup"><span data-stu-id="df502-129">These are explained in more details in hello [Advanced section](#advanced).</span></span>

<span data-ttu-id="df502-130">A hello azonos **Tanúsítványkonfiguráció** panel használja a 3. lépésben, kattintson a **2. lépés: Ellenőrizze**.</span><span class="sxs-lookup"><span data-stu-id="df502-130">From hello same **Certificate Configuration** blade you used in Step 3, click **Step 2: Verify**.</span></span>

<span data-ttu-id="df502-131">**Tartományok ellenőrzésének** hello legkényelmesebben folyamat azt **csak ha** rendelkezik  **[az Azure App Service szolgáltatásban az egyéni tartomány vásárolt.](custom-dns-web-site-buydomains-web-app.md)**</span><span class="sxs-lookup"><span data-stu-id="df502-131">**Domain Verification** This is hello most convenient process **ONLY IF** you have **[purchased your custom domain from Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span></span>
<span data-ttu-id="df502-132">Kattintson a **ellenőrizze** gomb toocomplete ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="df502-132">Click on **Verify** button toocomplete this step.</span></span>

![Helyezze be a tartomány ellenőrzése képe](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

<span data-ttu-id="df502-134">Kattintás után **ellenőrizze**, hello használata **frissítése** gombra, amíg hello **győződjön meg arról** beállítást meg kell jelennie a sikeres.</span><span class="sxs-lookup"><span data-stu-id="df502-134">After clicking **Verify**, use hello **Refresh** button until hello **Verify** option should show success.</span></span>

![Kép beszúrása a KV sikerességének ellenőrzése](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-tooapp-service-app"></a><span data-ttu-id="df502-136">5. lépés – tanúsítvány tooApp Service alkalmazás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="df502-136">Step 5 - Assign Certificate tooApp Service App</span></span>

> [!NOTE]
> <span data-ttu-id="df502-137">Ebben a szakaszban hello lépések elvégzése előtt kell társítva van egy egyéni tartománynevet az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="df502-137">Before performing hello steps in this section, you must have associated a custom domain name with your app.</span></span> <span data-ttu-id="df502-138">További információkért lásd:  **[egy webalkalmazást az egyéni tartománynév beállítása.](app-service-web-tutorial-custom-domain.md)**</span><span class="sxs-lookup"><span data-stu-id="df502-138">For more information, see **[Configuring a custom domain name for a web app.](app-service-web-tutorial-custom-domain.md)**</span></span>
>

<span data-ttu-id="df502-139">A hello  **[Azure-portálon](https://portal.azure.com/)**, hello kattintson **App Service** hello lap hello bal oldali lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="df502-139">In hello **[Azure portal](https://portal.azure.com/)**, click hello **App Service** option on hello left of hello page.</span></span>

<span data-ttu-id="df502-140">Hello neve kattintson az alkalmazás toowhich kívánt tooassign ezt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="df502-140">Click hello name of your app toowhich you want tooassign this certificate.</span></span>

<span data-ttu-id="df502-141">A hello **beállítások**, kattintson a **SSL-tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="df502-141">In hello **Settings**, click **SSL certificates**.</span></span>

<span data-ttu-id="df502-142">Kattintson a **App Service-tanúsítvány importálása** és select hello tanúsítvány csak megvásárolt.</span><span class="sxs-lookup"><span data-stu-id="df502-142">Click **Import App Service Certificate** and select hello certificate that you just purchased.</span></span>

![Helyezze be a tanúsítvány importálása képe](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

<span data-ttu-id="df502-144">A hello **ssl-kötések** szakaszban kattintson a **felvenni kötéseket**, és a hello legördülő listák megnyílásának tooselect hello tartomány neve toosecure használjon SSL, és a tanúsítvány toouse hello.</span><span class="sxs-lookup"><span data-stu-id="df502-144">In hello **ssl bindings** section Click on **Add bindings**, and use hello dropdowns tooselect hello domain name toosecure with SSL, and hello certificate toouse.</span></span> <span data-ttu-id="df502-145">Is kiválaszthat a e toouse  **[kiszolgálónév jelzése (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  vagy IP-alapú SSL.</span><span class="sxs-lookup"><span data-stu-id="df502-145">You may also select whether toouse **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP based SSL.</span></span>

![az SSL-kötések kép beszúrása](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

<span data-ttu-id="df502-147">Kattintson a **kötésének hozzáadása** toosave hello módosításokat, és engedélyezi az SSL.</span><span class="sxs-lookup"><span data-stu-id="df502-147">Click **Add Binding** toosave hello changes and enable SSL.</span></span>

> [!NOTE]
> <span data-ttu-id="df502-148">Ha a kiválasztott **IP-alapú SSL** és az egyéni tartomány úgy van konfigurálva, az A rekord, hello következő további lépéseket kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="df502-148">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform hello following additional steps.</span></span> <span data-ttu-id="df502-149">Ezek magyarázata hello részletesebben [szakasz speciális](#Advanced).</span><span class="sxs-lookup"><span data-stu-id="df502-149">These are explained in more details in hello [Advanced section](#Advanced).</span></span>

<span data-ttu-id="df502-150">Ekkor meg kell tudni toovisit az alkalmazás használatával `HTTPS://` helyett `HTTP://` tooverify, amely hello tanúsítvány megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="df502-150">At this point, you should be able toovisit your app using `HTTPS://` instead of `HTTP://` tooverify that hello certificate has been configured correctly.</span></span>

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a><span data-ttu-id="df502-151">6 - felügyeleti feladatokat. lépés</span><span class="sxs-lookup"><span data-stu-id="df502-151">Step 6 - Management tasks</span></span>

### <a name="azure-cli"></a><span data-ttu-id="df502-152">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="df502-152">Azure CLI</span></span>

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a><span data-ttu-id="df502-153">PowerShell</span><span class="sxs-lookup"><span data-stu-id="df502-153">PowerShell</span></span>

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a><span data-ttu-id="df502-154">Extra szintű</span><span class="sxs-lookup"><span data-stu-id="df502-154">Advanced</span></span>

### <a name="verifying-domain-ownership"></a><span data-ttu-id="df502-155">Tartomány-tulajdonjogának ellenőrzéséről</span><span class="sxs-lookup"><span data-stu-id="df502-155">Verifying Domain Ownership</span></span>

<span data-ttu-id="df502-156">App service-tanúsítványokkal által támogatott tartományok ellenőrzésének további két típusa van: Mail és a kézi ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="df502-156">There are two more types of domain verification supported by App service Certificates: Mail, and Manual Verification.</span></span>

#### <a name="mail-verification"></a><span data-ttu-id="df502-157">Mail ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="df502-157">Mail Verification</span></span>

<span data-ttu-id="df502-158">Megerősítési e-mailt már elküldte toohello az egyéni tartomány társított E-mail címe.</span><span class="sxs-lookup"><span data-stu-id="df502-158">Verification email has already been sent toohello Email Address(es) associated with this custom domain.</span></span>
<span data-ttu-id="df502-159">toocomplete hello E-mail ellenőrzési lépés, nyissa meg a hello e-mailt, és kattintson a hello megerősítési hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="df502-159">toocomplete hello Email verification step, open hello email and click hello verification link.</span></span>

![Helyezze be az e-mail ellenőrzése képe](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

<span data-ttu-id="df502-161">Ha tooresend hello megerősítési e-mailt, kattintson a hello **E-mail újraküldése** gombra.</span><span class="sxs-lookup"><span data-stu-id="df502-161">If you need tooresend hello verification email, click hello **Resend Email** button.</span></span>

#### <a name="manual-verification"></a><span data-ttu-id="df502-162">Kézi ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="df502-162">Manual Verification</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df502-163">HTML-weblap ellenőrzése (csak a Standard tanúsítvány Termékváltozat működik)</span><span class="sxs-lookup"><span data-stu-id="df502-163">HTML Web Page Verification (only works with Standard Certificate SKU)</span></span>
>

1. <span data-ttu-id="df502-164">Hozzon létre egy HTML fájlt **"starfield.html"**</span><span class="sxs-lookup"><span data-stu-id="df502-164">Create an HTML file named **"starfield.html"**</span></span>

1. <span data-ttu-id="df502-165">Tartalom fájl kell tartomány ellenőrző jogkivonat hello hello pontos nevét.</span><span class="sxs-lookup"><span data-stu-id="df502-165">Content of this file should be hello exact name of hello Domain Verification Token.</span></span> <span data-ttu-id="df502-166">(Átmásolhatja hello token hello tartomány ellenőrzési állapota panel)</span><span class="sxs-lookup"><span data-stu-id="df502-166">(You can copy hello token from hello Domain Verification Status Blade)</span></span>

1. <span data-ttu-id="df502-167">A feltöltés, a tartomány hello webkiszolgáló hello gyökérkönyvtárában`/.well-known/pki-validation/starfield.html`</span><span class="sxs-lookup"><span data-stu-id="df502-167">Upload this file at hello root of hello web server hosting your domain `/.well-known/pki-validation/starfield.html`</span></span>

1. <span data-ttu-id="df502-168">Kattintson a **frissítése** tooupdate hello tanúsítvány állapotának ellenőrzése után.</span><span class="sxs-lookup"><span data-stu-id="df502-168">Click **Refresh** tooupdate hello certificate status after verification is completed.</span></span> <span data-ttu-id="df502-169">Ellenőrzési toocomplete néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="df502-169">It might take few minutes for verification toocomplete.</span></span>

> [!TIP]
> <span data-ttu-id="df502-170">Ellenőrizze a terminál használatával `curl -G http://<domain>/.well-known/pki-validation/starfield.html` hello válasz tartalmaznia kell a hello `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="df502-170">Verify in a terminal using `curl -G http://<domain>/.well-known/pki-validation/starfield.html` hello response should contain hello `<verification-token>`.</span></span>

#### <a name="dns-txt-record-verification"></a><span data-ttu-id="df502-171">DNS TXT rekord ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="df502-171">DNS TXT Record Verification</span></span>

1. <span data-ttu-id="df502-172">A DNS-kezelő használatával hozzon létre egy TXT rekordot a hello `@` altartomány és értéke egyenlő toohello tartomány ellenőrző jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="df502-172">Using your DNS manager, Create a TXT record on hello `@` subdomain with value equal toohello Domain Verification Token.</span></span>
1. <span data-ttu-id="df502-173">Kattintson a **"Frissítés"** tooupdate hello tanúsítvány állapotának ellenőrzése után.</span><span class="sxs-lookup"><span data-stu-id="df502-173">Click **“Refresh”** tooupdate hello Certificate status after verification is completed.</span></span>

> [!TIP]
> <span data-ttu-id="df502-174">A TXT-rekord toocreate kell `@.<domain>` értékű `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="df502-174">You need toocreate a TXT record on `@.<domain>` with value `<verification-token>`.</span></span>

### <a name="assign-certificate-tooapp-service-app"></a><span data-ttu-id="df502-175">Tanúsítvány tooApp Service alkalmazás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="df502-175">Assign Certificate tooApp Service App</span></span>

<span data-ttu-id="df502-176">Ha a kiválasztott **IP-alapú SSL** és az egyéni tartomány úgy van konfigurálva, az A rekord, végre kell hajtania a következő további lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="df502-176">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform hello following additional steps:</span></span>

<span data-ttu-id="df502-177">Beállítása után egy IP-alapú SSL-kötés, dedikált IP-címnek a tooyour alkalmazás hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="df502-177">After you have configured an IP based SSL binding, a dedicated IP address is assigned tooyour app.</span></span> <span data-ttu-id="df502-178">Az IP-cím található hello **egyéni tartomány** az alkalmazás jobbra fent hello beállítások lapján **Hostnames** szakasz.</span><span class="sxs-lookup"><span data-stu-id="df502-178">You can find this IP address on hello **Custom domain** page under settings of your app, right above hello **Hostnames** section.</span></span> <span data-ttu-id="df502-179">Szerepel **külső IP-cím**</span><span class="sxs-lookup"><span data-stu-id="df502-179">It is listed as **External IP Address**</span></span>

![Helyezze be az IP-SSL képe](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

<span data-ttu-id="df502-181">Ne feledje, hogy az IP-cím más, mint a virtuális IP-cím hello korábban használt tooconfigure hello A rekord a tartomány.</span><span class="sxs-lookup"><span data-stu-id="df502-181">Note that this IP address is different than hello virtual IP address used previously tooconfigure hello A record for your domain.</span></span> <span data-ttu-id="df502-182">Ha konfigurált toouse SNI SSL-alapú, és nincsenek beállítva toouse SSL, akkor a bejegyzés nincs cím szerepel.</span><span class="sxs-lookup"><span data-stu-id="df502-182">If you are configured toouse SNI based SSL, or are not configured toouse SSL, no address is listed for this entry.</span></span>

<span data-ttu-id="df502-183">A regisztrációs által biztosított hello eszközöket használ, módosítsa a hello egy rekordot az egyéni tartomány nevét toopoint toohello IP-cím hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="df502-183">Using hello tools provided by your domain name registrar, modify hello A record for your custom domain name toopoint toohello IP address from hello previous step.</span></span>

## <a name="rekey-and-sync-hello-certificate"></a><span data-ttu-id="df502-184">Kulcs újbóli létrehozása és a szinkronizálási hello tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="df502-184">Rekey and Sync hello Certificate</span></span>

<span data-ttu-id="df502-185">Ha valaha is kell tooRekey a tanúsítvány, válasszon **kulcsismétlés és tényleges szinkronizálási** parancsát **tanúsítvány tulajdonságai** panelen.</span><span class="sxs-lookup"><span data-stu-id="df502-185">If you ever need tooRekey your certificate, select **Rekey and Sync** option from **Certificate Properties** Blade.</span></span>

<span data-ttu-id="df502-186">Kattintson a **kulcsismétlés** tooinitiate hello folyamat gombra.</span><span class="sxs-lookup"><span data-stu-id="df502-186">Click **Rekey** Button tooinitiate hello process.</span></span> <span data-ttu-id="df502-187">1 – 10 percig toocomplete vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="df502-187">This process can take 1-10 minutes toocomplete.</span></span>

![Helyezze be a kulcsismétlés SSL képe](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

<span data-ttu-id="df502-189">A tanúsítvány újbóli visszaállítja a hello tanúsítvány hello hitelesítésszolgáltatótól származó kibocsátott új tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="df502-189">Rekeying your certificate rolls hello certificate with a new certificate issued from hello certificate authority.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df502-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df502-190">Next Steps</span></span>

* [<span data-ttu-id="df502-191">Egy Tartalomkézbesítési hálózat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="df502-191">Add a Content Delivery Network</span></span>](app-service-web-tutorial-content-delivery-network.md)