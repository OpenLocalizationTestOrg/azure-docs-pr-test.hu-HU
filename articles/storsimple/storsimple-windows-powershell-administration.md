---
title: "a StorSimple eszközök felügyeletére aaaPowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Windows PowerShell a StorSimple toomanage a StorSimple eszközt."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0ff3bb0d-897a-4676-bdcb-402c2628dac5
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: alkohli@microsoft.com
ms.openlocfilehash: 1adcbb8bb89e3e3b4f328aac198690476c2a78cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-powershell-for-storsimple-tooadminister-your-device"></a>A Windows PowerShell használata a StorSimple tooadminister az eszköz
## <a name="overview"></a>Áttekintés
A Windows PowerShell-lel egy parancssori felületet biztosít, amelyeket felhasználhat toomanage a Microsoft Azure StorSimple eszközt. Mivel hello nevet javasol, a Windows PowerShell-alapú, parancssori felület egy korlátozott futási térrel részét képező is. Hello felhasználói hello parancssorból hello szempontjából egy korlátozott futási térrel jelenik meg egy Windows PowerShell korlátozott verziója. Fenntartva néhány, a Windows PowerShell hello alapvető képességeit, ez az interfész rendelkezik kezelése a Microsoft Azure StorSimple eszköz körétől további dedikált parancsmagokat. 

Ez a cikk ismerteti a StorSimple-szolgáltatások, hogyan csatlakoztathatja a toothis felületén, beleértve a Windows PowerShell hello, és hivatkozások toostep lépésre eljárások vagy hajthat végre a kapcsolat használatával munkafolyamatok tartalmaz. hello munkafolyamatok például hogyan tooregister az eszköz hello hálózati adapter konfigurálása az eszközön, a frissítések telepítése, amely szükséges hello eszköz toobe karbantartási üzemmódban, majd hello az eszköz állapotát, és bármely problémák léphetnek fel.

A cikk elolvasása után fogja tudni:

* Csatlakozás tooyour StorSimple eszközt a StorSimple Windows PowerShell használatával.
* A StorSimple eszközt a StorSimple Windows PowerShell segítségével felügyelheti.
* Segítség a Windows PowerShellben a StorSimple.

