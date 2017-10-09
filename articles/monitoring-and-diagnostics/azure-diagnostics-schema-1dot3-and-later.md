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
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a><span data-ttu-id="db883-103">Az Azure Diagnostics 1.3 és későbbi konfigurációs séma</span><span class="sxs-lookup"><span data-stu-id="db883-103">Azure Diagnostics 1.3 and later configuration schema</span></span>
> [!NOTE]
> <span data-ttu-id="db883-104">a következő hello Azure Diagnostics bővítmény hello összetevő használt toocollect teljesítményszámlálók és más statisztikáit:</span><span class="sxs-lookup"><span data-stu-id="db883-104">hello Azure Diagnostics extension is hello component used toocollect performance counters and other statistics from:</span></span>
> - <span data-ttu-id="db883-105">Azure-alapú virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="db883-105">Azure Virtual Machines</span></span> 
> - <span data-ttu-id="db883-106">Virtual Machine Scale Sets</span><span class="sxs-lookup"><span data-stu-id="db883-106">Virtual Machine Scale Sets</span></span>
> - <span data-ttu-id="db883-107">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="db883-107">Service Fabric</span></span> 
> - <span data-ttu-id="db883-108">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="db883-108">Cloud Services</span></span> 
> - <span data-ttu-id="db883-109">Network Security Groups (Hálózati biztonsági csoportok)</span><span class="sxs-lookup"><span data-stu-id="db883-109">Network Security Groups</span></span>
> 
> <span data-ttu-id="db883-110">Ezen a lapon csak fontos, ha ilyen szolgáltatást használ.</span><span class="sxs-lookup"><span data-stu-id="db883-110">This page is only relevant if you are using one of these services.</span></span>

<span data-ttu-id="db883-111">Ezen a lapon lehet verziók 1.3-as és újabb (az Azure SDK 2.4-es és újabb).</span><span class="sxs-lookup"><span data-stu-id="db883-111">This page is valid for versions 1.3 and newer (Azure SDK 2.4 and newer).</span></span> <span data-ttu-id="db883-112">Újabb konfigurációs szakaszokat megjegyzésként tooshow addig adták hozzá, milyen verzióban.</span><span class="sxs-lookup"><span data-stu-id="db883-112">Newer configuration sections are commented tooshow in what version they were added.</span></span>  

<span data-ttu-id="db883-113">az itt ismertetett hello konfigurációs fájl használt tooset diagnosztikai konfigurációs beállítások esetén hello diagnosztika indítása figyelése.</span><span class="sxs-lookup"><span data-stu-id="db883-113">hello configuration file described here is used tooset diagnostic configuration settings when hello diagnostics monitor starts.</span></span>  

<span data-ttu-id="db883-114">hello kiterjesztést más Microsoft-diagnosztika termékek, például Azure figyelő, az Application Insights és Naplóelemzési együtt használja.</span><span class="sxs-lookup"><span data-stu-id="db883-114">hello extension is used in conjunction with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span>



<span data-ttu-id="db883-115">Töltse le a hello nyilvános konfigurációs fájl sémadefiníciót a következő hello a következő PowerShell-parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="db883-115">Download hello public configuration file schema definition by executing hello following PowerShell command:</span></span>  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

