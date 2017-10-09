---
title: "aaaUse Windows ügyfél képek az Azure-ban |} Microsoft Docs"
description: "Hogyan toouse Visual Studio-előfizetéssel előnyökkel jár a toodeploy Windows 7, Windows 8 vagy Windows 10 Azure-ban fejlesztési/Tesztelési forgatókönyvek"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 91c3880a-cede-44f1-ae25-f8f9f5b6eaa4
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: 4ac7c3d9872673030f4ea0f0ab38625dd9d9c1b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Windows-ügyfél használata az Azure-ban fejlesztési/Tesztelési forgatókönyvek
Használhatja a Windows 7, Windows 8 vagy Windows 10 fejlesztési és tesztelési célú forgatókönyvek az Azure-ban biztosított megfelelő (korábbi nevén MSDN) Visual Studio-előfizetéssel rendelkezik. Ez a cikk ismerteti a hello való futtatásának követelményei Windows ügyfél Azure-ban és hello használata Azure-katalógus képek.

## <a name="subscription-eligibility"></a>Előfizetés jogosultság
Aktív (személyek szerezték be egy Visual Studio előfizetői licenccel) Visual Studio-előfizetők fejlesztési és tesztelési célra használhatja Windows ügyfél. Windows-ügyfél hardver- és a saját Azure-előfizetés típusú futó Azure virtuális gépek is használhatók. Windows-ügyfél nem lehet a telepített tooor használható Azure normális üzemi használatra, vagy azok, akik nem aktív Visual Studio-előfizetők által használt.

Az Ön kényelme érdekében a Microsoft bizonyos Windows 10-lemezképek elérhetővé tett belül hello Azure gyűjteményből [jogosult fejlesztési és tesztelési célú kínál](#eligible-offers). A Visual Studio-előfizetők ajánlat bármilyen típusú belül is [megfelelően készítse elő és hozzon létre](prepare-for-upload-vhd-image.md) egy 64 bites Windows 7, Windows 8 vagy Windows 10-lemezképet, majd [tooAzure feltöltése](upload-generalized-managed.md). hello használata korlátozott toodev/test aktív Visual Studio-előfizetők által marad.

## <a name="eligible-offers"></a>Jogosult ajánlatok
a következő táblázat részleteket hello hello azonosítóját, amely jogosult a Windows 10 keresztül hello Azure-katalógus toodeploy kínálnak. Windows 10 hello lemezkép csak a következő ajánlatok látható toohello. A Visual Studio-előfizetők számára szükséges toorun Windows ügyfél egy másik ajánlattípus kell túl[megfelelően készítse elő és hozzon létre](prepare-for-upload-vhd-image.md) egy 64 bites Windows 7, Windows 8 vagy Windows 10 lemezképet és [majd feltölteni tooAzure](upload-generalized-managed.md).

| Csomag neve | Csomag száma | Rendelkezésre álló ügyfélkezelési lemezképek |
|:--- |:---:|:---:|
| [Fejlesztés/tesztelés – használatalapú fizetés](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [A Visual Studio Enterprise (MPN) előfizetők](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [A Visual Studio Professional előfizetők](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [A Visual Studio Test Professional előfizetők](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium MSDN (juttatás)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [A Visual Studio vállalati előfizetők](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [A Visual Studio Enterprise (BizSpark) előfizetői](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Vállalati fejlesztési és tesztelési célú](https://azure.microsoft.com/ofers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Ellenőrizze az Azure-előfizetéshez
Ha nem ismeri a ajánlat Azonosítóját, beszerezhet keresztül hello Azure portálra az alábbi két módszer egyikével:  

- Az "Előfizetés" panelen hello:

  ![Az ajánlat azonosító részleteinek hello Azure-portálon](./media/client-images/offer-id-azure-portal.png) 

- Vagy kattintson a **számlázási** , majd az előfizetés-azonosító. hello ajánlat azonosító hello számlázási panel jelenik meg.

Megtekintheti hello ajánlat azonosító a hello ["Előfizetések" lapon](http://account.windowsazure.com/Subscriptions) hello Azure fiókportál:

![Ajánlat részletei hello Azure-fiók portálon](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Következő lépések
Most már telepítheti a virtuális gépek [PowerShell](quick-create-powershell.md), [Resource Manager-sablonok](ps-template.md), vagy [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

