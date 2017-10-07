---
title: "aaaPersist eredményeit, illetve a naplók befejeződött feladatok és a feladatok tooa adatokat tároló - Azure Batch |} Microsoft Docs"
description: "Ismerje meg a kötegelt feladatok és munkák kimeneti adatait különböző lehetőségek közül. Lehet megőrizni a adatok tárolási tooAzure vagy tooanother adatok tárolásához."
services: batch
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0b11e387f1694e1ce3e9573db7f6013f0154cad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="persist-job-and-task-output"></a>Feladatok és tevékenységek kimenetének megőrzése

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Néhány gyakori példán feladat kimenetéből a következők:

- A fájlok jön létre, amikor hello feladat végpontja bemeneti adatok.
- A feladat a végrehajtás társított naplófájlokat. 

Ez a cikk ismerteti a különböző lehetőségek tárolásakor tevékenység kimeneti és hello forgatókönyvekhez készült, amelynek minden elem leginkább megfelelő.   

## <a name="about-hello-batch-file-conventions-standard"></a>Hello kötegelt fájl egyezmények standard kapcsolatos

Kötegelt meghatároz egy választható feladat kimeneti fájlok az Azure Storage elnevezési konvenciókat. Hello [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) ezek konvenciókat ismerteti. hello fájl egyezmények standard hello hello tároló és a blob célútvonalat az Azure Storage hello feladat- és hello neve alapján adott kimeneti fájl nevét határozza meg.

Már fel tooyou eldöntheti, hogy az toouse hello fájl egyezmények a kimeneti adatok fájlok elnevezési szabványa. Azonban kívánja hello céltárolója és blob is nevet. Ha hello fájl egyezmények szabványos kimeneti fájlok elnevezési, akkor a kimeneti fájlok megtekintését a hello [Azure-portálon][portal].

Hello fájl egyezmények standard használt néhány különböző módja van:

- Ha hello szolgáltatás API toopersist kimeneti parancsfájlokat használ, választhatja a tooname cél tárolók és blobok toohello szabványos fájl konvenciók szerint. hello Batch szolgáltatás API lehetővé teszi toopersist kimeneti fájlok ügyfélkódot, a feladat alkalmazás módosítása nélkül.
- Ha a .NET fejleszt, használhatja a hello [Azure Batch fájl egyezmények .NET-keretrendszerhez készült][nuget_package]. Ezt a szalagtárat használó előnye, hogy az támogatja a kimeneti fájlok tootheir azonosító vagy a cél szerint lekérdezése. hello beépített lekérdező funkció teszi, hogy könnyen tooaccess kimeneti fájlokat egy ügyfélalkalmazást, illetve más feladatok. A feladat alkalmazás azonban módosított toocall fájl egyezmények könyvtárban kell lennie. További információkért lásd: hello hello referencia [fájl egyezmények .NET-keretrendszerhez készült](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.aspx).
- .NET eltérő nyelvű fejleszt, ha az alkalmazás normál fájl egyezmények hello valósíthat meg.

## <a name="design-considerations-for-persisting-output"></a>Persisting kimeneti kialakítási szempontjai 

A Batch-megoldás tervezésekor vegye figyelembe a következő hello kapcsolódó toojob tényezők és a feladatok kimenete.

* **Számítási csomópont élettartama**: számítási csomópontok gyakran átmeneti jellegűek, különösen a készletek automatikus skálázás engedélyezve van. Egy csomóponton futó feladat kimenete csak során hello csomópont létezik-e, és csak a hello fájl megőrzési időn belül állított hello feladat érhető el. Ha a feladat kimenete a hello feladat végrehajtása után lehet szükség, hello feladat a kimeneti fájlok tooa tartós tárolójába például az Azure Storage kell feltöltenie.

* **A kimeneti tárolási**: a feladat kimenete adattárként Azure Storage ajánlott, de a tartós tárolót is használhatja. A feladat kimenete tooAzure írása tárolási integrálva van hello Batch szolgáltatás API. Ha használ a tartós tárolási másik formája, szüksége lesz toowrite hello alkalmazás logika toopersist feladat kimenetének saját kezűleg.   

* **Lekérés kimeneti**: kérheti le a feladat kimenetének közvetlenül a hello számítási csomópontok a készlethez, vagy Azure Storage vagy más adattárolóbeli tárolására, ha azt teszi lehetővé a megőrzött feladat kimenete. tooretrieve feladat kimenetének közvetlenül a számítási csomópont a hello fájl neve és helye hello csomópont a kimeneti van szükség. Ha a feladat kimenete tooAzure tárolási is fennáll, majd hello teljes elérési útja toohello fájl szükséges az Azure Storage toodownload hello kimeneti fájlok hello Azure Storage szolgáltatás SDK.

