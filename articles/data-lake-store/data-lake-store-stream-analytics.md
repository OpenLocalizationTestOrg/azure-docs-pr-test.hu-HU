---
title: a Data Lake Store Stream Analytics aaaStream adatait |} Microsoft Docs
description: "Azure Data Lake Store az Azure Stream Analytics toostream adatok használata"
services: data-lake-store,stream-analytics
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 68c727d4807db0abe6efa90145d68c78902eb789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Adatok streamelése az Azure Storage-blobokból a Data Lake Store-ba az Azure Stream Analytics használatával
Ebben a cikkben megtudhatja, hogyan toouse az Azure Data Lake tárolót, mint az Azure Stream Analytics-feladat kimenettel. Ez a cikk bemutatja egy olyan egyszerű forgatókönyvet, amely egy Azure Storage-blobba (bemeneti) olvassa be az adatokat, és írási műveletek hello tooData Lake adattárban (kimenet).

> [!NOTE]
> Most, létrehozását és a Data Lake Store konfigurációs kimenetként Stream Analytics csak hello támogatott [klasszikus Azure portál](https://manage.windowsazure.com). Ezért ez az oktatóanyag egyes részei hello klasszikus Azure portál fogja használni.
>
>

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Storage-fiók** A Stream Analytics-feladat szüksége lesz egy blob-tároló, a fiók tooinput adatokból. Az oktatóanyag során feltételezzük, hogy a tárfiók neve **storageforasa** és a tároló hello fiókon belül nevű **storageforasacontainer**. Miután létrehozta a hello tároló, töltse fel egy minta adatok fájl tooit. 
  
* **Azure Data Lake Store-fiók**. Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md). Tegyük fel, egy Data Lake Store-fiók neve van **asadatalakestore**. 

## <a name="create-a-stream-analytics-job"></a>A Stream Analytics-feladat létrehozása
Indítsa el a Stream Analytics-feladat, amely tartalmaz egy bemeneti forrás- és egy kimeneti létrehozásával. Ebben az oktatóanyagban hello forrás egy Azure blob-tároló és hello célobjektuma Data Lake Store.

1. Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).

2. Hello bal oldali ablaktáblában kattintson **Stream Analytics-feladatok**, és kattintson a **Hozzáadás**.

    ![A Stream Analytics-feladat létrehozása](./media/data-lake-store-stream-analytics/create.job.png "a Stream Analytics-feladat létrehozása")

    > [!NOTE]
    > Győződjön meg arról, hogy hello feladatot hoz létre és hello tárfiók vagy ugyanabban a régióban merülnek fel, amelyekre az adatok régiók közötti áthelyezése további költség nélkül.
    >

## <a name="create-a-blob-input-for-hello-job"></a>Egy Blob bemeneti hello feladat létrehozása

1. Nyissa meg hello hello Stream Analytics-feladat hello bal oldali ablaktáblán kattintson hello **bemenetek** fülre, majd **hozzáadása**.

    ![Egy bemeneti tooyour feladat hozzáadása](./media/data-lake-store-stream-analytics/create.input.1.png "egy bemeneti tooyour feladat hozzáadása")

2. A hello **új bemeneti** panelen adja meg a következő értékek hello.

    ![Egy bemeneti tooyour feladat hozzáadása](./media/data-lake-store-stream-analytics/create.input.2.png "egy bemeneti tooyour feladat hozzáadása")

    * A **bemeneti alias**, adjon meg egy egyedi nevet hello feladat bemenet.
    * A **adatforrástípust**, jelölje be **adatfolyam**.
    * A **forrás**, jelölje be **Blob-tároló**.
    * A **előfizetés**, jelölje be **használja a jelenlegi előfizetés blob-tároló**.
    * A **tárfiók**, válassza ki a létrehozott tárfiók hello hello Előfeltételek részeként. 
    * A **tároló**, jelölje be a hello hello tároló kiválasztott tárfiók.
    * A **esemény szerializálási formátum**, jelölje be **CSV**.
    * A **elválasztó**, jelölje be **lapon**.
    * A **kódolás**, jelölje be **UTF-8**.

    Kattintson a **Create** (Létrehozás) gombra. hello portal most hello bemeneti hozzáadja, és ellenőrzi hello kapcsolat tooit.


## <a name="create-a-data-lake-store-output-for-hello-job"></a>A Data Lake Store kimeneti hello feladat létrehozása

