---
title: "aaaConfiguring és a Storage Emulator a Visual Studio használatával hello |} Microsoft Docs"
description: "A Visual Studio hello Storage Emulator használatával és konfigurálása"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: c8e7996f-6027-4762-806e-614b93131867
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/17/2017
ms.author: kraigb
ms.openlocfilehash: d590f21146c86bcb7bfa6b6164b92c6df5938d5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-and-using-hello-storage-emulator-with-visual-studio"></a>A Visual Studio hello Storage Emulator használatával és konfigurálása
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Áttekintés
hello Azure SDK fejlesztési környezet magában foglalja a hello storage emulatort, a segédprogram, amely szimulálja hello Blob, Queue és Table storage szolgáltatások az Azure-ban található a helyi fejlesztési számítógépén. Ha egy felhőalapú szolgáltatás létrehozása, amely védett hello Azure storage szolgáltatások, vagy, hogy a hívások tárolószolgáltatások hello írása bármely külső alkalmazások, tesztelheti a helyileg hello storage emulatorban kódot. hello Azure eszközök a Microsoft Visual Studio integrálja hello storage emulator ebbe a Visual Studio. hello Azure eszközök inicializálása hello storage emulator adatbázis első használat, elindul hello storage emulator szolgáltatás futtatásához vagy hibakeresése a Visual Studio eszközből a kódot, és adja meg a csak olvasási hozzáféréssel toohello storage emulator adatait hello Azure Tártallózó keresztül.

Részletes információk hello storage emulatort, beleértve a telepítési rendszerkövetelményeket és egyéni konfigurációs utasításokat: [fejlesztés és tesztelés az Azure Storage Emulator használata hello](storage/common/storage-use-emulator.md).

> [!NOTE]
> Néhány funkció hello storage emulator szimuláció és hello Azure storage szolgáltatások közötti különbségek vannak. Lásd: [közötti különbségek hello Storage Emulator és az Azure Storage szolgáltatásainak](storage/common/storage-use-emulator.md) hello hello speciális eltéréseket Azure SDK dokumentációjában található.
> 
> 

## <a name="configuring-a-connection-string-for-hello-storage-emulator"></a>Hello storage Emulator kapcsolati karakterlánc konfigurálása
tooaccess hello storage emulatort a szerepkörön belüli kódból, érdemes tooconfigure kapcsolatot karakterláncot adott pontok toohello storage emulatort, és később lehet, hogy módosított toopoint tooan Azure storage-fiók. A kapcsolati karakterlánc egy konfigurációs beállítás, amelyet a szerepkör elolvashatják a futásidejű tooconnect tooa tárfiók. További információ toocreate kapcsolati karakterláncok, lásd: [konfigurálása hello Azure alkalmazás](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

> [!NOTE]
> Adhatja vissza a hivatkozás toohello emulátor tárfiók a kód hello segítségével **DevelopmentStorageAccount** tulajdonság. Ez a megközelítés megfelelően működik, ha azt szeretné, hogy tooaccess hello a felhasználói kódból storage emulatort, de ha toopublish az alkalmazás tooAzure, lesz egy kapcsolati karakterlánc tooaccess toocreate kell az Azure storage-fiók és a kód toouse módosítása, közzététel előtt kapcsolati karakterláncot. Ha vált hello emulátor tárfiók és az Azure-tárfiók között gyakran, akkor egy kapcsolati karakterláncot a folyamat leegyszerűsítése érdekében.
> 
> 

## <a name="initializing-and-running-hello-storage-emulator"></a>Inicializálása és futtatása hello storage emulator
Megadhatja, hogy futtatásakor vagy a szolgáltatás a Visual Studio hibakeresési, Visual Studio automatikusan elindítja a hello storage emulator. A Solution Explorerben nyissa meg a hello helyi menüje a **Azure** projektre, és válassza a **tulajdonságok**. A hello **fejlesztési** lap hello **indítsa el az Azure Storage Emulator** menüben válassza ki **igaz** (Ha nincs már toothat érték).

hello első alkalommal futtat vagy hibakeresése a Visual Studio, a hello storage emulator szolgáltatást az inicializálási folyamat elindítása. Ez a folyamat foglalja le a helyi portok hello storage Emulator és hello storage emulator adatbázist hoz létre. Művelet befejeződése után ez a folyamat nem kell toorun újra kivéve hello storage emulator adatbázisa törlődik.

> [!NOTE]
> A hello Azure eszközök hello 2012. júniusi kiadástól kezdve, hello storage emulator fut, alapértelmezés szerint az SQL Express localdb programba. Hello Azure eszközei korábbi kiadásaiban, hello storage emulator fut az SQL Express 2005 vagy 2008 alapértelmezett példánya is telepíthető, amelyet előtt telepíteni kell hello Azure SDK-t. Hello storage emulator egy SQL Express vagy egy nevesített megnevezett példánya vagy a Microsoft SQL Server alapértelmezett példányát is futtathatja. Ha tooconfigure hello storage emulator toorun elleni eltérő hello alapértelmezett példány példánya van szüksége, tekintse meg [fejlesztés és tesztelés az Azure Storage Emulator használata hello](storage/common/storage-use-emulator.md).
> 
> 

hello storage emulator hello helyi adattárolási szolgáltatások felhasználói felület tooview hello állapotot és toostart, állítsa le, és visszaállíthatja őket. Hello storage emulator szolgáltatás elindítása után hello felhasználói felület megjelenítéséhez, vagy indítsa el vagy hello szolgáltatás leállítása a hello értesítési területen megjelenő ikonra kattint a Microsoft Azure Emulátorban hello hello Windows tálcán.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>A Server Explorer storage emulator adatok megtekintése
a Server Explorer hello Azure Storage csomópont lehetővé teszi tooview adatok és a módosítási beállítások BLOB, és a storage-fiókok, beleértve a táblabeli adatok a storage emulator hello. Lásd: [a Tártallózó (előzetes verzió) kezelése az Azure Blob Storage-erőforrások](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) további információt.

