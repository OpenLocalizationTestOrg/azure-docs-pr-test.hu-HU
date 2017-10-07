---
title: "aaaStorSimple 8000 frissítése 0,3 kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új szolgáltatások és javításokat, nyissa meg a problémákat, és ismerteti hello elérhető megoldásai 2015. február Microsoft Azure StorSimple release (0,3. frissítés)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: b01bfd04-f9f8-45f4-ade8-95ac2b979e6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 53638f9d286b2085c6b45f9e3fae75c34fc73e7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>A StorSimple 8000 Series Update 0,3 kibocsátási megjegyzések – 2015. február
## <a name="overview"></a>Áttekintés
kibocsátási megjegyzések a következő hello hello kritikus megnyitott problémák azonosításához a StorSimple 8000 Series Update 0,3, amely 2015. február. Hello StorSimple szoftverek listáját és a belső vezérlőprogram-frissítésekre ebben a kiadásban szereplő is tartalmazzák. Ez az hello harmadikként kiadott után hello a StorSimple 8000 Series kiadási verziójával történt a 2014. július általánosan elérhető.

A frissítés nem módosul hello. januári frissítés hello eszköz szoftverének verziójával. Toobe verzió 6.3.9600.17312 továbbra is. Úgy ellenőrizheti, hogy hello frissítés telepítve van a hello ellenőrzésével **utolsó frissített** dátum. Ha hello dátum 2/10/2015 vagy újabb verzióját, majd hello frissítés telepítése sikeresen befejeződött.  

Azt javasoljuk, hogy keressen, és elérhető frissítések alkalmazása a StorSimple eszköz telepítése után azonnal. Kapcsolja be az automatikus frissítések toodownload is, és fontos frissítéseket telepíti a Microsoft, amint azok kiadásakor. További információkért lásd: [a StorSimple eszköz frissítése](storsimple-update-device.md).  

Tekintse át a hello kibocsátási megjegyzések hello központi telepítése előtt frissítse a StorSimple megoldásban lévő hello információt.  

> [!IMPORTANT]
> * Hello StorSimple Manager szolgáltatás és a Windows PowerShell-nem használható a StorSimple tooinstall hello. februári frissítés.   
> * Körülbelül egy óra tooinstall veszi ezt a frissítést. Azonban az összegző frissítések telepítésekor, akkor hello folyamat körülbelül 3 óra toocomplete vehet igénybe.  
> * hello StorSimple. február kiadása nem tartalmaz semmilyen frissítések toohello StorSimple virtuális eszköz. Továbbra is érvényesek lesznek minden elérhető Windows frissítések toohello virtuális eszközt, beleértve a legutóbbi biztonsági javítások, de nem jelenik meg egy változást hello virtuális eszköz verzióját.  
> 
> 

Győződjön meg arról, hogy hello alábbi előfeltételek is fennállnak előzetes tooupdating a StorSimple eszközt.  

