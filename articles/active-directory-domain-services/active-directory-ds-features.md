---
title: "Az Azure Active Directory tartományi szolgáltatások: Szolgáltatások |} Microsoft Docs"
description: "Az Azure Active Directory tartományi szolgáltatások jellemzői"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1a9417bafa35959d280eabf3db6ad8530db4ea45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Szolgáltatások
a következő funkciók hello Azure AD tartományi szolgáltatások által felügyelt tartományok érhetők el.

* **Egyszerű üzembe helyezését:** Azure AD tartományi szolgáltatásokat az Azure AD-bérlő néhány kattintással használatával engedélyezheti. Függetlenül attól, hogy az Azure AD-bérlőről egy felhő-bérlő, vagy a helyszíni címtárral szinkronizálja a felügyelt tartományok gyorsan telepíthető.
* **Tartományhoz való csatlakozást támogatása:** könnyedén hello Azure virtuális hálózat, a felügyelt tartományok érhető el a számítógépek tartományhoz való csatlakozást. a Windows ügyfél és kiszolgáló operációs rendszerek működik problémamentesen szemben Azure AD tartományi szolgáltatások által kiszolgált tartományok hello a tartományhoz való csatlakozást felületet. Használhatja az automatikus tartományhoz való csatlakozást tooling eszköz ilyen tartományok ellen is.
* **Egy tartomány példány lehet az Azure AD-címtár:** egy egyetlen Active Directory-tartomány minden Azure AD-címtár is létrehozhat.
* **Hozzon létre egyéni nevek tartományok:** hozhat létre Azure AD tartományi szolgáltatásokat használó tartományok egyéni nevű (például "contoso100.com"). Használhatja bármelyik ellenőrzött és érvényes vagy nem ellenőrzött tartomány nevét. Szükség esetén is létrehozhat egy tartomány hello beépített tartományi utótaggal rendelkező (Ez azt jelenti, hogy "*. onmicrosoft.com") az Azure AD-címtár által kínált.
* **Az Azure ad-vel integrált:** nem kell tooconfigure vagy replikáció kezelésére tooAzure AD tartományi szolgáltatásokban. Felhasználói fiókok, a csoporttagságot és a felhasználói hitelesítő adatokat (jelszó) az Azure AD-címtár automatikusan elérhetők az Azure AD tartományi szolgáltatásokban. Új felhasználók, csoportok vagy Azure AD-bérlőn vagy a helyszíni címtár módosítások tooattributes automatikusan szinkronizált tooAzure AD tartományi szolgáltatásokban.
* **Az NTLM és Kerberos hitelesítés:** NTLM és Kerberos hitelesítés támogatása, a integrált Windows-hitelesítést használó alkalmazásokat helyezhet üzembe.
* **A vállalati hitelesítő adatokat vagy jelszavakat:** jelszavak felhasználók az Azure ad bérlői az Azure AD tartományi szolgáltatások használata. Felhasználók a vállalati hitelesítő adatok toodomain illesztési gépeikhez használja, jelentkezzen be, interaktív vagy a távoli asztali kapcsolaton keresztül és hello által kezelt tartomány hitelesítése.
* **LDAP-kötés & LDAP olvassa el a támogatási szolgálathoz:** alkalmazások LDAP kötések tooauthenticate felhasználók Azure AD tartományi szolgáltatások által kiszolgált tartományokban is használhatja. Emellett az LDAP használó alkalmazások olvasási tooquery felhasználói vagy számítógép-attribútumok hello könyvtárból is dolgozhat Azure AD tartományi szolgáltatások műveleteket.
* **Biztonságos LDAP (LDAPS):** engedélyezheti a hozzáférést toohello directory keresztül biztonságos LDAP (LDAPS). Biztonságos LDAP hozzáférést alapértelmezés szerint a hello virtuális hálózaton belül érhető el. Azonban is engedélyezheti a biztonságos LDAP hozzáférést keresztül hello internet.
* **Csoportházirend:** hello felhasználók és számítógépek használhat egyetlen beépített csoportházirend-objektum minden egyes tárolók tooenforce szabályzatoknak való megfelelőségét szükséges biztonsági felhasználói fiókokhoz és a tartományhoz csatlakoztatott számítógépeken. Is a saját egyéni csoportházirend-objektumok létrehozása, és rendeljen hozzájuk túl toocustom szervezeti egységek[csoportházirendet](active-directory-ds-admin-guide-administer-group-policy.md).
* **DNS kezelése:** hello "AAD DC rendszergazdák" csoportba kezelheti a DNS, a jól ismert DNS felügyelete a felügyelt tartományra eszközök, például hello DNS felügyeleti beépülő MMC-modulban.
* **Hozzon létre egyéni szervezeti egységekhez (OU-k):** hello "AAD DC rendszergazdák" csoportba hozhat létre egyéni szervezeti egységek hello kezelt tartományban. Ezek a felhasználók teljes körű rendszergazdai jogosultságokkal kapnak egyéni szervezeti egységek, keresztül, így azok hozzáadására és eltávolítására szolgáltatásfiókok, számítógépek, csoportok stb. ezeket egyéni szervezeti egységek belül.
* **Több Azure-régiók érhető el:** lásd: hello [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/) lap tooknow hello Azure-régiók, amelyben az Azure AD tartományi szolgáltatások érhető el.
* **Magas rendelkezésre állású:** Azure AD tartományi szolgáltatások kínál a magas rendelkezésre állást a tartományra. Ez a funkció magasabb szolgáltatás üzemideje és rugalmasság toofailures hello garanciát kínál. Beépített állapotfigyelési ajánlatok automatikus szervizelési hibák az új példányok nem sikerült tooreplace példányok és tooprovide forgó által a tartomány szolgáltatás továbbra is.
* **Használja az tisztában kezelőeszközöket:** például az Active Directory felügyeleti központ vagy az Active Directory PowerShell hello tooadminister felügyelt tartományok is használhatja a jól ismert Windows Server Active Directory felügyeleti eszközökkel.
