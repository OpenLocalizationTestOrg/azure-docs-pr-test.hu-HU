---
title: "Verziókövetés integrálása az Azure Automation aaaSource |} Microsoft Docs"
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
ms.openlocfilehash: 6ceee1de8065fafe85a13bbd7f585e74dbc96b47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="source-control-integration-in-azure-automation"></a>Verziókövetés integrálása az Azure Automation szolgáltatásban
Verziókövetés integrálása lehetővé teszi az automatizálási fiók tooa GitHub verziókövetési tárházat tooassociate runbookot. A verziókövetési rendszerrel lehetővé teszi tooeasily együttműködni a csapatával, követni a változásokat, és állítsa vissza a runbookok tooearlier verzióit. Például a verziókövetési rendszerrel lehetővé teszi, hogy Ön toosync ággal a verziókövetési tooyour fejlesztési, tesztelési vagy éles Automation-fiók, így könnyen toopromote kódot, amely a fejlesztési környezet tooyour éles Automation tesztelték fiók.

A verziókövetési rendszerrel lehetővé teszi az Azure Automation toosource vezérlő toopush kódot, vagy a runbookok a forrás vezérlő tooAzure Automation lekéréses. Ez a cikk ismerteti, hogyan szabályozza a tooset be forrás az Azure Automation-környezetben. Rendszer indítsa el a GitHub-tárház Azure Automation tooaccess konfigurálásával és különböző műveleteket végezheti el bízná verziókövetés integrálása használatával. 

