---
title: "Azure Automation aaaRole-alapú hozzáférés-vezérlés |} Microsoft Docs"
description: "A Szerepköralapú hozzáférés-vezérlés (RBAC) hozzáférés-vezérlést biztosít az Azure-erőforrásokhoz. Ez a cikk ismerteti, hogyan mentése az Azure Automationben RBAC tooset."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: "automation rbac, szerepköralapú hozzáférés-vezérlés, azure rbac"
ms.assetid: 04b5625e-0ee8-4b5b-85cd-7734c1b3d4a3
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;sngun
ms.openlocfilehash: 051438e44d0c5c514d6dbaac5a312344ee311cdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-in-azure-automation"></a>Szerepköralapú hozzáférés-vezérlés az Azure Automationben
## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés
A Szerepköralapú hozzáférés-vezérlés (RBAC) hozzáférés-vezérlést biztosít az Azure-erőforrásokhoz. Használatával [RBAC](../active-directory/role-based-access-control-configure.md), a csapat belül is elkülönítse a feladatokat, és támogatás csak hello hozzáférés toousers, csoportok és alkalmazások, hogy be kell tooperform a feladatokat a mennyisége. Szerepköralapú hozzáférés-toousers hello Azure-portálon, az Azure parancssori eszközöket vagy Azure szolgáltatásfelügyeleti API használatával engedélyezhetők.

## <a name="rbac-in-automation-accounts"></a>RBAC Automation-fiókokban
Az Azure Automationben hozzáférést kapnak a hello megfelelő RBAC szerepkör toousers, csoportok és hello Automation-fiók hatókörének alkalmazásokat hozzárendelésével. Következő Automation-fiók által támogatott beépített szerepkörök vannak hello:  

| **Szerepkör** | **Leírás** |
|:--- |:--- |
| Tulajdonos |hello tulajdonosi szerepkör lehetővé teszi, hogy tooall erőforrások eléréséhez és műveletek belül egy Automation-fiók, beleértve a hozzáférési tooother felhasználók, csoportok és alkalmazások toomanage hello Automation-fiók megadása. |
| Közreműködő |hello közreműködői szerepkör lehetővé teszi, hogy toomanage más felhasználó módosítása kivételével mindent hozzáférési engedélyek tooan Automation-fiók. |
| Olvasó |hello olvasó szerepkör lehetővé teszi tooview összes hello erőforrása egy automatizálási fiókot, de nem módosíthatja. |
| Automation-operátor |hello Automation kezelői szerepkör lehetővé teszi tooperform működtetési feladatok például start, állítsa le, felfüggesztése, folytatása és feladatok ütemezése. Ez a szerepkör akkor hasznos, ha azt szeretné, tooprotect az Automation-fiók erőforrások, például a hitelesítő adatok eszközök és nem runbookok megtekinteni vagy módosítani, de továbbra is lehetővé teszik a szervezet tooexecute tagjai ezeknél a runbookoknál. |
| Felhasználói hozzáférés rendszergazdája |hello felhasználói hozzáférés adminisztrátora szerepkör lehetővé teszi toomanage felhasználói hozzáférés tooAzure Automation-fiók. |

> [!NOTE]
> Hozzáférési jogok tooa adott runbook vagy runbookok, csak a toohello erőforrások és a műveletek hello Automation-fiók belül nem tudja biztosítani.  
> 
> 

Ebben a cikkben a Microsoft végigvezeti hogyan mentése az Azure Automationben RBAC tooset. Első, de most, hogy azt hogy bárki megadása előtt alaposan megismernie a hello egyéni engedélyek megadott toohello közreműködő, olvasó, az automatizálási operátor és a felhasználói hozzáférés adminisztrátora nézze a közelebb hajtsa végre a megfelelő tartalomvédelmi toohello Automation-fiók.  Ha ezt elmulasztja, a hozzáférés biztosítása nem tervezett és nem kívánt következményekkel járhat.     

## <a name="contributor-role-permissions"></a>Közreműködői szerepkör engedélyei
hello alábbi táblázat mutatja be hello közreműködő szerepkört az automatizálás által végrehajtható műveleteket hello.

