---
title: "tartományhoz csatlakozó HDInsight-fürtök használata a PowerShell - Azure aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset össze, és konfigurálja az Azure PowerShell tartományhoz a HDInsight-fürtök"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a13b2f7a-612d-4800-bc92-7fc0524f3e89
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 49da3439513d1e51171f0f7f7f9c3d967d55cb7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview-using-azure-powershell"></a>Az Azure PowerShell tartományhoz a HDInsight-fürtök (előzetes verzió) konfigurálása
Ismerje meg, hogyan tooset mentése az Azure HDInsight-fürtöt Azure Active Directory (Azure AD) és [Apache Pletyka](http://hortonworks.com/apache/ranger/) Azure PowerShell használatával. Az Azure PowerShell-parancsfájl toomake hello konfigurációs gyorsabb és kevesebb a hibalehetőség hiba valósul meg. A HDInsight-tartományhoz csak Linux-alapú fürtökön konfigurálható. További információkért lásd: [bevezetni a tartományhoz a HDInsight-fürtök](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie nincs engedélyezve a HDInsight-tartományhoz.

A tipikus tartományhoz HDInsight fürt konfigurációjába a lépéseket követve hello foglal magában:

1. Hozzon létre egy Azure klasszikus virtuális hálózatot az Azure AD.  
2. Hozzon létre, és konfigurálja az Azure AD és az Azure Active Directory tartományi Szolgáltatásokban.
3. Adja hozzá a virtuális gép toohello klasszikus virtuális hálózatot a szervezeti egység létrehozásához. 
4. Az Azure Active Directory tartományi Szolgáltatásokban szervezeti egység létrehozása.
5. Hozzon létre egy HDInsight virtuális hálózatot hello Azure-erőforrás felügyeleti mód.
6. A telepítő hello Azure Active Directory tartományi szolgáltatások névkeresési DNS-zónáját.
7. Társ hello két Vnetek.
8. HDInsight-fürtök létrehozása.

hello megadott PowerShell-parancsfájl 3-7 lépéseket végzi el. Haladjon végig 1 és 2. lépés manuálisan.  Ha jobban szeret toouse Azure PowerShell, lásd: [konfigurálása tartományhoz csatlakoztatott HDInsight-fürtök](hdinsight-domain-joined-configure.md). 

Hello végső topológia például a következőképpen néz ki:

![A HDInsight-topológia a tartományhoz](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Mivel az Azure AD jelenleg csak a klasszikus virtuális hálózatokról (Vnetekről) támogatja, és Linux-alapú HDInsight-fürtök csak támogatási Azure Resource Manager Vnetek, HDInsight az Azure AD-integrációs van szükség, két virtuális hálózatokat és a társviszony-létesítés közöttük. Hello összehasonlító információk között hello két üzembe helyezési modellek: [Azure Resource Manager és klasszikus üzembe helyezési: üzembe helyezési modellel megértéséhez, valamint az erőforrások állapotát hello](../azure-resource-manager/resource-manager-deployment-model.md). két Vnetek kell hello hello hello Azure Active Directory tartományi szolgáltatások és ugyanabban a régióban.

> [!NOTE]
> Ez az oktatóanyag feltételezi, hogy nem rendelkezik az Azure AD. Ha nincs fiókja, kihagyhatja a 2. lépésben hello részét.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
A következő elemek toogo az oktatóanyag teljesítéséhez hello kell rendelkeznie:

* Ismerje meg a [Azure AD tartományi szolgáltatások](https://azure.microsoft.com/services/active-directory-ds/) a [árképzési](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktúra.
* Győződjön meg arról, hogy az előfizetés szerepel az engedélyezési listán az nyilvános előzetes verzió. Ehhez az e-mail toohdipreview@microsoft.com az előfizetés-azonosítóval.
* A tartomány egy aláíró hitelesítésszolgáltatóval által aláírt SSL-tanúsítvány. biztonságos LDAP konfigurálásával hello tanúsítvány szükséges. Önaláírt tanúsítványok nem használhatók.
* Azure PowerShell.  Lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a name="create-an-azure-classic-vnet-for-your-azure-ad"></a>Hozzon létre egy Azure klasszikus virtuális hálózatot az Azure AD.
Hello útmutatásért lásd: [Itt](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic).

## <a name="create-and-configure-azure-ad-and-azure-ad-ds"></a>Hozzon létre, és konfigurálja az Azure AD és az Azure Active Directory tartományi Szolgáltatásokban.
Hello útmutatásért lásd: [Itt](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).

## <a name="run-hello-powershell-script"></a>Hello PowerShell parancsfájl futtatása
hello PowerShell-parancsfájl letölthető [GitHub](https://github.com/hdinsight/DomainJoinedHDInsight). Hello zip-fájl kicsomagolásához, és mentse helyileg a hello fájlokat.

**tooedit hello PowerShell-parancsfájl**

1. Nyissa meg a Windows PowerShell ISE vagy bármilyen szövegszerkesztővel run.ps1.
2. Töltse ki a következő változók hello hello értékeit:
   
   * **$SubscriptionName** – hello hello Azure-előfizetéssel, ahová toocreate a HDInsight-fürt nevét. Már létrehozott egy klasszikus virtuális hálózatot az ehhez az előfizetéshez, és az Azure Resource Manager hello HDInsight-fürt előfizetéshez tartozó virtuális hálózati létrehozni.
   * **$ClassicVNetName** -hello klasszikus virtuális hálózatot, amely tartalmazza a hello Azure Active Directory tartományi Szolgáltatásokban. A virtuális hálózat kell hello ugyanahhoz az előfizetéshez biztosított fent. Ez a virtuális hálózat hello Azure-portál használatával, és nem használja a klasszikus portálon kell létrehozni. Ha követi a hello utasítás [konfigurálása tartományhoz csatlakoztatott HDInsight-fürtök (előzetes verzió)](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic), hello alapértelmezés szerint ez contosoaadvnet.
   * **$ClassicResourceGroupName** – hello klasszikus virtuális hálózatot, amely a fent említett hello erőforrás-kezelő neve. Például contosoaadrg. 
   * **$ArmResourceGroupName** – hello az erőforráscsoport neve, amelyen belül toocreate hello HDInsight-fürt. Hello használhatja ugyanazt az erőforráscsoportot, $ArmResourceGroupName.  Ha hello erőforráscsoport nem létezik, a hello parancsfájl hello erőforráscsoportot hoz létre.
   * **$ArmVNetName** -hello erőforrás-kezelő virtuális hálózat nevére belül, amely toocreate hello HDInsight-fürthöz. Ez a virtuális hálózat $ArmResourceGroupName bekerülnek.  Ha hello virtuális hálózat nem létezik, a PowerShell-parancsfájl hello létrehozza azt. Ha létezik, fent biztosító hello erőforráscsoportban kell.
   * **$AddressVnetAddressSpace** – hello hálózati címtér hello erőforrás-kezelő virtuális hálózat. Győződjön meg arról, hogy ezen a címterületen elérhető legyen. Ez a címtartomány hello klasszikus virtuális hálózat címtartományán nem lehetnek átfedésben. Például: "10.1.0.0/16"
   * **$ArmVnetSubnetName** -hello erőforrás-kezelő virtuális hálózati alhálózat neve belül, amely tooplace hello HDInsight-fürt virtuális gépeket szeretne. Ha hello alhálózat nem létezik, a PowerShell-parancsfájl hello létrehozza azt. Ha létezik, akkor hello virtuális hálózat felett biztosító részének kell lennie.
   * **$AddressSubnetAddressSpace** – hello hálózati hello erőforrás-kezelő virtuális hálózati alhálózat címtartománya. virtuális gép IP-címeket fogja a alhálózati címtartományt a HDInsight-fürt hello. Például "10.1.0.0/24."
   * **$ActiveDirectoryDomainName** – hello Azure AD-tartomány nevét, amelyet az toojoin hello HDInsight fürt számára. Például: "contoso.onmicrosoft.com"
   * **$ClusterUsersGroups** – hello biztonsági csoportjait a megjeleníteni kívánt toosync toohello HDInsight-fürtöt AD hello köznapi neve. a biztonsági csoporthoz tartozó hello felhasználók fognak tudni toolog toohello fürt irányítópulton a active directory tartományi hitelesítő adataik használatával. A biztonsági csoportokkal hello active directory léteznie kell. Például a "hiveusers" vagy "clusteroperatorusers" lehetőséget.
   * **$OrganizationalUnitName** -hello szervezeti egység hello a tartományban, amelyen belül tooplace hello HDInsight-fürt virtuális gépeket, és hello fürt által használt szolgáltatásnevekről hello. hello PowerShell-parancsfájlt hoz létre a szervezeti egység, ha nem létezik. Például "HDInsightOU."
3. Hello módosítások mentéséhez.

**toorun hello parancsfájl**

1. Futtatás **Windows PowerShell** rendszergazdaként.
2. Keresse meg a run.ps1 toohello mappájában. 
3. Hello parancsfájl futtatásához írja be a hello fájl nevét, majd nyomja le **ENTER**.  3 bejelentkezési párbeszédpanelek előugró:
   
   1. **Jelentkezzen be a klasszikus portálon tooAzure** – adja meg a hitelesítő adatait, amelyet használhat toosign tooAzure a klasszikus portálon. Az Azure AD és az Azure Active Directory tartományi Szolgáltatásokban, ezek a hitelesítő adatok használatával hello hozott létre.
   2. **Jelentkezzen be tooAzure erőforrás-kezelő portálon** – adja meg a hitelesítő adatait, amelyet használhat toosign tooAzure erőforrás-kezelő portálon.
   3. **Tartományi felhasználónév** – hello hello tartományi felhasználó nevét, amelyet az toobe hello HDInsight-fürt rendszergazda hitelesítő adatait adja meg. Ha létrehozott egy Azure AD teljesen új, kell hozott létre a felhasználó a dokumentum. 
      
      > [!IMPORTANT]
      > Adjon meg hello felhasználónevet a következő formátumban: 
      > 
      > Tartománynév\felhasználónév (például contoso.onmicrosoft.com\clusteradmin)
      > 
      > 
      
      Ez a felhasználó 3 jogosultsággal kell rendelkeznie: toojoin gépek toohello megadott Active Directory-tartomány; a megadott szervezeti egység; toocreate szolgáltatásnevekről és a számítógép-objektumok hello belül és tooadd címfeloldási DNS proxy szabályokat.

Amíg létrehozása névkeresési DNS-zónák, hello parancsfájl kérni fogja tooenter a hálózati azonosítót. A hálózati azonosító hello erőforrás-kezelő virtuális hálózat címelőtagot kell lennie. Ha az erőforrás-kezelő virtuális hálózati alhálózat címteret 10.2.0.0/24, írja be például 10.2.0.0/24 amikor hello eszköz kér hello hálózati azonosítóval. 

## <a name="create-hdinsight-cluster"></a>HDInsight-fürt létrehozása
Ebben a szakaszban egy Linux-alapú Hadoop-fürt hdinsightban vagy hello Azure-portál használatával létrehozhat vagy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-template-deploy.md). Egyéb Fürtlétrehozási módszerekhez és hello beállításainak ismertetése, tanulmányozza a [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md). A Resource Manager sablon toocreate Hadoop használatával kapcsolatos további információk a HDInsight-fürtök, lásd: [létrehozása Hadoop-fürtök a HDInsight a Resource Manager-sablonok](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**a tartományhoz csatlakoztatott HDInsight fürt használt toocreate hello Azure-portálon**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **új**, **Eszközintelligencia + analitika**, majd **HDInsight**.
3. A hello **új HDInsight-fürt** panelen adja meg vagy válassza ki a következő értékek hello:
   
   * **Fürt neve**: Adjon meg egy új neve hello tartományhoz HDInsight-fürthöz.
   * **Előfizetés**: válassza ki a fürt létrehozásához használt Azure-előfizetéssel.
   * **Fürtkonfiguráció**:
     
     * **Fürt típusa**: Hadoop. Tartományhoz csatlakozó HDInsight jelenleg csak a támogatott Hadoop-fürtök.
     * **Operációs rendszer**: Linux.  HDInsight-tartományhoz csak a Linux-alapú HDInsight-fürtökön támogatott.
     * **Verzió**: Hadoop 2.7.3 (HDI 3.5). A HDInsight-tartományhoz csak HDInsight fürt 3.5-ös verziója támogatott.
     * **Fürt típusa**: prémium szintű
       
       Kattintson a **válasszon** toosave hello módosításokat.
   * **Hitelesítő adatok**: konfigurálja a hello hello fürt felhasználói és hello SSH-felhasználó hitelesítő adatait.
   * **Az adatforrás**: hozzon létre egy új tárfiókot, vagy az alapértelmezett tárfiók hello HDInsight-fürthöz hello meglévő tárfiókot használja. hello hely ugyanaz, mint a két Vnetek hello kell hello.  hello helye is hello hello HDInsight-fürt.
   * **Árképzési**: válassza ki a fürt munkavégző csomópontokhoz hello számát.
   * **Speciális beállításokat**: 
     
     * **Tartományhoz való csatlakozás & virtuális hálózatot/alhálózatot**: 
       
       * **Megtörtént a tartománybeállítások**: 
         
         * **Tartománynév**: contoso.onmicrosoft.com
         * **Tartományi felhasználónév**: Adjon meg egy tartományi felhasználó nevét. Ebben a tartományban hello a következő jogosultságokkal kell rendelkeznie: gépek toohello tartományhoz, és helyezze el őket a korábban megadott értékektől; hello szervezeti egység Szolgáltatásnevekről belül korábban megadott értékektől; hello szervezeti egység létrehozása Névkeresési DNS-bejegyzéseket létrehozni. A tartományi felhasználó a tartományhoz csatlakoztatott HDInsight-fürt hello rendszergazdája lesz.
         * **Tartományi jelszó**: hello tartományi felhasználói jelszó.
         * **Szervezeti egység**: hello hello korábban megadott értékektől szervezeti egység megkülönböztető nevét adja meg. Például: OU HDInsightOU, DC = contoso, DC = = onmicrosoft, DC = com
         * **LDAPS URL-cím**: ldaps://contoso.onmicrosoft.com:636
         * **Hozzáférés felhasználói csoport**: Adja meg a hello biztonsági csoportot, amelynek Ön a wan felhasználók toosync toohello fürt. Például HiveUsers.
           
           Kattintson a **válasszon** toosave hello módosításokat.
           
           ![Tartományhoz csatlakozó HDInsight portal tartomány beállítás konfigurálása](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtuális hálózati**: contosohdivnet
       * **Alhálózati**: Alhalozat_1
         
         Kattintson a **válasszon** toosave hello módosításokat.        
         Kattintson a **válasszon** toosave hello módosításokat.
   * **Erőforráscsoport**: Select hello erőforráscsoport hello HDInsight VNet (contosohdirg) használt.
4. Kattintson a **Create** (Létrehozás) gombra.  

A tartományhoz csatlakoztatott HDInsight-fürt létrehozásához egy másik lehetőség az toouse Azure Resource Manager sablon. a következő eljárás hello mutatja be:

**toocreate egy tartományhoz csatlakozó HDInsight-fürt erőforrás-kezelés sablon használatával**

1. Kattintson a következő kép tooopen hello Azure-portálon a Resource Manager sablon hello. hello Resource Manager-sablon a következő nyilvános blobtárolóban található. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure-use-powershell/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. A hello **paraméterek** panelen adja meg a következő értékek hello:
   
   * **Előfizetés**: (válassza ki az Azure-előfizetéshez).
   * **Erőforráscsoport**: kattintson a **meglévő**, és adja meg a hello ugyanahhoz az erőforráscsoporthoz tartozik használja.  Például contosohdirg. 
   * **Hely**: Adjon meg egy erőforráscsoport helye.
   * **A fürt neve**: Adja meg a létrehozandó hello Hadoop-fürt nevét. Például contosohdicluster.
   * **Fürt típusa**: válassza ki a fürt típusa.  hello alapértelmezett értéke **hadoop**.
   * **Hely**: hello fürt helyének kiválasztására.  hello alapértelmezett tárfiókot használja hello ugyanazon a helyen.
   * **A fürt feldolgozó csomópontok száma**: válassza ki a feldolgozó csomópontok száma hello.
   * **A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az **admin**.
   * **SSH-felhasználónév és jelszó**: hello alapértelmezett felhasználónév az **sshuser**.  Ezt át lehet nevezni. 
   * **Virtuális hálózati azonosító**: /subscriptions/&lt;; előfizetés-azonosító > /resourceGroups/&lt;erőforráscsoport-név > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >
   * **Virtuális hálózati alhálózat**: /subscriptions/&lt;; előfizetés-azonosító > /resourceGroups/&lt;erőforráscsoport-név > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >/alhálózatok/Alhalozat_1
   * **Tartománynév**: contoso.onmicrosoft.com
   * **Szervezeti egység megkülönböztető név**: OU HDInsightOU, DC = contoso, DC = = onmicrosoft, DC = com
   * **Fürt felhasználók csoport D Ns**: "\"CN HiveUsers, OU = AADDC Users, DC = =<DomainName>, DC = onmicrosoft, DC = com\""
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (hello tartományi rendszergazda felhasználó nevének megadása)
   * **Értéket**: (írja be a tartomány rendszergazdai jelszóval hello)
   * **Elfogadom toohello feltételek és kikötések fenti**: (ellenőrzés)
   * **PIN-kód toodashboard**: (ellenőrzés)
3. Kattintson a **Purchase** (Vásárlás) gombra. Egy új csempe jelenik meg **Deploying Template deployment** (Üzembe helyezés – Sablon telepítése) címmel. Vesz igénybe körülbelül 20 percet toocreate egy fürt. Hello fürt létrehozása után kattintson a fürt paneljén hello hello portál tooopen azt.

Hello az oktatóanyag befejezése után érdemes toodelete hello fürt. A HDInsight az Azure Storage szolgáltatásban tárolja az adatokat, így biztonságosan törölhet olyan fürtöket, amelyek nincsenek használatban. Ráadásul a HDInsight-fürtök akkor is díjkötelesek, amikor éppen nincsenek használatban. Mivel hello díjak hello fürt sokszor több mint hello a tárolási, érdemes gazdasági toodelete fürtök amikor nincsenek használatban. A fürtök törlésével a hello utasításokért lásd: [kezelése Hadoop-fürtöket a HDInsight használatával hello Azure-portálon](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Következő lépések

* A Hive-házirendek konfigurálásához és a Hive-lekérdezések futtatásához lásd: [Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-fürtökben](hdinsight-domain-joined-run-hive.md).
* SSH tooconnect tooDomain csatlakoztatott a HDInsight-fürtök használatával, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

