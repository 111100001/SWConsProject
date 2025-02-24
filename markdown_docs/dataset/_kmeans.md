## FunctionDef kmeans_plusplus(X, n_clusters)
**kmeans_plusplus**: The function of kmeans_plusplus is to initialize cluster centers for the k-means clustering algorithm using the k-means++ method.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - The data to pick seeds from.
· n_clusters: int - The number of centroids to initialize.
· sample_weight: array-like of shape (n_samples,), default=None - The weights for each observation in `X`. If `None`, all observations are assigned equal weight. `sample_weight` is ignored if `init` is a callable or a user provided array.
· x_squared_norms: array-like of shape (n_samples,), default=None - Squared Euclidean norm of each data point.
· random_state: int or RandomState instance, default=None - Determines random number generation for centroid initialization. Pass an int for reproducible output across multiple function calls.
· n_local_trials: int, default=None - The number of seeding trials for each center (except the first), of which the one reducing inertia the most is greedily chosen. Set to None to make the number of trials depend logarithmically on the number of seeds (2+log(k)) which is the recommended setting.

**Code Description**: The kmeans_plusplus function is designed to select initial cluster centers for the k-means clustering algorithm in an efficient manner. It employs the k-means++ initialization method, which significantly improves the convergence speed of the k-means algorithm by carefully selecting the initial centroids. 

The function begins by validating the input data `X` and checking that the number of samples is greater than or equal to the number of clusters specified. It also verifies the provided `sample_weight` and calculates the squared Euclidean norms of the data points if not provided. The random state is set for reproducibility of results.

The core of the function involves calling the private helper function _kmeans_plusplus, which performs the actual initialization of the centroids. This helper function selects the first centroid randomly from the dataset, and subsequent centroids are chosen based on their distance from existing centroids, with a probability proportional to the squared distance. This method ensures that points further away from existing centers have a higher chance of being selected, which is crucial for effective clustering.

The kmeans_plusplus function ultimately returns two outputs: the initialized centers and their corresponding indices in the original dataset. This function serves as a public interface for initializing cluster centers and is utilized within the k-means algorithm implementation, ensuring that the clustering process starts with a well-informed selection of initial centroids.

**Note**: It is essential to ensure that the number of samples in `X` is greater than or equal to the number of clusters specified. The function assumes that prior validation of the data has been conducted.

**Output Example**: A possible return value of the function could be:
centers: array([[10, 2],
                [1, 0]])
indices: array([3, 2])
## FunctionDef _kmeans_plusplus(X, n_clusters, x_squared_norms, sample_weight, random_state, n_local_trials)
**_kmeans_plusplus**: The function of _kmeans_plusplus is to initialize cluster centers for the k-means clustering algorithm using the k-means++ method.

**parameters**: The parameters of this Function.
· X: {ndarray, sparse matrix} of shape (n_samples, n_features) - The data to pick seeds for.
· n_clusters: int - The number of seeds to choose.
· sample_weight: ndarray of shape (n_samples,) - The weights for each observation in `X`.
· x_squared_norms: ndarray of shape (n_samples,) - Squared Euclidean norm of each data point.
· random_state: RandomState instance - The generator used to initialize the centers.
· n_local_trials: int, default=None - The number of seeding trials for each center (except the first).

**Code Description**: The _kmeans_plusplus function is a computational component designed to facilitate the initialization of cluster centers in the k-means clustering algorithm by employing the k-means++ strategy. This method is particularly advantageous as it selects initial cluster centers in a way that is expected to improve the convergence speed of the algorithm.

The function begins by determining the number of samples and features from the input data `X`. It then initializes an empty array to hold the cluster centers. If the number of local trials is not specified, it defaults to a logarithmic function of the number of clusters, which helps in selecting better initial centers.

The first cluster center is chosen randomly from the data points, weighted by the provided sample weights. The function then calculates the squared distances from this center to all other points in the dataset, which is crucial for the selection of subsequent centers. The remaining centers are chosen based on their distance from the existing centers, with a probability proportional to the squared distance, ensuring that points farther away from existing centers have a higher chance of being selected.

The function also handles sparse matrices appropriately, converting them to dense arrays when necessary. Finally, it returns the initialized centers and their corresponding indices in the original dataset.

This function is called by the kmeans_plusplus function, which serves as a public interface for initializing cluster centers. The kmeans_plusplus function performs preliminary checks on the input data and parameters before delegating the actual initialization task to _kmeans_plusplus. Additionally, it is utilized within the _init_centroids method of the _BaseKMeans class, which is responsible for computing the initial centroids based on the specified initialization method.

**Note**: It is important to ensure that the number of samples in `X` is greater than or equal to the number of clusters specified. The function assumes that prior validation of the data has been conducted.

**Output Example**: A possible return value of the function could be:
centers: array([[10, 2],
                [1, 0]])
indices: array([3, 2])
## FunctionDef _tolerance(X, tol)
**_tolerance**: The function of _tolerance is to calculate a tolerance value based on the dataset provided.

**parameters**: The parameters of this Function.
· parameter1: X - The input dataset, which can be either a dense or sparse matrix.
· parameter2: tol - A scalar value representing the tolerance factor.

**Code Description**: The _tolerance function computes a tolerance value that is dependent on the characteristics of the dataset X. If the tol parameter is set to zero, the function immediately returns zero, indicating no tolerance. For datasets represented as sparse matrices, the function calculates the variances of the features using the mean_variance_axis function, which is assumed to return both the mean and variance along the specified axis. For dense matrices, it directly computes the variances using NumPy's var function. Finally, the function returns the mean of these variances multiplied by the tol parameter, effectively scaling the tolerance based on the variability of the dataset.

This function is called within the _check_params_vs_input method of the _BaseKMeans class. In this context, it is used to set the instance variable self._tol, which represents the computed tolerance for the KMeans clustering algorithm. The _check_params_vs_input method ensures that the number of samples in the dataset is sufficient relative to the number of clusters specified. By calculating the tolerance using the _tolerance function, it helps in determining the convergence criteria for the clustering process, thereby influencing the behavior of the KMeans algorithm during its execution.

**Note**: It is important to ensure that the tol parameter is set appropriately, as a value of zero will lead to a tolerance of zero, which may affect the clustering results. Additionally, the function is designed to handle both sparse and dense datasets, making it versatile for different types of input data.

**Output Example**: For a dataset X with variances [0.5, 1.0, 1.5] and a tol value of 0.1, the function would return (0.5 + 1.0 + 1.5) / 3 * 0.1 = 0.1.
## FunctionDef k_means(X, n_clusters)
**k_means**: The function of k_means is to perform K-means clustering algorithm on a given dataset.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - The observations to cluster. The data will be converted to C ordering, which may cause a memory copy if the data is not C-contiguous.
· n_clusters: int - The number of clusters to form as well as the number of centroids to generate.
· sample_weight: array-like of shape (n_samples,), default=None - The weights for each observation in X. If None, all observations are assigned equal weight. sample_weight is not used during initialization if init is a callable or a user-provided array.
· init: {'k-means++', 'random'}, callable or array-like of shape (n_clusters, n_features), default='k-means++' - Method for initialization of cluster centers.
· n_init: 'auto' or int, default="auto" - Number of times the k-means algorithm will be run with different centroid seeds.
· max_iter: int, default=300 - Maximum number of iterations of the k-means algorithm to run.
· verbose: bool, default=False - Verbosity mode.
· tol: float, default=1e-4 - Relative tolerance with regards to Frobenius norm of the difference in the cluster centers of two consecutive iterations to declare convergence.
· random_state: int, RandomState instance or None, default=None - Determines random number generation for centroid initialization.
· copy_x: bool, default=True - When pre-computing distances, indicates whether to modify the original data.
· algorithm: {"lloyd", "elkan"}, default="lloyd" - K-means algorithm to use, either "lloyd" or "elkan".
· return_n_iter: bool, default=False - Whether or not to return the number of iterations.

**Code Description**: The k_means function serves as a high-level interface for performing K-means clustering. It accepts a dataset X and various parameters that control the clustering process. The function initializes an instance of the KMeans class with the provided parameters and calls its fit method to execute the clustering algorithm. The KMeans class is responsible for managing the clustering process, including the initialization of cluster centers, the iterative assignment of data points to clusters, and the updating of cluster centers until convergence is achieved or the maximum number of iterations is reached.

The k_means function returns the final cluster centroids, the labels indicating the closest centroid for each observation, and the inertia, which measures how tightly the clusters are packed. If the return_n_iter parameter is set to True, it also returns the number of iterations taken to reach the final clustering solution.

The relationship between k_means and its callees is straightforward: k_means acts as a user-friendly wrapper around the KMeans class, allowing users to perform clustering without needing to directly interact with the class's internal methods.

**Note**: It is essential to ensure that the input data X is properly formatted and that the parameters are set according to the desired clustering behavior. The choice of initialization method and the number of initializations can significantly impact the performance and outcome of the K-means clustering process.

**Output Example**: A possible return value from the k_means function could be:
```python
(array([[10.,  2.],
         [ 1.,  2.]]), 
 array([1, 1, 1, 0, 0, 0], dtype=int32), 
 16.0)
```
This output indicates the coordinates of the cluster centers, the cluster assignment for each sample in the input data, and the final inertia value.
## FunctionDef _kmeans_single_elkan(X, sample_weight, centers_init, max_iter, verbose, tol, n_threads)
**_kmeans_single_elkan**: The function of _kmeans_single_elkan is to perform a single run of the k-means clustering algorithm using the Elkan variant, which is optimized for speed and efficiency.

