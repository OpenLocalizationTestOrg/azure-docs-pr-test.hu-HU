---
title: "VMware tooAzure a hibaelhárítás a Site Recovery aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 912097c8892540dd798ba025e0b10374ca51d664
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-mobility-service-push-install-issues"></a>Mobilitási szolgáltatás leküldéses telepítési problémák elhárítása

Ez a cikk részletesen hello tooinstall hello mobilitási szolgáltatás a védelem engedélyezéséhez toosource kiszolgálón tett kísérlet során tapasztalt gyakori problémákat.

## <a name="error-code-95107-protection-could-not-be-enabled"></a>(Hibakód: 95107) Nem sikerült engedélyezni a védelmet.
**Hibakód:** | **Lehetséges okok** | **Hiba vonatkozó javaslatok**
--- | --- | ---
95107 </br>***Üzenet -*** hello mobilitási szolgáltatás toohello forrásgép leküldéses telepítése sikertelen volt, hibakód: ***EP0858***. <br> Hogy hello megadott hitelesítő adatok, tooinstall mobilitási szolgáltatás helytelen vagy hello felhasználói fiók nem rendelkezik megfelelő jogosultsággal | A megadott felhasználói hitelesítő adatok tooinstall mobilitási szolgáltatást a forrásgép nem megfelelő | Győződjön meg arról, megadott konfigurációs kiszolgálón hello forrásgép a hello felhasználói hitelesítő adatok helyesek. <br> felhasználói hitelesítő adatok tooadd/szerkesztése: lépjen tooconfiguration server > Cspsconfigtool ikon > fiók kezelése. </br> Ezenkívül győződjön meg arról alábbi előfeltételek vannak bejelölt toosuccessfully teljes leküldéses telepítése.

## <a name="error-code-95015-protection-could-not-be-enabled"></a>(Hibakód: 95015) Nem sikerült engedélyezni a védelmet.
**Hibakód:** | **Lehetséges okok** | **Hiba vonatkozó javaslatok**
--- | --- | ---
95105 </br>***Üzenet -*** hello mobilitási szolgáltatás toohello forrásgép leküldéses telepítése sikertelen volt, hibakód: ***EP0856***. <br> Vagy "Fájl-és nyomtatómegosztás" nem engedélyezett hello forrásgépen, vagy a hálózati csatlakozási hibák hello folyamatkiszolgáló és a hello forrásgép között| Fájl- és nyomtatómegosztás nincs engedélyezve | "Fájl-és nyomtatómegosztás" hello forrásgépen engedélyezése a Windows tűzfal, nyissa meg toohello forrásgép hello > a Windows tűzfal beállításai > "Engedélyezése egy alkalmazás vagy szolgáltatás átengedése a tűzfalon" > válassza a "Fájl- és nyomtatómegosztás minden profil esetében". </br> Ezenkívül győződjön meg arról alábbi előfeltételek vannak bejelölt toosuccessfully teljes leküldéses telepítése.

## <a name="error-code-95117-protection-could-not-be-enabled"></a>(Hibakód: 95117) Nem sikerült engedélyezni a védelmet.
**Hibakód:** | **Lehetséges okok** | **Hiba vonatkozó javaslatok**
--- | --- | ---
95117 </br>***Üzenet -*** hello mobilitási szolgáltatás toohello forrásgép leküldéses telepítése sikertelen volt, hibakód: ***EP0865***. <br> Hello forrásgép nem fut, vagy hálózati csatlakozási hibák hello folyamatkiszolgáló és a hello forrásgép között | A folyamatkiszolgáló és a forráskiszolgáló közötti hálózati kapcsolat | Ellenőrizze a folyamatkiszolgáló és a forráskiszolgáló közötti kapcsolatot. </br> Ezenkívül győződjön meg arról alábbi előfeltételek vannak bejelölt toosuccessfully teljes leküldéses telepítése.