> [!NOTE]
> A verziókövetési rendszerrel támogatja húzza és kérdez le [PowerShell munkafolyamat-forgatókönyvekről](automation-runbook-types.md#powershell-workflow-runbooks) , valamint [PowerShell-forgatókönyvek](automation-runbook-types.md#powershell-runbooks). [Grafikus forgatókönyvek](automation-runbook-types.md#graphical-runbooks) még nem támogatottak.<br><br>
> 
> 

Nincsenek két egyszerű lépéseket szükséges tooconfigure verziókövetésének az Automation-fiók, és csak egyet, ha már rendelkezik egy GitHub-fiók. Ezek a következők:

## <a name="step-1--create-a-github-repository"></a>1. lépés – a GitHub-tárház létrehozása
Ha már rendelkezik egy GitHub-fiók és a kívánt helyen toolink tooAzure Automation, majd bejelentkezés tooyour meglévő fiókot, és indítsa el az alábbi 2. lépés. Ellenkező esetben nyissa meg a túl[GitHub](https://github.com/), létrehoz egy új fiókot és [hozzon létre egy új tárház](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>A 2 – az Azure Automationben Verziókövetés beállítása
1. Hello Azure-portálon a hello Automation-fiók paneljén kattintson **Verziókövetés beállítása.** 
   
    ![Verziókövetés beállítása](media/automation-source-control-integration/automation_01_SetUpSourceControl.png)
2. Hello **verziókezelő** panel megnyitása, amelyen konfigurálhatja a GitHub-fiók adatait. Az alábbiakban olvashat paraméterek tooconfigure hello listát:  
   
   | **A paraméter** | **Leírás** |
   |:--- |:--- |
   | Forrás kiválasztása |Válasszon hello forrást. Jelenleg csak **GitHub** esetén támogatott. |
   | Engedélyezés |Kattintson a hello **engedélyezés** gomb toogrant Azure Automation hozzáférés tooyour GitHub-tárházban. Ha már bejelentkezett az tooyour GitHub-fiók egy másik ablakban, majd hello-fiók hitelesítő adatait használja. Engedélyezési végrehajtása sikeres, hello panel fognak megjelenni a GitHub-felhasználónevét alatt **engedélyezési tulajdonság**. |
   | Válassza ki a tárházban |Válassza ki a GitHub-tárházban elérhető adattárak hello listából. |
   | Válassza ki a fiókiroda |Válassza ki egy ágat a hello elérhető ágak. Csak hello **fő** fiókirodai látható, ha még nem hozott létre bármely ágak. |
   | Runbook-mappa elérési útja |hello mappájának elérési hello elérési hello GitHub-tárházban, amelyben szeretné, hogy toopush, illetve a kód lekéréses határozza meg. Az hello formátumban kell megadni **/mappanév/almappanév**. Csak a runbookokat hello mappájának elérési lesz szinkronizált tooyour Automation-fiók. Runbookokat hello mappájának elérési hello almappáiban fog **nem** szinkronizálva. Használjon  **/**  toosync összes alatt hello tárház runbookokat hello. |
3. Például, ha a tárház nevű **PowerShellScripts** , amely tartalmaz egy nevű mappát **RootFolder**, amely tartalmaz egy nevű mappát **almappa**. A következő karakterláncok toosync hello mappa szintenként használhatja:
   
   1. toosync runbookokat **tárház**, runbook-mappa elérési útja*/*
   2. toosync runbookokat **RootFolder**, runbook-mappa elérési út */RootFolder*
   3. toosync runbookokat **almappa**, runbook-mappa elérési út */RootFolder/almappa*.
4. Hello paraméterek beállítása után jelennek meg a hello **Verziókövetés beállítása panelen.**  
   
    ![Konfigurálja a panelen](media/automation-source-control-integration/automation_02_SourceControlConfigure.png)
5. Után kattintson az OK gombra, a verziókövetés integrálása lett konfigurálva az Automation-fiók, és a GitHub-adataihoz frissíteni kell. Most kattintson az a része tooview összes a verziókövetési szinkronizálása feladatelőzményekben.  
   
    ![Tárház értékek](media/automation-source-control-integration/automation_03_RepoValues.png)
6. Miután beállította a verziókövetés, hello következő Automation-erőforrások létrejön az Automation-fiók:  
   Két [változó eszközök](automation-variables.md) jönnek létre.  
   
   * hello változó **Microsoft.Azure.Automation.SourceControl.Connection** hello kapcsolati karakterlánc, hello értékeket tartalmaz, alább látható módon.  
     
     | **A paraméter** | **Érték** |
     |:--- |:--- |
     | Név |Microsoft.Azure.Automation.SourceControl.Connection |
     | Típus |Karakterlánc |
     | Érték |{"Ág":\<*a fióknév*>, "RunbookFolderPath":\<*mappájának elérési*>, "Szolgáltatótípus":\<*az 1 értékű GitHub*>, "Tárház":\<*a tárház nevét*>, "Felhasználónév":\<*a GitHub felhasználói név*>} |

    * hello változó **Microsoft.Azure.Automation.SourceControl.OAuthToken**, hello biztonságos titkosított értékét a OAuthToken tartalmazza.  

    |**A paraméter**            |**Érték** |
    |:---|:---|
    | Név  | Microsoft.Azure.Automation.SourceControl.OAuthToken |
    | Típus | Unknown(Encrypted) |
    | Érték | <*Titkosított OAuthToken*> |  

    ![Változók](media/automation-source-control-integration/automation_04_Variables.png)  

    * **Automatizálási verziókezelő** az engedélyezett alkalmazás tooyour GitHub-fiók meg van adva. tooview hello alkalmazás: a Githubon kezdőlapról lépjen a tooyour **profil** > **beállítások** > **alkalmazások**. Ez az alkalmazás lehetővé teszi, hogy Azure Automation toosync a GitHub-tárház tooan Automation-fiók.  

    ![Git-alkalmazás](media/automation-source-control-integration/automation_05_GitApplication.png)


## <a name="using-source-control-in-automation"></a>A verziókövetési rendszerrel az automatizálás használatával
### <a name="check-in-a-runbook-from-azure-automation-toosource-control"></a>Bejelentkezés az Azure Automation toosource vezérlő runbook
Runbook be lehetővé teszi toopush hello végrehajtott módosítások tooa runbook az Azure Automationben azokat a verziókövetési tárházzal. Lépései a következők hello toocheck a runbook:

1. Az Automation-fiók a [hozzon létre egy új szöveges forgatókönyvként](automation-first-runbook-textual.md), vagy [egy meglévő, szöveges runbook szerkesztése](automation-edit-textual-runbook.md). Ez a forgatókönyv egy PowerShell-munkafolyamat vagy a PowerShell parancsfájl runbook lehet.  
2. A runbook szerkesztése után mentse, és kattintson a **be** a hello **szerkesztése** panelen.  
   
    ![Beadása gomb](media/automation-source-control-integration/automation_06_CheckinButton.png)

     > [!NOTE] 
     > Bejelentkezés Azure Automation felülírja hello kód jelenleg már szerepel a verziókövetési rendszerrel. hello Git egyenértékű parancssori utasítás toocheck a van **git hozzáadása + a git commit + git push**  

1. Amikor rákattint **be**, egy megerősítést kérő üzenet kéri, kattintson az Igen toocontinue.  
   
    ![Beadása üzenet](media/automation-source-control-integration/automation_07_CheckinMessage.png)
2. Bejelentkezés indítása hello forrás vezérlő runbook: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Ez a forgatókönyv tooGitHub csatlakozik, és leküldéses értesítések módosítások Azure Automation tooyour tárházból. tooview hello be feladatelőzmények, lépjen vissza toohello **verziókövetés integrálása** fülre, majd a tooopen hello tárház szinkronizálási panelen. Ezen a panelen láthatók a verziókövetési feladatok.  Jelölje ki a hello feladatot tooview ki, majd kattintson a tooview hello részleteit.  
   
    ![Beadása Runbook](media/automation-source-control-integration/automation_08_CheckinRunbook.png)
   
   > [!NOTE]
   > Forrás ellenőrzési forgatókönyvek olyan speciális tudja megtekinteni vagy szerkeszteni Automation-forgatókönyv. Amíg azok nem jelennek meg a runbook listáján, látni fogja a szinkronizálási feladatokat jelenik meg a feladatok listáján.
   > 
   > 
3. módosított hello runbook hello neve legyen elküldve, egy bemeneti paraméter toohello be runbook. Is [hello feladat részleteinek megtekintéséhez](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) runbook kibontásával **tárház szinkronizálási** panelen.  
   
    ![Beadása bemeneti](media/automation-source-control-integration/automation_09_CheckinInput.png)
4. Frissítse a GitHub-tárház hello feladat tooview hello módosítások után.  Kell egy véglegesítési véglegesítési üzenetet a tárházban: **frissített *Runbook neve* az Azure Automationben.**  

### <a name="sync-runbooks-from-source-control-tooazure-automation"></a>Szinkronizálási forrás vezérlő tooAzure Automation forgatókönyveket
hello tárház szinkronizálási panelen hello szinkronizálás gomb lehetővé teszi a toopull összes hello runbookokat hello runbook mappa elérési útja a tárház tooyour Automation-fiók. hello adott adattár lehet szinkronizált toomore, mint egy Automation-fiók. Az alábbiakban hello lépéseket toosync egy runbookot:

1. Az Automation-fiók Verziókövetés beállítása ahol hello, nyissa meg a hello **forrás szinkronizálni a verziókövetést integrációs/adattára panel** kattintson **szinkronizálási** majd megerősítést kéri üzenet, kattintson a **Igen** toocontinue.  
   
    ![Szinkronizálás gomb](media/automation-source-control-integration/automation_10_SyncButtonwithMessage.png)
2. Szinkronizálási hello runbook indítása: **Sync-MicrosoftAzureAutomationAccountFromGitHubV1**. Ez a forgatókönyv tooGitHub kapcsolódik, és lekéri a tárház tooAzure Automation hello módosításait. Új feladat megjelenik hello **tárház szinkronizálási** panel ehhez a művelethez. hello szinkronizálási feladat, tooview adatait tooopen hello feladat részleteit megjelenítő panelen kattintson.  
   
    ![Szinkronizálási forgatókönyv](media/automation-source-control-integration/automation_11_SyncRunbook.png)

    > [!NOTE] 
    > A verziókövetésből szinkronizálási felülírja, amely jelenleg szerepel az Automation-fiók a runbookokat hello hello vázlatverzióját **összes** runbookok, amelyek jelenleg a forrás-vezérlőelem. hello egyenértékű parancssori utasítás toosync van Git **git lekéréses**


## <a name="troubleshooting-source-control-problems"></a>Forrás vezérlő problémák elhárítása
Ha bármilyen hiba merül be vagy a szinkronizálási feladat, fel kell függeszteni hello feladat állapota, és hello feladat panelen megtekintheti a hello hibával kapcsolatos további részletekért.  Hello **összes napló** rész bemutatja, mindez összes hello feladatot társított PowerShell adatfolyamokat. Ezzel a funkcióval hello adatokkal, a bejelentkezés vagy a szinkronizálási problémák toohelp szükséges. Azt is bemutatja, hello szinkronizálása vagy ellenőrzése – a runbookokban közben végrehajtott műveletek sorozata.  

![AllLogs kép](media/automation-source-control-integration/automation_13_AllLogs.png)

## <a name="disconnecting-source-control"></a>Verziókezelő leválasztása
a GitHub-fiókhoz tartozó toodisconnect hello tárház szinkronizálási panel megnyitásához, majd kattintson **Disconnect**. Verziókezelő leválasztása után a runbookok, amelyek korábban szinkronizált volt az Automation-fiók továbbra is megmarad, de hello tárház szinkronizálási panel nem lesz engedélyezve.  

  ![Kapcsolat bontása gomb](media/automation-source-control-integration/automation_12_Disconnect.png)

## <a name="next-steps"></a>Következő lépések
Verziókövetés integrálása kapcsolatos további információkért tekintse meg a következő erőforrások hello:  

* [Azure Automation szolgáltatásbeli: Verziókövetés integrálása az Azure Automationben](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [A kedvenc vezérlő forrásrendszer szavazzon](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Azure Automation szolgáltatásbeli: Integrálása Runbook verziókezelő Visual Studio Team Services használatával](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  

