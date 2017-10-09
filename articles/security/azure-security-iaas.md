---
title: "IaaS munkaterhelések az Azure-ban aaaSecurity ajánlott eljárások |} Microsoft Docs"
description: " IaaS munkaterhelések tooAzure hello áttelepítésének számos lehetőséget kínál lehetőségek tooreevaluate a tervek "
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: barclayn
ms.openlocfilehash: 9cee1ca6effe9561e51dc8b945e7388ffea169b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Ajánlott biztonsági eljárások az Azure IaaS munkaterhelések

Továbbléphetnek áthelyezése munkaterhelések tooAzure infrastruktúra (IaaS) szolgáltatás használatba, akkor valószínűleg is tudjuk, hogy tisztában legyenek-e azt is számításba kell. Előfordulhat, hogy már nincs a virtuális környezetek biztonságossá tétele. Amikor tooAzure IaaS helyezi át, a virtuális környezetek biztonságossá tétele a szakértelem alkalmazása, és biztonságos beállítások toohelp új készletének használata az eszközök.

Kezdjük, hogy nem várható toobring helyszíni erőforrásokat, egy az egyhez típusú tooAzure kimondásával. hello új kihívásokat és hello új beállítások kapcsolása lehetőség tooreevaluate meglévő deigns, eszközök és dolgozza fel.

A biztonsági felelősséget hello típusú felhőalapú szolgáltatás alapul. hello következő diagram foglalja a Microsoft és az Ön felelőssége hello egyenleg:


![A felelősségi területeket](./media/azure-security-iaas/sec-cloudstack-new.png)


Mutatjuk érhető el, amelyek segítségével a szervezet biztonsági követelményeinek Azure hello-beállítások egy része. Ne feledje, hogy a biztonsági követelmények eltérőek lehetnek, munkaterhelések különböző típusú. Az alábbi gyakorlati tanácsok nem egyik önmagában biztonságossá teheti a számítógépeken. Bármi más, a biztonsági, például toochoose hello megfelelő lehetősége van, és tekintse meg, hogyan hello megoldások is kiegészíteni egymással hézagok kitöltésével.

## <a name="use-privileged-access-workstations"></a>Szintű hozzáféréssel rendelkező munkaállomások használata

A szervezetek gyakran tartoznak kihasználják toocyberattacks mert rendszergazdák műveleteket emelt szintű jogosultságokkal rendelkező fiókok használata során. Általában ez nem történik ártó, de mivel a meglévő konfiguráció és folyamatok engedélyezi-e. Ezek a felhasználók a legtöbb megismeréséhez ezeket a műveleteket fogalmi szempontból hello kockázatát, de továbbra is a toodo válassza ki azokat.

Például az e-mailek ellenőrzése, és keresse meg az Internet hello látszólag elég tűnve történt. De esetleg felfedi a emelt szintű fiókok toocompromise böngészési tevékenységek, kifejezetten kialakított e-maileket és egyéb technikák toogain hozzáférés tooyour vállalati használó rosszindulatú szereplője által. Erősen ajánlott minden Azure adminisztrációs feladatokat, mint egy lerövidíthető kitettség tooaccidental biztonsági sérülés végző hello biztonságos felügyeleti munkaállomás használatát.

Jogosultsági szintű hozzáféréssel rendelkező munkaállomások (láb) egy dedikált operációs rendszer bizalmas feladatokhoz – egyet, amely védett az Internet támadások és fenyegetési vektoroknak enged utat adja meg. A bizalmas feladatok és a fiókok elválasztó napi használható munkaállomások hello és eszközök erős védelmet nyújt az adathalász támadások, alkalmazás és az operációs rendszer biztonsági rések, különböző megszemélyesítési támadásokkal szemben, és hitelesítő adatokkal való visszaéléseket támadások ellen, mint a billentyűlenyomást a Pass-the-Hash és Pass-the-Ticket naplózása.

