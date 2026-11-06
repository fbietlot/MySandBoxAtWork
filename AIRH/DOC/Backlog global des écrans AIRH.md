# Backlog global AIRH priorisé

## Périmètre et complétude

Les quatre exports fournis sont complets au niveau de leur structure
hiérarchique déclarée :

-   [App.Formulas.fx.json]  : 1 796 lignes
-   [Screen_Document_D.fx.json] : 473 lignes
-   [Screen_Form_F.fx.json] : 5 402 lignes
-   [Screen_Main_M.fx.json] : 7 778 lignes
    

**Limites précises de l'audit :**

-   Le code interne de cmpActionButton et cmpProcessTimeline n'est pas
    fourni. Les conclusions sur IsLoading, StateValues, OnAvailable,
    OnAlreadyRunning et ProcessStepStates restent donc à vérifier dans
    les composants.

-   Les propriétés de l'objet App, notamment OnStart, ainsi que la
    définition de vvAppConfig, ne sont pas fournies.

-   Les schémas SharePoint et les Power Automate AIRH_Upload_Files_ToSP,
    AIRH_GetAllLinkedDocuments ne sont pas fournis.

-   L'analyse visuelle repose sur les valeurs déclarées, pas sur un
    rendu aux différentes résolutions.

Les éléments ci-dessous sont des **corrections certaines**, sauf la
seconde partie explicitement présentée comme recommandations.

# Corrections certaines

## 1. Corriger la visibilité de frmGeneralInfo_F

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** frmGeneralInfo_F.Visible lit
    vcStatutConfig.Controls.FormNextStep.Visible au lieu de
    vcStatutConfig.Controls.FormGeneralInfo.Visible. La configuration
    canonique distingue pourtant explicitement ces deux formulaires.

-   **Concerné :** Screen_Form_F, frmGeneralInfo_F.Visible,
    \_StatutConfigTable.Controls.FormGeneralInfo

-   **Proposition :** Faire dépendre frmGeneralInfo_F.Visible de
    Controls.FormGeneralInfo.Visible.

-   **Gain attendu :** Respect effectif de la matrice déclarative des
    statuts et suppression d'un risque de masquage ou d'affichage
    incorrect.

-   **Risque de régression :** Faible

-   **Effort :** Très faible

-   **Test recommandé :** Parcourir un statut avec FormNextStep.Visible
    = false, puis un statut avec FormNextStep.Visible = true, et
    vérifier que frmGeneralInfo_F reste piloté uniquement par
    FormGeneralInfo.Visible.
    
### Modification
#### AS-IS
```
- frmGeneralInfo_F:
    Control: Form@2.4.4
    Layout: Vertical
    Properties:
      DataSource: =AIRH_Demandes
      DefaultMode: =Coalesce(vcStatutConfig.Controls.FormGeneralInfo.Mode,FormMode.View)
      Height: =Parent.Height
      Item: =vcDemande
      NumberOfColumns: =2
      SnapToColumns: =false
      Visible: =Coalesce( vcStatutConfig.Controls.FormNextStep.Visible,true)
      Width: =Parent.Width
```
#### TO-BE
```
- frmGeneralInfo_F:
    Control: Form@2.4.4
    Layout: Vertical
    Properties:
      DataSource: =AIRH_Demandes
      DefaultMode: =Coalesce(vcStatutConfig.Controls.FormGeneralInfo.Mode,FormMode.View)
      Height: =Parent.Height
      Item: =vcDemande
      NumberOfColumns: =2
      SnapToColumns: =false
      Visible: =Coalesce( vcStatutConfig.Controls.FormGeneralInfo.Visible,true)
      Width: =Parent.Width
```
    

## 2. Corriger l'historisation lors de la constitution d'un lot

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** Dans btnCreateLotSave_M.OnSelect, la boucle traite
    dlstDemandes_M.SelectedItems As MyDemande, mais appelle
    fxLogStatusHistory(ShowColumns(vcDemande,\...)). vcDemande n'est pas
    la demande courante de cette boucle et peut être vide ou résiduelle.

-   **Concerné :** Screen_Main_M, btnCreateLotSave_M.OnSelect,
    fxLogStatusHistory

-   **Proposition :** Historiser MyDemande ou déplacer l'historisation
    hors de la boucle avec une table regroupée par statut cible.

-   **Gain attendu :** Historique fiable pour chaque demande du lot.

-   **Risque de régression :** Faible si le remplacement est local,
    moyen en cas de regroupement.

-   **Effort :** Faible

-   **Test recommandé :** Constituer un lot comportant au moins une
    demande A4 et une non-A4, puis vérifier deux entrées dans
    AIRH_Status_History, avec les bons ID_Demande, ProcessStepKey et
    statuts cibles.
