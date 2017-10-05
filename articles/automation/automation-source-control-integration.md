---
title: "Verziókövetés integrálása az Azure Automation forrás |} Microsoft Docs"
description: "Ez a cikk ismerteti a verziókövetés integrálása az Azure Automation a Githubon."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 224d7375-9887-44dd-b137-06ffe396a4b4
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: magoedte;sngun
ms.openlocfilehash: 763bf5805e3a3cb95ad63c7a354dd3d4cd531b2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="source-control-integration-in-azure-automation"></a>Verziókövetés integrálása az Azure Automation szolgáltatásban
Verziókövetés integrálása lehetővé teszi az Automation-fiók egy GitHub verziókövetési tárházat a runbookok hozzárendelni. A verziókövetési rendszerrel lehetővé teszi, hogy egyszerűen együttműködni a csapatával, követni a változásokat, és visszaállíthatja a runbookok korábbi verzióiban. Például verziókezelő lehetővé teszi a fejlesztői, tesztelési vagy éles Automation-fiók, így könnyen kódot, amely a fejlesztési környezetet az éles tárhely Automation tesztelését elősegítő forrás vezérlőelemet ággal szinkronizálása fiók.

A verziókövetési rendszerrel lehetővé teszi leküldéses kód Azure Automation-ellenőrzés, vagy a runbookok verziókövetésből az Azure Automation lekéréses. Ez a cikk ismerteti az Azure Automation-környezetben a verziókövetés beállítása. Először a GitHub-adattár eléréséhez, és különböző műveleteket végezheti el bízná Azure Automation konfigurálásával verziókövetés integrálása használatával. 

