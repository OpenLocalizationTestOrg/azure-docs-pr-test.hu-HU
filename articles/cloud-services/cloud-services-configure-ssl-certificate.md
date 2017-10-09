---
title: "a felhőszolgáltatás (klasszikus) SSL aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan toospecify egy webes szerepkör, és hogyan tooupload egy SSL tanúsítvány toosecure az alkalmazás HTTPS-végpontnak."
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
ms.openlocfilehash: a1ca031b98af49d371977a208ed24f6dc8ea2ac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="9b7f1-103">SSL konfigurálása az alkalmazás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="9b7f1-103">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9b7f1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9b7f1-104">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="9b7f1-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="9b7f1-105">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
> 
> 

<span data-ttu-id="9b7f1-106">Secure Socket Layer (SSL) titkosítás hello leggyakrabban használt módszer hello keresztül küldött adatok védelme az interneten.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-106">Secure Socket Layer (SSL) encryption is hello most commonly used method of securing data sent across hello internet.</span></span> <span data-ttu-id="9b7f1-107">Általános feladat ismerteti hogyan toospecify egy webes szerepkör, és hogyan tooupload egy SSL tanúsítvány toosecure az alkalmazás HTTPS-végpontnak.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-107">This common task discusses how toospecify an HTTPS endpoint for a web role and how tooupload an SSL certificate toosecure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="9b7f1-108">Ebben a feladatban hello eljárások alkalmazhatók tooAzure Felhőszolgáltatások; alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-configure-ssl-certificate.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-108">hello procedures in this task apply tooAzure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md) article.</span></span>
> 
> 

<span data-ttu-id="9b7f1-109">Ez a feladat használja az éles környezet.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-109">This task uses a production deployment.</span></span> <span data-ttu-id="9b7f1-110">Az átmeneti központi telepítéssel információ hello Ez a témakör végén.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-110">Information on using a staging deployment is provided at hello end of this topic.</span></span>

<span data-ttu-id="9b7f1-111">Olvasási [ez](cloud-services-how-to-create-deploy.md) először annak, ha egy felhőalapú szolgáltatás még nem létrehozott.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-111">Read [this](cloud-services-how-to-create-deploy.md) article first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="9b7f1-112">1. lépés: SSL-tanúsítvány beszerzése</span><span class="sxs-lookup"><span data-stu-id="9b7f1-112">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="9b7f1-113">SSL tooconfigure az alkalmazáshoz, először tooget által egy tanúsítvány (CA), a megbízható tanúsítványokat állít ki erre a célra harmadik fél aláírt SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-113">tooconfigure SSL for an application, you first need tooget an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="9b7f1-114">Ha még nem rendelkezik egy, akkor tooobtain egy vállalat, amely SSL-tanúsítványok értékesít.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-114">If you do not already have one, you need tooobtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="9b7f1-115">hello tanúsítvány hello az SSL-tanúsítványok az Azure-követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="9b7f1-115">hello certificate must meet hello following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="9b7f1-116">hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-116">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="9b7f1-117">a kulcscsere, exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-117">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="9b7f1-118">hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello használt tartomány tooaccess hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-118">hello certificate's subject name must match hello domain used tooaccess hello cloud service.</span></span> <span data-ttu-id="9b7f1-119">Egy SSL-tanúsítványt a hitelesítésszolgáltatótól (CA) hello cloudapp.net tartomány nem lehet megszerezni.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-119">You cannot obtain an SSL certificate from a certificate authority (CA) for hello cloudapp.net domain.</span></span> <span data-ttu-id="9b7f1-120">Be kell szerezni a egyéni tartomány nevét toouse amikor éri el a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-120">You must acquire a custom domain name toouse when access your service.</span></span> <span data-ttu-id="9b7f1-121">A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello egyéni tartomány használt név tooaccess az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-121">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name used tooaccess your application.</span></span> <span data-ttu-id="9b7f1-122">Például, ha az egyéni tartománynevet **contoso.com** tanúsítványt kérhet tenné a hitelesítésszolgáltató a ***. contoso.com** vagy **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-122">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="9b7f1-123">hello tanúsítvány legalább 2048 bites titkosítást kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-123">hello certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="9b7f1-124">Tesztelési célokra is [létrehozása](cloud-services-certs-create.md) és egy önaláírt tanúsítványt használjon.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-124">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="9b7f1-125">Egy önaláírt tanúsítványt nem lehet hitelesíteni a hitelesítésszolgáltató keresztül, és használhatja a hello cloudapp.net tartomány hello webhely URL-címként.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-125">A self-signed certificate is not authenticated through a CA and can use hello cloudapp.net domain as hello website URL.</span></span> <span data-ttu-id="9b7f1-126">Például hello következő tevékenységet egy önaláírt tanúsítványt használ a mely hello hello tanúsítványban használt köznapi nevének (CN): **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-126">For example, hello following task uses a self-signed certificate in which hello common name (CN) used in hello certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="9b7f1-127">A következő bele kell hello tanúsítvány adatait a szolgáltatás definíciós és konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-127">Next, you must include information about hello certificate in your service definition and service configuration files.</span></span>

