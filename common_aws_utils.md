# common_aws_utils package

## Submodules

## common_aws_utils.cf_util module

Class : cf_util
Purpose      : It is a common util class that contains all the cloudformation template related code


### class common_aws_utils.cf_util.CFUtil(region_name)
Bases: `object`

Common class for having utility functions for cloudformation


#### create_resource(stack_name, template, parameter_local, role)
This function will create the resource based on cloudformation and other details


* **Parameters**

    
    * **stack_name** (*string*) – stack name of the cloudformation


    * **template** (*template data*) – template data containing yaml content


    * **parameter_local** (*map*) – parameter data containing inputs needed for cloudformation


    * **role** (*string*) – iam role needed for creating


    * **region_name** (*string*) – region name for the stack



* **Returns**

    None if success, else it will throw error



#### delete_resource(stack_name)
This function will delete the resource based on cloudformation and other details
:param stack_name: stack name of the cloudformation
:type stack_name: string


* **Returns**

    None if success, else it will throw error



#### parse_template(template)
This function will parse the cloudformation template and look for any discrepancies
After that it will return the Body of the template
:param template: template data containing yaml content
:type template: template data


* **Returns**

    Parsed template body contents



* **Return type**

    template_data



#### stack_exists(stack_name)
This method checks if stack is already existing or getting processed

    Args:

        stack_name(stack_name)  : Stack name containing the cloudformation stack

    Returns:

        status : True if it is running, False if not running

## common_aws_utils.common_util module

Class : common_util
Purpose      : It is a common util class having all the common functions that can be reused


### class common_aws_utils.common_util.CommonUtil()
Bases: `object`

Common class for having utility functions


#### addvalue(parameter_local, addval)
# Adding new values to parameter_local

    Args:

        parameter_local(list) : list containining cf param values
        input_field(string) : has the input field

    Returns:

        parameter_local(list) : list containining cf param output values


#### check_for_null(input_data, input_field)
This function will check for null and make it empty if null
:param input_data: has the input data elements
:type input_data: dictionary
:param input_field: has the input field
:type input_field: string


* **Returns**

    Returns the string back as empty if null



* **Return type**

    output_string



#### dissect_properties(module_name, folder, file_name)
Method that goes through the config file. Then forms a dictionary of section and key/values pair
:param module_name: python module which contains the config file
:type module_name: string
:param folder: folder that contains the config file
:type folder: string
:param file_name: name of the config file
:type file_name: string


* **Returns**

    dictionary of items containing the section based key/value pair



* **Return type**

    config_dict(string)



#### form_multiple_s3_url(ops_bucket, path_list)
This function will form the full path for a file in S3 or other service and return as comma seperated values
. For eg, this is one path that would be formed

> s3://doc-artemis-ops-106/artemis-core/glue/library-scripts/ArtemisSparkSession.py,
> s3://doc-artemis-ops-106/artemis-core/lambda/lambda-functions/gnupg.py
> Args:

> > ops_bucket(string) : s3 bucket name
> > path_list(string) : list of python files used for framing the path

> Returns:

>     full_url: Returns the full formed url


