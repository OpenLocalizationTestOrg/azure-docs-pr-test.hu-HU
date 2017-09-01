## <a name="what-are-service-bus-topics-and-subscriptions"></a>Mik azok a Service Bus-üzenettémák és -előfizetések?
A Service Bus-üzenettémák és -előfizetések *közzétételi/előfizetési* modellt biztosítanak az üzenettovábbításhoz. Üzenettémák és előfizetések használata esetén az elosztott alkalmazások összetevői nem közvetlenül egymással kommunikálnak, hanem egy közvetítőként szolgáló üzenettémakörön keresztül.

![Az üzenettémakörök alapfogalmai](./media/howto-service-bus-topics/sb-topics-01.png)

A Service Bus-üzenetsorokkal ellentétben, amely esetében minden üzenetet egy-egy fogyasztó dolgoz fel, az üzenettémák és előfizetések a közzététel/előfizetés minta használatával „egy a többhöz” típusú kommunikációt biztosítanak. Egy üzenettémakörhöz több előfizetést is lehet regisztrálni. Ha egy üzenetet elküldenek egy témakörbe, az összes előfizetés számára elérhetővé válik, amelyek egymástól függetlenül kezelhetik és dolgozhatják fel az üzenetet.

Az egyes témakörökre való előfizetés egy virtuális üzenetsorra hasonlít, amely minden, a témakörnek elküldött üzenetről kap egy másolatot. Lehetősége van szűrőszabályok előfizetésenként történő beállítására is, amelyek lehetővé teszik annak szűrését vagy korlátozását, hogy az adott témakörbe beérkezett üzenetet melyik előfizetések kaphatják meg.

A Service Bus-üzenettémák és -előfizetések lehetővé teszik a számos felhasználótól és alkalmazásból érkező, hatalmas mennyiségű üzenet méretezését és feldolgozását.

## <a name="create-a-namespace"></a>Névtér létrehozása
A Service Bus-üzenettémák és -előfizetések Azure-ban való használatához először létre kell hoznia egy *szolgáltatásnévteret*. A névtér egy hatókörkezelési tárolót biztosít a Service Bus erőforrásainak címzéséhez az alkalmazáson belül.

Névtér létrehozása:

1. Jelentkezzen be az [Azure Portalra][Azure portal].
2. A portál bal oldali navigációs paneljén kattintson az **Új**, majd a **Enterprise Integration** (Vállalati integráció), végül a **Service Bus** elemre.
3. A **Névtér létrehozása** párbeszédpanelen adja meg a névtér nevét. A rendszer azonnal ellenőrzi, hogy a név elérhető-e.
4. Miután ellenőrizte, hogy a névtér neve elérhető-e, válassza ki a tarifacsomagot (Basic, Standard vagy Prémium).
5. Az **Előfizetés** mezőben válassza ki azt az Azure-előfizetést, amelyben a névteret létre kívánja hozni.
6. Az **Erőforráscsoport** mezőben válasszon ki egy meglévő erőforráscsoportot, amelynek a névtér a része lesz, vagy hozzon létre egy újat.      
7. A **Hely** mezőben válassza ki azt az országot vagy régiót, amelyben a névtér üzemeltetve lesz.
   
    ![Névtér létrehozása][create-namespace]
8. Kattintson a **Létrehozás** gombra. A rendszer ekkor létrehozza és engedélyezi a névteret. Előfordulhat, hogy néhány percet várnia kell, amíg a rendszer kiosztja az erőforrásokat a fiókja számára.

### <a name="obtain-the-credentials"></a>Hitelesítő adatok beszerzése
1. A névterek listájában kattintson az újonnan létrehozott névtér nevére.
2. A **Service Bus-névtér** panelen kattintson a **Megosztott elérési házirendek** elemre.
3. A **Megosztott elérési házirendek** panelen kattintson a **RootManageSharedAccessKey** elemre.
   
    ![connection-info][connection-info]
4. A **Házirend: RootManageSharedAccessKey** panelen a **Kapcsolati karakterlánc – elsődleges kulcs** melletti másolás gombra kattintva másolja a kapcsolati karakterláncot a vágólapra későbbi használatra.
   
    ![connection-string][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


