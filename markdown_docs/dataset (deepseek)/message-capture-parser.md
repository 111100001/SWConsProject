## ClassDef ProgressBar
**ProgressBar**: The function of ProgressBar is to visually represent the progress of a task in the terminal using a dynamic progress bar.

**attributes**: The attributes of this Class.
· total: A float value representing the total amount of work to be completed.
· running: A float value representing the amount of work that has been completed so far.

**Code Description**: The ProgressBar class is designed to provide a visual representation of progress for tasks such as file processing. It initializes with a total amount of work (`total`) and tracks the completed work (`running`). The `set_progress` method calculates the progress percentage and displays a progress bar in the terminal. The progress bar consists of `#` characters representing completed work and spaces representing remaining work. The `update` method increments the `running` attribute by a specified amount and updates the progress bar accordingly.

In the project, the ProgressBar class is used in conjunction with the `process_file` function to track the progress of reading and processing binary message capture files. The `process_file` function reads the file in chunks and updates the progress bar after each chunk is processed. The `main` function initializes the ProgressBar with the total size of all files to be processed and passes it to `process_file` for each file. This ensures that the progress bar accurately reflects the overall progress of the task.

**Note**: 
1. The progress bar is only displayed if the terminal width is greater than 12 columns. If the terminal is too narrow, the progress bar will not be shown.
2. The progress bar is updated dynamically in the terminal, overwriting the previous progress bar display. This requires the terminal to support carriage return (`\r`) for proper rendering.
3. The progress bar is designed for tasks where the total amount of work is known in advance, such as processing files of known sizes.

**Output Example**: 
When the progress bar is displayed, it might look like this in the terminal:
```
[ #########################                          ]  65%
```
This indicates that 65% of the task has been completed. The `#` characters represent completed work, and the spaces represent remaining work. The percentage is displayed to the right of the progress bar.
### FunctionDef __init__(self, total)
**__init__**: The function of __init__ is to initialize an instance of the ProgressBar class with a total value and set the initial running progress to zero.

**parameters**: The parameters of this Function.
· total: A float value representing the total progress or the maximum value that the progress bar will track.

**Code Description**: The __init__ method is the constructor for the ProgressBar class. It takes a single parameter, `total`, which is a float representing the total amount of progress to be tracked. Upon initialization, the method assigns this `total` value to the instance variable `self.total`. Additionally, it initializes another instance variable, `self.running`, to zero. This `self.running` variable is intended to keep track of the current progress as the task progresses.

**Note**: Ensure that the `total` parameter is set to a meaningful value that accurately represents the total progress to be tracked. The `self.running` variable should be updated appropriately during the execution of the task to reflect the current progress.
***
### FunctionDef set_progress(self, progress)
**set_progress**: The function of set_progress is to visually display the progress of a task in the terminal using a progress bar.

**parameters**: The parameters of this Function.
· progress: A float value representing the current progress of the task, typically ranging from 0 to 1, where 0 indicates no progress and 1 indicates completion.

**Code Description**: The `set_progress` function is designed to update and display a progress bar in the terminal. It first retrieves the terminal's width using `shutil.get_terminal_size()[0]` to determine how many columns are available for the progress bar. If the terminal width is too small (less than or equal to 12 columns), the function exits early without displaying anything. Otherwise, it calculates the maximum number of blocks that can be displayed in the progress bar by subtracting 9 from the terminal width (to leave space for the percentage display). The number of blocks to be filled is determined by multiplying the maximum blocks by the `progress` parameter. The progress bar is then printed to the terminal using a formatted string, where filled blocks are represented by `#` and empty blocks by spaces. The percentage of completion is displayed alongside the progress bar. The `end=''` argument in the `print` function ensures that the cursor remains on the same line, allowing for smooth updates to the progress bar.

This function is primarily called by the `update` method of the `ProgressBar` class, which updates the progress based on the amount of work completed (`more`) relative to the total work (`total`). It is also called in the `main` function to set the progress bar to 100% when the task is complete.

**Note**: The progress bar will not be displayed if the terminal width is too small (less than or equal to 12 columns). Additionally, the function assumes that the terminal supports carriage return (`\r`) for updating the same line.