| **Erőforrás típusa** | **Olvasás** | **Írás** | **Törlés** | **Egyéb műveletek** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Automation-fiók |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation tanúsítvány-adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation kapcsolat-adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation kapcsolattípus-adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation hitelesítési adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation ütemezési adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation változó-adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation kívánt állapotának konfigurálása | | | |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |
| Hibrid forgatókönyv-feldolgozó erőforrástípus |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Azure Automation-feladat |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |
| Automation-feladatstream |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-feladatütemezés |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation-modul |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |
| Azure Automation-runbook |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |
| Automation-runbookvázlat |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |
| Automation-runbookvázlat tesztfeladat |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |
| Automation-webhook |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |

## <a name="reader-role-permissions"></a>Olvasói szerepkör engedélyei
hello alábbi táblázat mutatja be hello olvasó szerepkört az automatizálás által végrehajtható műveleteket hello.

| **Erőforrás típusa** | **Olvasás** | **Írás** | **Törlés** | **Egyéb műveletek** |
|:--- |:--- |:--- |:--- |:--- |
| Hagyományos előfizetés-adminisztrátor |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Felügyeleti zárolás |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Engedély |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Szolgáltatói műveletek |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Szerepkör-kijelölés |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Szerepkör-definíció |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="automation-operator-role-permissions"></a>Automation-operátori szerepkör engedélyei
hello alábbi táblázat mutatja be hello konkrét műveletek az automatizálás hello Automation operátor szerepkör végzi el.

| **Erőforrás típusa** | **Olvasás** | **Írás** | **Törlés** | **Egyéb műveletek** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Automation-fiók |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation tanúsítvány-adategység | | | | |
| Automation kapcsolat-adategység | | | | |
| Automation kapcsolattípus-adategység | | | | |
| Automation hitelesítési adategység | | | | |
| Automation ütemezési adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automation változó-adategység | | | | |
| Automation kívánt állapotának konfigurálása | | | | |
| Hibrid forgatókönyv-feldolgozó erőforrástípus | | | | |
| Azure Automation-feladat |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |
| Automation-feladatstream |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-feladatütemezés |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automation-modul | | | | |
| Azure Automation-runbook |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-runbookvázlat | | | | |
| Automation-runbookvázlat tesztfeladat | | | | |
| Automation-webhook | | | | |

