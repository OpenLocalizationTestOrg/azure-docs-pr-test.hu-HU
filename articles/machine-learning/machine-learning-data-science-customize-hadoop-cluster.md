---
title: "aaaCustomize Hadoop-fürtök a csapat az tudományos folyamata hello |} Microsoft Docs"
description: "Népszerű Python-modulok egyéni Azure HDInsight Hadoop-fürtök elérhetik."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: e192542dd39f71bccbb5163382b4050d0f12ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-hello-team-data-science-process"></a>Hello csapat az tudományos folyamata az Azure HDInsight Hadoop-fürtök testreszabása
Ez a cikk ismerteti hogyan toocustomize egy HDInsight Hadoop cluster (Python 2.7) 64 bites Anaconda telepítésével minden egyes csomóponton hello fürt létesítése HDInsight szolgáltatásként. Azt is bemutatja, hogyan tooaccess hello headnode toosubmit egyéni feladatok toohello fürt. Kényelmesen használható felhasználói meghatározott funkciókat (UDF), amelyek tervezték tooprocess Hive rekordok hello fürtben, ez a beállítás lehetővé teszi számos népszerű Python modult, Anaconda található. Ebben a forgatókönyvben használt hello eljárásokat, lásd: [hogyan toosubmit Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit).

hello következő menü hivatkozik, amely ismerteti, hogyan tooset hello be különböző adatok tudományos környezetekben által használt hello tootopics [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

## <a name="customize"></a>Az Azure HDInsight Hadoop-fürt testreszabása
toocreate testreszabott HDInsight Hadoop-fürt első lépésként túl bejelentkezés[**a klasszikus Azure portálon**](https://manage.windowsazure.com/), kattintson a **új** : hello bal alsó sarokban, és válassza a DATA SERVICES HDINSIGHT -> -> **egyéni létrehozás** mentése hello toobring **a fürt részleteinek** ablak. 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Adjon meg az 1-konfiguráció lapon létre hello fürt toobe hello nevét, és fogadja el a hello tartozó alapértelmezett értékeket más mezők. Kattintson a hello nyíl toogo toohello következő konfiguráció lapon. 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

A konfiguráció lapon 2 bemeneti hello száma **ADATCSOMÓPONTOKAT**, jelölje be hello **régió/virtuális hálózati**, és válassza ki a hello hello méretű **ÁTJÁRÓCSOMÓPONT** és hello **ADATCSOMÓPONTON**. Kattintson a hello nyíl toogo toohello következő konfiguráció lapon.

> [!NOTE]
> Hello **régió/virtuális hálózati** hello ugyanaz, mint hello terület lesz használatos a HDInsight Hadoop-fürt hello toobe hello tárfiók toobe rendelkezik. Ellenkező esetben hello negyedik konfigurálása lapon hello tárfiók nem jelennek meg hello legördülő listája **fióknév**.
> 
> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

A konfiguráció lapon 3 adja meg egy felhasználónevet és jelszót hello HDInsight Hadoop-fürt. **Ne** válassza hello *Enter hello Hive/Oozie Metaadattárhoz*. Kattintson a hello nyíl toogo toohello következő konfiguráció lapon. 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Konfiguráció lapon adja meg, 4 hello tárfióknév, hello alapértelmezett tároló hello HDInsight Hadoop-fürt. Ha *hozzon létre alapértelmezett tároló* a hello **alapértelmezett tároló** legördülő listából, a tárolóhoz hello azonos nevet hello fürt jön létre. Kattintson a hello nyíl toogo toohello utolsó konfiguráció lapon.

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

A végső hello **Parancsfájlműveletek** konfigurációs lapon kattintson a **parancsfájlművelet hozzáadása** gombra, és beírja a következő értékek hello hello szövegmezők.

* **NÉV** -bármilyen karakterlánc hello neveként a parancsfájl művelet
* **A CSOMÓPONTTÍPUS** – Itt adhatja meg **minden csomópont**
* **PARANCSFÁJL URI azonosítója** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
  * *publicscripts* hello tárfiókban lévő nyilvános tárolója 
  * *getgoing* tooshare PowerShell parancsfájl fájlok toofacilitate felhasználók munkahelyi használjuk az Azure-ban
* **PARAMÉTEREK** -(hagyja üresen)

Végezetül kattintson hello pipa toostart hello létrehozása testre szabott hello HDInsight Hadoop-fürt. 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Hozzáférés hello Head csomópont a Hadoop-fürt
Távelérés toohello Hadoop-fürt az Azure-ban engedélyeznie kell, csak hello hello Hadoop-fürt átjárócsomópontjához RDP érheti el. 

1. Jelentkezzen be toohello [ **a klasszikus Azure portálon**](https://manage.windowsazure.com/), jelölje be **HDInsight** hello bal oldalon válassza ki a Hadoop-fürt a hello fürtök, hello kattintson  **KONFIGURÁCIÓS** fülre, majd hello **távoli engedélyezése** alján hello hello ikonra.
   
    ![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)
2. A hello **konfigurálása a távoli asztal** ablakban hello felhasználónév és jelszó mezők megadása, valamint távelérés hello lejárati dátuma. Kattintson a hello pipa tooenable hello távelérési toohello átjárócsomópontjához hello Hadoop-fürt.
   
    ![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> hello felhasználónevet és jelszót hello táveléréshez nincsenek hello felhasználónevét és jelszavát, hello Hadoop-fürt létrehozásakor használt. Ez a hitelesítő adatok külön készletét. Hello lejárati dátuma hello távelérési rendszer is toobe aktuális dátum hello számított 7 napon belül.
> 
> 

Távelérés engedélyezése után kattintson **CONNECT** hello lap tooremote hello központi csomópontjába hello alján. Hello hitelesítő adatokat adjon meg a korábban megadott hello távelérési felhasználói bejelentkezés toohello hello Hadoop-fürt átjárócsomópontjához.

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

hello hello advanced analytics folyamat a következő lépések vannak leképezve a hello [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , és tartalmazhatnak lépéseket, amely adatok áthelyezi HDInsight, majd feldolgozást és minták ott előkészítésekor hello adatok alapján az Azure Machine Learning segítségével.

Lásd: [hogyan toosubmit Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit) kapcsolatban, hogyan tooaccess hello hello felhasználói függvény (UDF), amelyek használt tooprocess hello fürt átjárócsomópontjához Anaconda szerepel Python-modulok Hive tárolt rekordok hello fürtben.

