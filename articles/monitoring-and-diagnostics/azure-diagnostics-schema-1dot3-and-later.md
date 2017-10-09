---
title: "aaaAzure diagnosztika bővítmény 1.3-as és újabb verziók konfigurációs séma |} Microsoft Docs"
description: "1.3 sémaverzió és az újabb Azure diagnostics szállított hello Microsoft Azure SDK 2.4-es és újabb verziók része."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: robb
ms.openlocfilehash: bd15d3a79ea818fcb3235854717e58d5da36518e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a>Az Azure Diagnostics 1.3 és későbbi konfigurációs séma
> [!NOTE]
> a következő hello Azure Diagnostics bővítmény hello összetevő használt toocollect teljesítményszámlálók és más statisztikáit:
> - Azure-alapú virtuális gépek 
> - Virtual Machine Scale Sets
> - Service Fabric 
> - Cloud Services 
> - Network Security Groups (Hálózati biztonsági csoportok)
> 
> Ezen a lapon csak fontos, ha ilyen szolgáltatást használ.

Ezen a lapon lehet verziók 1.3-as és újabb (az Azure SDK 2.4-es és újabb). Újabb konfigurációs szakaszokat megjegyzésként tooshow addig adták hozzá, milyen verzióban.  

az itt ismertetett hello konfigurációs fájl használt tooset diagnosztikai konfigurációs beállítások esetén hello diagnosztika indítása figyelése.  

hello kiterjesztést más Microsoft-diagnosztika termékek, például Azure figyelő, az Application Insights és Naplóelemzési együtt használja.



Töltse le a hello nyilvános konfigurációs fájl sémadefiníciót a következő hello a következő PowerShell-parancs futtatásával:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

Azure Diagnostics használatával kapcsolatos további információkért lásd: [Azure Diagnostics bővítmény](azure-diagnostics.md).  

## <a name="example-of-hello-diagnostics-configuration-file"></a>Hello diagnosztika konfigurációs fájl példa  
 a következő példa hello mutatja egy tipikus diagnosztika konfigurációs fájlt:  

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
    <WadCfg>  
      <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  

        <PerformanceCounters scheduledTransferPeriod="PT1M">  
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
        </PerformanceCounters>  

        <Directories scheduledTransferPeriod="PT5M">  
          <IISLogs containerName="iislogs" />  
          <FailedRequestLogs containerName="iisfailed" />  

          <DataSources>  
            <DirectoryConfiguration containerName="mynewprocess">  
              <Absolute path="C:\MyNewProcess" expandEnvironment="false" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="badapp">  
              <Absolute path="%SYSTEMDRIVE%\BadApp" expandEnvironment="true" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="goodapp">  
              <LocalResource name="Skippy" relativePath="..\PeanutButter"/>  
            </DirectoryConfiguration>  
          </DataSources>  

        </Directories>  

        <EtwProviders>  
          <EtwEventSourceProviderConfiguration   
                       provider="MyProviderClass"   
                       scheduledTransferPeriod="PT5M">  
            <Event id="0"/>  
            <Event id="1" eventDestination="errorTable"/>  
            <DefaultEvents />  
          </EtwEventSourceProviderConfiguration>  
          <EtwManifestProviderConfiguration provider="5974b00b-84c2-44bc-9e58-3a2451b4e3ad" scheduledTransferLogLevelFilter="Information" scheduledTransferPeriod="PT2M">  
            <Event id="0"/>  
            <DefaultEvents eventDestination="defaultTable"/>  
          </EtwManifestProviderConfiguration>  
        </EtwProviders>  

        <WindowsEventLog scheduledTransferPeriod="PT5M">  
          <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>  
          <DataSource name="System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]" />  
          <DataSource name="System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]" />  
        </WindowsEventLog>  

        <Logs  bufferQuotaInMB="1024"   
             scheduledTransferPeriod="PT1M"   
             scheduledTransferLogLevelFilter="Verbose"   
             sinks="ApplicationInsights.AppLogs"/>  <!-- sinks attribute added in 1.5 -->  

        <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
          <CrashDumpConfiguration processName="mynewprocess.exe" />  
          <CrashDumpConfiguration processName="badapp.exe"/>  
        </CrashDumps>  

        <DockerSources> <!-- Added in 1.9 --> 
          <Stats enabled="true" sampleRate="PT1M" scheduledTransferPeriod="PT1M" />
        </DockerSources>

      </DiagnosticMonitorConfiguration>  

      <SinksConfig>   <!-- Added in 1.5 -->  
        <Sink name="ApplicationInsights">   
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>   
          <Channels>   
            <Channel logLevel="Error" name="Errors"  />   
            <Channel logLevel="Verbose" name="AppLogs"  />   
          </Channels>   
        </Sink>   
        <Sink name="EventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryEventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryStorageAccount"> <!-- Added in 1.7 -->
          <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" />
        </Sink>
   </SinksConfig>

  </WadCfg>  

  <StorageAccount>diagstorageaccount</StorageAccount>
  <StorageType>TableAndBlob</StorageType> <!-- Added in 1.8 -->  
  </PublicConfig>  

  <PrivateConfig>  <!-- Added in 1.3 -->  
    <StorageAccount name="" key="" endpoint="" sasToken="{sas token}"  />  <!-- sasToken in Private config added in 1.8.1 -->  
    <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
   
    <SecondaryStorageAccounts>
       <StorageAccount name="secondarydiagstorageaccount" key="{base64 encoded key}" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
   
    <SecondaryEventHubs>
       <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
    </SecondaryEventHubs>

  </PrivateConfig>  
  <IsEnabled>true</IsEnabled>  
