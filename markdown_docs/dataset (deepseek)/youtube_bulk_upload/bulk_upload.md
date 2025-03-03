## ClassDef VideoPrivacyStatus
**VideoPrivacyStatus**: The function of VideoPrivacyStatus is to define and manage the privacy status options for YouTube video uploads within the project.

**attributes**: The attributes of this Class.
· PUBLIC: Represents the "public" privacy status for YouTube videos, making the video accessible to everyone.
· PRIVATE: Represents the "private" privacy status for YouTube videos, restricting access to only the uploader and users explicitly granted permission.
· UNLISTED: Represents the "unlisted" privacy status for YouTube videos, making the video accessible only to users who have the direct link.

**Code Description**: 
The `VideoPrivacyStatus` class is an enumeration (Enum) that defines the possible privacy statuses for YouTube videos. It provides three options: `PUBLIC`, `PRIVATE`, and `UNLISTED`, each corresponding to a specific privacy setting on YouTube. These options are used throughout the project to ensure that the privacy status of uploaded videos is correctly set and validated.

In the project, this class is primarily used in the `YouTubeBulkUpload` class and the `YouTubeBulkUploaderGUI` class. In `YouTubeBulkUpload`, the `privacy_status` parameter is initialized with the value `VideoPrivacyStatus.PRIVATE.value` by default, ensuring that videos are uploaded as private unless specified otherwise. The `validate_input_parameters` method in `YouTubeBulkUpload` checks if the provided `privacy_status` is valid by comparing it against the values defined in `VideoPrivacyStatus`. If the provided status is not one of the valid options, an exception is raised.

In the `YouTubeBulkUploaderGUI` class, the `privacy_status_var` is initialized with the value `VideoPrivacyStatus.PRIVATE.value`, and a dropdown menu is created in the GUI to allow users to select the desired privacy status from the available options. This ensures that the privacy status is correctly set based on user input before the upload process begins.

**Note**: 
- When using the `VideoPrivacyStatus` class, ensure that the privacy status value is correctly passed and validated to avoid errors during the video upload process.
- The default privacy status is set to `PRIVATE`, but this can be changed by the user either through the GUI or by directly modifying the `privacy_status` parameter in the `YouTubeBulkUpload` class.
- Always ensure that the privacy status aligns with the intended audience and access level for the uploaded videos.
## ClassDef YouTubeBulkUpload
**YouTubeBulkUpload**: The function of YouTubeBulkUpload is to automate the bulk uploading of video files to YouTube, including handling metadata such as titles, descriptions, thumbnails, and privacy settings, while providing options for user interaction and validation.

**attributes**: The attributes of this Class.
· youtube_client_secrets_file: Path to the YouTube API client secrets file for authentication.
· logger: Optional logger instance for logging messages. If not provided, a default logger is created.
· dry_run: Boolean flag to enable dry-run mode, where no actual uploads are performed.
· interactive_prompt: Boolean flag to enable interactive prompts for user confirmation during the upload process.
· stop_event: Optional threading event to stop the upload process if triggered.
· gui: Optional GUI object for handling user interactions in a graphical interface.
· source_directory: Directory path where video files are located. Defaults to the current working directory.
· input_file_extensions: List of file extensions for video files to be uploaded.
· upload_batch_limit: Maximum number of videos to upload in a single batch.
· youtube_category_id: YouTube category ID for the uploaded videos. Defaults to "10" (Music).
· youtube_keywords: List of keywords for YouTube video metadata.
· youtube_description_template_file: Path to a template file for YouTube video descriptions.
· youtube_description_replacements: List of replacement patterns for the description template.
· youtube_title_prefix: Prefix to add to YouTube video titles.
· youtube_title_suffix: Suffix to add to YouTube video titles.
· youtube_title_replacements: List of replacement patterns for video titles.
· thumbnail_filename_prefix: Prefix to add to thumbnail filenames.
· thumbnail_filename_suffix: Suffix to add to thumbnail filenames.
· thumbnail_filename_replacements: List of replacement patterns for thumbnail filenames.
· thumbnail_filename_extensions: List of file extensions for thumbnail images.
· privacy_status: Privacy status for uploaded videos (e.g., private, public, unlisted).
· check_for_duplicate_titles: Boolean flag to check for duplicate video titles on the YouTube channel.
· progress_callback_func: Optional callback function to report upload progress.

**Code Description**: 
The YouTubeBulkUpload class is designed to streamline the process of uploading multiple video files to YouTube. It handles authentication with the YouTube API, validates input parameters, and processes video files for upload. The class supports customization of video metadata, including titles, descriptions, and thumbnails, through various replacement patterns and templates. It also provides options for user interaction, such as confirmation prompts and progress updates.

The class integrates with both CLI and GUI interfaces, as seen in the project structure. The CLI interface (cli.py) initializes the class with command-line arguments, while the GUI interface (gui.py) uses it to handle user inputs and trigger the upload process. The class also includes methods for validating the YouTube client secrets file, authenticating with YouTube, and checking for duplicate video titles on the user's channel.

Key methods include:
- `find_input_files`: Scans the source directory for video files with specified extensions.
- `prompt_user_confirmation_or_raise_exception`: Prompts the user for confirmation or raises an exception if the user declines.
- `validate_input_parameters`: Validates input parameters, such as the existence of the description template file and the validity of the privacy status.
- `authenticate_youtube`: Authenticates with the YouTube API using OAuth 2.0 and returns a YouTube service object.
- `upload_video_to_youtube_with_title_thumbnail`: Uploads a video to YouTube with the specified title, description, and thumbnail.
- `process`: Main method that orchestrates the entire upload process, including file discovery, metadata generation, and upload execution.

