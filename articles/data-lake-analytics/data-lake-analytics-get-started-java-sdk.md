---
title: "aaaUse Data Lake Analytics Java SDK toodevelop alkalmazások |} Microsoft Docs"
description: "Az Azure Data Lake Analytics Java SDK toodevelop alkalmazások használata"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 07830b36-2fe3-4809-a846-129cf67b6a9e
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 0d975812fe659ed34ee9befd37ee7c0bf50d3414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-java-sdk"></a><span data-ttu-id="0cb7e-103">Az Azure Data Lake Analytics használatának első lépései a Java SDK-val</span><span class="sxs-lookup"><span data-stu-id="0cb7e-103">Get started with Azure Data Lake Analytics using Java SDK</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="0cb7e-104">Ismerje meg, hogyan toouse hello Azure Data Lake Analytics Java SDK toocreate Azure Data Lake-fiókot, és alapvető műveleteket, mint például mappák létrehozása, fájlok feltöltését és letöltését adatokat, törölje a fiókját, és a feladatok használatát.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-104">Learn how toouse hello Azure Data Lake Analytics Java SDK toocreate an Azure Data Lake account and perform basic operations such as create folders, upload and download data files, delete your account, and work with jobs.</span></span> <span data-ttu-id="0cb7e-105">További információ a Data Lake: [Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0cb7e-105">For more information about Data Lake, see [Azure Data Lake Analytics](data-lake-analytics-overview.md).</span></span>

<span data-ttu-id="0cb7e-106">Az oktatóanyag során elkészít egy Java-konzolalkalmazást minták a gyakori felügyeleti feladatokat, valamint vizsgálati adatok létrehozás és elküld egy feladatot tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-106">In this tutorial, you will develop a Java console application which contains samples of common administrative tasks as well as creating test data and submitting a job.</span></span>  <span data-ttu-id="0cb7e-107">toogo keresztül hello ugyanezt az oktatóanyagot más használatával támogatott eszközöket, kattintson a hello lapok hello látható az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-107">toogo through hello same tutorial using other supported tools, click hello tabs on hello top of this section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0cb7e-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0cb7e-108">Prerequisites</span></span>
* <span data-ttu-id="0cb7e-109">Java fejlesztői készlet (JDK) 8 (a Java 1.8-as verzióját használja).</span><span class="sxs-lookup"><span data-stu-id="0cb7e-109">Java Development Kit (JDK) 8 (using Java version 1.8).</span></span>
* <span data-ttu-id="0cb7e-110">IntelliJ vagy egyéb megfelelő Java fejlesztőkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-110">IntelliJ or another suitable Java development environment.</span></span> <span data-ttu-id="0cb7e-111">Nem kötelező, de ajánlott.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-111">This is optional but recommended.</span></span> <span data-ttu-id="0cb7e-112">az alábbi hello utasítások az intellij-t használják.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-112">hello instructions below use IntelliJ.</span></span>
* <span data-ttu-id="0cb7e-113">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-113">**An Azure subscription**.</span></span> <span data-ttu-id="0cb7e-114">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0cb7e-114">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="0cb7e-115">Hozzon létre egy Azure Active Directory- (AAD-) alkalmazást, és kérje le az **ügyfél-azonosítóját**, a **bérlőazonosítóját** és a **kulcsát**.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-115">Create an Azure Active Directory (AAD) application and retrieve its **Client ID**, **Tenant ID**, and **Key**.</span></span> <span data-ttu-id="0cb7e-116">További információt az AAD-alkalmazások és utasításokat hogyan tooget egy ügyfél-Azonosítót, lásd: [létrehozása az Active Directory-alkalmazás és szolgáltatás egyszerű portálon](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0cb7e-116">For more information about AAD applications and instructions on how tooget a client ID, see [Create Active Directory application and service principal using portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="0cb7e-117">hello Reply URI és kulcs is elérhető lesz a hello portálról hello alkalmazást hoz létre, és a kulcs jön létre.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-117">hello Reply URI and Key will also be available from hello portal once you have hello application created and key generated.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="0cb7e-118">Hogyan végezhető el a hitelesítés az Azure Active Directory használatával?</span><span class="sxs-lookup"><span data-stu-id="0cb7e-118">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="0cb7e-119">hello kódrészletben biztosít kódját **nem interaktív** hitelesítési, ahol hello alkalmazás maga biztosítja saját hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-119">hello code snippet below provides code for **non-interactive** authentication, where hello application provides its own credentials.</span></span>

<span data-ttu-id="0cb7e-120">Szüksége lesz toogive az alkalmazás engedélyt toocreate erőforrások az Azure-ban ez az oktatóanyag toowork.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-120">You will need toogive your application permission toocreate resources in Azure for this tutorial toowork.</span></span> <span data-ttu-id="0cb7e-121">Az **erősen ajánlott** csak biztosítják az alkalmazás közreműködői engedélyekkel tooa új, nem használt és üres erőforráscsoporthoz az Azure-előfizetéséhez Ez az oktatóanyag hello alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-121">It is **highly recommended** that you only give this application Contributor permissions tooa new, unused, and empty resource group in your Azure subscription for hello purposes of this tutorial.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="0cb7e-122">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0cb7e-122">Create a Java application</span></span>
1. <span data-ttu-id="0cb7e-123">Nyissa meg az intellij-t, és hozzon létre egy új Java-projektet hello segítségével **parancssori alkalmazás** sablont.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-123">Open IntelliJ and create a new Java project using hello **Command Line App** template.</span></span>
2. <span data-ttu-id="0cb7e-124">Kattintson a jobb gombbal a képernyő bal oldalán hello hello projekt, és kattintson a **keretrendszer-támogatás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-124">Right-click on hello project on hello left-hand side of your screen and click **Add Framework Support**.</span></span> <span data-ttu-id="0cb7e-125">Válassza a **Maven** lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-125">Choose **Maven** and click **OK**.</span></span>
3. <span data-ttu-id="0cb7e-126">Nyissa meg az újonnan létrehozott hello **"pom.xml"** fájlt, és adja hozzá a következő szövegrészletet közötti hello hello  **\<version >** címke és hello  **\< /project >** címke:</span><span class="sxs-lookup"><span data-stu-id="0cb7e-126">Open hello newly created **"pom.xml"** file and add hello following snippet of text between hello **\</version>** tag and hello **\</project>** tag:</span></span>

    >[!NOTE]
    ><span data-ttu-id="0cb7e-127">Ez a lépés nem ideiglenes, amíg a hello Azure Data Lake Analytics SDK elérhető válik a Mavenben.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-127">This step is temporary until hello Azure Data Lake Analytics SDK is available in Maven.</span></span> <span data-ttu-id="0cb7e-128">Ez a cikk fog frissülni, amint hello SDK elérhető válik a Mavenben.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-128">This article will be updated once hello SDK is available in Maven.</span></span> <span data-ttu-id="0cb7e-129">Az összes jövőbeli frissítései toothis SDK lesz a mavenen keresztül.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-129">All future updates toothis SDK will be availble through Maven.</span></span>
    >

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
4. <span data-ttu-id="0cb7e-130">Nyissa meg túl**fájl**, majd **beállítások**, majd **Build**, **végrehajtási**, **telepítési**.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-130">Go too**File**, then **Settings**, then **Build**, **Execution**, **Deployment**.</span></span> <span data-ttu-id="0cb7e-131">Válassza ki **buildet**, **Maven**, **importálása**.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-131">Select **Build Tools**, **Maven**, **Importing**.</span></span> <span data-ttu-id="0cb7e-132">Ezután ellenőrizze **Import Maven projektek automatikusan**.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-132">Then check **Import Maven projects automatically**.</span></span>
5. <span data-ttu-id="0cb7e-133">Nyissa meg **Main.java** és a következő kód csere hello meglévő kódblokkot az hello.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-133">Open **Main.java** and replace hello existing code block with hello following code.</span></span> <span data-ttu-id="0cb7e-134">Emellett hello értéket ad a paraméterek hello kódrészletet, például a **localFolderPath**, **_adlaAccountName**, **_adlsAccountName**, **_ resourceGroupName** , és cserélje le a helyőrzőket **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID**, és  **ELŐFIZETÉS-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-134">Also, provide hello values for parameters called out in hello code snippet, such as **localFolderPath**, **_adlaAccountName**, **_adlsAccountName**, **_resourceGroupName** and replace placeholders for **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID**, and **SUBSCRIPTION-ID**.</span></span>

    <span data-ttu-id="0cb7e-135">Ez a kód kerül hello folyamatot, amely fájlok létrehozásával hello tárolóban, Data Lake Store és a Data Lake Analytics fiókokat hozhat létre fut egy feladat, feladat állapota első, feladatkiemenetét letöltése és végül a hello fiók törlése.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-135">This code goes through hello process of creating Data Lake Store and Data Lake Analytics accounts, creating files in hello store, running a job, getting job status, downloading job output, and finally deleting hello account.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0cb7e-136">Jelenleg egy ismert probléma az Azure Data Lake szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-136">There is currently a known issue with hello Azure Data Lake Service.</span></span>  <span data-ttu-id="0cb7e-137">Ha hello mintaalkalmazás megszakad, vagy hibát észlel, szükség lehet a toomanually delete hello Data Lake Store & Data Lake Analytics fiókok hello parancsfájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-137">If hello sample app is interrupted or encounters an error, you may need toomanually delete hello Data Lake Store & Data Lake Analytics accounts that hello script creates.</span></span>  <span data-ttu-id="0cb7e-138">Ha még nem ismeri a hello Portal, hello [kezelése Azure Data Lake Analytics Azure portál használatával](data-lake-analytics-manage-use-portal.md) útmutató segítséget.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-138">If you're not familiar with hello Portal, hello [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) guide will get you started.</span></span>
   >
   >

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

                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring hello application client secret, rather than hard-coding it in hello source code.

                String localFolderPath = "C:\\local_path\\"; // TODO: Change this tooany unused, new, empty folder on your local machine.

                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);

                // Create Data Lake Store and Analytics accounts
                WaitForNewline("Authenticated.", "Creating NEW accounts.");
                CreateAccounts();
                WaitForNewline("Accounts created.", "Displaying accounts.");

                // List Data Lake Store and Analytics accounts that this app can access
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

                // Create a file in Data Lake Store: input1.csv
                // TODO: these change order in hello next patch
                byte[] bytesContents = "123,abc".getBytes();
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, "/input1.csv", bytesContents, true);

                WaitForNewline("File created.", "Submitting a job.");

                // Submit a job tooData Lake Analytics
                UUID jobId = SubmitJobByScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input too@\"/output1.csv\" USING Outputters.Csv();", "testJob");
                WaitForNewline("Job submitted.", "Getting job status.");

                // Wait for job completion and output job status
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                System.out.println("Waiting for job completion.");
                WaitForJob(jobId);
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                WaitForNewline("Job completed.", "Downloading job output.");

                // Download job output from Data Lake Store
                DownloadFile("/output1.csv", localFolderPath + "output1.csv");
                WaitForNewline("Job output downloaded.", "Deleting file.");

                // Delete file from Data Lake Store
                DeleteFile("/output1.csv");
                WaitForNewline("File deleted.", "Deleting account.");

                // Delete account
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
                _adlaClient.getAccountOperations().delete(_resourceGroupName, _adlaAccountName);
                WaitForNewline("Account deleted.", "DONE.");
            }

            //Set up clients
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

            // Helper function tooshow status and wait for user input
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

            // Create Data Lake Store and Analytics accounts
            public static void CreateAccounts() throws InterruptedException, CloudException, IOException {
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

                    /* If this line generates an error message like "hello deep update for property 'DataLakeStoreAccounts' is not supported", please delete hello ADLS and ADLA accounts via hello portal and re-run your script. */

                _adlaClient.getAccountOperations().create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }

            //todo: this changes in hello next version of hello API
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }

            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(filePath, _adlsAccountName);
            }

            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
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

            // Submit a U-SQL job by providing script contents.
            // Returns hello job ID
            public static UUID SubmitJobByScript(String script, String jobName) throws IOException, CloudException {
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
            public static JobResult WaitForJob(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                while (jobInfo.getState() != JobState.ENDED)
                {
                    jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName,jobId).getBody();
                }
                return jobInfo.getResult();
            }

            // Get job status
            public static String GetJobStatus(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                return jobInfo.getState().toValue();
            }
        }

