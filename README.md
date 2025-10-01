# Audit_Server_2022

> **Objectif** : évaluer la conformité d’un serveur Windows Server 2022 aux bonnes pratiques de durcissement (CIS, Microsoft Baselines, ANSSI), identifier les écarts et prioriser les remédiations.

---

## 1) Informations générales
- **Client / Environnement** : ...................................................
- **Serveur audité** : Nom / Rôle / Version (Core/GUI) / Build / VM ou Physique
- **Datacenter / Zone réseau** : ...........................................
- **Périmètre** : OS / Rôles spécifiques (IIS, SQL, AD DS, Fichiers, RDS, etc.)
- **Fenêtre d’audit** : Date / Plage horaire
- **Accès fournis** : Compte, Niveau, Bastion/Jump, Outillage autorisé
- **Référentiels** : CIS WS2022 vX, MS Security Baselines (MSCT), ANSSI (Windows), ISO 27001 A.8/A.12/A.14

---

## 2) Résumé exécutif (à compléter après audit)
- **Score de conformité global** : …… / 100
- **Principaux risques** (Top 5) : ……………………………………………………
- **Décisions & quick wins** (≤ 7 jours) : …………………………………………
- **Actions prioritaires P1/P2/P3** : ………………………………………………
- **Prochaines étapes** : re‑test, baselines GPO, suivi SIEM, etc.

---

## 3) Méthodologie
- **Approche** : revue documentaire, entretiens, collecte de preuves, tests non intrusifs, scoring par contrôle.
- **Échelle de sévérité** :
  - **Critique (P1)** : exploitable à distance/sans authentification ; impact majeur (confidentialité, intégrité, disponibilité).
  - **Élevée (P2)** : privilèges élevés requis ou surface limitée ; impact important.
  - **Modérée (P3)** : durcissement recommandé ; impact moyen.
  - **Faible (P4)** : hygiène, cosmétique, documentation.
- **Notation** : (Contrôles conformes ÷ Contrôles applicables) × 100.

---

## 4) Tableau de bord des contrôles (synthèse)
> Cochez **OK / Écart / N/A** et notez la **preuve** (capture, export, référence de log) ; indiquez **P1..P4**.

| Domaine               | Contrôle                                              | Attendu               | OK  | Écart | N/A | Preuve/référence | Priorité |
| --------------------- | ----------------------------------------------------- | --------------------- | --- | ----- | --- | ---------------- | -------- |
| Inventaire & build    | Version WS2022 supportée, patchs ≤30 jours            | Build, KB récents     | ☐   | ☐     | ☐   | …                | P?       |
| Mode d’installation   | Server Core privilégié (si compatible)                | Core ou justification | ☐   | ☐     | ☐   | …                | P?       |
| Démarrage sécurisé    | UEFI + Secure Boot activés                            | BIOS/UEFI & bcdedit   | ☐   | ☐     | ☐   | …                | P?       |
| Matériel de confiance | TPM 2.0 présent/actif                                 | tpm.msc               | ☐   | ☐     | ☐   | …                | P?       |
| VBS/HVCI              | Virtualization‑Based Security + HVCI actifs           | DeviceGuard état      | ☐   | ☐     | ☐   | …                | P?       |
| Credential Guard      | Activé + LSASS en PPL                                 | Registre/DeviceGuard  | ☐   | ☐     | ☐   | …                | P?       |
| Comptes locaux        | Admin builtin renommé/désactivé + LAPS                | Local Users & Groups  | ☐   | ☐     | ☐   | …                | P?       |
| Pol. mots de passe    | Longueur ≥14, complexité ON, âge ≤60j                 | Secpol/GPO            | ☐   | ☐     | ☐   | …                | P?       |
| Groupes d’admin       | Moindre privilège, revues périodiques                 | Membres du groupe     | ☐   | ☐     | ☐   | …                | P?       |
| MFA bastion           | Accès admin via jump/MFA                              | Procédure bastion     | ☐   | ☐     | ☐   | …                | P?       |
| Pare‑feu              | Profils ON, inbound DENY, règles minimales            | NetFirewallProfile    | ☐   | ☐     | ☐   | …                | P?       |
| Journal pare‑feu      | Logging allowed/blocked activé                        | pfirewall.log         | ☐   | ☐     | ☐   | …                | P?       |
| Filtrage sortant      | Egress contrôlé ou justifié                           | Règles outbound       | ☐   | ☐     | ☐   | …                | P?       |
| Services              | Services inutiles désactivés                          | Services.msc          | ☐   | ☐     | ☐   | …                | P?       |
| Protocoles legacy     | SMBv1 OFF ; LM/NTLMv1 refusés                         | Features/Registre     | ☐   | ☐     | ☐   | …                | P?       |
| TLS/Schannel          | TLS 1.2/1.3 ON ; SSLv3/TLS1.0/1.1 OFF                 | Schannel conf         | ☐   | ☐     | ☐   | …                | P?       |
| RDP                   | NLA ON ; accès restreint ; bannière légale            | GPO, config RDP       | ☐   | ☐     | ☐   | …                | P?       |
| WinRM                 | HTTPS‑only, cert valide, listeners restreints         | winrm/config          | ☐   | ☐     | ☐   | …                | P?       |
| BitLocker             | Chiffrement disque OS ; clés stockées                 | manage‑bde            | ☐   | ☐     | ☐   | …                | P?       |
| Antivirus/EDR         | Defender/EDR actif, signatures à jour                 | Get‑Mp* / EDR         | ☐   | ☐     | ☐   | …                | P?       |
| ASR                   | Règles ASR en Audit/Block selon contexte              | MpPreference          | ☐   | ☐     | ☐   | …                | P?       |
| Exploit Guard         | Stratégies applicables (Exploit Protection)           | Policy XML            | ☐   | ☐     | ☐   | …                | P?       |
| Contrôle applis       | AppLocker/WDAC selon rôle                             | Politiques applis     | ☐   | ☐     | ☐   | …                | P?       |
| Logs Windows          | Tailles augmentées, rétention définie                 | wevtutil sl           | ☐   | ☐     | ☐   | …                | P?       |
| Audit avancé          | 4688/4624/4625/4769… activés                          | auditpol              | ☐   | ☐     | ☐   | …                | P?       |
| PowerShell logging    | ScriptBlock + Transcription                           | Registre/Pol          | ☐   | ☐     | ☐   | …                | P?       |
| NTP                   | Temps synchronisé (source autorisée)                  | w32tm                 | ☐   | ☐     | ☐   | …                | P?       |
| Sauvegardes           | Sauvegarde + restauration testées                     | Procédures/report     | ☐   | ☐     | ☐   | …                | P?       |
| SIEM/centralisation   | Redirection des logs (Windows Event Forwarding/agent) | SIEM/WEF              | ☐   | ☐     | ☐   | …                | P?       |
| Durcissement rôle     | IIS/SQL/AD DS : contrôles spécifiques                 | Voir sections         | ☐   | ☐     | ☐   | …                | P?       |

