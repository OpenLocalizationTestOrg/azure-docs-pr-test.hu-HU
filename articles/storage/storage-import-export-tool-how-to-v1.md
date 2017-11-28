---
title: "aaaUsing hello Azure Import/Export eszköz - 1-es verzió |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Import/Export eszköz tooprepare merevlemezeit, az importálási feladat javítsa az importálási feladat, vagy javítsa ki az exportálási feladat."
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
ms.openlocfilehash: e2be722b32b4ee57adb273e3fd07dec1e3b13375
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-importexport-tool-v1"></a><span data-ttu-id="029e2-103">Hello Azure Import/Export eszköz (v1) használatával</span><span class="sxs-lookup"><span data-stu-id="029e2-103">Using hello Azure Import/Export Tool (v1)</span></span>

<span data-ttu-id="029e2-104">hello Azure Import/Export eszköz (WAImportExport.exe) használt toocreate és hello Azure Import/Export szolgáltatás, amely lehetővé teszi, tootransfer nagy mennyiségű adat virtuális gépbe vagy onnan Azure Blob Storage feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="029e2-104">hello Azure Import/Export Tool (WAImportExport.exe) is used toocreate and manage jobs for hello Azure Import/Export service, enabling you tootransfer large amounts of data into or out of Azure Blob Storage.</span></span>

<span data-ttu-id="029e2-105">Ez a dokumentáció az Azure Import/Export eszköz hello v1 szolgál.</span><span class="sxs-lookup"><span data-stu-id="029e2-105">This documentation is for v1 of hello Azure Import/Export Tool.</span></span> <span data-ttu-id="029e2-106">Hello hello eszköz legújabb verziójának használatával kapcsolatos információkért lásd: [Using hello Azure Import/Export eszköz](storage-import-export-tool-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="029e2-106">For information about using hello most recent version of hello tool, please see [Using hello Azure Import/Export Tool](storage-import-export-tool-how-to.md).</span></span>

<span data-ttu-id="029e2-107">Ezek a cikkek fog látni, hogyan toouse hello eszköz toodo hello következő:</span><span class="sxs-lookup"><span data-stu-id="029e2-107">In these articles, you will see how toouse hello tool toodo hello following:</span></span>

- <span data-ttu-id="029e2-108">Telepítsen és konfiguráljon hello Import/Export eszköz.</span><span class="sxs-lookup"><span data-stu-id="029e2-108">Install and set up hello Import/Export Tool.</span></span>
- <span data-ttu-id="029e2-109">Készítse elő a merevlemez-meghajtók ahol adatokat importálhat a meghajtók tooAzure Blob Storage-feladat.</span><span class="sxs-lookup"><span data-stu-id="029e2-109">Prepare your hard drives for a job where you import data from your drives tooAzure Blob Storage.</span></span>
- <span data-ttu-id="029e2-110">Tekintse át a feladat állapotának hello másolási naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="029e2-110">Review hello status of a job with Copy Log Files.</span></span> 
- <span data-ttu-id="029e2-111">Javítsa az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="029e2-111">Repair an import job.</span></span> 
- <span data-ttu-id="029e2-112">Javítsa ki az exportálási feladat.</span><span class="sxs-lookup"><span data-stu-id="029e2-112">Repair an export job.</span></span> 
- <span data-ttu-id="029e2-113">Végezzen hibaelhárítást a hello Azure Import/Export eszköz, abban az esetben, ha a probléma volt a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="029e2-113">Troubleshoot hello Azure Import/Export Tool, in case you had a problem during process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="029e2-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="029e2-114">Next steps</span></span>

* [<span data-ttu-id="029e2-115">Hello WAImportExport eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="029e2-115">Setting up hello WAImportExport tool</span></span>](storage-import-export-tool-how-to.md)
