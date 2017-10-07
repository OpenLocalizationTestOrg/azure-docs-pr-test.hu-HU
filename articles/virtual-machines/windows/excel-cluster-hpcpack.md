---
title: "az Excel és SOA fürt aaaHPC Pack |} Microsoft Docs"
description: "Ismerkedés a nagyméretű Excel és a SOA munkaterhelések egy HPC Pack fürtben futó Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: cb6a9abe-caf3-44da-b911-849a50f6cfb3
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 55b4b2c25fe65d06b75025cc23c3c13b8b764238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Ismerkedés az Excel és a SOA munkaterhelések egy HPC Pack fürtben futó Azure-ban
Ez a cikk bemutatja, hogyan toodeploy a Microsoft HPC Pack 2012 R2 fürt Azure virtuális gépeken futó Azure gyors üzembe helyezés sablonná és opcionálisan egy Azure PowerShell telepítési parancsfájlt. hello fürt HPC Pack használ az Azure piactér virtuális gép tervezett képek toorun Microsoft Excel vagy szolgáltatásorientált architektúra (SOA) munkaterhelések. Hello fürt toorun Excel HPC és SOA szolgáltatások egy helyszíni ügyfélszámítógépről is használhatja. hello Excel HPC-szolgáltatások közé tartoznak az Excel-munkafüzet kiürítéséhez és a felhasználó által definiált függvények Excel vagy a felhasználó által megadott függvények.

> [!IMPORTANT] 
> Ez a cikk a HPC Pack 2012 R2 alapul funkciók, sablonok és parancsfájlok. Ebben a forgatókönyvben jelenleg nem támogatott a HPC Pack 2016.
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Magas szinten, hello alábbi ábrán látható hello HPC Pack fürtöt hoz létre.

![Excel munkaterheket futtatnak csomópontok HPC-fürt][scenario]

