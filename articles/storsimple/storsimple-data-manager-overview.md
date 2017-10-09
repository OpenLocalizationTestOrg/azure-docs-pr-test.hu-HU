---
title: "aaaMicrosoft Azure StorSimple adatkezelő áttekintése |} Microsoft Docs"
description: "Hello StorSimple adatok Manager szolgáltatással (magán előnézetben) áttekintése"
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
ms.openlocfilehash: 5d29f7d26be9f2b36857526bdea770d991cece6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a>StorSimple adatkezelő áttekintése (magán előnézetben)

## <a name="overview"></a>Áttekintés

A Microsoft Azure StorSimple hibrid felhőalapú tárolási megoldás, amely címek hello gyakran társított fájlmegosztások strukturálatlan adatok bonyolultságára. StorSimple felhőbeli tárhelyét használja az hello kiterjesztése a helyszíni megoldás, és automatikusan tiers adatok között hello a helyszíni és felhőalapú tárolására. Az adatvédelem integrálva van a helyi és felhőalapú pillanatfelvételek, szükségtelenné teszi hello sprawling tároló-infrastruktúra. Archiválás és katasztrófa-helyreállítás is zökkenőmentes hello a felhő egy külső helyszín működött.

hello átalakítása ADATSZOLGÁLTATÁSNÁL, amely ebben a dokumentumban vezetjük lehetővé teszi, hogy Ön tooseamlessly hozzáférés hello StorSimple adatok hello felhőben. Ez a szolgáltatás API-k tooextract adatokat biztosít a StorSimple, és azt tooother Azure bemutatja a szolgáltatások, amelyek könnyen használható formátumban. Ebben az előzetes verzióban támogatott hello formátumok a következők: Azure-blobokat és Azure Media Services eszközök. Ez a transzformáció lehetővé teszi, hogy Ön tooeasily vezetékes szolgáltatások, például az Azure Media Services, az Azure HDInsight, az Azure Machine Learning és az Azure Search toooperate adatok a StorSimple 8000 series a helyi eszközön.

Ez ábrázoló diagram elérésű magas szintű alább láthatók.

![Magas szintű diagramját](./media//storsimple-data-manager-overview/high-level-diagram.png)

Ez a dokumentum azt ismerteti, hogyan regisztrálhat a egy, a szolgáltatás private Preview verziójára. Azt is bemutatja, hogyan használhatja a StorSimple adatok és más Azure-szolgáltatásokat használó hello felhőben szolgáltatás toowrite alkalmazások.

## <a name="sign-up-for-data-manager-preview"></a>Az adatkezelő előzetes regisztráció
Hello adatkezelő szolgáltatás regisztrálása előtt tekintse át a következő előfeltételek hello.

### <a name="prerequisites"></a>Előfeltételek

A gyakorlat feltételezi, hogy rendelkezik
* Egy aktív Azure-előfizetéssel.
* hozzáférés tooa regisztrálni a StorSimple 8000 series eszköz
* az összes hello hello StorSimple 8000 series eszközhöz társított kulcsok.

### <a name="sign-up"></a>Regisztráció

StorSimple adatkezelő hello van private Preview verziójára. Hajtsa végre a következő lépéseket toosign feliratkozott egy, a szolgáltatás private Preview verziójára hello:

1.  Jelentkezzen be hello Azure-portálon: hello StorSimple adatkezelő kiterjesztésű: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager). Az Azure hitelesítő adataival toolog a használata.

2.  Kattintson a hello  **+**  ikon toocreate szolgáltatás. Kattintson a **tárolási** , majd **tekintse meg az összes** megnyíló hello panelen.

    ![Keresési StorSimple adatkezelő ikon](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. Hello StorSimple adatkezelő ikon jelenik meg.

    ![Válassza ki a StorSimple Manager ikon](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. Kattintson a StorSimple adatkezelő ikonra, majd **létrehozása**. Válassza ki a hello előfizetés tooenable hello private Preview verziójára szeretne, és kattintson a **bejelentkezés!**

    ![Bejelentkezés](./media/storsimple-data-manager-overview/sign-me-up.png)

5. Ez egy kérelem tooonboard elküldi azt. Azt is megjelenik majd, amint lehetséges. Az előfizetés engedélyezése után létrehozhat egy StorSimple adatkezelő szolgáltatás.

6. tooeasily hello StorSimple adatkezelő szolgáltatás, kattintson a hello csillagra toopin azt tooyour Kedvencek.

    ![Hozzáférés a StorSimple adatok kezelők](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a>Következő lépések

[StorSimple adatokat kezelő felhasználói felületén tootransform használja az adatok](storsimple-data-manager-ui.md).
