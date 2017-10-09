1. Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.

1. Kattintson a **Számítás** > **Függvényalkalmazás** elemre, és válassza az **Előfizetés** elemet. Ezt követően beállításokkal hello függvény app hello táblázatban megadottak szerint.

    ![Függvény-alkalmazás létrehozása az Azure-portálon hello](./media/functions-create-function-app-portal/function-app-create-flow.png)

    | Beállítás      | Ajánlott érték  | Leírás                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Alkalmazás neve** | Globálisan egyedi név | Az új függvényalkalmazást azonosító név. | 
    | **[Erőforráscsoport](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | A függvény app hello mely toocreate az új erőforráscsoporthoz tartozó nevet. | 
    | **[Szolgáltatási csomag](../articles/azure-functions/functions-scale.md)** |   Használatalapú csomag | Üzemeltetési terv, amely meghatározza a-erőforrások elosztását vezérli tooyour függvény alkalmazást. Hello alapértelmezett **fogyasztás megtervezése**, erőforrások hozzáadása a funkciók által megkövetelt dinamikusan. Csak kell fizetnie hello futtatásakor a függvényeket.   |
    | **Hely** | Nyugat-Európa | Válasszon egy helyet a közelben vagy a függvények által elérendő más szolgáltatások közelében. |
    | **[Tárfiók](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Globálisan egyedi név |  Hello új tárfiók a függvény alkalmazás által használt nevét. A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat. Meglévő fiókot is használhat. |

1. Kattintson a **létrehozása** tooprovision és hello új függvény alkalmazás telepítése.
