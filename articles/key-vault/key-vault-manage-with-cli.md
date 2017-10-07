---
title: "az Azure Key tároló parancssori felület használatával aaaManage |} Microsoft Docs"
description: "Az oktatóanyag tooautomate gyakori feladatok használata a Key Vault hello parancssori felület használatával"
services: key-vault
documentationcenter: 
author: BrucePerlerMS
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: bruceper
ms.openlocfilehash: 9ef506faa67e1f0db5b9e303300d63b135ddd7b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli"></a>Kezelheti a Key Vault parancssori felület használatával

Az Azure Key Vault a legtöbb régióban elérhető. További információkért lásd: hello [Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Bevezetés

Használja az oktatóanyag toohelp kap használatába az Azure Key Vault toocreate megerősített tárolókat (kulcstartókat) toostore, az Azure-ban, és kezelheti a titkosítási kulcsok és titkos az Azure-ban. Az bemutatja, hogyan hello folyamatot, amely segítségével az Azure platformfüggetlen parancssori felületre toocreate egy kulcsot vagy jelszót, majd használható az Azure-alkalmazások tartalmazó. Ezután bemutatja, hogyan alkalmazás használhatja adott kulcsot vagy jelszót.

**Becsült idő toocomplete:** 20 perc

> [!NOTE]
> Ez az oktatóanyag nem tartalmazza a hogyan toowrite hello Azure tartalmazó alkalmazás hello lépések egyikét, amely bemutatja, hogyan tooauthorize egy alkalmazás toouse kulcsok vagy titkos kulcs hello tároló.
> 
> Jelenleg nem konfigurálhatja az Azure Key Vault hello Azure-portálon. Ehelyett használja a platformfüggetlen parancssori felületre útmutatóhoz. Vagy az Azure PowerShell útmutatásért lásd: [ebben az egyenértékű oktatóanyagban](key-vault-get-started.md).
> 
> 

Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az Azure Key Vault?) című cikkben talál.

## <a name="prerequisites"></a>Előfeltételek

toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:

* Egy előfizetés tooMicrosoft Azure. Ha még nem rendelkezik ilyennel, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial).
* Parancssori felület 0.9.1 verzió vagy újabb. tooinstall hello legújabb verzióját, és csatlakozzon a tooyour Azure-előfizetéssel, lásd: [telepítése és konfigurálása az Azure platformfüggetlen parancssori felületre hello](../cli-install-nodejs.md).
* Egy alkalmazás, amely konfigurált toouse hello kulcsot vagy jelszót, amely ebben az oktatóanyagban létrehozhat lesz. A mintaalkalmazás érhető el a hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343). Útmutatásért lásd: hello kísérő információs fájlt.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Segítség az Azure platformfüggetlen parancssori felülettel

Ez az oktatóanyag feltételezi, hogy Ön ismeri a hello parancssori felület (Bash, terminál, parancssor)

hello – súgó vagy -h paraméter lehet tooview súgó használt parancsok. Alternatív megoldásként hello azure súgó [parancs] [kapcsolók] formátumban is használt tooreturn hello ugyanazokat az információkat. Például a hello következő parancsokat minden visszatérési hello ugyanazokat az információkat:

    azure account set --help

    azure account set -h

    azure help account set

A parancs által igényelt hello paraméterekről kétségei vannak, tekintse meg a toohelp használatával – súgó, -h vagy azure [parancs].

A következő oktatóanyagok tooget ismeri az Azure Resource Manager az Azure platformfüggetlen parancssori felület hello is olvasható:

* [Hogyan tooinstall és konfigurálása az Azure platformfüggetlen parancssori felülettel](../cli-install-nodejs.md)
* [Az Azure platformfüggetlen parancssori felületre az Azure erőforrás-kezelő használatával](../xplat-cli-azure-resource-manager.md)

## <a name="connect-tooyour-subscriptions"></a>Csatlakozás tooyour előfizetések

egy szervezeti fiókjával, a következő parancs használata hello toolog:

    azure login -u username -p password

vagy ha a kívánt toolog interaktív beírásával

    azure login

> [!NOTE]
> hello bejelentkezési metódus csak olyan szervezeti fiók működik. Egy szervezeti fiók egy olyan felhasználó, a szervezet által felügyelt, és a szervezet Azure Active Directory-bérlő definiálva.
> 
> 

Ha jelenleg nem rendelkezik a szervezeti fiókkal, és tooyour Azure-előfizetés a Microsoft-fiók toolog használnak, egyszerűen létrehozhat, egyet az hello a következő lépéseket.

