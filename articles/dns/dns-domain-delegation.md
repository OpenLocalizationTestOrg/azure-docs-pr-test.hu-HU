---
title: "aaaAzure DNS-delegálás áttekintése |} Microsoft Docs"
description: "Ismerje meg, hogyan toochange tartományok delegálását és használata Azure DNS neve kiszolgálók tooprovide tartomány üzemeltetéséhez."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.openlocfilehash: eaf2d2e345004b4d631e8d81d548b8ca23277d05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delegation-of-dns-zones-with-azure-dns"></a>DNS-zónák delegálása az Azure DNS-sel

Az Azure DNS lehetővé teszi a DNS-zóna toohost, és egy tartományhoz az Azure-ban hello DNS-rekordok kezelése. Ahhoz, hogy a tartomány tooreach Azure DNS-ben a DNS-lekérdezések, hello tartomány rendelkezik toobe hello szülőtartomány tooAzure DNS átadva. Ne feledje Azure DNS nincs hello tartományregisztráló. Ez a cikk azt ismerteti, hogyan tartománydelegálás működését, és hogyan toodelegate tartományok tooAzure DNS.

## <a name="how-dns-delegation-works"></a>A DNS-delegálás működése

### <a name="domains-and-zones"></a>Tartományok és zónák

hello tartománynévrendszer tartományok hierarchiájából áll. hello hierarchia első eleme hello "gyökértartomány", amelynek neve egyszerűen "**.**".  Ez alatt találhatók a legfelső szintű tartományok, mint a „com”, a „net”, az „org”, az „uk” vagy a „jp”.  A legfelső szintű tartományok alatt találhatók a másodlagos szintű tartományok, mint az „org.uk” vagy a „co.jp”.  És így tovább. a DNS-hierarchiában hello hello tartományok különálló DNS-zónák üzemelteti. A zónák globálisan fel vannak osztva, DNS-névkiszolgálók hello világ üzemelteti.

**DNS-zóna** -tartomány az egyedi nevek a tartománynévrendszerben, például "contoso.com" hello. A DNS-zónák használt toohost hello DNS-rekordok az adott tartományban. Hello "contoso.com" tartomány például "mail.contoso.com" (levelezési kiszolgálóhoz) és "www.contoso.com" (webhelyhez) például számos DNS-rekordokat is tartalmazhat.

**Tartományregisztráló** – A tartományregisztráló egy olyan cég, amely internetes tartományneveket biztosít. Ezek ellenőrzik, hogy ha hello internetes tartomány toouse érhető el, és lehetővé teszik toopurchase azt. Miután hello tartománynév van regisztrálva, hello jogi tulajdonos hello tartománynév áll. Ha már van internetes tartománya, hello aktuális tartomány regisztráló toodelegate tooAzure DNS fogja használni.

