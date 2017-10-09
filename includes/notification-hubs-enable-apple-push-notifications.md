

## <a name="generate-hello-certificate-signing-request-file"></a>Hello tanúsítvány-aláírási kérelem fájljának létrehozása
hello Apple leküldéses értesítési (APN) szolgáltatás által használt tanúsítványok tooauthenticate a leküldéses értesítések. Kövesse az alábbi utasításokat toocreate hello szükséges leküldéses tanúsítvány toosend, és értesítéseket kaphat. További információ ezekről a fogalmakról: hello hivatalos [Apple Push Notification szolgáltatás](http://go.microsoft.com/fwlink/p/?LinkId=272584) dokumentációját.

Hozza létre hello tanúsítvány-aláírási kérelem (CSR) fájlját, amely egy aláírt leküldéses tanúsítványt az Apple toogenerate használják.

1. A Mac gépen futtassa a kulcskarika-elérés eszközt hello. A hello megnyitható **segédprogramok** mappa vagy hello **más** hello launchpadről mappájába.
2. Kattintson a **Kulcskarika-elérés** menüpontra, bontsa ki a **Tanúsítványasszisztens** almenü, majd kattintson a **Tanúsítvány igénylése egy tanúsítványkiadó központból...** lehetőségre.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Válassza ki a **felhasználó E-mail címét** és **köznapi név** , győződjön meg arról, hogy **toodisk mentett** van kiválasztva, és kattintson **Folytatás**. Hagyja hello **CA E-mail-cím** mező üres, mert nincs rá szükség.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. Írja be a hello tanúsítvány-aláírási kérelem (CSR) fájl nevét **Mentés másként**, válassza ki azt a hello helyet **ahol**, majd kattintson a **mentése**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      Ez menti hello CSR-fájlt a kijelölt hello location; hello alapértelmezett helye a hello asztali. Ne feledje, hogy a fájl választott hello helyet.

A következő, regisztrálja az alkalmazást az Apple rendszerében, engedélyezi leküldéses értesítéseket, és töltse fel az exportált CSR toocreate leküldéses tanúsítvány.

## <a name="register-your-app-for-push-notifications"></a>Alkalmazás regisztrálása leküldéses értesítésekhez
toobe képes toosend leküldéses értesítések tooan iOS-alkalmazás, regisztrálnia kell az alkalmazás az Apple, és a leküldéses értesítések regisztrálása.  

1. Ha már nincs regisztrálva az alkalmazás, keresse meg a toohello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> hello Apple Developer Center központban, jelentkezzen be az Apple-Azonosítójával, kattintson **azonosítók**, majd kattintson a **Alkalmazásazonosítók** , végül kattintson a hello  **+**  tooregister egy új alkalmazások aláírásához.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
      
2. A következő három mezőt az új alkalmazás hello frissítést, majd kattintson **Folytatás**:
   
   * **Név**: Adjon egy leíró nevet az alkalmazás a hello **neve** hello mezőjét **App ID leírás** szakasz.
   * **Azonosító csomagot**: hello alatt **Explicit Alkalmazásazonosító** területen adja meg a **Bundle Identifier** hello formában `<Organization Identifier>.<Product Name>` a hello [alkalmazások terjesztése Az útmutató](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Hello *Organization Identifier* és *Terméknév* kell használnia megfelel-e hello szervezeti azonosító és a termék neve, az XCode projektje létrehozásakor fog használni. Az alábbi képernyőképen hello *NotificationHubs* használt szervezetazonosító és *GetStarted* hello terméknév használatos. Meggyőződött arról, hogy ez megegyezik, használhatja az XCode projektjében hello értékek lehetővé teszi az Ön toouse hello helyes közzétételi profilt az XCode. 
   * **Leküldéses értesítések**: ellenőrzés hello **leküldéses értesítések** hello beállítása **alkalmazásszolgáltatások** területen.
     
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
      Ezzel létrejön az Alkalmazásazonosító, és megkéri Önt tooconfirm hello információkat. Kattintson a **regisztrálása** tooconfirm hello új alkalmazást.
     
      Gombra kattintva **regisztrálása**, látni fogja a hello **végezze el a regisztrációs** képernyőn, a lent látható módon. Kattintson a **Done** (Kész) gombra.
      
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. A hello fejlesztői központban az Alkalmazásazonosítók alatt keresse meg az imént létrehozott, és kattintson az azt tartalmazó sorra hello alkalmazás azonosítója.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
      Kattintson a hello Alkalmazásazonosító hello alkalmazás adatait jeleníti meg. Kattintson a hello **szerkesztése** hello alsó gombra.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
      
2. Görgessen toohello hello képernyő aljára, és kattintson a hello **tanúsítvány létrehozása...**  hello szakaszban gomb **Development Push SSL-tanúsítvány**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
      Ez megjeleníti a hello "IOS tanúsítvány hozzáadása" segéd.
   
   > [!NOTE]
   > Ez az oktatóprogram fejlesztési tanúsítványt használ. hello ugyanaz a folyamat használatos a termelési tanúsítvány regisztrálásához. Ne feledje, hogy használja-e hello azonos tanúsítványtípus értesítések küldéséhez.
   > 
   > 
3. Kattintson a **Choose File**, keresse meg a toohello helyet, ahová mentette hello első feladatban létrehozott hello CSR-fájlt, majd kattintson a **Generate**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. Hello tanúsítvány hello portál által a létrehozása után kattintson a hello **letöltése** gombra, és kattintson a **végzett**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
      Ez letölti a hello tanúsítványt, és a Letöltések mappába tooyour számítógép menti.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > Alapértelmezés szerint hello letöltött fejlesztési tanúsítványfájl neve fájl **aps_development.cer**.
   > 
   > 
5. Kattintson duplán a letöltött leküldéses tanúsítvány hello **aps_development.cer**.
   
      Ez telepíti hello új tanúsítványt hello Kulcsláncba, az alább látható módon:
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > hello a tanúsítványban található név lehet különböző, de rendelkezni fog az **Apple Development iOS Push Services:**.
   > 
   > 
6. A kulcslánc-hozzáférési, kattintson a jobb gombbal hello új leküldéses tanúsítványra hello létrehozott **tanúsítványok** kategóriát. Kattintson a **exportálása**, név: hello fájlt, jelölje ki a hello **.p12** formázása, és kattintson a **mentése**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    Győződjön meg a Megjegyzés hello fájlnév és hello exportált .p12 tanúsítvány helyét. Az apns-sel használt tooenable hitelesítési lesz.
   
   > [!NOTE]
   > Ez az oktatóprogram egy QuickStart.p12 fájlt hoz létre. Az Ön fájljának neve és helye eltérhet ettől.
   > 
   > 

## <a name="create-a-provisioning-profile-for-hello-app"></a>Hozzon létre egy létesítési profilt hello alkalmazás
1. Vissza a hello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>, jelölje be **létesítési profilok**, jelölje be **összes**, és kattintson a hello  **+**  gomb toocreate új profil. Ekkor elindul a hello **iOS Provisiong profil hozzáadása** varázsló
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. Válassza ki **iOS App Development** alatt **fejlesztési** elemet hello provisiong profil típusát, és kattintson **Folytatás**. 
3. Ezután válassza ki az imént létrehozott hello hello Alkalmazásazonosító **Alkalmazásazonosító** legördülő listából, és kattintson **Folytatás**
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. A hello **válassza ki a tanúsítványok** képernyőn, válassza ki a kódaláíráshoz használt szokásos fejlesztési tanúsítványt, és kattintson **Folytatás**. Ez az imént létrehozott hello leküldéses tanúsítvány.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. Ezután válassza ki a hello **eszközök** toouse tesztelése, és kattintson a **Folytatás**
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. Végül adjon meg nevet a hello-profil **profilnév**, kattintson a **Generate**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. Új létesítési profil hello létrehozásakor kattintson toodownload, és telepítse az xcode-ban fejlesztési számítógépen. Ezután kattintson a **Done** (Kész) gombra.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
