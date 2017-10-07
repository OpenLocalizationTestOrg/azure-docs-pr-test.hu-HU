---
title: "az IIS és a tábla tárolás az Azure Naplóelemzés eseményeket aaaUse blob-tároló |} Microsoft Docs"
description: "A Naplóelemzési hello naplók az Azure-szolgáltatásokat, hogy az írási diagnosztika tootable tárolási vagy az IIS-napló írása tooblob tárolási olvashatja."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff3de04dc8cb6729c1443372ec31a0e8dc47f273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a>Az IIS és az Azure Naplóelemzés eseményeit a table storage Azure blob storage használata

A Naplóelemzési hello naplókat, hogy az írási tootable diagnosztika tároló- és IIS naplózza az írásbeli tooblob tárolási szolgáltatások a következő hello olvashatja:

* A Service Fabric-fürtök (előzetes verzió)
* Virtuális gépek
* Webes vagy feldolgozói szerepkörök

A Naplóelemzési ezekhez az erőforrásokhoz tartozó adatokat gyűjthet, mielőtt Azure diagnostics engedélyezve kell lennie.

Ha engedélyezve vannak a diagnosztika, hello Azure-portálon is használhatja, vagy PowerShell Naplóelemzési toocollect hello naplók konfigurálása.

Az Azure Diagnostics egy Azure-bővítményt, amely lehetővé teszi a feldolgozói szerepkör, a webes szerepkör vagy az Azure-beli virtuális gép diagnosztikai adatait toocollect. hello adatok Azure storage-fiók tárolva van, és majd gyűjthetők a Naplóelemzési.

A Naplóelemzési toocollect a Azure diagnosztikai naplók hello naplók kell az alábbi helyek hello:

| Napló típusa | Erőforrás típusa | Hely |
| --- | --- | --- |
| IIS-naplók |Virtuális gépek <br> Webes szerepkörök <br> Feldolgozói szerepkörök |üvegvatta-az iis-naplófájlok (Blob-tároló) |
| Rendszernapló |Virtuális gépek |LinuxsyslogVer2v0 (Table Storage) |
| Service Fabric működési események |Service Fabric-csomópontokon |WADServiceFabricSystemEventTable |
| Service Fabric megbízható szereplő események |Service Fabric-csomópontokon |WADServiceFabricReliableActorEventTable |
| Service Fabric megbízható eseményei |Service Fabric-csomópontokon |WADServiceFabricReliableServiceEventTable |
| Windows-Eseménynapló |Service Fabric-csomópontokon <br> Virtuális gépek <br> Webes szerepkörök <br> Feldolgozói szerepkörök |WADWindowsEventLogsTable (Table-tároló) |
| A Windows ETW-naplók |Service Fabric-csomópontokon <br> Virtuális gépek <br> Webes szerepkörök <br> Feldolgozói szerepkörök |WADETWEventTable (Table-tároló) |

> [!NOTE]
> Az Azure-webhelyek IIS-napló jelenleg nem támogatottak.
>
>

A virtuális gépek, lehetősége van hello hello telepítésének [Naplóelemzési ügynök](log-analytics-azure-vm-extension.md) be a virtuális gép tooenable további betekintést nyerjen. Ezenkívül toobeing képes tooanalyze IIS naplókat és az eseménynaplók, végezhet további elemzés, beleértve a konfigurációs változások követését, az SQL értékelése és a frissítések értékelését.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>A virtuális gép Azure diagnostics engedélyezése az eseménynaplót, és az IIS naplózása gyűjtemény
Az Eseménynapló és az IIS naplózása gyűjtemény hello Microsoft Azure portál használatával a következő eljárás tooenable Azure diagnosztika a virtuális gép használata hello.

### <a name="tooenable-azure-diagnostics-in-a-virtual-machine-with-hello-azure-portal"></a>tooenable hello Azure-portál virtuális gépet az Azure diagnostics
1. Hello Virtuálisgép-ügynök telepítése a virtuális gép létrehozásakor. Ha hello virtuális gép már létezik, győződjön meg arról, hogy hello Virtuálisgép-ügynök már telepítve van.

   * A hello Azure-portálon lépjen toohello virtuális gépet, jelölje ki **opcionális konfigurációs**, majd **diagnosztika** és **állapot** túl**a**.

     Befejezett hello VM kiterjesztése hello Azure Diagnostics telepített és futó. A bővítmény felelős a diagnosztikai adatainak összegyűjtése.
2. Engedélyezze a megfigyelést és a meglévő virtuális az eseménynaplózás konfigurálásához. A virtuális gép szintjének hello diagnosztika engedélyezéséhez. tooenable diagnosztika és majd az eseménynaplózás konfigurálásához, hajtsa végre az alábbi lépésekkel hello:

   1. Válassza ki a virtuális gép hello.
   2. Kattintson a **figyelési**.
   3. Kattintson a **diagnosztika**.
   4. Set hello **állapot** túl**ON**.
   5. Válassza ki a megjeleníteni kívánt toocollect diagnosztikai naplófájlok.
   6. Kattintson az **OK** gombra.

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Az IIS-napló és a esemény gyűjteményhez webes szerepkörrel rendelkező Azure diagnostics engedélyezése
Tekintse meg a túl[hogyan tooEnable felhőszolgáltatásban diagnosztika](../cloud-services/cloud-services-dotnet-diagnostics.md) általános lépései az Azure diagnostics engedélyezése. az alábbi utasítások hello ezekkel az adatokkal, és testre szabható Log Analyticshez való használatra.

