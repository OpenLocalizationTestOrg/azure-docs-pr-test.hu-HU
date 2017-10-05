---
title: "Data Lake Analytics Java SDK használata alkalmazások fejlesztéséhez |} Microsoft Docs"
description: "Az Azure Data Lake Analytics Java SDK használata alkalmazások fejlesztéséhez"
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
ms.openlocfilehash: 795d9ec0b0cac5d74673404f1d0d851393336df0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-java-sdk"></a><span data-ttu-id="8a18a-103">Az Azure Data Lake Analytics használatának első lépései a Java SDK-val</span><span class="sxs-lookup"><span data-stu-id="8a18a-103">Get started with Azure Data Lake Analytics using Java SDK</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="8a18a-104">Útmutató: Azure Data Lake-fiók létrehozása és végezhet el olyan alapvető műveleteket, mint például mappák létrehozása, és töltse le a fájlokat, a fiók törlése, és feltöltése feladatok használata az Azure Data Lake Analytics Java SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="8a18a-104">Learn how to use the Azure Data Lake Analytics Java SDK to create an Azure Data Lake account and perform basic operations such as create folders, upload and download data files, delete your account, and work with jobs.</span></span> <span data-ttu-id="8a18a-105">További információ a Data Lake: [Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8a18a-105">For more information about Data Lake, see [Azure Data Lake Analytics](data-lake-analytics-overview.md).</span></span>

<span data-ttu-id="8a18a-106">Az oktatóanyag során elkészít egy Java-konzolalkalmazást minták a gyakori felügyeleti feladatokat, valamint vizsgálati adatok létrehozás és elküld egy feladatot tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="8a18a-106">In this tutorial, you will develop a Java console application which contains samples of common administrative tasks as well as creating test data and submitting a job.</span></span>  <span data-ttu-id="8a18a-107">Ha ugyanezt az oktatóanyagot más támogatott eszközök használatával szeretné elvégezni, kattintson a szakasz tetején található fülekre.</span><span class="sxs-lookup"><span data-stu-id="8a18a-107">To go through the same tutorial using other supported tools, click the tabs on the top of this section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a18a-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8a18a-108">Prerequisites</span></span>
* <span data-ttu-id="8a18a-109">Java fejlesztői készlet (JDK) 8 (a Java 1.8-as verzióját használja).</span><span class="sxs-lookup"><span data-stu-id="8a18a-109">Java Development Kit (JDK) 8 (using Java version 1.8).</span></span>
* <span data-ttu-id="8a18a-110">IntelliJ vagy egyéb megfelelő Java fejlesztőkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="8a18a-110">IntelliJ or another suitable Java development environment.</span></span> <span data-ttu-id="8a18a-111">Nem kötelező, de ajánlott.</span><span class="sxs-lookup"><span data-stu-id="8a18a-111">This is optional but recommended.</span></span> <span data-ttu-id="8a18a-112">Az alábbi utasítások az IntelliJ-t használják.</span><span class="sxs-lookup"><span data-stu-id="8a18a-112">The instructions below use IntelliJ.</span></span>
* <span data-ttu-id="8a18a-113">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="8a18a-113">**An Azure subscription**.</span></span> <span data-ttu-id="8a18a-114">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8a18a-114">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="8a18a-115">Hozzon létre egy Azure Active Directory- (AAD-) alkalmazást, és kérje le az **ügyfél-azonosítóját**, a **bérlőazonosítóját** és a **kulcsát**.</span><span class="sxs-lookup"><span data-stu-id="8a18a-115">Create an Azure Active Directory (AAD) application and retrieve its **Client ID**, **Tenant ID**, and **Key**.</span></span> <span data-ttu-id="8a18a-116">További információ az AAD-alkalmazásokról és útmutató az ügyfél-azonosító lekéréséhez: [Create Active Directory application and service principal using portal](../azure-resource-manager/resource-group-create-service-principal-portal.md) (Active Directory-alkalmazás és egyszerű szolgáltatás létrehozása a portál használatával).</span><span class="sxs-lookup"><span data-stu-id="8a18a-116">For more information about AAD applications and instructions on how to get a client ID, see [Create Active Directory application and service principal using portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="8a18a-117">A válasz URI és a kulcs a portálon is elérhető lesz az alkalmazás és a kulcs létrehozását követően.</span><span class="sxs-lookup"><span data-stu-id="8a18a-117">The Reply URI and Key will also be available from the portal once you have the application created and key generated.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="8a18a-118">Hogyan végezhető el a hitelesítés az Azure Active Directory használatával?</span><span class="sxs-lookup"><span data-stu-id="8a18a-118">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="8a18a-119">Az alábbi kódrészlet a **nem interaktív** hitelesítéshez biztosít kódot, amely során az alkalmazás maga biztosítja saját hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="8a18a-119">The code snippet below provides code for **non-interactive** authentication, where the application provides its own credentials.</span></span>

