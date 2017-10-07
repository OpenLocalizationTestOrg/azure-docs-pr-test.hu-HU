---
title: "aaaStorSimple 8000 frissítése 0,1 kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új szolgáltatások és javításokat, nyissa meg a problémákat, és ismerteti hello elérhető megoldásai 2014. októberi Microsoft Azure StorSimple release (frissítés 0,1)."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: fd35e3c3-4770-460c-999d-f72ab7053a20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 684e31cb0b356ec59a9d6c245e5d2bc062cc4f0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>A StorSimple 8000 Series Update 0,1 kibocsátási megjegyzések – 2014. október
## <a name="overview"></a>Áttekintés
kibocsátási megjegyzések a következő hello hello kritikus megnyitott problémák azonosításához a StorSimple 8000 Series frissítés 0,1 2014. októberi-ben. Hello StorSimple szoftverek listáját és a belső vezérlőprogram-frissítésekre ebben a kiadásban szereplő is tartalmazzák. Ez az első kiadása hello hello a StorSimple 8000 Series kiadási verzió általánosan elérhető a 2014. július történt, és 6.3.9600.17312 toosoftware verziója megfelel-e után.  

Azt javasoljuk, hogy keressen, és elérhető frissítések alkalmazása hello eszköz telepítése után azonnal. Kapcsolja be az automatikus frissítések toodownload is, és fontos frissítéseket telepíti a Microsoft, amint azok kiadásakor. További információkért lásd: hogyan túl[a StorSimple eszköz frissítése](storsimple-update-device.md).  

Tekintse át a hello információ hello kibocsátási megjegyzések a StorSimple megoldásban hello frissítések telepítése előtt.  

> [!IMPORTANT]
> * Hello StorSimple Manager szolgáltatás és a Windows PowerShell-nem használható a StorSimple tooinstall hello október frissíti.  
> * hello frissítések általában körülbelül 3 óra toocomplete vesz igénybe.  
> * hello StorSimple. októberi kiadása nem tartalmaz semmilyen frissítések toohello StorSimple virtuális eszköz. Elérhető Windows-frissítések, többek között a legutóbbi biztonsági javítások, továbbra is alkalmazhat, de nem jelenik meg egy változást hello virtuális eszköz verzióját.  
> 
> 

Győződjön meg arról, hogy a következő előfeltételek hello is fennállnak előzetes tooupdating a StorSimple eszközt.  

