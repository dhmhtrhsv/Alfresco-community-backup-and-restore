# Backup process of Alfresco community website

## Backup process

 1. From the alfresco-global.properties (in folder C:\alfresco-community\tomcat\shared\classes) find the dir.root (e.g. C:/ALFRES~1/alf_data)
 2. If it doesn't exist, create a folder solr4Backup inside the dir.root (e.g. inside the directory C:\alfresco-community\alf_data)
 3. The folders: alfresco and archive are automatically copied in the solr4Backup folder. By default, the cron job is run at 2 am for alfrescoCore and 4 am for archiveCore. For futher information visit https://docs.alfresco.com/5.2/tasks/backup-hot.html
 4. If it doesn't exist, create a file with name pgpass.conf inside the C:\alfresco-community\postgresql\bin with the following format (replace the password with the db.password found in the alfresco-global.properties):
	```
	#hostname:port:database:username:password
	127.0.0.1:5432:alfresco:alfresco:12345678
	```
 5. Create a folder inside the C with name AlfrescoBackup (i.e. C:\AlfrescoBackup) and a folder inside this folder withe name Backups (i.e. C:\AlfrescoBackup\Backups)
 6. Install 7-zip from https://www.7-zip.org/
 7. Create a .bat file with the following code and run it.
	 ```
	:: Set backup Folders
	set root_backup=C:\AlfrescoBackup\
	MKDIR %root_backup%temp
	set deflated_backup_folder=%root_backup%temp\
	set backup_folder=%root_backup%Backups\

	::
	::Change below alfresco-community to alfresco-community or alfresco5-community according to your version
	::
	set alfresco_folder=C:\alfresco-community\
	:: Set solr4Backup folder
	set solr4backup_folder=%alfresco_folder%alf_data\solr4Backup

	::Delete the ZIP file
	cd /d %backup_folder%
	for /F "delims=" %%i in ('dir /b') do (rmdir "%%i" /s/q || del "%%i" /s/q)

	:: Copy solr indexes
	xcopy /s /y %solr4backup_folder% %deflated_backup_folder%

	:: Create db dump
	cd %alfresco_folder%postgresql\bin
	SET PGPASSFILE=%alfresco_folder%postgresql\bin\pgpass.conf
	pg_dump -h 127.0.0.1 -p 5432 -U alfresco -f %deflated_backup_folder%pg_dump.sql

	:: Create contentstore folder and copy its content
	MKDIR %deflated_backup_folder%contentstore
	xcopy /s /y %alfresco_folder%alf_data\contentstore %deflated_backup_folder%contentstore
	cd %root_backup%
	:: Zip the backup folders
	cd C:\Program Files\7-Zip
	7z a %backup_folder%AlfrescoBackup.zip %deflated_backup_folder%*
	rmdir /s /q %deflated_backup_folder%
	```
