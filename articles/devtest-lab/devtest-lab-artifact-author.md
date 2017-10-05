---
title: "Egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális Géphez |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhatnak létre a saját összetevők DevTest Labs szolgáltatásban való használatra"
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
ms.openlocfilehash: 2412033daa1d97860dd9f380178622b1ddc590c0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a><span data-ttu-id="e4661-103">Egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális Géphez</span><span class="sxs-lookup"><span data-stu-id="e4661-103">Create custom artifacts for your DevTest Labs VM</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a><span data-ttu-id="e4661-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e4661-104">Overview</span></span>
<span data-ttu-id="e4661-105">**Az összetevők** segítségével telepítheti és konfigurálhatja az alkalmazás, egy virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="e4661-105">**Artifacts** are used to deploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="e4661-106">Összetevő egy összetevő csomagdefiníciós fájlt, és más parancsfájl egy git-tárházban mappában tárolt fájlok áll.</span><span class="sxs-lookup"><span data-stu-id="e4661-106">An artifact consists of an artifact definition file and other script files that are stored in a folder in a git repository.</span></span> <span data-ttu-id="e4661-107">Összetevő definíciós fájlok tartalmazzák a JSON, és adja meg szeretné telepíteni a virtuális gép használó kifejezéseket.</span><span class="sxs-lookup"><span data-stu-id="e4661-107">Artifact definition files consist of JSON and expressions that you can use to specify what you want to install on a VM.</span></span> <span data-ttu-id="e4661-108">Például megadhatja összetevő futtatandó parancsot és paramétereket, elérhetővé válnak a parancs futtatásakor nevét.</span><span class="sxs-lookup"><span data-stu-id="e4661-108">For example, you can define the name of an artifact, command to run, and parameters that are made available when the command is run.</span></span> <span data-ttu-id="e4661-109">Más parancsfájlok belül az összetevő-definíciós fájlt nevű hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="e4661-109">You can refer to other script files within the artifact definition file by name.</span></span>

## <a name="artifact-definition-file-format"></a><span data-ttu-id="e4661-110">Összetevő csomagdefiníciós fájlformátumról</span><span class="sxs-lookup"><span data-stu-id="e4661-110">Artifact definition file format</span></span>
<span data-ttu-id="e4661-111">A következő példa bemutatja az alapvető szerkezete egy csomagdefiníciós fájl szakaszait:</span><span class="sxs-lookup"><span data-stu-id="e4661-111">The following example shows the sections that make up the basic structure of a definition file:</span></span>

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

| <span data-ttu-id="e4661-112">Elem neve</span><span class="sxs-lookup"><span data-stu-id="e4661-112">Element name</span></span> | <span data-ttu-id="e4661-113">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="e4661-113">Required?</span></span> | <span data-ttu-id="e4661-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="e4661-114">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e4661-115">$schema</span><span class="sxs-lookup"><span data-stu-id="e4661-115">$schema</span></span> |<span data-ttu-id="e4661-116">Nem</span><span class="sxs-lookup"><span data-stu-id="e4661-116">No</span></span> |<span data-ttu-id="e4661-117">A JSON-fájl, amely segít a tesztelése érvényességét, a csomagdefiníciós fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="e4661-117">Location of the JSON schema file that helps in testing the validity of the definition file.</span></span> |
| <span data-ttu-id="e4661-118">Cím</span><span class="sxs-lookup"><span data-stu-id="e4661-118">title</span></span> |<span data-ttu-id="e4661-119">Igen</span><span class="sxs-lookup"><span data-stu-id="e4661-119">Yes</span></span> |<span data-ttu-id="e4661-120">A labor megjelenő összetevő nevét.</span><span class="sxs-lookup"><span data-stu-id="e4661-120">Name of the artifact displayed in the lab.</span></span> |
| <span data-ttu-id="e4661-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="e4661-121">description</span></span> |<span data-ttu-id="e4661-122">Igen</span><span class="sxs-lookup"><span data-stu-id="e4661-122">Yes</span></span> |<span data-ttu-id="e4661-123">A labor megjelenő összetevő leírása.</span><span class="sxs-lookup"><span data-stu-id="e4661-123">Description of the artifact displayed in the lab.</span></span> |
| <span data-ttu-id="e4661-124">iconUri</span><span class="sxs-lookup"><span data-stu-id="e4661-124">iconUri</span></span> |<span data-ttu-id="e4661-125">Nem</span><span class="sxs-lookup"><span data-stu-id="e4661-125">No</span></span> |<span data-ttu-id="e4661-126">Ikon, a laborban URI azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="e4661-126">Uri of the icon displayed in the lab.</span></span> |
| <span data-ttu-id="e4661-127">targetOsType</span><span class="sxs-lookup"><span data-stu-id="e4661-127">targetOsType</span></span> |<span data-ttu-id="e4661-128">Igen</span><span class="sxs-lookup"><span data-stu-id="e4661-128">Yes</span></span> |<span data-ttu-id="e4661-129">Operációs rendszer a virtuális gép, amelyen telepítve van-e az összetevő.</span><span class="sxs-lookup"><span data-stu-id="e4661-129">Operating system of the VM where artifact is installed.</span></span> <span data-ttu-id="e4661-130">Támogatott beállítások: a Windows és Linux.</span><span class="sxs-lookup"><span data-stu-id="e4661-130">Supported options are: Windows and Linux.</span></span> |
| <span data-ttu-id="e4661-131">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="e4661-131">parameters</span></span> |<span data-ttu-id="e4661-132">Nem</span><span class="sxs-lookup"><span data-stu-id="e4661-132">No</span></span> |<span data-ttu-id="e4661-133">Olyan értékek által biztosított összetevő telepítési parancs futtatásakor a gépen.</span><span class="sxs-lookup"><span data-stu-id="e4661-133">Values that are provided when artifact install command is run on a machine.</span></span> <span data-ttu-id="e4661-134">Ez segít a összetevő testreszabása.</span><span class="sxs-lookup"><span data-stu-id="e4661-134">This helps in customizing your artifact.</span></span> |
| <span data-ttu-id="e4661-135">ParancsFuttatása</span><span class="sxs-lookup"><span data-stu-id="e4661-135">runCommand</span></span> |<span data-ttu-id="e4661-136">Igen</span><span class="sxs-lookup"><span data-stu-id="e4661-136">Yes</span></span> |<span data-ttu-id="e4661-137">Összetevő telepítése egy virtuális gépen végrehajtott parancs.</span><span class="sxs-lookup"><span data-stu-id="e4661-137">Artifact install command that is executed on a VM.</span></span> |