> [!NOTE]
> A verziókövetési rendszerrel támogatja húzza és kérdez le [PowerShell munkafolyamat-forgatókönyvekről](automation-runbook-types.md#powershell-workflow-runbooks) , valamint [PowerShell-forgatókönyvek](automation-runbook-types.md#powershell-runbooks). [Grafikus forgatókönyvek](automation-runbook-types.md#graphical-runbooks) még nem támogatottak.<br><br>
> 
> 

Nincsenek konfigurálásához verziókövetésének az Automation-fiók, és csak egyet, ha már rendelkezik egy GitHub-fiók szükséges két egyszerű lépéseket. Ezek a következők:

## <a name="step-1--create-a-github-repository"></a>1. lépés – a GitHub-tárház létrehozása
Ha már rendelkezik egy GitHub-fiók és a tárház, amely kapcsolódik az Azure Automation, majd jelentkezzen be a meglévő fiókot, és indítsa el az alábbi 2. lépés. Ellenkező esetben nyissa meg [GitHub](https://github.com/), létrehoz egy új fiókot és [hozzon létre egy új tárház](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>A 2 – az Azure Automationben Verziókövetés beállítása
1. Az Azure-portálon Automation-fiók paneljén kattintson **Verziókövetés beállítása.** 
   
    ![Verziókövetés beállítása](media/automation-source-control-integration/automation_01_SetUpSourceControl.png)
2. A **verziókezelő** panel megnyitása, amelyen konfigurálhatja a GitHub-fiók adatait. Alább látható a konfigurálandó paraméterek listája:  
   
   | **A paraméter** | **Leírás** |
   |:--- |:--- |
   | Forrás kiválasztása |Válasszon ki forrást. Jelenleg csak **GitHub** esetén támogatott. |
   | Engedélyezés |Kattintson a **engedélyezés** Azure Automation-hozzáférést biztosít a GitHub-tárház gombra. Ha már bejelentkezett GitHub-fiókjában másik ablakban, a fiók hitelesítő adatait fogja használni. Engedélyezési végrehajtása sikeres, a panel fognak megjelenni a GitHub-felhasználónevét alatt **engedélyezési tulajdonság**. |
   | Válassza ki a tárházban |Válasszon ki egy GitHub-tárházban elérhető adattárak a listából. |
   | Válassza ki a fiókiroda |Válasszon ki egy ágat elérhető ággal a listából. Csak a **fő** fiókirodai látható, ha még nem hozott létre bármely ágak. |
   | Runbook-mappa elérési útja |A runbook-mappa elérési útját adja meg az elérési út a GitHub-tárházban, amelyből el kívánja leküldéses és lekéréses a kódot. A formátumban kell megadni **/mappanév/almappanév**. Az Automation-fiók csak a runbook-mappa elérési útját a runbookok lesznek szinkronizálva. Runbookok a runbook-mappa elérési útja almappáiban fog **nem** szinkronizálva. Használjon  **/**  alatt a tárházban lévő runbookok szinkronizálására. |
3. Például, ha a tárház nevű **PowerShellScripts** , amely tartalmaz egy nevű mappát **RootFolder**, amely tartalmaz egy nevű mappát **almappa**. A következő karakterláncok használatával mappa szintenként szinkronizálása:
   
   1. A szinkronizálás runbookok **tárház**, runbook-mappa elérési útja*/*
   2. A szinkronizálás runbookok **RootFolder**, runbook-mappa elérési út */RootFolder*
   3. A szinkronizálás runbookok **almappa**, runbook-mappa elérési út */RootFolder/almappa*.
4. Miután konfigurálta a paramétereket, akkor jelenik meg a **Verziókövetés beállítása panelen.**  
   
    ![Konfigurálja a panelen](media/automation-source-control-integration/automation_02_SourceControlConfigure.png)
5. Után kattintson az OK gombra, a verziókövetés integrálása lett konfigurálva az Automation-fiók, és a GitHub-adataihoz frissíteni kell. Most kattintson erre a részre összes a forrás vezérlő szinkronizálási feladat előzményeinek megtekintése.  
   
    ![Tárház értékek](media/automation-source-control-integration/automation_03_RepoValues.png)
6. Verziókövetés beállítása után a következő Automation-erőforrások létrejön az Automation-fiók:  
   Két [változó eszközök](automation-variables.md) jönnek létre.  
   
   * A változó **Microsoft.Azure.Automation.SourceControl.Connection** a kapcsolati karakterlánc értékeket tartalmazza az alább látható módon.  
     
     | **A paraméter** | **Érték** |
     |:--- |:--- |
     | Név |Microsoft.Azure.Automation.SourceControl.Connection |
     | Típus |Karakterlánc |
     | Érték |{"Ág":\<*a fióknév*>, "RunbookFolderPath":\<*mappájának elérési*>, "Szolgáltatótípus":\<*az 1 értékű GitHub*>, "Tárház":\<*a tárház nevét*>, "Felhasználónév":\<*a GitHub felhasználói név*>} |

    * A változó **Microsoft.Azure.Automation.SourceControl.OAuthToken**, a OAuthToken biztonságos titkosított értékét tartalmazza.  

    |**A paraméter**            |**Érték** |
    |:---|:---|
    | Név  | Microsoft.Azure.Automation.SourceControl.OAuthToken |
    | Típus | Unknown(Encrypted) |
    | Érték | <*Titkosított OAuthToken*> |  

    ![Változók](media/automation-source-control-integration/automation_04_Variables.png)  

    * **Automatizálási verziókezelő** GitHub-fiókjában engedélyezett alkalmazás meg van adva. Az alkalmazás megtekintése: a Githubon kezdőlapról lépjen a **profil** > **beállítások** > **alkalmazások**. Ez az alkalmazás lehetővé teszi, hogy az Azure Automation a GitHub-tárházban az Automation-fiók szinkronizálása.  

    ![Git-alkalmazás](media/automation-source-control-integration/automation_05_GitApplication.png)


## <a name="using-source-control-in-automation"></a>A verziókövetési rendszerrel az automatizálás használatával
### <a name="check-in-a-runbook-from-azure-automation-to-source-control"></a>Bejelentkezés egy Azure Automation forgatókönyv verziókövetési
Runbook be lehetővé teszi a a verziókövetési tárházat az Azure Automation forgatókönyv végzett módosításokat. Az alábbiakban be runbook lépéseit:

1. Az Automation-fiók a [hozzon létre egy új szöveges forgatókönyvként](automation-first-runbook-textual.md), vagy [egy meglévő, szöveges runbook szerkesztése](automation-edit-textual-runbook.md). Ez a forgatókönyv egy PowerShell-munkafolyamat vagy a PowerShell parancsfájl runbook lehet.  
2. A runbook szerkesztése után mentse, és kattintson a **be** a a **szerkesztése** panelen.  
   
    ![Beadása gomb](media/automation-source-control-integration/automation_06_CheckinButton.png)

     > [!NOTE] 
     > Bejelentkezés Azure Automation felülírja a kód jelenleg már szerepel a verziókövetési rendszerrel. A Git egyenértékű parancssori parancsot be van **git hozzáadása + a git commit + git push**  

1. Amikor rákattint **be**, egy megerősítést kérő üzenet kéri, kattintson az Igen gombra a folytatáshoz.  
   
    ![Beadása üzenet](media/automation-source-control-integration/automation_07_CheckinMessage.png)
2. Bejelentkezés elindítja a forrás vezérlő runbookot: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Ez a forgatókönyv GitHub csatlakozik, és leküldéses értesítések módosítások Azure Automation a tárházhoz. A bejelentkezési feladatelőzmények megtekintéséhez lépjen vissza a **verziókövetés integrálása** lapon, majd kattintson a tárház szinkronizálási panel megnyitásához. Ezen a panelen láthatók a verziókövetési feladatok.  Jelölje ki a feladatot, megtekintéséhez, és kattintson a részletek megtekintéséhez.  
   
    ![Beadása Runbook](media/automation-source-control-integration/automation_08_CheckinRunbook.png)
   
   > [!NOTE]
   > Forrás ellenőrzési forgatókönyvek olyan speciális tudja megtekinteni vagy szerkeszteni Automation-forgatókönyv. Amíg azok nem jelennek meg a runbook listáján, látni fogja a szinkronizálási feladatokat jelenik meg a feladatok listáján.
   > 
   > 
3. A módosított runbook nevét a be runbook küldött bemeneti paraméterként. Is [feladat részleteinek megtekintéséhez](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) runbook kibontásával **tárház szinkronizálási** panelen.  
   
    ![Beadása bemeneti](media/automation-source-control-integration/automation_09_CheckinInput.png)
4. Frissítse a GitHub-tárházban, amint az a feladat befejeződik, a módosítások megtekintéséhez.  Kell egy véglegesítési véglegesítési üzenetet a tárházban: **frissített *Runbook neve* az Azure Automationben.**  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Azure Automation verziókövetésből runbookok szinkronizálása
A tárház szinkronizálási panelen a szinkronizálás gomb lehetővé teszi a runbookok a mappájának elérési a tárház húzza az Automation-fiók. Az adott adattár egynél több Automation-fiók is szinkronizálva. A runbookok szinkronizálása a lépései a következők:

1. A verziókövetés beállítása ahol Automation-fiók, nyissa meg a **forrás szinkronizálni a verziókövetést integrációs/adattára panel** kattintson **szinkronizálási** majd megerősítést kéri üzenet, kattintson a **Igen** folytatja.  
   
    ![Szinkronizálás gomb](media/automation-source-control-integration/automation_10_SyncButtonwithMessage.png)
2. Szinkronizálási elindítja a runbookot: **Sync-MicrosoftAzureAutomationAccountFromGitHubV1**. Ez a forgatókönyv GitHub csatlakozik, és kéri le a módosításokat a tárházból az Azure Automation. Új feladat megjelenik a **tárház szinkronizálási** panel ehhez a művelethez. A szinkronizálási feladat adatainak megtekintéséhez kattintson a feladat részletei panel megnyitásához.  
   
    ![Szinkronizálási forgatókönyv](media/automation-source-control-integration/automation_11_SyncRunbook.png)

    > [!NOTE] 
    > A verziókövetésből szinkronizálási felülírja a vázlatként megjelölt verziót, amely jelenleg szerepel az Automation-fiók a futó runbookok **minden** runbookok, amelyek jelenleg a forrás-vezérlőelem. A Git egyenértékű parancssori utasítás szinkronizálása **git lekéréses**


## <a name="troubleshooting-source-control-problems"></a>Forrás vezérlő problémák elhárítása
Ha bármilyen hiba merül be vagy a szinkronizálási feladat, fel kell függeszteni a feladat állapotát, és megtekintheti a hibával kapcsolatos további részleteket a feladat panelen.  A **összes napló** rész azt ismerteti, hogy a feladathoz társított összes PowerShell adatfolyamot. Ezzel a funkcióval a adatokkal segítséget nyújt a problémák elhárítása a bejelentkezés vagy a szinkronizálás szükséges. Azt is megtudhatja, a feladatütemezési műveletek történt a szinkronizálás vagy ellenőrzése – a runbookokban.  

![AllLogs kép](media/automation-source-control-integration/automation_13_AllLogs.png)

## <a name="disconnecting-source-control"></a>Verziókezelő leválasztása
Megszakítja a kapcsolatot a GitHub-fiók, nyissa meg a tárház szinkronizálási panelt, és kattintson a **Disconnect**. Verziókezelő leválasztása után runbookok, amelyek korábban szinkronizált volt az Automation-fiók továbbra is megmarad, de a tárház szinkronizálási panel nem lesz engedélyezve.  

  ![Kapcsolat bontása gomb](media/automation-source-control-integration/automation_12_Disconnect.png)

## <a name="next-steps"></a>Következő lépések
Verziókövetés integrálása kapcsolatos további információkért lásd a következőket:  

* [Azure Automation szolgáltatásbeli: Verziókövetés integrálása az Azure Automationben](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [A kedvenc vezérlő forrásrendszer szavazzon](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Azure Automation szolgáltatásbeli: Integrálása Runbook verziókezelő Visual Studio Team Services használatával](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  

