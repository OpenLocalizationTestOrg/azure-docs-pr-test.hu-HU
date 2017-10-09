---
redirect_url: /azure/log-analytics/log-analytics-agent-linux
redirect_document_id: True
ROBOTS: NOINDEX
ms.openlocfilehash: 8b526144cd565f6750368e12970f008e66cc2023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toolog-analytics"></a><span data-ttu-id="a67a3-101">Csatlakozás a Linux rendszerű számítógépek tooLog elemzés</span><span class="sxs-lookup"><span data-stu-id="a67a3-101">Connect your Linux computers tooLog Analytics</span></span>
<span data-ttu-id="a67a3-102">Log Analytics használ, összegyűjtése és Linux rendszerű számítógépek a létrehozott adatok rájuk.</span><span class="sxs-lookup"><span data-stu-id="a67a3-102">Using Log Analytics, you can collect and act on data generated from Linux computers.</span></span> <span data-ttu-id="a67a3-103">A Linux tooOMS során gyűjtött adatok hozzáadása lehetővé teszi az toomanage Linux rendszerek és tároló-megoldásokkal Docker, például a számítógépek helyétől függetlenül – pontjáról.</span><span class="sxs-lookup"><span data-stu-id="a67a3-103">Adding data collected from Linux tooOMS allows you toomanage Linux systems and container solutions like Docker, regardless of where your computers are located — virtually anywhere.</span></span> <span data-ttu-id="a67a3-104">Adatforrások lehet, hogy a helyszíni adatközpontban fizikai kiszolgálóként, például az Amazon Web Services (AWS) vagy a Microsoft Azure-felhőben üzemeltetett szolgáltatás a virtuális gépek találhatók, vagy még hello nehezedő hordozható.</span><span class="sxs-lookup"><span data-stu-id="a67a3-104">Data sources might reside in your on-premises datacenter as physical servers, virtual computers in a cloud-hosted service like Amazon Web Services (AWS) or Microsoft Azure, or even hello laptop on your desk.</span></span> <span data-ttu-id="a67a3-105">Emellett OMS is adatokat gyűjt a Windows rendszerű számítógépek hasonlóan, az támogatja-e egy valóban hibrid informatikai környezetben.</span><span class="sxs-lookup"><span data-stu-id="a67a3-105">In addition, OMS also collects data from Windows computers similarly, so it supports a truly hybrid IT environment.</span></span>

<span data-ttu-id="a67a3-106">Megtekintheti és adatok kezelésében az összes egy egyetlen felügyeleti portállal forrásokra az OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="a67a3-106">You can view and manage data from all of those sources with Log Analytics in OMS with a single management portal.</span></span> <span data-ttu-id="a67a3-107">Ez csökkenti a hello kell toomonitor használ a különböző rendszerek, így könnyen tooconsume, és adatokat toowhatever üzleti elemzési megoldások vagy a rendszer, amely már tetszés exportálhatja.</span><span class="sxs-lookup"><span data-stu-id="a67a3-107">This reduces hello need toomonitor it using many different systems, makes it easy tooconsume, and you can export any data you like toowhatever business analytics solution or system that you already have.</span></span>

