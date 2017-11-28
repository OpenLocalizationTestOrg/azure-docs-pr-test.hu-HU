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
# <a name="on-premises-application-with-blob-storage"></a><span data-ttu-id="33b5a-104">A helyszíni alkalmazások az blob storage szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="33b5a-104">On-premises application with blob storage</span></span>
## <a name="overview"></a><span data-ttu-id="33b5a-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="33b5a-105">Overview</span></span>
<span data-ttu-id="33b5a-106">hello alábbi példa bemutatja, hogyan használhatja az Azure storage tud lemezképeket menteni az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="33b5a-106">hello following example shows you how you can use Azure storage to store images in Azure.</span></span> <span data-ttu-id="33b5a-107">Ebben a cikkben hello kód van egy konzolalkalmazást, amely feltölt egy képet tooAzure, és létrehozza a HTML-fájlba hello kép megjelenítése a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="33b5a-107">hello code in this article is for a console application that uploads an image tooAzure, and then creates an HTML file that displays hello image in your browser.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33b5a-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="33b5a-108">Prerequisites</span></span>
* <span data-ttu-id="33b5a-109">A Java fejlesztői készlet (JDK), 1,6 vagy újabb verziója már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="33b5a-109">A Java Developer Kit (JDK), version 1.6 or later, is installed.</span></span>
* <span data-ttu-id="33b5a-110">hello Azure SDK telepítve van.</span><span class="sxs-lookup"><span data-stu-id="33b5a-110">hello Azure SDK is installed.</span></span>
* <span data-ttu-id="33b5a-111">hello JAR a Java hello Azure-könyvtárakban, és bármely alkalmazandó függőségi JAR-fájlok kivételével, telepítve van, és a Java-fordítónak használják a hello build elérési úton találhatók.</span><span class="sxs-lookup"><span data-stu-id="33b5a-111">hello JAR for hello Azure Libraries for Java, and any applicable dependency JARs, are installed and are in hello build path used by your Java compiler.</span></span> <span data-ttu-id="33b5a-112">Java hello Azure-könyvtárakban telepítésével kapcsolatos információkért lásd: [letöltési hello Azure SDK for Java](../java-download-azure-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="33b5a-112">For information about installing hello Azure Libraries for Java, see [Download hello Azure SDK for Java](../java-download-azure-sdk.md).</span></span>
* <span data-ttu-id="33b5a-113">Egy Azure storage-fiók be van állítva.</span><span class="sxs-lookup"><span data-stu-id="33b5a-113">An Azure storage account has been set up.</span></span> <span data-ttu-id="33b5a-114">hello fióknevet és a fiókkulcsot hello tárfiók által használandó hello kód ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="33b5a-114">hello account name and account key for hello storage account will be used by hello code in this article.</span></span> <span data-ttu-id="33b5a-115">Lásd: [hogyan tooCreate Tárfiók](storage-create-storage-account.md#create-a-storage-account) a storage-fiók létrehozásával kapcsolatos információkat és [megtekintése és másolása tárelérési kulcsok](storage-create-storage-account.md#view-and-copy-storage-access-keys) hello fiókkulcs beolvasásával kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="33b5a-115">See [How tooCreate a Storage Account](storage-create-storage-account.md#create-a-storage-account) for information about creating a storage account, and [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys) for information about retrieving hello account key.</span></span>
* <span data-ttu-id="33b5a-116">Létrehozott nevű kép: helyi fájl helye: hello elérési c:\\myimages\\image1.jpg.</span><span class="sxs-lookup"><span data-stu-id="33b5a-116">You have created a local image file named stored at hello path c:\\myimages\\image1.jpg.</span></span> <span data-ttu-id="33b5a-117">Azt is megteheti, módosítsa a **FileInputStream** konstruktor a hello példa toouse különböző kép elérési útját és nevét.</span><span class="sxs-lookup"><span data-stu-id="33b5a-117">Alternatively, modify the **FileInputStream** constructor in hello example toouse a different image path and file name.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="toouse-azure-blob-storage-tooupload-a-file"></a><span data-ttu-id="33b5a-118">toouse Azure blob storage tooupload egy fájlt</span><span class="sxs-lookup"><span data-stu-id="33b5a-118">toouse Azure blob storage tooupload a file</span></span>
<span data-ttu-id="33b5a-119">Eljárás pontos lépéseit itt akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="33b5a-119">A step-by-step procedure is presented here.</span></span> <span data-ttu-id="33b5a-120">Ha szeretné azokat, amelyek tooskip, hello teljes kód számára jelenik meg a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="33b5a-120">If you'd like tooskip ahead, hello entire code is presented later in this article.</span></span>

<span data-ttu-id="33b5a-121">Kezdje az hello kód hello Azure core tárolási osztályok, hello Azure blob ügyfél osztályok, hello Java IO osztályok és hello irányuló beleértve **URISyntaxException** osztály.</span><span class="sxs-lookup"><span data-stu-id="33b5a-121">Begin hello code by including imports for hello Azure core storage classes, hello Azure blob client classes, hello Java IO classes, and hello **URISyntaxException** class.</span></span>

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

<span data-ttu-id="33b5a-122">Egy osztályt deklarálható **StorageSample**, és adja meg a nyitó zárójel hello, **{**.</span><span class="sxs-lookup"><span data-stu-id="33b5a-122">Declare a class named **StorageSample**, and include hello open bracket, **{**.</span></span>

```java
public class StorageSample {
```

<span data-ttu-id="33b5a-123">Hello belül **StorageSample** osztály, fogja tartalmazni, hello alapértelmezett végpont protokollt, a tárfiók nevét és a tárelérési kulcsát, az Azure storage-fiók a megadott karakterlánc változót.</span><span class="sxs-lookup"><span data-stu-id="33b5a-123">Within hello **StorageSample** class, declare a string variable that will contain hello default endpoint protocol, your storage account name, and your storage access key, as specified in your Azure storage account.</span></span> <span data-ttu-id="33b5a-124">Cserélje le a helyőrző értékeket hello **a\_fiók\_neve** és **a\_fiók\_kulcs** saját fiók nevét és a fiókkulcsot, kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="33b5a-124">Replace hello placeholder values **your\_account\_name** and **your\_account\_key** with your own account name and account key, respectively.</span></span>

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

<span data-ttu-id="33b5a-125">Adja hozzá a deklaráció a következő **fő**, közé tartozik egy **próbálja** blokkolni, és tartalmazzák a hello szükséges nyitott zárójelek között van feltüntetve, **{**.</span><span class="sxs-lookup"><span data-stu-id="33b5a-125">Add in your declaration for **main**, include a **try** block, and include hello necessary open brackets, **{**.</span></span>

```java
    public static void main(String[] args)
    {
        try
        {
```

<span data-ttu-id="33b5a-126">Deklarálja a változókat hello típusa (hello azok használata ebben a példában a rendszer leírása) a következő:</span><span class="sxs-lookup"><span data-stu-id="33b5a-126">Declare variables of hello following type (hello descriptions are for how they are used in this example):</span></span>

* <span data-ttu-id="33b5a-127">**CloudStorageAccount**: használt tooinitialize hello fiók objektum az az Azure storage-fiók nevét és -kulccsal és toocreate a blob ügyfél objektumot.</span><span class="sxs-lookup"><span data-stu-id="33b5a-127">**CloudStorageAccount**: Used tooinitialize hello account object with your Azure storage account name and key, and toocreate the blob client object.</span></span>
* <span data-ttu-id="33b5a-128">**CloudBlobClient**: tooaccess hello blob szolgáltatás használt.</span><span class="sxs-lookup"><span data-stu-id="33b5a-128">**CloudBlobClient**: Used tooaccess hello blob service.</span></span>
* <span data-ttu-id="33b5a-129">**CloudBlobContainer**: használt toocreate egy blob tároló hello tároló és a delete hello tárolóban lévő blobok listázása.</span><span class="sxs-lookup"><span data-stu-id="33b5a-129">**CloudBlobContainer**: Used toocreate a blob container, list the blobs in hello container, and delete hello container.</span></span>
* <span data-ttu-id="33b5a-130">**CloudBlockBlob**: használt tooupload egy kép: helyi fájl toothe tárolót.</span><span class="sxs-lookup"><span data-stu-id="33b5a-130">**CloudBlockBlob**: Used tooupload a local image file toothe container.</span></span>

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

<span data-ttu-id="33b5a-131">Rendelje hozzá egy érték toohello **fiók** változó.</span><span class="sxs-lookup"><span data-stu-id="33b5a-131">Assign a value toohello **account** variable.</span></span>

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

<span data-ttu-id="33b5a-132">Rendelje hozzá egy érték toohello **serviceClient** változó.</span><span class="sxs-lookup"><span data-stu-id="33b5a-132">Assign a value toohello **serviceClient** variable.</span></span>

```java
serviceClient = account.createCloudBlobClient();
```

<span data-ttu-id="33b5a-133">Rendelje hozzá egy érték toohello **tároló** változó.</span><span class="sxs-lookup"><span data-stu-id="33b5a-133">Assign a value toohello **container** variable.</span></span> <span data-ttu-id="33b5a-134">Azt a referencia-tooa nevű tárolót kaphat **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="33b5a-134">We'll get a reference tooa container named **gettingstarted**.</span></span>

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

<span data-ttu-id="33b5a-135">Hello tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="33b5a-135">Create hello container.</span></span> <span data-ttu-id="33b5a-136">Ezzel a módszerrel hoz létre hello tárolót, ha még nem létezik (és visszatérési **igaz**).</span><span class="sxs-lookup"><span data-stu-id="33b5a-136">This method will create hello container if it doesn't exist (and return **true**).</span></span> <span data-ttu-id="33b5a-137">Ha hello tároló létezik, akkor a rendszer visszaküldi **hamis**.</span><span class="sxs-lookup"><span data-stu-id="33b5a-137">If hello container does exist, it will return **false**.</span></span> <span data-ttu-id="33b5a-138">Alternatív túl**createIfNotExists** hello van **létrehozása** metódust (hibát adnak vissza, ha már van hello tároló).</span><span class="sxs-lookup"><span data-stu-id="33b5a-138">An alternative too**createIfNotExists** is hello **create** method (which will return an error if hello container already exists).</span></span>

```java
container.createIfNotExists();
```

<span data-ttu-id="33b5a-139">Névtelen hozzáférés hello tároló beállítása.</span><span class="sxs-lookup"><span data-stu-id="33b5a-139">Set anonymous access for hello container.</span></span>

```java
// Set anonymous access on hello container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

<span data-ttu-id="33b5a-140">Egy hivatkozási toohello blokkblob, amely jelöli az Azure storage hello blob beolvasása.</span><span class="sxs-lookup"><span data-stu-id="33b5a-140">Get a reference toohello block blob, which will represent hello blob in Azure storage.</span></span>

```java
blob = container.getBlockBlobReference("image1.jpg");
```

<span data-ttu-id="33b5a-141">Használjon hello **fájl** konstruktor tooget fel kell töltenie hivatkozás toohello helyileg létrehozott fájlt.</span><span class="sxs-lookup"><span data-stu-id="33b5a-141">Use hello **File** constructor tooget a reference toohello locally created file that you will upload.</span></span> <span data-ttu-id="33b5a-142">Győződjön meg arról, ez a fájl létrehozott hello kód futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="33b5a-142">Ensure you have created this file before running hello code.</span></span>

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

<span data-ttu-id="33b5a-143">A hívás toohello keresztül hello a helyi fájl feltöltése **CloudBlockBlob.upload** metódust.</span><span class="sxs-lookup"><span data-stu-id="33b5a-143">Upload hello local file through a call toohello **CloudBlockBlob.upload** method.</span></span> <span data-ttu-id="33b5a-144">első paraméter toohello hello **CloudBlockBlob.upload** metódus egy **FileInputStream** adott jelöli hello helyi fájl, amely lesz feltöltve tooAzure tárolási objektum.</span><span class="sxs-lookup"><span data-stu-id="33b5a-144">hello first parameter toohello **CloudBlockBlob.upload** method is a **FileInputStream** object that represents hello local file that will be uploaded tooAzure storage.</span></span> <span data-ttu-id="33b5a-145">második hello paraméter-hello méretét, bájtban hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="33b5a-145">hello second parameter is hello size, in bytes, of hello file.</span></span>

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

<span data-ttu-id="33b5a-146">Nevű segítő függvény **MakeHTMLPage**, toomake egy egyszerű HTML-oldal, amely tartalmaz egy  **&lt;kép&gt;**  elem az hello forrás beállítása toohello blob, amelyik most már az Azure-ban Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="33b5a-146">Call a helper function named **MakeHTMLPage**, toomake a basic HTML page that contains an **&lt;image&gt;** element with hello source set toohello blob that is now in your Azure storage account.</span></span> <span data-ttu-id="33b5a-147">a kód hello **MakeHTMLPage** a cikk tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="33b5a-147">hello code for **MakeHTMLPage** will be discussed later in this article.</span></span>

```java
MakeHTMLPage(container);
```

<span data-ttu-id="33b5a-148">Nyomtassa ki a állapotüzenet és a HTML-weblap létrehozott hello kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="33b5a-148">Print out a status message and information about hello created HTML page.</span></span>

```java
System.out.println("Processing complete.");
System.out.println("Open index.html toosee hello images stored in your storage account.");
```

<span data-ttu-id="33b5a-149">Bezárás hello **próbálja** blokk egy záró zárójel beszúrásával: **}**</span><span class="sxs-lookup"><span data-stu-id="33b5a-149">Close hello **try** block by inserting a close bracket: **}**</span></span>

<span data-ttu-id="33b5a-150">A következő kivételek hello kezelni:</span><span class="sxs-lookup"><span data-stu-id="33b5a-150">Handle hello following exceptions:</span></span>

* <span data-ttu-id="33b5a-151">**FileNotFoundException**: hello által is okozhat **FileInputStream** vagy **FileOutputStream** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="33b5a-151">**FileNotFoundException**: Can be thrown by hello **FileInputStream** or **FileOutputStream** constructors.</span></span>
* <span data-ttu-id="33b5a-152">**StorageException**: hello Azure storage ügyfélkódtár által is okozhat.</span><span class="sxs-lookup"><span data-stu-id="33b5a-152">**StorageException**: Can be thrown by hello Azure client storage library.</span></span>
* <span data-ttu-id="33b5a-153">**URISyntaxException**: hello által is okozhat **ListBlobItem.getUri** metódust.</span><span class="sxs-lookup"><span data-stu-id="33b5a-153">**URISyntaxException**: Can be thrown by hello **ListBlobItem.getUri** method.</span></span>
* <span data-ttu-id="33b5a-154">**Kivétel**: Általános kivétel kezelése.</span><span class="sxs-lookup"><span data-stu-id="33b5a-154">**Exception**: Generic exception handling.</span></span>

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

<span data-ttu-id="33b5a-155">Zárja be az **fő** záró zárójel beszúrásával: **}**</span><span class="sxs-lookup"><span data-stu-id="33b5a-155">Close **main** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="33b5a-156">Nevű metódus létrehozása **MakeHTMLPage** toocreate egy egyszerű HTML-lapon.</span><span class="sxs-lookup"><span data-stu-id="33b5a-156">Create a method named **MakeHTMLPage** toocreate a basic HTML page.</span></span> <span data-ttu-id="33b5a-157">Ez a módszer típusú paraméterrel rendelkezik **CloudBlobContainer**, amely lesz használt tooiterate keresztül feltöltött blobon hello listája.</span><span class="sxs-lookup"><span data-stu-id="33b5a-157">This method has a parameter of type **CloudBlobContainer**, which will be used tooiterate through hello list of uploaded blobs.</span></span> <span data-ttu-id="33b5a-158">Ez a módszer kivételhibát típusú kivételek **FileNotFoundException**, hello által is okozhat **FileOutputStream** konstruktor, és **URISyntaxException**, amely lehet hello által okozott kell **ListBlobItem.getUri** metódust.</span><span class="sxs-lookup"><span data-stu-id="33b5a-158">This method will throw exceptions of type **FileNotFoundException**, which can be thrown by hello **FileOutputStream** constructor, and **URISyntaxException**, which can be thrown by hello **ListBlobItem.getUri** method.</span></span> <span data-ttu-id="33b5a-159">A nyitó zárójel tartalmaznak **{**.</span><span class="sxs-lookup"><span data-stu-id="33b5a-159">Include the opening bracket, **{**.</span></span>

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

<span data-ttu-id="33b5a-160">Hozzon létre egy helyi fájlt **index.html**.</span><span class="sxs-lookup"><span data-stu-id="33b5a-160">Create a local file named **index.html**.</span></span>

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

<span data-ttu-id="33b5a-161">Toohello helyi fájl hozzáadása a hello írható  **&lt;html&gt;**,  **&lt;fejléc&gt;**, és  **&lt;törzs&gt;**  elemek.</span><span class="sxs-lookup"><span data-stu-id="33b5a-161">Write toohello local file, adding in hello **&lt;html&gt;**, **&lt;header&gt;**, and **&lt;body&gt;** elements.</span></span>

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

<span data-ttu-id="33b5a-162">Iterációt feltöltött blobon hello listája.</span><span class="sxs-lookup"><span data-stu-id="33b5a-162">Iterate through hello list of uploaded blobs.</span></span> <span data-ttu-id="33b5a-163">Minden egyes blob hello HTML-lapon hozzon létre egy  **&lt;img&gt;**  elem, amelynek a **src** küldött hello hello blob URI Azonosítóját az Azure storage-fiókjában található attribútum.</span><span class="sxs-lookup"><span data-stu-id="33b5a-163">For each blob, in hello HTML page create an **&lt;img&gt;** element that has its **src** attribute sent to hello URI of hello blob as it exists in your Azure storage account.</span></span>
<span data-ttu-id="33b5a-164">Bár csak egy kép ebben a mintában hozzáadni, ha több hozzáadott, ezt a kódot az összes kellene többször.</span><span class="sxs-lookup"><span data-stu-id="33b5a-164">Although you added only one image in this sample, if you added more, this code would iterate all of them.</span></span>

<span data-ttu-id="33b5a-165">Az egyszerűség kedvéért ez a példa feltételezi minden feltöltött blobon lemezkép.</span><span class="sxs-lookup"><span data-stu-id="33b5a-165">For simplicity, this example assumes each uploaded blob is an image.</span></span> <span data-ttu-id="33b5a-166">Blobok eltérő képek vagy lapblobokat helyett blokkblobokat frissítése, ha úgy dönt, igény szerint hello kódot.</span><span class="sxs-lookup"><span data-stu-id="33b5a-166">If you've updated blobs other than images, or page blobs instead of block blobs, adjust hello code as needed.</span></span>

```java
// Enumerate hello uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in hello HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

<span data-ttu-id="33b5a-167">Bezárás hello  **&lt;törzs&gt;**  elem és hello  **&lt;html&gt;**  elemet.</span><span class="sxs-lookup"><span data-stu-id="33b5a-167">Close hello **&lt;body&gt;** element and hello **&lt;html&gt;** element.</span></span>

```java
stream.println("</body>");
stream.println("</html>");
```

<span data-ttu-id="33b5a-168">Bezárás hello helyi fájl.</span><span class="sxs-lookup"><span data-stu-id="33b5a-168">Close hello local file.</span></span>

```java
stream.close();
```

<span data-ttu-id="33b5a-169">Zárja be az **MakeHTMLPage** záró zárójel beszúrásával: **}**</span><span class="sxs-lookup"><span data-stu-id="33b5a-169">Close **MakeHTMLPage** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="33b5a-170">Zárja be az **StorageSample** záró zárójel beszúrásával: **}**</span><span class="sxs-lookup"><span data-stu-id="33b5a-170">Close **StorageSample** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="33b5a-171">hello hello teljes kód látható az ebben a példában látható.</span><span class="sxs-lookup"><span data-stu-id="33b5a-171">hello following is hello complete code for this example.</span></span> <span data-ttu-id="33b5a-172">Ne feledje toomodify hello helyőrző értékeket **a\_fiók\_neve** és **a\_fiók\_kulcs** toouse a fiók nevét és a fiók billentyűt, illetve.</span><span class="sxs-lookup"><span data-stu-id="33b5a-172">Remember toomodify hello placeholder values **your\_account\_name** and **your\_account\_key** toouse your account name and account key, respectively.</span></span>

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

<span data-ttu-id="33b5a-173">Továbbá toouploading kép: helyi fájl tooAzure tárhelyét, hello példakód létrehoz egy helyi fájl namedindex.html, a böngésző toosee megnyithat a feltöltött lemezkép.</span><span class="sxs-lookup"><span data-stu-id="33b5a-173">In addition toouploading your local image file tooAzure storage, hello example code creates a local file namedindex.html, which you can open in your browser toosee your uploaded image.</span></span>

<span data-ttu-id="33b5a-174">Hello kódot tartalmaz, a fiók nevét és a fiókkulcsot, győződjön meg arról, hogy a forráskód biztonságos.</span><span class="sxs-lookup"><span data-stu-id="33b5a-174">Because hello code contains your account name and account key, ensure that your source code is secure.</span></span>

## <a name="toodelete-a-container"></a><span data-ttu-id="33b5a-175">egy tároló toodelete</span><span class="sxs-lookup"><span data-stu-id="33b5a-175">toodelete a container</span></span>
<span data-ttu-id="33b5a-176">Mert van szó, a tárolási, érdemes lehet a toodelete a **gettingstarted** tároló, miután befejezte az ebben a példában ki.</span><span class="sxs-lookup"><span data-stu-id="33b5a-176">Because you are charged for storage, you may want toodelete the **gettingstarted** container after you are done experimenting with this example.</span></span> <span data-ttu-id="33b5a-177">egy tároló toodelete hello használata **CloudBlobContainer.delete** metódust.</span><span class="sxs-lookup"><span data-stu-id="33b5a-177">toodelete a container, use hello **CloudBlobContainer.delete** method.</span></span>

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

<span data-ttu-id="33b5a-178">toocall hello **CloudBlobContainer.delete** metódust, hello folyamat inicializálása **CloudStorageAccount**, **ClodBlobClient**, és  **CloudBlobContainer** objektumok van hello ugyanaz, ahogy a **createIfNotExist** metódust.</span><span class="sxs-lookup"><span data-stu-id="33b5a-178">toocall hello **CloudBlobContainer.delete** method, hello process of initializing **CloudStorageAccount**, **ClodBlobClient**, and **CloudBlobContainer** objects is hello same as shown for the **createIfNotExist** method.</span></span> <span data-ttu-id="33b5a-179">hello az alábbiakban látható egy teljes példa, hogy törli hello nevű tárolót **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="33b5a-179">hello following is a complete example that deletes hello container named **gettingstarted**.</span></span>

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

<span data-ttu-id="33b5a-180">Más blob tárolási osztályokhoz és módszerek áttekintését lásd: [hogyan toouse Blob storage-ának Java](storage-java-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="33b5a-180">For an overview of other blob storage classes and methods, see [How toouse Blob storage from Java](storage-java-how-to-use-blob-storage.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="33b5a-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="33b5a-181">Next steps</span></span>
<span data-ttu-id="33b5a-182">Kövesse az e további információ az összetettebb tárolási feladatok elvégzéséről hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="33b5a-182">Follow these links toolearn more about more complex storage tasks.</span></span>

* [<span data-ttu-id="33b5a-183">Az Azure Storage Java SDK</span><span class="sxs-lookup"><span data-stu-id="33b5a-183">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="33b5a-184">Az Azure Storage ügyfél SDK-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="33b5a-184">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="33b5a-185">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="33b5a-185">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="33b5a-186">Az Azure Storage csapat blogja</span><span class="sxs-lookup"><span data-stu-id="33b5a-186">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)

