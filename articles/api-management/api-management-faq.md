---
title: "aaaAzure API Management – gyakori kérdések |} Microsoft Docs"
description: "Ismerje meg, hello megválaszolja toocommon kérdéseket, a mintákat és ajánlott eljárások az Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 9e7cdf1b881a4dfed4bd2cfd7fbb4994f48b5f79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-faqs"></a>Az Azure API Management – gyakori kérdések
Első hello válaszok toocommon kérdéseket, a mintákat és ajánlott eljárások az Azure API Management.

## <a name="contact-us"></a>Kapcsolat
* [Hogyan lehet I kérdés hello Microsoft Azure API Management csapatának?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)


## <a name="frequently-asked-questions"></a>Gyakori kérdések
* [Mit jelent, ha a szolgáltatás egyelőre?](#what-does-it-mean-when-a-feature-is-in-preview)
* [Hogyan biztosíthassa hello API adatkezelési átjáró és a háttér-szolgáltatásaihoz hello kapcsolatot?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
* [Hogyan másolja át a saját API-kezelés szolgáltatás példány tooa új példányt?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
* [Programozott módon is kezelhető az API Management példányt?](#can-i-manage-my-api-management-instance-programmatically)
* [Hogyan hozzá egy felhasználói toohello rendszergazdák csoportot?](#how-do-i-add-a-user-to-the-administrators-group)
* [Miért van, hogy szeretném tooadd nem érhetők el a Helyicsoportházirend-szerkesztő hello hello házirend?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
* [Miként használható az API Management API versioning?](#how-do-i-use-api-versioning-in-api-management)
* [Hogyan állíthatom be több környezeteknek a egyetlen API-val?](#how-do-i-set-up-multiple-environments-in-a-single-api)
* [Használhatok SOAP API Management?](#can-i-use-soap-with-api-management)
* [Hello API Management gateway IP cím állandó van? Képes használni a tűzfalszabályok?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
* [Beállítható az OAuth 2.0 hitelesítési kiszolgáló AD FS biztonsági?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
* [Milyen útválasztási módszert használ az API Management központi telepítések toomultiple földrajzi helyeken található?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
* [Az Azure Resource Manager sablon toocreate API Management service-példány használata](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
* [Használhatók a háttérből az önaláírt SSL-tanúsítvány?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
* [Miért kapok hitelesítési hiba jelenik meg, hogy a GIT-tárház tooclone?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
* [Az API Management működik az Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
* [Miért azt igényelnek erőforrás-kezelő stílusban Vnetek dedikált alhálózat amikor API Management központilag telepítik őket?](#why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them)
* [Mi az, hogy egy VNETET az API Management telepítése során szükséges hello minimális alhálózat méretét?](#what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet)
* [Áthelyezhető egy API-kezelés szolgáltatás a egy előfizetés tooanother?](#can-i-move-an-api-management-service-from-one-subscription-to-another)
* [Korlátozások vagy importálása a API ismert problémái vannak-e?](#are-there-restrictions-on-or-known-issues-with-importing-my-api)

### <a name="how-can-i-ask-hello-microsoft-azure-api-management-team-a-question"></a>Hogyan lehet I kérdés hello Microsoft Azure API Management csapatának?
Akkor is kapcsolatba velünk a következő beállítások egyikét:

* A kérdéseit a [API Management MSDN fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
* Az e-mailek küldése túl<mailto:apimgmt@microsoft.com>.
* A hello szolgáltatás kérelmet küldjön [Azure visszajelzési fórumon](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Mit jelent, ha a szolgáltatás egyelőre?
A szolgáltatás egyelőre, azt jelzi, hogy azt aktívan kérdése megválaszolásában a hello szolgáltatás alakulását az Ön visszajelzését. Egy előzetes szolgáltatás funkcionálisan befejeződött, de előfordulhat, hogy a legfrissebb válasz toocustomer visszajelzés változása lesz biztosítjuk. Azt javasoljuk, hogy nincs-e függ egy szolgáltatás, amely jelenleg előzetes verzióban érhető az éles környezetben. Ha olyan visszajelzést az előzetes verziójú funkciókat, tudassa velünk hello kapcsolattartási beállítások egyikével [hogyan is szeretnék kérdés hello Microsoft Azure API Management csapatának?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-hello-connection-between-hello-api-management-gateway-and-my-back-end-services"></a>Hogyan biztosíthassa hello API adatkezelési átjáró és a háttér-szolgáltatásaihoz hello kapcsolatot?
Van több beállítások toosecure hello kapcsolatot hello API adatkezelési átjáró és a háttér-szolgáltatásaihoz. A következőket teheti:

* Egyszerű HTTP-hitelesítést használjon. További információkért lásd: [konfigurálhatja az API-beállítások](api-management-howto-create-apis.md#configure-api-settings).
* SSL kölcsönös hitelesítést használ, a [hogyan toosecure háttér-szolgáltatásainak ügyféltanúsítvány-alapú hitelesítés használata az Azure API Management](api-management-howto-mutual-certificates.md).
* IP-engedélyezése a háttér-szolgáltatáshoz felhasználhat. Ha a Standard vagy prémium szint API Management példánya, hello átjáró IP-címe hello állandó marad. Az IP-címet az engedélyezési lista tooallow állíthatja be. Az irányítópult hello hello Azure-portálon a kaphat hello IP-címet a API Management-példány.
* Az API Management példány tooan Azure-beli virtuális hálózathoz csatlakozni.

### <a name="how-do-i-copy-my-api-management-service-instance-tooa-new-instance"></a>Hogyan másolja át a saját API-kezelés szolgáltatás példány tooa új példányt?
Erre számos lehetősége van, ha azt szeretné, hogy toocopy egy API-kezelés példány tooa új példányát. A következőket teheti:

* Hello biztonsági mentése és visszaállítása az API Management függvény. További információkért lásd: [hogyan tooimplement vész-helyreállítási segítségével biztonsági mentés szolgáltatást, és állítsa vissza az Azure API Management](api-management-howto-disaster-recovery-backup-restore.md).
* A saját biztonsági mentést, majd állítsa vissza a szolgáltatás hello használatával [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Hello REST API toosave használja, és hello entitások kívánt hello szolgáltatáspéldány szeretné visszaállítani.
* Töltse le a hello szolgáltatás konfigurációja Git használatával, és ezután töltse fel az új példány tooa. További információkért lásd: [hogyan toosave használatával és konfigurálhatja az API Management szolgáltatáskonfiguráció Git](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Programozott módon is kezelhető az API Management példányt?
Igen, akkor az API Management programozott módon segítségével kezelheti:

* Hello [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
* Hello [Microsoft Azure ApiManagement Service Management könyvtár SDK](http://aka.ms/apimsdk).
* Hello [szolgáltatás telepítését](https://msdn.microsoft.com/library/mt619282.aspx) és [szolgáltatásfelügyelet](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell-parancsmagokkal.

### <a name="how-do-i-add-a-user-toohello-administrators-group"></a>Hogyan hozzá egy felhasználói toohello rendszergazdák csoportot?
Itt látható, hogyan adhat egy felhasználói toohello rendszergazdák csoportot:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Nyissa meg, amely rendelkezik hello API Management-példányt, tooupdate toohello erőforráscsoportot.
3. Az API Management, rendelje hozzá a hello **Api Management közreműködői** toohello felhasználói szerepkör.

Most hello újonnan hozzáadott közreműködői használhatja az Azure PowerShell [parancsmagok](https://msdn.microsoft.com/library/mt613507.aspx). Itt hogyan rendszergazdaként a toosign:

1. Használjon hello `Login-AzureRmAccount` a parancsmag toosign.
2. Hello környezetben toohello előfizetés, amely rendelkezik hello szolgáltatás használatával állítsa `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Egyszeri bejelentkezési URL-cím segítségével könnyebben nyerhet `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Hello URL-cím tooaccess hello felügyeleti portál használata.

### <a name="why-is-hello-policy-that-i-want-tooadd-unavailable-in-hello-policy-editor"></a>Miért van, hogy szeretném tooadd nem érhetők el a Helyicsoportházirend-szerkesztő hello hello házirend?
Ha alkalmazni kívánt tooadd megjelenik hello szabályzat szürkén jelenik meg, vagy a Helyicsoportházirend-szerkesztő hello árnyékolt, kell arra, hogy a hello megfelelő hatókörben hello házirend. Minden egyes házirend-utasítás toouse adott hatókörhöz és házirend szakaszok tervezték. tooreview hello házirend szakaszok és egy házirend hatókörök lásd hello házirend használatának szakasz [API-felügyeleti házirendek](https://msdn.microsoft.com/library/azure/dn894080.aspx).

### <a name="how-do-i-use-api-versioning-in-api-management"></a>Miként használható az API Management API versioning?
Az API Management rendelkezik néhány beállítások toouse API versioning:

* Az API Management konfigurálhatja az API-k toorepresent különböző verziói. Például lehetséges, hogy két különböző API-k, MyAPIv1 és MyAPIv2. Egy fejlesztő választható fejlesztői hello hello verziót szeretne toouse.
* Az API-t egy szolgáltatási URL-cím, amely nem tartalmazza a szegmens verziójával, például https://my.api is konfigurálhatja. Ezt követően konfigurálja a verzió szegmens minden műveletnek [URL-cím újraírása](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) sablont. Például lehet egy olyan műveletet egy [URL-cím sablon](api-management-howto-add-operations.md#url-template) nevű /resource és egy [URL-cím újraírása](api-management-howto-add-operations.md#rewrite-url-template) nevű sablon/v1/erőforrás. Módosíthatja a szegmens verzióértéket hello külön-külön az egyes műveletek.
* Ha azt szeretné, hogy tookeep egy "alapértelmezett" verzió szegmens hello API szolgáltatás URL-címében, a kijelölt műveletek hello használó szabály beállításához [be háttérszolgáltatás](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) szabályzat toochange hello háttér-kérelem elérési útja.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Hogyan állíthatom be több környezeteknek a egyetlen API-val?
tooset be több környezetekben, például egy tesztkörnyezetben és éles környezetben, egyetlen API-val, a két lehetőség közül választhat. A következőket teheti:

* Gazdagép másik API-hello ugyanannak a bérlőnek.
* Állomás azonos API-k, a különböző bérlők hello.

### <a name="can-i-use-soap-with-api-management"></a>Használhatok SOAP API Management?
[SOAP-áteresztő](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) a rendszer támogatja. -Rendszergazdák importálni tudják a hello WSDL SOAP szolgáltatása, és az Azure API Management létrehoz egy SOAP-előtér. Fejlesztői portál dokumentációjában, a teszt konzol, a házirendek és a analytics az összes elérhető SOAP-szolgáltatások.

### <a name="is-hello-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Hello API Management gateway IP cím állandó van? Képes használni a tűzfalszabályok?
Hello Standard és Premium rétegek, a hello nyilvános IP-cím (VIP) hello API Management-bérlő statikus hello élettartama hello bérlői, néhány kivétellel. hello IP-cím módosításainak ilyen körülmények között:

* hello szolgáltatás törlődik, és újból létrehozza majd.
* hello szolgáltatás előfizetés [felfüggesztve](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) vagy [figyelmezteti](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) (például nonpayment) és majd érvényességét.
* Adja hozzá, vagy távolítsa el az Azure Virtual Network (használhat virtuális hálózat csak hello fejlesztői és a Premium szint).

Több területi telepítések esetén hello regionális címet érintő módosításainak Ha hello régió vacated és majd érvényességét (használhat több területi telepítési csak hello prémium csomagban).

Prémium szint bérlők több területi telepítési konfigurált rendelt régiónként egy nyilvános IP-címet.

Kaphat az IP-cím (vagy címek, a központi telepítés több területi) lapján hello bérlői hello Azure-portálon.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Beállítható az OAuth 2.0 hitelesítési kiszolgáló AD FS biztonsági?
Hogyan tooconfigure egy OAuth 2.0 hitelesítési kiszolgáló Active Directory összevonási szolgáltatások (AD FS) biztonsági: toolearn [AD FS segítségével az API Management](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-toomultiple-geographic-locations"></a>Milyen útválasztási módszert használ az API Management központi telepítések toomultiple földrajzi helyeken található?
API-kezelési funkciója hello [teljesítmény forgalom-útválasztási módszer](../traffic-manager/traffic-manager-routing-methods.md#priority) központi telepítések toomultiple földrajzi helyeken található. Bejövő forgalom irányított toohello legközelebbi API átjáró. Egy régió tartozik offline állapotba kerül, ha a bejövő forgalom automatikusan irányított toohello következő legközelebbi átjáró. További tudnivalók az útválasztási metódusait [Traffic Manager útválasztási módjai](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-toocreate-an-api-management-service-instance"></a>Az Azure Resource Manager sablon toocreate API Management service-példány használata
Igen. Lásd: hello [Azure API Management Service](http://aka.ms/apimtemplate) gyorsindítási sablonok.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Használhatók a háttérből az önaláírt SSL-tanúsítvány?
Igen. Hogyan toouse egy önaláírt Secure Sockets Layer (SSL) tanúsítványt a háttérből az itt található:

1. Hozzon létre egy [háttér](https://msdn.microsoft.com/library/azure/dn935030.aspx) entitás API Management használatával.
2. Set hello **skipCertificateChainValidation** tulajdonság túl**igaz**.
3. Ha már nem szeretné tooallow önaláírt tanúsítványokat, hello háttér entitás törlése, vagy állítsa a hello **skipCertificateChainValidation** tulajdonság túl**hamis**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-tooclone-a-git-repository"></a>Miért kapok hitelesítési hiba jelenik meg, hogy a Git-tárház tooclone?
Git hitelesítőadat-kezelő használatakor, vagy a Visual Studio használatával tooclone Git-tárház próbált, mutatjuk be egy ismert probléma az hello Windows hitelesítő adatok párbeszédpanel. hello párbeszédpanel korlátozza a jelszó too127 karakter, és csonkolja a hello Microsoft által létrehozott jelszót. Jelenleg is dolgozunk lerövidíteni hello jelszót. Most használja a Git bash eszközt tooclone a Git-tárház.

### <a name="does-api-management-work-with-azure-expressroute"></a>Az API Management működik az Azure ExpressRoute?
Igen. Az API Management Azure ExpressRoute működik.

### <a name="why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them"></a>Miért azt igényelnek erőforrás-kezelő stílusban Vnetek dedikált alhálózat amikor API Management központilag telepítik őket?
az API Management hello dedikált alhálózati követelmény hello, hogy be van építve (PAAS V1 réteg) klasszikus telepítési modell származik. Telepíthetünk egy erőforrás-kezelő virtuális hálózat (V2 réteg) azokat, amíg nincsenek következmények toothat. hello klasszikus telepítési modellt az Azure-ban nem szorosan együtt használja hello Resource Manager-modell, ezért ha V2 réteg hozzon létre egy erőforrást, hello V1 réteg nem ismert, és a probléma akkor fordulhat elő, például az API Management toouse próbált egy IP-cím már le van foglalva a hálózati adapter (V2 épülő) tooa.
További információk az Azure klasszikus és Resource Manager modellek különbség tekintse meg a túl toolearn[különbség az üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md).

### <a name="what-is-hello-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet"></a>Mi az, hogy egy VNETET az API Management telepítése során szükséges hello minimális alhálózat méretét?
hello minimális alhálózat méretét szükséges API-kezelés toodeploy [/29](../virtual-network/virtual-networks-faq.md#configuration), vagyis hello minimális alhálózat méretét, amely támogatja az Azure.

### <a name="can-i-move-an-api-management-service-from-one-subscription-tooanother"></a>Áthelyezhető egy API-kezelés szolgáltatás a egy előfizetés tooanother?
Igen. toolearn című témakörben talál [erőforrások tooa új erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md).

### <a name="are-there-restrictions-on-or-known-issues-with-importing-my-api"></a>Korlátozások vagy importálása a API ismert problémái vannak-e?
[Ismert problémák és korlátozások](api-management-api-import-restrictions.md) nyitott API(Swagger), a WSDL és WADL formázza.
