---
title: "az Azure SQL Server virtuális gépen aaaExplore adatok |} Microsoft Docs"
description: "Adatokba és szolgáltatások létrehozása az Azure SQL Server virtuális gépen"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: 
ms.assetid: 3949fb2c-ffab-49fb-908d-27d5e42f743b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 67f4b058b0f6557ee15fd42795c918d68f1a9871
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="heading"></a>Az SQL Server virtuális gépet az Azure adatfeldolgozásra
Ez a dokumentum hogyan tooexplore adatok és az SQL Server virtuális gép az Azure-on tárolt adatok szolgáltatásai generálásához. Ezt adatok wrangling SQL használatával, és Python hasonló programozási nyelv használatával is végrehajthatja.

> [!NOTE]
> Ez a dokumentum hello minta SQL-utasítások feltételezik, hogy adatokat az SQL Server. Ha nem, olvassa el az toohello felhő adatok tudományos folyamat térkép toolearn hogyan toomove az adatok tooSQL kiszolgáló.
> 
> 

## <a name="SQL"></a>SQL használatával
Azt írja le a hello wrangling feladatok ebben a szakaszban az SQL a következő:

1. [Az adatok feltárása](#sql-dataexploration)
2. [Szolgáltatás létrehozása](#sql-featuregen)

### <a name="sql-dataexploration"></a>Az adatok feltárása
Íme néhány példa SQL-parancsfájlok, amely az SQL Server használt tooexplore adattárolókhoz.

> [!NOTE]
> Gyakorlati például használhatja hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) , majd tekintse át a toohello című IPNB [NYC adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) egy végpontok közötti segédlet az.
> 
> 

1. Napi megfigyeléseket hello számbavétele
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Hello szintek kategorikus oszlopban beolvasása
   
    `select  distinct <column_name> from <databasename>`
3. Hello szintek száma kombinációja kategorikus kétoszlopos lekérése 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Hello terjesztési numerikus oszlopok az beszerzése
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <a name="sql-featuregen"></a>Szolgáltatás létrehozása
Ez a szakasz azt módokat SQL funkciók generálása mutatják be:  

1. [Count alapú szolgáltatás létrehozása](#sql-countfeature)
2. [A dobozolás szolgáltatás létrehozása](#sql-binningfeature)
3. [Működés közbeni hello szolgáltatások csak egy oszlop](#sql-featurerollout)

> [!NOTE]
> Létrehozhat további szolgáltatásokat, ha oszlopok toohello meglévő táblaként adja hozzá, vagy hozzon létre egy új tábla hello további funkciók és elsődleges kulcs, hello eredeti tábla lehetne illeszteni. 
> 
> 

### <a name="sql-countfeature"></a>Count alapú szolgáltatás létrehozása
hello alábbi példák bemutatják, kétféle módon száma szolgáltatások létrehozásakor. hello első módszer Feltételes összegzés és hello második módszert használja a "where" záradék hello szolgáltatást. Ezek ezután társíthatók hello eredeti (elsődleges kulcs oszlopokat használó) tábla toohave száma szolgáltatásokkal együtt hello eredeti adatok.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <a name="sql-binningfeature"></a>A dobozolás szolgáltatás létrehozása
hello következő példa bemutatja, hogyan toogenerate binned szolgáltatások által a dobozolás (használatával öt bins) egy numerikus oszlopot, amely a ehelyett szolgáltatásként használható:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Működés közbeni hello szolgáltatások csak egy oszlop
Ebben a szakaszban a bemutatjuk, hogyan tooroll csak egy oszlop a tábla toogenerate további szolgáltatások ki. hello példa feltételezi, hogy van-e a szélességi és hosszúsági oszlop, amelyből toogenerate szolgáltatások kívánt hello tábla.

Íme egy rövid ismertetése a szélesség/hosszúság helyadatok (a stackoverflow forrásokat [hogyan toomeasure hello szélességi és hosszúsági pontosságát?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Ez az featurizing hello a location mező előtt hasznos toounderstand:

* hello bejelentkezési közli nekünk e dolgozunk, déli vagy északi keleti vagy nyugati a hello földgolyó méretét.
* Egy nem nulla több száz számjegy közli velünk, hogy használjuk-e hosszúsági és szélességi nem!
* hello több számjegyet biztosít a pozíció tooabout 1000 kilométerben. Milyen kontinensen vagy óceáni dolgozunk a hasznos információkat nyújt nekünk.
* hello egységek számjegy (egy decimális fok) biztosít egy feljebb too111 kilométerben (60 tengeri miles, körülbelül 69 miles). Azt is adja meg, nagyjából milyen nagy állapot vagy a dolgozunk ország.
* hello tizedes jegyre érdemes működik-e too11.1 km: azt is különbözteti meg a szomszédos nagy város egy nagy város hello pozícióját.
* hello második tizedes érdemes működik-e too1.1 km: azt is egy falu eltérő, külön hello mellett.
* hello harmadik tizedes működik-érdemes e too110 m: nagy mezőgazdasági mező vagy intézményi egyetemi is meghatározhatja.
* hello negyedik tizedes működik-érdemes e too11 m: egy is meghatározhatja. Hasonló toohello tipikus pontossága nem zavarja a nem javított GPS egység.
* hello ötödik tizedes működik-érdemes e too1.1 m: azt fák különbözteti meg egymástól. Pontosság toothis szintjét az kereskedelmi GPS-egységekhez csak különbözeti helyesbítéssel lehet elérni.
* hello hatodik tizedes érdemes működik-e too0.11 m: is használhatja ezt a részletes tájak, tervezéséhez struktúrák elrendezése utak létrehozása. Több mint elég jó glaciers és folyókat követési kell lennie. Ez megvalósítható a GPS, például a differentially javított GPS painstaking intézkedéseket.

hello helyére vonatkozó információkat is a következők featurized, régió, a hely és a várost elválasztó. Vegye figyelembe, hogy Ön is meghívhatja a REST-végpont például a Bing térképek API érhető el [pont hely keresése](https://msdn.microsoft.com/library/ff701710.aspx) tooget hello régió/körzeti információkat.

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

Ezeket a helyalapú szolgáltatásokat lehet további használt toogenerate további száma szolgáltatások ismertetett módon. 

> [!TIP]
> Programozott módon szúrhat be a választott nyelven hello rögzíti. Szükség lehet az adattömbök tooimprove írási hatékonyságát tooinsert hello adatokat (a példa bemutatja, hogyan toodo a pyodbc használata, lásd: [A HelloWorld minta tooaccess SQLServer python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Egy másik lehetőség az tooinsert adatai hello adatbázisban hello használatával [BCP segédprogram](https://msdn.microsoft.com/library/ms162802.aspx).
> 
> 

### <a name="sql-aml"></a>Csatlakozás tooAzure gépi tanulás
újonnan létrehozott hello szolgáltatás felvehető táblázatként oszlop tooan meglévő vagy új táblában tárolt és hello eredeti táblázatban a machine Learning szolgáltatáshoz csatlakozik. Szolgáltatások jön létre, vagy használatával érhető el, ha már létrehozott, hello [és adatokat importálhat] [ import-data] modul az Azure Machine Learning alább látható módon:

![az azureml-olvasók][1] 

## <a name="python"></a>Például a Python-programozási nyelv használatával
Python tooexplore adatokkal, és generáljon szolgáltatásokat, ha hello adatok SQL Server olyan hasonló tooprocessing adatok dokumentált Python használatával az Azure BLOB [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md). hello adatok pandas adatok keretbe hello adatbázisból betöltött toobe kell, és majd dolgozhatók további. Azt a dokumentum hello folyamat toohello adatbázis csatlakoztatása és hello adatok betöltését hello adatok keret ebben a szakaszban.

a következő kapcsolati karakterlánc-formátum hello használt tooconnect tooa SQL Server-adatbázis a Python pyodbc (csere kiszolgálónév, dbname, a felhasználónevet és jelszót az adott értékek) használatával lehet:

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hello [Pandas könyvtár](http://pandas.pydata.org/) Python szolgáltatás széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket ad adatkezelési Python programozáshoz. az alábbi kód hello olvassa hello eredményt Pandas adatok keretbe egy SQL Server-adatbázist vissza:

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Most, együttműködhet hello Pandas adatok keret hello cikkben szereplő [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Az Azure Data tudományos művelet példa
Hello Azure adatok tudományos folyamat egy nyilvános adatkészlet-végpontok közötti bemutató példa: [Azure adatok tudományos folyamat működés közben](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

