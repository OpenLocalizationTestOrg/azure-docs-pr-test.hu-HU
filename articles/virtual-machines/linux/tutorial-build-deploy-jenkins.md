---
title: "aaaCI/CD-KET a virtuális gépek Team Services Jenkins tooAzure |} Microsoft Docs"
description: "Folyamatos integrációt (CI) és a folyamatos üzembe helyezés (CD) Jenkins tooAzure virtuális gépek a Kiadáskezelés a Visual Studio Team Services (VSTS) vagy a Microsoft Team Foundation Server (TFS) használata Node.js-alkalmazás beállítása"
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
ms.openlocfilehash: 400ae34cbdf45da65351811c0ff6ff5d61ef862c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-toolinux-vms-using-jenkins-and-team-services"></a>Az alkalmazás tooLinux virtuális gépek telepítése Jenkins és Team Services használatával

Folyamatos integrációt (CI) és a folyamatos üzembe helyezés (CD) nem egy folyamatot, amely létre, a kiadási és a kód telepítésére. Team Services biztosít egy teljes, teljes funkcionalitású CI/CD automatizálási eszközeivel telepítési tooAzure. Jenkins egy olyan népszerű 3. fél CI/CD server-alapú eszköz, amely CI/CD automation is biztosít. Mindkét együtt toocustomize is használhatja a felhőalapú alkalmazás vagy szolgáltatás biztosításához hogyan.

Ebben az oktatóanyagban Jenkins toobuild használhat egy **Node.js webalkalmazás**, és a Visual Studio Team Services toodeploy azt tooa [üzembe helyezési csoport](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) Linux virtuális gépeket tartalmazó.

Az alábbiakat fogja elvégezni:

> [!div class="checklist"]
> * Az alkalmazás a Jenkins elkészítésére
> * Jenkins Team Services-integráció konfigurálása
> * Egy központi telepítési csoport hello Azure virtuális gépek létrehozása
> * Konfigurálja a hello virtuális gépek és hello alkalmazást telepíti a kiadási-definíció létrehozása

## <a name="before-you-begin"></a>Előkészületek

