# WSDM 2022 CUP - Cross-Market Recommendation
Our team ranks the second place on the final leaderboard with an excellent performance very close to the first place.
## Requirements:
The requirements of experiment environment are listed in requirements.txt.

## Train LightGCN model:

We totally follow the structure of xmrec's sample code for our own model design. Thus it is very easy to reproduce for version of lightgcn.

Before running train script, you should perform preprocessing by running script `merge_train.py`. After that, train script is easy to run by referring to the samples below.

`train_baseline.py` is the script for training our model that is taking one target market. 

Here is a sample train script from training model using data of t1 without word2vec:

    python train_baseline.py --tgt_market t1 --src_markets none --tgt_market_valid DATA/t1/valid_run.tsv --tgt_market_test DATA/t1/test_run.tsv --exp_name lightgcn_without --num_epoch 60 --cuda --model_name LightGCN_no_w2v

Here is a sample train script from training model using data of t2 without word2vec:

```
python train_baseline.py --tgt_market t2 --src_markets none --tgt_market_valid DATA/t2/valid_run.tsv --tgt_market_test DATA/t2/test_run.tsv --exp_name lightgcn_without --num_epoch 50 --cuda --tgt_market_valid_pos DATA/t2/valid_qrel.tsv --model_name LightGCN_no_w2v
```

Here is a sample train script from training model using data of t1 with word2vec:

```
python train_baseline.py --tgt_market t1 --src_markets none --tgt_market_valid DATA/t1/valid_run.tsv --tgt_market_test DATA/t1/test_run.tsv --exp_name lightgcn_with --num_epoch 60 --cuda --model_name LightGCN_with_w2v
```

Here is a sample train script from training model using data of t2 with word2vec:

```
python train_baseline.py --tgt_market t2 --src_markets none --tgt_market_valid DATA/t2/valid_run.tsv --tgt_market_test DATA/t2/test_run.tsv --exp_name lightgcn_with --num_epoch 50 --cuda --tgt_market_valid_pos DATA/t2/valid_qrel.tsv --model_name LightGCN_with_w2v
```

These four samples can be executed seperately to accelerate training procedure.

After training your model, the scripts prints the directories of model and index checkpoints as well as the run files for the validation and test data as below. 

    Run output files:
    --validation: ./merge/t1/LightGCN_with_w2v_valid.tsv
    --test: ./merge/t1/LightGCN_with_w2v_test.tsv
    Experiment finished successfully!

After getting corresponding result files, result files will be input into other tree-like model as ranking features.

## Train Tree model

You can run scripts below:

```
#6.recall feature
python recall_feature.py
python recall_feature_2.py
# Two python scripts above can run in parellel

#7.lightgbm Classifier model   
python class_lgb_model.py

#8.xgboost Classifier model    
python class_xgb_model.py

#9.catboost Classifier model   
python class_cat_model.py

#10.lightgbm Rank model        
python rank_lgb_model.py

# Four python scripts(#7,#8,#9,#10) above can run in parellel
```

Finally, by running `trans_file.py` to aggregate multiple results, you can  get final submissions we have submitted in path `./submission`.

