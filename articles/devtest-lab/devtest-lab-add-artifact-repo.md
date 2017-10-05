---
title: "A Git-tárház hozzáadása egy tesztkörnyezetet a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Adja hozzá az egyéni összetevők az adatforrásból a Githubból vagy a Visual Studio Team Services Git tára Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 01b459f7-eaf2-45a8-b4b5-2c0a821b33c8
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: 053f92a65f9ae29154d471fd22ee842620b4f273
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="add-a-git-repository-to-store-custom-artifacts-and-azure-resource-manager-templates"></a>A Git-tárház tárolásához egyéni összetevők és az Azure Resource Manager-sablonok hozzáadása

Ha azt szeretné, hogy [egyéni összetevők létrehozása](devtest-lab-artifact-author.md) a tesztkörnyezetben lévő virtuális géphez vagy [Azure Resource Manager-sablonok segítségével hozzon létre egy egyéni tesztkörnyezetben](devtest-lab-create-environment-from-arm.md), is fel kell vennie egy privát Git-tárház felvenni a az összetevők vagy a csoport létrehozó Azure Resource Manager-sablonok. A tárház az alábbiakon tárolható [GitHub](https://github.com) vagy a [Visual Studio Team Services (VSTS)](https://visualstudio.com).

Adtunk egy [Github-tárházban műtermék](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) , amely telepítheti-, vagy testre szabhatja a tesztkörnyezetekhez. Testre szabhatja, vagy hozzon létre egy összetevő, nem a nyilvános tárház tárolása – létre kell hoznia a saját személyes tárházban. 

Ha a virtuális gép létrehozása az Azure Resource Manager sablon mentése, testre szabhatja, ha szeretné, és majd a segítségével később könnyen hozzanak létre további virtuális gépek. Létre kell hoznia a saját személyes adattár a egyéni Azure Resource Manager-sablonok.  

* A GitHub-tárház létrehozásához, lásd: [GitHub Bootcamp](https://help.github.com/categories/bootcamp/).
* A Git-tárház Team Services-projekt létrehozása, lásd: [kapcsolódás a Visual Studio Team Services](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online).

Az alábbi képernyőfelvételen szemlélteti, hogyan nézhet ki egy tárház összetevők tartalmazó a Githubon:  
![Példa GitHub-tárház összetevők](./media/devtest-lab-add-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-the-repository-information-and-credentials"></a>A tárház információkért és a hitelesítő adatok
A tárház hozzáadása a labor, be kell szereznie bizonyos adatokat a tárházból. A következő szakaszok végigvezetik ezt az információt a Githubon és a Visual Studio Team Services üzemeltetett adattárak beolvasásakor.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>A GitHub tárház klón URL-cím és a személyes hozzáférési jogkivonat beszerzése
A GitHub-tárház klón URL-cím és a személyes hozzáférési jogkivonat beszerzéséhez kövesse az alábbi lépéseket:

1. Tallózással keresse meg a GitHub-tárházban, amely tartalmazza az összetevő vagy Azure Resource Manager sablon definíciók kezdőlapján.
2. Válassza ki **Klónozás vagy letöltési**.
3. Kattintson a gombra, majd másolja a **HTTPS-klón URL-címét** a vágólapra, majd mentse az URL-cím későbbi használatra.
4. Válassza ki a olyan profil képe GitHub jobb felső sarkában, majd **beállítások**.
5. Az a **személyes beállítások** a bal oldalon válassza a menü **személyes hozzáférési jogkivonatok**.
6. Válassza ki **új Generate token**.
7. A a **új személyes hozzáférési jogkivonat** lapján adjon meg egy **leírás Token**, fogadja el az alapértelmezett elemek a **hatókörök kiválasztása**, és válassza a **azonosító létrehozása** .
8. A generált jogkivonat akkor menteni, mert később szüksége.
9. Most már bezárhatja a GitHub.   
10. Továbbra is a [a labor csatlakozni a tárház](#connect-your-lab-to-the-repository) szakasz.

### <a name="get-the-visual-studio-team-services-repository-clone-url-and-personal-access-token"></a>A Visual Studio Team Services tárház klón URL-cím és a személyes hozzáférési jogkivonat beszerzése
Ahhoz, hogy a Visual Studio Team Services tárház klón URL-cím és a személyes hozzáférési jogkivonat, kövesse az alábbi lépéseket:

1. Nyissa meg a csoport gyűjtemény kezdőlapján (például `https://contoso-web-team.visualstudio.com`), majd válassza ki a projekthez.
2. A projekt kezdőlapján válassza **kód**.
3. A klón URL-cím meg a projekt **kód** lapon jelölje be **Klónozás**.
4. Az URL-cím mentése az oktatóanyag későbbi részében szüksége.
5. Személyes hozzáférési jogkivonat létrehozásához válasszon **saját profil** a felhasználói fiók legördülő menüből.
6. A profil információ lapon válassza az **biztonsági**.
7. Az a **biztonsági** lapon jelölje be **Hozzáadás**.
8. Az a **személyes hozzáférési jogkivonat létrehozása** lap:

   * Adjon meg egy **leírás** a jogkivonat esetében.
   * Válassza ki **180 nap** a a **lejár a** listája.
   * Válasszon **elérhető fiókokhoz** a a **fiókok** listája.
   * Válassza ki a **minden hatókör** lehetőséget.
   * Válasszon **jogkivonat létrehozása**.
9. Befejeztével, az új jogkivonatot megjelenik a **személyes hozzáférési jogkivonatok** listája. Válassza ki **másolási Token**, majd mentse a token értéknek a későbbi használatra.
10. Továbbra is a [a labor csatlakozni a tárház](#connect-your-lab-to-the-repository) szakasz.

## <a name="connect-your-lab-to-the-repository"></a>A tesztkörnyezet csatlakozni a tárházban
1. Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** a listából.
3. Válassza ki a kívánt labor labs listájának megtekintéséhez.   
4. A bal oldali panelen, jelölje ki **konfigurációs és házirendek**.
5. A tesztlabor a **konfigurációs és házirendek** területen válassza **Tárházak**.
6. Az a **Tárházak** területen válassza **+ Hozzáadás**.

    ![Adja hozzá a tárház gomb](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
7. A második **Tárházak** lapján adja meg a következőket:

   * **Név** -adja meg a tárház nevét.
   * **Git-klón URL-cím** -adja meg a Git HTTPS klón URL-cím már korábban átmásolt a Githubból vagy a Visual Studio Team Services.
   * **Fiókirodai** -adja meg a definíciók beolvasásához használt ág.
   * **Személyes hozzáférési jogkivonat** -adja meg a korábban beszerzett Githubból vagy a Visual Studio Team Services személyes hozzáférési jogkivonat.
   * **Mappák elérési útjaiban** -adjon meg legalább egy mappa elérési útja, a klón URL-cím, amely tartalmazza az összetevő vagy Azure Resource Manager sablon definíciók viszonyítva. Egy alkönyvtárban megadásakor győződjön meg arról, a perjel szerepeljenek a mappa elérési útját.

     ![Adattárak terület](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
8. Kattintson a **Mentés** gombra.

## <a name="next-steps"></a>Következő lépések
Privát Git-tárház létrehozása után a a következők valamelyikét teheti igényeitől függően:
* Tároló a [egyéni összetevők](devtest-lab-artifact-author.md), amelyek később használhatja új virtuális gépek létrehozásához.
* [Hozzon létre virtuális Gépre kiterjedő környezetek és PaaS-erőforrások Azure Resource Manager-sablonok](devtest-lab-create-environment-from-arm.md) majd tárolja a sablonok a személyes tárházban.

Amikor egy virtuális Gépet hoz létre, ellenőrizheti, hogy az összetevők vagy a sablonok hozzáadódnak a Git-tárház. Elérhetők, azonnal tükröződjön a listában, összetevők vagy a sablonok, a titkos tárház, amely megadja azt a forrást az oszlopban látható nevével. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="related-blog-posts"></a>Kapcsolódó blogbejegyzések
* [Sikertelen Azure DevTest Labs szolgáltatásban lévő hibaelhárítása](devtest-lab-troubleshoot-artifact-failure.md)
* [A virtuális gép csatlakoztatása a meglévő Active Directory-tartománynak a resource manager-sablon használatával a Azure DevTest Labs szolgáltatásban](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)