Az Azure diagnostics engedélyezése:

* IIS-napló hello scheduledTransferPeriod átviteli időközönként átvitt adatainak naplózása alapértelmezés szerint tárolja.
* Alapértelmezés szerint a Windows-Eseménynapló nem kerülnek.

### <a name="tooenable-diagnostics"></a>tooenable diagnosztika
Windows-Eseménynapló tooenable vagy toochange hello scheduledTransferPeriod, hello XML konfigurációs fájl (diagnostics.wadcfg), Azure-diagnosztika konfigurálása, ahogy az [4. lépés: a diagnosztika konfigurációs fájl létrehozása és telepítése hello bővítmény](../cloud-services/cloud-services-dotnet-diagnostics.md)

hello következő konfigurációs szakasz gyűjti az IIS-naplókba és minden alkalmazás hello származó események és a rendszer naplóit:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant tooWeb roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Győződjön meg arról, hogy a ConfigurationSettings határozza meg a storage-fiók, mint például a következő hello:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Hello **AccountName** és **AccountKey** értékek találhatók hello hello tárolási fiók irányítópulton, elérési kulcsok kezelése az Azure portálon. hello protokollja hello kapcsolati karakterláncot kell **https**.

Hello frissített diagnosztikai konfiguráció alkalmazása után tooyour felhőalapú szolgáltatás, és ír diagnosztika tooAzure tárolására, akkor készen áll a tooconfigure Naplóelemzési áll.

## <a name="use-hello-azure-portal-toocollect-logs-from-azure-storage"></a>Hello Azure portál toocollect naplók Azure Storage-ból
Hello Azure portál tooconfigure Naplóelemzési toocollect hello naplók hello Azure-szolgáltatások a következő használható:

* Service Fabric-fürtök
* Virtuális gépek
* Webes vagy feldolgozói szerepkörök

Az Azure-portálon hello keresse meg a tooyour Naplóelemzési munkaterület, és hajtsa végre a következő feladatok hello:

1. Kattintson a *tárfiókok naplók*
2. Kattintson a hello *Hozzáadás* feladat
3. Válassza ki, amely tartalmazza a hello diagnosztikai naplók hello tárfiókot
   * Ez a fiók lehet egy hagyományos tárolási fiók vagy egy Azure Resource Manager storage-fiók
4. Válassza ki a kívánt toocollect naplókat adattípus hello
   * hello választási lehetőségek: IIS-naplóit; Az eseményeket; Syslog (Linux); ETW naplók; Service Fabric-események
5. forrás hello érték automatikusan alapján van feltöltve hello adattípusúra, és nem módosítható
6. Kattintson az OK toosave hello konfigurálása

További tárhely fiókok és az adatok esetében, amelyet az Naplóelemzési toocollect ismételje meg a 2 – 6.

Körülbelül 30 percet, a rendszer hello storage-fiókot Log Analytics képes toosee adatait. Írt adatok toostorage hello konfiguráció alkalmazása után csak akkor jelenik meg. A Naplóelemzési nem hello tárfiók hello már létező adatokat olvasni.

> [!NOTE]
> hello portál nem felel meg a forrás létezik hello tárfiókban hello, vagy ha új adatok írása.
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>A virtuális gép Azure diagnostics engedélyezése az eseménynaplót, és IIS töltsék PowerShell használatával
Használjon hello szükséges lépések [Naplóelemzési konfigurálása tooindex Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) toouse PowerShell tooread az Azure diagnostics tootable tárolási írt.

Azure PowerShell használatával pontosabban megadhatja tooAzure tárolási írt hello események.
További információkért lásd: [diagnosztika engedélyezésével az Azure virtuális gépek](../virtual-machines-dotnet-diagnostics.md).

Engedélyezi, és használja a következő PowerShell-parancsfájl hello Azure diagnostics frissítése.
Ez a parancsfájl egy egyéni naplózási konfiguráció is használható.
Módosítsa a hello parancsfájl tooset hello tárfiókot, a szolgáltatás neve és a virtuális gép neve.
hello parancsfájl parancsmagokat használja a klasszikus virtuális gépekhez.

Tekintse át a következő parancsfájl minta hello, másolja, szükség szerint módosítsa hello minta elmentse egy olyan PowerShell-parancsfájlt, és futtassa a hello parancsfájl.

```
    #Connect tooAzure
    Add-AzureAccount

    # settings toochange:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert tooconfig format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of hello extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Következő lépések
* [Gyűjteni a naplókat és az Azure-szolgáltatások metrikáját](log-analytics-azure-storage.md) támogatott Azure-szolgáltatásokhoz.
* [Megoldások](log-analytics-add-solutions.md) hello adatok tooprovide betekintést.
* [Használja a keresési lekérdezések](log-analytics-log-searches.md) tooanalyze hello adatokat.
