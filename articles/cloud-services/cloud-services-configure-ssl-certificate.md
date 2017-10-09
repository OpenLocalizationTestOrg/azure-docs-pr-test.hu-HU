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
# <a name="configuring-ssl-for-an-application-in-azure"></a>SSL konfigurálása az alkalmazás az Azure-ban
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-configure-ssl-certificate-portal.md)
> * [klasszikus Azure portál](cloud-services-configure-ssl-certificate.md)
> 
> 

Secure Socket Layer (SSL) titkosítás hello leggyakrabban használt módszer hello keresztül küldött adatok védelme az interneten. Általános feladat ismerteti hogyan toospecify egy webes szerepkör, és hogyan tooupload egy SSL tanúsítvány toosecure az alkalmazás HTTPS-végpontnak.

> [!NOTE]
> Ebben a feladatban hello eljárások alkalmazhatók tooAzure Felhőszolgáltatások; alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-configure-ssl-certificate.md) cikk.
> 
> 

Ez a feladat használja az éles környezet. Az átmeneti központi telepítéssel információ hello Ez a témakör végén.

Olvasási [ez](cloud-services-how-to-create-deploy.md) először annak, ha egy felhőalapú szolgáltatás még nem létrehozott.

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a>1. lépés: SSL-tanúsítvány beszerzése
SSL tooconfigure az alkalmazáshoz, először tooget által egy tanúsítvány (CA), a megbízható tanúsítványokat állít ki erre a célra harmadik fél aláírt SSL-tanúsítvány. Ha még nem rendelkezik egy, akkor tooobtain egy vállalat, amely SSL-tanúsítványok értékesít.

hello tanúsítvány hello az SSL-tanúsítványok az Azure-követelményeknek kell megfelelniük:

* hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.
* a kulcscsere, exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.
* hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello használt tartomány tooaccess hello felhőalapú szolgáltatás. Egy SSL-tanúsítványt a hitelesítésszolgáltatótól (CA) hello cloudapp.net tartomány nem lehet megszerezni. Be kell szerezni a egyéni tartomány nevét toouse amikor éri el a szolgáltatást. A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello egyéni tartomány használt név tooaccess az alkalmazás. Például, ha az egyéni tartománynevet **contoso.com** tanúsítványt kérhet tenné a hitelesítésszolgáltató a ***. contoso.com** vagy **www.contoso.com**.
* hello tanúsítvány legalább 2048 bites titkosítást kell használnia.

Tesztelési célokra is [létrehozása](cloud-services-certs-create.md) és egy önaláírt tanúsítványt használjon. Egy önaláírt tanúsítványt nem lehet hitelesíteni a hitelesítésszolgáltató keresztül, és használhatja a hello cloudapp.net tartomány hello webhely URL-címként. Például hello következő tevékenységet egy önaláírt tanúsítványt használ a mely hello hello tanúsítványban használt köznapi nevének (CN): **sslexample.cloudapp.net**.

A következő bele kell hello tanúsítvány adatait a szolgáltatás definíciós és konfigurációs fájlok.

## <a name="step-2-modify-hello-service-definition-and-configuration-files"></a>2. lépés: Hello szolgáltatás definíció- és konfigurációs fájlok módosítása
Az alkalmazáshoz beállított toouse hello tanúsítványnak kell lennie, és a HTTPS-végpontnak kell hozzáadni. Ennek eredményeképpen hello szolgáltatásdefiníció és konfigurációs fájlok kell toobe frissítése.

1. A fejlesztői környezetben nyissa meg a hello szolgáltatásdefiníciós fájl (CSDEF), adja hozzá a **tanúsítványok** belül hello szakasz **webrole típusról** szakaszt, és tartalmazza a következő információ hello a tanúsítvány (és köztes tanúsítványokat):
   
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
   
   Hello **tanúsítványok** szakasz a tanúsítványt, a helyük és az helyétől hello tároló neve hello hello nevét adja meg.
   
   Engedélyek (`permisionLevel` attribútum) is lehet a hello beállítása tooone a következő értékeket:
   
   | Engedély érték | Leírás |
   | --- | --- |
   | limitedOrElevated |**(Alapértelmezett)**  Összes szerepkör folyamat hozzáférhet hello titkos kulcsot. |
   | emelt szintű |Csak a rendszergazda jogú folyamatok hello titkos kulcs férhetnek hozzá. |
2. A szolgáltatásdefiníciós fájlban, adja hozzá egy **bemeneti végponthoz** hello elemet **végpontok** tooenable HTTPS szakasz:
   
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

