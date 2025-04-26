# CodePro_EdTech_startup
Model Experimentation Workflow
In your jarvis instance, you have to upload the below files as directed in the first segment if it is not present in the jarvis instance already

You can also find all the files of project here or in the zip file below.

Assignment_Code_Files
Download
Now that you have cleaned the raw data and performed EDA using pandas_profiling, we will move on to creating a model using pycaret.

But before proceeding, copy and paste the cleaned data stored in “Assignment/01_data_pipeline/notebooks/Data/cleaned_data.csv” to “Assignment/02_training_pipeline/notebooks/Data” so that the cleaned data can be used to train the model. 
Once you have done this, navigate to /02_training_pipeline/notebooks folder. Here you will find a jupyter notebook named ‘lead_scoring_model_experimentation.ipynb’, which will contain instructions on how to train a model that can be deployed into production.  You need to follow these instructions and write code for them.
For changes to be tracked by MLflow, you would require to start the MLflow server in a new terminal as you advance through the ‘lead_scoring_model_experimentation.ipynb’ notebook. The steps are as follows:
Open a new terminal.
Then go to the Assignment directory using the cd command. Type the command: cd Assignment/
Create a folder named mlruns here. You can create this folder using either the command line or GUI. To create this folder via the command line, run the command: mkdir ./mlruns
Then, type the following command to start the MLflow server:  mlflow server --backend-store-uri='sqlite:///<your_db_path>' --default-artifact-root="./mlruns" --port=<port_number> --host=0.0.0.0
Let’s understand the parameters passed to this command:
db_path: The path to the database, where different data related to runs, parameters etc. will be saved.
mlruns_folder_path: This will be used as an artifact store for models, images, etc. 
port_number: This will be the port number where the MLflow server will run and be accessible.
 

We are optimising the auc_score for our assignment. 
Ensure that you perform the following activities properly

Follow the naming convention mentioned in the notebook.
Ensure that all the files are arranged properly as mentioned in the notebook.
Once you finish all the tasks mentioned in the notebook, you would have completed the experimentation stage. The next stage will be creating a pipeline to automate data cleaning. model retraining and model inference.

 

In the next segment you will create a data cleaning pipeline using Apache Airflow.
In this segment you will create a pipeline to automate data pre-processing using Airflow.

 

Data Pipeline : 

For creating the pipeline follow the below mentioned instructions:


1. Go to the Assignment/01_data_pipeline/scripts folder.     

 

2. Now, we need to break down all the data cleaning tasks(of the data cleaning notebook) into functions. To do so follow the instructions mentioned in the utils.py file(present in this script folder) and make the required changes. 

 

You can make these changes by simply taking(copying) the appropriate code from the data cleaning notebook named “data_cleaning_template.ipynb” that has been provided in the “Assignment/01_data_pipeline/notebooks” folder and pasting it in the function’s body in utils.py.

 

3. While defining the above functions, you are not required to pass the input variables as arguments. Instead, these variables have already been defined in the constants.py file and significant_categorical_level.py file. Therefore, to use them, you just need to set appropriate values for them and import these files. 

 

4. Recall that while reducing the level of “first_platform_c”, “first_utm_medium_c” and “first_utm_source_c” we mapped the bottom 10 percentile of these columns to ‘others’. In our notebook, we created a list of significant levels of these three columns. We will store this list in the “significant_categorical_level.py” file here. Now import this in utils.py and use the variables instead of hardcoded lists.

 

Note: The value of the variables in the “significant_categorical_level.py” file have been intentionally left blank as an empty list and need to be completed by you. 

 

5. Once you have converted the code into the respective functions. You will modify the function to read their input data and write their output to the database present in file 'utils_output.db'. The function will be executed in the following order: read the input for the functions and write the outputs of the function to the above db appropriately.

 
Please ensure that you follow the naming convention specified in the docstring of the function while naming the tables to which your function will read and write the data from.

 
The order of the functions is
build_dbs -> load_data_into_db -> map_city_tier -> map_categorical_vars -> interactions_mapping.

 

NOTE:  ‘utils_output.db’ will be created once you execute the build_dbs function. This db should be created in the/Assignment/01_data_pipeline/scripts folder.
    
6. Once you have modified the functions, create a dummy notebook in the same folder, import the utils file, and run all the functions in the proper order, as mentioned above. This will help you debug your code and create the  ‘utils_output.db’ database in the specified path.

 

7. Once you have ensured that the functions in the utils.py file are working as intended, store all the constant values in constants.py and repeat step 5 to ensure that the code works properly. Some of the variables’ names are mentioned in the constants file, which you can use. You can also create your additional variable if required. 


NOTE: utils.py and constants.py must be present in the same directory always.

 

8. Once you have ensured that the code is working properly, create a folder named 'unit_test' and copy paste all the files except for the data folder in the “/Assignment/01_data_pipeline/scripts” folder in it. Please do not delete the files in the “/Assignment/01_data_pipeline/scripts”; rather, just copy-paste them to the “unit_test” folder. 

