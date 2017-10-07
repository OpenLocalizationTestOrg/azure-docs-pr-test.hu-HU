---
title: "az Azure-erőforrások aaaSet deploymentorder |} Microsoft Docs"
description: "Ismerteti, hogyan tooset egy erőforrást, erőforrástól függ másik során telepítési tooensure erőforrásai hello megfelelő sorrendben legyenek telepítve."
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
ms.openlocfilehash: 2f658f4c85236966c46b34a65aafb8426c92806c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="define-hello-order-for-deploying-resources-in-azure-resource-manager-templates"></a><span data-ttu-id="e0f36-103">Hello ahhoz, hogy az Azure Resource Manager sablonokban erőforrásokat üzembe helyezi meghatározása</span><span class="sxs-lookup"><span data-stu-id="e0f36-103">Define hello order for deploying resources in Azure Resource Manager templates</span></span>
<span data-ttu-id="e0f36-104">A megadott erőforrás lehet más erőforrások, amelyek már léteznie kell hello erőforrás van telepítve.</span><span class="sxs-lookup"><span data-stu-id="e0f36-104">For a given resource, there can be other resources that must exist before hello resource is deployed.</span></span> <span data-ttu-id="e0f36-105">Például egy SQL server léteznie kell egy SQL-adatbázis toodeploy megkísérlése előtt.</span><span class="sxs-lookup"><span data-stu-id="e0f36-105">For example, a SQL server must exist before attempting toodeploy a SQL database.</span></span> <span data-ttu-id="e0f36-106">Megadhatja a kapcsolat hello függenek, egy erőforrás megjelölésével egyéb erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e0f36-106">You define this relationship by marking one resource as dependent on hello other resource.</span></span> <span data-ttu-id="e0f36-107">Megadhatja a hello függőség **dependsOn** elem, vagy hello segítségével **hivatkozás** függvény.</span><span class="sxs-lookup"><span data-stu-id="e0f36-107">You define a dependency with hello **dependsOn** element, or by using hello **reference** function.</span></span> 

<span data-ttu-id="e0f36-108">Erőforrás-kezelő erőforrások közti függőségeket hello kiértékeli, és telepíti azokat a függő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="e0f36-108">Resource Manager evaluates hello dependencies between resources, and deploys them in their dependent order.</span></span> <span data-ttu-id="e0f36-109">Ha nincsenek függő erőforrások, erőforrás-kezelő telepíti azokat párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="e0f36-109">When resources are not dependent on each other, Resource Manager deploys them in parallel.</span></span> <span data-ttu-id="e0f36-110">Elegendő toodefine függőségek hello üzembe helyezett erőforrások ugyanazt a sablont.</span><span class="sxs-lookup"><span data-stu-id="e0f36-110">You only need toodefine dependencies for resources that are deployed in hello same template.</span></span> 

## <a name="dependson"></a><span data-ttu-id="e0f36-111">dependsOn</span><span class="sxs-lookup"><span data-stu-id="e0f36-111">dependsOn</span></span>
<span data-ttu-id="e0f36-112">A sablonon belül hello dependsOn elem lehetővé teszi toodefine egy erőforrást, egy vagy több erőforrást egy függ.</span><span class="sxs-lookup"><span data-stu-id="e0f36-112">Within your template, hello dependsOn element enables you toodefine one resource as a dependent on one or more resources.</span></span> <span data-ttu-id="e0f36-113">Az érték lehet erőforrásnevek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="e0f36-113">Its value can be a comma-separated list of resource names.</span></span> 

<span data-ttu-id="e0f36-114">hello következő példa bemutatja a virtuálisgép-méretezési csoport, amely egy adott terheléselosztóhoz, a virtuális hálózati és a több tárfiókot létrehozó hurkot függ.</span><span class="sxs-lookup"><span data-stu-id="e0f36-114">hello following example shows a virtual machine scale set that depends on a load balancer, virtual network, and a loop that creates multiple storage accounts.</span></span> <span data-ttu-id="e0f36-115">Ezeket az erőforrásokat a következő példa hello nem láthatók, de tooexist máshol hello sablonban kell.</span><span class="sxs-lookup"><span data-stu-id="e0f36-115">These other resources are not shown in hello following example, but they would need tooexist elsewhere in hello template.</span></span>

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