## <a name="error-code-95103-protection-could-not-be-enabled"></a>(Hibakód: 95103) Nem sikerült engedélyezni a védelmet.
**Hibakód:** | **Lehetséges okok** | **Hiba vonatkozó javaslatok**
--- | --- | ---
95103 </br>***Üzenet -*** hello mobilitási szolgáltatás toohello forrásgép leküldéses telepítése sikertelen volt, hibakód: ***EP0854***. <br> Vagy "Windows Management Instrumentation (WMI)" nem engedélyezett hello forrásgépen, vagy hálózati csatlakozási hibák hello folyamatkiszolgáló és a hello forrásgép között| Hello Windows tűzfal blokkolja a Windows Management Instrumentation (WMI) | A Windows Management Instrumentation (WMI) engedélyezése a Windows tűzfal hello. A Windows tűzfal beállításai > "Engedélyezése egy alkalmazás vagy szolgáltatás átengedése a tűzfalon" > "WMI kiválasztása az összes profil". </br> Ezenkívül győződjön meg arról alábbi előfeltételek vannak bejelölt toosuccessfully teljes leküldéses telepítése.

## <a name="check-push-install-logs-for-errors"></a>Az esetleges hibák ellenőrzéséhez a leküldéses telepítés naplók

Hello konfigurációs/folyamatkiszolgáló, keresse meg a helyen található "PushinstallService" toofile <Microsoft Azure Site Recovery Install Location>\home\svsystems\pushinstallsvc\ toounderstand hello forrás hello probléma és az alábbi lépéseket tooresolve hello hiba elhárításához használja.</br>
![pushiinstalllogs](./media/site-recovery-protection-common-errors/pushinstalllogs.png)

## <a name="push-install-pre-requisites-for-windows"></a>A Windows leküldéses telepítési előfeltételek
### <a name="ensure-file-and-printer-sharing-is-enabled"></a>Győződjön meg róla, engedélyezve van a "Fájl-és nyomtatómegosztás"
Annak engedélyezése, hogy "Fájl-és nyomtatómegosztás" és "A Windows Management Instrumentation" hello forrásgép hello Windows tűzfal </br>
#### <a name="if-source-machine-is-domain-joined-br"></a>Ha a forrásgép tartományhoz: </br>
Csoportházirend kezelése konzol (GPMC) használatával, a tűzfal beállításainak konfigurálása.
1. Bejelentkezési tooActive directory tartományi rendszergazdaként számítógéphez, és nyissa meg a Csoportházirend kezelése konzol (GPMC. MSC, futtassa a Start > futtatása).</br>
3. Ha nincs telepítve a Csoportházirend kezelése KONZOL, kattintson a hivatkozásra hello túl[telepítés hello Csoportházirend kezelése KONZOL](https://technet.microsoft.com/library/cc725932.aspx) </br>
4. A hello Csoportházirend kezelése konzol konzolfáján kattintson duplán a csoportházirend-objektumok hello erdőben és tartományban, és keresse meg a túl "Alapértelmezett tartományházirend". </br>
![gpmc1](./media/site-recovery-protection-common-errors/gpmc1.png) </br>
</br>
5. Kattintson a jobb gombbal az "Alapértelmezett tartományházirend" > szerkesztése > "Csoportházirendkezelés-szerkesztő" új ablakban nyílik. </br>
![gpmc2](./media/site-recovery-protection-common-errors/gpmc2.png) </br>
</br>
6. A Csoportházirendkezelés-szerkesztő hello keresse meg a konfigurációs tooComputer > házirendek > Felügyeleti sablonok > Hálózat > Hálózati kapcsolatok > Windows tűzfal. </br>
![gpmc3](./media/site-recovery-protection-common-errors/gpmc3.png) </br>
</br>
7. A következő beállításokat a tartományi profilba, a szokásos profilt hello engedélyezése </br>
a) kattintson duplán a kivétel "Windows tűzfal: bejövő fájl- és nyomtatómegosztási kivétel engedélyezése". Válassza ki az engedélyezett, és kattintson az OK gombra. </br>
b) kattintson duplán a kivétel "Windows tűzfal: bejövő Távfelügyeleti kivétel engedélyezése". Válassza ki az engedélyezett, és kattintson az OK gombra. </br>
![gpmc4](./media/site-recovery-protection-common-errors/gpmc4.png) </br>
</br>

