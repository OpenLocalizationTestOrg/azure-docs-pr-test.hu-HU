---
title: "Azure Data Catalog támogatott adatforrások |} Microsoft Docs"
description: "Ez a cikk felsorolja a jelenleg támogatott adatforrások előírásainak."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: jstevens
editor: 
tags: 
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d6867c73bc6ea3c238cceef8a68466d451f3365c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="supported-data-sources-in-azure-data-catalog"></a><span data-ttu-id="2d667-103">Az Azure Data Catalog támogatott adatforrások</span><span class="sxs-lookup"><span data-stu-id="2d667-103">Supported data sources in Azure Data Catalog</span></span>

<span data-ttu-id="2d667-104">Egy nyilvános API-t vagy a kattintson a metaadatok közzététele-egyszer regisztrációs eszköz, vagy közvetlenül az Azure Data Catalog az információk manuális megadásával webes portál.</span><span class="sxs-lookup"><span data-stu-id="2d667-104">You can publish metadata by using a public API or a click-once registration tool, or by manually entering information directly to the Azure Data Catalog web portal.</span></span> <span data-ttu-id="2d667-105">Az alábbi táblázat foglalja össze az egyes ma a katalógusban, és a közzétételi lehetőségeket által támogatott összes adatforrás.</span><span class="sxs-lookup"><span data-stu-id="2d667-105">The following table summarizes all data sources that are supported by the catalog today, and the publishing capabilities for each.</span></span> <span data-ttu-id="2d667-106">A külső adatok eszközök, amelyek az egyes adatforrások is elindíthatja a "Megnyitás a következőben" portal tapasztalatunkat is listában megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="2d667-106">Also listed are the external data tools that each data source can launch from our portal "open-in" experience.</span></span> <span data-ttu-id="2d667-107">A második tábla minden egyes adatforrás kapcsolat tulajdonság olyan további technikai meghatározást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2d667-107">The second table contains a more technical specification of each data-source connection property.</span></span>


## <a name="list-of-supported-data-sources"></a><span data-ttu-id="2d667-108">A támogatott adatforrások listája</span><span class="sxs-lookup"><span data-stu-id="2d667-108">List of supported data sources</span></span>

