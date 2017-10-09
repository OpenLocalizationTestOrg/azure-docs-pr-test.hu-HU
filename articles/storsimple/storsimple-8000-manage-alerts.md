---
title: "aaaView és a StorSimple 8000 series eszköz riasztások kezelése |} Microsoft Docs"
description: "StorSimple riasztási feltételek és a súlyosság, hogyan tooconfigure riasztási értesítések és hogyan toouse hello StorSimple Device Manager szolgáltatás toomanage riasztások ismerteti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/22/2017
ms.author: alkohli
ms.openlocfilehash: fdd1a911ceff542bc6bdeae877e1f0fc381bf27c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-and-manage-storsimple-alerts"></a>Hello StorSimple Device Manager szolgáltatás tooview használja, és a StorSimple-riasztások kezelése

## <a name="overview"></a>Áttekintés

Hello **riasztások** panel az hello StorSimple Device Manager szolgáltatás lehetőséget biztosít az Ön tooreview és törölje a jelet a StorSimple eszköz – kapcsolatos riasztásokat valós idejű alapon. Ezen a panelen központilag figyelheti hello ügynökállapottal kapcsolatos hibákkal StorSimple eszközt, és hello általános Microsoft Azure StorSimple megoldáshoz.

Ez az oktatóanyag leírja a gyakori riasztási állapot, a riasztás súlyossági szintek és a hogyan tooconfigure riasztási értesítéseket. Ezenkívül azt is riasztás rövid összefoglaló táblákat, amelyek lehetővé teszik, hogy tooquickly keresse meg az adott riasztásra, és megfelelő választ.

