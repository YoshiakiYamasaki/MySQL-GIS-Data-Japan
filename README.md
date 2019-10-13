# MySQL-GIS-Data-Japan
MySQLで使えるGISデータの配布用リポジトリ<br>
　- e-Statで配布されている境界データのシェープファイルをogr2ogrを使ってMySQLに取り込んで、Spatialインデックスを追加したもの
 
 Repository for distributing Japanese GIS Data which is made from e-Stat's Shape files for MySQL<br>
　- Shape file of boundary data distributed with e-Stat is imported into MySQL using ogr2ogr and added Spatial index


# 配布データの出典、加工方法 / Source and how to proceed of distribution data

出典：政府統計の総合窓口(e-Stat)（https://www.e-stat.go.jp/）<br>
Source: General contact for government statistics(e-Stat)（https://www.e-stat.go.jp/）

加工方法 / How to proceed<br>

 1.ogr2ogrを使って文字コードをUTF-8に変更<br>
  ogr2ogr -f “ESRI Shapefile” -lco ENCODING=UTF-8 -oo ENCODING=CP932 h27ka28_utf8.shp h27ka28.shp

 2.ogr2ogrを使ってシェープファイルをMySQLへインポート<br>
  ogr2ogr -f "MySQL" MySQL:"geotest,host=127.0.0.1,user=root,password=root,port=3306" h27ka28_utf8.shp

 3.SHAPE列にSRIDを定義し、空間インデックスを追加<br>
  mysql> ALTER TABLE geotest.h27ka28_utf8 DROP INDEX SHAPE;<br>
  mysql> ALTER TABLE geotest.h27ka28_utf8 MODIFY SHAPE GEOMETRY NOT NULL SRID 4612;<br>
  mysql> ALTER TABLE geotest.h27ka28_utf8 ADD SPATIAL INDEX (SHAPE);<br>
  mysql> ALTER TABLE geotest.h27ka28_utf8 RENAME hyogo;<br>

# ダンプファイルのインポート方法 / How to import dmp file
mysql> use ""Database Name""<br>
mysql> source hyogo.dmp

# 免責事項
本データを用いて行う一切の行為について、いかなる責任も負いません。本データを使用したことによって被った損害、損失に対して一切の責任を負いません。
