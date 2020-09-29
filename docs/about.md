
# Get Started  
Dokumentation für die FFT Aufgaben

## Matrixansicht erntfernen:


Matrixansicht erntfernen:

 In `/var/www/html/ilias/Customizing/global/plugins/Services/UIComponent/UserInterfaceHook/SrLpReport/src/Report/Matrix `

Die Datei **class.MatrixReportGUI.php** bearbeiten: Zeile 25  **const TAB_ID = "trac_matrix";**   auskommentieren.



## Email Benachrichtigung wenn Vorgesetzter einen Mitarbeiter in einen Kurs eingetragen hat:

Dazu benutzt man schon die bereits vorhandene "Vorlage" die das Modul "Course" bietet:

die Klasse: **class.ilCourseMembershipMailNotification.php** in `/var/www/html/ilias/Modules/Course/classes`

Die Mail wird versendet, wenn der Vorgesetzte auf den Button "einschreiben" bzw. "Abmelden" klickt unter
dem Verzeichnis in Ilias: *Persönlicher Schreibtisch -> Mitarbeiter-> Teilnehmerverwaltung*.
Die dazu vorhandene Klasse im Plugin heißt:

**class.CourseAdministrationStaffGUI.php** in `/var/www/html/ilias/Customizing/global/plugins/Services/UIComponent/UserInterfaceHook/SrLpReport/src/Staff/CourseAdministration`

###  Externe Klasse einbetten (Exkurs)
1.  In dieser Klasse muss man nun die externe Klasse: **class.ilCourseMembershipMailNotification.php** einbetten.
Das funktioniert wie folgt:
Fast jedes Plugin besitzt eine **autoload_classmap.php** Datei in `{$Plugin_DIR}/vendor/composer`.
Dort wird folgendes ins Array, dazu eingetragen:
`
'ilCourseMembershipMailNotification' => '/var/www/html/ilias/Modules/Course/classes/class.ilCourseMembershipMailNotification.php',
`
Das referenziert auf die Klasse die wir benötigen und wir können es nun in jeder Plugin Klasse verwenden, indem wir
`use ilCourseMembershipMailNotification` eingeben!

2. Einbetten der Klasse in class.CourseAdministration.StaffGUI.php (siehe oben)

### Mail senden
1. In die Funktion **enroll()**

         $mail = new ilCourseMembershipMailNotification();
         $mail->setType(ilCourseMembershipMailNotification::TYPE_ADMISSION_MEMBER);
         $mail ->setObjId($crs_obj_id);
         $mail->setRecipients(array($usr_id));
         $mail->send();

einfügen, um die Benachrichtigung zu bekommen, dass der User in ein Kurs eingetragen wurde

2. In die Funktion signout()

         $mail = new ilCourseMembershipMailNotification();
         $mail->setType(ilCourseMembershipMailNotification::TYPE_DISMISS_MEMBER);
         $mail ->setObjId($crs_obj_id);
         $mail->setRecipients(array($usr_id));
         $mail->send();

einfügen, um die Benachrichtigung zu bekommen, dass der User von einem Kurs ausgetragen wurde.
