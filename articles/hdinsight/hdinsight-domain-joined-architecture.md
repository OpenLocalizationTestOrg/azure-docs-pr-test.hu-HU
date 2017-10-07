---
title: "aaaDomain csatlakoztatott Azure HDInsight-architektúra |} Microsoft Docs"
description: "Ismerje meg, hogyan tooplan tartományhoz HDInsight."
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
ms.date: 02/03/2017
ms.author: saurinsh
ms.openlocfilehash: 1c3ecedf3739b4f8fa54160225be9c1d6e2ca6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Azure-tartományhoz csatlakoztatott Hadoop-fürtök tervezése a HDInsightban

hello hagyományos Hadoop egyfelhasználós fürt. Ez megfelel a legtöbb olyan cégnek, amelyekben kisebb alkalmazásfejlesztő csapatok dolgoznak a nagy adatszámítási feladatok kiépítésén. A Hadoop növekvő népszerűségével, sok vállalat vált olyan modellre, ahol a fürtöket az informatikai csapatok felügyelik, és több alkalmazásfejlesztő csapat is dolgozik ugyanazokon a fürtökön. Így a funkciói közé tartoznak, amely többfelhasználós fürtök hello Azure hdinsight leginkább kért funkciók.

Helyett a saját többfelhasználós hitelesítési és engedélyezési felépítése, HDInsight hello legnépszerűbb identitásszolgáltató – Active Directory (AD) alapul. hello hatékony biztonsági funkciókat az ad-ben használt toomanage többfelhasználós engedélyezési hdinsight lehet. Az ad-val a HDInsight integrálásával kommunikálhat hello fürtök AD hitelesítő adataival. HDInsight hozzárendeli egy AD felhasználó tooa helyi Hadoop felhasználót, így az összes hello hdinsighton futó szolgáltatások (Ambari, kiszolgáló, idejére Spark thrift Hive server és a mások) munkahelyi zökkenőmentesen hello hitelesített felhasználó számára.

## <a name="integrate-hdinsight-with-ad-and-ad-on-iaas-vm"></a>HDInsight integrálása AD és az AD IaaS virtuális gépen

HDInsight integrálása az Azure AD vagy AD Iaas virtuális gépen, hello HDInsight-fürtcsomóponton található, a tartományhoz csatlakoztatott tooa tartomány. HDInsight hoz létre a hello szolgáltatásnevekről hello fürtben futó Hadoop-szolgáltatás, és az Azure AD vagy ad szolgáltatásokba, az infrastruktúra-szolgáltatási virtuális gép egy adott szervezeti egység (OU) belül helyezi azokat. HDInsight is létrehoz címfeloldási DNS-hozzárendelések hello hello tartomány, a tartományhoz csatlakoztatott toohello hello csomópontok IP-címét.

Ezt a beállítást többféle architektúra használatával érheti el. A következő architektúrák hello közül választhat.

**A HDInsight az Azure infrastruktúra-szolgáltatáson futó AD-vel integrált**

Ez a legegyszerűbb architektúra hello HDInsight integrálható az Active Directoryban. egy (vagy több) virtuális gépek (VM) az Azure AD-tartományvezérlő hello fut. Ezek a virtuális gépek általában egy virtuális hálózatot alkotnak. Állít be egy másik virtuális hálózati hello HDInsight-fürthöz. A HDInsight toohave egy sor a láthatáron tooActive könyvtár, kell toopeer ezek a virtuális hálózatok használatával [VNet – VNet társviszony-létesítés](../virtual-network/virtual-network-create-peering.md). Ha hoz létre az ARM ESZKÖZBEN, az Active Directory hello, akkor létrehozhat hello Active Directory és a HDInsight hello ugyanazt a virtuális hálózatot, és nincs szükség toodo társviszony-létesítés. 

