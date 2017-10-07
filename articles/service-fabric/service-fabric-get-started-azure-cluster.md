---
title: "az Azure Service Fabric-fürt aaaSet |} Microsoft Docs"
description: "Rövid útmutató – Windows vagy Linux Service Fabric-fürt létrehozása az Azure-ban."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: 13c60e293d19d607bb41ee4859706508c219a833
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Az első saját Service Fabric-fürt létrehozása az Azure-on
A [Service Fabric-fürt](service-fabric-deploy-anywhere.md) virtuális és fizikai gépek hálózaton keresztül csatlakozó készlete, amelyen mikroszolgáltatásokat helyezhet üzembe és felügyelhet. A gyors üzembe helyezés toocreate öt csomópontból álló fürt, hello keresztül futó Windows vagy Linux-segít [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) vagy [Azure-portálon](http://portal.azure.com) csak néhány perc múlva.  

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.


## <a name="use-hello-azure-portal"></a>Hello Azure portál használata

Jelentkezzen be toohello: az Azure portál [http://portal.azure.com](http://portal.azure.com).

### <a name="create-hello-cluster"></a>Hello fürt létrehozása

1. Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.
2. Válassza ki **számítási** a hello **új** panelen, majd válassza ki **Service Fabric-fürt** a hello **számítási** panelen.
3. Töltse ki a Service Fabric hello **alapjai** űrlap. A **operációs rendszer**, jelölje be a Windows vagy Linux azt szeretné, hogy a fürt csomópontjai toorun hello hello verzióját. hello felhasználónevét és jelszavát, az itt megadott használt toolog toohello virtuális gépen. Hozzon létre egy új **Erőforráscsoportot**. Az erőforráscsoport olyan logikai tároló, amelybe a rendszer létrehozza és együttesen kezeli az Azure-erőforrásokat. Amikor végzett, kattintson az **OK** gombra.

    ![A fürtbeállítás kimenete][cluster-setup-basics]

4. Töltse ki a hello **fürtkonfiguráció** űrlap.  A **Csomóponttípusok száma** elemnél adja meg az „1” értéket.

5. Válassza ki **1 (elsődleges) típusú csomópont** és kitöltése a hello **csomópont típuskonfigurációban** űrlap.  A csomóponttípus nevének megadása és hello beállítása [tartóssági szint](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) túl "bronz."  Válassza ki a virtuális gép méretét.

    Csomóponttípusok hello Virtuálisgép-méretet, a virtuális gépek, egyéni végpontokat számát határozza meg, és más beállításait az adott típusú virtuális gépek hello. Az egyes csomóponttípusok definiált egy különálló virtuális gép méretezési csoport, amely használt toodeploy és felügyelt virtuális gépek egyetlen egységként lett beállítva. Mindegyik csomóponttípus egymástól függetlenül skálázható vertikálisan le vagy föl, eltérő nyitott portokkal rendelkezik, és eltérő kapacitásmetrikái lehetnek.  első vagy elsődleges, csomóponttípus hello, ahol a Service Fabric rendszerszolgáltatások és kell rendelkeznie legalább öt virtuális gépeket.

    A [kapacitástervezés](service-fabric-cluster-capacity.md) az éles rendszerek üzembe helyezésének lényeges lépése.  A jelen rövid útmutatóban azonban nem fog alkalmazásokat futtatni, így válassza a *DS1_v2 Standard* VM-méretet.  Válassza ki a "Silver" hello a [megbízhatósági szint](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) és egy kezdeti virtuálisgép-méretezési csoport 5 kapacitását.  

    Egyéni végpontokat, hogy hello fürtben futó alkalmazások kapcsolatba léphet hello Azure terheléselosztó a portok megnyitása.  Adja meg a "80, 8172" tooopen mentése 80-as és 8172-es portot.

    Ne ellenőrizze hello **speciális beállítások konfigurálása** jelenik meg, amelyen a TCP/HTTP felügyeleti végpontok alkalmazás porttartományok, testreszabásához használt [placement Constraints korlátozásokat](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), és [kapacitás Tulajdonságok](service-fabric-cluster-resource-manager-metrics.md).    

    Kattintson az **OK** gombra.

6. A hello **fürtkonfiguráció** alkotnak, állítsa **diagnosztika** túl**a**.  A gyors üzembe helyezés, nem kell tooenter minden [beállítás háló](service-fabric-cluster-fabric-settings.md) tulajdonságait.  A **Fabric-verzió**, jelölje be **automatikus** frissítési mód, hogy a Microsoft automatikusan frissíti a hello fürtön futó hello háló kód hello verzióját.  Hello mód beállítása túl**manuális** Ha túl[válasszon egy támogatott verzióját](service-fabric-cluster-upgrade.md) tooupgrade számára. 

    ![Csomóponttípus konfigurálása][node-type-config]

    Kattintson az **OK** gombra.

7. Töltse ki a hello **biztonsági** űrlap.  Ehhez a rövid útmutatóhoz válassza a **Nem biztonságos** beállítást.  Nagyon fontos ajánlott toocreate a termelési számítási feladatokhoz, biztonságos fürt, bárki névtelenül csatlakoztassa tooan nem biztonságos fürtöt és felügyeleti műveletek végrehajtása óta.  

    A rendszer tanúsítványokat használ a Service Fabric tooprovide hitelesítési és titkosítási toosecure különböző szempontjairól a fürt és az alkalmazásokhoz. A tanúsítványok Service Fabricban való használatával kapcsolatos további információkért lásd a [Service Fabric-fürtök biztonsági forgatókönyveit](service-fabric-cluster-security.md).  Azure Active Directory vagy a szükséges tanúsítványok tooset használatával az alkalmazásbiztonság, tooenable felhasználóhitelesítés [fürt létrehozása a Resource Manager-sablon](service-fabric-cluster-creation-via-arm.md).

    Kattintson az **OK** gombra.

8. Tekintse át a hello összegzése.  Ha azt szeretné, hogy toodownload megadott beépített hello-beállítások a Resource Manager-sablon, jelölje be **töltse le a sablon és a paraméterek**.  Válassza ki **létrehozása** toocreate hello fürt.

    Hello létrehozásának folyamata hello értesítések tekintheti meg. (Kattintson a hello "harang" ikonra hello állapotsorban hello jobb felső sarkában a képernyőn közelében.) Kattintott **PIN-kód tooStartboard** hello fürt létrehozásakor, lásd: **Fabric-fürt üzembe helyezése** toohello rögzítve **Start** tábla.

### <a name="view-cluster-status"></a>Fürt állapotának megtekintése
A fürt létrehozása után vizsgálhatja meg a fürt hello **áttekintése** hello portál panel. Most már megtekintheti a fürt hello irányítópulton, beleértve a nyilvános végpontot hello fürt és a hivatkozás tooService Fabric Explorer hello részleteit.

![Fürt állapota][cluster-status]

### <a name="visualize-hello-cluster-using-service-fabric-explorer"></a>A Service Fabric Explorerrel hello fürt megjelenítése
A [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) hatékony eszköz a fürtök megjelenítéséhez és az alkalmazások kezeléséhez.  Service Fabric Explorerben talál egy olyan szolgáltatás, hello fürtben futó.  Hozzáférési jogosultsága hello a webböngésző használatával **Service Fabric Explorer** hello fürt hivatkozás **áttekintése** hello lapjára.  Hello címet adja meg közvetlenül hello böngészőbe: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)

hello fürt irányítópult áttekintése a fürt, beleértve az alkalmazás és a csomópont állapotának összegzését. hello csomópont nézetben látható hello hello fürt fizikai elrendezését. Az egyes csomópontoknál megtekintheti, hogy melyik alkalmazások kódja üzemel az adott csomóponton.

![Service Fabric Explorer][service-fabric-explorer]

### <a name="connect-toohello-cluster-using-powershell"></a>Csatlakozás toohello fürt PowerShell-lel
Győződjön meg arról, hogy hello fürt fut. csatlakozzon a PowerShell használatával.  hello ServiceFabric PowerShell modul telepítve van a hello [Service Fabric SDK](service-fabric-get-started.md).  Hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag létesít kapcsolatot toohello fürt.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
Lásd: [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md) csatlakozó tooa fürt más példákat. Miután toohello fürthöz kapcsolódó, használja a hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) parancsmag toodisplay hello fürt és a állapot információt az egyes csomópontok csomópontok listáját. A **HealthState** tulajdonságnak *OK* értékűnek kell lennie minden csomópont esetében.

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-hello-cluster"></a>Hello fürt eltávolítása
A Service Fabric-fürt épül fel más Azure-erőforrások továbbá toohello fürterőforrás magát. Így toocompletely a Service Fabric-fürt törlése szükség toodelete összes hello történik, az erőforrásokat. hello legegyszerűbb módja toodelete hello fürt összes hello erőforrások pedig toodelete hello erőforráscsoportot. Más módokon toodelete egy fürtöt vagy toodelete néhány (de nem minden) hello erőforrások az erőforráscsoportban, tekintse meg a [egy fürt törlése](service-fabric-cluster-delete.md)

