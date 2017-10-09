---
title: "az Azure Key Vault használatába aaaGet |} Microsoft Docs"
description: "Használja az oktatóanyag toohelp kap használatába az Azure Key Vault toocreate megerősített tárolókat toostore, az Azure-ban, és kezelheti a titkosítási kulcsok és titkos az Azure-ban."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 865853b778dec5fca5c7db0d060627554c0a9cb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-key-vault"></a>Bevezetés az Azure Key Vault használatába
Az Azure Key Vault a legtöbb régióban elérhető. További információkért lásd: hello [Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Bevezetés
Használja az oktatóanyag toohelp kap használatába az Azure Key Vault toocreate megerősített tárolókat (kulcstartókat) toostore, az Azure-ban, és kezelheti a titkosítási kulcsok és titkos az Azure-ban. Azt végigvezeti Azure PowerShell toocreate használatával hello folyamat egy kulcsot vagy jelszót, majd használható az Azure-alkalmazások tartalmazó. Ezután bemutatja, hogyan használhatják az adott kulcsot vagy jelszót az alkalmazásai.

**Becsült idő toocomplete:** 20 perc

> [!NOTE]
> Ez az oktatóanyag nem tartalmazza a hogyan toowrite hello Azure-alkalmazást, amely tartalmazza a hello lépések egyike, nevezetesen hogyan tooauthorize egy alkalmazás toouse kulcsok vagy titkos kulcs hello tároló számára.
>
> Az oktatóanyag az Azure PowerShellt használja. A platformfüggetlen parancssori felületre vonatkozó utasításokat [ebben az oktatóanyagban](key-vault-manage-with-cli2.md) tekintheti meg.
>
>

Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az Azure Key Vault?) című cikkben talál.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:

