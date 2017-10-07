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
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a><span data-ttu-id="6de64-103">A biztonságos fürt létrehozása az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="6de64-103">Create a secure cluster on Azure using PowerShell</span></span>
<span data-ttu-id="6de64-104">Az oktatóanyag bemutatja, hogyan toocreate a Service Fabric fürt (Windows vagy Linux) fut az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6de64-104">This tutorial shows you how toocreate a Service Fabric cluster (Windows or Linux) running in Azure.</span></span> <span data-ttu-id="6de64-105">Amikor végzett, hogy a fürt, amely központilag telepíthető alkalmazások hello felhőben futó.</span><span class="sxs-lookup"><span data-stu-id="6de64-105">When you're finished, you have a cluster running in hello cloud that you can deploy applications to.</span></span>

<span data-ttu-id="6de64-106">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="6de64-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6de64-107">A biztonságos Service Fabric-fürt létrehozása az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="6de64-107">Create a secure Service Fabric cluster in Azure using PowerShell</span></span>
> * <span data-ttu-id="6de64-108">Egy X.509 tanúsítvánnyal biztonságos hello fürt</span><span class="sxs-lookup"><span data-stu-id="6de64-108">Secure hello cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="6de64-109">Csatlakozás toohello fürt PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="6de64-109">Connect toohello cluster using PowerShell</span></span>
> * <span data-ttu-id="6de64-110">A fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6de64-110">Remove a cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6de64-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6de64-111">Prerequisites</span></span>
<span data-ttu-id="6de64-112">Ez az oktatóanyag elkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="6de64-112">Before you begin this tutorial:</span></span>
- <span data-ttu-id="6de64-113">Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="6de64-113">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="6de64-114">Telepítse a hello [Service Fabric SDK és a PowerShell modul](service-fabric-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="6de64-114">Install hello [Service Fabric SDK and PowerShell module](service-fabric-get-started.md)</span></span>
- <span data-ttu-id="6de64-115">Telepítse a hello [Azure Powershell 4.1-es vagy újabb verziója](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span><span class="sxs-lookup"><span data-stu-id="6de64-115">Install hello [Azure Powershell module version 4.1 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span></span>

<span data-ttu-id="6de64-116">a következő eljárás hello hoz létre egy egycsomópontos (egyetlen virtuális gép) preview Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="6de64-116">hello following procedure creates a single-node (single virtual machine) preview Service Fabric cluster.</span></span> <span data-ttu-id="6de64-117">hello fürt védi-e egy önaláírt tanúsítvány, amely lekérdezi hello fürt együtt létrehozott kulcstároló majd helyezve.</span><span class="sxs-lookup"><span data-stu-id="6de64-117">hello cluster is secured by a self-signed certificate that gets created along with hello cluster and then placed in a key vault.</span></span> <span data-ttu-id="6de64-118">Egy csomópontos fürtök túl egy virtuális gép nem méretezhető és preview fürtök frissített toonewer verzió nem lehet.</span><span class="sxs-lookup"><span data-stu-id="6de64-118">Single-node clusters cannot be scaled beyond one virtual machine and preview clusters cannot be upgraded toonewer versions.</span></span>

<span data-ttu-id="6de64-119">a Service Fabric-fürt futtatja az Azure használatát hello toocalculate költsége [Azure Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/).</span><span class="sxs-lookup"><span data-stu-id="6de64-119">toocalculate cost incurred by running a Service Fabric cluster in Azure use hello [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/).</span></span>
<span data-ttu-id="6de64-120">Service Fabric-fürtök létrehozásáról további információk: [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="6de64-120">For more information on creating Service Fabric clusters, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="create-hello-cluster-using-azure-powershell"></a><span data-ttu-id="6de64-121">Az Azure PowerShell hello fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="6de64-121">Create hello cluster using Azure PowerShell</span></span>
1. <span data-ttu-id="6de64-122">Letöltheti helyi hello Azure Resource Manager sablon és a hello paraméterfájl hello [Azure Resource Manager sablon a Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="6de64-122">Download a local copy of hello Azure Resource Manager template and hello parameter file from hello [Azure Resource Manager template for Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub repository.</span></span>  <span data-ttu-id="6de64-123">*azuredeploy.JSON* hello Azure Resource Manager sablon, amely meghatározza a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="6de64-123">*azuredeploy.json* is hello Azure Resource Manager template that defines a Service Fabric cluster.</span></span> <span data-ttu-id="6de64-124">*azuredeploy.Parameters.JSON* paraméterfájl van az Ön toocustomize hello fürtöt tartalmazó környezetben.</span><span class="sxs-lookup"><span data-stu-id="6de64-124">*azuredeploy.parameters.json* is a parameters file for you toocustomize hello cluster deployment.</span></span>

2. <span data-ttu-id="6de64-125">A következő paramétereket a hello hello testreszabása *azuredeploy.parameters.json* paraméterfájl:</span><span class="sxs-lookup"><span data-stu-id="6de64-125">Customize hello following parameters in hello *azuredeploy.parameters.json* parameters file:</span></span>

   | <span data-ttu-id="6de64-126">Paraméter</span><span class="sxs-lookup"><span data-stu-id="6de64-126">Parameter</span></span>       | <span data-ttu-id="6de64-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="6de64-127">Description</span></span> | <span data-ttu-id="6de64-128">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="6de64-128">Suggested Value</span></span> |
   | --------------- | ----------- | --------------- |
   | <span data-ttu-id="6de64-129">clusterLocation</span><span class="sxs-lookup"><span data-stu-id="6de64-129">clusterLocation</span></span> | <span data-ttu-id="6de64-130">hello Azure-régiót toowhich toodeploy hello fürt.</span><span class="sxs-lookup"><span data-stu-id="6de64-130">hello Azure region toowhich toodeploy hello cluster.</span></span> | <span data-ttu-id="6de64-131">*például westeurope, eastasia, eastus*</span><span class="sxs-lookup"><span data-stu-id="6de64-131">*for example, westeurope, eastasia, eastus*</span></span> |
   | <span data-ttu-id="6de64-132">Fürtnév</span><span class="sxs-lookup"><span data-stu-id="6de64-132">clusterName</span></span>     | <span data-ttu-id="6de64-133">Név hello fürt toocreate szeretné.</span><span class="sxs-lookup"><span data-stu-id="6de64-133">Name of hello cluster you want toocreate.</span></span> | <span data-ttu-id="6de64-134">*például szamitogepnev sfpreviewcluster*</span><span class="sxs-lookup"><span data-stu-id="6de64-134">*for example, bobs-sfpreviewcluster*</span></span> |
   | <span data-ttu-id="6de64-135">adminUserName</span><span class="sxs-lookup"><span data-stu-id="6de64-135">adminUserName</span></span>   | <span data-ttu-id="6de64-136">hello helyi rendszergazdai fiók hello fürt virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="6de64-136">hello local admin account on hello cluster virtual machines.</span></span> | <span data-ttu-id="6de64-137">*Bármilyen érvényes Windows Server felhasználóneve*</span><span class="sxs-lookup"><span data-stu-id="6de64-137">*Any valid Windows Server username*</span></span> |
   | <span data-ttu-id="6de64-138">adminPassword</span><span class="sxs-lookup"><span data-stu-id="6de64-138">adminPassword</span></span>   | <span data-ttu-id="6de64-139">Hello fürt virtuális gépeken hello helyi rendszergazda fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6de64-139">Password of hello local admin account on hello cluster virtual machines.</span></span> | <span data-ttu-id="6de64-140">*Bármilyen érvényes Windows Server jelszava*</span><span class="sxs-lookup"><span data-stu-id="6de64-140">*Any valid Windows Server password*</span></span> |
   | <span data-ttu-id="6de64-141">clusterCodeVersion</span><span class="sxs-lookup"><span data-stu-id="6de64-141">clusterCodeVersion</span></span> | <span data-ttu-id="6de64-142">a Service Fabric-verzió toorun hello.</span><span class="sxs-lookup"><span data-stu-id="6de64-142">hello Service Fabric version toorun.</span></span> <span data-ttu-id="6de64-143">(255.255.X.255 előzetes verziója).</span><span class="sxs-lookup"><span data-stu-id="6de64-143">(255.255.X.255 are preview versions).</span></span> | <span data-ttu-id="6de64-144">**255.255.5718.255**</span><span class="sxs-lookup"><span data-stu-id="6de64-144">**255.255.5718.255**</span></span> |
   | <span data-ttu-id="6de64-145">vmInstanceCount</span><span class="sxs-lookup"><span data-stu-id="6de64-145">vmInstanceCount</span></span> | <span data-ttu-id="6de64-146">virtuális gépek a fürt (lehet 1-es vagy 3-99) hello száma.</span><span class="sxs-lookup"><span data-stu-id="6de64-146">hello number of virtual machines in your cluster (can be 1 or 3-99).</span></span> | <span data-ttu-id="6de64-147">**1**</span><span class="sxs-lookup"><span data-stu-id="6de64-147">**1**</span></span> | <span data-ttu-id="6de64-148">*Előzetes fürt adja meg a csak egy virtuális géphez*</span><span class="sxs-lookup"><span data-stu-id="6de64-148">*For a preview cluster specify only one virtual machine*</span></span> |

3. <span data-ttu-id="6de64-149">Nyisson meg egy PowerShell-konzolban, a bejelentkezési tooAzure, és válassza ki a kívánt toodeploy hello fürt hello előfizetést:</span><span class="sxs-lookup"><span data-stu-id="6de64-149">Open a PowerShell console, login tooAzure, and select hello subscription you want toodeploy hello cluster in:</span></span>

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. <span data-ttu-id="6de64-150">Hozzon létre, és hozzon létre egy jelszót a Service Fabric által használt hello tanúsítvány toobe titkosításához.</span><span class="sxs-lookup"><span data-stu-id="6de64-150">Create and encrypt a password for hello certificate toobe used by Service Fabric.</span></span>

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. <span data-ttu-id="6de64-151">Hozzon létre hello fürt és a tanúsítvány hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6de64-151">Create hello cluster and its certificate by running hello following command:</span></span>

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
   ><span data-ttu-id="6de64-152">Hello `-CertificateSubjectName` paraméter megadott hello paraméterfájl hello clusterName paramétert kell igazodnak, valamint hello kötött tartomány toohello az Azure-régió szerint választja, például: `clustername.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="6de64-152">hello `-CertificateSubjectName` parameter should align with hello clusterName parameter specified in hello parameters file, as well as hello domain tied toohello Azure region you chose, such as: `clustername.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="6de64-153">Hello konfiguráció befejezése után azt hello fürt létrehozása az Azure-ban információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="6de64-153">Once hello configuration finishes, it outputs information about hello cluster created in Azure.</span></span> <span data-ttu-id="6de64-154">Átmásolja hello fürt tanúsítvány toohello - CertificateOutputFolder könyvtárat ehhez a paraméterhez megadott hello elérési úton.</span><span class="sxs-lookup"><span data-stu-id="6de64-154">It also copies hello cluster certificate toohello -CertificateOutputFolder directory on hello path you specified for this parameter.</span></span> <span data-ttu-id="6de64-155">A tanúsítvány tooaccess Service Fabric Explorerben talál, és tekintse meg a fürt hello állapotfigyelő van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6de64-155">You need this certificate tooaccess Service Fabric Explorer and view hello health of your cluster.</span></span>

<span data-ttu-id="6de64-156">Jegyezze fel a fürt, amely lehet hasonló toohello hello URL-címe a következő URL-cím: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span><span class="sxs-lookup"><span data-stu-id="6de64-156">Take note of hello URL for your cluster, which may be similar toohello following URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span></span>

## <a name="modify-hello-certificate--access-service-fabric-explorer"></a><span data-ttu-id="6de64-157">Hello tanúsítvány módosítása & hozzáférni a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="6de64-157">Modify hello certificate & access Service Fabric Explorer</span></span> ##

1. <span data-ttu-id="6de64-158">Kattintson duplán a hello tanúsítvány tooopen hello Tanúsítványimportáló varázsló.</span><span class="sxs-lookup"><span data-stu-id="6de64-158">Double-click hello certificate tooopen hello Certificate Import Wizard.</span></span>

2. <span data-ttu-id="6de64-159">Az alapértelmezett beállításokkal, de győződjön meg arról, hogy toocheck hello **kulcs megjelölése exportálhatóként.**</span><span class="sxs-lookup"><span data-stu-id="6de64-159">Use default settings, but make sure toocheck hello **Mark this key as exportable.**</span></span> <span data-ttu-id="6de64-160">jelölőnégyzet, hello **kulcsvédelem** lépés.</span><span class="sxs-lookup"><span data-stu-id="6de64-160">check box, in hello **private key protection** step.</span></span> <span data-ttu-id="6de64-161">A Visual Studio tooexport hello tanúsítvány van szüksége, az oktatóanyag későbbi részében Azure tároló beállításjegyzék tooService Fabric-fürt hitelesítés konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="6de64-161">Visual Studio needs tooexport hello certificate when configuring Azure Container Registry tooService Fabric Cluster authentication later in this tutorial.</span></span>

3. <span data-ttu-id="6de64-162">Most egy böngészőben nyissa meg Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="6de64-162">You can now open Service Fabric Explorer in a browser.</span></span> <span data-ttu-id="6de64-163">toodo Igen, keresse meg a toohello **ManagementEndpoint** a webböngészőt, és a számítógépre mentett válassza hello tanúsítványt használó fürt URL-címe.</span><span class="sxs-lookup"><span data-stu-id="6de64-163">toodo so, navigate toohello **ManagementEndpoint** URL for your cluster using a web browser, and select hello certificate that was saved on your machine.</span></span>

>[!NOTE]
><span data-ttu-id="6de64-164">Service Fabric Explorer megnyitásakor egy tanúsítvány hibát látja, egy önaláírt tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="6de64-164">When opening Service Fabric Explorer, you see a certificate error, as you are using a self-signed certificate.</span></span> <span data-ttu-id="6de64-165">Az Edge, hogy tooclick *részletek* és majd hello *nyissa meg a weblap toohello* hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6de64-165">In Edge, you have tooclick *Details* and then hello *Go on toohello webpage* link.</span></span> <span data-ttu-id="6de64-166">A Chrome-ban, hogy tooclick *speciális* és majd hello *folytatható* hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6de64-166">In Chrome, you have tooclick *Advanced* and then hello *proceed* link.</span></span>

>[!NOTE]
><span data-ttu-id="6de64-167">Ha hello fürt létrehozása sikertelen, mindig futtathatja hello parancsot, amely frissíti a már telepített hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6de64-167">If hello cluster creation fails, you can always rerun hello command, which updates hello resources already deployed.</span></span> <span data-ttu-id="6de64-168">Ha egy tanúsítványt nem sikerült hello központi telepítésének részeként jött létre, egy új jön létre.</span><span class="sxs-lookup"><span data-stu-id="6de64-168">If a certificate was created as part of hello failed deployment, a new one is generated.</span></span> <span data-ttu-id="6de64-169">tootroubleshoot fürt létrehozása, lásd: [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="6de64-169">tootroubleshoot cluster creation, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="connect-toohello-secure-cluster"></a><span data-ttu-id="6de64-170">Csatlakoztassa toohello biztonságos fürtöt</span><span class="sxs-lookup"><span data-stu-id="6de64-170">Connect toohello secure cluster</span></span>
<span data-ttu-id="6de64-171">Csatlakoztassa a telepített Service Fabric SDK hello hello Service Fabric PowerShell-modullal toohello fürtöt.</span><span class="sxs-lookup"><span data-stu-id="6de64-171">Connect toohello cluster using hello Service Fabric PowerShell module installed with hello Service Fabric SDK.</span></span>  <span data-ttu-id="6de64-172">Először telepítse a hello tanúsítvány hello (a) személyes tárolójába hello aktuális felhasználó a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6de64-172">First, install hello certificate into hello Personal (My) store of hello current user on your computer.</span></span>  <span data-ttu-id="6de64-173">Futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="6de64-173">Run hello following PowerShell command:</span></span>

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

<span data-ttu-id="6de64-174">Most már áll készen tooconnect tooyour biztonságos fürt.</span><span class="sxs-lookup"><span data-stu-id="6de64-174">You are now ready tooconnect tooyour secure cluster.</span></span>

<span data-ttu-id="6de64-175">Hello **Service Fabric** PowerShell modul sok-parancsmagokat kínál a Service Fabric fürt, alkalmazások és szolgáltatások kezelése.</span><span class="sxs-lookup"><span data-stu-id="6de64-175">hello **Service Fabric** PowerShell module provides many cmdlets for managing Service Fabric clusters, applications, and services.</span></span>  <span data-ttu-id="6de64-176">Használjon hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) parancsmag tooconnect toohello biztonságos fürt.</span><span class="sxs-lookup"><span data-stu-id="6de64-176">Use hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet tooconnect toohello secure cluster.</span></span> <span data-ttu-id="6de64-177">tanúsítvány ujjlenyomata hello és kapcsolódási végpont adatait az előző lépésben hello kimeneti találhatók.</span><span class="sxs-lookup"><span data-stu-id="6de64-177">hello certificate thumbprint and connection endpoint details are found in hello output from a previous step.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="6de64-178">Győződjön meg arról, hogy kapcsolódik-e, valamint hello fürt kifogástalan hello használata [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="6de64-178">Check that you are connected and hello cluster is healthy using hello [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.</span></span>

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a><span data-ttu-id="6de64-179">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6de64-179">Clean up resources</span></span>

<span data-ttu-id="6de64-180">A fürt épül fel más Azure-erőforrások továbbá toohello fürterőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="6de64-180">A cluster is made up of other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="6de64-181">hello legegyszerűbb módja toodelete hello fürt összes hello erőforrások pedig toodelete hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="6de64-181">hello simplest way toodelete hello cluster and all hello resources it consumes is toodelete hello resource group.</span></span>

<span data-ttu-id="6de64-182">Jelentkezzen be tooAzure, majd válassza a kívánja tooremove hello fürt hello előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="6de64-182">Log in tooAzure and select hello subscription ID with which you want tooremove hello cluster.</span></span>  <span data-ttu-id="6de64-183">Az előfizetés-Azonosítóval találhatja meg a bejelentkezés toohello [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6de64-183">You can find your subscription ID by logging in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="6de64-184">Hello erőforráscsoport és hello használó összes hello fürterőforrás törlése [Remove-AzureRMResourceGroup parancsmag](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="6de64-184">Delete hello resource group and all hello cluster resources using hello [Remove-AzureRMResourceGroup cmdlet](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="next-steps"></a><span data-ttu-id="6de64-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6de64-185">Next steps</span></span>
<span data-ttu-id="6de64-186">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="6de64-186">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6de64-187">A Service Fabric-fürt létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6de64-187">Create a Service Fabric cluster in Azure</span></span>
> * <span data-ttu-id="6de64-188">Egy X.509 tanúsítvánnyal biztonságos hello fürt</span><span class="sxs-lookup"><span data-stu-id="6de64-188">Secure hello cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="6de64-189">Csatlakozás toohello fürt PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="6de64-189">Connect toohello cluster using PowerShell</span></span>
> * <span data-ttu-id="6de64-190">A fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6de64-190">Remove a cluster</span></span>

<span data-ttu-id="6de64-191">A következő előzetes módját a következő útmutató toolearn toohello toodeploy egy meglévő alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6de64-191">Next, advance toohello following tutorial toolearn how toodeploy an existing application.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6de64-192">A Docker Compose meglévő .NET alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6de64-192">Deploy an existing .NET application with Docker Compose</span></span>](service-fabric-host-app-in-a-container.md)
