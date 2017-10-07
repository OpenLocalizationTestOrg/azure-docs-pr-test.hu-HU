---
title: "aaaConnect távolról tooyour StorSimple eszköz |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooconfigure távoli kezelésére szolgál az eszközt, és hogyan tooconnect tooWindows PowerShell-lel HTTP vagy HTTPS PROTOKOLLON keresztül."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 923377aa-f451-4656-87de-5e95a34a6a2a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 55ed8fcdd997901301e0adc164a302216cde0332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-remotely-tooyour-storsimple-8000-series-device"></a>Távoli csatlakozás tooyour a StorSimple 8000 series eszköz

## <a name="overview"></a>Áttekintés
A Windows PowerShell távoli eljáráshívás tooconnect tooyour StorSimple eszközt is használhatja. Ezzel a módszerrel csatlakoztatásakor menü nem jelenik meg. (Megjelenik a menü csak akkor, ha a soros konzol hello hello eszköz tooconnect használ.) A Windows PowerShell-távelérést csatlakozás tooa adott futási térből. Hello megjelenítési nyelv is megadható. 

A Windows PowerShell távoli eljáráshívás toomanage az eszköz használatával kapcsolatos további információkért lépjen túl[a Windows Powershellt használja a StorSimple tooadminister a StorSimple eszköz](storsimple-windows-powershell-administration.md).

Ez az oktatóanyag azt ismerteti, hogyan tooconfigure majd és a távoli felügyeleti eszközét tooconnect tooWindows PowerShell-lel. A Windows PowerShell távoli eljáráshívás keresztül HTTP vagy HTTPS tooconnect is használhatja. Azonban meghatározásakor hogyan tooconnect tooWindows PowerShell-lel, hello következőket vegye figyelembe: 

* Csatlakozás közvetlenül toohello eszköz soros konzoljához biztonságos, de csatlakozó toohello soros konzolon keresztül hálózati kapcsolók nem. Legyen óvatos az hello biztonsági kockázatot jelent a toohello eszköz soros konzolon keresztül hálózati kapcsolók kapcsolódó. 
* Egy HTTP-kapcsolaton keresztül csatlakozó felajánlhatja a további biztonsági mint hello hálózati hello soros konzolon keresztül. Ez azonban nem hello legbiztonságosabb módszert, az elfogadható megbízható hálózatokon. 
* Hello legbiztonságosabb és ajánlott beállítás hello keresztül egy önaláírt tanúsítványt a HTTPS-KAPCSOLATON keresztül csatlakozó.

Csatlakoztathatja távolról toohello Windows PowerShell-felületet. Azonban távelérési tooyour StorSimple eszközt hello Windows PowerShell felületén keresztül nem alapértelmezés szerint engedélyezve van. Először kell tooenable Távfelügyelet hello eszközön, és majd a hello használt tooaccess ügyfélnek az eszközt.

Ebben a cikkben ismertetett hello lépéseket a Windows Server 2012 R2 rendszerű gazdarendszer végrehajtott.

## <a name="connect-through-http"></a>Kapcsolódás HTTP Protokollon keresztül
Csatlakozás tooWindows PowerShell StorSimple egy HTTP-kapcsolaton keresztül hello a StorSimple eszköz soros konzolon keresztül csatlakozó-nál nagyobb védelmet kínál. Ez azonban nem hello legbiztonságosabb módszert, az elfogadható megbízható hálózatokon.

A klasszikus Azure portálon hello vagy hello soros konzol tooconfigure Távfelügyelet is használhatja. Válassza ki az alábbi eljárásokat hello:

