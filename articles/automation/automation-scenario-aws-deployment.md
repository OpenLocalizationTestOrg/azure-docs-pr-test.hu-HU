---
title: "a virtuális gépek az Amazon Web Services aaaAutomating telepítési |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toouse az Amazon Web Service virtuális gép Azure Automation tooautomate létrehozása"
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 1d85c01a-d795-4523-8194-84fc15b53838
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/14/2017
ms.author: tiandert; bwren
ms.openlocfilehash: dd1f3af47d8700ced85a3ee5a406dde1c1311990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Automation forgatókönyv - kiépítés az AWS virtuális gép
Ebben a témakörben bemutatjuk, hogyan használhatják az Amazon Web Service (AWS) előfizetésében egy virtuális gép Azure Automation tooprovision fel, és nevezze el ezt a virtuális Gépet egy adott – mely AWS "címkézés" hello VM tooas hivatkozik.

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk hello célokra toohave egy Azure Automation-fiók és szükséges az AWS előfizetéssel. További információ az Azure Automation-fiók beállításával, és konfigurálni kell az AWS-előfizetés hitelesítő adatait, tekintse át a [hitelesítés beállítása az Amazon Web Services](automation-configure-aws-account.md).  Ezt a fiókot kell hozható létre vagy nem frissült az AWS előfizetés hitelesítő adatait, mielőtt továbblépne, azt fogja hivatkozási hello lépéseket ehhez a fiókhoz.

## <a name="deploy-amazon-web-services-powershell-module"></a>Amazon Web Services PowerShell-modul telepítése
A Virtuálisgép-létrehozásnál runbook hello AWS PowerShell modul toodo fogja használni a tevékenységeket. Hajtsa végre a következő lépéseket tooadd hello modul tooyour Automation-fiók, amelynek része az AWS-előfizetés hitelesítő adatait hello.  

