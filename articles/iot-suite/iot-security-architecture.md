---
title: "aaaIoT biztonsági architektúrája |} Microsoft Docs"
description: "IoT-biztonsági architektúrája irányelvek és szempontok"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 18ed3eb0-9406-44e1-8a3a-93dc6726c7ac
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: yurid
ms.openlocfilehash: 5b59133f6b1b45573318c3bd5b621d27b147d71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-architecture"></a>Az eszközök internetes hálózatát biztonsági architektúrája
A rendszer tervezésekor fontos toounderstand hello potenciális fenyegetések toothat rendszer, és adjon hozzá megfelelő védelmekkel ennek megfelelően módon hello rendszer terveztek és tervezett. Különösen fontos toodesign hello hello start terméket a biztonságot szem előtt tartva, mert megértése, hogyan lehet egy támadó, képes toocompromise a rendszer segít, gondoskodjon arról, hogy megfelelő megoldást hello elejétől helyen. 

## <a name="security-starts-with-a-threat-model"></a>Biztonsági kezdődik-e a fenyegetések modellezése
A Microsoft fenyegetés modellek hosszú által használt termékek és által végrehajtott folyamat nyilvánosan elérhető modellezési hello vállalati fenyegetést. hello vállalati élmény mutatja be, hogy rendelkezik-e hello modellezési túl azonnali ismertetése, hogy milyen fenyegetéseket, hello legtöbb vonatkozó hello váratlan előnyeit. Például is létrehoz egy erőfeszítések egy megnyitott vitafórum másokkal kívül hello fejlesztőcsoportunk, ami toonew ötleteket és fejlesztéseket hello termékben.

hello modellezni célja toounderstand arról, hogy a támadó hogyan lehet, hogy képes toocompromise rendszert kell és ellenőrizze, megfelelő megoldást legyenek érvényben. Modellezni kényszeríti hello tervezési csapat tooconsider azok mérséklési hello rendszer úgy van kialakítva, hanem a rendszer telepítését követően. Ennek oka különösen fontos, biztonsági védelmekkel tooa számtalan hello mezőben eszközök alkalmazásával lehessen hozni, nagyon eséllyel fordulnak elő hiba, és bezárja az ügyfelek veszélyben.

Sok a fejlesztési csapat munkaköre egy kiváló hello funkcionális követelményeinek hello rendszer ügyfelek előnyeit kihasználó rögzítése. Nem egyértelmű módon, hogy valaki előfordulhat, hogy visszaél hello rendszer azonosításához azonban további kihívást. Modellezni segít megérteni, hogy egy támadó előfordulhat, hogy mit a fejlesztési csapat és okát. Modellezni rendkívül létrehozó döntéseken hello biztonságával kapcsolatos tervezési döntések hello rendszer, valamint mentén hatással lehetnek a biztonsági hello módon végrehajtott módosítások toohello tervezési strukturált folyamat. Míg a fenyegetések modellezése egyszerűen egy dokumentumot, az ebben a dokumentációban is egy ideális Tudásbázis, a megszerzett megőrzést tooensure folyamatosságát megtanulta, és segítségével gyorsan bevezetésében az új csoport jelöli. Végezetül modellezni egy eredményét tooenable meg tooconsider más vonatkozásai, például hogy milyen biztonsági kötelezettségeit kívánja tooprovide tooyour ügyfelek. E kötelezettségvállalások modellezni együtt értesíti, és a meghajtó vizsgálata a az eszközök internetes hálózatát (IoT) megoldás.

### <a name="when-toothreat-model"></a>Amikor toothreat modell
[Modellezni](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) ajánlatok hello legnagyobb érték, ha hello tervezési szakaszban részét képezi. Ha tervez, akkor hello legnagyobb rugalmasságot toomake módosítások tooeliminate fenyegetéseket. Hello kívánt eredménytől fenyegetések kiküszöbölése azért. Sokkal egyszerűbb, mint azok mérséklési hozzáadása, kipróbálása és biztosítása naprakészek maradjanak és továbbá ilyen eltávolítási nem mindig lehetséges legyen. Fenyegetések, a termék további lesz számos, és a viszont végső soron esetén további munkahelyi és sokkal nehezebb mellékhatásokkal, mint a hello fejlesztési korai a modellezési fenyegetés nehezebben tooeliminate válik.

### <a name="what-toothreat-model"></a>Milyen toothreat modell
Modell hello megoldás egész és is a következő területeken hello fókusz kell szál:

* hello biztonsági és adatvédelmi szolgáltatások
* hello funkciókat, amelyek hibák a megfelelő biztonsági
* a megbízhatósági kapcsolat határán touch hello szolgáltatások 

### <a name="who-threat-models"></a>Ki a fenyegetés modellek
Modellezni az a folyamat más.  Jó ötlet tootreat hello fenyegetés modell dokumentumok például hello megoldás bármely másik összetevője, és érvényesíti. Sok a fejlesztési csapat munkaköre egy kiváló hello funkcionális követelményeinek hello rendszer ügyfelek előnyeit kihasználó rögzítése. Nem egyértelmű módon, hogy valaki előfordulhat, hogy visszaél hello rendszer azonosításához azonban további kihívást. Modellezni segít megérteni, hogy egy támadó előfordulhat, hogy mit a fejlesztési csapat és okát.

