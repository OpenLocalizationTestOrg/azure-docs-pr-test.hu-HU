1. Jelentkezzen be toohello [Azure-portálon][Azure portal].
2. Hello portal hello bal oldali navigációs ablaktábláján kattintson **új**, majd kattintson a **vállalati integrációs**, és kattintson a **továbbítási**.
3. A hello **névtér létrehozása** párbeszédpanelen adja meg a névtér nevét. hello rendszer azonnal ellenőrzi toosee, ha hello neve érhető el.
4. A hello **előfizetés** mezőben válassza ki az Azure-előfizetés mely toocreate hello névtérben.
5. A hello  **[erőforráscsoport](../articles/azure-resource-manager/resource-group-portal.md)**  mezőben válasszon egy meglévő erőforráscsoportot, mely hello névtér lesz élő, vagy hozzon létre egy újat.      
6. A **hely**, válassza ki a hello országban vagy régióban, amelyben a névtér üzemeltetve lesz.
   
    ![Névtér létrehozása][create-namespace]
7. Kattintson a **Create** (Létrehozás) gombra. hello rendszer most hoz létre a névteret, majd engedélyezi. Néhány perc elteltével hello rendszer kiosztja az erőforrásokat a fiókjához.

### <a name="obtain-hello-management-credentials"></a>Hello felügyeleti hitelesítő adatok beszerzése
1. Hello névterek, kattintson az újonnan létrehozott névtér neve hello.
2. Hello névtér paneljén kattintson **megosztott elérési házirendek**.
3. A hello **megosztott elérési házirendek** panelen kattintson a **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. A hello **házirend: RootManageSharedAccessKey** panelen kattintson a Másolás gombra hello tovább túl**kapcsolati karakterlánc – elsődleges kulcs**, toocopy hello kapcsolati karakterlánc tooyour vágólapra későbbi használatra. Illessze be ezt az értéket a Jegyzettömbbe vagy egy másik ideiglenes helyre.
   
    ![connection-string][connection-string]

5. Ismétlődő hello előző lépést, másolás és beillesztés hello értékének **elsődleges kulcs** tooa ideiglenes helyre későbbi használatra.  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
