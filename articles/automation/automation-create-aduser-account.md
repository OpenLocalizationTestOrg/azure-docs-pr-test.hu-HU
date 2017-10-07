---
title: "Azure AD-felhasználó fiók aaaCreate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan hitelesítőadat-toocreate egy Azure AD-felhasználói fiókot az Azure Automation tooauthenticate Azure és a klasszikus Azure runbookok."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: "azure active directory felhasználó, azure szolgáltatás kezelése, azure ad és felhasználói fiók"
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 7c6ed4182dbab074d0bc5da7057f74ad321d8884
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Runbookok hitelesítése a klasszikus Azure üzemi modellel és a Resource Managerrel
Ez a cikk meg kell ismételnie tooconfigure egy Azure AD-felhasználói fiókot az Azure Automation-runbook Azure klasszikus üzembe helyezési modellel vagy Azure Resource Manager erőforrások futtatott hello lépéseit ismerteti.  Amíg ez továbbra is toobe egy támogatott hitelesítési identitást az az Azure Resource Manager runbookokat alapú, hello ajánlott módszer egy Azure-beli futtató fiókot használja.       

## <a name="create-a-new-azure-active-directory-user"></a>Egy új Azure Active Directory-felhasználó létrehozása
1. A hello toomanage kívánt Azure-előfizetés szolgáltatás-rendszergazdaként jelentkezzen toohello a klasszikus Azure portálon.
2. Válassza ki **Active Directory**, majd válassza ki a szervezete címtárának nevét hello.
3. Jelölje be hello **felhasználók** lapot, és ezt követően hello parancs területen válasszon **felhasználó hozzáadása**.
4. A hello **adja meg azt a felhasználó** lap **felhasználó típusa**, jelölje be **a szervezet új felhasználó**.
5. Írjon be egy felhasználónevet.  
6. Válassza ki a hello Active Directory oldalon az Azure-előfizetéshez társított hello könyvtárnevet.
7. A hello **felhasználói profil** lapján adja meg az első és utolsó nevét, egy felhasználóbarát nevet, és felhasználói a hello **szerepkörök** listája.  Ne jelölje be a **Multi-Factor Authentication engedélyezése** lehetőséget.
8. Vegye figyelembe a hello felhasználó teljes neve és ideiglenes jelszót.
9. Válassza a **Beállítások > Rendszergazdák > Hozzáadás** elemet.
10. Írja be a teljes felhasználónevet hello hello felhasználói létrehozott.
11. Válassza ki a használni kívánt felhasználói toomanage hello hello-előfizetést.
12. Jelentkezzen ki az Azure, és jelentkezzen vissza be most létrehozott hello fiókkal. Rákérdezéses toochange hello jelszó lesz.

## <a name="create-an-automation-account-in-azure-classic-portal"></a>Automation-fiók létrehozása a klasszikus Azure portálon
Ebben a szakaszban követő lépéseket toocreate hello használata Azure-portálon az Azure Automation-fiók az Azure klasszikus üzembe helyezési erőforrások kezelése a runbookokkal hello hajtsa végre.  

> [!NOTE]
> Automation-fiók hello a klasszikus Azure portálon létrehozott kezelheti is hello Azure klasszikus és az Azure portálon, de parancsmagok. Hello-fiók létrehozása után azt mindegy, hogyan hozzon létre és kezelheti az erőforrásokat hello fiókon belül. Ha azt tervezi toocontinue toouse hello klasszikus Azure portálra, akkor ahelyett, hogy azt használja az Azure portál toocreate hello Automation-fiók.
> 
> 

1. A hello toomanage kívánt Azure-előfizetés szolgáltatás-rendszergazdaként jelentkezzen toohello a klasszikus Azure portálon.
2. Válassza az **Automation** elemet.
3. A hello **Automation** lapon jelölje be **Automation-fiók létrehozása**.
4. A hello **Automation-fiók létrehozása** mezőben, írja be az új Automation-fiók nevét, és válassza ki a **régió** hello legördülő listából.  
5. Kattintson a **OK** tooaccept a beállítások és hello-fiók létrehozása.
6. Létrehozás után jelenik meg a hello **Automation** lap.
7. Kattintson a hello fiókot, és megjelenik a akkor toohello irányítópult-oldalon.  
8. Hello automatizálási irányítópult lapon jelölje be **eszközök**.
9. A hello **eszközök** lapon jelölje be **beállítások hozzáadása** hello hello lap alsó részén található.
10. A hello **beállítások hozzáadása** lapon jelölje be **adja hozzá a hitelesítő adatok**.
11. A hello **hitelesítő adatok megadása** lapon jelölje be **Windows PowerShell-hitelesítő adat** a hello **hitelesítőadat-típus** legördülő listában, és adja meg a hello hitelesítő adat nevét.
12. Hello következő **hitelesítő adatok megadása** írja be a hello felhasználónév hello AD a felhasználói fiók hello korábban létrehozott **felhasználónév** hello mező és hello jelszót **jelszó**és **jelszó megerősítése** mezőket. Kattintson a **OK** toosave a módosításokat.

