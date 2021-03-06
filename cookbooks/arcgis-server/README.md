arcgis-server cookbook
===============

This cookbook installs and configures ArcGIS Enterprise products, components, and tools.

Requirements
------------

### Supported ArcGIS software
* ArcGIS 10.4/10.4.1/10.5 Server
* ArcGIS 10.4/10.4.1/10.5 Data Store
* Portal for ArcGIS 10.4/10.4.1/10.5
* ArcGIS 10.4/10.4.1/10.5 Web Adaptor (IIS/Java) 

### Platforms
* Windows 7
* Windows 8 (8.1)
  - 8.1 requires .Net Framework 3.5 (See ms_dotnet cookbook README)
* Windows 10
  - requires .Net Framework 3.5 (See ms_dotnet cookbook README)
* Windows Server 2008 (R2)
* Windows Server 2012 (R2)
* Ubuntu 14.04 (when deploying ArcGIS Server on Amazon Web Services)
* Rhel 6.5, 7.0

### Dependencies
The following cookbooks are required:
* hostsfile
* limits
* authbind
* iptables
* windows
* ms_dotnet
* openssl

Attributes
----------
#### General
* `node['arcgis']['version']` = ArcGIS version. Default value is `10.5`.
* `node['arcgis']['run_as_user']` = Account used to run ArcGIS Server, Portal for ArcGIS, and ArcGIS Data Store. Default account name is `arcgis`.
* `node['arcgis']['run_as_password']` = Password for the account used to run ArcGIS Server, 
Portal for ArcGIS, and ArcGIS Data Store. Default value is`Pa$$w0rdPa$$w0rd`.
* `node['arcgis']['hosts']` = Hostname to IP address mappings to be added to /etc/hosts file. Default value is `{}`.
* `node['arcgis']['cache_authorization_files']` = If set to true, the software authorization file is cached on the machine and software authorization is skipped in the subsequent chef runs.
* `node['arcgis']['python']['install_dir']` = Python installation directory. By default Python is is installed at `C:\Python27`.

#### Server

