Tracks Flow file storage
========================

Structure
---------

* client - C# client library for file storage
* server - file storage server on php language
* setup - nginx example config

Dependencies
------------

* nginx
* php-fpm

How to use
----------

�������� ����� ���������� �������� ������ (�������) � �� ������ POST-������� �� ���� ����. �������� ���������� ���������������. ��� ��������� ���������� ������� ������ ���������� ���������� ����� � �������������� ����. ��� �������� ����������� �� ������� � ����� �������� �����.
��� ����� �������� �� �������� ������� � ��������� ���������:

`/<��������_����>/<scope>/<������_���_�������_id>/<������_���_�������_id>/<�����-���������_������_id>/<filename.ext>`

��� �������� ����� � ������� ���������� ��������� ��������� �����, ������� ����� ���������.
���������� ������ ���������� ����� �������� � Nginx ��������� X-ACCEL_REDIRECT � ����� � �����. ��������� ���������� ��� Nginx.

How to set up
-------------

* ������ Nginx + php-fpm.
* � nginx ������� ������ ��� ����� ������� �������� �������� ���������� ����������:

    	server {
    	listen		<your_LAN_ip>;
    	server_name		<you_file_storage_hostname>;
    
    	location /<location_name> {
    		internal;
    		root		<storage_root_path_should_be_the_same_in_lib.php>;
    	}
    
    	location ~ \.php {
    		root		/<path_to_php_code>;
    		fastcgi_pass	127.0.0.1:9000;
    		fastcgi_index	index.php;
    		fastcgi_param	SCRIPT_FILENAME /<path_to_php_code>/$fastcgi_script_name;
    		include		fastcgi_params;
    	}
     	}

* �������� ����� ������� � ���������� `<path_to_php_code>`
* ��������� lib.php � ������� getfsroot() ������ ���������� ����, ��������� � ������� � `<storage_root_path_should_be_the_same_in_lib.php>`; ������� `getlocroot()` ������ ��������� �� ��, ��� ������� � ������� � `<location_name>`.
* ������ ����� �� `<storage_root_path_should_be_the_same_in_lib.php>` �� ��, ��� � � Nginx / php-fpm, ���� ����� ������.
* ���� �� ����� ������������ ����� ������ 512�����, �� ���� ��������� � php-fpm, nginx � php.ini ��������������� �������� ��� ����������� �� ������.