---
title: "aaaUse tooback REST-be, és állítsa vissza az App Service-alkalmazásokhoz"
description: "Megtudhatja, hogyan toouse RESTful API meghívja a tooback és visszaállítása egy alkalmazást az Azure App Service"
services: app-service
documentationcenter: 
author: NKing92
manager: erikre
editor: 
ms.assetid: f94d0aea-edc1-43ab-9b51-ea7375859276
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2016
ms.author: nicking
ms.openlocfilehash: f77bdfc7de1626d04d8d0c0e8979231ae1ced81a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-rest-tooback-up-and-restore-app-service-apps"></a>REST tooback használja, és állítsa vissza az App Service-alkalmazásokhoz
> [!div class="op_single_selector"]
> * [PowerShell](../app-service/app-service-powershell-backup.md)
> * [REST API](websites-csm-backup.md)
> 
> 

[App Service apps](https://azure.microsoft.com/services/app-service/web/) , az Azure-tárfiókba blobok készíthető. hello biztonsági mentés hello app adatbázisokat is tartalmazhat. Ha hello alkalmazást legalább egyszer véletlenül törlik, vagy ha hello alkalmazás igényeinek toobe visszaállít tooa korábbi verziót, visszaállítható a korábbi biztonsági mentésből. Igény szerint bármikor végezhető biztonsági mentések, vagy a biztonsági mentés ütemezése megfelelő időközönként.

Ez a cikk azt ismerteti, hogyan toobackup és visszaállítása egy alkalmazás RESTful API-t kér. Ha például a toocreate és alkalmazás biztonsági mentéseit grafikusan hello Azure-portálon keresztül, lásd: [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service-ben](web-sites-backup.md)

<a name="gettingstarted"></a>

## <a name="getting-started"></a>Első lépések
toosend REST kér, az alkalmazás kell tooknow **neve**, **erőforráscsoport**, és **előfizetés-azonosító**. Ezek az információk találhatók az alkalmazást a hello kattintva **App Service** panelen található hello [Azure-portálon](https://portal.azure.com). A cikkben szereplő példák hello, hogy megadja hello webhely **backuprestoreapiexamples.azurewebsites.net**. Hello alapértelmezett-webalkalmazás-WestUS erőforráscsoport tárolja, és fut a hello azonosító 00001111-2222-3333-4444-555566667777 rendelkező előfizetés.

![A minta webhely-információk][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a>Biztonsági mentés és visszaállítás REST API-n
Most bemutatjuk, hogyan toouse hello REST API toobackup, és állítsa vissza az alkalmazás számos példát. Minden például tartalmaz egy URL-cím és a HTTP-kérés törzsében. hello minta URL-cím kapcsos zárójelek, például {előfizetés-azonosító} becsomagolt helyőrzők tartalmaz. Hello helyőrzőket cserélje le hello megfelelő információkat az alkalmazás. Összehasonlításul Ez minden helyőrző hello példa URL-címek megjelenő magyarázata.

* előfizetés-azonosító – hello Azure-előfizetés tartalmazó hello alkalmazás azonosítója
* erőforrás-csoportnevet – hello erőforrás csoportot tartalmazó hello alkalmazás nevét
* név – hello alkalmazás nevét
* biztonsági mentés-id – hello alkalmazás biztonsági azonosítója

További hello hello API teljes dokumentációját, beleértve néhány választható paraméterek: hello HTTP kérelemben szereplő: hello [Azure erőforrás-kezelő](https://resources.azure.com/).

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a>Egy alkalmazásét az igény szerinti biztonsági mentése
egy alkalmazás tooback azonnal, küldjön egy **POST** kérelem túl**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ biztonsági mentési /**.

Ez hello URL-cím néz a példa webhelyről. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Adjon meg egy JSON-objektum, a kérelem toospecify, mely tárolási fiók toouse toostore hello biztonsági mentési hello törzsében. hello JSON-objektumból, rendelkeznie kell nevű tulajdonság **storageAccountUrl**, mely tartalmazza a [SAS URL-cím](../storage/common/storage-dotnet-shared-access-signature-part-1.md) írási toohello Azure tároló, amely tárolja a biztonsági mentési blokkblob hello megadása. Ha tooback másolatot az adatbázisokról, meg kell adnia egy hello nevek, típusok és a biztonsági mentés hello adatbázisok toobe kapcsolati karakterláncokat tartalmazó lista is.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Hello alkalmazás biztonsági másolatot azonnal hello kérelem érkezik elindul. hello biztonsági mentési folyamat eltarthat egy hosszú ideig toocomplete. hello HTTP-válasz egy Azonosítót, amely segítségével is hello biztonsági mentés egy másik kérelem toosee hello állapotát tartalmazza. Itt található példa hello hello HTTP-válasz tooour biztonsági mentési kérés törzsét.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

> [!NOTE]
> Hibaüzenetek hello HTTP-válasz hello napló tulajdonságában találhatók.
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a>Az automatikus biztonsági mentés ütemezése
A hozzáadása toobacking be egy alkalmazást az igény szerinti azt is beállíthatja a biztonsági mentési toohappen automatikusan.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Állítson be egy új automatikus biztonsági mentés ütemezése
biztonsági mentés ütemezését, tooset küldése egy **PUT** kérelem túl**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config és biztonsági mentési**.

Ez hello URL-cím néz a példában a webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

hello kérelemtörzset rendelkeznie kell egy JSON-objektum hello biztonsági mentési konfigurációját. Íme egy példa, és az összes szükséges hello paramétert.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set tootrue tooenable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Ez a példa hello app toobe automatikusan biztonsági másolat hét naponta konfigurálja. paraméterek hello **frequencyInterval** és **frequencyUnit** együtt határozzák meg, milyen gyakran hello biztonsági mentések fordulhat elő. Az érvényes értékek **frequencyUnit** vannak **óra** és **nap**. Ha például egy alkalmazás 12 óránként tooback frequencyInterval too12 és frequencyUnit toohour beállítása.

Régi biztonsági másolatok hello tárfiók automatikusan törlődnek. Megadhatja a biztonsági mentések hello beállítása is lehet, hogy régi hello **retentionPeriodInDays** paraméter. Ha azt szeretné, hogy a tooalways rendelkezik legalább egy biztonsági mentése, függetlenül attól, hogy régi, beállítás **keepAtLeastOneBackup** tootrue.

### <a name="get-hello-automatic-backup-schedule"></a>Automatikus biztonsági mentés ütemezése hello beolvasása
tooget alkalmazás biztonsági mentési konfiguráció, küldjön egy **POST** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ webhelyek / {name} / config/biztonsági mentés/lista**.

a hely URL-címe hello **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>

## <a name="get-hello-status-of-a-backup"></a>A biztonsági mentés hello állapotának beolvasása
Attól függően, hogy milyen nagy hello alkalmazás, a biztonsági mentés eltarthat egy ideig toocomplete. Biztonsági mentések is sikertelenek, túllépi az időkorlátot, előfordulhat, hogy, vagy részben sikeres. az alkalmazások biztonsági mentések toosee hello állapot küldése egy **beolvasása** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {előfizetés-azonosító} /resourceGroups/ {csoport-erőforrásnév} /providers/ Microsoft.Web/sites/{name}/backups**.

toosee hello állapotát egy biztonsági másolat, küldjön egy GET kérelem toohello URL-címe **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/ { biztonsági mentés-azonosító}**.

Ez hello URL-cím néz a példában a webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

hello adott válasz törzsének egy JSON objektum hasonló toothis példa tartalmazza.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

hello biztonsági állapota egy felsorolt típus. Ez minden lehetséges állapota.

* 0 – esetbejegyzések: hello biztonsági mentés elindult, de még nem fejeződött be.
* 1 – nem sikerült: hello biztonsági mentés sikertelen volt.
* 2 – sikeres: hello biztonsági mentése sikeresen befejeződött.
* 3 – időtúllépésbe került: hello biztonsági mentés nem fejeződött be időben, és meg lett szakítva.
* 4 – létrehozott: hello biztonsági mentési kérelem várakoznia, de nem lett elindítva.
* 5 – kihagyva: hello biztonsági mentés nem haladt esedékes időt. túl sok biztonsági mentések tooa ütemezés.
* 6 – PartiallySucceeded: hello biztonsági mentése sikeres volt, de az egyes fájlok biztonsági mentése nem történt, mert azok nem olvasható. Ez általában akkor fordul elő, mert kizárólagos zárolást hello fájlok lett helyezve.
* 7 – DeleteInProgress: hello biztonsági mentés kért toobe törölve lett, de még nem lett törölve.
* 8 – DeleteFailed: hello biztonsági mentés nem sikerült törölni. Ez akkor fordulhat elő, mert lejárt hello SAS URL-címet, de a használt toocreate hello biztonsági mentés.
* 9 – törölve: hello biztonsági mentés törlése sikeresen megtörtént.

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a>Egy alkalmazás visszaállítása biztonsági másolatból
Ha az alkalmazás törölve lett, vagy ha azt szeretné, toorevert app tooa korábbi verzióját, visszaállíthatja egy biztonsági másolatból hello alkalmazás. a visszaállítás tooinvoke küldése egy **POST** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ biztonsági mentés / {backup-azonosító} / visszaállítása**.

Ez hello URL-cím néz a példában a webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/restore**

Hello kérelemtörzset küld egy JSON-objektum, amely hello visszaállítási művelet hello tulajdonságait tartalmazza. Íme egy példa a tartalmazó minden kötelező tulajdonság:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL toostorage container containing your website backup
    }
}
```

### <a name="restore-tooa-new-app"></a>Új alkalmazás tooa visszaállítása
Néha szükség lehet toocreate egy új alkalmazást a biztonsági mentéshez, már meglévő alkalmazások felülírása helyett visszaállításakor. toodo a, módosítás hello kérelem URL-cím toopoint toohello új alkalmazás toocreate, és hello megadhatjuk **felülírása** tulajdonsága túl hello JSON**hamis**.

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a>Az alkalmazás biztonsági
Ha szeretné, hogy a biztonsági másolat toodelete, küldjön egy **törlése** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ webhelyek / {name} {backup-azonosító} /backups/**.

Ez hello URL-cím néz a példában a webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a>A biztonsági mentés SAS URL-cím kezelése
Az Azure App Service toodelete kísérli meg a biztonsági mentés az Azure Storage hello hello biztonsági másolat létrehozásakor megadott SAS URL-cím használatával. Ha az SAS URL-cím már nem érvényes, hello biztonsági mentés nem lehet törölni, hello REST API-n keresztül. Azonban frissítheti az SAS URL-cím elküldésével társított biztonsági hello egy **POST** kérelem toohello URL-CÍMÉT **https://management.azure.com/subscriptions/ {előfizetés-azonosító} /resourceGroups/ {csoport-erőforrásnév} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Ez hello URL-cím néz a példában a webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/List**

Hello kérelemtörzset küld egy JSON-objektum, amely tartalmazza a hello új SAS URL-címet. Íme egy példa.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> Biztonsági okokból SAS URL-címet a biztonsági mentéséhez kapcsolódó hello nem jelennek meg a biztonsági másolat egy GET kérelem küldésekor. Ha azt szeretné, hogy egy biztonsági mentéséhez kapcsolódó SAS URL-cím tooview hello, küldjön egy POST kérést toohello URL-CÍMÉRE a fenti. Tartalmaz egy üres JSON-objektum hello kérés törzsében. hello hello kiszolgáló válasza az összes adott biztonsági mentés adatai, beleértve az SAS URL-CÍMÉT tartalmazza.
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
