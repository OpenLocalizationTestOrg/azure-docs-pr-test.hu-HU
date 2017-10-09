---
title: "a Media Services hibakódok aaaAzure |} Microsoft Docs"
description: "hello a témakör áttekintést nyújt az Azure Media Services hibakód."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: de1ffd6dee8901a3051eb5032536c3669482d6b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-error-codes"></a>Az Azure Media Services-hibakódok
Microsoft Azure Media Services használata esetén előfordulhat, hogy kapott HTTP hibakódok hello szolgáltatást, attól függően, például a hitelesítési tokenek nem támogatott a Media Services tooactions lejár. hello alábbiakban olvashat egy listát **HTTP hibakódok** előfordulhat, hogy a Media Services által visszaadott, és az hello lehetséges okait őket.  

## <a name="400-bad-request"></a>400 Hibás kérés
hello kérelem érvénytelen adatokat tartalmaz, és onnan elutasítva a következő okok miatt hello tooone:

* Egy nem támogatott API-verzió van megadva. Hello legújabb verzióját, lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).
* Nincs megadva a Media Services hello API verziója. Információ a hogyan toospecify hello API-verzió: [Media Services Operations REST API-referencia](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).
  
  > [!NOTE]
  > Ha az hello .NET vagy Java SDK-k tooconnect tooMedia szolgáltatások, hello API-verzió van megadva, ha próbálja, illetve a Media Services elleni valamilyen művelet végrehajtása.
  > 
  > 
