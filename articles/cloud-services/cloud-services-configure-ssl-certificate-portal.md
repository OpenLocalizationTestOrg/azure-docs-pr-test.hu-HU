---
title: "Az SSL konfigurálása egy felhőalapú szolgáltatás |} Microsoft Docs"
description: "Útmutató: a webes szerepkör HTTPS-végpontnak megadása és az alkalmazás biztonságos SSL-tanúsítvány feltöltése. Ezekben a példákban az Azure-portálon."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 371ba204-48b6-41af-ab9f-ed1d64efe704
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: e5c8c3b098772c0586712305a577b24a6f0d924c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="d2e05-104">SSL konfigurálása az alkalmazás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="d2e05-104">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d2e05-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d2e05-105">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="d2e05-106">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="d2e05-106">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
>

<span data-ttu-id="d2e05-107">Az SSL-alapú titkosítás a leggyakrabban használt módszer az interneten keresztül küldött adatok biztonságossá tételéhez.</span><span class="sxs-lookup"><span data-stu-id="d2e05-107">Secure Socket Layer (SSL) encryption is the most commonly used method of securing data sent across the internet.</span></span> <span data-ttu-id="d2e05-108">Ebben az általános feladatban azt fejtjük ki, hogy miként lehet HTTPS-végpontokat meghatározni webes szerepkörökhöz, és hogyan tölthetők fel SSL-tanúsítványok az alkalmazások biztonságossá tételéhez.</span><span class="sxs-lookup"><span data-stu-id="d2e05-108">This common task discusses how to specify an HTTPS endpoint for a web role and how to upload an SSL certificate to secure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="d2e05-109">Ebben a feladatban eljárásokat alkalmazni az Azure Felhőszolgáltatások; alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="d2e05-109">The procedures in this task apply to Azure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md).</span></span>
>

<span data-ttu-id="d2e05-110">Ez a feladat használja az éles környezet.</span><span class="sxs-lookup"><span data-stu-id="d2e05-110">This task uses a production deployment.</span></span> <span data-ttu-id="d2e05-111">Az átmeneti központi telepítéssel információt a témakör végén.</span><span class="sxs-lookup"><span data-stu-id="d2e05-111">Information on using a staging deployment is provided at the end of this topic.</span></span>

<span data-ttu-id="d2e05-112">Olvasási [ez](cloud-services-how-to-create-deploy-portal.md) első, ha egy felhőalapú szolgáltatás még nem létrehozott.</span><span class="sxs-lookup"><span data-stu-id="d2e05-112">Read [this](cloud-services-how-to-create-deploy-portal.md) first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="d2e05-113">1. lépés: SSL-tanúsítvány beszerzése</span><span class="sxs-lookup"><span data-stu-id="d2e05-113">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="d2e05-114">Az SSL konfigurálása az alkalmazáshoz, akkor először kell, amely által egy tanúsítvány (CA), a megbízható tanúsítványokat állít ki erre a célra harmadik fél aláírt SSL-tanúsítvány beszerzése.</span><span class="sxs-lookup"><span data-stu-id="d2e05-114">To configure SSL for an application, you first need to get an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="d2e05-115">Ha még nem rendelkezik egy, meg kell szereznie egy SSL-tanúsítványok értékesít vállalat közül.</span><span class="sxs-lookup"><span data-stu-id="d2e05-115">If you do not already have one, you need to obtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="d2e05-116">A tanúsítványt az SSL-tanúsítványok az Azure-ban az alábbi követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="d2e05-116">The certificate must meet the following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="d2e05-117">A tanúsítványnak tartalmaznia kell egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="d2e05-117">The certificate must contain a private key.</span></span>
* <span data-ttu-id="d2e05-118">A kulcscseréhez használt, a személyes információcsere (.pfx) fájl exportálható léteznie kell a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d2e05-118">The certificate must be created for key exchange, exportable to a Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="d2e05-119">A tanúsítvány tulajdonosának nevét meg kell egyeznie a felhőalapú szolgáltatás eléréséhez használt tartomány.</span><span class="sxs-lookup"><span data-stu-id="d2e05-119">The certificate's subject name must match the domain used to access the cloud service.</span></span> <span data-ttu-id="d2e05-120">Az SSL-tanúsítvány nem lehet megszerezni a cloudapp.net tartomány hitelesítésszolgáltatótól (CA) származó.</span><span class="sxs-lookup"><span data-stu-id="d2e05-120">You cannot obtain an SSL certificate from a certificate authority (CA) for the cloudapp.net domain.</span></span> <span data-ttu-id="d2e05-121">Egy egyéni tartománynevet használhat, amikor be kell szerezni a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="d2e05-121">You must acquire a custom domain name to use when access your service.</span></span> <span data-ttu-id="d2e05-122">A hitelesítésszolgáltató tanúsítvány kérése, amikor a tanúsítvány tulajdonosának nevét meg kell egyeznie az egyéni tartománynév, az alkalmazás eléréséhez használt.</span><span class="sxs-lookup"><span data-stu-id="d2e05-122">When you request a certificate from a CA, the certificate's subject name must match the custom domain name used to access your application.</span></span> <span data-ttu-id="d2e05-123">Például, ha az egyéni tartománynevet **contoso.com** tanúsítványt kérhet tenné a hitelesítésszolgáltató a ***. contoso.com** vagy **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="d2e05-123">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="d2e05-124">A tanúsítvány legalább 2048 bites titkosítást kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d2e05-124">The certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="d2e05-125">Tesztelési célokra is [létrehozása](cloud-services-certs-create.md) és egy önaláírt tanúsítványt használjon.</span><span class="sxs-lookup"><span data-stu-id="d2e05-125">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="d2e05-126">Egy önaláírt tanúsítványt nem lehet hitelesíteni a hitelesítésszolgáltató használatával, és a cloudapp.net tartomány használható a webhely URL-címe.</span><span class="sxs-lookup"><span data-stu-id="d2e05-126">A self-signed certificate is not authenticated through a CA and can use the cloudapp.net domain as the website URL.</span></span> <span data-ttu-id="d2e05-127">Például a következő feladatot használ egy önaláírt tanúsítványt, amelyben a tanúsítványban használt köznapi név (CN) van **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="d2e05-127">For example, the following task uses a self-signed certificate in which the common name (CN) used in the certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="d2e05-128">A következő szerepelnie kell a tanúsítvány adatait a szolgáltatás definíciós és konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="d2e05-128">Next, you must include information about the certificate in your service definition and service configuration files.</span></span>

