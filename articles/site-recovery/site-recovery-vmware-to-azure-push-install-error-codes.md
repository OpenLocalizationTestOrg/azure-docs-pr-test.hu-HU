---
title: "Az Azure Site Recovery hibaelhárítási VMware az Azure-bA |} Microsoft Docs"
description: "Az Azure virtuális gépek replikálása esetén kapcsolatos hibák elhárítása"
services: site-recovery
documentationcenter: 
author: asgang
manager: srinathv
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/24/2017
ms.author: asgang
ms.openlocfilehash: 1e2452ccc6d3f1859a39e1ee09cdde32f53767ba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-mobility-service-push-install-issues"></a>Mobilitási szolgáltatás leküldéses telepítési problémák elhárítása

Ez a cikk adatokat a próbál telepíteni a mobilitási szolgáltatást a forráskiszolgálón a védelem engedélyezéséhez be tapasztalt gyakori problémákat.

## <a name="error-code-95107-protection-could-not-be-enabled"></a>(Hibakód: 95107) Nem sikerült engedélyezni a védelmet.
**Hibakód:** | **Lehetséges okok** | **Hiba vonatkozó javaslatok**
--- | --- | ---
95107 </br>***Üzenet -*** kódú hiba miatt sikertelen a forrásgépen a mobilitási szolgáltatás leküldéses telepítéséhez ***EP0858***. <br> Vagy, hogy a megadott hitelesítő adatok a mobilitási szolgáltatás telepítése érvénytelen, vagy a felhasználói fiók nem rendelkezik megfelelő jogosultsággal | A védendő gépen telepíteni a mobilitási szolgáltatás megadott felhasználói hitelesítő adatok nem megfelelő | Győződjön meg arról, a konfigurációs kiszolgálón a forrásgép megadott felhasználói hitelesítő adatok helyesek. <br> Felhasználói hitelesítő adatok felvehet és szerkeszthet: Nyissa meg a konfigurációs kiszolgálón > Cspsconfigtool ikon > fiók kezelése. </br> Alább is ellenőrizze sikeresen befejezettnek leküldéses telepítéséhez szükséges előfeltételek a rendszer ellenőrzi.

## <a name="error-code-95015-protection-could-not-be-enabled"></a>(Hibakód: 95015) Nem sikerült engedélyezni a védelmet.
**Hibakód:** | **Lehetséges okok** | **Hiba vonatkozó javaslatok**
--- | --- | ---
95105 </br>***Üzenet -*** kódú hiba miatt sikertelen a forrásgépen a mobilitási szolgáltatás leküldéses telepítéséhez ***EP0856***. <br> Vagy a "Fájl-és nyomtatómegosztás" nem engedélyezett a forrásgépen, vagy a hálózati csatlakozási hibák a folyamatkiszolgáló és a forrásgép között| Fájl- és nyomtatómegosztás nincs engedélyezve | Engedélyezze a "Fájl-és nyomtatómegosztás" a forrásgépen a Windows tűzfalon, nyissa meg a forrásgépen > a Windows tűzfal beállításai > "Engedélyezése egy alkalmazás vagy szolgáltatás átengedése a tűzfalon" > válassza a "Fájl- és nyomtatómegosztás minden profil esetében". </br> Alább is ellenőrizze sikeresen befejezettnek leküldéses telepítéséhez szükséges előfeltételek a rendszer ellenőrzi.

## <a name="error-code-95117-protection-could-not-be-enabled"></a>(Hibakód: 95117) Nem sikerült engedélyezni a védelmet.
**Hibakód:** | **Lehetséges okok** | **Hiba vonatkozó javaslatok**
--- | --- | ---
95117 </br>***Üzenet -*** kódú hiba miatt sikertelen a forrásgépen a mobilitási szolgáltatás leküldéses telepítéséhez ***EP0865***. <br> A forrásgép nem fut, vagy a folyamatkiszolgáló és a forrásgép között a hálózati csatlakozási hibák | A folyamatkiszolgáló és a forráskiszolgáló közötti hálózati kapcsolat | Ellenőrizze a folyamatkiszolgáló és a forráskiszolgáló közötti kapcsolatot. </br> Alább is ellenőrizze sikeresen befejezettnek leküldéses telepítéséhez szükséges előfeltételek a rendszer ellenőrzi.

