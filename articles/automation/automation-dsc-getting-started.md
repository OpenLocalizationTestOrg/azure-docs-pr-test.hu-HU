---
title: "Azure Automation DSC használatába aaaGetting |} Microsoft Docs"
description: "MAGYARÁZAT és példákat a hello leggyakoribb feladatokat az Azure Automation szükséges konfiguráló (DSC)"
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: a3816593-70a3-403b-9a43-d5555fd2cee2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/21/2016
ms.author: magoedte;eslesar
ms.openlocfilehash: 82910c96e928b9264c2e1b52a5c98dc47273dcc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a>Ismerkedés az Azure Automation DSC
Ez a témakör azt ismerteti, hogyan toodo hello leggyakoribb feladatokat az Azure Automation szükséges konfiguráló (DSC), például a létrehozása, importálása, és konfigurációk, gépek túl kezelése, bevezetési fordítása és jelentések megtekintése. Milyen Azure Automation DSC áttekintést van, a következő témakörben: [Azure Automation DSC – áttekintés](automation-dsc-overview.md). A DSC-dokumentáció, lásd: [Windows PowerShell kívánt állapot beállítása – áttekintés](https://msdn.microsoft.com/PowerShell/dsc/overview).

Ez a témakör egy részletes útmutató toousing Azure Automation DSC Szolgáltatásban. Ha azt szeretné, hogy egy minta-környezet, amely már be van állítva a jelen témakörben ismertetett hello lépések nélkül, [ARM-sablon következő hello](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup). Ez a sablon állít be egy befejezett Azure Automation DSC-környezetben, például egy Azure Automation DSC által felügyelt Azure virtuális Gépen.

## <a name="prerequisites"></a>Előfeltételek
Ebben a témakörben toocomplete hello példákért hello következők szükségesek:

* Egy Azure Automation-fiókra. Azure Automation futtató fiók létrehozásával kapcsolatos információkért tekintse meg az [Azure-beli futtató fiókkal](automation-sec-configure-azure-runas-account.md) kapcsolatos részt.
* Az Azure Resource Manager virtuális gép (klasszikus) Windows Server 2008 R2 rendszerű vagy újabb. Virtuális gép létrehozása, lásd: [hello Azure-portálon az első Windows rendszerű virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="creating-a-dsc-configuration"></a>A DSC-konfiguráció létrehozása
Létre fogunk hozni egy egyszerű [DSC-konfiguráció](https://msdn.microsoft.com/powershell/dsc/configurations) hello jelenléte vagy hiánya hello, ezzel biztosítható **webkiszolgáló** Windows szolgáltatás (IIS), attól függően, hogy hogyan lehet kijelölni csomópontok.

1. Indítsa el a Windows PowerShell ISE hello (vagy bármilyen szövegszerkesztővel).
2. Írja be a következő szöveg hello:
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
   
            }
        }
   
        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
   
            }
        }
        }
    ```
3. Hello fájl mentése másként `TestConfig.ps1`.

Ez a konfiguráció meghívja egy erőforrást minden egyes csomópont blokkban hello [WindowsFeature erőforrás](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), hello jelenléte vagy hiánya hello, ezzel biztosítható **webkiszolgáló** szolgáltatás.

## <a name="importing-a-configuration-into-azure-automation"></a>Azure Automation konfiguráció importálása
A következő hello konfigurációs, azt fogja importálni hello Automation-fiók.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**.
4. A hello **a DSC-konfigurációk** panelen kattintson a **hozzáadása egy konfigurációs**.
5. A hello **konfiguráció importálása** panelen, a Tallózás toohello `TestConfig.ps1` fájlt a számítógépen.
   
    ![Képernyőfelvétel a hello ** importálási konfigurációs ** panel](./media/automation-dsc-getting-started/AddConfig.png)
6. Kattintson az **OK** gombra.

## <a name="viewing-a-configuration-in-azure-automation"></a>A beállítások megjelenítése az Azure Automationben
Miután importálta a konfiguráció, megtekintheti a hello Azure-portálon.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**
4. A hello **a DSC-konfigurációk** panelen kattintson a **TestConfig** (Ez az importált hello konfigurációs hello neve hello előző eljárásban).
5. A hello **TestConfig konfigurációs** panelen kattintson a **konfigurációs forrás megtekintése**.
   
    ![Hello TestConfig konfigurációs paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    A **TestConfig konfigurációs forrás** panel nyílik hello PowerShell-kódjába hello konfigurációs megjelenítése.

## <a name="compiling-a-configuration-in-azure-automation"></a>Az Azure Automationben konfiguráció fordítása
A kívánt állapot tooa csomópont alkalmazása előtt a DSC-konfiguráció meghatározása az állapotban kell egy vagy több csomópont-konfigurációt (MOF dokumentum) lefordítva, és hello Automation DSC lekéréses kiszolgáló helyezve. Azure Automation DSC-konfigurációja összeállításának részletes ismertetését lásd: [Azure Automation DSC-konfigurációja fordítása](automation-dsc-compile.md). Konfiguráció fordítása kapcsolatos további információkért lásd: [a DSC-konfigurációk](https://msdn.microsoft.com/PowerShell/DSC/configurations).

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**
4. A hello **a DSC-konfigurációk** panelen kattintson a **TestConfig** (korábban importált konfigurációs hello hello neve).
5. A hello **TestConfig konfigurációs** panelen kattintson a **fordítási**, és kattintson a **Igen**. Egy fordítási feladat elindul.
   
    ![Fordítási gomb kiemelés hello TestConfig konfigurációs paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Ha egy Azure Automation konfigurációs lefordításához bármely létrehozott csomópont konfigurációs MOF-ot toohello lekérési kiszolgálójával automatikusan telepíti.
> 
> 

## <a name="viewing-a-compilation-job"></a>Egy fordítási feladat megtekintése
A fordítás megkezdése után megtekintheti az hello **fordítási feladatok** hello csempére **konfigurációs** panelen. Hello **fordítási feladatok** csempe megjeleníti a jelenleg futó, befejeződött, és sikertelen feladatok. Amikor megnyit egy fordítási feladat panelen, azt mutatja, beleértve az esetleges hibákat vagy figyelmeztetéseket észlelt feladatot információt, hello konfigurációja és fordítási használt bemeneti paraméterek naplókat.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **a DSC-konfigurációk**.
4. A hello **a DSC-konfigurációk** panelen kattintson a **TestConfig** (korábban importált konfigurációs hello hello neve).
5. A hello **fordítási feladatok** hello csempéjéről **TestConfig konfigurációs** panelen kattintson a felsorolt hello feladatok bármelyikét. A **fordítási feladat** panel nyílik fordítási feladat hello hello dátummal feliratú lett elindítva.
   
    ![Hello fordítási feladat paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/CompilationJob.png)
6. Kattintson az összes csempe hello az **fordítási feladat** panel toosee további hello feladat vonatkozó részletek.

## <a name="viewing-node-configurations"></a>Csomópont-konfigurációt megtekintése
Egy fordítási feladat sikeres létrehozása után egy vagy több új csomópont-konfigurációkat hoz létre. A csomópont-konfiguráció, amely telepített toohello lekérési kiszolgálójával, és készen áll a toobe lekért és egy vagy több csomópont által alkalmazott MOF dokumentum. Az Automation-fiókban lévő hello meg tudja tekinteni hello csomópont-konfigurációt **DSC-Csomópontkonfigurációk** panelen. A csomópont-konfiguráció van egy hello űrlap nevű *ConfigurationName*. *Csomópontnév*.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **DSC-Csomópontkonfigurációk**.
   
    ![Hello DSC-Csomópontkonfigurációk paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a>Egy Azure virtuális gép Management az Azure Automation DSC Szolgáltatásban
Azure Automation DSC toomanage Azure virtuális gépeken (klasszikus és Resource Manager), a helyszíni virtuális gépek, Linux rendszerű gépek, AWS virtuális gépek és a helyszíni fizikai gépeket is használhatja. Ebben a témakörben azt fedezi hogyan tooonboard csak a Azure Resource Manager virtuális gépeken. Gépek, más típusú bevezetési kapcsolatos információk: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md).

### <a name="tooonboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a>az Azure Resource Manager virtuális gép Azure Automation DSC általi kezelésre tooonboard
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.
4. A hello **DSC-csomópontok** panelen kattintson a **adja hozzá az Azure virtuális gép**.
   
    ![Hello Azure VM hozzáadása gomb kiemelés hello DSC-csomópontok paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/OnboardVM.png)
5. A hello **adja hozzá az Azure virtuális gépek** panelen kattintson a **válassza ki a virtuális gépek tooonboard**.
6. A hello **válassza ki a virtuális gépek** panelen, jelölje be hello tooonboard szeretne, és kattintson a virtuális gép **OK**.
   
   > [!IMPORTANT]
   > Az értéknek kell lennie az Azure Resource Manager virtuális gép Windows Server 2008 R2 rendszerű vagy újabb.
   > 
   > 
7. A hello **adja hozzá az Azure virtuális gépek** panelen kattintson a **konfigurálása a regisztrációs adatok**.
8. A hello **regisztrációs** panelen hello beírása hello csomópont-konfiguráció tooapply toohello VM hello a kívánt **csomópont-konfiguráció neve** mezőbe. A csomópont-konfiguráció a hello Automation-fiók neve hello egyeznie kell. Ezen a ponton biztosító nevét nem kötelező megadni. Csomópont-konfiguráció hozzárendelése hello bevezetési hello csomópont után módosíthatja.
   Ellenőrizze **szükség esetén újraindítás csomópont**, és kattintson a **OK**.
   
    ![Hello regisztrációs paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/RegisterVM.png)
   
    a megadott csomópont-konfiguráció hello lesz alkalmazott toohello VM hello által meghatározott időközönként **konfigurációs mód gyakoriságának**, és a frissítések toohello csomópont-konfiguráció hello általmeghatározottidőközönkéntellenőrzihelloVM **Frissítési gyakoriság**. Ezek az értékek módjáról további információkért lásd: [konfigurálása hello helyi Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
9. A hello **adja hozzá az Azure virtuális gépek** panelen kattintson a **létrehozása**.

Azure VM bevezetési hello hello folyamata elindul. Amikor elkészült, hello VM fog megjelenni hello **DSC-csomópontok** hello Automation-fiók panelén.

## <a name="viewing-hello-list-of-dsc-nodes"></a>A DSC-csomópontok hello listájának megtekintése
Megtekintheti az összes gép lett előkészítve az Automation-fiókban lévő hello Management hello listája **DSC-csomópontok** panelen.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.

## <a name="viewing-reports-for-dsc-nodes"></a>A DSC-csomópontok számára jelentések megtekintése
Minden alkalommal, amikor az Azure Automation DSC a felügyelt csomóponton konzisztencia-ellenőrzést végez hello csomópont állapota hátsó toohello lekéréses jelentéskiszolgáló küld. Az adott csomópont hello panelen megtekintheti ezekre a jelentésekre.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.
4. A hello **jelentések** csempére, az egyes hello jelentések hello listában.
   
    ![Hello jelentés paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/NodeReport.png)

Egy egyedi jelentés hello paneljén láthatja, ellenőrizze a következő állapotadatokat hello megfelelő konzisztencia hello:

* az állapot jelentése hello – e hello csomópont "Megfelelő", "Sikertelen" hello konfigurációs vagy hello csomópont "Nem megfelelő" (ha hello csomópontjának nem **applyandmonitor** hello és a gép nem szükséges hello állapotban van.).
* hello kezdési idejét hello konzisztencia-ellenőrzést.
* hello teljes futási hello konzisztencia-ellenőrzése.
* hello típusú konzisztencia-ellenőrzése.
* Hibáit, beleértve a hiba kódja és hiba üdvözlőüzenetére. 
* Bármely hello konfigurációja és hello állapotát (hogy hello csomópont hello szükséges állapotban van az adott erőforrás) az egyes erőforrások használt DSC-erőforrások – kattinthat a minden egyes erőforrás tooget részletes információkat az adott erőforráshoz.
* hello nevét, IP-cím és hello csomópont konfigurációs módja.

Is **nyers jelentés megtekintéséhez** toosee hello tényleges adatokat csomópont hello toohello server küld. Adatok használatával kapcsolatos további információkért lásd: [a DSC-jelentés kiszolgálóval](https://msdn.microsoft.com/powershell/dsc/reportserver).

Miután csomópont előkészítve, mielőtt hello első jelentés érhető el némi időbe telhet. Szükség lehet másolatot too30 perc toowait hello első jelentés követően bevezetésében egy csomópont.

## <a name="reassigning-a-node-tooa-different-node-configuration"></a>Egy csomópont tooa másik csomópont-konfiguráció újbóli hozzárendelése
Hozzárendelhet egy csomópont toouse egy másik csomópont-konfiguráció mint hello kezdetben hozzárendelt.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.
4. A hello **DSC-csomópontok** panelen kattintson a hello neve hello csomópont tooreassign szeretné.
5. Az adott csomópont hello paneljén kattintson **hozzárendelése csomópont**.
   
    ![Hello hozzárendelése csomópont gomb kiemelés hello csomópont paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/AssignNode.png)
6. A hello **csomópont-konfiguráció hozzárendelése** panelen, jelölje be hello csomópont konfigurációs toowhich szeretné tooassign hello csomópont, és kattintson a **OK**.
   
    ![Hello csomópont-konfiguráció hozzárendelése paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Egy csomópont
Ha már nincs szüksége egy Azure Automation DSC által kezelt csomópont toobe, regisztrációjának megszüntetésére.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben kattintson a **összes erőforrás** és majd hello az Automation-fiók nevét.
3. A hello **Automation-fiók** panelen kattintson a **DSC-csomópontok**.
4. A hello **DSC-csomópontok** panelen kattintson a hello neve hello csomópont toounregister szeretné.
5. Az adott csomópont hello paneljén kattintson **Unregister**.
   
    ![Hello Unregister gomb kiemelés hello csomópont paneljét bemutató képernyőkép](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Azure Automation DSC – áttekintés](automation-dsc-overview.md)
* [Azure Automation DSC általi kezelésre bevezetési gépek](automation-dsc-onboarding.md)
* [A Windows PowerShell célállapot-konfiguráló áttekintése](https://msdn.microsoft.com/powershell/dsc/overview)
* [Azure Automation DSC-parancsmagokkal](/powershell/module/azurerm.automation/#automation)
* [Azure Automation DSC – díjszabás](https://azure.microsoft.com/pricing/details/automation/)