</DiagnosticsConfiguration>  

```  

JSON egyenértékű hello előző XML konfigurációs fájlt. 

hello PublicConfig és a PrivateConfig egymástól, mert az json használati esetek többségében azok átadása pedig különböző változók. Ezekben az esetekben közé tartozik a Resource Manager-sablonok, a virtuálisgép-méretezési csoport PowerShell és a Visual Studio. 

```json
"PublicConfig" {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 10000,
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT1M",
                        "unit": "percent"
                    }
                ]
            },
            "Directories": {
                "scheduledTransferPeriod": "PT5M",
                "IISLogs": {
                    "containerName": "iislogs"
                },
                "FailedRequestLogs": {
                    "containerName": "iisfailed"
                },
                "DataSources": [
                    {
                        "containerName": "mynewprocess",
                        "Absolute": {
                            "path": "C:\\MyNewProcess",
                            "expandEnvironment": false
                        }
                    },
                    {
                        "containerName": "badapp",
                        "Absolute": {
                            "path": "%SYSTEMDRIVE%\\BadApp",
                            "expandEnvironment": true
                        }
                    },
                    {
                        "containerName": "goodapp",
                        "LocalResource": {
                            "relativePath": "..\\PeanutButter",
                            "name": "Skippy"
                        }
                    }
                ]
            },
            "EtwProviders": {
                "sinks": "",
                "EtwEventSourceProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT5M",
                        "provider": "MyProviderClass",
                        "Event": [
                            {
                                "id": 0
                            },
                            {
                                "id": 1,
                                "eventDestination": "errorTable"
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ],
                "EtwManifestProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT2M",
                        "scheduledTransferLogLevelFilter": "Information",
                        "provider": "5974b00b-84c2-44bc-9e58-3a2451b4e3ad",
                        "Event": [
                            {
                                "id": 0
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT5M",
                "DataSource": [
                    {
                        "name": "System!*[System[Provider[@Name='Microsoft Antimalware']]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Verbose",
                "sinks": "ApplicationInsights.AppLogs"
            },
            "CrashDumps": {
                "directoryQuotaPercentage": 30,
                "dumpType": "Mini",
                "containerName": "wad-crashdumps",
                "CrashDumpConfiguration": [
                    {
                        "processName": "mynewprocess.exe"
                    },
                    {
                        "processName": "badapp.exe"
                    }
                ]
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "ApplicationInsights",
                    "ApplicationInsights": "{Insert InstrumentationKey}",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "Errors"
                            },
                            {
                                "logLevel": "Verbose",
                                "name": "AppLogs"
                            }
                        ]
                    }
                },
                {
                    "name": "EventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryEventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryStorageAccount",
                    "StorageAccount": {
                        "name": "secondarydiagstorageaccount",
                        "endpoint": "https://core.windows.net"
                    }
                }
            ]
        }
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```json
"PrivateConfig" {
    "storageAccountName": "diagstorageaccount",
    "storageAccountKey": "{base64 encoded key}",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "EventHub": {
        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    },
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "key": "{base64 encoded key}",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    },
    "SecondaryEventHubs": {
        "EventHub": [
            {
                "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                "SharedAccessKeyName": "SendRule",
                "SharedAccessKey": "{base64 encoded key}"
            }
        ]
    }
}

```

