---
title: "Tartományhoz csatlakozó Azure HDInsight-architektúra |} Microsoft Docs"
description: "Útmutató a tartományhoz csatlakoztatott HDInsight tervezéséhez."
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
ms.openlocfilehash: 7e34f47f09466a40993b4cc797ff1cad2bdaeafe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Azure-tartományhoz csatlakoztatott Hadoop-fürtök tervezése a HDInsightban

A hagyományos Hadoop egy egyfelhasználós fürt. Ez megfelel a legtöbb olyan cégnek, amelyekben kisebb alkalmazásfejlesztő csapatok dolgoznak a nagy adatszámítási feladatok kiépítésén. A Hadoop növekvő népszerűségével, sok vállalat vált olyan modellre, ahol a fürtöket az informatikai csapatok felügyelik, és több alkalmazásfejlesztő csapat is dolgozik ugyanazokon a fürtökön. Így a többfelhasználós fürtöket használó funkció az egyik legtöbbet kért funkció a HDInsightban.

Helyett a saját többfelhasználós hitelesítési és engedélyezési készítéséhez, a HDInsight legnépszerűbb identitásszolgáltató – Active Directory (AD) alapul. A hatékony biztonsági funkciókat AD hdinsight többfelhasználós engedélyezési kezelésére használható. Az ad-val a HDInsight integrálásával kommunikálhat a fürtök AD hitelesítő adataival. HDInsight van leképezve egy AD-felhasználó a helyi Hadoop-felhasználó, így a szolgáltatás HDInsight (Ambari, kiszolgáló, idejére Spark thrift Hive server és a mások) munkahelyi zökkenőmentesen a hitelesített felhasználó számára.

## <a name="integrate-hdinsight-with-ad-and-ad-on-iaas-vm"></a>HDInsight integrálása AD és az AD IaaS virtuális gépen

HDInsight integrálása az Azure AD vagy AD Iaas virtuális gépen, a HDInsight-fürtcsomóponton található, a tartományhoz csatlakoztatott egy tartományhoz. HDInsight a Hadoop-szolgáltatások, a fürtben futó szolgáltatásnevekről hoz létre, és az Azure AD vagy ad szolgáltatásokba, az infrastruktúra-szolgáltatási virtuális gép egy adott szervezeti egység (OU) belül helyezi azokat. HDInsight címfeloldási DNS-hozzárendelések is létrehoz a tartomány az IP-címekhez tartozó csomópontot csatlakoznak a tartományhoz.

Ezt a beállítást többféle architektúra használatával érheti el. Az alábbi architektúrák közül választhat.

**A HDInsight az Azure infrastruktúra-szolgáltatáson futó AD-vel integrált**

Ez a legegyszerűbb architektúra a HDInsight és az Active Directory integrálásához. Az AD-tartományvezérlő egy (vagy több) virtuális gépek (VM) az Azure-ban futó. Ezek a virtuális gépek általában egy virtuális hálózatot alkotnak. Konfigurálni kell egy másik virtuális hálózatot is a HDInsight-fürt számára. HDInsight kell rendelkeznie az Active Directory egy sor a láthatáron, van szüksége a virtuális hálózatok használatával egyenrangú [VNet – VNet társviszony-létesítés](../virtual-network/virtual-network-create-peering.md). Ha az ARM ESZKÖZBEN az Active Directory hoz létre, majd hozhat létre az Active Directory és a HDInsight ugyanazon virtuális, és nem kell tennie társviszony-létesítés. 

