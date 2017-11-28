---
title: "Virtuálisgép-méretezési kapcsolatos aaaLearn sablonok beállítása |} Microsoft Docs"
description: "Ismerje meg a minimális életképes méretezési toocreate sablon virtuálisgép-méretezési csoportok beállítása"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: b7a1cf6c03b22585e16db9c071d45795c8ae75df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a><span data-ttu-id="02914-103">További tudnivalók a virtuálisgép-méretezési készlet sablonok</span><span class="sxs-lookup"><span data-stu-id="02914-103">Learn about virtual machine scale set templates</span></span>
<span data-ttu-id="02914-104">[Az Azure Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) vannak egy kiváló módja toodeploy kapcsolódó erőforrások csoportja.</span><span class="sxs-lookup"><span data-stu-id="02914-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way toodeploy groups of related resources.</span></span> <span data-ttu-id="02914-105">Az oktatóanyag adatsorozat bemutatja, hogyan toocreate egy minimális életképes méretezési sablon beállítása, és hogyan toomodify a sablon toosuit különböző lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="02914-105">This tutorial series shows how toocreate a minimum viable scale set template and how toomodify this template toosuit various scenarios.</span></span> <span data-ttu-id="02914-106">Minden példában ez származhat [GitHub-tárházban](https://github.com/gatneil/mvss).</span><span class="sxs-lookup"><span data-stu-id="02914-106">All examples come from this [GitHub repository](https://github.com/gatneil/mvss).</span></span> 