The class is designed to be flexible, allowing users to customize the upload process through various parameters and options. It also includes safeguards, such as dry-run mode and duplicate title checking, to prevent unintended uploads.

**Note**: 
- Ensure the YouTube client secrets file is valid and accessible for authentication.
- Use dry-run mode to test the upload process without actually uploading videos.
- Interactive prompts can be disabled for fully automated operation, but this may result in skipped uploads if issues arise.
- The class supports batch uploading, but be mindful of YouTube's daily upload limits.

**Output Example**: 
The `process` method returns a list of dictionaries, each containing details of the uploaded videos:
```python
[
    {
        "input_filename": "/path/to/video1.mp4",
        "youtube_title": "My Video Title 1",
        "youtube_id": "abc123xyz",
        "youtube_url": "https://www.youtube.com/watch?v=abc123xyz"
    },
    {
        "input_filename": "/path/to/video2.mp4",
        "youtube_title": "My Video Title 2",
        "youtube_id": "def456uvw",
        "youtube_url": "https://www.youtube.com/watch?v=def456uvw"
    }
]
```
### FunctionDef __init__(self, youtube_client_secrets_file, logger, dry_run, interactive_prompt, stop_event, gui, source_directory, input_file_extensions, upload_batch_limit, youtube_category_id, youtube_keywords, youtube_description_template_file, youtube_description_replacements, youtube_title_prefix, youtube_title_suffix, youtube_title_replacements, thumbnail_filename_prefix, thumbnail_filename_suffix, thumbnail_filename_replacements, thumbnail_filename_extensions, privacy_status, check_for_duplicate_titles, progress_callback_func)
**__init__**: The function of __init__ is to initialize the YouTubeBulkUpload class with necessary configurations and settings for bulk uploading videos to YouTube.

**parameters**: The parameters of this Function.
· youtube_client_secrets_file: A string representing the path to the YouTube client secrets file, which is required for authentication with the YouTube API.
· logger: An optional logging.Logger object for logging information during the initialization process. If not provided, a default logger will be created.
· dry_run: A boolean flag indicating whether the upload process should be simulated without actually uploading videos. Default is False.
· interactive_prompt: A boolean flag indicating whether to prompt the user for confirmation during the upload process. Default is True.
· stop_event: An optional event object that can be used to stop the upload process externally.
· gui: An optional GUI object that can be used to interact with a graphical user interface.
· source_directory: A string representing the directory from which video files will be sourced. Default is the current working directory.
· input_file_extensions: An iterable of strings representing the file extensions of video files to be uploaded. Default includes common video formats like .mp4, .mov, etc.
· upload_batch_limit: An integer representing the maximum number of videos to be uploaded in a single batch. Default is 100.
· youtube_category_id: A string representing the YouTube category ID for the uploaded videos. Default is "10" (Music).
· youtube_keywords: An iterable of strings representing the keywords to be associated with the uploaded videos. Default is ["music"].
· youtube_description_template_file: An optional string representing the path to a template file for video descriptions.
· youtube_description_replacements: An optional iterable of iterables representing replacement patterns for the video description template.
· youtube_title_prefix: An optional string representing a prefix to be added to video titles.
· youtube_title_suffix: An optional string representing a suffix to be added to video titles.
· youtube_title_replacements: An optional iterable of iterables representing replacement patterns for video titles.
· thumbnail_filename_prefix: An optional string representing a prefix to be added to thumbnail filenames.
· thumbnail_filename_suffix: An optional string representing a suffix to be added to thumbnail filenames.
· thumbnail_filename_replacements: An optional iterable of iterables representing replacement patterns for thumbnail filenames.
· thumbnail_filename_extensions: An iterable of strings representing the file extensions of thumbnail images. Default includes .png, .jpg, and .jpeg.
· privacy_status: A string representing the privacy status of the uploaded videos. Default is "private".
· check_for_duplicate_titles: A boolean flag indicating whether to check for duplicate video titles before uploading. Default is True.
· progress_callback_func: An optional callback function to report progress during the upload process.

**Code Description**: The __init__ method initializes the YouTubeBulkUpload class by setting up the necessary configurations and validating the provided parameters. It first checks if a logger is provided; if not, it creates a default logger with a predefined logging level and formatter. The method then validates the YouTube client secrets file using the `validate_secrets_file` method, ensuring that the file exists and is valid JSON. After validation, it authenticates with the YouTube API using the `authenticate_youtube` method, which returns a YouTube service object.

The method logs various initialization parameters, including the source directory, input file extensions, and privacy status, to provide transparency and debugging information. It sets up the class attributes based on the provided parameters, such as the source directory, file extensions, YouTube category ID, keywords, and privacy status. Additionally, it configures optional settings like title and description templates, thumbnail filename patterns, and progress callback functions.

The `privacy_status` parameter is initialized with the value `VideoPrivacyStatus.PRIVATE.value` by default, ensuring that videos are uploaded as private unless specified otherwise. The method also sets up flags for dry run, interactive prompts, and duplicate title checks, which control the behavior of the upload process.

**Note**: 
- Ensure that the `youtube_client_secrets_file` points to a valid file containing the necessary OAuth 2.0 credentials for YouTube API access.
- The `dry_run` flag is useful for testing the upload process without actually uploading videos to YouTube.
- The `interactive_prompt` flag allows users to confirm each upload step, providing an additional layer of control.
- The `privacy_status` parameter should be set according to the desired visibility of the uploaded videos, with options including "public", "private", and "unlisted".
- The `check_for_duplicate_titles` flag helps prevent uploading videos with identical titles, which could cause confusion or errors in the YouTube library.
***
### FunctionDef find_input_files(self)
**find_input_files**: The function of find_input_files is to locate and return a list of video files in the specified source directory that match the allowed file extensions.

