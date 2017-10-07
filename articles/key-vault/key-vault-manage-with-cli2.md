---
title: "az Azure Key tároló parancssori felület használatával aaaManage |} Microsoft Docs"
description: "Az oktatóanyag tooautomate gyakori feladatok használata a Key Vault hello 2.0 parancssori felület használatával"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ambapat
ms.openlocfilehash: 76855c0ea09b6b307159468382a6a63627205556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli-20"></a>Kezelheti a Key Vault 2.0 parancssori felület használatával
Az Azure Key Vault a legtöbb régióban elérhető. További információkért lásd: hello [Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Bevezetés
Használja az oktatóanyag toohelp kap használatába az Azure Key Vault toocreate megerősített tárolókat (kulcstartókat) toostore, az Azure-ban, és kezelheti a titkosítási kulcsok és titkos az Azure-ban. Az bemutatja, hogyan hello folyamatot, amely segítségével az Azure platformfüggetlen parancssori felületre toocreate egy kulcsot vagy jelszót, majd használható az Azure-alkalmazások tartalmazó. Ezután bemutatja, hogyan alkalmazás használhatja adott kulcsot vagy jelszót.

**Becsült idő toocomplete:** 20 perc

> [!NOTE]
> Ez az oktatóanyag nem tartalmazza a hogyan toowrite hello Azure tartalmazó alkalmazás hello lépések egyikét, amely bemutatja, hogyan tooauthorize egy alkalmazás toouse kulcsok vagy titkos kulcs hello tároló.
>
> A oktatóanyag használja az Azure CLI legújabb 2.0 hello.
>
>

Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az Azure Key Vault?) című cikkben talál.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:

* Egy előfizetés tooMicrosoft Azure. Ha még nem rendelkezik ilyennel, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial).
* Parancssori felület 2.0-s vagy újabb verziója. tooinstall hello legújabb verzióját, és csatlakozzon a tooyour Azure-előfizetéssel, lásd: [telepítése és konfigurálása az Azure platformfüggetlen parancssori felület 2.0 hello](/cli/azure/install-azure-cli).
* Egy alkalmazás, amely konfigurált toouse hello kulcsot vagy jelszót, amely ebben az oktatóanyagban létrehozhat lesz. A mintaalkalmazás érhető el a hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343). Útmutatásért lásd: hello kísérő információs fájlt.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Segítség az Azure platformfüggetlen parancssori felülettel
Ez az oktatóanyag feltételezi, hogy Ön ismeri a hello parancssori felület (Bash, terminál, parancssor)

hello – súgó vagy -h paraméter lehet tooview súgó használt parancsok. Alternatív megoldásként hello azure súgó [parancs] [kapcsolók] formátumban is használt tooreturn hello ugyanazokat az információkat. Például a hello következő parancsokat minden visszatérési hello ugyanazokat az információkat:

```
az account set --help
az account set -h
```

A parancs által igényelt hello paraméterekről kétségei vannak, tekintse meg a toohelp használatával – súgó, – h vagy az [parancs] segítségével.

A következő oktatóanyagok tooget ismeri az Azure Resource Manager az Azure platformfüggetlen parancssori felület hello is olvasható:

* [Az Azure parancssori felület telepítése](/cli/azure/install-azure-cli)
* [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli)

## <a name="connect-tooyour-subscriptions"></a>Csatlakozás tooyour előfizetések
egy szervezeti fiókjával, a következő parancs használata hello toolog:

```
az login -u username@domain.com -p password
```

vagy ha a kívánt toolog interaktív beírásával

```
az login
```

Ha több előfizetéssel rendelkezik, és szeretné, hogy egy adott egy toouse toospecify az Azure Key Vaulthoz, írja be a fiókhoz toosee hello előfizetések a következő hello:

```
az account list
```

Ezt követően toospecify hello előfizetés toouse, típus:

```
az account set --subscription <subscription name or ID>
```

