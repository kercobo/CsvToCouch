

Script To upload OpenSRP data from csv File to Couchdb:
========================================
this script is to upload data from csv file into couchdb, you need to provide the form_definition.json(you can get this from OpenSRP client assets folder) and csv file that have same columns name as variable names in the form_definition.json variable name so this data can be read by the OpenSRP client 
Ex : in form_definition 
	{
        "name": "unique_id",
        "bind": "/model/instance/kartu_ibu/unique_id",
        "shouldLoadValue" : true
      	},

then the columns name in csv file should be "unique_id"


How To Use:
=================================

* OpenSRP server should be running

* edit your BASE_URL into your OpenSRP server

* edit your_user_here and your_password_here with your OpenSRP client user and password

* put the form_definition.json and csv file into one directory with this script

* rename your csv file name with the same name as the form name in your OpenSRP Client form_name (ex : in your assest folder is kartu_ibu_registration then your file name should be kartu_ibu_registration.csv)

* run this script from terminal python ppl_fix.py -c csv_file_name -j json_file_name

