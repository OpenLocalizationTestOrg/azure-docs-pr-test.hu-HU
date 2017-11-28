---
title: "Tároló figyelés megoldás az Azure Naplóelemzés |} Microsoft Docs"
description: "A tároló figyelésére szolgáló megoldás a Log Analyticshez segít megtekintése és kezelése a Docker és a Windows tároló állomások egyetlen helyen megvalósítható."
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
ms.openlocfilehash: b2e03531ee401f4552198e5dd50fbfe1d970f0e5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="container-monitoring-solution-in-log-analytics"></a><span data-ttu-id="1a28a-103">A Naplóelemzési tároló figyelés megoldás</span><span class="sxs-lookup"><span data-stu-id="1a28a-103">Container Monitoring solution in Log Analytics</span></span>

![Tárolók szimbólum](./media/log-analytics-containers/containers-symbol.png)

<span data-ttu-id="1a28a-105">Ez a cikk ismerteti, hogyan állítsa be, és a tároló figyelésére szolgáló megoldás használata Naplóelemzési, amely segít megtekintése és kezelése a Docker és a Windows tároló állomások egyetlen helyen megvalósítható.</span><span class="sxs-lookup"><span data-stu-id="1a28a-105">This article describes how to set up and use the Container Monitoring solution in Log Analytics, which helps you view and manage your Docker and Windows container hosts in a single location.</span></span> <span data-ttu-id="1a28a-106">Docker egy olyan szoftver virtualizálási a rendszer, az informatikai infrastruktúra szoftver központi telepítését automatizáló tárolók létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="1a28a-106">Docker is a software virtualization system used to create containers that automate software deployment to their IT infrastructure.</span></span>

<span data-ttu-id="1a28a-107">A megoldás bemutatja, hogy mely tárolók fut, a tároló képet azok futtatja, és ahol tárolók futnak.</span><span class="sxs-lookup"><span data-stu-id="1a28a-107">The solution shows which containers are running, what container image they’re running, and where containers are running.</span></span> <span data-ttu-id="1a28a-108">Tárolók használt parancsok bemutató részletes naplózási információk is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="1a28a-108">You can view detailed audit information showing commands used with containers.</span></span> <span data-ttu-id="1a28a-109">És tárolók megtekintésével és központosított naplók keresése távolról megtekintheti a Docker vagy a Windows rendszerű nélkül is elháríthatók.</span><span class="sxs-lookup"><span data-stu-id="1a28a-109">And, you can troubleshoot containers by viewing and searching centralized logs without having to remotely view Docker or Windows hosts.</span></span> <span data-ttu-id="1a28a-110">Tárolók, amelyek esetleg zajos vagy a felhasználó felesleges erőforrások egy gazdagépen található.</span><span class="sxs-lookup"><span data-stu-id="1a28a-110">You can find containers that may be noisy and consuming excess resources on a host.</span></span> <span data-ttu-id="1a28a-111">És megtekintheti a központi Processzor, memória, tárolási és hálózati használati és teljesítményadatokat adatai tárolók.</span><span class="sxs-lookup"><span data-stu-id="1a28a-111">And, you can view centralized CPU, memory, storage, and network usage and performance information for containers.</span></span> <span data-ttu-id="1a28a-112">Windows rendszerű számítógépeken központosítása és hasonlítsa össze a napló, a Windows Server Hyper-V és a Docker-tárolók.</span><span class="sxs-lookup"><span data-stu-id="1a28a-112">On computers running Windows, you can centralize and compare logs from Windows Server, Hyper-V, and Docker containers.</span></span> <span data-ttu-id="1a28a-113">A megoldás a következő tároló orchestrators támogatja:</span><span class="sxs-lookup"><span data-stu-id="1a28a-113">The solution supports the following container orchestrators:</span></span>

- <span data-ttu-id="1a28a-114">Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="1a28a-114">Docker Swarm</span></span>
- <span data-ttu-id="1a28a-115">DC/OS</span><span class="sxs-lookup"><span data-stu-id="1a28a-115">DC/OS</span></span>
- <span data-ttu-id="1a28a-116">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="1a28a-116">Kubernetes</span></span>
- <span data-ttu-id="1a28a-117">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1a28a-117">Service Fabric</span></span>
- <span data-ttu-id="1a28a-118">Red Hat OpenShift</span><span class="sxs-lookup"><span data-stu-id="1a28a-118">Red Hat OpenShift</span></span>


<span data-ttu-id="1a28a-119">Az alábbi ábrán látható a különböző tároló gazdagépek és az OMS ügynökök közötti kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="1a28a-119">The following diagram shows the relationships between various container hosts and agents with OMS.</span></span>

