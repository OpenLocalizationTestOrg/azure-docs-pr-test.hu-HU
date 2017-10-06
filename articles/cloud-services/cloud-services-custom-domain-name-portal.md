---
title: "Cloud Services egy egyéni tartománynevet aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooexpose az Azure alkalmazás vagy adatok toohello internetes DNS-beállítások konfigurálásával olyan egyéni tartományhoz.  Ezekben a példákban hello Azure-portálon."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 5783a246-a151-4fb1-b488-441bfb29ee44
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: a0f3186b6022fbc4570ef1ce4b921426842bde76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Egy egyéni tartománynevet, az Azure-felhőszolgáltatás konfigurálása
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-custom-domain-name-portal.md)
> * [klasszikus Azure portál](cloud-services-custom-domain-name.md)
> 
> 

Amikor létrehoz egy felhőalapú szolgáltatás, Azure rendeli tooa altartománya **cloudapp.net**. Például, ha a Felhőszolgáltatás neve "contoso", a felhasználók fog kell tudni tooaccess http://contoso.cloudapp.net URL-az alkalmazást. Azure is hozzárendel egy virtuális IP-címet.

Azonban Ön is is elérhetővé teheti az alkalmazás a saját tartománynevét, például a **contoso.com**. Ez a cikk azt ismerteti, hogyan tooreserve vagy állítson be egy egyéni tartománynevet a felhőalapú szolgáltatás webes szerepkörök.

Tegye meg már megismeréséhez CNAME és A rekordok? [Ugrás túli hello magyarázat](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> Ebben a feladatban hello eljárások tooAzure Felhőszolgáltatások alkalmazni. Alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-custom-domain-name.md). Storage-fiókok, lásd: [ez](../storage/blobs/storage-custom-domain-name.md).
> 
> 

<p/>

