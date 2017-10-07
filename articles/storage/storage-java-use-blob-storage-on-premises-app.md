---
title: "a blob storage (Java) alkalmazás aaaOn helyszíni |} Microsoft Docs"
description: "Ismerje meg, hogyan hello az toocreate egy konzolalkalmazást, amely feltölt egy képet tooAzure, majd megjeleníti a lemezképet a böngészőben. Kódminták Java nyelven."
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
ms.openlocfilehash: ed8eb4c1045691c25abe94bf6c1b18b797adc3e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="on-premises-application-with-blob-storage"></a>A helyszíni alkalmazások az blob storage szolgáltatással
## <a name="overview"></a>Áttekintés
hello alábbi példa bemutatja, hogyan használhatja az Azure storage tud lemezképeket menteni az Azure-ban. Ebben a cikkben hello kód van egy konzolalkalmazást, amely feltölt egy képet tooAzure, és létrehozza a HTML-fájlba hello kép megjelenítése a böngészőben.

## <a name="prerequisites"></a>Előfeltételek
* A Java fejlesztői készlet (JDK), 1,6 vagy újabb verziója már telepítve van.
* hello Azure SDK telepítve van.
* hello JAR a Java hello Azure-könyvtárakban, és bármely alkalmazandó függőségi JAR-fájlok kivételével, telepítve van, és a Java-fordítónak használják a hello build elérési úton találhatók. Java hello Azure-könyvtárakban telepítésével kapcsolatos információkért lásd: [letöltési hello Azure SDK for Java](../java-download-azure-sdk.md).
* Egy Azure storage-fiók be van állítva. hello fióknevet és a fiókkulcsot hello tárfiók által használandó hello kód ebben a cikkben. Lásd: [hogyan tooCreate Tárfiók](storage-create-storage-account.md#create-a-storage-account) a storage-fiók létrehozásával kapcsolatos információkat és [megtekintése és másolása tárelérési kulcsok](storage-create-storage-account.md#view-and-copy-storage-access-keys) hello fiókkulcs beolvasásával kapcsolatos.
* Létrehozott nevű kép: helyi fájl helye: hello elérési c:\\myimages\\image1.jpg. Azt is megteheti, módosítsa a **FileInputStream** konstruktor a hello példa toouse különböző kép elérési útját és nevét.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="toouse-azure-blob-storage-tooupload-a-file"></a>toouse Azure blob storage tooupload egy fájlt
Eljárás pontos lépéseit itt akkor jelenik meg. Ha szeretné azokat, amelyek tooskip, hello teljes kód számára jelenik meg a cikk későbbi részében.

Kezdje az hello kód hello Azure core tárolási osztályok, hello Azure blob ügyfél osztályok, hello Java IO osztályok és hello irányuló beleértve **URISyntaxException** osztály.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

Egy osztályt deklarálható **StorageSample**, és adja meg a nyitó zárójel hello, **{**.

```java
public class StorageSample {
```

Hello belül **StorageSample** osztály, fogja tartalmazni, hello alapértelmezett végpont protokollt, a tárfiók nevét és a tárelérési kulcsát, az Azure storage-fiók a megadott karakterlánc változót. Cserélje le a helyőrző értékeket hello **a\_fiók\_neve** és **a\_fiók\_kulcs** saját fiók nevét és a fiókkulcsot, kulcsattribútumokkal.

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

Adja hozzá a deklaráció a következő **fő**, közé tartozik egy **próbálja** blokkolni, és tartalmazzák a hello szükséges nyitott zárójelek között van feltüntetve, **{**.

```java
    public static void main(String[] args)
    {
        try
        {
```

Deklarálja a változókat hello típusa (hello azok használata ebben a példában a rendszer leírása) a következő:

* **CloudStorageAccount**: használt tooinitialize hello fiók objektum az az Azure storage-fiók nevét és -kulccsal és toocreate a blob ügyfél objektumot.
* **CloudBlobClient**: tooaccess hello blob szolgáltatás használt.
* **CloudBlobContainer**: használt toocreate egy blob tároló hello tároló és a delete hello tárolóban lévő blobok listázása.
* **CloudBlockBlob**: használt tooupload egy kép: helyi fájl toothe tárolót.

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

Rendelje hozzá egy érték toohello **fiók** változó.

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

Rendelje hozzá egy érték toohello **serviceClient** változó.

```java
serviceClient = account.createCloudBlobClient();
```

Rendelje hozzá egy érték toohello **tároló** változó. Azt a referencia-tooa nevű tárolót kaphat **gettingstarted**.

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

Hello tároló létrehozása. Ezzel a módszerrel hoz létre hello tárolót, ha még nem létezik (és visszatérési **igaz**). Ha hello tároló létezik, akkor a rendszer visszaküldi **hamis**. Alternatív túl**createIfNotExists** hello van **létrehozása** metódust (hibát adnak vissza, ha már van hello tároló).

```java
container.createIfNotExists();
```

Névtelen hozzáférés hello tároló beállítása.

```java
// Set anonymous access on hello container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

Egy hivatkozási toohello blokkblob, amely jelöli az Azure storage hello blob beolvasása.

```java
blob = container.getBlockBlobReference("image1.jpg");
```

Használjon hello **fájl** konstruktor tooget fel kell töltenie hivatkozás toohello helyileg létrehozott fájlt. Győződjön meg arról, ez a fájl létrehozott hello kód futtatása előtt.

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

A hívás toohello keresztül hello a helyi fájl feltöltése **CloudBlockBlob.upload** metódust. első paraméter toohello hello **CloudBlockBlob.upload** metódus egy **FileInputStream** adott jelöli hello helyi fájl, amely lesz feltöltve tooAzure tárolási objektum. második hello paraméter-hello méretét, bájtban hello fájlt.

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

Nevű segítő függvény **MakeHTMLPage**, toomake egy egyszerű HTML-oldal, amely tartalmaz egy  **&lt;kép&gt;**  elem az hello forrás beállítása toohello blob, amelyik most már az Azure-ban Storage-fiók. a kód hello **MakeHTMLPage** a cikk tárgyalja.

```java
MakeHTMLPage(container);
```

Nyomtassa ki a állapotüzenet és a HTML-weblap létrehozott hello kapcsolatos információkat.

```java
System.out.println("Processing complete.");
System.out.println("Open index.html toosee hello images stored in your storage account.");
```

Bezárás hello **próbálja** blokk egy záró zárójel beszúrásával: **}**

A következő kivételek hello kezelni:

* **FileNotFoundException**: hello által is okozhat **FileInputStream** vagy **FileOutputStream** konstruktor.
* **StorageException**: hello Azure storage ügyfélkódtár által is okozhat.
* **URISyntaxException**: hello által is okozhat **ListBlobItem.getUri** metódust.
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

Nevű metódus létrehozása **MakeHTMLPage** toocreate egy egyszerű HTML-lapon. Ez a módszer típusú paraméterrel rendelkezik **CloudBlobContainer**, amely lesz használt tooiterate keresztül feltöltött blobon hello listája. Ez a módszer kivételhibát típusú kivételek **FileNotFoundException**, hello által is okozhat **FileOutputStream** konstruktor, és **URISyntaxException**, amely lehet hello által okozott kell **ListBlobItem.getUri** metódust. A nyitó zárójel tartalmaznak **{**.

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

Hozzon létre egy helyi fájlt **index.html**.

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

Toohello helyi fájl hozzáadása a hello írható  **&lt;html&gt;**,  **&lt;fejléc&gt;**, és  **&lt;törzs&gt;**  elemek.

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

Iterációt feltöltött blobon hello listája. Minden egyes blob hello HTML-lapon hozzon létre egy  **&lt;img&gt;**  elem, amelynek a **src** küldött hello hello blob URI Azonosítóját az Azure storage-fiókjában található attribútum.
Bár csak egy kép ebben a mintában hozzáadni, ha több hozzáadott, ezt a kódot az összes kellene többször.

Az egyszerűség kedvéért ez a példa feltételezi minden feltöltött blobon lemezkép. Blobok eltérő képek vagy lapblobokat helyett blokkblobokat frissítése, ha úgy dönt, igény szerint hello kódot.

```java
// Enumerate hello uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in hello HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

Bezárás hello  **&lt;törzs&gt;**  elem és hello  **&lt;html&gt;**  elemet.

```java
stream.println("</body>");
stream.println("</html>");
```

Bezárás hello helyi fájl.

```java
stream.close();
```

Zárja be az **MakeHTMLPage** záró zárójel beszúrásával: **}**

Zárja be az **StorageSample** záró zárójel beszúrásával: **}**

hello hello teljes kód látható az ebben a példában látható. Ne feledje toomodify hello helyőrző értékeket **a\_fiók\_neve** és **a\_fiók\_kulcs** toouse a fiók nevét és a fiók billentyűt, illetve.

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;

// Create an image, c:\myimages\image1.jpg, prior toorunning this sample.
// Alternatively, change hello value used by hello FileInputStream constructor
// toouse a different image path and file that you have already created.
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

            // Set anonymous access on hello container.
            BlobContainerPermissions containerPermissions;
            containerPermissions = new BlobContainerPermissions();
            containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
            container.uploadPermissions(containerPermissions);

            // Upload an image file.
            blob = container.getBlockBlobReference("image1.jpg");

            File fileReference = new File("c:\\myimages\\image1.jpg");
            blob.upload(new FileInputStream(fileReference), fileReference.length());

            // At this point hello image is uploaded.
            // Next, create an HTML page that lists all of hello uploaded images.
            MakeHTMLPage(container);

            System.out.println("Processing complete.");
            System.out.println("Open index.html toosee hello images stored in your storage account.");

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

    // Create an HTML page that can be used toodisplay hello uploaded images.
    // This example assumes all of hello blobs are for images.
    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {
        PrintStream stream;
        stream = new PrintStream(new FileOutputStream("index.html"));

        // Create hello opening <html>, <header>, and <body> elements.
        stream.println("<html>");
        stream.println("<header/>");
        stream.println("<body>");

        // Enumerate hello uploaded blobs.
        for (ListBlobItem blobItem : container.listBlobs()) {
            // List each blob as an <img> element in hello HTML body.
            stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
        }

        stream.println("</body>");

        // Complete hello <html> element and close hello file.
        stream.println("</html>");
        stream.close();
    }
}
```

Továbbá toouploading kép: helyi fájl tooAzure tárhelyét, hello példakód létrehoz egy helyi fájl namedindex.html, a böngésző toosee megnyithat a feltöltött lemezkép.

Hello kódot tartalmaz, a fiók nevét és a fiókkulcsot, győződjön meg arról, hogy a forráskód biztonságos.

## <a name="toodelete-a-container"></a>egy tároló toodelete
Mert van szó, a tárolási, érdemes lehet a toodelete a **gettingstarted** tároló, miután befejezte az ebben a példában ki. egy tároló toodelete hello használata **CloudBlobContainer.delete** metódust.

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

toocall hello **CloudBlobContainer.delete** metódust, hello folyamat inicializálása **CloudStorageAccount**, **ClodBlobClient**, és  **CloudBlobContainer** objektumok van hello ugyanaz, ahogy a **createIfNotExist** metódust. hello az alábbiakban látható egy teljes példa, hogy törli hello nevű tárolót **gettingstarted**.

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

Más blob tárolási osztályokhoz és módszerek áttekintését lásd: [hogyan toouse Blob storage-ának Java](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Következő lépések
Kövesse az e további információ az összetettebb tárolási feladatok elvégzéséről hivatkozások toolearn.

* [Az Azure Storage Java SDK](https://github.com/azure/azure-storage-java)
* [Az Azure Storage ügyfél SDK-dokumentáció](http://dl.windowsazure.com/storage/javadoc/)
* [Az Azure Storage-szolgáltatások REST API-ja](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Az Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)