###### <a name="if-source-machine-is-not-domain-joined-and-part-of-workgroup-br"></a>Ha a forrásgép nincs tartományhoz és munkacsoporthoz tartozik </br>
Tűzfalbeállítások konfigurálása a távoli gépen (a munkacsoporthoz tartozó):
1. Nyissa meg a forrásgép toohello,</br>
2. A Windows tűzfal beállításai > "Engedélyezése egy alkalmazás vagy szolgáltatás átengedése a tűzfalon" > válassza a "Fájl- és nyomtatómegosztás minden profil esetében". </br>
3. A Windows tűzfal beállításai > "Engedélyezése egy alkalmazás vagy szolgáltatás átengedése a tűzfalon" > "WMI kiválasztása az összes profil". </br>

#### <a name="disable-remote-user-account-control-uac"></a>Tiltsa le a távoli felhasználói fiókok felügyelete (UAC)
Tiltsa le a felhasználói fiókok Felügyeletének beállításjegyzékbeli kulcs toopush hello mobilitási szolgáltatás használatával.
1. Kattintson a Start > futtassa > írja be a regedit > adjon meg
2. Keresse meg és kattintson a következő beállításkulcsot hello: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
3. Ha hello LocalAccountTokenFilterPolicy bejegyzés nem létezik, kövesse az alábbi lépéseket:
4. A Szerkesztés menü hello > Új > kattintson a DWORD értékét.
5. Írja be a LocalAccountTokenFilterPolicy, és nyomja le az ENTER BILLENTYŰT.
6. Kattintson a jobb gombbal a LocalAccountTokenFilterPolicy, és kattintson a módosítás.
7. Hello érték mezőbe írja be az 1, és kattintson az OK gombra.
8. Kilépés a Beállításszerkesztő.


## <a name="push-install-pre-requisites-for-linux"></a>Leküldéses telepítése szükséges előfeltételek Linux:

1. Hozzon létre egy gyökér szintű felhasználó hello Linux forráskiszolgálón. (Ezt a fiókot használja csak a hello ügyfélleküldéses telepítés és frissítés)</br>
2. Ellenőrizze, hogy hello forrás Linux-kiszolgáló rendelkezik bejegyzéseit, amelyek az összes hálózati adapter társított hello helyi állomásnevével tooIP címek leképezése hello/Etc/Hosts fájlt. </br>
3. Ellenőrizze, hogy hello legújabb openssh openssh-kiszolgáló és openssl-csomagok telepített Linux-kiszolgálón. </br>
Ellenőrizze, hogy ssh 22-es port engedélyezve van és fut.. </br>
4. Ellenőrizze, ha már hello forráskiszolgálón lévő ügynökök elavult bejegyzéseket, távolítsa el a régebbi ügynökök hello és hello kiszolgáló újraindul, telepítse újra az ügynököt. </br>

#### <a name="enable-sftp-subsystem-and-password-authentication-in-hello-sshdconfig-file"></a>Hello sshd_config fájl SFTP-alrendszer- és jelszóalapú hitelesítés engedélyezése
1. A forráskiszolgálón jelentkezzen be rendszergazdaként. </br>
2. Hello fájl /etc/ssh/sshd_config fájlban</br> PasswordAuthentication kezdődő hello sort található. </br>
3. d.   Állítsa vissza a hello sor, és módosítsa a "no" hello értéket túl "yes". </br>
![Linux1](./media/site-recovery-protection-common-errors/linux1.png)
4. Keresse meg "Alrendszer" kezdődő hello sort, és állítsa vissza a hello sor. </br>
![Linux2](./media/site-recovery-protection-common-errors/linux2.png)
5. Hello módosítások mentéséhez, és indítsa újra hello sshd szolgáltatást. </br>

## <a name="push-installation-checks-on-configurationprocess-server"></a>Ügyfélleküldéses telepítés ellenőrzések konfigurációs/folyamatkiszolgáló.
#### <a name="validate-credentials-for-discovery-and-installation"></a>A felderítés és a telepítés hitelesítő adatainak ellenőrzése

1. A konfigurációs kiszolgálón indítsa el a Cspsconfigtool</br>
![WMItestConnect5](./media/site-recovery-protection-common-errors/wmitestconnect5.png) </br>

2. Győződjön meg arról, hogy használt védelemre hello fiók rendelkezik-e rendszergazda jogosultságokkal a forrásgép hello. </br>

