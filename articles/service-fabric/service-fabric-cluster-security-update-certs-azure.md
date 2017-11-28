---
title: "az Azure Service Fabric-fürt aaaManage tanúsítványok |} Microsoft Docs"
description: "Ismerteti, hogyan tooadd új tanúsítványokat, a helyettesítő tanúsítványt és a remove tanúsítvány tooor a Service Fabric-fürt."
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
ms.openlocfilehash: 8e57bd95dbb800ecc04cf6988047e3abdc2fe56a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a><span data-ttu-id="bc6ec-103">Hozzáadhat és eltávolíthat tanúsítványokat a Service Fabric-fürtök az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="bc6ec-103">Add or remove certificates for a Service Fabric cluster in Azure</span></span>
<span data-ttu-id="bc6ec-104">Javasoljuk, hogy megismerje a módját a Service Fabric X.509-tanúsítványokat használ, és hello ismernie kell a [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="bc6ec-104">It is recommended that you familiarize yourself with how Service Fabric uses X.509 certificates and be familiar with hello [Cluster security scenarios](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="bc6ec-105">Ismernie kell fürt tanúsítvány és felhasználási területének, mielőtt végrehajtásának folytatásához.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-105">You must understand what a cluster certificate is and what is used for, before you proceed further.</span></span>

<span data-ttu-id="bc6ec-106">Megadja, hogy két fürt tanúsítványok, egy elsődleges és másodlagos, amikor konfigurálja a szolgáltatás háló lehetővé biztonsági tanúsítvány tooclient tanúsítványok hozzáadása a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-106">Service fabric lets you specify two cluster certificates, a primary and a secondary, when you configure certificate security during cluster creation, in addition tooclient certificates.</span></span> <span data-ttu-id="bc6ec-107">Tekintse meg a túl[létrehozása egy azure-portálon fürttel](service-fabric-cluster-creation-via-portal.md) vagy [létrehozása az azure-fürttel Azure Resource Manageren keresztül](service-fabric-cluster-creation-via-arm.md) azok beállításával kapcsolatos részleteket létre idő.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-107">Refer too[creating an azure cluster via portal](service-fabric-cluster-creation-via-portal.md) or [creating an azure cluster via Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) for details on setting them up at create time.</span></span> <span data-ttu-id="bc6ec-108">Ha csak egy fürt tanúsítványt ad meg, létrehozás ideje, majd használt hello elsődleges tanúsítványként.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-108">If you specify only one cluster certificate at create time, then that is used as hello primary certificate.</span></span> <span data-ttu-id="bc6ec-109">Fürt létrehozása után hozzáadhat egy új tanúsítványt, a egy másodlagos.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-109">After cluster creation, you can add a new certificate as a secondary.</span></span>

> [!NOTE]
> <span data-ttu-id="bc6ec-110">Biztonságos fürt esetén mindig szüksége lesz legalább egy érvényes (a nem visszavont és a nem lejárt) fürtre (elsődleges vagy másodlagos) telepített tanúsítvány (Ha nem, hello fürt leáll a működése).</span><span class="sxs-lookup"><span data-stu-id="bc6ec-110">For a secure cluster, you will always need at least one valid (not revoked and not expired) cluster certificate (primary or secondary) deployed (if not, hello cluster stops functioning).</span></span> <span data-ttu-id="bc6ec-111">90 napig minden érvényes tanúsítvány lejárati, elérni hello rendszer állít elő, egy figyelmeztetés nyomkövetést, és egy figyelmeztetés állapotesemény hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-111">90 days before all valid certificates reach expiration, hello system generates a warning trace and also a warning health event on hello node.</span></span> <span data-ttu-id="bc6ec-112">Jelenleg nincs e-mail vagy más, ebben a témakörben a küld a service fabric értesítést.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-112">There is currently no email or any other notification that service fabric sends out on this topic.</span></span> 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-hello-portal"></a><span data-ttu-id="bc6ec-113">Hello portálon fürt másodlagos tanúsítvány hozzáadásához</span><span class="sxs-lookup"><span data-stu-id="bc6ec-113">Add a secondary cluster certificate using hello portal</span></span>

<span data-ttu-id="bc6ec-114">Másodlagos fürt tanúsítvány nem adható hozzá hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-114">Secondary cluster certificate cannot be added through hello Azure portal.</span></span> <span data-ttu-id="bc6ec-115">Toouse Azure rendelkezik, amely a powershell.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-115">You have toouse Azure powershell for that.</span></span> <span data-ttu-id="bc6ec-116">a dokumentum későbbi szakaszában meghatározott hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-116">hello process is outlined later in this document.</span></span>

## <a name="swap-hello-cluster-certificates-using-hello-portal"></a><span data-ttu-id="bc6ec-117">A felcserélendő hello fürt tanúsítványok hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="bc6ec-117">Swap hello cluster certificates using hello portal</span></span>

<span data-ttu-id="bc6ec-118">Miután sikeresen telepítette a fürt másodlagos tanúsítvány, ha azt szeretné, tooswap hello elsődleges és másodlagos, majd keresse meg a toohello biztonsági panel, és hello "Elsődleges felcserélés" beállítást választva hello helyi menü tooswap hello másodlagos cert a hello elsődleges cert.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-118">After you have successfully deployed a secondary cluster certificate, if you want tooswap hello primary and secondary, then navigate toohello Security blade, and select hello 'Swap with primary' option from hello context menu tooswap hello secondary cert with hello primary cert.</span></span>

![Lapozófájl-kapacitás tanúsítvány][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-hello-portal"></a><span data-ttu-id="bc6ec-120">Távolítsa el a fürt tanúsítványt hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="bc6ec-120">Remove a cluster certificate using hello portal</span></span>

<span data-ttu-id="bc6ec-121">Biztonságos fürt esetén mindig szüksége lesz legalább egy érvényes (a nem visszavont és a nem lejárt) (elsődleges vagy másodlagos) telepített tanúsítvány Ha nem, hello fürt megszűnik működni.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-121">For a secure cluster, you will always need at least one valid (not revoked and not expired) certificate (primary or secondary) deployed if not, hello cluster stops functioning.</span></span>

<span data-ttu-id="bc6ec-122">a fürt biztonsági, Navigálás toohello biztonsági panel és select hello "Delete" kapcsolót hello helyi menüből hello másodlagos tanúsítvány használatát másodlagos tanúsítvány tooremove.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-122">tooremove a secondary certificate from being used for cluster security, Navigate toohello Security blade and select hello 'Delete' option from hello context menu on hello secondary certificate.</span></span>

<span data-ttu-id="bc6ec-123">Ha a szándéka az elsődleges, van megjelölve, akkor kell tooswap tooremove hello tanúsítványt a másodlagos hello először, és törölje hello másodlagos, hello frissítés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-123">If your intent is tooremove hello certificate that is marked primary, then you will need tooswap it with hello secondary first, and then delete hello secondary after hello upgrade has completed.</span></span>

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a><span data-ttu-id="bc6ec-124">Erőforrás-kezelő Powershell-lel másodlagos tanúsítvány hozzáadásához</span><span class="sxs-lookup"><span data-stu-id="bc6ec-124">Add a secondary certificate using Resource Manager Powershell</span></span>

<span data-ttu-id="bc6ec-125">Ezek a lépések feltételezik, hogy ismeri a Resource Manager működésével és legalább egy Service Fabric-fürt Resource Manager-sablon használatával telepített, és be lesz szüksége hello fürtöt tooset használt hello sablont.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-125">These steps assume that you are familiar with how Resource Manager works and have deployed atleast one Service Fabric cluster using a Resource Manager template, and have hello template that you used tooset up hello cluster handy.</span></span> <span data-ttu-id="bc6ec-126">Az Ön számára elfogadható JSON használatával is feltételezzük.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-126">It is also assumed that you are comfortable using JSON.</span></span>

> [!NOTE]
> <span data-ttu-id="bc6ec-127">Ha a keresett mintasablon és, hogy toofollow mentén vagy kiindulási pontként használható paramétereket, majd töltse le azt a ez [git-tárház](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="bc6ec-127">If you are looking for a sample template and parameters that you can use toofollow along or as a starting point, then download it from this [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span> 
> 
> 

### <a name="edit-your-resource-manager-template"></a><span data-ttu-id="bc6ec-128">A Resource Manager sablon szerkesztése</span><span class="sxs-lookup"><span data-stu-id="bc6ec-128">Edit your Resource Manager template</span></span>

<span data-ttu-id="bc6ec-129">A következő mentén megkönnyítése érdekében minta 5-VM-1-NodeType tulajdonságok értéke-Secure_Step2.JSON frissítjük összes hello módosításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-129">For ease of following along, sample 5-VM-1-NodeTypes-Secure_Step2.JSON contains all hello edits we will be making.</span></span> <span data-ttu-id="bc6ec-130">hello minta érhető el: [git-tárház](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="bc6ec-130">hello sample is available at [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span>

<span data-ttu-id="bc6ec-131">**Győződjön meg arról, hogy toofollow hello lépéseket**</span><span class="sxs-lookup"><span data-stu-id="bc6ec-131">**Make sure toofollow all hello steps**</span></span>

<span data-ttu-id="bc6ec-132">**1. lépés:** hello Resource Manager sablon, amelyet fürtbe toodeploy nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-132">**Step 1:** Open up hello Resource Manager template you used toodeploy you Cluster.</span></span> <span data-ttu-id="bc6ec-133">(Ha hello minta letöltötte a fenti tárház hello, majd használja az 5-VM-1-NodeType tulajdonságok értéke-Secure_Step1.JSON toodeploy biztonságos fürt, és nyisson meg, hogy a sablon mentése).</span><span class="sxs-lookup"><span data-stu-id="bc6ec-133">(If you have downloaded hello sample from hello above repo, then Use 5-VM-1-NodeTypes-Secure_Step1.JSON toodeploy a secure cluster and then open up that template).</span></span>

<span data-ttu-id="bc6ec-134">**2. lépés:** Hozzáadás **két új paraméterrel** "secCertificateThumbprint" és "secCertificateUrlValue" típusú "string" toohello paraméter szakasza a sablont.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-134">**Step 2:** Add **two new parameters** "secCertificateThumbprint" and "secCertificateUrlValue" of type "string" toohello parameter section of your template.</span></span> <span data-ttu-id="bc6ec-135">A következő kódrészletet hello lemásolhatja és toohello sablon hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-135">You can copy hello following code snippet and add it toohello template.</span></span> <span data-ttu-id="bc6ec-136">Attól függően, hogy a sablon hello forrását már előfordulhat, hogy ezeknek a definiált, ha így folytassa toohello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-136">Depending on hello source of your template, you may already have these defined, if so move toohello next step.</span></span> 
 
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
        "description": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
      }
    },

```

<span data-ttu-id="bc6ec-137">**3. lépés:** módosítása toohello **Microsoft.ServiceFabric/clusters** erőforrás - keresse meg a sablon hello "Microsoft.ServiceFabric/clusters" erőforrás-definícióban.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-137">**Step 3:** Make changes toohello **Microsoft.ServiceFabric/clusters** resource - Locate hello "Microsoft.ServiceFabric/clusters" resource definition in your template.</span></span> <span data-ttu-id="bc6ec-138">Az adott definíció tulajdonságai "Tanúsítvány" JSON található címke, amely a következő JSON részlet hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="bc6ec-138">Under properties of that definition, you will find "Certificate" JSON tag, which should look something like hello following JSON snippet:</span></span>

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="bc6ec-139">Új címke "thumbprintSecondary" hozzáadása, és adjon neki egy "[parameters('secCertificateThumbprint')]" értéket.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-139">Add a new tag "thumbprintSecondary" and give it a value "[parameters('secCertificateThumbprint')]".</span></span>  

<span data-ttu-id="bc6ec-140">Igen, most hello erőforrás-definíció hello hasonlóan kell kinéznie (attól függően, hogy a forrás hello sablon, nem lehet pontosan például az alábbi hello részlet).</span><span class="sxs-lookup"><span data-stu-id="bc6ec-140">So now hello resource definition should look like hello following (depending on your source of hello template, it may not be exactly like hello snippet below).</span></span> 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="bc6ec-141">Ha azt szeretné, túl**helyettesítő hello cert**, majd adja meg az új tanúsítvány hello az elsődleges és a mozgási hello aktuális elsődleges, a másodlagos.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-141">If you want too**rollover hello cert**, then specify hello new cert as primary and moving hello current primary as secondary.</span></span> <span data-ttu-id="bc6ec-142">Ez az aktuális elsődleges tanúsítvány toohello új tanúsítvány egy központi telepítési lépésben hello helyettesítő eredményez.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-142">This results in hello rollover of your current primary certificate toohello new certificate in one deployment step.</span></span>

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


<span data-ttu-id="bc6ec-143">**4. lépés:** módosítása túl**összes** hello **Microsoft.Compute/virtualMachineScaleSets** erőforrás-definíciókban - hello Microsoft.Compute/virtualMachineScaleSets erőforrás található. definíciója.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-143">**Step 4:** Make changes too**all** hello **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate hello Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="bc6ec-144">Görgessen toohello "publisher": "Microsoft.Azure.ServiceFabric", "virtualMachineProfile" alatt.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-144">Scroll toohello "publisher": "Microsoft.Azure.ServiceFabric", under "virtualMachineProfile".</span></span>

<span data-ttu-id="bc6ec-145">A hello szolgáltatás háló publisher beállításaiban megtekintheti az alábbihoz hasonló.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-145">In hello service fabric publisher settings, you should see something like this.</span></span>

![Json_Pub_Setting1][Json_Pub_Setting1]

<span data-ttu-id="bc6ec-147">Új tanúsítvány bejegyzések tooit hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bc6ec-147">Add hello new cert entries tooit</span></span>

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

<span data-ttu-id="bc6ec-148">hello tulajdonságok most példához hasonló</span><span class="sxs-lookup"><span data-stu-id="bc6ec-148">hello properties should now look like this</span></span>

![Json_Pub_Setting2][Json_Pub_Setting2]

<span data-ttu-id="bc6ec-150">Ha azt szeretné, túl**helyettesítő hello cert**, majd adja meg az új tanúsítvány hello az elsődleges és a mozgási hello aktuális elsődleges, a másodlagos.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-150">If you want too**rollover hello cert**, then specify hello new cert as primary and moving hello current primary as secondary.</span></span> <span data-ttu-id="bc6ec-151">Ez az aktuális tanúsítvány toohello új tanúsítványt egy központi telepítési lépésben hello kapcsolódó kulcsváltást eredményez.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-151">This results in hello rollover of your current certificate toohello new certificate in one deployment step.</span></span> 


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
<span data-ttu-id="bc6ec-152">hello tulajdonságok most példához hasonló</span><span class="sxs-lookup"><span data-stu-id="bc6ec-152">hello properties should now look like this</span></span>

![Json_Pub_Setting3][Json_Pub_Setting3]


<span data-ttu-id="bc6ec-154">**5. lépés:** módosításokat túl**összes** hello **Microsoft.Compute/virtualMachineScaleSets** erőforrás-definíciókban - hello Microsoft.Compute/virtualMachineScaleSets erőforrás található. definíciója.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-154">**Step 5:** Make Changes too**all** hello **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate hello Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="bc6ec-155">Görgessen toohello "vaultCertificates":, a "OSProfile".</span><span class="sxs-lookup"><span data-stu-id="bc6ec-155">Scroll toohello "vaultCertificates": , under "OSProfile".</span></span> <span data-ttu-id="bc6ec-156">akkor kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-156">it should look something like this.</span></span>


![Json_Pub_Setting4][Json_Pub_Setting4]

<span data-ttu-id="bc6ec-158">Adja hozzá a hello secCertificateUrlValue tooit.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-158">Add hello secCertificateUrlValue tooit.</span></span> <span data-ttu-id="bc6ec-159">a következő kódrészletet hello használata:</span><span class="sxs-lookup"><span data-stu-id="bc6ec-159">use hello following snippet:</span></span>

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
<span data-ttu-id="bc6ec-160">Most Json eredő hello kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-160">Now hello resulting Json should look something like this.</span></span>
<span data-ttu-id="bc6ec-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span><span class="sxs-lookup"><span data-stu-id="bc6ec-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span></span>


> [!NOTE]
> <span data-ttu-id="bc6ec-162">Győződjön meg arról, hogy Ön a következő ismétlődő 4. és 5 Nodetypes/Microsoft.Compute/virtualMachineScaleSets erőforrás-definíciók minden hello a sablonban.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-162">Make sure that you have repeated steps 4 and 5 for all hello Nodetypes/Microsoft.Compute/virtualMachineScaleSets resource definitions in your template.</span></span> <span data-ttu-id="bc6ec-163">Ha kihagyná valamelyik őket, hello tanúsítvány nem az adott VMSS beolvasása telepítve, és hogy előre az eredmény a fürtben, beleértve a hello fürt is (Ha nem érvényes tanúsítványokkal hello fürthöz használható biztonsági végén.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-163">If you miss one of them, hello certificate will not get installed on that VMSS and you will have unpredictable results in your cluster, including hello cluster going down (if you end up with no valid certificates that hello cluster can use for security.</span></span> <span data-ttu-id="bc6ec-164">Ezért kérjük, ellenőrizze, a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-164">So please double check, before proceeding further.</span></span>
> 
> 


### <a name="edit-your-template-file-tooreflect-hello-new-parameters-you-added-above"></a><span data-ttu-id="bc6ec-165">A fájl tooreflect hello új Sablonparaméterek korábban ismertetett módon hozzáadott szerkesztése</span><span class="sxs-lookup"><span data-stu-id="bc6ec-165">Edit your template file tooreflect hello new parameters you added above</span></span>
<span data-ttu-id="bc6ec-166">Ha hello hello mintát használ [git-tárház](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) toofollow jelszavat, megkezdheti hello minta 5-VM-1-NodeType tulajdonságok értéke-Secure.paramters_Step2.JSON toomake módosítások</span><span class="sxs-lookup"><span data-stu-id="bc6ec-166">If you are using hello sample from hello [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) toofollow along, you can start toomake changes in hello sample 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON</span></span> 

<span data-ttu-id="bc6ec-167">A Resource Manager-sablon paraméter fájl szerkesztése, hello két új secCertificateThumbprint és a paraméterek secCertificateUrlValue hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-167">Edit your Resource Manager Template parameter File, add hello two new parameters for secCertificateThumbprint and secCertificateUrlValue.</span></span> 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-hello-template-tooazure"></a><span data-ttu-id="bc6ec-168">Hello sablon tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="bc6ec-168">Deploy hello template tooAzure</span></span>

- <span data-ttu-id="bc6ec-169">Ön most már készen áll a toodeploy a sablon tooAzure vannak.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-169">You are now ready toodeploy your template tooAzure.</span></span> <span data-ttu-id="bc6ec-170">Nyisson meg egy Azure PS 1 vagy újabb parancssort.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-170">Open an Azure PS version 1+ command prompt.</span></span>
- <span data-ttu-id="bc6ec-171">Jelentkezzen be Azure-fiók tooyour, majd válassza a hello adott azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-171">Log in tooyour Azure Account and select hello specific azure subscription.</span></span> <span data-ttu-id="bc6ec-172">Ez egy fontos eleme a hozzáférés toomore, mint egy azure-előfizetéssel rendelkező segítsen.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-172">This is an important step for folks who have access toomore than one azure subscription.</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

<span data-ttu-id="bc6ec-173">Hello sablon előzetes toodeploying tesztelje azt.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-173">Test hello template prior toodeploying it.</span></span> <span data-ttu-id="bc6ec-174">Használjon hello ugyanahhoz az erőforráscsoporthoz tartozik, amely a fürt jelenleg a rendszer.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-174">Use hello same Resource Group that your cluster is currently deployed to.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

<span data-ttu-id="bc6ec-175">Hello sablon tooyour erőforrás-csoport központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-175">Deploy hello template tooyour resource group.</span></span> <span data-ttu-id="bc6ec-176">Használjon hello ugyanahhoz az erőforráscsoporthoz tartozik, amely a fürt jelenleg a rendszer.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-176">Use hello same Resource Group that your cluster is currently deployed to.</span></span> <span data-ttu-id="bc6ec-177">Hello New-AzureRmResourceGroupDeployment parancsot.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-177">Run hello New-AzureRmResourceGroupDeployment command.</span></span> <span data-ttu-id="bc6ec-178">Mivel hello alapértelmezett értéke, nem kell toospecify hello mód, **növekményes**.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-178">You do not need toospecify hello mode, since hello default value is **incremental**.</span></span>

> [!NOTE]
> <span data-ttu-id="bc6ec-179">Ha mód tooComplete, erőforrásokat, amelyek nem szerepelnek a sablon akaratlanul is törölheti.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-179">If you set Mode tooComplete, you can inadvertently delete resources that are not in your template.</span></span> <span data-ttu-id="bc6ec-180">Ebben a forgatókönyvben azt nem használja.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-180">So do not use it in this scenario.</span></span>
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

<span data-ttu-id="bc6ec-181">Ez egy kitöltött kimenő hello példa azonos powershell.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-181">Here is a filled out example of hello same powershell.</span></span>

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

<span data-ttu-id="bc6ec-182">Hello központi telepítés befejezése után tooyour fürt használatával kapcsolódó levelezőprogramokkal hello új tanúsítványt, és hajtson végre néhány lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-182">Once hello deployment is complete, connect tooyour cluster using hello new Certificate and perform some queries.</span></span> <span data-ttu-id="bc6ec-183">Ha tudja toodo.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-183">If you are able toodo.</span></span> <span data-ttu-id="bc6ec-184">Ezt követően törölheti a régi tanúsítvány hello.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-184">Then you can delete hello old certificate.</span></span> 

<span data-ttu-id="bc6ec-185">Ha önaláírt tanúsítványt használ, ne feledje tooimport a megbízható személyek nevű helyi tanúsítványtároló őket.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-185">If you are using a self-signed certificate, do not forget tooimport them into your local TrustedPeople cert store.</span></span>

```powershell
######## Set up hello certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
<span data-ttu-id="bc6ec-186">Gyors referenciaként Íme hello parancs tooconnect tooa biztonságos fürt</span><span class="sxs-lookup"><span data-stu-id="bc6ec-186">For quick reference here is hello command tooconnect tooa secure cluster</span></span> 

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
<span data-ttu-id="bc6ec-187">Gyors referenciaként Íme hello parancs tooget fürt állapotát</span><span class="sxs-lookup"><span data-stu-id="bc6ec-187">For quick reference here is hello command tooget cluster health</span></span>

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-toohello-cluster"></a><span data-ttu-id="bc6ec-188">Alkalmazás tanúsítványok toohello fürt telepítése.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-188">Deploying Application certificates toohello cluster.</span></span>

<span data-ttu-id="bc6ec-189">Hello azonos lépések ajánlásait az 5. lépéseket a keyvault toohello csomópontok alapján telepített toohave hello tanúsítványokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-189">You can use hello same steps as outlined in Steps 5 above toohave hello certificates deployed from a keyvault toohello Nodes.</span></span> <span data-ttu-id="bc6ec-190">ugyanúgy kell definiálására és használjon más paramétereket.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-190">you just need define and use different parameters.</span></span>


## <a name="adding-or-removing-client-certificates"></a><span data-ttu-id="bc6ec-191">Hozzáadása vagy eltávolítása az ügyfél-tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="bc6ec-191">Adding or removing Client certificates</span></span>

<span data-ttu-id="bc6ec-192">Toohello fürt tanúsítványok hozzáadását adhat hozzá a service fabric-fürt ügyfél tanúsítványok tooperform felügyeleti műveleteihez.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-192">In addition toohello cluster certificates, you can add client certificates tooperform management operations on a service fabric cluster.</span></span>

<span data-ttu-id="bc6ec-193">Kétféle ügyféltanúsítványok - rendszergazda adhat hozzá vagy csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-193">You can add two kinds of client certificates - Admin or Read-only.</span></span> <span data-ttu-id="bc6ec-194">Ezek ezután lehetnek használt toocontrol hozzáférés toohello felügyeleti műveletek és a lekérdezési műveletek hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-194">These then can be used toocontrol access toohello admin operations and Query operations on hello cluster.</span></span> <span data-ttu-id="bc6ec-195">Alapértelmezés szerint hello fürt hozzáadja a tanúsítványokat toohello engedélyezett felügyeleti tanúsítványok listáját.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-195">By default, hello cluster certificates are added toohello allowed Admin certificates list.</span></span>

<span data-ttu-id="bc6ec-196">megadhatja, hogy minden ügyfél-tanúsítványok száma.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-196">you can specify any number of client certificates.</span></span> <span data-ttu-id="bc6ec-197">Minden egyes és törléséről a konfigurációs frissítés toohello service fabric-fürt eredményez.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-197">Each addition/deletion results in a configuration update toohello service fabric cluster</span></span>


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a><span data-ttu-id="bc6ec-198">Ügyféltanúsítványok - rendszergazda felvétele vagy csak olvasható-portálon</span><span class="sxs-lookup"><span data-stu-id="bc6ec-198">Adding client certificates - Admin or Read-Only via portal</span></span>

1. <span data-ttu-id="bc6ec-199">Navigáljon a toohello biztonsági panelen, majd válassza ki a hello "+ hitelesítési" gomb felett hello biztonsági panel.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-199">Navigate toohello Security blade, and select hello '+ Authentication' button on top of hello security blade.</span></span>
2. <span data-ttu-id="bc6ec-200">A hello hitelesítés hozzáadása panelen válassza a "Hitelesítés típusa" - "Csak olvasható" vagy "Admin ügyfélszámítógépekhez" hello</span><span class="sxs-lookup"><span data-stu-id="bc6ec-200">On hello 'Add Authentication' blade, choose hello 'Authentication Type' - 'Read-only client' or 'Admin client'</span></span>
3. <span data-ttu-id="bc6ec-201">Most adja hello hitelesítési módját.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-201">Now choose hello Authorization method.</span></span> <span data-ttu-id="bc6ec-202">Ez a tooService háló jelzi, hogy keresi ezt a tanúsítványt hello tulajdonosnévvel vagy ujjlenyomattal hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-202">This indicates tooService Fabric whether it should look up this certificate by using hello subject name or hello thumbprint.</span></span> <span data-ttu-id="bc6ec-203">Ez általában nem jó biztonsági gyakorlat toouse hello engedélyezési módszer tulajdonos neve.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-203">In general, it is not a good security practice toouse hello authorization method of subject name.</span></span> 

![Ügyfél-tanúsítvány hozzáadása][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-hello-portal"></a><span data-ttu-id="bc6ec-205">Az ügyféltanúsítványok - rendszergazdai vagy csak olvasható használatával törlését hello portál</span><span class="sxs-lookup"><span data-stu-id="bc6ec-205">Deletion of Client Certificates - Admin or Read-Only using hello portal</span></span>

<span data-ttu-id="bc6ec-206">a fürt biztonsági, Navigálás toohello biztonsági panel és select hello "Delete" kapcsolót hello helyi menüből hello adott tanúsítvány használatát másodlagos tanúsítvány tooremove.</span><span class="sxs-lookup"><span data-stu-id="bc6ec-206">tooremove a secondary certificate from being used for cluster security, Navigate toohello Security blade and select hello 'Delete' option from hello context menu on hello specific certificate.</span></span>



## <a name="next-steps"></a><span data-ttu-id="bc6ec-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc6ec-207">Next steps</span></span>
<span data-ttu-id="bc6ec-208">Ezek a cikkek további információt a kiszolgálófürt-felügyelet olvasható:</span><span class="sxs-lookup"><span data-stu-id="bc6ec-208">Read these articles for more information on cluster management:</span></span>

* [<span data-ttu-id="bc6ec-209">Service Fabric-fürt verziófrissítés folyamatáról és az Ön elvárásainak</span><span class="sxs-lookup"><span data-stu-id="bc6ec-209">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="bc6ec-210">Az ügyfelek a szerepköralapú hozzáférés-telepítő</span><span class="sxs-lookup"><span data-stu-id="bc6ec-210">Setup role-based access for clients</span></span>](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


