---
title: "Azure Data Lake Analytics használatának Azure Java SDK aaaManage |} Microsoft Docs"
description: "Az Azure Data Lake Analytics Java SDK toodevelop alkalmazások használata"
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: 07830b36-2fe3-4809-a846-129cf67b6a9e
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: saveenr
ms.openlocfilehash: 79e5fa1bacd5fd65072a1c3c480482a8e51d94b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage--azure-data-lake-analytics-using-java-sdk"></a><span data-ttu-id="84c57-103">Azure Data Lake Analytics Java SDK használatával kezelése</span><span class="sxs-lookup"><span data-stu-id="84c57-103">Manage  Azure Data Lake Analytics using Java SDK</span></span>

<span data-ttu-id="84c57-104">Ebben az oktatóanyagban fejlesztése egy Java-konzolalkalmazást, amely az Azure Data Lake-közös műveletet hajtanak végre.</span><span class="sxs-lookup"><span data-stu-id="84c57-104">In this tutorial, you develop a Java console application that performs common operations for Azure Data Lake.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84c57-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84c57-105">Prerequisites</span></span>
* <span data-ttu-id="84c57-106">**Java fejlesztői készlet (JDK) 8** (a Java 1.8-as verzióját).</span><span class="sxs-lookup"><span data-stu-id="84c57-106">**Java Development Kit (JDK) 8** (using Java version 1.8).</span></span>
* <span data-ttu-id="84c57-107">**Az IntelliJ** vagy egy másik megfelelő Java fejlesztőkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="84c57-107">**IntelliJ** or another suitable Java development environment.</span></span> <span data-ttu-id="84c57-108">Ebben a dokumentumban hello utasítások az intellij-t használják.</span><span class="sxs-lookup"><span data-stu-id="84c57-108">hello instructions in this document use IntelliJ.</span></span>
* <span data-ttu-id="84c57-109">Hozzon létre egy Azure Active Directory- (AAD-) alkalmazást, és kérje le az **ügyfél-azonosítóját**, a **bérlőazonosítóját** és a **kulcsát**.</span><span class="sxs-lookup"><span data-stu-id="84c57-109">Create an Azure Active Directory (AAD) application and retrieve its **Client ID**, **Tenant ID**, and **Key**.</span></span> <span data-ttu-id="84c57-110">További információt az AAD-alkalmazások és utasításokat hogyan tooget egy ügyfél-Azonosítót, lásd: [létrehozása az Active Directory-alkalmazás és szolgáltatás egyszerű portálon](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="84c57-110">For more information about AAD applications and instructions on how tooget a client ID, see [Create Active Directory application and service principal using portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="84c57-111">hello Reply URI és kulcs érhető hello portálról hello alkalmazást hoz létre, és a kulcs jön létre.</span><span class="sxs-lookup"><span data-stu-id="84c57-111">hello Reply URI and Key is available from hello portal once you have hello application created and key generated.</span></span>

## <a name="authenticating-using-azure-active-directory"></a><span data-ttu-id="84c57-112">Hitelesítés az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="84c57-112">Authenticating using Azure Active Directory</span></span>

<span data-ttu-id="84c57-113">következő kódrészlettel biztosít kódot a kód hello **nem interaktív** hitelesítési, ahol hello alkalmazás maga biztosítja saját hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="84c57-113">hello code following snippet provides code for **non-interactive** authentication, where hello application provides its own credentials.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="84c57-114">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="84c57-114">Create a Java application</span></span>
1. <span data-ttu-id="84c57-115">Nyissa meg az intellij-t, és hozzon létre egy Java-projektet hello segítségével **parancssori alkalmazás** sablont.</span><span class="sxs-lookup"><span data-stu-id="84c57-115">Open IntelliJ and create a Java project using hello **Command-Line App** template.</span></span>
2. <span data-ttu-id="84c57-116">Kattintson a jobb gombbal a képernyő bal oldalán hello hello projekt, és kattintson a **keretrendszer-támogatás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="84c57-116">Right-click on hello project on hello left-hand side of your screen and click **Add Framework Support**.</span></span> <span data-ttu-id="84c57-117">Válassza a **Maven** lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="84c57-117">Choose **Maven** and click **OK**.</span></span>
3. <span data-ttu-id="84c57-118">Nyissa meg az újonnan létrehozott hello **"pom.xml"** fájlt, és adja hozzá a következő szövegrészletet közötti hello hello  **\<version >** címke és hello  **\< /project >** címke:</span><span class="sxs-lookup"><span data-stu-id="84c57-118">Open hello newly created **"pom.xml"** file and add hello following snippet of text between hello **\</version>** tag and hello **\</project>** tag:</span></span>

```
<repositories>
    <repository>
        <id>adx-snapshots</id>
        <name>Azure ADX Snapshots</name>
        <url>http://adxsnapshots.azurewebsites.net/</url>
        <layout>default</layout>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
    <repository>
        <id>oss-snapshots</id>
        <name>Open Source Snapshots</name>
        <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
        <layout>default</layout>
        <snapshots>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
        </snapshots>
    </repository>
</repositories>
<dependencies>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-client-authentication</artifactId>
        <version>1.0.0-20160513.000802-24</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-client-runtime</artifactId>
        <version>1.0.0-20160513.000812-28</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.rest</groupId>
        <artifactId>client-runtime</artifactId>
        <version>1.0.0-20160513.000825-29</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-mgmt-datalake-store</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-mgmt-datalake-analytics</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </dependency>
</dependencies>
```

<span data-ttu-id="84c57-119">Nyissa meg túl**fájl > Beállítások > Build > végrehajtási > telepítési**.</span><span class="sxs-lookup"><span data-stu-id="84c57-119">Go too**File > Settings > Build > Execution > Deployment**.</span></span> <span data-ttu-id="84c57-120">Válassza ki **buildet > Maven > importálása**.</span><span class="sxs-lookup"><span data-stu-id="84c57-120">Select **Build Tools > Maven > Importing**.</span></span> <span data-ttu-id="84c57-121">Ezután ellenőrizze **Import Maven projektek automatikusan**.</span><span class="sxs-lookup"><span data-stu-id="84c57-121">Then check **Import Maven projects automatically**.</span></span>

<span data-ttu-id="84c57-122">Nyissa meg `Main.java` és csere hello meglévő kódblokkot hello a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="84c57-122">Open `Main.java` and replace hello existing code block with hello following code snippet:</span></span>

```
package com.company;

import com.microsoft.azure.CloudException;
import com.microsoft.azure.credentials.ApplicationTokenCredentials;
import com.microsoft.azure.management.datalake.store.*;
import com.microsoft.azure.management.datalake.store.models.*;
import com.microsoft.azure.management.datalake.analytics.*;
import com.microsoft.azure.management.datalake.analytics.models.*;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import java.io.*;
import java.nio.charset.Charset;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.UUID;
import java.util.List;

public class Main {
    private static String _adlsAccountName;
    private static String _adlaAccountName;
    private static String _resourceGroupName;
    private static String _location;

    private static String _tenantId;
    private static String _subId;
    private static String _clientId;
    private static String _clientSecret;

    private static DataLakeStoreAccountManagementClient _adlsClient;
    private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
    private static DataLakeAnalyticsAccountManagementClient _adlaClient;
    private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
    private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;

    public static void main(String[] args) throws Exception {

        _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
        _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>";
        _resourceGroupName = "<RESOURCE-GROUP-NAME>";
        _location = "East US 2";

        _tenantId = "<TENANT-ID>";
        _subId =  "<SUBSCRIPTION-ID>";
        _clientId = "<CLIENT-ID>";

        _clientSecret = "<CLIENT-SECRET>"; 
        
        String localFolderPath = "C:\\local_path\\"; 
        
        // ----------------------------------------
        // Authenticate
        // ----------------------------------------
        ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
        SetupClients(creds);

        // ----------------------------------------
        // List Data Lake Store and Analytics accounts that this app can access
        // ----------------------------------------
        System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
        List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
        for (DataLakeStoreAccount acct : adlsListResult) {
            System.out.println(acct.getName());
        }
        
        System.out.println(String.format("All ADL Analytics accounts that this app can access in subscription %s:", _subId));
        List<DataLakeAnalyticsAccount> adlaListResult = _adlaClient.getAccountOperations().list().getBody();
        for (DataLakeAnalyticsAccount acct : adlaListResult) {
            System.out.println(acct.getName());
        }
        WaitForNewline("Accounts displayed.", "Creating files.");

        // ----------------------------------------
        // Create a file in Data Lake Store: input1.csv
        // ----------------------------------------

        byte[] bytesContents = "123,abc".getBytes();
        _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, "/input1.csv", bytesContents, true);

        WaitForNewline("File created.", "Submitting a job.");

        // ----------------------------------------
        // Submit a job tooData Lake Analytics
        // ----------------------------------------

string script = "@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input too@\"/output1.csv\" USING Outputters.Csv();", "testJob";
        UUID jobId = SubmitJobByScript(script);
        WaitForNewline("Job submitted.", "Getting job status.");

        // ----------------------------------------
        // Wait for job completion and output job status
        // ----------------------------------------
        System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
        System.out.println("Waiting for job completion.");
        WaitForJob(jobId);
        System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
        WaitForNewline("Job completed.", "Downloading job output.");

        // ----------------------------------------
        // Download job output from Data Lake Store
        // ----------------------------------------
        DownloadFile("/output1.csv", localFolderPath + "output1.csv");
        WaitForNewline("Job output downloaded.", "Deleting file.");

    }
}
```

<span data-ttu-id="84c57-123">Adja meg hello paraméterek hello kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="84c57-123">Provide hello values for parameters called out in hello code snippet:</span></span>
* `localFolderPath`
* `_adlaAccountName`
* `_adlsAccountName`
* `_resourceGroupName`

<span data-ttu-id="84c57-124">Cserélje le a helyőrzőket hello:</span><span class="sxs-lookup"><span data-stu-id="84c57-124">Replace hello placeholders for:</span></span>
* <span data-ttu-id="84c57-125">`CLIENT-ID`,</span><span class="sxs-lookup"><span data-stu-id="84c57-125">`CLIENT-ID`,</span></span>
* <span data-ttu-id="84c57-126">`CLIENT-SECRET`,</span><span class="sxs-lookup"><span data-stu-id="84c57-126">`CLIENT-SECRET`,</span></span>
* `TENANT-ID`
* `SUBSCRIPTION-ID`

## <a name="helper-functions"></a><span data-ttu-id="84c57-127">Segédfüggvények találhatók</span><span class="sxs-lookup"><span data-stu-id="84c57-127">Helper functions</span></span>

### <a name="setup-clients"></a><span data-ttu-id="84c57-128">A telepítő ügyfelek</span><span class="sxs-lookup"><span data-stu-id="84c57-128">Setup clients</span></span>

```
public static void SetupClients(ServiceClientCredentials creds)
{
    _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
    _adlaClient = new DataLakeAnalyticsAccountManagementClientImpl(creds);
    _adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);
    _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClientImpl(creds);
    _adlsClient.setSubscriptionId(_subId);
    _adlaClient.setSubscriptionId(_subId);
}
```


### <a name="wait-for-input"></a><span data-ttu-id="84c57-129">Várjon, amíg a bevitelhez</span><span class="sxs-lookup"><span data-stu-id="84c57-129">Wait for input</span></span>

```
public static void WaitForNewline(String reason, String nextAction)
{
    if (nextAction == null)
        nextAction = "";

    System.out.println(reason + "\r\nPress ENTER toocontinue...");
    try{System.in.read();}
    catch(Exception e){}

    if (!nextAction.isEmpty())
    {
        System.out.println(nextAction);
    }
}
```

### <a name="create-accounts"></a><span data-ttu-id="84c57-130">Fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="84c57-130">Create accounts</span></span>

```
public static void CreateAccounts() throws InterruptedException, CloudException, IOException 
{
    // Create ADLS account
    DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
    adlsParameters.setLocation(_location);

    _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);

    // Create ADLA account
    DataLakeStoreAccountInfo adlsInfo = new DataLakeStoreAccountInfo();
    adlsInfo.setName(_adlsAccountName);

    DataLakeStoreAccountInfoProperties adlsInfoProperties = new DataLakeStoreAccountInfoProperties();
    adlsInfo.setProperties(adlsInfoProperties);

    List<DataLakeStoreAccountInfo> adlsInfoList = new ArrayList<DataLakeStoreAccountInfo>();
    adlsInfoList.add(adlsInfo);

    DataLakeAnalyticsAccountProperties adlaProperties = new DataLakeAnalyticsAccountProperties();
    adlaProperties.setDataLakeStoreAccounts(adlsInfoList);
    adlaProperties.setDefaultDataLakeStoreAccount(_adlsAccountName);

    DataLakeAnalyticsAccount adlaParameters = new DataLakeAnalyticsAccount();
    adlaParameters.setLocation(_location);
    adlaParameters.setName(_adlaAccountName);
    adlaParameters.setProperties(adlaProperties);

    _adlaClient.getAccountOperations().create(_resourceGroupName, _adlaAccountName, adlaParameters);
}
```

### <a name="create-a-file"></a><span data-ttu-id="84c57-131">Hozzon létre egy fájlt</span><span class="sxs-lookup"><span data-stu-id="84c57-131">Create a file</span></span>

```
public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException 
{
    byte[] bytesContents = contents.getBytes();

    _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
}
```

### <a name="delete-a-file"></a><span data-ttu-id="84c57-132">Fájl törlése</span><span class="sxs-lookup"><span data-stu-id="84c57-132">Delete a file</span></span>

```
public static void DeleteFile(String filePath) throws IOException, CloudException 
{
    _adlsFileSystemClient.getFileSystemOperations().delete(filePath, _adlsAccountName);
}
```

### <a name="download-a-file"></a><span data-ttu-id="84c57-133">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="84c57-133">Download a file</span></span>

```
public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException 
{
    InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(srcPath, _adlsAccountName).getBody();

    PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());

    String fileContents = "";
    if (stream != null) {
        Writer writer = new StringWriter();

        char[] buffer = new char[1024];
        try {
            Reader reader = new BufferedReader(
                    new InputStreamReader(stream, "UTF-8"));
            int n;
            while ((n = reader.read(buffer)) != -1) {
                writer.write(buffer, 0, n);
            }
        } finally {
            stream.close();
        }
        fileContents =  writer.toString();
    }

    pWriter.println(fileContents);
    pWriter.close();
}
```

### <a name="submit-a-u-sql-job"></a><span data-ttu-id="84c57-134">U-SQL-feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="84c57-134">Submit a U-SQL job</span></span>

```
public static UUID SubmitJobByScript(String script, String jobName) throws IOException, CloudException 
{
    UUID jobId = java.util.UUID.randomUUID();
    USqlJobProperties properties = new USqlJobProperties();
    properties.setScript(script);
    JobInformation parameters = new JobInformation();
    parameters.setName(jobName);
    parameters.setJobId(jobId);
    parameters.setType(JobType.USQL);
    parameters.setProperties(properties);

    JobInformation jobInfo = _adlaJobClient.getJobOperations().create(_adlaAccountName, jobId, parameters).getBody();

    return jobId;
}

// Wait for job completion
public static JobResult WaitForJob(UUID jobId) throws IOException, CloudException 
{
    JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
    while (jobInfo.getState() != JobState.ENDED)
    {
        jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName,jobId).getBody();
    }
    return jobInfo.getResult();
}
```

### <a name="retrieve-job-status"></a><span data-ttu-id="84c57-135">Feladat állapotának lekérése</span><span class="sxs-lookup"><span data-stu-id="84c57-135">Retrieve job status</span></span>

```
public static String GetJobStatus(UUID jobId) throws IOException, CloudException 
{
    JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
    return jobInfo.getState().toValue();
}
```

## <a name="next-steps"></a><span data-ttu-id="84c57-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84c57-136">Next steps</span></span>

* <span data-ttu-id="84c57-137">toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md), és [U-SQL nyelvi referencia](http://go.microsoft.com/fwlink/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="84c57-137">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md), and [U-SQL language reference](http://go.microsoft.com/fwlink/?LinkId=691348).</span></span>
* <span data-ttu-id="84c57-138">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).</span><span class="sxs-lookup"><span data-stu-id="84c57-138">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
* <span data-ttu-id="84c57-139">a Data Lake Analytics áttekintésének tooget lásd [Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="84c57-139">tooget an overview of Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>
