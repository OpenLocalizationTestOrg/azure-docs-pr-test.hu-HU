---
title: "aaaHadoop összetevők és verziók - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hello Hadoop-összetevők és a HDInsight és hello szolgáltatási szintek érhető el a felhőalapú terjesztési Hortonworks Data platform verziók."
keywords: "hadoop verziók, a hadoop-ökoszisztémával összetevők, a hadoop-összetevők, hogyan toocheck hadoop verziója"
services: hdinsight
editor: cgronlun
manager: asadk
author: bprakash
tags: azure-portal
documentationcenter: 
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: bprakash
ms.openlocfilehash: b661d901b0113458c3501ec06454fc8841189672
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-hello-hadoop-components-and-versions-available-with-hdinsight"></a>Mik azok a hello Hadoop-összetevők és a hdinsight eszközzel verziók?

Hello Apache Hadoop-ökoszisztémával összetevők és a Microsoft Azure hdinsight verziókkal kapcsolatos tudnivalók, valamint Standard és Premium szolgáltatásszintek hello. Emellett ismerje meg, hogyan toocheck Hadoop összetevő verziók a hdinsight eszközben. 

Minden HDInsight-verzió egy felhőalapú terjesztési verziójának Hortonworks Data Platform (HDP).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>HDInsight különböző verzióiban Hadoop-összetevők
Az Azure HDInsight Hadoop fürt több verziója, amely bármikor telepíthető támogatja. Minden egyes verzió choice hoz létre, egy adott verziójához hello HDP telepítési és összetevők belüli, hogy a terjesztési. 2017. február 17., frissítésétől hello Azure HDInsight által használt alapértelmezett fürt verzió 3.5-ös és HDP 2.5 alapul.

HDInsight-fürt verziókról társított hello összetevő verziók hello a következő táblázatban láthatók. 

> [!NOTE]
> hello alapértelmezett verziója a HDInsight-szolgáltatás hello minden külön értesítés nélkül változhatnak. Ha verzió függőség, ha hello .NET SDK-val Azure PowerShell és az Azure parancssori felület a fürt létrehozásához adja meg hello HDInsight-verzió.

| Összetevő | HDInsight 3.6 (alapértelmezett) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | A HDInsight 3.2. | HDInsight 3.1 | A HDInsight 3.0 |
| --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks Data Platform |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop és a YARN |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive és HCatalog |1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez Hive2 | 0.8.4 |-|-|-|-|-|-|
| Apache Pletyka | 0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.1.0 (csak Linux) |1.6.2 + 2.0 (csak Linux) |1.6.0 (csak Linux) |1.5.2 (Linux kísérleti build csak) |1.3.1 (csak Windows) |-|-|
| Apache Kafka | 0.10.0 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.5.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|-|
| Monó |4.2.1 |4.2.1 |3.2.8 |-|-|-|

## <a name="check-for-current-hadoop-component-version-information"></a>Ellenőrizze a jelenlegi Hadoop összetevő verzióinformáció

hello Hadoop ökoszisztémájának összetevő társított verziók HDInsight-fürt verziókról módosíthatja a frissítések tooHDInsight. toocheck hello Hadoop-összetevők és tooverify mely verzióit használatban van egy fürt használja hello Ambari REST API-t. Hello **GetComponentInformation** parancs segítségével lekérdezhető szolgáltatás-összetevőivel kapcsolatos információk. További információkért lásd: hello [Ambari dokumentáció][ambari-docs].

Windows-fürtök esetén egy másik módja toocheck hello összetevő verziószáma toolog tooa fürt távoli asztal használatával, és vizsgálja meg a hello hello C:\apps\dist\ könyvtár tartalmának.

