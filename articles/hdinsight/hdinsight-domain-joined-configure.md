---
title: "aaaConfigure tartományhoz a HDInsight-fürtök - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset össze, és a tartományhoz a HDInsight-fürtök konfigurálása"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 8c4b3d269a7662d27a49b839e5cd05a3e24f7023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Tartományhoz csatlakozó HDInsight-fürtök (előzetes verzió) konfigurálása

Ismerje meg, hogyan tooset mentése az Azure HDInsight-fürtöt Azure Active Directory (Azure AD) és [Apache Pletyka](http://hortonworks.com/apache/ranger/) erős hitelesítési és gazdag szerepköralapú hozzáférés-vezérlést (RBAC) házirendek tootake előnyeit.  A HDInsight-tartományhoz csak Linux-alapú fürtökön konfigurálható. További információkért lásd: [bevezetni a tartományhoz a HDInsight-fürtök](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie nincs engedélyezve a HDInsight-tartományhoz.

Ez a cikk az hello első oktatóanyaga, amely egy sor:

* Hozzon létre egy HDInsight fürt csatlakoztatott tooAzure AD (keresztül hello Azure Directory tartományi szolgáltatások képesség) kompatibilis Apache Pletyka.
* Hozzon létre és Pletyka Apache Hive-házirendeket alkalmazni, és lehetővé teszi a felhasználók (például adatszakértőkön) tooconnect tooHive ODBC-alapú eszközökkel, például az Excel, a Tableau stb. A Microsoft egyéb munkaterhelések, például a HBase, Spark, és a Storm, HDInsight tooDomain csatlakoztatott hamarosan hozzáadásával dolgozik.

Hello végső topológia például a következőképpen néz ki:

![A HDInsight-topológia a tartományhoz](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Mivel az Azure AD jelenleg csak a klasszikus virtuális hálózatokról (Vnetekről) támogatja, és Linux-alapú HDInsight-fürtök csak támogatási Azure Resource Manager Vnetek, HDInsight az Azure AD-integrációs van szükség, két virtuális hálózatokat és a társviszony-létesítés közöttük. Hello összehasonlító információk között hello két üzembe helyezési modellek: [Azure Resource Manager és klasszikus üzembe helyezési: üzembe helyezési modellel megértéséhez, valamint az erőforrások állapotát hello](../azure-resource-manager/resource-manager-deployment-model.md). két Vnetek kell hello hello hello Azure Active Directory tartományi szolgáltatások és ugyanabban a régióban.

Azure-szolgáltatás nevének globálisan egyedinek kell lennie. a következő neveket hello ebben az oktatóanyagban használt. Contoso nevű fiktív. Le kell cserélnie *contoso* egy eltérő nevű hello oktatóanyag lépve. 

**Nevek:**

| Tulajdonság | Érték |
| --- | --- |
| Az Azure AD-hálózatok |contosoaadvnet |
| Az Azure AD-Vnet erőforráscsoport |contosoaadrg |
| Azure AD-címtár |contosoaaddirectory |
| Az Azure AD-tartomány neve |Contoso (contoso.onmicrosoft.com) |
| HDInsight virtuális hálózat |contosohdivnet |
| HDInsight VNet erőforráscsoport |contosohdirg |
| HDInsight-fürt |contosohdicluster |

Ez az oktatóanyag egy tartományhoz csatlakozó HDInsight-fürt konfigurálásához hello lépéseit. Az egyes szakaszokon hivatkozások tooother cikkek tartalmaz további információt.

## <a name="prerequisite"></a>Előfeltétel:
* Ismerje meg a [Azure AD tartományi szolgáltatások](https://azure.microsoft.com/services/active-directory-ds/) a [árképzési](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktúra.
* Győződjön meg arról, hogy az előfizetés szerepel az engedélyezési listán az nyilvános előzetes verzió. Ehhez az e-mail toohdipreview@microsoft.com az előfizetés-azonosítóval.
* A tartomány egy aláíró hitelesítésszolgáltatóval által aláírt SSL-tanúsítvány. biztonságos LDAP konfigurálásával hello tanúsítvány szükséges. Önaláírt tanúsítványok nem használhatók.

## <a name="procedures"></a>Eljárások
1. Hozzon létre egy Azure klasszikus virtuális hálózatot az Azure AD.  
2. Hozzon létre, és konfigurálja az Azure AD és az Azure Active Directory tartományi Szolgáltatásokban.
3. Hozzon létre egy HDInsight virtuális hálózatot hello Azure-erőforrás felügyeleti mód.
4. Társ hello két Vnetek.
5. HDInsight-fürtök létrehozása.

> [!NOTE]
> Ez az oktatóanyag feltételezi, hogy nem rendelkezik az Azure AD. Ha nincs fiókja, kihagyhatja a 2. lépésben hello részét.
> 
> 

Nincs egy PowerShell-parancsfájlt, amely automatizálja a 3. lépés – 7. lépés.  További információkért lásd: [konfigurálása tartományhoz csatlakoztatott HDInsight-fürtök használata az Azure PowerShell](hdinsight-domain-joined-configure-use-powershell.md).

## <a name="create-an-azure-virtual-network-classic"></a>Hozzon létre egy Azure virtuális hálózat (klasszikus)
Ebben a szakaszban egy virtuális hálózat (klasszikus) Azure-portálon hello hoz létre. A következő szakaszban hello engedélyezte a hello Azure Active Directory tartományi szolgáltatások az Azure AD-beli virtuális hálózaton hello. Hello eljárást követi, és más virtuális hálózat létrehozása módszerekkel kapcsolatos további információkért lásd: [hozzon létre egy virtuális hálózat (klasszikus) hello Azure-portál használatával](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

**toocreate egy klasszikus virtuális hálózaton**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com). 
2. Kattintson a **új** > **hálózati** > **virtuális hálózati**.
3. A **telepítési modell kiválasztása**, jelölje be **klasszikus**, és kattintson a **létrehozása**.
4. Adja meg, vagy válassza ki a következő értékek hello:
   
   * **Név**: contosoaadvnet
   * **Címtér**: 10.1.0.0/16
   * **Alhálózati név**: Alhalozat_1
   * **Alhálózati címtartományt**: 10.1.0.0/24
   * **Előfizetés**: (válassza ki a virtuális hálózat létrehozásához használt előfizetés.)
   * **Erőforráscsoport**: contosoaadrg
   * **Hely**: (válasszon ki egy régiót a HDInsight-fürthöz.)
     
     > [!IMPORTANT]
     > Ki kell választania egy helyet, amely támogatja az Azure Active Directory tartományi Szolgáltatásokban. További információért lásd a [régiónként elérhető termékeket](https://azure.microsoft.com/en-us/regions/services/). 
     > 
     > Mindkét hello klasszikus virtuális hálózatot és hello erőforrás csoport virtuális hálózaton kell lennie a hello hello Azure Active Directory tartományi szolgáltatások és ugyanabban a régióban.
     > 
     > 
5. Kattintson a **létrehozása** toocreate hello virtuális hálózat.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Hozzon létre, és az Azure Active Directory tartományi szolgáltatások konfigurálása az Azure AD
Ez a szakasz tartalma:

1. Hozzon létre egy Azure AD.
2. Az Azure AD-felhasználók létrehozásához. Ezek a felhasználók tartományi felhasználók. Hello HDInsight-fürt konfigurálása az Azure AD hello hello első felhasználó használja.  hello más két felhasználók opcionálisak ehhez az oktatóanyaghoz. A használandó [konfigurálása Hive házirendek a tartományhoz a HDInsight-fürtök](hdinsight-domain-joined-run-hive.md) Apache Pletyka házirendek konfigurálásakor.
3. Hozzon létre hello AAD DC rendszergazdák csoportot, majd adja hozzá hello Azure AD felhasználói toohello csoportot. A felhasználó toocreate hello szervezeti egység használhatja.
4. Engedélyezi az Azure AD tartományi szolgáltatásokat (az Azure Active Directory tartományi szolgáltatások) hello Azure AD.
5. LDAPS konfigurálása az Azure AD hello. hello Lightweight Directory Access Protocol (LDAP) a használt tooread és tooAzure AD írni.

Egy meglévő Azure AD toouse tetszés szerint kihagyhatja az 1. és 2.

**az Azure AD toocreate**

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), kattintson a **új** > **alkalmazásszolgáltatások** > **Active Directory**  >  **Directory** > **egyéni létrehozás**. 
2. Adja meg, vagy válassza ki a következő értékek hello:
   
   * **Név**: contosoaaddirectory
   * **Tartománynév**: contoso.  Ez a név globálisan egyedinek kell lennie.
   * **Ország vagy régió**: válassza ki az országában vagy régiójában.
3. Kattintson a **Befejezés** gombra.

**Hozzon létre egy Azure AD-felhasználó**

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), kattintson a **Active Directory** -> **contosoaaddirectory**. 
2. Kattintson a **felhasználók** hello felső menüjében.
3. Kattintson a **felhasználó hozzáadása**.
4. Adja meg **felhasználónév**, és kattintson a **következő**. 
5. Felhasználói profil; konfigurálása A **szerepkör**, jelölje be **globális rendszergazda**; majd **következő**.  hello globális rendszergazdai szerepkör szükséges toocreate szervezeti egység.
6. Kattintson a **létrehozása** tooget ideiglenes jelszót.
7. Másolatot készít hello jelszót, és kattintson a **Complete**. Az oktatóanyag későbbi részében szüksége lesz a globális rendszergazdai felhasználói toocreate hello HDInsight-fürthöz.

Hajtsa végre hello azonos eljárás toocreate két további felhasználók rendelkező hello **felhasználói** szerepkör, hiveuser1 és hiveuser2. hello következő felhasználók használandó [tartományhoz a HDInsight-fürtök házirendek konfigurálása Hive](hdinsight-domain-joined-run-hive.md).

**toocreate hello AAD DC rendszergazdák csoportnak, és az Azure AD-felhasználó hozzáadása**

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), kattintson a **Active Directory** > **contosoaaddirectory**. 
2. Kattintson a **csoportok** hello felső menüjében.
3. Kattintson a **csoport hozzáadása** vagy **csoport hozzáadása**.
4. Adja meg, vagy válassza ki a következő értékek hello:
   
   * **Név**: DC rendszergazdák aad-ben.  Hello csoport neve nem módosítható.
   * **Csoporttípust**: biztonsági.
5. Kattintson a **Befejezés** gombra.
6. Kattintson a **AAD DC rendszergazdák** tooopen hello csoport.
7. Kattintson a **tagok hozzáadása**.
8. Válassza ki a felhasználó első hello hello előző lépésben létrehozott, és kattintson **Complete**.
9. Ismétlődő hello azonos lépéseket toocreate egy másik csoport nevű **HiveUsers**, majd adja hozzá a két hello Hive felhasználók toohello csoportot.

További információkért lásd: [Azure AD tartományi szolgáltatások (előzetes verzió) – létrehozása hello "AAD DC rendszergazdák" csoportba](../active-directory-domain-services/active-directory-ds-getting-started.md).

**az Azure AD az Azure Active Directory tartományi szolgáltatások tooenable**

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), kattintson a **Active Directory** > **contosoaaddirectory**. 
2. Kattintson a **konfigurálása** hello felső menüjében.
3. Görgessen lefelé, túl**tartományi szolgáltatások**, és a set hello a következő értékeket:
   
   * **A címtárban tartományi szolgáltatások engedélyezése**: Igen.
   * **DNS-tartománynevet a tartományi szolgáltatások**: Ez azt jelenti, hogy az Azure directory hello hello alapértelmezett DNS-nevét. Például a contoso.onmicrosoft.com.
   * **Tartományi szolgáltatások toothis virtuális hálózat**: válassza ki a korábban létrehozott klasszikus virtuális hálózatot hello azaz **contosoaadvnet**.