![Tárolók diagramja](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a><span data-ttu-id="1a28a-121">Rendszerkövetelmények</span><span class="sxs-lookup"><span data-stu-id="1a28a-121">System requirements</span></span>

<span data-ttu-id="1a28a-122">Megkezdése előtt tekintse át az alábbi részleteket megfelel az Előfeltételek ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1a28a-122">Before starting, review the following details to verify you meet the prerequisites.</span></span>

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a><span data-ttu-id="1a28a-123">Figyelési megoldást igényelnek tároló Docker Orchestrator és az operációs rendszer platform támogatása</span><span class="sxs-lookup"><span data-stu-id="1a28a-123">Container monitoring solution support for Docker Orchestrator and OS platform</span></span>
<span data-ttu-id="1a28a-124">Az alábbi táblázat ismerteti a Docker vezénylési és az operációs rendszer figyelési tároló készlet, a teljesítmény és a naplók és a Naplóelemzési támogatást.</span><span class="sxs-lookup"><span data-stu-id="1a28a-124">The following table outlines the Docker orchestration and operating system monitoring support of container inventory, performance, and logs with Log Analytics.</span></span>   

| | <span data-ttu-id="1a28a-125">ACS</span><span class="sxs-lookup"><span data-stu-id="1a28a-125">ACS</span></span> | <span data-ttu-id="1a28a-126">Linux</span><span class="sxs-lookup"><span data-stu-id="1a28a-126">Linux</span></span> | <span data-ttu-id="1a28a-127">Windows</span><span class="sxs-lookup"><span data-stu-id="1a28a-127">Windows</span></span> | <span data-ttu-id="1a28a-128">Tároló</span><span class="sxs-lookup"><span data-stu-id="1a28a-128">Container</span></span><br><span data-ttu-id="1a28a-129">Szoftverleltár</span><span class="sxs-lookup"><span data-stu-id="1a28a-129">Inventory</span></span> | <span data-ttu-id="1a28a-130">Kép</span><span class="sxs-lookup"><span data-stu-id="1a28a-130">Image</span></span><br><span data-ttu-id="1a28a-131">Szoftverleltár</span><span class="sxs-lookup"><span data-stu-id="1a28a-131">Inventory</span></span> | <span data-ttu-id="1a28a-132">Csomópont</span><span class="sxs-lookup"><span data-stu-id="1a28a-132">Node</span></span><br><span data-ttu-id="1a28a-133">Szoftverleltár</span><span class="sxs-lookup"><span data-stu-id="1a28a-133">Inventory</span></span> | <span data-ttu-id="1a28a-134">Tároló</span><span class="sxs-lookup"><span data-stu-id="1a28a-134">Container</span></span><br><span data-ttu-id="1a28a-135">Teljesítmény</span><span class="sxs-lookup"><span data-stu-id="1a28a-135">Performance</span></span> | <span data-ttu-id="1a28a-136">Tároló</span><span class="sxs-lookup"><span data-stu-id="1a28a-136">Container</span></span><br><span data-ttu-id="1a28a-137">Esemény</span><span class="sxs-lookup"><span data-stu-id="1a28a-137">Event</span></span> | <span data-ttu-id="1a28a-138">Esemény</span><span class="sxs-lookup"><span data-stu-id="1a28a-138">Event</span></span><br><span data-ttu-id="1a28a-139">Napló</span><span class="sxs-lookup"><span data-stu-id="1a28a-139">Log</span></span> | <span data-ttu-id="1a28a-140">Tároló</span><span class="sxs-lookup"><span data-stu-id="1a28a-140">Container</span></span><br><span data-ttu-id="1a28a-141">Napló</span><span class="sxs-lookup"><span data-stu-id="1a28a-141">Log</span></span> |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| <span data-ttu-id="1a28a-142">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="1a28a-142">Kubernetes</span></span> | <span data-ttu-id="1a28a-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-143">&#8226;</span></span> | <span data-ttu-id="1a28a-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-144">&#8226;</span></span> | | <span data-ttu-id="1a28a-145">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-145">&#8226;</span></span> | <span data-ttu-id="1a28a-146">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-146">&#8226;</span></span> | <span data-ttu-id="1a28a-147">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-147">&#8226;</span></span> | <span data-ttu-id="1a28a-148">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-148">&#8226;</span></span> | <span data-ttu-id="1a28a-149">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-149">&#8226;</span></span> | <span data-ttu-id="1a28a-150">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-150">&#8226;</span></span> | <span data-ttu-id="1a28a-151">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-151">&#8226;</span></span> |
| <span data-ttu-id="1a28a-152">Mesosphere</span><span class="sxs-lookup"><span data-stu-id="1a28a-152">Mesosphere</span></span><br><span data-ttu-id="1a28a-153">DC/OS</span><span class="sxs-lookup"><span data-stu-id="1a28a-153">DC/OS</span></span> | <span data-ttu-id="1a28a-154">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-154">&#8226;</span></span> | <span data-ttu-id="1a28a-155">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-155">&#8226;</span></span> | | <span data-ttu-id="1a28a-156">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-156">&#8226;</span></span> | <span data-ttu-id="1a28a-157">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-157">&#8226;</span></span> | <span data-ttu-id="1a28a-158">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-158">&#8226;</span></span> | <span data-ttu-id="1a28a-159">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-159">&#8226;</span></span>| <span data-ttu-id="1a28a-160">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-160">&#8226;</span></span> | <span data-ttu-id="1a28a-161">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-161">&#8226;</span></span> | <span data-ttu-id="1a28a-162">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-162">&#8226;</span></span> |
| <span data-ttu-id="1a28a-163">Docker</span><span class="sxs-lookup"><span data-stu-id="1a28a-163">Docker</span></span><br><span data-ttu-id="1a28a-164">Swarm</span><span class="sxs-lookup"><span data-stu-id="1a28a-164">Swarm</span></span> | <span data-ttu-id="1a28a-165">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-165">&#8226;</span></span> | <span data-ttu-id="1a28a-166">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-166">&#8226;</span></span> | <span data-ttu-id="1a28a-167">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-167">&#8226;</span></span> | <span data-ttu-id="1a28a-168">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-168">&#8226;</span></span> | <span data-ttu-id="1a28a-169">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-169">&#8226;</span></span> | <span data-ttu-id="1a28a-170">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-170">&#8226;</span></span> | <span data-ttu-id="1a28a-171">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-171">&#8226;</span></span> | <span data-ttu-id="1a28a-172">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-172">&#8226;</span></span> | | <span data-ttu-id="1a28a-173">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-173">&#8226;</span></span> |
| <span data-ttu-id="1a28a-174">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="1a28a-174">Service</span></span><br><span data-ttu-id="1a28a-175">Háló</span><span class="sxs-lookup"><span data-stu-id="1a28a-175">Fabric</span></span> | | | <span data-ttu-id="1a28a-176">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-176">&#8226;</span></span> | <span data-ttu-id="1a28a-177">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-177">&#8226;</span></span> | <span data-ttu-id="1a28a-178">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-178">&#8226;</span></span> | <span data-ttu-id="1a28a-179">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-179">&#8226;</span></span> | <span data-ttu-id="1a28a-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-180">&#8226;</span></span> | <span data-ttu-id="1a28a-181">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-181">&#8226;</span></span> | <span data-ttu-id="1a28a-182">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-182">&#8226;</span></span> | <span data-ttu-id="1a28a-183">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-183">&#8226;</span></span> |
| <span data-ttu-id="1a28a-184">Red Hat megnyitása</span><span class="sxs-lookup"><span data-stu-id="1a28a-184">Red Hat Open</span></span><br><span data-ttu-id="1a28a-185">SHIFT</span><span class="sxs-lookup"><span data-stu-id="1a28a-185">Shift</span></span> | | <span data-ttu-id="1a28a-186">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-186">&#8226;</span></span> | | <span data-ttu-id="1a28a-187">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-187">&#8226;</span></span> | <span data-ttu-id="1a28a-188">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-188">&#8226;</span></span>| <span data-ttu-id="1a28a-189">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-189">&#8226;</span></span> | <span data-ttu-id="1a28a-190">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-190">&#8226;</span></span> | <span data-ttu-id="1a28a-191">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-191">&#8226;</span></span> | | <span data-ttu-id="1a28a-192">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-192">&#8226;</span></span> |
| <span data-ttu-id="1a28a-193">Windows Server</span><span class="sxs-lookup"><span data-stu-id="1a28a-193">Windows Server</span></span><br><span data-ttu-id="1a28a-194">(önálló)</span><span class="sxs-lookup"><span data-stu-id="1a28a-194">(standalone)</span></span> | | | <span data-ttu-id="1a28a-195">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-195">&#8226;</span></span> | <span data-ttu-id="1a28a-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-196">&#8226;</span></span> | <span data-ttu-id="1a28a-197">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-197">&#8226;</span></span> | <span data-ttu-id="1a28a-198">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-198">&#8226;</span></span> | <span data-ttu-id="1a28a-199">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-199">&#8226;</span></span> | <span data-ttu-id="1a28a-200">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-200">&#8226;</span></span> | | <span data-ttu-id="1a28a-201">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-201">&#8226;</span></span> |
| <span data-ttu-id="1a28a-202">Linux-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="1a28a-202">Linux Server</span></span><br><span data-ttu-id="1a28a-203">(önálló)</span><span class="sxs-lookup"><span data-stu-id="1a28a-203">(standalone)</span></span> | | <span data-ttu-id="1a28a-204">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-204">&#8226;</span></span> | | <span data-ttu-id="1a28a-205">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-205">&#8226;</span></span> | <span data-ttu-id="1a28a-206">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-206">&#8226;</span></span> | <span data-ttu-id="1a28a-207">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-207">&#8226;</span></span> | <span data-ttu-id="1a28a-208">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-208">&#8226;</span></span> | <span data-ttu-id="1a28a-209">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-209">&#8226;</span></span> | | <span data-ttu-id="1a28a-210">&#8226;</span><span class="sxs-lookup"><span data-stu-id="1a28a-210">&#8226;</span></span> |


### <a name="docker-versions-supported-on-linux"></a><span data-ttu-id="1a28a-211">Támogatott Linux docker-verziók</span><span class="sxs-lookup"><span data-stu-id="1a28a-211">Docker versions supported on Linux</span></span>

- <span data-ttu-id="1a28a-212">Docker 1.11-1,13.</span><span class="sxs-lookup"><span data-stu-id="1a28a-212">Docker 1.11 to 1.13</span></span>
- <span data-ttu-id="1a28a-213">Docker CE és EE v17.06</span><span class="sxs-lookup"><span data-stu-id="1a28a-213">Docker CE and EE v17.06</span></span>

### <a name="x64-linux-distributions-supported-as-container-hosts"></a><span data-ttu-id="1a28a-214">x64 tároló gazdagépként támogatott Linux-disztribúciók</span><span class="sxs-lookup"><span data-stu-id="1a28a-214">x64 Linux distributions supported as container hosts</span></span>

- <span data-ttu-id="1a28a-215">Ubuntu 14.04 LTS és 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="1a28a-215">Ubuntu 14.04 LTS and 16.04 LTS</span></span>
- <span data-ttu-id="1a28a-216">CoreOS(stable)</span><span class="sxs-lookup"><span data-stu-id="1a28a-216">CoreOS(stable)</span></span>
- <span data-ttu-id="1a28a-217">Amazon Linux 2016.09.0</span><span class="sxs-lookup"><span data-stu-id="1a28a-217">Amazon Linux 2016.09.0</span></span>
- <span data-ttu-id="1a28a-218">openSUSE, 13.2</span><span class="sxs-lookup"><span data-stu-id="1a28a-218">openSUSE 13.2</span></span>
- <span data-ttu-id="1a28a-219">openSUSE LEAP 42.2</span><span class="sxs-lookup"><span data-stu-id="1a28a-219">openSUSE LEAP 42.2</span></span>
- <span data-ttu-id="1a28a-220">7.2 és 7.3 centOS</span><span class="sxs-lookup"><span data-stu-id="1a28a-220">CentOS 7.2 and 7.3</span></span>
- <span data-ttu-id="1a28a-221">SLES 12</span><span class="sxs-lookup"><span data-stu-id="1a28a-221">SLES 12</span></span>
- <span data-ttu-id="1a28a-222">RHEL 7.2 és 7.3.</span><span class="sxs-lookup"><span data-stu-id="1a28a-222">RHEL 7.2 and 7.3</span></span>
- <span data-ttu-id="1a28a-223">Red Hat OpenShift tároló Platform (OCP) 3.4-es és 3.5-ös verzióját</span><span class="sxs-lookup"><span data-stu-id="1a28a-223">Red Hat OpenShift Container Platform (OCP) 3.4 and 3.5</span></span>
- <span data-ttu-id="1a28a-224">ACS Mesosphere DC/OS 1.7.3 való 1.8.8</span><span class="sxs-lookup"><span data-stu-id="1a28a-224">ACS Mesosphere DC/OS 1.7.3 to 1.8.8</span></span>
- <span data-ttu-id="1a28a-225">ACS Kubernetes 1.4.5 az 1.6-os</span><span class="sxs-lookup"><span data-stu-id="1a28a-225">ACS Kubernetes 1.4.5 to 1.6</span></span>
- <span data-ttu-id="1a28a-226">ACS Docker swarm használata</span><span class="sxs-lookup"><span data-stu-id="1a28a-226">ACS Docker Swarm</span></span>

### <a name="supported-windows-operating-system"></a><span data-ttu-id="1a28a-227">Támogatott Windows operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="1a28a-227">Supported Windows operating system</span></span>

- <span data-ttu-id="1a28a-228">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="1a28a-228">Windows Server 2016</span></span>
- <span data-ttu-id="1a28a-229">Windows 10 évforduló Edition (Professional vagy vállalati)</span><span class="sxs-lookup"><span data-stu-id="1a28a-229">Windows 10 Anniversary Edition (Professional or Enterprise)</span></span>

### <a name="docker-versions-supported-on-windows"></a><span data-ttu-id="1a28a-230">A Windows támogatott docker-verziók</span><span class="sxs-lookup"><span data-stu-id="1a28a-230">Docker versions supported on Windows</span></span>

- <span data-ttu-id="1a28a-231">Docker 1.12 és 1,13.</span><span class="sxs-lookup"><span data-stu-id="1a28a-231">Docker 1.12 and 1.13</span></span>
- <span data-ttu-id="1a28a-232">Docker 17.03.0 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="1a28a-232">Docker 17.03.0 and later</span></span>

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="1a28a-233">Telepítése és a megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a28a-233">Installing and configuring the solution</span></span>
<span data-ttu-id="1a28a-234">Az alábbi információk segítségével telepítse és konfigurálja a megoldást.</span><span class="sxs-lookup"><span data-stu-id="1a28a-234">Use the following information to install and configure the solution.</span></span>

1. <span data-ttu-id="1a28a-235">A tároló figyelésére szolgáló megoldás hozzáadása az OMS-munkaterület a [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) vagy ismertetett folyamatot követve [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="1a28a-235">Add the Container Monitoring solution to your OMS workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>

2. <span data-ttu-id="1a28a-236">Telepítheti és használhatja a Docker az OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="1a28a-236">Install and use Docker with an OMS agent.</span></span>  <span data-ttu-id="1a28a-237">Az operációs rendszer alapján, választhat a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="1a28a-237">Based on your operating system, you can choose from the following methods:</span></span>

  * <span data-ttu-id="1a28a-238">A támogatott Linux operációs rendszerek telepítése futtatásához Docker és telepítse és konfigurálja a [Linux OMS-ügynököt](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1a28a-238">On supported Linux operating systems, install and run Docker and then install and configure the [OMS Agent for Linux](log-analytics-agent-linux.md).</span></span>  
  * <span data-ttu-id="1a28a-239">A CoreOS Linux rendszeren futtatható, az OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="1a28a-239">On CoreOS, you cannot run the OMS Agent for Linux.</span></span> <span data-ttu-id="1a28a-240">Az OMS-ügynököt a tárolóalapú változata ehelyett Linux fusson.</span><span class="sxs-lookup"><span data-stu-id="1a28a-240">Instead, you run a containerized version of the OMS Agent for Linux.</span></span> <span data-ttu-id="1a28a-241">Felülvizsgálati [Linux tároló gazdagépek, köztük a CoreOS](#for-all-linux-container-hosts-including-coreos) vagy [Azure Government Linux tároló gazdagépek, köztük a CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) Ha Azure Government felhőalapú tárolók dolgozik.</span><span class="sxs-lookup"><span data-stu-id="1a28a-241">Review [Linux container hosts including CoreOS](#for-all-linux-container-hosts-including-coreos) or [Azure Government Linux container hosts including CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) if you are working with containers in Azure Government Cloud.</span></span>
  * <span data-ttu-id="1a28a-242">A Windows Server 2016 és a Windows 10 a Docker-motorhoz és az ügyfél telepítése, majd kösse a az ügynök információkat gyűjt, és elküldi a Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="1a28a-242">On Windows Server 2016 and Windows 10, install the Docker Engine and client then connect an agent to gather information and send it to Log Analytics.</span></span>  

### <a name="container-services"></a><span data-ttu-id="1a28a-243">Tároló szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="1a28a-243">Container services</span></span>

- <span data-ttu-id="1a28a-244">Ha a Red Hat OpenShift környezettel rendelkezik, tekintse át a [OMS-ügynököt a Red Hat OpenShift konfigurálása](#configure-an-oms-agent-for-red-hat-openshift).</span><span class="sxs-lookup"><span data-stu-id="1a28a-244">If you have a Red Hat OpenShift environment, review [Configure an OMS agent for Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).</span></span>
- <span data-ttu-id="1a28a-245">Ha az Azure Tárolószolgáltatás Kubernetes fürtöt, tekintse át a [figyelése az Azure Tárolószolgáltatás-fürtöt a Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span><span class="sxs-lookup"><span data-stu-id="1a28a-245">If you have a Kubernetes cluster using the Azure Container Service, review [Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span></span>
- <span data-ttu-id="1a28a-246">Ha egy Azure tároló szolgáltatás DC/OS-fürtről, további tudnivalókért olvassa el [figyelése az Azure tároló szolgáltatás DC/OS fürtben, az Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span><span class="sxs-lookup"><span data-stu-id="1a28a-246">If you have an Azure Container Service DC/OS cluster, learn more at [Monitor an Azure Container Service DC/OS cluster with Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span></span>
- <span data-ttu-id="1a28a-247">Ha a Docker Swarm mód környezettel rendelkezik, további tudnivalókért olvassa el [OMS-ügynököt a Docker Swarmra konfigurálása](#configure-an-oms-agent-for-docker-swarm).</span><span class="sxs-lookup"><span data-stu-id="1a28a-247">If you have a Docker Swarm mode environment, learn more at [Configure an OMS agent for Docker Swarm](#configure-an-oms-agent-for-docker-swarm).</span></span>
- <span data-ttu-id="1a28a-248">A Service Fabric tárolók használja, ha további tudnivalókért olvassa el [áttekintés az Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1a28a-248">If you use containers with Service Fabric, learn more at [Overview of Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span></span>
- <span data-ttu-id="1a28a-249">Tekintse át a [Docker-motorhoz Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) cikk telepítése és konfigurálása a Docker motorok Windows rendszerű számítógépekre vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="1a28a-249">Review the [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) article for additional information about how to install and configure your Docker Engines on computers running Windows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1a28a-250">Futnia kell az docker **előtt** telepítése a [Linux OMS-ügynököt](log-analytics-agent-linux.md) tároló gazdagépei.</span><span class="sxs-lookup"><span data-stu-id="1a28a-250">Docker must be running **before** you install the [OMS Agent for Linux](log-analytics-agent-linux.md) on your container hosts.</span></span> <span data-ttu-id="1a28a-251">Ha már telepítette az ügynököt Docker telepítése előtt, telepítse újra az OMS-ügynököt Linux szeretné.</span><span class="sxs-lookup"><span data-stu-id="1a28a-251">If you've already installed the agent before installing Docker, you need to reinstall the OMS Agent for Linux.</span></span> <span data-ttu-id="1a28a-252">További információ a Docker: a [Docker webhely](https://www.docker.com).</span><span class="sxs-lookup"><span data-stu-id="1a28a-252">For more information about Docker, see the [Docker website](https://www.docker.com).</span></span>


## <a name="linux-container-hosts"></a><span data-ttu-id="1a28a-253">Linux-tároló állomások</span><span class="sxs-lookup"><span data-stu-id="1a28a-253">Linux container hosts</span></span>

<span data-ttu-id="1a28a-254">Ha már telepítette a Docker, használja a következő beállításokat, a tároló állomás Docker használható az ügynök konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1a28a-254">After you've installed Docker, use the following settings for your container host to configure the agent for use with Docker.</span></span> <span data-ttu-id="1a28a-255">Először szükség van a OMS-munkaterület azonosítója és a kulcs, amely az Azure portálon található.</span><span class="sxs-lookup"><span data-stu-id="1a28a-255">First you need your OMS workspace ID and key, which you can find in the Azure portal.</span></span> <span data-ttu-id="1a28a-256">A munkaterületen kattintson **gyors üzembe helyezés** > **számítógépek** megtekintéséhez a **munkaterület azonosítója** és **elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="1a28a-256">In your workspace, click **Quick Start** > **Computers** to view your **Workspace ID** and **Primary Key**.</span></span>  <span data-ttu-id="1a28a-257">Másolással illessze be a kedvenc szerkesztő mindkettő.</span><span class="sxs-lookup"><span data-stu-id="1a28a-257">Copy and paste both into your favorite editor.</span></span>

### <a name="for-all-linux-container-hosts-except-coreos"></a><span data-ttu-id="1a28a-258">CoreOS kivételével az összes Linux tároló állomás</span><span class="sxs-lookup"><span data-stu-id="1a28a-258">For all Linux container hosts except CoreOS</span></span>

- <span data-ttu-id="1a28a-259">További információkat és lépéseket az OMS-ügynök telepítése Linux [csatlakozzon a Linux rendszerű számítógépek Operations Management Suite (OMS)](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1a28a-259">For more information and steps on how to install the OMS Agent for Linux, see [Connect your Linux Computers to Operations Management Suite (OMS)](log-analytics-agent-linux.md).</span></span>

### <a name="for-all-linux-container-hosts-including-coreos"></a><span data-ttu-id="1a28a-260">Az összes Linux tároló gazdagépek, köztük a CoreOS</span><span class="sxs-lookup"><span data-stu-id="1a28a-260">For all Linux container hosts including CoreOS</span></span>

<span data-ttu-id="1a28a-261">Indítsa el az OMS-tároló, amely segítségével nyomon követni kívánt.</span><span class="sxs-lookup"><span data-stu-id="1a28a-261">Start the OMS container that you want to monitor.</span></span> <span data-ttu-id="1a28a-262">Módosítsa és használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="1a28a-262">Modify and use the following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a><span data-ttu-id="1a28a-263">Minden Azure Government Linux tároló gazdagépek, köztük a CoreOS</span><span class="sxs-lookup"><span data-stu-id="1a28a-263">For all Azure Government Linux container hosts including CoreOS</span></span>

<span data-ttu-id="1a28a-264">Indítsa el az OMS-tároló, amely segítségével nyomon követni kívánt.</span><span class="sxs-lookup"><span data-stu-id="1a28a-264">Start the OMS container that you want to monitor.</span></span> <span data-ttu-id="1a28a-265">Módosítsa és használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="1a28a-265">Modify and use the following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-to-one-in-a-container"></a><span data-ttu-id="1a28a-266">Egy tároló telepített Linux ügynök használatával való váltás</span><span class="sxs-lookup"><span data-stu-id="1a28a-266">Switching from using an installed Linux agent to one in a container</span></span>
<span data-ttu-id="1a28a-267">Ha korábban a közvetlenül telepített ügynök használt, és használja a tárolóban futó ügynök szeretne, el kell távolítania az OMS-ügynököt Linux.</span><span class="sxs-lookup"><span data-stu-id="1a28a-267">If you previously used the directly-installed agent and want to instead use an agent running in a container, you must first remove the OMS Agent for Linux.</span></span> <span data-ttu-id="1a28a-268">Lásd: [az OMS-ügynök eltávolítása Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) annak megértése, hogyan sikeresen távolítsa el az ügynököt.</span><span class="sxs-lookup"><span data-stu-id="1a28a-268">See [Uninstalling the OMS Agent for Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) to understand how to successfully uninstall the agent.</span></span>  

### <a name="configure-an-oms-agent-for-docker-swarm"></a><span data-ttu-id="1a28a-269">A Docker Swarmra OMS-ügynököt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a28a-269">Configure an OMS agent for Docker Swarm</span></span>

<span data-ttu-id="1a28a-270">A Docker Swarmról az OMS-ügynököt is futhat, a globális szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1a28a-270">You can run the OMS Agent as a global service on Docker Swarm.</span></span> <span data-ttu-id="1a28a-271">Az alábbi információk használatával hozzon létre egy OMS-ügynököt szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1a28a-271">Use the following information to create an OMS Agent service.</span></span> <span data-ttu-id="1a28a-272">Helyezze be az OMS-munkaterület azonosítója és az elsődleges kulcs van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1a28a-272">You need to insert your OMS Workspace ID and Primary Key.</span></span>

- <span data-ttu-id="1a28a-273">Futtassa a következő fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="1a28a-273">Run the following on the master node.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a><span data-ttu-id="1a28a-274">Red Hat OpenShift az OMS-ügynököt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a28a-274">Configure an OMS Agent for Red Hat OpenShift</span></span>
<span data-ttu-id="1a28a-275">Nincsenek három módszerrel adhat hozzá az OMS-ügynököt Red Hat OpenShift tároló figyelési adatok gyűjtése elindításához.</span><span class="sxs-lookup"><span data-stu-id="1a28a-275">There are three ways to add the OMS Agent to Red Hat OpenShift to start collecting container monitoring data.</span></span>

* <span data-ttu-id="1a28a-276">[Az OMS-ügynök telepítése Linux](log-analytics-agent-linux.md) közvetlenül a OpenShift csomópontokon</span><span class="sxs-lookup"><span data-stu-id="1a28a-276">[Install the OMS Agent for Linux](log-analytics-agent-linux.md) directly on each OpenShift node</span></span>  
* <span data-ttu-id="1a28a-277">[Napló Analytics Virtuálisgép-bővítmény engedélyezése](log-analytics-azure-vm-extension.md) minden OpenShift csomóponton található az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="1a28a-277">[Enable Log Analytics VM Extension](log-analytics-azure-vm-extension.md) on each OpenShift node residing in Azure</span></span>  
* <span data-ttu-id="1a28a-278">Az OMS-ügynököt telepíteni készletként OpenShift démon-</span><span class="sxs-lookup"><span data-stu-id="1a28a-278">Install the OMS Agent as a OpenShift daemon-set</span></span>  

<span data-ttu-id="1a28a-279">Ebben a szakaszban foglalkozunk azt egy OpenShift démon beállított az OMS-ügynök telepítéséhez szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="1a28a-279">In this section we cover the steps required to install the OMS Agent as an OpenShift daemon-set.</span></span>  

1. <span data-ttu-id="1a28a-280">Jelentkezzen be a OpenShift fő csomópont, és másolja az yam [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) a Githubból a fő csomópontra, és módosítsa az OMS-munkaterület azonosítója és az elsődleges kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="1a28a-280">Sign on to the OpenShift master node and copy the yaml file [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) from GitHub to your master node and modify the value with your OMS Workspace ID and with your Primary Key.</span></span>
2. <span data-ttu-id="1a28a-281">A következő parancsokat az OMS-projekt létrehozása és beállítása a felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="1a28a-281">Run the following commands to create a project for OMS and set the user account.</span></span>

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="1a28a-282">A szűrődémon-set telepítéséhez futtassa a következő:</span><span class="sxs-lookup"><span data-stu-id="1a28a-282">To deploy the daemon-set, run the following:</span></span>

    `oc create -f ocp-omsagent.yaml`

5. <span data-ttu-id="1a28a-283">Ellenőrizze, hogy van-e konfigurálva, és megfelelően működik, írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="1a28a-283">To verify it is configured and working correctly, type the following:</span></span>

    `oc describe daemonset omsagent`  

    <span data-ttu-id="1a28a-284">és a kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="1a28a-284">and the output should resemble:</span></span>

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

<span data-ttu-id="1a28a-285">Ha szeretné védeni kell az OMS-munkaterület azonosítója és az elsődleges kulcs, amikor az OMS-ügynököt démon-set yam fájl használata a titkos kulcsok segítségével, a következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="1a28a-285">If you want to use secrets to secure your OMS Workspace ID and Primary Key when using the OMS Agent daemon-set yaml file, perform the following steps.</span></span>

1. <span data-ttu-id="1a28a-286">Jelentkezzen be a OpenShift fő csomópont, és másolja az yam [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) és a titkos kulcs parancsfájljának [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) a Githubról.</span><span class="sxs-lookup"><span data-stu-id="1a28a-286">Sign on to the OpenShift master node and copy the yaml file [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) and secret generating script [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) from GitHub.</span></span>  <span data-ttu-id="1a28a-287">Ezt a parancsfájlt hoz létre a titkos kulcsok yam fájlt az OMS-munkaterület azonosítója és az elsődleges kulcs biztonságossá tételéhez a secrete információkat.</span><span class="sxs-lookup"><span data-stu-id="1a28a-287">This script will generate the secrets yaml file for OMS Workspace ID and Primary Key to secure your secrete information.</span></span>  
2. <span data-ttu-id="1a28a-288">A következő parancsokat az OMS-projekt létrehozása és beállítása a felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="1a28a-288">Run the following commands to create a project for OMS and set the user account.</span></span> <span data-ttu-id="1a28a-289">A titkos kulcs parancsfájljának kér az OMS-munkaterület azonosítója <WSID> és az elsődleges kulcs <KEY> és befejezésekor a ocp-secret.yaml fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1a28a-289">The secret generating script asks for your OMS Workspace ID <WSID> and Primary Key <KEY> and upon completion, it creates the ocp-secret.yaml file.</span></span>  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="1a28a-290">A titkos fájl központi telepítése a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1a28a-290">Deploy the secret file by running the following:</span></span>

    `oc create -f ocp-secret.yaml`

5. <span data-ttu-id="1a28a-291">Telepítés ellenőrzése a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1a28a-291">Verify deployment by running the following:</span></span>

    `oc describe secret omsagent-secret`  

    <span data-ttu-id="1a28a-292">és a kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="1a28a-292">and the  output should resemble:</span></span>  

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

6. <span data-ttu-id="1a28a-293">Az OMS-ügynököt démon-set yam fájl központi telepítése a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1a28a-293">Deploy the OMS Agent daemon-set yaml file by running the following:</span></span>

    `oc create -f ocp-ds-omsagent.yaml`  

7. <span data-ttu-id="1a28a-294">Telepítés ellenőrzése a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1a28a-294">Verify deployment by running the following:</span></span>

    `oc describe ds oms`

    <span data-ttu-id="1a28a-295">és a kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="1a28a-295">and the output should resemble:</span></span>

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

### <a name="secure-your-secret-information-for-docker-swarm-and-kubernetes"></a><span data-ttu-id="1a28a-296">A Docker Swarm és Kubernetes titkos adatok védelméhez</span><span class="sxs-lookup"><span data-stu-id="1a28a-296">Secure your secret information for Docker Swarm and Kubernetes</span></span>

<span data-ttu-id="1a28a-297">A titkos OMS-munkaterület azonosítója és az elsődleges kulcsok Docker Swarm-és tárolószolgáltatások Kubernetes biztonságát.</span><span class="sxs-lookup"><span data-stu-id="1a28a-297">You can secure your secret OMS Workspace ID and Primary Keys for Docker Swarm and Kubernetes container services.</span></span>

#### <a name="secure-secrets-for-docker-swarm"></a><span data-ttu-id="1a28a-298">Biztonságos kulcsok Docker Swarm esetén</span><span class="sxs-lookup"><span data-stu-id="1a28a-298">Secure secrets for Docker Swarm</span></span>

<span data-ttu-id="1a28a-299">A Docker Swarmra a titkos kulcsot a munkaterület azonosítója és az elsődleges kulcs létrehozása után is futtathatja a létrehozás OMSagent a Docker szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="1a28a-299">For Docker Swarm, once the secret for Workspace ID and Primary Key is created, you can run the create the Docker service for OMSagent.</span></span> <span data-ttu-id="1a28a-300">Az alábbi információk használatával hozzon létre a titkos adatokat.</span><span class="sxs-lookup"><span data-stu-id="1a28a-300">Use the following information to create your secret information.</span></span>

1. <span data-ttu-id="1a28a-301">Futtassa a következő fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="1a28a-301">Run the following on the master node.</span></span>

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. <span data-ttu-id="1a28a-302">Győződjön meg arról, hogy a titkos kulcsok megfelelően hozta létre.</span><span class="sxs-lookup"><span data-stu-id="1a28a-302">Verify that secrets were created properly.</span></span>

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. <span data-ttu-id="1a28a-303">A következő parancsot a titkos kulcsok számára a tárolóalapú OMS-ügynököt csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="1a28a-303">Run the following command to mount the secrets to the containerized OMS Agent.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a><span data-ttu-id="1a28a-304">Biztonságos titkot Kubernetes yam-fájlokkal</span><span class="sxs-lookup"><span data-stu-id="1a28a-304">Secure secrets for Kubernetes with yaml files</span></span>

<span data-ttu-id="1a28a-305">Kubernetes, a parancsfájl hozza létre a titkos kulcsok yam fájlt a munkaterület azonosítója és az elsődleges kulcs használata.</span><span class="sxs-lookup"><span data-stu-id="1a28a-305">For Kubernetes, you use a script to generate the secrets yaml file for your Workspace ID and Primary Key.</span></span> <span data-ttu-id="1a28a-306">: A [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) lapon, a fájl vagy a titkos adatait anélkül is használhatja.</span><span class="sxs-lookup"><span data-stu-id="1a28a-306">At the [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) page, there are files that you can use with or without your secret information.</span></span>

- <span data-ttu-id="1a28a-307">Az OMS-ügynök alapértelmezett DaemonSet nem rendelkezik titkos információk (omsagent.yaml)</span><span class="sxs-lookup"><span data-stu-id="1a28a-307">The Default OMS Agent DaemonSet does not have secret information (omsagent.yaml)</span></span>
- <span data-ttu-id="1a28a-308">Az OMS-ügynök DaemonSet yam fájl titkos generációs parancsfájlok titkos információk (omsagent – ds-secrets.yaml) segítségével hozza létre a titkos kulcsok yam (omsagentsecret.yaml) fájlt.</span><span class="sxs-lookup"><span data-stu-id="1a28a-308">The OMS Agent DaemonSet yaml file uses secret information (omsagent-ds-secrets.yaml) with secret generation scripts to generate the secrets yaml (omsagentsecret.yaml) file.</span></span>

<span data-ttu-id="1a28a-309">Ha szeretné omsagent DaemonSets létrehozása, vagy a titkos kulcsok nélkül.</span><span class="sxs-lookup"><span data-stu-id="1a28a-309">You can choose to create omsagent DaemonSets with or without secrets.</span></span>

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a><span data-ttu-id="1a28a-310">Alapértelmezett OMSagent DaemonSet yam fájl titkos kulcsok nélkül</span><span class="sxs-lookup"><span data-stu-id="1a28a-310">Default OMSagent DaemonSet yaml file without secrets</span></span>

- <span data-ttu-id="1a28a-311">Az alapértelmezett OMS-ügynök DaemonSet yam fájl cserélje le a `<WSID>` és `<KEY>` a WSID és a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="1a28a-311">For the default OMS Agent DaemonSet yaml file, replace the `<WSID>` and `<KEY>` to your WSID and KEY.</span></span> <span data-ttu-id="1a28a-312">A fájl átmásolása a fő csomópont, és futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="1a28a-312">Copy the file to your master node and run the following:</span></span>

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a><span data-ttu-id="1a28a-313">Alapértelmezett OMSagent DaemonSet yam fájl titkos kulcsok</span><span class="sxs-lookup"><span data-stu-id="1a28a-313">Default OMSagent DaemonSet yaml file with secrets</span></span>

1. <span data-ttu-id="1a28a-314">Titkos információk alapján OMS-ügynök DaemonSet használatához először hozza létre a titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="1a28a-314">To use OMS Agent DaemonSet using secret information, create the secrets first.</span></span>
    1. <span data-ttu-id="1a28a-315">Másolja a parancsfájlt és a titkos sablonfájl, és győződjön meg arról, ezek is ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="1a28a-315">Copy the script and secret template file and make sure they are on the same directory.</span></span>
        - <span data-ttu-id="1a28a-316">Titkos kulcs parancsprogram - secret-gen.sh létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a28a-316">Secret generating script - secret-gen.sh</span></span>
        - <span data-ttu-id="1a28a-317">titkos sablon - secret-template.yaml</span><span class="sxs-lookup"><span data-stu-id="1a28a-317">secret template - secret-template.yaml</span></span>
    2. <span data-ttu-id="1a28a-318">Futtassa a parancsfájlt, az alábbi példához hasonló.</span><span class="sxs-lookup"><span data-stu-id="1a28a-318">Run the script, like the following example.</span></span> <span data-ttu-id="1a28a-319">A parancsprogram kéri az OMS-munkaterület azonosítója és az elsődleges kulcs, és azokat megadása után a parancsfájl létrehoz egy titkos yam ezért is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="1a28a-319">The script asks for the OMS Workspace ID and Primary Key and after you enter them, the script creates a secret yaml file so you can run it.</span></span>   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. <span data-ttu-id="1a28a-320">Hozza létre a titkos kulcsok fogyasztanak a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1a28a-320">Create the secrets pod by running the following:</span></span>
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. <span data-ttu-id="1a28a-321">Győződjön meg arról, hogy futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="1a28a-321">To verify, run the following:</span></span>

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        <span data-ttu-id="1a28a-322">Kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="1a28a-322">Output should resemble:</span></span>

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        <span data-ttu-id="1a28a-323">Kimeneti kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="1a28a-323">Output should resemble:</span></span>

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

    5. <span data-ttu-id="1a28a-324">A omsagent futó démon-készlet létrehozása``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="1a28a-324">Create your omsagent daemon-set by running ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

2. <span data-ttu-id="1a28a-325">Győződjön meg arról, hogy az OMS-ügynök DaemonSet fut, a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="1a28a-325">Verify that the OMS Agent DaemonSet is running, similar to the following:</span></span>

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


<span data-ttu-id="1a28a-326">Kubernetes, a parancsfájl segítségével hozza létre a titkos kulcsok yam fájlt a munkaterület azonosítója és az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="1a28a-326">For Kubernetes, use a script to generate the secrets yaml file for Workspace ID and Primary Key.</span></span> <span data-ttu-id="1a28a-327">A következő példa információk a [omsagent yam fájl](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) a titkos adatok védelméhez.</span><span class="sxs-lookup"><span data-stu-id="1a28a-327">Use the following example information with the [omsagent yaml file](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) to secure your secret information.</span></span>

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

## <a name="windows-container-hosts"></a><span data-ttu-id="1a28a-328">Windows-tároló gazdagépek</span><span class="sxs-lookup"><span data-stu-id="1a28a-328">Windows container hosts</span></span>

### <a name="preparation-before-installing-windows-agents"></a><span data-ttu-id="1a28a-329">Windows-ügynökök telepítése előtt előkészítése</span><span class="sxs-lookup"><span data-stu-id="1a28a-329">Preparation before installing Windows agents</span></span>

<span data-ttu-id="1a28a-330">Ügynökök telepítése Windows rendszerű számítógépekre, előtt kell beállítani a Docker-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1a28a-330">Before you install agents on computers running Windows, you need to configure the Docker service.</span></span> <span data-ttu-id="1a28a-331">A konfiguráció lehetővé teszi, hogy a Windows-ügynök vagy a Naplóelemzési virtuálisgép-bővítmény, hogy az ügynökök távolról elérjék a Docker démon a Docker TCP szoftvercsatorna használandó és a figyelési adatok rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1a28a-331">The configuration allows the Windows agent or the Log Analytics virtual machine extension to use the Docker TCP socket so that the agents can access the Docker daemon remotely and to capture data for monitoring.</span></span>

#### <a name="to-start-docker-and-verify-its-configuration"></a><span data-ttu-id="1a28a-332">Indítsa el a Docker és a konfiguráció ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="1a28a-332">To start Docker and verify its configuration</span></span>

<span data-ttu-id="1a28a-333">A Windows Server nevesített cső TCP beállításához szükséges lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="1a28a-333">There are steps needed to set up TCP named pipe for Windows Server:</span></span>

1. <span data-ttu-id="1a28a-334">A Windows PowerShellben engedélyezze a TCP cső és a nevesített cső.</span><span class="sxs-lookup"><span data-stu-id="1a28a-334">In Windows PowerShell, enable TCP pipe and named pipe.</span></span>

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. <span data-ttu-id="1a28a-335">A konfigurációs fájl TCP pipe Docker konfigurálásához, valamint a nevesített cső.</span><span class="sxs-lookup"><span data-stu-id="1a28a-335">Configure Docker with the configuration file for TCP pipe and named pipe.</span></span> <span data-ttu-id="1a28a-336">A konfigurációs fájl nem található: C:\ProgramData\docker\config\daemon.json.</span><span class="sxs-lookup"><span data-stu-id="1a28a-336">The configuration file is located at C:\ProgramData\docker\config\daemon.json.</span></span>

    <span data-ttu-id="1a28a-337">A daemon.json fájlban a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="1a28a-337">In the daemon.json file, you will need the following:</span></span>

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

<span data-ttu-id="1a28a-338">Windows tárolók használt Docker démon konfigurációjával kapcsolatos további információkért lásd: [Docker-motorhoz Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span><span class="sxs-lookup"><span data-stu-id="1a28a-338">For more information about the Docker daemon configuration used with Windows Containers, see [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span></span>


### <a name="install-windows-agents"></a><span data-ttu-id="1a28a-339">Windows-ügynökök telepítése</span><span class="sxs-lookup"><span data-stu-id="1a28a-339">Install Windows agents</span></span>

<span data-ttu-id="1a28a-340">Windows és a Hyper-V tároló figyelés engedélyezéséhez telepítse a Microsoft Monitoring Agent (MMA) Windows rendszerű számítógépeken, amelyek tároló gazdagépek.</span><span class="sxs-lookup"><span data-stu-id="1a28a-340">To enable Windows and Hyper-V container monitoring, install the Microsoft Monitoring Agent (MMA) on Windows computers that are container hosts.</span></span> <span data-ttu-id="1a28a-341">A helyszíni környezetben futó Windows-számítógépek esetén lásd: [Log Analyticshez való csatlakozás Windows számítógépek](log-analytics-windows-agents.md).</span><span class="sxs-lookup"><span data-stu-id="1a28a-341">For computers running Windows in your on-premises environment, see [Connect Windows computers to Log Analytics](log-analytics-windows-agents.md).</span></span> <span data-ttu-id="1a28a-342">A virtuális gépek Azure-ban futó csatlakoztassa őket Naplóelemzési használatával a [virtuálisgép-bővítmény](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="1a28a-342">For virtual machines running in Azure, connect them to Log Analytics using the [virtual machine extension](log-analytics-azure-vm-extension.md).</span></span>

<span data-ttu-id="1a28a-343">A Service Fabric futó Windows-tárolók figyelheti.</span><span class="sxs-lookup"><span data-stu-id="1a28a-343">You can monitor Windows containers running on Service Fabric.</span></span> <span data-ttu-id="1a28a-344">Azonban csak [az Azure-ban futó virtuális gépek](log-analytics-azure-vm-extension.md) és [a helyszíni környezetben a Windows rendszert futtató számítógépek](log-analytics-windows-agents.md) jelenleg a Service Fabric támogatottak.</span><span class="sxs-lookup"><span data-stu-id="1a28a-344">However, only [virtual machines running in Azure](log-analytics-azure-vm-extension.md) and [computers running Windows in your on-premises environment](log-analytics-windows-agents.md) are currently supported for Service Fabric.</span></span>

<span data-ttu-id="1a28a-345">Ellenőrizheti, hogy a tároló figyelésére szolgáló megoldás helyesen van megadva a Windows.</span><span class="sxs-lookup"><span data-stu-id="1a28a-345">You can verify that the Container Monitoring solution is set correctly for Windows.</span></span> <span data-ttu-id="1a28a-346">Ellenőrizze, hogy a felügyeleti csomag megfelelő volt-e letöltésre, keressen *ContainerManagement.xxx*.</span><span class="sxs-lookup"><span data-stu-id="1a28a-346">To check whether the management pack was download properly, look for *ContainerManagement.xxx*.</span></span> <span data-ttu-id="1a28a-347">A fájlok a C:\Program Files\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok mappában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1a28a-347">The files should be in the C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs folder.</span></span>


## <a name="solution-components"></a><span data-ttu-id="1a28a-348">Megoldás-összetevők</span><span class="sxs-lookup"><span data-stu-id="1a28a-348">Solution components</span></span>

<span data-ttu-id="1a28a-349">Ha Windows-ügynökök használ, akkor a következő felügyeleti csomag telepítve van minden számítógépen, amelyen ügynök ebben a megoldásban hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="1a28a-349">If you are using Windows agents, then the following management pack is installed on each computer with an agent when you add this solution.</span></span> <span data-ttu-id="1a28a-350">Nincs konfigurációs és karbantartási szükség a felügyeleti csomag.</span><span class="sxs-lookup"><span data-stu-id="1a28a-350">No configuration or maintenance is required for the management pack.</span></span>

- <span data-ttu-id="1a28a-351">*ContainerManagement.xxx* telepítve a C:\Program Files\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok</span><span class="sxs-lookup"><span data-stu-id="1a28a-351">*ContainerManagement.xxx* installed in C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs</span></span>

## <a name="container-data-collection-details"></a><span data-ttu-id="1a28a-352">Tároló az gyűjtemény adatait</span><span class="sxs-lookup"><span data-stu-id="1a28a-352">Container data collection details</span></span>
<span data-ttu-id="1a28a-353">A tároló figyelésére szolgáló megoldás különböző metrikák és a napló teljesítményadatokat gyűjt tároló állomások és a tárolók ügynököt, amely engedélyezi a használ.</span><span class="sxs-lookup"><span data-stu-id="1a28a-353">The Container Monitoring solution collects various performance metrics and log data from container hosts and containers using agents that you enable.</span></span>

<span data-ttu-id="1a28a-354">Adatgyűjtés három percenként a következő ügynök típusok.</span><span class="sxs-lookup"><span data-stu-id="1a28a-354">Data is collected every three minutes by the following agent types.</span></span>

- [<span data-ttu-id="1a28a-355">Linux OMS-ügynök</span><span class="sxs-lookup"><span data-stu-id="1a28a-355">OMS Agent for Linux</span></span>](log-analytics-linux-agents.md)
- [<span data-ttu-id="1a28a-356">Windows-ügynök</span><span class="sxs-lookup"><span data-stu-id="1a28a-356">Windows agent</span></span>](log-analytics-windows-agents.md)
- [<span data-ttu-id="1a28a-357">Napló Analytics Virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="1a28a-357">Log Analytics VM extension</span></span>](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a><span data-ttu-id="1a28a-358">Tárolórekordok</span><span class="sxs-lookup"><span data-stu-id="1a28a-358">Container records</span></span>

<span data-ttu-id="1a28a-359">Az alábbi táblázat a tároló figyelésére szolgáló megoldás és az adatok, amelyek szerepelnek a találatok között napló által gyűjtött rekordok példát.</span><span class="sxs-lookup"><span data-stu-id="1a28a-359">The following table shows examples of records collected by the Container Monitoring solution and the data types that appear in log search results.</span></span>

| <span data-ttu-id="1a28a-360">Adattípus</span><span class="sxs-lookup"><span data-stu-id="1a28a-360">Data type</span></span> | <span data-ttu-id="1a28a-361">A naplófájl-keresési adattípust tartalmaz</span><span class="sxs-lookup"><span data-stu-id="1a28a-361">Data type in Log Search</span></span> | <span data-ttu-id="1a28a-362">Mezők</span><span class="sxs-lookup"><span data-stu-id="1a28a-362">Fields</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1a28a-363">A gazdagépek és a tárolók teljesítmény</span><span class="sxs-lookup"><span data-stu-id="1a28a-363">Performance for hosts and containers</span></span> | `Type=Perf` | <span data-ttu-id="1a28a-364">Számítógép, ObjectName, CounterName &#40; a processzor kihasználtsága, lemez beolvassa MB, szabad MB memória kihasználtsága (MB), írja hálózati fogadott bájtok, hálózati küldését bájt, a processzor kihasználtsága mp hálózati &#41; ellenértéknek, TimeGenerated, Számláló_elérési_útja, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="1a28a-364">Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue,TimeGenerated, CounterPath, SourceSystem</span></span> |
| <span data-ttu-id="1a28a-365">Tároló leltár</span><span class="sxs-lookup"><span data-stu-id="1a28a-365">Container inventory</span></span> | `Type=ContainerInventory` | <span data-ttu-id="1a28a-366">TimeGenerated, a számítógép, a tároló neve, ContainerHostname, kép, ImageTag, ContainerState, ExitCode, EnvironmentVar, a parancs, CreatedTime, StartedTime, FinishedTime, SourceSystem, Tárolóazonosító, ImageID</span><span class="sxs-lookup"><span data-stu-id="1a28a-366">TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID</span></span> |
| <span data-ttu-id="1a28a-367">Tároló kép készlet</span><span class="sxs-lookup"><span data-stu-id="1a28a-367">Container image inventory</span></span> | `Type=ContainerImageInventory` | <span data-ttu-id="1a28a-368">TimeGenerated, számítógép, kép, ImageTag, ImageSize, VirtualSize, futó, szünetel, leállítását követően nem sikerült, SourceSystem, ImageID, TotalContainer</span><span class="sxs-lookup"><span data-stu-id="1a28a-368">TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer</span></span> |
| <span data-ttu-id="1a28a-369">Tároló-napló</span><span class="sxs-lookup"><span data-stu-id="1a28a-369">Container log</span></span> | `Type=ContainerLog` | <span data-ttu-id="1a28a-370">TimeGenerated, a számítógép, a lemezkép-Azonosítót, a tároló neve, LogEntrySource, LogEntry, SourceSystem, Tárolóazonosító</span><span class="sxs-lookup"><span data-stu-id="1a28a-370">TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="1a28a-371">Tároló szolgáltatás bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="1a28a-371">Container service log</span></span> | `Type=ContainerServiceLog`  | <span data-ttu-id="1a28a-372">TimeGenerated, számítógép, TimeOfCommand, kép, a parancs, SourceSystem, Tárolóazonosító</span><span class="sxs-lookup"><span data-stu-id="1a28a-372">TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="1a28a-373">Tároló csomópont készlet</span><span class="sxs-lookup"><span data-stu-id="1a28a-373">Container node inventory</span></span> | `Type=ContainerNodeInventory_CL`| <span data-ttu-id="1a28a-374">TimeGenerated, számítógép, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="1a28a-374">TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span></span>|
| <span data-ttu-id="1a28a-375">Kubernetes készlet</span><span class="sxs-lookup"><span data-stu-id="1a28a-375">Kubernetes inventory</span></span> | `Type=KubePodInventory_CL` | <span data-ttu-id="1a28a-376">TimeGenerated, számítógép, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="1a28a-376">TimeGenerated, Computer, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span></span> |
| <span data-ttu-id="1a28a-377">Tároló folyamat</span><span class="sxs-lookup"><span data-stu-id="1a28a-377">Container process</span></span> | `Type=ContainerProcess_CL` | <span data-ttu-id="1a28a-378">TimeGenerated, számítógép, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="1a28a-378">TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span></span> |
| <span data-ttu-id="1a28a-379">Kubernetes események</span><span class="sxs-lookup"><span data-stu-id="1a28a-379">Kubernetes events</span></span> | `Type=KubeEvents_CL` | <span data-ttu-id="1a28a-380">TimeGenerated, számítógép, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, üzenet</span><span class="sxs-lookup"><span data-stu-id="1a28a-380">TimeGenerated, Computer, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, Message</span></span> |

<span data-ttu-id="1a28a-381">Címkék fűzött *PodLabel* adattípusok a következők saját címkét.</span><span class="sxs-lookup"><span data-stu-id="1a28a-381">Labels appended to *PodLabel* data types are your own custom labels.</span></span> <span data-ttu-id="1a28a-382">A hozzáfűzött PodLabel címkék a táblázatban szereplő példák.</span><span class="sxs-lookup"><span data-stu-id="1a28a-382">The appended PodLabel labels shown in the table are examples.</span></span> <span data-ttu-id="1a28a-383">Igen `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` lesznek a környezet adatkészlet különböznek és általános hasonlítanak `PodLabel_yourlabel_s`.</span><span class="sxs-lookup"><span data-stu-id="1a28a-383">So, `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` will differ in your environment's data set and generically resemble `PodLabel_yourlabel_s`.</span></span>


## <a name="monitor-containers"></a><span data-ttu-id="1a28a-384">A figyelő tárolók</span><span class="sxs-lookup"><span data-stu-id="1a28a-384">Monitor containers</span></span>
<span data-ttu-id="1a28a-385">Miután az OMS-portálon, a megoldás a **tárolók** csempe a tároló gazdagépek és a tárolók az állomáson fut összegző információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1a28a-385">After you have the solution enabled in the OMS portal, the **Containers** tile shows summary information about your container hosts and the containers running in hosts.</span></span>

![Tárolók csempe](./media/log-analytics-containers/containers-title.png)

<span data-ttu-id="1a28a-387">A csempén áttekintheti hány tárolók van a környezetben, és hogy azok még nem sikerült, fut vagy leállt.</span><span class="sxs-lookup"><span data-stu-id="1a28a-387">The tile shows an overview of how many containers you have in the environment and whether they're failed, running, or stopped.</span></span>

### <a name="using-the-containers-dashboard"></a><span data-ttu-id="1a28a-388">A tárolók irányítópult használata</span><span class="sxs-lookup"><span data-stu-id="1a28a-388">Using the Containers dashboard</span></span>
<span data-ttu-id="1a28a-389">Kattintson a **tárolók** csempére.</span><span class="sxs-lookup"><span data-stu-id="1a28a-389">Click the **Containers** tile.</span></span> <span data-ttu-id="1a28a-390">Ott látni fogja a nézetek szerint vannak rendszerezve:</span><span class="sxs-lookup"><span data-stu-id="1a28a-390">From there you'll see views organized by:</span></span>

- <span data-ttu-id="1a28a-391">**Tároló események** -tároló és a sikertelen tárolók rendelkező számítógépeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1a28a-391">**Container Events** - Shows container status and computers with failed containers.</span></span>
- <span data-ttu-id="1a28a-392">**Tároló naplók** -tároló naplófájlok idő és a számítógépeknek a listáját, a naplófájlok maximális számú generált diagramot ábrázol.</span><span class="sxs-lookup"><span data-stu-id="1a28a-392">**Container Logs** - Shows a chart of container log files generated over time and a list of computers with the highest number of log files.</span></span>
- <span data-ttu-id="1a28a-393">**Kubernetes események** -Kubernetes eseményeit idő és a okok miért három munkaállomás-csoporttal generált események listája látható.</span><span class="sxs-lookup"><span data-stu-id="1a28a-393">**Kubernetes Events** - Shows a chart of Kubernetes events generated over time and a list of the reasons why pods generated the events.</span></span> <span data-ttu-id="1a28a-394">*Az adatkészlet csak Linux környezetben használatos.*</span><span class="sxs-lookup"><span data-stu-id="1a28a-394">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="1a28a-395">**Kubernetes Namespace készlet** - névterek és három munkaállomás-csoporttal számát jeleníti meg, és megjeleníti a hierarchiában.</span><span class="sxs-lookup"><span data-stu-id="1a28a-395">**Kubernetes Namespace Inventory** - Shows the number of namespaces and pods and shows their hierarchy.</span></span> <span data-ttu-id="1a28a-396">*Az adatkészlet csak Linux környezetben használatos.*</span><span class="sxs-lookup"><span data-stu-id="1a28a-396">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="1a28a-397">**Tároló csomópont készlet** -tároló csomópontok/állomáson használt vezénylési típusok számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1a28a-397">**Container Node Inventory** - Shows the number of orchestration types used on container nodes/hosts.</span></span> <span data-ttu-id="1a28a-398">A csomópontok vagy a számítógépen is a tárolók száma alapján vannak listázva.</span><span class="sxs-lookup"><span data-stu-id="1a28a-398">The computer nodes/hosts are also listed by the number of containers.</span></span> <span data-ttu-id="1a28a-399">*Az adatkészlet csak Linux környezetben használatos.*</span><span class="sxs-lookup"><span data-stu-id="1a28a-399">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="1a28a-400">**Tároló képek készlet** -tároló rendszerképek használt teljes száma és típusú kép számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1a28a-400">**Container Images Inventory** - Shows the total number of container images used and number of image types.</span></span> <span data-ttu-id="1a28a-401">A lemezképek számát is a képcímke alapján vannak listázva.</span><span class="sxs-lookup"><span data-stu-id="1a28a-401">The number of images are also listed by the image tag.</span></span>
- <span data-ttu-id="1a28a-402">**Tárolók állapot** -csomópontok és a gazdagép számítógépek, amelyeken a futó tárolók tároló teljes számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1a28a-402">**Containers Status** - Shows the total number of container nodes/host computers that have running containers.</span></span> <span data-ttu-id="1a28a-403">Számítógépek is futtató gazdagépek száma alapján vannak listázva.</span><span class="sxs-lookup"><span data-stu-id="1a28a-403">Computers are also listed by the number of running hosts.</span></span>
- <span data-ttu-id="1a28a-404">**Tároló folyamat** -tároló folyamatok az állomásokon futó sor látható.</span><span class="sxs-lookup"><span data-stu-id="1a28a-404">**Container Process** - Shows a line chart of container processes running over time.</span></span> <span data-ttu-id="1a28a-405">Tárolók is találhatók futtatásával parancs/folyamat tárolók belül.</span><span class="sxs-lookup"><span data-stu-id="1a28a-405">Containers are also listed by running command/process within containers.</span></span> <span data-ttu-id="1a28a-406">*Az adatkészlet csak Linux környezetben használatos.*</span><span class="sxs-lookup"><span data-stu-id="1a28a-406">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="1a28a-407">**Tároló processzorteljesítmény** -sor látható a számítógép-csomópont/gazdagépek időbeli átlagos CPU-használatot.</span><span class="sxs-lookup"><span data-stu-id="1a28a-407">**Container CPU Performance** - Shows a line chart of the average CPU utilization over time for computer nodes/hosts.</span></span> <span data-ttu-id="1a28a-408">Is a számítógép-csomópont/gazdagépek listák alapuló CPU-felhasználást.</span><span class="sxs-lookup"><span data-stu-id="1a28a-408">Also lists the computer nodes/hosts based on average CPU utilization.</span></span>
- <span data-ttu-id="1a28a-409">**Tároló memóriateljesítmény** -memóriahasználatának vonaldiagram időbeli változását ábrázolja.</span><span class="sxs-lookup"><span data-stu-id="1a28a-409">**Container Memory Performance** - Shows a line chart of memory usage over time.</span></span> <span data-ttu-id="1a28a-410">Számítógép memória kihasználtsága a példány neve alapján is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1a28a-410">Also lists computer memory utilization based on instance name.</span></span>
- <span data-ttu-id="1a28a-411">**A számítógép teljesítménye** -időbeli változását ábrázolja az idő múlásával processzorteljesítmény százaléka vonaldiagramok memóriahasználat idő és a megabájtokban kifejezett szabad lemezterület százalékában.</span><span class="sxs-lookup"><span data-stu-id="1a28a-411">**Computer Performance** - Shows line charts of the percent of CPU performance over time, percent of memory usage over time, and megabytes of free disk space over time.</span></span> <span data-ttu-id="1a28a-412">Minden sor egy diagram megtekintheti annak további részleteit is mutat.</span><span class="sxs-lookup"><span data-stu-id="1a28a-412">You can hover over any line in a chart to view more details.</span></span>


<span data-ttu-id="1a28a-413">Az irányítópult területenként a Keresés a gyűjtött adatok futó vizuális ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1a28a-413">Each area of the dashboard is a visual representation of a search that is run on collected data.</span></span>

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash01.png)

![Tárolók irányítópult](./media/log-analytics-containers/containers-dash02.png)

<span data-ttu-id="1a28a-416">Az a **tároló** területen kattintson a felső terület alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="1a28a-416">In the **Container Status** area, click the top area, as shown below.</span></span>

![Tárolók állapota](./media/log-analytics-containers/containers-status.png)

<span data-ttu-id="1a28a-418">Naplófájl-keresési jelenik meg, amelyen a tárolók állapotára vonatkozó adatokat.</span><span class="sxs-lookup"><span data-stu-id="1a28a-418">Log Search opens, displaying information about the state of your containers.</span></span>

![Tárolók napló keresése](./media/log-analytics-containers/containers-log-search.png)

<span data-ttu-id="1a28a-420">Itt szerkesztheti a keresési lekérdezést úgy, hogy megtalálhatók a keresett információk módosításához kíváncsiak vagyunk.</span><span class="sxs-lookup"><span data-stu-id="1a28a-420">From here, you can edit the search query to modify it to find the specific information you're interested in.</span></span> <span data-ttu-id="1a28a-421">Napló keresések kapcsolatos további információkért lásd: [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="1a28a-421">For more information about Log Searches, see [Log searches in Log Analytics](log-analytics-log-searches.md).</span></span>

## <a name="troubleshoot-by-finding-a-failed-container"></a><span data-ttu-id="1a28a-422">Keresse meg a sikertelen tároló hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="1a28a-422">Troubleshoot by finding a failed container</span></span>

<span data-ttu-id="1a28a-423">A Naplóelemzési jelöli meg a tárolóban **sikertelen** Ha egy nem nulla kilépési kóddal kilépett.</span><span class="sxs-lookup"><span data-stu-id="1a28a-423">Log Analytics marks a container as **Failed** if it has exited with a non-zero exit code.</span></span> <span data-ttu-id="1a28a-424">A hibák és a környezet hibáinak áttekintését láthatja a **sikertelen tárolók** területen.</span><span class="sxs-lookup"><span data-stu-id="1a28a-424">You can see an overview of the errors and failures in the environment in the **Failed Containers** area.</span></span>

### <a name="to-find-failed-containers"></a><span data-ttu-id="1a28a-425">Nem sikerült tárolók kereséséhez</span><span class="sxs-lookup"><span data-stu-id="1a28a-425">To find failed containers</span></span>
1. <span data-ttu-id="1a28a-426">Kattintson a **tároló** területen.</span><span class="sxs-lookup"><span data-stu-id="1a28a-426">Click the **Container Status** area.</span></span>  
   <span data-ttu-id="1a28a-427">![tárolók állapota](./media/log-analytics-containers/containers-status.png)</span><span class="sxs-lookup"><span data-stu-id="1a28a-427">![containers status](./media/log-analytics-containers/containers-status.png)</span></span>
2. <span data-ttu-id="1a28a-428">Naplófájl-keresési megnyílik, és a tárolók, az alábbihoz hasonló állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1a28a-428">Log Search opens and displays the state of your containers, similar to the following.</span></span>  
   ![tárolók állapota](./media/log-analytics-containers/containers-log-search.png)
3. <span data-ttu-id="1a28a-430">Ezután kattintson a további információk megjelenítéséhez sikertelen tárolók összesített értékét.</span><span class="sxs-lookup"><span data-stu-id="1a28a-430">Next, click the aggregated value of failed containers to view additional information.</span></span> <span data-ttu-id="1a28a-431">Bontsa ki a **megjelenítése további** megtekintéséhez a lemezkép-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="1a28a-431">Expand **show more** to view the image ID.</span></span>  
   <span data-ttu-id="1a28a-432">![nem sikerült tárolók](./media/log-analytics-containers/containers-state-failed.png)</span><span class="sxs-lookup"><span data-stu-id="1a28a-432">![failed containers](./media/log-analytics-containers/containers-state-failed.png)</span></span>  
4. <span data-ttu-id="1a28a-433">Ezután írja be a következőt a keresési lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="1a28a-433">Next, type the following in the search query.</span></span> <span data-ttu-id="1a28a-434">`Type=ContainerInventory <ImageID>`a kép, például a lemezkép mérete és a leállított és sikertelen csomópontképek száma kapcsolatos részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="1a28a-434">`Type=ContainerInventory <ImageID>` to see details about the image such as image size and number of stopped and failed images.</span></span>  
   <span data-ttu-id="1a28a-435">![nem sikerült tárolók](./media/log-analytics-containers/containers-failed04.png)</span><span class="sxs-lookup"><span data-stu-id="1a28a-435">![failed containers](./media/log-analytics-containers/containers-failed04.png)</span></span>

## <a name="search-logs-for-container-data"></a><span data-ttu-id="1a28a-436">Keresési naplókat a további adatai</span><span class="sxs-lookup"><span data-stu-id="1a28a-436">Search logs for container data</span></span>
<span data-ttu-id="1a28a-437">Ha egy adott hiba elhárításához van, hogy hol lépett fel a környezetben segíthet.</span><span class="sxs-lookup"><span data-stu-id="1a28a-437">When you're troubleshooting a specific error, it can help to see where it is occurring in your environment.</span></span> <span data-ttu-id="1a28a-438">A következő napló típusok segítségével hozhat létre a kívánt lekérdezések adott vissza az adatokat.</span><span class="sxs-lookup"><span data-stu-id="1a28a-438">The following log types will help you create queries to return the information you want.</span></span>


- <span data-ttu-id="1a28a-439">**ContainerImageInventory** – ezt a típust használja, a lemezkép szerint vannak rendszerezve kapcsolatos információk és lemezkép információk például az azonosítók vagy méretű kép megjelenítéséhez regisztráláskor.</span><span class="sxs-lookup"><span data-stu-id="1a28a-439">**ContainerImageInventory** – Use this type when you're trying to find information organized by image and to view image information such as image IDs or sizes.</span></span>
- <span data-ttu-id="1a28a-440">**ContainerInventory** – Ez a típus használható, ha azt szeretné, hogy információt tároló helye, Mik azok a nevük, és mi képek azok futtatja.</span><span class="sxs-lookup"><span data-stu-id="1a28a-440">**ContainerInventory** – Use this type when you want information about container location, what their names are, and what images they're running.</span></span>
- <span data-ttu-id="1a28a-441">**ContainerLog** – Ez a típus használja, ha a keresett adott hibanaplóban szereplő információkat, és bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="1a28a-441">**ContainerLog** – Use this type when you want to find specific error log information and entries.</span></span>
- <span data-ttu-id="1a28a-442">**ContainerNodeInventory_CL** használni ehhez a típushoz a gazdacsomópont/vonatkozó információkat ahol tárolók tartózkodnak.</span><span class="sxs-lookup"><span data-stu-id="1a28a-442">**ContainerNodeInventory_CL**  Use this type when you want the information about host/node where containers are residing.</span></span> <span data-ttu-id="1a28a-443">Azt teszi lehetővé Docker verzióval, vezénylés típusát, tárolási és hálózati információit.</span><span class="sxs-lookup"><span data-stu-id="1a28a-443">It provides you Docker version, orchestration type, storage, and network information.</span></span>
- <span data-ttu-id="1a28a-444">**ContainerProcess_CL** ennek használatával gyorsan megtekintheti a tárolóban futó folyamatot.</span><span class="sxs-lookup"><span data-stu-id="1a28a-444">**ContainerProcess_CL** Use this type to quickly see the process running within the container.</span></span>
- <span data-ttu-id="1a28a-445">**ContainerServiceLog** – ezt a típust használja, ellenőrzési útvonalat nyújt információkat a Docker démon, például Indítás, Leállítás, törlése, illetve parancsok lekéréses kereséséhez regisztráláskor.</span><span class="sxs-lookup"><span data-stu-id="1a28a-445">**ContainerServiceLog** – Use this type when you're trying to find audit trail information for the Docker daemon, such as start, stop, delete, or pull commands.</span></span>
- <span data-ttu-id="1a28a-446">**KubeEvents_CL** ennek használatával az Kubernetes eseményeket.</span><span class="sxs-lookup"><span data-stu-id="1a28a-446">**KubeEvents_CL**  Use this type to see the Kubernetes events.</span></span>
- <span data-ttu-id="1a28a-447">**KubePodInventory_CL** ezt a típust használja, ha szeretné tudni, hogy a fürt hierarchiára vonatkozó információk.</span><span class="sxs-lookup"><span data-stu-id="1a28a-447">**KubePodInventory_CL**  Use this type when you want to understand the cluster hierarchy information.</span></span>


### <a name="to-search-logs-for-container-data"></a><span data-ttu-id="1a28a-448">Keresés a naplókat a további adatai</span><span class="sxs-lookup"><span data-stu-id="1a28a-448">To search logs for container data</span></span>
* <span data-ttu-id="1a28a-449">Válassza ki, hogy tudja, hogy a képfájl nemrég sikertelen volt, és a hibanaplók keresése.</span><span class="sxs-lookup"><span data-stu-id="1a28a-449">Choose an image that you know has failed recently and find the error logs for it.</span></span> <span data-ttu-id="1a28a-450">Indítsa el a tároló neve, amelyen fut. a lemezkép keresése a **ContainerInventory** keresési.</span><span class="sxs-lookup"><span data-stu-id="1a28a-450">Start by finding a container name that is running that image with a **ContainerInventory** search.</span></span> <span data-ttu-id="1a28a-451">Például keresése`Type=ContainerInventory ubuntu Failed`</span><span class="sxs-lookup"><span data-stu-id="1a28a-451">For example, search for `Type=ContainerInventory ubuntu Failed`</span></span>  
    <span data-ttu-id="1a28a-452">![Ubuntu tárolók keresése](./media/log-analytics-containers/search-ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="1a28a-452">![Search for Ubuntu containers](./media/log-analytics-containers/search-ubuntu.png)</span></span>

  <span data-ttu-id="1a28a-453">A tároló neve **neve**, majd keresse meg a lesznek a naplók.</span><span class="sxs-lookup"><span data-stu-id="1a28a-453">The name of the container next to **Name**, and search for those logs.</span></span> <span data-ttu-id="1a28a-454">Ebben a példában ez `Type=ContainerLog cranky_stonebreaker`.</span><span class="sxs-lookup"><span data-stu-id="1a28a-454">In this example, it is `Type=ContainerLog cranky_stonebreaker`.</span></span>

<span data-ttu-id="1a28a-455">**Teljesítmény-információk megtekintése**</span><span class="sxs-lookup"><span data-stu-id="1a28a-455">**View performance information**</span></span>

<span data-ttu-id="1a28a-456">Ha éppen kezdődő, lekérdezések összeállításához, megtudhatja, mi lehetséges először segítséget.</span><span class="sxs-lookup"><span data-stu-id="1a28a-456">When you're beginning to construct queries, it can help to see what's possible first.</span></span> <span data-ttu-id="1a28a-457">Például minden teljesítményadat megtekintéséhez próbálja írja be a következő keresési lekérdezés széleskörű lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="1a28a-457">For example, to see all performance data, try a broad query by typing the following search query.</span></span>

```
Type=Perf
```

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf01.png)

<span data-ttu-id="1a28a-459">Hatókörét megadhatja a teljesítményadatokat is lát egy adott tárolóhoz jobb oldalán a lekérdezést, hogy a név beírásával.</span><span class="sxs-lookup"><span data-stu-id="1a28a-459">You can scope the performance data you're seeing to a specific container by typing the name of it to the right of your query.</span></span>

```
Type=Perf <containerName>
```

<span data-ttu-id="1a28a-460">Amely az egyes tároló összegyűjtött teljesítménymutatók listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1a28a-460">That shows the list of performance metrics that are collected for an individual container.</span></span>

![tárolók teljesítmény](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a><span data-ttu-id="1a28a-462">Példa napló keresési lekérdezések</span><span class="sxs-lookup"><span data-stu-id="1a28a-462">Example log search queries</span></span>
<span data-ttu-id="1a28a-463">Gyakran érdemes hozhatók létre olyan lekérdezések például vagy két és megfeleljenek a környezet módosításával.</span><span class="sxs-lookup"><span data-stu-id="1a28a-463">It's often useful to build queries starting with an example or two and then modifying them to fit your environment.</span></span> <span data-ttu-id="1a28a-464">Kiindulási pontként, kísérletezhet az **mintalekérdezések** terület segíteni bonyolultabb lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="1a28a-464">As a starting point, you can experiment with the **Sample Queries** area to help you build more advanced queries.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Tárolók lekérdezések](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a><span data-ttu-id="1a28a-466">Naplófájl-keresési lekérdezések mentése</span><span class="sxs-lookup"><span data-stu-id="1a28a-466">Saving log search queries</span></span>
<span data-ttu-id="1a28a-467">Lekérdezések mentése az Naplóelemzési szabványos szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="1a28a-467">Saving queries is a standard feature in Log Analytics.</span></span> <span data-ttu-id="1a28a-468">Mentve, konfigurálnia kell az alábbiakhoz hasznos talált későbbi használatra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="1a28a-468">By saving them, you'll have those that you've found useful handy for future use.</span></span>

<span data-ttu-id="1a28a-469">Miután létrehozott egy lekérdezést, amely akkor hasznosak, mentse kattintva **Kedvencek** a napló keresése oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="1a28a-469">After you create a query that you find useful, save it by clicking **Favorites** at the top of the Log Search page.</span></span> <span data-ttu-id="1a28a-470">Ezután egyszerűen hozzáférhet az azt később a **saját irányítópult** lap.</span><span class="sxs-lookup"><span data-stu-id="1a28a-470">Then you can easily access it later from the **My Dashboard** page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a28a-471">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1a28a-471">Next steps</span></span>
* <span data-ttu-id="1a28a-472">[Naplók keresése](log-analytics-log-searches.md) részletes tároló rekordok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="1a28a-472">[Search logs](log-analytics-log-searches.md) to view detailed container data records.</span></span>
