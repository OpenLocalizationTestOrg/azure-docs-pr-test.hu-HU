Helyi Git telepítési toohello webalkalmazás konfigurálása hello [az webalkalmazás központi telepítési forrás config-helyi-git](/cli/azure/webapp/deployment/source#config-local-git) parancsot.

App Service számos módon toodeploy tartalom tooa web app alkalmazásban például FTP, a helyi Git, a GitHub, a Visual Studio Team Services és a Bitbucketből támogatja. Ebben a gyors útmutatóban a helyi Gitet fogjuk használni. Ez azt jelenti, hogy a helyi tárház tooa tárházból az Azure-ban a Git parancs toopush használatával kíván üzembe helyezni. 

Hello a következő parancsban cserélje le  *\<alkalmazás_neve >* a webalkalmazás-névvel.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

hello kimeneti formátum a következő hello rendelkezik:

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

Hello `<username>` hello van [központi felhasználói](#configure-a-deployment-user) az előző lépésben létrehozott.

Másolás hello URI látható; a következő lépésben hello ismertetjük.