## <a name="error-code-95103-protection-could-not-be-enabled"></a>(Hibakód: 95103) Nem sikerült engedélyezni a védelmet.
**Hibakód:** | **Lehetséges okok** | **Hiba vonatkozó javaslatok**
--- | --- | ---
95103 </br>***Üzenet -*** kódú hiba miatt sikertelen a forrásgépen a mobilitási szolgáltatás leküldéses telepítéséhez ***EP0854***. <br> "A Windows Management Instrumentation (WMI)" nem engedélyezett a forrásgépen, vagy a folyamatkiszolgáló és a forrásgép között a hálózati csatlakozási hibák| A Windows tűzfal blokkolja a Windows Management Instrumentation (WMI) | A Windows Management Instrumentation (WMI) engedélyezése a Windows tűzfalon. A Windows tűzfal beállításai > "Engedélyezése egy alkalmazás vagy szolgáltatás átengedése a tűzfalon" > "WMI kiválasztása az összes profil". </br> Alább is ellenőrizze sikeresen befejezettnek leküldéses telepítéséhez szükséges előfeltételek a rendszer ellenőrzi.

## <a name="check-push-install-logs-for-errors"></a>Az esetleges hibák ellenőrzéséhez a leküldéses telepítés naplók

A konfiguráció/folyamatkiszolgáló, navigáljon a helyen található "PushinstallService" <Microsoft Azure Site Recovery Install Location>\home\svsystems\pushinstallsvc\ a probléma forrása megértéséhez, valamint az alábbi hibaelhárítási lépések segítségével hárítsa el a problémát.</br>
![pushiinstalllogs](./media/site-recovery-protection-common-errors/pushinstalllogs.png)

## <a name="push-install-pre-requisites-for-windows"></a>A Windows leküldéses telepítési előfeltételek
### <a name="ensure-file-and-printer-sharing-is-enabled"></a>Győződjön meg róla, engedélyezve van a "Fájl-és nyomtatómegosztás"
A forrásoldali számítógép a Windows tűzfalon "Fájl-és nyomtatómegosztás" és "A Windows Management Instrumentation" engedélyezése </br>
#### <a name="if-source-machine-is-domain-joined-br"></a>Ha a forrásgép tartományhoz: </br>
Csoportházirend kezelése konzol (GPMC) használatával, a tűzfal beállításainak konfigurálása.
1. Jelentkezzen be az Active directory tartományhoz gép alkalmazást rendszergazdaként, nyissa meg Csoportházirend kezelése konzol (GPMC. MSC, futtassa a Start > futtatása).</br>
3. Ha a Csoportházirend kezelése KONZOL nincs telepítve, a hivatkozás segítségével [a Csoportházirend kezelése KONZOL telepítése](https://technet.microsoft.com/library/cc725932.aspx) </br>
4. A Csoportházirend kezelése konzol konzolfáján kattintson duplán a csoportházirend-objektumok abban az erdőben és tartományban, és navigáljon az "Alapértelmezett tartományházirend". </br>
![gpmc1](./media/site-recovery-protection-common-errors/gpmc1.png) </br>
</br>
5. Kattintson a jobb gombbal az "Alapértelmezett tartományházirend" > szerkesztése > "Csoportházirendkezelés-szerkesztő" új ablakban nyílik. </br>
![gpmc2](./media/site-recovery-protection-common-errors/gpmc2.png) </br>
</br>
6. A Csoportházirendkezelés-szerkesztő lépjen a számítógép konfigurációja > házirendek > Felügyeleti sablonok > Hálózat > Hálózati kapcsolatok > a Windows tűzfal. </br>
![gpmc3](./media/site-recovery-protection-common-errors/gpmc3.png) </br>
</br>
7. A következő értékeket a tartományi profilba, a szokásos profilt engedélyezése </br>
a) kattintson duplán a kivétel "Windows tűzfal: bejövő fájl- és nyomtatómegosztási kivétel engedélyezése". Válassza ki az engedélyezett, és kattintson az OK gombra. </br>
b) kattintson duplán a kivétel "Windows tűzfal: bejövő Távfelügyeleti kivétel engedélyezése". Válassza ki az engedélyezett, és kattintson az OK gombra. </br>
![gpmc4](./media/site-recovery-protection-common-errors/gpmc4.png) </br>
</br>

