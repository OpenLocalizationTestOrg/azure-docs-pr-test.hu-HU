---
cím: aaa "PowerShell: Azure HDInsight fürt a Data Lake Store bővítmény tárolóként |} Microsoft Docs"szolgáltatások:-lake-adattár, hdinsight documentationcenter:" Szerző: nitinme manager: jhubbard szerkesztő: cgronlun

MS.AssetId: 164ada5a-222e-4be2-bd32-e51dbe993bc0 ms.service: lake-adattár ms.devlang: na ms.topic: ms.tgt_pltfrm a következő cikket: na ms.workload: big data ms.date: 06/08/2017 ms.author: nitinme

---
# <a name="use-azure-powershell-toocreate-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a>Azure PowerShell toocreate HDInsight-fürtöt használja a Data Lake Store (a további tárhely)
> [!div class="op_single_selector"]
> * [A Portal használata](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell használatával (az alapértelmezett tároló)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [(Tárhely) a PowerShell használatával](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Erőforrás-kezelő használatával](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Ismerje meg, hogyan toouse Azure PowerShell tooconfigure egy HDInsight-fürtöt az Azure Data Lake Store **további tárolóként**. Hogyan toocreate egy HDInsight-fürtöt az Azure Data Lake Store alapértelmezett tárolóként, lásd: [HDInsight-fürtök létrehozása a Data Lake Store alapértelmezett tárolóként](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).

> [!NOTE]
> Ha azt tervezi, hogy toouse Azure Data Lake Store további tárolóként HDInsight-fürthöz, javasoljuk, hogy ehhez hello fürt létrehozásakor, a cikkben leírtak szerint. Azure Data Lake Store további tárhely tooan hozzáadása meglévő HDInsight-fürt egy összetett folyamat és hibalehetőségeket rejt magában tooerrors.
>

Támogatott fürttípusok Data Lake Store egy alapértelmezett tároló vagy a további tárhely fiókként használható. Ha a Data Lake Store további tárterületet, hello alapértelmezett tárfiók hello fürtök továbbra is Azure Storage Blobs (WASB), és hello fürt kapcsolatos fájlokhoz (például naplói, stb.) továbbra is írt toohello alapértelmezett tárolási hello adatainak közben, amikor szeretné, hogy tooprocess tárolható Data Lake Store-fiók. További tárhely fiókként használatával a Data Lake Store nem befolyásolja a teljesítményt és a hello képességét tooread/írás toohello tárolást hello fürtből.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Data Lake Store használata a HDInsight-fürt tárolására

Az alábbiakban a HDInsight a Data Lake Store használatára vonatkozó szempontokat:

* Beállítás toocreate a HDInsight-fürtök hozzáféréssel rendelkező tooData Lake Store további tárolóként verzióihoz áll rendelkezésre HDInsight 3.2-es, 3.4, 3.5-ös és 3.6.

HDInsight konfigurálása a PowerShell használatával a Data Lake Store toowork magában foglalja a hello a következő lépéseket:

* Hozzon létre egy Azure Data Lake Store
* A hitelesítés beállítása, szerepköralapú hozzáférési tooData Lake
* A hitelesítési tooData Lake Store HDInsight-fürt létrehozása
* Egy tesztelési feladat futtatása hello fürtön

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* Az **Azure PowerShell 1.0-s vagy újabb verziója**. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* **Windows SDK**. A későbbiekben telepítheti az [Itt](https://dev.windows.com/en-us/downloads). A toocreate olyan biztonsági tanúsítványt használ.
* **Az Azure Active Directory szolgáltatás egyszerű**. Ez az oktatóanyag lépéseit ad útmutatást toocreate egy egyszerű Azure AD-ben. Azonban az Azure AD rendszergazdai toobe képes toocreate szolgáltatásnevet kell lennie. Ha az Azure AD-rendszergazdaként, hagyja ki ezt az előfeltételt, és hello oktatóanyag folytatásához.

    **Ha nem az Azure AD-rendszergazda**, nem fogja tudni tooperform hello lépéseket szükséges toocreate egy egyszerű szolgáltatást. Ebben az esetben az Azure AD-rendszergazda először létre kell hoznia egy egyszerű szolgáltatást a Data Lake Store egy HDInsight-fürt létrehozása előtt. Emellett hello szolgáltatás egyszerű segítségével kell létrehozni egy tanúsítványt, részben ismertetett módon [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-azure-data-lake-store"></a>Hozzon létre egy Azure Data Lake Store
Hajtsa végre az alábbi lépéseket toocreate egy Data Lake Store.

1. Az asztalon nyisson meg egy új Azure PowerShell-ablakot, és adja meg a következő kódrészletet hello. Amikor felszólító toolog, győződjön meg arról, hogy jelentkezik be a rendelkezésre álló hello előfizetés rendszergazdája vagy tulajdonosa:

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > Ha a hibaüzenet hasonló túl`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid` hello Data Lake Store erőforrás-szolgáltató regisztrálása, esetén lehetséges, hogy az előfizetés nem szerepel az Azure Data Lake Store az engedélyezési listán. Győződjön meg arról, hogy az Azure-előfizetéshez a Data Lake Store nyilvános előzetes verziójához engedélyezze a következő [utasításokat](data-lake-store-get-started-portal.md).
   >
   >
2. Az Azure Data Lake Store-fiókok egy Azure-erőforráscsoporthoz vannak társítva. Először hozzon létre egy Azure-erőforráscsoportot.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Ez hasonló kimenetnek kell megjelennie:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Hozzon létre egy Azure Data Lake Store-fiókot. hello fiókhoz megadott neve csak kisbetűket és számokat tartalmazhat.

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

5. Néhány példa adatok tooAzure Data Lake feltöltése. Ez a cikk tooverify, hogy hello adatok érhető el a HDInsight-fürtök a későbbi fogjuk használni. Néhány példa adatok tooupload keres, ha kaphat a hello **Ambulance Data** hello mappát [Azure Data Lake Git-tárház](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path toodata>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a>A hitelesítés beállítása, szerepköralapú hozzáférési tooData Lake
Egy Azure Active Directory minden Azure-előfizetés tartozik. Felhasználók és a szolgáltatások hello előfizetés hello klasszikus Azure portálon vagy az Azure Resource Manager API-t használó erőforrásokat elérő először hitelesítenie kell magát, hogy Azure Active Directoryban. Hozzáférés tooAzure előfizetések és a szolgáltatások hello megfelelő szerepkört egy Azure-erőforrás hozzárendelésével.  Szolgáltatások esetén egy egyszerű szolgáltatásnév hello Azure Active Directory (AAD) hello szolgáltatást azonosítja. Ez a szakasz bemutatja, hogyan toogrant alkalmazás szolgáltatást, mint például a HDInsight hozzáférést tooan Azure-erőforrás (hello korábban létrehozott Azure Data Lake Store-fiók) hoz létre egy egyszerű hello alkalmazáshoz és szerepkörök toothat Azure keresztül PowerShell.

az Active Directory-hitelesítés az Azure Data Lake tooset, végre kell hajtania a következő feladatok hello.

* Önaláírt tanúsítvány létrehozása
* Létrehoz egy alkalmazást az Azure Active Directory és az egyszerű szolgáltatás

### <a name="create-a-self-signed-certificate"></a>Önaláírt tanúsítvány létrehozása
Győződjön meg arról, hogy [Windows SDK](https://dev.windows.com/en-us/downloads) ebben a szakaszban lévő lépéseket hello folytatása előtt. Kell is létrehozott egy könyvtárat, például a **C:\mycertdir**, ahol hello tanúsítvány jön létre.

1. Hello PowerShell ablakban, keresse meg a toohello helyét Windows SDK-t (általában `C:\Program Files (x86)\Windows Kits\10\bin\x86` és hello [MakeCert] [ makecert] segédprogram toocreate egy önaláírt tanúsítványt és a titkos kulcs. A következő parancsok hello használata.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Rákérdezéses tooenter hello titkos kulcs jelszava lesz. Miután hello parancs sikeres végrehajtása során, megjelenik egy **CertFile.cer** és **mykey.pvk** megadott hello tanúsítvány könyvtárban.
2. Használjon hello [Pvk2Pfx] [ pvk2pfx] segédprogram tooconvert hello .pvk és .cer-fájlokat a MakeCert létrehozott tooa .pfx fájlt. Futtassa a következő parancs hello.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Amikor a rendszer kéri hello titkos kulcs jelszava korábban meghatározott adja meg. hello számára megadott érték hello **-po** paraméter hello jelszavát hello .pfx-fájllal. Ha hello parancs sikeresen befejeződött, emellett meg kell jelennie egy CertFile.pfx megadott hello tanúsítvány könyvtárban.

### <a name="create-an-azure-active-directory-and-a-service-principal"></a>Egy Azure Active Directory és az egyszerű szolgáltatás létrehozása
Ebben a szakaszban az Azure Active Directory-alkalmazás végrehajtja a hello lépéseket toocreate egy egyszerű szolgáltatást, hozzárendelése egy szerepkörhöz toohello szolgáltatás egyszerű, és hitelesítse magát hello szolgáltatás egyszerű, adja meg a tanúsítványt. Futtassa a következő parancsok toocreate hello egy alkalmazást az Azure Active Directoryban.

1. Illessze be a következő parancsmagok a PowerShell-konzolablakot hello hello. Győződjön meg arról, hogy hello tulajdonság hello **- DisplayName** tulajdonság értéke egyedi. Emellett hello értékeinek **- kezdőlap** és **- IdentiferUris** helyőrző értékeket, és nem ellenőrzi.

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
2. Hozzon létre egy egyszerű szolgáltatást hello alkalmazás azonosítójával.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Adja meg a hello szolgáltatás egyszerű hozzáférés toohello Data Lake Store a mappák és hello használni, akkor a hello HDInsight-fürthöz. az alábbi hello részlet hozzáférés toohello gyökere hello Data Lake Store (másolásakor hello mintaadatfájlokat) fiók, és maga hello biztosít.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a>Egy HDInsight Linux-fürt létrehozása a Data Lake Store további tárhely

Ebben a részben azt egy HDInsight Hadoop Linux fürt létrehozása a Data Lake Store további tárolóként. Ebben a kiadásban hello HDInsight-fürtöt és hello Data Lake Store kell, hogy a hello ugyanazon a helyen.

1. Indítsa el beolvasása hello előfizetés bérlő-azonosító. Később szüksége lesz, amely.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. Ebben a kiadásban a Hadoop fürtök a Data Lake Store csak használható további tárterületként hello fürthöz. hello alapértelmezett tárolási is hello az Azure storage blobs (WASB). Igen először létrehozunk hello fiók- és tárolási tárolókban hello fürt szükséges.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. Hello HDInsight-fürtök létrehozása. A következő parancsmagok hello használata.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have hello same name for hello cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Ha hello parancsmag sikeresen befejeződött, akkor listaelem hello fürt részletei kimenetnek kell megjelennie.


## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a>Hello HDInsight fürt toouse hello Data Lake Store a teszt feladatok futtatása
Miután konfigurálta a HDInsight-fürtöt, futtathatja Tesztfeladatok hello fürt tootest adott hello HDInsight fürt Data Lake Store férhetnek hozzá. toodo úgy, hogy fog futni egy minta Hive-feladatot, amely táblát hoz létre, hogy a korábbi tooyour Data Lake Store feltöltött hello mintaadatok használatával.

Ez a szakasz a SSH hello HDInsight Linux cluster során létrehozott és hello minta Hive-lekérdezések futtatása a tartalma.

* Ha egy Windows ügyfél tooSSH hello fürtbe használ, tekintse meg [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Ha a Linux-ügyfél tooSSH hello fürtbe használ, tekintse meg [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

1. A csatlakozás után indítsa el a hello Hive CLI hello a következő parancs segítségével:

        hive
2. Hello CLI, adja meg a következő utasítások toocreate nevű új tábla hello **járművekről gyűjtött** hello mintaadatok használatával a Data Lake Store hello:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Egy kimeneti hasonló toohello következő kell megjelennie:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1

## <a name="access-data-lake-store-using-hdfs-commands"></a>Hozzáférés Data Lake Store HDFS parancs használatával
Miután konfigurálta a hello HDInsight fürt toouse Data Lake Store, hello HDFS rendszerhéj parancsok tooaccess hello tároló is használhatja.

Az itt SSH hello HDInsight Linux cluster során létrehozott és hello HDFS parancs futtatása a lesz.

* Ha egy Windows ügyfél tooSSH hello fürtbe használ, tekintse meg [SSH használata a HDInsight Windows Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Ha a Linux-ügyfél tooSSH hello fürtbe használ, tekintse meg [SSH használata a HDInsight Linux Linux-alapú Hadooppal](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

Miután csatlakozott, használja a következő HDFS hello filesystem parancs toolist fájlok hello Data Lake Store hello.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Hello fájlt, hogy a korábbi toohello Data Lake Store feltöltött megjelenik.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Is használhatja a hello `hdfs dfs -put` tooupload bizonyos fájlok toohello Data Lake Store parancsot, és ezután `hdfs dfs -ls` tooverify e hello fájlok sikeresen feltöltve.

## <a name="see-also"></a>Lásd még:
* [-Portálon: Létrehozása egy HDInsight-fürt toouse Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
