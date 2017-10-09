## <a name="prepare-for-akv-integration"></a>Készítse elő a AKV-integráció
toouse Azure Key Vault-integráció tooconfigure az SQL Server virtuális gép nincsenek több Előfeltételek: 

1. [Azure Powershell telepítése](#install-azure-powershell)
2. [Hozzon létre egy Azure Active Directory](#create-an-azure-active-directory)
3. [Hozzon létre egy kulcstartót](#create-a-key-vault)

hello következő szakaszok ismertetik ezeket Előfeltételek és a szükséges toocollect futtatása toolater hello PowerShell-parancsmagok hello információkat.

### <a name="install-azure-powershell"></a>Az Azure PowerShell telepítése
Győződjön meg arról, hogy a telepített legújabb Azure PowerShell SDK hello. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).

### <a name="create-an-azure-active-directory"></a>Hozzon létre egy Azure Active Directory
Először toohave egy [Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) az előfizetésben. Között számos előnyt így toogrant engedély tooyour kulcstárolójának bizonyos felhasználók és az alkalmazások.

Ezután regisztrálni egy alkalmazás AAD-ben. Ekkor kap egy egyszerű hozzáférés tooyour kulcstároló, amely a virtuális Gépet kell rendelkező fiók. Hello Azure Key Vault a cikkben található lépéseket hello [alkalmazás regisztrálása az Azure Active Directory](../articles/key-vault/key-vault-get-started.md#register) szakasz, vagy látható hello a képernyőképek hello lépéseket **identitás hello alkalmazás beszerzése a szakasz** a [ebben a blogbejegyzésben](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Az alábbi lépések elvégzése előtt ne feledje, hogy a következő információkat, amelyek később szükség van az SQL virtuális gép az Azure Key Vault-integráció engedélyezése esetén a regisztráció során toocollect hello.

* Hello alkalmazás hozzáadása után található hello **ügyfél-azonosító** a hello **KONFIGURÁLÁSA** fülre.   ![Az Azure Active Directory ügyfél-azonosító](./media/virtual-machines-sql-server-akv-prepare/aad-client-id.png)
  
    hello ügyfél-azonosító hozzá van rendelve a későbbi toohello **$spName** hello PowerShell parancsfájl tooenable Azure Key Vault-integráció (egyszerű szolgáltatásnév) paramétere. 
* Is során ezeket a lépéseket a kulcs létrehozásakor másolja a kulcs titkos kulcsát hello hello a következő képernyőfelvételen látható módon. A kulcs titkos kódok hozzá van rendelve a későbbi toohello **$spSecret** hello PowerShell parancsfájl paramétere (egyszerű titok).  
    ![Az Azure Active Directory titkos kulcs](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)
* Engedélyeznie kell az új ügyfél azonosító toohave hello hozzáférési engedélyek a következő: **titkosítása**, **visszafejtéséhez**, **wrapKey**, **unwrapKey**, **bejelentkezési**, és **ellenőrizze**. Ez a lépés hello [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625.aspx) parancsmag. További információ: [engedélyezés hello alkalmazás toouse hello kulcsok vagy titkos kulcsok](../articles/key-vault/key-vault-get-started.md#authorize).

### <a name="create-a-key-vault"></a>Kulcstartó létrehozása
Rendelés toouse Azure Key Vault toostore hello kulcsok szüksége lesz a titkosításhoz a virtuális Gépet meg kell hozzáférés tooa kulcstároló. Ha már nem beállítása a kulcstároló, hozzon létre egyet hello hello utasításait követve [Ismerkedés az Azure Key Vault](../articles/key-vault/key-vault-get-started.md) témakör. A lépések végrehajtása előtt vegye figyelembe, hogy néhány adat toocollect van szüksége, amely a beállítása során később van szükség, ha engedélyezi az Azure Key Vault-integráció az SQL virtuális gép a.

Meg toohello hozzon létre egy kulcstartót lépés Megjegyzés hello visszaadott **vaultUri** tulajdonságot, amelynek hello kulcstároló URL-cím. Hello példában, hogy az alábbi lépésben megadott hello kulcstároló neve ContosoKeyVault, ezért a hello kulcstároló URL-cím https://contosokeyvault.vault.azure.net/ lenne.

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

hello kulcstároló URL-cím hozzá van rendelve a későbbi toohello **$akvURL** hello PowerShell parancsfájl tooenable Azure Key Vault-integráció paramétere.

