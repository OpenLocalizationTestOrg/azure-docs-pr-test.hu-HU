---
title: "Az SSL konfigurálása egy felhőalapú szolgáltatás (klasszikus) |} Microsoft Docs"
description: "Útmutató: a webes szerepkör HTTPS-végpontnak megadása és az alkalmazás biztonságos SSL-tanúsítvány feltöltése."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 4cbb7f38-7994-454d-b4f0-7259b558c766
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: edb9aaf6dae11c9b8a171b22bc8a17003f80d86b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="6e464-103">SSL konfigurálása az alkalmazás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6e464-103">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6e464-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6e464-104">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="6e464-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="6e464-105">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
> 
> 

<span data-ttu-id="6e464-106">Az SSL-alapú titkosítás a leggyakrabban használt módszer az interneten keresztül küldött adatok biztonságossá tételéhez.</span><span class="sxs-lookup"><span data-stu-id="6e464-106">Secure Socket Layer (SSL) encryption is the most commonly used method of securing data sent across the internet.</span></span> <span data-ttu-id="6e464-107">Ebben az általános feladatban azt fejtjük ki, hogy miként lehet HTTPS-végpontokat meghatározni webes szerepkörökhöz, és hogyan tölthetők fel SSL-tanúsítványok az alkalmazások biztonságossá tételéhez.</span><span class="sxs-lookup"><span data-stu-id="6e464-107">This common task discusses how to specify an HTTPS endpoint for a web role and how to upload an SSL certificate to secure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="6e464-108">Ebben a feladatban eljárásokat alkalmazni az Azure Felhőszolgáltatások; alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-configure-ssl-certificate.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="6e464-108">The procedures in this task apply to Azure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md) article.</span></span>
> 
> 

<span data-ttu-id="6e464-109">Ez a feladat használja az éles környezet.</span><span class="sxs-lookup"><span data-stu-id="6e464-109">This task uses a production deployment.</span></span> <span data-ttu-id="6e464-110">Az átmeneti központi telepítéssel információt a témakör végén.</span><span class="sxs-lookup"><span data-stu-id="6e464-110">Information on using a staging deployment is provided at the end of this topic.</span></span>

<span data-ttu-id="6e464-111">Olvasási [ez](cloud-services-how-to-create-deploy.md) először annak, ha egy felhőalapú szolgáltatás még nem létrehozott.</span><span class="sxs-lookup"><span data-stu-id="6e464-111">Read [this](cloud-services-how-to-create-deploy.md) article first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="6e464-112">1. lépés: SSL-tanúsítvány beszerzése</span><span class="sxs-lookup"><span data-stu-id="6e464-112">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="6e464-113">Az SSL konfigurálása az alkalmazáshoz, akkor először kell, amely által egy tanúsítvány (CA), a megbízható tanúsítványokat állít ki erre a célra harmadik fél aláírt SSL-tanúsítvány beszerzése.</span><span class="sxs-lookup"><span data-stu-id="6e464-113">To configure SSL for an application, you first need to get an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="6e464-114">Ha még nem rendelkezik egy, meg kell szereznie egy SSL-tanúsítványok értékesít vállalat közül.</span><span class="sxs-lookup"><span data-stu-id="6e464-114">If you do not already have one, you need to obtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="6e464-115">A tanúsítványt az SSL-tanúsítványok az Azure-ban az alábbi követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="6e464-115">The certificate must meet the following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="6e464-116">A tanúsítványnak tartalmaznia kell egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="6e464-116">The certificate must contain a private key.</span></span>
* <span data-ttu-id="6e464-117">A kulcscseréhez használt, a személyes információcsere (.pfx) fájl exportálható léteznie kell a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="6e464-117">The certificate must be created for key exchange, exportable to a Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="6e464-118">A tanúsítvány tulajdonosának nevét meg kell egyeznie a felhőalapú szolgáltatás eléréséhez használt tartomány.</span><span class="sxs-lookup"><span data-stu-id="6e464-118">The certificate's subject name must match the domain used to access the cloud service.</span></span> <span data-ttu-id="6e464-119">Az SSL-tanúsítvány nem lehet megszerezni a cloudapp.net tartomány hitelesítésszolgáltatótól (CA) származó.</span><span class="sxs-lookup"><span data-stu-id="6e464-119">You cannot obtain an SSL certificate from a certificate authority (CA) for the cloudapp.net domain.</span></span> <span data-ttu-id="6e464-120">Egy egyéni tartománynevet használhat, amikor be kell szerezni a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="6e464-120">You must acquire a custom domain name to use when access your service.</span></span> <span data-ttu-id="6e464-121">A hitelesítésszolgáltató tanúsítvány kérése, amikor a tanúsítvány tulajdonosának nevét meg kell egyeznie az egyéni tartománynév, az alkalmazás eléréséhez használt.</span><span class="sxs-lookup"><span data-stu-id="6e464-121">When you request a certificate from a CA, the certificate's subject name must match the custom domain name used to access your application.</span></span> <span data-ttu-id="6e464-122">Például, ha az egyéni tartománynevet **contoso.com** tanúsítványt kérhet tenné a hitelesítésszolgáltató a ***. contoso.com** vagy **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="6e464-122">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="6e464-123">A tanúsítvány legalább 2048 bites titkosítást kell használnia.</span><span class="sxs-lookup"><span data-stu-id="6e464-123">The certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="6e464-124">Tesztelési célokra is [létrehozása](cloud-services-certs-create.md) és egy önaláírt tanúsítványt használjon.</span><span class="sxs-lookup"><span data-stu-id="6e464-124">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="6e464-125">Egy önaláírt tanúsítványt nem lehet hitelesíteni a hitelesítésszolgáltató használatával, és a cloudapp.net tartomány használható a webhely URL-címe.</span><span class="sxs-lookup"><span data-stu-id="6e464-125">A self-signed certificate is not authenticated through a CA and can use the cloudapp.net domain as the website URL.</span></span> <span data-ttu-id="6e464-126">Például a következő feladatot használ egy önaláírt tanúsítványt, amelyben a tanúsítványban használt köznapi név (CN) van **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="6e464-126">For example, the following task uses a self-signed certificate in which the common name (CN) used in the certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="6e464-127">A következő szerepelnie kell a tanúsítvány adatait a szolgáltatás definíciós és konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="6e464-127">Next, you must include information about the certificate in your service definition and service configuration files.</span></span>

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a><span data-ttu-id="6e464-128">2. lépés: A definíció- és konfigurációs fájljainak módosítása</span><span class="sxs-lookup"><span data-stu-id="6e464-128">Step 2: Modify the service definition and configuration files</span></span>
<span data-ttu-id="6e464-129">Az alkalmazás be kell állítani a tanúsítványt használja, és a HTTPS-végpontnak kell hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="6e464-129">Your application must be configured to use the certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="6e464-130">Ennek eredményeképpen a szolgáltatásdefiníció és konfigurációs fájlok frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="6e464-130">As a result, the service definition and service configuration files need to be updated.</span></span>

