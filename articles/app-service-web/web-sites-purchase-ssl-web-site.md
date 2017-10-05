---
title: "Az SSL-tanúsítvány hozzáadása az Azure App Service alkalmazáshoz |} Microsoft Docs"
description: "Tudnivalók az SSL-tanúsítvány hozzáadása az App Service alkalmazáshoz."
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
ms.openlocfilehash: 191dd7240ad15b4936a72bc27a2d0162350f3afb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a><span data-ttu-id="65a44-103">SSL-tanúsítvány vásárlása és konfigurálása saját Azure App Service szolgáltatások számára</span><span class="sxs-lookup"><span data-stu-id="65a44-103">Buy and Configure an SSL Certificate for your Azure App Service</span></span>

<span data-ttu-id="65a44-104">Ebben az oktatóanyagban az SSL-tanúsítvány megvásárlásával fog biztonságos a webalkalmazás a  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, biztonságos helyen tárolja [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), és való társítás egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="65a44-104">In this tutorial, you will secure your web app by purchasing an SSL certificate for your **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, securely storing it in [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), and associating it with a custom domain.</span></span>

## <a name="step-1---log-in-to-azure"></a><span data-ttu-id="65a44-105">1. lépés – Azure való bejelentkezéshez</span><span class="sxs-lookup"><span data-stu-id="65a44-105">Step 1 - Log in to Azure</span></span>

<span data-ttu-id="65a44-106">Jelentkezzen be az Azure portálon, a http://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="65a44-106">Log in to the Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---place-an-ssl-certificate-order"></a><span data-ttu-id="65a44-107">2. lépés - az SSL-tanúsítvány megrendelés</span><span class="sxs-lookup"><span data-stu-id="65a44-107">Step 2 - Place an SSL Certificate order</span></span>

<span data-ttu-id="65a44-108">Hozzon létre egy új SSL-tanúsítvány rendelés elhelyezhet [App szolgáltatástanúsítvány](https://portal.azure.com/#create/Microsoft.SSL) a a **Azure-portálon**.</span><span class="sxs-lookup"><span data-stu-id="65a44-108">You can place an SSL Certificate order by creating a new [App Service Certificate](https://portal.azure.com/#create/Microsoft.SSL) In the **Azure portal**.</span></span>

![Tanúsítvány létrehozása](./media/app-service-web-purchase-ssl-web-site/createssl.png)

<span data-ttu-id="65a44-110">Adjon meg egy rövid **neve** SSL-tanúsítványok a tanúsítványok, és írja be a **tartománynév**</span><span class="sxs-lookup"><span data-stu-id="65a44-110">Enter a friendly **Name** for your SSL certificate and enter the **Domain Name**</span></span>

> [!NOTE]
> <span data-ttu-id="65a44-111">Ez az egyik legfontosabb részeit a vásárlási eljárást.</span><span class="sxs-lookup"><span data-stu-id="65a44-111">This is one of the most critical parts of the purchase process.</span></span> <span data-ttu-id="65a44-112">Győződjön meg arról, hogy ezt a tanúsítványt a védeni kívánt megfelelő gazdagép nevét (egyéni tartomány).</span><span class="sxs-lookup"><span data-stu-id="65a44-112">Make sure to enter correct host name (custom domain) that you want to protect with this certificate.</span></span> <span data-ttu-id="65a44-113">**NE** az állomásnév WWW rendelkező hozzáfűzése.</span><span class="sxs-lookup"><span data-stu-id="65a44-113">**DO NOT** append the Host name with WWW.</span></span> 
>

<span data-ttu-id="65a44-114">Válassza ki a **előfizetés**, **erőforráscsoport**, és **tanúsítvány-SKU**</span><span class="sxs-lookup"><span data-stu-id="65a44-114">Select your **Subscription**, **Resource Group**, and **Certificate SKU**</span></span>

> [!WARNING]
> <span data-ttu-id="65a44-115">App Service-tanúsítványok csak egyazon előfizetésen belül más alkalmazásszolgáltatások használható.</span><span class="sxs-lookup"><span data-stu-id="65a44-115">App Service Certificates can only be used by other App Services within the same subscription.</span></span>  
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a><span data-ttu-id="65a44-116">3. lépés - a tanúsítványt az Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="65a44-116">Step 3 - Store the certificate in Azure Key Vault</span></span>

> [!NOTE]
> <span data-ttu-id="65a44-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) Azure szolgáltatás, amely segít a felhőalapú alkalmazások és szolgáltatások által használt titkosítási kulcsok és titkos védelme.</span><span class="sxs-lookup"><span data-stu-id="65a44-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) is an Azure service that helps safeguard cryptographic keys and secrets used by cloud applications and services.</span></span>
>

