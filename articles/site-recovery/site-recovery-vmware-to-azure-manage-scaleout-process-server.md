---
title: " Az Azure Site Recovery kibővített folyamat kiszolgáló kezelése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset és egy kibővített Folyamatkiszolgáló az Azure Site Recovery kezelése."
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
ms.openlocfilehash: 3d72f9c2c7014a4ff2fa2af168aa55ad1452eae5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-scale-out-process-server"></a>Kibővített folyamat kiszolgáló kezelése

Kibővített folyamat kiszolgáló hello Site Recovery services és a helyszíni infrastruktúra közötti adatátvitel koordinátora működik. Ez a cikk ismerteti, hogyan állítsa be, konfigurálása és kibővített folyamat kiszolgáló kezeléséhez.

## <a name="prerequisites"></a>Előfeltételek
hello az alábbiakban hello hardveres, szoftveres és hálózati konfiguráció szükséges tooset kibővített folyamat kiszolgáló ajánlott.

> [!NOTE]
> [Kapacitástervezés](site-recovery-capacity-planner.md) van egy fontos lépés tooensure központi telepítését hello konfigurálása kibővített Folyamatkiszolgáló, hogy a csomagok a terhelési követelményeknek. Tudjon meg többet az [jellemzők skálázás kibővített folyamat kiszolgáló](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-scale-out-process-server-software"></a>Hello kibővített Folyamatkiszolgáló szoftver letöltése
1. Jelentkezzen be toohello az Azure portálon, és keresse meg tooyour Recovery Services-tároló.
2. Keresse meg a túl**Site Recovery-infrastruktúra** > **konfigurációs kiszolgálók** (alatt a VMware és fizikai gépek).
3. Válassza ki a konfigurációs kiszolgáló toodrill le hello konfigurációs kiszolgáló részletei lapon.
4. Kattintson a hello **+ Folyamatkiszolgáló** gombra.
5. A hello **adja hozzá a folyamatkiszolgáló** lapon jelölje be **helyszíni telepítés kibővített Folyamatkiszolgáló** hello kapcsolót **válassza ki a kívánt toodeploy a folyamatkiszolgáló** legördülő lista.

  ![Kiszolgálók lap hozzáadása](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. Kattintson a hello **letöltési hello Microsoft Azure Site Recovery az egységes telepítő** hivatkozás toodownload hello legújabb verziójának hello kibővített Folyamatkiszolgáló telepítése.

  > [!WARNING]
  hello a kibővített Folyamatkiszolgáló verziójának meg kell egyeznie a környezetben futó hello konfigurációs kiszolgáló verziónál régebbi tooor. Egy egyszerű módon tooensure verziókompatibilitás toouse hello azonos telepítő bit, hogy nemrég használt tooinstall/frissítés a konfigurációs kiszolgáló.

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a>Telepítése és regisztrálása a kibővített folyamat kiszolgáló grafikus felhasználói felület
Ha ki a központi telepítés meghaladja a 200 forrásgépek tooscale rendelkezik, vagy egy teljes napi kavarog egyidejűleg több, mint 2 TB-os aránya, akkor további folyamat kiszolgálók toohandle hello adatforgalma.

Ellenőrizze a hello [vonatkozó javaslatok kiszolgálókhoz folyamat méretezés](#size-recommendations-for-the-process-server), és hajtsa végre az ezen utasításokat tooset hello folyamat kiszolgáló. Hello kiszolgáló telepítése után telepít át forrás gépek toouse azt.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a>Telepítése és regisztrálása a kibővített Folyamatkiszolgáló parancssori használata

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a>Példa
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a>Kibővített Folyamatkiszolgáló telepítő parancssori argumentumokat.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a>Proxykiszolgáló-beállítások konfigurációs fájl létrehozása
ProxySettingsFilePath paraméter egy fájl fogadja bemeneti adatként. Használja a következő formátumú, és adja át bemeneti ProxySettingsFilePath paraméterként hello fájl létrehozásához.
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a>Kibővített Folyamatkiszolgáló proxy beállításainak módosítása
1. Jelentkezzen be a kibővített Folyamatkiszolgálót.
2. Nyisson meg egy rendszergazda PowerShell parancsablakot.
3. Futtassa a következő parancs hello
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. Ezután a toohello könyvtár tallózása **%PROGRAMDATA%\ASR\Agent** és a következő parancs futtatása hello
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a>A kibővített folyamat-kiszolgáló
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* Ezután nyisson meg egy rendszergazdai parancssort.
* Toohello könyvtár tallózása **%PROGRAMDATA%\ASR\Agent** és hello parancs

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a>A kibővített folyamat-kiszolgáló frissítése
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a>Kibővített folyamat kiszolgáló leszerelése
1. Győződjön meg arról, hogy:
  - Konfigurációs kiszolgáló kapcsolat állapota jeleníti meg, mint a **csatlakoztatva** a hello Azure-portálon
  - Folyamat kiszolgáló ennek ellenére toocommunicate hello konfigurációs kiszolgálóval.
2. Jelentkezzen be a folyamatkiszolgáló toohello rendszergazdaként
3. Nyissa meg Vezérlőpult > Program > Programok eltávolítása
4. Távolítsa el a hello programokat hello sorrendben megadva a következő:
  * Microsoft Azure Recovery konfigurációs kiszolgáló/folyamat helykiszolgáló
  * A Microsoft Azure Site helyreállítási konfigurációs kiszolgáló függőségek
  * Microsoft Azure Recovery Services Agent

Hello Folyamatkiszolgáló törlési tooreflect felfelé-too15 percig hello Azure-portálon a is igénybe vehet.

  > [!NOTE]
  Ha hello folyamatkiszolgáló-e a konfigurációs kiszolgáló hello nem toocommunicate (a portál kapcsolati állapota Disconnected), akkor szükséges, hogy toofollow hello következő lépések toopurge azt a konfigurációs kiszolgáló hello.

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a>A konfigurációs kiszolgálóról a leválasztott kibővített folyamat-kiszolgáló regisztrációjának törlése

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a>A kibővített Folyamatkiszolgáló méretezési követelményei

| **További folyamatkiszolgáló** | **Gyorsítótár-lemez mérete** | **Adatváltozási sebesség** | **Védett gépek** |
| --- | --- | --- | --- |
|4 Vcpu (2 sockets * @ 2,5 GHz-es 2 mag), 8 GB-os memória |300 GB |250 GB vagy kevesebb |Kisebb vagy 85 gépek replikálása. |
|8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 12-GB memória |600 GB |250 GB too1 TB |Replikálja a 85-150 gépek között. |
|12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) 24-GB memória |1 TB |1 TB-os too2 TB |150-225 gépek közti replikálásához. |
