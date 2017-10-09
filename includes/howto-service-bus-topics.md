## <a name="what-are-service-bus-topics-and-subscriptions"></a>Mik azok a Service Bus-üzenettémák és -előfizetések?
A Service Bus-üzenettémák és -előfizetések *közzétételi/előfizetési* modellt biztosítanak az üzenettovábbításhoz. Üzenettémák és előfizetések használata esetén az elosztott alkalmazások összetevői nem közvetlenül egymással kommunikálnak, hanem egy közvetítőként szolgáló üzenettémakörön keresztül.

![Az üzenettémakörök alapfogalmai](./media/howto-service-bus-topics/sb-topics-01.png)

A Service Bus-üzenetsorokkal ellentétben, amely esetében minden üzenetet egy-egy fogyasztó dolgoz fel, az üzenettémák és előfizetések a közzététel/előfizetés minta használatával „egy a többhöz” típusú kommunikációt biztosítanak. Akkor lehet több előfizetések tooa témakör regisztrálni. Egy üzenet elküldésekor tooa témakör, majd teszi elérhetővé tooeach előfizetés toohandle/folyamat függetlenül.

Egy előfizetés tooa témakör hasonlít egy virtuális üzenetsorra, amely hello távozó üzeneteinek toohello témakör példányait megkapja. Opcionálisan Állapotszűrő szabályok témakör, amely lehetővé teszi toofilter előfizetésenként alapon regisztrálja, vagy korlátozhatja, hogy mely üzenetek tooa témakör melyik előfizetések kaphatják által fogadott.

Service Bus-üzenettémák és előfizetések lehetővé teszik a tooscale, és számos felhasználótól és alkalmazások nagyon nagy számú üzenetek feldolgozásához.

## <a name="create-a-namespace"></a>Névtér létrehozása
az Azure Service Bus-üzenettémák és előfizetések használata toobegin létre kell hoznia egy *szolgáltatásnévtér*. A névtér egy hatókörkezelési tárolót biztosít a Service Bus erőforrásainak címzéséhez az alkalmazáson belül.

egy névtér toocreate:

1. Jelentkezzen be toohello [Azure-portálon][Azure portal].
2. Hello portal hello bal oldali navigációs ablaktábláján kattintson **új**, majd kattintson a **vállalati integrációs**, és kattintson a **Service Bus**.
3. A hello **névtér létrehozása** párbeszédpanelen adja meg a névtér nevét. hello rendszer azonnal ellenőrzi toosee, ha hello neve érhető el.
4. Miután meggyőződött arról hello név nem érhető el, válassza ki azt a hello tarifacsomag (alapszintű, Standard vagy prémium).
5. A hello **előfizetés** mezőben válassza ki az Azure-előfizetés mely toocreate hello névtérben.
6. A hello **erőforráscsoport** mezőben válasszon egy meglévő erőforráscsoportot, mely hello névtér lesz élő, vagy hozzon létre egy újat.      
7. A **hely**, válassza ki a hello országban vagy régióban, amelyben a névtér üzemeltetve lesz.
   
    ![Névtér létrehozása][create-namespace]
8. Kattintson a hello **létrehozása** gombra. hello rendszer most hoz létre a névteret, majd engedélyezi. Lehetséges, hogy toowait hello rendszer kiosztja az erőforrásokat, a fiók néhány percig.

### <a name="obtain-hello-credentials"></a>Hello hitelesítő adatok beszerzése
1. Hello névterek, kattintson az újonnan létrehozott névtér neve hello.
2. A hello **Service Bus-névtér** panelen kattintson a **megosztott elérési házirendek**.
3. A hello **megosztott elérési házirendek** panelen kattintson a **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. A hello **házirend: RootManageSharedAccessKey** panelen kattintson a Másolás gombra hello tovább túl**kapcsolati karakterlánc – elsődleges kulcs**, toocopy hello kapcsolati karakterlánc tooyour vágólapra későbbi használatra.
   
    ![connection-string][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