## <a name="create-an-automation-account-in-hello-azure-portal"></a>Automation-fiók létrehozása a hello Azure-portálon
Ebben a szakaszban hajtsa végre a következő lépések toocreate hello használata Azure-portálon az Azure Automation-fiók az Azure Resource Manager módra az erőforrások kezelése runbookok hello.  

1. Jelentkezzen be toohello Azure-portálon a hello toomanage kívánt Azure-előfizetés szolgáltatás-rendszergazdaként.
2. Válassza az **Automation-fiókok** elemet.
3. Hello Automation-fiók paneljén kattintson **Hozzáadás**.<br><br>![Automation-fiók hozzáadása](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. A hello **Automation-fiók hozzáadása** paneljén, hello **neve** mezőbe írja be az új Automation-fiók nevét.
5. Ha egynél több előfizetéssel rendelkezik, adja meg a hello hello új fiók, valamint egy új vagy meglévő **erőforráscsoport** és egy Azure-adatközpontban **hely**.
6. Adja meg a hello értéket **Igen** a hello **létrehozása Azure-beli futtató fiók** lehetőséget, majd kattintson a hello **létrehozása** gombra.  
   
    > [!NOTE]
    > Ha úgy dönt, toonot hello lehetőség kiválasztásával hello futtató fiók létrehozása **nem**, hello figyelmeztető üzenetet választhat **Automation-fiók hozzáadása** panelen.  Amíg hello fiókot létrehozni és hozzárendelni toohello **közreműködő** szerepkör hello az előfizetést, nem lesz az előfizetések címtárszolgáltatás, és ezért nem lehet hozzáférni a megfelelő hitelesítési identitás erőforrást az előfizetésében.  Ez megakadályozza ennek a fióknak nem tud tooauthenticate hivatkozó runbookokat, és feladatokat az Azure Resource Manager erőforrásokon.
    > 
    >

    <br>![Az Automation-fiókhoz kapcsolódó figyelmeztetés hozzáadása](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. Azure Automation-fiók hello hoz létre, amíg előrehaladásának hello alatt **értesítések** hello menüből.

Hello credential hello létrehozásának befejezése után a hitelesítőadat-eszköz tooassociate hello Automation-fiók hello AD-felhasználói fiókot a korábban létrehozott toocreate kell.  Ne feledje, hogy csak létrehozott hello Automation-fiók, és nincs társítva egy hitelesítési identitását.  Hello leírt hello lépésekkel [hitelesítőadat-eszközök az Azure Automation-cikkben](automation-credentials.md#creating-a-new-credential-asset) , és írja be a hello értéke **felhasználónév** hello formátumban **tartomány\felhasználó**.

## <a name="use-hello-credential-in-a-runbook"></a>Hello hitelesítő adatok használata a runbookokban
Hello használata a runbookokban hello hitelesítőadat le [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) tevékenységet, majd a [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) tooconnect tooyour Azure-előfizetés. Ha hello hitelesítő adatok rendszergazdai több Azure-előfizetéssel, akkor is használjon [válasszon-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) toospecify hello helyes névre. Ez általában a legtöbb Azure Automation-runbook hello tetején megjelenő hello minta az alábbi Windows PowerShell jelenik meg.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

A forgatókönyv minden [ellenőrzőpontja](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) után ismételje meg ezeket a sorokat. Ha hello runbook fel van függesztve, és egy másik feldolgozónak a majd folytatja, majd el kell tooperform hello hitelesítési újra.

## <a name="next-steps"></a>Következő lépések
* Felülvizsgálati hello másik runbook típusok és a lépéseket a saját runbookok létrehozásáról a következő cikk hello [Azure Automation-runbook típusok](automation-runbook-types.md)