További információt a ki az adott tartománynév tulajdonosa, vagy kapcsolatos információk toofind toobuy egy tartományhoz, lásd: [internetes tartományok az Azure AD](https://msdn.microsoft.com/library/azure/hh969248.aspx).

### <a name="resolution-and-delegation"></a>Feloldás és delegálás

Kétféle DNS-kiszolgáló létezik:

* A *mérvadó* DNS-kiszolgáló üzemelteti a DNS-zónákat. Csak az ezekben a zónákban található rekordokra irányuló DNS-lekérdezéseket válaszolja meg.
* A *rekurzív* DNS-kiszolgáló nem üzemeltet DNS-zónákat. DNS-lekérdezéseket válaszolja meg mérvadó DNS-kiszolgálók toogather hello adatok kell meghívásával.

Az Azure DNS mérvadó DNS szolgáltatást nyújt.  Rekurzív DNS szolgáltatást nem biztosít. Felhőalapú szolgáltatásokhoz és az Azure virtuális gépek a rekurzív DNS-szolgáltatás külön Azure infrastruktúrája részeként biztosított automatikusan konfigurált toouse. Hogyan toochange ezeket a DNS-beállításokat: kapcsolatos [névfeloldás az Azure-ban](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

A számítógépek vagy mobileszközök a DNS-ügyfelek általában hívható meg egy rekurzív DNS-kiszolgáló tooperform hello ügyfélalkalmazások számára szükséges DNS-lekérdezések.

Amikor egy rekurzív DNS-kiszolgáló egy DNS-rekordot, például "www.contoso.com" vonatkozó lekérdezést kap, először meg kell toofind hello neve server üzemeltetési hello zóna hello "contoso.com" tartomány. toofind hello névkiszolgáló, akkor hello gyökérkiszolgálók neve kezdődik, és névkiszolgálótól kiindulva megkeresi a hello hello "com" zónát üzemeltető névkiszolgálókat. Ezután lekérdezi hello "com" név kiszolgálók toofind hello üzemeltető névkiszolgálókat hello "contoso.com" zóna.  Végezetül fontos képes tooquery ezek névkiszolgálóit "www.contoso.com".

Ez az eljárás neve hello DNS-név feloldása. Szigorúan fogalmazva a DNS-feloldás más lépéseket például a CNAME-rekordok követését is tartalmaz, de ez nem fontos toounderstanding DNS-delegálás működése.

Hogyan nem a szülő "mutat rá" toohello névkiszolgálók gyermekzóna? Ezt egy speciális DNS-rekorddal, az úgynevezett névkiszolgálói rekorddal hajtja végre. Hello gyökérzóna például a "com" Névkiszolgálói rekordokat tartalmaz, és hello "com" zóna névkiszolgálóit hello jeleníti meg. Hello "com" zóna pedig tartalmazza a Névkiszolgálói rekordokat a "contoso.com", amely hello "contoso.com" zóna névkiszolgálóit hello mutatja. Egy szülőzónán belüli gyermekzóna Névkiszolgálói rekordjainak hello beállítása felhatalmazó hello tartomány neve.

kép a következő hello látható egy példa DNS-lekérdezést. hello contoso.net és partners.contoso.net az Azure DNS-zónák.

![DNS-névkiszolgáló](./media/dns-domain-delegation/image1.png)

1. ügyfélkérések hello `www.partners.contoso.net` a helyi DNS-kiszolgálóról.
1. hello helyi DNS-kiszolgáló nem rendelkezik hello rekord, a kérelem tootheir gyökér neve kiszolgáló teszi.
1. hello gyökérkiszolgáló neve nincs hello rekord, de tudja hello hello címe `.net` kiszolgáló nevét, biztosítja, hogy cím toohello DNS-kiszolgáló
1. hello DNS küldi hello kérelem toohello `.net` névkiszolgáló, nincs hello rekord azonban hello contoso.net névkiszolgáló hello címét. Ebben az esetben ez egy DNS-zóna, amely az Azure DNS-en fut.
1. hello zóna `contoso.net` hello rekord rendelkezik, de tudja a névkiszolgáló hello `partners.contoso.net` és, hogy válaszol. Ebben az esetben ez egy DNS-zóna, amely az Azure DNS-en fut.
1. hello DNS-kiszolgáló IP-címe hello kérelmek `partners.contoso.net` a hello `partners.contoso.net` zóna. Hello A rekord tartalmazza, és hello IP-cím válaszol.
1. hello DNS-kiszolgáló látja el hello IP cím toohello ügyfelet
1. hello ügyfél kapcsolódik toohello webhely `www.partners.contoso.net`.

Összes delegálás rendelkezik-e két példányban hello Névkiszolgálói rekordokat; egy-egy hello gyermekzónára toohello, egy pedig a hello gyermekzónát magát. hello "contoso.net" zónához hello Névkiszolgálói rekordokat a "contoso.net" (a hozzáadása toohello Névkiszolgálói rekordokat a "net") tartalmaz. Ezeket a rekordokat hívják mérvadó Névkiszolgálói rekordokat, és a gyermekzóna hello gyermekzónát hello legfelső pontján.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan túl[delegálása a tartományi tooAzure DNS](dns-delegate-domain-azure-dns.md)

