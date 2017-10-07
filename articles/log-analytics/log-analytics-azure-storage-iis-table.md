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
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a><span data-ttu-id="38499-103">Az IIS és az Azure Naplóelemzés eseményeit a table storage Azure blob storage használata</span><span class="sxs-lookup"><span data-stu-id="38499-103">Use Azure blob storage for IIS and Azure table storage for events with Log Analytics</span></span>

<span data-ttu-id="38499-104">A Naplóelemzési hello naplókat, hogy az írási tootable diagnosztika tároló- és IIS naplózza az írásbeli tooblob tárolási szolgáltatások a következő hello olvashatja:</span><span class="sxs-lookup"><span data-stu-id="38499-104">Log Analytics can read hello logs for hello following services that write diagnostics tootable storage or IIS logs written tooblob storage:</span></span>

* <span data-ttu-id="38499-105">A Service Fabric-fürtök (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="38499-105">Service Fabric clusters (Preview)</span></span>
* <span data-ttu-id="38499-106">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="38499-106">Virtual Machines</span></span>
* <span data-ttu-id="38499-107">Webes vagy feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="38499-107">Web/Worker Roles</span></span>

<span data-ttu-id="38499-108">A Naplóelemzési ezekhez az erőforrásokhoz tartozó adatokat gyűjthet, mielőtt Azure diagnostics engedélyezve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="38499-108">Before Log Analytics can collect data for these resources, Azure diagnostics must be enabled.</span></span>

<span data-ttu-id="38499-109">Ha engedélyezve vannak a diagnosztika, hello Azure-portálon is használhatja, vagy PowerShell Naplóelemzési toocollect hello naplók konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="38499-109">Once diagnostics are enabled, you can use hello Azure portal or PowerShell configure Log Analytics toocollect hello logs.</span></span>

<span data-ttu-id="38499-110">Az Azure Diagnostics egy Azure-bővítményt, amely lehetővé teszi a feldolgozói szerepkör, a webes szerepkör vagy az Azure-beli virtuális gép diagnosztikai adatait toocollect.</span><span class="sxs-lookup"><span data-stu-id="38499-110">Azure Diagnostics is an Azure extension that enables you toocollect diagnostic data from a worker role, web role, or virtual machine running in Azure.</span></span> <span data-ttu-id="38499-111">hello adatok Azure storage-fiók tárolva van, és majd gyűjthetők a Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="38499-111">hello data is stored in an Azure storage account and can then be collected by Log Analytics.</span></span>

<span data-ttu-id="38499-112">A Naplóelemzési toocollect a Azure diagnosztikai naplók hello naplók kell az alábbi helyek hello:</span><span class="sxs-lookup"><span data-stu-id="38499-112">For Log Analytics toocollect these Azure Diagnostics logs, hello logs must be in hello following locations:</span></span>

| <span data-ttu-id="38499-113">Napló típusa</span><span class="sxs-lookup"><span data-stu-id="38499-113">Log Type</span></span> | <span data-ttu-id="38499-114">Erőforrás típusa</span><span class="sxs-lookup"><span data-stu-id="38499-114">Resource Type</span></span> | <span data-ttu-id="38499-115">Hely</span><span class="sxs-lookup"><span data-stu-id="38499-115">Location</span></span> |
| --- | --- | --- |
| <span data-ttu-id="38499-116">IIS-naplók</span><span class="sxs-lookup"><span data-stu-id="38499-116">IIS logs</span></span> |<span data-ttu-id="38499-117">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="38499-117">Virtual Machines</span></span> <br> <span data-ttu-id="38499-118">Webes szerepkörök</span><span class="sxs-lookup"><span data-stu-id="38499-118">Web roles</span></span> <br> <span data-ttu-id="38499-119">Feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="38499-119">Worker roles</span></span> |<span data-ttu-id="38499-120">üvegvatta-az iis-naplófájlok (Blob-tároló)</span><span class="sxs-lookup"><span data-stu-id="38499-120">wad-iis-logfiles (Blob Storage)</span></span> |
| <span data-ttu-id="38499-121">Rendszernapló</span><span class="sxs-lookup"><span data-stu-id="38499-121">Syslog</span></span> |<span data-ttu-id="38499-122">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="38499-122">Virtual Machines</span></span> |<span data-ttu-id="38499-123">LinuxsyslogVer2v0 (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="38499-123">LinuxsyslogVer2v0 (Table Storage)</span></span> |
| <span data-ttu-id="38499-124">Service Fabric működési események</span><span class="sxs-lookup"><span data-stu-id="38499-124">Service Fabric Operational Events</span></span> |<span data-ttu-id="38499-125">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="38499-125">Service Fabric nodes</span></span> |<span data-ttu-id="38499-126">WADServiceFabricSystemEventTable</span><span class="sxs-lookup"><span data-stu-id="38499-126">WADServiceFabricSystemEventTable</span></span> |
| <span data-ttu-id="38499-127">Service Fabric megbízható szereplő események</span><span class="sxs-lookup"><span data-stu-id="38499-127">Service Fabric Reliable Actor Events</span></span> |<span data-ttu-id="38499-128">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="38499-128">Service Fabric nodes</span></span> |<span data-ttu-id="38499-129">WADServiceFabricReliableActorEventTable</span><span class="sxs-lookup"><span data-stu-id="38499-129">WADServiceFabricReliableActorEventTable</span></span> |
| <span data-ttu-id="38499-130">Service Fabric megbízható eseményei</span><span class="sxs-lookup"><span data-stu-id="38499-130">Service Fabric Reliable Service Events</span></span> |<span data-ttu-id="38499-131">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="38499-131">Service Fabric nodes</span></span> |<span data-ttu-id="38499-132">WADServiceFabricReliableServiceEventTable</span><span class="sxs-lookup"><span data-stu-id="38499-132">WADServiceFabricReliableServiceEventTable</span></span> |
| <span data-ttu-id="38499-133">Windows-Eseménynapló</span><span class="sxs-lookup"><span data-stu-id="38499-133">Windows Event logs</span></span> |<span data-ttu-id="38499-134">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="38499-134">Service Fabric nodes</span></span> <br> <span data-ttu-id="38499-135">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="38499-135">Virtual Machines</span></span> <br> <span data-ttu-id="38499-136">Webes szerepkörök</span><span class="sxs-lookup"><span data-stu-id="38499-136">Web roles</span></span> <br> <span data-ttu-id="38499-137">Feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="38499-137">Worker roles</span></span> |<span data-ttu-id="38499-138">WADWindowsEventLogsTable (Table-tároló)</span><span class="sxs-lookup"><span data-stu-id="38499-138">WADWindowsEventLogsTable (Table Storage)</span></span> |
| <span data-ttu-id="38499-139">A Windows ETW-naplók</span><span class="sxs-lookup"><span data-stu-id="38499-139">Windows ETW logs</span></span> |<span data-ttu-id="38499-140">Service Fabric-csomópontokon</span><span class="sxs-lookup"><span data-stu-id="38499-140">Service Fabric nodes</span></span> <br> <span data-ttu-id="38499-141">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="38499-141">Virtual Machines</span></span> <br> <span data-ttu-id="38499-142">Webes szerepkörök</span><span class="sxs-lookup"><span data-stu-id="38499-142">Web roles</span></span> <br> <span data-ttu-id="38499-143">Feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="38499-143">Worker roles</span></span> |<span data-ttu-id="38499-144">WADETWEventTable (Table-tároló)</span><span class="sxs-lookup"><span data-stu-id="38499-144">WADETWEventTable (Table Storage)</span></span> |

> [!NOTE]
> <span data-ttu-id="38499-145">Az Azure-webhelyek IIS-napló jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="38499-145">IIS logs from Azure Websites are not currently supported.</span></span>
>
>

<span data-ttu-id="38499-146">A virtuális gépek, lehetősége van hello hello telepítésének [Naplóelemzési ügynök](log-analytics-azure-vm-extension.md) be a virtuális gép tooenable további betekintést nyerjen.</span><span class="sxs-lookup"><span data-stu-id="38499-146">For virtual machines, you have hello option of installing hello [Log Analytics agent](log-analytics-azure-vm-extension.md) into your virtual machine tooenable additional insights.</span></span> <span data-ttu-id="38499-147">Ezenkívül toobeing képes tooanalyze IIS naplókat és az eseménynaplók, végezhet további elemzés, beleértve a konfigurációs változások követését, az SQL értékelése és a frissítések értékelését.</span><span class="sxs-lookup"><span data-stu-id="38499-147">In addition toobeing able tooanalyze IIS logs and Event Logs, you can perform additional analysis including configuration change tracking, SQL assessment, and update assessment.</span></span>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a><span data-ttu-id="38499-148">A virtuális gép Azure diagnostics engedélyezése az eseménynaplót, és az IIS naplózása gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="38499-148">Enable Azure diagnostics in a virtual machine for event log and IIS log collection</span></span>
<span data-ttu-id="38499-149">Az Eseménynapló és az IIS naplózása gyűjtemény hello Microsoft Azure portál használatával a következő eljárás tooenable Azure diagnosztika a virtuális gép használata hello.</span><span class="sxs-lookup"><span data-stu-id="38499-149">Use hello following procedure tooenable Azure diagnostics in a virtual machine for Event Log and IIS log collection using hello Microsoft Azure portal.</span></span>

### <a name="tooenable-azure-diagnostics-in-a-virtual-machine-with-hello-azure-portal"></a><span data-ttu-id="38499-150">tooenable hello Azure-portál virtuális gépet az Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="38499-150">tooenable Azure diagnostics in a virtual machine with hello Azure portal</span></span>
1. <span data-ttu-id="38499-151">Hello Virtuálisgép-ügynök telepítése a virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="38499-151">Install hello VM Agent when you create a virtual machine.</span></span> <span data-ttu-id="38499-152">Ha hello virtuális gép már létezik, győződjön meg arról, hogy hello Virtuálisgép-ügynök már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="38499-152">If hello virtual machine already exists, verify that hello VM Agent is already installed.</span></span>

   * <span data-ttu-id="38499-153">A hello Azure-portálon lépjen toohello virtuális gépet, jelölje ki **opcionális konfigurációs**, majd **diagnosztika** és **állapot** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="38499-153">In hello Azure portal, navigate toohello virtual machine, select **Optional Configuration**, then **Diagnostics** and set **Status** too**On**.</span></span>

     <span data-ttu-id="38499-154">Befejezett hello VM kiterjesztése hello Azure Diagnostics telepített és futó.</span><span class="sxs-lookup"><span data-stu-id="38499-154">Upon completion, hello VM has hello Azure Diagnostics extension installed and running.</span></span> <span data-ttu-id="38499-155">A bővítmény felelős a diagnosztikai adatainak összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="38499-155">This extension is responsible for collecting your diagnostics data.</span></span>
2. <span data-ttu-id="38499-156">Engedélyezze a megfigyelést és a meglévő virtuális az eseménynaplózás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="38499-156">Enable monitoring and configure event logging on an existing VM.</span></span> <span data-ttu-id="38499-157">A virtuális gép szintjének hello diagnosztika engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="38499-157">You can enable diagnostics at hello VM level.</span></span> <span data-ttu-id="38499-158">tooenable diagnosztika és majd az eseménynaplózás konfigurálásához, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="38499-158">tooenable diagnostics and then configure event logging, perform hello following steps:</span></span>

   1. <span data-ttu-id="38499-159">Válassza ki a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="38499-159">Select hello VM.</span></span>
   2. <span data-ttu-id="38499-160">Kattintson a **figyelési**.</span><span class="sxs-lookup"><span data-stu-id="38499-160">Click **Monitoring**.</span></span>
   3. <span data-ttu-id="38499-161">Kattintson a **diagnosztika**.</span><span class="sxs-lookup"><span data-stu-id="38499-161">Click **Diagnostics**.</span></span>
   4. <span data-ttu-id="38499-162">Set hello **állapot** túl**ON**.</span><span class="sxs-lookup"><span data-stu-id="38499-162">Set hello **Status** too**ON**.</span></span>
   5. <span data-ttu-id="38499-163">Válassza ki a megjeleníteni kívánt toocollect diagnosztikai naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="38499-163">Select each diagnostics log that you want toocollect.</span></span>
   6. <span data-ttu-id="38499-164">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="38499-164">Click **OK**.</span></span>

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a><span data-ttu-id="38499-165">Az IIS-napló és a esemény gyűjteményhez webes szerepkörrel rendelkező Azure diagnostics engedélyezése</span><span class="sxs-lookup"><span data-stu-id="38499-165">Enable Azure diagnostics in a Web role for IIS log and event collection</span></span>
<span data-ttu-id="38499-166">Tekintse meg a túl[hogyan tooEnable felhőszolgáltatásban diagnosztika](../cloud-services/cloud-services-dotnet-diagnostics.md) általános lépései az Azure diagnostics engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="38499-166">Refer too[How tooEnable Diagnostics in a Cloud Service](../cloud-services/cloud-services-dotnet-diagnostics.md) for general steps on enabling Azure diagnostics.</span></span> <span data-ttu-id="38499-167">az alábbi utasítások hello ezekkel az adatokkal, és testre szabható Log Analyticshez való használatra.</span><span class="sxs-lookup"><span data-stu-id="38499-167">hello instructions below use this information and customize it for use with Log Analytics.</span></span>

<span data-ttu-id="38499-168">Az Azure diagnostics engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="38499-168">With Azure diagnostics enabled:</span></span>

* <span data-ttu-id="38499-169">IIS-napló hello scheduledTransferPeriod átviteli időközönként átvitt adatainak naplózása alapértelmezés szerint tárolja.</span><span class="sxs-lookup"><span data-stu-id="38499-169">IIS logs are stored by default, with log data transferred at hello scheduledTransferPeriod transfer interval.</span></span>
* <span data-ttu-id="38499-170">Alapértelmezés szerint a Windows-Eseménynapló nem kerülnek.</span><span class="sxs-lookup"><span data-stu-id="38499-170">Windows Event Logs are not transferred by default.</span></span>

### <a name="tooenable-diagnostics"></a><span data-ttu-id="38499-171">tooenable diagnosztika</span><span class="sxs-lookup"><span data-stu-id="38499-171">tooenable diagnostics</span></span>
<span data-ttu-id="38499-172">Windows-Eseménynapló tooenable vagy toochange hello scheduledTransferPeriod, hello XML konfigurációs fájl (diagnostics.wadcfg), Azure-diagnosztika konfigurálása, ahogy az [4. lépés: a diagnosztika konfigurációs fájl létrehozása és telepítése hello bővítmény](../cloud-services/cloud-services-dotnet-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="38499-172">tooenable Windows Event Logs, or toochange hello scheduledTransferPeriod, configure Azure Diagnostics using hello XML configuration file (diagnostics.wadcfg), as shown in [Step 4: Create your Diagnostics configuration file and install hello extension](../cloud-services/cloud-services-dotnet-diagnostics.md)</span></span>

<span data-ttu-id="38499-173">hello következő konfigurációs szakasz gyűjti az IIS-naplókba és minden alkalmazás hello származó események és a rendszer naplóit:</span><span class="sxs-lookup"><span data-stu-id="38499-173">hello following example configuration file collects IIS Logs and all Events from hello Application and System logs:</span></span>

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

<span data-ttu-id="38499-174">Győződjön meg arról, hogy a ConfigurationSettings határozza meg a storage-fiók, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="38499-174">Ensure that your ConfigurationSettings specifies a storage account, as in hello following example:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

<span data-ttu-id="38499-175">Hello **AccountName** és **AccountKey** értékek találhatók hello hello tárolási fiók irányítópulton, elérési kulcsok kezelése az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="38499-175">hello **AccountName** and **AccountKey** values are found in hello Azure portal in hello storage account dashboard, under Manage Access Keys.</span></span> <span data-ttu-id="38499-176">hello protokollja hello kapcsolati karakterláncot kell **https**.</span><span class="sxs-lookup"><span data-stu-id="38499-176">hello protocol for hello connection string must be **https**.</span></span>

<span data-ttu-id="38499-177">Hello frissített diagnosztikai konfiguráció alkalmazása után tooyour felhőalapú szolgáltatás, és ír diagnosztika tooAzure tárolására, akkor készen áll a tooconfigure Naplóelemzési áll.</span><span class="sxs-lookup"><span data-stu-id="38499-177">Once hello updated diagnostic configuration is applied tooyour cloud service and it is writing diagnostics tooAzure Storage, then you are ready tooconfigure Log Analytics.</span></span>

## <a name="use-hello-azure-portal-toocollect-logs-from-azure-storage"></a><span data-ttu-id="38499-178">Hello Azure portál toocollect naplók Azure Storage-ból</span><span class="sxs-lookup"><span data-stu-id="38499-178">Use hello Azure portal toocollect logs from Azure Storage</span></span>
<span data-ttu-id="38499-179">Hello Azure portál tooconfigure Naplóelemzési toocollect hello naplók hello Azure-szolgáltatások a következő használható:</span><span class="sxs-lookup"><span data-stu-id="38499-179">You can use hello Azure portal tooconfigure Log Analytics toocollect hello logs for hello following Azure services:</span></span>

* <span data-ttu-id="38499-180">Service Fabric-fürtök</span><span class="sxs-lookup"><span data-stu-id="38499-180">Service Fabric clusters</span></span>
* <span data-ttu-id="38499-181">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="38499-181">Virtual Machines</span></span>
* <span data-ttu-id="38499-182">Webes vagy feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="38499-182">Web/Worker Roles</span></span>

<span data-ttu-id="38499-183">Az Azure-portálon hello keresse meg a tooyour Naplóelemzési munkaterület, és hajtsa végre a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="38499-183">In hello Azure portal, navigate tooyour Log Analytics workspace and perform hello following tasks:</span></span>

1. <span data-ttu-id="38499-184">Kattintson a *tárfiókok naplók*</span><span class="sxs-lookup"><span data-stu-id="38499-184">Click *Storage accounts logs*</span></span>
2. <span data-ttu-id="38499-185">Kattintson a hello *Hozzáadás* feladat</span><span class="sxs-lookup"><span data-stu-id="38499-185">Click hello *Add* task</span></span>
3. <span data-ttu-id="38499-186">Válassza ki, amely tartalmazza a hello diagnosztikai naplók hello tárfiókot</span><span class="sxs-lookup"><span data-stu-id="38499-186">Select hello Storage account that contains hello diagnostics logs</span></span>
   * <span data-ttu-id="38499-187">Ez a fiók lehet egy hagyományos tárolási fiók vagy egy Azure Resource Manager storage-fiók</span><span class="sxs-lookup"><span data-stu-id="38499-187">This account can be either a classic storage account or an Azure Resource Manager storage account</span></span>
4. <span data-ttu-id="38499-188">Válassza ki a kívánt toocollect naplókat adattípus hello</span><span class="sxs-lookup"><span data-stu-id="38499-188">Select hello Data Type you want toocollect logs for</span></span>
   * <span data-ttu-id="38499-189">hello választási lehetőségek: IIS-naplóit; Az eseményeket; Syslog (Linux); ETW naplók; Service Fabric-események</span><span class="sxs-lookup"><span data-stu-id="38499-189">hello choices are IIS Logs; Events; Syslog (Linux); ETW Logs; Service Fabric Events</span></span>
5. <span data-ttu-id="38499-190">forrás hello érték automatikusan alapján van feltöltve hello adattípusúra, és nem módosítható</span><span class="sxs-lookup"><span data-stu-id="38499-190">hello value for Source is automatically populated based on hello data type and cannot be changed</span></span>
6. <span data-ttu-id="38499-191">Kattintson az OK toosave hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="38499-191">Click OK toosave hello configuration</span></span>

<span data-ttu-id="38499-192">További tárhely fiókok és az adatok esetében, amelyet az Naplóelemzési toocollect ismételje meg a 2 – 6.</span><span class="sxs-lookup"><span data-stu-id="38499-192">Repeat steps 2-6 for additional storage accounts and data types that you want Log Analytics toocollect.</span></span>

<span data-ttu-id="38499-193">Körülbelül 30 percet, a rendszer hello storage-fiókot Log Analytics képes toosee adatait.</span><span class="sxs-lookup"><span data-stu-id="38499-193">In approximately 30 minutes, you are able toosee data from hello storage account in Log Analytics.</span></span> <span data-ttu-id="38499-194">Írt adatok toostorage hello konfiguráció alkalmazása után csak akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="38499-194">You will only see data that is written toostorage after hello configuration is applied.</span></span> <span data-ttu-id="38499-195">A Naplóelemzési nem hello tárfiók hello már létező adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="38499-195">Log Analytics does not read hello pre-existing data from hello storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="38499-196">hello portál nem felel meg a forrás létezik hello tárfiókban hello, vagy ha új adatok írása.</span><span class="sxs-lookup"><span data-stu-id="38499-196">hello portal does not validate that hello Source exists in hello storage account or if new data is being written.</span></span>
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a><span data-ttu-id="38499-197">A virtuális gép Azure diagnostics engedélyezése az eseménynaplót, és IIS töltsék PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="38499-197">Enable Azure diagnostics in a virtual machine for event log and IIS log collection using PowerShell</span></span>
<span data-ttu-id="38499-198">Használjon hello szükséges lépések [Naplóelemzési konfigurálása tooindex Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) toouse PowerShell tooread az Azure diagnostics tootable tárolási írt.</span><span class="sxs-lookup"><span data-stu-id="38499-198">Use hello steps in [Configuring Log Analytics tooindex Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) toouse PowerShell tooread from Azure diagnostics that are written tootable storage.</span></span>

<span data-ttu-id="38499-199">Azure PowerShell használatával pontosabban megadhatja tooAzure tárolási írt hello események.</span><span class="sxs-lookup"><span data-stu-id="38499-199">Using Azure PowerShell you can more precisely specify hello events that are written tooAzure Storage.</span></span>
<span data-ttu-id="38499-200">További információkért lásd: [diagnosztika engedélyezésével az Azure virtuális gépek](../virtual-machines-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="38499-200">For more information, see [Enabling Diagnostics in Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="38499-201">Engedélyezi, és használja a következő PowerShell-parancsfájl hello Azure diagnostics frissítése.</span><span class="sxs-lookup"><span data-stu-id="38499-201">You can enable and update Azure diagnostics using hello following PowerShell script.</span></span>
<span data-ttu-id="38499-202">Ez a parancsfájl egy egyéni naplózási konfiguráció is használható.</span><span class="sxs-lookup"><span data-stu-id="38499-202">You can also use this script with a custom logging configuration.</span></span>
<span data-ttu-id="38499-203">Módosítsa a hello parancsfájl tooset hello tárfiókot, a szolgáltatás neve és a virtuális gép neve.</span><span class="sxs-lookup"><span data-stu-id="38499-203">Modify hello script tooset hello storage account, service name, and virtual machine name.</span></span>
<span data-ttu-id="38499-204">hello parancsfájl parancsmagokat használja a klasszikus virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="38499-204">hello script uses cmdlets for classic virtual machines.</span></span>

<span data-ttu-id="38499-205">Tekintse át a következő parancsfájl minta hello, másolja, szükség szerint módosítsa hello minta elmentse egy olyan PowerShell-parancsfájlt, és futtassa a hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="38499-205">Review hello following script sample, copy it, modify it as needed, save hello sample as a PowerShell script file, and then run hello script.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="38499-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="38499-206">Next steps</span></span>
* <span data-ttu-id="38499-207">[Gyűjteni a naplókat és az Azure-szolgáltatások metrikáját](log-analytics-azure-storage.md) támogatott Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="38499-207">[Collect logs and metrics for Azure services](log-analytics-azure-storage.md) for supported Azure services.</span></span>
* <span data-ttu-id="38499-208">[Megoldások](log-analytics-add-solutions.md) hello adatok tooprovide betekintést.</span><span class="sxs-lookup"><span data-stu-id="38499-208">[Enable Solutions](log-analytics-add-solutions.md) tooprovide insight into hello data.</span></span>
* <span data-ttu-id="38499-209">[Használja a keresési lekérdezések](log-analytics-log-searches.md) tooanalyze hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="38499-209">[Use search queries](log-analytics-log-searches.md) tooanalyze hello data.</span></span>
