---
title: "SQL és a Python, SQL Server adatainak aaaCreate szolgáltatások |} Microsoft Docs"
description: "Folyamat adatokat az SQL Azure-ból"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: 
ms.assetid: bf1f4a6c-7711-4456-beb7-35fdccd46a44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;fashah;garye
ms.openlocfilehash: 223352edb0377a159333e7528ad03c43902e6f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>Funkciók létrehozása az adatokhoz az SQL Serveren SQL és Python használatával
Ez a dokumentum bemutatja, hogyan toogenerate szolgáltatásokat az SQL Server virtuális gép az Azure-on tárolt adatok, algoritmusok segítő további hatékonyabban hello adatokból. Ez végezhető SQL vagy Python, amelyek mindegyikét egy itt hasonló programozási nyelv használatával.

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toocreate-adatok különböző környezetekben funkcióit tootopics. Ez a feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

> [!NOTE]
> Gyakorlati például további részleteket hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) , majd tekintse át a toohello című IPNB [NYC adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) egy végpontok közötti segédlet az.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik:

* Egy Azure storage-fiók létrehozása. Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* SQL Server adatait tárolja. Ha nem, olvassa el [adatok tooan Azure SQL-adatbázis áthelyezése az Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) hogyan toomove hello hiba adatok kapcsolatos utasításokat.

## <a name="sql-featuregen"></a>Az SQL szolgáltatás létrehozása
Ez a szakasz azt módokat SQL funkciók generálása mutatják be:  

