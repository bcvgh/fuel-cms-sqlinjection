Need to login to the background.

`fuel/modules/fuel/controllers/Blocks.php`

line 64  `import_view` method starts

Line 70 receives the id parameter of the post request and enters the `import` method

![image-20230509201322059](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509201322059.png)

fuel/modules/fuel/libraries/Fuel_blocks.php

Then enter the `find_by_key` method on line 307

![image-20230509202318805](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509202318805.png)

Because the `find_by_key` method does not exist, enter the `__call `method of the current object

![image-20230509203308898](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509203308898.png)

Enter line 4421 of MY_Model.php, pass parameters to `$this->db->where()` method

![image-20230509203527073](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509203527073.png)

At this time, the external input string is spliced into the SQL statement through the `$this->db->where()` method, but at this time the external input will be surrounded by single quotes Contains, so no injection has been caused yet.

![image-20230509211423687](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509211423687.png)

Until line 4450, the user's external input is stored in the `$other_args` array at this time, and has not been processed safely

![image-20230509205939795](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509205939795.png)

Enter the `$this->db->order_by()` method, at this time the external input is spliced into the sql statement again, and there is no single quotation mark included

![image-20230509210727525](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509210727525.png)

Finally, the `$this->db->get()` method executes the database command, causing sql injection

![image-20230509204416766](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509204416766.png)

![image-20230509210948547](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509210948547.png)

sqlmap:

![image-20230509211043471](C:\Users\zll\AppData\Roaming\Typora\typora-user-images\image-20230509211043471.png)
