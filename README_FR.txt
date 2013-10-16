Version 20.09.2013 


/* pour plus d'information sur les modifications lisez mod/via/version.php */


Proc�dure d'une NOUVELLE installation pour Moodle de 2.0 � 2.5 :
****************************************************************


1 - Copier les fichiers du module dans votre Moodle 2.x
	- Copier le dossier avec tout son contenu "mod / via" dans le r�pertoire "MoodleSite / mod" de votre Moodle
	- Copier le dossier 'UApi' � la racine du site - ceci permet acc�s aux activit�s sur tablette, mais n�cessite de la version de via 5.5 et que l'application Mobile de Via soit t�l�charg� sur le mobile ou la tablette.

	* Optionnel 
		- Copier le dossier et son contenu "blocks / via" dans le r�pertoire "MoodleSite / blocks" de votre Moodle
			:  ce bloc affiche un lien rapide vers les enregistrements faits dans via pr�sente dans les activit�s du cours

		- copier le dossier et son contenu "blocks / via_permanent" dans le r�pertoire "MoodleSite / blocks" de votre Moodle 
			:  ce bloc affiche un lien rapide vers les activit�s permanentes pr�sentes dans le cours


2 - Cliquer sur le lien "Notifications" dans le menu "Administration"
	- Installer le module "Via - Enseignement virtuel"

	* Optionnel 
		- Installer le bloc "Via"
		- installer le bloc "Via Activit�s permanentes"


3 - Sous 'Administration du site-> Plugins-> Modules d'activit�-> Via - Enseignement virtuel' entrez les informations de configuration pour l'API, 
    ceux-ci consistent de:
	- URL de l'API de Via
	- Cl� Via (CieID)
	- Cl� API (ApiID)
	- tester la connexion

	- Cl� Moodle * ceci est nouveau, si vous n'avez pas encore re�u cette cl� contacter SVIesolutions
	- tester la cl�
     Dans la page de param�tres du plug-in vous pouvez configurer le plug in pour qu'il r�pondre � vos besoins
	- Vous voulez pouvoir ajouter les activit�s aux cat�gories que vous avez cr��es dans Via? Vous n'avez qu'� cochez la casse de de configurer les cat�gories
	- Vous voulez envoyer les invitations et rappels � partir de moodle (fortement sugg�r�) ou par Via?
	- Vous voulez que les participants confirment leur pr�sence?
	- vous voulez pouvoir envoyer des courriels personnalis�s
	- vous voulez que les informations des participants soient synchronis�es avec leurs informations sur Via?


    

Proc�dure de MISE � JOUR pour Moodle de 2.0 � 2.5 :
****************************************************************

1 - Copier les fichiers du module dans votre Moodle 2.x
	- Copier le dossier avec tout son contenu "mod / via" dans le r�pertoire "MoodleSite / mod" de votre Moodle
	- Copier le dossier 'UApi' � la racine du site - ceci permet acc�s aux activit�s sur tablette, mais n�cessite de la version de via 5.2

	* Optionnel 
		- Copier le dossier et son contenu "blocks / via" dans le r�pertoire "MoodleSite / blocks" de votre Moodle
		- copier le dossier et son contenu "blocks / via_permanent" dans le r�pertoire "MoodleSite / blocks" de votre Moodle 


2 - ENLEVER le code suivant dans lib/enrollib.php 
    � la fonction 'public function enrol_user()' 
    avant 'if ($userid == $USER->id)'
    dans moodle 2.0 ceci donne vers la ligne +/- 1098
    dans moodle 2.4 ceci donne vers la ligne +/- 1317
	
	/********************************************/
			
	/*Added in order to update Via participants */
			
	require_once($CFG->dirroot.'/mod/via/lib.php');
			
	add_participant_via($userid,  $instance->courseid);
			
	/********************************************/


3 - ENELEVER le code suivant dans lib/enrollib.php
    � la fonction 'public function unenrol_user()'
    avant '$DB->delete_records('user_lastaccess', array('userid'=>$userid, 'courseid'=>$courseid));'
    dans moodle 2.0 ceci donne vers la ligne +/- 1220
    dans moodle 2.4 ceci donne vers la ligne +/- 1448 

	/**********************************************/
			
	/*Added in order to update Via participants */
			
	require_once("$CFG->dirroot/mod/via/lib.php");
			
	remove_participant_via($userid, $courseid);
			
	/**********************************************/



2 - Ajouter la Cl� moodle * ceci est nouveau de la version anteilleur, si vous n'avez pas encore re�u cette cl� contacter SVIesolutions.

4 - Faire rouler le cron de fa�on manuel en appelant la page site/admin/cron.php pour mettre les nouveaux champs dans la BD via_participants utile pour la synchronisation � jour. 
    Ceci peut prendre quelque temps, nous vous sugg�rons donc de faire la mise � jour en moment plus tranquille. 

4 - Vider toutes les caches


Les activit�s existantes ne seront pas affect�es par ce changement.




Proc�dure de mise � jour de l'URL pour Moodle de 2.0 � 2.5 :
****************************************************************

Dans une situation de nouvelle URL, Cl� Via et Cl� api, c'est mieux de d�sinstaller le plug-in compl�tement puis le r�installer. 

Si vous ne le d�sinstallez pas, il faut supprimer toutes les activit�s dans mdl_via, tous les utilisateurs dans mdl_via_participants et mdl_via_users. 

Ces informations sont li�es � l'ancienne URL et causeront des erreurs s�ils ne sont pas supprim�s.