## <a name="reading-this-page"></a>Ez a lap olvasása  
 hello a következő címkék vannak nagyjából példa megelőző hello sorrendben.  Ha nem látja a teljes leírás várt azt, keressen hello lap hello elem vagy attribútum.  

## <a name="common-attribute-types"></a>Közös attribútum típusát  
 **scheduledTransferPeriod** attribútum több elem szerepel. Hello időköz ütemezett átvitelek toostorage felfelé toohello legközelebbi perc között. hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp)


## <a name="diagnosticsconfiguration-element"></a>DiagnosticsConfiguration elem  
 *: Tartománygyökér - DiagnosticsConfiguration*

1.3-as verzió felvételére.  

hello legfelső szintű elem hello diagnosztika konfigurációs fájl.  

**Attribútum** xmlns - hello XML-névtér az hello diagnosztika konfigurációs fájl:  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  


|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**PublicConfig**|Kötelező. Ezen az oldalon máshol lásd a leírást.|  
|**PrivateConfig**|Választható. Ezen az oldalon máshol lásd a leírást.|  
|**IsEnabled**|Logikai érték. Ezen az oldalon máshol lásd a leírást.|  

## <a name="publicconfig-element"></a>PublicConfig elem  
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig*

 Hello nyilvános diagnosztikai konfigurációja ismerteti.  

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**WadCfg**|Kötelező. Ezen az oldalon máshol lásd a leírást.|  
|**StorageAccount**|hello Azure Storage fiók toostore hello adatait a hello neve. Előfordulhat, hogy is meg kell adni egy paraméterként hello Set-AzureServiceDiagnosticsExtension parancsmag végrehajtása közben.|  
|**StorageType**|Lehet *tábla*, *Blob*, vagy *TableAndBlob*. Tábla alapértelmezett beállítás. Ha TableAndBlob van kiválasztva, diagnosztikai adatot ír kétszer – egyszer tooeach típusa.|  
|**LocalResourceDirectory**|hello directory ahol hello Monitoring Agent-tárolók eseményadatokat hello virtuális gépen. Ha nem, állítsa be, hello alapértelmezett címtár szerepel:<br /><br /> A munkavégző vagy webes szerepkör:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> A virtuális gép:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Szükséges attribútumok pedig a következők:<br /><br /> - **elérési út** – hello Azure Diagnostics által használt hello rendszer toobe könyvtárába.<br /><br /> - **expandEnvironment** -szabályozza, hogy a környezeti változók vannak bontva hello elérési útja.|  

## <a name="wadcfg-element"></a>WadCFG elem  
 *Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG*
 
 Azonosítja és hello telemetriai adatok toobe gyűjtött konfigurálja.  


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration elem 
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*

 Szükséges 

|Attribútumok|Leírás|  
|----------------|-----------------|  
| **overallQuotaInMB** | hello maximális memóriamennyiség helyi, amelyeket a hello használni Azure Diagnostics által gyűjtött diagnosztikai adatok különböző típusú. hello alapértéke 5120 MB.<br />
|**useProxyServer** | Ahogyan az Internet Explorer beállításainak Azure Diagnostics toouse hello proxykiszolgáló-beállításainak konfigurálása.|  

<br /> <br />

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**CrashDumps**|Ezen az oldalon máshol lásd a leírást.|  
|**DiagnosticInfrastructureLogs**|Az Azure diagnosztikai által létrehozott naplók gyűjtésének engedélyezése. hello diagnosztikai infrastruktúra naplók hibaelhárítási hello diagnosztika rendszert illeti hasznosak. Nem kötelező attribútumok pedig a következők:<br /><br /> - **scheduledTransferLogLevelFilter** -hello minimális súlyossági szintet az összegyűjtött hello naplók konfigurálja.<br /><br /> - **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**Könyvtárak**|Ezen az oldalon máshol lásd a leírást.|  
|**EtwProviders**|Ezen az oldalon máshol lásd a leírást.|  
|**Metrikák**|Ezen az oldalon máshol lásd a leírást.|  
|**PerformanceCounters**|Ezen az oldalon máshol lásd a leírást.|  
|**WindowsEventLog**|Ezen az oldalon máshol lásd a leírást.| 
|**DockerSources**|Ezen az oldalon máshol lásd a leírást. | 



