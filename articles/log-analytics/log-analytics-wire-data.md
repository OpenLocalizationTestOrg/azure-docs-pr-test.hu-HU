---
title: "Naplóelemzési adatok megoldás aaaWire |} Microsoft Docs"
description: "Átviteli adatokat az OMS-ügynököt, beleértve az Operations Manager és a Windows-csatlakoztatott ügynökök rendelkező számítógépekről összevont hálózati és a teljesítmény adatai. Hálózati adatok egyesítése a naplózási adatok toohelp az adatok összefüggéseket."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: banders
ms.openlocfilehash: adafdf98dfbda9d87759643a1a606a84eafd1348
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a><span data-ttu-id="6fba3-104">A Naplóelemzési átviteli adatok 2.0 (előzetes verzió) megoldás</span><span class="sxs-lookup"><span data-stu-id="6fba3-104">Wire Data 2.0 (Preview) solution in Log Analytics</span></span>

![Átviteli adatokat szimbólum](./media/log-analytics-wire-data/wire-data2-symbol.png)

<span data-ttu-id="6fba3-106">Átviteli adatokat az OMS-ügynököt, beleértve az Operations Manager, a Windows-csatlakozóval csatlakoztatott, és a Linux-ügynökök rendelkező számítógépekről összevont hálózati és a teljesítmény adatai.</span><span class="sxs-lookup"><span data-stu-id="6fba3-106">Wire data is consolidated network and performance data from computers with OMS agents, including Operations Manager, Windows-connected, and Linux agents.</span></span> <span data-ttu-id="6fba3-107">Hálózati adatok egyesítése a többi napló adatok toohelp az adatok összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="6fba3-107">Network data is combined with your other log data toohelp you correlate data.</span></span>