3. A szolgáltatásdefiníciós fájlban, vegye fel a **kötés** hello elemet **helyek** szakasz. Ez a szakasz egy HTTPS-kötés toomap a végpont tooyour webhely hozzáadása:
   
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
   
   Minden hello a szükséges változtatásokat toohello szolgáltatásdefiníciós fájl elvégzése után, de továbbra is kell hello szolgáltatás konfigurációs fájlja a tooadd hello hitelesítő adatokat.
4. A szolgáltatás konfigurációs fájljában (a szolgáltatáskonfigurációs SÉMA), ServiceConfiguration.Cloud.cscfg, vegye fel a **tanúsítványok** belül hello szakasz **szerepkör** behelyettesíteni hello minta ujjlenyomat értékét az alább látható területen amely a tanúsítvány:
   
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

(a példában megelőző hello **sha1** hello ujjlenyomat algoritmushoz. Adja meg a tanúsítvány ujjlenyomata algoritmus hello megfelelő értéket.)

Most, hogy hello szolgáltatás definíció- és konfigurációs fájlok frissültek, csomag tooAzure feltöltése a központi telepítés. Ha használ **cspack**, ne használjon a **/generateConfigurationFile** jelzőt, módon, hogy felülírja a tanúsítvány adatait, szúrja be.

## <a name="step-3-upload-a-certificate"></a>3. lépés: A tanúsítvány feltöltése
A központi telepítési csomag már a frissített toouse hello tanúsítványt, és a HTTPS-végpontnak hozzá lett adva. Most már feltöltheti a klasszikus Azure portálon hello csomagok és tanúsítványok tooAzure hello.

1. Jelentkezzen be toohello [a klasszikus Azure portálon][Azure classic portal]. 
2. Kattintson a **Felhőszolgáltatások** hello bal oldali navigációs panelen.
3. Kattintson a kívánt hello felhőalapú szolgáltatás.
4. Kattintson a hello **tanúsítványok** fülre.
   
    ![Kattintson a hello tanúsítványok lap](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Kattintson a hello **feltöltése** gombra.
   
    ![Feltöltés](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Adja meg a hello **fájl**, **jelszó**, majd kattintson a **Complete** (hello pipa).

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a>4. lépés: Csatlakozás toohello szerepkörpéldányt HTTPS használatával
Most, hogy a központi telepítés megfelelően működik, és az Azure-ban, tooit HTTPS-kapcsolaton keresztül is elérheti.

1. A klasszikus Azure portálon hello, ki kell jelölni a telepítést, majd kattintson a hello hivatkozás alatt **webhely URL-címe**.
   
   ![Határozza meg a webhely URL-címe][2]
2. A böngészőben, módosítsa a hello hivatkozás toouse **https** helyett **http**, majd látogasson el a hello lap.
   
   > [!NOTE]
   > Ha önaláírt tanúsítványt, ha a felhasználó hello önaláírt tanúsítvány társított tooan a HTTPS-végpont használ egy Tanúsítványhiba hello böngészőben jelenhet meg. Megszünteti egy megbízható hitelesítésszolgáltató által aláírt tanúsítványt használ az ezt a problémát. hello addig, figyelmen kívül hello hiba. (Másik lehetőségként a tooadd hello önaláírt tanúsítvány toohello megbízható hitelesítésszolgáltatói tanúsítvány tanúsítványtárában.)
   > 
   > 
   
   ![SSL-webhely – példa][3]

Ha toouse SSL éles környezet helyett egy átmeneti telepítéshez, először toodetermine hello használt URL-cím telepítési átmeneti hello. A felhőalapú szolgáltatás toohello átmeneti környezet telepítése nélkül, beleértve a tanúsítvány vagy tanúsítvány információkat. Amennyiben a telepített, azt is meghatározhatja, hello GUID-alapú URL-t, melynek hello a klasszikus Azure portálon szerepel **webhely URL-címe** mező. Tanúsítvány létrehozása a hello köznapi nevének (CN) egyenlő toohello GUID-alapú URL-CÍMÉT (például **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Használjon hello Azure klasszikus portál tooadd hello tanúsítvány tooyour előkészített felhőalapú szolgáltatás. Adja hozzá a hello információk tooyour CSDEF és a szolgáltatáskonfigurációs SÉMA tanúsítványfájlok, csomagolja újra az alkalmazást, és a szakaszos telepítés toouse hello új csomag.

## <a name="next-steps"></a>Következő lépések
* [A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure.md).
* Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy.md).
* Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage.md).

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
