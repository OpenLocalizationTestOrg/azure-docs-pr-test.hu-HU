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
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a><span data-ttu-id="69b42-103">A biztonságos fürt létrehozása az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="69b42-103">Create a secure cluster on Azure using PowerShell</span></span>
<span data-ttu-id="69b42-104">Ez az oktatóanyag bemutatja, hogyan hozzon létre egy Service Fabric-fürt (Windows vagy Linux) fut az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="69b42-104">This tutorial shows you how to create a Service Fabric cluster (Windows or Linux) running in Azure.</span></span> <span data-ttu-id="69b42-105">Amikor végzett, hogy a fürt fut a felhőben, amely központilag telepíthető alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="69b42-105">When you're finished, you have a cluster running in the cloud that you can deploy applications to.</span></span>

<span data-ttu-id="69b42-106">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="69b42-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="69b42-107">A biztonságos Service Fabric-fürt létrehozása az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="69b42-107">Create a secure Service Fabric cluster in Azure using PowerShell</span></span>
> * <span data-ttu-id="69b42-108">A fürt egy X.509 tanúsítvánnyal biztonságos</span><span class="sxs-lookup"><span data-stu-id="69b42-108">Secure the cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="69b42-109">Csatlakozás a fürthöz PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="69b42-109">Connect to the cluster using PowerShell</span></span>
> * <span data-ttu-id="69b42-110">A fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="69b42-110">Remove a cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69b42-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="69b42-111">Prerequisites</span></span>
<span data-ttu-id="69b42-112">Ez az oktatóanyag elkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="69b42-112">Before you begin this tutorial:</span></span>
- <span data-ttu-id="69b42-113">Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="69b42-113">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="69b42-114">Telepítse a [Service Fabric SDK és a PowerShell modul](service-fabric-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="69b42-114">Install the [Service Fabric SDK and PowerShell module](service-fabric-get-started.md)</span></span>
- <span data-ttu-id="69b42-115">Telepítse a [Azure Powershell 4.1-es vagy újabb verziója](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span><span class="sxs-lookup"><span data-stu-id="69b42-115">Install the [Azure Powershell module version 4.1 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span></span>

<span data-ttu-id="69b42-116">Az alábbi eljárás egy egy csomópontos (egyetlen virtuális gép) preview Service Fabric-fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="69b42-116">The following procedure creates a single-node (single virtual machine) preview Service Fabric cluster.</span></span> <span data-ttu-id="69b42-117">A fürt egy önaláírt tanúsítvány, amely lekérdezi és a fürt létrehozása majd helyezett kulcstároló által védett.</span><span class="sxs-lookup"><span data-stu-id="69b42-117">The cluster is secured by a self-signed certificate that gets created along with the cluster and then placed in a key vault.</span></span> <span data-ttu-id="69b42-118">Egy csomópontos fürtök túl egy virtuális gép nem méretezhető és preview fürtök nem frissíthető újabb verzióra való.</span><span class="sxs-lookup"><span data-stu-id="69b42-118">Single-node clusters cannot be scaled beyond one virtual machine and preview clusters cannot be upgraded to newer versions.</span></span>

<span data-ttu-id="69b42-119">Használja az Azure Service Fabric-fürt futtatásával felmerülő költség kiszámításához a [Azure Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/).</span><span class="sxs-lookup"><span data-stu-id="69b42-119">To calculate cost incurred by running a Service Fabric cluster in Azure use the [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/).</span></span>
<span data-ttu-id="69b42-120">Service Fabric-fürtök létrehozásáról további információk: [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="69b42-120">For more information on creating Service Fabric clusters, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="create-the-cluster-using-azure-powershell"></a><span data-ttu-id="69b42-121">Az Azure PowerShell fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="69b42-121">Create the cluster using Azure PowerShell</span></span>
1. <span data-ttu-id="69b42-122">Töltse le az Azure Resource Manager-sablon és a paraméter a fájl helyi másolatát a [Azure Resource Manager sablon a Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="69b42-122">Download a local copy of the Azure Resource Manager template and the parameter file from the [Azure Resource Manager template for Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub repository.</span></span>  <span data-ttu-id="69b42-123">*azuredeploy.JSON* az Azure Resource Manager-sablon, amely meghatározza a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="69b42-123">*azuredeploy.json* is the Azure Resource Manager template that defines a Service Fabric cluster.</span></span> <span data-ttu-id="69b42-124">*azuredeploy.Parameters.JSON* a paraméterek fájlja segítségével testre szabhatja a fürtöt tartalmazó környezetben.</span><span class="sxs-lookup"><span data-stu-id="69b42-124">*azuredeploy.parameters.json* is a parameters file for you to customize the cluster deployment.</span></span>

2. <span data-ttu-id="69b42-125">A következő paraméterek testreszabása a *azuredeploy.parameters.json* paraméterfájl:</span><span class="sxs-lookup"><span data-stu-id="69b42-125">Customize the following parameters in the *azuredeploy.parameters.json* parameters file:</span></span>

   | <span data-ttu-id="69b42-126">Paraméter</span><span class="sxs-lookup"><span data-stu-id="69b42-126">Parameter</span></span>       | <span data-ttu-id="69b42-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="69b42-127">Description</span></span> | <span data-ttu-id="69b42-128">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="69b42-128">Suggested Value</span></span> |
   | --------------- | ----------- | --------------- |
   | <span data-ttu-id="69b42-129">clusterLocation</span><span class="sxs-lookup"><span data-stu-id="69b42-129">clusterLocation</span></span> | <span data-ttu-id="69b42-130">Az Azure-régió, amelyre a fürtön telepíteni szeretné.</span><span class="sxs-lookup"><span data-stu-id="69b42-130">The Azure region to which to deploy the cluster.</span></span> | <span data-ttu-id="69b42-131">*például westeurope, eastasia, eastus*</span><span class="sxs-lookup"><span data-stu-id="69b42-131">*for example, westeurope, eastasia, eastus*</span></span> |
   | <span data-ttu-id="69b42-132">Fürtnév</span><span class="sxs-lookup"><span data-stu-id="69b42-132">clusterName</span></span>     | <span data-ttu-id="69b42-133">A létrehozni kívánt fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="69b42-133">Name of the cluster you want to create.</span></span> | <span data-ttu-id="69b42-134">*például szamitogepnev sfpreviewcluster*</span><span class="sxs-lookup"><span data-stu-id="69b42-134">*for example, bobs-sfpreviewcluster*</span></span> |
   | <span data-ttu-id="69b42-135">adminUserName</span><span class="sxs-lookup"><span data-stu-id="69b42-135">adminUserName</span></span>   | <span data-ttu-id="69b42-136">A helyi rendszergazdai fiók a fürt virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="69b42-136">The local admin account on the cluster virtual machines.</span></span> | <span data-ttu-id="69b42-137">*Bármilyen érvényes Windows Server felhasználóneve*</span><span class="sxs-lookup"><span data-stu-id="69b42-137">*Any valid Windows Server username*</span></span> |
   | <span data-ttu-id="69b42-138">adminPassword</span><span class="sxs-lookup"><span data-stu-id="69b42-138">adminPassword</span></span>   | <span data-ttu-id="69b42-139">A fürt virtuális gépeken a helyi rendszergazda fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="69b42-139">Password of the local admin account on the cluster virtual machines.</span></span> | <span data-ttu-id="69b42-140">*Bármilyen érvényes Windows Server jelszava*</span><span class="sxs-lookup"><span data-stu-id="69b42-140">*Any valid Windows Server password*</span></span> |
   | <span data-ttu-id="69b42-141">clusterCodeVersion</span><span class="sxs-lookup"><span data-stu-id="69b42-141">clusterCodeVersion</span></span> | <span data-ttu-id="69b42-142">A Service Fabric változata fusson.</span><span class="sxs-lookup"><span data-stu-id="69b42-142">The Service Fabric version to run.</span></span> <span data-ttu-id="69b42-143">(255.255.X.255 előzetes verziója).</span><span class="sxs-lookup"><span data-stu-id="69b42-143">(255.255.X.255 are preview versions).</span></span> | <span data-ttu-id="69b42-144">**255.255.5718.255**</span><span class="sxs-lookup"><span data-stu-id="69b42-144">**255.255.5718.255**</span></span> |
   | <span data-ttu-id="69b42-145">vmInstanceCount</span><span class="sxs-lookup"><span data-stu-id="69b42-145">vmInstanceCount</span></span> | <span data-ttu-id="69b42-146">A virtuális gépek a fürt (lehet 1-es vagy 3-99) száma.</span><span class="sxs-lookup"><span data-stu-id="69b42-146">The number of virtual machines in your cluster (can be 1 or 3-99).</span></span> | <span data-ttu-id="69b42-147">**1**</span><span class="sxs-lookup"><span data-stu-id="69b42-147">**1**</span></span> | <span data-ttu-id="69b42-148">*Előzetes fürt adja meg a csak egy virtuális géphez*</span><span class="sxs-lookup"><span data-stu-id="69b42-148">*For a preview cluster specify only one virtual machine*</span></span> |

3. <span data-ttu-id="69b42-149">Nyissa meg a PowerShell-konzolban, a bejelentkezés az Azure-ba, és válassza ki a kívánt előfizetést, a fürt központi telepítése:</span><span class="sxs-lookup"><span data-stu-id="69b42-149">Open a PowerShell console, login to Azure, and select the subscription you want to deploy the cluster in:</span></span>

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. <span data-ttu-id="69b42-150">Hozzon létre, és egy jelszót a tanúsítványhoz a Service Fabric által használandó titkosításához.</span><span class="sxs-lookup"><span data-stu-id="69b42-150">Create and encrypt a password for the certificate to be used by Service Fabric.</span></span>

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. <span data-ttu-id="69b42-151">A fürt és a tanúsítvány létrehozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="69b42-151">Create the cluster and its certificate by running the following command:</span></span>

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
   ><span data-ttu-id="69b42-152">A `-CertificateSubjectName` paraméter kell megfelel-e a paraméterfájlban megadott fürtnév paramétert, valamint a tartomány választott, mint például az Azure-régió kötve: `clustername.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="69b42-152">The `-CertificateSubjectName` parameter should align with the clusterName parameter specified in the parameters file, as well as the domain tied to the Azure region you chose, such as: `clustername.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="69b42-153">A konfiguráció befejezése után azt a fürt létrehozása az Azure-ban kapcsolatos információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="69b42-153">Once the configuration finishes, it outputs information about the cluster created in Azure.</span></span> <span data-ttu-id="69b42-154">Ehhez a paraméterhez megadott elérési úton - CertificateOutputFolder könyvtárához a fürt tanúsítvány is másolja.</span><span class="sxs-lookup"><span data-stu-id="69b42-154">It also copies the cluster certificate to the -CertificateOutputFolder directory on the path you specified for this parameter.</span></span> <span data-ttu-id="69b42-155">A Service Fabric Explorer eléréséhez, és a fürt állapotának megtekintéséhez tanúsítványra van szükség.</span><span class="sxs-lookup"><span data-stu-id="69b42-155">You need this certificate to access Service Fabric Explorer and view the health of your cluster.</span></span>

<span data-ttu-id="69b42-156">Jegyezze fel az URL-cím a fürtöt, a következő URL-cím hasonló lehet: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span><span class="sxs-lookup"><span data-stu-id="69b42-156">Take note of the URL for your cluster, which may be similar to the following URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span></span>

## <a name="modify-the-certificate--access-service-fabric-explorer"></a><span data-ttu-id="69b42-157">A tanúsítvány módosítása & hozzáférni a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="69b42-157">Modify the certificate & access Service Fabric Explorer</span></span> ##

1. <span data-ttu-id="69b42-158">Kattintson duplán a tanúsítványra, a Tanúsítványimportáló varázsló megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="69b42-158">Double-click the certificate to open the Certificate Import Wizard.</span></span>

2. <span data-ttu-id="69b42-159">Az alapértelmezett beállításokkal, de győződjön meg arról, hogy ellenőrizze a **kulcs megjelölése exportálhatóként.**</span><span class="sxs-lookup"><span data-stu-id="69b42-159">Use default settings, but make sure to check the **Mark this key as exportable.**</span></span> <span data-ttu-id="69b42-160">a jelölőnégyzet jelölését, a **kulcsvédelem** lépés.</span><span class="sxs-lookup"><span data-stu-id="69b42-160">check box, in the **private key protection** step.</span></span> <span data-ttu-id="69b42-161">A Visual Studio kell exportálja a tanúsítványt tároló beállításjegyzék Azure Service Fabric-fürt hitelesítési az oktatóanyag későbbi részében konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="69b42-161">Visual Studio needs to export the certificate when configuring Azure Container Registry to Service Fabric Cluster authentication later in this tutorial.</span></span>

3. <span data-ttu-id="69b42-162">Most egy böngészőben nyissa meg Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="69b42-162">You can now open Service Fabric Explorer in a browser.</span></span> <span data-ttu-id="69b42-163">Ehhez navigáljon a **ManagementEndpoint** URL-címet egy webböngészőben megtekintheti a fürt számára, és válassza ki a tanúsítványt a számítógépre mentett.</span><span class="sxs-lookup"><span data-stu-id="69b42-163">To do so, navigate to the **ManagementEndpoint** URL for your cluster using a web browser, and select the certificate that was saved on your machine.</span></span>

>[!NOTE]
><span data-ttu-id="69b42-164">Service Fabric Explorer megnyitásakor egy tanúsítvány hibát látja, egy önaláírt tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="69b42-164">When opening Service Fabric Explorer, you see a certificate error, as you are using a self-signed certificate.</span></span> <span data-ttu-id="69b42-165">Az Edge, meg kell kattintania *részletek* , majd a *nyissa meg a képernyőn látható weblapon* hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="69b42-165">In Edge, you have to click *Details* and then the *Go on to the webpage* link.</span></span> <span data-ttu-id="69b42-166">A Chrome-ban, meg kell kattintania *speciális* , majd a *folytatható* hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="69b42-166">In Chrome, you have to click *Advanced* and then the *proceed* link.</span></span>

>[!NOTE]
><span data-ttu-id="69b42-167">Ha a fürt létrehozása sikertelen, mindig futtathatja a parancsot, amely frissíti a már telepített erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="69b42-167">If the cluster creation fails, you can always rerun the command, which updates the resources already deployed.</span></span> <span data-ttu-id="69b42-168">A tanúsítvány létrejött, a sikertelen központi telepítésének részeként, jön létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="69b42-168">If a certificate was created as part of the failed deployment, a new one is generated.</span></span> <span data-ttu-id="69b42-169">A fürt létrehozása elhárításához lásd: [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="69b42-169">To troubleshoot cluster creation, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="connect-to-the-secure-cluster"></a><span data-ttu-id="69b42-170">Csatlakozás a biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="69b42-170">Connect to the secure cluster</span></span>
<span data-ttu-id="69b42-171">Csatlakozzon a fürthöz, a Service Fabric PowerShell-modullal, telepített Service Fabric SDK-t.</span><span class="sxs-lookup"><span data-stu-id="69b42-171">Connect to the cluster using the Service Fabric PowerShell module installed with the Service Fabric SDK.</span></span>  <span data-ttu-id="69b42-172">Először telepítse a tanúsítványt az aktuális felhasználó a számítógépen (a) személyes tárolóba.</span><span class="sxs-lookup"><span data-stu-id="69b42-172">First, install the certificate into the Personal (My) store of the current user on your computer.</span></span>  <span data-ttu-id="69b42-173">Futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="69b42-173">Run the following PowerShell command:</span></span>

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

<span data-ttu-id="69b42-174">Most már készen áll a biztonságos fürthöz való csatlakozásra.</span><span class="sxs-lookup"><span data-stu-id="69b42-174">You are now ready to connect to your secure cluster.</span></span>

<span data-ttu-id="69b42-175">A **Service Fabric** PowerShell modul sok-parancsmagokat kínál a Service Fabric fürt, alkalmazások és szolgáltatások kezelése.</span><span class="sxs-lookup"><span data-stu-id="69b42-175">The **Service Fabric** PowerShell module provides many cmdlets for managing Service Fabric clusters, applications, and services.</span></span>  <span data-ttu-id="69b42-176">Használja a [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) parancsmag kapcsolódjon a biztonságos fürthöz.</span><span class="sxs-lookup"><span data-stu-id="69b42-176">Use the [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet to connect to the secure cluster.</span></span> <span data-ttu-id="69b42-177">A tanúsítvány ujjlenyomata és kapcsolat végpont adatait az előző lépésben kimenete találhatók.</span><span class="sxs-lookup"><span data-stu-id="69b42-177">The certificate thumbprint and connection endpoint details are found in the output from a previous step.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="69b42-178">Ellenőrizze, hogy kapcsolódik-e, és a fürt állapota kifogástalan használatával a [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="69b42-178">Check that you are connected and the cluster is healthy using the [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.</span></span>

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a><span data-ttu-id="69b42-179">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="69b42-179">Clean up resources</span></span>

<span data-ttu-id="69b42-180">A fürtben a fürt erőforrásán felül egyéb Azure-erőforrások is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="69b42-180">A cluster is made up of other Azure resources in addition to the cluster resource itself.</span></span> <span data-ttu-id="69b42-181">A fürt és az összes általa használt erőforrás törlésének legegyszerűbb módja az erőforráscsoport törlése.</span><span class="sxs-lookup"><span data-stu-id="69b42-181">The simplest way to delete the cluster and all the resources it consumes is to delete the resource group.</span></span>

<span data-ttu-id="69b42-182">Jelentkezzen be az Azure-ba, és válassza ki az előfizetés-azonosító, amelynél el szeretné távolítani a fürt.</span><span class="sxs-lookup"><span data-stu-id="69b42-182">Log in to Azure and select the subscription ID with which you want to remove the cluster.</span></span>  <span data-ttu-id="69b42-183">Miután bejelentkezett az előfizetés-azonosító található a [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="69b42-183">You can find your subscription ID by logging in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="69b42-184">Törölje az erőforráscsoportot és használó fürt erőforrásait a [Remove-AzureRMResourceGroup parancsmag](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="69b42-184">Delete the resource group and all the cluster resources using the [Remove-AzureRMResourceGroup cmdlet](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="next-steps"></a><span data-ttu-id="69b42-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69b42-185">Next steps</span></span>
<span data-ttu-id="69b42-186">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="69b42-186">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="69b42-187">A Service Fabric-fürt létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="69b42-187">Create a Service Fabric cluster in Azure</span></span>
> * <span data-ttu-id="69b42-188">A fürt egy X.509 tanúsítvánnyal biztonságos</span><span class="sxs-lookup"><span data-stu-id="69b42-188">Secure the cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="69b42-189">Csatlakozás a fürthöz PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="69b42-189">Connect to the cluster using PowerShell</span></span>
> * <span data-ttu-id="69b42-190">A fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="69b42-190">Remove a cluster</span></span>

<span data-ttu-id="69b42-191">A következő előzetes telepítése egy meglévő alkalmazást a következő oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="69b42-191">Next, advance to the following tutorial to learn how to deploy an existing application.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="69b42-192">A Docker Compose meglévő .NET alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="69b42-192">Deploy an existing .NET application with Docker Compose</span></span>](service-fabric-host-app-in-a-container.md)
