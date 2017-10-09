### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás
Hello **Azure Storage társított szolgáltatásnak** lehetővé teszi a hello toolink egy Azure storage fiók tooan az Azure data factory **fiókkulcs**, amely hello adat-előállító biztosítja a globális hozzáférési toohello Azure Tárolás. a következő táblázat hello biztosít JSON-elemek adott tooAzure tárolás társított szolgáltatásának leírását.

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type |hello type tulajdonságot kell beállítani: **AzureStorage** |Igen |
| connectionString |Adjon meg információt tooconnect tooAzure tárolási hello connectionString tulajdonság szükséges. |Igen |

Tekintse meg a következő lépéseket tooview vagy másolási hello-fiókjához tartozó cikket az Azure tárolás hello: [nézet, a másolás és a hívóbetűk újragenerálása tárolási](../articles/storage/common/storage-create-storage-account.md#manage-your-storage-account).

**Példa**  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

### <a name="azure-storage-sas-linked-service"></a>Az Azure Storage Sas társított szolgáltatás
A közös hozzáférésű jogosultságkód (SAS) delegált hozzáférést tooresources a tárfiókban lévő biztosít. Lehetővé teszi egy ügyfél toogrant csak korlátozott engedélyekkel tooobjects a tárfiókban lévő egy megadott időszakban, és engedélyeket, megadott számú anélkül, hogy tooshare a tárelérési kulcsok. hello SAS URI, amely a lekérdezési paraméterek magában foglalja a hitelesített hozzáférést tooa tárolási erőforrás szükséges minden hello információt. tárolási erőforrások tooaccess hello SAS, hello ügyfélnek csak kell toopass hello SAS toohello megfelelő konstruktort vagy metódust. Részletes információ a SAS: [megosztott hozzáférési aláírásokkal: Understanding hello SAS-modell](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Az Azure Data Factory most csak támogatja **szolgáltatás SAS** , de nem fiók SAS. Lásd: [típusok a megosztott hozzáférési aláírásokkal](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) kétféle típusú vonatkozó további információért és hogyan tooconstruct. Megjegyzés: az SAS URL-CÍMÉT az Azure portálról generable hello, vagy a Tártallózó egy fiók SAS, ami nem támogatott.
> 

hello Azure Storage SAS kapcsolódó szolgáltatás lehetővé teszi egy Azure Storage-fiók tooan az Azure data factory toolink egy közös hozzáférésű Jogosultságkód (SAS) használatával. Ez hozzáférést biztosít a hello adat-előállító korlátozott/időhöz kötött tooall/specifikus erőforrások (blobtárolóban /) hello tárolóban. a következő táblázat hello biztosít JSON elemek adott tooAzure kapcsolódó tároló SAS szolgáltatás leírását. 

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type |hello type tulajdonságot kell beállítani: **AzureStorageSas** |Igen |
| sasUri |Adja meg a megosztott hozzáférési aláírást URI toohello Azure tárolási erőforrások, például blob, a tároló vagy a tábla.  |Igen |

**Példa**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<Specify SAS URI of hello Azure Storage resource>"   
        }  
    }  
}  
```

Létrehozásakor egy **SAS URI**, figyelembe véve a következők hello:  

* Állítsa be a megfelelő olvasási/írási **engedélyek** alapján objektumok hogyan hello társított szolgáltatás (olvasási, írási, olvasás/írás) a data factory van használatban.
* Állítsa be **lejárati idejének** megfelelően. Győződjön meg arról, hogy hello hozzáférés tooAzure tárolási objektum már időkorlátozás nélkül hello hello adatcsatorna aktív időszakát.
* URI kell létrehozni: hello megfelelő tárolót vagy blobot, vagy táblázatok szintjén hello alapján kell. Egy Azure blob SAS Uri tooan lehetővé teszi, hogy hello adat-előállító szolgáltatás tooaccess, hogy a blob. A SAS Uri tooan Azure blob-tároló lehetővé teszi, hogy a hello adat-előállító szolgáltatás tooiterate adott tárolóban lévő blobok keresztül. Ha szüksége van tooprovide több/kevesebb objektumok később, vagy frissítés hello SAS URI, ne feledje tooupdate kapcsolódó hello szolgáltatást, amely hello új URI.   