* Győződjön meg arról, hogy mindkét eszközvezérlők futnak, a frissítések keresése előtt. Ha bármelyik vezérlő nem fut, hello vizsgálat sikertelen lesz. hello, tartományvezérlői állapota kifogástalan, amelyek tooverify lépjen túl**hardverállapot** alatt hello **karbantartási** lap. Ha az összetevő, amely **kezeléséről**, forduljon a Microsoft Support adatbázist.
* Győződjön meg arról, hogy a vezérlő 0 és 1. vezérlő rögzített IP-címek irányítható, és képes kapcsolódni toohello Internet, mivel ezek hello frissítések toohello eszköz karbantartásához használatosak. Használhatja a hello [Test-Connection parancsmag](https://technet.microsoft.com/library/hh849808.aspx) tooping egy ismert cím hello hálózatokon, például outlook.com, a tartományvezérlő hello tooverify kívül van kapcsolat toohello hálózaton kívülről.
* Győződjön meg arról, hogy elérhetők-e 80-as és 443-as port kimenő kommunikációra a StorSimple eszköz. További információkért lásd: hello [a StorSimple eszköz hálózatkezelési követelményei](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* Ha hello eszköz szoftver verziója régebbi, mint 6.3.9600.17312 (2014. októberi frissítés), tiltsa le a hello Data 2 és a Data 3 portok, ha engedélyezve van, hello frissítés elkezdése előtt. Így hello Data 2 vagy a Data 3 hello frissítés telepítésekor engedélyezett portok okozhat az eszköz vezérlő toogo helyreállítási módba. Adjon vegye figyelembe, hogy hello hálózati illesztők letiltása, az összes kapcsolódó hello kötetek a rendszer offline állapotra hello i/o hello ideje alatt hello frissítés fog működni.  

## <a name="whats-new-in-hello-february-release"></a>What's new in hello. február kiadás
A frissítés tartalmaz egy javítást alkalmaztunk hello gyári alaphelyzetbe állítása probléma történt a hello GA kiadás toohello 2014. októberi kiadás frissített eszközökön. További információkért lásd: [ebben a kiadásban javított](#issues-fixed-in-the-february-release).   

A frissítés nem tartalmaz új szolgáltatások vagy funkciók.  

## <a name="issues-fixed-in-hello-february-release"></a>Javított hello. február kiadás
hello következő táblázat ismerteti a frissítés rögzített hello probléma.  

| Nem. | Szolgáltatás | Probléma | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- |
| 1 |Gyári beállítások visszaállítása |A gyári beállításokat egy eszközön, amely eredetileg rendelkezett hello GA kiadás (verzió: 6.3.9600.17215) telepítve, de frissített toohello. októberi kiadása (6.3.9600.17312 verzió) lett tooperform meg. hello gyári meghiúsul, és hello eszköz instabillá válik. |Igen |Nem |

## <a name="known-issues-in-hello-february-release"></a>Hello. február kiadásban ismert problémák
hello a következő táblázat összefoglalja az ismert problémákról, ebben a kiadásban.

| Nem. | Szolgáltatás | Probléma | Megjegyzések/megoldás | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- | --- |
| 1 |Gyári beállítások visszaállítása |Bizonyos esetekben a gyári beállítások visszaállítása, végrehajtásakor hello StorSimple eszköz előfordulhat, hogy, és megjeleníti ezt az üzenetet: **toofactory alaphelyzetbe állítása folyamatban van a (fázis 8)**. Ez akkor fordul elő, ha a CTRL + C gombra, amíg hello parancsmag folyamatban van. |CTRL + C nem nyomja meg a gyári beállítások visszaállítása kezdeményezése után. Ha már ebben az állapotban, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 2 |Lemez kvórum |Ritka esetekben ha hello EBOD szolgáltatással egy 8600device a lemezek hello többsége megszakadt a kapcsolat nincs lemez kvórum eredményezi majd hello tárolókészlet sem kapcsolódik a hálózathoz. Offline állapotban maradnak, még akkor is, ha hello lemez újracsatlakoztatását. |Szüksége lesz tooreboot hello eszköz. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 3 |Felhőalapú pillanatfelvétel hibák |Ritka esetekben egy felhő-pillanatfelvételt hello hiba miatt meghiúsulhat **elérte a maximális biztonsági korlátot**. Ez akkor fordul elő, ha az túllépi a hello 255 online klónok ugyanarra az eszközre, a hello azonos eredeti kötet, amelyet már töröltek. | |Igen |Igen |
| 4 |Helytelen vezérlő azonosítója |A vezérlő helyettesítő végrehajtásakor vezérlő 0 lehet, hogy megjelennek, a vezérlő 1. A tartományvezérlő csere közben hello kép hello társ-csomópont betöltésekor hello vezérlő azonosítója is megjelennek kezdetben, hello társ vezérlő azonosítója. Ritka esetben ez a viselkedés előfordulhat, hogy is látható. a rendszer újraindítását követően. |Nincs felhasználói beavatkozásra szükség. Ez a helyzet magától megoldódik, amikor hello vezérlő helyettesítő befejeződése után. |Igen |Nem |
| 5 |Eszközfigyelési diagramok |A StorSimple Manager szolgáltatás hello hello eszköz figyelési diagramok nem működnek a Basic, vagy az NTLM-hitelesítés nincs engedélyezve az hello proxykiszolgálót hello eszközhöz. |Hello webproxy konfigurálása a StorSimple Manager szolgáltatásban regisztrált úgy, hogy a hitelesítés be van állítva tooNONE hello eszköz módosításához. toodo StorSimple Set-HcsWebProxy parancsmag, ez futtatási hello hello Windows PowerShell. |Igen |Igen |
| 6 |Tárfiókok |Hello tárolási toodelete hello tárolási szolgáltatásfiók használata egy nem támogatott forgatókönyvet. Ez vezet tooa helyzetet, amelyben felhasználói nem lehet adatokat beolvasni. | |Igen |Igen |
| 7 |Eszköz feladatátvétel |A kötettároló hello azonos forrás eszköz toodifferent Céleszközök nem támogatja a több feladatátvétele.    Egy egyetlen elhalt eszközt toomultiple eszközökről feladatátvételi hello kötettárolók először átadja a feladatokat eszköz hello az adatok tulajdonjoga elvesztése biztosítják. Egy feladatátvétel után ezek kötettárolók jelenik meg, vagy a klasszikus Azure portálon hello meg eltérően viselkednek. | |Igen |Nem |
| 8 |Telepítés |StorSimple Adapter SharePoint telepítéshez, során kell tooprovide egy eszköz IP-cím ahhoz, hogy hello telepítés toofinish sikeresen. | |Igen |Nem |
| 9 |Webproxy |Ha a webproxy-konfigurációja HTTPS hello meg van adva protokoll, akkor az eszköz-szolgáltatások közötti kommunikáció néven érinti, és hello eszköz nélküli módba. Támogatási csomag is tudna generálni hello folyamat során fel jelentős erőforrásokat az eszközön. |Győződjön meg arról, hogy hello webes proxy URL-címe HTTP, a megadott protokoll hello. További információt túl[az eszköz webalkalmazás-proxy konfigurálása](storsimple-configure-web-proxy.md). |Igen |Nem |
| 10 |Webproxy |Ha a konfigurált és engedélyezett a webalkalmazás-proxy regisztrált egy eszközt, akkor szüksége lesz toorestart hello aktív vezérlő az eszközön. | |Igen |Nem |
| 11 |Magas felhő késéssel és nagy i/o-munkaterhelés |Amikor a StorSimple eszköz nagyon magas felhő késések (másodperc sorrendben) és a magas i/o-munkaterhelés észlel, hello eszköz kötetek csökkent állapotba, és hello i/o "az eszköz nem áll készen" hiba miatt sikertelen lehet. |Meg kell, újraindítás hello eszközvezérlők toomanually, vagy végezzen el egy eszköz feladatátvételi toorecover ebben a helyzetben a. |Igen |Nem |

## <a name="physical-device-updates-in-hello-february-release"></a>Hello. február kiadásban fizikai eszköz frissítése
A frissítés javítások hello gyári probléma történt a hello GA kiadás toohello 2014. októberi kiadás frissített eszközökön. Nem tartalmaz semmilyen más frissítések toohello StorSimple eszközt.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-hello-february-release"></a>Soros csatlakozású SCSI (SAS) vezérlő és a belső vezérlőprogram frissítések hello. február kiadásban
Ebben a kiadásban nem tartalmaz a frissítések toohello soros csatlakozású SCSI (SAS) vezérlő vagy hello belső vezérlőprogramját. hello illesztőprogram frissítéséhez hello október, 2014 kiadásban volt.  

## <a name="virtual-device-updates-in-hello-february-release"></a>Hello. február kiadásban virtuális eszköz frissítése
Ez a kiadás nem tartalmaz virtuális eszköz hello frissítéseket. A frissítés telepítése nem módosítja a virtuális eszköz hello szoftverének verziójával.

