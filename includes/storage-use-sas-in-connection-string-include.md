<span data-ttu-id="b79bb-101">Ha egy közös hozzáférésű jogosultságkód (SAS) URL-címet, amely hozzáférést biztosít a tárfiókban lévő tooresources kell rendelkeznie, használhatja hello SAS a kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="b79bb-101">If you possess a shared access signature (SAS) URL that grants you access tooresources in a storage account, you can use hello SAS in a connection string.</span></span> <span data-ttu-id="b79bb-102">Hello SAS hello információ szükséges tooauthenticate hello kérelmet tartalmaz, mert a SAS-kód a kapcsolati karakterlánc hello protokoll, hello szolgáltatás végpontjának és hello szükséges hitelesítő adatokat tooaccess hello erőforrás biztosít.</span><span class="sxs-lookup"><span data-stu-id="b79bb-102">Because hello SAS contains hello information required tooauthenticate hello request, a connection string with a SAS provides hello protocol, hello service endpoint, and hello necessary credentials tooaccess hello resource.</span></span>

<span data-ttu-id="b79bb-103">egy kapcsolati karakterláncot, amely tartalmazza a közös hozzáférésű jogosultságkód toocreate hello karakterlánc hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="b79bb-103">toocreate a connection string that includes a shared access signature, specify hello string in hello following format:</span></span>

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

<span data-ttu-id="b79bb-104">Minden szolgáltatási végpont nem kötelező, de hello kapcsolati karakterláncnak tartalmaznia kell legalább egy.</span><span class="sxs-lookup"><span data-stu-id="b79bb-104">Each service endpoint is optional, although hello connection string must contain at least one.</span></span>

> [!NOTE]
> <span data-ttu-id="b79bb-105">Ajánlott eljárásként ajánlott a HTTPS használata a SAS-kód.</span><span class="sxs-lookup"><span data-stu-id="b79bb-105">Using HTTPS with a SAS is recommended as a best practice.</span></span>
>
> <span data-ttu-id="b79bb-106">SAS-kód megadása esetén a kapcsolati karakterláncot egy konfigurációs fájlban, szükség lehet a speciális karakterek tooencode hello URL-címben.</span><span class="sxs-lookup"><span data-stu-id="b79bb-106">If you are specifying a SAS in a connection string in a configuration file, you may need tooencode special characters in hello URL.</span></span>
>
>

### <a name="service-sas-example"></a><span data-ttu-id="b79bb-107">Szolgáltatás SAS – példa</span><span class="sxs-lookup"><span data-stu-id="b79bb-107">Service SAS example</span></span>
<span data-ttu-id="b79bb-108">Íme egy példa egy kapcsolati karakterláncot, amely tartalmazza a szolgáltatásalapú SAS Blob Storage:</span><span class="sxs-lookup"><span data-stu-id="b79bb-108">Here's an example of a connection string that includes a service SAS for Blob storage:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

<span data-ttu-id="b79bb-109">Íme egy példa hello és speciális karakterek kódolással ugyanazt a kapcsolati karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="b79bb-109">And here's an example of hello same connection string with encoding of special characters:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a><span data-ttu-id="b79bb-110">Fiók SAS – példa</span><span class="sxs-lookup"><span data-stu-id="b79bb-110">Account SAS example</span></span>
<span data-ttu-id="b79bb-111">Íme egy példa egy kapcsolati karakterláncot, amely tartalmazza az SAS-Blob és a fájl tárolási fiók.</span><span class="sxs-lookup"><span data-stu-id="b79bb-111">Here's an example of a connection string that includes an account SAS for Blob and File storage.</span></span> <span data-ttu-id="b79bb-112">Vegye figyelembe, hogy mindkét szolgáltatás végpontjai vannak megadva:</span><span class="sxs-lookup"><span data-stu-id="b79bb-112">Note that endpoints for both services are specified:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

<span data-ttu-id="b79bb-113">Íme egy példa hello és ugyanazt a kapcsolati karakterláncot az URL-kódolást:</span><span class="sxs-lookup"><span data-stu-id="b79bb-113">And here's an example of hello same connection string with URL encoding:</span></span>

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

