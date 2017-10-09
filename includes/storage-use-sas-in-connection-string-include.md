Ha egy közös hozzáférésű jogosultságkód (SAS) URL-címet, amely hozzáférést biztosít a tárfiókban lévő tooresources kell rendelkeznie, használhatja hello SAS a kapcsolati karakterláncban. Hello SAS hello információ szükséges tooauthenticate hello kérelmet tartalmaz, mert a SAS-kód a kapcsolati karakterlánc hello protokoll, hello szolgáltatás végpontjának és hello szükséges hitelesítő adatokat tooaccess hello erőforrás biztosít.

egy kapcsolati karakterláncot, amely tartalmazza a közös hozzáférésű jogosultságkód toocreate hello karakterlánc hello a következő formátumban kell megadni:

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

Minden szolgáltatási végpont nem kötelező, de hello kapcsolati karakterláncnak tartalmaznia kell legalább egy.

> [!NOTE]
> Ajánlott eljárásként ajánlott a HTTPS használata a SAS-kód.
>
> SAS-kód megadása esetén a kapcsolati karakterláncot egy konfigurációs fájlban, szükség lehet a speciális karakterek tooencode hello URL-címben.
>
>

### <a name="service-sas-example"></a>Szolgáltatás SAS – példa
Íme egy példa egy kapcsolati karakterláncot, amely tartalmazza a szolgáltatásalapú SAS Blob Storage:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

Íme egy példa hello és speciális karakterek kódolással ugyanazt a kapcsolati karakterláncot:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a>Fiók SAS – példa
Íme egy példa egy kapcsolati karakterláncot, amely tartalmazza az SAS-Blob és a fájl tárolási fiók. Vegye figyelembe, hogy mindkét szolgáltatás végpontjai vannak megadva:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

Íme egy példa hello és ugyanazt a kapcsolati karakterláncot az URL-kódolást:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

