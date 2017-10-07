---
title: "aaaAzure Traffic Manager végpontmonitoring kijelző |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan Traffic Manager használja-e a végpontmonitoring kijelző és automatikus végpont feladatátvételi toohelp Azure ügyfelek magas rendelkezésre állású alkalmazások központi telepítése"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: fff25ac3-d13a-4af9-8916-7c72e3d64bc7
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2017
ms.author: kumud
ms.openlocfilehash: b4862499c88bdb1951833d06199b034a07ac7576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-endpoint-monitoring"></a>A TRAFFIC Manager-végpont figyelése

Az Azure Traffic Manager beépített végpontmonitoring kijelző és automatikus végpont feladatátvételi tartalmazza. Ez a szolgáltatás segítségével magas rendelkezésre állású alkalmazások, amelyek rugalmas tooendpoint hiba, beleértve az Azure-régiót hibák.

## <a name="configure-endpoint-monitoring"></a>Konfigurálja a végpontmonitoring kijelző

tooconfigure a végpontmonitoring kijelző, meg kell adnia a beállításokat a Traffic Manager-profil a következő hello:

* **Protokoll**. Válassza a HTTP, HTTPS vagy TCP hello protokollként, amely a Traffic Manager használja, amikor a végpont toocheck probing az állapotát. HTTPS-figyelő nem ellenőrzi, hogy az SSL-tanúsítvány érvényes--csak ellenőrzi, hogy hello-e tanúsítvány.
* **Port**. Válassza ki a hello kéréshez használt hello portot.
* **Elérési út**. A konfigurációs értéke csak érvényes hello HTTP és HTTPS protokollt, amelynek megadásával hello elérési beállításra akkor szükség. Ezt a beállítást hello TCP figyelési protokoll hibát eredményez a biztosítása. TCP protokollra vonatkozó adhat a hello relatív elérési út és hello hello weblap vagy hello fájl nevét, hogy hozzáférések figyelési hello. Perjellel (/) hello relatív elérési útnak egy érvényes bejegyzés. Ez az érték azt jelenti, hogy hello fájl hello gyökérkönyvtárában (alapértelmezett).
* **Probing időköz**. Ez az érték határozza meg, milyen gyakran a végpont a vizsgálathoz használt Traffic Manager-ügynök állapotának ellenőrzése. Itt két értékeket adhat meg: 30 másodperc (szokásos vizsgálattal) és (gyors számlálása) 10 másodperc. Ha értékek találhatók, hello készlet tooa alapértelmezett érték 30 másodperc. A Microsoft hello [Traffic Manager árazás](https://azure.microsoft.com/pricing/details/traffic-manager) lap toolearn gyors vizsgálathoz használt árazással kapcsolatos további.
* **Hibák száma a megengedett**. Ez az érték határozza meg a vizsgálathoz használt Traffic Manager-ügynök eltűr sérültként végpontra való megjelölése előtt hány sikertelen. Az érték 0 és 9 közötti tartományba. Egy érték 0, akkor egy egyetlen figyelési hiba okozhatja, hogy megfelelő állapotúként megjelölt végpont toobe. Ha nincs érték megadva, akkor az hello alapértelmezett érték 3.
* **Figyelési időkorlát**. Ez a tulajdonság meghatározza hello idő hello Traffic Manager vizsgálathoz használt ügynök várakozási figyelembe véve, hogy hiba ellenőrzése egy állapotát ellenőrző vizsgálat toohello végpont elküldésekor. Ha Probing időköz értéke too30 másodperc, majd hello hello időtúllépési értéket 5 és 10 másodperc között állíthatja be. Ha nincs érték megadva, akkor az alapértelmezett érték 10 másodperc. Ha Probing időköz értéke too10 másodperc, majd hello hello időtúllépési értéket 5 és 9 másodperc között állíthatja be. Ha nincs időtúllépés értéke meg van adva, használja a 9-es másodperc, az alapértelmezett érték.

![A TRAFFIC Manager-végpont figyelése](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

**1. ábra: A Traffic Manager-végpont figyelése**

## <a name="how-endpoint-monitoring-works"></a>Végpontmonitoring kijelző működése

Ha HTTP vagy HTTPS protokoll figyelési hello, hello Traffic Manager vizsgálathoz használt ügynök teszi egy GET kérelmet toohello végpont hello protokoll, port és megadott relatív elérési út használatával. Ha azt lekérése vissza 200-OK választ, akkor kifogástalan tekinti a végpontot. Ha a rendszer hello választ, egy másik értéket, vagy ha nem érkezik válasz hello megadott időkorláton belül, majd hello Traffic Manager-ügynök probing újrapróbálkozik szerint toohello hibák száma megengedett beállítás (nincs újrapróbálkozik történik, ha a beállítás értéke 0) . Ha egymást követő hibák hello száma nagyobb, mint hello hibák száma megengedett beállítást, majd, hogy a végpont megfelelő jelölése. 

A TCP protokoll figyelési hello esetén a hello Traffic Manager vizsgálathoz használt ügynök kezdeményez egy megadott hello port használatával TCP-kapcsolódási kérelem. Ha hello végpont válaszol toohello kérelem a válasz tooestablish hello kapcsolattal, állapot-ellenőrzése sikeres van megjelölve, és a hello Traffic Manager vizsgálathoz használt ügynök hello TCP-kapcsolat alaphelyzetbe állítása. Ha a rendszer hello választ, egy másik értéket, vagy ha nem érkezik válasz belül hello időkorlát megadott, hello Traffic Manager-ügynök probing újrapróbálkozik szerint toohello hibák száma megengedett beállítás (nincs újrapróbálkozik történik, ha a beállítás értéke 0). Ha egymást követő hibák hello száma nagyobb, mint hello hibák száma megengedett beállítást, majd, hogy a végpont van jelölve.

Minden esetben a Traffic Manager-vizsgálat több helyről és hello egymást követő sikertelen meghatározása minden régión belül történik. Ez azt is jelenti, hogy végpontok kapnak állapotteljesítmény Traffic Manager Probing időköz használt hello beállítás-nál nagyobb gyakorisággal.

>[!NOTE]
>A HTTP vagy HTTPS protokoll figyelése hello végpont oldalán általános gyakorlat tooimplement belül az alkalmazás - például /health.aspx egyéni lap. Az elérési út használatával figyelésre, alkalmazásspecifikus ellenőrzéseket, például a teljesítményszámlálók keresését, vagy az adatbázis rendelkezésre állásának ellenőrzése hajthatók végre. Az Egyéni ellenőrzések alapján, a hello lap megfelelő HTTP-állapotkódot adja vissza.

Traffic Manager-profil található összes végpontok ossza meg a figyelési beállításokat. Ha másik különböző végpontokhoz monitoringbeállítások kell toouse, létrehozhat [Traffic Manager-profilok beágyazott](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings).

## <a name="endpoint-and-profile-status"></a>Végpont és a profil állapota

Engedélyezi, és tiltsa le a Traffic Manager-profilokat és a végpontok. Azonban a végpont állapota módosítják is előfordulhat, hogy elő egy eredményt a Traffic Manager automatikus beállítások és a folyamatok.

### <a name="endpoint-status"></a>Végpont állapota

Engedélyezheti vagy letilthatja az adott végponti. hello alapul szolgáló szolgáltatást, amely lehet, hogy kifogástalan, nem. Változó hello végpont állapota vezérlők hello hello végpont a Traffic Manager-profil hello rendelkezésre állását. Ha egy végpont állapota le van tiltva, a Traffic Manager nem ellenőrzi az állapotát, és hello végpont nem szerepel a DNS-választ.

### <a name="profile-status"></a>Profil állapota

Hello profil állapota beállítással engedélyezheti vagy egy adott profil letiltása. Végpont állapota hatással van egy végpontot, amíg a profil állapota hello teljes profil, beleértve az összes végpontok hatással van. Ha letilt egy profil, hello végpontok állapotát nem ellenőrzi, és a máshol funkció végpontjai nem szerepelnek a DNS-választ. Egy [NXDOMAIN](https://tools.ietf.org/html/rfc2308) hello DNS-lekérdezés vissza.

### <a name="endpoint-monitor-status"></a>Végpont-figyelő állapota

Végpont a figyelő állapota egy Traffic Manager által generált érték, amely hello végpont hello állapotát jeleníti meg. Ez a beállítás manuálisan nem módosítható. hello végpont a figyelő állapota a végpontmonitoring kijelző hello eredményeinek kombinációját, és hello beállított végpont állapota. a lehetséges értékek hello végpont-figyelő állapotának hello a következő táblázatban láthatók:

| Profil állapota | Végpont állapota | Végpont-figyelő állapota | Megjegyzések |
| --- | --- | --- | --- |
| Letiltva |Engedélyezve |Inaktív |hello-profil le van tiltva. Bár a hello végpont állapota engedélyezve van, a hello-profil állapota (letiltva) élvez elsőbbséget. A letiltott profilok végpontok nem figyeli. Egy NXDOMAIN vissza hello DNS-lekérdezés. |
| &lt;bármely&gt; |Letiltva |Letiltva |hello végpont le van tiltva. Letiltott végpontok nem figyeli. hello végpont nem szerepel a DNS-válaszok, ezért az nem forgalom fogadására. |
| Engedélyezve |Engedélyezve |Online |hello végpont megfigyelés alatt áll, és megfelelő állapotban. DNS-válaszok szerepel, és képes forgalom fogadására. |
| Engedélyezve |Engedélyezve |Csökkentett teljesítményű |A végpont állapotfigyelése ellenőrzése sikertelen. hello végpont nem része a DNS-válaszok, és nem kap a forgalmat. <br>Egy kivétel toothis végpontjai állapotromlást, ha ebben az esetben az összes minősülnek hello lekérdezés válaszként visszaadott toobe).</br>|
| Engedélyezve |Engedélyezve |CheckingEndpoint |hello végpont megfigyelés alatt áll, de hello első vizsgálat eredményeinek hello még nem érkeztek. CheckingEndpoint: ideiglenes állapot, amely közvetlenül a hozzáadásával vagy egy olyan végpont hello-profil engedélyezése után általában akkor következik be. Ebben az állapotban a végpont DNS-válaszok tartalmazza, és képes forgalom fogadására. |
| Engedélyezve |Engedélyezve |Leállítva |hello felhőalapú szolgáltatás vagy a webes alkalmazást, amely nem fut a végpont pontok toois hello. Ellenőrizze a hello felhőalapú szolgáltatás, vagy a webes alkalmazás beállításai. Ez is fordulhat, ha hello végpont írja be a beágyazott végpont és hello gyermek profil le van tiltva, vagy nem aktív. <br>A rendszer nem figyeli egy végpontot Leállítva állapotú. DNS-válaszok nem szerepel, és nem kap a forgalmat. Egy kivétel toothis, ha végpontjai állapotromlást, ebben az esetben az összes akkor veszi figyelembe az eredmény abban hello lekérdezés toobe.</br>|

Hogyan végpont figyelő állapotának kiszámítása beágyazott végpontok kapcsolatos részletekért lásd: [Traffic Manager-profilok beágyazott](traffic-manager-nested-profiles.md).

### <a name="profile-monitor-status"></a>Profil a figyelő állapota

hello profil a figyelő állapota hello konfigurált profil állapota és a hello végpont a figyelő állapota értékek összes végpontok. hello a következő táblázat ismerteti a hello a lehetséges értékek:

| Profil állapota (a be van állítva) | Végpont-figyelő állapota | Profil a figyelő állapota | Megjegyzések |
| --- | --- | --- | --- |
| Letiltva |&lt;bármely&gt; vagy egy nem meghatározott végpont-profilt. |Letiltva |hello-profil le van tiltva. |
| Engedélyezve |legalább egy végpont hello állapota jelenleg csökkentett. |Csökkentett teljesítményű |Tekintse át a hello egyes végpont állapota értékek toodetermine további figyelmet igénylő milyen végpontokat. |
| Engedélyezve |hello legalább egy végpont állapota Online. Nincsenek végpontjai csökkentett teljesítményű állapotú. |Online |hello szolgáltatás fogadja a forgalmat. Nincs szükség semmilyen további műveletre. |
| Engedélyezve |hello legalább egy végpont állapota CheckingEndpoint. Nincsenek végpontok Online vagy a csökkentett teljesítményű állapotban vannak. |CheckingEndpoints |A közbenső műveletfázisa következik be, amikor egy profil, ha a létrehozott vagy engedélyezett. hello végpont állapota ellenőrzés hello az első alkalommal. |
| Engedélyezve |hello állapotok hello-profil minden végpontok vagy le van tiltva, vagy leállt, vagy hello-profil rendelkezik-e meghatározott végpontok. |Inaktív |Máshol funkció végpontjai nem aktív, de hello-profil még mindig engedélyezve van. |

## <a name="endpoint-failover-and-recovery"></a>Végpont feladatátvétel és helyreállítás

A TRAFFIC Manager rendszeres időközönként minden végpontot, beleértve a nem megfelelő állapotú végpontok hello állapotát ellenőrzi. A TRAFFIC Manager észleli, ha a végpont lesz kifogástalan, és vissza során azt az elforgatás kerülnek.

A végpont állapota nem megfelelő, amikor hello események bármelyikét tapasztalja:
- Ha HTTP vagy HTTPS protokoll figyelési hello:
    - A nem 200 választ (beleértve a különböző 2xx kódot, vagy 301/302 irányítja át a felhasználókat).
- Ha protokoll figyelési hello TCP: 
    - Válasz eltérő Nyugtázási vagy SZIN-Nyugtázási érkezett válasz toohello SZINKRONIZÁLÁSI kérelmet küldött Traffic Manager tooattempt a kapcsolat felépítése.
- Időtúllépés. 
- Bármely más kapcsolati probléma eredményezve hello végpont alatt nem érhető el.

Hibaelhárítási sikertelen ellenőrzést kapcsolatos további információkért lásd: [hibaelhárítás "csökkentett teljesítményű" állapota az Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md). 

hello 2. ábra a következő ütemterv egy figyelő, amely rendelkezik a következő beállítások hello Traffic Manager-végpont folyamatát hello részletes leírása: HTTP protokoll figyelési, a ellenőrzési időköz érték 30 másodperc, a megengedett hibák száma: 3, időtúllépési értéket 10 másodperc, és a DNS-élettartam érték 30 másodperc.

![A TRAFFIC Manager végpont feladatátvétel és a feladat-visszavétel feladatütemezési](./media/traffic-manager-monitoring/timeline.png)

**2. ábra: Traffic manager végpont feladatátvételének és helyreállításának feladatütemezési**

1. **ELSŐ**. Minden egyes végpont hello Traffic Manager figyelési rendszer végez GET kérés hello elérési út van megadva a figyelési beállításokat hello.
2. **200 OK**. megfigyelési rendszere hello egy 10 másodpercen belül visszaadott HTTP 200 OK üzenet toobe vár. Ha ezt a választ kap, felismeri, hogy elérhető legyen-e hello szolgáltatást.
3. **ellenőrzések között, 30 másodperces**. hello végpont az állapot-ellenőrzéssel 30 másodpercenként ismétlődik.
4. **A szolgáltatás nem érhető el**. hello szolgáltatás nem érhető el. A TRAFFIC Manager hello tovább az állapot-ellenőrzéssel addig nem szerez tudomást.
5. **Kísérletek tooaccess hello figyelési elérési út**. megfigyelési rendszere hello hajt végre egy GET kérelmet, de nem kapott választ a 10 másodperces hello időkorláton belül (másik lehetőségként nem 200 válasz lehet, hogy fogadható). Akkor megpróbálja háromszor, 30 másodperces időközönként. Ha egyik hello próbálkozás sikeres, majd záma hello száma alaphelyzetbe áll.
6. **Csökkentett tooDegraded**. A negyedik egymást követő sikertelen követően megfigyelési rendszere hello hello nem érhető el-végpont állapota csökkentett teljesítményű állapotúként jelöli meg.
7. **Akkor elterelt tooother végpontok**. hello Traffic Manager DNS-névkiszolgálók frissülnek, és a Traffic Manager már nem beolvasása hello végpont válasz tooDNS lekérdezésekben. Új kapcsolatok irányított tooother, elérhető végpontok. Azonban előző DNS-válaszok, amelyek tartalmazzák ezt a végpontot előfordulhat, hogy továbbra is gyorsítótárazható rekurzív DNS-kiszolgálók és DNS-ügyfeleket. Ügyfelek toouse hello végpont folytatható, amíg a DNS-gyorsítótár hello lejár. DNS-gyorsítótár hello lejár, az ügyfelek új DNS-lekérdezések, és olyan irányított toodifferent végpontok. gyorsítótárazás időtartama hello hello TTL beállítása a Traffic Manager-profil, például 30 másodperces hello vezérli.
8. **Állapotfigyelő ellenőrzi, hogy továbbra is**. A TRAFFIC Manager csökkentett teljesítményű állapotba került, miközben továbbra is a hello végpont toocheck hello állapotát. A TRAFFIC Manager észleli, ha hello végpont toohealth adja vissza.
9. **Szolgáltatás ismét online elérhető**. hello szolgáltatás elérhetővé válik. hello végpont a csökkentett teljesítményű állapota a Traffic Manager megőrzi, amíg megfigyelési rendszere hello hajtja végre a következő állapot-ellenőrzése.
10. **Folytatja a forgalom tooservice**. A TRAFFIC Manager GET kérést küld, és 200 OK állapotot választ kap. hello szolgáltatás tooa Kifogástalan állapotot adott vissza. hello Traffic Manager névkiszolgálók frissülnek, és a kimenő hello szolgáltatás DNS-név, a DNS-válaszok toohand megkezdése. Forgalom toohello végpont adja vissza, gyorsítótárazott DNS-válaszok végpontja visszaadó lejár, és a meglévő kapcsolatok tooother végpontok leáll.

    > [!NOTE]
    > Traffic Manager DNS szint hello működik, mert az nem befolyásolja a meglévő kapcsolatok tooany végpont. Arra utasítja a (vagy módosított profil beállításait, feladatátvétel vagy a feladat-visszavétel során) végpontok közötti forgalmat, ha a Traffic Manager új kapcsolatok tooavailable végpontok irányítja. Azonban más végpontok előfordulhat, hogy folytatható tooreceive forgalom meglévő kapcsolatokon keresztül, amíg a munkamenetek leállítása van. tooenable forgalom toodrain a meglévő kapcsolatokat, alkalmazásokat kell korlátoznia hello végpontok használt munkamenet időtartama.

## <a name="traffic-routing-methods"></a>Forgalom-útválasztási módszer

A végpont állapota csökkentett teljesítményű, ha már nem visszaadott válasz tooDNS lekérdezésekben. Ehelyett egy másik végpont választott és adott vissza. hello profilban konfigurált hello forgalom-útválasztási módszer határozza meg, hogyan hello alternatív végpont van kiválasztva.

* **Prioritás**. Végpontok alkotják a prioritási sorrend listájáról. hello első érhető el-végpont hello lista mindig adja vissza. Ha egy végpont állapota jelenleg csökkentett, majd hello következő elérhető végpontot ad vissza.
* **Súlyozott**. Minden elérhető végpontot véletlenszerűen hozzárendelt tanúsítványsúly alapján kell kiválasztani, és hello hello súlyozását más elérhető végpontok.
* **Teljesítmény**. hello végpont legközelebbi toohello végfelhasználó adja vissza. Hogy a végpont nem érhető el, ha a végpont véletlenszerűen kiválasztott az összes többi elérhető végpontok hello. Egy véletlenszerű végpont kiválasztása Ezzel elkerülheti a kaszkádolt hiba, amely akkor fordulhat elő, amikor hello legközelebbi végpont válik túlterhelt. Konfigurálhatja az alternatív feladatátvételi tervek teljesítmény forgalom-útválasztás használatával [Traffic Manager-profilok beágyazott](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region).
* **Földrajzi**. hello végpont leképezve tooserve hello földrajzi helye alapján hello lekérdezési kérelmek az IP-címeknek adja vissza. Hogy a végpont nem érhető el, ha egy másik végpont csak akkor kijelölt toofailover, mivel a földrajzi helyet lehet csatlakoztatott csak tooone végpont a profil (További részletek szerepelnek hello [gyakran ismételt kérdések](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)). Ajánlott eljárásként, földrajzi útválasztási használatakor, a felhasználóknak ajánljuk beágyazott toouse Traffic Manager-profilok csak egy végponttal hello végpontként hello-profil.

További információkért lásd: [Traffic Manager forgalom-útválasztási módszert](traffic-manager-routing-methods.md).

> [!NOTE]
> Egy kivétel toonormal forgalom-útválasztási fordul elő, ha az összes jogosult végpontok leromlott állapot. A TRAFFIC Manager elérhetővé válnak a lehető legjobb rendezését,"kísérlet és *tűnik, mintha az összes hello csökkentett teljesítményű állapot végpontok ténylegesen online állapotban vannak*. Ez a viselkedés akkor érdemes toohello alternatív, amelyeket meg kell toonot hello DNS-válasz visszaadása a tetszőleges végpontot. Letiltott vagy leállított végpontok nem figyeli, ezért nem tekinthetők jogosult a forgalmat.
>
> Ez az állapot általában okozza helytelen konfigurációja hello szolgáltatást, például:
>
> * Hozzáférés-vezérlési listaként [ACL] blokkolja a hello Traffic Manager állapotát ellenőrzi.
> * Egy figyelő port vagy a protokoll a Traffic manager-profil hello hello helytelen konfigurációja.
>
> hello ez lesz a következménye, hogy ha a Traffic Manager állapotellenőrzést nem megfelelően vannak konfigurálva, az jelenhet meg a hello adatforgalomra kiterjed, mint a Traffic Manager útválasztási *van* megfelelően működik. Azonban ebben az esetben végpont feladatátvételi nem fordulhat elő, ami hatással van az alkalmazás általános rendelkezésre állási. Fontos, hogy hello-profil látható-e egy Online állapotát, nem a csökkentett teljesítményű állapot toocheck. Az Online állapot azt jelzi, hogy hello Traffic Manager állapotát ellenőrzi a vártnak megfelelően működnek.

Nem sikerült a következő hibaelhárítással kapcsolatos további információkat a állapotellenőrzést, lásd: [hibaelhárítás "csökkentett teljesítményű" állapota az Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).



## <a name="next-steps"></a>Következő lépések

Ismerje meg, [Traffic Manager működése](traffic-manager-how-traffic-manager-works.md)

További tudnivalók hello [forgalom-útválasztási módszert](traffic-manager-routing-methods.md) Traffic Manager által támogatott

Ismerje meg, hogyan túl[Traffic Manager-profil létrehozása](traffic-manager-manage-profiles.md)

[Csökkentett teljesítményű állapot hibaelhárítása](traffic-manager-troubleshooting-degraded.md) a Traffic Manager-végpontot