<span data-ttu-id="e0f36-116">A fenti példa hello, egy függőségi szerepel-e a másolási ciklust nevű keresztül létrehozott hello erőforrások **storageLoop**.</span><span class="sxs-lookup"><span data-stu-id="e0f36-116">In hello preceding example, a dependency is included on hello resources that are created through a copy loop named **storageLoop**.</span></span> <span data-ttu-id="e0f36-117">Egy vonatkozó példáért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="e0f36-117">For an example, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<span data-ttu-id="e0f36-118">Függőségek meghatározásakor hello erőforrás szolgáltató névtér és az erőforrás típusa tooavoid kétértelműség is megadhat.</span><span class="sxs-lookup"><span data-stu-id="e0f36-118">When defining dependencies, you can include hello resource provider namespace and resource type tooavoid ambiguity.</span></span> <span data-ttu-id="e0f36-119">Például tooclarify egy terheléselosztó és a virtuális hálózat, amelyek azonos nevek egyéb erőforrásokat, a következő használatát hello hello formázása:</span><span class="sxs-lookup"><span data-stu-id="e0f36-119">For example, tooclarify a load balancer and virtual network that may have hello same names as other resources, use hello following format:</span></span>

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

<span data-ttu-id="e0f36-120">Ferde toouse dependsOn toomap kapcsolatok lehet, ha az erőforrások között, de napjainkban fontos toounderstand miért végzett azt.</span><span class="sxs-lookup"><span data-stu-id="e0f36-120">While you may be inclined toouse dependsOn toomap relationships between your resources, it's important toounderstand why you're doing it.</span></span> <span data-ttu-id="e0f36-121">Például, milyen erőforrásokat kötik, toodocument dependsOn nincs hello megfelelő módszert.</span><span class="sxs-lookup"><span data-stu-id="e0f36-121">For example, toodocument how resources are interconnected, dependsOn is not hello right approach.</span></span> <span data-ttu-id="e0f36-122">Nem lehet lekérdezni, mely erőforrásokhoz a telepítés utáni hello dependsOn elemben definiált.</span><span class="sxs-lookup"><span data-stu-id="e0f36-122">You cannot query which resources were defined in hello dependsOn element after deployment.</span></span> <span data-ttu-id="e0f36-123">DependsOn tulajdonság használatával, potenciálisan hatással van az telepítési idő mert erőforrás-kezelő nem telepíti a függőségi viszonyban párhuzamos két erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e0f36-123">By using dependsOn, you potentially impact deployment time because Resource Manager does not deploy in parallel two resources that have a dependency.</span></span> <span data-ttu-id="e0f36-124">erőforrások toodocument kapcsolatai inkább [erőforrás linking](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="e0f36-124">toodocument relationships between resources, instead use [resource linking](/rest/api/resources/resourcelinks).</span></span>

## <a name="child-resources"></a><span data-ttu-id="e0f36-125">Gyermek-erőforrások</span><span class="sxs-lookup"><span data-stu-id="e0f36-125">Child resources</span></span>
<span data-ttu-id="e0f36-126">hello erőforrások tulajdonság teszi lehetővé, amelyek a múltbeli kapcsolódó toohello erőforrás toospecify gyermekerőforrásait.</span><span class="sxs-lookup"><span data-stu-id="e0f36-126">hello resources property allows you toospecify child resources that are related toohello resource being defined.</span></span> <span data-ttu-id="e0f36-127">Gyermek erőforrások csak meghatározott öt szintnél mélyebb lehet.</span><span class="sxs-lookup"><span data-stu-id="e0f36-127">Child resources can only be defined five levels deep.</span></span> <span data-ttu-id="e0f36-128">Fontos toonote, amely egy implicit függőség nem hoz létre egy gyermek és hello szülő típusú erőforrást között.</span><span class="sxs-lookup"><span data-stu-id="e0f36-128">It is important toonote that an implicit dependency is not created between a child resource and hello parent resource.</span></span> <span data-ttu-id="e0f36-129">Gyermek-erőforrás toobe hello szülő erőforrás után kell hello, hello dependsOn tulajdonság függőséget explicit módon meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="e0f36-129">If you need hello child resource toobe deployed after hello parent resource, you must explicitly state that dependency with hello dependsOn property.</span></span> 

