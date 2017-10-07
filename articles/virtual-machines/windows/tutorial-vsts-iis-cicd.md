---
title: az Azure-ban Team Services CI/CD adatcsatorna aaaCreate |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate a Visual Studio Team Services a következő feldolgozási sorban folyamatos integrációt és kézbesítését, hogy egy webes alkalmazás tooIIS egy Windows virtuális gépre telepíti"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b758a124c4742854dd3b543f747fd8700f954414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a>Visual Studio Team Services és az IIS egy folyamatos integrációt folyamat létrehozása
tooautomate hello build, tesztelése és alkalmazásfejlesztés szakaszainak központi telepítés, a folyamatos integrációt és a központi telepítés (CI/CD) folyamat is használhatja. Ebben az oktatóanyagban létrehoz egy Visual Studio Team Services és a Windows rendszerű virtuális gép (VM) használata az IIS-t futtató Azure CI/CD folyamat. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * ASP.NET webes alkalmazás tooa Team Services projekt közzététele
> * A kód véglegesítések által elindított build-definíció létrehozása
> * IIS telepítése és konfigurálása az Azure virtuális gépen
> * A Team Services hello IIS példány tooa üzembe helyezési csoport hozzáadása
> * Kiadás definition toopublish webhely tooIIS csomagok központi telepítése
> * Hello CI/CD folyamat tesztelése

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás `Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).


## <a name="create-project-in-team-services"></a>A Team Services-projekt létrehozása
A Visual Studio Team Services lehetővé teszi, hogy könnyen együttműködés és fejlesztési egy helyszíni-felügyeleti megoldás fenntartása nélkül. Team Services biztosít a felhő kód tesztelése, elkészítéséhez és az application insights. Választhat egy verzió vezérlő tárház és IDE a kód fejlesztési legjobban illő. Ebben az oktatóanyagban egy ingyenes fiókot toocreate egy egyszerű ASP.NET webalkalmazást és CI/CD kimenetátirányítási is használhatja. Ha még nem rendelkezik egy Team Services-fiók [hozzon létre egyet](http://go.microsoft.com/fwlink/?LinkId=307137).

toomanage hello kód véglegesítési folyamat build-definíciókat, és kiadási definíciók, az alábbiak szerint hozzon létre egy projektet Team Services:

1. A Team Services irányítópult megnyitása a böngészőben, és válassza a **új projekt**.
2. Adja meg *myWebApp* a hello **projektnevet**. Hagyja a további alapértelmezett értékek toouse *Git* verziókezelést és *Agile* munkaelemet bekérő folyamat.
3. Hello választógomb túl**megosztása** *csapattagok*, majd jelölje be **létrehozása**.
5. A projekt létrehozása után válassza ki az hello beállítás túl**információs vagy gitignore inicializálása**, majd **inicializálása**.
6. Válassza ki az új projekt belül **irányítópultok** hello tetején, majd válassza ki **Megnyitás Visual Studio**.


## <a name="create-aspnet-web-application"></a>ASP.NET-webalkalmazás létrehozása
Hello előző lépésben létrehozott csapat szolgáltatásokban projekt. utolsó lépésként hello Visual Studio az új projekt megnyitása. Kezelheti a kód véglegesíti a hello **Team Explorer** ablak. Hozzon létre az új projekt helyi példányát, majd egy ASP.NET-webalkalmazás létrehozása sablonból az alábbiak szerint:

1. Válassza ki **Klónozás** toocreate egy helyi git-tárház a Team Services projekt.
    
    ![Team Services projektből tárház klónozása](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. A **megoldások**, jelölje be **új**.

    ![Webalkalmazási megoldás létrehozása](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Válassza ki **webes** sablonokat, és válassza a hello **ASP.NET Web Application** sablont.
    1. Írjon be egy nevet az alkalmazásnak, például *myWebApp*, és törölje a jelet hello be a **könyvtár létrehozása a megoldáshoz**.
    2. Ha hello lehetőség nem érhető el, törölje a jelet hello mezőben túl**Application Insights hozzáadása tooproject**. Az Application Insights igényel, tooauthorize a webalkalmazás az Azure Application insights szolgáltatással. tookeep ebben az oktatóanyagban egyszerű, hagyja ki ezt a folyamatot.
    3. Kattintson az **OK** gombra.
4. Válasszon **MVC** hello sablon listából.
    1. Válassza ki **hitelesítés módosítása**, válassza a **nem hitelesítési**, majd jelölje be **OK**.
    2. Válassza ki **OK** toocreate a megoldás.
5. A hello **Team Explorer** ablakban válasszon **módosítások**.

    ![Véglegesítse a változtatásokat tooTeam szolgáltatások git-tárház](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. Hello véglegesítési szövegmezőben, adjon meg egy üzenetet, mint *kezdeti véglegesítési*. Válasszon **véglegesíti az összes és a szinkronizálási** hello legördülő menüből.


## <a name="create-build-definition"></a>Build definíció létrehozása
A Team Services a build definition toooutline, hogyan kell kialakítani, az alkalmazás használja. Ebben az oktatóanyagban létrehozhatunk egy alapszintű definition vesz igénybe a forráskódban, hello megoldást hoz létre, akkor hoz létre web csomag használhatjuk toorun hello webalkalmazást az IIS-kiszolgáló üzembe helyezése.

1. A Team Services projekthez, válassza a **Build & kiadási** hello tetején, majd válassza ki **buildek**.
3. Válassza ki **+ új definition**.
4. Válassza ki a hello **ASP.NET (előzetes verzió)** sablont, és válassza **alkalmaz**.
5. Minden hello alapértelmezett feladat értékek hagyja. A **adatforrások beolvasása**, győződjön meg arról, hogy hello *myWebApp* tárház és *fő* fiókirodai van kiválasztva.

    ![Build definíció Team Services-projekt létrehozása](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. A hello **eseményindítók** lapon, a hello csúszkát **engedélyezése ehhez az eseményindítóhoz** túl*engedélyezve*.
7. Mentés hello build definíció- és várólista új buildverziót kiválasztásával **mentés és a feldolgozási sor** , majd **mentés és a feldolgozási sor** újra. Meghagyhatja hello alapértelmezett beállításokat, és válassza ki **várólista**.

A Watch hello build üzemeltetett ügynökön van ütemezve, majd megkezdi toobuild. hello hasonló toohello a következő példa a kimenetre:

![A Team Services projekt sikeres build](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Virtuális gép létrehozása
a platform toorun tooprovide az ASP.NET webalkalmazás szükséges IIS-t futtató Windows rendszerű virtuális gép. Team Services egy ügynök toointeract kód véglegesíti és buildek által kiváltott hello IIS-példány használja.

Windows Server 2016 VM létre [a parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json). Hello parancsfájl toorun néhány percet vesz igénybe, és hozzon létre virtuális gép hello. Hello virtuális gép létrehozása után, nyissa meg a 80-as port webes forgalom a [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) az alábbiak szerint:

```powershell
Get-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $resourceGroup `
  -Name "myNetworkSecurityGroup" | `
