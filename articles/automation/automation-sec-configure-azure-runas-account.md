---
title: "egy Azure futtató fiók aaaConfigure |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan rendszerbiztonsági hitelesítés az Azure Automationben hello létrehozása, tesztelése és példa használatát."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
keywords: "egyszerű szolgáltatásnév, setspn, azure-hitelesítés"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: magoedte
ROBOTS: NOINDEX
redirect_url: /azure/automation/
redirect_document_id: True
ms.openlocfilehash: 06675d2f6b9d8e7260ffaead4f2b2f61c2b98d13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a>Runbookok hitelesítése Azure-beli futtató fiókkal
Ez a cikk bemutatja, hogyan tooconfigure egy Azure Automation fiókként hello Azure-portálon. toodo Igen, használja a hello Futtatás mint fiók funkció tooauthenticate runbookok Azure Resource Manager vagy az Azure szolgáltatásfelügyelet erőforrások kezelése.

Automation-fiók létrehozásakor a hello Azure-portál automatikusan két fiókokat hozhat létre:

* Egy futtató fiók. Ez a fiók létrehoz egy egyszerű szolgáltatást az Azure Active Directory-ban (Azure AD), valamint egy tanúsítványt. Azt is rendel hello közreműködői szerepköralapú hozzáférés-vezérlést (RBAC), amely kezeli a Resource Managerhez tartozó erőforrások runbookok használatával.
* Egy klasszikus futtató fiókot. Ez a fiók feltölt egy felügyeleti tanúsítványt, amely toomanage szolgáltatásfelügyelet vagy hagyományos erőforrások használt runbookok használatával.

Az automatizálási fiók egyszerűbben hello és a segítségével gyorsan indítása létrehozása és telepítése a runbookok toosupport létrehozása az automation kell.

A futtató és klasszikus futtató fiókok segítségével a következőket teheti meg:

* Azure egy szabványos utat tooauthenticate adja meg, a runbookokat hello Azure-portálon az erőforrás-kezelő vagy a Service Management erőforrások kezelésekor.
* Globális runbookokat, konfigurálhatja a Azure riasztások hello használata automatizálásához.

> [!NOTE]
> Hello [Azure riasztási integrációs szolgáltatás](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) az Automation szolgáltatásban, globális runbookok Automation-fiók be van állítva egy futtató fiókot és a klasszikus futtató fiók szükséges. Automation-fiók, amely már definiálva van futtató és a klasszikus futtató fiókokat is kiválaszthatja, vagy dönthet úgy toocreate új Automation-fiók.
>  

Ez a cikk bemutatja, hogyan toocreate hello Azure-portálon az Automation-fiók egy Automation-fiók frissítéséhez, Azure PowerShell használatával, hello fiók konfiguráció kezelése, és a runbookok tudják hitelesíteni magukat.

Automation-fiók létrehozása előtt egy jó ötlet toounderstand, és vegye figyelembe a következőket hello:

* Automation-fiók létrehozása nincs hatással lehet, hogy már létrehozott vagy a klasszikus hello, vagy a Resource Manager üzembe helyezési modellben az Automation-fiók.
* hello folyamat csak a hello Azure-portálon létrehozott Automation-fiók esetén használható. Kísérlet egy fiókot a klasszikus Azure portálon hello toocreate nem replikálja hello Futtatás mint fiók konfigurálásával.
* Ha már rendelkezik forgatókönyve és eszköze (például ütemezések vagy változók) hely toomanage hagyományos erőforrások, és azt szeretné, hogy a runbookok tooauthenticate hello új klasszikus futtató fiókhoz, hello alábbi módszerekkel:

  * toocreate klasszikus futtató fiókot, hello "Kezelése a futtató fiók" szakasz hello utasításait kövesse. 
  * tooupdate a meglévő fiók használata hello PowerShell parancsfájl-hello "Az Automation-fiók frissítése a PowerShell használatával" szakaszában.
