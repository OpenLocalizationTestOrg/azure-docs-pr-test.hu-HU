---
title: "404-es állapotot visszaadó aaaTroubleshooting Azure CDN-végpontok |} Microsoft Docs"
description: "404-es válaszkódot az Azure CDN-végpontok hibaelhárítása."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 450bfbd641c869cfd88169a12c4b69819eaa7c26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>404-es állapotot visszaadó CDN-végpontok hibaelhárítása
Ez a cikk segít elhárítása [CDN-végpontok](cdn-create-new-endpoint.md) 404-es hibát ad vissza.

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure hello és fórumok Stack Overflow hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl is az Azure támogatási incidens. Nyissa meg toohello [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , majd kattintson a **támogatás**.

## <a name="symptom"></a>Jelenség
Létrehozta a CDN-profil és egy végpontot, de a tartalom úgy tűnik, toobe hello CDN érhető el.  Kísérlet történt a tartalom keresztül hello CDN URL-címet kap HTTP 404-es állapotkód tooaccess felhasználók. 

## <a name="cause"></a>Ok
Van több lehetséges oka, beleértve:

* hello fájl eredetében nem látható toohello CDN
* hello végpont helytelenül van konfigurálva, ami hello CDN toolook hello megfelelő helyen
* hello állomás visszautasítja a hello állomásfejléc a CDN hello
* hello végpont még nem volt idő toopropagate teljes hello CDN

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések
> [!IMPORTANT]
> A CDN-végpont létrehozása után azt nem azonnal elérhetővé válik, használatra hello regisztrációs toopropagate keresztül hello CDN időt vesz igénybe.  A <b>Akamai Azure CDN</b> -profilok propagálása általában befejezi egy percen belül.  A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.  Ha hello lépésekkel ezen dokumentum, és továbbra is kap 404-es válaszkódot, néhány óra toocheck újra egy támogatási jegy megnyitása előtt várja meg.
> 
> 

### <a name="check-hello-origin-file"></a>Ellenőrizze a hello forrásfájl
Először ellenőrizze hello gyorsítótárazott szeretnénk hello fájl az eredeti érhető el, és nyilvánosan elérhető.  hello leggyorsabb módon toodo, amely egy része az InPrivate- vagy Incognito munkamenetben tooopen egy böngészőt, és keresse meg közvetlenül toohello fájlt.  Csak írja be vagy hello cím mezőbe illessze be hello URL-címet, és tekintse meg, ha, amely várhatóan hello fájl okoz.  Ehhez a példához fogom toouse van egy Azure Storage-fiók esetén érhető el, a fájl `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Ahogy látja, hello teszt sikeresen továbbítja.

![Sikerült!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Amíg ez a leggyorsabb hello és legegyszerűbb módja tooverify a fájl nyilvánosan elérhető, a szervezet bizonyos hálózati konfigurációk adhat meg, hogy a fájl esetén nyilvánosan elérhető legyen, tulajdonképpen csak látható toousers, a hálózati (érhet hello akkor is, ha azt tárolja az Azure-ban).  Ha egy külső böngészőt, ahonnan tesztelheti, ilyen például a mobileszközök, amelyek nem csatlakozó tooyour vállalati hálózaton, vagy egy virtuális gép az Azure-ban, amely lenne ajánlott.
> 
> 

### <a name="check-hello-origin-settings"></a>Hello forrás beállításainak ellenőrzése
Most, hogy azt ellenőrzése hello fájl nyilvánosan elérhető internetes hello, nem kell ellenőrizni a forrás beállításainak.  A hello [Azure Portal](https://portal.azure.com), keresse meg a tooyour CDN-profilt, és kattintson a hello végpont meg elhárítani.  A hello eredő **végpont** panelen kattintson hello forrás.  

![A kijelölt forrás végpont panelről](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Hello **származási** panel jelenik meg. 

![Forrás panel](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Forrása típusa és az állomásnév
Ellenőrizze a hello **forrása típusa** javítsa ki, és ellenőrizze a hello **forrásállomásnév**.  A példában `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, URL-cím hello hello hostname része `cdndocdemo.blob.core.windows.net`.  Ahogyan azt láthatja hello a képernyőfelvételen látható helyes.  Az Azure Storage, Web App és a felhőalapú szolgáltatás források, hello **forrásállomásnév** mező értéke egy legördülő menüben, így nem szükséges tooworry kapcsolatos helyesírás-megfelelően.  Azonban ha egy egyéni forrás használ, van *feltétlenül kritikus* helyesen adta meg az állomásnevet!

#### <a name="http-and-https-ports"></a>A HTTP és HTTPS-portok
hello más dolog toocheck itt van a **HTTP** és **HTTPS-portok**.  A legtöbb esetben 80-as és 443-as helyesek, és nem lesz szükség.  Azonban ha hello eredeti kiszolgálón egy másik portot figyeli, amely kell toobe, melyet itt.  Ha nem biztos benne, csak tekintse meg az eredeti fájl hello URL-címe.  hello HTTP és HTTPS specifikációk adja meg a 80-as és 443-as portot hello alapértelmezett értéke. Az URL-címben `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, a port nincs megadva, ezért feltételezett hello alapértelmezett 443-as és a beállítások helyesek.  

Azonban mondja ki hello URL-cím, az eredeti fájl, amely korábban tesztelt érték `http://www.contoso.com:8080/file.txt`.  Megjegyzés: hello `:8080` hello állomásnév szegmens hello végén.  Mely jelzi, hogy hello böngésző toouse port `8080` tooconnect toohello helyen `www.contoso.com`, ezért szüksége hello a 8080-as tooenter **HTTP-port** mező.  Fontos, hogy ezeket a portbeállításokat csak hatással milyen port hello végpont tooretrieve adatokat hello forrásból használja toonote.

> [!NOTE]
> **Akamai Azure CDN** végpontok nem teszik lehetővé az hello teljes TCP-porttartomány esetén a források számára.  A nem engedélyezett forrásportok listáját lást: [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx) (Az Akamai Azure CDN engedélyezett forrásportjai).  
> 
> 

### <a name="check-hello-endpoint-settings"></a>Ellenőrizze a hello végpont beállításait
Vissza a hello **végpont** panelen kattintson hello **konfigurálása** gombra.

![Konfigurálás gomb végpont panelről](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

végpont hello **konfigurálása** panel jelenik meg.

![Konfigurálja a panelen](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokollok
A **protokollok**, győződjön meg arról, hogy hello ügyfelek által használt hello protokoll van kiválasztva.  hello hello ügyfél használja ugyanazt a protokollt lesz hello használt egyik tooaccess hello forrása, ezért fontos toohave hello forrásportok hello előző szakaszban megfelelően konfigurálva.  hello végpont csak hello alapértelmezett HTTP és HTTPS-portok (80-as és 443-as), függetlenül hello forrásportok figyel.

Most vissza tooour képzeletbeli példában a `http://www.contoso.com:8080/file.txt`.  Felejtse el, a Contoso megadott `8080` mint a HTTP porttal, de tegyük is fel azokat a megadott `44300` a HTTPS-portjaként működik.  Ha nevű végpont hozza létre őket `contoso`, a CDN-végpont állomásnevéhez lenne `contoso.azureedge.net`.  Kérelem `http://contoso.azureedge.net/file.txt` egy HTTP-kérelem van, így a hello végpont HTTP használna a 8080-as porton tooretrieve azt hello a forrásból.  A HTTPS PROTOKOLLOKON keresztül biztonságos kérelmet `https://contoso.azureedge.net/file.txt`, okozna hello végpont toouse HTTPS port 44300 amikor beolvasni hello fájl hello a forrásból.

#### <a name="origin-host-header"></a>Forrás állomásfejlécét
Hello **forrás állomásfejlécét** van hello küldött állomásfejléc-érték minden egyes kérelemmel toohello forrása.  A legtöbb esetben ez kell kell hello ugyanaz, mint hello **forrásállomásnév** azt korábban ellenőrzése.  Ebben a mezőben értékkel általában nem okoz a 404-es állapotot, de valószínűleg toocause többi 4xx állapot, attól függően, hogy milyen hello származási vár.

#### <a name="origin-path"></a>Forrás elérési útvonalának
Végül ellenőrizze azt a **forrás elérési útvonalának**.  Alapértelmezés szerint ez üres az.  Csak használja ezt a mezőt ha azt szeretné, hogy toonarrow hello hatókör hello származási által szolgáltatott erőforrások kívánt toomake hello CDN érhető el.  

Például a saját végpont az Excel összes erőforrás érhető el, a tárolási fiók toobe a I balra, **forrás elérési útvonalának** üres.  Ez azt jelenti, hogy a kérelem túl`https://cdndocdemo.azureedge.net/publicblob/lorem.txt` túl eredményezi egy kapcsolatot saját végpont`cdndocdemo.core.windows.net` , amely a kérelmek `/publicblob/lorem.txt`.  Hasonlóképpen, a kérelem `https://cdndocdemo.azureedge.net/donotcache/status.png` hello végpont a kért eredményezi `/donotcache/status.png` hello a forrásból.

De mi történik, ha nem szeretnék toouse hello CDN minden elérési út a forrás?  Tegyük fel például i. csak kívánta tooexpose hello `publicblob` elérési útja.  Ha a megadott */publicblob* a saját **forrás elérési útvonalának** mezőben, amely újraindítja a hello végpont tooinsert */publicblob* előtt minden kérelem benyújtásától toohello forrása.  Ez azt jelenti, hogy hello kérelem `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` most ténylegesen érvénybe hello URL-cím, hello kérelem része `/publicblob/lorem.txt`, és a hozzáfűző `/publicblob` toohello elején. Az eredmény kérelmet `/publicblob/publicblob/lorem.txt` hello a forrásból.  Ha az elérési út nem oldja meg a fájl tényleges tooan, hello származási visszaállítja az 404-es állapotot.  hello helyes URL-cím tooretrieve lorem.txt ebben a példában ténylegesen lenne `https://cdndocdemo.azureedge.net/lorem.txt`.  Vegye figyelembe, hogy nem magában foglalja az hello */publicblob* , mert hello kérelem része hello URL-cím elérési út `/lorem.txt` és hello végpont hozzáadása `/publicblob`, így a `/publicblob/lorem.txt` hello kérelem átadott toohello forrása .

