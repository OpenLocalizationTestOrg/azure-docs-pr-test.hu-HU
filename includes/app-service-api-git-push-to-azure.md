<span data-ttu-id="eaef0-101">Az API-alkalmazás hello Azure CLI tooget hello távoli telepítés URL-CÍMÉT használja.</span><span class="sxs-lookup"><span data-stu-id="eaef0-101">Use hello Azure CLI tooget hello remote deployment URL for your API App.</span></span> <span data-ttu-id="eaef0-102">Hello a következő parancsban cserélje le  *\<alkalmazás_neve >* a webalkalmazás-névvel.</span><span class="sxs-lookup"><span data-stu-id="eaef0-102">In hello following command, replace *\<app_name>* with your web app's name.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

<span data-ttu-id="eaef0-103">A helyi Git telepítési toobe képes toopush toohello távoli konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="eaef0-103">Configure your local Git deployment toobe able toopush toohello remote.</span></span>

```bash
git remote add azure <URI from previous step>
```

<span data-ttu-id="eaef0-104">Leküldéses toohello Azure távoli toodeploy az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eaef0-104">Push toohello Azure remote toodeploy your app.</span></span> <span data-ttu-id="eaef0-105">A korábban létrehozott hello központi felhasználói létrehozásakor hello jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="eaef0-105">You are prompted for hello password you created earlier when you created hello deployment user.</span></span> <span data-ttu-id="eaef0-106">Győződjön meg arról, hogy hello jelszó megadását létrehozott hello gyors üzembe helyezés során korábban küldje el, és nem hello jelszó toolog toohello Azure-portált használja.</span><span class="sxs-lookup"><span data-stu-id="eaef0-106">Make sure that you enter hello password you created in earlier in hello quickstart, and not hello password you use toolog in toohello Azure portal.</span></span>

```bash
git push azure master
```