### Modification
#### AS-IS
```
- btnCreateLotSave_M:
    Control: ModernButton@1.0.0
    Properties:
      OnSelect: |-
        =
        ...
        // Create the 'Lot' by updating each selected 'Demande' to its next status (A4 path vs standard path)
        ForAll(dlstDemandes_M.SelectedItems As MyDemande;
        With({
            MyDemandeNextStatus:        
                If(MyDemande.Rang = "A4";
                LookUp(_StatutConfigTable;StatusKey = _StatusKey.DirReaffConsult);
                LookUp(_StatutConfigTable;StatusKey = _StatusKey.DACDVEnPrevision)
                )
            };

            Patch(AIRH_Demandes;
                ShowColumns( MyDemande;ID);
                {
                    StatusKey:          MyDemandeNextStatus.StatusKey;
                    'Statut Detail':    MyDemandeNextStatus.StatutDetail;
                    'Statut General':   MyDemandeNextStatus.StatutGeneral
                };
                If(MyDemande.Rang = "A4";
                    // Route 'Demande' through the director reaffectation consultation (with end date)
                    {
                        Lot : inpNumLot_M.Text;
                        DateDirReaffFinConsult: steCreateLotDirReaffFinConsult_M.SelectedDate
                    };
                    // Route 'Demande' through the standard DV validation pipeline
                    {
                        Lot : inpNumLot_M.Text
                    }
                )
            );;
        
            // # Trace chaque changement de statut dans l'historique
            fxLogStatusHistory(
                ShowColumns(vcDemande,ID,'Numéro d''emploi'),
                MyDemandeNextStatus.StatusKey,
                vvUserRole.Key
            )
        )
    );
```
#### TO-BE
```
    // Create the 'Lot' by updating each selected 'Demande' to its next status (A4 path vs standard path)
    ForAll(dlstDemandes_M.SelectedItems As MyDemande;
        With({
            MyDemandeNextStatus:        
                If(MyDemande.Rang = "A4";
                LookUp(_StatutConfigTable;StatusKey = _StatusKey.DirReaffConsult);
                LookUp(_StatutConfigTable;StatusKey = _StatusKey.DACDVEnPrevision)
                )
            };

            Patch(AIRH_Demandes;
                ShowColumns( MyDemande;ID);
                {
                    StatusKey:          MyDemandeNextStatus.StatusKey;
                    'Statut Detail':    MyDemandeNextStatus.StatutDetail;
                    'Statut General':   MyDemandeNextStatus.StatutGeneral
                };
                If(MyDemande.Rang = "A4";
                    // Route 'Demande' through the director reaffectation consultation (with end date)
                    {
                        Lot : inpNumLot_M.Text;
                        DateDirReaffFinConsult: steCreateLotDirReaffFinConsult_M.SelectedDate
                    };
                    // Route 'Demande' through the standard DV validation pipeline
                    {
                        Lot : inpNumLot_M.Text
                    }
                )
            );;

           // # Trace chaque changement de statut dans l'historique
           // L'historisation est faite demande par demande car le statut peut être différent à chaque fois
            fxLogStatusHistory(
                ShowColumns(MyDemande;ID;NumEmploi);
                MyDemandeNextStatus.StatusKey;
                vvUserRole.Key
            )
        )
    );;
```
    

## 3. Ajouter StatusKey lors de la création technique d'une demande

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** btnGalNewDemande.OnSelect crée immédiatement un
    enregistrement avec \'Statut General\' et \'Statut Detail\', mais ne
    renseigne pas StatusKey. Le reste du système, notamment
    \_StatutConfigTable, vcStatutConfig, la grille et l'historisation,
    utilise StatusKey comme clé canonique.

-   **Concerné :** Screen_Main_M, btnGalNewDemande.OnSelect,
    \_StatusKey.DemandeNew

-   **Proposition :** Renseigner aussi StatusKey:
    MyStatusDetail.StatusKey dans le Patch initial.

-   **Gain attendu :** Élimination d'un état temporaire incohérent et
    sécurisation des reprises après interruption.

-   **Risque de régression :** Très faible

-   **Effort :** Très faible

-   **Test recommandé :** Commencer une création, interrompre la
    navigation avant validation, puis contrôler l'enregistrement
    SharePoint et son rechargement dans ccDemandes.
### Modification
#### AS-IS
```
- btnGalNewDemande:
    Control: ModernButton@1.0.0
    Properties:
      OnSelect: |-
        =With({
...
                Patch(AIRH_Demandes,
                    Defaults(AIRH_Demandes),
                    {
                        'Type de demande': {Value : ThisItem.Value}
                    },
                    {
                        'Statut General': MyStatusDetail.StatutGeneral,
                        'Statut Detail':  MyStatusDetail.StatutDetail
                    }
                )
            });
...

```
#### TO-BE
```
- btnGalNewDemande:
    Control: ModernButton@1.0.0
    Properties:
      OnSelect: |-
        =With({
...
                Patch(AIRH_Demandes,
                    Defaults(AIRH_Demandes),
                    {
                        'Type de demande': {Value : ThisItem.Value}
                    },
                    {
                        'Statut General': MyStatusDetail.StatutGeneral,
                        'Statut Detail':  MyStatusDetail.StatutDetail
                    }
                )
            });
...
```

## 4. Supprimer le doublon \_ActionOnList.CODIDecision dans Screen_Main_M.OnVisible

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** Le même cas apparaît deux fois dans
    Switch(vcCurrentProcess,\...). Le premier exécute
    Select(BtnLoadDemandes_M) et le second retourne false. Le second cas
    est inatteignable.

-   **Concerné :** Screen_Main_M.OnVisible

-   **Proposition :** Conserver un seul comportement, après validation
    du besoin de reprise du processus CODI.

-   **Gain attendu :** Suppression d'une ambiguïté et comportement de
    retour lisible.

-   **Risque de régression :** Faible, sous réserve de confirmer le
    comportement attendu après retour de Screen_Document_D.

-   **Effort :** Très faible

-   **Test recommandé :** Ouvrir puis quitter Screen_Document_D dans le
    contexte \_ActionOnList.CODIDecision et vérifier la conservation de
    vcWizardStep, de la sélection et du filtre.
### Modification
#### AS-IS
```
Screens:
  Screen_Main_M:
    Properties:
      OnVisible: |-
...
        Switch(vcCurrentProcess,
            _ActionOnList.Finaliser,
                Select(BtnLoadDemandes_M), // on recharge la liste avec le statut mis à jour mais on ne touche pas au filtre
            ...
            _ActionOnList.CODIDecision,
                Select(BtnLoadDemandes_M), // on recharge la liste avec le statut mis à jour mais on ne touche pas au filtre
            _ActionOnList.PromReception,
                Select(BtnLoadDemandes_M), // on recharge la liste avec le statut mis à jour mais on ne touche pas au filtre
            _ActionOnList.GWDecision , 
                false, // Si on est en cours de process on ne doit pas recharger et reinitialiser
            _ActionOnList.CODIDecision , 
                false, // Si on est en cours de process on ne doit pas recharger et reinitialiser
            ...
...
```
#### TO-BE
```
Screens:
  Screen_Main_M:
    Properties:
      OnVisible: |-
...
        Switch(vcCurrentProcess,
            _ActionOnList.Finaliser,
                Select(BtnLoadDemandes_M), // on recharge la liste avec le statut mis à jour mais on ne touche pas au filtre
            ...
            _ActionOnList.CODIDecision,
                Select(BtnLoadDemandes_M), // on recharge la liste avec le statut mis à jour mais on ne touche pas au filtre
            _ActionOnList.PromReception,
                Select(BtnLoadDemandes_M), // on recharge la liste avec le statut mis à jour mais on ne touche pas au filtre
            _ActionOnList.GWDecision , 
                false, // Si on est en cours de process on ne doit pas recharger et reinitialiser
            ...
...
```

