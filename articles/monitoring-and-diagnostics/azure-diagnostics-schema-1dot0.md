---
title: "Diagnosztika 1.0 konfigurációs séma aaaAzure |} Microsoft Docs"
description: "CSAK akkor érvényes, ha az Azure SDK 2.4 használ és az alacsonyabb Azure virtuális gépek, a virtuálisgép-méretezési csoportok, a Service Fabric vagy a Cloud Services."
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
ms.openlocfilehash: bdd2b26217d6ea28f19e651ab429e7e7401ff57b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Az Azure Diagnostics 1.0 konfigurációs séma
> [!NOTE]
> Az Azure Diagnostics hello használt toocollect teljesítményszámlálók és más Azure virtuális gépek, a virtuálisgép-méretezési csoportok, a Service Fabric és a Felhőszolgáltatások statisztikáit.  Ezen a lapon csak fontos, ha ilyen szolgáltatást használ.
>

Az Azure Diagnostics más Microsoft-diagnosztika termékekkel, például Azure figyelő, az Application Insights és Naplóelemzési szolgál.

hello Azure Diagnostics konfigurációs fájl használt tooinitialize hello diagnosztikai figyelő értékeket határozza meg. A fájl használt tooinitialize diagnosztikai konfigurációs beállítások, amikor a hello diagnosztika indítása.  

 Alapértelmezés szerint hello Azure Diagnostics konfigurációs sémafájl telepített toohello `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` könyvtár. Cserélje le `<version>` hello verziójával telepített hello [Azure SDK](http://www.windowsazure.com/develop/downloads/).  

> [!NOTE]
>  hello diagnosztika konfigurációs fájl főként a a korábbi hello indítási folyamat gyűjtött diagnosztikai adatok toobe igénylő feladatok indítása. Azure Diagnostics használatával kapcsolatos további információkért lásd: [naplózási adatok gyűjtése Azure Diagnostics használatával](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).  

## <a name="example-of-hello-diagnostics-configuration-file"></a>Hello diagnosztika konfigurációs fájl példa  
 a következő példa hello mutatja egy tipikus diagnosztika konfigurációs fájlt:  

```xml  
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"  
      configurationChangePollInterval="PT1M"  
      overallQuotaInMB="4096">  
   <DiagnosticInfrastructureLogs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  
   <Logs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  

   <Directories bufferQuotaInMB="1024"   
      scheduledTransferPeriod="PT1M">  

      <!-- These three elements specify hello special directories   
           that are set up for hello log types -->  
      <CrashDumps container="wad-crash-dumps" directoryQuotaInMB="256" />  
      <FailedRequestLogs container="wad-frq" directoryQuotaInMB="256" />  
      <IISLogs container="wad-iis" directoryQuotaInMB="256" />  

      <!-- For regular directories hello DataSources element is used -->  
      <DataSources>  
         <DirectoryConfiguration container="wad-panther" directoryQuotaInMB="128">  
            <!-- Absolute specifies an absolute path with optional environment expansion -->  
            <Absolute expandEnvironment="true" path="%SystemRoot%\system32\sysprep\Panther" />  
         </DirectoryConfiguration>  
         <DirectoryConfiguration container="wad-custom" directoryQuotaInMB="128">  
            <!-- LocalResource specifies a path relative tooa local   
                 resource defined in hello service definition -->  
            <LocalResource name="MyLoggingLocalResource" relativePath="logs" />  
         </DirectoryConfiguration>  
      </DataSources>  
   </Directories>  

   <PerformanceCounters bufferQuotaInMB="512" scheduledTransferPeriod="PT1M">  
      <!-- hello counter specifier is in hello same format as hello imperative   
           diagnostics configuration API -->  
      <PerformanceCounterConfiguration   
         counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT5S" />  
   </PerformanceCounters>  

   <WindowsEventLog bufferQuotaInMB="512"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M">  
      <!-- hello event log name is in hello same format as hello imperative   
           diagnostics configuration API -->  
      <DataSource name="System!*" />  
   </WindowsEventLog>  
</DiagnosticMonitorConfiguration>  
```  

## <a name="diagnosticsconfiguration-namespace"></a>DiagnosticsConfiguration Namespace  
 hello XML-névtér hello diagnosztika konfigurációs fájl van:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Séma elemei  
 hello diagnosztika konfigurációs fájl hello a következő elemeket tartalmazza.


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration elem  
hello legfelső szintű elem hello diagnosztika konfigurációs fájl.  

Attribútumok:

|Attribútum  |Típus   |Szükséges| Alapértelmezett | Leírás|  
|-----------|-------|--------|---------|------------|  
|**configurationChangePollInterval**|Időtartam|Optional | PT1M| Diagnosztikai konfigurációs módosítások, mely hello diagnosztikai figyelő szavazások hello időközét adja meg.|  
|**overallQuotaInMB**|unsignedInt|Optional| 4000 MB. Ha megad egy értéket, nem haladhatja meg ezt a mennyiséget |hello teljes mennyisége az összes naplózási pufferek lefoglalt fájl rendszerhez szükséges tárhelyet.|  

## <a name="diagnosticinfrastructurelogs-element"></a>DiagnosticInfrastructureLogs elem  
Definiálja a hello puffer konfigurációt hello tartozó diagnosztikai infrastruktúra által létrehozott hello naplókhoz.

Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).  

