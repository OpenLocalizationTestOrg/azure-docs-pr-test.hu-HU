---
title: "Azure Automation aaaIntegrate a Visual Stuido Team Services verziókezelő |} Microsoft Docs"
description: "A forgatókönyv bemutatja, hogyan egy Azure Automation-fiók és a Visual Stuido Team Services a verziókövetési rendszerrel való integráció beállításával."
services: automation
documentationcenter: 
author: eamono
manager: 
editor: 
keywords: "az Azure powershell, a VSTS, verziókezelő, automatizálás"
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.openlocfilehash: 8f6faa596a5ad1f8b72e820ca320b3e103d83579
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Azure Automation forgatókönyv - automatizálási verziókövetés integrálása a Visual Studio Team Services

Ebben a forgatókönyvben használt toomanage Azure Automation-forgatókönyveket vagy a DSC-konfigurációk verziókövetési Visual Studio Team Services projekt rendelkezik.
Ez a cikk ismerteti, hogyan toointegrate VSTS úgy, hogy folyamatos integrációt történik, az egyes be az Azure Automation-környezettel.

## <a name="getting-hello-scenario"></a>Első hello forgatókönyv

Ebben a forgatókönyvben két PowerShell-forgatókönyvek, amelyek közvetlenül a hello importálhatja áll [forgatókönyvek](automation-runbook-gallery.md) az Azure-portálon hello, vagy letöltheti a hello [PowerShell-galériában](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbookok

Forgatókönyv | Leírás| 
--------|------------|
Szinkronizálási-VSTS | Importálhat forgatókönyvek vagy konfigurációk VSTS verziókezelő végzett egy be. Ha manuálisan futtassa azt importálja, és minden runbookok vagy az Automation-fiók hello konfigurációk közzététele.| 
Szinkronizálási-VSTSGit | Importálhat forgatókönyvek vagy konfigurációk VSTS a Git verziókezelő végzett egy be. Ha manuálisan futtassa azt importálja, és minden runbookok vagy az Automation-fiók hello konfigurációk közzététele.|

### <a name="variables"></a>Változók

Változó | Leírás|
-----------|------------|
VSToken | Biztonságos változóeszköz hoz létre, amely tartalmazza a hello VSTS személyes hozzáférési jogkivonat. Megismerheti, hogyan toocreate VSTS személyes hozzáférési jogkivonat a hello [VSTS hitelesítés lap](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview). 
## <a name="installing-and-configuring-this-scenario"></a>A forgatókönyv telepítése és konfigurálása

Hozzon létre egy [személyes hozzáférési jogkivonat](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) a VSTS használt toosync hello forgatókönyvek vagy konfiguráció az automation-fiók be.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Hozzon létre egy [biztonságos változó](automation-variables.md) az automation-fiók toohold hello a személyes hozzáférési jogkivonatot, hogy hello runbook hitelesíthető tooVSTS és szinkronizálási hello forgatókönyvek vagy a beállításokat a rendszerbe hello Automation-fiók. A VSToken nevére. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

A runbookok vagy konfigurációk hello automation-fiók a szinkronizálandó hello forgatókönyv importálása. Használhatja a hello [VSTS minta runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) vagy hello [VSTS a Git minta runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) hello PowerShellGallery.com attól függően, hogy a VSTS használata forrás-vezérlő vagy a Git VSTS, és telepítheti a tooyour automation-fiók.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Most [közzététele](automation-creating-importing-runbook.md#publishing-a-runbook) ezt a runbookot, hogy létrehozhasson egy webhook. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Hozzon létre egy [webhook](automation-webhooks.md) a Sync-VSTS runbookhoz és adja meg a hello paraméterek alább látható módon. Győződjön meg arról, szüksége lesz, a szolgáltatás hook a VSTS másolása hello webhook URL-címét. hello VSAccessTokenVariableName esetén hello (VSToken) hello biztonságos változó létrehozott korábbi toohold hello személyes hozzáférési jogkivonat. 

VSTS (szinkronizálási-VSTS.ps1) integrálása a következő paraméterek hello igénybe vehet.
### <a name="sync-vsts-parameters"></a>Szinkronizálási-VSTS-paraméterek

Paraméter | Leírás| 
--------|------------|
WebhookData | Hello VSTS szolgáltatás hook küldött hello beadása információt tartalmaz. Ez a paraméter kell hagyja üresen.| 
ResourceGroup | Ez az, hogy hello automation-fiók megtalálható hello erőforráscsoport hello nevét.|
AutomationAccountName | hello VSTS, szinkronizálandó hello automation-fiók neve.|
VSFolder | Ha létezik a runbookokat hello és konfigurációk VSTS hello mappájában neve.|
VSAccount | hello hello Visual Studio Team Services-fiók neve.| 
VSAccessTokenVariableName | hello hello biztonságos változó neve (VSToken), amely tárolja a hello VSTS személyes hozzáférési jogkivonat.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

VSTS git (szinkronizálási-VSTSGit.ps1) használatakor a következő paraméterek hello fog tartani.

Paraméter | Leírás|
--------|------------|
WebhookData | Hello VSTS szolgáltatás hook küldött hello beadása információt tartalmaz. Ez a paraméter kell hagyja üresen.| ResourceGroup | A hello név, automation-fiók hello hello erőforráscsoport van.|
AutomationAccountName | hello VSTS, szinkronizálandó hello automation-fiók neve.|
VSAccount | hello hello Visual Studio Team Services-fiók neve.|
VSProject | Ha létezik a runbookokat hello és konfigurációk VSTS hello projekt hello nevét.|
GitRepo | hello Git-tárház hello neve.|
GitBranch | hello ág VSTS Git-tárházban hello nevét.|
Mappa | hello mappa VSTS Git ágában hello neve.|
VSAccessTokenVariableName | hello hello biztonságos változó neve (VSToken), amely tárolja a hello VSTS személyes hozzáférési jogkivonat.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Hozzon létre egy service hook VSTS jelölőnégyzet-modulok toohello mappa, amely elindítja a kód be a webhook. Jelöljön ki webes csatlakozik, hello szolgáltatás toointegrate az új előfizetés létrehozásakor. További service hurkok kapcsolatos a [VSTS szolgáltatás hurkok dokumentáció](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Meg kell tudni toodo összes jelölőnégyzet-modulok a runbookok és a beállításokat a VSTS rendszerbe, és ezek automatikusan be vannak szinkronizálni az automation-fiók be kellett.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Ha ez a forgatókönyv manuálisan nélkül futtatja VSTS által indított alatt, üresen hello webhookdata paraméter, és a teljes szinkronizálás megadott hello VSTS mappából fog tenni.

Ha a toouninstall hello forgatókönyv, hello szolgáltatás hook eltávolítása VSTS, hello runbook és hello VSToken változó törlése.
