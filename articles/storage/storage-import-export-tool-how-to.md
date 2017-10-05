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
# <a name="using-the-azure-importexport-tool"></a>Az Azure Import/Export eszköz használatával 

Az Azure Import/Export eszköz (WAImportExport.exe) segítségével az Azure Import/Export szolgáltatás feladatok létrehozásához és kezeléséhez így lehetővé teszi nagy adatmennyiségek átvitelét a virtuális gépbe vagy onnan Azure Blob Storage.

Ez a dokumentáció az Azure Import/Export eszköz legújabb verziója van. A V1-es eszköz használatával kapcsolatos információkért lásd: [használata az Azure Import/Export eszköz v1](storage-import-export-tool-how-to-v1.md).

Ezek a cikkek látni fogja az eszköz használatáról a következő:  

- Telepítse és állítsa be az Import/Export eszköz.
- Készítse elő a merevlemez-meghajtók egy feladatot, amikor adatokat importál a meghajtók Azure Blob Storage.
- Tekintse át a feladat állapotának másolási naplófájlok. 
- Javítsa az importálási feladat. 
- Javítsa ki az exportálási feladat. 
- Végezzen hibaelhárítást az Azure Import/Export eszköz, abban az esetben, ha a probléma volt a folyamat során. 

## <a name="next-steps"></a>Következő lépések

* [A WAImportExport eszköz beállítása](storage-import-export-tool-setup.md)