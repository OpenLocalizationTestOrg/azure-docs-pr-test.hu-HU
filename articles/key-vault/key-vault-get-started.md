---
title: "Bevezetés az Azure Key Vault használatába | Microsoft Docs"
description: "Ez az oktatóanyag segítségére lesz az Azure Key Vault szolgáltatás megismerésében, amellyel megerősített tárolókat hozhat létre, valamint kriptográfiai kulcsokat és titkos kódokat tárolhat az Azure-ban."
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/20/2017
ms.author: barclayn
ms.openlocfilehash: 1b70802945b710059e93b54607996ccf74510d1f
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/20/2017
---
# <a name="get-started-with-azure-key-vault"></a>Bevezetés az Azure Key Vault használatába
Ez a cikk útmutatást nyújt az Azure Key Vault PowerShell segítségével történő használatának a megismerésében, és az alábbi tevékenységeken vezeti végig:
- Megerősített tároló létrehozása az Azure-ban.
- A KeyVault használata kriptográfiai kulcsok és titkos kódok kezeléséhez az Azure-ban.
- Az adott kulcs vagy jelszó alkalmazások általi használata.

Az Azure Key Vault a legtöbb régióban elérhető. További információ: [A Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/).

> [!NOTE]
> Ebben a cikkben nem szerepelnek az Azure-alkalmazások írására vonatkozó utasítások. Ezen lépésekhez az [Azure Key Vault-mintaalkalmazást](https://www.microsoft.com/download/details.aspx?id=45343) használhatja.

A platformfüggetlen parancssori felületre vonatkozó utasításokat [ebben az oktatóanyagban](key-vault-manage-with-cli2.md) tekintheti meg.

## <a name="requirements"></a>Követelmények
Mielőtt folytatná a cikket, ellenőrizze, hogy rendelkezik-e az alábbiakkal:

- **Azure-előfizetés**. Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes fiókkal](https://azure.microsoft.com/pricing/free-trial/).
- Az **Azure PowerShell** legalább **1.1.0-s verziója**. Az Azure PowerShell telepítésérről és az Azure-előfizetéssel való társításáról további információt [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása) című cikkben találhat. Ha már telepítette az Azure PowerShellt, de nem tudja, melyik verziót, írja be az Azure PowerShell-konzolon az alábbi parancsot: `(Get-Module azure -ListAvailable).Version`. Ha az Azure PowerShell 0.9.1-től 0.9.8-ig terjedő verziói közül rendelkezik valamelyikkel, néhány apró eltéréstől függetlenül Önre is vonatkozik az útmutató. Például a `Switch-AzureMode AzureResourceManager` parancsot kell használnia, valamint bizonyos Azure Key Vault parancsok módosultak. A 0.9.1 és 0.9.8 közötti verziók Key Vault parancsmagjainak listája az alábbi témakörben található: [Azure Key Vault Cmdlets](/powershell/module/azurerm.keyvault/#key_vault) (Az Azure Key Vault parancsmagjai).
- **A Key Vault használatára konfigurálható alkalmazás**. Egy mintaalkalmazás elérhető a [Microsoft letöltőközpontból](http://www.microsoft.com/download/details.aspx?id=45343). Útmutatásért tekintse meg a kísérő **információs fájlt**.

>[!NOTE]
A cikk feltételezi, hogy rendelkezik a PowerShell és az Azure használatára vonatkozó alapvető ismeretekkel. További információk a PowerShell-ről: [Ismerkedés a Windows PowerShell-lel](https://technet.microsoft.com/library/hh857337.aspx).

Az útmutatóban található parancsmagokhoz részletes segítséget kérhet a **Get-Help** parancsmaggal.

```powershell-interactive
Get-Help <cmdlet-name> -Detailed
```
    
Például ha a **Login-AzureRmAccount** parancsmaghoz szeretne segítséget kérni, írja be a következőt:

```PowerShell
Get-Help Login-AzureRmAccount -Detailed
```

Az alábbi cikkekben megismerkedhet az Azure Resource Manager-alapú üzemi modellnek az Azure PowerShell-lel való használatával:

* [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview)
* [Using Azure PowerShell with Resource Manager (Az Azure PowerShell és a Resource Manager együttes használata)](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Csatlakozás az előfizetésekhez
Indítson el egy Azure PowerShell-munkamenetet, és jelentkezzen be az Azure-fiókjába az alábbi paranccsal:  

```PowerShell
Login-AzureRmAccount
```

>[!NOTE]
 Az Azure bizonyos példányainak használatakor az -Environment paramétert kell használni. Példa: 
 ```powershell
 Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
 ```

Az előugró böngészőablakban adja meg az Azure-fiókja felhasználónevét és jelszavát. Az Azure PowerShell megkeresi az összes olyan előfizetést, amely ehhez a fiókhoz van rendelve, és alapértelmezés szerint kiválasztja az elsőt.

Ha több előfizetése van, és meg szeretné szabni, hogy melyiket használja az Azure Key Vaulthoz, írja be az alábbi parancsot a fiókhoz tartozó előfizetések megtekintéséhez:

```powershell
Get-AzureRmSubscription
```

Ezt követően írja be az alábbi parancsot a kívánt előfizetés kiválasztásához:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

További információ az Azure PowerShell konfigurálásáról: [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása).

## <a id="resource"></a>Új erőforráscsoport létrehozása
Az Azure Resource Manager használatakor minden kapcsolódó erőforrás egy erőforráscsoportban jön létre. Ehhez az útmutatóhoz hozzon létre egy új erőforráscsoportot **ContosoResourceGroup** névvel:

```powershell
New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East US'
```

## <a id="vault"></a>Kulcstartó létrehozása
A[New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) parancsmag segítségével hozzon létre egy kulcstartót. Ez a parancsmag három kötelező paraméterrel rendelkezik: egy **erőforráscsoport-név**, egy **kulcstartónév**, és a **földrajzi hely**.

Ha például az alábbiakat használja:
- Tároló neve: **ContosoKeyVault**.
- Erőforráscsoport neve: **ContosoResourceGroup**.
- Hely: **USA keleti régiója**.

Az alábbiakat kell beírnia:

```powershell
New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```
![Kimenet a Key Vault létrehozási parancsának a befejeződése után](./media/key-vault-get-started/output-after-creating-keyvault.png)

Ezzel a parancsmaggal megjelenítheti az újonnan létrehozott kulcstartó tulajdonságait. A két legfontosabb tulajdonság:

* **A tároló neve**: A példában ez a **ContosoKeyVault**. Ezt a nevet fogja majd más Key Vault parancsmagokban is megadni.
* **A tároló URI-ja**: A példában ez a https://contosokeyvault.vault.azure.net/. A tárolót a REST API-ján keresztül használó alkalmazásoknak ezt az URI-t kell használniuk.

Azure-fiókja most már engedéllyel rendelkezik arra, hogy bármilyen műveletet végezzen ezen a kulcstartón. Egyelőre senki másnak nincs erre engedélye.

> [!NOTE]
> Az új kulcstartó létrehozásakor a következő hibaüzenettel találkozhat: **Az előfizetés nincs regisztrálva a(z) Microsoft.KeyVault névtér használatára**. Ha ez az üzenet jelenik meg, futtassa a `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` parancsot. Ha a regisztráció sikeresen befejeződött, újra futtathatja a New-AzureRmKeyVault parancsot. További információ: [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Kulcs vagy titkos kód hozzáadása a kulcstartóhoz
A Key Vaulttal, illetve a kulcsokkal és titkos kódokkal számos különféle műveletet végezhet.

### <a name="azure-key-vault-generates-a-software-protected-key"></a>Az Azure Key Vault szoftveresen védett kulcsot hoz létre

Ha azt szeretné, hogy az Azure Key Vault szoftveresen védett kulcsot hozzon létre, használja az [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) parancsmagot az alábbi módon:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'
```
a kulcs URI-jának megtekintéséhez írja be az alábbiakat:
```powershell
$key.id
```

A létrehozott vagy az Azure Key Vaultba feltöltött kulcsra ez után az URI használatával hivatkozhat. A **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** oldalról mindig beszerezheti a legfrissebb verziót, a **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** oldalról pedig ezt a verziót töltheti le.  

### <a name="importing-an-existing-pfx-file-into-azure-key-vault"></a>Meglévő PFX-fájl importálása az Azure Key Vaultba

Az Azure Key Vaultba feltölteni kívánt PFX-fájlban tárolt meglévő kulcsok esetén más lépéseket kell végrehajtania. Példa:
- Ha PFX-fájlban tárolt meglévő, szoftveresen védett kulccsal rendelkezik
- A PFX-fájl neve softkey.pfx 
- A fájlt a C: meghajtón tárolja a rendszer.

Az alábbiakat írhatja be:

```powershell
$securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force  // This stores the password 123 in the variable $securepfxpwd
```

Ezután az alábbi paranccsal importálja a kulcsot a .PFX-fájlból, így szoftveres védelmet biztosíthat neki a Key Vault szolgáltatásban:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoImportedPFX' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd
```

A kulcs URI-jának megjelenítéséhez írja be az alábbi parancsot:

```powershell
$Key.id
```
A kulcs megtekintéséhez írja be az alábbi parancsot: 

```powershell
Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'
```
Ha a PFX-fájl tulajdonságait szeretné megtekinteni a portálon, akkor az alábbi képen láthatóhoz hasonló adatok jelennek meg.

![A tanúsítványok megjelenése a portálon](./media/key-vault-get-started/imported-pfx.png)
### <a name="to-add-a-secret-to-azure-key-vault"></a>Titkos kód hozzáadása az Azure Key Vaulthoz

A titkos kód – amely egy SQLPassword nevű, Pa$$w0rd értékű jelszó – hozzáadásához az Azure Key Vault szolgáltatásbeli tárolóhoz először konvertálja a Pa$$w0rd értéket egy biztonságos karakterláncra az alábbi paranccsal:

```powershell    
$secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force
```

Majd írja be az alábbiakat:

```powershell
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue
```


Az Azure Key Vaulthoz hozzáadott jelszóra ez után az URI használatával hivatkozhat. A **https://ContosoVault.vault.azure.net/secrets/SQLPassword** oldalról mindig letöltheti a legfrissebb verziót, a **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** oldalról pedig ezt a verziót töltheti le.

A titkos kódok URI-jának megjelenítéséhez írja be az alábbi parancsot:

```powershell
$secret.Id
```
A titkos kód megtekintéséhez írja be a következő parancsot: `Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'` A titkos kódot a portálon is megtekintheti.

![titkos kód](./media/key-vault-get-started/secret-value.png)

A titkos kódban tárolt érték megtekintése egyszerű szövegként:
```powershell
(get-azurekeyvaultsecret -vaultName "Contosokeyvault" -name "SQLPassword").SecretValueText
```
A kulcstartó és a kulcs vagy a titkos kód mostantól készen áll az alkalmazásokkal való használatra. Használatukat engedélyeznie kell az alkalmazások számára.  

## <a id="register"></a>Alkalmazás regisztrálása az Azure Active Directory szolgáltatásban
Ezt a lépést általában egy fejlesztő végzi egy másik számítógépről. Nem kifejezetten az Azure Key Vaulthoz kapcsolódik. Az alkalmazások Azure Active Directoryban való regisztrálásának részletes lépéseit az [Alkalmazások integrálása az Azure Active Directoryval](../active-directory/develop/active-directory-integrating-applications.md) vagy az [Azure Active Directory-alkalmazás és -szolgáltatásnév létrehozása a portálon erőforrások eléréséhez](../azure-resource-manager/resource-group-create-service-principal-portal.md) című cikkben tekintheti meg.

> [!IMPORTANT]
> Az oktatóprogram sikeres befejezéséhez a fiókjának, a tárolónak és az ebben a lépésben regisztrálandó alkalmazásnak ugyanabban az Azure-címtárban kell lenniük.


A kulcstartót használó alkalmazásoknak az Azure Active Directoryból származó jogkivonat használatával kell hitelesítést végezniük. Ehhez az alkalmazás tulajdonosának először regisztrálnia kell az alkalmazást az Azure Active Directory szolgáltatásban. A regisztrációt követően az alkalmazás tulajdonosa az alábbi értékeket kapja:

- egy **alkalmazásazonosítót**, 
- egy **hitelesítési kulcsot** (más néven közös titkos kódot). 

Az alkalmazásnak mindkét értékkel rendelkeznie kell ahhoz, hogy Azure Active Directory-jogkivonatot kapjon. Az értékek megadásának módja az adott alkalmazástól függ. A [Key Vault-mintaalkalmazás](https://www.microsoft.com/download/details.aspx?id=45343) esetében az alkalmazás tulajdonosa adja meg ezeket az értékeket az app.config fájlban.


Az alkalmazás regisztrálása az Azure Active Directory szolgáltatásban:

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. A bal oldalon kattintson az **Alkalmazásregisztrációk** elemre. Ha nem lát alkalmazásregisztrációkat, kattintson a **további szolgáltatások** elemre.  
>[!NOTE]
Azt a címtárat kell kiválasztania, amely a kulcstartót létrehozó Azure-előfizetést tartalmazza. 
3. Kattintson az **Új alkalmazásregisztráció** elemre.
4. A **Létrehozás** panelen adjon meg egy nevet az alkalmazásnak, majd válassza a **WEB APPLICATION AND/OR WEB API** (WEBALKALMAZÁS ÉS/VAGY WEBES API) (az alapértelmezett beállítás) lehetőséget, és adja meg a webalkalmazás **SIGN-ON URL** (Bejelentkezési URL) címét. Ha jelenleg nem rendelkezik ezekkel az információkkal, ehhez a lépéshez nem létező értékeket is megadhat (például megadhatja a http://test1.contoso.com címet). Nem számít, hogy ezek a webhelyek léteznek-e. 

    ![Új alkalmazásregisztráció](./media/key-vault-get-started/new-application-registration.png)
    >[!WARNING]
    Ügyeljen arra, hogy a **WEB APPLICATION AND/OR WEB API** (WEBALKALMAZÁS ÉS/VAGY WEBES API) lehetőség legyen kiválasztva, különben a **kulcsok** lehetőség nem jelenik meg a beállítások között.

5. Kattintson a **Létrehozás** gombra.
6. Az alkalmazásregisztráció befejezését követően megjelenik a regisztrált alkalmazások listája. Keresse meg az imént regisztrált alkalmazást, és kattintson rá.
7. Kattintson a **Regisztrált alkalmazás** panelre, és másolja az **alkalmazásazonosítót**.
8. Kattintson az **Összes beállítás** elemre.
9. A **Beállítások** panelen kattintson az **kulcsok** elemre.
9. A **Kulcs leírása** mezőben írja be a leírást, válassza ki az időtartamot, majd kattintson **MENTÉS** gombra. A lap ekkor frissül, és kijelzi a kulcs értékét. 
10. A következő lépésben az **alkalmazásazonosító** és a **kulcs** adataival fogja beállítani a tároló engedélyeit.

## <a id="authorize"></a>A kulcs vagy titkos kód használatának engedélyezése az alkalmazás számára
Az alkalmazás a tároló kulcsához vagy titkos kódjához való hozzáférésének engedélyezéséhez használja a [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) parancsmagot.

Ha például a tároló neve **ContosoKeyVault**, az engedélyezni kívánt alkalmazás ügyfélazonosítója 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, és engedélyezni szeretné az alkalmazás számára, hogy a tároló kulcsait visszafejtse és használja, futtassa az alábbi parancsot:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Ha engedélyezni szeretné, hogy az alkalmazás megnyithassa a tárolóban lévő titkos kódokat, futtassa az alábbi parancsot:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get
```

## <a id="HSM"></a>Hardveres biztonsági modul (HSM) használata
A nagyobb biztonság érdekében hardveres biztonsági modulokkal importálhat vagy hozhat létre a HSM határait mindig betartó kulcsokat. A hardveres biztonsági modulok a 2. szintű FIPS 140-2 szerint vannak érvényesítve. Ha ez a követelmény nem vonatkozik Önre, ugorja át ezt a szakaszt, és folytassa a [Kulcsartó és a hozzá tartozó kulcsok és titkos kódok törlése](#delete) szakasszal.

Ezeknek a HSM-védelemmel ellátott kulcsoknak a létrehozásához az [Azure Key Vault Premium szolgáltatási réteget kell használni a HSM-védelemmel ellátott kulcsok támogatására](https://azure.microsoft.com/pricing/free-trial/). Emellett felhívjuk figyelmét, hogy ezt a funkciót az Azure China nem támogatja.

A kulcstároló létrehozásakor adja hozzá az alábbi **-SKU** paramétert:

```powershell
New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US' -SKU 'Premium'
```


Ehhez a kulcstárolóhoz a korábbiakban bemutatott szoftveresen védett és HSM által védett kulcsokat is hozzáadhat. HSM által védett kulcs létrehozásához állítsa a **-Destination** paramétert „HSM”-re:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'
```

Az alábbi paranccsal egy .PFX-fájlban lévő kulcsot importálhat a számítógépéről. Ez a parancs a hardveres Key Vault szolgáltatás biztonsági moduljaiba importálja a kulcsot:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'
```

A következő parancs egy „saját kulcs használata” (BYOK-) csomagot importál. Ezzel a forgatókönyvvel a helyi HSM-ben hozhatja létre a kulcsot, majd helyezheti át a Key Vault szolgáltatás HSM-jeibe anélkül, hogy a kulcs elhagyná a HSM határait:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'
```

A BYOK-csomag létrehozásával kapcsolatos részletesebb útmutatásért tekintse meg a [How to generate and transfer HSM-protected keys for Azure Key Vault](key-vault-hsm-protected-keys.md) (HSM által védett kulcsok létrehozása és áthelyezése az Azure Key Vaultban) című témakört.

## <a id="delete"></a>Kulcstartó és a hozzá tartozó kulcsok és titkos kódok törlése
Ha már nincs szüksége a kulcstartóra és az ahhoz tartozó kulcsra vagy titkos kódra, eltávolíthatja a kulcstartót a [Remove-AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault) parancsmaggal:

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'
```

Lehetősége van a teljes Azure-erőforráscsoport törlésére is, amely magában foglalja a kulcstartót és a csoport összes erőforrását:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'
```

## <a id="other"></a>Egyéb Azure PowerShell-parancsmagok
Egyéb parancsok, amelyek hasznosak lehetnek az Azure Key Vault kezeléséhez:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Ez a parancs táblázatos formában jeleníti meg az összes kulcsot és a megadott tulajdonságokat.
- `$Keys[0]`: Ez a parancs a megadott kulcs tulajdonságainak teljes listáját jeleníti meg
- `Get-AzureKeyVaultSecret`: Ez a parancs táblázatos formában jeleníti meg az összes titkos kód nevét és a megadott tulajdonságokat.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Példa egy adott kulcs eltávolítására.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Példa egy adott titkos kód eltávolítására.

## <a name="next-steps"></a>Következő lépések

- Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az Azure Key Vault?) című cikkben talál.
- A kulcstartó használatának módjairól az [Azure Key Vault Logging](key-vault-logging.md) (Az Azure Key Vault naplózása) című témakörben olvashat.
- Az Azure Key Vault webalkalmazásban való használatáról a [Use Azure Key Vault from a Web Application](key-vault-use-from-web-application.md) (Az Azure Key Vault webalkalmazással való használata) című témakörben találhat további útmutatást.
- Programozási hivatkozások: [Az Azure Key Vault fejlesztői útmutatója](key-vault-developers-guide.md).
- A legújabb Azure Key Vaultra vonatkozó Azure PowerShell-parancsmagok listáját az [Azure Key Vault Cmdlets](/powershell/module/azurerm.keyvault/#key_vault) (Az Azure Key Vault parancsmagjai) című témakörben találja.