## 5. Corriger les références croisées résiduelles dans btnCreateLotSave_M

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** btnCreateLotSave_M.OnSelect initialise
    MySourceStatuses depuis cmpActBtnSendDAC_M.SourceStatusesData, alors
    que l'action courante est \_ActionOnList.CreateLot portée par
    cmpActBtnCreateLot_M.

-   **Concerné :** Screen_Main_M, btnCreateLotSave_M.OnSelect,
    cmpActBtnCreateLot_M, cmpActBtnSendDAC_M

-   **Proposition :** Utiliser cmpActBtnCreateLot_M.SourceStatusesData.

-   **Gain attendu :** Découplage du processus de constitution de lot et
    fiabilité du filtre post-traitement.

-   **Risque de régression :** Très faible

-   **Effort :** Très faible

-   **Test recommandé :** Terminer une constitution de lot et vérifier
    que vcApplyFilterOnStatus_V3 contient le statut source de
    cmpActBtnCreateLot_M et les statuts cibles attendus.
### Modification
#### AS-IS
```
- btnCreateLotSave_M:
    Control: ModernButton@1.0.0
    Properties:
      OnSelect: |-
        =With({
                // Statut cible 
                //(à priori déclaré sur le bouton de lancement du processus)
                // MyNextStatus:        cmpActBtnSendDAC_M.NextStatusData;
                // Statuts depuis lesquels le processus peut être exécuté 
                // (à priori déclaré sur le bouton de lancement du processus)
                // (utilisés plus loin pour maintenir le filtre après traitement)
                MySourceStatuses :  cmpActBtnSendDAC_M.SourceStatusesData
            },
...

```
#### TO-BE
```
- btnCreateLotSave_M:
    Control: ModernButton@1.0.0
    Properties:
      OnSelect: |-
        =With({
                // Statut cible 
                //(à priori déclaré sur le bouton de lancement du processus)
                // MyNextStatus:        cmpActBtnSendDAC_M.NextStatusData;
                // Statuts depuis lesquels le processus peut être exécuté 
                // (à priori déclaré sur le bouton de lancement du processus)
                // (utilisés plus loin pour maintenir le filtre après traitement)
                MySourceStatuses :  cmpActBtnCreateLot_M.SourceStatusesData
            },
...
```
   

## 6. Corriger les contrôles de sortie associés aux mauvais processus

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** Plusieurs OnAlreadyRunning ciblent un bouton de sortie
    appartenant à un autre processus. cmpActBtnGWDecision_M appelle
    btnSendGWCancel_M et cmpActBtnPromEnvoi_M appelle
    btnAppelCanidExit_M. Ces références ressemblent à des résidus de
    duplication.

-   **Concerné :** cmpActBtnGWDecision_M.OnAlreadyRunning,
    cmpActBtnPromEnvoi_M.OnAlreadyRunning, boutons de sortie des
    panneaux concernés

-   **Proposition :** Pointer chaque action vers sa sortie locale ou
    vers btnGenericExitProcess_M.

-   **Gain attendu :** Annulation cohérente et absence d'état de wizard
    résiduel.

-   **Risque de régression :** Faible

-   **Effort :** Très faible

-   **Test recommandé :** Lancer chaque processus, recliquer sur son
    action, puis vérifier la remise à blanc de vcCurrentProcess,
    vcWizardStep, vcApplyFilter et de la sélection.
    
### Modification
#### AS-IS
```
- cmpActBtnGWDecision_M:
    Control: CanvasComponent
    ComponentName: cmpActionButton
    Properties:
      OnAlreadyRunning: =Select(btnSendGWCancel_M); //Arrêter Le processus

- cmpActBtnPromEnvoi_M:
    Control: CanvasComponent
    ComponentName: cmpActionButton
    Properties:
      OnAlreadyRunning: =Select(btnAppelCanidExit_M); //Arrêter Le processus
```
#### TO-BE
```
- cmpActBtnGWDecision_M:
    Control: CanvasComponent
    ComponentName: cmpActionButton
    Properties:
      OnAlreadyRunning: =Select(btnGenericExitProcess_M); //Arrêter Le processus

- cmpActBtnPromEnvoi_M:
    Control: CanvasComponent
    ComponentName: cmpActionButton
    Properties:
      OnAlreadyRunning: =Select(btnGenericExitProcess_M); //Arrêter Le processus

```
## 7. Corriger l'icône de l'action d'approbation GW

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** cmpActBtnPromAppro_F.Icon vaut \"DismissCircle\",
    comme le bouton de refus cmpActBtnPromRefus_F. Le bouton exécute
    pourtant \_ActionOnForm.PromAppro.

-   **Concerné :** Screen_Form_F, cmpActBtnPromAppro_F.Icon

-   **Proposition :** Utiliser une icône de validation cohérente avec
    les autres approbations, par exemple \"CheckmarkCircle\".

-   **Gain attendu :** Réduction du risque d'erreur utilisateur sur une
    décision irréversible.

-   **Risque de régression :** Très faible

-   **Effort :** Très faible

-   **Test recommandé :** Afficher simultanément les actions
    \_ActionOnForm.PromAppro et \_ActionOnForm.PromRefus et vérifier
    leur distinction visuelle et accessible.
### Modification
#### AS-IS
```
- cmpActBtnPromAppro_F:
    Control: CanvasComponent
    ComponentName: cmpActionButton
    Properties:
      Icon: ="DismissCircle"
```
#### TO-BE
```
- cmpActBtnPromAppro_F:
    Control: CanvasComponent
    ComponentName: cmpActionButton
    Properties:
      Icon: ="CheckmarkCircle"
```


## 8. Corriger les AccessibleLabel incorrects ou vides

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** Plusieurs composants ont un AccessibleLabel vide ou
    copié d'une autre action. Exemples : cmpActBtnCODICandidat_F annonce
    \"Bouton Refuser la demande\", plusieurs actions principales de
    Screen_Main_M annoncent une décision GW ou une modification sans
    rapport avec l'action réelle.

-   **Concerné :** Composants cmpActionButton des écrans Screen_Main_M
    et Screen_Form_F

