## <a name="push-tooazure-from-git"></a><span data-ttu-id="73b5b-101">A Git Push tooAzure</span><span class="sxs-lookup"><span data-stu-id="73b5b-101">Push tooAzure from Git</span></span>

<span data-ttu-id="73b5b-102">Adjon hozzá egy helyi Git-tárház Azure távoli tooyour.</span><span class="sxs-lookup"><span data-stu-id="73b5b-102">Add an Azure remote tooyour local Git repository.</span></span>

```bash
git remote add azure <URI from previous step>
```

<span data-ttu-id="73b5b-103">Leküldéses toohello Azure távoli toodeploy az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="73b5b-103">Push toohello Azure remote toodeploy your app.</span></span> <span data-ttu-id="73b5b-104">A korábban létrehozott hello központi felhasználói létrehozásakor hello jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="73b5b-104">You are prompted for hello password you created earlier when you created hello deployment user.</span></span> <span data-ttu-id="73b5b-105">Győződjön meg arról, hogy a létrehozott hello jelszó megadása [konfigurálása a központi felhasználói](#configure-a-deployment-user), nem hello jelszó toolog toohello Azure-portált használja.</span><span class="sxs-lookup"><span data-stu-id="73b5b-105">Make sure that you enter hello password you created in [Configure a deployment user](#configure-a-deployment-user), not hello password you use toolog in toohello Azure portal.</span></span>

```bash
git push azure master
```

<span data-ttu-id="73b5b-106">hello előző parancs megjeleníti a toohello hasonló információkat a következő példa:</span><span class="sxs-lookup"><span data-stu-id="73b5b-106">hello preceding command displays information similar toohello following example:</span></span>
