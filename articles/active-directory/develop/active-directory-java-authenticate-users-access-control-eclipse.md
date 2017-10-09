---
title: "Hozzáférés-vezérlés (Java) aaaHow toouse |} Microsoft Docs"
description: "Megtudhatja, hogyan toodevelop és -felhasználási hozzáférés-vezérlés: a Java az Azure-ban."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 247dfd59-0221-4193-97ec-4f3ebe01d3c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: cbbce3b1a05eabf3b86a8cb91db1bde92dbb8960
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Hogyan tooAuthenticate Azure Access Control Service használatával Eclipse webes felhasználók
Ez az útmutató bemutatja, hogyan toouse hello belül hello Azure eszköztára Eclipse Azure Access Control Service (ACS). Az ACS további információkért lásd: hello [további lépések](#next_steps) szakasz.

> [!NOTE]
> hello Azure Access Services vezérlő szűrő egy közösségi technológiai előzetes. Kiadás előtti szoftverként azt van hivatalosan Microsoft nem támogatja.
> 
> 

## <a name="what-is-acs"></a>Mi az az ACS?
Legtöbb fejlesztő nem identity szakértők és általában nem szeretné töltött idő fejlesztését hitelesítési és engedélyezési mechanizmusok az alkalmazások és szolgáltatások számára. ACS egy Azure-szolgáltatás, amely egyszerűen azok a felhasználók, akik tooaccess a webes alkalmazások és szolgáltatások anélkül, hogy a kód toofactor összetett hitelesítés logika.

az ACS hello a következő funkciók érhetők el:

* Integráció a Windows Identity Foundation (WIF).
* Beleértve a Windows Live ID, Google, Yahoo!-s és Facebook a népszerű webes Identitásszolgáltatók (IP-címek) támogatása.
* Támogatás az Active Directory összevonási szolgáltatások (AD FS) 2.0-s.
* Egy nyitott Data protokoll (OData)-alapú szolgáltatás, amely szoftveres hozzáférést biztosít az tooACS beállításait.
* A felügyeleti portál, amely lehetővé teszi, hogy a rendszergazdai hozzáférés toohello ACS-beállításokat.

ACS kapcsolatos további információkért lásd: [Access Control Service 2.0][Access Control Service 2.0].

## <a name="concepts"></a>Alapelvek
Az Azure ACS hello rendszerbiztonsági tagok a jogcímalapú identitás - egy egységes módszer toocreating hitelesítési mechanizmusok, helyileg futó alkalmazásokhoz vagy hello felhőben épül. Jogcímalapú identitás közös lehetőséget biztosít az alkalmazások és szolgáltatások tooacquire a identitás felhasználókról van szükségük a szervezet más, és az hello Internet belül.

Ez az útmutató toocomplete hello feladatokat, tisztában kell lennie a következő fogalmak hello:

**Ügyfél** -hello környezetében ez hogyan tooguide, ez az kísérel meg toogain hozzáférés tooyour webalkalmazás böngészőt.

**Függő entitásonkénti (RP) alkalmazás** -alkalmazás egy függő Entitás egy webhelyre vagy szolgáltatásba, amelyek a hitelesítési tooone külső hatóság outsources. Az azonosító egy zsargon azt tegyük fel például, hogy hello RP megbízhatónak tekinti az adott szolgáltatót. Ez az útmutató ismerteti, hogyan tooconfigure az alkalmazás tootrust ACS.

**Token** -jogkivonat általában egy felhasználó sikeres hitelesítés után kiadott biztonsági adatok gyűjteménye. Tartalmaz egy *jogcímek*, hello attribútumait hitelesítette a felhasználót. A jogcím jelenthet a felhasználó nevét, egy felhasználói szerepkör azonosítójának tartozik, a felhasználó élettartama, és így tovább. A jogkivonat általában digitális aláírással, ami azt jelenti, hogy mindig azok termékekre hátsó tooits kibocsátó, és a benne lévő tartalom ne lehessen illetéktelenül. Egy felhasználói kap hozzáférést tooa RP alkalmazást hello RP alkalmazás megbízhatónak hatóságnak érvényes jogkivonat segítségével.

**Identity Provider (IP)** -egy IP-cím egy szervezet, amely hitelesíti a felhasználói identitások és biztonsági jogkivonatokat bocsát ki. tényleges munka hello jogkivonatok kiállításának valósul meg, ha egy különleges biztonsági jogkivonat szolgáltatás (STS) nevű szolgáltatás. IP-címek tipikus például Windows Live ID, Facebook-on, üzleti felhasználói tárolóhelyekkel (például az Active Directory), és így tovább.
Ha ACS konfigurált tootrust olyan IP-címet, hello rendszer fogadja el, és ellenőrizze, hogy az IP által kiállított jogkivonatokat. ACS is megbízhat több IP-cím egyszerre, ami azt jelenti, hogy amikor az alkalmazás ACS megbízhatónak tekinti, azonnal kínálhat az alkalmazás tooall hello hitelesített felhasználó az összes hello IP-címeket, hogy az ACS megbízhatónak tekinti az Ön nevében.

**Összevonási szolgáltató (pi)** -IP-címek közvetlen ismerő felhasználók, és azokat a hitelesítő adataik használatával hitelesíti és mi tudják róluk kapcsolatos jogcímeket kiadni. Egy összevonási szolgáltató (pi) egy eltérő jellegű hatóság: ahelyett, hogy közvetlenül a felhasználók hitelesítéséhez, úgy működik, mint egy közvetítő és a brókerek hitelesítési között egy függő Entitás és egy vagy több IP-címek. IP-címek és a FPs ki biztonsági jogkivonatokat, így mindkettő használja a biztonsági jogkivonat szolgáltatás (STS). ACS egy pi.

**ACS Jogcímszabály-motor** -használt tootransform bejövő jogkivonatokat a megbízható IP-címek tootokens azt jelentette, hogy hello függő Entitás által felhasznált toobe egyszerű formájában van szerkezetbe hello logika jogcím-átalakítási szabályok. ACS funkciókat a jogcímszabály-motor, amely gondoskodik, függetlenül a függő Entitás megadott átalakítása logika alkalmazását.

**Az ACS-Namespace** -egy névtér, a beállítások sorolására használó ACS legfelső szintű partíciójának. Egy névtér, amely tárolja a megbízható IP-címek listáját, hello RP alkalmazásokat tooserve, várt hello szabályok hello szabály motor tooprocess bejövő jogkivonatokat, és így tovább. Egy névtér elérhetővé teszi a különböző végpontok, amelyek lesznek hello alkalmazás és a fejlesztői tooget ACS tooperform funkciójához használják.

hello alábbi ábrán láthatók az ACS-hitelesítés működéséről webes alkalmazásokkal együtt:

![ACS folyamatábrája][acs_flow]

1. hello ügyfél (ebben az esetben a böngésző) kér hello RP oldal.
2. Hello kérelem még nem hitelesíthetők, mert a hello RP átirányítja hello felhasználói toohello hatóság megbízhatónak Ez az ACS. hello ACS hello felhasználói IP-címe az e RP megadott hello választható mutatja be. hello felhasználó hello megfelelő IP választ.
3. hello ügyfél böngészik toohello IP hitelesítési lapot, és megkéri a hello felhasználói toolog a.
4. Hello ügyfél (például hitelesítőadatok meg legyenek adva hello identitás) hitelesítése után hello IP kibocsát egy biztonsági jogkivonatot.
5. A biztonsági jogkivonat kiállító, után hello IP átirányítja hello ügyfél tooACS, és hello ügyfél küldi hello által kiadott biztonsági jogkivonat hello IP tooACS.
6. ACS hello biztonsági jogkivonat kiállító hello IP, bemeneti adatokat hello identitás hello ACS szabálymotor be ez a token jogcímek hello kimeneti identitási jogcímek kiszámítja és kiad egy új biztonsági jogkivonatot, amely tartalmazza a kimeneti jogcímek ellenőrzi.
7. ACS hello ügyfél toohello RP irányítja át. hello ügyfél küldi hello ACS toohello függő Entitás által kiadott új biztonsági jogkivonatot. hello RP ellenőrzi az ACS által kibocsátott hello biztonsági jogkivonat aláírása hello, ez a token hello jogcímek ellenőrzi, és adja vissza az eredetileg kért hello lapot.

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató toocomplete hello feladatokat, szüksége lesz a hello következő:

* A Java fejlesztői készlet (JDK), v 1.6 vagy újabb.
* Java EE-fejlesztőknek IDE Holdas Indigo vagy újabb. Ez letölthető <http://www.eclipse.org/downloads/>. 
* A Java-alapú webkiszolgáló vagy a kiszolgáló, például az Apache Tomcat, GlassFish, JBoss alkalmazáskiszolgáló vagy Jetty terjesztési.
* Azure-előfizetéssel, amely szerezhető: a <http://www.microsoft.com/windowsazure/offers/>.
* hello Azure eszközkészlet eclipse-ben a 2014. április kiadás vagy újabb. További információkért lásd: [telepítése hello Azure eszköztára Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
* Egy X.509 tanúsítvány toouse az alkalmazással. Szüksége lesz a tanúsítvány nyilvános tanúsítvány (.cer), mind a személyes információcsere (. PFX) formátumú. (Ez a tanúsítvány létrehozásához beállítások előnyben, annak leírását az oktatóanyag későbbi részében).
* Hello Azure ismeretét számítási megvitatni emulátor és a központi telepítési módszerek [egy Hello World alkalmazásról az Azure létrehozása az eclipse-ben](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Hozzon létre egy ACS-Namespace
az Azure Access Control Service (ACS) használatával toobegin létre kell hoznia egy ACS-névtér. hello névteret biztosít az alkalmazáson belül az ACS-erőforrások címzéshez egyedi hatókör.

1. Jelentkezzen be a hello [Azure felügyeleti portálon][Azure Management Portal].
2. Kattintson a **Active Directory**. 
3. toocreate egy új hozzáférés-vezérlés névtér esetén kattintson **új**, kattintson a **alkalmazásszolgáltatások**, kattintson a **hozzáférés-vezérlés**, és kattintson a **Gyorslétrehozás** . 
4. Adja meg a hello névtér nevét. Azure ellenőrzi, hogy hello neve egyedi.
5. Válassza ki a hello régióban mely hello névtér használják. A hello legjobb teljesítmény érdekében használjon hello régióban, amelyben a az alkalmazás telepítéséhez.
6. Ha egynél több előfizetéssel rendelkezik, válassza ki a megjeleníteni kívánt toouse hello ACS névtér hello előfizetést.
7. Kattintson a **Create** (Létrehozás) gombra.

Azure hoz létre, és aktiválja a hello névtér. Várjon, amíg a hello hello új névtér állapota **aktív** folytatása előtt. 

## <a name="add-identity-providers"></a>Identitás-szolgáltatóktól hozzáadása
Ez a feladat hozzáadja IP-címek toouse hitelesítéshez RP az alkalmazáshoz. Bemutatási célokra Ez a feladat azt mutatja, hogyan Windows Live olyan IP-címet, de tooadd sikerült használják hello hello ACS felügyeleti portál szereplő IP-címek valamelyikét.

1. A hello [Azure felügyeleti portálon][Azure Management Portal], kattintson a **Active Directory**, jelöljön ki egy hozzáférés-vezérlés névteret, és kattintson a **kezelése**. Megnyílik a hello ACS felügyeleti portálon.
2. Hello ACS felügyeleti portál hello bal oldali navigációs ablaktábláján kattintson **identitás-szolgáltatóktól**.
3. A Windows Live ID alapértelmezés szerint engedélyezve van, és nem törölhető. Ez az oktatóanyag céljából csak az Windows Live ID szolgál. Azonban erre a képernyőre, ahol hello kattintva hozzáadhatja más IP-címek **Hozzáadás** gombra.

A Windows Live ID engedélyezve van, egy IP-Címek használhatók a ACS névterét. Ezt követően adja meg a Java-alkalmazásokra (újabb toobe), egy függő Entitás.

## <a name="add-a-relying-party-application"></a>Függő entitás alkalmazás felvétele
Ebben a feladatban konfigurálja ACS toorecognize a Java-webalkalmazás érvényes RP-alkalmazásként.

1. A hello ACS felügyeleti portál, kattintson **függő entitás alkalmazásai számára**.
2. A hello **függő entitás alkalmazásai számára** kattintson **Hozzáadás**.
3. A hello **függő entitás alkalmazás hozzáadása** lapon, a következő hello:
   
   1. A **neve**, hello RP hello nevét. Ez az oktatóanyag céljából, írja be a következőt **Azure Web Apps**.
   2. A **mód**, jelölje be **adja meg a beállításokat manuálisan**.
   3. A **tartomány**, típus hello URI toowhich hello biztonsági jogkivonatot az ACS által kibocsátott vonatkozik. Ez a feladat, írja be a következőt **8080 /**.
      ![Függő entitás realm compute emulator használható][relying_party_realm_emulator]
   4. A **visszatérési URL-** típus hello URL-cím toowhich ACS hello biztonsági jogkivonatot ad vissza. Ez a feladat, írja be a következőt **http://localhost:8080/MyACSHelloWorld/index.jsp**
      ![függő entitás URL-cím használható compute emulator adja vissza.][relying_party_return_url_emulator]
   5. Fogadja el a hello mezők hello többi hello alapértelmezett értékeket.
4. Kattintson a **Save** (Mentés) gombra.

Ezzel sikeresen beállította a Java-webalkalmazás hello Azure compute emulator futtatáskor (: 8080 /) toobe egy függő Entitás az ACS-névtérben. Ezután hozzon létre hello szabályok, amelyek ACS hello RP tooprocess jogcímeket használ.

## <a name="create-rules"></a>Szabályok létrehozása
Ebben a feladatban hello szabályok, amelyek meghatározzák a jogcímek az IP-címek tooyour RP átadott hogyan határozza meg. A jelen útmutató hello célra azt egyszerűen beállítja a ACS toocopy hello bemeneti jogcímtípusokat és -értékek közvetlenül a hello kimeneti jogkivonatot, nélkül szűrését, és módosítja azokat.

1. A hello ACS felügyeleti portál fő lapján, kattintson **csoportok szabály**.
2. A hello **csoportok** kattintson **Szabálycsoportja alapértelmezett Azure webalkalmazás**.
3. A hello **szabály csoport szerkesztése** kattintson **Generate**.
4. A hello **szabályok létrehozása: alapértelmezett szabály csoport Azure webalkalmazás** lapon, győződjön meg arról, a Windows Live ID be van jelölve, majd kattintson **Generate**.    
5. A hello **szabály csoport szerkesztése** kattintson **mentése**.

## <a name="upload-a-certificate-tooyour-acs-namespace"></a>A tanúsítványnévtéren tooyour ACS feltöltése
Ebben a feladatban feltöltése egy. PFX-tanúsítvány, amely lesz használt toosign jogkivonat-kérelmeket hozta létre a ACS névterét.

1. A hello ACS felügyeleti portál fő lapján, kattintson **tanúsítványok és kulcsok**.
2. A hello **tanúsítványok és kulcsok** kattintson **Hozzáadás** fent **jogkivonat-aláíró**.
3. A hello **hozzáadása jogkivonat-aláíró tanúsítvány és kulcs** lap:
   1. A hello **használt** területen kattintson **függő alkalmazás** válassza **Azure Web Apps** (amely a korábban állítja be a függő entitás alkalmazás hello neve).
   2. A hello **típus** szakaszban jelölje be **X.509 tanúsítvány**.
   3. A hello **tanúsítvány** szakaszt, hello Tallózás gombra, és keresse meg, hogy szeretné-e toouse toohello X.509 tanúsítvány fájl. Ez lesz a. PFX-fájlt. Jelölje ki azt a hello fájl **nyissa meg**, és írja be a hello hello tanúsítvány jelszava **jelszó** szövegmezőben. Vegye figyelembe, hogy a tesztelési célokra, előfordulhat, hogy önkiszolgáló-signed-tanúsítványt használ. toocreate egy önaláírt tanúsítványt használjon hello **új** hello gombjára **ACS könyvtára** párbeszédpanelen (a későbbiekben olvashat), vagy használja a hello **encutil.exe** hello azeszközt[projekt webhely] [ project website] a Javához készült Azure Starter Kit hello.
   4. Győződjön meg arról, hogy **elsődleges ellenőrizze** be van jelölve. A **hozzáadása jogkivonat-aláíró tanúsítvány és kulcs** lap alábbihoz hasonló toohello következő.
       ![Jogkivonat-aláíró tanúsítvány hozzáadása][add_token_signing_cert]
   5. Kattintson a **mentése** toosave beállításainak és Bezárás hello **hozzáadása jogkivonat-aláíró tanúsítvány és kulcs** lap.

Ezt követően ellenőrizze, hogy szüksége lesz tooconfigure a Java webes alkalmazás toouse ACS hello alkalmazásintegráció lapjáról, és másolja hello URI információk hello.

## <a name="review-hello-application-integration-page"></a>Áttekintés hello alkalmazásintegráció lap
Található összes hello információt és hello kód szükséges tooconfigure a Java webes alkalmazás (függő Entitás alkalmazás hello) toowork az ACS használatával az ACS felügyeleti portál hello hello alkalmazásintegráció oldalán. Szüksége lesz ezeket az információkat a Java-webalkalmazás az összevont hitelesítés konfigurálásakor.

1. A hello ACS felügyeleti portál, kattintson **alkalmazásintegráció**.  
2. A hello **alkalmazásintegráció** kattintson **bejelentkezési oldalak**.
3. A hello **bejelentkezési oldal integrációs** kattintson **Azure Web Apps**.

A hello **bejelentkezési oldal integrációs: Azure Web Apps** hello URL-címe a lap **1. lehetőség: hivatkozás tooan ACS által szolgáltatott bejelentkezési oldal** fogja használni a Java-alkalmazásokra. Ez az érték akkor hello Azure Access Control szolgáltatások szűrő könyvtár tooyour Java-alkalmazások hozzáadásakor.

## <a name="create-a-java-web-application"></a>Java-webalkalmazás létrehozása
1. Eclipse-ben belül hello menüben kattintson **fájl**, kattintson a **új**, és kattintson a **dinamikus webes projekt**. (Ha nem lát **dinamikus webes projekt** kattintás után az elérhető projektek tulajdonosaként **fájl**, **új**, majd hello a következő: kattintson a **fájl**, kattintson a **új**, kattintson **projekt**, bontsa ki a **webes**, kattintson a **dinamikus webes projekt**, és kattintson a  **Ezután**.) Ez az oktatóanyag céljából, nevet hello projektnek **MyACSHelloWorld**. (Ellenőrizze a nevet használja, ez az oktatóanyag következő lépései a WAR-fájl MyACSHelloWorld nevű toobe várt). A képernyő jelenik meg hasonló toohello következő:
   
    ![ACS exampple Hello World projekt létrehozása][create_acs_hello_world]
   
    Kattintson a **Befejezés** gombra.
2. A Eclipse Project Explorer nézet, bontsa ki a **MyACSHelloWorld**. Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.
3. A hello **új JSP-fájl** párbeszédpanelen nevű hello fájl **index.jsp**. Tartsa hello szülőmappa MyACSHelloWorld/WebContent, mint ahogy az alábbi hello:
   
    ![ACS például JSP-fájl hozzáadása][add_jsp_file_acs]
   
    Kattintson a **Tovább** gombra.
4. A hello **JSP-sablon kiválasztása** párbeszédpanelen válassza **új JSP-fájl (html)** kattintson **Befejezés**.
5. Hello index.jsp fájl megnyitásakor az eclipse-ben, adja hozzá a szöveg toodisplay **Hello ACS World!** hello meglévő belül `<body>` elemet. A frissített `<body>` tartalom hello következő jelennek meg:
   
        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
   
    Mentse az index.jsp.

## <a name="add-hello-acs-filter-library-tooyour-application"></a>Hello ACS szűrő könyvtár tooyour alkalmazás hozzáadása
1. A Project Explorer Eclipse meg, kattintson a jobb gombbal **MyACSHelloWorld**, kattintson **Build elérési**, és kattintson a **Build elérési konfigurálása**.
2. A hello **Java Build elérési** párbeszédpanel, kattintson hello **szalagtárak** lapon.
3. Kattintson a **könyvtár hozzáadása**.
4. Kattintson a **Azure hozzáférést vezérlő Services szűrés (MS nyitott műszaki)** majd **következő**. Hello **Azure Access Control szolgáltatások szűrő** párbeszédpanel jelenik meg.  (hello **hely** mező lehet egy másik elérési utat, attól függően, hogy a telepítési eclipse-ben, és hello verziószáma eltérő, attól függően, hogy a szoftverfrissítések lehet.)
   
    ![Az ACS-szűrő könyvtár hozzáadása][add_acs_filter_lib]
5. Egy böngészőben megnyitva toohello **bejelentkezési oldal integrációs** hello felügyeleti portál másolási hello URL-címe a hello oldalán **1. lehetőség: hivatkozás tooan ACS által szolgáltatott bejelentkezési oldal** mezőben, majd illessze be hello **ACS hitelesítési végpont** hello Eclipse párbeszédpanel mezőjében.
6. Egy böngészőben megnyitva toohello **függő entitás alkalmazás szerkesztése** hello felügyeleti portál másolási hello URL-címe a hello oldalán **tartomány** mezőben, majd illessze be hello **függő entitás Realm**  hello Eclipse párbeszédpanel mezőjében.
7. Hello belül **biztonsági** hello Eclipse párbeszédpanel, ha azt szeretné, hogy egy meglévő tanúsítvány toouse részén kattintson **Tallózás**, keresse meg a toohello tanúsítvány toouse szeretne, válassza ki azt, és kattintson  **Nyissa meg**. Vagy, ha azt szeretné, hogy toocreate egy új tanúsítványt, kattintson a **új** toodisplay hello **új tanúsítvány** párbeszédpanelen, majd adja meg a hello jelszó hello .cer fájl neve és hello .pfx-fájlt az új hello neve tanúsítvány.
8. Ellenőrizze **beágyazási hello tanúsítvány hello WAR-fájlt a**. Az ilyen módon beágyazási hello tanúsítvány megtalálható a környezetben anélkül, hogy Ön toomanually célútvonallal összetevőt. (Ha inkább kell tárolnia a tanúsítványt kívülről a WAR-fájl, sikerült szerepkör összetevőjeként hello tanúsítvány hozzáadása és törölje a jelet **beágyazási hello tanúsítvány hello WAR-fájlt a**.)
9. [Választható] Tartsa **szükséges HTTPS-kapcsolatok** be van jelölve. Ha ezt a beállítást, szüksége lesz tooaccess az alkalmazás hello HTTPS protokoll használatával. Ha nem szeretné toorequire HTTPS-kapcsolatok, törölje ezt a beállítást.
10. A központi telepítés toohello számítási emulátor, a **Azure ACS szűrő** beállítások hasonló toohello következő fog kinézni.
    
    ![A központi telepítés toohello Azure ACS szűrő beállításának számítási emulátor][add_acs_filter_lib_emulator]
11. Kattintson a **Befejezés** gombra.
12. Kattintson a **Igen** amikor az egy párbeszédpanel, amely arról tájékoztat, hogy egy web.xml fájl jön létre.
13. Kattintson a **OK** tooclose hello **Java Build elérési** párbeszédpanel.

## <a name="deploy-toohello-compute-emulator"></a>Toohello compute emulator telepítése
1. A Project Explorer Eclipse meg, kattintson a jobb gombbal **MyACSHelloWorld**, kattintson a **Azure**, és kattintson a **az Azure-csomag**.
2. A **projektnevet**, típus **MyAzureACSProject** kattintson **következő**.
3. Válassza ki a JDK és az alkalmazáskiszolgálóhoz. (Ezeket a lépéseket a hello részletesen ismertetett [egy Hello World alkalmazásról az Azure létrehozása az eclipse-ben](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) oktatóanyag).
4. Kattintson a **Befejezés** gombra.
5. Kattintson a hello **Azure emulátorban futtatása** gombra.
6. A Java-webalkalmazás hello compute emulator a elindulása után zárja be a böngésző minden példányát (úgy, hogy a jelenlegi böngésző-munkamenetek ne zavarja meg az ACS-bejelentkezés tesztelése).
7. Futtassa az alkalmazást megnyitásával <8080/MyACSHelloWorld/> a böngésző (vagy <https://localhost:8080/MyACSHelloWorld/> Ha bejelölte **szükséges HTTPS-kapcsolatok** ). A rendszer kéri a Windows Live ID bejelentkezési azonosítóhoz, akkor kell toohello visszatérési URL-cím függő entitás alkalmazás megadott kerül.
8. Amikor befejezte az alkalmazást, kattintson a hello **visszaállítása Azure Emulátorban** gombra.

## <a name="deploy-tooazure"></a>TooAzure telepítése
toodeploy tooAzure, szüksége lesz toochange hello függő entitás realm, és térjen vissza URL-címet a ACS névterét.

1. Hello Azure felügyeleti portálon, a hello belül **függő entitás alkalmazás szerkesztése** lapján módosíthatja **tartomány** toobe hello webhely URL-CÍMÉT a telepített. Cserélje le **példa** az üzembe helyezéshez megadott hello DNS-névvel.
   
    ![Függő entitás realm üzemi használatra][relying_party_realm_production]
2. Módosítsa **visszatérési URL-** toobe hello URL-cím, az alkalmazás. Cserélje le **példa** az üzembe helyezéshez megadott hello DNS-névvel.
   
    ![Függő entitás visszatérési URL-cím üzemi használatra][relying_party_return_url_production]
3. Kattintson a **mentése** toosave a frissített függő entitás realm, és térjen vissza URL-cím módosítása.
4. Tartsa hello **bejelentkezési oldal integrációs** lapon nyissa meg a böngészőben, hamarosan szüksége toocopy belőle.
5. A Project Explorer Eclipse meg, kattintson a jobb gombbal **MyACSHelloWorld**, kattintson **Build elérési**, és kattintson a **Build elérési konfigurálása**.
6. Kattintson a hello **szalagtárak** lapra, majd **Azure Access Control szolgáltatások szűrő**, és kattintson a **szerkesztése**.
7. Egy böngészőben megnyitva toohello **bejelentkezési oldal integrációs** hello felügyeleti portál másolási hello URL-címe a hello oldalán **1. lehetőség: hivatkozás tooan ACS által szolgáltatott bejelentkezési oldal** mezőben, majd illessze be hello **ACS hitelesítési végpont** hello Eclipse párbeszédpanel mezőjében.
8. Egy böngészőben megnyitva toohello **függő entitás alkalmazás szerkesztése** hello felügyeleti portál másolási hello URL-címe a hello oldalán **tartomány** mezőben, majd illessze be hello **függő entitás Realm**  hello Eclipse párbeszédpanel mezőjében.
9. Hello belül **biztonsági** hello Eclipse párbeszédpanel, ha azt szeretné, hogy egy meglévő tanúsítvány toouse részén kattintson **Tallózás**, keresse meg a toohello tanúsítvány toouse szeretne, válassza ki azt, és kattintson  **Nyissa meg**. Vagy, ha azt szeretné, hogy toocreate egy új tanúsítványt, kattintson a **új** toodisplay hello **új tanúsítvány** párbeszédpanelen, majd adja meg a hello jelszó hello .cer fájl neve és hello .pfx-fájlt az új hello neve tanúsítvány.
10. Tartsa **beágyazási hello tanúsítvány hello WAR-fájlt a** be van jelölve, feltéve, hogy azt szeretné, hogy tooembed hello tanúsítványt hello WAR-fájlt.
11. [Választható] Tartsa **szükséges HTTPS-kapcsolatok** be van jelölve. Ha ezt a beállítást, szüksége lesz tooaccess az alkalmazás hello HTTPS protokoll használatával. Ha nem szeretné toorequire HTTPS-kapcsolatok, törölje ezt a beállítást.
12. A központi telepítési tooAzure az Azure ACS szűrési beállítások hasonló toohello következő fog kinézni.
    
    ![Éles környezet az Azure ACS szűrő beállításai][add_acs_filter_lib_production]
13. Kattintson a **Befejezés** tooclose hello **szerkesztése könyvtár** párbeszédpanel.
14. Kattintson a **OK** tooclose hello **MyACSHelloWorld tulajdonságainak** párbeszédpanel.
15. Az eclipse-ben, kattintson a hello **közzététele a felhőalapú tooAzure** gombra. Válasz toohello kér, hasonló módon végezhető el hello **toodeploy az alkalmazás tooAzure** hello szakasza [egy Hello World alkalmazásról az Azure létrehozása az eclipse-ben](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) témakör. 

Miután a webes alkalmazás telepítve van, zárjon be minden nyitott böngésző-munkamenetet, futtassa a webes alkalmazás és a rendszer kéri a toosign be Windows Live ID hitelesítő adataival, követ toohello küldi el a függő entitás alkalmazás URL-címe térjen vissza.

Ha végzett toodelete hello központi telepítés használatával az ACS Hello World alkalmazásról, ne feledje (áttekintheti, hogyan toodelete hello a központi telepítés [egy Hello World alkalmazásról az Azure létrehozása az eclipse-ben](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) témakör).

## <a name="next_steps"></a>Következő lépések
A Security Assertion Markup Language (SAML) ACS tooyour alkalmazás által visszaadott hello vizsgálata, lásd: [hogyan tooview SAML hello Azure Access Control Service által visszaadott][How tooview SAML returned by hello Azure Access Control Service]. toofurther ACS azon funkcióit és az összetettebb forgatókönyveket tooexperiment című [Access Control Service 2.0][Access Control Service 2.0].

Emellett a példában hello **beágyazási hello tanúsítvány hello WAR-fájlt a** lehetőséget. Ez a beállítás teszi egyszerű toodeploy hello tanúsítványt. Ha ehelyett tookeep az aláíró tanúsítványban elkülönül a WAR-fájlt, használhatja a következő eljárás hello:

1. Hello belül **biztonsági** hello szakasza **Azure Access Control szolgáltatások szűrő** párbeszédpanel, írja be **${env. JAVA_HOME}/MyCert.cer** és törölje a jelet **beágyazási hello tanúsítvány hello WAR-fájlt a**. (Beállítása mycert.cer Ha a tanúsítvány neve eltér.) Kattintson a **Befejezés** tooclose hello párbeszédpanel.
2. Másolás hello tanúsítványt a központi telepítésben összetevőjeként: bontsa ki a Eclipse Project Explorer **MyAzureACSProject**, kattintson a jobb gombbal **WorkerRole1**, kattintson a **tulajdonságok** , bontsa ki a **Azure szerepkör**, és kattintson a **összetevők**.
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. Hello belül **összetevő felvétele** párbeszédpanel:
   
   1. A hello **importálási** szakasz:
      1. Használjon hello **fájl** gomb toonavigate toohello tanúsítványt toouse. 
      2. A **metódus**, jelölje be **másolási**.
   2. A **mint neve**, kattintson a hello szövegmezőbe, és fogadja el hello alapértelmezett nevet.
   3. A hello **telepítés** szakasz:
      1. A **metódus**, jelölje be **másolási**.
      2. A **toodirectory**, típus **JAVA_HOME %**.
   4. A **összetevő felvétele** párbeszédpanel alábbihoz hasonló toohello következő.
      
       ![Tanúsítvány-összetevő hozzáadása][add_cert_component]
   5. Kattintson az **OK** gombra.

Ezen a ponton a tanúsítvány szerepel a központi telepítés. Vegye figyelembe, függetlenül attól, hogy hello tanúsítvány beágyazása hello WAR-fájlt, vagy adja hozzá azt egy összetevő tooyour telepítési, telepíteni kell, amely tooupload hello tanúsítványnévtéren tooyour hello leírtak [tooyour ACS névtértanúsítványfeltöltése] [ Upload a certificate tooyour ACS namespace] szakasz.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Upload a certificate tooyour ACS namespace]: #upload-certificate
[Review hello Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add hello ACS Filter library tooyour application]: #add_acs_filter_library
[Deploy toohello compute emulator]: #deploy_compute_emulator
[Deploy tooAzure]: #deploy_azure
[Next steps]: #next_steps
[project website]: http://wastarterkit4java.codeplex.com/releases/view/61026
[How tooview SAML returned by hello Azure Access Control Service]: active-directory-java-view-saml-returned-by-access-control.md
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure Management Portal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png

