---
title: "Gyakori kérdések az Azure Data Lake Store aaaFrequently |} Microsoft Docs"
description: "Útmutató az Azure Data Lake Store hibaelhárításához és a hibák csökkentéséhez"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf7fd555-3e30-43ce-b28c-c3ad0a241fdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: eeabdeef18f3b72901bd1a14283f85dd9c0ead44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-azure-data-lake-store"></a>Gyakori kérdések az Azure Data Lake Store-ról
Ebben a cikkben megtudhatja, hogyan – gyakori kérdések kapcsolódó tooAzure Data Lake Store.

## <a name="how-can-i-further-protect-my-data-from-region-wide-disasters-or-accidental-deletions"></a>Hogyan gondoskodhatok még jobban az adataim védelméről a régiószintű katasztrófák és a véletlen törlés ellen?
az Azure Data Lake Store-fiókban hello adatok rugalmas tootransient hardverhibák keresztül automatizált replikák régión belül. Ez biztosítja a tartósság és magas rendelkezésre állás érdekében értekezlet hello Azure Data Lake Store SLA-t. Hogyan toofurther megvédeni az adatait a ritka régió kiterjedő valamilyen okból kimaradás vagy véletlen törlések itt van néhány útmutatást.

### <a name="disaster-recovery-guidance"></a>Vészhelyreállítási útmutató
Alapvető fontosságú a minden felhasználói tooprepare saját vész-helyreállítási terv. A vész-helyreállítási terv tekintse meg a toohello toobuild alatt az Azure dokumentációja. Itt talál néhány forrásanyagot, amelyek segítenek a saját terve létrehozásában.

* [Vészhelyreállítás és magas szintű rendelkezésre állás az Azure-alkalmazásokhoz](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Műszaki útmutató az Azure rugalmasságáról](../resiliency/resiliency-technical-guidance.md)

#### <a name="best-practices"></a>Ajánlott eljárások
Azt javasoljuk, hogy egy másik régióban, a vész-helyreállítási terv igazítva gyakoriság toohello igényű másolja át a kritikus fontosságú adatok tooanother Data Lake Store-fiók. Nincsenek módszerek toocopy adatok többek között számos [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) vagy [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md). Az Azure Data Factory hasznos szolgáltatás az adatáthelyezési folyamatok rendszeres létrehozásához és üzembe helyezéséhez.

Ha egy regionális kimaradás során, majd hozzáférhet hello régióban, ahol hello adatok másolta az adatokat. Figyelheti a hello [Azure az állapotjelző irányítópulthoz](https://azure.microsoft.com/status/) toodetermine hello Azure szolgáltatás állapota között hello földgolyó méretét.

### <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Adatsérülés vagy véletlen törlés helyreállításával kapcsolatos útmutató
Az Azure Data Lake Store biztosítja az adatok rugalmasságát biztosítja az automatikus replikációval, ez azonban nem gátolja meg, hogy az alkalmazás (vagy a fejlesztők/felhasználók) az adatok sérülését vagy véletlen törlését okozzák.

#### <a name="best-practices"></a>Ajánlott eljárások
tooprevent véletlen törlés, azt javasoljuk, hogy először hello megfelelő hozzáférési házirendek beállítása a Data Lake Store-fiók.  Ez magában foglalja az alkalmazása [Azure-erőforrás zárolások](../azure-resource-manager/resource-group-lock-resources.md) le fontos erőforrásokat, valamint alkalmazása fiók és a fájl szintű hozzáférés-vezérlés használatával a rendelkezésre álló hello toolock [Data Lake Store biztonsági funkciók](data-lake-store-security-overview.md). Emellett javasoljuk, hogy rendszeresen készítsen másolatokat a kritikus adatokról az [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) vagy [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) használatával egy másik Data Lake Store-fiókban, mappában vagy Azure-előfizetésben.  Ez lehet egy adatok sérülése vagy a törlés eseményből származó használt toorecover. Az Azure Data Factory hasznos szolgáltatás az adatáthelyezési folyamatok rendszeres létrehozásához és üzembe helyezéséhez.

Engedélyezheti a szervezetek [diagnosztikai naplózás](data-lake-store-diagnostic-logs.md) az az Azure Data Lake Store-fiók toocollect adatok a fájlhozzáférés napló ellenőrzését, amely tájékoztatást ad azokról ki lehet, hogy törölték vagy a fájl frissítése.

## <a name="next-steps"></a>Következő lépések
* [Az Azure Data Lake Store használatának első lépései](data-lake-store-get-started-portal.md)
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)

