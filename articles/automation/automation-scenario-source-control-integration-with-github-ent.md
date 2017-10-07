---
title: "Automatizálási verziókövetés integrálása a Githubon vállalati aaaAzure |} Microsoft Docs"
description: "Ismerteti, hogyan hello részleteit az Automation-forgatókönyv verziókövetési tooconfigure-integráció a Githubon vállalati."
services: automation
documentationCenter: 
authors: mgoedtel
manager: jwhit
editor: 
ms.assetid: e01d817c-7d38-421c-adf5-647a4b526eb4
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: magoedte
ms.openlocfilehash: 915d36ccabb72fdee1dba663049a0b331249cd73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure Automation forgatókönyv - automatizálási verziókövetés integrálása a Githubon vállalati

Automatizálási jelenleg a verziókövetés integrálása, amely lehetővé teszi runbookok tooassociate az automatizálási fiók tooa GitHub verziókövetési tárházat támogatja.  Azonban az ügyfelek, akik telepített [GitHub vállalati](https://enterprise.github.com/home) toosupport a DevOps eljárásokat, azt is szeretnék toouse azt toomanage hello életciklusa a runbookok, amelyek fejlett tooautomate üzleti folyamatok és szolgáltatások kezelése műveletek.  

Ebben a forgatókönyvben a Windows rendszerű számítógépeken az Adatközpont konfigurálva, a hibrid forgatókönyv-feldolgozók hello Azure Resource Manager modulok és a Git-eszközök telepítve van.  hello hibrid feldolgozó gépnek hello helyi Git-tárház klónja.  Hello runbook futásakor hello hibrid feldolgozón hello Git directory szinkronizálása és hello runbook fájl tartalmát a rendszer importálta hello Automation-fiók.

Ez a cikk ismerteti, hogyan tooset be ezt a konfigurációt, az Azure Automation-környezetben. Először Automation konfigurálásával hello biztonsági hitelesítő adatokkal, runbookok szükséges toosupport ebben a forgatókönyvben és központi telepítését az adatok a hibrid forgatókönyv-feldolgozó toorun hello runbookok középpontba állításához és hozzáférni a vállalati GitHub-tárház toosynchronize az Automation-fiók runbookok.  


## <a name="getting-hello-scenario"></a>Első hello forgatókönyv

Ebben a forgatókönyvben két PowerShell-forgatókönyvek, amelyek közvetlenül a hello importálhatja áll [forgatókönyvek](automation-runbook-gallery.md) az Azure-portálon hello, vagy letöltheti a hello [PowerShell-galériában](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbookok

Forgatókönyv | Leírás| 
--------|------------|
Export-RunAsCertificateToHybridWorker | Runbook RunAs tanúsítványt, hogy a runbookokat hello munkavégző hitelesíthetők az Azure-ral rendelés tooimport runbookokat hello Automation-fiók be egy automatizálási fiókot tooa hibrid feldolgozó exportálja.| 
Szinkronizálási-LocalGitFolderToAutomationAccount | Runbook szinkronizálások hello hello hibrid gépen helyi Git-mappa, és majd hello runbook fájlok (*.ps1) importálása hello Automation-fiók.|

### <a name="credentials"></a>Hitelesítő adatok

Hitelesítő adat | Leírás|
-----------|------------|
GitHRWCredential | Hitelesítőadat-eszköz hoz létre toocontain hello felhasználónevet és jelszót egy felhasználó engedélyek toohello hibrid feldolgozó.|

## <a name="installing-and-configuring-this-scenario"></a>A forgatókönyv telepítése és konfigurálása

### <a name="prerequisites"></a>Előfeltételek

1. hello Sync-LocalGitFolderToAutomationAccount runbook hitelesíti hello segítségével [Azure-beli futtató fiók](automation-sec-configure-azure-runas-account.md). 

2. Hello Azure Automation-megoldás engedélyezve és konfigurálva van a Microsoft Operations Management Suite (OMS) csomópontjában is szükség.  Ha nem rendelkezik társított hello automatizálási művelet végrehajtására használt fiók tooinstall, és ez a forgatókönyv konfigurálásához, akkor létrehozásáról és az Ön hello végrehajtásakor **New-OnPremiseHybridWorker.ps1** hello hibrid parancsfájlt forgatókönyv-feldolgozó.        

    > [!NOTE]
    > Jelenleg hello következő régiókban csak támogatja az OMS - automatizálás integrációja **Ausztrália délkeleti**, **USA keleti régiója 2**, **Délkelet-Ázsia**, és **nyugati régiója Európa**. 

3. Olyan számítógépre, amelyen a dedikált hibrid forgatókönyv-feldolgozók, amely is hello GitHub szoftver hello runbook fájlok karbantartása szolgálhatnak (*runbook*.ps1) hello fájl rendszer toosynchronize GitHub között a forráskönyvtárat a és a Automation-fiók.

### <a name="import-and-publish-hello-runbooks"></a>Importálja és runbookokat hello közzététele

tooimport hello *Export-RunAsCertificateToHybridWorker* és *Sync-LocalGitFolderToAutomationAccount* runbookokat hello forgatókönyvek az Automation-fiókban lévő hello Azure-portálon a a hello eljárások [hello forgatókönyvek Runbook importálása](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Tegye közzé a runbookokat hello után azok sikeresen importálva lettek az Automation-fiók.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Telepítse és konfigurálja a hibrid forgatókönyv-feldolgozó

Ha nem rendelkezik a hibrid forgatókönyv-feldolgozók már telepítették az adatközpont, kell hello követelményeinek áttekintése és lépések automatikus hello telepítési hello eljárás használatával [Azure Automation hibrid forgatókönyv-feldolgozók - telepítés automatizálása és konfigurációs](automation-hybrid-runbook-worker.md#automated-deployment).  Miután sikeresen telepítette hello hibridfeldolgozó egy számítógépen, hajtsa végre a hello lépéseket toocomplete követően a konfigurációs toosupport ebben a forgatókönyvben.

1. Jelentkezzen be toohello számítógép birtokosi hello hibrid forgatókönyv-feldolgozói szerepkör egy olyan fiókkal, amely helyi rendszergazdai jogosultságokkal rendelkezik, és hozzon létre egy könyvtárat toohold hello Git runbook fájlt.  Klónozás hello belső Git tárház toohello könyvtár.
2. Ha még nem rendelkezik egy futtató fiókot, vagy egy új erre a célra kijelölt toocreate kívánt, hello Azure-portálon a keresse meg a tooAutomation fiókok válassza ki az Automation-fiók, hozzon létre egy [hitelesítőadat-eszköz](automation-credentials.md) , hello felhasználónévvel és jelszóval rendelkező engedélyek toohello hibridfeldolgozó tartalmazza.  
3. Az Automation-fiók a [hello runbook szerkesztése](automation-edit-textual-runbook.md)**Export-RunAsCertificateToHybridWorker** és módosítható hello változó értéke hello *$Password* rendelkező egy erős a jelszó.    Hello érték módosítása után kattintson **közzététel** toohave hello hello runbook vázlatverzióját közzé. 
5. Indítsa el a hello runbook **Export-RunAsCertificateToHybridWorker**, és hello **runbook meghívása** panelen a hello beállítás **futtatási beállítások** hello beállítást **Hibridfeldolgozó** és hello legördülő listában válassza hello hibrid feldolgozócsoport ebben a forgatókönyvben a korábban létrehozott.  

    A tanúsítvány toohello hibrid feldolgozók ez exportálja, így a runbookokat hello munkavégző hitelesítheti az Azure-ral ahhoz toomanage hello futtató kapcsolat használatával Azure-erőforrások (különösen a forgatókönyv - importálási runbookok toohello Automation-fiók).

4. Az Automation-fiók a válassza ki a korábban létrehozott hibrid feldolgozócsoport hello és [adja meg egy futtató fiókot](automation-hrw-run-runbooks.md#runas-account) hello hibrid feldolgozócsoport, és a kiválasztott hitelesítőadat-eszköz hello csak vagy már hozott létre.  Ez biztosítja, hogy hello szinkronizálási runbook futhat a Git-parancsokat. 
5. Hello runbook indítása **Sync-LocalGitFolderToAutomationAccount**, adja meg a hello következő kötelező bemeneti paraméterérték és hello **runbook meghívása** panelen a hello beállítás **futtatása beállítások** hello beállítást **Hibridfeldolgozó** és hello legördülő listában válassza hello hibrid feldolgozócsoport ebben a forgatókönyvben a korábban létrehozott:
    * *Erőforráscsoport* – hello az Automation-fiók társított az erőforráscsoport neve
    * *AutomationAccountName* – hello az Automation-fiók neve
    * *GitPath* – hello helyi mappa vagy fájl a következő hello hibrid forgatókönyv-feldolgozó ahol Git toopull legutóbbi módosítások történő beállítása

    A szinkronizálás hello helyi Git mappa hello hibrid feldolgozó számítógépen, és majd hello .ps1 fájlok importálása hello forrás directory toohello Automation-fiók.

    ![Indítsa el a Sync-LocalGitFolderToAutomationAccount Runbook](media/automation-scenario-source-control-integration-with-github-ent/start-runbook-synclocalgitfoldertoautoacct.png)<br>

7. Feladat összefoglaló információk hello runbook megtekintése hello kijelölésével **Runbookok** panel az Automation-fiók, és jelölje ki hello **feladatok** csempére.  Ellenőrizze, hogy sikeresen befejeződött-e hello kiválasztásával **összes napló** csempe és áttekintette hello részletes naplófolyamot.  

## <a name="next-steps"></a>Következő lépések

-  További információ az runbooktípusokkal, azok előnyeit, és a korlátozásokról tooknow lásd [Azure Automation-runbook típusok](automation-runbook-types.md)
-  További információk a PowerShell-parancsprogramok támogatásáról: [PowerShell-parancsprogramok natív támogatása az Azure Automationben](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