**parameters**: The parameters of this Function.
· X: {ndarray, sparse matrix} of shape (n_samples, n_features) - The observations to cluster. If a sparse matrix is provided, it must be in CSR format.
· sample_weight: array-like of shape (n_samples,) - The weights for each observation in X.
· centers_init: ndarray of shape (n_clusters, n_features) - The initial centers for the clusters.
· max_iter: int, default=300 - Maximum number of iterations of the k-means algorithm to run.
· verbose: bool, default=False - Verbosity mode to control the output of the function.
· tol: float, default=1e-4 - Relative tolerance with respect to the Frobenius norm of the difference in the cluster centers of two consecutive iterations to declare convergence.
· n_threads: int, default=1 - The number of OpenMP threads to use for the computation, allowing for parallelism on the main Cython loop.

**Code Description**: The _kmeans_single_elkan function executes a single iteration of the k-means clustering algorithm using the Elkan method, which is designed to reduce the number of distance calculations needed during the clustering process. The function begins by initializing several buffers to store intermediate results, such as the new cluster centers, weights in clusters, and labels for each sample. It checks whether the input data X is sparse or dense and sets up the appropriate functions for initialization and iteration accordingly.

The function then enters a loop that runs for a maximum of max_iter iterations. In each iteration, it updates the cluster centers and assigns labels to the samples based on their proximity to the centers. The function also checks for convergence by comparing the current labels with the previous labels and by evaluating the shift in cluster centers against the specified tolerance. If convergence is achieved, the loop breaks early.

After the iterations, the function performs a final assignment of labels to ensure that they match the updated cluster centers. It calculates the final inertia, which is the sum of squared distances from each sample to its closest cluster center, and returns the labels, inertia, final cluster centers, and the number of iterations run.

This function is called by the fit method of the KMeans class, which is responsible for computing k-means clustering. The fit method validates the input data, initializes the cluster centers, and then calls _kmeans_single_elkan to perform the clustering. The results from _kmeans_single_elkan are used to determine the best clustering configuration based on inertia and distinct clusters.

**Note**: It is important to ensure that the input data X is in the correct format (either dense or sparse) and that the sample weights are appropriately defined. The function is optimized for performance, especially when using multiple threads, which can significantly speed up the clustering process.

**Output Example**: 
An example of the return value from the function could look like this:
```python
labels = np.array([0, 1, 0, 1, 0])
inertia = 12.34
centroids = np.array([[1.0, 2.0], [3.0, 4.0]])
n_iter = 5
``` 
This output indicates the labels assigned to each sample, the final inertia value, the coordinates of the cluster centroids, and the number of iterations completed during the clustering process.
## FunctionDef _kmeans_single_lloyd(X, sample_weight, centers_init, max_iter, verbose, tol, n_threads)
**_kmeans_single_lloyd**: The function of _kmeans_single_lloyd is to perform a single run of the k-means clustering algorithm using the Lloyd's method.

**parameters**: The parameters of this Function.
· X: {ndarray, sparse matrix} of shape (n_samples, n_features) - The observations to cluster. If a sparse matrix is provided, it must be in CSR format.  
· sample_weight: ndarray of shape (n_samples,) - The weights for each observation in X.  
· centers_init: ndarray of shape (n_clusters, n_features) - The initial centers for the clusters.  
· max_iter: int, default=300 - The maximum number of iterations of the k-means algorithm to run.  
· verbose: bool, default=False - Controls the verbosity of the output during the execution of the algorithm.  
· tol: float, default=1e-4 - The relative tolerance with respect to the Frobenius norm of the difference in the cluster centers of two consecutive iterations, used to declare convergence.  
· n_threads: int, default=1 - The number of OpenMP threads to use for computation, allowing for parallel processing on the main Cython loop.

**Code Description**: The _kmeans_single_lloyd function implements the Lloyd's algorithm for k-means clustering. It begins by initializing several buffers to store the current and new cluster centers, labels for each observation, and other necessary variables. The function checks if the input data X is sparse or dense and assigns the appropriate iteration function and inertia calculation method accordingly.

The algorithm iterates up to max_iter times, updating the cluster centers and labels based on the distances of the observations to the centers. If the labels do not change between iterations, strict convergence is declared. If the change in cluster centers is below the specified tolerance (tol), convergence is also declared. The function finally calculates the inertia, which is the sum of squared distances from each observation to its closest cluster center, and returns the final labels, inertia, cluster centers, and the number of iterations run.

This function is called by the fit method of the KMeans class in the same module. During the fitting process, the KMeans class prepares the data and initializes the cluster centers before invoking _kmeans_single_lloyd to perform the clustering. The results from this function are then used to determine the best clustering configuration based on inertia and distinct clusters found.

**Note**: It is important to ensure that the input data X is properly formatted and that the sample weights are correctly specified. The function assumes that the necessary preparations have been completed prior to its invocation.

**Output Example**: A possible return value of the function could be:
- labels: array([0, 1, 0, 1, 0])  # Indicating the closest centroid for each observation
- inertia: 12.34  # The final inertia value
- centers: array([[1.5, 2.5], [3.5, 4.5]])  # Final cluster centers
- n_iter: 10  # Number of iterations run
## FunctionDef _labels_inertia(X, sample_weight, centers, n_threads, return_inertia)
**_labels_inertia**: The function of _labels_inertia is to compute the labels and the inertia of the given samples and centers in the K-means EM algorithm.

**parameters**: The parameters of this Function.
· X: {ndarray, sparse matrix} of shape (n_samples, n_features) - The input samples to assign to the labels. If sparse matrix, must be in CSR format.  
· sample_weight: ndarray of shape (n_samples,) - The weights for each observation in X.  
· centers: ndarray of shape (n_clusters, n_features) - The cluster centers.  
· n_threads: int, default=1 - The number of OpenMP threads to use for the computation. Parallelism is sample-wise on the main cython loop which assigns each sample to its closest center.  
· return_inertia: bool, default=True - Whether to compute and return the inertia.  

**Code Description**: The _labels_inertia function is a critical component of the K-means clustering algorithm, specifically designed to perform the expectation step (E step) of the K-means EM algorithm. It takes a dataset X and a set of cluster centers and computes the closest cluster for each sample in X, assigning labels accordingly. The function also calculates the inertia, which is the sum of squared distances from each sample to its nearest cluster center, if requested.

The function begins by determining the number of samples and clusters based on the input data shapes. It initializes an array to hold the labels for each sample, setting them to -1 initially. Depending on whether the input data X is a sparse matrix or a dense array, it selects the appropriate implementation for label assignment and inertia calculation.

The label assignment is performed by calling either the _labels function for sparse or dense data. After the labels are assigned, if the return_inertia parameter is set to True, the function computes the inertia using the selected inertia calculation method and returns both the labels and the inertia value. If return_inertia is False, only the labels are returned.

This function is called by other functions within the K-means implementation, such as _mini_batch_step, which utilizes _labels_inertia to first assign labels to the samples before updating the cluster centers. This relationship highlights the function's role in the overall K-means algorithm, where accurate label assignment is essential for effective clustering.

**Note**: It is important to ensure that the input data X is in the correct format (ndarray or CSR sparse matrix) and that the sample weights are appropriately defined. The function is optimized for performance with the option to utilize multiple threads for computation.

**Output Example**: A possible return value of the function when called with a dataset of 5 samples and 3 clusters might look like this:  
Labels: [0, 1, 0, 2, 1]  
Inertia: 12.34
## FunctionDef _labels_inertia_threadpool_limit(X, sample_weight, centers, n_threads, return_inertia)
**_labels_inertia_threadpool_limit**: The function of _labels_inertia_threadpool_limit is to compute the labels and inertia of the given samples and centers in a controlled thread pool context.

**parameters**: The parameters of this Function.
· X: {ndarray, sparse matrix} of shape (n_samples, n_features) - The input samples to assign to the labels. If sparse matrix, must be in CSR format.  
· sample_weight: ndarray of shape (n_samples,) - The weights for each observation in X.  
· centers: ndarray of shape (n_clusters, n_features) - The cluster centers.  
· n_threads: int, default=1 - The number of OpenMP threads to use for the computation. Parallelism is sample-wise on the main cython loop which assigns each sample to its closest center.  
· return_inertia: bool, default=True - Whether to compute and return the inertia.  

**Code Description**: The _labels_inertia_threadpool_limit function serves as a wrapper around the _labels_inertia function, specifically designed to operate within a thread pool limit context. This function ensures that the computation of labels and inertia adheres to a specified threading limit, which is particularly useful in environments where resource management is critical.

Upon invocation, the function sets the thread pool limit to 1 for the BLAS user API, ensuring that the underlying computations do not exceed this limit. It then calls the _labels_inertia function, passing along the parameters it received. The _labels_inertia function is responsible for computing the labels for each sample in the dataset X by determining the closest cluster center and, if requested, calculating the inertia, which quantifies the compactness of the clusters.

The _labels_inertia_threadpool_limit function is called by several methods within the K-means implementation, including the predict method of the _BaseKMeans class, the score method, and the fit and partial_fit methods of the MiniBatchKMeans class. In these contexts, it is used to obtain the labels for new data points or to evaluate the clustering performance on validation sets. The function's design allows it to maintain efficient resource usage while performing essential computations for the K-means algorithm.

**Note**: It is important to ensure that the input data X is in the correct format (ndarray or CSR sparse matrix) and that the sample weights are appropriately defined. The function is optimized for performance with the option to utilize multiple threads for computation, but it enforces a limit to prevent excessive resource consumption.

