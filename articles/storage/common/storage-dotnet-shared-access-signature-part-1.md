---
title: "aaaUsing közös hozzáférésű jogosultságkód (SAS) az Azure Storage |} Microsoft Docs"
description: "Ismerje meg a megosztott toouse hozzáférési jogosultságkódokat (SAS) toodelegate hozzáférés tooAzure tároló-erőforrások, például BLOB, üzenetsorok, táblák és fájlokat."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 46fd99d7-36b3-4283-81e3-f214b29f1152
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2017
ms.author: marsma
ms.openlocfilehash: 5b75a3c25bcfb9f1ceb81f31dc2d42376bd105cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-shared-access-signatures-sas"></a>Közös hozzáférésű jogosultságkód (SAS) használatával

A közös hozzáférésű jogosultságkód (SAS) biztosít egy módon toogrant korlátozott hozzáférés tooobjects a tárfiókban lévő ügyfelek tooother, anélkül, hogy a fiókkulcs. Ebben a cikkben azt az SAS-modell hello nyújt áttekintést, SAS ajánlott eljárásokat, és tekintse meg néhány példa.

Kód itt bemutatott ruházzák SAS használatával tekintse meg a [Ismerkedés az Azure Blob Storage a .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) és egyéb hello elérhető mintákat [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?service=storage) könyvtár. Hello mintaalkalmazások letöltése és futtatni azokat, vagy keresse meg a hello kódja a Githubon.

## <a name="what-is-a-shared-access-signature"></a>Mi az a közös hozzáférésű jogosultságkód?
A közös hozzáférésű jogosultságkód delegált hozzáférést tooresources a tárfiókban lévő biztosít. Az SAS-kód megadásához ügyfelek hozzáférhessenek a tárfiókban lévő tooresources anélkül, hogy a kulcsait megosztása. Ez az hello lényeg az alkalmazásokban – SAS használatával a közös hozzáférésű jogosultságkód van egy biztonságos módon tooshare a tárolási erőforrások a kulcsait veszélyeztetése nélkül.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

SAS-kód lehetővé teszi részletes szabályozását hello típusú hello SAS, beleértve a rendelkező tooclients számára biztosítson hozzáférést:

* hello keresztül mely hello SAS érvényességi időtartama, beleértve hello kezdési ideje és hello lejárati idő.
* hello engedélyekre hello SAS. Például egy blob SAS-kód előfordulhat, hogy adjon olvasási és írási engedélyek toothat blob, de nem törli az engedélyeket.
* Nem kötelező IP-cím vagy az IP-címek tartománya, amelyből elfogadja az Azure Storage hello SAS. Például előfordulhat, hogy adja meg az IP-címek egy tartozó tooyour szervezet.
* hello protokoll, amelyben az Azure Storage hello SAS fogja fogadni. Ez nem kötelező paraméter toorestrict hozzáférés tooclients HTTPS-kapcsolaton keresztül is használhatja.

## <a name="when-should-you-use-a-shared-access-signature"></a>Mikor kell használni a közös hozzáférésű jogosultságkód?
SAS-kód is használhatja, ha a tárolási fiók tooany ügyfél, amelyek nem rendelkeznek a tárfiók tárelérési kulcsok tooprovide hozzáférés tooresources szeretne. A tárfiók egy elsődleges és másodlagos elérési kulcsot, rendszergazdai hozzáféréssel tooyour fiók adja meg mindkettőt, és hozzá tartozó összes erőforrást tartalmaz. Ezek a kulcsok az ilyen megnyitja a rosszindulatú vagy gondatlan használata fiók toohello lehetőségét. Közös hozzáférésű jogosultságkód adjon meg egy biztonságos megoldás, amely lehetővé teszi az ügyfelek tooread, írási és törlési adatok tárfiókba Ön kifejezetten biztosított toohello engedélyek alapján történik, és fiókkulcs szükségessége nélkül.

Egy gyakori forgatókönyv, ahol a biztonsági Társítások akkor hasznos, egy olyan szolgáltatás, amelyben felhasználók írási és olvasási a saját adatok tooyour tárfiók. Forgatókönyv esetében, ahol a tárfiók tárolja a felhasználói adatok a rendszer két jellemző tervezési mintáról olvashat:

1. Ügyfelek töltse fel, és töltse le adatokat, amely végrehajtja a hitelesítési előtér-proxy szolgáltatáson keresztül. Az előtér-proxyszolgáltatás, ha engedélyezi az üzleti szabályok érvényesítési hello előnnyel rendelkezik, de nagy mennyiségű adatok vagy tranzakciók nagy mennyiségű toomatch igény szerint is méretezhető szolgáltatás létrehozása lehet költséges vagy nehézkes.

  ![A forgatókönyv diagramja: előtér-proxyszolgáltatás](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png)   

