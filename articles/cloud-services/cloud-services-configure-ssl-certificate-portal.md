---
title: "egy felhőalapú szolgáltatás SSL aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan toospecify egy webes szerepkör, és hogyan tooupload egy SSL tanúsítvány toosecure az alkalmazás HTTPS-végpontnak. Ezekben a példákban hello Azure-portálon."
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
ms.openlocfilehash: b19283bb7b0e95374f2ae9c3532eb1effc7d6a9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="f6de0-104">SSL konfigurálása az alkalmazás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="f6de0-104">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f6de0-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f6de0-105">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="f6de0-106">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="f6de0-106">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
>

<span data-ttu-id="f6de0-107">Secure Socket Layer (SSL) titkosítás hello leggyakrabban használt módszer hello keresztül küldött adatok védelme az interneten.</span><span class="sxs-lookup"><span data-stu-id="f6de0-107">Secure Socket Layer (SSL) encryption is hello most commonly used method of securing data sent across hello internet.</span></span> <span data-ttu-id="f6de0-108">Általános feladat ismerteti hogyan toospecify egy webes szerepkör, és hogyan tooupload egy SSL tanúsítvány toosecure az alkalmazás HTTPS-végpontnak.</span><span class="sxs-lookup"><span data-stu-id="f6de0-108">This common task discusses how toospecify an HTTPS endpoint for a web role and how tooupload an SSL certificate toosecure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="f6de0-109">Ebben a feladatban hello eljárások alkalmazhatók tooAzure Felhőszolgáltatások; alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="f6de0-109">hello procedures in this task apply tooAzure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md).</span></span>
>

<span data-ttu-id="f6de0-110">Ez a feladat használja az éles környezet.</span><span class="sxs-lookup"><span data-stu-id="f6de0-110">This task uses a production deployment.</span></span> <span data-ttu-id="f6de0-111">Az átmeneti központi telepítéssel információ hello Ez a témakör végén.</span><span class="sxs-lookup"><span data-stu-id="f6de0-111">Information on using a staging deployment is provided at hello end of this topic.</span></span>

<span data-ttu-id="f6de0-112">Olvasási [ez](cloud-services-how-to-create-deploy-portal.md) első, ha egy felhőalapú szolgáltatás még nem létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f6de0-112">Read [this](cloud-services-how-to-create-deploy-portal.md) first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="f6de0-113">1. lépés: SSL-tanúsítvány beszerzése</span><span class="sxs-lookup"><span data-stu-id="f6de0-113">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="f6de0-114">SSL tooconfigure az alkalmazáshoz, először tooget által egy tanúsítvány (CA), a megbízható tanúsítványokat állít ki erre a célra harmadik fél aláírt SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="f6de0-114">tooconfigure SSL for an application, you first need tooget an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="f6de0-115">Ha még nem rendelkezik egy, akkor tooobtain egy vállalat, amely SSL-tanúsítványok értékesít.</span><span class="sxs-lookup"><span data-stu-id="f6de0-115">If you do not already have one, you need tooobtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="f6de0-116">hello tanúsítvány hello az SSL-tanúsítványok az Azure-követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="f6de0-116">hello certificate must meet hello following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="f6de0-117">hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="f6de0-117">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="f6de0-118">a kulcscsere, exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f6de0-118">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="f6de0-119">hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello használt tartomány tooaccess hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f6de0-119">hello certificate's subject name must match hello domain used tooaccess hello cloud service.</span></span> <span data-ttu-id="f6de0-120">Egy SSL-tanúsítványt a hitelesítésszolgáltatótól (CA) hello cloudapp.net tartomány nem lehet megszerezni.</span><span class="sxs-lookup"><span data-stu-id="f6de0-120">You cannot obtain an SSL certificate from a certificate authority (CA) for hello cloudapp.net domain.</span></span> <span data-ttu-id="f6de0-121">Be kell szerezni a egyéni tartomány nevét toouse amikor éri el a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f6de0-121">You must acquire a custom domain name toouse when access your service.</span></span> <span data-ttu-id="f6de0-122">A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello egyéni tartomány használt név tooaccess az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f6de0-122">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name used tooaccess your application.</span></span> <span data-ttu-id="f6de0-123">Például, ha az egyéni tartománynevet **contoso.com** tanúsítványt kérhet tenné a hitelesítésszolgáltató a ***. contoso.com** vagy **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="f6de0-123">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="f6de0-124">hello tanúsítvány legalább 2048 bites titkosítást kell használnia.</span><span class="sxs-lookup"><span data-stu-id="f6de0-124">hello certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="f6de0-125">Tesztelési célokra is [létrehozása](cloud-services-certs-create.md) és egy önaláírt tanúsítványt használjon.</span><span class="sxs-lookup"><span data-stu-id="f6de0-125">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="f6de0-126">Egy önaláírt tanúsítványt nem lehet hitelesíteni a hitelesítésszolgáltató keresztül, és használhatja a hello cloudapp.net tartomány hello webhely URL-címként.</span><span class="sxs-lookup"><span data-stu-id="f6de0-126">A self-signed certificate is not authenticated through a CA and can use hello cloudapp.net domain as hello website URL.</span></span> <span data-ttu-id="f6de0-127">Például hello következő tevékenységet egy önaláírt tanúsítványt használ a mely hello hello tanúsítványban használt köznapi nevének (CN): **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="f6de0-127">For example, hello following task uses a self-signed certificate in which hello common name (CN) used in hello certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="f6de0-128">A következő bele kell hello tanúsítvány adatait a szolgáltatás definíciós és konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="f6de0-128">Next, you must include information about hello certificate in your service definition and service configuration files.</span></span>