* `node['arcgis']['server']['wa_name']` = The name of the Web Adaptor used for ArcGIS Server. Default name is `server`.
* `node['arcgis']['server']['wa_url']` = URL of the Web Adaptor used for ArcGIS Server. Default name is `https://<FQDN of the machine>/<Server Web Adaptor name>`.
* `node['arcgis']['server']['domain_name']` = ArcGIS Server domain name. By default, `<node FQDN>` is used.
* `node['arcgis']['server']['url']` = ArcGIS Server URL. Default URL is `https://<FQDN of the machine>:6443/arcgis`.
* `node['arcgis']['server']['private_url']` = ArcGIS Server URL without Web Adaptor. Default URL is `https://<server domain name>:6443/arcgis`.
* `node['arcgis']['server']['primary_server_url']` = URL of ArcGIS Server site to join. Default is `nil`.
* `node['arcgis']['server']['web_context_url']` = ArcGIS server web context URL. By default, this is `'https://<Domain name>/<WA name>`.
* `node['arcgis']['server']['use_join_site_tool']` = Is set to true, join-site command line tool is used to add machine to the site and the default cluster. Default value is `false`.
* `node['arcgis']['server']['admin_username']` = Primary ArcGIS Server administrator user name. Default user name is `admin`.
* `node['arcgis']['server']['admin_password']` = Primary ArcGIS Server administrator password. Default value is `changeit`.
* `node['arcgis']['server']['publisher_username']` = ArcGIS Server publisher user name. Default user name is `node['arcgis']['server']['admin_username']`.
* `node['arcgis']['server']['publisher_password']` = ArcGIS Server publisher password. Default value is `node['arcgis']['server']['admin_password']`.
* `node['arcgis']['server']['directories_root']` = ArcGIS Server root directory. Default Windows directory is `C:\arcgisserver`; default Linux directory is `/arcgis/server/usr`.
* `node['arcgis']['server']['setup']` = The location of the ArcGIS Server setup executable. Default location on Windows is `C:\Temp\ArcGISServer\Setup.exe`; default location on Linux is `/tmp/server-cd/Setup`.
* `node['arcgis']['server']['lp-setup']` = The location of language pack for ArcGIS Server. Default location is `nil`.
* `node['arcgis']['server']['install_dir']` = ArcGIS Server installation directory. By default, ArcGIS Server is installed to  `%ProgramW6432%\ArcGIS\Server` on Windows machines and `/` on Linux machines.
* `node['arcgis']['server']['is_hosting']` = Must be set to true for hosting server. Default value is `true`.
* `node['arcgis']['server']['install_system_requirements']` = If set to true, the required third party libraries are installed on the machine before running ArcGIS Server setup. Default value is `true`.
* `node['arcgis']['server']['configure_autostart']` = If set to true, on Linux the ArcGIS Server is configured to start with the operating system. Default value is `true`.
* `node['arcgis']['server']['authorization_file']` = ArcGIS Server authorization file path. Default location and authorization file is `C:\Temp\server_license.prvc` on Windows and `/tmp/server_license.prvc` on Linux.
* `node['arcgis']['server']['authorization_file_version']` = ArcGIS Server authorization file version. Default version is `node['arcgis']['version']`.
* `node['arcgis']['server']['managed_database']` = ArcGIS Server's managed database connection string. By default, this value is `nil`.
* `node['arcgis']['server']['replicated_database']` = ArcGIS Server's replicated geodatabase connection string. By default, this value is `nil`.
* `node['arcgis']['server']['keystore_file']` = Path to PKSC12 keystore file (.pfx) with SSL certificate for ArcGIS Server. Default value is `nil`.
* `node['arcgis']['server']['keystore_password']` = Keystore file password for ArcGIS Server. Default value is `nil`.
* `node['arcgis']['server']['cert_alias']` = SSL certificate alias for ArcGIS Server. Default alias is composed of these values: `node['arcgis']['server']['domain_name']`.
* `node['arcgis']['server']['system_properties']` = ArcGIS Server system properties. Default value is `{}`. 
* `node['arcgis']['server']['log_level']` = ArcGIS Server log level. Default value is `WARNING`.
* `node['arcgis']['server']['log_dir']` = ArcGIS Server log directory. Default value is `C:\arcgisserver\logs` on Windows and `/arcgis/server/usr/logs` on Linux.
* `node['arcgis']['server']['max_log_file_age']` = ArcGIS Server maximul log file age. Default value is 90.
* `node['arcgis']['server']['config_store_type']` = ArcGIS Server config store type. Default value is `FILESYSTEM`.
* `node['arcgis']['server']['config_store_connection_string']` = ArcGIS Server config store type. Default value is `C:\arcgisserver\config-store` on Windows and `/arcgis/server/usr/config-store` on Linux.
* `node['arcgis']['server']['config_store_connection_secret']` = ArcGIS Server config store type secret. Default value is `nil`.
* `node['arcgis']['server']['services']` = An array of ArcGIS Server services to be published. Default value is `{}`.

#### Web Adaptor

* `node['arcgis']['web_adaptor']['admin_access']` = Indicates if ArcGIS Server Manager and Admin API will be available through the Web Adaptor <true|false>. Default value is `false`.
* `node['arcgis']['web_adaptor']['setup']` = Location of ArcGIS Web Adaptor setup executable. Default location is `C:\Temp\WebAdaptorIIS\Setup.exe` on Windows and `/tmp/web-adaptor-cd/Setup` on Linux.
* `node['arcgis']['web_adaptor']['lp-setup']` = The location of language pack for ArcGIS Web Adaptor. Default location is `nil`.
* `node['arcgis']['web_adaptor']['install_dir']` = ArcGIS Web Adaptor installation directory (Linux only). By default, ArcGIS Web Adaptor is installed to `/` on Linux.

#### Portal