* [HTTP Protokollon keresztül hello Azure klasszikus portál tooenable Távfelügyelet használata](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [HTTP Protokollon keresztül hello soros konzol tooenable Távfelügyelet használata](#use-the-serial-console-to-enable-remote-management-over-http)

Miután engedélyezte távfelügyeletet, használja a következő eljárás tooprepare hello ügyfél a távoli kapcsolat hello.

* [Hello-ügyfél előkészítése a távoli kapcsolat](#prepare-the-client-for-remote-connection)

### <a name="use-hello-azure-classic-portal-tooenable-remote-management-over-http"></a>HTTP Protokollon keresztül hello Azure klasszikus portál tooenable Távfelügyelet használata
Hajtsa végre a HTTP Protokollon keresztül hello Azure klasszikus portál tooenable Távfelügyelet lépések hello.

#### <a name="tooenable-remote-management-through-hello-azure-classic-portal"></a>távoli felügyeleti tooenable hello a klasszikus Azure portálon keresztül
1. Hozzáférés **eszközök** > **konfigurálása** az eszközhöz.
2. Görgessen lefelé toohello **Távfelügyelet** szakasz.
3. Állítsa be **távoli felügyelet engedélyezése** túl**Igen**.
4. Ezután eldöntheti, tooconnect HTTP-n keresztül. (hello alapértelmezett érték tooconnect HTTPS-KAPCSOLATON keresztül.) Győződjön meg arról, hogy a HTTP van kiválasztva.
   
   > [!NOTE]
   > A HTTP-n keresztüli csatlakozást kizárólag megbízható hálózatokon célszerű engedélyezni.
   > 
   > 
5. Kattintson a **mentése** hello lap hello alján.

### <a name="use-hello-serial-console-tooenable-remote-management-over-http"></a>HTTP Protokollon keresztül hello soros konzol tooenable Távfelügyelet használata
Hajtsa végre a következő lépéseket a hello eszköz soros konzoljához tooenable távoli felügyeleti hello.

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a>távoli felügyeleti tooenable hello eszköz soros konzolon keresztül
1. A hello soros konzol menüben válassza a 1. lehetőség. Hello eszközön hello soros konzol használatával kapcsolatos további információkért lépjen túl[tooWindows PowerShell csatlakoztatni a StorSimple eszköz soros konzolon keresztül](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Hello, írja be a parancssorba:`Enable-HcsRemoteManagement –AllowHttp`
3. Értesítést fog kapni hello biztonsági rések HTTP tooconnect toohello eszköz használatával kapcsolatban. Amikor a rendszer kéri, erősítse meg, írja be a **Y**.
4. Győződjön meg arról, hogy a HTTP engedélyezett beírásával:`Get-HcsSystem`
5. Győződjön meg arról, hogy hello **RemoteManagementMode** mezőben látható **HttpsAndHttpEnabled**.hello a következő ábrán az ezek a beállítások a PuTTY jeleníti meg.
   
     ![Soros HTTPS- és HTTP engedélyezve](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-hello-client-for-remote-connection"></a>Hello-ügyfél előkészítése a távoli kapcsolat
Hajtsa végre a következő lépéseket a hello ügyfél tooenable távoli felügyeleti hello.

#### <a name="tooprepare-hello-client-for-remote-connection"></a>a távoli kapcsolat tooprepare hello ügyfél
1. Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.
2. Írja be a következő parancs tooadd hello IP-cím hello StorSimple eszköz toohello ügyfél megbízható gazdagépek listájának hello: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     Cserélje le <*device_ip*> hello IP-cím, az eszköz; például: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Írja be a következő parancs toosave hello eszközhitelesítő adatok változóként hello: 
   
    ```
    $cred = Get-Credential
    ```
    
4. Hello párbeszédpanel, amely akkor jelenik meg:
   
   1. Írja be a hello felhasználónevet a következő formátumban: *device_ip\SSAdmin*.
   2. Írja be a hello eszköz rendszergazdai jelszava lett beállítva, amikor hello eszköz hello beállítása varázsló lett konfigurálva. hello alapértelmezett jelszó *jelszó1*.
5. Ez a parancs beírásával indítsa el a Windows PowerShell-munkamenetet hello eszközön:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > Windows PowerShell-munkamenet hello StorSimple virtuális eszköz való használatra toocreate hozzáfűzése hello `–Port` paraméter, és adja meg a StorSimple virtuális készülék távoli eljáráshívási konfigurált hello nyilvános port.
   > 
   > 
   
     Ezen a ponton rendelkeznie kell egy aktív távoli Windows PowerShell munkamenet toohello eszköz.
   
    ![PowerShell távvezérlése HTTP-n keresztül](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>HTTPS keresztül csatlakozni
StorSimple tooWindows PowerShell csatlakozás egy HTTPS-kapcsolaton keresztül hello legbiztonságosabb és ajánlott távolról csatlakozó tooyour Microsoft Azure StorSimple eszköz metódusában. hello a következő eljárások azt ismertetik, hogyan mentése tooset hello-e soros konzol és az ügyfél számítógépeken, hogy a StorSimple HTTPS tooconnect tooWindows PowerShell használható.

A klasszikus Azure portálon hello vagy hello soros konzol tooconfigure Távfelügyelet is használhatja. Válassza ki az alábbi eljárásokat hello:

* [Hello Azure klasszikus portál tooenable Távfelügyelet használja a HTTPS-KAPCSOLATON keresztül](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [Hello soros konzol tooenable Távfelügyelet használja a HTTPS-KAPCSOLATON keresztül](#use-the-serial-console-to-enable-remote-management-over-https)

Miután engedélyezte távfelügyeletet, használjon a következő eljárások tooprepare hello állomás olyan távoli kezelési hello és toohello eszköz hello távoli állomástól.

* [Hello állomás Felkészülés a távfelügyeletre](#prepare-the-host-for-remote-management)
* [A távoli állomástól hello toohello eszköz csatlakoztatása](#connect-to-the-device-from-the-remote-host)

### <a name="use-hello-azure-classic-portal-tooenable-remote-management-over-https"></a>Hello Azure klasszikus portál tooenable Távfelügyelet használja a HTTPS-KAPCSOLATON keresztül
Hajtsa végre a HTTPS-KAPCSOLATON keresztül hello Azure klasszikus portál tooenable Távfelügyelet lépések hello.

#### <a name="tooenable-remote-management-over-https-from-hello-azure-classic-portal"></a>tooenable Távfelügyelet HTTPS-KAPCSOLATON keresztül a hello a klasszikus Azure portálon
1. Hozzáférés **eszközök** > **konfigurálása** az eszközhöz.
2. Görgessen lefelé toohello **Távfelügyelet** szakasz.
3. Állítsa be **távoli felügyelet engedélyezése** túl**Igen**.
4. Ezután eldöntheti, tooconnect HTTPS-kapcsolaton keresztül. (hello alapértelmezett érték tooconnect HTTPS-KAPCSOLATON keresztül.) Győződjön meg arról, hogy HTTPS van-e kiválasztva. 
5. Kattintson a **távfelügyeleti tanúsítvány letöltése**. Adjon meg egy helyet toosave ezt a fájlt. Szüksége lesz tooinstall tooconnect toohello eszköz használt hello ügyfélszámítógépre vagy állomásra számítógépen a tanúsítvány.
6. Kattintson a **mentése** hello lap hello alján.

### <a name="use-hello-serial-console-tooenable-remote-management-over-https"></a>Hello soros konzol tooenable Távfelügyelet használja a HTTPS-KAPCSOLATON keresztül
Hajtsa végre a következő lépéseket a hello eszköz soros konzoljához tooenable távoli felügyeleti hello.

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a>távoli felügyeleti tooenable hello eszköz soros konzolon keresztül
1. A hello soros konzol menüben válassza a 1. lehetőség. Hello eszközön hello soros konzol használatával kapcsolatos további információkért lépjen túl[tooWindows PowerShell csatlakoztatni a StorSimple eszköz soros konzolon keresztül](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Hello, írja be a parancssorba: 
   
     `Enable-HcsRemoteManagement`
   
    Ez engedélyezze a HTTPS-eszközön.
3. Ellenőrizze, hogy engedélyezték-e a HTTPS beírásával: 
   
     `Get-HcsSystem`
   
    Győződjön meg arról, hogy hello **RemoteManagementMode** mezőben látható **HttpsEnabled**.hello a következő ábrán az ezek a beállítások a PuTTY jeleníti meg.
   
     ![Soros HTTPS-kompatibilis](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. Hello kimenetét a `Get-HcsSystem`, hello eszköz sorozatszámát hello másolja ki és mentse azt későbbi használatra.
   
   > [!NOTE]
   > hello sorozatszám maps toohello hello tanúsítvány CN-név.
   > 
   > 
5. Írja be a távoli felügyeleti tanúsítvány beszerzése: 
   
     `Get-HcsRemoteManagementCert`
   
    A tanúsítvány hasonló toohello következő jelenik meg.
   
    ![Távfelügyeleti tanúsítvány beszerzése](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. Hello adatok másolása hello tanúsítványban lévő **---BEGIN CERTIFICATE---** túl**---vége tanúsítvány---** egy szövegszerkesztőbe például a Jegyzettömbben, és mentse azt egy .cer fájlba. (Fogja másolni a fájl tooyour távoli állomás, hello állomás előkészítésekor.)
   
   > [!NOTE]
   > toogenerate egy új tanúsítvány használatára hello `Set-HcsRemoteManagementCert` parancsmag.
   > 
   > 

### <a name="prepare-hello-host-for-remote-management"></a>Hello állomás Felkészülés a távfelügyeletre
tooprepare hello fogadó számítógép a távoli kapcsolat által használt HTTPS-KAPCSOLATON keresztül, hajtsa végre az alábbi eljárásokat hello:

* [Importálási hello .cer fájl hello ügyfél vagy a távoli állomás hello legfelső szintű tárolóba](#to-import-the-certificate-on-the-remote-host).
* [Adjon hozzá hello eszköz sorozatszámokat toohello hosts fájlt a távoli állomáson levő](#to-add-device-serial-numbers-to-the-remote-host).

Ezek az eljárások leírását alább.

#### <a name="tooimport-hello-certificate-on-hello-remote-host"></a>hello távoli állomáson levő tooimport hello tanúsítvány
1. Kattintson a jobb gombbal a hello .cer fájlt, és válassza ki **telepítés tanúsítvány**. Tanúsítványimportáló varázsló hello elindítja.
   
    ![Tanúsítványimportáló varázsló 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. A **tárolóhelyére**, jelölje be **helyi számítógép**, és kattintson a **következő**.
3. Válassza ki **minden tanúsítvány tárolása a következő tároló hello**, és kattintson a **Tallózás**. Keresse meg a távoli állomás toohello főtárolójához, és kattintson a **következő**.
   
    ![Tanúsítványimportáló varázsló 2. régiója](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. Kattintson a **Befejezés** gombra. Megjelenik egy üzenet, amely közli, hogy hello importálása sikeres volt-e.
   
    ![Tanúsítványimportáló varázsló 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="tooadd-device-serial-numbers-toohello-remote-host"></a>tooadd eszköz sorozatszámokat toohello távoli állomás
1. Rendszergazdaként a Jegyzettömb, és nyissa meg a \Windows\System32\Drivers\etc hello állomások fájlba.
2. Adja hozzá a következő három bejegyzések tooyour hosts fájl hello: **DATA 0 IP-cím**, **0 vezérlő rögzített IP-cím**, és **vezérlő 1 fix IP-cím**.
3. Adja meg a korábban mentett hello eszköz gyári számát. Képezze le toohello IP-cím, ahogy az a következő kép hello. A vezérlő 0 és a vezérlő 1 hozzáfűzése **Controller0** és **Controller1** hello végén lévő hello sorozatszám (CN-név).
   
    ![CN-név toohosts fájl hozzáadása](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Mentés hello hosts fájlt.

### <a name="connect-toohello-device-from-hello-remote-host"></a>A távoli állomástól hello toohello eszköz csatlakoztatása
Windows PowerShell és az SSL használata az eszközön, ügyfél vagy egy távoli állomástól SSAdmin munkamenet egy tooenter. hello SSAdmin munkamenet leképezi a hello 1 toooption [soros konzol](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menü az eszköz.

Hajtsa végre a következő eljárás, amelyből el kívánja toomake hello távoli Windows PowerShell-kapcsolat hello számítógépen hello.

#### <a name="tooenter-an-ssadmin-session-on-hello-device-by-using-windows-powershell-and-ssl"></a>egy Windows PowerShell és az SSL használatával hello eszközön SSAdmin munkamenet tooenter
1. Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.
2. Írja be a hello eszköz IP címe toohello ügyfél megbízható gazdagépeket vehet fel:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    Ahol <*device_ip*> hello IP-cím, az eszköz, például: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Hozzon létre új hitelesítő adatokat írja be: 
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    Ahol <*IP-címe céleszközön*> DATA 0 az eszközhöz; hello IP-címe például **10.126.173.90** hello gazdafájl képe megelőző hello ismertetett módon. Emellett adja meg az eszköz rendszergazdai jelszava hello.
4. Írja be a munkamenet létrehozása:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    -ComputerName paramétere hello hello parancsmagot, adja meg a hello <*figyelt eszköz sorozatszámát*>. Ezzel a sorozatszámmal leképezve a DATA 0 hello állomások fájlba a távoli állomáson; toohello IP-címe például **SHX0991003G44MT** hello kép a következő ábrán.
5. Típus: 
   
     `Enter-PSSession $session`
6. Kell toowait néhány percet, és akkor lesz csatlakoztatott tooyour eszközt keresztül HTTPS SSL-en keresztül. Egy üzenet, amely jelzi, hogy csatlakoztatott tooyour eszköz jelenik meg.
   
    ![PowerShell távvezérlése HTTPS és az SSL használata](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Következő lépések
* További információ [Windows PowerShell tooadminister StorSimple-eszköz használata az](storsimple-windows-powershell-administration.md).
* További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

