---
title: "Az Azure Analysis Services támogatott adatforrások |} Microsoft Docs"
description: "Az Azure Analysis Services adatmodellekben támogatott adatforrások ismerteti."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 8bd6c3b5a923ce2f3cd0f60af82e59c5cc27cbb4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="data-sources-supported-in-azure-analysis-services"></a><span data-ttu-id="dd130-103">Az Azure Analysis Services támogatott adatforrások</span><span class="sxs-lookup"><span data-stu-id="dd130-103">Data sources supported in Azure Analysis Services</span></span>
<span data-ttu-id="dd130-104">Azure Analysis Services-kiszolgálók esetén támogatja az adatforrások a felhőben és a szervezet helyszíni csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="dd130-104">Azure Analysis Services servers support connecting to data sources in the cloud and on-premises in your organization.</span></span> <span data-ttu-id="dd130-105">További támogatott adatforrások folyamatosan felvétel alatt.</span><span class="sxs-lookup"><span data-stu-id="dd130-105">Additional supported data sources are being added all the time.</span></span> <span data-ttu-id="dd130-106">Gyakran ellátogatnia webhelyünkre.</span><span class="sxs-lookup"><span data-stu-id="dd130-106">Check back often.</span></span> 

<span data-ttu-id="dd130-107">A következő adatforrások jelenleg támogatottak:</span><span class="sxs-lookup"><span data-stu-id="dd130-107">The following data sources are currently supported:</span></span>

| <span data-ttu-id="dd130-108">Felhő</span><span class="sxs-lookup"><span data-stu-id="dd130-108">Cloud</span></span>  |
|---|
| <span data-ttu-id="dd130-109">Az Azure Blob Storage *</span><span class="sxs-lookup"><span data-stu-id="dd130-109">Azure Blob Storage*</span></span>  |
| <span data-ttu-id="dd130-110">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="dd130-110">Azure SQL Database</span></span>  |
| <span data-ttu-id="dd130-111">Az Azure Data warehouse-bA</span><span class="sxs-lookup"><span data-stu-id="dd130-111">Azure Data Warehouse</span></span> |


| <span data-ttu-id="dd130-112">Helyszíni követelmények</span><span class="sxs-lookup"><span data-stu-id="dd130-112">On-premises</span></span>  |   |   |   |
|---|---|---|---|
| <span data-ttu-id="dd130-113">Access-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="dd130-113">Access Database</span></span>  | <span data-ttu-id="dd130-114">Mappa *</span><span class="sxs-lookup"><span data-stu-id="dd130-114">Folder*</span></span> | <span data-ttu-id="dd130-115">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="dd130-115">Oracle Database</span></span>  | <span data-ttu-id="dd130-116">Teradata-adatbázis</span><span class="sxs-lookup"><span data-stu-id="dd130-116">Teradata Database</span></span> |
| <span data-ttu-id="dd130-117">Active Directory *</span><span class="sxs-lookup"><span data-stu-id="dd130-117">Active Directory*</span></span>  | <span data-ttu-id="dd130-118">JSON-dokumentum *</span><span class="sxs-lookup"><span data-stu-id="dd130-118">JSON document*</span></span>  | <span data-ttu-id="dd130-119">Postgre SQL adatbázis *</span><span class="sxs-lookup"><span data-stu-id="dd130-119">Postgre SQL Database*</span></span>  |<span data-ttu-id="dd130-120">XML tábla *</span><span class="sxs-lookup"><span data-stu-id="dd130-120">XML table*</span></span> |
| <span data-ttu-id="dd130-121">Analysis Services</span><span class="sxs-lookup"><span data-stu-id="dd130-121">Analysis Services</span></span>  | <span data-ttu-id="dd130-122">A bináris * sorok</span><span class="sxs-lookup"><span data-stu-id="dd130-122">Lines from binary*</span></span>  | <span data-ttu-id="dd130-123">SAP HANA *</span><span class="sxs-lookup"><span data-stu-id="dd130-123">SAP HANA*</span></span>  |
| <span data-ttu-id="dd130-124">Analytics Platform System</span><span class="sxs-lookup"><span data-stu-id="dd130-124">Analytics Platform System</span></span>  | <span data-ttu-id="dd130-125">MySQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="dd130-125">MySQL Database</span></span>  | <span data-ttu-id="dd130-126">SAP Business Warehouse *</span><span class="sxs-lookup"><span data-stu-id="dd130-126">SAP Business Warehouse*</span></span>  | |
| <span data-ttu-id="dd130-127">Dynamics CRM *</span><span class="sxs-lookup"><span data-stu-id="dd130-127">Dynamics CRM*</span></span>  | <span data-ttu-id="dd130-128">Az OData-adatcsatorna *</span><span class="sxs-lookup"><span data-stu-id="dd130-128">OData Feed*</span></span>  | <span data-ttu-id="dd130-129">SharePoint *</span><span class="sxs-lookup"><span data-stu-id="dd130-129">SharePoint*</span></span>  |
| <span data-ttu-id="dd130-130">Az Excel-munkafüzet</span><span class="sxs-lookup"><span data-stu-id="dd130-130">Excel workbook</span></span>  | <span data-ttu-id="dd130-131">Az ODBC-lekérdezés</span><span class="sxs-lookup"><span data-stu-id="dd130-131">ODBC query</span></span>  | <span data-ttu-id="dd130-132">SQL Database</span><span class="sxs-lookup"><span data-stu-id="dd130-132">SQL Database</span></span>  |
| <span data-ttu-id="dd130-133">Exchange *</span><span class="sxs-lookup"><span data-stu-id="dd130-133">Exchange*</span></span>  | <span data-ttu-id="dd130-134">OLE DB RENDSZERHEZ</span><span class="sxs-lookup"><span data-stu-id="dd130-134">OLE DB</span></span>  | <span data-ttu-id="dd130-135">Sybase-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="dd130-135">Sybase Database</span></span>  |

