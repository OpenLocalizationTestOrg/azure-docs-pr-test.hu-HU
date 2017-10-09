a Service Bus használatával toobegin várólisták az Azure-ban, akkor először létre kell hoznia egy névtér esetében Azure egyedi névvel. A névtér egy hatókörkezelési tárolót biztosít a Service Bus erőforrásainak címzéséhez az alkalmazáson belül.

egy névtér toocreate:

1. Jelentkezzen be toohello [Azure-portálon][Azure portal].
2. Hello portal hello bal oldali navigációs ablaktábláján kattintson **új**, majd kattintson a **vállalati integrációs**, és kattintson a **Service Bus**.
3. A hello **névtér létrehozása** párbeszédpanelen adja meg a névtér nevét. hello rendszer azonnal ellenőrzi toosee, ha hello neve érhető el.
4. Miután meggyőződött arról hello név nem érhető el, válassza ki azt a hello tarifacsomag (alapszintű, Standard vagy prémium).
5. A hello **előfizetés** mezőben válassza ki az Azure-előfizetés mely toocreate hello névtérben.
6. A hello **erőforráscsoport** mezőben válasszon egy meglévő erőforráscsoportot, mely hello névtér lesz élő, vagy hozzon létre egy újat.      
7. A **hely**, válassza ki a hello országban vagy régióban, amelyben a névtér üzemeltetve lesz.
   
    ![Névtér létrehozása][create-namespace]
8. Kattintson a **Create** (Létrehozás) gombra. hello rendszer most hoz létre a névteret, majd engedélyezi. Lehetséges, hogy toowait hello rendszer kiosztja az erőforrásokat, a fiók néhány percig.

### <a name="obtain-hello-management-credentials"></a>Hello felügyeleti hitelesítő adatok beszerzése

1. Hello névterek, kattintson az újonnan létrehozott névtér neve hello.
2. Hello névtér paneljén kattintson **megosztott elérési házirendek**.
3. A hello **megosztott elérési házirendek** panelen kattintson a **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. A hello **házirend: RootManageSharedAccessKey** panelen kattintson a Másolás gombra hello tovább túl**kapcsolati karakterlánc – elsődleges kulcs**, toocopy hello kapcsolati karakterlánc tooyour vágólapra későbbi használatra. Illessze be ezt az értéket a Jegyzettömbbe vagy egy másik ideiglenes helyre.
   
    ![connection-string][connection-string]

5. Ismétlődő hello előző lépést, másolás és beillesztés hello értékének **elsődleges kulcs** tooa ideiglenes helyre későbbi használatra.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