The following files should be present in the “unit_test” folder.

├── city_tier_mapping.py
├── constants.py
├── data_validation_checks.py
├── dummy.ipynb
├── interaction_mapping.csv
├── lead_scoring_data_pipeline.py
├── leadscoring_test.csv
├── schema.py
├── significant_categorical_level.py
├── test_with_pytest.py
├── unit_test_cases.db
├── utils.py
└── utils_output.db


 

NOTE: The 'leadscoring_test.csv' present in the scripts folder has a sample input which will be given to the “load_data_into_db” function. Since other functions will work on the output of the previous function input, we do not need input. However, we need to compare their output to the expected output, so we will need the expected output for all the functions present in the test_with_pytest.py' file. 

 

9. Go to the 'unit_test' folder and open the 'test_with_pytest.py' file. Here you will write unit test cases to verify that the functions are giving their intended outputs. For this, you will write tests that check that the output for all the functions matches against the expected output provided in 'unit_test_cases.db' . The sample output for each function is provided in the following table:
    
    load_data_into_db :  “loaded_data_test_case”.
    map_city_tier  : “city_tier_mapped_test_case”
    map_categorical_vars : “categorical_variables_mapped_test_case” 
    Interactions_mapping : “interactions_mapped_test_case”

 

Note: Here for the testing purpose, we are going to work with ‘leadscoring_test.csv’ file. Therefore, change your functions accordingly present in utils.py in the unit_test folder. Do not make any changes in the utils.py file present in the script folder.

 

10. Once all the test cases are passed, go back to the 'scripts' folder and create a folder named 'mapping'. Save all the mapping files, namely “ city_tier_mapping.py”, interaction_mapping.csv, and significant_categorical_level.py in the ‘mapping’ folder.

Note: After putting the mentioned files in the mapping folder, please modify the import statements accordingly.

 

11. Now open 'data_validation_check.py' and write the mentioned code there. The function mentioned in the 'data_validation_check.py' will run in the order mentioned below. Ensure that you read and write from the appropriate table from the db for these two functions in 'data_validation_check.py'. 

 

build_dbs -> raw_data_schema_check -> load_data_into_db -> map_city_tier -> map_categorical_vars -> interactions_mapping -> model_input_schema_check

 

NOTE: Note that there will not be any change in the functions present in utils.py as for the function present in “data_validation_checks.py” we are simply reading the data from the appropriate source and checking if it is the same as the schema present in schema.py file.

 

12. Now, in the same dummy notebook, ensure that these functions are working properly.

 

13. Now create a pipeline using airflow in the 'lead_scoring_data_pipeline.py'. Follow the instructions in this file for creating the dag for airflow.
    
14. Now go to the '~/airflow/dags/' folder and create a folder named 'Lead_scoring_data_pipeline'.

The following file should be present in Lead_scoring_data_pipeline
├── data
│   ├── data/leadscoring.csv
├── mapping
│   ├── mapping/city_tier_mapping.py
│   ├── mapping/interaction_mapping.csv
│   └── mapping/significant_categorical_level.py
├── schema.py
├── data_validation_checks.py
├── lead_scoring_data_pipeline.py
├── constants.py
└── utils.py.

Therefore, go back to the scripts folder and copy all the files and folders shown in the above image. Once this is done, paste them inside the
‘~/airflow/dags/lead_scoring_data_pipeline’ folder.

 

15. After copying all the necessary files and folder from the 'scripts' folder to 'Lead_scoring_data_pipeline', modify your constants, utils and lead_scoring_data_pipeline.py as the paths have been changed when you have changed the directory. Make changes in the file path names in the constants.py file.

 

Notice that we did not copy paste “utils_output.db”. This is because we plan to create a new db named “lead_scoring_data_cleaning.db”. So you have to change the name of db to “lead_scoring_data_cleaning.db” in the constants.py file and, finally, make changes in the import statements in utils.py and “lead_scoring_data_pipeline.py” file.
 

16. With this, you should be able to run the data pipeline. Follow the instructions below and take a screenshot of Airflow UI for submission.

Go to airflow.cfg file and make the following changes:
Change the base_url to ‘http://localhost:6007’.
Change the web server port to 6007.
Now, run the following commands:
Run the airflow db init command to initialise the database.
Run the following command to create a user to access the database: 
airflow users create --username <argument> --firstname <argument> --lastname <argument> --role Admin --email <argument> --password <argument> 

Note: All the <argument> needs to be replaced by your input. For e.g., 
airflow users create --username upgrad --firstname upgrad --lastname upgrad --role Admin --email spiderman@superhero.org --password admin

Start the Airflow server: airflow webserver

Go to a new terminal and run the command to start the scheduler: airflow scheduler
Now go to the Jarvis dashboard and select the appropriate API Endpoint. Enter the credentials of the user that you created above. With this, you will see your Airflow dashboard.
