---
title: "aaaTroubleshooting hello Azure Import/Export eszköz |} Microsoft Docs"
description: "Megismerhet néhány hello tapasztalt gyakori problémákat mutatjuk hello Azure Import/Export eszköz használatakor, és hogyan toohandle őket."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: b91ca5eb-c557-460a-9afc-0590b38471f9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 5445cefe2703edf4d9d285f761433b7b66d8cb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-azure-importexport-tool"></a>Hibaelhárítási hello Azure Import/Export eszköz
Microsoft Azure Import/Export eszköz hello hibaüzenetek adja vissza, ha fut a problémákat. Ez a témakör néhány gyakori probléma, amely futtathatják azokat.  
  
## <a name="a-copy-session-fails-what-i-should-do"></a>A másolási munkamenet nem sikerül, mit kell tenni?  
 A másolási munkamenet sikertelensége esetén van két lehetőség közül választhat:  
  
 Ha hello hiba Újrapróbálkozást lehetővé tevő, például ha hálózati megosztásra hello offline állapotban volt, a rövid időszak, és most újra online állapotba kerül, folytathatja a hello másolási munkamenet. Ha hello hiba nem Újrapróbálkozást lehetővé tevő, például ha a megadott hello hibás fájl forráskönyvtár hello parancssori paraméterek vannak, akkor tooabort hello másolási munkamenet. Lásd: [előkészítése merevlemezek hogy az importálás](storage-import-export-tool-preparing-hard-drives-import-v1.md) folytatása és másolási munkamenet megszakítása további információt.  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a>Nem lehet folytatni vagy egy másolás munkamenet megszakítása.  
 Ha hello példány neve hello első másolás munkamenet meghajtóhoz, majd hello hibaüzenet kell nyújtaniuk: "hello első másolás munkamenet nem folytatható vagy megszakadt." Ebben az esetben törölje hello régi napló fájlt, és futtassa újra hello parancsot.  
  
 Ha a Másolás munkamenet nem van hello elsőt meghajtóhoz, azt is mindig folytatódik, vagy megszakadt.  
  
## <a name="i-lost-hello-journal-file-can-i-still-create-hello-job"></a>Hello napló fájl elvesztése, továbbra is létrehozhatók hello feladatot?  
 hello napló fájl meghajtóhoz hello teljes körű információkat toothis adatmeghajtó másolásának tartalmaz, és azt szükséges tooadd további fájlokat toohello meghajtót, azaz használt toocreate az importálási feladat. Ha hello naplófájl elveszik, akkor minden hello másolási munkamenet hello meghajtó tooredo.  
  
## <a name="next-steps"></a>Következő lépések
 
* [Hello azure import/export eszköz beállítása](storage-import-export-tool-setup-v1.md)   
* [Merevlemezek előkészítése importálási feladatokhoz](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Feladatok állapotának áttekintése a másolási naplófájlok segítségével](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Importálási feladat javítása](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Exportálási feladat javítása](storage-import-export-tool-repairing-an-export-job-v1.md)
