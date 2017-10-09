---
title: "önálló Azure Service Fabric fürt Windows Server aaaUpgrade |} Microsoft Docs"
description: "Hello Azure Service Fabric-kód és/vagy egy különálló Service Fabric-fürt, beleértve a hello fürt frissítési mód beállítása futtató konfiguráció frissítése."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 5132795e544b6f0185accedbf5092dcaafd66df0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a>Az önálló Azure Service Fabric a Windows Server-fürt frissítése
> [!div class="op_single_selector"]
> * [Azure-fürttel](service-fabric-cluster-upgrade.md)
> * [Önálló fürthöz](service-fabric-cluster-upgrade-windows-server.md)
>
>

A modern rendszerben hello képességét tooupgrade a termék egy kulcs toohello hosszú távú sikert. Az Azure Service Fabric-fürt saját erőforrás. Ez a cikk ismerteti, hogyan biztosíthatja, hogy adott hello fürt mindig fut a Service Fabric-kódot és konfigurációk támogatott verzióit.

## <a name="control-hello-service-fabric-version-that-runs-on-your-cluster"></a>Vezérlő hello Service Fabric verziója, amely a fürt fut
a Service Fabric, amikor a Microsoft által kiadott egy új verzióra, a set hello frissíti a fürt toodownload tooset **fabricClusterAutoupgradeEnabled** konfigurációs tootrue fürt. a Service Fabric támogatott verziója, amelyet a fürt toobe a készlet hello tooselect **fabricClusterAutoupgradeEnabled** konfigurációs toofalse fürt.

