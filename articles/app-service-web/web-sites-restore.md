---
title: "Alkalmazás visszaállítása az Azure-ban"
description: "Útmutató: az alkalmazás visszaállítása biztonsági másolatból."
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
ms.openlocfilehash: 5fe74d992edb7028fa4a2500e427013d98ebc250
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="restore-an-app-in-azure"></a>Alkalmazás visszaállítása az Azure-ban
A cikkből megtudhatja, hogyan lehet visszaállítani az alkalmazásban [Azure App Service](../app-service/app-service-value-prop-what-is.md) korábban biztonsági (lásd: [készítsen biztonsági másolatot az alkalmazás az Azure-ban](web-sites-backup.md)). Az alkalmazás a csatolt adatbázisok az igény a visszaállítás egy korábbi állapotára, vagy hozzon létre egy új alkalmazást, az eredeti alkalmazás biztonsági mentési valamelyike alapján. Az Azure App Service a következő adatbázisok biztonsági mentését és helyreállítását támogatja:
- [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
- [A MySQL (előzetes verzió) Azure-adatbázis](https://azure.microsoft.com/en-us/services/mysql)
- [Azure-adatbázis PostgreSQL (előzetes verzió)](https://azure.microsoft.com/en-us/services/postgres)
- [ClearDB MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [MySQL alkalmazásbeli](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Futó alkalmazások számára elérhető visszaállítása biztonsági másolatból rendszer **szabványos** és **prémium** réteg. Az alkalmazás vertikális felskálázásával kapcsolatos információkért lásd: [vertikális felskálázás az Azure alkalmazásban](web-sites-scale.md). **Prémium szintű** réteg lehetővé teszi, hogy a napi biztonsági mentését, mint nagyobb számú **szabványos** réteg.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Egy alkalmazás meglévő biztonsági másolaton visszaállítása
1. Az a **beállítások** panelen található az alkalmazás az Azure portálon kattintson **biztonsági mentések** megjelenítéséhez a **biztonsági mentések** panelen. Kattintson a **visszaállítása**.
   
    ![Válassza ki a visszaállítási most][ChooseRestoreNow]
2. Az a **visszaállítása** panelen, először válassza ki a biztonsági mentési forrását.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    A **alkalmazás biztonsági mentési** beállítást választja, megjelenik az összes meglévő biztonsági mentését az aktuális alkalmazás, és egyszerűen kiválaszthatja az egyik.
    A **tárolási** beállítás lehetővé teszi bármely biztonsági mentési ZIP-fájl válasszon a meglévő Azure Storage-fiók és tároló az előfizetésben.
    Ha próbál biztonsági másolatát egy másik alkalmazás használja a **tárolási** lehetőséget.
3. Ezt követően adja meg az alkalmazás visszaállításához **visszaállítási célhelyének ellenőrzése**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Ha úgy dönt, **felülírása**, az összes meglévő adatok az aktuális alkalmazás törlése, és írja felül. Kattintás előtt **OK**, győződjön meg arról, hogy pontosan mit kíván tenni.
   > 
   > 
   
    Kiválaszthatja **meglévő App** alkalmazás biztonsági másolat visszaállítása a azonos resoure csoport egy másik alkalmazásnak. Mielőtt ezt a beállítást használja, kell már létrehozott egy másik alkalmazás a tükrözési adatbázis konfigurációja egy alkalmazás biztonsági mentési definiálva az erőforráscsoportban. Létrehozhat egy **új** app visszaállítani a tartalmat.

4. Kattintson az **OK** gombra.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Töltse le, illetve törölhet egy biztonsági mentés a storage-fiók
1. A fő **Tallózás** panel az Azure portálon, válassza a **tárfiókok**. A meglévő tárfiókok listája jelenik meg.
2. Válassza ki a kívánt letöltheti és törölheti a biztonsági másolatot tartalmazó tárfiókot. A storage-fiók panelen jelenik meg.
3. A storage-fiók panelen válassza ki a kívánt tároló
   
    ![Nézet tárolók][ViewContainers]
4. Válassza ki a biztonságimásolat-fájl letöltése vagy törölni szeretné.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Kattintson a **letöltése** vagy **törlése** attól függően, hogy mit kíván tenni.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>A figyelő a visszaállítási művelet
A sikeres vagy sikertelen volt-e az alkalmazás-visszaállítási művelet részleteinek megtekintéséhez lépjen a **tevékenységnapló** panel az Azure portálon.  
 

Megtekintéséhez görgessen le a kívánt visszaállítási műveletet, és kattintással jelölje ki azt.

A részleteket tartalmazó panelt jeleníti meg a rendelkezésre álló információkat a visszaállítási művelethez kapcsolódó.

## <a name="next-steps"></a>Következő lépések
Biztonsági mentéshez, és állítsa vissza az App Service apps REST API használatával (lásd: [biztonsági mentése és visszaállítása az App Service apps használata REST](websites-csm-backup.md)).


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