![Tartományhoz csatlakoztatott HDInsight-fürtök topológiája](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> Ebben az architektúrában nem használható az Azure Data Lake Store hello HDInsight-fürthöz.


Az Active Directory előfeltételei:

* Egy [szervezeti egység](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) belül kell létrehoznia, amely hová kívánja helyezni a HDInsight-fürt virtuális gépek hello és hello fürt által használt szolgáltatásnevekről hello.
* [Lightweight Directory Access protokollok](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) be kell állítani a az ad-vel való kommunikációhoz. hello használt tanúsítvány tooset LDAPS be valós tanúsítványnak (nem egy önaláírt tanúsítvány) kell lennie.
* Névkeresési DNS-zónák hello tartomány hello IP-címtartomány hello HDInsight alhálózat (például az előző ábrán hello 10.2.0.0/24) léteznie kell.
* Szükséges egy szolgáltatásfiók vagy egy felhasználói fiók is. A fiók toocreate hello HDInsight-fürt használatára. Ennek a fióknak hello a következő engedélyekkel kell rendelkeznie:

    - Engedélyek toocreate szolgáltatás egyszerű és gép objektumok belül hello szervezeti egység
    - Engedélyek toocreate címfeloldási DNS proxy szabályok
    - Engedélyek toojoin gépek toohello Active Directory-tartomány

**Kizárólag felhőalapú Azure AD-vel integrált HDInsight**

A kizárólag felhőalapú Azure AD esetében konfiguráljon egy tartományvezérlőt, hogy a HDInsight integrálható legyen az Azure AD-vel. Ehhez a [Azure Active Directory tartományi szolgáltatások](../active-directory-domain-services/active-directory-ds-overview.md) (az Azure AD DS). Az Azure Active Directory tartományi szolgáltatások tartományvezérlő gépeket tartományt hoz hello felhő, és IP-címek biztosít. Két tartományvezérlőt hoz létre a magas rendelkezésre állás érdekében.

Az Azure AD DS jelenleg kizárólag klasszikus virtuális hálózatokon létezik. Csak elérhető hello klasszikus Azure portál használatával. virtuális hálózat szerepel a hello Azure-portálon, amelyekre szüksége van a toobe HDInsight hello a VNet – VNet-társviszony létesítése – társviszonyban hello klasszikus virtuális hálózattal.

> [!NOTE]
> A klasszikus virtuális hálózatot és a virtuális hálózat szükséges, hogy mindkét virtuális hálózat legyen-e az Azure Resource Manager-társviszony létesítése – hello ugyanabban a régióban, és a hello azonos Azure-előfizetés.

![Tartományhoz csatlakoztatott HDInsight-fürtök topológiája](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Az Azure AD előfeltételei:

* Egy [szervezeti egység](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) belül, amely helyezi-e létre kell hozni a HDInsight-fürt virtuális gépek hello és hello fürt által használt szolgáltatásnevekről hello.
* Az [LDAPS-t](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) is konfigurálni kell az Azure AD DS konfigurálásakor. hello használt tanúsítvány tooset LDAPS be valós tanúsítványnak (nem egy önaláírt tanúsítvány) kell lennie.
* Névkeresési DNS-zónák hello tartomány hello IP-címtartomány hello HDInsight alhálózat (például az előző ábrán hello 10.2.0.0/24) léteznie kell.
* [Jelszó-kivonatok](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) kell szinkronizálva az Azure AD tooAzure Active Directory tartományi Szolgáltatásokban.
* Szükséges egy szolgáltatásfiók vagy egy felhasználói fiók is. A fiók toocreate hello HDInsight-fürt használatára. Ennek a fióknak hello a következő engedélyekkel kell rendelkeznie:

    - Engedélyek toocreate szolgáltatás egyszerű és gép objektumok belül hello szervezeti egység
    - Engedélyek toocreate címfeloldási DNS proxy szabályok
    - Engedélyek toojoin gépek toohello az Azure AD-tartomány

## <a name="next-steps"></a>Következő lépések
* tooconfigure egy tartományhoz csatlakozó HDInsight-fürtöt, tekintse meg [konfigurálása tartományhoz a HDInsight-fürtök](hdinsight-domain-joined-configure.md).
* toomanage tartományhoz a HDInsight-fürtök, lásd: [tartományhoz HDInsight-fürtök kezelése](hdinsight-domain-joined-manage.md).
* tooconfigure Hive házirendek és a Hive-lekérdezések futtatása [házirendek konfigurálása Hive HDInsight-fürtök tartományhoz](hdinsight-domain-joined-run-hive.md).
* Hive-lekérdezések toorun SSH segítségével tartományhoz HDInsight-fürtök, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).
