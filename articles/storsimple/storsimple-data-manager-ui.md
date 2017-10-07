---
title: "Azure StorSimple adatokat kezelő felhasználói felületén aaaMicrosoft |} Microsoft Docs"
description: "Ismerteti, hogyan toouse StorSimple adatok Manager szolgáltatással felhasználói felület (magán előnézetben)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: b0ee12b3e495400b54e48eb1a98c68b1af2e5f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-using-hello-storsimple-data-manager-service-ui-private-preview"></a>Kezelheti a hello StorSimple adatkezelő szolgáltatás felhasználói felületi (magán előnézetben)

Ez a cikk ismerteti, hogyan használhatja hello StorSimple adatokat kezelő felhasználói felületén tooperform adatok átalakítása hello StorSimple 8000 sorozat eszközeire levő adatok. hello átalakított adatok lehet majd használni például az Azure Media Services, az Azure HDInsight, az Azure Machine Learning és az Azure Search más Azure-szolgáltatásokkal. 


## <a name="use-storsimple-data-transformation"></a>StorSimple adatok átalakítással

StorSimple adatkezelő hello Data Transformation példányosítható hello erőforrás. hello Data Transformation szolgáltatás lehetővé teszi a tárolt adatok mozgatása az Azure storage a StorSimple a helyszíni eszközök tooblobs. Emiatt munkafolyamat kell toospecify hello részletek a StorSimple eszköz és hello adatok részleteit, amelyet az toomove toohello tárfiók iránt.

### <a name="create-a-storsimple-data-manager-service"></a>StorSimple adatkezelő szolgáltatás létrehozása

Hajtsa végre a következő lépéseket toocreate StorSimple adatkezelő szolgáltatás hello.

1. túl nyissa meg a StorSimple adatkezelő szolgáltatás toocreate[https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)

2. Kattintson a hello  **+**  ikonra, és keresse a StorSimple Data Manager. Kattintson a StorSimple adatok Manager szolgáltatást, majd **létrehozása**.

