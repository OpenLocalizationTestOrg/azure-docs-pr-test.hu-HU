---
title: "aaaCreate egyéni összetevők a DevTest Labs virtuális géphez |} Microsoft Docs"
description: "Ismerje meg, hogyan tooauthor a saját összetevők használata DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 32dcdc61-ec23-4a01-b731-78c029ea5316
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 2bd603bc1241ca6b669a3a276a677729514f0df2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a><span data-ttu-id="2307d-103">Egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális Géphez</span><span class="sxs-lookup"><span data-stu-id="2307d-103">Create custom artifacts for your DevTest Labs VM</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a><span data-ttu-id="2307d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2307d-104">Overview</span></span>
<span data-ttu-id="2307d-105">**Az összetevők** használt toodeploy, és állítsa be az alkalmazását, a virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="2307d-105">**Artifacts** are used toodeploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="2307d-106">Összetevő egy összetevő csomagdefiníciós fájlt, és más parancsfájl egy git-tárházban mappában tárolt fájlok áll.</span><span class="sxs-lookup"><span data-stu-id="2307d-106">An artifact consists of an artifact definition file and other script files that are stored in a folder in a git repository.</span></span> <span data-ttu-id="2307d-107">Összetevő-definíciós fájlok tartalmazzák a JSON és használható toospecify mit kifejezések tooinstall a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2307d-107">Artifact definition files consist of JSON and expressions that you can use toospecify what you want tooinstall on a VM.</span></span> <span data-ttu-id="2307d-108">Például megadhatja hello nevét összetevő, toorun parancsot és paramétereket, amelyeket közzétettek hello parancs futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="2307d-108">For example, you can define hello name of an artifact, command toorun, and parameters that are made available when hello command is run.</span></span> <span data-ttu-id="2307d-109">Parancsfájlok tooother hello összetevő definíciós fájlból olvassa el a neve.</span><span class="sxs-lookup"><span data-stu-id="2307d-109">You can refer tooother script files within hello artifact definition file by name.</span></span>

