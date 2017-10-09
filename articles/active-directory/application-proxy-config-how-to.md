---
title: "Az alkalmazásproxy alkalmazás aaaHow tooconfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy néhány egyszerű lépésben alkalmazásproxy alkalmazás konfigurálása"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c64019098fc124e4fe10b8288830bcd2b7239d3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application"></a>Hogyan tooconfigure az alkalmazásproxy-alkalmazás

Ez a cikk segítséget toounderstand hogyan tooconfigure belül tooexpose az Azure AD alkalmazásproxy kérelmet a helyszíni alkalmazások toohello a felhő.

## <a name="recommended-documents"></a>Ajánlott dokumentumok 

hello kezdeti, valamint az alkalmazásproxy alkalmazások hello felügyeleti portálon keresztül létrehozása toolearn kövesse hello [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Az összekötők konfigurálása, lásd: [alkalmazásproxy engedélyezése az Azure portál hello](active-directory-application-proxy-enable.md).

Tanúsítványok feltöltése és egyéni tartomány használatával kapcsolatos tudnivalókat lásd: [egyéni tartományok az Azure AD alkalmazásproxy használata](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).

## <a name="create-hello-applicationsetting-hello-urls"></a>Hello alkalmazás-beállítás hello URL-címek létrehozása

Ha követi hello hello szükséges lépések [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) és a dokumentáció első hello alkalmazás létrehozásakor hiba lépett fel a hello hiba részletes információkat, és javaslatokat arról, hogyan lásd: toofix hello alkalmazás. A legtöbb hiba üzenetekben javasolt javítás. tooavoid gyakori hibákat, ellenőrizze, hogy:

-   Az engedély toocreate proxyval alkalmazás rendszergazda

-   hello belső URL-címe egyedi:

-   hello külső URL-címe egyedi:

-   hello http vagy https URL-címek kezdődik, és a szöveg végén a "/"

-   hello URL-cím a tartomány nevét, IP-cím nem kell lennie.

hello hibaüzenet jelenik meg hello jobb felső sarokban található hello alkalmazás létrehozásakor. Ehelyett választhatja hello értesítési ikon toosee hello hibaüzenetek.

   ![Értesítési üzenet](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Az összekötők/összekötő csoportok beállítása

Ha az alkalmazás beállítása miatt figyelmeztetést jelenít meg hello összekötők és összekötő csoportok probléma, lásd: utasításokat a alkalmazásproxy engedélyezése kapcsolatos részletes tudnivalókért toodownload összekötők. Ha azt szeretné, hogy további információk az összekötők toolearn, tekintse meg a hello [összekötők dokumentációja](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).

Ha az összekötők nem működnek, ez azt jelenti, hogy azok nem tooreach hello szolgáltatást. Ez gyakran azért van, mert minden szükséges hello port nincs nyitva. toosee szükséges portok listáját a alkalmazásproxy dokumentáció engedélyezése hello hello Előfeltételek című szakaszában talál.

## <a name="upload-certificates-for-custom-domains"></a>Az egyéni tartományok tanúsítványok feltöltése

Egyéni tartományok toospecify hello tartományának a külső URL-címek engedélyezése. egyéni tartományok toouse, tanúsítványra van szükség tooupload hello ehhez a tartományhoz. Egyéni tartományok és tanúsítványok használatával kapcsolatos információkért lásd: [egyéni tartományok az Azure AD alkalmazásproxy használata](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains). 

Ha a tanúsítvány feltöltése problémák adódtak, keresse meg hello hibaüzenetek hello portálon kapcsolatos további információkért hello hello tanúsítványával kapcsolatos problémára. Közös tanúsítvánnyal kapcsolatos problémák a következők:

-   A lejárt tanúsítvány

-   Önaláírt tanúsítvány

-   Tanúsítvány hiányzik a hello titkos kulcs

hello hibaüzenet jelenik meg a hello jobb felső sarokban tooupload hello tanúsítványt próbálja ki. Ehelyett választhatja hello értesítési ikon toosee hello hibaüzenetek.

   ![Értesítési üzenet](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a>Következő lépések
[Az Azure AD-alkalmazásproxy használó alkalmazások közzététele](application-proxy-publish-azure-portal.md)
