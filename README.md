# table-GAN
**table-GAN** is a synthetic data generation tool which has been implemented using a deep learnign model based on **Generative Adversarial Network** architecture. The goal of this tool is to protect sensitive data against re-identification attacks by producing syhtetic data out of real data while preserving statitical features. 


## Prerequisites

- Python 2.7 or Python 3.3+
- [Tensorflow 0.12.1](https://github.com/tensorflow/tensorflow/tree/r0.12)
- [SciPy,Numpy](http://www.scipy.org/install.html)



### Data Sets 
All the datasets used in tableGAN should be placed in **/data/** directory, each dataset is placed in a seperate folder with its corresponding name. Our experiments uses these four datasets :
-   "[Adult](https://archive.ics.uci.edu/ml/machine-learning-databases/adult)": Contains personal records (such as nationality, education level, occupation, work hours per week, and so forth)

### Input Data Files
Each data set folder contains different files used for training phase:
-   **DATASET_train_cleaned.csv**: Contains cleaned data from original source of dataset (above link), stored in a CSV format. This is used for **training** only.

## Usage

### 1.Training

To **Train** a model with the datasets it is required to run the follwong scipt in the shell of your operating system (assuming Python and Tensoflow and all librariesa are installed before)

    $ python main.py 
    --dataset=DATASET_NAME 
    --test_id =TEST_ID  
    --epoch= 200
    --train
 
**Command Line Parameters:**

  
   - **DATASET_NAME** parameter used in input files and and generating script should be one of the following values (case-sensitive):       
        -   Adult
        
   - **TEST_ID** parameter is a parameter defining a set of internal paramaters affecting the quality or privacy level of synthesized data. 
           
        - **TEST_ID** used in training and generating command lines should have one of the following values :
            -   'Origin' : 'beta':0.0 , 'delta_v': 0.0 , 'delta_m' : 0.0         --> Best Quality in the Generated Outputs
            -   'OI_11_00': 'beta':1.0 , 'delta_v': 0.0 , 'delta_m' : 0.0
            -   'OI_11_11': 'beta':1.0 , 'delta_v': 0.1 , 'delta_m' : 0.1 
            -   'OI_11_22': 'beta':1.0 , 'delta_v': 0.2 , 'delta_m' : 0.2        --> Best Privacy in the Generated Outputs        
         
    
   - **--train** parameter indicates the training phase of the model and is very important to be placed in the command line
    
   - **--epoch** paramter defines the number of iterations(epochs) used to train the model. Default value is 25, but other vlaues cna be set. Bigger values can lead to better quality models but can be time consuming.



**Example**: Training a model for Adult dataset based on the Origin parameter-set (as also mentioned in train_Adult.sh script file).

``` bash
$ python main.py --dataset=Adult   --test_id=Origin  --train

```

**Important :** Once the trainig is complete, checkpoint files will be generated in the **/checkpoint/DATASET_NAME/TEST_ID/DATASET_NAME_64-8-8** folder. For example for the above training command the 
following files will be created:
 - /checkpoint/Adult/Origin/Adult_64_8_8/tableGAN_model_6002.data-0000-of -00001
 - /checkpoint/Adult/Origin/Adult_64_8_8/tableGAN_model_6002.index
 - /checkpoint/Adult/Origin/Adult_64_8_8/tableGAN_model_6002.meta
 
 These files will be used automatically to generate the synthesized data.


### 2. Generating
To **Generate** synthetic data using a trained model use:

    $ python main.py 
    --dataset=DATA_SET_NAME 
    --test_id =TEST_ID  

All the parameters are similar to the training phase but the **--train--** paramter should **NOT** be applied (as mentioned in generate_Adult.sh script file).
```
Example:
$ python main.py --dataset=Adult   --test_id=Origin  
```


- ## Results

  The generated fakes files are placed in the  **/samples/** folder of root. The results of each data-set is placed in a separate folder such as:
  **/samples/Adult**. Because the fake tables are generated using different TEST_ID settings ( affecting data privacy and data utility of results), 
  each data-set folder has sub-folders with the corresponding TEST-ID values. 
  For example  **/samples/Adult/Origin** contains all the results for Adult dataset generated under the settings indicated by "Origin". 
  Each TEST_ID subfolder contains the following content:
  - **DATASET_TESTID_fake.csv** : Generated fake table. 
   
   For example, results of table "Adult" with settings of TEST_ID=Origin:

   -    Results folder : /samples/Adult/Origin
   -    Generated Fake Tabels :  Adult_Origin_fake.csv
        

  