1. [Count alapú szolgáltatás létrehozása](#sql-countfeature)
2. [A dobozolás szolgáltatás létrehozása](#sql-binningfeature)
3. [Működés közbeni hello szolgáltatások csak egy oszlop](#sql-featurerollout)

> [!NOTE]
> Létrehozhat további szolgáltatásokat, ha oszlopok toohello meglévő táblaként adja hozzá, vagy hozzon létre egy új tábla hello további funkciók és elsődleges kulcs, hello eredeti tábla lehetne illeszteni.
> 
> 

### <a name="sql-countfeature"></a>Count alapú szolgáltatás létrehozása
Ez a dokumentum bemutatja kétféleképpen száma szolgáltatások generálására. hello első módszer Feltételes összegzés és hello második módszert használja a "where" záradék hello szolgáltatást. Ezek ezután társíthatók hello eredeti (elsődleges kulcs oszlopokat használó) tábla toohave száma szolgáltatásokkal együtt hello eredeti adatok.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>A dobozolás szolgáltatás létrehozása
hello következő példa bemutatja, hogyan toogenerate binned szolgáltatások által a dobozolás (5 bins használatával) egy numerikus oszlopot, amely a ehelyett szolgáltatásként használható:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Működés közbeni hello szolgáltatások csak egy oszlop
Ebben a szakaszban a bemutatjuk, hogyan tooroll kibővített egyetlen oszlop a tábla toogenerate további szolgáltatásokat. hello példa feltételezi, hogy van-e a szélességi és hosszúsági oszlop, amelyből toogenerate szolgáltatások kívánt hello tábla.

Íme egy rövid ismertetése a szélesség/hosszúság helyadatok (a stackoverflow forrásokat `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Ez az featurizing hello a location mező előtt hasznos toounderstand:

* hello bejelentkezési közli nekünk e dolgozunk, déli vagy északi keleti vagy nyugati a hello földgolyó méretét.
* Egy nem nulla több száz számjegy közli a gép hosszúsági és szélességi nem használunk!
* hello több számjegyet biztosít a pozíció tooabout 1000 kilométerben. Milyen kontinensen vagy óceáni dolgozunk a hasznos információkat nyújt nekünk.
* hello egységek számjegy (egy decimális fok) biztosít egy feljebb too111 kilométerben (60 tengeri miles, körülbelül 69 miles). Azt is adja meg, nagyjából milyen nagy állapot vagy a dolgozunk ország.
* hello tizedes jegyre érdemes működik-e too11.1 km: azt is különbözteti meg a szomszédos nagy város egy nagy város hello pozícióját.
* hello második tizedes érdemes működik-e too1.1 km: azt is egy falu eltérő, külön hello mellett.
* hello harmadik tizedes működik-érdemes e too110 m: nagy mezőgazdasági mező vagy intézményi egyetemi is meghatározhatja.
* hello negyedik tizedes működik-érdemes e too11 m: egy is meghatározhatja. Hasonló toohello tipikus pontossága nem zavarja a nem javított GPS egység.
* hello ötödik tizedes működik-érdemes e too1.1 m: azt fák megkülönböztetni egymástól. Pontosság toothis szintjét az kereskedelmi GPS-egységekhez csak különbözeti helyesbítéssel lehet elérni.
* hello hatodik tizedes érdemes működik-e too0.11 m: is használhatja ezt a részletes tájak, tervezéséhez struktúrák elrendezése utak létrehozása. Több mint elég jó glaciers és folyókat követési kell lennie. Ez megvalósítható a GPS, például a differentially javított GPS painstaking intézkedéseket.

hello helyére vonatkozó információkat is lehet featurized következőképpen, régió, a hely és a várost elválasztó. Vegye figyelembe, hogy a többször is meghívhatja a REST-végpont például a Bing térképek API érhető el `https://msdn.microsoft.com/library/ff701710.aspx` tooget hello régió/körzeti információkat.

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

hello fenti alapú szolgáltatások lehet további helyre toogenerate további száma funkciókat használ, a fentebb leírt módon.

> [!TIP]
> Programozott módon szúrhat be a választott nyelven hello rögzíti. Szükség lehet a tooinsert hello adatok adattömbök tooimprove írási hatékonyságát [hello példa bemutatja, hogyan kivétele toodo a pyodbc itt használatával](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).
> Egy másik lehetőség az tooinsert adatai hello adatbázis használatával [BCP segédprogram](https://msdn.microsoft.com/library/ms162802.aspx)
> 
> 

### <a name="sql-aml"></a>Csatlakozás tooAzure gépi tanulás
újonnan létrehozott hello szolgáltatás felvehető táblázatként oszlop tooan meglévő vagy új táblában tárolt és hello eredeti táblázatban a machine Learning szolgáltatáshoz csatlakozik. Szolgáltatások jön létre, vagy használatával érhető el, ha már létrehozott, hello [és adatokat importálhat](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modul Azure ml alább látható módon:

![az azureml-olvasók](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Például a Python-programozási nyelv használatával
Python toogenerate funkciókat használ, amikor hello adatok SQL Server hasonló tooprocessing adatok Azure blob dokumentált Python használatával [Azure Blobadatok folyamat akkor adatok tudományos környezetben](machine-learning-data-science-process-data-blob.md). hello adatok pandas adatok keretbe hello adatbázisból betöltött toobe kell, és majd dolgozhatók további. Azt a dokumentum hello folyamat toohello adatbázis csatlakoztatása és hello adatok betöltését hello adatok keret ebben a szakaszban.

a következő kapcsolati karakterlánc-formátum hello használt tooconnect tooa SQL Server-adatbázis a Python pyodbc (csere kiszolgálónév, dbname, felhasználónevet és jelszót az adott értékek) használatával lehet:

    #Set up hello SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hello [Pandas könyvtár](http://pandas.pydata.org/) Python szolgáltatás széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket ad adatkezelési Python programozáshoz. az alábbi kód hello olvassa hello eredményt Pandas adatok keretbe egy SQL Server-adatbázist vissza:

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Most a témakörök kiterjed hello Pandas adatok keret dolgozhat [létrehozása az Azure blob storage adatok Panda szolgáltatásai](machine-learning-data-science-create-features-blob.md).