1. Egy egyszerűsített szolgáltatás hitelesíti hello ügyfelet, igény szerint, és akkor hoz létre egy SAS. Hello ügyfél hello SAS kap, ha közvetlenül a hello SAS és hello SAS által engedélyezett hello időköz definiált hello engedélyek tárfiók erőforrásainak eléréséhez. hello SAS azzal csökkenti az útválasztási hello előtér-proxy szolgáltatás összes adat hello szükségességét.

  ![A forgatókönyv diagramja: SAS-szolgáltató szolgáltatás](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png)   

Számos valós szolgáltatás használhat egy hibrid két megoldás. Például bizonyos adatok feldolgozása lehet, és hello előtér-proxyn keresztül érvényesítése közben egyéb adatok mentése és/vagy olvasása közvetlenül a SAS használatával.

Emellett egy SAS tooauthenticate hello adatforrás-objektum bizonyos esetekben a másolási műveletek a toouse lesz szüksége:

* Egy másik tárolási fiókot tartalmazó blob tooanother blob másolásakor forrás blob SAS tooauthenticate hello kell használnia. Opcionálisan SAS tooauthenticate hello cél blob is használhatja.
* Ha egy másik tárolási fiókot tartalmazó fájl tooanother fájl másolása, SAS tooauthenticate hello forrásfájl kell használnia. Is használhat egy SAS tooauthenticate hello célfájl is.
* Egy blob tooa fájlt, vagy a fájl tooa blob másolásakor kell használnia egy SAS tooauthenticate hello adatforrás-objektum, még akkor is, ha hello forrás és cél-objektumai találhatók hello belül ugyanaz a tárfiók.

## <a name="types-of-shared-access-signatures"></a>Közös hozzáférésű jogosultságkód típusai
Közös hozzáférésű jogosultságkód két típusa hozható létre:

* **SAS-szolgáltatás.** hello szolgáltatás SAS delegáltak csak egy hello tárolószolgáltatások tooa erőforrás elérésére: hello Blob, a várólista, a tábla vagy a fájl szolgáltatás. Lásd: [hozhat létre, egy szolgáltatás SAS](https://msdn.microsoft.com/library/dn140255.aspx) és [szolgáltatás SAS példák](https://msdn.microsoft.com/library/dn140256.aspx) hello szolgáltatás SAS-jogkivonat létrehozásával kapcsolatos részletes információk.
* **SAS-fiók.** hello fiók SAS delegáltak tooresources egy vagy több hello tárolási szolgáltatások eléréséhez. Hello műveleteket elérhető SAS szolgáltatáson keresztül is elérhetők SAS fiók használatával. Emellett hello fiókkal SAS, delegálhatja szolgáltatás, például a megadott tooa vonatkozó hozzáférési toooperations **Get vagy Set szolgáltatástulajdonságok** és **első szolgáltatás-statisztikák**. Delegálhatja hozzáférés tooread, írási és törlési műveletek a blobtárolók, táblák, üzenetsorok és fájlmegosztások a szolgáltatásalapú SAS nem engedélyezett. Lásd: [hozhat létre egy fiókot SAS](https://msdn.microsoft.com/library/mt584140.aspx) a hello fiók SAS-jogkivonat létrehozásával kapcsolatos részletesebb információk.

## <a name="how-a-shared-access-signature-works"></a>A közös hozzáférésű jogosultságkód működése
A közös hozzáférésű jogosultságkód tooone vagy több tároló-erőforrások mutat, és a lekérdezési paraméterek különleges készletét tartalmazó jogkivonatok tartalmazó aláírt URI. hello jogkivonat azt jelzi, hogyan hello erőforrásokat érhetik el hello ügyfél. A hello lekérdezési paraméterek közül, aláírás hello hello SAS paraméterek értékekből összeállított, amely hello fiók kulccsal aláírva. Az aláírás Azure Storage tooauthenticate hello SAS használják.

Íme egy példa a SAS URI-t, ábrázoló hello erőforrás URI és hello SAS-jogkivonat:

![A SAS URI-összetevők](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png)   

hello SAS-jogkivonat: a hello létrehozhat karakterlánc *ügyfél* ügyféloldali (lásd: hello [SAS példák](#sas-examples) kódpéldák szakasz). Egy SAS-jogkivonatot hoz létre a hello a storage ügyféloldali kódtára, például a nyomkövetés nem Azure Storage bármilyen módon. SAS-tokenje korlátlan számú hello ügyféloldalon hozhat létre.

Ha az ügyfél egy SAS URI tooAzure tárolási megad egy kérelem részeként, a hello szolgáltatás ellenőrzi a hello SAS-paraméterek és aláírás tooverify érvényes hello kérelem hitelesítése. Hello szolgáltatás ellenőrzi, hogy hello aláírás érvényes, ha a hello kérelem hitelesítése. Ellenkező esetben hello van elutasította a kérelmet hibakód 403 (tiltott).

## <a name="shared-access-signature-parameters"></a>Megosztott hozzáférési aláírást paraméterek
hello fiók SAS és SAS-tokenje néhány általános paramétereket tartalmaz, és is igénybe vehet néhány paramétert, amely különböző.

### <a name="parameters-common-tooaccount-sas-and-service-sas-tokens"></a>Paraméterek közös tooaccount SAS és SAS-tokenje
* **API-verzió** egy nem kötelező paraméter, amely meghatározza a hello szolgáltatás verziója toouse tooexecute hello kérelem.
* **Verziójú** egyik kötelező paraméter, amely meghatározza a hello szolgáltatás verziója toouse tooauthenticate hello kérelem.
* **Kezdési időpontja.** Ez az hello idő, mely hello SAS hatályba lép. a közös hozzáférésű jogosultságkód hello kezdési időpontja nem kötelező megadni. Ha nincs megadva a kezdési idő, hello SAS azonnal hatékony. hello kezdési ideje kell megadni az UTC (egyezményes világidő), egy különös UTC jelzéssel ("Z"), például `1994-11-05T13:15:30Z`.
* **Lejárati idő.** Ez az hello idő elteltével hello SAS már nem érvényes. A bevált gyakorlat része, hogy adja meg a lejárat időpontjának tartozó SAS korlátozására, vagy rendelje hozzá azt a tárolt házirend. hello lejárati idejének kell megadni az UTC (egyezményes világidő), egy különös UTC jelzéssel ("Z"), például `1994-11-05T13:15:30Z` (további információk alatt).
* **Engedélyek.** hello SAS megadott hello engedélyek jelző hello ügyfél hello storage erőforrás hello SAS használata elleni hajthat végre műveleteket. Elérhető engedélyek a SAS fiók és a szolgáltatásalapú SAS térnek el egymástól.
* **IP-CÍMET.** Egy nem kötelező paraméter, amely megadja egy IP-címet vagy egy Azure-on kívüli az IP-címek (hello című [útválasztás munkamenet-konfiguráció állapota](../../expressroute/expressroute-workflows.md#routing-session-configuration-state) az Express Route) a tooaccept kérések.
* **Protokoll.** Egy nem kötelező paraméter, amely megadja a hello protokollt érkező kérelmek végrehajtására megengedett. Lehetséges értékei a HTTPS és a HTTP (`https,http`), ez az alapértelmezett érték hello, vagy HTTPS csak (`https`). Vegye figyelembe, hogy HTTP csak nem megengedett értéket.
* **Aláírás.** hello aláírás hello értékekből összeállított rész lexikális elem szerepel, és titkosítja, majd más paramétereket. Hogy tooauthenticate hello SAS használják.

### <a name="parameters-for-a-service-sas-token"></a>A szolgáltatás SAS-jogkivonat paraméterek
* **Tároló-erőforrás.** Tároló-erőforrások, amelynek is adhat hozzáférést egy olyan biztonsági Társítások tartalmazzák:
  * Tárolók és blobok
  * A fájlmegosztások és fájlok
  * Üzenetsorok
  * Táblák és a táblaentitásokat tartományait.

### <a name="parameters-for-an-account-sas-token"></a>Egy fiók SAS-jogkivonat paramétereinek
* **Szolgáltatás vagy a szolgáltatások.** SAS fiók hozzáférési tooone vagy több hello tárolószolgáltatások is átadhatja. Például hogy a delegáltak toohello Blob és a fájl szolgáltatás hozzáférési fiók SAS is létrehozhat. Vagy, hogy delegáltak hozzáférjen a tooall négy szolgáltatásokhoz (Blob, várólista, a táblának és fájl) Készletével hozhat létre.
* **Erőforrás-típusú tárolókat.** Egy fiók SAS tooone a tárolási erőforrások ahelyett, hogy egy adott erőforrás további osztályait vonatkozik. Létrehozhat egy fiók SAS toodelegate elérésére:
  * Szolgáltatás szintű API-k, hello tárolóeszközt fiók ellen néven. Példák **Get vagy Set szolgáltatástulajdonságok**, **első szolgáltatás-statisztikák**, és **tárolók/várólisták/táblák, megosztások listája**.
  * Tároló szintű API-k, az egyes szolgáltatásokhoz hello tároló objektumokon néven: blob-tároló, a várólisták, a táblák és fájlmegosztások. Példák **létrehozása vagy törlése tároló**, **létrehozása vagy törlése-várólista**, **tábla létrehozása vagy törlése**, **létrehozása vagy törlése megosztás**, és **lista Blobok/fájlok és könyvtárak**.
  * Objektum szintű API-k, blobok, az üzenetsor-üzeneteket, a táblaentitásokat és a fájlok elleni néven. Például **Put Blob**, **lekérdezés entitás**, **üzeneteket beolvasni**, és **fájl létrehozása**.

## <a name="examples-of-sas-uris"></a>Példák SAS URI-azonosítók

### <a name="service-sas-uri-example"></a>Szolgáltatás SAS URI-példa

Íme egy példa a SAS URI-t, amely írási és olvasási engedélyek tooa blob szolgáltatás. hello tábla megszakad hello URI toounderstand részét képező összes toohello SAS hozzájárul módját:

```
https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D
```

| Név | SAS részére | Leírás |
| --- | --- | --- |
| A BLOB URI |`https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt` |hello blob hello címe. Vegye figyelembe, hogy a HTTPS-kapcsolaton keresztül erősen ajánlott. |
| Storage services, verziószám: |`sv=2015-04-05` |A tárolási szolgáltatások 2012-02-12-es verzió, és később, ez a paraméter azt jelzi, hello verzió toouse. |
| Kezdési idő |`st=2015-04-29T22%3A18%3A26Z` |UTC idő szerint megadva. Ha azt szeretné, hello SAS toobe érvényes azonnal, hagyja ki ezt a hello kezdési ideje. |
| Lejárati idő |`se=2015-04-30T02%3A23%3A26Z` |UTC idő szerint megadva. |
| Erőforrás |`sr=b` |hello erőforrás blob. |
| Engedélyek |`sp=rw` |hello SAS hello jogosultságaitól Read (r) és írjunk (w). |
| IP-címtartomány |`sip=168.1.5.60-168.1.5.70` |hello az IP-címek tartománya, amelyből a kérelem fogja elfogadni. |
| Protokoll |`spr=https` |Csak a HTTPS-kapcsolaton keresztül kérelmek engedélyezettek. |
| Aláírás |`sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D` |Tooauthenticate hozzáférés toohello blob használt. hello aláírás egy HMAC egy karakterlánc-bejelentkezési és a kulcs hello SHA-256 algoritmussal felett, és ezután a Base64 kódolás használatával kódolt. |

### <a name="account-sas-uri-example"></a>Fiók SAS URI-példa

Íme egy példa egy olyan fiók SAS, hogy használ hello hello jogkivonat gyakori paramétereket. Mivel ezek a paraméterek fent ismerteti, hogy a dokumentum nem ismerteti. Csak a hello paraméterek, amelyek adott tooaccount SAS hello az alábbi táblázat ismerteti.

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

| Név | SAS részére | Leírás |
| --- | --- | --- |
| Erőforrás URI azonosítója |`https://myaccount.blob.core.windows.net/?restype=service&comp=properties` |hello Blob-szolgáltatásvégpont, a szolgáltatás Tulajdonságok (Ha hívása GET) vagy (készlettel meghívásakor) szolgáltatási tulajdonságainak beállítása paraméterekkel. |
| Szolgáltatások |`ss=bf` |hello SAS toohello Blob fájl- és vonatkozik |
| Típusú erőforrások |`srt=s` |hello SAS tooservice szintű műveletek vonatkozik. |
| Engedélyek |`sp=rw` |hello engedélyek tooread hozzáférést biztosítani, és az írási műveletek. |

Fényében, hogy az engedélyek korlátozottak toohello szolgáltatási szint, a biztonsági Társítások elérhető műveletek vannak **Blob szolgáltatástulajdonságok** (olvasás) és **Blob szolgáltatás tulajdonságainak beállítása** (írás). Azonban egy másik erőforrás URI hello azonos SAS-tokennel is lehet toodelegate hozzáférés túl használt**lekérése Blob szolgáltatás statisztikák** (olvasás).

## <a name="controlling-a-sas-with-a-stored-access-policy"></a>A biztonsági Társítások tárolt hozzáférési házirenddel vezérlése
A közös hozzáférésű jogosultságkód két űrlap valamelyikét hajthatja végre:

* **Ad hoc biztonsági Társítások:** egy alkalmi SAS, a hello kezdési ideje, a lejárat időpontjának létrehozásakor, és a hello SAS engedélyek az összes megadott hello SAS URI-t (vagy azt jelentette, ha nincs megadva kezdő időpont hello esetében). Az ilyen típusú SAS SAS fiók vagy a szolgáltatásalapú SAS hozhatók létre.
* **SAS tárolt hozzáférési házirenddel:** egy tárolt hozzáférési házirend egy erőforrás tároló – egy blob tároló van definiálva, tábla, várólistára, vagy fájlmegosztásba--és legalább egy közös hozzáférésű jogosultságkód használt toomanage megkötéseit lehet. Ha SAS-kód társítja a tárolt házirend, hello SAS örökli hello megkötések – hello kezdési ideje, a lejárati idő és engedélyek – hello tárolt házirend.

> [!NOTE]
> Egy fiók SAS jelenleg egy ad hoc biztonsági Társítások kell lennie. Hozzáférési házirendek még nem támogatottak fiók SAS tárolja.

hello hello két űrlapok közötti különbség fontos egyik-forgatókönyvben: visszavont tanúsítványok. Mivel a SAS URI-t egy URL-címet, bárki jut hozzá a hello SAS használható, függetlenül attól, aki eredetileg létrehozott. Ha SAS-kód nyilvánosságra, azt bármely hello world használható. A biztonsági Társítások biztosít hozzáférést tooresources tooanyone csak négy dolog történik, amelyek rendelkeznek:

1. hello lejárati idejének megadott hello SAS éri el.
2. hello lejárati idejének hello tárolt házirend hello SAS éri el (Ha a tárolt házirend hivatkozik, és adja meg a lejárat időpontjának) által hivatkozott meg. Ez akkor fordulhat elő, mert hello időtartam, vagy mert tárolt hello hozzáférési házirend egy lejárati idővel a hello korábbi, amely egyirányú toorevoke hello SAS módosította.
3. hello hivatkozás által hello SAS törlődik, amely egy másik módja toorevoke hello SAS hozzáférési házirendben tárolt. Vegye figyelembe, hogy ha a hozzáférési házirendben tárolt hello pontosan hozni hello ugyanazzal a névvel minden meglévő SAS-jogkivonatok fog újra érvényes függően toohello engedélyeket társítani a tárolt házirend (feltéve, hogy még nem múlt el SAS hello hello lejárati idő). Ha a toorevoke hello SAS vannak szándékos volt, esetén meg arról, hogy toouse egy másik nevet hello hozzáférési házirend a jövőbeli hello egy lejárati idővel hozza létre újra.
4. hello kulcsára, de a használt toocreate hello SAS újragenerálják. Egy fiók kulcs újragenerálása eredményezi, hogy a kulcs toofail tooauthenticate segítségével minden alkalmazás-összetevők mindaddig, amíg a mezők frissítése toouse vagy hello más érvényes fiókkulcs vagy újonnan újragenerálni hello kulcsára.

> [!IMPORTANT]
> A közös hozzáférésű jogosultságkód URI hello fiók kulcs használt toocreate hello aláírás társítva, és hello tartozó tárolt házirend (ha van ilyen). Ha nincs tárolt házirend van megadva, hello csak úgy toorevoke egy közös hozzáférésű jogosultságkódot toochange hello fiókkulcs.

## <a name="authenticating-from-a-client-application-with-a-sas"></a>A SAS-kód az ügyfélalkalmazás hitelesítése
Ügyfél, aki rendelkezik egy SAS használható hello SAS tooauthenticate egy tárfiókot, amelynek nem rendelkeznek hello kulcsait a kérelmet. SAS-kód szerepel a kapcsolati karakterláncot, vagy közvetlenül használt hello megfelelő konstruktort vagy metódust.

### <a name="using-a-sas-in-a-connection-string"></a>A kapcsolati karakterláncban a SAS használatával
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a>Egy konstruktort vagy metódust a SAS használatával
Több Azure Storage ügyfél könyvtár konstruktorok és metódus túlterhelések kínál a SAS-paramétert, az, hogy a SAS-kód kérelem toohello szolgáltatás képes hitelesíteni.

Például egy SAS URI ez használt toocreate egy hivatkozás tooa blokkblob. hello SAS hello csak a hitelesítő adatok hello kérelem biztosít. hello elérésű blokkblob hivatkozását egy írási művelet használja:

```csharp
string sasUri = "https://storagesample.blob.core.windows.net/sample-container/" +
    "sampleBlob.txt?sv=2015-07-08&sr=b&sig=39Up9JzHkxhUIhFEjEH9594DJxe7w6cIRCg0V6lCGSo%3D" +
    "&se=2016-10-18T21%3A51%3A37Z&sp=rcw";

CloudBlockBlob blob = new CloudBlockBlob(new Uri(sasUri));

// Create operation: Upload a blob with hello specified name toohello container.
// If hello blob does not exist, it will be created. If it does exist, it will be overwritten.
try
{
    MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    msWrite.Position = 0;
    using (msWrite)
    {
        await blob.UploadFromStreamAsync(msWrite);
    }

    Console.WriteLine("Create operation succeeded for SAS {0}", sasUri);
    Console.WriteLine();
}
catch (StorageException e)
{
    if (e.RequestInformation.HttpStatusCode == 403)
    {
        Console.WriteLine("Create operation failed for SAS {0}", sasUri);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    else
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}

```

## <a name="best-practices-when-using-sas"></a>Gyakorlati tanácsok SAS használatával
Közös hozzáférésű jogosultságkód az alkalmazásokban való használatakor kell toobe két esetleges kockázatokat tudomást:

* Ha SAS-kód kiszivárgott, használat bárki, aki szerzi be, ami kedvezőtlenül befolyásolhatja a potenciálisan a tárfiók.
* Amennyiben egy SAS tooa ügyfélalkalmazás lejár, és hello alkalmazás nem tooretrieve szolgáltatás új SAS, hello alkalmazás funkcióinak előfordulhat, hogy korlátozva.

hello következő javaslatok a közös hozzáférésű jogosultságkód segítségével a kockázatok csökkentése érdekében:

1. **Mindig HTTPS PROTOKOLLT használnak** toocreate vagy egy SAS terjesztése. Ha SAS-kód átadott HTTP Protokollon keresztül, és elfogta, a-átjárójának támadás egy támadó képes tooread hello SAS, és majd használni ugyanúgy, mint a hello szánt felhasználói rendelkezhetnek, ezzel együtt a bizalmas adatokat, vagy a program sérült adatokat hello lehetővé rosszindulatú felhasználók.
2. **Hivatkozás tárolt hozzáférési házirendeket, ahol csak lehetséges.** Hozzáférési házirendek adjon meg beállítás toorevoke engedélyek hello anélkül, hogy tooregenerate hello tárfiókkulcsok tárolja. Be hello lejárati ezek nagyon, amennyiben az hello jövőbeli (vagy végtelen), és győződjön meg arról, hogy rendszeresen frissített toomove távolabb jövőbeli hello be azt.
3. **Az ad hoc biztonsági Társítások near-term lejárati időpontjait használja.** Ily módon még akkor is, ha SAS-kód sérült, érvénytelen, így csak rövid ideig. Ez az eljárás különösen fontos, ha a tárolt házirend nem hivatkozhatnak. NEAR-term lejárati időpontjait is korlátozza a hello adatmennyiséget csak írható tooa blob hello idő elérhető tooupload tooit korlátozásával.
4. **Ügyfelek automatikusan megújítani hello SAS szükség van.** Ügyfelek kell megújítani hello SAS hello lejárati, ha hello SAS biztosító hello szolgáltatás nem érhető el az újrapróbálkozások rendelés tooallow ideje előtt. Célja, hogy a SAS toobe azonnali kis számú használatos, ha rövid élettartamú műveletek várt toobe hello elévülési időszakon belül fejeződött be, majd a szükségtelen hello SAS nem várt megújítani toobe módon lehet. Azonban ha ügyfél, amely rendszeresen lehetővé teszi a kérelmek SAS segítségével, majd lejárati hello lehetőségét előre szerepet. hello legfontosabb szempont, hogy toobalance hello kell a hello SAS toobe rövid élettartamú (korábban is hangsúlyoztuk), amely az ügyfél hello hello kell tooensure a korai kérelmezi a megújítási elég (tooavoid megszűnésének toohello SAS lejáró előzetes toosuccessful megújítási miatt).
5. **Ügyeljen arra, SAS kezdési időponttal.** Ha megadta hello kezdési ideje a SAS-kód túl**most**, majd miatt tooclock döntés (toodifferent gépek szerint aktuális idő eltérései), sikertelen lehet figyelni időnként hello az első néhány perc múlva. Általában beállítása hello kezdési idő toobe legalább 15 percet az elmúlt hello. Vagy, nem minden, ami megkönnyítő azonnal minden esetekben érvényes. Ugyanez általában vonatkozik tooexpiry idő is – hello ne feledje, hogy jelenhet meg mentése too15 percek száma, az óra eltérésére kérelmet az mindkét irányban. A többi verziója előzetes too2012-02-12 használó ügyfelek hello engedélyezett maximális időtartam, amely nem hivatkozik a tárolt házirend SAS esetén 1 óra, és adja meg a hosszú távú, sikertelen lesz, mint a szabályzatoknak.
6. **Meg kell adott hello erőforrás toobe érhető el.** Biztonsági szempontból ajánlott az tooprovide hello minimálisan szükséges jogosultsággal. Ha egy felhasználónak csak olvasási hozzáféréssel tooa egyetlen entitás van szüksége, adja meg számukra olvasási hozzáférés toothat egyetlen entitás, és nem olvasási/írási és törlési engedélyt tooall entitások. Azt is lehetővé teszi hello kárt csökkentheti, ha SAS-kód sérült, mert hello SAS kevesebb energiát egy támadó hello tagoknál.
7. **Ismerje meg, hogy a fiók lesz terhelve bármely használatra, beleértve, amely a SAS használatával történik.** Ha megadta az írási hozzáférést tooa blob, a felhasználó választhat tooupload 200 GB-os blob. Feljogosított őket olvasási hozzáféréssel is, ha lehetséges, hogy választ toodownload 10-szer nélül 2 TB, a kimenő forgalom költséggel meg. Ebben az esetben adja meg a korlátozott engedélyekkel toohelp hello lehetséges műveletek a rosszindulatú felhasználók mérsékelni. Rövid élettartamú SAS tooreduce használja a fenyegetést (de vegye figyelembe az óra eltérésére a hello befejezési idő).
8. **SAS használatával írt adatok ellenőrzése.** Amikor egy ügyfél-alkalmazás írja az adatokat tooyour tárfiók, tartsa szem előtt, hogy az adatok problémák lehetnek. Ha az alkalmazás megköveteli, hogy adatokat érvényesítve vagy ahhoz, hogy készen áll a toouse jogosult, végre kell hajtania az ellenőrzés hello adatok írása után, és az alkalmazás használata előtt. Ez az eljárás sérült vagy rosszindulatú adat íródik a tooyour fiók, a felhasználó, aki megfelelően beszerzett hello SAS, vagy egy felhasználó egy kiszivárgott SAS kihasználva is véd.
9. **SAS mindig ne használjon.** Egyes esetekben a tárfiók egy adott művelethez társított hello kockázatok járó SAS hello előnyeit. Az ilyen műveletek tooyour tárfiók írja az üzleti végrehajtása után középső rétegbeli szolgáltatás létrehozása érvényesítési, hitelesítési és naplózási szabály. Egyes esetekben sokkal egyszerűbb toomanage hozzáférési más módon. Például, ha azt szeretné toomake a tárolóban lévő összes BLOB nyilvánosan olvasható, hogy hello tároló nyilvános, ahelyett, hogy hozzáférést biztosító SAS tooevery ügyfél.
10. **Tárolási analitika toomonitor használhassa az alkalmazást.** Használhatja a naplózás és a metrikák tooobserve bármely lefoglalását tooan kívül az a SAS szolgáltató szolgáltatás, vagy toohello véletlen eltávolítása a tárolt házirend miatt a hitelesítés sikertelen. Lásd: hello [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) további információt.

## <a name="sas-examples"></a>SAS-példák
Az alábbiakban néhány példa a megosztott hozzáférési aláírásokkal, SAS fiók mindkét típusú, és a szolgáltatásalapú SAS.

toorun C# példában, a következő NuGet-csomagok a projekt tooreference hello szüksége:

* [Az Azure Storage ügyféloldali kódtára a .NET](http://www.nuget.org/packages/WindowsAzure.Storage), verziója 6.x vagy újabb (toouse fiók SAS).
* [Azure Configuration Manager](http://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager)

További példák hogyan toocreate és egy SAS-teszt: az [mintakódok Azure tárolási](https://azure.microsoft.com/documentation/samples/?service=storage).

### <a name="example-create-and-use-an-account-sas"></a>Példa: Létrehozása és SAS fiók használatára
hello alábbi példakód létrehoz egy fiókot, amely érvényes hello Blob fájl- és a SAS, és biztosítja az ügyfél engedélyeket olvasási, írási és listázási engedély hello tooaccess szolgáltatásiszint-API-k. hello fiók SAS hello protokoll tooHTTPS, korlátozza, így hello kérelem kell tenni a HTTPS.

```csharp
static string GetAccountSASToken()
{
    // toocreate hello account SAS, you need toouse your shared key credentials. Modify for your account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create a new access policy for hello account.
    SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
        {
            Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
            Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
            ResourceTypes = SharedAccessAccountResourceTypes.Service,
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Protocols = SharedAccessProtocol.HttpsOnly
        };

    // Return hello SAS token.
    return storageAccount.GetSharedAccessSignature(policy);
}
```

toouse hello fiók SAS tooaccess szolgáltatás hello Blob szolgáltatás szintű API-k, létrehozni egy Blob ügyfél objektumot hello SAS használatával és a tárfiók a Blob storage végpont hello.

```csharp
static void UseAccountSAS(string sasToken)
{
    // Create new storage credentials using hello SAS token.
    StorageCredentials accountSAS = new StorageCredentials(sasToken);
    // Use these credentials and hello account name toocreate a Blob service client.
    CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "account-name", endpointSuffix: null, useHttps: true);
    CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

    // Now set hello service properties for hello Blob client created with hello SAS.
    blobClientWithSAS.SetServiceProperties(new ServiceProperties()
    {
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        Logging = new LoggingProperties()
        {
            LoggingOperations = LoggingOperations.All,
            RetentionDays = 14,
            Version = "1.0"
        }
    });

    // hello permissions granted by hello account SAS also permit you tooretrieve service properties.
    ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
}
```

### <a name="example-create-a-stored-access-policy"></a>Példa: Tárolt hozzáférési házirend létrehozása
hello alábbi kód létrehoz egy tárolt házirend tárolóba. Hello hozzáférési házirend toospecify megkötéseket használ a szolgáltatásalapú SAS hello tárolóra vagy a benne található blobokat.

```csharp
private static async Task CreateSharedAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new shared access policy and define its constraints.
    // hello access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When hello start time for hello SAS is omitted, hello start time is assumed toobe hello time when hello storage service receives hello request.
        // Omitting hello start time for a SAS that is effective immediately helps tooavoid clock skew.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get hello container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add hello new policy toohello container's permissions, and set hello container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

### <a name="example-create-a-service-sas-on-a-container"></a>Példa: A szolgáltatásalapú SAS létrehozása a tárolóba
a következő kód hello létrehoz egy SAS-tárolóba. Ha egy meglévő tárolt hozzáférési házirend hello név van megadva, az adott házirendnek hello SAS társítva. Ha a nem tárolt házirend valósul meg, majd hello kód hello tároló az ad hoc biztonsági Társítások hoz létre.

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that hello SharedAccessBlobPolicy class is used both toodefine hello parameters of an ad-hoc SAS, and
        // tooconstruct a shared access policy that is saved toohello container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When hello start time for hello SAS is omitted, hello start time is assumed toobe hello time when hello storage service receives hello request.
            // Omitting hello start time for a SAS that is effective immediately helps tooavoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate hello shared access signature on hello container. In this case, all of hello constraints for the
        // shared access signature are specified on hello stored access policy, which is provided by name.
        // It is also possible toospecify some constraints on an ad-hoc SAS and others on hello stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return hello URI string for hello container, including hello SAS token.
    return container.Uri + sasContainerToken;
}
```

### <a name="example-create-a-service-sas-on-a-blob"></a>Példa: A szolgáltatásalapú SAS létrehozása a blob
a következő kód hello SAS blob hoz létre. Ha egy meglévő tárolt hozzáférési házirend hello név van megadva, az adott házirendnek hello SAS társítva. Ha a nem tárolt házirend valósul meg, majd hello kód hello blob egy ad hoc biztonsági Társítások hoz létre.

```csharp
private static string GetBlobSasUri(CloudBlobContainer container, string blobName, string policyName = null)
{
    string sasBlobToken;

    // Get a reference tooa blob within hello container.
    // Note that hello blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that hello SharedAccessBlobPolicy class is used both toodefine hello parameters of an ad-hoc SAS, and
        // tooconstruct a shared access policy that is saved toohello container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When hello start time for hello SAS is omitted, hello start time is assumed toobe hello time when hello storage service receives hello request.
            // Omitting hello start time for a SAS that is effective immediately helps tooavoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create
        };

        // Generate hello shared access signature on hello blob, setting hello constraints directly on hello signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate hello shared access signature on hello blob. In this case, all of hello constraints for the
        // shared access signature are specified on hello container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```

## <a name="conclusion"></a>Összegzés
Közös hozzáférésű jogosultságkód hasznosak korlátozott engedélyekkel tooyour tárolási fiók tooclients hello kulcsa nem lehet biztosítani. Ilyen fontos része a biztonsági modell hello Azure Storage használó alkalmazások. Ha az itt felsorolt hello ajánlott eljárást követi, használhatja SAS tooprovide nagyobb rugalmasságot kapott az access tooresources tárfiókba, az alkalmazás hello biztonság csökkenése nélkül.

## <a name="next-steps"></a>Következő lépések
* [Kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../blobs/storage-manage-access-to-resources.md)
* [Hozzáférés delegálása közös hozzáférésű jogosultságkód használatával](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Tábla- és várólista SAS bemutatása](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