<span data-ttu-id="02914-107">Ez a sablon egyszerű tervezett toobe.</span><span class="sxs-lookup"><span data-stu-id="02914-107">This template is intended toobe simple.</span></span> <span data-ttu-id="02914-108">Skála teljesebb példákat sablonok, olvassa el hello [Azure gyors üzembe helyezési sablonokat GitHub-tárházban](https://github.com/Azure/azure-quickstart-templates) és keresési mappák hello karakterláncot tartalmazó `vmss`.</span><span class="sxs-lookup"><span data-stu-id="02914-108">For more complete examples of scale set templates, see hello [Azure Quickstart Templates GitHub repository](https://github.com/Azure/azure-quickstart-templates) and search for folders that contain hello string `vmss`.</span></span>

<span data-ttu-id="02914-109">Ha már ismeri a sablonok létrehozásával, kihagyhatja toohello "További lépések" szakasz toosee hogyan toomodify ezt a sablont.</span><span class="sxs-lookup"><span data-stu-id="02914-109">If you are already familiar with creating templates, you can skip toohello "Next steps" section toosee how toomodify this template.</span></span>

## <a name="review-hello-template"></a><span data-ttu-id="02914-110">Felülvizsgálati hello sablonja</span><span class="sxs-lookup"><span data-stu-id="02914-110">Review hello template</span></span>

<span data-ttu-id="02914-111">A GitHub használatára tooreview a minimális életképes méretezési sablon, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="02914-111">Use GitHub tooreview our minimum viable scale set template, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span></span>

<span data-ttu-id="02914-112">Az oktatóanyag azt vizsgálja meg hello diff (`git diff master minimum-viable-scale-set`) toocreate hello minimális életképes méretezési sablon adat által adat.</span><span class="sxs-lookup"><span data-stu-id="02914-112">In this tutorial, we examine hello diff (`git diff master minimum-viable-scale-set`) toocreate hello minimum viable scale set template piece by piece.</span></span>

## <a name="define-schema-and-contentversion"></a><span data-ttu-id="02914-113">$Schema és contentVersion megadása</span><span class="sxs-lookup"><span data-stu-id="02914-113">Define $schema and contentVersion</span></span>
<span data-ttu-id="02914-114">Első lépésként meghatároztuk `$schema` és `contentVersion` hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="02914-114">First, we define `$schema` and `contentVersion` in hello template.</span></span> <span data-ttu-id="02914-115">Hello `$schema` elem hello sablon nyelvi hello verziója határozza meg, és a Visual Studio szintaxis kiemelését és hasonló érvényesítési szolgáltatások használható.</span><span class="sxs-lookup"><span data-stu-id="02914-115">hello `$schema` element defines hello version of hello template language and is used for Visual Studio syntax highlighting and similar validation features.</span></span> <span data-ttu-id="02914-116">Hello `contentVersion` elem nem használja az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="02914-116">hello `contentVersion` element is not used by Azure.</span></span> <span data-ttu-id="02914-117">Ehelyett segít nyomon követjük, hello verziójának.</span><span class="sxs-lookup"><span data-stu-id="02914-117">Instead, it helps you keep track of hello template version.</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a><span data-ttu-id="02914-118">Paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="02914-118">Define parameters</span></span>
<span data-ttu-id="02914-119">A következő két paraméter meghatároztuk `adminUsername` és `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="02914-119">Next, we define two parameters, `adminUsername` and `adminPassword`.</span></span> <span data-ttu-id="02914-120">A paraméterek olyan értékek adja meg, ha a központi telepítés hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="02914-120">Parameters are values you specify at hello time of deployment.</span></span> <span data-ttu-id="02914-121">Hello `adminUsername` paraméter egyszerűen egy `string` típusa, de mivel `adminPassword` van egy titkos kulcsot, amelyben tudatjuk a felhasználókkal az típus `securestring`.</span><span class="sxs-lookup"><span data-stu-id="02914-121">hello `adminUsername` parameter is simply a `string` type, but because `adminPassword` is a secret, we give it type `securestring`.</span></span> <span data-ttu-id="02914-122">Később ezek a paraméterek vannak átadott hello méretezési készlet konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="02914-122">Later, these parameters are passed into hello scale set configuration.</span></span>

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a><span data-ttu-id="02914-123">Változók meghatározása</span><span class="sxs-lookup"><span data-stu-id="02914-123">Define variables</span></span>
<span data-ttu-id="02914-124">Resource Manager-sablonok segítségével definiálhatja a változók toobe hello sablon szerepel.</span><span class="sxs-lookup"><span data-stu-id="02914-124">Resource Manager templates also let you define variables toobe used later in hello template.</span></span> <span data-ttu-id="02914-125">A példa változó, nem használ, így a jelenleg üres már hello JSON-objektumból.</span><span class="sxs-lookup"><span data-stu-id="02914-125">Our example doesn't use any variables, so we've left hello JSON object empty.</span></span>

```json
  "variables": {},
```

## <a name="define-resources"></a><span data-ttu-id="02914-126">Adja meg az erőforrások</span><span class="sxs-lookup"><span data-stu-id="02914-126">Define resources</span></span>
<span data-ttu-id="02914-127">Ezután van hello sablon hello források szakaszában.</span><span class="sxs-lookup"><span data-stu-id="02914-127">Next is hello resources section of hello template.</span></span> <span data-ttu-id="02914-128">Itt adhat meg ténylegesen mit toodeploy.</span><span class="sxs-lookup"><span data-stu-id="02914-128">Here, you define what you actually want toodeploy.</span></span> <span data-ttu-id="02914-129">Eltérően `parameters` és `variables` (amelyeket JSON-objektumok), `resources` JSON JSON-objektumok listáját.</span><span class="sxs-lookup"><span data-stu-id="02914-129">Unlike `parameters` and `variables` (which are JSON objects), `resources` is a JSON list of JSON objects.</span></span>

```json
   "resources": [
```

<span data-ttu-id="02914-130">Összes erőforrást igényelnek `type`, `name`, `apiVersion`, és `location` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="02914-130">All resources require `type`, `name`, `apiVersion`, and `location` properties.</span></span> <span data-ttu-id="02914-131">Ez a példa első erőforrás fájltípusú `Microsft.Network/virtualNetwork`nevű `myVnet`, és apiVersion `2016-03-30`.</span><span class="sxs-lookup"><span data-stu-id="02914-131">This example's first resource has type `Microsft.Network/virtualNetwork`, name `myVnet`, and apiVersion `2016-03-30`.</span></span> <span data-ttu-id="02914-132">(toofind hello legújabb API-verzió erőforrástípus, lásd: hello [Azure REST API-dokumentáció](https://docs.microsoft.com/rest/api/).)</span><span class="sxs-lookup"><span data-stu-id="02914-132">(toofind hello latest API version for a resource type, see hello [Azure REST API documentation](https://docs.microsoft.com/rest/api/).)</span></span>

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a><span data-ttu-id="02914-133">Adjon meg helyet</span><span class="sxs-lookup"><span data-stu-id="02914-133">Specify location</span></span>
<span data-ttu-id="02914-134">virtuális hálózati hello toospecify hello helyét, használjuk a [Resource Manager sablon függvény](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="02914-134">toospecify hello location for hello virtual network, we use a [Resource Manager template function](../azure-resource-manager/resource-group-template-functions.md).</span></span> <span data-ttu-id="02914-135">Ez a funkció ajánlatok és a kapcsos zárójeleket kell tenni: `"[<template-function>]"`.</span><span class="sxs-lookup"><span data-stu-id="02914-135">This function must be enclosed in quotes and square brackets like this: `"[<template-function>]"`.</span></span> <span data-ttu-id="02914-136">Ebben az esetben használjuk hello `resourceGroup` függvény.</span><span class="sxs-lookup"><span data-stu-id="02914-136">In this case, we use hello `resourceGroup` function.</span></span> <span data-ttu-id="02914-137">Veszi a argumentum nélkül, és a központi telepítés telepítése hello erőforráscsoport kapcsolatos metaadatok egy JSON-objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="02914-137">It takes in no arguments and returns a JSON object with metadata about hello resource group this deployment is being deployed to.</span></span> <span data-ttu-id="02914-138">hello erőforráscsoport hello időpontban a központi telepítés hello felhasználó van beállítva.</span><span class="sxs-lookup"><span data-stu-id="02914-138">hello resource group is set by hello user at hello time of deployment.</span></span> <span data-ttu-id="02914-139">Azt, majd a JSON-objektum az index `.location` tooget hello hely hello JSON-objektumból.</span><span class="sxs-lookup"><span data-stu-id="02914-139">We then index into this JSON object with `.location` tooget hello location from hello JSON object.</span></span>

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a><span data-ttu-id="02914-140">Adja meg a virtuális hálózati tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="02914-140">Specify virtual network properties</span></span>
<span data-ttu-id="02914-141">Minden erőforrás-kezelő erőforrás rendelkezik saját `properties` konfigurációk adott toohello erőforrás szakaszát.</span><span class="sxs-lookup"><span data-stu-id="02914-141">Each Resource Manager resource has its own `properties` section for configurations specific toohello resource.</span></span> <span data-ttu-id="02914-142">Ebben az esetben adtuk meg hello virtuális hálózatban kell hello privát IP-címtartományt egy alhálózatot `10.0.0.0/16`.</span><span class="sxs-lookup"><span data-stu-id="02914-142">In this case, we specify that hello virtual network should have one subnet using hello private IP address range `10.0.0.0/16`.</span></span> <span data-ttu-id="02914-143">A méretezési mindig szerepel egy alhálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="02914-143">A scale set is always contained within one subnet.</span></span> <span data-ttu-id="02914-144">Alhálózatok nem foglal magába.</span><span class="sxs-lookup"><span data-stu-id="02914-144">It cannot span subnets.</span></span>

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a><span data-ttu-id="02914-145">Adja hozzá a dependsOn listája</span><span class="sxs-lookup"><span data-stu-id="02914-145">Add dependsOn list</span></span>
<span data-ttu-id="02914-146">Ezenkívül toohello szükséges `type`, `name`, `apiVersion`, és `location` tulajdonságok, az egyes erőforrások lehetnek egy nem kötelező `dependsOn` karakterláncok listáját.</span><span class="sxs-lookup"><span data-stu-id="02914-146">In addition toohello required `type`, `name`, `apiVersion`, and `location` properties, each resource can have an optional `dependsOn` list of strings.</span></span> <span data-ttu-id="02914-147">Ez a lista megadja, hogy más erőforrások, a központi telepítésből Befejezés ehhez az erőforráshoz telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="02914-147">This list specifies which other resources from this deployment must finish before deploying this resource.</span></span>

<span data-ttu-id="02914-148">Ebben az esetben van csak egy elem hello listában hello hello előző példa virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="02914-148">In this case, there is only one element in hello list, hello virtual network from hello previous example.</span></span> <span data-ttu-id="02914-149">A függőség adtuk meg, mert hello méretezési igények hello hálózati tooexist bármely virtuális gépek létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="02914-149">We specify this dependency because hello scale set needs hello network tooexist before creating any VMs.</span></span> <span data-ttu-id="02914-150">Ezzel a módszerrel hello méretezési hello IP-címtartomány hello hálózati tulajdonságainak korábban már megadta a biztosíthat a virtuális gépek magánhálózati IP-címét.</span><span class="sxs-lookup"><span data-stu-id="02914-150">This way, hello scale set can give these VMs private IP addresses from hello IP address range previously specified in hello network properties.</span></span> <span data-ttu-id="02914-151">hello hello dependsOn listán minden karakterlánc formátuma `<type>/<name>`.</span><span class="sxs-lookup"><span data-stu-id="02914-151">hello format of each string in hello dependsOn list is `<type>/<name>`.</span></span> <span data-ttu-id="02914-152">Használja az azonos hello `type` és `name` korábban használt hello virtuális hálózati erőforrás-definícióban.</span><span class="sxs-lookup"><span data-stu-id="02914-152">Use hello same `type` and `name` used previously in hello virtual network resource definition.</span></span>

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a><span data-ttu-id="02914-153">Adja meg a skála tulajdonságainak beállítása</span><span class="sxs-lookup"><span data-stu-id="02914-153">Specify scale set properties</span></span>
<span data-ttu-id="02914-154">Méretezési csoportok hello virtuális gépek hello méretezési csoportban lévő testreszabásához sok tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="02914-154">Scale sets have many properties for customizing hello VMs in hello scale set.</span></span> <span data-ttu-id="02914-155">Ezek a tulajdonságok teljes listáját lásd: hello [méretezési REST API-dokumentáció](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span><span class="sxs-lookup"><span data-stu-id="02914-155">For a full list of these properties, see hello [scale set REST API documentation](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span></span> <span data-ttu-id="02914-156">Ebben az oktatóanyagban helyezünk csak néhány gyakran használt tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="02914-156">For this tutorial, we will set only a few commonly used properties.</span></span>
### <a name="supply-vm-size-and-capacity"></a><span data-ttu-id="02914-157">Adja meg a Virtuálisgép-méretet és a kapacitás</span><span class="sxs-lookup"><span data-stu-id="02914-157">Supply VM size and capacity</span></span>
<span data-ttu-id="02914-158">hello méretezési igények tooknow milyen mértékű VM toocreate ("termékváltozat"), és hány ilyen virtuális gépek toocreate ("termékváltozat-kapacitás").</span><span class="sxs-lookup"><span data-stu-id="02914-158">hello scale set needs tooknow what size of VM toocreate ("sku name") and how many such VMs toocreate ("sku capacity").</span></span> <span data-ttu-id="02914-159">toosee mely Virtuálisgép-méretek érhetők el, lásd: hello [Virtuálisgép-méretek dokumentáció](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span><span class="sxs-lookup"><span data-stu-id="02914-159">toosee which VM sizes are available, see hello [VM Sizes documentation](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span></span>

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a><span data-ttu-id="02914-160">Válassza ki a kívánt frissítések típusát</span><span class="sxs-lookup"><span data-stu-id="02914-160">Choose type of updates</span></span>
<span data-ttu-id="02914-161">hello méretezési kell tooknow toohandle frissítéséről hello méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="02914-161">hello scale set also needs tooknow how toohandle updates on hello scale set.</span></span> <span data-ttu-id="02914-162">Jelenleg nincsenek két lehetőség `Manual` és `Automatic`.</span><span class="sxs-lookup"><span data-stu-id="02914-162">Currently, there are two options, `Manual` and `Automatic`.</span></span> <span data-ttu-id="02914-163">Hello különbségei hello két további információkért lásd: hello dokumentáció a [hogyan tooupgrade egy méretezési](./virtual-machine-scale-sets-upgrade-scale-set.md).</span><span class="sxs-lookup"><span data-stu-id="02914-163">For more information on hello differences between hello two, see hello documentation on [how tooupgrade a scale set](./virtual-machine-scale-sets-upgrade-scale-set.md).</span></span>

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a><span data-ttu-id="02914-164">Válassza ki a virtuális gép operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="02914-164">Choose VM operating system</span></span>
<span data-ttu-id="02914-165">hello méretezési igények tooknow milyen operációs rendszer tooput hello virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="02914-165">hello scale set needs tooknow what operating system tooput on hello VMs.</span></span> <span data-ttu-id="02914-166">Itt létrehozhatunk hello virtuális gépek egy teljes mértékben javított Ubuntu 16.04-es lts verzió lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="02914-166">Here, we create hello VMs with a fully patched Ubuntu 16.04-LTS image.</span></span>

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a><span data-ttu-id="02914-167">Adja meg a gépnév előtagja</span><span class="sxs-lookup"><span data-stu-id="02914-167">Specify computerNamePrefix</span></span>
<span data-ttu-id="02914-168">hello méretezési több virtuális gép telepítését.</span><span class="sxs-lookup"><span data-stu-id="02914-168">hello scale set deploys multiple VMs.</span></span> <span data-ttu-id="02914-169">Minden virtuális gép nevének megadása helyett adtuk meg `computerNamePrefix`.</span><span class="sxs-lookup"><span data-stu-id="02914-169">Instead of specifying each VM name, we specify `computerNamePrefix`.</span></span> <span data-ttu-id="02914-170">hello méretezési hozzáfűz egy index toohello előtagot az egyes virtuális gépek, virtuális gépek nevénél jogosult hello űrlap `<computerNamePrefix>_<auto-generated-index>`.</span><span class="sxs-lookup"><span data-stu-id="02914-170">hello scale set appends an index toohello prefix for each VM, so VM names have hello form `<computerNamePrefix>_<auto-generated-index>`.</span></span>

<span data-ttu-id="02914-171">A következő kódrészletet hello hello tooset hello rendszergazdai jogosultságú felhasználónevet és jelszót előtt származó paraméterek használjuk hello méretezési csoportban lévő virtuális gép esetében.</span><span class="sxs-lookup"><span data-stu-id="02914-171">In hello following snippet, we use hello parameters from before tooset hello administrator username and password for all VMs in hello scale set.</span></span> <span data-ttu-id="02914-172">A Microsoft ehhez a hello `parameters` sablonfüggvény.</span><span class="sxs-lookup"><span data-stu-id="02914-172">We do this with hello `parameters` template function.</span></span> <span data-ttu-id="02914-173">Ez a függvény egy karakterlánc, amely meghatározza, melyik paraméter toorefer tooand kiírja, hogy a paraméter értékét hello időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="02914-173">This function takes in a string that specifies which parameter toorefer tooand outputs hello value for that parameter.</span></span>

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a><span data-ttu-id="02914-174">Adja meg a Virtuálisgép-hálózati konfiguráció</span><span class="sxs-lookup"><span data-stu-id="02914-174">Specify VM network configuration</span></span>
<span data-ttu-id="02914-175">Végül igazolnia kell toospecify hello hálózati konfiguráció hello méretezési csoportban lévő hello virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="02914-175">Finally, we need toospecify hello network configuration for hello VMs in hello scale set.</span></span> <span data-ttu-id="02914-176">Ebben az esetben csak kell korábban létrehozott hello alhálózati toospecify hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="02914-176">In this case, we only need toospecify hello ID of hello subnet created earlier.</span></span> <span data-ttu-id="02914-177">Ez alapján hello méretezési tooput hello hálózati illesztők az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="02914-177">This tells hello scale set tooput hello network interfaces in this subnet.</span></span>

<span data-ttu-id="02914-178">Hello Azonosítóhoz hello virtuális hálózat hello alhálózati tartalmazó hello használatával juthat `resourceId` sablonfüggvény.</span><span class="sxs-lookup"><span data-stu-id="02914-178">You can get hello ID of hello virtual network containing hello subnet by using hello `resourceId` template function.</span></span> <span data-ttu-id="02914-179">Ez a függvény argumentuma hello típus és az erőforrás neve, és vissza hello teljesen minősített, hogy az erőforrás-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="02914-179">This function takes in hello type and name of a resource and returns hello fully qualified identifier of that resource.</span></span> <span data-ttu-id="02914-180">Ezt az Azonosítót hello űrlap rendelkezik:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span><span class="sxs-lookup"><span data-stu-id="02914-180">This ID has hello form: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span></span>

<span data-ttu-id="02914-181">Azonban hello hello virtuális hálózat azonosító nem elegendő.</span><span class="sxs-lookup"><span data-stu-id="02914-181">However, hello identifier of hello virtual network is not enough.</span></span> <span data-ttu-id="02914-182">Meg kell adnia, hogy a virtuális gépek kell a méretezési hello hello alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="02914-182">You must specify hello specific subnet that hello scale set VMs should be in.</span></span> <span data-ttu-id="02914-183">toodo, összefűzésére `/subnets/mySubnet` toohello hello virtuális hálózat Azonosítójává.</span><span class="sxs-lookup"><span data-stu-id="02914-183">toodo this, concatenate `/subnets/mySubnet` toohello ID of hello virtual network.</span></span> <span data-ttu-id="02914-184">hello eredménye hello alhálózati teljesen minősített hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="02914-184">hello result is hello fully qualified ID of hello subnet.</span></span> <span data-ttu-id="02914-185">Hajtsa végre a kapott a hello `concat` funkció, amely a karakterláncok több időt vesz igénybe, és visszaadja a kapott.</span><span class="sxs-lookup"><span data-stu-id="02914-185">Do this concatenation with hello `concat` function, which takes in a series of strings and returns their concatenation.</span></span>

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a><span data-ttu-id="02914-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="02914-186">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
