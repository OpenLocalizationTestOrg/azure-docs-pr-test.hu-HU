---
title: "a tartományhoz csatlakoztatott HDInsight - Azure aaaConfigure Hive házirendek |} Microsoft Docs"
description: "Információk ...."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3fade1e5-c2e1-4ad5-b371-f95caea23f6d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 56f2bf9d872abc5f772b886fcf91c2e2422092f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a>Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-ban (előzetes verzió)
Megtudhatja, hogyan tooconfigure Apache Pletyka házirendek a Hive. Ebben a cikkben létrehoz két Pletyka házirendek toorestrict hozzáférés toohello hivesampletable. a HDInsight-fürtök hello hivesampletable tartalmaz. Miután konfigurálta hello házirendeket, a Hdinsightban az Excel és az ODBC illesztőprogram tooconnect tooHive táblák.

## <a name="prerequisites"></a>Előfeltételek
* Tartományhoz csatlakoztatott HDInsight-fürt. Lásd a [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md) című részt.
* Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, az Excel 2013 Standalone vagy Office 2010 Professional Plus rendszert futtató munkaállomás.

## <a name="connect-tooapache-ranger-admin-ui"></a>Csatlakozás tooApache Pletyka felügyeleti felhasználói felület
**tooconnect tooRanger felügyeleti felhasználói felület**

1. Egy böngészőből csatlakozás tooRanger felügyeleti felhasználói felület. hello URL-címe: https://&lt;ClusterName >.azurehdinsight.net/Ranger/.

   > [!NOTE]
   > A Ranger más hitelesítő adatokat használ, mint a Hadoop-fürt. gyorsítótárazott hitelesítő adatok használata Hadoop, tooprevent böngészők arra használják új InPrivate-böngésző ablak tooconnect toohello Pletyka felügyeleti felhasználói felület.
   >
   >
