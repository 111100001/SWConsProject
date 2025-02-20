## FunctionDef key_value_to_dict(lst)
**key_value_to_dict**: The function of key_value_to_dict is to convert a list of key-value pair strings into a Python dictionary.

**parameters**: The parameters of this Function.
· lst: A list of strings, where each string is formatted as "key:value". If a single string is provided instead of a list, it will be converted into a list containing that string.

**Code Description**: The key_value_to_dict function processes a list of strings, each representing a key-value pair separated by a colon. It first checks if the input is a list; if not, it wraps the input string in a list. The function then initializes a defaultdict of lists to store the results. As it iterates through each item in the list, it splits the string into a key and a value. An assertion ensures that a value is present for each key. If a key already exists in the result and the value is not already associated with that key, the value is appended to the list for that key. If the key does not exist, it initializes a new list with the value. 

After processing all items, the function converts any lists containing a single item back into a string, ensuring that the output dictionary has a consistent format. The final output is a dictionary where each key maps to either a single value or a list of values, depending on how many values were associated with that key.

This function is called within the main function of the tubeup/__main__.py module, specifically when parsing command-line arguments. The metadata argument, which is expected to be a string or a list of strings formatted as key-value pairs, is passed to key_value_to_dict. The resulting dictionary is then used to provide metadata for the TubeUp instance, which handles the archiving of URLs. This shows that key_value_to_dict plays a crucial role in transforming user input into a structured format that can be utilized by other components of the application.

**Note**: It is important to ensure that the input strings are correctly formatted as "key:value" pairs. If any value is missing for a key, an assertion error will be raised.

**Output Example**: 
Given an input list like `["title:My Video", "description:This is a test video", "tags:video,test", "tags:example"]`, the function would return:
```python
{
    "title": "My Video",
    "description": "This is a test video",
    "tags": ["video", "test", "example"]
}
```
## FunctionDef sanitize_identifier(identifier, replacement)
**sanitize_identifier**: The function of sanitize_identifier is to sanitize a given identifier by replacing any illegal characters with a specified replacement character.

**parameters**: The parameters of this Function.
· parameter1: identifier - A string that represents the identifier to be sanitized.
· parameter2: replacement - A string that specifies the character to replace illegal characters with. The default value is a hyphen ('-').

**Code Description**: The sanitize_identifier function utilizes a regular expression to identify and replace any character in the input string that is not a word character (alphanumeric or underscore) or a hyphen. This is achieved through the use of the re.sub function, which substitutes all occurrences of the matched pattern with the specified replacement character. 

This function is particularly useful in scenarios where identifiers need to conform to specific formatting rules, such as when generating URLs or file names that cannot contain certain special characters. 

In the project, sanitize_identifier is called within the get_itemname function, which constructs an identifier by concatenating the 'extractor' and 'display_id' (or 'id') fields from an input dictionary. The resulting string is then sanitized to ensure it adheres to the required format. 

Additionally, the function is tested in the UtilsTest class, where it is used to verify that valid identifiers remain unchanged and that invalid identifiers are correctly sanitized. The tests ensure that the function behaves as expected across a range of inputs, confirming its reliability in maintaining identifier integrity.

**Note**: It is important to ensure that the replacement character does not introduce ambiguity in the identifier, especially if the identifier is used in contexts where unique identification is critical.

**Output Example**: For an input of 'twitch:vod-v181464551', the function would return 'twitch-vod-v181464551' when using the default replacement character '-'.
## FunctionDef get_itemname(infodict)
**get_itemname**: The function of get_itemname is to construct a sanitized identifier for an item based on the provided information dictionary.

**parameters**: The parameters of this Function.
· parameter1: infodict - A dictionary containing information about the item, specifically the 'extractor' and either 'display_id' or 'id'.

**Code Description**: The get_itemname function takes a single parameter, infodict, which is expected to be a dictionary containing metadata about an item. The function constructs a string by concatenating the value associated with the 'extractor' key and the value associated with either the 'display_id' key or the 'id' key from the infodict. If 'display_id' is not present, it defaults to using 'id'. 

This concatenated string is formatted with a hyphen ('-') separating the two values. The resulting string is then passed to the sanitize_identifier function, which sanitizes the identifier by replacing any illegal characters with a specified replacement character (defaulting to a hyphen). This ensures that the identifier conforms to the required formatting rules, making it suitable for use in contexts such as URLs or file names.

The get_itemname function is called in multiple locations within the project. For instance, in the check_if_ia_item_exists function, get_itemname is used to generate the item name before checking if the item already exists in the Internet Archive. Similarly, in the upload_ia method, get_itemname is invoked to create the item name based on the video metadata before proceeding with the upload process. This highlights the function's role in ensuring that identifiers are consistently formatted and valid across different operations involving item management.

**Note**: It is important to ensure that the infodict passed to get_itemname contains the necessary keys ('extractor', 'display_id', or 'id') to avoid potential KeyError exceptions. Proper handling of these keys is crucial for the function to operate correctly.

**Output Example**: For an input dictionary like {'extractor': 'twitch', 'display_id': 'vod-v181464551'}, the function would return 'twitch-vod-v181464551' after sanitization.
## FunctionDef check_is_file_empty(filepath)
**check_is_file_empty**: The function of check_is_file_empty is to determine whether a specified file is empty or not.

**parameters**: The parameters of this Function.
· filepath: Path of a file that will be checked.

**Code Description**: The check_is_file_empty function checks if a file at the given filepath exists and whether it is empty. It utilizes the os module to verify the existence of the file and to retrieve its size. If the file exists, the function returns True if the file size is zero, indicating that the file is empty. If the file does not exist, the function raises a FileNotFoundError with a message indicating that the specified path does not exist.

This function is called in various test cases within the tests/test_utils.py file. Specifically, it is used in three test methods: 
1. test_check_is_file_empty_when_file_is_empty: This test creates an empty file and asserts that the function correctly identifies it as empty.
2. test_check_is_file_empty_when_file_is_not_empty: This test creates a non-empty file and asserts that the function correctly identifies it as not empty.
3. test_check_is_file_empty_when_file_doesnt_exist: This test checks the function's behavior when a non-existent file path is provided, expecting a FileNotFoundError to be raised.

The check_is_file_empty function is also utilized in the upload_ia method of the TubeUp class. In this context, it is used to check if certain files, such as a description file and an annotations file, are empty before deciding to delete them. This ensures that only relevant files are uploaded to the Internet Archive, preventing unnecessary empty files from being processed.

**Note**: It is important to ensure that the filepath provided to the function is valid and that the file exists to avoid raising a FileNotFoundError.

**Output Example**: 
- If the file is empty: True
- If the file is not empty: False
- If the file does not exist: Raises FileNotFoundError with the message "Path 'filepath' doesn't exist"