-   **Proposition :** Définir systématiquement AccessibleLabel =
    \"Bouton \" & Self.Action, sauf lorsqu'un libellé métier plus précis
    est nécessaire.

-   **Gain attendu :** Accessibilité lecteur d'écran et diminution des
    erreurs d'action.

-   **Risque de régression :** Très faible

-   **Effort :** Faible

-   **Test recommandé :** Parcours clavier avec Narrateur Windows sur
    les actions visibles des rôles DFA et DAC.
### Modification

#### TO-BE
Pour les AccessibleLabel qui n'ont pas été défini explicitment, utilisation systematique de `"Bouton " & Self.Action`
```
- cmpActBtnAddDoc_F:
    Control: CanvasComponent
    ComponentName: cmpActionButton
    Properties:
        AccessibleLabel: ="Bouton " & Self.Action
```

## 9. Corriger le libellé et la logique du conteneur GW intermédiaire

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** ctnGWDecisionDateComment_M.Visible vaut
    radGWdecisionChoice_M.Selected.Value = Blank(), alors que son texte
    annonce \"Date de DV / Commentaire Refus\". Il disparaît précisément
    après le choix qui détermine laquelle de ces informations est
    requise.

-   **Concerné :** Screen_Main_M, ctnGWDecisionDateComment_M.Visible,
    lblGWDecisionDateComment_M.Text

-   **Proposition :** Vérifier si ce conteneur est un simple titre
    d'étape. S'il l'est, l'afficher après un choix non vide ou le
    supprimer au profit des conteneurs spécifiques ctnGWDecisionDate_M
    et ctnGWDecisionComment_M.

-   **Gain attendu :** Wizard GW compréhensible et absence d'étape
    fantôme.

-   **Risque de régression :** Faible

-   **Effort :** Très faible

-   **Test recommandé :** Tester successivement \_Decision.Validée,
    \_Decision.Refusée et l'absence de choix, en contrôlant les étapes
    visibles.
    
### Modification
#### AS-IS
```
- ctnGWDecisionDateComment_M:
    Control: GroupContainer@1.5.0
    Variant: AutoLayout
    Properties:
      Visible: =radGWdecisionChoice_M.Selected.Value = Blank()
- lblGWDecisionDateComment_M:
    Control: ModernText@1.0.0
    Properties:
      Text: |
... 
            & " Date de DV / Commentaire Refus"
        )

```

#### TO-BE
```
- ctnGWDecisionDateComment_M:
    Control: GroupContainer@1.5.0
    Variant: AutoLayout
    Properties:
      Visible: |-
        =// Placeholder de l'étape 4 en attendant le choix utilisateur concernant la décision du GW
        radGWdecisionChoice_M.Selected.Value = Blank()

- lblGWDecisionDateComment_M:
    Control: ModernText@1.0.0
    Properties:
      Text: |
... 
            & " Informations complémentaires"
        )



```
## 10. Sécuriser la suppression d'une demande liée à des documents

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** btnDeleteCancel_F_2.OnSelect supprime vcDemande, avec
    un commentaire [TODO] Remove ghost link to document. Les documents
    utilisent pourtant un multi-lookup DemandesLiees. La suppression
    peut donc laisser des liens orphelins ou dépendre d'un comportement
    SharePoint non établi.

-   **Concerné :** Screen_Form_F, btnDeleteCancel_F_2.OnSelect,
    AIRH_Documents.DemandesLiees

-   **Proposition :** Avant suppression, délier la demande de tous les
    documents associés ou confirmer par test que SharePoint nettoie
    automatiquement le multi-lookup.

-   **Gain attendu :** Intégrité documentaire et absence de références
    fantômes.

-   **Risque de régression :** Moyen

-   **Effort :** Moyen

-   **Test recommandé :** Créer une demande liée à deux documents, la
    supprimer, puis contrôler les deux éléments de AIRH_Documents et le
    résultat de AIRH_GetAllLinkedDocuments.
### Documentation [TODO]
Clarification du commentaire [TODO]
>   // [TODO] Avant de supprimer la demande, retirer sa référence du champ DemandesLiees de tous les documents liés afin d'éviter les liens documentaires orphelins

    

## 11. Bloquer la double exécution des traitements non couverts par le spinner

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** Les traitements métier sont souvent longs, avec Patch,
    historique et courriel. Les spinners ne couvrent que IsLoading des
    cmpActionButton. Les boutons standards de sauvegarde des wizards,
    comme btnGWDecisionSave_M, btnCreateLotSave_M ou
    btnCandAppelidSave_M, ne sont pas inclus.

-   **Concerné :** ctnSpinner_M.Visible, boutons \*Save_M, opérations
    Patch et Office365Outlook.SendEmailV2

-   **Proposition :** Introduire un verrou partagé de traitement ou
    migrer les boutons finaux vers le composant d'action.

-   **Gain attendu :** Prévention des doubles Patch, doubles historiques
    et doubles courriels.

-   **Risque de régression :** Moyen

-   **Effort :** Moyen

-   **Test recommandé :** Double-cliquer rapidement chaque bouton final
    et vérifier une seule transition, une seule entrée d'historique et
    un seul courriel.
### Documentation [TODO]
Clarification du commentaire [TODO]
```
- ctnSpinner_M:
    Control: GroupContainer@1.5.0
    Variant: ManualLayout
    Properties:
      Fill: =ColorValue("#80808080")
      Height: =Parent.Height - Self.Y - ctnMainContainer_M.PaddingBottom
      Visible: |
        =// [TODO] Sécuriser les traitements déclenchés par les boutons *Save_M 
        contre les doubles exécutions : le spinner actuel couvre uniquement IsLoading des cmpActionButton 
        et ne verrouille pas ces boutons pendant les Patch, l'historisation et l'envoi des notifications.
        Prévoir un état de traitement partagé pour désactiver les actions concernées 
        et afficher le spinner jusqu'à la fin complète du traitement.
        cmpActBtnNew_M.IsLoading
        || cmpActBtnFinaliser_M.IsLoading
        || cmpActBtnSendDAC_M.IsLoading
```

    

## 12. Gérer explicitement les erreurs des traitements de masse

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Haute

-   **Constat :** Les wizards de Screen_Main_M exécutent généralement
    ForAll(\...Patch\...), puis historisent, notifient et rechargent
    sans contrôler Errors(AIRH_Demandes) ou IfError. Screen_Form_F
    effectue, lui, ce contrôle après plusieurs mises à jour
    individuelles.