Az Azure-portálon hello erőforráscsoport törlése:
1. Keresse meg a kívánt toodelete toohello Service Fabric-fürt.
2. Kattintson a hello **erőforráscsoport** nevű hello fürt alapvető erőforrások lapon.
3. A hello **erőforrás csoport Essentials** kattintson **erőforrás csoport törlése** és az adott lapon toocomplete hello törlési hello erőforráscsoport hello útmutatás.
    ![Hello erőforráscsoport törlése][cluster-delete]


## <a name="use-azure-powershell-toodeploy-a-secure-cluster"></a>Azure Powershell toodeploy biztonságos fürt használatára
1. Töltse le a hello [Azure Powershell 4.0-s vagy újabb verziója](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) a számítógépen.

2. Nyisson meg egy Windows PowerShell ablakot, a következő parancs futtatása hello. 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    Meg kell jelennie egy kimeneti hasonló toohello következő.

    ![ps-list][ps-list]

3. Bejelentkezési tooAzure és Select hello előfizetés toowhich toocreate hello fürt

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. Futtassa a következő parancs toonow hello biztonságos fürt létrehozása. Ne feledje toocustomize hello paraméterek. 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also hello name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    Hello parancsot is igénybe vehet 10 perc too30 perc toocomplete, azt a hello végén, egy kimeneti hasonló toohello következő kapja meg. hello kimeneti hello tanúsítvány, ahol töltöttek fel, KeyVault hello adatait és a helyi mappát, amelybe a hello tanúsítvány van másolja hello. 

    ![ps-out][ps-out]