<span data-ttu-id="8a18a-120">Ahhoz, hogy az oktatóanyag működjön, engedélyeznie kell az alkalmazásnak, hogy erőforrásokat hozzon létre az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8a18a-120">You will need to give your application permission to create resources in Azure for this tutorial to work.</span></span> <span data-ttu-id="8a18a-121">**Határozottan ajánlott**, hogy a jelen oktatóanyagban csak közreműködői jogosultságokat adjon az alkalmazásnak egy új, nem használt és üres erőforráscsoporthoz az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="8a18a-121">It is **highly recommended** that you only give this application Contributor permissions to a new, unused, and empty resource group in your Azure subscription for the purposes of this tutorial.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="8a18a-122">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8a18a-122">Create a Java application</span></span>
1. <span data-ttu-id="8a18a-123">Nyissa meg az IntelliJ-t, és hozzon létre egy Java-projektet a **Parancssori alkalmazás** sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="8a18a-123">Open IntelliJ and create a new Java project using the **Command Line App** template.</span></span>
2. <span data-ttu-id="8a18a-124">Kattintson a jobb gombbal a képernyő bal oldalán található projektre, majd kattintson az **Add Framework Support** (Keretrendszer-támogatás felvétele) elemre.</span><span class="sxs-lookup"><span data-stu-id="8a18a-124">Right-click on the project on the left-hand side of your screen and click **Add Framework Support**.</span></span> <span data-ttu-id="8a18a-125">Válassza a **Maven** lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8a18a-125">Choose **Maven** and click **OK**.</span></span>
3. <span data-ttu-id="8a18a-126">Nyissa meg az újonnan létrehozott **„pom.xml”** fájlt, majd illessze be a következő szövegrészletet a **\</version>** és a **\</project>** címkék közé:</span><span class="sxs-lookup"><span data-stu-id="8a18a-126">Open the newly created **"pom.xml"** file and add the following snippet of text between the **\</version>** tag and the **\</project>** tag:</span></span>

    >[!NOTE]
    ><span data-ttu-id="8a18a-127">Ez a lépés nem ideiglenes, amíg az Azure Data Lake Analytics SDK elérhető válik a Mavenben.</span><span class="sxs-lookup"><span data-stu-id="8a18a-127">This step is temporary until the Azure Data Lake Analytics SDK is available in Maven.</span></span> <span data-ttu-id="8a18a-128">Amikor az SDK elérhető válik a Mavenben, a cikket frissítjük.</span><span class="sxs-lookup"><span data-stu-id="8a18a-128">This article will be updated once the SDK is available in Maven.</span></span> <span data-ttu-id="8a18a-129">Az SDK minden jövőbeni frissítése elérhető lesz a Mavenen keresztül.</span><span class="sxs-lookup"><span data-stu-id="8a18a-129">All future updates to this SDK will be availble through Maven.</span></span>
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
4. <span data-ttu-id="8a18a-130">Nyissa meg a **fájl**, majd **beállítások**, majd **Build**, **végrehajtási**, **telepítési**.</span><span class="sxs-lookup"><span data-stu-id="8a18a-130">Go to **File**, then **Settings**, then **Build**, **Execution**, **Deployment**.</span></span> <span data-ttu-id="8a18a-131">Válassza ki **buildet**, **Maven**, **importálása**.</span><span class="sxs-lookup"><span data-stu-id="8a18a-131">Select **Build Tools**, **Maven**, **Importing**.</span></span> <span data-ttu-id="8a18a-132">Ezután ellenőrizze **Import Maven projektek automatikusan**.</span><span class="sxs-lookup"><span data-stu-id="8a18a-132">Then check **Import Maven projects automatically**.</span></span>
5. <span data-ttu-id="8a18a-133">Nyissa meg **Main.java** , és cserélje le a meglévő kódblokkot az alábbi kódra.</span><span class="sxs-lookup"><span data-stu-id="8a18a-133">Open **Main.java** and replace the existing code block with the following code.</span></span> <span data-ttu-id="8a18a-134">Emellett adja meg a paraméterek a kódrészletet, többek között **localFolderPath**, **_adlaAccountName**, **_adlsAccountName**, **_resourceGroupName** , és cserélje le a helyőrzőket **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID**, és **SUBSCRIPTION-ID**.</span><span class="sxs-lookup"><span data-stu-id="8a18a-134">Also, provide the values for parameters called out in the code snippet, such as **localFolderPath**, **_adlaAccountName**, **_adlsAccountName**, **_resourceGroupName** and replace placeholders for **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID**, and **SUBSCRIPTION-ID**.</span></span>

    <span data-ttu-id="8a18a-135">Ez a kód végighalad a Data Lake Store és a Data Lake Analytics-fiókokat hozhat létre, -fájlok létrehozásáról a tárolóban, a feladat futtatását, feladat állapota első, feladatkiemenetét letöltése és végül törli a fiókot.</span><span class="sxs-lookup"><span data-stu-id="8a18a-135">This code goes through the process of creating Data Lake Store and Data Lake Analytics accounts, creating files in the store, running a job, getting job status, downloading job output, and finally deleting the account.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8a18a-136">Az Azure Data Lake szolgáltatásnak van egy ismert hibája.</span><span class="sxs-lookup"><span data-stu-id="8a18a-136">There is currently a known issue with the Azure Data Lake Service.</span></span>  <span data-ttu-id="8a18a-137">Ha a példaalkalmazás megszakad vagy hibába ütközik, előfordulhat, hogy manuálisan kell törölnie a szkript által létrehozott Data Lake Store- és Data Lake Analytics-fiókokat.</span><span class="sxs-lookup"><span data-stu-id="8a18a-137">If the sample app is interrupted or encounters an error, you may need to manually delete the Data Lake Store & Data Lake Analytics accounts that the script creates.</span></span>  <span data-ttu-id="8a18a-138">Ha nem ismeri a portált, [Az Azure Data Lake Analytics kezelése az Azure Portal használatával](data-lake-analytics-manage-use-portal.md) útmutatóból elsajátíthatja az első lépéseket.</span><span class="sxs-lookup"><span data-stu-id="8a18a-138">If you're not familiar with the Portal, the [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) guide will get you started.</span></span>
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

                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.

                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.

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
                // TODO: these change order in the next patch
                byte[] bytesContents = "123,abc".getBytes();
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, "/input1.csv", bytesContents, true);

                WaitForNewline("File created.", "Submitting a job.");

                // Submit a job to Data Lake Analytics
                UUID jobId = SubmitJobByScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input TO @\"/output1.csv\" USING Outputters.Csv();", "testJob");
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

            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";

                System.out.println(reason + "\r\nPress ENTER to continue...");
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

                    /* If this line generates an error message like "The deep update for property 'DataLakeStoreAccounts' is not supported", please delete the ADLS and ADLA accounts via the portal and re-run your script. */

                _adlaClient.getAccountOperations().create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }

            //todo: this changes in the next version of the API
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
            // Returns the job ID
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

