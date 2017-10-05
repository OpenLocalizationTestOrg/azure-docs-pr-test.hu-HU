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
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a>Egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális Géphez
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Áttekintés
**Az összetevők** segítségével telepítheti és konfigurálhatja az alkalmazás, egy virtuális gép kiépítése után. Összetevő egy összetevő csomagdefiníciós fájlt, és más parancsfájl egy git-tárházban mappában tárolt fájlok áll. Összetevő definíciós fájlok tartalmazzák a JSON, és adja meg szeretné telepíteni a virtuális gép használó kifejezéseket. Például megadhatja összetevő futtatandó parancsot és paramétereket, elérhetővé válnak a parancs futtatásakor nevét. Más parancsfájlok belül az összetevő-definíciós fájlt nevű hivatkozhat.

## <a name="artifact-definition-file-format"></a>Összetevő csomagdefiníciós fájlformátumról
A következő példa bemutatja az alapvető szerkezete egy csomagdefiníciós fájl szakaszait:

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

| Elem neve | Kötelező megadni? | Leírás |
| --- | --- | --- |
| $schema |Nem |A JSON-fájl, amely segít a tesztelése érvényességét, a csomagdefiníciós fájl helyét. |
| Cím |Igen |A labor megjelenő összetevő nevét. |
| Leírás |Igen |A labor megjelenő összetevő leírása. |
| iconUri |Nem |Ikon, a laborban URI azonosítóját. |
| targetOsType |Igen |Operációs rendszer a virtuális gép, amelyen telepítve van-e az összetevő. Támogatott beállítások: a Windows és Linux. |
| Paraméterek |Nem |Olyan értékek által biztosított összetevő telepítési parancs futtatásakor a gépen. Ez segít a összetevő testreszabása. |
| ParancsFuttatása |Igen |Összetevő telepítése egy virtuális gépen végrehajtott parancs. |

### <a name="artifact-parameters"></a>Összetevő-paraméterek
A csomagdefiníciós fájl paraméterek szakaszban adja meg egy felhasználói összetevő telepítése során is adjon meg értékeket. Ezek az értékek összetevő telepítési parancs hivatkozhat.

Megadhatja az alábbi szerkezettel paramétereket:

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Elem neve | Kötelező megadni? | Leírás |
| --- | --- | --- |
| type |Igen |A paraméter értékének típusa. Tekintse meg a következő lista az engedélyezett típusok: |
| displayName |Igen |A paraméter, a laborban a felhasználó számára megjelenített neve. | |
| Leírás |Igen |A paraméter, a laborban megjelenített leírása. |

Az engedélyezett típusok a következők:

* karakterlánc – bármilyen érvényes JSON-karakterlánc
* int – bármely JSON érvényes egész szám
* logikai – bármilyen érvényes JSON logikai
* a tömb – bármilyen érvényes JSON-tömb

## <a name="artifact-expressions-and-functions"></a>Összetevő kifejezések és funkciók
Kifejezés használható, és funkciók összeállításához összetevő telepítési parancs.
Kifejezések parancsfájlblokkjában találhatók szögletes zárójelbe ([és]), és a kiértékelésük, ha az összetevő telepítve van. Kifejezések bárhol megjelenhet egy JSON karakterláncértéket, és mindig adja vissza egy másik JSON-érték. Ha szeretné-e használni a zárójel kezdetű szövegkonstansnak [, használjon két zárójeleket [[.
Általában akkor használják kifejezések függvényekkel érték létrehozásához. Csakúgy, mint a JavaScript, függvényhívások-ként formázott functionName(arg1,arg2,arg3).

A következő lista mutatja azokat a közös funkciók:

* parameters(parameterName) - egy paraméterérték a összetevő parancs futtatásakor megadott ad vissza.
* (arg1, arg2, arg3,...) - összefűzés több karakterlánc-értékek egyesíti. Ez a funkció tetszőleges számú argumentumot is igénybe vehet.

A következő példa bemutatja, hogyan kifejezés és funkciók használatával hozható létre érték:

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Hozzon létre egy egyéni összetevő
Az egyéni eltérés létrehozása a következő lépések végrehajtásával:

1. JSON-szerkesztő telepítése, mert egy JSON-szerkesztővel összetevő definíciós fájlok van szüksége. Azt javasoljuk, [Visual Studio Code](https://code.visualstudio.com/), amely érhető el a Windows, Linux vagy OS X.
2. Egy minta artifactfile.json - kivétel az összetevők hozta létre Azure DevTest Labs csapatának Get a [GitHub-tárházban](https://github.com/Azure/azure-devtestlab), ahol létrehoztunk egy gazdag gyűjteményének összetevők, amelyek segítenek a saját összetevők létrehozása. Töltse le egy összetevő csomagdefiníciós fájlt, és módosítja a saját összetevők létrehozásához.
3. Ellenőrizze, hogy az IntelliSense - emelés IntelliSense egy összetevő csomagdefiníciós fájl létrehozásához használható érvényes elemek megjelenítéséhez használja. Megtekintheti a különböző lehetőségek közül értékek elem is. Például az IntelliSense megjelenítése, két választási lehetőség a Windows vagy Linux szerkesztésekor a **targetOsType** elemet.
4. Az összetevő tárolni egy [git-tárház](devtest-lab-add-artifact-repo.md).
   
   1. Hozzon létre egy külön könyvtárat minden összetevő, ahol a könyvtár neve megegyezik az összetevő neve.
   2. Az összetevő-definíciós fájlt (artifactfile.json) tárolja a könyvtárban létrehozott.
   3. A parancsprogramok, az összetevő telepítési parancs hivatkozott tárolja.
      
      Itt látható egy példa hogyan nézhet ki egy összetevő mappára:
      
      ![Összetevő git-tárház példa](./media/devtest-lab-artifact-author/git-repo.png)
5. Az összetevők tárház hozzáadása a laborkörnyezetben – tekintse meg a cikk [vegyen fel egy Git-tárházat összetevők és sablonok](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a>Kapcsolódó cikkek
* [A DevTest Labs szolgáltatásban összetevő hibák diagnosztizálása](devtest-lab-troubleshoot-artifact-failure.md)
* [A virtuális gép csatlakoztatása a meglévő Active Directory-tartománynak a resource manager-sablon használatával a Azure DevTest Labs szolgáltatásban](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Következő lépések
* Megtudhatja, hogyan [egy Git összetevőtárban hozzáadása egy laborhoz](devtest-lab-add-artifact-repo.md).

