<span data-ttu-id="ed10d-101">Helyi Git telepítési toohello webalkalmazás konfigurálása hello [az webalkalmazás központi telepítési forrás config-helyi-git](/cli/azure/webapp/deployment/source#config-local-git) parancsot.</span><span class="sxs-lookup"><span data-stu-id="ed10d-101">Configure local Git deployment toohello web app with hello [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) command.</span></span>

<span data-ttu-id="ed10d-102">App Service számos módon toodeploy tartalom tooa web app alkalmazásban például FTP, a helyi Git, a GitHub, a Visual Studio Team Services és a Bitbucketből támogatja.</span><span class="sxs-lookup"><span data-stu-id="ed10d-102">App Service supports several ways toodeploy content tooa web app, such as FTP, local Git, GitHub, Visual Studio Team Services, and Bitbucket.</span></span> <span data-ttu-id="ed10d-103">Ebben a gyors útmutatóban a helyi Gitet fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="ed10d-103">For this quickstart, you deploy by using local Git.</span></span> <span data-ttu-id="ed10d-104">Ez azt jelenti, hogy a helyi tárház tooa tárházból az Azure-ban a Git parancs toopush használatával kíván üzembe helyezni.</span><span class="sxs-lookup"><span data-stu-id="ed10d-104">That means you deploy by using a Git command toopush from a local repository tooa repository in Azure.</span></span> 

<span data-ttu-id="ed10d-105">Hello a következő parancsban cserélje le  *\<alkalmazás_neve >* a webalkalmazás-névvel.</span><span class="sxs-lookup"><span data-stu-id="ed10d-105">In hello following command, replace *\<app_name>* with your web app's name.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

<span data-ttu-id="ed10d-106">hello kimeneti formátum a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="ed10d-106">hello output has hello following format:</span></span>

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

<span data-ttu-id="ed10d-107">Hello `<username>` hello van [központi felhasználói](#configure-a-deployment-user) az előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="ed10d-107">hello `<username>` is hello [deployment user](#configure-a-deployment-user) that you created in a previous step.</span></span>

<span data-ttu-id="ed10d-108">Másolás hello URI látható; a következő lépésben hello ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="ed10d-108">Copy hello URI shown; you'll use it in hello next step.</span></span>
