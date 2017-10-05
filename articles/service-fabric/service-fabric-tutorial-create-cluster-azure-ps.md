---
title: "A Service Fabric-fürt létrehozása az Azure-ban |} Microsoft Docs"
description: "Útmutató a Windows vagy Linux Service Fabric-fürt létrehozása az Azure PowerShell használatával."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi
ms.openlocfilehash: f62249417552b9cf840bfac3b94c27f63bd7064e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a>A biztonságos fürt létrehozása az Azure PowerShell használatával
Ez az oktatóanyag bemutatja, hogyan hozzon létre egy Service Fabric-fürt (Windows vagy Linux) fut az Azure-ban. Amikor végzett, hogy a fürt fut a felhőben, amely központilag telepíthető alkalmazások.

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * A biztonságos Service Fabric-fürt létrehozása az Azure PowerShell használatával
> * A fürt egy X.509 tanúsítvánnyal biztonságos
> * Csatlakozás a fürthöz PowerShell használatával
> * A fürt eltávolítása

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez:
- Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Telepítse a [Service Fabric SDK és a PowerShell modul](service-fabric-get-started.md)
- Telepítse a [Azure Powershell 4.1-es vagy újabb verziója](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

Az alábbi eljárás egy egy csomópontos (egyetlen virtuális gép) preview Service Fabric-fürtöt hoz létre. A fürt egy önaláírt tanúsítvány, amely lekérdezi és a fürt létrehozása majd helyezett kulcstároló által védett. Egy csomópontos fürtök túl egy virtuális gép nem méretezhető és preview fürtök nem frissíthető újabb verzióra való.

Használja az Azure Service Fabric-fürt futtatásával felmerülő költség kiszámításához a [Azure Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/).
Service Fabric-fürtök létrehozásáról további információk: [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md).

## <a name="create-the-cluster-using-azure-powershell"></a>Az Azure PowerShell fürt létrehozása
1. Töltse le az Azure Resource Manager-sablon és a paraméter a fájl helyi másolatát a [Azure Resource Manager sablon a Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub-tárházban.  *azuredeploy.JSON* az Azure Resource Manager-sablon, amely meghatározza a Service Fabric-fürt. *azuredeploy.Parameters.JSON* a paraméterek fájlja segítségével testre szabhatja a fürtöt tartalmazó környezetben.

2. A következő paraméterek testreszabása a *azuredeploy.parameters.json* paraméterfájl:

   | Paraméter       | Leírás | Ajánlott érték |
   | --------------- | ----------- | --------------- |
   | clusterLocation | Az Azure-régió, amelyre a fürtön telepíteni szeretné. | *például westeurope, eastasia, eastus* |
   | Fürtnév     | A létrehozni kívánt fürt nevét. | *például szamitogepnev sfpreviewcluster* |
   | adminUserName   | A helyi rendszergazdai fiók a fürt virtuális gépeken. | *Bármilyen érvényes Windows Server felhasználóneve* |
   | adminPassword   | A fürt virtuális gépeken a helyi rendszergazda fiók jelszavát. | *Bármilyen érvényes Windows Server jelszava* |
   | clusterCodeVersion | A Service Fabric változata fusson. (255.255.X.255 előzetes verziója). | **255.255.5718.255** |
   | vmInstanceCount | A virtuális gépek a fürt (lehet 1-es vagy 3-99) száma. | **1** | *Előzetes fürt adja meg a csak egy virtuális géphez* |

3. Nyissa meg a PowerShell-konzolban, a bejelentkezés az Azure-ba, és válassza ki a kívánt előfizetést, a fürt központi telepítése:

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. Hozzon létre, és egy jelszót a tanúsítványhoz a Service Fabric által használandó titkosításához.

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. A fürt és a tanúsítvány létrehozása a következő parancs futtatásával:

   ```powershell
      New-AzureRmServiceFabricCluster
          -TemplateFile C:\Users\me\Desktop\azuredeploy.json `
          -ParameterFile C:\Users\me\Desktop\azuredeploy.parameters.json `
          -CertificateOutputFolder C:\Users\me\Desktop\ `
          -CertificatePassword $pwd `
          -CertificateSubjectName "mycluster.westeurope.cloudapp.azure.com" `
          -ResourceGroupName myclusterRG
   ```

   >[!NOTE]
   >A `-CertificateSubjectName` paraméter kell megfelel-e a paraméterfájlban megadott fürtnév paramétert, valamint a tartomány választott, mint például az Azure-régió kötve: `clustername.eastus.cloudapp.azure.com`.

A konfiguráció befejezése után azt a fürt létrehozása az Azure-ban kapcsolatos információkat jelenít meg. Ehhez a paraméterhez megadott elérési úton - CertificateOutputFolder könyvtárához a fürt tanúsítvány is másolja. A Service Fabric Explorer eléréséhez, és a fürt állapotának megtekintéséhez tanúsítványra van szükség.

Jegyezze fel az URL-cím a fürtöt, a következő URL-cím hasonló lehet: *https://mycluster.westeurope.cloudapp.azure.com:19080*

## <a name="modify-the-certificate--access-service-fabric-explorer"></a>A tanúsítvány módosítása & hozzáférni a Service Fabric Explorerrel ##

1. Kattintson duplán a tanúsítványra, a Tanúsítványimportáló varázsló megnyitásához.

2. Az alapértelmezett beállításokkal, de győződjön meg arról, hogy ellenőrizze a **kulcs megjelölése exportálhatóként.** a jelölőnégyzet jelölését, a **kulcsvédelem** lépés. A Visual Studio kell exportálja a tanúsítványt tároló beállításjegyzék Azure Service Fabric-fürt hitelesítési az oktatóanyag későbbi részében konfigurálásakor.

3. Most egy böngészőben nyissa meg Service Fabric Explorerben talál. Ehhez navigáljon a **ManagementEndpoint** URL-címet egy webböngészőben megtekintheti a fürt számára, és válassza ki a tanúsítványt a számítógépre mentett.

>[!NOTE]
>Service Fabric Explorer megnyitásakor egy tanúsítvány hibát látja, egy önaláírt tanúsítványt használ. Az Edge, meg kell kattintania *részletek* , majd a *nyissa meg a képernyőn látható weblapon* hivatkozásra. A Chrome-ban, meg kell kattintania *speciális* , majd a *folytatható* hivatkozásra.

>[!NOTE]
>Ha a fürt létrehozása sikertelen, mindig futtathatja a parancsot, amely frissíti a már telepített erőforrásokhoz. A tanúsítvány létrejött, a sikertelen központi telepítésének részeként, jön létre egy újat. A fürt létrehozása elhárításához lásd: [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md).

## <a name="connect-to-the-secure-cluster"></a>Csatlakozás a biztonságos fürthöz
Csatlakozzon a fürthöz, a Service Fabric PowerShell-modullal, telepített Service Fabric SDK-t.  Először telepítse a tanúsítványt az aktuális felhasználó a számítógépen (a) személyes tárolóba.  Futtassa a következő PowerShell-parancsot:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Most már készen áll a biztonságos fürthöz való csatlakozásra.

A **Service Fabric** PowerShell modul sok-parancsmagokat kínál a Service Fabric fürt, alkalmazások és szolgáltatások kezelése.  Használja a [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) parancsmag kapcsolódjon a biztonságos fürthöz. A tanúsítvány ujjlenyomata és kapcsolat végpont adatait az előző lépésben kimenete találhatók.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Ellenőrizze, hogy kapcsolódik-e, és a fürt állapota kifogástalan használatával a [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) parancsmag.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

A fürtben a fürt erőforrásán felül egyéb Azure-erőforrások is megtalálhatók. A fürt és az összes általa használt erőforrás törlésének legegyszerűbb módja az erőforráscsoport törlése.

Jelentkezzen be az Azure-ba, és válassza ki az előfizetés-azonosító, amelynél el szeretné távolítani a fürt.  Miután bejelentkezett az előfizetés-azonosító található a [Azure-portálon](http://portal.azure.com). Törölje az erőforráscsoportot és használó fürt erőforrásait a [Remove-AzureRMResourceGroup parancsmag](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * A Service Fabric-fürt létrehozása az Azure-ban
> * A fürt egy X.509 tanúsítvánnyal biztonságos
> * Csatlakozás a fürthöz PowerShell használatával
> * A fürt eltávolítása

A következő előzetes telepítése egy meglévő alkalmazást a következő oktatóanyagot.
> [!div class="nextstepaction"]
> [A Docker Compose meglévő .NET alkalmazás központi telepítése](service-fabric-host-app-in-a-container.md)