1. <span data-ttu-id="6e464-131">A fejlesztői környezetben nyissa meg a szolgáltatásdefiníciós fájlban (CSDEF), adja hozzá a **tanúsítványok** belül szakasz a **webrole típusról** szakaszt, és a következő információkat tartalmaznak a tanúsítvány (és köztes tanúsítványokat):</span><span class="sxs-lookup"><span data-stu-id="6e464-131">In your development environment, open the service definition file (CSDEF), add a **Certificates** section within the **WebRole** section, and include the following information about the certificate (and intermediate certificates):</span></span>
   
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
   
   <span data-ttu-id="6e464-132">A **tanúsítványok** szakasz határozza meg a nevét, valamint a tanúsítvány, helyére, a tárolási helyétől nevét.</span><span class="sxs-lookup"><span data-stu-id="6e464-132">The **Certificates** section defines the name of our certificate, its location, and the name of the store where it is located.</span></span>
   
   <span data-ttu-id="6e464-133">Engedélyek (`permisionLevel` attribútum) a következő értékek egyikére állítható be:</span><span class="sxs-lookup"><span data-stu-id="6e464-133">Permissions (`permisionLevel` attribute) can be set to one of the following values:</span></span>
   
   | <span data-ttu-id="6e464-134">Engedély érték</span><span class="sxs-lookup"><span data-stu-id="6e464-134">Permission Value</span></span> | <span data-ttu-id="6e464-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="6e464-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="6e464-136">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="6e464-136">limitedOrElevated</span></span> |<span data-ttu-id="6e464-137">**(Alapértelmezett)**  Összes szerepkör folyamat hozzáférhet a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="6e464-137">**(Default)** All role processes can access the private key.</span></span> |
   | <span data-ttu-id="6e464-138">emelt szintű</span><span class="sxs-lookup"><span data-stu-id="6e464-138">elevated</span></span> |<span data-ttu-id="6e464-139">Csak a rendszergazda jogú folyamatok férhetnek hozzá a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="6e464-139">Only elevated processes can access the private key.</span></span> |
2. <span data-ttu-id="6e464-140">A szolgáltatásdefiníciós fájlban, adja hozzá egy **bemeneti végponthoz** elemet a **végpontok** szakaszt, hogy HTTPS:</span><span class="sxs-lookup"><span data-stu-id="6e464-140">In your service definition file, add an **InputEndpoint** element within the **Endpoints** section to enable HTTPS:</span></span>
   
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