hello PAW megközelítés neves hello kiterjesztése, és ajánlott eljárás toouse külön-külön hozzárendelt rendszergazdai fiókkal, amely nem egy általános jogú felhasználói fiók. Egy PAW megbízható munkaállomás olyan kényes fiókok számára biztosít.

További információk és megvalósítási útmutatóért lásd: [jogosultsági szintű hozzáféréssel rendelkező munkaállomások](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations).

## <a name="use-multi-factor-authentication"></a>Többtényezős hitelesítés használata

Hello korábbi a hálózati szegélyhálózati volt használt toocontrol access toocorporate adatokat. A felhő-first, a mobileszköz-első világ identitás hello vezérlő vezérlősík: használatba vétel toocontrol tooIaaS szolgáltatást bármilyen eszközről. Akkor is használhatja tooget látható, és betekintést hol és hogyan az adatok használatban van. Digitális identitásokat hello Azure felhasználó védelme nem hello legfontosabb feladatai közé tartoznak az előfizetések származó adatokkal való visszaéléshez és egyéb cybercrimes védelme érdekében.

Hello legtöbb előnyös menetéről, hogy toosecure fiók egyik tooenable kéttényezős hitelesítéssel. Kéttényezős hitelesítés módja a hitelesítő valamit a hozzáadása tooa jelszó használatával. Ennek segítségével hozzáférést valaki tooget felügyelő hello kockázatok csökkentése valaki más jelszót.

[Az Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) segítségével megakadályozhatja hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint. Egyszerű hitelesítési beállítások – a telefonhívás, szöveges üzenetet vagy mobilalkalmazásban megjelenő értesítést számos erős hitelesítés biztosítja. Felhasználók adja meg, amely az előnyben részesített hello módját.

hello legegyszerűbb módja toouse multi-factor Authentication rendszer hello Microsoft Authenticator mobilalkalmazás használható a Windows, iOS és Android rendszerű mobileszközök. Windows 10 és a helyszíni Active Directory, az Azure Active Directoryval (Azure AD) integrálása hello legújabb kiadása hello [a vállalati Windows Hello](../active-directory/active-directory-azureadjoin-passport-deployment.md) zökkenőmentes egyszeri bejelentkezés tooAzure erőforrások nem használható. Ebben az esetben hello Windows 10-es eszköz használatos hello második tényezőként a hitelesítéshez.

Fiókok kezelése az Azure-előfizetéshez és bejelentkezhet a toovirtual gépek-fiókok multi-factor Authentication használata lehetővé teszi az sokkal nagyobb biztonsági szintet mint csak a jelszavak használata. Kéttényezős hitelesítés más formában is előfordulhat, hogy alkalmazhatók, de a központi telepítés bonyolult lehet, ha azok még nem éles környezetben.

hello alábbi képernyőfelvételen látható Azure multi-factor Authentication elérhető hello beállításai közül:

![Többtényezős hitelesítés beállításai](./media/azure-security-iaas/mfa-options.png)

## <a name="limit-and-constrain-administrative-access"></a>Korlátozza és a hozzáférési rendszergazdai hozzáféréssel

Kezelheti az Azure-előfizetéshez hello fiókok védelme rendkívül fontos. hello sérült biztonság esetén ezek a fiókok bármelyikének összes hello hello értékének más lépéseket, hogy előfordulhat, hogy az adatok sértetlenségét és tooensure hello bizalmas ellentettjét adja. A közelmúltban is mutatják hello [Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) memóriavesztés minősített információk belső támadások jelent a hatalmas fenyegetés toohello bármely szervezet átfogó biztonsági.

Rendszergazdai jogosultságokkal az egyéni felhasználók számára a következő feltételek hasonló toothese kiértékelési:

- Végzi, akkor a rendszergazdai jogosultságokat igénylő feladatok?
- Milyen gyakran történik a hello feladatok?
- Miért hello feladatok nem hajtható végre, hogy a nevükben egy másik rendszergazda valamiért van?

A dokumentum minden egyéb ismert alternatív módszerek toogranting hello jogosultság és minden nem elfogadható.

