---
title: "aaaCreate HDInsight-fürtök a Data Lake Store alapértelmezett tárolóként PowerShell használatával |} Microsoft Docs"
description: "Azure PowerShell toocreate igénybe, valamint a HDInsight-fürtök az Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/08/2017
ms.author: nitinme
ms.openlocfilehash: a5c0ad416da6ad9bd07204af2ebb6b7470916085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a>HDInsight-fürtök létrehozásához a Data Lake Store alapértelmezett tárolóként PowerShell használatával
> [!div class="op_single_selector"]
> * [Hello Azure portál használata](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [A PowerShell (az alapértelmezett tároló)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Használja a Powershellt (tárhely)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Erőforrás-kezelő használata](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Ismerje meg, hogyan toouse Azure PowerShell tooconfigure Azure HDInsight-fürtök az Azure Data Lake Store, alapértelmezett tárolóként. A HDInsight-fürtök létrehozása a Data Lake Store további tárolóként útmutatásért lásd: [HDInsight-fürtök létrehozása a Data Lake Store további tárolóként](data-lake-store-hdinsight-hadoop-use-powershell.md).

Az alábbiakban a HDInsight a Data Lake Store használatára vonatkozó szempontokat:

* hello beállítás toocreate a HDInsight-fürtök hozzáféréssel rendelkező tooData Lake Store alapértelmezett tárolóként esetén HDInsight 3.5-ös és 3.6 verzió érhető el.

* a HDInsight-fürtök hello beállítás toocreate tooData Lake Store férhetnek hozzá, mint a alapértelmezett tároló *nem érhető el* a HDInsight prémium fürtök.

tooconfigure HDInsight toowork a Data Lake Store powershellel, kövesse a következő öt szakaszok hello hello utasításait.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag megkezdése előtt győződjön meg arról, hogy teljesülnek-e hello követelményeknek:

* **Azure-előfizetés**: Nyissa meg túl[beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* **Az Azure PowerShell 1.0-ás vagy újabb**: lásd: [hogyan tooinstall, és konfigurálja a PowerShell](/powershell/azure/overview).
* **A Windows Software Development Kit (SDK)**: tooinstall Windows SDK-t, lépjen túl[letölti és a Windows 10-eszközök](https://dev.windows.com/en-us/downloads). hello SDK használt toocreate biztonsági tanúsítvány.
* **Az Azure Active Directory szolgáltatás egyszerű**: Ez az oktatóanyag leírja, hogyan toocreate egy egyszerű szolgáltatást az Azure Active Directory (Azure AD). Azonban toocreate egy egyszerű szolgáltatást, kell lennie az Azure AD-rendszergazda. Ha Ön rendszergazda, akkor hagyja ki ezt az előfeltételt, és folytassa a hello oktatóanyag.

    >[!NOTE]
    >Létrehozhat egy szolgáltatás egyszerű csak akkor, ha az Azure AD-rendszergazdaként. Az Azure AD rendszergazdának létre kell hoznia egy szolgáltatás egyszerű HDInsight-fürtök létrehozása a Data Lake Store előtt. hello szolgáltatás egyszerű léteznie kell a tanúsítvánnyal leírtak [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).
    >

## <a name="create-a-data-lake-store-account"></a>Data Lake Store-fiók létrehozása
Data Lake Store-fiók, toocreate hello a következő:

1. Az asztalon nyisson meg egy PowerShell-ablakot, és adja meg az alábbi kódtöredékek hello. Amikor felszólító toosign a bejelentkezési hello előfizetés rendszergazdák vagy a tulajdonosok egyikeként. 

        # Sign in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > Ha hello Data Lake Store erőforrás-szolgáltató regisztrálása és a hibaüzenet hasonló túl`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid`, az előfizetés nem feltétlenül szerepel az engedélyezési listán a Data Lake Store. tooenable hello Data Lake Store nyilvános előzetes Azure-előfizetése hello utasításokat követve [hello Azure-portál használatával Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md).
    >

2. Data Lake Store-fiók tartozik egy Azure-erőforráscsoportot. Először hozzon létre egy erőforráscsoportot.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Ez hasonló kimenetnek kell megjelennie:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Data Lake Store-fiók létrehozása. hello fiókhoz megadott név csak kisbetűket és számokat tartalmazhat.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    Hello hasonló kimenetnek kell megjelennie:

        ...
        ProvisioningState           : Succeeded
        State                       : Active
        CreationTime                : 5/5/2017 10:53:56 PM
        EncryptionState             : Enabled
        ...
        LastModifiedTime            : 5/5/2017 10:53:56 PM
        Endpoint                    : hdiadlstore.azuredatalakestore.net
        DefaultGroup                :
        Id                          : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp/providers/Microsoft.DataLakeStore/accounts/hdiadlstore
        Name                        : hdiadlstore
        Type                        : Microsoft.DataLakeStore/accounts
        Location                    : East US 2
        Tags                        : {}

4. A Data Lake Store alapértelmezett tárolási szükség van a toospecify, a legfelső szintű elérési toowhich hello fürtre másol fürt létrehozása során. egy legfelső szintű elérési útját, amely toocreate **/fürtök/hdiadlcluster** hello részlet, használja a következő parancsmagok hello:

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a>A hitelesítés beállítása, szerepköralapú hozzáférési tooData Lake
Minden Azure-előfizetés nem tartozik az Azure AD entitás. Felhasználók és a szolgáltatások, hogy az előfizetés-erőforrások eléréséhez a hello Azure-portálon, vagy hello Azure Resource Manager API először hitelesíteniük kell az Azure ad-val. Hozzáférés tooAzure előfizetések és a szolgáltatások hello megfelelő szerepkört egy Azure-erőforrás hozzárendelésével. Szolgáltatások esetén egy egyszerű hello szolgáltatás azonosítja az Azure AD.

Ez a szakasz bemutatja, hogyan toogrant egy alkalmazás-szolgáltatáshoz, például a HDInsight hozzáférést tooan Azure-erőforrás (hello korábban létrehozott Data Lake Store-fiókot). Ehhez a szolgáltatás egyszerű hello alkalmazáshoz hoz létre és szerepkörök tooit PowerShell.

az Active Directory-hitelesítés az Azure Data Lake tooset a következő két szakasz hello hello feladatok végrehajtására.

### <a name="create-a-self-signed-certificate"></a>Önaláírt tanúsítvány létrehozása
Győződjön meg arról, hogy [Windows SDK](https://dev.windows.com/en-us/downloads) ebben a szakaszban lévő lépéseket hello folytatása előtt. Kell is létrehozott egy könyvtárat, például a *C:\mycertdir*, ahol létre hello tanúsítványt.

1. Hello PowerShell ablakban nyissa meg toohello helyét Windows SDK-t (általában *C:\Program Files (x86) \Windows Kits\10\bin\x86*), és hello [MakeCert] [ makecert] segédprogram toocreate önaláírt tanúsítvány és titkos kulccsal. A következő parancsok hello használata:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Rákérdezéses tooenter hello titkos kulcs jelszava lesz. Miután hello parancs végrehajtása sikeres, megtekintheti az **CertFile.cer** és **mykey.pvk** hello tanúsítvány megadott könyvtárban található.
2. Használjon hello [Pvk2Pfx] [ pvk2pfx] segédprogram tooconvert hello .pvk és .cer-fájlokat a MakeCert létrehozott tooa .pfx fájlt. Futtassa a következő parancs hello:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Amikor a rendszer kéri, adja meg a hello titkos kulcs jelszava korábban megadott. hello számára megadott érték hello **-po** paraméter hello jelszót, amely hello .pfx fájlba van társítva. Miután hello parancs sikeresen befejeződött, emellett meg kell jelennie egy **CertFile.pfx** hello tanúsítvány megadott könyvtárban található.

### <a name="create-an-azure-ad-and-a-service-principal"></a>Hozzon létre egy Azure AD és az egyszerű szolgáltatás
Ebben a szakaszban egy egyszerű szolgáltatást az Azure AD-alkalmazás létrehozása, hozzárendelése egy szerepkörhöz toohello szolgáltatás egyszerű, és hitelesítse magát hello szolgáltatás egyszerű, adja meg a tanúsítványt. Futtassa a következő parancsok hello az Azure AD-alkalmazás toocreate:

1. Illessze be a következő parancsmagok a PowerShell-konzolablakot hello hello. Győződjön meg arról, hogy hello értéket állít be hello **- DisplayName** tulajdonság értéke egyedi. az értékek hello **- kezdőlap** és **- IdentiferUris** helyőrző értékeket, és nem ellenőrzi.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter hello password" # This is hello password you specified for hello .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Hozzon létre egy egyszerű hello azonosítót.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Adja meg a hello szolgáltatás egyszerű hozzáférés toohello Data Lake Store legfelső szintű és a korábban megadott hello gyökérútvonalát hello mappához. A következő parancsmagok hello használata:

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-hello-default-storage"></a>Egy HDInsight Linux-fürt létrehozása a Data Lake Store hello alapértelmezett tároló

Ebben a szakaszban egy HDInsight Hadoop Linux fürt létrehozása a Data Lake Store hello alapértelmezett tárolóként. Ebben a kiadásban hello HDInsight-fürtöt, és a Data Lake Store hello kell lennie ugyanazon a helyen.

1. Hello előfizetés Bérlőazonosító lekérni, és tárolja toouse később.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Hello HDInsight-fürt létrehozása a következő parancsmagok hello használatával:

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.6" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    Hello parancsmag sikeres befejezése után, akkor a fürt részleteinek hello felsoroló kimenetnek kell megjelennie.

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-data-lake-store"></a>Hello HDInsight fürt toouse Data Lake Store a teszt feladatok futtatása
Miután konfigurálta a HDInsight-fürtöt, futtathatja Tesztfeladatok azt, hogy hozzáférhessen Data Lake Store tooensure. toodo Igen, futtassa a minta Hive feladat toocreate egy tábla már elérhető a Data Lake Store: mintaadatok hello használó * <cluster root>/example/data/sample.log*.

Ebben a szakaszban a Secure Shell (SSH) kapcsolat hello létrehozott HDInsight Linux-fürt be, és egy minta Hive-lekérdezést futtassa.

* Ha egy Windows ügyfél toomake hello fürthöz az SSH-kapcsolat használata esetén lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Ha a Linux-ügyfél toomake hello fürthöz az SSH-kapcsolat használata esetén lásd: [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Hello kapcsolat el hello a következő parancs használatával indítsa el az hello Hive parancssori felület (CLI):

        hive
2. Használjon hello CLI tooenter hello utasítások toocreate nevű új tábla a következő **járművekről gyűjtött** hello mintaadatok használatával a Data Lake Store-ban:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    A hello SSH konzolon hello lekérdezés kimenetnek kell megjelennie.

    >[!NOTE]
    >hello elérési toohello minta CREATE TABLE parancs megelőző hello adatai `adl:///example/data/`, ahol `adl:///` hello fürt legfelső szintű. Következő hello fürt legfelső szintű ebben az oktatóanyagban megadott hello példában hello parancs: `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`. Használjon rövidebb alternatív hello, vagy adja meg a hello teljes elérési útja toohello fürt legfelső szintű.
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a>Hozzáférés Data Lake Store HDFS parancs használatával
Miután konfigurálta a hello HDInsight fürt toouse Data Lake Store, Hadoop elosztott fájlrendszerrel (HDFS) rendszerhéj parancsok tooaccess hello tároló is használhatja.

Ebben a szakaszban egy SSH-kapcsolatot hello létrehozott HDInsight Linux-fürt be, és hello HDFS parancs futtatásával.

* Ha egy Windows ügyfél toomake hello fürthöz az SSH-kapcsolat használata esetén lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Ha a Linux-ügyfél toomake hello fürthöz az SSH-kapcsolat használata esetén lásd: [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Hello kapcsolat elkészítése után hello fájlok listázása a Data Lake Store a következő HDFS hello segítségével a rendszer parancs fájl.

    hdfs dfs -ls adl:///

Is használhatja a hello `hdfs dfs -put` tooupload bizonyos fájlok tooData Lake Store parancsot, és ezután `hdfs dfs -ls` tooverify e hello fájlok sikeresen feltöltve.

## <a name="see-also"></a>Lásd még:
* [Azure-portálon: hozzon létre egy HDInsight-fürt toouse Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
