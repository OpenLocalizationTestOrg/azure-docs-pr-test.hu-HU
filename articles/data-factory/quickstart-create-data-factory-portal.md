---
title: "Azure-beli adat-előállító létrehozása az Azure Portal használatával | Microsoft Docs"
description: "Létrehozhat egy Azure-beli adat-előállítót az adatok egy felhőalapú adattárból (Azure Blob Storage) egy másik felhőalapú adattárba (Azure SQL Database) való másolásához."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 09/19/2017
ms.author: jingwang
ms.openlocfilehash: b884d7f08311cc60dc3af500f552d525d23b91e6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-data-factory-using-the-azure-portal"></a>Adat-előállító létrehozása az Azure Portal használatával
Az Azure Data Factory egy felhőalapú adatintegrációs szolgáltatás. Lehetővé teszi olyan, a felhőben futó, adatvezérelt munkafolyamatok létrehozását, amelyek alkalmasak az adatok átvitelének és átalakításának irányítására és automatizálására. Az Azure Data Factory segítségével létrehozhatók és ütemezhetők a különböző adattárolókból adatokat beolvasó adatvezérelt munkafolyamatok, feldolgozhatók és átalakíthatók az adatok különböző számítási szolgáltatások használatával (pl. Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics és Azure Machine Learning), és a kimeneti adatok közzétehetők olyan adattárakban, mint például az Azure SQL Data Warehouse, ahonnan az üzleti intelligenciára épülő (BI-) alkalmazások felhasználhatják őket. 

Ebben a rövid útmutatóban egy adat-előállítót hoz létre az Azure Portal használatával. Miután létrehozta az adat-előállítót, a többi rövid útmutatóhoz hasonlóan a PowerShellt, a .NET SDK-t, a Python SDK-t vagy a REST API-t kell használnia adatfolyamatként, hogy adatokat másoljon a forrásadattárból a céladattárba. Jelenleg nem hozhatók létre folyamatok az adat-előállítóban az Azure Portal használatával.

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

## <a name="log-in-to-the-azure-portal"></a>Jelentkezzen be az Azure portálra.
Jelentkezzen be az [Azure portálra](https://portal.azure.com/).

## <a name="create-a-data-factory"></a>Data factory létrehozása
A rövid útmutató során a következő lépéseket fogja elvégezni:
1. Kattintson az **Új** elemre, majd az **Adatok + analitika**, végül a **Data Factory** elemre. 
   
   ![New (Új)->DataFactory](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
2. Az **Új data factory** panelen írja be az **ADFTutorialDataFactory** kifejezést a **név** mezőbe. 
      
     ![A New data factory (Új data factory) panel](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   Az Azure data factory nevének **globálisan egyedinek** kell lennie. Ha a következő hibaüzenetet kapja, módosítsa a data factory nevét (például sajátnévADFTutorialDataFactory-ra), majd próbálkozzon újra a létrehozással. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.
  
       `Data factory name “ADFTutorialDataFactory” is not available`
3. Válassza ki azt az **Azure-előfizetést**, amelyben az adat-előállítót létre szeretné hozni. 
4. Az **erőforráscsoportban** hajtsa végre a következő lépések egyikét:
     
      - Kattintson a **Meglévő használata** elemre, majd a legördülő listából válasszon egy meglévő erőforráscsoportot. 
      - Kattintson az **Új létrehozása** elemre, és adja meg az erőforráscsoport nevét.   
         
      A rövid útmutató egyes lépései azt feltételezik, hogy az **ADFTutorialResourceGroup** nevet adta az erőforráscsoportnak. Az erőforráscsoportokkal kapcsolatos információkért tekintse meg a [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Erőforráscsoportok használata az Azure-erőforrások kezeléséhez) című cikket.  
4. Válassza a **V2 (előzetes verzió)** értéket a **verzió** esetén.
5. Válassza ki a Data Factory **helyét**. Jelenleg csak az **USA keleti régiójában** hozhatók létre V2 verziójú adat-előállítók. Az adat-előállításhoz használt számítási és adattárak azonban lehetnek más régióban is. 
6. Válassza a **Rögzítés az irányítópulton** lehetőséget.     
7. Kattintson a **Létrehozás** gombra.
      
      > [!IMPORTANT]
      > Data Factory-példány létrehozásához a [Data Factory közreműködője](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör tagjának kell lennie az előfizetés/erőforráscsoport szintjén.
      > 
      > A data factory neve később DNS-névként regisztrálható, így nyilvánosan láthatóvá válhat.             
3. Az irányítópulton megjelenő csempén a következő állapotleírás látható: **Adat-előállító üzembe helyezése**. 

    ![adat-előállító üzembe helyezése csempe](media//quickstart-create-data-factory-portal/deploying-data-factory.png)
1. A létrehozás befejezése után a **Data Factory** panel a képen látható módon jelenik meg.
   
   ![Data factory kezdőlap](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)


## <a name="next-steps"></a>Következő lépések
A példában szereplő folyamat adatokat másol az egyik helyről egy másikra egy Azure Blob Storage-ban. A Data Factory más forgatókönyvekben való használatát ismertető további információkért tekintse meg az [oktatóanyagokat](tutorial-copy-data-dot-net.md). 