<span data-ttu-id="a67a3-108">Ez a cikk egy gyors üzembe helyezési útmutató, amely segít a Linux rendszerű számítógépek OMS-ügynököt hello használata Linux adatok gyűjtésére és kezelésére.</span><span class="sxs-lookup"><span data-stu-id="a67a3-108">This article is a quick start guide that will help you collect and manage data for your Linux computers using hello OMS Agent for Linux.</span></span> <span data-ttu-id="a67a3-109">Például a proxykiszolgáló-konfigurációt, az CollectD metrikákat, és egyéni JSON-adatforrások további technikai részleteket, hogy információkat találhat [OMS-ügynököt Linux – áttekintés](https://github.com/Microsoft/OMS-Agent-for-Linux) és [OMS-ügynököt Linux teljes dokumentációja](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) a Githubon.</span><span class="sxs-lookup"><span data-stu-id="a67a3-109">For more technical details such as proxy server configuration, information about CollectD metrics, and custom JSON data sources, you’ll find that information at [OMS Agent for Linux overview](https://github.com/Microsoft/OMS-Agent-for-Linux) and [OMS Agent for Linux full documentation](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) on GitHub.</span></span>

<span data-ttu-id="a67a3-110">Jelenleg típusú adatokat a Linux rendszerű számítógépek következő hello hozhatja létre:</span><span class="sxs-lookup"><span data-stu-id="a67a3-110">Currently, you can collect hello following types of data from Linux computers:</span></span>

* <span data-ttu-id="a67a3-111">Teljesítménymértékeket</span><span class="sxs-lookup"><span data-stu-id="a67a3-111">Performance metrics</span></span>
* <span data-ttu-id="a67a3-112">Syslog-események</span><span class="sxs-lookup"><span data-stu-id="a67a3-112">Syslog events</span></span>
* <span data-ttu-id="a67a3-113">Nagios és Zabbix-riasztások</span><span class="sxs-lookup"><span data-stu-id="a67a3-113">Alerts from Nagios and Zabbix</span></span>
* <span data-ttu-id="a67a3-114">Docker-tároló teljesítménymutatók, a készlet és a naplók</span><span class="sxs-lookup"><span data-stu-id="a67a3-114">Docker container performance metrics, inventory and logs</span></span>

## <a name="supported-linux-versions"></a><span data-ttu-id="a67a3-115">Támogatott Linux-verziók</span><span class="sxs-lookup"><span data-stu-id="a67a3-115">Supported Linux versions</span></span>
<span data-ttu-id="a67a3-116">X86 és az x64 verziója Linux terjesztésekről számos hivatalosan támogatottak.</span><span class="sxs-lookup"><span data-stu-id="a67a3-116">Both x86 and x64 versions are officially supported on a variety of Linux distributions.</span></span> <span data-ttu-id="a67a3-117">Azonban hello Linux OMS-ügynököt előfordulhat, hogy is futtathatja más azokat a terjesztéseket nem szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="a67a3-117">However, hello OMS Agent for Linux might also run on other distributions not listed.</span></span>

* <span data-ttu-id="a67a3-118">Amazon Linux 2012.09 2015.09 keresztül</span><span class="sxs-lookup"><span data-stu-id="a67a3-118">Amazon Linux 2012.09 through 2015.09</span></span>
* <span data-ttu-id="a67a3-119">CentOS Linux 5, 6 és 7</span><span class="sxs-lookup"><span data-stu-id="a67a3-119">CentOS Linux 5, 6, and 7</span></span>
* <span data-ttu-id="a67a3-120">Oracle Linux 5, 6 és 7</span><span class="sxs-lookup"><span data-stu-id="a67a3-120">Oracle Linux 5, 6, and 7</span></span>
* <span data-ttu-id="a67a3-121">Red Hat Enterprise Linux Server 5, 6, 7</span><span class="sxs-lookup"><span data-stu-id="a67a3-121">Red Hat Enterprise Linux Server 5, 6 and 7</span></span>
* <span data-ttu-id="a67a3-122">Debian GNU/Linux 6, 7 és 8</span><span class="sxs-lookup"><span data-stu-id="a67a3-122">Debian GNU/Linux 6, 7, and 8</span></span>
* <span data-ttu-id="a67a3-123">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10</span><span class="sxs-lookup"><span data-stu-id="a67a3-123">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10</span></span>
* <span data-ttu-id="a67a3-124">SUSE Linux Enterprise Server 11 és 12</span><span class="sxs-lookup"><span data-stu-id="a67a3-124">SUSE Linux Enterprise Server 11 and 12</span></span>

## <a name="oms-agent-for-linux"></a><span data-ttu-id="a67a3-125">Linux OMS-ügynök</span><span class="sxs-lookup"><span data-stu-id="a67a3-125">OMS Agent for Linux</span></span>
<span data-ttu-id="a67a3-126">Operations Management Suite-ügynök Linux hello több csomagot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a67a3-126">hello Operations Management Suite Agent for Linux comprises multiple packages.</span></span> <span data-ttu-id="a67a3-127">hello kiadás fájl tartalmazza a következő csomagok, a futó hello rendszerhéj köteg által elérhető hello `--extract`.</span><span class="sxs-lookup"><span data-stu-id="a67a3-127">hello release file contains hello following packages, available by running hello shell bundle with `--extract`.</span></span>

| <span data-ttu-id="a67a3-128">**Csomag**</span><span class="sxs-lookup"><span data-stu-id="a67a3-128">**Package**</span></span> | <span data-ttu-id="a67a3-129">**Verzió**</span><span class="sxs-lookup"><span data-stu-id="a67a3-129">**Version**</span></span> | <span data-ttu-id="a67a3-130">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="a67a3-130">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a67a3-131">omsagent</span><span class="sxs-lookup"><span data-stu-id="a67a3-131">omsagent</span></span> |<span data-ttu-id="a67a3-132">1.1.0</span><span class="sxs-lookup"><span data-stu-id="a67a3-132">1.1.0</span></span> |<span data-ttu-id="a67a3-133">Operations Management Suite-ügynök Linux hello</span><span class="sxs-lookup"><span data-stu-id="a67a3-133">hello Operations Management Suite Agent for Linux</span></span> |
| <span data-ttu-id="a67a3-134">omsconfig</span><span class="sxs-lookup"><span data-stu-id="a67a3-134">omsconfig</span></span> |<span data-ttu-id="a67a3-135">1.1.1</span><span class="sxs-lookup"><span data-stu-id="a67a3-135">1.1.1</span></span> |<span data-ttu-id="a67a3-136">Az OMS-ügynököt hello konfiguráció ügynök</span><span class="sxs-lookup"><span data-stu-id="a67a3-136">Configuration agent for hello OMS Agent</span></span> |
| <span data-ttu-id="a67a3-137">OMI</span><span class="sxs-lookup"><span data-stu-id="a67a3-137">omi</span></span> |<span data-ttu-id="a67a3-138">1.0.8.3</span><span class="sxs-lookup"><span data-stu-id="a67a3-138">1.0.8.3</span></span> |<span data-ttu-id="a67a3-139">Open Management Infrastructure (OMI) – egy egyszerűsített CIM-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a67a3-139">Open Management Infrastructure (OMI) -- a lightweight CIM Server</span></span> |
| <span data-ttu-id="a67a3-140">az scx</span><span class="sxs-lookup"><span data-stu-id="a67a3-140">scx</span></span> |<span data-ttu-id="a67a3-141">1.6.2</span><span class="sxs-lookup"><span data-stu-id="a67a3-141">1.6.2</span></span> |<span data-ttu-id="a67a3-142">Operációs rendszer teljesítménymutatók OMI a CIM-szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="a67a3-142">OMI CIM Providers for operating system performance metrics</span></span> |
| <span data-ttu-id="a67a3-143">Apache-cimprov</span><span class="sxs-lookup"><span data-stu-id="a67a3-143">apache-cimprov</span></span> |<span data-ttu-id="a67a3-144">1.0.0</span><span class="sxs-lookup"><span data-stu-id="a67a3-144">1.0.0</span></span> |<span data-ttu-id="a67a3-145">Apache HTTP Server teljesítményfigyelés-szolgáltató az OMI.</span><span class="sxs-lookup"><span data-stu-id="a67a3-145">Apache HTTP Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="a67a3-146">Csak akkor telepíthetők, ha a rendszer észleli az Apache HTTP Server.</span><span class="sxs-lookup"><span data-stu-id="a67a3-146">Only installed if Apache HTTP Server is detected.</span></span> |
| <span data-ttu-id="a67a3-147">MySQL-cimprov</span><span class="sxs-lookup"><span data-stu-id="a67a3-147">mysql-cimprov</span></span> |<span data-ttu-id="a67a3-148">1.0.0</span><span class="sxs-lookup"><span data-stu-id="a67a3-148">1.0.0</span></span> |<span data-ttu-id="a67a3-149">MySQL-kiszolgáló teljesítményfigyelés-szolgáltató az OMI.</span><span class="sxs-lookup"><span data-stu-id="a67a3-149">MySQL Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="a67a3-150">Csak akkor telepíthetők, ha a MySQL/MariaDB kiszolgáló észleli.</span><span class="sxs-lookup"><span data-stu-id="a67a3-150">Only installed if MySQL/MariaDB server is detected.</span></span> |
| <span data-ttu-id="a67a3-151">docker-cimprov</span><span class="sxs-lookup"><span data-stu-id="a67a3-151">docker-cimprov</span></span> |<span data-ttu-id="a67a3-152">0.1.0</span><span class="sxs-lookup"><span data-stu-id="a67a3-152">0.1.0</span></span> |<span data-ttu-id="a67a3-153">Docker-szolgáltató az OMI.</span><span class="sxs-lookup"><span data-stu-id="a67a3-153">Docker provider for OMI.</span></span> <span data-ttu-id="a67a3-154">Csak akkor telepíthetők, ha a Docker észleli.</span><span class="sxs-lookup"><span data-stu-id="a67a3-154">Only installed if Docker is detected.</span></span> |

### <a name="additional-installation-artifacts"></a><span data-ttu-id="a67a3-155">További telepítési összetevők</span><span class="sxs-lookup"><span data-stu-id="a67a3-155">Additional installation artifacts</span></span>
<span data-ttu-id="a67a3-156">Miután telepítette a Linux-csomagok hello OMS-ügynököt, hello következő további rendszerszintű konfigurációs a módosításai érvényesek lesznek.</span><span class="sxs-lookup"><span data-stu-id="a67a3-156">After installing hello OMS agent for Linux packages, hello following additional system-wide configuration changes are applied.</span></span> <span data-ttu-id="a67a3-157">Ezen összetevők hello omsagent csomag eltávolításakor a rendszer törli.</span><span class="sxs-lookup"><span data-stu-id="a67a3-157">These artifacts are removed when hello omsagent package is uninstalled.</span></span>

* <span data-ttu-id="a67a3-158">Egy jogosulatlan felhasználó nevű: `omsagent` jön létre.</span><span class="sxs-lookup"><span data-stu-id="a67a3-158">A non-privileged user named: `omsagent` is created.</span></span> <span data-ttu-id="a67a3-159">Ez a hello fiók hello omsagent démon fut</span><span class="sxs-lookup"><span data-stu-id="a67a3-159">This is hello account hello omsagent daemon runs as</span></span>
* <span data-ttu-id="a67a3-160">Egy sudoers "tartalmaz" fájl jön létre, /etc/sudoers.d/omsagent Ez engedélyezi a omsagent toorestart hello syslog és omsagent démonok.</span><span class="sxs-lookup"><span data-stu-id="a67a3-160">A sudoers “include” file is created at /etc/sudoers.d/omsagent This authorizes omsagent toorestart hello syslog and omsagent daemons.</span></span> <span data-ttu-id="a67a3-161">Ha a sudo "tartalmaz" direktívák nem támogatottak a sudo hello telepített verziója, ezek a bejegyzések kerülnek túl/etc/sudoers.</span><span class="sxs-lookup"><span data-stu-id="a67a3-161">If sudo “include” directives are not supported in hello installed version of sudo, these entries will be written too/etc/sudoers.</span></span>
* <span data-ttu-id="a67a3-162">hello Rendszernaplózás konfigurálásánál módosított tooforward események toohello ügynök egy részét.</span><span class="sxs-lookup"><span data-stu-id="a67a3-162">hello syslog configuration is modified tooforward a subset of events toohello agent.</span></span> <span data-ttu-id="a67a3-163">További információkért lásd: hello **adatok gyűjtésének konfigurálása** az alábbi szakasz</span><span class="sxs-lookup"><span data-stu-id="a67a3-163">For more information, see hello **Configuring Data Collection** section below</span></span>

### <a name="linux-data-collection-details"></a><span data-ttu-id="a67a3-164">Linux az gyűjtemény adatait</span><span class="sxs-lookup"><span data-stu-id="a67a3-164">Linux data collection details</span></span>
<span data-ttu-id="a67a3-165">hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan részleteit.</span><span class="sxs-lookup"><span data-stu-id="a67a3-165">hello following table shows data collection methods and other details about how data is collected.</span></span>

| <span data-ttu-id="a67a3-166">Forrás</span><span class="sxs-lookup"><span data-stu-id="a67a3-166">source</span></span> | <span data-ttu-id="a67a3-167">Közvetlen ügynök</span><span class="sxs-lookup"><span data-stu-id="a67a3-167">Direct Agent</span></span> | <span data-ttu-id="a67a3-168">SCOM-ügynököt</span><span class="sxs-lookup"><span data-stu-id="a67a3-168">SCOM agent</span></span> | <span data-ttu-id="a67a3-169">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="a67a3-169">Azure Storage</span></span> | <span data-ttu-id="a67a3-170">SCOM szükséges?</span><span class="sxs-lookup"><span data-stu-id="a67a3-170">SCOM required?</span></span> | <span data-ttu-id="a67a3-171">Felügyeleti csoport keresztül küldött SCOM ügynök adatok</span><span class="sxs-lookup"><span data-stu-id="a67a3-171">SCOM agent data sent via management group</span></span> | <span data-ttu-id="a67a3-172">Gyűjtemény gyakorisága</span><span class="sxs-lookup"><span data-stu-id="a67a3-172">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="a67a3-173">Zabbix</span><span class="sxs-lookup"><span data-stu-id="a67a3-173">Zabbix</span></span> |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a67a3-179">1 perc</span><span class="sxs-lookup"><span data-stu-id="a67a3-179">1 minute</span></span> |
| <span data-ttu-id="a67a3-180">Nagios</span><span class="sxs-lookup"><span data-stu-id="a67a3-180">Nagios</span></span> |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a67a3-186">érkezésükkor</span><span class="sxs-lookup"><span data-stu-id="a67a3-186">on arrival</span></span> |
| <span data-ttu-id="a67a3-187">syslog</span><span class="sxs-lookup"><span data-stu-id="a67a3-187">syslog</span></span> |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a67a3-193">az Azure storage: 10 perc; az ügynök: érkezésükkor</span><span class="sxs-lookup"><span data-stu-id="a67a3-193">from Azure storage: 10 minutes; from agent: on arrival</span></span> |
| <span data-ttu-id="a67a3-194">Linux-teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="a67a3-194">Linux performance counters</span></span> |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a67a3-200">Ütemezés szerint, a 10 másodperces minimális</span><span class="sxs-lookup"><span data-stu-id="a67a3-200">As scheduled, minimum of 10 seconds</span></span> |
| <span data-ttu-id="a67a3-201">a változáskövetés</span><span class="sxs-lookup"><span data-stu-id="a67a3-201">change tracking</span></span> |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a67a3-207">óránként</span><span class="sxs-lookup"><span data-stu-id="a67a3-207">hourly</span></span> |

### <a name="package-requirements"></a><span data-ttu-id="a67a3-208">Csomag követelmények</span><span class="sxs-lookup"><span data-stu-id="a67a3-208">Package Requirements</span></span>
| <span data-ttu-id="a67a3-209">**Szükséges csomag**</span><span class="sxs-lookup"><span data-stu-id="a67a3-209">**Required package**</span></span> | <span data-ttu-id="a67a3-210">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="a67a3-210">**Description**</span></span> | <span data-ttu-id="a67a3-211">**Minimális verzió**</span><span class="sxs-lookup"><span data-stu-id="a67a3-211">**Minimum version**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a67a3-212">Glibc</span><span class="sxs-lookup"><span data-stu-id="a67a3-212">Glibc</span></span> |<span data-ttu-id="a67a3-213">GNU C-függvénytár</span><span class="sxs-lookup"><span data-stu-id="a67a3-213">GNU C library</span></span> |<span data-ttu-id="a67a3-214">2.5-12</span><span class="sxs-lookup"><span data-stu-id="a67a3-214">2.5-12</span></span> |
| <span data-ttu-id="a67a3-215">Openssl</span><span class="sxs-lookup"><span data-stu-id="a67a3-215">Openssl</span></span> |<span data-ttu-id="a67a3-216">OpenSSL-függvénytárak</span><span class="sxs-lookup"><span data-stu-id="a67a3-216">OpenSSL libraries</span></span> |<span data-ttu-id="a67a3-217">0.9.8e vagy 1.0</span><span class="sxs-lookup"><span data-stu-id="a67a3-217">0.9.8e or 1.0</span></span> |
| <span data-ttu-id="a67a3-218">Curl</span><span class="sxs-lookup"><span data-stu-id="a67a3-218">Curl</span></span> |<span data-ttu-id="a67a3-219">cURL-webügyfél</span><span class="sxs-lookup"><span data-stu-id="a67a3-219">cURL web client</span></span> |<span data-ttu-id="a67a3-220">7.15.5</span><span class="sxs-lookup"><span data-stu-id="a67a3-220">7.15.5</span></span> |
| <span data-ttu-id="a67a3-221">Python-ctypes</span><span class="sxs-lookup"><span data-stu-id="a67a3-221">Python-ctypes</span></span> |<span data-ttu-id="a67a3-222">függvény függvénytárak</span><span class="sxs-lookup"><span data-stu-id="a67a3-222">function libraries</span></span> |<span data-ttu-id="a67a3-223">n/a</span><span class="sxs-lookup"><span data-stu-id="a67a3-223">n/a</span></span> |
| <span data-ttu-id="a67a3-224">A PAM</span><span class="sxs-lookup"><span data-stu-id="a67a3-224">PAM</span></span> |<span data-ttu-id="a67a3-225">Cserélhető hitelesítési modulok</span><span class="sxs-lookup"><span data-stu-id="a67a3-225">Pluggable authentication Modules</span></span> |<span data-ttu-id="a67a3-226">n/a</span><span class="sxs-lookup"><span data-stu-id="a67a3-226">n/a</span></span> |

> [!NOTE]
> <span data-ttu-id="a67a3-227">Rsyslog vagy a syslog-ng van szükség toocollect syslog-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="a67a3-227">Either rsyslog or syslog-ng are required toocollect syslog messages.</span></span> <span data-ttu-id="a67a3-228">syslog-események gyűjtése nem támogatott hello alapértelmezett syslog démon a Red Hat Enterprise Linux-, CentOS, és Oracle Linux verziója (sysklog) 5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="a67a3-228">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="a67a3-229">ezek azokat a terjesztéseket, a jelen verziójában a syslog-adatot toocollect hello rsyslog démon kell telepíteni, és tooreplace sysklog konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a67a3-229">toocollect syslog data from this version of these distributions, hello rsyslog daemon should be installed and configured tooreplace sysklog.</span></span>
>
>

## <a name="quick-install"></a><span data-ttu-id="a67a3-230">Gyors telepítés</span><span class="sxs-lookup"><span data-stu-id="a67a3-230">Quick install</span></span>
<span data-ttu-id="a67a3-231">Futtassa a következő parancsok toodownload hello omsagent hello, hello ellenőrzőösszeg, majd telepítse és előkészítésére hello ügynök ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a67a3-231">Run hello following commands toodownload hello omsagent, validate hello checksum, then  install and onboard hello agent.</span></span> <span data-ttu-id="a67a3-232">Parancsok vannak a 64 bites.</span><span class="sxs-lookup"><span data-stu-id="a67a3-232">Commands are for 64-bit.</span></span> <span data-ttu-id="a67a3-233">hello munkaterület azonosítója és az elsődleges kulcs megtalálható az OMS-portálon hello **beállítások** a hello **csatlakoztatott források** fülre.</span><span class="sxs-lookup"><span data-stu-id="a67a3-233">hello Workspace ID and Primary Key are found in hello OMS portal under **Settings** on hello **Connected Sources** tab.</span></span>

![munkaterület részletei](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

<span data-ttu-id="a67a3-235">Számos egyéb módszerek tooinstall ügynök hello és frissíteni.</span><span class="sxs-lookup"><span data-stu-id="a67a3-235">There are a variety of other methods tooinstall hello agent and upgrade it.</span></span> <span data-ttu-id="a67a3-236">További róluk, [lépéseket tooinstall hello Linux OMS-ügynököt](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).</span><span class="sxs-lookup"><span data-stu-id="a67a3-236">You can read more about them at [Steps tooinstall hello OMS Agent for Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).</span></span>

<span data-ttu-id="a67a3-237">Emellett megtekinthet különböző hello [Azure bemutató videó](https://www.youtube.com/watch?v=mF1wtHPEzT0).</span><span class="sxs-lookup"><span data-stu-id="a67a3-237">You can also view hello [Azure video walkthrough](https://www.youtube.com/watch?v=mF1wtHPEzT0).</span></span>

## <a name="choose-your-linux-data-collection-method"></a><span data-ttu-id="a67a3-238">A Linux-adatok gyűjtési módjának kiválasztása</span><span class="sxs-lookup"><span data-stu-id="a67a3-238">Choose your Linux data collection method</span></span>
<span data-ttu-id="a67a3-239">Hogyan kezeli a hello adattípusok, például toocollect, attól függ, hogy szeretné-e toouse hello OMS-portálon, vagy ha azt szeretné, hogy közvetlenül a Linux-ügyfelek a különböző konfigurációs fájlok szerkeszthetők.</span><span class="sxs-lookup"><span data-stu-id="a67a3-239">How you choose hello data types you'd like toocollect depends on whether you want toouse hello OMS portal or if you want edit various configuration files directly on your Linux clients.</span></span> <span data-ttu-id="a67a3-240">Ha úgy dönt, hogy toouse hello portál, hello konfigurációs küld tooall a Linux-ügyfelek automatikusan.</span><span class="sxs-lookup"><span data-stu-id="a67a3-240">If you choose toouse hello portal, hello configuration is sent tooall of your Linux clients automatically.</span></span> <span data-ttu-id="a67a3-241">Ha különböző Linux-ügyfelek különböző konfigurációt, vagy tooedit ügyfélfájlok külön-külön – kell lesz, például a PowerShell DSC, Chef vagy Puppet helyett használja.</span><span class="sxs-lookup"><span data-stu-id="a67a3-241">If you need different configurations for different Linux clients, you will need tooedit client files individually – or use an alternative like PowerShell DSC, Chef, or Puppet.</span></span>

<span data-ttu-id="a67a3-242">Megadhat hello syslog-események és teljesítményszámlálók, amelyet az toocollect hello Linux-számítógépeken konfigurációs fájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="a67a3-242">You can specify hello syslog events and performance counters that you want toocollect using configuration files on hello Linux computers.</span></span> <span data-ttu-id="a67a3-243">*Ha úgy dönt, tooconfigure adatgyűjtés ügynök konfigurációs fájlok szerkesztésével, le kell tiltania hello központi konfigurációs.*</span><span class="sxs-lookup"><span data-stu-id="a67a3-243">*If you chose tooconfigure data collection by editing agent configuration files, you should disable hello centralized configuration.*</span></span>  <span data-ttu-id="a67a3-244">Útmutatás alább tooconfigure adatgyűjtés hello ügynök konfigurációs fájlokat, valamint toodisable központi konfigurálása-az összes OMS-ügynök a Linux vagy különálló számítógépeket.</span><span class="sxs-lookup"><span data-stu-id="a67a3-244">Instructions are provided below tooconfigure data collection in hello agent's configuration files as well as toodisable central configuration for all OMS Agents for Linux, or individual computers.</span></span>

### <a name="disable-oms-management-for-an-individual-linux-computer"></a><span data-ttu-id="a67a3-245">Tiltsa le az OMS egyéni Linux számítógép-kezelés</span><span class="sxs-lookup"><span data-stu-id="a67a3-245">Disable OMS management for an individual Linux computer</span></span>
<span data-ttu-id="a67a3-246">Konfigurációs adatok gyűjtésének központi le van tiltva a hello OMS_MetaConfigHelper.py parancsfájl egyes Linux számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="a67a3-246">Centralized data collection for configuration data is disabled for an individual Linux computer with hello OMS_MetaConfigHelper.py script.</span></span> <span data-ttu-id="a67a3-247">Ez akkor lehet hasznos, ha a számítógépek egy részhalmazát kell rendelkeznie egy speciális konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="a67a3-247">This can be useful if a subset of computers should have a specialized configuration.</span></span>

<span data-ttu-id="a67a3-248">központi konfiguráció toodisable:</span><span class="sxs-lookup"><span data-stu-id="a67a3-248">toodisable centralized configuration:</span></span>

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

<span data-ttu-id="a67a3-249">központi konfiguráció toore engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="a67a3-249">toore-enable centralized configuration:</span></span>

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a><span data-ttu-id="a67a3-250">Linux-teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="a67a3-250">Linux performance counters</span></span>
<span data-ttu-id="a67a3-251">Linux-teljesítményszámlálók hasonló tooWindows teljesítményszámlálók – is hasonló módon fog működni.</span><span class="sxs-lookup"><span data-stu-id="a67a3-251">Linux performance counters are similar tooWindows performance counters—both operate similarly.</span></span> <span data-ttu-id="a67a3-252">A következő eljárások tooadd hello használja, és konfigurálja őket.</span><span class="sxs-lookup"><span data-stu-id="a67a3-252">You can use hello following procedures tooadd and configure them.</span></span> <span data-ttu-id="a67a3-253">Miután hozzáadták azokat tooOMS, adatgyűjtés számukra 30 másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="a67a3-253">After they are added tooOMS, data is collected for them every 30 seconds.</span></span>

### <a name="tooadd-a-linux-performance-counter-in-oms"></a><span data-ttu-id="a67a3-254">tooadd OMS Linux teljesítményszámláló</span><span class="sxs-lookup"><span data-stu-id="a67a3-254">tooadd a Linux performance counter in OMS</span></span>
1. <span data-ttu-id="a67a3-255">hello OMS-portálon Linux ügynökök OMS tooconfigure, adhat hozzá Linux teljesítményszámlálók hello beállítások lapján kattintson a **adatok**.</span><span class="sxs-lookup"><span data-stu-id="a67a3-255">tooconfigure OMS Agents for Linux using hello OMS portal, you can add Linux performance counters on hello Settings page, click **Data**.</span></span>  
2. <span data-ttu-id="a67a3-256">A hello **beállítások** lapon az **adatok** , kattintson a **Linux teljesítményszámlálók** , majd válassza ki vagy típus hello név hello számláló tooadd szeretné.</span><span class="sxs-lookup"><span data-stu-id="a67a3-256">On hello **Settings** page under **Data** , click **Linux performance counters** and then select or type hello name of hello counter you want tooadd.</span></span>  
    <span data-ttu-id="a67a3-257">![adatok](./media/log-analytics-linux-agents/oms-settings-data01.png)</span><span class="sxs-lookup"><span data-stu-id="a67a3-257">![data](./media/log-analytics-linux-agents/oms-settings-data01.png)</span></span>
3. <span data-ttu-id="a67a3-258">Ha nem tudja hello teljes hello számláló nevét, egy részleges név indítható, és elérhető számlálók listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a67a3-258">If you don't know hello full name of hello counter, you can start typing a partial name and a list of available counters will appear.</span></span> <span data-ttu-id="a67a3-259">Ha megtalálta hello számláló meg tooadd szeretné, kattintson hello listában hello nevére, majd hello plusz ikon tooadd hello számláló.</span><span class="sxs-lookup"><span data-stu-id="a67a3-259">When you find hello counter you want tooadd, click hello name in hello list and then click hello plus icon tooadd hello counter.</span></span>
4. <span data-ttu-id="a67a3-260">Hello Számláló hozzáadása után színű sáv a kijelölt számlálók hello listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a67a3-260">After you add hello counter, it appears in hello list of counters highlighted with a colored bar.</span></span>
5. <span data-ttu-id="a67a3-261">Alapértelmezés szerint hello **alábbi konfigurációs toomy számítógépek alkalmaz** beállítást.</span><span class="sxs-lookup"><span data-stu-id="a67a3-261">By default, hello **Apply below configuration toomy machines** option is selected.</span></span> <span data-ttu-id="a67a3-262">Ha azt szeretné, hogy a konfigurációs adatok küldése toodisable, törölje a jelet hello.</span><span class="sxs-lookup"><span data-stu-id="a67a3-262">If you want toodisable sending configuration data, clear hello selection.</span></span>
6. <span data-ttu-id="a67a3-263">Ha befejezte az módosítása teljesítményszámlálók alján hello hello kattintson **mentése** toofinalize a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-263">When you are done modifying performance counters, at hello bottom of hello page click **Save** toofinalize your changes.</span></span> <span data-ttu-id="a67a3-264">hello végrehajtott konfigurációs módosításokat Linux általában 5 percnél regisztrált OMS-ben, majd küldése tooall hello OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-264">hello configuration changes that you've made are then sent tooall hello OMS Agents for Linux that are registered with OMS, typically within 5 minutes.</span></span>

### <a name="configure-linux-performance-counters-in-oms"></a><span data-ttu-id="a67a3-265">Linux-teljesítményszámlálók OMS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a67a3-265">Configure Linux performance counters in OMS</span></span>
<span data-ttu-id="a67a3-266">Windows-teljesítményszámlálókat kiválaszthatja az egyes teljesítményszámlálókhoz bizonyos példányainak.</span><span class="sxs-lookup"><span data-stu-id="a67a3-266">For Windows performance counters, you can choose a specific instance for each performance counter.</span></span> <span data-ttu-id="a67a3-267">Azonban Linux teljesítményszámlálókat, a számláló az Ön által választott példány tooall gyermek számlálók hello szülő számláló vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a67a3-267">However, for Linux performance counters, whatever instance of a counter that you choose applies tooall child counters of hello parent counter.</span></span> <span data-ttu-id="a67a3-268">hello következő táblázatban hello közös példányok elérhető tooboth Linux és a Windows a teljesítményszámlálókat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-268">hello following table shows hello common instances available tooboth Linux and Windows performance counters.</span></span>

| <span data-ttu-id="a67a3-269">**Példány neve**</span><span class="sxs-lookup"><span data-stu-id="a67a3-269">**Instance name**</span></span> | <span data-ttu-id="a67a3-270">**Jelentése**</span><span class="sxs-lookup"><span data-stu-id="a67a3-270">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="a67a3-271">\_Összesen</span><span class="sxs-lookup"><span data-stu-id="a67a3-271">\_Total</span></span> |<span data-ttu-id="a67a3-272">Hello példányainak száma</span><span class="sxs-lookup"><span data-stu-id="a67a3-272">Total of all hello instances</span></span> |
| \* |<span data-ttu-id="a67a3-273">Minden példány</span><span class="sxs-lookup"><span data-stu-id="a67a3-273">All instances</span></span> |
| <span data-ttu-id="a67a3-274">(/ &#124; / var)</span><span class="sxs-lookup"><span data-stu-id="a67a3-274">(/&#124;/var)</span></span> |<span data-ttu-id="a67a3-275">Példány neve megegyezik: / vagy /var</span><span class="sxs-lookup"><span data-stu-id="a67a3-275">Matches instances named: / or /var</span></span> |

<span data-ttu-id="a67a3-276">Ehhez hasonlóan hello mintavételi időköznek az Ön által a szülő számláló érvényes tooall a gyermek számlálókat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-276">Similarly, hello sample interval that you choose for a parent counter applies tooall its child counters.</span></span> <span data-ttu-id="a67a3-277">Ez azt jelenti az összes hello gyermek számláló minta időközök és példányok vannak társítva együtt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-277">In other words, all hello child counter sample intervals and instances are tied together.</span></span>

### <a name="add-and-configure-performance-metrics-with-linux"></a><span data-ttu-id="a67a3-278">Hozzáadása és a Linux és metrikák konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a67a3-278">Add and configure performance metrics with Linux</span></span>
<span data-ttu-id="a67a3-279">Teljesítmény-mérőszámok toocollect hello konfigurációs az/etc/opt/microsoft/omsagent által vezérelt/&lt;munkaterület azonosítója&gt;/conf/omsagent.conf.</span><span class="sxs-lookup"><span data-stu-id="a67a3-279">Performance metrics toocollect are controlled by hello configuration in /etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf.</span></span> <span data-ttu-id="a67a3-280">Lásd: [elérhetővé teljesítménymutatók](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) elérhető osztályok és metrikáinak Linux OMS-ügynököt hello.</span><span class="sxs-lookup"><span data-stu-id="a67a3-280">See [Available performance metrics](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) for available classes and metrics for hello OMS Agent for Linux.</span></span>

<span data-ttu-id="a67a3-281">Minden objektumot, vagy a teljesítmény-mérőszámok toocollect kategóriáját definiálni kell egy hello konfigurációs fájlban `<source>` elemet.</span><span class="sxs-lookup"><span data-stu-id="a67a3-281">Each object, or category, of performance metrics toocollect should be defined in hello configuration file as a single `<source>` element.</span></span> <span data-ttu-id="a67a3-282">hello szintaxisa az alábbi hello mintát követi.</span><span class="sxs-lookup"><span data-stu-id="a67a3-282">hello syntax follows hello pattern below.</span></span>

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

<span data-ttu-id="a67a3-283">Ennek az elemnek a hello konfigurálható paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="a67a3-283">hello configurable parameters of this element are:</span></span>

* <span data-ttu-id="a67a3-284">**Objektum\_neve**: hello gyűjtemény hello objektum nevét.</span><span class="sxs-lookup"><span data-stu-id="a67a3-284">**Object\_name**: hello object name for hello collection.</span></span>
* <span data-ttu-id="a67a3-285">**Példány\_regex**: egy *reguláris kifejezés* mely példányok toocollect meghatározása.</span><span class="sxs-lookup"><span data-stu-id="a67a3-285">**Instance\_regex**: a *regular expression* defining which instances toocollect.</span></span> <span data-ttu-id="a67a3-286">érték hello: `.*` határozza meg az összes példányát.</span><span class="sxs-lookup"><span data-stu-id="a67a3-286">hello value: `.*` specifies all instances.</span></span> <span data-ttu-id="a67a3-287">csak hello toocollect processzor metrikáját \_teljes példányt kell megadni `_Total`.</span><span class="sxs-lookup"><span data-stu-id="a67a3-287">toocollect processor metrics for only hello \_Total instance, you could specify `_Total`.</span></span> <span data-ttu-id="a67a3-288">a toocollect folyamatmetrikák csak hello crond vagy sshd példány, meg kell megadni: `(crond|sshd)`.</span><span class="sxs-lookup"><span data-stu-id="a67a3-288">toocollect process metrics for only hello crond or sshd instances, you could specify: `(crond|sshd)`.</span></span>
* <span data-ttu-id="a67a3-289">**A számláló\_neve\_regex**: egy *reguláris kifejezés* mely számlálói (hello objektum) toocollect meghatározása.</span><span class="sxs-lookup"><span data-stu-id="a67a3-289">**Counter\_name\_regex**: a *regular expression* defining which counters (for hello object) toocollect.</span></span> <span data-ttu-id="a67a3-290">toocollect számlálók hello objektumhoz, adja meg: `.*`.</span><span class="sxs-lookup"><span data-stu-id="a67a3-290">toocollect all counters for hello object, specify: `.*`.</span></span> <span data-ttu-id="a67a3-291">toocollect terület számlálók csak felcserélése hello memória objektumhoz tartozó, kell megadni:`.+Swap.+`</span><span class="sxs-lookup"><span data-stu-id="a67a3-291">toocollect only swap space counters for hello memory object, you could specify: `.+Swap.+`</span></span>
* <span data-ttu-id="a67a3-292">**Időköz:**: hello gyakorisága, mely hello objektum számlálók összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="a67a3-292">**Interval:**: hello frequency at which hello object's counters are collected.</span></span>

<span data-ttu-id="a67a3-293">a metrikák hello alapértelmezett konfigurációja a következő:</span><span class="sxs-lookup"><span data-stu-id="a67a3-293">hello default configuration for performance metrics is:</span></span>

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### <a name="enable-mysql-performance-counters-using-linux-commands"></a><span data-ttu-id="a67a3-294">Linux-parancsok használatával MySQL teljesítményszámlálók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a67a3-294">Enable MySQL performance counters using Linux commands</span></span>
<span data-ttu-id="a67a3-295">MySQL vagy MariaDB Server észlelésekor hello számítógépen hello omsagent csomag telepítésekor a rendszer automatikusan telepíti a Teljesítményfigyelő MySQL kiszolgáló-szolgáltatója.</span><span class="sxs-lookup"><span data-stu-id="a67a3-295">If MySQL Server or MariaDB Server is detected on hello computer when hello omsagent bundle is installed, a performance monitoring provider for MySQL Server is automatically installed.</span></span> <span data-ttu-id="a67a3-296">Ez a szolgáltató toohello helyi MySQL/MariaDB kiszolgáló tooexpose teljesítményének statisztikájára csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="a67a3-296">This provider connects toohello local MySQL/MariaDB server tooexpose performance statistics.</span></span> <span data-ttu-id="a67a3-297">Érdekében, hogy a hello szolgáltató hello MySQL Server szüksége tooconfigure MySQL felhasználói hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-297">You need tooconfigure MySQL user credentials so that hello provider can access hello MySQL Server.</span></span>

<span data-ttu-id="a67a3-298">toodefine egy alapértelmezett felhasználói fiókot a hello MySQL server – localhost, a következő parancs használata hello.</span><span class="sxs-lookup"><span data-stu-id="a67a3-298">toodefine a default user account for hello MySQL server on localhost, use hello following command example.</span></span>

> [!NOTE]
> <span data-ttu-id="a67a3-299">hello hitelesítőadat-fájlt hello omsagent fiókkal olvashatónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a67a3-299">hello credentials file must be readable by hello omsagent account.</span></span> <span data-ttu-id="a67a3-300">Hello mycimprovauth parancs futtatásához használt omsgent ajánlott.</span><span class="sxs-lookup"><span data-stu-id="a67a3-300">Running hello mycimprovauth command as omsgent is recommended.</span></span>
>
>

```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>

sudo /opt/omi/bin/service_control restart
```


<span data-ttu-id="a67a3-301">Másik megoldásként megadhatja, hello MySQL hitelesítő adatokat egy fájlban szükséges hello fájl létrehozásával: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Hello mysql-alapú hitelesítés fájl figyeléshez MySQL hitelesítő adatai kezelésével kapcsolatos további információkért lásd: [figyelési hello hitelesítési fájlt a hitelesítő adatok kezelése MySQL](#manage-mysql-monitoring-credentials-in-the-authentication-file).</span><span class="sxs-lookup"><span data-stu-id="a67a3-301">Alternatively, you can specify hello required MySQL credentials in a file, by creating hello file: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. For more information about managing MySQL credentials for monitoring through hello mysql-auth file, see [Manage MySQL monitoring credentials in hello authentication file](#manage-mysql-monitoring-credentials-in-the-authentication-file).</span></span>

<span data-ttu-id="a67a3-302">Lásd: [MySQL teljesítményszámlálókkal szükséges engedélyek adatbázis](#database-permissions-required-for-mysql-performance-counters) hello MySQL felhasználói toocollect MySQL kiszolgáló teljesítményadatait szükséges objektum engedélyek vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="a67a3-302">See [Database permissions required for MySQL performance counters](#database-permissions-required-for-mysql-performance-counters) for details about object permissions required by hello MySQL user toocollect MySQL Server performance data.</span></span>

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a><span data-ttu-id="a67a3-303">Linux-parancsok használatával Apache HTTP Server teljesítményszámlálói engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a67a3-303">Enable Apache HTTP Server performance counters using Linux commands</span></span>
<span data-ttu-id="a67a3-304">Apache HTTP Server észlelésekor hello számítógépen hello omsagent csomag telepítésekor a rendszer automatikusan telepíti a teljesítményfigyelés-szolgáltató az Apache HTTP Server.</span><span class="sxs-lookup"><span data-stu-id="a67a3-304">If Apache HTTP Server is detected on hello computer when hello omsagent bundle is installed, a performance monitoring provider for Apache HTTP Server is automatically installed.</span></span> <span data-ttu-id="a67a3-305">Ez a szolgáltató egy Apache "module", amely töltse be az Apache HTTP Server hello rendelés tooaccess teljesítményadatok támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="a67a3-305">This provider relies on an Apache "module" that must be loaded into hello Apache HTTP Server in order tooaccess performance data.</span></span>

<span data-ttu-id="a67a3-306">Hello modul is betöltése a következő parancsot a következő hello:</span><span class="sxs-lookup"><span data-stu-id="a67a3-306">You can load hello module with hello following command:</span></span>

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

<span data-ttu-id="a67a3-307">toounload hello Apache figyelőmodult, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="a67a3-307">toounload hello Apache monitoring module, run hello following command:</span></span>

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="tooview-performance-data-with-log-analytics"></a><span data-ttu-id="a67a3-308">a Naplóelemzési tooview teljesítményadatok</span><span class="sxs-lookup"><span data-stu-id="a67a3-308">tooview performance data with Log Analytics</span></span>
1. <span data-ttu-id="a67a3-309">A hello Operations Management Suite portálját kattintson a hello napló keresése csempe.</span><span class="sxs-lookup"><span data-stu-id="a67a3-309">In hello Operations Management Suite portal, click hello Log Search tile.</span></span>
2. <span data-ttu-id="a67a3-310">Hello keresősávban, írja be a `* (Type=Perf)` tooview teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="a67a3-310">In hello search bar, type `* (Type=Perf)` tooview all performance counters.</span></span>

<span data-ttu-id="a67a3-311">Mivel OMS Windows teljesítményszámláló-adatokat is gyűjti, meg kell hatókör-le nyilas hello keresési tooLinux vonatkozó adatokat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-311">Because OMS also collects Windows performance counter data, you should scope-down hello search tooLinux-specific data.</span></span> <span data-ttu-id="a67a3-312">Igen hello alábbi példa látható teljesítmény adatok adott tooan példa Linux server Chorizo21 nevű.</span><span class="sxs-lookup"><span data-stu-id="a67a3-312">So, hello following example would show performance data specific tooan example Linux server named Chorizo21.</span></span>

```
Type=Perf Computer=chorizo*
```

![a találatok között látható példa kiszolgáló](./media/log-analytics-linux-agents/oms-perfsearch01.png)

<span data-ttu-id="a67a3-314">Hello eredményekben kattintson **metrikák** tooview hello számlálók az összegyűjtött adatokat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-314">In hello results, you can click **Metrics** tooview hello counters that data was collected for.</span></span> <span data-ttu-id="a67a3-315">Valós idejű adatok jelenik meg az egyes számlálóit diagramjait.</span><span class="sxs-lookup"><span data-stu-id="a67a3-315">Real-time data is shown as graphs for each counter.</span></span>

![metrics](./media/log-analytics-linux-agents/oms-perfmetrics01.png)

## <a name="syslog"></a><span data-ttu-id="a67a3-317">Rendszernapló</span><span class="sxs-lookup"><span data-stu-id="a67a3-317">Syslog</span></span>
<span data-ttu-id="a67a3-318">Syslog egy eseménynaplózás protokoll hasonló tooWindows eseménynaplók – egyaránt működéséhez hasonlóan az OMS Szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="a67a3-318">Syslog is an event logging protocol similar tooWindows Event logs—both operate similarly when displayed in OMS.</span></span>

### <a name="tooadd-a-new-linux-syslog-facility-in-oms"></a><span data-ttu-id="a67a3-319">egy új Linux syslog-szolgáltatást az OMS tooadd</span><span class="sxs-lookup"><span data-stu-id="a67a3-319">tooadd a new Linux syslog facility in OMS</span></span>
1. <span data-ttu-id="a67a3-320">A hello **beállítások** lapon az **adatok** , kattintson a **Syslog** és toohello balra hello plusz ikonra, írja be hello hello syslog-szolgáltatást, amelyet az tooadd.</span><span class="sxs-lookup"><span data-stu-id="a67a3-320">On hello **Settings** page under **Data** , click **Syslog** and then toohello left of hello plus icon, type hello name of hello syslog facility that you want tooadd.</span></span>
    <span data-ttu-id="a67a3-321">![Linux rendszernaplójába bejegyzett](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)</span><span class="sxs-lookup"><span data-stu-id="a67a3-321">![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)</span></span>
2. <span data-ttu-id="a67a3-322">Ha nem tudja hello létesítmény hello teljes nevét, egy részleges név indítható, és elérhető syslog létesítményekben listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a67a3-322">If you don’t know hello full name of hello facility, you can start typing a partial name and a list of available syslog facilities will appear.</span></span> <span data-ttu-id="a67a3-323">Ha megtalálta hello syslog-szolgáltatást, hogy meg szeretné, hogy tooadd, kattintson hello listában hello nevére, majd hello plusz ikon tooadd hello syslog-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a67a3-323">When you find hello syslog facility that you want tooadd, click hello name in hello list and then click hello plus icon tooadd hello syslog facility.</span></span>
3. <span data-ttu-id="a67a3-324">Miután hello létesítmény hello listája megjelenik a kijelölt színű sáv.</span><span class="sxs-lookup"><span data-stu-id="a67a3-324">After you add hello facility, it appears in hello list of highlighted with a colored bar.</span></span> <span data-ttu-id="a67a3-325">Ezután válassza ki a hello fokozatok (kategóriák syslog-létesítményt a személyes adatok), amelyet az toocollect.</span><span class="sxs-lookup"><span data-stu-id="a67a3-325">Next, choose hello severities (categories of syslog facility information) that you want toocollect.</span></span>
4. <span data-ttu-id="a67a3-326">Hello lap hello alján kattintson **mentése** toofinalize a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-326">At hello bottom of hello page click **Save** toofinalize your changes.</span></span> <span data-ttu-id="a67a3-327">hello végrehajtott konfigurációs módosításokat Linux általában 5 percnél regisztrált OMS-ben, majd küldése tooall hello OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-327">hello configuration changes that you’ve made are then sent tooall hello OMS Agents for Linux that are registered with OMS, typically within 5 minutes.</span></span>

### <a name="configure-linux-syslog-facilities-in-linux"></a><span data-ttu-id="a67a3-328">Linux syslog létesítményekben lévő Linux konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a67a3-328">Configure Linux syslog facilities in Linux</span></span>
<span data-ttu-id="a67a3-329">Syslog-események küldi hello a syslog démon, például rsyslog vagy a syslog-ng, helyi port tooa adott hello ügynök figyeli.</span><span class="sxs-lookup"><span data-stu-id="a67a3-329">Syslog events are sent from hello syslog daemon, for example rsyslog or syslog-ng, tooa local port that hello agent is listening on.</span></span> <span data-ttu-id="a67a3-330">Alapértelmezés szerint port 25224.</span><span class="sxs-lookup"><span data-stu-id="a67a3-330">By default, port 25224.</span></span> <span data-ttu-id="a67a3-331">Ha hello ügynök telepítve van, egy alapértelmezett Rendszernaplózás konfigurálásánál alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="a67a3-331">When hello agent is installed, a default syslog configuration is applied.</span></span> <span data-ttu-id="a67a3-332">Ez a következő címen található:</span><span class="sxs-lookup"><span data-stu-id="a67a3-332">This is found at:</span></span>

<span data-ttu-id="a67a3-333">Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf</span><span class="sxs-lookup"><span data-stu-id="a67a3-333">Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf</span></span>

<span data-ttu-id="a67a3-334">Syslog-ng: /etc/syslog-ng/syslog-ng.conf</span><span class="sxs-lookup"><span data-stu-id="a67a3-334">Syslog-ng: /etc/syslog-ng/syslog-ng.conf</span></span>

<span data-ttu-id="a67a3-335">hello alapértelmezett OMS ügynök Rendszernaplózás konfigurálásánál feltölti a syslog-események az összes létesítményekben kiegészített egy figyelmeztetés vagy magasabb.</span><span class="sxs-lookup"><span data-stu-id="a67a3-335">hello default OMS agent syslog configuration uploads syslog events from all facilities with a severity of warning or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="a67a3-336">Hello Rendszernaplózás konfigurálásánál szerkesztése esetén újra kell indítani a syslog démon hello hello módosítások tootake hatás.</span><span class="sxs-lookup"><span data-stu-id="a67a3-336">If you edit hello syslog configuration, you must restart hello syslog daemon for hello changes tootake effect.</span></span>
>
>

<span data-ttu-id="a67a3-337">hello alapértelmezett syslog beállítása az OMS-ügynököt hello Linux OMS történik:</span><span class="sxs-lookup"><span data-stu-id="a67a3-337">hello default syslog configuration for hello OMS Agent for Linux for OMS is:</span></span>

#### <a name="rsyslog"></a><span data-ttu-id="a67a3-338">Rsyslog</span><span class="sxs-lookup"><span data-stu-id="a67a3-338">Rsyslog</span></span>
```
kern.warning       @127.0.0.1:25224
user.warning       @127.0.0.1:25224
daemon.warning     @127.0.0.1:25224
auth.warning       @127.0.0.1:25224
syslog.warning     @127.0.0.1:25224
uucp.warning       @127.0.0.1:25224
authpriv.warning   @127.0.0.1:25224
ftp.warning        @127.0.0.1:25224
cron.warning       @127.0.0.1:25224
local0.warning     @127.0.0.1:25224
local1.warning     @127.0.0.1:25224
local2.warning     @127.0.0.1:25224
local3.warning     @127.0.0.1:25224
local4.warning     @127.0.0.1:25224
local5.warning     @127.0.0.1:25224
local6.warning     @127.0.0.1:25224
local7.warning     @127.0.0.1:25224
```

#### <a name="syslog-ng"></a><span data-ttu-id="a67a3-339">Syslog-ng</span><span class="sxs-lookup"><span data-stu-id="a67a3-339">Syslog-ng</span></span>
```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="tooview-all-syslog-events-with-log-analytics"></a><span data-ttu-id="a67a3-340">tooview Naplóelemzési a Syslog-események</span><span class="sxs-lookup"><span data-stu-id="a67a3-340">tooview all Syslog events with Log Analytics</span></span>
1. <span data-ttu-id="a67a3-341">A hello Operations Management Suite portálját, kattintson a hello **naplófájl-keresési** csempére.</span><span class="sxs-lookup"><span data-stu-id="a67a3-341">In hello Operations Management Suite portal, click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="a67a3-342">A hello **napló felügyeleti** csoportosítás, válasszon egy előre meghatározott syslog keresést, és válassza ki egy toorun azt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-342">In hello **Log Management** grouping, choose a predefined syslog search and then select one toorun it.</span></span>

<span data-ttu-id="a67a3-343">Ez a példa bemutatja az összes Syslog-események.</span><span class="sxs-lookup"><span data-stu-id="a67a3-343">This example shows all Syslog events.</span></span>

![Syslog-események megjelennek a keresési napló](./media/log-analytics-linux-agents/oms-linux-syslog.png)

<span data-ttu-id="a67a3-345">Most részletezhető keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="a67a3-345">Now you can drill into search results.</span></span>

## <a name="linux-alerts"></a><span data-ttu-id="a67a3-346">Linux-riasztások</span><span class="sxs-lookup"><span data-stu-id="a67a3-346">Linux alerts</span></span>
<span data-ttu-id="a67a3-347">Ha a Linux-gépeket, majd az OMS ezekhez az eszközökhöz által előállított hello riasztásokat fogadhat Nagios vagy Zabbix toomanage.</span><span class="sxs-lookup"><span data-stu-id="a67a3-347">If you use Nagios or Zabbix toomanage your Linux machines, then OMS can receive hello alerts generated from those tools.</span></span> <span data-ttu-id="a67a3-348">Azonban nincs jelenleg metódus tooconfigure bejövő riasztási adat hello OMS-portálon.</span><span class="sxs-lookup"><span data-stu-id="a67a3-348">However, there is currently no method tooconfigure incoming alert data using hello OMS portal.</span></span> <span data-ttu-id="a67a3-349">Ehelyett szüksége lesz egy konfigurációs fájl toostart küldő riasztások tooOMS tooedit.</span><span class="sxs-lookup"><span data-stu-id="a67a3-349">Instead, you will need tooedit a config file toostart sending alerts tooOMS.</span></span>

### <a name="collect-alerts-from-nagios"></a><span data-ttu-id="a67a3-350">Nagios riasztások gyűjtése</span><span class="sxs-lookup"><span data-stu-id="a67a3-350">Collect alerts from Nagios</span></span>
<span data-ttu-id="a67a3-351">toocollect riasztások Nagios-kiszolgálótól, a következő konfigurációs módosításainak toomake hello kell.</span><span class="sxs-lookup"><span data-stu-id="a67a3-351">toocollect alerts from a Nagios server, you need toomake hello following configuration changes.</span></span>

1. <span data-ttu-id="a67a3-352">Támogatás hello felhasználói **omsagent** olvasási hozzáférés toohello Nagios naplófájl (azaz /var/log/nagios/nagios.log).</span><span class="sxs-lookup"><span data-stu-id="a67a3-352">Grant hello user **omsagent** read access toohello Nagios log file (i.e. /var/log/nagios/nagios.log).</span></span> <span data-ttu-id="a67a3-353">Feltéve, hogy hello nagios.log fájl hello csoport tulajdonosa **nagios** , hozzáadhat hello felhasználót **omsagent** toohello **nagios** csoport.</span><span class="sxs-lookup"><span data-stu-id="a67a3-353">Assuming hello nagios.log file is owned by hello group **nagios** , you can add hello user **omsagent** toohello **nagios** group.</span></span>

    ```
    sudo usermod –a -G nagios omsagent
    ```
2. <span data-ttu-id="a67a3-354">Hello omsagent.confconfiguration fájl módosítása (/ etc/opt/microsoft/omsagent/&lt;munkaterület azonosítója&gt;/conf/omsagent.conf).</span><span class="sxs-lookup"><span data-stu-id="a67a3-354">Modify hello omsagent.confconfiguration file (/etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf).</span></span> <span data-ttu-id="a67a3-355">Győződjön meg arról, bejegyzéseket a következő hello jelen, és nem megjegyzésként ki:</span><span class="sxs-lookup"><span data-stu-id="a67a3-355">Ensure hello following entries are present and not commented out:</span></span>

    ```
    <source>
    type tail
    #Update path toopoint tooyour nagios.log
    path /var/log/nagios/nagios.log
    format none
    tag oms.nagios
    </source>

    <filter oms.nagios>
    type filter_nagios_log
    </filter>
    ```
3. <span data-ttu-id="a67a3-356">Indítsa újra a hello omsagent démon:</span><span class="sxs-lookup"><span data-stu-id="a67a3-356">Restart hello omsagent daemon:</span></span>

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="collect-alerts-from-zabbix"></a><span data-ttu-id="a67a3-357">Zabbix riasztások gyűjtése</span><span class="sxs-lookup"><span data-stu-id="a67a3-357">Collect alerts from Zabbix</span></span>
<span data-ttu-id="a67a3-358">Nagios fenti, hasonló lépéseket toothose kivételével toospecify egy felhasználó és a jelszó lesz szüksége lesz hajtsa végre a Zabbix kiszolgálóról toocollect riasztások *törölje a szöveget*.</span><span class="sxs-lookup"><span data-stu-id="a67a3-358">toocollect alerts from a Zabbix server, you'll perform similar steps toothose for Nagios above, except you'll need toospecify a user and password in *clear text*.</span></span> <span data-ttu-id="a67a3-359">Ez nem ideális, de hamarosan valószínűleg változik.</span><span class="sxs-lookup"><span data-stu-id="a67a3-359">This is not ideal, but will likely change soon.</span></span> <span data-ttu-id="a67a3-360">tooaddress probléma, javasoljuk, hogy hello felhasználó létrehozása, és csak az engedély toomonitor biztosítania.</span><span class="sxs-lookup"><span data-stu-id="a67a3-360">tooaddress this issue, we recommend that you create hello user and grant it permission toomonitor only.</span></span>

<span data-ttu-id="a67a3-361">Egy hello omsagent.conf konfigurációs fájl példa részét (/ etc/opt/microsoft/omsagent/&lt;munkaterület azonosítója&gt;/conf/omsagent.conf) a Zabbix hello következő kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="a67a3-361">An example section of hello omsagent.conf configuration file  (/etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf) for Zabbix should resemble hello following:</span></span>

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### <a name="view-alerts-in-log-analytics-search"></a><span data-ttu-id="a67a3-362">A Naplóelemzési keresési riasztások megtekintése</span><span class="sxs-lookup"><span data-stu-id="a67a3-362">View alerts in Log Analytics search</span></span>
<span data-ttu-id="a67a3-363">Miután konfigurálta a Linux rendszerű számítógépek toosend riasztások tooOMS, néhány egyszerű napló keresési lekérdezések tooview hello értesítések is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a67a3-363">After you've configured your Linux computers toosend alerts tooOMS, you can use a few simple log search queries tooview hello alerts.</span></span> <span data-ttu-id="a67a3-364">hello következő keresési lekérdezés példa adja vissza az összes rögzített hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="a67a3-364">hello following search query example returns all hello recorded alerts that were generated.</span></span> <span data-ttu-id="a67a3-365">Például ha valamilyen probléma lép fel az informatikai infrastruktúra, majd jár-e a következő példában a lekérdezés jelezhetik, ahol előfordulhat, hogy származnak hello probléma hello.</span><span class="sxs-lookup"><span data-stu-id="a67a3-365">For example, if some sort of problem occurs in your IT infrastructure, then results for hello following example query might indicate where hello problem might originate.</span></span> <span data-ttu-id="a67a3-366">És egyszerűen megtekintheti a toohello riasztások forrás rendszer toohelp keskeny által a vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-366">And, you can easily drill in toohello alerts by source system toohelp narrow your investigation.</span></span> <span data-ttu-id="a67a3-367">hello előnyt az jelenti, hogy nem szükségszerűen rendelkezik toogo toovarious felügyeleti rendszerekkel hello indítás – feltéve, hogy az értesítések küldése tooOMS megkezdése van.</span><span class="sxs-lookup"><span data-stu-id="a67a3-367">hello benefit is that you don't necessarily have toogo toovarious management systems from hello start—provided that your alerts are sent tooOMS, you can start there.</span></span>

```
Type=Alert
```

#### <a name="tooview-all-nagios-alerts-with-log-analytics"></a><span data-ttu-id="a67a3-368">minden Nagios riasztásokkal Naplóelemzési tooview</span><span class="sxs-lookup"><span data-stu-id="a67a3-368">tooview all Nagios alerts with Log Analytics</span></span>
1. <span data-ttu-id="a67a3-369">A hello Operations Management Suite portálját, kattintson a hello **naplófájl-keresési** csempére.</span><span class="sxs-lookup"><span data-stu-id="a67a3-369">In hello Operations Management Suite portal, click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="a67a3-370">Hello lekérdezés sávon írja be a következő keresési lekérdezés hello</span><span class="sxs-lookup"><span data-stu-id="a67a3-370">In hello query bar, type hello following search query</span></span>

    ```
    Type=Alert SourceSystem=Nagios
    ```
   ![Naplófájl-keresési látható Nagios-riasztások](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

<span data-ttu-id="a67a3-372">Miután meggyőződött arról, hogy a keresési eredmények hello, részletezhető további részleteket például *AlertState*.</span><span class="sxs-lookup"><span data-stu-id="a67a3-372">After you see hello search results, you can drill into additional details such as *AlertState*.</span></span>

### <a name="tooview-all-zabbix-alerts-with-log-analytics"></a><span data-ttu-id="a67a3-373">tooview Naplóelemzési összes Zabbix riasztások</span><span class="sxs-lookup"><span data-stu-id="a67a3-373">tooview all Zabbix alerts with Log Analytics</span></span>
1. <span data-ttu-id="a67a3-374">A hello Operations Management Suite portálját, kattintson a hello **naplófájl-keresési** csempére.</span><span class="sxs-lookup"><span data-stu-id="a67a3-374">In hello Operations Management Suite portal, click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="a67a3-375">Hello lekérdezés sávon írja be a következő keresési lekérdezés hello</span><span class="sxs-lookup"><span data-stu-id="a67a3-375">In hello query bar, type hello following search query</span></span>

    ```
    Type=Alert SourceSystem=Zabbix
    ```
   ![Naplófájl-keresési látható Zabbix-riasztások](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

<span data-ttu-id="a67a3-377">Miután meggyőződött arról, hogy a keresési eredmények hello, részletezhető további részleteket például *AlertName*.</span><span class="sxs-lookup"><span data-stu-id="a67a3-377">After you see hello search results, you can drill into additional details such as *AlertName*.</span></span>

## <a name="compatibility-with-system-center-operations-manager"></a><span data-ttu-id="a67a3-378">A System Center Operations Manager kompatibilitása</span><span class="sxs-lookup"><span data-stu-id="a67a3-378">Compatibility with System Center Operations Manager</span></span>
<span data-ttu-id="a67a3-379">hello Linux OMS-ügynököt hello System Center Operations Manager ügynök ügynök bináris fájlokat osztanak meg.</span><span class="sxs-lookup"><span data-stu-id="a67a3-379">hello OMS Agent for Linux shares agent binaries with hello System Center Operations Manager agent.</span></span> <span data-ttu-id="a67a3-380">Hello OMS-ügynököt Linux telepítését felügyelete jelenleg az Operations Manager rendszerre frissítés hello OMI és az SCX-csomagokat a hello tooa újabb verziójú.</span><span class="sxs-lookup"><span data-stu-id="a67a3-380">Installing hello OMS Agent for Linux on a system currently managed by Operations Manager upgrades hello OMI and SCX packages on hello computer tooa newer version.</span></span> <span data-ttu-id="a67a3-381">hello OMS-ügynököt a Linux és a System Center 2012 R2 által támogatott.</span><span class="sxs-lookup"><span data-stu-id="a67a3-381">hello OMS Agent for Linux and System Center 2012 R2 are compatible.</span></span> <span data-ttu-id="a67a3-382">Azonban **System Center 2012 SP1 és a korábbi verziók jelenleg nem kompatibilis vagy nem támogatott a hello Linux OMS-ügynököt.**</span><span class="sxs-lookup"><span data-stu-id="a67a3-382">However, **System Center 2012 SP1 and earlier versions are currently not compatible or supported with hello OMS Agent for Linux.**</span></span>

> [!NOTE]
> <span data-ttu-id="a67a3-383">Ha hello Linux OMS-ügynököt, amely jelenleg nem kezeli az Operations Manager telepített tooa számítógép, és később szeretné toomanage hello számítógépen az Operations Manager, módosítania kell hello OMI konfigurációs előtt hello számítógép felderítése.</span><span class="sxs-lookup"><span data-stu-id="a67a3-383">If hello OMS Agent for Linux is installed tooa computer that is not currently managed by Operations Manager, and you later want toomanage hello computer with Operations Manager, you must modify hello OMI configuration before you discover hello computer.</span></span> <span data-ttu-id="a67a3-384">**Ez a lépés hello Operations Manager-ügynök van telepítve előtt hello OMS-ügynököt Linux esetén nincs szükség.**</span><span class="sxs-lookup"><span data-stu-id="a67a3-384">**This step is not needed if hello Operations Manager agent is installed before hello OMS Agent for Linux.**</span></span>
>
>

### <a name="tooenable-hello-oms-agent-for-linux-toocommunicate-with-operations-manager"></a><span data-ttu-id="a67a3-385">a Linux toocommunicate az Operations Manager OMS-ügynököt tooenable hello</span><span class="sxs-lookup"><span data-stu-id="a67a3-385">tooenable hello OMS Agent for Linux toocommunicate with Operations Manager</span></span>
1. <span data-ttu-id="a67a3-386">Hello fájl /etc/opt/omi/conf/omiserver.conf szerkesztése</span><span class="sxs-lookup"><span data-stu-id="a67a3-386">Edit hello file /etc/opt/omi/conf/omiserver.conf</span></span>
2. <span data-ttu-id="a67a3-387">Győződjön meg arról, hogy hello kezdődő **httpsport =** hello 1270-es port határozza meg.</span><span class="sxs-lookup"><span data-stu-id="a67a3-387">Ensure that hello line beginning with **httpsport=** defines hello port 1270.</span></span> <span data-ttu-id="a67a3-388">Többek között`httpsport=1270`</span><span class="sxs-lookup"><span data-stu-id="a67a3-388">Such as `httpsport=1270`</span></span>
3. <span data-ttu-id="a67a3-389">Indítsa újra a hello OMI-kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="a67a3-389">Restart hello OMI server:</span></span>

    ```
    sudo /opt/omi/bin/service_control restart
    ```

## <a name="database-permissions-required-for-mysql-performance-counters"></a><span data-ttu-id="a67a3-390">A MySQL-teljesítményszámlálók szükséges adatbázis-engedélyek</span><span class="sxs-lookup"><span data-stu-id="a67a3-390">Database permissions required for MySQL performance counters</span></span>
<span data-ttu-id="a67a3-391">toogrant engedélyek tooa MySQL figyelési felhasználó, hello támogatást nyújtó felhasználónak rendelkeznie kell hello "GRANT option" jogosultsággal, valamint az engedély hello jogosultság.</span><span class="sxs-lookup"><span data-stu-id="a67a3-391">toogrant permissions tooa MySQL monitoring user, hello granting user must have hello 'GRANT option' privilege as well as hello privilege being granted.</span></span>

<span data-ttu-id="a67a3-392">Ahhoz, hogy hello MySQL felhasználói tooreturn teljesítmény adatok hello felhasználói kell érik el a következő lekérdezések toohello:</span><span class="sxs-lookup"><span data-stu-id="a67a3-392">In order for hello MySQL User tooreturn performance data hello user will need access toohello following queries:</span></span>

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

<span data-ttu-id="a67a3-393">Ezenkívül toothese lekérdezések hello MySQL felhasználói megköveteli a következő alapértelmezett táblák VÁLASSZA hozzáférés toohello:</span><span class="sxs-lookup"><span data-stu-id="a67a3-393">In addition toothese queries hello MySQL user requires SELECT access toohello following default tables:</span></span>

* <span data-ttu-id="a67a3-394">entitástulajdonos</span><span class="sxs-lookup"><span data-stu-id="a67a3-394">information_schema</span></span>
* <span data-ttu-id="a67a3-395">MySQL</span><span class="sxs-lookup"><span data-stu-id="a67a3-395">mysql</span></span>

<span data-ttu-id="a67a3-396">Ezek a jogosultságok hello következő grant parancsok futtatásával kaphatja meg.</span><span class="sxs-lookup"><span data-stu-id="a67a3-396">These privileges can be granted by running hello following grant commands.</span></span>

```
GRANT SELECT ON information_schema.* too‘monuser’@’localhost’;
GRANT SELECT ON mysql.* too‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-hello-authentication-file"></a><span data-ttu-id="a67a3-397">MySQL figyelési hello hitelesítési fájlt a hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="a67a3-397">Manage MySQL monitoring credentials in hello authentication file</span></span>
<span data-ttu-id="a67a3-398">hello alábbi szakaszokban MySQL hitelesítő adatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="a67a3-398">hello following sections help you manage MySQL credentials.</span></span>

### <a name="configure-hello-mysql-omi-provider"></a><span data-ttu-id="a67a3-399">Hello MySQL OMI szolgáltató konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a67a3-399">Configure hello MySQL OMI provider</span></span>
<span data-ttu-id="a67a3-400">hello MySQL OMI szolgáltató előre konfigurált MySQL felhasználónak, és MySQL klienskódtárak segítségével telepített tooquery hello teljesítményállapotot/rendelésinformációkat hello MySQL példányból.</span><span class="sxs-lookup"><span data-stu-id="a67a3-400">hello MySQL OMI provider requires a preconfigured MySQL user and installed MySQL client libraries in order tooquery hello performance/health information from hello MySQL instance.</span></span>

### <a name="mysql-omi-authentication-file"></a><span data-ttu-id="a67a3-401">MySQL OMI hitelesítési fájl</span><span class="sxs-lookup"><span data-stu-id="a67a3-401">MySQL OMI authentication file</span></span>
<span data-ttu-id="a67a3-402">MySQL OMI szolgáltató egy hitelesítési fájl toodetermine milyen bind-cím és port hello MySQL példány figyeli és a mi toouse toogather metrikák hitelesítő adatait használja.</span><span class="sxs-lookup"><span data-stu-id="a67a3-402">MySQL OMI provider uses an authentication file toodetermine what bind-address and port hello MySQL instance is listening on and what credentials toouse toogather metrics.</span></span> <span data-ttu-id="a67a3-403">Telepítési hello MySQL OMI során szolgáltató keresni kezdi a MySQL my.cnf konfigurációs fájljait (alapértelmezett hely) bind-cím és port, valamint részlegesen set hello MySQL OMI hitelesítési fájlt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-403">During installation hello MySQL OMI provider will scan MySQL my.cnf configuration files (default locations) for bind-address and port and partially set hello MySQL OMI authentication file.</span></span>

<span data-ttu-id="a67a3-404">előre generált MySQL OMI hitelesítési fájl toocomplete MySQL server-példány, figyelés felvétele hello megfelelő könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="a67a3-404">toocomplete monitoring of a MySQL server instance, add a pre-generated MySQL OMI authentication file into hello correct directory.</span></span>

### <a name="authentication-file-format"></a><span data-ttu-id="a67a3-405">Hitelesítési fájlformátum</span><span class="sxs-lookup"><span data-stu-id="a67a3-405">Authentication file format</span></span>
<span data-ttu-id="a67a3-406">hello MySQL OMI hitelesítési fájl egy szövegfájlt, amely kapcsolatos információkat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="a67a3-406">hello MySQL OMI authentication file is a text file that contains information about:</span></span>

* <span data-ttu-id="a67a3-407">Port</span><span class="sxs-lookup"><span data-stu-id="a67a3-407">Port</span></span>
* <span data-ttu-id="a67a3-408">A kötés-cím</span><span class="sxs-lookup"><span data-stu-id="a67a3-408">Bind-Address</span></span>
* <span data-ttu-id="a67a3-409">MySQL-felhasználónév</span><span class="sxs-lookup"><span data-stu-id="a67a3-409">MySQL username</span></span>
* <span data-ttu-id="a67a3-410">A Base64 kódolású jelszó</span><span class="sxs-lookup"><span data-stu-id="a67a3-410">Base64 encoded password</span></span>

<span data-ttu-id="a67a3-411">hello MySQL OMI hitelesítési fájl csak jön létre az olvasási/írási toohello Linux felhasználói jogosultságokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="a67a3-411">hello MySQL OMI authentication file only grants privileges for read/write toohello Linux user that generated it.</span></span>

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

<span data-ttu-id="a67a3-412">Egy alapértelmezett MySQL OMI hitelesítési fájl tartalmaz egy alapértelmezett példány és egy portszámot, attól függően, hogy milyen információ elérhető és a MySQL-konfigurációs fájlt talált hello elemzett.</span><span class="sxs-lookup"><span data-stu-id="a67a3-412">A default MySQL OMI authentication file contains a default instance and a port number depending on what information is available and parsed from hello found MySQL configuration file.</span></span>

<span data-ttu-id="a67a3-413">hello alapértelmezett példány egy egyszerűbb egy Linux gazdagép több MySQL példány kezelése azt jelenti, hogy toomake, és 0-s port hello példány helyén.</span><span class="sxs-lookup"><span data-stu-id="a67a3-413">hello default instance is a means toomake managing multiple MySQL instances on one Linux host easier, and is denoted by hello instance with port 0.</span></span> <span data-ttu-id="a67a3-414">Minden hozzáadott példány tulajdonsága nincs beállítva alapértelmezett példányból hello öröklik.</span><span class="sxs-lookup"><span data-stu-id="a67a3-414">All added instances will inherit properties set from hello default instance.</span></span> <span data-ttu-id="a67a3-415">Például "3308" porton figyel a MySQL-példány kerül hozzáadásra, ha hello alapértelmezett példány bind-cím, a felhasználónév és a Base64-kódolású jelszó lesz használt tootry kell és figyeli a 3308 hello példány figyelése.</span><span class="sxs-lookup"><span data-stu-id="a67a3-415">For example, if MySQL instance listening on port '3308' is added, hello default instance's bind-address, username, and Base64 encoded password will be used tootry and monitor hello instance listening on 3308.</span></span> <span data-ttu-id="a67a3-416">Ha 3308 hello példánya kötve tooanother cím és használ hello azonos MySQL felhasználónév és jelszó pár csak hello respecification hello, bind-cím van szükség, és hello egyéb tulajdonságok öröklik.</span><span class="sxs-lookup"><span data-stu-id="a67a3-416">If hello instance on 3308 is binded tooanother address and uses hello same MySQL username and password pair only hello respecification of hello bind-address is needed and hello other properties will be inherited.</span></span>

<span data-ttu-id="a67a3-417">Példák hello hitelesítési fájl hello alábbihoz.</span><span class="sxs-lookup"><span data-stu-id="a67a3-417">Examples of hello authentication file resemble hello following.</span></span>

<span data-ttu-id="a67a3-418">Alapértelmezett példány és -port 3308 példány:</span><span class="sxs-lookup"><span data-stu-id="a67a3-418">Default instance and instance with port 3308:</span></span>

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

<span data-ttu-id="a67a3-419">Alapértelmezett példány és -példány portja 3308 + különböző Base 64 kódolású jelszó:</span><span class="sxs-lookup"><span data-stu-id="a67a3-419">Default instance and instance with port 3308 + different Base 64 encoded password:</span></span>

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| <span data-ttu-id="a67a3-420">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="a67a3-420">**Property**</span></span> | <span data-ttu-id="a67a3-421">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="a67a3-421">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="a67a3-422">Port</span><span class="sxs-lookup"><span data-stu-id="a67a3-422">Port</span></span> |<span data-ttu-id="a67a3-423">Port hello aktuális port hello példánya által figyelt MySQL jelöli.</span><span class="sxs-lookup"><span data-stu-id="a67a3-423">Port represents hello current port hello MySQL instance is listening on.</span></span>  <span data-ttu-id="a67a3-424">hello port 0 azt jelenti, hogy hello tulajdonságait a következő alapértelmezett példányát használja.</span><span class="sxs-lookup"><span data-stu-id="a67a3-424">hello port 0 implies that hello properties following are used for default instance.</span></span> |
| <span data-ttu-id="a67a3-425">A kötés-cím</span><span class="sxs-lookup"><span data-stu-id="a67a3-425">Bind-Address</span></span> |<span data-ttu-id="a67a3-426">hello címet kötni az hello aktuális MySQL bind-cím</span><span class="sxs-lookup"><span data-stu-id="a67a3-426">hello Bind Address is hello current MySQL bind-address</span></span> |
| <span data-ttu-id="a67a3-427">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="a67a3-427">username</span></span> |<span data-ttu-id="a67a3-428">A hello felhasználó felhasználóneve, hello MySQL toouse toomonitor hello MySQL server-példányt kívánja.</span><span class="sxs-lookup"><span data-stu-id="a67a3-428">This hello username of hello MySQL user you wish toouse toomonitor hello MySQL server instance.</span></span> |
| <span data-ttu-id="a67a3-429">A Base64 kódolású jelszó</span><span class="sxs-lookup"><span data-stu-id="a67a3-429">Base64 encoded Password</span></span> |<span data-ttu-id="a67a3-430">Ez a hello MySQL figyelési felhasználó Base64 kódolású hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="a67a3-430">This is hello password of hello MySQL monitoring user encoded in Base64.</span></span> |
| <span data-ttu-id="a67a3-431">Automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="a67a3-431">AutoUpdate</span></span> |<span data-ttu-id="a67a3-432">Hello MySQL OMI szolgáltató frissítésekor hello szolgáltató megkeresheti a módosításokat a hello my.cnf fájlban, és felülírja hello MySQL OMI hitelesítési fájlt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-432">When hello MySQL OMI Provider is upgraded hello provider will rescan for changes in hello my.cnf file and overwrite hello MySQL OMI Authentication file.</span></span> <span data-ttu-id="a67a3-433">Állítsa be a jelző tootrue és hamis értéket, attól függően, hogy a szükséges frissítések toohello MySQL OMI hitelesítési fájlt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-433">Set this flag tootrue or false depending on required updates toohello MySQL OMI authentication file.</span></span> |

#### <a name="authentication-file-location"></a><span data-ttu-id="a67a3-434">Hitelesítési fájl helye</span><span class="sxs-lookup"><span data-stu-id="a67a3-434">Authentication file location</span></span>
<span data-ttu-id="a67a3-435">legyen hello a következő helyen található, és "mysql-alapú hitelesítés" nevű hello MySQL OMI hitelesítési fájlt:</span><span class="sxs-lookup"><span data-stu-id="a67a3-435">hello MySQL OMI Authentication File should be located in hello following location and named "mysql-auth":</span></span>

<span data-ttu-id="a67a3-436">/var/OPT/Microsoft/MySQL-cimprov/auth/omsagent/MySQL-auth</span><span class="sxs-lookup"><span data-stu-id="a67a3-436">/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth</span></span>

<span data-ttu-id="a67a3-437">hello fájl (és a hitelesítési/omsagent könyvtár) hello omsagent felhasználói kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="a67a3-437">hello file (and auth/omsagent directory) should be owned by hello omsagent user.</span></span>

## <a name="agent-logs"></a><span data-ttu-id="a67a3-438">Ügynök naplók</span><span class="sxs-lookup"><span data-stu-id="a67a3-438">Agent logs</span></span>
<span data-ttu-id="a67a3-439">hello naplókat hello Linux OMS-ügynököt a jelenleg:</span><span class="sxs-lookup"><span data-stu-id="a67a3-439">hello logs for hello OMS Agent for Linux is at:</span></span>

<span data-ttu-id="a67a3-440">/ var/opt/microsoft/omsagent/&lt;munkaterület azonosítója&gt;/log/</span><span class="sxs-lookup"><span data-stu-id="a67a3-440">/var/opt/microsoft/omsagent/&lt;workspace id&gt;/log/</span></span>

<span data-ttu-id="a67a3-441">hello hello naplókat omsconfig (ügynök konfigurációjának) program Linux OMS-ügynök van:</span><span class="sxs-lookup"><span data-stu-id="a67a3-441">hello logs for hello OMS Agent for Linux for omsconfig (agent configuration) program is at:</span></span>

<span data-ttu-id="a67a3-442">/ var/opt/microsoft/omsconfig/log /</span><span class="sxs-lookup"><span data-stu-id="a67a3-442">/var/opt/microsoft/omsconfig/log/</span></span>

<span data-ttu-id="a67a3-443">Hello OMI és az SCX-összetevők naplóinak (amely teljesítményadatokat metrikák) jelenleg:</span><span class="sxs-lookup"><span data-stu-id="a67a3-443">Logs for hello OMI and SCX components (which provide performance metrics data) is at:</span></span>

<span data-ttu-id="a67a3-444">/ var/opt/omi/log/és /var/opt/microsoft/scx/log</span><span class="sxs-lookup"><span data-stu-id="a67a3-444">/var/opt/omi/log/ and /var/opt/microsoft/scx/log</span></span>

## <a name="troubleshooting-hello-oms-agent-for-linux"></a><span data-ttu-id="a67a3-445">Hibaelhárítási hello OMS-ügynököt a Linux</span><span class="sxs-lookup"><span data-stu-id="a67a3-445">Troubleshooting hello OMS Agent for Linux</span></span>
<span data-ttu-id="a67a3-446">A következő információk toodiagnose hello használja, és a gyakori problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="a67a3-446">Use hello following information toodiagnose and troubleshoot common issues.</span></span>

<span data-ttu-id="a67a3-447">Hibaelhárítási információkat ebben a szakaszban hello egyike sem segít, ha használhatja hello a következő erőforrások toohelp hárítsa el a problémát.</span><span class="sxs-lookup"><span data-stu-id="a67a3-447">If none of hello troubleshooting information in this section helps you, you can also use hello following resources toohelp resolve your problem.</span></span>

* <span data-ttu-id="a67a3-448">Az ügyfelek a Premier szintű támogatási be tud jelentkezni egy támogatási esetet keresztül [Premier](https://premier.microsoft.com/)</span><span class="sxs-lookup"><span data-stu-id="a67a3-448">Customers with Premier support can log a support case via [Premier](https://premier.microsoft.com/)</span></span>
* <span data-ttu-id="a67a3-449">Az Azure támogatási szerződésből rendelkező ügyfelek is bejelentkezhetnek támogatási eseteket hello [Azure-portálon](https://manage.windowsazure.com/?getsupport=true)</span><span class="sxs-lookup"><span data-stu-id="a67a3-449">Customers with Azure support agreements can log support cases in hello [Azure portal](https://manage.windowsazure.com/?getsupport=true)</span></span>
* <span data-ttu-id="a67a3-450">Fájl egy [GitHub-probléma](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)</span><span class="sxs-lookup"><span data-stu-id="a67a3-450">File a [GitHub Issue](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)</span></span>
* <span data-ttu-id="a67a3-451">Visszajelzési fórumon ötleteket és a hibajelentés toocreate [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)</span><span class="sxs-lookup"><span data-stu-id="a67a3-451">Feedback forum for ideas and toocreate a bug report [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)</span></span>

### <a name="important-log-locations"></a><span data-ttu-id="a67a3-452">Fontos naplók helye</span><span class="sxs-lookup"><span data-stu-id="a67a3-452">Important log locations</span></span>
| <span data-ttu-id="a67a3-453">Fájl</span><span class="sxs-lookup"><span data-stu-id="a67a3-453">File</span></span> | <span data-ttu-id="a67a3-454">Elérési út</span><span class="sxs-lookup"><span data-stu-id="a67a3-454">Path</span></span> |
| --- | --- |
| <span data-ttu-id="a67a3-455">Linux-naplófájl OMS-ügynököt</span><span class="sxs-lookup"><span data-stu-id="a67a3-455">OMS Agent for Linux Log File</span></span> |`/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log ` |
| <span data-ttu-id="a67a3-456">OMS-ügynök konfigurációs naplófájl</span><span class="sxs-lookup"><span data-stu-id="a67a3-456">OMS Agent Configuration Log File</span></span> |`/var/opt/microsoft/omsconfig/omsconfig.log` |

### <a name="important-configuration-files"></a><span data-ttu-id="a67a3-457">Fontos konfigurációs fájlok</span><span class="sxs-lookup"><span data-stu-id="a67a3-457">Important configuration files</span></span>
| <span data-ttu-id="a67a3-458">Catergory</span><span class="sxs-lookup"><span data-stu-id="a67a3-458">Catergory</span></span> | <span data-ttu-id="a67a3-459">Fájl helye</span><span class="sxs-lookup"><span data-stu-id="a67a3-459">File Location</span></span> |
| --- | --- |
| <span data-ttu-id="a67a3-460">Rendszernapló</span><span class="sxs-lookup"><span data-stu-id="a67a3-460">Syslog</span></span> |<span data-ttu-id="a67a3-461">`/etc/syslog-ng/syslog-ng.conf`vagy `/etc/rsyslog.conf` vagy`/etc/rsyslog.d/95-omsagent.conf`</span><span class="sxs-lookup"><span data-stu-id="a67a3-461">`/etc/syslog-ng/syslog-ng.conf` or `/etc/rsyslog.conf` or `/etc/rsyslog.d/95-omsagent.conf`</span></span> |
| <span data-ttu-id="a67a3-462">Teljesítmény, Nagios, Zabbix, OMS kimeneti és általános ügynök</span><span class="sxs-lookup"><span data-stu-id="a67a3-462">Performance, Nagios, Zabbix, OMS output and general agent</span></span> |`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` |
| <span data-ttu-id="a67a3-463">További konfigurációk</span><span class="sxs-lookup"><span data-stu-id="a67a3-463">Additional configurations</span></span> |`/etc/opt/microsoft/omsagent/<workspace id>/omsagent.d/*.conf` |

> [!NOTE]
> <span data-ttu-id="a67a3-464">Ha az OMS-portálon konfigurációs engedélyezve van, a rendszer felülírja az teljesítményszámlálók és syslog szerkesztési konfigurációs fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-464">Editing configuration files for performance counters and syslog are overwritten if OMS Portal Configuration is enabled.</span></span> <span data-ttu-id="a67a3-465">Letilthatja az OMS-portálon hello (az összes csomópont) konfigurációs vagy az egyetlen csomópont hello következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a67a3-465">You can disable configuration in hello OMS Portal (for all nodes) or for single nodes by running hello following:</span></span>
>
>

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a><span data-ttu-id="a67a3-466">A hibakeresési naplózást engedélyező</span><span class="sxs-lookup"><span data-stu-id="a67a3-466">Enable debug logging</span></span>
<span data-ttu-id="a67a3-467">tooenable hibakeresési naplózást, használhat hello OMS kimeneti beépülő modul és részletes kimenet.</span><span class="sxs-lookup"><span data-stu-id="a67a3-467">tooenable debug logging, you can use hello OMS output plugin and verbose output.</span></span>

#### <a name="oms-output-plugin"></a><span data-ttu-id="a67a3-468">OMS kimeneti beépülő modul</span><span class="sxs-lookup"><span data-stu-id="a67a3-468">OMS output plugin</span></span>
<span data-ttu-id="a67a3-469">FluentD lehetővé teszi, hogy hello beépülő modul toospecify naplózási szintjeinek bemenetekhez és kimenetekhez a különböző naplózási szintet.</span><span class="sxs-lookup"><span data-stu-id="a67a3-469">FluentD allows hello plugin toospecify logging levels for different log levels for inputs and outputs.</span></span> <span data-ttu-id="a67a3-470">a különböző naplózási szint OMS kimeneti toospecify hello általános ügynök konfigurációjának hello szerkesztése `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-470">toospecify a different log level for OMS output, edit hello general agent configuration in hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` file.</span></span>

<span data-ttu-id="a67a3-471">Kis hatótávolságú hello alsó hello konfigurációs fájl, módosítsa a hello `log_level` tulajdonságot `info` túl`debug`.</span><span class="sxs-lookup"><span data-stu-id="a67a3-471">Near hello bottom of hello configuration file, change hello `log_level` property from `info` too`debug`.</span></span>

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

<span data-ttu-id="a67a3-472">A hibakeresési naplózás lehetővé teszi a kötegelni toosee feltöltések toohello típusát, számát adatelemek és az idő toosend elválasztott OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="a67a3-472">Debug logging allows you toosee batched uploads toohello OMS Service separated by type, number of data items, and time taken toosend.</span></span>

<span data-ttu-id="a67a3-473">*Példa engedélyezett hibakeresési napló:*</span><span class="sxs-lookup"><span data-stu-id="a67a3-473">*Example debug enabled log:*</span></span>

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a><span data-ttu-id="a67a3-474">Részletes kimenet</span><span class="sxs-lookup"><span data-stu-id="a67a3-474">Verbose output</span></span>
<span data-ttu-id="a67a3-475">Hello OMS kimeneti beépülő modul helyett is a kimenetre küldheti adatelemek közvetlenül túl`stdout`, ez az OMS-ügynököt hello Linux naplófájl látható.</span><span class="sxs-lookup"><span data-stu-id="a67a3-475">Instead of using hello OMS output plugin, you can also output data items directly too`stdout`, which is visible in hello OMS Agent for Linux log file.</span></span>

<span data-ttu-id="a67a3-476">Hello OMS általános ügynök konfigurációs fájlban a következő `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, Megjegyzés kibővített hello OMS kimeneti beépülő modul hozzáadásával egy `#` minden sor elé.</span><span class="sxs-lookup"><span data-stu-id="a67a3-476">In hello OMS general agent configuration file at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, comment-out hello OMS output plugin by adding a `#` in front of each line.</span></span>

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

<span data-ttu-id="a67a3-477">Az alábbiakban hello kimeneti beépülő modul, hello Megjegyzés eltávolítása a következő szakasz hello eltávolításával hello `#` szimbólum minden sor hello elején.</span><span class="sxs-lookup"><span data-stu-id="a67a3-477">Below hello output plugin, remove hello comment in hello following section by removing hello `#` symbol at hello beginning of each line.</span></span>

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-hello-log"></a><span data-ttu-id="a67a3-478">Továbbított Syslog-üzeneteket nem jelennek meg hello napló</span><span class="sxs-lookup"><span data-stu-id="a67a3-478">Forwarded Syslog messages do not appear in hello log</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a67a3-479">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="a67a3-479">Probable causes</span></span>
* <span data-ttu-id="a67a3-480">hello alkalmazott konfiguráció toohello Linux kiszolgálón nem küldött hello létesítmények gyűjtemény engedélyezése és/vagy szintek naplózása</span><span class="sxs-lookup"><span data-stu-id="a67a3-480">hello configuration applied toohello Linux server does not allow collection of hello sent facilities and/or log levels</span></span>
* <span data-ttu-id="a67a3-481">Syslog nem nem lesznek továbbítva megfelelően toohello Linux kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a67a3-481">Syslog is not being forwarded correctly toohello Linux server</span></span>
* <span data-ttu-id="a67a3-482">üzenetek száma másodpercenként továbbított hello száma túl nagyok hello OMS-ügynököt a Linux toohandle hello-alapkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="a67a3-482">hello number of messages being forwarded per second are too large for hello base configuration of hello OMS Agent for Linux toohandle</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a67a3-483">Megoldások</span><span class="sxs-lookup"><span data-stu-id="a67a3-483">Resolutions</span></span>
* <span data-ttu-id="a67a3-484">Győződjön meg arról, hogy az OMS-portálon hello hello a konfigurálás a Syslog van minden hello létesítményekben és hello megfelelő naplózási szintek</span><span class="sxs-lookup"><span data-stu-id="a67a3-484">Verify that hello configuration in hello OMS Portal for Syslog has all hello facilities and hello correct log levels</span></span>
  * <span data-ttu-id="a67a3-485">**OMS-portálon > Beállítások > adatok > Syslog**</span><span class="sxs-lookup"><span data-stu-id="a67a3-485">**OMS Portal > Settings > Data > Syslog**</span></span>
* <span data-ttu-id="a67a3-486">Győződjön meg arról, hogy natív syslog üzenetküldési démonok (`rsyslog`, `syslog-ng`) is képes tooreceive továbbított köszönőüzenetei</span><span class="sxs-lookup"><span data-stu-id="a67a3-486">Verify that native syslog messaging daemons (`rsyslog`, `syslog-ng`) are able tooreceive hello forwarded messages</span></span>
* <span data-ttu-id="a67a3-487">Ellenőrizze a tűzfal beállításait hello Syslog server tooensure, hogy üzenetek nem blokkolja a</span><span class="sxs-lookup"><span data-stu-id="a67a3-487">Check firewall settings on hello Syslog server tooensure that messages are not being blocked</span></span>
* <span data-ttu-id="a67a3-488">A Syslog-üzenet hello segítségével tooOMS szimulálása `logger` paranccsal – például:</span><span class="sxs-lookup"><span data-stu-id="a67a3-488">Simulate a Syslog message tooOMS using hello `logger` command - for example:</span></span>
  * `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-toooms-when-using-a-proxy"></a><span data-ttu-id="a67a3-489">Problémái tooOMS proxy használata esetén</span><span class="sxs-lookup"><span data-stu-id="a67a3-489">Problems connecting tooOMS when using a proxy</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a67a3-490">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="a67a3-490">Probable causes</span></span>
* <span data-ttu-id="a67a3-491">hello proxy határozni, ha telepítése és konfigurálása hello ügynök helytelen</span><span class="sxs-lookup"><span data-stu-id="a67a3-491">hello proxy specified when installing and configuring hello agent is incorrect</span></span>
* <span data-ttu-id="a67a3-492">hello OMS végpontok nincsenek az adatközpontban található whitelistested</span><span class="sxs-lookup"><span data-stu-id="a67a3-492">hello OMS Service endpoints are not whitelistested in your datacenter</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a67a3-493">Megoldások</span><span class="sxs-lookup"><span data-stu-id="a67a3-493">Resolutions</span></span>
* <span data-ttu-id="a67a3-494">Telepítse újra a hello OMS-ügynököt a következő parancsot hello kapcsolóval hello használata Linux `-v` engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a67a3-494">Reinstall hello OMS Agent for Linux using hello following command with hello option `-v` enabled.</span></span> <span data-ttu-id="a67a3-495">Ez lehetővé teszi a részletes kimenet hello ügynök hello proxy toohello OMS szolgáltatás keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="a67a3-495">This allows verbose output of hello agent connecting through hello proxy toohello OMS Service.</span></span>
  * `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  * <span data-ttu-id="a67a3-496">Tekintse meg az OMS-proxy: hello dokumentációt [hello ügynökök konfigurálása HTTP-proxykiszolgáló való használatra](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)</span><span class="sxs-lookup"><span data-stu-id="a67a3-496">Review hello documentation for OMS proxy at [Configuring hello agent for use with an HTTP proxy server](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)</span></span>
* <span data-ttu-id="a67a3-497">Ellenőrizze, hogy a következő OMS Szolgáltatásvégpontok hello szerepel az engedélyezési listán</span><span class="sxs-lookup"><span data-stu-id="a67a3-497">Verify that hello following OMS Service endpoints are whitelisted</span></span>

| <span data-ttu-id="a67a3-498">Ügynök erőforrása</span><span class="sxs-lookup"><span data-stu-id="a67a3-498">Agent Resource</span></span> | <span data-ttu-id="a67a3-499">Portok</span><span class="sxs-lookup"><span data-stu-id="a67a3-499">Ports</span></span> |
| --- | --- |
| <span data-ttu-id="a67a3-500">&#42;. ods.opinsights.Azure.com</span><span class="sxs-lookup"><span data-stu-id="a67a3-500">&#42;.ods.opinsights.azure.com</span></span> |<span data-ttu-id="a67a3-501">443-as port</span><span class="sxs-lookup"><span data-stu-id="a67a3-501">Port 443</span></span> |
| <span data-ttu-id="a67a3-502">&#42;. OMS.opinsights.Azure.com</span><span class="sxs-lookup"><span data-stu-id="a67a3-502">&#42;.oms.opinsights.azure.com</span></span> |<span data-ttu-id="a67a3-503">443-as port</span><span class="sxs-lookup"><span data-stu-id="a67a3-503">Port 443</span></span> |
| <span data-ttu-id="a67a3-504">ods.systemcenteradvisor.com</span><span class="sxs-lookup"><span data-stu-id="a67a3-504">ods.systemcenteradvisor.com</span></span> |<span data-ttu-id="a67a3-505">443-as port</span><span class="sxs-lookup"><span data-stu-id="a67a3-505">Port 443</span></span> |
| <span data-ttu-id="a67a3-506">&#42;.blob.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="a67a3-506">&#42;.blob.core.windows.net/</span></span> |<span data-ttu-id="a67a3-507">443-as port</span><span class="sxs-lookup"><span data-stu-id="a67a3-507">Port 443</span></span> |

### <a name="a-403-error-is-displayed-when-onboarding"></a><span data-ttu-id="a67a3-508">A 403-as hibaüzenet jelenik meg ha bevezetése</span><span class="sxs-lookup"><span data-stu-id="a67a3-508">A 403 error is displayed when onboarding</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a67a3-509">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="a67a3-509">Probable causes</span></span>
* <span data-ttu-id="a67a3-510">hello dátum és idő nem megfelelő Linux-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="a67a3-510">hello date and time are incorrect on Linux Server</span></span>
* <span data-ttu-id="a67a3-511">hello munkaterületének Azonosítóját és kulcsát használt helytelenek</span><span class="sxs-lookup"><span data-stu-id="a67a3-511">hello Workspace ID and Workspace Key used are incorrect</span></span>

#### <a name="resolution"></a><span data-ttu-id="a67a3-512">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="a67a3-512">Resolution</span></span>
* <span data-ttu-id="a67a3-513">Ellenőrizze a Linux-kiszolgálóra a hello hello idő `date` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a67a3-513">Verify hello time on your Linux server with hello `date` command.</span></span> <span data-ttu-id="a67a3-514">Ha adatok érték nagyobb, mint hello vagy kevesebb mint 15 percet a hello a jelenlegi időpontnál, akkor bevezetési sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="a67a3-514">If hello data is greater than or less than 15 minutes from hello current time, then onboarding fails.</span></span> <span data-ttu-id="a67a3-515">toocorrect, hello dátumot és/vagy a Linux-kiszolgálóra az időzóna frissítése.</span><span class="sxs-lookup"><span data-stu-id="a67a3-515">toocorrect this, update hello date and/or timezone of your Linux server.</span></span>
* <span data-ttu-id="a67a3-516">hello Linux OMS-ügynök legújabb verziójának hello értesítést küld, ha időeltérése bevezetési hibája okozza.</span><span class="sxs-lookup"><span data-stu-id="a67a3-516">hello latest version of hello OMS Agent for Linux notifies you if a time difference is causing onboarding failure</span></span>
* <span data-ttu-id="a67a3-517">Újra-beépített használatával hello megfelelő munkaterület Azonosítóját és kulcsát.</span><span class="sxs-lookup"><span data-stu-id="a67a3-517">Re-onboard using hello correct Workspace ID and Workspace Key.</span></span> <span data-ttu-id="a67a3-518">Lásd: [hello parancssorból bevezetési](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) további információt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-518">See  [Onboarding using hello command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>

### <a name="a-500-error-or-404-error-appears-in-hello-log-file-after-onboarding"></a><span data-ttu-id="a67a3-519">500 hiba vagy 404-es hiba után jelenik meg hello naplófájlban bevezetése</span><span class="sxs-lookup"><span data-stu-id="a67a3-519">A 500 error or 404 error appears in hello log file after onboarding</span></span>
<span data-ttu-id="a67a3-520">Ez az egy ismert hiba, amely a hello első az adatok feltöltése a Linux az OMS-munkaterület során fordul elő.</span><span class="sxs-lookup"><span data-stu-id="a67a3-520">This is a known issue that occurs during hello first upload of Linux data into an OMS workspace.</span></span> <span data-ttu-id="a67a3-521">Ez nem befolyásolja, küldött adatok mennyisége vagy az egyéb problémákat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-521">This does not affect data being sent or other problems.</span></span> <span data-ttu-id="a67a3-522">Kezdeti hello hibák figyelmen kívül hagyhatja bevezetése.</span><span class="sxs-lookup"><span data-stu-id="a67a3-522">You can ignore hello errors when initially onboarding.</span></span>

### <a name="nagios-data-does-not-appear-in-hello-oms-portal"></a><span data-ttu-id="a67a3-523">Nagios-adatok nem jelennek meg hello OMS-portálon</span><span class="sxs-lookup"><span data-stu-id="a67a3-523">Nagios data does not appear in hello OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a67a3-524">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="a67a3-524">Probable causes</span></span>
* <span data-ttu-id="a67a3-525">hello omsagent felhasználó nem rendelkezik engedélyekkel tooread hello Nagios naplófájlból</span><span class="sxs-lookup"><span data-stu-id="a67a3-525">hello omsagent user does not have permissions tooread from hello Nagios log file</span></span>
* <span data-ttu-id="a67a3-526">továbbra is megjegyzésként hello omsagent.conf fájlban hello Nagios forrás és a szűrő szakasz</span><span class="sxs-lookup"><span data-stu-id="a67a3-526">hello Nagios source and filter sections are still commented in hello omsagent.conf file</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a67a3-527">Megoldások</span><span class="sxs-lookup"><span data-stu-id="a67a3-527">Resolutions</span></span>
* <span data-ttu-id="a67a3-528">Adja hozzá a hello omsagent felhasználói rendelés tooread hello Nagios fájlból.</span><span class="sxs-lookup"><span data-stu-id="a67a3-528">Add hello omsagent user in order tooread from hello Nagios file.</span></span> <span data-ttu-id="a67a3-529">Lásd: [Nagios riasztások](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) további információt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-529">See [Nagios alerts](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) for more information.</span></span>
* <span data-ttu-id="a67a3-530">A Linux általános konfigurációs fájlban a következő OMS-ügynököt hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ügyeljen arra, hogy **mindkét** Nagios forrás- és a szűrő szakaszok észrevételeit eltávolítva, a következő példa hasonló toohello hello.</span><span class="sxs-lookup"><span data-stu-id="a67a3-530">In hello OMS Agent for Linux general configuration file at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ensure that **both** hello Nagios source and filter sections have comments removed, similar toohello following example.</span></span>

```
<source>
  type tail
  path /var/log/nagios/nagios.log
  format none
  tag oms.nagios
</source>

<filter oms.nagios>
  type filter_nagios_log
</filter>
```


### <a name="linux-data-doesnt-appear-in-hello-oms-portal"></a><span data-ttu-id="a67a3-531">Linux-adatok nem jelennek meg hello OMS-portálon</span><span class="sxs-lookup"><span data-stu-id="a67a3-531">Linux data doesn't appear in hello OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a67a3-532">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="a67a3-532">Probable causes</span></span>
* <span data-ttu-id="a67a3-533">Bevezetési toohello OMS-szolgáltatás nem tudta</span><span class="sxs-lookup"><span data-stu-id="a67a3-533">Onboarding toohello OMS Service failed</span></span>
* <span data-ttu-id="a67a3-534">Kapcsolat toohello OMS szolgáltatás le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="a67a3-534">Connection toohello OMS Service is blocked</span></span>
* <span data-ttu-id="a67a3-535">hello OMS-ügynököt a Linux-adatok biztonsági másolatban</span><span class="sxs-lookup"><span data-stu-id="a67a3-535">hello OMS Agent for Linux data is backed-up</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a67a3-536">Megoldások</span><span class="sxs-lookup"><span data-stu-id="a67a3-536">Resolutions</span></span>
* <span data-ttu-id="a67a3-537">Ellenőrizze, hogy bevezetési toohello OMS szolgáltatás sikeres volt ellenőrzi, hogy hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` létezik-e.</span><span class="sxs-lookup"><span data-stu-id="a67a3-537">Verify that onboarding toohello OMS Service was successful by verifying that hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` exists.</span></span>
* <span data-ttu-id="a67a3-538">Újra-beépített használatával hello omsadmin.sh parancssor.</span><span class="sxs-lookup"><span data-stu-id="a67a3-538">Re-onboard using hello omsadmin.sh command line.</span></span> <span data-ttu-id="a67a3-539">Lásd: [hello parancssorból bevezetési](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) további információt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-539">See [Onboarding using hello command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>
* <span data-ttu-id="a67a3-540">Ha proxyt használ, akkor hello proxy hibaelhárítási fent leírt lépésekkel</span><span class="sxs-lookup"><span data-stu-id="a67a3-540">If using a proxy, use hello proxy troubleshooting steps above</span></span>
* <span data-ttu-id="a67a3-541">Bizonyos esetekben hello Linux OMS-ügynököt hello OMS szolgáltatás nem tud kommunikálni hello ügynök adatok esetén a biztonsági mentésben toohello teljes puffer mérete 50 MB.</span><span class="sxs-lookup"><span data-stu-id="a67a3-541">In some cases, when hello OMS Agent for Linux cannot communicate with hello OMS Service, data on hello Agent is backed-up toohello full buffer size of 50 MB.</span></span> <span data-ttu-id="a67a3-542">Indítsa újra az OMS-ügynököt hello Linux hello futtatásával `/opt/microsoft/omsagent/bin/service_control restart` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a67a3-542">Restart hello OMS Agent for Linux by running hello `/opt/microsoft/omsagent/bin/service_control restart` command.</span></span>
  >[AZURE.NOTE] <span data-ttu-id="a67a3-543">Ez a probléma fennáll ügynök verziója 1.1.0-28 és újabb verziók.</span><span class="sxs-lookup"><span data-stu-id="a67a3-543">This issue is fixed in Agent version 1.1.0-28 and later.</span></span>

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-hello-oms-portal"></a><span data-ttu-id="a67a3-544">Syslog Linux teljesítmény számláló konfiguráció nem alkalmazható az hello OMS-portálon</span><span class="sxs-lookup"><span data-stu-id="a67a3-544">Syslog Linux performance counter configuration is not applied in hello OMS portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a67a3-545">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="a67a3-545">Probable causes</span></span>
* <span data-ttu-id="a67a3-546">hello konfigurációs ügynök hello Linux OMS-ügynököt nem rendelkezik hello legfrissebb konfigurációt lekért hello OMS-portálon.</span><span class="sxs-lookup"><span data-stu-id="a67a3-546">hello configuration agent in hello OMS Agent for Linux has not retrieved hello latest configuration from hello OMS portal.</span></span>
* <span data-ttu-id="a67a3-547">hello hello portálon módosított beállítások a rendszer nem alkalmazta</span><span class="sxs-lookup"><span data-stu-id="a67a3-547">hello revised settings in hello portal were not applied</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a67a3-548">Megoldások</span><span class="sxs-lookup"><span data-stu-id="a67a3-548">Resolutions</span></span>
<span data-ttu-id="a67a3-549">`omsconfig`hello konfigurációs ügynök hello OMS-ügynököt a szolgál, amely lekéri az OMS-portálon konfigurációs módosítások 5 percenként Linux.</span><span class="sxs-lookup"><span data-stu-id="a67a3-549">`omsconfig` is hello configuration agent in hello OMS Agent for Linux that retrieves OMS portal configuration changes every 5 minutes.</span></span> <span data-ttu-id="a67a3-550">Ez a konfiguráció nem található alkalmazott toohello OMS-ügynököt a Linux konfigurációs fájlok `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.</span><span class="sxs-lookup"><span data-stu-id="a67a3-550">This configuration is then applied toohello OMS Agent for Linux configuration files located at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.</span></span>

* <span data-ttu-id="a67a3-551">Bizonyos esetekben hello OMS-ügynököt a Linux-ügynök konfigurációs nem feltétlenül tudja toocommunicate hello beállítása szolgáltatással sem alkalmazza a legújabb konfigurációt eredményez.</span><span class="sxs-lookup"><span data-stu-id="a67a3-551">In some cases, hello OMS Agent for Linux configuration agent might not be able toocommunicate with hello portal configuration service resulting in latest configuration not being applied.</span></span>
* <span data-ttu-id="a67a3-552">Győződjön meg arról, hogy hello `omsconfig` -ügynök telepítve van a hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="a67a3-552">Verify that hello `omsconfig` agent is installed with hello following:</span></span>

  * <span data-ttu-id="a67a3-553">`dpkg --list omsconfig` vagy `rpm -qi omsconfig`</span><span class="sxs-lookup"><span data-stu-id="a67a3-553">`dpkg --list omsconfig` or `rpm -qi omsconfig`</span></span>
  * <span data-ttu-id="a67a3-554">Ha nincs telepítve, telepítse újra a legújabb verziójának hello hello OMS-ügynököt Linux</span><span class="sxs-lookup"><span data-stu-id="a67a3-554">If not installed, reinstall hello latest version of hello OMS Agent for Linux</span></span>
* <span data-ttu-id="a67a3-555">Győződjön meg arról, hogy hello `omsconfig` ügynök kommunikálhatnak hello OMS szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="a67a3-555">Verify that hello `omsconfig` agent can communicate with hello OMS service</span></span>

  * <span data-ttu-id="a67a3-556">Futtassa a hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` parancs</span><span class="sxs-lookup"><span data-stu-id="a67a3-556">Run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` command</span></span>
    * <span data-ttu-id="a67a3-557">hello fenti adja vissza hello konfigurálása az ügynök lekéri a hello portal, beleértve a Syslog-beállítások, Linux teljesítményszámlálók és egyéni naplókat</span><span class="sxs-lookup"><span data-stu-id="a67a3-557">hello command above returns hello configuration that agent retrieves from hello portal, including Syslog settings, Linux performance counters, and custom logs</span></span>
    * <span data-ttu-id="a67a3-558">Ha a fenti hello parancs sikertelen, futtassa a hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a67a3-558">If hello command above fails, run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` command.</span></span> <span data-ttu-id="a67a3-559">Ez a parancs kényszeríti hello omsconfig ügynök toocommunicate hello OMS szolgáltatás tooretrieve hello legújabb konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="a67a3-559">This command forces hello omsconfig agent toocommunicate with hello OMS service tooretrieve hello latest configuration.</span></span>

### <a name="custom-linux-log-data-does-not-appear-in-hello-oms-portal"></a><span data-ttu-id="a67a3-560">Egyéni Linux naplóadatokat nem jelenik meg hello OMS-portálon</span><span class="sxs-lookup"><span data-stu-id="a67a3-560">Custom Linux log data does not appear in hello OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a67a3-561">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="a67a3-561">Probable causes</span></span>
* <span data-ttu-id="a67a3-562">Bevezetési tooOMS szolgáltatás nem tudta</span><span class="sxs-lookup"><span data-stu-id="a67a3-562">Onboarding tooOMS Service failed</span></span>
* <span data-ttu-id="a67a3-563">Hello **konfigurációs toomy Linux-kiszolgálókon a következő Apply hello** beállítás nincs kiválasztva</span><span class="sxs-lookup"><span data-stu-id="a67a3-563">hello **Apply hello following configuration toomy Linux Servers** setting has not been selected</span></span>
* <span data-ttu-id="a67a3-564">omsconfig nem kiválasztotta hello legújabb egyéni napló hello portálról</span><span class="sxs-lookup"><span data-stu-id="a67a3-564">omsconfig has not picked up hello latest custom log from hello portal</span></span>
* <span data-ttu-id="a67a3-565">Hello `omsagent` használata nem tooaccess hello egyéni napló tooa engedélyek probléma miatt vagy `omsagent` nem található.</span><span class="sxs-lookup"><span data-stu-id="a67a3-565">hello `omsagent` use is unable tooaccess hello custom log due tooa permissions problem or `omsagent` was not found.</span></span> <span data-ttu-id="a67a3-566">Ebben az esetben látni fogja a következő kimeneti hello:</span><span class="sxs-lookup"><span data-stu-id="a67a3-566">In this case, you'll see hello following output:</span></span>
  * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  * `[DATETIME] [error]: file not accessible by omsagent.`
* <span data-ttu-id="a67a3-567">Ez az hello hello OMS-ügynököt a Linux-verzió 1.1.0-217 javított versenyhelyzet ismert problémái</span><span class="sxs-lookup"><span data-stu-id="a67a3-567">This is a known issue with hello Race Condition that was fixed in hello OMS Agent for Linux version 1.1.0-217</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a67a3-568">Megoldások</span><span class="sxs-lookup"><span data-stu-id="a67a3-568">Resolutions</span></span>
* <span data-ttu-id="a67a3-569">Győződjön meg arról, hogy sikeresen előkészítve, hogy hello meghatározásával `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` fájl létezik-e.</span><span class="sxs-lookup"><span data-stu-id="a67a3-569">Verify that you've successfully onboarded, by determining whether hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` file exists.</span></span>
  * <span data-ttu-id="a67a3-570">Ha szükséges, előkészítésére újra a hello omsadmin.sh parancssor használatával.</span><span class="sxs-lookup"><span data-stu-id="a67a3-570">If needed, onboard again using hello omsadmin.sh command line.</span></span> <span data-ttu-id="a67a3-571">Lásd: [hello parancssorból bevezetési](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) további információt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-571">See [Onboarding using hello command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>
* <span data-ttu-id="a67a3-572">A hello OMS-portálon, a **beállítások** a hello **adatok** lapra, győződjön meg arról, hogy hello **konfigurációs toomy Linux-kiszolgálókon a következő Apply hello** beállítás</span><span class="sxs-lookup"><span data-stu-id="a67a3-572">In hello OMS Portal, under **Settings** on hello **Data** tab, ensure that hello **Apply hello following configuration toomy Linux Servers** setting is selected</span></span>  
  <span data-ttu-id="a67a3-573">![konfigurációjának alkalmazása](./media/log-analytics-linux-agents/customloglinuxenabled.png)</span><span class="sxs-lookup"><span data-stu-id="a67a3-573">![apply configuration](./media/log-analytics-linux-agents/customloglinuxenabled.png)</span></span>
* <span data-ttu-id="a67a3-574">Győződjön meg arról, hogy hello `omsconfig` ügynök kommunikálhatnak hello OMS szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="a67a3-574">Verify that hello `omsconfig` agent can communicate with hello OMS service</span></span>

  * <span data-ttu-id="a67a3-575">Futtassa a hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` parancs</span><span class="sxs-lookup"><span data-stu-id="a67a3-575">Run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` command</span></span>
  * <span data-ttu-id="a67a3-576">hello fenti adja vissza hello konfigurálása az ügynök lekéri az hello Portal, beleértve a Syslog-beállítások, Linux teljesítményszámlálók és egyéni naplókat</span><span class="sxs-lookup"><span data-stu-id="a67a3-576">hello command above returns hello configuration that agent retrieves from hello Portal, including Syslog settings, Linux performance counters, and custom Logs</span></span>
  * <span data-ttu-id="a67a3-577">Ha a fenti hello parancs sikertelen, futtassa a hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a67a3-577">If hello command above fails, run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` command.</span></span> <span data-ttu-id="a67a3-578">Ez a parancs kényszeríti hello omsconfig ügynök toocommunicate OMS szolgáltatással, és hello legfrissebb konfigurációt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a67a3-578">This command forces hello omsconfig agent toocommunicate with OMS service and retrieve hello latest configuration.</span></span>

<span data-ttu-id="a67a3-579">Linux felhasználói jogosultsággal rendelkező felhasználóként fut az OMS-ügynököt hello helyett `root`, hello OMS-ügynököt, a Linux fut a hello `omsagent` felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a67a3-579">Instead of hello OMS Agent for Linux user running as a privileged user `root`, hello OMS Agent for Linux runs as hello `omsagent` user.</span></span> <span data-ttu-id="a67a3-580">A legtöbb esetben kifejezett engedélyekké kell megadott sorrendben tooread toohello felhasználó bizonyos fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-580">In most cases, explicit permission must be granted toohello user in order tooread certain files.</span></span>

<span data-ttu-id="a67a3-581">toogrant engedély túl`omsagent` felhasználói, futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="a67a3-581">toogrant permission too`omsagent` user, run hello following commands:</span></span>

1. <span data-ttu-id="a67a3-582">Adja hozzá a hello `omsagent` felhasználói tooa adott csoportban található`sudo usermod -a -G <GROUPNAME> <USERNAME>`</span><span class="sxs-lookup"><span data-stu-id="a67a3-582">Add hello `omsagent` user tooa specific group with `sudo usermod -a -G <GROUPNAME> <USERNAME>`</span></span>
2. <span data-ttu-id="a67a3-583">Általános olvasási hozzáférést toohello fájl szükséges a engedélyezése`sudo chmod -R ugo+rw <FILE DIRECTORY>`</span><span class="sxs-lookup"><span data-stu-id="a67a3-583">Grant universal read access toohello required file with `sudo chmod -R ugo+rw <FILE DIRECTORY>`</span></span>

<span data-ttu-id="a67a3-584">Egy ismert probléma az hello hello OMS-ügynököt a Linux-verzió 1.1.0-217 javított versenyhelyzet van.</span><span class="sxs-lookup"><span data-stu-id="a67a3-584">There is a known issue with hello Race Condition that was fixed in hello OMS Agent for Linux version 1.1.0-217.</span></span> <span data-ttu-id="a67a3-585">Toohello legújabb ügynök frissítése, után futtassa a következő parancs tooget hello legújabb verziójának hello kimeneti beépülő modul hello:</span><span class="sxs-lookup"><span data-stu-id="a67a3-585">After updating toohello latest agent, run hello following command tooget hello latest version of hello output plugin:</span></span>

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf
```

## <a name="known-limitations"></a><span data-ttu-id="a67a3-586">Ismert korlátozásai</span><span class="sxs-lookup"><span data-stu-id="a67a3-586">Known limitations</span></span>
<span data-ttu-id="a67a3-587">Tekintse át a következő szakaszok toolearn kapcsolatos Linux hello OMS-ügynököt a jelenlegi korlátozások hello.</span><span class="sxs-lookup"><span data-stu-id="a67a3-587">Review hello following sections toolearn about current limitations of hello OMS Agent for Linux.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="a67a3-588">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="a67a3-588">Azure Diagnostics</span></span>
<span data-ttu-id="a67a3-589">Az Azure-ban futó Linux virtuális gépek emellett további lépésekre lehet szükség tooallow adatgyűjtés Azure Diagnostics és az Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="a67a3-589">For Linux virtual machines running in Azure, additional steps may be required tooallow data collection by Azure Diagnostics and Operations Management Suite.</span></span> <span data-ttu-id="a67a3-590">**2.2-es verziója** hello Linux diagnosztika kiterjesztése az hello OMS-ügynököt a Linux-való kompatibilitás szükséges.</span><span class="sxs-lookup"><span data-stu-id="a67a3-590">**Version 2.2** of hello Diagnostics Extension for Linux is required for compatibility with hello OMS Agent for Linux.</span></span>

<span data-ttu-id="a67a3-591">Telepítésének és konfigurálásának hello diagnosztikai bővítmény Linux további információkért lásd: [hello Azure CLI parancs tooenable Linux diagnosztikai bővítmény használatára](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).</span><span class="sxs-lookup"><span data-stu-id="a67a3-591">For more information on installing and configuring hello Diagnostic Extension for Linux, see [Use hello Azure CLI command tooenable Linux Diagnostic Extension](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).</span></span>

<span data-ttu-id="a67a3-592">**2.0 too2.2 Azure CLI ASM hello Linux diagnosztika bővítmény frissítése:**</span><span class="sxs-lookup"><span data-stu-id="a67a3-592">**Upgrading hello Linux Diagnostics Extension from 2.0 too2.2 Azure CLI ASM:**</span></span>

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

<span data-ttu-id="a67a3-593">**ARM**</span><span class="sxs-lookup"><span data-stu-id="a67a3-593">**ARM**</span></span>

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

<span data-ttu-id="a67a3-594">A parancs példákat hivatkozhatnak PrivateConfig.json nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="a67a3-594">These command examples reference a file named PrivateConfig.json.</span></span> <span data-ttu-id="a67a3-595">hello formátuma, hogy a fájl a következő minta hello kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="a67a3-595">hello format of that file should resemble hello following sample.</span></span>

```
    {
    "storageAccountName":"hello storage account tooreceive data",
    "storageAccountKey":"hello key of hello account"
    }
```

### <a name="sysklog-is-not-supported"></a><span data-ttu-id="a67a3-596">Sysklog nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="a67a3-596">Sysklog is not supported</span></span>
<span data-ttu-id="a67a3-597">Rsyslog vagy a syslog-ng van szükség toocollect syslog-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="a67a3-597">Either rsyslog or syslog-ng are required toocollect syslog messages.</span></span> <span data-ttu-id="a67a3-598">syslog-események gyűjtése nem támogatott hello alapértelmezett syslog démon a Red Hat Enterprise Linux-, CentOS, és Oracle Linux verziója (sysklog) 5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="a67a3-598">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="a67a3-599">ezek azokat a terjesztéseket, a jelen verziójában a syslog-adatot toocollect hello rsyslog démon kell telepíteni, és tooreplace sysklog konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a67a3-599">toocollect syslog data from this version of these distributions, hello rsyslog daemon should be installed and configured tooreplace sysklog.</span></span> <span data-ttu-id="a67a3-600">A sysklog lecserélését rsyslog további információkért lásd: [telepítése RPM újonnan létrehozott hello rsyslog](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).</span><span class="sxs-lookup"><span data-stu-id="a67a3-600">For more information on replacing sysklog with rsyslog, see [Install hello newly built rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a67a3-601">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a67a3-601">Next Steps</span></span>
* <span data-ttu-id="a67a3-602">[Log Analytics-megoldások hozzáadása hello megoldások gyűjtemény](log-analytics-add-solutions.md) tooadd funkciókat és összefog adatokat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-602">[Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md) tooadd functionality and gather data.</span></span>
* <span data-ttu-id="a67a3-603">Ismerkedjen meg [keresések jelentkezzen](log-analytics-log-searches.md) tooview részletes megoldások által összegyűjtött adatokat.</span><span class="sxs-lookup"><span data-stu-id="a67a3-603">Get familiar with [log searches](log-analytics-log-searches.md) tooview detailed information gathered by solutions.</span></span>
* <span data-ttu-id="a67a3-604">Használjon [irányítópultok](log-analytics-dashboards.md) toosave és megjelenítheti a saját egyéni keres.</span><span class="sxs-lookup"><span data-stu-id="a67a3-604">Use [dashboards](log-analytics-dashboards.md) toosave and display your own custom searches.</span></span>
