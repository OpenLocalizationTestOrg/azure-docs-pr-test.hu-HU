---
title: "aaaCreate PaaS-erőforrások Azure Resource Manager-sablonok és virtuális Gépre kiterjedő környezetben |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate PaaS erőforrások a Azure DevTest Labs szolgáltatásban az Azure Resource Manager-sablonok és virtuális Gépre kiterjedő környezetben"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: tarcher
ms.openlocfilehash: ab8628f6cb5a666435258efb93921ec69ad3a13a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Hozzon létre virtuális Gépre kiterjedő környezetek és PaaS-erőforrások Azure Resource Manager-sablonok

Hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) lehetővé teszi a tooeasily [létrehozása és hozzáadása a virtuális gép tooa labor](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-add-vm). Egyszerre csak egy virtuális gép létrehozása esetén ez működik. Azonban ha hello környezet több virtuális gépeket tartalmaz, minden virtuális gép egyénileg létrehozott toobe rendelkezik. Például egy többrétegű webalkalmazást vagy a SharePoint-farm forgatókönyvek esetén mechanizmusra több virtuális gép egyetlen lépésben hello létrehozásához szükséges tooallow. Azure Resource Manager-sablonok használatával most a hello infrastruktúráját és konfigurációját az Azure-megoldás meghatározása, és több virtuális gépek konzisztens állapotban ismételten telepítheti. Ez a funkció hello a következő előnyöket biztosítja:

- Az Azure Resource Manager-sablonok töltődnek be közvetlenül a (GitHub vagy Team Services Git) a verziókövetési tárházzal.
- Beállítása után a felhasználók által az Azure Resource Manager sablon hello Azure-portálon végrehajtott műveleteket más, a kiadási hozhat létre egy olyan környezetben [VM körrel](./devtest-lab-comparing-vm-base-image-types.md).
- Azure PaaS-erőforrások kiépítése az Azure Resource Manager-sablon hozzáadása tooIaaS virtuális gépek a környezetben.
- a környezetek hello költség hello labor hozzáadása tooindividual hozta létre alapjait más típusú virtuális gépek követhető nyomon.
- PaaS erőforrások jönnek létre, és megjelennek a költségkövetést; virtuális gép automatikus leállítási azonban nem alkalmazza a tooPaaS erőforrásokat.
- Felhasználó rendelkezik ugyanazon VM szabályzatvezérlést környezetek hello single-tesztlabor virtuális gépek rendelkeznek.

További tudnivalók hello számos [előnyei a Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) toodeploy, frissítése vagy törlése a labor erőforrásokat egy művelettel.

> [!NOTE]
> Ha használja a Resource Manager-sablon egy alap toocreate további labor virtuális gépeken, nincsenek néhány különbség tookeep szem előtt tartva e Multi-virtuális gépek vagy egyetlen virtuális gépek létrehozásakor. A virtuális gép használata Azure Resource Manager sablon ezek a különbségek részletesebben ismerteti.
>
>

## <a name="configure-azure-resource-manager-template-repositories"></a>Azure Resource Manager sablon adattárak konfigurálása

Hello rendelkezésre álló infrastruktúra a kódot, és a konfigurációs, kód, a környezet sablonok gyakorlati tanácsok a verziókövetési rendszerrel kell kezelni. Azure DevTest Labs Ez az eljárás a következő, és betölti az összes Azure Resource Manager sablon közvetlenül a Githubból vagy VSTS Git tárházak a. Ennek eredményeképpen a Resource Manager-sablonok használható hello teljes kiadás ciklust, a hello teszt toohello üzemi környezet között.

Nincsenek szabályok toofollow tooorganize néhány az Azure Resource Manager-sablonok tárházban:

