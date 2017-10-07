---
title: "bemutatja, hogyan aaaOverview toocreate és központi telepítése egy ajánlat toohello piactér |} Microsoft Docs"
description: "Hello lépéseket szükséges toobecome egy engedélyezett Microsoft developer megértésére, létrehozása és központi telepítése egy virtuálisgép-lemezkép, sablont, adatok vagy fejlesztői szolgáltatást a hello Azure piactéren"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
ms.openlocfilehash: ac5480b98b8b1021a595db951ed9c974f74415dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-and-manage-an-offer-in-hello-azure-marketplace"></a>Közzétehet és kezelhet egy ajánlatot az hello Azure piactéren
Ez a cikk toohelp fejlesztők létrehozásához, telepítéséhez és felügyeletéhez más Azure-ügyfél és a partnerek toopurchase és a használata hello Azure piactér felsorolt megoldásuk valósul meg.

## <a name="marketplace-publishing"></a>Piactér-közzététel
Egy Azure közzétevőként terjesztése, és a innovatív megoldás vagy tooother szolgáltatásfejlesztők, ISV-k, és az informatikai szakemberek számára hello piactér. Hello piactér, keresztül érheti el az ügyfelek számára, akik tooquickly a felhőalapú alkalmazások és a mobil megoldások fejlesztése. Ha a megoldás az üzleti felhasználók célozza, érdemes lehet a tooconsider hello [AppSource](http://appsource.microsoft.com) piactéren.


## <a name="supported-types-of-solutions"></a>Támogatott típusok megoldások
hello elsőként toodo azt szeretné, mert a közzétevő toodefine milyen megoldást kínál a vállalat. hello piactér következő ajánlatok típusú hello támogatja:

|Megoldás típusa|Virtuális gép|Megoldás sablon|
|---|---|---|
|**Meghatározása**|Előre konfigurált képek a teljes telepített operációs rendszer és egy vagy több alkalmazást. Egy virtuálisgép-lemezkép hello információ szükséges toocreate biztosít, valamint hello Azure virtuális gépek szolgáltatás a virtuális gépek telepítése.|Olyan adatszerkezet, amely egy vagy több különböző Azure-szolgáltatások hivatkozhat, más eladók többek között a szolgáltatások közzé. Az Azure-előfizetők használható toodeploy egy vagy több ajánlatok egyetlen, koordinált módon.|
|**Példa**|Egy Azure közzétevőként elkészítette és ellenőrizni a virtuális gép és egy új adatbázis-szolgáltatás. Egyéb Azure-előfizetők tooprocure szeretne, és a virtuális gép üzembe helyezés a cloud service-környezetek.|Egy Azure közzétevőként a szolgáltatások már kötegelt különböző Azure-felhőszolgáltatások gyors toodeploy terheléselosztás, a fokozott biztonságot és a magas rendelkezésre állású legyen. Egyéb Azure-előfizetők is időt takaríthat meg, amely megfelel a cél hello megoldássablonban beszerzése. Nem található, be kell szereznie, telepítése és konfigurálása toomanually rendelkeznek hello ugyanazon vagy hasonló Azure-szolgáltatásokhoz.|

> [!NOTE]
> Néhány lépést a megoldások különböző típusú hello között vannak megosztva, és a megoldás megfelelő típusú különböző toohello. Ez a cikk nyújt rövid áttekintést hello lépések toocomplete szükséges bármely típusú megoldáshoz.

## <a name="publish-a-solution"></a>A megoldás közzététele
![Jelöljön ki, regisztrálása, közzététele](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>A megoldás megadjuk előtti jóváhagyásra
a virtuális gépek toopublish [megoldás](https://createopportunity.azurewebsites.net) toohello piactér, teljes hello hivatalos Microsoft Azure **megoldás jelölési űrlap**.

>[!NOTE]
> Ha egy Partner-Ügyfélreferenséhez vagy a DX Partner Manager dolgozik, kérje meg őket toonominate hello Azure hitelesített program a megoldás. Is elvégezheti a toohello [hivatalos Microsoft Azure](http://createopportunity.azurewebsites.net) weblap és kitöltése során hello kérelmet. Adja meg a Partner Ügyfélfelelőshöz vagy a DX Partner vezető hello e-mail hello **forduljon a Microsoft támogató** mezőbe.

Ha megfelel a hello jogosultsági feltételeket hello [Azure piactér részvételét házirendek](http://go.microsoft.com/fwlink/?LinkID=526833) és az alkalmazás jóváhagyva, először a megoldás toohello piactér meg tooonboard használata.

### <a name="register-your-account-as-a-microsoft-seller"></a>A fiók regisztrálásához a Microsoft értékesítő
Regisztrálja a Microsoft-fiókjával, mint egy [Microsoft Developer-fiók](marketplace-publishing-accounts-creation-registration.md).

### <a name="publish-your-solution"></a>A megoldás közzététele
a megoldás toohello piactér, toopublish kövesse az alábbi lépéseket:
1. Hello felhasználóknak követelmények teljesítéséhez.

    a. Hello teljesítéséhez [felhasználóknak Előfeltételek](marketplace-publishing-pre-requisites.md).

    b. Hello teljesítéséhez [VM műszaki Előfeltételek](marketplace-publishing-vm-image-creation-prerequisites.md).

    c. Hello teljesítéséhez [megoldás sablon műszaki Előfeltételek](marketplace-publishing-solution-template-creation-prerequisites.md).

2. Az ajánlat létrehozása.

    a. Hozzon létre egy [virtuális gép](marketplace-publishing-vm-image-creation.md) kínálnak.

    b. Hozzon létre egy [megoldássablonban](marketplace-publishing-solution-template-creation.md) kínálnak.

3. Hozzon létre az ajánlatot [tartalom marketing](marketplace-publishing-push-to-staging.md).

4. Tesztelje az ajánlatot a tesztelési.

    a. A virtuális gép ajánlatát tesztelése [átmeneti](marketplace-publishing-vm-image-test-in-staging.md).

    b. A sablon megoldás ajánlatát tesztelése [átmeneti](marketplace-publishing-solution-template-test-in-staging.md).

5. Az ajánlat toohello telepítése [piactér](marketplace-publishing-push-to-production.md).


### <a name="create-and-manage-a-virtual-machine-image"></a>Létrehozását és kezelését egy virtuálisgép-lemezkép
Hozzon létre, és Virtuálisgép-lemezkép kezelheti ezeket az erőforrásokat:
* Hozzon létre egy Virtuálisgép-lemezkép [helyszíni](marketplace-publishing-vm-image-creation-on-premise.md).
* Hozzon létre egy virtuális gépet futtató [Windows hello Azure-portálon a](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Hozzon létre egy virtuális gépet futtató [hello Azure portálon lévő Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Hibaelhárítás során tapasztalt gyakori problémákkal [virtuális merevlemez létrehozása](marketplace-publishing-vm-image-creation-troubleshooting.md).

## <a name="manage-your-solution"></a>A megoldás kezelése
Súgó a megoldást a következő erőforrások hello kezelése:
* [Olvasási hello utáni éles útmutató a virtuális gép nyújt](marketplace-publishing-vm-image-post-publishing.md)
* [Az ajánlat vagy egy SKU hello a felhasználóknak részleteinek frissítése](marketplace-publishing-vm-image-post-publishing.md#update-the-nontechnical-details-of-an-offer-or-a-sku)
* [Ajánlat vagy egy SKU technikai részleteit hello frissítése](marketplace-publishing-vm-image-post-publishing.md#update-the-technical-details-of-a-sku)
* [Adja hozzá a felsorolt ajánlat keretében egy új Termékváltozat](marketplace-publishing-vm-image-post-publishing.md#add-a-new-sku-under-a-listed-offer)
* [Hello adatlemez felsorolt SKU módosítása](marketplace-publishing-vm-image-post-publishing.md#change-the-data-disk-count-for-a-listed-sku)
* [A felsorolt ajánlat törlése hello piactér](marketplace-publishing-vm-image-post-publishing.md)
* [A felsorolt SKU törlése hello piactér](marketplace-publishing-vm-image-post-publishing.md#delete-a-listed-sku-from-the-marketplace)
* [A felsorolt SKU hello jelenlegi verziója törlése hello piactér](marketplace-publishing-vm-image-post-publishing.md#delete-the-current-version-of-a-listed-sku-from-the-marketplace)
* [Állítsa vissza a hello tooproduction árértékeket listázása](marketplace-publishing-vm-image-post-publishing.md#revert-the-listing-price-to-production-values)
* [Állítsa vissza a számlázási modell tooproduction értékek hello](marketplace-publishing-vm-image-post-publishing.md#revert-the-billing-model-to-production-values)
* [A felsorolt SKU toohello éles értékét hello láthatósági visszaállítása](marketplace-publishing-vm-image-post-publishing.md#revert-the-visibility-setting-of-a-listed-sku-to-the-production-value)
* [A Cloud Solution Provider viszonteladóhoz célzó olyan ösztönzők előnyeivel módosítása](marketplace-publishing-csp-incentive.md)
* [A kifizetés reporting ismertetése](marketplace-publishing-report-payout.md)
* [Segítségre van szüksége közzétevőként](marketplace-publishing-get-publisher-support.md)

## <a name="additional-resources"></a>További források
[Azure PowerShell beállítása](marketplace-publishing-powershell-setup.md)
