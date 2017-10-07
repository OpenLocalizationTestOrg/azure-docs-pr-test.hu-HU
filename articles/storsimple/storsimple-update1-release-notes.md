---
title: "aaaStorSimple 8000 Series Update 1.2 kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új funkciókat, problémák és megoldások ismerteti a StorSimple 8000 Series Update 1.2-es."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c9aae87-6f77-44b8-b7fa-ebbdc9d8517c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7f564b794573fc3302ab15732e8dd85632ab9243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-12-release-notes-for-your-storsimple-8000-series-device"></a>1.2-es kibocsátási megjegyzések a StorSimple 8000 series eszköz frissítése

## <a name="overview"></a>Áttekintés
hello alábbi kibocsátási megjegyzések leírják hello új szolgáltatásokat és a StorSimple 8000 Series Update 1.2 hello kritikus megnyitott problémák azonosításához. Hello StorSimple szoftver, illesztőprogram és lemez belső vezérlőprogram-frissítésekre ebben a kiadásban szereplő listáját is tartalmazzák. 

1.2-es frissítés alkalmazott tooany StorSimple eszköz kiadásban (GA), frissítés 0,1, 0,2 frissítés vagy frissítési 0,3 szoftver lehet. 1.2-es frissítés nem érhető el, ha az eszköz fut, 1. frissítés vagy frissítési 1.1. Ha az eszköz kiadásban (GA) fut, akkor segítségért [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) tooassist, a frissítés telepítésével.

a következő tábla listák hello eszköz szoftver verziója megfelelő, 1, 1.1-es és 1.2-es tooUpdates hello.

| Ha az update futtatása... | Ez az eszköz szoftververziók. |
| --- | --- |
| 1.2-es frissítés |6.3.9600.17584 |
| 1.1-es frissítés |6.3.9600.17521 |
| Frissítés 1.0 |6.3.9600.17491 |

Tekintse át a hello kibocsátási megjegyzések hello központi telepítése előtt frissítse a StorSimple megoldásban lévő hello információt. További információkért lásd: hogyan túl[1.2-es frissítés telepítése a StorSimple eszköz](storsimple-install-update-1.md). 

> [!IMPORTANT]
> * A frissítés (beleértve a Windows hello-frissítések) tart a körülbelül 5 – 10 óra tooinstall. 
> * 1.2-es frissítés szoftver, a LSI illesztőprogram és a belső vezérlőprogram-frissítésekre van. tooinstall, kövesse az utasításokat hello a [1.2-es frissítés telepítése a StorSimple eszköz](storsimple-install-update-1.md).
> * Új kiadásokban nem láthatók frissítések azonnal, mert a hello frissítések fázisokra bontva történő bevezetéséhez végezzük. A néhány nap múlva újra a frissítések keresését, ezek hamarosan elérhető lesz.
> 
> 

## <a name="whats-new-in-update-12"></a>1.2-es frissítés újdonságai
Ezek a funkciók először kiadott frissítés 1, de a felhasználók készült elérhető tooa korlátozott készletét. Hello frissítés 1.2-es kiadással hello StorSimple felhasználók többsége a következő új szolgáltatásait és fejlesztéseit hello jelenik meg:

