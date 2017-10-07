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
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>SSL-tanúsítvány vásárlása és konfigurálása saját Azure App Service szolgáltatások számára

Ebben az oktatóanyagban az SSL-tanúsítvány megvásárlásával fog biztonságos a webalkalmazás a  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, biztonságos helyen tárolja [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), és való társítás egyéni tartományt.

## <a name="step-1---log-in-tooazure"></a>1. lépés – tooAzure bejelentkezés

Jelentkezzen be toohello http://portal.azure.com: az Azure portál

## <a name="step-2---place-an-ssl-certificate-order"></a>2. lépés - az SSL-tanúsítvány megrendelés

Hozzon létre egy új SSL-tanúsítvány rendelés elhelyezhet [App szolgáltatástanúsítvány](https://portal.azure.com/#create/Microsoft.SSL) a hello **Azure-portálon**.

![Tanúsítvány létrehozása](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Adjon meg egy rövid **neve** SSL-tanúsítványok a tanúsítványok, és írja be a hello **tartománynév**

> [!NOTE]
> Ez az egyik legfontosabb részeit hello hello vásárlási eljárást. Győződjön meg arról, hogy javítsa ki a gazdagép nevét (egyéni tartomány), amelyet az ezzel a tanúsítvánnyal tooprotect tooenter. **NE** hello állomásnévvel rendelkező Webszolgáltatáshoz append. 
>

Válassza ki a **előfizetés**, **erőforráscsoport**, és **tanúsítvány-SKU**

> [!WARNING]
> App Service-tanúsítványokkal csak használhatja más alkalmazásszolgáltatások belül hello ugyanahhoz az előfizetéshez.  
>

## <a name="step-3---store-hello-certificate-in-azure-key-vault"></a>3. lépés – az Azure Key Vault-tároló hello tanúsítványt

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) Azure szolgáltatás, amely segít a felhőalapú alkalmazások és szolgáltatások által használt titkosítási kulcsok és titkos védelme.
>

Hello SSL-tanúsítvány a vásárlás befejezése után kell tooopen [App Service-tanúsítványokkal](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) erőforráspanelen.

![Helyezze be KV készen toostore képe](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

Megfigyelheti, hogy van-e a tanúsítvány állapotának **"Függőben lévő kiállítási"** , néhány további lépést kell toocomplete ezzel a tanúsítvánnyal megkezdése előtt.

Kattintson a **Tanúsítványkonfiguráció** belül tanúsítvány tulajdonságai panelen, majd kattintson a **1. lépés: tároló** toostore Azure Key Vault ezt a tanúsítványt.

A **Key Vault állapot** panelen kattintson a **Key Vault tárház** toochoose egy meglévő kulcstároló toostore ezt a tanúsítványt **vagy hozzon létre új Key Vault** toocreate új kulcsot tároló azonos előfizetésbe és erőforráscsoportba csoporton belül.

> [!NOTE]
> Az Azure Key Vault tárolja a tanúsítványt a minimális díja van.
> További információkért lásd:  **[Azure Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Miután kiválasztotta hello Key Vault tárház toostore ezt a tanúsítványt, hello **tároló** beállítást meg kell jelennie a sikeres.

![Helyezze be a tároló sikeres KV képe](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-hello-domain-ownership"></a>4. lépés - hello tartomány tulajdonjog ellenőrzése

> [!NOTE]
> App service-tanúsítványokkal által támogatott tartományok ellenőrzésének három típusa van: tartomány, E-mail, manuális ellenőrzése. Ezek magyarázata hello részletesebben [szakasz speciális](#advanced).

A hello azonos **Tanúsítványkonfiguráció** panel használja a 3. lépésben, kattintson a **2. lépés: Ellenőrizze**.

**Tartományok ellenőrzésének** hello legkényelmesebben folyamat azt **csak ha** rendelkezik  **[az Azure App Service szolgáltatásban az egyéni tartomány vásárolt.](custom-dns-web-site-buydomains-web-app.md)**
Kattintson a **ellenőrizze** gomb toocomplete ezt a lépést.

![Helyezze be a tartomány ellenőrzése képe](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

Kattintás után **ellenőrizze**, hello használata **frissítése** gombra, amíg hello **győződjön meg arról** beállítást meg kell jelennie a sikeres.

![Kép beszúrása a KV sikerességének ellenőrzése](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-tooapp-service-app"></a>5. lépés – tanúsítvány tooApp Service alkalmazás hozzárendelése

> [!NOTE]
> Ebben a szakaszban hello lépések elvégzése előtt kell társítva van egy egyéni tartománynevet az alkalmazást. További információkért lásd:  **[egy webalkalmazást az egyéni tartománynév beállítása.](app-service-web-tutorial-custom-domain.md)**
>

A hello  **[Azure-portálon](https://portal.azure.com/)**, hello kattintson **App Service** hello lap hello bal oldali lehetőséget.

Hello neve kattintson az alkalmazás toowhich kívánt tooassign ezt a tanúsítványt.

A hello **beállítások**, kattintson a **SSL-tanúsítványok**.

Kattintson a **App Service-tanúsítvány importálása** és select hello tanúsítvány csak megvásárolt.

![Helyezze be a tanúsítvány importálása képe](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

A hello **ssl-kötések** szakaszban kattintson a **felvenni kötéseket**, és a hello legördülő listák megnyílásának tooselect hello tartomány neve toosecure használjon SSL, és a tanúsítvány toouse hello. Is kiválaszthat a e toouse  **[kiszolgálónév jelzése (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  vagy IP-alapú SSL.

![az SSL-kötések kép beszúrása](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Kattintson a **kötésének hozzáadása** toosave hello módosításokat, és engedélyezi az SSL.

> [!NOTE]
> Ha a kiválasztott **IP-alapú SSL** és az egyéni tartomány úgy van konfigurálva, az A rekord, hello következő további lépéseket kell végrehajtania. Ezek magyarázata hello részletesebben [szakasz speciális](#Advanced).

Ekkor meg kell tudni toovisit az alkalmazás használatával `HTTPS://` helyett `HTTP://` tooverify, amely hello tanúsítvány megfelelően van konfigurálva.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>6 - felügyeleti feladatokat. lépés

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a>Extra szintű

### <a name="verifying-domain-ownership"></a>Tartomány-tulajdonjogának ellenőrzéséről

App service-tanúsítványokkal által támogatott tartományok ellenőrzésének további két típusa van: Mail és a kézi ellenőrzése.

#### <a name="mail-verification"></a>Mail ellenőrzése

Megerősítési e-mailt már elküldte toohello az egyéni tartomány társított E-mail címe.
toocomplete hello E-mail ellenőrzési lépés, nyissa meg a hello e-mailt, és kattintson a hello megerősítési hivatkozást.

![Helyezze be az e-mail ellenőrzése képe](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

Ha tooresend hello megerősítési e-mailt, kattintson a hello **E-mail újraküldése** gombra.

#### <a name="manual-verification"></a>Kézi ellenőrzése

> [!IMPORTANT]
> HTML-weblap ellenőrzése (csak a Standard tanúsítvány Termékváltozat működik)
>

1. Hozzon létre egy HTML fájlt **"starfield.html"**

1. Tartalom fájl kell tartomány ellenőrző jogkivonat hello hello pontos nevét. (Átmásolhatja hello token hello tartomány ellenőrzési állapota panel)

1. A feltöltés, a tartomány hello webkiszolgáló hello gyökérkönyvtárában`/.well-known/pki-validation/starfield.html`

1. Kattintson a **frissítése** tooupdate hello tanúsítvány állapotának ellenőrzése után. Ellenőrzési toocomplete néhány percig is eltarthat.

> [!TIP]
> Ellenőrizze a terminál használatával `curl -G http://<domain>/.well-known/pki-validation/starfield.html` hello válasz tartalmaznia kell a hello `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>DNS TXT rekord ellenőrzése

1. A DNS-kezelő használatával hozzon létre egy TXT rekordot a hello `@` altartomány és értéke egyenlő toohello tartomány ellenőrző jogkivonat.
1. Kattintson a **"Frissítés"** tooupdate hello tanúsítvány állapotának ellenőrzése után.

> [!TIP]
> A TXT-rekord toocreate kell `@.<domain>` értékű `<verification-token>`.

### <a name="assign-certificate-tooapp-service-app"></a>Tanúsítvány tooApp Service alkalmazás hozzárendelése

Ha a kiválasztott **IP-alapú SSL** és az egyéni tartomány úgy van konfigurálva, az A rekord, végre kell hajtania a következő további lépéseket hello:

Beállítása után egy IP-alapú SSL-kötés, dedikált IP-címnek a tooyour alkalmazás hozzá van rendelve. Az IP-cím található hello **egyéni tartomány** az alkalmazás jobbra fent hello beállítások lapján **Hostnames** szakasz. Szerepel **külső IP-cím**

![Helyezze be az IP-SSL képe](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Ne feledje, hogy az IP-cím más, mint a virtuális IP-cím hello korábban használt tooconfigure hello A rekord a tartomány. Ha konfigurált toouse SNI SSL-alapú, és nincsenek beállítva toouse SSL, akkor a bejegyzés nincs cím szerepel.

A regisztrációs által biztosított hello eszközöket használ, módosítsa a hello egy rekordot az egyéni tartomány nevét toopoint toohello IP-cím hello előző lépésben.

## <a name="rekey-and-sync-hello-certificate"></a>Kulcs újbóli létrehozása és a szinkronizálási hello tanúsítvány

Ha valaha is kell tooRekey a tanúsítvány, válasszon **kulcsismétlés és tényleges szinkronizálási** parancsát **tanúsítvány tulajdonságai** panelen.

Kattintson a **kulcsismétlés** tooinitiate hello folyamat gombra. 1 – 10 percig toocomplete vehet igénybe.

![Helyezze be a kulcsismétlés SSL képe](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

A tanúsítvány újbóli visszaállítja a hello tanúsítvány hello hitelesítésszolgáltatótól származó kibocsátott új tanúsítványt.

## <a name="next-steps"></a>Következő lépések

* [Egy Tartalomkézbesítési hálózat hozzáadása](app-service-web-tutorial-content-delivery-network.md)