###### <a name="if-source-machine-is-not-domain-joined-and-part-of-workgroup-br"></a>Ha a forrásgép nincs tartományhoz és munkacsoporthoz tartozik </br>
Tűzfalbeállítások konfigurálása a távoli gépen (a munkacsoporthoz tartozó):
1. Ugrás a forrásgép</br>
2. A Windows tűzfal beállításai > "Engedélyezése egy alkalmazás vagy szolgáltatás átengedése a tűzfalon" > válassza a "Fájl- és nyomtatómegosztás minden profil esetében". </br>
3. A Windows tűzfal beállításai > "Engedélyezése egy alkalmazás vagy szolgáltatás átengedése a tűzfalon" > "WMI kiválasztása az összes profil". </br>

#### <a name="disable-remote-user-account-control-uac"></a>Tiltsa le a távoli felhasználói fiókok felügyelete (UAC)
Tiltsa le a felhasználói fiókok felügyelete a mobilitási szolgáltatás leküldéses beállításkulcs használatával.
1. Kattintson a Start > futtassa > írja be a regedit > adjon meg
2. Keresse meg és jelölje ki a következő beállításkulcsot: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
3. Ha a LocalAccountTokenFilterPolicy beállításjegyzék-bejegyzés nem létezik, kövesse az alábbi lépéseket:
4. A Szerkesztés menü > Új > kattintson a DWORD értékét.
5. Írja be a LocalAccountTokenFilterPolicy, és nyomja le az ENTER BILLENTYŰT.
6. Kattintson a jobb gombbal a LocalAccountTokenFilterPolicy, és kattintson a módosítás.
7. Az érték mezőbe írja be az 1, és kattintson az OK gombra.
8. Kilépés a Beállításszerkesztő.


## <a name="push-install-pre-requisites-for-linux"></a>Leküldéses telepítése szükséges előfeltételek Linux:

1. A gyökér szintű felhasználó létrehozása Linux forráskiszolgálón. (Ez a fiók csak használják a leküldéses telepítés és a frissítések)</br>
2. Ellenőrizze, hogy a forrás Linux-kiszolgálón található /etc/hosts fájl tartalmaz-e olyan bejegyzéseket, amelyek a helyi gazdanevet az összes hálózati adapterhez társított IP-címekké képezik le. </br>
3. Győződjön meg arról, hogy a legújabb openssh openssh-kiszolgáló és openssl csomag Linux-kiszolgálón telepítve. </br>
Ellenőrizze, hogy ssh 22-es port engedélyezve van és fut.. </br>
4. Ellenőrizze, hogy ha az ügynökök elavult bejegyzések már léteznek a forráskiszolgálón, távolítsa el a régebbi ügynökök, és indítsa újra a kiszolgálót, telepítse újra az ügynököt. </br>

#### <a name="enable-sftp-subsystem-and-password-authentication-in-the-sshdconfig-file"></a>A sshd_config fájl SFTP-alrendszer- és jelszóalapú hitelesítés engedélyezése
1. A forráskiszolgálón jelentkezzen be rendszergazdaként. </br>
2. A fájl /etc/ssh/sshd_config fájlban</br> a sor PasswordAuthentication kezdődő található. </br>
3. d.   Állítsa vissza a sort, és módosítsa az értéket "nem" "Igen" gombra. </br>
![Linux1](./media/site-recovery-protection-common-errors/linux1.png)
4. Keresse meg a sor "Alrendszer" karakterlánccal kezdődik, és állítsa vissza a sor. </br>
![Linux2](./media/site-recovery-protection-common-errors/linux2.png)
5. A módosítások mentéséhez, és indítsa újra a sshd szolgáltatást. </br>

## <a name="push-installation-checks-on-configurationprocess-server"></a>Ügyfélleküldéses telepítés ellenőrzések konfigurációs/folyamatkiszolgáló.
#### <a name="validate-credentials-for-discovery-and-installation"></a>A felderítés és a telepítés hitelesítő adatainak ellenőrzése

1. A konfigurációs kiszolgálón indítsa el a Cspsconfigtool</br>
![WMItestConnect5](./media/site-recovery-protection-common-errors/wmitestconnect5.png) </br>

2. Győződjön meg arról, hogy a a védelemhez használt fióknak rendszergazdai jogosultságokkal a forrásgép. </br>

