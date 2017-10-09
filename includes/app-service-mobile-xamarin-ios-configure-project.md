#### <a name="configure-hello-ios-project-in-xamarin-studio"></a>A Xamarin Studio hello iOS-projekt konfigurálása
1. Xamarin.Studio, nyissa meg **Info.plist**, és a frissítés hello **Bundle Identifier** hello a csomagot az új alkalmazást. a korábban létrehozott azonosító

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Görgessen lefelé, túl**Háttérmódok**. Jelölje be hello **Háttérmódok engedélyezése** mezőbe, majd hello **távoli értesítések** mezőbe.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Kattintson duplán a projektre a hello megoldás Panel tooopen **Projektbeállítások**.
4. A **Build**, válassza ki **iOS alkalmazáscsomag aláírása**, és válassza ki a megfelelő identitás hello és létesítési profil imént beállított ebben a projektben.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Ez biztosítja, hogy adott hello project kódaláíró hello új profilt használja. Hello hivatalos Xamarin eszköz-üzembehelyezési dokumentáció, lásd: [Xamarin eszköz kiépítése].

#### <a name="configure-hello-ios-project-in-visual-studio"></a>A Visual Studio hello iOS-projekt konfigurálása
1. A Visual Studióban, kattintson a jobb gombbal a hello projektet, és kattintson **tulajdonságok**.
2. Hello tulajdonságai lapon kattintson a hello **iOS alkalmazás** fülre, majd a frissítés hello **azonosító** korábban létrehozott hello azonosítóval.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. A hello **iOS alkalmazáscsomag aláírása** lapra, jelölje be hello megfelelő identitás létesítési profil és Ön imént beállított fel ebben a projektben.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Ez biztosítja, hogy adott hello project kódaláíró hello új profilt használja. Hello hivatalos Xamarin eszköz-üzembehelyezési dokumentáció, lásd: [Xamarin eszköz kiépítése].
4. Kattintson duplán az Info.plist tooopen és majd engedélyezése **RemoteNotifications** alatt **Háttérmódok**.

[Xamarin eszköz kiépítése]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
