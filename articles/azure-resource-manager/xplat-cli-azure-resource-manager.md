---
title: "Kezelheti az erőforrásokat az Azure parancssori felülettel |} Microsoft Docs"
description: "Azure-erőforrások és csoportok kezelése az Azure parancssori felület (CLI) használatával"
editor: 
manager: timlt
documentationcenter: 
author: tfitzmac
services: azure-resource-manager
ms.assetid: bb0af466-4f65-4559-ac3a-43985fa096ff
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: tomfitz
ms.openlocfilehash: 3ad4e68b90979fd7f9d3ddf5278e65e19cb07152
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a><span data-ttu-id="a9436-103">Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a9436-103">Use the Azure CLI to manage Azure resources and resource groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a9436-104">Portal</span><span class="sxs-lookup"><span data-stu-id="a9436-104">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="a9436-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a9436-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="a9436-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a9436-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="a9436-107">REST API</span><span class="sxs-lookup"><span data-stu-id="a9436-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="a9436-108">Az Azure parancssori felület (CLI) használatával telepítheti és kezelheti az erőforrásokat a Resource Manager számos eszköz egyike.</span><span class="sxs-lookup"><span data-stu-id="a9436-108">The Azure Command-Line Interface (Azure CLI) is one of several tools you can use to deploy and manage resources with Resource Manager.</span></span> <span data-ttu-id="a9436-109">Ez a cikk az Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával erőforrás-kezelő módban gyakori módjai be.</span><span class="sxs-lookup"><span data-stu-id="a9436-109">This article introduces common ways to manage Azure resources and resource groups by using the Azure CLI in Resource Manager mode.</span></span> <span data-ttu-id="a9436-110">Erőforrások telepítése a parancssori felület használatával kapcsolatos információkért lásd: [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a9436-110">For information about using the CLI to deploy resources, see [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> <span data-ttu-id="a9436-111">Azure-erőforrások és erőforrás-kezelő kapcsolatos háttér, látogasson el a [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a9436-111">For background about Azure resources and Resource Manager, visit the [Azure Resource Manager Overview](resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a9436-112">Az Azure parancssori felület az Azure erőforrások kezeléséhez, kell [az Azure parancssori felület telepítése](../cli-install-nodejs.md), és [jelentkezzen be az Azure](../xplat-cli-connect.md) használatával a `azure login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a9436-112">To manage Azure resources with the Azure CLI, you need to [install the Azure CLI](../cli-install-nodejs.md), and [log in to Azure](../xplat-cli-connect.md) by using the `azure login` command.</span></span> <span data-ttu-id="a9436-113">Ellenőrizze, hogy a parancssori felület erőforrás-kezelő módban van (Futtatás `azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="a9436-113">Make sure the CLI is in Resource Manager mode (run `azure config mode arm`).</span></span> <span data-ttu-id="a9436-114">Ha ezt ezeket, készen áll nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="a9436-114">If you've done these things, you're ready to go.</span></span>
> 
> 

## <a name="get-resource-groups-and-resources"></a><span data-ttu-id="a9436-115">Erőforráscsoport-sablonok és erőforrások</span><span class="sxs-lookup"><span data-stu-id="a9436-115">Get resource groups and resources</span></span>
### <a name="resource-groups"></a><span data-ttu-id="a9436-116">Erőforráscsoportok</span><span class="sxs-lookup"><span data-stu-id="a9436-116">Resource groups</span></span>
<span data-ttu-id="a9436-117">Ahhoz, hogy az összes erőforráscsoport az előfizetés és a helyek listáját, futtassa ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="a9436-117">To get a list of all resource groups in your subscription and their locations, run this command.</span></span>

    azure group list


### <a name="resources"></a><span data-ttu-id="a9436-118">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="a9436-118">Resources</span></span>
 <span data-ttu-id="a9436-119">Egy csoport például nevű összes erőforrás listáját *testRG*, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a9436-119">To list all resources in a group, such as one with name *testRG*, use the following command:</span></span>

    azure resource list testRG

<span data-ttu-id="a9436-120">A csoporton belül egyedi erőforrás megtekintéséhez, például a virtuális gép nevű *MyUbuntuVM*, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="a9436-120">To view an individual resource within the group, such as a VM named *MyUbuntuVM*, use a command like the following:</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="a9436-121">Figyelje meg a **Microsoft.Compute/virtualMachines** paraméter.</span><span class="sxs-lookup"><span data-stu-id="a9436-121">Notice the **Microsoft.Compute/virtualMachines** parameter.</span></span> <span data-ttu-id="a9436-122">Ez a paraméter azt jelzi, hogy az adatokat a kért erőforrás típusa.</span><span class="sxs-lookup"><span data-stu-id="a9436-122">This parameter indicates the type of the resource you are requesting information on.</span></span>

> [!NOTE]
> <span data-ttu-id="a9436-123">Használatakor a **azure-erőforrás** eltérő parancsok a **lista** parancs, meg kell adnia az erőforrás API-verzió a **-o** paraméter.</span><span class="sxs-lookup"><span data-stu-id="a9436-123">When using the **azure resource** commands other than the **list** command, you must specify the API version of the resource with the **-o** parameter.</span></span> <span data-ttu-id="a9436-124">Ha bizonytalan az API-verzió, tekintse meg a sablon fájlt, és a apiVersion mező található az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="a9436-124">If you're unsure about the API version, consult the template file and find the apiVersion field for the resource.</span></span> <span data-ttu-id="a9436-125">A Resource Manager API-verziók kapcsolatban bővebben lásd: [erőforrás-szolgáltatók és típusok](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="a9436-125">For more about API versions in Resource Manager, see [Resource providers and types](resource-manager-supported-services.md).</span></span>
> 
> 

<span data-ttu-id="a9436-126">Ha valamely erőforrás adatainak megtekintése, gyakran célszerű használni a `--json` paraméter.</span><span class="sxs-lookup"><span data-stu-id="a9436-126">When viewing details on a resource, it is often useful to use the `--json` parameter.</span></span> <span data-ttu-id="a9436-127">Ez a paraméter a kimeneti olvashatóbbá teszi, mivel egyes értékek beágyazott struktúrákat, és a gyűjteményekhez.</span><span class="sxs-lookup"><span data-stu-id="a9436-127">This parameter makes the output more readable, because some values are nested structures, or collections.</span></span> <span data-ttu-id="a9436-128">A következő példa bemutatja, hogy az eredmények visszaadása a **megjelenítése** JSON-dokumentumként parancsot.</span><span class="sxs-lookup"><span data-stu-id="a9436-128">The following example demonstrates returning the results of the **show** command as a JSON document.</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> <span data-ttu-id="a9436-129">Ha azt szeretné, mentse a JSON-adatok fájlba a &gt; fájlba kimenete karakter.</span><span class="sxs-lookup"><span data-stu-id="a9436-129">If you want, save the JSON data to file by using the &gt; character to direct the output to a file.</span></span> <span data-ttu-id="a9436-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="a9436-130">For example:</span></span>
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a><span data-ttu-id="a9436-131">Címkék</span><span class="sxs-lookup"><span data-stu-id="a9436-131">Tags</span></span>
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a><span data-ttu-id="a9436-132">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="a9436-132">Manage resources</span></span>
<span data-ttu-id="a9436-133">Például a storage-fiókok erőforrás hozzáadása az erőforráscsoporthoz, hasonló parancs futtatása:</span><span class="sxs-lookup"><span data-stu-id="a9436-133">To add a resource such as a storage account to a resource group, run a command similar to:</span></span>

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

<span data-ttu-id="a9436-134">Az erőforrás API-verzió megadása mellett a **-o** paraméter a **-p** paraméter felelt meg az összes szükséges JSON-formátumú karakterlánc- vagy további tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="a9436-134">In addition to specifying the API version of the resource with the **-o** parameter, use the **-p** parameter to pass a JSON-formatted string with any required or additional properties.</span></span>

<span data-ttu-id="a9436-135">Egy meglévő erőforrás, például virtuálisgép-erőforrás törléséhez használja a következőhöz hasonló parancsot:</span><span class="sxs-lookup"><span data-stu-id="a9436-135">To delete an existing resource such as a virtual machine resource, use a command like the following:</span></span>

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="a9436-136">Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a **az azure erőforrás-áthelyezés** parancsot.</span><span class="sxs-lookup"><span data-stu-id="a9436-136">To move existing resources to another resource group or subscription, use the **azure resource move** command.</span></span> <span data-ttu-id="a9436-137">A következő példa bemutatja, hogyan egy Redis gyorsítótárhoz áthelyezése egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a9436-137">The following example shows how to move a Redis Cache to a new resource group.</span></span> <span data-ttu-id="a9436-138">Az a **-i** paraméter, adjon meg egy vesszővel elválasztott lista az erőforrás-azonosító által történő áthelyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="a9436-138">In the **-i** parameter, provide a comma-separated list of the resource id's to move.</span></span>

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a><span data-ttu-id="a9436-139">Az erőforrások hozzáférés-vezérlése</span><span class="sxs-lookup"><span data-stu-id="a9436-139">Control access to resources</span></span>
<span data-ttu-id="a9436-140">Az Azure parancssori felület használatával történő hozzáférés szabályozása érdekében Azure-erőforrások szabályzatok létrehozása és kezelése.</span><span class="sxs-lookup"><span data-stu-id="a9436-140">You can use the Azure CLI to create and manage policies to control access to Azure resources.</span></span> <span data-ttu-id="a9436-141">Házirend-definíciók és a szabályzatok hozzárendelését erőforrások kapcsolatos háttér, lásd: [házirend segítségével kezelheti az erőforrásokat, és hozzáférést](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="a9436-141">For background about policy definitions and assigning policies to resources, see [Use policy to manage resources and control access](resource-manager-policy.md).</span></span>

<span data-ttu-id="a9436-142">Például adja meg a következő házirendet megtagadja a hozzáférést minden kérelmek, ahol helyre nincs USA nyugati régiója vagy északi középső Régiójában, és mentse a házirend-definíció fájl policy.json:</span><span class="sxs-lookup"><span data-stu-id="a9436-142">For example, define the following policy to deny all requests where location is not West US or North Central US, and save it to the policy definition file policy.json:</span></span>

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

<span data-ttu-id="a9436-143">Ezután futtassa a **házirend-definíció létrehozása** parancs:</span><span class="sxs-lookup"><span data-stu-id="a9436-143">Then run the **policy definition create** command:</span></span>

    azure policy definition create MyPolicy -p c:\temp\policy.json

<span data-ttu-id="a9436-144">Ez a parancs kimenetét mutatja a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="a9436-144">This command shows output similar to the following:</span></span>

    + <span data-ttu-id="a9436-145">Házirend-definíció MyPolicy adatok létrehozása: házirendnév: MyPolicy adatok: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span><span class="sxs-lookup"><span data-stu-id="a9436-145">Creating policy definition MyPolicy data:    PolicyName:             MyPolicy data:    PolicyDefinitionId:     /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span></span>

    <span data-ttu-id="a9436-146">adatok: PolicyType: egyéni adatok: DisplayName: adatok meg nem határozott: Leírás: adatok nincs definiálva: PolicyRule: mező = hely, a = [westus, northcentralus], hatás = megtagadása</span><span class="sxs-lookup"><span data-stu-id="a9436-146">data:    PolicyType:             Custom data:    DisplayName:            undefined data:    Description:            undefined data:    PolicyRule:             field=location, in=[westus, northcentralus], effect=deny</span></span>

 <span data-ttu-id="a9436-147">Rendelje hozzá a házirend a hatókörben, ha azt szeretné, használja a **PolicyDefinitionId** az előző parancs által visszaadott.</span><span class="sxs-lookup"><span data-stu-id="a9436-147">To assign a policy at the scope you want, use the **PolicyDefinitionId** returned from the previous command.</span></span> <span data-ttu-id="a9436-148">A következő példában az ebben a hatókörben az előfizetés, de erőforráscsoport-sablonok vagy egyéni erőforrások hatókörét megadhatja:</span><span class="sxs-lookup"><span data-stu-id="a9436-148">In the following example, this scope is the subscription, but you can scope to resource groups or individual resources:</span></span>

    <span data-ttu-id="a9436-149">az Azure házirend-hozzárendelés létrehozása MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /</span><span class="sxs-lookup"><span data-stu-id="a9436-149">azure policy assignment create MyPolicyAssignment -p /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/########-####-####-####-############/</span></span>

<span data-ttu-id="a9436-150">Get, módosíthatja, vagy távolítsa el a házirend-definíciók használatával a **házirend-definíció megjelenítése**, **házirend-definíció set**, és **házirend-definíció törlése** parancsok.</span><span class="sxs-lookup"><span data-stu-id="a9436-150">You can get, change, or remove policy definitions by using the **policy definition show**, **policy definition set**, and **policy definition delete** commands.</span></span>

<span data-ttu-id="a9436-151">Hasonlóan beolvasása, módosítsa vagy távolítsa el a házirend-hozzárendelések használatával a **házirend-hozzárendelés megjelenítése**, **házirend-hozzárendelés set**, és **házirend-hozzárendelés törlése** parancsok.</span><span class="sxs-lookup"><span data-stu-id="a9436-151">Similarly, you can get, change, or remove policy assignments by using the **policy assignment show**, **policy assignment set**, and **policy assignment delete** commands.</span></span>

## <a name="export-a-resource-group-as-a-template"></a><span data-ttu-id="a9436-152">Erőforráscsoport exportálása sablonként</span><span class="sxs-lookup"><span data-stu-id="a9436-152">Export a resource group as a template</span></span>
<span data-ttu-id="a9436-153">Egy meglévő erőforráscsoportot megtekintheti a Resource Manager-sablon ahhoz az erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="a9436-153">For an existing resource group, you can view the Resource Manager template for the resource group.</span></span> <span data-ttu-id="a9436-154">A sablon exportálása két előnyökkel jár:</span><span class="sxs-lookup"><span data-stu-id="a9436-154">Exporting the template offers two benefits:</span></span>

1. <span data-ttu-id="a9436-155">Könnyen automatizálható a későbbi központi telepítés alkalmával a megoldás, mert az infrastrukturális van definiálva a sablonban.</span><span class="sxs-lookup"><span data-stu-id="a9436-155">You can easily automate future deployments of the solution because all the infrastructure is defined in the template.</span></span>
2. <span data-ttu-id="a9436-156">Válhat ismeri a sablon szintaxisát a JSON a megoldás jelölő megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="a9436-156">You can become familiar with template syntax by looking at the JSON that represents your solution.</span></span>

<span data-ttu-id="a9436-157">Az Azure parancssori felület használatával, exportálja a sablont, amely az erőforráscsoport aktuális állapotát jeleníti meg, vagy töltse le az adott példány használt tanúsítványsablont.</span><span class="sxs-lookup"><span data-stu-id="a9436-157">Using the Azure CLI, you can either export a template that represents the current state of your resource group, or download the template that was used for a particular deployment.</span></span>

* <span data-ttu-id="a9436-158">**Erőforráscsoport sablonjának exportálása** – Ez akkor hasznos, ha módosítja egy erőforráscsoport, és szeretné beolvasni a jelenlegi állapotában a JSON-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="a9436-158">**Export the template for a resource group** - This is helpful when you made changes to a resource group, and need to retrieve the JSON representation of its current state.</span></span> <span data-ttu-id="a9436-159">A létrehozott sablon azonban csak minimális mennyiségű paraméterek és változók nem tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a9436-159">However, the generated template contains only a minimal number of parameters and no variables.</span></span> <span data-ttu-id="a9436-160">Kódolt legtöbbje a sablonban lévő érték.</span><span class="sxs-lookup"><span data-stu-id="a9436-160">Most of the values in the template are hard-coded.</span></span> <span data-ttu-id="a9436-161">A létrehozott sablon telepítése előtt lehet, hogy a konvertálandó értékek-paraméterek sorozatán testreszabásához, különböző környezetek központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="a9436-161">Before deploying the generated template, you may wish to convert more of the values into parameters so you can customize the deployment for different environments.</span></span>
  
    <span data-ttu-id="a9436-162">Erőforráscsoport sablonjának exportálása egy helyi könyvtárba, futtassa a `azure group export` parancsot a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="a9436-162">To export the template for a resource group to a local directory, run the `azure group export` command as shown in the following example.</span></span> <span data-ttu-id="a9436-163">(A helyi könyvtárat, az operációs rendszer környezetének megfelelő helyettesítő.)</span><span class="sxs-lookup"><span data-stu-id="a9436-163">(Substitute a local directory appropriate for your operating system environment.)</span></span>
  
        azure group export testRG ~/azure/templates/
* <span data-ttu-id="a9436-164">**Töltse le a sablont az adott példány** – Ez akkor hasznos, ha meg kell a tényleges erőforrások telepítéséhez használt tanúsítványsablont.</span><span class="sxs-lookup"><span data-stu-id="a9436-164">**Download the template for a particular deployment** -- This is helpful when you need to view the actual template that was used to deploy resources.</span></span> <span data-ttu-id="a9436-165">A sablon tartalmazza az összes paraméterek és az eredeti telepítési definiált változókat.</span><span class="sxs-lookup"><span data-stu-id="a9436-165">The template includes all parameters and variables defined for the original deployment.</span></span> <span data-ttu-id="a9436-166">Azonban ha valaki van a szervezet módosította az erőforráscsoport a sablon a definíció kívül, ez a sablon nem határoz meg az erőforráscsoport aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="a9436-166">However, if someone in your organization made changes to the resource group outside of the definition in the template, this template doesn't represent the current state of the resource group.</span></span>
  
    <span data-ttu-id="a9436-167">Az adott példány egy helyi könyvtárba használt sablon letöltéséhez futtassa a `azure group deployment template download` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a9436-167">To download the template used for a particular deployment to a local directory, run the `azure group deployment template download` command.</span></span> <span data-ttu-id="a9436-168">Példa:</span><span class="sxs-lookup"><span data-stu-id="a9436-168">For example:</span></span>
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> <span data-ttu-id="a9436-169">Sablon exportálása jelenleg előzetes verzióban érhető, és nem az összes erőforrástípus jelenleg támogatja a sablon exportálása.</span><span class="sxs-lookup"><span data-stu-id="a9436-169">Template export is in preview, and not all resource types currently support exporting a template.</span></span> <span data-ttu-id="a9436-170">Sablon exportálása megkísérelte arról, hogy bizonyos erőforrások nem lettek-e exportálva hiba jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="a9436-170">When attempting to export a template, you may see an error that states some resources were not exported.</span></span> <span data-ttu-id="a9436-171">Szükség esetén manuálisan ezeket az erőforrásokat a sablonban megadott azt letöltése után.</span><span class="sxs-lookup"><span data-stu-id="a9436-171">If needed, manually define these resources in your template after downloading it.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="a9436-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9436-172">Next steps</span></span>
* <span data-ttu-id="a9436-173">Részletek a telepítési műveletek és az Azure parancssori felülettel telepítési hibáinak elhárítása: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="a9436-173">To get details of deployment operations and troubleshoot deployment errors with the Azure CLI, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="a9436-174">Ha azt szeretné, állítson be egy alkalmazás vagy parancsprogram hozzáférését az erőforrásokhoz, tekintse meg a parancssori felület segítségével [Azure CLI használata az erőforrásokhoz való hozzáféréshez szolgáltatásnevet létrehozni](resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a9436-174">If you want to use the CLI to set up an application or script to access resources, see [Use Azure CLI to create a service principal to access resources](resource-group-authenticate-service-principal-cli.md).</span></span>
* <span data-ttu-id="a9436-175">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="a9436-175">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

