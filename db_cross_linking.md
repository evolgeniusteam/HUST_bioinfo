## Cross db referencing

Purpose: to increase visibility of all our databases by embedding links to other databases in the corresponding webpages in our databases.

For example, for a gene that is a TF AND a kinase:
* in the PTM database, add a link to AnimalTFDB3.0 and mention that it is a transcription factor,
* in AnimalTFDB3.0, add a link to the corresponding page of this gene in PTM;
see the screenshots below for examples.

How??
1. prepare a MySQL table:
```sql
CREATE TABLE `cross_db_links` (
  `search_key` varchar(255) NOT NULL, -- main keyword to be searched against
  `key_type` varchar(255) NOT NULL, -- additional information for the `search_key`
  `source_db` varchar(255) NOT NULL, -- database id from which the data are from
  `attributes` text NOT NULL, -- a json string contains everything ...
  KEY `search_key`(`search_key`)
);
```

2. insert some data:
```sql
insert into `cross_db_links` ( `search_key`, `key_type`, `source_db`, `attributes` )
    values( '10090', "ncbi taxonomy id", "MVP",
        '{"url":"http://mvp.medgenius.info/microbes/75/Caulobacter",
        "dbtype":"MVP",
        "anno":"assoc w/ 5 phage clusters"}'),
        ( 'CBF', 'TF family name', 'AnimalTFDB3.0',
        '{"url": "http://bioinfo.life.hust.edu.cn/AnimalTFDB/#!/tf_summary?family=CBF",
        "anno" : "TF family contains 99 member genes",
        "dbtype" : "AnimalTFDB3.0"}' )
```

show the newly inserted data:
```sql
select * from `cross_db_links`;
```
![](/images/01_mysql_screenshot.png)

3. show on web:
then it can be displayed as one of the following ways:
* external link:
![](/images/02_display_1.png)
* popup panel:
![](/images/03_display_2.png)

Of course there can be two or more external links associated with a keyword, e.g.:
![](/images/04_display_3.png)
