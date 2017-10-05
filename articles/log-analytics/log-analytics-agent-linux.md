---
title: "A Linux rendszerű számítógépek csatlakozni az Operations Management Suite (OMS) |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan csatlakozzon az Azure, a más felhőalapú vagy helyszíni OMS-ügynök használatával Linux OMS üzemeltetett Linux rendszerű számítógépek."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte
ms.openlocfilehash: 1c05f68235aafd0fa098a3b0edaba1258df09380
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-linux-computers-to-operations-management-suite-oms"></a><span data-ttu-id="6bd30-103">A Linux rendszerű számítógépek csatlakozni az Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="6bd30-103">Connect your Linux Computers to Operations Management Suite (OMS)</span></span> 

<span data-ttu-id="6bd30-104">A Microsoft Operations Management Suite (OMS), összegyűjtheti, és Linux rendszerű számítógépek és a tároló-megoldásokkal, például a Docker, fizikai kiszolgálóként vagy virtuális gépek, virtuális gépek a helyszíni adatközpontban található adatok intézkedjen egy például az Amazon Web Services (AWS) vagy a Microsoft Azure-felhőben üzemeltetett szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6bd30-104">With Microsoft Operations Management Suite (OMS), you can collect and act on data generated from Linux computers and container solutions like Docker, residing in your on-premises data center as physical servers or virtual machines, virtual machines in a cloud-hosted service like Amazon Web Services (AWS) or Microsoft Azure.</span></span> <span data-ttu-id="6bd30-105">Is használhatja OMS elérhető megoldások például a változások követése, konfigurációs módosításokat, és a frissítéskezelés szoftverfrissítések segítségével proaktívan felügyelheti a Linux virtuális gépek az életciklus kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="6bd30-105">You can also use management solutions available in OMS such as Change Tracking, to identify configuration changes, and Update Management to manage software updates to proactively manage the lifecycle of your Linux VMs.</span></span> 

<span data-ttu-id="6bd30-106">Az OMS-ügynököt a Linux rendszerhez használt 443-as TCP-porton keresztül kommunikál az OMS szolgáltatással kimenő, és ha a számítógép csatlakozik egy tűzfal vagy a proxy-kiszolgálót kommunikálnak az interneten keresztül tekintse meg [használható az ügynök OMS vagy HTTP-proxy kiszolgáló konfigurálása Átjáró](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) milyen konfiguráció módosításait fogja tudni alkalmazni kell.</span><span class="sxs-lookup"><span data-stu-id="6bd30-106">The OMS Agent for Linux communicates outbound with the OMS service over TCP port 443, and if the computer connects to a firewall or proxy server to communicate over the Internet, review [Configuring the agent for use with an HTTP proxy server or OMS Gateway](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) to understand what configuration changes will need to be applied.</span></span>  <span data-ttu-id="6bd30-107">Ha a System Center 2016 - Operations Manager, illetve az Operations Manager 2012 R2-ben futtató számítógép lehet többhelyű az adatok gyűjtéséhez és a szolgáltatás továbbítja, és továbbra is az Operations Manager által figyelt az OMS szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="6bd30-107">If you are monitoring the computer with System Center 2016 - Operations Manager or Operations Manager 2012 R2, it can be multi-homed with the OMS service to collect data and forward to the service and still be monitored by Operations Manager.</span></span>  <span data-ttu-id="6bd30-108">Egy Operations Manager OMS szolgáltatással integrált felügyeleti csoportot által figyelt Linux rendszerű számítógépek nem kapják meg a konfigurációs adatforrások, és előre gyűjtött adatokat a felügyeleti csoporton keresztül.</span><span class="sxs-lookup"><span data-stu-id="6bd30-108">Linux computers monitored by an Operations Manager management group that is integrated with OMS do not receive configuration for data sources and forward collected data through the management group.</span></span>  <span data-ttu-id="6bd30-109">Az OMS-ügynököt nem konfigurálható a jelentés egynél több munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="6bd30-109">The OMS agent cannot be configured to report to more than one workspace.</span></span>  

