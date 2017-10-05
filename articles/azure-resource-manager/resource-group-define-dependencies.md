---
title: "Állítsa be a telepítési sorrendet, az Azure-erőforrások |} Microsoft Docs"
description: "Útmutatás állítja be egy erőforrást egy másik erőforrástól függő erőforrások a megfelelő sorrendben legyenek telepítve. Ellenőrizze a telepítés során."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: tomfitz
ms.openlocfilehash: 3d6a46116ae9d7d940bc10dfa832540f42c0af7e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="define-the-order-for-deploying-resources-in-azure-resource-manager-templates"></a><span data-ttu-id="857a7-103">Adja meg a ahhoz, hogy az Azure Resource Manager sablonokban erőforrások telepítése</span><span class="sxs-lookup"><span data-stu-id="857a7-103">Define the order for deploying resources in Azure Resource Manager templates</span></span>
<span data-ttu-id="857a7-104">A megadott erőforrás lehet más erőforrások, amelyek már léteznie kell az erőforrás van telepítve.</span><span class="sxs-lookup"><span data-stu-id="857a7-104">For a given resource, there can be other resources that must exist before the resource is deployed.</span></span> <span data-ttu-id="857a7-105">Például egy SQL server léteznie kell egy SQL-adatbázis telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="857a7-105">For example, a SQL server must exist before attempting to deploy a SQL database.</span></span> <span data-ttu-id="857a7-106">Ez a kapcsolat egy erőforrást, mint a más erőforrástól függő megjelölésével határozza meg.</span><span class="sxs-lookup"><span data-stu-id="857a7-106">You define this relationship by marking one resource as dependent on the other resource.</span></span> <span data-ttu-id="857a7-107">A függőség megadhatja a **dependsOn** elem, vagy használatával a **hivatkozás** függvény.</span><span class="sxs-lookup"><span data-stu-id="857a7-107">You define a dependency with the **dependsOn** element, or by using the **reference** function.</span></span> 

<span data-ttu-id="857a7-108">Erőforrás-kezelő kiértékeli az erőforrások közti függőségeket, és telepíti azokat a függő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="857a7-108">Resource Manager evaluates the dependencies between resources, and deploys them in their dependent order.</span></span> <span data-ttu-id="857a7-109">Ha nincsenek függő erőforrások, erőforrás-kezelő telepíti azokat párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="857a7-109">When resources are not dependent on each other, Resource Manager deploys them in parallel.</span></span> <span data-ttu-id="857a7-110">Csak akkor kell megadni telepített erőforrások függőségek ugyanabban a sablonban.</span><span class="sxs-lookup"><span data-stu-id="857a7-110">You only need to define dependencies for resources that are deployed in the same template.</span></span> 

## <a name="dependson"></a><span data-ttu-id="857a7-111">dependsOn</span><span class="sxs-lookup"><span data-stu-id="857a7-111">dependsOn</span></span>
<span data-ttu-id="857a7-112">A sablonon belül a dependsOn elem teszi meghatározni egy erőforrást, egy vagy több erőforrást egy függ.</span><span class="sxs-lookup"><span data-stu-id="857a7-112">Within your template, the dependsOn element enables you to define one resource as a dependent on one or more resources.</span></span> <span data-ttu-id="857a7-113">Az érték lehet erőforrásnevek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="857a7-113">Its value can be a comma-separated list of resource names.</span></span> 

<span data-ttu-id="857a7-114">A következő példában egy virtuálisgép-méretezési csoport, amely egy adott terheléselosztóhoz, a virtuális hálózati és a több tárfiókot létrehozó hurkot függ.</span><span class="sxs-lookup"><span data-stu-id="857a7-114">The following example shows a virtual machine scale set that depends on a load balancer, virtual network, and a loop that creates multiple storage accounts.</span></span> <span data-ttu-id="857a7-115">Ezeket az erőforrásokat nem jelennek meg az alábbi példában, de a sablonban máshol léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="857a7-115">These other resources are not shown in the following example, but they would need to exist elsewhere in the template.</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "apiVersion": "2016-03-30",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

