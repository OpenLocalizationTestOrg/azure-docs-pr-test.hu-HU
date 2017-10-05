---
title: "Az Azure Import/Export eszközzel - 1-es verzió |} Microsoft Docs"
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
ms.date: 1/15/2017
ms.author: muralikk
ms.openlocfilehash: 67bdfa8c2cd0f8314c82e2b334a3fa3a5c520c66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-azure-importexport-tool-v1"></a><span data-ttu-id="08ac2-103">Az Azure Import/Export eszközzel (v1)</span><span class="sxs-lookup"><span data-stu-id="08ac2-103">Using the Azure Import/Export Tool (v1)</span></span>

<span data-ttu-id="08ac2-104">Az Azure Import/Export eszköz (WAImportExport.exe) segítségével az Azure Import/Export szolgáltatás feladatok létrehozásához és kezeléséhez így lehetővé teszi nagy adatmennyiségek átvitelét a virtuális gépbe vagy onnan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="08ac2-104">The Azure Import/Export Tool (WAImportExport.exe) is used to create and manage jobs for the Azure Import/Export service, enabling you to transfer large amounts of data into or out of Azure Blob Storage.</span></span>

<span data-ttu-id="08ac2-105">Ez a dokumentáció az Azure Import/Export eszköz v1 szolgál.</span><span class="sxs-lookup"><span data-stu-id="08ac2-105">This documentation is for v1 of the Azure Import/Export Tool.</span></span> <span data-ttu-id="08ac2-106">Az eszköz legújabb verziójának használatával kapcsolatos információkért lásd: [az Azure Import/Export eszközzel](storage-import-export-tool-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="08ac2-106">For information about using the most recent version of the tool, please see [Using the Azure Import/Export Tool](storage-import-export-tool-how-to.md).</span></span>

<span data-ttu-id="08ac2-107">Ezek a cikkek látni fogja az eszköz használatáról a következő:</span><span class="sxs-lookup"><span data-stu-id="08ac2-107">In these articles, you will see how to use the tool to do the following:</span></span>

- <span data-ttu-id="08ac2-108">Telepítse és állítsa be az Import/Export eszköz.</span><span class="sxs-lookup"><span data-stu-id="08ac2-108">Install and set up the Import/Export Tool.</span></span>
- <span data-ttu-id="08ac2-109">Készítse elő a merevlemez-meghajtók egy feladatot, amikor adatokat importál a meghajtók Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="08ac2-109">Prepare your hard drives for a job where you import data from your drives to Azure Blob Storage.</span></span>
- <span data-ttu-id="08ac2-110">Tekintse át a feladat állapotának másolási naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="08ac2-110">Review the status of a job with Copy Log Files.</span></span> 
- <span data-ttu-id="08ac2-111">Javítsa az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="08ac2-111">Repair an import job.</span></span> 
- <span data-ttu-id="08ac2-112">Javítsa ki az exportálási feladat.</span><span class="sxs-lookup"><span data-stu-id="08ac2-112">Repair an export job.</span></span> 
- <span data-ttu-id="08ac2-113">Végezzen hibaelhárítást az Azure Import/Export eszköz, abban az esetben, ha a probléma volt a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="08ac2-113">Troubleshoot the Azure Import/Export Tool, in case you had a problem during process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="08ac2-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="08ac2-114">Next steps</span></span>

* [<span data-ttu-id="08ac2-115">A WAImportExport eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="08ac2-115">Setting up the WAImportExport tool</span></span>](storage-import-export-tool-how-to.md)