<span data-ttu-id="6bd30-110">Ha az IT-biztonsági házirendeknek nem engedélyezi a számítógépek a hálózat csatlakozik az internethez, az ügynök beállítható úgy, hogy az OMS-átjárón, attól függően, hogy a megoldás engedélyezte az összegyűjtött adatok küldésére és fogadására a konfigurációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="6bd30-110">If your IT security policies do not allow computers on your network to connect to the Internet, the agent can be configured to connect to the OMS Gateway to receive configuration information and send collected data depending on the solution you have enabled.</span></span> <span data-ttu-id="6bd30-111">További információt és az OMS Linux-ügynök kommunikációja áthaladjon egy OMS-átjárót az OMS szolgáltatáshoz konfigurálásával kapcsolatos lépéseket, [számítógépeket csatlakoztatni az OMS Szolgáltatáshoz az OMS-átjáró](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="6bd30-111">For more information and steps on how to configure your OMS Linux Agent to communicate through an OMS Gateway to the OMS service, see [Connect computers to OMS using the OMS Gateway](log-analytics-oms-gateway.md).</span></span>  

<span data-ttu-id="6bd30-112">A következő ábra szemlélteti a Linux rendszerű számítógépek ügynök által felügyelt és az OMS-ben, beleértve a irányát és portok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="6bd30-112">The following diagram depicts the connection between the agent-managed Linux computers and OMS, including the direction and ports.</span></span>

![közvetlen ügynökkommunikációhoz OMS diagramja](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a><span data-ttu-id="6bd30-114">Rendszerkövetelmények</span><span class="sxs-lookup"><span data-stu-id="6bd30-114">System requirements</span></span>
<span data-ttu-id="6bd30-115">Megkezdése előtt tekintse át az alábbi részleteket megfelel az Előfeltételek ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="6bd30-115">Before starting, review the following details to verify you meet the prerequisites.</span></span>

### <a name="supported-linux-operating-systems"></a><span data-ttu-id="6bd30-116">Támogatott Linux operációs rendszerek</span><span class="sxs-lookup"><span data-stu-id="6bd30-116">Supported Linux operating systems</span></span>
<span data-ttu-id="6bd30-117">A következő Linux terjesztésekről hivatalosan támogatottak.</span><span class="sxs-lookup"><span data-stu-id="6bd30-117">The following Linux distributions are officially supported.</span></span>  <span data-ttu-id="6bd30-118">Azonban az OMS-ügynököt Linux előfordulhat, hogy is futtathatja más azokat a terjesztéseket nem szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="6bd30-118">However, the OMS Agent for Linux might also run on other distributions not listed.</span></span>

* <span data-ttu-id="6bd30-119">Amazon Linux 2012.09 való 2015.09 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="6bd30-119">Amazon Linux 2012.09 to 2015.09 (x86/x64)</span></span>
* <span data-ttu-id="6bd30-120">CentOS Linux 5, 6 és 7 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="6bd30-120">CentOS Linux 5, 6, and 7 (x86/x64)</span></span>
* <span data-ttu-id="6bd30-121">Oracle Linux 5, 6 és 7 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="6bd30-121">Oracle Linux 5, 6, and 7 (x86/x64)</span></span>
* <span data-ttu-id="6bd30-122">Red Hat Enterprise Linux Server 5, 6, 7 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="6bd30-122">Red Hat Enterprise Linux Server 5, 6 and 7 (x86/x64)</span></span>
* <span data-ttu-id="6bd30-123">Debian GNU/Linux 8 (x86/x64), 6, 7</span><span class="sxs-lookup"><span data-stu-id="6bd30-123">Debian GNU/Linux 6, 7, and 8 (x86/x64)</span></span>
* <span data-ttu-id="6bd30-124">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="6bd30-124">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)</span></span>
* <span data-ttu-id="6bd30-125">SUSE Linux Enterprise Server 11 és 12 (x86/x64)</span><span class="sxs-lookup"><span data-stu-id="6bd30-125">SUSE Linux Enterprise Server 11 and 12 (x86/x64)</span></span>

### <a name="network"></a><span data-ttu-id="6bd30-126">Network (Hálózat)</span><span class="sxs-lookup"><span data-stu-id="6bd30-126">Network</span></span>
<span data-ttu-id="6bd30-127">Az alábbi lista a proxy és tűzfal konfigurációs adatokat, a Linux-ügynök OMS folytatott kommunikációhoz szükséges információt.</span><span class="sxs-lookup"><span data-stu-id="6bd30-127">The information below list the proxy and firewall configuration information required for the Linux agent to communicate with OMS.</span></span> <span data-ttu-id="6bd30-128">Akkor kimenő forgalomról beszélünk a hálózatról az OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="6bd30-128">Traffic is outbound from your network to the OMS service.</span></span> 

|<span data-ttu-id="6bd30-129">Ügynök erőforrása</span><span class="sxs-lookup"><span data-stu-id="6bd30-129">Agent Resource</span></span>| <span data-ttu-id="6bd30-130">Portok</span><span class="sxs-lookup"><span data-stu-id="6bd30-130">Ports</span></span> |  
|------|---------|  
|<span data-ttu-id="6bd30-131">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="6bd30-131">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="6bd30-132">443-as port</span><span class="sxs-lookup"><span data-stu-id="6bd30-132">Port 443</span></span>|   
|<span data-ttu-id="6bd30-133">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="6bd30-133">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="6bd30-134">443-as port</span><span class="sxs-lookup"><span data-stu-id="6bd30-134">Port 443</span></span>|   
|<span data-ttu-id="6bd30-135">*.BLOB.Core.Windows.NET/</span><span class="sxs-lookup"><span data-stu-id="6bd30-135">*.blob.core.windows.net/</span></span> | <span data-ttu-id="6bd30-136">443-as port</span><span class="sxs-lookup"><span data-stu-id="6bd30-136">Port 443</span></span>|   
|<span data-ttu-id="6bd30-137">*.azure-automation.net</span><span class="sxs-lookup"><span data-stu-id="6bd30-137">*.azure-automation.net</span></span> | <span data-ttu-id="6bd30-138">443-as port</span><span class="sxs-lookup"><span data-stu-id="6bd30-138">Port 443</span></span>|  

### <a name="package-requirements"></a><span data-ttu-id="6bd30-139">Csomag követelmények</span><span class="sxs-lookup"><span data-stu-id="6bd30-139">Package requirements</span></span>

 <span data-ttu-id="6bd30-140">**Szükséges csomag**</span><span class="sxs-lookup"><span data-stu-id="6bd30-140">**Required package**</span></span>   | <span data-ttu-id="6bd30-141">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6bd30-141">**Description**</span></span>   | <span data-ttu-id="6bd30-142">**Minimális verzió**</span><span class="sxs-lookup"><span data-stu-id="6bd30-142">**Minimum version**</span></span>
--------------------- | --------------------- | -------------------
<span data-ttu-id="6bd30-143">Glibc</span><span class="sxs-lookup"><span data-stu-id="6bd30-143">Glibc</span></span> | <span data-ttu-id="6bd30-144">GNU C-függvénytár</span><span class="sxs-lookup"><span data-stu-id="6bd30-144">GNU C Library</span></span>   | <span data-ttu-id="6bd30-145">2.5-12</span><span class="sxs-lookup"><span data-stu-id="6bd30-145">2.5-12</span></span> 
<span data-ttu-id="6bd30-146">Openssl</span><span class="sxs-lookup"><span data-stu-id="6bd30-146">Openssl</span></span> | <span data-ttu-id="6bd30-147">OpenSSL-függvénytárak</span><span class="sxs-lookup"><span data-stu-id="6bd30-147">OpenSSL Libraries</span></span> | <span data-ttu-id="6bd30-148">0.9.8e vagy 1.0</span><span class="sxs-lookup"><span data-stu-id="6bd30-148">0.9.8e or 1.0</span></span>
<span data-ttu-id="6bd30-149">Curl</span><span class="sxs-lookup"><span data-stu-id="6bd30-149">Curl</span></span> | <span data-ttu-id="6bd30-150">cURL-webügyfél</span><span class="sxs-lookup"><span data-stu-id="6bd30-150">cURL web client</span></span> | <span data-ttu-id="6bd30-151">7.15.5</span><span class="sxs-lookup"><span data-stu-id="6bd30-151">7.15.5</span></span>
<span data-ttu-id="6bd30-152">Python-ctypes</span><span class="sxs-lookup"><span data-stu-id="6bd30-152">Python-ctypes</span></span> | | 
<span data-ttu-id="6bd30-153">A PAM</span><span class="sxs-lookup"><span data-stu-id="6bd30-153">PAM</span></span> | <span data-ttu-id="6bd30-154">Cserélhető hitelesítési modulok</span><span class="sxs-lookup"><span data-stu-id="6bd30-154">Pluggable authentication Modules</span></span> | 

> [!NOTE]
>  <span data-ttu-id="6bd30-155">Rsyslog vagy a syslog-ng kell gyűjteni a syslog-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="6bd30-155">Either rsyslog or syslog-ng are required to collect syslog messages.</span></span> <span data-ttu-id="6bd30-156">Syslog-események gyűjtése nem támogatott az alapértelmezett a syslog démon a Red Hat Enterprise Linux-, CentOS, és Oracle Linux verziója (sysklog) 5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="6bd30-156">The default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="6bd30-157">Syslog-adatokat gyűjteni a terjesztéseket ezen verziója, a rsyslog démon kell telepíthetők és konfigurálhatók lecseréli sysklog,</span><span class="sxs-lookup"><span data-stu-id="6bd30-157">To collect syslog data from this version of these distributions, the rsyslog daemon should be installed and configured to replace sysklog,</span></span> 

<span data-ttu-id="6bd30-158">Az ügynök több csomagot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6bd30-158">The agent includes multiple packages.</span></span> <span data-ttu-id="6bd30-159">A kiadási fájl tartalmazza a következő csomagok, futtassa a rendszerhéj-csomagot a rendelkezésre álló `--extract`:</span><span class="sxs-lookup"><span data-stu-id="6bd30-159">The release file contains the following packages, available by running the shell bundle with `--extract`:</span></span>

<span data-ttu-id="6bd30-160">**Csomag**</span><span class="sxs-lookup"><span data-stu-id="6bd30-160">**Package**</span></span> | <span data-ttu-id="6bd30-161">**Verzió**</span><span class="sxs-lookup"><span data-stu-id="6bd30-161">**Version**</span></span> | <span data-ttu-id="6bd30-162">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6bd30-162">**Description**</span></span>
----------- | ----------- | --------------
<span data-ttu-id="6bd30-163">omsagent</span><span class="sxs-lookup"><span data-stu-id="6bd30-163">omsagent</span></span> | <span data-ttu-id="6bd30-164">1.4.0</span><span class="sxs-lookup"><span data-stu-id="6bd30-164">1.4.0</span></span> | <span data-ttu-id="6bd30-165">Az Operations Management Suite-ügynök Linux</span><span class="sxs-lookup"><span data-stu-id="6bd30-165">The Operations Management Suite Agent for Linux</span></span>
<span data-ttu-id="6bd30-166">omsconfig</span><span class="sxs-lookup"><span data-stu-id="6bd30-166">omsconfig</span></span> | <span data-ttu-id="6bd30-167">1.1.1</span><span class="sxs-lookup"><span data-stu-id="6bd30-167">1.1.1</span></span> | <span data-ttu-id="6bd30-168">Az OMS-ügynök konfigurálása ügynök</span><span class="sxs-lookup"><span data-stu-id="6bd30-168">Configuration agent for the OMS Agent</span></span>
<span data-ttu-id="6bd30-169">OMI</span><span class="sxs-lookup"><span data-stu-id="6bd30-169">omi</span></span> | <span data-ttu-id="6bd30-170">1.2.0</span><span class="sxs-lookup"><span data-stu-id="6bd30-170">1.2.0</span></span> | <span data-ttu-id="6bd30-171">Nyissa meg a Management Infrastructure (OMI) – egy egyszerűsített CIM-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="6bd30-171">Open Management Infrastructure (OMI) - a lightweight CIM Server</span></span>
<span data-ttu-id="6bd30-172">az scx</span><span class="sxs-lookup"><span data-stu-id="6bd30-172">scx</span></span> | <span data-ttu-id="6bd30-173">1.6.3</span><span class="sxs-lookup"><span data-stu-id="6bd30-173">1.6.3</span></span> | <span data-ttu-id="6bd30-174">Operációs rendszer teljesítménymutatók OMI a CIM-szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="6bd30-174">OMI CIM Providers for operating system performance metrics</span></span>
<span data-ttu-id="6bd30-175">Apache-cimprov</span><span class="sxs-lookup"><span data-stu-id="6bd30-175">apache-cimprov</span></span> | <span data-ttu-id="6bd30-176">1.0.1</span><span class="sxs-lookup"><span data-stu-id="6bd30-176">1.0.1</span></span> | <span data-ttu-id="6bd30-177">Apache HTTP Server teljesítményfigyelés-szolgáltató az OMI.</span><span class="sxs-lookup"><span data-stu-id="6bd30-177">Apache HTTP Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="6bd30-178">Apache HTTP Server észlelésekor telepítve.</span><span class="sxs-lookup"><span data-stu-id="6bd30-178">Installed if Apache HTTP Server is detected.</span></span>
<span data-ttu-id="6bd30-179">MySQL-cimprov</span><span class="sxs-lookup"><span data-stu-id="6bd30-179">mysql-cimprov</span></span> | <span data-ttu-id="6bd30-180">1.0.1</span><span class="sxs-lookup"><span data-stu-id="6bd30-180">1.0.1</span></span> | <span data-ttu-id="6bd30-181">MySQL-kiszolgáló teljesítményfigyelés-szolgáltató az OMI.</span><span class="sxs-lookup"><span data-stu-id="6bd30-181">MySQL Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="6bd30-182">Ha a MySQL/MariaDB kiszolgáló észleli telepítve.</span><span class="sxs-lookup"><span data-stu-id="6bd30-182">Installed if MySQL/MariaDB server is detected.</span></span>
<span data-ttu-id="6bd30-183">docker-cimprov</span><span class="sxs-lookup"><span data-stu-id="6bd30-183">docker-cimprov</span></span> | <span data-ttu-id="6bd30-184">1.0.0</span><span class="sxs-lookup"><span data-stu-id="6bd30-184">1.0.0</span></span> | <span data-ttu-id="6bd30-185">Docker-szolgáltató az OMI.</span><span class="sxs-lookup"><span data-stu-id="6bd30-185">Docker provider for OMI.</span></span> <span data-ttu-id="6bd30-186">Ha a Docker észleli telepítve.</span><span class="sxs-lookup"><span data-stu-id="6bd30-186">Installed if Docker is detected.</span></span>

### <a name="compatibility-with-system-center-operations-manager"></a><span data-ttu-id="6bd30-187">A System Center Operations Manager kompatibilitása</span><span class="sxs-lookup"><span data-stu-id="6bd30-187">Compatibility with System Center Operations Manager</span></span>
<span data-ttu-id="6bd30-188">Az OMS-ügynököt a Linux ügynök bináris fájlokat osztanak meg a System Center Operations Manager-ügynök.</span><span class="sxs-lookup"><span data-stu-id="6bd30-188">The OMS Agent for Linux shares agent binaries with the System Center Operations Manager agent.</span></span> <span data-ttu-id="6bd30-189">Ha telepíti az OMS-ügynököt Linux rendszeren jelenleg az OMI és az SCX-csomagok egy újabb verzióra a számítógépen az Operations Manager által felügyelt.</span><span class="sxs-lookup"><span data-stu-id="6bd30-189">If you install the OMS Agent for Linux on a system currently managed by Operations Manager, it the OMI and SCX packages on the computer to a newer version.</span></span> <span data-ttu-id="6bd30-190">Ebben a kiadásban az OMS Szolgáltatáshoz és a System Center 2016 - Operations Manager, illetve az Operations Manager 2012 R2 ügynökök Linux kompatibilisek.</span><span class="sxs-lookup"><span data-stu-id="6bd30-190">In this release, the OMS and System Center 2016 - Operations Manager/Operations Manager 2012 R2 agents for Linux are compatible.</span></span> 

> [!NOTE]
> <span data-ttu-id="6bd30-191">A System Center 2012 SP1 és a korábbi verziók jelenleg nem kompatibilis vagy nem támogatott a Linux OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="6bd30-191">System Center 2012 SP1 and earlier versions are currently not compatible or supported with the OMS Agent for Linux.</span></span><br>
> <span data-ttu-id="6bd30-192">Ha olyan számítógépre, amelyen az Operations Manager által jelenleg nem figyeli a Linux az OMS-ügynök telepítve van és folyamatosan figyelje a számítógépet az Operations Manager majd kívánja, módosítania kell a [OMI konfigurációs](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) előtt felderítése a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6bd30-192">If the OMS Agent for Linux is installed to a computer that is not currently monitored by Operations Manager, and you then wish to monitor the computer with Operations Manager, you must modify the [OMI configuration](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) prior to discovering the computer.</span></span> <span data-ttu-id="6bd30-193">**Ez a lépés *nem* szükséges, ha az Operations Manager-ügynök telepítve van az OMS-ügynököt előtt Linux.**</span><span class="sxs-lookup"><span data-stu-id="6bd30-193">**This step is *not* needed if the Operations Manager agent is installed before the OMS Agent for Linux.**</span></span>

### <a name="system-configuration-changes"></a><span data-ttu-id="6bd30-194">Rendszer-konfigurációs módosítások</span><span class="sxs-lookup"><span data-stu-id="6bd30-194">System configuration changes</span></span>
<span data-ttu-id="6bd30-195">Az OMS-ügynököt a Linux-csomagok telepítése után a következő további rendszerszintű konfigurációs módosítások.</span><span class="sxs-lookup"><span data-stu-id="6bd30-195">After installing the OMS Agent for Linux packages, the following additional system-wide configuration changes are applied.</span></span> <span data-ttu-id="6bd30-196">Ezen összetevők a omsagent csomag eltávolításakor a rendszer törli.</span><span class="sxs-lookup"><span data-stu-id="6bd30-196">These artifacts are removed when the omsagent package is uninstalled.</span></span>

* <span data-ttu-id="6bd30-197">Egy jogosulatlan felhasználó nevű: `omsagent` jön létre.</span><span class="sxs-lookup"><span data-stu-id="6bd30-197">A non-privileged user named: `omsagent` is created.</span></span> <span data-ttu-id="6bd30-198">Ez az a fiók futtatja a omsagent démon fut.</span><span class="sxs-lookup"><span data-stu-id="6bd30-198">This is the account the omsagent daemon runs as.</span></span>
* <span data-ttu-id="6bd30-199">/Etc/sudoers.d/omsagent sudoers "tartalmaz" fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="6bd30-199">A sudoers “include” file is created at /etc/sudoers.d/omsagent.</span></span> <span data-ttu-id="6bd30-200">Ez engedélyezi a syslog és omsagent démonok újraindítására omsagent.</span><span class="sxs-lookup"><span data-stu-id="6bd30-200">This authorizes omsagent to restart the syslog and omsagent daemons.</span></span> <span data-ttu-id="6bd30-201">Ha a sudo "tartalmaz" direktívák nem támogatottak a sudo telepített verziója, ezek a bejegyzések /etc/sudoers kerülnek.</span><span class="sxs-lookup"><span data-stu-id="6bd30-201">If sudo “include” directives are not supported in the installed version of sudo, these entries are written to /etc/sudoers.</span></span>
* <span data-ttu-id="6bd30-202">A rendszernaplózás konfigurálásánál úgy módosul, hogy események csoportját, hogy az ügynök továbbítja.</span><span class="sxs-lookup"><span data-stu-id="6bd30-202">The syslog configuration is modified to forward a subset of events to the agent.</span></span> <span data-ttu-id="6bd30-203">További információkért lásd: a **adatok gyűjtésének konfigurálása** az alábbi szakasz</span><span class="sxs-lookup"><span data-stu-id="6bd30-203">For more information, see the **Configuring Data Collection** section below</span></span>

### <a name="upgrade-from-a-previous-release"></a><span data-ttu-id="6bd30-204">Frissítse korábbi kiadásáról</span><span class="sxs-lookup"><span data-stu-id="6bd30-204">Upgrade from a previous release</span></span>
<span data-ttu-id="6bd30-205">Ebben a kiadásban 1.0.0-47 támogatottnál korábbi verzióit frissíteni.</span><span class="sxs-lookup"><span data-stu-id="6bd30-205">Upgrade from versions earlier than 1.0.0-47 is supported in this release.</span></span> <span data-ttu-id="6bd30-206">A telepítés végrehajtása a `--upgrade` parancs összetevők az ügynök a legújabb verzióra frissíti.</span><span class="sxs-lookup"><span data-stu-id="6bd30-206">Performing the installation with the `--upgrade` command upgrades all components of the agent to the latest version.</span></span>

## <a name="installing-the-agent"></a><span data-ttu-id="6bd30-207">Az ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="6bd30-207">Installing the agent</span></span>

<span data-ttu-id="6bd30-208">Ez a szakasz ismerteti az OMS-ügynök telepítése Linux rendszert egy bunndle, az ügynök összetevői a Debian és RPM csomagokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="6bd30-208">This section describes how to install the OMS Agent for Linux using a bunndle, which contains Debian and RPM packages for each of the agent components.</span></span>  <span data-ttu-id="6bd30-209">Ez közvetlenül telepített vagy kibontva, az egyes csomagok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="6bd30-209">It can be installed directly or extracted to retrieve the individual packages.</span></span>  

<span data-ttu-id="6bd30-210">Először van szüksége az OMS-munkaterület azonosítója és a kulcs, amely átvált a [OMS klasszikus portál](https://mms.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="6bd30-210">First you need your OMS workspace ID and key, which you can find by switching to the [OMS classic portal](https://mms.microsoft.com).</span></span>  <span data-ttu-id="6bd30-211">A a **áttekintése** lap, a felső menüben válassza **beállítások**, és navigáljon arra **Sources\Linux kiszolgálók csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="6bd30-211">On the **Overview** page, from the top menu select **Settings**, and then navigate to **Connected Sources\Linux Servers**.</span></span>  <span data-ttu-id="6bd30-212">Jobb oldalán érték **munkaterület azonosítója** és **elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="6bd30-212">You see the value to the right of **Workspace ID** and **Primary Key**.</span></span>  <span data-ttu-id="6bd30-213">Másolással illessze be a kedvenc szerkesztő mindkettő.</span><span class="sxs-lookup"><span data-stu-id="6bd30-213">Copy and paste both into your favorite editor.</span></span>    

1. <span data-ttu-id="6bd30-214">Töltse le a legújabb [Linux (x64) OMS-ügynököt](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) vagy [Linux x86 OMS-ügynököt](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) a Githubról.</span><span class="sxs-lookup"><span data-stu-id="6bd30-214">Download the latest [OMS Agent for Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) or [OMS Agent for Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) from GitHub.</span></span>  
2. <span data-ttu-id="6bd30-215">Vigye át a megfelelő csomagot (x86 vagy x64) a Linux-számítógép scp/sftp használatával.</span><span class="sxs-lookup"><span data-stu-id="6bd30-215">Transfer the appropriate bundle (x86 or x64) to your Linux computer using scp/sftp.</span></span>
3. <span data-ttu-id="6bd30-216">A csomag telepítéséhez használja a `--install` vagy `--upgrade` argumentum.</span><span class="sxs-lookup"><span data-stu-id="6bd30-216">Install the bundle by using the `--install` or `--upgrade` argument.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="6bd30-217">Ha a meglévő csomagokat például ha már telepítve van a a System Center Operations Manager Linux-ügynök van telepítve, használja a `--upgrade` argumentum.</span><span class="sxs-lookup"><span data-stu-id="6bd30-217">If any existing packages are installed such as when the System Center Operations Manager agent for Linux is already installed, use the `--upgrade` argument.</span></span> <span data-ttu-id="6bd30-218">A telepítés során kapcsolódjon az Operations Management Suite, adja meg a `-w <WorkspaceID>` és `-s <Shared Key>` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="6bd30-218">To connect to Operations Management Suite during installation, provide the `-w <WorkspaceID>` and `-s <Shared Key>` parameters.</span></span>


#### <a name="to-install-and-onboard-directly"></a><span data-ttu-id="6bd30-219">A telepítendő és közvetlen előkészítéséről</span><span class="sxs-lookup"><span data-stu-id="6bd30-219">To install and onboard directly</span></span>
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="to-upgrade-the-agent-package"></a><span data-ttu-id="6bd30-220">Az ügynök-csomag frissítése</span><span class="sxs-lookup"><span data-stu-id="6bd30-220">To upgrade the agent package</span></span>
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="to-install-and-onboard-to-a-workspace-in-us-government-cloud"></a><span data-ttu-id="6bd30-221">A telepítendő és érheti el a felhőben US Government munkaterület</span><span class="sxs-lookup"><span data-stu-id="6bd30-221">To install and onboard to a workspace in US Government Cloud</span></span>
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway"></a><span data-ttu-id="6bd30-222">Az ügynök számára egy HTTP-proxy kiszolgáló vagy az OMS-átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6bd30-222">Configuring the agent for use with an HTTP proxy server or OMS Gateway</span></span>
<span data-ttu-id="6bd30-223">Az OMS-ügynököt Linux támogatja, vagy a HTTP vagy HTTPS-proxy kiszolgáló vagy az OMS szolgáltatáshoz OMS-átjárón keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="6bd30-223">The OMS Agent for Linux supports communicating either through an HTTP or HTTPS proxy server or OMS Gateway to the OMS service.</span></span>  <span data-ttu-id="6bd30-224">A névtelen és alapszintű hitelesítést (felhasználónév/jelszó) is támogatja.</span><span class="sxs-lookup"><span data-stu-id="6bd30-224">Both anonymous and basic authentication (username/password) is supported.</span></span>  

### <a name="proxy-configuration"></a><span data-ttu-id="6bd30-225">Proxykiszolgáló-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="6bd30-225">Proxy configuration</span></span>
<span data-ttu-id="6bd30-226">A proxy konfigurációs érték szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="6bd30-226">The proxy configuration value has the following syntax:</span></span>

`[protocol://][user:password@]proxyhost[:port]`

<span data-ttu-id="6bd30-227">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6bd30-227">Property</span></span>|<span data-ttu-id="6bd30-228">Leírás</span><span class="sxs-lookup"><span data-stu-id="6bd30-228">Description</span></span>
-|-
<span data-ttu-id="6bd30-229">Protokoll</span><span class="sxs-lookup"><span data-stu-id="6bd30-229">Protocol</span></span>|<span data-ttu-id="6bd30-230">http vagy https</span><span class="sxs-lookup"><span data-stu-id="6bd30-230">http or https</span></span>
<span data-ttu-id="6bd30-231">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="6bd30-231">user</span></span>|<span data-ttu-id="6bd30-232">Nem kötelező felhasználónév a proxyhitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="6bd30-232">Optional username for proxy authentication</span></span>
<span data-ttu-id="6bd30-233">jelszó</span><span class="sxs-lookup"><span data-stu-id="6bd30-233">password</span></span>|<span data-ttu-id="6bd30-234">Opcionális jelszót a proxyhitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="6bd30-234">Optional password for proxy authentication</span></span>
<span data-ttu-id="6bd30-235">proxyhost</span><span class="sxs-lookup"><span data-stu-id="6bd30-235">proxyhost</span></span>|<span data-ttu-id="6bd30-236">Cím vagy FQDN-jét a proxy server/OMS átjáró</span><span class="sxs-lookup"><span data-stu-id="6bd30-236">Address or FQDN of the proxy server/OMS Gateway</span></span>
<span data-ttu-id="6bd30-237">port</span><span class="sxs-lookup"><span data-stu-id="6bd30-237">port</span></span>|<span data-ttu-id="6bd30-238">A proxy server/OMS átjáró választható portszáma</span><span class="sxs-lookup"><span data-stu-id="6bd30-238">Optional port number for the proxy server/OMS Gateway</span></span>

<span data-ttu-id="6bd30-239">Például:`http://user01:password@proxy01.contoso.com:8080`</span><span class="sxs-lookup"><span data-stu-id="6bd30-239">For example: `http://user01:password@proxy01.contoso.com:8080`</span></span>

<span data-ttu-id="6bd30-240">A proxy kiszolgáló telepítése során vagy a telepítés után a proxy.conf konfigurációs fájl módosításával adható meg.</span><span class="sxs-lookup"><span data-stu-id="6bd30-240">The proxy server can be specified during installation or by modifying the proxy.conf configuration file after installation.</span></span>   

### <a name="specify-proxy-configuration-during-installation"></a><span data-ttu-id="6bd30-241">Adja meg a proxy konfigurációjának telepítése során</span><span class="sxs-lookup"><span data-stu-id="6bd30-241">Specify proxy configuration during installation</span></span>
<span data-ttu-id="6bd30-242">A `-p` vagy `--proxy` argumentuma a omsagent telepítési csomagot, adja meg a proxykiszolgáló-konfigurációt szeretne használni.</span><span class="sxs-lookup"><span data-stu-id="6bd30-242">The `-p` or `--proxy` argument for the omsagent installation bundle specifies the proxy configuration to use.</span></span> 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p http://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-the-proxy-configuration-in-a-file"></a><span data-ttu-id="6bd30-243">Adja meg a proxy konfigurációs fájlba</span><span class="sxs-lookup"><span data-stu-id="6bd30-243">Define the proxy configuration in a file</span></span>
<span data-ttu-id="6bd30-244">A proxykonfiguráció állítható be a fájlok `/etc/opt/microsoft/omsagent/proxy.conf` és `/etc/opt/microsoft/omsagent/conf/proxy.conf `.</span><span class="sxs-lookup"><span data-stu-id="6bd30-244">The proxy configuration can be set in the files `/etc/opt/microsoft/omsagent/proxy.conf`  and `/etc/opt/microsoft/omsagent/conf/proxy.conf `.</span></span> <span data-ttu-id="6bd30-245">A fájlok is kell közvetlenül létrehozott vagy módosított, de a rájuk vonatkozó engedélyek meg kell adni a omiuser felhasználó olvasási jogosultsággal a fájlokra.</span><span class="sxs-lookup"><span data-stu-id="6bd30-245">The files can be directly created or edited, but their permissions must be updated to grant the omiuser user read permission on the files.</span></span> <span data-ttu-id="6bd30-246">Példa:</span><span class="sxs-lookup"><span data-stu-id="6bd30-246">For example:</span></span>
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:8080"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-the-proxy-configuration"></a><span data-ttu-id="6bd30-247">A proxykiszolgáló-konfiguráció eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6bd30-247">Removing the proxy configuration</span></span>
<span data-ttu-id="6bd30-248">Távolítsa el a korábban meghatározott proxy konfigurációját, és a közvetlen kapcsolat visszaállításához, távolítsa el a proxy.conf fájlt:</span><span class="sxs-lookup"><span data-stu-id="6bd30-248">To remove a previously defined proxy configuration and revert to direct connectivity, remove the proxy.conf file:</span></span>
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-operations-management-suite"></a><span data-ttu-id="6bd30-249">Az Operations Management Suite előkészítési</span><span class="sxs-lookup"><span data-stu-id="6bd30-249">Onboarding with Operations Management Suite</span></span>
<span data-ttu-id="6bd30-250">A munkaterület azonosítója és kulcsa nem lett megadva a csomag telepítése során, ha az ügynök később regisztrálni kell az Operations Management Suite szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="6bd30-250">If a workspace ID and key were not provided during the bundle installation, the agent must be subsequently registered with Operations Management Suite.</span></span>

### <a name="onboarding-using-the-command-line"></a><span data-ttu-id="6bd30-251">Bevezetés a parancssor használatával</span><span class="sxs-lookup"><span data-stu-id="6bd30-251">Onboarding using the command line</span></span>
<span data-ttu-id="6bd30-252">Futtassa a omsadmin.sh parancsot, hogy megadja a munkaterület azonosítója és a munkaterület kulcsát.</span><span class="sxs-lookup"><span data-stu-id="6bd30-252">Run the omsadmin.sh command supplying the workspace id and key for your workspace.</span></span> <span data-ttu-id="6bd30-253">Ez a parancs (rendelkező sudo jogosultsági szint emeléséhez) rendszergazdaként kell futtatnia:</span><span class="sxs-lookup"><span data-stu-id="6bd30-253">This command must be run as root (with sudo elevation):</span></span>
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a><span data-ttu-id="6bd30-254">A bevezetési fájl használatával</span><span class="sxs-lookup"><span data-stu-id="6bd30-254">Onboarding using a file</span></span>
1.  <span data-ttu-id="6bd30-255">A fájl létrehozása `/etc/omsagent-onboard.conf`.</span><span class="sxs-lookup"><span data-stu-id="6bd30-255">Create the file `/etc/omsagent-onboard.conf`.</span></span> <span data-ttu-id="6bd30-256">A fájl olvasható és írható az alapvető kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6bd30-256">The file must be readable and writable for root.</span></span>
`sudo vi /etc/omsagent-onboard.conf`
2.  <span data-ttu-id="6bd30-257">Helyezze be a fájlt a munkaterület azonosítója és a megosztott kulcs a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="6bd30-257">Insert the following lines in the file with your Workspace ID and Shared Key:</span></span>

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  <span data-ttu-id="6bd30-258">A következő parancsot a Onboard az OMS-be:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`</span><span class="sxs-lookup"><span data-stu-id="6bd30-258">Run the following command to Onboard to OMS: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh`</span></span>
4.  <span data-ttu-id="6bd30-259">A fájl törlődik a sikeres bevezetése.</span><span class="sxs-lookup"><span data-stu-id="6bd30-259">The file is deleted on successful onboarding.</span></span>

## <a name="enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager"></a><span data-ttu-id="6bd30-260">A Linux számára, hogy a System Center Operations Manager OMS-ügynök engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6bd30-260">Enable the OMS Agent for Linux to report to System Center Operations Manager</span></span>
<span data-ttu-id="6bd30-261">Hajtsa végre a következő lépésekkel állíthatja be az OMS-ügynököt, hogy egy System Center Operations Manager felügyeleti csoport linuxos.</span><span class="sxs-lookup"><span data-stu-id="6bd30-261">Perform the following steps to configure the OMS Agent for Linux to report to a System Center Operations Manager management group.</span></span>  

1. <span data-ttu-id="6bd30-262">A fájl szerkesztése`/etc/opt/omi/conf/omiserver.conf`</span><span class="sxs-lookup"><span data-stu-id="6bd30-262">Edit the file `/etc/opt/omi/conf/omiserver.conf`</span></span>
2. <span data-ttu-id="6bd30-263">Győződjön meg arról, hogy a kezdődő **httpsport =** határozza meg az 1270-es port.</span><span class="sxs-lookup"><span data-stu-id="6bd30-263">Ensure that the line beginning with **httpsport=** defines the port 1270.</span></span> <span data-ttu-id="6bd30-264">Például:`httpsport=1270`</span><span class="sxs-lookup"><span data-stu-id="6bd30-264">Such as: `httpsport=1270`</span></span>
3. <span data-ttu-id="6bd30-265">Indítsa újra az OMI-kiszolgálón:`sudo /opt/omi/bin/service_control restart`</span><span class="sxs-lookup"><span data-stu-id="6bd30-265">Restart the OMI server: `sudo /opt/omi/bin/service_control restart`</span></span>

## <a name="agent-logs"></a><span data-ttu-id="6bd30-266">Ügynök naplók</span><span class="sxs-lookup"><span data-stu-id="6bd30-266">Agent logs</span></span>
<span data-ttu-id="6bd30-267">Linux OMS-ügynököt a naplókban találhatók: `/var/opt/microsoft/omsagent/<workspace id>/log/` a naplókat a omsconfig (ügynök konfigurációjának) program helyen találhatók: `/var/opt/microsoft/omsconfig/log/` (amely teljesítményadatokat metrikák) OMI és az SCX-összetevők naplóinak helyen találhatók:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`</span><span class="sxs-lookup"><span data-stu-id="6bd30-267">The logs for the OMS Agent for Linux can be found at: `/var/opt/microsoft/omsagent/<workspace id>/log/` The logs for the omsconfig (agent configuration) program can be found at: `/var/opt/microsoft/omsconfig/log/` Logs for the OMI and SCX components (which provide performance metrics data) can be found at: `/var/opt/omi/log/ and /var/opt/microsoft/scx/log`</span></span>

### <a name="log-rotation-configuration"></a><span data-ttu-id="6bd30-268">Napló Elforgatás konfigurációs ##</span><span class="sxs-lookup"><span data-stu-id="6bd30-268">Log rotation configuration##</span></span>
<span data-ttu-id="6bd30-269">A napló omsagent Elforgatás konfigurációs helyen találhatók:`/etc/logrotate.d/omsagent-<workspace id>`</span><span class="sxs-lookup"><span data-stu-id="6bd30-269">The log rotate configuration for omsagent can be found at: `/etc/logrotate.d/omsagent-<workspace id>`</span></span>

<span data-ttu-id="6bd30-270">Az alapértelmezett beállítások a következők:</span><span class="sxs-lookup"><span data-stu-id="6bd30-270">The default settings are:</span></span> 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-the-oms-agent-for-linux"></a><span data-ttu-id="6bd30-271">A Linux OMS-ügynök eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6bd30-271">Uninstalling the OMS Agent for Linux</span></span>
<span data-ttu-id="6bd30-272">A ügynökcsomagokat el lehet távolítani a csomagot .sh fájl futtatásával az `--purge` argumentum, amely az ügynök és a konfiguráció teljesen eltávolítja a számítógépről.</span><span class="sxs-lookup"><span data-stu-id="6bd30-272">The agent packages can be uninstalled by running the bundle .sh file with the `--purge` argument, which completely removes the agent and its configuration from the computer.</span></span>   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a><span data-ttu-id="6bd30-273">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="6bd30-273">Troubleshooting</span></span>

### <a name="issue-unable-to-connect-through-proxy-to-oms"></a><span data-ttu-id="6bd30-274">Probléma: Nem lehet csatlakozni az OMS-proxy használatával</span><span class="sxs-lookup"><span data-stu-id="6bd30-274">Issue: Unable to connect through proxy to OMS</span></span>

#### <a name="probable-causes"></a><span data-ttu-id="6bd30-275">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="6bd30-275">Probable causes</span></span>
* <span data-ttu-id="6bd30-276">A bevezetési során megadott proxy helytelen volt.</span><span class="sxs-lookup"><span data-stu-id="6bd30-276">The proxy specified during onboarding was incorrect</span></span>
* <span data-ttu-id="6bd30-277">Az OMS végpontok nincsenek az adatközpontban található whitelistested</span><span class="sxs-lookup"><span data-stu-id="6bd30-277">The OMS Service Endpoints are not whitelistested in your datacenter</span></span> 

#### <a name="resolutions"></a><span data-ttu-id="6bd30-278">Megoldások</span><span class="sxs-lookup"><span data-stu-id="6bd30-278">Resolutions</span></span>
1. <span data-ttu-id="6bd30-279">Az OMS szolgáltatáshoz az OMS-ügynököt a Linux-a kapcsolóval használja a következő parancsot a Reonboard `-v` engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="6bd30-279">Reonboard to the OMS Service with the OMS Agent for Linux by using the following command with the option `-v` enabled.</span></span> <span data-ttu-id="6bd30-280">Ez lehetővé teszi, hogy az ügynök az OMS szolgáltatáshoz a proxyn keresztül történő kapcsolódás részletes kimenet.</span><span class="sxs-lookup"><span data-stu-id="6bd30-280">This allows verbose output of the agent connecting through the proxy to the OMS Service.</span></span> 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. <span data-ttu-id="6bd30-281">Tekintse át a szakasz [használható az ügynök konfigurálását a HTTP-proxy server(#configuring the-agent-for-use-with-a-http-proxy-server) megfelelően konfigurált proxykiszolgálón keresztül kommunikálnak az ügynököt ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="6bd30-281">Review the section [Configuring the agent for use with an HTTP proxy server(#configuring the-agent-for-use-with-a-http-proxy-server) to verify you have properly configured the agent to communicate through a proxy server.</span></span>    
* <span data-ttu-id="6bd30-282">Kettős ellenőrizze, hogy az OMS Szolgáltatásvégpontok szerepel az engedélyezési listán:</span><span class="sxs-lookup"><span data-stu-id="6bd30-282">Double check that the following OMS Service endpoints are whitelisted:</span></span>

    |<span data-ttu-id="6bd30-283">Ügynök erőforrása</span><span class="sxs-lookup"><span data-stu-id="6bd30-283">Agent Resource</span></span>| <span data-ttu-id="6bd30-284">Portok</span><span class="sxs-lookup"><span data-stu-id="6bd30-284">Ports</span></span> |  
    |------|---------|  
    |<span data-ttu-id="6bd30-285">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="6bd30-285">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="6bd30-286">443-as port</span><span class="sxs-lookup"><span data-stu-id="6bd30-286">Port 443</span></span>|   
    |<span data-ttu-id="6bd30-287">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="6bd30-287">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="6bd30-288">443-as port</span><span class="sxs-lookup"><span data-stu-id="6bd30-288">Port 443</span></span>|   
    |<span data-ttu-id="6bd30-289">ods.systemcenteradvisor.com</span><span class="sxs-lookup"><span data-stu-id="6bd30-289">ods.systemcenteradvisor.com</span></span> | <span data-ttu-id="6bd30-290">443-as port</span><span class="sxs-lookup"><span data-stu-id="6bd30-290">Port 443</span></span>|   
    |<span data-ttu-id="6bd30-291">*.BLOB.Core.Windows.NET/</span><span class="sxs-lookup"><span data-stu-id="6bd30-291">*.blob.core.windows.net/</span></span> | <span data-ttu-id="6bd30-292">443-as port</span><span class="sxs-lookup"><span data-stu-id="6bd30-292">Port 443</span></span>|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a><span data-ttu-id="6bd30-293">Probléma: 403-as hibaüzenetet kap közben előkészítésére</span><span class="sxs-lookup"><span data-stu-id="6bd30-293">Issue: You receive a 403 error when trying to onboard</span></span>

#### <a name="probable-causes"></a><span data-ttu-id="6bd30-294">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="6bd30-294">Probable causes</span></span>
* <span data-ttu-id="6bd30-295">Dátum és idő nem megfelelő Linux-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="6bd30-295">Date and Time is incorrect on Linux Server</span></span> 
* <span data-ttu-id="6bd30-296">Munkaterületének Azonosítóját és kulcsát használt nem megfelelőek</span><span class="sxs-lookup"><span data-stu-id="6bd30-296">Workspace ID and Workspace Key used are not correct</span></span>

#### <a name="resolution"></a><span data-ttu-id="6bd30-297">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="6bd30-297">Resolution</span></span>

1. <span data-ttu-id="6bd30-298">Ellenőrizze az időt a parancs dátuma a Linux-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="6bd30-298">Check the time on your Linux server with the command date.</span></span> <span data-ttu-id="6bd30-299">Ha a idő 15 perc, az aktuális időponthoz képest +/-, akkor bevezetési sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="6bd30-299">If the time is +/- 15 minutes from current time, then onboarding fails.</span></span> <span data-ttu-id="6bd30-300">Megfelelő ez frissítse a dátumot és/vagy a Linux-kiszolgáló időzónáját.</span><span class="sxs-lookup"><span data-stu-id="6bd30-300">To correct this update the date and/or timezone of your Linux server.</span></span> 
2. <span data-ttu-id="6bd30-301">Ellenőrizze, hogy a legújabb verzióját az OMS-ügynököt telepítette Linux.</span><span class="sxs-lookup"><span data-stu-id="6bd30-301">Verify you have installed the latest version of the OMS Agent for Linux.</span></span>  <span data-ttu-id="6bd30-302">A legújabb verziója most értesíti, ha időeltérési a bevezetési hibát okozó.</span><span class="sxs-lookup"><span data-stu-id="6bd30-302">The newest version now notifies you if time skew is causing the onboarding failure.</span></span>
3. <span data-ttu-id="6bd30-303">A Reonboard használja a megfelelő munkaterület Azonosítóját és kulcsát a következő témakör korábbi szakaszában a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="6bd30-303">Reonboard using correct Workspace ID and Workspace Key following the installation instructions earlier in this topic.</span></span>

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a><span data-ttu-id="6bd30-304">Probléma: Egy 500 és 404-es hiba látható a naplófájlban bevezetése után</span><span class="sxs-lookup"><span data-stu-id="6bd30-304">Issue: You see a 500 and 404 error in the log file right after onboarding</span></span>
<span data-ttu-id="6bd30-305">Ez az egy ismert probléma, amely az OMS-munkaterület Linux adatok első feltöltéskor.</span><span class="sxs-lookup"><span data-stu-id="6bd30-305">This is a known issue that occurs on first upload of Linux data into an OMS workspace.</span></span> <span data-ttu-id="6bd30-306">Ez nem befolyásolja az elküldött és a szolgáltatás élmény adatok.</span><span class="sxs-lookup"><span data-stu-id="6bd30-306">This does not affect data being sent or service experience.</span></span>

### <a name="issue--you-are-not-seeing-any-data-in-the-oms-portal"></a><span data-ttu-id="6bd30-307">Probléma: Nem Ön az OMS-portálon megadott adattal sem</span><span class="sxs-lookup"><span data-stu-id="6bd30-307">Issue:  You are not seeing any data in the OMS portal</span></span>

#### <a name="probable-causes"></a><span data-ttu-id="6bd30-308">Lehetséges okok</span><span class="sxs-lookup"><span data-stu-id="6bd30-308">Probable causes</span></span>

- <span data-ttu-id="6bd30-309">Bevezetés az OMS szolgáltatáshoz sikertelen volt</span><span class="sxs-lookup"><span data-stu-id="6bd30-309">Onboarding to the OMS Service failed</span></span>
- <span data-ttu-id="6bd30-310">Az OMS szolgáltatáshoz kapcsolat le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="6bd30-310">Connection to the OMS Service is blocked</span></span>
- <span data-ttu-id="6bd30-311">OMS-ügynököt a Linux-adatok biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="6bd30-311">OMS Agent for Linux data is backed up</span></span>

#### <a name="resolutions"></a><span data-ttu-id="6bd30-312">Megoldások</span><span class="sxs-lookup"><span data-stu-id="6bd30-312">Resolutions</span></span>
1. <span data-ttu-id="6bd30-313">Ellenőrizze, hogy bevezetése az OMS szolgáltatáshoz a következő fájl létezésének ellenőrzésével sikeres volt:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`</span><span class="sxs-lookup"><span data-stu-id="6bd30-313">Check if onboarding the OMS Service was successful by checking if the following file exists: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`</span></span>
2. <span data-ttu-id="6bd30-314">Reonboard használatával a `omsadmin.sh` parancssori utasításokat</span><span class="sxs-lookup"><span data-stu-id="6bd30-314">Reonboard using the `omsadmin.sh` command-line instructions</span></span>
3. <span data-ttu-id="6bd30-315">Ha proxyt használ, tekintse meg a korábban megadott proxy megoldási lépések.</span><span class="sxs-lookup"><span data-stu-id="6bd30-315">If using a proxy, refer to the proxy resolution steps provided earlier.</span></span>
4. <span data-ttu-id="6bd30-316">Egyes esetekben amikor Linux az OMS-ügynök nem tud kommunikálni az OMS szolgáltatással adatai az ügynökön várólistára van állítva 50 MB teljes puffer mérete.</span><span class="sxs-lookup"><span data-stu-id="6bd30-316">In some cases, when the OMS Agent for Linux cannot communicate with the OMS Service, data on the agent is queued to the full buffer size, which is 50 MB.</span></span> <span data-ttu-id="6bd30-317">A Linux OMS-ügynököt újra kell indítani a következő parancs futtatásával: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`.</span><span class="sxs-lookup"><span data-stu-id="6bd30-317">The OMS Agent for Linux should be restarted by running the following command: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`.</span></span> 

    >[!NOTE]
    ><span data-ttu-id="6bd30-318">Ez a probléma fennáll ügynök verziója 1.1.0-28 és újabb verziók.</span><span class="sxs-lookup"><span data-stu-id="6bd30-318">This issue is fixed in agent version 1.1.0-28 and later.</span></span>
> 