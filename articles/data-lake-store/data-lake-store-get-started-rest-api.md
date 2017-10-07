---
title: "aaaUse hello REST API tooget Data Lake Store használatába |} Microsoft Docs"
description: "A Data Lake Store a WebHDFS REST API-k tooperform műveletek használata"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: 62fce8293dfee730a61f2a3d37fc138ce7c3afdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Az Azure Data Lake Store használatának első lépései a REST API használatával
> [!div class="op_single_selector"]
> * [Portál](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Ebből a cikkből megtudhatja, hogyan toouse WebHDFS REST API-k és a Data Lake Store REST API-k tooperform fiók kezelése, valamint az Azure Data Lake Store a fájlrendszer-műveletekhez. Az Azure Data Lake Store saját REST API-kkal rendelkezik a fiókkezelési műveletekhez. Mivel azonban a Data Lake Store kompatibilis a HDFS- és a Hadoop-ökoszisztémával, támogatja a WebHDFS REST API-k használatát a fájlrendszer-műveletekhez.

> [!NOTE]
> A Data Lake Store REST API-támogatása hello részletes információkért lásd: [Azure Data Lake Store REST API-referencia](https://msdn.microsoft.com/library/mt693424.aspx).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Active Directory-alkalmazás létrehozása**. Hello Azure AD alkalmazás tooauthenticate hello Data Lake Store-alkalmazás használhatja az Azure ad-val. Nincsenek különböző szempontok tooauthenticate az Azure ad-vel, amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**. További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).
* [cURL](http://curl.haxx.se/). Ez a cikk a cURL toodemonstrate hogyan toomake REST API-t hívja, egy Data Lake Store-fiók használ.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hogyan végezhető el a hitelesítés az Azure Active Directory használatával?
Az Azure Active Directory használatával két megközelítés tooauthenticate is használhatja.

### <a name="end-user-authentication-interactive"></a>Végfelhasználó hitelesítése (interaktív)
Ebben a forgatókönyvben hello alkalmazás kérni fogja a hello felhasználói toolog, és minden hello műveleteket hello hello felhasználó környezetében. Hajtsa végre a következő lépéseket az interaktív hitelesítéshez hello.

1. Az alkalmazáson keresztül átirányítási URL-cím a következő hello felhasználói toohello:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI > használható URL-kódolású toobe kell. A https://localhost esetében tehát használja a következőt: `https%3A%2F%2Flocalhost`)
   > 
   > 
   
    Az oktatóanyagban szereplő hello célból cserélje le a hello helyőrző értékeket a fenti hello URL-ben, és beillesztheti egy webböngésző címsorába. Az Azure bejelentkezési azonosítójával átirányított tooauthenticate lesz. Miután sikeresen jelentkezik be, hello válasz hello böngésző címsorában megjelenik. hello válasz hello formátuma a következő lesz:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Hello engedélyezési kód a hello válaszban rögzítéséhez. Ebben az oktatóanyagban hello webböngésző címsorába hello hello engedélyezési kód másolását, és adja át hello POST kérelem toohello jogkivonat végpontjához az alább látható módon:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > Ebben az esetben hello \<REDIRECT-URI > kódolása nem szükséges.
   > 
   > 
3. a rendszer hello választ, egy JSON-objektum, amely egy hozzáférési jogkivonatot tartalmaz (például `"access_token": "<ACCESS_TOKEN>"`) és egy frissítési (pl. `"refresh_token": "<REFRESH_TOKEN>"`). Az alkalmazás használja hello hozzáférési jogkivonatot az Azure Data Lake Store és hello frissítési jogkivonat tooget való hozzáférés során egy új hozzáférési jogkivonat mikor jár le a hozzáférési tokent.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. Amikor hello hozzáférési jogkivonat lejár, kérhet egy új hozzáférési jogkivonat hello frissítési jogkivonat használatával alább látható módon:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

További információk az interaktív felhasználói hitelesítéssel kapcsolatban: [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Az engedélyezési kód engedélyezési folyamata).

### <a name="service-to-service-authentication-non-interactive"></a>Szolgáltatások közötti hitelesítés (nem interaktív)
Ebben a forgatókönyvben hello hello alkalmazás maga biztosítja saját hitelesítő adatait tooperform hello műveletek. Ehhez hello az alábbihoz hasonló POST-kérelmet kell kiadnia. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

a kérelem hello kimenete egy engedélyezési jogkivonatot tartalmazza (kimaradásával `access-token` hello kimenetben alább), amely ezt követően továbbítják a REST API-hívásokat. Mentse ezt az engedélyezési jogkivonatot egy szövegfájlba, mivel később még szüksége lesz rá a cikkben.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Ebben a cikkben az hello **nem interaktív** megközelítést. A nem interaktív (szolgáltatások közötti hívások) további információkért lásd: [tooservice hívások hitelesítő szolgáltatás](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Data Lake Store-fiók létrehozása
Ez a művelet alapján definiált hello REST API-hívás [Itt](https://msdn.microsoft.com/library/mt694078.aspx).

A következő cURL-parancsot hello használata. Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

A fenti parancs hello, cserélje le a \< `REDACTED` \> hello engedélyezési jogkivonat a korábban kapott. hello kérelem hasznos adatai ezen parancs hello lévő **input.json** hello a megadott fájl `-d` fenti paraméter. hello input.json fájl tartalmának hello hello alábbihoz:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a>Mappák létrehozása Data Lake Store-fiókban
Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

A következő cURL-parancsot hello használata. Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

A fenti parancs hello, cserélje le a \< `REDACTED` \> hello engedélyezési jogkivonat a korábban kapott. Ezzel a paranccsal létrejön egy nevű könyvtárat **mytempdir** hello gyökérmappájában a Data Lake Store-fiók.

Ha hello művelet sikeresen befejeződik a következőhöz hasonló választ kell megjelennie:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Data Lake Store-fiók mappáinak listázása
Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

A következő cURL-parancsot hello használata. Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

A fenti parancs hello, cserélje le a \< `REDACTED` \> hello engedélyezési jogkivonat a korábban kapott.

Ha hello művelet sikeresen befejeződik a következőhöz hasonló választ kell megjelennie:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Adatok feltöltése egy Data Lake Store-fiókba
Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

A következő cURL-parancsot hello használata. Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

A fenti szintaxis hello **-T** paraméter hello fájl helyét, hello meg feltölteni.

hello hasonló toohello következő kimenete:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a>Adatok beolvasása a Data Lake Store-fiókból
Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Az adatok beolvasása a Data Lake Store-fiókból egy kétlépéses folyamat.

* Először küldenie kell egy GET kérelmet hello végpont `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Ezzel visszatér a hely toosubmit hello következő GET kérelmet.
* Ezután küldenie kell hello GET kérelmet hello végpont `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Ez megjeleníti hello hello fájl tartalmát.

Azonban mivel nincs különbség hello a bemeneti paraméterek között hello először és hello második lépésben, használhatja a hello `-L` paraméter toosubmit hello első kérésre. `-L`a beállítás lényegében egyesít két kérelmet egy, és biztosítják a cURL hello kérelem hello új helyen. Végül minden hello kérelem hívások hello kimenet jelenik meg, például alább látható módon. Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Egy kimeneti hasonló toohello következő kell megjelennie:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Fájl átnevezése a Data Lake Store-fiókban
Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Használjon hello alábbi cURL parancs toorename egy fájlt. Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Egy kimeneti hasonló toohello következő kell megjelennie:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Fájl törlése a Data Lake Store-fiókból
Ez a művelet alapján definiált hello WebHDFS REST API-hívás [Itt](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Használjon hello alábbi cURL parancs toodelete egy fájlt. Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Hello hasonló kimenetnek kell megjelennie:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Data Lake Store-fiók törlése
Ez a művelet alapján definiált hello REST API-hívás [Itt](https://msdn.microsoft.com/library/mt694075.aspx).

Használjon hello alábbi cURL parancs toodelete Data Lake Store-fiók. Cserélje le a **\<yourstorename>** elemet saját Data Lake Store-nevére.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Hello hasonló kimenetnek kell megjelennie:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Lásd még:
* [Az Azure Data Lake Store-ral kompatibilis nyílt forráskódú big data-alkalmazások](data-lake-store-compatible-oss-other-applications.md)

