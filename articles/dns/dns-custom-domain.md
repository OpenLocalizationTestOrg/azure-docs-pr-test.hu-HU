---
title: "az Azure-erőforrások az Azure DNS aaaIntegrate |} Microsoft Docs"
description: "Megtudhatja, hogyan mentén tooprovide DNS az Azure-erőforrások az Azure DNS toouse."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: b9b6f829513f0ad9da510190c75bc60dc7431545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-dns-tooprovide-custom-domain-settings-for-an-azure-service"></a>Azure DNS-tooprovide egyéni tartomány beállítások használata az Azure-szolgáltatások

Az Azure DNS biztosít a DNS az Azure-erőforrások valamelyike egyéni tartományt, támogatási egyéni tartományok, illetve hogy rendelkezik-e egy teljesen minősített tartománynevét (FQDN). Példa: egy Azure webes alkalmazás, és azt szeretné, hogy a felhasználók tooaccess azt által contoso.com, vagy a www.contoso.com teljes tartománynév használatával. Ez a cikk végigvezeti az Azure service konfigurálása az Azure DNS-beli egyéni tartományok használatának.

## <a name="prerequisites"></a>Előfeltételek

A sorrend toouse Azure DNS-ben az egyéni tartomány akkor kell átadhatja a tartomány tooAzure DNS. Látogasson el [delegálása a tartományi tooAzure DNS](./dns-delegate-domain-azure-dns.md) útmutatást tooconfigure a Névkiszolgálók delegálásra. Ha a tartomány delegált tooyour Azure DNS-zóna, képes tooconfigure hello DNS-rekordok szükséges áll.