4. Kattintson a **mentése** hello lap hello lista aljáról. Látni fogja **folyamatban...**  következő túl**engedélyezése tartományi szolgáltatásokat a címtárhoz**.  
5. Várjon, amíg **folyamatban...**  eltűnik, és **IP-cím** lekérdezi feltöltve. Két IP-címet fogja lekérni feltöltve. Ezek a tartományi szolgáltatások által kiosztott hello tartományvezérlők hello IP-címét. Minden IP-cím után hello megfelelő tartományvezérlőn létesítése sikeres, és készen áll a lesznek láthatók. Írja le hello két IP-címet. Később szüksége lesz rájuk.

További információkért lásd: [Azure AD tartományi szolgáltatások (előzetes verzió) – engedélyezi az Azure AD tartományi szolgáltatások](../active-directory-domain-services/active-directory-ds-getting-started-enableaadds.md).

**toosynchronize jelszó**

Ha a saját tartomány használata esetén meg kell toosynchronize hello jelszó. Lásd: [engedélyezi jelszó szinkronizálási tooAzure AD tartományi szolgáltatásokat egy kizárólag felhőalapú Azure AD-címtár](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).

**az Azure AD hello LDAPS tooconfigure**

1. A tartomány egy aláíró hitelesítésszolgáltatóval által aláírt SSL-tanúsítvány beszerzése. Ha azt szeretné, hogy toouse egy önaláírt tanúsítványt, lépjen kapcsolatba toohdipreview@microsoft.com kivételt.
2. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), kattintson a **Active Directory** > **contosoaaddirectory**. 
3. Kattintson a **konfigurálása** hello felső menüjében.
4. Görgessen túl**tartományi szolgáltatások**.
5. Kattintson a **konfigurálása tanúsítvány**.
6. Hajtsa végre a hello utasítás toospecify hello tanúsítványfájl és hello jelszót. Látni fogja **folyamatban...**  következő túl**engedélyezése tartományi szolgáltatásokat a címtárhoz**.  
7. Várjon, amíg **folyamatban...**  eltűnik, és **biztonságos LDAP tanúsítvány** fel lett töltve.  A is tarthat 10 perc vagy több.

