---
title: "egy függvény hello Azure portál alkalmazásából aaaCreate |} Microsoft Docs"
description: "Létrehoz egy új funkció alkalmazást az Azure App Service hello portálról."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: c531fc71c798edf22e25a5f4b79c15413809dc86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-from-hello-azure-portal"></a>Az Azure-portálon hello függvény-alkalmazás létrehozása

Az Azure függvény Apps hello Azure App Service-infrastruktúrát használja. Ez a témakör bemutatja, hogyan toocreate egy függvény alkalmazás hello Azure-portálon. Egy függvény app egyéni függvényei hello végrehajtásának hello tárolót. Amikor létrehoz egy függvény alkalmazást az App Service-csomag üzemeltetési hello, a függvény app funkciók is használhatók összes hello az App Service.

## <a name="create-a-function-app"></a>Függvényalkalmazás létrehozása

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Ha létrehoz egy függvény alkalmazást, adja meg egy érvényes **alkalmazásnév**, amely csak betűket, számokat és kötőjeleket tartalmazhat. Az aláhúzás (**_**) nem engedélyezett karakter.

A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat. A tárfiók nevének egyedinek kell lennie az Azure rendszerben. 

Hello függvény alkalmazás létrehozása után létrehozhat egyéni függvényei egy vagy több különböző nyelveken. Hozzon létre funkciók [hello portál használatával](functions-create-first-azure-function.md#create-function), [folyamatos üzembe helyezés](functions-continuous-deployment.md), vagy a [FTP-vel feltöltése](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Service-csomagok

Az Azure Functions van két különböző service-csomagokról: fogyasztás terv és az App Service-csomag. hello fogyasztás terv automatikusan lefoglalja számára a számítási teljesítményt, ha a kódja fut., és méretezik kibővített szükséges toohandle terhelés, majd méretezik-a kód nem futtatásakor. App Service-csomag hello biztosít a függvény app hozzáférés tooall hello létesítményekben az App Service. Ki kell választania a service-csomag, a függvény alkalmazás létrehozása, és azt jelenleg nem módosul. További információkért lásd: [válassza ki az Azure Functions üzemeltetési terv](functions-scale.md).

Ha azt tervezi, toorun JavaScript-funkcióként az App Service-csomagot, válasszon egy tervet a kevesebb magok. További információkért lásd: hello [függvények JavaScript hivatkozás](functions-reference-node.md#choose-single-core-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Tárolási fiókra vonatkozó követelmények

Egy függvény alkalmazást az App Service létrehozásakor létre kell hoznia vagy tooa általános célú Azure Storage-fiók, amely támogatja a Blob, Queue és Table storage hivatkozásra. Belsőleg funkciók tárolást használ műveletek, például eseményindítók kezelése és naplózási funkciót végrehajtások. Néhány tárfiókok nem támogatják az üzenetsorok és táblák, például csak a blob storage-fiókok, a prémium szintű Azure Storage és a ZRS replikáció általános célú tárfiókok esetében. Ezek a fiókok kiszűri kívüli hello Storage-fiók panelen egy függvény alkalmazás létrehozásakor.

>[!NOTE]
>Hello fogyasztás üzemeltetési terv használata esetén a függvény kód és a kötés a konfigurációs fájlok Azure File storage hello fő tárfiókban lévő vannak tárolva. Hello fő storage-fiók törlésekor a tartalom törlődik, és nem állítható helyre.

toolearn tárfióktípusokat, kapcsolatos további információkért lásd: [hello Azure Storage szolgáltatásainak bemutatása](../storage/common/storage-introduction.md#introducing-the-azure-storage-services). 

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



