---
title: "az Azure Backup aaaConfigure jelentések"
description: "Ez a cikk beszél Power BI-jelentések konfigurálása az Azure Backup használatával Recovery Services-tároló."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 86e465f1-8996-4a40-b582-ccf75c58ab87
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 503a240b36ea999e0fea434b6a50d26ddf7677bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-backup-reports"></a>Azure Backup-jelentések konfigurálása
Ez a cikk beszél lépéseket tooconfigure jelentések az Azure biztonsági mentést a Recovery Services-tároló és tooaccess ezeket a jelentéseket a Power BI használatával. A lépések elvégzése után közvetlenül megnyithatja tooPower BI tooview minden hello jelentést, testreszabása és jelentések létrehozása. 

## <a name="supported-scenarios"></a>Támogatott helyzetek
1. Az Azure biztonsági mentés jelentések Azure virtuális gép biztonsági mentése és a fájl vagy mappa biztonsági mentési toocloud Azure Recovery Services Agent használata esetén támogatottak.
2. Az Azure SQL, a DPM és az Azure Backup Server jelentések jelenleg nem támogatottak.
3. Jelentések megtekintéséhez tárolók és között előfizetések, ha ugyanazt a tárfiókot az egyes hello tárolók van konfigurálva. Kiválasztott tárfiók kell hello megegyező régióban helyreállítási szolgáltatások tárolóban.
4. hello jelentések az ütemezett frissítési gyakoriságának hello a Power BI 24 óra. Egy ad hoc frissítése hello jelentést a Power BI, amelyek az ügyfél tárfiókja eset legújabb adatait használják a jelentések megjelenítése is elvégezheti. 

