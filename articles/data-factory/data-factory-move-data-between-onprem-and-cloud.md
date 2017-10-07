---
title: "aaaMove adatok - az adatkezelési átjáró |} Microsoft Docs"
description: "Egy adatok átjáró toomove adatok között a helyszíni és hello beállítása felhő. Használja az adatkezelési átjáró Azure Data Factory toomove adatait."
keywords: "az adatátjáró, Adatintegráció, áthelyezni az adatokat, átjáró hitelesítő adatok"
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 314341c142d5260c785b7e82081774f044450e81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-between-on-premises-sources-and-hello-cloud-with-data-management-gateway"></a>Adatok áthelyezése a helyszíni adatforrások és az adatkezelési átjáró hello felhő között
Ez a cikk áttekintést adatok integrálása a helyszíni adattárolókhoz és a Data Factory használatával felhő adattárolók között. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk és az egyéb adatok gyári alapvető fogalmak: [adatkészletek](data-factory-create-datasets.md) és [folyamatok](data-factory-create-pipelines.md).

## <a name="data-management-gateway"></a>Adatkezelési átjáró
Telepítenie kell az adatkezelési átjáró a helyi gép tooenable adatmozgatás belőle egy helyszíni adattároló. hello átjáró ugyanaz a gép hello adattárban vagy egy másik számítógépen, amíg hello-átjáró képes kapcsolódni toohello adattár hello is telepíthető.

> [!IMPORTANT]
> Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) szóló cikkben olvashat az adatkezelési átjáró. 

hello következő forgatókönyv bemutatja, hogyan toocreate egy adat-előállító egy folyamatot, amely egy helyszíni helyezi át az adatokat a **SQL Server** tooan Azure blob-tároló adatbázis. Hello forgatókönyv részeként telepítse, és konfigurálja a hello az adatkezelési átjáró a számítógépre.

## <a name="walkthrough-copy-on-premises-data-toocloud"></a>Forgatókönyv: a helyszíni adatok toocloud másolása
A bemutatóban szereplő lépéseket követve hello: 

1. Adat-előállító létrehozása
2. Hozzon létre egy az adatkezelési átjáró. 
3. A forrás és a fogadó adattárolókhoz társított szolgáltatások létrehozásához.
4. Létrehozni adatkészletek toorepresent bemeneti és kimeneti adatai.
5. Hozzon létre egy folyamatot egy tevékenység toomove hello adatok másolása.

## <a name="prerequisites-for-hello-tutorial"></a>Hello oktatóanyag előfeltételei
Ez a forgatókönyv megkezdése előtt rendelkeznie kell a következő előfeltételek hello:

* **Azure-előfizetés**.  Ha nem rendelkezik előfizetéssel, mindössze néhány perc alatt létrehozhat egy ingyenes próbafiókot. Lásd: hello [ingyenes](http://azure.microsoft.com/pricing/free-trial/) cikkben alább.
* **Azure Storage-fiók**. Hello blob Storage tárolót használja a **cél/fogadó** ebben az oktatóanyagban az adattároló. Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) lépéseket toocreate egyet a cikkben találhat.
* **Egy SQL Server**. Ebben az oktatóanyagban egy helyszíni SQL Server-adatbázist használunk **forrásadattárként**. 

