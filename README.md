# rsID_to_CHR_BP
This R script processes GWAS data by filtering SNPs based on naming rules, checking their existence in a reference database (SNPlocs.Hsapiens.dbSNP155.GRCh37), and annotating valid SNPs with chromosome and base pair position information. The script also logs details about invalid and missing SNPs, the percentage of successfully annotated SNPs, and the R package versions used.

该R脚本处理GWAS数据，通过基于命名规则过滤SNP，检查其在参考数据库 (SNPlocs.Hsapiens.dbSNP155.GRCh37) 中的存在性，并为有效的SNP添加染色体和碱基对位置信息。脚本还记录了无效和缺失SNP的详细信息、成功注释SNP的百分比，以及使用的R包版本。

# R包：
SNPlocs.Hsapiens.dbSNP155.GRCh37

BSgenome.Hsapiens.UCSC.hg19

data.table


# 使用说明
执行R脚本，它将提示您选择一个GWAS文件。

过滤和注释：脚本将：
删除不符合“rs”前缀命名规则的SNP。
使用参考数据库对剩余的SNP进行注释。
记录在参考中找不到的SNP的详细信息。

输出：
处理后的GWAS数据：输出将包含有效SNP的染色体和碱基对信息。
日志文件：日志文件记录无效和缺失SNP的数量，成功注释SNP的百分比，以及会话信息。
结果保存：处理后的数据和日志文件保存在原始GWAS文件目录下的GRCh37_processed_results目录中。

输出文件
GWAS_data_with_SNP_info.csv.gz：处理后的GWAS数据，包含染色体和碱基对信息。
processing_log.txt：记录处理步骤和结果的日志文件。
