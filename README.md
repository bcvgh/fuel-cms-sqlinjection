Need to login to the background.

`fuel/modules/fuel/controllers/Blocks.php`

line 64  `import_view` method starts

Line 70 receives the id parameter of the post request and enters the `import` method

![image](https://github.com/bcvgh/fuel-cms-sqlinjection/assets/56790427/7338b901-b9d0-465e-9e31-e2520dad1a87)

fuel/modules/fuel/libraries/Fuel_blocks.php

Then enter the `find_by_key` method on line 307

![image](https://github.com/bcvgh/fuel-cms-sqlinjection/assets/56790427/3ec8b7ad-7117-4d1c-bf18-24243fed7c36)

Because the `find_by_key` method does not exist, enter the `__call `method of the current object


Enter line 4421 of MY_Model.php, pass parameters to `$this->db->where()` method

![image](https://github.com/bcvgh/fuel-cms-sqlinjection/assets/56790427/8400c273-d6ae-479a-a4e8-154eb907d316)

At this time, the external input string is spliced into the SQL statement through the `$this->db->where()` method, 
But at this time, the external input will be surrounded by single quotes because of codeigniter's safe processing of the where method, so the injection has not yet been caused..

![image](https://github.com/bcvgh/fuel-cms-sqlinjection/assets/56790427/aee8ea06-c1cb-48eb-80f2-7e6172681ec4)

Until line 4450, the user's external input is stored in the `$other_args` array at this time, and has not been processed safely

![image](https://github.com/bcvgh/fuel-cms-sqlinjection/assets/56790427/840de685-5b5d-47ed-ade6-b44ca1dd5beb)

Enter the `$this->db->order_by()` method, at this time the external input is spliced into the sql statement again, and there is no single quotation mark included

![image](https://github.com/bcvgh/fuel-cms-sqlinjection/assets/56790427/918184c0-54b9-4689-b178-5f0a98f21ee3)

Finally, the `$this->db->get()` method executes the database command, causing sql injection

![image](https://github.com/bcvgh/fuel-cms-sqlinjection/assets/56790427/12f850ef-0e88-4421-9f78-6d56c1dd652a)

![image](https://github.com/bcvgh/fuel-cms-sqlinjection/assets/56790427/f034f59d-10fe-49ec-adef-dbea7170b8a8)

sqlmap:

![image](https://github.com/bcvgh/fuel-cms-sqlinjection/assets/56790427/555f972a-a32e-47c9-a6fe-b2030028cf45)
