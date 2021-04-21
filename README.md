KOBIO_biologics
===============

It is codes repository for a medical journal, 'Machine learning based prediction model for responses of bDMARDs in patients with rheumatoid arthritis and ankylosing spondylitis'.

The raw input data is a obscure medical record so that we cannot make public. If you want to ask about data or codes, please contact us. The original data is Korean College of Rheumatology Biologics (KOBIO) registry, and the Korean College of Rheumatology (KCR) has the rights of data.

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
conda install tensorflow=2.4.1 scikit-learn=0.24.1 numpy=1.19.5 pandas=1.2.1 matplotlib=3.3.3 xgboost=1.3.3 jedi=0.17.2 h5py=2.10.0
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

> Both input data are tab delimited text. Those are clinical data of twenty virtual patients of each disease. Because they are not the real data of patients, please use the example data only for testing codes or for creating your own input file.

* You can use the identical variables of our input data. However, our codes do not have pre-defined number of input variables so you can use your own input dataset. 
* Input file must be a tab delimited text file.
* Input file must include following three variables: 'newID', 'region', 'ACR20' for RA / 'ASAS20' for AS.
  *  newID: ID of each individual. It is not included in learning process.
  *  ACR20 for AS / ASAS20 for AS
    *  ACR20: 20% ACR Response Criteria
    *  ASAS20: 20% ASAS Response Criteria
    *  Both variables are the result variables what we want to predict. You have to include the responses of bDMARDs here.
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

