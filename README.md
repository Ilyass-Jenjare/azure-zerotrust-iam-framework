#  Azure Zero Trust IAM Framework

![CI Status](https://github.com/ILyass-Jenjare/azure-zerotrust-iam-framework/actions/workflows/ci.yml/badge.svg)
![Terraform](https://img.shields.io/badge/Terraform-1.5+-623CE4?logo=terraform)
![PowerShell](https://img.shields.io/badge/PowerShell-7.0+-5391FE?logo=powershell)
![Azure](https://img.shields.io/badge/Azure-Cloud_Native-0078D4?logo=microsoft-azure)

> **Blueprint d'architecture et d'automatisation pour la gouvernance des identités (IAM) sur Microsoft Azure, basé sur les principes Zero Trust.**

##  Contexte d'Affaires & Cas d'Usage
Ce projet simule un mandat de modernisation d'infrastructure pour une organisation. L'objectif est double : 
1. **Migration Cloud** : Migrer une charge de travail *on-premises* existante vers Azure.
2. **Gouvernance des Accès** : Remplacer des accès locaux obsolètes (comptes partagés, clés SSH dormantes) par un modèle d'accès cloud centralisé, auditable et respectant les principes **Zero Trust**.

L'ensemble de l'infrastructure est codé avec Terraform et la gestion du cycle de vie des accès (octroi, durée limitée, révocation) est entièrement automatisée via un module PowerShell dédié.

##  Principes Zero Trust Implémentés
* **Vérification Explicite :** Exigence MFA, centralisation des identités sur Entra ID.
* **Moindre Privilège :** Application stricte du RBAC (Role-Based Access Control) sur des groupes (jamais d'utilisateurs directs), rôles personnalisés, et élévation de privilèges justifiée (JIT/Entra ID PIM).
* **Supposer la Compromission :** Stockage des secrets dans Azure Key Vault (sans mot de passe direct via Managed Identities), journalisation centralisée et alertes KQL (Log Analytics).

---

##  Architecture du Projet

*(Ajouter ici le schéma de l'architecture cible et la matrice RBAC)*

##  Stack Technique & Outils

| Catégorie | Technologies Utilisées |
| :--- | :--- |
| **Identité & Sécurité** | Microsoft Entra ID (Azure AD), Entra ID PIM, Azure RBAC, Key Vault, Azure Policy |
| **Infrastructure as Code** | Terraform (`azurerm`, `azuread`) |
| **Automatisation IAM** | PowerShell (Module `AzAccessGov`), Microsoft Graph API, Pester (Tests unitaires) |
| **Supervision & Audit** | Azure Log Analytics, requêtes KQL, Azure Activity Logs |
| **DevSecOps (CI/CD)** | GitHub Actions, Checkov (Analyse IaC), PSScriptAnalyzer |

---

##  Structure du Référentiel

```text
.
├── .github/workflows/       # Pipelines CI/CD (Validation TF, Scan Checkov, Linting PS)
├── docs/                    # Documentation fonctionnelle et technique
│   ├── architecture-as-is.md       # État initial on-premises
│   ├── matrice-rbac-cible.md       # Conception des rôles et groupes
│   └── scenarios-incidents.md      # Playbooks de réponse aux anomalies d'accès
├── src/
│   ├── powershell/          # Module d'automatisation des accès (AzAccessGov)
│   │   ├── AzAccessGov.psm1        # Fonctions principales (New-AccessGrant, etc.)
│   │   └── tests/                  # Tests unitaires Pester
│   └── terraform/           # Code IaC de la zone d'atterrissage (Landing Zone)
│       ├── main.tf                 # Déploiement RG, VNet, NSG, VM, Key Vault
│       └── rbac.tf                 # Groupes Entra ID et attributions de rôles
└── README.md
