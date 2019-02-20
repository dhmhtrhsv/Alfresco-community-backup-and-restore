
# Restore process of Alfresco community website

## Restore process

 1. Create a new vm
 2. Install alfresco community
 3. Start only Postgres from the Alfresco Community Manager Tool
 4. Start cmd (Linux: Terminal)
 5. Type
	```
	cd C:\alfresco-community\postgresql\bin
	```
	or for Linux
	```
	cd /opt/alfresco-community/postgresql/bin/
	```
 6. Type
	```
	psql -U postgres
	```
	or for Linux
	```
	./psql
	```	
 7. Type 
	```
	/list
	```	
	to see the existing databases.
 8. Type the password from the database you have already (It is in the alfresco-global.properties file. See the backup process.)
 9. Type
	```
	drop database alfresco;
	```			
     If there is a mistake, press enter and then type again the command.
 10. Check if it is deleted. Type: 
		```
		\list
		```	
 11. Type
		```
		create database alfresco;
		```	
     If there is a mistake, press enter and then type again the command.
 12.  Check if it is created. Type: 
		```
		\list
		```	
 13. Type
		```
		\q
		```
 14. Unzip the content of the Backup process (i.e. solr4Backup folder, contentstore folder, pg_dump.sql) in a folder of your choice (i.e. C:\Users\dimitris\Downloads)
 15. Type editing your path appropriately:
		```
		psql -U alfresco -d alfresco -f C:\Users\dimitris\Downloads\AlfrescoBackup\pg_dump.sql
		```
		in order to restore the database.
 16. In the C:\alfresco-community\alf_data (Linux: /opt/alfresco-community/alf_data), replace:
		- the folder contentstore with the contentstore of the backup folder
		- the content of the folder \solr4\index\archive\SpacesStore\index with the content of the solr4Backup\archive\\"snapshotNumber"
		- the content of the folder \solr4\index\ workspace\SpacesStore\index with the content of the solr4Backup\workspace\\"snapshotNumber"

		The snapshotNumbers should be the same or the ones which are nearest to each other.
 17. Restart Alfresco