**Output Example**: A possible return value of the function when called with a dataset of 5 samples and 3 clusters might look like this:  
Labels: [0, 1, 0, 2, 1]  
Inertia: 12.34
## ClassDef _BaseKMeans
**_BaseKMeans**: The function of _BaseKMeans is to serve as a base class for KMeans and MiniBatchKMeans clustering algorithms, providing shared functionality and parameter validation.

**attributes**: The attributes of this Class.
· n_clusters: The number of clusters to form.
· init: Method for initialization of cluster centers.
· n_init: Number of times the k-means algorithm will be run with different centroid seeds.
· max_iter: Maximum number of iterations of the k-means algorithm for a single run.
· tol: Relative tolerance with regards to Frobenius norm of the difference in the cluster centers of two consecutive iterations to declare convergence.
· verbose: Verbosity mode.
· random_state: Determines random number generation for centroid initialization.
· _parameter_constraints: A dictionary that defines the constraints for the parameters.

**Code Description**: The _BaseKMeans class is an abstract base class that provides the foundational structure for implementing KMeans clustering algorithms. It inherits from several mixins and base classes, including ClassNamePrefixFeaturesOutMixin, TransformerMixin, ClusterMixin, BaseEstimator, and ABC, which equip it with various functionalities such as transformation, clustering, and parameter validation.

The class defines a set of parameter constraints that ensure the validity of the input parameters when initializing the clustering algorithm. The constructor initializes the parameters and performs validation checks to ensure that the number of samples in the input data is greater than or equal to the number of clusters specified.

The class includes several methods that are crucial for the clustering process:
- _check_params_vs_input: Validates the input parameters against the data provided, ensuring that the number of clusters does not exceed the number of samples and that the initialization method is appropriate.
- _validate_center_shape: Checks if the shape of the initial cluster centers is compatible with the input data and the specified number of clusters.
- _init_centroids: Computes the initial centroids based on the specified initialization method, which can be 'k-means++', 'random', or a user-defined callable or array.
- fit_predict, predict, fit_transform, and transform: These methods provide the functionality to fit the model to the data, predict cluster assignments, and transform the data into a cluster-distance space.

The _BaseKMeans class is designed to be subclassed by specific implementations of KMeans, such as the KMeans and MiniBatchKMeans classes. These subclasses inherit the core functionality of _BaseKMeans while adding specific features and optimizations relevant to their respective algorithms. For instance, KMeans implements the standard batch KMeans algorithm, while MiniBatchKMeans is optimized for large datasets by processing data in smaller batches.

**Note**: When using the _BaseKMeans class, it is essential to ensure that the input data meets the specified constraints, particularly regarding the number of samples and clusters. Additionally, users should be aware of the initialization methods and their implications on the clustering results.

**Output Example**: A possible return value from the fit_predict method could be an array of cluster labels, such as:
```
array([0, 0, 1, 1, 0, 1], dtype=int32)
``` 
This output indicates the cluster assignment for each sample in the input data, where each unique integer represents a different cluster.
### FunctionDef __init__(self, n_clusters)
**__init__**: The function of __init__ is to initialize an instance of the BaseKMeans class with specified parameters for clustering.

**parameters**: The parameters of this Function.
· n_clusters: The number of clusters to form as well as the number of centroids to generate.
· init: Method for initialization of the centroids. This can be a string or an array.
· n_init: Number of times the k-means algorithm will be run with different centroid seeds.
· max_iter: Maximum number of iterations of the k-means algorithm for a single run.
· tol: Relative tolerance with regards to inertia to declare convergence.
· verbose: Controls the verbosity of the output during the fitting process.
· random_state: Determines random number generation for centroid initialization. Use an int for reproducibility.

**Code Description**: The __init__ function serves as the constructor for the BaseKMeans class. It takes several parameters that dictate how the k-means clustering algorithm will operate. The parameter n_clusters specifies how many clusters the algorithm should identify in the dataset. The init parameter determines how the initial centroids are chosen, which can significantly affect the outcome of the clustering. The n_init parameter indicates how many times the algorithm should be executed with different initial centroid seeds to ensure a more reliable result. The max_iter parameter sets a limit on the number of iterations for each run of the algorithm, while tol provides a threshold for determining when the algorithm has converged. The verbose parameter allows users to control the level of detail in the output messages during the fitting process, which can be useful for debugging or understanding the algorithm's progress. Lastly, the random_state parameter ensures that the results can be reproduced by controlling the randomness in the initialization process.

**Note**: It is important to provide appropriate values for these parameters to achieve optimal clustering results. Users should be aware that the choice of n_clusters can significantly influence the performance and outcome of the k-means algorithm. Additionally, setting a random_state can help in obtaining consistent results across different runs.
***
### FunctionDef _check_params_vs_input(self, X, default_n_init)
**_check_params_vs_input**: The function of _check_params_vs_input is to validate the input parameters against the dataset provided to ensure that the KMeans clustering algorithm can operate correctly.

**parameters**: The parameters of this Function.
· parameter1: X - The input dataset, which is expected to be a two-dimensional array-like structure representing the samples and features.
· parameter2: default_n_init - An optional integer that specifies the default number of initializations to use if the initialization method is set to "random" or a callable.

**Code Description**: The _check_params_vs_input function performs several critical checks and assignments related to the parameters used in the KMeans clustering algorithm. 

First, it verifies that the number of samples in the dataset X (represented by X.shape[0]) is greater than or equal to the number of clusters specified by self.n_clusters. If this condition is not met, a ValueError is raised, indicating that the number of samples must be sufficient to form the requested clusters.

Next, the function calculates the tolerance value by calling the _tolerance function, passing the dataset X and the tolerance parameter self.tol. This computed tolerance is stored in the instance variable self._tol, which is essential for determining the convergence criteria during the clustering process.

The function then assesses the initialization method specified by self.init and determines the appropriate number of initializations (self._n_init) to perform. If self.n_init is set to "auto", the function checks the type of self.init. If it is a string and equals "k-means++", it sets self._n_init to 1. If it is "random", it assigns the value of default_n_init to self._n_init. If self.init is a callable, it also uses default_n_init. For any other case, including when self.init is an array-like structure, it defaults to 1.

Additionally, if self.init is an array-like structure and self._n_init is not equal to 1, a warning is issued to inform the user that only one initialization will be performed, despite the specified n_init value. This warning is raised as a RuntimeWarning, indicating that the behavior of the algorithm may differ from the user's expectations.

This function is integral to the operation of the KMeans algorithm, as it ensures that the parameters are correctly set up before the clustering process begins, thereby influencing the algorithm's performance and results.

**Note**: It is crucial to ensure that the dataset X has enough samples relative to the number of clusters specified. Additionally, the initialization method and the number of initializations should be chosen carefully, as they can significantly impact the convergence and final clustering results.
***
### FunctionDef _warn_mkl_vcomp(self, n_active_threads)
**_warn_mkl_vcomp**: The function of _warn_mkl_vcomp is to issue a specific warning related to the use of vcomp and MKL libraries when both are present in the environment.

**parameters**: The parameters of this Function.
· n_active_threads: An integer representing the number of active threads that are currently being utilized.

**Code Description**: The _warn_mkl_vcomp function is designed to issue a warning when both the vcomp (Microsoft OpenMP) and MKL (Intel Math Kernel Library) libraries are detected in the execution environment. This function is invoked by the _check_mkl_vcomp method, which is responsible for checking the compatibility of these libraries during the execution of KMeans clustering algorithms.

The _check_mkl_vcomp method first assesses whether the input data X is sparse. If it is not sparse, the method calculates the number of active threads based on the number of samples and a predefined chunk size. It then checks if the number of active threads is less than the number of threads configured for the KMeans instance. If this condition is met, the method retrieves information about the threading modules in use and checks for the presence of both vcomp and MKL.

If both libraries are present, the _warn_mkl_vcomp function is called with the number of active threads as an argument. This warning mechanism is crucial as it addresses a known issue where the combination of these libraries can lead to performance degradation or memory leaks during the execution of the KMeans algorithm.

**Note**: It is important for developers to be aware of the implications of using both vcomp and MKL together, as this can affect the stability and performance of the KMeans clustering process. Proper configuration and awareness of the threading libraries in use are essential to avoid potential issues.
***
### FunctionDef _check_mkl_vcomp(self, X, n_samples)
**_check_mkl_vcomp**: The function of _check_mkl_vcomp is to verify the compatibility of the vcomp and MKL libraries when both are present in the execution environment.

**parameters**: The parameters of this Function.
· X: The input data, which can be either a dense array or a sparse matrix, representing the samples to be clustered.
· n_samples: An integer representing the total number of samples in the input data X.

**Code Description**: The _check_mkl_vcomp function is designed to assess the compatibility of the vcomp (Microsoft OpenMP) and MKL (Intel Math Kernel Library) libraries during the execution of KMeans clustering algorithms. This function is particularly important as it addresses a known issue where the combination of these libraries can lead to performance degradation or memory leaks.

The function begins by checking if the input data X is sparse using the `sp.issparse` method. If X is sparse, the function exits early, as the compatibility check is not necessary for sparse data. 

Next, the function calculates the number of active threads based on the number of samples and a predefined chunk size (CHUNK_SIZE). It then compares the number of active threads with the number of threads configured for the KMeans instance (self._n_threads). If the number of active threads is less than the configured threads, the function proceeds to gather information about the threading modules currently in use.

