---
title: "az Azure AD alkalmazásproxy aaaEnable távelérési tooSharePoint |} Microsoft Docs"
description: "Ismerteti hogyan hello alapjairól toointegrate egy helyszíni SharePoint server az Azure AD alkalmazásproxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 6ab413462fcaf387e150449df9c97505c4108bcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-access-toosharepoint-with-azure-ad-application-proxy"></a>Távelérés tooSharePoint az Azure AD alkalmazásproxy engedélyezése

Ez a cikk azt ismerteti, hogyan toointegrate egy helyszíni SharePoint server az Azure Active Directory (Azure AD) alkalmazásproxy.

tooenable távelérési tooSharePoint az Azure AD-alkalmazásproxy, kövesse a cikk részletes hello szakaszai.

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk feltételezi, hogy már rendelkezik a SharePoint 2013 vagy újabb a környezetben. Emellett vegye figyelembe a következő előfeltételek hello:

* SharePoint natív Kerberos támogatja. Ezért elérő belső helyek távolról keresztül az Azure AD-alkalmazásproxyval oldható felhasználók számára egy egyszeri bejelentkezés (SSO) tapasztal toohave veheti fel.

* Toomake néhány konfigurációs módosítások tooyour SharePoint server szükséges. Átmeneti környezet használatát javasoljuk. Így teheti frissítések tooyour server először átmeneti, és majd lehetővé teszi egy tesztelési ciklust, mielőtt éles környezetben.

* Feltételezzük, hogy Ön már beállított SSL a SharePoint, mert a hello SSL közzétett URL-cím szükséges. A belső webhelyen, hogy hivatkozások küldött/leképezve megfelelően tooensure toohave SSL Protokollt engedélyezni kell. Ha SSL még nincs konfigurálva, lásd: [SSL konfigurálása a SharePoint 2013 rendszerhez](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) utasításokat. Ne felejtse hello összekötő gép bizalmi kapcsolatok hello tanúsítvány, amely kiadása. (a hello tanúsítványt nem kell nyilvánosan kiadott toobe.)

## <a name="step-1-set-up-single-sign-on-toosharepoint"></a>1. lépés: Az egyszeri bejelentkezés tooSharePoint beállítása

Ügyfeleink hello legjobb egyszeri Bejelentkezéses felhasználói élmény a háttér-alkalmazások, a SharePoint server ebben az esetben érdemes. Ilyenkor közös az Azure AD hello felhasználó hitelesítése csak egyszer, mert nem kéri újra hitelesítéshez.