**parameters**: The function does not take any explicit parameters. It relies on the instance attributes `self.source_directory` and `self.input_file_extensions` to determine the directory to search and the file extensions to filter by.

**Code Description**: 
The `find_input_files` function is responsible for scanning the directory specified by `self.source_directory` and identifying all files that match the allowed file extensions defined in `self.input_file_extensions`. It uses `os.listdir` to list all files in the directory and filters them based on their extensions. If no matching files are found, the function logs an error and raises an `Exception` to indicate that no valid video files are available for upload. If matching files are found, the function logs the number of files discovered and returns a list of their full paths.

This function is critical in the workflow of the `YouTubeBulkUpload` class, as it is called by the `process` method to retrieve the list of video files to be uploaded. The `process` method relies on the output of `find_input_files` to proceed with the upload process. Additionally, the function is tested in the `YouTubeBulkUploadTest` class to ensure it behaves correctly in scenarios where no files are found, invalid files are present, or valid files are detected.

**Note**: 
- Ensure that `self.source_directory` is set to a valid directory path before calling this function.
- The `self.input_file_extensions` attribute should contain a list of valid file extensions (e.g., `['.mp4', '.mov']`) to filter the files correctly.
- If no files matching the specified extensions are found, the function will raise an `Exception`, which should be handled appropriately by the caller.

**Output Example**: 
```python
['/path/to/source_directory/video1.mp4', '/path/to/source_directory/video2.mov']
```
***
### FunctionDef prompt_user_confirmation_or_raise_exception(self, prompt_message, exit_message, allow_empty)
**prompt_user_confirmation_or_raise_exception**: The function of prompt_user_confirmation_or_raise_exception is to prompt the user for confirmation and raise an exception if the user declines the confirmation.

**parameters**: The parameters of this Function.
· prompt_message: A string that contains the message or question to be displayed to the user, asking for confirmation.
· exit_message: A string that contains the error message to be logged and raised as an exception if the user declines the confirmation.
· allow_empty: A boolean flag that determines whether an empty input is considered a valid response. Defaults to False.

**Code Description**: The function `prompt_user_confirmation_or_raise_exception` is designed to ensure that the user explicitly confirms an action before proceeding. It leverages the `prompt_user_bool` function to interact with the user and obtain a boolean response. If the user responds with a negative or invalid input (i.e., `False` is returned by `prompt_user_bool`), the function logs the provided `exit_message` as an error using the logger and raises an exception with the same message. This ensures that the operation is halted if the user does not confirm the action.

The function is used in scenarios where user confirmation is critical before proceeding with an operation. For example, in the `determine_thumbnail_filepath` method, it is used to confirm whether the user is willing to proceed without a thumbnail file. If the user declines, the function raises an exception, effectively canceling the operation. This behavior is also tested in the test cases `test_prompt_user_confirmation_or_raise_exception_raises_Exception` and `test_prompt_user_confirmation_or_raise_exception_returns_None_if_user_bool_is_True`, which verify that the function raises an exception when the user declines and returns `None` when the user confirms, respectively.

**Note**: 
- The function relies on the `prompt_user_bool` function to handle the actual user interaction, which supports both GUI and CLI environments.
- If `allow_empty` is set to True, the user can confirm the action by simply pressing Enter without typing anything.
- The function is designed to enforce user confirmation in critical operations, ensuring that unintended actions are avoided.
***
### FunctionDef prompt_user_bool(self, prompt_message, allow_empty)
**prompt_user_bool**: The function of prompt_user_bool is to prompt the user for a boolean (yes/no) response, either through a graphical user interface (GUI) or a command-line interface (CLI), depending on the availability of a GUI.

**parameters**: The parameters of this Function.
· prompt_message: A string that contains the message or question to be displayed to the user.
· allow_empty: A boolean flag that determines whether an empty input is considered a valid response. Defaults to False.

**Code Description**: The function `prompt_user_bool` is designed to interact with the user to obtain a boolean response. It first checks if a GUI is available (`self.gui` is not `None`). If a GUI is available, it triggers the GUI's `prompt_user_bool` method, which displays a prompt to the user. The function then waits for the user to provide input or close the dialog by waiting on the `user_input_event`. Once the event is set, the function retrieves the user's input from `user_input_result` and returns it.

If no GUI is available, the function defaults to a CLI-based interaction. It constructs a string of acceptable responses based on the `allow_empty` parameter. If `allow_empty` is True, an empty input is considered a valid response. The function then prints the prompt message and waits for the user to input a response. The response is stripped of leading/trailing whitespace and converted to lowercase. The function returns `True` if the response matches any of the acceptable responses (e.g., "y", "yes", or an empty string if allowed), otherwise it returns `False`.

In the project, this function is used in various scenarios where user confirmation is required. For example, it is called in `prompt_user_confirmation_or_raise_exception` to confirm actions before proceeding, in `check_if_video_title_exists_on_youtube_channel` to confirm whether a found video matches the user's expectations, and in `determine_youtube_title` to confirm the generated title for a video. It is also used in the `process` method to confirm upload details before proceeding with the upload.

**Note**: 
- When using the CLI, the function is case-insensitive for user input (e.g., "Y", "y", "YES", "yes" are all treated as affirmative responses).
- If `allow_empty` is set to True, pressing Enter without typing anything will be treated as an affirmative response.
- The function assumes that the GUI's `prompt_user_bool` method and related attributes (`user_input_event` and `user_input_result`) are properly implemented and synchronized.

**Output Example**: 
- If the user inputs "y" or "yes" (or presses Enter when `allow_empty` is True), the function returns `True`.
- If the user inputs "n" or any other invalid response, the function returns `False`.

