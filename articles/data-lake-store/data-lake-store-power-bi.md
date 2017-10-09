---
title: "a Power BI használatával a Data Lake Store aaaAnalyze adatok |} Microsoft Docs"
description: "A Power BI tooanalyze adataihoz az Azure Data Lake Store használatára"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 6a1bfa80fd1b0dda59b7eaaae9ca1585ba42783e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>A Data Lake Store adatok elemzése a Power BI használatával
Ebben a cikkben megtudhatja, hogyan toouse Power BI Desktop tooanalyze és az Azure Data Lake Store-ban tárolt adatok megjelenítése.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Store-fiók**. Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md). Ez a cikk feltételezi, hogy már létrejött egy Data Lake Store-fiókot, úgynevezett **mybidatalakestore**, és fel kell tölteni egy minta adatfájl (**Drivers.txt**) tooit. Ezt a mintafájlt letölthető [Azure Data Lake Git-tárház](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).
* **A Power BI Desktopban**. Letöltheti ezt a [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 

## <a name="create-a-report-in-power-bi-desktop"></a>Hozzon létre egy jelentést a Power BI Desktop
1. Indítsa el a Power BI Desktop a számítógépen.
2. A hello **Home** menüszalag, kattintson a **adatok beolvasása**, és kattintson a több. A hello **adatok beolvasása** párbeszédpanel, kattintson a **Azure**, kattintson a **Azure Data Lake Store**, és kattintson a **Connect**.
   
    ![Csatlakozás tooData Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Connect tooData Lake Store")
3. Ha megjelenik egy párbeszédpanel fázisban vannak fejlesztés alatt álló hello connector névjegye, használatának toocontinue.
4. A hello **Microsoft Azure Data Lake Store** párbeszédpanelen adjon meg hello URL-cím tooyour Data Lake Store-fiókot, és kattintson **OK**.
   
    ![A Data Lake Store URL-cím](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "a Data Lake Store URL-címe")
5. Hello következő párbeszédpanelen, kattintson a **bejelentkezés** toosign be Data Lake Store-fiókba. Fogja átirányítani tooyour szervezete bejelentkezési oldalára. Hajtsa végre a hello kér toosign hello figyelembe.
   
    ![Jelentkezzen be a Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "jelentkezzen be a Data Lake Store")
6. Miután sikeresen bejelentkezett, kattintson **Connect**.
   
    ![Csatlakozás tooData Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Connect tooData Lake Store")
7. hello tovább párbeszédpanel hello fájl jeleníti meg, hogy a feltöltött tooyour Data Lake Store-fiók. Ellenőrizze a hello adatait, majd kattintson **terhelés**.
   
    ![Adatok betöltése az Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "adatok betöltése a Data Lake Store-ból")
8. Miután hello adatok sikeresen betöltve a Power bi-ba, látni fogja a következő hello mezőinek hello **mezők** fülre.
   
    ![Importált mezők](./media/data-lake-store-power-bi/imported-fields.png "importált mezők")
   
    Azonban toovisualize és hello adatok elemzésére, azt inkább hello adatok toobe, amely a következő mezők hello száma
   
    ![Szükségeskonfiguráció-mezők](./media/data-lake-store-power-bi/desired-fields.png "szükséges mezők")
   
    A következő lépések hello frissítjük hello lekérdezés tooconvert hello importált adatok hello kívánt formátumban.
9. A hello **Home** menüszalag, kattintson a **szerkesztése lekérdezések**.
   
    ![Szerkessze a lekérdezések](./media/data-lake-store-power-bi/edit-queries.png "lekérdezések szerkesztése")
10. A Lekérdezésszerkesztő, hello alatt hello **tartalom** oszlopban kattintson **bináris**.
    
    ![Szerkessze a lekérdezések](./media/data-lake-store-power-bi/convert-query1.png "lekérdezések szerkesztése")
11. A fájl ikon, hello jelölő **Drivers.txt** feltöltött fájl. Kattintson a jobb gombbal a hello fájlt, és kattintson a **CSV**.    
    
    ![Szerkessze a lekérdezések](./media/data-lake-store-power-bi/convert-query2.png "lekérdezések szerkesztése")
12. Alább látható módon kimenetnek kell megjelennie. Az adatok már használható toocreate képi megjelenítések formátumban elérhető.
    
    ![Szerkessze a lekérdezések](./media/data-lake-store-power-bi/convert-query3.png "lekérdezések szerkesztése")
13. A hello **Home** menüszalag, kattintson a **zárja be, és alkalmazni**, és kattintson a **zárja be, és alkalmazni**.
    
    ![Szerkessze a lekérdezések](./media/data-lake-store-power-bi/load-edited-query.png "lekérdezések szerkesztése")
14. Miután hello lekérdezés frissül, hello **mezők** látható hello elérhető a képi megjelenítéshez tartozó új mezők.
    
    ![Mezők frissítése](./media/data-lake-store-power-bi/updated-query-fields.png "mezők frissítése")
15. Ossza meg velünk hozzon létre egy kördiagram toorepresent hello illesztőprogramok egy adott országban minden város. toodo, ügyeljen a következő beállításokat hello.
    
    1. Hello képi megjelenítések lapján kattintson a kördiagram hello szimbólum.
       
        ![A tortadiagram létrehozása](./media/data-lake-store-power-bi/create-pie-chart.png "kördiagram létrehozása")
    2. hello oszlopok fogjuk toouse vannak **oszlop 4** (hello város neve) és **oszlop 7** (hello ország neve). Ezekben az oszlopokban a húzza **mezők** túl lapon**képi megjelenítések** lapon a lent látható módon.
       
        ![Képi megjelenítéseket készíthet](./media/data-lake-store-power-bi/create-visualizations.png "képi megjelenítéseket készíthet")
    3. hello kördiagram például hello alábbihoz kell hasonlítania.
       
        ![A tortadiagram](./media/data-lake-store-power-bi/pie-chart.png "képi megjelenítéseket készíthet")
16. Egy adott országban hello szintű Lapszűrők való kiválasztással most már megtekintheti a hello illesztőprogramok száma az egyes kiválasztott hello ország város. Például az hello **képi megjelenítések** lap **szintű szűrők lapon**, jelölje be **brazíliai**.
    
    ![Válassza ki a ország](./media/data-lake-store-power-bi/select-country.png "ország kiválasztása")
17. hello kördiagram automatikusan frissített toodisplay hello illesztőprogramja hello városokat Brazília.
    
    ![Illesztőprogramok országban](./media/data-lake-store-power-bi/driver-per-country.png "országonkénti illesztőprogramok")
18. A hello **fájl** menüben kattintson a **mentése** toosave hello képi megjelenítés Power BI Desktop-fájlként.

## <a name="publish-report-toopower-bi-service"></a>A jelentés tooPower BI-ban közzététele
Ha a Power BI Desktopban létrehozott hello képi megjelenítéseket, megoszthatja azt másokkal történő közzétételével toohello Power BI-ban. Útmutatást, lásd: toodo [a Power BI Desktop közzététele](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Lásd még:
* [Adatok elemzése a Data Lake Analytics használatával a Data Lake Store az](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

