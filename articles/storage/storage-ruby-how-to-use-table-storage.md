---
title: "Azure Table Storage-ának Ruby aaaHow toouse |} Microsoft Docs"
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
services: storage
documentationcenter: ruby
author: mmacy
manager: timlt
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 9c77ff9f384a776c9bc075b60b351685c61acc36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-ruby"></a>Hogyan toouse Azure Table Storage-ának Ruby
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Table szolgáltatás. hello minták hello Ruby API segítségével készül. hello tárgyalt forgatókönyvekben szerepel a **létrehozása egy tábla törlésével, és a tábla az entitások lekérdezése**.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby-alkalmazás létrehozása
Hogyan toocreate Ruby alkalmazás, lásd: [Ruby sínek webalkalmazás egy Azure virtuális gépen](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-tooaccess-storage"></a>Az alkalmazás tooaccess tároló konfigurálása
Azure Storage toouse, és van szükség toodownload használata hello Ruby azure csomagot tartalmaz, a felhasználók kényelme érdekében szalagtár szerepel, amely hello Storage REST szolgáltatásokkal kommunikálni.

### <a name="use-rubygems-tooobtain-hello-package"></a>RubyGems tooobtain hello csomag használata
1. Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).
2. Típus **gem telepítése azure** hello parancs ablak tooinstall hello gem és függőségeit.

### <a name="import-hello-package"></a>Hello csomag importálása
Kedvenc szövegszerkesztőjével használatához adja hozzá a következő hello toouse tárolási célhelyeként Ruby fájl elejéhez toohello hello:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Egy Azure Storage-kapcsolat beállítása
hello azure modul olvassák hello környezeti változók **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_hozzáférés\_kulcs**információ tooconnect tooyour Azure Storage-fiók szükséges. Ha ezek a környezeti változók nem, meg kell adnia hello fiók használata előtt **Azure::TableService** a hello a következő kódot:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

tooobtain ezeket az értékeket a klasszikus vagy erőforrás-kezelő tárolási fiók hello Azure-portálon:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Keresse meg a kívánt toouse toohello tárfiók.
3. Kattintson a jobb oldali hello hello beállítási paneljén **hívóbetűk**.
4. Hello elérési kulcsok paneljén megjelenő láthatja, hello hozzáférési kulcs 1 és 2 elérési kulcsot. Ezek bármelyikét használhatja.
5. Kattintson a hello másolás ikon toocopy hello kulcs toohello vágólapra.

Ezeket az értékeket a klasszikus tárolási funkciókat biztosító fiókot a klasszikus Azure portálon hello tooobtain:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Keresse meg a kívánt toouse toohello tárfiók.
3. Kattintson a **ELÉRÉSI kulcsok kezelése** hello aljához hello navigációs ablaktáblán.
4. Hello előugró párbeszédpanelen látható hello tárfiók neve, az elsődleges elérési kulcsát és a másodlagos elérési kulcsot. Hozzáférési kulcs hello elsődleges vagy másodlagos is hello is használhatja.
5. Kattintson a hello másolás ikon toocopy hello kulcs toohello vágólapra.

## <a name="create-a-table"></a>Tábla létrehozása
Hello **Azure::TableService** objektum lehetővé teszi, hogy a táblákat és entitásokat. egy tábla toocreate hello használata **létrehozása\_table()** metódust. hello alábbi példa létrehoz egy táblát, vagy nyomtatása hello hiba, ha van ilyen.

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása
egy entitás tooadd először létre kell hoznia egy ujjlenyomat-objektumot, amely meghatározza az entitás tulajdonságait. Vegye figyelembe, hogy minden entitás meg kell adnia egy **PartitionKey** és **RowKey**. Ezek hello egyedi azonosítói az entitások, és sokkal gyorsabb, mint a többi tulajdonság lekérdezett érték. Használja az Azure Storage **PartitionKey** tooautomatically hello tábla entitások terjesztése számos tárolási csomópont feladatait. Az entitások hello azonos **PartitionKey** hello tárolt ugyanahhoz a csomóponthoz. Hello **RowKey** hello tartozik hello partíción belül hello entitás egyedi azonosítója.

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>Egy entitás frissítése
Van több módszer érhető el tooupdate meglévő entitás:

* **frissítés\_entity():** cserélje ki egy meglévő entitás frissítése.
* **Egyesítési\_entity():** meglévő entitás frissíti új tulajdonságértékek egyesíti hello meglévő entitás.
* **Helyezze be\_vagy\_egyesítési\_entity():** cserélje ki egy meglévő entitás frissíti. Ha nem entitás létezik, egy új szúrja be:
* **Helyezze be\_vagy\_cserélje le\_entity():** meglévő entitás frissíti új tulajdonságértékek egyesíti hello meglévő entitás. Ha nem entitás létezik, a program beszúrja egy újat.

hello következő példa bemutatja egy entitás használatával frissítése **frissítése\_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

A **frissítése\_entity()** és **egyesítési\_entity()**, ha frissített hello entitás nem létezik, hello frissítési művelet sikertelen lesz. Ezért ha toostore egy entitás, függetlenül attól, hogy létezik-e, Ehelyett használjon **beszúrása\_vagy\_cserélje le\_entity()** vagy **beszúrása\_vagy \_egyesítési\_entity()**.

## <a name="work-with-groups-of-entities"></a>Az entitások csoportok használata
Néha azt teszi logika toosubmit több művelet együtt egy kötegelt tooensure atomi hello kiszolgáló általi feldolgozás alatt. tooaccomplish, hogy először létre kell hoznia egy **kötegelt** objektumot, majd hello **hajtható végre\_batch()** metódusa **TableService**. hello következő példa bemutatja, két olyan entitásra RowKey 2 és 3 kötegben elküldése. Figyelje meg, hogy az informatikai, csak rendelkező entitások works hello azonos PartitionKey.

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>Egy entitás lekérdezése
egy tábla, használjon hello entitás tooquery **beolvasása\_entity()** metódus úgy, hogy hello táblanév, **PartitionKey** és **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Az entitások készletének lekérdezése
az egy tábla entitások készletének tooquery létrehozása lekérdezés ujjlenyomat-objektumot, és hello használata **lekérdezés\_entities()** metódust. hello következő példa bemutatja a hello entitásokhoz hello első azonos **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Ha hello eredménykészlet túl nagy a lekérdezés egyetlen tooreturn, a folytatási kód visszaadott amellyel tooretrieve következő lapjain.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Az entitástulajdonságok egy részének lekérdezése
A lekérdezéstábla tooa pár tulajdonságok kérhetnek le egy entitás. Ez a módszer, "leképezés" nevű csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű entitásokat. Használjon hello select záradékban és pass hello nevei hello tulajdonságai milyen toobring toohello ügyfél keresztül.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Entitás törlése
egy entitás toodelete hello használata **törlése\_entity()** metódust. Hello nevében hello entitás, hello PartitionKey és hello entitás RowKey tartalmazó hello tábla toopass van szüksége.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Tábla törlése
egy tábla toodelete hello használata **törlése\_table()** metódus és pass hello nevű hello toodelete kívánt táblát.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Következő lépések

* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.
* [Rubyhoz készült Azure SDK](http://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub tárházából