<span data-ttu-id="857a7-116">A fenti példában egy függőséget szerepel-e az erőforrásokat, a másolási ciklust nevű keresztül létrehozott **storageLoop**.</span><span class="sxs-lookup"><span data-stu-id="857a7-116">In the preceding example, a dependency is included on the resources that are created through a copy loop named **storageLoop**.</span></span> <span data-ttu-id="857a7-117">Egy vonatkozó példáért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="857a7-117">For an example, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<span data-ttu-id="857a7-118">Függőségek meghatározásakor az erőforrás-szolgáltató névtere és a félreérthetőség elkerülése érdekében erőforrástípus lehetnek.</span><span class="sxs-lookup"><span data-stu-id="857a7-118">When defining dependencies, you can include the resource provider namespace and resource type to avoid ambiguity.</span></span> <span data-ttu-id="857a7-119">Elmagyarázza, a terheléselosztó és a virtuális hálózat, amelyek az azonos nevű más erőforrások, például a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="857a7-119">For example, to clarify a load balancer and virtual network that may have the same names as other resources, use the following format:</span></span>

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

<span data-ttu-id="857a7-120">Akkor lehet, hogy az erőforrások közötti kapcsolatok DependsOn tulajdonság használatával kell dönteni, de napjainkban fontos tudni, miért végzett azt.</span><span class="sxs-lookup"><span data-stu-id="857a7-120">While you may be inclined to use dependsOn to map relationships between your resources, it's important to understand why you're doing it.</span></span> <span data-ttu-id="857a7-121">Például hogyan erőforrásokat kötik dokumentum dependsOn nincs a megfelelő módszert.</span><span class="sxs-lookup"><span data-stu-id="857a7-121">For example, to document how resources are interconnected, dependsOn is not the right approach.</span></span> <span data-ttu-id="857a7-122">Nem lehet lekérdezni, mely erőforrásokat telepítést követően a DependsOn tulajdonság elemben definiált.</span><span class="sxs-lookup"><span data-stu-id="857a7-122">You cannot query which resources were defined in the dependsOn element after deployment.</span></span> <span data-ttu-id="857a7-123">DependsOn tulajdonság használatával, potenciálisan hatással van az telepítési idő mert erőforrás-kezelő nem telepíti a függőségi viszonyban párhuzamos két erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="857a7-123">By using dependsOn, you potentially impact deployment time because Resource Manager does not deploy in parallel two resources that have a dependency.</span></span> <span data-ttu-id="857a7-124">A dokumentum erőforrásainak kapcsolatai helyette használja a [erőforrás linking](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="857a7-124">To document relationships between resources, instead use [resource linking](/rest/api/resources/resourcelinks).</span></span>

## <a name="child-resources"></a><span data-ttu-id="857a7-125">Gyermek-erőforrások</span><span class="sxs-lookup"><span data-stu-id="857a7-125">Child resources</span></span>
<span data-ttu-id="857a7-126">Az erőforrás-tulajdonság adja meg, amely kapcsolódik a múltbeli erőforrás gyermekerőforrásait teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="857a7-126">The resources property allows you to specify child resources that are related to the resource being defined.</span></span> <span data-ttu-id="857a7-127">Gyermek erőforrások csak meghatározott öt szintnél mélyebb lehet.</span><span class="sxs-lookup"><span data-stu-id="857a7-127">Child resources can only be defined five levels deep.</span></span> <span data-ttu-id="857a7-128">Fontos megjegyezni, hogy egy implicit függőség nem jön létre a gyermek-erőforrás és a szülő erőforrás között.</span><span class="sxs-lookup"><span data-stu-id="857a7-128">It is important to note that an implicit dependency is not created between a child resource and the parent resource.</span></span> <span data-ttu-id="857a7-129">A gyermek-erőforrás a szülő erőforrás után telepítésre van szüksége, a dependsOn tulajdonság függőséget explicit módon meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="857a7-129">If you need the child resource to be deployed after the parent resource, you must explicitly state that dependency with the dependsOn property.</span></span> 

<span data-ttu-id="857a7-130">Minden szülő erőforrás csak bizonyos típusú erőforrások gyermekerőforrásait fogad el.</span><span class="sxs-lookup"><span data-stu-id="857a7-130">Each parent resource accepts only certain resource types as child resources.</span></span> <span data-ttu-id="857a7-131">Az elfogadott erőforrás típusok vannak megadva a [sablonséma](https://github.com/Azure/azure-resource-manager-schemas) a szülő erőforrás.</span><span class="sxs-lookup"><span data-stu-id="857a7-131">The accepted resource types are specified in the [template schema](https://github.com/Azure/azure-resource-manager-schemas) of the parent resource.</span></span> <span data-ttu-id="857a7-132">A gyermek erőforrástípus neve tartalmazza a szülő erőforrástípus nevét, mint **Microsoft.Web/sites/config** és **Microsoft.Web/sites/extensions** mindkét alsóbb szintű erőforrásai vannak a **Microsoft.Web/sites**.</span><span class="sxs-lookup"><span data-stu-id="857a7-132">The name of child resource type includes the name of the parent resource type, such as **Microsoft.Web/sites/config** and **Microsoft.Web/sites/extensions** are both child resources of the **Microsoft.Web/sites**.</span></span>

<span data-ttu-id="857a7-133">A következő példában egy SQL server és SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="857a7-133">The following example shows a SQL server and SQL database.</span></span> <span data-ttu-id="857a7-134">Figyelje meg, hogy egy explicit függőség között nincs definiálva a SQL database és SQL server, annak ellenére, hogy az adatbázis egy alárendelt kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="857a7-134">Notice that an explicit dependency is defined between the SQL database and SQL server, even though the database is a child of the server.</span></span>

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "apiVersion": "2014-04-01-preview",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "Database"
        },
        "apiVersion": "2014-04-01-preview",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-function"></a><span data-ttu-id="857a7-135">hivatkozás függvény</span><span class="sxs-lookup"><span data-stu-id="857a7-135">reference function</span></span>
<span data-ttu-id="857a7-136">A [függvényre](resource-group-template-functions-resource.md#reference) lehetővé teszi, hogy a kifejezés értéke származik más JSON név-érték párok vagy futásidejű erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="857a7-136">The [reference function](resource-group-template-functions-resource.md#reference) enables an expression to derive its value from other JSON name and value pairs or runtime resources.</span></span> <span data-ttu-id="857a7-137">Hivatkozási kifejezések implicit módon deklarálja, hogy egy erőforrást egy másik függ.</span><span class="sxs-lookup"><span data-stu-id="857a7-137">Reference expressions implicitly declare that one resource depends on another.</span></span> <span data-ttu-id="857a7-138">Az általános formátuma:</span><span class="sxs-lookup"><span data-stu-id="857a7-138">The general format is:</span></span>

```json
reference('resourceName').propertyPath
```

<span data-ttu-id="857a7-139">A következő példában a CDN-végpontok explicit módon függ a CDN-profilt, és implicit módon függ a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="857a7-139">In the following example, a CDN endpoint explicitly depends on the CDN profile, and implicitly depends on a web app.</span></span>

```json
{
    "name": "[variables('endpointName')]",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "apiVersion": "2016-04-02",
    "dependsOn": [
            "[variables('profileName')]"
    ],
    "properties": {
        "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
        ...
    }
```

<span data-ttu-id="857a7-140">Függőségeket használhatja ezt az elemet vagy a dependsOn elem, de nem szeretné használni, mind az azonos függő erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="857a7-140">You can use either this element or the dependsOn element to specify dependencies, but you do not need to use both for the same dependent resource.</span></span> <span data-ttu-id="857a7-141">Amikor csak lehetséges, használjon egy implicit hivatkozási szükségtelen-függőség felvétele elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="857a7-141">Whenever possible, use an implicit reference to avoid adding an unnecessary dependency.</span></span>

<span data-ttu-id="857a7-142">További tudnivalókért lásd: [függvényre](resource-group-template-functions-resource.md#reference).</span><span class="sxs-lookup"><span data-stu-id="857a7-142">To learn more, see [reference function](resource-group-template-functions-resource.md#reference).</span></span>

## <a name="recommendations-for-setting-dependencies"></a><span data-ttu-id="857a7-143">Javaslatok függőségek beállítása</span><span class="sxs-lookup"><span data-stu-id="857a7-143">Recommendations for setting dependencies</span></span>

<span data-ttu-id="857a7-144">Amikor eldönti, milyen függőségek beállításához, kövesse az alábbi iránymutatásokat:</span><span class="sxs-lookup"><span data-stu-id="857a7-144">When deciding what dependencies to set, use the following guidelines:</span></span>

* <span data-ttu-id="857a7-145">A lehető legkevesebb függőségek beállítása.</span><span class="sxs-lookup"><span data-stu-id="857a7-145">Set as few dependencies as possible.</span></span>
* <span data-ttu-id="857a7-146">Állítsa be a gyermek-erőforrás a szülő erőforrás függ.</span><span class="sxs-lookup"><span data-stu-id="857a7-146">Set a child resource as dependent on its parent resource.</span></span>
* <span data-ttu-id="857a7-147">Használja a **hivatkozás** függvényt kell megosztani egy tulajdonság erőforrások közötti implicit függőségek beállítása.</span><span class="sxs-lookup"><span data-stu-id="857a7-147">Use the **reference** function to set implicit dependencies between resources that need to share a property.</span></span> <span data-ttu-id="857a7-148">Ne vegyen fel egy explicit függőség (**dependsOn**) Ha már megadta egy implicit függőség.</span><span class="sxs-lookup"><span data-stu-id="857a7-148">Do not add an explicit dependency (**dependsOn**) when you have already defined an implicit dependency.</span></span> <span data-ttu-id="857a7-149">Ez a megközelítés csökkenti a szükségtelen függőségek kockázatot.</span><span class="sxs-lookup"><span data-stu-id="857a7-149">This approach reduces the risk of having unnecessary dependencies.</span></span> 
* <span data-ttu-id="857a7-150">Amikor az erőforrás nem lehet a függőség beállítása **létrehozott** nélkül egy másik erőforrás funkciói.</span><span class="sxs-lookup"><span data-stu-id="857a7-150">Set a dependency when a resource cannot be **created** without functionality from another resource.</span></span> <span data-ttu-id="857a7-151">Ne állítson be egy függőséget az erőforrások csak interaktív telepítés után.</span><span class="sxs-lookup"><span data-stu-id="857a7-151">Do not set a dependency if the resources only interact after deployment.</span></span>
* <span data-ttu-id="857a7-152">Lehetővé függőségek cascade explicit módon beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="857a7-152">Let dependencies cascade without setting them explicitly.</span></span> <span data-ttu-id="857a7-153">Például a virtuális gép virtuális hálózati adapteren, valamint a virtuális hálózati adapter függ a virtuális hálózat és a nyilvános IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="857a7-153">For example, your virtual machine depends on a virtual network interface, and the virtual network interface depends on a virtual network and public IP addresses.</span></span> <span data-ttu-id="857a7-154">Ezért a virtuális gép telepített összes három erőforrások, de nincs explicit módon beállítva a virtuális gép összes három erőforrástól függ.</span><span class="sxs-lookup"><span data-stu-id="857a7-154">Therefore, the virtual machine is deployed after all three resources, but do not explicitly set the virtual machine as dependent on all three resources.</span></span> <span data-ttu-id="857a7-155">Ez a megközelítés a függőségi sorrend tisztázza, és megkönnyíti a sablon később módosítható.</span><span class="sxs-lookup"><span data-stu-id="857a7-155">This approach clarifies the dependency order and makes it easier to change the template later.</span></span>
* <span data-ttu-id="857a7-156">Ha a telepítés előtt értéket lehet meghatározni, próbálja üzembe helyezni a függőség nélküli erőforrás.</span><span class="sxs-lookup"><span data-stu-id="857a7-156">If a value can be determined before deployment, try deploying the resource without a dependency.</span></span> <span data-ttu-id="857a7-157">Például ha egy konfigurációs értéke egy másik erőforrás nevét, nem szükség lehet egy függőséget.</span><span class="sxs-lookup"><span data-stu-id="857a7-157">For example, if a configuration value needs the name of another resource, you might not need a dependency.</span></span> <span data-ttu-id="857a7-158">Ez az útmutató nem mindig működik, mert egyes erőforrásokat ellenőriznie az egyéb erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="857a7-158">This guidance does not always work because some resources verify the existence of the other resource.</span></span> <span data-ttu-id="857a7-159">Ha hibaüzenetet kap, hozzáadjon egy függőséget.</span><span class="sxs-lookup"><span data-stu-id="857a7-159">If you receive an error, add a dependency.</span></span> 

<span data-ttu-id="857a7-160">Erőforrás-kezelő körkörös függőségi viszony azonosítja a sablon érvényesítése során.</span><span class="sxs-lookup"><span data-stu-id="857a7-160">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="857a7-161">Ha hibaüzenet jelenik meg, amely meghatározza, hogy, hogy létezik-e körkörös függőséget, értékelje ki, hogy összes függőséget nem szükségesek, és el kell távolítani a sablont.</span><span class="sxs-lookup"><span data-stu-id="857a7-161">If you receive an error stating that a circular dependency exists, evaluate your template to see if any dependencies are not needed and can be removed.</span></span> <span data-ttu-id="857a7-162">Ha függőségek nem segít, elkerülhető körkörös függőségi viszony, néhány telepítési művelet áthelyezése gyermekszintű erőforrása, amely után az erőforrásokat, amelyek a körkörös függőségi viszonyban vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="857a7-162">If removing dependencies does not work, you can avoid circular dependencies by moving some deployment operations into child resources that are deployed after the resources that have the circular dependency.</span></span> <span data-ttu-id="857a7-163">Tegyük fel például, két olyan virtuális gépet telepít, de a tulajdonságokat meg kell adni az egyes, amely a másikra hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="857a7-163">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer to the other.</span></span> <span data-ttu-id="857a7-164">A következő sorrendben telepíthetők:</span><span class="sxs-lookup"><span data-stu-id="857a7-164">You can deploy them in the following order:</span></span>

1. <span data-ttu-id="857a7-165">vm1</span><span class="sxs-lookup"><span data-stu-id="857a7-165">vm1</span></span>
2. <span data-ttu-id="857a7-166">vm2 virtuális gépnek</span><span class="sxs-lookup"><span data-stu-id="857a7-166">vm2</span></span>
3. <span data-ttu-id="857a7-167">Vm1 kiterjesztés vm1 és vm2 virtuális gépnek függ.</span><span class="sxs-lookup"><span data-stu-id="857a7-167">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="857a7-168">A bővítmény beállítása, amely azt lekérése vm2 virtuális gépnek vm1 értékeket.</span><span class="sxs-lookup"><span data-stu-id="857a7-168">The extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="857a7-169">Bővítmény vm2 virtuális gépnek a vm1 és vm2 virtuális gépnek függ.</span><span class="sxs-lookup"><span data-stu-id="857a7-169">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="857a7-170">A bővítmény beállítása értékek, amelyek azt lekérése vm1 vm2 virtuális gépnek.</span><span class="sxs-lookup"><span data-stu-id="857a7-170">The extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="857a7-171">Értékelése a telepítési sorrendet, és a függőségi hibák feloldására vonatkozó információkért lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="857a7-171">For information about assessing the deployment order and resolving dependency errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="857a7-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="857a7-172">Next steps</span></span>
* <span data-ttu-id="857a7-173">Függőségek telepítése során hibaelhárítással kapcsolatos információkért lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="857a7-173">To learn about troubleshooting dependencies during deployment, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="857a7-174">Azure Resource Manager-sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="857a7-174">To learn about creating Azure Resource Manager templates, see [Authoring templates](resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="857a7-175">A sablon elérhető funkciókat listájáért lásd: [sablonfüggvényei](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="857a7-175">For a list of the available functions in a template, see [Template functions](resource-group-template-functions.md).</span></span>

