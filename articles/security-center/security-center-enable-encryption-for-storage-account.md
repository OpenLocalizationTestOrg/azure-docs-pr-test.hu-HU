---
title: "aaaEnable titkosítási tárfiók az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslatait ** engedélyezheti a titkosítást az Azure Storage fiók **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: c5cbafbf3a8be86f213dcf1c0c0ddcc0222b3d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Engedélyezze a titkosítást az Azure storage-fiókot az Azure Security Centerben
Az Azure Security Center javasolhatja Azure Storage szolgáltatás titkosítási engedélyeznie az inaktív adatok.

Storage Service Encryption (SSE) hello adatok beolvasása előtt titkosítása hello adatokat, amikor tooAzure tárolási írás és visszafejtése során.  SSE jelenleg csak hello Azure Blob szolgáltatáshoz érhető el, és nem használható blokkblobokat, lapblobokat, és hozzáfűző blobokat.  több, lásd: toolearn [Storage szolgáltatás titkosítási az inaktív adatok](../storage/common/storage-service-encryption.md).


> [!Note]
> Miután engedélyezte a titkosítás, csak az új adatok titkosítva van. Minden létező blobot, amely a tárfiók nem titkosított marad. tooencrypt létező blobot, lásd: hello [Storage szolgáltatás titkosítási GYIK](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).
>
>

Storage szolgáltatás titkosítási csak erőforrás-kezelő tárfiókok esetén támogatott. Klasszikus tárfiókokba jelenleg nem támogatottak. toounderstand hello klasszikus és Resource Manager üzembe helyezési modellel, lásd: [Azure üzembe helyezési modellel](../azure-classic-rm.md).

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **engedélyezheti a titkosítást az Azure Storage-fiók**.
   ![Titkosítás engedélyezése tárfiókokon][1]
2. Hello **storage-titkosítás engedélyezéséhez** panel nyílik meg. Ezen a panelen sorolja fel, ahol le a tárolás titkosítása hello Azure storage-fiókok. Ebben a példában most válasszon **storageacct1**.
   ![Engedélyezze a tárolás titkosítása][2]
3. Hello **titkosítási** paneljén **storageacct1** nyílik meg. Válassza ki **engedélyezett**.
   ![Titkosítási panel][3]
4. Kattintson a **Mentés** gombra.

Most már engedélyezte a tárolás titkosítása **storageacct1**.


## <a name="see-also"></a>Lásd még:
Ez a dokumentum bemutatta, hogyan tooimplement hello Security Center ajánlás "engedélyezése a titkosítását az Azure Storage-fiók." További információ az Azure Storage szolgáltatás titkosítási toolearn hello következő lásd:

* [Az Azure Storage szolgáltatás titkosítási az inaktív adatok](../storage/common/storage-service-encryption.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) -megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) -megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) -megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) -megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Azure Security Center: GYIK](security-center-faq.md) -gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
