---
title: "a Data Factory adatkezelési átjáró aaaData |} Microsoft Docs"
description: "Egy adatok átjáró toomove adatok között a helyszíni és hello beállítása felhő. Használja az adatkezelési átjáró Azure Data Factory toomove adatait."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 6f523891a6230cbc7b407f46f4b02d91dfd2cf46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-management-gateway"></a><span data-ttu-id="b9c34-104">Adatkezelési átjáró</span><span class="sxs-lookup"><span data-stu-id="b9c34-104">Data Management Gateway</span></span>
<span data-ttu-id="b9c34-105">hello az adatkezelési átjáró egy olyan ügyfélügynök, telepítenie kell a helyszíni környezet toocopy adatok között felhő- és a helyszíni adattárolókhoz.</span><span class="sxs-lookup"><span data-stu-id="b9c34-105">hello Data management gateway is a client agent that you must install in your on-premises environment toocopy data between cloud and on-premises data stores.</span></span> <span data-ttu-id="b9c34-106">hello a helyszíni adatok adat-előállító által támogatott tárolók hello szereplő [támogatott adatforrások](data-factory-data-movement-activities.md#supported-data-stores-and-formats) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b9c34-106">hello on-premises data stores supported by Data Factory are listed in hello [Supported data sources](data-factory-data-movement-activities.md#supported-data-stores-and-formats) section.</span></span>

<span data-ttu-id="b9c34-107">Ez a cikk kiegészíti a hello hello forgatókönyv [adatok áthelyezése között a helyszíni és felhőalapú adattároló](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b9c34-107">This article complements hello walkthrough in hello [Move data between on-premises and cloud data stores](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="b9c34-108">Hello forgatókönyv hozzon létre egy folyamatot, amely egy helyi SQL Server adatbázis tooan Azure blob hello átjáró toomove adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="b9c34-108">In hello walkthrough, you create a pipeline that uses hello gateway toomove data from an on-premises SQL Server database tooan Azure blob.</span></span> <span data-ttu-id="b9c34-109">Ez a cikk hello az adatkezelési átjáró részletes részletes információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="b9c34-109">This article provides detailed in-depth information about hello data management gateway.</span></span> 

<span data-ttu-id="b9c34-110">Az adatkezelési átjáró kiterjesztése hello átjáró több helyszíni gépet társít.</span><span class="sxs-lookup"><span data-stu-id="b9c34-110">You can scale out a data management gateway by associating multiple on-premises machines with hello gateway.</span></span> <span data-ttu-id="b9c34-111">Méretezheti növelésével csomóponton egyidejűleg futtatható az adatátviteli feladatok száma legfeljebb.</span><span class="sxs-lookup"><span data-stu-id="b9c34-111">You can scale up by increasing number of data movement jobs that can run concurrently on a node.</span></span> <span data-ttu-id="b9c34-112">Ez a szolgáltatás egy logikai átjárójának egyetlen csomópont is érhető el.</span><span class="sxs-lookup"><span data-stu-id="b9c34-112">This feature is also available for a logical gateway with a single node.</span></span> <span data-ttu-id="b9c34-113">Lásd: [méretezés az adatkezelési átjáró az Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="b9c34-113">See [Scaling data management gateway in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) article for details.</span></span>

> [!NOTE]
> <span data-ttu-id="b9c34-114">Átjáró jelenleg csak a hello másolási tevékenység és a tárolt eljárási tevékenység adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b9c34-114">Currently, gateway supports only hello copy activity and stored procedure activity in Data Factory.</span></span> <span data-ttu-id="b9c34-115">Már nem lehetséges toouse hello átjáró egy egyéni tevékenység tooaccess a helyszíni adatforrásokból.</span><span class="sxs-lookup"><span data-stu-id="b9c34-115">It is not possible toouse hello gateway from a custom activity tooaccess on-premises data sources.</span></span>      

## <a name="overview"></a><span data-ttu-id="b9c34-116">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b9c34-116">Overview</span></span>
### <a name="capabilities-of-data-management-gateway"></a><span data-ttu-id="b9c34-117">Az adatkezelési átjáró képességei</span><span class="sxs-lookup"><span data-stu-id="b9c34-117">Capabilities of data management gateway</span></span>
<span data-ttu-id="b9c34-118">Az adatkezelési átjáró hello a következő lehetőségeket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b9c34-118">Data management gateway provides hello following capabilities:</span></span>

* <span data-ttu-id="b9c34-119">Modell a helyszíni adatforrások és felhő adatforrások belül azonos adat-előállító hello és áthelyezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b9c34-119">Model on-premises data sources and cloud data sources within hello same data factory and move data.</span></span>
* <span data-ttu-id="b9c34-120">Figyelési és-kezelés az átjáró állapotának hello adat-előállító oldalról lássák az üveg egytáblás rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b9c34-120">Have a single pane of glass for monitoring and management with visibility into gateway status from hello Data Factory page.</span></span>
* <span data-ttu-id="b9c34-121">Hozzáférés tooon helyszíni adatforrások biztonságosan kezelése.</span><span class="sxs-lookup"><span data-stu-id="b9c34-121">Manage access tooon-premises data sources securely.</span></span>
  * <span data-ttu-id="b9c34-122">Nincs változás toocorporate tűzfal szükséges.</span><span class="sxs-lookup"><span data-stu-id="b9c34-122">No changes required toocorporate firewall.</span></span> <span data-ttu-id="b9c34-123">Átjáró csak lehetővé teszi a kimenő HTTP-alapú kapcsolatok tooopen internet.</span><span class="sxs-lookup"><span data-stu-id="b9c34-123">Gateway only makes outbound HTTP-based connections tooopen internet.</span></span>
  * <span data-ttu-id="b9c34-124">A helyszíni adattárolókhoz a tanúsítványhoz tartozó hitelesítő adatok titkosításához.</span><span class="sxs-lookup"><span data-stu-id="b9c34-124">Encrypt credentials for your on-premises data stores with your certificate.</span></span>
* <span data-ttu-id="b9c34-125">Hatékony áthelyezni az adatokat – adatátvitel automatikus újrapróbálkozási logika párhuzamos, rugalmas toointermittent hálózati problémákat.</span><span class="sxs-lookup"><span data-stu-id="b9c34-125">Move data efficiently – data is transferred in parallel, resilient toointermittent network issues with auto retry logic.</span></span>

### <a name="command-flow-and-data-flow"></a><span data-ttu-id="b9c34-126">Parancs folyamata és adatfolyama</span><span class="sxs-lookup"><span data-stu-id="b9c34-126">Command flow and data flow</span></span>
<span data-ttu-id="b9c34-127">A másolási tevékenység toocopy adatok a helyszíni és a felhő között használatakor hello tevékenység használja egy átjáró tootransfer adatokat a helyszíni adatok forrás toocloud, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="b9c34-127">When you use a copy activity toocopy data between on-premises and cloud, hello activity uses a gateway tootransfer data from on-premises data source toocloud and vice versa.</span></span>

<span data-ttu-id="b9c34-128">Ez hello magas szintű adatok áramlását és adatátjáró Sablonhivatkozás lépései összefoglalása: ![átjáró adatfolyama](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)</span><span class="sxs-lookup"><span data-stu-id="b9c34-128">Here is hello high-level data flow for and summary of steps for copy with data gateway: ![Data flow using gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)</span></span>

1. <span data-ttu-id="b9c34-129">Adatok fejlesztői átjárót hoz létre egy Azure Data Factory használatával vagy hello [Azure-portálon](https://portal.azure.com) vagy [PowerShell-parancsmag](https://msdn.microsoft.com/library/dn820234.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9c34-129">Data developer creates a gateway for an Azure Data Factory using either hello [Azure portal](https://portal.azure.com) or [PowerShell Cmdlet](https://msdn.microsoft.com/library/dn820234.aspx).</span></span>
2. <span data-ttu-id="b9c34-130">Adatok fejlesztői hello átjáró megadásával hoz létre egy helyszíni adattároló összekapcsolt szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b9c34-130">Data developer creates a linked service for an on-premises data store by specifying hello gateway.</span></span> <span data-ttu-id="b9c34-131">Hello beállításakor társított szolgáltatás, mert adatok fejlesztő hello hitelesítő adatok beállítása a toospecify hitelesítési típusok és a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b9c34-131">As part of setting up hello linked service, data developer uses hello Setting Credentials application toospecify authentication types and credentials.</span></span>  <span data-ttu-id="b9c34-132">hello hitelesítő adatok beállítása a alkalmazás párbeszédpanel kommunikál hello adatok tootest kapcsolat és hello átjáró toosave hitelesítő adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="b9c34-132">hello Setting Credentials application dialog communicates with hello data store tootest connection and hello gateway toosave credentials.</span></span>
3. <span data-ttu-id="b9c34-133">Átjáró hello hitelesítő adatok (adatok fejlesztő megadva), hello átjáróhoz társított hello tanúsítvánnyal hello felhőben hello hitelesítő adatok mentése előtt titkosítja.</span><span class="sxs-lookup"><span data-stu-id="b9c34-133">Gateway encrypts hello credentials with hello certificate associated with hello gateway (supplied by data developer), before saving hello credentials in hello cloud.</span></span>
4. <span data-ttu-id="b9c34-134">Data Factory szolgáltatásnak hello átjáró ütemezés & a felügyeleti feladatok egy közös Azure service bus-üzenetsort használó vezérlő csatornán keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="b9c34-134">Data Factory service communicates with hello gateway for scheduling & management of jobs via a control channel that uses a shared Azure service bus queue.</span></span> <span data-ttu-id="b9c34-135">Ha a másolási tevékenység feladat toobe kezdődött el, a Data Factory várólisták hello kérelem együtt hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b9c34-135">When a copy activity job needs toobe kicked off, Data Factory queues hello request along with credential information.</span></span> <span data-ttu-id="b9c34-136">Átjáró másolattól hello feladat lekérdezési hello várólista után.</span><span class="sxs-lookup"><span data-stu-id="b9c34-136">Gateway kicks off hello job after polling hello queue.</span></span>
5. <span data-ttu-id="b9c34-137">hello átjáró visszafejti hello azonos tanúsítványt, és csatlakoztatja a megfelelő hitelesítési típus és a hitelesítő adatok toohello helyszíni adattár hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b9c34-137">hello gateway decrypts hello credentials with hello same certificate and then connects toohello on-premises data store with proper authentication type and credentials.</span></span>
6. <span data-ttu-id="b9c34-138">hello átjáró másolja az adatokat a helyszíni tárolási tooa felhő tárolóból, vagy fordítva attól függően, hogy a másolási tevékenység hello hello adatok feldolgozási konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="b9c34-138">hello gateway copies data from an on-premises store tooa cloud storage, or vice versa depending on how hello Copy Activity is configured in hello data pipeline.</span></span> <span data-ttu-id="b9c34-139">Ebben a lépésben hello átjáró közvetlenül kommunikál a felhőalapú tárolási szolgáltatások például az Azure Blob Storage egy biztonságos csatornán (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="b9c34-139">For this step, hello gateway directly communicates with cloud-based storage services such as Azure Blob Storage over a secure (HTTPS) channel.</span></span>

### <a name="considerations-for-using-gateway"></a><span data-ttu-id="b9c34-140">Átjáró használatának szempontjai</span><span class="sxs-lookup"><span data-stu-id="b9c34-140">Considerations for using gateway</span></span>
* <span data-ttu-id="b9c34-141">Az adatkezelési átjáró egyetlen példányán több helyszíni adatforrások használható.</span><span class="sxs-lookup"><span data-stu-id="b9c34-141">A single instance of data management gateway can be used for multiple on-premises data sources.</span></span> <span data-ttu-id="b9c34-142">Azonban **átjáró egyetlen példányán a feltételekhez tooonly egy az Azure data factory** és nem lehet megosztani az egy másik data factoryvel.</span><span class="sxs-lookup"><span data-stu-id="b9c34-142">However, **a single gateway instance is tied tooonly one Azure data factory** and cannot be shared with another data factory.</span></span>
* <span data-ttu-id="b9c34-143">Akkor is **csak egy példányát az adatkezelési átjáró** egy gépen telepítve.</span><span class="sxs-lookup"><span data-stu-id="b9c34-143">You can have **only one instance of data management gateway** installed on a single machine.</span></span> <span data-ttu-id="b9c34-144">Tegyük fel, amelyeket a helyszíni adatforrások tooaccess két adat-előállítók rendelkezik, a két helyszíni számítógépeken tooinstall átjárók szüksége van.</span><span class="sxs-lookup"><span data-stu-id="b9c34-144">Suppose, you have two data factories that need tooaccess on-premises data sources, you need tooinstall gateways on two on-premises computers.</span></span> <span data-ttu-id="b9c34-145">Ez azt jelenti az átjáró az adott adat-előállító kapcsolt tooa</span><span class="sxs-lookup"><span data-stu-id="b9c34-145">In other words, a gateway is tied tooa specific data factory</span></span>
* <span data-ttu-id="b9c34-146">Hello **átjáró nem kell azonos számítógépre hello adatforrásként hello a toobe**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-146">hello **gateway does not need toobe on hello same machine as hello data source**.</span></span> <span data-ttu-id="b9c34-147">Azonban szorosabb toohello adatforrás átjáró csökkenti a hello idő hello átjáró tooconnect toohello az adatforrásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="b9c34-147">However, having gateway closer toohello data source reduces hello time for hello gateway tooconnect toohello data source.</span></span> <span data-ttu-id="b9c34-148">Azt javasoljuk, hogy a gép, amely egy adott adatforrást tartalmaz a helyszíni hello különböző hello átjáró telepítése.</span><span class="sxs-lookup"><span data-stu-id="b9c34-148">We recommend that you install hello gateway on a machine that is different from hello one that hosts on-premises data source.</span></span> <span data-ttu-id="b9c34-149">Ha hello átjáró és az adatforrás a különböző gépeken, hello átjáró nem "versenyeznek" az adatforrás erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b9c34-149">When hello gateway and data source are on different machines, hello gateway does not compete for resources with data source.</span></span>
* <span data-ttu-id="b9c34-150">Akkor is **több átjáró különböző gépeken toohello csatlakozás egy helyszíni adatforrás**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-150">You can have **multiple gateways on different machines connecting toohello same on-premises data source**.</span></span> <span data-ttu-id="b9c34-151">Például előfordulhat, hogy két átjáró két adat-előállítók, de egy helyszíni adatforrás regisztrálva van az mindkét hello adat-előállítók hello szolgál.</span><span class="sxs-lookup"><span data-stu-id="b9c34-151">For example, you may have two gateways serving two data factories but hello same on-premises data source is registered with both hello data factories.</span></span>
* <span data-ttu-id="b9c34-152">Ha már van egy átjáró telepíthető a számítógép szolgál egy **Power BI** esetben telepítése egy **az Azure Data Factory külön átjáró** egy másik számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b9c34-152">If you already have a gateway installed on your computer serving a **Power BI** scenario, install a **separate gateway for Azure Data Factory** on another machine.</span></span>
* <span data-ttu-id="b9c34-153">Átjáró kell használni, még akkor is, ha használ **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-153">Gateway must be used even when you use **ExpressRoute**.</span></span>
* <span data-ttu-id="b9c34-154">Az adatforrás tekinti egy helyszíni adatforrás (tűzfal mögött van) is használatos **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-154">Treat your data source as an on-premises data source (that is behind a firewall) even when you use **ExpressRoute**.</span></span> <span data-ttu-id="b9c34-155">Használja a tooestablish átjárókapcsolat hello hello szolgáltatást és hello adatforrás között.</span><span class="sxs-lookup"><span data-stu-id="b9c34-155">Use hello gateway tooestablish connectivity between hello service and hello data source.</span></span>
* <span data-ttu-id="b9c34-156">Meg kell **hello átjáró** akkor is, ha a hello felhőben van hello adattár egy **Azure IaaS virtuális**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-156">You must **use hello gateway** even if hello data store is in hello cloud on an **Azure IaaS VM**.</span></span>

## <a name="installation"></a><span data-ttu-id="b9c34-157">Telepítés</span><span class="sxs-lookup"><span data-stu-id="b9c34-157">Installation</span></span>
### <a name="prerequisites"></a><span data-ttu-id="b9c34-158">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b9c34-158">Prerequisites</span></span>
* <span data-ttu-id="b9c34-159">hello támogatott **operációs rendszer** azok Windows 7, Windows 8 vagy 8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 verziók.</span><span class="sxs-lookup"><span data-stu-id="b9c34-159">hello supported **Operating System** versions are Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2.</span></span> <span data-ttu-id="b9c34-160">Hello az adatkezelési átjáró egy olyan tartományvezérlő telepítése jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b9c34-160">Installation of hello data management gateway on a domain controller is currently not supported.</span></span>
* <span data-ttu-id="b9c34-161">.NET-keretrendszer 4.5.1-es vagy újabb verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="b9c34-161">.NET Framework 4.5.1 or above is required.</span></span> <span data-ttu-id="b9c34-162">Ha átjáró a Windows 7 számítógépen telepíti, telepítse a .NET-keretrendszer 4.5-ös vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="b9c34-162">If you are installing gateway on a Windows 7 machine, install .NET Framework 4.5 or later.</span></span> <span data-ttu-id="b9c34-163">Lásd: [.NET-keretrendszer rendszerkövetelmények](https://msdn.microsoft.com/library/8z6watww.aspx) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="b9c34-163">See [.NET Framework System Requirements](https://msdn.microsoft.com/library/8z6watww.aspx) for details.</span></span>
* <span data-ttu-id="b9c34-164">ajánlott hello **konfigurációs** hello átjáró számítógépén legalább 2 GHz, 4 mag, 8 GB RAM és 80 GB-os lemez van.</span><span class="sxs-lookup"><span data-stu-id="b9c34-164">hello recommended **configuration** for hello gateway machine is at least 2 GHz, 4 cores, 8-GB RAM, and 80-GB disk.</span></span>
* <span data-ttu-id="b9c34-165">Ha hello gazdaszámítógépen hibernálás hello átjáró toodata kérelmek nem válaszol.</span><span class="sxs-lookup"><span data-stu-id="b9c34-165">If hello host machine hibernates, hello gateway does not respond toodata requests.</span></span> <span data-ttu-id="b9c34-166">Ezért, konfigurálja a megfelelő **energiaséma** hello számítógépen hello-átjáró telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="b9c34-166">Therefore, configure an appropriate **power plan** on hello computer before installing hello gateway.</span></span> <span data-ttu-id="b9c34-167">Ha hello gép konfigurált toohibernate, hello átjáró telepítésének megadását kéri az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="b9c34-167">If hello machine is configured toohibernate, hello gateway installation prompts a message.</span></span>
* <span data-ttu-id="b9c34-168">Rendszergazdaként bejelentkezve hello gép tooinstall és hello az adatkezelési átjáró sikeresen konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="b9c34-168">You must be an administrator on hello machine tooinstall and configure hello data management gateway successfully.</span></span> <span data-ttu-id="b9c34-169">Hozzáadhat további felhasználók toohello **az adatkezelési átjáró felhasználók** helyi Windows-csoport.</span><span class="sxs-lookup"><span data-stu-id="b9c34-169">You can add additional users toohello **data management gateway Users** local Windows group.</span></span> <span data-ttu-id="b9c34-170">hello e csoport tagjai képes toouse hello **az adatkezelési átjáró konfigurációkezelőjének** eszköz tooconfigure hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="b9c34-170">hello members of this group are able toouse hello **Data Management Gateway Configuration Manager** tool tooconfigure hello gateway.</span></span>

<span data-ttu-id="b9c34-171">Az adott gyakoriságát a hiba akkor fordulhat elő másolja a tevékenység fut, mint hello Erőforrás kihasználtsága (Processzor, memória) hello gépen is a következő hello azonos mintát csúcs és üresjárati idő.</span><span class="sxs-lookup"><span data-stu-id="b9c34-171">As copy activity runs happen on a specific frequency, hello resource usage (CPU, memory) on hello machine also follows hello same pattern with peak and idle times.</span></span> <span data-ttu-id="b9c34-172">Erőforrás-használat is függ, erősen hello áthelyezett adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="b9c34-172">Resource utilization also depends heavily on hello amount of data being moved.</span></span> <span data-ttu-id="b9c34-173">Ha több másolási feladat van folyamatban, megjelenik az Erőforrás kihasználtsága csúcsidőben feljebb.</span><span class="sxs-lookup"><span data-stu-id="b9c34-173">When multiple copy jobs are in progress, you see resource usage go up during peak times.</span></span>

### <a name="installation-options"></a><span data-ttu-id="b9c34-174">Telepítési beállítások</span><span class="sxs-lookup"><span data-stu-id="b9c34-174">Installation options</span></span>
<span data-ttu-id="b9c34-175">Az adatkezelési átjáró hello a következő módokon telepíthető:</span><span class="sxs-lookup"><span data-stu-id="b9c34-175">Data management gateway can be installed in hello following ways:</span></span>

* <span data-ttu-id="b9c34-176">Az MSI-telepítő csomag letöltésére hello által [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span><span class="sxs-lookup"><span data-stu-id="b9c34-176">By downloading an MSI setup package from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span></span>  <span data-ttu-id="b9c34-177">hello MSI is használt tooupgrade meglévő felügyeleti adatátjáró toohello legújabb verziója, az összes beállítás megőrzi.</span><span class="sxs-lookup"><span data-stu-id="b9c34-177">hello MSI can also be used tooupgrade existing data management gateway toohello latest version, with all settings preserved.</span></span>
* <span data-ttu-id="b9c34-178">Kattintva **töltse le és telepítse az adatátjáró** manuális telepítés alatt vagy **telepíthető közvetlenül a számítógépen** EXPRESSZ telepítés alatt.</span><span class="sxs-lookup"><span data-stu-id="b9c34-178">By clicking **Download and install data gateway** link under MANUAL SETUP or **Install directly on this computer** under EXPRESS SETUP.</span></span> <span data-ttu-id="b9c34-179">Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit a gyors telepítés használatával.</span><span class="sxs-lookup"><span data-stu-id="b9c34-179">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on using express setup.</span></span> <span data-ttu-id="b9c34-180">hello manuális lépés viszi toohello letöltőközpontból.</span><span class="sxs-lookup"><span data-stu-id="b9c34-180">hello manual step takes you toohello download center.</span></span>  <span data-ttu-id="b9c34-181">hello utasításokat letöltése és telepítése hello átjáró letöltőközpontból hello a következő szakaszban találhatók.</span><span class="sxs-lookup"><span data-stu-id="b9c34-181">hello instructions for downloading and installing hello gateway from download center are in hello next section.</span></span>

### <a name="installation-best-practices"></a><span data-ttu-id="b9c34-182">Gyakorlati tanácsok a telepítéshez:</span><span class="sxs-lookup"><span data-stu-id="b9c34-182">Installation best practices:</span></span>
1. <span data-ttu-id="b9c34-183">Energiaséma konfigurálása, hello gazdagépen hello átjáró számára, hogy hello gép hibernálásra nem.</span><span class="sxs-lookup"><span data-stu-id="b9c34-183">Configure power plan on hello host machine for hello gateway so that hello machine does not hibernate.</span></span> <span data-ttu-id="b9c34-184">Ha hello gazdaszámítógépen hibernálás hello átjáró toodata kérelmek nem válaszol.</span><span class="sxs-lookup"><span data-stu-id="b9c34-184">If hello host machine hibernates, hello gateway does not respond toodata requests.</span></span>
2. <span data-ttu-id="b9c34-185">Készítsen biztonsági másolatot hello átjáró társított hello tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="b9c34-185">Back up hello certificate associated with hello gateway.</span></span>

### <a name="install-hello-gateway-from-download-center"></a><span data-ttu-id="b9c34-186">A letöltőközpontból hello átjáró telepítése</span><span class="sxs-lookup"><span data-stu-id="b9c34-186">Install hello gateway from download center</span></span>
1. <span data-ttu-id="b9c34-187">Keresse meg a túl[Microsoft adatkezelési átjáró letöltési oldala](https://www.microsoft.com/download/details.aspx?id=39717).</span><span class="sxs-lookup"><span data-stu-id="b9c34-187">Navigate too[Microsoft Data Management Gateway download page](https://www.microsoft.com/download/details.aspx?id=39717).</span></span>
2. <span data-ttu-id="b9c34-188">Kattintson a **letöltése**, válassza ki a megfelelő verzió hello (**32 bites** vs. **64 bites**), és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-188">Click **Download**, select hello appropriate version (**32-bit** vs. **64-bit**), and click **Next**.</span></span>
3. <span data-ttu-id="b9c34-189">Futtassa a hello **MSI** közvetlenül vagy tooyour merevlemezre mentheti, és futtassa.</span><span class="sxs-lookup"><span data-stu-id="b9c34-189">Run hello **MSI** directly or save it tooyour hard disk and run.</span></span>
4. <span data-ttu-id="b9c34-190">A hello **üdvözlő** lapon jelölje be a **nyelvi** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-190">On hello **Welcome** page, select a **language** click **Next**.</span></span>
5. <span data-ttu-id="b9c34-191">**Fogadja el** hello a végfelhasználói licencszerződést, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-191">**Accept** hello End-User License Agreement and click **Next**.</span></span>
6. <span data-ttu-id="b9c34-192">Válassza ki **mappa** tooinstall hello átjáró, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-192">Select **folder** tooinstall hello gateway and click **Next**.</span></span>
7. <span data-ttu-id="b9c34-193">A hello **készen tooinstall** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-193">On hello **Ready tooinstall** page, click **Install**.</span></span>
8. <span data-ttu-id="b9c34-194">Kattintson a **Befejezés** toocomplete telepítését.</span><span class="sxs-lookup"><span data-stu-id="b9c34-194">Click **Finish** toocomplete installation.</span></span>
9. <span data-ttu-id="b9c34-195">Hello kulcs beszerzése hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b9c34-195">Get hello key from hello Azure portal.</span></span> <span data-ttu-id="b9c34-196">Című rész hello következő lépéseit.</span><span class="sxs-lookup"><span data-stu-id="b9c34-196">See hello next section for step-by-step instructions.</span></span>
10. <span data-ttu-id="b9c34-197">A hello **Register átjáró** oldalán **az adatkezelési átjáró konfigurációkezelőjének** fut a gépen hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b9c34-197">On hello **Register gateway** page of **Data Management Gateway Configuration Manager** running on your machine, do hello following steps:</span></span>
    1. <span data-ttu-id="b9c34-198">Beillesztés hello kulcs hello szöveg.</span><span class="sxs-lookup"><span data-stu-id="b9c34-198">Paste hello key in hello text.</span></span>
    2. <span data-ttu-id="b9c34-199">Ha úgy gondolja, a **megjelenítése átjárókulcs** toosee hello kulcs szövegét.</span><span class="sxs-lookup"><span data-stu-id="b9c34-199">Optionally, click **Show gateway key** toosee hello key text.</span></span>
    3. <span data-ttu-id="b9c34-200">Kattintson a **regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-200">Click **Register**.</span></span>

### <a name="register-gateway-using-key"></a><span data-ttu-id="b9c34-201">Regisztrálja az átjárót kulcsával.</span><span class="sxs-lookup"><span data-stu-id="b9c34-201">Register gateway using key</span></span>
#### <a name="if-you-havent-already-created-a-logical-gateway-in-hello-portal"></a><span data-ttu-id="b9c34-202">Ha még nem hozott létre egy logikai átjáró hello portálon</span><span class="sxs-lookup"><span data-stu-id="b9c34-202">If you haven't already created a logical gateway in hello portal</span></span>
<span data-ttu-id="b9c34-203">hello hello portál és a get-hello kulcs egy átjárót toocreate **konfigurálása** lapon, a hello forgatókönyv lépéseit kövesse [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b9c34-203">toocreate a gateway in hello portal and get hello key from hello **Configure** page, Follow steps from walkthrough in hello [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>    

#### <a name="if-you-have-already-created-hello-logical-gateway-in-hello-portal"></a><span data-ttu-id="b9c34-204">Ha már létrehozott logikai átjáró hello hello portálon</span><span class="sxs-lookup"><span data-stu-id="b9c34-204">If you have already created hello logical gateway in hello portal</span></span>
1. <span data-ttu-id="b9c34-205">Azure-portálon lépjen a toohello **adat-előállító** lapon, majd kattintson **összekapcsolt szolgáltatások** csempére.</span><span class="sxs-lookup"><span data-stu-id="b9c34-205">In Azure portal, navigate toohello **Data Factory** page, and click **Linked Services** tile.</span></span>

    ![Data Factory lap](media/data-factory-data-management-gateway/data-factory-blade.png)
2. <span data-ttu-id="b9c34-207">A hello **összekapcsolt szolgáltatások** lapon, jelölje be hello logikai **átjáró** hello portálon létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b9c34-207">In hello **Linked Services** page, select hello logical **gateway** you created in hello portal.</span></span>

    ![logikai átjáró](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. <span data-ttu-id="b9c34-209">A hello **Data Gateway** kattintson **töltse le és telepítse az adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-209">In hello **Data Gateway** page, click **Download and install data gateway**.</span></span>

    ![Töltse le a hivatkozás hello portálon](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. <span data-ttu-id="b9c34-211">A hello **konfigurálása** kattintson **hozza létre újra kulcs**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-211">In hello **Configure** page, click **Recreate key**.</span></span> <span data-ttu-id="b9c34-212">Kattintson az Igen gombra a hello figyelmeztető üzenet, gondosan elolvasása után.</span><span class="sxs-lookup"><span data-stu-id="b9c34-212">Click Yes on hello warning message after reading it carefully.</span></span>

    ![Kulcs újbóli létrehozása](media/data-factory-data-management-gateway/recreate-key-button.png)
5. <span data-ttu-id="b9c34-214">Kattintson a Másolás gombra következő toohello kulcs.</span><span class="sxs-lookup"><span data-stu-id="b9c34-214">Click Copy button next toohello key.</span></span> <span data-ttu-id="b9c34-215">hello kulcsa a vágólapra másolt toohello.</span><span class="sxs-lookup"><span data-stu-id="b9c34-215">hello key is copied toohello clipboard.</span></span>

    ![Kulcs másolása](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a><span data-ttu-id="b9c34-217">Rendszer tálcaikonok / értesítések</span><span class="sxs-lookup"><span data-stu-id="b9c34-217">System tray icons/ notifications</span></span>
<span data-ttu-id="b9c34-218">hello következő kép bemutatja, néhány hello tálcaikonok látható.</span><span class="sxs-lookup"><span data-stu-id="b9c34-218">hello following image shows some of hello tray icons that you see.</span></span>

![rendszer Tálcaikonok igazítása](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

<span data-ttu-id="b9c34-220">Ha kurzor hello rendszer tálcai ikon/értesítési üzenetet, lásd: hello átjáró/frissítés műveletet egy felugró ablakban hello állapotának adatait.</span><span class="sxs-lookup"><span data-stu-id="b9c34-220">If you move cursor over hello system tray icon/notification message, you see details about hello state of hello gateway/update operation in a popup window.</span></span>

### <a name="ports-and-firewall"></a><span data-ttu-id="b9c34-221">Portok és a tűzfalon</span><span class="sxs-lookup"><span data-stu-id="b9c34-221">Ports and firewall</span></span>
<span data-ttu-id="b9c34-222">Két tűzfal tooconsider szüksége van: **vállalati tűzfal** hello szervezet hello központi útválasztón futó és **Windows tűzfal** konfigurálva démonként hello helyi számítógépen, ahol hello átjáró telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b9c34-222">There are two firewalls you need tooconsider: **corporate firewall** running on hello central router of hello organization, and **Windows firewall** configured as a daemon on hello local machine where hello gateway is installed.</span></span>  

![tűzfalak](./media/data-factory-data-management-gateway/firewalls2.png)

<span data-ttu-id="b9c34-224">Vállalati tűzfal szinten, konfigurálnia kell a hello következő tartományok és a kimenő portok:</span><span class="sxs-lookup"><span data-stu-id="b9c34-224">At corporate firewall level, you need configure hello following domains and outbound ports:</span></span>

| <span data-ttu-id="b9c34-225">Tartománynevek</span><span class="sxs-lookup"><span data-stu-id="b9c34-225">Domain names</span></span> | <span data-ttu-id="b9c34-226">Portok</span><span class="sxs-lookup"><span data-stu-id="b9c34-226">Ports</span></span> | <span data-ttu-id="b9c34-227">Leírás</span><span class="sxs-lookup"><span data-stu-id="b9c34-227">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b9c34-228">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="b9c34-228">*.servicebus.windows.net</span></span> |<span data-ttu-id="b9c34-229">443, 80</span><span class="sxs-lookup"><span data-stu-id="b9c34-229">443, 80</span></span> |<span data-ttu-id="b9c34-230">Az adatátviteli szolgáltatás háttér-kommunikációhoz használt</span><span class="sxs-lookup"><span data-stu-id="b9c34-230">Used for communication with Data Movement Service backend</span></span> |
| <span data-ttu-id="b9c34-231">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="b9c34-231">*.core.windows.net</span></span> |<span data-ttu-id="b9c34-232">443</span><span class="sxs-lookup"><span data-stu-id="b9c34-232">443</span></span> |<span data-ttu-id="b9c34-233">Használt előkészített másolása Azure Blob használatával (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="b9c34-233">Used for Staged copy using Azure Blob (if configured)</span></span>|
| <span data-ttu-id="b9c34-234">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="b9c34-234">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="b9c34-235">443</span><span class="sxs-lookup"><span data-stu-id="b9c34-235">443</span></span> |<span data-ttu-id="b9c34-236">Az adatátviteli szolgáltatás háttér-kommunikációhoz használt</span><span class="sxs-lookup"><span data-stu-id="b9c34-236">Used for communication with Data Movement Service backend</span></span> |


<span data-ttu-id="b9c34-237">A Windows tűzfal szinten a kimenő portok általában engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="b9c34-237">At Windows firewall level, these outbound ports are normally enabled.</span></span> <span data-ttu-id="b9c34-238">Ha nem, konfigurálható hello tartományok és portok ennek megfelelően az átjárót működtető gépen.</span><span class="sxs-lookup"><span data-stu-id="b9c34-238">If not, you can configure hello domains and ports accordingly on gateway machine.</span></span>

> [!NOTE]
> 1. <span data-ttu-id="b9c34-239">A forrás alapján / felül, előfordulhat, hogy toowhitelist további tartományokkal és a kimenő portok a vállalati és a Windows tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="b9c34-239">Based on your source/ sinks, you may have toowhitelist additional domains and outbound ports in your corporate/Windows firewall.</span></span>
> 2. <span data-ttu-id="b9c34-240">Az egyes felhőalapú adatbázisok (például: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access)stb), előfordulhat, hogy a tűzfal konfigurációját az átjárót működtető gépen toowhitelist IP-címét.</span><span class="sxs-lookup"><span data-stu-id="b9c34-240">For some Cloud Databases (For example: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access), etc.), you may need toowhitelist IP address of Gateway machine on their firewall configuration.</span></span>
>
>


#### <a name="copy-data-from-a-source-data-store-tooa-sink-data-store"></a><span data-ttu-id="b9c34-241">Adatok másolása egy forrás adatokat tároló tooa fogadó adatokat tároló</span><span class="sxs-lookup"><span data-stu-id="b9c34-241">Copy data from a source data store tooa sink data store</span></span>
<span data-ttu-id="b9c34-242">Győződjön meg arról, hogy hello tűzfalszabályai engedélyezve vannak a megfelelő hello vállalati tűzfal, a Windows tűzfal hello-átjáró számítógépén, hello adattároló magát.</span><span class="sxs-lookup"><span data-stu-id="b9c34-242">Ensure that hello firewall rules are enabled properly on hello corporate firewall, Windows firewall on hello gateway machine, and hello data store itself.</span></span> <span data-ttu-id="b9c34-243">Ezek a szabályok lehetővé teszi, hogy átjáró tooconnect tooboth forrás hello és gyűjtése sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="b9c34-243">Enabling these rules allows hello gateway tooconnect tooboth source and sink successfully.</span></span> <span data-ttu-id="b9c34-244">Szabályok minden adattároló, amely részt vesz hello másolási műveletek engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b9c34-244">Enable rules for each data store that is involved in hello copy operation.</span></span>

<span data-ttu-id="b9c34-245">Például a toocopy **egy a helyszíni adatokat tároló tooan Azure SQL Database fogadó vagy egy Azure SQL Data Warehouse fogadó**, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b9c34-245">For example, toocopy from **an on-premises data store tooan Azure SQL Database sink or an Azure SQL Data Warehouse sink**, do hello following steps:</span></span>

* <span data-ttu-id="b9c34-246">Kimenő forgalom engedélyezése **TCP** kommunikációs port **1433** a Windows tűzfal és a vállalati tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="b9c34-246">Allow outbound **TCP** communication on port **1433** for both Windows firewall and corporate firewall.</span></span>
* <span data-ttu-id="b9c34-247">Azure SQL server tooadd hello IP-cím hello átjáró gép toohello listájának engedélyezett IP-címek hello tűzfal beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b9c34-247">Configure hello firewall settings of Azure SQL server tooadd hello IP address of hello gateway machine toohello list of allowed IP addresses.</span></span>

> [!NOTE]
> <span data-ttu-id="b9c34-248">Ha a tűzfal nem engedélyezi a 1433-as kimenő port, átjáró közvetlenül Azure SQL nem tud hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="b9c34-248">If your firewall does not allow outbound port 1433, Gateway can't access Azure SQL directly.</span></span> <span data-ttu-id="b9c34-249">Ebben az esetben használhatja [előkészített másolási](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) tooSQL Azure-adatbázis / SQL Azure DW.</span><span class="sxs-lookup"><span data-stu-id="b9c34-249">In this case, you may use [Staged Copy](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) tooSQL Azure Database/ SQL Azure DW.</span></span> <span data-ttu-id="b9c34-250">Ebben a forgatókönyvben csak hello adatátvitelt jelölik a lenne szükséges HTTPS (443-as port).</span><span class="sxs-lookup"><span data-stu-id="b9c34-250">In this scenario, you would only require HTTPS (port 443) for hello data movement.</span></span>
>
>


### <a name="proxy-server-considerations"></a><span data-ttu-id="b9c34-251">Proxy server szempontjai</span><span class="sxs-lookup"><span data-stu-id="b9c34-251">Proxy server considerations</span></span>
<span data-ttu-id="b9c34-252">Ha a vállalati hálózati környezet proxy server tooaccess hello internet, felügyeleti átjáró toouse megfelelő proxy beállításainak megadása.</span><span class="sxs-lookup"><span data-stu-id="b9c34-252">If your corporate network environment uses a proxy server tooaccess hello internet, configure data management gateway toouse appropriate proxy settings.</span></span> <span data-ttu-id="b9c34-253">Hello proxy hello kezdeti regisztrációs fázis során állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="b9c34-253">You can set hello proxy during hello initial registration phase.</span></span>

![Set proxy regisztráció során](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

<span data-ttu-id="b9c34-255">Átjáró hello proxy server tooconnect toohello felhőalapú szolgáltatást használ.</span><span class="sxs-lookup"><span data-stu-id="b9c34-255">Gateway uses hello proxy server tooconnect toohello cloud service.</span></span> <span data-ttu-id="b9c34-256">Kattintson a **módosítás** hivatkozás kezdeti beállítás során.</span><span class="sxs-lookup"><span data-stu-id="b9c34-256">Click **Change** link during initial setup.</span></span> <span data-ttu-id="b9c34-257">Megjelenik a hello **proxybeállítást** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b9c34-257">You see hello **proxy setting** dialog.</span></span>

![A Konfigurációkezelő használatával állítsa proxy](media/data-factory-data-management-gateway/SetProxySettings.png)

<span data-ttu-id="b9c34-259">Három konfigurációs lehetőség áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="b9c34-259">There are three configuration options:</span></span>

* <span data-ttu-id="b9c34-260">**Ne használjon proxyt**: átjáró nincs explicit módon bármely proxy tooconnect toocloud szolgáltatást használni.</span><span class="sxs-lookup"><span data-stu-id="b9c34-260">**Do not use proxy**: Gateway does not explicitly use any proxy tooconnect toocloud services.</span></span>
* <span data-ttu-id="b9c34-261">**Használja a rendszer proxy**: átjáró hello proxybeállítást, amely a konfigurált diahost.exe.config és diawp.exe.config használja.  Ha nincs olyan proxy diahost.exe.config és diawp.exe.config van konfigurálva, az átjáró csatlakozik toocloud szolgáltatás közvetlenül a proxy áthaladás nélkül.</span><span class="sxs-lookup"><span data-stu-id="b9c34-261">**Use system proxy**: Gateway uses hello proxy setting that is configured in diahost.exe.config and diawp.exe.config.  If no proxy is configured in diahost.exe.config and diawp.exe.config, gateway connects toocloud service directly without going through proxy.</span></span>
* <span data-ttu-id="b9c34-262">**Egyéni proxyt használ**: konfigurálása hello HTTP proxy beállítás toouse átjáró diahost.exe.config és diawp.exe.config konfigurációk használata helyett.  Cím és Port is szükséges.</span><span class="sxs-lookup"><span data-stu-id="b9c34-262">**Use custom proxy**: Configure hello HTTP proxy setting toouse for gateway, instead of using configurations in diahost.exe.config and diawp.exe.config.  Address and Port are required.</span></span>  <span data-ttu-id="b9c34-263">Felhasználónév és jelszó megadása nem kötelező, attól függően, hogy a proxy hitelesítési beállítást.</span><span class="sxs-lookup"><span data-stu-id="b9c34-263">User Name and Password are optional depending on your proxy’s authentication setting.</span></span>  <span data-ttu-id="b9c34-264">Minden beállítás hello átjáró hitelesítő tanúsítványa hello titkosítva, és helyben tárolja, hello átjáró gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="b9c34-264">All settings are encrypted with hello credential certificate of hello gateway and stored locally on hello gateway host machine.</span></span>

<span data-ttu-id="b9c34-265">hello az adatkezelési átjáró gazdaszolgáltatás a frissített hello proxybeállítások mentése után automatikusan újraindul.</span><span class="sxs-lookup"><span data-stu-id="b9c34-265">hello data management gateway Host Service restarts automatically after you save hello updated proxy settings.</span></span>

<span data-ttu-id="b9c34-266">Miután az átjáró sikeresen regisztrálva van, ha szeretné, hogy tooview vagy frissíteni a proxykiszolgáló beállításait, használja az adatkezelési átjáró konfigurációkezelőjének.</span><span class="sxs-lookup"><span data-stu-id="b9c34-266">After gateway has been successfully registered, if you want tooview or update proxy settings, use Data Management Gateway Configuration Manager.</span></span>

1. <span data-ttu-id="b9c34-267">Indítsa el **az adatkezelési átjáró konfigurációkezelőjének**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-267">Launch **Data Management Gateway Configuration Manager**.</span></span>
2. <span data-ttu-id="b9c34-268">Váltás toohello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="b9c34-268">Switch toohello **Settings** tab.</span></span>
3. <span data-ttu-id="b9c34-269">Kattintson a **módosítás** hivatkozásra **HTTP-Proxy** szakasz toolaunch hello **HTTP-Proxy beállítása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b9c34-269">Click **Change** link in **HTTP Proxy** section toolaunch hello **Set HTTP Proxy** dialog.</span></span>  
4. <span data-ttu-id="b9c34-270">Miután rákattintott hello **következő** gomb, megjelenik egy figyelmeztető párbeszédpanel esetében az engedély toosave hello proxybeállítást, és indítsa újra a hello átjáró gazdaszolgáltatás kéri.</span><span class="sxs-lookup"><span data-stu-id="b9c34-270">After you click hello **Next** button, you see a warning dialog asking for your permission toosave hello proxy setting and restart hello Gateway Host Service.</span></span>

<span data-ttu-id="b9c34-271">Megtekintheti és HTTP-proxy frissítse a Configuration Manager eszközzel.</span><span class="sxs-lookup"><span data-stu-id="b9c34-271">You can view and update HTTP proxy by using Configuration Manager tool.</span></span>

![A Konfigurációkezelő használatával állítsa proxy](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> <span data-ttu-id="b9c34-273">Ha korábban beállított proxykiszolgálót NTLM-hitelesítéssel, átjáró gazdaszolgáltatás hello tartományi fiók alatt fut.</span><span class="sxs-lookup"><span data-stu-id="b9c34-273">If you set up a proxy server with NTLM authentication, Gateway Host Service runs under hello domain account.</span></span> <span data-ttu-id="b9c34-274">Hello jelszó később hello tartományi fiók módosításakor megjegyzése tooupdate hello szolgáltatás konfigurációs beállításait, és indítsa újra a ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b9c34-274">If you change hello password for hello domain account later, remember tooupdate configuration settings for hello service and restart it accordingly.</span></span> <span data-ttu-id="b9c34-275">Toothis követelmény miatt javasoljuk, hogy egy dedikált tartományi fiókot tooaccess hello proxykiszolgálóra, amely nem követeli meg tooupdate hello gyakran használ.</span><span class="sxs-lookup"><span data-stu-id="b9c34-275">Due toothis requirement, we suggest you use a dedicated domain account tooaccess hello proxy server that does not require you tooupdate hello password frequently.</span></span>
>
>

### <a name="configure-proxy-server-settings"></a><span data-ttu-id="b9c34-276">Proxykiszolgáló-beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9c34-276">Configure proxy server settings</span></span>
<span data-ttu-id="b9c34-277">Ha **system proxy használata** hello HTTP proxy beállításnál átjáró hello proxybeállítást diahost.exe.config és diawp.exe.config használja.  Ha nincs olyan proxy diahost.exe.config és diawp.exe.config van megadva, az átjáró csatlakozik toocloud szolgáltatás közvetlenül a proxy áthaladás nélkül.</span><span class="sxs-lookup"><span data-stu-id="b9c34-277">If you select **Use system proxy** setting for hello HTTP proxy, gateway uses hello proxy setting in diahost.exe.config and diawp.exe.config.  If no proxy is specified in diahost.exe.config and diawp.exe.config, gateway connects toocloud service directly without going through proxy.</span></span> <span data-ttu-id="b9c34-278">hello alábbi eljárás ismerteti hello diahost.exe.config fájl frissítése.</span><span class="sxs-lookup"><span data-stu-id="b9c34-278">hello following procedure provides instructions for updating hello diahost.exe.config file.</span></span>  

1. <span data-ttu-id="b9c34-279">A Fájlkezelőben hello eredeti fájlt a C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\Shared\diahost.exe.config tooback példányának alkotják.</span><span class="sxs-lookup"><span data-stu-id="b9c34-279">In File Explorer, make a safe copy of C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config tooback up hello original file.</span></span>
2. <span data-ttu-id="b9c34-280">Indítsa el a Notepad.exe rendszergazdaként fut, és nyissa meg a szöveges fájl "C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\Shared\diahost.exe.config. Hello az alapértelmezett címke a System.NET névtérbeli találja, ahogy az a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="b9c34-280">Launch Notepad.exe running as administrator, and open text file “C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. You find hello default tag for system.net as shown in hello following code:</span></span>

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   <span data-ttu-id="b9c34-281">Proxy kiszolgálóadatok amelyet ezután felvehet, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="b9c34-281">You can then add proxy server details as shown in hello following example:</span></span>

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   <span data-ttu-id="b9c34-282">További tulajdonságok megengedettek hello proxy címke toospecify hello szükséges beállítások, például scriptLocation belül.</span><span class="sxs-lookup"><span data-stu-id="b9c34-282">Additional properties are allowed inside hello proxy tag toospecify hello required settings like scriptLocation.</span></span> <span data-ttu-id="b9c34-283">Tekintse meg a túl[proxy (hálózati beállítások) elem](https://msdn.microsoft.com/library/sa91de1e.aspx) a szintaxist.</span><span class="sxs-lookup"><span data-stu-id="b9c34-283">Refer too[proxy Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on syntax.</span></span>

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. <span data-ttu-id="b9c34-284">Hello eredeti helyre mentse a hello konfigurációs fájlt, majd indítsa újra az adatkezelési átjáró Gazdaszolgáltatáshoz szolgáltatást, amely átveszi a hello módosítások hello.</span><span class="sxs-lookup"><span data-stu-id="b9c34-284">Save hello configuration file into hello original location, then restart hello Data Management Gateway Host service, which picks up hello changes.</span></span> <span data-ttu-id="b9c34-285">toorestart hello szolgáltatás: hello vagy hello Vezérlőpult szolgáltatások kisalkalmazását használja **az adatkezelési átjáró konfigurációkezelőjének** > hello kattintson **szolgáltatás leállítása** gombra, majd kattintson az hello **Szolgáltatás elindítása**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-285">toorestart hello service: use services applet from hello control panel, or from hello **Data Management Gateway Configuration Manager** > click hello **Stop Service** button, then click hello **Start Service**.</span></span> <span data-ttu-id="b9c34-286">Hello szolgáltatás nem indul el, akkor valószínű, hogy egy érvénytelen XML-címke szintaxissal szerkesztették hello alkalmazás konfigurációs fájljában hozzáadta-e.</span><span class="sxs-lookup"><span data-stu-id="b9c34-286">If hello service does not start, it is likely that an incorrect XML tag syntax has been added into hello application configuration file that was edited.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b9c34-287">Ne feledje tooupdate **mindkét** diahost.exe.config és diawp.exe.config.</span><span class="sxs-lookup"><span data-stu-id="b9c34-287">Do not forget tooupdate **both** diahost.exe.config and diawp.exe.config.</span></span>  


<span data-ttu-id="b9c34-288">Ezenkívül toothese pontok szükség toomake meg arról, hogy a Microsoft Azure a vállalat engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b9c34-288">In addition toothese points, you also need toomake sure Microsoft Azure is in your company’s whitelist.</span></span> <span data-ttu-id="b9c34-289">érvényes Microsoft Azure IP-címek listája hello tölthető le: hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="b9c34-289">hello list of valid Microsoft Azure IP addresses can be downloaded from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a><span data-ttu-id="b9c34-290">A tűzfal és proxy-kiszolgálóval kapcsolatos problémák lehetséges jelenségek</span><span class="sxs-lookup"><span data-stu-id="b9c34-290">Possible symptoms for firewall and proxy server-related issues</span></span>
<span data-ttu-id="b9c34-291">Hogy a felmerülő hibák hasonló toohello azokat, a következő valószínű hello tűzfalhoz vagy proxyhoz kiszolgáló, amely megakadályozza a kapcsolódó tooData gyári tooauthenticate maga az átjáró tooimproper konfigurációja miatt.</span><span class="sxs-lookup"><span data-stu-id="b9c34-291">If you encounter errors similar toohello following ones, it is likely due tooimproper configuration of hello firewall or proxy server, which blocks gateway from connecting tooData Factory tooauthenticate itself.</span></span> <span data-ttu-id="b9c34-292">Tekintse meg a tooprevious szakasz tooensure a tűzfal és proxy kiszolgáló konfigurációja megfelelő-e.</span><span class="sxs-lookup"><span data-stu-id="b9c34-292">Refer tooprevious section tooensure your firewall and proxy server are properly configured.</span></span>

1. <span data-ttu-id="b9c34-293">Tooregister hello átjáró használatakor kapni hello hiba a következő: "nem sikerült tooregister hello átjáró kulcsa.</span><span class="sxs-lookup"><span data-stu-id="b9c34-293">When you try tooregister hello gateway, you receive hello following error: "Failed tooregister hello gateway key.</span></span> <span data-ttu-id="b9c34-294">Mielőtt újra próbálkozna tooregister hello átjárókulcsot, győződjön meg arról, hogy csatlakoztatott állapotban van az adatkezelési átjáró hello és hello adatkezelési átjáró gazdaszolgáltatás elindult."</span><span class="sxs-lookup"><span data-stu-id="b9c34-294">Before trying tooregister hello gateway key again, confirm that hello data management gateway is in a connected state and hello Data Management Gateway Host Service is Started."</span></span>
2. <span data-ttu-id="b9c34-295">A Configuration Manager megnyitásakor állapota megjelenik "Kapcsolat" vagy "Csatlakozás".</span><span class="sxs-lookup"><span data-stu-id="b9c34-295">When you open Configuration Manager, you see status as “Disconnected” or “Connecting.”</span></span> <span data-ttu-id="b9c34-296">Amikor megtekintik a Windows eseménynaplóiban keresse meg, a "Eseménynapló" > "Alkalmazások és szolgáltatások Logs" > "Az adatkezelési átjáró" hibaüzenetek jelennek meg például a következő hiba hello:`Unable tooconnect toohello remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`</span><span class="sxs-lookup"><span data-stu-id="b9c34-296">When viewing Windows event logs, under “Event Viewer” > “Application and Services Logs” > “Data Management Gateway”, you see error messages such as hello following error: `Unable tooconnect toohello remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`</span></span>

### <a name="open-port-8050-for-credential-encryption"></a><span data-ttu-id="b9c34-297">Nyissa meg a portot 8050 a hitelesítő adatok titkosításához</span><span class="sxs-lookup"><span data-stu-id="b9c34-297">Open port 8050 for credential encryption</span></span>
<span data-ttu-id="b9c34-298">Hello **hitelesítő adatok beállítása a** alkalmazás használja a bejövő portot hello **8050** toorelay hitelesítő adatok toohello átjáró egy helyszíni beállításakor társított szolgáltatásnak az hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b9c34-298">hello **Setting Credentials** application uses hello inbound port **8050** toorelay credentials toohello gateway when you set up an on-premises linked service in hello Azure portal.</span></span> <span data-ttu-id="b9c34-299">Átjáró telepítésekor alapértelmezés szerint hello átjáró telepítése megnyitja hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="b9c34-299">During gateway setup, by default, hello gateway installation opens it on hello gateway machine.</span></span>

<span data-ttu-id="b9c34-300">Ha egy külső tűzfalat használ, manuálisan hello portot 8050 is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="b9c34-300">If you are using a third-party firewall, you can manually open hello port 8050.</span></span> <span data-ttu-id="b9c34-301">Ha futtatja a tűzfallal kapcsolatos probléma átjáró telepítése során, próbálkozhat hello parancs tooinstall hello átjáró következő hello tűzfal konfigurálása nélkül.</span><span class="sxs-lookup"><span data-stu-id="b9c34-301">If you run into firewall issue during gateway setup, you can try using hello following command tooinstall hello gateway without configuring hello firewall.</span></span>

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

<span data-ttu-id="b9c34-302">Ha nem tooopen hello port 8050 hello-átjáró számítógépén, használja a hello használatától eltérő mechanizmusok **hitelesítő adatok beállítása a** tooconfigure alkalmazásadatok tárolja a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b9c34-302">If you choose not tooopen hello port 8050 on hello gateway machine, use mechanisms other than using hello **Setting Credentials** application tooconfigure data store credentials.</span></span> <span data-ttu-id="b9c34-303">Használhat például [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="b9c34-303">For example, you could use [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="b9c34-304">Lásd: [hitelesítő adatok beállítása és biztonsági](#set-credentials-and-securityy) állíthat be szakaszt, hogyan tárolja az adatokat a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b9c34-304">See [Setting Credentials and Security](#set-credentials-and-securityy) section on how data store credentials can be set.</span></span>

## <a name="update"></a><span data-ttu-id="b9c34-305">Frissítés</span><span class="sxs-lookup"><span data-stu-id="b9c34-305">Update</span></span>
<span data-ttu-id="b9c34-306">Alapértelmezés szerint az adatkezelési átjáró automatikusan frissül, amikor hello átjáró egy újabb verziója érhető el.</span><span class="sxs-lookup"><span data-stu-id="b9c34-306">By default, data management gateway is automatically updated when a newer version of hello gateway is available.</span></span> <span data-ttu-id="b9c34-307">hello átjáró frissítése nem történik meg, amíg minden hello ütemezett feladat befejezése.</span><span class="sxs-lookup"><span data-stu-id="b9c34-307">hello gateway is not updated until all hello scheduled tasks are done.</span></span> <span data-ttu-id="b9c34-308">Nincsenek további feladatok dolgozza fel hello átjáró hello frissítési művelet befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="b9c34-308">No further tasks are processed by hello gateway until hello update operation is completed.</span></span> <span data-ttu-id="b9c34-309">Ha hello frissítés sikertelen, átjáró vissza lesz állítva toohello régi verzióját.</span><span class="sxs-lookup"><span data-stu-id="b9c34-309">If hello update fails, gateway is rolled back toohello old version.</span></span>

<span data-ttu-id="b9c34-310">Megjelenik a következő helyek hello hello ütemezett frissítés időpontja:</span><span class="sxs-lookup"><span data-stu-id="b9c34-310">You see hello scheduled update time in hello following places:</span></span>

* <span data-ttu-id="b9c34-311">hello átjáró tulajdonságlapján hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b9c34-311">hello gateway properties page in hello Azure portal.</span></span>
* <span data-ttu-id="b9c34-312">Az adatkezelési átjáró konfigurációkezelőjének hello kezdőlapja</span><span class="sxs-lookup"><span data-stu-id="b9c34-312">Home page of hello Data Management Gateway Configuration Manager</span></span>
* <span data-ttu-id="b9c34-313">Rendszer tálca értesítési üzenetet.</span><span class="sxs-lookup"><span data-stu-id="b9c34-313">System tray notification message.</span></span>

<span data-ttu-id="b9c34-314">hello kezdőlap lapján hello az adatkezelési átjáró konfigurációkezelőjének hello frissítési ütemezés jeleníti meg, és hello utolsó idő hello átjáró volt telepítése/frissítése.</span><span class="sxs-lookup"><span data-stu-id="b9c34-314">hello Home tab of hello Data Management Gateway Configuration Manager displays hello update schedule and hello last time hello gateway was installed/updated.</span></span>

![Frissítések ütemezése](media/data-factory-data-management-gateway/UpdateSection.png)

<span data-ttu-id="b9c34-316">A frissítés hello azonnal, vagy várjon, amíg hello átjáró toobe hello ütemezett időpontban automatikusan frissül.</span><span class="sxs-lookup"><span data-stu-id="b9c34-316">You can install hello update right away or wait for hello gateway toobe automatically updated at hello scheduled time.</span></span> <span data-ttu-id="b9c34-317">Például hello a következő kép azt mutatja be, akkor hello látható hello átjáró konfigurációkezelőjének együtt hello frissítés gombra, hogy tooinstall kattinthat, azonnal értesítési üzenetet.</span><span class="sxs-lookup"><span data-stu-id="b9c34-317">For example, hello following image shows you hello notification message shown in hello Gateway Configuration Manager along with hello Update button that you can click tooinstall it immediately.</span></span>

![Frissítés a DMG Configuration Managerben](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

<span data-ttu-id="b9c34-319">hello értesítési üzenet hello tálcán látható módon hello kép a következő lenne:</span><span class="sxs-lookup"><span data-stu-id="b9c34-319">hello notification message in hello system tray would look as shown in hello following image:</span></span>

![Rendszer tálca üzenet](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

<span data-ttu-id="b9c34-321">Lásd a frissítési művelet (Manuális vagy automatikus) hello tálcán hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="b9c34-321">You see hello status of update operation (manual or automatic) in hello system tray.</span></span> <span data-ttu-id="b9c34-322">Átjáró konfigurációkezelőjének elindításakor legközelebb látja hello értesítési sáv adott hello átjáró üzenet frissítve lett hivatkozással együtt túl[Mi az az új témakör](data-factory-gateway-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="b9c34-322">When you launch Gateway Configuration Manager next time, you see a message on hello notification bar that hello gateway has been updated along with a link too[what's new topic](data-factory-gateway-release-notes.md).</span></span>

### <a name="toodisableenable-auto-update-feature"></a><span data-ttu-id="b9c34-323">toodisable/enable automatikus frissítési szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b9c34-323">toodisable/enable auto-update feature</span></span>
<span data-ttu-id="b9c34-324">Akkor is tiltása/engedélyezése hello automatikus frissítési szolgáltatás hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="b9c34-324">You can disable/enable hello auto-update feature by doing hello following steps:</span></span>

<span data-ttu-id="b9c34-325">[Az átjáró egyetlen csomópont]</span><span class="sxs-lookup"><span data-stu-id="b9c34-325">[For single node gateway]</span></span>
1. <span data-ttu-id="b9c34-326">Indítsa el a Windows PowerShell hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="b9c34-326">Launch Windows PowerShell on hello gateway machine.</span></span>
2. <span data-ttu-id="b9c34-327">Váltás toohello C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\PowerShellScript mappát.</span><span class="sxs-lookup"><span data-stu-id="b9c34-327">Switch toohello C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript folder.</span></span>
3. <span data-ttu-id="b9c34-328">Futtassa a következő parancs tooturn hello az automatikus frissítés hello szolgáltatás kikapcsolása (Letiltás).</span><span class="sxs-lookup"><span data-stu-id="b9c34-328">Run hello following command tooturn hello auto-update feature OFF (disable).</span></span>   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. <span data-ttu-id="b9c34-329">tooturn újra be:</span><span class="sxs-lookup"><span data-stu-id="b9c34-329">tooturn it back on:</span></span>

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
<span data-ttu-id="b9c34-330">[[Többcsomópontos magas rendelkezésre állású és méretezhető átjáró (előzetes verzió)](data-factory-data-management-gateway-high-availability-scalability.md)]</span><span class="sxs-lookup"><span data-stu-id="b9c34-330">[[For multi-node highly available and scalable gateway (preview)](data-factory-data-management-gateway-high-availability-scalability.md)]</span></span>
1. <span data-ttu-id="b9c34-331">Indítsa el a Windows PowerShell hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="b9c34-331">Launch Windows PowerShell on hello gateway machine.</span></span>
2. <span data-ttu-id="b9c34-332">Váltás toohello C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\PowerShellScript mappát.</span><span class="sxs-lookup"><span data-stu-id="b9c34-332">Switch toohello C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript folder.</span></span>
3. <span data-ttu-id="b9c34-333">Futtassa a következő parancs tooturn hello az automatikus frissítés hello szolgáltatás kikapcsolása (Letiltás).</span><span class="sxs-lookup"><span data-stu-id="b9c34-333">Run hello following command tooturn hello auto-update feature OFF (disable).</span></span>   

    <span data-ttu-id="b9c34-334">Átjáró (előzetes verzió) magas rendelkezésre állás szolgáltatással egy extra AuthKey paraméter megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="b9c34-334">For gateway with high availability feature (preview), an extra AuthKey param is required.</span></span>
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. <span data-ttu-id="b9c34-335">tooturn újra be:</span><span class="sxs-lookup"><span data-stu-id="b9c34-335">tooturn it back on:</span></span>

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install hello gateway, you can launch Data Management Gateway Configuration Manager in one of hello following ways:

1. In hello **Search** window, type **Data Management Gateway** tooaccess this utility.
2. Run hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
hello Home page allows you toodo hello following actions:

* View status of hello gateway (connected toohello cloud service etc.).
* **Register** using a key from hello portal.
* **Stop** and start hello **Data Management Gateway Host service** on hello gateway machine.
* **Schedule updates** at a specific time of hello days.
* View hello date when hello gateway was **last updated**.

### Settings page
hello Settings page allows you toodo hello following actions:

* View, change, and export **certificate** used by hello gateway. This certificate is used tooencrypt data source credentials.
* Change **HTTPS port** for hello endpoint. hello gateway opens a port for setting hello data source credentials.
* **Status** of hello endpoint
* View **SSL certificate** is used for SSL communication between portal and hello gateway tooset credentials for data sources.  

### Diagnostics page
hello Diagnostics page allows you toodo hello following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs tooMicrosoft if there was a failure.
* **Test connection** tooa data source.  

### Help page
hello Help page displays hello following information:  

* Brief description of hello gateway
* Version number
* Links tooonline help, privacy statement, and license agreement.  

## Monitor gateway in hello portal
In hello Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate toohello home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select hello **gateway** in hello **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In hello **Gateway** page, you can see hello memory and CPU usage of hello gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** toosee more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

hello following table provides descriptions of columns in hello **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of hello logical gateway and nodes associated with hello gateway. Node is an on-premises Windows machine that has hello gateway installed on it. For information on having more than one node (up toofour nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of hello logical gateway and hello gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows hello version of hello logical gateway and each gateway node. hello version of hello logical gateway is determined based on version of majority of nodes in hello group. If there are nodes with different versions in hello logical gateway setup, only hello nodes with hello same version number as hello logical gateway function properly. Others are in hello limited mode and need toobe manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies hello maximum concurrent jobs for each node. This value is defined based on hello machine size. You can increase hello limit tooscale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when hello scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used tooexecute jobs. There is only one dispatcher node, which is used toopull tasks/jobs from cloud services and dispatch them toodifferent worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in hello gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
hello following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected tooData Factory service.
Offline | Node is offline.
Upgrading | hello node is being auto-updated.
Limited | Due tooConnectivity issue. May be due tooHTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from hello configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect tooother nodes. 


hello following table provides possible statuses of a **logical gateway**. hello gateway status depends on statuses of hello gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered toothis logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due toocredential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure hello number of **concurrent data movement jobs** that can run on a node tooscale up hello capability of moving data between on-premises and cloud data stores. 

When hello available memory and CPU are not utilized well, but hello idle capacity is 0, you should scale up by increasing hello number of concurrent jobs that can run on a node. You may also want tooscale up when activities are timing out because hello gateway is overloaded. In hello advanced settings of a gateway node, you can increase hello maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using hello data management gateway.  

## Move gateway from one machine tooanother
This section provides steps for moving gateway client from one machine tooanother machine.

1. In hello portal, navigate toohello **Data Factory home page**, and click hello **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in hello **DATA GATEWAYS** section of hello **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In hello **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In hello **Configure** page, click **Download and install data gateway**, and follow instructions tooinstall hello data gateway on hello machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep hello **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In hello **Configure** page in hello portal, click **Recreate key** on hello command bar, and click **Yes** for hello warning message. Click **copy button** next tookey text that copies hello key toohello clipboard. hello gateway on hello old machine stops functioning as soon you recreate hello key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste hello **key** into text box in hello **Register Gateway** page of hello **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box toosee hello key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** tooregister hello gateway with hello cloud service.
9. On hello **Settings** tab, click **Change** tooselect hello same certificate that was used with hello old gateway, enter hello **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from hello old gateway by doing hello following steps: launch Data Management Gateway Configuration Manager on hello old machine, switch toohello **Certificate** tab, click **Export** button and follow hello instructions.
10. After successful registration of hello gateway, you should see hello **Registration** set too**Registered** and **Status** set too**Started** on hello Home page of hello Gateway Configuration Manager.

## Encrypting credentials
tooencrypt credentials in hello Data Factory Editor, do hello following steps:

1. Launch web browser on hello **gateway machine**, navigate too[Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in hello **DATA FACTORY** page and then click **Author & Deploy** toolaunch Data Factory Editor.   
2. Click an existing **linked service** in hello tree view toosee its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In hello JSON editor, for hello **gatewayName** property, enter hello name of hello gateway.
4. Enter server name for hello **Data Source** property in hello **connectionString**.
5. Enter database name for hello **Initial Catalog** property in hello **connectionString**.    
6. Click **Encrypt** button on hello command bar that launches hello click-once **Credential Manager** application. You should see hello **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In hello **Setting Credentials** dialog box, do hello following steps:
   1. Select **authentication** that you want hello Data Factory service toouse tooconnect toohello database.
   2. Enter name of hello user who has access toohello database for hello **USERNAME** setting.
   3. Enter password for hello user for hello **PASSWORD** setting.  
   4. Click **OK** tooencrypt credentials and close hello dialog box.
8. You should see a **encryptedCredential** property in hello **connectionString** now.

    ```JSON
    {
        "name": "SqlServerLinkedService",
        "properties": {
            "type": "OnPremisesSqlServer",
            "description": "",
            "typeProperties": {
                "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                "gatewayName": "adftutorialgateway"
            }
        }
    }
    ```
<span data-ttu-id="b9c34-336">Ha olyan számítógépen, amelyen eltér a hello átjárót működtető gépen hello portálon elérhető, meg kell győződnie arról, hogy hello hitelesítőadat-kezelője alkalmazás csatlakozni tud-e toohello átjárót működtető gépen.</span><span class="sxs-lookup"><span data-stu-id="b9c34-336">If you access hello portal from a machine that is different from hello gateway machine, you must make sure that hello Credentials Manager application can connect toohello gateway machine.</span></span> <span data-ttu-id="b9c34-337">Ha hello alkalmazások nem tudják elérni a hello átjárót működtető gépen, akkor nem teszi lehetővé tooset hitelesítő adatok hello adatforrás és tootest kapcsolat toohello adatforrás.</span><span class="sxs-lookup"><span data-stu-id="b9c34-337">If hello application cannot reach hello gateway machine, it does not allow you tooset credentials for hello data source and tootest connection toohello data source.</span></span>  

<span data-ttu-id="b9c34-338">Hello használata esetén **hitelesítő adatok beállítása a** alkalmazás, hello portal titkosítja hello hitelesítő adatok megadott hello hello tanúsítvány **tanúsítvány** hello lapján **átjáró A Configuration Manager** hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="b9c34-338">When you use hello **Setting Credentials** application, hello portal encrypts hello credentials with hello certificate specified in hello **Certificate** tab of hello **Gateway Configuration Manager** on hello gateway machine.</span></span>

<span data-ttu-id="b9c34-339">Ha hello hitelesítő adatok titkosítására egy API-alapú módszert keres, használhatja a hello [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell parancsmag tooencrypt hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b9c34-339">If you are looking for an API-based approach for encrypting hello credentials, you can use hello [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet tooencrypt credentials.</span></span> <span data-ttu-id="b9c34-340">hello parancsmagot, hogy az átjáró konfigurált toouse tooencrypt hello hitelesítő adatokat az hello tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="b9c34-340">hello cmdlet uses hello certificate that gateway is configured toouse tooencrypt hello credentials.</span></span> <span data-ttu-id="b9c34-341">Akkor adja hozzá a titkosított hitelesítő toohello **EncryptedCredential** hello eleme **connectionString** a hello JSON.</span><span class="sxs-lookup"><span data-stu-id="b9c34-341">You add encrypted credentials toohello **EncryptedCredential** element of hello **connectionString** in hello JSON.</span></span> <span data-ttu-id="b9c34-342">Hello JSON használata hello [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) parancsmag vagy a Data Factory Editor hello.</span><span class="sxs-lookup"><span data-stu-id="b9c34-342">You use hello JSON with hello [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) cmdlet or in hello Data Factory Editor.</span></span>

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

<span data-ttu-id="b9c34-343">Nincs egy további módszer használatával a Data Factory Editor hitelesítő adatok beállítására.</span><span class="sxs-lookup"><span data-stu-id="b9c34-343">There is one more approach for setting credentials using Data Factory Editor.</span></span> <span data-ttu-id="b9c34-344">Ha létrehoz egy csatolt SQL Server szolgáltatás használatával hello szerkesztőt, és adja meg a hitelesítő adatokat egyszerű szöveges, hello azonosító adatok titkosítása hello Data Factory szolgáltatásnak birtokló tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="b9c34-344">If you create a SQL Server linked service by using hello editor and you enter credentials in plain text, hello credentials are encrypted using a certificate that hello Data Factory service owns.</span></span> <span data-ttu-id="b9c34-345">Nem használ, hogy az átjáró az beállított toouse hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="b9c34-345">It does NOT use hello certificate that gateway is configured toouse.</span></span> <span data-ttu-id="b9c34-346">Lehet, hogy bizonyos esetekben kissé gyorsabb ezt a módszert használja, de napjainkban kevésbé biztonságos.</span><span class="sxs-lookup"><span data-stu-id="b9c34-346">While this approach might be a little faster in some cases, it is less secure.</span></span> <span data-ttu-id="b9c34-347">Ezért azt javasoljuk, hogy hajtsa végre ezt a módszert csak fejlesztési/tesztelési célokra.</span><span class="sxs-lookup"><span data-stu-id="b9c34-347">Therefore, we recommend that you follow this approach only for development/testing purposes.</span></span>

## <a name="powershell-cmdlets"></a><span data-ttu-id="b9c34-348">PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="b9c34-348">PowerShell cmdlets</span></span>
<span data-ttu-id="b9c34-349">Ez a szakasz ismerteti, hogyan toocreate és regisztrál egy átjárót, Azure PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="b9c34-349">This section describes how toocreate and register a gateway using Azure PowerShell cmdlets.</span></span>

1. <span data-ttu-id="b9c34-350">Indítsa el **Azure PowerShell** rendszergazdai módban.</span><span class="sxs-lookup"><span data-stu-id="b9c34-350">Launch **Azure PowerShell** in administrator mode.</span></span>
2. <span data-ttu-id="b9c34-351">Jelentkezzen be Azure-fiók tooyour futtatásával hello a következő parancsot, majd gépelje be a Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b9c34-351">Log in tooyour Azure account by running hello following command and entering your Azure credentials.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="b9c34-352">Használjon hello **New-AzureRmDataFactoryGateway** parancsmag toocreate logikai átjáró az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b9c34-352">Use hello **New-AzureRmDataFactoryGateway** cmdlet toocreate a logical gateway as follows:</span></span>

    ```PowerShell
    $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    <span data-ttu-id="b9c34-353">**Példa parancs és a kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="b9c34-353">**Example command and output**:</span></span>

    ```
    PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

    Name              : MyGateway
    Description       : gateway for walkthrough
    Version           :
    Status            : NeedRegistration
    VersionStatus     : None
    CreateTime        : 9/28/2014 10:58:22
    RegisterTime      :
    LastConnectTime   :
    ExpiryTime        :
    ProvisioningState : Succeeded
    Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI
    ```

1. <span data-ttu-id="b9c34-354">Azure PowerShell, a kapcsoló toohello mappa: **C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\PowerShellScript\**.</span><span class="sxs-lookup"><span data-stu-id="b9c34-354">In Azure PowerShell, switch toohello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**.</span></span> <span data-ttu-id="b9c34-355">Futtatás **RegisterGateway.ps1** hello lokális változó társított **$Key** hello parancs a következő ábrán.</span><span class="sxs-lookup"><span data-stu-id="b9c34-355">Run **RegisterGateway.ps1** associated with hello local variable **$Key** as shown in hello following command.</span></span> <span data-ttu-id="b9c34-356">Ez a parancsfájl regisztrálja hello ügyfélügynök hello logikai átjáróval korábban létrehozta a gépen telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b9c34-356">This script registers hello client agent installed on your machine with hello logical gateway you create earlier.</span></span>

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    <span data-ttu-id="b9c34-357">Hello IsRegisterOnRemoteMachine paraméter használatával regisztrálhatja hello átjáró egy távoli számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b9c34-357">You can register hello gateway on a remote machine by using hello IsRegisterOnRemoteMachine parameter.</span></span> <span data-ttu-id="b9c34-358">Példa:</span><span class="sxs-lookup"><span data-stu-id="b9c34-358">Example:</span></span>

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. <span data-ttu-id="b9c34-359">Használhatja a hello **Get-AzureRmDataFactoryGateway** parancsmag tooget hello az adat-előállítóban átjárók listája.</span><span class="sxs-lookup"><span data-stu-id="b9c34-359">You can use hello **Get-AzureRmDataFactoryGateway** cmdlet tooget hello list of Gateways in your data factory.</span></span> <span data-ttu-id="b9c34-360">Ha hello **állapot** látható **online**, az azt jelenti, hogy az átjáró készen áll a toouse.</span><span class="sxs-lookup"><span data-stu-id="b9c34-360">When hello **Status** shows **online**, it means your gateway is ready toouse.</span></span>

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
<span data-ttu-id="b9c34-361">Egy átjárónak hello segítségével eltávolíthatja **Remove-AzureRmDataFactoryGateway** parancsmag és a frissítés leírása hello segítségével átjáró **Set-AzureRmDataFactoryGateway** parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="b9c34-361">You can remove a gateway using hello **Remove-AzureRmDataFactoryGateway** cmdlet and update description for a gateway using hello **Set-AzureRmDataFactoryGateway** cmdlets.</span></span> <span data-ttu-id="b9c34-362">A szintaxis és egyéb részletek ezekről a parancsmagokról tekintse meg a Data Factory parancsmag-referencia.</span><span class="sxs-lookup"><span data-stu-id="b9c34-362">For syntax and other details about these cmdlets, see Data Factory Cmdlet Reference.</span></span>  

### <a name="list-gateways-using-powershell"></a><span data-ttu-id="b9c34-363">PowerShell-lel átjáróit</span><span class="sxs-lookup"><span data-stu-id="b9c34-363">List gateways using PowerShell</span></span>

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a><span data-ttu-id="b9c34-364">Távolítsa el a PowerShell használatával átjáró</span><span class="sxs-lookup"><span data-stu-id="b9c34-364">Remove gateway using PowerShell</span></span>

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a><span data-ttu-id="b9c34-365">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9c34-365">Next steps</span></span>
* <span data-ttu-id="b9c34-366">Lásd: [adatok áthelyezése között a helyszíni és felhőalapú adattároló](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b9c34-366">See [Move data between on-premises and cloud data stores](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="b9c34-367">Hello forgatókönyv hozzon létre egy folyamatot, amely egy helyi SQL Server adatbázis tooan Azure blob hello átjáró toomove adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="b9c34-367">In hello walkthrough, you create a pipeline that uses hello gateway toomove data from an on-premises SQL Server database tooan Azure blob.</span></span>  