> [!TIP]
> Gyorsabb--induláshoz használható hello új Azure [forgatókönyv interaktív](http://support.microsoft.com/kb/2990804)!  Így a egy egyéni tartománynevet és biztonságossá tétele kommunikációhoz (SSL) társítása Azure Cloud Services vagy az Azure Websitesra egy beépülő.
> 
> 

## <a name="understand-cname-and-a-records"></a>CNAME és az A rekordok ismertetése
CNAME (vagy alias rekordokat) és A rekordok is lehetővé teszik a tartomány nevét, egy adott kiszolgálóval tooassociate (vagy szolgáltatás ebben az esetben) azonban ezek eltérően működik. Nincsenek is figyelembe kell venni néhány bizonyos Azure Cloud serviceshez, mely toouse dönt előtt figyelembe kell venni A rekordok használatával.

### <a name="cname-or-alias-record"></a>CNAME- vagy aliasnév rekord
A CNAME rekord rendeli hozzá egy *adott* tartomány, például a **contoso.com** vagy **www.contoso.com**, tooa kanonikus tartomány nevét. Ebben az esetben hello kanonikus tartománynév hello **[myapp] .cloudapp .net** tartománynevét, az Azure futó alkalmazás. Létrehozása után hello CNAME létrehoz egy aliast hello **[myapp] .cloudapp .net**. hello CNAME bejegyzés megoldja toohello IP-címét a **[myapp] .cloudapp .net** szolgáltatás automatikusan, így ha megváltozik a hello IP-cím hello felhőalapú szolgáltatás, nem kell tootake bármilyen műveletet.

> [!NOTE]
> Néhány tartomány regisztráló szervezetek csak teszi toomap altartományok egy olyan CNAME rekordot, például www.contoso.com és a nem gyökér neve, például contoso.com használatakor. A regisztráló által biztosított hello dokumentációjában talál további információt a CNAME-rekordot, [Wikipedia bejegyzés a CNAME rekord hello](http://en.wikipedia.org/wiki/CNAME_record), vagy hello [IETF tartománynév - megvalósítása és meghatározása](http://tools.ietf.org/html/rfc1035) a dokumentum.
> 
> 

### <a name="a-record"></a>Egy rekord
Egy *A* rekord rendeli hozzá a tartományhoz, például a **contoso.com** vagy **www.contoso.com**, *vagy helyettesítő tartomány* például  **\*. contoso.com**, tooan IP-címet. Azure Cloud Service hello esetben hello hello szolgáltatás virtuális IP-cím. Így hello fő előnye, hogy az A rekord keresztül egy olyan CNAME rekordot, akkor használja, mint a helyettesítő karakter, egy vagy több bejegyzése \* **. contoso.com**, amely volna tanúsítványigénylések több altartományok például  **mail.contoso.com**, **login.contoso.com**, vagy **www.contso.com**.

> [!NOTE]
> Mivel az A rekord le van képezve tooa statikus IP-cím, nem tudják automatikusan feloldani a felhőalapú szolgáltatás módosítások toohello IP-címét. a felhőalapú szolgáltatás által használt hello IP-cím van lefoglalva a hello először telepít tooan üres helyre (éles vagy átmeneti.) Hello tárolóhely hello központi telepítésének törlésekor Azure által kiadott hello IP-címet, és a jövőben a központi telepítésekre toohello tárolóhely új IP-cím is megadható.
> 
> Állandó kényelmesen, egy adott üzembe helyezési pontjának (üzemi és átmeneti) hello IP-címét a csere közötti átmeneti és üzemi környezetek vagy egy meglévő központi telepítési helyben történő frissítése során. Ezek a műveletek végrehajtásával további információkért lásd: [hogyan toomanage felhőalapú szolgáltatások](cloud-services-how-to-manage.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Adjon hozzá egy CNAME rekordot, az egyéni tartományhoz
toocreate egy olyan CNAME rekordot, hozzá kell adnia egy új bejegyzést hello DNS-tábla az egyéni tartomány a regisztráló által biztosított hello eszközökkel. Minden tartományregisztráló egy CNAME rekordot a telepítésükhöz hasonló, csak metódust tartalmaz, de koncepciók hello hello azonos.

1. Ezen módszerek toofind hello valamelyikével **. cloudapp.net** hozzárendelt tooyour felhőalapú szolgáltatás, a tartomány nevét.
   
   * Bejelentkezési toohello [Azure-portálon], válassza ki a felhőalapú szolgáltatás, nézze meg hello **Essentials** szakaszt, és keresse meg hello **webhely URL-címe** bejegyzés.
     
       ![gyors áttekintő szakasz megjelenítő hello webhely URL-címe][csurl]
     
       **VAGY**
   * Telepítse és konfigurálja [Azure Powershell](/powershell/azure/overview), és majd a hello használata a következő parancsot:
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     Mentse a hello tartománynevet hello URL-címet, vagy a metódus által visszaadott, szüksége lesz rá egy olyan CNAME rekordot létrehozásakor.
2. Jelentkezzen be tooyour DNS regisztráló webhelyén, és lépjen toohello lapra DNS kezeléséhez. Keressen hivatkozásokat vagy hello hely területek címkézve **tartománynév**, **DNS**, vagy **neve kezelő**.
3. Most már található, ahol válassza ki vagy adja meg a CNAME meg. Előfordulhat, hogy egy legördülő menüben írja be, vagy válassza a Speciális beállítások lap tooan tooselect hello rekord. Hello szavak kell keresnie **CNAME**, **Alias**, vagy **altartományok**.
4. Meg kell adni hello tartomány és altartomány alias a hello CNAME, például a **www** Ha azt szeretné, toocreate alias **www.customdomain.com**. Ha szeretne toocreate alias hello gyökértartomány, akkor elérhetőként hello "**@**" szimbólumot lát, az a regisztráló DNS-eszközök.
5. Ezt követően meg kell adnia egy kanonikus nevet, amely az alkalmazás **cloudapp.net** tartomány ebben az esetben.

Például a CNAME rekordot a következő hello származó összes forgalmat továbbítja **www.contoso.com** túl**contoso.cloudapp.net**, a telepített alkalmazás hello egyéni tartomány nevét:

| Alias és a gazdagép neve/altartomány | Canonical tartomány |
| --- | --- |
| www |contoso.cloudapp.NET |

> [!NOTE]
> A látogatói **www.contoso.com** nem veszi észre hello igaz állomás (contoso.cloudapp.net), így folyamat továbbítási hello láthatatlan toothe végfelhasználói.
> 
> hello példa a fenti csak érvényes tootraffic: hello **www** altartomány. Helyettesítő karakterek nem használhatja a CNAME-rekordot, mert minden tartomány/altartomány egy CNAME REKORDOT kell létrehoznia. Toodirect forgalmát altartományt, például szeretné *. contoso.com, tooyour cloudapp.net cím, konfigurálhat egy **átirányítási URL-cím** vagy **előre URL-cím** bejegyzést a DNS-beállításokat, vagy hozzon létre egy A rekordot.
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a>Az egyéni tartomány az A rekord hozzáadása
toocreate egy A rekordot, először keresse meg a felhőalapú szolgáltatás hello virtuális IP-címét. Majd új bejegyzés hozzáadása az egyéni tartomány hello DNS táblázatban a regisztráló által biztosított hello eszközökkel. Minden tartományregisztráló egy A rekordot telepítésükhöz hasonló, csak metódust tartalmaz, de koncepciók hello hello azonos.

1. A következő módszerek tooget hello IP-címet a felhőszolgáltatás hello egyikét használhatja.
   
   * Bejelentkezési toohello [Azure-portálon], válassza ki a felhőalapú szolgáltatás, nézze meg hello **Essentials** szakaszt, és keresse meg hello **nyilvános IP-címek** bejegyzés.
     
       ![hello VIP megjelenítő gyors áttekintő szakasz][vip]
     
       **VAGY**
   * Telepítse és konfigurálja [Azure Powershell](/powershell/azure/overview), és majd a hello használata a következő parancsot:
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     Hello IP-cím mentése szüksége lesz rá egy A rekordot létrehozásakor.
2. Jelentkezzen be tooyour DNS regisztráló webhelyén, és lépjen toohello lapra DNS kezeléséhez. Keressen hivatkozásokat vagy hello hely területek címkézve **tartománynév**, **DNS**, vagy **neve kezelő**.
3. Most már található, ahol válasszon vagy adjon meg egy olyan rekordot. Előfordulhat, hogy egy legördülő menüben írja be, vagy válassza a Speciális beállítások lap tooan tooselect hello rekord.
4. Válassza ki, vagy adja meg a hello tartomány vagy altartományt, amelyet ez A rekord fog használni. Válassza például **www** Ha azt szeretné, toocreate alias **www.customdomain.com**. Ha egy helyettesítő karakteres bejegyzés toocreate altartományokkal szeretne, adjon meg "x". Ez például lefedi altartományokkal **mail.customdomain.com**, **login.customdomain.com**, és **www.customdomain.com**.
   
    Hello gyökértartomány toocreate egy A rekordot szeretne, ha azt elérhetőként hello "**@**" szimbólumot lát, az a regisztráló DNS-eszközök.
5. Adja meg a felhőszolgáltatás hello IP-cím mezőben megadott hello. Ez társít hello tartomány bejegyzés hello A rekordban a cloud service-környezet hello IP-címmel.

Például a rekord a következő hello származó összes forgalmat továbbítja **contoso.com** túl**137.135.70.239**, IP-címét a telepített alkalmazás hello:

| Gazdagép neve/altartomány | IP-cím |
| --- | --- |
| @ |137.135.70.239 |

Ez a példa azt mutatja be, hello legfelső szintű tartomány az A rekord létrehozása. Ha egy helyettesítő karakteres bejegyzés toocover kívánja toocreate altartományokkal, adjon meg "x", hello altartomány.

> [!WARNING]
> IP-címek az Azure-ban alapértelmezés szerint dinamikus. Toouse érdemes egy [fenntartott IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure, amely az IP-címe nem változik.
> 
> 

## <a name="next-steps"></a>Következő lépések
* [TooManage Cloud Services](cloud-services-how-to-manage.md)
* [Hogyan tooMap CDN tartalom tooa egyéni tartományhoz](../cdn/cdn-map-content-to-custom-domain.md)
* [A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).
* Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).
* Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates hello subdomain with hello storage account]: #create-cname
[Azure-portálon]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