This function is a critical part of the interactive workflow in the project, ensuring that user input is consistently and reliably captured across both GUI and CLI environments.
***
### FunctionDef prompt_user_text(self, prompt_message, default_response)
**prompt_user_text**: The function of prompt_user_text is to prompt the user for text input, either through a graphical user interface (GUI) or a command-line interface (CLI), depending on the availability of a GUI.

**parameters**: The parameters of this Function.
· prompt_message: A string that represents the message displayed to the user when prompting for input.
· default_response: A string that represents the default response provided to the user. This parameter is optional and defaults to an empty string.

**Code Description**: The prompt_user_text function is designed to handle user input in two different scenarios: when a GUI is available and when it is not. If a GUI is available (i.e., self.gui is not None), the function triggers the GUI's prompt_user_text method, passing the prompt_message and default_response as arguments. The function then waits for the user to provide input or close the dialog by waiting on the user_input_event. Once the event is set, the function retrieves the user's input from the gui.user_input_result and returns it.

If no GUI is available, the function falls back to using the built-in input function to prompt the user for input via the command line. The prompt_message is displayed to the user, and the function returns the user's input directly.

In the project, this function is used in two main contexts: determining the YouTube title and description for a video file. In both cases, if the interactive_prompt flag is set, the function is called to allow the user to confirm or modify the automatically generated title or description. This ensures that the user has the final say in what is uploaded to YouTube, enhancing the flexibility and user-friendliness of the bulk upload process.

**Note**: When using this function, ensure that the GUI object (if available) is properly initialized and that the user_input_event and user_input_result attributes are correctly set up to handle the user's input. Additionally, when using the CLI fallback, be aware that the input function will block the program's execution until the user provides input.

**Output Example**: 
If the user inputs "My Custom Title" when prompted, the function will return:
"My Custom Title"

If the user does not provide any input and the default_response is "Default Title", the function will return:
"Default Title"
***
### FunctionDef validate_input_parameters(self)
**validate_input_parameters**: The function of validate_input_parameters is to validate the input parameters required for the YouTube bulk upload process, ensuring that all necessary files and settings are correctly configured before proceeding with the upload.

**parameters**: The parameters of this Function.
· self: Refers to the instance of the YouTubeBulkUpload class, allowing access to its attributes and methods.

**Code Description**: The `validate_input_parameters` method performs several critical checks to ensure that the input parameters for the YouTube bulk upload process are valid and correctly configured. It begins by logging the start of the validation process and the current working directory. 

The method first checks if a YouTube description template file is provided. If no file is provided, a warning is logged, indicating that the video description will be empty unless entered interactively. If a file is provided, the method verifies that the file exists. If the file does not exist, an exception is raised, halting the process and notifying the user of the missing file. If the file exists, its existence is logged for confirmation.

Next, the method validates the privacy status of the video. It checks if the provided privacy status is one of the valid options defined in the `VideoPrivacyStatus` enumeration (i.e., `PUBLIC`, `PRIVATE`, or `UNLISTED`). If the privacy status is invalid, an exception is raised, informing the user of the invalid value. This ensures that the privacy status is correctly set before the upload process begins.

Finally, if all checks pass, the method logs a debug message indicating that the YouTube upload checks have been successfully completed. This method is called within the `process` method of the `YouTubeBulkUpload` class to ensure that all input parameters are validated before proceeding with the video upload process.

**Note**: 
- Ensure that the YouTube description template file, if provided, exists and is accessible. Otherwise, the upload process will fail.
- The privacy status must be one of the valid options (`PUBLIC`, `PRIVATE`, or `UNLISTED`). Any other value will result in an exception.
- This method is essential for preventing errors during the upload process by validating critical input parameters early in the workflow.
***
### FunctionDef validate_secrets_file(cls, logger, secrets_file)
**validate_secrets_file**: The function of validate_secrets_file is to validate the existence and JSON format of a YouTube client secrets file.

**parameters**: The parameters of this Function.
· logger: A logging.Logger object used to log information or errors during the validation process.
· secrets_file: A string representing the file path of the YouTube client secrets file to be validated.

**Code Description**: 
The validate_secrets_file function performs two main checks on the provided YouTube client secrets file. First, it verifies that the file exists by checking if the file path is not None and if the file exists on the filesystem using os.path.isfile. If the file does not exist, the function raises an exception with a descriptive message indicating that the file is missing.

Second, the function attempts to parse the file as JSON to ensure it is valid. It opens the file in read mode with UTF-8 encoding and uses json.load to parse the contents. If the file is successfully parsed as JSON, the function logs a message confirming the file's validity. If the file is not valid JSON, the function raises an exception with a message indicating that the file is not valid JSON.

This function is called during the initialization of the YouTubeBulkUpload class to ensure that the provided YouTube client secrets file is valid before proceeding with further operations. It is also tested in the YouTubeBulkUploadAuthenticationTest class, where various scenarios are simulated, including valid JSON, invalid JSON, and missing file cases, to ensure the function behaves as expected.

**Note**: 
- Ensure that the secrets_file parameter points to a valid file path before calling this function.
- The function assumes that the file, if it exists, is encoded in UTF-8. If the file uses a different encoding, the function may fail to read it correctly.
- The function raises exceptions with descriptive messages, which should be handled appropriately by the caller to provide meaningful feedback to the user.
***
### FunctionDef authenticate_youtube(cls, logger, youtube_client_secrets_file)
**authenticate_youtube**: The function of authenticate_youtube is to authenticate and return a YouTube service object. If the service is started for the first time or the refresh token is expired or revoked, a browser window will open so the user can authenticate manually.

**parameters**: The parameters of this Function.
· logger: A logging.Logger object used for logging authentication-related information.
· youtube_client_secrets_file: A string representing the path to the client secrets file, which contains the OAuth 2.0 credentials required for authentication.