1. <span data-ttu-id="8a18a-139">Kövesse az utasításokat az alkalmazás futtatásához és befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="8a18a-139">Follow the prompts to run and complete the application.</span></span>

## <a name="see-also"></a><span data-ttu-id="8a18a-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8a18a-140">See also</span></span>
* <span data-ttu-id="8a18a-141">Ha ugyanezt az oktatóanyagot más eszközök használatával szeretné megtekinteni, kattintson az oldal tetején található lapválasztókra.</span><span class="sxs-lookup"><span data-stu-id="8a18a-141">To see the same tutorial using other tools, click the tab selectors on the top of the page.</span></span>
* <span data-ttu-id="8a18a-142">Egy összetettebb lekérdezés megtekintéséhez lásd: [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) (Webhelyek naplóinak elemzése az Azure Data Lake Analytics használatával).</span><span class="sxs-lookup"><span data-stu-id="8a18a-142">To see a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="8a18a-143">Ismerkedés a U-SQL-alkalmazások fejlesztésével: [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) (U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával).</span><span class="sxs-lookup"><span data-stu-id="8a18a-143">To get started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="8a18a-144">A U-SQL nyelv megismerése: [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md) (Ismerkedés az Azure Data Lake Analytics U-SQL nyelvével). és [U-SQL language reference](http://go.microsoft.com/fwlink/?LinkId=691348) (U-SQL nyelvi referencia).</span><span class="sxs-lookup"><span data-stu-id="8a18a-144">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md), and [U-SQL language reference](http://go.microsoft.com/fwlink/?LinkId=691348).</span></span>
* <span data-ttu-id="8a18a-145">Felügyeleti feladatok: [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) (Az Azure Data Lake Analytics kezelése az Azure Portallal).</span><span class="sxs-lookup"><span data-stu-id="8a18a-145">For management tasks, see [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md).</span></span>
* <span data-ttu-id="8a18a-146">A Data Lake Analytics áttekintése: [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Az Azure Data Lake Analytics áttekintése).</span><span class="sxs-lookup"><span data-stu-id="8a18a-146">To get an overview of Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>
