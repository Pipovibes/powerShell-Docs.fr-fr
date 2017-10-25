---
ms.date: 2017-06-09
schema: 2.0.0
keywords: powershell
title: RequireLicenseAcceptanceScript
ms.openlocfilehash: 7092fb2e63b9e2b1eca59cd418317631bff8b172
ms.sourcegitcommit: cd66d4f49ea762a31887af2c72d087b219ddbe10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2017
---
# <a name="requiring-license-acceptance-for-scripts"></a><span data-ttu-id="92c7b-103">Exiger l’acceptation de la licence pour les scripts</span><span class="sxs-lookup"><span data-stu-id="92c7b-103">Requiring License Acceptance for Scripts</span></span>

<span data-ttu-id="92c7b-104">L’acceptation de licence n’est pas prise en charge pour les scripts.</span><span class="sxs-lookup"><span data-stu-id="92c7b-104">License Acceptance is not supported for scripts.</span></span> <span data-ttu-id="92c7b-105">Cependant, le scénario où un script dépend d’un module qui exige l’acceptation de la licence est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="92c7b-105">However, the scenario where a script depends on a module that requires license acceptance is supported.</span></span>

<span data-ttu-id="92c7b-106">Les commandes de scripts (Install-Script/Save-Script/Update-Script) prennent en charge un nouveau paramètre -AcceptLicense qui se comporte comme si l’utilisateur avait vu la licence.</span><span class="sxs-lookup"><span data-stu-id="92c7b-106">Script commands(Install-Script/Save-Script/Update-Script) support a new parameter -AcceptLicense that behaves as though user saw the license.</span></span> <span data-ttu-id="92c7b-107">Si AcceptLicense n’est pas spécifié, l’utilisateur verra license.txt pour le module dépendant et sera invité à accepter la licence.</span><span class="sxs-lookup"><span data-stu-id="92c7b-107">If -AcceptLicense is not specified; the user will be shown license.txt for dependent module and prompted to accept the license.</span></span>

## <a name="examples"></a><span data-ttu-id="92c7b-108">EXEMPLES</span><span class="sxs-lookup"><span data-stu-id="92c7b-108">EXAMPLES</span></span>

### <a name="example-1-install-script-with-dependencies-requiring-license-acceptance"></a><span data-ttu-id="92c7b-109">Exemple 1 : installation d’un script avec des dépendances exigeant une acceptation de licence</span><span class="sxs-lookup"><span data-stu-id="92c7b-109">Example 1: Install Script with dependencies requiring license acceptance</span></span>
<span data-ttu-id="92c7b-110">Le script 'ScriptRequireLicenseAcceptance' dépend du module 'ModuleRequireLicenseAcceptance'.</span><span class="sxs-lookup"><span data-stu-id="92c7b-110">Script 'ScriptRequireLicenseAcceptance' depends on module 'ModuleRequireLicenseAcceptance'.</span></span> <span data-ttu-id="92c7b-111">L’utilisateur est invité à accepter la licence.</span><span class="sxs-lookup"><span data-stu-id="92c7b-111">User is prompted to Accept License.</span></span>
```PowerShell
PS C:\> Install-Script -Name ScriptRequireLicenseAcceptance

License Acceptance
MIT License 2.0
Copyright (c) 2016 PowerShell Team
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software.

Do you accept the license terms for module 'ModuleRequireLicenseAcceptance'.
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): 
```

### <a name="example-2-install-script-with-dependencies-requiring-license-acceptance-and--acceptlicense"></a><span data-ttu-id="92c7b-112">Exemple 2 : installation d’un script avec des dépendances exigeant une acceptation de licence et AcceptLicense</span><span class="sxs-lookup"><span data-stu-id="92c7b-112">Example 2: Install Script with dependencies requiring license acceptance and -AcceptLicense</span></span>
<span data-ttu-id="92c7b-113">Le script 'ScriptRequireLicenseAcceptance' dépend du module 'ModuleRequireLicenseAcceptance'.</span><span class="sxs-lookup"><span data-stu-id="92c7b-113">Script 'ScriptRequireLicenseAcceptance' depends on module 'ModuleRequireLicenseAcceptance'.</span></span> <span data-ttu-id="92c7b-114">L’utilisateur n’est pas invité à accepter une licence, puisqu’AcceptLicense est spécifié.</span><span class="sxs-lookup"><span data-stu-id="92c7b-114">User is not prompted to accept license as -AcceptLicense is specified.</span></span>
```PowerShell
PS C:\> Install-Script -Name ScriptRequireLicenseAcceptance -AcceptLicense
```

## <a name="more-details"></a><span data-ttu-id="92c7b-115">Plus d’informations</span><span class="sxs-lookup"><span data-stu-id="92c7b-115">More details</span></span>
### <a name="require-license-acceptance-support-for-modulesmodulerequirelicenseacceptancemd"></a>[<span data-ttu-id="92c7b-116">Nécessitent la prise en charge de l’acceptation de licence pour les modules</span><span class="sxs-lookup"><span data-stu-id="92c7b-116">Require License Acceptance support for Modules</span></span>](../module/RequireLicenseAcceptance.md)

### <a name="require-license-acceptance-support-on-powershellgallerypsgallerypsgalleryrequireslicenseacceptancemd"></a>[<span data-ttu-id="92c7b-117">Nécessitent la prise en charge de l’acceptation de licence sur PowerShellGallery</span><span class="sxs-lookup"><span data-stu-id="92c7b-117">Require License Acceptance support on PowerShellGallery</span></span>](../../psgallery/psgallery_requires_license_acceptance.md)

### <a name="require-license-acceptance-on-deploy-to-azure-automationpsgallerypsgallerydeploytoazureautomationrequirelicenseacceptancemd"></a>[<span data-ttu-id="92c7b-118">Exigent l’acceptation de la licence lors du déploiement sur Azure Automation</span><span class="sxs-lookup"><span data-stu-id="92c7b-118">Require License Acceptance on Deploy to Azure Automation</span></span>](../../psgallery/psgallery_deploy_to_azure_automation_requireLicenseAcceptance.md)