<table>
    <tr>
       <td><span data-ttu-id="2d667-109"><b>Adatforrás-objektum</b></span><span class="sxs-lookup"><span data-stu-id="2d667-109"><b>Data source object</b></span></span></td>
       <td><span data-ttu-id="2d667-110"><b>API</b></span><span class="sxs-lookup"><span data-stu-id="2d667-110"><b>API</b></span></span></td>
       <td><span data-ttu-id="2d667-111"><b>Kézi bevitele</b></span><span class="sxs-lookup"><span data-stu-id="2d667-111"><b>Manual entry</b></span></span></td>
       <td><span data-ttu-id="2d667-112"><b>Regisztrációs eszköz</b></span><span class="sxs-lookup"><span data-stu-id="2d667-112"><b>Registration tool</b></span></span></td>
       <td><span data-ttu-id="2d667-113"><b>Nyissa meg az eszközök</b></span><span class="sxs-lookup"><span data-stu-id="2d667-113"><b>Open-in tools</b></span></span></td>
       <td><span data-ttu-id="2d667-114"><b>Megjegyzések</b></span><span class="sxs-lookup"><span data-stu-id="2d667-114"><b>Notes</b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-115">Azure Data Lake Store-könyvtár</span><span class="sxs-lookup"><span data-stu-id="2d667-115">Azure Data Lake Store directory</span></span></td>
      <td><span data-ttu-id="2d667-116">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-116">✓</span></span></td>
      <td><span data-ttu-id="2d667-117">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-117">✓</span></span></td>
      <td><span data-ttu-id="2d667-118">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-118">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-119">Azure Data Lake Store-fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-119">Azure Data Lake Store file</span></span></td>
      <td><span data-ttu-id="2d667-120">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-120">✓</span></span></td>
      <td><span data-ttu-id="2d667-121">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-121">✓</span></span></td>
      <td><span data-ttu-id="2d667-122">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-122">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-123">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="2d667-123">Azure Blob storage</span></span></td>
      <td><span data-ttu-id="2d667-124">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-124">✓</span></span></td>
      <td><span data-ttu-id="2d667-125">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-125">✓</span></span></td>
      <td><span data-ttu-id="2d667-126">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-126">✓</span></span></td>
      <td><span data-ttu-id="2d667-127"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2d667-127"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-128">Azure Storage-címtár</span><span class="sxs-lookup"><span data-stu-id="2d667-128">Azure Storage directory</span></span></td>
      <td><span data-ttu-id="2d667-129">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-129">✓</span></span></td>
      <td><span data-ttu-id="2d667-130">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-130">✓</span></span></td>
      <td><span data-ttu-id="2d667-131">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-131">✓</span></span></td>
      <td><span data-ttu-id="2d667-132"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2d667-132"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-133">Azure Storage-tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-133">Azure Storage table</span></span></td>
      <td><span data-ttu-id="2d667-134">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-134">✓</span></span></td>
      <td><span data-ttu-id="2d667-135">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-135">✓</span></span></td>
      <td><span data-ttu-id="2d667-136">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-136">✓</span></span></td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-137">HDFS könyvtár</span><span class="sxs-lookup"><span data-stu-id="2d667-137">HDFS directory</span></span></td>
      <td><span data-ttu-id="2d667-138">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-138">✓</span></span></td>
      <td><span data-ttu-id="2d667-139">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-139">✓</span></span></td>
      <td><span data-ttu-id="2d667-140">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-140">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-141">HDFS-fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-141">HDFS file</span></span></td>
      <td><span data-ttu-id="2d667-142">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-142">✓</span></span></td>
      <td><span data-ttu-id="2d667-143">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-143">✓</span></span></td>
      <td><span data-ttu-id="2d667-144">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-144">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-145">Hive tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-145">Hive table</span></span></td>
      <td><span data-ttu-id="2d667-146">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-146">✓</span></span></td>
      <td><span data-ttu-id="2d667-147">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-147">✓</span></span></td>
      <td><span data-ttu-id="2d667-148">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-148">✓</span></span></td>
      <td><span data-ttu-id="2d667-149"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="2d667-149"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-150">Hive nézete</span><span class="sxs-lookup"><span data-stu-id="2d667-150">Hive view</span></span></td>
      <td><span data-ttu-id="2d667-151">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-151">✓</span></span></td>
      <td><span data-ttu-id="2d667-152">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-152">✓</span></span></td>
      <td><span data-ttu-id="2d667-153">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-153">✓</span></span></td>
      <td><span data-ttu-id="2d667-154"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="2d667-154"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-155">MySQL-tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-155">MySQL table</span></span></td>
      <td><span data-ttu-id="2d667-156">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-156">✓</span></span></td>
      <td><span data-ttu-id="2d667-157">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-157">✓</span></span></td>
      <td><span data-ttu-id="2d667-158">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-158">✓</span></span></td>
      <td><span data-ttu-id="2d667-159"><font size=2>Az Excel, a Power bi-ban</font></span><span class="sxs-lookup"><span data-stu-id="2d667-159"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-160">MySQL-nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-160">MySQL view</span></span></td>
      <td><span data-ttu-id="2d667-161">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-161">✓</span></span></td>
      <td><span data-ttu-id="2d667-162">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-162">✓</span></span></td>
      <td><span data-ttu-id="2d667-163">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-163">✓</span></span></td>
      <td><span data-ttu-id="2d667-164"><font size=2>Az Excel, a Power bi-ban</font></span><span class="sxs-lookup"><span data-stu-id="2d667-164"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-165">Oracle Database tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-165">Oracle Database table</span></span></td>
      <td><span data-ttu-id="2d667-166">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-166">✓</span></span></td>
      <td><span data-ttu-id="2d667-167">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-167">✓</span></span></td>
      <td><span data-ttu-id="2d667-168">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-168">✓</span></span></td>
      <td><span data-ttu-id="2d667-169"><font size=2>Az Excel, a Power bi-ban</font></span><span class="sxs-lookup"><span data-stu-id="2d667-169"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-170">Oracle Database megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-170">Oracle Database view</span></span></td>
      <td><span data-ttu-id="2d667-171">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-171">✓</span></span></td>
      <td><span data-ttu-id="2d667-172">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-172">✓</span></span></td>
      <td><span data-ttu-id="2d667-173">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-173">✓</span></span></td>
      <td><span data-ttu-id="2d667-174"><font size=2>Az Excel, a Power bi-ban</font></span><span class="sxs-lookup"><span data-stu-id="2d667-174"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-175">Más (általános eszköz)</span><span class="sxs-lookup"><span data-stu-id="2d667-175">Other (generic asset)</span></span></td>
      <td><span data-ttu-id="2d667-176">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-176">✓</span></span></td>
      <td><span data-ttu-id="2d667-177">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-177">✓</span></span></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-178">Azure SQL Data Warehouse-tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-178">Azure SQL Data Warehouse table</span></span></td>
      <td><span data-ttu-id="2d667-179">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-179">✓</span></span></td>
      <td><span data-ttu-id="2d667-180">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-180">✓</span></span></td>
      <td><span data-ttu-id="2d667-181">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-181">✓</span></span></td>
      <td><span data-ttu-id="2d667-182"><font size=2>Az Excel, a Power BI-ban az SQL Server data Tools összetevővel</font></span><span class="sxs-lookup"><span data-stu-id="2d667-182"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-183">Az SQL Data Warehouse megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-183">SQL Data Warehouse view</span></span></td>
      <td><span data-ttu-id="2d667-184">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-184">✓</span></span></td>
      <td><span data-ttu-id="2d667-185">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-185">✓</span></span></td>
      <td><span data-ttu-id="2d667-186">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-186">✓</span></span></td>
      <td><span data-ttu-id="2d667-187"><font size=2>Az Excel, a Power BI-ban az SQL Server data Tools összetevővel</font></span><span class="sxs-lookup"><span data-stu-id="2d667-187"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-188">SQL Server Analysis Services dimenzió</span><span class="sxs-lookup"><span data-stu-id="2d667-188">SQL Server Analysis Services dimension</span></span></td>
      <td><span data-ttu-id="2d667-189">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-189">✓</span></span></td>
      <td><span data-ttu-id="2d667-190">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-190">✓</span></span></td>
      <td><span data-ttu-id="2d667-191">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-191">✓</span></span></td>
      <td><span data-ttu-id="2d667-192"><font size=2>Az Excel, a Power bi-ban</font></span><span class="sxs-lookup"><span data-stu-id="2d667-192"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-193">Az SQL Server Analysis Services KPI</span><span class="sxs-lookup"><span data-stu-id="2d667-193">SQL Server Analysis Services KPI</span></span></td>
      <td><span data-ttu-id="2d667-194">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-194">✓</span></span></td>
      <td><span data-ttu-id="2d667-195">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-195">✓</span></span></td>
      <td><span data-ttu-id="2d667-196">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-196">✓</span></span></td>
      <td><span data-ttu-id="2d667-197"><font size=2>Az Excel, a Power bi-ban</font></span><span class="sxs-lookup"><span data-stu-id="2d667-197"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-198">SQL Server Analysis Services mérték</span><span class="sxs-lookup"><span data-stu-id="2d667-198">SQL Server Analysis Services measure</span></span></td>
      <td><span data-ttu-id="2d667-199">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-199">✓</span></span></td>
      <td><span data-ttu-id="2d667-200">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-200">✓</span></span></td>
      <td><span data-ttu-id="2d667-201">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-201">✓</span></span></td>
      <td><span data-ttu-id="2d667-202"><font size=2>Az Excel, a Power bi-ban</font></span><span class="sxs-lookup"><span data-stu-id="2d667-202"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-203">SQL Server Analysis Services tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-203">SQL Server Analysis Services table</span></span></td>
      <td><span data-ttu-id="2d667-204">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-204">✓</span></span></td>
      <td><span data-ttu-id="2d667-205">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-205">✓</span></span></td>
      <td><span data-ttu-id="2d667-206">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-206">✓</span></span></td>
      <td><span data-ttu-id="2d667-207"><font size=2>Az Excel, a Power bi-ban</font></span><span class="sxs-lookup"><span data-stu-id="2d667-207"><font size=2>Excel, Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-208">SQL Server Reporting Services jelentés</span><span class="sxs-lookup"><span data-stu-id="2d667-208">SQL Server Reporting Services report</span></span></td>
      <td><span data-ttu-id="2d667-209">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-209">✓</span></span></td>
      <td><span data-ttu-id="2d667-210">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-210">✓</span></span></td>
      <td><span data-ttu-id="2d667-211">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-211">✓</span></span></td>
      <td><span data-ttu-id="2d667-212"><font size=2>Böngésző</font></span><span class="sxs-lookup"><span data-stu-id="2d667-212"><font size=2>Browser</font></span></span></td>
      <td><span data-ttu-id="2d667-213"><font size=2>Csak natív üzemmódú kiszolgálók esetén. SharePoint-módban nem támogatott.</font></span><span class="sxs-lookup"><span data-stu-id="2d667-213"><font size=2>Native mode servers only. SharePoint mode is not supported.</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-214">SQL Server tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-214">SQL Server table</span></span></td>
      <td><span data-ttu-id="2d667-215">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-215">✓</span></span></td>
      <td><span data-ttu-id="2d667-216">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-216">✓</span></span></td>
      <td><span data-ttu-id="2d667-217">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-217">✓</span></span></td>
      <td><span data-ttu-id="2d667-218"><font size=2>Az Excel, a Power BI-ban az SQL Server data Tools összetevővel</font></span><span class="sxs-lookup"><span data-stu-id="2d667-218"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-219">SQL Server megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-219">SQL Server view</span></span></td>
      <td><span data-ttu-id="2d667-220">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-220">✓</span></span></td>
      <td><span data-ttu-id="2d667-221">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-221">✓</span></span></td>
      <td><span data-ttu-id="2d667-222">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-222">✓</span></span></td>
      <td><span data-ttu-id="2d667-223"><font size=2>Az Excel, a Power BI-ban az SQL Server data Tools összetevővel</font></span><span class="sxs-lookup"><span data-stu-id="2d667-223"><font size=2>Excel, Power BI, SQL Server data tools</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-224">Teradata tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-224">Teradata table</span></span></td>
      <td><span data-ttu-id="2d667-225">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-225">✓</span></span></td>
      <td><span data-ttu-id="2d667-226">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-226">✓</span></span></td>
      <td><span data-ttu-id="2d667-227">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-227">✓</span></span></td>
      <td><span data-ttu-id="2d667-228"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="2d667-228"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-229">Teradata megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-229">Teradata view</span></span></td>
      <td><span data-ttu-id="2d667-230">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-230">✓</span></span></td>
      <td><span data-ttu-id="2d667-231">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-231">✓</span></span></td>
      <td><span data-ttu-id="2d667-232">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-232">✓</span></span></td>
      <td><span data-ttu-id="2d667-233"><font size=2>Excel</font></span><span class="sxs-lookup"><span data-stu-id="2d667-233"><font size=2>Excel</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-234">SAP HANA-nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-234">SAP HANA view</span></span></td>
      <td><span data-ttu-id="2d667-235">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-235">✓</span></span></td>
      <td><span data-ttu-id="2d667-236">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-236">✓</span></span></td>
      <td><span data-ttu-id="2d667-237">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-237">✓</span></span></td>
      <td><span data-ttu-id="2d667-238"><font size=2>Power BI</font></span><span class="sxs-lookup"><span data-stu-id="2d667-238"><font size=2>Power BI</font></span></span></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-239">DB2 tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-239">DB2 table</span></span></td>
      <td><span data-ttu-id="2d667-240">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-240">✓</span></span></td>
      <td><span data-ttu-id="2d667-241">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-241">✓</span></span></td>
      <td><span data-ttu-id="2d667-242">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-242">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-243">DB2 megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-243">DB2 view</span></span></td>
      <td><span data-ttu-id="2d667-244">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-244">✓</span></span></td>
      <td><span data-ttu-id="2d667-245">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-245">✓</span></span></td>
      <td><span data-ttu-id="2d667-246">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-246">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-247">Rendszer-fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-247">File system file</span></span></td>
      <td><span data-ttu-id="2d667-248">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-248">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-249">FTP-könyvtár</span><span class="sxs-lookup"><span data-stu-id="2d667-249">FTP directory</span></span></td>
      <td><span data-ttu-id="2d667-250">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-250">✓</span></span></td>
      <td><span data-ttu-id="2d667-251">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-251">✓</span></span></td>
      <td><span data-ttu-id="2d667-252">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-252">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-253">FTP-fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-253">FTP file</span></span></td>
      <td><span data-ttu-id="2d667-254">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-254">✓</span></span></td>
      <td><span data-ttu-id="2d667-255">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-255">✓</span></span></td>
      <td><span data-ttu-id="2d667-256">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-256">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-257">HTTP-jelentés</span><span class="sxs-lookup"><span data-stu-id="2d667-257">HTTP report</span></span></td>
      <td><span data-ttu-id="2d667-258">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-258">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-259">HTTP-végpont</span><span class="sxs-lookup"><span data-stu-id="2d667-259">HTTP endpoint</span></span></td>
      <td><span data-ttu-id="2d667-260">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-260">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-261">HTTP-fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-261">HTTP file</span></span></td>
      <td><span data-ttu-id="2d667-262">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-262">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-263">OData entitáskészlet</span><span class="sxs-lookup"><span data-stu-id="2d667-263">OData entity set</span></span></td>
      <td><span data-ttu-id="2d667-264">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-264">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-265">Az OData-funkció</span><span class="sxs-lookup"><span data-stu-id="2d667-265">OData function</span></span></td>
      <td><span data-ttu-id="2d667-266">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-266">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-267">PostgreSQL tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-267">PostgreSQL table</span></span></td>
      <td><span data-ttu-id="2d667-268">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-268">✓</span></span></td>
      <td><span data-ttu-id="2d667-269">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-269">✓</span></span></td>
      <td><span data-ttu-id="2d667-270">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-270">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-271">PostgreSQL megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-271">PostgreSQL view</span></span></td>
      <td><span data-ttu-id="2d667-272">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-272">✓</span></span></td>
      <td><span data-ttu-id="2d667-273">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-273">✓</span></span></td>
      <td><span data-ttu-id="2d667-274">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-274">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-275">SAP HANA-nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-275">SAP HANA view</span></span></td>
      <td><span data-ttu-id="2d667-276">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-276">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> <span data-ttu-id="2d667-277">Salesforce-objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-277">Salesforce object</span></span></td>
      <td><span data-ttu-id="2d667-278">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-278">✓</span></span></td>
      <td><span data-ttu-id="2d667-279">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-279">✓</span></span></td>
      <td><span data-ttu-id="2d667-280">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-280">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-281">SharePoint-lista</span><span class="sxs-lookup"><span data-stu-id="2d667-281">SharePoint list</span></span> </td>
      <td><span data-ttu-id="2d667-282">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-282">✓</span></span></td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-283">Az Azure Cosmos DB gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="2d667-283">Azure Cosmos DB collection</span></span></td>
      <td><span data-ttu-id="2d667-284">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-284">✓</span></span></td>
      <td><span data-ttu-id="2d667-285">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-285">✓</span></span></td>
      <td><span data-ttu-id="2d667-286">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-286">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-287">Általános ODBC tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-287">Generic ODBC table</span></span></td>
      <td><span data-ttu-id="2d667-288">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-288">✓</span></span></td>
      <td><span data-ttu-id="2d667-289">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-289">✓</span></span></td>
      <td><span data-ttu-id="2d667-290">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-290">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-291">Általános ODBC megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-291">Generic ODBC view</span></span></td>
      <td><span data-ttu-id="2d667-292">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-292">✓</span></span></td>
      <td><span data-ttu-id="2d667-293">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-293">✓</span></span></td>
      <td><span data-ttu-id="2d667-294">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-294">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-295">Cassandra tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-295">Cassandra table</span></span></td>
      <td><span data-ttu-id="2d667-296">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-296">✓</span></span></td>
      <td><span data-ttu-id="2d667-297">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-297">✓</span></span></td>
      <td><span data-ttu-id="2d667-298">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-298">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="2d667-299"><font size=2>Általános eszközként ODBC közzététele</font></span><span class="sxs-lookup"><span data-stu-id="2d667-299"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-300">Cassandra megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-300">Cassandra view</span></span></td>
      <td><span data-ttu-id="2d667-301">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-301">✓</span></span></td>
      <td><span data-ttu-id="2d667-302">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-302">✓</span></span></td>
      <td><span data-ttu-id="2d667-303">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-303">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="2d667-304"><font size=2>Általános eszközként ODBC közzététele</font></span><span class="sxs-lookup"><span data-stu-id="2d667-304"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-305">Sybase tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-305">Sybase table</span></span></td>
      <td><span data-ttu-id="2d667-306">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-306">✓</span></span></td>
      <td><span data-ttu-id="2d667-307">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-307">✓</span></span></td>
      <td><span data-ttu-id="2d667-308">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-308">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-309">Sybase megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-309">Sybase view</span></span></td>
      <td><span data-ttu-id="2d667-310">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-310">✓</span></span></td>
      <td><span data-ttu-id="2d667-311">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-311">✓</span></span></td>
      <td><span data-ttu-id="2d667-312">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-312">✓</span></span></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-313">MongoDB tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-313">MongoDB table</span></span></td>
      <td><span data-ttu-id="2d667-314">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-314">✓</span></span></td>
      <td><span data-ttu-id="2d667-315">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-315">✓</span></span></td>
      <td><span data-ttu-id="2d667-316">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-316">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="2d667-317"><font size=2>Általános eszközként ODBC közzététele</font></span><span class="sxs-lookup"><span data-stu-id="2d667-317"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-318">MongoDB megtekintése</span><span class="sxs-lookup"><span data-stu-id="2d667-318">MongoDB view</span></span></td>
      <td><span data-ttu-id="2d667-319">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-319">✓</span></span></td>
      <td><span data-ttu-id="2d667-320">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-320">✓</span></span></td>
      <td><span data-ttu-id="2d667-321">✓</span><span class="sxs-lookup"><span data-stu-id="2d667-321">✓</span></span></td>
      <td><font size=2></font></td>
      <td><span data-ttu-id="2d667-322"><font size=2>Általános eszközként ODBC közzététele</font></span><span class="sxs-lookup"><span data-stu-id="2d667-322"><font size=2>Publish as a generic ODBC asset</font></span></span></td>
    </tr>
