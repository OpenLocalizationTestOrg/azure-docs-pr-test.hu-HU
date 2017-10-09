## <a name="next-steps"></a>Következő lépések

Miután engedélyezte az Azure Key Vault-integráció, az SQL virtuális gép az SQL Server titkosítási engedélyezhető. Először szüksége lesz toocreate aszimmetrikus kulcsok belül a kulcstartót és egy szimmetrikus kulcsot az SQL Serverben a virtuális gépen. Ezt követően tudja tooexecute T-SQL utasítás tooenable titkosítás az adatbázisok és a biztonsági mentések fogja.

Kihasználhatja a titkosítási több űrlap van:

* [Az átlátható adattitkosítás (TDE)](https://msdn.microsoft.com/library/bb934049.aspx)
* [Titkosított biztonsági mentések](https://msdn.microsoft.com/library/dn449489.aspx)
* [Oszlop a blokkszintű titkosítás (törlése)](https://msdn.microsoft.com/library/ms173744.aspx)

hello következő Transact-SQL-parancsfájlok példákat biztosítanak az egyes ezeket a területeket.

### <a name="prerequisites-for-examples"></a>Példák előfeltételei

Minden egyes példa alapján hello két Előfeltételek: a key vaultban aszimmetrikus kulcsok nevű **CONTOSO_KEY** és nevű hello AKV-integráció funkció által létrehozott hitelesítő adatokat **Azure_EKM_TDE_cred**. hello következő Transact-SQL-parancsokat a telepítő az Előfeltételek hello példák futtatásához.

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

### <a name="transparent-data-encryption-tde"></a>Az átlátható adattitkosítás (TDE)

1. Hozzon létre egy SQL Server bejelentkezési toobe TDE hello adatbázismotor által használt, és vegye fel a hello credential tooit.

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

1. Hello adatbázis-titkosítási kulcs, amely jelzi a TDE létrehozása.

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

### <a name="encrypted-backups"></a>Titkosított biztonsági mentések

1. Hozzon létre egy SQL Server bejelentkezési toobe hello adatbázismotor biztonsági mentések titkosításához használja, és adja hozzá a hello credential tooit.

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

1. Adja meg a titkosítási hello aszimmetrikus kulccsal hello key vaultban tárolt biztonsági mentési hello adatbázis.

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   tooDISK = N'[PATH tooBACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a>Oszlop a blokkszintű titkosítás (törlése)

Ez a parancsfájl létrehoz egy szimmetrikus kulcsot hello aszimmetrikus kulcs hello a key vaultban által védett, és ezután hello szimmetrikus kulcs tooencrypt adatok hello adatbázisban.

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

## <a name="additional-resources"></a>További források

További információ a hogyan toouse, titkosítási szolgáltatásokkal: [EKM használata az SQL Server titkosítási szolgáltatások](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).

Vegye figyelembe, hogy ez a cikk hello lépések azt feltételezik, hogy már rendelkezik egy Azure virtuális gépen futó SQL Server. Ha nem, lásd: [egy SQL Server rendszerű virtuális gép az Azure-ban](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md). Az Azure virtuális gépeken futó SQL Server más útmutatóért lásd: [SQL Server Azure virtuális gépek – áttekintés](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).
