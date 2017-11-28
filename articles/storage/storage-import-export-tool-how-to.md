---
title: "Az Azure Import/Export eszközzel |} Microsoft Docs"
description: "Útmutató: az Import/Export eszköz segítségével készítse elő a merevlemez-meghajtók egy importálási feladatnak, javítsa az importálási feladat, vagy javítsa ki az exportálási feladat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f77535bb-d577-438a-bdd3-e15a82e0c543
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 86073f5d15253d658fcb371e913dd3a543a2b075
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-azure-importexport-tool"></a><span data-ttu-id="f5264-103">Az Azure Import/Export eszköz használatával</span><span class="sxs-lookup"><span data-stu-id="f5264-103">Using the Azure Import/Export Tool</span></span> 

<span data-ttu-id="f5264-104">Az Azure Import/Export eszköz (WAImportExport.exe) segítségével az Azure Import/Export szolgáltatás feladatok létrehozásához és kezeléséhez így lehetővé teszi nagy adatmennyiségek átvitelét a virtuális gépbe vagy onnan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f5264-104">The Azure Import/Export Tool (WAImportExport.exe) is used to create and manage jobs for the Azure Import/Export service, enabling you to transfer large amounts of data into or out of Azure Blob Storage.</span></span>

<span data-ttu-id="f5264-105">Ez a dokumentáció az Azure Import/Export eszköz legújabb verziója van.</span><span class="sxs-lookup"><span data-stu-id="f5264-105">This documentation is for the most recent version of the Azure Import/Export Tool.</span></span> <span data-ttu-id="f5264-106">A V1-es eszköz használatával kapcsolatos információkért lásd: [használata az Azure Import/Export eszköz v1](storage-import-export-tool-how-to-v1.md).</span><span class="sxs-lookup"><span data-stu-id="f5264-106">For information about using the v1 tool, please see [Using the Azure Import/Export Tool v1](storage-import-export-tool-how-to-v1.md).</span></span>

<span data-ttu-id="f5264-107">Ezek a cikkek látni fogja az eszköz használatáról a következő:</span><span class="sxs-lookup"><span data-stu-id="f5264-107">In these articles, you will see how to use the tool to do the following:</span></span>  

- <span data-ttu-id="f5264-108">Telepítse és állítsa be az Import/Export eszköz.</span><span class="sxs-lookup"><span data-stu-id="f5264-108">Install and set up the Import/Export Tool.</span></span>
- <span data-ttu-id="f5264-109">Készítse elő a merevlemez-meghajtók egy feladatot, amikor adatokat importál a meghajtók Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f5264-109">Prepare your hard drives for a job where you import data from your drives to Azure Blob Storage.</span></span>
- <span data-ttu-id="f5264-110">Tekintse át a feladat állapotának másolási naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="f5264-110">Review the status of a job with Copy Log Files.</span></span> 
- <span data-ttu-id="f5264-111">Javítsa az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="f5264-111">Repair an import job.</span></span> 
- <span data-ttu-id="f5264-112">Javítsa ki az exportálási feladat.</span><span class="sxs-lookup"><span data-stu-id="f5264-112">Repair an export job.</span></span> 
- <span data-ttu-id="f5264-113">Végezzen hibaelhárítást az Azure Import/Export eszköz, abban az esetben, ha a probléma volt a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="f5264-113">Troubleshoot the Azure Import/Export Tool, in case you had a problem during process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f5264-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f5264-114">Next steps</span></span>

* [<span data-ttu-id="f5264-115">A WAImportExport eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="f5264-115">Setting up the WAImportExport tool</span></span>](storage-import-export-tool-setup.md)