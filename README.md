KOBIO_biologics
===============

It is codes repository for a medical journal, 'Machine learning based prediction model for responses of bDMARDs in patients with rheumatoid arthritis and ankylosing spondylitis'.

The raw input data is a obscure medical record so that we cannot make public. If you want to ask about data or codes, please contact us. The original data is Korean College of Rheumatology Biologics (KOBIO) registry, and the Korean College of Rheumatology (KCR) has the rights of data.

We open our codes for random forest model which was the best performing in our analysis.

Environment setting
-------------------

Installing miniconda:
* We recommend using miniconda as a package manager when executing KOBIO_biologics. Miniconda can be installed from the following link: https://docs.conda.io/en/latest/miniconda.html#

Environment setting:
* All software tests were done under the following environment setting, and therefore we strongly recommend to set the miniconda environment via following commands:

<pre>
<code>
conda create --name KOBIO_biologics python=3.8.5
conda activate KOBIO_biologics
conda install tensorflow=2.4.1 scikit-learn=0.24.1 numpy=1.19.5 pandas=1.2.1 matplotlib=3.3.3 xgboost=1.3.3 jedi=0.17.2 h5py=2.10.0 joblib
</code>
</pre>

Cloning the repository:
* When environment setting is complete, clone or download the repository. 

Input file preparation
----------------------

### Input file format:

* Example input files are prepared in 'Software/Input_data' folder. 

<pre>
<code>
RA_input_example.txt
AS_input_example.txt
</code>
</pre>

> Both input data are tab delimited text. Those are clinical data of twenty virtual patients of each disease. Because they are not the real data of patients, please use the example data only for testing codes or for refering to create your own input file.

* You can use the identical variables of our input data. However, our codes do not have pre-defined number of input variables so you can use your own input dataset. 
* Input file must be a tab delimited text file.
* Input file must include following three variables: 'newID', 'region', 'ACR20' for RA / 'ASAS20' for AS.
  *  newID: ID of each individual. It is not included in learning process.
  *  ACR20 for AS / ASAS20 for AS
    *  ACR20: 20% ACR Response Criteria
    *  ASAS20: 20% ASAS Response Criteria
    *  Both variables are the result variables what we want to predict. You have to include the responses of bDMARDs here.
    *  Both variables have to be binary. (Only 0 and 1)
  *  region: Located region of enrolled hostpital. As explained in our article, we divided training dataset and independent test dataset according to region of enrolled hospital. If you want to change the regional codes or even want to change how to divide dataset, you can change following part of codes.

> RA
<pre>
<code>
df_training = df[(df.region!=1) & (df.region != 11)]
df_independent = df[(df.region == 1) | (df.region == 11)]
</code>
</pre>

> AS
<pre>
<code>
df_training = df[(df.region!=2) & (df.region != 11) & (df.region != 21) & (df.region != 3) & (df.region != 24)]
df_independent = df[(df.region == 2) | (df.region == 11) | (df.region == 21) | (df.region == 3) | (df.region == 24)]
</code>
</pre>

1. Hyperparameter tuning
------------------------
01-1_RA_RF_hyperparameter_tuning.ipynb
> Code for hyperparameter tuning for RA patients.
11-1_AS_RF_hyperparameter_tuning.ipynb
> Code for hyperparameter tuning for AS patients.

* Codes below are lists of hyperparameters we used. You can add or remove some hyperparameter if you want. However, we recommend do not change list_iteration. Our codes include performance analysis and it assume that the models are created three times with an identical hyperparameter set.

<pre>
<code>
list_n_tree = [10,30,100,300,1000]
list_n_max_depth = [2,3,4,5,7,10, None]
list_n_min_samples_split = [2,3,4,5]
list_n_min_samples_leaf = [1,2,3,4,5]
list_iteration = [1,2,3]
</code>
</pre>

* You have to change the name of input file or use our example input file. 
<pre>
<code>
df=pd.read_csv('./Input_data/RA_input_example.txt', sep='\t')
</code>
</pre>
> df=pd.read_csv('YOUR/OWN/INPUT/FILE', sep='\t')

2. Save model
-------------

You can save the best models with selected hyperparameter set from '1. Hyperparameter tuning'.

01-2_RA_RF_save_model.ipynb
> Code for saving machine learning model for RA patients.
11-2_AS_RF_save_model.ipynb
> Code for saving machine learning model for AS patients.

* You have to change the hyperparameter set as you analyzed in '1. Hyperparameter tuning'.

<pre>
<code>
n_tree= 1000
n_max_depth = 4
n_min_samples_split = 3
n_min_samples_leaf = 5
list_seed = [547, 254, 758]
</code>
</pre>

* You have to change the name of input file or use our example input file. 
<pre>
<code>
df=pd.read_csv('./Input_data/RA_input_example.txt', sep='\t')
</code>
</pre>
> df=pd.read_csv('YOUR/OWN/INPUT/FILE', sep='\t')

3. Confidence interval (Boostrap)
---------------------------------

You can find confidence interval of each model by bootstrap method.
01-3_RA_RF_CI.ipynb
> Code for bootstrap for RA patients.
11-3_AS_RF_CI.ipynb
> Code for bootstrap for AS patients.

* You have to change the hyperparameter set and input file as you did in '2. Save model'.

### Models we generated are saved in 'Original_models' folder. 
