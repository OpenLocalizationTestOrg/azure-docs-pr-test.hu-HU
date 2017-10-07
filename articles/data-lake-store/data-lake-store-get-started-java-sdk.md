---
title: "aaaUse hello Java SDK toodevelop alkalmazások az Azure Data Lake Store |} Microsoft Docs"
description: "Azure Data Lake Store Java SDK toocreate Data Lake Store-fiók és alapszintű műveletek végrehajtása a Data Lake Store hello"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/28/2017
ms.author: nitinme
ms.openlocfilehash: d3bcee449c2a2a4bd2f7b241af46ecc010b6b62e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-java"></a>Az Azure Data Lake Store használatának első lépései a Java használatával
> [!div class="op_single_selector"]
> * [Portál](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Ismerje meg, hogyan toouse hello Azure Data Lake Store Java SDK tooperform alapvető műveleteket, mint létre mappák, le- és feltöltése az adatfájlok stb. További információk a Data Lake-ről: [Azure Data Lake Store](data-lake-store-overview.md).

Érheti el az Azure Data Lake Store: hello Java SDK API docs [Azure Data Lake Store Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Előfeltételek
* Java-fejlesztőkészlet (JDK 7 vagy újabb, Java 1.7 vagy újabb verzió használatával)
* Azure Data Lake Store-fiók. Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). Ez az oktatóanyag a Mavent használja a build- és projektfüggőségek kezeléséhez. Bár lehetséges toobuild nélkül Maven vagy a gradle-lel build rendszert használ, akkor ezek rendszerek győződjön sokkal könnyebb toomanage függőségek.
* (Nem kötelező) [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) vagy hasonló integrált fejlesztőkörnyezet.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hogyan végezhető el a hitelesítés az Azure Active Directory használatával?
Ez az oktatóanyag egy Azure AD alkalmazás ügyfél titkos tooretrieve az Azure Active Directory-tokent (szolgáltatások közötti hitelesítés) használjuk. A token toocreate egy Data Lake Store ügyfél tooperform műveletek fájlt és a directory-műveletek használjuk. Hogyan Azure Data Lake Store használatát tooauthenticate hello ügyfélkulcs konfigurálásáról végezzük hello magas szintű lépéseket:

1. Azure AD-webalkalmazás létrehozása
2. Hello ügyfél-azonosító, a titkos ügyfélkulcs és a hello Azure AD-webalkalmazás token-végpont beolvasása.
3. Data Lake Store mappára vagy fájlra, amelyet a Java-alkalmazás létrehozásakor hello tooaccess hello hozzáférés hello Azure AD-webalkalmazás konfigurálása

Hogyan tooperform ezeket a lépéseket: kapcsolatos utasításokat [hozzon létre egy Active Directory-alkalmazást](data-lake-store-authenticate-using-active-directory.md).

Az Azure Active Directory nyújt más lehetőségek is tooretrieve jogkivonatot. Választhatja ki a számos különböző hitelesítési mechanizmusok toosuit helyzetnek, például egy futó böngészővel, egy asztali alkalmazás elosztott alkalmazás vagy egy kiszolgáló helyszíni alkalmazást vagy egy Azure virtuális alkalmazás gép. Továbbá különböző típusú hitelesítő adatok közül választhat, például jelszavak, tanúsítványok, kétfaktoros hitelesítés, stb. Emellett Azure Active Directory lehetővé teszi a toosynchronize a helyszíni Active Directory-felhasználók hello felhővel. További információt a [Hitelesítési eljárások az Azure Active Directoryhoz](../active-directory/active-directory-authentication-scenarios.md) című témakörben talál. 

## <a name="create-a-java-application"></a>Java-alkalmazás létrehozása
rendelkezésre álló hello kódminta [a Githubon](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) bemutatja, hogyan hello folyamatot, amely fájlok hello tárolóban, összefűzi, letölti a fájlokat, fájlok létrehozása és törlése néhány hello tárolójában. Ez a szakasz hello cikk ismerteti az hello kód legfontosabb részeit hello keresztül.

1. Hozzon létre egy Maven project az [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) hello parancssori vagy az IDE használatával. Hogyan toocreate Java projekt IntelliJ, lásd: [Itt](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Útmutatást a projekt eclipse-ben programmal toocreate lásd [Itt](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 
2. Adja hozzá a következő függőségek tooyour Maven hello **pom.xml** fájlt. Adja hozzá a következő szövegrészletet közötti hello hello  **\<version >** címke és hello  **\</project >** címke:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.5</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    hello első függőség toouse hello Data Lake Store SDK (`azure-data-lake-store-sdk`) adattárból hello maven. második függőségi hello (`slf4j-nop`) van toospecify mely naplózási keretrendszer toouse ehhez az alkalmazáshoz. hello Data Lake Store SDK használ [slf4j](http://www.slf4j.org/) naplózási homlokzati, amely lehetővé teszi, hogy számos népszerű naplózási keretrendszert, például a log4j, válassza a Java-naplózás, logback, stb., vagy nem. Ehhez a példához letiltjuk a naplózást, ezért használjuk hello **slf4j-nop** kötés. toouse más naplózási beállításokat az alkalmazásban, lásd: [Itt](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-hello-application-code"></a>Hello alkalmazáskód hozzáadása
Három fő részből áll toohello kódot.

1. Hello Azure Active Directory jogkivonat beszerzése
2. Hello token toocreate egy Data Lake Store ügyfél használja.
3. Hello Data Lake Store Ügyfélműveletek tooperform használja.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>1. lépés: Az Azure Active Directory-jogkivonat beszerzése.
Data Lake Store SDK hello biztosít, amelyekkel kezelheti a hello biztonsági jogkivonatokat kényelmes módszert tootalk toohello Data Lake Store-fiók szükséges. Azonban hello SDK nem határozza meg, hogy a metódusokról csak használható. Is használhatja, sem egyéb módszerrel lehet beszerezni a token is, például használjanak hello [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), vagy egyéni kódot.

Data Lake Store SDK tooobtain token hello Active Directory webalkalmazás korábban létrehozott toouse hello valamelyikével hello alosztályokat `AccessTokenProvider` (használja az alábbi hello példában `ClientCredsTokenProvider`). hello jogkivonat-szolgáltató gyorsítótárak hello hitelesítő adatok tooobtain hello lexikális elem szerepel a memória, és automatikusan megújítja hello tokent, ha tooexpire szól. A saját alosztályokat lehetséges toocreate `AccessTokenProvider` , jogkivonatok akkor kapja meg, amelyet a felhasználói kód, de most hozzuk csak használata hello egy megadott hello SDK.

Cserélje le **FILL Itt** értékekkel hello tényleges hello Azure Active Directory webes alkalmazást.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>2. lépés: Az Azure Data Lake Store-ügyfél (ADLStoreClient) objektumának létrehozása
Létrehozása egy [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) objektum szükséges toospecify hello Data Lake Store fiók nevét és hello jogkivonat-szolgáltató hello előző lépésben létrehozott. Vegye figyelembe, hogy hello Data Lake Store-fiók nevének kell toobe egy teljesen minősített tartománynevét. A **FILL-IN-HERE** értéket például a következő tartománynévre cserélheti ki: **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just hello account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-hello-adlstoreclient-tooperform-file-and-directory-operations"></a>3. lépés: Hello ADLStoreClient tooperform fájlokra és könyvtárakra műveletek használata
hello kódot az alábbi példa kódtöredékek néhány gyakori műveletek tartalmazza. Megnézheti teljes hello [Data Lake Store Java SDK API-dokumentumok](https://azure.github.io/azure-data-lake-store-java/javadoc/) a hello **ADLStoreClient** objektum toosee más műveleteket.

Vegye figyelmembe, hogy a fájlok olvasása és írása szabványos Java-adatfolyamok használatával történik. Ez azt jelenti, hogy tud-e réteg hello Java adatfolyamok fölött hello Data Lake Store az adatfolyamokat, a standard Java funkciók (például nyomtató adatfolyamok formázott vagy bármely hello tömörítés és titkosítás adatfolyamokat a további funkciók toobenefit bármelyikét "felső, stb.).

     // create file and write some content
     String filename = "/a/b/c.txt";
     OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
     PrintStream out = new PrintStream(stream);
     for (int i = 1; i <= 10; i++) {
         out.println("This is line #" + i);
         out.format("This is hello same line (%d), but using formatted output. %n", i);
     }
     out.close();
    
    // set file permission
    client.setPermission(filename, "744");

    // append toofile
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate hello two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename hello file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all hello subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-hello-application"></a>4. lépés: Build és hello alkalmazás futtatása
1. egy IDE belül a toorun keresse meg, és nyomja le az ENTER hello **futtatása** gombra. a Maven használata toorun [exec: exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).
2. egy önálló jar, amelyek futtatásához a parancssori hello jar tartalmazza, minden függőségekkel rendelkező hello segítségével tooproduce [Maven szerelvény beépülő modul](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). a hello pom.xml hello [példa forráskód a githubon](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) példa bemutatja, hogyan van toodo ez.

## <a name="next-steps"></a>Következő lépések
* [Hello Java SDK JavaDoc fel](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)