1. Nyissa meg a Stream Analytics-feladat hello hello lap, kattintson a hello **kimenetek** fülre, majd **Hozzáadás**.

    ![Adja hozzá egy kimeneti tooyour feladat](./media/data-lake-store-stream-analytics/create.output.1.png "egy kimeneti tooyour feladat hozzáadása")

2. A hello **új kimeneti** panelen adja meg a következő értékek hello.

    ![Adja hozzá egy kimeneti tooyour feladat](./media/data-lake-store-stream-analytics/create.output.2.png "egy kimeneti tooyour feladat hozzáadása")

    * A **kimeneti alias**, adjon meg egy egyedi nevet az hello feladatkiemenetét. Ez az egy rövid nevet használt a lekérdezések toodirect hello lekérdezés kimeneti toothis Data Lake Store.
    * A **gyűjtése**, jelölje be **Data Lake Store**.
    * Fel a kért tooauthorize tooData Lake Store-fiók eléréséhez. Kattintson a **engedélyezik**.

3. A hello **új kimeneti** panelen a következő értékek tooprovide hello folytatni.

    ![Adja hozzá egy kimeneti tooyour feladat](./media/data-lake-store-stream-analytics/create.output.3.png "egy kimeneti tooyour feladat hozzáadása")

    * A **fióknév**, válassza ki a már létrehozott hello feladat kimeneti toobe küldött, ahová hello Data Lake Store-fiókot.
    * A **elérési út előtag mintája**, adjon meg egy fájl elérési útja toowrite hello található a fájl megadott Data Lake Store-fiók.
    * A **dátumformátum**, ha egy dátum-tokent használt hello előtag elérési úton, kiválaszthatja, amelyben a fájlok vannak rendszerezve hello dátumformátum.
    * A **időformátum**, ha egy ideje-tokent használt hello előtag elérési úton, adja meg, amelyben a fájlok vannak rendszerezve hello idő formátumban.
    * A **esemény szerializálási formátum**, jelölje be **CSV**.
    * A **elválasztó**, jelölje be **lapon**.
    * A **kódolás**, jelölje be **UTF-8**.
    
    Kattintson a **Create** (Létrehozás) gombra. hello portal most hello kimeneti hozzáadja, és ellenőrzi hello kapcsolat tooit.
    
## <a name="run-hello-stream-analytics-job"></a>Hello Stream Analytics-feladat futtatása

1. a Stream Analytics-feladat toorun, kell futtatnia a lekérdezés hello **lekérdezés** fülre. Ebben az oktatóanyagban futtathatja hello mintalekérdezés cseréje hello hello helyőrzőt bemeneti és kimeneti aliasok, feladat, ahogy az alábbi hello képernyőfelvétel-készítés.

    ![Lekérdezés](./media/data-lake-store-stream-analytics/run.query.png "lekérdezés futtatása")

2. Kattintson a **mentése** hello felső részén üdvözlő képernyőt, majd a hello **áttekintése** lapra, majd **Start**. Hello párbeszédpanelen jelölje ki **egyéni idő**, és utána állítsa be az aktuális dátum és idő hello.

    ![Feladat idő beállítása](./media/data-lake-store-stream-analytics/run.query.2.png "feladat idő beállítása")

    Kattintson a **Start** toostart hello feladat. Tooa néhány perc toostart hello feladatot is eltarthat.

3. tootrigger hello toopick hello feladatadatok hello blobból, másolja egy minta adatok fájl toohello blob tároló. Egy minta adatfájl letölthető hello [Azure Data Lake Git-tárház](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Ebben az oktatóanyagban most másolás hello **vehicle1_09142014.csv**. Használhatja például a különböző ügyfelek [Azure Tártallózó](http://storageexplorer.com/), tooupload adatok tooa blob tároló.

4. A hello **áttekintése** lap **figyelés**, lásd: hello adatok feldolgozásának módja.

    ![A figyelő feladat](./media/data-lake-store-stream-analytics/run.query.3.png "figyelő feladat")

5. Végül ellenőrizheti, hogy hello feladat kimeneti adatok hello Data Lake Store-fiók legyen. 

    ![Ellenőrizze a kimeneti](./media/data-lake-store-stream-analytics/run.query.4.png "kimeneti ellenőrzése")

    Hello Data Explorer ablaktáblában figyelje meg, hogy hello kimeneti írásbeli tooa mappa elérési útja a megadott hello Data Lake Store beállításait kimenetre (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Lásd még:
* [Hozzon létre egy HDInsight-fürt toouse Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
