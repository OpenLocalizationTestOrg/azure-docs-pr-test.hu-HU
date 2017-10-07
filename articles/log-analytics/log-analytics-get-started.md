---
title: "az Azure Log Analytics-munkaterülethez használatába aaaGet |} Microsoft Docs"
description: "A Log Analyticsban a munkaterület percek alatt üzembe helyezhető."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 508716de-72d3-4c06-9218-1ede631f23a6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/08/2017
ms.author: magoedte
ms.openlocfilehash: 442a9258a37ee79e8f0b45759ef24b5e3dae0130
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-a-log-analytics-workspace"></a>Ismerkedés a Log Analytics-munkaterülettel
Az Azure Log Analytics percek alatt üzembe helyezhető, és segít kiértékelni az informatikai infrastruktúrából gyűjtött operatív adatelemzési információkat. Ennek a cikknek a segítségével könnyedén nekiláthat az *ingyenesen* gyűjtött adatok felfedezésének és elemzésének, valamint reagálhat azokra.

Ez a cikk szolgál egy bevezető tooLog Analytics egy rövid útmutató toowalk használatának meg a minimális központi az Azure-ban, hogy elindíthatja hello szolgáltatás használatával. hello logikai tárolónak tárolja a felügyeleti adatokat az Azure-ban a munkaterület neve. Miután befejezte a saját értékelési és olvasta ezt az információt, eltávolíthatja a hello értékelési munkaterületen. Lévén ez a cikk egy oktatóanyag, az üzleti követelményekkel, a tervezéssel és az architektúrával kapcsolatos iránymutatásokkal nem foglalkozik.

>[!NOTE]
>Ha a Microsoft Azure Government felhő hello használja, a [Azure Government figyelési + dokumentáció](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#log-analytics) helyette.

Itt található lépések hello használt folyamat tooget gyorsan át:

![folyamatábra](./media/log-analytics-get-started/onboard-oms.png)

## <a name="1-create-an-azure-account-and-sign-in"></a>1 Azure-fiók létrehozása és bejelentkezés

Ha még nem rendelkezik Azure-fiókra, toocreate egy toouse Naplóelemzési kell. Létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/), amely 30 napon keresztül működik, és ezalatt bármely Azure-szolgáltatáshoz hozzáférést biztosít.

