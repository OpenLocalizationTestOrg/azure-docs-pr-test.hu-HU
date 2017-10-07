---
title: "Diagnosztika 1.2-es konfigurációs séma aaaAzure |} Microsoft Docs"
description: "CSAK akkor érvényes, ha Azure virtuális gépek, a virtuálisgép-méretezési csoportok, a Service Fabric vagy a Cloud Services Azure SDK 2.5 használ."
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
ms.openlocfilehash: 31559317b696556a64b51b58800b176ade9a4679
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-12-configuration-schema"></a>Az Azure Diagnostics 1.2-es konfigurációs séma
> [!NOTE]
> Az Azure Diagnostics hello használt toocollect teljesítményszámlálók és más Azure virtuális gépek, a virtuálisgép-méretezési csoportok, a Service Fabric és a Felhőszolgáltatások statisztikáit.  Ezen a lapon csak fontos, ha ilyen szolgáltatást használ.
>

Az Azure Diagnostics más Microsoft-diagnosztika termékekkel, például Azure figyelő, az Application Insights és Naplóelemzési szolgál.

A séma definiálja az hello a lehetséges értékek hello diagnosztikai figyelő indításakor használhatja tooinitialize diagnosztikai beállításait.  


 Töltse le a hello nyilvános konfigurációs fájl sémadefiníciót a következő hello a következő PowerShell-parancs futtatásával:  

```PowerShell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Azure Diagnostics használatával kapcsolatos további információkért lásd: [Azure Cloud Services diagnosztika engedélyezésével](http://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

## <a name="example-of-hello-diagnostics-configuration-file"></a>Hello diagnosztika konfigurációs fájl példa  
 a következő példa hello mutatja egy tipikus diagnosztika konfigurációs fájlt:  

```xml
<?xml version="1.0" encoding="utf-8"?>  
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">  
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
        <EtwEventSourceProviderConfiguration provider="MyProviderClass" scheduledTransferPeriod="PT5M">  
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
      <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
        <CrashDumpConfiguration processName="mynewprocess.exe" />  
        <CrashDumpConfiguration processName="badapp.exe"/>  
      </CrashDumps>  
    </DiagnosticMonitorConfiguration>  
  </WadCfg>  
</PublicConfig>  

