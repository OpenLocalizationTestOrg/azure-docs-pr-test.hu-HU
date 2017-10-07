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
# <a name="configuring-ssl-for-an-application-in-azure"></a>SSL konfigurálása az alkalmazás az Azure-ban
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-configure-ssl-certificate-portal.md)
> * [klasszikus Azure portál](cloud-services-configure-ssl-certificate.md)
>

Secure Socket Layer (SSL) titkosítás hello leggyakrabban használt módszer hello keresztül küldött adatok védelme az interneten. Általános feladat ismerteti hogyan toospecify egy webes szerepkör, és hogyan tooupload egy SSL tanúsítvány toosecure az alkalmazás HTTPS-végpontnak.

> [!NOTE]
> Ebben a feladatban hello eljárások alkalmazhatók tooAzure Felhőszolgáltatások; alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-configure-ssl-certificate.md).
>

Ez a feladat használja az éles környezet. Az átmeneti központi telepítéssel információ hello Ez a témakör végén.

Olvasási [ez](cloud-services-how-to-create-deploy-portal.md) első, ha egy felhőalapú szolgáltatás még nem létrehozott.

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

<a name="modify"> </a>

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

3. A szolgáltatásdefiníciós fájlban, vegye fel a **kötés** hello elemet **helyek** szakasz. Ez az elem egy HTTPS-kötés toomap a végpont tooyour webhely hozzáadása:

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

   Minden hello a szükséges változtatásokat toohello szolgáltatásdefiníciós fájl befejeződtek; de hello szolgáltatás konfigurációs fájlja a tooadd hello hitelesítő adatokat kell.
4. A szolgáltatás konfigurációs fájljában (a szolgáltatáskonfigurációs SÉMA), ServiceConfiguration.Cloud.cscfg, vegye fel a **tanúsítványok** értéket, amely a tanúsítvány. hello alábbi kódminta ismerteti hello **tanúsítványok** szakaszban, kivéve a hello ujjlenyomat értékét.

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

(A példában az **sha1** hello ujjlenyomat algoritmushoz. Adja meg a tanúsítvány ujjlenyomata algoritmus hello megfelelő értéket.)

Most, hogy hello szolgáltatás definíció- és konfigurációs fájlok frissültek, csomag tooAzure feltöltése a központi telepítés. Ha használ **cspack**, ne használjon a **/generateConfigurationFile** jelzőt, mivel, amelyek felülírják az imént beillesztett tanúsítvány adatait.

## <a name="step-3-upload-a-certificate"></a>3. lépés: A tanúsítvány feltöltése
Csatlakozás Azure-portálon toohello és...

1. A hello **összes erőforrás** szakasza hello portált, válassza ki a felhőalapú szolgáltatás.

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Kattintson a **tanúsítványok**.

    ![Hello tanúsítványok ikonra](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Kattintson a **feltöltése** hello tanúsítványok terület hello tetején.

    ![Hello feltöltés menüpontra.](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. Adja meg a hello **fájl**, **jelszó**, majd kattintson **feltöltése** hello adatok terület hello alján.

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a>4. lépés: Csatlakozás toohello szerepkörpéldányt HTTPS használatával
Most, hogy a központi telepítés megfelelően működik, és az Azure-ban, tooit HTTPS-kapcsolaton keresztül is elérheti.

1. Kattintson a hello **webhely URL-címe** tooopen hello webböngésző fel.

   ![Kattintson a hello webhely URL-címe](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. A böngészőben, módosítsa a hello hivatkozás toouse **https** helyett **http**, majd látogasson el a hello lap.

   > [!NOTE]
   > Ha önaláírt tanúsítványt, ha a felhasználó hello önaláírt tanúsítvány társított tooan a HTTPS-végpont használ egy Tanúsítványhiba hello böngészőben jelenhet meg. Megszünteti egy megbízható hitelesítésszolgáltató által aláírt tanúsítványt használ az ezt a problémát. hello addig, figyelmen kívül hello hiba. (Másik lehetőségként a tooadd hello önaláírt tanúsítvány toohello megbízható hitelesítésszolgáltatói tanúsítvány tanúsítványtárában.)
   >
   >

   ![Hely megtekintése](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > Ha toouse SSL éles környezet helyett egy átmeneti telepítéshez, először toodetermine hello használt URL-cím telepítési átmeneti hello. A felhőalapú szolgáltatás van telepítve, ha átmeneti környezet hello URL-cím toohello határozza meg hello **üzembehelyezés-azonosító** GUID formátuma:`https://deployment-id.cloudapp.net/`  
   >
   > Tanúsítvány létrehozása a hello köznapi nevének (CN) egyenlő toohello GUID-alapú URL-CÍMÉT (például **328187776e774ceda8fc57609d404462.cloudapp.net**). Használjon hello portál tooadd hello tanúsítvány tooyour előkészített felhőalapú szolgáltatás. Adja hozzá a hello információk tooyour CSDEF és a szolgáltatáskonfigurációs SÉMA tanúsítványfájlok, csomagolja újra az alkalmazást, és a szakaszos telepítés toouse hello új csomag.
   >

## <a name="next-steps"></a>Következő lépések
* [A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).
* Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).
* Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).