- hello fő sablonfájl névvel kell ellátni `azuredeploy.json`. 

    ![Kulcs Azure Resource Manager sablon fájlok](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Ha azt szeretné, hogy a paraméter fájlban definiált paraméterértékek toouse, a hello paraméterfájl névvel kell ellátni `azuredeploy.parameters.json`.
- Hello paramétereket használhatja `_artifactsLocation` és `_artifactsLocationSasToken` tooconstruct hello parametersLink URI azonosítóját, lehetővé téve a DevTest Labs tooautomatically kezelése beágyazott sablonok. Lásd: [hogyan Azure DevTest Labs egyszerűbbé teszi a beágyazott erőforrás-kezelő sablon központi telepítések tesztelési környezetben](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/) további információt.
- Metaadatok lehet meghatározott toospecify hello sablon megjelenítendő neve és leírása. A metaadatok nevű fájlba kell `metadata.json`. hello alábbi példa metaadatfájl bemutatja, hogyan jelenjen meg a toospecify hello a nevet és leírást: 

```json
{
 
"itemDisplayName": "<your template name>",
 
"description": "<description of hello template>"
 
}
```

hello következő lépések végigvezetik hello Azure-portál használatával tárház tooyour labor hozzáadása. 

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
1. Labs hello listában jelölje ki hello kívánt labor.   
1. Hello labor paneljén válassza **konfigurációs és házirendek**.

    ![Konfigurációs és házirendek](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. A hello **konfigurációs és házirendek** beállítások listáról válassza ki **Tárházak**. Hello **Tárházak** panel hello tárházak találhatók, amelyek hozzá vannak adva toohello labor sorolja fel. A tárház nevű `Public Repo` összes labs automatikusan létrejön, és kapcsolódik toohello [DevTest Labs GitHub-tárház](https://github.com/Azure/azure-devtestlab) , amely tartalmazza a virtuális gép összetevők a használatra.

    ![Nyilvános tárház](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Válassza ki **Add +** tooadd az Azure Resource Manager sablon tárházba.
1. Ha a második hello **Tárházak** panel nyílik meg hello szükséges információkat az alábbiak szerint:
    - **Név** -hello labor használt hello tárház nevét adja meg.
    - **Git-klón URL-cím** -a Githubból vagy a Visual Studio Team Services hello GIT HTTPS-klón URL-cím megadása.  
    - **Fiókirodai** -adja meg a hello fiókirodai neve tooaccess az Azure Resource Manager sablon definíciókat. 
    - **Személyes hozzáférési jogkivonat** -hello személyes hozzáférési jogkivonat toosecurely hozzáférni a tárházban. tooget a Visual Studio Team Services, a token kiválasztása  **&lt;sajatNev >> saját profil > biztonsági > nyilvános hozzáférési jogkivonat**. tooget a jogkivonatot a Githubból, válassza ki a profilképet kiválasztása után **beállítások > nyilvános hozzáférési jogkivonat**. 
    - **Mappák elérési útjaiban** – hello két beviteli mezőt, egyikének használatával adja meg a hello mappa elérési útját, - / - perjellel kezdődik, és relatív tooyour Git-klón URI tooeither az összetevő-definíciók (első beviteli mezőt) vagy az Azure Resource Manager-sablon definíciók.   
    
        ![Nyilvános tárház](./media/devtest-lab-create-environment-from-arm/repo-values.png)

1. Amennyiben az összes szükséges hello mezők van-e megadva, és hello érvényesíteni, válassza ki a **mentése**.

hello a következő szakasz végigvezeti környezetek létrehozása az Azure Resource Manager sablon alapján.

## <a name="create-an-environment-from-an-azure-resource-manager-template"></a>Környezet létrehozása az Azure Resource Manager-sablonok

Miután az Azure Resource Manager sablon tárház hello labor lett konfigurálva, a labor felhasználók olyan Azure-portált használja az alábbi lépésekkel hello környezetet hozhat létre:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
1. Labs hello listában jelölje ki hello kívánt labor.   
1. Hello labor paneljén válassza **Add +**.
1. Hello **base válasszon** panel hello alap képeket először felsorolt hello Azure Resource Manager-sablonok használatával jeleníti meg. Jelölje be hello Azure Resource Manager sablon szükséges.

    ![Válasszon egy alapja](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. A hello **Hozzáadás** panelen adja meg a hello **környezet neve** érték. hello környezet értéke Újdonságok megjelenített tooyour felhasználók hello tesztkörnyezetben. hello Azure Resource Manager sablon hello fennmaradó beviteli mezők vannak definiálva. Ha alapértelmezett értékek meg vannak-e határozva hello sablon vagy hello `azuredeploy.parameter.json` fájl jelen, az alapértelmezett értékek megjelenítése a beviteli mezők. Típusú paraméterek *biztonságos karakterlánc*, használhatja hello laborban tárolt hello titkok [titkos tárolójának](https://azure.microsoft.com/en-us/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store).

    ![Hozzáadása panel](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Nincsenek több paraméter - még akkor is, ha a megadott – a megjelenített értékek üres értékeket. Ezért, ha a felhasználók hozzárendelése az Azure Resource Manager-sablonok ezen értékek tooparameters, DevTest Labs nem hello értékek megjelenítése; helyette üres, ha a labor felhasználók hello kell tooenter beviteli mezők értéket megjelenítő hello környezet létrehozásakor.
    > 
    > - GENERÁCIÓS EGYEDI
    > - -EGYEDI - GENERÁCIÓS [N]
    > - GENERÁCIÓS SSH-PUB-KULCS
    > - GENERÁCIÓS-JELSZÓ 
 
1. Válassza ki **Hozzáadás** toocreate hello környezetben. hello környezetben kezdődik hello megjelenő hello állapotú azonnal kiépítés **a virtuális gépek** listája. Új erőforráscsoport automatikusan létrehozta hello labor tooprovision hello Azure Resource Manager sablon definiált összes hello erőforrást.
1. Hello környezet létrehozása után válassza hello környezet hello **a virtuális gépek** tooopen hello erőforráscsoport panel listában, és keresse meg az összes hello környezetben kiosztott hello erőforrásokat.
    
    ![A virtuális gépek listája](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   Bontsa ki a hello környezet tooview csak hello lista által hello környezetben telepített virtuális gépek is.
    
    ![A virtuális gépek listája](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Kattintson bármelyik hello környezetek tooview hello elérhető műveletek - például adatlemezek, változó automatikus leállítási ideje, és több, az összetevők alkalmazása.

    ![Környezet műveletek](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="next-steps"></a>Következő lépések
* A virtuális gép létrehozása után toohello VM kapcsolódhatnak kiválasztásával **Connect** hello VM panelen.
* Megtekinthető és kezelhető környezetben erőforrások hello hello környezet kiválasztásával **a virtuális gépek** lista a tesztkörnyezetben. 
* Fedezze fel hello [Azure Resource Manager sablonok Azure gyors üzembe helyezési sablon gyűjteményből](https://github.com/Azure/azure-quickstart-templates)
