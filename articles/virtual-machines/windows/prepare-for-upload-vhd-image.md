---
title: "a Windows virtuális merevlemez tooupload tooAzure aaaPrepare |} Microsoft Docs"
description: "Hogyan tooprepare egy Windows VHD- vagy VHDX tooAzure feltöltés előtt"
services: virtual-machines-windows
documentationcenter: 
author: glimoli
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: genli
ms.openlocfilehash: 530390e4c6a4f66ddfd4da23338f9bb3708c299f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-windows-vhd-or-vhdx-tooupload-tooazure"></a>A Windows VHD- vagy VHDX tooupload tooAzure előkészítése
A Windows virtuális gépek (VM) helyszíni tooMicrosoft Azure feltöltés előtt elő kell készítenie hello virtuális merevlemez (VHD- vagy VHDX-). Azure csak 1. generációs virtuális gépek, amelyek hello VHD formátumban és a rögzített méretű lemez támogatja. maximális méret hello engedélyezett hello VHD 1,023 GB. 1 virtuális gép hello VHDX rendszer tooVHD fájlt, és dinamikusan bővülő lemezről toofixed méretű generáció válthat. De nem módosíthatja a virtuális gép generációját. További információkért lásd: [érdemes létrehozni egy 1 vagy 2. generációs virtuális gép a Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

Az Azure virtuális gépekhez hello támogatási szabályzata kapcsolatos további információkért lásd: [Microsoft server szoftver a Microsoft Azure virtuális gépek támogatása](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!Note]
> jelen cikkben lévő utasítások hello toohello 64 bites Windows Server 2008 R2 és újabb Windows server operációs rendszer alkalmazása. További információ az operációs rendszer az Azure-ban futó 32 bites verzióját: [az Azure virtuális gépeken 32 bites operációs rendszerek támogatása](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).

## <a name="convert-hello-virtual-disk-toovhd-and-fixed-size-disk"></a>Átalakítás hello virtuális lemez tooVHD és a rögzített méretű lemez 
Ha tooconvert van szüksége a virtuális lemez toohello formátum Azure, ebben a szakaszban hello módszerek használata szükséges. Készítsen biztonsági másolatot a virtuális gép hello, mielőtt hello virtuális lemezen konvertálási folyamat futtatja, és győződjön meg arról, hogy a Windows virtuális merevlemez megfelelően működik-e a helyi kiszolgálón hello hello. Javítsa ki a hibákat, hello virtuális gépért belül, mielőtt tooconvert próbálja vagy tooAzure töltse fel.

Hello lemez konvertálása után hello alakítja át a lemezt használó virtuális gép létrehozása. Indítsa el, és jelentkezzen be toohello VM toofinish hello virtuális gép előkészítése a feltöltés.

### <a name="convert-disk-using-hyper-v-manager"></a>Hyper-V kezelőjével lemez konvertálása
1. Nyissa meg a Hyper-V kezelőt, és válassza ki a helyi számítógép hello bal oldalon. A hello hello számítógép lista fölött kattintson **művelet** > **szerkesztése lemezre**.
2. A hello **virtuális merevlemez keresése** képernyőn, keresse meg és válassza ki a virtuális lemezt.
3. A hello **művelet kiválasztása** képernyőn, majd válassza ki **átalakítása** és **következő**.
4. Ha a VHDX tooconvert van szüksége, válassza ki a **VHD** majd **tovább**
5. Ha egy dinamikusan bővülő lemezről tooconvert van szüksége, válassza ki a **mérete rögzített** majd **tovább**
6. Keresse meg és jelölje ki az elérési út toosave hello új VHD-fájl.
7. Kattintson a **Befejezés** gombra.

>[!NOTE]
>Ebben a cikkben hello parancsokat egy rendszergazda jogú PowerShell-munkamenetben kell futtatni.

### <a name="convert-disk-by-using-powershell"></a>Konvertálja a lemezt a PowerShell használatával
Virtuális lemez átválthat hello segítségével [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) Windows PowerShell parancsot. Válassza ki **Futtatás rendszergazdaként** PowerShell indításakor. 

hello következő példaparancs alakít VHDX tooVHD, és dinamikusan bővülő lemezek toofixed-méret:

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```
Ebben a parancsban cserélje le a hello értéke "-elérési út" hello elérési toohello virtuális merevlemezzel, amelyen szeretné tooconvert és hello értéke "-DestinationPath" hello új elérési útja és neve hello konvertálja a lemezt.

### <a name="convert-from-vmware-vmdk-disk-format"></a>VMware VMDK lemezformátum konvertálása
Ha Windows Virtuálisgép-lemezkép hello [VMDK-fájl formátumú](https://en.wikipedia.org/wiki/VMDK), átalakíthatja a virtuális merevlemez tooa hello segítségével [Microsoft VM konverter](https://www.microsoft.com/download/details.aspx?id=42497). További információkért lásd: hello blog cikk [hogyan tooConvert VMware VMDK tooHyper-V virtuális merevlemez](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Állítsa be a Windows Azure konfigurációi

A virtuális gép tervezett hello tooupload tooAzure hello követően az összes parancsokat a lépések egy [emelt jogosultságszintű parancssort](https://technet.microsoft.com/library/cc947813.aspx):

1. Távolítsa el a statikus állandó útvonal hello útválasztási táblázat:
   
   * tooview hello útválasztási táblázatot, futtassa `route print` hello parancssorban.
   * Ellenőrizze a hello **adatmegőrzési útvonalak** szakaszok. Ha egy állandó útvonal, akkor [útvonal delete](https://technet.microsoft.com/library/cc739598.apx) tooremove azt.
2. Hello WinHTTP proxy eltávolítása:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```
3. Hello lemez TÁROLÓHÁLÓZATI szabályzatát állítsa túl[Onlineall](https://technet.microsoft.com/library/gg252636.aspx). 
   
    ```PowerShell
    diskpart 
    ```
    Hello megnyitott parancssori ablak írja be a következő parancsok hello:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Windows idő egyezményes világidő (UTC) beállítását és hello Windows időszolgáltatás (w32time) indítási típusa túl hello**automatikusan**:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" 1 -Type DWord

    Set-Service -Name w32time -StartupType Auto
    ```
5. Állítsa be a hello power profil toohello **nagy teljesítményű**:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```

## <a name="check-hello-windows-services"></a>Ellenőrizze a Windows hello-szolgáltatások
Győződjön meg arról, hogy egyes Windows-szolgáltatások a következő hello beállításai toohello **Windows alapértelmezett értékek**. Ezek a minimális számú hello szolgáltatásokról, amelyek meg arról, hogy tud-e a virtuális gép hello toomake be kell állítani. tooreset hello indítási beállításait, futtassa a következő parancsok hello:
   
```PowerShell
Set-Service -Name bfe -StartupType Auto
Set-Service -Name dhcp -StartupType Auto
Set-Service -Name dnscache -StartupType Auto
Set-Service -Name IKEEXT -StartupType Auto
Set-Service -Name iphlpsvc -StartupType Auto
Set-Service -Name netlogon -StartupType Manual
Set-Service -Name netman -StartupType Manual
Set-Service -Name nsi -StartupType Auto
Set-Service -Name termService -StartupType Manual
Set-Service -Name MpsSvc -StartupType Auto
Set-Service -Name RemoteRegistry -StartupType Auto
```

## <a name="update-remote-desktop-registry-settings"></a>A távoli asztal beállításjegyzék-beállítások frissítése
Győződjön meg arról, hogy a következő beállítások hello helyesen vannak-e konfigurálva a távoli asztali kapcsolat:

>[!Note] 
>Hibaüzenet jelenhet meg, hello futtatásakor **Set-ItemProperty-elérési út "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal szolgáltatások – név &lt;objektumnév&gt; &lt;érték&gt;** az alábbi lépéseket. hello hibaüzenet biztonságosan figyelmen kívül hagyhatja. Az azt jelenti, hogy csak adott hello tartomány nem küldését, hogy a konfigurálás egy csoportházirend-objektumon keresztül.
>
>

1. Engedélyezve van a távoli asztal protokoll (RDP):
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDenyTSConnections" -Value 0 -Type DWord
    ```
   
2. RDP-portjára hello helyesen van beállítva (3389-es alapértelmezett):
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "PortNumber" 3389 -Type DWord
    ```
    A virtuális gép telepítésekor hello alapértelmezett szabályok elleni 3389-es port jönnek létre. Toochange hello portszámot, tegye, hogy az Azure-ban hello virtuális gép telepítését követően.

3. hello-figyelő által minden hálózati felületen:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "LanAdapter" 0 -Type DWord
   ```
4. Hello hálózati szintű hitelesítéssel mód konfigurálása a hello RDP-kapcsolatok:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SecurityLayer" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "fAllowSecProtocolNegotiation" 1 -Type DWord
     ```

5. Hello életben tartási érték beállítása:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" 1 -Type DWord
    ```
6. Csatlakozzon újra:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" 0 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" 0 -Type DWord
    ```
7. Hello az egyidejű kapcsolatok számának korlátja:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" 4294967295 -Type DWord
    ```
8. Ha bármely önaláírt tanúsítványokat toohello RDP figyelő kötődik, távolítsa el őket:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash"
    ```
    Ez a toomake meg arról, hogy csatlakoztathatja hello elején hello virtuális gép telepítésekor. Emellett áttekintheti a később a után hello VM a rendszer telepíti az Azure-ban, ha szükséges.

9. Ha hello virtuális gép egy tartomány része lesz, ellenőrizze a összes hello beállítások toomake meg arról, hogy hello korábbi beállításokat nem állítja vissza a következő. hello házirendek ellenőrizendő hello következők tartoznak:
    
    - RDP engedélyezve van:

         Számítógép konfigurációja\Házirendek\A Settings\Administrative Templates\ összetevők\Távoli asztali szolgáltatások\Távoli asztali munkamenetgazda\Kapcsolatok:
         
         **Távoli asztal használatával távolról lehetővé teszi a felhasználók tooconnect**

    - NLA csoportházirend:

        Settings\Administrative Templates\Components\Remote asztali szolgáltatások\Távoli asztali munkamenetgazda\Biztonság: 
        
        **Távoli kapcsolatok hálózati szintű hitelesítéssel felhasználói hitelesítés megkövetelése**
    
    - Élő beállítások megtartásához:

        Számítógép konfigurációja\Házirendek\A Settings\Administrative sablonok\Windows összetevők\Távoli asztali szolgáltatások\Távoli asztali munkamenetgazda\Kapcsolatok: 
        
        **Keep-alive kapcsolat időköz beállítása**

    - Csatlakozzon újra a beállításokat:

        Számítógép konfigurációja\Házirendek\A Settings\Administrative sablonok\Windows összetevők\Távoli asztali szolgáltatások\Távoli asztali munkamenetgazda\Kapcsolatok: 
        
        **Az automatikus újracsatlakozás**

    - Hello számának korlátozása kapcsolatbeállításai:

        Számítógép konfigurációja\Házirendek\A Settings\Administrative sablonok\Windows összetevők\Távoli asztali szolgáltatások\Távoli asztali munkamenetgazda\Kapcsolatok: 
        
        **A kapcsolatok száma**

## <a name="configure-windows-firewall-rules"></a>A Windows tűzfal-szabályok konfigurálása
1. A Windows tűzfal bekapcsolása a három hello profilok (a tartományt, a Standard és a nyilvános):

   ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\Standardprofile' -name "EnableFirewall" -Value 1 -Type DWord
   ```

2. Futtassa a következő parancsot a PowerShell tooallow WinRM hello három hálózatitűzfal-profilok (tartományi, privát és nyilvános) keresztül hello és hello PowerShell távoli szolgáltatás engedélyezése:
   
   ```PowerShell
    Enable-PSRemoting -force
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
   ```
3. A következő tűzfal szabályok tooallow hello RDP-forgalmát hello engedélyezése 

   ```PowerShell
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes
   ```   
4. Engedélyezze a hello fájl- és nyomtatómegosztás szabály, hogy hello VM válaszolhassanak tooa ping parancs hello virtuális hálózaton belül:

   ```PowerShell
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes
   ``` 
5. Hello virtuális gép egy tartomány része lesz, ha ellenőrizze a következő beállítások toomake meg arról, hogy hello korábbi beállításokat nem állítja vissza a hello. hello AD házirendek ellenőrizendő hello következők tartoznak:

    - A Windows tűzfal hello-profilok engedélyezése

        Számítógép konfigurációja\Házirendek\A Settings\Administrative sablonok\Hálózat\Hálózati Connection\Windows Firewall\Domain Profile\Windows tűzfal: **minden hálózati kapcsolatok védelme**

       Számítógép konfigurációja\Házirendek\A Settings\Administrative sablonok\Hálózat\Hálózati Connection\Windows Firewall\Standard Profile\Windows tűzfal: **minden hálózati kapcsolatok védelme**

    - RDP engedélyezése 

        Számítógép konfigurációja\Házirendek\A Settings\Administrative sablonok\Hálózat\Hálózati Connection\Windows Firewall\Domain Profile\Windows tűzfal: **bejövő távoli asztal Kivételek tiltása**

        Számítógép konfigurációja\Házirendek\A Settings\Administrative sablonok\Hálózat\Hálózati Connection\Windows Firewall\Standard Profile\Windows tűzfal: **bejövő távoli asztal Kivételek tiltása**

    - ICMP-V4 engedélyezése

        Számítógép konfigurációja\Házirendek\A Settings\Administrative sablonok\Hálózat\Hálózati Connection\Windows Firewall\Domain Profile\Windows tűzfal: **ICMP kivételek tiltása**

        Számítógép konfigurációja\Házirendek\A Settings\Administrative sablonok\Hálózat\Hálózati Connection\Windows Firewall\Standard Profile\Windows tűzfal: **ICMP kivételek tiltása**

## <a name="verify-vm-is-healthy-secure-and-accessible-with-rdp"></a>Győződjön meg arról VM kifogástalan, biztonságos és RDP-érhető el 
1. toomake meg arról, hogy hello lemez kifogástalan állapotú és egységes, futtassa egy ellenőrző lemez művelet hello következő virtuális gép újraindítása:

    ```PowerShell
    Chkdsk /f
    ```
    Győződjön meg arról, hogy hello jelentésben egy tiszta és egészséges lemezt.

2. Hello rendszerindítási konfigurációs adatok (BCD) beállítása. 

    > [!Note]
    > Győződjön meg arról, hogy egy rendszergazda jogú CMD-ablakban futtassa az alábbi parancsokat és **nem** a PowerShell:
   
   ```CMD
   bcdedit /set {bootmgr} integrityservices enable
   
   bcdedit /set {default} device partition=C:
   
   bcdedit /set {default} integrityservices enable
   
   bcdedit /set {default} recoveryenabled Off
   
   bcdedit /set {default} osdevice partition=C:
   
   bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
   ```
3. Győződjön meg arról, hogy hello Windows Management Instrumentations tárház egységes. tooperform a, a következő parancs futtatása hello:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Ha hello tárház sérült, lásd: [WMI: tárház sérülése, vagy nem](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

4. Győződjön meg arról, hogy más alkalmazás nem használ hello 3389-es portot. Ezt a portot használja a hello RDP-szolgáltatás az Azure-ban. Futtathat **netstat - anob** toosee portokat a használt virtuális gép hello:

    ```PowerShell
    netstat -anob
    ```

5. Ha Windows VHD-t, amelyet az tooupload hello egy olyan tartományvezérlőre, majd kövesse az alábbi lépéseket:

    A. Hajtsa végre a [további lépések](https://support.microsoft.com/kb/2904015) tooprepare hello lemez.

    B. Győződjön meg arról, hogy ismeri hello DSRM-jelszót, abban az esetben toostart hello virtuális gép címtárszolgáltatás-javító módban egy bizonyos ponton rendelkezik. Érdemes lehet toorefer toothis hivatkozás tooset hello [DSRM-jelszót](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

6. Győződjön meg arról, hogy hello beépített Rendszergazda fiók és jelszó tooyou ismert. Előfordulhat, hogy szeretné, hogy tooreset hello aktuális helyi rendszergazda jelszavát, és győződjön meg arról, hogy a fiók toosign használhatja a tooWindows hello RDP-kapcsolaton keresztül. A hozzáférési engedélyt hello "Bejelentkezés engedélyezése a távoli asztali szolgáltatások használatával" csoportházirend-objektum vezérli. Ez az objektum megtekintheti a Helyicsoportházirend-szerkesztő hello alatt:

    Számítógép konfigurációja\A Windows beállításai\Biztonsági beállítások\Helyi házirendek\Felhasználói jogosultságok kiosztása

7. Ellenőrizze, hogy a következő AD hello házirendek toomake meg arról, hogy az RDP-hozzáférés RDP keresztül, és nem hello hálózatról nem blokkolják:

    - Számítógép konfigurációja\A Windows beállításai\Biztonsági beállítások\Helyi házirendek\Felhasználói jogosultságok Assignment\Deny hozzáférés toothis számítógép hello hálózatról

    - Számítógép konfigurációja\A Windows beállításai\Biztonsági beállítások\Helyi házirendek\Felhasználói jogosultságok Assignment\Deny naplót a távoli asztali szolgáltatások használatával


8. Hello RDP-kapcsolaton keresztül elérhető újraindítás hello VM toomake arra, hogy a Windows továbbra is működik megfelelően. Ezen a ponton előfordulhat, hogy a helyi Hyper-V toomake meg arról, hogy hello teljesen elindítja a virtuális gép toocreate egy virtuális Gépet szeretne, és ellenőrizze, hogy elérhető-e az RDP-e.

9. Távolítsa el a felesleges Transport Driver Interface szűrőket, például szoftver, amely elemzi a TCP csomagokat, vagy további tűzfalakat. Emellett áttekintheti a később a után hello VM a rendszer telepíti az Azure-ban, ha szükséges.

10. Távolítsa el a harmadik féltől származó szoftverek és illesztőprogramot, amely a kapcsolódó toophysical összetevők vagy a más virtualizációs technológiát.

### <a name="install-windows-updates"></a>Windows-frissítések telepítése
hello ideális beállítás túl**hello gépen legújabb hello: hello javítás szintű**. Ha ez nem lehetséges, győződjön meg arról, hogy a következő frissítések hello telepítve vannak:

|                       |                   |           |                                       Minimális verzió x64       |                                      |                                      |                            |
|-------------------------|-------------------|------------------------------------|---------------------------------------------|--------------------------------------|--------------------------------------|----------------------------|
| Összetevő               | Bináris            | Windows 7 és Windows Server 2008 R2 | Windows 8 és Windows Server 2012-ben             | Windows 8.1 és Windows Server 2012 R2 rendszerben | Windows 10 és Windows Server 2016 RS1 | Windows 10 RS2             |
| Storage                 | a Disk.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061           | -                                    | -                          |
|                         | Storport.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.332             |
|                         | NTFS.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17623 / 6.2.9200.21743 - KB3121255 | 6.3.9600.18654 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.447             |
|                         | Iologmsg.dll      | 6.1.7601.23403 - KB3125574         | 6.2.9200.16384 - KB2995387                  | -                                    | -                                    | -                          |
|                         | Classpnp.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17061 / 6.2.9200.21180 - KB2995387 | 6.3.9600.18334 - KB3172614           | 10.0.14393.953 - KB4022715           | -                          |
|                         | Volsnap.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17047 / 6.2.9200.21165 - KB2975331 | 6.3.9600.18265 - KB3145384           | -                                    | 10.0.15063.0               |
|                         | PartMgr.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.16681 - KB2877114                  | 6.3.9600.17401 - KB3000850           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | volmgr.sys        |                                    |                                             |                                      |                                      | 10.0.15063.0               |
|                         | Volmgrx.sys       | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | 10.0.15063.0               |
|                         | Msiscsi.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.21006 - KB2955163                  | 6.3.9600.18624 - KB4022726           | 10.0.14393.1066 - KB4022715          | 10.0.15063.447             |
|                         | Msdsm.sys         | 6.1.7601.23403 - KB3125574         | 6.2.9200.21474 - KB3046101                  | 6.3.9600.18592 - KB4022726           | -                                    | -                          |
|                         | MPIO.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.21190 - KB3046101                  | 6.3.9600.18616 - KB4022726           | 10.0.14393.1198 - KB4022715          | -                          |
|                         | Fveapi.dll        | 6.1.7601.23311 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.18294 - KB3172614           | 10.0.14393.576 - KB4022715           | -                          |
|                         | Fveapibase.dll    | 6.1.7601.23403 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.17415 - KB3172614           | 10.0.14393.206 - KB4022715           | -                          |
| Network (Hálózat)                 | netvsc.sys        | -                                  | -                                           | -                                    | 10.0.14393.1198 - KB4022715          | 10.0.15063.250 - KB4020001 |
|                         | mrxsmb10.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.22108 - KB4022724                  | 6.3.9600.18603 - KB4022726           | 10.0.14393.479 - KB4022715           | 10.0.15063.483             |
|                         | mrxsmb20.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.21548 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.483             |
|                         | Mrxsmb.sys        | 6.1.7601.23816 - KB4022722         | 6.2.9200.22074 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | Tcpip.sys         | 6.1.7601.23761 - KB4022722         | 6.2.9200.22070 - KB4022724                  | 6.3.9600.18478 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.447             |
|                         | a HTTP.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17285 - KB3042553                  | 6.3.9600.18574 - KB4022726           | 10.0.14393.251 - KB4022715           | 10.0.15063.483             |
|                         | vmswitch.sys      | 6.1.7601.23727 - KB4022719         | 6.2.9200.22117 - KB4022724                  | 6.3.9600.18654 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.138             |
| Mag                    | Ntoskrnl.exe      | 6.1.7601.23807 - KB4022719         | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.483             |
| Távoli asztali szolgáltatások | rdpcorets.dll     | 6.2.9200.21506 - KB4022719         | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.0               |
|                         | TERMSRV.dll       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850           | 10.0.14393.0 - KB4022715             | 10.0.15063.0               |
|                         | Termdd.sys        | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Win32k.sys        | 6.1.7601.23807 - KB4022719         | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726           | 10.0.14393.594 - KB4022715           | -                          |
|                         | rdpdd.dll         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Rdpwd.sys         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
| Biztonság                | Esedékes tooWannaCrypt | KB4012212                          | KB4012213                                   | KB4012213                            | KB4012606                            | KB4012606                  |
|                         |                   |                                    | KB4012216                                   |                                      | KB4013198                            | KB4013198                  |
|                         |                   | KB4012215                          | KB4012214                                   | KB4012216                            | KB4013429                            | KB4013429                  |
|                         |                   |                                    | KB4012217                                   |                                      | KB4013429                            | KB4013429                  |
       
### Amikor toouse sysprep<a id="step23"></a>    

A Sysprep egy folyamat, amely alaphelyzetbe állítja a hello rendszer hello telepítését, és a "hello kezdőélmény" eltávolíthatja személyes adatokat, és alaphelyzetbe állításával több összetevőt biztosít windows-telepítést történő futtatásával. Általában ehhez Ha azt szeretné, hogy toocreate egy sablont, ahol központilag telepíthető a többi virtuális adott konfigurációval rendelkezik. Ez a lehetőség egy **kép általánosítva**.

Ha ehelyett azt szeretné, hogy csak toocreate egy virtuális gép egy lemezről toouse sysprep nincs. Ebben az esetben csak a virtuális gép is ismert, hello hozhat létre egy **speciális kép**.

Hogyan toocreate egy virtuális Gépet egy speciális lemezből: további információk:

- [Virtuális gép létrehozása egy speciális lemezről](create-vm-specialized.md)
- [Hozzon létre egy virtuális Gépet egy speciális VHD lemezről](https://azure.microsoft.com/resources/templates/201-vm-specialized-vhd/)

Ha azt szeretné, hogy általános lemezkép toocreate, toorun sysprep szüksége. További információ a Sysprep: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx). 

Nem minden szerepkör vagy a Windows-alapú számítógépen telepített alkalmazás támogatja-e általánosítása. Adott számítógép hello szerepköréhez, mielőtt elkezdi a műveletet, olvassa el, hogy a következő cikk toomake toohello sysprep támogatja. További információ [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="steps-toogeneralize-a-vhd"></a>Virtuális merevlemez lépéseket toogeneralize

>[!NOTE]
> Miután lefuttatta a sysprep.exe, a következő lépéseket a hello, kapcsolja ki a virtuális gép hello, és nem kapcsolja vissza addig, amíg az Azure-ban, a lemezkép létrehozása..

1. Toohello Windows Virtuálisgép jelentkezni.
2. Futtatás **parancssor** rendszergazdaként. 
3. Hello könyvtárváltás: **%windir%\system32\sysprep**, majd futtassa a **sysprep.exe**.
3. A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy hello **Generalize** jelölőnégyzet be van jelölve.

    ![Rendszer-előkészítő eszköz](media/prepare-for-upload-vhd-image/syspre.png)
4. A **leállítási beállítások**, jelölje be **leállítási**.
5. Kattintson az **OK** gombra.
6. Sysprep befejeződött, állítsa le a virtuális gép hello. Ne használjon **indítsa újra a** tooshut hello VM le.
7. Most már hello VHD áll készen toobe feltöltve. További információ a hogyan toocreate egy virtuális Gépet egy általánosított lemezből: [egy általánosított virtuális merevlemez feltöltéséhez, és használja azt az Azure-ban toocreate egy új virtuális gépek](sa-upload-generalized.md).


## <a name="complete-recommended-configurations"></a>Adja meg az ajánlott beállításait
hello a következő beállítások nem befolyásolják a virtuális merevlemez feltöltése. Azonban javasoljuk, hogy azokat konfigurálta.

* Telepítse a hello [Azure virtuális gépek ügynök](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Ezt követően engedélyezheti a Virtuálisgép-bővítmények. Virtuálisgép-bővítmények hello valósítja meg a legtöbb hello kritikus funkcióit, hogy előfordulhat, hogy szeretné, hogy a virtuális gépek például a jelszavak, alaphelyzetbe állítását RDP konfigurálása toouse, és így tovább. További információkért lásd:

    - [Ügynök és Virtuálisgép-bővítmények – 1. rész](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-1/)
    - [Ügynök és Virtuálisgép-bővítmények – 2. rész](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)
* hello memóriakép napló Windows összeomlási kapcsolatos problémák elhárításához hasznos lehet. Hello memóriakép naplógyűjtést engedélyezése:
  
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "AutoReboot" -Value 0 -Type DWord
    New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
    Ha hiba az hello eljárási lépések ebben a cikkben, ez azt jelenti, hogy hello beállításkulcsok már létezik. Ebben a helyzetben használja inkább a következő parancsok hello:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
*  Az Azure-ban hello virtuális gép létrehozása után ajánlott hello lapozófájl elhelyezése hello "Historikus meghajtó" tooimprove teljesítménye. Állíthat be a következőképpen:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -name "PagingFiles" -Value "D:\pagefile"
    ```
Ha bármely, amely csatolt toohello VM adatlemez, hello Historikus meghajtó kötet meghajtójának betűjelét általában-e "D" Lehet, hogy a kijelölés különböző, attól függően, hogy az elérhető meghajtók és hello-beállítások hello száma.

## <a name="next-steps"></a>Következő lépések
* [A Windows virtuális gép lemezképének tooAzure a Resource Manager üzembe helyezések feltöltése](upload-generalized-managed.md)

