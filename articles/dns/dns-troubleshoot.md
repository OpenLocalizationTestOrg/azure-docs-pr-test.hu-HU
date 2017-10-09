---
title: "aaaAzure DNS-hibaelhárítási útmutatója |} Microsoft Docs"
description: "Hogyan tootroubleshoot közös Azure DNS-problémák"
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: 95b01dc3-ee69-4575-a259-4227131e4f9c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/20/2017
ms.author: jonatul
ms.openlocfilehash: 944aa1811c980063f739268cd2c79b647b2754a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-troubleshooting-guide"></a>Az Azure DNS-hibaelhárítási útmutatója

Ezen a lapon hibaelhárításával kapcsolatos kérdésekre Azure DNS-ben.

Ha ezeket a lépéseket nem sikerül megoldani a problémát, keresse meg vagy is a probléma írjon a [közösségi támogatási fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork). Azt is megteheti nyissa meg az Azure támogatási kérelmet.


## <a name="i-cant-create-a-dns-zone"></a>Nem lehet létrehozni egy DNS-zóna

tooresolve gyakori problémákat, próbálja meg egy vagy több hello a következő lépéseket:

1.  Felülvizsgálati hello Azure DNS naplózási toodetermine hello hiba okát is megadva naplózza.
2.  Minden DNS-zónanévnek egyedinek kell lennie a saját erőforráscsoportján belül. Ez azt jelenti, hogy két DNS-zóna hello az azonos név nem lehet megosztani az egy erőforráscsoport. Próbáljon meg eltérő zónanevet vagy erőforráscsoportot használni.
3.  Hiba jelenhet meg, "Elérte vagy túllépte a zónák az előfizetés {előfizetés-azonosító} hello maximális számát." Vagy egy másik Azure-előfizetéssel, törölje néhány zónákat, vagy forduljon Azure támogatási tooraise az előfizetési határértéket.
4.  Előfordulhat, hogy tekintse meg a hiba "hello zóna ({name} zóna) nem áll rendelkezésre." Ez a hiba azt jelenti, hogy Azure DNS-ben a DNS-zóna névkiszolgálóit nem tooallocate. Próbáljon meg egy másik zónanevet használni. Azt is megteheti Ha hello tartomány neve tulajdonos, lépjen kapcsolatba az Azure-támogatás, akik névkiszolgálók foglalhatja meg.


### <a name="recommended-documents"></a>**Ajánlott dokumentumok**

[DNS-zónák és -rekordok](dns-zones-records.md)
<br>
[DNS-zóna létrehozása](dns-getstarted-create-dnszone-portal.md)

## <a name="i-cant-create-a-dns-record"></a>Nem tudok létrehozni egy DNS-rekordot

tooresolve gyakori problémákat, próbálja meg egy vagy több hello a következő lépéseket:

1.  Felülvizsgálati hello Azure DNS naplózási toodetermine hello hiba okát is megadva naplózza.
2.  Már létezik hello rekordhalmaz?  Az Azure DNS-rekordok rekord kezeli *beállítja*, melyek azonos nevet, és azonos hello hello rekordjának hello gyűjteménye típusa. Ha egy rekord, hello azonos nevet, és írja be a már létezik, majd tooadd egy másik ilyen rekord meglévő hello kell szerkeszteni rekordhalmaz.
3.  Próbálja toocreate egy kötetet a következő hello DNS zóna felső pontja (hello "Gyökér" hello zóna)? Ha igen, a DNS egyezmény hello toouse hello "@" karakter hello rekord neveként. Ne feledje, hogy hello DNS-szabványokból nem teszik lehetővé a CNAME-rekordok hello zóna csúcsán.
4.  CNAME-ütközést tapasztal?  hello DNS-szabványokból nem teszik lehetővé az azonos név bármilyen más típusú rekordként hello egy CNAME rekordot. Ha egy meglévő CNAME rekord létrehozásával a hello nevétől eltérő típusú sikertelen.  Hasonlóképpen egy olyan CNAME létrehozása sikertelen lesz, ha hello neve megegyezik egy eltérő típusú létező rekordot. Távolítsa el a hello ütközés hello eltávolításával más rekord vagy egy másik rekord név kiválasztásában.
5.  Elérte hello engedélyezett DNS-zóna a rekordhalmazok hello számára vonatkozó korlátozást? rekordhalmazok aktuális száma hello és hello rekordhalmazok maximális száma látható hello a hello zóna "Tulajdonságok" hello Azure-portálon. Ha elérte ezt a határt, majd vagy néhány rekordhalmazok törlése, vagy forduljon az Azure támogatási tooraise rekordhalmaz korlát a zóna, majd próbálja meg újra. 