* **Kimeneti megtekintése**: tooa kötegelt feladat hello Azure portál, és válassza a kiválasztásakor **csomóponton fájlok**, lehetősége lesz hello feladathoz hozzárendelt összes fájl, nem csak a kimeneti fájlokat továbbra is érdekli hello. Ebben az esetben a számítási csomópontok fájl áll rendelkezésre, csak hello csomópont létezik-e, és csak a hello fájl megőrzési időn belül állított hello feladat során. tooview tevékenység kimeneti, hogy már megőrizte tooAzure tárolási, hello Azure-portálon vagy egy Azure Storage-ügyfél alkalmazás, például hello [Azure Tártallózó][storage_explorer]. tooview kimeneti adatok az Azure Storage hello portálon vagy egy másik eszköz, kell ismeri hello fájl helyét, és keresse meg a tooit közvetlenül.

## <a name="options-for-persisting-output"></a>Persisting kimenet beállításai

A forgatókönyvtől függően számos toopersist feladat kimenetének is tarthat néhány különböző szempontok.

- Hello Batch szolgáltatás API-t használja.  
- Használja a hello kötegelt fájl egyezmények könyvtár a .NET-hez.  
- Hello kötegelt fájl egyezmények az alkalmazás normál megvalósításához.
- Egy egyéni fájl adatátviteli szolgáltatás megvalósítása.

a következő szakaszok hello mindkét megközelítés részletesebben ismertetik.

### <a name="use-hello-batch-service-api"></a>Hello Batch szolgáltatás API-val