**Code Description**: 
The `authenticate_youtube` function is a class method designed to handle the authentication process for accessing the YouTube API. It first checks for existing credentials stored in a pickle file located in the system's temporary directory. If the file exists and contains valid credentials, the function loads and uses them. If the credentials are invalid, expired, or missing, the function initiates a manual authentication process by calling the `open_browser_to_authenticate` method.

The `open_browser_to_authenticate` method triggers a browser-based OAuth 2.0 flow, allowing the user to authenticate manually. Once new credentials are obtained, they are saved to the pickle file for future use. The function then builds and returns a YouTube service object using the `build` function from the Google API client library, which is initialized with the authenticated credentials.

This function is primarily called during the initialization of the `YouTubeBulkUpload` class, where it ensures that the application has valid credentials to interact with the YouTube API. The function is also tested in unit tests, such as `test_authenticate_youtube_with_valid_token` and `test_authenticate_youtube_with_new_auth`, to verify its behavior under different scenarios, such as when valid credentials are present or when new authentication is required.

**Note**: 
- Ensure that the `youtube_client_secrets_file` provided is valid and contains the correct OAuth 2.0 credentials.
- The function may open a browser window for manual authentication, so it should be used in environments where a browser is accessible.
- Handle exceptions appropriately when calling this function, as authentication failures can disrupt the application's functionality.

**Output Example**: 
The function returns a YouTube service object, which can be used to interact with the YouTube API. An example of the return value might look like this:
```python
<googleapiclient.discovery.Resource object at 0x7f8b1c2d3a90>
```
This object provides methods for accessing various YouTube API endpoints, such as uploading videos, managing playlists, and retrieving video metadata.
***
### FunctionDef open_browser_to_authenticate(cls, secrets_file)
**open_browser_to_authenticate**: The function of open_browser_to_authenticate is to trigger browser-based authentication for YouTube API access and return new credentials.

**parameters**: The parameters of this Function.
· secrets_file: A string representing the path to the client secrets file, which contains the OAuth 2.0 credentials required for authentication.

**Code Description**: 
The `open_browser_to_authenticate` function is a class method designed to handle the authentication process for accessing the YouTube API. It uses the `InstalledAppFlow` class from the Google API client library to initiate the OAuth 2.0 flow. The function reads the client secrets from the specified `secrets_file`, which contains the necessary OAuth 2.0 credentials such as the client ID and client secret. The `scopes` parameter is set to `["https://www.googleapis.com/auth/youtube"]`, which grants the application access to the YouTube API.

The function then calls `run_local_server(port=0)` on the `flow` object, which starts a local server and opens a browser window for the user to authenticate manually. The `port=0` argument allows the system to automatically select an available port. Once the user completes the authentication process, the function returns the obtained credentials.

If any exception occurs during the authentication process, the function raises a `RuntimeError` with the message "Re-authentication failed," ensuring that the calling function is aware of the failure.

This function is primarily called by the `authenticate_youtube` method within the same class. The `authenticate_youtube` method checks for existing credentials and, if they are invalid or expired, calls `open_browser_to_authenticate` to obtain new credentials. The new credentials are then saved for future use, ensuring that the application can continue to access the YouTube API without requiring manual authentication every time.

**Note**: 
- Ensure that the `secrets_file` provided is valid and contains the correct OAuth 2.0 credentials.
- The function requires user interaction via a browser window, so it should be used in environments where a browser is accessible.
- Handle exceptions appropriately when calling this function, as authentication failures can disrupt the application's functionality.

**Output Example**: 
The function returns an instance of `Credentials` or `Creds`, which contains the OAuth 2.0 tokens required for accessing the YouTube API. An example of the return value might look like this:
```python
<google.oauth2.credentials.Credentials object at 0x7f8b1c2d3a90>
```
This object includes attributes such as `token`, `refresh_token`, and `expiry`, which are used to authenticate API requests.
***
### FunctionDef get_channel_id(self)
**get_channel_id**: The function of get_channel_id is to retrieve the authenticated user's YouTube channel ID using the YouTube Data API.

**parameters**: The function does not take any parameters.

**Code Description**: 
The `get_channel_id` function is designed to fetch the channel ID of the authenticated user's YouTube channel. It achieves this by making a request to the YouTube Data API using the `youtube.channels().list()` method with the `part="snippet"` and `mine=True` parameters. This request retrieves information about the authenticated user's channel. The function then executes the request and processes the response.

If the response contains an "items" key, the function extracts the channel ID from the first item in the list and returns it. If the "items" key is not present in the response, the function returns `None`, indicating that no channel ID was found.

This function is used in other parts of the project, such as in the `check_if_video_title_exists_on_youtube_channel` method, where it retrieves the channel ID to search for videos on the user's YouTube channel. Additionally, the function is tested in the `YouTubeBulkUploadTest` class, where two test cases verify its behavior: one checks if the function returns a valid channel ID, and the other ensures it returns `None` when no items are found in the API response.

**Note**: 
- Ensure that the YouTube API client (`self.youtube`) is properly authenticated before calling this function, as it relies on the `mine=True` parameter to fetch the authenticated user's channel.
- The function assumes that the API response structure is consistent with the YouTube Data API documentation. Any changes in the API response structure may require updates to this function.

**Output Example**: 
- If the channel ID is successfully retrieved, the function might return a value like `"UC1234567890ABCDEF"`.
- If no channel ID is found, the function will return `None`.
***
### FunctionDef check_if_video_title_exists_on_youtube_channel(self, youtube_title)
**check_if_video_title_exists_on_youtube_channel**: The function of check_if_video_title_exists_on_youtube_channel is to check if a video with a specific title already exists on the authenticated user's YouTube channel.

