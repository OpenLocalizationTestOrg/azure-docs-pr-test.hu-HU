---
title: "a tűzfal mögött Key Vault aaaAccess |} Microsoft Docs"
description: "Ismerje meg, hogyan tooaccess Azure kulcsot tároló tűzfal mögött található alkalmazásokból"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 50d21774-2ee1-4212-8995-570c9de603c5
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: ab4bb0c27a41fef878a20dace6cab203df04e579
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-azure-key-vault-behind-a-firewall"></a>Az Azure Key Vault elérése tűzfal mögött
### <a name="q-my-key-vault-client-application-needs-toobe-behind-a-firewall-what-ports-hosts-or-ip-addresses-should-i-open-tooenable-access-tooa-key-vault"></a>K: a kulcstartót ügyfélalkalmazás toobe tűzfal mögött van szüksége. Milyen portokat, a gazdagépek és az IP-címek megnyitok kell tooenable hozzáférés tooa kulcstároló?
tooaccess kulcstároló, a kulcstároló ügyfélalkalmazás rendelkezik tooaccess több végpontot a különböző funkciók:

* Hitelesítés az Azure Active Directory (Azure AD) használatával.
* Az Azure Key Vault kezelése. Ide tartozik a hozzáférési szabályzatok létrehozása, olvasása, frissítése, törlése és beállítása az Azure Resource Manageren keresztül.
* Elérése és kezelése (kulcsok és titkos) tárolt objektumok a Key Vault magát, áthaladás hello Key Vault-specifikus végpont (például [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

A konfigurációtól és a környezettől függően lehetnek bizonyos eltérések.   

## <a name="ports"></a>Portok
HTTPS-KAPCSOLATON keresztül kerül az összes forgalom tooa kulcstároló összes három függvények (hitelesítés, felügyelete és az adatok vezérlősík access): 443-as porton. A CRL használata esetén azonban alkalmanként HTTP-forgalom is előfordul (a 80-as porton keresztül). Az OCSP protokollhoz hozzáférő ügyfeleknek nem szabad elérniük a CRL-t, azonban alkalmanként elérhetik [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl) címet.  

## <a name="authentication"></a>Authentication
Kulcstároló ügyfélalkalmazások tooaccess Azure Active Directory-végpontokat kell a hitelesítéshez. hello Azure AD bérlő konfigurációjához függ használt hello végpontnak, hello típusú egyszerű (egyszerű vagy egyszerű szolgáltatásneve), és hello írja be a fiók – például a Microsoft-fiók vagy a munkahelyi vagy iskolai fiókkal.  

| Résztvevő típusa | Végpont:port |
| --- | --- |
| A Microsoft-fiókot használó felhasználó<br> (például user@hotmail.com) |**Globálisan:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Amerikai Egyesült Államok kormánya által használt Azure:**<br> login-us.microsoftonline.com:443<br><br>**Azure Germany:**<br> login.microsoftonline.de:443<br><br> és <br>login.live.com:443 |
| Felhasználó vagy a munkahelyi vagy iskolai fiókkal az Azure AD szolgáltatás egyszerű (például user@contoso.com) |**Globálisan:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Amerikai Egyesült Államok kormánya által használt Azure:**<br> login-us.microsoftonline.com:443<br><br>**Azure Germany:**<br> login.microsoftonline.de:443 |
| Felhasználó vagy a munkahelyi vagy iskolai fiókját, és az Active Directory összevonási szolgáltatások (AD FS) vagy más összevont végpont használatával egyszerű szolgáltatásnév (például user@contoso.com) |A munkahelyi vagy iskolai fiókhoz tartozó valamennyi végpont plusz az AD FS vagy más összevont végpontok |

Más összetett forgatókönyvek is előfordulhatnak. Tekintse meg a túl[Azure Active Directory hitelesítési Flow](/documentation/articles/active-directory-authentication-scenarios/), [alkalmazások integrálása az Azure Active Directoryval](/documentation/articles/active-directory-integrating-applications/), és [Active Directory hitelesítési protokolljai](https://msdn.microsoft.com/library/azure/dn151124.aspx) További információt.  

## <a name="key-vault-management"></a>A Key Vault felügyelete
A Key Vault-kezelés (CRUD és a hozzáférési házirend beállítása) hello kulcstároló ügyfélalkalmazás kell tooaccess Azure Resource Manager-végpont.  

| Művelet típusa | Végpont:port |
| --- | --- |
| A Key Vault vezérlési síkjával végzett műveletek<br> az Azure Resource Manager használatával |**Globálisan:**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Amerikai Egyesült Államok kormánya által használt Azure:**<br> management.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> management.microsoftazure.de:443 |
| Azure Active Directory – Graph API |**Globálisan:**<br> graph.windows.net:443<br><br> **Azure China:**<br> graph.chinacloudapi.cn:443<br><br> **Amerikai Egyesült Államok kormánya által használt Azure:**<br> graph.windows.net:443<br><br> **Azure Germany:**<br> graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Key Vault-műveletek
Az összes kulcstároló objektum (kulcsok és titkos) felügyeleti és titkosítási műveleteket hello kulcstároló ügyfél kell tooaccess hello kulcstároló végpont. hello végpont DNS-utótag hello helyét a kulcstartót függ. hello kulcstároló-végpont esetében hello formátum *tároló-neve*. *terület-specifikus-dns-utótag*hello a következő táblázatban leírtak szerint.  

| Művelet típusa | Végpont:port |
| --- | --- |
| A kulcsokon végzett műveletek, beleértve a titkosítási műveleteket is; kulcsok és titkos létrehozása, olvasása, frissítése és törlése; címkék és egyéb attribútumok beállítása és olvasása kulcstároló-objektumokon (kulcsokon vagy titkokon) |**Globálisan:**<br> &lt;tároló-neve&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;tároló-neve&gt;.vault.azure.cn:443<br><br> **Amerikai Egyesült Államok kormánya által használt Azure:**<br> &lt;tároló-neve&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> &lt;tároló-neve&gt;.vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-címtartományok
hello Key Vault szolgáltatás más Azure-erőforrások például PaaS infrastruktúrát használ. Ezért nem lehetséges tooprovide egy adott tartomány IP-címek a Key Vault szolgáltatás végpontjait kell egy adott időpontban. Ha a tűzfal csak az IP-címtartományok használatát támogatja, olvassa el a toohello [Microsoft Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653) dokumentum. A hitelesítés és identitás (az Azure Active Directory), az alkalmazás képes tooconnect toohello végpontok ismertetett kell [hitelesítés és identitás címek](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Következő lépések
Ha kérdései a Key Vault, látogasson el a hello [Azure Key Vault fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