## <a name="artifact-definition-file-format"></a><span data-ttu-id="2307d-110">Összetevő csomagdefiníciós fájlformátumról</span><span class="sxs-lookup"><span data-stu-id="2307d-110">Artifact definition file format</span></span>
<span data-ttu-id="2307d-111">hello alábbi példa bemutatja egy csomagdefiníciós fájl alapvető szerkezete hello hello szakaszait:</span><span class="sxs-lookup"><span data-stu-id="2307d-111">hello following example shows hello sections that make up hello basic structure of a definition file:</span></span>

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2016-11-28/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| <span data-ttu-id="2307d-112">Elem neve</span><span class="sxs-lookup"><span data-stu-id="2307d-112">Element name</span></span> | <span data-ttu-id="2307d-113">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="2307d-113">Required?</span></span> | <span data-ttu-id="2307d-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="2307d-114">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2307d-115">$schema</span><span class="sxs-lookup"><span data-stu-id="2307d-115">$schema</span></span> |<span data-ttu-id="2307d-116">Nem</span><span class="sxs-lookup"><span data-stu-id="2307d-116">No</span></span> |<span data-ttu-id="2307d-117">Hello JSON séma-fájl helyét, amelyek segítségével a tesztelése hello hello csomagdefiníciós fájl érvényességét.</span><span class="sxs-lookup"><span data-stu-id="2307d-117">Location of hello JSON schema file that helps in testing hello validity of hello definition file.</span></span> |
| <span data-ttu-id="2307d-118">Cím</span><span class="sxs-lookup"><span data-stu-id="2307d-118">title</span></span> |<span data-ttu-id="2307d-119">Igen</span><span class="sxs-lookup"><span data-stu-id="2307d-119">Yes</span></span> |<span data-ttu-id="2307d-120">Hello összetevő hello labor megjelenített neve.</span><span class="sxs-lookup"><span data-stu-id="2307d-120">Name of hello artifact displayed in hello lab.</span></span> |
| <span data-ttu-id="2307d-121">leírás</span><span class="sxs-lookup"><span data-stu-id="2307d-121">description</span></span> |<span data-ttu-id="2307d-122">Igen</span><span class="sxs-lookup"><span data-stu-id="2307d-122">Yes</span></span> |<span data-ttu-id="2307d-123">Hello labor megjelenő hello összetevő leírása.</span><span class="sxs-lookup"><span data-stu-id="2307d-123">Description of hello artifact displayed in hello lab.</span></span> |
| <span data-ttu-id="2307d-124">iconUri</span><span class="sxs-lookup"><span data-stu-id="2307d-124">iconUri</span></span> |<span data-ttu-id="2307d-125">Nem</span><span class="sxs-lookup"><span data-stu-id="2307d-125">No</span></span> |<span data-ttu-id="2307d-126">URI-je hello ikon, hello tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2307d-126">Uri of hello icon displayed in hello lab.</span></span> |
| <span data-ttu-id="2307d-127">targetOsType</span><span class="sxs-lookup"><span data-stu-id="2307d-127">targetOsType</span></span> |<span data-ttu-id="2307d-128">Igen</span><span class="sxs-lookup"><span data-stu-id="2307d-128">Yes</span></span> |<span data-ttu-id="2307d-129">Operációs rendszer a virtuális gép, amelyen telepítve van-e az összetevő hello.</span><span class="sxs-lookup"><span data-stu-id="2307d-129">Operating system of hello VM where artifact is installed.</span></span> <span data-ttu-id="2307d-130">Támogatott beállítások: a Windows és Linux.</span><span class="sxs-lookup"><span data-stu-id="2307d-130">Supported options are: Windows and Linux.</span></span> |
| <span data-ttu-id="2307d-131">paraméterek</span><span class="sxs-lookup"><span data-stu-id="2307d-131">parameters</span></span> |<span data-ttu-id="2307d-132">Nem</span><span class="sxs-lookup"><span data-stu-id="2307d-132">No</span></span> |<span data-ttu-id="2307d-133">Olyan értékek által biztosított összetevő telepítési parancs futtatásakor a gépen.</span><span class="sxs-lookup"><span data-stu-id="2307d-133">Values that are provided when artifact install command is run on a machine.</span></span> <span data-ttu-id="2307d-134">Ez segít a összetevő testreszabása.</span><span class="sxs-lookup"><span data-stu-id="2307d-134">This helps in customizing your artifact.</span></span> |
| <span data-ttu-id="2307d-135">ParancsFuttatása</span><span class="sxs-lookup"><span data-stu-id="2307d-135">runCommand</span></span> |<span data-ttu-id="2307d-136">Igen</span><span class="sxs-lookup"><span data-stu-id="2307d-136">Yes</span></span> |<span data-ttu-id="2307d-137">Összetevő telepítése egy virtuális gépen végrehajtott parancs.</span><span class="sxs-lookup"><span data-stu-id="2307d-137">Artifact install command that is executed on a VM.</span></span> |

### <a name="artifact-parameters"></a><span data-ttu-id="2307d-138">Összetevő-paraméterek</span><span class="sxs-lookup"><span data-stu-id="2307d-138">Artifact parameters</span></span>
<span data-ttu-id="2307d-139">Hello paraméterek szakaszban hello definíciós fájl mely értékeket megadhatja, hogy egy felhasználói összetevő telepítésekor adja meg.</span><span class="sxs-lookup"><span data-stu-id="2307d-139">In hello parameters section of hello definition file, you specify which values a user can input when installing an artifact.</span></span> <span data-ttu-id="2307d-140">Olvassa el a hello összetevő telepítési parancs toothese értékeit.</span><span class="sxs-lookup"><span data-stu-id="2307d-140">You can refer toothese values in hello artifact install command.</span></span>

