<span data-ttu-id="920e2-101">Üzembe helyezési hitelesítő adatok létrehozása a hello [beállítva az az webalkalmazás üzembe helyező felhasználó](/cli/azure/webapp/deployment/user#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="920e2-101">Create deployment credentials with hello [az webapp deployment user set](/cli/azure/webapp/deployment/user#set) command.</span></span>

<span data-ttu-id="920e2-102">A központi telepítés felhasználóra szükség az FTP és a helyi Git telepítési tooa webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="920e2-102">A deployment user is required for FTP and local Git deployment tooa web app.</span></span> <span data-ttu-id="920e2-103">hello felhasználónév és jelszó fiókszinten.</span><span class="sxs-lookup"><span data-stu-id="920e2-103">hello user name and password are account level.</span></span> <span data-ttu-id="920e2-104">_Ezek nem azonosak az Azure-előfizetés hitelesítő adataival._</span><span class="sxs-lookup"><span data-stu-id="920e2-104">_They are different from your Azure subscription credentials._</span></span>

<span data-ttu-id="920e2-105">Hello a következő parancsban cserélje le  *\<felhasználónév->* és  *\<jelszó >* új felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="920e2-105">In hello following command, replace *\<user-name>* and *\<password>* with a new user name and password.</span></span> <span data-ttu-id="920e2-106">hello felhasználó nevének egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="920e2-106">hello user name must be unique.</span></span> <span data-ttu-id="920e2-107">hello jelszó legalább 8 karakter hosszúságúnak kell lennie, és két a következő három elemek hello: betűket, számokat, szimbólumokat.</span><span class="sxs-lookup"><span data-stu-id="920e2-107">hello password must be at least eight characters long, with two of hello following three elements: letters, numbers, symbols.</span></span> 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

<span data-ttu-id="920e2-108">Ha egy ` 'Conflict'. Details: 409` hiba, a módosítás hello felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="920e2-108">If you get a ` 'Conflict'. Details: 409` error, change hello username.</span></span> <span data-ttu-id="920e2-109">` 'Bad Request'. Details: 400` hibaüzenet esetén használjon erősebb jelszót.</span><span class="sxs-lookup"><span data-stu-id="920e2-109">If you get a ` 'Bad Request'. Details: 400` error, use a stronger password.</span></span>

<span data-ttu-id="920e2-110">Ezt az üzembe helyező felhasználót csak egyszer kell létrehoznia, és minden Azure-környezetben használhatja.</span><span class="sxs-lookup"><span data-stu-id="920e2-110">You create this deployment user only once; you can use it for all your Azure deployments.</span></span>

> [!NOTE]
> <span data-ttu-id="920e2-111">Rekord hello felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="920e2-111">Record hello user name and password.</span></span> <span data-ttu-id="920e2-112">Őket toodeploy hello webalkalmazás később szüksége.</span><span class="sxs-lookup"><span data-stu-id="920e2-112">You need them toodeploy hello web app later.</span></span>
>
>