---
title: "aaaService kvótái és korlátai Azure Batch |} Microsoft Docs"
description: "További tudnivalók az alapértelmezett Azure Batch kvóták, korlátok és korlátozások és hogyan toorequest kvóta növeli"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6035d1c7618cfe97ebca3780e02a4ee34f54e534
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="batch-service-quotas-and-limits"></a>A Bach szolgáltatás kvótái és korlátozásai

Mint az egyéb Azure-szolgáltatásokkal, nincsenek korlátozások bizonyos hello Batch szolgáltatás társított erőforrások. Ezek a korlátozások számos alapértelmezett kvóták alkalmazása az Azure-ban hello előfizetés vagy a fiók szintjén. Ez a cikk ismerteti azokat az alapértelmezett beállításokat, és hogyan kérheti a kvótájának növeli.

Vegye figyelembe ezeket a kvótákat a Batch számítási feladatok tervezésekor és bővítésekor. Például ha a készlet a megadott számítási csomópontok száma hello cél nem elérése, előfordulhat, hogy elérte hello core kvótakorlátot a Batch-fiók, vagy a regionális virtuális gép magok kvóta az előfizetéséhez.

Több kötegelt feladatok futtatása egyetlen Batch-fiók, vagy a Batch-fiókok, amelyek a hello munkaterhelésének terjesztése ugyanahhoz az előfizetéshez, de különböző Azure-régiókban.