#### form_s3_url(bucket_name, yaml_path)
This function will form the full path for a file in S3 or other service.
For eg, this is one path that would be formed
[https://doc-artemis-ops-106.s3.amazonaws.com/artemis-core/nested-templates/dynamoTable.yaml](https://doc-artemis-ops-106.s3.amazonaws.com/artemis-core/nested-templates/dynamoTable.yaml)
:param bucket_name: Name of the bucket
:type bucket_name: string
:param yaml_path: path of the yaml file including bucket details (eg artemis/glue.yaml)
:type yaml_path: string


* **Returns**

    Returns the full formed url



* **Return type**

    full_url



#### parameter_pass(params, parameter_data)
# Filtering Required Parameters from parameter_data to create parameter_local


#### run_unix_cmd(args_list)
This function will run the unix command that is being sent and return the results
eg implementation :

> (output, errors)= run_cmd([‘find’,path,’-name’,’

> ```
> *
> ```

> .csv’,’-type’,’f’])
> file_list = output.decode(“utf-8”)


* **Parameters**

    **args_list** (*dictionary*) – has the array containing the command to be run



* **Returns**

    Returns the result from the run



* **Return type**

    output


## common_aws_utils.dynamo_util module

Class : dynamo_util
Purpose      : It is a common util class that contains all the dynamodb integration components


### class common_aws_utils.dynamo_util.DynamoUtil(region_name)
Bases: `object`

Common class for having utility functions for dynamodb


#### put_item(table_name, table_items)
This method will update the item in dynamodb table with the items that are shared
PutItem will Replace an entire item including any new columns that were added/removed based on primary key

> Args:

>     table_name (String) : Name of the table which needs to be updated
>     table_items (map) : Map of items containing key value pair which needs to be updated

> Returns:

>     None


#### query_table(table_name, key_dictionary)
This method will query the table based on the key sent

    Args:

        table_name (String) : Name of the table which needs to be updated
        key_dictionary (dict): dict containing key value pair for table key

    Returns:

        scanned_result : Scanned item data


#### scan_item(table_name, scan_items)
This method will scan the table and return the items that are requested

    Args:

        table_name (String) : Name of the table which needs to be updated
        scan_items (Array) : List of items that are requested for scan

    Returns:

        scanned_result : Scanned item data


#### scan_table(table_name, limit=1, last_eval_key=None)
This method will scan the full table and return the items that are requested

    Args:

        table_name (String) : Name of the table which needs to be updated
        limit (Array) : List of items that are requested for scan
        last_eval_key (string) : Last evaluated primary key , if empty it will not be considered

    Returns:

        scanned_result : Scanned item data

## common_aws_utils.logging_util module

Logging utility module

This module serves as a utility module for an opinionated logging approach to avoid having to create the same
boilerplate logging code over and over again.

When using the functionality in this module the following approach should be used:

In the main module the function `initialize_logging` should be called either in the entry function or on top
of the module. By handing over the root module to this function a default location for logging_config.ini can
be used in a config folder inside of that root module.
The function `initialize_logging` will create a trace log level and corresponding log methods and read logging
configuration from a file, either in the above mentioned config folder, or, if that is supposed to be overridden
with custom logging_config.ini from the location where the python code is called.

If a class is defined this class should be decorated with the `log_and_trace` decorator. This will provide a
_logger attribute to the class, which can then be used for logging in its method. Further more it will wrap all
class methods with trace logging.

If module level functions are used these should be decorated with the `trace` decorator if there could be a
potential need for trace logging of these functions.
Apart from that, to be able to use a logger within these functions, a logger should be gotten via the standard
logging.getLogger(__name__) approach on top of the module (after the logging intialization mentioned above).


### common_aws_utils.logging_util.add_trace_loglevel()
Adds a TRACE loglevel with value 5 and a trace method to the
Logger class.


### common_aws_utils.logging_util.get_logging_config_path(root_module: module)
Returns the logging config file path based on the following rules:

> 
> * The filename is expected to be logging_config.ini


> * If a config can be found in the current directory this will be used
> to enable custom configuration.


> * If no custom config is found the config file is expected to be in the
> root module under the folder config.


* **Parameters**

    **root_module** (*ModuleType*) – Root module of the calling package



* **Raises**

    **IOError** – If no config file can be found an IOError is raised



* **Returns**

    Config file path as string



* **Return type**

    str



### common_aws_utils.logging_util.initialize_logging(root_module: module)
Initializes logging based on the standard for Artemis.

    It creates a new trace log level and initializes log configuration
    based on a configuration file when one can be found.


* **Parameters**

    **root_module** (*ModuleType*) – Root module of the calling package. Used to
    determine config file location.



### common_aws_utils.logging_util.log_and_trace()
Decorator for classes that will add an initialized private attribute

    _logger of type logging.Logger to the class. Apart from that it
    creates a new log level TRACE and wraps each class function with
    trace logging of the arguments and return values.

    Note: Can only be used on classes, will raise an Exception if used
    on a function.


* **Returns**

    The decorator function for the class decorated with @log_and_trace



* **Return type**

    function



### common_aws_utils.logging_util.trace(logger: logging.Logger)
Decorator for functions that adds trace logging with the function arguments

    before the actual function is called and that intercepts the return
    value of the function to create a trace log of that before actually
    returning.


* **Parameters**

    **logger** (*logging.Logger*) – Logger to be used for the trace logging.



### common_aws_utils.logging_util.trace_all_methods(cls)
Decorates all class functions with the @traced decorator

## common_aws_utils.s3_util module

Class : s3_util
Purpose      : It is a common util class that contains all the s3 integration code


### class common_aws_utils.s3_util.S3Util(region_name)
Bases: `object`

Common class for having utility functions for s3


#### s3_get_object(bucket_name, path, s3_filename)
This function will read a ddl file and then execute the query from it
:param bucket_name: s3 location where the query results will be stored
:type bucket_name: string
:param path: s3 location where the query results will be stored
:type path: string
:param s3_filename: Name of the s3 file
:type s3_filename: string


* **Raises**

    **Exception** – Error executing the query



* **Returns**

    object details



* **Return type**

    s3_object



#### upload_file_to_s3(local_dir, bucket_name, local_directory)
This function will place the file in the relative parent path that was sent in the argument

    If local_dir = ‘/usr/local/data’  and local_directory = ‘data’, it will place all the filers/folders
    from ‘data’ in to s3://<bucket_name>/data/

    ```
    *
    ```

    ….

params:

    
    * local_dir - os path for input files


    * bucket_name - s3 bucket name


    * local_directory - parent directory under which the folders have to be placed in s3

## Module contents