* Egy előfizetés tooMicrosoft Azure. Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes fiókkal](https://azure.microsoft.com/pricing/free-trial/).
* Az Azure PowerShell legalább **1.1.0-ás verziójára**. Azure PowerShell tooinstall és rendelje hozzá azt az Azure-előfizetéssel, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). Ha már telepítette az Azure PowerShell, és nem tudja hello verzió hello Azure PowerShell-konzolon, írja be a `(Get-Module azure -ListAvailable).Version`. Ha az Azure PowerShell 0.9.1-től 0.9.8-ig terjedő verziói közül rendelkezik valamelyikkel, néhány apró eltéréstől függetlenül Önre is vonatkozik az útmutató. Például az hello segítségével kell `Switch-AzureMode AzureResourceManager` parancsot, és néhány hello Azure Key Vault parancsok módosultak. Hello 0.9.1 és 0.9.8 verziók Key Vault parancsmagjainak listája: [Azure Key Vault parancsmagjainak](/powershell/module/azurerm.keyvault/#key_vault).
* Egy alkalmazás, amely konfigurált toouse hello kulcsot vagy jelszót, amely ebben az oktatóanyagban létrehozhat lesz. A mintaalkalmazás érhető el a hello [Microsoft Download Center](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Útmutatásért lásd: hello kísérő információs fájlt.

Ez az oktatóanyag az Azure PowerShell kezdők készült, de azt feltételezi, hogy tudomásul veszi hello alapszintű fogalmakkal, mint a modulok, a parancsmagok és a munkamenetek. További információ: [Getting started with Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx) (Ismerkedés a Windows PowerShellel).

tooget részletes súgó a jelen oktatóanyag esetében használja hello látni parancsmagokhoz **Get-Help** parancsmag.

    Get-Help <cmdlet-name> -Detailed

Hello például tooget súgóját **Login-AzureRmAccount** parancsmag, típus:

    Get-Help Login-AzureRmAccount -Detailed

A következő oktatóanyagok tooget ismeri az Azure Resource Manager az Azure PowerShell hello is olvasható:

* [Hogyan tooinstall Azure PowerShell és konfigurálása](/powershell/azure/overview)
* [Using Azure PowerShell with Resource Manager (Az Azure PowerShell és a Resource Manager együttes használata)](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Csatlakozás tooyour előfizetések
Indítson el egy Azure PowerShell-munkamenetet, és jelentkezzen be Azure-fiók tooyour hello a következő parancsot:  

    Login-AzureRmAccount

Vegye figyelembe, hogy egy adott példányához Azure, például az Azure Governmentnek használatakor használjon hello - Environment paramétert ezzel a paranccsal. Például:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Hello előugró böngészőablakban adja meg a Azure-fiók felhasználói nevét és jelszavát. Az Azure PowerShell lekérdezi hello előfizetéseket, amelyek társítva ezzel a fiókkal, és alapértelmezés szerint, használja az elsőt hello.

Ha több előfizetéssel rendelkezik, és szeretné, hogy egy adott egy toouse toospecify az Azure Key Vaulthoz, írja be a fiókhoz toosee hello előfizetések a következő hello:

    Get-AzureRmSubscription

Ezt követően toospecify hello előfizetés toouse, típus:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Azure PowerShell konfigurálásával kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a id="resource"></a>Új erőforráscsoport létrehozása
Az Azure Resource Manager használatakor minden kapcsolódó erőforrás egy erőforráscsoportban jön létre. Ehhez az útmutatóhoz hozzon létre egy új erőforráscsoportot **ContosoResourceGroup** névvel:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Kulcstartó létrehozása
Használjon hello [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) parancsmag toocreate kulcstároló. Ez a parancsmag három kötelező paraméterrel rendelkezik: egy **erőforráscsoport-név**, egy **kulcstároló neve**, és hello **földrajzi hely**.

Például, ha a tároló neve hello használata **ContosoKeyVault**, hello erőforráscsoport neve **ContosoResourceGroup**, és hello helyének **Kelet-Ázsia**, típus:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Ez a parancsmag kimenete hello az újonnan létrehozott kulcstartó hello tulajdonságok láthatók. hello két legfontosabb tulajdonságai a következők:

* **Tároló neve**: hello a példában ez az **ContosoKeyVault**. Ezt a nevet fogja majd más Key Vault parancsmagokban is megadni.
* **Tároló URI-ja**: hello a példában ez a https://contosokeyvault.vault.azure.net/. A tárolót a REST API-ján keresztül használó alkalmazásoknak ezt az URI-t kell használniuk.

Azure-fiókja most engedélyezett tooperform bármilyen műveletet ezt a kulcsot tároló van. Egyelőre senki másnak nincs erre engedélye.

> [!NOTE]
> Ha hello hibát látja **hello előfizetés nincs regisztrált toouse névtér "Microsoft.KeyVault"** meg toocreate futtassa az új kulcstartó `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` és futtassa újból a New-AzureRmKeyVault parancsot. További információ: [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Kulcs vagy titkos toohello kulcstároló hozzáadása
Ha meg szeretné az Azure Key Vault toocreate szoftveres védelemmel ellátott kulcs, használja a hello [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) parancsmagot, és írja be a következő hello:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Azonban ha meglévő szoftveres védelemmel ellátott kulcs rendelkezik egy. PFX-fájl mentése tooyour C:\ meghajtóra, amelyet az tooupload tooAzure Key Vault, tooset hello változó a következő típus hello softkey.pfx nevű fájlt **securepfxpwd** változójának **123** hello számára. PFX-fájlt:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Írja be a hello tooimport hello kulcsot következő hello. PFX-fájl védetté hello kulcs szoftveres hello Key Vault szolgáltatás:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Ez a kulcs létrehozása vagy tooAzure Key Vaultba feltöltött az URI használatával hivatkozhat. Használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways hello aktuális verzióra, és használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget ezt a verziót.  

toodisplay hello URI ezt a kulcsot, típus:

    $Key.key.kid

tooadd egy titkos toohello tárolóban, amely egy SQLPassword nevű jelszót, és Pa$ $w0rd tooAzure Key Vault hello értékkel rendelkezik, először konvertálja Pa$ $w0rd tooa biztonságos karakterlánc hello értéke hello következő:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Ezután írja be a következő hello:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Ezt a jelszót, hogy tooAzure Key Vault hozzáadta az URI használatával hivatkozhat. Használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways hello aktuális verzióra, és használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget ezt a verziót.

toodisplay hello URI ezt a titkot, típus:

    $secret.Id

Tekintse meg hello kulcsok vagy titkos, újonnan létrehozott:

* tooview a, írja be:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
* tooview a titkos, típus:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Most a kulcstartó és a kulcsok vagy titkos kulcsok készen áll a alkalmazások toouse. Alkalmazások toouse engedélyeznie kell azokat.  

## <a id="register"></a>Alkalmazás regisztrálása az Azure Active Directory szolgáltatásban
Ezt a lépést általában egy fejlesztő végzi egy másik számítógépről. Nem adott tooAzure Key Vault, de a teljesség.

> [!IMPORTANT]
> toocomplete hello oktatóanyag, a fiók, hello tárolóban, és ebben a lépésben regisztrálandó hello alkalmazás kell lenniük hello Azure könyvtárába.
>
>

A kulcstartót használó alkalmazásoknak az Azure Active Directoryból származó jogkivonat használatával kell hitelesítést végezniük. toodo a, hello hello alkalmazás tulajdonosának először regisztrálnia kell a hello alkalmazás az Azure Active Directoryban. A regisztrációs hello végén hello alkalmazás tulajdonosa lekérdezi a következő értékek hello:

* Egy **Alkalmazásazonosító** (más néven Ügyfélazonosítót) és **hitelesítési kulcs** (más néven hello közös titkos kulcs). hello alkalmazás e mindkét ezen értékek tooAzure Active Directory tooget jogkivonatot. Ez függ hello alkalmazás toodo hogyan hello alkalmazás van konfigurálva. A Key Vault mintaalkalmazás hello hello alkalmazás tulajdonosa adja meg ezeket az értékeket hello app.config fájlban.

tooregister hello alkalmazás Azure Active Directoryban:

1. Jelentkezzen be toohello a klasszikus Azure portálon.
2. Hello bal oldalon kattintson **Active Directory**, majd válassza ki az alkalmazást regisztrálni hello directory. <br> <br> **Megjegyzés:** ki kell választania a hello ugyanabban a könyvtárban, amely tartalmazza a kulcstartót létrehozó Azure-előfizetés hello. Ha nem tudja, melyik címtárban ez, kattintson a **beállítások**, a kulcstartót létrehozó hello előfizetés azonosítása és hello utolsó oszlopban megjelenített megjegyzés hello hello könyvtár nevét.
3. Kattintson az **APPLICATIONS** (ALKALMAZÁSOK) elemre. Nincs alkalmazás tooyour directory lettek hozzáadva, ha ezen a lapon látható csak hello **hozzáadhat egy alkalmazást** hivatkozásra. Hello hivatkozásra, vagy másik lehetőségként kattinthat, és **hozzáadása** hello parancssávon.
4. A hello **alkalmazás hozzáadása** hello varázsló **miről szeretne toodo?** lapján kattintson **a szerveztem által fejlesztett alkalmazás hozzáadása**.
5. A hello **adja meg azt az alkalmazás** lapon adja meg az alkalmazás nevét, és válassza ki **WEB APPLICATION AND/OR WEB API** (hello alapértelmezett). Kattintson a hello **következő** ikonra.
6. A hello **alkalmazás tulajdonságainak** adja meg azokat a hello **SIGN-ON URL** és **APP ID URI** a webalkalmazás. Ha az alkalmazás nem rendelkezik ezekkel az értékekkel, ehhez a lépéshez nem létező értékeket is megadhat (például http://test1.contoso.com mindkét mezőhöz). Nem számít, hogy ezek a webhelyek léteznek-e. A fontos, hogy hello app ID URI mindegyik alkalmazáshoz különböző alkalmazás esetében minden egyes a címtárban. hello directory használja a karakterlánc tooidentify az alkalmazást.
7. Kattintson a hello **Complete** ikon toosave hello varázslóban a módosítások.
8. A hello **gyors üzembe helyezés** kattintson **KONFIGURÁLÁSA**.
9. Görgessen toohello **kulcsok** szakasz, válassza ki a hello időtartama, majd kattintson a **mentése**. hello oldal frissül, és most kijelzi a kulcs értékét. Be kell állítania az alkalmazás a kulcs értékét, és a hello **ügyfél-azonosító** érték. (A konfigurálásra vonatkozó utasítások alkalmazásspecifikusak.)
10. Ezen a lapon, mert ezzel fogja a következő lépés tooset hello engedélyei a tároló hello Ügyfélazonosító értékét másolja.

## <a id="authorize"></a>Hello alkalmazás toouse hello kulcs vagy titkos kulcs engedélyezése
tooauthorize hello alkalmazás tooaccess hello kulcsok vagy titkos hello tárolóban, használja a [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) parancsmag.

Például, ha a tároló neve **ContosoKeyVault** és tooauthorize Ügyfélazonosítója 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, és ezen tooauthorize hello alkalmazás toodecrypt és bejelentkezési kulcsokkal rendelkező kívánt hello-alkalmazás a tároló, futtassa a következő hello:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Ha azt szeretné tooauthorize, hogy ugyanazon alkalmazás tooread titkokat a tárolóban, futtassa a hello következő:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Ha azt szeretné, hogy toouse hardveres biztonsági modul (HSM)
A nagyobb importálhatja és a hardveres biztonsági modulokkal (HSM), amely sosem hagyják el a HSM határait hello kulcsok létrehozásához. hello hardveres biztonsági modulok FIPS 140-2 2. szintű érvényesítve. Ha ez a követelmény nem érvényes tooyou, hagyja ki ezt a szakaszt, és folytassa túl[hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése](#delete).

toocreate a HSM által védett kulcsokat hello kell használnia [Key Vault prémium szintű szolgáltatási réteg toosupport HSM által védett kulcsokat](https://azure.microsoft.com/pricing/free-trial/). Emellett felhívjuk figyelmét, hogy ezt a funkciót az Azure China nem támogatja.

Hello kulcstartó létrehozásakor adja hozzá a hello **- SKU** paraméter:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Szoftveres védelemmel ellátott kulcs (korábban bemutatva) és HSM által védett kulcsokat toothis kulcstároló is hozzáadhat. toocreate HSM által védett kulcs set hello **-cél** paraméter too'HSM ":

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Használhatja a következő parancs tooimport hello származó kulcsok egy. PFX-fájlt a számítógépen. Ez a parancs hello kulcs importálja a Key Vault szolgáltatás hello a HSM-EK:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


hello a következő paranccsal importálhatók, egy "állapotba hozása a saját kulcs" (BYOK) csomagot. Ebben a forgatókönyvben lehetővé teszi, hogy a kulcs létrehozása a helyi HSM-ben, és hello kulcs elhagyná a HSM határait hello nélkül a Key Vault szolgáltatás hello tooHSMs továbbítható:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Leírja, hogyan részletes toogenerate a BYOK-csomag, lásd: [hogyan toogenerate és átviteli HSM által védett kulcsok Azure Key vault](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése
Ha már nincs szüksége hello kulcstartó és hello kulcs vagy titkos kódra, törölheti hello segítségével hello kulcstároló [Remove-AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault) parancsmagot:

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Vagy egy teljes Azure erőforráscsoport, ide tartozik az hello kulcstartó és abba a csoportba más erőforrások törlése:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Egyéb Azure PowerShell-parancsmagok
Egyéb parancsok, amelyek hasznosak lehetnek az Azure Key Vault kezeléséhez:

* `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Ez a parancs táblázatos formában jeleníti meg az összes kulcsot és a megadott tulajdonságokat.
* `$Keys[0]`: Ez a parancs hello megadott kulcs tulajdonságainak teljes listáját jeleníti meg.
* `Get-AzureKeyVaultSecret`: Ez a parancs táblázatos formában jeleníti meg az összes titkos kód nevét és a megadott tulajdonságokat.
* `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Példa hogyan tooremove egy adott kulcs.
* `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Példa hogyan tooremove egy adott titkos kód.

## <a id="next"></a>Következő lépések
Az Azure Key Vault webalkalmazásban való használatáról a [Use Azure Key Vault from a Web Application](key-vault-use-from-web-application.md) (Az Azure Key Vault webalkalmazással való használata) című témakörben találhat további útmutatást.

toosee hogyan használja a kulcstároló, lásd: [Azure Key Vault naplózása](key-vault-logging.md).

Az Azure Key Vault legújabb hello Azure PowerShell-parancsmagok listáját lásd: [Azure Key Vault parancsmagjainak](/powershell/module/azurerm.keyvault/#key_vault).

Programozási hivatkozások: [hello Azure Key Vault fejlesztői útmutatója](key-vault-developers-guide.md).