<span data-ttu-id="dd130-136">\*A táblázatos 1 400 modellek csak.</span><span class="sxs-lookup"><span data-stu-id="dd130-136">\* Tabular 1400 models only.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="dd130-137">A helyszíni adatforrásokhoz való csatlakozás igényel egy [helyszíni adatátjáró](analysis-services-gateway.md) környezetét a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="dd130-137">Connecting to on-premises data sources require an [On-premises data gateway](analysis-services-gateway.md) installed on a computer in your environment.</span></span>

## <a name="data-providers"></a><span data-ttu-id="dd130-138">Adatszolgáltatók</span><span class="sxs-lookup"><span data-stu-id="dd130-138">Data providers</span></span>

<span data-ttu-id="dd130-139">Az Azure Analysis Services adatmodellekben szükség lehet különböző adatszolgáltatók, egyes adatforrásokhoz való kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="dd130-139">Data models in Azure Analysis Services may require different data providers when connecting to certain data sources.</span></span> <span data-ttu-id="dd130-140">Bizonyos esetekben csatlakozás az adatforrásokhoz, például az SQL Server Native Client (SQLNCLI11) natív szolgáltatók használata táblázatos modellek hibát adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="dd130-140">In some cases, tabular models connecting to data sources using native providers such as SQL Server Native Client (SQLNCLI11) may return an error.</span></span>

<span data-ttu-id="dd130-141">Egy felhőbeli adatát csatlakozó adatmodellekben forrás például az Azure SQL Database, ha eltérő SQLOLEDB natív szolgáltatók használatához hibaüzenet jelenhet: **"a provider" "SQLNCLI11.1" nincs regisztrálva.**</span><span class="sxs-lookup"><span data-stu-id="dd130-141">For data models that connect to a cloud data source such as Azure SQL Database, if you use native providers other than SQLOLEDB, you may see error message: **“The provider 'SQLNCLI11.1' is not registered.”**</span></span> <span data-ttu-id="dd130-142">Vagy, ha egy DirectQuery modell csatlakozik a helyi adatforrások, ha natív szolgáltatók hibaüzenet jelenhet: **"hiba történt a sorkészlet OLE DB létrehozásakor. Helytelen szintaxis a(z) "KORLÁTJA" közelében "**.</span><span class="sxs-lookup"><span data-stu-id="dd130-142">Or, if you have a DirectQuery model connecting to on-premises data sources, if you use native providers you may see error message: **“Error creating OLE DB row set. Incorrect syntax near 'LIMIT'”**.</span></span>

<span data-ttu-id="dd130-143">A következő adatforrás-szolgáltatók támogatottak a memóriában vagy a DirectQuery-modellekre adatok a felhőben, vagy a helyszíni adatforrások való csatlakozáskor:</span><span class="sxs-lookup"><span data-stu-id="dd130-143">The following datasource providers are supported for in-memory or DirectQuery data models when connecting to data sources in the cloud or on-premises:</span></span>