<span data-ttu-id="d2e05-129"><a name="modify"> </a></span><span class="sxs-lookup"><span data-stu-id="d2e05-129"><a name="modify"> </a></span></span>

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a><span data-ttu-id="d2e05-130">2. lépés: A definíció- és konfigurációs fájljainak módosítása</span><span class="sxs-lookup"><span data-stu-id="d2e05-130">Step 2: Modify the service definition and configuration files</span></span>
<span data-ttu-id="d2e05-131">Az alkalmazás be kell állítani a tanúsítványt használja, és a HTTPS-végpontnak kell hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="d2e05-131">Your application must be configured to use the certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="d2e05-132">Ennek eredményeképpen a szolgáltatásdefiníció és konfigurációs fájlok frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="d2e05-132">As a result, the service definition and service configuration files need to be updated.</span></span>

1. <span data-ttu-id="d2e05-133">A fejlesztői környezetben nyissa meg a szolgáltatásdefiníciós fájlban (CSDEF), adja hozzá a **tanúsítványok** belül szakasz a **webrole típusról** szakaszt, és a következő információkat tartalmaznak a tanúsítvány (és köztes tanúsítványokat):</span><span class="sxs-lookup"><span data-stu-id="d2e05-133">In your development environment, open the service definition file (CSDEF), add a **Certificates** section within the **WebRole** section, and include the following information about the certificate (and intermediate certificates):</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by the CA root, you
            must include all the intermediate certificates
            here. You must list them here, even if they are
            not bound to any endpoints. Failing to list any of
            the intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```

   <span data-ttu-id="d2e05-134">A **tanúsítványok** szakasz határozza meg a nevét, valamint a tanúsítvány, helyére, a tárolási helyétől nevét.</span><span class="sxs-lookup"><span data-stu-id="d2e05-134">The **Certificates** section defines the name of our certificate, its location, and the name of the store where it is located.</span></span>

   <span data-ttu-id="d2e05-135">Engedélyek (`permisionLevel` attribútum) a következő értékek egyikére állítható be:</span><span class="sxs-lookup"><span data-stu-id="d2e05-135">Permissions (`permisionLevel` attribute) can be set to one of the following values:</span></span>

   | <span data-ttu-id="d2e05-136">Engedély érték</span><span class="sxs-lookup"><span data-stu-id="d2e05-136">Permission Value</span></span> | <span data-ttu-id="d2e05-137">Leírás</span><span class="sxs-lookup"><span data-stu-id="d2e05-137">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="d2e05-138">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="d2e05-138">limitedOrElevated</span></span> |<span data-ttu-id="d2e05-139">**(Alapértelmezett)**  Összes szerepkör folyamat hozzáférhet a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d2e05-139">**(Default)** All role processes can access the private key.</span></span> |
   | <span data-ttu-id="d2e05-140">emelt szintű</span><span class="sxs-lookup"><span data-stu-id="d2e05-140">elevated</span></span> |<span data-ttu-id="d2e05-141">Csak a rendszergazda jogú folyamatok férhetnek hozzá a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d2e05-141">Only elevated processes can access the private key.</span></span> |

2. <span data-ttu-id="d2e05-142">A szolgáltatásdefiníciós fájlban, adja hozzá egy **bemeneti végponthoz** elemet a **végpontok** szakaszt, hogy HTTPS:</span><span class="sxs-lookup"><span data-stu-id="d2e05-142">In your service definition file, add an **InputEndpoint** element within the **Endpoints** section to enable HTTPS:</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443"
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. <span data-ttu-id="d2e05-143">A szolgáltatásdefiníciós fájlban, vegye fel a **kötés** elemet a **helyek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d2e05-143">In your service definition file, add a **Binding** element within the **Sites** section.</span></span> <span data-ttu-id="d2e05-144">Ez az elem egy HTTPS-kötést a végpont hozzárendelése a webhely hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="d2e05-144">This element adds an HTTPS binding to map the endpoint to your site:</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```

   <span data-ttu-id="d2e05-145">A szükséges módosításokat a szolgáltatásdefiníciós fájlban befejeződtek; azonban továbbra is szeretné az tanúsítvány-adatokat hozzáadni a szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="d2e05-145">All the required changes to the service definition file have been completed; but, you still need to add the certificate information to the service configuration file.</span></span>
