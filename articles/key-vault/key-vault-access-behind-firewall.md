---
title: "A Key Vault elérése tűzfal mögül | Microsoft Docs"
description: "Megtudhatja, hogyan lehet elérni a tűzfal mögötti Azure Key Vaultot egy alkalmazásból"
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
ms.openlocfilehash: ad31e869d998d29d403ff97c17150c5078ce856d
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/17/2018
---
# <a name="access-azure-key-vault-behind-a-firewall"></a>Az Azure Key Vault elérése tűzfal mögött
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-ports-hosts-or-ip-addresses-should-i-open-to-enable-access-to-a-key-vault"></a>K: A kulcstároló ügyfélalkalmazásomnak tűzfal mögött kell lennie. Milyen portokat, állomásokat vagy IP-címeket kell megnyitnom a kulcstárolóhoz való hozzáférés engedélyezéséhez?
A kulcstároló eléréséhez a kulcstároló-ügyfélalkalmazásnak a különféle funkciók biztosításához képesnek kell lennie több végpont elérésére:

* Hitelesítés az Azure Active Directory (Azure AD) használatával.
* Az Azure Key Vault kezelése. Ide tartozik a hozzáférési szabályzatok létrehozása, olvasása, frissítése, törlése és beállítása az Azure Resource Manageren keresztül.
* A magában a Key Vaultban tárolt objektumok (kulcsok és titkos kulcsok) elérése és kezelése a Key Vault speciális végpontján (pl. [https://sajattaroloneve.vault.azure.net](https://yourvaultname.vault.azure.net)) keresztül.  

A konfigurációtól és a környezettől függően lehetnek bizonyos eltérések.   

## <a name="ports"></a>Portok
Mindhárom funkció (a hitelesítés, a felügyelet és az adatsíkhoz való hozzáférés) Key Vault felé irányuló összes forgalma a 443-as HTTPS-porton keresztül zajlik. A CRL használata esetén azonban alkalmanként HTTP-forgalom is előfordul (a 80-as porton keresztül). Az OCSP protokollhoz hozzáférő ügyfeleknek nem szabad elérniük a CRL-t, azonban alkalmanként elérhetik [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl) címet.  

## <a name="authentication"></a>Hitelesítés
A Key Vault-ügyfélalkalmazásoknak a hitelesítés érdekében hozzá kell férniük az Azure Active Directory-végpontokhoz. A használt végpont függ az Azure AD bérlői konfigurációjától, valamint a név típusától (felhasználói név, szolgáltatásnév), illetve a fiók típusától (pl. Microsoft-fiók vagy munkahelyi/iskolai fiók).  

| Résztvevő típusa | Végpont:port |
| --- | --- |
| A Microsoft-fiókot használó felhasználó<br> (például: user@hotmail.com) |**Globálisan:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Amerikai Egyesült Államok kormánya által használt Azure:**<br> login.microsoftonline.us:443<br><br>**Azure Germany:**<br> login.microsoftonline.de:443<br><br> és <br>login.live.com:443 |
| Az Azure AD-vel munkahelyi vagy iskolai fiókot használó felhasználó vagy szolgáltatás (például user@contoso.com) |**Globálisan:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Amerikai Egyesült Államok kormánya által használt Azure:**<br> login.microsoftonline.us:443<br><br>**Azure Germany:**<br> login.microsoftonline.de:443 |
| Munkahelyi vagy iskolai fiókot és az Active Directory Federation Servicest (AD FS) vagy más összevont végpontot (például: user@contoso.com) használó felhasználó vagy szolgáltatás. |A munkahelyi vagy iskolai fiókhoz tartozó valamennyi végpont plusz az AD FS vagy más összevont végpontok |

Más összetett forgatókönyvek is előfordulhatnak. További információkért tekintse meg az [Azure Active Directory hitelesítési folyamatát](/documentation/articles/active-directory-authentication-scenarios/), az [alkalmazások Azure Active Directoryval való integrálását](/documentation/articles/active-directory-integrating-applications/) és [az Active Directory hitelesítési protokolljait ismertető cikket](https://msdn.microsoft.com/library/azure/dn151124.aspx).  

## <a name="key-vault-management"></a>A Key Vault felügyelete
A Key Vault felügyeletéhez (CRUD és hozzáférési házirend beállítása) a Key Vault-ügyfélalkalmazásnak el kell érnie az Azure Resource Manager-végpontot.  

| Művelet típusa | Végpont:port |
| --- | --- |
| A Key Vault vezérlési síkjával végzett műveletek<br> az Azure Resource Manager használatával |**Globálisan:**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Amerikai Egyesült Államok kormánya által használt Azure:**<br> management.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> management.microsoftazure.de:443 |
| Azure Active Directory – Graph API |**Globálisan:**<br> graph.windows.net:443<br><br> **Azure China:**<br> graph.chinacloudapi.cn:443<br><br> **Amerikai Egyesült Államok kormánya által használt Azure:**<br> graph.windows.net:443<br><br> **Azure Germany:**<br> graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Key Vault-műveletek
Az összes Key Vault-objektummal (kulcsok és titkos kulcsok) végzett felügyeleti és titkosítási művelethez a Key Vault-ügyfélnek el kell érnie a Key Vault-végpontot. A végpont DNS-utótagja a Key Vault helyétől függően eltérő. A Key Vault-végpont formátuma az alábbi táblázatban látható módon: *<tároló-neve>*.*<területspecifikus-dns-utótag>*.  

| Művelet típusa | Végpont:port |
| --- | --- |
| A kulcsokon végzett műveletek, beleértve a titkosítási műveleteket is; kulcsok és titkos létrehozása, olvasása, frissítése és törlése; címkék és egyéb attribútumok beállítása és olvasása kulcstároló-objektumokon (kulcsokon vagy titkokon) |**Globálisan:**<br> &lt;tároló-neve&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;tároló-neve&gt;.vault.azure.cn:443<br><br> **Amerikai Egyesült Államok kormánya által használt Azure:**<br> &lt;tároló-neve&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> &lt;tároló-neve&gt;.vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-címtartományok
A Key Vault szolgáltatás egyéb Azure-erőforrásokat is használ, amilyen például a PaaS-infrastruktúra. Éppen ezért nem lehetséges megadni IP-címek meghatározott tartományát, amellyel a Key Vault szolgáltatás végpontjai egy adott időpontban rendelkeznek. Ha a tűzfal csak az IP-címtartományokat támogatja, akkor tekintse meg a [Microsoft Azure adatközpont IP-címtartományait bemutató cikket](https://www.microsoft.com/download/details.aspx?id=41653). A hitelesítéshez és az identitásszolgáltatáshoz (Azure Active Directory) az alkalmazásnak képesnek kell lennie kapcsolódni a [hitelesítési és identitáscímeket ismertető cikkben](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) leírt végpontokhoz.

## <a name="next-steps"></a>További lépések
Amennyiben a Key Vault szolgáltatással kapcsolatban kérdése merülne fel, tekintse meg az [Azure Key Vault fórumait](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