**parameters**: The parameters of this Function.
· youtube_title: A string representing the title of the video to be checked for existence on the YouTube channel.

**Code Description**: 
The `check_if_video_title_exists_on_youtube_channel` function is designed to search for a video with a specific title on the authenticated user's YouTube channel. It begins by retrieving the channel ID using the `get_channel_id` method. Once the channel ID is obtained, the function constructs a search request using the YouTube Data API's `search().list()` method, specifying the channel ID, the video title as the search query, and limiting the results to 10 videos.

The function then executes the search request and processes the response. If the response contains any items, the function iterates through the list of videos and compares the titles of the found videos with the provided `youtube_title` using a fuzzy string matching algorithm (`fuzz.ratio`). If the similarity score between the titles is 70% or higher, the function considers it a potential match. 

If a potential match is found, the function logs the details of the match, including the video ID and the similarity score. If the `interactive_prompt` flag is enabled, the function prompts the user to confirm whether the found video matches the one they are trying to upload. If the user confirms the match, the function returns the video ID of the matching video. If the `interactive_prompt` flag is disabled, the function automatically returns the video ID of the matching video.

If no matching video is found, the function logs a message indicating that no match was found and returns `None`, allowing the upload process to continue.

This function is called in the `process` method of the `YouTubeBulkUpload` class, where it is used to check for duplicate video titles before uploading a new video to the YouTube channel. It is also tested in the `YouTubeBulkUploadTest` class, where various scenarios are simulated, including finding a match, not finding a match, and handling user interaction through the interactive prompt.

**Note**: 
- The function relies on the `fuzz.ratio` method from the `fuzzywuzzy` library to compare video titles. A similarity score of 70% or higher is considered a match.
- The function assumes that the YouTube API client (`self.youtube`) is properly authenticated and that the `get_channel_id` method returns a valid channel ID.
- The `interactive_prompt` flag determines whether the function will prompt the user for confirmation when a potential match is found. If disabled, the function will automatically return the video ID of the matching video.

**Output Example**: 
- If a matching video is found and confirmed by the user (or if `interactive_prompt` is disabled), the function might return a value like `"dQw4w9WgXcQ"`.
- If no matching video is found, the function will return `None`.
***
### FunctionDef truncate_to_nearest_word(self, title, max_length)
**truncate_to_nearest_word**: The function of truncate_to_nearest_word is to truncate a given title string to the nearest whole word while ensuring it does not exceed a specified maximum length. If truncation occurs, an ellipsis ("...") is appended to indicate the truncation.

**parameters**: The parameters of this Function.
· title: A string representing the title that needs to be truncated.
· max_length: An integer specifying the maximum allowed length for the title.

**Code Description**: 
The `truncate_to_nearest_word` function is designed to handle situations where a title string exceeds a specified maximum length. It ensures that the title is truncated cleanly at the nearest whole word boundary, avoiding mid-word cuts. The function first checks if the length of the title is already within the allowed limit. If so, it returns the title unchanged. If the title exceeds the limit, it truncates the title to the specified `max_length` and then further truncates it to the nearest whole word by splitting the string at the last space character. If the resulting truncated title is shorter than the `max_length`, an ellipsis ("...") is appended to indicate that the title has been shortened.

The function is called within the `determine_youtube_title` method of the `YouTubeBulkUpload` class, where it is used to ensure that YouTube video titles do not exceed a maximum length of 95 characters. This is particularly important for maintaining readability and compliance with YouTube's title length restrictions. The function is also tested in the `YouTubeBulkUploadTest` class, where its behavior is verified under different scenarios, such as when the title is within the limit, exceeds the limit, or exactly matches the limit.

**Note**: 
- The function assumes that the input title is a valid string and that `max_length` is a positive integer.
- If the title contains no spaces and exceeds the `max_length`, the function will truncate it directly at the `max_length` without adding an ellipsis.
- The ellipsis is only added if the truncated title is shorter than the `max_length`, ensuring that the final title does not exceed the specified limit.

**Output Example**: 
For a title "This is a sample video title for testing purposes" and a `max_length` of 20, the function might return "This is a sample ...".
***
### FunctionDef upload_video_to_youtube_with_title_thumbnail(self, video_file, youtube_title, youtube_description, thumbnail_filepath)
**upload_video_to_youtube_with_title_thumbnail**: The function of upload_video_to_youtube_with_title_thumbnail is to upload a video to YouTube with a specified title, description, and optional thumbnail, and return the YouTube video ID of the uploaded video.

**parameters**: The parameters of this Function.
· video_file: A string representing the file path of the video to be uploaded.
· youtube_title: A string representing the title of the video on YouTube.
· youtube_description: A string representing the description of the video on YouTube.
· thumbnail_filepath: An optional string representing the file path of the thumbnail image for the video. If not provided, no thumbnail will be uploaded.

**Code Description**: 
The `upload_video_to_youtube_with_title_thumbnail` function is responsible for uploading a video to YouTube with metadata such as title, description, and an optional thumbnail. The function begins by logging the start of the upload process. If the `dry_run` flag is enabled, the function simulates the upload process by logging the details of what would be uploaded and returns a mock video ID ("dry-run-video-id").

In the actual upload scenario (when `dry_run` is False), the function constructs a request body containing the video's metadata, including the title, description, tags, category ID, and privacy status. The video file is then prepared for upload using `MediaFileUpload`, which supports resumable uploads with a specified chunk size. The function calls the YouTube API's `videos().insert()` method to initiate the upload, and the upload progress is monitored using a chunked upload mechanism. If a `progress_callback_func` is provided, it is called periodically to report the upload progress.