<span data-ttu-id="6fba3-108">Továbbá tooOMS ügynökök hello átviteli adatokat megoldást használ Microsoft Dependency ügynökök, a számítástechnikai infrastruktúra számítógépeken telepíteni.</span><span class="sxs-lookup"><span data-stu-id="6fba3-108">In addition tooOMS agents, hello Wire Data solution uses Microsoft Dependency agents that you install on computers in your IT infrastructure.</span></span> <span data-ttu-id="6fba3-109">Függőségi ügynökök figyelő hálózati adatforgalom tooand a számítógépek hálózati szintek a 2-3 hello [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), beleértve a hello különböző protokollok és portok.</span><span class="sxs-lookup"><span data-stu-id="6fba3-109">Dependency Agents monitor network data sent tooand from your computers for network levels 2-3 in hello [OSI model](https://en.wikipedia.org/wiki/OSI_model), including hello various protocols and ports used.</span></span> <span data-ttu-id="6fba3-110">Adatokat küldi el a rendszer tooLog Analytics ügynököt használ.</span><span class="sxs-lookup"><span data-stu-id="6fba3-110">Data is then sent tooLog Analytics using agents.</span></span>

> [!NOTE]
> <span data-ttu-id="6fba3-111">Nem adható hozzá hello hello átviteli adatokat megoldás toonew munkaterületek korábbi verzióját.</span><span class="sxs-lookup"><span data-stu-id="6fba3-111">You cannot add hello previous version of hello Wire Data solution toonew workspaces.</span></span> <span data-ttu-id="6fba3-112">Ha hello eredeti átviteli adatokat megoldás, toouse folytathatja azt.</span><span class="sxs-lookup"><span data-stu-id="6fba3-112">If you have hello original Wire Data solution enabled, you can continue toouse it.</span></span> <span data-ttu-id="6fba3-113">Azonban, hálózaton adatok 2.0 toouse, el kell távolítania hello eredeti verzióját.</span><span class="sxs-lookup"><span data-stu-id="6fba3-113">However, toouse Wire Data 2.0, you must first remove hello original version.</span></span>

<span data-ttu-id="6fba3-114">Alapértelmezés szerint a Naplóelemzési naplózott adatokat gyűjti a Processzor, memória, lemez és hálózat teljesítményadatokat számlálók a Windows beépített.</span><span class="sxs-lookup"><span data-stu-id="6fba3-114">By default, Log Analytics collects logged data for CPU, memory, disk, and network performance data from counters built into Windows.</span></span> <span data-ttu-id="6fba3-115">Hálózati és egyéb adatok gyűjtése történik valós idejű minden ügynökhöz, beleértve az alhálózatok és hello számítógép által használt alkalmazásszintű protokollok.</span><span class="sxs-lookup"><span data-stu-id="6fba3-115">Network and other data collection is done in real-time for each agent, including subnets and application-level protocols being used by hello computer.</span></span> <span data-ttu-id="6fba3-116">Más teljesítményszámlálók a hello beállítások lapon a hello naplófájlok lapján is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-116">You can add other performance counters on hello Settings page on hello Logs tab.</span></span>

<span data-ttu-id="6fba3-117">Ha már használta [sFlow](http://www.sflow.org/) vagy más szoftvereket [Cisco tartozó NetFlow protokoll](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), majd hello statisztika, és adatokat átviteli adatokat a megszokott tooyou.</span><span class="sxs-lookup"><span data-stu-id="6fba3-117">If you've used [sFlow](http://www.sflow.org/) or other software with [Cisco's NetFlow protocol](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), then hello statistics and data you see from wire data will be familiar tooyou.</span></span>

<span data-ttu-id="6fba3-118">Beépített napló keresési lekérdezések hello típusai a következők:</span><span class="sxs-lookup"><span data-stu-id="6fba3-118">Some of hello types of built-in Log search queries include:</span></span>

- <span data-ttu-id="6fba3-119">Átviteli adatokat szolgáltató ügynökök</span><span class="sxs-lookup"><span data-stu-id="6fba3-119">Agents that provide wire data</span></span>
- <span data-ttu-id="6fba3-120">Átviteli adatokat szolgáltató ügynökök IP-címe</span><span class="sxs-lookup"><span data-stu-id="6fba3-120">IP address of agents providing wire data</span></span>
- <span data-ttu-id="6fba3-121">Kimenő kommunikáció IP-cím</span><span class="sxs-lookup"><span data-stu-id="6fba3-121">Outbound communications by IP addresses</span></span>
- <span data-ttu-id="6fba3-122">Alkalmazás-protokollokra által küldött bájtok száma</span><span class="sxs-lookup"><span data-stu-id="6fba3-122">Number of bytes sent by application protocols</span></span>
- <span data-ttu-id="6fba3-123">Egy alkalmazás szolgáltatás által küldött bájtok száma</span><span class="sxs-lookup"><span data-stu-id="6fba3-123">Number of bytes sent by an application service</span></span>
- <span data-ttu-id="6fba3-124">A különböző protokollok által fogadott bájtok</span><span class="sxs-lookup"><span data-stu-id="6fba3-124">Bytes received by different protocols</span></span>
- <span data-ttu-id="6fba3-125">IP-verziója által küldött és fogadott bájtok teljes száma</span><span class="sxs-lookup"><span data-stu-id="6fba3-125">Total bytes sent and received by IP version</span></span>
- <span data-ttu-id="6fba3-126">Volt megbízhatóan mérhető kapcsolatok átlagos késése</span><span class="sxs-lookup"><span data-stu-id="6fba3-126">Average latency for connections that were measured reliably</span></span>
- <span data-ttu-id="6fba3-127">A számítógép folyamatainak kezdeményezett, illetve a hálózati forgalom érkezett</span><span class="sxs-lookup"><span data-stu-id="6fba3-127">Computer processes that initiated or received network traffic</span></span>
- <span data-ttu-id="6fba3-128">A folyamatot a hálózati forgalom mennyisége</span><span class="sxs-lookup"><span data-stu-id="6fba3-128">Amount of network traffic for a process</span></span>

<span data-ttu-id="6fba3-129">Átviteli adatokat használó keresésénél szűrheti is, és csoport adatok tooview információ hello felső ügynökök és a felső protokollok.</span><span class="sxs-lookup"><span data-stu-id="6fba3-129">When you search using wire data, you can filter and group data tooview information about hello top agents and top protocols.</span></span> <span data-ttu-id="6fba3-130">Szeretné megtekinteni, ha egyes számítógépek (IP-címet vagy MAC-címek) közölt egymással, hogyan hosszú, és mennyi adatot küldött – alapvetően, megtekintheti a hálózati forgalmat, amely keresési-alapú metaadatait.</span><span class="sxs-lookup"><span data-stu-id="6fba3-130">Or you can view when certain computers (IP addresses/MAC addresses) communicated with each other, for how long, and how much data was sent—basically, you view metadata about network traffic, which is search-based.</span></span>

<span data-ttu-id="6fba3-131">Azonban mivel megtekintett metaadatok, célszerű nem feltétlenül részletes hibaelhárítási.</span><span class="sxs-lookup"><span data-stu-id="6fba3-131">However, since you're viewing metadata, it's not necessarily useful for in-depth troubleshooting.</span></span> <span data-ttu-id="6fba3-132">Átvitel közbeni Naplóelemzési adatai nem a teljes rögzítési hálózati adatok.</span><span class="sxs-lookup"><span data-stu-id="6fba3-132">Wire data in Log Analytics is not a full capture of network data.</span></span> <span data-ttu-id="6fba3-133">Tehát nem készült részletes csomagszintű hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="6fba3-133">So, it's not intended for deep packet-level troubleshooting.</span></span> <span data-ttu-id="6fba3-134">hello hello ügynök használatának előnye, tooother adatgyűjtési módszerek képest, hogy nem tooinstall készülékek, konfigurálja újra a hálózati kapcsolókat, vagy összetett konfigurációk végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="6fba3-134">hello advantage of using hello agent, compared tooother collection methods, is that you don't have tooinstall appliances, reconfigure your network switches, or preform complicated configurations.</span></span> <span data-ttu-id="6fba3-135">Átviteli adatokat egyszerűen ügynök-alapú – hello ügynököt telepít egy számítógépre, és azt a saját hálózati forgalom figyeli.</span><span class="sxs-lookup"><span data-stu-id="6fba3-135">Wire data is simply agent-based—you install hello agent on a computer and it will monitor its own network traffic.</span></span> <span data-ttu-id="6fba3-136">Egy másik előnye, ha azt szeretné, hogy a szolgáltatók vagy az üzemeltetési szolgáltató vagy a Microsoft Azure, ahol a hello felhasználói saját hello háló réteg nem toomonitor munkaterhelésekre.</span><span class="sxs-lookup"><span data-stu-id="6fba3-136">Another advantage is when you want toomonitor workloads running in cloud providers or hosting service provider or Microsoft Azure, where hello user doesn't own hello fabric layer.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="6fba3-137">Összekapcsolt források</span><span class="sxs-lookup"><span data-stu-id="6fba3-137">Connected sources</span></span>

<span data-ttu-id="6fba3-138">Átviteli adatokat veszi hello Microsoft függőségi ügynök az adatokat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-138">Wire Data gets its data from hello Microsoft Dependency Agent.</span></span> <span data-ttu-id="6fba3-139">hello függőségi ügynök hello OMS-ügynököt a kapcsolatok tooLog Analytics a függ.</span><span class="sxs-lookup"><span data-stu-id="6fba3-139">hello Dependency Agent depends on hello OMS Agent for its connections tooLog Analytics.</span></span> <span data-ttu-id="6fba3-140">Ez azt jelenti, hogy egy kiszolgáló hello OMS-ügynököt telepítette, és konfigurálta az első kell rendelkeznie, és hello függőségi ügynök telepítését követően.</span><span class="sxs-lookup"><span data-stu-id="6fba3-140">This means that a server must have hello OMS Agent installed and configured first, and then you install hello Dependency Agent.</span></span> <span data-ttu-id="6fba3-141">hello következő táblázat ismerteti, amely támogatja a hello átviteli adatokat megoldás hello csatlakoztatott adatforrások.</span><span class="sxs-lookup"><span data-stu-id="6fba3-141">hello following table describes hello connected sources that hello Wire Data solution supports.</span></span>

| <span data-ttu-id="6fba3-142">**Csatlakoztatott adatforrás**</span><span class="sxs-lookup"><span data-stu-id="6fba3-142">**Connected source**</span></span> | <span data-ttu-id="6fba3-143">**Támogatott**</span><span class="sxs-lookup"><span data-stu-id="6fba3-143">**Supported**</span></span> | <span data-ttu-id="6fba3-144">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6fba3-144">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6fba3-145">Windows-ügynökök</span><span class="sxs-lookup"><span data-stu-id="6fba3-145">Windows agents</span></span> | <span data-ttu-id="6fba3-146">Igen</span><span class="sxs-lookup"><span data-stu-id="6fba3-146">Yes</span></span> | <span data-ttu-id="6fba3-147">Átviteli adatokat elemzi, és a Windows-ügynök számítógépekről gyűjt adatokat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-147">Wire Data analyzes and collects data from Windows agent computers.</span></span> <br><br> <span data-ttu-id="6fba3-148">A hozzáadása toohello [OMS-ügynököt](log-analytics-windows-agents.md), Windows-ügynökök hello Microsoft függőségi ügynök szükséges.</span><span class="sxs-lookup"><span data-stu-id="6fba3-148">In addition toohello [OMS Agent](log-analytics-windows-agents.md), Windows agents require hello Microsoft Dependency Agent.</span></span> <span data-ttu-id="6fba3-149">Lásd: hello [támogatott operációs rendszerek](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) operációs rendszerek teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="6fba3-149">See hello [supported operating systems](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) for a complete list of operating system versions.</span></span> |
| <span data-ttu-id="6fba3-150">Linux-ügynökök</span><span class="sxs-lookup"><span data-stu-id="6fba3-150">Linux agents</span></span> | <span data-ttu-id="6fba3-151">Igen</span><span class="sxs-lookup"><span data-stu-id="6fba3-151">Yes</span></span> | <span data-ttu-id="6fba3-152">Átviteli adatokat elemzi, és a Linux-ügynök számítógépekről gyűjt adatokat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-152">Wire Data analyzes and collects data from Linux agent computers.</span></span><br><br> <span data-ttu-id="6fba3-153">A hozzáadása toohello [OMS-ügynököt](log-analytics-linux-agents.md), Linux-ügynökök hello Microsoft függőségi ügynök szükséges.</span><span class="sxs-lookup"><span data-stu-id="6fba3-153">In addition toohello [OMS Agent](log-analytics-linux-agents.md), Linux agents require hello Microsoft Dependency Agent.</span></span> <span data-ttu-id="6fba3-154">Lásd: hello [támogatott operációs rendszerek](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) operációs rendszerek teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="6fba3-154">See hello [supported operating systems](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) for a complete list of operating system versions.</span></span> |
| <span data-ttu-id="6fba3-155">System Center Operations Manage felügyeleti csoport</span><span class="sxs-lookup"><span data-stu-id="6fba3-155">System Center Operations Manager management group</span></span> | <span data-ttu-id="6fba3-156">Igen</span><span class="sxs-lookup"><span data-stu-id="6fba3-156">Yes</span></span> | <span data-ttu-id="6fba3-157">Átviteli adatokat elemzi, és összegyűjti az adatokat a Windows és Linux-ügynökök a csatlakoztatott [System Center Operations Manager felügyeleti csoport](log-analytics-om-agents.md).</span><span class="sxs-lookup"><span data-stu-id="6fba3-157">Wire Data analyzes and collects data from Windows and Linux agents in a connected [System Center Operations Manager management group](log-analytics-om-agents.md).</span></span> <br><br> <span data-ttu-id="6fba3-158">Közvetlen kapcsolat az hello System Center Operations Manager ügynök számítógép tooLog Analytics megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="6fba3-158">A direct connection from hello System Center Operations Manager agent computer tooLog Analytics is required.</span></span> <span data-ttu-id="6fba3-159">A hello felügyeleti csoport tooLog Analytics adat továbbítódik.</span><span class="sxs-lookup"><span data-stu-id="6fba3-159">Data is forwarded from hello management group tooLog Analytics.</span></span> |
| <span data-ttu-id="6fba3-160">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="6fba3-160">Azure storage account</span></span> | <span data-ttu-id="6fba3-161">Nem</span><span class="sxs-lookup"><span data-stu-id="6fba3-161">No</span></span> | <span data-ttu-id="6fba3-162">Átviteli adatokat adatokat gyűjt ügynök számítógépekről, így nincsenek adatok, toocollect Azure Storage-ból.</span><span class="sxs-lookup"><span data-stu-id="6fba3-162">Wire Data collects data from agent computers, so there is no data from it toocollect from Azure Storage.</span></span> |

<span data-ttu-id="6fba3-163">Windows, a Microsoft Monitoring Agent (MMA) hello System Center Operations Manager és a Naplóelemzési toogather használja, és adatokat küldeni.</span><span class="sxs-lookup"><span data-stu-id="6fba3-163">On Windows, hello Microsoft Monitoring Agent (MMA) is used by both System Center Operations Manager and Log Analytics toogather and send data.</span></span> <span data-ttu-id="6fba3-164">Attól függően, hogy hello környezetben hello ügynök hello System Center Operations Manager ügynök, OMS-ügynököt, Log Analytics Agent, MMA vagy közvetlen ügynök neve.</span><span class="sxs-lookup"><span data-stu-id="6fba3-164">Depending on hello context, hello agent is called hello System Center Operations Manager Agent, OMS Agent, Log Analytics Agent, MMA, or Direct Agent.</span></span> <span data-ttu-id="6fba3-165">A System Center Operations Manager és a Naplóelemzési biztosít hello MMA némileg különböző verziói.</span><span class="sxs-lookup"><span data-stu-id="6fba3-165">System Center Operations Manager and Log Analytics provide slightly different versions of hello MMA.</span></span> <span data-ttu-id="6fba3-166">Ezen verziói egyes jelenthetik tooSystem Center Operations Manager, tooLog Analytics vagy tooboth.</span><span class="sxs-lookup"><span data-stu-id="6fba3-166">These versions can each report tooSystem Center Operations Manager, tooLog Analytics, or tooboth.</span></span>

<span data-ttu-id="6fba3-167">Linux hello Linux OMS-ügynököt gyűjt, és elküldi az adatokat tooLog elemzés.</span><span class="sxs-lookup"><span data-stu-id="6fba3-167">On Linux, hello OMS Agent for Linux gathers and sends data tooLog Analytics.</span></span> <span data-ttu-id="6fba3-168">Átviteli adatokat közvetlen OMS-ügynököt a kiszolgálón vagy kiszolgálókon, amelyek a System Center Operations Manager felügyeleti csoportok keresztül csatlakoztatott tooLog Analytics használható.</span><span class="sxs-lookup"><span data-stu-id="6fba3-168">You can use Wire Data on servers with OMS Direct Agents or on servers that are attached tooLog Analytics via System Center Operations Manager management groups.</span></span>

<span data-ttu-id="6fba3-169">Ez a cikk hivatkozik tooall ügynökök, hogy Linux vagy a Windows, hogy csatlakoztatott tooa System Center Operations Manager felügyeleti csoportot, vagy közvetlenül tooLog Analytics hello hívják _OMS-ügynököt_.</span><span class="sxs-lookup"><span data-stu-id="6fba3-169">In this article, references tooall agents, whether Linux or Windows, whether connected tooa System Center Operations Manager management group or directly tooLog Analytics are termed hello _OMS agent_.</span></span> <span data-ttu-id="6fba3-170">Hello ügynök hello adott központi telepítés neve csak akkor, ha szükség van a környezetben fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="6fba3-170">We'll use hello specific deployment name of hello agent only if it's needed for context.</span></span>

<span data-ttu-id="6fba3-171">hello függőségi ügynök nem továbbítja az adatokat saját magát, és nem igényel módosításokat toofirewalls vagy portok.</span><span class="sxs-lookup"><span data-stu-id="6fba3-171">hello Dependency Agent does not transmit any data itself, and it does not require any changes toofirewalls or ports.</span></span> <span data-ttu-id="6fba3-172">hello az átviteli adatokat mindig továbbított adatok által hello OMS ügynök tooLog elemzés, vagy közvetlenül, vagy az OMS átjáró használatával hello.</span><span class="sxs-lookup"><span data-stu-id="6fba3-172">hello data in Wire Data is always transmitted by hello OMS agent tooLog Analytics, either directly or using hello OMS Gateway.</span></span>

![ügynök diagramja](./media/log-analytics-wire-data/agents.png)

<span data-ttu-id="6fba3-174">Ha egy felügyeleti csoporthoz csatlakoztatott tooLog Analytics rendelkező System Center Operations Manager felhasználó:</span><span class="sxs-lookup"><span data-stu-id="6fba3-174">If you are a System Center Operations Manager user with a management group connected tooLog Analytics:</span></span>

- <span data-ttu-id="6fba3-175">Hello Internet tooconnect tooLog Analytics férhetnek hozzá a System Center Operations Manager-ügynökök nincs szükség további konfigurációra.</span><span class="sxs-lookup"><span data-stu-id="6fba3-175">No additional configuration is required when your System Center Operations Manager agents can access hello Internet tooconnect tooLog Analytics.</span></span>
- <span data-ttu-id="6fba3-176">Ha a System Center Operations Manager-ügynökök nem tud hozzáférni a Naplóelemzési hello interneten keresztül kell tooconfigure hello OMS átjáró toowork a System Center Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="6fba3-176">You need tooconfigure hello OMS Gateway toowork with System Center Operations Manager when your System Center Operations Manager agents cannot access Log Analytics over hello Internet.</span></span>

<span data-ttu-id="6fba3-177">Ha közvetlen ügynök hello használ, kell tooconfigure hello OMS-ügynököt maga tooconnect tooLog Analytics vagy tooyour OMS-átjáró.</span><span class="sxs-lookup"><span data-stu-id="6fba3-177">If you are using hello Direct Agent, you need tooconfigure hello OMS agent itself tooconnect tooLog Analytics or tooyour OMS Gateway.</span></span> <span data-ttu-id="6fba3-178">Hello OMS átjáró letölthető hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).</span><span class="sxs-lookup"><span data-stu-id="6fba3-178">You can download hello OMS Gateway from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6fba3-179">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6fba3-179">Prerequisites</span></span>

- <span data-ttu-id="6fba3-180">Hello szükséges [betekintést és az elemzések](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) megoldás ajánlat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-180">Requires hello [Insight and Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) solution offer.</span></span>
- <span data-ttu-id="6fba3-181">Átviteli adatokat megoldás hello korábbi verziójának hello használata, távolítsa el azt.</span><span class="sxs-lookup"><span data-stu-id="6fba3-181">If you're using hello previous version of hello Wire Data solution, you must first remove it.</span></span> <span data-ttu-id="6fba3-182">Azonban az összes rögzített hello eredeti átviteli adatokat megoldáson keresztül érhetők el az adatok továbbra is átviteli adatok 2.0 és a keresési napló.</span><span class="sxs-lookup"><span data-stu-id="6fba3-182">However, all data captured through hello original Wire Data solution is still available in Wire Data 2.0 and log search.</span></span>
- <span data-ttu-id="6fba3-183">Rendszergazdai jogosultságokkal szükséges tooinstall, vagy távolítsa el a függőségi ügynök hello.</span><span class="sxs-lookup"><span data-stu-id="6fba3-183">Administrator privileges are required tooinstall or uninstall hello Dependency Agent.</span></span>
- <span data-ttu-id="6fba3-184">hello függőségi ügynök telepítenie kell a 64 bites operációs rendszerrel rendelkező számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6fba3-184">hello Dependency Agent must be installed on a computer with a 64-bit operating system.</span></span>

### <a name="operating-systems"></a><span data-ttu-id="6fba3-185">Operációs rendszerek</span><span class="sxs-lookup"><span data-stu-id="6fba3-185">Operating systems</span></span>

<span data-ttu-id="6fba3-186">hello alábbi szakaszok tartalmazzák hello támogatott operációs rendszerek hello függőségi ügynök.</span><span class="sxs-lookup"><span data-stu-id="6fba3-186">hello following sections list hello supported operating systems for hello Dependency Agent.</span></span> <span data-ttu-id="6fba3-187">Átviteli adatokat nem támogatja 32 bites operációs rendszert.</span><span class="sxs-lookup"><span data-stu-id="6fba3-187">Wire Data doesn't support 32-bit architectures for any operating system.</span></span>

#### <a name="windows-server"></a><span data-ttu-id="6fba3-188">Windows Server</span><span class="sxs-lookup"><span data-stu-id="6fba3-188">Windows Server</span></span>

- <span data-ttu-id="6fba3-189">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="6fba3-189">Windows Server 2016</span></span>
- <span data-ttu-id="6fba3-190">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="6fba3-190">Windows Server 2012 R2</span></span>
- <span data-ttu-id="6fba3-191">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="6fba3-191">Windows Server 2012</span></span>
- <span data-ttu-id="6fba3-192">Windows Server 2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="6fba3-192">Windows Server 2008 R2 SP1</span></span>

#### <a name="windows-desktop"></a><span data-ttu-id="6fba3-193">Asztali Windows</span><span class="sxs-lookup"><span data-stu-id="6fba3-193">Windows desktop</span></span>

- <span data-ttu-id="6fba3-194">Windows 10</span><span class="sxs-lookup"><span data-stu-id="6fba3-194">Windows 10</span></span>
- <span data-ttu-id="6fba3-195">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="6fba3-195">Windows 8.1</span></span>
- <span data-ttu-id="6fba3-196">Windows 8</span><span class="sxs-lookup"><span data-stu-id="6fba3-196">Windows 8</span></span>
- <span data-ttu-id="6fba3-197">Windows 7</span><span class="sxs-lookup"><span data-stu-id="6fba3-197">Windows 7</span></span>

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a><span data-ttu-id="6fba3-198">Red Hat Enterprise Linux, a CentOS Linux és az Oracle Linux (az RHEL Kernel)</span><span class="sxs-lookup"><span data-stu-id="6fba3-198">Red Hat Enterprise Linux, CentOS Linux, and Oracle Linux (with RHEL Kernel)</span></span>

- <span data-ttu-id="6fba3-199">Csak az alapértelmezett és az SMP Linux kernel verziókban támogatott.</span><span class="sxs-lookup"><span data-stu-id="6fba3-199">Only default and SMP Linux kernel releases are supported.</span></span>
- <span data-ttu-id="6fba3-200">Nem szabványos kernel kiadását, például a Xen, és a fizikai nem támogatottak a Linux-disztribúció.</span><span class="sxs-lookup"><span data-stu-id="6fba3-200">Nonstandard kernel releases, such as PAE and Xen, are not supported for any Linux distribution.</span></span> <span data-ttu-id="6fba3-201">Például egy hello kiadás karakterláncot a rendszer _2.6.16.21-0.8-xen_ nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6fba3-201">For example, a system with hello release string of _2.6.16.21-0.8-xen_ is not supported.</span></span>
- <span data-ttu-id="6fba3-202">Egyéni mag, beleértve a szabványos kernelek újrafordításainak maximális nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="6fba3-202">Custom kernels, including recompiles of standard kernels, are not supported.</span></span>
- <span data-ttu-id="6fba3-203">CentOSPlus kernel nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6fba3-203">CentOSPlus kernel is not supported.</span></span>
- <span data-ttu-id="6fba3-204">Ez a cikk későbbi részében Oracle szoros vállalati Kernel (UEK) vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="6fba3-204">Oracle Unbreakable Enterprise Kernel (UEK) is covered in a later section of this article.</span></span>

#### <a name="red-hat-linux-7"></a><span data-ttu-id="6fba3-205">Red Hat Linux 7</span><span class="sxs-lookup"><span data-stu-id="6fba3-205">Red Hat Linux 7</span></span>

| <span data-ttu-id="6fba3-206">**Operációs rendszer verziója**</span><span class="sxs-lookup"><span data-stu-id="6fba3-206">**OS version**</span></span> | <span data-ttu-id="6fba3-207">**Kernel-verzió**</span><span class="sxs-lookup"><span data-stu-id="6fba3-207">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="6fba3-208">7.0</span><span class="sxs-lookup"><span data-stu-id="6fba3-208">7.0</span></span> | <span data-ttu-id="6fba3-209">3.10.0-123</span><span class="sxs-lookup"><span data-stu-id="6fba3-209">3.10.0-123</span></span> |
| <span data-ttu-id="6fba3-210">7.1</span><span class="sxs-lookup"><span data-stu-id="6fba3-210">7.1</span></span> | <span data-ttu-id="6fba3-211">3.10.0-229</span><span class="sxs-lookup"><span data-stu-id="6fba3-211">3.10.0-229</span></span> |
| <span data-ttu-id="6fba3-212">7.2</span><span class="sxs-lookup"><span data-stu-id="6fba3-212">7.2</span></span> | <span data-ttu-id="6fba3-213">3.10.0-327</span><span class="sxs-lookup"><span data-stu-id="6fba3-213">3.10.0-327</span></span> |
| <span data-ttu-id="6fba3-214">7.3</span><span class="sxs-lookup"><span data-stu-id="6fba3-214">7.3</span></span> | <span data-ttu-id="6fba3-215">3.10.0-514</span><span class="sxs-lookup"><span data-stu-id="6fba3-215">3.10.0-514</span></span> |

#### <a name="red-hat-linux-6"></a><span data-ttu-id="6fba3-216">Red Hat Linux 6</span><span class="sxs-lookup"><span data-stu-id="6fba3-216">Red Hat Linux 6</span></span>

| <span data-ttu-id="6fba3-217">**Operációs rendszer verziója**</span><span class="sxs-lookup"><span data-stu-id="6fba3-217">**OS version**</span></span> | <span data-ttu-id="6fba3-218">**Kernel-verzió**</span><span class="sxs-lookup"><span data-stu-id="6fba3-218">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="6fba3-219">6.0</span><span class="sxs-lookup"><span data-stu-id="6fba3-219">6.0</span></span> | <span data-ttu-id="6fba3-220">2.6.32-71</span><span class="sxs-lookup"><span data-stu-id="6fba3-220">2.6.32-71</span></span> |
| <span data-ttu-id="6fba3-221">6.1</span><span class="sxs-lookup"><span data-stu-id="6fba3-221">6.1</span></span> | <span data-ttu-id="6fba3-222">2.6.32-131</span><span class="sxs-lookup"><span data-stu-id="6fba3-222">2.6.32-131</span></span> |
| <span data-ttu-id="6fba3-223">6.2</span><span class="sxs-lookup"><span data-stu-id="6fba3-223">6.2</span></span> | <span data-ttu-id="6fba3-224">2.6.32-220</span><span class="sxs-lookup"><span data-stu-id="6fba3-224">2.6.32-220</span></span> |
| <span data-ttu-id="6fba3-225">6.3</span><span class="sxs-lookup"><span data-stu-id="6fba3-225">6.3</span></span> | <span data-ttu-id="6fba3-226">2.6.32-279</span><span class="sxs-lookup"><span data-stu-id="6fba3-226">2.6.32-279</span></span> |
| <span data-ttu-id="6fba3-227">6.4</span><span class="sxs-lookup"><span data-stu-id="6fba3-227">6.4</span></span> | <span data-ttu-id="6fba3-228">2.6.32-358</span><span class="sxs-lookup"><span data-stu-id="6fba3-228">2.6.32-358</span></span> |
| <span data-ttu-id="6fba3-229">6.5</span><span class="sxs-lookup"><span data-stu-id="6fba3-229">6.5</span></span> | <span data-ttu-id="6fba3-230">2.6.32-431</span><span class="sxs-lookup"><span data-stu-id="6fba3-230">2.6.32-431</span></span> |
| <span data-ttu-id="6fba3-231">6.6</span><span class="sxs-lookup"><span data-stu-id="6fba3-231">6.6</span></span> | <span data-ttu-id="6fba3-232">2.6.32-504</span><span class="sxs-lookup"><span data-stu-id="6fba3-232">2.6.32-504</span></span> |
| <span data-ttu-id="6fba3-233">6.7</span><span class="sxs-lookup"><span data-stu-id="6fba3-233">6.7</span></span> | <span data-ttu-id="6fba3-234">2.6.32-573</span><span class="sxs-lookup"><span data-stu-id="6fba3-234">2.6.32-573</span></span> |
| <span data-ttu-id="6fba3-235">6.8</span><span class="sxs-lookup"><span data-stu-id="6fba3-235">6.8</span></span> | <span data-ttu-id="6fba3-236">2.6.32-642</span><span class="sxs-lookup"><span data-stu-id="6fba3-236">2.6.32-642</span></span> |

#### <a name="red-hat-linux-5"></a><span data-ttu-id="6fba3-237">Red Hat Linux 5</span><span class="sxs-lookup"><span data-stu-id="6fba3-237">Red Hat Linux 5</span></span>

| <span data-ttu-id="6fba3-238">**Operációs rendszer verziója**</span><span class="sxs-lookup"><span data-stu-id="6fba3-238">**OS version**</span></span> | <span data-ttu-id="6fba3-239">**Kernel-verzió**</span><span class="sxs-lookup"><span data-stu-id="6fba3-239">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="6fba3-240">5.8</span><span class="sxs-lookup"><span data-stu-id="6fba3-240">5.8</span></span> | <span data-ttu-id="6fba3-241">2.6.18-308</span><span class="sxs-lookup"><span data-stu-id="6fba3-241">2.6.18-308</span></span> |
| <span data-ttu-id="6fba3-242">5.9</span><span class="sxs-lookup"><span data-stu-id="6fba3-242">5.9</span></span> | <span data-ttu-id="6fba3-243">2.6.18-348</span><span class="sxs-lookup"><span data-stu-id="6fba3-243">2.6.18-348</span></span> |
| <span data-ttu-id="6fba3-244">5.10</span><span class="sxs-lookup"><span data-stu-id="6fba3-244">5.10</span></span> | <span data-ttu-id="6fba3-245">2.6.18-371</span><span class="sxs-lookup"><span data-stu-id="6fba3-245">2.6.18-371</span></span> |
| <span data-ttu-id="6fba3-246">5.11</span><span class="sxs-lookup"><span data-stu-id="6fba3-246">5.11</span></span> | <span data-ttu-id="6fba3-247">2.6.18-398</span><span class="sxs-lookup"><span data-stu-id="6fba3-247">2.6.18-398</span></span> <br> <span data-ttu-id="6fba3-248">2.6.18-400</span><span class="sxs-lookup"><span data-stu-id="6fba3-248">2.6.18-400</span></span> <br><span data-ttu-id="6fba3-249">2.6.18-402</span><span class="sxs-lookup"><span data-stu-id="6fba3-249">2.6.18-402</span></span> <br><span data-ttu-id="6fba3-250">2.6.18-404</span><span class="sxs-lookup"><span data-stu-id="6fba3-250">2.6.18-404</span></span> <br><span data-ttu-id="6fba3-251">2.6.18-406</span><span class="sxs-lookup"><span data-stu-id="6fba3-251">2.6.18-406</span></span> <br> <span data-ttu-id="6fba3-252">2.6.18-407</span><span class="sxs-lookup"><span data-stu-id="6fba3-252">2.6.18-407</span></span> <br> <span data-ttu-id="6fba3-253">2.6.18-408</span><span class="sxs-lookup"><span data-stu-id="6fba3-253">2.6.18-408</span></span> <br> <span data-ttu-id="6fba3-254">2.6.18-409</span><span class="sxs-lookup"><span data-stu-id="6fba3-254">2.6.18-409</span></span> <br> <span data-ttu-id="6fba3-255">2.6.18-410</span><span class="sxs-lookup"><span data-stu-id="6fba3-255">2.6.18-410</span></span> <br> <span data-ttu-id="6fba3-256">2.6.18-411</span><span class="sxs-lookup"><span data-stu-id="6fba3-256">2.6.18-411</span></span> <br> <span data-ttu-id="6fba3-257">2.6.18-412</span><span class="sxs-lookup"><span data-stu-id="6fba3-257">2.6.18-412</span></span> <br> <span data-ttu-id="6fba3-258">2.6.18-416</span><span class="sxs-lookup"><span data-stu-id="6fba3-258">2.6.18-416</span></span> <br> <span data-ttu-id="6fba3-259">2.6.18-417</span><span class="sxs-lookup"><span data-stu-id="6fba3-259">2.6.18-417</span></span> <br> <span data-ttu-id="6fba3-260">2.6.18-419</span><span class="sxs-lookup"><span data-stu-id="6fba3-260">2.6.18-419</span></span> |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a><span data-ttu-id="6fba3-261">Szoros vállalati Kernel az Oracle Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="6fba3-261">Oracle Enterprise Linux with Unbreakable Enterprise Kernel</span></span>

#### <a name="oracle-linux-6"></a><span data-ttu-id="6fba3-262">Oracle Linux 6</span><span class="sxs-lookup"><span data-stu-id="6fba3-262">Oracle Linux 6</span></span>

| <span data-ttu-id="6fba3-263">**Operációs rendszer verziója**</span><span class="sxs-lookup"><span data-stu-id="6fba3-263">**OS version**</span></span> | <span data-ttu-id="6fba3-264">**Kernel-verzió**</span><span class="sxs-lookup"><span data-stu-id="6fba3-264">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="6fba3-265">6.2</span><span class="sxs-lookup"><span data-stu-id="6fba3-265">6.2</span></span> | <span data-ttu-id="6fba3-266">Oracle 2.6.32-300 (UEK R1)</span><span class="sxs-lookup"><span data-stu-id="6fba3-266">Oracle 2.6.32-300 (UEK R1)</span></span> |
| <span data-ttu-id="6fba3-267">6.3</span><span class="sxs-lookup"><span data-stu-id="6fba3-267">6.3</span></span> | <span data-ttu-id="6fba3-268">Oracle 2.6.39-200 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="6fba3-268">Oracle 2.6.39-200 (UEK R2)</span></span> |
| <span data-ttu-id="6fba3-269">6.4</span><span class="sxs-lookup"><span data-stu-id="6fba3-269">6.4</span></span> | <span data-ttu-id="6fba3-270">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="6fba3-270">Oracle 2.6.39-400 (UEK R2)</span></span> |
| <span data-ttu-id="6fba3-271">6.5</span><span class="sxs-lookup"><span data-stu-id="6fba3-271">6.5</span></span> | <span data-ttu-id="6fba3-272">Oracle 2.6.39-400 (UEK R2 i386)</span><span class="sxs-lookup"><span data-stu-id="6fba3-272">Oracle 2.6.39-400 (UEK R2 i386)</span></span> |
| <span data-ttu-id="6fba3-273">6.6</span><span class="sxs-lookup"><span data-stu-id="6fba3-273">6.6</span></span> | <span data-ttu-id="6fba3-274">Oracle 2.6.39-400 (UEK R2 i386)</span><span class="sxs-lookup"><span data-stu-id="6fba3-274">Oracle 2.6.39-400 (UEK R2 i386)</span></span> |

#### <a name="oracle-linux-5"></a><span data-ttu-id="6fba3-275">Oracle Linux 5</span><span class="sxs-lookup"><span data-stu-id="6fba3-275">Oracle Linux 5</span></span>

| <span data-ttu-id="6fba3-276">**Operációs rendszer verziója**</span><span class="sxs-lookup"><span data-stu-id="6fba3-276">**OS version**</span></span> | <span data-ttu-id="6fba3-277">**Kernel-verzió**</span><span class="sxs-lookup"><span data-stu-id="6fba3-277">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="6fba3-278">5.8</span><span class="sxs-lookup"><span data-stu-id="6fba3-278">5.8</span></span> | <span data-ttu-id="6fba3-279">Oracle 2.6.32-300 (UEK R1)</span><span class="sxs-lookup"><span data-stu-id="6fba3-279">Oracle 2.6.32-300 (UEK R1)</span></span> |
| <span data-ttu-id="6fba3-280">5.9</span><span class="sxs-lookup"><span data-stu-id="6fba3-280">5.9</span></span> | <span data-ttu-id="6fba3-281">Oracle 2.6.39-300 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="6fba3-281">Oracle 2.6.39-300 (UEK R2)</span></span> |
| <span data-ttu-id="6fba3-282">5.10</span><span class="sxs-lookup"><span data-stu-id="6fba3-282">5.10</span></span> | <span data-ttu-id="6fba3-283">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="6fba3-283">Oracle 2.6.39-400 (UEK R2)</span></span> |
| <span data-ttu-id="6fba3-284">5.11</span><span class="sxs-lookup"><span data-stu-id="6fba3-284">5.11</span></span> | <span data-ttu-id="6fba3-285">Oracle 2.6.39-400 (UEK R2)</span><span class="sxs-lookup"><span data-stu-id="6fba3-285">Oracle 2.6.39-400 (UEK R2)</span></span> |

#### <a name="suse-linux-enterprise-server"></a><span data-ttu-id="6fba3-286">SUSE Linux Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="6fba3-286">SUSE Linux Enterprise Server</span></span>

#### <a name="suse-linux-11"></a><span data-ttu-id="6fba3-287">SUSE Linux 11</span><span class="sxs-lookup"><span data-stu-id="6fba3-287">SUSE Linux 11</span></span>

| <span data-ttu-id="6fba3-288">**Operációs rendszer verziója**</span><span class="sxs-lookup"><span data-stu-id="6fba3-288">**OS version**</span></span> | <span data-ttu-id="6fba3-289">**Kernel-verzió**</span><span class="sxs-lookup"><span data-stu-id="6fba3-289">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="6fba3-290">11</span><span class="sxs-lookup"><span data-stu-id="6fba3-290">11</span></span> | <span data-ttu-id="6fba3-291">2.6.27</span><span class="sxs-lookup"><span data-stu-id="6fba3-291">2.6.27</span></span> |
| <span data-ttu-id="6fba3-292">11 SP1</span><span class="sxs-lookup"><span data-stu-id="6fba3-292">11 SP1</span></span> | <span data-ttu-id="6fba3-293">2.6.32</span><span class="sxs-lookup"><span data-stu-id="6fba3-293">2.6.32</span></span> |
| <span data-ttu-id="6fba3-294">11 SP2</span><span class="sxs-lookup"><span data-stu-id="6fba3-294">11 SP2</span></span> | <span data-ttu-id="6fba3-295">3.0.13</span><span class="sxs-lookup"><span data-stu-id="6fba3-295">3.0.13</span></span> |
| <span data-ttu-id="6fba3-296">11 SP3</span><span class="sxs-lookup"><span data-stu-id="6fba3-296">11 SP3</span></span> | <span data-ttu-id="6fba3-297">3.0.76</span><span class="sxs-lookup"><span data-stu-id="6fba3-297">3.0.76</span></span> |
| <span data-ttu-id="6fba3-298">11 SP4</span><span class="sxs-lookup"><span data-stu-id="6fba3-298">11 SP4</span></span> | <span data-ttu-id="6fba3-299">3.0.101</span><span class="sxs-lookup"><span data-stu-id="6fba3-299">3.0.101</span></span> |

#### <a name="suse-linux-10"></a><span data-ttu-id="6fba3-300">SUSE Linux 10</span><span class="sxs-lookup"><span data-stu-id="6fba3-300">SUSE Linux 10</span></span>

| <span data-ttu-id="6fba3-301">**Operációs rendszer verziója**</span><span class="sxs-lookup"><span data-stu-id="6fba3-301">**OS version**</span></span> | <span data-ttu-id="6fba3-302">**Kernel-verzió**</span><span class="sxs-lookup"><span data-stu-id="6fba3-302">**Kernel version**</span></span> |
| --- | --- |
| <span data-ttu-id="6fba3-303">10 SP4</span><span class="sxs-lookup"><span data-stu-id="6fba3-303">10 SP4</span></span> | <span data-ttu-id="6fba3-304">2.6.16.60</span><span class="sxs-lookup"><span data-stu-id="6fba3-304">2.6.16.60</span></span> |

#### <a name="dependency-agent-downloads"></a><span data-ttu-id="6fba3-305">A függőségi ügynök letöltése</span><span class="sxs-lookup"><span data-stu-id="6fba3-305">Dependency Agent downloads</span></span>

| <span data-ttu-id="6fba3-306">**Fájl**</span><span class="sxs-lookup"><span data-stu-id="6fba3-306">**File**</span></span> | <span data-ttu-id="6fba3-307">**AZ OPERÁCIÓS RENDSZER**</span><span class="sxs-lookup"><span data-stu-id="6fba3-307">**OS**</span></span> | <span data-ttu-id="6fba3-308">**Verzió**</span><span class="sxs-lookup"><span data-stu-id="6fba3-308">**Version**</span></span> | <span data-ttu-id="6fba3-309">**AZ SHA-256-RA**</span><span class="sxs-lookup"><span data-stu-id="6fba3-309">**SHA-256**</span></span> |
| --- | --- | --- | --- |
| [<span data-ttu-id="6fba3-310">InstallDependencyAgent-Windows.exe</span><span class="sxs-lookup"><span data-stu-id="6fba3-310">InstallDependencyAgent-Windows.exe</span></span>](https://aka.ms/dependencyagentwindows) | <span data-ttu-id="6fba3-311">Windows</span><span class="sxs-lookup"><span data-stu-id="6fba3-311">Windows</span></span> | <span data-ttu-id="6fba3-312">9.0.5</span><span class="sxs-lookup"><span data-stu-id="6fba3-312">9.0.5</span></span> | <span data-ttu-id="6fba3-313">73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43</span><span class="sxs-lookup"><span data-stu-id="6fba3-313">73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43</span></span> |
| [<span data-ttu-id="6fba3-314">InstallDependencyAgent-Linux64.bin</span><span class="sxs-lookup"><span data-stu-id="6fba3-314">InstallDependencyAgent-Linux64.bin</span></span>](https://aka.ms/dependencyagentlinux) | <span data-ttu-id="6fba3-315">Linux</span><span class="sxs-lookup"><span data-stu-id="6fba3-315">Linux</span></span> | <span data-ttu-id="6fba3-316">9.0.5</span><span class="sxs-lookup"><span data-stu-id="6fba3-316">9.0.5</span></span> | <span data-ttu-id="6fba3-317">A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371</span><span class="sxs-lookup"><span data-stu-id="6fba3-317">A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371</span></span> |



## <a name="configuration"></a><span data-ttu-id="6fba3-318">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="6fba3-318">Configuration</span></span>

<span data-ttu-id="6fba3-319">Hajtsa végre a következő lépéseket tooconfigure hello átviteli adatokat megoldás a munkaterületek hello.</span><span class="sxs-lookup"><span data-stu-id="6fba3-319">Perform hello following steps tooconfigure hello Wire Data solution for your workspaces.</span></span>

1. <span data-ttu-id="6fba3-320">Hello tevékenység Naplóelemzési megoldást hello engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="6fba3-320">Enable hello Activity Log Analytics solution from hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="6fba3-321">Hello függőségi ügynök telepítése minden olyan számítógépen, ahová tooget adatokat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-321">Install hello Dependency Agent on each computer where you want tooget data.</span></span> <span data-ttu-id="6fba3-322">hello függőségi ügynök kapcsolatok tooimmediate szomszédok, figyelhet, így előfordulhat, hogy nem kell minden olyan számítógépen az ügynök.</span><span class="sxs-lookup"><span data-stu-id="6fba3-322">hello Dependency Agent can monitor connections tooimmediate neighbors, so you might not need an agent on every computer.</span></span>

### <a name="install-hello-dependency-agent-on-windows"></a><span data-ttu-id="6fba3-323">A Windows hello függőségi ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="6fba3-323">Install hello Dependency Agent on Windows</span></span>

<span data-ttu-id="6fba3-324">Rendszergazdai jogosultság szükséges tooinstall vagy hello ügynök eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="6fba3-324">Administrator privileges are required tooinstall or uninstall hello agent.</span></span>

<span data-ttu-id="6fba3-325">hello függőségi ügynök InstallDependencyAgent-Windows.exe Windows rendszert futtató számítógépekre telepíthető.</span><span class="sxs-lookup"><span data-stu-id="6fba3-325">hello Dependency Agent is installed on computers running Windows through InstallDependencyAgent-Windows.exe.</span></span> <span data-ttu-id="6fba3-326">Ha a végrehajtható fájl kapcsolók nélkül futtatja, az elindít egy varázslót, hogy követésével tooinstall interaktív módon.</span><span class="sxs-lookup"><span data-stu-id="6fba3-326">If you run this executable file without any options, it starts a wizard that you can follow tooinstall interactively.</span></span>

<span data-ttu-id="6fba3-327">Hello lépéseket tooinstall hello függőségi ügynök minden Windows rendszerű számítógépen a következő használja:</span><span class="sxs-lookup"><span data-stu-id="6fba3-327">Use hello following steps tooinstall hello Dependency Agent on each computer running Windows:</span></span>

1. <span data-ttu-id="6fba3-328">Telepítés hello OMS-ügynököt a hello található utasítások segítségével: [csatlakozás Windows számítógépek toohello az Azure Naplóelemzés szolgáltatás](log-analytics-windows-agents.md).</span><span class="sxs-lookup"><span data-stu-id="6fba3-328">Install hello OMS Agent by using hello instructions at [Connect Windows computers toohello Log Analytics service in Azure](log-analytics-windows-agents.md).</span></span>
2. <span data-ttu-id="6fba3-329">Az előző szakaszban hello hello hivatkozással hello Windows-ügynök letöltése, és futtassa a következő parancs hello segítségével: InstallDependencyAgent-Windows.exe</span><span class="sxs-lookup"><span data-stu-id="6fba3-329">Download hello Windows agent using hello link in hello previous section and then run it by using hello following command: InstallDependencyAgent-Windows.exe</span></span>
3. <span data-ttu-id="6fba3-330">Hajtsa végre a hello varázsló tooinstall hello ügynök.</span><span class="sxs-lookup"><span data-stu-id="6fba3-330">Follow hello wizard tooinstall hello agent.</span></span>
4. <span data-ttu-id="6fba3-331">Ha hello függőségi ügynök toostart sikertelen, tekintse meg a hibával kapcsolatos részletes információk hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-331">If hello Dependency Agent fails toostart, check hello logs for detailed error information.</span></span> <span data-ttu-id="6fba3-332">A Windows-ügynökök hello naplókönyvtár %Programfiles%\Microsoft függőségi Agent\logs.</span><span class="sxs-lookup"><span data-stu-id="6fba3-332">On Windows agents, hello log directory is %Programfiles%\Microsoft Dependency Agent\logs.</span></span>

#### <a name="windows-command-line"></a><span data-ttu-id="6fba3-333">A Windows parancssor</span><span class="sxs-lookup"><span data-stu-id="6fba3-333">Windows command line</span></span>

<span data-ttu-id="6fba3-334">A hello tábla tooinstall a parancssorból a következő beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="6fba3-334">Use options from hello following table tooinstall from a command line.</span></span> <span data-ttu-id="6fba3-335">hello telepítési jelző, hello telepítő futtatásához hello segítségével listája toosee /?</span><span class="sxs-lookup"><span data-stu-id="6fba3-335">toosee a list of hello installation flags, run hello installer by using hello /?</span></span> <span data-ttu-id="6fba3-336">Ez a jelző az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="6fba3-336">flag as follows.</span></span>

<span data-ttu-id="6fba3-337">InstallDependencyAgent-Windows.exe /?</span><span class="sxs-lookup"><span data-stu-id="6fba3-337">InstallDependencyAgent-Windows.exe /?</span></span>

| <span data-ttu-id="6fba3-338">**Jelzője**</span><span class="sxs-lookup"><span data-stu-id="6fba3-338">**Flag**</span></span> | <span data-ttu-id="6fba3-339">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6fba3-339">**Description**</span></span> |
| --- | --- |
| <code>/?</code> | <span data-ttu-id="6fba3-340">Hello parancssori kapcsolók listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="6fba3-340">Get a list of hello command-line options.</span></span> |
| <code>/S</code> | <span data-ttu-id="6fba3-341">Felhasználói beavatkozás nélküli telepítés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="6fba3-341">Perform a silent installation with no user prompts.</span></span> |

<span data-ttu-id="6fba3-342">Hello Windows függőségi ügynök fájljait alapértelmezésben a C:\Program Files\Microsoft függőségi ügynök kerül.</span><span class="sxs-lookup"><span data-stu-id="6fba3-342">Files for hello Windows Dependency Agent are placed in C:\Program Files\Microsoft Dependency Agent by default.</span></span>

### <a name="install-hello-dependency-agent-on-linux"></a><span data-ttu-id="6fba3-343">Hello függőségi ügynök telepítése Linux rendszerre</span><span class="sxs-lookup"><span data-stu-id="6fba3-343">Install hello Dependency Agent on Linux</span></span>

<span data-ttu-id="6fba3-344">Legfelső szintű hozzáférés szükséges tooinstall vagy hello ügynök konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6fba3-344">Root access is required tooinstall or configure hello agent.</span></span>

<span data-ttu-id="6fba3-345">Linux rendszerű számítógépek InstallDependencyAgent Linux64.bin, egy önkicsomagoló bináris héjparancsfájlt keresztül hello függőségi ügynök van telepítve.</span><span class="sxs-lookup"><span data-stu-id="6fba3-345">hello Dependency Agent is installed on Linux computers through InstallDependencyAgent-Linux64.bin, a shell script with a self-extracting binary.</span></span> <span data-ttu-id="6fba3-346">Hello fájl segítségével is futtathatja _megosztása_ , vagy adja hozzá engedélyek toohello fájl saját magát.</span><span class="sxs-lookup"><span data-stu-id="6fba3-346">You can run hello file by using _sh_ or add execute permissions toohello file itself.</span></span>

<span data-ttu-id="6fba3-347">Hello lépéseket tooinstall hello függőségi ügynök a Linux rendszerű számítógépekre a következő használja:</span><span class="sxs-lookup"><span data-stu-id="6fba3-347">Use hello following steps tooinstall hello Dependency Agent on each Linux computer:</span></span>

1. <span data-ttu-id="6fba3-348">Telepítés hello OMS-ügynököt a hello található utasítások segítségével: [adatok gyűjtésére és kezelésére a Linux rendszerű számítógépek](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6fba3-348">Install hello OMS Agent by using hello instructions at [Collect and manage data from Linux computers](log-analytics-agent-linux.md).</span></span>
2. <span data-ttu-id="6fba3-349">Hello Linux függőségi ügynök az előző szakaszban hello hello hivatkozással töltse le és telepítse legfelső szintű hello a következő parancs használatával: sh InstallDependencyAgent-Linux64.bin</span><span class="sxs-lookup"><span data-stu-id="6fba3-349">Download hello Linux Dependency agent using hello link in hello previous section and then install it as root by using hello following command: sh InstallDependencyAgent-Linux64.bin</span></span>
3. <span data-ttu-id="6fba3-350">Ha hello függőségi ügynök toostart sikertelen, tekintse meg a hibával kapcsolatos részletes információk hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-350">If hello Dependency Agent fails toostart, check hello logs for detailed error information.</span></span> <span data-ttu-id="6fba3-351">A Linux-ügynökök hello naplókönyvtár van: /var/opt/microsoft/dependency-agent/log.</span><span class="sxs-lookup"><span data-stu-id="6fba3-351">On Linux agents, hello log directory is: /var/opt/microsoft/dependency-agent/log.</span></span>

<span data-ttu-id="6fba3-352">hello telepítési jelző, hello programjának futtatása a hello listája toosee `-help` jelzőt az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="6fba3-352">toosee a list of hello installation flags, run hello installation program with hello `-help` flag as follows.</span></span>

```
InstallDependencyAgent-Linux64.bin -help
```

| <span data-ttu-id="6fba3-353">**Jelzője**</span><span class="sxs-lookup"><span data-stu-id="6fba3-353">**Flag**</span></span> | <span data-ttu-id="6fba3-354">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6fba3-354">**Description**</span></span> |
| --- | --- |
| <code>-help</code> | <span data-ttu-id="6fba3-355">Hello parancssori kapcsolók listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="6fba3-355">Get a list of hello command-line options.</span></span> |
| <code>-s</code> | <span data-ttu-id="6fba3-356">Felhasználói beavatkozás nélküli telepítés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="6fba3-356">Perform a silent installation with no user prompts.</span></span> |
| <code>--check</code> | <span data-ttu-id="6fba3-357">Ellenőrizze az engedélyeit és hello operációs rendszer, de ne telepítse hello ügynök.</span><span class="sxs-lookup"><span data-stu-id="6fba3-357">Check permissions and hello operating system but do not install hello agent.</span></span> |

<span data-ttu-id="6fba3-358">Hello függőségi ügynök fájlok kerülnek, a következő könyvtárak hello:</span><span class="sxs-lookup"><span data-stu-id="6fba3-358">Files for hello Dependency Agent are placed in hello following directories:</span></span>

| <span data-ttu-id="6fba3-359">**Fájlok**</span><span class="sxs-lookup"><span data-stu-id="6fba3-359">**Files**</span></span> | <span data-ttu-id="6fba3-360">**Hely**</span><span class="sxs-lookup"><span data-stu-id="6fba3-360">**Location**</span></span> |
| --- | --- |
| <span data-ttu-id="6fba3-361">Alapvető fájljait</span><span class="sxs-lookup"><span data-stu-id="6fba3-361">Core files</span></span> | <span data-ttu-id="6fba3-362">/OPT/Microsoft/Dependency-Agent</span><span class="sxs-lookup"><span data-stu-id="6fba3-362">/opt/microsoft/dependency-agent</span></span> |
| <span data-ttu-id="6fba3-363">Naplófájlok</span><span class="sxs-lookup"><span data-stu-id="6fba3-363">Log files</span></span> | <span data-ttu-id="6fba3-364">/var/OPT/Microsoft/Dependency-Agent/log</span><span class="sxs-lookup"><span data-stu-id="6fba3-364">/var/opt/microsoft/dependency-agent/log</span></span> |
| <span data-ttu-id="6fba3-365">Olyan konfigurációs fájlt</span><span class="sxs-lookup"><span data-stu-id="6fba3-365">Config files</span></span> | <span data-ttu-id="6fba3-366">/ETC/OPT/Microsoft/Dependency-Agent/config</span><span class="sxs-lookup"><span data-stu-id="6fba3-366">/etc/opt/microsoft/dependency-agent/config</span></span> |
| <span data-ttu-id="6fba3-367">Végrehajtható fájlok</span><span class="sxs-lookup"><span data-stu-id="6fba3-367">Service executable files</span></span> | <span data-ttu-id="6fba3-368">/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent</span><span class="sxs-lookup"><span data-stu-id="6fba3-368">/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent</span></span><br><br><span data-ttu-id="6fba3-369">/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent-Manager</span><span class="sxs-lookup"><span data-stu-id="6fba3-369">/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager</span></span> |
| <span data-ttu-id="6fba3-370">A tároló bináris fájljai</span><span class="sxs-lookup"><span data-stu-id="6fba3-370">Binary storage files</span></span> | <span data-ttu-id="6fba3-371">/var/OPT/Microsoft/Dependency-Agent/Storage</span><span class="sxs-lookup"><span data-stu-id="6fba3-371">/var/opt/microsoft/dependency-agent/storage</span></span> |

### <a name="installation-script-examples"></a><span data-ttu-id="6fba3-372">Telepítési parancsfájl példák</span><span class="sxs-lookup"><span data-stu-id="6fba3-372">Installation script examples</span></span>

<span data-ttu-id="6fba3-373">tooeasily telepítése több kiszolgálón függőségi ügynök hello egyszerre, hanem toouse parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="6fba3-373">tooeasily deploy hello Dependency Agent on many servers at once, it helps toouse a script.</span></span> <span data-ttu-id="6fba3-374">A következő parancsfájl példák toodownload hello használja, és hello függőségi ügynök telepíthető Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="6fba3-374">You can use hello following script examples toodownload and install hello Dependency Agent on either Windows or Linux.</span></span>

#### <a name="powershell-script-for-windows"></a><span data-ttu-id="6fba3-375">A Windows PowerShell-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="6fba3-375">PowerShell script for Windows</span></span>

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a><span data-ttu-id="6fba3-376">A Linux rendszerhez parancsfájl</span><span class="sxs-lookup"><span data-stu-id="6fba3-376">Shell script for Linux</span></span>

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a><span data-ttu-id="6fba3-377">Célállapot-konfiguráló</span><span class="sxs-lookup"><span data-stu-id="6fba3-377">Desired State Configuration</span></span>

<span data-ttu-id="6fba3-378">toodeploy hello függőségi ügynök keresztül célállapot-konfiguráció, hello xPSDesiredStateConfiguration modul és a kód hasonló hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="6fba3-378">toodeploy hello Dependency Agent via Desired State Configuration, you can use hello xPSDesiredStateConfiguration module and a bit of code like hello following:</span></span>

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install hello Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-hello-dependency-agent"></a><span data-ttu-id="6fba3-379">Hello függőségi ügynök eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6fba3-379">Uninstall hello Dependency Agent</span></span>

<span data-ttu-id="6fba3-380">A következő szakaszok toohelp hello függőségi ügynök eltávolítása hello használata.</span><span class="sxs-lookup"><span data-stu-id="6fba3-380">Use hello following sections toohelp you remove hello Dependency Agent.</span></span>

#### <a name="uninstall-hello-dependency-agent-on-windows"></a><span data-ttu-id="6fba3-381">Távolítsa el a függőségi ügynök a Windows hello</span><span class="sxs-lookup"><span data-stu-id="6fba3-381">Uninstall hello Dependency Agent on Windows</span></span>

<span data-ttu-id="6fba3-382">A rendszergazda eltávolíthat hello függőségi ügynök a Windows Vezérlőpult segítségével.</span><span class="sxs-lookup"><span data-stu-id="6fba3-382">An administrator can uninstall hello Dependency Agent for Windows through Control Panel.</span></span>

<span data-ttu-id="6fba3-383">A rendszergazda %Programfiles%\Microsoft függőségi Agent\Uninstall.exe toouninstall hello függőségi ügynök is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="6fba3-383">An administrator can also run %Programfiles%\Microsoft Dependency Agent\Uninstall.exe toouninstall hello Dependency Agent.</span></span>

#### <a name="uninstall-hello-dependency-agent-on-linux"></a><span data-ttu-id="6fba3-384">Hello Linux függőségi ügynök eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6fba3-384">Uninstall hello Dependency Agent on Linux</span></span>

<span data-ttu-id="6fba3-385">toocompletely eltávolítás hello függőségi Linux-ügynököt, el kell távolítania a hello ügynök magát, és hello összekötőt, amely automatikusan telepítve van a hello ügynökkel.</span><span class="sxs-lookup"><span data-stu-id="6fba3-385">toocompletely uninstall hello Dependency Agent from Linux, you must remove hello agent itself and hello connector, which is installed automatically with hello agent.</span></span> <span data-ttu-id="6fba3-386">Eltávolíthatja, mindkettő hello egyetlen parancs a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="6fba3-386">You can uninstall both by using hello following single command:</span></span>

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a><span data-ttu-id="6fba3-387">Felügyeleti csomagok</span><span class="sxs-lookup"><span data-stu-id="6fba3-387">Management packs</span></span>

<span data-ttu-id="6fba3-388">A Naplóelemzési munkaterület átviteli adatokat aktiválásakor 300 KB-os felügyeleti csomag munkaterületről tooall hello Windows kiszolgálók küldése.</span><span class="sxs-lookup"><span data-stu-id="6fba3-388">When Wire Data is activated in a Log Analytics workspace, a 300-KB management pack is sent tooall hello Windows servers in that workspace.</span></span> <span data-ttu-id="6fba3-389">Ha a System Center Operations Manager ügynököt használ egy [csatlakoztatott felügyeleti csoport](log-analytics-om-agents.md), hello alkalmazásfüggőség-figyelő a felügyeleti csomag a System Center Operations Manager telepítésének módja.</span><span class="sxs-lookup"><span data-stu-id="6fba3-389">If you are using System Center Operations Manager agents in a [connected management group](log-analytics-om-agents.md), hello Dependency Monitor management pack is deployed from System Center Operations Manager.</span></span> <span data-ttu-id="6fba3-390">Ha hello ügynökök közvetlenül csatlakoztatott, Naplóelemzési nyújt hello felügyeleti csomag.</span><span class="sxs-lookup"><span data-stu-id="6fba3-390">If hello agents are directly connected, Log Analytics delivers hello management pack.</span></span>

<span data-ttu-id="6fba3-391">Microsoft.IntelligencePacks.ApplicationDependencyMonitor hello felügyeleti csomag neve.</span><span class="sxs-lookup"><span data-stu-id="6fba3-391">hello management pack is named Microsoft.IntelligencePacks.ApplicationDependencyMonitor.</span></span> <span data-ttu-id="6fba3-392">Az írás: %Programfiles%\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok.</span><span class="sxs-lookup"><span data-stu-id="6fba3-392">It's written to: %Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs.</span></span> <span data-ttu-id="6fba3-393">hello felügyeleti csomagot használó hello adatforrás: % Program files%\Microsoft figyelés Agent\Agent\Health szolgáltatás State\Resources&lt;AutoGeneratedID&gt;\ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.</span><span class="sxs-lookup"><span data-stu-id="6fba3-393">hello data source that hello management pack uses is: %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.</span></span>

## <a name="using-hello-solution"></a><span data-ttu-id="6fba3-394">Hello megoldással</span><span class="sxs-lookup"><span data-stu-id="6fba3-394">Using hello solution</span></span>

<span data-ttu-id="6fba3-395">**Telepítése és konfigurálása hello megoldás**</span><span class="sxs-lookup"><span data-stu-id="6fba3-395">**Installing and configuring hello solution**</span></span>

<span data-ttu-id="6fba3-396">A következő információk tooinstall hello használja, és hello megoldás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6fba3-396">Use hello following information tooinstall and configure hello solution.</span></span>

- <span data-ttu-id="6fba3-397">Átviteli adatokat megoldás hello szerez be a Windows Server 2012 R2, Windows 8.1 és újabb operációs rendszereket futtató számítógépek adatait.</span><span class="sxs-lookup"><span data-stu-id="6fba3-397">hello Wire Data solution acquires data from computers running Windows Server 2012 R2, Windows 8.1, and later operating systems.</span></span>
- <span data-ttu-id="6fba3-398">A számítógépeken, ahol azt szeretné, hogy tooacquire átviteli adatokat a Microsoft .NET-keretrendszer 4.0-s vagy újabb szükséges.</span><span class="sxs-lookup"><span data-stu-id="6fba3-398">Microsoft .NET Framework 4.0 or later is required on computers where you want tooacquire wire data from.</span></span>
- <span data-ttu-id="6fba3-399">Adja hozzá a hello átviteli adatokat megoldás tooyour Naplóelemzési munkaterület ismertetett hello eljárással [hozzáadni a Naplóelemzési megoldások hello megoldások gyűjteménye a](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="6fba3-399">Add hello Wire Data solution tooyour Log Analytics workspace using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="6fba3-400">Nincs szükség további konfigurációra.</span><span class="sxs-lookup"><span data-stu-id="6fba3-400">There is no further configuration required.</span></span>
- <span data-ttu-id="6fba3-401">Ha egy bizonyos megoldást szeretne tooview átviteli adatokat, megoldásra van szüksége toohave hello már hozzá van adva tooyour munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="6fba3-401">If you want tooview wire data for a specific solution, you need toohave hello solution already added tooyour workspace.</span></span>

<span data-ttu-id="6fba3-402">Miután telepítette az ügynököt telepíteni, és hello megoldás telepítése, a munkaterület hello átviteli adatok 2.0 csempe jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6fba3-402">After you have agents installed and you install hello solution, hello Wire Data 2.0 tile appears in your workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="6fba3-403">Jelenleg hello OMS portál tooview átviteli adatokat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="6fba3-403">Currently, you must use hello OMS portal tooview wire data.</span></span> <span data-ttu-id="6fba3-404">Hello Azure portál tooview átviteli adatokat nem használható.</span><span class="sxs-lookup"><span data-stu-id="6fba3-404">You cannot use hello Azure portal tooview wire data.</span></span>

![Átviteli adatokat csempe](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-hello-wire-data-20-solution"></a><span data-ttu-id="6fba3-406">Átviteli adatokat 2.0 hello megoldással</span><span class="sxs-lookup"><span data-stu-id="6fba3-406">Using hello Wire Data 2.0 solution</span></span>

<span data-ttu-id="6fba3-407">Az hello OMS-portálon, kattintson a hello **átviteli adatok 2.0** csempe tooopen hello átviteli adatokat irányítópult.</span><span class="sxs-lookup"><span data-stu-id="6fba3-407">In hello OMS portal, click hello **Wire Data 2.0** tile tooopen hello Wire Data dashboard.</span></span> <span data-ttu-id="6fba3-408">hello irányítópult hello paneleken hello a következő táblázat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6fba3-408">hello dashboard includes hello blades in hello following table.</span></span> <span data-ttu-id="6fba3-409">Minden egyes panel be, hogy a panel a feltételek hello megadva hatókör és a kívánt időtartományt megfelelő too10 elemeket sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="6fba3-409">Each blade lists up too10 items matching that blade's criteria for hello specified scope and time range.</span></span> <span data-ttu-id="6fba3-410">A napló keresési, amely visszaadja az összes rekord kattintva futtathatja **láthatja az összes** hello panel vagy hello panel fejléc kattintva hello alján.</span><span class="sxs-lookup"><span data-stu-id="6fba3-410">You can run a log search that returns all records by clicking **See all** at hello bottom of hello blade or by clicking hello blade header.</span></span>

| <span data-ttu-id="6fba3-411">**Panel**</span><span class="sxs-lookup"><span data-stu-id="6fba3-411">**Blade**</span></span> | <span data-ttu-id="6fba3-412">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="6fba3-412">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="6fba3-413">Hálózati forgalmat rögzítő ügynökök</span><span class="sxs-lookup"><span data-stu-id="6fba3-413">Agents capturing network traffic</span></span> | <span data-ttu-id="6fba3-414">Ügynökök, a hálózati forgalom rögzítése hello számát jeleníti meg, és hello felső 10 olyan számítógépet megjeleníti, hogy a forgalom rögzítése.</span><span class="sxs-lookup"><span data-stu-id="6fba3-414">Shows hello number of agents that are capturing network traffic and lists hello top 10 computers that are capturing traffic.</span></span> <span data-ttu-id="6fba3-415">Kattintson a szám toorun hello napló keresése <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>.</span><span class="sxs-lookup"><span data-stu-id="6fba3-415">Click hello number toorun a log search for <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>.</span></span> <span data-ttu-id="6fba3-416">Hello lista toorun számítógépen a napló kattintson a Keresés gombra visszaadó hello rögzített bájtok teljes száma.</span><span class="sxs-lookup"><span data-stu-id="6fba3-416">Click a computer in hello list toorun a log search returning hello total number of bytes captured.</span></span> |
| <span data-ttu-id="6fba3-417">Helyi alhálózatok</span><span class="sxs-lookup"><span data-stu-id="6fba3-417">Local Subnets</span></span> | <span data-ttu-id="6fba3-418">Helyi alhálózatok ügynökök észlelt hello számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6fba3-418">Shows hello number of local subnets that agents have discovered.</span></span>  <span data-ttu-id="6fba3-419">Kattintson a szám toorun hello napló keresése <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> összes alhálózatot, amely felsorolja a hello minden egyes küldött bájtok száma.</span><span class="sxs-lookup"><span data-stu-id="6fba3-419">Click hello number toorun a log search for <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> that lists all subnets with hello number of bytes sent over each one.</span></span> <span data-ttu-id="6fba3-420">Kattintson a hello lista toorun lévő alhálózatot a napló keresés visszaadó hello hello alhálózattal küldött bájtok teljes száma.</span><span class="sxs-lookup"><span data-stu-id="6fba3-420">Click a subnet in hello list toorun a log search returning hello total number of bytes sent over hello subnet.</span></span> |
| <span data-ttu-id="6fba3-421">Alkalmazásszintű protokollok</span><span class="sxs-lookup"><span data-stu-id="6fba3-421">Application-level Protocols</span></span> | <span data-ttu-id="6fba3-422">Alkalmazásszintű protokollok hello számát jeleníti meg használja, mint az ügynököket.</span><span class="sxs-lookup"><span data-stu-id="6fba3-422">Shows hello number of application-level protocols in use, as discovered by agents.</span></span> <span data-ttu-id="6fba3-423">Kattintson a szám toorun hello napló keresése <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>.</span><span class="sxs-lookup"><span data-stu-id="6fba3-423">Click hello number toorun a log search for <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>.</span></span> <span data-ttu-id="6fba3-424">A protokoll toorun a napló kattintson a Keresés gombra visszaadó hello hello protokoll használatával küldött bájtok teljes száma.</span><span class="sxs-lookup"><span data-stu-id="6fba3-424">Click a protocol toorun a log search returning hello total number of bytes sent using hello protocol.</span></span> |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Átviteli adatokat irányítópult](./media/log-analytics-wire-data/wire-data-dash.png)

<span data-ttu-id="6fba3-426">Használhatja a hello **hálózati forgalmat rögzítő ügynökök** számítógépek fogyasztja, a hálózati sávszélesség hány panel toodetermine.</span><span class="sxs-lookup"><span data-stu-id="6fba3-426">You can use hello **Agents capturing network traffic** blade toodetermine how much network bandwidth is being consumed by computers.</span></span> <span data-ttu-id="6fba3-427">Ez a panel segítségével könnyen keresés hello _chattiest_ számítógép a környezetben.</span><span class="sxs-lookup"><span data-stu-id="6fba3-427">This blade can help you easily find hello _chattiest_ computer in your environment.</span></span> <span data-ttu-id="6fba3-428">Ezek a számítógépek sikerült túlterheltté vált, rendellenesen működő, vagy további hálózati erőforrások, mint a normál használatával.</span><span class="sxs-lookup"><span data-stu-id="6fba3-428">Such computers could be overloaded, acting abnormally, or using more network resources than normal.</span></span>

![naplófájl-keresési példa](./media/log-analytics-wire-data/log-search-example01.png)

<span data-ttu-id="6fba3-430">Hasonlóképpen, használhatja a hello **helyi alhálózatok** panel toodetermine mekkora hálózati forgalom mozgatása a alhálózatokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="6fba3-430">Similarly, you can use hello **Local Subnets** blade toodetermine how much network traffic is moving through your subnets.</span></span> <span data-ttu-id="6fba3-431">Felhasználók gyakran határozza meg azt az alkalmazáshoz kritikus területeket körül alhálózatokat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-431">Users often define subnets around critical areas for their applications.</span></span> <span data-ttu-id="6fba3-432">Ezen a panelen a fenti területekre megtekintésében kínál.</span><span class="sxs-lookup"><span data-stu-id="6fba3-432">This blade offers a view into those areas.</span></span>

![naplófájl-keresési példa](./media/log-analytics-wire-data/log-search-example02.png)

<span data-ttu-id="6fba3-434">Hello **alkalmazásszintű protokollok** panel akkor hasznos, előnyös, mert tudják, mit protokollt használja.</span><span class="sxs-lookup"><span data-stu-id="6fba3-434">hello **Application-level Protocols** blade is useful because it's helpful know what protocols are in use.</span></span> <span data-ttu-id="6fba3-435">Például várt SSH toonot használatban a hálózati környezetben.</span><span class="sxs-lookup"><span data-stu-id="6fba3-435">For example, you might expect SSH toonot be in use in your network environment.</span></span> <span data-ttu-id="6fba3-436">Hello panelen elérhető adatok megtekintéséhez használatos időkategóriát gyorsan erősítse meg, és a várt eredmény disprove.</span><span class="sxs-lookup"><span data-stu-id="6fba3-436">Viewing information available in hello blade can quickly confirm or disprove your expectation.</span></span>

![naplófájl-keresési példa](./media/log-analytics-wire-data/log-search-example03.png)

<span data-ttu-id="6fba3-438">Ebben a példában lehetett-feltárás SSH részletek toosee mely számítógépeket használ SSH és sok más kommunikáció részletei.</span><span class="sxs-lookup"><span data-stu-id="6fba3-438">In this example, you could drill-into SSH details toosee which computers are using SSH and many other communication details.</span></span>

![SH keresési eredmények](./media/log-analytics-wire-data/ssh-details.png)

<span data-ttu-id="6fba3-440">Akkor is hasznos tooknow Ha növekvő vagy csökkenő időbeli protokoll forgalmat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-440">It's also useful tooknow if protocol traffic is increasing or decreasing over time.</span></span> <span data-ttu-id="6fba3-441">Például az alkalmazások által továbbított adatok hello mennyisége növekszik, ha, előfordulhat, hogy valami kell ügyelnie, vagy hogy előfordulhat, hogy a fontos.</span><span class="sxs-lookup"><span data-stu-id="6fba3-441">For example, if hello amount of data being transmitted by an application is increasing, that might be something you should be aware of, or that you might find noteworthy.</span></span>

## <a name="input-data"></a><span data-ttu-id="6fba3-442">A bemeneti adatok</span><span class="sxs-lookup"><span data-stu-id="6fba3-442">Input data</span></span>

<span data-ttu-id="6fba3-443">Átviteli adatokat gyűjt, amelyeken engedélyezve hello ügynököt használ, a hálózati forgalommal kapcsolatos metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-443">Wire data collects metadata about network traffic using hello agents that you have enabled.</span></span> <span data-ttu-id="6fba3-444">Minden ügynök 15 másodpercenként adatokat küld.</span><span class="sxs-lookup"><span data-stu-id="6fba3-444">Each agent sends data about every 15 seconds.</span></span>

## <a name="output-data"></a><span data-ttu-id="6fba3-445">kimeneti adatok</span><span class="sxs-lookup"><span data-stu-id="6fba3-445">Output data</span></span>

<span data-ttu-id="6fba3-446">A típusú rekord _WireData_ jön létre az egyes bemeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="6fba3-446">A record with a type of _WireData_ is created for each type of input data.</span></span> <span data-ttu-id="6fba3-447">WireData rekordok hello a következő táblázatban látható jellemzőkkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="6fba3-447">WireData records have properties shown in hello following table:</span></span>

| <span data-ttu-id="6fba3-448">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6fba3-448">Property</span></span> | <span data-ttu-id="6fba3-449">Leírás</span><span class="sxs-lookup"><span data-stu-id="6fba3-449">Description</span></span> |
|---|---|
| <span data-ttu-id="6fba3-450">Computer</span><span class="sxs-lookup"><span data-stu-id="6fba3-450">Computer</span></span> | <span data-ttu-id="6fba3-451">Számítógép neve, hol történt adatgyűjtés</span><span class="sxs-lookup"><span data-stu-id="6fba3-451">Computer name where data was collected</span></span> |
| <span data-ttu-id="6fba3-452">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="6fba3-452">TimeGenerated</span></span> | <span data-ttu-id="6fba3-453">Hello rekord idő</span><span class="sxs-lookup"><span data-stu-id="6fba3-453">Time of hello record</span></span> |
| <span data-ttu-id="6fba3-454">LocalIP</span><span class="sxs-lookup"><span data-stu-id="6fba3-454">LocalIP</span></span> | <span data-ttu-id="6fba3-455">Hello helyi számítógép IP-címe</span><span class="sxs-lookup"><span data-stu-id="6fba3-455">IP address of hello local computer</span></span> |
| <span data-ttu-id="6fba3-456">SessionState</span><span class="sxs-lookup"><span data-stu-id="6fba3-456">SessionState</span></span> | <span data-ttu-id="6fba3-457">Csatlakoztatva, vagy nincs csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="6fba3-457">Connected or disconnected</span></span> |
| <span data-ttu-id="6fba3-458">ReceivedBytes</span><span class="sxs-lookup"><span data-stu-id="6fba3-458">ReceivedBytes</span></span> | <span data-ttu-id="6fba3-459">Fogadott bájtok mennyiségét</span><span class="sxs-lookup"><span data-stu-id="6fba3-459">Amount of bytes received</span></span> |
| <span data-ttu-id="6fba3-460">ProtocolName</span><span class="sxs-lookup"><span data-stu-id="6fba3-460">ProtocolName</span></span> | <span data-ttu-id="6fba3-461">Hello használt hálózati protokoll neve</span><span class="sxs-lookup"><span data-stu-id="6fba3-461">Name of hello network protocol used</span></span> |
| <span data-ttu-id="6fba3-462">IPVersion</span><span class="sxs-lookup"><span data-stu-id="6fba3-462">IPVersion</span></span> | <span data-ttu-id="6fba3-463">IP-verziója</span><span class="sxs-lookup"><span data-stu-id="6fba3-463">IP version</span></span> |
| <span data-ttu-id="6fba3-464">Irány</span><span class="sxs-lookup"><span data-stu-id="6fba3-464">Direction</span></span> | <span data-ttu-id="6fba3-465">Bejövő vagy kimenő</span><span class="sxs-lookup"><span data-stu-id="6fba3-465">Inbound or outbound</span></span> |
| <span data-ttu-id="6fba3-466">MaliciousIP</span><span class="sxs-lookup"><span data-stu-id="6fba3-466">MaliciousIP</span></span> | <span data-ttu-id="6fba3-467">Egy ismert rosszindulatú forrás IP-címe</span><span class="sxs-lookup"><span data-stu-id="6fba3-467">IP address of a known malicious source</span></span> |
| <span data-ttu-id="6fba3-468">Súlyosság</span><span class="sxs-lookup"><span data-stu-id="6fba3-468">Severity</span></span> | <span data-ttu-id="6fba3-469">Gyanús kártevőt súlyossága</span><span class="sxs-lookup"><span data-stu-id="6fba3-469">Suspected malware severity</span></span> |
| <span data-ttu-id="6fba3-470">RemoteIPCountry</span><span class="sxs-lookup"><span data-stu-id="6fba3-470">RemoteIPCountry</span></span> | <span data-ttu-id="6fba3-471">Ország hello távoli IP-cím</span><span class="sxs-lookup"><span data-stu-id="6fba3-471">Country of hello remote IP address</span></span> |
| <span data-ttu-id="6fba3-472">ManagementGroupName</span><span class="sxs-lookup"><span data-stu-id="6fba3-472">ManagementGroupName</span></span> | <span data-ttu-id="6fba3-473">Hello Operations Manager felügyeleti csoport neve</span><span class="sxs-lookup"><span data-stu-id="6fba3-473">Name of hello Operations Manager management group</span></span> |
| <span data-ttu-id="6fba3-474">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="6fba3-474">SourceSystem</span></span> | <span data-ttu-id="6fba3-475">Ha adatokat gyűjtött a program forrás</span><span class="sxs-lookup"><span data-stu-id="6fba3-475">Source where data was collected</span></span> |
| <span data-ttu-id="6fba3-476">SessionStartTime</span><span class="sxs-lookup"><span data-stu-id="6fba3-476">SessionStartTime</span></span> | <span data-ttu-id="6fba3-477">A munkamenet kezdési idejét</span><span class="sxs-lookup"><span data-stu-id="6fba3-477">Start time of session</span></span> |
| <span data-ttu-id="6fba3-478">SessionEndTime</span><span class="sxs-lookup"><span data-stu-id="6fba3-478">SessionEndTime</span></span> | <span data-ttu-id="6fba3-479">Munkamenet befejezési időpontja</span><span class="sxs-lookup"><span data-stu-id="6fba3-479">End time of session</span></span> |
| <span data-ttu-id="6fba3-480">LocalSubnet</span><span class="sxs-lookup"><span data-stu-id="6fba3-480">LocalSubnet</span></span> | <span data-ttu-id="6fba3-481">Alhálózati hol történt adatgyűjtés</span><span class="sxs-lookup"><span data-stu-id="6fba3-481">Subnet where data was collected</span></span> |
| <span data-ttu-id="6fba3-482">LocalPortNumber</span><span class="sxs-lookup"><span data-stu-id="6fba3-482">LocalPortNumber</span></span> | <span data-ttu-id="6fba3-483">Helyi port száma</span><span class="sxs-lookup"><span data-stu-id="6fba3-483">Local port number</span></span> |
| <span data-ttu-id="6fba3-484">RemoteIP</span><span class="sxs-lookup"><span data-stu-id="6fba3-484">RemoteIP</span></span> | <span data-ttu-id="6fba3-485">Hello távoli számítógép által használt távoli IP-cím</span><span class="sxs-lookup"><span data-stu-id="6fba3-485">Remote IP address used by hello remote computer</span></span> |
| <span data-ttu-id="6fba3-486">RemotePortNumber</span><span class="sxs-lookup"><span data-stu-id="6fba3-486">RemotePortNumber</span></span> | <span data-ttu-id="6fba3-487">Hello távoli IP-cím által használt portszám</span><span class="sxs-lookup"><span data-stu-id="6fba3-487">Port number used by hello remote IP address</span></span> |
| <span data-ttu-id="6fba3-488">Munkamenet-azonosító</span><span class="sxs-lookup"><span data-stu-id="6fba3-488">SessionID</span></span> | <span data-ttu-id="6fba3-489">Két IP-címek közötti kommunikáció munkamenetet azonosító egyedi érték</span><span class="sxs-lookup"><span data-stu-id="6fba3-489">A unique value that identifies communication session between two IP addresses</span></span> |
| <span data-ttu-id="6fba3-490">SentBytes</span><span class="sxs-lookup"><span data-stu-id="6fba3-490">SentBytes</span></span> | <span data-ttu-id="6fba3-491">Küldött bájtok száma</span><span class="sxs-lookup"><span data-stu-id="6fba3-491">Number of bytes sent</span></span> |
| <span data-ttu-id="6fba3-492">TotalBytes</span><span class="sxs-lookup"><span data-stu-id="6fba3-492">TotalBytes</span></span> | <span data-ttu-id="6fba3-493">A munkamenetben küldött bájtok teljes száma</span><span class="sxs-lookup"><span data-stu-id="6fba3-493">Total number of bytes sent during session</span></span> |
| <span data-ttu-id="6fba3-494">ApplicationProtocol</span><span class="sxs-lookup"><span data-stu-id="6fba3-494">ApplicationProtocol</span></span> | <span data-ttu-id="6fba3-495">A használt hálózati protokoll típusa</span><span class="sxs-lookup"><span data-stu-id="6fba3-495">Type of network protocol used</span></span>   |
| <span data-ttu-id="6fba3-496">Folyamatazonosító</span><span class="sxs-lookup"><span data-stu-id="6fba3-496">ProcessID</span></span> | <span data-ttu-id="6fba3-497">Windows-folyamat azonosítója</span><span class="sxs-lookup"><span data-stu-id="6fba3-497">Windows process ID</span></span> |
| <span data-ttu-id="6fba3-498">Folyamatnév</span><span class="sxs-lookup"><span data-stu-id="6fba3-498">ProcessName</span></span> | <span data-ttu-id="6fba3-499">Hello folyamat elérési útját és nevét</span><span class="sxs-lookup"><span data-stu-id="6fba3-499">Path and file name of hello process</span></span> |
| <span data-ttu-id="6fba3-500">RemoteIPLongitude</span><span class="sxs-lookup"><span data-stu-id="6fba3-500">RemoteIPLongitude</span></span> | <span data-ttu-id="6fba3-501">IP-hosszúság érték</span><span class="sxs-lookup"><span data-stu-id="6fba3-501">IP longitude value</span></span> |
| <span data-ttu-id="6fba3-502">RemoteIPLatitude</span><span class="sxs-lookup"><span data-stu-id="6fba3-502">RemoteIPLatitude</span></span> | <span data-ttu-id="6fba3-503">IP-szélesség értéke</span><span class="sxs-lookup"><span data-stu-id="6fba3-503">IP latitude value</span></span> |


## <a name="next-steps"></a><span data-ttu-id="6fba3-504">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6fba3-504">Next steps</span></span>

- <span data-ttu-id="6fba3-505">[Naplók keresése](log-analytics-log-searches.md) tooview részletes vezetékes keresési rekordok.</span><span class="sxs-lookup"><span data-stu-id="6fba3-505">[Search logs](log-analytics-log-searches.md) tooview detailed wire data search records.</span></span>
