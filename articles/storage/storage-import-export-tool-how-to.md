---
title: "aaaUsing hello Azure Import/Export eszköz |} Microsoft Docs"
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
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: fa2021e5a03281128e494e6e63f58bc6319aeb4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-importexport-tool"></a><span data-ttu-id="8be38-103">Hello Azure Import/Export eszköz használatával</span><span class="sxs-lookup"><span data-stu-id="8be38-103">Using hello Azure Import/Export Tool</span></span> 

<span data-ttu-id="8be38-104">hello Azure Import/Export eszköz (WAImportExport.exe) használt toocreate és hello Azure Import/Export szolgáltatás, amely lehetővé teszi, tootransfer nagy mennyiségű adat virtuális gépbe vagy onnan Azure Blob Storage feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="8be38-104">hello Azure Import/Export Tool (WAImportExport.exe) is used toocreate and manage jobs for hello Azure Import/Export service, enabling you tootransfer large amounts of data into or out of Azure Blob Storage.</span></span>

<span data-ttu-id="8be38-105">Ebben a dokumentációban hello hello Azure Import/Export eszköz legújabb verziója van.</span><span class="sxs-lookup"><span data-stu-id="8be38-105">This documentation is for hello most recent version of hello Azure Import/Export Tool.</span></span> <span data-ttu-id="8be38-106">Hello V1-es eszköz használatával kapcsolatos információkért lásd: [Using hello Azure Import/Export eszköz v1](storage-import-export-tool-how-to-v1.md).</span><span class="sxs-lookup"><span data-stu-id="8be38-106">For information about using hello v1 tool, please see [Using hello Azure Import/Export Tool v1](storage-import-export-tool-how-to-v1.md).</span></span>

<span data-ttu-id="8be38-107">Ezek a cikkek fog látni, hogyan toouse hello eszköz toodo hello következő:</span><span class="sxs-lookup"><span data-stu-id="8be38-107">In these articles, you will see how toouse hello tool toodo hello following:</span></span>  

- <span data-ttu-id="8be38-108">Telepítsen és konfiguráljon hello Import/Export eszköz.</span><span class="sxs-lookup"><span data-stu-id="8be38-108">Install and set up hello Import/Export Tool.</span></span>
- <span data-ttu-id="8be38-109">Készítse elő a merevlemez-meghajtók ahol adatokat importálhat a meghajtók tooAzure Blob Storage-feladat.</span><span class="sxs-lookup"><span data-stu-id="8be38-109">Prepare your hard drives for a job where you import data from your drives tooAzure Blob Storage.</span></span>
- <span data-ttu-id="8be38-110">Tekintse át a feladat állapotának hello másolási naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="8be38-110">Review hello status of a job with Copy Log Files.</span></span> 
- <span data-ttu-id="8be38-111">Javítsa az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="8be38-111">Repair an import job.</span></span> 
- <span data-ttu-id="8be38-112">Javítsa ki az exportálási feladat.</span><span class="sxs-lookup"><span data-stu-id="8be38-112">Repair an export job.</span></span> 
- <span data-ttu-id="8be38-113">Végezzen hibaelhárítást a hello Azure Import/Export eszköz, abban az esetben, ha a probléma volt a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="8be38-113">Troubleshoot hello Azure Import/Export Tool, in case you had a problem during process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="8be38-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8be38-114">Next steps</span></span>

* [<span data-ttu-id="8be38-115">Hello WAImportExport eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="8be38-115">Setting up hello WAImportExport tool</span></span>](storage-import-export-tool-setup.md)
