---
title: "a Service Fabric aaaCreate fürtön, az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Windows vagy Linux Service Fabric fürt az Azure PowerShell használatával."
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
ms.openlocfilehash: e697e2a2e99f67cb02422efdb368c664c9fd5a97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a>A biztonságos fürt létrehozása az Azure PowerShell használatával
Az oktatóanyag bemutatja, hogyan toocreate a Service Fabric fürt (Windows vagy Linux) fut az Azure-ban. Amikor végzett, hogy a fürt, amely központilag telepíthető alkalmazások hello felhőben futó.

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * A biztonságos Service Fabric-fürt létrehozása az Azure PowerShell használatával
> * Egy X.509 tanúsítvánnyal biztonságos hello fürt
> * Csatlakozás toohello fürt PowerShell-lel
> * A fürt eltávolítása

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez:
- Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Telepítse a hello [Service Fabric SDK és a PowerShell modul](service-fabric-get-started.md)
- Telepítse a hello [Azure Powershell 4.1-es vagy újabb verziója](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

a következő eljárás hello hoz létre egy egycsomópontos (egyetlen virtuális gép) preview Service Fabric-fürt. hello fürt védi-e egy önaláírt tanúsítvány, amely lekérdezi hello fürt együtt létrehozott kulcstároló majd helyezve. Egy csomópontos fürtök túl egy virtuális gép nem méretezhető és preview fürtök frissített toonewer verzió nem lehet.

a Service Fabric-fürt futtatja az Azure használatát hello toocalculate költsége [Azure Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/).
Service Fabric-fürtök létrehozásáról további információk: [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md).

## <a name="create-hello-cluster-using-azure-powershell"></a>Az Azure PowerShell hello fürt létrehozása
1. Letöltheti helyi hello Azure Resource Manager sablon és a hello paraméterfájl hello [Azure Resource Manager sablon a Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub-tárházban.  *azuredeploy.JSON* hello Azure Resource Manager sablon, amely meghatározza a Service Fabric-fürt. *azuredeploy.Parameters.JSON* paraméterfájl van az Ön toocustomize hello fürtöt tartalmazó környezetben.

2. A következő paramétereket a hello hello testreszabása *azuredeploy.parameters.json* paraméterfájl:

   | Paraméter       | Leírás | Ajánlott érték |
   | --------------- | ----------- | --------------- |
   | clusterLocation | hello Azure-régiót toowhich toodeploy hello fürt. | *például westeurope, eastasia, eastus* |
   | Fürtnév     | Név hello fürt toocreate szeretné. | *például szamitogepnev sfpreviewcluster* |
   | adminUserName   | hello helyi rendszergazdai fiók hello fürt virtuális gépeken. | *Bármilyen érvényes Windows Server felhasználóneve* |
   | adminPassword   | Hello fürt virtuális gépeken hello helyi rendszergazda fiók jelszavát. | *Bármilyen érvényes Windows Server jelszava* |
   | clusterCodeVersion | a Service Fabric-verzió toorun hello. (255.255.X.255 előzetes verziója). | **255.255.5718.255** |
   | vmInstanceCount | virtuális gépek a fürt (lehet 1-es vagy 3-99) hello száma. | **1** | *Előzetes fürt adja meg a csak egy virtuális géphez* |

3. Nyisson meg egy PowerShell-konzolban, a bejelentkezési tooAzure, és válassza ki a kívánt toodeploy hello fürt hello előfizetést:

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. Hozzon létre, és hozzon létre egy jelszót a Service Fabric által használt hello tanúsítvány toobe titkosításához.

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. Hozzon létre hello fürt és a tanúsítvány hello a következő parancs futtatásával:

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
   >Hello `-CertificateSubjectName` paraméter megadott hello paraméterfájl hello clusterName paramétert kell igazodnak, valamint hello kötött tartomány toohello az Azure-régió szerint választja, például: `clustername.eastus.cloudapp.azure.com`.

Hello konfiguráció befejezése után azt hello fürt létrehozása az Azure-ban információkat jelenít meg. Átmásolja hello fürt tanúsítvány toohello - CertificateOutputFolder könyvtárat ehhez a paraméterhez megadott hello elérési úton. A tanúsítvány tooaccess Service Fabric Explorerben talál, és tekintse meg a fürt hello állapotfigyelő van szüksége.

Jegyezze fel a fürt, amely lehet hasonló toohello hello URL-címe a következő URL-cím: *https://mycluster.westeurope.cloudapp.azure.com:19080*

## <a name="modify-hello-certificate--access-service-fabric-explorer"></a>Hello tanúsítvány módosítása & hozzáférni a Service Fabric Explorerrel ##

1. Kattintson duplán a hello tanúsítvány tooopen hello Tanúsítványimportáló varázsló.

2. Az alapértelmezett beállításokkal, de győződjön meg arról, hogy toocheck hello **kulcs megjelölése exportálhatóként.** jelölőnégyzet, hello **kulcsvédelem** lépés. A Visual Studio tooexport hello tanúsítvány van szüksége, az oktatóanyag későbbi részében Azure tároló beállításjegyzék tooService Fabric-fürt hitelesítés konfigurálásakor.

3. Most egy böngészőben nyissa meg Service Fabric Explorerben talál. toodo Igen, keresse meg a toohello **ManagementEndpoint** a webböngészőt, és a számítógépre mentett válassza hello tanúsítványt használó fürt URL-címe.

>[!NOTE]
>Service Fabric Explorer megnyitásakor egy tanúsítvány hibát látja, egy önaláírt tanúsítványt használ. Az Edge, hogy tooclick *részletek* és majd hello *nyissa meg a weblap toohello* hivatkozásra. A Chrome-ban, hogy tooclick *speciális* és majd hello *folytatható* hivatkozásra.

>[!NOTE]
>Ha hello fürt létrehozása sikertelen, mindig futtathatja hello parancsot, amely frissíti a már telepített hello erőforrásokat. Ha egy tanúsítványt nem sikerült hello központi telepítésének részeként jött létre, egy új jön létre. tootroubleshoot fürt létrehozása, lásd: [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md).

## <a name="connect-toohello-secure-cluster"></a>Csatlakoztassa toohello biztonságos fürtöt
Csatlakoztassa a telepített Service Fabric SDK hello hello Service Fabric PowerShell-modullal toohello fürtöt.  Először telepítse a hello tanúsítvány hello (a) személyes tárolójába hello aktuális felhasználó a számítógépen.  Futtassa a következő PowerShell-paranccsal hello:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Most már áll készen tooconnect tooyour biztonságos fürt.

Hello **Service Fabric** PowerShell modul sok-parancsmagokat kínál a Service Fabric fürt, alkalmazások és szolgáltatások kezelése.  Használjon hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) parancsmag tooconnect toohello biztonságos fürt. tanúsítvány ujjlenyomata hello és kapcsolódási végpont adatait az előző lépésben hello kimeneti találhatók.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Győződjön meg arról, hogy kapcsolódik-e, valamint hello fürt kifogástalan hello használata [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) parancsmag.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

A fürt épül fel más Azure-erőforrások továbbá toohello fürterőforrás magát. hello legegyszerűbb módja toodelete hello fürt összes hello erőforrások pedig toodelete hello erőforráscsoportot.

Jelentkezzen be tooAzure, majd válassza a kívánja tooremove hello fürt hello előfizetés-azonosító.  Az előfizetés-Azonosítóval találhatja meg a bejelentkezés toohello [Azure-portálon](http://portal.azure.com). Hello erőforráscsoport és hello használó összes hello fürterőforrás törlése [Remove-AzureRMResourceGroup parancsmag](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

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
> * Egy X.509 tanúsítvánnyal biztonságos hello fürt
> * Csatlakozás toohello fürt PowerShell-lel
> * A fürt eltávolítása

A következő előzetes módját a következő útmutató toolearn toohello toodeploy egy meglévő alkalmazást.
> [!div class="nextstepaction"]
> [A Docker Compose meglévő .NET alkalmazás központi telepítése](service-fabric-host-app-in-a-container.md)