<span data-ttu-id="e0f36-130">Minden szülő erőforrás csak bizonyos típusú erőforrások gyermekerőforrásait fogad el.</span><span class="sxs-lookup"><span data-stu-id="e0f36-130">Each parent resource accepts only certain resource types as child resources.</span></span> <span data-ttu-id="e0f36-131">hello elfogadott erőforrástípusok esetében meg van határozva a hello [sablonséma](https://github.com/Azure/azure-resource-manager-schemas) hello szülő erőforrás.</span><span class="sxs-lookup"><span data-stu-id="e0f36-131">hello accepted resource types are specified in hello [template schema](https://github.com/Azure/azure-resource-manager-schemas) of hello parent resource.</span></span> <span data-ttu-id="e0f36-132">gyermek erőforrástípus neve hello tartalmaz hello neve hello szülő erőforrástípusra, például **Microsoft.Web/sites/config** és **Microsoft.Web/sites/extensions** mindkét hello agyermek-erőforrás **Microsoft.Web/sites**.</span><span class="sxs-lookup"><span data-stu-id="e0f36-132">hello name of child resource type includes hello name of hello parent resource type, such as **Microsoft.Web/sites/config** and **Microsoft.Web/sites/extensions** are both child resources of hello **Microsoft.Web/sites**.</span></span>

<span data-ttu-id="e0f36-133">a következő példa hello jeleníti meg, egy SQL server és SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e0f36-133">hello following example shows a SQL server and SQL database.</span></span> <span data-ttu-id="e0f36-134">Figyelje meg, hogy egy explicit függőség hello SQL database és SQL server között van-e definiálva annak ellenére, hogy hello adatbázis hello server gyermeke.</span><span class="sxs-lookup"><span data-stu-id="e0f36-134">Notice that an explicit dependency is defined between hello SQL database and SQL server, even though hello database is a child of hello server.</span></span>

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

## <a name="reference-function"></a><span data-ttu-id="e0f36-135">hivatkozás függvény</span><span class="sxs-lookup"><span data-stu-id="e0f36-135">reference function</span></span>
<span data-ttu-id="e0f36-136">Hello [függvényre](resource-group-template-functions-resource.md#reference) lehetővé teszi egy kifejezés tooderive más JSON név-érték párok vagy futásidejű erőforrások az értékét.</span><span class="sxs-lookup"><span data-stu-id="e0f36-136">hello [reference function](resource-group-template-functions-resource.md#reference) enables an expression tooderive its value from other JSON name and value pairs or runtime resources.</span></span> <span data-ttu-id="e0f36-137">Hivatkozási kifejezések implicit módon deklarálja, hogy egy erőforrást egy másik függ.</span><span class="sxs-lookup"><span data-stu-id="e0f36-137">Reference expressions implicitly declare that one resource depends on another.</span></span> <span data-ttu-id="e0f36-138">hello általános formátuma:</span><span class="sxs-lookup"><span data-stu-id="e0f36-138">hello general format is:</span></span>

```json
reference('resourceName').propertyPath
```

<span data-ttu-id="e0f36-139">A következő példa hello a CDN-végpontok explicit módon függ hello CDN-profilt, és a implicit módon függ a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e0f36-139">In hello following example, a CDN endpoint explicitly depends on hello CDN profile, and implicitly depends on a web app.</span></span>

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

<span data-ttu-id="e0f36-140">Ezen elem vagy a hello dependsOn elem toospecify függőségeket is használhat, de nem kell mindkét toouse hello az azonos függő erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e0f36-140">You can use either this element or hello dependsOn element toospecify dependencies, but you do not need toouse both for hello same dependent resource.</span></span> <span data-ttu-id="e0f36-141">Amikor csak lehetséges, használjon egy implicit hivatkozási tooavoid szükségtelen-függőség felvétele.</span><span class="sxs-lookup"><span data-stu-id="e0f36-141">Whenever possible, use an implicit reference tooavoid adding an unnecessary dependency.</span></span>

<span data-ttu-id="e0f36-142">több, lásd: toolearn [függvényre](resource-group-template-functions-resource.md#reference).</span><span class="sxs-lookup"><span data-stu-id="e0f36-142">toolearn more, see [reference function](resource-group-template-functions-resource.md#reference).</span></span>

## <a name="recommendations-for-setting-dependencies"></a><span data-ttu-id="e0f36-143">Javaslatok függőségek beállítása</span><span class="sxs-lookup"><span data-stu-id="e0f36-143">Recommendations for setting dependencies</span></span>

<span data-ttu-id="e0f36-144">Amikor eldönti, milyen függőségek tooset, használja a következő irányelveket hello:</span><span class="sxs-lookup"><span data-stu-id="e0f36-144">When deciding what dependencies tooset, use hello following guidelines:</span></span>

* <span data-ttu-id="e0f36-145">A lehető legkevesebb függőségek beállítása.</span><span class="sxs-lookup"><span data-stu-id="e0f36-145">Set as few dependencies as possible.</span></span>
* <span data-ttu-id="e0f36-146">Állítsa be a gyermek-erőforrás a szülő erőforrás függ.</span><span class="sxs-lookup"><span data-stu-id="e0f36-146">Set a child resource as dependent on its parent resource.</span></span>
* <span data-ttu-id="e0f36-147">Használjon hello **hivatkozás** tooset tooshare tulajdonság szükséges erőforrások közötti implicit függőségek működik.</span><span class="sxs-lookup"><span data-stu-id="e0f36-147">Use hello **reference** function tooset implicit dependencies between resources that need tooshare a property.</span></span> <span data-ttu-id="e0f36-148">Ne vegyen fel egy explicit függőség (**dependsOn**) Ha már megadta egy implicit függőség.</span><span class="sxs-lookup"><span data-stu-id="e0f36-148">Do not add an explicit dependency (**dependsOn**) when you have already defined an implicit dependency.</span></span> <span data-ttu-id="e0f36-149">Ez a megközelítés hello kockázatot szükségtelen függőségek csökkenti.</span><span class="sxs-lookup"><span data-stu-id="e0f36-149">This approach reduces hello risk of having unnecessary dependencies.</span></span> 
* <span data-ttu-id="e0f36-150">Amikor az erőforrás nem lehet a függőség beállítása **létrehozott** nélkül egy másik erőforrás funkciói.</span><span class="sxs-lookup"><span data-stu-id="e0f36-150">Set a dependency when a resource cannot be **created** without functionality from another resource.</span></span> <span data-ttu-id="e0f36-151">Ne állítson be egy függőséget hello erőforrások csak interaktív telepítés után.</span><span class="sxs-lookup"><span data-stu-id="e0f36-151">Do not set a dependency if hello resources only interact after deployment.</span></span>
* <span data-ttu-id="e0f36-152">Lehetővé függőségek cascade explicit módon beállítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="e0f36-152">Let dependencies cascade without setting them explicitly.</span></span> <span data-ttu-id="e0f36-153">Például a virtuális gép virtuális hálózati adapteren, valamint hello virtuális hálózati adapter függ a virtuális hálózat és a nyilvános IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="e0f36-153">For example, your virtual machine depends on a virtual network interface, and hello virtual network interface depends on a virtual network and public IP addresses.</span></span> <span data-ttu-id="e0f36-154">Ezért hello virtuális gép telepített összes három erőforrások, de nincs explicit módon beállítva az hello virtuális gép összes három erőforrástól függ.</span><span class="sxs-lookup"><span data-stu-id="e0f36-154">Therefore, hello virtual machine is deployed after all three resources, but do not explicitly set hello virtual machine as dependent on all three resources.</span></span> <span data-ttu-id="e0f36-155">Ez a megközelítés hello függőségi sorrend tisztázza, így könnyebben toochange hello sablon később.</span><span class="sxs-lookup"><span data-stu-id="e0f36-155">This approach clarifies hello dependency order and makes it easier toochange hello template later.</span></span>
* <span data-ttu-id="e0f36-156">Ha a telepítés előtt értéket lehet meghatározni, próbálja hello erőforrás függőség nélküli telepítése.</span><span class="sxs-lookup"><span data-stu-id="e0f36-156">If a value can be determined before deployment, try deploying hello resource without a dependency.</span></span> <span data-ttu-id="e0f36-157">Például ha egy konfigurációs értéke egy másik erőforrás hello nevét, nem szükség lehet egy függőséget.</span><span class="sxs-lookup"><span data-stu-id="e0f36-157">For example, if a configuration value needs hello name of another resource, you might not need a dependency.</span></span> <span data-ttu-id="e0f36-158">Ez az útmutató nem mindig működik, mert egyes erőforrásokat hello hello létezésének ellenőrzése egyéb erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e0f36-158">This guidance does not always work because some resources verify hello existence of hello other resource.</span></span> <span data-ttu-id="e0f36-159">Ha hibaüzenetet kap, hozzáadjon egy függőséget.</span><span class="sxs-lookup"><span data-stu-id="e0f36-159">If you receive an error, add a dependency.</span></span> 

<span data-ttu-id="e0f36-160">Erőforrás-kezelő körkörös függőségi viszony azonosítja a sablon érvényesítése során.</span><span class="sxs-lookup"><span data-stu-id="e0f36-160">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="e0f36-161">Ha hibaüzenet jelenik meg, amely meghatározza, hogy, hogy létezik-e körkörös függőséget, a sablon toosee kiértékelheti, hogy bármely függőségek esetén nem szükséges, és távolíthatja el.</span><span class="sxs-lookup"><span data-stu-id="e0f36-161">If you receive an error stating that a circular dependency exists, evaluate your template toosee if any dependencies are not needed and can be removed.</span></span> <span data-ttu-id="e0f36-162">Ha függőségek nem segít, elkerülhető körkörös függőségi viszony, néhány telepítési művelet áthelyezése gyermekszintű erőforrása, amely után hello erőforrásokat, amelyek hello körkörös függőségi viszonyban vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="e0f36-162">If removing dependencies does not work, you can avoid circular dependencies by moving some deployment operations into child resources that are deployed after hello resources that have hello circular dependency.</span></span> <span data-ttu-id="e0f36-163">Tegyük fel például, két olyan virtuális gépet telepít, de a tulajdonságok az egyes más toohello hivatkozó meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="e0f36-163">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer toohello other.</span></span> <span data-ttu-id="e0f36-164">A sorrend hello telepíthetné őket:</span><span class="sxs-lookup"><span data-stu-id="e0f36-164">You can deploy them in hello following order:</span></span>

1. <span data-ttu-id="e0f36-165">vm1</span><span class="sxs-lookup"><span data-stu-id="e0f36-165">vm1</span></span>
2. <span data-ttu-id="e0f36-166">vm2 virtuális gépnek</span><span class="sxs-lookup"><span data-stu-id="e0f36-166">vm2</span></span>
3. <span data-ttu-id="e0f36-167">Vm1 kiterjesztés vm1 és vm2 virtuális gépnek függ.</span><span class="sxs-lookup"><span data-stu-id="e0f36-167">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="e0f36-168">hello bővítmény, amely azt lekérése vm2 virtuális gépnek vm1 értékek beállítása.</span><span class="sxs-lookup"><span data-stu-id="e0f36-168">hello extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="e0f36-169">Bővítmény vm2 virtuális gépnek a vm1 és vm2 virtuális gépnek függ.</span><span class="sxs-lookup"><span data-stu-id="e0f36-169">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="e0f36-170">hello bővítmény értékek beállítása, amely azt lekérése vm1 vm2 virtuális gépnek.</span><span class="sxs-lookup"><span data-stu-id="e0f36-170">hello extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="e0f36-171">Felmérése hello telepítési sorrendje: és függőségi hibák feloldására vonatkozó információkért lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="e0f36-171">For information about assessing hello deployment order and resolving dependency errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0f36-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0f36-172">Next steps</span></span>
* <span data-ttu-id="e0f36-173">a telepítés során függőségek hibaelhárítással kapcsolatos toolearn lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="e0f36-173">toolearn about troubleshooting dependencies during deployment, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="e0f36-174">toolearn, Azure Resource Manager-sablonok létrehozásával kapcsolatban lásd: [sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e0f36-174">toolearn about creating Azure Resource Manager templates, see [Authoring templates](resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="e0f36-175">Hello elérhető funkciók egy listáját lásd: [sablonfüggvényei](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="e0f36-175">For a list of hello available functions in a template, see [Template functions](resource-group-template-functions.md).</span></span>