## <a name="step-2-modify-hello-service-definition-and-configuration-files"></a><span data-ttu-id="9b7f1-128">2. lépés: Hello szolgáltatás definíció- és konfigurációs fájlok módosítása</span><span class="sxs-lookup"><span data-stu-id="9b7f1-128">Step 2: Modify hello service definition and configuration files</span></span>
<span data-ttu-id="9b7f1-129">Az alkalmazáshoz beállított toouse hello tanúsítványnak kell lennie, és a HTTPS-végpontnak kell hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-129">Your application must be configured toouse hello certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="9b7f1-130">Ennek eredményeképpen hello szolgáltatásdefiníció és konfigurációs fájlok kell toobe frissítése.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-130">As a result, hello service definition and service configuration files need toobe updated.</span></span>

1. <span data-ttu-id="9b7f1-131">A fejlesztői környezetben nyissa meg a hello szolgáltatásdefiníciós fájl (CSDEF), adja hozzá a **tanúsítványok** belül hello szakasz **webrole típusról** szakaszt, és tartalmazza a következő információ hello a tanúsítvány (és köztes tanúsítványokat):</span><span class="sxs-lookup"><span data-stu-id="9b7f1-131">In your development environment, open hello service definition file (CSDEF), add a **Certificates** section within hello **WebRole** section, and include hello following information about the certificate (and intermediate certificates):</span></span>
   
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
   
   <span data-ttu-id="9b7f1-132">Hello **tanúsítványok** szakasz a tanúsítványt, a helyük és az helyétől hello tároló neve hello hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-132">hello **Certificates** section defines hello name of our certificate, its location, and hello name of hello store where it is located.</span></span>
   
   <span data-ttu-id="9b7f1-133">Engedélyek (`permisionLevel` attribútum) is lehet a hello beállítása tooone a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="9b7f1-133">Permissions (`permisionLevel` attribute) can be set tooone of hello following values:</span></span>
   
   | <span data-ttu-id="9b7f1-134">Engedély érték</span><span class="sxs-lookup"><span data-stu-id="9b7f1-134">Permission Value</span></span> | <span data-ttu-id="9b7f1-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="9b7f1-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="9b7f1-136">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="9b7f1-136">limitedOrElevated</span></span> |<span data-ttu-id="9b7f1-137">**(Alapértelmezett)**  Összes szerepkör folyamat hozzáférhet hello titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-137">**(Default)** All role processes can access hello private key.</span></span> |
   | <span data-ttu-id="9b7f1-138">emelt szintű</span><span class="sxs-lookup"><span data-stu-id="9b7f1-138">elevated</span></span> |<span data-ttu-id="9b7f1-139">Csak a rendszergazda jogú folyamatok hello titkos kulcs férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-139">Only elevated processes can access hello private key.</span></span> |
2. <span data-ttu-id="9b7f1-140">A szolgáltatásdefiníciós fájlban, adja hozzá egy **bemeneti végponthoz** hello elemet **végpontok** tooenable HTTPS szakasz:</span><span class="sxs-lookup"><span data-stu-id="9b7f1-140">In your service definition file, add an **InputEndpoint** element within hello **Endpoints** section tooenable HTTPS:</span></span>
   
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

3. <span data-ttu-id="9b7f1-141">A szolgáltatásdefiníciós fájlban, vegye fel a **kötés** hello elemet **helyek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-141">In your service definition file, add a **Binding** element within hello **Sites** section.</span></span> <span data-ttu-id="9b7f1-142">Ez a szakasz egy HTTPS-kötés toomap a végpont tooyour webhely hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="9b7f1-142">This section adds an HTTPS binding toomap the endpoint tooyour site:</span></span>
   
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
   
   <span data-ttu-id="9b7f1-143">Minden hello a szükséges változtatásokat toohello szolgáltatásdefiníciós fájl elvégzése után, de továbbra is kell hello szolgáltatás konfigurációs fájlja a tooadd hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-143">All hello required changes toohello service definition file have been completed, but you still need tooadd hello certificate information to hello service configuration file.</span></span>