</table>

<span data-ttu-id="2d667-323">Ha további segítségre, küldje el a szolgáltatás kérelmet a [Azure Data Catalog fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="2d667-323">If you need support for additional sources, submit a feature request to the [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).</span></span>


## <a name="data-source-reference-specification"></a><span data-ttu-id="2d667-324">Adatforrás hivatkozás megadása</span><span class="sxs-lookup"><span data-stu-id="2d667-324">Data-source reference specification</span></span>
> [!NOTE]
> <span data-ttu-id="2d667-325">A **DSL struktúra** oszlop a következő táblázat felsorolja a csak a csatlakozási tulajdonságokat az "address" tulajdonságcsomag Azure Data Catalog által használt.</span><span class="sxs-lookup"><span data-stu-id="2d667-325">The **DSL structure** column in the following table lists only the connection properties for "address" property bag that are used by Azure Data Catalog.</span></span> <span data-ttu-id="2d667-326">Ez azt jelenti, hogy a "address" tulajdonságcsomag tartalmazhat az adatforrás, amely az Azure Data Catalog továbbra is fennáll, de nem használ más kapcsolat tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="2d667-326">That is, "address" property bag can contain other connection properties of the data source which Azure Data Catalog persists, but does not use.</span></span>

<table>
    <tr>
       <td><span data-ttu-id="2d667-327"><b>Adatforrás típusa</b></span><span class="sxs-lookup"><span data-stu-id="2d667-327"><b>Source type</b></span></span></td>
       <td><span data-ttu-id="2d667-328"><b>Eszköz típusa</b></span><span class="sxs-lookup"><span data-stu-id="2d667-328"><b>Asset type</b></span></span></td>
       <td><span data-ttu-id="2d667-329"><b>Objektumtípus</b></span><span class="sxs-lookup"><span data-stu-id="2d667-329"><b>Object types</b></span></span></td>
       <td><span data-ttu-id="2d667-330"><b>DSL struktúra<b></span><span class="sxs-lookup"><span data-stu-id="2d667-330"><b>DSL structure<b></span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-331">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2d667-331">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="2d667-332">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-332">Container</span></span></td>
      <td><span data-ttu-id="2d667-333">Data Lake</span><span class="sxs-lookup"><span data-stu-id="2d667-333">Data Lake</span></span></td>
      <td><span data-ttu-id="2d667-334">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="2d667-334">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="2d667-335">Hitelesítési: {alapvető, oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-335">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="2d667-336">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-336">Address:</span></span> <br><span data-ttu-id="2d667-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-337">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-338">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2d667-338">Azure Data Lake Store</span></span></td>
      <td><span data-ttu-id="2d667-339">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-339">Table</span></span></td>
      <td><span data-ttu-id="2d667-340">Könyvtár, fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-340">Directory, file</span></span></td>
      <td><span data-ttu-id="2d667-341">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="2d667-341">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="2d667-342">Hitelesítési: {alapvető, oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-342">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="2d667-343">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-343">Address:</span></span> <br><span data-ttu-id="2d667-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-344">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-345">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2d667-345">Azure Storage</span></span></td>
      <td><span data-ttu-id="2d667-346">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-346">Container</span></span></td>
      <td><span data-ttu-id="2d667-347">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-347">Container</span></span></td>
      <td><span data-ttu-id="2d667-348">
        <font size=2>Protokoll: azure-blobot</span><span class="sxs-lookup"><span data-stu-id="2d667-348">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="2d667-349">Hitelesítési: {azure-access-kulcs}</span><span class="sxs-lookup"><span data-stu-id="2d667-349">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2d667-350">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-350">Address:</span></span> <br><span data-ttu-id="2d667-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tartomány</span><span class="sxs-lookup"><span data-stu-id="2d667-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="2d667-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;fiók</span><span class="sxs-lookup"><span data-stu-id="2d667-352">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="2d667-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tároló</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-353">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-354">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2d667-354">Azure Storage</span></span></td>
      <td><span data-ttu-id="2d667-355">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-355">Table</span></span></td>
      <td><span data-ttu-id="2d667-356">A BLOB-, könyvtár</span><span class="sxs-lookup"><span data-stu-id="2d667-356">Blob, directory</span></span></td>
      <td><span data-ttu-id="2d667-357">
        <font size=2>Protokoll: azure-blobot</span><span class="sxs-lookup"><span data-stu-id="2d667-357">
        <font size=2> Protocol: azure-blobs</span></span> <br><span data-ttu-id="2d667-358">Hitelesítési: {azure-access-kulcs}</span><span class="sxs-lookup"><span data-stu-id="2d667-358">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2d667-359">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-359">Address:</span></span> <br><span data-ttu-id="2d667-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tartomány</span><span class="sxs-lookup"><span data-stu-id="2d667-360">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="2d667-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;fiók</span><span class="sxs-lookup"><span data-stu-id="2d667-361">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="2d667-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container</span></span> <br><span data-ttu-id="2d667-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;név</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-364">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2d667-364">Azure Storage</span></span></td>
      <td><span data-ttu-id="2d667-365">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-365">Container</span></span></td>
      <td><span data-ttu-id="2d667-366">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-366">Container</span></span></td>
      <td><span data-ttu-id="2d667-367">
        <font size=2>Protokoll: azure-táblák</span><span class="sxs-lookup"><span data-stu-id="2d667-367">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="2d667-368">Hitelesítési: {azure-access-kulcs}</span><span class="sxs-lookup"><span data-stu-id="2d667-368">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2d667-369">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-369">Address:</span></span> <br><span data-ttu-id="2d667-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tartomány</span><span class="sxs-lookup"><span data-stu-id="2d667-370">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="2d667-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;fiók</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-371">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-372">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2d667-372">Azure Storage</span></span></td>
      <td><span data-ttu-id="2d667-373">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-373">Table</span></span></td>
      <td><span data-ttu-id="2d667-374">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-374">Table</span></span></td>
      <td><span data-ttu-id="2d667-375">
        <font size=2>Protokoll: azure-táblák</span><span class="sxs-lookup"><span data-stu-id="2d667-375">
        <font size=2> Protocol: azure-tables</span></span> <br><span data-ttu-id="2d667-376">Hitelesítési: {azure-access-kulcs}</span><span class="sxs-lookup"><span data-stu-id="2d667-376">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2d667-377">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-377">Address:</span></span> <br><span data-ttu-id="2d667-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tartomány</span><span class="sxs-lookup"><span data-stu-id="2d667-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain</span></span> <br><span data-ttu-id="2d667-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;fiók</span><span class="sxs-lookup"><span data-stu-id="2d667-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account</span></span> <br><span data-ttu-id="2d667-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;név</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-380">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-381">Cosmos</span><span class="sxs-lookup"><span data-stu-id="2d667-381">Cosmos</span></span></td>
      <td><span data-ttu-id="2d667-382">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-382">Container</span></span></td>
      <td><span data-ttu-id="2d667-383">Virtuális fürt</span><span class="sxs-lookup"><span data-stu-id="2d667-383">Virtual cluster</span></span></td>
      <td><span data-ttu-id="2d667-384">
        <font size=2>Protokoll: cosmos</span><span class="sxs-lookup"><span data-stu-id="2d667-384">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="2d667-385">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-385">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-386">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-386">Address:</span></span> <br><span data-ttu-id="2d667-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-387">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-388">Cosmos</span><span class="sxs-lookup"><span data-stu-id="2d667-388">Cosmos</span></span></td>
      <td><span data-ttu-id="2d667-389">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-389">Table</span></span></td>
      <td><span data-ttu-id="2d667-390">Az adatfolyam, az adatfolyam be van állítva, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-390">Stream, stream set, view</span></span></td>
      <td><span data-ttu-id="2d667-391">
        <font size=2>Protokoll: cosmos</span><span class="sxs-lookup"><span data-stu-id="2d667-391">
        <font size=2> Protocol: cosmos</span></span> <br><span data-ttu-id="2d667-392">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-392">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-393">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-393">Address:</span></span> <br><span data-ttu-id="2d667-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-395">Datazen</span><span class="sxs-lookup"><span data-stu-id="2d667-395">Datazen</span></span></td>
      <td><span data-ttu-id="2d667-396">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-396">Container</span></span></td>
      <td><span data-ttu-id="2d667-397">Helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-397">Site</span></span></td>
      <td><span data-ttu-id="2d667-398">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="2d667-398">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2d667-399">Hitelesítési: {none, basic, a windows, a oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-399">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2d667-400">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-400">Address:</span></span> <br><span data-ttu-id="2d667-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-401">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-402">Datazen</span><span class="sxs-lookup"><span data-stu-id="2d667-402">Datazen</span></span></td>
      <td><span data-ttu-id="2d667-403">Jelentés</span><span class="sxs-lookup"><span data-stu-id="2d667-403">Report</span></span></td>
      <td><span data-ttu-id="2d667-404">A jelentés, az irányítópult</span><span class="sxs-lookup"><span data-stu-id="2d667-404">Report, dashboard</span></span></td>
      <td><span data-ttu-id="2d667-405">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="2d667-405">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2d667-406">Hitelesítési: {none, basic, a windows, a oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-406">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2d667-407">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-407">Address:</span></span> <br><span data-ttu-id="2d667-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-408">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-409">DB2</span><span class="sxs-lookup"><span data-stu-id="2d667-409">DB2</span></span></td>
      <td><span data-ttu-id="2d667-410">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-410">Container</span></span></td>
      <td><span data-ttu-id="2d667-411">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-411">Database</span></span></td>
      <td><span data-ttu-id="2d667-412">
        <font size=2>Protokoll: db2</span><span class="sxs-lookup"><span data-stu-id="2d667-412">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="2d667-413">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-413">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-414">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-414">Address:</span></span> <br><span data-ttu-id="2d667-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-415">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-416">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-417">DB2</span><span class="sxs-lookup"><span data-stu-id="2d667-417">DB2</span></span></td>
      <td><span data-ttu-id="2d667-418">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-418">Table</span></span></td>
      <td><span data-ttu-id="2d667-419">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-419">Table, view</span></span></td>
      <td><span data-ttu-id="2d667-420">
        <font size=2>Protokoll: db2</span><span class="sxs-lookup"><span data-stu-id="2d667-420">
        <font size=2> Protocol: db2</span></span> <br><span data-ttu-id="2d667-421">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-421">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-422">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-422">Address:</span></span> <br><span data-ttu-id="2d667-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-423">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-424">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-425">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2d667-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-426">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-427">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="2d667-427">File system</span></span></td>
      <td><span data-ttu-id="2d667-428">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-428">Table</span></span></td>
      <td><span data-ttu-id="2d667-429">Fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-429">File</span></span></td>
      <td><span data-ttu-id="2d667-430">
        <font size=2>Protokoll: fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-430">
        <font size=2> Protocol: file</span></span> <br><span data-ttu-id="2d667-431">Hitelesítési: {none, basic, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-431">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="2d667-432">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-432">Address:</span></span> <br><span data-ttu-id="2d667-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;elérési út</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-433">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-434">FTP</span><span class="sxs-lookup"><span data-stu-id="2d667-434">FTP</span></span></td>
      <td><span data-ttu-id="2d667-435">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-435">Table</span></span></td>
      <td><span data-ttu-id="2d667-436">Könyvtár, fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-436">Directory, file</span></span></td>
      <td><span data-ttu-id="2d667-437">
        <font size=2>Protokoll: ftp</span><span class="sxs-lookup"><span data-stu-id="2d667-437">
        <font size=2> Protocol: ftp</span></span> <br><span data-ttu-id="2d667-438">Hitelesítési: {none, basic, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-438">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="2d667-439">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-439">Address:</span></span> <br><span data-ttu-id="2d667-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-440">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-441">Hadoop elosztott fájlrendszerhez</span><span class="sxs-lookup"><span data-stu-id="2d667-441">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="2d667-442">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-442">Container</span></span></td>
      <td><span data-ttu-id="2d667-443">Fürt</span><span class="sxs-lookup"><span data-stu-id="2d667-443">Cluster</span></span></td>
      <td><span data-ttu-id="2d667-444">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="2d667-444">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="2d667-445">Hitelesítési: {alapvető, oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-445">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="2d667-446">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-446">Address:</span></span> <br><span data-ttu-id="2d667-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-447">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-448">Hadoop elosztott fájlrendszerhez</span><span class="sxs-lookup"><span data-stu-id="2d667-448">Hadoop Distributed File System</span></span></td>
      <td><span data-ttu-id="2d667-449">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-449">Table</span></span></td>
      <td><span data-ttu-id="2d667-450">Könyvtár, fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-450">Directory, file</span></span></td>
      <td><span data-ttu-id="2d667-451">
        <font size=2>Protokoll: webhdfs</span><span class="sxs-lookup"><span data-stu-id="2d667-451">
        <font size=2> Protocol: webhdfs</span></span> <br><span data-ttu-id="2d667-452">Hitelesítési: {alapvető, oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-452">Authentication: {basic, oauth}</span></span> <br><span data-ttu-id="2d667-453">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-453">Address:</span></span> <br><span data-ttu-id="2d667-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-454">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-455">Hive</span><span class="sxs-lookup"><span data-stu-id="2d667-455">Hive</span></span></td>
      <td><span data-ttu-id="2d667-456">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-456">Container</span></span></td>
      <td><span data-ttu-id="2d667-457">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-457">Database</span></span></td>
      <td><span data-ttu-id="2d667-458">
        <font size=2>Protokoll: struktúra</span><span class="sxs-lookup"><span data-stu-id="2d667-458">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="2d667-459">Hitelesítési: {HDInsight, basic, a felhasználónevet, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-459">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="2d667-460">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-460">Address:</span></span> <br><span data-ttu-id="2d667-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-461">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-462">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-463">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="2d667-463">connectionProperties:</span></span> <br><span data-ttu-id="2d667-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-464">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-465">Hive</span><span class="sxs-lookup"><span data-stu-id="2d667-465">Hive</span></span></td>
      <td><span data-ttu-id="2d667-466">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-466">Table</span></span></td>
      <td><span data-ttu-id="2d667-467">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-467">Table, view</span></span></td>
      <td><span data-ttu-id="2d667-468">
        <font size=2>Protokoll: struktúra</span><span class="sxs-lookup"><span data-stu-id="2d667-468">
        <font size=2> Protocol: hive</span></span> <br><span data-ttu-id="2d667-469">Hitelesítési: {HDInsight, basic, a felhasználónevet, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-469">Authentication: {HDInsight, basic, username, none}</span></span> <br><span data-ttu-id="2d667-470">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-470">Address:</span></span> <br><span data-ttu-id="2d667-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-471">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-472">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-473">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2d667-474">connectionProperties:</span><span class="sxs-lookup"><span data-stu-id="2d667-474">connectionProperties:</span></span> <br><span data-ttu-id="2d667-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-475">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-476">HTTP</span><span class="sxs-lookup"><span data-stu-id="2d667-476">HTTP</span></span></td>
      <td><span data-ttu-id="2d667-477">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-477">Container</span></span></td>
      <td><span data-ttu-id="2d667-478">Helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-478">Site</span></span></td>
      <td><span data-ttu-id="2d667-479">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="2d667-479">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2d667-480">Hitelesítési: {none, basic, a windows, a oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-480">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2d667-481">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-481">Address:</span></span> <br><span data-ttu-id="2d667-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-482">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-483">HTTP</span><span class="sxs-lookup"><span data-stu-id="2d667-483">HTTP</span></span></td>
      <td><span data-ttu-id="2d667-484">Jelentés</span><span class="sxs-lookup"><span data-stu-id="2d667-484">Report</span></span></td>
      <td><span data-ttu-id="2d667-485">A jelentés, az irányítópult</span><span class="sxs-lookup"><span data-stu-id="2d667-485">Report, dashboard</span></span></td>
      <td><span data-ttu-id="2d667-486">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="2d667-486">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2d667-487">Hitelesítési: {none, basic, a windows, a oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-487">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2d667-488">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-488">Address:</span></span> <br><span data-ttu-id="2d667-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-489">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-490">HTTP</span><span class="sxs-lookup"><span data-stu-id="2d667-490">HTTP</span></span></td>
      <td><span data-ttu-id="2d667-491">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-491">Table</span></span></td>
      <td><span data-ttu-id="2d667-492">Végpont, fájl</span><span class="sxs-lookup"><span data-stu-id="2d667-492">Endpoint, file</span></span></td>
      <td><span data-ttu-id="2d667-493">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="2d667-493">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2d667-494">Hitelesítési: {none, basic, a windows, a oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-494">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2d667-495">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-495">Address:</span></span> <br><span data-ttu-id="2d667-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-496">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-497">MySQL</span><span class="sxs-lookup"><span data-stu-id="2d667-497">MySQL</span></span></td>
      <td><span data-ttu-id="2d667-498">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-498">Container</span></span></td>
      <td><span data-ttu-id="2d667-499">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-499">Database</span></span></td>
      <td><span data-ttu-id="2d667-500">
        <font size=2>Protokoll: mysql</span><span class="sxs-lookup"><span data-stu-id="2d667-500">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="2d667-501">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-501">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-502">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-502">Address:</span></span> <br><span data-ttu-id="2d667-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-503">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-504">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-505">MySQL</span><span class="sxs-lookup"><span data-stu-id="2d667-505">MySQL</span></span></td>
      <td><span data-ttu-id="2d667-506">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-506">Table</span></span></td>
      <td><span data-ttu-id="2d667-507">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-507">Table, view</span></span></td>
      <td><span data-ttu-id="2d667-508">
        <font size=2>Protokoll: mysql</span><span class="sxs-lookup"><span data-stu-id="2d667-508">
        <font size=2> Protocol: mysql</span></span> <br><span data-ttu-id="2d667-509">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-509">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-510">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-510">Address:</span></span> <br><span data-ttu-id="2d667-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-511">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-512">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-513">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-514">OData</span><span class="sxs-lookup"><span data-stu-id="2d667-514">OData</span></span></td>
      <td><span data-ttu-id="2d667-515">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-515">Container</span></span></td>
      <td><span data-ttu-id="2d667-516">Entitás tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-516">Entity container</span></span></td>
      <td><span data-ttu-id="2d667-517">
        <font size=2>Protokoll: odata</span><span class="sxs-lookup"><span data-stu-id="2d667-517">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="2d667-518">Hitelesítési: {none, basic, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-518">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="2d667-519">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-519">Address:</span></span> <br><span data-ttu-id="2d667-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-520">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-521">OData</span><span class="sxs-lookup"><span data-stu-id="2d667-521">OData</span></span></td>
      <td><span data-ttu-id="2d667-522">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-522">Table</span></span></td>
      <td><span data-ttu-id="2d667-523">Entitáskészlet, a függvény</span><span class="sxs-lookup"><span data-stu-id="2d667-523">Entity set, function</span></span></td>
      <td><span data-ttu-id="2d667-524">
        <font size=2>Protokoll: odata</span><span class="sxs-lookup"><span data-stu-id="2d667-524">
        <font size=2> Protocol: odata</span></span> <br><span data-ttu-id="2d667-525">Hitelesítési: {none, basic, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-525">Authentication: {none, basic, windows}</span></span> <br><span data-ttu-id="2d667-526">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-526">Address:</span></span> <br><span data-ttu-id="2d667-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</span><span class="sxs-lookup"><span data-stu-id="2d667-527">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2d667-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;erőforrás</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-528">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; resource </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-529">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="2d667-529">Oracle Database</span></span></td>
      <td><span data-ttu-id="2d667-530">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-530">Container</span></span></td>
      <td><span data-ttu-id="2d667-531">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-531">Database</span></span></td>
      <td><span data-ttu-id="2d667-532">
        <font size=2>Protokoll: oracle</span><span class="sxs-lookup"><span data-stu-id="2d667-532">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="2d667-533">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-533">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-534">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-534">Address:</span></span> <br><span data-ttu-id="2d667-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-535">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-536">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-537">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="2d667-537">Oracle Database</span></span></td>
      <td><span data-ttu-id="2d667-538">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-538">Table</span></span></td>
      <td><span data-ttu-id="2d667-539">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-539">Table, view</span></span></td>
      <td><span data-ttu-id="2d667-540">
        <font size=2>Protokoll: oracle</span><span class="sxs-lookup"><span data-stu-id="2d667-540">
        <font size=2> Protocol: oracle</span></span> <br><span data-ttu-id="2d667-541">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-541">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-542">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-542">Address:</span></span> <br><span data-ttu-id="2d667-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-543">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-544">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-545">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-546">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-547">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="2d667-547">PostgreSQL</span></span></td>
      <td><span data-ttu-id="2d667-548">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-548">Container</span></span></td>
      <td><span data-ttu-id="2d667-549">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-549">Database</span></span></td>
      <td><span data-ttu-id="2d667-550">
        <font size=2>Protokoll: postgresql</span><span class="sxs-lookup"><span data-stu-id="2d667-550">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="2d667-551">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-551">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-552">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-552">Address:</span></span> <br><span data-ttu-id="2d667-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-553">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-554">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-555">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="2d667-555">PostgreSQL</span></span></td>
      <td><span data-ttu-id="2d667-556">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-556">Table</span></span></td>
      <td><span data-ttu-id="2d667-557">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-557">Table, view</span></span></td>
      <td><span data-ttu-id="2d667-558">
        <font size=2>Protokoll: postgresql</span><span class="sxs-lookup"><span data-stu-id="2d667-558">
        <font size=2> Protocol: postgresql</span></span> <br><span data-ttu-id="2d667-559">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-559">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-560">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-560">Address:</span></span> <br><span data-ttu-id="2d667-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-561">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-562">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-563">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-564">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-565">Power BI</span><span class="sxs-lookup"><span data-stu-id="2d667-565">Power BI</span></span></td>
      <td><span data-ttu-id="2d667-566">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-566">Container</span></span></td>
      <td><span data-ttu-id="2d667-567">Helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-567">Site</span></span></td>
      <td><span data-ttu-id="2d667-568">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="2d667-568">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2d667-569">Hitelesítési: {none, basic, a windows, a oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-569">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2d667-570">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-570">Address:</span></span> <br><span data-ttu-id="2d667-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-571">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-572">Power BI</span><span class="sxs-lookup"><span data-stu-id="2d667-572">Power BI</span></span></td>
      <td><span data-ttu-id="2d667-573">Jelentés</span><span class="sxs-lookup"><span data-stu-id="2d667-573">Report</span></span></td>
      <td><span data-ttu-id="2d667-574">A jelentés, az irányítópult</span><span class="sxs-lookup"><span data-stu-id="2d667-574">Report, dashboard</span></span></td>
      <td><span data-ttu-id="2d667-575">
        <font size=2>Protokoll: http</span><span class="sxs-lookup"><span data-stu-id="2d667-575">
        <font size=2> Protocol: http</span></span> <br><span data-ttu-id="2d667-576">Hitelesítési: {none, basic, a windows, a oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-576">Authentication: {none, basic, windows, oauth}</span></span> <br><span data-ttu-id="2d667-577">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-577">Address:</span></span> <br><span data-ttu-id="2d667-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-578">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-579">A Power Query</span><span class="sxs-lookup"><span data-stu-id="2d667-579">Power Query</span></span></td>
      <td><span data-ttu-id="2d667-580">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-580">Table</span></span></td>
      <td><span data-ttu-id="2d667-581">Adatok adategyesítési</span><span class="sxs-lookup"><span data-stu-id="2d667-581">Data mashup</span></span></td>
      <td><span data-ttu-id="2d667-582">
        <font size=2>Protokoll: power-lekérdezés</span><span class="sxs-lookup"><span data-stu-id="2d667-582">
        <font size=2> Protocol: power-query</span></span> <br><span data-ttu-id="2d667-583">Hitelesítési: {oauth}</span><span class="sxs-lookup"><span data-stu-id="2d667-583">Authentication: {oauth}</span></span> <br><span data-ttu-id="2d667-584">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-584">Address:</span></span> <br><span data-ttu-id="2d667-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-585">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-586">Salesforce</span><span class="sxs-lookup"><span data-stu-id="2d667-586">Salesforce</span></span></td>
      <td><span data-ttu-id="2d667-587">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-587">Table</span></span></td>
      <td><span data-ttu-id="2d667-588">Objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-588">Object</span></span></td>
      <td><span data-ttu-id="2d667-589">
        <font size=2>Protokoll: salesforce-com</span><span class="sxs-lookup"><span data-stu-id="2d667-589">
        <font size=2> Protocol: salesforce-com</span></span> <br><span data-ttu-id="2d667-590">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-590">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-591">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-591">Address:</span></span> <br><span data-ttu-id="2d667-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer</span><span class="sxs-lookup"><span data-stu-id="2d667-592">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; loginServer</span></span> <br><span data-ttu-id="2d667-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;osztály</span><span class="sxs-lookup"><span data-stu-id="2d667-593">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class</span></span> <br><span data-ttu-id="2d667-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemName</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-594">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itemName </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-595">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="2d667-595">SAP HANA</span></span></td>
      <td><span data-ttu-id="2d667-596">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-596">Container</span></span></td>
      <td><span data-ttu-id="2d667-597">Kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-597">Server</span></span></td>
      <td><span data-ttu-id="2d667-598">
        <font size=2>Protokoll: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="2d667-598">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="2d667-599">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-599">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-600">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-600">Address:</span></span> <br><span data-ttu-id="2d667-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-601">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-602">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="2d667-602">SAP HANA</span></span></td>
      <td><span data-ttu-id="2d667-603">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-603">Table</span></span></td>
      <td><span data-ttu-id="2d667-604">Nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-604">View</span></span></td>
      <td><span data-ttu-id="2d667-605">
        <font size=2>Protokoll: sap hana-sql</span><span class="sxs-lookup"><span data-stu-id="2d667-605">
        <font size=2> Protocol: sap-hana-sql</span></span> <br><span data-ttu-id="2d667-606">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-606">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-607">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-607">Address:</span></span> <br><span data-ttu-id="2d667-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-608">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-609">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-610">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-611">SharePoint</span><span class="sxs-lookup"><span data-stu-id="2d667-611">SharePoint</span></span></td>
      <td><span data-ttu-id="2d667-612">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-612">Table</span></span></td>
      <td><span data-ttu-id="2d667-613">Lista</span><span class="sxs-lookup"><span data-stu-id="2d667-613">List</span></span></td>
      <td><span data-ttu-id="2d667-614">
        <font size=2>Protokoll: sharepoint-lista</span><span class="sxs-lookup"><span data-stu-id="2d667-614">
        <font size=2> Protocol: sharepoint-list</span></span> <br><span data-ttu-id="2d667-615">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-615">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-616">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-616">Address:</span></span> <br><span data-ttu-id="2d667-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-617">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-618">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2d667-618">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="2d667-619">Parancs</span><span class="sxs-lookup"><span data-stu-id="2d667-619">Command</span></span></td>
      <td><span data-ttu-id="2d667-620">Tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="2d667-620">Stored procedure</span></span></td>
      <td><span data-ttu-id="2d667-621">
        <font size=2>Protokoll: TDS-adatfolyam</span><span class="sxs-lookup"><span data-stu-id="2d667-621">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2d667-622">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-622">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-623">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-623">Address:</span></span> <br><span data-ttu-id="2d667-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-624">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-625">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-626">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-627">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-628">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2d667-628">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="2d667-629">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="2d667-629">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="2d667-630">Táblázat értékű függvényben</span><span class="sxs-lookup"><span data-stu-id="2d667-630">Table-valued function</span></span></td>
      <td><span data-ttu-id="2d667-631">
        <font size=2>Protokoll: TDS-adatfolyam</span><span class="sxs-lookup"><span data-stu-id="2d667-631">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2d667-632">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-632">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-633">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-633">Address:</span></span> <br><span data-ttu-id="2d667-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-634">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-635">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-636">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-637">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-638">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2d667-638">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="2d667-639">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-639">Container</span></span></td>
      <td><span data-ttu-id="2d667-640">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-640">Database</span></span></td>
      <td><span data-ttu-id="2d667-641">
        <font size=2>Protokoll: TDS-adatfolyam</span><span class="sxs-lookup"><span data-stu-id="2d667-641">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2d667-642">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-642">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-643">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-643">Address:</span></span> <br><span data-ttu-id="2d667-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-644">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-645">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-646">SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2d667-646">SQL Data Warehouse</span></span></td>
      <td><span data-ttu-id="2d667-647">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-647">Table</span></span></td>
      <td><span data-ttu-id="2d667-648">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-648">Table, view</span></span></td>
      <td><span data-ttu-id="2d667-649">
        <font size=2>Protokoll: TDS-adatfolyam</span><span class="sxs-lookup"><span data-stu-id="2d667-649">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2d667-650">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-650">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-651">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-651">Address:</span></span> <br><span data-ttu-id="2d667-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-652">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-653">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-654">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-655">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-656">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2d667-656">SQL Server</span></span></td>
      <td><span data-ttu-id="2d667-657">Parancs</span><span class="sxs-lookup"><span data-stu-id="2d667-657">Command</span></span></td>
      <td><span data-ttu-id="2d667-658">Tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="2d667-658">Stored procedure</span></span></td>
      <td><span data-ttu-id="2d667-659">
        <font size=2>Protokoll: TDS-adatfolyam</span><span class="sxs-lookup"><span data-stu-id="2d667-659">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2d667-660">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-660">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-661">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-661">Address:</span></span> <br><span data-ttu-id="2d667-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-662">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-663">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-664">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-665">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-666">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2d667-666">SQL Server</span></span></td>
      <td><span data-ttu-id="2d667-667">TableValuedFunction</span><span class="sxs-lookup"><span data-stu-id="2d667-667">TableValuedFunction</span></span></td>
      <td><span data-ttu-id="2d667-668">Táblázat értékű függvényben</span><span class="sxs-lookup"><span data-stu-id="2d667-668">Table-valued function</span></span></td>
      <td><span data-ttu-id="2d667-669">
        <font size=2>Protokoll: TDS-adatfolyam</span><span class="sxs-lookup"><span data-stu-id="2d667-669">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2d667-670">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-670">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-671">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-671">Address:</span></span> <br><span data-ttu-id="2d667-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-672">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-673">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-674">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-675">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-676">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2d667-676">SQL Server</span></span></td>
      <td><span data-ttu-id="2d667-677">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-677">Container</span></span></td>
      <td><span data-ttu-id="2d667-678">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-678">Database</span></span></td>
      <td><span data-ttu-id="2d667-679">
        <font size=2>Protokoll: TDS-adatfolyam</span><span class="sxs-lookup"><span data-stu-id="2d667-679">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2d667-680">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-680">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-681">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-681">Address:</span></span> <br><span data-ttu-id="2d667-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-682">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-683">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-684">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2d667-684">SQL Server</span></span></td>
      <td><span data-ttu-id="2d667-685">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-685">Table</span></span></td>
      <td><span data-ttu-id="2d667-686">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-686">Table, view</span></span></td>
      <td><span data-ttu-id="2d667-687">
        <font size=2>Protokoll: TDS-adatfolyam</span><span class="sxs-lookup"><span data-stu-id="2d667-687">
        <font size=2> Protocol: tds</span></span> <br><span data-ttu-id="2d667-688">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-688">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-689">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-689">Address:</span></span> <br><span data-ttu-id="2d667-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-690">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-691">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-692">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-693">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-694">SQL Server Analysis Services többdimenziós</span><span class="sxs-lookup"><span data-stu-id="2d667-694">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="2d667-695">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-695">Container</span></span></td>
      <td><span data-ttu-id="2d667-696">Modell</span><span class="sxs-lookup"><span data-stu-id="2d667-696">Model</span></span></td>
      <td><span data-ttu-id="2d667-697">
        <font size=2>Protokoll: analysis-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-697">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2d667-698">Hitelesítési: {windows, a basic, a névtelen, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-698">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2d667-699">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-699">Address:</span></span> <br><span data-ttu-id="2d667-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-700">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-701">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-702">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-703">SQL Server Analysis Services többdimenziós</span><span class="sxs-lookup"><span data-stu-id="2d667-703">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="2d667-704">KPI-T</span><span class="sxs-lookup"><span data-stu-id="2d667-704">KPI</span></span></td>
      <td><span data-ttu-id="2d667-705">KPI-T</span><span class="sxs-lookup"><span data-stu-id="2d667-705">KPI</span></span></td>
      <td><span data-ttu-id="2d667-706">
        <font size=2>Protokoll: analysis-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-706">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2d667-707">Hitelesítési: {windows, a basic, a névtelen, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-707">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2d667-708">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-708">Address:</span></span> <br><span data-ttu-id="2d667-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-709">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-710">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</span><span class="sxs-lookup"><span data-stu-id="2d667-711">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2d667-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-712">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2d667-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektumtípus: {KPI}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-713">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-714">SQL Server Analysis Services többdimenziós</span><span class="sxs-lookup"><span data-stu-id="2d667-714">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="2d667-715">mértékcsoport</span><span class="sxs-lookup"><span data-stu-id="2d667-715">Measure</span></span></td>
      <td><span data-ttu-id="2d667-716">mértékcsoport</span><span class="sxs-lookup"><span data-stu-id="2d667-716">Measure</span></span></td>
      <td><span data-ttu-id="2d667-717">
        <font size=2>Protokoll: analysis-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-717">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2d667-718">Hitelesítési: {windows, a basic, a névtelen, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-718">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2d667-719">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-719">Address:</span></span> <br><span data-ttu-id="2d667-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-720">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-721">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</span><span class="sxs-lookup"><span data-stu-id="2d667-722">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2d667-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-723">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2d667-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektumtípus: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-724">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-725">SQL Server Analysis Services többdimenziós</span><span class="sxs-lookup"><span data-stu-id="2d667-725">SQL Server Analysis Services multidimensional</span></span></td>
      <td><span data-ttu-id="2d667-726">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-726">Table</span></span></td>
      <td><span data-ttu-id="2d667-727">Dimenzió</span><span class="sxs-lookup"><span data-stu-id="2d667-727">Dimension</span></span></td>
      <td><span data-ttu-id="2d667-728">
        <font size=2>Protokoll: analysis-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-728">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2d667-729">Hitelesítési: {windows, a basic, a névtelen, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-729">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2d667-730">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-730">Address:</span></span> <br><span data-ttu-id="2d667-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-731">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-732">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</span><span class="sxs-lookup"><span data-stu-id="2d667-733">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2d667-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-734">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2d667-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektumtípus: {dimenzió}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-735">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Dimension} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-736">SQL Server Analysis Services táblázatos</span><span class="sxs-lookup"><span data-stu-id="2d667-736">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="2d667-737">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-737">Container</span></span></td>
      <td><span data-ttu-id="2d667-738">Modell</span><span class="sxs-lookup"><span data-stu-id="2d667-738">Model</span></span></td>
      <td><span data-ttu-id="2d667-739">
        <font size=2>Protokoll: analysis-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-739">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2d667-740">Hitelesítési: {windows, a basic, a névtelen, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-740">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2d667-741">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-741">Address:</span></span> <br><span data-ttu-id="2d667-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-742">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-743">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-744">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-745">SQL Server Analysis Services táblázatos</span><span class="sxs-lookup"><span data-stu-id="2d667-745">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="2d667-746">KPI-T</span><span class="sxs-lookup"><span data-stu-id="2d667-746">KPI</span></span></td>
      <td><span data-ttu-id="2d667-747">KPI-T</span><span class="sxs-lookup"><span data-stu-id="2d667-747">KPI</span></span></td>
      <td><span data-ttu-id="2d667-748">
        <font size=2>Protokoll: analysis-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-748">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2d667-749">Hitelesítési: {windows, a basic, a névtelen, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-749">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2d667-750">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-750">Address:</span></span> <br><span data-ttu-id="2d667-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-751">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-752">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</span><span class="sxs-lookup"><span data-stu-id="2d667-753">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2d667-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-754">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2d667-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektumtípus: {KPI}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-755">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-756">SQL Server Analysis Services táblázatos</span><span class="sxs-lookup"><span data-stu-id="2d667-756">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="2d667-757">mértékcsoport</span><span class="sxs-lookup"><span data-stu-id="2d667-757">Measure</span></span></td>
      <td><span data-ttu-id="2d667-758">mértékcsoport</span><span class="sxs-lookup"><span data-stu-id="2d667-758">Measure</span></span></td>
      <td><span data-ttu-id="2d667-759">
        <font size=2>Protokoll: analysis-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-759">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2d667-760">Hitelesítési: {windows, a basic, a névtelen, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-760">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2d667-761">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-761">Address:</span></span> <br><span data-ttu-id="2d667-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-762">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-763">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</span><span class="sxs-lookup"><span data-stu-id="2d667-764">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2d667-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-765">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2d667-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektumtípus: {Measure}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-766">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-767">SQL Server Analysis Services táblázatos</span><span class="sxs-lookup"><span data-stu-id="2d667-767">SQL Server Analysis Services tabular</span></span></td>
      <td><span data-ttu-id="2d667-768">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-768">Table</span></span></td>
      <td><span data-ttu-id="2d667-769">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-769">Table</span></span></td>
      <td><span data-ttu-id="2d667-770">
        <font size=2>Protokoll: analysis-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-770">
        <font size=2> Protocol: analysis-services</span></span> <br><span data-ttu-id="2d667-771">Hitelesítési: {windows, a basic, a névtelen, nincs}</span><span class="sxs-lookup"><span data-stu-id="2d667-771">Authentication: {windows, basic, anonymous, none}</span></span> <br><span data-ttu-id="2d667-772">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-772">Address:</span></span> <br><span data-ttu-id="2d667-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-773">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-774">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</span><span class="sxs-lookup"><span data-stu-id="2d667-775">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2d667-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-776">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2d667-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektumtípus: {TABLE táblázat}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-777">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Table} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-778">SQL Server Reporting Services szoftverben</span><span class="sxs-lookup"><span data-stu-id="2d667-778">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="2d667-779">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-779">Container</span></span></td>
      <td><span data-ttu-id="2d667-780">Kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-780">Server</span></span></td>
      <td><span data-ttu-id="2d667-781">
        <font size=2>Protokoll: jelentési szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-781">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="2d667-782">Hitelesítési: {windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-782">Authentication: {windows}</span></span> <br><span data-ttu-id="2d667-783">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-783">Address:</span></span> <br><span data-ttu-id="2d667-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-784">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verzió: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-785">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-786">SQL Server Reporting Services szoftverben</span><span class="sxs-lookup"><span data-stu-id="2d667-786">SQL Server Reporting Services</span></span></td>
      <td><span data-ttu-id="2d667-787">Jelentés</span><span class="sxs-lookup"><span data-stu-id="2d667-787">Report</span></span></td>
      <td><span data-ttu-id="2d667-788">Jelentés</span><span class="sxs-lookup"><span data-stu-id="2d667-788">Report</span></span></td>
      <td><span data-ttu-id="2d667-789">
        <font size=2>Protokoll: jelentési szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2d667-789">
        <font size=2> Protocol: reporting-services</span></span> <br><span data-ttu-id="2d667-790">Hitelesítési: {windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-790">Authentication: {windows}</span></span> <br><span data-ttu-id="2d667-791">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-791">Address:</span></span> <br><span data-ttu-id="2d667-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-792">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;elérési út</span><span class="sxs-lookup"><span data-stu-id="2d667-793">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path</span></span> <br><span data-ttu-id="2d667-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verzió: {ReportingService2010}</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-794">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-795">Teradata</span><span class="sxs-lookup"><span data-stu-id="2d667-795">Teradata</span></span></td>
      <td><span data-ttu-id="2d667-796">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-796">Container</span></span></td>
      <td><span data-ttu-id="2d667-797">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-797">Database</span></span></td>
      <td><span data-ttu-id="2d667-798">
        <font size=2>Protokoll: teradata rendszerhez</span><span class="sxs-lookup"><span data-stu-id="2d667-798">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="2d667-799">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-799">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-800">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-800">Address:</span></span> <br><span data-ttu-id="2d667-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-801">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-802">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-803">Teradata</span><span class="sxs-lookup"><span data-stu-id="2d667-803">Teradata</span></span></td>
      <td><span data-ttu-id="2d667-804">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-804">Table</span></span></td>
      <td><span data-ttu-id="2d667-805">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-805">Table, view</span></span></td>
      <td><span data-ttu-id="2d667-806">
        <font size=2>Protokoll: teradata rendszerhez</span><span class="sxs-lookup"><span data-stu-id="2d667-806">
        <font size=2> Protocol: teradata</span></span> <br><span data-ttu-id="2d667-807">Hitelesítési: {protokollt, a windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-807">Authentication: {protocol, windows}</span></span> <br><span data-ttu-id="2d667-808">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-808">Address:</span></span> <br><span data-ttu-id="2d667-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-809">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-810">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-811">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-812">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="2d667-812">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="2d667-813">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-813">Container</span></span></td>
      <td><span data-ttu-id="2d667-814">Modell</span><span class="sxs-lookup"><span data-stu-id="2d667-814">Model</span></span></td>
      <td><span data-ttu-id="2d667-815">
        <font size="2">Protokoll: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="2d667-815">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="2d667-816">Hitelesítési: {windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-816">Authentication: {windows}</span></span> <br><span data-ttu-id="2d667-817">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-817">Address:</span></span> <br><span data-ttu-id="2d667-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</span><span class="sxs-lookup"><span data-stu-id="2d667-818">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2d667-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</span><span class="sxs-lookup"><span data-stu-id="2d667-819">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2d667-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verzió</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-820">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-821">SQL Server Master Data Services</span><span class="sxs-lookup"><span data-stu-id="2d667-821">SQL Server Master Data Services</span></span></td>
      <td><span data-ttu-id="2d667-822">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-822">Table</span></span></td>
      <td><span data-ttu-id="2d667-823">Entitás</span><span class="sxs-lookup"><span data-stu-id="2d667-823">Entity</span></span></td>
      <td><span data-ttu-id="2d667-824">
        <font size="2">Protokoll: mssql-mds</span><span class="sxs-lookup"><span data-stu-id="2d667-824">
        <font size="2"> Protocol: mssql-mds</span></span> <br><span data-ttu-id="2d667-825">Hitelesítési: {windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-825">Authentication: {windows}</span></span> <br><span data-ttu-id="2d667-826">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-826">Address:</span></span> <br><span data-ttu-id="2d667-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</span><span class="sxs-lookup"><span data-stu-id="2d667-827">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2d667-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modell</span><span class="sxs-lookup"><span data-stu-id="2d667-828">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model</span></span> <br><span data-ttu-id="2d667-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verzió</span><span class="sxs-lookup"><span data-stu-id="2d667-829">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version</span></span> <br><span data-ttu-id="2d667-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entitás</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-830">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; entity </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-831">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2d667-831">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="2d667-832">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-832">Container</span></span></td>
      <td><span data-ttu-id="2d667-833">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-833">Database</span></span></td>
      <td><span data-ttu-id="2d667-834">
        <font size=2>Protokoll: dokumentum-adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-834">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="2d667-835">Hitelesítési: {azure-access-kulcs}</span><span class="sxs-lookup"><span data-stu-id="2d667-835">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2d667-836">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-836">Address:</span></span> <br><span data-ttu-id="2d667-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</span><span class="sxs-lookup"><span data-stu-id="2d667-837">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2d667-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-838">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-839">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2d667-839">Azure Cosmos DB</span></span></td>
      <td><span data-ttu-id="2d667-840">Gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="2d667-840">Collection</span></span></td>
      <td><span data-ttu-id="2d667-841">Gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="2d667-841">Collection</span></span></td>
      <td><span data-ttu-id="2d667-842">
        <font size=2>Protokoll: dokumentum-adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-842">
        <font size=2> Protocol: document-db</span></span> <br><span data-ttu-id="2d667-843">Hitelesítési: {azure-access-kulcs}</span><span class="sxs-lookup"><span data-stu-id="2d667-843">Authentication: {azure-access-key}</span></span> <br><span data-ttu-id="2d667-844">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-844">Address:</span></span> <br><span data-ttu-id="2d667-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-címe</span><span class="sxs-lookup"><span data-stu-id="2d667-845">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url</span></span> <br><span data-ttu-id="2d667-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-846">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;gyűjtemény</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-847">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; collection </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-848">Általános ODBC</span><span class="sxs-lookup"><span data-stu-id="2d667-848">Generic ODBC</span></span></td>
      <td><span data-ttu-id="2d667-849">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-849">Container</span></span></td>
      <td><span data-ttu-id="2d667-850">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-850">Database</span></span></td>
      <td><span data-ttu-id="2d667-851">
        <font size=2>Protokoll: odbc</span><span class="sxs-lookup"><span data-stu-id="2d667-851">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="2d667-852">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-852">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-853">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-853">Address:</span></span> <br><span data-ttu-id="2d667-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;beállítások</span><span class="sxs-lookup"><span data-stu-id="2d667-854">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="2d667-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-855">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-856">Általános ODBC</span><span class="sxs-lookup"><span data-stu-id="2d667-856">Generic ODBC</span></span></td>
      <td><span data-ttu-id="2d667-857">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-857">Table</span></span></td>
      <td><span data-ttu-id="2d667-858">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-858">Table, View</span></span></td>
      <td><span data-ttu-id="2d667-859">
        <font size=2>Protokoll: odbc</span><span class="sxs-lookup"><span data-stu-id="2d667-859">
        <font size=2> Protocol: odbc</span></span> <br><span data-ttu-id="2d667-860">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-860">Authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-861">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-861">Address:</span></span> <br><span data-ttu-id="2d667-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;beállítások</span><span class="sxs-lookup"><span data-stu-id="2d667-862">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options</span></span> <br><span data-ttu-id="2d667-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-863">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</span><span class="sxs-lookup"><span data-stu-id="2d667-864">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object</span></span> <br><span data-ttu-id="2d667-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-865">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-866">Sybase</span><span class="sxs-lookup"><span data-stu-id="2d667-866">Sybase</span></span></td>
      <td><span data-ttu-id="2d667-867">Tároló</span><span class="sxs-lookup"><span data-stu-id="2d667-867">Container</span></span></td>
      <td><span data-ttu-id="2d667-868">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-868">Database</span></span></td>
      <td><span data-ttu-id="2d667-869">
        <font size=2>protokoll: sybase</span><span class="sxs-lookup"><span data-stu-id="2d667-869">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="2d667-870">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-870">authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-871">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-871">address:</span></span> <br><span data-ttu-id="2d667-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-872">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-873">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-874">Sybase</span><span class="sxs-lookup"><span data-stu-id="2d667-874">Sybase</span></span></td>
      <td><span data-ttu-id="2d667-875">Tábla</span><span class="sxs-lookup"><span data-stu-id="2d667-875">Table</span></span></td>
      <td><span data-ttu-id="2d667-876">A tábla, nézet</span><span class="sxs-lookup"><span data-stu-id="2d667-876">Table, View</span></span></td>
      <td><span data-ttu-id="2d667-877">
        <font size=2>protokoll: sybase</span><span class="sxs-lookup"><span data-stu-id="2d667-877">
        <font size=2> protocol: sybase</span></span> <br><span data-ttu-id="2d667-878">Hitelesítési: {alapvető, windows}</span><span class="sxs-lookup"><span data-stu-id="2d667-878">authentication: {basic, windows}</span></span> <br><span data-ttu-id="2d667-879">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-879">address:</span></span> <br><span data-ttu-id="2d667-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2d667-880">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server</span></span> <br><span data-ttu-id="2d667-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d667-881">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database</span></span> <br><span data-ttu-id="2d667-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;séma</span><span class="sxs-lookup"><span data-stu-id="2d667-882">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema</span></span> <br><span data-ttu-id="2d667-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektum</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-883">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </span></span></td>
    </tr>
    <tr>
      <td><span data-ttu-id="2d667-884">Más (egyik sem)</span><span class="sxs-lookup"><span data-stu-id="2d667-884">Other (none of the above)</span></span></td>
      <td>\*</td>
      <td>\*</td>
      <td><span data-ttu-id="2d667-885">
        <font size=2>Protokoll: általános-eszköz</span><span class="sxs-lookup"><span data-stu-id="2d667-885">
        <font size=2> Protocol: generic-asset</span></span> <br><span data-ttu-id="2d667-886">Cím:</span><span class="sxs-lookup"><span data-stu-id="2d667-886">Address:</span></span> <br><span data-ttu-id="2d667-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assetId</font>
      </span><span class="sxs-lookup"><span data-stu-id="2d667-887">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; assetId </font>
      </span></span></td>
    </tr>
</table>
