---
ms.assetid: 
title: "Az Azure Key Vault biztonsági világot |} Microsoft Docs"
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: aeab36153d3a4e004d12206bab92cea9b30400ad
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/19/2018
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Az Azure Key Vault biztonsági világot és földrajzi határok

Az Azure Key Vault egy több-bérlős szolgáltatás, és az egyes Azure-beli hely használja a hardveres biztonsági modulokkal (HSM) készletét. 

A ugyanabban a földrajzi régióban Azure helyen lévő összes HSM ossza meg az azonos kriptográfiai határ (Thales Biztonságivilág). Például USA keleti régiója, és az USA nyugati régiója megoszthatja a ugyanazt biztonsági világ mert tartoznak az USA földrajzi hely. Ehhez hasonlóan japán minden Azure hely ossza meg a ugyanazt biztonsági világ és az összes Azure helyek Ausztrália, India és így tovább. 

## <a name="backup-and-restore-behavior"></a>Biztonsági mentés és helyreállítás viselkedése

Egy készült biztonsági másolatok a kulcstároló egyetlen Azure helyen kulcs vissza tudja állítani egy másik Azure-beli hely, a kulcstároló, amíg az alábbi két feltétel teljesül:

- Az Azure-hely tartoznak ugyanazon a földrajzi helyen
- Mind a kulcstárolók az azonos Azure-előfizetés tartozik

Például egy kulcs kulcstároló Nyugat-Indiában, az adott előfizetés által végrehajtott biztonsági csak vissza tudja állítani az ugyanahhoz az előfizetéshez és a földrajzi hely; egy másik kulcstároló Nyugat-Indiában, közép-Indiában és Dél-India.

## <a name="regions-and-products"></a>Régiók és termékek

- [Azure-régiók](https://azure.microsoft.com/regions/)
- [Microsoft-termékek régió szerint](https://azure.microsoft.com/regions/services/)

Régiók biztonsági világot fő fejlécében a táblázatok látható van leképezve:

A termékek régió cikkben, például a **Americas** lap tartalmaz kelet VELÜNK, USA KÖZÉPSŐ RÉGIÓJA, USA nyugati RÉGIÓJA Americas régió összes leképezésére. 

>[!NOTE]
>Egy kivétel ez alól, Velünk DOD EAST és a US DOD központi rendelkezik-e a saját biztonsági világot. 

Hasonlóképpen, a a **Európa** lap, Észak-Európa, és Nyugat-Európa, mind a Európa régió van leképezve. Ugyanez érvényes is a a **Ázsia Csendes-óceáni** fülre.



