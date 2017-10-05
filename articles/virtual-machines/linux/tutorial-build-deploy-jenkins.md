---
title: "CI/CD Jenkins az Azure virtuális gépek Team Services |} Microsoft Docs"
description: "Folyamatos integrációt (CI) és a folyamatos üzembe helyezés (CD) Jenkins segítségével Azure virtuális gépeken futó szolgáltatásban a Visual Studio Team Services (VSTS) vagy a Microsoft Team Foundation Server (TFS) a Node.js-alkalmazás beállítása"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/15/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: a40e26a8681df31fad664e4d1df4c1513311900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-team-services"></a>Az alkalmazás telepítése Linux virtuális gépek Jenkins és Team Services használatával

Folyamatos integrációt (CI) és a folyamatos üzembe helyezés (CD) nem egy folyamatot, amely létre, a kiadási és a kód telepítésére. Team Services számos teljes, teljes funkcionalitású CI/CD automatizálási eszközeivel központi telepítés az Azure-bA. Jenkins egy olyan népszerű 3. fél CI/CD server-alapú eszköz, amely CI/CD automation is biztosít. Segítségével mindkét együtt a felhőalapú alkalmazás vagy szolgáltatás biztosításához hogyan testreszabása.

Az oktatóanyag segítségével Jenkins összeállítása a **Node.js webalkalmazás**, és a Visual Studio Team Services telepítéséhez, úgy, hogy egy [üzembe helyezési csoport](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) Linux virtuális gépeket tartalmazó.

Az alábbiakat fogja elvégezni:

> [!div class="checklist"]
> * Az alkalmazás a Jenkins elkészítésére
> * Jenkins Team Services-integráció konfigurálása
> * Az Azure virtuális gépek telepítési csoport létrehozása
> * Hozzon létre olyan kiadási-definícióval, konfigurálja a virtuális gépeket, és telepíti az alkalmazást

## <a name="before-you-begin"></a>Előkészületek

