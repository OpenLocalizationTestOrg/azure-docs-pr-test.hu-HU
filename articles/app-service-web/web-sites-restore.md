---
title: "aaaRestore egy alkalmazást az Azure-ban"
description: "Megtudhatja, hogyan toorestore az alkalmazás egy biztonsági másolatból."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 4b54029a9197064f990f29a3c4558c8322668714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-app-in-azure"></a>Alkalmazás visszaállítása az Azure-ban
Ez a cikk bemutatja, hogyan egy alkalmazás toorestore [Azure App Service](../app-service/app-service-value-prop-what-is.md) korábban biztonsági (lásd: [készítsen biztonsági másolatot az alkalmazás az Azure-ban](web-sites-backup.md)). Az alkalmazás a csatolt adatbázisok igény szerinti tooa korábbi állapotának visszaállításához, vagy hozzon létre egy új alkalmazást, az eredeti alkalmazás biztonsági mentési valamelyike alapján. Az Azure App Service adatbázisok biztonsági mentéséhez és visszaállításához a következő hello támogatja:
- [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
- [A MySQL (előzetes verzió) Azure-adatbázis](https://azure.microsoft.com/en-us/services/mysql)
- [Azure-adatbázis PostgreSQL (előzetes verzió)](https://azure.microsoft.com/en-us/services/postgres)
- [ClearDB MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [MySQL alkalmazásbeli](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Visszaállítása biztonsági másolatból a rendszer fut rendelkezésre álló tooapps **szabványos** és **prémium** réteg. Az alkalmazás vertikális felskálázásával kapcsolatos információkért lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md). **Prémium szintű** réteg lehetővé teszi, hogy a napi biztonsági mentések toobe végre, mint nagyobb számú **szabványos** réteg.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Egy alkalmazás meglévő biztonsági másolaton visszaállítása
1. A hello **beállítások** panelen található az alkalmazáshoz a hello Azure portál, kattintson a **biztonsági mentések** toodisplay hello **biztonsági mentések** panelen. Kattintson a **visszaállítása**.
   
    ![Válassza ki a visszaállítási most][ChooseRestoreNow]
2. A hello **visszaállítása** panelen, először válassza hello biztonsági mentési forrás.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    Hello **alkalmazás biztonsági mentési** beállítást választja, megjelenik az összes hello hello aktuális alkalmazás létező biztonsági másolatai, és egyszerűen kiválaszthatja az egyik.
    Hello **tárolási** beállítás lehetővé teszi bármely biztonsági mentési ZIP-fájl válasszon a meglévő Azure Storage-fiók és tároló az előfizetésben.
    Ha azt egy másik alkalmazás biztonsági toorestore, használja a hello **tárolási** lehetőséget.
3. Ezt követően adja a hello alkalmazás visszaállításához hello céljának **visszaállítási célhelyének ellenőrzése**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Ha úgy dönt, **felülírása**, az összes meglévő adatok az aktuális alkalmazás törlése, és írja felül. Kattintás előtt **OK**, győződjön meg arról, hogy pontosan mit toodo.
   > 
   > 
   
    Kiválaszthatja **meglévő App** toorestore hello alkalmazás biztonsági mentési tooanother alkalmazás hello resoure ugyanabban a csoportban. Mielőtt ezt a beállítást használja, kell már létrehozott egy másik alkalmazás a tükrözés adatbázis konfigurációs toohello több hello alkalmazás biztonsági mentése van definiálva az erőforráscsoportban. Létrehozhat egy **új** app toorestore a tartalmat.

4. Kattintson az **OK** gombra.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Töltse le, illetve törölhet egy biztonsági mentés a storage-fiók
1. A fő hello **Tallózás** hello Azure portálon, válassza a panel **tárfiókok**. A meglévő tárfiókok listája jelenik meg.
2. Válassza ki a kívánt hello tárfiók toodownload vagy delete.hello panel jelenik meg hello biztonsági másolatot tartalmazó hello tárfiók.
3. A hello storage-fiók panelen válassza ki a kívánt hello tároló
   
    ![Nézet tárolók][ViewContainers]
4. Válassza ki a biztonságimásolat-fájl szeretné, hogy toodownload vagy törlése.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Kattintson a **letöltése** vagy **törlése** attól függően, hogy milyen azt szeretné, hogy toodo.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>A figyelő a visszaállítási művelet
toosee adatait hello sikerességét vagy sikertelenségét hello app visszaállítási műveletet, nyissa meg a toohello **tevékenységnapló** panel az Azure-portálon hello.  
 

Görgessen le a szükséges hello visszaállítási művelet, és kattintson tooselect toofind azt.

hello Részletek panel információit jeleníti meg hello elérhető kapcsolatos toohello visszaállítási művelet.

## <a name="next-steps"></a>Következő lépések
Biztonsági mentéshez, és állítsa vissza az App Service apps REST API használatával (lásd: [használata REST tooback össze, és állítsa vissza az App Service apps](websites-csm-backup.md)).


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