### <a name="recommended-documents"></a>**Ajánlott dokumentumok**

[DNS-zónák és -rekordok](dns-zones-records.md)
<br>
[DNS-zóna létrehozása](dns-getstarted-create-dnszone-portal.md)



## <a name="i-cant-resolve-my-dns-record"></a>Nem tudom feloldani a DNS-rekordomat

A DNS-névfeloldás egy többlépéses folyamat, amely több okból is meghiúsulhat. hello következő lépések segítségével megvizsgálhatja, hogy miért üzemeltetett az Azure DNS-zóna a DNS-rekord a DNS-feloldás sikertelen.

1.  Győződjön meg arról, hogy hello DNS-rekordok vannak konfigurálva megfelelően Azure DNS-ben. Tekintse át a DNS-rekordok hello hello Azure-portálon ellenőrzése hello Zónanév, rekord neve és típusa helyesek.
2.  Győződjön meg arról, hogy hello DNS-rekordok megfelelően feloldani hello Azure DNS névkiszolgálóit.
    - Ha DNS-lekérdezések a helyi számítógépről, gyorsítótárazott eredményt tükröző hello névkiszolgálók hello jelenlegi állapotában nem jelenhet meg.  Emellett a vállalati hálózatok gyakran használják DNS-proxy kiszolgálók, ami megakadályozhatja, DNS-lekérdezések toospecific névkiszolgálók irányítva.  tooavoid ezek a problémák, használja a web-alapú névfeloldási szolgáltatásnál például [digwebinterface](http://digwebinterface.com).
    - Kell, hogy toospecify hello megfelelő névkiszolgálók a DNS-zóna látható módon hello Azure-portálon.
    - Ellenőrizze, hogy hello DNS-név helyes (kell toospecify hello teljesen minősített neve hello Zónanév együtt), hello rekordtípus helyességéről.
3.  Győződjön meg arról, hogy hello DNS-neve megfelelően lett-e [toohello Azure DNS névkiszolgálóit meghatalmazott](dns-domain-delegation.md). [Számos, harmadik féltől származó webhely létezik, amely lehetővé teszi a DNS-delegálás ellenőrzését](https://www.bing.com/search?q=dns+check+tool). Ez a teszt egy *zóna* delegálási tesztet, ezért kell csak megadni hello DNS-zóna nevét, és nem hello teljesen minősített bejegyzés neve.
4.  A fenti hello befejezését követően, a DNS-rekord most oldja fel az megfelelően. újra használható tooverify, [digwebinterface](http://digwebinterface.com), ezúttal hello alapértelmezett neve beállításait.


### <a name="recommended-documents"></a>**Ajánlott dokumentumok**

[A tartomány tooAzure DNS delegálása](dns-domain-delegation.md)



## <a name="how-do-i-specify-hello-service-and-protocol-for-an-srv-record"></a>Hogyan meg hello "szolgáltatás" és "protokoll", az SRV-rekordot?

Az Azure DNS rekordhalmazok mint DNS-rekordok kezelése – hello bejegyzések gyűjteményét hello azonos nevet, és hello ugyanaz a típusa. Az SRV rekordkészlete hello "szolgáltatás" és "protokoll" kell hello rekordhalmaz nevének részeként megadott toobe. hello SRV megadva más paraméter ("priority", "súly", "port" és "target") külön-külön az egyes rekordokhoz a hello rekordhalmaz.

Példák az SRV-rekordnevekre („sip” szolgáltatásnév, „tcp” protokoll):

- \_a SIP. \_tcp (létrehoz egy rekordot, állítsa be megfelelően hello zóna felső pontja)
- \_sip.\_tcp.sipservice (egy „sipservice” nevű rekordhalmazt hoz létre)

### <a name="recommended-documents"></a>**Ajánlott dokumentumok**

[DNS-zónák és -rekordok](dns-zones-records.md)
<br>
[DNS-rekordhalmazok és rekordok létrehozása hello Azure-portál használatával](dns-getstarted-create-recordset-portal.md)
<br>
[SRV rekordtípus (Wikipédia)](https://en.wikipedia.org/wiki/SRV_record)


## <a name="next-steps"></a>Következő lépések

* További tudnivalók [Azure DNS-zónák és rekordok](dns-zones-records.md)
* az Azure DNS használatával toostart megtudhatja, hogyan túl[hozzon létre egy DNS-zóna](dns-getstarted-create-dnszone-portal.md) és [DNS-rekordok létrehozása](dns-getstarted-create-recordset-portal.md).
* egy meglévő DNS-zóna toomigrate megtudhatja, hogyan túl[importálni és exportálni egy DNS-zónafájlját](dns-import-export.md).