> [!NOTE]
> * A Windows PowerShell parancsmagokkal StorSimple toomanage lehetővé teszik a StorSimple eszköz soros konzolon vagy a Windows PowerShell távoli eljáráshívás keresztül távolról. További információ az egyes hello egy parancsmag használható ezen a felületen lépjen túl[parancsmag-referencia a Windows PowerShell-lel](https://technet.microsoft.com/library/dn688168.aspx).
> * hello Azure PowerShell StorSimple parancsmagok olyan parancsmagokat, amelyek lehetővé teszik a StorSimple szolgáltatásiszint-tooautomate és áttelepítési feladatok hello parancssorból egy másik gyűjteményt. A StorSimple hello Azure PowerShell-parancsmagokkal kapcsolatos további információkért nyissa meg a toohello [Azure StorSimple parancsmag-referencia](/powershell/module/azure/?view=azuresmps-3.7.0).
> 
> 

A Windows PowerShell hello hozzáférhetnek a StorSimple hello a következő módszerek egyikével:

* [Csatlakozás tooStorSimple eszköz soros konzoljához](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
* [Távoli csatlakozás tooStorSimple Windows PowerShell használatával](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)

## <a name="connect-toowindows-powershell-for-storsimple-via-hello-device-serial-console"></a>Csatlakozás a StorSimple tooWindows PowerShell hello eszköz soros konzoljához
Is [töltse le a PuTTY](http://www.putty.org/) vagy hasonló terminálemulációs szoftver tooconnect tooWindows PowerShell-lel. A PuTTY tooconfigure kell kifejezetten tooaccess hello Microsoft Azure StorSimple eszközt. hello következő témakörök tartalmaznak részletes lépéseket a PuTTy tooconfigure csatlakoztassa toohello eszközt. Különböző menüpontok hello soros konzolon is ismerteti.

### <a name="putty-settings"></a>PuTTY-beállítások
Győződjön meg arról, hogy használja-e a következő PuTTY beállítások tooconnect toohello Windows PowerShell-felületet hello soros konzolból hello.

#### <a name="tooconfigure-putty"></a>a PuTTY tooconfigure
1. A hello PuTTY **újrakonfigurálás** párbeszédpanel hello **kategória** ablaktáblán válassza előbb **billentyűzet**.
2. Győződjön meg arról, hogy a következő beállítások hello kiválasztott (amelyek hello alapértelmezett beállításait egy új munkamenet indítása). 
   
   | Billentyűzet elem | Válassza ezt: |
   | --- | --- |
   | BACKSPACE kulcs |Ellenőrzés-? (127) |
   | Otthoni és a záró kulcsok |Standard |
   | Funkcióbillentyűk és billentyűzet |ESC [n ~ |
   | A kurzor kulcsok kezdeti állapot |Normál |
   | Számbillentyűzeten kezdeti állapota |Normál |
   | Egyéb billentyűzet szolgáltatások engedélyezése |Ellenőrzés-Alt AltGr eltér |
   
    ![Támogatott Putty-beállítások](./media/storsimple-windows-powershell-administration/IC740877.png)
3. Kattintson az **Alkalmaz** gombra.
4. A hello **kategória** ablaktáblán válassza előbb **fordítási**.
5. A hello **távoli karakterkészlet** listáján jelölje ki **UTF-8**.
6. A **sor rajz karakterek kezelését**, jelölje be **használata Unicode sor rajz kódpontokat**. hello alábbi ábrán hello megfelelő PuTTY beállításokat.
   
    ![UTF Putty-beállítások](./media/storsimple-windows-powershell-administration/IC740878.png)
7. Kattintson az **Alkalmaz** gombra.

Most már használhatja PuTTY tooconnect toohello eszköz soros konzoljához hello lépések végrehajtásával.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="about-hello-serial-console"></a>Hello soros konzol bemutatása
Hello Windows PowerShell felületet a StorSimple eszköz hello soros konzolon keresztül fér hozzá, amikor egy szalagcím üzenet akkor jelenik meg, menüpontok követ. 

Transzparens üdvözlőüzenetére a StorSimple eszköz alapvető információkat, például a hello modell, a nevét, a telepített szoftverek verzióját és a elérésére hello tartományvezérlő állapotának tartalmazza. a következő kép hello szalagcím üzenet példáját mutatja be.

![Soros szalagcím üzenet](./media/storsimple-windows-powershell-administration/IC741098.png)

> [!IMPORTANT]
> Hello szalagcím üzenet tooidentify használhatja akár hello tartományvezérlő van aktív vagy passzív toois csatlakoztatva.
> 
> 

a következő kép bemutatja hello hello hello soros konzol menüben elérhető különböző futási térben beállításokat.

![Regisztrálja az eszközt a 2. régiója](./media/storsimple-windows-powershell-administration/IC740906.png)

Hello a következő beállítások közül választhat:

1. **Jelentkezzen be a teljes körű hozzáférési** lehetővé teszi az olyan (a hello megfelelő hitelesítő adatok) tooconnect toohello **SSAdminConsole** futási térben hello helyi tartományvezérlőn. (hello helyi tartományvezérlő a hello vezérlő jelenleg hello a StorSimple eszköz soros konzolon keresztül elért.) Ezt a lehetőséget is tooallow Microsoft Support tooaccess korlátlan futási teret (a támogatási munkamenet) tootroubleshoot lehetséges probléma merül fel. Miután az 1. lehetőség toolog, engedélyezze hello Microsoft Support engineer tooaccess korlátlan futási térben egy adott parancsmag futtatásával. Részletekért lásd a túl[támogatási munkamenet indításához](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).
2. **Jelentkezzen be a teljes hozzáféréssel rendelkező toopeer vezérlő** Ez a beállítás akkor az hello ugyanaz, mint 1. lehetőség –, azzal a különbséggel, hogy akkor csatlakozhatnak (hello megfelelő hitelesítő adatok) toohello **SSAdminConsole** futási térben hello társ tartományvezérlőn. Mivel hello StorSimple eszköz aktív-passzív konfigurációban két vezérlő egy magas rendelkezésre állású eszköz, társ hivatkozik toohello hello eszköz hello soros konzolon keresztül elérő egyéb tartományvezérlő).
   Hasonló toooption 1, ez a beállítás akkor is használt tooallow Microsoft Support tooaccess korlátlan futási térben egy partner-tartományvezérlő.
3. **Korlátozott hozzáférésű csatlakozás** ezt a beállítást a használt tooaccess Windows PowerShell-felületet korlátozott módban. Nem kéri a hozzáférési hitelesítő adatokat. Ez a beállítás összekapcsolja tooa további korlátozott futási térrel képest toooptions 1 és 2.  1. lehetőség elérhető hello feladatokat, amelyek **nem* végezhető el a futási térben van:
   
   * Toohello gyári beállítások visszaállítása
   * Hello jelszó módosítása
   * Engedélyezheti vagy tilthatja le a támogatási hozzáférés
   * Frissítések alkalmazása
   * Gyorsjavítások telepítése 

    > [!NOTE]
    > Ez a lehetőség ajánlott hello Ha elfelejtette a hello eszköz rendszergazdai jelszava és 1 vagy 2 keresztül nem tud kapcsolódni.

4. **Nyelv váltása** lehetővé teszi az olyan toochange hello megjelenítési nyelvét a hello Windows PowerShell-felületet. támogatott nyelvek hello angol, japán, orosz, francia, dél koreai, német, olasz, német, kínai és brazíliai portugál.

## <a name="connect-remotely-toostorsimple-using-windows-powershell-for-storsimple"></a>Távoli csatlakozás a Windows PowerShell használatával a StorSimple tooStorSimple
A Windows PowerShell távoli eljáráshívás tooconnect tooyour StorSimple eszközt is használhatja. Ezzel a módszerrel csatlakoztatásakor menü nem jelenik meg. (Megjelenik a menü csak akkor, ha a soros konzol hello hello eszköz tooconnect használ. Távoli kapcsolódás viszi "1. lehetőség – teljes körű hozzáférési" hello soros konzolon közvetlenül toohello megfelelője.) A Windows PowerShell-távelérést csatlakozás tooa adott futási térből. Hello megjelenítési nyelv is megadható. 

hello megjelenítési nyelv az hello segítségével beállított hello nyelvet független **nyelv váltása** hello soros konzol menüben lehetőség. Távoli PowerShell automatikusan felveszi hello területi hello eszköz csatlakozik, ha nincs megadva.

> [!NOTE]
> Ha a Microsoft Azure virtuális gazdagépek és a StorSimple virtuális eszközökön dolgozik, használhatja a Windows PowerShell távoli eljáráshívás és hello virtuális állomás tooconnect toohello virtuális eszköz. Ha állította be a hello gazdagép mely toosave adatokat hello Windows PowerShell-munkamenetben a helyét, vegye figyelembe, hogy mindenki egyszerű tartalmaz hello csak hitelesített felhasználók kell lennie. Ezért ha meg van adva hello megosztási tooallow hozzáférés mindenki számára, és hitelesítő adatainak megadása nélkül csatlakoztatni, hello nem hitelesített névtelen egyszerű fogja használni, és hibaüzenet jelenik meg. a probléma hello megosztása kell hello Vendég fiók engedélyezése, és adja meg vagy hello Vendég fiók teljes hozzáféréssel toohello megosztást fogadó toofix hello Windows PowerShell-parancsmaggal együtt érvényes hitelesítő adatokat kell megadnia.
> 
> 

A Windows PowerShell távoli eljáráshívás keresztül HTTP vagy HTTPS tooconnect is használhatja. A következő oktatóanyagok hello használata hello utasításait:

* [Csatlakozás távoli a HTTP-n keresztül](storsimple-remote-connect.md#connect-through-http)
* [Távolról a HTTPS használatával](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Kapcsolat biztonsági megfontolások
Meghatározásakor hogyan tooconnect tooWindows PowerShell-lel, hello következőket vegye figyelembe:

* Csatlakozás közvetlenül toohello eszköz soros konzoljához biztonságos, de csatlakozó toohello soros konzolon keresztül hálózati kapcsolók nem. Legyen óvatos az hello biztonsági kockázatot jelent a hálózati kapcsolók keresztül toodevice soros kapcsolódó.
* Egy HTTP-kapcsolaton keresztül csatlakozó felajánlhatja több biztonságos, mint a hálózaton keresztül hello soros konzolon keresztül kapcsolódik. Ez azonban nem hello legbiztonságosabb módszert, az elfogadható megbízható hálózatokon.
* Egy HTTPS-kapcsolaton keresztül csatlakozó hello legbiztonságosabb és ajánlott beállítás hello.

## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>A StorSimple eszközt a StorSimple a Windows PowerShell felügyelete
hello következő tábla összegzi hello általános kezelési feladatok és a bonyolult munkafolyamatok végrehajtható belül hello Windows PowerShell felületet a StorSimple eszköz. Minden munkafolyamat kapcsolatos további információkért kattintson a megfelelő bejegyzés hello hello tábla.

#### <a name="windows-powershell-for-storsimple-workflows"></a>A Windows PowerShell a StorSimple-munkafolyamatok
| Ha azt szeretné, toodo ez... | Ez az eljárás használható. |
| --- | --- |
| Regisztrálja az eszközt |[A Windows PowerShell használatával a StorSimple hello eszköz konfigurálása és regisztrálása](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
| A webproxy konfigurálása</br>Webalkalmazás-proxy beállítások megjelenítése |[A StorSimple eszköz webalkalmazás-proxy konfigurálása](storsimple-configure-web-proxy.md) |
| DATA 0 hálózati kapcsolat az eszközön található beállítások módosítása |[A StorSimple eszközt a DATA 0 hálózati adapterén módosítása](storsimple-modify-data-0.md) |
| A vezérlő leállítása </br> A vezérlő leállítása vagy újraindítása </br> Egy eszköz leállítása</br>Hello eszköz toofactory alapértelmezett beállításainak alaphelyzetbe állítása |[Eszközvezérlők kezelése](storsimple-manage-device-controller.md) |
| Karbantartási mód frissítéseket és gyorsjavításokat telepítése |[Az eszköz frissítése](storsimple-update-device.md) |
| Adja meg a karbantartási mód </br>Kilépés karbantartási mód |[A StorSimple eszköz módok](storsimple-device-modes.md) |
| Hozzon létre egy támogatási csomag</br>Fejti vissza és egy támogatási csomag szerkesztése |[Létrehozását és kezelését egy támogatási csomag](storsimple-create-manage-support-package.md) |
| Támogatási munkamenet indítása</br> |[Támogatási munkamenetet indítani a Windows PowerShellben a StorSimple](storsimple-create-manage-support-package.md#manually-create-a-support-package) |

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Segítség a Windows PowerShellben a StorSimple
A Windows PowerShell-lel a parancsmag súgó áll rendelkezésre. A súgó online, naprakész verzióját is lehetőség, amelynek segítségével tooupdate hello súgó a rendszeren.

Ezen a felületen kapcsolatos segítség kérése hasonló toothat a Windows PowerShellben, és a legtöbb hello súgó kapcsolatos parancsmagok fog működni. Súgó a Windows PowerShell környezethez online hello TechNet-könyvtárban található: [a Windows PowerShell parancsfájlok](http://go.microsoft.com/fwlink/?LinkID=108518).

hello az alábbiakban látható a Windows PowerShell felületén, beleértve a hogyan tooupdate hello súgó hello súgó típusokkal rövid leírása.

#### <a name="tooget-help-for-a-cmdlet"></a>a parancsmag súgójában tooget
* tooget Help parancsmag és funkció, a következő parancs használata hello:`Get-Help <cmdlet-name>`
* tooget bármely parancsmag online súgójában hello előző parancsmag használata hello `-Online` paraméter:`Get-Help <cmdlet-name> -Online`
* A teljes Súgó hello használhatják `–Full` paramétert, és a példákat, használja a hello `–Examples` paraméter.

#### <a name="tooupdate-help"></a>tooupdate Súgó
Segítségre van szüksége a Windows PowerShell-felületet hello hello könnyen frissítheti. Hajtsa végre a lépéseket tooupdate hello súgó a rendszeren a következő hello.

#### <a name="tooupdate-cmdlet-help"></a>tooupdate parancsmag Súgó
1. Indítsa el a Windows Powershellt hello **Futtatás rendszergazdaként** lehetőséget.
2. Hello parancsot, írja be a parancssorba:`Update-Help`
3. hello frissített Súgó fájlok lesznek telepítve.
4. Miután hello súgófájlok telepítve vannak-e, írja be: `Get-Help Get-Command`. Ez megjeleníti a parancsmagok, amelynek súgója is elérhető.

> [!NOTE]
> hello elérhető parancsmagok térben, listájának tooget toohello megfelelő menüpont be, és futtassa a hello `Get-Command` parancsmag.
> 
> 

## <a name="next-steps"></a>Következő lépések
Ha probléma merül fel a StorSimple eszközt a valamelyik fenti munkafolyamatok hello végrehajtása során, tekintse meg a túl[StorSimple központi telepítések hibaelhárítási eszközök](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