Once the video is successfully uploaded, the function retrieves the YouTube video ID from the response and constructs the video URL. If a thumbnail file is provided, the function uploads the thumbnail using the YouTube API's `thumbnails().set()` method. Finally, the function resets the progress callback to 0 and returns the YouTube video ID.

This function is called by the `process` method in the `YouTubeBulkUpload` class, which handles the bulk upload of multiple videos. The `process` method determines the title, description, and thumbnail for each video and then calls `upload_video_to_youtube_with_title_thumbnail` to perform the actual upload. The function is also tested in various scenarios, including dry-run mode, actual uploads with and without thumbnails, and handling of duplicate titles.

**Note**: 
- Ensure that the `dry_run` flag is set appropriately to avoid unintended uploads during testing or development.
- The function relies on the YouTube API and requires proper authentication and API setup.
- The `progress_callback_func` can be used to monitor upload progress, but it is optional.
- If the upload fails, the function does not handle retries or error recovery explicitly.

**Output Example**: 
The function returns a string representing the YouTube video ID of the uploaded video. For example:
```
"abc123xyz"
```
***
### FunctionDef determine_thumbnail_filepath(self, video_file)
**determine_thumbnail_filepath**: The function of determine_thumbnail_filepath is to determine the filepath of a thumbnail associated with a given video file by applying filename modifications and checking for the existence of the thumbnail file.

**parameters**: The parameters of this Function.
· video_file: A string representing the filepath of the video for which the thumbnail filepath needs to be determined.

**Code Description**: The `determine_thumbnail_filepath` function is designed to locate a thumbnail file associated with a given video file. It begins by logging the process of determining the thumbnail filepath. The function then extracts the base filename of the video file (without its extension) and applies any specified filename modifications, including a prefix, suffix, and replacement patterns. These modifications are controlled by the instance attributes `thumbnail_filename_prefix`, `thumbnail_filename_suffix`, and `thumbnail_filename_replacements`, respectively.

After modifying the filename, the function iterates through a list of possible thumbnail file extensions (`thumbnail_filename_extensions`) to check if a corresponding thumbnail file exists. If a matching file is found, its filepath is returned. If no matching file is found and the `interactive_prompt` attribute is enabled, the function prompts the user to confirm whether they wish to proceed without a thumbnail. This is done by calling the `prompt_user_confirmation_or_raise_exception` function, which will raise an exception if the user declines to proceed. If the user confirms or if `interactive_prompt` is disabled, the function returns `None`, indicating that no thumbnail file was found.

This function is used within the `process` method of the `YouTubeBulkUpload` class to determine the thumbnail filepath for each video file before uploading it to YouTube. It ensures that the correct thumbnail is associated with the video, or that the user is informed and given the option to proceed without a thumbnail if none is found.

**Note**: 
- The function relies on the `thumbnail_filename_prefix`, `thumbnail_filename_suffix`, and `thumbnail_filename_replacements` attributes to modify the thumbnail filename. These attributes should be set appropriately before calling this function.
- The `interactive_prompt` attribute controls whether the user is prompted to confirm proceeding without a thumbnail. If set to `True`, the function will interact with the user; otherwise, it will silently return `None` if no thumbnail is found.
- The function uses regular expressions to apply filename replacements, so ensure that the `thumbnail_filename_replacements` attribute contains valid pattern-replacement pairs.

**Output Example**: 
- If a thumbnail file is found, the function might return: `/path/to/video_thumbnail.png`
- If no thumbnail file is found and the user confirms to proceed without one, the function returns: `None`
***
### FunctionDef determine_youtube_title(self, video_file)
**determine_youtube_title**: The function of determine_youtube_title is to generate a YouTube-compatible title for a given video file, applying optional prefixes, suffixes, replacements, and truncation rules, while allowing user confirmation in interactive mode.

**parameters**: The parameters of this Function.
· video_file: A string representing the name of the video file for which the YouTube title is being generated.

**Code Description**: 
The `determine_youtube_title` function is responsible for crafting a YouTube-compatible title for a video file. It begins by logging the process of generating the title for the specified video file. The function extracts the base title from the video file name by removing the file extension using `os.path.splitext`.

Next, the function applies an optional YouTube title prefix if `self.youtube_title_prefix` is set. This prefix is prepended to the base title. Similarly, if `self.youtube_title_suffix` is set, it appends the suffix to the title. These prefixes and suffixes allow for consistent formatting across multiple video titles.

The function then checks if `self.youtube_title_replacements` is set. If so, it iterates through the list of replacement patterns and applies them to the title. Each pattern and its corresponding replacement are logged for debugging purposes. This feature is useful for standardizing or modifying parts of the title, such as replacing specific words or phrases.

After applying prefixes, suffixes, and replacements, the function ensures the title does not exceed YouTube's maximum length requirements. It calls the `truncate_to_nearest_word` method to truncate the title to a maximum of 95 characters, ensuring the truncation occurs at the nearest whole word boundary. If truncation is necessary, an ellipsis ("...") is appended to indicate the title has been shortened.

If `self.interactive_prompt` is enabled, the function prompts the user to confirm the generated title. It uses the `prompt_user_bool` method to ask the user if they are satisfied with the title. If the user is not satisfied, the function calls `prompt_user_text` to allow the user to manually input a new title, with the generated title provided as the default response.

The function is called within the `process` method of the `YouTubeBulkUpload` class, where it is used to generate titles for videos being uploaded to YouTube. It is also tested in the `YouTubeBulkUploadTest` class to ensure it correctly applies prefixes, suffixes, replacements, and truncation, and handles user interaction as expected.

**Note**: 
- The function assumes that the input video file name is a valid string and that the optional attributes (`youtube_title_prefix`, `youtube_title_suffix`, `youtube_title_replacements`) are properly initialized if used.
- The interactive prompt feature (`interactive_prompt`) allows users to override the generated title, providing flexibility in the upload process.
- The function ensures compliance with YouTube's title length restrictions by truncating titles to a maximum of 95 characters.

