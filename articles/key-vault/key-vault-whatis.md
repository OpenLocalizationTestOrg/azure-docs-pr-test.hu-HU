---
title: aaaWhat az Azure Key Vault? | Microsoft Docs
description: "Az Azure Key Vault segít a felhőalapú alkalmazások és szolgáltatások által használt titkosítási kulcsok és titkos kulcsok védelmében. Az Azure Key Vault használatával az ügyfelek hardveres biztonsági modulokkal (HSM) védett kulcsokkal titkosíthatják a kulcsokat és a titkos kulcsokat (például a hitelesítési kulcsokat, a tárfiókok kulcsait, az adattitkosítási kulcsokat, a PFX-fájlokat és a jelszavakat)."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 296fcce03658b96b84afab299b73681bbe8ac9fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-key-vault"></a>Mi az Azure Key Vault?
Az Azure Key Vault a legtöbb régióban elérhető. További információkért lásd: hello [Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Bevezetés
Az Azure Key Vault segít a felhőalapú alkalmazások és szolgáltatások által használt titkosítási kulcsok és titkos kulcsok védelmében. A Key Vault lehetővé teszi, hogy hardveres biztonsági modulokkal (HSM) védett kulcsokkal titkosítsa a kulcsokat és a titkos kulcsokat (például a hitelesítési kulcsokat, a tárfiókok kulcsait, az adattitkosítási kulcsokat, a PFX-fájlokat és a jelszavakat). A még nagyobb biztonság érdekében lehetőség van arra is, hogy kulcsokat importáljon és generáljon a hardveres biztonsági modulokban. Ha úgy dönt, toodo, a Microsoft folyamatok a kulcsokat a FIPS 140-2. szintű 2 ellenőrzött hardveres biztonsági modulok (a hardver és a belső vezérlőprogram).  

Key Vault leegyszerűsíti a kulcskezelési folyamat hello, és lehetővé teszi a hozzáférést, és az adatok titkosításához használt kulcsok toomaintain irányítását. A fejlesztők a fejlesztéshez és teszteléshez percek alatt kulcsok létrehozása, és majd zökkenőmentesen áttelepíthetik őket tooproduction kulcsok. Biztonsági rendszergazdák adja meg (és visszavonása) engedély tookeys, igény szerint.

A következő tábla toobetter használata hello megérteni, hogyan segíti a Key Vault a fejlesztők és rendszergazdák toomeet hello igényeinek.

| Szerepkör | Probléma leírása | Az Azure Key Vault megoldása |
| --- | --- | --- |
| Azure-alkalmazásfejlesztő |"Szeretném toowrite egy alkalmazást az Azure-hoz, amely az aláírási és titkosítási kulcsokat használ, de szeretnék ezen kulcsok toobe külső legyenek, hogy hello megoldás megfelelő-e egy földrajzilag elosztott alkalmazáshoz. <br/><br/>Is érdemes ezen kulcsok és titkos toobe védett, anélkül, hogy toowrite hello kód magam megírni. Is szeretnék ezen kulcsok és titkos toobe könnyen számomra toouse az optimális teljesítménnyel alkalmazásaimból." |√ A kulcsok tárolása tárolóban történik, és szükség esetén egy URI segítségével lehet előhívni őket.<br/><br/> √ A kulcsok számára az Azure biztosít védelmet az ipari szabványoknak megfelelő algoritmusok, kulcshosszok és hardveres biztonsági modulok (HSM) segítségével.<br/><br/> √ a kulcsok dolgoznak fel hardveres biztonsági modulok hello megtalálható azonos Azure adatközpontjaiban hello alkalmazásként. Ez biztosítja, nagyobb megbízhatóságot és kisebb késést biztosít Ha hello kulcsok külön helyen, például a helyszínen találhatók. |
| SaaS-fejlesztő |"Nem szeretnék hello felelősséget és esetleges felelősséget a felhasználó bérlőinek kulcsaiért és titkos kulcsok számára. <br/><br/>I szeretné, hogy az ügyfelek tooown hello és a kulcsok kezelése, így I pedig arra összpontosíthatnék mire leginkább, amely szolgáltató hello core szoftverfunkciók." |√ Az ügyfelek importálhatják a saját kulcsaikat az Azure rendszerbe, és kezelhetik őket. Amikor egy SaaS-alkalmazáshoz tooperform titkosítási műveleteket kell az ügyfelek kulcsainak használatával, a Key Vault végzi ezeket a műveleteket hello alkalmazás nevében. hello alkalmazás nem látja a hello ügyfelek kulcsait. |
| Biztonsági vezető (CSO) |"Szeretném, hogy a FIPS 140-2 2. szint HSM-EK a biztonságos kulcskezelés tekintetében megfelelnek tooknow. <br/><br/>Szeretné, hogy a saját szervezetem hello kulcs életciklus irányítását toomake rendelkezem, és képes figyelni a kulcshasználatot. <br/><br/>És bár több Azure-szolgáltatások és erőforrások használjuk, toomanage hello kulcsokat az Azure-ban egyetlen helyről szeretném." |√ A hardveres biztonsági modulokat a FIPS 140-2 2. szintje szerint ellenőrizték.<br/><br/>√ A Key Vault kialakításának köszönhetően a Microsoft nem tekintheti meg, illetve nem nyerheti ki a kulcsokat.<br/><br/>√ Közel valós idejű kulcshasználat-naplózás.<br/><br/>√ hello tároló egyetlen felületet, függetlenül attól, hogy Ön hány tárolóval rendelkezik az Azure-régiók azok biztosít, támogatási, és mely alkalmazások használják őket. |

Azure-előfizetés birtokában bárki létrehozhat és használhat kulcstárolót. Bár a Key Vault elsősorban a fejlesztők és a rendszergazdák számára hasznos, a szervezet más Azure-szolgáltatásait kezelő rendszergazdák is beállíthatják és kezelhetik. Például a rendszergazda volna jelentkezzen be Azure-előfizetéssel, létrehozhat egy tárolót hello szervezet mely toostore kulcsok és majd felelős üzemeltetési feladatokat, például:

* A kulcsok vagy titkos kulcsok létrehozása és importálása
* A kulcsok vagy titkos kulcsok visszavonása, illetve törlése
* Engedélyezi a felhasználók vagy alkalmazások tooaccess hello kulcstároló, így azok kezeléséhez, vagy használja a benne tárolt kulcsokat és titkos kulcsok
* A kulcshasználat konfigurálása (például aláíráshoz vagy titkosításhoz)
* A kulcshasználat figyelése

Ez a rendszergazda szeretné majd fejlesztők URI-azonosítók toocall saját alkalmazásaikból, és a biztonsági rendszergazda kulcshasználati naplózási információkkal. 

   ![Az Azure Key Vault áttekintése][1]

A fejlesztők is kezelhetők hello kulcsok közvetlenül, API-k használatával. További információkért lásd: [Key Vault fejlesztői útmutatója hello](key-vault-developers-guide.md).

## <a name="next-steps"></a>Következő lépések
A rendszergazdáknak szóló bevezető oktatóanyagért tekintse meg a [Bevezetés az Azure Key Vault használatába](key-vault-get-started.md) című cikket.

A Key Vault használatának naplózásával kapcsolatos további információkért tekintse meg [Az Azure Key Vault naplózása](key-vault-logging.md) című cikket.

A kulcsoknak és titkos kulcsoknak az Azure Key Vault szolgáltatással való használatával kapcsolatos további információkért tekintse meg az [About Keys, Secrets, and Certificates](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx) (Információk a kulcsokról, titkos kulcsokról és tanúsítványokról) című cikket.

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