![Riasztások lap](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="common-alert-conditions"></a>Gyakori riasztási állapot

A StorSimple eszköz riasztásokat állít elő, a válasz tooa különböző feltételeket. Az alábbiakban hello hello leggyakoribb riasztási feltételek:

* **Hardverproblémák** – ezek a riasztások adja meg a hardver állapotát hello. Azt jelzi, ha a belső vezérlőprogram frissítések szükségesek, ha egy adott hálózati csatoló problémákkal rendelkezik, vagy ha valamelyik, a meghajtók probléma van a használatukkal.
* **Kapcsolódási problémák** – ezek a riasztások fordulhat elő, ha az adatok átvitele nehézségekbe ütközik. Hello Azure storage-fiókot vagy megfelelő adatok tooand adatátvitel során fordulhatnak elő a kommunikációs problémák toolack a hello eszközök és hello StorSimple Device Manager szolgáltatás közötti kapcsolat. Kommunikációs problémák hello nagyon nehéz toofix közé tartoznak, mert nincs olyan sok ponton felmerülő hibákat is. Először mindig ellenőrizze, hogy hálózati kapcsolatot és az Internet-hozzáférés érhetők el, mielőtt továbblép a toomore speciális hibaelhárítás. A segítséget kaptak, go túl[hello Test-Connection parancsmaggal kapcsolatos problémák elhárítása](storsimple-troubleshoot-deployment.md).
* **Teljesítményproblémák** – ezek a riasztások vannak okozza, ha a rendszer nem optimális, például a nagy terhelés alatt van.

Emellett riasztásokat kapcsolódó toosecurity, vagy csak a sikertelen feladatok vehet észre.

## <a name="alert-severity-levels"></a>Riasztások súlyossági szintjei

Riasztások különböző súlyossági szinttel rendelkezik, hello gyakorolt, amely hello riasztási helyzet rendelkezik lesz, és a válasz toohello riasztások szükségességét hello. hello súlyossági szintek a következők:

* **Kritikus** – Ez a riasztás válasz tooa állapotban, amely érinti a rendszer hello sikeres teljesítményét van. A művelet akkor szükséges tooensure hello StorSimple szolgáltatás nem szakad meg.
* **Figyelmeztetés** – Ez az állapot kritikus, ha nem sikerül válhat. Meg kell hello helyzet vizsgálata hajtanak végre minden szükséges művelet tooclear hello kapcsolatos probléma.
* **Információ** – Ez a riasztás információkat tartalmaznak, amelyek nyomon követése és a rendszer kezelése hasznos lehet.

## <a name="configure-alert-settings"></a>A riasztási beállítások konfigurálása

Kiválaszthatja, hogy kívánja-e toobe minden, a StorSimple eszközök riasztási feltételek e-mailben értesítést kap. Ezenkívül azonosítsa a más riasztási értesítés címzettjeinek hello e-mail címüket megadásával **más e-mailek címzettjeinek** mezőbe pontosvesszővel elválasztva.

> [!NOTE]
> Legfeljebb 20 e-mail címek eszközönként adhat meg.

Egy eszköz számára e-mail értesítések engedélyezése után tagok hello értesítési lista minden alkalommal, amikor egy kritikus riasztás esetén e-mailt fog kapni. hello üzeneteket küld a  *storsimple-alerts-noreply@mail.windowsazure.com*  és hello riasztási feltétel ismerteti. Címzettek kattintva **Unsubscribe** tooremove maguk hello e-mail értesítési listáról.

#### <a name="tooenable-email-notification-of-alerts-for-a-device"></a>tooenable értesítő riasztások egy eszköz számára
1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást. Az eszközök hello listáról válassza ki, majd válassza ki, hogy kívánja-e tooconfigure hello eszközt.
2. Nyissa meg túl**beállítások** > **általános** hello eszközhöz.

   ![Riasztások panel](./media/storsimple-8000-manage-alerts/configure-alerts-email2.png)
   
2. A hello **általános beállítások** panelen, lépjen túl**riasztási beállítások** és hello be:
   
   1. A hello **értesítés küldése e-mailben** mezőben válassza **Igen**.
   2. A hello **szolgáltatás-rendszergazdák E-mail** mezőben válassza **Igen** toohave hello szolgáltatás-rendszergazda és az összes társrendszergazdák hello riasztási értesítéseket kapni.
   3. A hello **más e-mailek címzettjeinek** mezőbe írja be az összes többi címzett hello riasztási értesítéseket kapjanak hello e-mail címét. Írja be a neveket hello formátumban  *someone@somewhere.com* . Pontosvesszővel tooseparate hello e-mail-címek használata. Legfeljebb 20 e-mail címek eszközönként konfigurálhatja. 
      
3. Kattintson egy teszt e-mail-értesítések toosend **teszt e-mail küldése**. hello tesztértesítés továbbítja hello StorSimple Device Manager szolgáltatás jelennek-állapotüzenetek.

    ![a riasztási beállítások](./media/storsimple-8000-manage-alerts/configure-alerts-email3.png)

4. Megjelenik egy értesítés, amikor hello teszt e-mailt küld. 
   
    ![Riasztások által küldött értesítő e-mail tesztelése](./media/storsimple-8000-manage-alerts/configure-alerts-email4.png)
   
   > [!NOTE]
   > Ha hello tesztelése értesítési üzenet nem küldhető, hello StorSimple Device Manager szolgáltatás megfelelő hibaüzenet jelenik meg. Várjon néhány percet, és próbálkozzon toosend az értesítési tesztüzenet újra. 

5. Hello konfiguráció befejezése után kattintson **mentése**. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.

     ![Riasztások által küldött értesítő e-mail tesztelése](./media/storsimple-8000-manage-alerts/configure-alerts-email5.png)

## <a name="view-and-track-alerts"></a>Megtekintheti, és nyomon követéséhez értesítések

hello StorSimple Device Manager szolgáltatás összefoglaló panel tesz lehetővé az eszközök, súlyossági szint szerint a riasztások számának hello kiadványok.

![Riasztások irányítópult](./media/storsimple-8000-manage-alerts/device-summary4.png)

Hello súlyossági szint kattintva megnyílik a hello **riasztások** panelen. hello eredmények csak azonos adott súlyossági szinttel hello riasztások tartalmazzák.

Egy riasztás hello listában kattintson nyújt további részletekkel hello riasztás, többek között a következőket hello a legutolsó alkalommal hello riasztás érkezett jelentés, hello eszközön, és a hello hello riasztás előfordulásainak száma hello javasolt művelet tooresolve hello riasztás. Ha a hardver-riasztások, azt is hello hardverösszetevő azonosítja.

![Riasztási hardver – példa](./media/storsimple-8000-manage-alerts/configure-alerts-email14.png)

Ha toosend hello információk tooMicrosoft támogatási kell hello riasztás részleteinek tooa szövegfájl másolhatja. Hello javaslat követni és hello riasztási feltétel helyszíni feloldva, után törölje hello riasztás hello eszközről hello riasztás kiválasztása a hello **riasztások** panel megnyitásához, és kattintson a **törlése**. tooclear több riasztást, jelölje be minden riasztást, kattintson a hello kivételével minden oszlop **riasztás** oszlop, és kattintson **egyértelmű** kiválasztása után az összes hello riasztások toobe nincs bejelölve. Vegye figyelembe, hogy egyes riasztások automatikusan törlődnek hello probléma megoldása után, vagy ha hello rendszer hello riasztás frissíti új információkkal.

Amikor rákattint **egyértelmű**, hello lehetőség tooprovide megjegyzéseket hello riasztást fog, és hello lépéseket, hogy, tooresolve hello probléma. Egyes események hello rendszer törlődik, ha egy másik esemény akkor váltódik ki, új információkkal. Ebben az esetben látni fogja a következő üzenet hello.

![Törölje a riasztási üzenet](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>A rendezés és tekintse át a riasztások

Szükség lehet arra riasztások hatékonyabb toorun jelentéseket, hogy tekintse át, és törölje őket a csoportokban. Emellett hello **riasztások** panel too250 figyelmeztetéseket tudja megjeleníteni. Túllépte a riasztások számát is, ha nem az összes riasztás hello alapértelmezett nézetben jelenik meg. A következő mezők toocustomize mely riasztások megjelenítésének hello kombinálva:

* **Állapot** – jelenítheti meg vagy **aktív** vagy **nincs bejelölve** riasztásokat. Aktív továbbra is folyamatban figyelmeztetéseket a rendszeren, amíg üres riasztások vagy manuálisan törölték egy rendszergazda, vagy programozott módon törölve, mert hello rendszer hello riasztási feltétel frissülnek az új adatokkal.
* **Súlyossági** – az összes súlyossági szint (kritikus, figyelmeztetés, tájékoztatás), vagy csak egy bizonyos súlyossági, például csak a kritikus riasztások riasztások jelenítheti meg.
* **Forrás** – forrásokból származó riasztások megjelenítése, vagy hello riasztások toothose hello szolgáltatás vagy egy vagy hello eszközeiről érkező korlátozható.
* **Időtartománynak** – hello megadásával **a** és **való** dátumok és időbélyegeket, vessen egy pillantást riasztások hello érdekli időtartamnak során.

![Riasztások listája](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="alerts-quick-reference"></a>Riasztások rövid összefoglalása

a következő táblák hello megjelenő néhány hello Microsoft Azure StorSimple riasztásokat, amelyek akkor fordulhatnak, valamint további információkra és ajánlásokra ahol az rendelkezésre áll. A StorSimple eszköz riasztások egyikébe hello a következő kategóriák:

* [Felhő csatlakozási riasztások](#cloud-connectivity-alerts)
* [Fürt riasztások](#cluster-alerts)
* [Vész-helyreállítási riasztások](#disaster-recovery-alerts)
* [Hardver riasztások](#hardware-alerts)
* [Figyelmeztetések](#job-failure-alerts)
* [Helyileg rögzített kötet riasztás](#locally-pinned-volume-alerts)
* [Hálózatkezelés riasztások](#networking-alerts)
* [Teljesítményével kapcsolatos riasztások](#performance-alerts)
* [Biztonsági riasztások](#security-alerts)
* [Támogatási csomag riasztások](#support-package-alerts)

### <a name="cloud-connectivity-alerts"></a>Felhő csatlakozási riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Kapcsolat túl <*felhő hitelesítő adat neve*> nem hozható létre. |Nem lehet kapcsolódni a toohello tárfiók. |Úgy tűnik, előfordulhat, hogy az eszköz csatlakozási probléma. Futtassa a hello `Test-HcsmConnection` parancsmag Windows PowerShell felületet a StorSimple hello meg az eszköz tooidentify és hello probléma. Ha hello beállítások helyesek, hello probléma lehet, amelynek hello riasztás kiadását okozó hello tárfiók hello hitelesítő adatokkal. Ebben az esetben használja a hello `Test-HcsStorageAccountCredential` parancsmag toodetermine, ha problémák megoldása.<ul><li>Ellenőrizze a hálózati beállításokat.</li><li>A tárfiók hitelesítő adatainak ellenőrzése.</li></ul> |
| Nem érkezett szívverés az eszközről a hello utolsó <*szám*> perc. |Nem lehet kapcsolódni a toodevice. |Úgy tűnik, az eszköz csatlakozási probléma van. Használja a hello `Test-HcsmConnection` parancsmag Windows PowerShell felületet a StorSimple meg az eszköz tooidentify hello és hello probléma, vagy forduljon a rendszergazdához. |

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>StorSimple működés, ha a felhő-kapcsolat sikertelen

Mi történik, ha nem sikerül felhő kapcsolat az éles környezetben futó StorSimple eszköz?

Ha felhő kapcsolat nem sikerül, a StorSimple éles eszköze, majd attól függően, hogy az eszköz állapotának hello hello következő akkor fordulhat elő:

* **A helyi adatok hello eszközén**: egy kis ideig lesz nem működőképes, és olvasási továbbra is toobe és kiszolgálása között. Függőben lévő IOs hello száma növekszik, és meghaladja a korlátot, azonban hello olvasások elindítása volt toofail.

    Attól függően, hogy az eszközön adatok mennyisége hello hello írási műveletek is továbbra is a hello toooccur első néhány órára hello megszakítás után a hello felhő kapcsolat. hello írások majd lelassul, és a toofail végül el, ha a több órán keresztül hello felhő kapcsolat megszakad. (Nincs ideiglenes tárolási toohello felhő leküldött toobe adatok hello az eszközön. Ez a terület ki van ürítve, amikor hello adatokat küldi el. Ha a kapcsolat nem sikerül, a tárolási terület adatokat nem lehet leküldeni toohello felhő, és IO meghiúsul.)
* **Hello adatok hello felhőben**: A legtöbb felhő kapcsolódási hibákat, hibát ad vissza. Miután hello kapcsolat helyreáll, a hello IOs toobring hello kötet online rendelkező hello felhasználói nélkül folytatja a munkát. Ritka esetekben a felhasználói beavatkozás szükséges toobring hátsó hello kötet online az Azure-portálon hello lehet.
* **A folyamatban lévő felhőalapú pillanatfelvételek**: hello műveletet a rendszer ismét megkísérli néhány alkalommal belül 4-5 óra és hello kapcsolat nem állítja vissza, ha hello felhőalapú pillanatfelvételek sikertelen lesz.

### <a name="cluster-alerts"></a>Fürt riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Az eszközt nem sikerült keresztül túl <*eszköznév*>. |Eszköz karbantartási módban van. |Eszköz keresztül tooentering vagy a meglévő karbantartási módba miatt sikertelen volt. Ez nem jelent problémát, és nincs szükség beavatkozásra. Után ez a riasztás rendelkezik nyugtázott, törölje a jelet hello riasztások lapján. |
| Az eszközt nem sikerült keresztül túl <*eszköznév*>. |Eszköz belső vezérlőprogram vagy a szoftver csak frissült. |Hiba történt egy fürtön belüli feladatátvétel tooan frissítése miatt. Ez nem jelent problémát, és nincs szükség beavatkozásra. Után ez a riasztás rendelkezik nyugtázott, törölje a jelet hello riasztások lapján. |
| Az eszközt nem sikerült keresztül túl <*eszköznév*>. |A tartományvezérlő le volt leállt vagy újraindult. |Eszköz keresztül nem sikerült, mert aktív vezérlő hello leállt vagy egy rendszergazda újra. Nincs szükség beavatkozásra. Után ez a riasztás rendelkezik nyugtázott, törölje a jelet hello riasztások lapján. |
| Az eszközt nem sikerült keresztül túl <*eszköznév*>. |Tervezett feladatátvétel. |Győződjön meg arról, hogy ez volt a tervezett feladatátvételt. Miután elvégezte a megfelelő műveletet, törölje ezt a riasztást hello riasztások lapján. |
| Az eszközt nem sikerült keresztül túl <*eszköznév*>. |Nem tervezett feladatátvételt. |StorSimple-t a nem tervezett feladatátvételek tooautomatically helyreállítása. Ha ezek a riasztások nagy számú látja, forduljon a Microsoft Support. |
| Az eszközt nem sikerült keresztül túl <*eszköznév*>. |Egyéb/ismeretlen ok. |Ha ezek a riasztások nagy számú látja, forduljon a Microsoft Support. Után hello probléma megoldódott, törölje a riasztás hello riasztások lapján. |
| Egy kritikus eszközök szolgáltatás azt jelenti állapota sikertelen. |DataPath szolgáltatás hibája. |Segítségért forduljon a Microsoft Support. |
| Virtuális hálózati adapter IP-címét <*DATA #*> Jelentések állapota sikertelen. |Egyéb/ismeretlen ok. |Egyes esetekben ideiglenes okok miatt jöhet létre ezeket a riasztásokat. Ha ez helyzet hello, majd ezt a riasztást automatikusan törölve lesz egy kis idő múlva. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. |
| Virtuális hálózati adapter IP-címét <*DATA #*> Jelentések állapota sikertelen. |A kapcsolat nevét: <*DATA #*> IP-cím <IP address> nem hozható online állapotba, mert egy ismétlődő IP-cím észlelhető hello hálózati. |Győződjön meg arról, hogy hello ismétlődő IP-cím van-e el hello hálózatról, vagy konfigurálja újra a hello felület egy másik IP-címmel. |

### <a name="disaster-recovery-alerts"></a>Vész-helyreállítási riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Helyreállítási művelet nem sikerült visszaállítani az összes hello beállítást ehhez a szolgáltatáshoz. Eszköz konfigurációs adatokat az egyes eszközök esetében inkonzisztens állapotban van. |Adatok inkonzisztencia észlelhető katasztrófa utáni helyreállítás után. |A titkosított adatok hello szolgáltatásban nincs szinkronizálva, amely hello eszközön. Hello eszköz hitelesítéséhez <*eszköznév*> StorSimple Device Manager toostart hello szinkronizálási folyamat során. Használjon hello Windows PowerShell felületet a StorSimple toorun hello `Restore-HcsmEncryptedServiceData` eszközön <*eszköznév*> parancsmag hello régi jelszó megadása egy bemeneti toothis, a parancsmag toorestore hello biztonsági profil. Futtassa a hello `Invoke-HcsmServiceDataEncryptionKeyChange` parancsmag tooupdate hello szolgáltatásadat-titkosítási kulcs. Miután elvégezte a megfelelő műveletet, törölje ezt a riasztást hello riasztások lapján. |

### <a name="hardware-alerts"></a>Hardver riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Hardverösszetevő <*összetevő-azonosító*> állapotának jelentések <*állapot*>. | |Egyes esetekben ideiglenes okok miatt jöhet létre ezeket a riasztásokat. Ha igen, ez a riasztás automatikusan törölve lesz egy kis idő múlva. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. |
| Passzív vezérlő hibásan működik. |hello passzív (másodlagos) vezérlő nem működik. |Az eszköz működik, de a vezérlőket hibásan működik. Indítsa újra, hogy az adott. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. |

### <a name="job-failure-alerts"></a>Figyelmeztetések

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| A biztonsági mentési <*kötet csoport forrásazonosító*> nem sikerült. |Biztonsági mentési feladat sikertelen volt. |Kapcsolódási problémák megakadályozzák, hogy hello biztonsági mentési művelet sikeres befejezését. Nincsenek kapcsolódási problémák, ha lehetséges, hogy túllépte hello biztonsági másolatok maximális számát. Minden biztonsági másolatának, már nem szükséges, és próbálja megismételni a műveletet hello törlése. Miután elvégezte a megfelelő műveletet, törölje ezt a riasztást hello riasztások lapján. |
| A klón <*adatforrás biztonsági mentési elem azonosítója*> túl <*cél kötet sorozatszámokat*> nem sikerült. |Klónozás feladat sikertelen volt. |Biztonsági mentési hello frissítési hello biztonságimásolat-lista tooverify még mindig érvényes. Érvénytelen hello biztonsági másolat, akkor lehetséges, hogy a felhő kapcsolódási problémák akadályozzák a hello klónozási művelet sikeres befejezését. Ha nincsenek kapcsolódási problémák, előfordulhat, hogy elérte hello tárolási kapacitása. Minden biztonsági másolatának, már nem szükséges, és próbálja megismételni a műveletet hello törlése. Miután elvégezte a megfelelő műveletet tooresolve hello problémát, törölje ezt a riasztást hello riasztások lapján. |
| Állítsa vissza a <*adatforrás biztonsági mentési elem azonosítója*> nem sikerült. |Állítsa vissza a feladat sikertelen volt. |Biztonsági mentési hello frissítési hello biztonságimásolat-lista tooverify még mindig érvényes. Érvénytelen hello biztonsági másolat, akkor lehetséges, hogy a felhő kapcsolódási problémák akadályozzák a hello visszaállítási művelet sikeres befejezését. Ha nincsenek kapcsolódási problémák, előfordulhat, hogy elérte hello tárolási kapacitása. Minden biztonsági másolatának, már nem szükséges, és próbálja megismételni a műveletet hello törlése. Miután elvégezte a megfelelő műveletet tooresolve hello problémát, törölje ezt a riasztást hello riasztások lapján. |

### <a name="locally-pinned-volume-alerts"></a>Helyileg rögzített kötet riasztás

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Helyi kötet létrehozása <*kötetneve*> nem sikerült. |hello kötet létrehozására irányuló feladat sikertelen volt. <*Hiba üzenet megfelelő toohello sikertelen volt, hibakód:*>. |Kapcsolódási problémák megakadályozzák, hogy hello terület létrehozási művelet sikeres befejezését. Helyileg rögzített kötet kiosztása és hello terület létrehozásának folyamata magában foglalja a rétegzett kötetek toohello felhő kiömlést. Ha nincsenek kapcsolódási problémák, előfordulhat, hogy helyi terület hello hello eszközön kimerített. Határozza meg, ha a hely létezik hello eszköz mielőtt megpróbálná megismételni a műveletet. |
| Helyi kötet bővítése <*kötetneve*> nem sikerült. |hello kötet módosítása feladat nem sikerült miatt túl <*hiba üzenet megfelelő toohello sikertelen volt, hibakód:*>. |Kapcsolódási problémák megakadályozzák, hogy hello kötet bővítése művelet sikeres befejezését. Helyileg rögzített kötet kiosztása és hello hello meglévő terület kiterjesztése magában foglalja kiömlést rétegzett kötetek toohello felhő. Ha nincsenek kapcsolódási problémák, előfordulhat, hogy helyi terület hello hello eszközön kimerített. Határozza meg, ha a hely létezik hello eszköz mielőtt megpróbálná megismételni a műveletet. |
| Kötet konvertálása <*kötetneve*> nem sikerült. |hello kötet átalakítási feladat tooconvert hello kötettípus a helyileg rögzített tootiered nem sikerült. |Helyileg rögzített típus tootiered hello kötet konverzió sikertelen volt. Győződjön meg arról, hogy nincsenek-e hello művelet sikeres befejezését kapcsolódási problémák. A kapcsolat hibaelhárítási problémák nyissa meg túl[hello Test-HcsmConnection parancsmag elhárítása](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>hello eredeti helyileg rögzített kötet most jelöléssel lett a rétegzett kötetek óta hello helyileg rögzített kötet hello adatok egy részét rendelkezik kiömlött toohello felhő hello átalakítás során. hello eredő rétegzett kötet még mindig van elfoglaló helyi terület hello az eszközön, amely nem nyerhető jövőbeli helyi köteteken.<br>Hárítson el minden kapcsolódási problémát, törölje a hello riasztást, és alakítsa át a kötet hátsó toohello eredeti helyileg rögzített kötet típus tooensure összes hello adatok helyben újra elérhetővé. |
| Kötet konvertálása <*kötetneve*> nem sikerült. |nem sikerült hello átalakítási feladat tooconvert hello kötet kötettípus a rétegzett toolocally rögzítve. |Rétegzett toolocally rögzítve típusból hello kötet konvertálása nem sikerült. Győződjön meg arról, hogy nincsenek-e hello művelet sikeres befejezését kapcsolódási problémák. A kapcsolat hibaelhárítási problémák nyissa meg túl[hello Test-HcsmConnection parancsmag elhárítása](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>hello eredeti rétegzett kötet most egy helyileg rögzített kötet jelölésű hello átalakítási folyamat részeként folyamatosan toohave adatokról készült hello felhőbe, miközben hello kiosztása hello az eszközön a kötet hely már nem szabadítható fel a jövőbeni helyi kötetek.<br>Hárítson el minden kapcsolódási problémák, egyértelmű hello riasztás szabadítható fel a kötet hátsó toohello eredeti rétegzett kötet tooensure helyi típustérre kiosztása hello eszközön convert. |
| Helyi pillanatképeinek helyi lemezterület-felhasználást hamarosan <*kötet csoport neve*> |Helyi pillanatképeinek hello biztonsági mentési házirend előfordulhat, hogy nincs elegendő szabad terület, és hamarosan kell érvénytelenített tooavoid állomás írási hibák. |A biztonsági mentési házirend csoporthoz tartozó hello kötetek magas adatforgalommal mellett gyakori helyi pillanatképeket hello eszköz toobe fel gyorsan a okozó helyi terület. Törölje a meglévő helyi pillanatképeket, amely már nem szükséges. Is ez a biztonsági mentési házirend tootake gyakori helyi pillanatképeket kisebb a helyi pillanatfelvétel ütemezésének frissítése, és győződjön meg arról, hogy felhőalapú pillanatfelvételek rendszeresen készül-e. Ha ezeket a műveleteket a rendszer nem hajtja végre, előfordulhat, hogy hamarosan felhasználhatók, ezek a pillanatképek helyi terület és hello rendszer automatikusan törölje azokat, az állomás írások folytatja sikeresen feldolgozott toobe tooensure. |
| A pillanatképek helyi <*kötet csoportnév*> érvénytelenítve lett. |a pillanatképek helyi hello <*kötet csoportnév*> érvénytelenítve és majd törölve, mert azok volt meghaladó hello helyi terület hello eszközön. |Ez a jövőbeni, hello nem ismétlődnek tooensure tekintse át a hello helyi pillanatkép ütemezése a biztonsági mentési házirend, és törölje az összes helyi pillanatképet, amely már nem szükséges. A biztonsági mentési házirend csoporthoz tartozó hello kötetek magas adatforgalommal mellett gyakori helyi pillanatképeket hello eszköz toobe fel gyorsan a helyi terület okozhatja. |
| Állítsa vissza a <*adatforrás biztonsági mentési elem azonosítója*> nem sikerült. |hello visszaállítási feladat sikerült. |Ha helyileg rögzített, illetve a biztonsági mentési házirend, biztonsági mentési hello frissítési hello biztonságimásolat-lista tooverify helyileg rögzített és a rétegzett kötetek vegyesen még mindig érvényes. Érvénytelen hello biztonsági másolat, akkor lehetséges, hogy a felhő kapcsolódási problémák akadályozzák a hello visszaállítási művelet sikeres befejezését. hello helyileg rögzített kötetek helyreállított alatt a pillanatkép-csoport része nem rendelkeznek az összes letöltött toohello eszközüket, és ha rétegzett és helyileg rögzített kötetek vegyesen a pillanatkép-csoportban található, nem fogja szinkronban vannak egymással. toosuccessfully hello visszaállítási művelet végrehajtása, készítsen hello köteteiről a csoport offline állapotba hello állomáson, majd próbálja megismételni hello visszaállítási műveletet. Vegye figyelembe, hogy a módosítások toohello kötetadatokról hello során végrehajtott visszaállítási folyamat el fog veszni. |

### <a name="networking-alerts"></a>Hálózatkezelés riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Nem lehetett elindítani a StorSimple szolgáltatás(oka)t. |DataPath hiba |Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. |
| Ismétlődő IP-cím észlelhető a következőnél: "Data0". | |hello rendszer IP-cím "10.0.0.1" ütközést észlelt. hálózati erőforrás "Data0" Hello hello eszközön  *<device1>*  offline állapotban. Győződjön meg arról, hogy az IP-címet nem használja ezt a hálózatot minden más entitás. tootroubleshoot hálózati problémák, nyissa meg túl[hello Get-NetAdapter parancsmaggal kapcsolatos problémák elhárítása](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). A probléma megoldásához forduljon a rendszergazdához. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. |
| IPv4-(vagy IPv6) címmel "Data0" offline állapotban. | |hello hálózati erőforrás "Data0" IP-cím "10.0.0.1." és előtag hossza "22" hello eszközön  *<device1>*  offline állapotban. Győződjön meg arról, hogy az illesztő csatlakozik hello kapcsoló portok toowhich működési. tootroubleshoot hálózati problémák, nyissa meg túl[hello Get-NetAdapter parancsmaggal kapcsolatos problémák elhárítása](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
| Nem sikerült csatlakozni a toohello hitelesítési szolgáltatás. |DataPath hiba |hello URLthat használt tooauthenticate nem érhető el. Győződjön meg arról, hogy a tűzfalszabályok tartalmazza-e a megadott hello StorSimple eszköz hello URL-mintával. További információ az URL-mintával az Azure portálon lépjen a toohttps://aka.ms/ss-8000-network-reqs. Azure Government felhő használata esetén nyissa meg a https://aka.ms/ss8000-gov-network-reqs toohello URL-mintával.|

### <a name="performance-alerts"></a>Teljesítményével kapcsolatos riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| hello eszköz terhelés túllépte <*küszöbérték*>. |Lassabb, mint a várt válaszidejét. |Az eszköz a bemeneti/kimeneti túlterheltség kihasználtsági jelentés. Ez az eszköz toonot munkahelyi legyen, valamint okozhat. Tekintse át a hello munkaterhelések csatolt toohello eszközt, és határozza meg, ha vannak ilyenek, amelyek sikerült áthelyezni a tooanother eszköz, vagy már nincs szükség.| Nem lehetett elindítani a StorSimple szolgáltatás(oka)t. |DataPath hiba |Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. |és hello aktuális állapotát, nyissa meg túl[hello StorSimple Device Manager szolgáltatás toomonitor az eszköz használata](storsimple-monitor-device.md) |

### <a name="security-alerts"></a>Biztonsági riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Microsoft Support munkamenet már megkezdődött. |Külső támogatási munkamenet érhető el. |Ellenőrizze, hogy a hozzáférés engedélyezése. Miután elvégezte a megfelelő műveletet, törölje ezt a riasztást hello riasztások lapján. |
| A jelszó <*elem*> múlva le fog járni <*mennyi ideig*>. |Jelszó érvényessége hamarosan eléri. |Változtassa meg jelszavát még a lejárta előtt. |
| A hiányzó biztonsági konfigurációadatait <*Elemazonosító*>. | |a kötettároló társított kötetek hello nem lehet használt tooreplicate a StorSimple-konfigurációt. tooensure, hogy biztonságosan tárolja az adatokat, azt javasoljuk, hogy törölje a hello kötettároló és hello kötettároló társított köteteket. Miután elvégezte a megfelelő műveletet, törölje ezt a riasztást hello riasztások lapján. |
| <*szám*> a sikertelen bejelentkezési kísérletek <*Elemazonosító*>. |Több sikertelen bejelentkezési kísérletek. |Az eszköz lehet támadás alatt áll, vagy a hitelesített felhasználó megpróbál tooconnect érvénytelen jelszóval.<ul><li>Lépjen kapcsolatba a jogosult felhasználók, és győződjön meg arról, hogy van-e ezeket a kísérleteket megbízható forrásból. Ha folytatja a toosee nagy számú sikertelen bejelentkezési kísérlet, fontolja meg a Távfelügyelet letiltása, és a hálózati rendszergazda. Miután elvégezte a megfelelő műveletet, törölje ezt a riasztást hello riasztások lapján.</li><li>Ellenőrizze, hogy a pillanatkép-kezelő példányok hello helyes jelszót vannak konfigurálva. Miután elvégezte a megfelelő műveletet, törölje ezt a riasztást hello riasztások lapján.</li></ul>További információ: túl[lejárt eszköz jelszó módosítása](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password). |
| Egy vagy több hiba történt a szolgáltatásadat-titkosítási kulcs hello módosítása közben. | |Szolgáltatásadat-titkosítási kulcs hello módosítása során hibák történtek. Miután hello hibaállapotok foglalkoztak, futtassa a hello `Invoke-HcsmServiceDataEncryptionKeyChange` hello Windows PowerShell-felületet a StorSimple az eszköz tooupdate hello szolgáltatáson parancsmagjával. Ha nem szűnik meg a hiba, forduljon a Microsoft támogatási szolgálatához. Hello probléma megoldása után törölje ezt a riasztást hello riasztások lapján. |

### <a name="support-package-alerts"></a>Támogatási csomag riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Nem sikerült létrehozni a támogatási csomag. |StorSimple hello csomagot nem lehetett létrehozni. |Próbálja megismételni a műveletet. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. Hello probléma megoldása után törölje ezt a riasztást hello riasztások lapján. |

## <a name="next-steps"></a>Következő lépések

További információ [StorSimple hibák és működési eszköz hibaelhárítása](storsimple-troubleshoot-operational-device.md).

