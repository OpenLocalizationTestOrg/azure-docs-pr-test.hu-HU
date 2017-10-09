---
title: "Azure Automation-fiók aaaCreate önálló |} Microsoft Docs"
description: "Útmutató, amely bemutatja, hogyan biztonság – egyszerű hitelesítés az Azure Automationben hello létrehozása, tesztelése és példa használatát."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 1500d25d9565d4082768933834303a17c5e84234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-standalone-azure-automation-account"></a>Önálló Azure Automation-fiók létrehozása
Ez a témakör bemutatja, hogyan toocreate hello Azure portálon, ha szeretné, hogy tooevaluate, és ismerje meg az Azure Automation hello további felügyeleti megoldások vagy OMS Naplóelemzési tooprovide integrációja nélkül az Automation-fiók speciális monitorozása runbook-feladatok.  Ezen megoldások hozzáadása, vagy Naplóelemzési integrálása jövőbeli hello bármely pontján.  Hello Automation-fiók az Azure Resource Manager vagy az Azure klasszikus üzembe helyezési erőforrások kezelése képes tooauthenticate runbookok.

Hello Azure-portálon az Automation-fiók létrehozásakor automatikusan létrehoz:

* Futtató fiók, ami létrehoz egy új szolgáltatás egyszerű az Azure Active Directoryban, a tanúsítványt, és rendel hello közreműködői szerepkör alapú hozzáférés-vezérlés (RBAC), amely használt toomanage Resource Managerhez tartozó erőforrások runbookok használata.   
* Klasszikus Futtatás mint fiók felügyeleti tanúsítvány, amely feltöltésével használt toomanage hagyományos erőforrások runbookok használata.  

Ez hello egyszerűbben, és a segítségével gyorsan az épület indíthat el és központi telepítése a runbookok toosupport kell az automation.  

## <a name="permissions-required-toocreate-automation-account"></a>Toocreate Automation-fiók szükséges jogosultságok
toocreate vagy frissítés Automation-fiók, rendelkeznie kell a következő bizonyos jogokat hello és engedélyek szükséges toocomplete ebben a témakörben.   
 
