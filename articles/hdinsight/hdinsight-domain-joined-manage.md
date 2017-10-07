---
title: "aaaManage tartományhoz a HDInsight-fürtök - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage tartományhoz HDInsight-fürtök"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 233ddf0953e981f9a24b77d9dde194d590e5e6d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a>Tartományhoz csatlakozó HDInsight-fürtök (előzetes verzió) kezelése
Ismerje meg, hello felhasználók és a tartományhoz, és hogyan toomanage tartományhoz HDInsight-fürtök hello szerepkörök.

## <a name="users-of-domain-joined-hdinsight-clusters"></a>A tartományhoz a HDInsight-fürtök felhasználók
Egy HDInsight-fürthöz nem csatlakozó tartomány-van két olyan felhasználói fiókot, amely hello fürt létrehozása során jönnek létre:

* **Ambari admin**: Ez a fiók akkor is *Hadoop felhasználói* vagy *HTTP felhasználói*. Ez a fiók lehet a következő https:// tooAmbari használt toolog&lt;clustername >. azurehdinsight.net. Azt is használt toorun lekérdezései Ambari nézetek, feladatok külső eszközök (azaz PowerShell, lépni a Templeton, Visual Studio) keresztül hajtható végre, és hello Hive ODBC-illesztővel és az Üzletiintelligencia-eszközök (pl. Excel, Power bi vagy Tableau) a hitelesítést.
* **SSH-felhasználó**: Ez a fiók SSH együtt, és sudo parancsok. Legfelső szintű jogosultságokkal toohello Linux virtuális gépeket tartalmaz.

Egy tartományhoz csatlakozó HDInsight-fürt rendelkezik három új felhasználók hozzáadása tooAmbari rendszergazda és az SSH-felhasználó.

* **Pletyka admin**: Ez egy hello helyi Apache Pletyka rendszergazdai fiók. Az active directory tartományi felhasználó nincs. Ehhez a fiókhoz használt toosetup házirendek kell, és ellenőrizze a más felhasználók rendszergazdák vagy delegált rendszergazdák (így ezen házirendek kezelése). Alapértelmezés szerint hello felhasználónév az *admin* és hello jelszó van hello megegyeznek a hello Ambari rendszergazdai jelszavát. hello jelszó hello beállításai lapon a Pletyka lehet frissíteni.
* **Fürt tartományi felhasználót a rendszergazda**: ezt a fiókot az active directory tartományi felhasználó, beleértve az Ambari és Pletyka Hadoop-fürt felügyeleti hello kijelölt. Fürt létrehozása során a felhasználó hitelesítő adatait kell megadnia. Ez a felhasználó rendelkezik jogosultsággal a következő hello:

  * Gépek toohello tartományhoz, és helyezze el őket belül hello szervezeti egységet a fürt létrehozása során.
  * Hozzon létre szolgáltatásnevekről belül hello szervezeti egységet a fürt létrehozása során.
  * Névkeresési DNS-bejegyzéseket létrehozni.

    Megjegyzés: hello más AD felhasználók is ezeket a jogokat.

    Nincsenek egyes végpontok (például lépni a Templeton) hello fürtön belül Pletyka által nem kezelt, és ezért nem biztonságos. A végpontok hello fürt rendszergazdai tartományi felhasználó kivételével minden felhasználóra van zárolva.
* **Rendszeres**: fürt létrehozása során megadhatja a több active directory-csoportokat. Ebben a csoportokban lévő hello felhasználók szinkronizált tooRanger és Ambari lesz. Ezek a felhasználók tartományi felhasználók pedig a hozzáférési tooonly Pletyka által felügyelt végpontok (például hiveserver2-n). Összes hello RBAC-házirendek és naplózás alkalmazható toothese felhasználók.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>A tartományhoz a HDInsight-fürtök szerepkörök
A HDInsight a tartományhoz a következő szerepkörök hello rendelkezik:

* A Fürtfelügyelő
* Fürt operátor
* Szolgáltatás-rendszergazda
* Szolgáltatás operátor
* Fürt felhasználói