## <a name="crashdumps-element"></a>CrashDumps elem  
 *Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*
 
 Engedélyezze a összeomlási memóriaképek hello gyűjteménye.  

|Attribútumok|Leírás|  
|----------------|-----------------|  
|**containerName**|Választható. a használt toostore összeomlási memóriaképek hello blob tárolóhoz az Azure Storage-fiók toobe hello neve.|  
|**crashDumpType**|Választható.  Konfigurálja az Azure Diagnostics toocollect mini vagy teljes összeomlási memóriaképeket.|  
|**directoryQuotaPercentage**|Választható.  Konfigurálja a hello százalékos aránya **overallQuotaInMB** lefoglalva a virtuális gép hello összeomlási memóriaképek toobe.|  

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**CrashDumpConfiguration**|Kötelező. Határozza meg a konfigurációs értékeket az összes folyamat.<br /><br /> a következő attribútum hello is szükség:<br /><br /> **Folyamatnév** – hello hello folyamat összeomlási adja az Azure Diagnostics toocollect kívánt nevét.|  

## <a name="directories-element"></a>Könyvtárak elem 
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - könyvtárak*

 Lehetővé teszi, hogy hello egy könyvtár tartalmának hello gyűjteménye, IIS sikertelen volt a hozzáférési kérelem naplók és/vagy az IIS-naplókba.  

 Nem kötelező **scheduledTransferPeriod** attribútum. Tekintse meg a korábban magyarázatot.  

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**IISLogs**|Ez az elem beleértve hello konfigurációban lehetővé teszi, hogy az IIS-naplók hello gyűjtésének:<br /><br /> **containerName** -használt toostore hello IIS naplók az Azure Storage-fiók toobe hello blob tárolóhoz hello neve.|   
|**FailedRequestLogs**|Ez az elem beleértve hello konfigurációban lehetővé teszi, hogy a sikertelen kérelmek tooan IIS webhelyet vagy alkalmazást kapcsolatos információkat naplózza gyűjteménye. Engedélyeznie kell a nyomkövetést **rendszer. Webkiszolgáló** a **Web.config**.|  
|**Adatforrások**|Könyvtárak toomonitor listáját.| 




## <a name="datasources-element"></a>Adatforrások elem  
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - könyvtárak - adatforrás*

 Könyvtárak toomonitor listáját.  

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**DirectoryConfiguration**|Kötelező. Kötelező attribútum:<br /><br /> **containerName** – hello hello blob tároló az adott toobe használt toostore hello naplófájlok az Azure Storage-fiók nevét.|  





## <a name="directoryconfiguration-element"></a>DirectoryConfiguration elem  
 *Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - könyvtárak - adatforrás - DirectoryConfiguration*

 Előfordulhat, hogy tartalmazza a vagy hello **abszolút** vagy **LocalResource** elem, de soha sem egyszerre mindkettőre.  

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**Abszolút**|hello abszolút elérési út toohello directory toomonitor. a következő attribútumok hello szükség:<br /><br /> - **Elérési út** -abszolút elérési út toohello directory toomonitor hello.<br /><br /> - **expandEnvironment** -konfigurálható, hogy az elérési út környezeti változókat levő figyelő kibontva látható.|  
|**LocalResource**|hello elérési útja relatív tooa helyi erőforrás toomonitor. Szükséges attribútumok pedig a következők:<br /><br /> - **Név** -hello hello directory toomonitor tartalmazó helyi erőforrás<br /><br /> - **relativePath** -elérési út relatív tooName hello directory toomonitor tartalmazó hello|  



