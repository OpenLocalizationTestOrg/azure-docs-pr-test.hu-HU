---
title: "Azure Data tudományos virtuális gépek IPython Notebook kiszolgálóként aaaProvision |} Microsoft Docs"
description: "Beállítva fel adatokat tudományos virtuális gép egy IPython Notebook támogatási eszközök."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 95e1fa87-794a-4d03-80a4-af4f3f3ac31e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev
ms.openlocfilehash: 7c0abcbcb822918332f76a9f16690a72b90f4b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-azure-data-science-virtual-machines-as-ipython-notebook-servers"></a>Kiépítés az Azure Data tudományos virtuális gépek IPython Notebook-kiszolgálóként
Útmutatás el, feltéve, hogy a leírása itt hogyan tooset egy Azure virtuális gép és az Azure virtuális gép az SQL szolgáltatás IPython Notebook kiszolgálóként. támogató eszközöket, például a IPython Notebook, Azure Tártallózó, és AzCopy, valamint más hasznos adatokat tudományos projektek a segédeszközök hello Windows rendszerű virtuális gép van beállítva. Az Azure Tártallózó és AzCopy, például módszereket biztosítanak az kényelmes tooAzure adattárolás tooupload a helyi számítógépen vagy toodownload azt tooyour a helyi számítógép a tárolóból. 

Ebben a menüben hivatkozik, amely ismerteti, hogyan tooset hello be különböző adatok tudományos környezetekben által használt hello tootopics [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Számos különböző Azure virtuális gépek kiépítése, és használja a felhőalapú adatokat tudományos környezet részeként toobe konfigurálva. virtuális gép mely kiadása kapcsolatos toouse függ hello típusa és adatok toobe mennyisége hello döntési modellezve a gépi tanulási és az adatok hello felhőben hello célhelyre. 

* Az ehhez a döntéshez meghozásakor hello kérdések tooconsider útmutatóért lásd: [tervezze meg a Azure Machine Learning adatok tudományos környezet](machine-learning-data-science-plan-your-environment.md). 
* Katalógus néhány speciális elemzési során esetleg felmerülő hello forgatókönyv, lásd: [forgatókönyvek hello Advanced Analytics folyamat és az Azure Machine Learning technológia](machine-learning-data-science-plan-sample-scenarios.md)

Két olyan utasításokat kell gondoskodni:

* [Állítsa be az Azure virtuális gép speciális elemzésekre IPython Notebook kiszolgálóként](machine-learning-data-science-setup-virtual-machine.md) jeleníti meg, hogyan tooprovision IPython jegyzetfüzet és egyéb eszközök Azure virtuális gép használja olyan esetekben, ahol toodo adattudomány egy formája, amelyet az Azure storage eltérő SQL használt toostore hello adatok is lehetnek.
* [Állítsa be az Azure SQL Server virtuális gép speciális elemzésekre IPython Notebook kiszolgálóként](machine-learning-data-science-setup-sql-server-virtual-machine.md) bemutatja, hogyan tooprovision IPython jegyzetfüzet és egyéb eszközöket egy Azure SQL Server virtuális gép használja olyan esetekben, ahol toodo adattudomány SQL-adatbázis használt toostore hello adatok is lehetnek.

Miután telepített és konfigurált, ezek a virtuális gépek készen áll használatra IPython Notebook kiszolgálóként hello feltárása és az adatok feldolgozására, és más feladatok elvégzéséhez szükséges, az Azure Machine Learning és hello Team adatok tudományos folyamat (TDSP) együtt. hello hello tudományos folyamata a következő lépések vannak leképezve a hello [TDSP képzési terv](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , SQL Server és a HDInsight, az adatok feldolgozásához, és minta ott hello adatokat az Azure-ral tanulva előkészítésekor váltó lépéseket is lehetnek. Gépi tanulás.

> [!NOTE]
> Azure virtuális gépeken árú **csak a valóban használt funkciókért kell fizetnie, amennyit**. nem éppen tooensure számlázva, amikor nem használja a virtuális gépet, rendelkezik toobe hello **leállítva (Deallocated)** hello állapotának [klasszikus Azure portál](http://manage.windowsazure.com/). A részletes útmutatót vagy hogyan toodeallocate, virtuális gép, lásd: [leállítási és felszabadítani a virtuális gép, ha nincsenek használatban](machine-learning-data-science-setup-virtual-machine.md#shutdown)
> 
> 

