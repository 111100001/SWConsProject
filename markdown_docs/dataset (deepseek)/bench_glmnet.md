## FunctionDef rmse(a, b)
**rmse**: The function of rmse is to calculate the Root Mean Squared Error (RMSE) between two arrays.

**parameters**: The parameters of this Function.
· a: The first array, typically representing the actual values.
· b: The second array, typically representing the predicted values.

**Code Description**: The `rmse` function computes the Root Mean Squared Error (RMSE) between two arrays, `a` and `b`. RMSE is a commonly used metric to measure the differences between values predicted by a model and the actual observed values. The function first calculates the squared differences between corresponding elements of the two arrays using `(a - b) ** 2`. It then computes the mean of these squared differences using `np.mean()`. Finally, it takes the square root of the mean squared differences using `np.sqrt()` to obtain the RMSE value. This value is returned as the output of the function.

In the context of the project, the `rmse` function is called within the `bench` function to evaluate the performance of a machine learning model. Specifically, it is used to calculate the RMSE between the actual test values (`Y_test`) and the predicted values (`clf.predict(X_test)`). This helps in assessing how well the model is performing in terms of prediction accuracy.

**Note**: Ensure that the input arrays `a` and `b` have the same shape, as the function performs element-wise operations. If the arrays have different shapes, it will result in an error.

**Output Example**: The function returns a single floating-point value representing the RMSE. For example, if the actual values are `[3, 5, 7]` and the predicted values are `[2.5, 5.5, 7.5]`, the function might return `0.7071` as the RMSE.
## FunctionDef bench(factory, X, Y, X_test, Y_test, ref_coef)
**bench**: The function of bench is to evaluate the performance of a machine learning model by measuring the training duration, prediction accuracy (RMSE), and the difference between the model's coefficients and reference coefficients.

**parameters**: The parameters of this Function.
· factory: A factory function that creates and returns a machine learning model instance. The model is expected to have an `alpha` parameter and methods `fit` and `predict`.
· X: The feature matrix used for training the model.
· Y: The target values (labels) corresponding to the training data.
· X_test: The feature matrix used for testing the model.
· Y_test: The target values (labels) corresponding to the test data.
· ref_coef: A reference coefficient array used to compare with the model's learned coefficients.

**Code Description**: The `bench` function performs the following steps to evaluate a machine learning model:
1. It starts by invoking garbage collection (`gc.collect()`) to free up memory and ensure a clean environment for timing the model training.
2. The function records the start time using `time()`.
3. It creates a model instance by calling the `factory` function with the `alpha` parameter and fits the model to the training data (`X`, `Y`) using the `fit` method.
4. The training duration is calculated by subtracting the start time from the current time.
5. The function prints the training duration in seconds.
6. It calculates the Root Mean Squared Error (RMSE) between the actual test values (`Y_test`) and the predicted values (`clf.predict(X_test)`) using the `rmse` function. The RMSE is printed as a measure of prediction accuracy.
7. The function computes the mean absolute difference between the reference coefficients (`ref_coef`) and the model's learned coefficients (`clf.coef_.ravel()`). This difference is printed as a measure of how closely the model's coefficients match the reference.
8. Finally, the function returns the training duration as a floating-point value.

The `bench` function relies on the `rmse` function to calculate the prediction accuracy. The `rmse` function computes the Root Mean Squared Error between two arrays, which is a standard metric for evaluating regression models. The relationship between `bench` and `rmse` is functional, as `bench` uses `rmse` to quantify the model's performance on the test data.

**Note**: Ensure that the `factory` function returns a model instance that supports the `fit` and `predict` methods, and that the `alpha` parameter is correctly set. Additionally, the input arrays (`X`, `Y`, `X_test`, `Y_test`, `ref_coef`) should be properly formatted and compatible with the model's requirements.

**Output Example**: The function returns the training duration as a floating-point value. For example, if the training process takes 1.234 seconds, the function will return `1.234`. The printed output might look like this:
```
duration: 1.234s
rmse: 0.567890
mean coef abs diff: 0.012345
```