* `node['arcgis']['portal']['domain_name']` = Portal for ArcGIS domain name. By default, `<node FQDN>` is used.
* `node['arcgis']['portal']['wa_name']` = The Web Adaptor name for Portal for ArcGIS., Default name is `portal`.
* `node['arcgis']['portal']['wa_url']` = URL of the Web Adaptor for Portal for ArcGIS., Default name is `https://<FQDN of the machine>/<Portal Web Adaptor name>`.
* `node['arcgis']['portal']['url']` = Portal for ArcGIS URL. Default URL is `https://<FQDN of the machine>:7443/arcgis`.
* `node['arcgis']['portal']['private_url']` = Portal for ArcGIS private URL. Default URL is `https://<portal domain name>:7443/arcgis`.
* `node['arcgis']['portal']['web_context_url']` = Portal for ArcGIS web context URL. By default, this is `nil`.
* `node['arcgis']['portal']['primary_machine_url']` = URL of primary Portal for ArcGIS machine. By default, this is `nil`.
* `node['arcgis']['portal']['admin_username']` = Initial portal administrator user name. Default user name is `admin`.
* `node['arcgis']['portal']['admin_password']` = Initial portal administrator password. Default password is `changeit`.
* `node['arcgis']['portal']['admin_email']` = Initial portal administrator e-mail. Default email is `admin@mydomain.com`.
* `node['arcgis']['portal']['admin_full_name']` = Initial portal administrator full name. Default full name is `Administrator`.
* `node['arcgis']['portal']['admin_description']` = Initial portal administrator description. Default description is `Initial portal account administrator`.
* `node['arcgis']['portal']['security_question']` = Security question. Default question is `Your favorite ice cream flavor?`
* `node['arcgis']['portal']['security_question_answer']` = Security question answer. Default answer is `bacon`.
* `node['arcgis']['portal']['setup']` = Portal for ArcGIS setup path. Default location is `C:\Temp\ArcGISPortal\Setup.exe` on Windows and `/tmp/portal-cd/Setup` on Linux.
* `node['arcgis']['portal']['lp-setup']` = The location of language pack for Portal for ArcGIS. Default location is `nil`.
* `node['arcgis']['portal']['install_dir']` = Portal for ArcGIS installation directory. By default, Portal for ArcGIS is installed to `%ProgramW6432%\ArcGIS\Portal` on Windows machines and `/` on Linux machines.
* `node['arcgis']['portal']['install_system_requirements']` = If set to true, the required third party libraries are installed on the machine before running Portal for ArcGIS setup. Default value is `true`.
* `node['arcgis']['portal']['configure_autostart']` = If set to true, on Linux the Portal for ArcGIS is configured to start with the operating system. Default value is `true`.
* `node['arcgis']['portal']['content_dir']` = Portal for ArcGIS content directory. Default directory is `C:\arcgisportal\content` on Windows and `/arcgis/portal/usr/arcgisportal/content` on Linux.
* `node['arcgis']['portal']['authorization_file']` = Portal for ArcGIS authorization file path. Default location and file name is `C:\Temp\portal_license.prvc` on Windows and `/tmp/portal_license.prvc` on Linux.
* `node['arcgis']['portal']['authorization_file_version']` = Portal for ArcGIS authorization file version. Default value is `10.4`.
* `node['arcgis']['portal']['keystore_file']` = Path to PKSC12 keystore file (.pfx) with SSL certificate for Portal for ArcGIS. Default value is `nil`.
* `node['arcgis']['portal']['keystore_password']` = Keystore file password for Portal for ArcGIS. Default value is `nil`.
* `node['arcgis']['portal']['cert_alias']` = SSL certificate alias for Portal for ArcGIS. Default alias is composed of these values:`node['arcgis']['portal']['domain_name']`.
* `node['arcgis']['portal']['preferredidentifier']` = Portal for ArcGIS preferred identifier method <hostname|ip>. Default method used is `hostname`.
* `node['arcgis']['portal']['content_store_type']` = Portal for ArcGIS content store type <FileStore|CloudStore>. Default value is `FileStore`.
* `node['arcgis']['portal']['content_store_provider']` = Portal for ArcGIS content store provider <Amazon|FileSystem>. Default value is `FileSystem`.
* `node['arcgis']['portal']['content_store_connection_string']` = Portal for ArcGIS content store connection string. Default connection string is `node['arcgis']['portal']['content_dir']`.
* `node['arcgis']['portal']['object_store']` = Portal for ArcGIS content store container such as S3 bucket name. Default value is `nil`.

