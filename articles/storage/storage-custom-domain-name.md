---
title: "egy egyéni tartománynevet a Azure Blob storage-végponthoz aaaConfigure |} Microsoft Docs"
description: "Az Azure portál toomap hello saját kanonikus név (CNAME) toohello Blob storage endpoint használja egy Azure Storage-fiókot."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: aaafd8c5-eacb-49dc-8c8b-3f7011ad5e92
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: marsma
ms.openlocfilehash: 6cca6a6e1dbb69e7078df7ed11b04e8b921ec2f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Egyéni tartományév konfigurálása a Blob Storage-végponthoz

A blob az Azure storage-fiók adataihoz fér hozzá az egyéni tartománynév konfigurálása alapértelmezett végpont hello a Blob Storage `<storage-account-name>.blob.core.windows.net`. Ha leképez egy egyéni tartomány és altartomány például **www.contoso.com** toohello blob végpont a tárfiók, a felhasználók ezután hozzáférhetnek blob-, hogy a tartomány a tárfiókban lévő adatokat.

> [!IMPORTANT]
> Az Azure Storage nem még natív módon támogatja a HTTPS az egyéni tartományokat. Jelenleg is [hello Azure CDN tooaccess blobok használja az egyéni tartomány HTTPS-KAPCSOLATON keresztül](./storage-https-custom-domain-cdn.md).
>

hello következő táblázatban a blob adatok nevű tárfiók néhány minta URL-címek **mystorageaccount**. az egyéni tartomány hello regisztrálva hello storage-fiók **www.contoso.com**:

| Erőforrás típusa | Alapértelmezett URL-címe | Az egyéni tartomány URL-címe |
| --- | --- | --- |
| Tárfiók | http://mystorageaccount.BLOB.Core.Windows.NET | http://www.contoso.com |
| Blob |http://mystorageaccount.BLOB.Core.Windows.NET/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Legfelső szintű tárolója | http://mystorageaccount.BLOB.Core.Windows.NET/myblob vagy http://mystorageaccount.blob.core.windows.net/$ legfelső szintű/myblob| http://www.contoso.com/myblob vagy http://www.contoso.com/$ legfelső szintű/myblob |

## <a name="direct-vs-intermediary-domain-mapping"></a>Közvetlen és köztes tartomány leképezése

Az egyéni tartomány toohello blob végpont a tárfiók két módon toopoint nincsenek: közvetlen leképezése és hello használata CNAME *asverify* közvetítő altartomány.

### <a name="direct-cname-mapping"></a>Közvetlen CNAME-leképezés

hello első, és a legegyszerűbb, metódus, amely hozzárendeli az egyéni tartomány és altartomány közvetlen toohello blob-végpont kanonikus név (CNAME) rekord toocreate. Egy olyan CNAME rekordot az egy tartomány nevét (DNS) rendszer szolgáltatása rendeli hozzá a forrás tartományi tooa cél tartományhoz. Ebben az esetben hello forrástartomány a saját egyéni tartomány és altartomány, például *www.contoso.com*. hello cél tartománya a Blob-szolgáltatásvégpont, például  *mystorageaccount.BLOB.Core.Windows.NET*.