## <a name="prerequisites"></a>Előfeltételek
1. Hozzon létre egy [Azure storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) tooconfigure a jelentéseket. Ez a tárfiók jelentések kapcsolódó adatok tárolására szolgál.
2. [A Power BI-fiók létrehozása](https://powerbi.microsoft.com/landing/signin/) tooview, testreszabása és saját Power BI-portál használatával jelentéseket készíthet.
3. Hello erőforrás-szolgáltató regisztrálása **Microsoft.insights** nincs regisztrálva már, ha hello előfizetői tárfióknak és Recovery Services-előfizetésével hello tároló jelentési adatok tooflow toohello tooenable Storage-fiók. toodo hello azonos, akkor be kell lépnie tooAzure portal > előfizetés > erőforrás-szolgáltatók és a szolgáltató tooregister ellenőrzése azt. 

## <a name="configure-storage-account-for-reports"></a>Tárfiók jelentések konfigurálása
A következő lépéseket tooconfigure hello tárfiók a recovery services-tároló Azure-portál használatával hello használata. Ez az egyszeri, és ha konfigurálva van a tárfiók, tooPower BI elvégezheti a közvetlen jelentések tooview tartalomcsomag és használja ki.
1. Ha már rendelkezik nyissa meg a Recovery Services-tároló, a folytatáshoz toonext lépés. Ha nem rendelkezik a Recovery Services-tároló nyílt, de a hello Azure-portálon, hello központ menüben kattintson a **Tallózás**.

   * Írja be az erőforrások listájához hello, **Recovery Services**.
   * Írja be megkezdése előtt, hello szűrők megjelenítése a megadott feltételeknek. Amikor meglátja a **Recovery Services-tárolót**, kattintson rá.

      ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Recovery Services-tárolók hello listája jelenik meg. Recovery Services-tárolók hello listában jelölje ki a tárolóban.

     Megnyitja a kijelölt tároló irányítópult hello.
2. A hello elemekből álló listák tárolóban megjelenik, kattintson az **biztonsági jelentések** figyelés és jelentéskészítés szakasz tooconfigure hello storage-fiók a jelentések szerint.

      ![Válassza ki biztonsági jelentések menü cikk 2. lépés](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. Hello biztonsági jelentések paneljén kattintson **konfigurálása** gombra. Ekkor megnyílik a tárházat adatok toocustomer tárfiók használt hello Azure Application Insights panelen.

      ![Konfigurálja a tárolási fiók lépés 3](./media/backup-azure-configure-reports/configure-storage-account.PNG)
4. Hello állapot váltógomb túl beállítása**a** válassza ki **tooa Tárfiók archiválására** jelölőnégyzetet, hogy a jelentési adatok elindíthatja toohello tárfiókban továbbítására.

      ![4. lépés: a diagnosztika engedélyezésével](./media/backup-azure-configure-reports/set-status-on.png)
5. Kattintson a Tárfiók objektumválasztó és select hello tárfiók hello listát tárolására a jelentési adatokat, és kattintson a **OK**.

      ![Válassza ki a tárolási fiók lépés 5](./media/backup-azure-configure-reports/select-storage-account.png)
6. Válassza ki **AzureBackupReport** négyzet jelölését, majd áthelyezése is hello csúszkát tooselect megőrzési időtartam a jelentéskészítéshez szükséges adatok. A csúszkával kijelölt hello időszak tartják a Reporting hello tárfiókban lévő adatokat.

      ![Válassza ki a tárolási fiók lépés 6](./media/backup-azure-configure-reports/save-configuration.png)
7. Tekintse át az összes hello módosításokat, és kattintson a **mentése** felül, gombra kattint, a fenti hello ábrán látható módon. Ez a művelet biztosítja, hogy a módosítások mentése és a storage-fiók ezzel konfigurálva van a jelentéskészítési adatok tárolására.

> [!NOTE]
> Ha megfelelően konfigurált jelentések úgy, hogy elmenti storage-fiók, akkor **Várjon 24 órát** a kezdeti adatok leküldéses toocomplete. Azure biztonsági mentés a tartalomcsomag a Power BI csak az adott idő után kell importálni. Tekintse meg a [feltett](#frequently-asked-questions) motorteljesítmény részleteiről. 
>
>

## <a name="view-reports-in-power-bi"></a>Jelentések megtekintése a Power bi-ban 
Után tárfiók konfigurálása jelent a recovery services-tároló használatával, a áramló adatok toostart reporting körülbelül 24 órát vesz igénybe. A storage-fiók beállítása 24 óra múlva használata hello alábbi lépéseit a tooview jelentéseket a Power bi-ban:
1. [Jelentkezzen be a](https://powerbi.microsoft.com/landing/signin/) tooPower BI.
2. Kattintson a **adatok beolvasása** , és kattintson a Get **szolgáltatások** tartalomtárban csomag. Az említett lépésekkel [Power BI dokumentáció tooaccess content pack](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-packs-services/).

     ![A tartalomcsomag importálása](./media/backup-azure-configure-reports/content-pack-import.png)
3. Típus **Azure biztonsági mentés** keresősávban, és kattintson a **most töltse le innen**.

      ![A tartalomcsomag beolvasása](./media/backup-azure-configure-reports/content-pack-get.png)
4. Adja meg a tárfiók neve hello a fenti 5. lépésben konfigurált, és kattintson a **következő** gombra.

    ![Adja meg a tárfiók neve](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Adja meg a hello tárfiók kulcsa a tárfiókhoz. Is [megtekintése és másolása a tárelérési kulcsok](../storage/common/storage-create-storage-account.md#manage-your-storage-account) tooyour storage-fiókot az Azure portálon lépjen. 

     ![Adja meg a storage-fiók](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>
     
6. Kattintson a **bejelentkezés** gombra. Bejelentkezési befejezését követően kap **adatimportálási** értesítést.

    ![Tartalom csomag importálása](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>
    
    Némi várakozás után kapott **sikeres** értesítési hello importálás befejezése után. Kevés hosszabb tooimport hello tartalomcsomag, ha nagy mennyiségű hello tárfiókban lévő adatokat is eltarthat.
    
    ![Sikeres tartalom csomag importálása](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>
    
7. Adatok importálása sikeresen megtörtént, miután **Azure biztonsági mentés** tartalomcsomag is elérhetővé válik a **alkalmazások** hello navigációs ablaktáblán. hello listában láthatók az újonnan importált jelentések jelző sárga csillag látható Azure biztonsági mentés irányítópult, jelentések és adatkészlet most. 

     ![Az Azure biztonsági mentés tartalomcsomag](./media/backup-azure-configure-reports/content-pack-azure-backup.png) <br/>
     
8. Kattintson a **Azure biztonsági mentés** az irányítópultokat, amely mutatja a rögzített kulcs jelentések.

      ![Az Azure biztonsági mentés irányítópult](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. tooview hello teljes körű jelentések, kattintson a bármely jelentés hello irányítópulton.

      ![Az Azure biztonsági mentési feladat állapota](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Kattintson az egyes lapokon hello jelentések tooview jelentésekben szereplő adott területre.

      ![Az Azure biztonsági mentés jelentések lap](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Gyakori kérdések
1. **Hogyan ellenőrzi Ha jelentésadatait megkezdődött toostorage fiók áramló?**
    
    Elvégezheti a toohello tárolási fiók konfigurált, és válassza a tárolók. Hello tároló szerepel egy bejegyzés insights-logs-azurebackupreport, azt jelzi, hogy jelentésadatait indult továbbítására.

2. **Mi az Azure Backup a tartalomcsomag a Power bi-ban és az adatok leküldéses toostorage fiók hello gyakorisága?**

   A 0 nap felhasználók körülbelül 24 óra toopush adatok toostorage fiók kellene. Ha a kezdeti leküldéses compelete, adatok frissítése a következő hello az alábbi ábrán látható gyakoriság hello. 
      * Kapcsolódó adatok túl**feladatok, a riasztások, a biztonsági mentés elemek, a tárolók, a védett kiszolgálók és a házirendek** fejlesztőre toocustomer tárfiók, és amikor a rendszer naplózza.
      * Kapcsolódó adatok túl**tárolási** fejlesztőre toocustomer tárfiók 24 óránként.
   
    ![Az Azure biztonsági mentés jelentések adatok leküldéses gyakorisága](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  A Power BI rendelkezik egy [ütemezett frissítés naponta egyszer](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Hello tartalomcsomag a Power BI hello adatok manuális frissítését hajtható végre.

3. **Mennyi ideig megőrizhetem hello jelentések?** 

   Tárolási fiók konfigurálása során kiválaszthatja jelentési adatok megőrzési idővel hello storage-fiókra (6. lépés a tárfiók konfigurálása a jelentések szakaszban). Amellett, hogy a is [elemzési jelentések az excel-](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) , és mentheti azokat hosszabb megőrzési időt, az igényeinek megfelelően. 

4. **Látható a jelentésekben lévő adatok hello storage-fiók beállítása után?**

   Az összes hello után létrehozott adatok **"konfigurálása a storage-fiók"** toohello tárfiók leküldött és a jelentésekben lesznek. Azonban **nem elküldte azokat a folyamatban lévő feladatok** a jelentéskészítéshez. Miután hello feladat befejeződik, vagy nem sikerül, tooreports továbbítja.

5. **Ha már konfigurált I hello tárolási fiók tooview jelentések, módosítható hello konfigurációs toouse másik tárolási fiókot?** 

   Igen, hello konfigurációs toopoint tooa eltérő tárfiók módosíthatja. Újonnan konfigurált hello tárfiókot kell használni, tooAzure biztonsági mentés tartalomcsomag kapcsolódás közben. Is ha konfigurálva van egy másik tárolási fiókot, új lenne adatfolyam a ezt a tárfiókot. Azonban a régebbi adatokat (előtt hello konfiguráció módosítása) továbbra is régebbi tárfiókban hello.

6. **Tekinthetők jelentések tárolók és előfizetések között?** 

   Igen, konfigurálhatja a hello ugyanazt a tárfiókot több különböző tárolók tooview cross-tároló jelentéseket. Emellett konfigurálhatja ugyanazt a tárfiókot, a tárolók hello előfizetésekhez. Használhatja ezt a tárfiókot tooAzure biztonsági mentés tartalomcsomag a Power BI tooview hello jelentésekben kapcsolódás közben. Azonban kijelölve tárfiók hello hello kell megegyező régióban helyreállítási szolgáltatások tárolóban.
   
## <a name="next-steps"></a>Következő lépések
Most, hogy konfigurálta hello tárfiókot és importált Azure Backup-tartalomcsomag, hello következő lépésre van toocustomize ezeket a jelentéseket és jelentéskészítési modell toocreate jelentések használata. Tekintse meg a következő cikkekben olvashat hello.

* [Adatmodell reporting Azure Backup segítségével](backup-azure-reports-data-model.md)
* [Szűrés a jelentéseket a Power bi-ban](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Jelentések létrehozása a Power bi-ban](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)