* **5000-7000-es adatsorozat too8000 sorozat eszközeire való áttelepítést** – ebben a kiadásban szolgáltatást vezet be egy új áttelepítési, amely lehetővé teszi, hogy a hello StorSimple 5000-7000-es adatsorozat készülék felhasználók toomigrate az adatok tooa a StorSimple 8000 series fizikai berendezés vagy egy virtuális készülék. hello áttelepítési funkcióját két fő értéknövelő rendelkezik:                                                                  
  
  * **Az üzletmenet folytonossága**, 5000-7000-es adatsorozat készülékek too8000 adatsorozat készülékek a meglévő adatok áttelepítésének engedélyezésével.
  * **Továbbfejlesztett hello 8000 sorozat készülékek a szolgáltatás által kínált**, hatékony központi felügyelet több készülékek StorSimple Manager szolgáltatással, például jobb osztály hardver és a belső vezérlőprogram, a virtuális készülékek, az adatok mobilitási frissítése és szolgáltatások jövőbeli terv hello.
    
    Tekintse meg a toohello [áttelepítési útmutató](http://www.microsoft.com/download/details.aspx?id=47322) kapcsolatos részletes tudnivalókért toomigrate egy 5000-7000-es adatsorozat tooan 8000 sorozat eszközét. 
* **Rendelkezésre állás biztosításához az Azure Government Portal hello** – StorSimple hello Azure Government portálon mostantól érhető el. Lásd: hogyan túl[egy hello Azure Government Portal a StorSimple eszköz üzembe helyezése](storsimple-deployment-walkthrough-gov.md).
* **Egyéb felhőszolgáltatók támogatása** – hello egyéb felhőszolgáltatók, támogatott Amazon S3, Amazon S3 with RRS, HP és OpenStack (béta).
* **Storage API-k toolatest frissítése** – ezzel a kiadással, a StorSimple lett toohello legújabb Azure Storage szolgáltatás API-k frissítése. 1 frissítés előtti szoftvert verzióját futtató StorSimple 8000 sorozat eszközeire (kiadásban 0,1, 0,2 és 0,3) hello Azure Storage szolgáltatás API-k 2009. július 17 régebbi verzióját használja. Frissített hello leírtaknak [kapcsolatos tárolási szolgáltatásverziók eltávolításának hirdetmény](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx), 1 megjelenésével 2016 augusztusától, amelyet ezen API-k elavulttá válik. Rendkívül fontos, hogy telepítenie hello a StorSimple 8000 Series Update 1 előzetes tooAugust 1, 2016. Ha így sem működik a toodo, StorSimple eszközökhöz leáll, megfelelően működik-e.
* **Támogatja a zóna redundáns tárolás (ZRS)** – hello frissítési toohello legújabb verziójával hello Storage API-k, hello StorSimple 8000 series a zóna redundáns tárolás (ZRS) a hozzáadása tooLocally redundáns tárolás (LRS) és a georedundáns támogatja Tárolás (GRS). Tekintse meg a toothis [cikk az Azure Storage redundancia beállítások](../storage/common/storage-redundancy.md) zrs-t a részletekért.
* **Kezdeti telepítés és a frissítési élmény fokozott** – ebben a kiadásban hello a telepítési és frissítési folyamatok javítása. hello beállítása varázsló hello telepítési továbbfejlesztett tooprovide visszajelzés toohello felhasználó esetén hello hálózati konfigurációs és a tűzfal beállításai helytelenek. További diagnosztikai parancsmagok toohelp adtak meg hálózati hello eszköz hibaelhárítás. Lásd: hello [telepítési cikk hibaelhárítási](storsimple-troubleshoot-deployment.md) hello diagnosztikai parancsmagjai a hibaelhárításhoz használható további információt.

## <a name="issues-fixed-in-update-12"></a>A frissítés 1.2 megoldott problémák
a következő táblázat hello 1, 1.2-es és 1.1-es frissítésben javított problémák összegzését tartalmazza.    

| Nem. | Szolgáltatás | Probléma | A rögzített frissítés | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- | --- |
| 1 |A Windows PowerShell-lel |Ha a felhasználó el távolról hello StorSimple eszközt a StorSimple Windows PowerShell használatával, és majd el hello beállítása varázsló, egy összeomlás, amint Data 0 IP lett bemeneti történt. A hiba az 1. frissítés most javítását. |1. frissítés |Igen |Igen |
| 2 |Gyári beállítások visszaállítása |Bizonyos esetekben a gépen hajtotta végre a gyári beállítások visszaállítása, ha hello StorSimple eszköz akadt vált, és ez az üzenet jelenik meg: **toofactory alaphelyzetbe állítása folyamatban van a (fázis 8)**. Ez történt, ha a CTRL + C billentyűt megnyomva, miközben folyamatban volt hello parancsmag. A hiba most javítását. |1. frissítés |Igen |Nem |
| 3 |Gyári beállítások visszaállítása |Miután egy hibás kettős vezérlő gyári alaphelyzetbe állítása, juthattak a tooproceed eszközregisztráció. Ennek következtében a nem támogatott rendszerben. 1. frissítés egy hibaüzenet jelenik meg, és regisztrációs le van tiltva az eszközön, hogy rendelkezik egy sikertelen vissza a gyári beállításokat. |1. frissítés |Igen |Nem |
| 4 |Gyári beállítások visszaállítása |Bizonyos esetekben a téves pozitív eltérés figyelmeztetések fordultak elő. 1. frissítést futtató eszközök nem lesznek generálva riasztások helytelen eltérés. |1. frissítés |Igen |Nem |
| 5 |Gyári beállítások visszaállítása |Ha megszakadt, a gyári beállítások visszaállítása korábbi toocompletion hello megadott eszköz helyreállítási módban, és nem engedélyezte a Windows PowerShell tooaccess lel. A hiba most javítását. |1. frissítés |Igen |Nem |
| 6 |Vészhelyreállítás |A vész-helyreállítási hiba volt rögzített, amelynek a vész-Helyreállítási sikertelen lesz a biztonsági mentések hello céleszközön hello felderítés során. |1. frissítés |Igen |Igen |
| 7 |Figyelési LED |Bizonyos esetekben a készülék hátsó hello LED ellenőrzés nem jelezte megfelelő állapotáról. kék hello LED ki lett kapcsolva. DATA 0 és 1 LED adatok villogó volt, még ha a felületek nem voltak konfigurálva. hello a problémát, és figyelési LED most jelzi hello megfelelő állapotáról. |1. frissítés |Igen |Nem |
| 8 |Figyelési LED |Bizonyos esetekben a 1. frissítés alkalmazása után hello aktív vezérlő hello kék fény ki van kapcsolva ezáltal rögzített tooidentify hello aktív vezérlő így. A probléma kijavítása a javítás kiadásban. |1.2-es frissítés |Igen |Nem |
| 9 |Hálózati illesztők |A korábbi verziókban nem átirányítható átjáróként konfigurált a StorSimple eszköz sikerült kapcsolat nélkül. Ebben a kiadásban Data 0 útválasztási metrikáját hello történt hello legalacsonyabb; ezért még akkor is, ha más hálózati illesztők felhő engedélyezve, minden hello felhőalapú forgalom hello eszközön keresztül lesznek átirányítva Data 0. |1. frissítés |Igen |Igen |
| 10 |Biztonsági másolatok |Update 1 okozó biztonsági mentések toofail 24 napos kijavítása után hello javítás hiba szabadítsa fel a frissítési 1.1. |1.1-es frissítés |Igen |Igen |
| 11 |Biztonsági másolatok |A korábbi verziókban programhiba gyenge teljesítményt, a felhőalapú pillanatfelvételek alacsony módosítás aránnyal eredményezett. Ez a hiba kijavítása ebben a kiadásban javítás. |1.2-es frissítés |Igen |Igen |
| 12 |Frissítések |Frissítés az 1., amely hello, tartományvezérlői toogo okozott helyreállítási módba, és sikertelen frissítés jelentett hiba a javítás kiadásban kijavítása. |1.2-es frissítés |Igen |Igen |

## <a name="known-issues-in-update-12"></a>A frissítés 1.2 ismert problémák
hello a következő táblázat összefoglalja az ismert problémákról, ebben a kiadásban.

| Nem. | Szolgáltatás | Probléma | Megjegyzések/megoldás | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- | --- |
| 1 |Lemez kvórum |Ritka esetekben ha hello EBOD szolgáltatással 8600-eszköz lemezek hello többsége megszakadt a kapcsolat nincs lemez kvórum eredményezi majd hello tárolókészlet sem kapcsolódik a hálózathoz. Offline állapotban maradnak, még akkor is, ha hello lemez újracsatlakoztatását. |Szüksége lesz tooreboot hello eszköz. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 2 |Helytelen vezérlő azonosítója |A vezérlő helyettesítő végrehajtásakor vezérlő 0 lehet, hogy megjelennek, a vezérlő 1. A tartományvezérlő csere közben hello kép hello társ-csomópont betöltésekor hello vezérlő azonosítója is megjelennek kezdetben, hello társ vezérlő azonosítója. Ritka esetben ez a viselkedés előfordulhat, hogy is látható. a rendszer újraindítását követően. |Nincs felhasználói beavatkozásra szükség. Ez a helyzet magától megoldódik, amikor hello vezérlő helyettesítő befejeződése után. |Igen |Nem |
| 3 |Tárfiókok |Hello tárolási toodelete hello tárolási szolgáltatásfiók használata egy nem támogatott forgatókönyvet. Ez vezet tooa helyzetet, amelyben felhasználói nem lehet adatokat beolvasni. |Igen |Igen | |
| 4 |Eszköz feladatátvétel |A kötettároló hello azonos forrás eszköz toodifferent Céleszközök nem támogatja a több feladatátvétele. Eszköz feladatátvételi egy egyetlen elhalt eszközt toomultiple eszközökről hello kötettárolók először átadja a feladatokat eszköz hello az adatok tulajdonjoga elvesztése biztosítják. Egy feladatátvétel után ezek kötettárolók jelenik meg, vagy a klasszikus Azure portálon hello meg eltérően viselkednek. | |Igen |Nem |
| 5 |Telepítés |StorSimple Adapter SharePoint telepítéshez, során kell tooprovide egy eszköz IP-cím ahhoz, hogy hello telepítés toofinish sikeresen. | |Igen |Nem |
| 6 |Webproxy |Ha a webproxy-konfigurációja HTTPS hello meg van adva protokoll, akkor az eszköz-szolgáltatások közötti kommunikáció néven érinti, és hello eszköz nélküli módba. Támogatási csomag is tudna generálni hello folyamat során fel jelentős erőforrásokat az eszközön. |Győződjön meg arról, hogy hello webes proxy URL-címe HTTP, a megadott protokoll hello. További információ: túl[az eszköz webalkalmazás-proxy konfigurálása](storsimple-configure-web-proxy.md). |Igen |Nem |
| 7 |Webproxy |Ha a konfigurált és engedélyezett a webalkalmazás-proxy regisztrált egy eszközt, akkor szüksége lesz toorestart hello aktív vezérlő az eszközön. | |Igen |Nem |
| 8 |Magas felhő késéssel és nagy i/o-munkaterhelés |Amikor a StorSimple eszköz nagyon magas felhő késések (másodperc sorrendben) és a magas i/o-munkaterhelés észlel, hello eszköz kötetek csökkent állapotba, és hello i/o "az eszköz nem áll készen" hiba miatt sikertelen lehet. |Meg kell, újraindítás hello eszközvezérlők toomanually, vagy végezzen el egy eszköz feladatátvételi toorecover ebben a helyzetben a. |Igen |Nem |
| 9 |Azure PowerShell |Hello StorSimple parancsmag használatakor **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - először 1 - várakozási** tooselect hello első objektum, így hozhat létre egy új **VolumeContainer** objektum hello parancsmag minden hello objektumot ad vissza. |Hello parancsmag burkolása zárójelek között az alábbiak szerint: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - először 1 - várakozási** |Igen |Igen |
| 10 |Migrálás |Több kötet tároló áttelepítési lett átadva, hello Európai legfrissebb biztonsági mentés esetén csak az első kötettároló hello pontos. Emellett a párhuzamos áttelepítés hello hello első kötettároló az első 4 biztonsági mentések áttelepítése után kezdődik. |Azt javasoljuk, hogy egyszerre több kötet tároló át. |Igen |Nem |
| 11 |Migrálás |Hello visszaállítása után a kötetek nem lettek hozzáadva toohello biztonsági mentési házirend vagy hello virtuális lemez csoport. |A kötetek tooa a biztonsági mentési házirend rendelés toocreate biztonsági mentései kell tooadd. |Igen |Igen |
| 12 |Migrálás |Hello áttelepítés befejezése után hello 5000/7000-es adatsorozat eszköz nem kell-e érni hello áttelepített adatok tárolók. |Azt javasoljuk, hogy törölje-e hello áttelepített adatok tárolók hello áttelepítés befejezése és véglegesítése után. |Igen |Nem |
| 13 |Klónozás és vész-Helyreállítási |1. frissítést futtató a StorSimple eszköz nem tudja klónozni, vagy hajtsa végre a vész-helyreállítási tooa eszköz 1 frissítés előtti szoftvert futtató. |Szüksége lesz tooupdate hello target eszköz tooUpdate 1 tooallow ezeket a műveleteket |Igen |Igen |
| 14 |Migrálás |Áttelepítés konfigurációs biztonsági másolat nem tud az adatsorozat 5000-7000-es eszközön, kötet csoportok nem társított kötetekkel vannak. |Nincs társított kötetek összes hello üres kötet csoportok törlése, majd próbálkozzon újra a hello konfigurációs biztonsági másolat. |Igen |Nem |

## <a name="physical-device-updates-in-update-12"></a>A frissítés 1.2 fizikai eszköz frissítése
Ha javítás frissítés 1.2 alkalmazott tooa fizikai eszköz (előzetes verzió-tooUpdate 1 fut), a hello szoftververzió too6.3.9600.17584 változik.

## <a name="controller-and-firmware-updates-in-update-12"></a>A frissítés 1.2-es tartományvezérlő és a belső vezérlőprogram frissítések
Ebben a kiadásban frissíti hello illesztőprogram és hello lemez belső vezérlőprogram az eszközön.

* Hello SAS vezérlő frissítéssel kapcsolatos további információkért lásd: [Update 1 LSI SAS-vezérlők, a Microsoft Azure StorSimple készülék](https://support.microsoft.com/kb/3043005). 
* Hello lemez belső vezérlőprogram frissítéssel kapcsolatos további információkért lásd: [lemez belső vezérlőprogram frissítése 1 Microsoft Azure StorSimple alapplatformjaként](https://support.microsoft.com/kb/3063416).

## <a name="virtual-device-updates-in-update-12"></a>1.2-es frissítés a virtuális eszköz frissítése
A frissítés alkalmazott toohello virtuális eszköz nem lehet. Új virtuális eszközök létrehozott toobe kell. 

## <a name="next-steps"></a>Következő lépések
* [A saját eszközére telepített frissítés 1.2](storsimple-install-update-1.md).