### <a name="cloud"></a><span data-ttu-id="dd130-144">Felhő</span><span class="sxs-lookup"><span data-stu-id="dd130-144">Cloud</span></span>
| <span data-ttu-id="dd130-145">**Adatforrás**</span><span class="sxs-lookup"><span data-stu-id="dd130-145">**Datasource**</span></span> | <span data-ttu-id="dd130-146">**Memóriabeli**</span><span class="sxs-lookup"><span data-stu-id="dd130-146">**In-memory**</span></span> | <span data-ttu-id="dd130-147">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="dd130-147">**DirectQuery**</span></span> |
|  --- | --- | --- |
| <span data-ttu-id="dd130-148">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="dd130-148">Azure SQL Data Warehouse</span></span> |<span data-ttu-id="dd130-149">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-149">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="dd130-150">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-150">.NET Framework Data Provider for SQL Server</span></span> |
| <span data-ttu-id="dd130-151">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="dd130-151">Azure SQL Database</span></span> |<span data-ttu-id="dd130-152">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-152">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="dd130-153">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-153">.NET Framework Data Provider for SQL Server</span></span> | |

### <a name="on-premises-via-gateway"></a><span data-ttu-id="dd130-154">A helyszíni (átjáró) keresztül</span><span class="sxs-lookup"><span data-stu-id="dd130-154">On-premises (via Gateway)</span></span>
|<span data-ttu-id="dd130-155">**Adatforrás**</span><span class="sxs-lookup"><span data-stu-id="dd130-155">**Datasource**</span></span> | <span data-ttu-id="dd130-156">**Memóriabeli**</span><span class="sxs-lookup"><span data-stu-id="dd130-156">**In-memory**</span></span> | <span data-ttu-id="dd130-157">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="dd130-157">**DirectQuery**</span></span> |
|  --- | --- | --- |
| <span data-ttu-id="dd130-158">SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-158">SQL Server</span></span> |<span data-ttu-id="dd130-159">SQL Server natív ügyfél 11.0</span><span class="sxs-lookup"><span data-stu-id="dd130-159">SQL Server Native Client 11.0</span></span> |<span data-ttu-id="dd130-160">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-160">.NET Framework Data Provider for SQL Server</span></span> |
| <span data-ttu-id="dd130-161">SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-161">SQL Server</span></span> |<span data-ttu-id="dd130-162">Microsoft OLE DB-szolgáltató az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-162">Microsoft OLE DB Provider for SQL Server</span></span> |<span data-ttu-id="dd130-163">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-163">.NET Framework Data Provider for SQL Server</span></span> | |
| <span data-ttu-id="dd130-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-164">SQL Server</span></span> |<span data-ttu-id="dd130-165">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-165">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="dd130-166">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-166">.NET Framework Data Provider for SQL Server</span></span> | |
| <span data-ttu-id="dd130-167">Oracle</span><span class="sxs-lookup"><span data-stu-id="dd130-167">Oracle</span></span> |<span data-ttu-id="dd130-168">Microsoft OLE DB-szolgáltató az Oracle rendszerhez</span><span class="sxs-lookup"><span data-stu-id="dd130-168">Microsoft OLE DB Provider for Oracle</span></span> |<span data-ttu-id="dd130-169">Oracle-adatszolgáltatóban a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="dd130-169">Oracle Data Provider for .NET</span></span> | |
| <span data-ttu-id="dd130-170">Oracle</span><span class="sxs-lookup"><span data-stu-id="dd130-170">Oracle</span></span> |<span data-ttu-id="dd130-171">Oracle-adatszolgáltatóban a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="dd130-171">Oracle Data Provider for .NET</span></span> |<span data-ttu-id="dd130-172">Oracle-adatszolgáltatóban a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="dd130-172">Oracle Data Provider for .NET</span></span> | |
| <span data-ttu-id="dd130-173">Teradata</span><span class="sxs-lookup"><span data-stu-id="dd130-173">Teradata</span></span> |<span data-ttu-id="dd130-174">OLE DB-szolgáltató a teradata rendszerhez</span><span class="sxs-lookup"><span data-stu-id="dd130-174">OLE DB Provider for Teradata</span></span> |<span data-ttu-id="dd130-175">Teradata-adatszolgáltatója a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="dd130-175">Teradata Data Provider for .NET</span></span> | |
| <span data-ttu-id="dd130-176">Teradata</span><span class="sxs-lookup"><span data-stu-id="dd130-176">Teradata</span></span> |<span data-ttu-id="dd130-177">Teradata-adatszolgáltatója a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="dd130-177">Teradata Data Provider for .NET</span></span> |<span data-ttu-id="dd130-178">Teradata-adatszolgáltatója a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="dd130-178">Teradata Data Provider for .NET</span></span> | |
| <span data-ttu-id="dd130-179">Analytics Platform System</span><span class="sxs-lookup"><span data-stu-id="dd130-179">Analytics Platform System</span></span> |<span data-ttu-id="dd130-180">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-180">.NET Framework Data Provider for SQL Server</span></span> |<span data-ttu-id="dd130-181">.NET framework Data Provider az SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd130-181">.NET Framework Data Provider for SQL Server</span></span> | |