<span data-ttu-id="2307d-141">A következő struktúra hello definiálhatja a paramétereket:</span><span class="sxs-lookup"><span data-stu-id="2307d-141">You define parameters with hello following structure:</span></span>

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| <span data-ttu-id="2307d-142">Elem neve</span><span class="sxs-lookup"><span data-stu-id="2307d-142">Element name</span></span> | <span data-ttu-id="2307d-143">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="2307d-143">Required?</span></span> | <span data-ttu-id="2307d-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="2307d-144">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2307d-145">type</span><span class="sxs-lookup"><span data-stu-id="2307d-145">type</span></span> |<span data-ttu-id="2307d-146">Igen</span><span class="sxs-lookup"><span data-stu-id="2307d-146">Yes</span></span> |<span data-ttu-id="2307d-147">A paraméter értékének típusa.</span><span class="sxs-lookup"><span data-stu-id="2307d-147">Type of parameter value.</span></span> <span data-ttu-id="2307d-148">Tekintse meg a következő lista az engedélyezett típusok hello hello:</span><span class="sxs-lookup"><span data-stu-id="2307d-148">See hello following list for hello allowed types:</span></span> |
| <span data-ttu-id="2307d-149">displayName</span><span class="sxs-lookup"><span data-stu-id="2307d-149">displayName</span></span> |<span data-ttu-id="2307d-150">Igen</span><span class="sxs-lookup"><span data-stu-id="2307d-150">Yes</span></span> |<span data-ttu-id="2307d-151">Megjelenített tooa felhasználói hello laborban hello paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="2307d-151">Name of hello parameter that is displayed tooa user in hello lab.</span></span> | |
| <span data-ttu-id="2307d-152">leírás</span><span class="sxs-lookup"><span data-stu-id="2307d-152">description</span></span> |<span data-ttu-id="2307d-153">Igen</span><span class="sxs-lookup"><span data-stu-id="2307d-153">Yes</span></span> |<span data-ttu-id="2307d-154">Hello paraméter hello labor megjelenő leírása.</span><span class="sxs-lookup"><span data-stu-id="2307d-154">Description of hello parameter that is displayed in hello lab.</span></span> |

<span data-ttu-id="2307d-155">hello engedélyezett típusok:</span><span class="sxs-lookup"><span data-stu-id="2307d-155">hello allowed types are:</span></span>

* <span data-ttu-id="2307d-156">karakterlánc – bármilyen érvényes JSON-karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2307d-156">string – any valid JSON string</span></span>
* <span data-ttu-id="2307d-157">int – bármely JSON érvényes egész szám</span><span class="sxs-lookup"><span data-stu-id="2307d-157">int – any valid JSON integer</span></span>
* <span data-ttu-id="2307d-158">logikai – bármilyen érvényes JSON logikai</span><span class="sxs-lookup"><span data-stu-id="2307d-158">bool – any valid JSON Boolean</span></span>
* <span data-ttu-id="2307d-159">a tömb – bármilyen érvényes JSON-tömb</span><span class="sxs-lookup"><span data-stu-id="2307d-159">array – any valid JSON array</span></span>

