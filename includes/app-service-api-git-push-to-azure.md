<span data-ttu-id="4d412-101">Az Azure CLI használatával megszerezheti az API-alkalmazás távoli üzembe helyezési URL-címét.</span><span class="sxs-lookup"><span data-stu-id="4d412-101">Use the Azure CLI to get the remote deployment URL for your API App.</span></span> <span data-ttu-id="4d412-102">A következő parancsban az *\<app_name>* helyett használja a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="4d412-102">In the following command, replace *\<app_name>* with your web app's name.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

<span data-ttu-id="4d412-103">A Git helyi üzemelő példányának konfigurálásával végezhet leküldést a távoli mappába.</span><span class="sxs-lookup"><span data-stu-id="4d412-103">Configure your local Git deployment to be able to push to the remote.</span></span>

```bash
git remote add azure <URI from previous step>
```

<span data-ttu-id="4d412-104">A távoli Azure-mappához történő küldéssel helyezze üzembe az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4d412-104">Push to the Azure remote to deploy your app.</span></span> <span data-ttu-id="4d412-105">A rendszer rákérdez az előzőleg, az üzembe helyező felhasználó létrehozásakor létrehozott jelszóra.</span><span class="sxs-lookup"><span data-stu-id="4d412-105">You are prompted for the password you created earlier when you created the deployment user.</span></span> <span data-ttu-id="4d412-106">Ügyeljen arra, hogy a gyors útmutató korábbi szakaszában létrehozott jelszót adja meg, ne azt, amelyet az Azure Portalra való bejelentkezéshez használ.</span><span class="sxs-lookup"><span data-stu-id="4d412-106">Make sure that you enter the password you created in earlier in the quickstart, and not the password you use to log in to the Azure portal.</span></span>

```bash
git push azure master
```
