---
ms.date: 2017-06-12
author: JKeithB
ms.topic: reference
keywords: wmf,powershell,configuration
ms.openlocfilehash: 555e01e88647b40717417360fb74bb6554a9c122
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2017
---
# <a name="authoring-improvements-using-powershell-ise"></a>Améliorations liées à la création à l’aide de PowerShell ISE

Créer des configurations DSC dans Windows PowerShell ISE est beaucoup plus facile grâce aux améliorations suivantes :

- Énumération de toutes les ressources DSC dans un bloc **configuration** ou **node** en entrant **Ctrl+Espace** sur une ligne vide dans le bloc.
- Saisie semi-automatique sur les propriétés de ressources de type **énumération**.
- Saisie semi-automatique sur la propriété **DependsOn** des ressources DSC, basée sur d’autres instances de ressources dans la configuration.
- Meilleure saisie semi-automatique via la touche Tab pour les valeurs de propriétés de ressources.

**Remarque :** Vous devez avoir une chaîne vide pour les valeurs de propriétés de ressources pour pouvoir utiliser Ctrl+Espace pour répertorier les options. Une pression sur la touche **Tab** permet de parcourir les options.

