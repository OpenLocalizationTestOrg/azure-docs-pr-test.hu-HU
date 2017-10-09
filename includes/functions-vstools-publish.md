1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **közzététel**. Válassza a **Create New** (Új létrehozása) elemet, majd kattintson a **Publish** (Közzététel) lehetőségre. 

    ![Új függvényalkalmazás közzététele és létrehozása](./media/functions-vstools-publish/functions-vstools-publish-new-function-app.png)

2. Ha már nem csatlakozott a Visual Studio tooyour Azure-fiókot, kattintson a **fiók hozzáadása...** .  

3. A hello **létrehozása az App Service** párbeszédablak, használjon hello **üzemeltetési** a következő táblázatban szereplő hello beállításokat: 

    ![Az Azure helyi futtatókörnyezete](./media/functions-vstools-publish/functions-vstools-publish.png)

    | Beállítás      | Ajánlott érték  | Leírás                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Alkalmazás neve** | Globálisan egyedi név | Az új függvényalkalmazást azonosító egyedi név. |
    | **Előfizetés** | Válassza ki az előfizetését | hello Azure-előfizetés toouse. |
    | **[Erőforráscsoport](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Mely toocreate a függvény app csoportosítási hello erőforrás nevét. |
    | **[App Service-csomag](../articles/azure-functions/functions-scale.md)** | Használatalapú csomag | Győződjön meg arról, hogy toochoose hello **fogyasztás** alatt **mérete** amikor létrehoz egy új tervet.  |
    | **[Tárfiók](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** | Globálisan egyedi név | Használjon egy már létező tárfiókot, vagy hozzon létre egy újat.   |

4. Kattintson a **létrehozása** toocreate egy függvény alkalmazást az Azure-ban ezekkel a beállításokkal. Hello kiépítés befejezése után jegyezze fel a hello **webhely URL-címe** értéket, amelyet a függvény alkalmazást az Azure-ban hello címe. 

    ![Az Azure helyi futtatókörnyezete](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