The function retrieves the threading module information using the `threadpool_info()` function. It checks for the presence of both vcomp and MKL by examining the module information. Specifically, it looks for "vcomp" in the list of module prefixes and checks if the tuple ("mkl", "intel") is present in the internal API and threading layer information.

If both vcomp and MKL are detected, the function invokes the _warn_mkl_vcomp method, passing the number of active threads as an argument. This warning mechanism is crucial for informing users about the potential issues that may arise from using both libraries simultaneously.

The _check_mkl_vcomp function is called within the fit method of both the KMeans and MiniBatchKMeans classes. In the KMeans fit method, it is called after the input data has been validated and before the clustering algorithm is executed. In the MiniBatchKMeans fit method, it is called similarly, ensuring that the compatibility check is performed before processing the mini-batches of data. Additionally, it is also invoked in the partial_fit method of MiniBatchKMeans to ensure compatibility during incremental updates.

**Note**: Developers should be aware of the implications of using both vcomp and MKL together, as this can affect the stability and performance of the KMeans clustering process. Proper configuration and awareness of the threading libraries in use are essential to avoid potential issues.

**Output Example**: The function does not return a value but may trigger a warning if both vcomp and MKL are detected in the environment. An example warning message could be: "Warning: Both vcomp and MKL are present. This may lead to performance issues."
***
### FunctionDef _validate_center_shape(self, X, centers)
**_validate_center_shape**: The function of _validate_center_shape is to check if the shape of the initial cluster centers is compatible with the input data and the specified number of clusters.

**parameters**: The parameters of this Function.
· X: {ndarray, sparse matrix} of shape (n_samples, n_features) - The input data samples for which the clustering is performed.
· centers: ndarray of shape (n_clusters, n_features) - The initial centers of the clusters that need to be validated.

**Code Description**: The _validate_center_shape function is designed to ensure that the shape of the provided initial cluster centers aligns with the expected dimensions based on the input data and the number of clusters specified in the model. It performs two primary checks:

1. It verifies that the number of rows in the centers array matches the number of clusters (self.n_clusters). If there is a mismatch, it raises a ValueError indicating that the shape of the initial centers does not correspond to the expected number of clusters.

2. It checks that the number of columns in the centers array matches the number of features in the input data (X). If this condition is not met, it raises a ValueError, stating that the shape of the initial centers does not match the number of features in the input data.

This function is called within the _init_centroids method, which is responsible for computing the initial centroids for the clustering algorithm. When the initialization method is set to a callable or a user-provided array, the _validate_center_shape function is invoked to ensure that the provided centers are valid before proceeding with the clustering process.

Additionally, the _validate_center_shape function is also utilized in the fit methods of both the KMeans and MiniBatchKMeans classes. In these methods, it validates the initial cluster centers when they are provided as an array. This ensures that the clustering algorithms operate with valid configurations, preventing runtime errors that could arise from incompatible shapes.

**Note**: It is essential to ensure that the initial centers provided to the clustering algorithm conform to the expected dimensions to avoid exceptions during the clustering process. Users should be aware of the shape requirements when initializing cluster centers, especially when using custom initialization methods.
***
### FunctionDef _check_test_data(self, X)
**_check_test_data**: The function of _check_test_data is to validate and prepare the input data for further processing.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - The input data that needs to be validated.

**Code Description**: The _check_test_data function is responsible for ensuring that the input data X is in an acceptable format before it is used in other methods of the _BaseKMeans class. It utilizes the _validate_data method to perform several checks and transformations on the input data. The parameters passed to _validate_data specify that the function accepts sparse matrices in the Compressed Sparse Row (CSR) format, allows for both float64 and float32 data types, and does not reset any internal state. Additionally, it ensures that large sparse matrices are not accepted.

This function is called by several other methods within the _BaseKMeans class, including predict, transform, and score. In each of these methods, _check_test_data is invoked to validate the input data before any clustering operations are performed. This ensures that the data being processed is in the correct format and meets the necessary requirements, thereby preventing potential errors during execution.

**Note**: It is important to ensure that the input data X conforms to the specified data types and formats, as violations may lead to exceptions or incorrect behavior in the clustering algorithms.

**Output Example**: A possible return value of the function could be a validated NumPy array of shape (n_samples, n_features) containing the input data in the specified format, ready for further processing in the K-means clustering methods.
***
### FunctionDef _init_centroids(self, X, x_squared_norms, init, random_state, sample_weight, init_size, n_centroids)
**_init_centroids**: The function of _init_centroids is to compute the initial centroids for the k-means clustering algorithm based on the specified initialization method.

**parameters**: The parameters of this Function.
· X: {ndarray, sparse matrix} of shape (n_samples, n_features) - The input samples for which the centroids are to be initialized.
· x_squared_norms: ndarray of shape (n_samples,) - Squared Euclidean norm of each data point. This can be passed directly to avoid recomputation.
· init: {'k-means++', 'random'}, callable or ndarray of shape (n_clusters, n_features) - Method for initialization of centroids.
· random_state: RandomState instance - Controls the randomness of centroid initialization.
· sample_weight: ndarray of shape (n_samples,) - The weights for each observation in X. This is not used during initialization if `init` is a callable or a user-provided array.
· init_size: int, default=None - Number of samples to randomly sample for speeding up the initialization process.
· n_centroids: int, default=None - Number of centroids to initialize. If None, it defaults to the number of clusters specified in the model.

**Code Description**: The _init_centroids function is responsible for calculating the initial cluster centers for the k-means clustering algorithm. It begins by determining the number of samples and the number of clusters to initialize. If an `init_size` is provided and is less than the total number of samples, a random subset of samples is selected to speed up the initialization process.

The function supports multiple methods for initializing centroids:
1. If `init` is set to "k-means++", it calls the _kmeans_plusplus function, which selects initial centers in a way that is expected to improve the convergence speed of the k-means algorithm.
2. If `init` is set to "random", it randomly selects samples from the input data as the initial centroids, weighted by the provided sample weights.
3. If `init` is an array-like structure, it uses the provided array as the initial centers.
4. If `init` is a callable function, it invokes this function to generate the initial centers.

After determining the initial centers, the function checks if the centers are in a sparse format and converts them to a dense array if necessary. The computed centers are then returned.

This function is called by the fit methods of both the KMeans and MiniBatchKMeans classes. In these methods, it is used to initialize the centroids before running the k-means algorithm. The fit methods validate the input data and parameters, and they ensure that the initialization is performed correctly, leveraging the _init_centroids function to set up the initial state for clustering.

**Note**: It is crucial to ensure that the number of samples in `X` is greater than or equal to the number of clusters specified. Users should also be aware that the choice of initialization method can significantly impact the performance and outcome of the k-means clustering process.

**Output Example**: A possible return value of the function could be:
centers: array([[10, 2],
                [1, 0]])
***
### FunctionDef fit_predict(self, X, y, sample_weight)
**fit_predict**: The function of fit_predict is to compute cluster centers and predict the cluster index for each sample.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - New data to transform.
· y: Ignored - Not used, present here for API consistency by convention.
· sample_weight: array-like of shape (n_samples,), default=None - The weights for each observation in X. If None, all observations are assigned equal weight.

**Code Description**: The fit_predict function serves as a convenience method that combines two operations: fitting the model to the provided data and predicting the cluster indices for that data. It first calls the fit method on the input data X, which computes the necessary parameters for clustering, such as the cluster centers. The sample_weight parameter allows users to assign different weights to each observation, which can influence the clustering outcome. After fitting the model, the function retrieves the labels of the clusters assigned to each sample from the fitted model and returns these labels as an ndarray of shape (n_samples,). This method is particularly useful for users who want to perform clustering and obtain the results in a single step.

**Note**: It is important to note that the parameter y is not utilized in this function; it is included solely for maintaining consistency with the API conventions. Users should ensure that the input data X is appropriately shaped and formatted to avoid errors during execution.

**Output Example**: A possible return value of the fit_predict function could be an array like [0, 1, 0, 2, 1], indicating the cluster index for each of the samples in the input data.
***
### FunctionDef predict(self, X, sample_weight)
**predict**: The function of predict is to determine the closest cluster for each sample in the input data X.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - New data to predict the cluster labels for.  
· sample_weight: array-like of shape (n_samples,), default="deprecated" - The weights for each observation in X. If None, all observations are assigned equal weight. This parameter is deprecated and will be removed in future versions.

**Code Description**: The predict function is designed to assign each sample in the input data X to the nearest cluster based on the cluster centers established during the fitting process. Initially, the function checks if the model has been fitted using the check_is_fitted method, ensuring that the necessary parameters are available for making predictions.

Next, the input data X is validated and prepared for processing through the _check_test_data method, which ensures that the data is in the correct format and meets the requirements for further analysis. The function also handles the sample_weight parameter, which is marked as deprecated. If the sample_weight is not set to the deprecated string, a warning is issued to inform the user of its impending removal in future versions. The sample weights are then validated using the _check_sample_weight function, which ensures that they are compatible with the input data.

The core functionality of the predict method is executed by calling the _labels_inertia_threadpool_limit function. This function computes the labels for the samples in X by determining which cluster center is closest to each sample. It operates within a controlled thread pool context to optimize performance and resource usage. The labels returned indicate the index of the cluster each sample belongs to.

The predict method is integral to the K-means clustering process, as it allows users to classify new data points based on the clusters identified during the fitting phase. It is commonly used in scenarios where new observations need to be categorized according to previously established clusters.

**Note**: Users should be aware that the sample_weight parameter is deprecated and should avoid using it in future implementations. Additionally, it is crucial to ensure that the input data X is formatted correctly to prevent errors during execution.

