---
title: "aaaHadoop biztonsági - tartományhoz a HDInsight-fürtök - Azure |} Microsoft Docs"
description: "Információk ...."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/31/2016
ms.author: saurinsh
ms.openlocfilehash: 5a9469402a61bcba4017e1ff4bd06dfba23ac963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-toohadoop-security-with-domain-joined-hdinsight-clusters-preview"></a>Egy bevezető tooHadoop biztonsági (előzetes verzió) HDInsight-fürtökkel a tartományhoz

Az Azure HDInsight eddig csak egyetlen felhasználói helyi rendszergazdát támogatott. Ez remekül működött a kisebb alkalmazásfejlesztő csapatoknál vagy részlegeknél. Mint a Hadoop-alapú munkaterhelések szerzett hello vállalati ágazatban további népszerűségét hello szükséges vállalati általános képességeket, például az active directory-alapú hitelesítés, a több felhasználó támogatása és a szerepköralapú hozzáférés-vezérlés egyre fontosabb inaktívvá vált. A tartományhoz a HDInsight-fürtök használata, HDInsight fürt csatlakoztatott tooan Active Directory-tartomány létrehozása, képes hitelesíteni az Azure Active Directory toolog tooHDInsight fürtön keresztül alkalmazottak hello vállalati listájának konfigurálásához. Hello vállalati kívüli személyek nem jelentkezzen be vagy hello HDInsight-fürthöz. hello vállalati rendszergazda konfigurálhat szerepköralapú hozzáférés-vezérlés Hive biztonsági használatával [Apache Pletyka](http://hortonworks.com/apache/ranger/), így toodata tooonly korlátozása elérni, mint amennyit szükséges. Végül Üdvözöljük a rendszergazdákat naplózhatja hello adatelérési alkalmazottai, és bármely végrehajtott változtatások tooaccess hozzáférésvezérlési házirendeket, így a cégirányítási a vállalati erőforrások magas fokú elérése.

> [!NOTE]
> csak a Linux-alapú HDInsight-fürtök Hive munkaterhelés hello megfogalmazandó ebben az előzetes verzióban érhetők el. hello egyéb munkaterhelések, például a HBase, Spark, Storm és Kafka, engedélyezve lesz későbbi kiadásai.

> [!IMPORTANT]
> Oozie nincs engedélyezve a HDInsight-tartományhoz.

## <a name="benefits"></a>Előnyök
A vállalati biztonság négy pillérre támaszkodik – szegélyhálózat-alapú biztonság, hitelesítés, engedélyezés és titkosítás.

![A tartományhoz csatlakoztatott HDInsight-fürtök előnyösek ezen pillérek számára](./media/hdinsight-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Szegélyhálózat-alapú biztonság
A HDInsight szegélyhálózat-alapú biztonságát a virtuális hálózatok és az átjáró szolgáltatás biztosítja. Egy vállalati rendszergazdai ma, hozzon létre egy virtuális hálózaton belüli HDInsight-fürtöt, és a hálózati biztonsági csoportok (bejövő vagy kimenő tűzfalszabályok) toorestrict hozzáférés toohello virtuális hálózat használata. Csak hello hello meghatározott IP-címek bejövő tűzfalszabályokat lesz képes toocommunicate hello HDInsight-fürthöz, így a szegélyhálózati biztonsági biztosítva. A szegélyhálózat-alapú biztonság egy másik rétege az átjáró-szolgáltatással érhető el. hello átjáró hello szolgáltatást, amely védelmet biztosít az összes bejövő kérelmet toohello HDInsight-fürt első sorának funkcionál. Ez hello kérést fogad, érvényesíti azt, és csak ezután lehetővé teszi, hogy hello kérelem toopass toohello más csomópontokat a fürthöz, így biztosítva szegélyhálózati biztonsági tooother nevét és az adatok hello fürt csomópontja.

### <a name="authentication"></a>Authentication
Ezzel a nyilvános előzetes verzióval a vállalati rendszergazdák üzembe helyezhetnek egy tartományhoz csatlakoztatott HDInsight-fürtöt egy [virtuális hálózatban](https://azure.microsoft.com/services/virtual-network/). hello csomópontok hello HDInsight-fürt hello vállalat által kezelt illesztett toohello tartomány lesz. Ez az [Azure Active Directory tartományi szolgáltatások](../active-directory-domain-services/active-directory-ds-overview.md) használatával érhető el. Hello fürt összes csomópontjának hello hello vállalati kezelő illesztett tooa tartományban. Ez a beállítás hello vállalati alkalmazottak bejelentkezhet a tartományi hitelesítő adataik használatával toohello fürtcsomóponton. A tartományi hitelesítő adatok tooauthenticate más jóváhagyott végpontokon Hue, az Ambari nézetek, ODBC, JDBC, PowerShell és a REST API-k toointeract hello fürthöz hasonló szolgáltatást is alkalmazhatja. Üdvözöljük a rendszergazdákat keresztül hello fürt keresztül a fenti végpontokkal való interakció felhasználók hello számának korlátozása teljes körű vezérléssel rendelkezik.

### <a name="authorization"></a>Engedélyezés
Kiegészítve a legtöbb vállalat ajánlott úgy, hogy nem minden alkalmazott rendelkezik-e tooall vállalati erőforrások eléréséhez. Hasonlóképpen a ebben a kiadásban Üdvözöljük a rendszergazdákat segítségével megadhatja az hello fürterőforrás szerepköralapú hozzáférés-vezérlési házirendeket. Üdvözöljük a rendszergazdákat beállíthatja például, [Apache Pletyka](http://hortonworks.com/apache/ranger/) a Hive tooset hozzáférés-vezérlési házirendeket. Ez a funkció biztosítja, hogy az alkalmazottak lesznek képesek tooaccess csak annyi adatot, amely szükségük toobe sikeres a munkájuk elvégzésének elősegítésében. SSH hozzáférés toohello fürt akkor is csak korlátozott toohello rendszergazda.

### <a name="auditing"></a>Naplózás
HDInsight fürt erőforrások a jogosulatlan felhasználóktól és hello adatok naplózásának biztonságossá tétele az összes toohello fürt erőforrásokhoz férnek hozzá, és hello hello védelme mellett adata szükséges tootrack hello erőforrások a jogosulatlan vagy véletlen hozzáférést. Üdvözöljük a rendszergazdákat ebben az előzetes megtekintheti és jelenti a összes toohello HDInsight fürt erőforrások eléréséhez és adatok. Üdvözöljük a rendszergazdákat is megtekintheti, és a jelentés minden változás toohello hozzáférés-vezérlési házirendeket az Apache Pletyka támogatott végpontok. Egy tartományhoz csatlakozó HDInsight-fürtöt használ hello ismerős Apache Pletyka felhasználói felület toosearch naplókat. Hello háttérkiszolgálón Pletyka használ [Apache Solr](http://hortonworks.com/apache/solr/) és a keresés hello naplók tárolásához.

### <a name="encryption"></a>Titkosítás
Az adatok védelmének fontos értekezlet szervezeti biztonsági és megfelelőségi követelményeknek, és hozzáférési toodata korlátozása a nem engedélyezett az alkalmazottak, valamint azt is titkosítani kell titkosítással. Mindkét hello adattárolókhoz a HDInsight-fürtök, Azure Storage-Blobba, és az Azure Data Lake-tárhely támogatja a transzparens kiszolgálóoldali [az adatok titkosítása](../storage/common/storage-service-encryption.md) aktívan. A biztonságos HDInsight-fürtök zökkenőmentesen működnek a kiszolgálóoldali inaktív adattitkosítás funkcióval.

## <a name="next-steps"></a>Következő lépések
* A tartományhoz csatlakoztatott HDInsight-fürtök konfigurálásához lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md).
* A tartományhoz csatlakoztatott HDInsight-fürtök kezeléséhhez lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök kezelése](hdinsight-domain-joined-manage.md).
* A Hive-házirendek konfigurálásához és a Hive-lekérdezések futtatásához lásd: [Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-fürtökben](hdinsight-domain-joined-run-hive.md).
* Az SSH használata a tartományhoz csatlakoztatott HDInsight-fürtökön Hive-lekérdezéseket futtat, tekintse meg a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
