---
title: "hello Azure piactér ajánlatot létrehozása aaaNon-technikai előfeltételei |} Microsoft Docs"
description: "A létrehozása és telepítése az ajánlat toohello Azure piactér mások hello követelmények megértése érdekében toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3dae463b-8f48-4f52-8fa8-4e3975f09f43
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: hascipio
ms.openlocfilehash: 472096863084cc58dc921313419ab60b1a08a3bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="general-prerequisites-for-creating-an-offer-for-hello-azure-marketplace"></a>Általános Előfeltételek létrehozásához hello Azure piactér vonatkozó ajánlatot
Ismerje meg hello általános, üzleti folyamat-központú előfeltételeket, a hello szükséges toomove kínálnak létrehozási folyamata.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Győződjön meg arról, hogy a Microsoft egy értékesítő néven regisztrált
Részletes információkra van szüksége egy értékesítő fiók regisztrálása a Microsoft, a go túl[fióklétrehozás és a regisztrációs](marketplace-publishing-accounts-creation-registration.md).

* **Ha a vállalata már regisztrálva van egy értékesítő hello fejlesztői központban a, és azt szeretné, hogy egy új ajánlat toocreate** majd bejelentkezési toohello közzétételi portálhoz a hello azonos e-mail-azonosító, mely fejlesztői központban való regisztrálása történik. Ez a lépés akkor szükséges, így hello Dev Center webhely és a közzétételi portal kapcsolódnak egymáshoz.
* **Ha a vállalata már regisztrálva van egy értékesítő hello fejlesztői központban a, és azt szeretné, hogy egy meglévő ajánlatot tooedit** vagy bejelentkezési toohello közzétételi portal hello rendszergazdai fiókkal vagy egy olyan fiókkal, amely egy hello társadminisztrátor meg van adva, majd portal közzététele . Az alábbi táblázat a lépéseket tooadd egy társadminisztrátori fiókot.

## <a name="steps-tooadd-a-co-admin-in-hello-publishing-portal"></a>Lépéseket tooadd egy társadminisztrátor az hello közzétételi Portáljára
Rendszergazdáknak az hello portal közzétételi adhat hozzá a hello vállalati más tagjai hello alkalmazásra, mint egy hello társadminisztrátor dolgozó hello portal közzététele. **Feltételezve, hogy Üdvözöljük a rendszergazdákat,** az alábbi vannak hello lépéseket tooadd egy co-rendszergazda segítségét.

> [!NOTE]
> Az új felhasználók számára, mielőtt hozzáadhat egy társadminisztrátor hello közzétételi portal, győződjön meg arról, hogy létrehozta a hello legalább egy alkalmazás közzététele a portálon. Erre azért szükség, mint a hello **KÖZZÉTEVŐK** lap jelenik meg, csak a legalább egy alkalmazás hello létrehozását követően portal közzététele.
> 
> 

1. Győződjön meg arról hello társadminisztrátor e-mail azonosító egy Microsoft account(MSA). Ha nem, regisztrálja, a felügyelt Szolgáltatásfiók használatát az [hivatkozás](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Győződjön meg arról, hogy van-e legalább egy alkalmazás hello rendszergazdai fiókkal előtt tooadd egy co-rendszergazda segítségét.
3. Hello a fenti lépések után történik, bejelentkezési toohello hello társadminisztrátor e-mail azonosítójú portal közzététele, és ezután jelentkezzen ki.
4. Most bejelentkezési toohello hello rendszergazda e-mailek azonosítójú portal közzététele.
5. Keresse meg tooPublishers -> Válassza ki a fiók -> rendszergazdák -> Hozzáadás hello társadminisztrátor (az alábbi képernyőfelvételen)
   
    ![rajz](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Győződjön meg arról, hogy e-mailben azonosítók megadott hello: hello közzétételi (pl. fejlesztői központhoz, portal közzététele) folyamat különböző szakaszaiban kell figyelni a Microsoft bármely kommunikációs.
7. Fejlesztői központ regisztrációs ne egy olyan fiókkal, egyetlen személy társított. Ez a függőség eltávolításához magánszemély javasolt.
8. Ha szembesülhetnek a fejlesztői központban regisztrációs merül fel, majd léptesse elő a jegy használatát az [hivatkozás](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-toodelete-a-co-admin-in-hello-publishing-portal"></a>Lépéseket toodelete egy társadminisztrátor hello a portál közzététele
**Feltételezve, hogy Üdvözöljük a rendszergazdákat,** az alábbi vannak hello lépéseket toodelete egy co-rendszergazda segítségét.

1. Bejelentkezési toohello hello rendszergazda e-mailek azonosítójú portal közzététele.
2. Keresse meg a túl**közzétevők** -> Válassza ki a fiók -> **rendszergazdák** -> **Társadminisztrátoroknak**.
3. Kattintson a hello **X** gomb következő toohello társadminisztrátor kívánt tot törlése (az alábbi képernyőfelvételen).
   
    ![rajz](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> Toocomplete vállalati adó és banki adatok nem rendelkeznek, ha tervezi, toopublish csak szabad ajánlatok (vagy saját licenc).
> 
> hello vállalati regisztrációs lépések befejezett tooget kell lennie. Azonban a vállalat hello adó és banki adatok hello Microsoft Developer-fiók működik, amíg hello fejlesztők megkezdheti a munkát hello virtuálisgép-lemezkép létrehozásáról hello [közzétételi Portáljára](https://publish.windowsazure.com), hitelesített, az első és hello Azure átmeneti környezet tesztelve. Szüksége lesz hello teljes értékesítő fiók jóváhagyási csak hello utolsó lépésében a ajánlat toohello Azure piactér közzététele.
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Szerezzen be a "használatalapú" Azure-előfizetés
Ez a hello előfizetés fog használni toocreate a Virtuálisgép-lemezképeket, és kézi hello képek toohello keresztül [Azure piactér](https://azure.microsoft.com/marketplace/). Ha még nem rendelkezik egy meglévő előfizetéshez, majd jelentkezzen: https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>"Értékesít-feladó" országok
> [!WARNING]
> A sorrend toosell hello Azure piactérről, a szolgáltatások győződjön meg arról, hogy a regisztrált entitás egy jóváhagyott hello "értékesít-az" ország. Ez a korlátozás kifizetés és adózás okból is. Azt aktívan keresést tooexpand a országok listája hello jövőbeli közelében, így kövessen bennünket. Hello teljes listáját, lásd: a hello szakasz 1b [Azure piactér részvételét házirendek](http://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## <a name="next-steps"></a>Következő lépések
Miután hello nem technikai jellegű előfeltételek teljesülnek, ezután előfeltételei hello ajánlat meghatározott műszaki. Kattintson a hello hivatkozás toohello cikk hello megfelelő ajánlattípus, hogy szeretné-e az Azure piactér hello toocreate.

* [Virtuális gép műszaki Előfeltételek](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Megoldás sablon műszaki Előfeltételek](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Lásd még:
* [Első lépések: hogyan toopublish egy ajánlat toohello Azure piactéren](marketplace-publishing-getting-started.md)

