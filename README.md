# suprisal_parser

An Incremental Dependency Parser intended to get surprisal values

## Packages required:
Package : command to install package
zip :     sudo apt install zip
python2 : sudo apt install python2
numpy :   sudo apt install python-numpy

## Files:
-ArcEager.py:		Implements the Arc Eager algorithm (can be replaced by any other incremental dependency parse algorithm)
-DependenyParse.py: 	The main function implemented here is best_parse which calulates likelihoods and surprisal
-file_utilities.py:	Implements mundane functions for file  handling
-maxent.py:			Implements prediction using megam weights. Can be replaced by any standard model for prediction
-file_tasks.py (optional): Contains functions to do intermediate file processing. Not needed if you have data in the right form

## To run the parser

first cd to code directory

Commands to find out surprisal values:

Step 1: Train the parser /n
`./main.py process --dir [PATH WHERE output of step1 is stored] --ifiles [PATH > TRAINING DATA FILE] [PATH > DEVELOPMENT DATA FILE] --ofile [PATH > OUTPUT DATA FILE] --file_type conllu --feature_set combined_all_features_without_gnp`

`./main.py process --dir ../data/output_step1 --ifiles ../data/utf8/hi_hdtb-ud-train.conllu.txt ../data/utf8/hi_hdtb-ud-dev.conllu.txt --ofile gold1.train.feats_co --file_type conllu --feature_set combined_all_features_without_gnp`

Note: For windows try python main.py proces... instead of ./main.py pro...

Step 2: Get weights of features from output file of step 1

./megam_i686.opt multiclass [PATH TO OUTPUT DATA FILE OF STEP1] > [PATH TO STORE OUTPUT WEIGHT FILE]

./megam_i686.opt multiclass ../data/output_step1/gold1.train.feats_co > ../data/output_step2/gold2.train.weights_co

Note:This command will only work in Linus or Sun4 Solaris, for more information check https://users.umiacs.umd.edu/~hal/megam/version0_91/


Step 3: Get surprisal values

 ./main.py surprisal --dp_file [PATH TO INPUT FILE] --ofile [PATH TO STORE PARSED CONLL OUTPUT FILE] --meta_file [PATH TO META OUTPUT FILE OF STEP1] --surp_file [PATH TO STORE FINAL OUTPUT SURPRISAL VALUE FILE] --wts_file [PATH TO OUTPUT WEIGHT FILE OF STEP2] --k 1 

 ./main.py surprisal --dp_file ../Files/sample_Pos.txt --ofile ../data/output_step3/parsed_output.txt --meta_file ../data/output_step1/meta_gold1.train.feats_co --surp_file ../data/output_step3/surprisal_output.txt --wts_file ../data/output_step2/gold2.train.weights_co --k 1  


Note: For windows try python main.py proces... instead of ./main.py pro...


Original code: https://github.com/theboywhoboasted/incremental-parser