További részletek hello [Automation operátor műveletek](../active-directory/role-based-access-built-in-roles.md#automation-operator) listák hello hello Automation operátor szerepköre hello Automation-fiók és az erőforrások által támogatott műveleteket.

## <a name="user-access-administrator-role-permissions"></a>Felhasználói hozzáférés rendszergazdája szerepkör engedélyei
hello alábbi táblázat mutatja be hello konkrét műveletek az automatizálás hello felhasználói hozzáférés adminisztrátora szerepkör végzi el.

| **Erőforrás típusa** | **Olvasás** | **Írás** | **Törlés** | **Egyéb műveletek** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Automation-fiók |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation tanúsítvány-adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation kapcsolat-adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation kapcsolattípus-adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation hitelesítési adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation ütemezési adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation változó-adategység |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation kívánt állapotának konfigurálása | | | | |
| Hibrid forgatókönyv-feldolgozó erőforrástípus |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Azure Automation-feladat |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-feladatstream |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-feladatütemezés |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-modul |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Azure Automation-runbook |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-runbookvázlat |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-runbookvázlat tesztfeladat |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-webhook |![Zöld állapot](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="configure-rbac-for-your-automation-account-using-azure-portal"></a>Az Automation-fiókhoz tartozó RBAC konfigurálása az Azure portál segítségével
1. Jelentkezzen be toohello [Azure Portal](https://portal.azure.com/) , és nyissa meg az Automation-fiók hello Automation-fiók paneljén.  
2. Kattintson a hello **hozzáférés** vezérlő hello bal felső sarokban. Ekkor megnyílik a hello **felhasználók** adhat hozzá új felhasználók, csoportok és alkalmazások toomanage az Automation-fiók panelen és nézet konfigurálhatja a meglévő szerepek hello Automation-fiók.  
   
   ![Hozzáférés gomb](media/automation-role-based-access-control/automation-01-access-button.png)  

> [!NOTE]
> **Előfizetés rendszergazdái** hello alapértelmezett felhasználóként már létezik. hello előfizetés rendszergazdái active directory-csoport hello szolgáltatás rendszergazdájával vagy rendszergazdáival és az Azure-előfizetéshez tartozó co-administrator(s) tartalmaz. hello szolgáltatás-rendszergazdák az Azure-előfizetés és az erőforrások hello tulajdonosa, és fog hello tulajdonosi szerepkör örökölt vannak az automation-fiók hello túl. Ez azt jelenti, hogy van-e hello hozzáférés **örökölt** a **szolgáltatás-rendszergazdák és a társadminisztrátoroknak** előfizetés és az **hozzárendelt** összes hello más felhasználók számára. Kattintson a **előfizetés rendszergazdái** tooview további részleteket a rájuk vonatkozó engedélyek.  
> 
> 

### <a name="add-a-new-user-and-assign-a-role"></a>Új felhasználó hozzáadása és szerepkör hozzárendelése
1. Hello felhasználók paneljén kattintson **Hozzáadás** tooopen hello **Hozzáadás hozzáférési panel** ahol hozzáadhat egy felhasználó, csoport vagy alkalmazás, és hozzárendelése egy szerepkörhöz toothem.  
   
   ![Felhasználó hozzáadása](media/automation-role-based-access-control/automation-02-add-user.png)  
2. Hello elérhető szerepkörök listájából válassza ki egy szerepkört. Azt fogja választani hello **olvasó** szerepkör, de választhat bármelyik hello elérhető beépített szerepkörök, amely támogatja az Automation-fiók vagy bármilyen definiált egyéni biztonsági szerepkört.  
   
   ![Szerepkör kiválasztása](media/automation-role-based-access-control/automation-03-select-role.png)  
3. Kattintson a **felhasználók hozzáadása az** tooopen hello **felhasználók hozzáadása az** panelen. Ha a felhasználók, csoportok vagy alkalmazások toomanage hozzáadott az előfizetéshez, akkor azoknak a felhasználóknak vannak felsorolva, és kiválaszthatja a tooadd hozzáférést. Ha nincs a felhasználók felsorolt, vagy ha érdekli hozzáadása hello felhasználó nem szerepel majd **meghívása** tooopen hello **hívhat meg vendégként** panel, ahol hívhat meg egy érvényes Microsoft-fiókkal rendelkező felhasználó e-mail címe például Outlook.com, OneDrive vagy Xbox Live ID azonosítót. Miután megadta a hello hello felhasználó e-mail címét, kattintson **kiválasztása** tooadd hello felhasználó, és kattintson a **OK**. 
   
   ![Felhasználók hozzáadása](media/automation-role-based-access-control/automation-04-add-users.png)  
   
   Most látnia kell a hello olyan hozzáadott felhasználó toohello **felhasználók** hello panelről **olvasó** szerepkörrel.  
   
   ![Felhasználók listázása](media/automation-role-based-access-control/automation-05-list-users.png)  
   
   Is hozzárendelhet egy szerepkör toohello felhasználót hello **szerepkörök** panelen. 
4. Kattintson a **szerepkörök** a hello felhasználók panel tooopen hello **szerepkörök panelen**. Ezen a panelen megtekintheti hello szerepkör, felhasználók és csoportok toothat szerepét hello száma hello nevét.
   
    ![Szerepkör hozzárendelése a Felhasználók panelről](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
   > [!NOTE]
   > Szerepköralapú hozzáférés-vezérlés állítható hello Automation-fiók szint csak, és nem a bármely alábbi erőforrás hello az Automation-fiók.
   > 
   > 
   
    Egynél több tooa felhasználói szerepkör, csoport vagy alkalmazás rendelhet. Például, ha jelenleg felvenni hello **Automation operátor** együtt hello szerepkör **olvasó szerepkört** toohello felhasználó, akkor azok is hello Automation erőforrások megtekintése, valamint hello runbook-feladatok végrehajtása. Hello legördülő tooview szerepkörrel toohello felhasználó listája bővítheti.  
   
    ![Több szerepkör megtekintése](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  

### <a name="remove-a-user"></a>Felhasználó eltávolítása
Egy felhasználó, aki nem kezel hello Automation-fiók, vagy akik már nem működik a hello szervezet hello engedéllyel is eltávolíthat. Következő lépések tooremove vannak hello egy felhasználó: 

1. A hello **felhasználók** panelen, jelölje be hello szerepkör-hozzárendelés, hogy kívánja-e tooremove.
2. Kattintson a hello **eltávolítása** hello hozzárendelés részleteit megjelenítő panelen gombjára.
3. Kattintson a **Igen** tooconfirm eltávolítása. 
   
   ![Felhasználók eltávolítása](media/automation-role-based-access-control/automation-08-remove-users.png)  

## <a name="role-assigned-user"></a>Szerepkörrel ellátott felhasználó
Amikor egy felhasználó lehet hozzárendelve tooa szerepkör tootheir Automation-fiók bejelentkezése, most láthatják hello tulajdonos fiókját hello listájában **alapértelmezett könyvtárak**. A sorrend tooview hello a hozzáadott Automation-fiók azok hello alapértelmezett címtár toohello tulajdonos alapértelmezett címtár kell váltania.  

![Alapértelmezett könyvtár](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### <a name="user-experience-for-automation-operator-role"></a>Automation-operátori szerepkör felhasználói élménye
Ha egy felhasználó, aki a hozzárendelt toohello Automation operátori szerepkör nézetek hello Automation-fiók vannak rendelve, csak hello listájának megtekintése a runbookok a következőkre, a runbook-feladatok és az ütemezések hello Automation-fiók hozott létre, de nem tekintheti meg azok definícióját. Ezek elindíthatja, állítsa le, felfüggesztése, folytatása vagy hello runbook-feladat ütemezése. hello felhasználónak nem kell tooother például konfigurációk, hibrid feldolgozó csoportokat vagy a DSC-csomópontok Automation erőforrások eléréséhez.  

![Nincs hozzáférés tooresourcres](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

Ha hello felhasználó hello runbook kattint, hello parancsok tooview forrás hello vagy hello runbook szerkesztése nincsenek megadva, hello Automation kezelői szerepkör nem teszi lehetővé a hozzáférést toothem.  

![Nincs hozzáférés tooedit runbook](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

hello felhasználói hozzáférés tooview és toocreate ütemezések fog rendelkezni, de más eszköz típusa nem lehet hozzáférési tooany.  

![Nincs hozzáférés tooassets](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Ez a felhasználó is nem rendelkezik hozzáférési tooview hello webhookok társított runbook

![Nincs hozzáférés toowebhooks](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## <a name="configure-rbac-for-your-automation-account-using-azure-powershell"></a>Az Automation-fiókhoz tartozó RBAC konfigurálása az Azure PowerShellel
Szerepköralapú hozzáférés-konfigurált tooan Automation-fiók is lehet hello alábbi [Azure PowerShell-parancsmagok](../active-directory/role-based-access-control-manage-access-powershell.md).

• A [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) felsorolja az Azure Active Directoryban elérhető összes RBAC-szerepkört. A paranccsal együtt hello **neve** tulajdonság toolist összes hello egy adott szerepkör által végrehajtható műveleteket.  
    **Példa**  
    ![Szerepkör-definíció lekérése](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) listák hello: Azure AD RBAC szerepkör-hozzárendelések megadott hatókörben. Paraméter nélkül a parancs visszaadja az összes hello szerepkör-hozzárendelések hello előfizetés alapján. Használjon hello **ExpandPrincipalGroups** paraméter toolist hozzáférés hozzárendelések hello adni a felhasználó, valamint a hello csoportok hello felhasználó a tagja.  
    **Példa:** használata hello következő toolist parancsot a hello minden felhasználó és az automation-fiók szerepkörét.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Szerepkör-kijelölés lekérése](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) tooassign hozzáférési toousers, csoportok és alkalmazások tooa adott hatókör.  
    **Példa:** használata hello következő parancsot a tooassign hello "Automation-kezelő" szerepkör hello Automation-fiók hatókör felhasználójának.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish toogrant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Új szerepkör hozzárendelése](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• Az [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) tooremove hozzáférést a megadott felhasználó, csoport vagy egy adott hatókör alkalmazást.  
    **Példa:** használata hello következő parancsot a hello "Automation-kezelő" szerepkör az Automation-fiók hatókör hello tooremove hello felhasználót.

    Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish tooremove> -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

A fenti példák hello, cserélje le **bejelentkezés Id**, **előfizetés-azonosítóval**, **erőforráscsoport-név** és **Automation-fiók neve** a a fiók adatait. Válasszon **Igen** amikor tooconfirm kéri a felhasználói szerepkör-hozzárendelés tooremove folytatása előtt.   

## <a name="next-steps"></a>Következő lépések
* Azure Automation a különböző módokon tooconfigure RBAC információkért tekintse meg túl[kezelése az Azure PowerShell RBAC](../active-directory/role-based-access-control-manage-access-powershell.md).
* A különböző módokon toostart egy runbookot, lásd: [runbook elindítása](automation-starting-a-runbook.md)
* Különböző runbooktípusokkal kapcsolatos információkért tekintse meg túl[Azure Automation-runbook típusok](automation-runbook-types.md)