hello használata csak időben történő felügyeletre megakadályozza, hogy a hello szükségtelen megléte emelt szintű jogosultságokkal rendelkező fiókok időszakban történjenek, amikor ezeket a jogokat nem szükségesek. Fiókok rendelkezik emelt szintű jogosultságokkal korlátozott ideig, hogy a rendszergazdák teheti a munkájukat. Ezeket a jogokat, majd a shift vagy a feladat végrehajtásának hello végén törlődnek.

Használhat [Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md) toomanage, megfigyelés és vezérlés hozzáférni a szervezetében. A program segít maradnak tudomást hello műveletei egyéni felhasználók számára a szervezetében. Csak időben történő felügyeletre tooAzure AD jogosult rendszergazdák hello fogalma bevezetésével is jelent. Ezek a személyeket, akik hello lehetséges toobe rendelkező fiókok rendelkeznek rendszergazdai jogosultságokkal. Felhasználók típusai Lépkedjen végig az aktiválási folyamat, és jogosultságokat admin adott időtartamra.


## <a name="use-devtest-labs"></a>Használja a DevTest Labs szolgáltatásban

Az Azure labs és fejlesztői környezetek lehetővé teszi a szervezetek toogain könnyen reagálhat tesztelési és fejlesztési véve kötelező hello késleltetése hardver beszerzése révén. Sajnos az Azure vagy egy desire toohelp ismeretét hiánya gyorsítása elfogadását jogok hozzárendelésekor hello rendszergazda toobe túlságosan megengedő vezethet. A kockázat véletlenül esetleg felfedi a hello szervezet toointernal támadásokat. Egyes felhasználók előfordulhat, hogy hozzáférést sokkal több mint kell rendelkezniük.

Hello [Azure DevTest Labs](../devtest-lab/devtest-lab-overview.md) használja [átruházásához hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) (RBAC). RBAC segítségével meg is elkülönítse azon belül a csapat be, amely csak hello szintjét, illetve felhasználók toodo szükséges adja meg a feladatokat a szerepkörök. Az RBAC előre definiált szerepkörök (tulajdonos, lab-felhasználó és közreműködő) tartalmaz. Nagy mértékben egyszerűsítheti, együttműködés és szerepkörök tooassign jogok tooexternal partnerek még akkor is használhatja.

Mivel a DevTest Labs RBAC használ,-e további, lehetséges toocreate [egyéni szerepkörök](../devtest-lab/devtest-lab-grant-user-permissions-to-specific-lab-policies.md). DevTest Labs nem csak egyszerűbbé teszi a hello tartozó engedélyek kezelésének, megkönnyíti a hello folyamatának kiépített környezetekben. Emellett segítséget nyújt az egyéb csoportok fejlesztési és tesztelési környezetben működő tipikus kihívásaival kezelésére. Bizonyos előkészületeket igényel, de hello hosszú távon, azt fogja egyszerűbbé a csapat számára.

Az Azure DevTest Labs funkciók a következők:

- Felügyeleti szabályozhatják a rendelkezésre álló toousers hello-beállítások. hello rendszergazda központilag kezelheti többek között az engedélyezett Virtuálisgép-méretek, a virtuális gépek és a virtuális gépek elindításakor maximális számát és a Leállítás.
- Tesztlabor-környezet létrehozása automatizálását.
- Költségek nyomon követése.
- Virtuális gépek egyszerűsített eloszlásáról együttműködési végzett munkához.
- Önkiszolgáló, amely lehetővé teszi a felhasználók tooprovision azok labs-sablonok használatával.
- Kezelése és a használat korlátozása.

![DevTest Labs toocreate labor használata](./media/azure-security-iaas/devtestlabs.png)

További költség nélkül társítva a DevTest Labs hello használata. hello létrehozása labs, házirendek, sablonok és összetevők felszabadul. Kell fizetnie csak hello a labs, például a virtuális hálózatok, virtuális gépek és tárfiókok használt Azure-erőforrások.



