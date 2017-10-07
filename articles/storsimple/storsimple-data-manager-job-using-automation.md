---
title: aaaUse Azure Automation tootrigger egy feladat |} Microsoft Docs
description: "Megtudhatja, hogyan toouse Azure Automation indítására, a StorSimple adatkezelő feladatok (magán előnézetben)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 0d9d6e5e6d52ed27ca759ed7e949b5f5dfd319c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-automation-tootrigger-a-job-private-preview"></a>Azure Automation tootrigger egy feladat (magán előnézetben) használata

Ez a cikk ismerteti, hogyan toouse Azure Automation tootrigger egy StorSimple adatkezelő feladat.

## <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, győződjön meg arról, hogy:

*   Az Azure Powershell telepítése. [Töltse le az Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Konfigurációs beállítások tooinitialize hello Data Transformation feladat (utasításokat tooobtain ezek a beállítások tartoznak ide).
*   A feladat definíciójához erőforráscsoporton belül egy hibrid adatforrás, melyhez a megfelelően konfigurált.
*   Töltse le `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) hello github-tárházban a fájlt.
*   Töltse le `Get-ConfigurationParams.ps1` [parancsfájl](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) a hello github-tárházban.
*   Töltse le `Trigger-DataTransformation-Job.ps1` [parancsfájl](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) a hello github-tárházban.

## <a name="step-by-step"></a>Lépésről lépésre

### <a name="get-azure-active-directory-permissions-for-hello-automation-job-toorun-hello-job-definition"></a>Azure Active Directory-engedélyek hello automation feladat toorun hello feladat definíciójának beolvasása

1. tooretrieve hello konfigurációs paramétereket az Active Directory hello a következő lépéseket:

    1. Nyissa meg a Windows PowerShell a helyi gépen. Győződjön meg arról, hogy [Azure PowerShell](https://azure.microsoft.com/downloads/) telepítve van.
    1. Futtassa a hello `Get-ConfigurationParams.ps1` parancsfájl (a fent letöltött hello mappában). Írja be a következő parancs hello PowerShell-ablakban hello:

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        hello ActiveDirectoryKey egy jelszót, amely későbbi használatra. Adjon meg egy tetszőleges jelszót. Alkalmazásnév karakterlánc lehet.

2. A parancsfájl kimenete a következő értékeket kell használni a hello automatizálási runbook indítására hello. Jegyezze fel ezeket az értékeket.

    - Ügyfél-azonosító
    - Bérlőazonosító
    - Active Directory-kulcs (azonos a fent megadott egyik hello)
    - Előfizetés azonosítója

### <a name="set-up-hello-automation-account"></a>Hello Automation-fiók beállítása

1. Jelentkezzen be tooAzure, és nyissa meg az Automation-fiók.
2. Kattintson a **eszközök** csempe tooopen hello eszközök listája.
3. Kattintson a **modulok** csempe tooopen hello listájához.
4. Kattintson a **+ a modul hozzá lesz adva** gombra és hello Hozzáadás modul panel nincs elindítva.

    ![Automation-fiók beállításai](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. Miután kiválasztotta a hello `DataTransformationApp.zip` fájlt a helyi számítógépről, kattintson **OK** tooimport hello modul.

   Azure Automation importálására a modul tooyour fiókot, ha hello modullal kapcsolatos metaadatok bontja ki. Ez a művelet eltarthat néhány percig.

   ![Automation-fiók beállításai](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. Értesítést kaphat, hogy hello modul telepítése és egy másik értesítés hello folyamat be nem fejeződik.  Ellenőrizheti a hello állapota **modulok** csempére.

### <a name="tooimport-hello-runbook-that-triggers-hello-job-definition"></a>hello feladatdefiníció kiváltó tooimport hello runbook

1. Hello Azure-portálon nyissa meg az Automation-fiók.
2. Kattintson a **Runbookok** csempe tooopen hello forgatókönyvek listája.
3. Kattintson a **+ Hozzáadás runbook** , majd **meglévő forgatókönyv importálása**.

   ![Meglévő forgatókönyv importálása](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. Kattintson a **Runbook fájl** és select hello fájl tooimport `Trigger-DataTransformation-Job.ps1`.
5. Kattintson a **létrehozása** tooimport hello runbook. hello új runbook megjelenik hello runbookokat hello Automation-fiók esetében.
7. Kattintson a **eseményindító-DataTransformation-feladat** runbookot, majd **szerkesztése**.
8. Kattintson a **közzététel** , majd **Igen** során a rendszer megerősítést kér.


### <a name="toorun-hello-runbook"></a>toorun hello runbook:
1. Hello Azure-portálon nyissa meg az Automation-fiók.
2. Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.
3. Kattintson a **eseményindító-DataTransformation-feladat**.
4. Kattintson a **Start** toostart hello runbook.

   ![Forgatókönyv indítása](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. A hello **indítsa el a runbook** panelen adja meg az összes hello paramétereket. Kattintson a **OK** toosubmit hello Data Transformation feladat.

   ![Forgatókönyv indítása](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a>Következő lépések

[StorSimple adatokat kezelő felhasználói felületén tootransform használja az adatok](storsimple-data-manager-ui.md).
