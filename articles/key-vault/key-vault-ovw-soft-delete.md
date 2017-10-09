---
ms.assetid: 
title: "aaaAzure Key Vault helyreállítható törlésre |} Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/10/2017
ms.openlocfilehash: 1b402c58db6f25ae4ae5e2720786fa81eb0e839a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-soft-delete-overview"></a>Az Azure Key Vault soft-törlés áttekintése

Key Vault helyreállítható törlés funkcióval törölt hello tárolók és a tároló objektumok soft-törlés néven ismert. Pontosabban a következő forgatókönyvek hello oldjuk:

- Helyreállítható törlésre kerültek a kulcstároló támogatása
- Támogatási helyreállítható törlésre kulcstároló-objektumok (például) kulcsok, a titkos kulcsokat, a tanúsítványok)

## <a name="supporting-interfaces"></a>Kapcsolatok támogatása

hello soft-törlés funkció érhető el eredetileg keresztül hello REST, .NET / C# és PowerShell-felületek. Ezeket a további részletekért tekintse meg a toohello hivatkozások [Key Vault hivatkozás](https://docs.microsoft.com/azure/key-vault/).

## <a name="scenarios"></a>Forgatókönyvek

Az Azure Key tárolók nyomon követett, Azure Resource Manager által kezelt erőforrások. Az Azure Resource Manager is törlésre, amely megköveteli, hogy egy sikeres törlési művelet eredménye nem érhető el többé erőforrás jól meghatározott tulajdonság határozza meg. hello soft-törlés funkció címek hello helyreállítási hello törölt objektum, hogy hello törlés véletlen vagy szándékos volt.

1. Hello szokásos esetben a felhasználó véletlenül törölték a kulcstároló vagy kulcstároló objektum; Ha adott kulcstároló vagy kulcsot használó objektum toobe helyreállítható volt előre meghatározott időtartamra, hello felhasználói hello visszavonni, és az adatok helyreállítását.

2. Más esetben egy rosszindulatú felhasználó megkísérelhet toodelete kulcstároló vagy kulcstároló-objektum, például egy kulcsot a tároló, a business szüneteltetése toocause belül. Hello törlése, hello Kulcstároló Kulcstároló objektum mappától hello tényleges hello alapjául szolgáló adatok törlését is használhatók, biztonsági intézkedésként például adatok törlését tooa különböző, a jogosultságok korlátozása révén megbízható szerepkört. Ez a megközelítés hatékonyan igényel kvórum egy műveletet, amelyek egyébként egy azonnali adatvesztés.

### <a name="soft-delete-behavior"></a>Helyreállítható törlési viselkedés

Ezzel a funkcióval kulcstároló a törlési művelet hello vagy kulcstároló objektum soft-törlési, hatékonyan tartó hello erőforrásokhoz egy adott megőrzési időszak jogosultságot ad a hello megjelenését hello objektum törlése közben. További hello szolgáltatás lehetővé teszi a helyreállítás hello törölt objektum, tulajdonképpen a hello törlés visszavonása. 

Soft-törlés választható Key Vault viselkedése és **alapértelmezés szerint nincs engedélyezve** ebben a kiadásban. További információ a key vaultban, soft-törlésének engedélyezése: hello konkrét útmutatást hello útmutatóban hello interfész az Ön által választott [Key Vault hivatkozás](https://docs.microsoft.com/azure/key-vault/).

### <a name="key-vault-recovery"></a>Kulcstároló-helyreállítás

Hello szolgáltatást kulcstároló törlése, akkor hoz létre a hello előfizetésben, a helyreállítás megfelelő metaadatok hozzáadása a proxy erőforrás. hello proxy erőforrás elérhető hello tárolt objektum törölt hello kulcs tárolóval megegyező helyen. 

### <a name="key-vault-object-recovery"></a>Kulcstároló objektum-helyreállítást

A kulcstároló objektumot, például a kulcs törlése után hello szolgáltatás helyezi el a következő hello objektum törölt állapotban, így azt nem érhető el tooany adatbeolvasási műveletekkel. Ebben az állapotban lévő hello kulcstároló objektum is csak akkor jelenik meg, helyreállított vagy kényszerített módon/véglegesen törlődik. 

Hello: azonos idő, a kulcs tároló beütemezett alapjául szolgáló adatok megfelelő toohello törölt hello hello törlésének kulcstároló vagy kulcstároló objektum egy előre meghatározott megőrzési időtartam után végrehajtásra. hello DNS rekord megfelelő toohello tárolót is megmarad hello adatmegőrzési időköz hello időtartama.

### <a name="soft-delete-retention-period"></a>Soft-törlése a megőrzési időtartam

Letölthető törölt erőforrásokat a beállított időn belül, 90 nap jelennek meg. Során hello soft-törlés megőrzési idejét, a következő apply hello:

- Előfordulhat, hogy listában összes hello kulcstárolójának és kulcstároló objektumok hello soft-törlés állapotú a előfizetés, valamint a rájuk vonatkozó törlése és a helyreállítási adatok eléréséhez.
    - Csak különleges engedélyekkel rendelkező felhasználók is listázhatja törölve tárolók. Azt javasoljuk, hogy a felhasználók egyéni szerepkör létrehozása kezelési törölt tárolók ezek különleges engedélyekkel.
- A kulcstároló ugyanazzal a névvel nem hozható létre hello hello ugyanazon a helyen; Ennek megfelelően egy kulcstároló objektum nem hozható létre egy adott tárolóban, ha adott kulcstároló hello objektum tartalmazza ugyanazt a nevet, és amely törölt állapotban van. 
- Csak a kifejezetten kiemelt felhasználó kulcstároló vagy kulcstároló objektum lehet, hogy visszaállítása a helyreállítás parancsot a hello megfelelő proxy erőforráson.
    - hello felhasználói hello egyéni biztonsági szerepkört, hello jogosultság toocreate hello erőforráscsoportba tartozó kulcstároló, aki tagja hello tároló visszaállításához.
- Csak a kifejezetten jogosultsággal rendelkező felhasználó lehet, hogy kényszerített kulcstároló vagy kulcstároló objektum törlése a Törlés parancsot a hello megfelelő proxy erőforráson.

Kivéve, ha egy kulcstartót vagy kulcstároló objektum helyre lett állítva, hello: hello adatmegőrzési időköz hello szolgáltatás végét a kiürítést végző helyreállíthatóan törölt hello kulcstároló vagy a kulcstároló objektum és a benne lévő tartalom. Erőforrás törlése nem lehetséges, hogy újraütemezte.

### <a name="permitted-purge"></a>Engedélyezett kiürítése

Végleg törlése végleges törlése, kulcstároló alkalmazáson keresztül a POST műveletet hello proxy erőforráson, amely speciális jogosultságra van szükség. Általában csak a hello előfizetés tulajdonosa lesz képes toopurge kulcstároló. hello POST művelet eseményindítók hello azonnali és helyreállíthatatlan, hogy a tároló törlése. 

Egy kivétel toothis helyzet hello olyankor, amikor hello Azure-előfizetés van megjelölve *undeletable*. Ebben az esetben csak hello szolgáltatást majd végezhet hello tényleges törlésre, és így tesz, ütemezett folyamatként. 



