---
title: "aaaReviewing Azure Import/Export feladatállapot - 1-es verzió |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse hello naplófájlokat, ha létre hello importálása vagy exportálása feladat volt futtatva toosee hello hello importálási/exportálási feladatok állapotát."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: c69d1d69-6403-4eee-9949-0185faeecfa1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.openlocfilehash: 363731ede4751124a714b4ce96852e0b8c4dbca4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>A másolási naplófájlok Azure Import/Export feladat állapota áttekintése
A Microsoft Azure Import/Export szolgáltatás hello egy importálása vagy exportálása feladattal társított meghajtók dolgozza fel, írja az másolási napló fájlok toohello tárolási fiók tooor, amelyből importálunk vagy bináris objektumok exportálása. hello naplófájl minden olyan fájlról, amely lett importálva, illetve nem exportálható részletes állapotát tartalmazza. Befejezett feladatok; hello állapotának lekérdezése hello URL-cím tooeach másolási naplófájl adott vissza. Lásd: [Get Job](/rest/api/storageservices/Get-Job3) további információt.  

## <a name="example-urls"></a>Példa URL-címek

Az alábbiakban hello példa URL-címek másolása naplófájlok hogy az importálás két meghajtók:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 Lásd: [Import/Export szolgáltatás naplófájlformátum](../storage-import-export-file-format-log.md) hello formátum másolási naplók és állapotkódok hello teljes listáját.  
  
## <a name="next-steps"></a>Következő lépések
 
 * [Hello Azure Import/Export eszköz beállítása](storage-import-export-tool-setup-v1.md)   
 * [Merevlemezek előkészítése importálási feladatokhoz](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Importálási feladat javítása](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Exportálási feladat javítása](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Hibaelhárítási hello Azure Import/Export eszköz](storage-import-export-tool-troubleshooting-v1.md)