5. Hello teljes kimenetének másolása HTML-, és mentse a szövegfájlt tooa toorefer tooit kell. Jegyezze fel a következő információ hello kimenetből hello. 

    - **CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx
    - **CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10
    - **ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080
    - **ClientConnectionEndpointPort** : 19000

### <a name="install-hello-certificate-on-your-local-machine"></a>A helyi gépen hello tanúsítvány telepítése
  
tooconnect toohello fürt, tanúsítványra van szükség tooinstall hello hello aktuális felhasználó hello (a) személyes tárolóba. 

Futtassa a következő PowerShell hello

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\hello name of hello cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

Most már áll készen tooconnect tooyour biztonságos fürt.

### <a name="connect-tooa-secure-cluster"></a>Csatlakoztassa tooa biztonságos fürtöt 

Futtassa a következő PowerShell parancs tooconnect tooa biztonságos fürt hello. hello Tanúsítványadatok meg kell egyeznie egy tanúsítványt, de a használt tooset be hello fürtöt. 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


a következő példa azt mutatja be hello hello paraméterek befejeződött: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Futtassa a következő parancs toocheck, hogy csatlakozott hello, és hello fürt megfelelő állapotban.

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="publish-your-apps-tooyour-cluster-from-visual-studio"></a>Visual Studio tooyour fürtöt alkalmazások közzététele

Most, hogy állította be az Azure-fürttel, közzéteheti a alkalmazások tooit Visual Studio által következő hello [közzététel tooan fürt](service-fabric-publish-app-remote-cluster.md) dokumentum. 

### <a name="remove-hello-cluster"></a>Hello fürt eltávolítása
A fürt épül fel más Azure-erőforrások továbbá toohello fürterőforrás magát. hello legegyszerűbb módja toodelete hello fürt összes hello erőforrások pedig toodelete hello erőforráscsoportot. 

```powershell

Remove-AzureRmResourceGroup -Name $RGname -Force

```

## <a name="next-steps"></a>Következő lépések
Most, hogy úgy állította be a fejlesztési fürtöt, próbálkozzon a hello következő:
* [Hozzon létre egy biztonságos fürt hello portálon](service-fabric-cluster-creation-via-portal.md)
* [Fürt létrehozása sablonból](service-fabric-cluster-creation-via-arm.md) 
* [Appokat helyezhet üzembe a PowerShell használatával](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
