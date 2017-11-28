---
title: "Az Azure Storage eszközei |} Microsoft Docs"
description: "Eszközök, amelyek lehetővé teszik a nézet vagy használhatják az Azure Storage adatokat listáját."
services: storage
documentationcenter: 
author: dineshmurthy
manager: jahogg
editor: tysonn
ms.assetid: e4748642-98c4-437e-b0ed-4f9641c2e894
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: dineshmurthy
ms.openlocfilehash: 620efda06d8225b21b6bb9b104b79061ebb6515c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-client-tools"></a><span data-ttu-id="9f470-103">Azure Storage-ügyféleszközök</span><span class="sxs-lookup"><span data-stu-id="9f470-103">Azure Storage Client Tools</span></span>
<span data-ttu-id="9f470-104">Az Azure Storage felhasználók gyakran érdemes megtekintése/kezelhetik az adatokat egy Azure Storage ügyfél eszközzel.</span><span class="sxs-lookup"><span data-stu-id="9f470-104">Users of Azure Storage frequently want to be able to view/interact with their data using an Azure Storage client tool.</span></span> <span data-ttu-id="9f470-105">Az alábbi táblázatban látható eszközöket tartalmazza, amelyek lehetővé teszik a számnak.</span><span class="sxs-lookup"><span data-stu-id="9f470-105">In the tables below, we list a number of tools that allow you to do this.</span></span> <span data-ttu-id="9f470-106">A Microsoft helyezze el az "X" valamennyi blokkja Ha felsorolni és/vagy a hozzáférési adatok kivételére lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="9f470-106">We put an "X" in each block if it provides the ability to either enumerate and/or access the data abstraction.</span></span> <span data-ttu-id="9f470-107">A táblázatban is látható, ha az eszközök szabad-e vagy sem.</span><span class="sxs-lookup"><span data-stu-id="9f470-107">The table also shows if the tools is free or not.</span></span> <span data-ttu-id="9f470-108">"Próba" azt jelzi, hogy van egy ingyenes próbaverziót, de a teljes termék nincs szabad.</span><span class="sxs-lookup"><span data-stu-id="9f470-108">"Trial" indicates that there is a free trial, but the full product is not free.</span></span> <span data-ttu-id="9f470-109">"I/N" azt jelzi, hogy egy újabb verziója érhető szabad, miközben egy másik verzió.</span><span class="sxs-lookup"><span data-stu-id="9f470-109">"Y/N" indicates that a version is available for free, while a different version is available for purchase.</span></span>

<span data-ttu-id="9f470-110">Csak az elérhető Azure Storage-ügyféleszközöket pillanatképe mellékelt.</span><span class="sxs-lookup"><span data-stu-id="9f470-110">We've only provided a snapshot of the available Azure Storage client tools.</span></span> <span data-ttu-id="9f470-111">Ezek az eszközök továbbra is fejlődnek, és funkcióit nő.</span><span class="sxs-lookup"><span data-stu-id="9f470-111">These tools may continue to evolve and grow in functionality.</span></span> <span data-ttu-id="9f470-112">Ha anyagban javították vagy a frissítéseket, írt hozzászólásban tudassa velünk, hogy.</span><span class="sxs-lookup"><span data-stu-id="9f470-112">If there are corrections or updates, please leave a comment to let us know.</span></span> <span data-ttu-id="9f470-113">Ugyanez érvényes Ha tudja, kell lennie a ide - eszközök örömmel adja hozzá lenne.</span><span class="sxs-lookup"><span data-stu-id="9f470-113">The same is true if you know of tools that ought to be here - we'd be happy to add them.</span></span>

<span data-ttu-id="9f470-114">**A Microsoft Azure Storage ügyféleszközök**</span><span class="sxs-lookup"><span data-stu-id="9f470-114">**Microsoft Azure Storage Client Tools**</span></span>

