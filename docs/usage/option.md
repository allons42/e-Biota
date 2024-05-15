Usage options
=====


**python [eBiota.py](http://eBiota.py) [--argument]**


Building GEM model
------------

我们使用Carveme开源工具来构建GEM，可以选择基于现有的faa文件或者RefSeq的GCF编号进行重建。也可以直接使用BiGG等数据库中现有的GEM。

```bash
carve genome.faa -o model.xml.gz
carve --refseq GCF_000005845.2 -o ecoli_k12_mg1655.xml

python eBiota.py --Function rebuild --faa=faa_files --path=.
python eBiota.py --Function rebuild --gcf=gcf_files --path=.
```

Bacteria evaluation
------------

```bash
python eBiota.py --Function evaluate 
```

Community design
------------

```bash
python eBiota.py --Function design --medium basic --eval_path ./evaluation
```

--medium: 输入培养基， --aerobic, --anaerobic: 指定有氧or无氧，默认有氧 --glucose, --glucose_free: 指定是否提供葡萄糖，默认有葡萄糖

--degrade/--produce：指定目标是降解还是生产

--target_substrate, --target_product, --target_intermediate: 指定底物、产物、衔接物，可以让用户从BiGG id中选择

Notice that the bacteria used for community design should be processed by “Function: evaluate”. The program will check “eval_path” and evaluate missing bacteria, using “evaluation” directory by default.

DeepCooc: Co-occurrence prediction
------------

1. 首先使用`parse_data_for_DeepCooc.py`文件将用户输入的菌群物种组成（GCF id）转变为模型可以接受的输入数据：

   ```bash
   python parse_data_for_DeepCooc.py --file_input $file_input --data_taxa_type $data_taxa_type --micro_num $micro_num --feature_index_type moreReacFeature
   
   python parse_data_for_DeepCooc.py --file_input $file_input --data_taxa_type $data_taxa_type --micro_num $micro_num --feature_index_type metflux_ori_431
   ```

   其中`--micro_num`指定群落的成员数量，`--file_input`是一个包含了群落信息的tsv文件，每行是一个群落，`Bac_1`…`Bac_N`列是相应的成员菌（输入类型由`--data_taxa_type`参数给定，目前支持GCF id和genus名称）。`--feature_index_type`参数指定提取的特征类别，`metflux_ori_431`为提取代谢物的flux特征，`moreReacFeature`为提取transport反应的flux特征，模型默认情况下仅使用后者作为输入。该程序会生成`./temp/x_part_-1_${data_taxa_type}_dataset_${feature_index_type}_micro_${micro_num}.npy`文件用作模型的输入数据。

2. 使用`hiorco_DeepCooc_main.py`文件进行菌群共存情况预测：

   ```bash
   python hiorco_DeepCooc_main.py --test_x ./temp/x_part_-1_${data_taxa_type}_dataset_moreReacFeature_micro_${micro_num}.npy \\
       --test_x_met ./temp/x_part_-1_${data_taxa_type}_dataset_moreReacFeature_micro_${micro_num}.npy \\
       --test_y ./temp/y_part_-1_${data_taxa_type}_dataset_moreReacFeature_micro_${micro_num}.npy \\
       --micro_num $micro_num \\
       --input_tsv $file_input \\
       --saved_dir $saved_dir \\
       --model_mode FC_mergedCNN \\
       --dl_model_path ../data/model_file/model_weights.pth \\
   ```

   其中`--test_x`和`--test_x_met`分别接收上述`moreReacFeature`和`metflux_ori_431`生成的输入特征（其中`--test_x_met`可以设为None而不启用这部分特征信息）。`--input_tsv`和`--micro_num`的输入与上文一致。`--saved_dir`为输出结果目录，`--dl_model_path`和`--model_mode`给定模型信息。输出的文件为在输入的tsv文件后添加两列`DeepCooc_pred_value`和`DeepCooc_Co_occurrence`分别为模型预测的菌群共存得分（0-1之间的数越接近1越倾向于共存）以及模型预测的共存情况（0为不共存1为共存）。

3. 除了`DeepCooc`的共存预测指标外，我们还提供来自其他文献的菌群相互作用指标：

   ```bash
   python other_filter_label.py --file_input $file_input --file_out $file_out
   ```

   这里的`--file_input`输入可以为上述DeepCooc模型输出的tsv文件，注意该tsv文件需要包括从eBiota主函数得到的单菌情况以及群落情况下的各个成员菌的生长以及生产的值。`--file_out`指定输出文件路径，输出文件额外给出6类相互作用关系（仅支持2菌）以及eveness index。
