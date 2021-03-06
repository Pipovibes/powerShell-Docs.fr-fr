---
ms.date: 2017-06-12
contributor: manikb
ms.topic: reference
keywords: gallery,powershell,cmdlet,psget
title: "Démarrage du fournisseur NuGet et de l’EXE"
ms.openlocfilehash: 0036972eb9a0c20469da1aadafe223e6ec80f16a
ms.sourcegitcommit: a5c0795ca6ec9332967bff9c151a8572feb1a53a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/27/2017
---
# <a name="bootstrap-both-nuget-provider-and-nugetexe-or-bootstrap-only-nuget-provider"></a>Démarrer le fournisseur NuGet et NuGet.exe ou démarrer uniquement le fournisseur NuGet

NuGet.exe n’est pas inclus dans le dernier fournisseur NuGet.
Pour les opérations de publication d’un module ou d’un script, PowerShellGet nécessite la version binaire exécutable, NuGet.exe.
Le fournisseur NuGet est requis pour toutes les autres opérations, y compris *trouver*, *installer*, *enregistrer* et *désinstaller*.
PowerShellGet inclut une logique pour gérer soit un démarrage combiné du fournisseur NuGet et de NuGet.exe soit un démarrage du fournisseur NuGet uniquement.
Dans les deux cas, une seule invite doit s’afficher.
Si l’ordinateur n’est pas connecté à Internet, l’utilisateur ou administrateur doit copier une instance approuvée du fournisseur NuGet et/ou du fichier NuGet.exe sur l’ordinateur déconnecté.