> [!IMPORTANT]
> Linux egy hello azt az egyetlen operációs rendszer, a HDInsight 3.4 vagy újabb verzióját használja. További információkért lásd: [Windows használatból való kivonást a HDInsight](#hdinsight-windows-retirement).

### <a name="release-notes"></a>Kibocsátási megjegyzések

Lásd: [HDInsight kibocsátási megjegyzések](hdinsight-release-notes.md) további kibocsátási megjegyzések hello HDInsight legújabb verziói.

## <a name="supported-hdinsight-versions"></a>Támogatott HDInsight-verziókról
hello következő táblázatban hello verziói HDInsight jelenleg elérhető hello Azure-portálon. hello HDP verziókat, amelyek megfelelnek a HDInsight-verzió tooeach hello termék kiadási dátum együtt jelennek meg. hello támogatás lejárati és a használatból való kivonást dátumát is biztosít, ha azok még ismert.

> [!NOTE]
> Támogatási után az egy lejárt, azt nem feltétlenül érhető el hello Microsoft Azure klasszikus portálon keresztül. Azonban továbbra is a fürt verzió érhető el a hello használatával toobe `Version` hello Windows PowerShell paraméter [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) parancsot, és a .NET SDK hello amíg hello verzió kivezetési dátum.
> 
> Magas rendelkezésre állású fürtök két átjárócsomópontokkal a HDInsight-verzió 2.1-es és újabb verziók esetében alapértelmezés szerint vannak telepítve. Nem elérhetők a HDInsight-fürtök 1.6-os verzióra.

| HDInsight-verzió | HDP verzió | VIRTUÁLIS GÉP OPERÁCIÓS RENDSZERE | Magas rendelkezésre állás | Kiadás dátuma | Elérhetőségét a hello Azure-portálon | Támogatás lejárati dátuma | Kivezetési dátum |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.6. |2.6 HDP |Ubuntu 16 |Igen |2017. április 4. |Igen | | |
| HDInsight 3.5 |2.5 HDP |Ubuntu 16 |Igen |2016. Szeptembertől 30. |Igen |2017. szeptember 5. |2018. május 31-ig. |
| HDInsight 3.4 |2.4 HDP |Ubuntu 14.0.4 LTS |Igen |2016. március 29. |Igen |2016. december 29. |2018. január 9. |
| HDInsight 3.3 |2.3 HDP |Windows Server 2012 R2 |Igen |2015. december 2. |Igen |2016. június 27. |2018. július 31-ig. |
| HDInsight 3.3 |2.3 HDP |Ubuntu 14.0.4 LTS |Igen |2015. december 2. |Igen |2016. június 27. |2017. július 31-ig. |
| A HDInsight 3.2. |2.2 HDP |Ubuntu, 12.04 LTS, vagy a Windows Server 2012 R2 rendszerben |Igen |2015. február 18. |Nem |2016. március 1. |2017. április 1. |
| HDInsight 3.1 |2.1 HDP |Windows Server 2012 R2 |Igen |2014. június 24. |Nem |2015. május 18. |2016. június 30. |
| A HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |Igen |2014. február 11. |Nem |2014. szeptember 17. |2015. június 30. |
| A HDInsight 2.1-es verziója |1.3 HDP |Windows Server 2012 R2 |Igen |2013. október 28. |Nem |2014. május 12. |2015. május 31-ig. |
| HDInsight 1.6-os |1.1 HDP | |Nem |2013. október 28. |Nem |2014. április 26. |2015. május 31-ig. |

## <a name="hdinsight-windows-retirement"></a>HDInsight Windows kivonása
Microsoft Azure HDInsight 3.3-as verziója lett HDInsight a Windows hello legutóbbi verzióját. hello kivezetési dátum Windows hdinsight 2018 július 31. Ha a HDInsight-fürtök Windows 3.3-as vagy annál régebbi, át kell tooHDInsight Linux (HDInsight 3.5-ös vagy újabb verzió) 2018 július 31 előtt. Toohello áttelepítése Linux operációs rendszert futtató tooretain hello képességét toocreate lehetővé teszi, vagy a HDInsight-fürtök átméretezése. HDInsight Windows 3.3-as verzió támogatása lejárt 2016. június 27.

HDInsight 3.4-es verziójú verziótól kezdődően a Microsoft HDInsight csak a Linux operációs rendszert futtató hello kiadott. Ennek eredményeképpen egyes hello összetevők HDInsight belül érhetők Linux csak. Ezek közé tartoznak, Apache Pletyka, Kafka, interaktív struktúra, a Spark HDInsight-alkalmazásokat, és az Azure Data Lake Store hello elsődleges fájlrendszer. Csak a Linux operációs rendszert futtató hello HDInsight későbbi kiadásaiban érhetők el. Nincs HDInsight a Windows későbbi kiadásaiban lesz. 

## <a name="faqs"></a>Gyakori kérdések

### <a name="what-is-hello-timeline-for-retiring-hdinsight-on-windows"></a>Mi az a HDInsight eltávolítása Windows hello ütemtervét?
2018. július 31. a kivezetési dátum a HDInsight a Windows hello. Ha hello tervezett kivezetési dátum nem azonos a régió, értesítést fog kapni külön-külön. 

### <a name="what-is-hello-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>Mi az a meglévő ügyfeleknek eltávolítása HDInsight Windows hello hatását?
A Windows HDInsight kivonását követően nem egy új HDInsight Windows-fürt létrehozása, vagy egy meglévő HDInsight-Windows-fürt méretezése. HDInsight 3.3-as verzió támogatása lejárt 2016. június 27. Ezért nincs támogatási vagy a HDInsight 3.3-as vagy korábbi verziójú hibajavításokat tartalmaz. Csak a Linux operációs rendszert futtató hello HDInsight későbbi kiadásaiban érhetők el. Nincs HDInsight a Windows későbbi kiadásaiban lesz.
 
### <a name="which-versions-of-hdinsight-on-windows-are-affected"></a>A HDInsight a Windows mely verzióit érintett?
Az Azure HDInsight 3.3-as verzióját az HDInsight a Windows hello utolsó verziója. Mielőtt Windows HDInsight kivonják, 3.3-as verziójának vagy korábbi fürtök verziójúnak kell lennie az összes HDInsight Windows át tooHDInsight Linux 3.5-ös vagy újabb verziója. A fürtök tooHDInsight Linux áttelepítése lehetővé teszi a tooretain hello képességét toocreate új fürtök, vagy meglévő fürtök átméretezése. 

### <a name="what-do-i-need-toodo"></a>Mire van szükségem az toodo?
A HDInsight Windows fürtök támogatott tooa HDInsight Linux-fürt áttelepítése előtt 2018 július 31. További információ: hello [HDInsight áttelepítési dokumentum](https://docs.microsoft.com/en-gb/azure/hdinsight/hdinsight-migrate-from-windows-to-linux). További Azure HDInsight verzióival kapcsolatos információkért lásd: hello listája [támogatott verziók](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions). 

### <a name="where-do-i-find-hello-cluster-os-type"></a>Hol található hello fürt operációs rendszerének típusa?
A hello Azure-portálon, válassza a toohello HDInsight-fürtök – áttekintés oldalra, és keresse meg **típusú fürt** alatt **Essentials**. az operációs rendszer fürttípusok hello adott oldalon felsorolt. 

### <a name="i-cant-migrate-tooan-hdinsight-linux-cluster-by-july-31-2018-what-is-hello-impact-toomy-hdinsight-windows-cluster"></a>Nem, ha az tooan HDInsight Linux-fürt által 2018 július 31 telepíthetők át. Mi az a hello hatás toomy HDInsight Windows-fürt?
hello HDInsight Windows-fürt futtatja-van, de nem hozható létre egy új HDInsight Windows-fürt, vagy egy meglévő HDInsight-Windows-fürt méretezése. 

### <a name="my-cluster-has-a-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>A fürt egy .NET-függőség van. Hogyan oldja meg a függőség Linux?
A Linux-fürt függőségi oldhatja hello segítségével [monó projekt](http://www.mono-project.com/). A nyílt forráskódú végrehajtása .NET HDInsight Linux-fürtök érhető el. További információ: hello [HDInsight áttelepítési dokumentum](https://docs.microsoft.com/en-gb/azure/hdinsight/hdinsight-migrate-from-windows-to-linux). 

### <a name="im-a-new-customer-for-hdinsight-on-windows-how-can-i-create-an-hdinsight-windows-cluster"></a>A HDInsight a Windows egy új ügyfél vagyok. Hogyan hozható létre egy HDInsight Windows fürt?
2017. július 3. frissítésétől csak meglévő HDInsight-Windows-ügyfelek tárolófürtöket hozhat létre új HDInsight Windows. Új ügyfelek nem hozható létre egy HDInsight Windows fürt hello Azure-portálon a PowerShell vagy a hello SDK használatával. Javasoljuk, hogy új ügyfelek hozzon létre egy Linux HDInsight-fürtöt. Meglévő ügyfelek tárolófürtöket hozhat létre új HDInsight Windows kivezetési dátum HDInsight a Windows hello-ig. 

### <a name="is-there-a-pricing-impact-associated-with-moving-from-hdinsight-on-windows-toohdinsight-on-linux"></a>A társított helyezze át a HDInsight Linux rendszeren a Windows tooHDInsight árképzési hatása van?
Nem, hello árképzési van hello azonos HDInsight vagy operációs rendszer. 

### <a name="what-are-hello-customer-advantages-associated-with-hello-move-tooonly-using-hdinsight-on-linux"></a>Mik azok a hello ügyfél előnyeit hello társított helyezze át a HDInsight használata Linux tooonly?
* Gyorsabb idő piacra jutási nyílt forráskódú big Data típusú adatok technológiák keresztül hello HDInsight-szolgáltatás
* A nagy közösségi és támogatási ökoszisztémájának
* Képes tooexercise aktív fejlesztési által hello nyissa meg a forrás közösségi Hadoop és egyéb big Data típusú adatok technológiák

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-what-is-available-in-hdinsight-on-windows"></a>HDInsight Linux rendszeren nyújt további funkciók túl az elérhető a Windows a Hdinsightban?
HDInsight 3.4-es verziójú verziótól kezdődően a Microsoft HDInsight csak a Linux operációs rendszert futtató hello kiadott. Ennek eredményeképpen egyes hello összetevők HDInsight belül érhetők Linux csak. Ezek közé tartoznak, Apache Pletyka, Kafka, interaktív struktúra, a Spark HDInsight-alkalmazásokat, és az Azure Data Lake Store hello elsődleges fájlrendszer. 

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight-fürt verziókról szolgáltatásiszint-szerződés
hello szolgáltatásiszint-szerződéssel (SLA) van megadva a egy _támogatási ablak_. hello támogatási időszak hello időn belül, egy HDInsight-fürt verziószáma Microsoft ügyfélszolgálata és által támogatott. Hello verzió-e egy _támogatja a lejárati dátum_ , amely megfelelt, a HDInsight-fürt hello hello támogatási időszakon kívül. Támogatott verzióival kapcsolatos további információkért lásd: hello listája [támogatott HDInsight-fürt verziók](https://docs.microsoft.com/en-gb/azure/hdinsight/hdinsight-migrate-from-windows-to-linux). hello támogatás lejárati dátuma a megadott HDInsight verziója X (miután elérhetővé vált egy újabb X + 1) hello később, akkor a program:  

* 1 képlet: 180 nap toohello dátum, amikor hello HDInsight-fürt verziószáma X jelent hozzáadása.
* 2 képlet: 90 nap toohello dátum, amikor hello HDInsight-fürt verziószáma X + 1 szeretné elérhetővé tenni az Azure portál hozzáadása.

Hello _kivezetési dátum_ hello dátum, amely után hello fürt verziószáma nem hozható létre a HDInsight-on. 2017. július 31., kezdve a kivezetési dátum után egy HDInsight-fürt nem tudja átméretezni. 

> [!NOTE]
> HDInsight Windows-fürtök (többek között a következőket verzió 2.1, 3.0-s, 3.1, 3.2-es és 3.3-as) futtatható Azure Vendég operációsrendszer-család 4-es verzióját, amely Windows Server 2012 R2 hello 64 bites verzióját használja. Azure Vendég operációsrendszer-család 4-es verziójú hello .NET-keretrendszer 4.0-s, 4.5-ös, 4.5.1, és verziói 4.5.2 támogatja.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks kibocsátási megjegyzéseket társított a HDInsight-verziókról

hello szakasz hivatkozások toorelease megjegyzések hello Hortonworks Data Platform disztribúcióiról, valamint a hdinsight eszközzel használt Apache-összetevők.
* HDInsight-fürt verziószáma 3.6 alapuló Hadoop-terjesztést használja [Hortonworks Data Platform 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* HDInsight-fürt verziószáma 3.5-ös verzióját használja egy Hadoop-terjesztést alapuló [Hortonworks Data Platform 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). A HDInsight fürt 3.5-ös verziója hello _alapértelmezett_ hello Azure-portálon létrehozott Hadoop-fürt.
* HDInsight-fürt verziószáma 3.4 alapuló Hadoop-terjesztést használja [Hortonworks Data Platform 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* HDInsight-fürt verziószáma 3.3-as verzióját használja egy Hadoop-terjesztést alapuló [Hortonworks Data Platform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Apache Storm kibocsátási megjegyzések](https://storm.apache.org/2015/11/05/storm0100-released.html) hello Apache webhelyen érhetők el.
  * [Kibocsátási megjegyzések Apache Hive](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) hello Apache webhelyen érhetők el.
* A HDInsight fürt 3.2-es verziójú alapuló Hadoop-terjesztést használja [Hortonworks Data Platform 2.2][hdp-2-2].

  * Kibocsátási megjegyzések a meghatározott Apache összetevők érhetők el: [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [közös](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [0.9.3-as Storm](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), és [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight-fürt verziószáma 3.1 alapuló Hadoop-terjesztést használja [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. 2014. novemberi, 7, előtt létrehozott HDInsight 3.1 fürtök alapuló [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* HDInsight-fürt verziószáma 3.0 alapuló Hadoop-terjesztést használja [Hortonworks Data Platform 2.0][hdp-2-0-8].
* HDInsight-fürt verziószáma 2.1-es verzióját használja egy Hadoop-terjesztést alapuló [Hortonworks Data Platform 1.3][hdp-1-3-0].
* HDInsight-fürt verziószáma 1.6-os alapuló Hadoop-terjesztést használja [Hortonworks Data Platform 1.1][hdp-1-1-0].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard és HDInsight Prémium

Az Azure HDInsight big data felhőajánlatokat hello két kategóriába biztosít: _szabványos_ és _prémium_. hello következő táblázatban elérhető _csak_ a HDInsight prémium. A HDInsight Standard és Premium funkciókat, amelyek nem kifejezetten ismertetett hello tábla érhetők el.

> [!NOTE]
> hello HDInsight prémium ajánlat jelenleg előzetes verzióban érhetők, és csak a Linux-fürtök esetén érhető el.

| HDInsight prémium funkció | Leírás |
| --- | --- |
| A HDInsight-fürtök tartományhoz |HDInsight fürtök tooAzure Active Directory (Azure AD) tartományhoz a vállalati szintű biztonság csatlakozni. HDInsight prémium konfigurálnia a vállalati, akik az Azure AD toolog tooan HDInsight-fürt használatával képes hitelesíteni az alkalmazottakat. hello vállalati rendszergazda konfigurálhatja a Hive biztonsági szerepköralapú hozzáférés-vezérlés használatával [Apache Pletyka](http://hortonworks.com/apache/ranger/) és adatok hozzáférési toouse korlátozhatja a csak, mint amennyit szükséges. Végül Üdvözöljük a rendszergazdákat naplózhatja az alkalmazottak által elért adatokat, és a módosítások tooaccess hozzáférésvezérlési házirendeket, ezáltal a cégirányítási a vállalati erőforrások magas fokú elérése. További információkért lásd: [konfigurálása tartományhoz a HDInsight-fürtök](hdinsight-domain-joined-configure.md). |

### <a name="cluster-types-supported-in-hdinsight-premium"></a>Fürt típusokat támogatja a HDInsight prémium
hello következő táblázatban hello fürttípusok, amelyekkel a HDInsight prémium támogatottak.

| Fürttípus | Standard | Prémium (előzetes verzió) |
| --- | --- | --- |
| Hadoop |Igen |Igen (csak a 3.6. HDInsight) |
| Spark |Igen |Nem |
| HBase |Igen |Nem |
| Storm |Igen |Nem |
| R Server |Igen |Nem |
| Interaktív struktúra (előzetes verzió) |Igen |Nem |
| Kafka (előzetes verzió) |Igen |Nem | 

### <a name="support-for-azure-data-lake-store-in-hdinsight-premium"></a>Az Azure Data Lake Store a HDInsight prémium támogatása

HDInsight prémium fürtök nem támogatják az Azure Data Lake Store elsődleges tárolóként használ. Azonban is használhatja az Azure Data Lake Store kiegészítő tárterület HDInsight prémium fürtökkel.

### <a name="pricing-and-sla"></a>Díjszabás és SLA
HDInsight prémium tarifacsomag és SLA-t információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Alapértelmezett csomópont konfigurációs és virtuális gépek méretei fürtök
a következő táblák hello alapértelmezett virtuális gép (VM) méretek listázása a HDInsight-fürtök hello.

> [!IMPORTANT]
> Ha több mint 32 munkavégző csomópontokhoz fürtben, ki kell választania egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM-mal.
> 
> 

* Dél-Brazília és Nyugat-japán kivételével minden támogatott régiók:

  | Fürttípus | Hadoop | HBase | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- |
  | HEAD: alapértelmezett Virtuálisgép-méretet |D3 v2 |D3 v2 |A3 |D12 v2 |D12 v2 |
  | HEAD: ajánlott Virtuálisgép-méretek |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |A3 MÉRETŰ, A4 MÉRETŰ, A5 CSOMAG |D12 v2, D13 v2, D14 v2 |D12 v2, D13 v2, D14 v2 |
  | Munkavégző: alapértelmezett Virtuálisgép-méretet |D3 v2 |D3 v2 |D3 v2 |Windows: D12 v2; Linux: D4 v2 |Windows: D12 v2; Linux: D4 v2 |
  | Munkavégző: ajánlott Virtuálisgép-méretek |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
  | ZooKeeper: alapértelmezett Virtuálisgép-méretet | |A3 |A2 | | |
  | ZooKeeper: ajánlott Virtuálisgép-méretek | |A3 MÉRETŰ, A4 MÉRETŰ, A5 CSOMAG |A2 MÉRETŰ, A3 MÉRETŰ, A4 | | |
  | Peremhálózati: alapértelmezett Virtuálisgép-méretet | | | | |Windows: D12 v2; Linux: D4 v2 |
  | Peremhálózati: ajánlott Virtuálisgép-mérettel | | | | |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
* Dél-Brazília és Nyugat-japán csak (nincs v2 méretű):

  | Fürttípus | Hadoop | HBase | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- |
  | HEAD: alapértelmezett Virtuálisgép-méretet |D3 |D3 |A3 |D12 |D12 |
  | HEAD: ajánlott Virtuálisgép-méretek |D3 D4, D12 |D3 D4, D12 |A3 MÉRETŰ, A4 MÉRETŰ, A5 CSOMAG |D12 D13, D14 |D12 D13, D14 |
  | Munkavégző: alapértelmezett Virtuálisgép-méretet |D3 |D3 |D3 |Windows: D12; Linux: D4 |Windows: D12; Linux: D4 |
  | Munkavégző: ajánlott Virtuálisgép-méretek |D3 D4, D12 |D3 D4, D12 |D3 D4, D12 |Windows: D12 D13, D14; Linux: D4, D12 D13, D14 |Windows: D12 D13, D14; Linux: D4, D12 D13, D14 |
  | ZooKeeper: alapértelmezett Virtuálisgép-méretet | |A2 |A2 | | |
  | ZooKeeper: ajánlott Virtuálisgép-méretek | |A2 MÉRETŰ, A3 MÉRETŰ, A4 |A2 MÉRETŰ, A3 MÉRETŰ, A4 | | |
  | Peremhálózati: alapértelmezett Virtuálisgép-méretek | | | | |Windows: D12; Linux: D4 |
  | Peremhálózati: ajánlott Virtuálisgép-méretek | | | | |Windows: D12 D13, D14; Linux: D4, D12 D13, D14 |

> [!NOTE]
> - HEAD nevezik *Nimbus* hello Storm a fürt típusa.
> - Néven dolgozó *felügyelő* hello Storm a fürt típusa.
> - Néven dolgozó *régió* hello HBase a fürt típusa.

## <a name="next-steps"></a>Következő lépések
- [A telepítő Hadoop, Spark, és a HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md)
- [Működik a Hadoop on HDInsight from Windows-számítógépek](hdinsight-hadoop-windows-tools.md)

[Supported HDInsight versions]:(#supported-hdinsight-versions)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