1. <span data-ttu-id="0cb7e-139">Hajtsa végre a hello kér toorun és teljes hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-139">Follow hello prompts toorun and complete hello application.</span></span>

## <a name="see-also"></a><span data-ttu-id="0cb7e-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0cb7e-140">See also</span></span>
* <span data-ttu-id="0cb7e-141">toosee hello ugyanaz az oktatóanyagot más eszközök használatával hello szeretné a hello hello lap tetején kattintson.</span><span class="sxs-lookup"><span data-stu-id="0cb7e-141">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>
* <span data-ttu-id="0cb7e-142">egy összetettebb lekérdezés toosee lásd [elemzés webhely naplózza az Azure Data Lake Analytics használatával](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="0cb7e-142">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="0cb7e-143">megkezdődött a U-SQL-alkalmazások fejlesztésével tooget lásd [Data Lake Tools for Visual Studio használatával fejlesztése U-SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0cb7e-143">tooget started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="0cb7e-144">toolearn U-SQL, lásd: [Ismerkedés az Azure Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md), és [U-SQL nyelvi referencia](http://go.microsoft.com/fwlink/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="0cb7e-144">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md), and [U-SQL language reference](http://go.microsoft.com/fwlink/?LinkId=691348).</span></span>
* <span data-ttu-id="0cb7e-145">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure portállal).</span><span class="sxs-lookup"><span data-stu-id="0cb7e-145">For management tasks, see [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md).</span></span>
* <span data-ttu-id="0cb7e-146">a Data Lake Analytics áttekintésének tooget lásd [Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0cb7e-146">tooget an overview of Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>
