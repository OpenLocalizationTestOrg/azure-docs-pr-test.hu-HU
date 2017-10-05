---
title: " Az Azure Site Recovery konfigurációs kiszolgáló kezelése |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan kezelheti a konfigurációs kiszolgáló."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 36da8c7d0f3ace194522e5288f26069cf46d470e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-configuration-server"></a>A konfigurációs kiszolgáló kezelése

Konfigurációs kiszolgáló között a Site Recovery services és a helyszíni infrastruktúra koordinátora működik. Ez a cikk ismerteti, hogyan állítsa be, konfigurálhatja és kezelheti a konfigurációs kiszolgáló.

## <a name="prerequisites"></a>Előfeltételek
A minimális hardver-, szoftver, és a konfigurációs kiszolgáló beállításához szükséges hálózati konfiguráció az alábbiakban.

> [!NOTE]
> [Kapacitástervezés](site-recovery-capacity-planner.md) egy fontos lépés annak érdekében, hogy telepít a konfigurációs kiszolgáló konfigurálása, hogy a csomagok a terhelési követelményeknek. Tudjon meg többet az [méretezése a konfigurációs kiszolgáló követelményei](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-configuration-server-software"></a>A konfigurációs kiszolgáló szoftver letöltése
1. Jelentkezzen be az Azure-portálon, és keresse meg a Recovery Services-tároló.
2. Keresse meg a **Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** (a VMware és fizikai gépek).

  ![Kiszolgálók lap hozzáadása](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. Kattintson a **+ kiszolgálók** gombra.
4. Az a **kiszolgáló hozzáadása** lapon, a Letöltés gombra kattintva töltse le a regisztrációs kulcsot. A konfigurációs kiszolgáló telepítése az Azure Site Recovery szolgáltatásban való regisztrálása során kell ezt a kulcsot.
5. Kattintson a **töltse le a Microsoft Azure Site Recovery az egységes telepítő** hivatkozást a konfigurációs kiszolgáló legújabb verziójának letöltéséhez.

  ![Letöltési oldala](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  A konfigurációs kiszolgáló legújabb verziója letölthető közvetlenül [Microsoft Download Center letöltési oldala](http://aka.ms/unifiedsetup)

## <a name="installing-and-registering-a-configuration-server-from-gui"></a>Telepítése és regisztrálása a konfigurációs kiszolgáló grafikus felhasználói felület
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a>Telepítése és regisztrálása a konfigurációs kiszolgáló parancssor használatával

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Példa
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a>Konfigurációs kiszolgáló telepítő parancssori argumentumokat.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a>Egy MySql hitelesítőadat-fájlt létrehozni
MySQLCredsFilePath paraméter egy fájl fogadja bemeneti adatként. Hozza létre a fájlt a következő formátumban, és adja át bemeneti MySQLCredsFilePath paraméterként.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a>Proxykiszolgáló-beállítások konfigurációs fájl létrehozása
ProxySettingsFilePath paraméter egy fájl fogadja bemeneti adatként. Hozza létre a fájlt a következő formátumban, és adja át bemeneti ProxySettingsFilePath paraméterként.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a>Konfigurációs kiszolgáló proxy beállításainak módosítása
1. A bejelentkezési és a konfigurációs kiszolgáló.
2. Indítsa el a használatával a helyi cspsconfigtool.exe a.
3. Kattintson a **tároló regisztrációs** fülre.
4. Töltsön le egy új tároló regisztrációs fájlt a portálról, és adja meg az eszköz számára.

  ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. Adja meg az új proxykiszolgáló adatait, és kattintson a **regisztrálása** gombra.
6. Nyisson meg egy rendszergazda PowerShell parancsablakot.
7. A következő parancsot
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Ha a konfigurációs kiszolgálóhoz kapcsolódó kibővített folyamat kiszolgálóval rendelkezik, akkor [hárítsa el a proxykiszolgáló beállításait a kibővített folyamat összes kiszolgálójára](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) a környezetben.

## <a name="re-register-a-configuration-server-with-the-same-recovery-services-vault"></a>Regisztrálja újra a konfigurációs kiszolgáló a azonos Recovery Services-tároló
  1. A bejelentkezési és a konfigurációs kiszolgáló.
  2. Indítsa el a cspsconfigtool.exe használatával a parancsikont az asztalon.
  3. Kattintson a **tároló regisztrációs** fülre.
  4. Töltsön le új regisztrációs fájlt a portálról, és adja meg az eszköz számára.
        ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
  5. Adja meg a proxykiszolgáló adatait, és kattintson a **regisztrálása** gombra.  
  6. Nyisson meg egy rendszergazda PowerShell parancsablakot.
  7. A következő parancsot

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  Ha a konfigurációs kiszolgálóhoz kapcsolódó kibővített folyamat kiszolgálóval rendelkezik, akkor [regisztrálja újra a kibővített folyamat kiszolgálók](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) a környezetben.

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a>A konfigurációs kiszolgáló regisztrálása egy másik Recovery Services-tároló.
1. A bejelentkezési és a konfigurációs kiszolgáló.
2. Futtassa a parancsot egy rendszergazdai parancssorból

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. Indítsa el a használatával a helyi cspsconfigtool.exe a.
4. Kattintson a **tároló regisztrációs** fülre.
5. Töltsön le új regisztrációs fájlt a portálról, és adja meg az eszköz számára.

    ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. Adja meg a proxykiszolgáló adatait, és kattintson a **regisztrálása** gombra.  
7. Nyisson meg egy rendszergazda PowerShell parancsablakot.
8. A következő parancsot
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a>Konfigurációs kiszolgáló leszerelése
Ellenőrizze a konfigurációs kiszolgáló leszerelése megkezdése előtt.
1. Tiltsa le a védelmet az összes virtuális gép ezen a konfigurációs kiszolgálón.
2. Szüntesse meg az összes replikációs házirendek a konfigurációs kiszolgálóról.
3. Törli a Vcenter kiszolgáló vagy vSphere minden gazdagép tartozó és a konfigurációs kiszolgáló.

### <a name="delete-the-configuration-server-from-azure-portal"></a>Törölje a konfigurációs kiszolgáló Azure-portálon
1. Azure-portálon keresse meg a **Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** a tároló menüből.
2. Kattintson a kiszolgáló leszerelése kívánt.
3. A konfigurációs kiszolgáló adatai lapon kattintson a Törlés gomb.

  ![törlés-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. Kattintson a **Igen** annak a kiszolgálónak a törlés megerősítéséhez.

  >[!WARNING]
  Ha a virtuális gépek, replikációs házirendek vagy konfigurációs kiszolgálóhoz társított vCenter-kiszolgáló vagy vSphere gazdagépek, a kiszolgáló nem törölhető. Törölje ezeket az entitásokat, mielőtt törli a tárolót.

### <a name="uninstall-the-configuration-server-software-and-its-dependencies"></a>Távolítsa el a konfigurációs kiszolgáló szoftver és annak függőségeit
  > [!TIP]
  Ha szeretne újra felhasználhatja a konfigurációs kiszolgáló Azure Site Recovery szolgáltatással, majd továbbléphet a 4. lépés közvetlenül

1. Jelentkezzen be rendszergazdaként a konfigurációs kiszolgáló.
2. Nyissa meg Vezérlőpult > Program > Programok eltávolítása
3. Távolítsa el a programokat, az alábbi sorrendben:
  * Microsoft Azure Recovery Services Agent
  * Microsoft Azure Site helyreállítási mobilitási szolgáltatás/fő célkiszolgáló megadásával
  * A Microsoft Azure Site Recovery Providert
  * Microsoft Azure Recovery konfigurációs kiszolgáló/folyamat helykiszolgáló
  * A Microsoft Azure Site helyreállítási konfigurációs kiszolgáló függőségek
  * MySQL Server 5.5
4. Futtassa a következő parancsot a és a rendszergazdai parancssorba.
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a>Konfigurációs kiszolgáló biztonságos szoftvercsatorna Layer(SSL) tanúsítványainak megújítása
A kiszolgáló rendelkezik egy beépített webkiszolgálót, amely a mobilitási szolgáltatás, a folyamat kiszolgálók és a fő célkiszolgáló kiszolgálók, a konfigurációs kiszolgálóhoz kapcsolódó tevékenységeket koordinálja. A konfigurációs kiszolgáló webkiszolgáló SSL-tanúsítványt használ az ügyfelek hitelesítéséhez. Ez a tanúsítvány rendelkezik egy három év lejárta, és az alábbi módszerrel bármikor újítható:

> [!WARNING]
Tanúsítványlejárat csak verziót lehetne végrehajtani 9.4.XXXX. X vagy újabb verzióját. Az Azure Site Recovery minden összetevőjét (konfigurációs kiszolgáló, a Folyamatkiszolgáló, fő célkiszolgáló, mobilitási szolgáltatás) frissítése a tanúsítványok megújítása munkafolyamat megkezdése előtt.

1. Az Azure-portálon keresse meg a tároló > Site Recovery-infrastruktúra > konfigurációs kiszolgáló.
2. Kattintson a konfigurációs kiszolgáló, amelyhez újítsa meg az SSL-tanúsítványt kell.
3. A konfigurációs kiszolgáló állapota tekintse meg a lejárat dátumát az SSL-tanúsítvány.
4. A tanúsítvány megújításához kattintson a **tanúsítványok megújítása** művelet a következő ábrán látható módon:

  ![törlés-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a>Secure Socket Layer tanúsítvány lejáratával kapcsolatos figyelmeztetés

> [!NOTE]
Az összes telepítés előtt 2016. május történt az SSL-tanúsítvány érvényességi egy évre lett beállítva. már elindította az Azure-portálon jelenik meg a tanúsítványok lejárati értesítéseinek jelent.

1. Ha a konfigurációs kiszolgáló SSL-tanúsítvány a következő két hónapon belül lejár, a szolgáltatás elindul, értesíti a felhasználókat az Azure-portálon & e-mailek (kell lehet feliratkozni az Azure Site Recovery értesítésekre). A tároló-erőforrás oldalon értesítésszalagról kezdenek el azt.

  ![tanúsítvány-értesítés](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. Kattintson a szalagcím, és kérjen további részleteket a tanúsítvány lejárta.

  ![tanúsítvány-részletek](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  Ahelyett, hogy ha egy **megújítása most** gomb megjelenik egy **frissítés most** gombra. Ez azt jelenti, hogy vannak-e néhány összetevőt a környezetben, amelyek még nincsenek frissítve az 9.4.xxxx.x vagy újabb verzió.

## <a name="sizing-requirements-for-a-configuration-server"></a>A konfigurációs kiszolgáló méretezése követelményei

| **PROCESSZOR** | **Memória** | **Gyorsítótár-lemez mérete** | **Adatváltozási sebesség** | **Védett gépek** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag) |16 GB |300 GB |500 GB vagy kevesebb |100-nál kevesebb gépek replikálása. |
| 12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) |18 GB |600 GB |1 TB 500 GB |100-150 gépek közti replikálásához. |
| 16 Vcpu (2 sockets * @ 2,5 GHz-es 8 mag) |32 GB |1 TB |1 TB-os és 2 TB |150-200 gépek közti replikálásához. |

  >[!TIP]
  Ha a napi adatforgalommal meghaladja a 2 TB, vagy több mint 200 olyan virtuális gépet replikálni tervezi, javasoljuk, hogy a replikációs forgalom további folyamat-kiszolgálót telepíthet. Ismerje meg, további kibővített folyamat telepítésével kapcsolatos kiszolgálója.


## <a name="common-issues"></a>Gyakori problémák
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