* Hozzáférés Jenkins fiókra van szüksége. Ha még nem hozott létre egy Jenkins server, lásd: [Jenkins dokumentáció](https://jenkins.io/doc/). 

* Jelentkezzen be a Team Services-fiókhoz (`https://{youraccount}.visualstudio.com`). 
  Beszerezheti a [ingyenes Team Services-fiókot](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > További információkért lásd: [Team Services kapcsolódás](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

* Ha még nem rendelkezik egy, hozzon létre egy személyes hozzáférési jogkivonat (PAT) Team Services-fiókját. Jenkins a Team Services-fiók eléréséhez szükséges.
  Olvasási [hogyan hozható létre személyes hozzáférési jogkivonat Team Services és a TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) megtudhatja, hogyan hozhat létre egy.

## <a name="get-the-sample-app"></a>A minta-alkalmazás letöltése

Kell telepíteni kívánt alkalmazást, a Git-tárház tárolja.
A jelen oktatóanyag esetében javasoljuk, használjon [a mintaalkalmazás elérhető a Githubon](https://github.com/azooinmyluggage/fabrikam-node).

1. Hozzon létre egy elágazás az alkalmazáshoz, és tekintse meg a helye (URL) számára ez az oktatóanyag későbbi lépéseiben.

1. Ellenőrizze a elágazás **nyilvános** való egyszerűbben csatlakozhatnak a Githubon később.

> [!NOTE]
> További információkért lásd: [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/) és [nyilvános és titkos tárház](https://help.github.com/articles/making-a-private-repository-public/).

> [!NOTE]
> Az alkalmazás használatával lett létrehozva [Yeoman](http://yeoman.io/learning/index.html); használ **Express**, **bower**, és **grunt**; és van-e néhány **npm** csomagok szerint függőségeinek.
> A mintaalkalmazás tartalmaz [Azure Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) dinamikusan létrehozni a virtuális gépek telepítési Azure használt. A feladatok által használt ezeket a sablonokat a [Team Services kiadási definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).
> A fő sablont hoz létre a hálózati biztonsági csoport, a virtuális gépek és virtuális hálózat. A nyilvános IP-címet rendel, és megnyílik a 80-as portot a bejövő. Hozzáadja egy címkét, amellyel a központi telepítési csoport válassza ki a gépeket megkapná a központi telepítést is.
>
> A minta is tartalmaz egy parancsfájlt, amely beállítja a Nginx telepíti az alkalmazást. Az egyes virtuális gépek végrehajtása. Pontosabban a parancsfájl telepíti-e a csomópont, Nginx és PM2; Konfigurálja az Nginx és PM2; Ezután elindítja a csomópont-alkalmazást.

## <a name="configure-jenkins-plugins"></a>Jenkins beépülő modulok konfigurálása

Először konfigurálnia kell a két Jenkins beépülők **NodeJS** és **integráció a Team Services**.

1. Nyissa meg a Jenkins fiókját, és válassza a **kezelése Jenkins**.

1. Az a **kezelése Jenkins** lapon, válassza ki **kezelése beépülő modulok**.

1. Keresse meg a listát szűrheti a **NodeJS** beépülő modul és telepítse a restart záradék nélkül.

   ![A NodeJS beépülő modul hozzáadása Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. Szűrheti a listát a **a Team Foundation Server** beépülő modul és telepítéséhez. (Ez a Team Services és a Team Foundation Server a beépülő modul működik.) Jenkins újraindítása nincs szükség.

## <a name="configure-jenkins-build-for-nodejs"></a>A Node.js Jenkins build konfigurálása

A Jenkins hozzon létre egy új build projektet, és konfigurálja az alábbiak szerint:

1. Az a **általános** lapra, adja meg a build projekt nevét.

1. Az a **forrás kód felügyeleti** lapon jelölje be **Git** , és adja meg a tárház és az alkalmazás kódját tartalmazó fiókirodai részleteit.

   ![A tárház hozzáadása a build](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > Ha a tárház nem nyilvános, válasszon **Hozzáadás** adja meg hitelesítő adatokat a csatlakozáshoz.

1. Az a **Build eseményindítók** lapon jelölje be **lekérdezési SCM** , és írja be az ütemezés `H/03 * * * *` , és kérdezze le a Git-tárház változásainak három percenként. 

1. A a **telepítési környezet** lapon jelölje be **meg csomópont &amp; npm bin / mappa elérési ÚTJA** , és írja be `NodeJS` csomópont JS telepítés érték. Hagyja **npmrc fájl** "használja a rendszer alapértelmezett."

1. Az a **Build** lapra, adja meg a parancsot `npm install` annak biztosítása érdekében minden függőséget frissülnek.

## <a name="configure-jenkins-for-team-services-integration"></a>Jenkins Team Services-integráció konfigurálása

1. Az a **utáni műveletek** lapon, a **archiválására fájlok**, adja meg `**/*` összes fájlokról.

1. A **indul el, a TFS/Team Services kiadás**, adja meg a fiók teljes URL-CÍMÉT (például `https://your-account-name.visualstudio.com`), a projekt nevét, egy nevet a kiadási definition (később létre), és a fiókjához szükséges hitelesítő adatokat.
   A felhasználónév és a korábban létrehozott PAT van szüksége. 

   ![Jenkins utáni műveletek konfigurálása](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. A build projekt mentése.

## <a name="create-a-jenkins-service-endpoint"></a>Hozzon létre egy Jenkins szolgáltatási végpont

A szolgáltatási végpont lehetővé teszi, hogy a Team Services Jenkins való kapcsolódáshoz.

1. Nyissa meg a **szolgáltatások** Team Services, nyissa meg a lap a **új szolgáltatási végpont** listában, és válassza a **Jenkins**.

   ![Vegyen fel egy Jenkins végpontot](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. Adjon meg egy nevet, tekintse meg a kapcsolat használatával fogja.

1. Adja meg az URL-CÍMÉT a Jenkins kiszolgáló, és az osztásjelek a **nem megbízható az SSL-tanúsítványokat elfogadó** lehetőséget.

1. Adja meg a felhasználónevet és jelszót a Jenkins fiók.

1. Válasszon **kapcsolat ellenőrzéséhez** futtatásával ellenőrizze, hogy az információ helyes.

1. Válasszon **OK** a végpont létrehozásához.

## <a name="create-a-deployment-group"></a>Központi telepítési csoport létrehozása

Kell egy [üzembe helyezési csoport](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) magában foglalja a virtuális gépek.

1. Nyissa meg a **kiadásokban** lapján a **Build &amp; kiadás** hub, nyissa meg a **telepítési csoportban** lapot, és válassza a **+ új**.

1. Adjon meg egy nevet a központi telepítési csoportot, és opcionális leírását.
   Válassza a **létrehozása**.

Az Azure erőforrás-csoport központi telepítésének feladat hoz létre, és regisztrálja a virtuális gépek, amikor fut az Azure Resource Manager sablon használatával.
Nem kell létrehozni és regisztrálni a virtuális gépek magát.

## <a name="create-a-release-definition"></a>Egy kiadási definíció létrehozása

Egy kiadási definition határozza meg, a folyamat Team Services fogja végrehajtani az alkalmazás központi telepítése.
A kiadási definíció létrehozása Team Services:

1. Nyissa meg a **kiadásokban** lapján a **Build &amp; kiadási** központi, nyissa meg a  **+**  legördülő kiadás definíciók listáján, majd válassza ki a a  **Kiadás definíció létrehozása**. 

1. Válassza ki a **üres** sablon kiválasztása és **következő**.

1. Az a **összetevők** területen kattintson a **összetevő hivatkozás** válassza **Jenkins**. Válassza ki a Jenkins szolgáltatási végpont kapcsolatot. Ezután jelölje ki a Jenkins forrás feladatot, és válassza a **létrehozása**. 

1. Az új kiadási definícióját, válassza a **+ Hozzáadás feladatok** , és adja hozzá egy **Azure erőforrás-csoport központi telepítésének** feladat az alapértelmezett környezetre.

1. Válassza ki a legördülő nyílra mellett a **+ Hozzáadás feladatok** hivatkozásra, és adja hozzá a központi telepítési csoport szakaszok a definíció.

   ![A központi telepítési csoport szakaszok hozzáadása](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. A feladat-katalógusban, nyissa meg a **segédprogram** szakaszt, és adjon hozzá egy a **Héjparancsfájlt** feladat.

1. Az Azure erőforrás-csoport központi telepítése a feladatban használt paraméterek sablon állítja be a virtuális gépek való csatlakozáshoz használt rendszergazdai jelszavát.
   Ezt a jelszót adja meg a változó a **$(adminpassword)**:
   
   - Nyissa meg a **változók** lapon és a a **változók** területen adja meg a név `adminpassword`.

   - Adja meg a rendszergazdai jelszót.

   - Válassza a "lakat" ikont a érték szövegbeviteli mező a jelszó védelme mellett. 

## <a name="configure-the-azure-resource-group-deployment-task"></a>Az Azure erőforrás-csoport központi telepítésének tevékenység konfigurálása

A **Azure erőforrás-csoport központi telepítésének** feladattal az üzembe helyezési csoport létrehozásához. Konfigurálja úgy az alábbiak szerint:

* **Azure-előfizetés:** alatti listából válasszon ki egy kapcsolatot **Azure-szolgáltatás rendelkezésre álló kapcsolatok**. 
  Ha a kapcsolat nem jelenik meg, válassza a **kezelése**, jelölje be **új szolgáltatási végpont** majd **Azure Resource Manager**, és kövesse a megjelenő utasításokat.
  Térjen vissza a kiadási definícióját, frissítse a **AzureRM előfizetés** listában, és válassza ki a létrehozott kapcsolatot.

* **Erőforráscsoport**: Adjon meg egy korábban létrehozott erőforráscsoportot.

* **Hely**: Válasszon ki egy régiót a központi telepítéshez.

  ![Új erőforráscsoport létrehozása](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* **A sablon helye**:`URL of the file`

* **Sablon hivatkozás**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`

* **Sablon paraméterei link**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`

* **Sablon paraméterének**: listáját a felülbírálási értékeket, például: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.<br />Helyezze be a {helyőrzőket} saját egyedi értékeket. 

* **Előfeltételek engedélyezése**:`Configure with Deployment Group agent`

* **TFS/VSTS-végpont**: válasszon **Hozzáadás** , és válassza az "Új Team Foundation Server/Team Services-kapcsolat hozzáadása" párbeszédpanelen **jogkivonat-alapú hitelesítés**. Adja meg a kapcsolat nevét és a csapatprojekt URL-CÍMÉT. Ezután létre, és adja meg egy [személyes hozzáférési jogkivonat (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) a csapat projekthez a kapcsolat hitelesítéséhez.

  ![Hozzon létre egy személyes hozzáférési tokent](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* **Csapatprojekt**: válassza ki az aktuális projektben.

* **Üzembe helyezési csoport**: Adja meg a központi telepítési csoport néven közben használatos a **erőforráscsoport** paraméter.

Az alapértelmezett beállításokat az Azure erőforrás-csoport központi telepítésének feladathoz a következők: az erőforrás frissítése és fokozatosan ehhez. A feladat létrehozza a virtuális gépeket először akkor fut, és csak ezt követően frissítse azokat.

## <a name="configure-the-shell-script-task"></a>A PowerShell parancsfájlhoz tevékenység konfigurálása

A **Héjparancsfájlt** feladattal minden egyes kiszolgálón telepítse a Node.js, és indítsa el az alkalmazást futtatni kívánt parancsfájl konfigurációját adja meg. Konfigurálja úgy az alábbiak szerint:

* **Parancsfájl elérési útja**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`

* **Adjon meg működő Directory**:`Checked`

* **Munkakönyvtár**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
   
## <a name="rename-and-save-the-release-definition"></a>Nevezze át, és a kiadási definíció mentése

1. A kiadási-definíciót a megadott néven nevének módosítása a **utáni műveletek** Jenkins buildverziót lapján. Jenkins ezt a nevet fogja tudni indul el egy új kiadásban, ha a forrás-összetevők frissítése szükséges.

1. Igény szerint megváltoztathatja a környezet neve a nevére kattintva. 

1. Válasszon **mentése**, és válassza a **OK**.

## <a name="start-a-manual-deployment"></a>Indítsa el a manuális központi telepítése

1. Válassza ki **+ kiadási** válassza **létrehozása kiadási**.

1. A build befejezte a kijelölt legördülő listában jelölje ki, és válassza a **létrehozása**.

1. Adja meg a kiadási hivatkozás a felbukkanó üzenet. Például: "kiadás **Release-1** létrejött."

1. Nyissa meg a **naplók** a kiadás a konzol kimeneti bemutató fülre.

1. A böngészőben nyissa meg az URL-CÍMÉT a központi telepítés csoporthoz hozzáadott kiszolgálók egyikét. Adja meg például`http://{your-server-ip-address}`

## <a name="start-a-cicd-deployment"></a>Indítsa el a CI/CD központi telepítés

1. A kiadási-definícióban, törölje a jelet a **engedélyezve** jelölőnégyzet a **beállítások** az Azure erőforrás-csoport központi telepítésének feladat beállításai.
   Jövőbeli központi telepítésére a meglévő üzembe helyezési csoport nem kell újra végrehajtani a feladatot.

1. Nyissa meg a forrás Git-tárházban, és a tartalmának módosítása a **h1** fejléc a fájlban [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).

1. A módosítás véglegesítése.

1. Néhány perc elteltével látni fogja a létrehozott új verziót a **kiadásokban** Team Services vagy a TFS oldalán. Nyissa meg a verzió: a központi telepítése folyamatban. Gratulálunk!

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban az Azure-ban Jenkins build és a Team Services, a kiadáshoz alkalmazások központi telepítése automatikus. Megtudta, hogyan, hogy:

> [!div class="checklist"]
> * Az alkalmazás a Jenkins elkészítésére
> * Jenkins Team Services-integráció konfigurálása
> * Az Azure virtuális gépek telepítési csoport létrehozása
> * Hozzon létre olyan kiadási-definícióval, konfigurálja a virtuális gépeket, és telepíti az alkalmazást

A következő oktatóanyaghoz egy LÁMPA telepítésével kapcsolatos további előzetes (Linux, Apache, MySQL és a PHP) verem.

> [!div class="nextstepaction"]
> [LAMP stack telepítése](tutorial-lamp-stack.md)