Azure platformfüggetlen parancssori felületre konfigurálásával kapcsolatos további információkért lásd: [Azure CLI telepítése](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Új erőforráscsoport létrehozása
Azure Resource Manager használatakor minden kapcsolódó erőforrás egy erőforráscsoportban jönnek létre. Ebben az oktatóanyagban létrehozunk egy új erőforráscsoportot "ContosoResourceGroup".

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

hello első paramétere az erőforráscsoport neve, és hello második paramétere hello helyét. A helyen, paranccsal hello `az account list-locations` tooidentify hogyan toospecify egy másodlagos helyet toohello egy ebben a példában. Ha további tájékoztatásra van szüksége, írja be: `az account list-locations -h`.

## <a name="register-hello-key-vault-resource-provider"></a>Hello Key Vault erőforrás-szolgáltató regisztrálása
Győződjön meg arról, hogy Key Vault erőforrás-szolgáltató regisztrálva van az előfizetésben:

```
az provider register -n Microsoft.KeyVault
```

Ez csak egyszer történik előfizetésenként toobe kell.

## <a name="create-a-key-vault"></a>Kulcstartó létrehozása
Használjon hello `az keyvault create` parancs toocreate kulcstároló. Ez a parancsfájl három kötelező paraméterrel rendelkezik: erőforráscsoport-nevet, a kulcstároló neve és hello földrajzi helyet.

Például ha hello tároló neve ContosoKeyVault, ContosoResourceGroup hello erőforráscsoport neve és helye hello Kelet-Ázsia, írja be:
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

a parancs kimenetének hello az újonnan létrehozott kulcstartó hello tulajdonságok láthatók. hello két legfontosabb tulajdonságai a következők:

* **név**: hello például ez az ContosoKeyVault. Más Key Vault parancsok ezt a nevet fogja használni.
* **vaultUri**: hello például ez az https://contosokeyvault.vault.azure.net. A tárolót a REST API-ján keresztül használó alkalmazásoknak ezt az URI-t kell használniuk.

Azure-fiókja most engedélyezett tooperform bármilyen műveletet ezt a kulcsot tároló van. Egyelőre senki másnak nincs erre engedélye.

## <a name="add-a-key-or-secret-toohello-key-vault"></a>Kulcs vagy titkos toohello kulcstároló hozzáadása
Ha meg szeretné az Azure Key Vault toocreate szoftveres védelemmel ellátott kulcs, használja a hello `az key create` parancsot, és írja be a következő hello:
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
Azonban ha meglévő kulcs egy helyi fájlba, amelyet az tooupload tooAzure Key Vault softkey.pem nevű fájlba menti a .pem fájl található, írja be a hello tooimport hello kulcsot következő hello. PEM-fájl, hello kulcs védetté hello Key Vault szolgáltatás szoftveres:
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
Hello kulcs létrehozása vagy tooAzure Key Vaultba feltöltött az URI használatával hivatkozhat. Használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways hello aktuális verzióra, és használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget ezt a verziót.

tooadd egy titkos toohello tárolóban, amely egy SQLPassword nevű jelszót, és Pa$ $w0rd tooAzure kulcstároló, a következő típus hello hello értékkel rendelkezik, amelyek:
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
Ezt a jelszót, hogy tooAzure Key Vault hozzáadta az URI használatával hivatkozhat. Használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways hello aktuális verzióra, és használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget ezt a verziót.

Tekintse meg hello kulcsok vagy titkos, újonnan létrehozott:

* tooview a, írja be:`az keyvault key list --vault-name 'ContosoKeyVault'`
* tooview a titkos, típus:`az keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Alkalmazás regisztrálása az Azure Active Directory szolgáltatásban
Ezt a lépést általában egy fejlesztő végzi egy másik számítógépről. Nincs adott tooAzure Key Vault, de meg adva, feltétlenül teljességében.

> [!IMPORTANT]
> toocomplete hello oktatóanyag, a fiók, hello tárolóban, és ebben a lépésben regisztrálandó hello alkalmazás kell lenniük hello Azure könyvtárába.
>
>

A kulcstartót használó alkalmazásoknak az Azure Active Directoryból származó jogkivonat használatával kell hitelesítést végezniük. toodo a, hello hello alkalmazás tulajdonosának először regisztrálnia kell a hello alkalmazás az Azure Active Directoryban. A regisztrációs hello végén hello alkalmazás tulajdonosa lekérdezi a következő értékek hello:

* Egy **Alkalmazásazonosító** (más néven Ügyfélazonosítót) és **hitelesítési kulcs** (más néven hello közös titkos kulcs). hello alkalmazás e ezen értékek tooAzure Active Directory, a jogkivonat tooget mindegyikét. Ez függ hello alkalmazás toodo hogyan hello alkalmazás van konfigurálva. A Key Vault mintaalkalmazás hello hello alkalmazás tulajdonosa adja meg ezeket az értékeket hello app.config fájlban.

tooregister hello alkalmazás Azure Active Directoryban:

1. Bejelentkezés toohello Azure-portálon.
2. Hello bal oldalon kattintson **Azure Active Directory**, majd válassza ki az alkalmazást regisztrálni hello directory. <br> <br> 

> [!Note] 
> Ki kell választania a hello ugyanabban a könyvtárban, amely tartalmazza a kulcstartót létrehozó Azure-előfizetés hello. Ha nem tudja, melyik címtárban ez, kattintson a **beállítások**, a kulcstartót létrehozó hello előfizetés azonosítása és hello utolsó oszlopban megjelenített megjegyzés hello hello könyvtár nevét.

3. Kattintson az **APPLICATIONS** (ALKALMAZÁSOK) elemre. Nincs alkalmazás tooyour directory lettek hozzáadva, ha ezen a lapon jelenjen-e csak hello **hozzáadhat egy alkalmazást** hivatkozásra. Hello hivatkozásra, vagy másik lehetőségként kattinthat hello **hozzáadása** hello parancssávon.
4. A hello **alkalmazás hozzáadása** hello varázsló **miről szeretne toodo?** lapján kattintson **a szerveztem által fejlesztett alkalmazás hozzáadása**.
5. A hello **adja meg azt az alkalmazás** lapon adja meg az alkalmazás nevét, és válassza **WEB APPLICATION AND/OR WEB API** (hello alapértelmezett). Kattintson a Tovább hello ikonra.
6. A hello **alkalmazás tulajdonságainak** adja meg azokat a hello **SIGN-ON URL** és **APP ID URI** a webalkalmazás. Ha az alkalmazás nem rendelkezik ezekkel az értékekkel, ehhez a lépéshez nem létező értékeket is megadhat (például http://test1.contoso.com mindkét mezőhöz). Nem számít, ha ezeken a webhelyeken létezik; a fontos, hogy hello app ID URI mindegyik alkalmazáshoz különböző alkalmazás esetében minden egyes a címtárban. hello directory használja a karakterlánc tooidentify az alkalmazást.
7. Kattintson a hello teljes ikon toosave hello varázslóban a módosítások.
8. Hello gyors kezdés lapon kattintson **KONFIGURÁLÁSA**.
9. Görgessen toohello **kulcsok** szakasz, válassza ki a hello időtartama, majd kattintson a **mentése**. hello oldal frissül, és most kijelzi a kulcs értékét. Be kell állítania az alkalmazás a kulcs értékét, és a hello **ügyfél-azonosító** érték. (A beállítás menete alkalmazásonként eltérő lehet.)
10. Ezen a lapon, mert ezzel fogja a következő lépés tooset hello engedélyei a tároló hello Ügyfélazonosító értékét másolja.

## <a name="authorize-hello-application-toouse-hello-key-or-secret"></a>Hello alkalmazás toouse hello kulcs vagy titkos kulcs engedélyezése
tooauthorize hello alkalmazás tooaccess hello kulcsok vagy titkos hello tárolóban, használjon hello `az keyvault set-policy` parancsot.

Például ha a tároló neve tooauthorize Ügyfélazonosítója 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, és ezen tooauthorize hello alkalmazás toodecrypt és bejelentkezési kulccsal rendelkező a tárolóban lévő kívánt ContosoKeyVault és hello alkalmazás, majd futtassa hello a következő:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Ha azt szeretné tooauthorize, hogy ugyanazon alkalmazás tooread titkokat a tárolóban, futtassa a hello következő:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a>Ha azt szeretné, hogy toouse hardveres biztonsági modul (HSM)
A nagyobb importálhatja és a hardveres biztonsági modulokkal (HSM), amely sosem hagyják el a HSM határait hello kulcsok létrehozásához. hello hardveres biztonsági modulok FIPS 140-2 2. szintű érvényesítve. Ha ez a követelmény nem érvényes tooyou, hagyja ki ezt a szakaszt, és folytassa túl[hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése](#delete-the-key-vault-and-associated-keys-and-secrets).

toocreate a HSM által védett kulcsok, a tároló-előfizetéssel kell rendelkeznie, amely támogatja a HSM által védett kulcsokat.

Hello keyvault létrehozásakor adja hozzá a hello "sku" paramétert:

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
Is hozzáadhat szoftveresen védett (korábban bemutatott), és HSM által védett kulcsokat toothis tároló. toocreate HSM által védett kulcs set hello cél paraméter too'HSM ":

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

A .pem fájlt a számítógépen a parancs tooimport egy kulcsot következő hello is használhatja. Ez a parancs hello kulcs importálja a Key Vault szolgáltatás hello a HSM-EK:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
hello a következő paranccsal importálhatók, egy "állapotba hozása a saját kulcs" (BYOK) csomagot. Ez lehetővé teszi, hogy a kulcs létrehozása a helyi HSM-ben, és hello kulcs elhagyná a HSM határait hello nélkül a Key Vault szolgáltatás hello tooHSMs továbbítható:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
Leírja, hogyan részletes toogenerate a BYOK-csomag, lásd: [hogyan toouse HSM-Protected kulcsokat az Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a>Hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése
Ha már nincs szüksége hello kulcstartó és hello kulcs vagy titkos kódra, törölheti hello segítségével hello kulcstároló `az keyvault delete` parancs:

```
az keyvault delete --name 'ContosoKeyVault'
```

Vagy egy teljes Azure erőforráscsoport, ide tartozik az hello kulcstartó és abba a csoportba más erőforrások törlése:

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Egyéb Azure platformfüggetlen parancssori felület parancsai
Egyéb parancsok, akkor lehet hasznos, ha az Azure Key Vault kezeléséhez.

Ez a parancs táblázatos formában jeleníti meg az összes kulcsot és a kijelölt tulajdonságok:

az keyvault kulcslista--tároló-neve "ContosoKeyVault"

Ez a parancs hello megadott kulcs tulajdonságainak teljes listáját jeleníti meg:

az keyvault kulcs megjelenítése--tároló-neve "ContosoKeyVault"--"ContosoFirstKey" neve

Ez a parancs táblázatos formában jeleníti meg az összes titkos kód nevét és a kijelölt tulajdonságokat:

az keyvault titkos lista--tároló-neve "ContosoKeyVault"

Íme egy példa bemutatja, hogyan tooremove egy adott kulcs:

az keyvault kulcs törlése--tároló-neve "ContosoKeyVault"--"ContosoFirstKey" neve

Íme egy példa bemutatja, hogyan tooremove egy adott titkos kód:

az keyvault titkos kulcs törlése--tároló-neve "ContosoKeyVault"--"SQLPassword" neve


## <a name="next-steps"></a>Következő lépések
A kulcstároló parancsok teljes Azure parancssori felület referenciáért lásd: [Key Vault CLI-hivatkozás](/cli/azure/keyvault)

Programozási hivatkozások: [hello Azure Key Vault fejlesztői útmutatója](key-vault-developers-guide.md).
