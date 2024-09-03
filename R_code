# 加载所需的R包
library(SNPlocs.Hsapiens.dbSNP155.GRCh37)
library(BSgenome.Hsapiens.UCSC.hg19)
library(data.table)

# 选择并读取GWAS文件
file_path <- file.choose()  # 打开文件选择对话框
GWAS_data <- fread(file_path)

# 提取SNP列
rsid_list <- GWAS_data$SNP # 根据你的文件列名填写

# 剔除不符合命名规则的SNP
invalid_snp_indices <- which(!grepl("^rs[0-9]+$", rsid_list))

if (length(invalid_snp_indices) > 0) {
  invalid_snps <- rsid_list[invalid_snp_indices]
  GWAS_data <- GWAS_data[-invalid_snp_indices, ]
  rsid_list <- rsid_list[-invalid_snp_indices]
} else {
  invalid_snps <- NULL  # 或者记录未发现不符合命名规则的SNP
  cat("No invalid SNPs found.\n")
}

# 记录剔除了多少行
num_invalid_snps <- length(invalid_snp_indices)

# 获取参考的SNP信息，同时自动忽略不存在的SNP
snp_data <- snpsById(SNPlocs.Hsapiens.dbSNP155.GRCh37, rsid_list, ifnotfound="drop")
valid_rsids <- mcols(snp_data)$RefSNP_id

# 找出在参考数据中不存在的SNP
missing_snps <- setdiff(rsid_list, valid_rsids)
num_missing_snps <- length(missing_snps)

# 过滤出有效的SNP对应的行
GWAS_data_new <- GWAS_data[rsid_list %in% valid_rsids, ]

# 提取染色体和位置信息并添加到新数据集中
CHR <- as.character(seqnames(snp_data))
BP <- start(snp_data)
GWAS_data_new <- cbind(GWAS_data_new, CHR = CHR, BP = BP)

# 保存结果文件和日志文件
output_dir <- file.path(dirname(file_path), "GRCh37_processed_results")
if (!dir.exists(output_dir)) {
  dir.create(output_dir)
}

output_file <- file.path(output_dir, "GWAS_data_with_SNP_info.csv.gz")
fwrite(GWAS_data_new, output_file, compress = "gzip")

log_file <- file.path(output_dir, "processing_log.txt")

# 使用 sessionInfo() 来记录R包版本信息
session_info <- sessionInfo()
reference_version <- "SNPlocs.Hsapiens.dbSNP155.GRCh37"

# 计算有效注释的百分比
total_snp_count <- length(rsid_list) + num_invalid_snps
percent_valid <- (nrow(GWAS_data_new) / total_snp_count) * 100

log_info <- paste0(
  "Invalid SNPs removed: ", num_invalid_snps, "\n",
  "Missing SNPs in reference: ", num_missing_snps, "\n",
  "Total SNPs processed: ", nrow(GWAS_data_new), "\n",
  "Percentage of SNPs successfully annotated: ", round(percent_valid, 2), "%\n",
  "Reference version: ", reference_version, "\n",
  "Processing time: ", Sys.time(), "\n",
  "\nHead of GWAS_data_new:\n",
  paste(capture.output(head(GWAS_data_new)), collapse = "\n")
)
write(log_info, file = log_file)

cat("Processing complete. Results and logs saved in:", output_dir, "\n")
