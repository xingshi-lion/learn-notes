一、导入数据

1、导入制表符分隔的数据

cat /data/ZDGL/stateAnalysis/dmt_term_stateAnalysisALL202010.txt | clickhouse-client -u default --password 6lYaUiFi  --query="INSERT INTO knowyou_ott_ods.dmt_term_stateAnalysisALL FORMAT TabSeparated";

cat /data/ZDGL/stateAnalysis/dmt_term_stateAnalysisALL202010.txt | clickhouse-client -u default --password 6lYaUiFi --query="INSERT INTO knowyou_ott_ods.dmt_term_stateAnalysisALL FORMAT TSV";

2、导入CSV格式数据

cat /data/ZDGL/stateAnalysis/dmt_term_stateAnalysisALL202010.csv | clickhouse-client -u default --password 6lYaUiFi  --query="INSERT INTO knowyou_ott_ods.dmt_term_stateAnalysisALL FORMAT CSV";

3、指定分割符导入

cat test.csv | clickhouse-client -u user --password password --format_csv_delimiter="|" --query="INSERT INTO db.tab1 FORMAT CSV";

4、加入最大分区数导入

cat /data/ZDGL/inventoryTurnover/dmt_term_inventoryTurnover202010.txt | clickhouse-client -u default --password 6lYaUiFi  --max_partitions_per_insert_block 10000 --query="INSERT INTO knowyou_ott_ods.dmt_term_inventoryTurnover FORMAT TabSeparated";

5、跳过错误行导入

cat /data/erqi/zhuangwei/company/edw_sp_grid.txt | clickhouse-client -u default --password 6lYaUiFi  --input_format_allow_errors_num=1 --input_format_allow_errors_ratio=0.1 --query="INSERT INTO knowyou_ott_ods.edw_sp_grid FORMAT TabSeparated";



二、导出数据

1、以CSV分隔符导出

clickhouse-client -u default --password 6lYaUiFi --query="select * from knowyou_ott_ods.dmt_ott_withduser limit 30 FORMAT CSV" > dmt_ott_withduser.csv

2、以制表符导出

clickhouse-client -u default --password 6lYaUiFi --query="select * from knowyou_ott_ods.dmt_ott_withduser limit 30 FORMAT TSV" > dmt_ott_withduser.txt



作者：无起_5d34
链接：https://www.jianshu.com/p/73636d216048