**Output Example**: A possible return value of the function when called with a dataset of 5 samples might look like this:  
Labels: [0, 1, 0, 2, 1]
***
### FunctionDef fit_transform(self, X, y, sample_weight)
**fit_transform**: The function of fit_transform is to compute clustering and transform the input data X into a cluster-distance space efficiently.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - New data to transform.
· y: Ignored - Not used, present here for API consistency by convention.
· sample_weight: array-like of shape (n_samples,), default=None - The weights for each observation in X. If None, all observations are assigned equal weight.

**Code Description**: The fit_transform method is designed to perform clustering on the input data X and subsequently transform it into a new space that reflects the distances to the identified clusters. This method is equivalent to first fitting the model to the data using the fit method and then transforming the data using the transform method. However, it is implemented in a more efficient manner to optimize performance. The input parameter X represents the data to be clustered, while the parameter y is included for consistency with other methods but is not utilized in this function. The sample_weight parameter allows for the specification of weights for each observation, enabling the user to influence the clustering process based on the importance of individual samples. If sample_weight is not provided, all samples are treated equally. The method returns an ndarray of shape (n_samples, n_clusters), which contains the transformed data in the new cluster-distance space.

**Note**: It is important to ensure that the input data X is in the correct format (array-like or sparse matrix) and has the appropriate shape. The sample_weight parameter is optional, but when used, it should match the number of samples in X.

**Output Example**: A possible return value of the fit_transform method could be an ndarray such as:
[[0.1, 0.9, 0.0],
 [0.2, 0.8, 0.0],
 [0.0, 0.0, 1.0]] 
This output represents the distances of each sample to the respective clusters in the transformed space.
***
### FunctionDef transform(self, X)
**transform**: The function of transform is to convert input data into a new space representing distances to cluster centers.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - New data to transform.

**Code Description**: The transform method is a key function within the _BaseKMeans class that facilitates the transformation of input data X into a cluster-distance space. In this transformed space, each dimension corresponds to the distance from the data points to the identified cluster centers. The method begins by ensuring that the model has been fitted with the training data through the call to check_is_fitted(self). This is crucial as it verifies that the necessary cluster centers are available for distance calculations.

Following this check, the method invokes _check_test_data(X) to validate and prepare the input data. This function ensures that the input data X is in an acceptable format, allowing for both dense and sparse representations. Once the data has been validated, the method proceeds to call _transform(X), which is responsible for computing the actual distances between the input data points and the cluster centers.

The _transform function operates without performing any input validation, relying on the prior checks conducted by the transform method. It utilizes the euclidean_distances function to calculate the distances, returning a new array where each row corresponds to a sample from X and each column corresponds to a cluster center.

This structured approach, where transform manages data validation and preparation while _transform focuses solely on distance computation, ensures a clear separation of responsibilities within the code. As a result, users can confidently transform their data into a format suitable for further analysis or clustering operations.

**Note**: It is essential to ensure that the model is fitted before invoking the transform method. Additionally, the input data X must conform to the specified formats and types to avoid potential errors during execution.

**Output Example**: A possible return value of the transform function could be a 2D NumPy array where each row represents a sample from X and each column corresponds to a cluster center, with values indicating the distances. For example, if there are 3 samples and 2 clusters, the output might look like:
```
array([[1.5, 2.0],
       [0.5, 1.2],
       [3.1, 0.8]])
```
***
### FunctionDef _transform(self, X)
**_transform**: The function of _transform is to compute the distances between input data points and the cluster centers.

**parameters**: The parameters of this Function.
· X: array-like or sparse matrix of shape (n_samples, n_features) representing the new data to transform.

**Code Description**: The _transform function is a core component of the clustering algorithm that calculates the Euclidean distances from each data point in the input array X to the cluster centers stored in the attribute self.cluster_centers_. This function does not perform any input validation, which means it assumes that the input data X has already been checked and is in the correct format. 

The _transform function is called by the transform method of the _BaseKMeans class. The transform method serves as a public interface for users to convert their data into a new space where each dimension corresponds to the distance from the data points to the identified cluster centers. Before invoking _transform, the transform method ensures that the model has been fitted with the training data by calling check_is_fitted(self) and also checks the test data format with _check_test_data(X). This layered approach allows for a clean separation of concerns, where _transform focuses solely on the distance computation, while transform handles data validation and preparation.

**Note**: It is important to ensure that the model is fitted before calling the transform method, as the _transform function relies on the cluster centers being available.

**Output Example**: A possible return value of the _transform function could be a 2D NumPy array where each row corresponds to a sample from X and each column corresponds to a cluster center, with the values representing the distances. For instance, if there are 3 samples and 2 clusters, the output might look like:
```
array([[1.5, 2.0],
       [0.5, 1.2],
       [3.1, 0.8]])
```
***
### FunctionDef score(self, X, y, sample_weight)
**score**: The function of score is to compute the opposite of the value of X on the K-means objective.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - New data to evaluate against the K-means model.  
· y: Ignored - This parameter is not used and is present for API consistency by convention.  
· sample_weight: array-like of shape (n_samples,), default=None - The weights for each observation in X. If None, all observations are assigned equal weight.

**Code Description**: The score function is designed to evaluate the performance of the K-means clustering algorithm on a given dataset X. It begins by ensuring that the K-means model has been fitted to the data through the call to check_is_fitted(self). This is a crucial step as it verifies that the model has learned the cluster centers from the training data before any scoring can occur.

Next, the function validates the input data X by calling the _check_test_data method. This method ensures that the data is in an acceptable format, either as a dense array or a sparse matrix in CSR format, and prepares it for further processing. The sample weights are also checked and validated through the _check_sample_weight function, which ensures that they are appropriately defined and match the shape of the input data.

The core of the scoring mechanism is executed by the _labels_inertia_threadpool_limit function. This function computes the labels and inertia for the input data X based on the cluster centers learned during the fitting process. It operates within a controlled threading context to optimize performance while managing resource usage. The function returns the inertia score, which quantifies the compactness of the clusters formed by the K-means algorithm.

Finally, the score function returns the negative of the computed inertia score. This is because, in the context of K-means, a lower inertia indicates a better clustering solution, and thus the function provides the opposite value to reflect this relationship.

The score function is typically called after fitting the K-means model to new data, allowing users to evaluate how well the model performs on unseen samples.

**Note**: It is essential to ensure that the input data X is formatted correctly and that the sample weights are defined if used. The function is optimized for performance but requires that the model has been fitted prior to invocation.

**Output Example**: A possible return value of the function when called with a dataset of 5 samples might look like this:  
Score: -12.34
***
### FunctionDef _more_tags(self)
**_more_tags**: The function of _more_tags is to provide additional metadata regarding the behavior of the class, specifically related to sample weight invariance checks.

**parameters**: The parameters of this Function.
· There are no parameters for this function.

**Code Description**: The _more_tags function returns a dictionary containing specific tags that provide insights into the behavior of the class it belongs to. In this case, it includes a key "_xfail_checks" which indicates that there is a known issue with the handling of sample weights. The dictionary specifies that the check for sample weight invariance is expected to fail, with a description stating that "zero sample_weight is not equivalent to removing samples." This suggests that when sample weights are set to zero, the model does not behave as if those samples were entirely excluded from the dataset, which is an important consideration for users implementing this functionality.

**Note**: It is important for users to be aware of this behavior when using the class, as it may affect the results of their model training and evaluation. Understanding this limitation can help in making informed decisions regarding data preprocessing and model configuration.

**Output Example**: The return value of the _more_tags function would appear as follows:
{
    "_xfail_checks": {
        "check_sample_weights_invariance": (
            "zero sample_weight is not equivalent to removing samples"
        ),
    },
}
***
## ClassDef KMeans
**KMeans**: The function of KMeans is to perform K-Means clustering, a popular algorithm used for partitioning a dataset into distinct groups based on feature similarity.

**attributes**: The attributes of this Class.
· n_clusters: The number of clusters to form as well as the number of centroids to generate.
· init: Method for initialization of cluster centers.
· n_init: Number of times the k-means algorithm will be run with different centroid seeds.
· max_iter: Maximum number of iterations of the k-means algorithm for a single run.
· tol: Relative tolerance with regards to Frobenius norm of the difference in the cluster centers of two consecutive iterations to declare convergence.
· verbose: Verbosity mode.
· random_state: Determines random number generation for centroid initialization.
· copy_x: When pre-computing distances, indicates whether to modify the original data.
· algorithm: K-means algorithm to use, either "lloyd" or "elkan".

**Code Description**: The KMeans class is an implementation of the K-Means clustering algorithm, inheriting from the _BaseKMeans class, which provides foundational functionality and parameter validation. The KMeans class allows users to specify the number of clusters (n_clusters) they wish to form, the method for initializing cluster centers (init), and various other parameters that control the behavior of the algorithm.

Upon initialization, the KMeans class sets up the parameters and checks them against the input data using the inherited methods from _BaseKMeans. The fit method computes the K-Means clustering by iterating through the data, adjusting the cluster centers based on the assigned labels until convergence is achieved or the maximum number of iterations is reached.

The KMeans class also includes methods for predicting cluster labels for new data points and transforming the data into a cluster-distance space. It maintains attributes that store the final cluster centers, labels for each data point, the inertia (a measure of how tightly the clusters are packed), and the number of iterations run.