Verziójával 2017-05-01 hello Batch szolgáltatás támogatást biztosít az Azure Storage kimeneti fájlok feladat adatainak megadása során meg [feladat tooa feladat vehető](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) vagy [felvesznek egy gyűjteményt feladatok tooa feladat](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

hello Batch szolgáltatás API támogatja tárolásakor feladat adatok tooan hello virtuálisgép-konfiguráció létre készletek Azure Storage-fiókot. A Batch szolgáltatás API hello hello alkalmazás, amely a feladatütemezés módosítása nélkül megőrizni a feladat adatait. Opcionálisan feleljen levő toohello [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) hello fájlok elnevezési tooAzure tárolási megőrzéséhez. 

Ha a kimeneti hello Batch szolgáltatás API toopersist feladat használható:

- Azt szeretné, hogy a kötegelt és feladat manager feladatok hello virtuálisgép-konfiguráció létre készletek toopersist adatait.
- Azt szeretné, hogy toopersist adatok tooan Azure Storage-tárolóban egy tetszőleges nevet.
- Azt szeretné, hogy toopersist adatok tooan Azure Storage nevű tárolót függően toohello [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

> [!NOTE]
> hello Batch szolgáltatás API nem támogatja a hello felhőalapú szolgáltatás konfigurációja létre készletek futó feladatok adatait. Hello felhőszolgáltatások konfigurálása futtató készletek kimenetét tárolásakor feladattal kapcsolatos információkért lásd: [megőrizni a feladat- és adatok tooAzure tárolási hello kötegelt fájl egyezmények könyvtárhoz tartozó .NET toopersist](batch-task-output-file-conventions.md)
> 
> 

A Batch szolgáltatás API hello tárolásakor feladat kimenetének további információkért lásd: [megőrizni a feladat adatainak tooAzure tárolási hello Batch szolgáltatás API](batch-task-output-files.md). Lásd még: hello [PersistOutputs] [github_persistoutputs] mintát a Githubon, amely bemutatja, hogyan toouse hello kötegelt ügyféloldali kódtára a .NET toopersist tevékenység kimeneti toodurable tárolási projekt.

### <a name="use-hello-batch-file-conventions-library-for-net"></a>Hello kötegelt fájl egyezmények könyvtár használata a .NET-hez

A fejlesztők kötegelt megoldások a C# és .NET felépítése a hello [fájl egyezmények .NET-keretrendszerhez készült] [ nuget_package] toopersist feladat adatok tooan Azure Storage-fiók esetén toohello szerint [kötegfájlt futtat Standard egyezmények](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). hello fájl egyezmények könyvtár a mozgóátlag kimeneti fájlok tooAzure tárolási és elnevezési cél tárolók és blobok egy jól ismert módon kezeli.

hello fájl egyezmények könyvtár támogatja a kimeneti fájlok vagy az azonosító, vagy a cél lekérdezése, így könnyen toolocate őket anélkül, hogy hello befejezéséhez fájl URI-azonosítók. 

Hello kötegelt fájl egyezmények library .NET toopersist feladatot, kimeneti amikor használhatja:

- Érdemes toostream adatok tooAzure tárolási hello feladat futása közben.
- Azt szeretné, hogy a készletek hello felhőalapú szolgáltatás konfigurációja vagy a virtuálisgép-konfiguráció hello toopersist adatait.
- Az ügyfélalkalmazás vagy más feladatok hello igények toolocate feladat, és töltse le a tevékenység kimeneti fájlok azonosító vagy célja. 
- Azt szeretné, hogy az eredmények ellenőrzés mutató vagy korai feltöltése tooperform.
- Azt szeretné, hogy a feladat kimenetének tooview hello Azure-portálon.

A .NET-hez lévő objektumán feladat kimenetének hello fájl egyezmények könyvtárhoz további információkért lásd: [megőrizni a feladat- és adatok tooAzure tárolási hello kötegelt fájl egyezmények könyvtárhoz tartozó .NET toopersist ](batch-task-output-file-conventions.md). Lásd még: hello [PersistOutputs] [github_persistoutputs] mintát a Githubon, amely bemutatja, hogyan toouse hello fájl egyezmények library .NET toopersist tevékenység kimeneti toodurable tárolási projekt.

hello [PersistOutputs] [github_persistoutputs] mintaprojektet a Githubon bemutatja, hogyan toouse hello kötegelt ügyféloldali kódtára a .NET toopersist tevékenység kimeneti toodurable tároló.

### <a name="implement-hello-batch-file-conventions-standard"></a>Hello szabványos kötegelt fájl egyezmények végrehajtása

.NET helyett más nyelvre használatakor hello Megvalósíthat [kötegelt fájl egyezmények standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) a saját alkalmazásban. 

Érdemes lehet tooimplement hello fájl egyezmények elnevezési szabványnak saját kezűleg Ha azt szeretné, hogy egy megbízható elnevezési sémát, vagy ha azt szeretné, hogy a feladat kimenetének tooview hello Azure-portálon.

### <a name="implement-a-custom-file-movement-solution"></a>Egy egyéni fájl adatátviteli megoldás megvalósítása

A saját teljes fájl adatátviteli megoldás is megvalósíthatja. Ez készíthető elő, amikor használja:

- Azt szeretné, hogy toopersist feladat tooa adatok adattár Azure Storage eltérő. tooupload fájlok tooa adattár Azure SQL- vagy Azure DataLake például, létrehozhat egy egyéni parancsfájl vagy végrehajtható tooupload toothat helyet. Majd hívása azt hello parancssorban az elsődleges végrehajtható fájl futtatása után. Például egy Windows-csomóponton, előfordulhat, hogy meghívja a két parancsokkal:`doMyWork.exe && uploadMyFilesToSql.exe`
- Azt szeretné, hogy az eredmények ellenőrzés mutató vagy korai feltöltése tooperform.
- Azt szeretné, hogy a hibakezelés toomaintain részletes szabályozását. Például érdemes lehet tooimplement saját megoldás, ha azt szeretné, toouse feladat függőségi műveletek tootake bizonyos töltse fel a megadott kilépési kódot alapuló intézkedések kezdeményezésére. A feladatütemezés-függőség műveletek további információkért lásd: [létrehozása feladat függőségek toorun feladatok, más feladatok függő](batch-task-dependencies.md). 

## <a name="next-steps"></a>Következő lépések

- Megismerkedhet a Batch szolgáltatás API hello toopersist feladat adatai hello az új funkciók használatához [megőrizni a feladat adatainak tooAzure tárolási hello Batch szolgáltatás API](batch-task-output-files.md).
- Hello kötegelt fájl egyezmények könyvtár használata a .NET-keretrendszerhez készült [megőrizni a feladat- és adatok tooAzure tárolási hello kötegelt fájl egyezmények könyvtárhoz tartozó .NET toopersist ](batch-task-output-file-conventions.md).
- Lásd: hello [PersistOutputs] [github_persistoutputs] mintaprojektet a Githubon, amely bemutatja, hogyan toouse mindkét hello kötegelt ügyféloldali kódtára a .NET és hello fájl egyezmények library .NET toopersist tevékenység kimeneti toodurable tároló.

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/
