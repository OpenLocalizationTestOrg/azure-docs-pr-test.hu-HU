---
title: "a Machine Learning Studióhoz online adatforrásokból aaaImport adatok |} Microsoft Docs"
description: "Hogyan tooimport a betanítási adatok Azure Machine Learning Studio különböző online forrásokból."
keywords: "adatok, adatformátum, adattípusok, adatforrások, betanítási adatok importálása"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 701b93fe-765b-4d15-a1cf-9b607f17add6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;garye
ms.openlocfilehash: aae6907cdd0b4dc373ae08c2569caa276c198b49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-hello-import-data-module"></a>Adatok importálása az Azure Machine Learning Studio különböző online adatforrásokból származó adatokat hello adatokat importálhat modullal
Ez a cikk ismerteti a különböző forrásokból online adatimportálási hello támogatása, és szükséges e forrásokból toomove adatok hello információk az Azure Machine Learning kísérlet.

> [!NOTE]
> Ez a cikk hello általános információkat nyújt [és adatokat importálhat] [ import-data] modul. Kapcsolatos részletesebb információkért hello típusú adatokat próbál hozzáférni, formátumok, a paraméterek és a válaszok toocommon kérdésekhez lásd: hello modul referencia-témakör hello [és adatokat importálhat] [ import-data] modul.
> 
> 