3. Az előfizetés engedélyezve van ez a szolgáltatás létrehozása, ha a következő panel hello láthatja.

    ![StorSimple adatok kezelők erőforrás létrehozása](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. Megadja a hello bemeneti adatokat, majd **létrehozása**. a megadott hello egyet, amely a storage-fiókok és a StorSimple Manager szolgáltatás hello kell lennie. Jelenleg csak az USA nyugati régiója és Nyugat-Európában régiók támogatottak. Emiatt a StorSimple Manager szolgáltatás, kezelő szolgáltatás és hello kapcsolódó tárfiók kell lenniük a fenti hello támogatott régióban. A percben toocreate hello szolgáltatás vesz igénybe.

### <a name="create-a-data-transformation-job-definition"></a>A data transformation feladatdefiníció létrehozása

A StorSimple adatkezelő szolgáltatáson belül kell toocreate a data transformation feladat definíciójához. A feladatdefiníció érdekli helyezi át a tárfiók hello natív formátumban hello adatok részleteit. 

Hajtsa végre a következő lépéseket toocreate egy új data transformation feladatdefiníció hello.

1.  Keresse meg a létrehozott toohello szolgáltatásra. Kattintson a **+ Definition feladat**.

    ![Kattintson a kívánt feladat definíciója](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. hello új feladat definition panel is megnyílik. Nevezze el a feladat definíciójához, és kattintson a **forrás**. A hello **konfigurálása adatforrás** panelen adja meg a StorSimple eszköz hello adatait, és hello érdeklő adatok.

    ![Feladat-definíció létrehozása](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. Mivel ez egy új adatokat kezelő szolgáltatás, nem adattárolók vannak konfigurálva. tooadd adatok tára, mint a StorSimple Manager kattintson **új hozzáadása** a hello adatok tárház legördülő menüből, majd **adattárház hozzáadása**.

4. Válasszon **StorSimple 8000 series Manager** hello tárház, írja be, és írja be a hello tulajdonságainak a **StorSimple Manager**. A hello **erőforrás-azonosító** mezőjét, előtt hello tooenter hello számot kell **:** hello regisztrációs kulcs a StorSimple Manager.

    ![Adatforrás létrehozása](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  Kattintson a **OK** végzett. Ez az adattárház menti, és a StorSimple Manager ezek a paraméterek ismételt beírása nélkül is újrahasznosíthatók lévő más definíciók. Kattintás után néhány másodpercet vesz igénybe **OK** a StorSimple Manager tooshow mentése hello legördülő hello.

6.  A hello **konfigurálása adatforrás** panelen hello eszköz nevét adja meg, és a fontos adatokat tartalmazó kötet neve hello.

7.  A hello **szűrő** alszakasz, adja meg az egyik fontos adatokat tartalmazó hello gyökérkönyvtár (ebben a mezőben kell kezdődnie, egy `\`). Bármely fájlszűrők is hozzáadhat.

8.  hello data transformation szolgáltatás hello adatokon keresztül pillanatképek be toohello Azure fejlesztőre működik. Ez a feladat futtatásakor választhat tootake biztonsági másolatot minden alkalommal, amikor a feladat fut (a legújabb adatokkal toowork) vagy toouse hello hello felhőben utolsó meglévő biztonsági mentés (ha az egyes archivált adatokat).

    ![Új adatforrás részletei](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. A következő hello tárolóbeállítások kell toobe konfigurálva. Támogatott célok – Azure Storage-fiókok és az Azure Media Services-fiókok 2 típusa van. Válassza ki tárfiókok tooput fájlok a fiókhoz tartozó blobokat. Tooput fájlok az media services-fiók kiválasztása a fiókhoz tartozó eszközök. Ebben az esetben kell tooadd tára. Hello legördülő menüben válassza ki **új hozzáadása** , majd **beállításainak**.

    ![Adatokat fogadó létrehozása](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. Itt kiválaszthatja kívánt tooadd és hello más paramétereket hello tárház társított tárház hello típusa. Mindkét esetben a tároló várólista létrejön hello feladat futtatásakor. A sorba az átalakított blobokkal kapcsolatos üzenetek kerülnek, amint a blobok elkészültek. a várólista nevét hello van hello ugyanaz, mint a hello feladatdefiníció hello nevét. Ha **Media Services** hello tárház típusa, majd is megadhat tárfiók hitelesítő adatainak ahol hello várólista létrejön.

    ![Új adatokat fogadó részletei](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. A felvett hello adattárház (amely néhány másodpercet vesz igénybe), található hello tárház hello hello legördülő **célfiók neve**.  Válassza ki a hello cél, amelyekre szüksége van.

12. Kattintson a **OK** toocreate hello feladat definíciójához. A feladat definíciójához most már be van állítva. A feladat definíciójához hello felhasználói felületén keresztül többször is használhatja.

    ![Új projekt hozzáadása](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-hello-job-definition"></a>Hello feladatdefiníció futtatása

StorSimple toohello tárfiók a hello feladatdefinícióban megadott toomove adatait van szüksége, szüksége lesz a tooinvoke azt. Nincs rugalmas hello paraméterek módosítása, minden alkalommal, amikor hello feladatot indít. hello lépései a következők:

1. Válassza ki a StorSimple adatkezelő szolgáltatást, és nyissa meg túl**figyelés**. Kattintson a **futtatása most**.

    ![Eseményindító feladatdefiníció](./media/storsimple-data-manager-ui/run-now.png)

2. Válassza ki a hello feladatdefiníció, amelyet az toorun. Kattintson a **futtatási beállítások** toomodify toochange érdemes futtatni a feladatot a beállításokat.

    ![Feladatbeállítások futtatása](./media/storsimple-data-manager-ui/run-settings.png)

3. Kattintson a **OK** majd **futtatása** toolaunch a feladatot. toomonitor ezt a feladatot, nyissa meg toohello **feladatok** a StorSimple adatkezelő lap.

    ![Feladatok listája és állapota](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. A hozzáadása toomonitoring a hello **feladatok** panelen, akkor is figyelheti a hello storage üzenetsorába, ahol egy üzenet jelenik meg minden alkalommal, amikor egy fájl átkerül a StorSimple toohello tárfiók.


## <a name="next-steps"></a>Következő lépések

[.NET SDK toolaunch StorSimple adatkezelő feladatok](storsimple-data-manager-dotnet-jobs.md).