#### <a name="check-connectivity-between-process-server-and-source-server"></a>Ellenőrizze a folyamatkiszolgáló és a forráskiszolgáló közötti kapcsolatot.
1. Győződjön meg arról, a Folyamatkiszolgáló internetkapcsolattal rendelkezik.
2. A WMI-kapcsolat használatával wbemtest.exe ellenőrzése. </br>
A folyamatkiszolgáló, kattintson a Start > futtassa > wbemtest.exe > Windows Management Instrumentation – tesztelés ablak nyílik látható módon.</br>
   ![WMItestConnect1](./media/site-recovery-protection-common-errors/wmitestconnect1.png) </br>
   </br>
Kattintson a csatlakozás > a Namespace bemeneti felhasználónevet és jelszót adjon meg a forrás-kiszolgáló IP-címe (Ha a forrásgép tartományhoz csatlakozik, adja meg "Tartománynév\felhasználónév", felhasználónév és a tartomány nevét. Ha forrásgép munkacsoporthoz tartozik, csak a felhasználónév megadásához.) Válassza ki a hitelesítési szintet, csomag. </br>
![WMItestConnect2](./media/site-recovery-protection-common-errors/wmitestconnect2.png) </br>
   </br>
   Kattintson a csatlakozás. Most már a WMI-kapcsolat a megadott adatokkal sikeres legyen, és a Windows Management Instrumentation – tesztelés ablak üzenetnek kell megjelennie, alább látható módon: </br>
   ![WMItestConnect3](./media/site-recovery-protection-common-errors/wmitestconnect3.png) </br>
</br>
   Ha a WMI-kapcsolat sikertelen lesz hibaüzenetet előugró. Az alábbi képernyőfelvételen látható sikertelen kísérlet Ha WMI-/ Távfelügyelet nincs engedélyezve a Windows tűzfal által engedélyezett alkalmazás. </br>
   ![WMItestConnect4](./media/site-recovery-protection-common-errors/wmitestconnect4.png) </br>
</br>

3. A WMI állapotát és a kapcsolat ellenőrzése.</br>
A konfiguráció/folyamatkiszolgáló, </br>
Kattintson a start > futtassa > Start > Műveletek > További műveletek > Csatlakozás másik számítógéphez (forrásgép). </br>
Adjon meg a sikeres kapcsolat esetén a védelmet, és ellenőrizze a használt fiók hitelesítő adatait. </br>

#### <a name="verify-network-shared-folders-of-source-machine-is-accessible-from-process-server-ps-remotely-using-specified-credentials"></a>Ellenőrizze, hogy megosztott hálózati mappákból forrásgép elérhető a folyamat Server (PS) távolról a megadott hitelesítő adatokkal.
  1. A bejelentkezési folyamat Server (PS) gépen, nyissa meg a Fájlkezelőben > a cím sáv típus > "\\\source-machine-ip\C$" > kattintson megadása. </br>
  ![Fileshare1](./media/site-recovery-protection-common-errors/fileshare1.png) </br>
  2. A Fájlkezelőben hitelesítő adatok megadását fogja kérni. A felhasználónév és jelszó > kattintson az OK gombra.</br>
   Ha a forrásgép tartományhoz csatlakozik, meg a tartománynevet, felhasználónevet és a "Tartománynév\felhasználónév" adatként.</br>
   Ha a forrásgép egy munkacsoportban található, adja meg a csak a "felhasználónév". </br>
  ![Fileshare2](./media/site-recovery-protection-common-errors/fileshare2.png) </br>
  3. Sikeres kapcsolat esetén megtekintheti a mappák forrásgép távolról a folyamat Server (PS) </br>
  ![Fileshare3](./media/site-recovery-protection-common-errors/fileshare3.png) </br>

> [!NOTE] 
> Ha kapcsolat sikertelen, ellenőrizze, hogy minden szükséges előfeltételek teljesülnek-e.
>

Ha nem szeretné nyissa meg a "Windows Management Instrumentation", is telepíthet mobilitási szolgáltatást manuálisan a forrásgépről.</br> [Telepítse a mobilitási szolgáltatást manuálisan a grafikus felhasználói felületen keresztül](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) </br>
[A configuration manager útmutatást telepítési](site-recovery-install-mobility-service-using-sccm.md) </br>

## <a name="next-steps"></a>Következő lépések
- [A VMware virtuális gépek replikálásának engedélyezése](vmware-walkthrough-enable-replication.md)