<table>
  <tr>
    <th rowspan="2"><span data-ttu-id="9f470-115">Az Azure Storage ügyfél eszköz</span><span class="sxs-lookup"><span data-stu-id="9f470-115">Azure Storage Client Tool</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-116">Blokkblob</span><span class="sxs-lookup"><span data-stu-id="9f470-116">Block Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-117">Az oldalakra vonatkozó Blob</span><span class="sxs-lookup"><span data-stu-id="9f470-117">Page Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-118">Hozzáfűző Blob</span><span class="sxs-lookup"><span data-stu-id="9f470-118">Append Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-119">Táblák</span><span class="sxs-lookup"><span data-stu-id="9f470-119">Tables</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-120">Üzenetsorok</span><span class="sxs-lookup"><span data-stu-id="9f470-120">Queues</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-121">Fájlok</span><span class="sxs-lookup"><span data-stu-id="9f470-121">Files</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-122">Ingyenes</span><span class="sxs-lookup"><span data-stu-id="9f470-122">Free</span></span></th>
    <th colspan="4"><span data-ttu-id="9f470-123">Platform</span><span class="sxs-lookup"><span data-stu-id="9f470-123">Platform</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-124">Web</span><span class="sxs-lookup"><span data-stu-id="9f470-124">Web</span></span></td>
    <td><span data-ttu-id="9f470-125">Windows</span><span class="sxs-lookup"><span data-stu-id="9f470-125">Windows</span></span></td>
    <td><span data-ttu-id="9f470-126">OSX</span><span class="sxs-lookup"><span data-stu-id="9f470-126">OSX</span></span></td>
    <td><span data-ttu-id="9f470-127">Linux</span><span class="sxs-lookup"><span data-stu-id="9f470-127">Linux</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-128"><a href="https://azure.microsoft.com/features/azure-portal/">Microsoft Azure-portálon</a></span><span class="sxs-lookup"><span data-stu-id="9f470-128"><a href="https://azure.microsoft.com/features/azure-portal/">Microsoft Azure Portal</a></span></span></td>
    <td><span data-ttu-id="9f470-129">X</span><span class="sxs-lookup"><span data-stu-id="9f470-129">X</span></span></td>
    <td><span data-ttu-id="9f470-130">X</span><span class="sxs-lookup"><span data-stu-id="9f470-130">X</span></span></td>
    <td><span data-ttu-id="9f470-131">X</span><span class="sxs-lookup"><span data-stu-id="9f470-131">X</span></span></td>
    <td><span data-ttu-id="9f470-132">X</span><span class="sxs-lookup"><span data-stu-id="9f470-132">X</span></span></td>
    <td><span data-ttu-id="9f470-133">X</span><span class="sxs-lookup"><span data-stu-id="9f470-133">X</span></span></td>
    <td><span data-ttu-id="9f470-134">X</span><span class="sxs-lookup"><span data-stu-id="9f470-134">X</span></span></td>
    <td><span data-ttu-id="9f470-135">I</span><span class="sxs-lookup"><span data-stu-id="9f470-135">Y</span></span></td>
    <td><span data-ttu-id="9f470-136">X</span><span class="sxs-lookup"><span data-stu-id="9f470-136">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-137"><a href="http://storageexplorer.com/">Microsoft Azure Storage Explorer</a></span><span class="sxs-lookup"><span data-stu-id="9f470-137"><a href="http://storageexplorer.com/">Microsoft Azure Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="9f470-138">X</span><span class="sxs-lookup"><span data-stu-id="9f470-138">X</span></span></td>
    <td><span data-ttu-id="9f470-139">X</span><span class="sxs-lookup"><span data-stu-id="9f470-139">X</span></span></td>
    <td><span data-ttu-id="9f470-140">X</span><span class="sxs-lookup"><span data-stu-id="9f470-140">X</span></span></td>
    <td><span data-ttu-id="9f470-141">X</span><span class="sxs-lookup"><span data-stu-id="9f470-141">X</span></span></td>
    <td><span data-ttu-id="9f470-142">X</span><span class="sxs-lookup"><span data-stu-id="9f470-142">X</span></span></td>
    <td><span data-ttu-id="9f470-143">X</span><span class="sxs-lookup"><span data-stu-id="9f470-143">X</span></span></td>
    <td><span data-ttu-id="9f470-144">I</span><span class="sxs-lookup"><span data-stu-id="9f470-144">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-145">X</span><span class="sxs-lookup"><span data-stu-id="9f470-145">X</span></span></td>
    <td><span data-ttu-id="9f470-146">X</span><span class="sxs-lookup"><span data-stu-id="9f470-146">X</span></span></td>
    <td><span data-ttu-id="9f470-147">X</span><span class="sxs-lookup"><span data-stu-id="9f470-147">X</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-148"><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">A Microsoft Visual Studio Server Explorer</a></span><span class="sxs-lookup"><span data-stu-id="9f470-148"><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Microsoft Visual Studio Server Explorer</a></span></span></td>
    <td><span data-ttu-id="9f470-149">X</span><span class="sxs-lookup"><span data-stu-id="9f470-149">X</span></span></td>
    <td><span data-ttu-id="9f470-150">X</span><span class="sxs-lookup"><span data-stu-id="9f470-150">X</span></span></td>
    <td><span data-ttu-id="9f470-151">X</span><span class="sxs-lookup"><span data-stu-id="9f470-151">X</span></span></td>
    <td><span data-ttu-id="9f470-152">X</span><span class="sxs-lookup"><span data-stu-id="9f470-152">X</span></span></td>
    <td><span data-ttu-id="9f470-153">X</span><span class="sxs-lookup"><span data-stu-id="9f470-153">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-154">I</span><span class="sxs-lookup"><span data-stu-id="9f470-154">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-155">X</span><span class="sxs-lookup"><span data-stu-id="9f470-155">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