<span data-ttu-id="db883-116">Azure Diagnostics használatával kapcsolatos további információkért lásd: [Azure Diagnostics bővítmény](azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="db883-116">For more information about using Azure Diagnostics, see [Azure Diagnostics Extension](azure-diagnostics.md).</span></span>  

## <a name="example-of-hello-diagnostics-configuration-file"></a><span data-ttu-id="db883-117">Hello diagnosztika konfigurációs fájl példa</span><span class="sxs-lookup"><span data-stu-id="db883-117">Example of hello diagnostics configuration file</span></span>  
 <span data-ttu-id="db883-118">a következő példa hello mutatja egy tipikus diagnosztika konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="db883-118">hello following example shows a typical diagnostics configuration file:</span></span>  

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

<span data-ttu-id="db883-119">JSON egyenértékű hello előző XML konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="db883-119">JSON equivalent of hello previous XML configuration file.</span></span> 

<span data-ttu-id="db883-120">hello PublicConfig és a PrivateConfig egymástól, mert az json használati esetek többségében azok átadása pedig különböző változók.</span><span class="sxs-lookup"><span data-stu-id="db883-120">hello PublicConfig and PrivateConfig are separated because in most json usage cases, they are passed as different variables.</span></span> <span data-ttu-id="db883-121">Ezekben az esetekben közé tartozik a Resource Manager-sablonok, a virtuálisgép-méretezési csoport PowerShell és a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db883-121">These cases include Resource Manager templates, Virtual Machine Scale set PowerShell, and Visual Studio.</span></span> 

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

## <a name="reading-this-page"></a><span data-ttu-id="db883-122">Ez a lap olvasása</span><span class="sxs-lookup"><span data-stu-id="db883-122">Reading this page</span></span>  
 <span data-ttu-id="db883-123">hello a következő címkék vannak nagyjából példa megelőző hello sorrendben.</span><span class="sxs-lookup"><span data-stu-id="db883-123">hello tags following are roughly in order shown in hello preceding example.</span></span>  <span data-ttu-id="db883-124">Ha nem látja a teljes leírás várt azt, keressen hello lap hello elem vagy attribútum.</span><span class="sxs-lookup"><span data-stu-id="db883-124">If you don't see a full description where you expect it, search hello page for hello element or attribute.</span></span>  

## <a name="common-attribute-types"></a><span data-ttu-id="db883-125">Közös attribútum típusát</span><span class="sxs-lookup"><span data-stu-id="db883-125">Common Attribute Types</span></span>  
 <span data-ttu-id="db883-126">**scheduledTransferPeriod** attribútum több elem szerepel.</span><span class="sxs-lookup"><span data-stu-id="db883-126">**scheduledTransferPeriod** attribute appears in several elements.</span></span> <span data-ttu-id="db883-127">Hello időköz ütemezett átvitelek toostorage felfelé toohello legközelebbi perc között.</span><span class="sxs-lookup"><span data-stu-id="db883-127">It is hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="db883-128">hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="db883-128">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span>


## <a name="diagnosticsconfiguration-element"></a><span data-ttu-id="db883-129">DiagnosticsConfiguration elem</span><span class="sxs-lookup"><span data-stu-id="db883-129">DiagnosticsConfiguration Element</span></span>  
 <span data-ttu-id="db883-130">*: Tartománygyökér - DiagnosticsConfiguration*</span><span class="sxs-lookup"><span data-stu-id="db883-130">*Tree: Root - DiagnosticsConfiguration*</span></span>

<span data-ttu-id="db883-131">1.3-as verzió felvételére.</span><span class="sxs-lookup"><span data-stu-id="db883-131">Added in version 1.3.</span></span>  

<span data-ttu-id="db883-132">hello legfelső szintű elem hello diagnosztika konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="db883-132">hello top-level element of hello diagnostics configuration file.</span></span>  

<span data-ttu-id="db883-133">**Attribútum** xmlns - hello XML-névtér az hello diagnosztika konfigurációs fájl:</span><span class="sxs-lookup"><span data-stu-id="db883-133">**Attribute**  xmlns - hello XML namespace for hello diagnostics configuration file is:</span></span>  
<span data-ttu-id="db883-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span><span class="sxs-lookup"><span data-stu-id="db883-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span></span>  


|<span data-ttu-id="db883-135">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-135">Child Elements</span></span>|<span data-ttu-id="db883-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-136">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-137">**PublicConfig**</span><span class="sxs-lookup"><span data-stu-id="db883-137">**PublicConfig**</span></span>|<span data-ttu-id="db883-138">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="db883-138">Required.</span></span> <span data-ttu-id="db883-139">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-139">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="db883-140">**PrivateConfig**</span><span class="sxs-lookup"><span data-stu-id="db883-140">**PrivateConfig**</span></span>|<span data-ttu-id="db883-141">Választható.</span><span class="sxs-lookup"><span data-stu-id="db883-141">Optional.</span></span> <span data-ttu-id="db883-142">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-142">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="db883-143">**IsEnabled**</span><span class="sxs-lookup"><span data-stu-id="db883-143">**IsEnabled**</span></span>|<span data-ttu-id="db883-144">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="db883-144">Boolean.</span></span> <span data-ttu-id="db883-145">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-145">See description elsewhere on this page.</span></span>|  

## <a name="publicconfig-element"></a><span data-ttu-id="db883-146">PublicConfig elem</span><span class="sxs-lookup"><span data-stu-id="db883-146">PublicConfig Element</span></span>  
 <span data-ttu-id="db883-147">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig*</span><span class="sxs-lookup"><span data-stu-id="db883-147">*Tree: Root - DiagnosticsConfiguration - PublicConfig*</span></span>

 <span data-ttu-id="db883-148">Hello nyilvános diagnosztikai konfigurációja ismerteti.</span><span class="sxs-lookup"><span data-stu-id="db883-148">Describes hello public diagnostics configuration.</span></span>  

|<span data-ttu-id="db883-149">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-149">Child Elements</span></span>|<span data-ttu-id="db883-150">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-150">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-151">**WadCfg**</span><span class="sxs-lookup"><span data-stu-id="db883-151">**WadCfg**</span></span>|<span data-ttu-id="db883-152">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="db883-152">Required.</span></span> <span data-ttu-id="db883-153">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-153">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="db883-154">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="db883-154">**StorageAccount**</span></span>|<span data-ttu-id="db883-155">hello Azure Storage fiók toostore hello adatait a hello neve.</span><span class="sxs-lookup"><span data-stu-id="db883-155">hello name of hello Azure Storage account toostore hello data in.</span></span> <span data-ttu-id="db883-156">Előfordulhat, hogy is meg kell adni egy paraméterként hello Set-AzureServiceDiagnosticsExtension parancsmag végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="db883-156">May also be specified as a parameter when executing hello Set-AzureServiceDiagnosticsExtension cmdlet.</span></span>|  
|<span data-ttu-id="db883-157">**StorageType**</span><span class="sxs-lookup"><span data-stu-id="db883-157">**StorageType**</span></span>|<span data-ttu-id="db883-158">Lehet *tábla*, *Blob*, vagy *TableAndBlob*.</span><span class="sxs-lookup"><span data-stu-id="db883-158">Can be *Table*, *Blob*, or *TableAndBlob*.</span></span> <span data-ttu-id="db883-159">Tábla alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="db883-159">Table is default.</span></span> <span data-ttu-id="db883-160">Ha TableAndBlob van kiválasztva, diagnosztikai adatot ír kétszer – egyszer tooeach típusa.</span><span class="sxs-lookup"><span data-stu-id="db883-160">When TableAndBlob is chosen, diagnostic data is written twice -- once tooeach type.</span></span>|  
|<span data-ttu-id="db883-161">**LocalResourceDirectory**</span><span class="sxs-lookup"><span data-stu-id="db883-161">**LocalResourceDirectory**</span></span>|<span data-ttu-id="db883-162">hello directory ahol hello Monitoring Agent-tárolók eseményadatokat hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="db883-162">hello directory on hello virtual machine where hello Monitoring Agent stores event data.</span></span> <span data-ttu-id="db883-163">Ha nem, állítsa be, hello alapértelmezett címtár szerepel:</span><span class="sxs-lookup"><span data-stu-id="db883-163">If not, set, hello default directory is used:</span></span><br /><br /> <span data-ttu-id="db883-164">A munkavégző vagy webes szerepkör:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span><span class="sxs-lookup"><span data-stu-id="db883-164">For a Worker/web role: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span></span><br /><br /> <span data-ttu-id="db883-165">A virtuális gép:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span><span class="sxs-lookup"><span data-stu-id="db883-165">For a Virtual Machine: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span></span><br /><br /> <span data-ttu-id="db883-166">Szükséges attribútumok pedig a következők:</span><span class="sxs-lookup"><span data-stu-id="db883-166">Required attributes are:</span></span><br /><br /> <span data-ttu-id="db883-167">- **elérési út** – hello Azure Diagnostics által használt hello rendszer toobe könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="db883-167">- **path** - hello directory on hello system toobe used by Azure Diagnostics.</span></span><br /><br /> <span data-ttu-id="db883-168">- **expandEnvironment** -szabályozza, hogy a környezeti változók vannak bontva hello elérési útja.</span><span class="sxs-lookup"><span data-stu-id="db883-168">- **expandEnvironment** - Controls whether environment variables are expanded in hello path name.</span></span>|  

## <a name="wadcfg-element"></a><span data-ttu-id="db883-169">WadCFG elem</span><span class="sxs-lookup"><span data-stu-id="db883-169">WadCFG Element</span></span>  
 <span data-ttu-id="db883-170">*Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG*</span><span class="sxs-lookup"><span data-stu-id="db883-170">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG*</span></span>
 
 <span data-ttu-id="db883-171">Azonosítja és hello telemetriai adatok toobe gyűjtött konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="db883-171">Identifies and configures hello telemetry data toobe collected.</span></span>  


## <a name="diagnosticmonitorconfiguration-element"></a><span data-ttu-id="db883-172">DiagnosticMonitorConfiguration elem</span><span class="sxs-lookup"><span data-stu-id="db883-172">DiagnosticMonitorConfiguration Element</span></span> 
 <span data-ttu-id="db883-173">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*</span><span class="sxs-lookup"><span data-stu-id="db883-173">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*</span></span>

 <span data-ttu-id="db883-174">Szükséges</span><span class="sxs-lookup"><span data-stu-id="db883-174">Required</span></span> 

|<span data-ttu-id="db883-175">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="db883-175">Attributes</span></span>|<span data-ttu-id="db883-176">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-176">Description</span></span>|  
|----------------|-----------------|  
| <span data-ttu-id="db883-177">**overallQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="db883-177">**overallQuotaInMB**</span></span> | <span data-ttu-id="db883-178">hello maximális memóriamennyiség helyi, amelyeket a hello használni Azure Diagnostics által gyűjtött diagnosztikai adatok különböző típusú.</span><span class="sxs-lookup"><span data-stu-id="db883-178">hello maximum amount of local disk space that may be consumed by hello various types of diagnostic data collected by Azure Diagnostics.</span></span> <span data-ttu-id="db883-179">hello alapértéke 5120 MB.</span><span class="sxs-lookup"><span data-stu-id="db883-179">hello default setting is 5120 MB.</span></span><br />
|<span data-ttu-id="db883-180">**useProxyServer**</span><span class="sxs-lookup"><span data-stu-id="db883-180">**useProxyServer**</span></span> | <span data-ttu-id="db883-181">Ahogyan az Internet Explorer beállításainak Azure Diagnostics toouse hello proxykiszolgáló-beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="db883-181">Configure Azure Diagnostics toouse hello proxy server settings as set in IE settings.</span></span>|  

<br /> <br />

|<span data-ttu-id="db883-182">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-182">Child Elements</span></span>|<span data-ttu-id="db883-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-183">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-184">**CrashDumps**</span><span class="sxs-lookup"><span data-stu-id="db883-184">**CrashDumps**</span></span>|<span data-ttu-id="db883-185">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-185">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="db883-186">**DiagnosticInfrastructureLogs**</span><span class="sxs-lookup"><span data-stu-id="db883-186">**DiagnosticInfrastructureLogs**</span></span>|<span data-ttu-id="db883-187">Az Azure diagnosztikai által létrehozott naplók gyűjtésének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="db883-187">Enable collection of logs generated by Azure Diagnostics.</span></span> <span data-ttu-id="db883-188">hello diagnosztikai infrastruktúra naplók hibaelhárítási hello diagnosztika rendszert illeti hasznosak.</span><span class="sxs-lookup"><span data-stu-id="db883-188">hello diagnostic infrastructure logs are useful for troubleshooting hello diagnostics system itself.</span></span> <span data-ttu-id="db883-189">Nem kötelező attribútumok pedig a következők:</span><span class="sxs-lookup"><span data-stu-id="db883-189">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="db883-190">- **scheduledTransferLogLevelFilter** -hello minimális súlyossági szintet az összegyűjtött hello naplók konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="db883-190">- **scheduledTransferLogLevelFilter** - Configures hello minimum severity level of hello logs collected.</span></span><br /><br /> <span data-ttu-id="db883-191">- **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti.</span><span class="sxs-lookup"><span data-stu-id="db883-191">- **scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="db883-192">hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="db883-192">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="db883-193">**Könyvtárak**</span><span class="sxs-lookup"><span data-stu-id="db883-193">**Directories**</span></span>|<span data-ttu-id="db883-194">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-194">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="db883-195">**EtwProviders**</span><span class="sxs-lookup"><span data-stu-id="db883-195">**EtwProviders**</span></span>|<span data-ttu-id="db883-196">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-196">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="db883-197">**Metrikák**</span><span class="sxs-lookup"><span data-stu-id="db883-197">**Metrics**</span></span>|<span data-ttu-id="db883-198">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-198">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="db883-199">**PerformanceCounters**</span><span class="sxs-lookup"><span data-stu-id="db883-199">**PerformanceCounters**</span></span>|<span data-ttu-id="db883-200">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-200">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="db883-201">**WindowsEventLog**</span><span class="sxs-lookup"><span data-stu-id="db883-201">**WindowsEventLog**</span></span>|<span data-ttu-id="db883-202">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-202">See description elsewhere on this page.</span></span>| 
|<span data-ttu-id="db883-203">**DockerSources**</span><span class="sxs-lookup"><span data-stu-id="db883-203">**DockerSources**</span></span>|<span data-ttu-id="db883-204">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-204">See description elsewhere on this page.</span></span> | 



## <a name="crashdumps-element"></a><span data-ttu-id="db883-205">CrashDumps elem</span><span class="sxs-lookup"><span data-stu-id="db883-205">CrashDumps Element</span></span>  
 <span data-ttu-id="db883-206">*Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*</span><span class="sxs-lookup"><span data-stu-id="db883-206">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*</span></span>
 
 <span data-ttu-id="db883-207">Engedélyezze a összeomlási memóriaképek hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="db883-207">Enable hello collection of crash dumps.</span></span>  

|<span data-ttu-id="db883-208">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="db883-208">Attributes</span></span>|<span data-ttu-id="db883-209">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-209">Description</span></span>|  
|----------------|-----------------|  
|<span data-ttu-id="db883-210">**containerName**</span><span class="sxs-lookup"><span data-stu-id="db883-210">**containerName**</span></span>|<span data-ttu-id="db883-211">Választható.</span><span class="sxs-lookup"><span data-stu-id="db883-211">Optional.</span></span> <span data-ttu-id="db883-212">a használt toostore összeomlási memóriaképek hello blob tárolóhoz az Azure Storage-fiók toobe hello neve.</span><span class="sxs-lookup"><span data-stu-id="db883-212">hello name of hello blob container in your Azure Storage account toobe used toostore crash dumps.</span></span>|  
|<span data-ttu-id="db883-213">**crashDumpType**</span><span class="sxs-lookup"><span data-stu-id="db883-213">**crashDumpType**</span></span>|<span data-ttu-id="db883-214">Választható.</span><span class="sxs-lookup"><span data-stu-id="db883-214">Optional.</span></span>  <span data-ttu-id="db883-215">Konfigurálja az Azure Diagnostics toocollect mini vagy teljes összeomlási memóriaképeket.</span><span class="sxs-lookup"><span data-stu-id="db883-215">Configures Azure Diagnostics toocollect mini or full crash dumps.</span></span>|  
|<span data-ttu-id="db883-216">**directoryQuotaPercentage**</span><span class="sxs-lookup"><span data-stu-id="db883-216">**directoryQuotaPercentage**</span></span>|<span data-ttu-id="db883-217">Választható.</span><span class="sxs-lookup"><span data-stu-id="db883-217">Optional.</span></span>  <span data-ttu-id="db883-218">Konfigurálja a hello százalékos aránya **overallQuotaInMB** lefoglalva a virtuális gép hello összeomlási memóriaképek toobe.</span><span class="sxs-lookup"><span data-stu-id="db883-218">Configures hello percentage of **overallQuotaInMB** toobe reserved for crash dumps on hello VM.</span></span>|  

|<span data-ttu-id="db883-219">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-219">Child Elements</span></span>|<span data-ttu-id="db883-220">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-220">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-221">**CrashDumpConfiguration**</span><span class="sxs-lookup"><span data-stu-id="db883-221">**CrashDumpConfiguration**</span></span>|<span data-ttu-id="db883-222">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="db883-222">Required.</span></span> <span data-ttu-id="db883-223">Határozza meg a konfigurációs értékeket az összes folyamat.</span><span class="sxs-lookup"><span data-stu-id="db883-223">Defines configuration values for each process.</span></span><br /><br /> <span data-ttu-id="db883-224">a következő attribútum hello is szükség:</span><span class="sxs-lookup"><span data-stu-id="db883-224">hello following attribute is also required:</span></span><br /><br /> <span data-ttu-id="db883-225">**Folyamatnév** – hello hello folyamat összeomlási adja az Azure Diagnostics toocollect kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="db883-225">**processName** - hello name of hello process you want Azure Diagnostics toocollect a crash dump for.</span></span>|  

## <a name="directories-element"></a><span data-ttu-id="db883-226">Könyvtárak elem</span><span class="sxs-lookup"><span data-stu-id="db883-226">Directories Element</span></span> 
 <span data-ttu-id="db883-227">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - könyvtárak*</span><span class="sxs-lookup"><span data-stu-id="db883-227">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration -  Directories*</span></span>

 <span data-ttu-id="db883-228">Lehetővé teszi, hogy hello egy könyvtár tartalmának hello gyűjteménye, IIS sikertelen volt a hozzáférési kérelem naplók és/vagy az IIS-naplókba.</span><span class="sxs-lookup"><span data-stu-id="db883-228">Enables hello collection of hello contents of a directory, IIS failed access request logs and/or IIS logs.</span></span>  

 <span data-ttu-id="db883-229">Nem kötelező **scheduledTransferPeriod** attribútum.</span><span class="sxs-lookup"><span data-stu-id="db883-229">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="db883-230">Tekintse meg a korábban magyarázatot.</span><span class="sxs-lookup"><span data-stu-id="db883-230">See explanation earlier.</span></span>  

|<span data-ttu-id="db883-231">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-231">Child Elements</span></span>|<span data-ttu-id="db883-232">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-232">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-233">**IISLogs**</span><span class="sxs-lookup"><span data-stu-id="db883-233">**IISLogs**</span></span>|<span data-ttu-id="db883-234">Ez az elem beleértve hello konfigurációban lehetővé teszi, hogy az IIS-naplók hello gyűjtésének:</span><span class="sxs-lookup"><span data-stu-id="db883-234">Including this element in hello configuration enables hello collection of IIS logs:</span></span><br /><br /> <span data-ttu-id="db883-235">**containerName** -használt toostore hello IIS naplók az Azure Storage-fiók toobe hello blob tárolóhoz hello neve.</span><span class="sxs-lookup"><span data-stu-id="db883-235">**containerName** - hello name of hello blob container in your Azure Storage account toobe used toostore hello IIS logs.</span></span>|   
|<span data-ttu-id="db883-236">**FailedRequestLogs**</span><span class="sxs-lookup"><span data-stu-id="db883-236">**FailedRequestLogs**</span></span>|<span data-ttu-id="db883-237">Ez az elem beleértve hello konfigurációban lehetővé teszi, hogy a sikertelen kérelmek tooan IIS webhelyet vagy alkalmazást kapcsolatos információkat naplózza gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="db883-237">Including this element in hello configuration enables collection of logs about failed requests tooan IIS site or application.</span></span> <span data-ttu-id="db883-238">Engedélyeznie kell a nyomkövetést **rendszer. Webkiszolgáló** a **Web.config**.</span><span class="sxs-lookup"><span data-stu-id="db883-238">You must also enable tracing options under **system.WebServer** in **Web.config**.</span></span>|  
|<span data-ttu-id="db883-239">**Adatforrások**</span><span class="sxs-lookup"><span data-stu-id="db883-239">**DataSources**</span></span>|<span data-ttu-id="db883-240">Könyvtárak toomonitor listáját.</span><span class="sxs-lookup"><span data-stu-id="db883-240">A list of directories toomonitor.</span></span>| 




## <a name="datasources-element"></a><span data-ttu-id="db883-241">Adatforrások elem</span><span class="sxs-lookup"><span data-stu-id="db883-241">DataSources Element</span></span>  
 <span data-ttu-id="db883-242">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - könyvtárak - adatforrás*</span><span class="sxs-lookup"><span data-stu-id="db883-242">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources*</span></span>

 <span data-ttu-id="db883-243">Könyvtárak toomonitor listáját.</span><span class="sxs-lookup"><span data-stu-id="db883-243">A list of directories toomonitor.</span></span>  

|<span data-ttu-id="db883-244">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-244">Child Elements</span></span>|<span data-ttu-id="db883-245">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-245">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-246">**DirectoryConfiguration**</span><span class="sxs-lookup"><span data-stu-id="db883-246">**DirectoryConfiguration**</span></span>|<span data-ttu-id="db883-247">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="db883-247">Required.</span></span> <span data-ttu-id="db883-248">Kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-248">Required attribute:</span></span><br /><br /> <span data-ttu-id="db883-249">**containerName** – hello hello blob tároló az adott toobe használt toostore hello naplófájlok az Azure Storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="db883-249">**containerName** - hello name of hello blob container in your Azure Storage account that toobe used toostore hello log files.</span></span>|  





## <a name="directoryconfiguration-element"></a><span data-ttu-id="db883-250">DirectoryConfiguration elem</span><span class="sxs-lookup"><span data-stu-id="db883-250">DirectoryConfiguration Element</span></span>  
 <span data-ttu-id="db883-251">*Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - könyvtárak - adatforrás - DirectoryConfiguration*</span><span class="sxs-lookup"><span data-stu-id="db883-251">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources - DirectoryConfiguration*</span></span>

 <span data-ttu-id="db883-252">Előfordulhat, hogy tartalmazza a vagy hello **abszolút** vagy **LocalResource** elem, de soha sem egyszerre mindkettőre.</span><span class="sxs-lookup"><span data-stu-id="db883-252">May include either hello **Absolute** or **LocalResource** element but not both.</span></span>  

|<span data-ttu-id="db883-253">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-253">Child Elements</span></span>|<span data-ttu-id="db883-254">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-254">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-255">**Abszolút**</span><span class="sxs-lookup"><span data-stu-id="db883-255">**Absolute**</span></span>|<span data-ttu-id="db883-256">hello abszolút elérési út toohello directory toomonitor.</span><span class="sxs-lookup"><span data-stu-id="db883-256">hello absolute path toohello directory toomonitor.</span></span> <span data-ttu-id="db883-257">a következő attribútumok hello szükség:</span><span class="sxs-lookup"><span data-stu-id="db883-257">hello following attributes are required:</span></span><br /><br /> <span data-ttu-id="db883-258">- **Elérési út** -abszolút elérési út toohello directory toomonitor hello.</span><span class="sxs-lookup"><span data-stu-id="db883-258">- **Path** - hello absolute path toohello directory toomonitor.</span></span><br /><br /> <span data-ttu-id="db883-259">- **expandEnvironment** -konfigurálható, hogy az elérési út környezeti változókat levő figyelő kibontva látható.</span><span class="sxs-lookup"><span data-stu-id="db883-259">- **expandEnvironment** - Configures whether environment variables in Path are expanded.</span></span>|  
|<span data-ttu-id="db883-260">**LocalResource**</span><span class="sxs-lookup"><span data-stu-id="db883-260">**LocalResource**</span></span>|<span data-ttu-id="db883-261">hello elérési útja relatív tooa helyi erőforrás toomonitor.</span><span class="sxs-lookup"><span data-stu-id="db883-261">hello path relative tooa local resource toomonitor.</span></span> <span data-ttu-id="db883-262">Szükséges attribútumok pedig a következők:</span><span class="sxs-lookup"><span data-stu-id="db883-262">Required attributes are:</span></span><br /><br /> <span data-ttu-id="db883-263">- **Név** -hello hello directory toomonitor tartalmazó helyi erőforrás</span><span class="sxs-lookup"><span data-stu-id="db883-263">- **Name** - hello local resource that contains hello directory toomonitor</span></span><br /><br /> <span data-ttu-id="db883-264">- **relativePath** -elérési út relatív tooName hello directory toomonitor tartalmazó hello</span><span class="sxs-lookup"><span data-stu-id="db883-264">- **relativePath** - hello path relative tooName that contains hello directory toomonitor</span></span>|  



## <a name="etwproviders-element"></a><span data-ttu-id="db883-265">EtwProviders elem</span><span class="sxs-lookup"><span data-stu-id="db883-265">EtwProviders Element</span></span>  
 <span data-ttu-id="db883-266">*Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*</span><span class="sxs-lookup"><span data-stu-id="db883-266">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*</span></span>

 <span data-ttu-id="db883-267">Konfigurálja az EventSource ETW-eseményeinek gyűjtése és/vagy a szolgáltatók ETW Manifest alapján.</span><span class="sxs-lookup"><span data-stu-id="db883-267">Configures collection of ETW events from EventSource and/or ETW Manifest based providers.</span></span>  

|<span data-ttu-id="db883-268">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-268">Child Elements</span></span>|<span data-ttu-id="db883-269">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-269">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-270">**EtwEventSourceProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="db883-270">**EtwEventSourceProviderConfiguration**</span></span>|<span data-ttu-id="db883-271">Konfigurálja az előállított eseményeinek gyűjtése [EventSource osztály](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="db883-271">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span> <span data-ttu-id="db883-272">Kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-272">Required attribute:</span></span><br /><br /> <span data-ttu-id="db883-273">**szolgáltató** -hello EventSource esemény hello osztály neve.</span><span class="sxs-lookup"><span data-stu-id="db883-273">**provider** - hello class name of hello EventSource event.</span></span><br /><br /> <span data-ttu-id="db883-274">Nem kötelező attribútumok pedig a következők:</span><span class="sxs-lookup"><span data-stu-id="db883-274">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="db883-275">- **scheduledTransferLogLevelFilter** -hello minimális súlyossági szint tootransfer tooyour tárfiók.</span><span class="sxs-lookup"><span data-stu-id="db883-275">- **scheduledTransferLogLevelFilter** - hello minimum severity level tootransfer tooyour storage account.</span></span><br /><br /> <span data-ttu-id="db883-276">- **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti.</span><span class="sxs-lookup"><span data-stu-id="db883-276">- **scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="db883-277">hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="db883-277">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="db883-278">**EtwManifestProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="db883-278">**EtwManifestProviderConfiguration**</span></span>|<span data-ttu-id="db883-279">Kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-279">Required attribute:</span></span><br /><br /> <span data-ttu-id="db883-280">**szolgáltató** -hello hello eseményszolgáltató GUID azonosítója</span><span class="sxs-lookup"><span data-stu-id="db883-280">**provider** - hello GUID of hello event provider</span></span><br /><br /> <span data-ttu-id="db883-281">Nem kötelező attribútumok pedig a következők:</span><span class="sxs-lookup"><span data-stu-id="db883-281">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="db883-282">- **scheduledTransferLogLevelFilter** -hello minimális súlyossági szint tootransfer tooyour tárfiók.</span><span class="sxs-lookup"><span data-stu-id="db883-282">- **scheduledTransferLogLevelFilter** - hello minimum severity level tootransfer tooyour storage account.</span></span><br /><br /> <span data-ttu-id="db883-283">- **scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti.</span><span class="sxs-lookup"><span data-stu-id="db883-283">- **scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="db883-284">hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="db883-284">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="etweventsourceproviderconfiguration-element"></a><span data-ttu-id="db883-285">EtwEventSourceProviderConfiguration elem</span><span class="sxs-lookup"><span data-stu-id="db883-285">EtwEventSourceProviderConfiguration Element</span></span>  
 <span data-ttu-id="db883-286">*: Tartománygyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*</span><span class="sxs-lookup"><span data-stu-id="db883-286">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders- EtwEventSourceProviderConfiguration*</span></span>

 <span data-ttu-id="db883-287">Konfigurálja az előállított eseményeinek gyűjtése [EventSource osztály](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="db883-287">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span>  

|<span data-ttu-id="db883-288">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-288">Child Elements</span></span>|<span data-ttu-id="db883-289">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-289">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-290">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="db883-290">**DefaultEvents**</span></span>|<span data-ttu-id="db883-291">Nem kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-291">Optional attribute:</span></span><br/><br/> <span data-ttu-id="db883-292">**eventDestination** – hello hello tábla toostore hello események neve</span><span class="sxs-lookup"><span data-stu-id="db883-292">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  
|<span data-ttu-id="db883-293">**Esemény**</span><span class="sxs-lookup"><span data-stu-id="db883-293">**Event**</span></span>|<span data-ttu-id="db883-294">Kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-294">Required attribute:</span></span><br /><br /> <span data-ttu-id="db883-295">**azonosító** -hello esemény hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="db883-295">**id** - hello id of hello event.</span></span><br /><br /> <span data-ttu-id="db883-296">Nem kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-296">Optional attribute:</span></span><br /><br /> <span data-ttu-id="db883-297">**eventDestination** – hello hello tábla toostore hello események neve</span><span class="sxs-lookup"><span data-stu-id="db883-297">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  



## <a name="etwmanifestproviderconfiguration-element"></a><span data-ttu-id="db883-298">EtwManifestProviderConfiguration elem</span><span class="sxs-lookup"><span data-stu-id="db883-298">EtwManifestProviderConfiguration Element</span></span>  
 <span data-ttu-id="db883-299">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*</span><span class="sxs-lookup"><span data-stu-id="db883-299">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*</span></span>

|<span data-ttu-id="db883-300">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-300">Child Elements</span></span>|<span data-ttu-id="db883-301">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-301">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-302">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="db883-302">**DefaultEvents**</span></span>|<span data-ttu-id="db883-303">Nem kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-303">Optional attribute:</span></span><br /><br /> <span data-ttu-id="db883-304">**eventDestination** – hello hello tábla toostore hello események neve</span><span class="sxs-lookup"><span data-stu-id="db883-304">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  
|<span data-ttu-id="db883-305">**Esemény**</span><span class="sxs-lookup"><span data-stu-id="db883-305">**Event**</span></span>|<span data-ttu-id="db883-306">Kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-306">Required attribute:</span></span><br /><br /> <span data-ttu-id="db883-307">**azonosító** -hello esemény hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="db883-307">**id** - hello id of hello event.</span></span><br /><br /> <span data-ttu-id="db883-308">Nem kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-308">Optional attribute:</span></span><br /><br /> <span data-ttu-id="db883-309">**eventDestination** – hello hello tábla toostore hello események neve</span><span class="sxs-lookup"><span data-stu-id="db883-309">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  



## <a name="metrics-element"></a><span data-ttu-id="db883-310">Metrikák elem</span><span class="sxs-lookup"><span data-stu-id="db883-310">Metrics Element</span></span>  
 <span data-ttu-id="db883-311">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - metrikák*</span><span class="sxs-lookup"><span data-stu-id="db883-311">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Metrics*</span></span>

 <span data-ttu-id="db883-312">Lehetővé teszi a toogenerate gyors lekérdezéseket optimalizált teljesítményt számláló tábla.</span><span class="sxs-lookup"><span data-stu-id="db883-312">Enables you toogenerate a performance counter table that is optimized for fast queries.</span></span> <span data-ttu-id="db883-313">Minden teljesítményszámláló hello definiált **PerformanceCounters** elem hello metrikák tábla hozzáadása toohello teljesítményszámláló tábla tárolja.</span><span class="sxs-lookup"><span data-stu-id="db883-313">Each performance counter that is defined in hello **PerformanceCounters** element is stored in hello Metrics table in addition toohello Performance Counter table.</span></span>  

 <span data-ttu-id="db883-314">Hello **resourceId** attribútumot kell megadni.</span><span class="sxs-lookup"><span data-stu-id="db883-314">hello **resourceId** attribute is required.</span></span>  <span data-ttu-id="db883-315">erőforrás-azonosítója hello hello Azure Diagnostics meg tudja telepít virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="db883-315">hello resource ID of hello Virtual Machine you are deploying Azure Diagnostics to.</span></span> <span data-ttu-id="db883-316">Első hello **resourceID** a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="db883-316">Get hello **resourceID** from hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="db883-317">Válassza ki **Tallózás** -> **erőforráscsoportok** -> **< név\>**.</span><span class="sxs-lookup"><span data-stu-id="db883-317">Select **Browse** -> **Resource Groups** -> **<Name\>**.</span></span> <span data-ttu-id="db883-318">Hello kattintson **tulajdonságok** csempén, és másolja hello érték hello **azonosító** mező.</span><span class="sxs-lookup"><span data-stu-id="db883-318">Click hello **Properties** tile and copy hello value from hello **ID** field.</span></span>  

|<span data-ttu-id="db883-319">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-319">Child Elements</span></span>|<span data-ttu-id="db883-320">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-320">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-321">**MetricAggregation**</span><span class="sxs-lookup"><span data-stu-id="db883-321">**MetricAggregation**</span></span>|<span data-ttu-id="db883-322">Kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-322">Required attribute:</span></span><br /><br /> <span data-ttu-id="db883-323">**scheduledTransferPeriod** -ütemezett átvitelek toostorage hello időközétől perc legközelebbi toohello kerekíti.</span><span class="sxs-lookup"><span data-stu-id="db883-323">**scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="db883-324">hello értéke egy [XML "Duration adattípusú."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="db883-324">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="performancecounters-element"></a><span data-ttu-id="db883-325">PerformanceCounters elem</span><span class="sxs-lookup"><span data-stu-id="db883-325">PerformanceCounters Element</span></span>  
 <span data-ttu-id="db883-326">*Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters*</span><span class="sxs-lookup"><span data-stu-id="db883-326">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters*</span></span>

 <span data-ttu-id="db883-327">Lehetővé teszi, hogy a teljesítményszámlálók hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="db883-327">Enables hello collection of performance counters.</span></span>  

 <span data-ttu-id="db883-328">Nem kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-328">Optional attribute:</span></span>  

 <span data-ttu-id="db883-329">Nem kötelező **scheduledTransferPeriod** attribútum.</span><span class="sxs-lookup"><span data-stu-id="db883-329">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="db883-330">Tekintse meg a korábban magyarázatot.</span><span class="sxs-lookup"><span data-stu-id="db883-330">See explanation earlier.</span></span>

|<span data-ttu-id="db883-331">Gyermekelem</span><span class="sxs-lookup"><span data-stu-id="db883-331">Child Element</span></span>|<span data-ttu-id="db883-332">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-332">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="db883-333">**PerformanceCounterConfiguration**</span><span class="sxs-lookup"><span data-stu-id="db883-333">**PerformanceCounterConfiguration**</span></span>|<span data-ttu-id="db883-334">a következő attribútumok hello szükség:</span><span class="sxs-lookup"><span data-stu-id="db883-334">hello following attributes are required:</span></span><br /><br /> <span data-ttu-id="db883-335">- **counterSpecifier** – hello hello teljesítményszámláló nevét.</span><span class="sxs-lookup"><span data-stu-id="db883-335">- **counterSpecifier** - hello name of hello performance counter.</span></span> <span data-ttu-id="db883-336">Például: `\Processor(_Total)\% Processor Time`.</span><span class="sxs-lookup"><span data-stu-id="db883-336">For example, `\Processor(_Total)\% Processor Time`.</span></span> <span data-ttu-id="db883-337">tooget teljesítmény listáját a gazdagépen futó számlálók paranccsal hello `typeperf`.</span><span class="sxs-lookup"><span data-stu-id="db883-337">tooget a list of performance counters on your host, run hello command `typeperf`.</span></span><br /><br /> <span data-ttu-id="db883-338">- **sampleRate** -milyen gyakran hello számláló kell mintát venni.</span><span class="sxs-lookup"><span data-stu-id="db883-338">- **sampleRate** - How often hello counter should be sampled.</span></span><br /><br /> <span data-ttu-id="db883-339">Nem kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-339">Optional attribute:</span></span><br /><br /> <span data-ttu-id="db883-340">**egység** -hello mértékegység hello számláló.</span><span class="sxs-lookup"><span data-stu-id="db883-340">**unit** - hello unit of measure of hello counter.</span></span>|  




## <a name="windowseventlog-element"></a><span data-ttu-id="db883-341">WindowsEventLog elem</span><span class="sxs-lookup"><span data-stu-id="db883-341">WindowsEventLog Element</span></span>
 <span data-ttu-id="db883-342">*Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*</span><span class="sxs-lookup"><span data-stu-id="db883-342">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*</span></span>
 
 <span data-ttu-id="db883-343">Lehetővé teszi, hogy hello gyűjtemény a Windows eseménynaplóiban keresse meg.</span><span class="sxs-lookup"><span data-stu-id="db883-343">Enables hello collection of Windows Event Logs.</span></span>  

 <span data-ttu-id="db883-344">Nem kötelező **scheduledTransferPeriod** attribútum.</span><span class="sxs-lookup"><span data-stu-id="db883-344">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="db883-345">Tekintse meg a korábban magyarázatot.</span><span class="sxs-lookup"><span data-stu-id="db883-345">See explanation  earlier.</span></span>  

|<span data-ttu-id="db883-346">Gyermekelem</span><span class="sxs-lookup"><span data-stu-id="db883-346">Child Element</span></span>|<span data-ttu-id="db883-347">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-347">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="db883-348">**Adatforrás**</span><span class="sxs-lookup"><span data-stu-id="db883-348">**DataSource**</span></span>|<span data-ttu-id="db883-349">Windows-eseménynaplók toocollect hello.</span><span class="sxs-lookup"><span data-stu-id="db883-349">hello Windows Event logs toocollect.</span></span> <span data-ttu-id="db883-350">Kötelező attribútum:</span><span class="sxs-lookup"><span data-stu-id="db883-350">Required attribute:</span></span><br /><br /> <span data-ttu-id="db883-351">**név** -leíró hello windows események toobe hello XPath-lekérdezés gyűjtött.</span><span class="sxs-lookup"><span data-stu-id="db883-351">**name** - hello XPath query describing hello windows events toobe collected.</span></span> <span data-ttu-id="db883-352">Példa:</span><span class="sxs-lookup"><span data-stu-id="db883-352">For example:</span></span><br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> <span data-ttu-id="db883-353">toocollect összes esemény, adja meg "*"</span><span class="sxs-lookup"><span data-stu-id="db883-353">toocollect all events, specify "*"</span></span>|  




## <a name="logs-element"></a><span data-ttu-id="db883-354">Naplók elem</span><span class="sxs-lookup"><span data-stu-id="db883-354">Logs Element</span></span>  
 <span data-ttu-id="db883-355">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - naplók*</span><span class="sxs-lookup"><span data-stu-id="db883-355">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Logs*</span></span>

 <span data-ttu-id="db883-356">Jelen verziójában 1.0 és 1.1.</span><span class="sxs-lookup"><span data-stu-id="db883-356">Present in version 1.0 and 1.1.</span></span> <span data-ttu-id="db883-357">Hiányzik a 1.2-es.</span><span class="sxs-lookup"><span data-stu-id="db883-357">Missing in 1.2.</span></span> <span data-ttu-id="db883-358">A hozzáadott 1.3.</span><span class="sxs-lookup"><span data-stu-id="db883-358">Added back in 1.3.</span></span>  

 <span data-ttu-id="db883-359">Definiálja a hello puffer konfigurációt az alapszintű Azure-naplók.</span><span class="sxs-lookup"><span data-stu-id="db883-359">Defines hello buffer configuration for basic Azure logs.</span></span>  

|<span data-ttu-id="db883-360">Attribútum</span><span class="sxs-lookup"><span data-stu-id="db883-360">Attribute</span></span>|<span data-ttu-id="db883-361">Típus</span><span class="sxs-lookup"><span data-stu-id="db883-361">Type</span></span>|<span data-ttu-id="db883-362">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-362">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="db883-363">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="db883-363">**bufferQuotaInMB**</span></span>|<span data-ttu-id="db883-364">**unsignedInt**</span><span class="sxs-lookup"><span data-stu-id="db883-364">**unsignedInt**</span></span>|<span data-ttu-id="db883-365">Választható.</span><span class="sxs-lookup"><span data-stu-id="db883-365">Optional.</span></span> <span data-ttu-id="db883-366">Ennyi hello maximális fájl rendszerhez szükséges tárhelyet, amelyet a hello megadott adatok.</span><span class="sxs-lookup"><span data-stu-id="db883-366">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="db883-367">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="db883-367">hello default is 0.</span></span>|  
|<span data-ttu-id="db883-368">**scheduledTransferLogLevelFilterr**</span><span class="sxs-lookup"><span data-stu-id="db883-368">**scheduledTransferLogLevelFilterr**</span></span>|<span data-ttu-id="db883-369">**karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="db883-369">**string**</span></span>|<span data-ttu-id="db883-370">Választható.</span><span class="sxs-lookup"><span data-stu-id="db883-370">Optional.</span></span> <span data-ttu-id="db883-371">Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje.</span><span class="sxs-lookup"><span data-stu-id="db883-371">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="db883-372">hello alapértelmezett értéke **meghatározatlan**, amely továbbítja az összes naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="db883-372">hello default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="db883-373">Más lehetséges értékek (a legtöbb tooleast információt sorrendben): **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **Kritikus**.</span><span class="sxs-lookup"><span data-stu-id="db883-373">Other possible values (in order of most tooleast information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="db883-374">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="db883-374">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="db883-375">**időtartam**</span><span class="sxs-lookup"><span data-stu-id="db883-375">**duration**</span></span>|<span data-ttu-id="db883-376">Választható.</span><span class="sxs-lookup"><span data-stu-id="db883-376">Optional.</span></span> <span data-ttu-id="db883-377">Hello időköze ütemezett átvitelek adatok felfelé toohello legközelebbi perc között.</span><span class="sxs-lookup"><span data-stu-id="db883-377">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="db883-378">hello alapértelmezés szerint PT0S.</span><span class="sxs-lookup"><span data-stu-id="db883-378">hello default is PT0S.</span></span>|  
|<span data-ttu-id="db883-379">**fogadók esetében** 1.5-ös hozzáadva</span><span class="sxs-lookup"><span data-stu-id="db883-379">**sinks** Added in 1.5</span></span>|<span data-ttu-id="db883-380">**karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="db883-380">**string**</span></span>|<span data-ttu-id="db883-381">Választható.</span><span class="sxs-lookup"><span data-stu-id="db883-381">Optional.</span></span> <span data-ttu-id="db883-382">Pontok tooa a fogadó hely tooalso diagnosztikai adatok küldése.</span><span class="sxs-lookup"><span data-stu-id="db883-382">Points tooa sink location tooalso send diagnostic data.</span></span> <span data-ttu-id="db883-383">Például az Application Insights.</span><span class="sxs-lookup"><span data-stu-id="db883-383">For example, Application Insights.</span></span>|  

## <a name="dockersources"></a><span data-ttu-id="db883-384">DockerSources</span><span class="sxs-lookup"><span data-stu-id="db883-384">DockerSources</span></span>
 <span data-ttu-id="db883-385">*Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*</span><span class="sxs-lookup"><span data-stu-id="db883-385">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*</span></span>

 <span data-ttu-id="db883-386">1.9 felvételére.</span><span class="sxs-lookup"><span data-stu-id="db883-386">Added in 1.9.</span></span>

|<span data-ttu-id="db883-387">Elem neve</span><span class="sxs-lookup"><span data-stu-id="db883-387">Element Name</span></span>|<span data-ttu-id="db883-388">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-388">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="db883-389">**Statisztikák**</span><span class="sxs-lookup"><span data-stu-id="db883-389">**Stats**</span></span>|<span data-ttu-id="db883-390">Hello rendszer közli a Docker-tároló toocollect statisztikái</span><span class="sxs-lookup"><span data-stu-id="db883-390">Tells hello system toocollect stats for Docker containers</span></span>|  

## <a name="sinksconfig-element"></a><span data-ttu-id="db883-391">SinksConfig elem</span><span class="sxs-lookup"><span data-stu-id="db883-391">SinksConfig Element</span></span>  
 <span data-ttu-id="db883-392">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*</span><span class="sxs-lookup"><span data-stu-id="db883-392">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*</span></span>

 <span data-ttu-id="db883-393">Helyek toosend diagnosztikai adatok tooand hello konfigurációja azokon a helyeken társított listáját.</span><span class="sxs-lookup"><span data-stu-id="db883-393">A list of locations toosend diagnostics data tooand hello configuration associated with those locations.</span></span>  

|<span data-ttu-id="db883-394">Elem neve</span><span class="sxs-lookup"><span data-stu-id="db883-394">Element Name</span></span>|<span data-ttu-id="db883-395">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-395">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="db883-396">**A fogadó**</span><span class="sxs-lookup"><span data-stu-id="db883-396">**Sink**</span></span>|<span data-ttu-id="db883-397">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-397">See description elsewhere on this page.</span></span>|  

## <a name="sink-element"></a><span data-ttu-id="db883-398">Elem gyűjtése</span><span class="sxs-lookup"><span data-stu-id="db883-398">Sink Element</span></span>
 <span data-ttu-id="db883-399">*Fa: A gyökérkönyvtár - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - fogadó*</span><span class="sxs-lookup"><span data-stu-id="db883-399">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink*</span></span>

 <span data-ttu-id="db883-400">1.5-ös verziójának felvételére.</span><span class="sxs-lookup"><span data-stu-id="db883-400">Added in version 1.5.</span></span>  

 <span data-ttu-id="db883-401">Határozza meg a helyek toosend diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="db883-401">Defines locations toosend diagnostic data to.</span></span> <span data-ttu-id="db883-402">Például hello Application Insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="db883-402">For example, hello Application Insights service.</span></span>  

|<span data-ttu-id="db883-403">Attribútum</span><span class="sxs-lookup"><span data-stu-id="db883-403">Attribute</span></span>|<span data-ttu-id="db883-404">Típus</span><span class="sxs-lookup"><span data-stu-id="db883-404">Type</span></span>|<span data-ttu-id="db883-405">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-405">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="db883-406">**név**</span><span class="sxs-lookup"><span data-stu-id="db883-406">**name**</span></span>|<span data-ttu-id="db883-407">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db883-407">string</span></span>|<span data-ttu-id="db883-408">Egy karakterlánc azonosító hello sinkname.</span><span class="sxs-lookup"><span data-stu-id="db883-408">A string identifying hello sinkname.</span></span>|  

|<span data-ttu-id="db883-409">Elem</span><span class="sxs-lookup"><span data-stu-id="db883-409">Element</span></span>|<span data-ttu-id="db883-410">Típus</span><span class="sxs-lookup"><span data-stu-id="db883-410">Type</span></span>|<span data-ttu-id="db883-411">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-411">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="db883-412">**Application Insights**</span><span class="sxs-lookup"><span data-stu-id="db883-412">**Application Insights**</span></span>|<span data-ttu-id="db883-413">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db883-413">string</span></span>|<span data-ttu-id="db883-414">Csak küldött használt adatok tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="db883-414">Used only when sending data tooApplication Insights.</span></span> <span data-ttu-id="db883-415">Hello hozzáféréssel rendelkező aktív Application Insights-fiók kulcsát Instrumentation tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="db883-415">Contain hello Instrumentation Key for an active Application Insights account that you have access to.</span></span>|  
|<span data-ttu-id="db883-416">**Csatornák**</span><span class="sxs-lookup"><span data-stu-id="db883-416">**Channels**</span></span>|<span data-ttu-id="db883-417">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db883-417">string</span></span>|<span data-ttu-id="db883-418">Minden további szűréséhez, hogy adatfolyamként küldje el, amikor egy</span><span class="sxs-lookup"><span data-stu-id="db883-418">One for each additional filtering that stream that you</span></span>|  

## <a name="channels-element"></a><span data-ttu-id="db883-419">Csatornák elem</span><span class="sxs-lookup"><span data-stu-id="db883-419">Channels Element</span></span>  
 <span data-ttu-id="db883-420">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - fogadó - csatornák*</span><span class="sxs-lookup"><span data-stu-id="db883-420">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels*</span></span>

 <span data-ttu-id="db883-421">1.5-ös verziójának felvételére.</span><span class="sxs-lookup"><span data-stu-id="db883-421">Added in version 1.5.</span></span>  

 <span data-ttu-id="db883-422">Határozza meg a naplózási adatokat, hogy át kellene haladnia egy fogadó adatfolyamokat szűrőket.</span><span class="sxs-lookup"><span data-stu-id="db883-422">Defines filters for streams of log data passing through a sink.</span></span>  

|<span data-ttu-id="db883-423">Elem</span><span class="sxs-lookup"><span data-stu-id="db883-423">Element</span></span>|<span data-ttu-id="db883-424">Típus</span><span class="sxs-lookup"><span data-stu-id="db883-424">Type</span></span>|<span data-ttu-id="db883-425">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-425">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="db883-426">**Csatorna**</span><span class="sxs-lookup"><span data-stu-id="db883-426">**Channel**</span></span>|<span data-ttu-id="db883-427">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="db883-427">string</span></span>|<span data-ttu-id="db883-428">Ezen az oldalon máshol lásd a leírást.</span><span class="sxs-lookup"><span data-stu-id="db883-428">See description elsewhere on this page.</span></span>|  

## <a name="channel-element"></a><span data-ttu-id="db883-429">Csatorna elem</span><span class="sxs-lookup"><span data-stu-id="db883-429">Channel Element</span></span>
 <span data-ttu-id="db883-430">*Fa: Gyökér - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - fogadó - csatornák - csatorna*</span><span class="sxs-lookup"><span data-stu-id="db883-430">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels - Channel*</span></span>

 <span data-ttu-id="db883-431">1.5-ös verziójának felvételére.</span><span class="sxs-lookup"><span data-stu-id="db883-431">Added in version 1.5.</span></span>  

 <span data-ttu-id="db883-432">Határozza meg a helyek toosend diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="db883-432">Defines locations toosend diagnostic data to.</span></span> <span data-ttu-id="db883-433">Például hello Application Insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="db883-433">For example, hello Application Insights service.</span></span>  

|<span data-ttu-id="db883-434">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="db883-434">Attributes</span></span>|<span data-ttu-id="db883-435">Típus</span><span class="sxs-lookup"><span data-stu-id="db883-435">Type</span></span>|<span data-ttu-id="db883-436">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-436">Description</span></span>|  
|----------------|----------|-----------------|  
|<span data-ttu-id="db883-437">**logLevel**</span><span class="sxs-lookup"><span data-stu-id="db883-437">**logLevel**</span></span>|<span data-ttu-id="db883-438">**karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="db883-438">**string**</span></span>|<span data-ttu-id="db883-439">Adja meg, amelyeket a naplóbejegyzések hello minimális súlyossági szintje.</span><span class="sxs-lookup"><span data-stu-id="db883-439">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="db883-440">hello alapértelmezett értéke **meghatározatlan**, amely továbbítja az összes naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="db883-440">hello default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="db883-441">Más lehetséges értékek (a legtöbb tooleast információt sorrendben): **részletes**, **információk**, **figyelmeztetés**, **hiba**, és **Kritikus**.</span><span class="sxs-lookup"><span data-stu-id="db883-441">Other possible values (in order of most tooleast information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="db883-442">**név**</span><span class="sxs-lookup"><span data-stu-id="db883-442">**name**</span></span>|<span data-ttu-id="db883-443">**karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="db883-443">**string**</span></span>|<span data-ttu-id="db883-444">Egy egyedi nevet a hello csatorna toorefer</span><span class="sxs-lookup"><span data-stu-id="db883-444">A unique name of hello channel toorefer to</span></span>|  


## <a name="privateconfig-element"></a><span data-ttu-id="db883-445">PrivateConfig elem</span><span class="sxs-lookup"><span data-stu-id="db883-445">PrivateConfig Element</span></span> 
 <span data-ttu-id="db883-446">*Fa: Gyökér - DiagnosticsConfiguration - PrivateConfig*</span><span class="sxs-lookup"><span data-stu-id="db883-446">*Tree: Root - DiagnosticsConfiguration - PrivateConfig*</span></span>

 <span data-ttu-id="db883-447">1.3-as verzió felvételére.</span><span class="sxs-lookup"><span data-stu-id="db883-447">Added in version 1.3.</span></span>  

 <span data-ttu-id="db883-448">Optional</span><span class="sxs-lookup"><span data-stu-id="db883-448">Optional</span></span>  

 <span data-ttu-id="db883-449">Személyes adatait hello hello storage-fiók (a nevét, a kulcs és a végpont) tárolja.</span><span class="sxs-lookup"><span data-stu-id="db883-449">Stores hello private details of hello storage account (name, key, and endpoint).</span></span> <span data-ttu-id="db883-450">Ezt az információt toohello virtuális gép küldött, de nem kérhetők le azt.</span><span class="sxs-lookup"><span data-stu-id="db883-450">This information is sent toohello virtual machine, but cannot be retrieved from it.</span></span>  

|<span data-ttu-id="db883-451">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="db883-451">Child Elements</span></span>|<span data-ttu-id="db883-452">Leírás</span><span class="sxs-lookup"><span data-stu-id="db883-452">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="db883-453">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="db883-453">**StorageAccount**</span></span>|<span data-ttu-id="db883-454">hello tárolási fiók toouse.</span><span class="sxs-lookup"><span data-stu-id="db883-454">hello storage account toouse.</span></span> <span data-ttu-id="db883-455">a következő attribútumok hello szükségesek.</span><span class="sxs-lookup"><span data-stu-id="db883-455">hello following attributes are required</span></span><br /><br /> <span data-ttu-id="db883-456">- **név** – hello hello storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="db883-456">- **name** - hello name of hello storage account.</span></span><br /><br /> <span data-ttu-id="db883-457">- **kulcs** – hello toohello storage-fiók kulcsát.</span><span class="sxs-lookup"><span data-stu-id="db883-457">- **key** - hello key toohello storage account.</span></span><br /><br /> <span data-ttu-id="db883-458">- **végpont** -hello végpont tooaccess hello storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="db883-458">- **endpoint** - hello endpoint tooaccess hello storage account.</span></span> <br /><br /> <span data-ttu-id="db883-459">-**sasToken** (hozzáadott 1.8.1)-hello titkos config is megadhat egy SAS-token helyett a tárfiók kulcsára. Ha adott, a rendszer figyelmen kívül hagyja hello tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="db883-459">-**sasToken** (added 1.8.1)- You can specify an SAS token instead of a storage account key in hello private config. If provided, hello storage account key is ignored.</span></span> <br /><span data-ttu-id="db883-460">SAS-Token hello vonatkozó követelmények:</span><span class="sxs-lookup"><span data-stu-id="db883-460">Requirements for hello SAS Token:</span></span> <br /><span data-ttu-id="db883-461">-Fiók SAS-jogkivonat csak támogatja</span><span class="sxs-lookup"><span data-stu-id="db883-461">- Supports account SAS token only</span></span> <br /><span data-ttu-id="db883-462">- *b*, *t* szolgáltatástípusok szükség.</span><span class="sxs-lookup"><span data-stu-id="db883-462">- *b*, *t* service types are required.</span></span> <br /> <span data-ttu-id="db883-463">- *egy*, *c*, *u*, *w* engedélyekre szükség.</span><span class="sxs-lookup"><span data-stu-id="db883-463">- *a*, *c*, *u*, *w* permissions are required.</span></span> <br /> <span data-ttu-id="db883-464">- *c*, *o* típusú erőforrások szükségesek.</span><span class="sxs-lookup"><span data-stu-id="db883-464">- *c*, *o* resource types are required.</span></span> <br /> <span data-ttu-id="db883-465">-Hello HTTPS protokollt támogatja</span><span class="sxs-lookup"><span data-stu-id="db883-465">- Supports hello HTTPS protocol only</span></span> <br /> <span data-ttu-id="db883-466">-Elindításához és a lejárati időpont érvényesnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="db883-466">- Start and expiry time must be valid.</span></span>|  


## <a name="isenabled-element"></a><span data-ttu-id="db883-467">IsEnabled elem</span><span class="sxs-lookup"><span data-stu-id="db883-467">IsEnabled Element</span></span>  
 <span data-ttu-id="db883-468">*Fa: Gyökér - DiagnosticsConfiguration - IsEnabled*</span><span class="sxs-lookup"><span data-stu-id="db883-468">*Tree: Root - DiagnosticsConfiguration - IsEnabled*</span></span>

 <span data-ttu-id="db883-469">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="db883-469">Boolean.</span></span> <span data-ttu-id="db883-470">Használjon `true` tooenable hello diagnosztika vagy `false` toodisable hello diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="db883-470">Use `true` tooenable hello diagnostics or `false` toodisable hello diagnostics.</span></span>