The KMeans class is called by the k_means function, which serves as a convenient interface for users to perform clustering without directly interacting with the class. The k_means function initializes an instance of KMeans with the specified parameters and calls its fit method to perform the clustering. The results, including the final centroids, labels, and inertia, are returned to the user.

**Note**: When using the KMeans class, it is essential to ensure that the input data meets the specified constraints, particularly regarding the number of samples and clusters. Users should also be aware of the initialization methods and their implications on the clustering results, as well as the potential for the algorithm to converge to local minima.

**Output Example**: A possible return value from the fit method could be:
```
cluster_centers_: array([[10.,  2.],
                          [ 1.,  2.]])
labels_: array([1, 1, 1, 0, 0, 0], dtype=int32)
inertia_: 16.0
n_iter_: 5
``` 
This output indicates the coordinates of the cluster centers, the cluster assignment for each sample in the input data, the final inertia value, and the number of iterations taken to converge.
### FunctionDef __init__(self, n_clusters)
**__init__**: The function of __init__ is to initialize an instance of the KMeans class with specified parameters for clustering.

**parameters**: The parameters of this Function.
· n_clusters: The number of clusters to form as well as the number of centroids to generate. Default is 8.  
· init: Method for initialization. Default is "k-means++", which helps in selecting initial cluster centers in a smart way to speed up convergence.  
· n_init: Number of times the k-means algorithm will be run with different centroid seeds. Default is "auto".  
· max_iter: Maximum number of iterations of the k-means algorithm for a single run. Default is 300.  
· tol: Relative tolerance with regards to inertia to declare convergence. Default is 1e-4.  
· verbose: Verbosity mode. Default is 0, which means no output.  
· random_state: Determines random number generation for centroid initialization. Use an int for reproducibility. Default is None.  
· copy_x: If True, the original data will be copied; else, it may be overwritten. Default is True.  
· algorithm: K-means algorithm to use. Default is "lloyd", which is the standard algorithm.

**Code Description**: The __init__ function serves as the constructor for the KMeans class, allowing users to create an instance of the KMeans clustering algorithm with customizable parameters. The function begins by calling the constructor of the parent class using `super().__init__`, passing essential parameters such as `n_clusters`, `init`, `n_init`, `max_iter`, `tol`, `verbose`, and `random_state`. This ensures that the base class is properly initialized with these values. The function then sets additional attributes specific to the KMeans class: `copy_x`, which determines whether to copy the input data or not, and `algorithm`, which specifies the algorithm to be used for clustering. This initialization process is crucial for configuring the behavior of the KMeans instance according to the user's requirements.

**Note**: It is important to choose the number of clusters (`n_clusters`) wisely, as it significantly affects the clustering results. Additionally, setting `random_state` can help in achieving reproducible results when running the algorithm multiple times.
***
### FunctionDef _check_params_vs_input(self, X)
**_check_params_vs_input**: The function of _check_params_vs_input is to validate the input parameters against the expected configuration for the KMeans clustering algorithm.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - The input data to be validated, which contains the training instances for clustering.

**Code Description**: The _check_params_vs_input function is responsible for ensuring that the input data X is compatible with the parameters set for the KMeans algorithm. It first calls the superclass method to perform initial checks on the input data, passing a default value for n_init (number of initializations) set to 10. This ensures that the input data adheres to the expected format and structure required by the KMeans algorithm.

Following the superclass validation, the function checks the algorithm specified for clustering. If the algorithm is set to "elkan" and the number of clusters (n_clusters) is equal to 1, a warning is issued. This warning indicates that using the "elkan" algorithm is not appropriate for a single cluster scenario, and the algorithm is automatically switched to "lloyd". This adjustment is crucial because the "elkan" algorithm is optimized for scenarios with multiple clusters and may not function correctly with only one cluster.

The _check_params_vs_input function is called within the fit method of the KMeans class. The fit method is responsible for computing the k-means clustering by validating the input data, initializing centroids, and running the clustering algorithm. By validating the parameters against the input data, _check_params_vs_input ensures that the clustering process is set up correctly before any computations are performed, thus preventing potential errors during execution.

**Note**: It is important to ensure that the input data X is in the correct format and structure before calling the fit method, as this will directly affect the performance and outcome of the clustering process. Additionally, users should be aware of the implications of using different algorithms based on the number of clusters specified.
***
### FunctionDef _warn_mkl_vcomp(self, n_active_threads)
**_warn_mkl_vcomp**: The function of _warn_mkl_vcomp is to issue a warning regarding potential memory leaks when using KMeans with MKL on Windows.

**parameters**: The parameters of this Function.
· n_active_threads: An integer representing the number of active threads that are available for processing.

**Code Description**: The _warn_mkl_vcomp function is designed to alert users when both the Intel Math Kernel Library (MKL) and the vcomp library are present in the environment while using the KMeans algorithm. This situation can lead to a memory leak on Windows systems if the number of chunks processed is less than the number of available threads. The function takes a single parameter, n_active_threads, which indicates how many threads are actively being utilized. When invoked, the function generates a warning message that informs the user of the potential issue and provides a recommendation to set the environment variable OMP_NUM_THREADS to the value of n_active_threads. This adjustment is suggested as a means to mitigate the risk of memory leaks during the execution of the KMeans algorithm.

**Note**: It is important for users to heed the warning generated by this function, especially when running KMeans on Windows with MKL. Proper configuration of the OMP_NUM_THREADS environment variable can help prevent performance degradation and memory-related issues.
***
### FunctionDef fit(self, X, y, sample_weight)
**fit**: The function of fit is to compute k-means clustering.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - Training instances to cluster. The data will be converted to C ordering, which may cause a memory copy if the data is not C-contiguous. If a sparse matrix is passed, a copy will be made if it's not in CSR format.
· y: Ignored - Not used, present here for API consistency by convention.
· sample_weight: array-like of shape (n_samples,), default=None - The weights for each observation in X. If None, all observations are assigned equal weight. `sample_weight` is not used during initialization if `init` is a callable or a user-provided array.

**Code Description**: The fit method is responsible for executing the k-means clustering algorithm. It begins by validating the input data X using the _validate_data method, ensuring that the data is in an acceptable format and type. The method checks for the compatibility of the input data with the parameters set for the KMeans algorithm through the _check_params_vs_input method.

Once the data is validated, the method initializes the random state for reproducibility and checks the sample weights using the _check_sample_weight function. It also determines the number of effective threads for computation.

The method then validates the initial cluster centers if provided, ensuring they conform to the expected shape using the _validate_center_shape method. If the input data is dense, it subtracts the mean of X to enhance the accuracy of distance computations.

The fit method precomputes the squared norms of the data points and selects the appropriate k-means algorithm (either Elkan or Lloyd) based on the specified parameters. It then enters a loop to perform multiple initializations of the cluster centers, running the k-means algorithm for each initialization.

During each iteration, the method initializes the cluster centers and calls the selected k-means function (_kmeans_single_elkan or _kmeans_single_lloyd) to perform the clustering. After each run, it evaluates the results based on inertia and updates the best labels and centers if the current run yields better results.

Finally, the method checks for distinct clusters and raises a warning if the number of distinct clusters found is smaller than the specified number of clusters. It assigns the best cluster centers, labels, inertia, and the number of iterations to the respective attributes of the KMeans instance and returns the fitted estimator.

The fit method is called by the k_means function, which serves as a high-level interface for performing k-means clustering. The k_means function initializes a KMeans instance and calls its fit method to execute the clustering process.

**Note**: It is essential to ensure that the input data X is properly formatted and that the sample weights are correctly specified. The choice of initialization method and the number of initializations can significantly impact the performance and outcome of the k-means clustering process.

**Output Example**: A possible return value from the fit method could be:
```python
KMeans(n_clusters=3, init='k-means++', n_init=10, max_iter=300, random_state=42)
```
This output indicates that the KMeans instance has been fitted with the specified parameters, ready for further analysis or predictions.
***
## FunctionDef _mini_batch_step(X, sample_weight, centers, centers_new, weight_sums, random_state, random_reassign, reassignment_ratio, verbose, n_threads)
**_mini_batch_step**: The function of _mini_batch_step is to perform an incremental update of the centers for the Minibatch K-Means algorithm.

**parameters**: The parameters of this Function.
· X: {ndarray, sparse matrix} of shape (n_samples, n_features) - The original data array. If sparse, must be in CSR format.  
· sample_weight: ndarray of shape (n_samples,) - The weights for each observation in `X`.  
· centers: ndarray of shape (n_clusters, n_features) - The cluster centers before the current iteration.  
· centers_new: ndarray of shape (n_clusters, n_features) - The cluster centers after the current iteration. Modified in-place.  
· weight_sums: ndarray of shape (n_clusters,) - The vector in which we keep track of the numbers of points in a cluster. This array is modified in place.  
· random_state: RandomState instance - Determines random number generation for low count centers reassignment.  
· random_reassign: boolean, default=False - If True, centers with very low counts are randomly reassigned to observations.  
· reassignment_ratio: float, default=0.01 - Control the fraction of the maximum number of counts for a center to be reassigned.  
· verbose: bool, default=False - Controls the verbosity of the output.  
· n_threads: int, default=1 - The number of OpenMP threads to use for the computation.

**Code Description**: The _mini_batch_step function is a key component of the Minibatch K-Means clustering algorithm, designed to efficiently update cluster centers based on a mini-batch of data. The function begins by assigning labels to the input samples (X) based on their proximity to the current cluster centers using the _labels_inertia function. This function computes both the labels and the inertia, which is the sum of squared distances from each sample to its nearest cluster center.

