---
title: "aaaAzure Analysis Services-adatbázis biztonsági mentése és visszaállítása |} Microsoft Docs"
description: "Ismerteti, hogyan toobackup és visszaállítása egy Azure Analysis Services adatbázis."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: cf0a782d237a95fdfa5ef628f998bd053aac0d9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-restore"></a>Biztonsági mentés és visszaállítás

Az Azure Analysis Services táblázatos modell adatbázisainak biztonsági mentése, sokkal hello ugyanaz, mint a helyszíni Analysis Services. hello elsődleges különbség az, ahol a biztonsági mentési fájljait tárolja. A biztonságimásolat-fájlok tooa tárolóhoz kell menteni egy [Azure storage-fiók](../storage/common/storage-create-storage-account.md). Használhatja a tárfiók és tároló már rendelkezik, vagy létrehozása a kiszolgáló tárolási beállításainak konfigurálásakor.

> [!NOTE]
> A storage-fiók létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti. több, lásd: toolearn [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/blobs/).
> 
> 

Biztonsági másolatok abf-fájl kiterjesztéssel együtt. A memóriában táblázatos modellek esetében a modell adatai és a metaadatok tárolja. DirectQuery táblázatos modellek esetében csak a modell metaadatait tárolja. Biztonsági mentések tömörített, és attól függően, hogy a kiválasztott lehetőségektől hello titkosított. 



## <a name="configure-storage-settings"></a>A tárolási beállítások konfigurálása
A biztonsági másolatot, előtt tooconfigure tárolási beállítások a kiszolgáló szükséges.


### <a name="tooconfigure-storage-settings"></a>tooconfigure tárolási beállításai
1.  Az Azure portál > **beállítások**, kattintson a **biztonsági mentés**.

    ![Beállítások a biztonsági másolatok](./media/analysis-services-backup/aas-backup-backups.png)

2.  Kattintson a **engedélyezve**, majd kattintson a **tárolási beállítások**.

    ![Bekapcsolás](./media/analysis-services-backup/aas-backup-enable.png)

3. Válassza ki a tárfiók, vagy hozzon létre egy újat.

4. Jelöljön ki egy tárolót, vagy hozzon létre egy újat.

    ![Válassza ki a tárolót](./media/analysis-services-backup/aas-backup-container.png)

5. A biztonsági mentési beállítások mentéséhez.

    ![Biztonsági mentési beállításainak mentése](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Biztonsági mentés

### <a name="toobackup-by-using-ssms"></a>toobackup szolgáltatáshoz az SSMS használatával

1. Az SSMS, kattintson a jobb gombbal egy adatbázis > **készítsen biztonsági másolatot**.

2. A **adatbázis biztonsági másolata** > **biztonságimásolat-fájl**, kattintson a **Tallózás**.

3. A hello **mentés fájlt** párbeszédpanelen ellenőrizze hello mappa elérési útját, és írja be a hello biztonságimásolat-fájl nevét. 

4. A hello **adatbázis biztonsági másolata** párbeszédpanelen válassza a beállítások.

    **Engedélyezi a fájl felülírása** -válassza ezt a beállítást toooverwrite biztonságimásolat-fájlok hello ugyanazzal a névvel. Ha ez a beállítás nincs bejelölve, hello fájlt ment hello ugyanaz a neve nem lehet egy már létező fájlt a hello azonos helyen.

    **Tömörítés alkalmazása** -ezt a beállítást toocompress hello a biztonsági mentési fájlt. Tömörített biztonságimásolat-fájlok helymegtakarítás, de valamivel nagyobb processzorhasználatot igényel. 

    **Biztonsági másolat titkosításához** -ezt a beállítást tooencrypt hello a biztonsági mentési fájlt. Ehhez szükség van a felhasználó által megadott jelszó toosecure hello biztonsági másolatból. hello jelszó megakadályozza, hogy bármilyen más módon, mint a visszaállítási művelet hello biztonsági mentési adatok olvasásakor. Ha tooencrypt biztonsági mentések, hello jelszó biztonságos helyen tárolja.

5. Kattintson a **OK** toocreate és hello biztonságimásolat-fájl mentése.


### <a name="powershell"></a>PowerShell
Használjon [Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) parancsmag.

## <a name="restore"></a>Visszaállítás
Visszaállításakor, a biztonságimásolat-fájlt úgy konfigurálta, a kiszolgáló hello tárfiókban kell lennie. Ha egy biztonságimásolat-fájlt egy helyszíni hely tooyour tárkonfigurációt toomove van szüksége, [Microsoft Azure Tártallózó](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) vagy hello [AzCopy](../storage/common/storage-use-azcopy.md) parancssori segédprogram. 



> [!NOTE]
> Ha egy helyi kiszolgálóról most visszaállítása, távolítsa el hello minden tartományi felhasználó hello modell szerepkörből, majd adja hozzá toohello szerepkörök Azure Active Directory-felhasználók biztonsági.
> 
> 

### <a name="toorestore-by-using-ssms"></a>toorestore szolgáltatáshoz az SSMS használatával

1. Az SSMS, kattintson a jobb gombbal egy adatbázis > **visszaállítása**.

2. A hello **adatbázis biztonsági másolata** párbeszédpanelen, a **biztonságimásolat-fájl**, kattintson a **Tallózás**.

3. A hello **található adatbázisfájlok** párbeszédpanelen toorestore kívánt válassza hello fájlt.

4. A **vissza az adatbázist a**, jelölje be hello adatbázis.

5. Adja meg a beállításokat. Biztonsági beállítások hello biztonsági mentésekor használt biztonsági mentési beállításainak egyeznie kell.


### <a name="powershell"></a>PowerShell

Használjon [visszaállítási-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) parancsmag.


## <a name="related-information"></a>Kapcsolódó információk

[Az Azure storage-fiókok](../storage/common/storage-create-storage-account.md)  
[Magas rendelkezésre állás](analysis-services-bcdr.md)     
[Az Azure Analysis Services kezelése](analysis-services-manage.md)
