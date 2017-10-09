---
title: "Speciális elemzés forgatókönyvek az Azure Machine Learning aaaIdentify |} Microsoft Docs"
description: "Válassza ki a megfelelő forgatókönyvek hello speciális hello csapat az tudományos folyamata a prediktív elemzés megteheti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 53aecc1e-5089-42cf-8d44-77678653f92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 52c6bb10d6df4f640a4f66cf17cf4993cc1067b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Speciális elemzési forgatókönyvek az Azure Machine Learning rendszerben
Ez a cikk ismerteti a minta adatforrások és a cél-szolgáltatásokat, amelyek kezelhetik hello hello számos [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md). hello TDSP rendszeres megközelítését ismerteti a csapatok toocollaborate az intelligens alkalmazások létrehozásához. Itt bemutatott hello forgatókönyvek bemutatására használható lehetőségekről hello adatfeldolgozási munkafolyamat szerelvényfájljaitól hello adatjellemzők, a Forráshelyek és a cél tárházak találhatók, az Azure-ban.

Hello **döntési fa** a hello szituáció, amely megfelelő-e az adatok és a cél kiválasztása számára jelenik meg a hello utolsó szakaszában.

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

A következő szakaszok hello mindegyikét megadja egy mintaforgatókönyv. Az egyes forgatókönyvek esetében, egy lehetséges adattudomány vagy speciális elemzésekre folyamata és a támogató Azure-erőforrások találhatók.

> [!NOTE]
> **Az összes hello a következő esetekben kell:**
> <br/>
> 
> * [A storage-fiók létrehozása](../storage/common/storage-create-storage-account.md)
>   <br/>
> * [Az Azure Machine Learning-munkaterület létrehozása](machine-learning-create-workspace.md)
> 
> 

## <a name="smalllocal"></a>A forgatókönyv \#1: kicsi toomedium táblázatos adatkészlet egy helyi fájlok
![Kis toomedium helyi fájlok][1]

#### <a name="additional-azure-resources-none"></a>További Azure-erőforrások: nincs
1. Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
2. Töltse fel a DataSet adatkészlet.
3. Hozzon létre egy Azure Machine Learning kísérlet folyamat feltöltött adatkészlet(ek) kezdve.