Konfigurálhat egy személyes vagy tartozó egyéni tartomány [Azure függvény alkalmazások](#azure-function-app), [Azure IoT](#azure-iot), [nyilvános IP-címek](#public-ip-address), [App Service (webalkalmazások)](#app-service-web-apps), [Blob-tároló](#blob-storage), és [Azure CDN](#azure-cdn).

## <a name="azure-function-app"></a>Az Azure-függvény alkalmazás

egy olyan CNAME rekordot az tooconfigure Azure függvény alkalmazásokhoz egyéni tartományt, valamint hello függvény alkalmazás maga a konfigurációs jön létre.
 
Keresse meg a túl**más** > **függvény App** válassza ki a függvény alkalmazást. Kattintson a **Platform funkciói** és a **hálózati** kattintson **egyéni tartományok**.

![függvény alkalmazás paneljén](./media/dns-custom-domain/functionapp.png)

Vegye figyelembe a hello érvényes URL-címet a hello **egyéni tartományok** panelen, ez a cím hello aliasként használatos hello DNS-rekord létrehozása.

![az egyéni tartomány panel](./media/dns-custom-domain/functionshostname.png)

Nyissa meg a tooyour DNS-zónát, és kattintson a **+ rekordhalmaz**. Töltse ki a következő információ a hello hello **adja hozzá a rekordhalmaz** panel megnyitásához, és kattintson **OK** toocreate azt.

|Tulajdonság  |Érték  |Leírás  |
|---------|---------|---------|
|Név     | myfunctionapp        | Ezt az értéket, valamint hello tartománynév-címke hello FQDN hello egyéni tartománynév.        |
|Típus     | CNAME        | Használjon egy olyan CNAME rekordot alias használ.        |
|ÉLETTARTAM     | 1        | 1 használt 1 óra        |
|Élettartam egység     | Óra        | Hello időmérést használatosak üzemideje (óra)         |
|Alias     | adatumfunction.azurewebsites.NET        | hello DNS-név létrehozásakor hello alias, ebben a példában hello adatumfunction.azurewebsites.net DNS-név alapértelmezett toohello függvény alkalmazás által biztosított.        |

Lépjen vissza tooyour függvény app, kattintson **Platform funkciói**, majd a **hálózati** kattintson **egyéni tartományok**, majd a **Hostnames**kattintson **+ Hozzáadás állomásnév**.

A hello **állomásnév hozzáadása** panelen adja meg a hello CNAME rekordot a hello **állomásnév** szövegmezőt, és kattintson **ellenőrzése**. Ha a hello rekord nem található képes toobe, hello **állomásnév hozzáadása** gomb akkor jelenik meg. Kattintson a **állomásnév hozzáadása** tooadd hello alias.

![függvény alkalmazások hozzáadása a gazdagép neve panel](./media/dns-custom-domain/functionaddhostname.png)

## <a name="azure-iot"></a>Azure IoT

Az Azure IoT nincs minden testreszabott beállítás hello szolgáltatást van szükség. az IoT-központ egyéni tartomány toouse csak egy CNAME rekordot a hivatkozott toohello erőforrások van szükség.

Keresse meg a túl**az eszközök internetes hálózatát** > **IoT-központ** válassza ki az IoT hub. A hello **áttekintése** panelen Megjegyzés hello hello IoT-központ teljes Tartományneve.

![IoT Hub panel](./media/dns-custom-domain/iot.png)

Ezután keresse meg a tooyour DNS-zónát, és kattintson **+ rekordhalmaz**. Töltse ki a következő információ a hello hello **adja hozzá a rekordhalmaz** panel megnyitásához, és kattintson **OK** toocreate azt.


|Tulajdonság  |Érték  |Leírás  |
|---------|---------|---------|
|Név     | myiothub        | Ezt az értéket, valamint hello tartománynév-címke hello FQDN hello IoT hub.        |
|Típus     | CNAME        | Használjon egy olyan CNAME rekordot alias használ.
|ÉLETTARTAM     | 1        | 1 használt 1 óra        |
|Élettartam egység     | Óra        | Hello időmérést használatosak üzemideje (óra)         |
|Alias     | adatumIOT.azure-devices.net        | hello DNS-név létrehozásakor hello alias, ebben a példában hello adatumIOT.azure-devices.net állomás név hello IoT-központ által biztosított.

Hello rekord létrehozása után hello CNAME rekord használatával a névfeloldás tesztelése`nslookup`

## <a name="public-ip-address"></a>Nyilvános IP-cím

egy egyéni tartományt a nyilvános IP-címet használó szolgáltatások tooconfigure cím Alkalmazásátjáró, Load Balancer felhőalapú szolgáltatás, erőforrás-kezelő virtuális gépeken, például erőforrás, és egy CNAME rekordot a klasszikus virtuális gépekhez használt.

Keresse meg a túl**hálózati** > **nyilvános IP-cím**, válassza ki a hello nyilvános IP-cím erőforrás, és kattintson a **konfigurációs**. Notate hello IP-cím látható.

![nyilvános IP-cím panel](./media/dns-custom-domain/publicip.png)

Nyissa meg a tooyour DNS-zónát, és kattintson a **+ rekordhalmaz**. Töltse ki a következő információ a hello hello **adja hozzá a rekordhalmaz** panel megnyitásához, és kattintson **OK** toocreate azt.


|Tulajdonság  |Érték  |Leírás  |
|---------|---------|---------|
|Név     | mywebserver        | Ezt az értéket, valamint hello tartománynév-címke hello FQDN hello egyéni tartománynév.        |
|Típus     | A        | Az A rekord használatára, mert hello erőforrás IP-címet.        |
|ÉLETTARTAM     | 1        | 1 használt 1 óra        |
|Élettartam egység     | Óra        | Hello időmérést használatosak üzemideje (óra)         |
|IP-cím     | <your ip address>       | hello nyilvános IP-cím.|

![az A rekord létrehozása](./media/dns-custom-domain/arecord.png)

Miután hello A rekord jön létre, futtassa az `nslookup` toovalidate hello rekord oldja fel.

![nyilvános IP-cím DNS-címkeresés](./media/dns-custom-domain/publicipnslookup.png)

## <a name="app-service-web-apps"></a>Az App Service (webalkalmazások)

hello következő lépések egy app service webalkalmazás az egyéni tartománynév beállítása.

Keresse meg a túl**webes és mobil** > **App Service** válassza ki a konfigurált egy egyéni tartománynevet, és kattintson a hello erőforrás **egyéni tartományok**.

Vegye figyelembe a hello érvényes URL-címet a hello **egyéni tartományok** panelen, ez a cím hello aliasként használatos hello DNS-rekord létrehozása.

![egyéni tartományok panel](./media/dns-custom-domain/url.png)

Nyissa meg a tooyour DNS-zónát, és kattintson a **+ rekordhalmaz**. Töltse ki a következő információ a hello hello **adja hozzá a rekordhalmaz** panel megnyitásához, és kattintson **OK** toocreate azt.


|Tulajdonság  |Érték  |Leírás  |
|---------|---------|---------|
|Név     | mywebserver        | Ezt az értéket, valamint hello tartománynév-címke hello FQDN hello egyéni tartománynév.        |
|Típus     | CNAME        | Használjon egy olyan CNAME rekordot alias használ. Ha hello erőforrás használt IP-cím, egy A rekordot használni.        |
|ÉLETTARTAM     | 1        | 1 használt 1 óra        |
|Élettartam egység     | Óra        | Hello időmérést használatosak üzemideje (óra)         |
|Alias     | WebServer.azurewebsites.NET        | hello DNS-név létrehozásakor hello alias, ebben a példában hello webserver.azurewebsites.net DNS-név alapértelmezett toohello webes alkalmazás által biztosított.        |


![Hozzon létre egy CNAME rekordot](./media/dns-custom-domain/createcnamerecord.png)

Lépjen vissza toohello alkalmazásszolgáltatás, mely hello egyéni tartománynév beállítása. Kattintson a **egyéni tartományok**, majd kattintson a **Hostnames**. a létrehozott tooadd hello CNAME rekordot, kattintson a **+ Hozzáadás állomásnév**.

![1. ábra](./media/dns-custom-domain/figure1.png)

Hello folyamat befejezése után futtassa **nslookup** toovalidate névfeloldás működik.

![1. ábra](./media/dns-custom-domain/finalnslookup.png)

toolearn leképezése egy szolgáltatás, az egyéni tartomány tooApp kapcsolatos további információkért látogasson el [leképezése egy meglévő egyéni DNS nevét tooAzure webalkalmazások](../app-service-web/app-service-web-tutorial-custom-domain.md?toc=%dns%2ftoc.json).

Ha egyéni tartományt toopurchase van szüksége, látogasson el [vásároljon egy egyéni tartománynevet, az Azure Web Apps](../app-service-web/custom-dns-web-site-buydomains-web-app.md) toolearn további információk az App Service-tartományokat.

## <a name="blob-storage"></a>Blob Storage

hello következő lépések egy CNAME rekordot hello asverify metódussal a blob storage-fiók konfigurálása. Ez a módszer biztosítja, hogy nincs állásidő nélkül.

Keresse meg a túl**tárolási** > **Tárfiókok**, válassza ki a tárfiók, és kattintson a **egyéni tartomány**. A 2. lépés FQDN hello notate, ez az érték használható toocreate hello első CNAME rekord

![a BLOB storage egyéni tartományt](./media/dns-custom-domain/blobcustomdomain.png)

Nyissa meg a tooyour DNS-zónát, és kattintson a **+ rekordhalmaz**. Töltse ki a következő információ a hello hello **adja hozzá a rekordhalmaz** panel megnyitásához, és kattintson **OK** toocreate azt.


|Tulajdonság  |Érték  |Leírás  |
|---------|---------|---------|
|Név     | asverify.mystorageaccount        | Ezt az értéket, valamint hello tartománynév-címke hello FQDN hello egyéni tartománynév.        |
|Típus     | CNAME        | Használjon egy olyan CNAME rekordot alias használ.        |
|ÉLETTARTAM     | 1        | 1 használt 1 óra        |
|Élettartam egység     | Óra        | Hello időmérést használatosak üzemideje (óra)         |
|Alias     | asverify.adatumfunctiona9ed.BLOB.Core.Windows.NET        | hello DNS-név létrehozásakor hello alias, ebben a példában hello asverify.adatumfunctiona9ed.blob.core.windows.net DNS-név alapértelmezett toohello tárfiók által biztosított.        |

Keresse meg a visszafelé tooyour tárfiók kattintva **tárolási** > **Tárfiókok**, válassza ki a tárfiók, és kattintson a **egyéni tartomány**. Hello alias létrehozott nélkül hello asverify előtag hello szövegmezőben, ellenőrizze a típushoz ** CNAME rekord közvetett ellenőrzésének használata, és kattintson a **mentése**. Ez a lépés végrehajtása után térjen vissza a tooyour DNS-zónát, és hozzon létre egy CNAME rekordot hello asverify előtag nélkül.  Később biztonságos toodelete hello CNAME rekord hello cdnverify előtaggal rendelkező áll.

![a BLOB storage egyéni tartományt](./media/dns-custom-domain/indirectvalidate.png)

Futtassa a DNS-feloldás eredményének ellenőrzése`nslookup`

egy egyéni tartomány tooa blob storage endpoint leképezési olvashat toolearn [állítson be egy egyéni tartománynevet a Blob storage-végponthoz](../storage/blobs/storage-custom-domain-name.md?toc=%dns%2ftoc.json)

## <a name="azure-cdn"></a>Azure CDN

hello következő lépések egy CNAME rekordot a CDN-végpontok hello cdnverify metódussal konfigurálása. Ez a módszer biztosítja, hogy nincs állásidő nélkül.

Keresse meg a túl**hálózati** > **CDN-profilra**, válassza ki a CDN-profilt, és kattintson a **végpontok** alatt **általános**.

Válassza ki a hello végpont dolgozik, és kattintson a **+ egyéni tartomány**. Megjegyzés: hello **végpont állomásnevéhez** , ez az érték hello rekord adott hello CNAME rekord mutat.

![Egyéni CDN-tartomány](./media/dns-custom-domain/endpointcustomdomain.png)

Nyissa meg a tooyour DNS-zónát, és kattintson a **+ rekordhalmaz**. Töltse ki a következő információ a hello hello **adja hozzá a rekordhalmaz** panel megnyitásához, és kattintson **OK** toocreate azt.

|Tulajdonság  |Érték  |Leírás  |
|---------|---------|---------|
|Név     | cdnverify.mycdnendpoint        | Ezt az értéket, valamint hello tartománynév-címke hello FQDN hello egyéni tartománynév.        |
|Típus     | CNAME        | Használjon egy olyan CNAME rekordot alias használ.        |
|ÉLETTARTAM     | 1        | 1 használt 1 óra        |
|Élettartam egység     | Óra        | Hello időmérést használatosak üzemideje (óra)         |
|Alias     | cdnverify.adatumcdnendpoint.azureedge.NET        | hello DNS-név létrehozásakor hello alias, ebben a példában hello cdnverify.adatumcdnendpoint.azureedge.net DNS-név alapértelmezett toohello tárfiók által biztosított.        |

Keresse meg a CDN-végpont hátsó tooyour kattintva **hálózati** > **CDN-profil**, és válassza ki a CDN-profilt. Kattintson a **+ egyéni tartomány** , és adja meg a CNAME rekord alias hello cdnverify előtag nélkül, és kattintson **Hozzáadás**.

Ez a lépés végrehajtása után térjen vissza a tooyour DNS-zónát, és hozzon létre egy CNAME rekordot hello cdnverify előtag nélkül.  Később biztonságos toodelete hello CNAME rekord hello cdnverify előtaggal rendelkező áll. További információk a CDN hogyan tooconfigure nélkül hello köztes regisztrációs lépésében az egyéni tartománynév felkeresi [tartalom tooa egyéni térkép Azure CDN-tartomány](../cdn/cdn-map-content-to-custom-domain.md?toc=%dns%2ftoc.json).

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan túl[névkeresési DNS az Azure-ban tárolt szolgáltatások konfigurálása](dns-reverse-dns-for-azure-services.md).
