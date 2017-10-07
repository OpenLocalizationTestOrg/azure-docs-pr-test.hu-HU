---
ms.assetid: 
title: "aaaAzure Key Vault biztonsági világot |} Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 1995119c9e9886829d6c50af921f275d10e382f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Az Azure Key Vault biztonsági világot és földrajzi határok

Az Azure Key Vault egy több-bérlős szolgáltatás, és az egyes Azure-beli hely használja a hardveres biztonsági modulokkal (HSM) készletét. 

Minden HSM-EK a hello Azure helyeken azonos földrajzi régióban megosztás hello ugyanazt a határt kriptográfiai (Thales Biztonságivilág). Például USA keleti régiója és megosztása USA nyugati hello ugyanazt biztonsági világ mert toohello USA földrajzi hely tartoznak. Hasonlóképpen japán megosztáson található összes Azure helyét hello ugyanazt biztonsági világ és Ausztráliában-Indiában, minden Azure helyek, és így tovább. 

## <a name="backup-and-restore-behavior"></a>Biztonsági mentés és helyreállítás viselkedése

Egy készült biztonsági másolatok a kulcstároló egy Azure-beli hely lehet kulcs visszaállítása tooa egy másik Azure-beli hely, a kulcstároló, mindaddig, amíg az alábbi két feltétel teljesülése:

- Mindkét hello Azure helyek tartozik toohello ugyanazon a földrajzi helyen
- Mindkét hello kulcstárolójának tartozik toohello azonos Azure-előfizetés

Például egy kulcs kulcstároló Nyugat-Indiában, az adott előfizetés által végrehajtott biztonsági csak lehet visszaállított tooanother kulcstároló hello az ugyanahhoz az előfizetéshez és a földrajzi hely; Nyugat-Indiában, közép-Indiában és Dél-India.

## <a name="regions-and-products"></a>Régiók és termékek

- [Azure-régiók](https://azure.microsoft.com/regions/)
- [Microsoft-termékek régió szerint](https://azure.microsoft.com/regions/services/)

Csatlakoztatott toosecurity világot hello táblák fő fejlécében megjelenő régiók a következők:

Hello termékek régió cikkben, például hello **Americas** lap tartalmaz kelet VELÜNK, USA KÖZÉPSŐ RÉGIÓJA, USA nyugati RÉGIÓJA összes leképezési toohello Americas régióban. 

>[!NOTE]
>Egy kivétel ez alól, Velünk DOD EAST és a US DOD központi rendelkezik-e a saját biztonsági világot. 

Hasonlóképpen, a hello **Európa** lap, Észak-Európa, és Nyugat-Európa toohello Európa régió mindkét képezze le. hello is is igaz a hello **Ázsia Csendes-óceáni** fülre.