Attribútumok:

|Attribútum|Típus|Leírás|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Választható. Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.<br /><br /> hello alapértelmezett érték 0.|  
|**scheduledTransferLogLevelFilter**|Karakterlánc|Választható. Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje. hello alapértelmezett értéke **meghatározatlan**. Más lehetséges értékei **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **kritikus**.|  
|**scheduledTransferPeriod**|Időtartam|Választható. Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.<br /><br /> hello alapértelmezés szerint PT0S.|  

## <a name="logs-element"></a>Naplók elem  
 Definiálja a hello puffer konfigurációt az alapszintű Azure-naplók.

 Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).  

Attribútumok:  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Választható. Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.<br /><br /> hello alapértelmezett érték 0.|  
|**scheduledTransferLogLevelFilter**|Karakterlánc|Választható. Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje. hello alapértelmezett értéke **meghatározatlan**. Más lehetséges értékei **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **kritikus**.|  
|**scheduledTransferPeriod**|Időtartam|Választható. Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.<br /><br /> hello alapértelmezés szerint PT0S.|  

## <a name="directories-element"></a>Könyvtárak elem  
Hello puffer konfigurációs fájl alapú naplófájlokat, amelyek alapján meghatározhatja az határozza meg.

Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).  


Attribútumok:  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Választható. Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.<br /><br /> hello alapértelmezett érték 0.|  
|**scheduledTransferPeriod**|Időtartam|Választható. Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.<br /><br /> hello alapértelmezés szerint PT0S.|  