> [!NOTE]
> Győződjön meg arról, hogy a fürt mindig a Service Fabric támogatott verzióját futtatja. Ha a Microsoft hello kiadása egy új verzióját a Service Fabric időről, hello korábbi verziója után 60 napon hello hello bejelentés legalább végéhez van megjelölve. Új kiadásokat történik bejelentés [hello Service Fabric csapat blogjában](https://blogs.msdn.microsoft.com/azureservicefabric/). hello új kiadásban elérhető toochoose ezen a ponton.
>
>

A fürt toohello új verzió csak akkor, ha egy éles stílusú csomópont konfigurációt használ, ahol minden Service Fabric-csomópont le egy külön fizikai vagy virtuális gép frissíthető. Ha a fejlesztési fürtöt, ahol egynél több Service Fabric-csomópont nem egyetlen fizikai vagy virtuális gépen, kell újból létrehoznia hello fürt hello új verziójával.

Két, különböző munkafolyamatokat frissítheti a fürt toohello legújabb verziója vagy a Service Fabric támogatott verzióra. Egy munkafolyamat fürtöknél kapcsolat toodownload hello legújabb verziója automatikusan van. hello más munkafolyamat van fürtök, amelyekhez nincs kapcsolat toodownload hello Service Fabric letöltéséhez.

### <a name="upgrade-clusters-that-have-connectivity-toodownload-hello-latest-code-and-configuration"></a>Kapcsolat toodownload hello legújabb kódot és konfigurációs rendelkező fürtök frissítése
Ezeket a lépéseket tooupgrade a fürt támogatott tooa verzióját használja, ha a fürtcsomópontok túl kapcsolódik az internethez[http://download.microsoft.com](http://download.microsoft.com).

Túl kapcsolattal rendelkező fürtök[http://download.microsoft.com](http://download.microsoft.com), a Microsoft rendszeres időközönként új Service Fabric-verziók hello rendelkezésre állását ellenőrzi.

Ha egy új Service Fabric-verzió érhető el, hello csomag letöltése helyileg toohello fürt és a frissítés kiépítve. Emellett tooinform hello ügyfél az új verzió, hello rendszer mutat egy explicit fürt állapotfigyelési figyelmeztetése, amely a következő hasonló toohello:

"hello aktuális fürt verziója [verzió #] támogatása megszűnik [dátum]".

Miután hello fürt hello legújabb verziója fut, hello figyelmeztetés eltűnik.

#### <a name="cluster-upgrade-workflow"></a>A fürt frissítésének munkafolyamata
Miután meggyőződött arról, hogy hello fürt állapotfigyelési figyelmeztetése, hello a következő:

1. Csatlakoztassa a toohello fürtöt, amely rendelkezik rendszergazdai hozzáférési tooall hello gépek hello fürtben csomópontként felsorolt bármely számítógépről. Ezt a parancsfájlt futtató hello számítógép nem rendelkezik toobe hello fürt része.

    ```powershell

    ###### connect toohello secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Első hello frissítheti a Service Fabric-verziók listáját.

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Egy kimeneti hasonló toothis szerezheti be:

    ![háló verziók beolvasása][getfabversions]
3. A fürt frissítési tooan rendelkezésre álló verzió használatával indítsa el a [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell parancssori

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   hello frissítés toomonitor hello előrehaladását, használhat Service Fabric Explorer vagy a következő Windows PowerShell-parancs futtatása hello.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva. egyéni állapotházirendeket toospecify hello **Start-ServiceFabricClusterUpgrade** paranccsal, a dokumentációban [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Hello visszaállítási eredményező hello problémák kijavítása után újra hello frissítés kezdeményezése következő hello korábban leírt lépéseket.

### <a name="upgrade-clusters-that-have-uno-connectivityu-toodownload-hello-latest-code-and-configuration"></a>Frissítés fürtöknél <U>kapcsolat</u> toodownload hello legújabb kód és a konfiguráció
Ezeket a lépéseket tooupgrade a fürt támogatott tooa verzióját használja, ha a fürt csomópontjai nem rendelkeznek internetkapcsolattal túl[http://download.microsoft.com](http://download.microsoft.com).

> [!NOTE]
> Ha a fürt, amely nincs csatlakoztatott toohello Internet futtatja, akkor toomonitor hello Service Fabric csapat blogja toolearn kapcsolatos új verziót. hello rendszer nem jeleníti meg a fürt állapotának figyelmeztetés tooalert meg új verziót.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Automatikus kiépítés és a manuális létesítési
tooenable automatikus letöltés és nyilvántartási hello legújabb verziójához kódot, a Service Fabric-frissítési szolgáltatás beállítása. Tekintse meg a toohello Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt belül hello [önálló csomag](service-fabric-cluster-standalone-package-contents.md) utasításokat.
A manuális eljáráshoz hajtsa végre az alábbi hello utasításokat.

A fürt konfigurációs tooset hello egy konfigurációs frissítés megkezdése előtt a következő tulajdonság toofalse módosítása.

        "fabricClusterAutoupgradeEnabled": false,

Tekintse meg a túl[Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) a használat részleteiről. Győződjön meg arról, hogy tooupdate "clusterConfigurationVersion" a JSON-ban, hello konfigurációs frissítés megkezdése előtt.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

#### <a name="cluster-upgrade-workflow"></a>A fürt frissítésének munkafolyamata

1. Futtassa a Get-ServiceFabricClusterUpgrade egy hello hello fürt csomópontja, és jegyezze fel a hello TargetCodeVersion.
2. Egy internethez csatlakoztatott számítógép toolist a futtatási hello alábbi összes hello aktuális verziójával kompatibilis verzió frissítése, és töltse le a csomag hello tartozó letöltési hivatkozásokat a megfelelő hello.

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Csatlakoztassa a toohello fürtöt, amely rendelkezik rendszergazdai hozzáférési tooall hello gépek hello fürtben csomópontként felsorolt bármely számítógépről. Ezt a parancsfájlt futtató hello számítógép nem rendelkezik toobe hello fürt része

    ```powershell

   ###### Get hello list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file including hello path tooit> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. Másolja a letöltött hello csomagot hello fürt kép tárolóba.

5. Regisztrálja a másolt hello csomag.

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. Indítsa el a fürt frissítési tooan elérhető verzióra.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Kísérheti hello hello frissítés a Service Fabric Explorer vagy hello a következő PowerShell-parancs futtatása.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva. egyéni állapotházirendeket toospecify hello **Start-ServiceFabricClusterUpgrade** paranccsal, a dokumentációban hello [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Hello visszaállítási eredményező hello problémák kijavítása után újra hello frissítés kezdeményezése következő hello korábban leírt lépéseket.


## <a name="upgrade-hello-cluster-configuration"></a>Hello fürt konfigurációjának frissítése
Hello konfigurációs frissítésének elindítása előtt tesztelheti az új fürt konfigurációs json hello önálló csomag hello powershell parancsfájl futtatásával.

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File>

```
vagy

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File> -FabricRuntimePackagePath <Path toohello .cab file which you want tootest hello configuration against>

```

Bizonyos konfigurációk nem lehet frissíteni, például a végpontok, a fürt nevét, a csomópont IP stb. A teszt hello új fürt konfigurációs json hello régi elleni és hibák throw hello Powershell-ablakban, ha bármilyen probléma.

tooupgrade hello konfigurációs Fürtfrissítés, futtassa **Start-ServiceFabricClusterConfigurationUpgrade**. hello konfigurációs frissítés frissítési tartomány által feldolgozott frissítési tartományban.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Fürtök tanúsítvány konfiguráció frissítése  
Fürt tanúsítvány fürtcsomópontok közötti hitelesítéshez használt, így hello tanúsítvány váltása kell végrehajtásra extra körültekintően mert hiba blokkolja hello kommunikációs fürtcsomópontok között.  
Technikai szempontból három beállítások támogatottak:  

1. A tanúsítványnak frissítés: hello frissítési elérési út "(elsődleges) -> tanúsítvány B (elsődleges) tanúsítvány -> tanúsítvány C (elsődleges) ->...".   
2. Kettős tanúsítvány frissítése: hello frissítési elérési út "(elsődleges) -> Tanúsítvány tanúsítvány-(elsődleges) és a B (másodlagos) -> tanúsítvány B (elsődleges) -> tanúsítvány B (elsődleges), és C (másodlagos) -> tanúsítvány C (elsődleges) ->...".
3. Tanúsítvány típusa frissítés: a tanúsítvány ujjlenyomata-alapú konfigurációs <> – Köznapinév-alapú tanúsítvány konfigurálását. Például a tanúsítvány ujjlenyomata (elsődleges) és az ujjlenyomat B (másodlagos) tanúsítvány Köznapinév c ->


## <a name="next-steps"></a>Következő lépések
* Megtudhatja, hogyan toocustomize néhány [Service Fabric-fürt beállítások](service-fabric-cluster-fabric-settings.md).
* Ismerje meg, hogyan túl[a fürt vagy horizontális](service-fabric-cluster-scale-up-down.md).
* További tudnivalók [alkalmazásfrissítések](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