After label assignment, the function updates the cluster centers. Depending on whether the input data is sparse or dense, it calls either _minibatch_update_sparse or _minibatch_update_dense to perform the update. This ensures that the algorithm can handle different data formats efficiently.

The function also includes a mechanism for reassignment of cluster centers that have very low counts. If the random_reassign parameter is set to True, centers with counts below a certain threshold (defined by the reassignment_ratio) are randomly reassigned to new observations from the dataset. This is intended to improve the clustering quality by preventing centers from becoming stagnant.

The _mini_batch_step function is called by the fit and partial_fit methods of the MiniBatchKMeans class. In these methods, it is used to iteratively update the cluster centers as new mini-batches of data are processed. This iterative approach allows the algorithm to converge more quickly and efficiently on the optimal cluster centers.

**Note**: It is important to ensure that the input data X is in the correct format (ndarray or CSR sparse matrix) and that the sample weights are appropriately defined. The function is optimized for performance with the option to utilize multiple threads for computation.

**Output Example**: A possible return value of the function when called with a dataset of 5 samples and 3 clusters might look like this:  
Inertia: 12.34
## ClassDef MiniBatchKMeans
**MiniBatchKMeans**: The function of MiniBatchKMeans is to perform clustering on large datasets using a mini-batch approach to optimize the K-Means algorithm.

**attributes**: The attributes of this Class.
· n_clusters: The number of clusters to form as well as the number of centroids to generate.
· init: Method for initialization of cluster centers, which can be 'k-means++', 'random', or a user-defined callable or array.
· max_iter: Maximum number of iterations over the complete dataset before stopping.
· batch_size: Size of the mini batches used in the optimization process.
· verbose: Verbosity mode to control the level of output during the fitting process.
· compute_labels: A boolean indicating whether to compute label assignments and inertia for the complete dataset after convergence.
· random_state: Determines random number generation for centroid initialization and random reassignment.
· tol: Control early stopping based on the relative center changes.
· max_no_improvement: Control early stopping based on the consecutive number of mini batches that do not yield an improvement on the inertia.
· init_size: Number of samples to randomly sample for speeding up the initialization.
· n_init: Number of random initializations that are tried.
· reassignment_ratio: Control the fraction of the maximum number of counts for a center to be reassigned.

**Code Description**: The MiniBatchKMeans class is an implementation of the K-Means clustering algorithm optimized for large datasets by processing data in smaller batches. It inherits from the _BaseKMeans class, which provides foundational functionality and parameter validation. The class is designed to handle clustering tasks efficiently by minimizing memory usage and computational time.

The constructor initializes several parameters, including the number of clusters, initialization method, maximum iterations, batch size, and others. The class includes methods for fitting the model to the data, predicting cluster assignments, and updating the model incrementally with new data through the `partial_fit` method.

The `_check_params_vs_input` method validates the input parameters against the provided data, ensuring that the number of clusters does not exceed the number of samples and that the initialization method is appropriate. The `_mini_batch_convergence` method implements early stopping logic based on the convergence of the clustering process, allowing the algorithm to terminate when no significant improvements are observed.

The `fit` method computes the centroids on the input data by chunking it into mini-batches, while the `partial_fit` method allows for updating the K-Means estimate on a single mini-batch. The class also provides attributes to store the resulting cluster centers, labels, inertia, and the number of iterations and steps processed during fitting.

Overall, MiniBatchKMeans is particularly useful for applications involving large datasets where traditional K-Means may be computationally expensive and memory-intensive.

**Note**: When using the MiniBatchKMeans class, it is essential to ensure that the input data meets the specified constraints, particularly regarding the number of samples and clusters. Additionally, users should be aware of the initialization methods and their implications on the clustering results.

**Output Example**: A possible return value from the `fit` method could be an object containing the fitted model, with attributes such as:
```
cluster_centers_: array([[3.55102041, 2.48979592],
                          [1.06896552, 1.        ]])
labels_: array([1, 0, 0, 1, 0, 1], dtype=int32)
inertia_: 5.123456789
n_iter_: 5
n_steps_: 10
``` 
This output indicates the cluster centers, the labels assigned to each sample, the inertia value, and the number of iterations and steps taken during the fitting process.
### FunctionDef __init__(self, n_clusters)
**__init__**: The function of __init__ is to initialize an instance of the MiniBatchKMeans class with specified parameters for clustering.

**parameters**: The parameters of this Function.
· n_clusters: The number of clusters to form, default is 8.  
· init: Method for initialization, default is "k-means++".  
· max_iter: Maximum number of iterations for a single run, default is 100.  
· batch_size: Size of the mini-batches, default is 1024.  
· verbose: Verbosity mode, default is 0 (no output).  
· compute_labels: Whether to compute labels for the clusters, default is True.  
· random_state: Seed for random number generation, default is None.  
· tol: Tolerance for convergence, default is 0.0.  
· max_no_improvement: Maximum number of iterations with no improvement before stopping, default is 10.  
· init_size: Size of the initialization set, default is None.  
· n_init: Number of time the k-means algorithm will be run with different centroid seeds, default is "auto".  
· reassignment_ratio: The ratio of reassignment for the clusters, default is 0.01.  

**Code Description**: The __init__ function is a constructor for the MiniBatchKMeans class, which is a variant of the KMeans clustering algorithm designed to handle large datasets efficiently by using mini-batches. The function accepts several parameters that allow users to customize the clustering process. The n_clusters parameter specifies how many clusters the algorithm should find. The init parameter determines the method used to initialize the cluster centers, with "k-means++" being a popular choice for better convergence. The max_iter parameter sets the upper limit on the number of iterations for the algorithm to run, ensuring that it does not run indefinitely. The batch_size parameter controls how many samples are processed in each iteration, which can significantly affect performance and memory usage.

The verbose parameter allows users to control the amount of information printed during the execution of the algorithm, which can be useful for debugging or monitoring progress. The compute_labels parameter indicates whether the algorithm should compute and return the labels of the clusters for each data point. The random_state parameter is used to seed the random number generator for reproducibility of results. The tol parameter sets the tolerance level for convergence, while max_no_improvement defines how many iterations without improvement are allowed before the algorithm stops. The init_size parameter can be specified to control the size of the initial sample used for initializing the cluster centers, and n_init determines how many times the algorithm will be run with different initializations to ensure a good solution. Finally, the reassignment_ratio parameter specifies the fraction of points that can be reassigned to different clusters during the clustering process.

**Note**: It is important to choose the parameters wisely based on the dataset and the specific requirements of the clustering task. The default values are provided for convenience, but they may need to be adjusted for optimal performance in different scenarios.
***
### FunctionDef _check_params_vs_input(self, X)
**_check_params_vs_input**: The function of _check_params_vs_input is to validate and adjust parameters based on the input data provided for the MiniBatchKMeans clustering algorithm.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - The input data to be clustered, which must be validated for compatibility with the clustering algorithm.

**Code Description**: The _check_params_vs_input function performs several critical checks and adjustments related to the parameters used in the MiniBatchKMeans algorithm. Initially, it calls the superclass method to ensure that the input data X meets the expected criteria, while also setting a default value for n_init if not specified. 

Next, the function determines the batch size by taking the minimum of the specified batch_size and the number of samples in X. This ensures that the batch size does not exceed the available data points. 

The function then checks and sets the _init_size parameter, which dictates how many samples will be used to initialize the cluster centers. If _init_size is not provided, it defaults to three times the batch size, with a further check to ensure it is at least three times the number of clusters. If _init_size is less than the number of clusters, a warning is issued, and _init_size is adjusted accordingly. The function also ensures that _init_size does not exceed the number of samples in X.

Additionally, the function validates the reassignment_ratio parameter, raising a ValueError if it is negative, as this would be invalid for the clustering process.

This function is called within the fit and partial_fit methods of the MiniBatchKMeans class. In the fit method, it is invoked after validating the input data to ensure that all parameters are correctly set before proceeding with the clustering process. In the partial_fit method, it is called when initializing the cluster centers for the first time, ensuring that the parameters are appropriately configured for subsequent updates to the clustering model.

**Note**: It is important to ensure that the input data X is in the correct format and that all parameters are set appropriately before calling the fit or partial_fit methods to avoid runtime errors and ensure optimal clustering performance.
***
### FunctionDef _warn_mkl_vcomp(self, n_active_threads)
**_warn_mkl_vcomp**: The function of _warn_mkl_vcomp is to issue a warning regarding potential memory leaks when using MiniBatchKMeans with MKL on Windows.

**parameters**: The parameters of this Function.
· n_active_threads: An integer representing the number of active threads that are being utilized.

**Code Description**: The _warn_mkl_vcomp function is designed to alert users when both the Intel Math Kernel Library (MKL) and the vcomp library are present in the environment. This situation can lead to a memory leak issue specifically on Windows systems when the number of chunks processed is less than the number of available threads. The function takes one parameter, n_active_threads, which indicates how many threads are actively being used during the execution of the MiniBatchKMeans algorithm. 

When the function is called, it triggers a warning message that informs the user about the potential memory leak. The warning suggests two possible solutions to mitigate the issue: either increase the batch size to be greater than or equal to the product of the number of threads and a predefined chunk size (self._n_threads * CHUNK_SIZE), or set the environment variable OMP_NUM_THREADS to the value of n_active_threads. This guidance helps users adjust their configurations to avoid performance degradation due to memory management issues.