3. <span data-ttu-id="6e464-141">A szolgáltatásdefiníciós fájlban, vegye fel a **kötés** elemet a **helyek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6e464-141">In your service definition file, add a **Binding** element within the **Sites** section.</span></span> <span data-ttu-id="6e464-142">Ez a szakasz egy HTTPS-kötést a végpont hozzárendelése a webhely hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="6e464-142">This section adds an HTTPS binding to map the endpoint to your site:</span></span>
   
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
   
   <span data-ttu-id="6e464-143">A szükséges módosításokat a szolgáltatásdefiníciós fájlban való elvégzése után, de továbbra is szeretné az tanúsítvány-adatokat hozzáadni a szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="6e464-143">All the required changes to the service definition file have been completed, but you still need to add the certificate information to the service configuration file.</span></span>
4. <span data-ttu-id="6e464-144">A szolgáltatás konfigurációs fájljában (a szolgáltatáskonfigurációs SÉMA), ServiceConfiguration.Cloud.cscfg, vegye fel a **tanúsítványok** belül szakasz a **szerepkör** szakasz behelyettesíteni a minta ujjlenyomat értékét, hogy a tanúsítvány az alább látható:</span><span class="sxs-lookup"><span data-stu-id="6e464-144">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** section within the **Role** section, replacing the sample thumbprint value shown below with that of your certificate:</span></span>
   
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

