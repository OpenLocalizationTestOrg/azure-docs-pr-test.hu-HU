---
title: "Figyelés megoldás az Azure Naplóelemzés aaaContainer |} Microsoft Docs"
description: "hello tároló figyelésére szolgáló megoldás a Log Analyticshez segít megtekintése és kezelése a Docker és a Windows tároló állomások egyetlen helyen megvalósítható."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte;banders
ms.openlocfilehash: 2eed1dd81c22faef78a375fca3ebece9e5300c09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="container-monitoring-solution-in-log-analytics"></a><span data-ttu-id="fe100-103">A Naplóelemzési tároló figyelés megoldás</span><span class="sxs-lookup"><span data-stu-id="fe100-103">Container Monitoring solution in Log Analytics</span></span>

![Tárolók szimbólum](./media/log-analytics-containers/containers-symbol.png)

<span data-ttu-id="fe100-105">Ez a cikk ismerteti, hogyan tooset fel és -felhasználási hello tároló figyelésére szolgáló megoldás Naplóelemzési, amely segít a Docker és a Windows megtekintéséhez és kezeléséhez a tároló állomások egyetlen helyen megvalósítható.</span><span class="sxs-lookup"><span data-stu-id="fe100-105">This article describes how tooset up and use hello Container Monitoring solution in Log Analytics, which helps you view and manage your Docker and Windows container hosts in a single location.</span></span> <span data-ttu-id="fe100-106">Docker a virtualizációs szoftver rendszer használt toocreate tárolók, amely automatizálja a szoftverfrissítési központi telepítési tootheir informatikai infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="fe100-106">Docker is a software virtualization system used toocreate containers that automate software deployment tootheir IT infrastructure.</span></span>

<span data-ttu-id="fe100-107">hello megoldást mutat be tárolók futtat, amely futtatja, tároló képet, és a tárolók futtató.</span><span class="sxs-lookup"><span data-stu-id="fe100-107">hello solution shows which containers are running, what container image they’re running, and where containers are running.</span></span> <span data-ttu-id="fe100-108">Tárolók használt parancsok bemutató részletes naplózási információk is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="fe100-108">You can view detailed audit information showing commands used with containers.</span></span> <span data-ttu-id="fe100-109">És tárolók megtekintésével és központosított naplók keresése anélkül, hogy a Docker tooremotely nézet vagy a Windows gazdagépek is elháríthatók.</span><span class="sxs-lookup"><span data-stu-id="fe100-109">And, you can troubleshoot containers by viewing and searching centralized logs without having tooremotely view Docker or Windows hosts.</span></span> <span data-ttu-id="fe100-110">Tárolók, amelyek esetleg zajos vagy a felhasználó felesleges erőforrások egy gazdagépen található.</span><span class="sxs-lookup"><span data-stu-id="fe100-110">You can find containers that may be noisy and consuming excess resources on a host.</span></span> <span data-ttu-id="fe100-111">És megtekintheti a központi Processzor, memória, tárolási és hálózati használati és teljesítményadatokat adatai tárolók.</span><span class="sxs-lookup"><span data-stu-id="fe100-111">And, you can view centralized CPU, memory, storage, and network usage and performance information for containers.</span></span> <span data-ttu-id="fe100-112">Windows rendszerű számítógépeken központosítása és hasonlítsa össze a napló, a Windows Server Hyper-V és a Docker-tárolók.</span><span class="sxs-lookup"><span data-stu-id="fe100-112">On computers running Windows, you can centralize and compare logs from Windows Server, Hyper-V, and Docker containers.</span></span> <span data-ttu-id="fe100-113">hello megoldás támogatja a következő tároló orchestrators hello:</span><span class="sxs-lookup"><span data-stu-id="fe100-113">hello solution supports hello following container orchestrators:</span></span>

- <span data-ttu-id="fe100-114">Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="fe100-114">Docker Swarm</span></span>
- <span data-ttu-id="fe100-115">DC/OS</span><span class="sxs-lookup"><span data-stu-id="fe100-115">DC/OS</span></span>
- <span data-ttu-id="fe100-116">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="fe100-116">Kubernetes</span></span>
- <span data-ttu-id="fe100-117">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="fe100-117">Service Fabric</span></span>
- <span data-ttu-id="fe100-118">Red Hat OpenShift</span><span class="sxs-lookup"><span data-stu-id="fe100-118">Red Hat OpenShift</span></span>


<span data-ttu-id="fe100-119">hello alábbi ábrán látható hello kapcsolatai különböző tároló gazdagépek és az ügynökök az OMS Szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="fe100-119">hello following diagram shows hello relationships between various container hosts and agents with OMS.</span></span>

