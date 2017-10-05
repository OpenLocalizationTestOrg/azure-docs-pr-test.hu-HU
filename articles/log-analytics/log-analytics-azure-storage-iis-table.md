---
title: "A blob storage használata az Azure Naplóelemzés eseményeket az IIS és a tábla tárolási |} Microsoft Docs"
description: "A Naplóelemzési olvashatja a naplók az Azure-szolgáltatásokat, hogy az írási diagnostics meg tudja a table storage vagy a blob-tároló írni az IIS-naplókba."
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
ms.openlocfilehash: 459ef90ca1d76bada6565bfefd7b4bd1086197d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a><span data-ttu-id="318e4-103">Az IIS és az Azure Naplóelemzés eseményeit a table storage Azure blob storage használata</span><span class="sxs-lookup"><span data-stu-id="318e4-103">Use Azure blob storage for IIS and Azure table storage for events with Log Analytics</span></span>

<span data-ttu-id="318e4-104">A Naplóelemzési a keresse meg a következő szolgáltatásokat, hogy az írási diagnostics meg tudja a table storage vagy a blob-tároló írni az IIS-napló olvashatja:</span><span class="sxs-lookup"><span data-stu-id="318e4-104">Log Analytics can read the logs for the following services that write diagnostics to table storage or IIS logs written to blob storage:</span></span>