<span data-ttu-id="6e464-145">(Az előző példában **sha1** ujjlenyomat algoritmust.</span><span class="sxs-lookup"><span data-stu-id="6e464-145">(The preceding example uses **sha1** for the thumbprint algorithm.</span></span> <span data-ttu-id="6e464-146">Adja meg a megfelelő értéket a tanúsítvány ujjlenyomata algoritmushoz.)</span><span class="sxs-lookup"><span data-stu-id="6e464-146">Specify the appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="6e464-147">Most, hogy a szolgáltatás definíció- és konfigurációs fájlok frissültek, csomag a központi telepítés feltöltése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="6e464-147">Now that the service definition and service configuration files have been updated, package your deployment for uploading to Azure.</span></span> <span data-ttu-id="6e464-148">Ha használ **cspack**, ne használjon a **/generateConfigurationFile** jelzőt, módon, hogy felülírja a tanúsítvány adatait, szúrja be.</span><span class="sxs-lookup"><span data-stu-id="6e464-148">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that overwrites the certificate information you inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="6e464-149">3. lépés: A tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="6e464-149">Step 3: Upload a certificate</span></span>
<span data-ttu-id="6e464-150">A központi telepítési csomag frissült a tanúsítványt használja, és a HTTPS-végpontnak hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="6e464-150">Your deployment package has been updated to use the certificate, and an HTTPS endpoint has been added.</span></span> <span data-ttu-id="6e464-151">Most már feltöltheti a csomagok és tanúsítványok az Azure-bA a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6e464-151">Now you can upload the package and certificate to Azure with the Azure classic portal.</span></span>

1. <span data-ttu-id="6e464-152">Jelentkezzen be a [a klasszikus Azure portálon][Azure classic portal].</span><span class="sxs-lookup"><span data-stu-id="6e464-152">Log in to the [Azure classic portal][Azure classic portal].</span></span> 
2. <span data-ttu-id="6e464-153">Kattintson a **Felhőszolgáltatások** a bal oldali navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="6e464-153">Click **Cloud Services** on the left-side navigation pane.</span></span>
3. <span data-ttu-id="6e464-154">Kattintson a kívánt felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6e464-154">Click the desired cloud service.</span></span>
4. <span data-ttu-id="6e464-155">Kattintson a **tanúsítványok** fülre.</span><span class="sxs-lookup"><span data-stu-id="6e464-155">Click the **Certificates** tab.</span></span>
   
    ![Kattintson a tanúsítványok lap](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. <span data-ttu-id="6e464-157">Kattintson az **Upload** (Feltöltés) gombra.</span><span class="sxs-lookup"><span data-stu-id="6e464-157">Click the **Upload** button.</span></span>
   
    ![Feltöltés](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. <span data-ttu-id="6e464-159">Adja meg a **fájl**, **jelszó**, majd kattintson a **Complete** (a pipa).</span><span class="sxs-lookup"><span data-stu-id="6e464-159">Provide the **File**, **Password**, then click **Complete** (the checkmark).</span></span>

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a><span data-ttu-id="6e464-160">4. lépés: Csatlakozás a szerepkörpéldányt HTTPS használatával</span><span class="sxs-lookup"><span data-stu-id="6e464-160">Step 4: Connect to the role instance by using HTTPS</span></span>
<span data-ttu-id="6e464-161">Most, hogy a központi telepítés megfelelően működik, és az Azure-ban, csatlakozhat, a HTTPS-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="6e464-161">Now that your deployment is up and running in Azure, you can connect to it using HTTPS.</span></span>

1. <span data-ttu-id="6e464-162">A klasszikus Azure portálon, válassza ki a központi telepítést, majd kattintson a hivatkozásra kattintva **webhely URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="6e464-162">In the Azure classic portal, select your deployment, then click the link under **Site URL**.</span></span>
   
   ![Határozza meg a webhely URL-címe][2]
2. <span data-ttu-id="6e464-164">A böngészőben, módosítsa a hivatkozásra kattintva **https** helyett **http**, és keresse fel a lap.</span><span class="sxs-lookup"><span data-stu-id="6e464-164">In your web browser, modify the link to use **https** instead of **http**, and then visit the page.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6e464-165">Ha használ egy önaláírt tanúsítványt, ha tallózással keres egy HTTPS-végpontot, amely egy tanúsítvány hiba a böngészőben megjelenik az önaláírt tanúsítvány van társítva.</span><span class="sxs-lookup"><span data-stu-id="6e464-165">If you are using a self-signed certificate, when you browse to an HTTPS endpoint that's associated with the self-signed certificate you may see a certificate error in the browser.</span></span> <span data-ttu-id="6e464-166">Megszünteti egy megbízható hitelesítésszolgáltató által aláírt tanúsítványt használ az ezt a problémát. időközben figyelmen kívül hagyhatja a hibát.</span><span class="sxs-lookup"><span data-stu-id="6e464-166">Using a certificate signed by a trusted certification authority eliminates this problem; in the meantime, you can ignore the error.</span></span> <span data-ttu-id="6e464-167">(Az önaláírt tanúsítvány hozzáadása a felhasználói megbízható tanúsítvány tanúsítványtárolójába egy másik lehetőséget.)</span><span class="sxs-lookup"><span data-stu-id="6e464-167">(Another option is to add the self-signed certificate to the user's trusted certificate authority certificate store.)</span></span>
   > 
   > 
   
   ![SSL-webhely – példa][3]

<span data-ttu-id="6e464-169">Ha azt szeretné, SSL használatát az éles környezet helyett egy átmeneti központi telepítést, először az átmeneti központi telepítéshez használt URL-cím meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="6e464-169">If you want to use SSL for a staging deployment instead of a production deployment, you first need to determine the URL used for the staging deployment.</span></span> <span data-ttu-id="6e464-170">A felhőalapú szolgáltatás telepítése az átmeneti tanúsítvány vagy a tanúsítvány adatainak megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="6e464-170">Deploy your cloud service to the staging environment without including a certificate or any certificate information.</span></span> <span data-ttu-id="6e464-171">Amennyiben telepített, meghatározhatja a GUID-alapú URL-címet, amely szerepel az Azure klasszikus portálon **webhely URL-címe** mező.</span><span class="sxs-lookup"><span data-stu-id="6e464-171">Once deployed, you can determine the GUID-based URL, which is listed in the Azure classic portal's **Site URL** field.</span></span> <span data-ttu-id="6e464-172">Hozzon létre egy tanúsítványt a köznapi nevének (CN) egyenlő a GUID-alapú URL-cím (például **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="6e464-172">Create a certificate with the common name (CN) equal to the GUID-based URL (for example, **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="6e464-173">A klasszikus Azure portál segítségével a tanúsítvány felvétele a előkészített felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6e464-173">Use the Azure classic portal to add the certificate to your staged cloud service.</span></span> <span data-ttu-id="6e464-174">A tanúsítvány adatait, majd hozzá a CSDEF és a szolgáltatáskonfigurációs SÉMA fájljaihoz, csomagolja újra az alkalmazást, és a szakaszos telepítés az új csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="6e464-174">Then, add the certificate information to your CSDEF and CSCFG files, repackage your application, and update your staged deployment to use the new package.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e464-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e464-175">Next steps</span></span>
* <span data-ttu-id="6e464-176">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure.md).</span><span class="sxs-lookup"><span data-stu-id="6e464-176">[General configuration of your cloud service](cloud-services-how-to-configure.md).</span></span>
* <span data-ttu-id="6e464-177">Megtudhatja, hogyan [felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="6e464-177">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="6e464-178">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="6e464-178">Configure a [custom domain name](cloud-services-custom-domain-name.md).</span></span>
* <span data-ttu-id="6e464-179">[A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="6e464-179">[Manage your cloud service](cloud-services-how-to-manage.md).</span></span>

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
