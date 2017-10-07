---
title: "aaaAzure Import/Export jegyzékfájl formátuma |} Microsoft Docs"
description: "További információk a hello meghajtó jegyzékfájl hello formátum, amely leírja, hogy hello leképezési Azure Blob Storage tárolóban lévő blobok és egy meghajtón az importálási vagy exportálási feladat hello Import/Export szolgáltatás között."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f3119e1c-2c25-48ad-8752-a6ed4adadbb0
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: d7e5e1990482916f7ff5f891c97343b52e82b2f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-manifest-file-format"></a>Azure Import/Export service manifest-fájl formátuma
hello meghajtó jegyzékfájl hello leképezése Azure Blob Storage tárolóban lévő blobok és egy importálási vagy exportálási feladat magában foglaló meghajtón lévő fájlokat ismerteti. Az importálási művelet hello jegyzékfájl hello meghajtó előkészítési folyamat részeként jön létre, és tárolása a meghajtón hello hello meghajtó toohello Azure-adatközpont elküldése előtt. Az exportálási művelet során hello jegyzékfájl van által létrehozott és tárolt hello meghajtón hello Azure Import/Export szolgáltatás.  
  
Mind az import és exportálási feladat, hello meghajtó jegyzékfájl tárolja a hello importálja vagy meghajtó; exportálása nem toohello service API-művelet keresztül továbbít.  
  
hello következő általános formát hello meghajtó jegyzékfájl ismerteti:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveManifest Version="2014-11-01">  
  <Drive>  
    <DriveId>drive-id</DriveId>  
    import-export-credential  
  
    <!-- First Blob List -->  
    <BlobList>  
      <!-- Global properties and metadata that applies tooall blobs -->  
      [<MetadataPath Hash="md5-hash">global-metadata-file-path</MetadataPath>]  
      [<PropertiesPath   
        Hash="md5-hash">global-properties-file-path</PropertiesPath>]  
  
      <!-- First Blob -->  
      <Blob>  
        <BlobPath>blob-path-relative-to-account</BlobPath>  
        <FilePath>file-path-relative-to-transfer-disk</FilePath>  
        [<ClientData>client-data</ClientData>]  
        [<Snapshot>snapshot</Snapshot>]  
        <Length>content-length</Length>  
        [<ImportDisposition>import-disposition</ImportDisposition>]  
        page-range-list-or-block-list          
        [<MetadataPath Hash="md5-hash">metadata-file-path</MetadataPath>]  
        [<PropertiesPath Hash="md5-hash">properties-file-path</PropertiesPath>]  
      </Blob>  
  
      <!-- Second Blob -->  
      <Blob>  
      . . .  
      </Blob>  
    </BlobList>  
  
    <!-- Second Blob List -->  
    <BlobList>  
    . . .  
    </BlobList>  
  </Drive>  
</DriveManifest>  
  
import-export-credential ::=   
  <StorageAccountKey>storage-account-key</StorageAccountKey> | <ContainerSas>container-sas</ContainerSas>  
  
page-range-list-or-block-list ::=   
  page-range-list | block-list  
  
page-range-list ::=   
    <PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       Hash="md5-hash"/>]  
    </PageRangeList>  
  
block-list ::=  
    <BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       Hash="md5-hash"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       Hash="md5-hash"/>]  
    </BlockList>  

