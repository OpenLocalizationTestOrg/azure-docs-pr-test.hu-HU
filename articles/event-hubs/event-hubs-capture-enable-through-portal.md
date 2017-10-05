---
title: "Az Azure Event Hubs Capture engedélyezése a portálon keresztül | Microsoft Docs"
description: "Engedélyezze az Event Hubs Capture funkciót az Azure Portal használatával."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 0cbf45dce922647f2996f929d2c7cf885a2f4db4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="enable-event-hubs-capture-using-the-azure-portal"></a>Az Event Hubs Capture engedélyezése az Azure Portal használatával

A Capture-t konfigurálhatja az eseményközpont létrehozásakor az [Azure-portálon](https://portal.azure.com). Az adatokat Azure [Blob Storage](https://azure.microsoft.com/services/storage/blobs/)-tárolóban vagy [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/)-fiókban is rögzítheti.

## <a name="capture-data-to-an-azure-storage-account"></a>Adatok rögzítése Azure Storage-fiókba  

Amikor létrehoz egy eseményközpontot, rögzítési kattintva engedélyezheti a **a** gombra a **Eseményközpont létrehozása** portál panel. Ezután a **Capture-szolgáltató** mező **Azure Storage** gombjára kattintva adhatja meg a Storage-fiókot és -tárolót. Mivel az Event Hubs Capture szolgáltatások közötti hitelesítést használ a tárolóval, nem kell megadnia egy tárfiók kapcsolati sztringjét. Az erőforrás-választó automatikusan kiválasztja az erőforrás URI-azonosítóját a tárfiókhoz. Az Azure Resource Manager használatakor explicit módon meg kell adnia ezt az URI-t karakterláncként.

Az időkeret alapértelmezett értéke 5 perc. A minimális értéke 1, a maximális 15. A **Méret** ablak 10–500 MB tartománnyal rendelkezik.

![][1]

## <a name="capture-data-to-an-azure-data-lake-store-account"></a>Adatok rögzítése egy Azure Data Lake Store-fiókba

Az adatok Azure Data Lake Store-ban történő rögzítéséhez létre kell hoznia egy Data Lake Store-fiókot és egy eseményközpontot:

### <a name="create-an-azure-data-lake-store-account-and-folders"></a>Azure Data Lake Store-fiók és -mappák létrehozása

1. A Data Lake Store-fiók létrehozásához kövesse [Az Azure Data Lake Store használatának első lépései az Azure Portal használatával](../data-lake-store/data-lake-store-get-started-portal.md) című témakör utasításait. 
2. Hozzon létre egy mappát ebben a fiókban a [Mappák létrehozása az Azure Data Lake Store-fiókban](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) szakasz útmutatásai szerint.
3. A Data Lake Store-fiók panelen kattintson a **adatkezelő**.
4. Kattintson a **Hozzáférés** elemre.
5. Kattintson az **Add** (Hozzáadás) parancsra.
6. A **Keresés név vagy e-mail-cím alapján** mezőbe írja be a **Microsoft.EventHubs** kifejezést, majd válassza ki ezt a beállítást. 
7. Megjelenik az **Engedélyek** lap. Állítsa be az engedélyeket az alábbi ábrán látható módon:

    ![][6]

8. Kattintson az **OK** gombra.
9. Most hozzon létre egy mappát a gyökérmappában. Ehhez navigáljon a célmappához, és kattintson a mappa nevére.
10. Kattintson a **Hozzáférés** elemre.
11. Kattintson az **Add** (Hozzáadás) parancsra.
12. A **Keresés név vagy e-mail-cím alapján** mezőbe írja be a **Microsoft.EventHubs** kifejezést, majd válassza ki ezt a beállítást.
13. Ekkor újra megjelenik az **Engedélyek** lap. Állítsa be az engedélyeket az alábbi ábrán látható módon:

    ![][5]

### <a name="create-an-event-hub"></a>Eseményközpont létrehozása

1. Vegye figyelembe, hogy az eseményközpontnak ugyanabban az Azure-előfizetésben kell lennie, mint amelyben az imént létrehozott Azure Data Lake Store van. Az event hubs gombra kattintva hozzon létre a **a** gombra kattint, a **rögzítése** a a **Eseményközpont létrehozása** portál panel. 
2. Az a **Eseményközpont létrehozása** portál panel, jelölje be **Azure Data Lake Store** a a **rögzítése szolgáltató** mezőbe.
3. A **Data Lake Store kiválasztása** mezőben adja meg a korábban létrehozott Data Lake Store-fiókot, a **Data Lake elérési útja** mezőben pedig a létrehozott adatmappa elérési útját.

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>A Capture hozzáadása vagy konfigurálása egy meglévő eseményközponton

A Capture-t olyan meglévő eseményközpontokon konfigurálhatja, amelyek az Event Hubs-névterekben találhatók. A Capture meglévő eseményközpontban történő engedélyezéséhez, vagy a Capture beállításainak módosításához kattintson a névtérre az **Alapvető szolgáltatások** panel betöltéséhez, majd kattintson arra az eseményközpontra, amelyet engedélyezni kíván, vagy amelyhez módosítani szeretné a Capture-beállítást. Végül kattintson a **tulajdonságok** szakasz nyissa meg a penge és szerkessze a rögzítési beállítások, a következő ábrákon látható módon:

### <a name="azure-blob-storage"></a>Azure Blob Storage

![][2]

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
[5]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture5.png
[6]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture6.png

## <a name="next-steps"></a>Következő lépések

Az Azure Resource Manager-sablonok használatával is konfigurálhatja az Event Hubs Capture-t. További információkért lásd: [Rögzítés funkció engedélyezése az Azure Resource Manager-sablonjának használatával](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).