## <a name="etwproviders-element"></a>EtwProviders elem  
 *Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*

 Konfigurálja az EventSource ETW-eseményeinek gyűjtése és/vagy a szolgáltatók ETW Manifest alapján.  

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Konfigurálja az előállított eseményeinek gyűjtése [EventSource osztály](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Kötelező attribútum:<br /><br /> **szolgáltató** -hello EventSource esemény hello osztály neve.<br /><br /> Nem kötelező attribútumok pedig a következők:<br /><br /> - **scheduledTransferLogLevelFilter** -hello minimális súlyossági szint tootransfer tooyour tárfiók.<br /><br /> - **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**EtwManifestProviderConfiguration**|Kötelező attribútum:<br /><br /> **szolgáltató** -hello hello eseményszolgáltató GUID azonosítója<br /><br /> Nem kötelező attribútumok pedig a következők:<br /><br /> - **scheduledTransferLogLevelFilter** -hello minimális súlyossági szint tootransfer tooyour tárfiók.<br /><br /> - **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration elem  
 *: Tartománygyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*

 Konfigurálja az előállított eseményeinek gyűjtése [EventSource osztály](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).  

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**DefaultEvents**|Nem kötelező attribútum:<br/><br/> **eventDestination** – hello hello tábla toostore hello események neve|  
|**Esemény**|Kötelező attribútum:<br /><br /> **azonosító** -hello esemény hello azonosítója.<br /><br /> Nem kötelező attribútum:<br /><br /> **eventDestination** – hello hello tábla toostore hello események neve|  



## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration elem  
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**DefaultEvents**|Nem kötelező attribútum:<br /><br /> **eventDestination** – hello hello tábla toostore hello események neve|  
|**Esemény**|Kötelező attribútum:<br /><br /> **azonosító** -hello esemény hello azonosítója.<br /><br /> Nem kötelező attribútum:<br /><br /> **eventDestination** – hello hello tábla toostore hello események neve|  



## <a name="metrics-element"></a>Metrikák elem  
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - metrikák*

 Lehetővé teszi a toogenerate gyors lekérdezéseket optimalizált teljesítményt számláló tábla. Minden teljesítményszámláló hello definiált **PerformanceCounters** elem hello metrikák tábla hozzáadása toohello teljesítményszámláló tábla tárolja.  

 Hello **resourceId** attribútumot kell megadni.  erőforrás-azonosítója hello hello Azure Diagnostics meg tudja telepít virtuális gépet. Első hello **resourceID** a hello [Azure-portálon](https://portal.azure.com). Válassza ki **Tallózás** -> **erőforráscsoportok** -> **< név\>**. Hello kattintson **tulajdonságok** csempén, és másolja hello érték hello **azonosító** mező.  

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**MetricAggregation**|Kötelező attribútum:<br /><br /> **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="performancecounters-element"></a>PerformanceCounters elem  
 *Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters*

 Lehetővé teszi, hogy a teljesítményszámlálók hello gyűjteménye.  

 Nem kötelező attribútum:  

 Nem kötelező **scheduledTransferPeriod** attribútum. Tekintse meg a korábban magyarázatot.

|Gyermekelem|Leírás|  
|-------------------|-----------------|  
|**PerformanceCounterConfiguration**|a következő attribútumok hello szükség:<br /><br /> - **counterSpecifier** – hello hello teljesítményszámláló nevét. Például: `\Processor(_Total)\% Processor Time`. tooget teljesítmény listáját a gazdagépen futó számlálók paranccsal hello `typeperf`.<br /><br /> - **sampleRate** -milyen gyakran hello számláló kell mintát venni.<br /><br /> Nem kötelező attribútum:<br /><br /> **egység** -hello mértékegység hello számláló.|  




## <a name="windowseventlog-element"></a>WindowsEventLog elem
 *Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*
 
 Lehetővé teszi, hogy hello gyűjtemény a Windows eseménynaplóiban keresse meg.  

 Nem kötelező **scheduledTransferPeriod** attribútum. Tekintse meg a korábban magyarázatot.  

|Gyermekelem|Leírás|  
|-------------------|-----------------|  
|**Adatforrás**|Windows-eseménynaplók toocollect hello. Kötelező attribútum:<br /><br /> **név** -leíró hello windows események toobe hello XPath-lekérdezés gyűjtött. Példa:<br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> toocollect összes esemény, adja meg "*"|  




## <a name="logs-element"></a>Naplók elem  
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - naplók*

 Jelen verziójában 1.0 és 1.1. Hiányzik a 1.2-es. A hozzáadott 1.3.  

 Definiálja a hello puffer konfigurációt az alapszintű Azure-naplók.  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|**unsignedInt**|Választható. Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.<br /><br /> hello alapértelmezett érték 0.|  
|**scheduledTransferLogLevelFilterr**|**karakterlánc**|Választható. Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje. hello alapértelmezett értéke **meghatározatlan**, amely továbbítja az összes naplófájlt. Más lehetséges értékek (a legtöbb tooleast információt sorrendben): **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **Kritikus**.|  
|**scheduledTransferPeriod**|**időtartam**|Választható. Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.<br /><br /> hello alapértelmezés szerint PT0S.|  
|**fogadók esetében** 1.5-ös hozzáadva|**karakterlánc**|Választható. Pontok tooa a fogadó hely tooalso diagnosztikai adatok küldése. Például az Application Insights.|  

## <a name="dockersources"></a>DockerSources
 *Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*

 1.9 felvételére.

|Elem neve|Leírás|  
|------------------|-----------------|  
|**Statisztikák**|Hello rendszer közli a Docker-tároló toocollect statisztikái|  

## <a name="sinksconfig-element"></a>SinksConfig elem  
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*

 Helyek toosend diagnosztikai adatok tooand hello konfigurációja azokon a helyeken társított listáját.  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**A fogadó**|Ezen az oldalon máshol lásd a leírást.|  

## <a name="sink-element"></a>Elem gyűjtése
 *Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - fogadó*

 1.5-ös verziójának felvételére.  

 Határozza meg a helyek toosend diagnosztikai adatokat. Például hello Application Insights szolgáltatással.  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**név**|Karakterlánc|Egy karakterlánc azonosító hello sinkname.|  

|Elem|Típus|Leírás|  
|-------------|----------|-----------------|  
|**Application Insights**|Karakterlánc|Csak küldött használt adatok tooApplication Insights. Hello hozzáféréssel rendelkező aktív Application Insights-fiók kulcsát Instrumentation tartalmaz.|  
|**Csatornák**|Karakterlánc|Minden további szűréséhez, hogy adatfolyamként küldje el, amikor egy|  

## <a name="channels-element"></a>Csatornák elem  
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - fogadó - csatornák*

 1.5-ös verziójának felvételére.  

 Határozza meg a naplózási adatokat, hogy át kellene haladnia egy fogadó adatfolyamokat szűrőket.  

|Elem|Típus|Leírás|  
|-------------|----------|-----------------|  
|**Csatorna**|Karakterlánc|Ezen az oldalon máshol lásd a leírást.|  

## <a name="channel-element"></a>Csatorna elem
 *Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - fogadó - csatornák - csatorna*

 1.5-ös verziójának felvételére.  

 Határozza meg a helyek toosend diagnosztikai adatokat. Például hello Application Insights szolgáltatással.  

|Attribútumok|Típus|Leírás|  
|----------------|----------|-----------------|  
|**logLevel**|**karakterlánc**|Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje. hello alapértelmezett értéke **meghatározatlan**, amely továbbítja az összes naplófájlt. Más lehetséges értékek (a legtöbb tooleast információt sorrendben): **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **Kritikus**.|  
|**név**|**karakterlánc**|Egy egyedi nevet a hello csatorna toorefer|  


## <a name="privateconfig-element"></a>PrivateConfig elem 
 *Fa: Gyökér - DiagnosticsConfiguration - PrivateConfig*

 1.3-as verzió felvételére.  

 Optional  

 Személyes adatait hello hello storage-fiók (a nevét, a kulcs és a végpont) tárolja. Ezt az információt toohello virtuális gép küldött, de nem kérhetők le azt.  

|Gyermekelemek|Leírás|  
|--------------------|-----------------|  
|**StorageAccount**|hello tárolási fiók toouse. a következő attribútumok hello szükségesek.<br /><br /> - **név** – hello hello storage-fiók nevét.<br /><br /> - **kulcs** – hello toohello storage-fiók kulcsát.<br /><br /> - **végpont** -hello végpont tooaccess hello storage-fiók. <br /><br /> -**sasToken** (hozzáadott 1.8.1)-hello titkos config is megadhat egy SAS-token helyett a tárfiók kulcsára. Ha adott, a rendszer figyelmen kívül hagyja hello tárfiók kulcsa. <br />SAS-Token hello vonatkozó követelmények: <br />-Fiók SAS-jogkivonat csak támogatja <br />- *b*, *t* szolgáltatástípusok szükség. <br /> - *egy*, *c*, *u*, *w* engedélyekre szükség. <br /> - *c*, *o* típusú erőforrások szükségesek. <br /> -Hello HTTPS protokollt támogatja <br /> -Elindításához és a lejárati időpont érvényesnek kell lennie.|  


## <a name="isenabled-element"></a>IsEnabled elem  
 *Fa: Gyökér - DiagnosticsConfiguration - IsEnabled*

 Logikai érték. Használjon `true` tooenable hello diagnosztika vagy `false` toodisable hello diagnosztika.