#### Data Store

* `node['arcgis']['data_store']['data_dir']` = ArcGIS Data Store data directory. Default location is `C:\arcgisdatastore\data` on Windows and `/arcgis/datastore/usr/arcgisdatastore` on Linux.
* `node['arcgis']['data_store']['backup_dir']` = ArcGIS Data Store backup directory. Default location is `C:\arcgisdatastore\data\backup` on Windows and `/arcgis/datastore/usr/arcgisdatastore/backup` on Linux.
* `node['arcgis']['data_store']['setup']` = Location of ArcGIS Data Store setup executable. Default location is `C:\Temp\ArcGISDataStore\Setup.exe` on Windows and `/tmp/tmp/data-store-cd/Setup` on Linux.
* `node['arcgis']['data_store']['lp-setup']` = The location of language pack for ArcGIS Data Store. Default location is `nil`.
* `node['arcgis']['data_store']['install_dir']` = ArcGIS Data Store installation directory. By default, ArcGIS Data Store is installed to `%ProgramW6432%\ArcGIS\DataStore` on Windows and `/` on Linux.
* `node['arcgis']['data_store']['install_system_requirements']` = If set to true, the required third party libraries are installed on the machine before running ArcGIS Data Store setup. Default value is `true`.
* `node['arcgis']['data_store']['configure_autostart']` = If set to true, on Linux the ArcGIS Data Store is configured to start with the operating system. Default value is `true`.
* `node['arcgis']['data_store']['preferredidentifier']` = ArcGIS Data Store preferred identifier method <hostname|ip>. Default method used is `hostname`.
* `node['arcgis']['data_store']['types']` = Comma-separated list of ArcGIS Data Store types to be created, <relational|tileCache|spatiotemporal>. By default, `tileCache,relational` is used.

#### GeoAnalytics

* `node['arcgis']['geoanalytics']['authorization_file']` = File path to the license file for authorizing GeoAnalytics

#### IIS

* `node['arcgis']['iis']['domain_name']` = Domain name used for generating self-signed SSL certificate. By default, `<node FQDN>` is used.
* `node['arcgis']['iis']['keystore_file']` = Path to PKSC12 keystore file (.pfx) with server SSL certificate for IIS. Default value is `nil`.
* `node['arcgis']['iis']['keystore_password']` = Password for keystore file with server SSL certificate for IIS. Default value is `nil`.
* `node['arcgis']['iis']['web_site']` = IIS web site to configure. Dafault value is `Default Web Site`.
* `node['arcgis']['iis']['replace_https_binding']` = If false, the current HTTPS binding is not changed if it is already configured. Default value is `false`.
* `node['arcgis']['iis']['features']` = An array of windows features to be installed with IIS. Default value depends on Windows version.

#### RDS
* `node['arcgis']['rds']['engine']` = RDS DB engine <nil|postgres|sqlserver-se>. Default DB engine is `nil`.
* `node['arcgis']['rds']['endpoint']` = RDS DB instance endpoint. Default endpoint is `nil`.
* `node['arcgis']['rds']['username']` = RDS DB instance master username. Default username is `nil`.
* `node['arcgis']['rds']['password']` = RDS DB instance master user password. Default password is `nil`.

#### Linux Web Server
* `node['arcgis']['web_server']['webapp_dir']` = Path to web server's web application directory (eg. /opt/tomcat/webapps). Default value is `''`


