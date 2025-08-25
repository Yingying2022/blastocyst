# README of file structure

## Data: 
### 1_fastq_from_liumin: 支线一
 - 10_fastq: 分好类的各种样本（所有）
   - 样本信息见/mnt/helab3/yyhan/Projects/embryo_classification/Data/1_fastq_form_liumin/merged_fastq_info.csv
 - 11_selected_20fastq: 从10_fastq中的36种样本（6个stage6种marker）中每种样本随机选20个样本
    - 00_fastq: fastq及其tacit预处理结果（bigwig, unique_bam(sorted bam), rmdup_bam）
        - bigWig_all_renamed_sample: 修改名称之后的bigwig
            - coverage_over_cell_specific_peak:根据/mnt/helab3/yyhan/Projects/embryo_classification/Data/1_fastq_form_liumin/11_selected_20fastq/02_celltype_specific_0.8_peak_20fq_top1000/merged_cell_specific_bed/merged_top1000_specific.bed计算得到的

    - 00_merged_bam_fq20:合并20个样本得到的
    - 01_merged_bam_20fq_callpeak_*:取不同的qValue阈值对00_merged_bam_fq20进行callpeak得到的peak
    - 02_celltype_specific_0.8_peak_20fq_top*:对01_merged_bam_20fq_callpeak_0.8取qvalue最大的N个peak
- logs: 一些脚本的log文件，不重要
- script_process_fq:对fastq文件进行处理的tacit脚本

### 2_all_bam_from_liumin: 支线二
- 00_all_bam_paired: 刘敏师姐给的单细胞bam数据及其去重复之后的bam（只是以防万一没去重复）和bigwig（每个细胞6种marker对应好的），已经改名为marker_stage_#number.bam；注意这里面是没有zygote的
- 01_merged_bam: 合并之后的bam
- 02_callpeak_0.05: qValue阈值取0.05得到peak


### 3_peaks_from_liumin: 支线三
- *.bed:刘敏给的peak：
- peak_distance: *bed得到的peak之间的距离
- figures/hist_peak_dist:*.bed得到的peak之间距离的统计图
- peak_counts:peak统计结果
- cell_specific_peak_from_liumin:
    - *specific.bed:根据 *.bed得到的细胞特异性peak区域
    - peak_distance: *specific.bed得到的peak之间的距离
    - figures/hist_peak_dist:*specific.bed得到的peak之间距离的统计图


## script: 对应Data中的三个支线
### 01_fastq_line:
- 011_prepare_data: 和下载、整理数据有关脚本
- 012_preprocess:
    - 最重要的是tacit pipline处理fastq原始数据：/mnt/helab3/yyhan/Projects/embryo_classification/script/01_fastq_line/012_preprocess/0121_pipline_tacit.sh
    - 其他就是基本的操作，最后获得bigwig of every fastq
- 013_obtain_peak_ranges: 获取细胞特异性peak区域
- 014_obtain_coverage: 根据012_preprocess和013_obtain_peak_ranges获得coverage over marker_cell specific peaks
- 05_classifier:
    - 0150_multi_classifier_v2.py：三个多分类模型训练脚本最终版本
    - 0151_test_classifier.py:未经调试的测试模型脚本
    - 0152_plot_pca.py:直接用PCA去聚类分析的脚本
### 02_bam_line：预处理和计算bam数据的coverage over cell specific peak脚本——未经调试
### 03_peak_line：简单处理peak的脚本

## Tables: 可以认为是fastq 支线的 metadata表格
 - **00_sample_list.txt** : 样本名称列表，即marker_cell组合
 - **00_SRR_list1_GSE235109.txt**：GSE235109中所有SRR* 编号，即上面Data中的fastq对应的SRR*编号,和00_GSE235109_1_fastq_list.txt对应
 - **00_SRR_list2_GSE259393.txt**：GSE259393中所有SRR* 编号，上面的Data中没有涉及,和00_GSE259393_2_fastq_list.txt对应
 - **01_data_GSM_marker_cellType.csv**:GEO_accession,sample_name
 - **02_data_GSM_SSR.csv**: GEO_accession,Run(SRR*编号)
 - **03_merged_fastq_info.csv**：GEO_accession,Run,sample_name
 - **04_geo_samples_numbered.csv**：GEO_accession,Run,sample_name,sample_name_indexed,sample_name_numbered（给同一种marker_stage不同样本进行编号）

## embryo_classifier.yml: 多分类模型所需要的环境
Note: Notes_Embryo_Classifier.pdf & 周工作汇报-韩莹莹-11-20250808.pptx may be useful.