* A sorrend toocreate Automation-fiók, az AD-felhasználói fiókot igények toobe hozzáadott tooa szerepkör-engedélyek egyenértékű toohello tulajdonosi szerepkör Microsoft.Automation erőforrások cikkben leírt módon történő [szerepköralapú hozzáférés-vezérlés az Azure Automationben ](automation-role-based-access-control.md).  
* Ha hello App regisztrációk beállítás értéke túl**Igen**, az Azure AD-bérlő nem rendszergazdai felhasználók számára is [AD-alkalmazásokat regisztrálni](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Ha hello app regisztrációk beállítás értéke túl**nem**, a művelet végrehajtása hello felhasználói globális rendszergazdának kell lennie az Azure ad-ben. 

Ha nem tagja Active Directory-példányban hello előfizetés toohello globális rendszergazda/járművezető-administrator szerepkör hello előfizetés vannak adása előtt, akkor tooActive Directory vendégként kerülnek. Ebben az esetben a megjelenik a "nem rendelkezik engedélyekkel toocreate..." Figyelmeztetés-hello **Automation-fiók hozzáadása** panelen. Felhasználók, akik toohello globális rendszergazda/járművezető-administrator szerepkör először eltávolíthatja hello előfizetés Active Directory-példányban, és újra hozzáadva toomake hozzáadták azokat a teljes felhasználói az Active Directoryban. tooverify ebben az esetben a hello **Azure Active Directory** hello Azure portálon, válassza a panelen **felhasználók és csoportok**, jelölje be **minden felhasználó** és hello kiválasztása után adott felhasználó, jelölje be **profil**. hello értékének hello **felhasználótípust** attribútum hello felhasználó profil alapján nem értékűnek kell lennie **vendég**.

## <a name="create-a-new-automation-account-from-hello-azure-portal"></a>Új Automation-fiók létrehozása hello Azure-portálon
Ebben a szakaszban hajtsa végre a következő lépéseket toocreate hello Azure-portálon az Azure Automation-fiók hello.    

1. Bejelentkezés toohello egy olyan fiókkal, amely hello előfizetés-Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként Azure-portálon.
2. Kattintson az **Új** lehetőségre.<br><br> ![Válassza az Új lehetőséget az Azure Portalon](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Keresse meg **Automation** és majd hello a keresési eredményeket megjelenítő válassza **Automation & vezérlő***.<br><br> ![Keresse meg és válassza ki az Automationt a Marketplace-en](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
3. Hello Automation-fiók paneljén kattintson **Hozzáadás**.<br><br>![Automation-fiók hozzáadása](media/automation-create-standalone-account/automation-create-automationacct-properties.png)
   
   > [!NOTE]
   > Ha megjelenik a következő hello figyelmeztetésre hello **Automation-fiók hozzáadása** panelen van, mert a fiók nincs hello előfizetés-Rendszergazdák szerepkör tagja, és a társadminisztrátornak hello előfizetés.<br><br>![Az Automation-fiókhoz kapcsolódó figyelmeztetés hozzáadása](media/automation-create-standalone-account/create-account-without-perms.png)
   > 
   > 
4. A hello **Automation-fiók hozzáadása** paneljén, hello **neve** mezőbe írja be az új Automation-fiók nevét.
5. Ha egynél több előfizetéssel rendelkezik, adja meg, egy hello új fiókot, egy új vagy meglévő **erőforráscsoport** és egy Azure-adatközpontban **hely**.
6. Hello érték ellenőrzése **Igen** van kiválasztva a hello **létrehozása Azure-beli futtató fiók** lehetőséget, majd kattintson a hello **létrehozása** gombra.  
   
   > [!NOTE]
   > Ha úgy dönt, toonot hello lehetőség kiválasztásával hello futtató fiók létrehozása **nem**, lehetősége lesz a hello figyelmeztető üzenet **Automation-fiók hozzáadása** panelen.  Hello fiók hello Azure-portálon jön létre, amíg az előfizetésben nem tartalmaz a megfelelő hitelesítési identitását a klasszikus vagy a Resource Manager előfizetés címtárszolgáltatás, ezért nem hozzáférés tooresources belül.  Ez megakadályozza, hogy ez a fiók nem tud tooauthenticate hivatkozó runbookokat, és ezek üzembe helyezési modellel erőforrásokon feladatokat.
   > 
   > ![Az Automation-fiókhoz kapcsolódó figyelmeztetés hozzáadása](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)<br>
   > Hello szolgáltatás egyszerű nem létrehozásakor hello közreműködői szerepkör nincs hozzárendelve.
   > 

7. Azure Automation-fiók hello hoz létre, amíg előrehaladásának hello alatt **értesítések** hello menüből.

### <a name="resources-included"></a>Érintett erőforrások
Ha hello Automation-fiók sikeresen létrejött, a több erőforrás automatikusan létrejönnek meg.  a következő táblázat hello hello Futtatás mint fiók erőforrásainak foglalja össze.<br>

| Erőforrás | Leírás |
| --- | --- |
| AzureAutomationTutorial forgatókönyv |Grafikus példaforgatókönyv, amely bemutatja, hogyan tooauthenticate használatával hello Futtatás mint fiókot, és minden hello Resource Managerhez tartozó erőforrások lekérése. |
| AzureAutomationTutorialScript forgatókönyv |PowerShell példaforgatókönyv, amely bemutatja, hogyan tooauthenticate használatával hello Futtatás mint fiókot, és minden hello Resource Managerhez tartozó erőforrások lekérése. |
| AzureRunAsCertificate |Tanúsítvány eszköz automatikusan Automation-fiók létrehozása során létrehozott vagy hello PowerShell-parancsfájl alatt egy olyan meglévő fiók használatával.  Lehetővé teszi az Azure-ral tooauthenticate, hogy a runbookok Azure Resource Manager-erőforrások kezelése.  Ennek a tanúsítványnak egy éves időtartama van. |
| AzureRunAsConnection |Kapcsolat eszköz automatikusan Automation-fiók létrehozása során létrehozott vagy hello PowerShell-parancsfájl alatt egy olyan meglévő fiók használatával. |

a következő táblázat hello hello klasszikus futtató fiókhoz tartozó erőforrások foglalja össze.<br>

| Erőforrás | Leírás |
| --- | --- |
| AzureClassicAutomationTutorial forgatókönyv |Grafikus példaforgatókönyv, amely minden hello klasszikus virtuális gépeinek kap egy előfizetésben hello klasszikus futtató fiók (tanúsítvány) használatával, és majd exportálja a hello virtuális gép nevét és állapotát. |
| AzureClassicAutomationTutorial parancsprogram-forgatókönyv |PowerShell példaforgatókönyv, amely minden hello klasszikus virtuális gépeinek kap egy előfizetésben hello klasszikus futtató fiók (tanúsítvány) használatával, és majd exportálja a hello virtuális gép nevét és állapotát. |
| AzureClassicRunAsCertificate |Automatikusan létrehozott, amely tanúsítványeszköz tooauthenticate, hogy a runbookok kezelheti a klasszikus Azure-erőforrások Azure használva.  Ennek a tanúsítványnak egy éves időtartama van. |
| AzureClassicRunAsConnection |Kapcsolat eszköz automatikusan jön létre, amely tooauthenticate, hogy a runbookok kezelheti a klasszikus Azure-erőforrások Azure használva. |


## <a name="next-steps"></a>Következő lépések
* toolearn grafikus szerzői kapcsolatos további információkért lásd: [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md).
* a PowerShell-forgatókönyvek, használatába tooget lásd [az első PowerShell runbook](automation-first-runbook-textual-powershell.md).
* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md).