Recipes
-------
### arcgis-server::authbind (DEPRECATED)
Configures authbind for Apache Tomcat user (for Debian Linux family).

### arcgis-server::clean
Deletes local directories created by ArcGIS Server, Portal for ArcGIS, and ArcGIS Data Store.

### arcgis-server::datastore
Installs and configures ArcGIS Data Store on primary machine.

### arcgis-server::datastore_standby
Installs and configures ArcGIS Data Store on standby machine.

### arcgis-server::disable_geoanalytics
Resets the big data analytics configuration on the Portal for ArcGIS.

### arcgis-server::egdb
Registers enterprise geodatabases with ArcGIS Server.

### arcgis-server::enable_geoanalytics
Configures the Portal for ArcGIS to perform big data analytics.

### arcgis-server::federation
Registers and federates ArcGIS Server with Portal for ArcGIS.

### arcgis-server::fileserver
Creates directories on file server.

### arcgis-server::hosts
Creates entries in /etc/hosts file for the specified hostname to IP address map.

### arcgis-server::iis
Enables IIS, installs features required by ArcGIS Web Adaptor IIS, configures HTTPS, and starts IIS.

### arcgis-server::iptables
Configures iptables to forward ports 80/443 to 8080/8443.

### arcgis-server::lp-install
Installs language packs for ArcGIS for Server software.

### arcgis-server::portal
Installs and configures Portal for ArcGIS.

### arcgis-server::portal_standby
Installs and configures Portal for ArcGIS on standby machine

### arcgis-server::portal_wa
Installs ArcGIS Web Adaptor and configures it with Portal for ArcGIS. IIS or Java application server such as Tomcat must be installed and configured before installing ArcGIS Web Adaptor.

### arcgis-server::rds_egdb 
Creates EGDBs in Amazon AWS RDS instance and registers the EGDBs with ArcGIS Server.

### arcgis-server::server
Installs and configures ArcGIS Server site.

### arcgis-server::server_node
Installs ArcGIS Server on the machine and joins an existing site.

### arcgis-server::server_wa
Installs ArcGIS Web Adaptor and configures it with ArcGIS Server. IIS or Java application server such as Tomcat must be installed and configured before installing ArcGIS Web Adaptor.

### arcgis-server::services
Publishes services to ArcGIS Server.

### arcgis-server::sql_alias 
Creates 'egdbhost' alias for SQL Server RDS instance endpoint domain name.

### arcgis-server::system
Configures system requirements for web GIS software by invoking ':system' actions for ArcGIS Server, ArcGIS Data Store, Portal for ArcGIS, and ArcGIS Web Adaptor resources, includes arcgis::hosts recipe.

### arcgis-server::webgis_installed
Installs ArcGIS Server, Portal for ArcGIS, ArcGIS Data Store, and ArcGIS Web Adaptors for server and portal.

### arcgis-server::webgis_uninstalled
Uninstalls ArcGIS Server, Portal for ArcGIS, ArcGIS Data Store, and ArcGIS Web Adaptors for server and portal.

### arcgis-server::webgis_validate
Checks if ArcGIS for Server setups and authorization files exist.


Usage
-----
See [wiki](https://github.com/Esri/arcgis-cookbook/wiki) pages for more information about using ArcGIS cookbooks.

## Issues

Find a bug or want to request a new feature?  Please let us know by submitting an [issue](https://github.com/Esri/arcgis-cookbook/issues).

## Contributing

Esri welcomes contributions from anyone and everyone. Please see our [guidelines for contributing](https://github.com/esri/contributing).

Licensing
---------

Copyright 2016 Esri

Licensed under the Apache License, Version 2.0 (the "License");
You may not use this file except in compliance with the License.
You may obtain a copy of the License at
   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

A copy of the license is available in the repository's [License.txt](https://github.com/Esri/arcgis-cookbook/blob/master/License.txt?raw=true) file.

[](Esri Tags: ArcGIS Server Chef Cookbook)
[](Esri Language: Ruby)
