---
title: "Event Hubs rögzítése aaaAzure portálon keresztül engedélyezése |} Microsoft Docs"
description: "Hello Azure-portál használatával hello Event Hubs rögzítése funkció engedélyezéséhez."
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
ms.openlocfilehash: 27c7528552c497a4d98873a22bd56a991c66247c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-event-hubs-capture-using-hello-azure-portal"></a>Engedélyezze az Event Hubs rögzítése hello Azure-portál használatával

Létrehozáskor hello event hub hello segítségével konfigurálhatja a rögzítési [Azure-portálon](https://portal.azure.com). Akkor is, vagy rögzítési hello adatok tooan Azure [Blob-tároló](https://azure.microsoft.com/services/storage/blobs/) tároló vagy tooan [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) fiók.

## <a name="capture-data-tooan-azure-storage-account"></a>Adatok tooan Azure Storage-fiókjának rögzítése  

Amikor létrehoz egy eseményközpontot, rögzítési hello kattintva engedélyezheti **a** hello gombjára **Eseményközpont létrehozása** portál panel. Majd adja meg a Tárfiók és tároló kattintva **Azure Storage** a hello **rögzítése szolgáltató** mezőbe. Mivel az Event Hubs rögzítése használja a szolgáltatások közötti hitelesítés tárolóval, nem kell toospecify egy tárolási kapcsolati karakterlánc. hello erőforrás objektumválasztó hello erőforrás URI azonosítója a tárfiók automatikusan kijelöli. Az Azure Resource Manager használatakor explicit módon meg kell adnia ezt az URI-t karakterláncként.

időkerete hello alapértelmezett érték 5 perc. minimális hello értéke-1, maximális 15 hello. Hello **mérete** ablak tartománnyal rendelkező 10 – 500 MB.

![][1]

## <a name="capture-data-tooan-azure-data-lake-store-account"></a>Rögzítése adatok tooan Azure Data Lake Store-fiók

toocapture adatok tooAzure Data Lake Store, létrehozhat egy Data Lake Store-fiókot, és az eseményközpontok:

### <a name="create-an-azure-data-lake-store-account-and-folders"></a>Azure Data Lake Store-fiók és -mappák létrehozása

1. Hozzon létre egy Data Lake Store-fiókot, hello utasításait követve [Ismerkedés az Azure Data Lake Store használatának hello Azure-portálon](../data-lake-store/data-lake-store-get-started-portal.md). 
2. Hozzon létre egy mappát, ezzel a fiókkal, hello hello utasításait követve [mappák létrehozása az Azure Data Lake Store-fiók](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) szakasz.
3. Hello Data Lake Store-fiók panelen kattintson a **adatkezelő**.
4. Kattintson a **Hozzáférés** elemre.
5. Kattintson az **Add** (Hozzáadás) parancsra.
6. A hello **Keresés név vagy e-mail** mezőbe írja be **Microsoft.EventHubs** , és válassza ki ezt a beállítást. 
7. Hello **engedélyek** lap jelenik meg. Hello engedélyek beállítása a hello a következő ábrán látható módon:

    ![][6]

8. Kattintson az **OK** gombra.
9. Most hozzon létre egy mappát hello gyökérmappában toohello Célmappa tallózása, majd kattintson a hello mappa neve.
10. Kattintson a **Hozzáférés** elemre.
11. Kattintson az **Add** (Hozzáadás) parancsra.
12. A hello **Keresés név vagy e-mail** mezőbe írja be **Microsoft.EventHubs** , és válassza ki ezt a beállítást.
13. Hello **engedélyek** lap jelenik meg újra. Hello engedélyek beállítása a hello a következő ábrán látható módon:

    ![][5]

### <a name="create-an-event-hub"></a>Eseményközpont létrehozása

1. Vegye figyelembe, hogy hello eseményközpont kell lennie a hello hello Azure Data Lake Store imént létrehozott, azonos Azure-előfizetés. Hozzon létre hello eseményközpont, hello kattintva **a** gombra kattint, a **rögzítése** a hello **Eseményközpont létrehozása** portál panel. 
2. A hello **Eseményközpont létrehozása** portál panel, jelölje be **Azure Data Lake Store** a hello **rögzítése szolgáltató** mezőbe.
3. A **válasszon Data Lake Store**, adja meg a korábban, és hello létrehozott Data Lake Store-fiókot hello **elérési utat a Data Lake** mezőbe írja be a hello elérési toohello adatok létrehozott mappába.

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>A Capture hozzáadása vagy konfigurálása egy meglévő eseményközponton

A Capture-t olyan meglévő eseményközpontokon konfigurálhatja, amelyek az Event Hubs-névterekben találhatók. tooenable egy meglévő eseményközpont, vagy toochange rögzíti a rögzítési beállításokat, kattintson a hello névtér tooload hello **Essentials** panelen, majd kattintson a hello eseményközpont, amelynek szeretné, hogy tooenable vagy hello rögzítési beállítás módosításával. Végül kattintson a hello **tulajdonságok** hello szakasza panel megnyitásához, és hello rögzítési beállításait, majd szerkessze a hello a következő ábrán látható módon:

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
