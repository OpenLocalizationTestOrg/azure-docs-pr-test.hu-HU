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
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a>Egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális Géphez
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Áttekintés
**Az összetevők** használt toodeploy, és állítsa be az alkalmazását, a virtuális gép kiépítése után. Összetevő egy összetevő csomagdefiníciós fájlt, és más parancsfájl egy git-tárházban mappában tárolt fájlok áll. Összetevő-definíciós fájlok tartalmazzák a JSON és használható toospecify mit kifejezések tooinstall a virtuális gép. Például megadhatja hello nevét összetevő, toorun parancsot és paramétereket, amelyeket közzétettek hello parancs futtatásakor. Parancsfájlok tooother hello összetevő definíciós fájlból olvassa el a neve.

## <a name="artifact-definition-file-format"></a>Összetevő csomagdefiníciós fájlformátumról
hello alábbi példa bemutatja egy csomagdefiníciós fájl alapvető szerkezete hello hello szakaszait:

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

| Elem neve | Kötelező? | Leírás |
| --- | --- | --- |
| $schema |Nem |Hello JSON séma-fájl helyét, amelyek segítségével a tesztelése hello hello csomagdefiníciós fájl érvényességét. |
| Cím |Igen |Hello összetevő hello labor megjelenített neve. |
| leírás |Igen |Hello labor megjelenő hello összetevő leírása. |
| iconUri |Nem |URI-je hello ikon, hello tesztkörnyezetben. |
| targetOsType |Igen |Operációs rendszer a virtuális gép, amelyen telepítve van-e az összetevő hello. Támogatott beállítások: a Windows és Linux. |
| paraméterek |Nem |Olyan értékek által biztosított összetevő telepítési parancs futtatásakor a gépen. Ez segít a összetevő testreszabása. |
| ParancsFuttatása |Igen |Összetevő telepítése egy virtuális gépen végrehajtott parancs. |

### <a name="artifact-parameters"></a>Összetevő-paraméterek
Hello paraméterek szakaszban hello definíciós fájl mely értékeket megadhatja, hogy egy felhasználói összetevő telepítésekor adja meg. Olvassa el a hello összetevő telepítési parancs toothese értékeit.

A következő struktúra hello definiálhatja a paramétereket:

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Elem neve | Kötelező? | Leírás |
| --- | --- | --- |
| type |Igen |A paraméter értékének típusa. Tekintse meg a következő lista az engedélyezett típusok hello hello: |
| displayName |Igen |Megjelenített tooa felhasználói hello laborban hello paraméter neve. | |
| leírás |Igen |Hello paraméter hello labor megjelenő leírása. |

hello engedélyezett típusok:

* karakterlánc – bármilyen érvényes JSON-karakterlánc
* int – bármely JSON érvényes egész szám
* logikai – bármilyen érvényes JSON logikai
* a tömb – bármilyen érvényes JSON-tömb

## <a name="artifact-expressions-and-functions"></a>Összetevő kifejezések és funkciók
Kifejezés használható, és funkciók tooconstruct hello összetevő telepítési parancs.
Kifejezések parancsfájlblokkjában találhatók szögletes zárójelbe ([és]), és a kiértékelésük hello összetevő telepítésekor. Kifejezések bárhol megjelenhet egy JSON karakterláncértéket, és mindig adja vissza egy másik JSON-érték. Ha szüksége van-e a zárójel kezdetű szövegkonstansnak toouse [, használjon két zárójeleket [[.
Általában kifejezések használata funkciók tooconstruct értéket. Csakúgy, mint a JavaScript, függvényhívások-ként formázott functionName(arg1,arg2,arg3).

hello alábbi lista mutatja azokat közös funkciók:

* parameters(parameterName) - hello összetevő parancs futtatásakor megadott paraméter értéket adja vissza.
* (arg1, arg2, arg3,...) - összefűzés több karakterlánc-értékek egyesíti. Ez a funkció tetszőleges számú argumentumot is igénybe vehet.

a következő példa azt mutatja meg hogyan hello toouse kifejezés és funkciók tooconstruct értéket:

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Hozzon létre egy egyéni összetevő
Az egyéni eltérés létrehozása a következő lépések végrehajtásával:

1. JSON-szerkesztő telepítése, mert egy JSON-szerkesztő toowork összetevő-definíciós fájlokat, a szükséges. Azt javasoljuk, [Visual Studio Code](https://code.visualstudio.com/), amely érhető el a Windows, Linux vagy OS X.
2. Egy minta artifactfile.json - hello összetevők kivételét hozta létre Azure DevTest Labs csapatának Get a [GitHub-tárházban](https://github.com/Azure/azure-devtestlab), ahol létrehoztunk egy gazdag gyűjteményének összetevők, amelyek segítenek a saját összetevők létrehozása. Töltse le egy összetevő csomagdefiníciós fájlt, és győződjön módosítások tooit toocreate saját összetevők.
3. Ellenőrizze, hogy az IntelliSense - emelés IntelliSense toosee érvényes elemek, amelyek lehetnek használt tooconstruct használja egy összetevő csomagdefiníciós fájlt. Hello különböző lehetőségek közül értékek elem is megtekinthető. Például a Windows vagy Linux két választási lehetőség hello az hello szerkesztésekor IntelliSense megjelenítése **targetOsType** elemet.
4. A tároló hello összetevő egy [git-tárház](devtest-lab-add-artifact-repo.md).
   
   1. Hozzon létre egy külön könyvtárat minden összetevő, ahol hello könyvtárnév van hello azonos hello összetevő neveként.
   2. Létrehozott hello könyvtárban tárolják a hello összetevő definíciós fájlja (artifactfile.json).
   3. A hello összetevő hivatkozott tároló hello parancsfájlok telepítési parancs.
      
      Itt látható egy példa hogyan nézhet ki egy összetevő mappára:
      
      ![Összetevő git-tárház példa](./media/devtest-lab-artifact-author/git-repo.png)
5. Hello összetevők tárház toohello labor hozzáadása – tekintse meg a toohello cikk [vegyen fel egy Git-tárházat összetevők és sablonok](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Hogyan toodiagnose összetevő hibák a DevTest Labs szolgáltatásban](devtest-lab-troubleshoot-artifact-failure.md)
* [Csatlakozás a virtuális gép tooexisting resource manager-sablon használata az Azure DevTest Labs AD-tartomány](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[Git összetevő tárház tooa labor hozzáadása](devtest-lab-add-artifact-repo.md).

