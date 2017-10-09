---
title: "a Batch-fiók hello Azure-portálon aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate az Azure Batch fiókként hello Azure portál toorun nagyméretű párhuzamos munkaterhelések hello felhőben"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2176f88ba0a1a3298023de8f520d46ef28a664b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-portal"></a>Az Azure-portálon hello Batch-fiók létrehozása

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
>
>

Megtudhatja, hogyan toocreate az Azure Batch hello fiókként [Azure-portálon][azure_portal], és válassza ki, amelyek felelnek meg a számítási forgatókönyv hello fiók tulajdonságait. Ismerje meg, ahol toofind fontos fiók tulajdonságait, például tárelérési kulcsok és a fiók URL-címek.

Batch-fiókok és forgatókönyvek kapcsolatos háttér, lásd: hello [szolgáltatás áttekintése](batch-api-basics.md).



## <a name="create-a-batch-account"></a>Batch-fiók létrehozása

Hello portál toocreate Batch-fiók használata egy hello két *tárolókészlet foglalási mód*: **a Batch szolgáltatás** módban vagy újabb hello **felhasználói előfizetési** módját, amely több helyre van szüksége konfiguráció. E két mód kapcsolatos információkért lásd: hello [szolgáltatás áttekintése](batch-api-basics.md#account). A hello felhasználói előfizetési mód, lásd is hello [blogbejegyzés](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).

## <a name="batch-service-mode"></a>Batch szolgáltatás mód



1. Jelentkezzen be toohello [Azure-portálon][azure_portal].
2. Kattintson az **Új** > **Számítás** > **Batch-szolgáltatás** elemre.

    ![Köteg hello piactér][marketplace_portal]
3. Hello **új Batch-fiók** panel jelenik meg. Lásd: hello leírását alább minden panel elemet.

    ![Batch-fiók létrehozása][account_portal]

    a. **Fióknév**: hello Batch-fiók neve mellett dönt hello Azure-régió, ahol hello-fiók létrehozása belül egyedinek kell lennie (lásd: **hely** alatt). hello fiók neve csak kisbetűket és számokat tartalmazhat, és 3 – 24 karakter hosszúságúnak kell lennie.

    b. **Előfizetés**: hello mely toocreate hello Batch-fiókhoz az előfizetéshez. Ha csak egy előfizetéssel rendelkezik, ez alapértelmezés szerint be van jelölve.

    c. **Készletfelosztási mód**: Válassza a **Batch szolgáltatás** lehetőséget.

    c. **Erőforráscsoport**: Kiválaszthat egy meglévő erőforráscsoportot az új Batch-fiókhoz, vagy újat is létrehozhat.

    d. **Hely**: hello Azure-régiót mely toocreate hello Batch-fiókhoz. Beállítások, csak az előfizetés és az erőforráscsoport által támogatott hello régiók jelennek meg.

    e. **Tárfiók** (nem kötelező): Olyan általános célú Azure Storage-fiók, amelyet a Batch-fiókhoz társít. A legtöbb Batch-fiókhoz ajánlott a használata. További részletekért tekintse meg a lentebb található [Társított Azure Storage-fiók](#linked-azure-storage-account) című szakaszt.

4. Kattintson a **létrehozása** toocreate hello fiók.

   hello portál jelzi, hogy központi telepítése folyamatban van. A befejezéskor **Az üzembe helyezések sikerültek** értesítés jelenik meg az **Értesítések** területen.

## <a name="user-subscription-mode"></a>Felhasználói előfizetés mód

### <a name="allow-azure-batch-tooaccess-hello-subscription-one-time-operation"></a>Engedélyezi az Azure Batch tooaccess hello előfizetés (egyszeri művelet)
Az első Batch-fiók felhasználói előfizetési módban létrehozásakor hajtsa végre a következő lépéseket tooregister hello kötegelt előfizetés. (Ha korábban már elvégezte ezt, hagyja ki toohello a következő szakaszt.)

1. Jelentkezzen be toohello [Azure-portálon][azure_portal].

2. Kattintson a **több szolgáltatások** > **előfizetések**, és hello az előfizetés Batch-fiók hello toouse használni szeretne.

3. A hello **előfizetés** panelen kattintson a **hozzáférés-vezérlés (IAM)** > **Hozzáadás**.

    ![Az előfizetéshez való hozzáférés vezérlése][subscription_access]

4. A hello **engedélyek hozzáadása** panelen, jelölje be hello **közreműködő** szerepkör, keressen a hello kötegelt API. Ezek a karakterláncok, amíg meg nem látja hello API mindegyikének keresése:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Újabb Azure AD-bérlők ezt a nevet használhatják.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** hello azonosítója hello kötegelt API. 

5. Miután megtalálta hello kötegelt API-t, jelölje ki, majd kattintson a **mentése**.

    ![Batch-engedélyek hozzáadása][add_permission]

### <a name="create-a-key-vault"></a>Kulcstartó létrehozása
Felhasználói előfizetési módban az az Azure key vault kell toothe tartozik, amely ugyanabban az erőforráscsoportban, hello Batch-fiók toobe létrehozott. Ellenőrizze, hogy a terület, ahol kötegelt hello erőforráscsoport van [elérhető](https://azure.microsoft.com/regions/services/) , és amely az előfizetés támogatja.

1. A hello [Azure-portálon][azure_portal], kattintson a **új** > **biztonság + identitás szakaszában** > **Key Vault** .

2. A hello **kulcstároló létrehozása** panelen adjon meg egy nevet hello kulcstároló, és hozzon létre egy erőforráscsoportot a Batch-fiókhoz használni kívánt hello régióban. Hagyja hello fennmaradó alapértelmezés szerinti beállításokat, majd kattintson az **létrehozása**.

### <a name="create-a-batch-account"></a>Batch-fiók létrehozása

1. A hello [Azure-portálon][azure_portal], kattintson a **új** > **számítási** > **Batch szolgáltatás**.

    ![Köteg hello piactér][marketplace_portal]
3. Hello **új Batch-fiók** panel jelenik meg. Lásd: hello leírását alább minden panel elemet.

    ![Batch-fiók létrehozása][account_portal_byos]

    a. **Fióknév**: hello Batch-fiók neve mellett dönt hello Azure-régió, ahol hello-fiók létrehozása belül egyedinek kell lennie (lásd: **hely** alatt). hello fiók neve csak kisbetűket és számokat tartalmazhat, és 3 – 24 karakter hosszúságúnak kell lennie.

    b. **Előfizetés**: Ha több előfizetéssel rendelkezik, válassza ki a Batch szolgáltatás hello regisztrált hello-előfizetést.

    c. **Készletfeldolgozási mód**: Válassza a **Felhasználói előfizetés** módot.

    d. **Kulcstároló**: a Batch-fiókhoz hello előző szakaszban létrehozott válassza hello kulcstároló. Szükség esetén hozzon létre egy új kulcstartót. Miután kijelölte a hello tárolóban, jelölje ki a hello jelölőnégyzet toogrant Azure Batch hozzáférés toohello kulcstároló.

    c. **Erőforráscsoport**: hello kulcstároló létrehozására használt válassza hello erőforráscsoportot.

    d. **Hely**: hello hello kulcstároló hello Batch-fiók létrehozására használt Azure-régiót.

    e. **Tárfiók** (nem kötelező): Olyan általános célú Azure Storage-fiók, amelyet a Batch-fiókhoz társít. A legtöbb Batch-fiókhoz ajánlott a használata. További részletekért tekintse meg az alábbi, [Társított Azure Storage-fiók](#linked-azure-storage-account) című szakaszt.

4. Kattintson a **létrehozása** toocreate hello fiók.

   hello portál jelzi, hogy központi telepítése folyamatban van. A befejezéskor **Az üzembe helyezések sikerültek** értesítés jelenik meg az **Értesítések** területen.



## <a name="view-batch-account-properties"></a>Batch-fiók tulajdonságainak megtekintése
Hello fiók létrehozása után megnyithatja hello **Batch-fiók panelen** tooaccess a beállítások és tulajdonságait. A bal oldali menüjében hello hello Batch-fiók panelen elérheti azokat fiókbeállítások és tulajdonságok.

![A Batch-fiók panel az Azure Portalon][account_blade]

* **A Batch-fiók URL-címe**: Ha a hello alkalmazást fejleszt [kötegelt API-k](batch-apis-tools.md#azure-accounts-for-batch-development), szüksége lesz egy fiók URL-cím tooaccess a kötegelt erőforrásokhoz. A Batch-fiók URL-címe van hello a következő formátumban:

    `https://<account_name>.<region>.batch.azure.com`

![A Batch-fiók URL-címe a portálon][account_url]

* **Hívóbetűk** (a Batch szolgáltatás mód): tooauthenticate hozzáférés tooyour Batch-fiókhoz az alkalmazásról, szüksége lesz egy fiók hozzáférési kulcsot. (Ez a beállítás nem áll rendelkezésre felhasználói előfizetés módban, amelyben Azure Active Directory-hitelesítést használ.)

    tooview vagy újragenerálása a Batch-fiók hozzáférési kulcsait, adja meg `keys` hello bal oldali menüben **keresési** hello Batch-fiók panelen lévő mezőbe, majd válasszon **kulcsok**.

    ![A Batch-fiók kulcsai az Azure Portalon][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Társított Azure Storage-fiók

Opcionálisan hozzákapcsolhatja egy általános célú Azure Storage-fiók tooyour Batch-fiókhoz. Hello [alkalmazáscsomagok](batch-application-packages.md) a Batch szolgáltatás használja Azure Blob Storage tárolóban hello [kötegelt fájl egyezmények .NET](batch-task-output.md) könyvtár. Ilyen választható szolgáltatások segítséget nyújt a rendszerbe állítása a kötegelt feladatok futtatása hello alkalmazásokat és általuk előállított hello adatait.

Érdemes létrehozni egy új Storage-fiókot kifejezetten a Batch-fiók általi használatra.

![Általános célú tárfiók létrehozása][storage_account]

> [!NOTE]
> Az Azure Batch jelenleg csak hello általános célú Tárfiók típusa. Erről a fióktípusról a [Tudnivalók az Azure Storage-fiókokról](../storage/common/storage-create-storage-account.md) oldal 5. lépésében [Create a storage account] (../storage/common/storage-create-storage-account.md#create-a-storage-account) talál leírást.
>
>

> [!WARNING]
> Legyen körültekintő, amikor egy kapcsolódó tárfiók hello tárelérési kulcsok újragenerálása. Csak egy Tárfiók kulcsának újragenerálása, és kattintson a **szinkronizálási kulcsok** hello a kapcsolódó Storage-fiók panelen. Várjon, amíg tooallow hello kulcsok toopropagate toohello számítási csomópontjain a készletek, majd újbóli létrehozása és szinkronizálása öt percen belül hello más kulcsot, ha szükséges. Ha mindkét újragenerálja a kulcsok: hello azonos időben, a számítási csomópontok nem lesz képes toosynchronize mindkét kulcsot, és elvesztik a hozzáférést toohello tárfiók.
>
>

![A tárfiók hozzáférési kulcsainak ismételt létrehozása][4]

## <a name="batch-service-quotas-and-limits"></a>A Bach szolgáltatás kvótái és korlátozásai
Felhívjuk a vegye figyelembe, hogy, az Azure-előfizetés és más Azure-szolgáltatások, bizonyos [kvótái és korlátai](batch-quota-limit.md) tooBatch alkalmazni. Batch-fiók aktuális kvótái hello fiók hello portálon jelennek meg **tulajdonságok**.

![Batch-fiókra vonatkozó kvóták az Azure Portalon][quotas]



Ezenkívül ezek mely százalékértékénél kéri számos növelhető egyszerűen az elküldött hello Azure-portálon a termék ingyenes támogatási kérelmet. Lásd: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md) talál részletes információt kérő kvóta növekszik.

## <a name="other-batch-account-management-options"></a>Egyéb Batch-fiókkezelési lehetőségek
Ezenkívül toousing hello Azure-portálon, is létrehozása és kezelése a Batch-fiókok hello alábbira:

* [Batch – PowerShell-parancsmagok](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Következő lépések
* Lásd: hello [Batch funkcióinak áttekintése](batch-api-basics.md) toolearn további kötegelt szolgáltatással kapcsolatos fogalmak és szolgáltatásokkal kapcsolatban. hello cikk ismerteti hello elsődleges kötegelt erőforrások, például a készletek, a számítási csomópontokat, a feladatok és a feladatokat, és hello áttekintése, amelyek lehetővé teszik a nagyméretű számítási munkaterhelés végrehajtási szolgáltatás szolgáltatásokat.
* A Batch-kompatibilis alkalmazások hello használata fejlődő hello alapvető [Batch .NET ügyféloldali kódtár](batch-dotnet-get-started.md) vagy [Python](batch-python-tutorial.md). A bevezető cikkeiben végigvezeti egy működő alkalmazást, amely hello Batch szolgáltatás tooexecute a munkaterhelés használ több számítási csomóponton, és a munkaterhelés fájl átmeneti és lekérése az Azure Storage használatát magában foglalja.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "A tárfiók hozzáférési kulcsainak ismételt létrehozása"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
