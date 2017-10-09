## <a name="configure-your-application-tooaccess-azure-storage"></a>Az alkalmazás tooaccess Azure Storage konfigurálása
Nincsenek két módon tooauthenticate az alkalmazás tooaccess tárolási szolgáltatások:

* Megosztott kulcs: Megosztott kulcs használata csak tesztelési célokra
* Közös hozzáférésű Jogosultságkód (SAS): SAS használja az éles környezetben

### <a name="shared-key"></a>Megosztott kulcsos
Megosztott kulcsos hitelesítést azt jelenti, hogy az alkalmazás fogja használni a fióknév fiók kulcs tooaccess tárolási szolgáltatások. Gyorsan megjelenítő hello céljából hogyan toouse ebben a könyvtárban fogjuk használni az első lépések megosztott kulcsos hitelesítést.

> [!WARNING] 
> **Csak tesztelési célra használja a megosztott kulcsos hitelesítést!** A fiók nevét és a fiókkulcsot, amelyek teljes olvasási/írási hozzáférést toohello kapcsolódó tárfiók, lesz elosztott tooevery személy, amely letölti az alkalmazást. Ez az **nem** jó gyakorlat az, akkor kockázatát, hogy a kulcsot ne legyenek az ügyfelek nem megbízható.
> 
> 

Ha megosztott kulcsos hitelesítést használ, létrehozhat egy [kapcsolati karakterlánc](../articles/storage/common/storage-configure-connection-string.md). hello kapcsolati karakterlánc áll:  

* Hello **megadni: DefaultEndpointsProtocol** -dönthet úgy, HTTP vagy HTTPS PROTOKOLLT. HTTPS-kapcsolaton keresztül azonban erősen ajánlott.
* Hello **fióknév** – hello a tárfiók neve
* Hello **Fiókkulcs** - hello a [Azure Portal](https://portal.azure.com), keresse meg a tooyour tárfiók, és kattintson a hello **kulcsok** ikon toofind ezt az információt.
* (Választható) **EndpointSuffix** -régiók másik végpont utótagok, például az Azure China vagy Azure irányítás tárolószolgáltatásokhoz szolgál.

Íme egy példa megosztott kulcsos hitelesítést használó kapcsolódási karakterlánc:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Közös hozzáférésű jogosultságkódok (SAS)
A mobilalkalmazás módszer kérelmet hitelesítésére hello Azure Storage szolgáltatás elleni ügyfél által ajánlott hello van egy közös hozzáférésű Jogosultságkód (SAS) használatával. SAS lehetővé teszi egy ügyfél-hozzáférési tooa erőforrás toogrant a megadott időn, meghatározott engedélyekkel vannak beállítva.
Hello tárfiók-tulajdonos, mint a mobil ügyfelek tooconsume toogenerate SAS lesz szüksége. toogenerate hello SAS, érdemes toowrite hello SAS elosztott toobe tooyour ügyfelek hoz létre különálló szolgáltatásként. Tesztelési célokra használható hello [Microsoft Azure Tártallózó](http://storageexplorer.com) vagy hello [Azure Portal](https://portal.azure.com) toogenerate SAS-kód. Hello SAS létrehozásakor megadhatja a hello időtartam, mely hello keresztül SAS érvénytelen, és a SAS biztosít toohello ügyfél hello hello engedélyeket.

hello a következő példa bemutatja, hogyan toouse hello Microsoft Azure Tártallózó toogenerate SAS-kód.

1. Ha még nem tette, [telepítés hello Microsoft Azure Tártallózó](http://storageexplorer.com)
2. Csatlakozás tooyour előfizetés.
3. Kattintson a tárfiók, majd kattintson a "Hello"műveletek"lapon hello alsó, bal oldali. A biztonsági Társítások kattintson a "Get közös hozzáférésű Jogosultságkód" toogenerate "kapcsolati karakterlánc".
4. Íme egy példa, hogy biztosít olvasási és írási engedélyek hello szolgáltatást, a tároló és a objektum szintjén hello blob szolgáltatás hello tárfiók SAS-kapcsolati karakterláncot.
   
   `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Ahogy látja, amikor a SAS használatával, akkor nem teszi ki a fiókkulcs az alkalmazásban. További biztonsági Társítások és gyakorlati tanácsok a SAS használatával kiveszi [megosztott hozzáférési aláírásokkal: Understanding hello SAS-modell](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md).