**Output Example**: 
```
[ #########################                          ]  75%
```
This output represents a progress bar that is 75% complete, with filled blocks (`#`) and empty blocks (` `) visually indicating the progress. The percentage is displayed at the end of the bar.
***
### FunctionDef update(self, more)
**update**: The function of update is to increment the progress of a task by a specified amount and update the progress bar accordingly.

**parameters**: The parameters of this Function.
· more: A float value representing the amount of progress to be added to the current progress. This value is typically a fraction of the total work to be done.

**Code Description**: The `update` function is a method of the `ProgressBar` class and is used to increment the progress of a task. It takes a single parameter, `more`, which specifies the amount of progress to be added. The function updates the `running` attribute of the `ProgressBar` instance by adding the value of `more` to it. After updating the `running` attribute, the function calls the `set_progress` method, passing the ratio of `running` to `total` as the progress value. This ratio represents the current progress of the task, where `running` is the amount of work completed so far, and `total` is the total amount of work to be done.

The `update` function is primarily used in conjunction with the `process_file` function, where it is called to update the progress bar as the file is being processed. In `process_file`, the `update` function is called with the difference in the file pointer position (`diff`) to reflect the progress of reading and processing the file. This ensures that the progress bar is updated in real-time as the file is being read, providing visual feedback to the user.

The `set_progress` method, which is called by `update`, is responsible for visually displaying the progress bar in the terminal. It calculates the number of blocks to be filled based on the current progress and prints the progress bar to the terminal. The `update` function, therefore, serves as a bridge between the task's progress and the visual representation of that progress.

**Note**: The `update` function assumes that the `running` and `total` attributes of the `ProgressBar` instance are properly initialized before it is called. Additionally, the `more` parameter should be a positive value representing a meaningful increment in progress. If `more` is zero or negative, the progress bar will not advance, and the visual representation will remain unchanged.
***
## FunctionDef to_jsonable(obj)
**to_jsonable**: The function of to_jsonable is to convert a given object into a JSON-serializable format. This function handles various types of objects, including those with `__dict__` or `__slots__` attributes, lists, bytes, and other basic types.

**parameters**: The parameters of this Function.
· obj: The object to be converted into a JSON-serializable format. This can be any type of object, including custom classes, lists, bytes, or basic data types.

**Code Description**: 
The `to_jsonable` function is designed to recursively convert an object into a format that can be easily serialized into JSON. It first checks if the object has a `__dict__` attribute, which is common in custom classes. If so, it returns the object's `__dict__`, which contains all the object's attributes as key-value pairs. If the object does not have a `__dict__` but has `__slots__`, the function iterates over the slots, retrieves the corresponding values, and converts them into a dictionary. Special handling is applied for certain slots that contain integers or lists of integers, which are converted into hexadecimal strings using the `ser_uint256` function.

If the object is a list, the function recursively applies `to_jsonable` to each element in the list. If the object is of type `bytes`, it converts the bytes into a hexadecimal string. For all other types, the function returns the object as-is, assuming it is already JSON-serializable.

This function is called within the `process_file` function, where it is used to convert the body of a deserialized message into a JSON-serializable format before appending it to the `messages` list. This ensures that the message data can be easily serialized into JSON for further processing or output.

**Note**: 
- The function assumes that objects with `__slots__` have attributes that can be safely accessed using `getattr`. If an object has private or protected slots, this function may not handle them correctly.
- The function does not handle circular references, which could lead to infinite recursion if present in the object structure.
- The `ser_uint256` function is used to convert integers into hexadecimal strings, but its implementation is not provided in the code snippet.

**Output Example**: 
Given an object with the following structure:
```python
class Example:
    __slots__ = ['a', 'b', 'c']
    def __init__(self):
        self.a = 123
        self.b = [456, 789]
        self.c = b'hello'
```
The output of `to_jsonable(Example())` would be:
```python
{
    'a': '7b',  # 123 in hexadecimal
    'b': ['1c8', '315'],  # [456, 789] in hexadecimal
    'c': '68656c6c6f'  # 'hello' in hexadecimal
}
```
## FunctionDef process_file(path, messages, recv, progress_bar)
**process_file**: The function of process_file is to read and parse a binary message capture file, extract message headers and bodies, and store the processed data in a list of dictionaries.

