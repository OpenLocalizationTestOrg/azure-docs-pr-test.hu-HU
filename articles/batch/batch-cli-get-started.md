---
title: "aaaGet Azure CLI köteg használatába |} Microsoft Docs"
description: "Helyezze a gyors bevezetés toohello kötegelt parancsok Azure CLI-t az Azure Batch szolgáltatás erőforrások kezelése"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 14f28311ecb16c8097d0d304a4ad89de282a2e9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-azure-cli"></a>Batch-erőforrássok kezelése az Azure CLI-vel

hello Azure CLI 2.0 Azure új parancssori felületet Azure-erőforrások kezeléséhez. A szolgáltatás macOS, Linux és Windows rendszereken használható. Az Azure CLI 2.0 kezelése és felügyelete az Azure-erőforrások hello parancssorból van optimalizálva. Hello Azure CLI toomanage használhatja az Azure Batch fiókjainak és toomanage erőforrások, például a készletek, a feladatok és a feladatokat. A hello Azure parancssori felület, akkor lehet parancsprogramot futtatni a hello számos ugyanazokhoz a feladatokhoz a hajthat végre hello kötegelt API-k, az Azure-portál és a kötegelt PowerShell-parancsmagokkal.

Ez a cikk betekintést nyújt az [Azure CLI 2.0-ás verziójának](https://docs.microsoft.com/cli/azure/overview) Batch-csel történő használatába. Lásd: [Ismerkedés az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) hello CLI használata az Azure-ral áttekintését.

A Microsoft azt javasolja, hello hello 2.0-s verzióját, az Azure parancssori felület legújabb verzióját használja. A 2.0-ás verzióval kapcsolatban további információt az [Mostantól általánosan elérhető az Azure parancssori felületének 2.0-s verziója](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/) című blogbejegyzésben talál.

## <a name="set-up-hello-azure-cli"></a>Hello Azure CLI beállítása

tooinstall hello Azure CLI lépésekkel hello leírt [telepítés hello Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md).

> [!TIP]
> Azt javasoljuk, hogy a frissíteni az Azure parancssori felület telepítése gyakran tootake előnyeit szolgáltatás frissítéseket és fejlesztéseket.
> 
> 

## <a name="command-help"></a>Segítség a parancsokhoz

Jeleníthet meg minden parancs súgószöveg hello Azure CLI hozzáfűzésével `-h` toohello parancsot. Az egyéb beállításokat hagyja változatlanul. Példa:

* hello tooget súgóját `az` parancsot, írja be:`az -h`
* használja a tooget hello CLI, az összes kötegelt parancsok listája:`az batch -h`
* Adja meg a Batch-fiók létrehozásával tooget súgó:`az batch account create -h`

A kétséges, használja a hello `-h` bármely Azure CLI parancs parancssori kapcsoló tooget súgóját.

> [!NOTE]
> Korábbi verzióiban használt Azure CLI hello `azure` toopreface CLI parancsot. A 2.0-ás verzióban minden parancs az `az` előtagot használja. Lehet, hogy tooupdate a parancsfájlok toouse hello új szintaxist a 2.0-s verziójában.
>
>  

Emellett tekintse meg a toohello Azure CLI hivatkozás dokumentációjában kapcsolatos [Azure parancssori felület parancsait köteg](https://docs.microsoft.com/cli/azure/batch). 

## <a name="log-in-and-authenticate"></a>Bejelentkezés és hitelesítés

toouse hello Azure CLI-es, a toolog kell, és hitelesíteni. Két egyszerű lépéseket toofollow van:

1. **Bejelentkezés az Azure-ba.** Bejelentkezés az Azure által biztosított van tooAzure erőforrás-kezelő parancsokat, beleértve a hozzáférési [kötegelt felügyeleti szolgáltatás](batch-management-dotnet.md) parancsok.  
2. **Bejelentkezés a Batch-fiókjába.** A kötegelt biztosít való bejelentkezés tooBatch szolgáltatás parancsok eléréséhez.   

### <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Van néhány különböző módokon toolog az Azure, a részletes leírását lásd [jelentkezzen be Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):

1. [Interaktív bejelentkezés](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in). Jelentkezzen be párbeszédes formában történő futtatásakor Azure parancssori felület parancsait saját kezűleg hello parancssorból.
2. [Bejelentkezés szolgáltatásnévvel](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal). Jelentkezzen be szolgáltatásnévvel, ha szkript vagy alkalmazás használatával kíván Azure CLI-parancsokat futtatni.

Ez a cikk hello célokra megmutatjuk, hogyan toolog az Azure interaktív módon. Típus [az bejelentkezési](https://docs.microsoft.com/cli/azure/#login) hello parancssorban:

```azurecli
# Log in tooAzure and authenticate interactively.
az login
```

Hello `az login` token használható tooauthenticate, ahogy az itt látható a parancs beolvasása. Hajtsa végre a hello utasításokat tooopen egy weblap, és küldje el a hello token tooAzure:

![Jelentkezzen be tooAzure](./media/batch-cli-get-started/az-login.png)

hello példákban szereplő hello [rendszerhéj-parancsfájlok minta](#sample-shell-scripts) . szakasz is megjelenítése hogyan toostart interaktívan jelentkezik be Azure által az Azure CLI munkamenet. Ha már bejelentkezett, hívása parancsok toowork Batch-fiókok, kulcsokkal, alkalmazáscsomagok és kvóták kezelését az erőforrásokkal.  

### <a name="log-in-tooyour-batch-account"></a>Jelentkezzen be tooyour Batch-fiókhoz.

toouse hello Azure CLI toomanage kötegelt erőforrások, például-készletek, feladatok, és feladatokat, a Batch-fiók toolog igénylő, és a hitelesítéshez. a Batch szolgáltatás toohello toolog hello használata [az batch-fiók bejelentkezési](https://docs.microsoft.com/cli/azure/batch/account#login) parancsot. 

A Batch-fiók hitelesítését két módon is elvégezheti:

- **Hitelesítés az Azure Active Directory (Azure AD) használatával.** 

    Az Azure AD hitelesítő hello alapértelmezett hello Azure CLI-es használatakor, és a legtöbb esetben ajánlott. 
    
    Bejelentkezéskor tooAzure interaktív módon, hello előző szakaszban leírtak szerint, a hitelesítőadatok gyorsítótárazva lettek, így hello Azure parancssori felület is bejelentkezés tooyour e ugyanazokat a hitelesítő adatokat használ a Batch-fiókhoz. Ha jelentkezik be egy egyszerű szolgáltatás használatával tooAzure, ezeket a hitelesítő adatokat egyaránt használt toolog a tooyour Batch-fiókhoz.

    Az Azure AD előnye a szerepköralapú hozzáférés-vezérlés (RBAC) használatában rejlik. Az RBAC a felhasználó hozzáférési függ hozzájuk rendelt szerepkör helyett hello kulcsait rendelkeznek-e. Így ahelyett, hogy hozzáférési kulcsokat kellene kezelnie, elég ha a szerepköröket kezeli, a hozzáférést és a hitelesítést pedig az Azure AD-ra bízhatja.  

    Az Azure AD hitelesítő szükség, ha létrehozta az Azure Batch-fiókhoz, a tárolókészlet foglalási mód beállítása too'User előfizetés ". 

    a kötegelt tooyour toolog fiók az Azure AD segítségével, hello hívás [az batch-fiók bejelentkezési](https://docs.microsoft.com/cli/azure/batch/account#login) parancs: 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- **Megosztott kulcsos hitelesítés.**

    [Megosztott kulcsos hitelesítést](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) használ, a fiók hozzáférési kulcsok tooauthenticate Azure parancssori felület parancsait hello a Batch-szolgáltatás.

    Létrehozásakor az Azure parancssori felület parancsfájlok tooautomate hívó kötegelt parancsok, megosztott kulcsos hitelesítést, vagy az Azure AD szolgáltatás egyszerű is használhatja. Bizonyos esetekben viszont a megosztott kulcsos hitelesítés használata egyszerűbb lehet, mint létrehozni egy szolgáltatásnevet.  

    a megosztott kulcsos hitelesítést használó toolog tartalmaznak hello `--shared-key-auth` hello parancssori beállítást:

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

hello példákban szereplő hello [rendszerhéj-parancsfájlok minta](#sample-shell-scripts) szakasz megjelenítése hogyan toolog a kötegelt fiókjába a hello szolgáltatást is használja az Azure parancssori felület az Azure AD és a megosztott kulcsot.

## <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Az Azure Batch parancssori felületi sablonjainak és fájlátviteli funkciójának (előzetes verzió) használata

Kód írása nélkül hello Azure CLI toorun kötegelt feladatok-végpontok is használhatja. Sablon parancsfájlokat létrehozása készletek, feladatok és az Azure CLI hello feladatok támogatja. Is használhatja a hello Azure CLI tooupload feladat bemeneti fájlok toohello hello társított Azure Storage-fiók a Batch-fiók, és a feladat kimeneti fájlok letöltését. További információk: [Az Azure Batch parancssori felületi sablonjainak és fájlátviteli funkciójának (előzetes verzió) használata](batch-cli-templates.md).

## <a name="sample-shell-scripts"></a>Shell-szkript minták

hello mintaparancsfájlok szerepel a következő táblázat megjelenítése hello hogyan toouse Azure parancssori felület parancsai hello Batch szolgáltatás és a Batch Management szolgáltatás tooaccomplish gyakori feladatokat. A minta parancsfájlokat hello parancsok hello Azure CLI köteg elérhető számos foglalkozik. 

| Szkript | Megjegyzések |
|---|---|
| [Batch-fiók létrehozása](./scripts/batch-cli-sample-create-account.md) | Létrehoz egy Batch-fiókot és hozzárendeli egy tárfiókhoz. |
| [Alkalmazás hozzáadása](./scripts/batch-cli-sample-add-application.md) | Hozzáad egy alkalmazást és feltölti a csomagolt bináris fájlokat.|
| [Batch-készletek kezelése](./scripts/batch-cli-sample-manage-pool.md) | Bemutatja a készletek létrehozását, átméretezését és kezelését. |
| [Feladatok és tevékenységek futtatása a Batch-csel](./scripts/batch-cli-sample-run-job.md) | Bemutatja a feladatok futtatását és a tevékenységek hozzáadását. |

## <a name="json-files-for-resource-creation"></a>Erőforrás létrehozása JSON-fájlok használatával

Kötegelt erőforrásokhoz, mint a készletek és a feladatok létrehozásakor egy JSON-fájlt tartalmazó hello új erőforrás konfigurációs helyett a paraméterek átadása, parancssori kapcsolókat is megadhat. Példa:

```azurecli
az batch pool create my_batch_pool.json
```

Legtöbb kötegelt erőforrások csak parancssori kapcsolók használatával hozhat létre, míg egyes funkciók szükségesek, egy hello erőforrás részleteit tartalmazó JSON-formátumú fájlt ad meg. Például egy JSON-fájlt kell használnia, ha azt szeretné, hogy toospecify Erőforrásfájlok kezdő tevékenység.

JSON-szintaxis toosee hello szükséges toocreate erőforrás, tekintse meg a toohello [Batch REST API-referenciában] [ rest_api] dokumentációját. Minden "Add *erőforrástípus*" hello REST API-referenciában témakör JSON mintaparancsfájlok erőforrás létrehozásához. Használható a minta JSON-parancsfájlok sablonként hello Azure parancssori Felülettel rendelkező JSON-fájlok toouse. Például toosee hello JSON-szintaxis készlet létrehozását, tekintse meg a túl[tooan alkalmazáskészlet-fiók hozzáadása][rest_add_pool].

JSON-fájlra hivatkozó szkriptre példát a [Feladatok és tevékenységek futtatása a Batch-csel](./scripts/batch-cli-sample-run-job.md) című témakörben talál.

> [!NOTE]
> Erőforrás létrehozásakor megad egy JSON-fájlt, ha a rendszer figyelmen kívül hagyja az adott erőforrás hello parancssori ad meg más paraméterekkel.
> 
> 

## <a name="efficient-queries-for-batch-resources"></a>Hatékony lekérdezések Batch-erőforrásokhoz

Minden Batch erőforrástípus támogat egy `list` parancsot, amely lekérdezi a Batch-fiókját, és listázza az adott típusú erőforrásokat. Például a fiók és a hello feladatokat egy feladat hello készletek is listázhatja:

```azurecli
az batch pool list
az batch task list --job-id job001
```

Ha lekérdezést hajt végre hello Batch szolgáltatást, amely egy `list` művelet, megadhat egy OData záradék toolimit hello mennyiségű adatot adott vissza. Kiszolgálóoldali összes szűrés következik be, mert csak hello adatokat kér le áthalad hello keresztülhaladnak a hálózaton. Ezek záradékok toosave sávszélességet (és időpontot, ezért) lista műveletek végrehajtásakor.

hello következő táblázatban hello OData záradékok hello Batch szolgáltatás által támogatott.

| Záradék | Leírás |
|---|---|
| `--select-clause [select-clause]` | A tulajdonságok egy részét adja vissza minden entitás esetében. |
| `--filter-clause [filter-clause]` | Csak entitások értéket ad vissza, amelyek megfelelnek a hello megadott OData kifejezés. |
| `--expand-clause [expand-clause]` | Beolvassa a hello entitás adatokat egyetlen alapul szolgáló REST-hívást. hello bontsa ki a záradékot jelenleg csak hello támogatja `stats` tulajdonság. |

A minta parancsfájl, hogy bemutatja, hogyan toouse egy OData záradék: [futtatni egy feladatot, és a feladatok kötegelt](./scripts/batch-cli-sample-run-job.md).

Az OData záradékot tartalmazó hatékony lista lekérdezések végrehajtásáról további információkért lásd: [hello Azure Batch szolgáltatás hatékony lekérdezéséhez](batch-efficient-list-queries.md).

## <a name="troubleshooting-tips"></a>Hibaelhárítási tippek

hello alábbi tippek segíthetnek az Azure parancssori felület problémák elhárításakor:

* Használjon `-h` tooget **súgószöveg** bármely CLI parancs esetében
* Használjon `-v` és `-vv` toodisplay **részletes** kimeneti parancsot. Ha hello `-vv` jelző megtalálható, hello Azure parancssori felület megjeleníti hello tényleges REST kérelmeit és válaszait. Ezek a kapcsolók jól jönnek a teljes hibakimenet megjelenítéséhez.
* Megtekintheti **parancsot a kimeneti adatok JSON** a hello `--json` lehetőséget. Például az `az batch pool show pool001 --json` JSON-formátumban jeleníti meg a pool001 tulajdonságait. Ezután másolja, és módosítsa ezt a kimeneti toouse egy `--json-file` (lásd: [JSON-fájlok](#json-files) korábbi ebben a cikkben).
<!---Loc Comment: Please, check link [JSON files] since it's not redirecting tooany location.--->
* Hello [Batch fórum] [ batch_forum] figyelt kötegelt csoport tagjai. Ott felteheti kérdéseit, ha problémákba ütközne, vagy segítségre lenne szüksége egy adott művelethez.

## <a name="next-steps"></a>Következő lépések

* Hello Azure parancssori Felülettel kapcsolatos további információkért lásd: hello [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).
* További információt a Batch-erőforrásokkal kapcsolatban a [Az Azure Batch áttekintése fejlesztők számára](batch-api-basics.md) című cikkben talál.
* Kód írása nélkül kötegelt sablonok toocreate készletek, feladatok és feladatok használatával kapcsolatos további információkért lásd: [használata Azure Batch CLI sablonok és a File Transfer (előzetes verzió)](batch-cli-templates.md).

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
