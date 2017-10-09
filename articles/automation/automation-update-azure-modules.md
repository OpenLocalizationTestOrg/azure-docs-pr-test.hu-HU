---
title: aaaUpdate Azure az Azure Automationben modulok |} Microsoft Docs
description: "Ez a cikk ismerteti, hogyan most már frissítheti az Azure Automationben alapértelmezés szerint biztosított közös Azure PowerShell-modulok."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: afa404a643349f044e55be2280e435b575049dec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-azure-powershell-modules-in-azure-automation"></a>Hogyan tooupdate Azure PowerShell-modulok az Azure Automationben

az egyes Automation-fiók alapértelmezés szerint biztosított hello leggyakoribb Azure PowerShell-modulok.  hello Azure csapata frissítések rendszeresen, hello Azure modulok, így az Automation-fiók hello nyújtunk oly módon, tooupdate hello modulok hello fiók Ha új verzió hello portálról érhetők el.  

Modulok a hello termékcsoport rendszeresen frissíteni, mert negatívan befolyásolhatja a runbookokat hello típusú változás, például egy paraméter átnevezése vagy teljesen elavulttá parancsmag függően szereplő hello parancsmagok módosítások alakulhat ki. a runbookok és hello érintő tooavoid folyamatok automatizálása, teszteléséhez, és ellenőrizze a folytatás előtt erősen ajánlott.  Ha erre a célra készült dedikált Automation-fiók nem rendelkezik, fontolja meg, hozzon létre egyet, hogy számos különböző alkalmazási helyzetek és kombinációinak tesztelheti a runbookokat, továbbá tooiterative módosítások például hello frissítése hello fejlesztése során PowerShell-modulok.  Miután hello eredmények ellenőrzését, és telepítette a szükséges módosításokat, összehangolása szükséges módosítását runbookokat hello áttelepítésének folytatásához és hello frissítését éles alább leírtak szerint.     

## <a name="updating-azure-modules"></a>Az Azure modulok frissítése

1. Hello modulban panelen található az Automation-fiók nincs lehetőség nevű **frissítés Azure modulok**.  Mindig engedélyezve van.<br><br> ![Modulok panel Azure modulok beállítás frissítése](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

2. Kattintson a **frissítés Azure modulok** és az Ön számára jelenik meg, amely arra kéri, ha azt szeretné, toocontinue megerősítési értesítés.<br><br> ![Azure-modulok értesítési frissítése](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Kattintson a **Igen** és hello modul frissítési folyamat megkezdődik.  hello frissítési folyamat veszi 15-20 perc tooupdate hello modulok a következő kapcsolatban:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Ha hello modulok már toodate be, majd hello folyamat befejezi néhány másodpercen belül.  Hello frissítési folyamat befejezéséről értesítést fog kapni.<br><br> ![Azure-modulok frissítési állapotának frissítése](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> Azure Automation szolgáltatásbeli használni fog hello legújabb modulok az Automation-fiók egy új ütemezett feladat futtatásakor.    

Parancsmagok használatakor ezeket a runbookok toomanage Azure az Azure PowerShell-modulok az erőforrásokat, akkor érdemes tooperform a frissítési folyamat havonta vagy úgy, hogy rendelkezik tooassure hello legújabb modulok.

## <a name="next-steps"></a>Következő lépések

* További információ az integrációs modulok, és hogyan toocreate az egyéni modulok toofurther integrálhatja más rendszerek, a szolgáltatások vagy a megoldások, Automation toolearn lásd [integrációs modulok](automation-integration-modules.md).

* Érdemes lehet forrás vezérlő integrációs [GitHub vállalati](automation-scenario-source-control-integration-with-github-ent.md) vagy [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) toocentrally kezelésére, és szabályozhatja a automatizálási runbook és konfigurációs portfóliót kiadásaiban.  
