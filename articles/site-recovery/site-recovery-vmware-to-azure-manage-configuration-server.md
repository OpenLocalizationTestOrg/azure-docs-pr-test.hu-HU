---
title: " Az Azure Site Recovery konfigurációs kiszolgáló kezelése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset és kezelheti a konfigurációs kiszolgálót."
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
ms.openlocfilehash: 2852bcd25409121be46a1ebf135ebfcdce6e5de5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-configuration-server"></a>A konfigurációs kiszolgáló kezelése

Konfigurációs kiszolgáló között hello Site Recovery services és a helyszíni infrastruktúra koordinátora működik. Ez a cikk ismerteti, hogyan állítsa be, konfigurálása és kezelése hello konfigurációs kiszolgáló.

## <a name="prerequisites"></a>Előfeltételek
hello az alábbiakban hello minimális hardver-, szoftver, és hálózati konfiguráció szükséges tooset konfigurációs kiszolgáló.

> [!NOTE]
> [Kapacitástervezés](site-recovery-capacity-planner.md) van egy fontos lépés tooensure központi telepítését hello konfigurációs kiszolgáló konfigurálása, hogy a csomagok a terhelési követelményeknek. Tudjon meg többet az [méretezése a konfigurációs kiszolgáló követelményei](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-configuration-server-software"></a>Hello konfigurációs kiszolgáló szoftver letöltése
1. Jelentkezzen be toohello az Azure portálon, és keresse meg tooyour Recovery Services-tároló.
2. Keresse meg a túl**Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** (alatt a VMware és fizikai gépek).

  ![Kiszolgálók lap hozzáadása](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. Kattintson a hello **+ kiszolgálók** gombra.
4. A hello **kiszolgáló hozzáadása** lapján kattintson a hello Letöltés gomb toodownload hello regisztrációs kulcsot. Ezt a kulcsot kell hello konfigurációs kiszolgáló telepítési tooregister során az Azure Site Recovery szolgáltatásban.
5. Kattintson a hello **letöltési hello Microsoft Azure Site Recovery az egységes telepítő** hivatkozás toodownload hello legújabb verziójának hello konfigurációs kiszolgáló.

  ![Letöltési oldala](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  Konfigurációs kiszolgáló hello legújabb verziója letölthető közvetlenül [Microsoft Download Center letöltési oldala](http://aka.ms/unifiedsetup)

## <a name="installing-and-registering-a-configuration-server-from-gui"></a>Telepítése és regisztrálása a konfigurációs kiszolgáló grafikus felhasználói felület
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a>Telepítése és regisztrálása a konfigurációs kiszolgáló parancssor használatával

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
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
MySQLCredsFilePath paraméter egy fájl fogadja bemeneti adatként. Használja a következő formátumú, és adja át bemeneti MySQLCredsFilePath paraméterként hello hello fájl létrehozásához.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a>Proxykiszolgáló-beállítások konfigurációs fájl létrehozása
ProxySettingsFilePath paraméter egy fájl fogadja bemeneti adatként. Használja a következő formátumú, és adja át bemeneti ProxySettingsFilePath paraméterként hello hello fájl létrehozásához.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a>Konfigurációs kiszolgáló proxy beállításainak módosítása
1. Bejelentkezési tooyour konfigurációs kiszolgáló.
2. Indítsa el a hello parancsikon használatával hello cspsconfigtool.exe a.
3. Kattintson a hello **tároló regisztrációs** fülre.
4. Töltsön le egy új tároló regisztrációs fájlt hello portálról, és adja meg a bemeneti toohello eszközként.

  ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. Adja meg hello új proxykiszolgáló adatait, majd kattintson a hello **regisztrálása** gombra.
6. Nyisson meg egy rendszergazda PowerShell parancsablakot.
7. Futtassa a következő parancs hello
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Ha kibővített folyamat kiszolgálók csatolt toothis konfigurációs kiszolgáló, túl kell[hello proxybeállításai hárítsa el az összes hello kibővített folyamat kiszolgálók](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) a környezetben.

## <a name="re-register-a-configuration-server-with-hello-same-recovery-services-vault"></a>Regisztrálja újra a konfigurációs kiszolgáló hello azonos Recovery Services-tároló
  1. Bejelentkezési tooyour konfigurációs kiszolgáló.
  2. Indítsa el a hello cspsconfigtool.exe használatával hello parancsikont az asztalon.
  3. Kattintson a hello **tároló regisztrációs** fülre.
  4. Töltsön le új regisztrációs fájl hello portálról, és adja meg a bemeneti toohello eszközként.
        ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
  5. Hello proxykiszolgáló részletesen, és kattintson a hello **regisztrálása** gombra.  
  6. Nyisson meg egy rendszergazda PowerShell parancsablakot.
  7. Futtassa a következő parancs hello

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  Ha kibővített folyamat kiszolgálók csatolt toothis konfigurációs kiszolgáló, túl kell[regisztrálja újra az összes hello kibővített folyamat kiszolgálók](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) a környezetben.

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a>A konfigurációs kiszolgáló regisztrálása egy másik Recovery Services-tároló.
1. Bejelentkezési tooyour konfigurációs kiszolgáló.
2. egy rendszergazdai parancssorból futtassa a hello parancsot

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. Indítsa el a hello parancsikon használatával hello cspsconfigtool.exe a.
4. Kattintson a hello **tároló regisztrációs** fülre.
5. Töltsön le új regisztrációs fájl hello portálról, és adja meg a bemeneti toohello eszközként.

    ![register-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. Hello proxykiszolgáló részletesen, és kattintson a hello **regisztrálása** gombra.  
7. Nyisson meg egy rendszergazda PowerShell parancsablakot.
8. Futtassa a következő parancs hello
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a>Konfigurációs kiszolgáló leszerelése
Hello következő győződjön meg arról, a konfigurációs kiszolgáló leszerelése megkezdése előtt.
1. Tiltsa le a védelmet az összes virtuális gép ezen a konfigurációs kiszolgálón.
2. Szüntesse meg a konfigurációs kiszolgáló hello összes replikációs házirendet.
3. Törölje az összes Vcenter kiszolgáló vagy vSphere-gazdagépek társított toohello konfigurációs kiszolgáló.

### <a name="delete-hello-configuration-server-from-azure-portal"></a>Törölje a hello konfigurációs kiszolgáló Azure-portálon
1. Azure-portálon Tallózás túl**Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** hello tároló menüből.
2. Kattintson a konfigurációs kiszolgáló, amelyet az toodecommission hello.
3. Hello konfigurációs kiszolgáló adatai lapon kattintson a hello Törlés gomb.

  ![törlés-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. Kattintson a **Igen** hello server tooconfirm hello törlését.

  >[!WARNING]
  Ha a virtuális gép, replikációs házirendek vagy vCenter-kiszolgáló vagy vSphere gazdagépek konfigurációs kiszolgálóhoz társított hello kiszolgáló nem törölhető. Törölje ezeket az entitásokat előtt toodelete hello tárolóban.

### <a name="uninstall-hello-configuration-server-software-and-its-dependencies"></a>Távolítsa el a hello konfigurációs kiszolgálók és a függőségek
  > [!TIP]
  Ha újra tooreuse hello konfigurációs kiszolgáló Azure Site Recovery szolgáltatással, majd továbbléphet toostep 4 közvetlenül

1. Jelentkezzen be a konfigurációs kiszolgáló toohello rendszergazdaként.
2. Nyissa meg Vezérlőpult > Program > Programok eltávolítása
3. Távolítsa el a következő feladatütemezési hello hello programok:
  * Microsoft Azure Recovery Services Agent
  * Microsoft Azure Site helyreállítási mobilitási szolgáltatás/fő célkiszolgáló megadásával
  * A Microsoft Azure Site Recovery Providert
  * Microsoft Azure Recovery konfigurációs kiszolgáló/folyamat helykiszolgáló
  * A Microsoft Azure Site helyreállítási konfigurációs kiszolgáló függőségek
  * MySQL Server 5.5
4. A következő parancsot a hello és rendszergazdai parancssorból futtatható.
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a>Konfigurációs kiszolgáló biztonságos szoftvercsatorna Layer(SSL) tanúsítványainak megújítása
Konfigurációs kiszolgáló hello egy beépített webkiszolgálót, amely koordinálja a hello mobilitási szolgáltatás folyamat kiszolgálók hello tevékenységeit és a fő célkiszolgáló kiszolgálók csatlakoztatva toohello konfigurációs kiszolgáló. hello konfigurációs kiszolgáló webkiszolgálót használ egy SSL-tanúsítvány tooauthenticate az ügyfeleknek. Ez a tanúsítvány rendelkezik egy három év lejárta, és a következő metódus hello segítségével lehet megújítani:

> [!WARNING]
Tanúsítványlejárat csak verziót lehetne végrehajtani 9.4.XXXX. X vagy újabb verzióját. Frissítse a hello Azure Site Recovery minden összetevőjét (konfigurációs kiszolgáló, a Folyamatkiszolgáló, fő célkiszolgáló, Mobilitásiszolgáltatás) hello tanúsítványok megújítása munkafolyamat megkezdése előtt.

1. A hello Azure-portálon, keresse meg a tároló tooyour > Site Recovery-infrastruktúra > konfigurációs kiszolgáló.
2. Kattintson a hello konfigurációs kiszolgáló, amelyhez toorenew kell hello SSL-tanúsítványa.
3. A konfigurációs kiszolgáló állapotának hello lásd: hello lejárati dátumának hello SSL-tanúsítvány.
4. Megújítás hello tanúsítvány hello kattintva **tanúsítványok megújítása** művelet látható hello kép a következő módon:

  ![törlés-konfiguráció-kiszolgáló](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a>Secure Socket Layer tanúsítvány lejáratával kapcsolatos figyelmeztetés

> [!NOTE]
hello SSL-tanúsítvány érvényességi 2016. május előtt történt összes telepítések be lett állítva tooone év. elindítását követően megtekintheti a tanúsítványok lejárati értesítéseinek hello Azure-portálon jelenik meg.

1. Ha hello konfigurációs kiszolgáló SSL-tanúsítvány tooexpire hello a következő két hónapon, hello szolgáltatás elindul, értesítési hello Azure-portálon & e-mailek (kell előfizetett toobe tooAzure Site Recovery-értesítések). Amikor megkezdi a értesítésszalagról hello tároló-erőforrás lapon látható.

  ![tanúsítvány-értesítés](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. Hello szalagcím tooget további részletek a hello tanúsítványlejárat gombra.

  ![tanúsítvány-részletek](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  Ahelyett, hogy ha egy **megújítása most** gomb megjelenik egy **frissítés most** gombra. Ez azt jelenti, hogy vannak-e néhány összetevőt a környezetben, amelyek még nincsenek frissített too9.4.xxxx.x vagy újabb verzió.

## <a name="sizing-requirements-for-a-configuration-server"></a>A konfigurációs kiszolgáló méretezése követelményei

| **PROCESSZOR** | **Memória** | **Gyorsítótár-lemez mérete** | **Adatváltozási sebesség** | **Védett gépek** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag) |16 GB |300 GB |500 GB vagy kevesebb |100-nál kevesebb gépek replikálása. |
| 12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) |18 GB |600 GB |500 GB too1 TB |100-150 gépek közti replikálásához. |
| 16 Vcpu (2 sockets * @ 2,5 GHz-es 8 mag) |32 GB |1 TB |1 TB-os too2 TB |150-200 gépek közti replikálásához. |

  >[!TIP]
  Ha a napi adatforgalommal meghaladja a 2 TB, vagy több mint 200 olyan virtuális gépet tooreplicate tervezi, ajánlott toodeploy további folyamat kiszolgálók tooload egyenleg hello replikációs forgalmat. További tudnivalók hogyan toodeploy kibővített folyamat kiszolgálója.


## <a name="common-issues"></a>Gyakori problémák
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
