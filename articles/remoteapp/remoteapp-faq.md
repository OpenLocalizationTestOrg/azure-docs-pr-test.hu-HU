---
title: "aaaAzure RemoteApp – gyakori kérdések |} Microsoft Docs"
description: "Tudnivalók a válaszok toohello legtöbb kapcsolatos gyakori kérdések az Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: swadhwa
editor: 
ms.assetid: bad66603-91f9-437f-8a70-236405d2a27f
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: da981fea9e38b4e74694aeaba5f97c8ed897ccd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp-faq"></a>Azure RemoteApp – gyakori kérdések
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Hallottuk az Azure RemoteApp kapcsolatos kérdések a következő hello. További kérdései vannak? A Microsoft hello [RemoteApp-fórumokra](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) és tudassa velünk, milyen tooknow kell, vagy szóljon hozzá alább.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Nem találja, amit keres? Kérdése van, amire nem kapott választ?
Ha nem találja a hello információt kell, vagy rendelkezik, amely jelenleg még nem kiterjedő Itt további kérdés, lépjen a toohello [Azure RemoteApp fórumán](http://aka.ms/araforum) , és kérje meg a a kérdése van. Itt bármikor közzétehetünk új válaszokat.

## <a name="what-is-azure-remoteapp"></a>Mi az az Azure RemoteApp?
* **Mi az az Azure RemoteApp?** RemoteApp, az Azure-szolgáltatások segítségével biztosítható a biztonságos távoli elérést tooapplications számos különböző felhasználói eszközről. Tudjon meg többet az [Azure RemoteApp](remoteapp-whatis.md) szolgáltatásról.
* **Mik a hello telepítési lehetőségei?** Kétféle RemoteApp-gyűjtemény létezik: felhőalapú és hibrid. Az, hogy Önnek melyikre van szüksége, több tényezőtől is függ, például attól, hogy szükség van-e tartományhoz csatlakozásra. A döntésről bővebben [itt](remoteapp-collections.md) olvashat.

## <a name="quick-tips-on-using-azure-remoteapp"></a>Gyors tippek az Azure RemoteApp használatához
* **Mennyi időm van a kapcsolat megszakításáig? Mennyi ideig lehetek tétlen hello rendszerindító átadása me előtt?** 4 órán át. Ha Ön vagy egy felhasználója 4 órán keresztül tétlen marad, az Azure RemoteApp automatikusan kilépteti. Kivételt hello további alapértelmezett beállításokat itt [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).
* **Kipróbálhatom a szolgáltatást ingyenesen?** Igen. Létezik egy ingyenes próbaverzió, amely 30 napig érhető el. Hello próbaverzió lejárta után szüntesse meg tooa fizetős fiókra (amelyet éles környezetben használhat) vagy hello szolgáltatás leállítása. Az ingyenes próbaverzió elindításához túl címen[portal.azure.com](http://portal.azure.com) -RemoteApp új példányának létrehozásakor. Hello ingyenes próbaverziót példányonként 10 felhasználóval 2 RemoteApp-példányt hozhat létre. Ne feledje, hogy a próbaverzió csak 30 napig használható.
  
  ## <a name="azure-remoteapp-subscription-details"></a>Az Azure RemoteApp-előfizetés részletei
* **Hello szolgáltatás korlátai** Tudnivalók a hello alapértelmezett beállításait, és szolgáltatási korlátait Azure RemoteApp [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md). Ha további kérdései vannak, ossza meg velünk.
* **Hány felhasználó tegye toohave van?** A felhasználók minimális száma 20. Csak az egyértelműség kedvéért adott toobe – hello legalább 20. A számlázás 20 felhasználó után történik. 
* **Mennyibe kerül a RemoteApp szolgáltatás?** Tekintse meg az [Azure RemoteApp díjszabásának részleteit](https://azure.microsoft.com/pricing/details/remoteapp/).
* **Van olyan gyűjteménytípus, amely drágább, mint a többi?** Igen, előfordulhat, hogy van. Ez a gyűjteménnyel szemben támasztott követelményektől függ. Hibrid gyűjtemény az Azure RemoteApp tooyour a helyi hálózaton keresztüli kapcsolat szükséges. Ha meglévő VNET-/Express Route-kapcsolatokat használ, ez nem jelent többletköltséget. De ha egy új Azure VNET és egy átjárót vagy Express Route használ, akkor fogjuk számlázni hello [VPN-átjáró](https://azure.microsoft.com/pricing/details/vpn-gateway) vagy [Express Route](https://azure.microsoft.com/pricing/details/expressroute/). Ez a költség (amelynek részleteit a hivatkozások hello) felett a havi Azure RemoteApp költséges.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Gyűjtemények – mi támogatott, mit érdemes használni és egyebek
* **Támogatottak az egyéni üzletági (LOB) alkalmazások?** Igen. egy egyéni alkalmazást az Azure Remoteappban toouse hozzon létre egy [egyéni sablonrendszerképet](remoteapp-create-custom-image.md), majd töltse fel toohello a RemoteApp-gyűjteményt.
* **Egyéni ÜZLETÁGI alkalmazásom az Azure Remoteappban működnek?**  legjobb módja toofigure, tootest hello azt. Tekintse meg a hello [távoli asztali kompatibilitási központjába](http://www.rdcompatibility.com/compatibility/default.aspx).
* **Melyik üzembe helyezési módszer (felhőalapú vagy hibrid) a legjobb a szervezetem számára?** A hibrid gyűjtemények nyújtják a legteljesebb körű élményt hello, ha azt szeretné, hogy a szolgáltatás teljesen integrálható az egyszeri bejelentkezés (SSO) és biztonságos helyszíni hálózati kapcsolatot. Felhőalapú gyűjtemények adjon meg egy gyors és egyszerű módot tooisolate a példányt több hitelesítési módszer használatával. További információk a hello [központi telepítési beállítások](remoteapp-whatis.md).
* **Van egy SQL- vagy más adatbázisunk a helyszínen vagy az Azure-ban. Melyik üzembe helyezési típust érdemes használnunk?** Ez attól függ, hogy az SQL- vagy háttéradatbázis hol található. Ha hello adatbázis egy magánhálózaton, hello hibrid gyűjtemény használható. Ha hello adatbázis kitett toohello Internet, és lehetővé teszi az ügyfél kapcsolatok tooconnect tooit, hello felhőalapú gyűjtemény használható.
* **Mi a helyzet a meghajtó-hozzárendeléssel, az USB- és a soros portokkal, a vágólapmegosztással és a nyomtatóátirányítással?** Az Azure RemoteApp ezek mindegyikét támogatja. A vágólapmegosztás és a nyomtatóátirányítás alapértelmezés szerint engedélyezve van. Az átirányításról [itt](remoteapp-redirection.md) tudhat meg többet. 

## <a name="template-images"></a>Sablonrendszerképek
* **Használhatom a felhő vagy a meglévő virtuális gép hello sablonként a RemoteApp-gyűjteményemhez?** Igen! Hozzon létre egy Azure virtuális Gépen alapuló rendszerképet, hello rendszerképeket az előfizetéskor valamelyikével, vagy létrehozhat egyéni rendszerképeket. Tekintse meg a hello [RemoteApp-rendszerképekkel kapcsolatos lehetőségek](remoteapp-imageoptions.md).

## <a name="network-options"></a>Hálózati lehetőségek
* **hello hibrid gyűjtemény egy virtuális hálózat szükséges. Használhatjuk a meglévő VNET-kapcsolatunkat?** Ha hello meglévő VNET egy Azure virtuális Hálózatot is. Lásd: "1. lépés: a virtuális hálózat beállítása" hello a [hibrid gyűjteményekkel kapcsolatos útmutatás](remoteapp-create-hybrid-deployment.md) további információt.
* **Használhatok VNET-kapcsolatot felhőalapú gyűjtemény esetében?** Természetesen igen. További információért lásd: [Felhőalapú gyűjtemény létrehozása](remoteapp-create-cloud-deployment.md), különösen az 1. lépést.

## <a name="authentication-options"></a>Hitelesítési lehetőségek
* **Mi a helyzet a hitelesítéssel? Milyen módszerek támogatottak?**  hello felhőalapú gyűjtemény támogatja a Microsoft-fiókok és az Azure Active Directory-fiókokat, amelyek Office 365-fiókok is. hello hibrid gyűjtemény csak Azure Active Directory-fiókok szinkronizált támogat (hasonló eszköz használatával [Azure Active Directory szinkronizáló](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) egy Windows Server Active Directory-telepítésből; pontosabban vagy szinkronizálása megtörtént-e hello Jelszó-szinkronizálási beállítás vagy az Active Directory összevonási szolgáltatások (AD FS) összevonásának konfigurálásával szinkronizálása megtörtént. Ezenkívül konfigurálhatja a [Multi-Factor Authentication (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/) szolgáltatást is.

> [!NOTE]
> hello Azure Active Directory-felhasználók hello bérlői előfizetéshez tartozó kell lennie. (Megtekintheti és módosíthatja az előfizetését hello **beállítások** hello portálon lapon. Lásd: [módosítás hello Azure Active Directory-bérlő RemoteApp által használt](remoteapp-changetenant.md) további információt.)
> 
> 

* **Miért nem adhat meg a saját Azure Active Directory-fiók hozzáférési?**  hello Azure Active Directory-felhasználók az előfizetéshez társított hello könyvtárból kell lennie. Megtekintheti vagy könyvtárhoz hello portálon hello beállítások lapon módosíthatja. Lásd: [módosítás hello Azure Active Directory-bérlő RemoteApp által használt](remoteapp-changetenant.md) további információt.

## <a name="clients---what-device-can-i-use-tooaccess-azure-remoteapp"></a>Ügyfelek - milyen eszközhöz is tooaccess Azure RemoteApp használata?
Ügyfél információt, beleértve a különböző ügyfelek hello telepítésének lépéseit [fér hozzá az alkalmazásokban az Azure Remoteappban](remoteapp-clients.md).

* **Eszközök és operációs rendszerek hello ügyfélalkalmazások támogatják?**
  Első, hello számítógépek és táblagépek: 
  
  * Windows 10 (ügyfél előzetes kiadása)
  * Windows 8.1 és Windows 8
  * Windows 7 Service Pack 1
  * Mac OS X
  * Windows RT
  * Android rendszerű táblagépek
  * iPadek

    És hello telefonok:
  * iPhone
  * Android rendszerű telefonok
  * Windows Phone
    
    [Töltsön le](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) egy RemoteApp-ügyfelet most.
* **Támogatja az Azure RemoteApp a vékony ügyfeleket?** Igen, hello következő Windows Embedded vékony ügyfelek támogatottak:
  
  * Windows Embedded Standard 7
  * Windows Embedded 8 Standard
  * Windows Embedded 8.1 Industry Pro
  * Windows 10 IoT Enterprise
* **A Windows Server melyik verziója esetén támogatott hello távoli asztali munkamenetgazda (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Támogatás és visszajelzés
* **Mi az a RemoteApp támogatási csomagjában hello?** A számlázási és előfizetés-kezelési támogatás ingyenesen elérhető. Technikai támogatás érhető el hello [Azure támogatási csomagok](https://azure.microsoft.com/support/plans/). Emellett [Azure vitafórumunkban](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp) ingyenes közösségi támogatáshoz juthat. 
* **Hogyan küldhetek visszajelzést?** A Microsoft hello [visszajelzési fórumon](https://feedback.azure.com/forums/247748-azure-remoteapp/).
* **Ki szeretnék működik Azure RemoteApp szolgáltatással kapcsolatban további toolearn?** Továbbá tooour a [vitafóruma](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), amely egy remek toopost kérdése, hetente csatlakozhat hello [kérje meg a hello szakértői válaszok webes szemináriumhoz](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), ahol mindenről szó van a RemoteApp kapcsolatos.
* **Mi a helyzet a RemoteApp dokumentációjával?** Örülünk, hogy megkérdezte. Ezenkívül toohello súgótartalom hello portál súgófiókjában fiókban (hello kattintson **?** bármely hello portál oldalán), a következő cikkek hello elérhető tooteach, mind a RemoteApp szolgáltatással kapcsolatban:
  
  * **Első lépések:**
    * [Mi az a RemoteApp?](remoteapp-whatis.md)
    * [Mi az az hello RemoteApp sablon rendszerképei?](remoteapp-images.md)
    * [Hogyan működik a licenckezelés?](remoteapp-licensing.md)
    * [Hogyan működik együtt a RemoteApp és az Office?](remoteapp-o365.md)
    * [Hogyan működik az átirányítás a RemoteApp szolgáltatásban](remoteapp-redirection.md)?
  * **Üzembe helyezés:**
    * [Egyéni sablonrendszerkép létrehozása](remoteapp-create-custom-image.md)
    * [Hibrid gyűjtemény létrehozása](remoteapp-create-hybrid-deployment.md)
    * [Felhőalapú gyűjtemény létrehozása](remoteapp-create-cloud-deployment.md)
    * [Az Azure Active Directory konfigurálása a RemoteApp szolgáltatáshoz](remoteapp-ad.md)
    * [Alkalmazás közzététele a RemoteApp szolgáltatásban](remoteapp-publish.md)
  * **Kezelés:**
    
    * [Felhasználók hozzáadása](remoteapp-user.md)
    * [Ajánlott eljárások a RemoteApp konfigurálásához és használatához](remoteapp-bestpractices.md)    
    
    Videók: A RemoteApp szolgáltatásról számos videónk érhető el. Néhány bevezetést nyújtanak a ([bemutatása tooAzure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)), míg mások az üzembe helyezésben ([felhőalapú üzemelő példány](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) és [hibrid telepítés](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Nézze meg őket!

### <a name="help-us-help-you"></a>Segítsen nekünk, hogy segítsünk
Tudta, hogy a hozzáadása toorating ebben a cikkben és a hozzászólások írása az alábbi tehet módosítások toohello cikkbe? Valami hiányzik? Valami nem működik? Írtam valami olyat, ami nem egyértelmű? Görgessen fel, és kattintson a **Szerkesztés a Githubon** toomake módosítások - ezek hozzánk kerülnek jóváhagyásra toous, és majd, miután jóváhagytuk őket, itt fognak megjelenni a módosításokat és fejlesztéseket azonnal itt.