> **Ajoutez des lignes** pour vos contrôles internes (PKI, bastion, PAM, sauvegardes immuables, etc.).

---

## 5) Détail des contrôles & procédures de vérification

### 5.1 Build & correctifs
- **Attendu** : système supporté, dernière CU/SSU ≤ 30 jours.
- **Vérification** : version (winver), `systeminfo`, historique Windows Update, inventaire patching.
- **Évidence** : capture winver, export KB installés.

### 5.2 Sécurité du démarrage & matériel de confiance
- **Secure Boot** activé, **TPM 2.0** présent/actif.
- **VBS/HVCI** : Virtualization‑Based Security + Hypervisor‑enforced Code Integrity actifs.
- **Credential Guard** + **LSASS PPL**.
- **Vérification** : `msinfo32`, `tpm.msc`, `Get‑CimInstance Win32_DeviceGuard`, clés Registre LSA.

### 5.3 Comptes, identités et accès à distance
- Admin builtin renommé/désactivé, comptes locaux minimaux, **LAPS** déployé.
- Politiques de mot de passe (longueur ≥ 14, complexité, rotation).
- Accès admin via **bastion/jump** + **MFA**.
- **RDP** : NLA obligatoire, groupes autorisés, bannière légale, timeouts, pas d’exposition Internet.
- **WinRM** : HTTPS‑only, certificats valides, liste blanche de sources.

### 5.4 Réseau & pare‑feu
- Profils pare‑feu **activés** (Domain/Private/Public), inbound **DENY** par défaut, règles minimales par rôle.
- Journalisation pare‑feu (allowed/blocked) et rotation.
- **Egress** : règles sortantes restreintes (ou justification documentée si non applicable).

### 5.5 Protocoles & chiffrement
- **SMBv1** désactivé, **NTLMv1** refusé ; réduction progressive de NTLM (audit → blocage) si applicatif.
- **TLS** : TLS 1.2/1.3 activés ; SSLv3/TLS1.0/TLS1.1 désactivés ; suites conformes.

### 5.6 Protection poste serveur
- **Antivirus/EDR** actif, signatures à jour, protection anti‑altération (**Tamper Protection**).
- **ASR** (Attack Surface Reduction) : politique Audit puis Block sur règles pertinentes (Office child, LSASS, scripts).
- **Exploit Protection** : politique appliquée et testée.
- **Contrôle applicatif** : AppLocker/WDAC selon rôle critique.

### 5.7 Journalisation, audit, centralisation
- **Taille & rétention** des journaux augmentées (Security ≥ 128 MB, PowerShell ≥ 32 MB, etc.).
- **Audit avancé** : Logon/Logoff, Process Creation (4688), Credential Validation, Account Lockout, Object Access.
- **PowerShell logging** : ScriptBlock Logging + Transcription.
- **Centralisation** : WEF/agent SIEM, horodatage fiable (NTP).

