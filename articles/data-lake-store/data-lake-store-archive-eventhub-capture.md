---
title: az Event Hubs az Azure Data Lake Store aaaCapture adatok |} Microsoft Docs
description: "Használja az Azure Data Lake Store toocapture Eseményközpontokból származó adatokat"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 09b17bd0b47043bd2c83dba72c01a8064f206a0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-store-toocapture-data-from-event-hubs"></a>Használja az Azure Data Lake Store toocapture Eseményközpontokból származó adatokat

Ismerje meg, hogyan toouse Azure Data Lake Store toocapture adatokat az Azure Event Hubs fogadja.

## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md).

*  **Az Event Hubs névtér**. Útmutatásért lásd: [az Event Hubs-névtér létrehozása](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Gondoskodjon arról, hogy hello Data Lake Store-fiók és hello Event Hubs névtér hello azonos Azure-előfizetés.


## <a name="assign-permissions-tooevent-hubs"></a>Engedélyek hozzárendelése tooEvent hubok

Ebben a szakaszban hozzon létre egy mappát hello fiókon belül, ahol azt szeretné, hogy toocapture hello Eseményközpontokból származó adatokat. Akkor is engedélyeket tooEvent hubok, hogy adatokat írhat be egy Data Lake Store-fiókba. 

1. Ha szeretné, hogy toocapture Eseményközpontokból származó adatokat, és kattintson a hello Data Lake Store-fiók megnyitása **adatkezelő**.

    ![Data Lake Store adatkezelő](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Store adatkezelő")

2.  Kattintson a **új mappa** és írjon be egy nevet a mappára, ahol toocapture hello adatokat.

    ![Hozzon létre egy új mappát a Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "hozzon létre egy új mappát a Data Lake Store-ban")

3. A Data Lake Store hello hello gyökerében engedélyeket rendelhet. 

    a. Kattintson a **adatkezelő**, jelölje ki a Data Lake Store-fiók hello hello gyökérkönyvtárában, és kattintson **hozzáférés**.

    ![Engedélyeket rendelhet a Data Lake Store legfelső szintű](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "engedélyeket rendelhet a Data Lake Store gyökér")

    b. A **hozzáférés**, kattintson a **Hozzáadás**, kattintson a **felhasználó vagy csoport kiválasztása**, majd keresse meg a `Microsoft.EventHubs`. 

    ![Engedélyeket rendelhet a Data Lake Store legfelső szintű](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "engedélyeket rendelhet a Data Lake Store gyökér")
    
    Kattintson a **Kiválasztás** gombra.

    c. A **engedélyek hozzárendelése**, kattintson a **Select engedélyeket**. Állítsa be **engedélyek** túl**Execute**. Állítsa be **hozzáadása** túl**ezt a mappát, és minden gyermeknek**. Állítsa be **hozzáadni** túl**egy hozzáférési engedélybejegyzés és egy alapértelmezett engedélybejegyzés**.

    ![Engedélyeket rendelhet a Data Lake Store legfelső szintű](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "engedélyeket rendelhet a Data Lake Store gyökér")

    Kattintson az **OK** gombra.

4. Engedélyeket rendelhet a hello célmappáját Data Lake Store-fiókjában toocapture adatokat.

    a. Kattintson a **adatkezelő**, jelölje ki hello mappa hello Data Lake Store-fiókba, és kattintson a **hozzáférés**.

    ![Engedélyeket rendelhet a Data Lake Store-mappa](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "engedélyeket rendelhet a Data Lake Store-mappa")

    b. A **hozzáférés**, kattintson a **Hozzáadás**, kattintson a **felhasználó vagy csoport kiválasztása**, majd keresse meg a `Microsoft.EventHubs`. 

    ![Engedélyeket rendelhet a Data Lake Store-mappa](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "engedélyeket rendelhet a Data Lake Store-mappa")
    
    Kattintson a **Kiválasztás** gombra.

    c. A **engedélyek hozzárendelése**, kattintson a **Select engedélyeket**. Állítsa be **engedélyek** túl**Olvasás, írás,** és **Execute**. Állítsa be **hozzáadása** túl**ezt a mappát, és minden gyermeknek**. Végezetül be **hozzáadni** túl**egy hozzáférési engedélybejegyzés és egy alapértelmezett engedélybejegyzés**.

    ![Engedélyeket rendelhet a Data Lake Store-mappa](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "engedélyeket rendelhet a Data Lake Store-mappa")
    
    Kattintson az **OK** gombra. 

## <a name="configure-event-hubs-toocapture-data-toodata-lake-store"></a>Az Event Hubs toocapture tooData Lake adattárban konfigurálása

Ebben a szakaszban egy Eseményközpontot, az Event Hubs névtéren belül hoz létre. Konfigurálnia hello Eseményközpont toocapture adatok tooan Azure Data Lake Store-fiók. Ez a szakasz azt feltételezi, hogy már létrehozta az Event Hubs névtér.

2. A hello **áttekintése** hello Event Hubs névtér, panelén kattintson **+ Eseményközpont**.

    ![Eseményközpont létrehozása](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Eseményközpont létrehozása")

3. Adja meg a következő hello értékek tooconfigure Event Hubs toocapture tooData Lake adattárban.

    ![Eseményközpont létrehozása](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Eseményközpont létrehozása")

    a. Adja meg a hello Eseményközpont nevét.
    
    b. Ebben az oktatóanyagban beállítása **Partíciószámnak** és **üzenet megőrzési** toohello alapértelmezett értékeket.
    
    c. Állítsa be **rögzítése** túl**a**. Set hello **időkerete** (milyen gyakran toocapture) és **méret ablakban** (adatok mérete toocapture). 
    
    d. A **rögzítése szolgáltató**, jelölje be **Azure Data Lake Store** és hello válasszon hello Data Lake Store a korábban létrehozott. A **elérési utat a Data Lake**, adja meg a Data Lake Store-fiók hello létrehozott hello mappa hello neve. Csak akkor kell tooprovide hello relatív elérési út toohello mappát.

    e. Hagyja hello **minta rögzítési neve formátumok** toohello alapértelmezett értéket. Ez a beállítás szabályozza hello mappaszerkezet, amely hello rögzítési mappában jön létre.

    f. Kattintson a **Create** (Létrehozás) gombra.

## <a name="test-hello-setup"></a>Teszt hello beállítása

Hello megoldás küldött adatok toohello Azure Event Hubs most tesztelheti. Hajtsa végre a hello található utasítások segítségével: [tooAzure Event Hubs üzenetküldési](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Hello adatküldés elindítását követően adataira hello megjelennek a Data Lake Store megadott hello mappaszerkezet használatával. Például láthatja a mappastruktúrát, ahogy az a következő képernyőkép a Data Lake Store-ban hello.

![Az EventHub-adatokat a Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "minta az EventHub-adatok Data Lake Store-ban")

> [!NOTE]
> Akkor is, ha még nem rendelkezik az Event Hubsban érkező üzenetek, az Event Hubs hello Data Lake Store-fiók adatbázisba írja az üres fájlok csak hello fejlécekkel. hello fájlok írt: hello azonos hello Event Hubs létrehozásakor megadott időintervallumban.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>A Data Lake Store adatok elemzése

Ha hello adatok a Data Lake Store, futtathatja analitikai feladatok tooprocess és crunch hello adatokat. Lásd: [USQL Avro példa](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) hogyan toodo az Azure Data Lake Analytics használatával.
  

## <a name="see-also"></a>Lásd még:
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Adatok másolása az Azure Storage Blobs tooData Lake Store-ból](data-lake-store-copy-data-azure-storage-blob.md)