2. Jelentkezzen be hello fürt rendszergazdai tartományi felhasználónevet és jelszót:

    ![HDInsight-tartományhoz csatlakoztatott Ranger kezdőlapja](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    A Ranger jelenleg csak a Yarn és a Hive rendszerrel működik.

## <a name="create-domain-users"></a>Tartományi felhasználók létrehozása
A [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) című részben létrehozott egy hiveuser1 és egy hiveuser2 nevű felhasználót. Ebben az oktatóanyagban hello két felhasználói fiókot fogja használni.

## <a name="create-ranger-policies"></a>Ranger-házirendek létrehozása
Ebben a szakaszban két Ranger-házirendet fog létrehozni a hivesampletable eléréséhez. Adjon kiválasztási engedélyt a különböző oszlopcsoportokra vonatkozóan. Mindkét felhasználó a [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) című részben lett létrehozva.  A következő szakaszban hello tesztelni hello két házirend, az Excel programban.

**toocreate Pletyka házirendek**

1. Nyissa meg a Ranger felügyeleti felhasználói felületét. Lásd: [tooApache Pletyka felügyeleti felhasználói felület csatlakozás](#connect-to-apache-ranager-admin-ui).
2. A **Hive** alatt kattintson a **&lt;ClusterName>_hive** elemre. Két előre konfigurált házirendnek kell megjelennie.
3. Kattintson a **új házirend hozzáadása**, és írja be a következő értékek hello:

   * Házirend neve: read-hivesampletable-all
   * Hive-adatbázis: alapértelmezett
   * tábla: hivesampletable
   * Hive-oszlop: *
   * Felhasználó kiválasztása: hiveuser1
   * Engedélyek: kiválasztás

     ![HDInsight-tartományhoz csatlakoztatott Ranger Hive-házirendjének konfigurálása](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png).

     > [!NOTE]
     > Ha egy tartományi felhasználó nincs feltöltve a felhasználó kiválasztása, pár perc múlva a Pletyka toosync AAD-ben.
     >
     >
4. Kattintson a **Hozzáadás** toosave hello házirend.
5. Ismételje meg a hello utolsó két lépést toocreate egy másik házirendet a következő tulajdonságai hello:

   * Házirend neve: read-hivesampletable-devicemake
   * Hive-adatbázis: alapértelmezett
   * tábla: hivesampletable
   * Hive-oszlop: clientid devicemake
   * Felhasználó kiválasztása: hiveuser2
   * Engedélyek: kiválasztás

## <a name="create-hive-odbc-data-source"></a>Hive ODBC-adatforrás létrehozása
hello utasítások megtalálhatók [létrehozása Hive ODBC-adatforrás](hdinsight-connect-excel-hive-odbc-driver.md).  

    Tulajdonság|Leírás
    ---|---
    Adatforrás neve|Adjon nevet tooyour adatforrás
    Gazdagép|Írja be a következő kifejezést: &lt;HDInsightClusterName>.azurehdinsight.net. Például: sajatHDICluster.azurehdinsight.net
    Port|Használja a <strong>443</strong> számú portot. (Ez a port módosult 563 too443.)
    Adatbázis|Használja az <strong>Alapértelmezett</strong> adatbázist.
    Hive Server típusa|Válassza ki a <strong>Hive Server 2</strong> típust
    Mechanizmus|Válassza ki az <strong>Azure HDInsight szolgáltatást</strong>
    HTTP elérési útja|Hagyja üresen.
    Felhasználónév|Adja meg hiveuser1@contoso158.onmicrosoft.com. Ha más, frissítse a hello tartomány nevét.
    Jelszó|Adjon meg hiveuser1 hello jelszót.
    </table>

Győződjön meg arról, hogy tooclick **teszt** hello adatforrás mentése előtt.

## <a name="import-data-into-excel-from-hdinsight"></a>Adatok importálása Excel formátumba a HDInsight-ból
Hello utolsó szakaszában konfigurálta a két házirendeket.  hiveuser1 hello válassza ki az összes hello oszlopához engedéllyel rendelkezik, és hiveuser2 rendelkezik engedéllyel a két olyan oszlopot válasszon hello. Ebben a szakaszban hello két felhasználók tooimport adatokat Excelbe megszemélyesíteni.

1. Nyisson meg egy új vagy egy meglévő munkafüzetet Excelben.
2. A hello **adatok** lapra, majd **az egyéb adatforrások**, és kattintson a **az Adatkapcsolat varázsló** toolaunch hello **Adatkapcsolat varázsló**.

    ![Adatkapcsolat varázsló megnyitása] [img-hdi-simbahiveodbc.excel.dataconnection]
3. Válassza ki **ODBC Adatforrásnevet** hello adatforrás lehetőséget, majd kattintson **következő**.
4. Az ODBC adatforrások, válassza hello adatok adatforrásnév hello előző lépésben létrehozott, és kattintson **következő**.
5. Jelszó ismételt hello hello fürt hello varázslóban, és kattintson **OK**. Várjon, amíg hello **adatbázis és tábla kijelölése** párbeszédpanel tooopen. Ez eltarthat néhány másodpercig.
6. Válassza ki a **hivesampletable** táblát, majd kattintson a **Tovább** gombra.
7. Kattintson a **Befejezés** gombra.
8. A hello **és adatokat importálhat** párbeszédpanelen módosíthatja vagy hello lekérdezést. toodo kattintson **tulajdonságok**. Ez eltarthat néhány másodpercig.
9. Kattintson a hello **Definition** külön-külön hello parancs szövege:

       SELECT * FROM "HIVE"."default"."hivesampletable"

   Hello Pletyka szabályzatok az Ön által definiált hiveuser1 válassza megfelelő jogosultságokkal rendelkezik az összes hello oszlop.  Ezért ez a lekérdezés hiveuser1 hitelesítő adataival működik, azonban hiveuser2 hitelesítő adataival nem működik.

   ![Kapcsolat tulajdonságai] [img-hdi-simbahiveodbc-excel-connectionproperties]
10. Kattintson a **OK** tooclose hello kapcsolat tulajdonságai párbeszédpanelen.
11. Kattintson a **OK** tooclose hello **és adatokat importálhat** párbeszédpanel.  
12. Írja be újból az hiveuser1 hello jelszót, és kattintson a **OK**. Előtt adatokat lekérdezi az importált tooExcel néhány másodpercet vesz igénybe. Amikor kész van, 11 adatoszlopnak kell megjelennie.

hello utolsó szakaszában létrehozott tootest hello második házirend (olvasás-hivesampletable-devicemake)

1. Adjon hozzá egy új munkalapot az Excelben.
2. Hajtsa végre a hello utolsó eljárás tooimport hello adatokat.  hello mindössze annyi a változás teheti toouse hiveuser2 hitelesítő adatok helyett hiveuser1 tartozó. Ez sikertelen lesz, mert hiveuser2, csak a engedély toosee két oszlopot tartalmaz. Hiba a következő hello kell beolvasása:

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. Hajtsa végre az azonos hello eljárás tooimport adatokat. Ennek az időnek hiveuser2 tartozó hitelesítő adatokat használja, és hello select utasítás a is módosíthatók:

        SELECT * FROM "HIVE"."default"."hivesampletable"

    erre:

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    Amikor kész van, két importáltadat-oszlopnak kell megjelennie.

## <a name="next-steps"></a>Következő lépések
* A tartományhoz csatlakoztatott HDInsight-fürtök konfigurálásához lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md).
* A tartományhoz csatlakoztatott HDInsight-fürtök kezeléséhhez lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök kezelése](hdinsight-domain-joined-manage.md).
* Az SSH használata a tartományhoz csatlakoztatott HDInsight-fürtökön Hive-lekérdezéseket futtat, tekintse meg a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
* Csatlakozás Hive Hive JDBC használatával, lásd: [tooHive on Azure HDInsight Hive JDBC-illesztőt hello használata csatlakozáshoz](hdinsight-connect-hive-jdbc-driver.md)
* Csatlakozás Excel tooHadoop Hive ODBC használatával, lásd: [kapcsolódás Excel tooHadoop a Microsoft Hive ODBC-meghajtó hello](hdinsight-connect-excel-hive-odbc-driver.md)
* Csatlakozás a Power Query használatával Excel tooHadoop, lásd: [kapcsolódás Excel tooHadoop Power Query használatával](hdinsight-connect-excel-power-query.md)
