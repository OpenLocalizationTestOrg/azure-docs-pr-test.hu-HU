---
title: aaaImport az adatok tooAnalytics Azure Application insightsban |} Microsoft Docs
description: "Az alkalmazás telemetriai statikus adatok toojoin importálni, vagy importáljon egy külön adatok adatfolyam tooquery az Analytics."
services: application-insights
keywords: "Nyissa meg a séma, az adatok importálása"
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bwren
ms.openlocfilehash: 7a3a3c9155adc1885dd366ddb13dda80bb894adb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-into-analytics"></a>Adatok importálása elemzés

Az adatok importálása [Analytics](app-insights-analytics.md), vagy toojoin együtt [Application Insights](app-insights-overview.md) telemetriai az alkalmazásból, vagy úgy, hogy egy külön adatfolyamként elemzése. Elemzés egy hatékony lekérdezési nyelv jól alkalmazható tooanalyzing telemetriai a nagy mennyiségű időbélyegzővel adatfolyamokat.

Adatok importálhatja a Analytics saját sémáját használja. Toouse hello szabványos Application Insights sémák például kérelem vagy a nyomkövetés nem tartalmaz.

Importálhatja, JSON vagy Adatforrásnézet (elválasztó tagolt - vesszővel, pontosvesszővel válassza el vagy lapon) fájlok.

Nincsenek három olyan helyzetekben, ahol tooAnalytics importálása hasznos:

* **Alkalmazás telemetriai csatlakozik.** Például a táblázat, amely leképezhető URL-címeket a webhely toomore olvasható lap címének sikerült importálni. Elemzés létrehozhat egy irányítópult diagram jelentés hello tíz legnépszerűbb lap a webhelyen. Most megmutatja hello lap címek hello URL-címei helyett.
* **A telemetriai összefüggéseket** más adatforrások, például a hálózati forgalom, a kiszolgáló adatait, vagy a CDN-naplófájljai.
* **Elemzés tooa külön adatfolyam alkalmazni.** Application Insights Analytics egy olyan erőteljes eszköz, amely ritka, időbélyegzővel adatfolyamok - SQL-re mint javulás megfelelően működik. Ha ilyen adatfolyam más forrásból, elemezheti az Analytics.

Küldő tooyour adatforrás is könnyen. 

1. (Egy alkalommal) Az adatok "adatforrás" hello séma határozza meg.
2. (Rendszeres) Töltse fel az adattárolás tooAzure, és hívja hello REST API toonotify, amely új adatok adatfeldolgozást vár. Néhány percen belül hello adatok érhető el elemzés lekérdezést.

hello hello feltöltés gyakoriságát határozza meg, és hogy milyen gyorsan kívánja, hogy az adatok toobe lekérdezhető. Nagyobb méretű adattömböket, de nem lehet nagyobb, mint 1 GB-os hatékonyabb tooupload adatok is.

