---
title: "Azure Service Fabric-fürtben lévő tanúsítványok kezelése |} Microsoft Docs"
description: "Hozzáadhat új tanúsítványokat, a helyettesítő tanúsítvány, és távolítsa el a tanúsítványt, vagy a Service Fabric-fürt ismerteti."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: chackdan
ms.openlocfilehash: c433e8683755e454f9561f094269c3daccf78a62
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a><span data-ttu-id="99b30-103">Hozzáadhat és eltávolíthat tanúsítványokat a Service Fabric-fürtök az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="99b30-103">Add or remove certificates for a Service Fabric cluster in Azure</span></span>
<span data-ttu-id="99b30-104">Javasoljuk, hogy megismerje a módját a Service Fabric X.509-tanúsítványokat használ, és ismernie kell a [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="99b30-104">It is recommended that you familiarize yourself with how Service Fabric uses X.509 certificates and be familiar with the [Cluster security scenarios](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="99b30-105">Ismernie kell fürt tanúsítvány és felhasználási területének, mielőtt végrehajtásának folytatásához.</span><span class="sxs-lookup"><span data-stu-id="99b30-105">You must understand what a cluster certificate is and what is used for, before you proceed further.</span></span>

<span data-ttu-id="99b30-106">A Service fabric lehetővé teszi az ügyfél-tanúsítványok mellett a fürt létrehozása során tanúsítvány biztonsági konfigurálásakor két fürt tanúsítvány, egy elsődleges és másodlagos, adja meg.</span><span class="sxs-lookup"><span data-stu-id="99b30-106">Service fabric lets you specify two cluster certificates, a primary and a secondary, when you configure certificate security during cluster creation, in addition to client certificates.</span></span> <span data-ttu-id="99b30-107">Tekintse meg [létrehozása egy azure-portálon fürttel](service-fabric-cluster-creation-via-portal.md) vagy [létrehozása az azure-fürttel Azure Resource Manageren keresztül](service-fabric-cluster-creation-via-arm.md) azok beállításával kapcsolatos részleteket létre idő.</span><span class="sxs-lookup"><span data-stu-id="99b30-107">Refer to [creating an azure cluster via portal](service-fabric-cluster-creation-via-portal.md) or [creating an azure cluster via Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) for details on setting them up at create time.</span></span> <span data-ttu-id="99b30-108">Ha csak egy fürt tanúsítványt ad meg, létrehozás ideje, majd használt elsődleges tanúsítványként.</span><span class="sxs-lookup"><span data-stu-id="99b30-108">If you specify only one cluster certificate at create time, then that is used as the primary certificate.</span></span> <span data-ttu-id="99b30-109">Fürt létrehozása után hozzáadhat egy új tanúsítványt, a egy másodlagos.</span><span class="sxs-lookup"><span data-stu-id="99b30-109">After cluster creation, you can add a new certificate as a secondary.</span></span>

> [!NOTE]
> <span data-ttu-id="99b30-110">Biztonságos fürt esetén mindig szüksége lesz legalább egy érvényes (a nem visszavont és a nem lejárt) fürt telepített tanúsítvány (elsődleges vagy másodlagos) (Ha nem, a fürt leáll a működése).</span><span class="sxs-lookup"><span data-stu-id="99b30-110">For a secure cluster, you will always need at least one valid (not revoked and not expired) cluster certificate (primary or secondary) deployed (if not, the cluster stops functioning).</span></span> <span data-ttu-id="99b30-111">90 napig minden érvényes tanúsítvány elérni lejárati, a rendszer létrehoz egy figyelmeztetés nyomkövetést, és egy figyelmeztetés állapotesemény a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="99b30-111">90 days before all valid certificates reach expiration, the system generates a warning trace and also a warning health event on the node.</span></span> <span data-ttu-id="99b30-112">Jelenleg nincs e-mail vagy más, ebben a témakörben a küld a service fabric értesítést.</span><span class="sxs-lookup"><span data-stu-id="99b30-112">There is currently no email or any other notification that service fabric sends out on this topic.</span></span> 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-the-portal"></a><span data-ttu-id="99b30-113">A portál használatával a fürt másodlagos tanúsítvány hozzáadásához</span><span class="sxs-lookup"><span data-stu-id="99b30-113">Add a secondary cluster certificate using the portal</span></span>

<span data-ttu-id="99b30-114">Másodlagos fürt tanúsítvány nem adható hozzá, az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="99b30-114">Secondary cluster certificate cannot be added through the Azure portal.</span></span> <span data-ttu-id="99b30-115">Az Azure powershell használata, amely rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="99b30-115">You have to use Azure powershell for that.</span></span> <span data-ttu-id="99b30-116">A folyamat a dokumentum későbbi szakaszában meghatározott.</span><span class="sxs-lookup"><span data-stu-id="99b30-116">The process is outlined later in this document.</span></span>

## <a name="swap-the-cluster-certificates-using-the-portal"></a><span data-ttu-id="99b30-117">A felcserélendő a fürt tanúsítványokat, a portál használatával</span><span class="sxs-lookup"><span data-stu-id="99b30-117">Swap the cluster certificates using the portal</span></span>

<span data-ttu-id="99b30-118">Miután egy másodlagos fürt tanúsítványt, ha szeretne váltani, az elsődleges és másodlagos sikeresen telepített, a biztonsági panelen keresse meg, és felcserélni a másodlagos cert az elsődleges tanúsítvány a helyi menüből válassza a "Elsődleges felcserélés" lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="99b30-118">After you have successfully deployed a secondary cluster certificate, if you want to swap the primary and secondary, then navigate to the Security blade, and select the 'Swap with primary' option from the context menu to swap the secondary cert with the primary cert.</span></span>

![Lapozófájl-kapacitás tanúsítvány][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-the-portal"></a><span data-ttu-id="99b30-120">Távolítsa el a fürt tanúsítványt, a portál használatával</span><span class="sxs-lookup"><span data-stu-id="99b30-120">Remove a cluster certificate using the portal</span></span>

<span data-ttu-id="99b30-121">Biztonságos fürt esetén mindig szüksége lesz legalább egy érvényes (a nem visszavont és a nem lejárt) (elsődleges vagy másodlagos) telepített tanúsítvány ellenkező esetben a fürt megszűnik működni.</span><span class="sxs-lookup"><span data-stu-id="99b30-121">For a secure cluster, you will always need at least one valid (not revoked and not expired) certificate (primary or secondary) deployed if not, the cluster stops functioning.</span></span>

<span data-ttu-id="99b30-122">Távolítsa el a másodlagos tanúsítvány használatát a fürt biztonsági okokból és a biztonsági panel lép, és a másodlagos tanúsítvány a helyi menüből válassza a "Törlés" lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="99b30-122">To remove a secondary certificate from being used for cluster security, Navigate to the Security blade and select the 'Delete' option from the context menu on the secondary certificate.</span></span>

<span data-ttu-id="99b30-123">Ha a szándéka az, eltávolítani a tanúsítványt, amely elsődleges van megjelölve, majd szüksége lesz a másodlagos felcserélése, és törölje a másodlagos, a frissítés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="99b30-123">If your intent is to remove the certificate that is marked primary, then you will need to swap it with the secondary first, and then delete the secondary after the upgrade has completed.</span></span>

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a><span data-ttu-id="99b30-124">Erőforrás-kezelő Powershell-lel másodlagos tanúsítvány hozzáadásához</span><span class="sxs-lookup"><span data-stu-id="99b30-124">Add a secondary certificate using Resource Manager Powershell</span></span>

<span data-ttu-id="99b30-125">Ezek a lépések feltételezik, hogy Ön ismeri a Resource Manager működésével és legalább egy Service Fabric-fürt Resource Manager-sablon használatával telepített, és a sablont, amely akkor lesz szüksége a fürt létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="99b30-125">These steps assume that you are familiar with how Resource Manager works and have deployed atleast one Service Fabric cluster using a Resource Manager template, and have the template that you used to set up the cluster handy.</span></span> <span data-ttu-id="99b30-126">Az Ön számára elfogadható JSON használatával is feltételezzük.</span><span class="sxs-lookup"><span data-stu-id="99b30-126">It is also assumed that you are comfortable using JSON.</span></span>

> [!NOTE]
> <span data-ttu-id="99b30-127">Ha a keresett mintasablon és mentén vagy kiindulási pontként használható paramétereket, majd töltse le azt a ez [git-tárház](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="99b30-127">If you are looking for a sample template and parameters that you can use to follow along or as a starting point, then download it from this [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span> 
> 
> 

### <a name="edit-your-resource-manager-template"></a><span data-ttu-id="99b30-128">A Resource Manager sablon szerkesztése</span><span class="sxs-lookup"><span data-stu-id="99b30-128">Edit your Resource Manager template</span></span>

<span data-ttu-id="99b30-129">A következő mentén megkönnyítése érdekében a minta 5-VM-1-NodeType tulajdonságok értéke-Secure_Step2.JSON frissítjük összes szerkesztést tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="99b30-129">For ease of following along, sample 5-VM-1-NodeTypes-Secure_Step2.JSON contains all the edits we will be making.</span></span> <span data-ttu-id="99b30-130">a minta érhető el: [git-tárház](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="99b30-130">the sample is available at [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span>

<span data-ttu-id="99b30-131">**Ügyeljen arra, hogy kövesse a lépéseket**</span><span class="sxs-lookup"><span data-stu-id="99b30-131">**Make sure to follow all the steps**</span></span>

<span data-ttu-id="99b30-132">**1. lépés:** nyissa meg a Resource Manager sablon, amelyet a fürt üzembe.</span><span class="sxs-lookup"><span data-stu-id="99b30-132">**Step 1:** Open up the Resource Manager template you used to deploy you Cluster.</span></span> <span data-ttu-id="99b30-133">(Ha letöltötte a minta a fenti tárházból, 5-VM-1-NodeType tulajdonságok értéke-Secure_Step1.JSON segítségével biztonságos fürt központi telepítése, és nyissa meg, hogy a sablon mentése).</span><span class="sxs-lookup"><span data-stu-id="99b30-133">(If you have downloaded the sample from the above repo, then Use 5-VM-1-NodeTypes-Secure_Step1.JSON to deploy a secure cluster and then open up that template).</span></span>

<span data-ttu-id="99b30-134">**2. lépés:** Hozzáadás **két új paraméterrel** "secCertificateThumbprint" és "secCertificateUrlValue", írja be a "karakterlánc" a sablon paraméter szakaszába.</span><span class="sxs-lookup"><span data-stu-id="99b30-134">**Step 2:** Add **two new parameters** "secCertificateThumbprint" and "secCertificateUrlValue" of type "string" to the parameter section of your template.</span></span> <span data-ttu-id="99b30-135">Másolja a következő kódrészletet, és adja hozzá a sablont.</span><span class="sxs-lookup"><span data-stu-id="99b30-135">You can copy the following code snippet and add it to the template.</span></span> <span data-ttu-id="99b30-136">Attól függően, hogy a sablon a forráskiszolgálón előfordulhat, hogy már ezek definiált, ha úgy helyezze át a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="99b30-136">Depending on the source of your template, you may already have these defined, if so move to the next step.</span></span> 
 
```JSON
   "secCertificateThumbprint": {
      "type": "string",
      "metadata": {
        "description": "Certificate Thumbprint"
      }
    },
    "secCertificateUrlValue": {
      "type": "string",
      "metadata": {
        "description": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
      }
    },

```

<span data-ttu-id="99b30-137">**3. lépés:** módosítja a **Microsoft.ServiceFabric/clusters** erőforrás - keresse meg a "Microsoft.ServiceFabric/clusters" erőforrás-definíció a sablonban.</span><span class="sxs-lookup"><span data-stu-id="99b30-137">**Step 3:** Make changes to the **Microsoft.ServiceFabric/clusters** resource - Locate the "Microsoft.ServiceFabric/clusters" resource definition in your template.</span></span> <span data-ttu-id="99b30-138">Az adott definíció tulajdonságai "Tanúsítvány" JSON található címke, amely a következő JSON-részlet hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="99b30-138">Under properties of that definition, you will find "Certificate" JSON tag, which should look something like the following JSON snippet:</span></span>

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="99b30-139">Új címke "thumbprintSecondary" hozzáadása, és adjon neki egy "[parameters('secCertificateThumbprint')]" értéket.</span><span class="sxs-lookup"><span data-stu-id="99b30-139">Add a new tag "thumbprintSecondary" and give it a value "[parameters('secCertificateThumbprint')]".</span></span>  

<span data-ttu-id="99b30-140">Igen, most az erőforrás-definíció a következőképpen kell kinéznie (attól függően, hogy a forrás-sablon, nem lehet pontosan például az alábbi részlet).</span><span class="sxs-lookup"><span data-stu-id="99b30-140">So now the resource definition should look like the following (depending on your source of the template, it may not be exactly like the snippet below).</span></span> 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="99b30-141">Ha azt szeretné, hogy **helyettesítő a cert**, majd adja meg az új tanúsítvány az elsődleges és áthelyezése az aktuális elsődleges, a másodlagos.</span><span class="sxs-lookup"><span data-stu-id="99b30-141">If you want to **rollover the cert**, then specify the new cert as primary and moving the current primary as secondary.</span></span> <span data-ttu-id="99b30-142">Ennek eredményeképp az aktuális elsődleges tanúsítvány helyettesítő egy központi telepítési lépésben új tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="99b30-142">This results in the rollover of your current primary certificate to the new certificate in one deployment step.</span></span>

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


<span data-ttu-id="99b30-143">**4. lépés:** módosítása **összes** a **Microsoft.Compute/virtualMachineScaleSets** erőforrás-definíciókban - keresse meg a Microsoft.Compute/virtualMachineScaleSets erőforrás definíciója.</span><span class="sxs-lookup"><span data-stu-id="99b30-143">**Step 4:** Make changes to **all** the **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate the Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="99b30-144">Görgessen az "publisher": "Microsoft.Azure.ServiceFabric", "virtualMachineProfile" alatt.</span><span class="sxs-lookup"><span data-stu-id="99b30-144">Scroll to the "publisher": "Microsoft.Azure.ServiceFabric", under "virtualMachineProfile".</span></span>

<span data-ttu-id="99b30-145">A service fabric publisher beállításaiban megtekintheti az alábbihoz hasonló.</span><span class="sxs-lookup"><span data-stu-id="99b30-145">In the service fabric publisher settings, you should see something like this.</span></span>

![Json_Pub_Setting1][Json_Pub_Setting1]

<span data-ttu-id="99b30-147">Az új tanúsítvány-bejegyzések felvétele</span><span class="sxs-lookup"><span data-stu-id="99b30-147">Add the new cert entries to it</span></span>

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

<span data-ttu-id="99b30-148">A Tulajdonságok most példához hasonló</span><span class="sxs-lookup"><span data-stu-id="99b30-148">The properties should now look like this</span></span>

![Json_Pub_Setting2][Json_Pub_Setting2]

<span data-ttu-id="99b30-150">Ha azt szeretné, hogy **helyettesítő a cert**, majd adja meg az új tanúsítvány az elsődleges és áthelyezése az aktuális elsődleges, a másodlagos.</span><span class="sxs-lookup"><span data-stu-id="99b30-150">If you want to **rollover the cert**, then specify the new cert as primary and moving the current primary as secondary.</span></span> <span data-ttu-id="99b30-151">Ennek eredményeképp az aktuális tanúsítvány helyettesítő egy központi telepítési lépésben új tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="99b30-151">This results in the rollover of your current certificate to the new certificate in one deployment step.</span></span> 


```JSON
               "certificate": {
                   "thumbprint": "[parameters('secCertificateThumbprint')]",
                   "x509StoreName": "[parameters('certificateStoreValue')]"
                     },
               "certificateSecondary": {
                    "thumbprint": "[parameters('certificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```
<span data-ttu-id="99b30-152">A Tulajdonságok most példához hasonló</span><span class="sxs-lookup"><span data-stu-id="99b30-152">The properties should now look like this</span></span>

![Json_Pub_Setting3][Json_Pub_Setting3]


<span data-ttu-id="99b30-154">**5. lépés:** módosítása **összes** a **Microsoft.Compute/virtualMachineScaleSets** erőforrás-definíciókban - keresse meg a Microsoft.Compute/virtualMachineScaleSets erőforrás definíciója.</span><span class="sxs-lookup"><span data-stu-id="99b30-154">**Step 5:** Make Changes to **all** the **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate the Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="99b30-155">A "vaultCertificates" görgessen:, a "OSProfile".</span><span class="sxs-lookup"><span data-stu-id="99b30-155">Scroll to the "vaultCertificates": , under "OSProfile".</span></span> <span data-ttu-id="99b30-156">akkor kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="99b30-156">it should look something like this.</span></span>


![Json_Pub_Setting4][Json_Pub_Setting4]

<span data-ttu-id="99b30-158">Adja hozzá a secCertificateUrlValue azt.</span><span class="sxs-lookup"><span data-stu-id="99b30-158">Add the secCertificateUrlValue to it.</span></span> <span data-ttu-id="99b30-159">a következő kódrészletet használja:</span><span class="sxs-lookup"><span data-stu-id="99b30-159">use the following snippet:</span></span>

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
<span data-ttu-id="99b30-160">Most már az eredményül kapott Json kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="99b30-160">Now the resulting Json should look something like this.</span></span>
<span data-ttu-id="99b30-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span><span class="sxs-lookup"><span data-stu-id="99b30-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span></span>


> [!NOTE]
> <span data-ttu-id="99b30-162">Győződjön meg arról, hogy Ön a következő ismétlődő 4. és 5 a Nodetypes/Microsoft.Compute/virtualMachineScaleSets erőforrás definíciókat a sablonban.</span><span class="sxs-lookup"><span data-stu-id="99b30-162">Make sure that you have repeated steps 4 and 5 for all the Nodetypes/Microsoft.Compute/virtualMachineScaleSets resource definitions in your template.</span></span> <span data-ttu-id="99b30-163">Ha kihagyná valamelyik őket, a tanúsítvány nem beolvasása telepítve az, hogy VMSS, és látható következményekkel járhat a fürtben, beleértve a fürt is (Ha Ön végül nem érvényes tanúsítványokat, a fürt biztonsági is használhat.</span><span class="sxs-lookup"><span data-stu-id="99b30-163">If you miss one of them, the certificate will not get installed on that VMSS and you will have unpredictable results in your cluster, including the cluster going down (if you end up with no valid certificates that the cluster can use for security.</span></span> <span data-ttu-id="99b30-164">Ezért kérjük, ellenőrizze, a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="99b30-164">So please double check, before proceeding further.</span></span>
> 
> 


### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a><span data-ttu-id="99b30-165">A sablonfájl megfelelően a korábban ismertetett módon hozzáadott új paraméterek szerkesztése</span><span class="sxs-lookup"><span data-stu-id="99b30-165">Edit your template file to reflect the new parameters you added above</span></span>
<span data-ttu-id="99b30-166">Ha a mintát használ a [git-tárház](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) követéséhez, megkezdheti az módosítsa a minta 5-VM-1-NodeType tulajdonságok értéke-Secure.paramters_Step2.JSON</span><span class="sxs-lookup"><span data-stu-id="99b30-166">If you are using the sample from the [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) to follow along, you can start to make changes in The sample 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON</span></span> 

<span data-ttu-id="99b30-167">A Resource Manager-sablon paraméter fájl szerkesztése, vegye fel a két új secCertificateThumbprint és paramétereinek secCertificateUrlValue.</span><span class="sxs-lookup"><span data-stu-id="99b30-167">Edit your Resource Manager Template parameter File, add the two new parameters for secCertificateThumbprint and secCertificateUrlValue.</span></span> 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-the-template-to-azure"></a><span data-ttu-id="99b30-168">A sablon telepítéséhez az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="99b30-168">Deploy the template to Azure</span></span>

- <span data-ttu-id="99b30-169">Most már készen áll a sablon telepítéséhez az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="99b30-169">You are now ready to deploy your template to Azure.</span></span> <span data-ttu-id="99b30-170">Nyisson meg egy Azure PS 1 vagy újabb parancssort.</span><span class="sxs-lookup"><span data-stu-id="99b30-170">Open an Azure PS version 1+ command prompt.</span></span>
- <span data-ttu-id="99b30-171">Jelentkezzen be az Azure-fiókjába, és válassza ki az adott azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="99b30-171">Log in to your Azure Account and select the specific azure subscription.</span></span> <span data-ttu-id="99b30-172">Ez egy fontos lépés a segítsen, akik hozzáférhetnek a több azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="99b30-172">This is an important step for folks who have access to more than one azure subscription.</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

<span data-ttu-id="99b30-173">Tesztelje a sablon üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="99b30-173">Test the template prior to deploying it.</span></span> <span data-ttu-id="99b30-174">Ugyanabban az erőforráscsoportban, amely a fürt jelenleg a rendszer használja.</span><span class="sxs-lookup"><span data-stu-id="99b30-174">Use the same Resource Group that your cluster is currently deployed to.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

<span data-ttu-id="99b30-175">Az erőforráscsoport a sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="99b30-175">Deploy the template to your resource group.</span></span> <span data-ttu-id="99b30-176">Ugyanabban az erőforráscsoportban, amely a fürt jelenleg a rendszer használja.</span><span class="sxs-lookup"><span data-stu-id="99b30-176">Use the same Resource Group that your cluster is currently deployed to.</span></span> <span data-ttu-id="99b30-177">Futtassa a New-AzureRmResourceGroupDeployment parancsot.</span><span class="sxs-lookup"><span data-stu-id="99b30-177">Run the New-AzureRmResourceGroupDeployment command.</span></span> <span data-ttu-id="99b30-178">Nem kell megadnia a a módot, mivel az alapértelmezett érték **növekményes**.</span><span class="sxs-lookup"><span data-stu-id="99b30-178">You do not need to specify the mode, since the default value is **incremental**.</span></span>

> [!NOTE]
> <span data-ttu-id="99b30-179">Mód Complete állítja be, ha véletlenül erőforrásokat, amelyek nem szerepelnek a sablon törölheti.</span><span class="sxs-lookup"><span data-stu-id="99b30-179">If you set Mode to Complete, you can inadvertently delete resources that are not in your template.</span></span> <span data-ttu-id="99b30-180">Ebben a forgatókönyvben azt nem használja.</span><span class="sxs-lookup"><span data-stu-id="99b30-180">So do not use it in this scenario.</span></span>
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

<span data-ttu-id="99b30-181">Ez az azonos powershell kitöltött példát.</span><span class="sxs-lookup"><span data-stu-id="99b30-181">Here is a filled out example of the same powershell.</span></span>

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

<span data-ttu-id="99b30-182">Ha a telepítés befejeződött, csatlakozzon a fürthöz, az új tanúsítványt használja, és hajtsa végre az egyes lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="99b30-182">Once the deployment is complete, connect to your cluster using the new Certificate and perform some queries.</span></span> <span data-ttu-id="99b30-183">Ha tudja megtenni.</span><span class="sxs-lookup"><span data-stu-id="99b30-183">If you are able to do.</span></span> <span data-ttu-id="99b30-184">Törölheti a régi tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="99b30-184">Then you can delete the old certificate.</span></span> 

<span data-ttu-id="99b30-185">Ha önaláírt tanúsítványt használ, ne felejtse el a megbízható személyek nevű helyi tanúsítványtároló importálja azokat.</span><span class="sxs-lookup"><span data-stu-id="99b30-185">If you are using a self-signed certificate, do not forget to import them into your local TrustedPeople cert store.</span></span>

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
<span data-ttu-id="99b30-186">Gyors referenciaként Ez a parancs egy biztonságos fürthöz való kapcsolódás</span><span class="sxs-lookup"><span data-stu-id="99b30-186">For quick reference here is the command to connect to a secure cluster</span></span> 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
<span data-ttu-id="99b30-187">Gyors referenciaként Ez a parancs használatával beszerezheti a fürt állapota</span><span class="sxs-lookup"><span data-stu-id="99b30-187">For quick reference here is the command to get cluster health</span></span>

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-to-the-cluster"></a><span data-ttu-id="99b30-188">A fürt alkalmazás tanúsítványok telepítését.</span><span class="sxs-lookup"><span data-stu-id="99b30-188">Deploying Application certificates to the cluster.</span></span>

<span data-ttu-id="99b30-189">A fenti lépéseket 5 a csomópontra a keyvault alapján telepített tanúsítványok is használhatja ugyanazokat a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="99b30-189">You can use the same steps as outlined in Steps 5 above to have the certificates deployed from a keyvault to the Nodes.</span></span> <span data-ttu-id="99b30-190">ugyanúgy kell definiálására és használjon más paramétereket.</span><span class="sxs-lookup"><span data-stu-id="99b30-190">you just need define and use different parameters.</span></span>


## <a name="adding-or-removing-client-certificates"></a><span data-ttu-id="99b30-191">Hozzáadása vagy eltávolítása az ügyfél-tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="99b30-191">Adding or removing Client certificates</span></span>

<span data-ttu-id="99b30-192">A fürt tanúsítvány mellett meg a service fabric-fürt felügyeleti műveletek elvégzéséhez ügyféltanúsítványok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="99b30-192">In addition to the cluster certificates, you can add client certificates to perform management operations on a service fabric cluster.</span></span>

<span data-ttu-id="99b30-193">Kétféle ügyféltanúsítványok - rendszergazda adhat hozzá vagy csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="99b30-193">You can add two kinds of client certificates - Admin or Read-only.</span></span> <span data-ttu-id="99b30-194">Ezek ezután használhatók a felügyeleti műveletek és a lekérdezési műveletek a fürthöz való hozzáférés vezérlése érdekében.</span><span class="sxs-lookup"><span data-stu-id="99b30-194">These then can be used to control access to the admin operations and Query operations on the cluster.</span></span> <span data-ttu-id="99b30-195">Alapértelmezés szerint a fürt tanúsítványok hozzáadódnak a megengedett felügyeleti tanúsítványok listája.</span><span class="sxs-lookup"><span data-stu-id="99b30-195">By default, the cluster certificates are added to the allowed Admin certificates list.</span></span>

<span data-ttu-id="99b30-196">megadhatja, hogy minden ügyfél-tanúsítványok száma.</span><span class="sxs-lookup"><span data-stu-id="99b30-196">you can specify any number of client certificates.</span></span> <span data-ttu-id="99b30-197">A service fabric-fürt az egy konfigurációs eredmények minden és törléséről</span><span class="sxs-lookup"><span data-stu-id="99b30-197">Each addition/deletion results in a configuration update to the service fabric cluster</span></span>


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a><span data-ttu-id="99b30-198">Ügyféltanúsítványok - rendszergazda felvétele vagy csak olvasható-portálon</span><span class="sxs-lookup"><span data-stu-id="99b30-198">Adding client certificates - Admin or Read-Only via portal</span></span>

1. <span data-ttu-id="99b30-199">Keresse meg a biztonsági panelt, és válassza ki a "+ hitelesítés" gombra a biztonsági panel felett.</span><span class="sxs-lookup"><span data-stu-id="99b30-199">Navigate to the Security blade, and select the '+ Authentication' button on top of the security blade.</span></span>
2. <span data-ttu-id="99b30-200">A hitelesítés hozzáadása panelen válassza ki a "hitelesítés" - "Csak olvasható" vagy "Admin ügyfélszámítógépekhez"</span><span class="sxs-lookup"><span data-stu-id="99b30-200">On the 'Add Authentication' blade, choose the 'Authentication Type' - 'Read-only client' or 'Admin client'</span></span>
3. <span data-ttu-id="99b30-201">Most adja meg a hitelesítési módját.</span><span class="sxs-lookup"><span data-stu-id="99b30-201">Now choose the Authorization method.</span></span> <span data-ttu-id="99b30-202">Ez azt jelzi, hogy a Service Fabric e keresi ezt a tanúsítványt a tulajdonos neve vagy az ujjlenyomat használatával.</span><span class="sxs-lookup"><span data-stu-id="99b30-202">This indicates to Service Fabric whether it should look up this certificate by using the subject name or the thumbprint.</span></span> <span data-ttu-id="99b30-203">Ez általában nem egy jó biztonsági gyakorlat a tulajdonos neve a hitelesítési módszer használatát.</span><span class="sxs-lookup"><span data-stu-id="99b30-203">In general, it is not a good security practice to use the authorization method of subject name.</span></span> 

![Ügyfél-tanúsítvány hozzáadása][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-the-portal"></a><span data-ttu-id="99b30-205">Ügyféltanúsítványok - rendszergazdai vagy csak olvasható a portál használatával törlése</span><span class="sxs-lookup"><span data-stu-id="99b30-205">Deletion of Client Certificates - Admin or Read-Only using the portal</span></span>

<span data-ttu-id="99b30-206">Távolítsa el a másodlagos tanúsítvány használatát a fürt biztonsági okokból és a biztonsági panel lép, és válassza a "Törlés" lehetőséget az adott tanúsítvány a helyi menüből.</span><span class="sxs-lookup"><span data-stu-id="99b30-206">To remove a secondary certificate from being used for cluster security, Navigate to the Security blade and select the 'Delete' option from the context menu on the specific certificate.</span></span>



## <a name="next-steps"></a><span data-ttu-id="99b30-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99b30-207">Next steps</span></span>
<span data-ttu-id="99b30-208">Ezek a cikkek további információt a kiszolgálófürt-felügyelet olvasható:</span><span class="sxs-lookup"><span data-stu-id="99b30-208">Read these articles for more information on cluster management:</span></span>

* [<span data-ttu-id="99b30-209">Service Fabric-fürt verziófrissítés folyamatáról és az Ön elvárásainak</span><span class="sxs-lookup"><span data-stu-id="99b30-209">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="99b30-210">Az ügyfelek a szerepköralapú hozzáférés-telepítő</span><span class="sxs-lookup"><span data-stu-id="99b30-210">Setup role-based access for clients</span></span>](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