## <a name="control-and-limit-endpoint-access"></a>Vezérlő és a limit végpont hozzáférés

Labs vagy éles rendszerek esetén az Azure-ban üzemeltető azt jelenti, hogy a rendszer kell toobe hello Internet érhető el. Alapértelmezés szerint egy új Windows virtuális gép számára elérhető hello Internet hello RDP-portjára, és a Linux virtuális gép rendelkezik hello SSH-port megnyitásához. Jogosulatlan hozzáférés szükséges toominimize hello kockázatát lépések megtétele kitett toolimit végpontok.

Az Azure-ban technológiák segítségével hello hozzáférés toothose felügyeleti végpontok korlátozza. Használhatja az Azure [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md) (NSG-k). Központi telepítés Azure Resource Manager használata esetén az NSG-k korlátozhatják hello származó összes hálózatok toojust hello felügyeleti végpontok (RDP- vagy SSH-). Ha úgy gondolja, hogy az NSG-k, gondolja, hogy útválasztó hozzáférés-vezérlési listák. Használhatja őket tootightly vezérlő hello hálózati kommunikáció a különböző részeit az Azure-hálózatok között. Ez a szegélyhálózat vagy más elkülönített hálózatokhoz hasonló toocreating hálózatok. Nem vizsgálja, hogy a hello forgalmat, de a hálózati szegmentálást segítenek.


Azure, konfigurálhat egy [telephelyek közötti VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) a helyi hálózatról. A telephelyek közötti VPN kiterjeszti a helyszíni hálózati toohello felhő. Ez lehetővé teszi egy másik lehetőség toouse NSG-ket, mert hello NSG-k toonot módosíthatja is, hogy bárhonnan más, mint hello helyi hálózathoz. Majd megkövetelheti, hogy felügyeleti első csatlakozás toohello Azure-hálózatot VPN-en keresztül történik.

hello pont-pont VPN beállítás akkor lehet legtöbb vonzó azokban az esetekben, ahol éles rendszerek esetén a helyszíni erőforrások az Azure-ban szorosan integrált üzemeltet.

Másik lehetőségként használhatja a hello [pont-pont](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) esetekben célszerű toomanage rendszerek, amelyek nem beállítást kell tooon helyszíni erőforrások elérését. Ezekhez a rendszerekhez el lehet különíteni a saját Azure virtuális hálózatban. A rendszergazdák is tud VPN hello Azure környezetben a felügyeleti munkaállomás üzemeltetni.

>[!NOTE]
>Vagy VPN-beállítás tooreconfigure hello hello NSG-k toonot ACL-ek engedélyezése hello Internet access toomanagement végpontok is használhatja.

Érdemes fontolóra veheti, hogy egy másik lehetőség egy [távoli asztali átjáró](../multi-factor-authentication/multi-factor-authentication-get-started-server-rdg.md) központi telepítés. Használhatja a központi telepítés toosecurely HTTPS-en keresztül csatlakozó tooRemote asztali kiszolgálók, és részletesebb alkalmazása vezérli toothose kapcsolatok.

Kellene lennie funkciókra tooinclude eléréséhez:

- Felügyeleti beállítások toolimit kapcsolatok toorequests adott rendszerekből.
- Intelligens kártyás hitelesítést vagy az Azure multi-factor Authentication.
- Ellenőrzés, hogy mely rendszerek keresztül valaki toovia hello átjáró képes kapcsolódni.
- Eszköz és a lemez átirányítás vezérlése.

## <a name="use-a-key-management-solution"></a>A kulcskezelési megoldással

Biztonságos kulcskezelés az alapvető tooprotecting adatai hello felhőben. A [Azure Key Vault](../key-vault/key-vault-whatis.md), tudja biztonságosan tárolni a titkosítási kulcsokat, és kis titkos kulcsok, például jelszavak, a hardveres biztonsági modulokkal (HSM). A még nagyobb biztonság érdekében lehetőség van arra is, hogy kulcsokat importáljon és generáljon a hardveres biztonsági modulokban.