### <a name="toocreate-a-free-account-and-sign-in"></a>toocreate egy ingyenes fiókot, és jelentkezzen be
1. Kövesse a hello utasításokat [az ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).
2. Nyissa meg toohello [Azure-portálon](https://portal.azure.com) , jelentkezzen be.

## <a name="2-create-a-workspace"></a>2 Munkaterület létrehozása

hello következő lépés egy munkaterület toocreate.

1. Hello Azure-portálon, a Keresés a piactér hello szolgáltatás hello listája a *Naplóelemzési*, majd válassza ki **Naplóelemzési**.  
    ![Azure Portal](./media/log-analytics-get-started/log-analytics-portal.png)
2. Kattintson a **létrehozása**, majd válassza ki a következő elemek hello lehetőségeit:
   * **OMS-munkaterület** – Adja meg a munkaterület nevét.
   * **Előfizetés** – Ha több előfizetéssel, válassza ki a kívánt hello új munkaterület tooassociate hello rendelkezik.
   * **Erőforráscsoport**
   * **Hely**
   * **Tarifacsomag**  
       ![gyors létrehozás](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Kattintson a **OK** toosee a munkaterületek listáját.
4. Válassza ki a munkaterület toosee a részletek hello Azure-portálon.       
    ![munkaterület részletei](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         

## <a name="3-upgrade-workspace-toonew-log-search"></a>3 frissítési munkaterület toonew napló keresése
Új Naplóelemzési lekérdezésnyelvet kiadása és rendelés tootake előnye az, hogy kell tooconvert a munkaterület.  Ha a munkaterület üzemeltetett hello régió frissítve lett, meg kell jelennie egy lila szalagcím hello tetején a munkaterület felkérte a tooconvert. hello frissítése teljes mértékben önkéntes, és nem befolyásolja a felhasználói élmény a Naplóelemzési és bármely megoldások ad hozzá.  

További információk toounderstand hello juttatásra, a szempontok és a folyamat tooupgrade, lásd: [frissítése Azure Naplóelemzés toonew napló keresési](log-analytics-log-search-upgrade.md).  

## <a name="4-add-solutions-and-solution-offerings"></a>4 Megoldások és megoldásajánlatok hozzáadása

A következő lépésben adjon hozzá megoldásokat és megoldásajánlatokat. A felügyeleti megoldások egy adott problématerületet érintő mérőszámokat szolgáltató logikai, adatábrázolási és adatgyűjtési szabályok gyűjteményei. A megoldásajánlat alatt csomagba foglalt felügyeleti megoldásokat értünk.

Hozzáadása megoldások tooyour munkaterület lehetővé teszi Naplóelemzési toocollect különböző típusú adatok tartozó ügynökök használatával csatlakoztatott tooyour munkaterület számítógépekről. A felvételi ügynökökről a későbbiekben lesz szó.

### <a name="tooadd-solutions-and-solution-offerings"></a>tooadd megoldások és a megoldás ajánlatok

1. Az Azure portálon kattintson **új** , majd a hello **hello a piactéren** mezőbe írja be **tevékenység Naplóelemzési** , és nyomja le az ENTER BILLENTYŰT.
2. A hello mindent panelen válassza **tevékenység Naplóelemzési** majd **létrehozása**.  
    ![Activity Log Analytics](./media/log-analytics-get-started/activity-log-analytics.png)  
3. A hello *felügyeleti megoldás neve* panelen válassza ki a megfelelő munkaterület, amelyet az tooassociate hello felügyeleti megoldással.
4. Kattintson a **Create** (Létrehozás) gombra.  
    ![megoldás munkaterület](./media/log-analytics-get-started/solution-workspace.png)  
5. Ismételje meg a lépéseket: 1-4 tooadd:
    - Hello **biztonsági és megfelelőségi** szolgáltatásajánlat hello Assessment kártevőirtó és biztonsági és naplózási megoldásokkal.
    - Hello **Automation & vezérlő** szolgáltatásajánlat hello Automation Hibridfeldolgozó, a változások követése és a rendszer frissítések értékelését (más néven frissítéskezelés) megoldásokat. Hello megoldás ajánlat hozzáadásakor, létre kell hoznia az Automation-fiók.  
        ![Automation-fiók](./media/log-analytics-get-started/automation-account.png)  
6. Megtekintheti a hello megoldások tooyour munkaterület túl navigálva hozzáadott**Naplóelemzési** > **előfizetések** > ***munkaterületneve***  >  **Áttekintése**. Hello felügyeleti megoldásokra hozzáadott csempék jelennek meg.  
    >[!NOTE]
    >Jelenleg még nem csatlakozott bármely ügynökök toohello munkaterületen, mert nem látható a hozzáadott hello megoldások adatokat.  

    ![megoldások csempéi adatok nélkül](./media/log-analytics-get-started/solutions-no-data.png)

## <a name="4-create-a-vm-and-onboard-an-agent"></a>4 Virtuális gép létrehozása és ügynök felvétele

A következő lépésben hozzon létre egy egyszerű virtuális gépet az Azure-ban. Miután létrehozta a virtuális gépek előkészítésére hello OMS-ügynök tooenable azt. Lehetővé teszi hello ügynök hello VM adatgyűjtés indul, és adatok tooLog Analytics küld.

### <a name="toocreate-a-virtual-machine"></a>a virtuális gépek toocreate

- Hello kövesse a következő [az első Windows rendszerű virtuális gép létrehozása az Azure-portálon hello](../virtual-machines/virtual-machines-windows-hero-tutorial.md) és hello új virtuális gép elindításához.

### <a name="connect-hello-virtual-machine-toolog-analytics"></a>Csatlakozás hello virtuális gép tooLog elemzés

- Kövesse a hello utasításokat [csatlakozás Azure virtuális gépek tooLog Analytics](log-analytics-azure-vm-extension.md) tooconnect hello VM tooLog analyticset hello Azure-portálon.

## <a name="6-view-and-act-on-data"></a>6 Adatok megtekintése és reagálás

Korábban engedélyezte hello tevékenység Naplóelemzési megoldás és hello biztonsági, megfelelőségi és automatizálás & vezérlő szolgáltatásajánlatok. A következő lépésben elkezdjük áttekinteni a megoldások által gyűjtött adatokat és a naplókeresések során kapott eredményeket.

toostart, nézze meg a megjelenő adatokat a megoldások belül. Ezután nézzünk meg néhány, a naplókeresésekben elérhető keresési eredményt. Napló keresések toocombine lehetővé teszi, és a számítógép adatainak a környezetben több forrásból. További információkért lásd: [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) , vagy ha a munkaterületet toohello új lekérdezési nyelv alakította át, keresse meg a [ismertetése napló keres a Naplóelemzési](log-analytics-log-search-new.md). 

### <a name="tooview-antimalware-data"></a>tooview kártevőirtó adatok

1. Az Azure-portálon hello, keresse meg a túl**Naplóelemzési** > ***a munkaterület***.
2. A munkaterület hello panelen a **általános**, kattintson a **áttekintése**.  
    ![Áttekintés](./media/log-analytics-get-started/overview.png)
3. Kattintson a hello **kártevőirtó Assessment** csempére. Ebben a példában láthatja, hogy a Windows Defender telepítve van az egyik számítógépen, azonban az aláírása elavult.  
    ![Kártevőirtó](./media/log-analytics-get-started/solution-antimalware.png)
4. Az ebben a példában a **védelmi állapot**, kattintson a **aláírás elavult** tooopen naplófájl-keresési és hello számítógépek adatait, amely elavult aláírással rendelkeznek-e. Ebben a példában, vegye figyelembe, hogy hello a számítógépnek a neve *getstarted*. Ha egynél több számítógép elavult aláírásokkal, azok összes jelennek meg hello napló keresése az eredmények.  
    ![A kártevőirtó elavult](./media/log-analytics-get-started/antimalware-search.png)

### <a name="tooview-security-and-audit-data"></a>Biztonsági és naplózási adatok tooview

1. A munkaterület hello panelen a **általános**, kattintson a **áttekintése**.  
2. Kattintson a hello **biztonsági és naplózási** csempére. Példánkban két jelentős problémát láthat: az egyik számítógépről kritikus frissítések hiányoznak, egy másikon pedig nincs elégséges védelem.  
    ![Biztonság és naplózás](./media/log-analytics-get-started/security-audit.png)
3. Az ebben a példában a **jelentős problémák**, kattintson a **gépek, amelyekről kritikus frissítések hiányoznak** tooopen naplófájl-keresési és részleteit gépek, amelyekről kritikus frissítések hiányoznak. Példánkban egy kritikus frissítés és 63 egyéb frissítés hiányzik.  
    ![Biztonság és naplózás naplókeresése](./media/log-analytics-get-started/security-audit-log-search.png)

### <a name="tooview-and-act-on-system-update-data"></a>tooview és a törvény Rendszerfrissítés adatokon

1. A munkaterület hello panelen a **általános**, kattintson a **áttekintése**.  
2. Kattintson a hello **rendszer frissítések értékelését** csempére. Példánkban azt láthatja, hogy van egy *getstarted* nevű Windows-számítógép, amelyről kritikus frissítések hiányoznak, egy másik számítógép pedig definíciófrissítésekre szorul.  
    ![Rendszerfrissítések](./media/log-analytics-get-started/system-updates.png)
3. Az ebben a példában a **hiányzó frissítések**, kattintson a **kritikus frissítések** tooopen naplófájl-keresési és részleteit gépek, amelyekről kritikus frissítések hiányoznak. Példánkban egy frissítés hiányzik és egy szükséges.  
    ![Rendszerfrissítések naplókeresése](./media/log-analytics-get-started/system-updates-log-search.png)
4. Nyissa meg toohello [Operations Management Suite](http://microsoft.com/oms) webhelyet, és jelentkezzen be az Azure-fiókjával. Ha jelentkezett be, láthatja, hogy hello információkat hasonló toowhat hello Azure-portálon látható.  
    ![OMS-portál](./media/log-analytics-get-started/oms-portal.png)
5. Kattintson a hello **frissítéskezelés** csempére.
6. Hello frissítése kezelési irányítópulton láthatja, hogy hello rendszer frissítésekkel kapcsolatos információk hasonló toohello rendszer frissíteni információkat korábban már látott hello Azure-portálon. Azonban hello **frissítési telepítések kezelése** egy új csempe. Kattintson a hello **frissítési telepítések kezelése** csempére.  
    ![Frissítéskezelés csempe](./media/log-analytics-get-started/update-management.png)
7. A hello **telepítések frissítése** kattintson **Hozzáadás** toocreate egy *frissítési menetet*.  
    ![Frissítéstelepítések](./media/log-analytics-get-started/update-management-update-deployments.png)
8.  A hello **új központi telepítésének** lapon adjon meg egy nevet hello központi telepítéséhez, és válassza ki a számítógépek tooupdate (ebben a példában *getstarted*), válassza ki egy ütemezést, és kattintson a **mentése**.  
    ![Új telepítés](./media/log-analytics-get-started/new-deployment.png)  
    Hello központi telepítésének mentése után megjelenik-e ütemezett hello frissítése.  
    ![ütemezett frissítés](./media/log-analytics-get-started/scheduled-update.png)  
    Hello frissítési menet befejezése után hello állapotát jeleníti meg **befejezett**.
    ![befejezett frissítés](./media/log-analytics-get-started/completed-update.png)
9. Hello frissítési menet befejezése után e hello futtatása sikeres volt-e, és megtekintheti, hogy mi frissítések alkalmazása adatait tekintheti meg.

## <a name="after-evaluation"></a>A kiértékelést követően

Ebben az oktatóanyagban egy ügynököt telepített egy virtuális gépre, és azonnal munkához is látott. hello lépéseket követte gyors és egyszerű volt. A legtöbb nagyobb szervezet és vállalat azonban összetett helyszíni informatikai infrastruktúrával rendelkezik. Igen adatainak begyűjtése e összetett környezetek veszi további tervezési és tevékenységi, mint az oktatóanyag hello. Tekintse át a következő további lépések szakaszt az hivatkozások toohelpful cikkek hello hello információit.

Eltávolíthatja az oktatóanyaghoz létrehozott hello munkaterület nem kötelező.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók: kapcsolódás [Windows-ügynökök](log-analytics-windows-agents.md) tooLog elemzés.
* További tudnivalók: kapcsolódás [Operations Manager-ügynökök](log-analytics-om-agents.md) tooLog elemzés.
* [Log Analytics-megoldások hozzáadása hello megoldások gyűjtemény](log-analytics-add-solutions.md) tooadd funkciókat és összefog adatokat.
* Ismerkedjen meg [keresések jelentkezzen](log-analytics-log-searches.md) tooview részletes megoldások által összegyűjtött adatokat.
