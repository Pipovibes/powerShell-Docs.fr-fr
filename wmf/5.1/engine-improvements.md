---
ms.date: 2017-06-12
author: JKeithB
ms.topic: reference
keywords: wmf,powershell,configuration
title: "Améliorations du moteur PowerShell dans WMF 5.1"
ms.openlocfilehash: 6c8000ccfc59ab46de95dc4f67161e12a5a41199
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2017
---
#<a name="powershell-engine-improvements"></a>Améliorations du moteur PowerShell

Les améliorations suivantes du moteur principal PowerShell ont été implémentées dans WMF 5.1 :


## <a name="performance"></a>Performances ##

Les performances ont été améliorées dans certains domaines importants :

- Démarrage
- Le traitement « pipeline » vers les applets de commande ForEach-Object et Where-Object est environ 50 % plus rapide. 

Voici quelques exemples d’améliorations (les résultats peuvent varier en fonction de votre matériel) : 

| Scénario | Durée 5.0 (ms) | Durée 5.1 (ms) |
| -------- | :---------------: | :---------------: |
| `powershell -command "echo 1"` | 900 | 250 |
| Première exécution PowerShell : `powershell -command "Unknown-Command"` | 30 000 | 13 000 |
| Création du cache d’analyse de commande : `powershell -command "Unknown-Command"` | 7000 | 520 |
| <code>1..1000000 &#124; % { }</code> | 1400 | 750 |
  
> Notez qu’une modification liée au démarrage peut affecter certains scénarios non pris en charge. 
> PowerShell ne lit plus les fichiers `$pshome\*.ps1xml`. Ces fichiers ont été convertis en C# pour éviter une surcharge du processeur et des fichiers lors du traitement des fichiers XML. 
Les fichiers existent toujours pour prendre en charge V2 côte à côte. Ainsi, si vous modifiez le contenu des fichiers, cela n’affecte pas V5 mais uniquement V2. 
Notez que la modification du contenu de ces fichiers n’a jamais été un scénario pris en charge.

Une autre modification visible est la façon dont PowerShell met en cache les commandes exportées et d’autres informations pour les modules installés sur un système. Auparavant, ce cache était stocké dans le répertoire `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\CommandAnalysis`. Dans WMF 5.1, le cache est un fichier unique `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\ModuleAnalysisCache`.
Pour plus de détails, voir la page [Cache d’analyse de module](scenarios-features.md#module-analysis-cache).

