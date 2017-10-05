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
# <a name="on-premises-application-with-blob-storage"></a><span data-ttu-id="3f050-104">A helyszíni alkalmazások az blob storage szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="3f050-104">On-premises application with blob storage</span></span>
## <a name="overview"></a><span data-ttu-id="3f050-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3f050-105">Overview</span></span>
<span data-ttu-id="3f050-106">A következő példa bemutatja, hogyan használhatja az Azure storage tud lemezképeket menteni az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3f050-106">The following example shows you how you can use Azure storage to store images in Azure.</span></span> <span data-ttu-id="3f050-107">Ebben a cikkben a kód egy konzolalkalmazást, amely feltölt egy képet az Azure-ba, és létrehozza a kép jelenik meg a böngészőben HTML-fájlba van.</span><span class="sxs-lookup"><span data-stu-id="3f050-107">The code in this article is for a console application that uploads an image to Azure, and then creates an HTML file that displays the image in your browser.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f050-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3f050-108">Prerequisites</span></span>
* <span data-ttu-id="3f050-109">A Java fejlesztői készlet (JDK), 1,6 vagy újabb verziója már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="3f050-109">A Java Developer Kit (JDK), version 1.6 or later, is installed.</span></span>
* <span data-ttu-id="3f050-110">Az Azure SDK telepítve van.</span><span class="sxs-lookup"><span data-stu-id="3f050-110">The Azure SDK is installed.</span></span>
* <span data-ttu-id="3f050-111">A Javához készült Azure-tárak JAR, és bármely alkalmazandó függőségi JAR-fájlok kivételével, telepítve van, és a Java-fordítónak használják, a build elérési úton találhatók.</span><span class="sxs-lookup"><span data-stu-id="3f050-111">The JAR for the Azure Libraries for Java, and any applicable dependency JARs, are installed and are in the build path used by your Java compiler.</span></span> <span data-ttu-id="3f050-112">Az Azure-könyvtárakban Java telepítésével kapcsolatos információkért lásd: [töltse le az Azure SDK for Java](../java-download-azure-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="3f050-112">For information about installing the Azure Libraries for Java, see [Download the Azure SDK for Java](../java-download-azure-sdk.md).</span></span>
* <span data-ttu-id="3f050-113">Egy Azure storage-fiók be van állítva.</span><span class="sxs-lookup"><span data-stu-id="3f050-113">An Azure storage account has been set up.</span></span> <span data-ttu-id="3f050-114">A fiók nevét és a tárfiók kulcsára ebben a cikkben a kód használják.</span><span class="sxs-lookup"><span data-stu-id="3f050-114">The account name and account key for the storage account will be used by the code in this article.</span></span> <span data-ttu-id="3f050-115">Lásd: [a Storage-fiók létrehozása](storage-create-storage-account.md#create-a-storage-account) a storage-fiók létrehozásával kapcsolatos információkat és [megtekintése és másolása tárelérési kulcsok](storage-create-storage-account.md#view-and-copy-storage-access-keys) a fiókkulcs beolvasásával kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="3f050-115">See [How to Create a Storage Account](storage-create-storage-account.md#create-a-storage-account) for information about creating a storage account, and [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys) for information about retrieving the account key.</span></span>
* <span data-ttu-id="3f050-116">Létrehozott nevű kép: helyi fájl elérési útja c: mentve\\myimages\\image1.jpg.</span><span class="sxs-lookup"><span data-stu-id="3f050-116">You have created a local image file named stored at the path c:\\myimages\\image1.jpg.</span></span> <span data-ttu-id="3f050-117">Azt is megteheti, módosítsa a **FileInputStream** konstruktor a példa egy másik kép elérési útját és fájlnevét név.</span><span class="sxs-lookup"><span data-stu-id="3f050-117">Alternatively, modify the **FileInputStream** constructor in the example to use a different image path and file name.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a><span data-ttu-id="3f050-118">Fájl feltöltése az Azure blob storage segítségével</span><span class="sxs-lookup"><span data-stu-id="3f050-118">To use Azure blob storage to upload a file</span></span>
<span data-ttu-id="3f050-119">Eljárás pontos lépéseit itt akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3f050-119">A step-by-step procedure is presented here.</span></span> <span data-ttu-id="3f050-120">Ha azt szeretné, kihagyhatja azokat, amelyek, a teljes kód számára jelenik meg a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="3f050-120">If you'd like to skip ahead, the entire code is presented later in this article.</span></span>

<span data-ttu-id="3f050-121">Kezdje az importálja az Azure core tárolási osztályok, az Azure blob ügyfél osztályok, a Java IO osztályok, beleértve a kódot és a **URISyntaxException** osztály.</span><span class="sxs-lookup"><span data-stu-id="3f050-121">Begin the code by including imports for the Azure core storage classes, the Azure blob client classes, the Java IO classes, and the **URISyntaxException** class.</span></span>

```java
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
import java.io.*;
import java.net.URISyntaxException;
```

<span data-ttu-id="3f050-122">Egy osztályt deklarálható **StorageSample**, és adja meg a nyitó zárójel **{**.</span><span class="sxs-lookup"><span data-stu-id="3f050-122">Declare a class named **StorageSample**, and include the open bracket, **{**.</span></span>

```java
public class StorageSample {
```

<span data-ttu-id="3f050-123">Belül a **StorageSample** osztály, az alapértelmezett végpont protokoll, a tárfiók nevét és a tárelérési kulcsát, az Azure storage-fiókhoz megadott tartalmazó karakterlánc változót.</span><span class="sxs-lookup"><span data-stu-id="3f050-123">Within the **StorageSample** class, declare a string variable that will contain the default endpoint protocol, your storage account name, and your storage access key, as specified in your Azure storage account.</span></span> <span data-ttu-id="3f050-124">Cserélje le a helyőrző értékeket **a\_fiók\_neve** és **a\_fiók\_kulcs** saját fiók nevét és a fiókkulcsot, kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="3f050-124">Replace the placeholder values **your\_account\_name** and **your\_account\_key** with your own account name and account key, respectively.</span></span>

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_account_name;" +
    "AccountKey=your_account_name";
```

<span data-ttu-id="3f050-125">Adja hozzá a deklaráció a következő **fő**, közé tartozik egy **próbálja** letiltása, és tartalmazza a szükséges nyitott szögletes **{**.</span><span class="sxs-lookup"><span data-stu-id="3f050-125">Add in your declaration for **main**, include a **try** block, and include the necessary open brackets, **{**.</span></span>

```java
    public static void main(String[] args)
    {
        try
        {
```

<span data-ttu-id="3f050-126">(Azok használata ebben a példában a rendszer a leírása) a következő típusú változó deklarálható:</span><span class="sxs-lookup"><span data-stu-id="3f050-126">Declare variables of the following type (the descriptions are for how they are used in this example):</span></span>

* <span data-ttu-id="3f050-127">**CloudStorageAccount**: inicializálni a számítógépfiók-objektum az az Azure storage-fiók nevét és a kulcs, és a blob ügyfél objektum létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="3f050-127">**CloudStorageAccount**: Used to initialize the account object with your Azure storage account name and key, and to create the blob client object.</span></span>
* <span data-ttu-id="3f050-128">**CloudBlobClient**: a blob szolgáltatás eléréséhez használt.</span><span class="sxs-lookup"><span data-stu-id="3f050-128">**CloudBlobClient**: Used to access the blob service.</span></span>
* <span data-ttu-id="3f050-129">**CloudBlobContainer**: egy blob-tároló létrehozásához használt, a tárolóban lévő blobok listázása, és a tároló törlése.</span><span class="sxs-lookup"><span data-stu-id="3f050-129">**CloudBlobContainer**: Used to create a blob container, list the blobs in the container, and delete the container.</span></span>
* <span data-ttu-id="3f050-130">**CloudBlockBlob**: használt kép: helyi fájl feltöltése a tárolóba.</span><span class="sxs-lookup"><span data-stu-id="3f050-130">**CloudBlockBlob**: Used to upload a local image file to the container.</span></span>

<!-- -->

```java
    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;
```

<span data-ttu-id="3f050-131">Adja meg az értéket a **fiók** változó.</span><span class="sxs-lookup"><span data-stu-id="3f050-131">Assign a value to the **account** variable.</span></span>

```java
account = CloudStorageAccount.parse(storageConnectionString);
```

<span data-ttu-id="3f050-132">Adja meg az értéket a **serviceClient** változó.</span><span class="sxs-lookup"><span data-stu-id="3f050-132">Assign a value to the **serviceClient** variable.</span></span>

```java
serviceClient = account.createCloudBlobClient();
```

<span data-ttu-id="3f050-133">Adja meg az értéket a **tároló** változó.</span><span class="sxs-lookup"><span data-stu-id="3f050-133">Assign a value to the **container** variable.</span></span> <span data-ttu-id="3f050-134">Azt is nevű tárolót mutató hivatkozás lesz **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="3f050-134">We'll get a reference to a container named **gettingstarted**.</span></span>

```java
// Container name must be lower case.
container = serviceClient.getContainerReference("gettingstarted");
```

<span data-ttu-id="3f050-135">A tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3f050-135">Create the container.</span></span> <span data-ttu-id="3f050-136">Ezzel a módszerrel hoz létre a tárolót, ha még nem létezik (és visszatérési **igaz**).</span><span class="sxs-lookup"><span data-stu-id="3f050-136">This method will create the container if it doesn't exist (and return **true**).</span></span> <span data-ttu-id="3f050-137">Ha a tároló létezik, akkor a rendszer visszaküldi **hamis**.</span><span class="sxs-lookup"><span data-stu-id="3f050-137">If the container does exist, it will return **false**.</span></span> <span data-ttu-id="3f050-138">Alternatív megoldás **createIfNotExists** van a **létrehozása** metódust (hibát adnak vissza, ha a tároló már létezik).</span><span class="sxs-lookup"><span data-stu-id="3f050-138">An alternative to **createIfNotExists** is the **create** method (which will return an error if the container already exists).</span></span>

```java
container.createIfNotExists();
```

<span data-ttu-id="3f050-139">Állítsa be a névtelen hozzáférést ahhoz a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="3f050-139">Set anonymous access for the container.</span></span>

```java
// Set anonymous access on the container.
BlobContainerPermissions containerPermissions;
containerPermissions = new BlobContainerPermissions();
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
container.uploadPermissions(containerPermissions);
```

<span data-ttu-id="3f050-140">Lekérdezni egy jelöli az Azure storage blob blokkblob hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="3f050-140">Get a reference to the block blob, which will represent the blob in Azure storage.</span></span>

```java
blob = container.getBlockBlobReference("image1.jpg");
```

<span data-ttu-id="3f050-141">Használja a **fájl** konstruktort a helyileg létrehozott fájl, amely feltölti való hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="3f050-141">Use the **File** constructor to get a reference to the locally created file that you will upload.</span></span> <span data-ttu-id="3f050-142">Győződjön meg arról, ez a fájl hozott létre a kód futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="3f050-142">Ensure you have created this file before running the code.</span></span>

```java
File fileReference = new File ("c:\\myimages\\image1.jpg");
```

<span data-ttu-id="3f050-143">Hívása a helyi fájl feltöltése a **CloudBlockBlob.upload** metódust.</span><span class="sxs-lookup"><span data-stu-id="3f050-143">Upload the local file through a call to the **CloudBlockBlob.upload** method.</span></span> <span data-ttu-id="3f050-144">Az első paraméterben a **CloudBlockBlob.upload** metódus egy **FileInputStream** a helyi fájl, amely az Azure storage feltöltés képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="3f050-144">The first parameter to the **CloudBlockBlob.upload** method is a **FileInputStream** object that represents the local file that will be uploaded to Azure storage.</span></span> <span data-ttu-id="3f050-145">A második paraméter a mérete, a fájl.</span><span class="sxs-lookup"><span data-stu-id="3f050-145">The second parameter is the size, in bytes, of the file.</span></span>

```java
blob.upload(new FileInputStream(fileReference), fileReference.length());
```

<span data-ttu-id="3f050-146">Nevű segítő függvény **MakeHTMLPage**, hogy egy egyszerű HTML-lapot tartalmazó egy  **&lt;kép&gt;**  elem a forrás beállítása az, amelyik most már az Azure Storage-blobba fiók.</span><span class="sxs-lookup"><span data-stu-id="3f050-146">Call a helper function named **MakeHTMLPage**, to make a basic HTML page that contains an **&lt;image&gt;** element with the source set to the blob that is now in your Azure storage account.</span></span> <span data-ttu-id="3f050-147">A kód **MakeHTMLPage** a cikk tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="3f050-147">The code for **MakeHTMLPage** will be discussed later in this article.</span></span>

```java
MakeHTMLPage(container);
```

<span data-ttu-id="3f050-148">Nyomtassa ki a hibaállapot-üzeneteket és a létrehozott HTML-weblap információkat.</span><span class="sxs-lookup"><span data-stu-id="3f050-148">Print out a status message and information about the created HTML page.</span></span>

```java
System.out.println("Processing complete.");
System.out.println("Open index.html to see the images stored in your storage account.");
```

<span data-ttu-id="3f050-149">Zárja be a **próbálja** blokk egy záró zárójel beszúrásával: **}**</span><span class="sxs-lookup"><span data-stu-id="3f050-149">Close the **try** block by inserting a close bracket: **}**</span></span>

<span data-ttu-id="3f050-150">Kezeli a következő kivételekkel:</span><span class="sxs-lookup"><span data-stu-id="3f050-150">Handle the following exceptions:</span></span>

* <span data-ttu-id="3f050-151">**FileNotFoundException**: által okozott is lehet a **FileInputStream** vagy **FileOutputStream** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="3f050-151">**FileNotFoundException**: Can be thrown by the **FileInputStream** or **FileOutputStream** constructors.</span></span>
* <span data-ttu-id="3f050-152">**StorageException**: az Azure storage ügyfélkódtár által is okozhat.</span><span class="sxs-lookup"><span data-stu-id="3f050-152">**StorageException**: Can be thrown by the Azure client storage library.</span></span>
* <span data-ttu-id="3f050-153">**URISyntaxException**: által okozott is lehet a **ListBlobItem.getUri** metódust.</span><span class="sxs-lookup"><span data-stu-id="3f050-153">**URISyntaxException**: Can be thrown by the **ListBlobItem.getUri** method.</span></span>
* <span data-ttu-id="3f050-154">**Kivétel**: Általános kivétel kezelése.</span><span class="sxs-lookup"><span data-stu-id="3f050-154">**Exception**: Generic exception handling.</span></span>

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

<span data-ttu-id="3f050-155">Zárja be az **fő** záró zárójel beszúrásával: **}**</span><span class="sxs-lookup"><span data-stu-id="3f050-155">Close **main** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="3f050-156">Nevű metódus létrehozása **MakeHTMLPage** létrehozni egy egyszerű HTML-lapot.</span><span class="sxs-lookup"><span data-stu-id="3f050-156">Create a method named **MakeHTMLPage** to create a basic HTML page.</span></span> <span data-ttu-id="3f050-157">Ez a módszer típusú paraméterrel rendelkezik **CloudBlobContainer**, amely használható a feltöltött blobon listáját iterációt.</span><span class="sxs-lookup"><span data-stu-id="3f050-157">This method has a parameter of type **CloudBlobContainer**, which will be used to iterate through the list of uploaded blobs.</span></span> <span data-ttu-id="3f050-158">Ez a módszer kivételhibát típusú kivételek **FileNotFoundException**, által is okozhat a **FileOutputStream** konstruktor, és **URISyntaxException**, amely lehet által okozott a **ListBlobItem.getUri** metódust.</span><span class="sxs-lookup"><span data-stu-id="3f050-158">This method will throw exceptions of type **FileNotFoundException**, which can be thrown by the **FileOutputStream** constructor, and **URISyntaxException**, which can be thrown by the **ListBlobItem.getUri** method.</span></span> <span data-ttu-id="3f050-159">A nyitó zárójel tartalmaznak **{**.</span><span class="sxs-lookup"><span data-stu-id="3f050-159">Include the opening bracket, **{**.</span></span>

```java
public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
{
```

<span data-ttu-id="3f050-160">Hozzon létre egy helyi fájlt **index.html**.</span><span class="sxs-lookup"><span data-stu-id="3f050-160">Create a local file named **index.html**.</span></span>

```java
PrintStream stream;
stream = new PrintStream(new FileOutputStream("index.html"));
```

<span data-ttu-id="3f050-161">Hozzáadása a helyi fájlba írni a  **&lt;html&gt;**,  **&lt;fejléc&gt;**, és  **&lt;törzs&gt;**  elemek.</span><span class="sxs-lookup"><span data-stu-id="3f050-161">Write to the local file, adding in the **&lt;html&gt;**, **&lt;header&gt;**, and **&lt;body&gt;** elements.</span></span>

```java
stream.println("<html>");
stream.println("<header/>");
stream.println("<body>");
```

<span data-ttu-id="3f050-162">A feltöltött blobon listáját iterációt.</span><span class="sxs-lookup"><span data-stu-id="3f050-162">Iterate through the list of uploaded blobs.</span></span> <span data-ttu-id="3f050-163">Minden egyes blob, a HTML-weblap hozzon létre egy  **&lt;img&gt;**  elem, amelynek a **src** , mert az az Azure storage-fiók létezik a BLOB URI küldött attribútum.</span><span class="sxs-lookup"><span data-stu-id="3f050-163">For each blob, in the HTML page create an **&lt;img&gt;** element that has its **src** attribute sent to the URI of the blob as it exists in your Azure storage account.</span></span>
<span data-ttu-id="3f050-164">Bár csak egy kép ebben a mintában hozzáadni, ha több hozzáadott, ezt a kódot az összes kellene többször.</span><span class="sxs-lookup"><span data-stu-id="3f050-164">Although you added only one image in this sample, if you added more, this code would iterate all of them.</span></span>

<span data-ttu-id="3f050-165">Az egyszerűség kedvéért ez a példa feltételezi minden feltöltött blobon lemezkép.</span><span class="sxs-lookup"><span data-stu-id="3f050-165">For simplicity, this example assumes each uploaded blob is an image.</span></span> <span data-ttu-id="3f050-166">Ha eltérő képek blobok vagy lapblobokat helyett blokkblobokat frissítette, szükség szerint állítsa be a kódot.</span><span class="sxs-lookup"><span data-stu-id="3f050-166">If you've updated blobs other than images, or page blobs instead of block blobs, adjust the code as needed.</span></span>

```java
// Enumerate the uploaded blobs.
for (ListBlobItem blobItem : container.listBlobs()) {
// List each blob as an <img> element in the HTML body.
stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
}
```

<span data-ttu-id="3f050-167">Zárja be a  **&lt;törzs&gt;**  elem és a  **&lt;html&gt;**  elemet.</span><span class="sxs-lookup"><span data-stu-id="3f050-167">Close the **&lt;body&gt;** element and the **&lt;html&gt;** element.</span></span>

```java
stream.println("</body>");
stream.println("</html>");
```

<span data-ttu-id="3f050-168">Zárja be a helyi fájlt.</span><span class="sxs-lookup"><span data-stu-id="3f050-168">Close the local file.</span></span>

```java
stream.close();
```

<span data-ttu-id="3f050-169">Zárja be az **MakeHTMLPage** záró zárójel beszúrásával: **}**</span><span class="sxs-lookup"><span data-stu-id="3f050-169">Close **MakeHTMLPage** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="3f050-170">Zárja be az **StorageSample** záró zárójel beszúrásával: **}**</span><span class="sxs-lookup"><span data-stu-id="3f050-170">Close **StorageSample** by inserting a close bracket: **}**</span></span>

<span data-ttu-id="3f050-171">A teljes kód látható az ebben a példában a következő:</span><span class="sxs-lookup"><span data-stu-id="3f050-171">The following is the complete code for this example.</span></span> <span data-ttu-id="3f050-172">Ne felejtse el módosítani a helyőrző értékeket **a\_fiók\_neve** és **a\_fiók\_kulcs** a fiók nevére és kulcsára, kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="3f050-172">Remember to modify the placeholder values **your\_account\_name** and **your\_account\_key** to use your account name and account key, respectively.</span></span>

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

<span data-ttu-id="3f050-173">Mellett a kép: helyi fájl feltöltése az Azure storage, a példakód létrehoz egy helyi fájl namedindex.html, a feltöltött kép megjelenítéséhez böngészőben megnyithat.</span><span class="sxs-lookup"><span data-stu-id="3f050-173">In addition to uploading your local image file to Azure storage, the example code creates a local file namedindex.html, which you can open in your browser to see your uploaded image.</span></span>

<span data-ttu-id="3f050-174">A kódot tartalmaz, a fiók nevét és a fiókkulcsot, győződjön meg arról, hogy a forráskód biztonságos.</span><span class="sxs-lookup"><span data-stu-id="3f050-174">Because the code contains your account name and account key, ensure that your source code is secure.</span></span>

## <a name="to-delete-a-container"></a><span data-ttu-id="3f050-175">A tároló törlése</span><span class="sxs-lookup"><span data-stu-id="3f050-175">To delete a container</span></span>
<span data-ttu-id="3f050-176">Van szó, a tárolási, mert előfordulhat, hogy törölni kívánja a **gettingstarted** tároló, miután befejezte az ebben a példában ki.</span><span class="sxs-lookup"><span data-stu-id="3f050-176">Because you are charged for storage, you may want to delete the **gettingstarted** container after you are done experimenting with this example.</span></span> <span data-ttu-id="3f050-177">Egy tároló törléséhez használja a **CloudBlobContainer.delete** metódust.</span><span class="sxs-lookup"><span data-stu-id="3f050-177">To delete a container, use the **CloudBlobContainer.delete** method.</span></span>

```java
container = serviceClient.getContainerReference("gettingstarted");
container.delete();
```

<span data-ttu-id="3f050-178">Hívása a **CloudBlobContainer.delete** metódus, a folyamat inicializálása **CloudStorageAccount**, **ClodBlobClient**, és **CloudBlobContainer**  objektumok megegyezik a látható módon a **createIfNotExist** metódust.</span><span class="sxs-lookup"><span data-stu-id="3f050-178">To call the **CloudBlobContainer.delete** method, the process of initializing **CloudStorageAccount**, **ClodBlobClient**, and **CloudBlobContainer** objects is the same as shown for the **createIfNotExist** method.</span></span> <span data-ttu-id="3f050-179">Az alábbiakban egy teljes példa, amely törli a tárolót egy **gettingstarted**.</span><span class="sxs-lookup"><span data-stu-id="3f050-179">The following is a complete example that deletes the container named **gettingstarted**.</span></span>

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

<span data-ttu-id="3f050-180">Más blob tárolási osztályokhoz és módszerek áttekintését lásd: [Blob storage-ának Java használatával](storage-java-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="3f050-180">For an overview of other blob storage classes and methods, see [How to use Blob storage from Java](storage-java-how-to-use-blob-storage.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f050-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f050-181">Next steps</span></span>
<span data-ttu-id="3f050-182">Az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről.</span><span class="sxs-lookup"><span data-stu-id="3f050-182">Follow these links to learn more about more complex storage tasks.</span></span>

* [<span data-ttu-id="3f050-183">Az Azure Storage Java SDK</span><span class="sxs-lookup"><span data-stu-id="3f050-183">Azure Storage SDK for Java</span></span>](https://github.com/azure/azure-storage-java)
* [<span data-ttu-id="3f050-184">Az Azure Storage ügyfél SDK-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="3f050-184">Azure Storage Client SDK Reference</span></span>](http://dl.windowsazure.com/storage/javadoc/)
* [<span data-ttu-id="3f050-185">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="3f050-185">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [<span data-ttu-id="3f050-186">Az Azure Storage csapat blogja</span><span class="sxs-lookup"><span data-stu-id="3f050-186">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)

