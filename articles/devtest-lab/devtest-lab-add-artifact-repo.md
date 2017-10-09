---
title: "aaaAdd egy Git tárház tooa, amikor a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
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
ms.openlocfilehash: e590559ffb2d497e39823e35c3f66974f42f13c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-git-repository-toostore-custom-artifacts-and-azure-resource-manager-templates"></a>A Git tárház toostore egyéni összetevők és az Azure Resource Manager-sablonok hozzáadása

Ha azt szeretné, túl[egyéni összetevők létrehozása](devtest-lab-artifact-author.md) a virtuális gépek hello a tesztkörnyezetben vagy [használata Azure Resource Manager sablonok toocreate egy egyéni tesztkörnyezetben](devtest-lab-create-environment-from-arm.md), is fel kell vennie egy privát Git-tárház tooinclude hello összetevők vagy a csoport létrehozó Azure Resource Manager-sablonok. hello tárház az alábbiakon tárolható [GitHub](https://github.com) vagy a [Visual Studio Team Services (VSTS)](https://visualstudio.com).

Adtunk egy [Github-tárházban műtermék](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) , amely telepítheti-, vagy testre szabhatja a tesztkörnyezetekhez. Testre szabhatja, vagy hozzon létre egy összetevő, nem hello nyilvános tárház tárolása – létre kell hoznia a saját személyes tárházban. 

A virtuális gépek létrehozásakor hello Azure Resource Manager sablon mentése, testre szabhatja, ha szeretné, és azt használja újabb tooeasily további virtuális gépek létrehozása. A saját személyes tárház toostore az egyéni Azure Resource Manager-sablonokat kell létrehoznia.  

* Hogyan toocreate egy GitHub-tárházban: toolearn [GitHub Bootcamp](https://help.github.com/categories/bootcamp/).
* Hogyan toocreate Team Services projekttel ezen a Git-tárház: toolearn [tooVisual Studio Team Services csatlakozás](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online).

hello alábbi képernyőfelvételen szemlélteti, hogyan nézhet ki egy tárház összetevők tartalmazó a Githubon:  
![Példa GitHub-tárház összetevők](./media/devtest-lab-add-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-hello-repository-information-and-credentials"></a>Hello tárház információkért és a hitelesítő adatok
tárház tooyour labor tooadd, be kell szereznie bizonyos adatokat a tárházból. a következő szakaszok hello végigvezetik ezt az információt a Githubon és a Visual Studio Team Services üzemeltetett adattárak beolvasásakor.

### <a name="get-hello-github-repository-clone-url-and-personal-access-token"></a>Hello GitHub tárház klón URL-cím és a személyes hozzáférési jogkivonat beszerzése
tooget hello GitHub tárház klón URL-cím és a személyes hozzáférési jogkivonat, kövesse az alábbi lépéseket:

1. Keresse meg a kezdőlapján toohello hello GitHub tárház, amely hello összetevő vagy Azure Resource Manager sablon definíciókat tartalmazza.
2. Válassza ki **Klónozás vagy letöltési**.
3. Jelölje be hello gomb toocopy hello **HTTPS-klón URL-címét** toohello vágólapra, és a későbbi használatra hello URL-cím mentése.
4. Válassza ki a hello olyan profil képe GitHub hello jobb felső sarkában, majd **beállítások**.
5. A hello **személyes beállítások** menü hello balra, a select **személyes hozzáférési jogkivonatok**.
6. Válassza ki **új Generate token**.
7. A hello **új személyes hozzáférési jogkivonat** lapján adja meg egy **leírás Token**, fogadja el az alapértelmezett elemek hello hello **hatókörök kiválasztása**, és válassza a **létrehozása Token**.
8. Menteni a generált hello token, mert később szüksége.
9. Most már bezárhatja a GitHub.   
10. Továbbra is toohello [csatlakozzon a labor toohello tárház](#connect-your-lab-to-the-repository) szakasz.

### <a name="get-hello-visual-studio-team-services-repository-clone-url-and-personal-access-token"></a>Hello Visual Studio Team Services tárház klón URL-cím és a személyes hozzáférési jogkivonat beszerzése
tooget hello Visual Studio Team Services tárház klón URL-cím és a személyes hozzáférési jogkivonat, kövesse az alábbi lépéseket:

1. A csapat gyűjtemény kezdőlapján nyissa meg hello (például `https://contoso-web-team.visualstudio.com`), majd válassza ki a projekthez.
2. A hello projekt kezdőlapján válassza **kód**.
3. tooview hello klón URL-cím, hello projekt **kód** lapon jelölje be **Klónozás**.
4. Az oktatóanyag későbbi részében szüksége mentse a hello URL-CÍMÉT.
5. a személyes hozzáférési jogkivonat, toocreate kiválasztása **saját profil** hello felhasználói fiók legördülő menüből.
6. Hello profil információ lapon jelölje be **biztonsági**.
7. A hello **biztonsági** lapon jelölje be **Hozzáadás**.
8. A hello **személyes hozzáférési jogkivonat létrehozása** lap:

   * Adjon meg egy **leírás** hello jogkivonat.
   * Válassza ki **180 nap** a hello **lejár a** listája.
   * Válasszon **elérhető fiókokhoz** a hello **fiókok** lista.
   * Válassza ki a hello **minden hatókör** lehetőséget.
   * Válasszon **jogkivonat létrehozása**.
9. Befejeztével hello új jogkivonat megjelenik-e a hello **személyes hozzáférési jogkivonatok** listája. Válassza ki **másolási Token**, majd mentse a hello token értéknek a későbbi használatra.
10. Továbbra is toohello [csatlakozzon a labor toohello tárház](#connect-your-lab-to-the-repository) szakasz.

## <a name="connect-your-lab-toohello-repository"></a>Csatlakozás a labor toohello tárház
1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
3. Labs hello listában jelölje ki hello kívánt labor.   
4. Hello bal oldali panelen, jelölje ki **konfigurációs és házirendek**.
5. A hello labor **konfigurációs és házirendek** területen válassza **Tárházak**.
6. A hello **Tárházak** területen válassza **+ Hozzáadás**.

    ![Adja hozzá a tárház gomb](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
7. A hello második **Tárházak** csoportjában adja meg a következő információ hello:

   * **Név** -hello tárház nevét adja meg.
   * **Git-klón URL-cím** -hello Git HTTPS klón URL-cím már korábban átmásolt a Githubból vagy a Visual Studio Team Services adja meg.
   * **Fiókirodai** -adja meg a hello fiókirodai tooget a definíciók.
   * **Személyes hozzáférési jogkivonat** -adja meg a Githubból vagy a Visual Studio Team Services korábban beszerzett hello személyes hozzáférési jogkivonat.
   * **Mappák elérési útjaiban** -adjon meg legalább egy mappa elérési útja relatív toohello klón URL-cím, amely az összetevő vagy Azure Resource Manager sablon definíciókat tartalmazza. Egy alkönyvtárban megadásakor győződjön meg arról, hogy tooinclude hello törtvonallal hello mappájának elérési.

     ![Adattárak terület](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
8. Kattintson a **Mentés** gombra.

## <a name="next-steps"></a>Következő lépések
Privát Git-tárház létrehozása után egyik vagy mindkét hello következő teheti igényeitől függően:
* Tároló a [egyéni összetevők](devtest-lab-artifact-author.md), amely újabb toocreate használható új virtuális gépeket.
* [Hozzon létre virtuális Gépre kiterjedő környezetek és PaaS-erőforrások Azure Resource Manager-sablonok](devtest-lab-create-environment-from-arm.md) majd tárolja a személyes tárházban hello sablonok.

Amikor létrehoz egy virtuális Gépet, ellenőrizheti, hogy hello összetevők vagy a sablonok tooyour Git-tárház hozzáadott. Azonnal hello nevet, a titkos tárház, amely meghatározza a hello forrás hello oszlopban látható az összetevők vagy a sablonok, hello listáját rendelkezésre áll. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="related-blog-posts"></a>Kapcsolódó blogbejegyzések
* [Hogyan tootroubleshoot Azure DevTest Labs szolgáltatásban lévő sikertelen](devtest-lab-troubleshoot-artifact-failure.md)
* [Csatlakozás a virtuális gép tooexisting resource manager-sablon használata az Azure DevTest Labs AD-tartomány](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)
