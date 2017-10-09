---
title: "aaaEnable átlátható adattitkosítást az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** engedélyezése átlátszó adatok titkosítás **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 94c6e9a1feddaa48faac6c835d416c4d131cd5c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Engedélyezze az átlátható adattitkosítást az Azure Security Centerben
Azure Security Center javasolni fogja engedélyezése átlátszó Data Encryption (TDE) az SQL-adatbázisok, ha a TDE nincs engedélyezve. TDE védi az adatokat, és segítséget nyújt a megfelelőségi követelményeknek a adatbázis, a társított biztonsági másolatok és a tranzakciós naplófájlok nyugalmi, anélkül, hogy a módosítások tooyour alkalmazás titkosításával. több lásd toolearn [átlátható adattitkosítást az Azure SQL Database](https://msdn.microsoft.com/library/dn948096).

Ez a javaslat alkalmazása toohello Azure SQL-szolgáltatás csak; a virtuális gépeken futó SQL nem tartalmaz.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  A dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **átlátható adattitkosítási engedélyezése**.
   ![Transzparens adattitkosítás engedélyezése][1]
2. Ekkor megnyílik a hello **átlátható adattitkosítási engedélyezése az SQL-adatbázisok** panelen. Jelölje be egy SQL-adatbázis tooenable TDE.
   ![Az SQL DB tooenable TDE kiválasztása][2]
3. A hello **átlátható adattitkosítás** panelen válassza **ON** adatok titkosítását, és válassza a **mentése** hello felső szalagon hello panelről.
   ![Kapcsolja be a TDE][3]

   Ha a TDE engedélyezve van a hello kijelölt SQL-adatbázis hello **titkosítási állapotát** túl változik**titkosított**.    

   ![A titkosítás állapota][4]

## <a name="see-also"></a>Lásd még:
Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "Enable átlátható adattitkosítási." toolearn Erőforráscsoportoknál, kapcsolatos további információkért tekintse meg a hello következőt:

* [Az Azure SQL Database átlátható adattitkosítás](https://msdn.microsoft.com/library/dn948096)
* [Ismerkedés a transzparens adatok titkosítás (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
