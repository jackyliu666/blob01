# 1、Open the target source code and determine the current version

<img width="415" height="287" alt="image" src="https://github.com/user-attachments/assets/f0691304-5627-465d-9aa5-5d0abe9a47fb" />

# 2、Locate the vulnerability file =>  crm/WeiXinApp/yunzhijia/yunzhijiaApi.php

<img width="415" height="183" alt="image" src="https://github.com/user-attachments/assets/47ee0dd5-df59-4cc0-a63c-8f5d29e87b96" />

# 3、According to the code, we can analyze that the execution process of the PHP file is as follows
## a、Get request parameters  $function = $_REQUEST["function"];

<img width="415" height="147" alt="image" src="https://github.com/user-attachments/assets/77e11dbc-badf-4f72-8b6d-e92fec2a2553" />

## b、Instantiate a class object , This class has already been defined below.
$obj = new yunzhijiaApi($root_dir,$adb); 

<img width="415" height="208" alt="image" src="https://github.com/user-attachments/assets/0b282907-3d15-48e1-bfa8-c10bee3c3e36" />


## c、Call the function of this class, and the function name is passed in from the request parameter 'function', which means we can control the function called by this class.
## d、 Find the vulnerability function delete_user in the yunzhijiaApi class; From the code, it can be seen that the request parameter user_ist is concatenated into the SQL execution statement, resulting in an SQL injection vulnerability. Since the parameter user_list is processed by the json decode function, when submitting the user_list parameter, it needs to be submitted in JSON format, such as user_st={"name": "jacky"}

<img width="379" height="237" alt="image" src="https://github.com/user-attachments/assets/49727da7-c20d-459f-af2a-277e37bd98bc" />


# 4.Vulnerability verification
## The following is the target website access page

<img width="315" height="155" alt="image" src="https://github.com/user-attachments/assets/edec5bfe-98e1-4569-b9c7-7b64d2f188f7" />

## Use the sqlmap tool to detect

python sqlmap.py -u "http://192.168.8.12:85//crm/WeiXinApp/yunzhijia/yunzhijiaApi.php?function=delete_user&user_list={%22name%22:%22jacky%27)*%22}" --level 5 --risk 3 --dbms mysql  --technique=T --time-sec 20

<img width="415" height="193" alt="image" src="https://github.com/user-attachments/assets/113e1abd-9dc7-4961-95e1-5b89ebb0a6cd" />


# The above is the complete content of this vulnerability. Thank you for your review and time!





