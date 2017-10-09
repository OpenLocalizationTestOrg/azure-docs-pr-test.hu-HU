---
title: "Azure Data Factory használatával aaaMove DB2 adatokat |} Microsoft Docs"
description: "Ismerje meg, hogyan toomove adatait egy helyszíni DB2-adatbázis használati ideje Azure Data Factory másolási tevékenység használatával"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: 696ac059be644cb3901c37d2fc746e0682c65a1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a><span data-ttu-id="deb05-103">Azure Data Factory másolási tevékenység segítségével DB2 tárolt adatok mozgatása</span><span class="sxs-lookup"><span data-stu-id="deb05-103">Move data from DB2 by using Azure Data Factory Copy Activity</span></span>
<span data-ttu-id="deb05-104">Ez a cikk ismerteti, hogyan használhatja ki egy a helyszíni DB2 adatbázis tooa adatok Azure Data Factory toocopy adatok másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="deb05-104">This article describes how you can use Copy Activity in Azure Data Factory toocopy data from an on-premises DB2 database tooa data store.</span></span> <span data-ttu-id="deb05-105">Másolhatja tooany adattároló, amely egy támogatott fogadó a hello blokkolandóként [adat-előállító adatok mozgása tevékenységek](data-factory-data-movement-activities.md#supported-data-stores-and-formats) cikk.</span><span class="sxs-lookup"><span data-stu-id="deb05-105">You can copy data tooany store that is listed as a supported sink in hello [Data Factory data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> <span data-ttu-id="deb05-106">Ez a témakör hello adat-előállító cikket, amely áttekintést nyújt az adatátvitelt jelölik a másolási tevékenység segítségével, és felsorolja a támogatott hello adatokat tároló kombinációk épül.</span><span class="sxs-lookup"><span data-stu-id="deb05-106">This topic builds on hello Data Factory article, which presents an overview of data movement by using Copy Activity and lists hello supported data store combinations.</span></span> 

<span data-ttu-id="deb05-107">Adat-előállító jelenleg csak áthelyezése adatait egy DB2-adatbázishoz tooa [támogatott fogadó adattár](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="deb05-107">Data Factory currently supports only moving data from a DB2 database tooa [supported sink data store](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="deb05-108">Adatok áthelyezése más adatokat tárolja tooa DB2-adatbázishoz nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="deb05-108">Moving data from other data stores tooa DB2 database is not supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="deb05-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="deb05-109">Prerequisites</span></span>
<span data-ttu-id="deb05-110">Adat-előállító támogatja a helyszíni DB2-adatbázishoz kapcsolódó tooan hello [az adatkezelési átjáró](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="deb05-110">Data Factory supports connecting tooan on-premises DB2 database by using hello [data management gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="deb05-111">Részletes útmutatás tooset hello átjáró adatok feldolgozási toomove az adatok című hello [tárolt adatok mozgatása a helyszíni toocloud](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="deb05-111">For step-by-step instructions tooset up hello gateway data pipeline toomove your data, see hello [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="deb05-112">Átjáróra szükség, akkor is, ha hello DB2 üzemelteti Azure IaaS virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="deb05-112">A gateway is required even if hello DB2 is hosted on Azure IaaS VM.</span></span> <span data-ttu-id="deb05-113">Hello átjáró telepíthető hello ugyanabból az infrastruktúra-szolgáltatási virtuális hello adatok tárolóként.</span><span class="sxs-lookup"><span data-stu-id="deb05-113">You can install hello gateway on hello same IaaS VM as hello data store.</span></span> <span data-ttu-id="deb05-114">Ha hello-átjáró képes kapcsolódni toohello adatbázis, hello átjárót telepítheti egy másik virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="deb05-114">If hello gateway can connect toohello database, you can install hello gateway on a different VM.</span></span>

<span data-ttu-id="deb05-115">hello az adatkezelési átjáró egy beépített DB2-illesztőprogram biztosít, így nem toomanually telepíteni kell egy illesztőprogram toocopy adatok DB2.</span><span class="sxs-lookup"><span data-stu-id="deb05-115">hello data management gateway provides a built-in DB2 driver, so you don't need toomanually install a driver toocopy data from DB2.</span></span>

> [!NOTE]
> <span data-ttu-id="deb05-116">Kapcsolat és az átjáró problémák hibaelhárításával kapcsolatos tippek, lásd: hello [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) cikk.</span><span class="sxs-lookup"><span data-stu-id="deb05-116">For tips on troubleshooting connection and gateway issues, see hello [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) article.</span></span>


## <a name="supported-versions"></a><span data-ttu-id="deb05-117">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="deb05-117">Supported versions</span></span>
<span data-ttu-id="deb05-118">hello Data Factory DB2-összekötő a következő IBM DB2-platformokat, 9, 10-es és 11 elosztott relációs adatbázis architektúra (DRDA) SQL hozzáférés-kezelési verzióival verziók hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="deb05-118">hello Data Factory DB2 connector supports hello following IBM DB2 platforms and versions with Distributed Relational Database Architecture (DRDA) SQL Access Manager versions 9, 10, and 11:</span></span>

* <span data-ttu-id="deb05-119">IBM DB2 z/os 11.1 verziója</span><span class="sxs-lookup"><span data-stu-id="deb05-119">IBM DB2 for z/OS version 11.1</span></span>
* <span data-ttu-id="deb05-120">IBM DB2 z/OS 10.1 verziója</span><span class="sxs-lookup"><span data-stu-id="deb05-120">IBM DB2 for z/OS version 10.1</span></span>
* <span data-ttu-id="deb05-121">IBM DB2-i (AS400) verziójának 7.2</span><span class="sxs-lookup"><span data-stu-id="deb05-121">IBM DB2 for i (AS400) version 7.2</span></span>
* <span data-ttu-id="deb05-122">IBM DB2-i (AS400) 7.1-es verziójához</span><span class="sxs-lookup"><span data-stu-id="deb05-122">IBM DB2 for i (AS400) version 7.1</span></span>
* <span data-ttu-id="deb05-123">IBM DB2 Linux, UNIX és a Windows (LUW) 11-es verzió</span><span class="sxs-lookup"><span data-stu-id="deb05-123">IBM DB2 for Linux, UNIX, and Windows (LUW) version 11</span></span>
* <span data-ttu-id="deb05-124">IBM DB2 a LUW 10.5 verziója</span><span class="sxs-lookup"><span data-stu-id="deb05-124">IBM DB2 for LUW version 10.5</span></span>
* <span data-ttu-id="deb05-125">IBM DB2 a LUW 10.1 verziója</span><span class="sxs-lookup"><span data-stu-id="deb05-125">IBM DB2 for LUW version 10.1</span></span>

> [!TIP]
> <span data-ttu-id="deb05-126">Ha hello hibaüzenet jelenhet meg: "hello csomag megfelelő tooan SQL utasítás végrehajtási kérelem nem található.</span><span class="sxs-lookup"><span data-stu-id="deb05-126">If you receive hello error message "hello package corresponding tooan SQL statement execution request was not found.</span></span> <span data-ttu-id="deb05-127">SQLSTATE 51002 SQLCODE =-805, = "hello ennek az oka, a szükséges csomag nem jön létre a normál felhasználói hello hello az operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="deb05-127">SQLSTATE=51002 SQLCODE=-805," hello reason is a necessary package is not created for hello normal user on hello OS.</span></span> <span data-ttu-id="deb05-128">tooresolve probléma, kövesse ezeket az utasításokat a DB2-kiszolgáló típusának:</span><span class="sxs-lookup"><span data-stu-id="deb05-128">tooresolve this issue, follow these instructions for your DB2 server type:</span></span>
> - <span data-ttu-id="deb05-129">A DB2 i (AS400): a másolási tevékenység futtatása előtt hello hello normál felhasználói gyűjtemény létrehozása kiemelt felhasználó segítségével.</span><span class="sxs-lookup"><span data-stu-id="deb05-129">DB2 for i (AS400): Let a power user create hello collection for hello normal user before running Copy Activity.</span></span> <span data-ttu-id="deb05-130">toocreate hello gyűjtemény, hello parancsot használja:`create collection <username>`</span><span class="sxs-lookup"><span data-stu-id="deb05-130">toocreate hello collection, use hello command: `create collection <username>`</span></span>
> - <span data-ttu-id="deb05-131">A z/OS- vagy LUW DB2: használja a magas jogosultságú fiók – egy kiemelt felhasználói vagy a felügyeleti csomag hitelesítésszolgáltatók és a kötési, BINDADD, ENGEDÉLYEKET EXECUTE tooPUBLIC – egyszer toorun hello másolása.</span><span class="sxs-lookup"><span data-stu-id="deb05-131">DB2 for z/OS or LUW: Use a high privilege account--a power user or admin that has package authorities and BIND, BINDADD, GRANT EXECUTE tooPUBLIC permissions--toorun hello copy once.</span></span> <span data-ttu-id="deb05-132">szükséges csomag hello hello másolása során automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="deb05-132">hello necessary package is automatically created during hello copy.</span></span> <span data-ttu-id="deb05-133">Ezt követően hátsó toohello normál felhasználói válthat a későbbi másolási kísérletekhez.</span><span class="sxs-lookup"><span data-stu-id="deb05-133">Afterward, you can switch back toohello normal user for your subsequent copy runs.</span></span>

## <a name="getting-started"></a><span data-ttu-id="deb05-134">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="deb05-134">Getting started</span></span>
<span data-ttu-id="deb05-135">A másolási tevékenység toomove származó adatokkal egy helyszíni DB2 adattároló adatcsatorna különböző eszközöket és API-k használatával hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="deb05-135">You can create a pipeline with a copy activity toomove data from an on-premises DB2 data store by using different tools and APIs:</span></span> 

- <span data-ttu-id="deb05-136">hello legegyszerűbb módja toocreate adatcsatorna toouse hello Azure Data Factory másolása varázsló.</span><span class="sxs-lookup"><span data-stu-id="deb05-136">hello easiest way toocreate a pipeline is toouse hello Azure Data Factory Copy Wizard.</span></span> <span data-ttu-id="deb05-137">Egy folyamat létrehozásával hello másolása varázsló használatával gyorsan útmutatást lásd: hello [oktatóanyag: hozzon létre egy folyamatot hello másolása varázsló használatával](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="deb05-137">For a quick walkthrough on creating a pipeline by using hello Copy Wizard, see hello [Tutorial: Create a pipeline by using hello Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span> 
- <span data-ttu-id="deb05-138">Is használhatja eszközök toocreate egy folyamatot, beleértve a hello Azure-portálon, a Visual Studio, Azure PowerShell, az Azure Resource Manager sablon, hello .NET API és hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="deb05-138">You can also use tools toocreate a pipeline, including hello Azure portal, Visual Studio, Azure PowerShell, an Azure Resource Manager template, hello .NET API, and hello REST API.</span></span> <span data-ttu-id="deb05-139">Részletes útmutatás toocreate a másolási tevékenység során a folyamat, lásd: hello [másolási tevékenység az oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="deb05-139">For step-by-step instructions toocreate a pipeline with a copy activity, see hello [Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

<span data-ttu-id="deb05-140">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="deb05-140">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="deb05-141">Társított szolgáltatások toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="deb05-141">Create linked services toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="deb05-142">Létrehozni adatkészletek toorepresent bemeneti és kimeneti adatai hello másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="deb05-142">Create datasets toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="deb05-143">Hozzon létre egy folyamatot, amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="deb05-143">Create a pipeline with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="deb05-144">Hello másolása varázsló JSON-definíciók hello Data Factory kapcsolt szolgáltatások esetén használatakor adatkészleteket, és a folyamat entitások automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="deb05-144">When you use hello Copy Wizard, JSON definitions for hello Data Factory linked services, datasets, and pipeline entities are automatically created for you.</span></span> <span data-ttu-id="deb05-145">(Kivéve a hello .NET API-t) eszközök vagy API-k használata esetén definiálni hello adat-előállító entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="deb05-145">When you use tools or APIs (except hello .NET API), you define hello Data Factory entities by using hello JSON format.</span></span> <span data-ttu-id="deb05-146">Hello [JSON-példa: adatok másolása az DB2 tooAzure Blob-tároló](#json-example-copy-data-from-db2-to-azure-blob) hello JSON definícióit hello adat-előállító entitások, amelyek egy helyszíni DB2 adattároló használt toocopy adatait mutatja.</span><span class="sxs-lookup"><span data-stu-id="deb05-146">hello [JSON example: Copy data from DB2 tooAzure Blob storage](#json-example-copy-data-from-db2-to-azure-blob) shows hello JSON definitions for hello Data Factory entities that are used toocopy data from an on-premises DB2 data store.</span></span>

<span data-ttu-id="deb05-147">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine hello adat-előállító entitások, amelyek adott tooa DB2-adattár hello részleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="deb05-147">hello following sections provide details about hello JSON properties that are used toodefine hello Data Factory entities that are specific tooa DB2 data store.</span></span>

## <a name="db2-linked-service-properties"></a><span data-ttu-id="deb05-148">Kapcsolódó DB2 szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="deb05-148">DB2 linked service properties</span></span>
<span data-ttu-id="deb05-149">a következő táblázat hello hello JSON tulajdonságokhoz adott tooa DB2 kapcsolódó szolgáltatás sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="deb05-149">hello following table lists hello JSON properties that are specific tooa DB2 linked service.</span></span>

| <span data-ttu-id="deb05-150">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="deb05-150">Property</span></span> | <span data-ttu-id="deb05-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="deb05-151">Description</span></span> | <span data-ttu-id="deb05-152">Szükséges</span><span class="sxs-lookup"><span data-stu-id="deb05-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="deb05-153">**típusa**</span><span class="sxs-lookup"><span data-stu-id="deb05-153">**type**</span></span> |<span data-ttu-id="deb05-154">Ez a tulajdonság túl be kell állítani**OnPremisesDB2**.</span><span class="sxs-lookup"><span data-stu-id="deb05-154">This property must be set too**OnPremisesDB2**.</span></span> |<span data-ttu-id="deb05-155">Igen</span><span class="sxs-lookup"><span data-stu-id="deb05-155">Yes</span></span> |
| <span data-ttu-id="deb05-156">**kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="deb05-156">**server**</span></span> |<span data-ttu-id="deb05-157">hello hello DB2-kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="deb05-157">hello name of hello DB2 server.</span></span> |<span data-ttu-id="deb05-158">Igen</span><span class="sxs-lookup"><span data-stu-id="deb05-158">Yes</span></span> |
| <span data-ttu-id="deb05-159">**adatbázis**</span><span class="sxs-lookup"><span data-stu-id="deb05-159">**database**</span></span> |<span data-ttu-id="deb05-160">hello hello DB2-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="deb05-160">hello name of hello DB2 database.</span></span> |<span data-ttu-id="deb05-161">Igen</span><span class="sxs-lookup"><span data-stu-id="deb05-161">Yes</span></span> |
| <span data-ttu-id="deb05-162">**séma**</span><span class="sxs-lookup"><span data-stu-id="deb05-162">**schema**</span></span> |<span data-ttu-id="deb05-163">hello neve hello séma hello DB2-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="deb05-163">hello name of hello schema in hello DB2 database.</span></span> <span data-ttu-id="deb05-164">Ez a tulajdonság a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="deb05-164">This property is case-sensitive.</span></span> |<span data-ttu-id="deb05-165">Nem</span><span class="sxs-lookup"><span data-stu-id="deb05-165">No</span></span> |
| <span data-ttu-id="deb05-166">**authenticationType**</span><span class="sxs-lookup"><span data-stu-id="deb05-166">**authenticationType**</span></span> |<span data-ttu-id="deb05-167">hello használt tooconnect toohello DB2 adatbázis hitelesítés típusát.</span><span class="sxs-lookup"><span data-stu-id="deb05-167">hello type of authentication that is used tooconnect toohello DB2 database.</span></span> <span data-ttu-id="deb05-168">hello lehetséges értékek a következők: névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="deb05-168">hello possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="deb05-169">Igen</span><span class="sxs-lookup"><span data-stu-id="deb05-169">Yes</span></span> |
| <span data-ttu-id="deb05-170">**felhasználónév**</span><span class="sxs-lookup"><span data-stu-id="deb05-170">**username**</span></span> |<span data-ttu-id="deb05-171">hello felhasználói fiók egyszerű vagy Windows-hitelesítés használata esetén hello nevét.</span><span class="sxs-lookup"><span data-stu-id="deb05-171">hello name for hello user account if you use Basic or Windows authentication.</span></span> |<span data-ttu-id="deb05-172">Nem</span><span class="sxs-lookup"><span data-stu-id="deb05-172">No</span></span> |
| <span data-ttu-id="deb05-173">**jelszó**</span><span class="sxs-lookup"><span data-stu-id="deb05-173">**password**</span></span> |<span data-ttu-id="deb05-174">hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="deb05-174">hello password for hello user account.</span></span> |<span data-ttu-id="deb05-175">Nem</span><span class="sxs-lookup"><span data-stu-id="deb05-175">No</span></span> |
| <span data-ttu-id="deb05-176">**gatewayName**</span><span class="sxs-lookup"><span data-stu-id="deb05-176">**gatewayName**</span></span> |<span data-ttu-id="deb05-177">hello nevét, amely a Data Factory szolgáltatásnak hello hello átjáró használjon tooconnect toohello helyszíni DB2-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="deb05-177">hello name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises DB2 database.</span></span> |<span data-ttu-id="deb05-178">Igen</span><span class="sxs-lookup"><span data-stu-id="deb05-178">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="deb05-179">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="deb05-179">Dataset properties</span></span>
<span data-ttu-id="deb05-180">Hello, illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="deb05-180">For a list of hello sections and properties that are available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="deb05-181">Szakasz, például a **struktúra**, **rendelkezésre állási**, és hello **házirend** adatkészlet JSON hasonlóak az összes adatkészlet esetében (Azure SQL, Azure Blob storage Azure Table tárolási, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="deb05-181">Sections, such as **structure**, **availability**, and hello **policy** for a dataset JSON, are similar for all dataset types (Azure SQL, Azure Blob storage, Azure Table storage, and so on).</span></span>

<span data-ttu-id="deb05-182">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="deb05-182">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="deb05-183">Hello **typeProperties** szakasz egy adatkészlet típusú **RelationalTable**, amely hello DB2 adatkészletet tartalmaz, akkor a következő tulajdonság hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="deb05-183">hello **typeProperties** section for a dataset of type **RelationalTable**, which includes hello DB2 dataset, has hello following property:</span></span>

| <span data-ttu-id="deb05-184">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="deb05-184">Property</span></span> | <span data-ttu-id="deb05-185">Leírás</span><span class="sxs-lookup"><span data-stu-id="deb05-185">Description</span></span> | <span data-ttu-id="deb05-186">Szükséges</span><span class="sxs-lookup"><span data-stu-id="deb05-186">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="deb05-187">**Táblanév**</span><span class="sxs-lookup"><span data-stu-id="deb05-187">**tableName**</span></span> |<span data-ttu-id="deb05-188">hello tábla társított szolgáltatás hello hello DB2-adatbázispéldány neve hello hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="deb05-188">hello name of hello table in hello DB2 database instance that hello linked service refers to.</span></span> <span data-ttu-id="deb05-189">Ez a tulajdonság a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="deb05-189">This property is case-sensitive.</span></span> |<span data-ttu-id="deb05-190">Nem (ha hello **lekérdezés** tulajdonság típusa másolási tevékenység **RelationalSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="deb05-190">No (if hello **query** property of a copy activity of type **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="deb05-191">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="deb05-191">Copy Activity properties</span></span>
<span data-ttu-id="deb05-192">Hello, illetve a másolási tevékenység meghatározásához rendelkezésre álló tulajdonságok listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="deb05-192">For a list of hello sections and properties that are available for defining copy activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="deb05-193">Másolja a tevékenység tulajdonságai, például a **neve**, **leírása**, **bemenetek** tábla, **kimenete** tábla, és **házirend**, tevékenységi érhetők el.</span><span class="sxs-lookup"><span data-stu-id="deb05-193">Copy Activity properties, such as **name**, **description**, **inputs** table, **outputs** table, and **policy**, are available for all types of activities.</span></span> <span data-ttu-id="deb05-194">hello elérhető tulajdonságok hello **typeProperties** szakasz tevékenységek minden típusának hello tevékenységét.</span><span class="sxs-lookup"><span data-stu-id="deb05-194">hello properties that are available in hello **typeProperties** section of hello activity for each activity type.</span></span> <span data-ttu-id="deb05-195">A másolási tevékenység során hello tulajdonságok hello típusú adatforrások és mosdók függenek.</span><span class="sxs-lookup"><span data-stu-id="deb05-195">For Copy Activity, hello properties vary depending on hello types of data sources and sinks.</span></span>

<span data-ttu-id="deb05-196">Másolási tevékenységhez, ha hello adatforrás típusú **RelationalSource** (amely tartalmazza a DB2 rendszerhez), a következő tulajdonságok hello érhetők el hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="deb05-196">For Copy Activity, when hello source is of type **RelationalSource** (which includes DB2), hello following properties are available in hello **typeProperties** section:</span></span>

| <span data-ttu-id="deb05-197">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="deb05-197">Property</span></span> | <span data-ttu-id="deb05-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="deb05-198">Description</span></span> | <span data-ttu-id="deb05-199">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="deb05-199">Allowed values</span></span> | <span data-ttu-id="deb05-200">Szükséges</span><span class="sxs-lookup"><span data-stu-id="deb05-200">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="deb05-201">**lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="deb05-201">**query**</span></span> |<span data-ttu-id="deb05-202">Hello egyéni lekérdezés tooread hello adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="deb05-202">Use hello custom query tooread hello data.</span></span> |<span data-ttu-id="deb05-203">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="deb05-203">SQL query string.</span></span> <span data-ttu-id="deb05-204">Például:`"query": "select * from "MySchema"."MyTable""`</span><span class="sxs-lookup"><span data-stu-id="deb05-204">For example: `"query": "select * from "MySchema"."MyTable""`</span></span> |<span data-ttu-id="deb05-205">Nem (ha hello **tableName** a DataSet adatkészlet tulajdonság meg van adva)</span><span class="sxs-lookup"><span data-stu-id="deb05-205">No (if hello **tableName** property of a dataset is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="deb05-206">Séma-és tábla-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="deb05-206">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="deb05-207">Hello lekérdezés utasításban, tegye a tulajdonságnevek "" (idézőjelek).</span><span class="sxs-lookup"><span data-stu-id="deb05-207">In hello query statement, enclose property names by using "" (double quotes).</span></span> <span data-ttu-id="deb05-208">Példa:</span><span class="sxs-lookup"><span data-stu-id="deb05-208">For example:</span></span>
>
> ```sql
> "query": "select * from "DB2ADMIN"."Customers""
> ```

## <a name="json-example-copy-data-from-db2-tooazure-blob-storage"></a><span data-ttu-id="deb05-209">JSON-példa: adatok másolása az DB2 tooAzure Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="deb05-209">JSON example: Copy data from DB2 tooAzure Blob storage</span></span>
<span data-ttu-id="deb05-210">Ebben a példában a minta JSON definícióit tartalmazza használható toocreate adatcsatorna hello segítségével [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="deb05-210">This example provides sample JSON definitions that you can use toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="deb05-211">hello példa bemutatja, hogyan adatbázis toocopy adatait egy DB2-tooBlob tároló.</span><span class="sxs-lookup"><span data-stu-id="deb05-211">hello example shows you how toocopy data from a DB2 database tooBlob storage.</span></span> <span data-ttu-id="deb05-212">Azonban adatokat másolhat túl[bármely támogatott adatok tárolásához a fogadó típusa](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Azure Data Factory másolási tevékenység használatával.</span><span class="sxs-lookup"><span data-stu-id="deb05-212">However, data can be copied too[any supported data store sink type](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Azure Data Factory Copy Activity.</span></span>

<span data-ttu-id="deb05-213">hello minta a következő adat-előállító entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="deb05-213">hello sample has hello following Data Factory entities:</span></span>

- <span data-ttu-id="deb05-214">Egy DB2 társított szolgáltatás típusa [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="deb05-214">A DB2 linked service of type [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)</span></span>
- <span data-ttu-id="deb05-215">Egy Azure Blob storage társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="deb05-215">An Azure Blob storage linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
- <span data-ttu-id="deb05-216">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="deb05-216">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)</span></span>
- <span data-ttu-id="deb05-217">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="deb05-217">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
- <span data-ttu-id="deb05-218">A [csővezeték](data-factory-create-pipelines.md) hello használó másolása tevékenységgel [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="deb05-218">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses hello [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) properties</span></span>

<span data-ttu-id="deb05-219">hello minta másol adatokat egy DB2-adatbázishoz tooan Azure blob egy lekérdezés eredményt óránként.</span><span class="sxs-lookup"><span data-stu-id="deb05-219">hello sample copies data from a query result in a DB2 database tooan Azure blob hourly.</span></span> <span data-ttu-id="deb05-220">hello entitás definíciók hello szakaszok hello mintában használt hello JSON tulajdonságokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="deb05-220">hello JSON properties that are used in hello sample are described in hello sections that follow hello entity definitions.</span></span>

<span data-ttu-id="deb05-221">Első lépésként telepítse, és konfigurálja a data gateway.</span><span class="sxs-lookup"><span data-stu-id="deb05-221">As a first step, install and configure a data gateway.</span></span> <span data-ttu-id="deb05-222">Útmutatás a hello el [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="deb05-222">Instructions are in hello [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="deb05-223">**DB2 társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="deb05-223">**DB2 linked service**</span></span>

```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="deb05-224">**Az Azure Blob storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="deb05-224">**Azure Blob storage linked service**</span></span>

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

<span data-ttu-id="deb05-225">**DB2 bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="deb05-225">**DB2 input dataset**</span></span>

<span data-ttu-id="deb05-226">hello példa feltételezi, hogy létrehozott egy táblát a "MyTable", oszlop, "időbélyeg" hello idő adatsorozat adatok feliratú nevű DB2.</span><span class="sxs-lookup"><span data-stu-id="deb05-226">hello sample assumes that you have created a table in DB2 named "MyTable" that has a column labeled "timestamp" for hello time series data.</span></span>

<span data-ttu-id="deb05-227">Hello **külső** tulajdonsága túl "true."</span><span class="sxs-lookup"><span data-stu-id="deb05-227">hello **external** property is set too"true."</span></span> <span data-ttu-id="deb05-228">Ez a beállítás arról értesíti hello Data Factory szolgáltatásnak, hogy ez az adatkészlet külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="deb05-228">This setting informs hello Data Factory service that this dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span> <span data-ttu-id="deb05-229">Figyelje meg, hogy hello **típus** tulajdonsága túl**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="deb05-229">Notice that hello **type** property is set too**RelationalTable**.</span></span>


```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="deb05-230">**Az Azure Blob kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="deb05-230">**Azure Blob output dataset**</span></span>

<span data-ttu-id="deb05-231">Adatok írják tooa új blob óránként hello beállítása **gyakoriság** tulajdonság túl "Hour", és hello **időköz** tulajdonság too1.</span><span class="sxs-lookup"><span data-stu-id="deb05-231">Data is written tooa new blob every hour by setting hello **frequency** property too"Hour" and hello **interval** property too1.</span></span> <span data-ttu-id="deb05-232">Hello **folderPath** tulajdonság hello blob dinamikusan ki lesz értékelve az alapján a hello szelet által feldolgozott hello kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="deb05-232">hello **folderPath** property for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="deb05-233">hello mappa elérési útját használja hello év, hónap, nap és óra részei hello kezdési ideje.</span><span class="sxs-lookup"><span data-stu-id="deb05-233">hello folder path uses hello year, month, day, and hour parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobDb2DataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="deb05-234">**Feldolgozási sor hello másolási tevékenységhez**</span><span class="sxs-lookup"><span data-stu-id="deb05-234">**Pipeline for hello copy activity**</span></span>

<span data-ttu-id="deb05-235">hello feldolgozási sor tartalmazza a másolási tevékenység során konfigurált toouse megadott bemeneti és kimeneti adatkészletek, és amelyet ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="deb05-235">hello pipeline contains a copy activity that is configured toouse specified input and output datasets and which is scheduled toorun every hour.</span></span> <span data-ttu-id="deb05-236">A hello hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és hello **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="deb05-236">In hello JSON definition for hello pipeline, hello **source** type is set too**RelationalSource** and hello **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="deb05-237">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság kiválasztja hello adatokat hello "Rendelések" táblából.</span><span class="sxs-lookup"><span data-stu-id="deb05-237">hello SQL query specified for hello **query** property selects hello data from hello "Orders" table.</span></span>

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for hello copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"Orders\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "Db2DataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDb2DataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "Db2ToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-db2"></a><span data-ttu-id="deb05-238">Típusleképezés a DB2 rendszerhez</span><span class="sxs-lookup"><span data-stu-id="deb05-238">Type mapping for DB2</span></span>
<span data-ttu-id="deb05-239">Hello a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység az automatikus típuskonverziók típus toosink forrástípus hello kétlépéses módszer a következő segítségével hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="deb05-239">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy Activity performs automatic type conversions from source type toosink type by using hello following two-step approach:</span></span>

1. <span data-ttu-id="deb05-240">Egy natív típusa tooa .NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="deb05-240">Convert from a native source type tooa .NET type</span></span>
2. <span data-ttu-id="deb05-241">A .NET típusú tooa natív a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="deb05-241">Convert from a .NET type tooa native sink type</span></span>

<span data-ttu-id="deb05-242">hello következő megfeleltetéseket használata, amikor másolási tevékenység hello adatok konvertál egy DB2 tooa .NET típusának:</span><span class="sxs-lookup"><span data-stu-id="deb05-242">hello following mappings are used when Copy Activity converts hello data from a DB2 type tooa .NET type:</span></span>

| <span data-ttu-id="deb05-243">DB2-adatbázishoz típusa</span><span class="sxs-lookup"><span data-stu-id="deb05-243">DB2 database type</span></span> | <span data-ttu-id="deb05-244">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="deb05-244">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="deb05-245">SmallInt</span><span class="sxs-lookup"><span data-stu-id="deb05-245">SmallInt</span></span> |<span data-ttu-id="deb05-246">Int16</span><span class="sxs-lookup"><span data-stu-id="deb05-246">Int16</span></span> |
| <span data-ttu-id="deb05-247">Egész szám</span><span class="sxs-lookup"><span data-stu-id="deb05-247">Integer</span></span> |<span data-ttu-id="deb05-248">Int32</span><span class="sxs-lookup"><span data-stu-id="deb05-248">Int32</span></span> |
| <span data-ttu-id="deb05-249">BigInt</span><span class="sxs-lookup"><span data-stu-id="deb05-249">BigInt</span></span> |<span data-ttu-id="deb05-250">Int64</span><span class="sxs-lookup"><span data-stu-id="deb05-250">Int64</span></span> |
| <span data-ttu-id="deb05-251">Real</span><span class="sxs-lookup"><span data-stu-id="deb05-251">Real</span></span> |<span data-ttu-id="deb05-252">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="deb05-252">Single</span></span> |
| <span data-ttu-id="deb05-253">Dupla</span><span class="sxs-lookup"><span data-stu-id="deb05-253">Double</span></span> |<span data-ttu-id="deb05-254">Dupla</span><span class="sxs-lookup"><span data-stu-id="deb05-254">Double</span></span> |
| <span data-ttu-id="deb05-255">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="deb05-255">Float</span></span> |<span data-ttu-id="deb05-256">Dupla</span><span class="sxs-lookup"><span data-stu-id="deb05-256">Double</span></span> |
| <span data-ttu-id="deb05-257">Decimális</span><span class="sxs-lookup"><span data-stu-id="deb05-257">Decimal</span></span> |<span data-ttu-id="deb05-258">Decimális</span><span class="sxs-lookup"><span data-stu-id="deb05-258">Decimal</span></span> |
| <span data-ttu-id="deb05-259">DecimalFloat</span><span class="sxs-lookup"><span data-stu-id="deb05-259">DecimalFloat</span></span> |<span data-ttu-id="deb05-260">Decimális</span><span class="sxs-lookup"><span data-stu-id="deb05-260">Decimal</span></span> |
| <span data-ttu-id="deb05-261">Numerikus</span><span class="sxs-lookup"><span data-stu-id="deb05-261">Numeric</span></span> |<span data-ttu-id="deb05-262">Decimális</span><span class="sxs-lookup"><span data-stu-id="deb05-262">Decimal</span></span> |
| <span data-ttu-id="deb05-263">Dátum</span><span class="sxs-lookup"><span data-stu-id="deb05-263">Date</span></span> |<span data-ttu-id="deb05-264">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="deb05-264">DateTime</span></span> |
| <span data-ttu-id="deb05-265">Time</span><span class="sxs-lookup"><span data-stu-id="deb05-265">Time</span></span> |<span data-ttu-id="deb05-266">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="deb05-266">TimeSpan</span></span> |
| <span data-ttu-id="deb05-267">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="deb05-267">Timestamp</span></span> |<span data-ttu-id="deb05-268">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="deb05-268">DateTime</span></span> |
| <span data-ttu-id="deb05-269">XML</span><span class="sxs-lookup"><span data-stu-id="deb05-269">Xml</span></span> |<span data-ttu-id="deb05-270">Byte]</span><span class="sxs-lookup"><span data-stu-id="deb05-270">Byte[]</span></span> |
| <span data-ttu-id="deb05-271">Karakter</span><span class="sxs-lookup"><span data-stu-id="deb05-271">Char</span></span> |<span data-ttu-id="deb05-272">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-272">String</span></span> |
| <span data-ttu-id="deb05-273">VarChar</span><span class="sxs-lookup"><span data-stu-id="deb05-273">VarChar</span></span> |<span data-ttu-id="deb05-274">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-274">String</span></span> |
| <span data-ttu-id="deb05-275">LongVarChar</span><span class="sxs-lookup"><span data-stu-id="deb05-275">LongVarChar</span></span> |<span data-ttu-id="deb05-276">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-276">String</span></span> |
| <span data-ttu-id="deb05-277">DB2DynArray</span><span class="sxs-lookup"><span data-stu-id="deb05-277">DB2DynArray</span></span> |<span data-ttu-id="deb05-278">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-278">String</span></span> |
| <span data-ttu-id="deb05-279">Bináris</span><span class="sxs-lookup"><span data-stu-id="deb05-279">Binary</span></span> |<span data-ttu-id="deb05-280">Byte]</span><span class="sxs-lookup"><span data-stu-id="deb05-280">Byte[]</span></span> |
| <span data-ttu-id="deb05-281">VarBinary</span><span class="sxs-lookup"><span data-stu-id="deb05-281">VarBinary</span></span> |<span data-ttu-id="deb05-282">Byte]</span><span class="sxs-lookup"><span data-stu-id="deb05-282">Byte[]</span></span> |
| <span data-ttu-id="deb05-283">LongVarBinary</span><span class="sxs-lookup"><span data-stu-id="deb05-283">LongVarBinary</span></span> |<span data-ttu-id="deb05-284">Byte]</span><span class="sxs-lookup"><span data-stu-id="deb05-284">Byte[]</span></span> |
| <span data-ttu-id="deb05-285">Kép</span><span class="sxs-lookup"><span data-stu-id="deb05-285">Graphic</span></span> |<span data-ttu-id="deb05-286">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-286">String</span></span> |
| <span data-ttu-id="deb05-287">VarGraphic</span><span class="sxs-lookup"><span data-stu-id="deb05-287">VarGraphic</span></span> |<span data-ttu-id="deb05-288">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-288">String</span></span> |
| <span data-ttu-id="deb05-289">LongVarGraphic</span><span class="sxs-lookup"><span data-stu-id="deb05-289">LongVarGraphic</span></span> |<span data-ttu-id="deb05-290">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-290">String</span></span> |
| <span data-ttu-id="deb05-291">CLOB</span><span class="sxs-lookup"><span data-stu-id="deb05-291">Clob</span></span> |<span data-ttu-id="deb05-292">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-292">String</span></span> |
| <span data-ttu-id="deb05-293">Blob</span><span class="sxs-lookup"><span data-stu-id="deb05-293">Blob</span></span> |<span data-ttu-id="deb05-294">Byte]</span><span class="sxs-lookup"><span data-stu-id="deb05-294">Byte[]</span></span> |
| <span data-ttu-id="deb05-295">DbClob</span><span class="sxs-lookup"><span data-stu-id="deb05-295">DbClob</span></span> |<span data-ttu-id="deb05-296">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-296">String</span></span> |
| <span data-ttu-id="deb05-297">SmallInt</span><span class="sxs-lookup"><span data-stu-id="deb05-297">SmallInt</span></span> |<span data-ttu-id="deb05-298">Int16</span><span class="sxs-lookup"><span data-stu-id="deb05-298">Int16</span></span> |
| <span data-ttu-id="deb05-299">Egész szám</span><span class="sxs-lookup"><span data-stu-id="deb05-299">Integer</span></span> |<span data-ttu-id="deb05-300">Int32</span><span class="sxs-lookup"><span data-stu-id="deb05-300">Int32</span></span> |
| <span data-ttu-id="deb05-301">BigInt</span><span class="sxs-lookup"><span data-stu-id="deb05-301">BigInt</span></span> |<span data-ttu-id="deb05-302">Int64</span><span class="sxs-lookup"><span data-stu-id="deb05-302">Int64</span></span> |
| <span data-ttu-id="deb05-303">Real</span><span class="sxs-lookup"><span data-stu-id="deb05-303">Real</span></span> |<span data-ttu-id="deb05-304">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="deb05-304">Single</span></span> |
| <span data-ttu-id="deb05-305">Dupla</span><span class="sxs-lookup"><span data-stu-id="deb05-305">Double</span></span> |<span data-ttu-id="deb05-306">Dupla</span><span class="sxs-lookup"><span data-stu-id="deb05-306">Double</span></span> |
| <span data-ttu-id="deb05-307">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="deb05-307">Float</span></span> |<span data-ttu-id="deb05-308">Dupla</span><span class="sxs-lookup"><span data-stu-id="deb05-308">Double</span></span> |
| <span data-ttu-id="deb05-309">Decimális</span><span class="sxs-lookup"><span data-stu-id="deb05-309">Decimal</span></span> |<span data-ttu-id="deb05-310">Decimális</span><span class="sxs-lookup"><span data-stu-id="deb05-310">Decimal</span></span> |
| <span data-ttu-id="deb05-311">DecimalFloat</span><span class="sxs-lookup"><span data-stu-id="deb05-311">DecimalFloat</span></span> |<span data-ttu-id="deb05-312">Decimális</span><span class="sxs-lookup"><span data-stu-id="deb05-312">Decimal</span></span> |
| <span data-ttu-id="deb05-313">Numerikus</span><span class="sxs-lookup"><span data-stu-id="deb05-313">Numeric</span></span> |<span data-ttu-id="deb05-314">Decimális</span><span class="sxs-lookup"><span data-stu-id="deb05-314">Decimal</span></span> |
| <span data-ttu-id="deb05-315">Dátum</span><span class="sxs-lookup"><span data-stu-id="deb05-315">Date</span></span> |<span data-ttu-id="deb05-316">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="deb05-316">DateTime</span></span> |
| <span data-ttu-id="deb05-317">Time</span><span class="sxs-lookup"><span data-stu-id="deb05-317">Time</span></span> |<span data-ttu-id="deb05-318">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="deb05-318">TimeSpan</span></span> |
| <span data-ttu-id="deb05-319">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="deb05-319">Timestamp</span></span> |<span data-ttu-id="deb05-320">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="deb05-320">DateTime</span></span> |
| <span data-ttu-id="deb05-321">XML</span><span class="sxs-lookup"><span data-stu-id="deb05-321">Xml</span></span> |<span data-ttu-id="deb05-322">Byte]</span><span class="sxs-lookup"><span data-stu-id="deb05-322">Byte[]</span></span> |
| <span data-ttu-id="deb05-323">Karakter</span><span class="sxs-lookup"><span data-stu-id="deb05-323">Char</span></span> |<span data-ttu-id="deb05-324">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="deb05-324">String</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="deb05-325">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="deb05-325">Map source toosink columns</span></span>
<span data-ttu-id="deb05-326">Hogyan toomap hello forrás adatkészlet toocolumns hello fogadó adatkészletben, oszlopok: toolearn [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="deb05-326">toolearn how toomap columns in hello source dataset toocolumns in hello sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-reads-from-relational-sources"></a><span data-ttu-id="deb05-327">A relációs források ismételhető olvasási műveletek</span><span class="sxs-lookup"><span data-stu-id="deb05-327">Repeatable reads from relational sources</span></span>
<span data-ttu-id="deb05-328">Amikor adatokat másolni relációs adattároló, ismételhetőség tartsa szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="deb05-328">When you copy data from a relational data store, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="deb05-329">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="deb05-329">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="deb05-330">Beállíthatja úgy is hello újrapróbálkozási **házirend** tulajdonsága egy adatkészlet toorerun a szelet, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="deb05-330">You can also configure hello retry **policy** property for a dataset toorerun a slice when a failure occurs.</span></span> <span data-ttu-id="deb05-331">Győződjön meg arról, hogy hello ugyanazokat az adatokat hogyan olvasható függetlenül attól, hogy hányszor hello szelet a rendszer Újrafuttatás, függetlenül attól, milyen hello szelet futtassa újra.</span><span class="sxs-lookup"><span data-stu-id="deb05-331">Make sure that hello same data is read no matter how many times hello slice is rerun, and regardless of how you rerun hello slice.</span></span> <span data-ttu-id="deb05-332">További információkért lásd: [Repeatable olvassa be az relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="deb05-332">For more information, see [Repeatable reads from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="deb05-333">Teljesítmény és finomhangolás</span><span class="sxs-lookup"><span data-stu-id="deb05-333">Performance and tuning</span></span>
<span data-ttu-id="deb05-334">További tudnivalók a másolási tevékenység és módon toooptimize teljesítmény hello hello teljesítményét befolyásoló legfontosabb tényezők [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="deb05-334">Learn about key factors that affect hello performance of Copy Activity and ways toooptimize performance in hello [Copy Activity Performance and Tuning Guide](data-factory-copy-activity-performance.md).</span></span>
