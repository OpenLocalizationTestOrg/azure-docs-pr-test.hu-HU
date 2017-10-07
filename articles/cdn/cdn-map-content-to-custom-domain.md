---
title: "aaaMap Azure CDN-tartalom tooa egyéni tartomány |} Microsoft Docs"
description: "Ismerje meg, hogyan toomap Azure CDN tartalom tooa egyéni tartományt."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d3ee77297f1dd7dbf31a9391191cc2910fbd2cee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="map-azure-cdn-content-tooa-custom-domain"></a>Azure CDN-tartalom tooa egyéni tartomány leképezése
Egy egyéni tartomány tooa CDN-végpont a rendelés toouse URL-címek toocached tartalom azureedge.net altartománya használata helyett a saját tartománynevét is leképezheti.

Nincsenek két módon toomap egyéni tartomány tooa CDN-végpont:

1. [Hozzon létre egy CNAME rekordot a tartományregisztrálónál és hozzárendelését az egyéni tartomány és altartomány toohello CDN-végpont](#register-a-custom-domain-for-an-azure-cdn-endpoint).
   
    Egy CNAME rekordot a rendszer egy DNS-szolgáltatás, amely leképezhető a forrástartomány, például `www.contosocdn.com` vagy `cdn.contoso.com`, tooa céltartományra. Ebben az esetben a forrástartomány hello az egyéni tartomány és altartomány (altartomány, például **www** vagy **cdn** megadása mindig kötelező). hello céltartományra a CDN-végpontot.  
   
    hello folyamat leképezésének egyéni tartomány tooyour CDN-végpont, azonban eredményezhet hello tartomány állásidő rövid idő alatt regisztrál hello tartományhoz az Azure portál hello közben.
2. [Adjon hozzá egy köztes regisztrációs lépést a **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)
   
    Ha az egyéni tartomány jelenleg támogat az alkalmazás a szolgáltatásiszint-szerződés (SLA), amely megköveteli, hogy nincs állásidő nélkül, akkor használhatja a hello Azure **cdnverify** altartomány tooprovide egy köztes regisztrációs lépés, hogy a felhasználók fognak tudni tooaccess közben a tartomány hello DNS leképezési történik.  

Az egyéni tartomány hello fenti eljárások egyikével regisztrálása után érdemes túl[győződjön meg arról, hogy hello egyéni altartomány hivatkozik a CDN-végpontot](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [!NOTE]
> Létre kell hoznia egy CNAME rekordot a tartomány regisztráló toomap tartomány toohello CDN-végpont. A CNAME-rekordok leképezése adott altartományok például `www.contoso.com` vagy `cdn.contoso.com`. Nincs lehetséges toomap egy CNAME rekord tooa gyökértartomány, például a `contoso.com`.
> 
> Altartomány csak társítható egy CDN-végpontot. az Ön által létrehozott CNAME rekord hello átirányítja az összes forgalom fogadását toohello altartomány toohello megadott végpont.  Például, ha társította `www.contoso.com` a CDN-végponthoz, akkor nem kapcsolhat hozzá más Azure-végpontok, például a tárolási fiók végpontját vagy egy felhőalapú szolgáltatás végpontjának. Azonban használhat a különböző altartományok hello a különböző végpontok ugyanabban a tartományban. Különböző altartományok toohello is hozzárendelhető ugyanaz a CDN-végponthoz.
> 
> A **Azure CDN Verizon** (Standard és prémium) végpont, vegye figyelembe, hogy foglal túl**90 percig** egyéni tartomány toopropagate tooCDN peremhálózati csomópontok módosul.
> 
> 

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Egy egyéni tartományt az Azure CDN-végpont regisztrálása
1. Jelentkezzen be a hello [Azure Portal](https://portal.azure.com/).
2. Kattintson a **Tallózás**, majd **CDN-profil**, majd a CDN-profil hello hello végponttal toomap tooa egyéni tartományt szeretne.  
3. A hello **CDN-profil** panelen kattintson kívánja tooassociate hello altartomány hello CDN-végpont.
4. Hello hello végpont panel felső részén kattintson hello **egyéni tartomány hozzáadása** gombra.  A hello **hozzá egyéni tartományt** panelen láthatja hello végpont állomásnevet, a CDN-végpontot, az új CNAME rekord létrehozásával toouse származik. hello állomás címe hello formátumban jelenik meg  **&lt;EndpointName >. azureedge.net**.  A gazdagép neve toouse hello CNAME rekord létrehozásával a másolhatja.  
5. Keresse meg a tooyour tartomány regisztráló webhelyén, és keresse meg a DNS-rekordok létrehozásához hello szakaszt. Ezt a **Tartománynév**, **DNS**, **Névkiszolgáló kezelése** vagy hasonló területen találja.
6. Hello szakaszban található CNAME kezeléséhez. Valószínűleg toogo tooan speciális beállításai oldal és CNAME, az Alias vagy a altartományok hello szavakat keresi.
7. Hozzon létre egy új CNAME rekordot, amely leképezhető a kiválasztott altartomány (például **www** vagy **cdn**) toohello gazdagép szereplő névtől hello **hozzá egyéni tartományt** panelen. 
8. Térjen vissza a toohello **hozzá egyéni tartományt** panelt, és az egyéni tartomány, hello altartomány, beleértve a hello párbeszédpanelen adja meg. Adja meg például hello formátumban hello tartománynév `www.contoso.com` vagy `cdn.contoso.com`.   
   
   Azure lesz győződjön meg arról, hogy létezik-e hello CNAME rekord megadott hello tartománynév. Ha hello CNAME helyességéről, az egyéni tartomány érvényességét.  A **Azure CDN Verizon** (Standard és prémium) végpont, igénybe vehet too90 percig, amíg az egyéni tartomány beállítások toopropagate tooall CDN peremhálózati csomópontok, azonban.  
   
   Vegye figyelembe, hogy egyes esetekben az hello CNAME rekord toopropagate tooname kiszolgálók hello Internet időt vehet igénybe. Ha a tartomány nincs érvényesítve azonnal, és úgy gondolja, hogy a CNAME rekord hello helyességéről, majd várjon néhány percet, és próbálkozzon újra.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-hello-intermediary-cdnverify-subdomain"></a>Regisztrálja az Azure CDN-végponthoz hello közvetítő cdnverify altartomány használatával egyéni tartományt
1. Jelentkezzen be a hello [Azure Portal](https://portal.azure.com/).
2. Kattintson a **Tallózás**, majd **CDN-profil**, majd a CDN-profil hello hello végponttal toomap tooa egyéni tartományt szeretne.  
3. A hello **CDN-profil** panelen kattintson kívánja tooassociate hello altartomány hello CDN-végpont.
4. Hello hello végpont panel felső részén kattintson hello **egyéni tartomány hozzáadása** gombra.  A hello **hozzá egyéni tartományt** panelen láthatja hello végpont állomásnevet, a CDN-végpontot, az új CNAME rekord létrehozásával toouse származik. hello állomás címe hello formátumban jelenik meg  **&lt;EndpointName >. azureedge.net**.  A gazdagép neve toouse hello CNAME rekord létrehozásával a másolhatja.
5. Keresse meg a tooyour tartomány regisztráló webhelyén, és keresse meg a DNS-rekordok létrehozásához hello szakaszt. Ezt a **Tartománynév**, **DNS**, **Névkiszolgáló kezelése** vagy hasonló területen találja.
6. Hello szakaszban található CNAME kezeléséhez. Valószínűleg toogo tooan speciális beállításai oldal és hello szavakat keresi **CNAME**, **Alias**, vagy **altartományok**.
7. Új CNAME rekordot kell létrehozni, és adjon meg egy altartomány alias, amely tartalmazza az hello **cdnverify** altartomány. Például a megadott hello altartomány hello formátumú lesz **cdnverify.www** vagy **cdnverify.cdn**. Adja meg hello állomás neve, amely a CDN-végpontot, hello formátumban **cdnverify.&lt; EndpointName >. azureedge.net**. A DNS-hozzárendelése hasonlóan kell kinéznie:`cdnverify.www.consoto.com   CNAME   cdnverify.consoto.azureedge.net`  
8. Térjen vissza a toohello **hozzá egyéni tartományt** panelt, és az egyéni tartomány, hello altartomány, beleértve a hello párbeszédpanelen adja meg. Adja meg például hello formátumban hello tartománynév `www.contoso.com` vagy `cdn.contoso.com`. Vegye figyelembe, hogy ebben a lépésben nem kell toopreface hello altartomány rendelkező **cdnverify**.  
   
    Azure lesz győződjön meg arról, hogy létezik-e hello CNAME rekord hello cdnverify megadott tartománynév.
9. Ezen a ponton az egyéni tartomány ellenőrzése után az Azure-ban, de a forgalom tooyour tartomány nem még folyamatban van irányított tooyour CDN-végpontot. Elég hosszú tooallow hello egyéni tartomány beállítások toopropagate várakozás után toohello CDN él csomópontok (90 percig **Azure CDN Verizon**, 1-2 percet **Akamai Azure CDN**), tooyour DNS vissza regisztráló webhelyén, és hozzon létre egy másik olyan CNAME rekordot, amely leképezhető nevű altartomány tooyour CDN-végpont. Például adja meg, mint hello altartomány **www** vagy **cdn**, és az állomásnév szerint hello  **&lt;EndpointName >. azureedge.net**. Az ebben a lépésben az egyéni tartomány hello regisztrálása sikeresen befejeződött.
10. Végezetül létrehozott hello CNAME-rekord törlése **cdnverify**, mint csak, egy köztes lépés szükséges.  

## <a name="verify-that-hello-custom-subdomain-references-your-cdn-endpoint"></a>Győződjön meg arról, hogy hello egyéni altartomány hivatkozik a CDN-végpontot
* Az egyéni tartomány hello regisztráció befejezését követően a CDN-végpont hello egyéni tartomány érheti el a gyorsítótárazott tartalmat.
  Először győződjön meg róla, hogy rendelkezik-e, hogy a rendszer gyorsítótárba helyezte hello végpontot a nyilvános tartalmakhoz. Például ha a CDN-végpontot a storage-fiók tartozik, hello CDN gyorsítótárba helyezi azt a nyilvános blob tárolók. tootest hello egyéni tartományt, győződjön meg arról, hogy a tároló tooallow nyilvános hozzáférés van beállítva, és tartalmaz legalább egy blob.
* A böngészőben nyissa meg a hello egyéni tartomány hello blob toohello címét. Például, ha az egyéni tartomány `cdn.contoso.com`, hello URL-cím tooa gyorsítótárazott blob hasonló toohello URL-cím a következő lesz: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Lásd még:
[Hogyan tooEnable hello Content Delivery Network (CDN) az Azure-bA](cdn-create-new-endpoint.md)  