* a meglévő runbookok hello hello szakaszban megadott példakódot toomodify szüksége tooauthenticate hello új futtató fiók és a klasszikus Futtatás mint Automation-fiók használatával, [hitelesítésikód-példák](#authentication-code-examples).

    >[!NOTE]
    >a Futtatás mint fiók hello erőforrás-kezelő erőforrásoknak hello-alapú szolgáltatás egyszerű hitelesítést szolgál. hello klasszikus Futtatás mint fiók felügyeleti tanúsítvánnyal szolgáltatásfelügyelet erőforrásokon hitelesítéséhez van.

## <a name="create-an-automation-account-from-hello-azure-portal"></a>Hello Azure-portálon az Automation-fiók létrehozása
Ebben a szakaszban hoz létre egy Azure Automation-fiók hello Azure-portálon, ami viszont hoz létre egy futtató fiókot és a klasszikus futtató fiókot.

>[!NOTE]
>Automation-fiók toocreate, a hello szolgáltatás-Rendszergazdák szerepkör tagjának vagy hello előfizetést biztosít hozzáférést toohello előfizetés társadminisztrátoraként kell lennie. Meg kell adni egy felhasználói toothat előfizetés alapértelmezett Active Directory-példányként. hello fióknak nem szükséges egy kiemelt szerepkörhöz hozzárendelt toobe.
>
>Ha nem tagja Active Directory-példányban hello előfizetés hello előfizetés társadminisztrátoraként szerepe toohello vannak adása előtt, hogy megkapja tooActive Directory vendégként. Ebben a példában kapni fog egy "nem rendelkezik engedélyekkel toocreate..." Figyelmeztetés-hello **Automation-fiók hozzáadása** panelen.
>
>Felhasználók, akik toohello közös rendszergazdai szerepkör először eltávolíthatja hello előfizetés Active Directory-példányban, és újból hozzáadták toomake hozzáadták azokat a teljes felhasználói az Active Directoryban. tooverify ebben a helyzetben a hello **Azure Active Directory** hello Azure-portálon kiválasztásával ablaktábláján **felhasználók és csoportok**, kiválasztásával **minden felhasználó** és kiválasztása után adott felhasználó hello kiválasztásával **profil**. hello értékének hello **felhasználótípust** attribútum hello felhasználó profil alapján nem értékűnek kell lennie **vendég**.
>

1. Jelentkezzen be az Azure portál egy olyan fiókkal, amely hello előfizetés Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként toohello.

2. Válassza az **Automation-fiókok** elemet.

3. A hello **Automation-fiók** panelen kattintson a **Hozzáadás**.
Hello **Automation-fiók hozzáadása** panel nyílik meg.

 ![hello "Hozzáadása Automation-fiók" panel](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > Ha a fiók nem hello előfizetés Rendszergazdák szerepkör tagja és hello előfizetés társadminisztrátoraként, hello a következő figyelmeztetés jelenik meg hello **Automation-fiók hozzáadása** panel:
   >
   >![Az Automation-fiókhoz kapcsolódó figyelmeztetés hozzáadása](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. A hello **Automation-fiók hozzáadása** paneljén, hello **neve** mezőbe írja be az új Automation-fiók nevét.

5. Ha egynél több előfizetéssel rendelkezik, a hello, a következő:

    a. A **előfizetés**, adjon meg egy új fiókot hello.

    b. Az **Erőforráscsoport** területen kattintson az **Új létrehozása** vagy a **Meglévő használata** elemre.

    c. A **Hely** területen adjon meg egy Azure-adatközpontot.

6. Az **Azure-beli futtató fiók létrehozása** területen válassza az **Igen** lehetőséget, majd kattintson a **Létrehozás** elemre.

   > [!NOTE]
   > Ha úgy dönt, nem toocreate hello Futtatás mint fiók kiválasztásával **nem**, megjelenik egy figyelmeztető üzenet hello **Automation-fiók hozzáadása** panelen. Bár a hello-fiók létrehozása az Azure-portálon hello nem rendelkezik a megfelelő hitelesítési identitását a klasszikus vagy erőforrás-kezelő előfizetési címtárszolgáltatás belül. Következésképpen hello fióknak nincs hozzáférési tooresources az előfizetésben. Ez megakadályozza, hogy az erre a fiókra hivatkozó runbookok hitelesítést végezhessenek, illetve feladatokat futtathassanak az ezekben az üzemi modellekben található erőforrások alapján.
   >
   > ![Figyelmeztető üzenet hello "Hozzáadása Automation-fiók" panelen](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > Emellett egyszerű hello szolgáltatást nem jön létre, mert hello közreműködői szerepkör nincs hozzárendelve.
   >

7. Azure Automation-fiók hello hoz létre, amíg előrehaladásának hello alatt **értesítések** hello menüből.

### <a name="resources"></a>Erőforrások
Ha hello Automation-fiók sikeresen létrejött, a több erőforrás automatikusan létrejönnek meg. a következő két tábla hello hello erőforrások foglalja össze:

#### <a name="run-as-account-resources"></a>Futtató fiók erőforrásai

| Erőforrás | Leírás |
| --- | --- |
| AzureAutomationTutorial forgatókönyv | Grafikus példaforgatókönyv, amely bemutatja, hogyan használatával tooauthenticate hello Futtatás mint fiókot, és minden hello Resource Managerhez tartozó erőforrások lekérése. |
| AzureAutomationTutorialScript forgatókönyv | PowerShell példaforgatókönyv, amely bemutatja, hogyan használatával tooauthenticate hello Futtatás mint fiókot, és minden hello Resource Managerhez tartozó erőforrások lekérése. |
| AzureRunAsCertificate | automatikusan létrejön egy Automation-fiók létrehozásakor, vagy használja a következő PowerShell-parancsfájl egy létező fiók hello hello tanúsítványeszköz. hello tanúsítvány teszi lehetővé az Azure-ral tooauthenticate, hogy a runbookok Azure Resource Manager-erőforrások kezelése. a tanúsítványnak hello egyéves-élettartamot. |
| AzureRunAsConnection | hello kapcsolódási eszköz, amely automatikusan létrejön egy Automation-fiók létrehozásakor vagy hello PowerShell-parancsfájl használata a meglévő fiók. |

#### <a name="classic-run-as-account-resources"></a>Klasszikus futtató fiók erőforrásai

| Erőforrás | Leírás |
| --- | --- |
| AzureClassicAutomationTutorial forgatókönyv | Grafikus példaforgatókönyv, amely lekérdezi a hello gépek hello klasszikus telepítési modell használatával egy előfizetésben hello klasszikus futtató fiók (tanúsítvány) használatával hozhatók létre, és ezután ír hello virtuális gép nevét és állapotát. |
| AzureClassicAutomationTutorial parancsprogram-forgatókönyv | PowerShell példaforgatókönyv, amely lekérdezi az összes előfizetést a klasszikus virtuális gépeinek hello hello klasszikus futtató fiók (tanúsítvány) használatával, és majd hello az írási műveletek a virtuális gép nevét és állapotát. |
| AzureClassicRunAsCertificate | automatikusan létrehozott hello tanúsítványeszköz használt tooauthenticate az Azure-ral, hogy a runbookok klasszikus Azure-erőforrások kezelése. a tanúsítványnak hello egyéves-élettartamot. |
| AzureClassicRunAsConnection | hello automatikusan létrehozott kapcsolódási eszköz használata tooauthenticate az Azure-ral, hogy a runbookok klasszikus Azure-erőforrások kezelése. |

## <a name="verify-run-as-authentication"></a>A futtató fiókos hitelesítés ellenőrzése
Hajtsa végre egy kis teszt tooconfirm, amely képes sikeresen hitelesíteni hello új futtató fiók használatával.

1. Hello Azure-portálon nyissa meg a korábban létrehozott hello Automation-fiók.

2. Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.

3. Jelölje be hello **AzureAutomationTutorialScript** runbookot, és kattintson a **Start** toostart hello runbook. a következő események hello fordulhat elő:
 * A [runbook-feladat](automation-runbook-execution.md) jön létre, hello **feladat** panel jelenik meg, és hello feladat állapota látható hello **feladat összegzése** csempére.
 * hello feladatállapot jön létre **várakozik**, azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toobecome érhető el a vár.
 * hello állapota lesz **indítása** Ha egy munkavégző jogcímek hello feladat.
 * hello állapota lesz **futtató** amikor hello runbook elindul.
 * Amikor hello runbook-feladat futása befejeződött, megjelenik egy állapotának **befejezve**.

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. toosee hello hello runbook részletes eredményét, kattintson a hello **kimeneti** csempére.  
Hello **kimeneti** panel jelenik meg, melyen hello runbook sikeresen hitelesített és hello erőforráscsoportban elérhető összes erőforrás listáját adja vissza.

5. Bezárás hello **kimeneti** panel tooreturn toohello **feladat összegzése** panelen.

6. Bezárás hello **feladat összegzése** panelen, a megfelelő hello **AzureAutomationTutorialScript** runbook panelen.

## <a name="verify-classic-run-as-authentication"></a>A klasszikus futtató fiókos hitelesítés ellenőrzése
Hajtsa végre a hasonló kis tooconfirm, amely képes sikeresen hitelesíteni hello új klasszikus futtató fiók segítségével tesztelheti.

1. Hello Azure-portálon nyissa meg a korábban létrehozott hello Automation-fiók.

2. Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.

3. Jelölje be hello **AzureClassicAutomationTutorialScript** runbookot, és kattintson a **Start** túl a hello runbook indítása. a következő események hello fordulhat elő:

 * A [runbook-feladat](automation-runbook-execution.md) jön létre, hello **feladat** panel jelenik meg, és hello feladat állapota látható hello **feladat összegzése** csempére.
 * hello feladatállapot jön létre **várakozik**, azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toobecome érhető el a vár.
 * hello állapota lesz **indítása** Ha egy munkavégző jogcímek hello feladat.
 * hello állapota lesz **futtató** amikor hello runbook elindul.
 * Amikor hello runbook-feladat futása befejeződött, megjelenik egy állapotának **befejezve**.

    ![Rendszerbiztonsági tag forgatókönyvtesztje](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. toosee hello hello runbook részletes eredményét, kattintson a hello **kimeneti** csempére.  
Hello **kimeneti** panel jelenik meg, melyen hello runbook sikeresen hitelesített, és visszahelyezi a klasszikus virtuális gépeinek listáját hello előfizetés.

5. Bezárás hello **kimeneti** panel tooreturn toohello **feladat összegzése** panelen.

6. Bezárás hello **feladat összegzése** panelen, a megfelelő hello **AzureAutomationTutorialScript** runbook panelen.

## <a name="managing-your-run-as-account"></a>Futtató fiók kezelése
Egy bizonyos ponton az Automation-fiók lejárta előtt toorenew hello tanúsítványt kell. Ha úgy véli, hogy a Futtatás mint fiók hello feltörték, törlése, és hozza létre újból. Ez a szakasz ismerteti hogyan tooperform ezeket a műveleteket.

### <a name="self-signed-certificate-renewal"></a>Önaláírt tanúsítvány megújítása
hello önaláírt tanúsítványt, amelyet létrehozott hello futtató fiók létrehozásának dátuma hello egy év lejár. A tanúsítványt bármikor meg lehet újítani a lejárata előtt. Amikor megújítja, a hello aktuális érvényes tanúsítványra, hogy legfeljebb vagy aktívan futó ütemezett, és az, hogy azokkal hello Futtatás mint fiókot, runbook érintett nem negatív megtartott tooensure. amíg a lejárati dátum nem érvényes marad hello tanúsítványt.

> [!NOTE]
> Ha konfigurálta az Automation Futtatás mint fiók toouse a vállalati hitelesítésszolgáltató által kiállított tanúsítványt, és ezt a beállítást használja, hello vállalati tanúsítvány helyébe egy önaláírt tanúsítványt.

toorenew hello tanúsítvány, a következő hello:

1. Hello Azure-portálon nyissa meg a hello Automation-fiók.

2. A hello **Automation-fiók** paneljén, hello **tulajdonságok fiók** ablaktáblán, a **Fiókbeállítások**, jelölje be **futtató fiókok**.

    ![Az Automation-fiók tulajdonságpanelje](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. A hello **futtató fiókok** tulajdonságok panelére léphet, válassza ki bármelyik hello futtató fiók vagy hello klasszikus Futtatás mint fiókot, amelyet toorenew hello tanúsítványt.

4. A hello **tulajdonságok** hello panelen kiválasztott fiókot, kattintson a **tanúsítvány megújítása**.

    ![Futtató fiók tanúsítványának megújítása](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. Hello tanúsítvány megújítása folyamatban van, amíg előrehaladásának hello alatt **értesítések** hello menüből.

### <a name="delete-a-run-as-or-classic-run-as-account"></a>Futtató fiók vagy klasszikus futtató fiók törlése
Ez a szakasz ismerteti, hogyan toodelete, majd hozza létre a Futtatás mint vagy a klasszikus futtató fiókot. Ez a művelet végrehajtásakor hello Automation-fiók őrződnek meg. A Futtatás mint vagy a klasszikus futtató fiók törlése után újra létrehozhatja a hello Azure-portálon.

1. Hello Azure-portálon nyissa meg a hello Automation-fiók.

2. A hello **Automation-fiók** hello fiók tulajdonságok panelen, jelölje be a panelt **futtató fiókok**.

3. A hello **futtató fiókok** tulajdonságok panelére léphet, válassza ki bármelyik hello futtató fiók vagy a klasszikus futtató fiókot, amelyet az toodelete. Ezután a hello **tulajdonságok** hello panelen kiválasztott fiókot, kattintson a **törlése**.

 ![Futtató fiók törlése](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. Hello fiók törlése folyamatban van, amíg előrehaladásának hello alatt **értesítések** hello menüből.

5. Hello fiók törlése után újra létrehozhatja a hello **futtató fiókok** hello kiválasztásával a Tulajdonságok panelére léphet létrehozása beállítást **Azure futtató fiók**.

 ![Hozza létre újból hello Automation futtató fiók](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a>Hibás konfiguráció
Néhány hello futtató vagy a klasszikus futtató fiók toofunction szükséges konfigurációs elemek megfelelő lehet, hogy törölték vagy helytelenül van létrehozva a kezdeti beállítás során. hello elemek a következők:

* Tanúsítványobjektum
* Kapcsolatobjektum
* A Futtatás mint fiók hello közreműködői szerepkör eltávolították.
* Egyszerű szolgáltatás vagy alkalmazás az Azure AD-ben

Az előző hello és helytelen konfigurálása más példányai, hello Automation-fiók észleli hello változik, és egy állapotát jeleníti meg az *hiányos* a hello **futtató fiókok** hello a Tulajdonságok panelen fiók.

![Hiányos futtatófiók-konfigurációs állapot](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

Hello Futtatás mint fiók kiválasztásakor hello fiók **tulajdonságok** ablaktáblán jelennek meg a következő hibaüzenet hello:

![Hiányos futtatófiók-konfigurációra figyelmeztető üzenet](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).

A Futtatás mint fiók problémák gyorsan megoldható törli, majd újra létrehozza a hello fiók.

## <a name="update-your-automation-account-by-using-powershell"></a>Az Automation-fiók frissítése a PowerShell használatával
Akkor használhatja PowerShell tooupdate a meglévő Automation-fiókot:

* Automation-fiók létrehozása, de elutasítása toocreate hello futtató fiókot.
* Már használja egy automatizálási fiókot toomanage Resource Managerhez tartozó erőforrások és tooupdate hello tooinclude hello futtató fiókok kívánt runbook-hitelesítéshez.
* Az automatizálási fiók toomanage hagyományos erőforrások már használja, és azt szeretné, hogy tooupdate azt toouse hello klasszikus futtató fiókot új fiók létrehozása és a forgatókönyve és eszköze tooit áttelepítése helyett.   
* Érdemes toocreate egy olyan futtató és a klasszikus futtató fiókot a vállalati hitelesítésszolgáltató (CA) által kiadott tanúsítvánnyal.

hello parancsfájl a következő előfeltételek hello rendelkezik:

* hello parancsfájl csak a Windows 10 és Windows Server 2016 futtatható Azure Resource Manager modulok 2.01 és újabb verziók. A korábbi Windows-verziók esetében nem támogatott.
* Az Azure PowerShell 1.0-s és újabb verziói. További információk hello PowerShell 1.0-s kiadásról: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* Automation-fiók, hello hello értékként hivatkozott *– AutomationAccountName* és *- ApplicationDisplayName* paramétereket a PowerShell-parancsfájl a következő hello.

tooget hello értékei *SubscriptionID*, *ResourceGroup*, és *AutomationAccountName*, amely hello parancsfájlok kötelező paraméterek tartoznak, a következő hello:
1. A hello Azure-portálon, válassza ki az Automation-fiókban lévő hello **Automation-fiók** panelt, és válassza **összes beállítás**. 
2. A hello **összes beállítás** panel alatt **Fiókbeállítások**, jelölje be **tulajdonságok**. 
3. Vegye figyelembe a hello hello értékek **tulajdonságok** panelen.

![hello Automation-fiók "Tulajdonságok" panelen](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a>Futtató fiókhoz használható PowerShell-szkript létrehozása
A PowerShell-parancsfájl a következő konfigurációk hello támogatását tartalmazza:

* Futtató fiók létrehozása önaláírt tanúsítvány használatával.
* Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával.
* Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával.
* Hozzon létre egy futtató fiókot és egy klasszikus futtató fiókot egy önaláírt tanúsítványt a hello Azure Government felhő.

Attól függően, hogy hello konfigurációs beállítást választja a hello parancsfájl a következő elemek hello hoz létre.

**Futtató fiókok esetén:**

* Létrehoz egy Azure AD alkalmazás toobe exportálni vagy hello önaláírt vagy vállalati tanúsítvány nyilvános kulcsát, az Azure AD-hoz létre egy egyszerű szolgáltatásfiók hello alkalmazáshoz, és rendel hello közreműködő szerepkört az aktuális hello fiókhoz előfizetés. Ez a beállítás tooOwner vagy bármilyen más szerepkör módosíthatja. További információk: [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).
* Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureRunAsCertificate* a hello megadva az Automation-fiók. hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello Azure AD-alkalmazás által használt tartalmazza.
* Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureRunAsConnection* a hello megadva az Automation-fiók. hello kapcsolódási eszköz rendelkezik hello applicationId, tenantId, előfizetés-azonosító és tanúsítvány-ujjlenyomatot.

**Klasszikus futtatófiókok esetében:**

* Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureClassicRunAsCertificate* a hello megadva az Automation-fiók. hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello felügyeleti tanúsítvány által használt tartalmazza.
* Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureClassicRunAsConnection* a hello megadva az Automation-fiók. hello kapcsolódási eszköz hello előfizetés nevét, a subscriptionId és a tanúsítvány eszköz neve tartalmazza.

>[!NOTE]
> Ha a beállítást vagy a klasszikus futtató fiók létrehozásához, hello parancsfájl végrehajtása után, feltöltés hello nyilvános tanúsítványtároló (.cer kiterjesztésű) toohello felügyeleti hello előfizetés adott hello Automation-fiók hozták létre.
> 

tooexecute parancsfájl hello és hello-tanúsítvány feltöltése, a következő hello:

1. Mentse a parancsfájlt a számítógépen a következő hello. Ebben a példában mentse hello Fájlnév *New-RunAsAccount.ps1*.

        #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds tooallow hello service principal application toobecome active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate hello ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload hello .cer format of #CERT# toohello Management store by following hello steps below." + [Environment]::NewLine +
                    "Log in toohello Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload hello .cer format of #CERT#"

             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate hello ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. A számítógépén kattintson a **Start** gombra, majd indítsa el a **Windows PowerShellt** emelt szintű felhasználói jogokkal.

3. A hello emelt szintű PowerShell parancssori felület, az 1. lépésben létrehozott hello parancsfájl tartalmazó lépjen toohello mappát.

4. Hajtsa végre a hello parancsfájlt hello paraméter értékének használatával hello konfiguráció szükséges.

    **Futtató fiók létrehozása önaláírt tanúsítvány használatával**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Hozzon létre egy futtató fiókot és egy klasszikus Futtatás mint fiók hello Azure Government felhő önaláírt tanúsítvány használatával**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Hello parancsfájl által végrehajtott, miután az Azure-ral felszólító tooauthenticate fogja. Olyan fiókkal jelentkezzen be, amely hello előfizetés Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként.
    >
    >

Miután hello parancsfájl végrehajtása sikeres, vegye figyelembe a hello következőket:
* Nyilvános önaláírt tanúsítvány (.cer-fájl) klasszikus futtató fiókot hozta létre, ha hello parancsfájl hoz létre, és a számítógép hello felhasználói profil toohello ideiglenes fájlok mappába menti *%USERPROFILE%\AppData\Local\Temp*, használt tooexecute hello PowerShell-munkamenetben.
* Ha egy (.cer formátumú) vállalati tanúsítvánnyal rendelkező klasszikus futtató fiókot hozott létre, használja ezt a tanúsítványt. Kövesse az utasításokat hello [feltöltése a felügyeleti API tanúsítvány toohello a klasszikus Azure portálon](../azure-api-management-certs.md), és szolgáltatásfelügyelet erőforrások hello hitelesítőadat-konfiguráció érvényesítése hello segítségével [példakód a szolgáltatás felügyeleti erőforrásról tooauthenticate](#sample-code-to-authenticate-with-service-management-resources). 
* Ha elvégezte *nem* klasszikus futtató fiók létrehozása, erőforrás-kezelő erőforrások a hitelesítést és hello hitelesítőadat-konfiguráció érvényesítése hello segítségével [példakód azonosítja a felügyeleti szolgáltatás erőforrások](#sample-code-to-authenticate-with-resource-manager-resources).

## <a name="sample-code-tooauthenticate-with-resource-manager-resources"></a>A minta kód tooauthenticate a Resource Managerhez tartozó erőforrások
Frissített hello alábbi példakód hello vett *AzureAutomationTutorialScript* példa runbook, a runbookokat hello Futtatás mint fiók toomanage Resource Managerhez tartozó erőforrások használatával tooauthenticate.

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get hello connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in tooAzure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context tooa specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }

több előfizetéssel is működnek, tooeasily toohelp, hello parancsfájl két további sornyi kód, amely támogatja a hivatkozó egy előfizetési kontextust is tartalmaz. Egy változó eszköz nevű *SubscriptionId* hello Azonosítóját hello előfizetés tartalmazza. Hello után `Add-AzureRmAccount` parancsmag utasítás hello [ `Set-AzureRmContext` ](/powershell/module/azurerm.profile/set-azurermcontext) parancsmag van megadva a hello paraméterhalmaz *- SubscriptionId*. Ha hello változó neve túl általános, javítsa ki a tooinclude előtag, vagy használja egy másik elnevezési egyezmény toomake azt könnyebb tooidentify. Másik lehetőségként használhatja a hello paraméterkészletet alakítanak *- SubscriptionName* helyett *- SubscriptionId* a megfelelő változóeszköz.

a hello runbook hitelesítéséhez használt parancsmag hello `Add-AzureRmAccount`, használ hello *ServicePrincipalCertificate* paraméterhalmaz. Hello szolgáltatás egyszerű tanúsítvány, nem a hello felhasználói hitelesítő adatok használatával hitelesíti.

## <a name="sample-code-tooauthenticate-with-service-management-resources"></a>A minta kód tooauthenticate szolgáltatásfelügyelet erőforrásokkal
Használhatja a következő frissített mintakód, amely hello forrása hello *AzureClassicAutomationTutorialScript* példa runbook, tooauthenticate klasszikus futtató fiókhoz, toomanage a hagyományos erőforrások hello segítségével a runbookok.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get hello connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate tooAzure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in hello Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting hello certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in hello Automation account."
    }

    Write-Verbose "Authenticating tooAzure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a>Következő lépések
* [Alkalmazás- és egyszerű szolgáltatási objektumok Azure AD-ben](../active-directory/active-directory-application-objects.md)
* [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md)
* [Azure Cloud Services – tanúsítványok áttekintése](../cloud-services/cloud-services-certs-create.md)
