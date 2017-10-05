---
title: "Azure-felhő rendszerhéj (előzetes verzió) funkció |} Microsoft Docs"
description: "Azure Cloud rendszerhéj funkcióinak áttekintése"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 67f03d5857e37b253ac57536e289b5468d69e9b5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="features-and-tools-for-azure-cloud-shell"></a><span data-ttu-id="5ee26-103">Funkciók és eszközök az Azure-felhőbe rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="5ee26-103">Features and Tools for Azure Cloud Shell</span></span>
<span data-ttu-id="5ee26-104">Azure Cloud rendszerhéj található az adott egy webböngésző-alapú felület és Azure-erőforrások fejlesztését.</span><span class="sxs-lookup"><span data-stu-id="5ee26-104">Azure Cloud Shell is a browser-based shell experience to manage and develop Azure resources.</span></span>

<span data-ttu-id="5ee26-105">Felhő rendszerhéj kínál a böngésző által elérhető, előre konfigurált rendszerhéj élmény nem növeli a telepítése, versioning, és a gép karbantartása, saját magának az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="5ee26-105">Cloud Shell offers a browser-accessible, pre-configured shell experience for managing Azure resources without the overhead of installing, versioning, and maintaining a machine yourself.</span></span>

<span data-ttu-id="5ee26-106">Felhő rendszerhéj kérelem alapú számítógépek kiépítését, és emiatt gép állapota nem minden munkamenetben megmarad.</span><span class="sxs-lookup"><span data-stu-id="5ee26-106">Cloud Shell provisions machines on a per-request basis and as a result machine state will not persist across sessions.</span></span> <span data-ttu-id="5ee26-107">Felhő rendszerhéj interaktív munkamenet lett tervezve, mivel a parancskörnyezet automatikusan leáll, 20 perc rendszerhéj inaktivitás után.</span><span class="sxs-lookup"><span data-stu-id="5ee26-107">Since Cloud Shell is built for interactive sessions, shells automatically terminate after 20 minutes of shell inactivity.</span></span>