* Hozzáférés tooa Jenkins fiók szükséges. Ha még nem hozott létre egy Jenkins server, lásd: [Jenkins dokumentáció](https://jenkins.io/doc/). 

* Jelentkezzen be tooyour Team Services-fiók (`https://{youraccount}.visualstudio.com`). 
  Beszerezheti a [ingyenes Team Services-fiókot](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > További információkért lásd: [Connect tooTeam szolgáltatások](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

* Ha még nem rendelkezik egy, hozzon létre egy személyes hozzáférési jogkivonat (PAT) Team Services-fiókját. Jenkins igényel az információk tooaccess Team Services-fiókját.
  Olvasási [hogyan hozható létre személyes hozzáférési jogkivonat Team Services és a TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) toolearn hogyan toogenerate egyet.

## <a name="get-hello-sample-app"></a>Hello minta alkalmazás letöltése

Egy alkalmazás toodeploy egy Git-tárház tárolva van szüksége.
A jelen oktatóanyag esetében javasoljuk, használjon [a mintaalkalmazás elérhető a Githubon](https://github.com/azooinmyluggage/fabrikam-node).

1. Hozzon létre egy elágazás az alkalmazás, és jegyezze fel a hello helye (URL) számára ez az oktatóanyag későbbi lépéseiben.

1. Ellenőrizze hello elágazás **nyilvános** toosimplify tooGitHub később csatlakoztatni.

> [!NOTE]
> További információkért lásd: [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/) és [nyilvános és titkos tárház](https://help.github.com/articles/making-a-private-repository-public/).

> [!NOTE]
> hello alkalmazás használatával lett létrehozva [Yeoman](http://yeoman.io/learning/index.html); használ **Express**, **bower**, és **grunt**; és van-e néhány **npm**szerint függőségeinek csomagok.
> hello mintaalkalmazás tartalmaz [Azure Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) , vannak használt toodynamically hozzon létre hello virtuális gépek telepítése az Azure-on. Ezek a sablonok hello feladatai által használt [Team Services kiadási definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).
> hello fő sablont hoz létre a hálózati biztonsági csoport, a virtuális gépek és virtuális hálózat. A nyilvános IP-címet rendel, és megnyílik a 80-as portot a bejövő. Hozzáadja a hello telepítési csoport túl válassza hello gépek tooreceive hello központi telepítése által használt címke is.
>
> hello minta is tartalmaz egy parancsfájlt, amely beállítja a Nginx hello alkalmazást telepíti. Az egyes virtuális gépek hello végrehajtása. Pontosabban telepíti az hello parancsfájl a csomópont, Nginx és PM2; Konfigurálja az Nginx és PM2; Ezután elindítja a hello csomópont alkalmazást.

## <a name="configure-jenkins-plugins"></a>Jenkins beépülő modulok konfigurálása

Először konfigurálnia kell a két Jenkins beépülők **NodeJS** és **integráció a Team Services**.

1. Nyissa meg a Jenkins fiókját, és válassza a **kezelése Jenkins**.

1. A hello **kezelése Jenkins** lapon, válassza ki **kezelése beépülő modulok**.

1. Szűrő hello lista toolocate hello **NodeJS** beépülő modul és a restart záradék nélkül telepítheti.

   ![Hello NodeJS beépülő modul tooJenkins hozzáadása](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. Szűrő hello lista toofind hello **a Team Foundation Server** beépülő modul és telepítéséhez. (Ez a Team Services és a Team Foundation Server a beépülő modul működik.) Jenkins újraindítása nincs szükség.

## <a name="configure-jenkins-build-for-nodejs"></a>A Node.js Jenkins build konfigurálása

A Jenkins hozzon létre egy új build projektet, és konfigurálja az alábbiak szerint:

1. A hello **általános** lapra, adja meg a build projekt nevét.

1. A hello **forrás kód felügyeleti** lapon jelölje be **Git** , és írja be a hello tárház és az alkalmazás kódját tartalmazó hello fiókirodai hello részleteit.

   ![A tárház tooyour build hozzáadása](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > Ha a tárház nem nyilvános, válasszon **Hozzáadás** , és adja meg a hitelesítő adatok tooconnect tooit.

1. A hello **Build eseményindítók** lapon jelölje be **lekérdezési SCM** , és írja be a hello ütemezés `H/03 * * * *` toopoll hello Git-tárház változásainak három percenként. 

1. A hello **környezet létrehozása** lapon jelölje be **meg csomópont &amp; npm bin / mappa elérési ÚTJA** , és írja be `NodeJS` hello csomópont JS telepítés érték a. Hagyja **npmrc fájl** "használja a rendszer alapértelmezett."

1. A hello **Build** lapra, adja meg a hello parancs `npm install` tooensure összes függőségét frissülnek.

## <a name="configure-jenkins-for-team-services-integration"></a>Jenkins Team Services-integráció konfigurálása

1. A hello **utáni műveletek** lapon, a **fájlok tooarchive**, adja meg `**/*` tooinclude összes fájlt.

1. A **indul el, a TFS/Team Services kiadás**, hello teljes fiókja URL-CÍMÉT adja meg (például `https://your-account-name.visualstudio.com`), hello projekt nevét, egy nevet a hello kiadás definition (később létre), és a hitelesítő adatok tooconnect tooyour fiók hello.
   A felhasználónév és a korábban létrehozott PAT hello van szüksége. 

   ![Jenkins utáni műveletek konfigurálása](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. Hello build projekt mentése.

## <a name="create-a-jenkins-service-endpoint"></a>Hozzon létre egy Jenkins szolgáltatási végpont

A szolgáltatási végpont lehetővé teszi, hogy a Team Services tooconnect tooJenkins.

1. Nyissa meg hello **szolgáltatások** Team Services, nyissa meg hello lap **új szolgáltatási végpont** listában, és válassza a **Jenkins**.

   ![Vegyen fel egy Jenkins végpontot](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. Adjon meg egy nevet, toorefer toothis kapcsolat használni fog.

1. Adja meg a Jenkins kiszolgáló hello URL-CÍMÉT, és hello osztásjelek **nem megbízható az SSL-tanúsítványokat elfogadó** lehetőséget.

1. Adja meg a Jenkins fiók hello felhasználónevet és jelszót.

1. Válasszon **kapcsolat ellenőrzéséhez** , amelyek információt hello toocheck helyességéről.

1. Válasszon **OK** toocreate hello szolgáltatásvégpontot.

## <a name="create-a-deployment-group"></a>Központi telepítési csoport létrehozása

Kell egy [üzembe helyezési csoport](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) túl a hello virtuális gépeket tartalmaz.

1. Megnyitás hello **kiadásokban** hello lapján **Build &amp; kiadás** hub, majd nyissa meg hello **telepítési csoportban** lapot, és válassza a **+ új**.

1. Adjon meg egy nevet hello üzembe helyezési csoport, és opcionális leírását.
   Válassza a **létrehozása**.

hello Azure erőforrás-csoport központi telepítésének feladatot hoz létre, és regisztrálja a hello virtuális gépek, hello Azure Resource Manager sablonnal futtatásakor.
Nem kell toocreate, és saját kezűleg hello a virtuális gépek regisztrálását.

## <a name="create-a-release-definition"></a>Egy kiadási definíció létrehozása

Egy kiadási definition határozza meg, hello folyamat Team Services toodeploy hello alkalmazás fogja végrehajtani.
toocreate hello kiadás definíciójában Team Services:

1. Megnyitás hello **kiadásokban** hello lapján **Build &amp; kiadási** hub, nyissa meg hello  **+**  legördülő kiadás definíciók hello listájában, és válassza a Hello **létrehozás kiadás definition**. 

1. Jelölje be hello **üres** sablon válassza **következő**.

1. A hello **összetevők** területen kattintson a **összetevő hivatkozás** válassza **Jenkins**. Válassza ki a Jenkins szolgáltatási végpont kapcsolatot. Ezután jelölje ki az hello Jenkins forrás feladatot, és válassza **létrehozása**. 

1. Az új hello definition kiadási, válassza a **+ Hozzáadás feladatok** , és adja hozzá egy **Azure erőforrás-csoport központi telepítésének** toohello alapértelmezett feladatsor.

1. Hello legördülő nyílra következő toohello **+ Hozzáadás feladatok** hivatkozásra, és adja hozzá a telepítési fázis toohello meghatározása.

   ![A központi telepítési csoport szakaszok hozzáadása](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. Hello feladat katalógusban, nyissa meg a hello **segédprogram** szakaszt, és adjon hozzá egy hello **Héjparancsfájlt** feladat.

1. hello Azure erőforrás-csoport központi telepítése a feladatban használt hello paraméterek sablon hello admin használt jelszó tooconnect toohello virtuális gépek állítja be.
   Adja meg ezt a jelszót a hello változóval **$(adminpassword)**:
   
   - Nyissa meg hello **változók** lap és hello **változók** területen adja meg a hello neve `adminpassword`.

   - Adja meg a hello rendszergazdai jelszót.

   - Hello "lakat" ikonra következő toohello érték szövegmező tooprotect hello jelszó választására. 

## <a name="configure-hello-azure-resource-group-deployment-task"></a>Hello Azure erőforrás-csoport központi telepítésének tevékenység konfigurálása

Hello **Azure erőforrás-csoport központi telepítésének** feladata használt toocreate hello üzembe helyezési csoport. Konfigurálja úgy az alábbiak szerint:

* **Azure-előfizetés:** alatti hello listából válasszon ki egy kapcsolatot **Azure-szolgáltatás rendelkezésre álló kapcsolatok**. 
  Ha a kapcsolat nem jelenik meg, válassza a **kezelése**, jelölje be **új szolgáltatási végpont** majd **Azure Resource Manager**, és hello utasításokat követve.
  Térjen vissza a tooyour kiadási definícióját, frissítési hello **AzureRM előfizetés** listából, és a létrehozott válassza hello kapcsolatot.

* **Erőforráscsoport**: Adjon meg egy korábban létrehozott erőforráscsoportot hello.

* **Hely**: Válasszon ki egy régiót hello központi telepítéshez.

  ![Új erőforráscsoport létrehozása](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* **A sablon helye**:`URL of hello file`

* **Sablon hivatkozás**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`

* **Sablon paraméterei link**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`

* **Sablon paraméterének**: hello listája felülírják értékeket, például: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.<br />Helyezze be a saját hello {helyőrzők} adott értékeit. 

* **Előfeltételek engedélyezése**:`Configure with Deployment Group agent`

* **TFS/VSTS-végpont**: válasszon **Hozzáadás** , és válassza az "Új Team Foundation Server/Team Services-kapcsolat hozzáadása" hello párbeszédpanelen **jogkivonat-alapú hitelesítés**. Adja meg a hello kapcsolat neve és a csapatprojekt hello URL-CÍMÉT. Ezután létre, és adja meg a [személyes hozzáférési jogkivonat (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) tooauthenticate hello kapcsolat tooyour csapatprojektben.

  ![Hozzon létre egy személyes hozzáférési tokent](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* **Csapatprojekt**: válassza ki az aktuális projektben.

* **Üzembe helyezési csoport**: Adjon meg hello azonos telepítési csoport neve, mint hello használt **erőforráscsoport** paraméter.

hello Azure erőforrás-csoport központi telepítésének feladat hello alapértelmezett beállításai toocreate vagy olyan erőforráshoz és toodo így Növekményesen frissíteni. hello feladat hoz létre a virtuális gépek hello hello első alkalommal fut, és csak ezt követően frissítse azokat.

## <a name="configure-hello-shell-script-task"></a>Hello Héjparancsfájlt tevékenység konfigurálása

Hello **Héjparancsfájlt** feladata egy parancsfájl toorun minden kiszolgáló tooinstall Node.js és kezdő hello alkalmazásában használt tooprovide hello konfigurációját. Konfigurálja úgy az alábbiak szerint:

* **Parancsfájl elérési útja**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`

* **Adjon meg működő Directory**:`Checked`

* **Munkakönyvtár**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
   
## <a name="rename-and-save-hello-release-definition"></a>Nevezze át és hello kiadás definíció mentése

1. Hello neve hello kiadás definition toohello megadott szerkesztése a **utáni műveletek** Jenkins hello buildverziót lapján. Jenkins igényel a név toobe képes tootrigger új kiadását, hello forrás összetevők frissítésekor.

1. Igény szerint megváltoztathatja hello környezet hello nevét a hello nevére kattintva. 

1. Válasszon **mentése**, és válassza a **OK**.

## <a name="start-a-manual-deployment"></a>Indítsa el a manuális központi telepítése

1. Válassza ki **+ kiadási** válassza **létrehozása kiadási**.

1. Hello build elvégezte hello kiemelt legördülő listában jelölje ki, és válassza a **létrehozása**.

1. Hello felbukkanó üzenet válassza hello kiadás hivatkozásra. Például: "kiadás **Release-1** létrejött."

1. Nyissa meg hello **naplók** toowatch hello kiadás konzol kimeneti fülre.

1. A böngészőben nyissa meg a hello URL-cím hello kiszolgálók közül az egyik tooyour üzembe helyezési csoport hozzáadni. Adja meg például`http://{your-server-ip-address}`

## <a name="start-a-cicd-deployment"></a>Indítsa el a CI/CD központi telepítés

1. Hello kiadásában definícióját, törölje a jelet hello **engedélyezve** hello jelölőnégyzet **beállítások** hello Azure erőforrás-csoport központi telepítésének feladat hello beállításai szakasza.
   A jövőben a központi telepítésekre toohello meglévő üzembe helyezési csoport, nem kell toore – a feladat végrehajtásához.

1. Nyissa meg toohello forrás Git-tárház és hello hello tartalmának módosítása **h1** hello fájlban fejléc [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).

1. A módosítás véglegesítése.

1. Néhány perc elteltével látni fogja a hello létrehozott új kiadási **kiadásokban** Team Services vagy a TFS oldalán. Nyissa meg a hello kiadás toosee hello telepítése folyamatban. Gratulálunk!

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban automatikus hello központi telepítése egy app tooAzure Jenkins build és a kiadáshoz Team Services használatával. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Az alkalmazás a Jenkins elkészítésére
> * Jenkins Team Services-integráció konfigurálása
> * Egy központi telepítési csoport hello Azure virtuális gépek létrehozása
> * Konfigurálja a hello virtuális gépek és hello alkalmazást telepíti a kiadási-definíció létrehozása

Előzetes toohello következő útmutató toolearn kapcsolatos hogyan toodeploy egy LÁMPA (Linux, Apache, MySQL és a PHP) a verem.

> [!div class="nextstepaction"]
> [LAMP stack telepítése](tutorial-lamp-stack.md)