* Egy nem definiált tulajdonság lett megadva. hello tulajdonság értéke a hello hibaüzenet. Csak azokat a tulajdonságokat, amelyek egy adott entitás tagjai adható meg. Lásd: [Azure Media Services REST API-referencia](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) entitásokat és a tulajdonságok listája.
* Az érvénytelen tulajdonság-érték lett megadva. hello tulajdonság értéke a hello hibaüzenet. Lásd: hello előző mutató érvényes Tulajdonságtípusok és azok értékeit.
* Tulajdonság értéke nincs megadva, pedig szükséges.
* Hello megadott URL-cím része helytelen értéket tartalmaz.
* Kísérlet történt végrehajtott tooupdate WriteOnce tulajdonság.
* Kísérlet történt toocreate végrehajtott egy feladatot, amely rendelkezik egy bemeneti objektum egy elsődleges AssetFile, amely nincs megadva, vagy nem határozható meg.
* Kísérlet történt végrehajtott tooupdate egy SAS-kereső. SAS-keresők csak létrehozott vagy törölt. Streamelési Locator lehet frissíteni. További információkért lásd: [keresők](https://docs.microsoft.com/rest/api/media/operations/locator).
* Egy nem támogatott műveletnek vagy a lekérdezés el lett küldve.

## <a name="401-unauthorized"></a>401 nem engedélyezett
nem sikerült hitelesíteni hello kérelmet, (előtt is engedélyezhető) a következő okok miatt hello esedékes tooone:

* Hiányzó hitelesítési fejléc.
* Hibás hitelesítési fejléc értéke.
  * hello-token érvényessége lejárt. 
  * hello jogkivonat érvénytelen aláírást tartalmaz.

## <a name="403-forbidden"></a>403 Tiltott
hello kérelem nem engedélyezett miatt a következő okok miatt hello tooone:

* hello Media Services-fiók nem található, vagy már törölték.
* hello Media Services-fiók le van tiltva, és hello kérelem típusa nincs HTTP GET. Szolgáltatási műveleteket, valamint a 403-as választ ad vissza.
* hello hitelesítésére szolgáló jogkivonat nem tartalmaz hello felhasználói hitelesítő adatok: AccountName és/vagy előfizetés-azonosító. Ezek az információk a Media Services felhasználói felületének bővítmény hello hello Azure felügyeleti portálon Media Services-fiókja található.
* hello erőforrás nem érhető el.
  
  * Kísérlet történt végrehajtott toouse egy MediaProcessor, amely nem érhető el a Media Services-fiókhoz.
  * Kísérlet történt egy JobTemplate definiált tooupdate Media Services által.
  * Kísérlet történt toooverwrite végzett néhány más Media Services-fiókhoz tartozó Lokátort.
  * Kísérlet történt toooverwrite végzett néhány más Media Services-fiókhoz tartozó ContentKey.
* nem hozható létre hello erőforrás miatt tooa szolgáltatás elérte a hello Media Services-fiókhoz tartozó kvótát. Hello szolgáltatás kvótákkal kapcsolatos további információkért lásd: [kvóták és korlátozások](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 – Nem található
hello kérelem nem engedélyezett az erőforráson a hello a következő okok miatt tooone:

* Kísérlet történt végrehajtott tooupdate olyan entitás, amely nem létezik.
* Kísérlet történt végrehajtott toodelete olyan entitás, amely nem létezik.
* Kísérlet történt végrehajtott toocreate egy entitás, amely a tooan entitás, amely nem létezik.
* Kísérlet történt végrehajtott tooGET olyan entitás, amely nem létezik.
* Kísérlet történt végrehajtott toospecify egy tárfiókot, amely nincs társítva hello Media Services-fiók.  

## <a name="409-conflict"></a>409 ütközés
hello kérelem nem engedélyezett miatt a következő okok miatt hello tooone:

* Egynél több AssetFile hello megadott neve hello eszköz belül van.
* Kísérlet történt egy második toocreate elsődleges AssetFile hello eszköz belül.
* Kísérlet történt egy ContentKey a hello toocreate megadott azonosító már használatban van.
* Kísérlet történt egy Lokátort a hello toocreate megadott azonosító már használatban van.
* Egynél több IngestManifestFile hello megadott neve hello IngestManifest belül van.
* Kísérlet történt végrehajtott toolink egy második storage encryption ContentKey toohello tárolási titkosított eszköz.
* Kísérlet történt toolink hello azonos ContentKey toohello eszköz.
* Kísérlet történt egy lokátor tooan eszköz, amelynek a tároló hiányzik, vagy már nem társított hello eszköz toocreate.
* Kísérlet történt végrehajtott toocreate egy lokátor tooan eszköz, amely már rendelkezik használatban 5 lokátorokat. (Az azure Storage kikényszeríti hello legfeljebb öt megosztott elérési házirendek egy tárolót.)
* Egy eszköz tooan IngestManifestAsset a tárfiók csatolása nincs hello szülő IngestManifest hello tárfiók azonos használt hello.  

## <a name="500-internal-server-error"></a>500 belső kiszolgálóhiba
Hello kérelem hello a feldolgozás során a Media Services során néhány, amely megakadályozza a folyamatos hello feldolgozási hiba lép fel. Ennek oka lehet miatt a következő okok miatt hello tooone:

* Egy eszköz vagy a feladat létrehozása sikertelen, mert a szolgáltatás kvótaadatok hello Media Services fiók átmenetileg nem érhető el.
* Egy eszköz vagy IngestManifest blob storage tárolók létrehozása sikertelen, mert hello fiók tárfiókadatok átmenetileg nem érhető el.
* Váratlan hiba.

## <a name="503-service-unavailable"></a>503-as szolgáltatás nem érhető el
hello kiszolgáló jelenleg nem tudja tooreceive kérelmek. Ez a hiba oka lehet túl sok kérelem toohello szolgáltatás. Media Services-szabályozás mechanizmus korlátozza az alkalmazások, amelyek túl sok kérelem toohello szolgáltatás hello erőforrás-felhasználása.

> [!NOTE]
> Jelölőnégyzet hello hiba üzenet és a hiba kódja karakterlánc tooget portáltól hello OK kapcsolatos részletesebb információkért hello 503-as hiba. Ez a hiba nem mindig jelenti sávszélesség-szabályozás.
> 
> 

A leírások lehetséges a következők:

* "A kiszolgáló túlterhelt. Előző fut. az ilyen típusú kérelem végrehajtásának {0} másodpercnél."
* "A kiszolgáló túlterhelt. Több mint {0} kérelmek / másodperc is lehet szabályozva."
* "A kiszolgáló túlterhelt. Egynél több {0} kérelmek {1} másodpercen belül is szabályozva."

toohandle Ez a hiba azt javasoljuk, exponenciális vissza az indító újrapróbálkozási logika. Ez azt jelenti, hogy egymást követő hibaválaszok próbálkozások közötti fokozatosan hosszabb ideig vár használatával.  További információkért lásd: [átmeneti hiba kezelési alkalmazás blokk](https://msdn.microsoft.com/library/hh680905.aspx).

> [!NOTE]
> Ha használ [Azure Media Services SDK for .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), hello újrapróbálkozási logika hello 503-as hiba hello SDK által is implementálva lett.  
> 
> 

## <a name="see-also"></a>Lásd még:
[Media Services-felügyeleti hibakódok](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