Add-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWeb" `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority "1001" `
  -SourceAddressPrefix "*" `
  -SourcePortRange "*" `
  -DestinationAddressPrefix "*" `
  -DestinationPortRange "80" `
  -Access "Allow" | `
Set-AzureRmNetworkSecurityGroup
```

tooconnect tooyour VM, hello nyilvános IP-cím az beszerzése [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) az alábbiak szerint:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

A távoli asztali munkamenetgazda tooyour virtuális gép létrehozása:

```cmd
mstsc /v:<publicIpAddress>
```

Hello VM, nyisson meg egy **rendszergazda PowerShell** parancssort. Az IIS és a kötelező .NET-szolgáltatások telepítése a következőképpen:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Üzembe helyezési csoport létrehozása
kimenő hello webes toopush csomag toohello IIS-kiszolgálón, a rendszer Team Services adjon meg egy központi telepítési csoportot. Ennek a csoportnak azok a kiszolgálók olyan új buildek hello célját, mint a szolgáltatások és -buildek befejezése kód tooTeam véglegesítést toospecify lehetővé teszi.

1. Team Services, válassza a **Build & kiadási** , és válassza **telepítési csoportban**.
2. Válasszon **központi telepítés hozzáadása csoporthoz**.
3. Írjon be hello csoport nevét, például *myIIS*, majd jelölje be **létrehozása**.
4. A hello **gépek regisztrálása** területen győződjön meg arról *Windows* van kiválasztva, majd hello jelölőnégyzetet túl**személyes hozzáférési jogkivonat hello parancsfájlban a hitelesítéshez használandó**.
5. Válassza ki **másolja a parancsfájl tooclipboard**.