-   **Concerné :** Screen_Main_M, boutons finaux des processus de masse,
    fxLogStatusHistory

-   **Proposition :** Ne lancer historique, courriel et message de
    succès qu'après validation de tous les Patch. Prévoir un retour
    partiel avec les ID en erreur.

-   **Gain attendu :** Absence de faux succès et d'historique
    discordant.

-   **Risque de régression :** Moyen

-   **Effort :** Moyen

-   **Test recommandé :** Provoquer un échec SharePoint sur une demande
    d'un lot et vérifier qu'elle n'est ni historisée ni annoncée comme
    traitée.
### Remarque ignorée
Cela nécésitte une implémentation générale de la gestion des erreurs qui ne peut être faite à ce stade du projet
    

## 13. Corriger les lectures de valeurs antérieures après Patch

-   **Catégorie :** Correction nécessaire

-   **Priorité :** Moyenne

-   **Constat :** Plusieurs courriels utilisent ensuite
    vcDemande.CommentValidationCOSTRA, vcDemande.CODICandidatRetenu ou
    vcDemande.CommentPromGW. Bien que vcDemande soit mis à jour via
    UpdateContext autour du Patch, l'ordre et les champs effectivement
    retournés doivent rester fiables. Certains messages pourraient
    reprendre la valeur antérieure plutôt que l'entrée utilisateur.

-   **Concerné :** cmpActBtnCOSTRARefus_F.OnAvailable,
    cmpActBtnCODICandidat_F.OnAvailable,
    cmpActBtnPromRefus_F.OnAvailable

-   **Proposition :** Utiliser les valeurs sources connues du
    traitement, par exemple frmNextStep_F.Updates ou les contrôles
    sélectionnés, dans le corps du courriel.

-   **Gain attendu :** Notifications cohérentes avec la décision venant
    d'être enregistrée.

-   **Risque de régression :** Faible

-   **Effort :** Faible

-   **Test recommandé :** Modifier une valeur, déclencher l'action
    immédiatement et comparer SharePoint, historique et contenu reçu.
### Remarque ignorée
Fortement lié au point de gestion des erreurs
>Après un Patch, ne poursuivre l'historisation et l'envoi des notifications que si l'enregistrement a réussi. Lorsque vcDemande contient le résultat du Patch, l'utilisation de ses valeurs dans la notification est cohérente et permet même de se baser sur les données effectivement retournées par SharePoint.

# Harmonisation, mutualisation et architecture

## 14. Mutualiser la transaction standard de changement de statut

-   **Catégorie :** Mutualisation ou simplification

-   **Priorité :** Haute

-   **Constat :** La séquence LookUp statut cible → Patch
    StatusKey/Statut Detail/Statut General → Errors → fxLogStatusHistory
    → Reset/Navigate/Refresh est répétée dans la majorité des actions de
    Screen_Form_F et des wizards de Screen_Main_M.

-   **Concerné :** Actions cmpActBtn\*\_F.OnAvailable, boutons
    \*Save_M.OnSelect, fxLogStatusHistory

-   **Proposition :** Créer une fonction ou un mécanisme partagé pour la
    partie strictement commune, en gardant les champs métier,
    notifications et différences individuel/masse comme paramètres ou
    étapes spécifiques.

-   **Gain attendu :** Forte réduction de duplication et correction
    uniforme des erreurs.

-   **Risque de régression :** Élevé si réalisé en une seule migration

-   **Effort :** Élevé

-   **Test recommandé :** Migration pilote sur une action simple sans
    courriel, puis comparaison des données avant extension.
### Remarque ignorée
Cela nécésitte une implémentation générale qui ne peut être faite à ce stade du projet
    

## 15. Mutualiser l'initialisation et la sortie des wizards

-   **Catégorie :** Mutualisation ou simplification

-   **Priorité :** Haute

-   **Constat :** Les OnAvailable des actions de liste répètent
    ccValidForTypeDemande, UpdateIf(ccDemandes\...),
    Select(btnGridClearSelect_M) et l'initialisation de
    vcCurrentProcess, vcWizardStep, vcWizardCompleted,
    vcWizardStepAfterSelect, vcApplyFilter, vcApplyFilterOnStatus_V3.
    Les sorties répètent également des remises à zéro, parfois
    différentes.

-   **Concerné :** Screen_Main_M, cmpActBtn\*\_M.OnAvailable,
    btnGenericExitProcess_M et sorties locales

-   **Proposition :** Centraliser l'entrée et la sortie standard, puis
    ne conserver localement que les Reset et paramètres spécifiques.

-   **Gain attendu :** Élimination des divergences de reprise,
    d'annulation et de sélection.

-   **Risque de régression :** Moyen

-   **Effort :** Moyen

-   **Test recommandé :** Matrice de test pour chaque processus :
    entrer, annuler avant sélection, annuler après sélection, terminer,
    revenir de Screen_Document_D.
### Remarque ignorée
Cela nécésitte une implémentation générale qui ne peut être faite à ce stade du projet 

## 16. Remplacer les contrôles numériques cachés des étapes par une configuration

-   **Catégorie :** Amélioration architecturale

-   **Priorité :** Moyenne

-   **Constat :** Chaque wizard utilise des ModernNumberInput de largeur
    0 comme constantes d'étape, puis répète le même Switch sur
    \_Emoji.\_1 à \_Emoji.\_6.

-   **Concerné :** Contrôles num\*, libellés lbl\*, \_Emoji,
    vcWizardStep

-   **Proposition :** Déclarer les étapes dans une table locale ou une
    configuration par processus et rendre les étapes via un composant de
    wizard.

-   **Gain attendu :** Réduction importante du nombre de contrôles et
    suppression des décalages d'étapes après duplication.

-   **Risque de régression :** Moyen

-   **Effort :** Élevé

-   **Test recommandé :** Comparaison visuelle et fonctionnelle sur un
    wizard à embranchement, idéalement GWDecision ou CreateLot.
### Remarque ignorée
Cela nécésitte une implémentation générale qui ne peut être faite à ce stade du projet

## 17. Centraliser la construction des notifications

-   **Catégorie :** Mutualisation ou simplification