Ha azt tervezi, hogy a kötegben toorun termelési számítási feladatokhoz, szükség lehet tooincrease egy vagy több hello kvóták fenti hello alapértelmezett. Ha azt szeretné, hogy tooraise egy kvótát, az online megnyithatja [ügyfél-támogatási kérelem](#increase-a-quota) díjmentesen.

> [!NOTE]
> A kvóta legfeljebb, nem kapacitás garancia. Ha nagyméretű lemezkapacitási igényekről, forduljon az Azure támogatási szolgálatához.
> 
> 

## <a name="resource-quotas"></a>Erőforráskvóták
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a>Kvótákat felhasználói előfizetési módban

Tárolókészlet foglalási módban a túl a Batch-fiókhoz**felhasználói előfizetési**, a Batch-virtuális gépek és más erőforrások, például a storage-fiókok, közvetlenül az előfizetésében jönnek létre, amikor egy alkalmazáskészlet jön létre. hello Azure Batch magok kvóta nem érvényes ebben a módban létrehozott tooan fiók. Ehelyett hello kvóták regionális számítási maggal és más erőforrásokhoz az előfizetésben érvényesek. További információ a ezek mely százalékértékénél kéri [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).

Erőforrás kihasználtsága egy olyan fiók, létre felhasználói előfizetési módban tervezésekor a következő kötegelt erőforrások (a hozzáadása toocompute magok) Megjegyzés hello minden 40 Linux virtuális gépek, vagy szükségesek 20 Windows virtuális gépek:

| Erőforrás | Kvóta | Szolgáltató |
| --- | ---| --- |
| Egy tárfiók | Tárfiókok | Microsoft.Storage |
| Egy nyilvános IP-cím | Nyilvános IP-címek | Microsoft.Network | 
| Egy virtuális hálózat | Virtuális hálózatok | Microsoft.Network | 
| Egy hálózati biztonsági csoport | Network Security Groups (Hálózati biztonsági csoportok) | Microsoft.Network | 
| Egy virtuálisgép-méretezési csoport | Virtual Machine Scale Sets | Microsoft.Compute | 
| Egy terheléselosztó | Terheléselosztók | Microsoft.Network | 

hello magok kvóta regionális szinten, vagy a virtuális gép termékcsalád kell set függően toohello Virtuálisgép-méretet a Batch-készlet vagy készletek szükséges:

| Kvóta | Szolgáltató |
| --- | ---- |
| Teljes regionális magok | Microsoft.Compute |
| … Családbiztonsági magok | Microsoft.Compute |



## <a name="other-limits"></a>Egyéb korlátozások
| **Erőforrás** | **Felső korlát** |
| --- | --- |
| [Egyidejű feladatok](batch-parallel-node-tasks.md) egyes számítási csomópontjain |csomópont magok száma 4 x |
| [Alkalmazások](batch-application-packages.md) / Batch-fiókhoz. |20 |
| Alkalmazáscsomagok alkalmazásonként |40 |
| Csomag mérete (minden) |KB. 195GB<sup>1</sup> |
| Maximális kezdő tevékenység mérete | 32768 karakterek<sup>2</sup> |

<sup>1</sup> blob maximális blokkméretének azure tárolási kapacitása<br />
<sup>2</sup> erőforrás fájlok és a környezeti változók

## <a name="view-batch-quotas"></a>Kötegelt kvóták megtekintése
Tekintse meg a Batch-fiók kvótákat hello [Azure-portálon][portal].

1. Válassza ki **Batch-fiókok** hello portálon, majd válassza ki kíváncsiak vagyunk hello Batch-fiókhoz.
2. Válassza ki **tulajdonságok** hello kötegelt fiók menü panelen.
3. hello tulajdonságok panelen megjeleníti hello **kvóták** jelenleg alkalmazott toohello Batch-fiókhoz.
   
    ![Batch-fiók kvóták][account_quotas]

Batch-fiók felhasználói előfizetési módban létrehozott nézet hello kapcsolatos hello Azure Portal előfizetés kvótáját.

1. Válassza ki **előfizetések**, és válassza ki a Batch-fiókhoz hello alkalmaz hello előfizetést.

2. A hello **előfizetés** panelen válassza **használati + kvóták**.



## <a name="increase-a-quota"></a>A kvóta növeléséhez
Kövesse ezeket a kvóta növeléséhez a Batch-fiók vagy a feliratkozás hello lépéseket toorequest [Azure-portálon][portal]. hello kvótájának növelését függ hello tárolókészlet foglalási mód a Batch-fiókhoz.

### <a name="increase-a-batch-cores-quota"></a>A kötegelt magok kvóta növelése 

Ha a Batch-fiókhoz készült **a Batch szolgáltatás** mód, kövesse ezeket a lépéseket toorequest a kötegelt magok kvóta növelését:

1. Jelölje be hello **súgó + támogatás** csempét a portál irányítópultján, vagy a kérdőjel hello (**?**) hello portál jobb felső sarkában hello.
2. Válassza ki **új támogatja a kérelem** > **alapjai**.
3. A hello **alapjai** panel:
   
    a. **Típusú** > **kvóta**
   
    b. Válassza ki előfizetését.
   
    c. **Kvóta típusa** > **kötegelt**
   
    d. **Támogatás megléte** > **kvóta támogatásához -**
   
    Kattintson a **Tovább** gombra.
4. A hello **probléma** panel:
   
    a. Válassza ki a **súlyossági** tooyour szerint [üzletmenetre gyakorolt hatás][support_sev].
   
    b. A **részletek**, adja meg minden egyes toochange hello Batch-fiók nevét és hello új korlát kívánt kvótát.
   
    Kattintson a **Tovább** gombra.
5. A hello **elérhetőségi adatai** panel:
   
    a. Válassza ki a **elsődleges kapcsolattartási módszert**.
   
    b. Győződjön meg arról, és írja be a szükséges hello kapcsolattartási adatait.
   
    Kattintson a **létrehozása** toosubmit hello támogatási kérelmet.

Ha a támogatási kérelmet küldött, az Azure támogatási kapcsolatba lép Önnel. Vegye figyelembe, hogy hello kérelem befejezése is eltarthat too2 munkanapos határidejűek.

### <a name="increase-a-subscription-cores-quota"></a>Egy mag előfizetési kvóta növeléséhez

Ha a Batch-fiókhoz készült **felhasználói előfizetési** módot, és át kell további területi vagy virtuális gép termékcsalád mag, a kvóta kérelem növelje az előfizetésben. Útmutató: [erőforrás-kezelő core kvóta növelése kérelmek](../azure-supportability/resource-manager-core-quotas-request.md).



## <a name="related-topics"></a>Kapcsolódó témakörök
* [Hello Azure portál használata az Azure Batch-fiók létrehozása](batch-account-create-portal.md)
* [Azure Batch funkcióinak áttekintése](batch-api-basics.md)
* [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