#### <a name="check-connectivity-between-process-server-and-source-server"></a>Ellenőrizze a folyamatkiszolgáló és a forráskiszolgáló közötti kapcsolatot.
1. Győződjön meg arról, a Folyamatkiszolgáló internetkapcsolattal rendelkezik.
2. A WMI-kapcsolat használatával wbemtest.exe ellenőrzése. </br>
Hello folyamatkiszolgáló, kattintson a Start > futtassa > wbemtest.exe > Windows Management Instrumentation – tesztelés ablak nyílik látható módon.</br>
   ![WMItestConnect1](./media/site-recovery-protection-common-errors/wmitestconnect1.png) </br>
   </br>
Kattintson a csatlakozás > hello forrás kiszolgáló IP-cím hello Namespace bemeneti felhasználónevet és jelszót adjon meg (Ha a forrásgép tartományhoz, adjon meg hello felhasználó felhasználóneve megegyezik "Tartománynév\felhasználónév" együtt. Ha a forrásgép egy munkacsoportban található, adjon meg csak hello.) Jelöljön ki hello hitelesítési szint, csomag. </br>
![WMItestConnect2](./media/site-recovery-protection-common-errors/wmitestconnect2.png) </br>
   </br>
   Kattintson a csatlakozás. Most a megadott adatok hello hello WMI-kapcsolat sikeres legyen, és hello Windows Management Instrumentation – tesztelés ablak üzenetnek kell megjelennie, alább látható módon: </br>
   ![WMItestConnect3](./media/site-recovery-protection-common-errors/wmitestconnect3.png) </br>
</br>
   Ha a WMI-kapcsolat sikertelen lesz hibaüzenetet előugró. hello alábbi képernyőfelvételen látható sikertelen kísérlet Ha WMI-/ Távfelügyelet nincs engedélyezve a Windows tűzfal által engedélyezett alkalmazás. </br>
   ![WMItestConnect4](./media/site-recovery-protection-common-errors/wmitestconnect4.png) </br>
</br>

3. Ellenőrizze a hello WMI állapotát és a kapcsolat.</br>
Hello konfigurációs/folyamat kiszolgálón </br>
Kattintson a start > futtassa > Start > Műveletek > További műveletek > tooanother számítógép (forrásgép). </br>
Adjon meg hello hitelesítő adatok védelmére és ellenőrzés használja, ha csatlakozási finom hello fiók. </br>

#### <a name="verify-network-shared-folders-of-source-machine-is-accessible-from-process-server-ps-remotely-using-specified-credentials"></a>Ellenőrizze, hogy megosztott hálózati mappákból forrásgép elérhető a folyamat Server (PS) távolról a megadott hitelesítő adatokkal.
  1. Bejelentkezési tooProcess Server (PS) számítógép, nyissa meg a Fájlkezelőben > hello cím sáv típus > "\\\source-machine-ip\C$" > kattintson az ENTER billentyűt. </br>
  ![Fileshare1](./media/site-recovery-protection-common-errors/fileshare1.png) </br>
  2. A Fájlkezelőben hitelesítő adatok megadását fogja kérni. Adja meg a hello felhasználónév és jelszó > kattintson az OK gombra.</br>
   Ha a forrásgép tartományhoz csatlakozik, olyan "Tartománynév\felhasználónév" hello tartománynevet, felhasználónevet és.</br>
   Ha a forrásgép egy munkacsoportban található, adja meg csak hello "felhasználónév". </br>
  ![Fileshare2](./media/site-recovery-protection-common-errors/fileshare2.png) </br>
  3. Ha a kapcsolódás sikeres, megtekintheti a hello mappák forrásgép távolról a folyamat Server (PS) </br>
  ![Fileshare3](./media/site-recovery-protection-common-errors/fileshare3.png) </br>

> [!NOTE] 
> Ha kapcsolat sikertelen, ellenőrizze, hogy minden szükséges előfeltételek teljesülnek-e.
>

Ha nem szeretné, "A Windows Management Instrumentation" tooopen, is telepíthet mobilitási szolgáltatást manuálisan hello forrásgépen.</br> [Telepítse a mobilitási szolgáltatást manuálisan a grafikus felhasználói felületen keresztül](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) </br>
[A configuration manager útmutatást telepítési](site-recovery-install-mobility-service-using-sccm.md) </br>

## <a name="next-steps"></a>Következő lépések
- [A VMware virtuális gépek replikálásának engedélyezése](vmware-walkthrough-enable-replication.md)