1. Bejelentkezési toohello bejelentkezési toohello [Azure felügyeleti portálon](https://manage.windowsazure.com/), majd kattintson az Active Directory.
2. Ha nem a könyvtár létezik, válassza ki a könyvtár létrehozása, és hello adja meg a kért információkat.
3. Válassza ki azt a címtárat, és új felhasználó hozzáadásához. Ennek a felhasználónak egy szervezeti fiók. Hello felhasználó hello létrehozásakor fog megadott hello felhasználó e-mail címének és ideiglenes jelszót. Ez az információ akkor menteni, mert használatban van egy másik lépés.
4. Hello portálról jelölje be a beállításokat, és válassza a rendszergazdák. Válassza ki a hozzáadása, és adja hozzá az új felhasználó hello társadminisztrátoraként. Ez lehetővé teszi, hogy hello szervezeti fiók toomanage az Azure-előfizetéshez.
5. Végezetül kijelentkezés hello Azure-portálon, és jelentkezzen be oda hello új szervezeti fiók használatával. Ha ez a fiók első alkalommal bejelentkezik hello, felszólító toochange hello jelszó fogja.

A Microsoft Azure rendelkező szervezeti fiókkal kapcsolatos további információkért lásd: [iratkozzon fel a Microsoft Azure-előfizetésre](../active-directory/sign-up-organization.md).

Ha több előfizetéssel rendelkezik, és szeretné, hogy egy adott egy toouse toospecify az Azure Key Vaulthoz, írja be a fiókhoz toosee hello előfizetések a következő hello:

    azure account list

Ezt követően toospecify hello előfizetés toouse, típus:

    azure account set <subscription name>

Azure platformfüggetlen parancssori felületre konfigurálásával kapcsolatos további információkért lásd: [hogyan tooInstall és konfigurálása Azure platformfüggetlen parancssori felület](../cli-install-nodejs.md).

## <a name="switch-toousing-azure-resource-manager"></a>Váltson Azure Resource Manager toousing
Key Vault hello Azure Resource Manager megköveteli, tehát hello tooswitch tooAzure Resource Manager módra a következő:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Új erőforráscsoport létrehozása
Azure Resource Manager használatakor minden kapcsolódó erőforrás egy erőforráscsoportban jönnek létre. Ebben az oktatóanyagban létrehozunk egy új erőforráscsoportot "ContosoResourceGroup".

    azure group create 'ContosoResourceGroup' 'East Asia'

hello első paramétere az erőforráscsoport neve, és hello második paramétere hello helyét. A helyen, paranccsal hello `azure location list` tooidentify hogyan toospecify egy másodlagos helyet toohello egy ebben a példában. Ha további tájékoztatásra van szüksége, írja be:`azure help location`

## <a name="register-hello-key-vault-resource-provider"></a>Hello Key Vault erőforrás-szolgáltató regisztrálása
Győződjön meg arról, hogy Key Vault erőforrás-szolgáltató regisztrálva van az előfizetésben:

`azure provider register Microsoft.KeyVault`

Ez csak egyszer történik előfizetésenként toobe kell.

## <a name="create-a-key-vault"></a>Kulcstartó létrehozása

Használjon hello `azure keyvault create` parancs toocreate kulcstároló. Ez a parancsfájl három kötelező paraméterrel rendelkezik: erőforráscsoport-nevet, a kulcstároló neve és hello földrajzi helyet.

Például ha hello tároló neve ContosoKeyVault, ContosoResourceGroup hello erőforráscsoport neve és helye hello Kelet-Ázsia, írja be:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

a parancs kimenetének hello az újonnan létrehozott kulcstartó hello tulajdonságok láthatók. hello két legfontosabb tulajdonságai a következők:

* **Név**: hello például ez az ContosoKeyVault. Ezt a nevet fogja majd más Key Vault parancsmagokban is megadni.
* **vaultUri**: hello például ez az https://contosokeyvault.vault.azure.net. A tárolót a REST API-ján keresztül használó alkalmazásoknak ezt az URI-t kell használniuk.

Azure-fiókja most engedélyezett tooperform bármilyen műveletet ezt a kulcsot tároló van. Egyelőre senki másnak nincs erre engedélye.

## <a name="add-a-key-or-secret-toohello-key-vault"></a>Kulcs vagy titkos toohello kulcstároló hozzáadása

Ha meg szeretné az Azure Key Vault toocreate szoftveres védelemmel ellátott kulcs, használja a hello `azure key create` parancsot, és írja be a következő hello:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Azonban ha meglévő kulcs egy helyi fájlba, amelyet az tooupload tooAzure Key Vault softkey.pem nevű fájlba menti a .pem fájl található, írja be a hello tooimport hello kulcsot következő hello. PEM-fájl, hello kulcs védetté hello Key Vault szolgáltatás szoftveres:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Hello kulcs létrehozása vagy tooAzure Key Vaultba feltöltött az URI használatával hivatkozhat. Használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways hello aktuális verzióra, és használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget ezt a verziót.

tooadd egy titkos toohello tárolóban, amely egy SQLPassword nevű jelszót, és Pa$ $w0rd tooAzure kulcstároló, a következő típus hello hello értékkel rendelkezik, amelyek:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Ezt a jelszót, hogy tooAzure Key Vault hozzáadta az URI használatával hivatkozhat. Használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways hello aktuális verzióra, és használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget ezt a verziót.

Tekintse meg hello kulcsok vagy titkos, újonnan létrehozott:

* tooview a, írja be:`azure keyvault key list --vault-name 'ContosoKeyVault'`
* tooview a titkos, típus:`azure keyvault secret list --vault-name 'ContosoKeyVault'`

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
2. Hello bal oldalon kattintson **Active Directory**, majd válassza ki az alkalmazást regisztrálni hello directory. <br> <br> 

>[!NOTE] 
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
tooauthorize hello alkalmazás tooaccess hello kulcsok vagy titkos hello tárolóban, használjon hello `azure keyvault set-policy` parancsot.

Például ha a tároló neve tooauthorize Ügyfélazonosítója 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, és ezen tooauthorize hello alkalmazás toodecrypt és bejelentkezési kulccsal rendelkező a tárolóban lévő kívánt ContosoKeyVault és hello alkalmazás, majd futtassa hello a következő:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'

> [!NOTE]
> Ha futtatja a Windows parancssori ablakba, inkább szimpla idézőjelet cserélje le, és is escape-hello belső dupla idézőjelek között. Például: "[\"visszafejtéséhez\",\"bejelentkezési\"]".
> 
> 

Ha azt szeretné tooauthorize, hogy ugyanazon alkalmazás tooread titkokat a tárolóban, futtassa a hello következő:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'

## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a>Ha azt szeretné, hogy toouse hardveres biztonsági modul (HSM)
A nagyobb importálhatja és a hardveres biztonsági modulokkal (HSM), amely sosem hagyják el a HSM határait hello kulcsok létrehozásához. hello hardveres biztonsági modulok FIPS 140-2 2. szintű érvényesítve. Ha ez a követelmény nem érvényes tooyou, hagyja ki ezt a szakaszt, és folytassa túl[hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése](#delete-the-key-vault-and-associated-keys-and-secrets).

toocreate a HSM által védett kulcsok, a tároló-előfizetéssel kell rendelkeznie, amely támogatja a HSM által védett kulcsokat.

Hello keyvault létrehozásakor adja hozzá a hello "sku" paramétert:

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Is hozzáadhat szoftveresen védett (korábban bemutatott), és HSM által védett kulcsokat toothis tároló. toocreate HSM által védett kulcs set hello cél paraméter too'HSM ":

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

A .pem fájlt a számítógépen a parancs tooimport egy kulcsot következő hello is használhatja. Ez a parancs hello kulcs importálja a Key Vault szolgáltatás hello a HSM-EK:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

hello a következő paranccsal importálhatók, egy "állapotba hozása a saját kulcs" (BYOK) csomagot. Ez lehetővé teszi, hogy a kulcs létrehozása a helyi HSM-ben, és hello kulcs elhagyná a HSM határait hello nélkül a Key Vault szolgáltatás hello tooHSMs továbbítható:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Leírja, hogyan részletes toogenerate a BYOK-csomag, lásd: [hogyan toouse HSM-Protected kulcsokat az Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a>Hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése
Ha már nincs szüksége hello kulcstartó és hello kulcs vagy titkos kódra, törölheti a hello kulcstároló hello azure keyvault delete parancs használatával:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Vagy egy teljes Azure erőforráscsoport, ide tartozik az hello kulcstartó és abba a csoportba más erőforrások törlése:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Egyéb Azure platformfüggetlen parancssori felület parancsai
Egyéb parancsok, akkor lehet hasznos, ha az Azure Key Vault kezeléséhez.

Ez a parancs táblázatos formában jeleníti meg az összes kulcsot és a kijelölt tulajdonságok:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Ez a parancs hello megadott kulcs tulajdonságainak teljes listáját jeleníti meg:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Ez a parancs táblázatos formában jeleníti meg az összes titkos kód nevét és a kijelölt tulajdonságokat:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Íme egy példa bemutatja, hogyan tooremove egy adott kulcs:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Íme egy példa bemutatja, hogyan tooremove egy adott titkos kód:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Következő lépések
Programozási hivatkozások: [hello Azure Key Vault fejlesztői útmutatója](key-vault-developers-guide.md).

