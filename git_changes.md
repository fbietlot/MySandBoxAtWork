diff --git a/AIRH/CODE/App.Formulas.fx.json b/AIRH/CODE/App.Formulas.fx.json
index 0742eaa..b82c509 100644
--- a/AIRH/CODE/App.Formulas.fx.json
+++ b/AIRH/CODE/App.Formulas.fx.json
@@ -1,4 +1,4 @@
-_AppVersion = "0.12.10";;
+_AppVersion = "0.13.03";;
 _AppName = "Promotion Encadrement";;
 _AppLink = vvAppConfig.AppUrl;;
 
@@ -673,7 +673,8 @@ _StatusKey = {
     CandidAppelEnCours: "CandidAppelEnCours";
     CandidAnalyse:      "CandidAnalyse";
     CandidDFA:          "CandidDFA";
-    CandidPVEnCours:    "CandidPVEnCours";
+    CandidPVEnCours_AvecRecours:    "CandidPVEnCours_AvecRecours";
+    CandidPVEnCours_SansRecours:    "CandidPVEnCours_SansRecours";
     CandidPVTermines:   "CandidPVTermines";
 
     //"Processus de promotion"
@@ -780,10 +781,10 @@ AddColumns(
             StatusKey:          _StatusKey.DemandeNew;
             StatutDetail:       _StatusDetail.DemandeNew;
             StatutGeneral:      _StatusGeneral.Demande;
-            ProcessStepKey:  _ProcessStepKey.InitDemande;
+            ProcessStepKey:     _ProcessStepKey.InitDemande;
             ActionOwner:        _Acteur.DFA;
             ActionRequired:     false;
-            LinkedTypeDemande:  [];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [
                                     _DocumentCategory.DemandeCODI;
                                     _DocumentCategory.Criteres;
@@ -803,10 +804,10 @@ AddColumns(
             StatusKey:          _StatusKey.DemandeBrouillon;
             StatutDetail:       _StatusDetail.DemandeBrouillon;
             StatutGeneral:      _StatusGeneral.Demande;
-            ProcessStepKey:  _ProcessStepKey.InitDemande;
+            ProcessStepKey:     _ProcessStepKey.InitDemande;
             ActionOwner:        _Acteur.DFA;
             ActionRequired:     true;
-            LinkedTypeDemande:  [];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [
                                     _DocumentCategory.DemandeCODI;
                                     _DocumentCategory.Criteres;
@@ -831,10 +832,10 @@ AddColumns(
             StatusKey:          _StatusKey.DemandeBrouillonFin;
             StatutDetail:       _StatusDetail.DemandeBrouillonFin;
             StatutGeneral:      _StatusGeneral.Demande;
-            ProcessStepKey:  _ProcessStepKey.InitDemande;
+            ProcessStepKey:     _ProcessStepKey.InitDemande;
             ActionOwner:        _Acteur.DFA;
             ActionRequired:     true;
-            LinkedTypeDemande:  [];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [
                                     _DocumentCategory.DemandeCODI;
                                     _DocumentCategory.Criteres;
@@ -859,10 +860,10 @@ AddColumns(
             StatusKey:          _StatusKey.DemandeEnCours;
             StatutDetail:       _StatusDetail.DemandeEnCours;
             StatutGeneral:      _StatusGeneral.Demande;
-            ProcessStepKey:  _ProcessStepKey.EnvoiDAC;
+            ProcessStepKey:     _ProcessStepKey.ValidationDAC;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     true;
-            LinkedTypeDemande:  [];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [];
             Actions:            [
                                     _ActionOnForm.DACRefuse;
@@ -888,7 +889,7 @@ AddColumns(
             ProcessStepKey:     _ProcessStepKey.InitDemande;
             ActionOwner:        _Acteur.DFA;
             ActionRequired:     true;
-            LinkedTypeDemande:  [];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [
                                     _DocumentCategory.DemandeCODI;
                                     _DocumentCategory.Criteres;
@@ -915,7 +916,7 @@ AddColumns(
             StatusKey:          _StatusKey.DACValidee_WithLot;
             StatutDetail:       _StatusDetail.DACValidee;
             StatutGeneral:      _StatusGeneral.DACValidation;
-            ProcessStepKey:     _ProcessStepKey.ValidationDAC;
+            ProcessStepKey:     _ProcessStepKey.ConstitutionLot;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     false;
             LinkedTypeDemande:  [   
@@ -933,7 +934,7 @@ AddColumns(
             StatusKey:          _StatusKey.DACValidee_WithAppel;
             StatutDetail:       _StatusDetail.DACValidee;
             StatutGeneral:      _StatusGeneral.DACValidation;
-            ProcessStepKey:     _ProcessStepKey.ValidationDAC;
+            ProcessStepKey:     _ProcessStepKey.CandAppel;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     true;
             LinkedTypeDemande:  [
@@ -951,8 +952,7 @@ AddColumns(
             StatusKey:          _StatusKey.DACDVEnPrevision;
             StatutDetail:       _StatusDetail.DACDVEnPrevision;
             StatutGeneral:      _StatusGeneral.DACValidation;
-            //ProcessStep:        _ProcessStepDetail.ConstitutionLot;
-            ProcessStepKey:  _ProcessStepKey.ConstitutionLot;
+            ProcessStepKey:     _ProcessStepKey.EnvoiNoteGW;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     false;
             LinkedTypeDemande:  [
@@ -972,8 +972,7 @@ AddColumns(
             StatusKey:          _StatusKey.DirReaffConsult;
             StatutDetail:       _StatusDetail.DirReaffConsult;
             StatutGeneral:      _StatusGeneral.DirReaff;
-            //ProcessStep:        _ProcessStepDetail.ConstitutionLot;
-            ProcessStepKey:  _ProcessStepKey.ConstitutionLot;
+            ProcessStepKey:     _ProcessStepKey.DirReaffReception;
             ActionOwner:        _Acteur.DFA;
             ActionRequired:     false;
             LinkedTypeDemande:  [
@@ -984,7 +983,7 @@ AddColumns(
             Controls:   {
                             FormGeneralInfo :               { Visible : true ; Mode : FormMode.View };
                             FormNextStep :                  { Visible : true ; Mode : FormMode.View };
-                            crdDirReaffFinConsultation_F :  { Visible : true ; Mode : FormMode.View }
+                            crdDirReaffFinConsultation_F :  { Visible : true ; Mode : DisplayMode.View }
             };
             StatusDetail_SortOrder: 10
         };
@@ -993,8 +992,7 @@ AddColumns(
             StatusKey:          _StatusKey.DirReaffFinConsult;
             StatutDetail:       _StatusDetail.DirReaffFinConsult;
             StatutGeneral:      _StatusGeneral.DirReaff;
-            //ProcessStep:        _ProcessStepDetail.ConstitutionLot;
-            ProcessStepKey:  _ProcessStepKey.ConstitutionLot;
+            ProcessStepKey:     _ProcessStepKey.DirReaffReception;
             ActionOwner:        _Acteur.DFA;
             ActionRequired:     true;
             LinkedTypeDemande:  [
@@ -1005,7 +1003,7 @@ AddColumns(
             Controls:   {
                             FormGeneralInfo :               { Visible : true ; Mode : FormMode.View };
                             FormNextStep :                  { Visible : true ; Mode : FormMode.View };
-                            crdDirReaffFinConsultation_F :  { Visible : true ; Mode : FormMode.View }
+                            crdDirReaffFinConsultation_F :  { Visible : true ; Mode : DisplayMode.View }
             };
             StatusDetail_SortOrder: 15
         };
@@ -1013,8 +1011,7 @@ AddColumns(
             StatusKey:          _StatusKey.DirReaffPasCandid;
             StatutDetail:       _StatusDetail.DirReaffPasCandid;
             StatutGeneral:      _StatusGeneral.DirReaff;
-            //ProcessStep:        _ProcessStepDetail.DirReaffReception;
-            ProcessStepKey:  _ProcessStepKey.DirReaffReception;
+            ProcessStepKey:     _ProcessStepKey.EnvoiNoteGW;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     false;
             LinkedTypeDemande:  [
@@ -1025,7 +1022,7 @@ AddColumns(
             Controls:   {
                             FormGeneralInfo :               { Visible : true ; Mode : FormMode.View };
                             FormNextStep :                  { Visible : true ; Mode : FormMode.View };
-                            crdDirReaffFinConsultation_F :  { Visible : true ; Mode : FormMode.View }
+                            crdDirReaffFinConsultation_F :  { Visible : true ; Mode : DisplayMode.View }
             };
             StatusDetail_SortOrder: 20
         };
@@ -1033,10 +1030,9 @@ AddColumns(
             StatusKey:          _StatusKey.DirReaffAuditCODI;
             StatutDetail:       _StatusDetail.DirReaffAuditCODI;
             StatutGeneral:      _StatusGeneral.DirReaff;
-            //ProcessStep:        _ProcessStepDetail.DirReaffReception;
-            ProcessStepKey:  _ProcessStepKey.DirReaffReception;
+            ProcessStepKey:     _ProcessStepKey.DirReaffAuditCODI;
             ActionOwner:        _Acteur.DFA;
-            ActionRequired:     false;
+            ActionRequired:     true;
             LinkedTypeDemande:  [
                                     _TypeDemande.DemandeDV
                                 ];
@@ -1050,9 +1046,10 @@ AddColumns(
                                     _ActionOnForm.DirCandNonRet
                                 ];
             Controls:   {
-                            FormGeneralInfo :               { Visible : true ; Mode : FormMode.View };
-                            FormNextStep :                  { Visible : true ; Mode : FormMode.View };
-                            crdDirReaffFinConsultation_F :  { Visible : true ; Mode : FormMode.View }
+                            FormGeneralInfo :               {Visible : true; Mode: FormMode.View};
+                            FormNextStep :                  {Visible : true; Mode: FormMode.Edit};
+                            crdDirReaffFinConsultation_F:   {Visible : true; Mode: DisplayMode.View};
+                            crdDirReaffCandidatRetenu_F:    {Visible : true; Mode: DisplayMode.Edit}
             };
             ButtonText: {
                 Edit: "Résultat audition"
@@ -1063,8 +1060,7 @@ AddColumns(
             StatusKey:          _StatusKey.DirReaffPasReaff;
             StatutDetail:       _StatusDetail.DirReaffPasReaff;
             StatutGeneral:      _StatusGeneral.DirReaff;
-            //ProcessStep:        _ProcessStepDetail.DirReaffAuditCODI;
-            ProcessStepKey:  _ProcessStepKey.DirReaffAuditCODI;
+            ProcessStepKey:     _ProcessStepKey.EnvoiNoteGW;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     false;
             LinkedTypeDemande:  [
@@ -1075,7 +1071,7 @@ AddColumns(
             Controls:   {
                             FormGeneralInfo :               { Visible : true ; Mode : FormMode.View };
                             FormNextStep :                  { Visible : true ; Mode : FormMode.View };
-                            crdDirReaffFinConsultation_F :  { Visible : true ; Mode : FormMode.View }
+                            crdDirReaffFinConsultation_F :  { Visible : true ; Mode : DisplayMode.View }
             };
             StatusDetail_SortOrder: 40
         };
@@ -1085,8 +1081,7 @@ AddColumns(
             StatusKey:          _StatusKey.NGWCOSTRADemande;
             StatutDetail:       _StatusDetail.NGWCOSTRADemande;
             StatutGeneral:      _StatusGeneral.NGWProcessusNGW_DV;
-            //ProcessStep:        _ProcessStepDetail.EnvoiNoteGW;
-            ProcessStepKey:  _ProcessStepKey.EnvoiNoteGW;
+            ProcessStepKey:     _ProcessStepKey.NoteDecisionCOSTRA;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     false;
             LinkedTypeDemande:  [
@@ -1112,8 +1107,7 @@ AddColumns(
             StatusKey:          _StatusKey.NGWAuGW;
             StatutDetail:       _StatusDetail.NGWAuGW;
             StatutGeneral:      _StatusGeneral.NGWProcessusNGW_DV;
-            //ProcessStep:        _ProcessStepDetail.NoteDecisionCOSTRA;
-            ProcessStepKey:  _ProcessStepKey.NoteDecisionCOSTRA;
+            ProcessStepKey:     _ProcessStepKey.NoteDecisionGW;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     false;
             LinkedTypeDemande:  [
@@ -1136,8 +1130,7 @@ AddColumns(
             StatusKey:          _StatusKey.NGWGWDVValidee;
             StatutDetail:       _StatusDetail.NGWGWDVValidee;
             StatutGeneral:      _StatusGeneral.NGWProcessusNGW_DV;
-            //ProcessStep:        _ProcessStepDetail.NoteDecisionGW;
-            ProcessStepKey:  _ProcessStepKey.NoteDecisionGW;
+            ProcessStepKey:     _ProcessStepKey.CandAppel;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     true;
             LinkedTypeDemande:  [
@@ -1159,11 +1152,10 @@ AddColumns(
             StatusKey:          _StatusKey.CandidAppelEnCours;
             StatutDetail:       _StatusDetail.CandidAppelEnCours;
             StatutGeneral:      _StatusGeneral.Candidatures;
-            //ProcessStep:        _ProcessStepDetail.CandAppel;
-            ProcessStepKey:  _ProcessStepKey.CandAppel;
+            ProcessStepKey:     _ProcessStepKey.CandAppel;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     false;
-            LinkedTypeDemande:  [];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [];
             Actions:            [
                                     // Le changement de statut est automatique via Power Automate
@@ -1182,13 +1174,10 @@ AddColumns(
             StatusKey:          _StatusKey.CandidAnalyse;
             StatutDetail:       _StatusDetail.CandidAnalyse;
             StatutGeneral:      _StatusGeneral.Candidatures;
-            //ProcessStep:        _ProcessStepDetail.CandAnalyse;
-            ProcessStepKey:  _ProcessStepKey.CandAnalyse;
+            ProcessStepKey:     _ProcessStepKey.CandAnalyse;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     true;
-            LinkedTypeDemande:  [
-                                    _TypeDemande.DemandeDV
-                                ];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [];
             Actions:            [
                                     _ActionOnList.CandSendDFA
@@ -1204,13 +1193,10 @@ AddColumns(
             StatusKey:          _StatusKey.CandidDFA;
             StatutDetail:       _StatusDetail.CandidDFA;
             StatutGeneral:      _StatusGeneral.Candidatures;
-            //ProcessStep:        _ProcessStepDetail.CandEnvoiDFA;
-            ProcessStepKey:  _ProcessStepKey.CandEnvoiDFA;
+            ProcessStepKey:     _ProcessStepKey.CandFixAuditions;
             ActionOwner:        _Acteur.DFA;
             ActionRequired:     true;
-            LinkedTypeDemande:  [
-                                    _TypeDemande.DemandeDV
-                                ];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [];
             Actions:            [
                                     _ActionOnList.CandAudit
@@ -1223,16 +1209,15 @@ AddColumns(
             StatusDetail_SortOrder: 30
         };
         {
-            StatusKey:          _StatusKey.CandidPVEnCours;
+            StatusKey:          _StatusKey.CandidPVEnCours_AvecRecours;
             StatutDetail:       _StatusDetail.CandidPVEnCours;
             StatutGeneral:      _StatusGeneral.Candidatures;
-            //ProcessStep:        _ProcessStepDetail.CandFixAuditions;
-            ProcessStepKey:  _ProcessStepKey.CandFixAuditions;
+            ProcessStepKey:     _ProcessStepKey.CandDecisionCODI;
             ActionOwner:        _Acteur.DFA;
             ActionRequired:     true;
             LinkedTypeDemande:  [
                                     _TypeDemande.DemandeDV
-                                ];
+            ]; 
             DocumentCategory:   [
                                     _DocumentCategory.PVDefinitif;
                                     _DocumentCategory.PVProvisoire
@@ -1240,7 +1225,39 @@ AddColumns(
             Actions:            [
                                     _ActionOnList.Modifier;
                                     _ActionOnForm.Save;
-                                    _ActionOnList.CODIDecision;
+                                    _ActionOnForm.AddDoc;
+                                    //_ActionOnList.CODIDecision;
+                                    _ActionOnForm.CODICandSelect
+                                ];
+            Controls:   {
+                            FormGeneralInfo :           { Visible: true ; Mode: FormMode.View };
+                            FormNextStep :              { Visible: true ; Mode: FormMode.Edit };
+                            crdDateCandAudition_F:      { Visible: true ; Mode: DisplayMode.Edit };
+                            crdCODICandRetenu_F:        { Visible: true ; Mode: DisplayMode.Edit }
+            };
+            ButtonText: {
+                Edit: "Décision CODI"
+            };
+            StatusDetail_SortOrder: 40
+        };
+        {
+            StatusKey:          _StatusKey.CandidPVEnCours_SansRecours;
+            StatutDetail:       _StatusDetail.CandidPVEnCours;
+            StatutGeneral:      _StatusGeneral.Candidatures;
+            ProcessStepKey:     _ProcessStepKey.CandDecisionCODI;
+            ActionOwner:        _Acteur.DFA;
+            ActionRequired:     true;
+            LinkedTypeDemande:  [
+                                _TypeDemande.DemandeFF
+            ]; 
+            DocumentCategory:   [
+                                    _DocumentCategory.PVDefinitif;
+                                ];
+            Actions:            [
+                                    _ActionOnList.Modifier;
+                                    _ActionOnForm.Save;
+                                    _ActionOnForm.AddDoc;
+                                    //_ActionOnList.CODIDecision;
                                     _ActionOnForm.CODICandSelect
                                 ];
             Controls:   {
@@ -1249,19 +1266,19 @@ AddColumns(
                             crdDateCandAudition_F:      { Visible: true ; Mode: DisplayMode.Edit };
                             crdCODICandRetenu_F:        { Visible: true ; Mode: DisplayMode.Edit }
             };
+            ButtonText: {
+                Edit: "Décision CODI"
+            };
             StatusDetail_SortOrder: 40
         };
         {
             StatusKey:          _StatusKey.CandidPVTermines;
             StatutDetail:       _StatusDetail.CandidPVTermines;
             StatutGeneral:      _StatusGeneral.Candidatures;
-            //ProcessStep:        _ProcessStepDetail.CandDecisionCODI;
-            ProcessStepKey:  _ProcessStepKey.CandDecisionCODI;
+            ProcessStepKey:     _ProcessStepKey.ArreteRedaction;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     true;
-            LinkedTypeDemande:  [
-                                    _TypeDemande.DemandeDV
-                                ];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [];
             Actions:            [
                                     _ActionOnList.PromRedaction
@@ -1280,12 +1297,10 @@ AddColumns(
             StatusKey:          _StatusKey.PromRedaction;
             StatutDetail:       _StatusDetail.PromRedaction;
             StatutGeneral:      _StatusGeneral.Promotion;
-            ProcessStepKey:  _ProcessStepKey.ArreteRedaction;
+            ProcessStepKey:     _ProcessStepKey.ArreteEnvoiGWDG;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     false;
-            LinkedTypeDemande:  [
-                                    _TypeDemande.DemandeDV
-                                ];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [];
             Actions:            [
                                     _ActionOnList.PromEnvoi
@@ -1301,12 +1316,10 @@ AddColumns(
             StatusKey:          _StatusKey.PromAuGWDG;
             StatutDetail:       _StatusDetail.PromAuGWDG;
             StatutGeneral:      _StatusGeneral.Promotion;
-            ProcessStepKey:     _ProcessStepKey.ArreteEnvoiGWDG;
+            ProcessStepKey:     _ProcessStepKey.ArreteDecision;
             ActionOwner:        _Acteur.DAC;
             ActionRequired:     false;
-            LinkedTypeDemande:  [
-                                    _TypeDemande.DemandeDV
-                                ];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [
                                     _DocumentCategory.ArreteDesign;
                                     _DocumentCategory.ArreteRecept;
@@ -1333,13 +1346,10 @@ AddColumns(
             StatusKey:          _StatusKey.PromSignes;
             StatutDetail:       _StatusDetail.PromSignes;
             StatutGeneral:      _StatusGeneral.Cloture;
-            //ProcessStep:        _ProcessStepDetail.ArreteDecision;
             ProcessStepKey:  _ProcessStepKey.ArreteDecision;
             ActionOwner:        _Acteur.DFA;
             ActionRequired:     true;
-            LinkedTypeDemande:  [
-                                    _TypeDemande.DemandeDV
-                                ];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             Controls:   {
                             FormGeneralInfo :   { Visible: true ; Mode: FormMode.View };
                             FormNextStep :      { Visible: true ; Mode: FormMode.View }
@@ -1352,7 +1362,6 @@ AddColumns(
             StatusKey:          _StatusKey.DirReaffCODI;
             StatutDetail:       _StatusDetail.DirReaffCODI;
             StatutGeneral:      _StatusGeneral.Cloture;
-            //ProcessStep:        _ProcessStepDetail.ClotDirReaffCODI;
             ProcessStepKey:  _ProcessStepKey.ClotDirReaffCODI;
             ActionOwner:        Blank();
             ActionRequired:     false;
@@ -1364,8 +1373,9 @@ AddColumns(
                                     _ActionOnList.Consulter
                                 ];
             Controls:   {
-                            FormGeneralInfo :   { Visible : true ; Mode : FormMode.View };
-                            FormNextStep :      { Visible : true ; Mode : FormMode.View }
+                            FormGeneralInfo:            {Visible: true; Mode: FormMode.View};
+                            FormNextStep:               {Visible: true; Mode: FormMode.View};
+                            crdDirReaffCandidatRetenu_F:{Visible: true; Mode: DisplayMode.Edit}
             };
             ButtonText: {
                 Edit: "N/A";
@@ -1377,8 +1387,7 @@ AddColumns(
             StatusKey:          _StatusKey.COSTRARefusee;
             StatutDetail:       _StatusDetail.COSTRARefusee;
             StatutGeneral:      _StatusGeneral.Cloture;
-            //ProcessStep:        _ProcessStepDetail.Cloture;
-            ProcessStepKey:  _ProcessStepKey.ClotCostraRefusee;
+            ProcessStepKey:     _ProcessStepKey.ClotCostraRefusee;
             ActionOwner:        Blank();
             ActionRequired:     false;
             LinkedTypeDemande:  [
@@ -1403,8 +1412,7 @@ AddColumns(
             StatusKey:          _StatusKey.GWRefusee;
             StatutDetail:       _StatusDetail.GWRefusee;
             StatutGeneral:      _StatusGeneral.Cloture;
-            //ProcessStep:        _ProcessStepDetail.Cloture;
-            ProcessStepKey:  _ProcessStepKey.ClotGWRefusee;
+            ProcessStepKey:     _ProcessStepKey.ClotGWRefusee;
             ActionOwner:        Blank();
             ActionRequired:     false;
             LinkedTypeDemande:  [
@@ -1429,24 +1437,18 @@ AddColumns(
             StatusKey:          _StatusKey.PostePourvu;
             StatutDetail:       _StatusDetail.PostePourvu;
             StatutGeneral:      _StatusGeneral.Cloture;
-            //ProcessStep:        _ProcessStepDetail.Cloture;
-            ProcessStepKey:  _ProcessStepKey.ClotPostePourvu;
+            ProcessStepKey:     _ProcessStepKey.ClotPostePourvu;
             ActionOwner:        Blank();
-            LinkedTypeDemande:  [
-                                    _TypeDemande.DemandeDV
-                                ];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             StatusDetail_SortOrder: Blank()
         };
         {
             StatusKey:          _StatusKey.PosteNonPourvu;
             StatutDetail:       _StatusDetail.PosteNonPourvu;
             StatutGeneral:      _StatusGeneral.Cloture;
-            //ProcessStep:        _ProcessStepDetail.Cloture;
             ProcessStepKey:  _ProcessStepKey.ClotPosteNonPourvu;
             ActionOwner:        Blank();
-            LinkedTypeDemande:  [
-                                    _TypeDemande.DemandeDV
-                                ];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             StatusDetail_SortOrder: Blank()
         };
         {
@@ -1456,9 +1458,7 @@ AddColumns(
             //ProcessStep:        _ProcessStepDetail.Cloture;
             ProcessStepKey:  _ProcessStepKey.ClotPromRefuseeGW;
             ActionOwner:        Blank();
-            LinkedTypeDemande:  [
-                                    _TypeDemande.DemandeDV
-                                ];
+            LinkedTypeDemande:  []; //Vide --> Valide pour tous les type de demandes
             DocumentCategory:   [];
             Actions:            [
                                     _ActionOnList.Consulter
@@ -1680,7 +1680,7 @@ _tblDemandesFieldCatalog = [
 
 
 //Purpose: Indicates whether multiple 'Deamandes' are currently selected in the main table to drive UI behavior and available bulk actions.
-_MuliSelectOnDemandes = CountRows(dlstDemandes_M.SelectedItems) > 1 ;;
+_MultiSelectOnDemandes = CountRows(dlstDemandes_M.SelectedItems) > 1 ;;
 _SingleSelectOnDemandes = CountRows(dlstDemandes_M.SelectedItems) = 1 ;;
 _NoSelectOnDemandes = CountRows(dlstDemandes_M.SelectedItems) = 0 ;;
 
@@ -1726,9 +1726,9 @@ _txtMessage = {
     NoRecipient : "Aucune liste de diffusion DFA n'est configurée pour ce SPW. Le changement est enregistré mais aucune notification n'a pu être envoyée.";
     EmailSubjectPrefix : If(vvAppConfig.ProdEnvironnement; ""; "[TEST] [" & _AppName & "] ");
     EmailFooter : 
-        "<br><br>👉 <a href='" & _AppLink & "'>" &
-        "Accéder à " & _AppName & " pour consulter la demande</a><br><br>" &
-        "Ceci est une notification automatique."
+        "<p>👉 <a href='" & _AppLink & "'>" &
+        "Accéder à " & _AppName & " pour consulter la demande</a></p>" &
+        "<p><i>Ceci est une notification automatique.</i></p>"
 };;
 
 _Emoji = {
diff --git a/AIRH/CODE/Screen_Document_D.fx.json b/AIRH/CODE/Screen_Document_D.fx.json
index 479cddb..bddd544 100644
--- a/AIRH/CODE/Screen_Document_D.fx.json
+++ b/AIRH/CODE/Screen_Document_D.fx.json
@@ -122,11 +122,25 @@ Screens:
                                 });
 
                                 With(
-                                    {MyAttachment : First(Self.Attachments)}, // Remark : The functionality will only allow to add one file
+                                    {
+                                        MyAttachment : First(Self.Attachments),
+                                        MyFileNameWithTimeStamp:
+                                            With(
+                                                {
+                                                    MyExtension: Last(Split(First(Self.Attachments).Name, ".")).Value,
+                                                    MyTimestamp: Text(Now(),"yyyymmddhhmmss")
+                                                },
+                                                Substitute(
+                                                    First(Self.Attachments).Name,
+                                                    "." & MyExtension,
+                                                    "_" & MyTimestamp & "." & MyExtension
+                                                )
+                                            )        
+                                    }, // Remark : The functionality will only allow to add one file
                                     UpdateContext({
                                         vcFileUploaded : 
                                             AIRH_Upload_Files_ToSP.Run(
-                                                MyAttachment.Name, 
+                                                MyFileNameWithTimeStamp, 
                                                 vcCategorieDocument,
                                                 If(!vcBatchMode, 
                                                     // Link one 'Document' to  single 'Demande'
@@ -134,12 +148,15 @@ Screens:
                                                     //Else Batch mode - link one 'Document' to  to multiple 'Demandes'
                                                     JSON(RenameColumns( vcTblIdDemande,ID,DemandeId))
                                                 ),
+                                                If(vvUserRole.Key = _Acteur.DAC.Key, ""  , vvUserSPW),
+                                                vcDemande.'Type de demande'.Value,
                                                 {                  
                                                     file: {
                                                         contentBytes:   MyAttachment.Value,
-                                                        name:           MyAttachment.Name
+                                                        name:           MyFileNameWithTimeStamp
                                                     }
                                                 }
+
                                             )
                                     })
                                 );
@@ -244,7 +261,24 @@ Screens:
                             Properties:
                               Default: =Blank()
                               Height: =Parent.Height
-                              Items: =SortByColumns( Filter( AIRH_Documents,CategorieDocument.Value = vcCategorieDocument), "Modified",SortOrder.Descending)
+                              Items: |-
+                                =If(vvUserRole.Key = _Acteur.DAC.Key,
+                                    SortByColumns(
+                                        Filter(
+                                            AIRH_Documents,
+                                            CategorieDocument.Value = vcCategorieDocument
+                                            ),
+                                        "Modified",SortOrder.Descending
+                                    ),
+                                    SortByColumns(
+                                        Filter(
+                                            AIRH_Documents,
+                                            CategorieDocument.Value = vcCategorieDocument,
+                                            SPW = vvUserSPW        
+                                            ),
+                                        "Modified",SortOrder.Descending
+                                    )
+                                )
                               OnSelect: |-
                                 =UpdateContext({vcSelectedDoc: ThisItem})
                               TemplateFill: |
@@ -288,7 +322,7 @@ Screens:
                                     PaddingRight: =0
                                     PaddingTop: =0
                                     Size: =10
-                                    Text: =ThisItem.'Modifié par'.DisplayName
+                                    Text: =ThisItem.'Créé par'.DisplayName
                                     VerticalAlign: =VerticalAlign.Top
                                     Width: =lblFileName_D.Width
                                     X: =lblFileName_D.X
@@ -306,7 +340,7 @@ Screens:
                                     PaddingRight: =0
                                     PaddingTop: =0
                                     Size: =10
-                                    Text: =ThisItem.Modifié
+                                    Text: =ThisItem.Créé
                                     VerticalAlign: =VerticalAlign.Top
                                     Width: =lblFileName_D.Width
                                     X: =lblModifBy_D.X
@@ -331,7 +365,21 @@ Screens:
                                     PaddingRight: =0
                                     PaddingTop: =0
                                     Size: =10
-                                    Text: =ThisItem.'Nom de fichier avec l’extension'
+                                    Text: |-
+                                      =With(
+                                          {
+                                              m: Match(
+                                                  ThisItem.'Nom de fichier avec l’extension',
+                                                  "^(.*)_\d{14}(\.[^.]+)$"
+                                              )
+                                          },
+                                          If(
+                                              IsBlank(m.FullMatch),
+                                              ThisItem.'Nom de fichier avec l’extension',
+                                              First(m.SubMatches).Value &
+                                              Last(m.SubMatches).Value
+                                          )
+                                      )
                                     VerticalAlign: =VerticalAlign.Top
                                     Width: =Parent.TemplateWidth - Self.X - 10
                                     X: =72
diff --git a/AIRH/CODE/Screen_Form_F.fx.json b/AIRH/CODE/Screen_Form_F.fx.json
index 3bd106a..ea4984d 100644
--- a/AIRH/CODE/Screen_Form_F.fx.json
+++ b/AIRH/CODE/Screen_Form_F.fx.json
@@ -54,7 +54,7 @@ Screens:
         );
 
         //#3  SET UP SubwayNAV menu
-        Reset(cbxSubNavRang_F);Reset(cbxSubNavStep_F);Reset(numSubNavMainStep_F);Reset(numSubNavStep_F);
+        Reset(cbxSubNavRang_F);Reset(cbxSubNavStep_F);Reset(numSubNavMainStep_F_V2);Reset(numSubNavStep_F_V2);
 
 
 
@@ -112,25 +112,11 @@ Screens:
                       Properties:
                         DefaultSelectedItems: =vcStatutConfig
                         ItemDisplayText: '="(" & ThisItem.StatutGeneral & ")  " & ThisItem.StatutDetail '
-                        Items: =_StatutConfigTable
+                        Items: =Filter(_StatutConfigTable,vcDemande.'Type de demande'.Value in ThisRecord.LinkedTypeDemande Or IsEmpty(ThisRecord.LinkedTypeDemande)) // _StatutConfigTable
                         SelectMultiple: =false
                         Size: =8
                         X: =422
                         Y: =40
-                  - numSubNavStep_F:
-                      Control: ModernNumberInput@1.1.2
-                      Properties:
-                        Default: =cbxSubNavStep_F.Selected.ProcessStep.StepOrder
-                        Width: =91
-                        X: =115
-                        Y: =40
-                  - numSubNavMainStep_F:
-                      Control: ModernNumberInput@1.1.2
-                      Properties:
-                        Default: =Value( Left(cbxSubNavStep_F.Selected.ProcessStep.StepOrder,2))
-                        Width: =91
-                        X: =57
-                        Y: =40
                   - cbxSubNavRang_F:
                       Control: ModernCombobox@1.1.2
                       Properties:
@@ -201,7 +187,7 @@ Screens:
                                     }
                                 }
                               Fill: =
-                              ProcessSteps: "= With( {\n    // Regroupe les étapes détaillées par étape principale. \n    MyMainSteps :\n        AddColumns( \n            GroupBy(\n                _ProcessStepConfigTable,\n                MainStep,\n                ProcessSteps\n            ),\n            MainStepOrder, \n                First(ThisRecord.ProcessSteps).MainStepOrder       \n        ),\n    // Position actuelle de la demande dans le processus.\n    MyDemandeMainStepOrder : \n            numSubNavMainStep_F_V2.Value,\n    MyDemandeStepOrder : \n            numSubNavStep_F_V2.Value,\n\n    // Rang utilisé pour déterminer si l'étape de réaffectation doit être affichée comme ignorée.\n    MyDemandeRang : \n            cbxSubNavRang_F.Selected.Value\n    },\n\n    Ungroup(\n        ForAll(MyMainSteps  As MyMainStep,\n            Table(\n                {\n                    ProcessStepOrder:   MyMainStep.MainStepOrder,\n                    ProcessStepText:    MyMainStep.MainStep,\n                    ProcessStepState : \n                              Switch(\n                                true,\n\n                                // L'étape de réaffectation ne concerne\n                                // que les demandes de rang A4.\n                                MyMainStep.MainStep = _ProcessStepMain.DirReaff\n                                    && (!IsBlank(MyDemandeRang) && MyDemandeRang <> \"A4\"),\n                                    cmpProcessTimeline_F.ProcessStepStates.Skipped,\n\n                                MyMainStep.MainStepOrder = MyDemandeMainStepOrder,\n                                    cmpProcessTimeline_F.ProcessStepStates.Current,\n\n                                MyMainStep.MainStepOrder < MyDemandeMainStepOrder,\n                                    cmpProcessTimeline_F.ProcessStepStates.Completed,\n\n                                cmpProcessTimeline_F.ProcessStepStates.NotStarted\n                            )\n\n\n                },\n                ForAll(Filter(_ProcessStepConfigTable,MainStep = MyMainStep.MainStep) As MyStep, // Only Steps of the current Main step\n                    { \n                        ProcessStepOrder:        MyStep.StepOrder , \n                        ProcessStepText:      MyStep.Text,\n                        ProcessStepState : \n                                Switch(\n                                    true,\n\n                                    MyStep.StepOrder = MyDemandeStepOrder,\n                                        cmpProcessTimeline_F.ProcessStepStates.Current,\n\n                                    MyStep.StepOrder < MyDemandeStepOrder,\n                                        cmpProcessTimeline_F.ProcessStepStates.Completed,\n\n                                    cmpProcessTimeline_F.ProcessStepStates.NotStarted\n                                ),\n                        ParentProcessStepOrder : MyMainStep.MainStepOrder\n                        \n                    }\n                )\n            )\n    ),\n    Value\n    )\n)\n\n            "
+                              ProcessSteps: "=With({ \n    MyStatusOfTypeDemande : Filter(_StatutConfigTable,vcDemande.'Type de demande'.Value in ThisRecord.LinkedTypeDemande Or IsEmpty(ThisRecord.LinkedTypeDemande)) ,\n    MyProcessStepOfTypeDemande : Distinct( Filter(_StatutConfigTable,vcDemande.'Type de demande'.Value in ThisRecord.LinkedTypeDemande Or IsEmpty(ThisRecord.LinkedTypeDemande)),ProcessStepKey),\n    MyProcessStepConfigTable : Filter(_ProcessStepConfigTable,ThisRecord.Key in Distinct( Filter(_StatutConfigTable,vcDemande.'Type de demande'.Value in ThisRecord.LinkedTypeDemande Or IsEmpty(ThisRecord.LinkedTypeDemande)),ProcessStepKey))\n    },\n    With( {\n\n    // Regroupe les étapes détaillées par étape principale. \n    MyMainSteps :\n        AddColumns( \n            GroupBy(\n                 MyProcessStepConfigTable, //_ProcessStepConfigTable;\n                MainStep,\n                ProcessSteps\n            ),\n            MainStepOrder, \n                First(ThisRecord.ProcessSteps).MainStepOrder       \n        ),\n    // Position actuelle de la demande dans le processus.\n    MyDemandeMainStepOrder : \n            numSubNavMainStep_F_V2.Value,\n    MyDemandeStepOrder : \n            numSubNavStep_F_V2.Value,\n\n    // Rang utilisé pour déterminer si l'étape de réaffectation doit être affichée comme ignorée.\n    MyDemandeRang : \n            cbxSubNavRang_F.Selected.Value\n    },\n\n    Ungroup(\n        ForAll(MyMainSteps  As MyMainStep,\n            Table(\n                {\n                    ProcessStepOrder:   MyMainStep.MainStepOrder,\n                    ProcessStepText:    MyMainStep.MainStep,\n                    ProcessStepState : \n                              Switch(\n                                true,\n\n                                // L'étape de réaffectation ne concerne\n                                // que les demandes de rang A4.\n                                MyMainStep.MainStep = _ProcessStepMain.DirReaff\n                                    && (!IsBlank(MyDemandeRang) && MyDemandeRang <> \"A4\"),\n                                    cmpProcessTimeline_F.ProcessStepStates.Skipped,\n\n                                MyMainStep.MainStepOrder = MyDemandeMainStepOrder,\n                                    cmpProcessTimeline_F.ProcessStepStates.Current,\n\n                                MyMainStep.MainStepOrder < MyDemandeMainStepOrder,\n                                    cmpProcessTimeline_F.ProcessStepStates.Completed,\n\n                                cmpProcessTimeline_F.ProcessStepStates.NotStarted\n                            )\n\n\n                },\n                ForAll(Filter(_ProcessStepConfigTable,MainStep = MyMainStep.MainStep) As MyStep, // Only Steps of the current Main step\n                    { \n                        ProcessStepOrder:     MyStep.StepOrder , \n                        ProcessStepText:      MyStep.Text,\n                        ProcessStepState : \n                                Switch(\n                                    true,\n\n                                    MyStep.StepOrder = MyDemandeStepOrder,\n                                        cmpProcessTimeline_F.ProcessStepStates.Current,\n\n                                    MyStep.StepOrder < MyDemandeStepOrder,\n                                        cmpProcessTimeline_F.ProcessStepStates.Completed,\n\n                                    cmpProcessTimeline_F.ProcessStepStates.NotStarted\n                                ),\n                        ParentProcessStepOrder : MyMainStep.MainStepOrder\n                        \n                    }\n                )\n            )\n    ),\n    Value\n    )\n)\n\n)"
                               ProcessStoppedAtStepOrder: =cbxSubNavStep_F.Selected.ProcessStep.ClosedFromStepOrder
                               Width: =200
                   - galStatusHistory:
@@ -2839,6 +2825,191 @@ Screens:
                                                       Visible: =And(Parent.Required, Parent.DisplayMode=DisplayMode.Edit)
                                                       Width: =30
                                                       Y: =DataCardKey4.Y
+                                          - crdDirReaffCandidatRetenu_F:
+                                              Control: TypedDataCard@1.0.7
+                                              Variant: ComboBoxEdit
+                                              Properties:
+                                                DataField: ="DirReaffCandidatRetenu"
+                                                Default: |-
+                                                  =If (
+                                                      IsBlank(ThisItem.DirReaffCandidatRetenu),
+                                                      Blank(),
+                                                      {
+                                                          DisplayName: ThisItem.DirReaffCandidatRetenu.DisplayName,
+                                                          Mail:       ThisItem.DirReaffCandidatRetenu.Email
+                                                      }
+                                                  )
+                                                DisplayMode: |-
+                                                  =// Mode :
+                                                  // - si le formulaire parent est en consultation (FormMode.View) : cette configuration est ignorée et le champ reste non modifiable.
+                                                  // - modifiable uniquement pour les statuts qui l'autorisent.
+                                                  // - lecture seule pour tous les autres statuts.
+                                                  // - lecture seule par défaut.
+                                                  Coalesce(
+                                                      vcStatutConfig.Controls.crdDirReaffCandidatRetenu_F.Mode,
+                                                      DisplayMode.View
+                                                  )
+                                                DisplayName: ="Candidat reaffecté"
+                                                Height: =crdCODICandRetenu_F.Height
+                                                Required: =false
+                                                Update: |-
+                                                  =If(
+                                                      Self.Visible && !IsBlank(drpDirReaffCandidatRetenu_F.Selected),
+                                                      {
+                                                          Claims: Concatenate(
+                                                              "i:0#.f|membership|",
+                                                              drpDirReaffCandidatRetenu_F.Selected.Mail// Person email
+                                                          ),
+                                                          Department: "",
+                                                          DisplayName: drpDirReaffCandidatRetenu_F.Selected.DisplayName,
+                                                          Email: drpDirReaffCandidatRetenu_F.Selected.Mail, // Person email
+                                                          JobTitle: "",
+                                                          Picture: ""
+                                                      },
+                                                      Blank()
+                                                  )
+                                                Visible: |-
+                                                  =// Visible :
+                                                  //  - dès qu'une valeur existe afin de conserver l'historique du processus.
+                                                  //  - pour les statuts qui déclarent explicitement ce champ comme visible.
+                                                  //  - en debug (et pour faciliter la mise en page du form)
+                                                  Or(
+                                                      !IsBlank(Self.Default),
+                                                      Coalesce(vcStatutConfig.Controls.crdDirReaffCandidatRetenu_F.Visible, false),
+                                                      vvDebug
+                                                  )
+                                                Width: =Parent.Width
+                                                X: =0
+                                                Y: =21
+                                              Children:
+                                                - DataCardKey32_2:
+                                                    Control: ModernText@1.0.0
+                                                    Properties:
+                                                      AutoHeight: =true
+                                                      FontWeight: =FontWeight.Semibold
+                                                      Height: =txtTypeDemandeLabel_F.Height
+                                                      PaddingBottom: =0
+                                                      PaddingLeft: =0
+                                                      PaddingRight: =0
+                                                      PaddingTop: =0
+                                                      RadiusBottomLeft: =0
+                                                      RadiusBottomRight: =0
+                                                      RadiusTopLeft: =0
+                                                      RadiusTopRight: =0
+                                                      Size: =txtTypeDemandeLabel_F.Size
+                                                      Text: =Parent.DisplayName & " :"
+                                                      VerticalAlign: =txtTypeDemandeLabel_F.VerticalAlign
+                                                      Width: =Parent.Width - 10
+                                                      X: =txtTypeDemandeLabel_F.X
+                                                      Y: =txtTypeDemandeLabel_F.Y
+                                                - ErrorMessage32_2:
+                                                    Control: ModernText@1.0.0
+                                                    Properties:
+                                                      Height: =30
+                                                      PaddingBottom: =0
+                                                      PaddingLeft: =0
+                                                      PaddingRight: =0
+                                                      PaddingTop: =0
+                                                      RadiusBottomLeft: =0
+                                                      RadiusBottomRight: =0
+                                                      RadiusTopLeft: =0
+                                                      RadiusTopRight: =0
+                                                      Size: =14
+                                                      Text: =Parent.Error
+                                                      VerticalAlign: =VerticalAlign.Top
+                                                      Visible: =And(!IsBlank(Parent.Error), Parent.DisplayMode=DisplayMode.Edit)
+                                                      Width: =Parent.Width - 48
+                                                      X: =24
+                                                      Y: =drpDirReaffCandidatRetenu_F.Y + drpDirReaffCandidatRetenu_F.Height
+                                                - StarVisible32_2:
+                                                    Control: ModernText@1.0.0
+                                                    Properties:
+                                                      Align: =Align.Center
+                                                      Height: =20
+                                                      PaddingBottom: =0
+                                                      PaddingLeft: =0
+                                                      PaddingRight: =0
+                                                      PaddingTop: =0
+                                                      RadiusBottomLeft: =0
+                                                      RadiusBottomRight: =0
+                                                      RadiusTopLeft: =0
+                                                      RadiusTopRight: =0
+                                                      Size: =14
+                                                      Text: ="*"
+                                                      VerticalAlign: =VerticalAlign.Top
+                                                      Visible: =And(Parent.Required, Parent.DisplayMode=DisplayMode.Edit)
+                                                      Width: =30
+                                                      Y: =DataCardKey32_2.Y
+                                                - drpDirReaffCandidatRetenu_F:
+                                                    Control: ModernCombobox@1.1.2
+                                                    Properties:
+                                                      AccessibleLabel: =Parent.DisplayName
+                                                      Appearance: =Appearance.FilledLighter
+                                                      DefaultSelectedItems: =Parent.Default
+                                                      DelayOutput: =true
+                                                      DisplayMode: =Parent.DisplayMode
+                                                      FontWeight: =""
+                                                      InputTextPlaceholder: |-
+                                                        ="👤Rechercher un agent"
+                                                      ItemDisplayText: =ThisItem.DisplayName & " (" & Text(ThisItem.MailNickname) & ")"
+                                                      Items: |-
+                                                        =With(
+                                                            { MySearch: Self.SearchText },
+                                                            'Utilisateursd’Office365'.SearchUserV2(
+                                                                {
+                                                                    searchTerm: MySearch,
+                                                                    isSearchTermRequired: true,
+                                                                    top: 50
+                                                                }
+                                                            ).value
+                                                        )
+                                                        //'Utilisateursd’Office365'.SearchUserV2({searchTerm: drpDirReaffCandidatRetenu_F.SearchText; isSearchTermRequired: false; top: 50}).value
+                                                      Required: =Parent.Required
+                                                      SelectMultiple: =false
+                                                      Size: =inpNumEmploi_F.Size
+                                                      ValidationState: =If(IsBlank(Parent.Error), "None", "Error")
+                                                      Width: =198
+                                                      X: =24
+                                                      Y: =36
+                                                - DataCardValue21_2:
+                                                    Control: ModernTextInput@1.1.2
+                                                    Properties:
+                                                      AccessibleLabel: =Parent.DisplayName
+                                                      Align: =Align.Right
+                                                      Default: =drpDirReaffCandidatRetenu_F.Selected.MailNickname
+                                                      DisplayMode: =inpCodeES_F.DisplayMode
+                                                      FontWeight: =""
+                                                      Height: '=26  '
+                                                      Italic: =true
+                                                      MaxLength: =-1
+                                                      Placeholder: =""
+                                                      Required: =Parent.Required
+                                                      Size: =inpNumEmploi_F.Size
+                                                      TriggerOutput: =TriggerOutput.FocusOut
+                                                      ValidationState: =If(IsBlank(Parent.Error), "None", "Error")
+                                                      Width: =100
+                                                      X: =122
+                                                      Y: =drpDirReaffCandidatRetenu_F.Y + drpCODICandRetenu_F.Height
+                                                - DataCardKey28_2:
+                                                    Control: ModernText@1.0.0
+                                                    Properties:
+                                                      Height: '=26  '
+                                                      Italic: =true
+                                                      PaddingBottom: =0
+                                                      PaddingLeft: =0
+                                                      PaddingRight: =0
+                                                      PaddingTop: =0
+                                                      RadiusBottomLeft: =0
+                                                      RadiusBottomRight: =0
+                                                      RadiusTopLeft: =0
+                                                      RadiusTopRight: =0
+                                                      Size: =txtTypeDemandeLabel_F.Size
+                                                      Text: ="Numéro Ulis:"
+                                                      VerticalAlign: =txtTypeDemandeLabel_F.VerticalAlign
+                                                      Width: =86
+                                                      Wrap: =false
+                                                      X: =80
+                                                      Y: =drpDirReaffCandidatRetenu_F.Y + drpCODICandRetenu_F.Height
                                           - crdCommentValidCOSTRA_F:
                                               Control: TypedDataCard@1.0.7
                                               Variant: TextualMultilineEdit
@@ -3266,21 +3437,30 @@ Screens:
                                                                   Mail:       ThisItem.CODICandidatRetenu.Email
                                                               }
                                                           )
-                                                DisplayMode: "=// Mode :\r\n// - si le formulaire parent est en consultation (FormMode.View) : cette configuration est ignorée et le champ reste non modifiable.\r\n// - modifiable uniquement pour les statuts qui l'autorisent.\r\n// - lecture seule pour tous les autres statuts.\r\n// - lecture seule par défaut.\r\nCoalesce(\r\n    vcStatutConfig.Controls.crdCODICandRetenu_F.Mode,\r\n    DisplayMode.View\r\n) "
+                                                DisplayMode: |-
+                                                  =// Mode :
+                                                  // - si le formulaire parent est en consultation (FormMode.View) : cette configuration est ignorée et le champ reste non modifiable.
+                                                  // - modifiable uniquement pour les statuts qui l'autorisent.
+                                                  // - lecture seule pour tous les autres statuts.
+                                                  // - lecture seule par défaut.
+                                                  Coalesce(
+                                                      vcStatutConfig.Controls.crdCODICandRetenu_F.Mode,
+                                                      DisplayMode.View
+                                                  )
                                                 DisplayName: ="Candidat Retenu"
-                                                Height: =80
+                                                Height: =100
                                                 Required: =false
                                                 Update: |-
                                                   =      If(
-                                                              Self.Visible && !IsBlank(DataCardValue32_1.Selected),
+                                                              Self.Visible && !IsBlank(drpCODICandRetenu_F.Selected),
                                                               {
                                                                   Claims: Concatenate(
                                                                       "i:0#.f|membership|",
-                                                                      DataCardValue32_1.Selected.Mail// Person email
+                                                                      drpCODICandRetenu_F.Selected.Mail// Person email
                                                                   ),
                                                                   Department: "",
-                                                                  DisplayName: DataCardValue32_1.Selected.DisplayName,
-                                                                  Email: DataCardValue32_1.Selected.Mail, // Person email
+                                                                  DisplayName: drpCODICandRetenu_F.Selected.DisplayName,
+                                                                  Email: drpCODICandRetenu_F.Selected.Mail, // Person email
                                                                   JobTitle: "",
                                                                   Picture: ""
                                                               },
@@ -3338,7 +3518,7 @@ Screens:
                                                       Visible: =And(!IsBlank(Parent.Error), Parent.DisplayMode=DisplayMode.Edit)
                                                       Width: =Parent.Width - 48
                                                       X: =24
-                                                      Y: =DataCardValue32_1.Y + DataCardValue32_1.Height
+                                                      Y: =drpCODICandRetenu_F.Y + drpCODICandRetenu_F.Height
                                                 - StarVisible32_1:
                                                     Control: ModernText@1.0.0
                                                     Properties:
@@ -3358,7 +3538,7 @@ Screens:
                                                       Visible: =And(Parent.Required, Parent.DisplayMode=DisplayMode.Edit)
                                                       Width: =30
                                                       Y: =DataCardKey32_1.Y
-                                                - DataCardValue32_1:
+                                                - drpCODICandRetenu_F:
                                                     Control: ModernCombobox@1.1.2
                                                     Properties:
                                                       AccessibleLabel: =Parent.DisplayName
@@ -3369,7 +3549,7 @@ Screens:
                                                       FontWeight: =""
                                                       InputTextPlaceholder: |-
                                                         ="👤Rechercher un agent"
-                                                      ItemDisplayText: =ThisItem.DisplayName
+                                                      ItemDisplayText: =ThisItem.DisplayName & " (" & Text(ThisItem.MailNickname) & ")"
                                                       Items: |-
                                                         ='Utilisateursd’Office365'.SearchUserV2({searchTerm: Self.SearchText, isSearchTermRequired: false, top: 50}).value
                                                       Required: =Parent.Required
@@ -3379,6 +3559,45 @@ Screens:
                                                       Width: =198
                                                       X: =24
                                                       Y: =36
+                                                - DataCardValue21_1:
+                                                    Control: ModernTextInput@1.1.2
+                                                    Properties:
+                                                      AccessibleLabel: =Parent.DisplayName
+                                                      Align: =Align.Right
+                                                      Default: =drpCODICandRetenu_F.Selected.MailNickname
+                                                      DisplayMode: =inpCodeES_F.DisplayMode
+                                                      FontWeight: =""
+                                                      Height: '=26  '
+                                                      Italic: =true
+                                                      MaxLength: =-1
+                                                      Placeholder: =""
+                                                      Required: =Parent.Required
+                                                      Size: =inpNumEmploi_F.Size
+                                                      TriggerOutput: =TriggerOutput.FocusOut
+                                                      ValidationState: =If(IsBlank(Parent.Error), "None", "Error")
+                                                      Width: =100
+                                                      X: =122
+                                                      Y: =66
+                                                - DataCardKey28_1:
+                                                    Control: ModernText@1.0.0
+                                                    Properties:
+                                                      Height: '=26  '
+                                                      Italic: =true
+                                                      PaddingBottom: =0
+                                                      PaddingLeft: =0
+                                                      PaddingRight: =0
+                                                      PaddingTop: =0
+                                                      RadiusBottomLeft: =0
+                                                      RadiusBottomRight: =0
+                                                      RadiusTopLeft: =0
+                                                      RadiusTopRight: =0
+                                                      Size: =txtTypeDemandeLabel_F.Size
+                                                      Text: ="Numéro Ulis:"
+                                                      VerticalAlign: =txtTypeDemandeLabel_F.VerticalAlign
+                                                      Width: =86
+                                                      Wrap: =false
+                                                      X: =80
+                                                      Y: =68
                                           - crdDatePromSend_F:
                                               Control: TypedDataCard@1.0.7
                                               Variant: DateEdit
@@ -3668,6 +3887,23 @@ Screens:
                                         )
                                     ,
 
+                                    _StatusKey.CandidPVEnCours_AvecRecours,
+                                        If(IsBlank( crdCODICandRetenu_F.Update),                           
+                                                            "Le champ [" & crdCODICandRetenu_F.DisplayName &"] est obligatoire" & Char(10) )
+                                                & With({MyCategorieDocument : _DocumentCategory.PVDefinitif},
+                                            If(IsBlank(LookUp(ccLinkedDocuments,CategorieDocument.Value = MyCategorieDocument)),
+                                                            "Le document [" & MyCategorieDocument &"] est obligatoire" & Char(10) ) 
+                                        )
+                                    ,
+                                    _StatusKey.CandidPVEnCours_SansRecours,
+                                        If(IsBlank( crdCODICandRetenu_F.Update),                           
+                                                            "Le champ [" & crdCODICandRetenu_F.DisplayName &"] est obligatoire" & Char(10) )
+                                                & With({MyCategorieDocument : _DocumentCategory.PVDefinitif},
+                                            If(IsBlank(LookUp(ccLinkedDocuments,CategorieDocument.Value = MyCategorieDocument)),
+                                                            "Le document [" & MyCategorieDocument &"] est obligatoire" & Char(10) ) 
+                                        )
+                                    ,
+
                                     _StatusKey.PromAuGWDG,
                                         If(!IsBlank(txtValidationAppro_F.Text),                           
                                                             "----- Pour l'approbabtion -----" & Char(10) & txtValidationAppro_F.Text
@@ -4257,7 +4493,9 @@ Screens:
                                     Action: =_ActionOnForm.DirCandRetenu
                                     ActionState: |
                                       =If(IsBlank(txtValidationMsg_F.Text) , // Rend l’action disponible uniquement lorsque le message de validation est vide (aka : tous les champs obligatoires sont bien remplis)
-                                          Self.StateValues.Available,
+                                          If(IsBlank(drpDirReaffCandidatRetenu_F.Selected.DisplayName),Self.StateValues.Unavailable,
+                                          Self.StateValues.Available
+                                          ),
                                           Self.StateValues.MissingRequiredFields
                                       )
                                     BasePaletteColor: =RGBA(141, 229, 250, 1)
@@ -4284,6 +4522,9 @@ Screens:
                                                       StatusKey:          MyNextStatus.StatusKey,   
                                                       'Statut Detail':    MyNextStatus.StatutDetail,
                                                       'Statut General':   MyNextStatus.StatutGeneral
+                                                  },
+                                                  {
+                                                      DirReaffCandidatRetenu: frmNextStep_F.Updates.DirReaffCandidatRetenu
                                                   }
                                               )
                                           });
@@ -4311,7 +4552,9 @@ Screens:
                                                               "<p>Bonjour,</p> " 
                                                                       & "<p><i>[" & MyTypeDemande & "]: </i></p>
                                                                       <p> Votre demande a un nouveau statut : " & MyNextStatus.StatutDetail & " (" & MyNextStatus.StatutGeneral & ")" &
-                                                                      "<p><b>" & MySpw & "</b> (emploi <b>" & MyNumEmploi & "</b>)."
+                                                                      "<p><b>" & MySpw & "</b> (emploi <b>" & MyNumEmploi & "</b>).</p>"
+                                                                      & "<p><u>Candidat retenu : </u> <br>" 
+                                                                      & "<i>" &  drpDirReaffCandidatRetenu_F.Selected.DisplayName & " (Ulis :  "& drpDirReaffCandidatRetenu_F.Selected.MailNickname  &")</i></p>"
                                                                       & _txtMessage.EmailFooter 
                                                           );
                                                       )
@@ -4331,7 +4574,7 @@ Screens:
                                               Notify(_txtMessage.UpdateFailled,NotificationType.Error,5000)
                                           );
                                       );
-                                    OnUnavailable: =Notify(_txtMessage.ActionUnsupported,NotificationType.Error,2000)
+                                    OnUnavailable: =Notify("Le champ [Candidat reaffecté] est obligatoire",NotificationType.Warning,4000)
                                     SourceStatuses: =[_StatusKey.DirReaffAuditCODI]
                                     SourceStatusesData: |-
                                       =ForAll(Self.SourceStatuses As MySourceStatus,
@@ -4385,6 +4628,9 @@ Screens:
                                                       StatusKey:          MyNextStatus.StatusKey,   
                                                       'Statut Detail':    MyNextStatus.StatutDetail,
                                                       'Statut General':   MyNextStatus.StatutGeneral
+                                                  },
+                                                  {
+                                                      DirReaffCandidatRetenu: frmNextStep_F.Updates.DirReaffCandidatRetenu
                                                   }
                                               )
                                           });
@@ -4564,9 +4810,9 @@ Screens:
                                     AccessibleLabel: ="Bouton Refuser la demande"
                                     Action: =_ActionOnForm.CODICandSelect
                                     ActionState: |
-                                      =If(Not(IsBlank(crdCODICandRetenu_F.Update)), 
+                                      =If(IsBlank(txtValidationMsg_F.Text) , // Rend l’action disponible uniquement lorsque le message de validation est vide (aka : tous les champs obligatoires sont bien remplis)
                                           Self.StateValues.Available,
-                                          Self.StateValues.Unavailable
+                                          Self.StateValues.MissingRequiredFields
                                       )
                                     BasePaletteColor: =RGBA(141, 229, 250, 1)
                                     EligibleItemsCount: =Blank()
@@ -4576,10 +4822,9 @@ Screens:
                                     NextStatus: =_StatusKey.CandidPVTermines
                                     NextStatusData: =LookUp(_StatutConfigTable,StatusKey = Self.NextStatus)
                                     OnAlreadyRunning: =Notify(_txtMessage.ActionUnsupported,NotificationType.Information,2000)
-                                    OnAvailable: "=// ## Logique métier de changement de statut\nWith(\n    // Défini via un With le prochain statut et le cas échéant l'actuel afin de rendre la suite du code générique\n    { \n        MyNextStatus :  Self.NextStatusData // Statut cible appliqué aux demandes après exécution\n    },\n\n    UpdateContext({vcDemande : \n        //# Applique le changement de statut à la demande traitée       \n        Patch(AIRH_Demandes,\n            vcDemande,\n            {CODICandidatRetenu:  frmNextStep_F.Updates.CODICandidatRetenu},\n            {\n                StatusKey:          MyNextStatus.StatusKey,\n                'Statut Detail':    MyNextStatus.StatutDetail,\n                'Statut General':   MyNextStatus.StatutGeneral\n            }\n        )\n    });\n\n    If(IsEmpty(Errors(AIRH_Demandes)),\n        Concurrent(\n            // # Notifie des changements de statut\n            With(\n                {\n                    MyTypeDemande :     vcDemande.'Type de demande'.Value,\n                    MySpw :             vcDemande.SPW,\n                    MyNumEmploi:        vcDemande.'Numéro d''emploi',\n                    MyRecipients :      _DACEmailList()\n                },\n                If(IsBlank(MyRecipients),\n                    // Aucune liste DFA configurée pour ce SPW ou pour la DAC\n                    Notify(_txtMessage.NoRecipient, NotificationType.Warning,5000),\n                    // Envoi standard\n                    Office365Outlook.SendEmailV2(\n                        // To  : DFA distribution list\n                        MyRecipients,    \n                        // Subject: notification métier\n                        _txtMessage.EmailSubjectPrefix \n                            &   MyTypeDemande & \" : Décision du CODI enregistrée\" & \" (\" & MyNumEmploi & \")\",\n                        // Body: informations détaillées\n                        \"<p>Bonjour,</p>\n                        <p>La décision du CODI enregistrée pour la <i>\" & MyTypeDemande & \"</i> via <b>\" & _AppName & \"</b> \" &\n                        \"pour le <b>\" & vcDemande.SPW & \"</b> (emploi <b>\" & vcDemande.'Numéro d''emploi' & \"</b>).\"\n                        & \"<br><u>Candidat retenu : </u> <br>\" \n                        & \"<i>\" &  vcDemande.CODICandidatRetenu.DisplayName & \" (\"& vcDemande.CODICandidatRetenu.Email  &\")</i>\" \n                        & _txtMessage.EmailFooter \n                    )\n                )\n            ),\n            // # Trace chaque changement de statut dans l'historique\n            fxLogStatusHistory(\n                ShowColumns(vcDemande,ID,'Numéro d''emploi'),\n                MyNextStatus.StatusKey,\n                vvUserRole.Key\n            )\n        );\n        ResetForm(frmGeneralInfo_F);\n        Navigate(Screen_Main_M);\n        \n        ,\n        Notify(_txtMessage.UpdateFailled,NotificationType.Error,5000)\n    );\n);\n\n\n"
+                                    OnAvailable: "=// ## Logique métier de changement de statut\nWith(\n    // Défini via un With le prochain statut et le cas échéant l'actuel afin de rendre la suite du code générique\n    { \n        MyNextStatus :  Self.NextStatusData // Statut cible appliqué aux demandes après exécution\n    },\n\n    UpdateContext({vcDemande : \n        //# Applique le changement de statut à la demande traitée       \n        Patch(AIRH_Demandes,\n            vcDemande,\n            {CODICandidatRetenu:  frmNextStep_F.Updates.CODICandidatRetenu},\n            {\n                StatusKey:          MyNextStatus.StatusKey,\n                'Statut Detail':    MyNextStatus.StatutDetail,\n                'Statut General':   MyNextStatus.StatutGeneral\n            }\n        )\n    });\n\n    If(IsEmpty(Errors(AIRH_Demandes)),\n        Concurrent(\n            // # Notifie des changements de statut\n            With(\n                {\n                    MyTypeDemande :     vcDemande.'Type de demande'.Value,\n                    MySpw :             vcDemande.SPW,\n                    MyNumEmploi:        vcDemande.'Numéro d''emploi',\n                    MyRecipients :      _DACEmailList()\n                },\n                If(IsBlank(MyRecipients),\n                    // Aucune liste DFA configurée pour ce SPW ou pour la DAC\n                    Notify(_txtMessage.NoRecipient, NotificationType.Warning,5000),\n                    // Envoi standard\n                    Office365Outlook.SendEmailV2(\n                        // To  : DFA distribution list\n                        MyRecipients,    \n                        // Subject: notification métier\n                        _txtMessage.EmailSubjectPrefix & \" \" & MyTypeDemande & \": \"\n                            & \" Décision du CODI enregistrée\" & \" (\" & MyNumEmploi & \")\",\n                        // Body: informations détaillées\n                        \"<p>Bonjour,</p>\"\n                        & \"<p><i><u>Concerne : [\" & MyTypeDemande & \"]: </i></u></p>\"\n                        & \" <p>La décision du CODI est enregistrée pour l'emploi <b>\" & vcDemande.'Numéro d''emploi' & \"</b> ( <b>\" & vcDemande.SPW & \"</b>).</p>\"\n                        & \"<p><u>Candidat retenu : </u> <br>\" \n                        & \"<i>\" &  vcDemande.CODICandidatRetenu.DisplayName & \" (Ulis :  \"& drpCODICandRetenu_F.Selected.MailNickname  &\")</i></p>\" \n                        & _txtMessage.EmailFooter \n                    )\n                )\n            ),\n            // # Trace chaque changement de statut dans l'historique\n            fxLogStatusHistory(\n                ShowColumns(vcDemande,ID,'Numéro d''emploi'),\n                MyNextStatus.StatusKey,\n                vvUserRole.Key\n            )\n        );\n        ResetForm(frmGeneralInfo_F);\n        Navigate(Screen_Main_M);\n        \n        ,\n        Notify(_txtMessage.UpdateFailled,NotificationType.Error,5000)\n    );\n);\n\n\n"
                                     OnUnavailable: |
-                                      =// ## Informe l'utilisateur des informations requises
-                                      Notify("Un candidat est obligatoire pour valider une demande" ,NotificationType.Warning ,2000);
+                                      =Notify(_txtMessage.ActionUnsupported ,NotificationType.Warning ,2000);
                                     SourceStatuses: =[""]
                                     SourceStatusesData: |-
                                       =ForAll(Self.SourceStatuses As MySourceStatus,
@@ -4903,7 +5148,21 @@ Screens:
                                     PaddingRight: =0
                                     PaddingTop: =0
                                     Size: =10
-                                    Text: =ThisItem.Nom
+                                    Text: |-
+                                      =With(
+                                          {
+                                              m: Match(
+                                                  ThisItem.Nom,
+                                                  "^(.*)_\d{14}(\.[^.]+)$"
+                                              )
+                                          },
+                                          If(
+                                              IsBlank(m.FullMatch),
+                                              ThisItem.Nom,
+                                              First(m.SubMatches).Value &
+                                              Last(m.SubMatches).Value
+                                          )
+                                      )
                                     VerticalAlign: =VerticalAlign.Top
                                     Width: =Parent.Width - Self.X
                                     X: =imgDocumentMiniature_F.X + imgDocumentMiniature_F.Width
@@ -5172,3 +5431,42 @@ Screens:
             Width: =40
             X: =Parent.Width - Self.Width
             Y: =Parent.Height - Self.Height
+      - ctnSpinner_F:
+          Control: GroupContainer@1.5.0
+          Variant: ManualLayout
+          Properties:
+            Fill: =ColorValue("#80808080")
+            Height: =ctnBodyContainer_F.Height
+            Visible: |-
+              =cmpActBtnCancel_F.IsLoading
+              ||cmpActBtnCreate_F.IsLoading
+              ||cmpActBtnDelete_F.IsLoading
+              ||cmpActBtnSave_F.IsLoading
+              ||cmpActBtnSend_F.IsLoading
+              ||cmpActbtnDACRefuse_F.IsLoading
+              ||cmpActbtnDACValidate_F.IsLoading
+              ||cmpActbtnDirCandRetenu_F.IsLoading
+              ||cmpActbtnDirCandPasRetenu_F.IsLoading
+              ||cmpActBtnCOSTRARefus_F.IsLoading
+              ||cmpActBtnCODICandidat_F.IsLoading
+              ||cmpActBtnPromRefus_F.IsLoading
+              ||cmpActBtnPromAppro_F.IsLoading
+            Width: =ctnBodyContainer_F.Width
+            X: =ctnBodyContainer_F.X
+            Y: =ctnBodyContainer_F.Y
+          Children:
+            - Spinner1:
+                Control: ModernSpinner@1.1.0
+                Properties:
+                  Height: =200
+                  Label: ="Traitement en cours"
+                  LabelPosition: =SpinnerLabelPosition.Below
+                  Width: '=300 '
+                  X: =(Parent.Width - Self.Width)/2
+                  Y: =(Parent.Height - Self.Height) / 2
+      - Text5:
+          Control: ModernText@1.0.0
+          Properties:
+            Text: =CountRows(drpDirReaffCandidatRetenu_F.SelectedItems)
+            X: =40
+            Y: =40
diff --git a/AIRH/CODE/Screen_Main_M.fx.json b/AIRH/CODE/Screen_Main_M.fx.json
index 27d47d0..ce89a6b 100644
--- a/AIRH/CODE/Screen_Main_M.fx.json
+++ b/AIRH/CODE/Screen_Main_M.fx.json
@@ -1,7 +1,6 @@
 Screens:
   Screen_Main_M:
     Properties:
-      Fill: '=Color.White '
       OnVisible: |-
         =Concurrent(
             If(IsEmpty(ccTypeDeDemande),
@@ -57,7 +56,7 @@ Screens:
                             If(
                                 !IsBlank(MyUserConfigByES),
                                 MyUserConfigByES,
-                                LookUp(ccAIRH_DFA_DAC_Infos,User().Email in IncorporationTemporaire.Email)
+                                LookUp(ccAIRH_DFA_DAC_Infos,vvUser.mail in IncorporationTemporaire.Email)
                             )
                         )
                     )
@@ -81,7 +80,7 @@ Screens:
                 If(
                     IsBlank(vvUserConfig),
                     Notify(
-                        "Votre rôle (DFA ou DAC) n’a pas pu être déterminé. Veuillez contacter le support.",
+                       "Votre rôle (DFA ou DAC) n’a pas pu être déterminé. Veuillez contacter " & vvAppConfig.AppContactPerson.Email & ".",
                         NotificationType.Error
                     )
                 );
@@ -100,6 +99,8 @@ Screens:
 
 
         Switch(vcCurrentProcess,
+            _ActionOnList.Finaliser,
+                Select(BtnLoadDemandes_M), // on recharge la liste avec le statut mis à jour mais on ne touche pas au filtre
             _ActionOnList.DACValider,
                 Select(BtnLoadDemandes_M), // on recharge la liste avec le statut mis à jour mais on ne touche pas au filtre
             _ActionOnList.DirAudition,
@@ -182,6 +183,7 @@ Screens:
                         Appearance: =TabListAppearance.SubtleCircular
                         Default: |-
                           ={Type : _TypeDemande.All}
+                        DisplayMode: =If(IsBlank(vcCurrentProcess), DisplayMode.Edit,DisplayMode.Disabled)
                         Height: =30
                         ItemDisplayText: =ThisItem.Icon &  ThisItem.Type
                         Items: |-
@@ -204,7 +206,7 @@ Screens:
                 Properties:
                   Fill: =RGBA(200, 200, 200, 1)
                   FillPortions: =0
-                  Height: "=80\r\n+ lblSelectTypeForAction_M.Height * lblSelectTypeForAction_M.Visible\r\n+ Max(\r\n    ((vcCurrentProcess = _ActionOnList.NewDemande)) * ctnSubActionButtons.Height,\r\n    (vcCurrentProcess = _ActionOnList.Filtrer) * ctnFilters.Height,\r\n    (vcCurrentProcess = _ActionOnList.Archive) * ctnArchive_M.Height\r\n) \r\n+ vvDebug * ctnTechnicalButtons_M.Height  "
+                  Height: "=80\r\n+ lblSelectTypeForAction_M.Height * lblSelectTypeForAction_M.Visible\r\n+ Max(\r\n    ((vcCurrentProcess = _ActionOnList.NewDemande)) * ctnActionChoice_M.Height,\r\n    (vcCurrentProcess = _ActionOnList.Filtrer) * ctnFilters_M.Height,\r\n    (vcCurrentProcess = _ActionOnList.Archive) * ctnArchive_M.Height\r\n) \r\n+ vvDebug * ctnTechnicalButtons_M.Height  "
                   LayoutAlignItems: =LayoutAlignItems.Center
                   LayoutDirection: =LayoutDirection.Vertical
                   LayoutMaxWidth: =Parent.Width
@@ -224,9 +226,9 @@ Screens:
                         PaddingTop: =2
                         Size: =13
                         Text: ="Veuilliez sélectionner un type de demande pour effectuer lance un processus"
-                        Visible: =tabTypeDemande_M.Selected.Type = _TypeDemande.All
+                        Visible: =tabTypeDemande_M.Selected.Type = _TypeDemande.All && Not(IsBlank(vvUserRole.Key))
                         Width: =Parent.Width
-                  - ctnActionButtons:
+                  - ctnActionButtons_M:
                       Control: GroupContainer@1.5.0
                       Variant: AutoLayout
                       Properties:
@@ -322,7 +324,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="Edit"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -412,7 +414,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey ))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="Send"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -502,7 +504,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="Checkmark"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -597,7 +599,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="AppsList"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -691,7 +693,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="ThumbLikeDislike"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -783,7 +785,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="ThumbLikeDislike"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -878,7 +880,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="AppsList"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -970,7 +972,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="ThumbLikeDislike"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -1026,7 +1028,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="ThumbLikeDislike"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -1118,7 +1120,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="AppsList"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -1225,7 +1227,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="Send"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -1281,11 +1283,11 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="Calendar"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
-                                    NextStatus: =_StatusKey.CandidPVEnCours
+                                    NextStatus: "=Switch(tabTypeDemande_M.Selected.Type,\r\n    _TypeDemande.DemandeDV, \r\n        _StatusKey.CandidPVEnCours_AvecRecours,\r\n    _TypeDemande.DemandeFF,\r\n        _StatusKey.CandidPVEnCours_SansRecours,\r\n        \r\n        _StatusKey.CandidPVEnCours_AvecRecours\r\n)"
                                     NextStatusData: =LookUp(_StatutConfigTable,StatusKey=Self.NextStatus)
                                     OnAlreadyRunning: =Select(btnCandAudExit_M); //Arrêter Le processus
                                     OnAvailable: |-
@@ -1374,7 +1376,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="ThumbLikeDislike"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -1418,7 +1420,7 @@ Screens:
                                           });
                                       )
                                     OnUnavailable: =Select(btnGenericActionUnsupported_M)
-                                    SourceStatuses: =[_StatusKey.CandidPVEnCours]
+                                    SourceStatuses: =[_StatusKey.CandidPVEnCours_AvecRecours,_StatusKey.CandidPVEnCours_SansRecours]
                                     SourceStatusesData: |-
                                       =ForAll(Self.SourceStatuses As MySourceStatus,
                                           LookUp(_StatutConfigTable,StatusKey=MySourceStatus.Value)
@@ -1465,7 +1467,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="DrawShape"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -1571,7 +1573,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="Send"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -1627,7 +1629,7 @@ Screens:
                                               Self.StateValues.Available
                                       )
                                     BasePaletteColor: =
-                                    EligibleItemsCount: =CountRows(Filter(ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey))
+                                    EligibleItemsCount: =CountRows( Filter( ccDemandes,StatusKey in Self.SourceStatusesData.StatusKey && ( tabTypeDemande_M.Selected.Type =_TypeDemande.All || ThisRecord.TypeDemandeValue = tabTypeDemande_M.Selected.Type )))
                                     Height: =32
                                     Icon: ="Checkmark"
                                     InteractionEnabled: =tabTypeDemande_M.Selected.Type <> _TypeDemande.All
@@ -1688,7 +1690,7 @@ Screens:
                                     Width: =130
                                     X: =496
                                     Y: =476
-                  - ctnSubActionButtons:
+                  - ctnActionChoice_M:
                       Control: GroupContainer@1.5.0
                       Variant: AutoLayout
                       Properties:
@@ -1731,6 +1733,9 @@ Screens:
                                           UpdateContext({vcDemande : 
                                               Patch(AIRH_Demandes,
                                                   Defaults(AIRH_Demandes),
+                                                  {
+                                                      'Type de demande': {Value : ThisItem.Value}
+                                                  },
                                                   {
                                                       'Statut General': MyStatusDetail.StatutGeneral,
                                                       'Statut Detail':  MyStatusDetail.StatutDetail
@@ -1762,7 +1767,7 @@ Screens:
                                     VerticalAlign: =VerticalAlign.Middle
                                     Width: =160
                                     Y: =4
-                  - ctnAction2:
+                  - ctnActionButtons2_M:
                       Control: GroupContainer@1.5.0
                       Variant: AutoLayout
                       Properties:
@@ -1783,12 +1788,12 @@ Screens:
                         Visible: |+
                           =true  
                           /*
-                          (_MuliSelectOnDemandes || _SingleSelectOnDemandes)
+                          (_MultiSelectOnDemandes || _SingleSelectOnDemandes)
                           && vcCurrentProcess = Blank()
                           */
                         Width: =Parent.Width
                       Children:
-                        - Container6:
+                        - ctnActionOnSibleSelect_M:
                             Control: GroupContainer@1.5.0
                             Variant: AutoLayout
                             Properties:
@@ -1807,7 +1812,7 @@ Screens:
                               PaddingRight: =8
                               PaddingTop: =8
                             Children:
-                              - Text4:
+                              - lblSelectSingleDemande_M:
                                   Control: ModernText@1.0.0
                                   Properties:
                                     Color: =App.Theme.Colors.Lighter10
@@ -1817,7 +1822,7 @@ Screens:
                                     Size: =14
                                     Text: ="Double-cliquez ou sélectionnez la demande à traiter."
                                     Underline: =true
-                                    Visible: =vcCurrentProcess in [_ActionOnList.Finaliser,_ActionOnList.DACValider,_ActionOnList.DirAudition,_ActionOnList.PromReception]
+                                    Visible: =vcCurrentProcess in [_ActionOnList.Finaliser,_ActionOnList.DACValider,_ActionOnList.DirAudition,_ActionOnList.PromReception, _ActionOnList.CODIDecision]
                                     Width: =400
                               - btnModifier_M:
                                   Control: ModernButton@1.0.0
@@ -1890,6 +1895,15 @@ Screens:
                                       );
                                     Size: =12
                                     Text: ="Consulter"
+                                    Visible: |-
+                                      =With({MyAction : _ActionOnList.Consulter},
+                                          // [Pattern cmpActionButton]
+                                          // Contrôle la visibilité de l'action indépendamment de son état métier.
+                                          // Une action peut être visible mais indisponible.
+                                          // La visibilité détermine uniquement si l'utilisateur est autorisé à voir cette action dans l'interface.
+                                          // MyAction in vcStatutConfig.Actions &&
+                                          MyAction in vvUserRole.Actions
+                                      )
                                     Width: =100
                               - cmpActBtnAddDoc_M:
                                   Control: CanvasComponent
@@ -1936,7 +1950,9 @@ Screens:
                                           Select(btnGridClearSelect_M);
                                           UpdateContext({
                                               vcCurrentProcess :      MyProcess,
-                                              vcWizardStep :   1,
+                                              vcWizardStep :          1,
+                                              vcWizardCompleted:      false,
+                                              vcWizardStepAfterSelect: numAddDocSelect_M.Value + 1,
                                               vcApplyFilter :         MyFilterToApply
                                           });
                                       )
@@ -1984,6 +2000,7 @@ Screens:
                                     LayoutMinWidth: =16
                                     OnCheck: =
                                     OnUncheck: =
+                                    Visible: =Not(IsBlank(vvUserRole.Key))
                                     Width: =155
                               - cmpActBtnFilters_M:
                                   Control: CanvasComponent
@@ -2146,7 +2163,7 @@ Screens:
                                     Width: =100
                                     X: =496
                                     Y: =476
-                  - ctnFilters:
+                  - ctnFilters_M:
                       Control: GroupContainer@1.5.0
                       Variant: AutoLayout
                       Properties:
@@ -2164,7 +2181,7 @@ Screens:
                         PaddingTop: =4
                         Visible: =vcCurrentProcess = _ActionOnList.Filtrer
                         Width: =Parent.Width
-                        Y: =40 +(vcCurrentProcess = _ActionOnList.NewDemande) * ctnSubActionButtons.Height
+                        Y: =40 +(vcCurrentProcess = _ActionOnList.NewDemande) * ctnActionChoice_M.Height
                       Children:
                         - ctnFilterSPW_M:
                             Control: GroupContainer@1.5.0
@@ -2363,7 +2380,7 @@ Screens:
                         PaddingTop: =4
                         Visible: =vcCurrentProcess = _ActionOnList.Archive
                         Width: =Parent.Width
-                        Y: =40 +(vcCurrentProcess = _ActionOnList.NewDemande) * ctnSubActionButtons.Height
+                        Y: =40 +(vcCurrentProcess = _ActionOnList.NewDemande) * ctnActionChoice_M.Height
                       Children:
                         - cntArchiveEmploi_M:
                             Control: GroupContainer@1.5.0
@@ -2398,7 +2415,7 @@ Screens:
                                       ="🔎 Numéro Emploi"
                                     TriggerOutput: =TriggerOutput.Delayed
                                     Width: =180
-                        - Button1:
+                        - btnArchiveSearch_M:
                             Control: ModernButton@1.0.0
                             Properties:
                               Icon: ="Search"
@@ -2428,7 +2445,26 @@ Screens:
                         Visible: =vvDebug
                         Width: =Parent.Width
                       Children:
-                        - radRole_M:
+                        - infTechButtons_M:
+                            Control: ModernInformationButton@1.0.1
+                            Properties:
+                              Color: =RGBA(189, 49, 51, 1)
+                              Content: ="Ce conteneur centralise des actions génériques réutilisables. Les boutons qu’il contient sont principalement déclenchés depuis d’autres parties de l’application via Select(), afin de mutualiser leur logique. Ils peuvent également être exécutés manuellement en mode développement, notamment pour recharger des données ou faciliter le diagnostic."
+                              Height: =20
+                              LayoutMinHeight: =16
+                              LayoutMinWidth: =16
+                              Width: =20
+                        - lblTechButton_M:
+                            Control: ModernText@1.0.0
+                            Properties:
+                              Color: =RGBA(160, 64, 8, 1)
+                              FontWeight: =FontWeight.Bold
+                              LayoutMinHeight: =16
+                              LayoutMinWidth: =16
+                              Size: =12
+                              Text: ="Boutons techniques ou de debug"
+                              Width: =220
+                        - radChangeRole_M:
                             Control: ModernRadio@1.0.2
                             Properties:
                               Default: |-
@@ -2461,7 +2497,7 @@ Screens:
                               BasePaletteColor: =RGBA(246, 142, 104, 1)
                               FontWeight: =""
                               Height: =20
-                              Icon: ="Library"
+                              Icon: ="Flash"
                               OnSelect: "=/*######### GRID DATA REFRESH CONTROLLER #######################################\nBouton technique centralisant le rechargement complet du FluentDetailList.\n\n    Son OnSelect est appelé depuis différents points de l'application afin de\n    garantir un comportement de rafraîchissement cohérent et d'éviter la duplication\n    de code :\n    • OnVisible de l'écran\n    • Après des opérations de masse (btnBulk...)\n    • À la fin de certains traitements métier (btnProcessDo...)\n    • En mode debug (ex. changement de rôle ou validation d'un scénario)\n\n    Lors d'un rafraîchissement :\n        • l'état de tri de la gille est initialisé \n        • Les données sont chargées depuis SharePoint dans une collection locale\n        • Les règles d'accès liées au rôle et au périmètre SPW sont appliquées\n        • Les métadonnées d'affichage sont pré-calculées\n        • La configuration des colonnes est adaptée au rôle courant\n        • Toute sélection active est réinitialisée\n\n    Le FluentDetailList travaille ensuite exclusivement sur les collections locales.\n    Les filtres, recherches et tris interactifs sont effectués dans la propriété\n    Items du contrôle sans réinterroger SharePoint, ce qui améliore les performances\n    et garantit un comportement cohérent de la grille.\n\n    Le contrôle FluentDetailList est un composant du Power Platform Creator Kit\n    https://learn.microsoft.com/en-us/power-platform/guidance/creator-kit/detailslist\n    Il est utilisé pour l'affichage d'une liste des \"Demandes\"\n###############################################################################*/\n\n/******** Initialisation de l'état de la grille ********************************\n    Les variables vcDemandes... sont nécessaires à la configuration du contrôle FluentDetailList\n    Définit l'état par défaut du FluentDetailList avant le chargement des données.\n    Le tri initial est appliqué sur la date de dernière modification afin de\n        présenter en priorité les demandes les plus récemment mises à jour.\n*******************************************************************************/\nUpdateContext({\n    vcDemandesSortField  : _DemandesFieldName.Modified,\n    vcDemandesSortOrder : false\n});\n\n/******** Précharge les demandes accessibles à l'utilisateur. *****************\n    La collection ccDemandes constitue la source de données principale du FluentDetailList.\n    Les règles de visibilité liées aux rôles et aux autorisations sont appliquées\n        dès le chargement afin de ne conserver que les demandes accessibles à l'utilisateur.\n    Les métadonnées utilisées par l'affichage sont également pré-calculées afin\n        d'éviter des traitements répétitifs lors des interactions avec la grille.\n    Remarques : \n        Les recherches, filtres et tris utilisateur sont ensuite effectués sur cette collection\n                locale via la propriété Items du FluentDetailList sans nouvel appel à SharePoint.\n*******************************************************************************/\nClearCollect(ccDemandes,\n    /*---- Logique de Construction de la source de données de la grille : ---------\n        • ShowColumns : sélection des colonnes utiles\n        • Filter : filtrage selon les droits de l'utilisateur\n        • AddColumns : préchargement des métadonnées de type et de statut\n        • AddColumns : enrichissement des données pour l'affichage\n        • DropColumns : nettoyage des colonnes temporaires\n    -------------------------------------------------------------------------*/\n    DropColumns(                    // Nettoie les métadonnées intermédiaires après enrichissement des demandes\n        AddColumns(                 // Construit les attributs d'affichage et d'interaction utilisés par la grille\n            AddColumns(             // Précharge temporairement les métadonnées liées au type et au statut pour éviter des recherches répétées lors de l'enrichissement\n                If(vcCurrentProcess <> _ActionOnList.Archive,\n                    Filter(             // Affiche uniquement les demandes actives accessibles à l'utilisateur selon son rôle et son périmètre SPW\n                        ShowColumns(    // Limite les données chargées aux colonnes utilisées par la grille\n                            AIRH_Demandes,\n                            // Les colonnes à afficher\n                            ID,Archive,'Type de demande',Lot,'Numéro d''emploi',\n                            'Code ES',SPW,'Département (Abréviation)',Département,Direction,Service,Rang,\n                            'Date de demande du CODI',\n                            'Statut General','Statut Detail',StatusKey,\n                            DateDirReaffFinConsult,\n                            Modifié,'Modifié par'\n                        ),\n                        // Les filtres à appliquer\n                        And( \n                            Archive = false,    // Seules les demandes actives sont affichées -  Exclut les demandes archivées\n                            Or(                 // Les demandes en brouillon sont masquées pour la DAC\n                                vvUserRole.Key <> _Acteur.DAC.Key, \n                                StatusKey <> _StatusKey.DemandeBrouillon\n                            ), \n                            Or(                 // Le périmètre de consultation est limité au SPW de l'utilisateur, sauf pour la DAC\n                                vvUserRole.Key = _Acteur.DAC.Key,\n                                SPW = vvUserSPW\n                            )\n                        )\n                    ),\n                    Filter(             // Affiche uniquement les demandes actives accessibles à l'utilisateur selon son rôle et son périmètre SPW\n                        ShowColumns(    // Limite les données chargées aux colonnes utilisées par la grille\n                            AIRH_Demandes,\n                            // Les colonnes à afficher\n                            ID,Archive,'Type de demande',Lot,'Numéro d''emploi',\n                            'Code ES',SPW,'Département (Abréviation)',Département,Direction,Service,Rang,\n                            'Date de demande du CODI',\n                            'Statut General','Statut Detail',StatusKey,\n                            DateDirReaffFinConsult,\n                            Modifié,'Modifié par'\n                        ),\n                        // Les filtres à appliquer\n                        And( \n                            Archive = true,\n                            StartsWith( NumEmploi,txtArchiveEmploi_M.Text), \n                            Or(                 // Les demandes en brouillon sont masquées pour la DAC\n                                vvUserRole.Key <> _Acteur.DAC.Key, \n                                StatusKey <> _StatusKey.DemandeBrouillon\n                            ), \n                            Or(                 // Le périmètre de consultation est limité au SPW de l'utilisateur, sauf pour la DAC\n                                vvUserRole.Key = _Acteur.DAC.Key,\n                                SPW = vvUserSPW\n                            )\n                        )\n                    ) \n                    \n                )As MyDemandesRawData,\n                // Les métadonnées à précharger\n                MyTypeInfo ,    // Précharge la configuration du type de demande\n                    LookUp(_TypeDemandeConfigTable, Type = MyDemandesRawData.TypeDemande.Value),\n                MyStatusInfo ,  // Précharge la configuration du statut de la demande\n                    LookUp(_StatutConfigTable,StatusKey = MyDemandesRawData.StatusKey)\n            ) As MyDemandes,\n            // Les attributs à ajouter\n            // • Attributs d'affichage du type de demande\n            TypeDemandeValue ,      // Libellé du type de demande\n                MyDemandes.TypeDemande.Value,\n            TypeDemandeIcon,        // Type de demande enrichi avec son indicateur visuel de couleur\n                MyDemandes.MyTypeInfo.Icon & MyDemandes.TypeDemande.Value,\n            TypeDemandeColor,       // Couleur associée au type de demande\n                MyDemandes.MyTypeInfo.ColorHex,\n            // • Attributs liés aux actions attendues\n            ActionOwner,            // Acteur responsable de la prochaine action avec indicateur d'action immédiate si requis\n                Switch(\n                    MyDemandes.MyStatusInfo.ActionOwner.Key, \n                    _Acteur.DAC.Key, If(vvUserRole.Key = _Acteur.DAC.Key && MyDemandes.MyStatusInfo.ActionRequired, \"⚡\") & _Acteur.DAC.Text, \n                    _Acteur.DFA.Key, If(vvUserRole.Key = _Acteur.DFA.Key && MyDemandes.MyStatusInfo.ActionRequired, \"⚡\") &_Acteur.DFA.Text, \n                    \"\"\n                ),\n            ActionOwnerColor,       // Couleur associée à l'acteur responsable\n                Switch(\n                    MyDemandes.MyStatusInfo.ActionOwner.Key, \n                    _Acteur.DAC.Key, _Acteur.DAC.ColorHex, \n                    _Acteur.DFA.Key, _Acteur.DFA.ColorHex, \n                    \"rgb(0, 0, 0)\"\n                ),\n            // • Attributs d'information\n            ModifiedBy,             // Nom du dernier utilisateur ayant modifié la demande\n                MyDemandes.Editor.DisplayName,\n            // • Attributs de suivi du processus\n            StatutSortOrder,        // Ordre de tri représentant la progression de la demande dans le processus\n                MyDemandes.MyStatusInfo.StatutGeneral_SortOrder & MyDemandes.MyStatusInfo.StatusDetail_SortOrder,\n            StatutProgressBar,      // Représentation visuelle de l'avancement de la demande dans le processus (▰▰▰▰▰▰▰✔)\n                MyDemandes.MyStatusInfo.StatutProgressBar,\n            StatutEcheance ,        // Échéance ou jalon métier associé à certains statuts\n                Switch(MyDemandes.StatusKey,\n                    _StatusKey.DirReaffConsult,MyDemandes.DateDirReaffFinConsult,\n                    Blank()\n                ),\n            // • Attributs techniques FluentDetailList\n            RecordSelected  ,       // Permet de forcer la sélection d'une ligne dans le FluentDetailList (non sélectionnée par défaut)\n                false,\n            expand,                 // Permet d'afficher une demande sur plusieurs lignes dans le FluentDetailList (vue compacte par défaut)\n                false\n        ),\n        // Les colonnes à oublier\n        MyStatusInfo,MyTypeInfo,Editor,Archive,TypeDemande\n    )\n);\n\n\n/******** Construit la configuration du FluentDetailList adaptée au rôle courant. ************\n    Le catalogue de référence contient l'ensemble des colonnes disponibles ainsi que leurs propriétés d'affichage (v*sibilité, tri, largeur, comporteme*t...).\n    Seules les colonnes autorisées pour le rôle de l'utilisateur sont conservées.\n    Cette configuration sert ensuite de source unique pou* le rendu de la grille.\n********************************************************************************************/\nClearCollect(ccDetailsListColumnsConfig,\n    DropColumns(\n        Filter(\n            _tblDemandesFieldCatalog, \n            vvUserRole.Key in View\n        ),\n        View\n    )\n);\n\n/******** Reset selection ***************************************************************\n    Force la suppression de toute sélection résiduelle après le rechargement de la grille \n**************************************************************************************/\nSelect(btnGridClearSelect_M); "
                               PaddingBottom: =5
                               PaddingLeft: =12
@@ -2471,133 +2507,13 @@ Screens:
                               RadiusBottomRight: =4
                               RadiusTopLeft: =4
                               RadiusTopRight: =4
-                              Text: ="Load Demandes"
-                              VerticalAlign: =VerticalAlign.Middle
-                              Visible: =vvDebug
-                              Width: =160
-                              X: =928
-                              Y: =18
-                        - BtnReloadRole_M_DEBUG:
-                            Control: ModernButton@1.0.0
-                            Properties:
-                              Align: =Align.Center
-                              BasePaletteColor: =RGBA(246, 142, 104, 1)
-                              FontWeight: =""
-                              Height: =20
-                              OnSelect: |-
-                                =//This is just for Dev and debug prupose.
-                                //This logic must stay in the App.Onstart and can't be performed via a button as it could break logic due to (a risk of) concurrent operation
-
-                                /*
-                                ## Configuration preload (notifications, contacts, etc.)
-                                - Loads configuration once to avoid repeated SharePoint calls
-                                - Used later for notification logic (per SPW)
-                                - but also for user context determination hereunder
-                                */
-                                ClearCollect(ccAIRH_DFA_DAC_Infos,
-                                    ShowColumns(
-                                        AIRH_DFA_DAC_Infos,
-                                        Role,ES,SPW,ContactPerson,AdditionnalContactEmail,IncorporationTemporaire
-                                    )
-                                );
-
-
-                                /*
-                                ## User profile initialization
-                                - Retrieves current user profile from M365
-                                - Stores it globally to avoid repeated connector calls
-                                */
-                                Set(vvUser,'Utilisateursd’Office365'.MyProfileV2());
-
-                                /*
-                                ## User ES extraction
-                                - Extracts the ES (department) from the user profile
-                                - Used as the primary key to determine SPW and role
-                                */
-                                With(
-                                    {MyUserES: vvUser.department},
-                                    /*
-                                    ## User configuration resolution
-                                    - Resolves user mapping based on ES first (standard case)
-                                    - Falls back to temporary incorporation if ES is not found
-                                    - Ensures every user can be mapped to a SPW and a Role
-                                    */
-                                    With(
-                                        {MyUserConfigByES:
-                                                LookUp(ccAIRH_DFA_DAC_Infos,!IsBlank(MyUserES) && MyUserES in ES.Value) 
-                                        },
-                                        Set(
-                                            vvUserConfig,
-                                            If(
-                                                !IsBlank(MyUserConfigByES),
-                                                MyUserConfigByES,
-                                                LookUp(ccAIRH_DFA_DAC_Infos,User().Email in IncorporationTemporaire.Email)
-                                            )
-                                        )
-                                    )
-                                );
-
-
-                                /*
-                                ## User context initialization
-                                - Exposes key properties for the rest of the app
-                                - Acts as a lightweight "current user context"
-                                */
-                                Set(vvUserSPW, vvUserConfig.SPW);
-                                Set(vvUserRole, Switch( vvUserConfig.Role.Value, _Acteur.DFA.Key,_Acteur.DFA,_Acteur.DAC.Key,_Acteur.DAC,Blank()));
-
-
-
-                                /*
-                                ## Safety fallback
-                                - Prevents undefined behavior if no configuration is found
-                                - Allows controlled handling of edge cases
-                                */
-                                If(
-                                    IsBlank(vvUserConfig),
-                                    Notify(
-                                        "Votre rôle (DFA ou DAC) n’a pas pu être déterminé. Veuillez contacter le support.",
-                                        NotificationType.Error
-                                    )
-                                );
-
-
-
-                                Reset(radRole_M);
-                              PaddingBottom: =5
-                              PaddingLeft: =12
-                              PaddingRight: =12
-                              PaddingTop: =5
-                              RadiusBottomLeft: =4
-                              RadiusBottomRight: =4
-                              RadiusTopLeft: =4
-                              RadiusTopRight: =4
-                              Text: ="Reload Role"
+                              Size: =10
+                              Text: ="Load/Refresh Demandes"
                               VerticalAlign: =VerticalAlign.Middle
                               Visible: =vvDebug
                               Width: =160
                               X: =928
                               Y: =18
-                        - btnConfigView_M:
-                            Control: ModernButton@1.0.0
-                            Properties:
-                              Align: =Align.Center
-                              Appearance: =If(vcShowConfigTable,    ButtonAppearance.Secondary,ButtonAppearance.Primary)
-                              FontWeight: =""
-                              OnSelect: =UpdateContext({vcShowConfigTable:!vcShowConfigTable});
-                              PaddingBottom: =5
-                              PaddingLeft: =12
-                              PaddingRight: =12
-                              PaddingTop: =5
-                              RadiusBottomLeft: =4
-                              RadiusBottomRight: =4
-                              RadiusTopLeft: =4
-                              RadiusTopRight: =4
-                              Text: ="⚙️ Configurer la Vue"
-                              VerticalAlign: =VerticalAlign.Middle
-                              Visible: =vvDebug // Cette fonctionalité n'est pas 100% fonctionnelle (mais non requise dans les US)
-                              Width: =200
-                              X: =Parent.Width - Self.Width
                         - btnGenericExitProcess_M:
                             Control: ModernButton@1.0.0
                             Properties:
@@ -2627,7 +2543,7 @@ Screens:
                               Text: ="GenericExitProcess"
                               VerticalAlign: =VerticalAlign.Middle
                               Visible: =vvDebug
-                              Width: =160
+                              Width: =140
                               X: =928
                               Y: =18
                         - btnGenericActionUnsupported_M:
@@ -2652,9 +2568,33 @@ Screens:
                               Text: ="GenericActionUnsupported"
                               VerticalAlign: =VerticalAlign.Middle
                               Visible: =vvDebug
-                              Width: =160
+                              Width: =150
                               X: =928
                               Y: =18
+                        - btnConfigView_M:
+                            Control: ModernButton@1.0.0
+                            Properties:
+                              Align: =Align.Center
+                              Appearance: =If(vcShowConfigTable,    ButtonAppearance.Secondary,ButtonAppearance.Primary)
+                              BasePaletteColor: =RGBA(246, 142, 104, 1)
+                              FontWeight: =""
+                              Height: =20
+                              Icon: ="Eyedropper"
+                              OnSelect: =UpdateContext({vcShowConfigTable:!vcShowConfigTable});
+                              PaddingBottom: =5
+                              PaddingLeft: =12
+                              PaddingRight: =12
+                              PaddingTop: =5
+                              RadiusBottomLeft: =4
+                              RadiusBottomRight: =4
+                              RadiusTopLeft: =4
+                              RadiusTopRight: =4
+                              Size: =10
+                              Text: ="Configurer la Vue"
+                              VerticalAlign: =VerticalAlign.Middle
+                              Visible: =vvDebug // Cette fonctionalité n'est pas 100% fonctionnelle (mais non requise dans les US)
+                              Width: =140
+                              X: =Parent.Width - Self.Width
             - ctnBody_M:
                 Control: GroupContainer@1.5.0
                 Variant: AutoLayout
@@ -2699,7 +2639,7 @@ Screens:
                           || ctnProcessCandSendDFA_M.Visible
                           ||ctnProcessCandAud_M.Visible
 
-                          ||ctnProcessCODIDecision_M.Visible 
+                          ||ctnProcessCODIDecision_M_OBSOLETE.Visible 
                           ||ctnProcessPromRedact_M.Visible 
                           || ctnProcessPromEnvoi_M.Visible
                         Width: =400
@@ -2734,7 +2674,7 @@ Screens:
                                       ="Processus : Ajout de document"
                                     Underline: =true
                                     Width: =220
-                              - ctnCandAudSelect_M_1:
+                              - ctnAddDocSelect_M:
                                   Control: GroupContainer@1.5.0
                                   Variant: AutoLayout
                                   Properties:
@@ -2751,7 +2691,7 @@ Screens:
                                     Width: =414
                                     Y: =40
                                   Children:
-                                    - numAddDoc_M:
+                                    - numAddDocSelect_M:
                                         Control: ModernNumberInput@1.1.2
                                         Properties:
                                           Default: =1
@@ -2759,13 +2699,13 @@ Screens:
                                           LayoutMinHeight: =16
                                           LayoutMinWidth: =16
                                           Width: =0
-                                    - lblCandAudSelect_M_1:
+                                    - lblAddDocSelect_M:
                                         Control: ModernText@1.0.0
                                         Properties:
                                           BorderColor: =App.Theme.Colors.Darker10
                                           BorderStyle: =BorderStyle.Solid
                                           BorderThickness: =0
-                                          Color: =If(vcWizardStep= numAddDoc_M.Value ,  App.Theme.Colors.Darker10, Color.Silver)
+                                          Color: =If(vcWizardStep= numAddDocSelect_M.Value ,  App.Theme.Colors.Darker10, Color.Silver)
                                           FontWeight: =FontWeight.Bold
                                           LayoutMinHeight: =16
                                           LayoutMinWidth: =16
@@ -2773,7 +2713,7 @@ Screens:
                                           PaddingTop: =3
                                           Size: =14
                                           Text: |-
-                                            =With({MyStepNumber : numAddDoc_M.Value},
+                                            =With({MyStepNumber : numAddDocSelect_M.Value},
                                                 Switch(MyStepNumber,
                                                     1,_Emoji._1,
                                                     2,_Emoji._2,
@@ -2785,12 +2725,12 @@ Screens:
                                                 & If(vcWizardStep > MyStepNumber , "✅","")  
                                                 & " Sélectionner les demandes"
                                             )
-                                          Width: =Parent.Width - Parent.PaddingLeft - Parent.PaddingRight - btnCandAudSelect_M_1.Width
-                                    - btnCandAudSelect_M_1:
+                                          Width: =Parent.Width - Parent.PaddingLeft - Parent.PaddingRight - btnAddDocSelect_M.Width
+                                    - btnAddDocSelect_M:
                                         Control: ModernButton@1.0.0
                                         Properties:
                                           Align: =Align.Center
-                                          DisplayMode: =If(vcWizardStep = numAddDoc_M.Value ,DisplayMode.Edit, DisplayMode.Disabled)
+                                          DisplayMode: =If(vcWizardStep = numAddDocSelect_M.Value ,DisplayMode.Edit, DisplayMode.Disabled)
                                           FontWeight: =FontWeight.Bold
                                           Height: =24
                                           Italic: =true
@@ -2798,7 +2738,7 @@ Screens:
                                           OnSelect: |-
                                             =UpdateContext({vcDemandesEvent:"SetSelection"&Text(Rand())});
                                             UpdateContext({
-                                                vcWizardStep: numAddDoc_M.Value + 1 
+                                                vcWizardStep: numAddDocSelect_M.Value + 1 
                                             });
                                           PaddingBottom: =5
                                           PaddingLeft: =5
@@ -2879,62 +2819,31 @@ Screens:
                                           Height: =Self.AllItemsCount * (Self.TemplateHeight + 5)
                                           Items: |-
                                             =If(vcCurrentProcess = _ActionOnList.AddDoc,
-                                            // Retourne les catégories documentaires communes à toutes les demandes actuellement sélectionnées.
-                                            With(
-                                                {
-                                                    // Utilise la première demande comme point de départ pour établir la liste des catégories à vérifier.
-                                                    MyFirstDemande: First(dlstDemandes_M.SelectedItems),
-                                                    // Conserve la sélection afin de la réutiliser pendant la vérification de chaque catégorie.
-                                                    MySelectedDemande: dlstDemandes_M.SelectedItems
-                                                },
-                                                Filter(
-                                                    // Récupère les catégories documentaires configurées pour le statut de la première demande.
-                                                    LookUp(_StatutConfigTable, StatusKey = MyFirstDemande.StatusKey).DocumentCategory 
-                                                        As MyCurrentCategory,
-                                                    // Compte les demandes dont la configuration contient également la catégorie en cours d'évaluation.
-                                                    CountIf(
-                                                        MySelectedDemande As MyCurrentDemande,
-                                                        !IsBlank(
-                                                            LookUp(
-                                                                LookUp(_StatutConfigTable,StatusKey = MyCurrentDemande.StatusKey).DocumentCategory,
-                                                                Value = MyCurrentCategory.Value
+                                                // Retourne les catégories documentaires communes à toutes les demandes actuellement sélectionnées.
+                                                With(
+                                                    {
+                                                        // Utilise la première demande comme point de départ pour établir la liste des catégories à vérifier.
+                                                        MyFirstDemande: First(dlstDemandes_M.SelectedItems),
+                                                        // Conserve la sélection afin de la réutiliser pendant la vérification de chaque catégorie.
+                                                        MySelectedDemande: dlstDemandes_M.SelectedItems
+                                                    },
+                                                    Filter(
+                                                        // Récupère les catégories documentaires configurées pour le statut de la première demande.
+                                                        LookUp(_StatutConfigTable, StatusKey = MyFirstDemande.StatusKey).DocumentCategory 
+                                                            As MyCurrentCategory,
+                                                        // Compte les demandes dont la configuration contient également la catégorie en cours d'évaluation.
+                                                        CountIf(
+                                                            MySelectedDemande As MyCurrentDemande,
+                                                            !IsBlank(
+                                                                LookUp(
+                                                                    LookUp(_StatutConfigTable,StatusKey = MyCurrentDemande.StatusKey).DocumentCategory,
+                                                                    Value = MyCurrentCategory.Value
+                                                                )
                                                             )
-                                                        )
-                                                    ) = CountRows(MySelectedDemande)
+                                                        ) = CountRows(MySelectedDemande)
+                                                    )
                                                 )
                                             )
-                                            )
-
-                                            /*
-                                            // Construit la liste des catégories de documents requises
-                                            // pour l'ensemble des demandes actuellement sélectionnées.
-                                            //
-                                            // Étapes :
-                                            // 1. Pour chaque demande sélectionnée,
-                                            //    récupère la configuration correspondant à son statut.
-                                            // 2. Extrait la table DocumentCategory définissant les catégories
-                                            //    de documents attendues pour ce statut.
-                                            // 3. Fusionne toutes les tables DocumentCategory obtenues
-                                            //    en une seule table à l'aide de Ungroup.
-                                            // 4. Retourne uniquement les valeurs uniques afin d'éviter
-                                            //    les doublons lorsqu'une même catégorie est requise
-                                            //    par plusieurs demandes sélectionnées.
-                                            Filter( Distinct(
-                                                Ungroup(
-                                                    ForAll(
-                                                        dlstDemandes_M.SelectedItems;
-                                                        LookUp(
-                                                            _StatutConfigTable;
-                                                            StatutDetail = Statut_x0020_Detail
-                                                        ).DocumentCategory
-                                                    );
-                                                    Value
-                                                );
-                                                Value
-                                            );
-                                            Value <> Blank()
-                                            )
-                                            */
                                           LayoutMaxWidth: =Parent.Width
                                           LayoutMinHeight: =40
                                           TemplatePadding: =3
@@ -3587,7 +3496,7 @@ Screens:
                                           Text: =If(vcWizardStep = numPromRedacExit_M.Value, "Terminer","Annuler")
                                           VerticalAlign: =VerticalAlign.Middle
                                           Width: =Parent.Width - Parent.PaddingLeft - Parent.PaddingRight
-                        - ctnProcessCODIDecision_M:
+                        - ctnProcessCODIDecision_M_OBSOLETE:
                             Control: GroupContainer@1.5.0
                             Variant: AutoLayout
                             Properties:
@@ -3598,7 +3507,7 @@ Screens:
                               PaddingLeft: =8
                               PaddingRight: =8
                               PaddingTop: =8
-                              Visible: =vcCurrentProcess = _ActionOnList.CODIDecision
+                              Visible: =vcCurrentProcess = _ActionOnList.CODIDecision && false // [OBSOLETE] Passage en traitement individuel JIRA-PJ03444-110
                               Width: =Parent.Width
                               X: =(Parent.Width - Self.Width ) /2
                             Children:
@@ -4596,6 +4505,7 @@ Screens:
                                                   // Déclenche l'application effective de la sélection via un timer.
                                                   // Un délai est nécessaire car la mise à jour immédiate de la sélection
                                                   // n'est pas correctement prise en compte par le contrôle de liste.
+                                                  UpdateContext({vcTimerApplyChoiceAppel:false});
                                                   UpdateContext({vcTimerApplyChoiceAppel:true});
                                                 Size: =12
                                                 Text: =ThisItem.Value
@@ -4768,15 +4678,16 @@ Screens:
                                                                         // To  : DFA distribution list
                                                                         MyRecipients,
                                                                         // Subject: notification métier                   
-                                                                        _txtMessage.EmailSubjectPrefix 
+                                                                        _txtMessage.EmailSubjectPrefix & " " & MyTypeDemande & ": "
                                                                             &   "Appel à candidature a été lancé (Clôture: " & dteCandAppelidDate_M.SelectedDate & ")" ,     
                                                                         // Body: informations détaillées
                                                                         "<p>Bonjour,</p> " 
-                                                                            & "<i>[" & MyTypeDemande & "]: </i> L'appel à candidature a été lancé pour les emplois suivants et se clôturera le <b>" & dteCandAppelidDate_M.SelectedDate & "</b>."
+                                                                            & "<p><i><u>Concerne : [" & MyTypeDemande & "]: </i></u></p>"
+                                                                            & "<p>L'appel à candidature a été lancé pour les emplois suivants et se clôturera le <b>" & dteCandAppelidDate_M.SelectedDate & "</b>.</p>"
                                                                             & "<p><strong>" & MySpw &"</strong></p>
                                                                                 <ul>"
                                                                                 & MyListeEmploiHtml
-                                                                                &"</ul>"                                      
+                                                                                &"</ul></p>"                                      
                                                                         & _txtMessage.EmailFooter 
                                                                     );
                                                                 )
@@ -7038,7 +6949,7 @@ Screens:
                                                                         // To  : distribution list
                                                                         MyRecipients,
                                                                         // Subject: notification métier                   
-                                                                        _txtMessage.EmailSubjectPrefix 
+                                                                        _txtMessage.EmailSubjectPrefix
                                                                             &   MyTypeDemande & " : Nouvelle(s) demande(s) envoyée(s) (" & MySpw & ")",    
                                                                         // Body: informations détaillées
                                                                         "<p>Bonjour,</p> " 
@@ -7175,36 +7086,7 @@ Screens:
                                 )
                               Height: =Parent.Height - 40 - tabTypeDemande_M.Height
                               InputEvent: =vcDemandesEvent
-                              Items: |+
-                                =SortByColumns(
-                                    Filter(
-                                        Switch(vcApplyFilter ,
-                                            _FilterToApply._DynamicOnStatus,
-                                                Filter(
-                                                    ccDemandes,
-                                                    StatusKey in vcApplyFilterOnStatus_V3.StatusKey
-                                                ), 
-                                            //default
-                                                Filter(
-                                                    ccDemandes,
-                                                    And(
-                                                        Or(drpFilterSPW_M.Selected.SPW = Blank() , SPW = Coalesce(vvUserSPW, drpFilterSPW_M.Selected.SPW)), // Coalesce est utilisé pour le cas de la DFA
-                                                        Or(drpFilterStatus.Selected.Id = Blank(), Statut_x0020_Detail = drpFilterStatus.Selected.Value),
-                                                        Or(IsBlank(txtFilterLot.Text), StartsWith(Lot, txtFilterLot.Text)),
-                                                        Or(IsBlank(txtFilterRang.Text), StartsWith(Rang, txtFilterRang.Text)),
-                                                        Or(!tglMesActions_M.Checked, StartsWith(ActionOwner, "⚡"))
-                                                    )
-                                                )
-                                        ),
-                                        If(IsEmpty( ccValidForTypeDemande ), true, TypeDemandeValue in ccValidForTypeDemande),
-                                        If(tabTypeDemande_M.Selected.Type = _TypeDemande.All, true, TypeDemandeValue =  tabTypeDemande_M.Selected.Type)
-                                    ),
-                                    //"ActionOwner";SortOrder.Ascending; //[DEPRECATED] Les Demandes où une action est requise ets toujours en premier
-                                    If(vcDefaultSort,"StatutSortOrder"),SortOrder.Descending,
-                                    //Tri par défaut dans vcDemandesSortField : "Modified" et vcDemandesSortOrder : SortOrder.Descending
-                                    vcDemandesSortField, 
-                                    If(vcDemandesSortOrder,SortOrder.Ascending,SortOrder.Descending)
-                                )
+                              Items: "=SortByColumns(\r\n    Filter(\r\n        Switch(vcApplyFilter ,\r\n            _FilterToApply._DynamicOnStatus,\r\n                Filter(\r\n                    ccDemandes,\r\n                    StatusKey in vcApplyFilterOnStatus_V3.StatusKey\r\n                ), \r\n            //default\r\n                Filter(\r\n                    ccDemandes,\r\n                    And(\r\n                        Or(drpFilterSPW_M.Selected.SPW = Blank() , SPW = Coalesce(vvUserSPW, drpFilterSPW_M.Selected.SPW)), // Coalesce est utilisé pour le cas de la DFA\r\n                        Or(drpFilterStatus.Selected.Id = Blank(), Statut_x0020_Detail = drpFilterStatus.Selected.Value),\r\n                        Or(IsBlank(txtFilterLot.Text), StartsWith(Lot, txtFilterLot.Text)),\r\n                        Or(IsBlank(txtFilterRang.Text), StartsWith(Rang, txtFilterRang.Text)),\r\n                        Or(!tglMesActions_M.Checked, StartsWith(ActionOwner, \"⚡\"))\r\n                    )\r\n                )\r\n        ),\r\n        If(IsEmpty( ccValidForTypeDemande ), true, TypeDemandeValue in ccValidForTypeDemande),\r\n        If(tabTypeDemande_M.Selected.Type = _TypeDemande.All, true, TypeDemandeValue =  tabTypeDemande_M.Selected.Type)\r\n    ),\r\n    //\"ActionOwner\";SortOrder.Ascending; //[DEPRECATED] Les Demandes où une action est requise est toujours en premier\r\n    If(vcDefaultSort,\r\n        \"StatutSortOrder\"),\r\n    SortOrder.Descending,\r\n    \r\n    //Tri par défaut dans vcDemandesSortField : \"Modified\" et vcDemandesSortOrder : SortOrder.Descending\r\n    vcDemandesSortField, \r\n    If(vcDemandesSortOrder,SortOrder.Ascending,SortOrder.Descending)\r\n)\r\n"
                               OnChange: |+
                                 =//EventName	Output Event when OnChange is triggered. Possible values - Sort, CellAction, OnRowSelectionChange
                                 //EventColumn	Output Event column field name used when CellAction is invoked
@@ -7240,13 +7122,13 @@ Screens:
                                             UpdateContext({vcWizardStep : Max( numCODIdecisionSelectSingle_M.Value + 1,vcWizardStep)})
                                         );
                                         If(vcCurrentProcess in [_ActionOnList.GWDecision] && !IsBlank( radGWdecisionChoice_M.Selected.Value)
-                                                && (_MuliSelectOnDemandes || _SingleSelectOnDemandes),
+                                                && (_MultiSelectOnDemandes || _SingleSelectOnDemandes),
                                         UpdateContext({vcWizardStep : Max( 3,vcWizardStep)})
                                         );
                                         //Approche générique
                                         If(
                                             vcWizardStepAfterSelect  
-                                                && (_MuliSelectOnDemandes || _SingleSelectOnDemandes),
+                                                && (_MultiSelectOnDemandes || _SingleSelectOnDemandes),
                                             UpdateContext({vcWizardStep : Max(vcWizardStepAfterSelect,vcWizardStep)})
                                         );
                                 )
@@ -7285,14 +7167,14 @@ Screens:
                                 *****************************/
                                 If(
                                     And(
-                                        vcCurrentProcess in [_ActionOnList.CostraDecision, _ActionOnList.GWDecision,_ActionOnList.DirCandid, _ActionOnList.CODIDecision,_ActionOnList.PromEnvoi] ,
+                                        vcCurrentProcess in [_ActionOnList.SendDAC, _ActionOnList.CostraDecision, _ActionOnList.GWDecision,_ActionOnList.DirCandid, _ActionOnList.PromEnvoi] ,
                                         (vcWizardStep = 1 || vcWizardCompleted)
                                     ),
                                     'PowerCAT.FluentDetailsList.SelectionType'.None,
                                     If(
                                         Or(
                                             vcCurrentProcess = _ActionOnList.CostraDecision && radCOSTRAdecisionChoice_M.Selected.Value = _Decision.Refusée,
-                                            vcCurrentProcess = _ActionOnList.CODIDecision && vcWizardStep >= numCODIdecisionSelectSingle_M.Value,
+                                            //[OBSOLETE]vcCurrentProcess = _ActionOnList.CODIDecision && vcWizardStep >= numCODIdecisionSelectSingle_M.Value;
                                             vcCurrentProcess = _ActionOnList.GWDecision && vcWizardStep >= numGwDecisionChoice_M.Value 
                                         ),
                                         'PowerCAT.FluentDetailsList.SelectionType'.Single,
@@ -7593,13 +7475,13 @@ Screens:
                               Height: =20
                               Icon: ="Dismiss"
                               Italic: =true
-                              Layout: =If(_MuliSelectOnDemandes|| _SingleSelectOnDemandes ,   ButtonLayout.IconBefore, ButtonLayout.TextOnly)
+                              Layout: =If(_MultiSelectOnDemandes|| _SingleSelectOnDemandes ,   ButtonLayout.IconBefore, ButtonLayout.TextOnly)
                               LayoutMinHeight: =16
                               LayoutMinWidth: =16
                               OnSelect: =UpdateContext({vcDemandesEvent:"ClearSelection"&Text(Rand())})
                               PaddingLeft: =0
                               Size: =12
-                              Text: =Text(CountRows(dlstDemandes_M.SelectedItems)+0) & " sélectionnée" &  If(_MuliSelectOnDemandes, "(s)")
+                              Text: =Text(CountRows(dlstDemandes_M.SelectedItems)+0) & " sélectionnée" &  If(_MultiSelectOnDemandes, "(s)")
                               Width: =125
                         - btnGridClearSelect_M_1:
                             Control: ModernButton@1.0.0
@@ -7610,7 +7492,7 @@ Screens:
                               Height: =20
                               Icon: =
                               Italic: =true
-                              Layout: =If(_MuliSelectOnDemandes || _SingleSelectOnDemandes ,   ButtonLayout.IconBefore, ButtonLayout.TextOnly)
+                              Layout: =If(_MultiSelectOnDemandes || _SingleSelectOnDemandes ,   ButtonLayout.IconBefore, ButtonLayout.TextOnly)
                               LayoutMinHeight: =16
                               LayoutMinWidth: =16
                               OnSelect: |-
@@ -8026,3 +7908,40 @@ Screens:
             Width: =40
             X: '=Parent.Width - Self.Width '
             Y: =Parent.Height - Self.Height
+      - ctnSpinner_M:
+          Control: GroupContainer@1.5.0
+          Variant: ManualLayout
+          Properties:
+            Fill: =ColorValue("#80808080")
+            Height: =Parent.Height - Self.Y - ctnMainContainer_M.PaddingBottom
+            Visible: |
+              =cmpActBtnNew_M.IsLoading
+              || cmpActBtnFinaliser_M.IsLoading
+              || cmpActBtnSendDAC_M.IsLoading
+              || cmpActBtnDACValid_M.IsLoading
+              || cmpActBtnCreateLot_M.IsLoading
+              || cmpActBtnDirCandid_M.IsLoading
+              || cmpActBtnDirAudit_M.IsLoading
+              || cmpActBtnSendGW.IsLoading
+              || cmpActBtnCOSTRADecision_M.IsLoading
+              || cmpActBtnGWDecision_M.IsLoading
+              || cmpActBtnCandAppel_M.IsLoading
+              || cmpActBtnCandSendDFA_M.IsLoading
+              || cmpActBtnCandAud_M.IsLoading
+              || cmpActBtnCODIDecision_M.IsLoading
+              || cmpActBtnPromRedac_M.IsLoading
+              || cmpActBtnPromEnvoi_M.IsLoading
+              || cmpActBtnPromGWRecept_M.IsLoading
+            Width: =Parent.Width - 2 * Self.X
+            X: =ctnMainContainer_M.X + ctnMainContainer_M.PaddingLeft
+            Y: =ctnMainContainer_M.Y + ctnMainContainer_M.PaddingTop + ctnHeaderContainer_M.Height + ctnMainContainer_M.LayoutGap
+          Children:
+            - Spinner1_1:
+                Control: ModernSpinner@1.1.0
+                Properties:
+                  Height: =200
+                  Label: ="Traitement en cours"
+                  LabelPosition: =SpinnerLabelPosition.Below
+                  Width: '=300 '
+                  X: =(Parent.Width - Self.Width)/2
+                  Y: =(Parent.Height - Self.Height) / 2