>**Remarque** : à compter de la version 6, le fournisseur NuGet est inclus dans l’installation de PowerShell. [http://github.com/powershell/powershell](http://github.com/powershell/powershell)

## <a name="resolving-error-when-the-nuget-provider-has-not-been-installed-on-a-machine-that-is-internet-connected"></a>Résolution des erreurs lorsque le fournisseur NuGet n’a pas été installé sur un ordinateur qui est connecté à Internet

```powershell
PS C:\> Find-Module -Repository PSGallery -Verbose -Name Contoso

NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
'C:\Users\manikb\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import the NuGet provider
now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): n
Find-Module : NuGet provider is required to interact with NuGet-based repositories. Please ensure that '2.8.5.201' or newer version of NuGet provider is installed.
At line:1 char:1
+ Find-Module -Repository PSGallery -Verbose -Name Contoso
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Find-Module], InvalidOperationException
   + FullyQualifiedErrorId : CouldNotInstallNuGetProvider,Find-Module

PS C:\> Find-Module -Repository PSGallery -Verbose -Name Contoso

NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
'C:\Users\manikb\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import the NuGet provider
now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet provider.

Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
2.5        Contoso                             Module     PSGallery        Contoso module
```
## <a name="resolving-error-when-the-nuget-provider-is-available-and-nugetexe-is-not-available-during-the-publish-operation-on-a-machine-that-is-internet-connected"></a>Résolution des erreurs lorsque le fournisseur NuGet est disponible et que NuGet.exe n’est pas disponible lors de l’opération de publication sur une machine connectée à Internet

```powershell
PS C:\> Publish-Module -Name Contoso -Repository PSGallery -Verbose

NuGet.exe is required to continue
PowerShellGet requires NuGet.exe to publish an item to the NuGet-based repositories. NuGet.exe must be available under one of the paths specified in PATH environment variable value. Do you want PowerShellGet to install NuGet.exe now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): N
Publish-Module : NuGet.exe is required to interact with NuGet-based repositories. Please ensure that NuGet.exe is available under one of the paths specified in PATH environment variable value.
At line:1 char:1
+ Publish-Module -Name Contoso -Repository PSGallery -Verbose
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Publish-Module], InvalidOperationException
    + FullyQualifiedErrorId : CouldNotInstallNuGetExe,Publish-Module

PS C:\> Publish-Module -Name Contoso -Repository PSGallery -Verbose

NuGet.exe is required to continue
PowerShellGet requires NuGet.exe to publish an item to the NuGet-based repositories. NuGet.exe must be available under one of the paths specified in PATH environment variable value. Do you want PowerShellGet to install NuGet.exe now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet.exe.
VERBOSE: Successfully published module 'Contoso' to the module publish location 'https://www.powershellgallery.com/api/v2/'. Please allow few minutes for 'Contoso' to show up in the search results.
```

## <a name="resolving-error-when-both-nuget-provider-and-nugetexe-are-not-available-during-the-publish-operation-on-a-machine-that-is-internet-connected"></a>Résolution des erreurs lorsque ni le fournisseur NuGet ni NuGet.exe ne sont disponibles lors de l’opération de publication sur une machine connectée à Internet

```powershell
PS C:\> Publish-Module -Name Contoso -Repository PSGallery -Verbose

NuGet.exe and NuGet provider are required to continue
PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer to interact with the NuGet-based repositories. Do you want PowerShellGet to install both NuGet.exe and NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): N
Publish-Module : PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer to interact with the NuGet-based repositories. Please ensure that '2.8.5.201' or newer version of NuGet provider is installed and NuGet.exe is available under 
one of the paths specified in PATH environment variable value.
At line:1 char:1
+ Publish-Module -Name Contoso -Repository PSGallery -Verbose
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Publish-Module], InvalidOperationException
    + FullyQualifiedErrorId : CouldNotInstallNuGetBinaries,Publish-Module

PS C:\> Publish-Module -Name Contoso -Repository PSGallery -Verbose

NuGet.exe and NuGet provider are required to continue
PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer to interact with the NuGet-based repositories. Do you want PowerShellGet to install both NuGet.exe and NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet provider.
VERBOSE: Installing NuGet.exe.
VERBOSE: Successfully published module 'Contoso' to the module publish location 'https://www.powershellgallery.com/api/v2/'. Please allow few minutes for 'Contoso' to show up in the search results.
```

## <a name="manually-bootstrapping-the-nuget-provider-on-a-machine-that-is-not-connected-to-the-internet"></a>Démarrage manuel du fournisseur NuGet sur un ordinateur qui n’est pas connecté à Internet

Les processus détaillés ci-dessus supposent que l’ordinateur est connecté à Internet et peut télécharger des fichiers à partir d’un emplacement public.
Si cela n’est pas possible, la seule option est de démarrer une machine en utilisant les processus ci-dessus et de copier manuellement le fournisseur sur le nœud isolé via un processus approuvé en mode hors connexion.
Le cas d’utilisation le plus courant pour ce scénario est lorsqu’une galerie privée est disponible pour prendre en charge un environnement isolé.

Après avoir suivi le processus ci-dessus pour démarrer une machine connectée à Internet, vous trouverez des fichiers du fournisseur à l’emplacement :
```
C:\Program Files\PackageManagement\ProviderAssemblies\
```

La structure de dossiers et fichiers du fournisseur NuGet sera (éventuellement avec un autre numéro de version) :

NuGet<br>
--2.8.5.208<br>
----Microsoft.PackageManagement.NuGetProvider.dll

Copiez ces dossiers et fichiers avec un processus approuvé sur les machines en mode hors connexion.

## <a name="manually-bootstrapping-nugetexe-to-support-publish-operations-on-a-machine-that-is-not-connected-to-the-internet"></a>Démarrage manuel de NuGet.exe pour prendre en charge les opérations de publication sur un ordinateur qui n’est pas connecté à Internet

Outre le processus de démarrage manuel du fournisseur NuGet, si la machine être utilisée pour publier des modules ou des scripts dans une galerie privée à l’aide des applets de commande *Publish-Module* ou *Publish-Script*, le fichier exécutable binaire NuGet.exe sera nécessaire.
Le cas d’utilisation le plus courant pour ce scénario est lorsqu’une galerie privée est disponible pour prendre en charge un environnement isolé.
Il existe deux options pour obtenir le fichier de NuGet.exe.

Une option consiste à démarrer une machine connectée à Internet et de copier les fichiers sur les machines en mode hors connexion à l’aide d’un processus approuvé.
Après le démarrage de la machine connectée à Internet, le fichier binaire exécutable NuGet.exe se trouve dans un de ces deux dossiers :

Si les applets de commande *Publish-Module* ou *Publish-Script* ont été exécutées avec des autorisations élevées (en tant qu’administrateur) :
```
$env:ProgramData\Microsoft\Windows\PowerShell\PowerShellGet
```

Si les applets de commande ont été exécutées en tant qu’utilisateur sans autorisations élevées :
```
$env:userprofile\AppData\Local\Microsoft\Windows\PowerShell\PowerShellGet\
```

Une seconde option consiste à télécharger NuGet.exe depuis le site web NuGet.Org : [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html)<br>
Lorsque vous sélectionnez une version de NuGet pour les machines de production, assurez-vous qu’elle est ultérieure à 2.8.5.208 et identifiez la version qui a été étiquetée comme étant « recommandée ».
N’oubliez pas de débloquer le fichier s’il a été téléchargé à l’aide d’un navigateur.
Cela peut être effectué à l’aide de l’applet de commande *Unblock-File*.

Dans les deux cas, le fichier NuGet.exe peut être copié vers un emplacement quelconque dans *$env:path*, mais les emplacements standards sont :

Pour rendre l’exécutable disponible afin que tous les utilisateurs puissent utiliser les applets de commande *Publish-Module* et *Publish-Script* :
```
$env:ProgramData\Microsoft\Windows\PowerShell\PowerShellGet
```

Pour rendre l’exécutable disponible pour seulement un utilisateur spécifique, copiez vers l’emplacement dans le profil de cet utilisateur uniquement :
```
$env:userprofile\AppData\Local\Microsoft\Windows\PowerShell\PowerShellGet\
```