</table>

<span data-ttu-id="9f470-156">**Az Azure Storage külső ügyféleszközök**</span><span class="sxs-lookup"><span data-stu-id="9f470-156">**Third-Party Azure Storage Client Tools**</span></span>

<span data-ttu-id="9f470-157">A Microsoft nem ellenőrizte a funkció vagy a következő külső eszközök által igényelt minőségét és a listaelem nem feltétlenül jelenti azt egy Microsoft záradéka.</span><span class="sxs-lookup"><span data-stu-id="9f470-157">We have not verified the functionality or quality claimed by the following third-party tools and their listing does not imply an endorsement by Microsoft.</span></span>

<table>
  <tr>
    <th rowspan="2"><span data-ttu-id="9f470-158">Az Azure Storage ügyfél eszköz</span><span class="sxs-lookup"><span data-stu-id="9f470-158">Azure Storage Client Tool</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-159">Blokkblob</span><span class="sxs-lookup"><span data-stu-id="9f470-159">Block Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-160">Az oldalakra vonatkozó Blob</span><span class="sxs-lookup"><span data-stu-id="9f470-160">Page Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-161">Hozzáfűző Blob</span><span class="sxs-lookup"><span data-stu-id="9f470-161">Append Blob</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-162">Táblák</span><span class="sxs-lookup"><span data-stu-id="9f470-162">Tables</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-163">Üzenetsorok</span><span class="sxs-lookup"><span data-stu-id="9f470-163">Queues</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-164">Fájlok</span><span class="sxs-lookup"><span data-stu-id="9f470-164">Files</span></span></th>
    <th rowspan="2"><span data-ttu-id="9f470-165">Ingyenes</span><span class="sxs-lookup"><span data-stu-id="9f470-165">Free</span></span></th>
    <th colspan="4"><span data-ttu-id="9f470-166">Platform</span><span class="sxs-lookup"><span data-stu-id="9f470-166">Platform</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-167">Web</span><span class="sxs-lookup"><span data-stu-id="9f470-167">Web</span></span></td>
    <td><span data-ttu-id="9f470-168">Windows</span><span class="sxs-lookup"><span data-stu-id="9f470-168">Windows</span></span></td>
    <td><span data-ttu-id="9f470-169">OSX</span><span class="sxs-lookup"><span data-stu-id="9f470-169">OSX</span></span></td>
    <td><span data-ttu-id="9f470-170">Linux</span><span class="sxs-lookup"><span data-stu-id="9f470-170">Linux</span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-171"><a href="http://www.cloudportam.com/">Felhő Portam</a></span><span class="sxs-lookup"><span data-stu-id="9f470-171"><a href="http://www.cloudportam.com/">Cloud Portam</a></span></span></td>
    <td><span data-ttu-id="9f470-172">X</span><span class="sxs-lookup"><span data-stu-id="9f470-172">X</span></span></td>
    <td><span data-ttu-id="9f470-173">X</span><span class="sxs-lookup"><span data-stu-id="9f470-173">X</span></span></td>
    <td><span data-ttu-id="9f470-174">X</span><span class="sxs-lookup"><span data-stu-id="9f470-174">X</span></span></td>
    <td><span data-ttu-id="9f470-175">X</span><span class="sxs-lookup"><span data-stu-id="9f470-175">X</span></span></td>
    <td><span data-ttu-id="9f470-176">X</span><span class="sxs-lookup"><span data-stu-id="9f470-176">X</span></span></td>
    <td><span data-ttu-id="9f470-177">X</span><span class="sxs-lookup"><span data-stu-id="9f470-177">X</span></span></td>
    <td><span data-ttu-id="9f470-178">Próbaverzió</span><span class="sxs-lookup"><span data-stu-id="9f470-178">Trial</span></span></td>
    <td><span data-ttu-id="9f470-179">X</span><span class="sxs-lookup"><span data-stu-id="9f470-179">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-180"><a href="http://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Az Azure Management Studio</a></span><span class="sxs-lookup"><span data-stu-id="9f470-180"><a href="http://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Azure Management Studio</a></span></span></td>
    <td><span data-ttu-id="9f470-181">X</span><span class="sxs-lookup"><span data-stu-id="9f470-181">X</span></span></td>
    <td><span data-ttu-id="9f470-182">X</span><span class="sxs-lookup"><span data-stu-id="9f470-182">X</span></span></td>
    <td><span data-ttu-id="9f470-183">X</span><span class="sxs-lookup"><span data-stu-id="9f470-183">X</span></span></td>
    <td><span data-ttu-id="9f470-184">X</span><span class="sxs-lookup"><span data-stu-id="9f470-184">X</span></span></td>
    <td><span data-ttu-id="9f470-185">X</span><span class="sxs-lookup"><span data-stu-id="9f470-185">X</span></span></td>
    <td><span data-ttu-id="9f470-186">X</span><span class="sxs-lookup"><span data-stu-id="9f470-186">X</span></span></td>
    <td><span data-ttu-id="9f470-187">Próbaverzió</span><span class="sxs-lookup"><span data-stu-id="9f470-187">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-188">X</span><span class="sxs-lookup"><span data-stu-id="9f470-188">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-189"><a href="http://www.cerebrata.com/products/azure-explorer/introduction">Cerabrata: Az Azure Explorer</a></span><span class="sxs-lookup"><span data-stu-id="9f470-189"><a href="http://www.cerebrata.com/products/azure-explorer/introduction">Cerabrata: Azure Explorer</a></span></span></td>
    <td><span data-ttu-id="9f470-190">X</span><span class="sxs-lookup"><span data-stu-id="9f470-190">X</span></span></td>
    <td><span data-ttu-id="9f470-191">X</span><span class="sxs-lookup"><span data-stu-id="9f470-191">X</span></span></td>
    <td><span data-ttu-id="9f470-192">X</span><span class="sxs-lookup"><span data-stu-id="9f470-192">X</span></span></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="9f470-193">X</span><span class="sxs-lookup"><span data-stu-id="9f470-193">X</span></span></td>
    <td><span data-ttu-id="9f470-194">I</span><span class="sxs-lookup"><span data-stu-id="9f470-194">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-195">X</span><span class="sxs-lookup"><span data-stu-id="9f470-195">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-196"><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Storage Explorer</a></span><span class="sxs-lookup"><span data-stu-id="9f470-196"><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="9f470-197">X</span><span class="sxs-lookup"><span data-stu-id="9f470-197">X</span></span></td>
    <td><span data-ttu-id="9f470-198">X</span><span class="sxs-lookup"><span data-stu-id="9f470-198">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-199">X</span><span class="sxs-lookup"><span data-stu-id="9f470-199">X</span></span></td>
    <td><span data-ttu-id="9f470-200">X</span><span class="sxs-lookup"><span data-stu-id="9f470-200">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-201">I</span><span class="sxs-lookup"><span data-stu-id="9f470-201">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-202">X</span><span class="sxs-lookup"><span data-stu-id="9f470-202">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-203"><a href="http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx">CloudBerry Explorer</a></span><span class="sxs-lookup"><span data-stu-id="9f470-203"><a href="http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx">CloudBerry Explorer</a></span></span></td>
    <td><span data-ttu-id="9f470-204">X</span><span class="sxs-lookup"><span data-stu-id="9f470-204">X</span></span></td>
    <td><span data-ttu-id="9f470-205">X</span><span class="sxs-lookup"><span data-stu-id="9f470-205">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="9f470-206">X</span><span class="sxs-lookup"><span data-stu-id="9f470-206">X</span></span></td>
    <td><span data-ttu-id="9f470-207">I/N</span><span class="sxs-lookup"><span data-stu-id="9f470-207">Y/N</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-208">X</span><span class="sxs-lookup"><span data-stu-id="9f470-208">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-209"><a href="http://www.gapotchenko.com/cloudcombine">Felhő egyesítése</a></span><span class="sxs-lookup"><span data-stu-id="9f470-209"><a href="http://www.gapotchenko.com/cloudcombine">Cloud Combine</a></span></span></td>
    <td><span data-ttu-id="9f470-210">X</span><span class="sxs-lookup"><span data-stu-id="9f470-210">X</span></span></td>
    <td><span data-ttu-id="9f470-211">X</span><span class="sxs-lookup"><span data-stu-id="9f470-211">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-212">X</span><span class="sxs-lookup"><span data-stu-id="9f470-212">X</span></span></td>
    <td><span data-ttu-id="9f470-213">X</span><span class="sxs-lookup"><span data-stu-id="9f470-213">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-214">Próbaverzió</span><span class="sxs-lookup"><span data-stu-id="9f470-214">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-215">X</span><span class="sxs-lookup"><span data-stu-id="9f470-215">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-216"><a href="http://clumsyleaf.com">ClumsyLeaf: AzureXplorer, CloudXplorer, TableXplorer</a></span><span class="sxs-lookup"><span data-stu-id="9f470-216"><a href="http://clumsyleaf.com">ClumsyLeaf: AzureXplorer, CloudXplorer, TableXplorer</a></span></span></td>
    <td><span data-ttu-id="9f470-217">X</span><span class="sxs-lookup"><span data-stu-id="9f470-217">X</span></span></td>
    <td><span data-ttu-id="9f470-218">X</span><span class="sxs-lookup"><span data-stu-id="9f470-218">X</span></span></td>
    <td><span data-ttu-id="9f470-219">X</span><span class="sxs-lookup"><span data-stu-id="9f470-219">X</span></span></td>
    <td><span data-ttu-id="9f470-220">X</span><span class="sxs-lookup"><span data-stu-id="9f470-220">X</span></span></td>
    <td><span data-ttu-id="9f470-221">X</span><span class="sxs-lookup"><span data-stu-id="9f470-221">X</span></span></td>
    <td><span data-ttu-id="9f470-222">X</span><span class="sxs-lookup"><span data-stu-id="9f470-222">X</span></span></td>
    <td><span data-ttu-id="9f470-223">I</span><span class="sxs-lookup"><span data-stu-id="9f470-223">Y</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-224">X</span><span class="sxs-lookup"><span data-stu-id="9f470-224">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-225"><a href="http://www.gladinet.com/Azure-Storage/index.htm">Gladinet felhő</a></span><span class="sxs-lookup"><span data-stu-id="9f470-225"><a href="http://www.gladinet.com/Azure-Storage/index.htm">Gladinet Cloud</a></span></span></td>
    <td><span data-ttu-id="9f470-226">X</span><span class="sxs-lookup"><span data-stu-id="9f470-226">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td><span data-ttu-id="9f470-227">Próbaverzió</span><span class="sxs-lookup"><span data-stu-id="9f470-227">Trial</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-228">X</span><span class="sxs-lookup"><span data-stu-id="9f470-228">X</span></span></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-229"><a href="http://storageexplorer.codeplex.com/">Az Azure Web Tártallózó</a></span><span class="sxs-lookup"><span data-stu-id="9f470-229"><a href="http://storageexplorer.codeplex.com/">Azure Web Storage Explorer</a></span></span></td>
    <td><span data-ttu-id="9f470-230">X</span><span class="sxs-lookup"><span data-stu-id="9f470-230">X</span></span></td>
    <td><span data-ttu-id="9f470-231">X</span><span class="sxs-lookup"><span data-stu-id="9f470-231">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-232">X</span><span class="sxs-lookup"><span data-stu-id="9f470-232">X</span></span></td>
    <td><span data-ttu-id="9f470-233">X</span><span class="sxs-lookup"><span data-stu-id="9f470-233">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-234">I</span><span class="sxs-lookup"><span data-stu-id="9f470-234">Y</span></span></td>
    <td><span data-ttu-id="9f470-235">X</span><span class="sxs-lookup"><span data-stu-id="9f470-235">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span data-ttu-id="9f470-236"><a href="https://zudio.co/">Zudio</a></span><span class="sxs-lookup"><span data-stu-id="9f470-236"><a href="https://zudio.co/">Zudio</a></span></span></td>
    <td><span data-ttu-id="9f470-237">X</span><span class="sxs-lookup"><span data-stu-id="9f470-237">X</span></span></td>
    <td><span data-ttu-id="9f470-238">X</span><span class="sxs-lookup"><span data-stu-id="9f470-238">X</span></span></td>
    <td></td>
    <td><span data-ttu-id="9f470-239">X</span><span class="sxs-lookup"><span data-stu-id="9f470-239">X</span></span></td>
    <td><span data-ttu-id="9f470-240">X</span><span class="sxs-lookup"><span data-stu-id="9f470-240">X</span></span></td>
    <td><span data-ttu-id="9f470-241">X</span><span class="sxs-lookup"><span data-stu-id="9f470-241">X</span></span></td>
    <td><span data-ttu-id="9f470-242">Próbaverzió</span><span class="sxs-lookup"><span data-stu-id="9f470-242">Trial</span></span></td>
    <td><span data-ttu-id="9f470-243">X</span><span class="sxs-lookup"><span data-stu-id="9f470-243">X</span></span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
