---
title: "Oktatóanyag: Folyamat létrehozása a Másolás varázsló használatával | Microsoft Docs"
description: "Ebben az oktatóanyagban hoz létre egy Azure Data Factory-folyamat a másolási tevékenység hello adat-előállító által támogatott másolása varázsló használatával"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 567b89e7a54c245c134cd0674690e6f3499b46d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a>Oktatóanyag: Másolási tevékenységgel rendelkező folyamat létrehozása a Data Factory Másolás varázslója használatával
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Másolás varázsló](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

Az oktatóanyag bemutatja, hogyan toouse hello **másolása varázsló** toocopy Azure blob storage tooan Azure SQL-adatbázis adatait. 

Azure Data Factory hello **másolása varázsló** lehetővé teszi a tooquickly hozzon létre egy adatok folyamatot, amely egy támogatott forráshierarchiából adatokat tároló támogatott tooa cél adattárból másolja az adatokat. Ezért azt javasoljuk, hogy hello varázslót használja, az első lépés toocreate egy minta folyamatot a mozgás forgatókönyvet. A forrásként és célként támogatott adattárak listáját a [támogatott adattárakról](data-factory-data-movement-activities.md#supported-data-stores-and-formats) szóló témakörben találja.  

Az oktatóanyag bemutatja, hogyan toocreate egy az Azure data factory indítási hello másolása varázsló halad át lépéseket tooprovide adatait az adatfeldolgozást/adatátviteli forgatókönyvet sorozata. Hello varázsló befejezése után hello varázsló automatikusan hoz létre egy folyamatot a másolási tevékenység toocopy adatait az Azure blob storage tooan Azure SQL-adatbázis. A másolási tevékenységről további információk az [adattovábbítási tevékenységekről](data-factory-data-movement-activities.md) szóló cikkben találhatók.

## <a name="prerequisites"></a>Előfeltételek
Végezze el a témakörben ismertetett előfeltételeknek: hello [oktatóanyag – áttekintés](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) cikk Ez az oktatóanyag végrehajtása előtt.

## <a name="create-data-factory"></a>Data factory létrehozása
Ebben a lépésben használhatja az Azure portál toocreate egy az Azure data factory nevű hello **ADFTutorialDataFactory**.

1. Jelentkezzen be túl[Azure-portálon](https://portal.azure.com).
2. Kattintson a **+ új** hello bal felső sarokban, kattintson **adatok + analitika**, és kattintson a **adat-előállító**. 
   
   ![New (Új)->DataFactory](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. A hello **új adat-előállító** panel:
   
   1. Adja meg **ADFTutorialDataFactory** a hello **neve**.
       az Azure data factory hello hello nevének globálisan egyedi kell lennie. Ha hello hibaüzenetet kapja: `Data factory name “ADFTutorialDataFactory” is not available`, hello adat-előállítóban (például yournameADFTutorialDataFactoryYYYYMMDD) hello nevének módosítása, és próbálja meg újra létrehozni. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.  
      
       ![A Data Factory name not available (A data factory neve nem érhető el) üzenet](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)    
   2. Jelölje ki az Azure-**előfizetést**.
   3. Az erőforráscsoport hajtsa végre a lépéseket követve hello egyikét: 
      
      - Válassza ki **meglévő** tooselect egy meglévő erőforráscsoportot.
      - Válassza ki **hozzon létre új** tooenter erőforráscsoport nevét.
          
        Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy, hogy hello nevét használja: **ADFTutorialResourceGroup** hello erőforráscsoport. tudnivalók az erőforráscsoportokról, toolearn lásd [erőforrás segítségével csoportosítja toomanage az Azure-erőforrások](../azure-resource-manager/resource-group-overview.md).
   4. Válassza ki a **hely** hello adat-előállító esetében.
   5. Válassza ki **PIN-kód toodashboard** hello hello panel alsó részén jelölőnégyzetet.  
   6. Kattintson a **Create** (Létrehozás) gombra.
      
       ![A New data factory (Új data factory) panel](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. Hello létrehozásának befejezése után megjelenik a hello **adat-előállító** panelen látható hello kép a következő módon:
   
   ![Data factory kezdőlap](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a>A Másolás varázsló indítása
1. Hello adat-előállító paneljén kattintson **[előzetes verzió] adatok másolása** toolaunch hello **másolása varázsló**. 
   
   > [!NOTE]
   > Ha azt látja, hogy hello webböngésző akadt-e a "Engedélyező...", tiltsa le vagy törölje a jelet **külső cookie-k blokkolását, és a helyadatok** beállítása a böngészőbeállítások hello (vagy) során is garantálják az engedélyezve van, és hozzon létre egy kivételt  **login.microsoftonline.com** , és ezután próbálja meg újból elindítani a hello varázsló.
2. A hello **tulajdonságok** lap:
   
   1. Írja be a **CopyFromBlobToAzureSql** kifejezést a **Task name** (Feladat neve) mezőbe.
   2. Adjon meg **leírást** (opcionális).
   3. Változás hello **kezdő dátum/idő** és hello **záró dátum és idő** úgy, hogy hello záró dátum tootoday be és indítsa el a korábbi dátumot toofive nap.  
   4. Kattintson a **Tovább** gombra.  
      
      ![Copy (Másolás) eszköz – Properties (Tulajdonságok) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. A hello **forrás adattár** kattintson **Azure Blob Storage** csempére. Az oldal toospecify hello forrás adattároló hello másolási feladathoz kell használnia. 
   
    ![Copy (Másolás) eszköz – Source data store (Forrásadattár) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. A hello **hello Azure Blob storage-fiók megadása** lap:
   
   1. Adja meg az **AzureStorageLinkedService** nevet a **Linked service name** (Társított szolgáltatás neve) mezőben.
   2. Győződjön meg arról, hogy az **Account selection method** (Fiókválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.
   3. Jelölje ki az Azure-**előfizetést**.  
   4. Válasszon egy **Azure storage-fiók** hello a hello kiválasztott előfizetésben elérhető fiókok az Azure storage listája. Másik lehetőségként tooenter tárolási fiók beállításait manuálisan kiválasztásával **adja meg manuálisan** hello beállítása **kijelöléséről fiók**, és kattintson a **tovább**. 
      
      ![Másolja az eszköz – hello Azure Blob storage-fiók megadása](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. A **válasszon hello bemeneti fájl vagy mappa** lap:
   
   1. Kattintson duplán az **adftutorial** mappára.
   2. Jelölje ki az **emp.txt** fájlt, és kattintson a **Choose** (Kiválasztás) lehetőségre.
      
      ![Másolja az eszköz - a hello bemeneti fájl vagy mappa](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. A hello **válasszon hello bemeneti fájl vagy mappa** kattintson **következő**. Ne jelölje be a **Binary copy** (Bináris másolat) beállítást. 
   
    ![Másolja az eszköz - a hello bemeneti fájl vagy mappa](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. A hello **fájl formázási beállítások** lapon látni hello határolójelek és hello sémát, amelyhez hello varázsló automatikusan észleli hello-fájl elemzése. Hello határolójelek manuálisan is beírhatja hello másolása varázsló toostop automatikus-észlelése vagy toooverride. Kattintson a **következő** után tekintse át a hello elválasztó karaktert, és tekintse meg adatok. 
   
    ![Copy (Másolás eszköz) – File format settings (Fájlformátum beállításai) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. Hello cél adatokon tárolja a lapra, válassza ki **Azure SQL Database**, és kattintson a **következő**.
   
    ![Copy (Másolás) eszköz – Choose destination store (Céltár kiválasztása) oldal](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. A **megadása hello Azure SQL adatbázis** lap:
   
   1. Adja meg **AzureSqlLinkedService** a hello **kapcsolatnév** mező.
   2. Győződjön meg arról, hogy a **Server/database selection method** (Kiszolgáló-/adatbázis-kiválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.
   3. Jelölje ki az Azure-**előfizetést**.  
   4. Válassza ki a **Server name** (Kiszolgálónév) és a **Database** (Adatbázis) mezők értékeit.
   5. Adja meg a **felhasználónevet** és a **jelszót**.
   6. Kattintson a **Tovább** gombra.  
      
      ![Copy (Másolás) eszköz – specify Azure SQL database (Az Azure SQL Database megadása) oldal](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. A hello **táblaleképezés** lapon jelölje be **üres** a hello **cél** hello legördülő listából, majd kattintson **lefelé mutató nyíl** (nem kötelező) toosee hello séma- és toopreview hello adatokat.
    
     ![Copy (Másolás) eszköz – Table mapping (Tábla-hozzárendelés) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. A hello **séma-hozzárendelése** kattintson **következő**.
    
    ![Copy (Másolás) eszköz – schema mapping (Séma-hozzárendelés) oldal](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. A hello **Teljesítménybeállítások** kattintson **következő**. 
    
    ![Copy (Másolás) eszköz – performance settings (Teljesítménybeállítások) oldal](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. Tekintse át a hello **összegzés** lapon, majd kattintson **Befejezés**. hello varázsló létrehoz két társított szolgáltatások, két adatkészletet (bemeneti és kimeneti) és egy folyamat (amelyen Ön indítani hello másolása varázsló) az adat-előállító hello. 
    
    ![Copy (Másolás) eszköz – performance settings (Teljesítménybeállítások) oldal](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a>A Monitor and Manage alkalmazás elindítása
1. A hello **telepítési** lapján kattintson a hello hivatkozás: `Click here toomonitor copy pipeline`.
   
   ![Copy (Másolás) eszköz – Deployment succeeded (Sikeres üzembe helyezés) oldal](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. a webböngészőben külön lapon alkalmazás hello indul el.   
   
   ![Monitoring App](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png)   
3. toosee hello legfrissebb állapotát óránkénti szeletek kattintson **frissítése** hello gombjára **tevékenység WINDOWS** lista hello lap alján. Öt tevékenység windows hello adatcsatorna kezdő és záró időpont között öt napig láthatja. hello lista nem frissül automatikusan, így Ön kell tooclick frissítse néhány alkalommal megjelenik az összes hello tevékenység windows hello üzemkész állapotban. 
4. Jelöljön ki egy tevékenységet ablak hello listán. Hello vonatkozó részletes azt a hello **tevékenység ablak Explorer** a jobb oldali hello.

    ![Tevékenységablakok részletei](media/data-factory-copy-data-wizard-tutorial/activity-window-details.png)    

    Figyelje meg, hogy hello dátumok 11, 12, 13, 14 vagy 15 zöld színnel történik, ami azt jelenti, hogy ebben az időszakban hello napi kimeneti szeletek már előállított legyenek. Is tekintse meg a színkódolás a hello folyamat, és hello kimeneti adatkészlet hello diagram nézetben. Hello előző lépésben figyelje meg, hogy két szeletek már előállított, egy szelet folyamatban van, és a hello más két végrehajtásra feldolgozott toobe (hello színkódolás alapján). 

    Az alkalmazás használatáról további tudnivalókat talál a [Monitor and manage pipeline using Monitoring App](data-factory-monitor-manage-app.md) (Folyamat figyelése és felügyelete a Monitoring App használatával) című cikkben.

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt. hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Mezők és tulajdonságok számára egy adattárból hello másolása varázsló látható információt rendszerben hivatkozásra hello hello adattároló hello táblában. 