**Note**: It is important for users to heed this warning when using MiniBatchKMeans in environments where both MKL and vcomp are present, especially on Windows, to ensure optimal performance and prevent memory leaks.
***
### FunctionDef _mini_batch_convergence(self, step, n_steps, n_samples, centers_squared_diff, batch_inertia)
**_mini_batch_convergence**: The function of _mini_batch_convergence is to implement early stopping logic for the MiniBatchKMeans clustering algorithm based on convergence criteria.

**parameters**: The parameters of this Function.
· step: An integer representing the current iteration step in the minibatch process, starting from 0.
· n_steps: An integer indicating the total number of steps to be performed during the fitting process.
· n_samples: An integer representing the total number of samples in the dataset.
· centers_squared_diff: A float representing the squared difference in the positions of the cluster centers between iterations.
· batch_inertia: A float representing the inertia (or within-cluster sum of squares) calculated for the current minibatch.

**Code Description**: The _mini_batch_convergence function is a helper function designed to monitor the convergence of the MiniBatchKMeans algorithm during its iterative fitting process. It normalizes the batch inertia by dividing it by the batch size to ensure comparability across different batch sizes. The function begins by incrementing the step count for user-friendly output. It ignores the first iteration since it only contains inertia from initialization.

The function computes an Exponentially Weighted Average (EWA) of the batch inertia to smooth out the stochastic variability inherent in minibatch processing. This is done using a formula that incorporates a decay factor (alpha), which is derived from the batch size and the total number of samples. The EWA inertia is then logged if verbosity is enabled.

The function checks for convergence based on two criteria: 
1. If the absolute change in the position of the cluster centers (centers_squared_diff) is less than a predefined tolerance (_tol), it indicates convergence due to small changes in the centers.
2. If there is no improvement in the smoothed inertia over a specified number of iterations (max_no_improvement), it also indicates convergence.

If either of these conditions is met, the function returns True, signaling that the algorithm can stop early. If neither condition is satisfied, it returns False, allowing the fitting process to continue.

This function is called within the fit method of the MiniBatchKMeans class, specifically during the iterative optimization loop. After each minibatch update, the fit method invokes _mini_batch_convergence to assess whether the algorithm has converged based on the current step's inertia and the change in cluster centers. This integration ensures that the fitting process is efficient and can terminate early when appropriate, enhancing performance and reducing unnecessary computations.

**Note**: It is important to ensure that the parameters passed to this function are correctly calculated and represent the current state of the fitting process to avoid incorrect convergence signals.

**Output Example**: The function may return a boolean value, such as True or False, indicating whether the algorithm has converged. For instance, if the function detects convergence due to small changes in cluster centers, it would return True.
***
### FunctionDef _random_reassign(self)
_random_reassign: The function of _random_reassign is to determine whether a random reassignment of clusters should occur based on the number of processed samples and the presence of empty clusters.

parameters: The parameters of this Function.
· None

Code Description: The _random_reassign function is a private method within the MiniBatchKMeans class that checks if a random reassignment of data points to clusters is necessary. This function is crucial for maintaining the effectiveness of the clustering algorithm, particularly in scenarios where some clusters may become empty or when a significant number of samples have been processed since the last reassignment.

The function operates by first incrementing the _n_since_last_reassign attribute by the size of the current batch (_batch_size). It then evaluates two conditions to decide if a reassignment should take place:
1. It checks if any clusters are empty by evaluating if the _counts array (which tracks the number of samples assigned to each cluster) contains any zeros. If any cluster is empty, a reassignment is warranted.
2. It also checks if the number of samples processed since the last reassignment (_n_since_last_reassign) has reached or exceeded ten times the number of clusters (10 * n_clusters). This ensures that reassignments occur at regular intervals, promoting better convergence of the algorithm.

If either condition is met, the function resets _n_since_last_reassign to zero and returns True, indicating that a reassignment should occur. If neither condition is satisfied, it returns False.

The _random_reassign function is called within the fit and partial_fit methods of the MiniBatchKMeans class. In the fit method, it is invoked during the iterative optimization process, where mini-batches of data are processed. The result of _random_reassign informs the _mini_batch_step function whether to perform a random reassignment of data points to clusters. Similarly, in the partial_fit method, it is used to determine if a reassignment is needed when updating the clustering model with a new mini-batch of data. This integration ensures that the clustering algorithm remains adaptive and responsive to the distribution of data points across clusters.

Note: It is important to ensure that the MiniBatchKMeans instance has been initialized correctly before invoking the fit or partial_fit methods, as the behavior of _random_reassign relies on the state of the clustering model.

Output Example: The function does not return a value in the traditional sense but returns a boolean indicating whether a reassignment is needed. For example, it may return True if a reassignment is warranted or False if it is not.
***
### FunctionDef fit(self, X, y, sample_weight)
**fit**: The function of fit is to compute the centroids on the input data X by chunking it into mini-batches.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - Training instances to cluster. The data will be converted to C ordering, which may cause a memory copy if the data is not C-contiguous. If a sparse matrix is passed, a copy will be made if it's not in CSR format.  
· y: Ignored - Not used, present here for API consistency by convention.  
· sample_weight: array-like of shape (n_samples,), default=None - The weights for each observation in X. If None, all observations are assigned equal weight. `sample_weight` is not used during initialization if `init` is a callable or a user-provided array.

**Code Description**: The fit function is a core method of the MiniBatchKMeans class, responsible for performing the clustering operation on the provided dataset X. It begins by validating the input data through the _validate_data method, ensuring that the data is in an acceptable format and meets the requirements for clustering. The function checks for the compatibility of the input parameters with the data, including the initialization of cluster centers and the handling of sample weights.

The function then initializes several variables, including the number of samples and features in the dataset. It validates the initial cluster centers if provided, ensuring they conform to the expected shape relative to the number of clusters and features in the data. The function also precomputes the squared norms of the data points to optimize subsequent calculations.

The main clustering process is executed in an iterative loop, where mini-batches of data are sampled and processed. For each mini-batch, the function updates the cluster centers based on the assigned labels using the _mini_batch_step method. This method performs the actual update of the centers and computes the inertia, which quantifies the compactness of the clusters.

The function monitors convergence through the _mini_batch_convergence method, which checks if the algorithm has reached a satisfactory solution based on the change in cluster centers and the inertia values. If convergence criteria are met, the fitting process is terminated early to enhance efficiency.

Finally, the function returns the fitted estimator, which includes the computed cluster centers and other relevant attributes, such as labels and inertia, if requested. The fit method is integral to the MiniBatchKMeans algorithm, allowing it to efficiently handle large datasets by processing them in smaller, manageable chunks.

**Note**: It is crucial to ensure that the input data X is in the correct format and that all parameters are appropriately set before invoking the fit method to avoid runtime errors and ensure optimal clustering performance.

**Output Example**: A possible return value of the function when called with a dataset of 5 samples and 3 clusters might look like this:  
Cluster Centers: [[1.5, 2.0], [3.0, 4.5], [5.0, 6.0]]  
Labels: [0, 1, 0, 2, 1]  
Inertia: 10.56
***
### FunctionDef partial_fit(self, X, y, sample_weight)
**partial_fit**: The function of partial_fit is to update the k-means estimate on a single mini-batch of data.

**parameters**: The parameters of this Function.
· X: {array-like, sparse matrix} of shape (n_samples, n_features) - Training instances to cluster. The data will be converted to C ordering, which may cause a memory copy if the data is not C-contiguous. If a sparse matrix is passed, a copy will be made if it's not in CSR format.  
· y: Ignored - Not used, present here for API consistency by convention.  
· sample_weight: array-like of shape (n_samples,), default=None - The weights for each observation in X. If None, all observations are assigned equal weight. `sample_weight` is not used during initialization if `init` is a callable or a user-provided array.

**Code Description**: The partial_fit function is a core method of the MiniBatchKMeans class, designed to incrementally update the clustering model with new data. It begins by checking if the model has already been fitted by examining the presence of cluster centers. If the model has not been fitted, it initializes the cluster centers based on the provided data and specified initialization method.

The function first validates the input data X, ensuring it is in an acceptable format (either dense or sparse) and conforms to the expected data types. It also checks the sample weights to ensure they are appropriately defined. The squared norms of the data points are precomputed to facilitate efficient distance calculations during the clustering process.

If the model is being fitted for the first time, the function validates the initialization parameters and computes the initial cluster centers using the _init_centroids method. It also initializes the counts of samples assigned to each cluster and tracks the number of samples seen since the last reassignment.

The core of the function involves performing a mini-batch step, where the _mini_batch_step function is called to update the cluster centers based on the current mini-batch of data. This function handles the assignment of samples to the nearest cluster centers and updates the centers accordingly.

Additionally, if the compute_labels attribute is set to True, the function computes the labels and inertia for the current mini-batch using the _labels_inertia_threadpool_limit function. This provides insights into the clustering quality and helps in monitoring the convergence of the algorithm.

The function concludes by incrementing the number of steps taken and returning the updated estimator, allowing for further incremental updates with additional data.

The partial_fit method is integral to the MiniBatchKMeans algorithm, enabling it to process large datasets in smaller, manageable chunks while continuously refining the clustering model.

**Note**: It is essential to ensure that the input data X is in the correct format and that all parameters are set appropriately before calling the partial_fit method to avoid runtime errors and ensure optimal clustering performance.

**Output Example**: A possible return value of the function could be the updated MiniBatchKMeans instance itself, reflecting the changes made during the fitting process. For example:  
MiniBatchKMeans(n_clusters=3, n_steps=1, cluster_centers_=array([[1.5, 2.5], [3.0, 4.0], [5.0, 6.0]]))
***
