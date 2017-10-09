---
title: "a magas rendelkezésre állású Azure MFA kiszolgáló aaaConfigure |} Microsoft Docs"
description: "Azure multi-factor Authentication kiszolgáló magas rendelkezésre állást biztosító konfigurációk több példányának telepítése."
services: multi-factor-authentication
keywords: Az Azure MFA
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 8c6b1921c734c7a7273e443b3591fbbb15cd5e6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-for-high-availability"></a>Magas rendelkezésre állásra konfigurálja a Azure multi-factor Authentication kiszolgáló

tooachieve magas rendelkezésre állású biztosít az Azure MFA-kiszolgáló-telepítéssel, toodeploy szüksége több multi-factor Authentication kiszolgálót. A szakasz tájékoztatást nyújt a Tervező elosztott terhelésű tooachieve Azure MFS Server-telepítéséhez, a magas rendelkezésre állású célt.

## <a name="mfa-server-overview"></a>Multi-factor Authentication kiszolgáló – áttekintés

hello Azure MFA kiszolgáló szolgáltatás architektúrája több összetevőből áll, ahogy az ábra a következő hello:

 ![Multi-factor Authentication kiszolgáló architektúrája](./media/mfa-server-high-availability/mfa-ha-architecture.png)

Az MFA kiszolgáló a Windows Server-hello Azure multi-factor Authentication alkalmazás telepítve. hello MFA kiszolgálópéldány aktiválnia kell a multi-factor Authentication szolgáltatás hello Azure toofunction a. Egynél több multi-factor Authentication kiszolgáló lehet a telepített helyszíni.

telepített első MFA-kiszolgáló hello hello a fő multi-factor Authentication kiszolgáló aktiválása után hello Azure MFA szolgáltatás alapértelmezés szerint. hello fő multi-factor Authentication kiszolgáló rendelkezik egy hello PhoneFactor.pfdata adatbázis írható példányával. Multi-factor Authentication Server-példányok telepítéseinél slaves nevezik. hello MFA slaves van hello PhoneFactor.pfdata adatbázis replikált csak olvasható másolata. Multi-factor Authentication kiszolgáló replikálja a távoli eljáráshívás (RPC) használatával információkat. Minden multi-factor Authentication kiszolgálóknak együttesen kell tartományhoz csatlakoztatott vagy önálló tooreplicate információkat.

Többtényezős hitelesítés fő és az alárendelt multi-factor Authentication kiszolgálók kommunikálni hello MFA szolgáltatás Ha kétfaktoros hitelesítést szükség. Például amikor egy felhasználó próbál toogain access tooan alkalmazás kéttényezős hitelesítést igénylő, hello felhasználó először hitelesítése az identitásszolgáltatótól, például az Active Directory (AD).

Az ad-vel a sikeres hitelesítés után hello MFA kiszolgáló a hello MFA szolgáltatással kommunikál. hello MFA kiszolgáló megvárja-e értesítés a multi-factor Authentication szolgáltatás tooallow hello, vagy letiltja a hello felhasználói hozzáférés toohello alkalmazás.

Hello MFA főkiszolgáló offline állapotba kerül, ha hitelesítési események feldolgozása még is, de a módosítások toohello MFA adatbázis igénylő műveletekhez nem dolgozható fel. (Például: hello a felhasználók, önkiszolgáló PIN módosítása, és felhasználói adatok módosítása)

## <a name="deployment"></a>Környezet

Vegye figyelembe a következő fontos pontok az Azure MFA kiszolgáló és a kapcsolódó összetevők terheléselosztási hello.

* **RADIUS szabványos tooachieve magas rendelkezésre állású használatával**. Ha az Azure MFA kiszolgáló RADIUS-kiszolgálóként használ, potenciálisan konfigurálhatja egy multi-factor Authentication kiszolgáló elsődleges RADIUS-hitelesítés célként, és a többi Azure MFA kiszolgáló másodlagos hitelesítés célként. Azonban ez metódus tooachieve magas rendelkezésre állás nem lehet, hogy gyakorlati meg kell várni egy időtúllépési időszak toooccur ha hitelesítés nem sikerül hello elsődleges hitelesítési célpont előtt meg lehet hitelesítése hello másodlagos hitelesítés cél. Hatékonyabb tooload egyenleg hello RADIUS-forgalmat hello RADIUS-ügyfél és a hello RADIUS-kiszolgálók (ebben az esetben az hello Azure MFA kiszolgáló RADIUS-kiszolgálóként működő) között, hogy hello RADIUS-ügyfelek konfigurálhatja azokat mutathat egy URL-címet.
* **Toomanually lépteti elő kell MFA slaves**. Hello fő Azure MFA kiszolgáló offline állapotba kerül, ha hello másodlagos Azure MFA kiszolgáló továbbra is tooprocess többtényezős hitelesítési kéréseket. Azonban addig, amíg a fő multi-factor Authentication kiszolgáló nem érhető el, rendszergazdák nem adja hozzá a felhasználókat vagy többtényezős hitelesítési beállítások módosítása, és felhasználó nem módosíthat hello felhasználói portál használatával. Az MFA alárendelt toohello főkiszolgálói szerepkört előléptetni mindig egy kézi művelet.
* **Az összetevők separability**. Azure MFA-kiszolgáló, amely telepíthető több összetevőt magában foglalja hello hello ugyanazon a Windows Server-példányra vagy különböző-példányokon. Ezen összetevők közé tartozik a felhasználói portál hello mobilalkalmazás webszolgáltatásának és hello az AD FS-adapter (ügynök). Ez separability teszi lehetséges toouse hello webalkalmazás-Proxy toopublish hello felhasználói portál és Mobile App webkiszolgáló a hello szegélyhálózaton. Ilyen konfiguráció toohello hozzáadja a tervezés, ahogy az ábra a következő hello általános biztonságosságát. multi-factor Authentication felhasználói portál hello és Mobile App webkiszolgáló is telepítésére a magas rendelkezésre ÁLLÁSÚ terhelésű konfigurációkban.

   ![MFA kiszolgáló a szegélyhálózaton](./media/mfa-server-high-availability/mfasecurity.png)