> [!NOTE]
> *Nagy mennyiségű adatok források tooanalyze kapott?* [*Érdemes lehet* logstash *tooship az Application Insights adatait.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Előkészületek

A következők szükségesek:

1. Az Application Insights-erőforrás a Microsoft Azure-ban.

 * Ha azt szeretné, tooanalyze elkülönítve egyéb telemetriai adatait [hozzon létre egy új Application Insights-erőforrást](app-insights-create-new-resource.md).
 * Ha való csatlakozás vagy az alkalmazás már be van állítva az Application insights szolgáltatással telemetriai adatok összehasonlítása, majd használhatja hello erőforrást az alkalmazáshoz.
 * Közreműködő vagy tulajdonosi hozzáférés toothat erőforrás.
 
2. Az Azure storage. TooAzure storage-be feltölteni, és elemzés lekérdezi az adatok ott. 

 * Javasoljuk, hogy hozzon létre egy dedikált tárfiókot, a blobok. Ha a blobok más folyamatokkal van megosztva, hosszabb ideig tart a folyamatok tooread a blobok.


## <a name="define-your-schema"></a>A séma meghatározása

Adatok importálása előtt meg kell adnia egy *adatforrás,* amely hello séma adatait adja meg.
Adatforrások too50 egyetlen Application Insights-erőforrás lehet

1. Hello adatforrás varázsló elindításához. Használja az "Új adatforrás hozzáadása" gombra. Másik lehetőségként - kattintson a jobb felső sarkában található beállítások gombra, és kattintson "Az adatforrások" legördülő menüre.

    ![Új adatforrás hozzáadása](./media/app-insights-analytics-import/add-new-data-source.png)

    Adja meg az új adatforrás nevét.

2. Adja meg, hogy fel kell töltenie hello fájlok formátuma.

    Manuálisan állítsa hello formátumot, vagy a minta-fájl feltöltése.

    Ha hello adatok CSV formátumban, hello első sorának hello minta lehet oszlopfejléceket. Hello mező nevét a következő lépésben hello módosíthatja.

    hello minta tartalmaznia kell legalább 10 sorok vagy adatokat rögzíti.

    Oszlop vagy mező neve alfanumerikus nevek (nélkül szóköz vagy írásjel) kell rendelkeznie.

    ![A minta-fájl feltöltése](./media/app-insights-analytics-import/sample-data-file.png)


3. Felülvizsgálati hello séma, amely varázsló hello kapott rendelkezik. Ha következtetni hello típusok mintaként, esetleg újra kell tooadjust következtetni hello típusú hello oszlopok.

    ![Tekintse át a hello következtetni séma](./media/app-insights-analytics-import/data-source-review-schema.png)

 * (Választható.) Töltse fel a sémadefiníciót. Tekintse meg az alábbi hello formátumban.

 * Válassza ki az időbélyegző. Minden adat Analytics kell rendelkezhetnek időbélyegmezővel. Típusúnak kell lennie `datetime`, de nincs beállítva a "időbélyeg" nevű toobe. Ha az adatok egy dátum és idő ISO formátumú tartalmazó oszlop, válassza ezt a hello Timestamp típusú oszlop. Ellenkező esetben válassza a "Data a Beérkezett", és hello az importálási folyamat felveszi időbélyegmezővel.

5. Hello adatforrás létrehozása.

### <a name="schema-definition-file-format"></a>Séma csomagdefiníciós fájlformátumról

A felhasználói felületen hello séma szerkesztése, helyett betöltése hello sémadefiníciót fájlból. hello schema definíció formátum a következőképpen történik: 

Tagolt formátumú 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

JSON formátumban 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
Egyes oszlopok azonosíthatók hello helyét, nevét és típusát. 

* Hely – tagolt fájl Format azt leképezve hello érték hello helyének. A JSON formátum esetén hello jpath leképezett hello kulcs.
* Név – hello hello oszlop neve jelenik meg.
* Írja be – hello adatok írja be annak az oszlopnak.
 
Abban az esetben a mintaadatok lett megadva, és fájlformátumot csak, a hello sémadefiníciót kell az összes oszlop leképezése, és adja hozzá az új oszlopok hello végén. 

JSON lehetővé teszi, hogy a részleges leképezést hello adatok, ezért hello sémadefiníciót JSON formátum nem rendelkezik toomap minden kulcsot, amely megtalálható a mintaadatokat. Az oszlopokat, amelyek nem részei hello mintaadatok is leképezheti. 

## <a name="import-data"></a>Adatok importálása

tooimport adatok tooAzure tárolási töltse fel, az access-kulcs létrehozása, és hajtsa végre az egy REST API-hívás.

![Új adatforrás hozzáadása](./media/app-insights-analytics-import/analytics-upload-process.png)

Hajtsa végre a folyamatot követve manuálisan hello, vagy állítsa be az automatikus rendszer-toodo, rendszeres időközönként. Meg kell toofollow ezeket a lépéseket az egyes adatblokk kívánt tooimport.

1. Fájlok feltöltése hello túl[Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md). 

 * Blobok tömörítetlen too1GB fel tetszőleges méretű lehet. Nagyméretű bináris objektumok MB több száz alkalmasak a teljesítmény szempontjából.
 * A Gzip tooimprove ideje és késés mellett hello adatok toobe lekérdezhetők a tömöríthetők. Használjon hello `.gz` fájlnév-kiterjesztés.
 * Ajánlott toouse külön tárfiókot erre a célra, tooavoid hívások a különböző szolgáltatások lelassulnak, ami a teljesítményt.
 * Adatküldés nagy gyakorisággal, minden néhány másodpercben esetén ajánlott toouse több mint egy tárfiókot, a teljesítmény szempontjából.

 
2. [Hozzon létre egy közös hozzáférésű Jogosultságkód kulcsot hello BLOB](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). hello kulcsot kell egy olyan lejárati időt egy nap van, és olvasási hozzáférést biztosítanak.
3. Ellenőrizze egy REST-hívást toonotify adatok váró Application Insights.

 * Végpont:`https://dc.services.visualstudio.com/v2/track`
 * HTTP-metódus: POST
 * Hasznos:

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

hello helyőrzői:

* `Blob URI with Shared Access Key`: Kap a hello eljárás a kulcs létrehozásához. Adott toohello blob.
* `Schema ID`: a megadott séma létrehozott séma azonosító hello. a blob hello adatokat meg kell felelniük toohello séma.
* `DateTime`: mely hello: küldés, hello ideje (UTC). Ezek a formátumok fogadunk: ISO8601 (például "2016-01-01 13:45:01"); RFC822 ("Sze, 14 Dec 16 14:57:01 + 0000"); RFC850 ("szerda, 14-Dec-16 14:57:00 UTC"); RFC1123 ("Sze, 14 Dec 2016 14:57:00 + 0000").
* `Instrumentation key`az Application Insights-erőforrás.

hello érhetők el az adatok Analytics néhány perc múlva.

## <a name="error-responses"></a>Hibaüzenetek

* **400 Hibás kérelem**: azt jelzi, hogy hello-kérések forgalma érvénytelen. Részletek:
 * Megfelelő instrumentation kulcs.
 * Érvényes időérték. Hello ideje most UTC Formátumban kell lennie.
 * JSON hello esemény megfelel toohello séma.
* **403 Tiltott**: elküldött hello blob nem érhető el. Győződjön meg arról, hogy hello megosztott elérési kulcsával érvényes, és nem járt le.
* **Nem található 404**:
 * hello blob nem létezik.
 * hello sourceId nem megfelelő.

További részletes információk a hello válasz hibaüzenet érhető el.


## <a name="sample-code"></a>Mintakód

Ezt a kódot használja hello [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) NuGet-csomagot.

### <a name="classes"></a>Osztályok

```C#
namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 
```

### <a name="ingest-data"></a>Adatok kigyűjtése

Ez a kód használható minden egyes blob. 

```C#
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a>Következő lépések

* [Log Analytics lekérdezési nyelv hello bemutatása](app-insights-analytics-tour.md)
* [Használjon *Logstash* toosend adatok tooApplication Insights](https://github.com/Microsoft/logstash-output-application-insights)