* <span data-ttu-id="318e4-105">A Service Fabric-fürtök (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="318e4-105">Service Fabric clusters (Preview)</span></span>
* <span data-ttu-id="318e4-106">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="318e4-106">Virtual Machines</span></span>
* <span data-ttu-id="318e4-107">Webes vagy feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="318e4-107">Web/Worker Roles</span></span>

<span data-ttu-id="318e4-108">A Naplóelemzési ezekhez az erőforrásokhoz tartozó adatokat gyűjthet, mielőtt Azure diagnostics engedélyezve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="318e4-108">Before Log Analytics can collect data for these resources, Azure diagnostics must be enabled.</span></span>

<span data-ttu-id="318e4-109">Ha diagnosztikai engedélyezve vannak, használhatja az Azure-portálon, vagy PowerShell Naplóelemzési a naplók gyűjtéséhez konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="318e4-109">Once diagnostics are enabled, you can use the Azure portal or PowerShell configure Log Analytics to collect the logs.</span></span>

<span data-ttu-id="318e4-110">Az Azure Diagnostics egy Azure-bővítményt, amely lehetővé teszi a feldolgozói szerepkör, a webes szerepkör vagy az Azure-beli virtuális gép diagnosztikai adatainak összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="318e4-110">Azure Diagnostics is an Azure extension that enables you to collect diagnostic data from a worker role, web role, or virtual machine running in Azure.</span></span> <span data-ttu-id="318e4-111">Az adatok Azure storage-fiók tárolva van, és ezután gyűjthetők a Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="318e4-111">The data is stored in an Azure storage account and can then be collected by Log Analytics.</span></span>

<span data-ttu-id="318e4-112">A Naplóelemzési, ezek az Azure diagnosztikai naplók gyűjtésére a naplók kell a következő helyeken:</span><span class="sxs-lookup"><span data-stu-id="318e4-112">For Log Analytics to collect these Azure Diagnostics logs, the logs must be in the following locations:</span></span>

| <span data-ttu-id="318e4-113">Napló típusa</span><span class="sxs-lookup"><span data-stu-id="318e4-113">Log Type</span></span> | <span data-ttu-id="318e4-114">Erőforrás típusa</span><span class="sxs-lookup"><span data-stu-id="318e4-114">Resource Type</span></span> | <span data-ttu-id="318e4-115">Hely</span><span class="sxs-lookup"><span data-stu-id="318e4-115">Location</span></span> |
| --- | --- | --- |
| <span data-ttu-id="318e4-116">IIS-naplók</span><span class="sxs-lookup"><span data-stu-id="318e4-116">IIS logs</span></span> |<span data-ttu-id="318e4-117">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="318e4-117">Virtual Machines</span></span> <br> <span data-ttu-id="318e4-118">Webes szerepkörök</span><span class="sxs-lookup"><span data-stu-id="318e4-118">Web roles</span></span> <br> <span data-ttu-id="318e4-119">Feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="318e4-119">Worker roles</span></span> |<span data-ttu-id="318e4-120">üvegvatta-az iis-naplófájlok (Blob-tároló)</span><span class="sxs-lookup"><span data-stu-id="318e4-120">wad-iis-logfiles (Blob Storage)</span></span> |
| <span data-ttu-id="318e4-121">Rendszernapló</span><span class="sxs-lookup"><span data-stu-id="318e4-121">Syslog</span></span> |<span data-ttu-id="318e4-122">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="318e4-122">Virtual Machines</span></span> |<span data-ttu-id="318e4-123">LinuxsyslogVer2v0 (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="318e4-123">LinuxsyslogVer2v0 (Table Storage)</span></span> |
| <span data-ttu-id="318e4-124">Service Fabric működési események</span><span class="sxs-lookup"><span data-stu-id="318e4-124">Service Fabric Operational Events</span></span> |<span data-ttu-id="318e4-125">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="318e4-125">Service Fabric nodes</span></span> |<span data-ttu-id="318e4-126">WADServiceFabricSystemEventTable</span><span class="sxs-lookup"><span data-stu-id="318e4-126">WADServiceFabricSystemEventTable</span></span> |
| <span data-ttu-id="318e4-127">Service Fabric megbízható szereplő események</span><span class="sxs-lookup"><span data-stu-id="318e4-127">Service Fabric Reliable Actor Events</span></span> |<span data-ttu-id="318e4-128">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="318e4-128">Service Fabric nodes</span></span> |<span data-ttu-id="318e4-129">WADServiceFabricReliableActorEventTable</span><span class="sxs-lookup"><span data-stu-id="318e4-129">WADServiceFabricReliableActorEventTable</span></span> |
| <span data-ttu-id="318e4-130">Service Fabric megbízható eseményei</span><span class="sxs-lookup"><span data-stu-id="318e4-130">Service Fabric Reliable Service Events</span></span> |<span data-ttu-id="318e4-131">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="318e4-131">Service Fabric nodes</span></span> |<span data-ttu-id="318e4-132">WADServiceFabricReliableServiceEventTable</span><span class="sxs-lookup"><span data-stu-id="318e4-132">WADServiceFabricReliableServiceEventTable</span></span> |
| <span data-ttu-id="318e4-133">Windows-Eseménynapló</span><span class="sxs-lookup"><span data-stu-id="318e4-133">Windows Event logs</span></span> |<span data-ttu-id="318e4-134">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="318e4-134">Service Fabric nodes</span></span> <br> <span data-ttu-id="318e4-135">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="318e4-135">Virtual Machines</span></span> <br> <span data-ttu-id="318e4-136">Webes szerepkörök</span><span class="sxs-lookup"><span data-stu-id="318e4-136">Web roles</span></span> <br> <span data-ttu-id="318e4-137">Feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="318e4-137">Worker roles</span></span> |<span data-ttu-id="318e4-138">WADWindowsEventLogsTable (Table-tároló)</span><span class="sxs-lookup"><span data-stu-id="318e4-138">WADWindowsEventLogsTable (Table Storage)</span></span> |
| <span data-ttu-id="318e4-139">A Windows ETW-naplók</span><span class="sxs-lookup"><span data-stu-id="318e4-139">Windows ETW logs</span></span> |<span data-ttu-id="318e4-140">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="318e4-140">Service Fabric nodes</span></span> <br> <span data-ttu-id="318e4-141">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="318e4-141">Virtual Machines</span></span> <br> <span data-ttu-id="318e4-142">Webes szerepkörök</span><span class="sxs-lookup"><span data-stu-id="318e4-142">Web roles</span></span> <br> <span data-ttu-id="318e4-143">Feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="318e4-143">Worker roles</span></span> |<span data-ttu-id="318e4-144">WADETWEventTable (Table-tároló)</span><span class="sxs-lookup"><span data-stu-id="318e4-144">WADETWEventTable (Table Storage)</span></span> |

> [!NOTE]
> <span data-ttu-id="318e4-145">Az Azure-webhelyek IIS-napló jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="318e4-145">IIS logs from Azure Websites are not currently supported.</span></span>
>
>

<span data-ttu-id="318e4-146">A virtuális gépek, lehetősége van telepíteni a [Naplóelemzési ügynök](log-analytics-azure-vm-extension.md) ahhoz, hogy további betekintést nyerjen a virtuális géppé.</span><span class="sxs-lookup"><span data-stu-id="318e4-146">For virtual machines, you have the option of installing the [Log Analytics agent](log-analytics-azure-vm-extension.md) into your virtual machine to enable additional insights.</span></span> <span data-ttu-id="318e4-147">Nem csak az IIS naplókat és az eseménynaplók elemzése, többek között a konfigurációs változások követését, az SQL értékelése és a frissítések értékelését további elemzés végezheti el.</span><span class="sxs-lookup"><span data-stu-id="318e4-147">In addition to being able to analyze IIS logs and Event Logs, you can perform additional analysis including configuration change tracking, SQL assessment, and update assessment.</span></span>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a><span data-ttu-id="318e4-148">A virtuális gép Azure diagnostics engedélyezése az eseménynaplót, és az IIS naplózása gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="318e4-148">Enable Azure diagnostics in a virtual machine for event log and IIS log collection</span></span>
<span data-ttu-id="318e4-149">A következő eljárással engedélyezheti a virtuális gépen a Microsoft Azure portál használatával Eseménynapló és az IIS-napló gyűjtemény Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="318e4-149">Use the following procedure to enable Azure diagnostics in a virtual machine for Event Log and IIS log collection using the Microsoft Azure portal.</span></span>

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a><span data-ttu-id="318e4-150">Ahhoz, hogy a virtuális gépen, és az Azure portál Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="318e4-150">To enable Azure diagnostics in a virtual machine with the Azure portal</span></span>
1. <span data-ttu-id="318e4-151">A Virtuálisgép-ügynök telepítése a virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="318e4-151">Install the VM Agent when you create a virtual machine.</span></span> <span data-ttu-id="318e4-152">Ha a virtuális gép már létezik, győződjön meg arról, hogy a Virtuálisgép-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="318e4-152">If the virtual machine already exists, verify that the VM Agent is already installed.</span></span>

   * <span data-ttu-id="318e4-153">A virtuális gépet, jelölje be az Azure-portálon lépjen **opcionális konfigurációs**, majd **diagnosztika** és **állapota** a **a**.</span><span class="sxs-lookup"><span data-stu-id="318e4-153">In the Azure portal, navigate to the virtual machine, select **Optional Configuration**, then **Diagnostics** and set **Status** to **On**.</span></span>

     <span data-ttu-id="318e4-154">Létrehozása után a virtuális gép kiterjesztése az Azure Diagnostics telepítve és fut.</span><span class="sxs-lookup"><span data-stu-id="318e4-154">Upon completion, the VM has the Azure Diagnostics extension installed and running.</span></span> <span data-ttu-id="318e4-155">A bővítmény felelős a diagnosztikai adatainak összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="318e4-155">This extension is responsible for collecting your diagnostics data.</span></span>
2. <span data-ttu-id="318e4-156">Engedélyezze a megfigyelést és a meglévő virtuális az eseménynaplózás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="318e4-156">Enable monitoring and configure event logging on an existing VM.</span></span> <span data-ttu-id="318e4-157">Diagnosztika a virtuális gép szintjén engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="318e4-157">You can enable diagnostics at the VM level.</span></span> <span data-ttu-id="318e4-158">Diagnosztika engedélyezése, majd válassza az eseménynaplózás, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="318e4-158">To enable diagnostics and then configure event logging, perform the following steps:</span></span>

   1. <span data-ttu-id="318e4-159">Válassza ki a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="318e4-159">Select the VM.</span></span>
   2. <span data-ttu-id="318e4-160">Kattintson a **figyelési**.</span><span class="sxs-lookup"><span data-stu-id="318e4-160">Click **Monitoring**.</span></span>
   3. <span data-ttu-id="318e4-161">Kattintson a **diagnosztika**.</span><span class="sxs-lookup"><span data-stu-id="318e4-161">Click **Diagnostics**.</span></span>
   4. <span data-ttu-id="318e4-162">Állítsa be a **állapot** való **ON**.</span><span class="sxs-lookup"><span data-stu-id="318e4-162">Set the **Status** to **ON**.</span></span>
   5. <span data-ttu-id="318e4-163">Válassza ki a gyűjteni kívánt diagnosztikai naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="318e4-163">Select each diagnostics log that you want to collect.</span></span>
   6. <span data-ttu-id="318e4-164">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="318e4-164">Click **OK**.</span></span>

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a><span data-ttu-id="318e4-165">Az IIS-napló és a esemény gyűjteményhez webes szerepkörrel rendelkező Azure diagnostics engedélyezése</span><span class="sxs-lookup"><span data-stu-id="318e4-165">Enable Azure diagnostics in a Web role for IIS log and event collection</span></span>
<span data-ttu-id="318e4-166">Tekintse meg [hogyan való engedélyezése diagnosztikai felhőszolgáltatásban](../cloud-services/cloud-services-dotnet-diagnostics.md) általános lépései az Azure diagnostics engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="318e4-166">Refer to [How To Enable Diagnostics in a Cloud Service](../cloud-services/cloud-services-dotnet-diagnostics.md) for general steps on enabling Azure diagnostics.</span></span> <span data-ttu-id="318e4-167">Az alábbi utasításokat ezekkel az adatokkal, és szabja testre a Log Analyticshez való használatra.</span><span class="sxs-lookup"><span data-stu-id="318e4-167">The instructions below use this information and customize it for use with Log Analytics.</span></span>

<span data-ttu-id="318e4-168">Az Azure diagnostics engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="318e4-168">With Azure diagnostics enabled:</span></span>

* <span data-ttu-id="318e4-169">IIS-napló alapértelmezés szerint a scheduledTransferPeriod átviteli időközönként átvitt naplóadatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="318e4-169">IIS logs are stored by default, with log data transferred at the scheduledTransferPeriod transfer interval.</span></span>
* <span data-ttu-id="318e4-170">Alapértelmezés szerint a Windows-Eseménynapló nem kerülnek.</span><span class="sxs-lookup"><span data-stu-id="318e4-170">Windows Event Logs are not transferred by default.</span></span>

### <a name="to-enable-diagnostics"></a><span data-ttu-id="318e4-171">Diagnosztika engedélyezése</span><span class="sxs-lookup"><span data-stu-id="318e4-171">To enable diagnostics</span></span>
<span data-ttu-id="318e4-172">Engedélyezése a Windows eseménynaplóiban keresse meg vagy módosíthatja a scheduledTransferPeriod, állítsa be az XML konfigurációs fájl (diagnostics.wadcfg), Azure-diagnosztika látható módon [4. lépés: a diagnosztika konfigurációs fájl létrehozása és a kiterjesztés telepítése](../cloud-services/cloud-services-dotnet-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="318e4-172">To enable Windows Event Logs, or to change the scheduledTransferPeriod, configure Azure Diagnostics using the XML configuration file (diagnostics.wadcfg), as shown in [Step 4: Create your Diagnostics configuration file and install the extension](../cloud-services/cloud-services-dotnet-diagnostics.md)</span></span>

<span data-ttu-id="318e4-173">A következő példa konfigurációs fájlt az IIS naplókat és az összes eseményt gyűjti az alkalmazás és rendszer kategóriájának bejegyzéseit:</span><span class="sxs-lookup"><span data-stu-id="318e4-173">The following example configuration file collects IIS Logs and all Events from the Application and System logs:</span></span>

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
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

<span data-ttu-id="318e4-174">Győződjön meg arról, hogy a ConfigurationSettings határozza meg a storage-fiók, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="318e4-174">Ensure that your ConfigurationSettings specifies a storage account, as in the following example:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

<span data-ttu-id="318e4-175">A **AccountName** és **AccountKey** értékek találhatók, a tárolási fiók Tárelérési kulcsok kezelése irányítópulton Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="318e4-175">The **AccountName** and **AccountKey** values are found in the Azure portal in the storage account dashboard, under Manage Access Keys.</span></span> <span data-ttu-id="318e4-176">A kapcsolati karakterlánc protokollja kell **https**.</span><span class="sxs-lookup"><span data-stu-id="318e4-176">The protocol for the connection string must be **https**.</span></span>

<span data-ttu-id="318e4-177">Miután a frissített diagnosztikai konfiguráció alkalmazása a felhőalapú szolgáltatáshoz és diagnosztika ír, az Azure-tárhelyre, majd készen áll Naplóelemzési konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="318e4-177">Once the updated diagnostic configuration is applied to your cloud service and it is writing diagnostics to Azure Storage, then you are ready to configure Log Analytics.</span></span>

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a><span data-ttu-id="318e4-178">Az Azure portál segítségével naplógyűjtéshez Azure Storage-ból</span><span class="sxs-lookup"><span data-stu-id="318e4-178">Use the Azure portal to collect logs from Azure Storage</span></span>
<span data-ttu-id="318e4-179">Az Azure portál segítségével konfigurálása az Azure-szolgáltatásokat a naplók összegyűjtésére Naplóelemzési:</span><span class="sxs-lookup"><span data-stu-id="318e4-179">You can use the Azure portal to configure Log Analytics to collect the logs for the following Azure services:</span></span>

* <span data-ttu-id="318e4-180">Service Fabric-fürtök</span><span class="sxs-lookup"><span data-stu-id="318e4-180">Service Fabric clusters</span></span>
* <span data-ttu-id="318e4-181">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="318e4-181">Virtual Machines</span></span>
* <span data-ttu-id="318e4-182">Webes vagy feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="318e4-182">Web/Worker Roles</span></span>

<span data-ttu-id="318e4-183">Az Azure portálon nyissa meg a Naplóelemzési munkaterületet, és hajtsa végre a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="318e4-183">In the Azure portal, navigate to your Log Analytics workspace and perform the following tasks:</span></span>

1. <span data-ttu-id="318e4-184">Kattintson a *tárfiókok naplók*</span><span class="sxs-lookup"><span data-stu-id="318e4-184">Click *Storage accounts logs*</span></span>
2. <span data-ttu-id="318e4-185">Kattintson a *Hozzáadás* feladat</span><span class="sxs-lookup"><span data-stu-id="318e4-185">Click the *Add* task</span></span>
3. <span data-ttu-id="318e4-186">Válassza ki a tárfiókot, amely tartalmazza a diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="318e4-186">Select the Storage account that contains the diagnostics logs</span></span>
   * <span data-ttu-id="318e4-187">Ez a fiók lehet egy hagyományos tárolási fiók vagy egy Azure Resource Manager storage-fiók</span><span class="sxs-lookup"><span data-stu-id="318e4-187">This account can be either a classic storage account or an Azure Resource Manager storage account</span></span>
4. <span data-ttu-id="318e4-188">Válassza ki az adattípust szeretne gyűjteni a naplókat</span><span class="sxs-lookup"><span data-stu-id="318e4-188">Select the Data Type you want to collect logs for</span></span>
   * <span data-ttu-id="318e4-189">A választási lehetőségek: IIS-naplóit; Az eseményeket; Syslog (Linux); ETW naplók; Service Fabric-események</span><span class="sxs-lookup"><span data-stu-id="318e4-189">The choices are IIS Logs; Events; Syslog (Linux); ETW Logs; Service Fabric Events</span></span>
5. <span data-ttu-id="318e4-190">Forrás értéket automatikusan kitölti a rendszer az adatok alapján, és nem módosítható</span><span class="sxs-lookup"><span data-stu-id="318e4-190">The value for Source is automatically populated based on the data type and cannot be changed</span></span>
6. <span data-ttu-id="318e4-191">Kattintson az OK gombra a konfiguráció mentéséhez</span><span class="sxs-lookup"><span data-stu-id="318e4-191">Click OK to save the configuration</span></span>

<span data-ttu-id="318e4-192">A további tárfiókok és a Naplóelemzési gyűjtéséhez kívánt adattípusok ismételje meg a 2 – 6.</span><span class="sxs-lookup"><span data-stu-id="318e4-192">Repeat steps 2-6 for additional storage accounts and data types that you want Log Analytics to collect.</span></span>

<span data-ttu-id="318e4-193">A körülbelül 30 percet is a a Naplóelemzési tárkonfigurációt adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="318e4-193">In approximately 30 minutes, you are able to see data from the storage account in Log Analytics.</span></span> <span data-ttu-id="318e4-194">Miután a konfiguráció alkalmazása Storage írt adatok csak akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="318e4-194">You will only see data that is written to storage after the configuration is applied.</span></span> <span data-ttu-id="318e4-195">A Naplóelemzési nem olvassa a már létező adatokat a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="318e4-195">Log Analytics does not read the pre-existing data from the storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="318e4-196">A portál nem ellenőrzi, hogy a forrás létezik-e a tárfiókban lévő, vagy ha új adatok írása.</span><span class="sxs-lookup"><span data-stu-id="318e4-196">The portal does not validate that the Source exists in the storage account or if new data is being written.</span></span>
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a><span data-ttu-id="318e4-197">A virtuális gép Azure diagnostics engedélyezése az eseménynaplót, és IIS töltsék PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="318e4-197">Enable Azure diagnostics in a virtual machine for event log and IIS log collection using PowerShell</span></span>
<span data-ttu-id="318e4-198">Az alábbi témakörben található lépésekkel [Naplóelemzési konfigurálása az Azure diagnostics indexelésre](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) lehet olvasni a table storage írt Azure diagnostics a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="318e4-198">Use the steps in [Configuring Log Analytics to index Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) to use PowerShell to read from Azure diagnostics that are written to table storage.</span></span>

<span data-ttu-id="318e4-199">Azure PowerShell használatával pontosabban megadhatja az Azure Storage írt események.</span><span class="sxs-lookup"><span data-stu-id="318e4-199">Using Azure PowerShell you can more precisely specify the events that are written to Azure Storage.</span></span>
<span data-ttu-id="318e4-200">További információkért lásd: [diagnosztika engedélyezésével az Azure virtuális gépek](../virtual-machines-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="318e4-200">For more information, see [Enabling Diagnostics in Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="318e4-201">Engedélyezi, és frissítse az Azure diagnostics a következő PowerShell-parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="318e4-201">You can enable and update Azure diagnostics using the following PowerShell script.</span></span>
<span data-ttu-id="318e4-202">Ez a parancsfájl egy egyéni naplózási konfiguráció is használható.</span><span class="sxs-lookup"><span data-stu-id="318e4-202">You can also use this script with a custom logging configuration.</span></span>
<span data-ttu-id="318e4-203">Módosítsa a parancsfájlt úgy állítsa be a tárfiók, a szolgáltatás neve és a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="318e4-203">Modify the script to set the storage account, service name, and virtual machine name.</span></span>
<span data-ttu-id="318e4-204">A parancsfájl parancsmagok a klasszikus virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="318e4-204">The script uses cmdlets for classic virtual machines.</span></span>

<span data-ttu-id="318e4-205">Tekintse át az alábbi parancsfájl-mintában, másolja, szükség szerint módosítsa a minta elmentse egy olyan PowerShell-parancsfájlt, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="318e4-205">Review the following script sample, copy it, modify it as needed, save the sample as a PowerShell script file, and then run the script.</span></span>

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

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
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a><span data-ttu-id="318e4-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="318e4-206">Next steps</span></span>
* <span data-ttu-id="318e4-207">[Gyűjteni a naplókat és az Azure-szolgáltatások metrikáját](log-analytics-azure-storage.md) támogatott Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="318e4-207">[Collect logs and metrics for Azure services](log-analytics-azure-storage.md) for supported Azure services.</span></span>
* <span data-ttu-id="318e4-208">[Megoldások](log-analytics-add-solutions.md) betekintést az adatokba, így.</span><span class="sxs-lookup"><span data-stu-id="318e4-208">[Enable Solutions](log-analytics-add-solutions.md) to provide insight into the data.</span></span>
* <span data-ttu-id="318e4-209">[Használja a keresési lekérdezések](log-analytics-log-searches.md) az adatok elemzésére.</span><span class="sxs-lookup"><span data-stu-id="318e4-209">[Use search queries](log-analytics-log-searches.md) to analyze the data.</span></span>