* Győződjön meg arról, hogy mindkét eszközvezérlők futnak, a frissítések keresése előtt. Ha bármelyik vezérlő nem fut, hello vizsgálat sikertelen lesz. hello, tartományvezérlői állapota kifogástalan, amelyek tooverify lépjen túl**hardverállapot** alatt hello **karbantartási** lap. Ha az összetevő, amely **kezeléséről**, forduljon a Microsoft Support adatbázist.  
* Győződjön meg arról, hogy mindkét 0 vezérlő rögzített IP-címek és a vezérlő 1 k toohello Internet kapcsolódhatnak, mivel ezek hello frissítések toohello eszköz karbantartásához használatosak. Használhatja a hello [Test-Connection parancsmag](https://technet.microsoft.com/library/hh849808.aspx) tooping egy ismert címet, például az outlook.com, a tartományvezérlő hello tooverify hello hálózaton kívüli kapcsolódási toohello hálózaton kívül van.  
* Győződjön meg arról, hogy hello szükséges kimenő portok érhető el a StorSimple eszköz kimenő kommunikációra. További információkért lásd: hello [a StorSimple eszköz hálózatkezelési követelményei](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
* Ha hello eszköz szoftver verziója régebbi, mint 6.3.9600.17312 (2014. októberi frissítés), tiltsa le a hello Data 2 és a Data 3 portok, ha engedélyezve van, hello frissítés elkezdése előtt. Ha nem adja meg hello Data 2 vagy a Data 3 portok hello frissítés alkalmazásakor engedélyezve van, az eszköz vezérlő toogo helyreállítási módba okozhat. Adjon vegye figyelembe, hogy hello hálózati illesztők letiltása, az összes kapcsolódó hello kötetek a rendszer offline állapotra hello i/o hello ideje alatt hello frissítés fog működni.  

## <a name="whats-new-in-hello-october-release"></a>What's new in hello. októberi kiadás
A frissítés hello a következő fejlesztéseket tartalmazza:

* Ezután már használhatja hello StorSimple Manager szolgáltatás felhasználói felületi toomanage a eszközvezérlők. hello felügyeleti műveletek közé tartoznak indítsa újra a Leállítás, vagy kapcsolja be a tartományvezérlő. További információ: túl[kezelése StorSimple eszközvezérlők](storsimple-manage-device-controller.md).  
* Ütemezheti a WAN sávszélesség-foglalással rendelkeznek tooday-hét a és a nap idő kombinációk szerint. Ez lehetővé teszi toomake jobb WAN sávszélesség használatának végezze alacsony forgalmú időszakban. Különböző sávszélesség sablonok engedélyezve van a különböző kötettárolók. További információ: túl[StorSimple sávszélesség sablonok kezelésére](storsimple-manage-bandwidth-templates.md).  
* E-mail értesítések tooproactively értesíteni hello rendszergazdájával vagy rendszergazdáival, és a meglévő vagy valószínűleg jövőbeli problémák másokat is konfigurálhat. További információ: túl[riasztási beállítások megadását](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-hello-october-release"></a>Javított hello. októberi kiadás
hello következő táblázat az összefoglalást tartalmazza a problémákat, amelyek a frissítés javítva lett.  

| Nem. | Szolgáltatás | Probléma | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- |
| 1 |Hálózati illesztők |Hello korábbi kiadásában, hello hálózati illesztők DATA 2 és a DATA 3 hello szoftverfrissítési lettek cserélve. Ez a frissítés megszüntetése. Törölje a hello beállításokat, és a hálózati illesztők letiltása, hello frissítés telepítése előtt. Hello frissítés telepítése után fog tooreconfigure konfigurációkezelővel. |Igen |Nem |
| 2 |Támogatási csomag |Hello előző kiadásban, ha a Windows PowerShell hello próbálkozott **Export-HcsSupportPackage** parancsmag tooretrieve hello alaplapi felügyeleti vezérlő (BMC) naplózza, hello művelet a következő figyelmeztetés hello miatt meghiúsult: "hello a művelet ezen a vezérlőn sikeres volt, de hello társ tartományvezérlőn toohello következő miatt nem sikerült egy vagy több hiba. Győződjön meg arról hogy hello társ állapota kifogástalan, és hogy hello aktuális csomópont toohello társ csatlakoznak." Ez a probléma fennáll most. |Igen |Nem |
| 3 |Eszköz feladatátvétel |Hello a korábbi verzióban, hiba történt egy esélye adatinkonzisztenciát, ha egy **felderítése a biztonsági mentés** feladat sikertelen volt, egy eszköz a feladatátvétel során. Ez a probléma fennáll most. |Igen |Nem |
| 4 |Eszköz feladatátvétel |Hello korábbi kiadásában, egy eszköz feladatátvétel után a biztonsági mentése volt látható, de hello társított kötettároló nem voltak hello céleszközön. Ez a probléma fennáll most. |Igen |Nem |
| 5 |Eszköz feladatátvétel |Hello a korábbi verzióban történt hiba, amely veremtúlcsordulást okozhat toodata inkonzisztenciája miatt, ha csatlakozási problémák felhő történtek hello beállításjegyzék-visszaállítási művelet során hello felhőbeli biztonsági másolatok felsorolása. |Igen |Nem |
| 6 |Belső vezérlőprogram frissítése |Hello a korábbi verzióban hello eszköz belső vezérlőprogram frissítési feladat sikertelen volt, és jelenik meg a hiba, amely hello parancsmagok nem volt felismerhető, és amely következtében hello frissítést nem sikerült. hello vezérlő majd helyreállítási módba merült fel. Ez a probléma fennáll most. |Igen |Nem |
| 7 |Telepítés |Most már rögzített nem kívánt lemezképet készít róla megfelelően telepítése során hello eszköz által okozott hibákat. |Igen |Nem |
| 8 |Gyári beállítások visszaállítása |Most már továbbléphet opcionálisan hello belső vezérlőprogram-ellenőrzést a gyári beállítások visszaállítása. Ez az hello korábbi kiadásáról. |Igen |Nem |
| 9 |Gyári beállítások visszaállítása |Hello a korábbi verzióban gyári alaphelyzetbe állítása a parancsmag futtatásakor a rendszer belső vezérlőprogram verziója ellenőrzések végzett csak az egyes hardverösszetevők. További belső vezérlőprogram ellenőrzi a rendszer hello hello folyamat során, így hello alaphelyzetbe állítása toofail első újraindítása után történtek. Ez a javítás biztosítja, hogy minden hello belső vezérlőprogram ellenőrzéseket hello gyári alaphelyzetbe állítása parancsmagot futtatja, és mielőtt hello első rendszer újraindul. |Igen |Nem |
| 10 |Tárolási fiók kulcs Elforgatás |Hello **Invoke-HcsmServiceDataEncryptionKeyChange** parancsmaggal toorotate hello tárfiókkulcsok most kér hello felhasználói tooenter hello szolgáltatásadat-titkosítási kulcs. Ez az hello korábbi kiadásáról mely hello a szolgáltatásadat-titkosítási kulcs egy beágyazott paraméter lett átadva. |Igen |Nem |
| 11 |Feladat-visszavétel 24 órában |A vészhelyreállítás során hello tisztítás hello forrás eszközön áramtalanították, ezért a feladat-visszavétel toofail nem következtek be. Ez kijavítása ebben a kiadásban. |Igen |Nem |

## <a name="known-issues-in-hello-october-release"></a>Ismert problémák hello. októberi kiadás
hello a következő táblázat összefoglalja az ismert problémákról, ebben a kiadásban.

| Nem. | Szolgáltatás | Probléma | Megjegyzések/megoldás | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- | --- |
| 1 |Gyári beállítások visszaállítása |Bizonyos esetekben a gyári beállítások visszaállítása, végrehajtásakor hello StorSimple eszköz előfordulhat, hogy, és megjeleníti ezt az üzenetet: **toofactory alaphelyzetbe állítása folyamatban van a (fázis 8)**. Ez akkor fordul elő, ha a CTRL + C gombra, amíg hello parancsmag folyamatban van. |CTRL + C nem nyomja meg a gyári beállítások visszaállítása kezdeményezése után. Ha már ebben az állapotban, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 2 |Gyári beállítások visszaállítása |A StorSimple eszköz GA tooOctober 2014 kiadásáról frissített hajtsa végre a nem a gyári beállítások visszaállítása. |Ez a művelet csak akkor fog működni, a javítás telepítve van. Lépjen kapcsolatba a Microsoft Support tooget a szükséges javítás. |Igen |Nem |
| 3 |Lemez kvórum |Ritka esetekben ha hello EBOD szolgáltatással 8600-eszköz lemezek hello többsége megszakadt a kapcsolat nincs lemez kvórum eredményezi majd hello tárolókészlet sem kapcsolódik a hálózathoz. Offline állapotban maradnak, még akkor is, ha hello lemez újracsatlakoztatását. |Szüksége lesz tooreboot hello eszköz. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 4 |Felhőalapú pillanatfelvétel hibák |Ritka esetekben egy felhő-pillanatfelvételt hello hiba miatt meghiúsulhat **elérte a maximális biztonsági korlátot**. Ez akkor fordul elő, ha az túllépi a hello 255 online klónok ugyanarra az eszközre, a hello azonos eredeti kötet, amelyet már töröltek. | |Igen |Igen |
| 5 |Helytelen vezérlő azonosítója |A vezérlő helyettesítő végrehajtásakor vezérlő 0 lehet, hogy megjelennek, a vezérlő 1. A tartományvezérlő csere közben hello kép hello társ-csomópont betöltésekor hello vezérlő azonosítója is megjelennek kezdetben, hello társ vezérlő azonosítója. Ritka esetben ez a viselkedés előfordulhat, hogy is látható. a rendszer újraindítását követően. |Nincs felhasználói beavatkozásra szükség. Ez a helyzet magától megoldódik, amikor hello vezérlő helyettesítő befejeződése után. |Igen |Nem |
| 6 |Eszközfigyelési diagramok |A StorSimple Manager szolgáltatás hello hello eszköz figyelési diagramok nem működnek a Basic, vagy az NTLM-hitelesítés nincs engedélyezve az hello proxykiszolgálót hello eszközhöz. |Hello webproxy konfigurálása a StorSimple Manager szolgáltatásban regisztrált úgy, hogy a hitelesítés be van állítva tooNONE hello eszköz módosításához. toodo StorSimple Set-HcsWebProxy parancsmag, ez futtatási hello hello Windows PowerShell. |Igen |Igen |
| 7 |Tárfiókok |Hello tárolási toodelete hello tárolási szolgáltatásfiók használata egy nem támogatott forgatókönyvet. Ez vezet tooa helyzetet, amelyben felhasználói nem lehet adatokat beolvasni. | |Igen |Igen |
| 8 |Eszköz feladatátvétel |A kötettároló hello azonos forrás eszköz toodifferent Céleszközök nem támogatja a több feladatátvétele. |Egy egyetlen elhalt eszközt toomultiple eszközökről feladatátvételi hello kötettárolók először átadja a feladatokat eszköz hello az adatok tulajdonjoga elvesztése biztosítják. Egy feladatátvétel után ezek kötettárolók jelenik meg, vagy a klasszikus Azure portálon hello meg eltérően viselkednek. |Igen |Nem |
| 9 |Telepítés |StorSimple Adapter SharePoint telepítéshez, során kell tooprovide egy eszköz IP-cím ahhoz, hogy hello telepítés toofinish sikeresen. | |Igen |Nem |
| 10 |Webproxy |Ha a webproxy-konfigurációja HTTPS hello meg van adva protokoll, akkor az eszköz-szolgáltatások közötti kommunikáció néven érinti, és hello eszköz nélküli módba. Támogatási csomag is tudna generálni hello folyamat során fel jelentős erőforrásokat az eszközön. |Győződjön meg arról, hogy hello webes proxy URL-címe HTTP, a megadott protokoll hello. További információt túl[az eszköz webalkalmazás-proxy konfigurálása](storsimple-configure-web-proxy.md). |Igen |Nem |
| 11 |Webproxy |Ha a konfigurált és engedélyezett a webalkalmazás-proxy regisztrált egy eszközt, akkor szüksége lesz toorestart hello aktív vezérlő az eszközön. | |Igen |Nem |
| 12 |Magas felhő késéssel és nagy i/o-munkaterhelés |Amikor a StorSimple eszköz nagyon magas felhő késések (másodperc sorrendben) és a magas i/o-munkaterhelés észlel, hello eszköz kötetek csökkent állapotba, és hello i/o "az eszköz nem áll készen" hiba miatt sikertelen lehet. |Meg kell, újraindítás hello eszközvezérlők toomanually, vagy végezzen el egy eszköz feladatátvételi toorecover ebben a helyzetben a. |Igen |Nem |

## <a name="physical-device-updates-in-hello-october-release"></a>Hello. októberi kiadás fizikai eszköz frissítése
Ha ezek a frissítések alkalmazott tooa fizikai eszköz, a hello szoftververzió too6.3.9600.17312 változik. Hacsak másként nincs megadva, a kibocsátási megjegyzéseket tooall modellek hello StorSimple eszköz alkalmazni. Ezekről a frissítésekről további információkért lásd: [2014. októberi fizikai készülék szoftverfrissítés Microsoft Azure StorSimple alapplatformjaként](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-hello-october-release"></a>Soros csatlakozású SCSI (SAS) vezérlő és a belső vezérlőprogram frissítések hello. októberi kiadás
Ebben a kiadásban frissíti hello illesztőprogram és a fizikai eszköz hello vezérlő hello belső vezérlőprogramját. Hello SAS vezérlő frissítéssel kapcsolatos további információkért lásd: [2014. októberi frissítés a Microsoft Azure StorSimple készülék LSI SAS-vezérlők](http://support.microsoft.com/kb/2987020).   

Ebben a kiadásban, amely hello eszköz hardverösszetevők kapcsolatos problémák összegző vezérlőprogram-frissítés is vonatkozik. Hello belső vezérlőprogram frissítéssel kapcsolatos további információkért lásd: [2014. októberi vezérlőprogram-frissítés a Microsoft Azure StorSimple készülék](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-hello-october-release"></a>Virtuális eszköz frissítések hello. októberi kiadás
Ez a kiadás nem tartalmaz virtuális eszköz hello frissítéseket. A frissítés telepítése nem módosítja a virtuális eszköz hello szoftverének verziójával.