<!-- -->

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Bevezetés
Hello segítségével [és adatokat importálhat] [ import-data] modulban lehet elérni az adatokat több online adatok forrásokból származó kísérletbe futása [Azure Machine Learning Studio](https://studio.azureml.net/Home):

* Egy webes URL-Címéhez HTTP-n keresztül
* Hadoop HiveQL használatával
* Az Azure blob-tároló
* Azure-tábla
* Az Azure SQL database vagy az SQL Server Azure virtuális gépen
* A helyszíni SQL Server-adatbázis
* Egy adatcsatornát jelenleg OData-szolgáltató
* Az Azure CosmosDB (korábbi nevén DocumentDB)

online adatforrások tooaccess Studio kísérletbe hozzáadása hello [és adatokat importálhat] [ import-data] modul tooyour, jelölje be hello **adatforrás**, majd adja meg a szükséges paramétereket hello tooaccess hello adatokat. az alábbi táblázat hello támogatott hello online adatforrások van felsorolva. Ezt a táblázatot is támogatott fájlformátumok hello foglalja össze, és a használt tooaccess paramétereket hello adatok.

Vegye figyelembe, hogy a betanítási adatok kísérletbe futása közben érhető el, mert az csak érhető el, hogy a kísérletben. A dataset modul tárolt adatok képest, elérhető tooany kísérlet a munkaterületen.

> [!IMPORTANT]
> Jelenleg hello [és adatokat importálhat] [ import-data] és [adatok exportálása] [ export-data] modulok olvashatja, és csak az Azure storage hello létre az adatok írása Klasszikus üzembe helyezési modellben. Ez azt jelenti, az új Azure Blob Storage fióktípus, egy gyakran használt adatok hozzáférési rétege által hello, vagy ritkán használt adatok hozzáférési rétege jelenleg nem támogatott. 
> 
> Általában minden Azure storage-fiókok, hogy előfordulhat, hogy létrehozta előtt a service kapcsoló váltak elérhetővé nem befolyásolja. 
> Ha új fiók toocreate van szüksége, válassza ki a **klasszikus** a hello telepítési modell, vagy erőforrás-kezelővel, és válassza ki **általános célú** helyett **Blob-tároló** a **Fiók kind**. 
> 
> További információkért lásd: [Azure Blob Storage: gyakran és ritkán használt adatok tárolási rétegek](../storage/blobs/storage-blob-storage-tiers.md).
> 
> 

## <a name="supported-online-data-sources"></a>Támogatott online adatforrások
Az Azure Machine Learning **és adatokat importálhat** modul támogatja a következő adatforrások hello:

| Adatforrás | Leírás | Paraméterek |
| --- | --- | --- |
| A HTTP Protokollon keresztül, a webhely URL-címe |Olvassa be az adatokat vesszővel tagolt (CSV), a lapon elválasztott értékeket (TSV), a attribútum-relációs fájlformátumra (ARFF) és a támogatási vektoros gépek (SVM-világos) formátumban, minden webes URL-Címéhez HTTP Protokollt használó |<b>URL-cím</b>: hello fájlnak, például hello webhely URL-címe és hello fájl kiterjesztésű, a teljes név hello határozza meg. <br/><br/><b>Az adatformátum</b>: Megadja a hello támogatott adatok egy formátumok: CSV, TSV, ARFF vagy SVM-világos. Hello fejlécsor tartalmaz, esetén használt tooassign oszlopneveket. |
| Hadoop/HDFS |A Hadoop elosztott tárolókban olvassa be az adatokat. Megadhatja a kívánt HiveQL, egy SQL-szerű lekérdező nyelv használatával hello adatokat. HiveQL is használt tooaggregate adatait és végezhet szűrést hello adatok tooMachine Learning Studio hozzáadása előtt. |<b>Adatbázis-lekérdezés Hive</b>: meghatározza a Hive-lekérdezések hello használt toogenerate hello adatokat.<br/><br/><b>HCatalog kiszolgáló URI azonosítója </b> : a fürt hello formátumban megadott hello nevére  *&lt;a fürt neve&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop felhasználói fiók nevét</b>: hello Hadoop felhasználói fiók nevét használja tooprovision hello fürtön határozza meg.<br/><br/><b>Hadoop felhasználói fiók jelszavát</b> : meghatározza hello hello fürt létesítésekor használt hitelesítő adatokat. További információkért lásd: [Hadoop létrehozása a HDInsight-fürtök](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Az kimeneti adatok</b>: meghatározza, hogy hello tárolja a Hadoop elosztott fájlrendszer (HDFS) vagy az Azure-ban. <br/><ul>Ha a kimeneti adatok HDFS-ben vannak tárolva, adja meg a hello HDFS kiszolgáló URI azonosítója. (Lehet, hogy toouse hello HDInsight fürt neve hello HTTPS:// előtag nélkül). <br/><br/>Ha a kimeneti adatok az Azure-ban tárolja, adjon meg, hogy hello az Azure storage-fiók neve, a tárelérési kulcs és a tárolási tároló nevét.</ul> |
| SQL-adatbázis |Olvassa be az Azure SQL adatbázis vagy egy Azure virtuális gépen futó SQL Server-adatbázisban tárolt adatokat. |<b>Adatbázis-kiszolgáló neve</b>: hello hello mely hello adatbázist futtató kiszolgáló nevét adja meg.<br/><ul>Azure SQL Database esetén adja meg a generált hello kiszolgáló neve. Általában hello űrlap rendelkezik  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Adjon meg egy Azure virtuális gépeken futó SQL server esetén *tcp:&lt;virtuális gép DNS-név&gt;, 1433*</ul><br/><b>Az adatbázisnév </b>: hello adatbázis neve hello hello kiszolgálón ad meg. <br/><br/><b>Felhasználói fiók kiszolgálónév</b>: a hozzáférési engedélyekkel rendelkezik a hello adatbázis fiók felhasználónevét határozza meg. <br/><br/><b>Kiszolgáló felhasználói fiók jelszavát</b>: hello hello felhasználói fiók jelszavát adja meg.<br/><br/><b>Fogadja el a kiszolgálói tanúsítvány</b>: használja ezt a beállítást (kevésbé biztonságos), ha azt szeretné, hogy az adatok olvasása előtt tekintse át a hello webhelytanúsítvány tooskip.<br/><br/><b>Adatbázis-lekérdezés</b>: Adjon meg egy SQL-utasítást, amely leírja a tooread kívánt hello adatokat. |
| A helyszíni SQL-adatbázis |A helyszíni SQL-adatbázisban tárolt adatok beolvasása. |<b>Az átjáró</b>: hello az adatkezelési átjáró egy számítógépre, ahol az SQL Server adatbázis férhetnek telepített hello nevét adja meg. Hello átjáró beállításával kapcsolatos információkért lásd: [advanced analytics egy helyszíni SQL server adatait használó Azure Machine Learning segítségével végezze el](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Adatbázis-kiszolgáló neve</b>: hello hello mely hello adatbázist futtató kiszolgáló nevét adja meg.<br/><br/><b>Az adatbázisnév </b>: hello adatbázis neve hello hello kiszolgálón ad meg. <br/><br/><b>Felhasználói fiók kiszolgálónév</b>: a hozzáférési engedélyekkel rendelkezik a hello adatbázis fiók felhasználónevét határozza meg. <br/><br/><b>Felhasználónév és jelszó</b>: kattintson a <b>adja meg az értékeket</b> tooenter adatbázis hitelesítő adatait. Használhatja az integrált Windows-hitelesítést vagy SQL Server-hitelesítés a helyszíni SQL Server konfigurációtól függően.<br/><br/><b>Adatbázis-lekérdezés</b>: Adjon meg egy SQL-utasítást, amely leírja a tooread kívánt hello adatokat. |
| Azure-tábla |Table szolgáltatás az Azure Storage hello olvassa be az adatokat.<br/><br/>Ha nagy mennyiségű adat ritkán olvasni, használja a hello Azure Table szolgáltatás. Biztosít egy rugalmas, nem relációs (NoSQL), nagymértékben méretezhető, alacsony költségű és magas rendelkezésre állású tárolási megoldást. |beállítások a hello hello **és adatokat importálhat** attól függően, hogy nyilvános információt vagy a privát storage-fiók bejelentkezési hitelesítő adatokat igénylő elérése módosítása. Ez határozza meg hello <b>hitelesítési típus</b> amelyben lehetnek értéke "PublicOrSAS" vagy "Fiók", amelyek mindegyikének saját beállítása olyan paraméterek összessége. <br/><br/><b>Nyilvános vagy közös hozzáférésű Jogosultságkód (SAS) URI</b>: hello paraméterek:<br/><br/><ul><b>Tábla URI</b>: hello nyilvános vagy SAS URL-cím megadása hello tábla.<br/><br/><b>Megadja a tulajdonságnevek hello sorok tooscan</b>: hello értékek a következők <i>TopN</i> tooscan hello megadott sorok, vagy <i>ScanAll</i> tooget az összes olyan sort hello táblában. <br/><br/>Ha hello adatok homogén és előre jelezhető, javasoljuk, hogy kiválassza *TopN* és N. meg Nagy táblák Ez gyorsabb olvasási idő eredményezhet.<br/><br/>Ha hello adatokat úgy vannak felépítve, különböző hello mélysége és pozíciója hello tábla az alapján, amelyek módosítják a tulajdonságokat, válassza ki azt a hello *ScanAll* tooscan lehetőséget az összes sort. Ez biztosítja, hogy az eredményül kapott tulajdonság és a metaadatok átalakítás hello integritása.<br/><br/></ul><b>Saját Tárfiók</b>: hello paraméterek: <br/><br/><ul><b>Fióknév</b>: hello hello fiók neve, amely tartalmazza a hello tábla tooread határozza meg.<br/><br/><b>A fiókkulcsot</b>: hello fiókhoz társított hello biztonságitár-kulcs határozza meg.<br/><br/><b>Táblanév</b> : hello adatok tooread tartalmazó tábla hello hello nevét adja meg.<br/><br/><b>A tulajdonságnevek sorok tooscan</b>: hello értékek a következők <i>TopN</i> tooscan hello megadott sorok, vagy <i>ScanAll</i> tooget az összes olyan sort hello táblában.<br/><br/>Ha hello adatok homogén és előre jelezhető, azt javasoljuk, hogy kiválassza *TopN* és N. meg Nagy táblák Ez gyorsabb olvasási idő eredményezhet.<br/><br/>Ha hello adatokat úgy vannak felépítve, különböző hello mélysége és pozíciója hello tábla az alapján, amelyek módosítják a tulajdonságokat, válassza ki azt a hello *ScanAll* tooscan lehetőséget az összes sort. Ez biztosítja, hogy az eredményül kapott tulajdonság és a metaadatok átalakítás hello integritása.<br/><br/> |
| Azure Blob Storage |Az Azure Storage, beleértve a képeket, strukturálatlan szöveges vagy bináris adatok hello Blob szolgáltatásban tárolt adatokat olvas.<br/><br/>Hello szolgáltatás toopublicly teszi közzé a blobadatokat, vagy tooprivately tároló alkalmazásadatok is használhatja. Hozzáférhet az adatokhoz, bárhonnan HTTP vagy HTTPS-kapcsolatokon keresztül. |hello beállítások hello **és adatokat importálhat** modul módosítás attól függően, hogy nyilvános információt vagy a privát storage-fiók bejelentkezési hitelesítő adatokat igénylő elérésére. Ez határozza meg hello <b>hitelesítési típus</b> amely "PublicOrSAS" vagy "Fiók" értéke lehet.<br/><br/><b>Nyilvános vagy közös hozzáférésű Jogosultságkód (SAS) URI</b>: hello paraméterek:<br/><br/><ul><b>URI</b>: hello tárolási blob hello nyilvános vagy SAS URL-CÍMÉT határozza meg.<br/><br/><b>Fájlformátum</b>: hello adatok formátuma hello hello Blob szolgáltatás határozza meg. hello támogatott formátumok a következők: CSV TSV és ARFF.<br/><br/></ul><b>Saját Tárfiók</b>: hello paraméterek: <br/><br/><ul><b>Fióknév</b>: hello hello fiók neve, amely tartalmazza azt szeretné, hogy tooread hello blob határozza meg.<br/><br/><b>A fiókkulcsot</b>: hello fiókhoz társított hello biztonságitár-kulcs határozza meg.<br/><br/><b>Elérési út toocontainer, könyvtár vagy a blob </b> : hello blob hello adatok tooread tartalmazó hello nevét adja meg.<br/><br/><b>A BLOB Formátum</b>: hello adatok formátuma hello hello blob szolgáltatás határozza meg. hello támogatott adatok formátumok a következők CSV, TSV, ARFF, fürt megosztott kötetei szolgáltatás a megadott kódolás és az Excel alkalmazásban. <br/><br/><ul>Ha hello formátum CSV vagy TSV, hogy meg arról, hogy tooindicate e hello fájl tartalmazza a fejlécsor.<br/><br/>Hello Excel beállítás tooread adatok az Excel-munkafüzetek is használhatja. A hello <i>Excel adatformátum</i> lehetőséget, adja meg, hogy hello adatok egy Excel-munkalap esik, vagy az Excel-táblázat van-e. A hello <i>Excel-táblában vagy beágyazott tábla </i>lehetőségre, adja meg a hello lap vagy az tooread kívánt hello nevét.</ul><br/> |
| Adatcsatorna-szolgáltató |A támogatott adatcsatorna szolgáltató olvassa be az adatokat. Jelenleg csak hello nyitott Data (OData) protokollnak formátum támogatott. |<b>Adatok tartalomtípus</b>: hello OData-formátum.<br/><br/><b>Forrás URL-címe</b>: hello teljes URL-címet hello adatcsatorna határozza meg. <br/>Például a következő URL-cím olvasások hello adatbázisunk a hello: http://services.odata.org/northwind/northwind.svc/ |

## <a name="next-steps"></a>Következő lépések

[Adatok importálása és az adatok exportálása modulokat használó Azure ML web szolgáltatások telepítése](machine-learning-web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