<span data-ttu-id="f6de0-129"><a name="modify"> </a></span><span class="sxs-lookup"><span data-stu-id="f6de0-129"><a name="modify"> </a></span></span>

## <a name="step-2-modify-hello-service-definition-and-configuration-files"></a><span data-ttu-id="f6de0-130">2. lépés: Hello szolgáltatás definíció- és konfigurációs fájlok módosítása</span><span class="sxs-lookup"><span data-stu-id="f6de0-130">Step 2: Modify hello service definition and configuration files</span></span>
<span data-ttu-id="f6de0-131">Az alkalmazáshoz beállított toouse hello tanúsítványnak kell lennie, és a HTTPS-végpontnak kell hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="f6de0-131">Your application must be configured toouse hello certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="f6de0-132">Ennek eredményeképpen hello szolgáltatásdefiníció és konfigurációs fájlok kell toobe frissítése.</span><span class="sxs-lookup"><span data-stu-id="f6de0-132">As a result, hello service definition and service configuration files need toobe updated.</span></span>

1. <span data-ttu-id="f6de0-133">A fejlesztői környezetben nyissa meg a hello szolgáltatásdefiníciós fájl (CSDEF), adja hozzá a **tanúsítványok** belül hello szakasz **webrole típusról** szakaszt, és tartalmazza a következő információ hello a tanúsítvány (és köztes tanúsítványokat):</span><span class="sxs-lookup"><span data-stu-id="f6de0-133">In your development environment, open hello service definition file (CSDEF), add a **Certificates** section within hello **WebRole** section, and include hello following information about the certificate (and intermediate certificates):</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by hello CA root, you
            must include all hello intermediate certificates
            here. You must list them here, even if they are
            not bound tooany endpoints. Failing toolist any of
            hello intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```

   <span data-ttu-id="f6de0-134">Hello **tanúsítványok** szakasz a tanúsítványt, a helyük és az helyétől hello tároló neve hello hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="f6de0-134">hello **Certificates** section defines hello name of our certificate, its location, and hello name of hello store where it is located.</span></span>

   <span data-ttu-id="f6de0-135">Engedélyek (`permisionLevel` attribútum) is lehet a hello beállítása tooone a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="f6de0-135">Permissions (`permisionLevel` attribute) can be set tooone of hello following values:</span></span>

   | <span data-ttu-id="f6de0-136">Engedély érték</span><span class="sxs-lookup"><span data-stu-id="f6de0-136">Permission Value</span></span> | <span data-ttu-id="f6de0-137">Leírás</span><span class="sxs-lookup"><span data-stu-id="f6de0-137">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="f6de0-138">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="f6de0-138">limitedOrElevated</span></span> |<span data-ttu-id="f6de0-139">**(Alapértelmezett)**  Összes szerepkör folyamat hozzáférhet hello titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f6de0-139">**(Default)** All role processes can access hello private key.</span></span> |
   | <span data-ttu-id="f6de0-140">emelt szintű</span><span class="sxs-lookup"><span data-stu-id="f6de0-140">elevated</span></span> |<span data-ttu-id="f6de0-141">Csak a rendszergazda jogú folyamatok hello titkos kulcs férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="f6de0-141">Only elevated processes can access hello private key.</span></span> |

2. <span data-ttu-id="f6de0-142">A szolgáltatásdefiníciós fájlban, adja hozzá egy **bemeneti végponthoz** hello elemet **végpontok** tooenable HTTPS szakasz:</span><span class="sxs-lookup"><span data-stu-id="f6de0-142">In your service definition file, add an **InputEndpoint** element within hello **Endpoints** section tooenable HTTPS:</span></span>

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

3. <span data-ttu-id="f6de0-143">A szolgáltatásdefiníciós fájlban, vegye fel a **kötés** hello elemet **helyek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f6de0-143">In your service definition file, add a **Binding** element within hello **Sites** section.</span></span> <span data-ttu-id="f6de0-144">Ez az elem egy HTTPS-kötés toomap a végpont tooyour webhely hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f6de0-144">This element adds an HTTPS binding toomap the endpoint tooyour site:</span></span>

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

   <span data-ttu-id="f6de0-145">Minden hello a szükséges változtatásokat toohello szolgáltatásdefiníciós fájl befejeződtek; de hello szolgáltatás konfigurációs fájlja a tooadd hello hitelesítő adatokat kell.</span><span class="sxs-lookup"><span data-stu-id="f6de0-145">All hello required changes toohello service definition file have been completed; but, you still need tooadd hello certificate information to hello service configuration file.</span></span>
4. <span data-ttu-id="f6de0-146">A szolgáltatás konfigurációs fájljában (a szolgáltatáskonfigurációs SÉMA), ServiceConfiguration.Cloud.cscfg, vegye fel a **tanúsítványok** értéket, amely a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="f6de0-146">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** value with that of your certificate.</span></span> <span data-ttu-id="f6de0-147">hello alábbi kódminta ismerteti hello **tanúsítványok** szakaszban, kivéve a hello ujjlenyomat értékét.</span><span class="sxs-lookup"><span data-stu-id="f6de0-147">hello following code sample provides details of hello **Certificates** section, except for hello thumbprint value.</span></span>

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

<span data-ttu-id="f6de0-148">(A példában az **sha1** hello ujjlenyomat algoritmushoz.</span><span class="sxs-lookup"><span data-stu-id="f6de0-148">(This example uses **sha1** for hello thumbprint algorithm.</span></span> <span data-ttu-id="f6de0-149">Adja meg a tanúsítvány ujjlenyomata algoritmus hello megfelelő értéket.)</span><span class="sxs-lookup"><span data-stu-id="f6de0-149">Specify hello appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="f6de0-150">Most, hogy hello szolgáltatás definíció- és konfigurációs fájlok frissültek, csomag tooAzure feltöltése a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="f6de0-150">Now that hello service definition and service configuration files have been updated, package your deployment for uploading tooAzure.</span></span> <span data-ttu-id="f6de0-151">Ha használ **cspack**, ne használjon a **/generateConfigurationFile** jelzőt, mivel, amelyek felülírják az imént beillesztett tanúsítvány adatait.</span><span class="sxs-lookup"><span data-stu-id="f6de0-151">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that will overwrite the certificate information you just inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="f6de0-152">3. lépés: A tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="f6de0-152">Step 3: Upload a certificate</span></span>
<span data-ttu-id="f6de0-153">Csatlakozás Azure-portálon toohello és...</span><span class="sxs-lookup"><span data-stu-id="f6de0-153">Connect toohello Azure portal and...</span></span>

1. <span data-ttu-id="f6de0-154">A hello **összes erőforrás** szakasza hello portált, válassza ki a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f6de0-154">In hello **All resources** section of hello Portal, select your cloud service.</span></span>

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. <span data-ttu-id="f6de0-156">Kattintson a **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="f6de0-156">Click **Certificates**.</span></span>

    ![Hello tanúsítványok ikonra](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. <span data-ttu-id="f6de0-158">Kattintson a **feltöltése** hello tanúsítványok terület hello tetején.</span><span class="sxs-lookup"><span data-stu-id="f6de0-158">Click **Upload** at hello top of hello certificates area.</span></span>

    ![Hello feltöltés menüpontra.](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. <span data-ttu-id="f6de0-160">Adja meg a hello **fájl**, **jelszó**, majd kattintson **feltöltése** hello adatok terület hello alján.</span><span class="sxs-lookup"><span data-stu-id="f6de0-160">Provide hello **File**, **Password**, then click **Upload** at hello bottom of hello data entry area.</span></span>

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a><span data-ttu-id="f6de0-161">4. lépés: Csatlakozás toohello szerepkörpéldányt HTTPS használatával</span><span class="sxs-lookup"><span data-stu-id="f6de0-161">Step 4: Connect toohello role instance by using HTTPS</span></span>
<span data-ttu-id="f6de0-162">Most, hogy a központi telepítés megfelelően működik, és az Azure-ban, tooit HTTPS-kapcsolaton keresztül is elérheti.</span><span class="sxs-lookup"><span data-stu-id="f6de0-162">Now that your deployment is up and running in Azure, you can connect tooit using HTTPS.</span></span>

1. <span data-ttu-id="f6de0-163">Kattintson a hello **webhely URL-címe** tooopen hello webböngésző fel.</span><span class="sxs-lookup"><span data-stu-id="f6de0-163">Click hello **Site URL** tooopen up hello web browser.</span></span>

   ![Kattintson a hello webhely URL-címe](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. <span data-ttu-id="f6de0-165">A böngészőben, módosítsa a hello hivatkozás toouse **https** helyett **http**, majd látogasson el a hello lap.</span><span class="sxs-lookup"><span data-stu-id="f6de0-165">In your web browser, modify hello link toouse **https** instead of **http**, and then visit hello page.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f6de0-166">Ha önaláírt tanúsítványt, ha a felhasználó hello önaláírt tanúsítvány társított tooan a HTTPS-végpont használ egy Tanúsítványhiba hello böngészőben jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="f6de0-166">If you are using a self-signed certificate, when you browse tooan HTTPS endpoint that's associated with hello self-signed certificate you may see a certificate error in hello browser.</span></span> <span data-ttu-id="f6de0-167">Megszünteti egy megbízható hitelesítésszolgáltató által aláírt tanúsítványt használ az ezt a problémát. hello addig, figyelmen kívül hello hiba.</span><span class="sxs-lookup"><span data-stu-id="f6de0-167">Using a certificate signed by a trusted certification authority eliminates this problem; in hello meantime, you can ignore hello error.</span></span> <span data-ttu-id="f6de0-168">(Másik lehetőségként a tooadd hello önaláírt tanúsítvány toohello megbízható hitelesítésszolgáltatói tanúsítvány tanúsítványtárában.)</span><span class="sxs-lookup"><span data-stu-id="f6de0-168">(Another option is tooadd hello self-signed certificate toohello user's trusted certificate authority certificate store.)</span></span>
   >
   >

   ![Hely megtekintése](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > <span data-ttu-id="f6de0-170">Ha toouse SSL éles környezet helyett egy átmeneti telepítéshez, először toodetermine hello használt URL-cím telepítési átmeneti hello.</span><span class="sxs-lookup"><span data-stu-id="f6de0-170">If you want toouse SSL for a staging deployment instead of a production deployment, you'll first need toodetermine hello URL used for hello staging deployment.</span></span> <span data-ttu-id="f6de0-171">A felhőalapú szolgáltatás van telepítve, ha átmeneti környezet hello URL-cím toohello határozza meg hello **üzembehelyezés-azonosító** GUID formátuma:`https://deployment-id.cloudapp.net/`</span><span class="sxs-lookup"><span data-stu-id="f6de0-171">Once your cloud service has been deployed, hello URL toohello staging environment is determined by hello **Deployment ID** GUID in this format: `https://deployment-id.cloudapp.net/`</span></span>  
   >
   > <span data-ttu-id="f6de0-172">Tanúsítvány létrehozása a hello köznapi nevének (CN) egyenlő toohello GUID-alapú URL-CÍMÉT (például **328187776e774ceda8fc57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="f6de0-172">Create a certificate with hello common name (CN) equal toohello GUID-based URL (for example, **328187776e774ceda8fc57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="f6de0-173">Használjon hello portál tooadd hello tanúsítvány tooyour előkészített felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f6de0-173">Use hello portal tooadd hello certificate tooyour staged cloud service.</span></span> <span data-ttu-id="f6de0-174">Adja hozzá a hello információk tooyour CSDEF és a szolgáltatáskonfigurációs SÉMA tanúsítványfájlok, csomagolja újra az alkalmazást, és a szakaszos telepítés toouse hello új csomag.</span><span class="sxs-lookup"><span data-stu-id="f6de0-174">Then, add hello certificate information tooyour CSDEF and CSCFG files, repackage your application, and update your staged deployment toouse hello new package.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="f6de0-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6de0-175">Next steps</span></span>
* <span data-ttu-id="f6de0-176">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f6de0-176">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="f6de0-177">Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f6de0-177">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="f6de0-178">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f6de0-178">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="f6de0-179">[A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f6de0-179">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
