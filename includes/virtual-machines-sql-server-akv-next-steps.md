## <a name="next-steps"></a><span data-ttu-id="2817b-101">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2817b-101">Next steps</span></span>

<span data-ttu-id="2817b-102">Miután engedélyezte az Azure Key Vault-integráció, az SQL virtuális gép az SQL Server titkosítási engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="2817b-102">After enabling Azure Key Vault Integration, you can enable SQL Server encryption on your SQL VM.</span></span> <span data-ttu-id="2817b-103">Először szüksége lesz toocreate aszimmetrikus kulcsok belül a kulcstartót és egy szimmetrikus kulcsot az SQL Serverben a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="2817b-103">First, you will need toocreate an asymmetric key inside your key vault and a symmetric key within SQL Server on your VM.</span></span> <span data-ttu-id="2817b-104">Ezt követően tudja tooexecute T-SQL utasítás tooenable titkosítás az adatbázisok és a biztonsági mentések fogja.</span><span class="sxs-lookup"><span data-stu-id="2817b-104">Then, you will be able tooexecute T-SQL statements tooenable encryption for your databases and backups.</span></span>

<span data-ttu-id="2817b-105">Kihasználhatja a titkosítási több űrlap van:</span><span class="sxs-lookup"><span data-stu-id="2817b-105">There are several forms of encryption you can take advantage of:</span></span>