A helyszíni alkalmazások esetében szükséges, vagy a Windows-hitelesítés használatára egyszeri Bejelentkezéses hello Kerberos hitelesítési protokoll és a Kerberos által korlátozott delegálás (KCD) nevű funkció segítségével érhet el. A Kerberos által korlátozott, ha konfigurálva van, lehetővé teszi, hogy hello Application Proxy connector tooobtain windows jegy/a felhasználó jogkivonatát, még akkor is, ha hello nincs bejelentkezett felhasználó tooWindows közvetlenül. toolearn Kerberos által korlátozott Delegálás, kapcsolatos további információkért lásd: [Kerberos által korlátozott delegálás áttekintése](https://technet.microsoft.com/library/jj553400.aspx).

SharePoint-kiszolgáló esetén használjon hello eljárások a következő, egymást követő szakaszok hello fel Kerberos által korlátozott Delegálás tooset:

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a>Győződjön meg arról, hogy a SharePoint szolgáltatásfiók alatt fut.

Először is győződjön meg arról, hogy a meghatározott szolgáltatási fiókkal--nem helyi rendszer, a helyi szolgáltatás vagy a hálózati szolgáltatás fut-e a SharePoint. Ehhez úgy, hogy csatolhat a szolgáltatás egyszerű szolgáltatásnevét (SPN) tooa érvényes fiókot. SPN-EK, hogyan hello Kerberos protokoll azonosítja a különböző szolgáltatások. Akkor lesz szüksége hello fiók és későbbi tooconfigure hello Kerberos által korlátozott Delegálás.

tooensure, amely egy meghatározott szolgáltatási fiókkal futtatja a helyek hajtsa végre a lépéseket követve hello:

1. Nyissa meg hello **SharePoint 2013 központi felügyelet** hely.
2. Nyissa meg túl**biztonsági** válassza **szolgáltatásfiókok konfigurálása**.
3. Válassza ki **webalkalmazás-készlet - SharePoint - 80**. hello-beállítások kis mértékben eltérő lehet a webalkalmazás-készlet hello neve alapján, vagy ha hello webes készlet alapértelmezés szerint az SSL használja.

  ![Lehetőségek a szolgáltatásfiók konfigurálása](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-web-application.png)

4. Ha **válassza ki a fiókot ehhez az összetevőhöz** van **helyi szolgáltatás** vagy **hálózati szolgáltatás**, toocreate fiókkal van szüksége. Ha nem, akkor végzett, és továbbléphet a következő szakasz toohello.
5. Válassza ki **új felügyelt fiók regisztrálása**. A fiók létrehozása után meg kell adni **webes alkalmazáskészlet** hello fiók használata előtt.

> [!NOTE]
A korábban toohave kell hello szolgáltatást az Azure AD-fiókot létrehozni. Javasoljuk, hogy lehetővé tegye egy automatikus jelszóváltoztatáshoz. A lépéseket és a hibaelhárítási problémák hello teljes készletének kapcsolatos további információkért lásd: [konfigurálása automatikus jelszó módosítása a SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).

### <a name="configure-sharepoint-for-kerberos"></a>A Kerberos a SharePoint konfigurálása

Kerberos által korlátozott Delegálás tooperform egyszeri bejelentkezés toohello SharePoint server használatakor, és ez csak Kerberos működik.

a SharePoint-webhely a Kerberos-hitelesítést tooconfigure:

1. Nyissa meg hello **SharePoint 2013 központi felügyelet** hely.
2. Nyissa meg túl**Alkalmazáskezelés**, jelölje be **webes alkalmazásokat kezeléséhez**, és válassza ki a SharePoint-webhelyét. Az ebben a példában is **SharePoint - 80**.

  ![Hello SharePoint-webhely kiválasztása](./media/application-proxy-remote-sharepoint/remote-sharepoint-manage-web-applications.png)

3. Kattintson a **hitelesítésszolgáltatókat** hello eszköztáron.
4. A hello **hitelesítésszolgáltatókat** kattintson **alapértelmezett zóna** tooview hello beállításait.
5. A hello **hitelesítés szerkesztése** párbeszédpanel mezőben görgessen lefelé, amíg megjelenik **jogcímek hitelesítési típusok** , és győződjön meg arról, hogy mindkét **Windows-hitelesítés engedélyezése** és  **Integrált Windows-hitelesítés** van kiválasztva.
6. Hello legördülő mezőben, ügyeljen arra, hogy **egyeztetés (Kerberos)** van kiválasztva.

  ![Hitelesítés szerkesztése párbeszédpanel](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-edit-authentication.png)

7. Hello hello alján **hitelesítés szerkesztése** párbeszédpanel, kattintson a **mentése**.

### <a name="set-a-service-principal-name-for-hello-sharepoint-service-account"></a>Egy egyszerű szolgáltatásnevét hello SharePoint szolgáltatásfiók beállítása

Hello Kerberos által korlátozott Delegálás konfigurálásához meg kell tooidentify hello SharePoint szolgáltatást futtató fiókként hello szolgáltatást, amelyet beállított. Ehhez úgy, hogy egy egyszerű Szolgáltatásnevet. További információkért lásd: [egyszerű szolgáltatásnevek](https://technet.microsoft.com/library/cc961723.aspx).

hello SPN formátuma:

```
<service class>/<host>:<port>
```

Hello SPN formátumban:

* _osztály szolgáltatás_ hello szolgáltatás egyedi neve. A SharePoint, használhat **HTTP**.

* _állomás_ hello teljesen minősített tartománynév vagy fut a szolgáltatás hello hello gazdagép NetBIOS-nevét. A SharePoint-webhelyre Ez a szöveg módosítania kell toobe hello webhely URL-címe hello, attól függően, hogy hello verzióját, amelyen az IIS.

* _port_ nem kötelező megadni.

Ha hello hello SharePoint-kiszolgáló teljes Tartományneve:

```
sharepoint.demo.o365identity.us
```

Majd hello SPN van:

```
HTTP/ sharepoint.demo.o365identity.us demo
```

Szükség lehet tooset SPN-ek beállítása adott webhelyekhez a kiszolgálón. További információkért lásd: [konfigurálja a Kerberos-hitelesítés](https://technet.microsoft.com/library/cc263449(v=office.12).aspx). Fizessen elolvassa toohello szakasz "Create egyszerű szolgáltatásnevek a webes alkalmazásokhoz, Kerberos-hitelesítést használ."

hello tooset SPN-ek a legkönnyebben toofollow hello SPN formátumok már jelen, a helyek lehetnek. Ezen elleni hello szolgáltatásfiók SPN-ek tooregister másolja. toodo ezt:

1. Keresse meg a másik gépről SPN hello toohello hely.
 Amikor, hello vonatkozó beállítva a Kerberos-jegyek hello gépen kerül a gyorsítótárba. Ezek a jegyek hello hello célhelyre vetítéséhez egyszerű Szolgáltatásnevének tartalmaznak.

2. Nevű eszköz használatával képes lekérni a hello egyszerű Szolgáltatásnevet az adott hely [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html). Egy parancsablakban, amely futtatja a hello ugyanabban a környezetben használt hello hely hello böngészőben futtató hello felhasználóként hello a következő parancsot:
```
Klist
```
Klist majd cél SPN-ek hello készletet ad vissza. Ebben a példában a kiemelt hello értéke hello SPN szükséges:

  ![Példa Klist eredménye](./media/application-proxy-remote-sharepoint/remote-sharepoint-target-service.png)

4. Most, hogy hello egyszerű Szolgáltatásnevet, van szüksége arról, hogy megfelelően van konfigurálva a webalkalmazás hello korábban beállított hello szolgáltatásfiók toomake. Futtassa a parancsot követő hello parancssorból hello tartományi rendszergazdaként hello:

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 Ez a parancs beállítása SPN hello hello SharePoint szolgáltatás fut, mint a fiók _demo\sp_svc_.

 Cserélje le _http/sharepoint.demo.o365identity.us_ a hello egyszerű Szolgáltatásnevet a kiszolgáló és _demo\sp_svc_ hello szolgáltatásfiókkal a környezetben. hello hello egyszerű Szolgáltatásnevet a Setspn parancs keres, mielőtt hozzáadja azt. Ebben az esetben előfordulhat, hogy megjelenik egy **ismétlődő SPN-értéket** hiba. Ha megjelenik ez a hiba, ellenőrizze, hogy hello érték hello szolgáltatás fiókhoz.

Ellenőrizheti, hogy hello SPN paranccsal hello Setspn -l kapcsolóval hello hozzá lett adva. További információk a parancs toolearn lásd [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-hello-connector-is-set-as-a-trusted-delegate-toosharepoint"></a>Győződjön meg arról, hogy hello összekötő be van állítva egy megbízható delegált tooSharePoint

Így hello Azure AD alkalmazásproxy-szolgáltatás delegálhatja a felhasználói identitások toohello SharePoint szolgáltatás hello Kerberos által korlátozott Delegálás konfigurálása Ehhez hello alkalmazásproxy-összekötő engedélyezésével tooretrieve Kerberos-jegyek a felhasználók, akik az Azure Active Directory hitelesített. Ezután, hogy a kiszolgáló továbbítja a hello környezetben toohello célalkalmazás vagy SharePoint ebben az esetben.

tooconfigure hello Kerberos által korlátozott Delegálás, a következő lépéseket az egyes csatlakozó gépek ismétlési hello:

1. Jelentkezzen be egy tartományi rendszergazda tooa tartományvezérlő, és nyisson **Active Directory – felhasználók és számítógépek**.
2. Hello számítógép, amely összekötő hello megkeresése Ebben a példában ez rendelkezik hello azonos SharePoint-kiszolgáló.
3. Kattintson duplán a hello számítógépet, majd hello **delegálás** fülre.
4. Győződjön meg arról, hogy túl van-e beállítva a hello a delegálási beállításokat**a számítógépen megadott delegálás toohello szolgáltatások csak**, majd válassza ki **bármely hitelesítési protokoll**.

  ![A delegálási beállításokat](./media/application-proxy-remote-sharepoint/remote-sharepoint-delegation-box.png)

5. Hello kattintson **Hozzáadás** gombra, majd **felhasználók vagy számítógépek**, és keresse meg a hello szolgáltatásfiók.

  ![Hello szolgáltatásfiók SPN hozzáadását hello](./media/application-proxy-remote-sharepoint/remote-sharepoint-users-computers.png)

6. Hello SPN-EK, jelölje ki valamelyik hello szolgáltatásfiók korábban létrehozott hello.
7. Kattintson az **OK** gombra. Kattintson a **OK** újra toosave hello módosításokat.

## <a name="step-2-enable-remote-access-toosharepoint"></a>2. lépés: A távelérés tooSharePoint engedélyezése

Most, hogy engedélyezve van a SharePoint a Kerberos és Kerberos által korlátozott Delegálás konfigurálva, készen áll a tooset egyszeri bejelentkezés tooSharePoint be most. Majd hello-összekötőről származó közzéteheti a távelérés szolgáltatás segítségével az Azure AD-alkalmazásproxy hello SharePoint-farm.

tooperform hello lépéseket követve toobe a szervezet Azure Active Directory-fiók globális rendszergazdai szerepkör hello tagja kell.

1. Jelentkezzen be toohello [Azure-portálon](https://manage.windowsazure.com) keresse meg az Azure AD-bérlő.
2. Kattintson a **alkalmazások**, és kattintson a **Hozzáadás**.
3. Válassza a **Publish an application that will be accessible from outside your network** (Hálózaton kívülről hozzáférhető alkalmazás közzététele) lehetőséget. Ha nem látja ezt a beállítást, ellenőrizze, hogy Azure AD alapvető rendelkezik, vagy Premium állítsa be a hello bérlői-e.
4. Végezze el hello lehetőségek az alábbiak szerint:
 * **Név**: használni kívánt – például értéket **SharePoint**.
 * **Belső URL-cím**: Ez az hello URL hello SharePoint-webhely belső, például **https://SharePoint/**. Ebben a példában, győződjön meg arról, hogy toouse **https**.
 * **Előhitelesítési módszer**: válasszon **az Azure Active Directory**.

  ![Egy alkalmazás hozzáadásának beállításai](./media/application-proxy-remote-sharepoint/remote-sharepoint-add-application.png)

5. Miután hello alkalmazás közzé van téve, kattintson a hello **konfigurálása** fülre.
6. Görgessen lefelé toohello beállítás **állomásneveket, az URL-címet fejlécek**. hello alapértelmezett értéke **Igen**. Módosítsa úgy túl**nem**.

 SharePoint használ hello _állomásfejléc_ érték toolook hello hely fel. Emellett ez az érték alapján hivatkozásokat hoz létre. hello nettó hatása az toomake arról, hogy minden hivatkozás, amely SharePoint-hoz létre egy közzétett URL-címet, amely toouse hello külső URL-cím helyesen van beállítva. Hello érték túl**Igen** is lehetővé teszi, hogy hello összekötő tooforward hello kérelem toohello háttér-alkalmazást. Azonban érték hello túl**nem** azt jelenti, hogy az összekötő hello nem küld hello belső állomásnevet. Ehelyett a hello összekötő hello állomásfejléc küldi hello közzétett URL-cím toohello háttér-alkalmazás.

 Emellett tooensure, hogy a SharePoint fogadja el az URL-cím, egy további konfigurációs hello SharePoint-kiszolgálón kell toocomplete. Azt teheti a következő szakaszban hello.

7. Változás **belső hitelesítési módszer** túl**integrált Windows-hitelesítés**. Ha az Azure AD-bérlőről egy egyszerű felhasználónév, amely nem egyezik a hello UPN helyszíni hello felhőben használ, ne feledje tooupdate **meghatalmazott bejelentkezési identitás** is.
8. Állítsa be **belső alkalmazás SPN** toohello korábban beállított értékeket. Tegyük fel például, **http/sharepoint.demo.o365identity.us**.
9. Rendelje hozzá a hello alkalmazás tooyour azon felhasználók megcélzása.

Az alkalmazás a következő példa hasonló toohello kell kinéznie:

  ![Befejezett alkalmazás](./media/application-proxy-remote-sharepoint/remote-sharepoint-internal-application-spn.png)

## <a name="step-3-ensure-that-sharepoint-knows-about-hello-external-url"></a>3. lépés: Győződjön meg arról, hogy a SharePoint ismer hello külső URL-címe

Az utolsó lépés tooensure, amely SharePoint található hello helyhez hello külső URL-cím alapján lesz jeleníti meg hivatkozások alapján a külső URL-címet. Ehhez másodlagos címek leképezése hello SharePoint-webhely konfigurálása.

1. Nyissa meg hello **SharePoint 2013 központi felügyelet** hely.
2. A **rendszerbeállítások**, jelölje be **alternatív leképezések konfigurálása**.

 Ekkor megnyílik a hello **másodlagos hozzáférési megfeleltetések** mezőbe.

  ![Másodlagos hozzáférési megfeleltetések mezőbe](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access1.png)

3. Hello legördülő lista melletti **másodlagos hozzáférési leképezési gyűjtemény**, jelölje be **módosítás másodlagos hozzáférési leképezési gyűjtemény**.
4. Válassza ki a webhely – például **SharePoint - 80**.

  ![A hely kiválasztása](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access2.png)

5. Kiválaszthatja a tooadd hello közzétett URL-címet vagy egy belső URL-CÍMÉT, vagy egy nyilvános URL-címet. Ez a példa extranetes hello egy nyilvános URL-címet használja.
6. Kattintson a **szerkesztése nyilvános URL-címek** a hello **Extranet** elérési utat, majd adja meg hello elérési útját hello közzétett alkalmazás, ahogy hello előző lépésben. Adja meg például **https://sharepoint-iddemo.msappproxy.net**.

  ![Hello elérési út megadása](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access3.png)

7. Kattintson a **Save** (Mentés) gombra.

 SharePoint-webhely hello kívülről az Azure AD-alkalmazásproxy keresztül érhetők el.

## <a name="next-steps"></a>Következő lépések

- [Hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások](active-directory-application-proxy-get-started.md)
- [Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md)
- [A SharePoint 2016 és Office Online Server az Azure AD-alkalmazásproxy közzététele](https://blogs.technet.microsoft.com/dawiese/2016/06/09/publishing-sharepoint-2016-and-office-online-server-with-azure-ad-application-proxy/)