## <a name="create-data-factory"></a>Data factory létrehozása
Ebben a lépésben használhatja az Azure Data Factory-példány neve az Azure portál toocreate hello **ADFTutorialOnPremDF**.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **+ új**, kattintson a **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.

   ![New (Új)->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. A hello **új adat-előállító** lapján adja meg **ADFTutorialOnPremDF** a hello nevét.

    ![TooStartboard hozzáadása](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > az Azure data factory hello hello nevének globálisan egyedi kell lennie. Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "ADFTutorialOnPremDF"**, hello adat-előállítóban (például yournameADFTutorialOnPremDF) hello nevének módosítása, és próbálja meg újra létrehozni. Használja ezt a nevet ADFTutorialOnPremDF helyett fennmaradó lépéseit az oktatóanyag végrehajtása közben.
   >
   > hello adat-előállító nevét hello regisztrálva előfordulhat, hogy legyen egy **DNS** hello jövőben nevet, és ezért a nyilvánosan láthatóvá válnak.
   >
   >
4. Jelölje be hello **Azure-előfizetés** hello data factory toobe létrehozni kívánt.
5. Jelöljön ki egy meglévő **erőforráscsoportot**, vagy hozzon létre egyet. Az oktatóanyagban hello nevű erőforráscsoport létrehozása: **ADFTutorialResourceGroup**.
6. Kattintson a **létrehozása** a hello **új adat-előállító** lap.

   > [!IMPORTANT]
   > toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.
   >
   >
7. Létrehozásának befejezése után megjelenik a hello **adat-előállító** lapon látható hello kép a következő módon:

   ![Data Factory kezdőlap](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Átjáró létrehozása
1. A hello **adat-előállító** kattintson **Szerző és központi telepítése** csempe toolaunch hello **szerkesztő** az hello data factory.

    ![Az Author and deploy (Fejlesztés és üzembe helyezés) csempe](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. A Data Factory Editor hello, kattintson **... További** az eszköztár hello, és kattintson a **az új adatátjáró**. Másik lehetőségként kattintson a jobb egérgombbal **Data Gateways** hello fanézetben, és kattintson a **az új adatátjáró**.

   ![Az új adatátjáró eszköztár](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. A hello **létrehozása** lapján adja meg **adftutorialgateway** a hello **neve**, és kattintson a **OK**.     

    ![Átjáró lap létrehozása](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > A forgatókönyv akkor hozzon létre hello logikai csak egy csomópont (a helyi Windows-számítógépen). Az adatkezelési átjáró kiterjesztése hello átjáró több helyszíni gépet társít. Méretezheti növelésével csomóponton egyidejűleg futtatható az adatátviteli feladatok száma legfeljebb. Ez a szolgáltatás egy logikai átjárójának egyetlen csomópont is érhető el. Lásd: [méretezés az adatkezelési átjáró az Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) cikkben alább.  
4. A hello **konfigurálása** kattintson **telepíthető közvetlenül a számítógépen**. Ez a művelet hello átjáró hello telepítési csomagot tölti le, telepíti, konfigurálja, és regisztrálja a hello átjáró hello számítógépen.  

   > [!NOTE]
   > Az Internet Explorer vagy a Microsoft ClickOnce kompatibilis böngésző használata.
   >
   > Ha a Chrome használ, lépjen a toohello [Chrome webes tároló](https://chrome.google.com/webstore/), keressen a "ClickOnce" kulcsszóval, válasszon egyet a hello ClickOnce kiterjesztések, és telepítse.
   >
   > Hello azonos Firefox (install-bővítmény). Kattintson a **menü megnyitása** hello gombjára (**három vízszintes vonal** hello jobb felső sarkában), kattintson a **bővítmények**, keressen a "ClickOnce" kulcsszóval, válasszon egyet a hello ClickOnce-bővítményeket, és telepítse azt.    
   >
   >

    ![Átjáró - lapjának konfigurálása](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Ily módon hello legegyszerűbb módja (egykattintásos) toodownload, telepítése, konfigurálása, és regisztrálja a hello átjárót egy egyetlen lépésben. Megtekintheti a hello **Microsoft adatkezelési átjáró konfigurációkezelőjének** alkalmazás telepítve van a számítógépen. Megtalálhat hello végrehajtható **ConfigManager.exe** hello mappában: **C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\Shared**.

    Is letöltheti és ezen a lapon hello hivatkozások használatával manuálisan telepíteni az átjárót és hello látható hello kulccsal regisztrálható **új kulcs** szövegmezőben.

    Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) a következő cikket: az összes hello hello átjáró adatait.

   > [!NOTE]
   > A helyi számítógép tooinstall hello rendszergazdai és hello az adatkezelési átjáró sikeresen konfigurálnia kell. Hozzáadhat további felhasználók toohello **adatkezelési átjáró felhasználói** helyi Windows-csoport. a csoport tagjai hello hello az adatkezelési átjáró konfigurációkezelőjének eszköz tooconfigure hello gateway használható.
   >
   >
5. Várjon néhány percet, vagy várjon, amíg megjelenik a következő értesítési üzenet hello:

    ![Átjáró telepítése sikeres](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. Indítsa el **az adatkezelési átjáró konfigurációkezelőjének** alkalmazás a számítógépre. A hello **keresési** ablakot, írja be **az adatkezelési átjáró** tooaccess ilyen eszközt. Megtalálhat hello végrehajtható **ConfigManager.exe** hello mappában: **C:\Program Files\Microsoft Data felügyeleti Gateway\2.0\Shared**

    ![Átjáró konfigurációkezelője](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. Ellenőrizze, hogy látható `adftutorialgateway is connected toohello cloud service` üzenet. hello alsó megjeleníti állapotsorban hello **toohello felhőszolgáltatás csatlakoztatott** együtt egy **zöld pipa**.

    A hello **Home** lapon is elvégezhető műveletek következő hello:

   * **Regisztrálni** egy átjárót kulccsal hello Azure-portálon hello Register gomb használatával.
   * **Állítsa le** az adatkezelési átjáró gazdaszolgáltatása az átjáró gépen futó hello.
   * **Frissítések ütemezése** toobe hello nap adott időpontban telepítve.
   * Megtekinthet, amikor a hello átjáró lett **utolsó frissítés**.
   * Adjon meg egy frissítési toohello átjárót telepítheti időpontot.
8. Váltás toohello **beállítások** hello megadott külön-külön hello tanúsítvány **tanúsítvány** szakaszban használt tooencrypt/visszafejtési hello helyszíni adattár hello portal meg hitelesítő adatait. (választható) Kattintson a **módosítás** toouse saját tanúsítvány helyette. Alapértelmezés szerint hello átjáró hello tanúsítványt használ, amelyet van a Data Factory szolgáltatásnak hello hozta létre automatikusan.

    ![Átjáró tanúsítvány konfigurálása](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Azt is megteheti hello az alábbi műveleteket a hello **beállítások** lapon:

   * Megtekintése vagy hello hello átjáró által használt tanúsítvány exportálása.
   * Módosítsa a hello átjáró által használt hello HTTPS-végponton.    
   * Egy hello átjáró által használt HTTP-proxy toobe beállítása.     
9. (választható) Váltás toohello **diagnosztika** fület, ellenőrizze a hello **részletes naplózás engedélyezése** lehetőséget, ha azt szeretné, tooenable részletes naplózás használható tootroubleshoot probléma merül fel hello átjáróval. hello naplóinformációk található **Eseménynapló** alatt **alkalmazási és Szolgáltatásnaplójában** -> **az adatkezelési átjáró** csomópont.

    ![Diagnosztika lap](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    A következő műveletek a hello hello is végrehajthat **diagnosztika** lapon:

   * Használjon **kapcsolat tesztelése** szakasz tooan helyszíni adatforrás hello átjáró használatával.
   * Kattintson a **naplók megtekintése** toosee hello az adatkezelési átjáró jelentkezzen be egy Eseménynapló-ablakban.
   * Kattintson a **naplók küldése** tooupload egy zip-fájlt az elmúlt hét napban tooMicrosoft toofacilitate hibaelhárításához a problémák rögzít.
10. A hello **diagnosztika** lap hello **kapcsolat tesztelése** szakaszban jelölje be **SqlServer** hello adattípushoz hello tárolja, adjon meg hello hello adatbázis-kiszolgáló, neve adatbázis hello, adja meg a hitelesítés típusát, írja be a felhasználónevet és jelszót, majd kattintson **teszt** tootest e hello-átjáró képes kapcsolódni a toohello adatbázis.
11. Kapcsoló toohello webböngésző, és a hello **Azure-portálon**, kattintson a **OK** a hello **konfigurálása** lap majd a hello **az új adatátjáró** lap.
12. Megtekintheti az **adftutorialgateway** alatt **Data Gateways** a hello bal oldali fanézetben hello.  A hivatkozásra, megtekintheti az hello kapcsolódó JSON.

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
Ebben a lépésben két társított szolgáltatások létrehozásához: **AzureStorageLinkedService** és **SqlServerLinkedService**. Hello **SqlServerLinkedService** kapcsolat egy helyi SQL Server-adatbázis és a hello **AzureStorageLinkedService** társított szolgáltatás az Azure blob-tároló toohello adat-előállító hivatkozásokat tartalmaz. Ez a forgatókönyv másol adatokat hello a helyszíni SQL Server adatbázis toohello Azure blob-tároló részében hoz létre egy folyamatot.

#### <a name="add-a-linked-service-tooan-on-premises-sql-server-database"></a>Adja hozzá a társított szolgáltatás tooan a helyszíni SQL Server-adatbázis
1. A hello **Data Factory Editor**, kattintson a **az új adattároló** hello eszköztár, és válassza a **SQL Server**.

   ![Új kapcsolódó SQL Server szolgáltatás](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. A hello **JSON-szerkesztő** a jobb oldali hello hello lépéseket követve:

   1. A hello **gatewayName**, adja meg **adftutorialgateway**.    
   2. A hello **connectionString**, hello a következő lépéseket:    

      1. A **kiszolgálónév**, adja meg, amelyen az SQL Server-adatbázis hello hello kiszolgáló hello nevét.
      2. A **databasename**, adja meg a hello hello adatbázis nevét.
      3. Kattintson a **titkosítása** hello gombjára. Hello hitelesítőadat-kezelője alkalmazás láthatja.

         ![Hitelesítő adatok Manager alkalmazásban](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. A hello **hitelesítő adatok beállítása a** párbeszédpanelen adja meg a hitelesítés típusa, a felhasználónevet és jelszót, majd kattintson **OK**. Sikeres hello kapcsolat esetén hello JSON hello titkosított hitelesítő adatokat tárolja, és hello párbeszédpanel bezárul.
      5. Zárja be, amelyek alkalmazhatók hello párbeszédpanel, ha nem automatikusan lezáródik és hello Azure-portálon toohello lap segítségnyújtáshoz hello üres böngészőlapon.

         Hello-átjáró számítógépén, ezek a hitelesítő adatok vannak **titkosított** tanúsítvánnyal, amely a Data Factory hello szolgáltatás tulajdonosa. Ha azt szeretné, hogy toouse hello tanúsítványt az adatkezelési átjáró hello társított helyett, lásd: [biztonságosan használandó hitelesítő adatok beállításához](#set-credentials-and-security).    
   3. Kattintson a **telepítés** a hello parancssávon toodeploy hello kapcsolódó SQL Server szolgáltatás. Kapcsolódó hello szolgáltatás hello faszerkezetes nézetben kell megjelennie.

      ![Kapcsolódó SQL Server szolgáltatás hello faszerkezetes nézetben](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>A társított szolgáltatás az Azure-tárfiók hozzáadása
1. A hello **Data Factory Editor**, kattintson a **az új adattároló** a hello parancssávon, és kattintson a **az Azure storage**.
2. Adja meg az Azure storage-fiókjának nevét hello hello **fióknév**.
3. Adja meg az Azure storage-fiók kulcsát hello hello **fiókkulcs**.
4. Kattintson a **telepítés** toodeploy hello **AzureStorageLinkedService**.

## <a name="create-datasets"></a>Adatkészletek létrehozása
Ebben a lépésben létrehozni bemeneti és kimeneti adatkészletek hello másolási művelet bemeneti és kimeneti adatokat megjelenítő (a helyszíni SQL Server-adatbázis = > az Azure blob-tároló). Az hello adatkészletek létrehozása, előtt a következő lépéseket (részletes lépéseit követi hello lista):

* Hozzon létre egy táblát nevű **üres** az SQL Server-adatbázis egy kapcsolódó szolgáltatás toohello adat-előállító hozzáadott hello, majd szúrja be a minta bejegyzések hello táblába néhány.
* Hozzon létre egy blob-tároló nevű **adftutorial** a hello Azure blob storage-fiók egy kapcsolódó szolgáltatás toohello adat-előállító hozzáadott.

### <a name="prepare-on-premises-sql-server-for-hello-tutorial"></a>A helyszíni SQL Server hello az oktatóanyag előkészítéséhez
1. A megadott hello a helyszíni SQL Server hello adatbázis társított szolgáltatás (**SqlServerLinkedService**), használja a következő SQL-parancsfájl toocreate hello hello **üres** hello adatbázis táblájában.

    ```SQL   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Néhány példa beszúrása hello táblába:

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása

1. A hello **Data Factory Editor**, kattintson a **... További**, kattintson a **új adatkészlet** hello parancssávon, és kattintson a **SQL Server tábla**.
2. Cserélje le a következő szöveg hello hello JSON hello jobb oldali ablaktáblában:

    ```JSON   
    {        
        "name": "EmpOnPremSQLTable",
        "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }     
    ```     
   Vegye figyelembe a következő pontok hello:

   * **típus** értéke túl**SqlServerTable**.
   * **tableName** értéke túl**üres**.
   * **linkedServiceName** értéke túl**SqlServerLinkedService** (korábban létrehozott szolgáltatásnak a forgatókönyv során korábban küldje el.).
   * Egy bemeneti adatkészlet nem az Azure Data Factory egy másik folyamat által generált, be kell **külső** túl**igaz**. Ez azt jelzi, bemeneti adatokat hello előállított külső toohello Azure Data Factory szolgáltatásnak. Opcionálisan megadhat bármilyen külső adatok házirendekkel hello **externalData** hello elemének **házirend** szakasz.    

   Lásd: [helyezi át az adatokat az SQL Server](data-factory-sqlserver-connector.md) JSON-tulajdonságok vonatkozó további információért.
3. Kattintson a **telepítés** a toodeploy hello dataset hello parancssávon.  

### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása

1. A hello **Data Factory Editor**, kattintson a **új adatkészlet** hello parancssávon, és kattintson a **Azure Blob Storage tárolóban**.
2. Cserélje le a következő szöveg hello hello JSON hello jobb oldali ablaktáblában:

    ```JSON   
    {
        "name": "OutputBlobTable",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/outfromonpremdf",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```   
   Vegye figyelembe a következő pontok hello:

   * **típus** értéke túl**AzureBlob**.
   * **linkedServiceName** értéke túl**AzureStorageLinkedService** (hozna létre szolgáltatásnak a 2. lépés).
   * **folderPath** értéke túl**adftutorial/outfromonpremdf** outfromonpremdf esetén hello mappa hello adftutorial tárolóban. Hozzon létre hello **adftutorial** tárolót, ha még nem létezik.
   * Hello **rendelkezésre állási** értéke túl**óránkénti** (**gyakoriság** túl beállítása**óra** és **időköz** túl beállítása **1**).  hello Data Factory szolgáltatásnak hoz létre egy kimeneti adatszelet óránként hello **üres** hello Azure SQL adatbázis táblájában.

   Ha nincs megadva egy **Fájlnév** a egy **eredménytábla**, hello hello létrehozott fájlokat **folderPath** formátuma a következő hello elnevezése: adatok.<Guid>. txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

   tooset **folderPath** és **Fájlnév** dinamikusan alapján hello **SliceStart** idő, hello partitionedBy tulajdonsággal. A következő példa hello folderPath év, hónap és nap származó hello SliceStart (kezdő időpontjának hello szelet feldolgozása folyamatban) használja, és a fájlnév óra használ a hello SliceStart. Például, ha a rendszer hozott létre a szelet 2014-10-20T08:00:00, hello mappanév toowikidatagateway/wikisampledataout/2014/10/20 hello fájlnév van beállítva és too08.csv.

    ```JSON
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
    [

        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],
    ```

    Lásd: [helyezi át az adatokat az Azure Blob Storage](data-factory-azure-blob-connector.md) JSON-tulajdonságok vonatkozó további információért.
3. Kattintson a **telepítés** a toodeploy hello dataset hello parancssávon. Győződjön meg arról, hogy megjelenik-e mindkét hello adatkészletek hello faszerkezetes nézetben.  

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehoz egy **csővezeték** egy **másolási tevékenység** használó **EmpOnPremSQLTable** bemenetként és **OutputBlobTable** output típusúként.

1. Kattintson a Data Factory Editor **... Továbbiak**, majd az **Új adatcsatorna** elemre.
2. Cserélje le a következő szöveg hello hello JSON hello jobb oldali ablaktáblában:    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on-prem SQL tooAzure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on-prem SQL server tooblob",
             "type": "Copy",
             "inputs": [
               {
                 "name": "EmpOnPremSQLTable"
               }
             ],
             "outputs": [
               {
                 "name": "OutputBlobTable"
               }
             ],
             "typeProperties": {
               "source": {
                 "type": "SqlSource",
                 "sqlReaderQuery": "select * from emp"
               },
               "sink": {
                 "type": "BlobSink"
               }
             },
             "Policy": {
               "concurrency": 1,
               "executionPriorityOrder": "NewestFirst",
               "style": "StartOfInterval",
               "retry": 0,
               "timeout": "01:00:00"
             }
           }
         ],
         "start": "2016-07-05T00:00:00Z",
         "end": "2016-07-06T00:00:00Z",
         "isPaused": false
       }
     }
    ```   
   > [!IMPORTANT]
   > Cserélje le a hello hello értékének **start** tulajdonságát az aktuális nap hello és **end** hello másnap értéket.
   >
   >

   Vegye figyelembe a következő pontok hello:

   * Hello tevékenységek szakaszban csak tevékenység nincs amelynek **típus** értéke túl**másolási**.
   * **Bemeneti** hello tevékenység túl van-e állítva a**EmpOnPremSQLTable** és **kimeneti** hello tevékenység túl van-e állítva a**OutputBlobTable**.
   * A hello **typeProperties** szakaszban **SqlSource** hello van megadva **adatforrástípust** és ** BlobSink ** hello megadott **típusgyűjtése**.
   * SQL-lekérdezés `select * from emp` hello megadott **sqlReaderQuery** tulajdonsága **SqlSource**.

   Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni. Például: 2014-10-14T16:32:41Z. Hello **end** idő megadása nem kötelező, de ebben az oktatóanyagban használjuk.

   Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra**". toorun hello folyamat határozatlan ideig, adja meg **9 9/9999** hello hello értékként **end** tulajdonság.

   Meghatározásakor hello hello alapján mely hello adatszeletek feldolgozása akkor történik meg időtartamig **rendelkezésre állási** minden Azure Data Factory adatkészlet definiált tulajdonságaihoz.

   Hello példában amelyeket 24 adatszeletek minden adatszelet rendszer óránként készít adatszeletet.        
3. Kattintson a **telepítés** a toodeploy hello dataset hello parancssávon (a tábla téglalap alakú dataset). Győződjön meg róla, hello faszerkezetes nézetben alapján jeleníti meg, hogy hello folyamat **folyamatok** csomópont.  
4. Most kattintson **X** kétszer tooclose hello lap tooget hátsó toohello **adat-előállító** hello lapján **ADFTutorialOnPremDF**.

**Gratulálunk!** Sikeresen létrehozott egy az Azure data factory, társított szolgáltatások, adathalmazt és egy folyamat és ütemezett hello folyamat.

#### <a name="view-hello-data-factory-in-a-diagram-view"></a>A Diagram nézet nézet hello adat-előállító
1. A hello **Azure-portálon**, kattintson a **Diagram** hello kezdőlap hello csempe **ADFTutorialOnPremDF** adat-előállítóban. :

    ![Diagram-hivatkozás](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. Hello diagram hasonló toohello kép a következő kell megjelennie:

    ![Diagramnézet](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Nagyítás, Kicsinyítés, nagyítás too100 %, nagyítás toofit, automatikusan pozíció folyamatok és adatkészleteket és Leszármaztatás adatok megjelenítése (kiemeli a kijelölt elemek felsőbb és alsóbb rétegbeli elemeket).  Egy objektum (bemeneti/kimeneti adatkészlet vagy csővezeték) toosee tulajdonságok az duplán kattintva.

## <a name="monitor-pipeline"></a>Folyamat figyelése
Ebben a lépésben az Azure data factory lesz az Azure portál toomonitor hello használja. PowerShell-parancsmagok toomonitor adathalmazok és adatcsatornák is használható. Figyelésével kapcsolatos részletekért lásd: [figyelő és folyamatok kezelése](data-factory-monitor-manage-pipelines.md).

1. Hello diagramon kattintson duplán **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable szeletek](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. Figyelje meg, hogy minden hello adatszelet fel vannak **készen** állapot, mert a korábbi hello hello folyamat időtartama (kezdési idő tooend idő). Akkor is mert hello adatok beszúrt hello SQL Server-adatbázisban, és nincs minden hello idő. Győződjön meg arról, hogy nincs szeletek megjelenni hello **probléma szeletek** szakasz hello lap alján. tooview összes hello szeletek, kattintson a **lásd: több** szeletek hello listája hello alján.
3. Most, a hello **adatkészletek** kattintson **OutputBlobTable**.

    ![OputputBlobTable szeletek](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. Bármely adatszelet hello listából kattintson, és megtekintheti az hello **Adatszelet** lap. Megjelenik a tevékenység futtatása hello szelet. Csak egy tevékenységfuttatási általában láthatja.  

    ![Adatok szelet panel](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Ha hello szelet nincs hello **készen** állapot, hello felfelé irányuló szeletek nem készen áll, és blokkol hello aktuális szelet hello végrehajtás alatt látható **felfelé irányuló szeletek nem áll készen** listája.
5. Kattintson a hello **tevékenységfuttatási** el hello alsó toosee hello listáról **tevékenység fut részletek**.

   ![Tevékenység futtatása részleteit megjelenítő oldalra](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   Például az átviteli sebesség időtartama és hello átjáró használt tootransfer hello adatok információkat mutatunk be.
6. Kattintson a **X** tooclose összes hello lapokat, amíg ki nem
7. vissza a toohello kezdőlapja a hello **ADFTutorialOnPremDF**.
8. (választható) Kattintson a **folyamatok**, kattintson a **ADFTutorialOnPremDF**, és a bemeneti tábla áthatoló részletezést (**felhasznált**) vagy a kimeneti adathalmazokat (**Produced**).
9. Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) tooverify, hogy minden órában létrejön egy blob vagy fájl.

   ![Azure Storage Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Következő lépések
* Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) a következő cikket: az összes hello hello az adatkezelési átjáró adatait.
* Lásd: [adatok másolása az Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) toolearn hogyan toouse másolási tevékenység toomove a forrásadatok adattároló tooa fogadó adattár kapcsolatban.
