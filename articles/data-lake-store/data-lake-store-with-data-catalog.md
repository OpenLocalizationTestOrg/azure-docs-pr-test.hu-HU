---
title: Data Lake Store az Azure Data Catalog aaaRegister adatait |} Microsoft Docs
description: "Data Lake Store-ból adatokat regisztrálni kell az Azure Data Catalog"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3e895b42cab4ba39d288950763312a243883cbdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Data Lake Store-ból adatokat regisztrálni kell az Azure Data Catalog
Ebben a cikkben megtudhatja, hogyan toointegrate az Azure Data Lake az Azure Data Catalog toomake az adatok tárolásához felderíthető egy szervezeten belül a Data Catalog integrálásával. Az adatok katalogizálása további információkért lásd: [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md). toounderstand forgatókönyvek, amelyben használhatja a Data Catalog, lásd: [Azure Data Catalog gyakori forgatókönyvei](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Az Azure-előfizetés engedélyezése** a Data Lake Store nyilvános előzetes verziójához. Lásd az [utasításokat](data-lake-store-get-started-portal.md).
* **Azure Data Lake Store-fiók**. Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md). Ebben az oktatóanyagban ossza meg velünk hozzon létre egy Data Lake Store-fiókot nevű **datacatalogstore**.

    Hello fiók létrehozását követően töltse fel a minta adatkészlet tooit. Ebben az oktatóanyagban ossza meg velünk hello található összes hello .csv fájl feltöltéséhez **AmbulanceData** hello mappájában [Azure Data Lake Git-tárház](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Használhatja például a különböző ügyfelek [Azure Tártallózó](http://storageexplorer.com/), tooupload adatok tooa blob tároló.
* **Az Azure Data Catalog**. A szervezet már rendelkeznie kell egy Azure Data Catalog létrehozni a szervezet számára. Egy szervezet csak egy katalógus esetén engedélyezett.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Register Data Lake Store a Data Catalog forrásként

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Nyissa meg túl`https://azure.microsoft.com/services/data-catalog`, és kattintson a **Ismerkedés**.
2. Jelentkezzen be hello Azure Data Catalog-portált, majd kattintson **adatok közzététele**.

    ![Egy adatforrás regisztrálása](./media/data-lake-store-with-data-catalog/register-data-source.png "egy adatforrás regisztrálása")
3. Hello következő lapon kattintson a **alkalmazás indítása**. Ez letölti azokat a hello Alkalmazásjegyzék-fájl a számítógépen. Kattintson duplán a hello jegyzékfájl toostart hello alkalmazásra.
4. A hello kezdőlapján kattintson **bejelentkezés**, és adja meg a hitelesítő adatokat.

    ![Üdvözlőképernyő](./media/data-lake-store-with-data-catalog/welcome.screen.png "üdvözlőképernyője")
5. Válasszon egy adatforrás lap hello, jelölje ki **Azure Data Lake**, és kattintson a **következő**.

    ![Adatforrás kiválasztása](./media/data-lake-store-with-data-catalog/select-source.png "adatforrás kiválasztása")
6. Hello következő lapon adja meg a Data Lake Store-fiók nevét, amelyet a Data Catalog tooregister hello. Egyéb lehetőségek hello hagyja az alapértelmezett, és kattintson a **Connect**.

    ![Csatlakozás toodata forrás](./media/data-lake-store-with-data-catalog/connect-to-source.png "Connect toodata forrás")
7. hello következő lap a következő szegmensek hello lehet osztani.

    a. Hello **kiszolgálói hierarchia** mezőben hello Data Lake Store-fiók mappaszerkezet jelöli. **$Root** jelöli hello Data Lake Store-fiók gyökérben, és **AmbulanceData** jelöli hello hello Data Lake Store-fiók gyökérkönyvtárában hello létrehozott mappába.

    b. Hello **rendelkezésre álló objektumok** mezőben hello fájlokat és mappákat a hello tartalmazza **AmbulanceData** mappa.

    c. **Objektumok toobe regisztrált mezőben** listák hello fájlok és mappák, amelyet az Azure Data Catalog tooregister.

    ![Adatszerkezet megtekintése](./media/data-lake-store-with-data-catalog/view-data-structure.png "adatszerkezet megtekintése")
8. Ebben az oktatóanyagban regisztrálnia kell az összes hello fájlok hello könyvtárban. Az adott, kattintson a hello (![-objektumainak áthelyezése](./media/data-lake-store-with-data-catalog/move-objects.png "-objektumainak áthelyezése")) gomb toomove összes hello fájlok túl**regisztrált objektumok toobe** mezőbe.

    Egy szervezeti adatkatalógus hello adatok lesz regisztrálva, mert egy ajánlott maszkolandó közelítse tooadd bizonyos metaadatait, amelyet később felhasználhat tooquickly hello adatok elhelyezése. Például egy e-mail címet (például egy ki az adatfeltöltés hello) hello adatok tulajdonosa hozzá, vagy egy címke tooidentify hello adatok hozzáadása. az alábbi hello képernyőfelvétel-készítés mutatja, hogy azt toohello adatok hozzáadása egy címkét.

    ![Adatszerkezet megtekintése](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "adatszerkezet megtekintése")

    Kattintson a **regisztrálása**.
9. hello következő képernyőfelvétel-készítés azt jelzi, hogy hello adatok sikeresen regisztrálva van a Data Catalog hello.

    ![Regisztráció kész](./media/data-lake-store-with-data-catalog/registration-complete.png "adatszerkezet megtekintése")
10. Kattintson a **View Portal** toogo biztonsági toohello Data Catalog-portált, és győződjön meg arról, hogy most hozzáférés hello regisztrált hello portálról adatokat is. toosearch hello adatok, hello adatok regisztrálása során használt hello címke is használhatja.

     ![Adatok keresése a katalógus](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "keresés adatokat keressen a katalógusban")
11. Most már például való hozzáadását a jegyzeteket és dokumentáció toohello adatok műveleteket hajthat végre. További információkért tekintse meg a következő hivatkozások hello.

    * [A Data Catalog adatforrások ellátása megjegyzésekkel](../data-catalog/data-catalog-how-to-annotate.md)
    * [A Data Catalog dokumentum adatforrások](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Lásd még:
* [A Data Catalog adatforrások ellátása megjegyzésekkel](../data-catalog/data-catalog-how-to-annotate.md)
* [A Data Catalog dokumentum adatforrások](../data-catalog/data-catalog-how-to-documentation.md)
* [Data Lake Store más Azure-szolgáltatásokkal integrálja](data-lake-store-integrate-with-other-services.md)