![Tárolók diagramja](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a><span data-ttu-id="fe100-121">Rendszerkövetelmények</span><span class="sxs-lookup"><span data-stu-id="fe100-121">System requirements</span></span>

<span data-ttu-id="fe100-122">Megkezdése előtt tekintse át a következő részleteket tooverify megfelel a hello Előfeltételek hello.</span><span class="sxs-lookup"><span data-stu-id="fe100-122">Before starting, review hello following details tooverify you meet hello prerequisites.</span></span>

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a><span data-ttu-id="fe100-123">Figyelési megoldást igényelnek tároló Docker Orchestrator és az operációs rendszer platform támogatása</span><span class="sxs-lookup"><span data-stu-id="fe100-123">Container monitoring solution support for Docker Orchestrator and OS platform</span></span>
<span data-ttu-id="fe100-124">hello következő tábla vázol fel hello Docker vezénylési és az operációs rendszer figyelési tároló készlet, a teljesítmény és a naplók és a Naplóelemzési támogatást.</span><span class="sxs-lookup"><span data-stu-id="fe100-124">hello following table outlines hello Docker orchestration and operating system monitoring support of container inventory, performance, and logs with Log Analytics.</span></span>   

| | <span data-ttu-id="fe100-125">ACS</span><span class="sxs-lookup"><span data-stu-id="fe100-125">ACS</span></span> | <span data-ttu-id="fe100-126">Linux</span><span class="sxs-lookup"><span data-stu-id="fe100-126">Linux</span></span> | <span data-ttu-id="fe100-127">Windows</span><span class="sxs-lookup"><span data-stu-id="fe100-127">Windows</span></span> | <span data-ttu-id="fe100-128">Tároló</span><span class="sxs-lookup"><span data-stu-id="fe100-128">Container</span></span><br><span data-ttu-id="fe100-129">Szoftverleltár</span><span class="sxs-lookup"><span data-stu-id="fe100-129">Inventory</span></span> | <span data-ttu-id="fe100-130">Kép</span><span class="sxs-lookup"><span data-stu-id="fe100-130">Image</span></span><br><span data-ttu-id="fe100-131">Szoftverleltár</span><span class="sxs-lookup"><span data-stu-id="fe100-131">Inventory</span></span> | <span data-ttu-id="fe100-132">Csomópont</span><span class="sxs-lookup"><span data-stu-id="fe100-132">Node</span></span><br><span data-ttu-id="fe100-133">Szoftverleltár</span><span class="sxs-lookup"><span data-stu-id="fe100-133">Inventory</span></span> | <span data-ttu-id="fe100-134">Tároló</span><span class="sxs-lookup"><span data-stu-id="fe100-134">Container</span></span><br><span data-ttu-id="fe100-135">Teljesítmény</span><span class="sxs-lookup"><span data-stu-id="fe100-135">Performance</span></span> | <span data-ttu-id="fe100-136">Tároló</span><span class="sxs-lookup"><span data-stu-id="fe100-136">Container</span></span><br><span data-ttu-id="fe100-137">Esemény</span><span class="sxs-lookup"><span data-stu-id="fe100-137">Event</span></span> | <span data-ttu-id="fe100-138">Esemény</span><span class="sxs-lookup"><span data-stu-id="fe100-138">Event</span></span><br><span data-ttu-id="fe100-139">Napló</span><span class="sxs-lookup"><span data-stu-id="fe100-139">Log</span></span> | <span data-ttu-id="fe100-140">Tároló</span><span class="sxs-lookup"><span data-stu-id="fe100-140">Container</span></span><br><span data-ttu-id="fe100-141">Napló</span><span class="sxs-lookup"><span data-stu-id="fe100-141">Log</span></span> |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| <span data-ttu-id="fe100-142">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="fe100-142">Kubernetes</span></span> | <span data-ttu-id="fe100-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-143">&#8226;</span></span> | <span data-ttu-id="fe100-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-144">&#8226;</span></span> | | <span data-ttu-id="fe100-145">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-145">&#8226;</span></span> | <span data-ttu-id="fe100-146">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-146">&#8226;</span></span> | <span data-ttu-id="fe100-147">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-147">&#8226;</span></span> | <span data-ttu-id="fe100-148">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-148">&#8226;</span></span> | <span data-ttu-id="fe100-149">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-149">&#8226;</span></span> | <span data-ttu-id="fe100-150">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-150">&#8226;</span></span> | <span data-ttu-id="fe100-151">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-151">&#8226;</span></span> |
| <span data-ttu-id="fe100-152">Mesosphere</span><span class="sxs-lookup"><span data-stu-id="fe100-152">Mesosphere</span></span><br><span data-ttu-id="fe100-153">DC/OS</span><span class="sxs-lookup"><span data-stu-id="fe100-153">DC/OS</span></span> | <span data-ttu-id="fe100-154">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-154">&#8226;</span></span> | <span data-ttu-id="fe100-155">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-155">&#8226;</span></span> | | <span data-ttu-id="fe100-156">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-156">&#8226;</span></span> | <span data-ttu-id="fe100-157">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-157">&#8226;</span></span> | <span data-ttu-id="fe100-158">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-158">&#8226;</span></span> | <span data-ttu-id="fe100-159">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-159">&#8226;</span></span>| <span data-ttu-id="fe100-160">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-160">&#8226;</span></span> | <span data-ttu-id="fe100-161">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-161">&#8226;</span></span> | <span data-ttu-id="fe100-162">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-162">&#8226;</span></span> |
| <span data-ttu-id="fe100-163">Docker</span><span class="sxs-lookup"><span data-stu-id="fe100-163">Docker</span></span><br><span data-ttu-id="fe100-164">Swarm</span><span class="sxs-lookup"><span data-stu-id="fe100-164">Swarm</span></span> | <span data-ttu-id="fe100-165">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-165">&#8226;</span></span> | <span data-ttu-id="fe100-166">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-166">&#8226;</span></span> | <span data-ttu-id="fe100-167">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-167">&#8226;</span></span> | <span data-ttu-id="fe100-168">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-168">&#8226;</span></span> | <span data-ttu-id="fe100-169">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-169">&#8226;</span></span> | <span data-ttu-id="fe100-170">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-170">&#8226;</span></span> | <span data-ttu-id="fe100-171">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-171">&#8226;</span></span> | <span data-ttu-id="fe100-172">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-172">&#8226;</span></span> | | <span data-ttu-id="fe100-173">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-173">&#8226;</span></span> |
| <span data-ttu-id="fe100-174">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="fe100-174">Service</span></span><br><span data-ttu-id="fe100-175">Háló</span><span class="sxs-lookup"><span data-stu-id="fe100-175">Fabric</span></span> | | | <span data-ttu-id="fe100-176">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-176">&#8226;</span></span> | <span data-ttu-id="fe100-177">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-177">&#8226;</span></span> | <span data-ttu-id="fe100-178">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-178">&#8226;</span></span> | <span data-ttu-id="fe100-179">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-179">&#8226;</span></span> | <span data-ttu-id="fe100-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-180">&#8226;</span></span> | <span data-ttu-id="fe100-181">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-181">&#8226;</span></span> | <span data-ttu-id="fe100-182">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-182">&#8226;</span></span> | <span data-ttu-id="fe100-183">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-183">&#8226;</span></span> |
| <span data-ttu-id="fe100-184">Red Hat megnyitása</span><span class="sxs-lookup"><span data-stu-id="fe100-184">Red Hat Open</span></span><br><span data-ttu-id="fe100-185">SHIFT</span><span class="sxs-lookup"><span data-stu-id="fe100-185">Shift</span></span> | | <span data-ttu-id="fe100-186">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-186">&#8226;</span></span> | | <span data-ttu-id="fe100-187">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-187">&#8226;</span></span> | <span data-ttu-id="fe100-188">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-188">&#8226;</span></span>| <span data-ttu-id="fe100-189">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-189">&#8226;</span></span> | <span data-ttu-id="fe100-190">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-190">&#8226;</span></span> | <span data-ttu-id="fe100-191">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-191">&#8226;</span></span> | | <span data-ttu-id="fe100-192">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-192">&#8226;</span></span> |
| <span data-ttu-id="fe100-193">Windows Server</span><span class="sxs-lookup"><span data-stu-id="fe100-193">Windows Server</span></span><br><span data-ttu-id="fe100-194">(önálló)</span><span class="sxs-lookup"><span data-stu-id="fe100-194">(standalone)</span></span> | | | <span data-ttu-id="fe100-195">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-195">&#8226;</span></span> | <span data-ttu-id="fe100-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-196">&#8226;</span></span> | <span data-ttu-id="fe100-197">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-197">&#8226;</span></span> | <span data-ttu-id="fe100-198">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-198">&#8226;</span></span> | <span data-ttu-id="fe100-199">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-199">&#8226;</span></span> | <span data-ttu-id="fe100-200">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-200">&#8226;</span></span> | | <span data-ttu-id="fe100-201">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-201">&#8226;</span></span> |
| <span data-ttu-id="fe100-202">Linux-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="fe100-202">Linux Server</span></span><br><span data-ttu-id="fe100-203">(önálló)</span><span class="sxs-lookup"><span data-stu-id="fe100-203">(standalone)</span></span> | | <span data-ttu-id="fe100-204">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-204">&#8226;</span></span> | | <span data-ttu-id="fe100-205">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-205">&#8226;</span></span> | <span data-ttu-id="fe100-206">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-206">&#8226;</span></span> | <span data-ttu-id="fe100-207">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-207">&#8226;</span></span> | <span data-ttu-id="fe100-208">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-208">&#8226;</span></span> | <span data-ttu-id="fe100-209">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-209">&#8226;</span></span> | | <span data-ttu-id="fe100-210">&#8226;</span><span class="sxs-lookup"><span data-stu-id="fe100-210">&#8226;</span></span> |


### <a name="docker-versions-supported-on-linux"></a><span data-ttu-id="fe100-211">Támogatott Linux docker-verziók</span><span class="sxs-lookup"><span data-stu-id="fe100-211">Docker versions supported on Linux</span></span>

- <span data-ttu-id="fe100-212">Docker 1.11 too1.13</span><span class="sxs-lookup"><span data-stu-id="fe100-212">Docker 1.11 too1.13</span></span>
- <span data-ttu-id="fe100-213">Docker CE és EE v17.06</span><span class="sxs-lookup"><span data-stu-id="fe100-213">Docker CE and EE v17.06</span></span>

### <a name="x64-linux-distributions-supported-as-container-hosts"></a><span data-ttu-id="fe100-214">x64 tároló gazdagépként támogatott Linux-disztribúciók</span><span class="sxs-lookup"><span data-stu-id="fe100-214">x64 Linux distributions supported as container hosts</span></span>

- <span data-ttu-id="fe100-215">Ubuntu 14.04 LTS és 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="fe100-215">Ubuntu 14.04 LTS and 16.04 LTS</span></span>
- <span data-ttu-id="fe100-216">CoreOS(stable)</span><span class="sxs-lookup"><span data-stu-id="fe100-216">CoreOS(stable)</span></span>
- <span data-ttu-id="fe100-217">Amazon Linux 2016.09.0</span><span class="sxs-lookup"><span data-stu-id="fe100-217">Amazon Linux 2016.09.0</span></span>
- <span data-ttu-id="fe100-218">openSUSE, 13.2</span><span class="sxs-lookup"><span data-stu-id="fe100-218">openSUSE 13.2</span></span>
- <span data-ttu-id="fe100-219">openSUSE LEAP 42.2</span><span class="sxs-lookup"><span data-stu-id="fe100-219">openSUSE LEAP 42.2</span></span>
- <span data-ttu-id="fe100-220">7.2 és 7.3 centOS</span><span class="sxs-lookup"><span data-stu-id="fe100-220">CentOS 7.2 and 7.3</span></span>
- <span data-ttu-id="fe100-221">SLES 12</span><span class="sxs-lookup"><span data-stu-id="fe100-221">SLES 12</span></span>
- <span data-ttu-id="fe100-222">RHEL 7.2 és 7.3.</span><span class="sxs-lookup"><span data-stu-id="fe100-222">RHEL 7.2 and 7.3</span></span>
- <span data-ttu-id="fe100-223">Red Hat OpenShift tároló Platform (OCP) 3.4-es és 3.5-ös verzióját</span><span class="sxs-lookup"><span data-stu-id="fe100-223">Red Hat OpenShift Container Platform (OCP) 3.4 and 3.5</span></span>
- <span data-ttu-id="fe100-224">ACS Mesosphere DC/OS 1.7.3 too1.8.8</span><span class="sxs-lookup"><span data-stu-id="fe100-224">ACS Mesosphere DC/OS 1.7.3 too1.8.8</span></span>
- <span data-ttu-id="fe100-225">ACS Kubernetes 1.4.5 too1.6</span><span class="sxs-lookup"><span data-stu-id="fe100-225">ACS Kubernetes 1.4.5 too1.6</span></span>
- <span data-ttu-id="fe100-226">ACS Docker swarm használata</span><span class="sxs-lookup"><span data-stu-id="fe100-226">ACS Docker Swarm</span></span>

### <a name="supported-windows-operating-system"></a><span data-ttu-id="fe100-227">Támogatott Windows operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="fe100-227">Supported Windows operating system</span></span>

- <span data-ttu-id="fe100-228">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="fe100-228">Windows Server 2016</span></span>
- <span data-ttu-id="fe100-229">Windows 10 évforduló Edition (Professional vagy vállalati)</span><span class="sxs-lookup"><span data-stu-id="fe100-229">Windows 10 Anniversary Edition (Professional or Enterprise)</span></span>

### <a name="docker-versions-supported-on-windows"></a><span data-ttu-id="fe100-230">A Windows támogatott docker-verziók</span><span class="sxs-lookup"><span data-stu-id="fe100-230">Docker versions supported on Windows</span></span>

- <span data-ttu-id="fe100-231">Docker 1.12 és 1,13.</span><span class="sxs-lookup"><span data-stu-id="fe100-231">Docker 1.12 and 1.13</span></span>
- <span data-ttu-id="fe100-232">Docker 17.03.0 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="fe100-232">Docker 17.03.0 and later</span></span>

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="fe100-233">Telepítése és konfigurálása hello megoldás</span><span class="sxs-lookup"><span data-stu-id="fe100-233">Installing and configuring hello solution</span></span>
<span data-ttu-id="fe100-234">A következő információk tooinstall hello használja, és hello megoldás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="fe100-234">Use hello following information tooinstall and configure hello solution.</span></span>

1. <span data-ttu-id="fe100-235">Adja hozzá a hello tároló figyelési megoldást tooyour OMS-munkaterület a [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldások hello megoldások gyűjteménye a](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="fe100-235">Add hello Container Monitoring solution tooyour OMS workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>

2. <span data-ttu-id="fe100-236">Telepítheti és használhatja a Docker az OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="fe100-236">Install and use Docker with an OMS agent.</span></span>  <span data-ttu-id="fe100-237">Az operációs rendszer alapján, választhat a következő módszerek hello:</span><span class="sxs-lookup"><span data-stu-id="fe100-237">Based on your operating system, you can choose from hello following methods:</span></span>

  * <span data-ttu-id="fe100-238">Telepítse a támogatott Linux operációs rendszerek, és futtassa a Docker majd és telepíteni hello konfigurálása [Linux OMS-ügynököt](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="fe100-238">On supported Linux operating systems, install and run Docker and then install and configure hello [OMS Agent for Linux](log-analytics-agent-linux.md).</span></span>  
  * <span data-ttu-id="fe100-239">A CoreOS Linux hello OMS-ügynököt nem futtatható.</span><span class="sxs-lookup"><span data-stu-id="fe100-239">On CoreOS, you cannot run hello OMS Agent for Linux.</span></span> <span data-ttu-id="fe100-240">Ehelyett futtassa a hello OMS-ügynököt indexelése verziója Linux.</span><span class="sxs-lookup"><span data-stu-id="fe100-240">Instead, you run a containerized version of hello OMS Agent for Linux.</span></span> <span data-ttu-id="fe100-241">Felülvizsgálati [Linux tároló gazdagépek, köztük a CoreOS](#for-all-linux-container-hosts-including-coreos) vagy [Azure Government Linux tároló gazdagépek, köztük a CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) Ha Azure Government felhőalapú tárolók dolgozik.</span><span class="sxs-lookup"><span data-stu-id="fe100-241">Review [Linux container hosts including CoreOS](#for-all-linux-container-hosts-including-coreos) or [Azure Government Linux container hosts including CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) if you are working with containers in Azure Government Cloud.</span></span>
  * <span data-ttu-id="fe100-242">A Windows Server 2016 és a Windows 10 Docker-motorhoz hello és az ügyfél telepítése, majd csatlakozzon egy ügynök toogather adatait, és elküldi a tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="fe100-242">On Windows Server 2016 and Windows 10, install hello Docker Engine and client then connect an agent toogather information and send it tooLog Analytics.</span></span>  

### <a name="container-services"></a><span data-ttu-id="fe100-243">Tároló szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="fe100-243">Container services</span></span>

- <span data-ttu-id="fe100-244">Ha a Red Hat OpenShift környezettel rendelkezik, tekintse át a [OMS-ügynököt a Red Hat OpenShift konfigurálása](#configure-an-oms-agent-for-red-hat-openshift).</span><span class="sxs-lookup"><span data-stu-id="fe100-244">If you have a Red Hat OpenShift environment, review [Configure an OMS agent for Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).</span></span>
- <span data-ttu-id="fe100-245">Ha egy Kubernetes hello Azure Tárolószolgáltatási fürtöt, tekintse át a [figyelése az Azure Tárolószolgáltatás-fürtöt a Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span><span class="sxs-lookup"><span data-stu-id="fe100-245">If you have a Kubernetes cluster using hello Azure Container Service, review [Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span></span>
- <span data-ttu-id="fe100-246">Ha egy Azure tároló szolgáltatás DC/OS-fürtről, további tudnivalókért olvassa el [figyelése az Azure tároló szolgáltatás DC/OS fürtben, az Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span><span class="sxs-lookup"><span data-stu-id="fe100-246">If you have an Azure Container Service DC/OS cluster, learn more at [Monitor an Azure Container Service DC/OS cluster with Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span></span>
- <span data-ttu-id="fe100-247">Ha a Docker Swarm mód környezettel rendelkezik, további tudnivalókért olvassa el [OMS-ügynököt a Docker Swarmra konfigurálása](#configure-an-oms-agent-for-docker-swarm).</span><span class="sxs-lookup"><span data-stu-id="fe100-247">If you have a Docker Swarm mode environment, learn more at [Configure an OMS agent for Docker Swarm](#configure-an-oms-agent-for-docker-swarm).</span></span>
- <span data-ttu-id="fe100-248">A Service Fabric tárolók használja, ha további tudnivalókért olvassa el [áttekintés az Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fe100-248">If you use containers with Service Fabric, learn more at [Overview of Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span></span>
- <span data-ttu-id="fe100-249">Felülvizsgálati hello [Docker-motorhoz Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) további információt a cikk tooinstall és a Docker motorok konfigurálása a Windows rendszert futtató számítógépeket.</span><span class="sxs-lookup"><span data-stu-id="fe100-249">Review hello [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) article for additional information about how tooinstall and configure your Docker Engines on computers running Windows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe100-250">Futnia kell az docker **előtt** hello telepítése [Linux OMS-ügynököt](log-analytics-agent-linux.md) tároló gazdagépei.</span><span class="sxs-lookup"><span data-stu-id="fe100-250">Docker must be running **before** you install hello [OMS Agent for Linux](log-analytics-agent-linux.md) on your container hosts.</span></span> <span data-ttu-id="fe100-251">Ha már telepített hello ügynök Docker telepítése előtt, Linux kell tooreinstall hello OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="fe100-251">If you've already installed hello agent before installing Docker, you need tooreinstall hello OMS Agent for Linux.</span></span> <span data-ttu-id="fe100-252">Docker kapcsolatos további információkért lásd: hello [Docker webhely](https://www.docker.com).</span><span class="sxs-lookup"><span data-stu-id="fe100-252">For more information about Docker, see hello [Docker website](https://www.docker.com).</span></span>


## <a name="linux-container-hosts"></a><span data-ttu-id="fe100-253">Linux-tároló állomások</span><span class="sxs-lookup"><span data-stu-id="fe100-253">Linux container hosts</span></span>

<span data-ttu-id="fe100-254">Ha már telepítette a Docker, használja a hello beállításait a tároló tooconfigure hello állomásügynök Docker való használathoz a következő.</span><span class="sxs-lookup"><span data-stu-id="fe100-254">After you've installed Docker, use hello following settings for your container host tooconfigure hello agent for use with Docker.</span></span> <span data-ttu-id="fe100-255">Először szükség van a OMS-munkaterület azonosítója és a kulcs, amely hello Azure-portálon találja.</span><span class="sxs-lookup"><span data-stu-id="fe100-255">First you need your OMS workspace ID and key, which you can find in hello Azure portal.</span></span> <span data-ttu-id="fe100-256">A munkaterületen kattintson **gyors üzembe helyezés** > **számítógépek** tooview a **munkaterület azonosítója** és **elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="fe100-256">In your workspace, click **Quick Start** > **Computers** tooview your **Workspace ID** and **Primary Key**.</span></span>  <span data-ttu-id="fe100-257">Másolással illessze be a kedvenc szerkesztő mindkettő.</span><span class="sxs-lookup"><span data-stu-id="fe100-257">Copy and paste both into your favorite editor.</span></span>

### <a name="for-all-linux-container-hosts-except-coreos"></a><span data-ttu-id="fe100-258">CoreOS kivételével az összes Linux tároló állomás</span><span class="sxs-lookup"><span data-stu-id="fe100-258">For all Linux container hosts except CoreOS</span></span>

- <span data-ttu-id="fe100-259">További információk és bemutatjuk, hogyan tooinstall hello OMS-ügynököt Linux [csatlakozzon a Linux rendszerű számítógépek tooOperations felügyeleti csomaggal (OMS)](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="fe100-259">For more information and steps on how tooinstall hello OMS Agent for Linux, see [Connect your Linux Computers tooOperations Management Suite (OMS)](log-analytics-agent-linux.md).</span></span>

### <a name="for-all-linux-container-hosts-including-coreos"></a><span data-ttu-id="fe100-260">Az összes Linux tároló gazdagépek, köztük a CoreOS</span><span class="sxs-lookup"><span data-stu-id="fe100-260">For all Linux container hosts including CoreOS</span></span>

<span data-ttu-id="fe100-261">Indítsa el a megjeleníteni kívánt toomonitor hello OMS-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="fe100-261">Start hello OMS container that you want toomonitor.</span></span> <span data-ttu-id="fe100-262">Módosítsa és használja fel a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="fe100-262">Modify and use hello following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a><span data-ttu-id="fe100-263">Minden Azure Government Linux tároló gazdagépek, köztük a CoreOS</span><span class="sxs-lookup"><span data-stu-id="fe100-263">For all Azure Government Linux container hosts including CoreOS</span></span>

<span data-ttu-id="fe100-264">Indítsa el a megjeleníteni kívánt toomonitor hello OMS-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="fe100-264">Start hello OMS container that you want toomonitor.</span></span> <span data-ttu-id="fe100-265">Módosítsa és használja fel a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="fe100-265">Modify and use hello following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-tooone-in-a-container"></a><span data-ttu-id="fe100-266">Váltás a egy telepített Linux ügynök tooone tároló használata</span><span class="sxs-lookup"><span data-stu-id="fe100-266">Switching from using an installed Linux agent tooone in a container</span></span>
<span data-ttu-id="fe100-267">Ha korábban már használt hello közvetlenül telepített ügynök, és tooinstead használja a tárolóban futó ügynök, előbb hello OMS-ügynököt az Linux eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="fe100-267">If you previously used hello directly-installed agent and want tooinstead use an agent running in a container, you must first remove hello OMS Agent for Linux.</span></span> <span data-ttu-id="fe100-268">Lásd: [Linux OMS-ügynök eltávolítása hello](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) hogyan toosuccessfully eltávolítása toounderstand hello ügynök.</span><span class="sxs-lookup"><span data-stu-id="fe100-268">See [Uninstalling hello OMS Agent for Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) toounderstand how toosuccessfully uninstall hello agent.</span></span>  

### <a name="configure-an-oms-agent-for-docker-swarm"></a><span data-ttu-id="fe100-269">A Docker Swarmra OMS-ügynököt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe100-269">Configure an OMS agent for Docker Swarm</span></span>

<span data-ttu-id="fe100-270">A Docker Swarmról hello OMS-ügynököt is futhat, a globális szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fe100-270">You can run hello OMS Agent as a global service on Docker Swarm.</span></span> <span data-ttu-id="fe100-271">A következő információk toocreate OMS-ügynököt szolgáltatásnak hello használata.</span><span class="sxs-lookup"><span data-stu-id="fe100-271">Use hello following information toocreate an OMS Agent service.</span></span> <span data-ttu-id="fe100-272">Tooinsert az OMS-munkaterület azonosítója és elsődleges kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe100-272">You need tooinsert your OMS Workspace ID and Primary Key.</span></span>

- <span data-ttu-id="fe100-273">Futtassa a hello következő hello fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="fe100-273">Run hello following on hello master node.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a><span data-ttu-id="fe100-274">Red Hat OpenShift az OMS-ügynököt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe100-274">Configure an OMS Agent for Red Hat OpenShift</span></span>
<span data-ttu-id="fe100-275">Nincsenek három módon tooadd hello OMS-ügynököt tooRed Hat OpenShift toostart gyűjtését tároló figyelési adatokat.</span><span class="sxs-lookup"><span data-stu-id="fe100-275">There are three ways tooadd hello OMS Agent tooRed Hat OpenShift toostart collecting container monitoring data.</span></span>

* <span data-ttu-id="fe100-276">[Hello OMS-ügynök telepítése Linux](log-analytics-agent-linux.md) közvetlenül a OpenShift csomópontokon</span><span class="sxs-lookup"><span data-stu-id="fe100-276">[Install hello OMS Agent for Linux](log-analytics-agent-linux.md) directly on each OpenShift node</span></span>  
* <span data-ttu-id="fe100-277">[Napló Analytics Virtuálisgép-bővítmény engedélyezése](log-analytics-azure-vm-extension.md) minden OpenShift csomóponton található az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="fe100-277">[Enable Log Analytics VM Extension](log-analytics-azure-vm-extension.md) on each OpenShift node residing in Azure</span></span>  
* <span data-ttu-id="fe100-278">Készletként OpenShift démon-hello OMS-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="fe100-278">Install hello OMS Agent as a OpenShift daemon-set</span></span>  

<span data-ttu-id="fe100-279">Ebben a szakaszban egy OpenShift démon beállított azt foglalkozik hello lépéseket szükséges tooinstall hello OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="fe100-279">In this section we cover hello steps required tooinstall hello OMS Agent as an OpenShift daemon-set.</span></span>  

1. <span data-ttu-id="fe100-280">Jelentkezzen be toohello OpenShift csomópont, és másolja hello yam mesterfájl [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) a Githubból tooyour fő csomópont, és módosítsa az OMS-munkaterület azonosítója és az elsődleges kulcs hello értékét.</span><span class="sxs-lookup"><span data-stu-id="fe100-280">Sign on toohello OpenShift master node and copy hello yaml file [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) from GitHub tooyour master node and modify hello value with your OMS Workspace ID and with your Primary Key.</span></span>
2. <span data-ttu-id="fe100-281">Futtassa a következő parancsok toocreate hello a projektet az OMS Szolgáltatáshoz, és állítsa be a hello felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="fe100-281">Run hello following commands toocreate a project for OMS and set hello user account.</span></span>

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="fe100-282">toodeploy hello démon-készlet, futtassa a hello következő:</span><span class="sxs-lookup"><span data-stu-id="fe100-282">toodeploy hello daemon-set, run hello following:</span></span>

    `oc create -f ocp-omsagent.yaml`

5. <span data-ttu-id="fe100-283">tooverify van konfigurálva, és megfelelően működik, írja be a hello következő:</span><span class="sxs-lookup"><span data-stu-id="fe100-283">tooverify it is configured and working correctly, type hello following:</span></span>

    `oc describe daemonset omsagent`  

    <span data-ttu-id="fe100-284">és hello kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="fe100-284">and hello output should resemble:</span></span>

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

<span data-ttu-id="fe100-285">Ha azt szeretné toouse titkok toosecure az OMS-munkaterület azonosítója és az elsődleges kulcs hello OMS-ügynököt démon-set yam fájl használata esetén, hajtsa végre a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="fe100-285">If you want toouse secrets toosecure your OMS Workspace ID and Primary Key when using hello OMS Agent daemon-set yaml file, perform hello following steps.</span></span>

1. <span data-ttu-id="fe100-286">Jelentkezzen be toohello OpenShift csomópont, és másolja hello yam mesterfájl [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) és a titkos kulcs parancsfájljának [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) a Githubról.</span><span class="sxs-lookup"><span data-stu-id="fe100-286">Sign on toohello OpenShift master node and copy hello yaml file [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) and secret generating script [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) from GitHub.</span></span>  <span data-ttu-id="fe100-287">Ez a parancsfájl hello titkok yam fájl OMS-munkaterület azonosítója és az elsődleges kulcs toosecure hoz létre a secrete információkat.</span><span class="sxs-lookup"><span data-stu-id="fe100-287">This script will generate hello secrets yaml file for OMS Workspace ID and Primary Key toosecure your secrete information.</span></span>  
2. <span data-ttu-id="fe100-288">Futtassa a következő parancsok toocreate hello a projektet az OMS Szolgáltatáshoz, és állítsa be a hello felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="fe100-288">Run hello following commands toocreate a project for OMS and set hello user account.</span></span> <span data-ttu-id="fe100-289">hello titkos parancsfájljának kér az OMS-munkaterület azonosítója <WSID> és az elsődleges kulcs <KEY> és befejezésekor hello ocp-secret.yaml fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fe100-289">hello secret generating script asks for your OMS Workspace ID <WSID> and Primary Key <KEY> and upon completion, it creates hello ocp-secret.yaml file.</span></span>  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="fe100-290">Hello titkos fájl telepítése hello következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fe100-290">Deploy hello secret file by running hello following:</span></span>

    `oc create -f ocp-secret.yaml`

5. <span data-ttu-id="fe100-291">Telepítés ellenőrzése hello következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fe100-291">Verify deployment by running hello following:</span></span>

    `oc describe secret omsagent-secret`  

    <span data-ttu-id="fe100-292">és hello kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="fe100-292">and hello  output should resemble:</span></span>  

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

6. <span data-ttu-id="fe100-293">Hello OMS-ügynököt démon-set yam fájl telepítése hello következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fe100-293">Deploy hello OMS Agent daemon-set yaml file by running hello following:</span></span>

    `oc create -f ocp-ds-omsagent.yaml`  

7. <span data-ttu-id="fe100-294">Telepítés ellenőrzése hello következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fe100-294">Verify deployment by running hello following:</span></span>

    `oc describe ds oms`

    <span data-ttu-id="fe100-295">és hello kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="fe100-295">and hello output should resemble:</span></span>

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  

    Type:   Opaque  

     Data  
     ====  
     KEY:    89 bytes  
     WSID:   37 bytes  
    ```

### <a name="secure-your-secret-information-for-docker-swarm-and-kubernetes"></a><span data-ttu-id="fe100-296">A Docker Swarm és Kubernetes titkos adatok védelméhez</span><span class="sxs-lookup"><span data-stu-id="fe100-296">Secure your secret information for Docker Swarm and Kubernetes</span></span>

<span data-ttu-id="fe100-297">A titkos OMS-munkaterület azonosítója és az elsődleges kulcsok Docker Swarm-és tárolószolgáltatások Kubernetes biztonságát.</span><span class="sxs-lookup"><span data-stu-id="fe100-297">You can secure your secret OMS Workspace ID and Primary Keys for Docker Swarm and Kubernetes container services.</span></span>

#### <a name="secure-secrets-for-docker-swarm"></a><span data-ttu-id="fe100-298">Biztonságos kulcsok Docker Swarm esetén</span><span class="sxs-lookup"><span data-stu-id="fe100-298">Secure secrets for Docker Swarm</span></span>

<span data-ttu-id="fe100-299">A Docker Swarmra, hello titkot a munkaterület azonosítója és az elsődleges kulcs létrehozása után futtathatja hello OMSagent hello Docker szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fe100-299">For Docker Swarm, once hello secret for Workspace ID and Primary Key is created, you can run hello create hello Docker service for OMSagent.</span></span> <span data-ttu-id="fe100-300">A következő információk toocreate hello a titkos adatokat használják.</span><span class="sxs-lookup"><span data-stu-id="fe100-300">Use hello following information toocreate your secret information.</span></span>

1. <span data-ttu-id="fe100-301">Futtassa a hello következő hello fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="fe100-301">Run hello following on hello master node.</span></span>

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. <span data-ttu-id="fe100-302">Győződjön meg arról, hogy a titkos kulcsok megfelelően hozta létre.</span><span class="sxs-lookup"><span data-stu-id="fe100-302">Verify that secrets were created properly.</span></span>

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. <span data-ttu-id="fe100-303">Futtassa a következő parancs toomount hello titkok toohello hello indexelése OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="fe100-303">Run hello following command toomount hello secrets toohello containerized OMS Agent.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a><span data-ttu-id="fe100-304">Biztonságos titkot Kubernetes yam-fájlokkal</span><span class="sxs-lookup"><span data-stu-id="fe100-304">Secure secrets for Kubernetes with yaml files</span></span>

<span data-ttu-id="fe100-305">A Kubernetes, a parancsfájl toogenerate hello titkok yam fájlt használja a munkaterület azonosítója és az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="fe100-305">For Kubernetes, you use a script toogenerate hello secrets yaml file for your Workspace ID and Primary Key.</span></span> <span data-ttu-id="fe100-306">A hello [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) lapon, a fájl vagy a titkos adatait anélkül is használhatja.</span><span class="sxs-lookup"><span data-stu-id="fe100-306">At hello [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) page, there are files that you can use with or without your secret information.</span></span>

- <span data-ttu-id="fe100-307">hello OMS-ügynök DaemonSet alapértelmezett nem rendelkezik titkos információk (omsagent.yaml)</span><span class="sxs-lookup"><span data-stu-id="fe100-307">hello Default OMS Agent DaemonSet does not have secret information (omsagent.yaml)</span></span>
- <span data-ttu-id="fe100-308">hello OMS-ügynök DaemonSet yam fájl titkos generációs parancsfájlok toogenerate hello titkok yam (omsagentsecret.yaml) fájl titkos információk (omsagent – ds-secrets.yaml) használ.</span><span class="sxs-lookup"><span data-stu-id="fe100-308">hello OMS Agent DaemonSet yaml file uses secret information (omsagent-ds-secrets.yaml) with secret generation scripts toogenerate hello secrets yaml (omsagentsecret.yaml) file.</span></span>

<span data-ttu-id="fe100-309">Kiválaszthatja a toocreate omsagent DaemonSets vagy titkos kulcsok nélkül.</span><span class="sxs-lookup"><span data-stu-id="fe100-309">You can choose toocreate omsagent DaemonSets with or without secrets.</span></span>

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a><span data-ttu-id="fe100-310">Alapértelmezett OMSagent DaemonSet yam fájl titkos kulcsok nélkül</span><span class="sxs-lookup"><span data-stu-id="fe100-310">Default OMSagent DaemonSet yaml file without secrets</span></span>

- <span data-ttu-id="fe100-311">Hello alapértelmezett OMS-ügynök DaemonSet yam-fájl, cserélje le a hello `<WSID>` és `<KEY>` tooyour WSID és a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="fe100-311">For hello default OMS Agent DaemonSet yaml file, replace hello `<WSID>` and `<KEY>` tooyour WSID and KEY.</span></span> <span data-ttu-id="fe100-312">Másolja a hello fájl tooyour főcsomópont és futtatási hello következő:</span><span class="sxs-lookup"><span data-stu-id="fe100-312">Copy hello file tooyour master node and run hello following:</span></span>

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a><span data-ttu-id="fe100-313">Alapértelmezett OMSagent DaemonSet yam fájl titkos kulcsok</span><span class="sxs-lookup"><span data-stu-id="fe100-313">Default OMSagent DaemonSet yaml file with secrets</span></span>

1. <span data-ttu-id="fe100-314">toouse OMS-ügynök DaemonSet titkos információk használatával hozzon létre hello titkok először.</span><span class="sxs-lookup"><span data-stu-id="fe100-314">toouse OMS Agent DaemonSet using secret information, create hello secrets first.</span></span>
    1. <span data-ttu-id="fe100-315">Hello parancsfájl és titkos sablonfájl másolja, és győződjön meg arról, hogy a hello ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="fe100-315">Copy hello script and secret template file and make sure they are on hello same directory.</span></span>
        - <span data-ttu-id="fe100-316">Titkos kulcs parancsprogram - secret-gen.sh létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe100-316">Secret generating script - secret-gen.sh</span></span>
        - <span data-ttu-id="fe100-317">titkos sablon - secret-template.yaml</span><span class="sxs-lookup"><span data-stu-id="fe100-317">secret template - secret-template.yaml</span></span>
    2. <span data-ttu-id="fe100-318">Hello parancsfájl, például a következő példa hello futtatása.</span><span class="sxs-lookup"><span data-stu-id="fe100-318">Run hello script, like hello following example.</span></span> <span data-ttu-id="fe100-319">hello parancsprogram kéri a hello OMS-munkaterület azonosítója és az elsődleges kulcs, és beírt őket, hello parancsfájl létrehoz egy titkos yam ezért is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="fe100-319">hello script asks for hello OMS Workspace ID and Primary Key and after you enter them, hello script creates a secret yaml file so you can run it.</span></span>   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. <span data-ttu-id="fe100-320">Hozzon létre hello titkok pod hello következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fe100-320">Create hello secrets pod by running hello following:</span></span>
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. <span data-ttu-id="fe100-321">Futtassa a következő hello tooverify:</span><span class="sxs-lookup"><span data-stu-id="fe100-321">tooverify, run hello following:</span></span>

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        <span data-ttu-id="fe100-322">Kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="fe100-322">Output should resemble:</span></span>

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        <span data-ttu-id="fe100-323">Kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="fe100-323">Output should resemble:</span></span>

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. <span data-ttu-id="fe100-324">A omsagent futó démon-készlet létrehozása``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="fe100-324">Create your omsagent daemon-set by running ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

2. <span data-ttu-id="fe100-325">Győződjön meg arról, hogy a következő hasonló toohello OMS-ügynök DaemonSet fut, hello:</span><span class="sxs-lookup"><span data-stu-id="fe100-325">Verify that hello OMS Agent DaemonSet is running, similar toohello following:</span></span>

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


<span data-ttu-id="fe100-326">A Kubernetes használjon egy toogenerate hello titkok yam-parancsfájl munkaterület azonosítója és az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="fe100-326">For Kubernetes, use a script toogenerate hello secrets yaml file for Workspace ID and Primary Key.</span></span> <span data-ttu-id="fe100-327">A következő példa információk hello használata hello [omsagent yam fájl](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) toosecure a titkos adatokat.</span><span class="sxs-lookup"><span data-stu-id="fe100-327">Use hello following example information with hello [omsagent yaml file](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) toosecure your secret information.</span></span>

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

## <a name="windows-container-hosts"></a><span data-ttu-id="fe100-328">Windows-tároló gazdagépek</span><span class="sxs-lookup"><span data-stu-id="fe100-328">Windows container hosts</span></span>

### <a name="preparation-before-installing-windows-agents"></a><span data-ttu-id="fe100-329">Windows-ügynökök telepítése előtt előkészítése</span><span class="sxs-lookup"><span data-stu-id="fe100-329">Preparation before installing Windows agents</span></span>

<span data-ttu-id="fe100-330">Ügynökök telepítése Windows rendszerű számítógépekre, meg kell tooconfigure hello Docker szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fe100-330">Before you install agents on computers running Windows, you need tooconfigure hello Docker service.</span></span> <span data-ttu-id="fe100-331">hello konfigurációs lehetővé hello Windows-ügynököt vagy hello Naplóelemzési virtuális gép bővítmény toouse hello Docker TCP szoftvercsatorna érdekében, hogy a hello ügynökök hello Docker démon távolról és a toocapture adatok figyelését.</span><span class="sxs-lookup"><span data-stu-id="fe100-331">hello configuration allows hello Windows agent or hello Log Analytics virtual machine extension toouse hello Docker TCP socket so that hello agents can access hello Docker daemon remotely and toocapture data for monitoring.</span></span>

#### <a name="toostart-docker-and-verify-its-configuration"></a><span data-ttu-id="fe100-332">toostart Docker és a konfiguráció ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="fe100-332">toostart Docker and verify its configuration</span></span>

<span data-ttu-id="fe100-333">Nincsenek szükséges lépéseket tooset nevesített cső a Windows Server TCP fel:</span><span class="sxs-lookup"><span data-stu-id="fe100-333">There are steps needed tooset up TCP named pipe for Windows Server:</span></span>

1. <span data-ttu-id="fe100-334">A Windows PowerShellben engedélyezze a TCP cső és a nevesített cső.</span><span class="sxs-lookup"><span data-stu-id="fe100-334">In Windows PowerShell, enable TCP pipe and named pipe.</span></span>

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. <span data-ttu-id="fe100-335">Hello konfigurációs fájljának TCP cső Docker konfigurálásához, valamint a nevesített cső.</span><span class="sxs-lookup"><span data-stu-id="fe100-335">Configure Docker with hello configuration file for TCP pipe and named pipe.</span></span> <span data-ttu-id="fe100-336">hello konfigurációs fájl itt található: C:\ProgramData\docker\config\daemon.json.</span><span class="sxs-lookup"><span data-stu-id="fe100-336">hello configuration file is located at C:\ProgramData\docker\config\daemon.json.</span></span>

    <span data-ttu-id="fe100-337">Hello daemon.json fájlban a hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="fe100-337">In hello daemon.json file, you will need hello following:</span></span>

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

<span data-ttu-id="fe100-338">Windows tárolók használt hello Docker démon konfigurációjával kapcsolatos további információkért lásd: [Docker-motorhoz Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span><span class="sxs-lookup"><span data-stu-id="fe100-338">For more information about hello Docker daemon configuration used with Windows Containers, see [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span></span>


### <a name="install-windows-agents"></a><span data-ttu-id="fe100-339">Windows-ügynökök telepítése</span><span class="sxs-lookup"><span data-stu-id="fe100-339">Install Windows agents</span></span>

<span data-ttu-id="fe100-340">tooenable Windows és a Hyper-V tárolófigyelő, telepíthető Windows rendszerű számítógépek, amelyek tároló állomások hello Microsoft Monitoring Agent (MMA).</span><span class="sxs-lookup"><span data-stu-id="fe100-340">tooenable Windows and Hyper-V container monitoring, install hello Microsoft Monitoring Agent (MMA) on Windows computers that are container hosts.</span></span> <span data-ttu-id="fe100-341">A helyszíni környezetben futó Windows-számítógépek esetén lásd: [csatlakozás Windows számítógépek tooLog Analytics](log-analytics-windows-agents.md).</span><span class="sxs-lookup"><span data-stu-id="fe100-341">For computers running Windows in your on-premises environment, see [Connect Windows computers tooLog Analytics](log-analytics-windows-agents.md).</span></span> <span data-ttu-id="fe100-342">A virtuális gépek Azure-ban futó csatlakoztassa őket tooLog Analytics hello segítségével [virtuálisgép-bővítmény](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="fe100-342">For virtual machines running in Azure, connect them tooLog Analytics using hello [virtual machine extension](log-analytics-azure-vm-extension.md).</span></span>

<span data-ttu-id="fe100-343">A Service Fabric futó Windows-tárolók figyelheti.</span><span class="sxs-lookup"><span data-stu-id="fe100-343">You can monitor Windows containers running on Service Fabric.</span></span> <span data-ttu-id="fe100-344">Azonban csak [az Azure-ban futó virtuális gépek](log-analytics-azure-vm-extension.md) és [a helyszíni környezetben a Windows rendszert futtató számítógépek](log-analytics-windows-agents.md) jelenleg a Service Fabric támogatottak.</span><span class="sxs-lookup"><span data-stu-id="fe100-344">However, only [virtual machines running in Azure](log-analytics-azure-vm-extension.md) and [computers running Windows in your on-premises environment](log-analytics-windows-agents.md) are currently supported for Service Fabric.</span></span>

<span data-ttu-id="fe100-345">Ellenőrizheti, hogy hello tároló figyelésére szolgáló megoldás megfelelően van-e megadva a Windows.</span><span class="sxs-lookup"><span data-stu-id="fe100-345">You can verify that hello Container Monitoring solution is set correctly for Windows.</span></span> <span data-ttu-id="fe100-346">hello felügyeleti csomag letölthető volt megfelelően, hogy hely toocheck *ContainerManagement.xxx*.</span><span class="sxs-lookup"><span data-stu-id="fe100-346">toocheck whether hello management pack was download properly, look for *ContainerManagement.xxx*.</span></span> <span data-ttu-id="fe100-347">hello fájlok hello C:\Program Files\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok mappában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fe100-347">hello files should be in hello C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs folder.</span></span>


## <a name="solution-components"></a><span data-ttu-id="fe100-348">Megoldás-összetevők</span><span class="sxs-lookup"><span data-stu-id="fe100-348">Solution components</span></span>

<span data-ttu-id="fe100-349">Ha Windows-ügynökök használ, majd hello következő felügyeleti csomag telepítve van minden számítógépen, amelyen ügynök ebben a megoldásban hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="fe100-349">If you are using Windows agents, then hello following management pack is installed on each computer with an agent when you add this solution.</span></span> <span data-ttu-id="fe100-350">Nincs konfigurációs és karbantartási hello felügyeleti csomag szükség.</span><span class="sxs-lookup"><span data-stu-id="fe100-350">No configuration or maintenance is required for hello management pack.</span></span>

- <span data-ttu-id="fe100-351">*ContainerManagement.xxx* telepítve a C:\Program Files\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok</span><span class="sxs-lookup"><span data-stu-id="fe100-351">*ContainerManagement.xxx* installed in C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs</span></span>

## <a name="container-data-collection-details"></a><span data-ttu-id="fe100-352">Tároló az gyűjtemény adatait</span><span class="sxs-lookup"><span data-stu-id="fe100-352">Container data collection details</span></span>
<span data-ttu-id="fe100-353">hello tároló figyelésére szolgáló megoldás különböző metrikák és a napló teljesítményadatokat gyűjt tároló állomások és a tárolók ügynököt, amely engedélyezi a használ.</span><span class="sxs-lookup"><span data-stu-id="fe100-353">hello Container Monitoring solution collects various performance metrics and log data from container hosts and containers using agents that you enable.</span></span>

<span data-ttu-id="fe100-354">A következő ügynök típusok hello három percenként adatokat gyűjti.</span><span class="sxs-lookup"><span data-stu-id="fe100-354">Data is collected every three minutes by hello following agent types.</span></span>

- [<span data-ttu-id="fe100-355">Linux OMS-ügynök</span><span class="sxs-lookup"><span data-stu-id="fe100-355">OMS Agent for Linux</span></span>](log-analytics-linux-agents.md)
- [<span data-ttu-id="fe100-356">Windows-ügynök</span><span class="sxs-lookup"><span data-stu-id="fe100-356">Windows agent</span></span>](log-analytics-windows-agents.md)
- [<span data-ttu-id="fe100-357">Napló Analytics Virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="fe100-357">Log Analytics VM extension</span></span>](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a><span data-ttu-id="fe100-358">Tárolórekordok</span><span class="sxs-lookup"><span data-stu-id="fe100-358">Container records</span></span>

<span data-ttu-id="fe100-359">hello következő táblázat példákat mutat hello tároló figyelésére szolgáló megoldás és hello adattípusok, amelyek szerepelnek a találatok között napló által gyűjtött rekordok.</span><span class="sxs-lookup"><span data-stu-id="fe100-359">hello following table shows examples of records collected by hello Container Monitoring solution and hello data types that appear in log search results.</span></span>

| <span data-ttu-id="fe100-360">Adattípus</span><span class="sxs-lookup"><span data-stu-id="fe100-360">Data type</span></span> | <span data-ttu-id="fe100-361">A naplófájl-keresési adattípust tartalmaz</span><span class="sxs-lookup"><span data-stu-id="fe100-361">Data type in Log Search</span></span> | <span data-ttu-id="fe100-362">Mezők</span><span class="sxs-lookup"><span data-stu-id="fe100-362">Fields</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe100-363">A gazdagépek és a tárolók teljesítmény</span><span class="sxs-lookup"><span data-stu-id="fe100-363">Performance for hosts and containers</span></span> | `Type=Perf` | <span data-ttu-id="fe100-364">Számítógép, ObjectName, CounterName &#40; a processzor kihasználtsága, lemez beolvassa MB, szabad MB memória kihasználtsága (MB), írja hálózati fogadott bájtok, hálózati küldését bájt, a processzor kihasználtsága mp hálózati &#41; ellenértéknek, TimeGenerated, Számláló_elérési_útja, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="fe100-364">Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue,TimeGenerated, CounterPath, SourceSystem</span></span> |
| <span data-ttu-id="fe100-365">Tároló leltár</span><span class="sxs-lookup"><span data-stu-id="fe100-365">Container inventory</span></span> | `Type=ContainerInventory` | <span data-ttu-id="fe100-366">TimeGenerated, a számítógép, a tároló neve, ContainerHostname, kép, ImageTag, ContainerState, ExitCode, EnvironmentVar, a parancs, CreatedTime, StartedTime, FinishedTime, SourceSystem, Tárolóazonosító, ImageID</span><span class="sxs-lookup"><span data-stu-id="fe100-366">TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID</span></span> |
| <span data-ttu-id="fe100-367">Tároló kép készlet</span><span class="sxs-lookup"><span data-stu-id="fe100-367">Container image inventory</span></span> | `Type=ContainerImageInventory` | <span data-ttu-id="fe100-368">TimeGenerated, számítógép, kép, ImageTag, ImageSize, VirtualSize, futó, szünetel, leállítását követően nem sikerült, SourceSystem, ImageID, TotalContainer</span><span class="sxs-lookup"><span data-stu-id="fe100-368">TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer</span></span> |
| <span data-ttu-id="fe100-369">Tároló-napló</span><span class="sxs-lookup"><span data-stu-id="fe100-369">Container log</span></span> | `Type=ContainerLog` | <span data-ttu-id="fe100-370">TimeGenerated, a számítógép, a lemezkép-Azonosítót, a tároló neve, LogEntrySource, LogEntry, SourceSystem, Tárolóazonosító</span><span class="sxs-lookup"><span data-stu-id="fe100-370">TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="fe100-371">Tároló szolgáltatás bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="fe100-371">Container service log</span></span> | `Type=ContainerServiceLog`  | <span data-ttu-id="fe100-372">TimeGenerated, számítógép, TimeOfCommand, kép, a parancs, SourceSystem, Tárolóazonosító</span><span class="sxs-lookup"><span data-stu-id="fe100-372">TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="fe100-373">Tároló csomópont készlet</span><span class="sxs-lookup"><span data-stu-id="fe100-373">Container node inventory</span></span> | `Type=ContainerNodeInventory_CL`| <span data-ttu-id="fe100-374">TimeGenerated, számítógép, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="fe100-374">TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span></span>|
| <span data-ttu-id="fe100-375">Kubernetes készlet</span><span class="sxs-lookup"><span data-stu-id="fe100-375">Kubernetes inventory</span></span> | `Type=KubePodInventory_CL` | <span data-ttu-id="fe100-376">TimeGenerated, számítógép, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="fe100-376">TimeGenerated, Computer, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span></span> |
| <span data-ttu-id="fe100-377">Tároló folyamat</span><span class="sxs-lookup"><span data-stu-id="fe100-377">Container process</span></span> | `Type=ContainerProcess_CL` | <span data-ttu-id="fe100-378">TimeGenerated, számítógép, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="fe100-378">TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span></span> |
| <span data-ttu-id="fe100-379">Kubernetes események</span><span class="sxs-lookup"><span data-stu-id="fe100-379">Kubernetes events</span></span> | `Type=KubeEvents_CL` | <span data-ttu-id="fe100-380">TimeGenerated, számítógép, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, üzenet</span><span class="sxs-lookup"><span data-stu-id="fe100-380">TimeGenerated, Computer, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, Message</span></span> |

<span data-ttu-id="fe100-381">Címkék fűzött túl*PodLabel* adattípusok a következők saját címkét.</span><span class="sxs-lookup"><span data-stu-id="fe100-381">Labels appended too*PodLabel* data types are your own custom labels.</span></span> <span data-ttu-id="fe100-382">hello hozzáfűzött PodLabel címkék hello táblázatban szereplő példák.</span><span class="sxs-lookup"><span data-stu-id="fe100-382">hello appended PodLabel labels shown in hello table are examples.</span></span> <span data-ttu-id="fe100-383">Igen `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` lesznek a környezet adatkészlet különböznek és általános hasonlítanak `PodLabel_yourlabel_s`.</span><span class="sxs-lookup"><span data-stu-id="fe100-383">So, `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` will differ in your environment's data set and generically resemble `PodLabel_yourlabel_s`.</span></span>


## <a name="monitor-containers"></a><span data-ttu-id="fe100-384">A figyelő tárolók</span><span class="sxs-lookup"><span data-stu-id="fe100-384">Monitor containers</span></span>
<span data-ttu-id="fe100-385">Miután az OMS-portálon hello hello megoldás, hello **tárolók** csempe a tároló-gazdagépeken és az állomáson futó hello tárolók összegző információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fe100-385">After you have hello solution enabled in hello OMS portal, hello **Containers** tile shows summary information about your container hosts and hello containers running in hosts.</span></span>

![Tárolók csempe](./media/log-analytics-containers/containers-title.png)

<span data-ttu-id="fe100-387">hello csempe is áttekintheti, hogy hány tárolók hello környezetben, és hogy azok még nem sikerült, fut vagy leállt.</span><span class="sxs-lookup"><span data-stu-id="fe100-387">hello tile shows an overview of how many containers you have in hello environment and whether they're failed, running, or stopped.</span></span>

### <a name="using-hello-containers-dashboard"></a><span data-ttu-id="fe100-388">Hello tárolók irányítópult használata</span><span class="sxs-lookup"><span data-stu-id="fe100-388">Using hello Containers dashboard</span></span>
<span data-ttu-id="fe100-389">Kattintson a hello **tárolók** csempére.</span><span class="sxs-lookup"><span data-stu-id="fe100-389">Click hello **Containers** tile.</span></span> <span data-ttu-id="fe100-390">Ott látni fogja a nézetek szerint vannak rendszerezve:</span><span class="sxs-lookup"><span data-stu-id="fe100-390">From there you'll see views organized by:</span></span>

- <span data-ttu-id="fe100-391">**Tároló események** -tároló és a sikertelen tárolók rendelkező számítógépeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fe100-391">**Container Events** - Shows container status and computers with failed containers.</span></span>
- <span data-ttu-id="fe100-392">**Tároló naplók** -időt és azon számítógépek listáját a naplófájlok maximális számú hello generált tároló naplófájlok látható.</span><span class="sxs-lookup"><span data-stu-id="fe100-392">**Container Logs** - Shows a chart of container log files generated over time and a list of computers with hello highest number of log files.</span></span>
- <span data-ttu-id="fe100-393">**Kubernetes események** -látható Kubernetes eseményeit idő és miért három munkaállomás-csoporttal hello események létrehozása hello okok listáját.</span><span class="sxs-lookup"><span data-stu-id="fe100-393">**Kubernetes Events** - Shows a chart of Kubernetes events generated over time and a list of hello reasons why pods generated hello events.</span></span> <span data-ttu-id="fe100-394">*Az adatkészlet csak Linux környezetben használatos.*</span><span class="sxs-lookup"><span data-stu-id="fe100-394">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="fe100-395">**Kubernetes Namespace készlet** - névterek és három munkaállomás-csoporttal hello számát jeleníti meg, és megjeleníti a hierarchiában.</span><span class="sxs-lookup"><span data-stu-id="fe100-395">**Kubernetes Namespace Inventory** - Shows hello number of namespaces and pods and shows their hierarchy.</span></span> <span data-ttu-id="fe100-396">*Az adatkészlet csak Linux környezetben használatos.*</span><span class="sxs-lookup"><span data-stu-id="fe100-396">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="fe100-397">**Tároló csomópont készlet** -tároló csomópontok/állomáson használt vezénylési típusok hello számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fe100-397">**Container Node Inventory** - Shows hello number of orchestration types used on container nodes/hosts.</span></span> <span data-ttu-id="fe100-398">hello csomópontok vagy a számítógépen is tárolók hello száma alapján vannak listázva.</span><span class="sxs-lookup"><span data-stu-id="fe100-398">hello computer nodes/hosts are also listed by hello number of containers.</span></span> <span data-ttu-id="fe100-399">*Az adatkészlet csak Linux környezetben használatos.*</span><span class="sxs-lookup"><span data-stu-id="fe100-399">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="fe100-400">**Tároló képek készlet** -tároló képek használt hello száma és kép típusú mutatja.</span><span class="sxs-lookup"><span data-stu-id="fe100-400">**Container Images Inventory** - Shows hello total number of container images used and number of image types.</span></span> <span data-ttu-id="fe100-401">lemezképek hello számát is hello képcímke alapján vannak listázva.</span><span class="sxs-lookup"><span data-stu-id="fe100-401">hello number of images are also listed by hello image tag.</span></span>
- <span data-ttu-id="fe100-402">**Tárolók állapot** -tárolók futó csomópontok és a gazdagép olyan számítógépet jeleníti meg a tároló hello teljes száma.</span><span class="sxs-lookup"><span data-stu-id="fe100-402">**Containers Status** - Shows hello total number of container nodes/host computers that have running containers.</span></span> <span data-ttu-id="fe100-403">Számítógépek is hello futtató gazdagépek száma alapján vannak listázva.</span><span class="sxs-lookup"><span data-stu-id="fe100-403">Computers are also listed by hello number of running hosts.</span></span>
- <span data-ttu-id="fe100-404">**Tároló folyamat** -tároló folyamatok az állomásokon futó sor látható.</span><span class="sxs-lookup"><span data-stu-id="fe100-404">**Container Process** - Shows a line chart of container processes running over time.</span></span> <span data-ttu-id="fe100-405">Tárolók is találhatók futtatásával parancs/folyamat tárolók belül.</span><span class="sxs-lookup"><span data-stu-id="fe100-405">Containers are also listed by running command/process within containers.</span></span> <span data-ttu-id="fe100-406">*Az adatkészlet csak Linux környezetben használatos.*</span><span class="sxs-lookup"><span data-stu-id="fe100-406">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="fe100-407">**Tároló processzorteljesítmény** -hello számítógép csomópontok/gazdagépek időbeli átlagos processzorkihasználtság sor látható.</span><span class="sxs-lookup"><span data-stu-id="fe100-407">**Container CPU Performance** - Shows a line chart of hello average CPU utilization over time for computer nodes/hosts.</span></span> <span data-ttu-id="fe100-408">Is listák hello számítógép csomópontok/futtatja alapján átlagos CPU-felhasználást.</span><span class="sxs-lookup"><span data-stu-id="fe100-408">Also lists hello computer nodes/hosts based on average CPU utilization.</span></span>
- <span data-ttu-id="fe100-409">**Tároló memóriateljesítmény** -memóriahasználatának vonaldiagram időbeli változását ábrázolja.</span><span class="sxs-lookup"><span data-stu-id="fe100-409">**Container Memory Performance** - Shows a line chart of memory usage over time.</span></span> <span data-ttu-id="fe100-410">Számítógép memória kihasználtsága a példány neve alapján is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fe100-410">Also lists computer memory utilization based on instance name.</span></span>
- <span data-ttu-id="fe100-411">**A számítógép teljesítménye** -időbeli változását ábrázolja vonaldiagramok processzorteljesítmény idővel hello százaléka memóriahasználat idő és a megabájtokban kifejezett szabad lemezterület százalékában.</span><span class="sxs-lookup"><span data-stu-id="fe100-411">**Computer Performance** - Shows line charts of hello percent of CPU performance over time, percent of memory usage over time, and megabytes of free disk space over time.</span></span> <span data-ttu-id="fe100-412">Is rámutat egy diagram tooview bármely sorára további részleteket.</span><span class="sxs-lookup"><span data-stu-id="fe100-412">You can hover over any line in a chart tooview more details.</span></span>


<span data-ttu-id="fe100-413">Mindegyik hello irányítópult területe a Keresés a gyűjtött adatok futó vizuális ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="fe100-413">Each area of hello dashboard is a visual representation of a search that is run on collected data.</span></span>

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash01.png)

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash02.png)

<span data-ttu-id="fe100-416">A hello **tároló** területen hello felső területen kattintson a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="fe100-416">In hello **Container Status** area, click hello top area, as shown below.</span></span>

![Tárolók állapota](./media/log-analytics-containers/containers-status.png)

<span data-ttu-id="fe100-418">Naplófájl-keresési jelenik meg, amelyen a tárolók hello állapotával kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="fe100-418">Log Search opens, displaying information about hello state of your containers.</span></span>

![Tárolók napló keresése](./media/log-analytics-containers/containers-log-search.png)

<span data-ttu-id="fe100-420">Itt szerkesztheti hello keresési lekérdezés toomodify azt toofind hello konkrét információk kíváncsiak vagyunk.</span><span class="sxs-lookup"><span data-stu-id="fe100-420">From here, you can edit hello search query toomodify it toofind hello specific information you're interested in.</span></span> <span data-ttu-id="fe100-421">Napló keresések kapcsolatos további információkért lásd: [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="fe100-421">For more information about Log Searches, see [Log searches in Log Analytics](log-analytics-log-searches.md).</span></span>

## <a name="troubleshoot-by-finding-a-failed-container"></a><span data-ttu-id="fe100-422">Keresse meg a sikertelen tároló hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="fe100-422">Troubleshoot by finding a failed container</span></span>

<span data-ttu-id="fe100-423">A Naplóelemzési jelöli meg a tárolóban **sikertelen** Ha egy nem nulla kilépési kóddal kilépett.</span><span class="sxs-lookup"><span data-stu-id="fe100-423">Log Analytics marks a container as **Failed** if it has exited with a non-zero exit code.</span></span> <span data-ttu-id="fe100-424">Az áttekintésben hello hibák és a hibákat a hello hello környezetben **sikertelen tárolók** területen.</span><span class="sxs-lookup"><span data-stu-id="fe100-424">You can see an overview of hello errors and failures in hello environment in hello **Failed Containers** area.</span></span>

### <a name="toofind-failed-containers"></a><span data-ttu-id="fe100-425">nem sikerült toofind tárolók</span><span class="sxs-lookup"><span data-stu-id="fe100-425">toofind failed containers</span></span>
1. <span data-ttu-id="fe100-426">Kattintson a hello **tároló** területen.</span><span class="sxs-lookup"><span data-stu-id="fe100-426">Click hello **Container Status** area.</span></span>  
   <span data-ttu-id="fe100-427">![tárolók állapota](./media/log-analytics-containers/containers-status.png)</span><span class="sxs-lookup"><span data-stu-id="fe100-427">![containers status](./media/log-analytics-containers/containers-status.png)</span></span>
2. <span data-ttu-id="fe100-428">Naplófájl-keresési megnyílik, és a tároló, a következő hasonló toohello hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fe100-428">Log Search opens and displays hello state of your containers, similar toohello following.</span></span>  
   ![tárolók állapota](./media/log-analytics-containers/containers-log-search.png)
3. <span data-ttu-id="fe100-430">Ezután kattintson a hello összesített értékének sikertelen tárolók tooview további információt.</span><span class="sxs-lookup"><span data-stu-id="fe100-430">Next, click hello aggregated value of failed containers tooview additional information.</span></span> <span data-ttu-id="fe100-431">Bontsa ki a **megjelenítése további** tooview hello kép azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fe100-431">Expand **show more** tooview hello image ID.</span></span>  
   <span data-ttu-id="fe100-432">![nem sikerült tárolók](./media/log-analytics-containers/containers-state-failed.png)</span><span class="sxs-lookup"><span data-stu-id="fe100-432">![failed containers](./media/log-analytics-containers/containers-state-failed.png)</span></span>  
4. <span data-ttu-id="fe100-433">Írja be a következő hello következőt hello keresési lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="fe100-433">Next, type hello following in hello search query.</span></span> <span data-ttu-id="fe100-434">`Type=ContainerInventory <ImageID>`például a kép mérete és a leállított és sikertelen képek hello kép toosee adatait.</span><span class="sxs-lookup"><span data-stu-id="fe100-434">`Type=ContainerInventory <ImageID>` toosee details about hello image such as image size and number of stopped and failed images.</span></span>  
   <span data-ttu-id="fe100-435">![nem sikerült tárolók](./media/log-analytics-containers/containers-failed04.png)</span><span class="sxs-lookup"><span data-stu-id="fe100-435">![failed containers](./media/log-analytics-containers/containers-failed04.png)</span></span>

## <a name="search-logs-for-container-data"></a><span data-ttu-id="fe100-436">Keresési naplókat a további adatai</span><span class="sxs-lookup"><span data-stu-id="fe100-436">Search logs for container data</span></span>
<span data-ttu-id="fe100-437">Ha egy adott hiba hibaelhárítás segíthet toosee hol lépett fel a környezetben.</span><span class="sxs-lookup"><span data-stu-id="fe100-437">When you're troubleshooting a specific error, it can help toosee where it is occurring in your environment.</span></span> <span data-ttu-id="fe100-438">a következő napló típusok hello segítségével hozhat létre lekérdezéseket kívánt tooreturn hello információt.</span><span class="sxs-lookup"><span data-stu-id="fe100-438">hello following log types will help you create queries tooreturn hello information you want.</span></span>


- <span data-ttu-id="fe100-439">**ContainerImageInventory** – ezt a típust használja a lemezkép és tooview kép információt például a lemezkép-azonosítók vagy méretek szerint vannak rendszerezve toofind információk regisztráláskor.</span><span class="sxs-lookup"><span data-stu-id="fe100-439">**ContainerImageInventory** – Use this type when you're trying toofind information organized by image and tooview image information such as image IDs or sizes.</span></span>
- <span data-ttu-id="fe100-440">**ContainerInventory** – Ez a típus használható, ha azt szeretné, hogy információt tároló helye, Mik azok a nevük, és mi képek azok futtatja.</span><span class="sxs-lookup"><span data-stu-id="fe100-440">**ContainerInventory** – Use this type when you want information about container location, what their names are, and what images they're running.</span></span>
- <span data-ttu-id="fe100-441">**ContainerLog** – Ez a típus használható, ha toofind adott hibanaplóban szereplő információkat, és bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="fe100-441">**ContainerLog** – Use this type when you want toofind specific error log information and entries.</span></span>
- <span data-ttu-id="fe100-442">**ContainerNodeInventory_CL** használni ehhez a típushoz hello információ a gazdacsomópont/ahol tárolók tartózkodnak.</span><span class="sxs-lookup"><span data-stu-id="fe100-442">**ContainerNodeInventory_CL**  Use this type when you want hello information about host/node where containers are residing.</span></span> <span data-ttu-id="fe100-443">Azt teszi lehetővé Docker verzióval, vezénylés típusát, tárolási és hálózati információit.</span><span class="sxs-lookup"><span data-stu-id="fe100-443">It provides you Docker version, orchestration type, storage, and network information.</span></span>
- <span data-ttu-id="fe100-444">**ContainerProcess_CL** használja a típus tooquickly lásd: hello tárolóban futó hello folyamata.</span><span class="sxs-lookup"><span data-stu-id="fe100-444">**ContainerProcess_CL** Use this type tooquickly see hello process running within hello container.</span></span>
- <span data-ttu-id="fe100-445">**ContainerServiceLog** – Ez a típus használható regisztráláskor toofind ellenőrzési útvonalat nyújt információkat a hello Docker démon, például Indítás, Leállítás, törléséhez vagy parancsok lekéréses.</span><span class="sxs-lookup"><span data-stu-id="fe100-445">**ContainerServiceLog** – Use this type when you're trying toofind audit trail information for hello Docker daemon, such as start, stop, delete, or pull commands.</span></span>
- <span data-ttu-id="fe100-446">**KubeEvents_CL** használja a típus toosee hello Kubernetes eseményeket.</span><span class="sxs-lookup"><span data-stu-id="fe100-446">**KubeEvents_CL**  Use this type toosee hello Kubernetes events.</span></span>
- <span data-ttu-id="fe100-447">**KubePodInventory_CL** ezt a típust használja, ha azt szeretné, hogy toounderstand hello fürt hierarchiára vonatkozó információk.</span><span class="sxs-lookup"><span data-stu-id="fe100-447">**KubePodInventory_CL**  Use this type when you want toounderstand hello cluster hierarchy information.</span></span>


### <a name="toosearch-logs-for-container-data"></a><span data-ttu-id="fe100-448">a tároló adatok toosearch naplók</span><span class="sxs-lookup"><span data-stu-id="fe100-448">toosearch logs for container data</span></span>
* <span data-ttu-id="fe100-449">Válassza ki, hogy tudja, hogy a képfájl nemrég sikertelen volt, és hello hibanaplók keresése.</span><span class="sxs-lookup"><span data-stu-id="fe100-449">Choose an image that you know has failed recently and find hello error logs for it.</span></span> <span data-ttu-id="fe100-450">Indítsa el a tároló neve, amelyen fut. a lemezkép keresése a **ContainerInventory** keresési.</span><span class="sxs-lookup"><span data-stu-id="fe100-450">Start by finding a container name that is running that image with a **ContainerInventory** search.</span></span> <span data-ttu-id="fe100-451">Például keresése`Type=ContainerInventory ubuntu Failed`</span><span class="sxs-lookup"><span data-stu-id="fe100-451">For example, search for `Type=ContainerInventory ubuntu Failed`</span></span>  
    <span data-ttu-id="fe100-452">![Ubuntu tárolók keresése](./media/log-analytics-containers/search-ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="fe100-452">![Search for Ubuntu containers](./media/log-analytics-containers/search-ubuntu.png)</span></span>

  <span data-ttu-id="fe100-453">hello hello tároló neve melletti túl**neve**, majd keresse meg a lesznek a naplók.</span><span class="sxs-lookup"><span data-stu-id="fe100-453">hello name of hello container next too**Name**, and search for those logs.</span></span> <span data-ttu-id="fe100-454">Ebben a példában ez `Type=ContainerLog cranky_stonebreaker`.</span><span class="sxs-lookup"><span data-stu-id="fe100-454">In this example, it is `Type=ContainerLog cranky_stonebreaker`.</span></span>

<span data-ttu-id="fe100-455">**Teljesítmény-információk megtekintése**</span><span class="sxs-lookup"><span data-stu-id="fe100-455">**View performance information**</span></span>

<span data-ttu-id="fe100-456">Amikor tooconstruct lekérdezések verziótól segíthet toosee Újdonságok lehetséges először.</span><span class="sxs-lookup"><span data-stu-id="fe100-456">When you're beginning tooconstruct queries, it can help toosee what's possible first.</span></span> <span data-ttu-id="fe100-457">Például minden teljesítményadat, írja be a széles körű lekérdezés hello követően próbálja meg keresési toosee lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="fe100-457">For example, toosee all performance data, try a broad query by typing hello following search query.</span></span>

```
Type=Perf
```

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf01.png)

<span data-ttu-id="fe100-459">Hello teljesítményadatokat is lát tooa adott tárolóhoz azt hello nevének beírásával toohello sarkában a lekérdezés hatókörét megadhatja.</span><span class="sxs-lookup"><span data-stu-id="fe100-459">You can scope hello performance data you're seeing tooa specific container by typing hello name of it toohello right of your query.</span></span>

```
Type=Perf <containerName>
```

<span data-ttu-id="fe100-460">Amely az egyes tároló összegyűjtött teljesítménymutatók hello listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fe100-460">That shows hello list of performance metrics that are collected for an individual container.</span></span>

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a><span data-ttu-id="fe100-462">Példa napló keresési lekérdezések</span><span class="sxs-lookup"><span data-stu-id="fe100-462">Example log search queries</span></span>
<span data-ttu-id="fe100-463">Fennállt gyakran hasznos toobuild lekérdezi például vagy két kezdve, és adja őket az toofit módosítása a környezetben.</span><span class="sxs-lookup"><span data-stu-id="fe100-463">It's often useful toobuild queries starting with an example or two and then modifying them toofit your environment.</span></span> <span data-ttu-id="fe100-464">Kiindulási pontként, kísérletezhet hello **mintalekérdezések** terület toohelp összetettebb lekérdezések létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fe100-464">As a starting point, you can experiment with hello **Sample Queries** area toohelp you build more advanced queries.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Tárolók lekérdezések](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a><span data-ttu-id="fe100-466">Naplófájl-keresési lekérdezések mentése</span><span class="sxs-lookup"><span data-stu-id="fe100-466">Saving log search queries</span></span>
<span data-ttu-id="fe100-467">Lekérdezések mentése az Naplóelemzési szabványos szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="fe100-467">Saving queries is a standard feature in Log Analytics.</span></span> <span data-ttu-id="fe100-468">Mentve, konfigurálnia kell az alábbiakhoz hasznos talált későbbi használatra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="fe100-468">By saving them, you'll have those that you've found useful handy for future use.</span></span>

<span data-ttu-id="fe100-469">Miután létrehozott egy lekérdezést, amely akkor hasznosak, mentse kattintva **Kedvencek** hello napló keresése oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="fe100-469">After you create a query that you find useful, save it by clicking **Favorites** at hello top of hello Log Search page.</span></span> <span data-ttu-id="fe100-470">Ezután könnyen elérhető azt később hello **saját irányítópult** lap.</span><span class="sxs-lookup"><span data-stu-id="fe100-470">Then you can easily access it later from hello **My Dashboard** page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe100-471">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe100-471">Next steps</span></span>
* <span data-ttu-id="fe100-472">[Naplók keresése](log-analytics-log-searches.md) tooview részletes tároló rekordok.</span><span class="sxs-lookup"><span data-stu-id="fe100-472">[Search logs](log-analytics-log-searches.md) tooview detailed container data records.</span></span>