**Output Example**: 
For a video file named "sample_video.mp4", with a prefix "My Channel: ", a suffix " - 2023", and a replacement pattern that changes "video" to "clip", the function might return:
"My Channel: sample_clip - 2023"

If the title exceeds 95 characters, it might return:
"My Channel: sample_clip with a very long title that has been truncat..."
***
### FunctionDef determine_youtube_description(self, video_file, youtube_title)
**determine_youtube_description**: The function of determine_youtube_description is to generate a YouTube description for a video file by applying a template and replacement patterns, or by prompting the user for input if no template is available.

**parameters**: The parameters of this Function.
· video_file: A string representing the path to the video file for which the YouTube description is being determined.
· youtube_title: A string representing the title of the YouTube video, which may be used in the description if specified in the replacement patterns.

**Code Description**: The determine_youtube_description function is responsible for generating a YouTube description for a given video file. It first checks if a YouTube description template file is specified (self.youtube_description_template_file). If a template file exists, the function reads its content into the description variable. 

Next, the function checks if any replacement patterns are defined (self.youtube_description_replacements). If replacements are specified, the function iterates through each pattern and applies the replacements to the description. Notably, the function supports a Jinja-like syntax for replacements, allowing the user to include the YouTube title in the description by using the placeholder `{{youtube_title}}`. The function replaces this placeholder with the actual YouTube title provided as an argument.

If no description is generated from the template and replacements, and if the interactive_prompt flag is enabled, the function logs a warning and prompts the user to manually input a description using the prompt_user_text function. This ensures that a description is always available, even if no template or replacements are provided.

The function is called within the process method of the YouTubeBulkUpload class, where it is used to determine the description for each video file being processed. This ensures that each video uploaded to YouTube has a properly formatted description, either automatically generated or manually provided by the user.

**Note**: When using this function, ensure that the YouTube description template file (if used) is properly formatted and that any replacement patterns are correctly defined. Additionally, if the interactive_prompt flag is enabled, be prepared to provide a description manually if no template is available.

**Output Example**: 
If the template file contains "This is a video about {{youtube_title}}." and the replacement pattern replaces `{{youtube_title}}` with "My Video Title", the function will return:
"This is a video about My Video Title."

If no template is available and the user inputs "This is a custom description." when prompted, the function will return:
"This is a custom description."
***
### FunctionDef process(self)
**process**: The function of process is to handle the bulk upload of video files to YouTube, including validation, file processing, and upload execution, while managing user interaction and error handling.

**parameters**: The parameters of this Function.
· self: Refers to the instance of the YouTubeBulkUpload class, allowing access to its attributes and methods.

**Code Description**: The `process` method is the core function responsible for orchestrating the bulk upload of video files to YouTube. It begins by checking if the `dry_run` flag is enabled, which simulates the upload process without performing any actual uploads. If enabled, a warning is logged to inform the user that no actions will be executed.

The method then validates the input parameters by calling the `validate_input_parameters` method, ensuring that all required files and settings are correctly configured before proceeding. This step is crucial to prevent errors during the upload process.

Next, the method retrieves a list of video files to be uploaded by calling the `find_input_files` method. This method scans the specified source directory and returns a list of video files that match the allowed file extensions. If no files are found, an exception is raised, and the process is halted.

For each video file in the list, the method performs the following steps:
1. Checks if a `stop_event` is set, which allows the process to be interrupted if necessary.
2. Verifies if the upload batch limit has been reached, stopping the process if the limit is exceeded.
3. Determines the YouTube title, description, and thumbnail filepath for the video using the `determine_youtube_title`, `determine_youtube_description`, and `determine_thumbnail_filepath` methods, respectively.
4. If duplicate title checking is enabled, the method calls `check_if_video_title_exists_on_youtube_channel` to ensure that a video with the same title does not already exist on the YouTube channel. If a duplicate is found, the video is skipped.
5. If interactive prompts are enabled, the method prompts the user to confirm the upload details using the `prompt_user_bool` method. If the user declines, the video is skipped.
6. If all checks pass, the video is uploaded to YouTube using the `upload_video_to_youtube_with_title_thumbnail` method. The method returns the YouTube video ID, which is then added to the list of uploaded videos.

If any errors occur during the upload process, they are logged, and the video file is recorded in a text file named `failed_uploads.txt` for further investigation.

Finally, the method returns a list of dictionaries, each containing details of the uploaded videos, including the input filename, YouTube title, YouTube ID, and YouTube URL.

**Note**: 
- Ensure that the `dry_run` flag is set appropriately to avoid unintended uploads during testing or development.
- The method relies on several other methods (`validate_input_parameters`, `find_input_files`, `determine_youtube_title`, etc.) to perform its tasks. Ensure these methods are properly implemented and configured.
- The `stop_event` can be used to interrupt the upload process if needed, providing flexibility in managing long-running upload tasks.
- The method handles errors gracefully by logging them and recording failed uploads, allowing for easy troubleshooting.

**Output Example**: 
The method returns a list of dictionaries, each representing an uploaded video. For example:
```python
[
    {
        "input_filename": "/path/to/video1.mp4",
        "youtube_title": "My Video Title",
        "youtube_id": "abc123xyz",
        "youtube_url": "https://www.youtube.com/watch?v=abc123xyz"
    },
    {
        "input_filename": "/path/to/video2.mp4",
        "youtube_title": "Another Video Title",
        "youtube_id": "def456uvw",
        "youtube_url": "https://www.youtube.com/watch?v=def456uvw"
    }
]
```
***
