---
title: "az Active Directory hibrid identitáskezelési kialakítási szempontok - aaaAzure lépések |} Microsoft Docs"
description: "Egy összegzést és hello hibrid Identitáskezelés – kialakítási szempontokat útmutató elolvasása után követő lépések"
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 02d48768-ea9e-4bfe-ae54-b54c4bd0a789
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 7100eaaf61a7b3b7d38a381f6bb9d8b82677c352
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-hybrid-identity-design-considerations--next-steps"></a>Az Azure Active Directory hibrid identitáskezelési kialakítási szempontok-következő lépések
Most, hogy befejezte a követelmények meghatározását és megvizsgálta az összes hello lehetséges mobileszköz-kezelési megoldást, készen áll a tootake hello tovább központi telepítésének lépései támogató infrastruktúra, amely jogot az Ön és szervezete hello most.

## <a name="hybrid-identity-solutions"></a>Hibrid identitáskezelési megoldások
-Igazodó az igényeihez konkrét megoldási forgatókönyvek felhasználásával egy kiváló módja tooreview és tervezze meg a mobileszköz-kezelési infrastruktúra üzembe hello részleteit. hello a következő megoldások felvázolják hello leggyakrabban használt mobileszköz-kezelési forgatókönyveket:

* Hello [mobileszközök és számítógépek kezelése vállalati környezetekben megoldás](https://technet.microsoft.com/library/dn582037.aspx) segítséget nyújt a mobileszközök kezelése a helyszíni System Center 2012 Configuration Manager infrastruktúrát kiterjeszti a Microsoft hello felhő Intune-ban. Ez a hibrid infrastruktúrával informatikai szakemberek a közepes és nagy méretű környezetekben lehetővé BYOD és a távelérés közben csökkenthetik az Adminisztráció összetettségét.
* Hello [Configuration Manager 2007-megoldás mobileszköz-kezelés](https://technet.microsoft.com/library/dn508400.aspx) segítséget nyújt a mobileszközök felügyeletéhez, amikor az infrastruktúra alapja a System Center Configuration Manager 2007. A megoldás bemutatja, hogyan tooset be egy kiszolgálót a System Center 2012 Configuration Manager fut, akkor utána a Microsoft Intune futtatásával kihasználhassa annak mobileszköz-kezelési képességeit.
* Hello [mobileszközök felügyeletéhez a kisebb környezetekben megoldás](https://technet.microsoft.com/library/dn715906.aspx) toosupport MDM igénylő kisvállalatok számára készült Azt is bemutatja, hogyan toouse a Microsoft Intune tooextend az aktuális infrastruktúra toosupport mobileszköz-kezelés és a BYOD. Ez a megoldás hello legegyszerűbb Microsoft Intune-t önálló, csak felhőalapú konfigurációban, helyi kiszolgáló nélkül használja támogatott forgatókönyvet írja le.

## <a name="hybrid-identity-documentation"></a>Hibrid identitáskezelési dokumentáció
Az elméleti és eljárástervezési, telepítési és felügyeleti tartalmak hasznosak a mobileszköz-kezelési megoldás megvalósításához:

* [A Microsoft System Center](https://technet.microsoft.com/library/cc507089.aspx) megoldások segítségével rögzítheti és összesítheti, az infrastruktúra, házirendek, folyamatok és ajánlott eljárások kapcsolatos tudnivalókat, hogy az informatikai munkatársak kezelhető rendszereket építhetnek és automatizálhatják a műveleteket.
* [A Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx) van egy felhőalapú Eszközkezelési szolgáltatás, amely akkor toomanage segít a számítógépek és mobileszközök és toosecure a vállalati információk.
* [Az Office 365 MDM](https://technet.microsoft.com/library/ms.o365.cc.devicepolicy.aspx) lehetővé teszi toomanage és a mobileszközök biztonságossá ha azok csatlakoztatott tooyour Office 365-szervezetben. Az Office 365 tooset eszköz biztonsági házirendek és hozzáférési szabályok és toowipe mobileszközök MDM használható, ha elvesztett vagy ellopott.

## <a name="hybrid-identity-resources"></a>Hibrid identitás erőforrások
Figyelés hello következő erőforrások gyakran hello legfrissebb hírekhez és frissíti a mobileszköz-kezelési megoldások:

* [A Microsoft nagyvállalati mobilitási blog](http://blogs.technet.com/b/enterprisemobility/)
* [A Microsoft hello Cloud blog](http://blogs.technet.com/b/in_the_cloud/)
* [A Microsoft Intune blogja](http://blogs.technet.com/b/microsoftintune/)
* [Microsoft System Center Configuration Manager blog](http://blogs.technet.com/b/configurationmgr/)
* [Microsoft System Center Configuration Manager munkacsoportjának blogja](http://blogs.technet.com/b/configmgrteam/)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