### <a name="how-toothreat-model"></a>Hogyan toothreat modell
hello fenyegetések modellezése folyamat áll négy lépést; hello lépései a következők:

* Minta hello alkalmazás
* Fenyegetések számbavétele
* Veszélyek mérséklése.
* Hello azok mérséklési ellenőrzése

#### <a name="hello-process-steps"></a>hello folyamat lépései
Három szabályok megoldás tookeep szem előtt a fenyegetések modellezése felépítése közben:

1. Hozzon létre egy referencia-architektúrában kívül diagramja. 
2. Indítsa el a szélesség-első. Áttekintheti, és megismerje hello rendszer egészére, mielőtt a mély-ról.  Ez gondoskodik róla, hogy Ön részletesen a jobb oldali hello helyezi.
3. Hello folyamat meghajtó, ne engedje meg meghajtó hello folyamat. Ha problémát található hello modellezési fázisban, és szeretné, hogy tooexplore, válassza ki azt!  Nem érzi, hogy újra kell toofollow ezeket a lépéseket slavishly.  

#### <a name="threats"></a>Fenyegetések
a fenyegetések modellezése hello négy fő elemei a következők:

* Folyamatok (web services, a Win32-szolgáltatások, * nix démonok stb. Vegye figyelembe, hogy bizonyos összetett entitások (például a mező átjárók és az érzékelők) azért, mivel a folyamat, amikor ezek a területek a műszaki Lehatolás nincs lehetőség.
* Adatokat tárolja (bárhol tárolja, például a konfigurációs fájlban vagy adatbázis)
* (Ha az adatok áthelyezése más hello alkalmazás elemek között) adatfolyama
* Külső entitások (semmit, amely együttműködik a hello rendszert, de nem hello alkalmazás hello ellenőrzése alatt, példák használó felhasználók belefoglalásához és hírcsatornák műholdas)

A hello architekturális diagramja elemek egyike sem tárgy toovarious fenyegetések; hello STRIDE válasszanak használjuk. Olvasási [fenyegetés újra modellezési, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) hello STRIDE elemekre vonatkozó további tooknow.

Hello alkalmazásdiagram különböző összetevői tulajdonos toocertain STRIDE fenyegetések:

* Folyamatok tulajdonos tooSTRIDE
* Data flow tulajdonos tooTID
* Adattároló tulajdonos tooTID, és egyes esetekben R, is, ha hello adattárolókhoz naplófájlokat.
* Külső rendszer tulajdonos tooSRD

## <a name="security-in-iot"></a>Az IoT biztonsági
Speciális csatlakoztatott eszközök lehetséges interakció területére és interakció kombinációját, amelyek figyelembe veendő tooprovide toothose eszközök digitális hozzáférés biztosítása érdekében a keretrendszer jelentős számú rendelkezik. hello "digitális hozzáférés" kifejezés használt ide toodistinguish közvetlen eszköz segítségével végzett műveleteket a hozzáférés-biztonságot amennyiben a fizikai hozzáférés-vezérlés használatával. A hello ajtó például üzembe hello eszköz ellátott zárolást. Fizikai hozzáférés nem lehet nem engedélyezett a szoftver- és hardverkonfigurációiról, amíg intézkedések megtételét tooprevent fizikai hozzáférés toosystem zavaró tényező vezető. 

Hello interakció minták tájékozódáshoz azt követően áttekintjük "eszköz control" és "eszköz adatok" hello figyelmet a azonos szinten. "Eszköz control" meghatározhassa semmilyen információt tooa eszköz által biztosított félre a módosítása, illetve annak állapotát vagy a környezet állapotát hello felé viselkedését befolyásoló hello célját. "Eszköz adatok" semmilyen információt, hogy egy eszköz bocsát ki tooany másik fél az állapotot és a környezet állapotának megfigyelt hello meghatározhassa.

Rendelés toooptimize ajánlott biztonsági eljárások javasolt, hogy egy tipikus IoT-architektúra kell osztani több összetevő/zóna hello fenyegetések modellezése a gyakorlatban részeként. A zónák teljesen ebben a szakaszban leírt, többek között:

* Eszköz,
* A mező átjáró
* A felhő átjárók, és
* Szolgáltatások.

Zónák széleskörű módon toosegment megoldás; az egyes zónák gyakran van a saját adatok és a hitelesítési és engedélyezési követelményeinek. Zónák is használt tooisolation kárt, és korlátozhatja a magasabb megbízhatósági zónák alacsony megbízhatósági zónák hello hatását.

Minden egyes zónában van eltérő megbízhatósági tartományba, mint hello pontozott piros sor az alábbi ábrán hello észlelt. Az adatok/információk átmenet egy forrás tooanother jelöli. Ez a változás során hello adatok/információ lehet tulajdonos tooSpoofing, Tampering, Letagadhatóság, Információfelfedés, szolgáltatásmegtagadásos és jogosultság jogosultságszint-emelés (STRIDE).

![Az IoT biztonsági zónák](media/iot-security-architecture/iot-security-architecture-fig1.png) 

hello belül minden határ kitaláltak összetevői is vetni tooSTRIDE engedélyezése a teljes 360 fenyegetések modellezése hello megoldás nézet. az alábbi hello szakaszok ismertetik az egyes hello összetevők és a meghatározott biztonsági problémáit és a megoldások, amelyek kell bevezetni.

Ezekben a zónákban található standard összetevők hello szakaszok ismertetik.

### <a name="hello-device-zone"></a>hello eszköz zóna
hello eszközkörnyezet hello azonnali fizikai hely körül hello eszköz fizikai hozzáférés, illetve a "helyi hálózat" társ-társ digitális elérhetik a toohello eszköz esetén valósítható meg. A "helyi hálózat" toobe egy önálló, és a – szigetelt, de a potenciálisan Áthidalt túl – hello nyilvános interneten, és a bármely rövid hatótávolságú vezeték nélküli rádió technológia, amely lehetővé teszi az eszközök társ-társ kommunikációs tartalmazó hálózati feltételezi. Létezik *nem* közé tartozik a hálózati virtualizálási technológia hello érhet helyi hálózat létrehozása, és nem is tartalmazza a nyilvános operátor hálózatok igénylő bármely két eszköz toocommunicate nyilvános hálózati hely között Ha egy társ-társ tooenter kommunikációs kapcsolat voltak.

### <a name="hello-field-gateway-zone"></a>hello mező átjáró zóna
A mező az átjáró az eszköz/berendezés vagy néhány általános célú kiszolgáló számítógépes szoftverek kommunikációs engedélyező, és esetleg eszköz verziókezelő rendszer-és eszköz adatfeldolgozási hub szolgáló. összes eszköz csatolt tooit és hello mező átjáró zóna tartalmaz hello mező átjáró magát. Hello név azt jelenti, mert mező átjárók külső dedikált adatfeldolgozási berendezések működésre, és általában hely kötve, potenciálisan tulajdonos toophysical behatolás, és csak korlátozott működési redundancia. Minden toosay, amely egy mező átjáró általában egy dolog egyikét is touch és szabotálására tudatában funkciójához van. 

Egy mező átjáró eltér puszta forgalom útválasztó, hogy ez volt-e a hozzáférés-kezelés aktív szerepet és információáramlás, ami azt jelenti, az alkalmazás címzett entitás és a hálózati kapcsolat vagy terminál-munkamenet. Egy NAT-eszköz vagy tűzfal, ezzel szemben nem felel meg a mező átjáróként mivel azok nem explicit kapcsolat vagy a munkamenet terminálok, de a ahelyett, hogy egy útvonalat (vagy blokk) kapcsolatok vagy a munkamenetek keresztül őket. hello mező átjáró két különböző területére rendelkezik. Egy csatlakoztatott tooit hello eszközök néz és hello belül hello zóna jelöli és a más hello előtt álló minden külső felek és hello szegély hello zóna.   

### <a name="hello-cloud-gateway-zone"></a>hello felhő átjáró zóna
Átjáró a rendszer, amely lehetővé teszi a távoli kommunikációhoz és több különböző helyére toodevices vagy a mezőparaméter-átjáróinak között nyilvános hálózati helyet, általában egy felhőalapú vezérlési és az elemző rendszer, az ilyen rendszerek összevonási felé. Bizonyos esetekben egy felhőátjáróhoz azonnal megkönnyíthetik hozzáférés toospecial célú eszközök például a táblagépek és telefonok részére. A tárgyalt hello környezetben itt "cloud" célja toorefer dedikált tooa adatfeldolgozási rendszer, amely nincs kötve toohello azonos hely hello eszközök vagy mező átjárók hozzá van kapcsolva. Is felhő zónában, operatív intézkedések célzott fizikai hozzáférés megakadályozása és nem feltétlenül kitett tooa "nyilvános" felhőinfrastruktúráját.  

A hálózati virtualizálási átfedő tooinsulate hello felhő átjáró és az összes csatlakoztatott eszközök vagy bármely más hálózati forgalmaktól mező átjárók potenciálisan képezhető le egy felhőátjáróhoz. hello átjáró maga sem egy eszköz, sem a feldolgozási vagy a tárolási létesítmény eszköz adatok; Ezek az eszközök csatlakozhatnak a hello felhőátjáróhoz. eszközök közvetve vagy közvetlenül csatlakoztatott tooit és hello felhő átjáró zóna tartalmaz hello felhőátjáróhoz maga minden mező átjáró együtt. hello zóna hello széle pedig egy distinct felületet biztosít, ahol az összes külső felek keresztül kommunikálnak.

### <a name="hello-services-zone"></a>hello szolgáltatások zóna
A "szolgáltatás" definiálva van ebben a környezetben, a szoftverfrissítési összetevő vagy modul, amely az eszközök mező vagy felhő átjárón keresztül az adatok gyűjtése és elemzése, valamint a parancs és a vezérlés vele.  Szolgáltatások a közvetítő folyamatok. Azok az identitásukat, átjárók és az egyéb alrendszerek alatt működjön, tárolását és elemzését az adatokat, önállóan probléma parancsok toodevices adatok insights vagy az ütemezések alapján, és információkat és a vezérlő képességek tooauthorized végfelhasználók számára elérhetővé tenni.

### <a name="information-devices-vs-special-purpose-devices"></a>Eszközök információk, és speciális eszközök összehasonlítása
Számítógépek, telefonok és táblagépek olyan eszközök elsősorban interaktív információk. Telefonok és táblagépek körül akkumulátor élettartama maximalizálva explicit módon vannak optimalizálva. Lehetőleg kikapcsolása részben Ha egy személy való interakció nem azonnal, vagy ha nem nyújt szolgáltatásokat, mint a zene lejátszását, vagy a tulajdonos irányítsa tooa adott helyről. Rendszerek szempontjából ezek az információk technológia eszközök főként, személyek felé proxyként működő. "Személyek rákötni" arra vonatkozóan, műveletek és a "személyek érzékelők" bemeneti gyűjtése. 

Speciális eszközöket, egyszerű hőmérséklet érzékelők toocomplex gyári éles sorok összetevőket tartalmaz, több ezer eltérőek. Ezek az eszközök sokkal több hatóköre rendeltetésüket, és akkor is, ha a felhasználói felület egyes biztosítanak, azokat a nagy mértékben hatókörön belüli toointerfacing vagy eszközök hello fizikai világ integrálni. Azok mérésére és környezeti körülmények jelentést, szelepek kapcsolja, vezérlését servos, riasztások hangkártya, fény kapcsoló és számos egyéb feladatok elvégzéséhez. Munkahelyi toodo segítenek, amelyek olyan információkat eszköz esetében túl általános, olcsóbbá túl, túl nagy vagy túl rideg. hello konkrét, meghatározott célú azonnal határozzák meg, hogy a műszaki tervezés mint jól hello pénzügyi rendelkezésre álló költségvetésnek éles és ütemezett élettartama műveletet. Ez a két kulcsfontosságú tényező hello kombinációja hello elérhető működési energia költségvetési, fizikai kezdjen, és így rendelkezésre álló tár, számítási és biztonsági képességeket korlátozza.  

Ha "is megnőnek hibás" az automatikus és a távoli vezérelhető eszközök, például valamilyen fizikai hibák vagy ellenőrzési logika hibák toowillful nem engedélyezett behatolás és kezelését végzi. hello éles sok meg kell semmisíteni, épületek looted vagy írása, és személyek lehet, hogy sérült, vagy még akkor is, die. Ez az, természetesen teljes különböző osztály kárt valaki maxing kimenő korlátja egy lopott hitelkártya-nál. hello művelet eszközök biztonsági sávját helyezze, és is az érzékelők adataiból, hogy végül dolgot toomove okozó parancsok eredményez nagyobbnak kell lennie elektronikus kereskedelmi vagy banki adathierarchia. 

### <a name="device-control-and-device-data-interactions"></a>Eszköz vezérlő és eszköz adatok interakciók
Speciális csatlakoztatott eszközök lehetséges interakció területére és interakció kombinációját, amelyek figyelembe veendő tooprovide toothose eszközök digitális hozzáférés biztosítása érdekében a keretrendszer jelentős számú rendelkezik. hello "digitális hozzáférés" kifejezés használt ide toodistinguish közvetlen eszköz segítségével végzett műveleteket a hozzáférés-biztonságot amennyiben a fizikai hozzáférés-vezérlés használatával. A hello ajtó például üzembe hello eszköz ellátott zárolást. Fizikai hozzáférés nem lehet nem engedélyezett a szoftver- és hardverkonfigurációiról, amíg intézkedések megtételét tooprevent fizikai hozzáférés toosystem zavaró tényező vezető. 

Hello interakció minták tájékozódáshoz azt követően áttekintjük "eszköz control" és "eszköz adatok" hello azonos szintű figyelmet modellezni közben. "Eszköz control" meghatározhassa semmilyen információt tooa eszköz által biztosított félre a módosítása, illetve annak állapotát vagy a környezet állapotát hello felé viselkedését befolyásoló hello célját. "Eszköz adatok" semmilyen információt, hogy egy eszköz bocsát ki tooany másik fél az állapotot és a környezet állapotának megfigyelt hello meghatározhassa. 

## <a name="threat-modeling-hello-azure-iot-reference-architecture"></a>Fenyegetések modellezése hello Azure IoT-referenciaarchitektúra
A Microsoft hello keretrendszer toodo fenyegetés modellezés Azure IoT a fent vázolt használja. Az alábbi részben hello ezért használjuk hello Azure IoT-Referenciaarchitektúra toodemonstrate hogyan kapcsolatos IoT, és hogyan tooaddress hello fenyegetések modellezése fenyegetés toothink azonosított konkrét példája. A mi esetünkben felismertük fókusz négy fő területet:

* Eszközök és az adatforrás,
* Adatok átvitel
* Eszköz és az események feldolgozásával, és
* Bemutató

![Az Azure IoT modellezési fenyegetés](media/iot-security-architecture/iot-security-architecture-fig2.png) 

az alábbi ábrán hello jeleníti meg egyszerűsített IoT-architektúra a Microsoft hello Microsoft fenyegetések modellezése eszköz által használt adatfolyam-Diagram modell segítségével:

![Az Azure IoT eszközzel MS fenyegetések modellezése modellezési fenyegetés](media/iot-security-architecture/iot-security-architecture-fig3.png)

Fontos, hogy architektúra hello toonote elválasztja hello eszköz- és átjáró lehetőségeket. Ez lehetővé teszi a hello felhasználói tooleverage átjáró, amely biztonságosabb: képesek kommunikálni a hello felhőátjáróhoz biztonságos protokollal, amelyhez általában kisebb feldolgozási terhelés, hogy a natív eszköz -, például egy termosztát - sikerült Adja meg saját maga. Hello Azure-szolgáltatások zóna feltételezzük, hogy Felhőátjáróhoz hello Azure IoT-központ szolgáltatás által képviselt hello.

### <a name="device-and-data-sourcesdata-transport"></a>Eszközök és adatok források/adatok átvitel
Ez a szakasz keresztül modellezni hello helyre gyűjti a fent vázolt hello architektúra felderíti, és hogyan jelenleg éppen aktuális néhány hello rejlő aggályokat áttekintést. A fenyegetések modellezése hello core elemeinek tárgyaljuk:

* (Azok alatt a vezérlő és a külső elemek) folyamatok
* Kommunikációs (más néven adatfolyamok)
* Tárhely (más néven adattároló)

#### <a name="processes"></a>Folyamatok
Az egyes hello Azure IoT-architektúra leírt hello kategóriák azt próbálja toomitigate számos különböző fenyegetések hello különböző szakaszaiban adatok/információk között szerepel: folyamat, a kommunikáció és a tároló. Az alábbiakban azt áttekintést hello leggyakrabban hello "folyamat" kategória, hogyan ezek sikerült legjobb szüntethető áttekintése után: 

**(S)-címek hamisítását**: támadó előfordulhat, hogy bontsa ki a titkosítási kulcsokat tárol egy eszközről, vagy szinten hello szoftveres vagy hardveres, és ezt követően hello rendszer elérni egy másik fizikai vagy virtuális eszköz a hello eszköz hello hello identitás kulcsokat tárol származik. Egy jó ábra, amely bekapcsolja a TV és népszerű prankster eszközök távoli szabályozza.

**Letiltja a szolgáltatást (D)**: egy eszköz nem működik, vagy történő kommunikációhoz választógomb-használati vagy kivágáshoz fenyegetéseknek zavarja alkalmas megjeleníthetők. Például a felügyeleti fényképezőgép, amelyekről a szándékosan kiejtett power vagy a hálózati kapcsolat nem küld adatokat, minden.

**(T) illetéktelen**: támadó részben vagy egészben helyettesítheti hello szoftver hello eszközön futó, potenciálisan cseréje hello szoftver tooleverage hello valódi identitás hello eszköz Ha hello billentyűt anyagokat vagy hello kriptográfiai kulcs anyagok okozó létesítményekben volt elérhető toohello tiltott program. Például egy támadó előfordulhat, hogy kihasználja a kibontott kulcs jelentős toointercept és hello eszközön hello kommunikációs elérési úton található adatok mellőzése és cserélje le ellopják hello kulcsokat tárol a hitelesített hamis adatok.

**Információk felfedése (I)**: hello eszköz úgy szoftvert futtat, ha úgy szoftverhez sikerült potenciálisan szivárgás lépett fel adatokat toounauthorized felek. Egy támadó például kihasználja a kibontott kulcs jelentős tooinject magát a hello kommunikációs útvonalat hello eszköz és a tartományvezérlő vagy mező átjáró közötti, a felhő átjáró toosiphon ki adatokat.

**A jogosultság E illetéktelen**: megadott funkciót hajtja lehet kényszerített toodo valami mással. Például, amely programozott tooopen fele eltérő módon lehet szelep megtevésztve összes hello tooopen módon.

| **Összetevő** | **Fenyegetések** | **Megoldás** | **Kockázati** | **Megvalósítása** |
| --- | --- | --- | --- | --- |
| Eszköz |S |Identitás toohello eszköz rendelni, illetve hello eszköz hitelesítése |Eszköz vagy hello eszköz része cseréje egy másik eszköz. Hogyan azt, hogy azt beszélünk toohello megfelelő eszközre? |Hello eszköz használata a Transport Layer Security (TLS) vagy az IPSec-hitelesítéséhez. Infrastruktúra támogatnia kell a előmegosztott kulccsal (PSK), amely nem tudja kezelni a teljes aszimmetrikus titkosítási eszközökön. Használja ki az Azure AD [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt) |
| TRID |Alkalmazza a tamperproof mechanizmusok toohello eszköz például azáltal, hogy nagyon nehéz tooimpossible tooextract kulcsokat és más kriptográfiai anyag hello eszközről. |hello kockázat, ha valaki van-e illetéktelen hello eszköz (fizikai zavaró tényező). Hogyan történik a Microsoft, az eszköz nem illetéktelenül módosítva. |hello leghatékonyabb megoldás egy platformmegbízhatósági modul (TPM) képesség, amely lehetővé teszi a kulcsok tárolása különleges-chip áramkört mely hello a kulcsok nem olvasható, de csak akkor használható, amelyek hello kulcsot használják, de soha nem fedjük fel hello kulcs titkosítási műveletek . Memória titkosítási hello eszköz. Kulcskezelés hello eszközhöz. Hello kód aláírása. | |
| E |Ha hozzáférés-vezérlés hello eszköz. Hitelesítési séma. |Hello eszköz lehetővé teszi, hogy az egyes műveletek toobe végre külső forrásból parancsok alapján, vagy még sérült az érzékelők, ha engedélyezi hello támadás tooperform műveletek egyébként nem érhető el. |Ha engedélyezési sémát hello eszköz | |
| A mező átjáró |S |Hello mező átjáró tooCloud (tanúsítványalapú, PSK, a jogcím-alapú,...) átjáró hitelesítése |Ha valaki mező átjáró hamis, majd azt is jelenthet maga mint bármely olyan eszközről. |A TLS RSA/PSK, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Összes hello azonos kulcstároló és igazolási vonatkozik, az eszközök általában – legjobb esetben van a TPM használatára. IPSec toosupport vezeték nélküli érzékelő hálózatok (WSN) 6LowPAN kiterjesztése. |
| TRID |A mező átjáró hello elleni illetéktelen (TPM)? |Hamisítási támadások, amelyek megtéveszteni végezni, akkor van van szó toofield hello átjáró átjáró okozhat adatokhoz való illetéktelen hozzáférés és adatmódosítás |Memória titkosítás, a TPM által, hitelesítés. | |
| E |Hozzáférés-vezérlési mechanizmus mező átjáró | | | |

Íme néhány példa a ebbe a kategóriába tartozó fenyegetések:

Címhamisítást: Támadó előfordulhat, hogy kinyerése kriptográfiai kulcsokat tárol egy eszközt, vagy hello szoftveres vagy hardveres szinten, és ezt követően hozzáférés hello eszköz hello kulcsokat tárol, egy másik fizikai vagy virtuális eszköz hello identitás hello rendszer lett-e venni.

**Szolgáltatásmegtagadást**: egy eszköz nem működik, vagy történő kommunikációhoz választógomb-használati vagy kivágáshoz fenyegetéseknek zavarja alkalmas megjeleníthetők. Például a felügyeleti fényképezőgép, amelyekről a szándékosan kiejtett power vagy a hálózati kapcsolat nem küld adatokat, minden.

**Illetéktelen**: támadó részben vagy egészben helyettesítheti hello szoftver hello eszközön futó, potenciálisan cseréje hello szoftver tooleverage hello valódi identitás hello eszköz Ha hello billentyűt anyagokat vagy hello kriptográfiai kulcs anyagok okozó létesítményekben volt elérhető toohello tiltott program.

**Illetéktelen**: egy felügyeleti kamera, amely egy üres Előszoba látható pontszámot képe látható ilyen Előszoba fényképe célzó sikerült. Füst vagy tűz érzékelő sikerült készítőnek tekintene valaki rendelkezik egy világosabb rajta. Mindkét esetben hello eszköz valószínűleg technikailag teljesen megbízható hello rendszer felé, de úgy adatokat fog jelenteni.

**Illetéktelen**: támadó előfordulhat, hogy használja ki a kibontandó kulcs jelentős toointercept és ne jelenjen meg többé hello eszközön hello kommunikációs elérési úton található adatok, és cserélje le ellopják hello kulcsokat tárol a hitelesített hamis adatokkal.

**Illetéktelen**: támadó előfordulhat, hogy részben vagy teljesen lecseréli hello szoftver hello eszközön futó, potenciálisan cseréje hello szoftver tooleverage hello valódi identitás hello eszköz Ha hello billentyűt anyagokat vagy hello kriptográfiai kulcs anyagok okozó létesítményekben volt elérhető toohello tiltott program.

**Információk felfedése**: hello eszköz úgy szoftvert futtat, ha úgy szoftverhez sikerült potenciálisan szivárgás lépett fel adatokat toounauthorized felek.

**Információk felfedése**: támadó előfordulhat, hogy kihasználja a kibontott kulcs jelentős tooinject magát a hello kommunikációs útvonalat hello eszköz és a tartományvezérlő vagy mező átjáró közötti vagy felhőalapú átjáró toosiphon ki adatokat.

**Szolgáltatásmegtagadást**: hello eszköz ki van kapcsolva, vagy egy módba kapcsolva, ahol kommunikáció nincs lehetőség (ez utóbbi érték az ipari gépekről sok szándékos).

**Illetéktelen**: hello az eszköz lehet újra konfigurált toooperate állapota ismeretlen toohello (kívül ismert hitelesítési paraméterek) rendszer vezérlik, és így a hibásan lesznek értelmezve adatok megadása

**Jogok kiterjesztése**: megadott funkciót hajtja lehet kényszerített toodo valami mással. Például, amely programozott tooopen fele eltérő módon lehet szelep megtevésztve összes hello tooopen módon.

**Szolgáltatásmegtagadást**: hello eszköz is be kell kapcsolni, ahol kommunikáció nincs lehetőség állapotba.

**Illetéktelen**: hello az eszköz lehet újra konfigurált toooperate állapota ismeretlen toohello (kívül ismert hitelesítési paraméterek) rendszer vezérlik, és így biztosít az adatokat, hibásan lesznek értelmezve.

**-Címek hamisítását vagy Tampering/Repudiation**: Ha nem biztonságos (ez utóbbi érték ritkán hello esetet, amelyben fogyasztói távoli vezérlők) egy támadó névtelenül állíthatók hello állapota az eszköz. Egy jó ábra, amely bekapcsolja a TV és népszerű prankster eszközök távoli szabályozza.

#### <a name="communication"></a>Kommunikáció
Eszközök, eszközök és a mező átjárók és az eszköz és a felhő átjáró közötti kommunikáció görbe körül fenyegetéseket. hello az alábbi táblázat néhány útmutatást hello eszköz és a VPN a nyitott sockets körül rendelkezik:

| **Összetevő** | **Fenyegetések** | **Megoldás** | **Kockázati** | **Megvalósítása** |
| --- | --- | --- | --- | --- |
| Az IoT-központ eszköz |TID |(D) A TLS (PSK/RSA) tooencrypt hello forgalom |Lehallgatás vagy hasonló eszköz hello és hello átjáró hello kommunikációját |Biztonsági hello protokoll szintjén. Az egyéni protokollok, igazolnia kell, hogyan toofigure tooprotect őket. A legtöbb esetben hello kommunikáció akkor történik meg a hello eszköz toohello IoT Hub (az eszköz kezdeményezi hello kapcsolat). |
| Eszköz eszköz |TID |(D) A TLS (PSK/RSA) tooencrypt hello forgalom. |Az eszközök közötti átvitel során adatainak olvasása. Hello adatok illetéktelen módosítása. Túl van terhelve hello eszközt az új kapcsolatok |Biztonsági hello protokoll szintjén (MQTT/AMQP/HTTP/CoAP. Az egyéni protokollok, igazolnia kell, hogyan toofigure tooprotect őket. hello DoS fenyegetés hello megoldás toopeer eszközökön felhő vagy mező átjárón keresztül, és lehet őket az ügyfelek felé hello hálózati csak act. hello társviszony-létesítés vonhat után rendelkező lett közvetítőalapú hello átjáró hello partnerek között közvetlen kapcsolat |
| Külső entitás eszköz |TID |Erős párosítás hello külső entitás toohello eszköz |Lehallgatási hello kapcsolat toohello eszköz. Zavaró hello kommunikáció eszközzel hello |Biztonságosan párosítás hello külső entitás toohello eszköz NFC/Bluetooth LE. Ellenőrző hello működési panel hello eszköz (tényleges) |
| A mező átjáró átjáró |TID |A TLS (PSK/RSA) tooencrypt hello forgalom. |Lehallgatás vagy hasonló eszköz hello és hello átjáró hello kommunikációját |Biztonsági hello protokoll szintjén (MQTT/AMQP/HTTP/CoAP). Az egyéni protokollok, igazolnia kell, hogyan toofigure tooprotect őket. |
| Eszköz átjáró |TID |A TLS (PSK/RSA) tooencrypt hello forgalom. |Lehallgatás vagy hasonló eszköz hello és hello átjáró hello kommunikációját |Biztonsági hello protokoll szintjén (MQTT/AMQP/HTTP/CoAP). Az egyéni protokollok, igazolnia kell, hogyan toofigure tooprotect őket. |

Íme néhány példa a ebbe a kategóriába tartozó fenyegetések:

**Szolgáltatásmegtagadást**: korlátozott eszközök általában alatt álló DoS amikor azokat aktívan figyelni a bejövő kapcsolatok vagy kéretlen datagramok a hálózaton, mivel egy támadó is nyissa meg a sok kapcsolattal párhuzamosan és nem őket szolgáltatás vagy szolgáltatás őket nagyon lassan, vagy a hello lehet árasztott kéretlen forgalommal. Mindkét esetben hello eszköz hatékonyan megjeleníthetők működésképtelenné hello hálózaton.

**Címek hamisítása, az információk felfedése**: korlátozott és speciális eszközre gyakran rendelkezik egy összes biztonsági létesítményekben például jelszó vagy PIN-kód védelmet, vagy teljesen támaszkodnak megbízó hello hálózati, azaz hozzáférést fog Ha egy eszköz hello tooinformation ugyanazon a hálózaton, és a hálózat gyakran csak védik a megosztott kulcs. Amely azt jelenti, amikor hello megosztott titkos toodevice, vagy hálózati felfedett, azt lehetséges toocontrol hello eszköz vagy megfigyelni hello eszközről kibocsátott adatokról.  

**-Címek hamisítását**: támadó előfordulhat, hogy intercept vagy hello szórásos és megszemélyesítés hello kezdeményezőjének (hello középső man) részben felülbírálása

**Illetéktelen**: támadó előfordulhat, hogy intercept vagy részben hello szórás bírálja felül, és hamis információk küldése 

**Információk felfedése:** egy támadó előfordulhat, hogy kihallgassa a közvetítés és engedély nélkül információkhoz **szolgáltatásmegtagadást:** egy támadó előfordulhat, hogy szórásos jelzést hello dzsem, és a megtagadási információk terjesztési

#### <a name="storage"></a>Storage
Minden eszköz és a mező átjáró storage (az Üzenetsor-kezelés hello adatok, operációs rendszer lemezkép tárolási ideiglenes) rendelkezik.

| **Összetevő** | **Fenyegetések** | **Megoldás** | **Kockázati** | **Megvalósítása** |
| --- | --- | --- | --- | --- |
| Eszköz tárhelyén |TRID |Tárolás titkosítása hello naplók aláírása |Adatok hello tárolási (személyazonosításra alkalmas adatokat), telemetriai adatok illetéktelen módosításának olvasása. Illetéktelenül aszinkron vagy gyorsítótárazott parancs vezérlő adatokat. Konfiguráció vagy a belső vezérlőprogram frissítési csomagokat illetéktelenül során gyorsítótárazva, vagy helyileg várólistára vezethet feltörésének tooOS és/vagy a rendszer összetevők |Titkosítás, az üzenethitelesítő kódot (MAC) vagy a digitális aláírás. Ha lehetséges, erős hozzáférés keresztül erőforrásokhoz való hozzáférést vezérlő listák (ACL) vagy hozzáférési engedélyekkel. |
| Az eszköz operációs rendszere kép |TRID | |Az operációs rendszer illetéktelenül / hello OS összetevők cseréje |Csak olvasható az operációs rendszer partíció aláírt operációsrendszer-lemezképek, a titkosítás |
| A mező átjárói tárolás (Üzenetsor-kezelés hello adatok) |TRID |Tárolás titkosítása hello naplók aláírása |Hello tárolási (személyazonosításra alkalmas adatokat), telemetriai adatok illetéktelen módosításának adatok olvasása illetéktelenül módosítsák a várólistára vagy gyorsítótárazott parancs vezérlő adatokat. (Az eszközök vagy mező átjáró szánt) konfigurációja vagy a belső vezérlőprogram frissítési csomagokat illetéktelenül során gyorsítótárazva, vagy helyileg várólistára vezethet feltörésének tooOS és/vagy a rendszer összetevők |A BitLocker |
| A mező az átjáró operációsrendszer-lemezképek |TRID | |Az operációs rendszer illetéktelenül / hello OS összetevők cseréje |Csak olvasható az operációs rendszer partíció aláírt operációsrendszer-lemezképek, a titkosítás |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Eszköz és az esemény feldolgozása/felhő átjáró zóna
Egy átjáró a rendszer, amely lehetővé teszi a távoli kommunikációhoz és több különböző helyére toodevices vagy a mezőparaméter-átjáróinak nyilvános hálózati helyet, általában egy felhőalapú vezérlési és az elemző rendszer, az ilyen rendszerek összevonási felé között. Bizonyos esetekben egy felhőátjáróhoz azonnal megkönnyíthetik hozzáférés toospecial célú eszközök például a táblagépek és telefonok részére. A tárgyalt hello környezetben itt "cloud" célja toorefer dedikált tooa adatfeldolgozási rendszer, amely nincs kötve toohello ugyanazon a helyen hello eszközök vagy mező átjárók hozzá van kapcsolva, és egyes esetekben működési intézkedések megakadályozása fizikai hozzáférés céloz, azonban nem feltétlenül tooa "nyilvános" felhőinfrastruktúráját.  A hálózati virtualizálási átfedő tooinsulate hello felhő átjáró és az összes csatlakoztatott eszközök vagy bármely más hálózati forgalmaktól mező átjárók potenciálisan képezhető le egy felhőátjáróhoz. hello átjáró maga sem egy eszköz, sem a feldolgozási vagy a tárolási létesítmény eszköz adatok; Ezek az eszközök csatlakozhatnak a hello felhőátjáróhoz. eszközök közvetve vagy közvetlenül csatlakoztatott tooit és hello felhő átjáró zóna tartalmaz hello felhőátjáróhoz maga minden mező átjáró együtt.

Átjáró többnyire egyéni beépített szoftvertermék kitett végpontok toowhich mező átjáróval szolgáltatásként fut, és eszközök csatlakoznak. Ilyen úgy kell megtervezni a biztonságot szem előtt. Kövesse az [SDL](http://www.microsoft.com/sdl) folyamatok tervezéséről és kiépítéséről ezt a szolgáltatást. 

#### <a name="services-zone"></a>Szolgáltatások zóna
A rendszer (vagy a tartományvezérlő) egy szoftveres megoldás, amely egy eszközt, vagy mező átjáróval, vagy egy vagy több eszközöket és/vagy toocollect és/vagy a tároló vezérlésének hello célra átjáró felületeihez és/vagy eszköz adatelemzés bemutató, vagy További ellenőrzési célból. Vezérlő rendszereket hello csak entitások, amely személyek való együttműködéshez azonnal elősegíti az ismertető hello hatókörében. hello kivétel köztes fizikai vezérlő felületek az eszközökön, például egy kapcsoló, amely lehetővé teszi, hogy egy személy tooturn hello eszköz ki, vagy más tulajdonságainak módosítása, és, amelynek nincs digitálisan elérhető funkcionális megfelelője. 

Köztes fizikai vezérlő felületek azok, amelyben tetszőleges logika szabályozó korlátozza-e hello függvény hello fizikai vezérlő felület úgy, hogy megfelelője távolról kezdeményezhetők, vagy távoli bemeneti bemeneti ütközik köszönhetően elkerült – ilyen lehet vezérlő felületek adatbázisoktól fogalmilag csatolt tooa helyi verziókezelő rendszert, hogy használja hello alapul szolgáló funkcióját, lehet, hogy bármely más távvezérlési rendszer, amely az eszköz hello csatolt tooin párhuzamos. Felső fenyegetések toohello felhőszámítási lehet olvasni a [felhőalapú biztonsági Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) lap.

## <a name="additional-resources"></a>További források
Tekintse meg a következő cikkekben további információt toohello:

* [SDL fenyegetés modellezési eszköz](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [A Microsoft Azure IoT-referenciaarchitektúra](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)

## <a name="see-also"></a>Lásd még:
További információ az biztonságossá tétele az IoT-megoldásból toolearn lásd [IoT üzembe][lnk-security-deployment].

Akkor is is felfedezheti hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:

* [Előre konfigurált prediktív karbantartási megoldás áttekintése][lnk-predictive-overview]
* [Gyakran ismételt kérdések az IoT Suite-ról][lnk-faq]

Az IoT-központ biztonsági olvashat [vezérlő hozzáférés tooIoT Hub] [ lnk-devguide-security] az IoT Hub fejlesztői útmutató hello.

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md

[lnk-security-deployment]: iot-suite-security-deployment.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md