hello közvetlen módszer tárgyalja [regisztrálni az egyéni tartománynév](#register-a-custom-domain).

### <a name="intermediary-mapping-with-asverify"></a>A köztes leképezési *asverify*

hello második módszer is használja a CNAME-rekordot, de először alkalmazza a ismeri fel az Azure tooavoid állásidő különleges altartomány: **asverify**.

hello folyamat leképezésének az egyéni tartomány tooa blob végpont eredményezhet hello tartomány állásidő rövid idő alatt közben próbál regisztrálni hello [Azure-portálon](https://portal.azure.com). Ha az egyéni tartomány jelenleg támogat az alkalmazás a szolgáltatásiszint-szerződés (SLA), amelyhez az állásidő, akkor használhatja a hello Azure *asverify* altartomány egy köztes regisztrációs lépés. Ez a köztes lépés biztosítja a felhasználók addig, a tartomány képes tooaccess amíg hello Rendelje hozzá kerül sor.

hello közvetítő metódus tárgyalja [hello segítségével egyéni tartományt regisztrálása *asverify* altartomány](#register-a-custom-domain-using-the-asverify-subdomain).

## <a name="register-a-custom-domain"></a>Az egyéni tartománynév regisztrálása
Használja az eljárás tooregister az egyéni tartomány Ha kérdése nincs folyamatban röviden nem érhető el tooyour felhasználók hello tartománnyal kapcsolatos információk, vagy ha az egyéni tartomány jelenleg nem üzemeltet egy alkalmazást.

Ha az egyéni tartomány jelenleg támogat, amelyek nem rendelkeznek leállási kérelmet, hajtsa végre a hello eljárásban ismertetett [hello segítségével egyéni tartományt regisztrálása *asverify* altartomány](#register-a-custom-domain-using-the-asverify-subdomain).

tooconfigure egy egyéni tartománynevet, a új CNAME rekordot kell létrehoznia a DNS-ben. hello CNAME rekordot a tartománynévhez tartozó alias határozza meg. Ebben az esetben azt a maps az egyéni tartomány toohello Blob storage endpoint a tárfiók hello címét.

Általában a tartomány DNS-beállítások a tartományregisztráló webhelyen keresztül kezelheti. Minden tartományregisztráló egy CNAME rekordot a telepítésükhöz hasonló, csak metódust tartalmaz, de hello koncepció van hello azonos. Néhány alapvető tartomány regisztrációs csomagok nem képes DNS-konfiguráció, így tooupgrade szükség lehet a tartomány regisztrációs csomagot hello CNAME rekord létrehozása előtt.

1. Keresse meg a storage-fiókot tooyour hello [Azure-portálon](https://portal.azure.com).
1. A **BLOB szolgáltatás** hello menü paneljén válassza **egyéni tartomány** tooopen hello *egyéni tartomány* panelen.
1. Jelentkezzen be tooyour tartomány regisztráló webhelyén, és lépjen toohello lapra DNS kezeléséhez. Ezt a **Tartománynév**, **DNS**, **Névkiszolgáló kezelése** vagy hasonló területen találja.
1. Hello szakaszban található CNAME kezeléséhez. Valószínűleg toogo tooan speciális beállításai oldal és hello szavakat keresi **CNAME**, **Alias**, vagy **altartományok**.
1. Új CNAME rekordot kell létrehozni, és adja meg például egy altartomány alias **www** vagy **fényképek**. Adja meg egy állomásnevet, amely a Blob-szolgáltatásvégpont, hello formátumban **mystorageaccount.blob.core.windows.net** (ahol *mystorageaccount* hello a tárfiók neve). hello #1 elem megjelenik hello állomás neve toouse *egyéni tartomány* hello paneljén [Azure-portálon](https://portal.azure.com).
1. Hello szövegmezőben a hello *egyéni tartomány* hello paneljén [Azure-portálon](https://portal.azure.com), adja meg az egyéni tartomány, beleértve a hello altartomány hello nevét. Ha a tartomány például **contoso.com** és a altartomány alias **www**, adja meg **www.contoso.com**. Ha a altartomány **fényképek**, adja meg **photos.contoso.com**. hello altartomány van *szükséges*.
1. Válassza ki **mentése** a hello *egyéni tartomány* panel tooregister az egyéni tartomány. Ha hello regisztráció sikeres, látni fogja, hogy a tárfiók sikeresen frissült a portál értesítései.

Miután az új CNAME-rekord propagálása DNS-en keresztül, a felhasználók tekintheti meg Blobadatok az egyéni tartomány mindaddig, amíg hello megfelelő engedélyek lettek.

## <a name="register-a-custom-domain-using-hello-asverify-subdomain"></a>Regisztrálja a hello segítségével egyéni tartományt *asverify* altartomány
Ez az eljárás tooregister használja az egyéni tartomány az egyéni tartomány van jelenleg támogatása az SLA-t, amely megköveteli, hogy az alkalmazás előfordulhatnak állásidő nélkül. Hozzon létre egy CNAME REKORDOT, mely az `asverify.<subdomain>.<customdomain>` túl`asverify.<storageaccount>.blob.core.windows.net`, előre regisztrálhatja a tartomány az Azure-ral. Ezután létrehozhat egy második CNAME REKORDOT, mely az `<subdomain>.<customdomain>` túl`<storageaccount>.blob.core.windows.net`, ekkor a forgalom tooyour egyéni tartomány lesz irányított tooyour blob végpontja.

Hello **asverify** altartomány altartománya különleges ismeri az Azure-ban. Által fertőző `asverify` tooyour saját altartomány, engedélyezi az Azure toorecognize az egyéni tartomány hello tartomány DNS-rekordot hello módosítása nélkül. Hello tartomány DNS-rekordot hello módosításakor lesz csatlakoztatott toohello blob végpont állásidő nélkül.

1. Keresse meg a storage-fiókot tooyour hello [Azure-portálon](https://portal.azure.com).
1. A **BLOB szolgáltatás** hello menü paneljén válassza **egyéni tartomány** tooopen hello *egyéni tartomány* panelen.
1. Jelentkezzen be tooyour DNS-szolgáltatónál webhely, és lépjen toohello lapra DNS kezeléséhez. Ezt a **Tartománynév**, **DNS**, **Névkiszolgáló kezelése** vagy hasonló területen találja.
1. Hello szakaszban található CNAME kezeléséhez. Valószínűleg toogo tooan speciális beállításai oldal és hello szavakat keresi **CNAME**, **Alias**, vagy **altartományok**.
1. Új CNAME rekordot kell létrehozni, és adjon meg egy altartomány alias, amely tartalmazza az hello *asverify* altartomány. Például **asverify.www** vagy **asverify.photos**. Adja meg egy állomásnevet, amely a Blob-szolgáltatásvégpont, hello formátumban **asverify.mystorageaccount.blob.core.windows.net** (ahol **mystorageaccount** hello a tárfiók neve). hello állomás neve toouse megjelenik hello #2 elemét *egyéni tartomány* hello paneljén [Azure-portálon](https://portal.azure.com).
1. Hello szövegmezőben a hello *egyéni tartomány* hello paneljén [Azure-portálon](https://portal.azure.com), adja meg az egyéni tartomány, beleértve a hello altartomány hello nevét. Nem tartalmaznak *asverify*. Ha a tartomány például **contoso.com** és a altartomány alias **www**, adja meg **www.contoso.com**. Ha a altartomány **fényképek**, adja meg **photos.contoso.com**. hello altartomány szükség.
1. Jelölje be hello **CNAME rekord közvetett ellenőrzésének használata** jelölőnégyzetet.
1. Válassza ki **mentése** a hello *egyéni tartomány* panel tooregister az egyéni tartomány. Sikeres hello regisztráció esetén megjelenik a portál értesítései figyelmezteti a felhasználókat arra, hogy a tárfiók sikeresen megtörtént. Ezen a ponton az egyéni tartomány ellenőrzése után az Azure-ban, de forgalom tooyour tartomány még nem továbbítása tooyour tárfiók.
1. Térjen vissza a tooyour DNS-szolgáltatónál webhelyet, és hozzon létre egy másik olyan CNAME rekordot, amely leképezhető a altartomány tooyour Blob-szolgáltatásvégpont. Például adja meg, mint hello altartomány **www** vagy **fényképek** (hello nélkül *asverify*), és az állomásnév szerint hello  **mystorageaccount.BLOB.Core.Windows.NET** (ahol **mystorageaccount** hello a tárfiók neve). Az ebben a lépésben az egyéni tartomány hello regisztrálása sikeresen befejeződött.
1. Végül, törölheti a hello CNAME rekordot tartalmazó hello létrehozott **asverify** altartomány, mert csak egy közvetítő lépéseként szükséges volt.

Miután az új CNAME-rekord propagálása DNS-en keresztül, a felhasználók tekintheti meg Blobadatok az egyéni tartomány mindaddig, amíg hello megfelelő engedélyek lettek.

## <a name="test-your-custom-domain"></a>Az egyéni tartomány tesztelése

az egyéni tartomány tooconfirm valóban leképezve tooyour Blob-szolgáltatásvégpont, hozzon létre egy blobot a tárfiókon belül nyilvános tárolókban lévő. Ezt követően webböngészőben, használja a következő formátum tooaccess hello blob hello URI:

`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Például előfordulhat, hogy használja a következő URI tooaccess hello webes űrlap hello **myforms** hello tárolóhoz **photos.contoso.com** egyéni altartomány:

`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Az egyéni tartománynév regisztrációjának törléséhez

a Blob storage-végponthoz, az egyéni tartománynév tooderegister hello a következő eljárások egyikét használhatja.

### <a name="azure-portal"></a>Azure Portal

Hello Azure portál tooremove hello egyéni tartomány beállításban hello következőket hajthatja végre:

1. Keresse meg a storage-fiókot tooyour hello [Azure-portálon](https://portal.azure.com).
1. A **BLOB szolgáltatás** hello menü paneljén válassza **egyéni tartomány** tooopen hello *egyéni tartomány* panelen.
1. Az egyéni tartománynevet tartalmazó hello szövegmező egyértelmű hello tartalmát.
1. Jelölje be hello **mentése** gombra.

Amikor hello egyéni tartomány sikeresen el lett távolítva, látni fogja a portál értesítései figyelmezteti a felhasználókat arra, hogy a tárfiók sikeresen megtörtént.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Használjon hello [az storage-fiók mentése](https://docs.microsoft.com/cli/azure/storage/account#update) CLI parancsot, és adjon meg üres karakterláncot (`""`) a hello `--custom-domain` argumentum értéke tooremove egyéni tartományregisztrációs.

* A parancs formátuma:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Példa:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

Használjon hello [Set-AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) PowerShell-parancsmag, és adja meg egy üres karakterlánc (`""`) a hello `-CustomDomainName` argumentum értéke tooremove egyéni tartományregisztrációs.

* A parancs formátuma:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Példa:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Következő lépések
* [Egy egyéni tartomány tooan Azure Content Delivery Network (CDN) végpontjának leképezése](../cdn/cdn-map-content-to-custom-domain.md)
* [Hello Azure CDN tooaccess blobs használata az egyéni tartomány HTTPS-KAPCSOLATON keresztül](./storage-https-custom-domain-cdn.md)