## <a name="artifact-expressions-and-functions"></a><span data-ttu-id="2307d-160">Összetevő kifejezések és funkciók</span><span class="sxs-lookup"><span data-stu-id="2307d-160">Artifact expressions and functions</span></span>
<span data-ttu-id="2307d-161">Kifejezés használható, és funkciók tooconstruct hello összetevő telepítési parancs.</span><span class="sxs-lookup"><span data-stu-id="2307d-161">You can use expression and functions tooconstruct hello artifact install command.</span></span>
<span data-ttu-id="2307d-162">Kifejezések parancsfájlblokkjában találhatók szögletes zárójelbe ([és]), és a kiértékelésük hello összetevő telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="2307d-162">Expressions are enclosed with brackets ([ and ]), and are evaluated when hello artifact is installed.</span></span> <span data-ttu-id="2307d-163">Kifejezések bárhol megjelenhet egy JSON karakterláncértéket, és mindig adja vissza egy másik JSON-érték.</span><span class="sxs-lookup"><span data-stu-id="2307d-163">Expressions can appear anywhere in a JSON string value and always return another JSON value.</span></span> <span data-ttu-id="2307d-164">Ha szüksége van-e a zárójel kezdetű szövegkonstansnak toouse [, használjon két zárójeleket [[.</span><span class="sxs-lookup"><span data-stu-id="2307d-164">If you need toouse a literal string that starts with a bracket [, you must use two brackets [[.</span></span>
<span data-ttu-id="2307d-165">Általában kifejezések használata funkciók tooconstruct értéket.</span><span class="sxs-lookup"><span data-stu-id="2307d-165">Typically, you use expressions with functions tooconstruct a value.</span></span> <span data-ttu-id="2307d-166">Csakúgy, mint a JavaScript, függvényhívások-ként formázott functionName(arg1,arg2,arg3).</span><span class="sxs-lookup"><span data-stu-id="2307d-166">Just like in JavaScript, function calls are formatted as functionName(arg1,arg2,arg3).</span></span>

<span data-ttu-id="2307d-167">hello alábbi lista mutatja azokat közös funkciók:</span><span class="sxs-lookup"><span data-stu-id="2307d-167">hello following list shows common functions:</span></span>

* <span data-ttu-id="2307d-168">parameters(parameterName) - hello összetevő parancs futtatásakor megadott paraméter értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2307d-168">parameters(parameterName) - Returns a parameter value that is provided when hello artifact command is run.</span></span>
* <span data-ttu-id="2307d-169">(arg1, arg2, arg3,...) - összefűzés több karakterlánc-értékek egyesíti.</span><span class="sxs-lookup"><span data-stu-id="2307d-169">concat(arg1,arg2,arg3, …..) -     Combines multiple string values.</span></span> <span data-ttu-id="2307d-170">Ez a funkció tetszőleges számú argumentumot is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="2307d-170">This function can take any number of arguments.</span></span>

<span data-ttu-id="2307d-171">a következő példa azt mutatja meg hogyan hello toouse kifejezés és funkciók tooconstruct értéket:</span><span class="sxs-lookup"><span data-stu-id="2307d-171">hello following example shows how toouse expression and functions tooconstruct a value:</span></span>

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a><span data-ttu-id="2307d-172">Hozzon létre egy egyéni összetevő</span><span class="sxs-lookup"><span data-stu-id="2307d-172">Create a custom artifact</span></span>
<span data-ttu-id="2307d-173">Az egyéni eltérés létrehozása a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="2307d-173">Create your custom artifact by following these steps:</span></span>

1. <span data-ttu-id="2307d-174">JSON-szerkesztő telepítése, mert egy JSON-szerkesztő toowork összetevő-definíciós fájlokat, a szükséges.</span><span class="sxs-lookup"><span data-stu-id="2307d-174">Install a JSON editor - You need a JSON editor toowork with artifact definition files.</span></span> <span data-ttu-id="2307d-175">Azt javasoljuk, [Visual Studio Code](https://code.visualstudio.com/), amely érhető el a Windows, Linux vagy OS X.</span><span class="sxs-lookup"><span data-stu-id="2307d-175">We recommend using [Visual Studio Code](https://code.visualstudio.com/), which is available for Windows, Linux and OS X.</span></span>
2. <span data-ttu-id="2307d-176">Egy minta artifactfile.json - hello összetevők kivételét hozta létre Azure DevTest Labs csapatának Get a [GitHub-tárházban](https://github.com/Azure/azure-devtestlab), ahol létrehoztunk egy gazdag gyűjteményének összetevők, amelyek segítenek a saját összetevők létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2307d-176">Get a sample artifactfile.json - Check out hello artifacts created by Azure DevTest Labs team at our [GitHub repository](https://github.com/Azure/azure-devtestlab), where we have created a rich library of artifacts that help you create your own artifacts.</span></span> <span data-ttu-id="2307d-177">Töltse le egy összetevő csomagdefiníciós fájlt, és győződjön módosítások tooit toocreate saját összetevők.</span><span class="sxs-lookup"><span data-stu-id="2307d-177">Download an artifact definition file and make changes tooit toocreate your own artifacts.</span></span>
3. <span data-ttu-id="2307d-178">Ellenőrizze, hogy az IntelliSense - emelés IntelliSense toosee érvényes elemek, amelyek lehetnek használt tooconstruct használja egy összetevő csomagdefiníciós fájlt.</span><span class="sxs-lookup"><span data-stu-id="2307d-178">Make use of IntelliSense - Leverage IntelliSense toosee valid elements that can be used tooconstruct an artifact definition file.</span></span> <span data-ttu-id="2307d-179">Hello különböző lehetőségek közül értékek elem is megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="2307d-179">You can also see hello different options for values of an element.</span></span> <span data-ttu-id="2307d-180">Például a Windows vagy Linux két választási lehetőség hello az hello szerkesztésekor IntelliSense megjelenítése **targetOsType** elemet.</span><span class="sxs-lookup"><span data-stu-id="2307d-180">For example, IntelliSense show you hello two choices of Windows or Linux when editing hello **targetOsType** element.</span></span>
4. <span data-ttu-id="2307d-181">A tároló hello összetevő egy [git-tárház](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="2307d-181">Store hello artifact in a [git repository](devtest-lab-add-artifact-repo.md).</span></span>
   
   1. <span data-ttu-id="2307d-182">Hozzon létre egy külön könyvtárat minden összetevő, ahol hello könyvtárnév van hello azonos hello összetevő neveként.</span><span class="sxs-lookup"><span data-stu-id="2307d-182">Create a separate directory for each artifact where hello directory name is hello same as hello artifact name.</span></span>
   2. <span data-ttu-id="2307d-183">Létrehozott hello könyvtárban tárolják a hello összetevő definíciós fájlja (artifactfile.json).</span><span class="sxs-lookup"><span data-stu-id="2307d-183">Store hello artifact definition file (artifactfile.json) in hello directory you created.</span></span>
   3. <span data-ttu-id="2307d-184">A hello összetevő hivatkozott tároló hello parancsfájlok telepítési parancs.</span><span class="sxs-lookup"><span data-stu-id="2307d-184">Store hello scripts that are referenced from hello artifact install command.</span></span>
      
      <span data-ttu-id="2307d-185">Itt látható egy példa hogyan nézhet ki egy összetevő mappára:</span><span class="sxs-lookup"><span data-stu-id="2307d-185">Here is an example of how an artifact folder might look:</span></span>
      
      ![Összetevő git-tárház példa](./media/devtest-lab-artifact-author/git-repo.png)
5. <span data-ttu-id="2307d-187">Hello összetevők tárház toohello labor hozzáadása – tekintse meg a toohello cikk [vegyen fel egy Git-tárházat összetevők és sablonok](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="2307d-187">Add hello artifacts repository toohello lab - Refer toohello article, [Add a Git repository for artifacts and templates](devtest-lab-add-artifact-repo.md).</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a><span data-ttu-id="2307d-188">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="2307d-188">Related articles</span></span>
* [<span data-ttu-id="2307d-189">Hogyan toodiagnose összetevő hibák a DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="2307d-189">How toodiagnose artifact failures in DevTest Labs</span></span>](devtest-lab-troubleshoot-artifact-failure.md)
* [<span data-ttu-id="2307d-190">Csatlakozás a virtuális gép tooexisting resource manager-sablon használata az Azure DevTest Labs AD-tartomány</span><span class="sxs-lookup"><span data-stu-id="2307d-190">Join a VM tooexisting AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="2307d-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2307d-191">Next steps</span></span>
* <span data-ttu-id="2307d-192">Ismerje meg, hogyan túl[Git összetevő tárház tooa labor hozzáadása](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="2307d-192">Learn how too[add a Git artifact repository tooa lab](devtest-lab-add-artifact-repo.md).</span></span>

