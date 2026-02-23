# Skill de développement L4G de Sage X3

Skill complet pour le développement en L4G de l'ERP Sage X3.

## Contenu

- **Syntaxe du langage** - Types de données, variables, structures de contrôle
- **Opérations sur tables** - File, Read, Write, Rewrite, Delete, Link, SQL
- **Sous-programmes et Fonctions** - Subprog, Funprog, Gosub
- **Gestion des masques** - Affzo, Effzo, Grizo, listes gauches
- **Actions OBJet** - Gestionnaires d'événements standard (création, modification, suppression)
- **Actions de champs** - Événements de saisie (AVANT_ZONE, CONTROLE, APRES_ZONE, etc.)
- **Variables système** - Application, tables, saisie, debug
- **Fonctions intégrées** - Numériques, chaînes, dates, tables, masques
- **Gestion des erreurs** - ONERRGO, codes d'erreur, FSTAT
- **Bonnes pratiques** - Conventions de nommage, performances, patterns

## Installation

### OpenCode - Méthode Manuelle

1. Copier le fichier `SKILL.md` dans le répertoire des skills de OpenCode
   ```bash
   cp SKILL.md ~/.opencode/skills/SAGE_X3_L4G/SKILL.md
   ```
2. Redémarrer OpenCode si nécessaire

### OpenCode - Avec npx skills

```bash
npx skills add opennext-dz/l4g-agent-skill
```


### Google Antigravity

Copier le fichier `SKILL.md` dans le répertoire des skills de Antigravity ou bien sur le répertoire du projet
   ```bash   
   cp SKILL.md ~/.antigravity/skills/SAGE_X3_L4G/SKILL.md
   
   # Windows
   copy SKILL.md %PROJECTPATH%\.agent\skills\SAGE_X3_L4G\SKILL.md
   ```


---