<span data-ttu-id="65a44-118">Az SSL-tanúsítvány a vásárlás befejezése után meg kell nyitnia [App Service-tanúsítványokkal](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) erőforráspanelen.</span><span class="sxs-lookup"><span data-stu-id="65a44-118">Once the SSL Certificate purchase is complete, you need to open [App Service Certificates](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) Resource blade.</span></span>

![Helyezze be a készen áll a KV tárolása képe](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

<span data-ttu-id="65a44-120">Megfigyelheti, hogy van-e a tanúsítvány állapotának **"Függőben lévő kiállítási"** , néhány további lépést kell végrehajtani, ezzel a tanúsítvánnyal megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="65a44-120">You will notice that Certificate status is **“Pending Issuance”** as there are few more steps you need to complete before you can start using this certificate.</span></span>

<span data-ttu-id="65a44-121">Kattintson a **Tanúsítványkonfiguráció** belül tanúsítvány tulajdonságai panelen, majd kattintson a **1. lépés: tárolására** Azure Key Vault ezt a tanúsítványt tárolni.</span><span class="sxs-lookup"><span data-stu-id="65a44-121">Click **Certificate Configuration** inside Certificate Properties blade and Click on **Step 1: Store** to store this certificate in Azure Key Vault.</span></span>

<span data-ttu-id="65a44-122">A **Key Vault állapot** panelen kattintson a **Key Vault tárház** választania ezt a tanúsítványt tárolni egy meglévő kulcstároló **vagy hozzon létre új Key Vault** azonos előfizetésbe és erőforráscsoportba csoporton belül kulcstároló létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="65a44-122">From **Key Vault Status** Blade, click **Key Vault Repository** to choose an existing Key Vault to store this certificate **OR Create New Key Vault** to create new Key Vault inside same subscription and resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="65a44-123">Az Azure Key Vault tárolja a tanúsítványt a minimális díja van.</span><span class="sxs-lookup"><span data-stu-id="65a44-123">Azure Key Vault has minimal charges for storing this certificate.</span></span>
> <span data-ttu-id="65a44-124">További információkért lásd:  **[Azure Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/)**.</span><span class="sxs-lookup"><span data-stu-id="65a44-124">For more information, see **[Azure Key Vault Pricing Details](https://azure.microsoft.com/pricing/details/key-vault/)**.</span></span>
>

<span data-ttu-id="65a44-125">Miután kiválasztotta a kulcsot tároló tárház tárolja ezt a tanúsítványt, a **tárolására** beállítást meg kell jelennie a sikeres.</span><span class="sxs-lookup"><span data-stu-id="65a44-125">Once you have selected the Key Vault Repository to store this certificate in, the **Store** option should show success.</span></span>

![Helyezze be a tároló sikeres KV képe](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a><span data-ttu-id="65a44-127">4. lépés - ellenőrizze a tartomány tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="65a44-127">Step 4 - Verify the Domain Ownership</span></span>

> [!NOTE]
> <span data-ttu-id="65a44-128">App service-tanúsítványokkal által támogatott tartományok ellenőrzésének három típusa van: tartomány, E-mail, manuális ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="65a44-128">There are three types of domain verification supported by App service Certificates: Domain, Mail, Manual Verification.</span></span> <span data-ttu-id="65a44-129">Ezek a további részleteket magyarázata a [szakasz speciális](#advanced).</span><span class="sxs-lookup"><span data-stu-id="65a44-129">These are explained in more details in the [Advanced section](#advanced).</span></span>

<span data-ttu-id="65a44-130">Az azonos **Tanúsítványkonfiguráció** panel használja a 3. lépésben, kattintson a **2. lépés: Ellenőrizze**.</span><span class="sxs-lookup"><span data-stu-id="65a44-130">From the same **Certificate Configuration** blade you used in Step 3, click **Step 2: Verify**.</span></span>

<span data-ttu-id="65a44-131">**Tartományok ellenőrzésének** legkényelmesebben folyamat **csak ha** rendelkezik  **[az Azure App Service szolgáltatásban az egyéni tartomány vásárolt.](custom-dns-web-site-buydomains-web-app.md)**</span><span class="sxs-lookup"><span data-stu-id="65a44-131">**Domain Verification** This is the most convenient process **ONLY IF** you have **[purchased your custom domain from Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span></span>
<span data-ttu-id="65a44-132">Kattintson a **ellenőrizze** gombra kattintva fejezze be ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="65a44-132">Click on **Verify** button to complete this step.</span></span>

![Helyezze be a tartomány ellenőrzése képe](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

<span data-ttu-id="65a44-134">Kattintás után **ellenőrizze**, használja a **frissítése** gombra kattint, amíg a **győződjön meg arról** beállítást meg kell jelennie a sikeres.</span><span class="sxs-lookup"><span data-stu-id="65a44-134">After clicking **Verify**, use the **Refresh** button until the **Verify** option should show success.</span></span>

![Kép beszúrása a KV sikerességének ellenőrzése](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a><span data-ttu-id="65a44-136">App Service alkalmazás hozzárendelése tanúsítványt, 5 -. lépés</span><span class="sxs-lookup"><span data-stu-id="65a44-136">Step 5 - Assign Certificate to App Service App</span></span>

> [!NOTE]
> <span data-ttu-id="65a44-137">Ebben a szakaszban a lépések elvégzése előtt kell társítva van egy egyéni tartománynevet az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="65a44-137">Before performing the steps in this section, you must have associated a custom domain name with your app.</span></span> <span data-ttu-id="65a44-138">További információkért lásd:  **[egy webalkalmazást az egyéni tartománynév beállítása.](app-service-web-tutorial-custom-domain.md)**</span><span class="sxs-lookup"><span data-stu-id="65a44-138">For more information, see **[Configuring a custom domain name for a web app.](app-service-web-tutorial-custom-domain.md)**</span></span>
>

<span data-ttu-id="65a44-139">Az a  **[Azure-portálon](https://portal.azure.com/)**, kattintson a **App Service** lehetőséget a bal oldali a lap.</span><span class="sxs-lookup"><span data-stu-id="65a44-139">In the **[Azure portal](https://portal.azure.com/)**, click the **App Service** option on the left of the page.</span></span>

<span data-ttu-id="65a44-140">Kattintson annak az alkalmazásnak a nevére, amelyhez hozzá szeretné rendelni a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="65a44-140">Click the name of your app to which you want to assign this certificate.</span></span>

<span data-ttu-id="65a44-141">Az a **beállítások**, kattintson a **SSL-tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="65a44-141">In the **Settings**, click **SSL certificates**.</span></span>

<span data-ttu-id="65a44-142">Kattintson a **App Service-tanúsítvány importálása** , és válassza ki az imént beszerzett tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="65a44-142">Click **Import App Service Certificate** and select the certificate that you just purchased.</span></span>

![Helyezze be a tanúsítvány importálása képe](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

<span data-ttu-id="65a44-144">Az a **ssl-kötések** szakaszban kattintson a **felvenni kötéseket**, és a legördülő lista segítségével válassza ki a biztonságos SSL és a tanúsítvány használatára a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="65a44-144">In the **ssl bindings** section Click on **Add bindings**, and use the dropdowns to select the domain name to secure with SSL, and the certificate to use.</span></span> <span data-ttu-id="65a44-145">Is kiválaszthat a használ-e  **[kiszolgálónév jelzése (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  vagy IP-alapú SSL.</span><span class="sxs-lookup"><span data-stu-id="65a44-145">You may also select whether to use **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP based SSL.</span></span>

![az SSL-kötések kép beszúrása](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

<span data-ttu-id="65a44-147">Kattintson a **kötésének hozzáadása** menti a módosításokat, és az SSL engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="65a44-147">Click **Add Binding** to save the changes and enable SSL.</span></span>

> [!NOTE]
> <span data-ttu-id="65a44-148">Ha a kiválasztott **IP-alapú SSL** és az egyéni tartomány úgy van konfigurálva, az A rekord, a következő további lépéseket kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="65a44-148">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform the following additional steps.</span></span> <span data-ttu-id="65a44-149">Ezek a további részleteket magyarázata a [szakasz speciális](#Advanced).</span><span class="sxs-lookup"><span data-stu-id="65a44-149">These are explained in more details in the [Advanced section](#Advanced).</span></span>

<span data-ttu-id="65a44-150">Ezen a ponton kell tudni látogasson el az alkalmazás használatával `HTTPS://` helyett `HTTP://` ellenőrzése, hogy a tanúsítvány helyesen van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="65a44-150">At this point, you should be able to visit your app using `HTTPS://` instead of `HTTP://` to verify that the certificate has been configured correctly.</span></span>

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a><span data-ttu-id="65a44-151">6 - felügyeleti feladatokat. lépés</span><span class="sxs-lookup"><span data-stu-id="65a44-151">Step 6 - Management tasks</span></span>

### <a name="azure-cli"></a><span data-ttu-id="65a44-152">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="65a44-152">Azure CLI</span></span>

<span data-ttu-id="65a44-153">[!code-azurecli[fő](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "egyéni SSL-tanúsítvány kötése a webes alkalmazás")]</span><span class="sxs-lookup"><span data-stu-id="65a44-153">[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]</span></span> 

### <a name="powershell"></a><span data-ttu-id="65a44-154">PowerShell</span><span class="sxs-lookup"><span data-stu-id="65a44-154">PowerShell</span></span>

<span data-ttu-id="65a44-155">[!code-powershell[fő](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "egyéni SSL-tanúsítvány kötése a webes alkalmazás")]</span><span class="sxs-lookup"><span data-stu-id="65a44-155">[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]</span></span>

## <a name="advanced"></a><span data-ttu-id="65a44-156">Extra szintű</span><span class="sxs-lookup"><span data-stu-id="65a44-156">Advanced</span></span>

### <a name="verifying-domain-ownership"></a><span data-ttu-id="65a44-157">Tartomány-tulajdonjogának ellenőrzéséről</span><span class="sxs-lookup"><span data-stu-id="65a44-157">Verifying Domain Ownership</span></span>

<span data-ttu-id="65a44-158">App service-tanúsítványokkal által támogatott tartományok ellenőrzésének további két típusa van: Mail és a kézi ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="65a44-158">There are two more types of domain verification supported by App service Certificates: Mail, and Manual Verification.</span></span>

#### <a name="mail-verification"></a><span data-ttu-id="65a44-159">Mail ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="65a44-159">Mail Verification</span></span>

<span data-ttu-id="65a44-160">Megerősítési e-mailt már az E-mail címeket, az egyéni tartomány társított el lett küldve.</span><span class="sxs-lookup"><span data-stu-id="65a44-160">Verification email has already been sent to the Email Address(es) associated with this custom domain.</span></span>
<span data-ttu-id="65a44-161">Végezze el az e-mailek ellenőrzési lépés, nyissa meg az e-mailt, és kattintson a megerősítési hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="65a44-161">To complete the Email verification step, open the email and click the verification link.</span></span>

![Helyezze be az e-mail ellenőrzése képe](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

<span data-ttu-id="65a44-163">Ha újra kell küldenie a megerősítési e-mailt, kattintson a **E-mail újraküldése** gombra.</span><span class="sxs-lookup"><span data-stu-id="65a44-163">If you need to resend the verification email, click the **Resend Email** button.</span></span>

#### <a name="manual-verification"></a><span data-ttu-id="65a44-164">Kézi ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="65a44-164">Manual Verification</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65a44-165">HTML-weblap ellenőrzése (csak a Standard tanúsítvány Termékváltozat működik)</span><span class="sxs-lookup"><span data-stu-id="65a44-165">HTML Web Page Verification (only works with Standard Certificate SKU)</span></span>
>

1. <span data-ttu-id="65a44-166">Hozzon létre egy HTML fájlt **"starfield.html"**</span><span class="sxs-lookup"><span data-stu-id="65a44-166">Create an HTML file named **"starfield.html"**</span></span>

1. <span data-ttu-id="65a44-167">Tartalom ennek a fájlnak kell lennie a tartomány ellenőrző jogkivonat pontos nevét.</span><span class="sxs-lookup"><span data-stu-id="65a44-167">Content of this file should be the exact name of the Domain Verification Token.</span></span> <span data-ttu-id="65a44-168">(A másoláshoz a jogkivonatot a tartomány ellenőrzése állapot paneljéről)</span><span class="sxs-lookup"><span data-stu-id="65a44-168">(You can copy the token from the Domain Verification Status Blade)</span></span>

1. <span data-ttu-id="65a44-169">Ez a fájl a webkiszolgálón, a tartomány gyökerében feltöltése`/.well-known/pki-validation/starfield.html`</span><span class="sxs-lookup"><span data-stu-id="65a44-169">Upload this file at the root of the web server hosting your domain `/.well-known/pki-validation/starfield.html`</span></span>

1. <span data-ttu-id="65a44-170">Kattintson a **frissítése** ellenőrzés után a tanúsítvány állapotának frissítése.</span><span class="sxs-lookup"><span data-stu-id="65a44-170">Click **Refresh** to update the certificate status after verification is completed.</span></span> <span data-ttu-id="65a44-171">Az ellenőrzés befejezéséhez néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="65a44-171">It might take few minutes for verification to complete.</span></span>

> [!TIP]
> <span data-ttu-id="65a44-172">Ellenőrizze a terminál használatával `curl -G http://<domain>/.well-known/pki-validation/starfield.html` a válasz tartalmaznia kell a `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="65a44-172">Verify in a terminal using `curl -G http://<domain>/.well-known/pki-validation/starfield.html` the response should contain the `<verification-token>`.</span></span>

#### <a name="dns-txt-record-verification"></a><span data-ttu-id="65a44-173">DNS TXT rekord ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="65a44-173">DNS TXT Record Verification</span></span>

1. <span data-ttu-id="65a44-174">A DNS-kezelő használatával hozzon létre egy TXT rekordot a a `@` altartomány értékkel egyenlőnek a tartomány ellenőrző jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="65a44-174">Using your DNS manager, Create a TXT record on the `@` subdomain with value equal to the Domain Verification Token.</span></span>
1. <span data-ttu-id="65a44-175">Kattintson a **"Frissítés"** ellenőrzés után a tanúsítvány állapotának frissítése.</span><span class="sxs-lookup"><span data-stu-id="65a44-175">Click **“Refresh”** to update the Certificate status after verification is completed.</span></span>

> [!TIP]
> <span data-ttu-id="65a44-176">A TXT-rekord létrehozásához szükséges `@.<domain>` értékű `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="65a44-176">You need to create a TXT record on `@.<domain>` with value `<verification-token>`.</span></span>

### <a name="assign-certificate-to-app-service-app"></a><span data-ttu-id="65a44-177">App Service alkalmazás-tanúsítványt hozzárendeli</span><span class="sxs-lookup"><span data-stu-id="65a44-177">Assign Certificate to App Service App</span></span>

<span data-ttu-id="65a44-178">Ha a kiválasztott **IP-alapú SSL** és az egyéni tartomány úgy van konfigurálva, az A rekord, a következő további lépéseket kell végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="65a44-178">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform the following additional steps:</span></span>

<span data-ttu-id="65a44-179">Beállítása után egy IP-alapú SSL-kötést, egy dedikált IP-cím hozzá van rendelve az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="65a44-179">After you have configured an IP based SSL binding, a dedicated IP address is assigned to your app.</span></span> <span data-ttu-id="65a44-180">Az IP-cím található a **egyéni tartomány** jobbra fent a az alkalmazás beállításai lapon a **Hostnames** szakasz.</span><span class="sxs-lookup"><span data-stu-id="65a44-180">You can find this IP address on the **Custom domain** page under settings of your app, right above the **Hostnames** section.</span></span> <span data-ttu-id="65a44-181">Szerepel **külső IP-cím**</span><span class="sxs-lookup"><span data-stu-id="65a44-181">It is listed as **External IP Address**</span></span>

![Helyezze be az IP-SSL képe](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

<span data-ttu-id="65a44-183">Vegye figyelembe, hogy az IP-cím nem egyezik a virtuális IP-cím a tartomány az A rekord konfigurálása korábban használt.</span><span class="sxs-lookup"><span data-stu-id="65a44-183">Note that this IP address is different than the virtual IP address used previously to configure the A record for your domain.</span></span> <span data-ttu-id="65a44-184">Ha használatára van konfigurálva SNI SSL-alapú vagy nem SSL használatára vannak konfigurálva, a bejegyzés nincs cím szerepel.</span><span class="sxs-lookup"><span data-stu-id="65a44-184">If you are configured to use SNI based SSL, or are not configured to use SSL, no address is listed for this entry.</span></span>

<span data-ttu-id="65a44-185">A regisztrációs által biztosított eszközöket használja, módosítsa az egyéni tartománynevet az IP-címre az előző lépésben rekordjában.</span><span class="sxs-lookup"><span data-stu-id="65a44-185">Using the tools provided by your domain name registrar, modify the A record for your custom domain name to point to the IP address from the previous step.</span></span>

## <a name="rekey-and-sync-the-certificate"></a><span data-ttu-id="65a44-186">Kulcsismétlés és a tanúsítványának szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="65a44-186">Rekey and Sync the Certificate</span></span>

<span data-ttu-id="65a44-187">Ha bármikor át kívánja kulcsismétlés a tanúsítvány, válasszon **kulcsismétlés és tényleges szinkronizálási** parancsát **tanúsítvány tulajdonságai** panelen.</span><span class="sxs-lookup"><span data-stu-id="65a44-187">If you ever need to Rekey your certificate, select **Rekey and Sync** option from **Certificate Properties** Blade.</span></span>

<span data-ttu-id="65a44-188">Kattintson a **kulcsismétlés** gombra kattintva indítani a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="65a44-188">Click **Rekey** Button to initiate the process.</span></span> <span data-ttu-id="65a44-189">Ez az eljárás 1 – 10 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="65a44-189">This process can take 1-10 minutes to complete.</span></span>

![Helyezze be a kulcsismétlés SSL képe](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

<span data-ttu-id="65a44-191">A tanúsítvány újbóli visszaállítja a tanúsítványt a hitelesítésszolgáltatótól származó kibocsátott új tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="65a44-191">Rekeying your certificate rolls the certificate with a new certificate issued from the certificate authority.</span></span>

## <a name="next-steps"></a><span data-ttu-id="65a44-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65a44-192">Next Steps</span></span>

* [<span data-ttu-id="65a44-193">Egy Tartalomkézbesítési hálózat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="65a44-193">Add a Content Delivery Network</span></span>](app-service-web-tutorial-content-delivery-network.md)