> [!NOTE]
> <span data-ttu-id="dd130-182">Győződjön meg arról, 64 bites szolgáltató van telepítve, a helyszíni átjáró használatakor.</span><span class="sxs-lookup"><span data-stu-id="dd130-182">Ensure 64-bit providers are installed when using On-premises gateway.</span></span>
> 
> 

<span data-ttu-id="dd130-183">Amikor telepít egy helyi SQL Server Analysis Services táblázatos modell Azure Analysis Services, akkor lehet szükség módosítani a szolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="dd130-183">When migrating an on-premises SQL Server Analysis Services tabular model to Azure Analysis Services, it may be necessary to change the provider.</span></span>

<span data-ttu-id="dd130-184">**Egy adatforrás-szolgáltatója megadása**</span><span class="sxs-lookup"><span data-stu-id="dd130-184">**To specify a datasource provider**</span></span>

1. <span data-ttu-id="dd130-185">Az SSDT > **táblázatos modell Explorer** > **adatforrások**, kattintson a jobb gombbal az adatforrás-kapcsolat, és kattintson a **adatforrás szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="dd130-185">In SSDT > **Tabular Model Explorer** > **Data Sources**, right-click a data source connection, and then click **Edit Data Source**.</span></span>
2. <span data-ttu-id="dd130-186">A **kapcsolat szerkesztése**, kattintson a **speciális** a Speciális tulajdonságok ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="dd130-186">In **Edit Connection**, click **Advanced** to open the Advance properties window.</span></span>
3. <span data-ttu-id="dd130-187">A **speciális tulajdonságok** > **szolgáltatók**, majd válassza ki a megfelelő szolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="dd130-187">In **Set Advanced Properties** > **Providers**, then select the appropriate provider.</span></span>

## <a name="impersonation"></a><span data-ttu-id="dd130-188">A megszemélyesítés</span><span class="sxs-lookup"><span data-stu-id="dd130-188">Impersonation</span></span>
<span data-ttu-id="dd130-189">Bizonyos esetekben elképzelhető különböző megszemélyesítési fiókot szeretne megadni.</span><span class="sxs-lookup"><span data-stu-id="dd130-189">In some cases, it may be necessary to specify a different impersonation account.</span></span> <span data-ttu-id="dd130-190">Megszemélyesítési fiókhoz SSDT vagy SSMS adható meg.</span><span class="sxs-lookup"><span data-stu-id="dd130-190">Impersonation account can be specified in SSDT or SSMS.</span></span>

<span data-ttu-id="dd130-191">A helyszíni adatforrások:</span><span class="sxs-lookup"><span data-stu-id="dd130-191">For on-premises data sources:</span></span>

* <span data-ttu-id="dd130-192">Ha az SQL-hitelesítést használ, a megszemélyesítési szolgáltatás fióknak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dd130-192">If using SQL authentication, impersonation should be Service Account.</span></span>
* <span data-ttu-id="dd130-193">Ha Windows-hitelesítést használ, állítsa be a Windows felhasználói/jelszó.</span><span class="sxs-lookup"><span data-stu-id="dd130-193">If using Windows authentication, set Windows user/password.</span></span> <span data-ttu-id="dd130-194">Az SQL Server Windows-hitelesítés egy adott megszemélyesítési fiókhoz csak támogatott a memóriában levő modellek.</span><span class="sxs-lookup"><span data-stu-id="dd130-194">For SQL Server, Windows authentication with a specific impersonation account is supported only for in-memory data models.</span></span>

<span data-ttu-id="dd130-195">Felhő adatforrások:</span><span class="sxs-lookup"><span data-stu-id="dd130-195">For cloud data sources:</span></span>

* <span data-ttu-id="dd130-196">Ha az SQL-hitelesítést használ, a megszemélyesítési szolgáltatás fióknak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dd130-196">If using SQL authentication, impersonation should be Service Account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd130-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dd130-197">Next steps</span></span>
<span data-ttu-id="dd130-198">Ha a helyszíni adatforrások, ügyeljen arra, hogy telepítse a [helyszíni átjáró](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="dd130-198">If you have on-premises data sources, be sure to install the [On-premises gateway](analysis-services-gateway.md).</span></span>   
<span data-ttu-id="dd130-199">A kiszolgáló SSDT vagy SSMS kezelésével kapcsolatos további tudnivalókért lásd: [kezelheti a kiszolgálót](analysis-services-manage.md).</span><span class="sxs-lookup"><span data-stu-id="dd130-199">To learn more about managing your server in SSDT or SSMS, see [Manage your server](analysis-services-manage.md).</span></span>

