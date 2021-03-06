---
ms.date: 2017-06-12
ms.topic: conceptual
keywords: dsc,powershell,configuration,setup
title: "Vue d’ensemble de la fonctionnalité Desired State Configuration de Windows PowerShell"
ms.openlocfilehash: 1d6ba0b2fdb514e703ddad11adf4cdace5c001a9
ms.sourcegitcommit: a444406120e5af4e746cbbc0558fe89a7e78aef6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2018
---
# <a name="windows-powershell-desired-state-configuration-overview"></a>Vue d’ensemble de la fonctionnalité Desired State Configuration de Windows PowerShell 

> S’applique à : Windows PowerShell 4.0, Windows PowerShell 5.0

DSC est une plateforme de gestion dans PowerShell qui vous permet de gérer votre infrastructure informatique et de développement avec la configuration en tant que code.

- Pour une vue d’ensemble des avantages métier de DSC, consultez [Présentation de la configuration de l’état souhaité pour les décideurs](decisionMaker.md).
- Pour une vue d’ensemble des avantages de l’utilisation de DSC en termes d’ingénierie, consultez [Présentation de la configuration de l’état souhaité pour les ingénieurs](DscForEngineers.md).
- Pour commencer à utiliser rapidement DSC, consultez [Démarrage rapide avec DSC](quickStart.md).

## <a name="key-concepts"></a>Concepts clés

DSC est une plateforme déclarative employée pour la configuration, le déploiement et la gestion des systèmes. Elle réunit trois composants principaux :

- Les [configurations](configurations.md) sont des scripts PowerShell déclaratifs qui définissent et configurent des instances de ressources.
    Quand une configuration est exécutée, DSC (et toutes les ressources appelées par cette configuration) « fait simplement en sorte » que le système se trouve dans l’état souhaité défini par la configuration. 
    Les configurations DSC sont également idempotent, c’est-à-dire que le Gestionnaire de configuration local (ou « LCM ») s’assure en permanence que les machines restent configurées dans l’état déclaré dans la configuration.
- Les [ressources](resources.md) constituent la base de DSC. Elles contiennent le code qui place et conserve la cible d’une configuration dans l’état spécifié. 
    Les ressources sont stockées dans les modules PowerShell et peuvent être créées pour modéliser des éléments généraux, comme un fichier ou un processus Windows, ou des éléments plus spécifiques, tels qu’un serveur IIS ou une machine virtuelle Azure.
- Le [LCM](metaConfig.md) est le moteur utilisé par DSC pour faciliter les interactions entre les ressources et les configurations. 
    Le LCM interroge régulièrement le système, via le flux de contrôle implémenté par les ressources, pour s’assurer que le système est dans l’état déclaré dans une configuration. 
    Si le système n’est pas dans l’état souhaité, le LCM effectue des appels aux code dans les ressources pour « faire en sorte » qu’il soit conforme à l’état déclaré dans la configuration. 

## <a name="see-also"></a>Voir aussi

- [Configurations DSC](configurations.md)
- [Ressources DSC](resources.md)
- [Configuration du Gestionnaire de configuration local](metaConfig.md)