4. <span data-ttu-id="d2e05-146">A szolgáltatás konfigurációs fájljában (a szolgáltatáskonfigurációs SÉMA), ServiceConfiguration.Cloud.cscfg, vegye fel a **tanúsítványok** értéket, amely a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="d2e05-146">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** value with that of your certificate.</span></span> <span data-ttu-id="d2e05-147">A következő példakód ismerteti a **tanúsítványok** szakaszban, kivéve az ujjlenyomat értékét.</span><span class="sxs-lookup"><span data-stu-id="d2e05-147">The following code sample provides details of the **Certificates** section, except for the thumbprint value.</span></span>

   ```xml
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff"
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc"
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

<span data-ttu-id="d2e05-148">(A példában az **sha1** ujjlenyomat algoritmust.</span><span class="sxs-lookup"><span data-stu-id="d2e05-148">(This example uses **sha1** for the thumbprint algorithm.</span></span> <span data-ttu-id="d2e05-149">Adja meg a megfelelő értéket a tanúsítvány ujjlenyomata algoritmushoz.)</span><span class="sxs-lookup"><span data-stu-id="d2e05-149">Specify the appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="d2e05-150">Most, hogy a szolgáltatás definíció- és konfigurációs fájlok frissültek, csomag a központi telepítés feltöltése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="d2e05-150">Now that the service definition and service configuration files have been updated, package your deployment for uploading to Azure.</span></span> <span data-ttu-id="d2e05-151">Ha használ **cspack**, ne használjon a **/generateConfigurationFile** jelzőt, mivel, amelyek felülírják az imént beillesztett tanúsítvány adatait.</span><span class="sxs-lookup"><span data-stu-id="d2e05-151">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that will overwrite the certificate information you just inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="d2e05-152">3. lépés: A tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="d2e05-152">Step 3: Upload a certificate</span></span>
<span data-ttu-id="d2e05-153">Az Azure-portál csatlakozni és...</span><span class="sxs-lookup"><span data-stu-id="d2e05-153">Connect to the Azure portal and...</span></span>

1. <span data-ttu-id="d2e05-154">Az a **összes erőforrás** szakasz a portált, válassza ki a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d2e05-154">In the **All resources** section of the Portal, select your cloud service.</span></span>

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. <span data-ttu-id="d2e05-156">Kattintson a **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="d2e05-156">Click **Certificates**.</span></span>

    ![A tanúsítványok ikonra](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. <span data-ttu-id="d2e05-158">Kattintson a **feltöltése** tanúsítványok területe tetején.</span><span class="sxs-lookup"><span data-stu-id="d2e05-158">Click **Upload** at the top of the certificates area.</span></span>

    ![Kattintson a feltöltés menüpont](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. <span data-ttu-id="d2e05-160">Adja meg a **fájl**, **jelszó**, majd kattintson a **feltöltése** a adatok terület alján.</span><span class="sxs-lookup"><span data-stu-id="d2e05-160">Provide the **File**, **Password**, then click **Upload** at the bottom of the data entry area.</span></span>

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a><span data-ttu-id="d2e05-161">4. lépés: Csatlakozás a szerepkörpéldányt HTTPS használatával</span><span class="sxs-lookup"><span data-stu-id="d2e05-161">Step 4: Connect to the role instance by using HTTPS</span></span>
<span data-ttu-id="d2e05-162">Most, hogy a központi telepítés megfelelően működik, és az Azure-ban, csatlakozhat, a HTTPS-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="d2e05-162">Now that your deployment is up and running in Azure, you can connect to it using HTTPS.</span></span>

1. <span data-ttu-id="d2e05-163">Kattintson a **webhely URL-címe** megnyitása a webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="d2e05-163">Click the **Site URL** to open up the web browser.</span></span>

   ![Kattintson a webhely URL-címe](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. <span data-ttu-id="d2e05-165">A böngészőben, módosítsa a hivatkozásra kattintva **https** helyett **http**, és keresse fel a lap.</span><span class="sxs-lookup"><span data-stu-id="d2e05-165">In your web browser, modify the link to use **https** instead of **http**, and then visit the page.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d2e05-166">Ha használ egy önaláírt tanúsítványt, ha tallózással keres egy HTTPS-végpontot, amely egy tanúsítvány hiba a böngészőben megjelenik az önaláírt tanúsítvány van társítva.</span><span class="sxs-lookup"><span data-stu-id="d2e05-166">If you are using a self-signed certificate, when you browse to an HTTPS endpoint that's associated with the self-signed certificate you may see a certificate error in the browser.</span></span> <span data-ttu-id="d2e05-167">Megszünteti egy megbízható hitelesítésszolgáltató által aláírt tanúsítványt használ az ezt a problémát. időközben figyelmen kívül hagyhatja a hibát.</span><span class="sxs-lookup"><span data-stu-id="d2e05-167">Using a certificate signed by a trusted certification authority eliminates this problem; in the meantime, you can ignore the error.</span></span> <span data-ttu-id="d2e05-168">(Az önaláírt tanúsítvány hozzáadása a felhasználói megbízható tanúsítvány tanúsítványtárolójába egy másik lehetőséget.)</span><span class="sxs-lookup"><span data-stu-id="d2e05-168">(Another option is to add the self-signed certificate to the user's trusted certificate authority certificate store.)</span></span>
   >
   >

   ![Hely megtekintése](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > <span data-ttu-id="d2e05-170">Ha azt szeretné, SSL használatát az éles környezet helyett egy átmeneti központi telepítést, először az átmeneti központi telepítéshez használt URL-cím meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="d2e05-170">If you want to use SSL for a staging deployment instead of a production deployment, you'll first need to determine the URL used for the staging deployment.</span></span> <span data-ttu-id="d2e05-171">Miután a felhőalapú szolgáltatás van telepítve, az átmeneti URL-CÍMÉT határozza meg a **üzembehelyezés-azonosító** GUID formátuma:`https://deployment-id.cloudapp.net/`</span><span class="sxs-lookup"><span data-stu-id="d2e05-171">Once your cloud service has been deployed, the URL to the staging environment is determined by the **Deployment ID** GUID in this format: `https://deployment-id.cloudapp.net/`</span></span>  
   >
   > <span data-ttu-id="d2e05-172">Hozzon létre egy tanúsítványt a köznapi nevének (CN) egyenlő a GUID-alapú URL-cím (például **328187776e774ceda8fc57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="d2e05-172">Create a certificate with the common name (CN) equal to the GUID-based URL (for example, **328187776e774ceda8fc57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="d2e05-173">A portál használatával a tanúsítvány felvétele a előkészített felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d2e05-173">Use the portal to add the certificate to your staged cloud service.</span></span> <span data-ttu-id="d2e05-174">A tanúsítvány adatait, majd hozzá a CSDEF és a szolgáltatáskonfigurációs SÉMA fájljaihoz, csomagolja újra az alkalmazást, és a szakaszos telepítés az új csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="d2e05-174">Then, add the certificate information to your CSDEF and CSCFG files, repackage your application, and update your staged deployment to use the new package.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="d2e05-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2e05-175">Next steps</span></span>
* <span data-ttu-id="d2e05-176">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d2e05-176">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="d2e05-177">Megtudhatja, hogyan [felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d2e05-177">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="d2e05-178">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d2e05-178">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="d2e05-179">[A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d2e05-179">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