4. <span data-ttu-id="9b7f1-144">A szolgáltatás konfigurációs fájljában (a szolgáltatáskonfigurációs SÉMA), ServiceConfiguration.Cloud.cscfg, vegye fel a **tanúsítványok** belül hello szakasz **szerepkör** behelyettesíteni hello minta ujjlenyomat értékét az alább látható területen amely a tanúsítvány:</span><span class="sxs-lookup"><span data-stu-id="9b7f1-144">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** section within hello **Role** section, replacing hello sample thumbprint value shown below with that of your certificate:</span></span>
   
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

<span data-ttu-id="9b7f1-145">(a példában megelőző hello **sha1** hello ujjlenyomat algoritmushoz.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-145">(hello preceding example uses **sha1** for hello thumbprint algorithm.</span></span> <span data-ttu-id="9b7f1-146">Adja meg a tanúsítvány ujjlenyomata algoritmus hello megfelelő értéket.)</span><span class="sxs-lookup"><span data-stu-id="9b7f1-146">Specify hello appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="9b7f1-147">Most, hogy hello szolgáltatás definíció- és konfigurációs fájlok frissültek, csomag tooAzure feltöltése a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-147">Now that hello service definition and service configuration files have been updated, package your deployment for uploading tooAzure.</span></span> <span data-ttu-id="9b7f1-148">Ha használ **cspack**, ne használjon a **/generateConfigurationFile** jelzőt, módon, hogy felülírja a tanúsítvány adatait, szúrja be.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-148">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that overwrites the certificate information you inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="9b7f1-149">3. lépés: A tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="9b7f1-149">Step 3: Upload a certificate</span></span>
<span data-ttu-id="9b7f1-150">A központi telepítési csomag már a frissített toouse hello tanúsítványt, és a HTTPS-végpontnak hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-150">Your deployment package has been updated toouse hello certificate, and an HTTPS endpoint has been added.</span></span> <span data-ttu-id="9b7f1-151">Most már feltöltheti a klasszikus Azure portálon hello csomagok és tanúsítványok tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-151">Now you can upload hello package and certificate tooAzure with hello Azure classic portal.</span></span>

1. <span data-ttu-id="9b7f1-152">Jelentkezzen be toohello [a klasszikus Azure portálon][Azure classic portal].</span><span class="sxs-lookup"><span data-stu-id="9b7f1-152">Log in toohello [Azure classic portal][Azure classic portal].</span></span> 
2. <span data-ttu-id="9b7f1-153">Kattintson a **Felhőszolgáltatások** hello bal oldali navigációs panelen.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-153">Click **Cloud Services** on hello left-side navigation pane.</span></span>
3. <span data-ttu-id="9b7f1-154">Kattintson a kívánt hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-154">Click hello desired cloud service.</span></span>
4. <span data-ttu-id="9b7f1-155">Kattintson a hello **tanúsítványok** fülre.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-155">Click hello **Certificates** tab.</span></span>
   
    ![Kattintson a hello tanúsítványok lap](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. <span data-ttu-id="9b7f1-157">Kattintson a hello **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-157">Click hello **Upload** button.</span></span>
   
    ![Feltöltés](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. <span data-ttu-id="9b7f1-159">Adja meg a hello **fájl**, **jelszó**, majd kattintson a **Complete** (hello pipa).</span><span class="sxs-lookup"><span data-stu-id="9b7f1-159">Provide hello **File**, **Password**, then click **Complete** (hello checkmark).</span></span>

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a><span data-ttu-id="9b7f1-160">4. lépés: Csatlakozás toohello szerepkörpéldányt HTTPS használatával</span><span class="sxs-lookup"><span data-stu-id="9b7f1-160">Step 4: Connect toohello role instance by using HTTPS</span></span>
<span data-ttu-id="9b7f1-161">Most, hogy a központi telepítés megfelelően működik, és az Azure-ban, tooit HTTPS-kapcsolaton keresztül is elérheti.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-161">Now that your deployment is up and running in Azure, you can connect tooit using HTTPS.</span></span>

1. <span data-ttu-id="9b7f1-162">A klasszikus Azure portálon hello, ki kell jelölni a telepítést, majd kattintson a hello hivatkozás alatt **webhely URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-162">In hello Azure classic portal, select your deployment, then click hello link under **Site URL**.</span></span>
   
   ![Határozza meg a webhely URL-címe][2]
2. <span data-ttu-id="9b7f1-164">A böngészőben, módosítsa a hello hivatkozás toouse **https** helyett **http**, majd látogasson el a hello lap.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-164">In your web browser, modify hello link toouse **https** instead of **http**, and then visit hello page.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9b7f1-165">Ha önaláírt tanúsítványt, ha a felhasználó hello önaláírt tanúsítvány társított tooan a HTTPS-végpont használ egy Tanúsítványhiba hello böngészőben jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-165">If you are using a self-signed certificate, when you browse tooan HTTPS endpoint that's associated with hello self-signed certificate you may see a certificate error in hello browser.</span></span> <span data-ttu-id="9b7f1-166">Megszünteti egy megbízható hitelesítésszolgáltató által aláírt tanúsítványt használ az ezt a problémát. hello addig, figyelmen kívül hello hiba.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-166">Using a certificate signed by a trusted certification authority eliminates this problem; in hello meantime, you can ignore hello error.</span></span> <span data-ttu-id="9b7f1-167">(Másik lehetőségként a tooadd hello önaláírt tanúsítvány toohello megbízható hitelesítésszolgáltatói tanúsítvány tanúsítványtárában.)</span><span class="sxs-lookup"><span data-stu-id="9b7f1-167">(Another option is tooadd hello self-signed certificate toohello user's trusted certificate authority certificate store.)</span></span>
   > 
   > 
   
   ![SSL-webhely – példa][3]

<span data-ttu-id="9b7f1-169">Ha toouse SSL éles környezet helyett egy átmeneti telepítéshez, először toodetermine hello használt URL-cím telepítési átmeneti hello.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-169">If you want toouse SSL for a staging deployment instead of a production deployment, you first need toodetermine hello URL used for hello staging deployment.</span></span> <span data-ttu-id="9b7f1-170">A felhőalapú szolgáltatás toohello átmeneti környezet telepítése nélkül, beleértve a tanúsítvány vagy tanúsítvány információkat.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-170">Deploy your cloud service toohello staging environment without including a certificate or any certificate information.</span></span> <span data-ttu-id="9b7f1-171">Amennyiben a telepített, azt is meghatározhatja, hello GUID-alapú URL-t, melynek hello a klasszikus Azure portálon szerepel **webhely URL-címe** mező.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-171">Once deployed, you can determine hello GUID-based URL, which is listed in hello Azure classic portal's **Site URL** field.</span></span> <span data-ttu-id="9b7f1-172">Tanúsítvány létrehozása a hello köznapi nevének (CN) egyenlő toohello GUID-alapú URL-CÍMÉT (például **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="9b7f1-172">Create a certificate with hello common name (CN) equal toohello GUID-based URL (for example, **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="9b7f1-173">Használjon hello Azure klasszikus portál tooadd hello tanúsítvány tooyour előkészített felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-173">Use hello Azure classic portal tooadd hello certificate tooyour staged cloud service.</span></span> <span data-ttu-id="9b7f1-174">Adja hozzá a hello információk tooyour CSDEF és a szolgáltatáskonfigurációs SÉMA tanúsítványfájlok, csomagolja újra az alkalmazást, és a szakaszos telepítés toouse hello új csomag.</span><span class="sxs-lookup"><span data-stu-id="9b7f1-174">Then, add hello certificate information tooyour CSDEF and CSCFG files, repackage your application, and update your staged deployment toouse hello new package.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b7f1-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b7f1-175">Next steps</span></span>
* <span data-ttu-id="9b7f1-176">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure.md).</span><span class="sxs-lookup"><span data-stu-id="9b7f1-176">[General configuration of your cloud service](cloud-services-how-to-configure.md).</span></span>
* <span data-ttu-id="9b7f1-177">Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="9b7f1-177">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="9b7f1-178">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="9b7f1-178">Configure a [custom domain name](cloud-services-custom-domain-name.md).</span></span>
* <span data-ttu-id="9b7f1-179">[A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="9b7f1-179">[Manage your cloud service](cloud-services-how-to-manage.md).</span></span>

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