```  

## <a name="diagnostics-configuration-namespace"></a>Diagnosztikai konfiguráció Namespace  
 hello XML-névtér hello diagnosztika konfigurációs fájl van:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="publicconfig-element"></a>PublicConfig elem  
 Legfelső szintű elem hello diagnosztika konfigurációs fájl. hello következő táblázatban hello elemek hello konfigurációs fájl.  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**WadCfg**|Kötelező. Hello telemetriai adatok toobe konfigurációs beállításainak összegyűjtése.|  
|**StorageAccount**|hello Azure Storage fiók toostore hello adatait a hello neve. Ez is adható meg paraméterként hello Set-AzureServiceDiagnosticsExtension parancsmag végrehajtása közben.|  
|**LocalResourceDirectory**|hello Figyelőügynök toostore eseményadatok használják hello virtuális gép toobe hello könyvtárához. Ha nem, hello alapértelmezett címtár használatos:<br /><br /> A munkavégző vagy webes szerepkör:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> A virtuális gép:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Szükséges attribútumok pedig a következők:<br /><br /> -                      **elérési út** – hello Azure Diagnostics által használt hello rendszer toobe könyvtárába.<br /><br /> -                      **expandEnvironment** -szabályozza, hogy a környezeti változók vannak bontva hello elérési útja.|  

## <a name="wadcfg-element"></a>WadCFG elem  
Meghatározza a hello telemetriai adatok toobe gyűjtött beállításait. a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Kötelező. Nem kötelező attribútumok pedig a következők:<br /><br /> -                     **overallQuotaInMB** -Azure Diagnostics által gyűjtött diagnosztikai adatok különböző típusú hello maximális memóriamennyiség helyi, amelyeket a hello lehet használni. hello alapértéke 5120MB.<br /><br /> -                     **useProxyServer** -konfigurálása Azure Diagnostics toouse hello proxykiszolgáló beállításait az Internet Explorer beállításainak megfelelően.|  
|**CrashDumps**|Az összeomlási memóriaképek gyűjtésének engedélyezése. Nem kötelező attribútumok pedig a következők:<br /><br /> -                     **containerName** -használt toostore összeomlási memóriaképek hello blob tárolóhoz az Azure Storage-fiók toobe hello neve.<br /><br /> -                     **crashDumpType** -konfigurálja az Azure Diagnostics toocollect Mini vagy teljes összeomlási memóriaképek.<br /><br /> -                     **directoryQuotaPercentage**-hello százaléka konfigurálja **overallQuotaInMB** lefoglalva a virtuális gép hello összeomlási memóriaképek toobe.|  
|**DiagnosticInfrastructureLogs**|Az Azure diagnosztikai által létrehozott naplók gyűjtésének engedélyezése. hello diagnosztikai infrastruktúra naplók hibaelhárítási hello diagnosztika rendszert illeti hasznosak. Nem kötelező attribútumok pedig a következők:<br /><br /> -                     **scheduledTransferLogLevelFilter** -hello minimális súlyossági szintet az összegyűjtött hello naplók konfigurálja.<br /><br /> -                     **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**Könyvtárak**|Lehetővé teszi, hogy hello egy könyvtár tartalmának hello gyűjteménye, IIS sikertelen volt a hozzáférési kérelem naplók és/vagy az IIS-naplókba. Nem kötelező attribútum:<br /><br /> **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**EtwProviders**|Konfigurálja az EventSource ETW-eseményeinek gyűjtése és/vagy a szolgáltatók ETW Manifest alapján.|  
|**Metrikák**|Ez az elem toogenerate gyors lekérdezéseket optimalizált teljesítményt számláló tábla lehetővé teszi. Minden teljesítményszámláló hello definiált **PerformanceCounters** elem hello metrikák tábla hozzáadása toohello teljesítményszámláló tábla tárolja. Kötelező attribútum:<br /><br /> **resourceId** -Ez az erőforrás-azonosítója hello hello Azure Diagnostics meg tudja telepít virtuális gép. Első hello **resourceID** a hello [Azure-portálon](https://portal.azure.com). Válassza ki **Tallózás** -> **erőforráscsoportok** -> **< név\>**. Hello kattintson **tulajdonságok** csempén, és másolja hello érték hello **azonosító** mező.|  
|**PerformanceCounters**|Lehetővé teszi, hogy a teljesítményszámlálók hello gyűjteménye. Nem kötelező attribútum:<br /><br /> **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. Az érték egy [XML "Duration adattípus".](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**WindowsEventLog**|Lehetővé teszi, hogy hello gyűjtemény a Windows eseménynaplóiban keresse meg. Nem kötelező attribútum:<br /><br /> **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. Az érték egy [XML "Duration adattípus".](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  

## <a name="crashdumps-element"></a>CrashDumps elem  
 Lehetővé teszi, hogy összeomlási memóriaképek gyűjteménye. a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**CrashDumpConfiguration**|Kötelező. Kötelező attribútum:<br /><br /> **Folyamatnév** – hello hello folyamat összeomlási adja az Azure Diagnostics toocollect kívánt nevét.|  
|**crashDumpType**|Konfigurálja az Azure Diagnostics toocollect mini vagy teljes összeomlási memóriaképeket.|  
|**directoryQuotaPercentage**|Konfigurálja a hello százalékos aránya **overallQuotaInMB** lefoglalva a virtuális gép hello összeomlási memóriaképek toobe.|  

## <a name="directories-element"></a>Könyvtárak elem  
 Lehetővé teszi, hogy hello egy könyvtár tartalmának hello gyűjteménye, IIS sikertelen volt a hozzáférési kérelem naplók és/vagy az IIS-naplókba. a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**Adatforrások**|Könyvtárak toomonitor listáját.|  
|**FailedRequestLogs**|Ez az elem beleértve hello konfigurációban lehetővé teszi, hogy a sikertelen kérelmek tooan IIS webhelyet vagy alkalmazást kapcsolatos információkat naplózza gyűjteménye. Engedélyeznie kell a nyomkövetést **rendszer. Webkiszolgáló** a **Web.config**.|  
|**IISLogs**|Ez az elem beleértve hello konfigurációban lehetővé teszi, hogy az IIS-naplók hello gyűjtésének:<br /><br /> **containerName** -használt toostore hello IIS naplók az Azure Storage-fiók toobe hello blob tárolóhoz hello neve.|  

## <a name="datasources-element"></a>Adatforrások elem  
 Könyvtárak toomonitor listáját. a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**DirectoryConfiguration**|Kötelező. Kötelező attribútum:<br /><br /> **containerName** -fiók használt toobe toostore hello naplófájlok hello blob tároló az Azure Storage hello neve.|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration elem  
 **DirectoryConfiguration** tartalmazhatják vagy hello **abszolút** vagy **LocalResource** elem, de soha sem egyszerre mindkettőre. a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**Abszolút**|hello abszolút elérési út toohello directory toomonitor. a következő attribútumok hello szükség:<br /><br /> -                     **Elérési út** -abszolút elérési út toohello directory toomonitor hello.<br /><br /> -                      **expandEnvironment** -konfigurálható, hogy az elérési út környezeti változókat levő figyelő kibontva látható.|  
|**LocalResource**|hello elérési útja relatív tooa helyi erőforrás toomonitor. Szükséges attribútumok pedig a következők:<br /><br /> -                     **Név** -hello hello directory toomonitor tartalmazó helyi erőforrás<br /><br /> -                     **relativePath** -elérési út relatív tooName hello directory toomonitor tartalmazó hello|  

## <a name="etwproviders-element"></a>EtwProviders elem  
 Konfigurálja az EventSource ETW-eseményeinek gyűjtése és/vagy a szolgáltatók ETW Manifest alapján. a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Konfigurálja az előállított eseményeinek gyűjtése [EventSource osztály](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Kötelező attribútum:<br /><br /> **szolgáltató** -hello EventSource esemény hello osztály neve.<br /><br /> Nem kötelező attribútumok pedig a következők:<br /><br /> -                     **scheduledTransferLogLevelFilter** -hello minimális súlyossági szint tootransfer tooyour tárfiók.<br /><br /> -                     **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. Az érték egy [XML-időtartam adattípust](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  
|**EtwManifestProviderConfiguration**|Kötelező attribútum:<br /><br /> **szolgáltató** -hello hello eseményszolgáltató GUID azonosítója<br /><br /> Nem kötelező attribútumok pedig a következők:<br /><br /> - **scheduledTransferLogLevelFilter** -hello minimális súlyossági szint tootransfer tooyour tárfiók.<br /><br /> -                     **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. Az érték egy [XML-időtartam adattípust](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration elem  
 Konfigurálja az előállított eseményeinek gyűjtése [EventSource osztály](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**DefaultEvents**|Nem kötelező attribútum:<br /><br /> **eventDestination** – hello hello tábla toostore hello események neve|  
|**Esemény**|Kötelező attribútum:<br /><br /> **azonosító** -hello esemény hello azonosítója.<br /><br /> Nem kötelező attribútum:<br /><br /> **eventDestination** – hello hello tábla toostore hello események neve|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration elem  
 a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**DefaultEvents**|Nem kötelező attribútum:<br /><br /> **eventDestination** – hello hello tábla toostore hello események neve|  
|**Esemény**|Kötelező attribútum:<br /><br /> **azonosító** -hello esemény hello azonosítója.<br /><br /> Nem kötelező attribútum:<br /><br /> **eventDestination** – hello hello tábla toostore hello események neve|  

## <a name="metrics-element"></a>Metrikák elem  
 Lehetővé teszi a toogenerate gyors lekérdezéseket optimalizált teljesítményt számláló tábla. a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**MetricAggregation**|Kötelező attribútum:<br /><br /> **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti. Az érték egy [XML-időtartam adattípust](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="performancecounters-element"></a>PerformanceCounters elem  
 Lehetővé teszi, hogy a teljesítményszámlálók hello gyűjteménye. a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**PerformanceCounterConfiguration**|a következő attribútumok hello szükség:<br /><br /> -                     **counterSpecifier** – hello hello teljesítményszámláló nevét. Például: `\Processor(_Total)\% Processor Time`. a gazdagépen hello paranccsal teljesítményszámlálók listája tooget `typeperf`.<br /><br /> -                     **sampleRate** -milyen gyakran hello számláló kell mintát venni.<br /><br /> Nem kötelező attribútum:<br /><br /> **egység** -hello mértékegység hello számláló.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration elem  
 a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**Megjegyzés**|Kötelező attribútum:<br /><br /> **displayName** -hello számláló hello megjelenített neve<br /><br /> Nem kötelező attribútum:<br /><br /> **területi beállítás** -területi toouse hello hello számlálónév megjelenítésekor|  

## <a name="windowseventlog-element"></a>WindowsEventLog elem  
 a következő táblázat hello gyermekelemek ismerteti:  

|Elem neve|Leírás|  
|------------------|-----------------|  
|**Adatforrás**|Windows-eseménynaplók toocollect hello. Kötelező attribútum:<br /><br /> **név** -leíró hello windows események toobe hello XPath-lekérdezés gyűjtött. Példa:<br /><br /> `Application!*[System[(Level >= 3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level >= 3]]`<br /><br /> toocollect összes esemény, adja meg "*".|