-   **Priorité :** Moyenne

-   **Constat :** Les courriels répètent la résolution des
    destinataires, le préfixe, le pied, les listes HTML et les contrôles
    IsBlank(MyRecipients). Plusieurs textes contiennent aussi des
    formulations ou accords différents.

-   **Concerné :** \_DACEmailList, \_DFAEmailList, \_txtMessage, appels
    Office365Outlook.SendEmailV2

-   **Proposition :** Centraliser le squelette des notifications et la
    construction de listes d'emplois, sans centraliser les contenus
    métier spécifiques.

-   **Gain attendu :** Cohérence des messages, maintenance plus sûre et
    gestion uniforme de l'absence de destinataire.

-   **Risque de régression :** Moyen

-   **Effort :** Moyen

-   **Test recommandé :** Tests DFA, DAC, destinataires absents,
    environnement de test et regroupement multi-SPW.
### Remarque ignorée
Cela nécésitte une implémentation générale qui ne peut être faite à ce stade du projet
    

## 18. Déplacer les validations déclaratives dans la configuration des statuts

-   **Catégorie :** Amélioration architecturale

-   **Priorité :** Moyenne

-   **Constat :** \_StatutConfigTable centralise les actions, documents
    et contrôles, mais txtValidationMsg_F contient encore un grand
    Switch dupliquant les exigences par statut. Les cas DemandeBrouillon
    et DACRefusee sont presque identiques.

-   **Concerné :** \_StatutConfigTable, txtValidationMsg_F,
    txtValidationAppro_F, txtValidationRefus_F

-   **Proposition :** Étendre progressivement la configuration avec les
    exigences documentaires et, si pertinent, les clés de validation de
    champs. Conserver les règles contextuelles complexes dans l'écran.

-   **Gain attendu :** Une source de vérité plus complète sans supprimer
    la flexibilité métier.

-   **Risque de régression :** Élevé

-   **Effort :** Élevé

-   **Test recommandé :** Table de décision par StatusKey, type de
    demande et action, comparée aux validations actuelles.
### Remarque ignorée
Cela nécésitte une implémentation générale qui ne peut être faite à ce stade du projet    

## 19. Harmoniser la forme de vcApplyFilterOnStatus_V3

-   **Catégorie :** Harmonisation recommandée

-   **Priorité :** Moyenne