```

## <a name="manifest-xml-elements-and-attributes"></a>Jegyzék XML-elemek és attribútumok

hello adatok elemek és attribútumok hello meghajtó jegyzék XML-formátumú meg van adva a következő táblázat hello.  
  
|XML-elem.|Típus|Leírás|  
|-----------------|----------|-----------------|  
|`DriveManifest`|Legfelső szintű elem|a gyökérelem hello hello jegyzékfájl. Minden más hello fájlban elemei az elem alatt.|  
|`Version`|Attribútum, karakterlánc|hello jegyzékfájl hello verzióját.|  
|`Drive`|Beágyazott XML-elem.|Minden olyan meghajtó hello jegyzék tartalmazza.|  
|`DriveId`|Karakterlánc|hello hello meghajtó meghajtó egyedi azonosítója. a sorozatszám hello meghajtó lekérdezésével hello meghajtó azonosítóját található. hello meghajtó sorozatszám általában nyomtatva hello kívül hello meghajtó is. Hello `DriveID` elemnek kell szerepelnie minden `BlobList` hello jegyzékfájl elemében.|  
|`StorageAccountKey`|Karakterlánc|Importálási feladatok, ha, és csak akkor, ha szükséges `ContainerSas` nincs megadva. hello feladathoz hozzárendelt hello fiókkulcs hello Azure storage-fiók esetében.<br /><br /> Ez az elem nem szerepel az exportálásba való belefoglalásra hello jegyzékfájljának.|  
|`ContainerSas`|Karakterlánc|Importálási feladatok, ha, és csak akkor, ha szükséges `StorageAccountKey` nincs megadva. hello tároló SAS hello feladattal társított hello blobok eléréséhez. Lásd: [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) annak formátum. Ez az elem nem szerepel az exportálásba való belefoglalásra hello jegyzékfájljának.|  
|`ClientCreator`|Karakterlánc|Megadja a hello ügyfél, amely hello XML-fájl létrehozása. Ez az érték nem értelmezik hello Import/Export szolgáltatás.|  
|`BlobList`|Beágyazott XML-elem.|A blobok, amelyek részei hello importálása vagy exportálása feladat listáját tartalmazza. Minden egyes blob egy blob listában megosztások hello azonos metaadatok és a tulajdonságok.|  
|`BlobList/MetadataPath`|Karakterlánc|Választható. Hello hello lemezen blobok az importálási művelet hello blob listában beállított hello alapértelmezett metaadatokat tartalmazó fájl relatív elérési útját adja meg. A metaadatok a blob-blob alapon opcionálisan felülbírálható.<br /><br /> Ez az elem nem szerepel az exportálásba való belefoglalásra hello jegyzékfájljának.|  
|`BlobList/MetadataPath/@Hash`|Attribútum, karakterlánc|Hello Base16 kódolású MD5 kivonatoló értékét hello metaadatfájl adja meg.|  
|`BlobList/PropertiesPath`|Karakterlánc|Választható. Hello lemezen hello hello alapértelmezett tulajdonságok blobok az importálási művelet hello blob listában beállított fájl relatív elérési útját adja meg. Ezeket a tulajdonságokat a blob-blob alapon opcionálisan felülbírálható.<br /><br /> Ez az elem nem szerepel az exportálásba való belefoglalásra hello jegyzékfájljának.|  
|`BlobList/PropertiesPath/@Hash`|Attribútum, karakterlánc|Hello Base16 kódolású MD5 kivonatoló értékét hello tulajdonságok fájlt adja meg.|  
|`Blob`|Beágyazott XML-elem.|Minden egyes blob listában blob adatait tartalmazza.|  
|`Blob/BlobPath`|Karakterlánc|hello relatív URI toohello blob, hello Tárolónév kezdve. Ha hello blob legfelső szintű tárolója, kell kezdődnie `$root`.|  
|`Blob/FilePath`|Karakterlánc|Adja meg a hello relatív elérési út toohello fájl hello meghajtón. Az exportálási feladatok ha lehetséges; hello fájl elérési útját hello blob elérési út lesz *pl.*, `pictures/bob/wild/desert.jpg` túl exportálja`\pictures\bob\wild\desert.jpg`. Előfordulhat azonban, NTFS nevek toohello-korlátozások miatt blob exportált tooa fájl elérési útja nem hasonlítanak hello blob elérési út.|  
|`Blob/ClientData`|Karakterlánc|Választható. Hello ügyfél megjegyzéseket tartalmaz. Ez az érték nem értelmezik hello Import/Export szolgáltatás.|  
|`Blob/Snapshot`|Dátum és idő|Exportálási feladat esetén nem kötelező. Megadja az exportált blob pillanatkép hello pillanatkép azonosítója.|  
|`Blob/Length`|Egész szám|Adja meg a teljes hossza hello hello blob bájtban. hello érték lehet egy blokkblobhoz tartoznak too200 GB és az oldalakra vonatkozó blob too1 TB fel. Az oldalakra vonatkozó blob Ez az érték 512 többszörösének kell lennie.|  
|`Blob/ImportDisposition`|Karakterlánc|Az importálási feladatok, nincs megadva az exportálási feladat nem kötelező. Azt határozza meg, hogyan hello Import/Export szolgáltatás kezelje a hello eset, hogy az importálás ahol egy blobot a hello ugyanaz a név már létezik. Ha ez az érték nincs megadva hello importálási jegyzékfájlból, hello alapértelmezett értéke `rename`.<br /><br /> Ehhez az elemhez hello értékek a következők:<br /><br /> -   `no-overwrite`: Ha a cél blob már létezik a hello ugyanazzal a névvel hello importálási művelet kihagyja ezt a fájlt importálja.<br />-   `overwrite`: Bármilyen meglévő cél blob teljesen felülírják hello újonnan importált fájl.<br />-   `rename`: hello új blob lesz feltöltve egy módosított nevű.<br /><br /> a szabály átnevezése hello a következőképpen történik:<br /><br /> -Ha hello blob neve nem tartalmaz olyan pont, más néven hozzáfűzésével létrejön `(2)` toohello eredeti blob neve; Ha az új név is ütközik egy meglévő blob neve, majd `(3)` helyett a rendszer hozzáfűzi `(2)`; és így tovább.<br />-Ha hello blob neve pontot tartalmaz, a következő hello utolsó pont hello részét minősül hello bővítmény neve. Hasonló toohello a fenti eljárással `(2)` előtt kerül beillesztésre hello utolsó pont toogenerate egy új nevet; Ha hello új továbbra is ütközik egy meglévő blob neve, majd hello szolgáltatás megpróbál `(3)`, `(4)`, és így tovább, amíg egy nem ütköző név található.<br /><br /> Néhány példa:<br /><br /> hello blob `BlobNameWithoutDot` átnevezésével:<br /><br /> `BlobNameWithoutDot (2)  // if BlobNameWithoutDot exists`<br /><br /> `BlobNameWithoutDot (3)  // if both BlobNameWithoutDot and BlobNameWithoutDot (2) exist`<br /><br /> hello blob `Seattle.jpg` átnevezésével:<br /><br /> `Seattle (2).jpg  // if Seattle.jpg exists`<br /><br /> `Seattle (3).jpg  // if both Seattle.jpg and Seattle (2).jpg exist`|  
|`PageRangeList`|Beágyazott XML-elem.|Az oldalakra vonatkozó blob szükséges.<br /><br /> Az importálás műveletet, az egy importált fájl toobe bájttartományok listáját adja meg. Az eltolás és a hossz hello forrásfájlban hello laptartomány együtt az MD5 kivonatoló hello régió leíró minden laptartomány írja le. Hello `Hash` egy laptartomány attribútumot kell megadni. hello szolgáltatás ellenőrzi, hogy hello üzenetkivonat hello hello BLOB megegyezik-e a számított hello MD5 kivonatoló hello lap tartományból. Tetszőleges számú tartományokat használt toodescribe egy fájlt az importálás hello teljes méretű mentése too1 TB lehet. Az összes lap tartomány eltolása kell rendezve, és nem a rendszer átfedéseket engedélyezettek.<br /><br /> Az exportálási művelet a bájttartományok BLOB lett készlete exportált toohello meghajtó határozza meg.<br /><br /> hello tartományokat együtt vonatkozhat egy blob vagy a fájl csak részleges tartományokat.  nem vonatkozik semmilyen laptartomány hello fájl hello fennmaradó részét egy várható üzenet, és lehet, hogy a benne lévő tartalom nem definiált.|  
|`PageRange`|XML-elem.|Egy laptartomány jelöli.|  
|`PageRange/@Offset`|Attribútum, egész szám|Megadja a hello eltolást hello átviteli fájlt, és ahol hello megadott laptartomány hello blob kezdődik. Ez az érték 512 többszörösének kell lennie.|  
|`PageRange/@Length`|Attribútum, egész szám|Hello laptartomány hello hosszát adja meg. Ez az érték legfeljebb 4 és 512 MB többszörösének kell lennie.|  
|`PageRange/@Hash`|Attribútum, karakterlánc|Hello Base16 kódolású MD5 kivonatoló értékét hello laptartomány adja meg.|  
|`BlockList`|Beágyazott XML-elem.|Az elnevezett blokkolja a blokkblob szükséges.<br /><br /> Az importálási művelet hello tiltólista, amely az Azure Storage importálható blokkokra határozza meg. Az exportálási művelet hello tiltólista határozza meg, ahol minden blokk rendelkezik lett fájlban tárolt hello hello exportálási lemezen. Minden blokk le a hello fájl- és a blokk hossza; eltolás valamennyi blokkja továbbá egy blokk-azonosító attribútum neve, és az MD5 kivonatoló hello blokk tartalmaz. Másolatot too50 000 blokkok lehet használt toodescribe blob.  Blokkok kell lehetnek rendezve eltolás, és együtt közé tartozik a teljes tartomány hello hello fájl *azaz*, csatlakoznia kell térköz nélkül blokkolja. Ha hello blob legfeljebb 64 MB, hello blokk azonosítók valamennyi blokkja nem lehet összes hiányzik, vagy az összes jelent-e. Blokk azonosítói szükséges toobe Base64 kódolású karakterlánc. Lásd: [Put blokk](/rest/api/storageservices/put-block) további azonosítók kért blokkra vonatkozó követelményeket.|  
|`Block`|XML-elem.|A blokk jelöli.|  
|`Block/@Offset`|Attribútum, egész szám|Megadja a hello eltolását, ahol hello megadott blokk kezdődik.|  
|`Block/@Length`|Attribútum, egész szám|Adja meg a bájtok száma hello hello blokkban; Ez az érték legfeljebb 4MB lehet.|  
|`Block/@Id`|Attribútum, karakterlánc|Megadja egy karakterlánc, amely hello Blokkazonosítót hello blokk.|  
|`Block/@Hash`|Attribútum, karakterlánc|Hello blokk hello Base16 kódolású MD5 kivonatának megadása.|  
|`Blob/MetadataPath`|Karakterlánc|Választható. A metaadatfájl hello relatív elérési út megadása Az importálás során hello metaadatok hello cél blob be van állítva. Az exportálási művelet során hello blob metaadatai hello metaadatok fájl hello meghajtón tárolja.|  
|`Blob/MetadataPath/@Hash`|Attribútum, karakterlánc|Hello blob metaadatait tartalmazó fájl hello Base16 kódolású MD5 kivonatának megadása.|  
|`Blob/PropertiesPath`|Karakterlánc|Választható. Hello tulajdonságok fájl relatív elérési útját adja meg. Az importálás során hello tulajdonság a hello cél blob beállítva. Az exportálási művelet során hello blob tulajdonságokat hello meghajtón hello tulajdonságok fájlban tárolja.|  
|`Blob/PropertiesPath/@Hash`|Attribútum, karakterlánc|Hello Base16 kódolású MD5 kivonatoló hello blob tulajdonságai fájl határozza meg.|  
  
## <a name="next-steps"></a>Következő lépések
 
* [Storage Import/Export REST API felülete](/rest/api/storageimportexport/)