![Tartományhoz csatlakoztatott HDInsight-fürtök topológiája](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> Ebben az architektúrában az Azure Data Lake Store és a HDInsight-fürt nem használható együtt.


Az Active Directory előfeltételei:

* Létre kell hoznia egy [szervezeti egységet](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), amelyben a HDInsight-fürt virtuális gépeit és a fürt által használt egyszerű szolgáltatásokat helyezi el.
* [Lightweight Directory Access protokollok](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) be kell állítani a az ad-vel való kommunikációhoz. Az LDAPS konfigurálásához használt tanúsítványnak valódi (nem önaláírt) tanúsítványnak kell lennie.
* Fordított irányú DNS-zónákat kell létrehoznia a tartományon a HDInsight-alhálózat IP-címtartománya számára (például 10.2.0.0/24 az előző képen).
* Szükséges egy szolgáltatásfiók vagy egy felhasználói fiók is. Ezt a fiókot a HDInsight-fürt létrehozására használhatja. A fióknak az alábbi engedélyekkel kell rendelkeznie:

    - A szervezeti egységben lévő egyszerű szolgáltatásobjektumok és gépobjektumok létrehozásához szükséges engedélyek
    - A fordított irányú DNS-proxyszabályok létrehozásához szükséges engedélyek;
    - A gépeknek az Active Directory-tartományhoz történő csatlakoztatásához szükséges engedélyek

**Kizárólag felhőalapú Azure AD-vel integrált HDInsight**

A kizárólag felhőalapú Azure AD esetében konfiguráljon egy tartományvezérlőt, hogy a HDInsight integrálható legyen az Azure AD-vel. Ehhez a [Azure Active Directory tartományi szolgáltatások](../active-directory-domain-services/active-directory-ds-overview.md) (az Azure AD DS). Az Azure AD DS tartományvezérlő gépeket hoz létre a felhőben, és megadja az IP-címeiket. Két tartományvezérlőt hoz létre a magas rendelkezésre állás érdekében.

Az Azure AD DS jelenleg kizárólag klasszikus virtuális hálózatokon létezik. Csak a klasszikus Azure-portálon keresztül érhető el. A HDInsight virtuális hálózat az Azure Portalon létezik, amelyet a virtuális hálózatok közötti társviszony-létesítéssel társítani kell a klasszikus virtuális hálózattal.

> [!NOTE]
> Egy klasszikus virtuális hálózat és egy Azure Resource Managerbeli virtuális hálózat társításához a két virtuális hálózatnak ugyanabban a régióban kell lennie, és ugyanazon Azure-előfizetés alá kell tartoznia.

![Tartományhoz csatlakoztatott HDInsight-fürtök topológiája](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Az Azure AD előfeltételei:

* Létre kell hoznia egy [szervezeti egységet](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), amelyben a HDInsight-fürt virtuális gépeit és a fürt által használt egyszerű szolgáltatásokat helyezi el.
* Az [LDAPS-t](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) is konfigurálni kell az Azure AD DS konfigurálásakor. Az LDAPS konfigurálásához használt tanúsítványnak valódi (nem önaláírt) tanúsítványnak kell lennie.
* Fordított irányú DNS-zónákat kell létrehoznia a tartományon a HDInsight-alhálózat IP-címtartománya számára (például 10.2.0.0/24 az előző képen).
* A [jelszókivonatokat](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) szinkronizálnia kell az Azure AD-ről az Azure AD DS-re.
* Szükséges egy szolgáltatásfiók vagy egy felhasználói fiók is. Ezt a fiókot a HDInsight-fürt létrehozására használhatja. A fióknak az alábbi engedélyekkel kell rendelkeznie:

    - A szervezeti egységben lévő egyszerű szolgáltatásobjektumok és gépobjektumok létrehozásához szükséges engedélyek
    - A fordított irányú DNS-proxyszabályok létrehozásához szükséges engedélyek;
    - A gépeknek az Azure AD-tartományhoz történő csatlakoztatásához szükséges engedélyek

## <a name="next-steps"></a>Következő lépések
* A tartományhoz csatlakoztatott HDInsight-fürtök konfigurálásához lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md).
* A tartományhoz csatlakoztatott HDInsight-fürtök kezeléséhez lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök kezelése](hdinsight-domain-joined-manage.md).
* A Hive-házirendek konfigurálásához és a Hive-lekérdezések futtatásához lásd: [Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-fürtökben](hdinsight-domain-joined-run-hive.md).
* Hive-lekérdezések futtatása HDInsight-fürtök tartományhoz az ssh protokoll használatával, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).