### <a name="add-iis-vm-toohello-deployment-group"></a>Az IIS virtuális gép toohello üzembe helyezési csoport hozzáadása
A hello telepítési csoportot létrehozni minden egyes IIS példány toohello csoport hozzáadása. Team Services hoz létre olyan parancsfájlt, amely tölti le, és konfigurálja az ügynök központi telepítése a virtuális Gépet, amely megkapja az új webes hello csomagok, és alkalmazza azt tooIIS.

1. Vissza a hello **rendszergazda PowerShell** munkamenet a virtuális gépen, illessze be, és átmásolja a Team Services hello parancsprogrammal.
2. Ha felszólító tooconfigure címkék hello Agent dönt *Y* , és írja be *webes*.
3. Amikor a hello felhasználói fiók megadását kéri, nyomja meg az *vissza* tooaccept hello alapértelmezett értéke.
4. Várjon, amíg hello parancsfájl toofinish üzenetet *szolgáltatás vstsagent.account.computername sikeresen elindult*.
5. A hello **telepítési csoportban** hello oldalán **Build & kiadási** menü, nyissa meg hello *myIIS* üzembe helyezési csoport. A hello **gépek** lapra, győződjön meg arról, hogy a virtuális gép.

    ![Virtuális gép sikeresen hozzáadta a tooTeam szolgáltatások üzembe helyezési csoport](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a>Kiadás definíció létrehozása
toopublish a buildek hoz létre egy kiadási definition Team Services. Ez a definíció sikeres build az alkalmazás automatikusan elindul. Válassza ki a hello telepítési csoport toopush a web deploy csomag számára, és hello megfelelő IIS-beállítások megadása.

1. Válasszon **Build & kiadási**, majd jelölje be **buildek**. Válassza ki az előző lépésben létrehozott hello build definition.
2. A **nemrég befejeződött**hello legutóbbi build válassza, majd válassza ki **kiadás**.
3. Válasszon **Igen** toocreate kiadás definícióját.
4. Válassza ki a hello **üres** sablont, majd válassza ki **következő**.
5. Ellenőrizze, hogy hello projekt és a forrás-build definíciója a rendszer feltölti a projektet.
6. Jelölje be hello **folyamatos üzembe helyezés** jelölőnégyzetet, majd válassza ki **létrehozása**.
7. Válasszon hello legördülő lista melletti túl**+ Hozzáadás feladatok** válassza *adja hozzá a központi telepítési csoport szakaszok*.
    
    ![A Team Services toorelease feladatdefiníció hozzáadása](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Válasszon **Hozzáadás** következő túl**IIS Web App Deploy(Preview)**, majd jelölje be **Bezárás**.
9. Jelölje be hello **futhat az üzembe helyezési csoport** Szülőtevékenység.
    1. A **üzembe helyezési csoport**, jelölje be hello telepítési csoportjának, létrehozott korábbi, mint például *myIIS*.
    2. A hello **számítógép-címkék** mezőben válassza **Hozzáadás** , és válassza a hello *webes* címke.
    
    ![Kiadás definition telepítési csoport feladat az IIS-hez](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. SELECT hello **központi telepítése: az IIS webes alkalmazás központi telepítése** feladat tooconfigure az IIS-példány beállításokat az alábbiak szerint:
    1. A **webhelynevet**, adja meg *alapértelmezett webhely*.
    2. Minden hello hagyja a további alapértelmezett beállításokat.
12. Válasszon **mentése**, majd jelölje be **OK** kétszer.


## <a name="create-a-release-and-publish"></a>Egy kiadási létrehozása és közzététele
Most tolható ki a webhely egy új kiadási csomag telepítéséhez. Ez a lépés kommunikál hello ügynök hello üzembe helyezési csoport része, a leküldéses értesítések hello webes összes példányt csomag telepítése, majd konfigurálja az IIS toorun frissített hello webes alkalmazást.

1. A kiadási definíciót, válassza ki **+ kiadási**, majd válassza ki **létrehozása kiadási**.
2. Győződjön meg arról, hogy hello legújabb buildjével hello legördülő listában kiválasztott, valamint **automatikus központi telepítési: kiadás létrehozása után**. Kattintson a **Létrehozás** gombra.
3. Egy kis szalagcím akkor jelenik meg a kiadási definíció hello tetején, mint *kiadás kibocsátási-1 létrejött*. Jelölje be hello kiadás hivatkozásra.
4. Nyissa meg hello **naplók** lapon toowatch hello kiadás folyamatban van.
    
    ![Sikeres Team Services kiadás és webes telepítési csomag leküldéses](media/tutorial-vsts-iis-cicd/successful_release.png)

5. Hello kiadás befejezése után nyisson meg egy webböngészőt, és adja meg a virtuális gép hello nyilvános KVI címét. Az ASP.NET-webalkalmazás fut.

    ![ASP.NET webalkalmazás az IIS virtuális gépen](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-hello-whole-cicd-pipeline"></a>Hello teljes CI/CD folyamat tesztelése
A webes alkalmazás IIS-kiszolgálón fut, és próbálja meg hello teljes CI/CD folyamat. Után módosítja a Visual Studio és akkor váltódik ki, a kódot, a build véglegesítési, majd elindítja a frissített Web kiadási csomag tooIIS központi telepítése:

1. A Visual Studióban nyissa meg a hello **Megoldáskezelőben** ablak.
2. Keresse meg a tooand nyílt *myWebApp |} Nézetek |} Kezdőlap |} Index.cshtml*
3. Sor 6 tooread szerkesztése:

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. Hello fájl mentéséhez.
5. Nyissa meg hello **Team Explorer** ablakban, a select hello *myWebApp* projektre, majd kattintson a **módosítások**.
6. Adja meg például a véglegesítési üzenetet, *tesztelés CI/CD csővezeték*, majd válassza ki **véglegesítési összes és a szinkronizálási** hello legördülő menüből.
7. Team Services munkaterület egy új build hello kód véglegesítési a elindul. 
    - Válasszon **Build & kiadási**, majd jelölje be **buildek**. 
    - Válasszon a build-definíciót, majd válassza ki a hello **várakozik & futó** build toowatch, hello létrehozása történik.
9. Ha sikeres hello build, egy új kiadási elindul.
    - Válasszon **Build & kiadási**, majd **kiadásokban** toosee hello web deploy csomag tooyour IIS VM leküldött. 
    - Jelölje be hello **frissítése** ikon tooupdate hello állapotát. Ha hello *környezetek* az oszlopban látható, egy zöld pipa, hello kiadás sikeresen alkalmazva van a tooIIS.
11. toosee a módosításokat alkalmazza, frissítse az IIS-webhely a böngészőben.

    ![CI/CD láncból az IIS virtuális gépen futó ASP.NET-webalkalmazás](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy ASP.NET-webalkalmazás létrehozott Team Services, és konfigurált build és kiadás definíciók toodeploy új webes telepítési csomagokat tooIIS minden kód érvényesítéskor. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * ASP.NET webes alkalmazás tooa Team Services projekt közzététele
> * A kód véglegesítések által elindított build-definíció létrehozása
> * IIS telepítése és konfigurálása az Azure virtuális gépen
> * A Team Services hello IIS példány tooa üzembe helyezési csoport hozzáadása
> * Kiadás definition toopublish webhely tooIIS csomagok központi telepítése
> * Hello CI/CD folyamat tesztelése

Hogyan előzetes toohello következő útmutató toolearn toosecure tartalmazó webkiszolgáló SSL-tanúsítványokat.

> [!div class="nextstepaction"]
> [Biztonságos webkiszolgáló SSL](tutorial-secure-web-server.md)