Microsoft folyamatok a kulcsokat a FIPS 140-2 2 ellenőrzött hardveres biztonsági modulok (a hardver és a belső vezérlőprogram) szinten. A figyelő és naplózási kulcs használata Azure naplózással: átadhatja a naplókat további elemzés és a fenyegetések észlelésére az Azure vagy a biztonsági adatai és az esemény felügyeleti SIEM-rendszerbe.

Bárki, aki az Azure-előfizetés hozhat létre és használhat kulcstárolót. Bár a Key Vault elsősorban a fejlesztők és rendszergazdák, megvalósítva, és a szervezet Azure-szolgáltatások kezelése felelős rendszergazda felügyeli.


## <a name="encrypt-virtual-disks-and-disk-storage"></a>Virtuális lemezek és a lemezes tárolás titkosítása

[Az Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) címek hello fenyegetését, hogy az adatlopással, illetve az, hogy érhető el, egy lemezt áthelyezésével történő jogosulatlan hozzáférést. hello a lemez csatlakoztatott tooanother rendszer egyéb biztonsági vezérlőt kihagyásával módja lehet. Titkosítás által használt lemezre [BitLocker](https://technet.microsoft.com/library/hh831713) a Windows és a DM-crypt program segítségével az Linux tooencrypt operációs rendszer és az adatokat. Az Azure Disk Encryption Key Vault toocontrol integrálható, és hello titkosítási kulcsok kezeléséhez. Érhető el a szokásos virtuális gépek és virtuális gépek prémium szintű Storage.

További információkért lásd: [Windows és Linux IaaS virtuális gépeket az Azure Disk Encryption](azure-security-disk-encryption.md).

[Az Azure Storage szolgáltatás titkosítási](../storage/common/storage-service-encryption.md) az inaktív adatok védelmét. Hello tárolási fiók szintjén engedélyezve van. Azt titkosítani fogja az adatokat adatközpontjainkba van írva, és a rendszer a hozzáféréskor automatikusan visszafejti. A következő forgatókönyvek hello támogatja:

- Titkosítási blokkblobokat, hozzáfűző blobokat és lapblobokat
- Archivált VHD-k és sablonok titkosításának tooAzure nincsenek a helyszíni
- IaaS virtuális gépeket, amelyet a VHD-k használatával hozott létre az alapul szolgáló operációsrendszer- és adatlemezek titkosítása

Mielőtt továbblép az Azure Storage Encryption, két korlátozások figyelembe vennie:

- Nem érhető el a klasszikus tárfiókokat.
- Csak a titkosítás engedélyezése után írt adatok, az titkosítja.

## <a name="use-a-centralized-security-management-system"></a>A központi biztonsági rendszer használata

A kiszolgálók toobe javítását, konfigurációs, eseményeket, és a tevékenységek, amelyek a biztonsági aggályokon tekinthető figyelni kell. használhatja a tooaddress azokat vonatkozik, [Security Center](https://azure.microsoft.com/services/security-center/) és [Operations Management Suite biztonsági és megfelelőségi](https://azure.microsoft.com/services/security-center/). Mindkét lehetőség hello konfigurációs hello operációs rendszeren túlmutató. Ezenkívül tartalmaznak az alapul szolgáló infrastruktúra, például a hálózati konfigurációs és virtuális készülék használata hello hello konfigurációjának ellenőrzése.

## <a name="manage-operating-systems"></a>Operációs rendszerek kezelése

IaaS-telepítés, továbbra is való telepítésért felelős hello felügyeleti hello rendszerek központilag, csakúgy, mint bármely más kiszolgálóra vagy munkaállomásra a környezetben. Javítás, korlátozására, engedélyek és egyéb tevékenységek kapcsolatos toohello karbantartási a rendszer továbbra is a felelős. A helyszíni erőforrások szorosan integrált rendszerekhez, érdemes toouse hello azonos eszközökkel és eljárásokkal, hogy a helyszíni többek között a víruskereső, a kártevőirtó, a javítás és a biztonsági mentés használja.

### <a name="harden-systems"></a>Rendszerek megerősítése
Azure IaaS összes virtuális gépnek kell megerősítve, így csak a telepített alkalmazások hello szükséges Szolgáltatásvégpontok szolgáltatnak. Windows virtuális gépek esetében kövesse hello Microsoft által közzétett hello alapkonfigurációkat, [Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) megoldás.

Security Compliance Manager ingyenes. Használat tooquickly konfigurálhatja és kezelheti a asztalok, a hagyományos adatközpontok és a privát és nyilvános felhő csoportházirend és a System Center Configuration Manager segítségével.

Security Compliance Manager házirendek kész a központi telepítése és ellenőrzi a Szükségeskonfiguráció-kezelési konfigurációs csomagok biztosít. Ezek alaptervek alapuló [Microsoft biztonsági útmutatói](https://technet.microsoft.com/en-us/library/cc184906.aspx) javaslatok és az iparág ajánlott eljárások. Ezek segítségével kezelheti a konfigurációs eltéréseket, cím megfelelőségi követelményeket, és a biztonsági kockázatok csökkentése.

Security Compliance Manager tooimport hello aktuális konfigurációt a számítógépek két különböző módon használhatja. Először az Active Directory-alapú csoportházirendek importálhatja. Második, "arany főkiszolgáló" hello konfigurációs importálhat hello segítségével a referencia-számítógép [LocalGPO eszköz](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) tooback hello helyi csoportházirend. Importálhatja hello helyi csoportházirend Security Compliance Manager.

Hasonlítsa össze a szabványok tooindustry gyakorlati tanácsok, testre is szabhatja őket, és hozzon létre új házirendek és a szükséges konfiguráció kezelésének konfigurációs csomagokat. Az összes támogatott operációs rendszerhez, beleértve a Windows 10 évforduló Update és Windows Server 2016 alaptervek közzé lettek.


### <a name="install-and-manage-antimalware"></a>Telepítheti és kezelheti a kártevőirtó

Az éles környezetben a külön-külön üzemeltetett környezetek egy kártevőirtó bővítmény toohelp használhatja a virtuális gépet véd, és a felhőalapú szolgáltatások. Az integrálható [az Azure Security Center](../security-center/security-center-intro.md).


[A Microsoft Antimalware](azure-security-antimalware.md) magában foglalja a szolgáltatások, mint a valós idejű védelem, ütemezett vizsgálatát, kártevő szoftverek eltávolítása, aláírás frissítések, motor frissítéseit, reporting, kizárási eseménygyűjtés, mintákat és [PowerShell-támogatás](https://msdn.microsoft.com/library/dn771715.aspx).

![Az Azure kártevőirtó](./media/azure-security-iaas/azantimalware.png)

### <a name="install-hello-latest-security-updates"></a>Hello a legújabb frissítések telepítése
Hello első munkaterhelések, hogy az ügyfelek tooAzure helyezhetik át labs és kívülre irányuló rendszerek. Ha az Azure által üzemeltetett virtuális gépek alkalmazásokhoz vagy szolgáltatásokhoz, toobe elérhető toohello Internet kell futtatni, éberen kapcsolatos javítását. Javítás hello operációs rendszer felett. Harmadik féltől származó alkalmazások a nem javított biztonsági rések is okozhat, ha rendelkezésre áll a javítások jó elkerülhetők tooproblems.

### <a name="deploy-and-test-a-backup-solution"></a>Telepítse és tesztelje a biztonsági mentési megoldás

Csakúgy, mint a biztonsági frissítések, biztonsági másolatot kell kezelni toobe hello azonos módon, hogy kezeli-e bármilyen más műveletet. Ez igaz az éles környezetben toohello felhő kiterjesztése részét képező rendszerek. Teszt és fejlesztési rendszerek biztonsági mentési stratégia visszaállítási képességeket, amelyek hasonló toowhat felhasználók rendelkezik hozzászoktak szeretné, azok a helyszíni környezetben élményt nyújtó kell követnie.

Termelési számítási feladatokhoz áthelyezése tooAzure kell integrálható a meglévő biztonsági mentési megoldás, ha lehetséges. Másik lehetőségként használhatja [Azure biztonsági mentési](../backup/backup-azure-arm-vms.md) toohelp címet a biztonsági mentés követelményeinek.


## <a name="monitor"></a>Figyelés

[A Security Center](../security-center/security-center-intro.md) biztosít az Azure-erőforrások biztonsági állapotának hello folyamatos értékelésének tooidentify potenciális biztonsági hiányosságok. A javaslatok listája végigvezeti hello szükséges szabályozási konfigurálásának lépésein.

Példák erre vonatkozóan:

- Kiépítése a toohelp határozza meg és eltávolítani a kártevő szoftvereket.
- Hálózati biztonsági csoportok és a szabályok toocontrol forgalom toovirtual gépek konfigurálása.
- A webes alkalmazások célzó támadások elleni védelemre toohelp webalkalmazási tűzfalak kiépítése.
- Hiányzó rendszerfrissítések telepítése.
- Operációs rendszer azon konfigurációit, amelyek nem felelnek meg a hello címzési ajánlott alaptervek.

hello következő kép bemutatja, hogy engedélyezheti a Security Center hello-beállítások egy része.

![Az Azure Security Center házirendek](./media/azure-security-iaas/security-center-policies.png)

[Az Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) a Microsoft felhőalapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra. Az Operations Management Suite a felhő alapú szolgáltatásként valósul meg, mert gyors és az infrastruktúrához kapcsolódó erőforrások használatára irányuló befektetéséből minimális telepíthető.

Új szolgáltatások automatikusan érkeznek, a folyamatos karbantartási így meg lehet spórolni, és frissítse a költségeket. Az Operations Management Suite is integrálható a System Center Operations Manager. Jobb kezelése az Azure munkaterhelésekhez, többek között a különböző összetevők toohelp rendelkezik egy [biztonsági és megfelelőségi](../operations-management-suite/oms-security-getting-started.md) modul.

Az Operations Management Suite tooview adatot hello biztonsági és megfelelőségi funkciók az erőforrásokra vonatkozó is használhatók. hello információt négy fő kategóriába sorolhatók:

- **Biztonsági tartományok**: további felfedezés biztonsági rekordok időbeli. Frissítse az értékelés, a hálózati biztonsági adatai, a identitások és hozzáférések információkat és a számítógépek biztonsági események, férhet hozzá a kártevő szoftverek assessment. A gyors elérés érdekében toohello Azure Security Center irányítópultjának előnyeit.
- **Jelentős problémák**: gyorsan aktív problémák hello számának meghatározása, és ezek a problémák súlyosságát hello.
- **Észlelések (előzetes verzió)**: azonosítására a támadási mintákat által a biztonsági riasztások megjelenítése, akkor fordulhat elő, az erőforrásokon.
- **A fenyegetés eszközintelligencia**: azonosítására a támadási minták kimenő kártékony IP-forgalom, a kiszolgálók száma hello megjelenítése hello rosszindulatú fenyegetés típusát és megjelenítő, ha ezeket az IP-címekről érkező.
- **Általános biztonsági lekérdezések**: lásd a leggyakrabban használt biztonsági hello listája lekérdezi a használható toomonitor a környezetben. Lekérdezések kiválasztásakor hello **keresési** panel nyílik meg, és ehhez a lekérdezéshez hello eredményeket jeleníti meg.

hello alábbi képernyőfelvételen szereplő példán látható Operations Management Suite megjelenítő hello információk.

![Az Operations Management Suite biztonsági alapterveket](./media/azure-security-iaas/oms-security-baseline.png)



## <a name="next-steps"></a>Következő lépések


* [Az Azure Security csapat blogja](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)
* [Az Azure biztonsági ajánlott eljárásairól és mintáiról](security-best-practices-and-patterns.md)
