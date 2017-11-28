---
title: "hello Azure CLI aaaManage erőforrások |} Microsoft Docs"
description: "Használja a hello Azure parancssori felület (CLI) toomanage Azure erőforrások és csoportok"
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
ms.openlocfilehash: 3df70e123d14d3bbf2648c71970bac1db4afc025
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cli-toomanage-azure-resources-and-resource-groups"></a><span data-ttu-id="3db56-103">Használja a hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások</span><span class="sxs-lookup"><span data-stu-id="3db56-103">Use hello Azure CLI toomanage Azure resources and resource groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3db56-104">Portál</span><span class="sxs-lookup"><span data-stu-id="3db56-104">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="3db56-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3db56-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="3db56-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3db56-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="3db56-107">REST API</span><span class="sxs-lookup"><span data-stu-id="3db56-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="3db56-108">hello Azure parancssori felület (CLI) az egyik több eszközt használhat toodeploy és a Resource Manager-erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="3db56-108">hello Azure Command-Line Interface (Azure CLI) is one of several tools you can use toodeploy and manage resources with Resource Manager.</span></span> <span data-ttu-id="3db56-109">Ez a cikk be közös módon toomanage Azure erőforráscsoport-sablonok használatával és az erőforrások hello Azure CLI Resource Manager módra.</span><span class="sxs-lookup"><span data-stu-id="3db56-109">This article introduces common ways toomanage Azure resources and resource groups by using hello Azure CLI in Resource Manager mode.</span></span> <span data-ttu-id="3db56-110">Hello CLI toodeploy erőforrások használatával kapcsolatos információkért lásd: [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="3db56-110">For information about using hello CLI toodeploy resources, see [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> <span data-ttu-id="3db56-111">Azure-erőforrások és erőforrás-kezelő kapcsolatos háttér, a Microsoft hello [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3db56-111">For background about Azure resources and Resource Manager, visit hello [Azure Resource Manager Overview](resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3db56-112">toomanage Azure hello Azure parancssori Felülettel rendelkező erőforrások, kell túl[hello Azure parancssori felület telepítése](../cli-install-nodejs.md), és [tooAzure-e jelentkezni](../xplat-cli-connect.md) hello segítségével `azure login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3db56-112">toomanage Azure resources with hello Azure CLI, you need too[install hello Azure CLI](../cli-install-nodejs.md), and [log in tooAzure](../xplat-cli-connect.md) by using hello `azure login` command.</span></span> <span data-ttu-id="3db56-113">Győződjön meg arról, hogy hello CLI erőforrás-kezelő módban van (Futtatás `azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="3db56-113">Make sure hello CLI is in Resource Manager mode (run `azure config mode arm`).</span></span> <span data-ttu-id="3db56-114">Régebben már kötöttek ezeket, ha készen áll a toogo most.</span><span class="sxs-lookup"><span data-stu-id="3db56-114">If you've done these things, you're ready toogo.</span></span>
> 
> 

## <a name="get-resource-groups-and-resources"></a><span data-ttu-id="3db56-115">Erőforráscsoport-sablonok és erőforrások</span><span class="sxs-lookup"><span data-stu-id="3db56-115">Get resource groups and resources</span></span>
### <a name="resource-groups"></a><span data-ttu-id="3db56-116">Erőforráscsoportok</span><span class="sxs-lookup"><span data-stu-id="3db56-116">Resource groups</span></span>
<span data-ttu-id="3db56-117">az előfizetés és a helyek, az összes erőforráscsoport listájának tooget futtassa ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="3db56-117">tooget a list of all resource groups in your subscription and their locations, run this command.</span></span>

    azure group list


### <a name="resources"></a><span data-ttu-id="3db56-118">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="3db56-118">Resources</span></span>
 <span data-ttu-id="3db56-119">toolist csoportosítva, például az összes erőforrás neve *testRG*, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="3db56-119">toolist all resources in a group, such as one with name *testRG*, use hello following command:</span></span>

    azure resource list testRG

<span data-ttu-id="3db56-120">hello csoportban, például egy nevű virtuális gép egyedi erőforrás tooview *MyUbuntuVM*, hello következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="3db56-120">tooview an individual resource within hello group, such as a VM named *MyUbuntuVM*, use a command like hello following:</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="3db56-121">Értesítés hello **Microsoft.Compute/virtualMachines** paraméter.</span><span class="sxs-lookup"><span data-stu-id="3db56-121">Notice hello **Microsoft.Compute/virtualMachines** parameter.</span></span> <span data-ttu-id="3db56-122">Ezzel a paraméterrel adhatja meg a kért információ hello erőforrás hello típusát.</span><span class="sxs-lookup"><span data-stu-id="3db56-122">This parameter indicates hello type of hello resource you are requesting information on.</span></span>

> [!NOTE]
> <span data-ttu-id="3db56-123">Hello használatakor **azure-erőforrás** hello eltérő parancsok **lista** parancsban, meg kell adnia hello erőforrás hello API verziója hello **-o** paraméter.</span><span class="sxs-lookup"><span data-stu-id="3db56-123">When using hello **azure resource** commands other than hello **list** command, you must specify hello API version of hello resource with hello **-o** parameter.</span></span> <span data-ttu-id="3db56-124">Ha bizonytalan hello API-verziót, tekintse át a hello sablonfájl, és hello apiVersion mező hello erőforrás található.</span><span class="sxs-lookup"><span data-stu-id="3db56-124">If you're unsure about hello API version, consult hello template file and find hello apiVersion field for hello resource.</span></span> <span data-ttu-id="3db56-125">A Resource Manager API-verziók kapcsolatban bővebben lásd: [erőforrás-szolgáltatók és típusok](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="3db56-125">For more about API versions in Resource Manager, see [Resource providers and types](resource-manager-supported-services.md).</span></span>
> 
> 

<span data-ttu-id="3db56-126">Az erőforrás adatainak megtekintése, esetén gyakran hasznos toouse hello `--json` paraméter.</span><span class="sxs-lookup"><span data-stu-id="3db56-126">When viewing details on a resource, it is often useful toouse hello `--json` parameter.</span></span> <span data-ttu-id="3db56-127">Ez a paraméter hello kimeneti olvashatóbbá teszi, mivel egyes értékek beágyazott struktúrákat, és a gyűjteményekhez.</span><span class="sxs-lookup"><span data-stu-id="3db56-127">This parameter makes hello output more readable, because some values are nested structures, or collections.</span></span> <span data-ttu-id="3db56-128">hello következő példa bemutatja, hogyan hello eredményeit adatszolgáltató hello **megjelenítése** JSON-dokumentumként parancsot.</span><span class="sxs-lookup"><span data-stu-id="3db56-128">hello following example demonstrates returning hello results of hello **show** command as a JSON document.</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> <span data-ttu-id="3db56-129">Ha azt szeretné, mentés hello JSON adatok toofile hello segítségével &gt; karakter tooa toodirect hello kimenetfájlba.</span><span class="sxs-lookup"><span data-stu-id="3db56-129">If you want, save hello JSON data toofile by using hello &gt; character toodirect hello output tooa file.</span></span> <span data-ttu-id="3db56-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="3db56-130">For example:</span></span>
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a><span data-ttu-id="3db56-131">Címkék</span><span class="sxs-lookup"><span data-stu-id="3db56-131">Tags</span></span>
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a><span data-ttu-id="3db56-132">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="3db56-132">Manage resources</span></span>
<span data-ttu-id="3db56-133">például a tárolási fiók tooa erőforráscsoport erőforrás tooadd hasonló parancs futtatása:</span><span class="sxs-lookup"><span data-stu-id="3db56-133">tooadd a resource such as a storage account tooa resource group, run a command similar to:</span></span>

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

<span data-ttu-id="3db56-134">Ezenkívül toospecifying hello-hello erőforrás API-verzió hello **-o** paraméter, használjon hello **-p** paraméter toopass JSON-formátumú karakterlánc, és a szükséges, vagy további tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="3db56-134">In addition toospecifying hello API version of hello resource with hello **-o** parameter, use hello **-p** parameter toopass a JSON-formatted string with any required or additional properties.</span></span>

<span data-ttu-id="3db56-135">Virtuálisgép-erőforrás, például a meglévő erőforrás toodelete hello következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="3db56-135">toodelete an existing resource such as a virtual machine resource, use a command like hello following:</span></span>

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="3db56-136">toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello **az azure erőforrás-áthelyezés** parancsot.</span><span class="sxs-lookup"><span data-stu-id="3db56-136">toomove existing resources tooanother resource group or subscription, use hello **azure resource move** command.</span></span> <span data-ttu-id="3db56-137">a következő példa azt mutatja meg hogyan hello toomove Redis Cache tooa új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3db56-137">hello following example shows how toomove a Redis Cache tooa new resource group.</span></span> <span data-ttu-id="3db56-138">A hello **-i** paraméter, hello erőforrás-azonosítók toomove vesszővel tagolt listáját tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="3db56-138">In hello **-i** parameter, provide a comma-separated list of hello resource id's toomove.</span></span>

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-tooresources"></a><span data-ttu-id="3db56-139">Vezérlő hozzáférés tooresources</span><span class="sxs-lookup"><span data-stu-id="3db56-139">Control access tooresources</span></span>
<span data-ttu-id="3db56-140">Hello Azure CLI toocreate használja, és házirendek toocontrol tooAzure erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="3db56-140">You can use hello Azure CLI toocreate and manage policies toocontrol access tooAzure resources.</span></span> <span data-ttu-id="3db56-141">Házirend-definíciók és hozzárendelése házirendek tooresources kapcsolatos háttér, lásd: [házirend toomanage erőforrásainak használatához, és hozzáférést](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="3db56-141">For background about policy definitions and assigning policies tooresources, see [Use policy toomanage resources and control access](resource-manager-policy.md).</span></span>

<span data-ttu-id="3db56-142">Például adja meg a következő házirend toodeny hello összes kérelmet, ahol helye nem USA nyugati régiója vagy északi középső Régiójában, és mentse toohello házirend definíciós fájl policy.json:</span><span class="sxs-lookup"><span data-stu-id="3db56-142">For example, define hello following policy toodeny all requests where location is not West US or North Central US, and save it toohello policy definition file policy.json:</span></span>

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

<span data-ttu-id="3db56-143">Futtassa a hello **házirend-definíció létrehozása** parancs:</span><span class="sxs-lookup"><span data-stu-id="3db56-143">Then run hello **policy definition create** command:</span></span>

    azure policy definition create MyPolicy -p c:\temp\policy.json

<span data-ttu-id="3db56-144">Ez a parancs megjeleníti a kimeneti hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="3db56-144">This command shows output similar toohello following:</span></span>

    + <span data-ttu-id="3db56-145">Házirend-definíció MyPolicy adatok létrehozása: házirendnév: MyPolicy adatok: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span><span class="sxs-lookup"><span data-stu-id="3db56-145">Creating policy definition MyPolicy data:    PolicyName:             MyPolicy data:    PolicyDefinitionId:     /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span></span>

    <span data-ttu-id="3db56-146">adatok: PolicyType: egyéni adatok: DisplayName: adatok meg nem határozott: Leírás: adatok nincs definiálva: PolicyRule: mező = hely, a = [westus, northcentralus], hatás = megtagadása</span><span class="sxs-lookup"><span data-stu-id="3db56-146">data:    PolicyType:             Custom data:    DisplayName:            undefined data:    Description:            undefined data:    PolicyRule:             field=location, in=[westus, northcentralus], effect=deny</span></span>

 <span data-ttu-id="3db56-147">egy házirend hello hatókörből szeretne, használja hello tooassign **PolicyDefinitionId** hello előző parancs által visszaadott.</span><span class="sxs-lookup"><span data-stu-id="3db56-147">tooassign a policy at hello scope you want, use hello **PolicyDefinitionId** returned from hello previous command.</span></span> <span data-ttu-id="3db56-148">A következő példa hello, az ebben a hatókörben hello előfizetés, de hatókörét megadhatja a tooresource csoportok vagy az egyes erőforrások:</span><span class="sxs-lookup"><span data-stu-id="3db56-148">In hello following example, this scope is hello subscription, but you can scope tooresource groups or individual resources:</span></span>

    <span data-ttu-id="3db56-149">az Azure házirend-hozzárendelés létrehozása MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /</span><span class="sxs-lookup"><span data-stu-id="3db56-149">azure policy assignment create MyPolicyAssignment -p /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/########-####-####-####-############/</span></span>

<span data-ttu-id="3db56-150">Get, módosítása, vagy távolítsa el a házirend-definíciók hello segítségével **házirend-definíció megjelenítése**, **házirend-definíció set**, és **házirend-definíció törlése** parancsok.</span><span class="sxs-lookup"><span data-stu-id="3db56-150">You can get, change, or remove policy definitions by using hello **policy definition show**, **policy definition set**, and **policy definition delete** commands.</span></span>

<span data-ttu-id="3db56-151">Hasonlóképpen, beolvasása, módosítása vagy eltávolítása a házirend-hozzárendelések hello segítségével **házirend-hozzárendelés megjelenítése**, **házirend-hozzárendelés set**, és **házirend-hozzárendelés törlése** parancsok .</span><span class="sxs-lookup"><span data-stu-id="3db56-151">Similarly, you can get, change, or remove policy assignments by using hello **policy assignment show**, **policy assignment set**, and **policy assignment delete** commands.</span></span>

## <a name="export-a-resource-group-as-a-template"></a><span data-ttu-id="3db56-152">Erőforráscsoport exportálása sablonként</span><span class="sxs-lookup"><span data-stu-id="3db56-152">Export a resource group as a template</span></span>
<span data-ttu-id="3db56-153">Egy meglévő erőforráscsoportot, a Resource Manager-sablon hello hello erőforráscsoport tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="3db56-153">For an existing resource group, you can view hello Resource Manager template for hello resource group.</span></span> <span data-ttu-id="3db56-154">Exportáló hello sablon két előnyökkel jár:</span><span class="sxs-lookup"><span data-stu-id="3db56-154">Exporting hello template offers two benefits:</span></span>

1. <span data-ttu-id="3db56-155">Könnyen automatizálható a későbbi központi telepítés alkalmával hello megoldás, mert minden hello infrastruktúra hello sablon definiálva van.</span><span class="sxs-lookup"><span data-stu-id="3db56-155">You can easily automate future deployments of hello solution because all hello infrastructure is defined in hello template.</span></span>
2. <span data-ttu-id="3db56-156">Válhat ismeri a sablon szintaxisát a megoldás jelölő JSON hello megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="3db56-156">You can become familiar with template syntax by looking at hello JSON that represents your solution.</span></span>

<span data-ttu-id="3db56-157">Hello Azure parancssori felület használatával, exportálja a sablont, amely hello az erőforráscsoport aktuális állapotát jeleníti meg, vagy töltse le az adott példány használt hello tanúsítványsablont.</span><span class="sxs-lookup"><span data-stu-id="3db56-157">Using hello Azure CLI, you can either export a template that represents hello current state of your resource group, or download hello template that was used for a particular deployment.</span></span>

* <span data-ttu-id="3db56-158">**Erőforráscsoport hello sablon exportálása** – Ez akkor hasznos, ha végrehajtott módosítások tooa erőforráscsoport, és tooretrieve hello kell a jelenlegi állapotában JSON-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3db56-158">**Export hello template for a resource group** - This is helpful when you made changes tooa resource group, and need tooretrieve hello JSON representation of its current state.</span></span> <span data-ttu-id="3db56-159">Hello létrehozott sablon azonban csak minimális mennyiségű paraméterek és változók nem tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3db56-159">However, hello generated template contains only a minimal number of parameters and no variables.</span></span> <span data-ttu-id="3db56-160">Hello értékek hello sablonban legtöbbje-sablonja.</span><span class="sxs-lookup"><span data-stu-id="3db56-160">Most of hello values in hello template are hard-coded.</span></span> <span data-ttu-id="3db56-161">Hello létrehozott sablon telepítés megkezdése előtt érdemes tooconvert hello több-paraméterek sorozatán különböző környezetek hello telepítés testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="3db56-161">Before deploying hello generated template, you may wish tooconvert more of hello values into parameters so you can customize hello deployment for different environments.</span></span>
  
    <span data-ttu-id="3db56-162">az erőforrás csoport tooa helyi címtárhoz, futtassa a hello tooexport hello sablon `azure group export` látható módon a következő példa hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="3db56-162">tooexport hello template for a resource group tooa local directory, run hello `azure group export` command as shown in hello following example.</span></span> <span data-ttu-id="3db56-163">(A helyi könyvtárat, az operációs rendszer környezetének megfelelő helyettesítő.)</span><span class="sxs-lookup"><span data-stu-id="3db56-163">(Substitute a local directory appropriate for your operating system environment.)</span></span>
  
        azure group export testRG ~/azure/templates/
* <span data-ttu-id="3db56-164">**Az adott példány hello sablon letöltése** – Ez akkor hasznos, ha tooview hello tényleges sablon, de a használt toodeploy erőforrások van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3db56-164">**Download hello template for a particular deployment** -- This is helpful when you need tooview hello actual template that was used toodeploy resources.</span></span> <span data-ttu-id="3db56-165">hello sablon minden paraméterek és változók hello eredeti központi telepítés meghatározott tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3db56-165">hello template includes all parameters and variables defined for hello original deployment.</span></span> <span data-ttu-id="3db56-166">Azonban ha valaki van a szervezet végzett módosítások toohello erőforráscsoport hello definíció kívül hello sablon, ez a sablon nem határoz meg hello hello erőforráscsoport aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="3db56-166">However, if someone in your organization made changes toohello resource group outside of hello definition in hello template, this template doesn't represent hello current state of hello resource group.</span></span>
  
    <span data-ttu-id="3db56-167">egy adott telepítési tooa helyi könyvtárat, hello futtatásához használt toodownload hello sablon `azure group deployment template download` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3db56-167">toodownload hello template used for a particular deployment tooa local directory, run hello `azure group deployment template download` command.</span></span> <span data-ttu-id="3db56-168">Példa:</span><span class="sxs-lookup"><span data-stu-id="3db56-168">For example:</span></span>
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> <span data-ttu-id="3db56-169">Sablon exportálása jelenleg előzetes verzióban érhető, és nem az összes erőforrástípus jelenleg támogatja a sablon exportálása.</span><span class="sxs-lookup"><span data-stu-id="3db56-169">Template export is in preview, and not all resource types currently support exporting a template.</span></span> <span data-ttu-id="3db56-170">A sablon tooexport megkísérelte arról, hogy bizonyos erőforrások nem lettek-e exportálva hiba jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="3db56-170">When attempting tooexport a template, you may see an error that states some resources were not exported.</span></span> <span data-ttu-id="3db56-171">Szükség esetén manuálisan ezeket az erőforrásokat a sablonban megadott azt letöltése után.</span><span class="sxs-lookup"><span data-stu-id="3db56-171">If needed, manually define these resources in your template after downloading it.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="3db56-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3db56-172">Next steps</span></span>
* <span data-ttu-id="3db56-173">üzembe helyezési műveletek tooget részleteit és hello Azure parancssori felület telepítési hibáinak elhárításával kapcsolatos tudnivalókat lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="3db56-173">tooget details of deployment operations and troubleshoot deployment errors with hello Azure CLI, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="3db56-174">Ha azt szeretné, hogy toouse hello CLI tooset egy alkalmazás vagy parancsfájl tooaccess erőforrások, lásd: [egyszerű szolgáltatás használata az Azure parancssori felület toocreate tooaccess erőforrások](resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="3db56-174">If you want toouse hello CLI tooset up an application or script tooaccess resources, see [Use Azure CLI toocreate a service principal tooaccess resources](resource-group-authenticate-service-principal-cli.md).</span></span>
* <span data-ttu-id="3db56-175">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="3db56-175">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