**Ezek a szerepkörök toosee hello engedélyei**

1. Nyissa meg a hello Ambari kezelő felhasználói felületén.  Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).
2. Hello bal oldali menüben kattintson **szerepkörök**.
3. Kattintson a kék hello kérdőjel toosee hello engedélyek:

    ![HDInsight szerepkörök engedélyekkel a tartományhoz](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-hello-ambari-management-ui"></a>Nyissa meg a hello Ambari felügyeleti felhasználói felület
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Nyissa meg a HDInsight-fürt egy panelen. Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Kattintson a **irányítópult** a hello felső menüjében tooopen Ambari.
4. Jelentkezzen be tooAmbari hello fürt rendszergazdai tartományi felhasználónevet és jelszót használja.
5. Hello kattintson **Admin** hello jobb felső sarokban, és kattintson a legördülő menü **kezelése az Ambari**.

    ![Tartományhoz csatlakozó HDInsight kezelése az Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    felhasználói felület hello néz ki:

    ![Tartományhoz csatlakozó HDInsight Ambari kezelőfelület](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-hello-domain-users-synchronized-from-your-active-directory"></a>Az Active Directoryból szinkronizált hello tartományi felhasználók listázása
1. Nyissa meg a hello Ambari kezelő felhasználói felületén.  Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).
2. Hello bal oldali menüben kattintson **felhasználók**. Ekkor megjelenik az Active Directory toohello HDInsight-fürtjéhez szinkronizált senki hello.

    ![Tartományhoz csatlakozó HDInsight Ambari felügyeleti felhasználói felület felhasználók listázása](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-hello-domain-groups-synchronized-from-your-active-directory"></a>Az Active Directoryból szinkronizált hello tartomány csoportjainak
1. Nyissa meg a hello Ambari kezelő felhasználói felületén.  Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).
2. Hello bal oldali menüben kattintson **csoportok**. Ekkor megjelenik az Active Directory toohello HDInsight-fürtjéhez szinkronizált összes hello csoport.

    ![Tartományhoz csatlakozó HDInsight Ambari felügyeleti felhasználói felület listázza a csoportokat](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Nézetek Hive-engedélyek konfigurálása
1. Nyissa meg a hello Ambari kezelő felhasználói felületén.  Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).
2. Hello bal oldali menüben kattintson **nézetek**.
3. Kattintson a **HIVE** tooshow hello részleteit.

    ![HDInsight Ambari felügyeleti tartományhoz felhasználói felület Hive-nézetek](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Kattintson a hello **Hive View** tooconfigure Hive nézetekhez kapcsolni.
5. Görgessen lefelé toohello **engedélyek** szakasz.

    ![HDInsight Ambari felügyeleti tartományhoz felhasználói felület Hive nézetek engedélyek konfigurálása](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Kattintson a **felhasználó hozzáadása** vagy **csoport hozzáadása**, majd adja meg a hello felhasználók vagy csoportok, a Hive-nézetek használatával.

## <a name="configure-users-for-hello-roles"></a>Felhasználók hello szerepkörök konfigurálása
 toosee szerepköröket és engedélyeiket, listáját lásd: [szerepkörök a tartományhoz csatlakoztatott HDInsight-fürtök](#roles-of-domain---joined-hdinsight-clusters).

1. Nyissa meg a hello Ambari kezelő felhasználói felületén.  Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).
2. Hello bal oldali menüben kattintson **szerepkörök**.
3. Kattintson a **felhasználó hozzáadása** vagy **csoport hozzáadása** tooassign felhasználók és csoportok toodifferent szerepkörök.

## <a name="next-steps"></a>Következő lépések
* A tartományhoz csatlakoztatott HDInsight-fürtök konfigurálásához lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md).
* A Hive-házirendek konfigurálásához és a Hive-lekérdezések futtatásához lásd: [Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-fürtökben](hdinsight-domain-joined-run-hive.md).
* Az SSH használata a tartományhoz csatlakoztatott HDInsight-fürtökön Hive-lekérdezéseket futtat, tekintse meg a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
