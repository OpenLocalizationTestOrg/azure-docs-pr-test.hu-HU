---
title: "A helyszíni a blob storage (Java) alkalmazás |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy konzolalkalmazást, amely feltölt egy képet az Azure-ba, majd a kép jelenik meg a böngészőben. Kódminták Java nyelven."
services: storage
documentationcenter: java
author: mmacy
manager: carmonm
editor: tysonn
ms.assetid: ccc9a7d7-6fe4-467b-b7fd-a73f17539e3f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/17/2016
ms.author: marsma
ms.openlocfilehash: a172b881fa38a69f4510df94f5797b7a56940c52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="on-premises-application-with-blob-storage"></a>A helyszíni alkalmazások az blob storage szolgáltatással
## <a name="overview"></a>Áttekintés
A következő példa bemutatja, hogyan használhatja az Azure storage tud lemezképeket menteni az Azure-ban. Ebben a cikkben a kód egy konzolalkalmazást, amely feltölt egy képet az Azure-ba, és létrehozza a kép jelenik meg a böngészőben HTML-fájlba van.

## <a name="prerequisites"></a>Előfeltételek
* A Java fejlesztői készlet (JDK), 1,6 vagy újabb verziója már telepítve van.
* Az Azure SDK telepítve van.
* A Javához készült Azure-tárak JAR, és bármely alkalmazandó függőségi JAR-fájlok kivételével, telepítve van, és a Java-fordítónak használják, a build elérési úton találhatók. Az Azure-könyvtárakban Java telepítésével kapcsolatos információkért lásd: [töltse le az Azure SDK for Java](../java-download-azure-sdk.md).
* Egy Azure storage-fiók be van állítva. A fiók nevét és a tárfiók kulcsára ebben a cikkben a kód használják. Lásd: [a Storage-fiók létrehozása](storage-create-storage-account.md#create-a-storage-account) a storage-fiók létrehozásával kapcsolatos információkat és [megtekintése és másolása tárelérési kulcsok](storage-create-storage-account.md#view-and-copy-storage-access-keys) a fiókkulcs beolvasásával kapcsolatos.
* Létrehozott nevű kép: helyi fájl elérési útja c: mentve\\myimages\\image1.jpg. Azt is megteheti, módosítsa a **FileInputStream** konstruktor a példa egy másik kép elérési útját és fájlnevét név.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Fájl feltöltése az Azure blob storage segítségével
Eljárás pontos lépéseit itt akkor jelenik meg. Ha azt szeretné, kihagyhatja azokat, amelyek, a teljes kód számára jelenik meg a cikk későbbi részében.

Kezdje az importálja az Azure core tárolási osztályok, az Azure blob ügyfél osztályok, a Java IO osztályok, beleértve a kódot és a **URISyntaxException** osztály.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

Egy osztályt deklarálható **StorageSample**, és adja meg a nyitó zárójel **{**.

```java
public class StorageSample {
```

Belül a **StorageSample** osztály, az alapértelmezett végpont protokoll, a tárfiók nevét és a tárelérési kulcsát, az Azure storage-fiókhoz megadott tartalmazó karakterlánc változót. Cserélje le a helyőrző értékeket **a\_fiók\_neve** és **a\_fiók\_kulcs** saját fiók nevét és a fiókkulcsot, kulcsattribútumokkal.

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

Adja hozzá a deklaráció a következő **fő**, közé tartozik egy **próbálja** letiltása, és tartalmazza a szükséges nyitott szögletes **{**.

```java
    public static void main(String[] args)
    {
        try
        {
```

(Azok használata ebben a példában a rendszer a leírása) a következő típusú változó deklarálható:

* **CloudStorageAccount**: inicializálni a számítógépfiók-objektum az az Azure storage-fiók nevét és a kulcs, és a blob ügyfél objektum létrehozásához használt.
* **CloudBlobClient**: a blob szolgáltatás eléréséhez használt.
* **CloudBlobContainer**: egy blob-tároló létrehozásához használt, a tárolóban lévő blobok listázása, és a tároló törlése.
* **CloudBlockBlob**: használt kép: helyi fájl feltöltése a tárolóba.

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

Adja meg az értéket a **fiók** változó.

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

Adja meg az értéket a **serviceClient** változó.

```java
serviceClient = account.createCloudBlobClient();
```

Adja meg az értéket a **tároló** változó. Azt is nevű tárolót mutató hivatkozás lesz **gettingstarted**.

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

A tároló létrehozása. Ezzel a módszerrel hoz létre a tárolót, ha még nem létezik (és visszatérési **igaz**). Ha a tároló létezik, akkor a rendszer visszaküldi **hamis**. Alternatív megoldás **createIfNotExists** van a **létrehozása** metódust (hibát adnak vissza, ha a tároló már létezik).

```java
container.createIfNotExists();
```

Állítsa be a névtelen hozzáférést ahhoz a tárolóhoz.

```java
// Set anonymous access on the container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

Lekérdezni egy jelöli az Azure storage blob blokkblob hivatkozását.

```java
blob = container.getBlockBlobReference("image1.jpg");
```

Használja a **fájl** konstruktort a helyileg létrehozott fájl, amely feltölti való hivatkozás. Győződjön meg arról, ez a fájl hozott létre a kód futtatása előtt.

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

Hívása a helyi fájl feltöltése a **CloudBlockBlob.upload** metódust. Az első paraméterben a **CloudBlockBlob.upload** metódus egy **FileInputStream** a helyi fájl, amely az Azure storage feltöltés képviselő objektum. A második paraméter a mérete, a fájl.

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

Nevű segítő függvény **MakeHTMLPage**, hogy egy egyszerű HTML-lapot tartalmazó egy  **&lt;kép&gt;**  elem a forrás beállítása az, amelyik most már az Azure Storage-blobba fiók. A kód **MakeHTMLPage** a cikk tárgyalja.

```java
MakeHTMLPage(container);
```

Nyomtassa ki a hibaállapot-üzeneteket és a létrehozott HTML-weblap információkat.

```java
System.out.println("Processing complete.");
System.out.println("Open index.html to see the images stored in your storage account.");
```

Zárja be a **próbálja** blokk egy záró zárójel beszúrásával: **}**

Kezeli a következő kivételekkel:

* **FileNotFoundException**: által okozott is lehet a **FileInputStream** vagy **FileOutputStream** konstruktor.
* **StorageException**: az Azure storage ügyfélkódtár által is okozhat.
* **URISyntaxException**: által okozott is lehet a **ListBlobItem.getUri** metódust.
* **Kivétel**: Általános kivétel kezelése.

<!-- -->

```java
catch (FileNotFoundException fileNotFoundException)
{
    System.out.print("FileNotFoundException encountered: ");
    System.out.println(fileNotFoundException.getMessage());
    System.exit(-1);
}
catch (StorageException storageException)
{
    System.out.print("StorageException encountered: ");
    System.out.println(storageException.getMessage());
    System.exit(-1);
}
catch (URISyntaxException uriSyntaxException)
{
    System.out.print("URISyntaxException encountered: ");
    System.out.println(uriSyntaxException.getMessage());
    System.exit(-1);
}
catch (Exception e)
{
    System.out.print("Exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Zárja be az **fő** záró zárójel beszúrásával: **}**

Nevű metódus létrehozása **MakeHTMLPage** létrehozni egy egyszerű HTML-lapot. Ez a módszer típusú paraméterrel rendelkezik **CloudBlobContainer**, amely használható a feltöltött blobon listáját iterációt. Ez a módszer kivételhibát típusú kivételek **FileNotFoundException**, által is okozhat a **FileOutputStream** konstruktor, és **URISyntaxException**, amely lehet által okozott a **ListBlobItem.getUri** metódust. A nyitó zárójel tartalmaznak **{**.

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

Hozzon létre egy helyi fájlt **index.html**.

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

Hozzáadása a helyi fájlba írni a  **&lt;html&gt;**,  **&lt;fejléc&gt;**, és  **&lt;törzs&gt;**  elemek.

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

A feltöltött blobon listáját iterációt. Minden egyes blob, a HTML-weblap hozzon létre egy  **&lt;img&gt;**  elem, amelynek a **src** , mert az az Azure storage-fiók létezik a BLOB URI küldött attribútum.
Bár csak egy kép ebben a mintában hozzáadni, ha több hozzáadott, ezt a kódot az összes kellene többször.

Az egyszerűség kedvéért ez a példa feltételezi minden feltöltött blobon lemezkép. Ha eltérő képek blobok vagy lapblobokat helyett blokkblobokat frissítette, szükség szerint állítsa be a kódot.

```java
// Enumerate the uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in the HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

Zárja be a  **&lt;törzs&gt;**  elem és a  **&lt;html&gt;**  elemet.

```java
stream.println("</body>");
stream.println("</html>");
```

Zárja be a helyi fájlt.

```java
stream.close();
```

Zárja be az **MakeHTMLPage** záró zárójel beszúrásával: **}**

Zárja be az **StorageSample** záró zárójel beszúrásával: **}**

A teljes kód látható az ebben a példában a következő: Ne felejtse el módosítani a helyőrző értékeket **a\_fiók\_neve** és **a\_fiók\_kulcs** a fiók nevére és kulcsára, kulcsattribútumokkal.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;

// Create an image, c:\myimages\image1.jpg, prior to running this sample.
// Alternatively, change the value used by the FileInputStream constructor
// to use a different image path and file that you have already created.
public class StorageSample {

    public static final String storageConnectionString =
            "DefaultEndpointsProtocol=http;" +
                    "AccountName=your_account_name;" +
                    "AccountKey=your_account_name";

    public static void main(String[] args) {
        try {
            CloudStorageAccount account;
            CloudBlobClient serviceClient;
            CloudBlobContainer container;
            CloudBlockBlob blob;

            account = CloudStorageAccount.parse(storageConnectionString);
            serviceClient = account.createCloudBlobClient();
            // Container name must be lower case.
            container = serviceClient.getContainerReference("gettingstarted");
            container.createIfNotExists();

            // Set anonymous access on the container.
            BlobContainerPermissions containerPermissions;
            containerPermissions = new BlobContainerPermissions();
            containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
            container.uploadPermissions(containerPermissions);

            // Upload an image file.
            blob = container.getBlockBlobReference("image1.jpg");

            File fileReference = new File("c:\\myimages\\image1.jpg");
            blob.upload(new FileInputStream(fileReference), fileReference.length());

            // At this point the image is uploaded.
            // Next, create an HTML page that lists all of the uploaded images.
            MakeHTMLPage(container);

            System.out.println("Processing complete.");
            System.out.println("Open index.html to see the images stored in your storage account.");

        } catch (FileNotFoundException fileNotFoundException) {
            System.out.print("FileNotFoundException encountered: ");
            System.out.println(fileNotFoundException.getMessage());
            System.exit(-1);
        } catch (StorageException storageException) {
            System.out.print("StorageException encountered: ");
            System.out.println(storageException.getMessage());
            System.exit(-1);
        } catch (URISyntaxException uriSyntaxException) {
            System.out.print("URISyntaxException encountered: ");
            System.out.println(uriSyntaxException.getMessage());
            System.exit(-1);
        } catch (Exception e) {
            System.out.print("Exception encountered: ");
            System.out.println(e.getMessage());
            System.exit(-1);
        }
    }

    // Create an HTML page that can be used to display the uploaded images.
    // This example assumes all of the blobs are for images.
    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {
        PrintStream stream;
        stream = new PrintStream(new FileOutputStream("index.html"));

        // Create the opening <html>, <header>, and <body> elements.
        stream.println("<html>");
        stream.println("<header/>");
        stream.println("<body>");

        // Enumerate the uploaded blobs.
        for (ListBlobItem blobItem : container.listBlobs()) {
            // List each blob as an <img> element in the HTML body.
            stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
        }

        stream.println("</body>");

        // Complete the <html> element and close the file.
        stream.println("</html>");
        stream.close();
    }
}
```

Mellett a kép: helyi fájl feltöltése az Azure storage, a példakód létrehoz egy helyi fájl namedindex.html, a feltöltött kép megjelenítéséhez böngészőben megnyithat.

A kódot tartalmaz, a fiók nevét és a fiókkulcsot, győződjön meg arról, hogy a forráskód biztonságos.

## <a name="to-delete-a-container"></a>A tároló törlése
Van szó, a tárolási, mert előfordulhat, hogy törölni kívánja a **gettingstarted** tároló, miután befejezte az ebben a példában ki. Egy tároló törléséhez használja a **CloudBlobContainer.delete** metódust.

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

Hívása a **CloudBlobContainer.delete** metódus, a folyamat inicializálása **CloudStorageAccount**, **ClodBlobClient**, és **CloudBlobContainer**  objektumok megegyezik a látható módon a **createIfNotExist** metódust. Az alábbiakban egy teljes példa, amely törli a tárolót egy **gettingstarted**.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

public class DeleteContainer {

    public static final String storageConnectionString =
            "DefaultEndpointsProtocol=http;" +
                "AccountName=your_account_name;" +
                "AccountKey=your_account_key";

    public static void main(String[] args)
    {
        try
        {
            CloudStorageAccount account;
            CloudBlobClient serviceClient;
            CloudBlobContainer container;

            account = CloudStorageAccount.parse(storageConnectionString);
            serviceClient = account.createCloudBlobClient();
            // Container name must be lower case.
            container = serviceClient.getContainerReference("gettingstarted");
            container.delete();

            System.out.println("Container deleted.");

        }
        catch (StorageException storageException)
        {
            System.out.print("StorageException encountered: ");
            System.out.println(storageException.getMessage());
            System.exit(-1);
        }
        catch (Exception e)
        {
            System.out.print("Exception encountered: ");
            System.out.println(e.getMessage());
            System.exit(-1);
        }
    }
}
```

Más blob tárolási osztályokhoz és módszerek áttekintését lásd: [Blob storage-ának Java használatával](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Következő lépések
Az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről.

* [Az Azure Storage Java SDK](https://github.com/azure/azure-storage-java)
* [Az Azure Storage ügyfél SDK-dokumentáció](http://dl.windowsazure.com/storage/javadoc/)
* [Az Azure Storage-szolgáltatások REST API-ja](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Az Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)