**parameters**: The parameters of this Function.
· path: A string representing the file path of the binary message capture file to be processed.
· messages: A list where the processed message data will be stored. Each message is represented as a dictionary.
· recv: A boolean flag indicating whether the messages in the file are received (True) or sent (False).
· progress_bar: An optional ProgressBar object used to visually track the progress of file processing. If provided, the progress bar will be updated as the file is read.

**Code Description**: 
The `process_file` function is responsible for reading and parsing binary message capture files. It opens the file in binary mode and processes it in chunks. The function reads the file header, which contains metadata such as the message timestamp, type, and length. It then reads the message body based on the length specified in the header. The function converts the message into a dictionary format, which includes the message direction (received or sent), timestamp, size, type, and body. If the message type is unrecognized or the message body cannot be deserialized, the function logs a warning and appends an error message to the dictionary.

The function uses the `to_jsonable` helper function to convert the message body into a JSON-serializable format. This ensures that the message data can be easily serialized into JSON for further processing or output. The processed message dictionaries are appended to the `messages` list, which is passed as a parameter.

If a `progress_bar` is provided, the function updates it after reading each chunk of the file. This provides real-time feedback on the progress of file processing. The progress bar is updated based on the difference in the file pointer position before and after reading each chunk.

The `process_file` function is called by the `main` function, which handles command-line arguments, initializes the progress bar, and processes multiple message capture files. The `main` function sorts the processed messages by timestamp and outputs them in JSON format, either to a file or to the standard output.

**Note**: 
- The function assumes that the binary message capture file follows a specific format, with headers containing timestamp, message type, and length fields.
- The function handles unrecognized message types and deserialization errors gracefully by logging warnings and including error information in the output.
- The progress bar is only updated if it is provided and if the file is being read in chunks. If the progress bar is not provided, the function processes the file without visual feedback.
## FunctionDef main
**main**: The function of main is to parse binary message capture files, process their contents, and output the results in JSON format either to a file or to the standard output.

**parameters**: The parameters of this Function.
· capturepaths: A list of file paths pointing to the binary message capture files to be parsed.
· output: An optional argument specifying the output file path where the JSON results will be saved. If not provided, the results are printed to stdout.
· no-progress-bar: An optional flag to disable the progress bar during file processing. The progress bar is automatically disabled if the output is not a terminal.

**Code Description**: 
The `main` function serves as the entry point for parsing binary message capture files. It begins by setting up an argument parser using `argparse.ArgumentParser`, which handles command-line arguments. The parser is configured with a description, an example usage, and three arguments: `capturepaths`, `output`, and `no-progress-bar`. The `capturepaths` argument accepts one or more file paths, while the `output` argument specifies the destination file for the JSON output. The `no-progress-bar` flag allows users to disable the progress bar, which is useful for non-interactive environments.

Once the arguments are parsed, the function converts the provided file paths into absolute paths using `Path.cwd()`. It also determines whether to display a progress bar based on the `no-progress-bar` flag and whether the output is a terminal (`sys.stdout.isatty()`). If a progress bar is enabled, the total size of all input files is calculated to initialize the `ProgressBar` object.

The function then iterates over each file path in `capturepaths` and processes the file using the `process_file` function. This function reads the binary message capture file, extracts message headers and bodies, and stores the processed data in a list of dictionaries (`messages`). The `process_file` function also updates the progress bar if it is enabled.

After all files are processed, the messages are sorted by their timestamp using the `sort` method. If a progress bar is active, it is set to 100% completion. Finally, the messages are serialized into a JSON string using `json.dumps`. If an output file is specified, the JSON string is written to the file; otherwise, it is printed to the standard output.

**Note**: 
- The progress bar is only displayed if the output is a terminal and the `no-progress-bar` flag is not set. This ensures compatibility with non-interactive environments.
- The function assumes that the binary message capture files follow a specific format, as handled by the `process_file` function.
- The output JSON is sorted by message timestamps, providing a chronological view of the captured messages.