## <a name="bash-in-cloud-shell"></a><span data-ttu-id="5ee26-108">A felhő rendszerhéj bash</span><span class="sxs-lookup"><span data-stu-id="5ee26-108">Bash in Cloud Shell</span></span>
### <a name="tools"></a><span data-ttu-id="5ee26-109">Eszközök</span><span class="sxs-lookup"><span data-stu-id="5ee26-109">Tools</span></span>
|<span data-ttu-id="5ee26-110">Kategória</span><span class="sxs-lookup"><span data-stu-id="5ee26-110">Category</span></span>   |<span data-ttu-id="5ee26-111">Név</span><span class="sxs-lookup"><span data-stu-id="5ee26-111">Name</span></span>   |
|---|---|
|<span data-ttu-id="5ee26-112">Linux-rendszerhéj parancsértelmező</span><span class="sxs-lookup"><span data-stu-id="5ee26-112">Linux shell interpreter</span></span>|<span data-ttu-id="5ee26-113">Bash</span><span class="sxs-lookup"><span data-stu-id="5ee26-113">Bash</span></span><br> <span data-ttu-id="5ee26-114">SH</span><span class="sxs-lookup"><span data-stu-id="5ee26-114">sh</span></span>               |
|<span data-ttu-id="5ee26-115">Azure-eszközök</span><span class="sxs-lookup"><span data-stu-id="5ee26-115">Azure tools</span></span>            |<span data-ttu-id="5ee26-116">[Az Azure CLI 2.0](https://github.com/Azure/azure-cli) és [1.0](https://github.com/Azure/azure-xplat-cli)</span><span class="sxs-lookup"><span data-stu-id="5ee26-116">[Azure CLI 2.0](https://github.com/Azure/azure-cli) and [1.0](https://github.com/Azure/azure-xplat-cli)</span></span><br> [<span data-ttu-id="5ee26-117">AzCopy</span><span class="sxs-lookup"><span data-stu-id="5ee26-117">AzCopy</span></span>](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [<span data-ttu-id="5ee26-118">Batch Shipyard</span><span class="sxs-lookup"><span data-stu-id="5ee26-118">Batch Shipyard</span></span>](https://github.com/Azure/batch-shipyard)     |
|<span data-ttu-id="5ee26-119">A szerkesztő szövege</span><span class="sxs-lookup"><span data-stu-id="5ee26-119">Text editors</span></span>           |<span data-ttu-id="5ee26-120">VIM</span><span class="sxs-lookup"><span data-stu-id="5ee26-120">vim</span></span><br> <span data-ttu-id="5ee26-121">nano</span><span class="sxs-lookup"><span data-stu-id="5ee26-121">nano</span></span><br> <span data-ttu-id="5ee26-122">emacs</span><span class="sxs-lookup"><span data-stu-id="5ee26-122">emacs</span></span>       |
|<span data-ttu-id="5ee26-123">A verziókövetési rendszerrel</span><span class="sxs-lookup"><span data-stu-id="5ee26-123">Source control</span></span>         |<span data-ttu-id="5ee26-124">git</span><span class="sxs-lookup"><span data-stu-id="5ee26-124">git</span></span>                    |
|<span data-ttu-id="5ee26-125">Buildet</span><span class="sxs-lookup"><span data-stu-id="5ee26-125">Build tools</span></span>            |<span data-ttu-id="5ee26-126">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="5ee26-126">make</span></span><br> <span data-ttu-id="5ee26-127">maven</span><span class="sxs-lookup"><span data-stu-id="5ee26-127">maven</span></span><br> <span data-ttu-id="5ee26-128">npm</span><span class="sxs-lookup"><span data-stu-id="5ee26-128">npm</span></span><br> <span data-ttu-id="5ee26-129">a pip</span><span class="sxs-lookup"><span data-stu-id="5ee26-129">pip</span></span>         |
|<span data-ttu-id="5ee26-130">Tárolók</span><span class="sxs-lookup"><span data-stu-id="5ee26-130">Containers</span></span>             |<span data-ttu-id="5ee26-131">[A docker parancssori felület](https://github.com/docker/cli)/[Docker gép](https://github.com/docker/machine)</span><span class="sxs-lookup"><span data-stu-id="5ee26-131">[Docker CLI](https://github.com/docker/cli)/[Docker Machine](https://github.com/docker/machine)</span></span><br> [<span data-ttu-id="5ee26-132">Kubectl</span><span class="sxs-lookup"><span data-stu-id="5ee26-132">Kubectl</span></span>](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [<span data-ttu-id="5ee26-133">Vázlat</span><span class="sxs-lookup"><span data-stu-id="5ee26-133">Draft</span></span>](https://github.com/Azure/draft)<br> [<span data-ttu-id="5ee26-134">DC/OS PARANCSSORI FELÜLET</span><span class="sxs-lookup"><span data-stu-id="5ee26-134">DC/OS CLI</span></span>](https://github.com/dcos/dcos-cli)         |
|<span data-ttu-id="5ee26-135">Adatbázisok</span><span class="sxs-lookup"><span data-stu-id="5ee26-135">Databases</span></span>              |<span data-ttu-id="5ee26-136">MySQL-ügyfél</span><span class="sxs-lookup"><span data-stu-id="5ee26-136">MySQL client</span></span><br> <span data-ttu-id="5ee26-137">PostgreSql-ügyfél</span><span class="sxs-lookup"><span data-stu-id="5ee26-137">PostgreSql client</span></span><br> [<span data-ttu-id="5ee26-138">Az Sqlcmd segédprogram használatával</span><span class="sxs-lookup"><span data-stu-id="5ee26-138">sqlcmd Utility</span></span>](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [<span data-ttu-id="5ee26-139">MSSQL-scripter</span><span class="sxs-lookup"><span data-stu-id="5ee26-139">mssql-scripter</span></span>](https://github.com/Microsoft/sql-xplat-cli) |
|<span data-ttu-id="5ee26-140">Egyéb</span><span class="sxs-lookup"><span data-stu-id="5ee26-140">Other</span></span>                  |<span data-ttu-id="5ee26-141">iPython ügyfél</span><span class="sxs-lookup"><span data-stu-id="5ee26-141">iPython Client</span></span><br> [<span data-ttu-id="5ee26-142">Felhő Foundry parancssori felület</span><span class="sxs-lookup"><span data-stu-id="5ee26-142">Cloud Foundry CLI</span></span>](https://github.com/cloudfoundry/cli)<br> |

### <a name="language-support"></a><span data-ttu-id="5ee26-143">Nyelvi támogatás</span><span class="sxs-lookup"><span data-stu-id="5ee26-143">Language support</span></span>
|<span data-ttu-id="5ee26-144">Nyelv</span><span class="sxs-lookup"><span data-stu-id="5ee26-144">Language</span></span>   |<span data-ttu-id="5ee26-145">Verzió</span><span class="sxs-lookup"><span data-stu-id="5ee26-145">Version</span></span>   |
|---|---|
|<span data-ttu-id="5ee26-146">.NET</span><span class="sxs-lookup"><span data-stu-id="5ee26-146">.NET</span></span>       |<span data-ttu-id="5ee26-147">1.01</span><span class="sxs-lookup"><span data-stu-id="5ee26-147">1.01</span></span>       |
|<span data-ttu-id="5ee26-148">Indítás</span><span class="sxs-lookup"><span data-stu-id="5ee26-148">Go</span></span>         |<span data-ttu-id="5ee26-149">1.7</span><span class="sxs-lookup"><span data-stu-id="5ee26-149">1.7</span></span>        |
|<span data-ttu-id="5ee26-150">Java</span><span class="sxs-lookup"><span data-stu-id="5ee26-150">Java</span></span>       |<span data-ttu-id="5ee26-151">1.8</span><span class="sxs-lookup"><span data-stu-id="5ee26-151">1.8</span></span>        |
|<span data-ttu-id="5ee26-152">Node.js</span><span class="sxs-lookup"><span data-stu-id="5ee26-152">Node.js</span></span>    |<span data-ttu-id="5ee26-153">6.9.4</span><span class="sxs-lookup"><span data-stu-id="5ee26-153">6.9.4</span></span>      |
|<span data-ttu-id="5ee26-154">Python</span><span class="sxs-lookup"><span data-stu-id="5ee26-154">Python</span></span>     |<span data-ttu-id="5ee26-155">2.7 és 3.5-ös (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="5ee26-155">2.7 and 3.5 (default)</span></span>|

## <a name="secure-automatic-authentication"></a><span data-ttu-id="5ee26-156">Biztonságos automatikus hitelesítés</span><span class="sxs-lookup"><span data-stu-id="5ee26-156">Secure automatic authentication</span></span>
<span data-ttu-id="5ee26-157">Felhő rendszerhéj biztonságosan és automatikusan hitelesíti felhasználóifiók-hozzáférés az Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="5ee26-157">Cloud Shell securely and automatically authenticates account access for the Azure CLI 2.0.</span></span>

## <a name="azure-files-persistence"></a><span data-ttu-id="5ee26-158">Az Azure fájlok adatmegőrzési</span><span class="sxs-lookup"><span data-stu-id="5ee26-158">Azure Files persistence</span></span>
<span data-ttu-id="5ee26-159">Ideiglenes gépek használatának kérelem alapú felhő rendszerhéj lefoglalt, mivel fájlok kívül a $Home és a gép állapota nem maradnak meg a munkamenetek között.</span><span class="sxs-lookup"><span data-stu-id="5ee26-159">Since Cloud Shell is allocated on a per-request basis using a temporary machine, files outside of your $Home and machine state are not persisted across sessions.</span></span>
<span data-ttu-id="5ee26-160">A munkamenetek között a fájlok továbbra is fennáll, felhőalapú rendszerhéj bemutatja, hogyan csatlakoztatása az Azure fájlmegosztások első indításkor.</span><span class="sxs-lookup"><span data-stu-id="5ee26-160">To persist files across sessions, Cloud Shell walks you through attaching an Azure file share on first launch.</span></span>
<span data-ttu-id="5ee26-161">Egyszer befejezett felhő rendszerhéj automatikusan elvégzi a tárhely minden további munkamenetekhez.</span><span class="sxs-lookup"><span data-stu-id="5ee26-161">Once completed Cloud Shell will automatically attach your storage for all future sessions.</span></span>

[<span data-ttu-id="5ee26-162">További tudnivalók az Azure fájlmegosztások csatolása felhő rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="5ee26-162">Learn more about attaching Azure file shares to Cloud Shell.</span></span>](persisting-shell-storage.md)

## <a name="next-steps"></a><span data-ttu-id="5ee26-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ee26-163">Next steps</span></span>
[<span data-ttu-id="5ee26-164">Felhő rendszerhéj gyors üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="5ee26-164">Cloud Shell Quickstart</span></span>](quickstart.md) <br><span data-ttu-id="5ee26-165">
[További tudnivalók az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span><span class="sxs-lookup"><span data-stu-id="5ee26-165">
[Learn about Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span></span> <br>