1. Nyissa meg a webböngészőt, és keresse meg a toohello [PowerShell-galériában](http://www.powershellgallery.com/packages/AWSPowerShell/) , majd kattintson a hello **telepítés tooAzure Automation gomb**.<br><br> ![AWS PS modul importálása](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. A következő lépés az toohello Azure bejelentkezési lapra és a hitelesítés után lesz irányított toohello Azure portálon, és jelenik meg a következő panel hello.<br><br> ![Importálás modul panel](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Jelölje be hello erőforráscsoport a hello **erőforráscsoport** legördülő listában, és a hello (paraméterek) panelen adja meg a következő információ hello:
   
   * A hello **új vagy meglévő Automation-fiók (karakterlánc)** legördülő listában válassza ki **meglévő**.  
   * A hello **Automation-fiók neve (karakterlánc)** írja be a hello pontos név a hello Automation-fiók, amely tartalmazza az AWS előfizetés hello hitelesítő adatait.  Például, ha létrehozott egy külön fiókot nevű **AWSAutomation**, akkor az hello mezőbe írja.
   * Válassza ki a hello megfelelő régió hello **Automation-fiók helyével** legördülő listából.
4. Hello szükséges adatok megadásával befejeződésekor kattintson **létrehozása**.
   
   > [!NOTE]
   > A PowerShell modul importálása az Azure Automation, akkor van is kibontása hello parancsmagok, és ezek a tevékenységek nem jelennek meg, amíg hello modul teljesen importálása és hello parancsmagok kibontása befejeződött. A folyamat eltarthat néhány percig.  
   > <br>
   > 
   > 
5. Hello Azure portál nyissa meg a 3. lépésben hivatkozott az Automation-fiók.
6. Kattintson a hello **eszközök** csempére a hello **eszközök** panelen, jelölje be hello **modulok** csempére.
7. A hello **modulok** panelen láthatja, hogy hello **AWSPowerShell** modul hello listában.

## <a name="create-aws-deploy-vm-runbook"></a>Hozzon létre AWS VM runbook központi telepítése
Egyszer hello AWS PowerShell modul telepítve van, hogy most létrehozható egy runbook tooautomate AWS egy PowerShell-parancsfájl használatával a virtuális gépek kiépítése. hello lépéseket mutatni, hogyan tooleverage natív PowerShell-parancsfájlt az Azure Automationben.  

> [!NOTE]
> További beállítások és a parancsfájl vonatkozó információkat, látogasson el a hello [PowerShell-galériában](https://www.powershellgallery.com/packages/New-AwsVM/DisplayScript).
> 

1. Nyisson meg egy PowerShell-munkamenetet, és hello következő PowerShell-galériában hello letöltése hello PowerShell-parancsfájlt új-AwsVM:<br>
   ```
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Hello Azure portál, nyissa meg az Automation-fiók, és kattintson hello **Runbookok** csempére.  
3. A hello **Runbookok** panelen válassza **hozzáadása egy runbook**.
4. A hello **hozzáadása egy runbook** panelen válassza **Gyorslétrehozás** (hozzon létre egy új runbookot).
5. A hello **Runbook** tulajdonságok panelére léphet, írja be a runbook és hello hello név mezőben nevét **runbooktípusba** legördülő listában válassza ki **PowerShell**, és kattintson a  **Hozzon létre**.<br><br> ![Importálás modul panel](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. Hello PowerShell Runbook szerkesztése panelen megjelenő másolja be hello PowerShell-parancsfájl a szerzői a vásznon a hello runbook.<br><br> ![Runbook PowerShell-parancsfájl](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Hello példa PowerShell-parancsfájlt az használatakor vegye figyelembe a következő hello:
    > 
    > * hello runbook tartalmaz egy alapértelmezett paraméterértékek száma. Az összes alapértelmezett értékeit értékelheti ki, és szükség esetén frissítse.
    > * Ha a tárolt AWS hitelesítő adatait, mint más néven hitelesítőadat-eszköz **AWScred**, ennek megfelelően kell sor 57 toomatch tooupdate hello parancsfájlt.  
    > * Amikor olyan hello AWS CLI-parancsokat a PowerShell, különösen akkor ez a példa runbook hello AWS régióban kell megadnia. Ellenkező esetben hello parancsmagok sikertelen lesz.  Nézet AWS témakör [meg AWS régió](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) hello AWS eszközök PowerShell dokumentum további részleteket a.  
    >

7. tooretrieve az AWS előfizetésből kép neveinek listáját indítsa el a PowerShell ISE és hello AWS PowerShell modul importálásához.  Azáltal, hogy az AWS hitelesítése **Get-AutomationPSCredential** a ISE környezetében lévő **AWScred = Get-Credential**.  Ez kérni fogja a hitelesítő adatokat, és megadhatja a **hozzáférési kulcs azonosító** hello felhasználónévhez és **titkos hívóbetű** hello jelszót.  Tekintse meg az alábbi példa hello:  

        #Sample tooget hello AWS VM available images
        #Please provide hello path where you have downloaded hello AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up hello environment tooaccess AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile

    hello következő eredményt ad vissza:<br><br>
   ![AWS képek beolvasása](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Másolja és illessze be az egyik hello képek nevének hello egy automatizálási változó hello runbook, amint a **$InstanceType**. Ebben a példában használjuk hello szabad AWS rétegzett előfizetés, mert fogjuk használni **t2.micro** runbook példa.  
9. Hello runbook mentse, majd kattintson a **közzététel** toopublish hello runbookot, majd **Igen** megjelenésekor.

### <a name="testing-hello-aws-vm-runbook"></a>Hello AWS VM runbook tesztelése
A Microsoft hello tesztelési runbook folytatásához, igazolnia kell tooverify folyamatban van. Konkrétan:  

* Az eszköz az AWS hitelesítést hívott létrejött **AWScred** vagy hello parancsfájl volt a hitelesítőadat-eszköz frissített tooreference hello nevét.    
* hello AWS PowerShell modul importálva van az Azure Automationben  
* Létrejött egy új runbookot, és a paraméterértékek ellenőrzése és frissítése, amennyiben szükséges  
* **Részletes rekordok naplózására** , illetve opcionálisan **naplózza az állapotrekordokat** hello runbook beállítás alatt **naplózás és nyomkövetés** túl be**a**.<br><br> ![Runbook-naplózás és nyomkövetés](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. Azt szeretné, hogy toostart hello runbook, így kattintson **Start** majd **OK** hello runbook meghívása panel megnyitásakor.
2. A runbook meghívása hello panelen adjon meg egy **VMname**.  Fogadja el a hello alapértelmezett értékeinek hello más paramétereket, amelyeket korábban előre hello parancsfájlban.  Kattintson a **OK** toostart hello runbook-feladat.<br><br> ![Új-AwsVM runbook elindítása](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. A feladat panelen, amely az imént létrehozott hello runbook-feladat van megnyitva. Zárja be az ezen az ablaktáblán.
4. Azt megtekintheti hello feladatot és tekintse meg a kimeneti előrehaladását **adatfolyamok** hello kiválasztásával **összes napló** csempe hello runbook feladat panelen.<br><br> ![Adatfolyam-kimenetét](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. tooconfirm hello VM telepítése folyamatban van, jelentkezzen be a hello AWS felügyeleti konzolon, ha jelenleg nem jelentkezik.<br><br> ![AWS konzol telepített virtuális gép](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Következő lépések
* Grafikus forgatókönyvek használatába tooget lásd [saját első grafikus forgatókönyv](automation-first-runbook-graphical.md)
* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md)
* További információ az runbooktípusokkal, azok előnyeit, és a korlátozásokról tooknow lásd [Azure Automation-runbook típusok](automation-runbook-types.md)
* További információk a PowerShell-parancsprogramok támogatásáról: [PowerShell-parancsprogramok natív támogatása az Azure Automationben](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