-   **Constat :** La variable reçoit plusieurs formes :
    Table(MyNextStatus, MySourceStatuses), Table({StatusKey: \...},
    MySourceStatuses.StatusKey), Table([MyNextStatus\,
    MySourceStatuses) et une table de LookUp. La grille attend pourtant
    vcApplyFilterOnStatus_V3.StatusKey.

-   **Concerné :** Screen_Main_M, processus de masse,
    dlstDemandes_M.Items

-   **Proposition :** Normaliser la variable à une table mono-colonne ou
    à une table de configurations de statut, mais pas un mélange des
    deux.

-   **Gain attendu :** Filtrage post-traitement prévisible et formules
    plus simples.

-   **Risque de régression :** Moyen

-   **Effort :** Moyen

-   **Test recommandé :** Vérifier le filtre après chacun des processus
    avec une cible unique, plusieurs cibles et conservation des statuts
    sources.
### Remarque ignorée
Cela nécésitte une implémentation générale qui ne peut être faite à ce stade du projet     

## 20. Uniformiser la convention de visibilité des actions

-   **Catégorie :** Harmonisation recommandée

-   **Priorité :** Moyenne

-   **Constat :** 
    * Les boutons du formulaire appliquent généralement
     `MyAction in vcStatutConfig.Actions && MyAction in vvUserRole.Actions`
    * btnConsulter_M et cmpActBtnAddDoc_M utilisent seulement les droits du rôle. 
    * btnModifier_M combine statut et propriétaire, mais selon une autre convention.

-   **Concerné :** Screen_Main_M, Screen_Form_F, propriétés Visible

-   **Proposition :** Formaliser trois règles distinctes : action
    contextuelle au statut, action transversale, action réservée au
    propriétaire. Documenter explicitement les exceptions.

-   **Gain attendu :** Autorisations UI compréhensibles et cohérentes.

-   **Risque de régression :** Moyen

-   **Effort :** Moyen

-   **Test recommandé :** Matrice rôle × statut × action pour DFA et
    DAC.
### Remarque ignorée
Pas de valeur ajoutée

## 21. Réduire les tables de statut codées en dur dans les composants filtre/archive

-   **Catégorie :** Mutualisation ou simplification

-   **Priorité :** Moyenne

-   **Constat :** cmpActBtnFilters_M et cmpActBtnArchive_M
    reconstruisent manuellement des objets contenant \"DemandeNew\",
    \"DemandeBrouillon\", les libellés et l'acteur DFA alors que
    \_StatutConfigTable constitue la source canonique.

-   **Concerné :** cmpActBtnFilters_M.NextStatusData,
    SourceStatusesData, cmpActBtnArchive_M

-   **Proposition :** Utiliser des LookUp(\_StatutConfigTable, StatusKey
    = \...).

-   **Gain attendu :** Suppression de valeurs résiduelles en cas
    d'évolution d'un statut ou acteur.

-   **Risque de régression :** Faible

-   **Effort :** Faible

-   **Test recommandé :** Comparer la forme des enregistrements
    résultants avec ceux consommés par cmpActionButton.
### Modification
Clean up du code residuel par defaut du composant custo pour `cmpActBtnFilters_M` et `cmpActBtnArchive_M`
#### AS-IS
```
- cmpActBtnArchive_M:
    Control: CanvasComponent
    ComponentName: cmpActionButton
    Properties:
      NextStatusData: |-
        ={
            StatusKey:          "DemandeBrouillon",
            StatutDetail:       "Brouillon de demande",
            StatutGeneral:      "Formulaire de demande",
            ProcessStepKey:     "InitDemande",
            ActionOwner:        {Key : "DFA",   Icon : "📝",       Text : "📝 DFA"},
            ActionRequired:     true,
            LinkedTypeDemande:  []
        }
      SourceStatusesData: |-
        =Table(
            {
                StatusKey:          "DemandeNew",
                StatutDetail:       "Nouvelle demande",
                StatutGeneral:      "Formulaire de demande",
                ProcessStepKey:     "InitDemande",
                ActionOwner:        {Key : "DFA",   Icon : "📝",       Text : "📝 DFA"},
                LinkedTypeDemande:  []
            },
            {
                StatusKey:          "DemandeBrouillon",
                StatutDetail:       "Brouillon de demande",
                StatutGeneral:      "Formulaire de demande",
                ProcessStepKey:     "InitDemande",
                ActionOwner:        {Key : "DFA",   Icon : "📝",       Text : "📝 DFA"},
                ActionRequired:     true,
                LinkedTypeDemande:  []
            }
        )
```

#### TO-BE
```
- cmpActBtnArchive_M:
    Control: CanvasComponent
    ComponentName: cmpActionButton
    Properties:
      NextStatusData: =
      SourceStatusesData: =
```
    

## 22. Mutualiser le formatage des noms de fichiers

-   **Catégorie :** Mutualisation ou simplification

-   **Priorité :** Faible

-   **Constat :** La même formule Match supprimant le suffixe horodaté
    apparaît dans lblFileName_D.Text et Title7_1.Text.

-   **Concerné :** Screen_Document_D.lblFileName_D.Text,
    Screen_Form_F.Title7_1.Text

-   **Proposition :** Créer une fonction nommée de présentation du nom
    de fichier.

-   **Gain attendu :** Une seule convention de restitution des noms.

-   **Risque de régression :** Faible

-   **Effort :** Faible

-   **Test recommandé :** Tester un nom standard, sans extension, avec
    plusieurs points et sans horodatage.
### Remarque ignorée
Cela nécésitte une implémentation générale qui ne peut être faite à ce stade du projet        

# Polish visuel et ergonomique

## 23. Harmoniser les gabarits principaux des trois écrans

-   **Catégorie :** Polish visuel ou ergonomique

-   **Priorité :** Moyenne

-   **Constat :** Les trois écrans utilisent un fond
    RGBA(245,245,245,1), des marges de 16 et un header de 75, mais
    Screen_Main_M emploie un LayoutGap de 10 contre 16 pour les deux
    autres.

-   **Concerné :** ctnMainContainer_M, ctnMainContainer_F,
    ctnMainContainer_D

-   **Proposition :** Définir des constantes de layout ou un composant
    de shell commun.

-   **Gain attendu :** Cohérence visuelle et ajustements globaux plus
    simples.

-   **Risque de régression :** Faible

-   **Effort :** Moyen

-   **Test recommandé :** Comparaison à 1366×768, 1920×1080 et largeur
    réduite.
    ,
### Remarque ignorée
Les visuels des écrans sont volontairement différents en terme de mise en page    

## 24. Harmoniser les composants de boutons

-   **Catégorie :** Polish visuel ou ergonomique

-   **Priorité :** Moyenne

-   **Constat :** L'application mélange Button@0.0.45,
    ModernButton@1.0.0 et cmpActionButton. Certains boutons utilisent
    des emojis dans Text, d'autres une propriété Icon, avec des hauteurs
    de 24, 28, 32 ou 46.

-   **Concerné :** Trois écrans, boutons d'action, navigation et
    documents

-   **Proposition :** Définir trois variantes cohérentes : action
    métier, navigation secondaire et action compacte de liste.

-   **Gain attendu :** Hiérarchie visuelle stable et meilleure
    accessibilité.

-   **Risque de régression :** Faible

-   **Effort :** Moyen

-   **Test recommandé :** Contraste, focus clavier, états
    désactivé/chargement et longueur maximale des libellés.
    ,
### Remarque ignorée
Cela nécésitte une implémentation générale qui ne peut être faite à ce stade du projet    
    

## 25. Corriger les libellés et coquilles visibles

-   **Catégorie :** Polish visuel ou ergonomique

-   **Priorité :** Moyenne

-   **Constat :** Plusieurs textes visibles contiennent des erreurs :
    \"Veuilliez sélectionner un type de demande pour effectuer lance un
    processus\", \"Fin d\'appel au candidature\", \"a été été
    transmise\", \"délieé\", \"approbabtion\", \"permetant\".

-   **Concerné :** \_txtMessage, lblSelectTypeForAction_M,
    crdDateCandidFinAppel_F.DisplayName, notifications et courriels

-   **Proposition :** Faire une passe linguistique globale sur les
    textes utilisateurs, séparément des commentaires techniques.

-   **Gain attendu :** Crédibilité et compréhension accrues.

-   **Risque de régression :** Très faible

-   **Effort :** Faible

-   **Test recommandé :** Inventaire visuel des messages, notifications
    et courriels des principaux parcours.
### Modification
Corrections sur :
- lblSelectTypeForAction_M.Text
- crdDateCandidFinAppel_F.DisplayName
- btnSendDACSave_M.OnSelect : texte du courriel
- cmpActBtnSend_F.OnAvailable : texte du courriel
- _txtMessage.DocUnLinkedToDemandeSuccess
- txtValidationMsg_F.Text : libellé « approbation »
- txtValidationRefus_F.Text : commentaire technique, facultatif
- txtValidationAppro_F.Text : commentaire technique, facultatif
    

## 26. Compléter les informations d'accessibilité des contrôles interactifs

-   **Catégorie :** Polish visuel ou ergonomique

-   **Priorité :** Moyenne

-   **Constat :** Outre les boutons d'action, certains champs de debug
    ou boutons icône ont un AccessibleLabel vide. Les images ouvrant un
    document n'exposent pas de libellé explicite dans l'export.

-   **Concerné :** Contrôles ModernButton, Image, ModernCombobox,
    actions documentaires

-   **Proposition :** Fournir un libellé accessible décrivant l'action
    et le document ciblé. Exclure les contrôles purement décoratifs de
    l'ordre de tabulation si la propriété est disponible.

-   **Gain attendu :** Navigation clavier et lecteur d'écran plus
    fiable.

-   **Risque de régression :** Très faible

-   **Effort :** Moyen

-   **Test recommandé :** Parcours complet sans souris sur ajout,
    liaison, consultation et suppression de document.
    
### Remarque ignorée
Cela nécésitte une implémentation générale qui ne peut être faite à ce stade du projet    
    

## 27. Retirer ou isoler les contrôles obsolètes et de diagnostic

-   **Catégorie :** Polish visuel ou ergonomique

-   **Priorité :** Faible

-   **Constat :** ctnProcessCODIDecision_M_OBSOLETE reste entièrement
    présent, Text5 affiche le nombre d'éléments sélectionnés sans
    propriété Visible, et plusieurs contrôles techniques sont cachés
    conditionnellement par vvDebug.

-   **Concerné :** ctnProcessCODIDecision_M_OBSOLETE, Text5, conteneurs
    et contrôles debug

-   **Proposition :** Supprimer après validation les éléments sans
    usage, ou les déplacer vers un écran diagnostic dédié.

-   **Gain attendu :** Arbre de contrôles plus lisible et réduction du
    risque de référence accidentelle.

-   **Risque de régression :** Moyen pour le bloc CODI, faible pour les
    contrôles purement diagnostiques

-   **Effort :** Faible

-   **Test recommandé :** Recherche de toutes les références aux
    contrôles avant suppression, puis test du parcours CODI actif.
### Modification
Corrections sur :
- ctnProcessCODIDecision_M_OBSOLETE : conservé, mais considéré comme deprecated. Son Visible reste volontairement neutralisé par && false.
- Référence à ctnProcessCODIDecision_M_OBSOLETE.Visible : supprimée de la formule Visible qui la contient, puisqu'elle ne peut jamais être vraie.
- vvDebug et les contrôles associés : aucune modification. Le mécanisme de diagnostic est conservé tel quel.
- Text5 : supprimé, car il s'agissait d'un contrôle résiduel de test.

# Points à vérifier avant refactorisation

## 28. Vérifier l'absence d'actions pour les types autres que DV et FF

-   **Catégorie :** Point à vérifier

-   **Priorité :** Haute

-   **Constat :** \_TypeDemandeConfigTable définit des Actions pour
    DemandeDV et DemandeFF, mais pas pour FinFF, DemandeAI, FinAI,
    DemandeMission et FinMission. Les actions de liste filtrent les
    types via vcCurrentProcess in Actions.

-   **Concerné :** \_TypeDemandeConfigTable, boutons de processus de
    Screen_Main_M

-   **Proposition :** Confirmer si ces types sont volontairement limités
    à la création/consultation ou si leur configuration est incomplète.

-   **Gain attendu :** Évite de centraliser autour d'un catalogue
    partiellement renseigné.

-   **Risque de régression :** Aucun pour la vérification

-   **Effort :** Faible

-   **Test recommandé :** Créer un cas de chaque type et inventorier les
    actions effectivement visibles à chaque statut.
### Remarque ignorée
Les autres demandes seront implémentées ultérieurement

## 29. Vérifier les statuts terminaux partiellement configurés

-   **Catégorie :** Point à vérifier

-   **Priorité :** Haute

-   **Constat :** PostePourvu et PosteNonPourvu n'exposent pas les mêmes
    propriétés que les autres statuts terminaux : ActionRequired,
    DocumentCategory, Actions, Controls et ButtonText sont absents, et
    StatusDetail_SortOrder vaut Blank().

-   **Concerné :** \_StatutConfigTable, statuts \_StatusKey.PostePourvu
    et \_StatusKey.PosteNonPourvu

-   **Proposition :** Confirmer si les valeurs par défaut consommées par
    les écrans sont suffisantes ou compléter explicitement ces statuts.

-   **Gain attendu :** Comportement terminal déterministe.

-   **Risque de régression :** Aucun pour la vérification

-   **Effort :** Faible

-   **Test recommandé :** Charger une demande dans chacun de ces statuts
    et vérifier formulaires, actions, timeline, tri et consultation.
    
### Remarque ignorée
Pas d'impact métier.
Statuts terminaux nécessitent moins de configuraiotn
    

## 30. Vérifier la cohérence PromSignes entre macro-statut et étape

-   **Catégorie :** Point à vérifier

-   **Priorité :** Moyenne

-   **Constat :** PromSignes a StatutGeneral = \_StatusGeneral.Cloture,
    mais ProcessStepKey = \_ProcessStepKey.ArreteDecision, qui
    appartient à la macro-étape \_ProcessStepMain.Promotion,
    contrairement aux autres clôtures qui utilisent une clé Clot\*.

-   **Concerné :** \_StatutConfigTable, \_ProcessStepConfigTable,
    timeline cmpProcessTimeline_F

-   **Proposition :** Confirmer si une promotion signée doit apparaître
    comme dernière étape normale achevée ou comme clôture terminale
    distincte.

-   **Gain attendu :** Timeline métier correcte.

-   **Risque de régression :** Moyen si la clé est modifiée

-   **Effort :** Faible

-   **Test recommandé :** Comparer l'affichage de PromSignes avec
    PromRefuseeGW et les autres fins de parcours.
### Remarque ignorée
C'est la fin de processus. URS encore un peu floues pour ce besoin
    

## 31. Vérifier les performances et limites de chargement de ccDemandes

-   **Catégorie :** Point à vérifier

-   **Priorité :** Moyenne

-   **Constat :** BtnLoadDemandes_M charge les demandes accessibles dans
    ccDemandes, puis la grille filtre localement. Cette stratégie est
    cohérente, mais son comportement au-delà des limites SharePoint et
    avec un historique volumineux n'est pas démontré par l'export.

-   **Concerné :** BtnLoadDemandes_M.OnSelect, AIRH_Demandes,
    dlstDemandes_M.Items

-   **Proposition :** Mesurer le volume réel, les avertissements de
    délégation et le temps de chargement DFA/DAC.

-   **Gain attendu :** Validation de la capacité avant refactorisation.

-   **Risque de régression :** Aucun pour la mesure

-   **Effort :** Faible

-   **Test recommandé :** Mesures à 500, 2 000 et 5 000 demandes
    actives, avec rôle DFA puis DAC.
    
### Remarque ignorée
Les considérations de performances ont déjà été prise en compte
## Ordre d'exécution conseillé

1.  Suggestions **1 à 9**, corrections locales à faible risque.

2.  Suggestions **10 à 13**, intégrité des données et robustesse
    transactionnelle.

3.  Points de vérification **28 à 31**.

4.  Harmonisation **19 à 21**.

5.  Mutualisation structurante **14 à 18**.

6.  Polish **23 à 27**.