### <a name="artifact-parameters"></a><span data-ttu-id="e4661-138">Összetevő-paraméterek</span><span class="sxs-lookup"><span data-stu-id="e4661-138">Artifact parameters</span></span>
<span data-ttu-id="e4661-139">A csomagdefiníciós fájl paraméterek szakaszban adja meg egy felhasználói összetevő telepítése során is adjon meg értékeket.</span><span class="sxs-lookup"><span data-stu-id="e4661-139">In the parameters section of the definition file, you specify which values a user can input when installing an artifact.</span></span> <span data-ttu-id="e4661-140">Ezek az értékek összetevő telepítési parancs hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="e4661-140">You can refer to these values in the artifact install command.</span></span>

<span data-ttu-id="e4661-141">Megadhatja az alábbi szerkezettel paramétereket:</span><span class="sxs-lookup"><span data-stu-id="e4661-141">You define parameters with the following structure:</span></span>

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| <span data-ttu-id="e4661-142">Elem neve</span><span class="sxs-lookup"><span data-stu-id="e4661-142">Element name</span></span> | <span data-ttu-id="e4661-143">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="e4661-143">Required?</span></span> | <span data-ttu-id="e4661-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="e4661-144">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e4661-145">type</span><span class="sxs-lookup"><span data-stu-id="e4661-145">type</span></span> |<span data-ttu-id="e4661-146">Igen</span><span class="sxs-lookup"><span data-stu-id="e4661-146">Yes</span></span> |<span data-ttu-id="e4661-147">A paraméter értékének típusa.</span><span class="sxs-lookup"><span data-stu-id="e4661-147">Type of parameter value.</span></span> <span data-ttu-id="e4661-148">Tekintse meg a következő lista az engedélyezett típusok:</span><span class="sxs-lookup"><span data-stu-id="e4661-148">See the following list for the allowed types:</span></span> |
| <span data-ttu-id="e4661-149">displayName</span><span class="sxs-lookup"><span data-stu-id="e4661-149">displayName</span></span> |<span data-ttu-id="e4661-150">Igen</span><span class="sxs-lookup"><span data-stu-id="e4661-150">Yes</span></span> |<span data-ttu-id="e4661-151">A paraméter, a laborban a felhasználó számára megjelenített neve.</span><span class="sxs-lookup"><span data-stu-id="e4661-151">Name of the parameter that is displayed to a user in the lab.</span></span> | |
| <span data-ttu-id="e4661-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="e4661-152">description</span></span> |<span data-ttu-id="e4661-153">Igen</span><span class="sxs-lookup"><span data-stu-id="e4661-153">Yes</span></span> |<span data-ttu-id="e4661-154">A paraméter, a laborban megjelenített leírása.</span><span class="sxs-lookup"><span data-stu-id="e4661-154">Description of the parameter that is displayed in the lab.</span></span> |