### 5.8 Chiffrement & sauvegardes
- **BitLocker** sur disque système et données ; stockage sécurisé des clés (AD/Azure AD/coffre).
- Sauvegardes régulières, **test de restauration** documenté, séparation des droits, immutabilité si possible.

### 5.9 Durcissement par rôle (évaluer uniquement si applicable)
- **AD DS (Contrôleur de domaine)** : pas d’apps tierces, journalisation renforcée, NTLM durci, audits 4719/4732.
- **IIS** : Request Filtering, en‑têtes sécurité (HSTS, X‑Content‑Type‑Options, etc.), pools d’appl dédiés, droits NTFS minimaux, TLS durci.
- **SQL Server** : chiffrement TLS, ports restreints, comptes de service gérés (gMSA), backups chiffrés, audit SQL.
- **Serveur de fichiers** : SMB signing, ACL minimales, quotas/FSRM (filtres), audit accès objet.
- **Jump server/RDS** : MFA obligatoire, enregistrement de session, accès Just‑In‑Time.

---

## 6) Registre & paramètres clés (lecture seule durant l’audit)
> **Ne pas modifier en production pendant l’audit** ; collecter seulement la valeur et la preuve.
- `HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\EnableVirtualizationBasedSecurity` = 1
- `HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity\Enabled` = 1
- `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\LsaCfgFlags` = 1 (Credential Guard)
- `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\RunAsPPL` = 1 (LSASS PPL)
- `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\LmCompatibilityLevel` ≥ 5 (refus LM/NTLMv1)
- `HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters\RequireSecuritySignature` = 1 (SMB signing)

---

## 7) Collecte de preuves (exemples à ajuster)
- **Inventaire** : export fonctionnalités/patchs, liste des services, membres groupes locaux.
- **Sécurité noyau** : export DeviceGuard (msinfo32), captures TPM/Secure Boot.
- **Pare‑feu** : export des profils et règles actives.
- **EDR/Defender** : statut temps réel, ASR, signatures.
- **Audit** : `auditpol /get /category:*`, tailles journaux, paramètres PowerShell logging.
- **Chiffrement** : état BitLocker, preuve stockage des clés.

---

## 8) Cartographie référentiels (traceabilité)
| Contrôle | CIS WS2022 | MS Baseline | ANSSI | ISO 27001 |
|---|---|---|---|---|
| VBS/HVCI | x.x | SecGuide / DeviceGuard | R16 | A.8/A.12 |
| Credential Guard | x.x | SecGuide / LSA | R16 | A.8/A.12 |
| Pare‑feu | x.x | Firewall | R30 | A.13 |
| NTLM/SMB | x.x | LanMan/NTLM | R23 | A.9 |
| TLS/Schannel | x.x | TLS settings | R35 | A.10 |
| Audit | x.x | Audit Policy | R40 | A.12 |
| BitLocker | x.x | BitLocker | R61 | A.8 |
> Remplir les références exactes selon les versions utilisées.

---

## 9) Registre des constats (modèle de fiche constat)
**ID Constat** : WS2022‑XXX  
**Titre** : ………………………………………………………………………………  
**Description** : ………………………………………………………………………  
**Preuves** : (captures, exports, logs)  
**Impact** : (confidentialité/intégrité/disponibilité, conformité)  
**Probabilité** : (Faible/Moyenne/Élevée)  
**Sévérité** : P1/P2/P3/P4  
**Contrôles associés** : (CIS/MS/ANSSI/ISO)  
**Recommandation** : (mesure de remédiation concrète)  
**Échéance** : JJ/MM/AAAA  
**Propriétaire** : ………………………  
**Statut** : Ouvert / En cours / Résolu / Accepté (avec justification)

---

## 10) Plan de remédiation (exemple de trame)
| Priorité | Action | Détail | Responsable | Échéance | KPI/Preuve de clôture |
|---|---|---|---|---|---|
| P1 | Activer Credential Guard + LSASS PPL | Validation compatibilité drivers | Équipe Systèmes | … | Export DeviceGuard + capture Reg |
| P1 | Durcir RDP (NLA, restriction groupes) | Bannière, timeout, bastion | Équipe Systèmes | … | GPO appliquée + test |
| P2 | Désactiver TLS1.0/1.1 | Revue compatibilité clients | Réseau/Sécu | … | Scan SSL + rapport |
| P2 | Activer ASR (progressif) | Audit → Block + exceptions | SecOps | … | Tableau incidents post‑déploiement |
| P3 | Egress policy par rôle | Modèle de flux sortants | Réseau | … | Export règles + validation applicative |

---

## 11) Annexes
- **Glossaire** : VBS, HVCI, CG, ASR, WDAC, WEF, gMSA, etc.
- **Hypothèses & limites** : pas de test de charge, pas d’intrusion active, accès en lecture seule.
- **Liste de distribution du rapport** : ………………………………………….

---

### Signature & validation
- **Rédigé par** : ....................................  **Date** : …………
- **Validé par (client)** : ............................  **Date** : …………