## <a name="prerequisites"></a>Előfeltételek
* **Ügyfélszámítógép** -ügyfél Windows-alapú számítógép toosubmit minta Excel és SOA feladatok toohello fürt van szüksége. A Windows számítógép toorun hello Azure PowerShell fürt telepítési parancsfájlt is (ha az adott központi telepítési módszer választása) kell.
* **Azure-előfizetés** – Ha nem rendelkezik Azure-előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) néhány percig.
* **Magok kvóta** -szükség lehet tooincrease hello beállított kvótát mag, különösen akkor, ha több fürtcsomóponton multicore Virtuálisgép-méretek és telepít. Egy Azure gyors üzembe helyezés sablont használ, ha hello magok kvóta az erőforrás-kezelőben / Azure-régióban van. Ebben az esetben szükség lehet egy adott régióban tooincrease hello kvótát. Lásd: [Azure-előfizetésre vonatkozó korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md). a kvóta tooincrease [nyissa meg az online támogatás ügyfélkérés](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) díjmentesen.
* **A Microsoft Office-licencet** – Ha a számítási csomópontok piactér HPC Pack 2012 R2 Virtuálisgép-lemezkép használatával a Microsoft Excel, a 30 napos próbaverzióját Microsoft Excel Professional Plus 2013 telepítve van. Hello próbaidőszak után munkaterheléseknek tooprovide egy érvényes Microsoft Office licenc tooactivate Excel toocontinue toorun. Lásd: [Excel-aktiválási](#excel-activation) című cikkben. 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>1. lépés Az Azure-ban egy HPC Pack fürt beállítása
Hello HPC Pack 2012 R2-fürt két beállítások tooset megmutatjuk: első, egy sablon Azure gyors üzembe helyezés és hello Azure-portálon; és a második, az Azure PowerShell telepítési parancsfájlt használ.

### <a name="option-1-use-a-quickstart-template"></a>1. lehetőség. A következő gyorsindítási sablonon használata
Használja az Azure gyors üzembe helyezés sablon tooquickly HPC Pack-fürt üzembe helyezése a hello Azure-portálon. Hello sablon hello portál megnyitásakor egy egyszerű felhasználói felület, ahol hello-beállítások megadása a fürt kap. Az alábbiakban hello lépéseket. 

> [!TIP]
> Ha azt szeretné, egy [Azure piactér sablon](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) , amely kifejezetten a Excel munkaterhelések hasonló fürt hoz létre. hello lépések némileg eltérő hello következő.
> 
> 

1. A Microsoft hello [sablonlap HPC-fürt létrehozása a Githubon](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Ha azt szeretné, tekintse át a hello sablon és hello forráskód adatait.
2. Kattintson a **tooAzure telepítése** toostart a központi telepítés sablonnal hello hello Azure-portálon.
   
   ![Sablon tooAzure telepítése][github]
3. Hello portálon hajtsa végre az ezen lépések tooenter hello paraméterek hello HPC-fürt sablon.
   
   a. A hello **paraméterek** lapon adja meg, vagy módosítsa hello sablon paraméter értékét. (Kattintson a hello ikon következő tooeach beállítás súgójában talál.) A következő képernyő hello mintaértékek láthatók. Ez a példa egy nevű fürtöt hoz létre *hpc01* a hello *hpc.local* tartomány egy átjárócsomóponttal és 2 álló számítási csomópontokat. hello számítási csomópontok HPC Pack VM-lemezkép, amely tartalmazza a Microsoft Excel készített.
   
   ![Adja meg a paraméterek][parameters-new-portal]
   
   > [!NOTE]
   > virtuális gép automatikusan létrejön a hello hello átjárócsomópont [legújabb Piactéri lemezképhez](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 Windows Server 2012 R2 rendszeren. Jelenleg hello kép HPC Pack 2012 R2 Update 3 alapul.
   > 
   > A számítási csomópont virtuális gépek jönnek létre a kiválasztott hello számítási csomópont termékcsalád hello legújabb lemezképről. Jelölje be hello **ComputeNodeWithExcel** hello legújabb HPC Pack számítási csomópont-kép próbaverziójáról a Microsoft Excel Professional Plus 2013 beállítást. az általános SOA munkamenetekhez, illetve az Excel UDF-ben történő kiszervezésével a toodeploy válasszon hello **Átjárócsomópontján** (nélkül telepített Excel) lehetőséget.
   > 
   > 
   
   b. Válasszon hello előfizetést.
   
   c. Például a hello fürt erőforráscsoport létrehozása *hpc01RG*.
   
   d. Válasszon egy helyet hello erőforráscsoport, például az USA középső RÉGIÓJA.
   
   e. A hello **jogi feltételeket** lapján tekintse át a hello feltételeket. Ha elfogadja, kattintson a **beszerzési**. Ha elkészült, hello sablon hello beállításértékek, kattintson a **létrehozása**.
4. Hello telepítési befejeződésekor (általában tart körülbelül 30 percig), hello fürt tanúsítványfájl exportálása hello fürt átjárócsomópontjából. Egy későbbi lépésben importálja ezt a nyilvános tanúsítványt hello tooprovide hello kiszolgálóoldali ügyfélszámítógépeket biztonságos HTTP-kötés.
   
   a. Nyissa meg toohello irányítópultot, válassza hello átjárócsomópont hello Azure-portálon, és **Connect** hello lap tooconnect távoli asztali kapcsolattal hello tetején.
   
    <!-- ![Connect toohello head node][connect] -->
   
   b. Eljárásokkal szabványos Tanúsítványkezelő tooexport hello átjárócsomópont tanúsítványban (az található a Cert: \LocalMachine\My) hello titkos kulcs nélkül. Ebben a példában exportálása *CN = hpc01.eastus.cloudapp.azure.com*.
   
   ![Hello tanúsítvány exportálása][cert]

### <a name="option-2-use-hello-hpc-pack-iaas-deployment-script"></a>2. lehetőség. Hello HPC Pack IaaS telepítési parancsfájl használata
HPC Pack IaaS telepítési parancsfájl hello biztosít egy másik sokoldalú módon toodeploy egy HPC Pack fürthöz. A fürt létrehoz hello klasszikus üzembe helyezési modellel, mivel hello a sablon által hello Azure Resource Manager üzembe helyezési modellben. Hello parancsfájlt is kompatibilis Azure globális hello vagy Azure Kína szolgáltatás előfizetés.

**További Előfeltételek**

* **Az Azure PowerShell** - [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) (0.8.10 verzió vagy újabb) az ügyfélszámítógépen.
* **HPC Pack IaaS telepítési parancsfájl** - töltse le és csomagolja ki a legújabb verziójának hello hello hello parancsfájlt [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Verzióellenőrzés hello hello parancsfájl futtatásával `New-HPCIaaSCluster.ps1 –Version`. Ez a cikk verzióján 4.5.0 vagy későbbi hello parancsfájl alapul.

**Hello konfigurációs fájl létrehozása**

 hello HPC Pack IaaS telepítési parancsfájl egy konfigurációs XML-fájl hello infrastruktúra hello HPC-fürt bemenetként használja. a fürt egy átjárócsomóponttal és 18 álló számítási csomópontjain hello számítási csomópont lemezkép, amely tartalmazza a Microsoft Excel alapján létre toodeploy értékek környezetnek a következő minta konfigurációs fájl hello helyettesítse. Hello konfigurációs fájllal kapcsolatos további információkért lásd: hello Manual.rtf fájlban hello parancsfájl és [HPC-fürt létrehozása hello HPC Pack IaaS telepítési parancsfájl](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Hello konfigurációs fájllal kapcsolatos megjegyzések**

* Hello **VMName** hello központi csomópont **kell** kell hello ugyanaz, mint a hello **szolgáltatásnév**, vagy SOA-feladatok sikertelenek toorun.
* Győződjön meg arról, hogy a megadott **EnableWebPortal** , hogy hello átjárócsomópont tanúsítvány jön létre, és exportálja.
* hello fájl konfiguráció utáni PowerShell-parancsfájl hello átjárócsomópont futó PostConfig.ps1 határozza meg. a következő mintaparancsfájl hello hello Azure tárolási kapcsolati karakterlánc konfigurálása, hello számítási csomópont szerepkör eltávolítása hello átjárócsomópont és összes csomópont online elérését, amikor központilag telepítették őket. 

```
    # add hello HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set hello Azure storage connection string for hello cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove hello compute node role for head node toomake sure hello Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in hello deployment including hello head node and compute nodes, which should match hello number specified in hello XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Hello parancsfájl futtatása**

1. Nyissa meg rendszergazdaként egy PowerShell-konzolban hello hello ügyfélszámítógépen.
2. Directory toohello parancsfájl mappa módosítása (E:\IaaSClusterScript ebben a példában).
   
   ```
   cd E:\IaaSClusterScript
   ```
3. toodeploy hello HPC Pack fürthöz, futtassa a következő parancs hello. Ez a példa feltételezi, hogy hello a konfigurációs fájl E:\HPCDemoConfig.xml található.
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

hello HPC Pack telepítési parancsfájlt futtatja egy kis ideig. Egy dolog hello parancsprogram tooexport és hello fürt tanúsítvány letöltése és mentheti hello ügyfélszámítógépen hello aktuális felhasználó Dokumentumok mappájának. hello parancsfájl állít elő, egy üzenet hasonló toohello következő. A következő lépésben hello tanúsítványt hello megfelelő tanúsítványtárolójában kell importálnia.    

    You have enabled REST API or web portal on HPC Pack head node. Please import hello following certificate in hello Trusted Root Certification Authorities certificate store on hello computer where you are submitting job or accessing hello HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>2. lépés Excel-munkafüzetek kiszervezése és a felhasználó által megadott függvények futtatása egy helyszíni ügyfélről
### <a name="excel-activation"></a>Excel-aktiválás
A termelési számítási feladatokhoz hello ComputeNodeWithExcel Virtuálisgép-lemezkép használatakor szüksége tooprovide egy érvényes Microsoft Office licenc kulcs tooactivate Excel hello számítási csomóponton. Ellenkező esetben Excel hello próbaverzióját 30 nap múlva lejár, és hello COMException (0x800AC472) Excel-munkafüzetek futtatása meghiúsul. 

Az értékelés időpontjához további 30 napra is állíthatnak alaphelyzetbe Excel: toohello átjárócsomópont és clusrun bejelentkezés `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` összes Excel a számítási csomópontok HPC Cluster Manager keresztül. Legfeljebb kétszer is újra. Ezt követően meg kell adnia egy érvényes Office licenckulcs.

hello Office Professional Plus 2013 telepítve van a Virtuálisgép-lemezkép hello mennyiségi kiadását az általános mennyiségi licenc kulcsot (GVLK). Kulcskezelő szolgáltatás (KMS) keresztül is aktiválhatja vagy Active Directory-alapú aktiválás (AD-BA), vagy a többször használható aktiválási kulcs (MAK). 

    * toouse KMS/AD-BA, a meglévő KMS-kiszolgáló használata, vagy állítson be egy új hello Microsoft Office 2013 mennyiségi licenc csomag használatával. (Ha kívánja, hello kiszolgáló beállítása hello központi csomóponton.) Ezután aktiválja hello KMS-állomás kulcsát hello interneten keresztül vagy telefonon keresztül. Majd clusrun `ospp.vbs` tooset hello KMS-kiszolgáló és a portot, és Office aktiválja az összes hello Excel számítási csomóponton. 

    * MAK-ot, első clusrun toouse `ospp.vbs` tooinput hello kulcs, és majd aktiválása az összes hello Excel számítási csomópontok hello interneten vagy telefonon keresztül. 

> [!NOTE]
> Ez a Virtuálisgép-lemezkép nem használható Office Professional Plus 2013 kereskedelmi termékkulcsokat. Ha van érvényes kulcsok és a telepítési adathordozó Office vagy az Excel kiadás kivételével az Office Professional Plus 2013 mennyiségi kiadását, használhatja őket helyette. Először távolítsa el a mennyiségi kiadását, és telepítse, hogy rendelkezik hello verziót. hello Excel számítási csomópont rögzíthető, a testre szabott VM kép toouse léptékű központi telepítés újratelepítése.
> 
> 

### <a name="offload-excel-workbooks"></a>Excel-munkafüzetek kiszervezése
Kövesse ezeket a lépéseket toooffload egy Excel-munkafüzet futtatása az Azure-ban hello HPC Pack fürtön. toodo, rendelkeznie kell az Excel 2010 vagy 2013 hello ügyfélszámítógépen már telepítve van.

1. Használja az 1. lépés toodeploy egy HPC Pack fürt hello Excel hello-beállítások számítási csomópont kép. Szerezze be a hello fürt tanúsítványfájlt (.cer) és a fürt felhasználónevet és jelszót.
2. Hello ügyfélszámítógépen hello fürt tanúsítványt a Cert: \CurrentUser\Root importálni.
3. Győződjön meg arról, hogy telepítve van a Excel. Hozzon létre egy Excel.exe.config fájlt hello hello tartalmát a következő mappában, amelyben Excel.exe hello ügyfélszámítógépen. Ez a lépés biztosítja, hogy hello HPC Pack 2012 R2 Excel COM beépülő modul sikeresen betöltődik.
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. Hello ügyfél toosubmit feladatok toohello HPC Pack fürt beállítása. Egy elem toodownload hello teljes [HPC Pack 2012 R2 Update 3 telepítési](http://www.microsoft.com/download/details.aspx?id=49922) és hello HPC Pack ügyfél telepítéséhez. Azt is megteheti, töltse le és telepítse a hello [HPC Pack 2012 R2 Update 3 ügyfél segédprogramok](https://www.microsoft.com/download/details.aspx?id=49923) és a megfelelő Visual C++ 2010 újraterjeszthető csomag a számítógép hello ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).
5. A jelen példában használjuk ConvertiblePricing_Complete.xlsb nevű minta Excel-munkafüzet. Letöltheti a [Itt](https://www.microsoft.com/en-us/download/details.aspx?id=2939).
6. Hello Excel munkafüzet tooa munkamappa D:\Excel\Run például másolja.
7. Nyissa meg a hello Excel-munkafüzet. A hello **Develop** menüszalag, kattintson a **COM-bővítmények** , és győződjön meg arról, hogy hello HPC Pack Excel COM-bővítmény sikeresen be van töltve.
   
   ![Excel-bővítmény HPC Pack][addin]
8. Szerkesztés hello VBA makró Excel HPCControlMacros hello módosításával sorok megjegyzésként, ahogy az a következő parancsfájl hello. Helyettesítse be a környezetének megfelelő értékeket.
   
   ![HPC Pack Excel makró][macro]
   
   ```
   'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
   Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
   
   'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
   Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.Initialize ActiveWorkbook
   HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
   
   'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
   HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
   HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
   ```
9. Hello Excel munkafüzet tooan feltöltés címtár D:\Excel\Upload például másolja. Ez a könyvtár hello HPC_DependsFiles állandó hello VBA makró van megadva.
10. toorun hello munkafüzet hello fürtön, az Azure-ban kattintson hello **fürt** hello munkalapon gombra.

### <a name="run-excel-udfs"></a>Excel univerzális Lemezformátumokat futtatása
Excel univerzális Lemezformátumokat toorun hajtsa végre az előző lépésekben hello 1 – 3 tooset hello ügyfélszámítógépet. Excel univerzális Lemezformátumokat toohave hello Excel alkalmazás telepítve van a számítási csomópontok nincs szükség. Amikor a fürt létrehozása számítási csomópontot, válassza a normál számítási csomópont lemezkép helyett hello számítási csomópont rendszerképének Excel.

> [!NOTE]
> Létrejön egy 34 karakteres korlátot, az Excel 2010 hello és 2013 fürt összekötő párbeszédpanel megnyitásához. A párbeszédpanel bezárásához toospecify hello futtató fürtöt hello felhasználó által megadott függvények használhatja. Ha hosszabb hello teljes fürt neve (például hpcexcelhn01.southeastasia.cloudapp.azure.com), nem fér el hello párbeszédpanel. hello megoldás, tooset például gépre kiterjedő változó *CCP_IAASHN* hello hosszú fürtnév hello értékű. Ezután írja be a *CCP_IAASHN %* hello párbeszédpanelen hello fürt átjárócsomópontjához neveként. 
> 
> 

Hello fürt sikeres telepítése után folytassa a következő lépéseket toorun hello minta beépített Excel UDF. A testre szabott Excel univerzális Lemezformátumokat tapasztalja [erőforrások](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) toobuild hello XLL-EK, és telepítheti azokat a hello IaaS-fürtön.

1. Nyisson meg egy új Excel-munkafüzet. A hello **Develop** menüszalag, kattintson a **bővítmények**. A hello párbeszédpanelen kattintson **Tallózás**toohello %CCP_HOME%Bin\XLL32 mappa keresse meg és válassza ki a hello minta ClusterUDF32.xll. Ha hello ClusterUDF32 nem létezik hello ügyfélszámítógépen, másolásához hello átjárócsomópont hello %CCP_HOME%Bin\XLL32 mappából.
   
   ![Válassza ki a hello UDF-ben][udf]
2. Kattintson a **fájl** > **beállítások** > **speciális**. A **képletek**, ellenőrizze **engedélyezése a felhasználó által definiált XLL funkciók toorun a számítási fürt**. Kattintson a **beállítások** , és írja be a teljes fürt neve hello a **fürt átjárócsomópontjához neve**. (Részben ismertetett beállításértékeket korábban a beviteli mezőbe az korlátozott too34 karakterből állhat, így előfordulhat, hogy nem felelnek meg egy hosszú neve. Használhatja a gépre kiterjedő változó nevét egy hosszú fürtnév.)
   
   ![Az UDF hello konfigurálása][options]
3. toorun hello UDF számítási fürtön hello, hello cellába, amelynek értéke =XllGetComputerNameC() kattintson, és nyomja le az ENTER billentyűt. hello függvény egyszerűen hello számítási csomópont, mely hello UDF fut. hello nevét kéri le. A hello először futtatja a hitelesítő adatok párbeszédpanel hello felhasználónév és jelszó tooconnect toohello IaaS fürt megadását kéri.
   
   ![Futtassa az UDF-ben][run]
   
   Ha sok cellát toocalculate, nyomja le az Alt-Shift-Ctrl + F9 toorun hello számítás összes cellákon.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>3. lépés A SOA munkaterhelések futtatásához egy helyszíni ügyfélről
általános SOA alkalmazások toorun hello HPC Pack IaaS fürtön, először hello módszerek valamelyikével 1. lépés toodeploy hello fürtben. Adjon meg általános számítási csomópont lemezképet ebben az esetben, mert Excel hello számítási csomóponton nem szükséges. Ezután kövesse az alábbi lépéseket.

1. Hello fürt tanúsítvány beolvasása, után importálja a Cert: \CurrentUser\Root hello ügyfélszámítógépen.
2. Telepítse a hello [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) és [HPC Pack 2012 R2 Update 3 ügyfél segédprogramok](https://www.microsoft.com/download/details.aspx?id=49923). Ezek az eszközök lehetővé teszik a toodevelop és SOA ügyfélalkalmazások futtatását.
3. Töltse le a hello HelloWorldR2 [példakód](https://www.microsoft.com/download/details.aspx?id=41633). Nyissa meg a Visual Studio 2010 HelloWorldR2.sln hello vagy 2012. (Ez a minta nincs jelenleg kompatibilis a Visual Studio legújabb verziói.)
4. Először a hello EchoService projekt felépítéséhez. Ezt követően a hello szolgáltatás toohello IaaS-fürt központi telepítése a hello ugyanúgy telepítheti tooan a helyi fürthöz. Részletes útmutató: a HelloWordR2 Readme.doc hello. Módosíthatja, és hello HellWorldR2 és más projektek hozhat létre a következő szakasz toogenerate hello SOA ügyfélalkalmazások Azure IaaS fürtökön futó hello leírtak szerint.

### <a name="use-http-binding-with-azure-storage-queue"></a>Http-kötés használata az Azure storage üzenetsorába
egy Azure storage üzenetsorába toouse Http kötés módosításokat néhány toohello mintakód.

* Frissítse a hello fürt nevét.
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* Másik lehetőségként SessionStartInfo hello alapértelmezett TransportScheme használja, vagy explicit módon állítsa be tooHttp.

```
    info.TransportScheme = TransportScheme.Http;
```

* Hello BrokerClient alapértelmezett kötés használja.
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    Vagy állítsa be explicit módon használja a hello basicHttpBinding.
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* Beállíthatja hello UseAzureQueue jelző tootrue is SessionStartInfo. Ha nincs beállítva, lesz beállítva tootrue alapértelmezés szerint amikor hello fürtnév Azure tartományutótagok és hello TransportScheme Http.
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a>Http-kötés nélkül az Azure storage-várólista használata
toouse Http-kötés egy Azure storage várólista, explicit módon beállítva hello UseAzureQueue jelző toofalse a hello SessionStartInfo nélkül.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>NetTcp kötés használata
toouse NetTcp hello konfigurálása még kötés, hasonló tooconnecting tooan a helyi fürthöz. Tooopen kell néhány hello átjárócsomópont virtuális gép végpontja. Ha hello HPC Pack IaaS telepítési parancsfájl toocreate hello fürt használt, például set hello végpontok hello Azure-portálon az alábbiak szerint.

1. Állítsa le a virtuális gép hello.
2. Adja hozzá a hello TCP-portok 9090, 9087, 9091, a munkamenet, hello 9094 Replikaszervező, munkavégző és adatszolgáltatások, illetve Replikaszervező
   
    ![Végpontok konfigurálása][endpoint-new-portal]
3. Indítsa el a virtuális gép hello.

hello SOA ügyfélalkalmazás nem kell módosítani a módosítása hello központi toohello IaaS fürt teljes név kivételével.

## <a name="next-steps"></a>Következő lépések
* Lásd: [ezeket az erőforrásokat](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) HPC Pack Excel munkaterhelések futtatásával kapcsolatos további információt.
* Lásd: [SOA-szolgáltatások kezelése a Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) központi telepítésére és felügyeletére SOA szolgáltatások HPC Pack bővebben.

<!--Image references-->
[scenario]: ./media/excel-cluster-hpcpack/scenario.png
[github]: ./media/excel-cluster-hpcpack/github.png
[template]: ./media/excel-cluster-hpcpack/template.png
[parameters]: ./media/excel-cluster-hpcpack/parameters.png
[parameters-new-portal]: ./media/excel-cluster-hpcpack/parameters-new-portal.png
[create]: ./media/excel-cluster-hpcpack/create.png
[connect]: ./media/excel-cluster-hpcpack/connect.png
[cert]: ./media/excel-cluster-hpcpack/cert.png
[addin]: ./media/excel-cluster-hpcpack/addin.png
[macro]: ./media/excel-cluster-hpcpack/macro.png
[options]: ./media/excel-cluster-hpcpack/options.png
[run]: ./media/excel-cluster-hpcpack/run.png
[endpoint]: ./media/excel-cluster-hpcpack/endpoint.png
[endpoint-new-portal]: ./media/excel-cluster-hpcpack/endpoint-new-portal.png
[udf]: ./media/excel-cluster-hpcpack/udf.png