* [<span data-ttu-id="2817b-106">Az átlátható adattitkosítás (TDE)</span><span class="sxs-lookup"><span data-stu-id="2817b-106">Transparent Data Encryption (TDE)</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="2817b-107">Titkosított biztonsági mentések</span><span class="sxs-lookup"><span data-stu-id="2817b-107">Encrypted backups</span></span>](https://msdn.microsoft.com/library/dn449489.aspx)
* [<span data-ttu-id="2817b-108">Oszlop a blokkszintű titkosítás (törlése)</span><span class="sxs-lookup"><span data-stu-id="2817b-108">Column Level Encryption (CLE)</span></span>](https://msdn.microsoft.com/library/ms173744.aspx)

<span data-ttu-id="2817b-109">hello következő Transact-SQL-parancsfájlok példákat biztosítanak az egyes ezeket a területeket.</span><span class="sxs-lookup"><span data-stu-id="2817b-109">hello following Transact-SQL scripts provide examples for each of these areas.</span></span>

### <a name="prerequisites-for-examples"></a><span data-ttu-id="2817b-110">Példák előfeltételei</span><span class="sxs-lookup"><span data-stu-id="2817b-110">Prerequisites for examples</span></span>

<span data-ttu-id="2817b-111">Minden egyes példa alapján hello két Előfeltételek: a key vaultban aszimmetrikus kulcsok nevű **CONTOSO_KEY** és nevű hello AKV-integráció funkció által létrehozott hitelesítő adatokat **Azure_EKM_TDE_cred**.</span><span class="sxs-lookup"><span data-stu-id="2817b-111">Each example is based on hello two prerequisites: an asymmetric key from your key vault called **CONTOSO_KEY** and a credential created by hello AKV Integration feature called **Azure_EKM_TDE_cred**.</span></span> <span data-ttu-id="2817b-112">hello következő Transact-SQL-parancsokat a telepítő az Előfeltételek hello példák futtatásához.</span><span class="sxs-lookup"><span data-stu-id="2817b-112">hello following Transact-SQL commands setup these prerequisites for running hello examples.</span></span>

``` sql
USE master;
GO

sp_configure [show advanced options], 1;
GO
RECONFIGURE;
GO

-- Enable EKM provider
sp_configure [EKM provider enabled], 1;
GO
RECONFIGURE;

--create provider
CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov
FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';
GO

--create credential
CREATE CREDENTIAL sysadmin_ekm_cred
    WITH IDENTITY = 'keytestvault', --keyvault
    SECRET = '<<SECRET>>'
FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;


--must have sysadmin
ALTER LOGIN [TDE_Login]
ADD CREDENTIAL sysadmin_ekm_cred;


CREATE ASYMMETRIC KEY CONTOSO_KEY
FROM PROVIDER [AzureKeyVault_EKM_Prov]
WITH PROVIDER_KEY_NAME = 'keytestvault',  --key name
CREATION_DISPOSITION = OPEN_EXISTING;
```

### <a name="transparent-data-encryption-tde"></a><span data-ttu-id="2817b-113">Az átlátható adattitkosítás (TDE)</span><span class="sxs-lookup"><span data-stu-id="2817b-113">Transparent Data Encryption (TDE)</span></span>

1. <span data-ttu-id="2817b-114">Hozzon létre egy SQL Server bejelentkezési toobe TDE hello adatbázismotor által használt, és vegye fel a hello credential tooit.</span><span class="sxs-lookup"><span data-stu-id="2817b-114">Create a SQL Server login toobe used by hello Database Engine for TDE, then add hello credential tooit.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with hello asymmetric key
   -- for hello Database engine toouse when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN TDE_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter hello TDE Login tooadd hello credential for use by the
   -- Database Engine tooaccess hello key vault
   ALTER LOGIN TDE_Login
   ADD CREDENTIAL Azure_EKM_TDE_cred;
   GO
   ```

1. <span data-ttu-id="2817b-115">Hello adatbázis-titkosítási kulcs, amely jelzi a TDE létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2817b-115">Create hello database encryption key that will be used for TDE.</span></span>

   ``` sql
   USE ContosoDatabase;
   GO

   CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_128 
   ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter hello database tooenable transparent data encryption.
   ALTER DATABASE ContosoDatabase
   SET ENCRYPTION ON;
   GO
   ```

### <a name="encrypted-backups"></a><span data-ttu-id="2817b-116">Titkosított biztonsági mentések</span><span class="sxs-lookup"><span data-stu-id="2817b-116">Encrypted backups</span></span>

1. <span data-ttu-id="2817b-117">Hozzon létre egy SQL Server bejelentkezési toobe hello adatbázismotor biztonsági mentések titkosításához használja, és adja hozzá a hello credential tooit.</span><span class="sxs-lookup"><span data-stu-id="2817b-117">Create a SQL Server login toobe used by hello Database Engine for encrypting backups, and add hello credential tooit.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with hello asymmetric key
   -- for hello Database engine toouse when it is encrypting hello backup.
   CREATE LOGIN Backup_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter hello Encrypted Backup Login tooadd hello credential for use by
   -- hello Database Engine tooaccess hello key vault
   ALTER LOGIN Backup_Login
   ADD CREDENTIAL Azure_EKM_Backup_cred ;
   GO
   ```

1. <span data-ttu-id="2817b-118">Adja meg a titkosítási hello aszimmetrikus kulccsal hello key vaultban tárolt biztonsági mentési hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2817b-118">Backup hello database specifying encryption with hello asymmetric key stored in hello key vault.</span></span>

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   tooDISK = N'[PATH tooBACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a><span data-ttu-id="2817b-119">Oszlop a blokkszintű titkosítás (törlése)</span><span class="sxs-lookup"><span data-stu-id="2817b-119">Column Level Encryption (CLE)</span></span>

<span data-ttu-id="2817b-120">Ez a parancsfájl létrehoz egy szimmetrikus kulcsot hello aszimmetrikus kulcs hello a key vaultban által védett, és ezután hello szimmetrikus kulcs tooencrypt adatok hello adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="2817b-120">This script creates a symmetric key protected by hello asymmetric key in hello key vault, and then uses hello symmetric key tooencrypt data in hello database.</span></span>

``` sql
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
WITH ALGORITHM=AES_256
ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

DECLARE @DATA VARBINARY(MAX);

--Open hello symmetric key for use in this session
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

--Encrypt syntax
SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data tooencrypt'));

-- Decrypt syntax
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));

--Close hello symmetric key
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;
```

## <a name="additional-resources"></a><span data-ttu-id="2817b-121">További források</span><span class="sxs-lookup"><span data-stu-id="2817b-121">Additional resources</span></span>

<span data-ttu-id="2817b-122">További információ a hogyan toouse, titkosítási szolgáltatásokkal: [EKM használata az SQL Server titkosítási szolgáltatások](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span><span class="sxs-lookup"><span data-stu-id="2817b-122">For more information on how toouse these encryption features, see [Using EKM with SQL Server Encryption Features](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span></span>

<span data-ttu-id="2817b-123">Vegye figyelembe, hogy ez a cikk hello lépések azt feltételezik, hogy már rendelkezik egy Azure virtuális gépen futó SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2817b-123">Note that hello steps in this article assume that you already have SQL Server running on an Azure virtual machine.</span></span> <span data-ttu-id="2817b-124">Ha nem, lásd: [egy SQL Server rendszerű virtuális gép az Azure-ban](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="2817b-124">If not, see [Provision a SQL Server virtual machine in Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="2817b-125">Az Azure virtuális gépeken futó SQL Server más útmutatóért lásd: [SQL Server Azure virtuális gépek – áttekintés](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2817b-125">For other guidance on running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