<span data-ttu-id="e4661-155">Az engedélyezett típusok a következők:</span><span class="sxs-lookup"><span data-stu-id="e4661-155">The allowed types are:</span></span>

* <span data-ttu-id="e4661-156">karakterlánc – bármilyen érvényes JSON-karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e4661-156">string – any valid JSON string</span></span>
* <span data-ttu-id="e4661-157">int – bármely JSON érvényes egész szám</span><span class="sxs-lookup"><span data-stu-id="e4661-157">int – any valid JSON integer</span></span>
* <span data-ttu-id="e4661-158">logikai – bármilyen érvényes JSON logikai</span><span class="sxs-lookup"><span data-stu-id="e4661-158">bool – any valid JSON Boolean</span></span>
* <span data-ttu-id="e4661-159">a tömb – bármilyen érvényes JSON-tömb</span><span class="sxs-lookup"><span data-stu-id="e4661-159">array – any valid JSON array</span></span>

## <a name="artifact-expressions-and-functions"></a><span data-ttu-id="e4661-160">Összetevő kifejezések és funkciók</span><span class="sxs-lookup"><span data-stu-id="e4661-160">Artifact expressions and functions</span></span>
<span data-ttu-id="e4661-161">Kifejezés használható, és funkciók összeállításához összetevő telepítési parancs.</span><span class="sxs-lookup"><span data-stu-id="e4661-161">You can use expression and functions to construct the artifact install command.</span></span>
<span data-ttu-id="e4661-162">Kifejezések parancsfájlblokkjában találhatók szögletes zárójelbe ([és]), és a kiértékelésük, ha az összetevő telepítve van.</span><span class="sxs-lookup"><span data-stu-id="e4661-162">Expressions are enclosed with brackets ([ and ]), and are evaluated when the artifact is installed.</span></span> <span data-ttu-id="e4661-163">Kifejezések bárhol megjelenhet egy JSON karakterláncértéket, és mindig adja vissza egy másik JSON-érték.</span><span class="sxs-lookup"><span data-stu-id="e4661-163">Expressions can appear anywhere in a JSON string value and always return another JSON value.</span></span> <span data-ttu-id="e4661-164">Ha szeretné-e használni a zárójel kezdetű szövegkonstansnak [, használjon két zárójeleket [[.</span><span class="sxs-lookup"><span data-stu-id="e4661-164">If you need to use a literal string that starts with a bracket [, you must use two brackets [[.</span></span>
<span data-ttu-id="e4661-165">Általában akkor használják kifejezések függvényekkel érték létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e4661-165">Typically, you use expressions with functions to construct a value.</span></span> <span data-ttu-id="e4661-166">Csakúgy, mint a JavaScript, függvényhívások-ként formázott functionName(arg1,arg2,arg3).</span><span class="sxs-lookup"><span data-stu-id="e4661-166">Just like in JavaScript, function calls are formatted as functionName(arg1,arg2,arg3).</span></span>

<span data-ttu-id="e4661-167">A következő lista mutatja azokat a közös funkciók:</span><span class="sxs-lookup"><span data-stu-id="e4661-167">The following list shows common functions:</span></span>

* <span data-ttu-id="e4661-168">parameters(parameterName) - egy paraméterérték a összetevő parancs futtatásakor megadott ad vissza.</span><span class="sxs-lookup"><span data-stu-id="e4661-168">parameters(parameterName) - Returns a parameter value that is provided when the artifact command is run.</span></span>
* <span data-ttu-id="e4661-169">(arg1, arg2, arg3,...) - összefűzés több karakterlánc-értékek egyesíti.</span><span class="sxs-lookup"><span data-stu-id="e4661-169">concat(arg1,arg2,arg3, …..) -     Combines multiple string values.</span></span> <span data-ttu-id="e4661-170">Ez a funkció tetszőleges számú argumentumot is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="e4661-170">This function can take any number of arguments.</span></span>

<span data-ttu-id="e4661-171">A következő példa bemutatja, hogyan kifejezés és funkciók használatával hozható létre érték:</span><span class="sxs-lookup"><span data-stu-id="e4661-171">The following example shows how to use expression and functions to construct a value:</span></span>

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a><span data-ttu-id="e4661-172">Hozzon létre egy egyéni összetevő</span><span class="sxs-lookup"><span data-stu-id="e4661-172">Create a custom artifact</span></span>
<span data-ttu-id="e4661-173">Az egyéni eltérés létrehozása a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="e4661-173">Create your custom artifact by following these steps:</span></span>

1. <span data-ttu-id="e4661-174">JSON-szerkesztő telepítése, mert egy JSON-szerkesztővel összetevő definíciós fájlok van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e4661-174">Install a JSON editor - You need a JSON editor to work with artifact definition files.</span></span> <span data-ttu-id="e4661-175">Azt javasoljuk, [Visual Studio Code](https://code.visualstudio.com/), amely érhető el a Windows, Linux vagy OS X.</span><span class="sxs-lookup"><span data-stu-id="e4661-175">We recommend using [Visual Studio Code](https://code.visualstudio.com/), which is available for Windows, Linux and OS X.</span></span>
2. <span data-ttu-id="e4661-176">Egy minta artifactfile.json - kivétel az összetevők hozta létre Azure DevTest Labs csapatának Get a [GitHub-tárházban](https://github.com/Azure/azure-devtestlab), ahol létrehoztunk egy gazdag gyűjteményének összetevők, amelyek segítenek a saját összetevők létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e4661-176">Get a sample artifactfile.json - Check out the artifacts created by Azure DevTest Labs team at our [GitHub repository](https://github.com/Azure/azure-devtestlab), where we have created a rich library of artifacts that help you create your own artifacts.</span></span> <span data-ttu-id="e4661-177">Töltse le egy összetevő csomagdefiníciós fájlt, és módosítja a saját összetevők létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e4661-177">Download an artifact definition file and make changes to it to create your own artifacts.</span></span>
3. <span data-ttu-id="e4661-178">Ellenőrizze, hogy az IntelliSense - emelés IntelliSense egy összetevő csomagdefiníciós fájl létrehozásához használható érvényes elemek megjelenítéséhez használja.</span><span class="sxs-lookup"><span data-stu-id="e4661-178">Make use of IntelliSense - Leverage IntelliSense to see valid elements that can be used to construct an artifact definition file.</span></span> <span data-ttu-id="e4661-179">Megtekintheti a különböző lehetőségek közül értékek elem is.</span><span class="sxs-lookup"><span data-stu-id="e4661-179">You can also see the different options for values of an element.</span></span> <span data-ttu-id="e4661-180">Például az IntelliSense megjelenítése, két választási lehetőség a Windows vagy Linux szerkesztésekor a **targetOsType** elemet.</span><span class="sxs-lookup"><span data-stu-id="e4661-180">For example, IntelliSense show you the two choices of Windows or Linux when editing the **targetOsType** element.</span></span>
4. <span data-ttu-id="e4661-181">Az összetevő tárolni egy [git-tárház](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="e4661-181">Store the artifact in a [git repository](devtest-lab-add-artifact-repo.md).</span></span>
   
   1. <span data-ttu-id="e4661-182">Hozzon létre egy külön könyvtárat minden összetevő, ahol a könyvtár neve megegyezik az összetevő neve.</span><span class="sxs-lookup"><span data-stu-id="e4661-182">Create a separate directory for each artifact where the directory name is the same as the artifact name.</span></span>
   2. <span data-ttu-id="e4661-183">Az összetevő-definíciós fájlt (artifactfile.json) tárolja a könyvtárban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="e4661-183">Store the artifact definition file (artifactfile.json) in the directory you created.</span></span>
   3. <span data-ttu-id="e4661-184">A parancsprogramok, az összetevő telepítési parancs hivatkozott tárolja.</span><span class="sxs-lookup"><span data-stu-id="e4661-184">Store the scripts that are referenced from the artifact install command.</span></span>
      
      <span data-ttu-id="e4661-185">Itt látható egy példa hogyan nézhet ki egy összetevő mappára:</span><span class="sxs-lookup"><span data-stu-id="e4661-185">Here is an example of how an artifact folder might look:</span></span>
      
      ![Összetevő git-tárház példa](./media/devtest-lab-artifact-author/git-repo.png)
5. <span data-ttu-id="e4661-187">Az összetevők tárház hozzáadása a laborkörnyezetben – tekintse meg a cikk [vegyen fel egy Git-tárházat összetevők és sablonok](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="e4661-187">Add the artifacts repository to the lab - Refer to the article, [Add a Git repository for artifacts and templates](devtest-lab-add-artifact-repo.md).</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a><span data-ttu-id="e4661-188">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="e4661-188">Related articles</span></span>
* [<span data-ttu-id="e4661-189">A DevTest Labs szolgáltatásban összetevő hibák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="e4661-189">How to diagnose artifact failures in DevTest Labs</span></span>](devtest-lab-troubleshoot-artifact-failure.md)
* [<span data-ttu-id="e4661-190">A virtuális gép csatlakoztatása a meglévő Active Directory-tartománynak a resource manager-sablon használatával a Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="e4661-190">Join a VM to existing AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="e4661-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4661-191">Next steps</span></span>
* <span data-ttu-id="e4661-192">Megtudhatja, hogyan [egy Git összetevőtárban hozzáadása egy laborhoz](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="e4661-192">Learn how to [add a Git artifact repository to a lab](devtest-lab-add-artifact-repo.md).</span></span>