## <a name="crashdumps-element"></a>CrashDumps elem  
 Hello összeomlási memóriaképek directory határozza meg.

 Szülőelem: [könyvtárak elem](#Directories).  

Attribútumok:  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**tároló**|Karakterlánc|hol hello hello könyvtár tartalma található toobe hello tároló neve hello át.|  
|**directoryQuotaInMB**|unsignedInt|Választható. Hello hello könyvtár maximális méretét megabájtban megadva.<br /><br /> hello alapértelmezett érték 0.|  

## <a name="failedrequestlogs-element"></a>FailedRequestLogs elem  
 Határozza meg a sikertelen kérelmek naplókönyvtár hello.

 Szülő elem [könyvtárak elem](#Directories).  

Attribútumok:  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**tároló**|Karakterlánc|hol hello hello könyvtár tartalma található toobe hello tároló neve hello át.|  
|**directoryQuotaInMB**|unsignedInt|Választható. Hello hello könyvtár maximális méretét megabájtban megadva.<br /><br /> hello alapértelmezett érték 0.|  

##  <a name="iislogs-element"></a>IISLogs elem  
 Hello IIS naplókönyvtár határozza meg.

 Szülő elem [könyvtárak elem](#Directories).  

Attribútumok:  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**tároló**|Karakterlánc|hol hello hello könyvtár tartalma található toobe hello tároló neve hello át.|  
|**directoryQuotaInMB**|unsignedInt|Választható. Hello hello könyvtár maximális méretét megabájtban megadva.<br /><br /> hello alapértelmezett érték 0.|  

## <a name="datasources-element"></a>Adatforrások elem  
 Határozza meg a nulla vagy több további naplók könyvtárak.

 Szülőelem: [könyvtárak elem](#Directories).

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration elem  
 Határozza meg a napló fájlok toomonitor hello könyvtárában.

 Szülőelem: [adatforrások elem](#DataSources).

Attribútumok:

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**tároló**|Karakterlánc|hol hello hello könyvtár tartalma található toobe hello tároló neve hello át.|  
|**directoryQuotaInMB**|unsignedInt|Választható. Hello hello könyvtár maximális méretét megabájtban megadva.<br /><br /> hello alapértelmezett érték 0.|  

## <a name="absolute-element"></a>Abszolút elem  
 A nem kötelező környezeti bővítéssel rendelkező hello directory toomonitor abszolút elérési utat határozza meg.

 Szülőelem: [DirectoryConfiguration elem](#DirectoryConfiguration).  

Attribútumok:  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**elérési út**|Karakterlánc|Kötelező. hello abszolút elérési út toohello directory toomonitor.|  
|**expandEnvironment**|Logikai érték|Kötelező. Ha túl beállítása**igaz**, környezeti változók hello elérési úton levő figyelő kibontva látható.|  

## <a name="localresource-element"></a>LocalResource elem  
 Határozza meg az elérési út relatív tooa helyi erőforrás hello szolgáltatás-definícióban meghatározott.

 Szülőelem: [DirectoryConfiguration elem](#DirectoryConfiguration).  

Attribútumok:  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**név**|Karakterlánc|Kötelező. hello helyi erőforrás hello directory toomonitor tartalmazó hello nevét.|  
|**relativePath**|Karakterlánc|Kötelező. hello elérési útja relatív toohello helyi erőforrás toomonitor.|  

## <a name="performancecounters-element"></a>PerformanceCounters elem  
 Hello elérési toohello teljesítmény számláló toocollect határozza meg.

 Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).


 Attribútumok:  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Választható. Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.<br /><br /> hello alapértelmezett érték 0.|  
|**scheduledTransferPeriod**|Időtartam|Választható. Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.<br /><br /> hello alapértelmezés szerint PT0S.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration elem  
 Hello teljesítmény számláló toocollect határozza meg.

 Szülőelem: [PerformanceCounters elem](#PerformanceCounters).  

 Attribútumok:  

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**counterSpecifier**|Karakterlánc|Kötelező. hello elérési toohello teljesítmény számláló toocollect.|  
|**sampleRate**|Időtartam|Kötelező. hello sebessége, mely hello teljesítményszámláló kell összegyűjteni.|  

## <a name="windowseventlog-element"></a>WindowsEventLog elem  
 Hello eseménynaplók toomonitor határozza meg.

 Szülőelem: [DiagnosticMonitorConfiguration elem](#DiagnosticMonitorConfiguration).

  Attribútumok:

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Választható. Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.<br /><br /> hello alapértelmezett érték 0.|  
|**scheduledTransferLogLevelFilter**|Karakterlánc|Választható. Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje. hello alapértelmezett értéke **meghatározatlan**. Más lehetséges értékei **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **kritikus**.|  
|**scheduledTransferPeriod**|Időtartam|Választható. Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.<br /><br /> hello alapértelmezés szerint PT0S.|  

## <a name="datasource-element"></a>DataSource elem  
 Hello Eseménynapló toomonitor határozza meg.

 Szülőelem: [WindowsEventLog elem](#windowsEventLog).  

 Attribútumok:

|Attribútum|Típus|Leírás|  
|---------------|----------|-----------------|  
|**név**|Karakterlánc|Kötelező. Hello napló toocollect megadó XPath kifejezés.|  