* **Egyszeri jelszavas (OTP) SMS (más néven egyirányú SMS) keresztül kapcsolódó munkamenetek hello használatát igényli, ha forgalom terhelésű**. Egyirányú SMS egy fordulnak hello MFA kiszolgáló toosend hello felhasználók szöveges üzenetben egy egyszeri Jelszavas hitelesítés lehetőséget. hello felhasználói hello egyszeri Jelszavas beírja egy parancsablakban toocomplete hello MFA-kérdést. Ha végezze el Azure MFA kiszolgáló, hello ugyanarra a kiszolgálóra hello kezdeti hitelesítési kérés hello kiszolgálók kell, amely egyszeri Jelszavas üdvözlőüzenetére fogad hello felhasználói; Ha egy másik MFA kiszolgáló hello egyszeri Jelszavas választ kap, hello hitelesítési kérdés sikertelen lesz. További információkért lásd: [SMS hozzáadott tooAzure multi-factor Authentication kiszolgáló egy idő jelszavát](https://blogs.technet.microsoft.com/enterprisemobility/2015/03/02/one-time-password-over-sms-added-to-azure-mfa-server).
* **Elosztott terhelésű telepítés hello felhasználói portál és a mobilalkalmazás webszolgáltatása igényel a kapcsolódó munkamenetek**. Ha terheléselosztás hello multi-factor Authentication felhasználói portál és a hello mobilalkalmazás webszolgáltatásának, mindegyik munkamenethez kell a hello toostay ugyanarra a kiszolgálóra.

## <a name="high-availability-deployment"></a>Magas rendelkezésre állású telepítés

hello alábbi ábrán látható egy teljes magas rendelkezésre ÁLLÁSÚ terhelésű végrehajtása az Azure MFA és összetevőit, és az AD FS referenciaként.

 ![Az Azure MFA kiszolgáló magas rendelkezésre ÁLLÁSÚ végrehajtása](./media/mfa-server-high-availability/mfa-ha-deployment.png)

Megjegyzés: hello hello elemét, ennek megfelelően a következő számozott diagram megelőző hello területén.

1. két Azure MFA kiszolgáló (MFA1 és MFA2) hello kiegyensúlyozott (mfaapp.contoso.com) betöltése és konfigurált toouse egy statikus port (4443) tooreplicate hello PhoneFactor.pfdata adatbázis. hello webszolgáltatási SDK telepítve van a hello MFA kiszolgáló tooenable kommunikáció hello AD FS-kiszolgáló a 443-as TCP-porton keresztül. hello MFA kiszolgáló állapot nélküli elosztott terhelésű konfiguráció lett telepítve. Azonban ha toouse OTP SMS keresztül, kell használnia az állapotalapú terheléselosztás.
   ![Az Azure MFA kiszolgáló - alkalmazások kiszolgálói magas rendelkezésre ÁLLÁSÚ](./media/mfa-server-high-availability/mfaapp.png)

   > [!NOTE]
   > RPC dinamikus portot használ, mert nem ajánlott tooopen tűzfalak toohello azon dinamikus portok tartományát, RPC is használhat fel. Ha a tűzfal **közötti** az MFA alkalmazáskiszolgálók, konfigurálnia kell hello MFA kiszolgáló toocommunicate egy statikus port hello replikációs forgalom az alárendelt és fő kiszolgálók között, és nyissa meg portot a tűzfalon. Hozzon létre egy DWORD beállításazonosítót, kényszerítheti a hello statikus port ```HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Positive Networks\PhoneFactor``` nevű ```Pfsvc_ncan_ip_tcp_port``` és érték hello tooan elérhető statikus portra. Kapcsolatok mindig hello az alárendelt multi-factor Authentication kiszolgálók toohello fő által kezdeményezett, hello statikus port csak szükséges hello fő, de mivel előléptetheti az alárendelt toobe hello fő bármikor, célszerű minden multi-factor Authentication kiszolgálón hello statikus port.

2. a két hello felhasználói portál/multi-factor Authentication mobilalkalmazás kiszolgálók (MFA-felfelé-MAS1 és MFA-felfelé-MAS2) az elosztott terhelésű egy **állapotalapú alkalmazások és szolgáltatások** konfigurációs (mfa.contoso.com). A visszaírási, hogy-e a kapcsolódó munkamenetek előfeltétele annak, hogy a terheléselosztási hello multi-factor Authentication felhasználói portál és Mobile App Service.
   ![Az Azure MFA kiszolgáló - a felhasználói portál és Mobile App szolgáltatás magas rendelkezésre ÁLLÁSÚ](./media/mfa-server-high-availability/mfaportal.png)
3. az AD FS kiszolgálófarm hello terhelés eloszlik és toohello interneten keresztül az AD FS proxy terhelésű közzétett hello peremhálózati. Minden egyes ADFS-kiszolgáló használ hello az AD FS ügynök toocommunicate hello Azure multi-factor Authentication kiszolgálók egy elosztott terhelésű URL-címet (mfaapp.contoso.com) használatával a 443-as TCP-porton keresztül.

## <a name="next-steps"></a>Következő lépések

* [Azure MFA kiszolgáló telepítése és konfigurálása](multi-factor-authentication-get-started-server.md)