## <a name="smalllocalprocess"></a>A forgatókönyv \#2: a helyi fájlok feldolgozást igénylő kis toomedium adatkészlet
![Kis toomedium helyi fájlok feldolgozása][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>További Azure-erőforrások: Azure virtuális gép (IPython Notebook kiszolgáló)
1. Hozzon létre egy Azure virtuális gépen futó IPython Notebook.
2. Töltse fel az adatok tooan az Azure storage-tároló.
3. Előre feldolgozzák a, és az adatok elérése az Azure storage tárolóból IPython Notebook adatait.
4. Alakítsa át az adatokat toocleaned, táblázatos formában.
5. Az Azure-blobokat átalakított adatok mentése.
6. Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
7. Hello adatokat olvasni az Azure BLOB hello segítségével [és adatokat importálhat] [ import-data] modul.
8. Hozzon létre egy Azure Machine Learning kísérlet folyamat feldolgozott adatkészlet(ek) kezdve.

## <a name="largelocal"></a>A forgatókönyv \#3: a helyi fájloknak, Azure-Blobokkal célzó nagy adatkészlet
![Nagy helyi fájlok][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>További Azure-erőforrások: Azure virtuális gép (IPython Notebook kiszolgáló)
1. Hozzon létre egy Azure virtuális gépen futó IPython Notebook.
2. Töltse fel az adatok tooan az Azure storage-tároló.
3. Előre feldolgozzák, és az adatok elérése az Azure-blobokat IPython Notebook adatait.
4. Ha szükséges. az átalakító adatok toocleaned, táblázatos formában.
5. Adatokba, és szükség szerint funkciók létrehozása.
6. Bontsa ki a kis és közepes méretű mintáját.
7. Az Azure-blobokat mintát hello adatok mentése.
8. Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Hello adatokat olvasni az Azure BLOB hello segítségével [és adatokat importálhat] [ import-data] modul.
10. Build Azure Machine Learning kísérlet folyamata feldolgozott adatkészlet(ek) kezdve.

## <a name="smalllocaltodb"></a>A forgatókönyv \#4: a helyi fájlok, SQL Server egy Azure virtuális gép célzó kis toomedium adatkészlet
![Kis toomedium helyi fájlok tooSQL DB az Azure-ban][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)
1. Hozzon létre egy Azure virtuális gépen fut az SQL-kiszolgáló + IPython Notebookot.
2. Töltse fel az adatok tooan az Azure storage-tároló.
3. Előre feldolgozzák, és az Azure storage tárolóban IPython Notebook használatával adatait.
4. Ha szükséges. az átalakító adatok toocleaned, táblázatos formában.
5. Adatok tooVM helyi fájlokat (IPython Notebook fut a virtuális gép tudnivalókat a helyi meghajtókra tooVM meghajtókat).
6. Terheléselosztási adatok tooSQL Server-adatbázis egy Azure virtuális gépen.
   
   A beállítás \#1: SQL Server Management Studio használatával.
   
   * Bejelentkezési tooSQL kiszolgálói virtuális gép
   * Futtassa az SQL Server Management Studio eszközt.
   * Adatbázis és a célként megadott táblák létrehozása.
   * Hello tömeges egyikét módszerek tooload hello adatok importálása a virtuális gép helyi fájlokból.
   
   A beállítás \#2: használatával IPython Notebook – közepes és nagyobb adatkészletek esetében nem ajánlott
   
   <!-- -->    
   * ODBC kapcsolati karakterlánc tooaccess SQL Server használata virtuális gépeken.
   * Adatbázis és a célként megadott táblák létrehozása.
   * Hello tömeges egyikét módszerek tooload hello adatok importálása a virtuális gép helyi fájlokból.
7. Adatokba, és szolgáltatások igény szerint. Vegye figyelembe, hogy hello szolgáltatások nem szükséges a hello adatbázistáblák materializált toobe. Csak vegye figyelembe a hello szükséges lekérdezés toocreate őket.
8. Döntse el, az adatok mintájának méretét, ha szükséges, és/vagy a szükséges.
9. Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
10. Olvasási hello adatok közvetlenül a hello hello használata az SQL Server [és adatokat importálhat] [ import-data] modul. Beillesztés hello szükséges lekérdezés mezők, amely szolgáltatások hoz létre, és minták adatokat, ha közvetlenül a hello szükséges [és adatokat importálhat] [ import-data] lekérdezés.
11. Build Azure Machine Learning kísérlet folyamata feldolgozott adatkészlet(ek) kezdve.

## <a name="largelocaltodb"></a>A forgatókönyv \#5: a helyi fájlok nagy dataset cél SQL Server Azure virtuális gépen
![Nagy helyi fájlok tooSQL DB az Azure-ban][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)
1. Hozzon létre egy Azure virtuális gépen futó SQL Server IPython Notebook kiszolgáló.
2. Töltse fel az adatok tooan az Azure storage-tároló.
3. (Választható) Előre feldolgozzák és adatait.
   
   a.  Előre feldolgozzák és adatait IPython jegyzetfüzet adatok elérése az Azure-ból
   
       blobs.
   
   b.  Ha szükséges. az átalakító adatok toocleaned, táblázatos formában.
   
   c.  Adatok tooVM helyi fájlokat (IPython Notebook fut a virtuális gép tudnivalókat a helyi meghajtókra tooVM meghajtókat).
4. Terheléselosztási adatok tooSQL Server-adatbázis egy Azure virtuális gépen.
   
   a.  Bejelentkezési tooSQL kiszolgálói virtuális gép.
   
   b.  Ha az adatok nem már mentve, adatfájlok le az Azure-ból
   
       storage container toolocal-VM folder.
   
   c.  Futtassa az SQL Server Management Studio eszközt.
   
   d.  Adatbázis és a célként megadott táblák létrehozása.
   
   e.  Hello tömeges egyikét módszerek tooload hello adatok importálása.
   
   f.  Ha táblákra szükség, hozzon létre indexek tooexpedite illesztéseket.
   
   > [!NOTE]
   > Nagy adatmennyiség gyorsabb betöltését, ajánlott particionált táblák létrehozása, és tömeges hello adatokat importálhat párhuzamosan. További információkért lásd: [párhuzamos importálhat tooSQL particionált táblák](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Adatokba, és szolgáltatások igény szerint. Vegye figyelembe, hogy hello szolgáltatások nem szükséges a hello adatbázistáblák materializált toobe. Csak vegye figyelembe a hello szükséges lekérdezés toocreate őket.
6. Döntse el, az adatok mintájának méretét, ha szükséges, és/vagy a szükséges.
7. Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
8. Olvasási hello adatok közvetlenül a hello hello használata az SQL Server [és adatokat importálhat] [ import-data] modul. Beillesztés hello szükséges lekérdezés mezők, amely szolgáltatások hoz létre, és minták adatokat, ha közvetlenül a hello szükséges [és adatokat importálhat] [ import-data] lekérdezés.
9. Egyszerű Azure Machine Learning kísérlet folyamata feltöltött dataset kezdődően

## <a name="largedbtodb"></a>A forgatókönyv \#6: nagy adatkészlet egy SQL Server adatbázis a helyszínen, célzás SQL Server egy Azure virtuális gépen
![Nagy SQL-adatbázis a helyszíni tooSQL DB az Azure-ban][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)
1. Hozzon létre egy Azure virtuális gépen futó SQL Server IPython Notebook kiszolgáló.
2. Használja az adatok hello módszerek tooexport hello adatok exportálása az SQL Server toodump fájlok.
   
   > [!NOTE]
   > Ha úgy dönt, toomove hello helyszíni adatbázisban, egy másik (gyorsabb) metódus toomove hello adatbázis teljes toothe SQL Server-példányra az Azure-ban minden adatát. Hello lépéseket tooexport adatok kihagyása, adatbázis, és a betöltés/importálási adatok toohello céladatbázis létrehozása, és kövesse a hello alternatív módszert.
   > 
   > 
3. Töltse fel a memóriakép fájlokhoz tooAzure tároló.
4. Betöltési hello adatok tooa SQL Server-adatbázis egy Azure virtuális gépet futtat.
   
   a.  Bejelentkezési toohello SQL Server virtuális gép.
   
   b.  Adatfájlok le az Azure storage tároló toohello helyi-VM mappából.
   
   c.  Futtassa az SQL Server Management Studio eszközt.
   
   d.  Adatbázis és a célként megadott táblák létrehozása.
   
   e.  Hello tömeges egyikét módszerek tooload hello adatok importálása.
   
   f.  Ha táblákra szükség, hozzon létre indexek tooexpedite illesztéseket.
   
   > [!NOTE]
   > Nagy adatmennyiség gyorsabb betöltését, hozzon létre a particionált táblákat és toobulk hello és adatokat importálhat párhuzamosan. További információkért lásd: [párhuzamos importálhat tooSQL particionált táblák](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Adatokba, és szolgáltatások igény szerint. Vegye figyelembe, hogy hello szolgáltatások nem szükséges a hello adatbázistáblák materializált toobe. Csak vegye figyelembe a hello szükséges lekérdezés toocreate őket.
6. Döntse el, az adatok mintájának méretét, ha szükséges, és/vagy a szükséges.
7. Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
8. Olvasási hello adatok közvetlenül a hello hello használata az SQL Server [és adatokat importálhat] [ import-data] modul. Beillesztés hello szükséges lekérdezés mezők, amely szolgáltatások hoz létre, és minták adatokat, ha közvetlenül a hello szükséges [és adatokat importálhat] [ import-data] lekérdezés.
9. Egyszerű Azure Machine Learning kísérlet folyamata feltöltött dataset kezdve.

### <a name="alternate-method-toocopy-a-full-database-from-an-on-premises--sql-server-tooazure-sql-database"></a>Alternatív módszert toocopy a teljes adatbázis egy helyi SQL Server tooAzure SQL-adatbázis
![Válassza le a helyi adatbázis, és csatlakoztassa tooSQL DB az Azure-ban][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)
tooreplicate hello teljes SQL Server-adatbázishoz az SQL Server virtuális gép, másolja egy adatbázist a hálózatihely-kiszolgáló egy tooanother, feltéve, hogy hello az adatbázis átmenetileg offline állapotú átvihető. Ez az SQL Server Management Studio Object Explorerben hello, vagy hello egyenértékű Transact-SQL-parancsok használatával teheti meg.

1. Hello forrás helyen hello adatbázis leválasztásához. További információkért lásd: [egy adatbázis leválasztásához](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).
2. Windows Explorer vagy a Windows parancssori ablakban a Másolás hello adatbázisfájl vagy a fájl és a naplófájl vagy a fájlok toohello célhelyet az SQL Server Azure-ban hello le.
3. Hello másolt fájlok toohello cél SQL Server-példányhoz csatolja. További információkért lásd: [adatbázis csatolása](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).

[Helyezze át egy adatbázis használatával válassza le, és csatolja (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)

## <a name="largedbtohive"></a>A forgatókönyv \#7: helyi fájlok Big Data típusú adatok cél Hive-adatbázisban az Azure HDInsight Hadoop-fürtök
![A helyi cél Hive big Data típusú adatok][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>További Azure-erőforrások: az Azure HDInsight Hadoop-fürt és az Azure virtuális gép (IPython Notebook kiszolgáló)
1. Hozzon létre egy IPython Notebook Servert futtató Azure virtuális gépen.
2. Hozzon létre egy Azure HDInsight Hadoop-fürt.
3. (Választható) Előre feldolgozzák és adatait.
   
   a.  Előre feldolgozzák és adatait IPython jegyzetfüzet adatok elérése az Azure-ból
   
       blobs.
   
   b.  Ha szükséges. az átalakító adatok toocleaned, táblázatos formában.
   
   c.  Adatok tooVM helyi fájlokat (IPython Notebook fut a virtuális gép tudnivalókat a helyi meghajtókra tooVM meghajtókat).
4. Töltse fel az adatok toohello alapértelmezett tároló hello Hadoop-fürt a hello 2.
5. Terheléselosztási adatok tooHive adatbázis Azure HDInsight Hadoop-fürt.
   
   a.  Jelentkezzen be toohello hello Hadoop-fürt átjárócsomópontjához
   
   b.  Hello Hadoop parancssor megnyitásához.
   
   c.  Adja meg a Hive gyökérkönyvtár hello parancs `cd %hive_home%\bin` Hadoop parancssor futtatása.
   
   d.  Futtassa a hello Hive-lekérdezések toocreate adatbázis és a táblák, és adatok betöltése az blob storage-tooHive táblákat.
   
   > [!NOTE]
   > Ha hello adatok nagy, a felhasználók létrehozhatják hello Hive tábla partíciókat. Ezt követően a felhasználók használhatják a `for` hello átjárócsomópont tooload adatok hello Hive tábla particionálva partíció által a Hadoop parancssori hello hurkot.
   > 
   > 
6. Adatokba, és szükség esetén a Hadoop parancssori funkciók létrehozása. Vegye figyelembe, hogy hello szolgáltatások nem szükséges a hello adatbázistáblák materializált toobe. Csak vegye figyelembe a hello szükséges lekérdezés toocreate őket.
   
   a.  Jelentkezzen be toohello hello Hadoop-fürt átjárócsomópontjához
   
   b.  Hello Hadoop parancssor megnyitásához.
   
   c.  Adja meg a Hive gyökérkönyvtár hello parancs `cd %hive_home%\bin` Hadoop parancssor futtatása.
   
   d.  A hello átjárócsomópontjához hello Hadoop-fürt tooexplore hello adatokat a Hadoop parancssor futtatása a hello Hive-lekérdezéseket, és igény szerint funkciók létrehozása.
7. Ha szükséges, és/vagy szükséges, mintát hello adatok toofit az Azure Machine Learning Studióban.
8. Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Hello adatok közvetlenül olvassák be hello `Hive Queries` hello segítségével [és adatokat importálhat] [ import-data] modul. Beillesztés hello szükséges lekérdezés mezők, amely szolgáltatások hoz létre, és minták adatokat, ha közvetlenül a hello szükséges [és adatokat importálhat] [ import-data] lekérdezés.
10. Egyszerű Azure Machine Learning kísérlet folyamata feltöltött dataset kezdve.

## <a name="decisiontree"></a>A forgatókönyv kiválasztása döntési fája
- - -
hello következő diagram foglalja össze a fent leírt hello forgatókönyvek és hello Advanced Analytics folyamat és a technológia választások tévő tooeach részletezett hello forgatókönyvek. Vegye figyelembe, hogy az adatok feldolgozása, a feltárása, a szolgáltatás műszaki osztály és a mintavételi is igénybe vehet egy vagy több metódus/környezeti – hello forrás, a köztes, és/vagy a cél környezetekben – helyezze, és szükség szerint ismételt folytathatja. hello diagram csak néhány lehetséges adatfolyamok szemléltetésére szolgál, és nem biztosít teljes körű enumerálást.

![A minta DS folyamat bemutató forgatókönyvek][8]

### <a name="advanced-analytics-in-action-examples"></a>Speciális elemzés a művelet példák
A végpont Azure Machine Learning forgatókönyvek alkalmazó szoftverbiztonsági hello Advanced Analytics folyamat és a technológia használatával nyilvános adatkészleteket, lásd:

* [Vonja össze az adatokat tudományos folyamat működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).
* [Vonja össze az adatokat tudományos folyamat működés közben: HDInsight Hadoop-fürtök használata](machine-learning-data-science-process-hive-walkthrough.md).

[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