> [!NOTE]
> Az Azure Active Directory tartományi szolgáltatások hello néhány háttérfeladatok készül futtatni, jelenhet meg tanúsítványának feltöltése - hibát <i>van egy művelet végrehajtás alatt álló ennél a bérlőnél. Próbálkozzon újra később</i>.  Ha ezt a hibát tapasztal, próbálja meg újra némi várakozás után. hello második tartományvezérlő IP-Címének too3 óra toobe kiépített is tarthat.
> 
> 

További információkért lásd: [konfigurálása biztonságos LDAP (LDAPS) egy Azure AD tartományi szolgáltatások által felügyelt tartomány](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>A HDInsight-fürt erőforrás-kezelő VNet létrehozása
Ebben a szakaszban egy Azure Resource Manager virtuális hálózatot, amely jelzi a HDInsight-fürt hello hoz létre. Azure-hálózatok más módszerekkel egyéni további információkért lásd: [virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)

Miután létrehozta a virtuális hálózat hello, erőforrás-kezelő virtuális hálózat toouse hello azonos DNS-kiszolgálókat, a hello Azure AD VNet hello állít. Ha követte az oktatóanyag toocreate hello lépéseit hello klasszikus virtuális hálózat és az Azure AD hello hello DNS-kiszolgálók 10.1.0.4 és 10.1.0.5.

**egy erőforrás-kezelő virtuális hálózat toocreate**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **új**, **hálózati**, majd **virtuális hálózati**. 
3. A **telepítési modell kiválasztása**, jelölje be **erőforrás-kezelő**, és kattintson a **létrehozása**.
4. Írja be vagy válassza ki a következő értékek hello:
   
   * **Név**: contosohdivnet
   * **Címtér**: 10.2.0.0/16. Ellenőrizze, hogy a címtartomány hello hello hello IP-címtartománya nem lehet átfedésben klasszikus virtuális hálózatot.
   * **Alhálózati név**: Alhalozat_1
   * **Alhálózati címtartományt**: 10.2.0.0/24
   * **Előfizetés**: (válassza ki az Azure-előfizetéshez.)
   * **Erőforráscsoport**: contosohdirg
   * **Hely**: (válasszon hello ugyanazon a helyen, az Azure AD VNet, azaz contosoaadvnet hello.)
5. Kattintson a **Create** (Létrehozás) gombra.

**az erőforrás-kezelő virtuális hálózat hello DNS tooconfigure**

1. A hello [Azure-portálon](https://portal.azure.com), kattintson a **további szolgáltatások** -> **virtuális hálózatok**. Tooclick nem biztosítására **virtuális hálózatok (klasszikus)**.
2. Kattintson a **contosohdivnet**.
3. Kattintson a **DNS-kiszolgálók** a hello bal oldalán található hello új panelen.
4. Kattintson a **egyéni**, és írja be a következő értékek hello:
   
   * 10.1.0.4
   * 10.1.0.5
     
     A DNS-kiszolgáló IP-címének egyeznie kell a toohello DNS-kiszolgálók hello Azure AD-hálózatok (klasszikus virtuális hálózaton).
5. Kattintson a **Save** (Mentés) gombra.

## <a name="peer-hello-azure-ad-vnet-and-hello-hdinsight-vnet"></a>Bejövő hello Azure AD VNet és HDInsight VNet hello
**toopeer hello két VNet**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **további szolgáltatások** hello bal oldali menüből.
3. Kattintson a **virtuális hálózatok**. Ne kattintson **virtuális hálózatok (klasszikus)**.
4. Kattintson a **contosohdivnet**.  Ez a hello HDInsight virtuális hálózat.
5. Kattintson a **Társviszony** hello bal oldali menüben hello panelről.
6. Kattintson a **Hozzáadás** hello felső menüjében. Hello nyílik **hozzáadása a társviszony-létesítés** panelen.
7. A hello **Hozzáadás társviszony-létesítés** panelen, a következő értékeket állítsa be vagy válassza hello:
   
   * **Név**: ContosoAADHDIVNetPeering
   * **Virtuális hálózat üzembe helyezési modellel**: klasszikus
   * **Előfizetés**: Jelölje ki a hello (az Azure AD) klasszikus virtuális hálózaton használt előfizetés nevét.
   * **Virtuális hálózati**: contosoaadvnet.
   * **Virtuális hálózati hozzáférés engedélyezése**: (ellenőrzés)
   * **Engedélyezi a továbbított forgalmat**: (ellenőrzés). Hagyja üresen hello más két négyzet jelölését.
8. Kattintson az **OK** gombra.

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
     * **Verzió**: HDI 3.6. A HDInsight-tartományhoz csak a HDInsight-fürt verziószáma 3.6 támogatott.
     * **Fürt típusa**: prémium szintű
       
       Kattintson a **válasszon** toosave hello módosításokat.
   * **Hitelesítő adatok**: konfigurálja a hello hello fürt felhasználói és hello SSH-felhasználó hitelesítő adatait.
   * **Az adatforrás**: hozzon létre egy új tárfiókot, vagy az alapértelmezett tárfiók hello HDInsight-fürthöz hello meglévő tárfiókot használja. hello hely ugyanaz, mint a két Vnetek hello kell hello.  hello helye is hello hello HDInsight-fürt.
   * **Árképzési**: válassza ki a fürt munkavégző csomópontokhoz hello számát.
   * **Speciális beállításokat**: 
     
     * **Tartományhoz való csatlakozás & virtuális hálózatot/alhálózatot**: 
       
       * **Megtörtént a tartománybeállítások**: 
         
         * **Tartománynév**: contoso.onmicrosoft.com
         * **Tartományi felhasználónév**: Adjon meg egy tartományi felhasználó nevét. Ebben a tartományban hello a következő jogosultságokkal kell rendelkeznie: gépek toohello tartományhoz, és helyezze el őket hello szervezeti egység során fürt létrehozása; Szolgáltatásnevekről belül; fürt létrehozásakor megadott hello szervezeti egység létrehozása Névkeresési DNS-bejegyzéseket létrehozni. A tartományi felhasználó a tartományhoz csatlakoztatott HDInsight-fürt hello rendszergazdája lesz.
         * **Tartományi jelszó**: hello tartományi felhasználói jelszó.
         * **Szervezeti egység**: hello hello, amelyet a HDInsight-fürthöz toouse szervezeti egység megkülönböztető nevét adja meg. Például: OU HDInsightOU, DC = contoso, DC = = onmicrosoft, DC = com. Ha a szervezeti egység nem létezik, HDInsight-fürt megpróbál toocreate a szervezeti egység. Győződjön meg arról, hello OU már létezik, vagy hello tartományi fiók rendelkezik engedélyekkel toocreate egy újat. Hello tartományi fiók, amely része AADDC rendszergazdák használatakor lesz szükséges engedélyek toocreate hello szervezeti Egységet.
         * **LDAPS URL-cím**: ldaps://contoso.onmicrosoft.com:636
         * **Hozzáférés felhasználói csoport**: Adja meg azt az hello biztonsági csoportot, amelynek felhasználók toosync toohello fürt. Például HiveUsers.
           
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
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure/deploy-to-azure.png" alt="Deploy tooAzure"></a>
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
   * **Fürt felhasználói csoport DNs**: [\"HiveUsers\"]
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (hello tartományi rendszergazda felhasználó nevének megadása)
   * **Értéket**: (írja be a tartomány rendszergazdai jelszóval hello)
   * **Elfogadom toohello feltételek és kikötések fenti**: (ellenőrzés)
   * **PIN-kód toodashboard**: (ellenőrzés)
3. Kattintson a **Purchase** (Vásárlás) gombra. Egy új csempe jelenik meg **Deploying Template deployment** (Üzembe helyezés – Sablon telepítése) címmel. Vesz igénybe körülbelül 20 percet toocreate egy fürt. Hello fürt létrehozása után kattintson a fürt paneljén hello hello portál tooopen azt.

Hello az oktatóanyag befejezése után érdemes toodelete hello fürt. A HDInsight az Azure Storage szolgáltatásban tárolja az adatokat, így biztonságosan törölhet olyan fürtöket, amelyek nincsenek használatban. Ráadásul a HDInsight-fürtök akkor is díjkötelesek, amikor éppen nincsenek használatban. Mivel hello díjak hello fürt sokszor több mint hello a tárolási, érdemes gazdasági toodelete fürtök amikor nincsenek használatban. A fürtök törlésével a hello utasításokért lásd: [kezelése Hadoop-fürtöket a HDInsight használatával hello Azure-portálon](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Következő lépések
* A Hive-házirendek konfigurálásához és a Hive-lekérdezések futtatásához lásd: [Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-fürtökben](hdinsight-domain-joined-run-hive.md).
* SSH tooconnect tooDomain csatlakoztatott a HDInsight-fürtök használatával, lásd: [SSH használata a HDInsight Linux, Unix vagy OS X, Linux-alapú Hadooppal](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

