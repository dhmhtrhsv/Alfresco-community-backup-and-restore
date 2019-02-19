
# Backup process of Alfresco community website with Cobian

## Initial process

 1. From the alfresco-global.properties (in folder C:\alfresco-community\tomcat\shared\classes) find the dir.root (e.g. C:/ALFRES~1/alf_data)
 2. If it doesn't exist, create a folder solr4Backup inside the dir.root (e.g. inside the directory C:\alfresco-community\alf_data)
 3. The folders: alfresco and archive are automatically copied in the solr4Backup folder. For futher information visit https://docs.alfresco.com/5.2/tasks/backup-hot.html
 4. If it doesn't exist, create a file with name pgpass.conf inside the C:\alfresco-community\postgresql\bin with the following format (replace the password with the db.password found in the alfresco-global.properties):
	```
	#hostname:port:database:username:password
	127.0.0.1:5432:alfresco:alfresco:12345
	```
 5. Create a folder inside the C with name AlfrescoBackup (i.e. C:\AlfrescoBackup)

## Cobian process
1.	Install Cobian from https://www.cobiansoft.com/
2.	Run Cobian
3.	Create a new task
4.	Select General:
    -	Give a task name, e.g. AlfrescoBackup
    -	Backup Type Full
5.	Select Files:
    - Source files-directories:  
	    - C:\alfresco-community\alf_data\solr4Backup
        - C:\AlfrescoBackup\pg_dump.sql
        - C:\alfresco-community\alf_data\contentstore
    - Destination files-directories:
        - C:\AlfrescoBackup
6.	Select Schedule:
    - Daily, Time 04:00:00 AM
7.	Select Dynamics:
    - 2 Full copies to keep
8.	Select Archive: 
    - Compression Type: Zip compression
9.	Select Events:
    - Pre-backup events
        - Execute C:\AlfrescoBackup\AlfrescoBackup.bat, that has the code: 		      
			```
			:: Set backup Folder
			set root_backup=C:\AlfrescoBackup\
			:: Set Alfresco Folder
			set alfresco_folder=C:\alfresco-community\
			:: Create db dump
			cd %alfresco_folder%postgresql\bin
			SET PGPASSFILE=%alfresco_folder%postgresql\bin\pgpass.conf
			pg_dump -h 127.0.0.1 -p 5432 -U alfresco -f %root_backup%pg_dump.sql
			```
10.	Select ok
