# Modelos e hiperparámetros

## Regresión Lineal

- [Scikit Learn model](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)
- Hiperparámetros (técnicamente no se consideran Hiperparámetros)
  - `fit_intercept`
    - Booleano (default = True)
    - Indica si se debe calcular el *intercept* del modelo.
  - `tol`
    - Float (default = *1e-6*)
    - Precisión de la solución.
    - ***Diría de utilizar el valor por defecto***.

## Ridge

- [Scikit Learn model](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Ridge.html)
- Hiperparámetros
  - `alpha`
    - Constante que multiplica el término `L2`.
    - Debe ser NO negativo
    - No debe ser cero (sino, es el equivalente a Regresión Lineal)
    - En varios ejemplos se suelen utilizar rangos como `(0, 100]`, con diferentes incrementos (`0.01`, `0.001` y menores).
  - `solver`
    - Determina el algoritmo que se utiliará para computar los coeficientes.
    - String (default = `auto`)
      - ‘auto’, ‘svd’, ‘cholesky’, ‘lsqr’, ‘sparse_cg’, ‘sag’, ‘saga’, ‘lbfgs’
    - Por lo que encontré, se suele utilizar directamente `auto`, el cual permite seleccionar automaticamente el algoritmo en función de los datos.
    - ***Diría de utilizar el valor por defecto***.
  - `tol`
    - Float (default = *1e-4*)
    - Precisión de la solución.
    - Tiene diferentes criterios de convergencia seguén el valor de `solver`. Para `svd` y `cholesky` no tiene impacto.
    - ***Diría de utilizar el valor por defecto***.
- Ejemplos
  - [Ridge y Lasso](https://ai-ml-analytics.com/Jupyter_notebook/Blog%20-%2020%20-%20Hyperparameter%20tuning%20using%20Ridge%20and%20Lasso%20Regression.html)

## Lasso

- [Scikit Learn model](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Lasso.html)
- Hiperparámetros
  - `alpha`
    - Constante que multiplica el término `L1`.
    - Debe ser NO negativo
    - No debe ser cero (sino, es el equivalente a Regresión Lineal)
    - En varios ejemplos se suelen utilizar rangos como `(0, 100]`, con diferentes incrementos (`0.01`, `0.001` y menores).
  - `tol`
    - Idem que `Ridge`, salvo la relación con `solver`, que no existe para Lasso.
    - ***Diría de utilizar el valor por defecto***.
- Ejemplos
  - [Ridge y Lasso](https://ai-ml-analytics.com/Jupyter_notebook/Blog%20-%2020%20-%20Hyperparameter%20tuning%20using%20Ridge%20and%20Lasso%20Regression.html)

## SVR (SVM para regresión)

- [Scikit Learn model](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVR.html)
- Hiperparámetros
  - `kernel`
    - Tipo de kernel a utilizar.
    - Uno de los siguientes strings:
      - ‘linear’, ‘poly’, ‘rbf’, ‘sigmoid’, ‘precomputed’
      - Default: `rbf`
  - `degree`
    - Entero positivo.
    - Default `3`
    - Grado de la función de kernel polinómica.
    - Solo aplica para kernel del tipo `poly`.
    - Se suele recomendar valores como 2, 3, 4, ya que números más grandes pueden aumentar el costo computacional y el riesgo de `overfitting`.
  - `gamma`
    - Float o uno de los siguientes strings: `scale` y `auto`
    - Default: `scale`
      - Si se pasa `scale`, utiliza 1/(n_features * X.var()) como valor de gamma.
      - Si se pasa `auto`, utiliza 1/n_features.
      - Si se pasa un float, debe ser NO negativo.
        - Encontré rangos que van hasta 10, como `[0.0001, 0.001, 1, 10]`.
        - Un valor grande puede causar `overfitting`, de la misma manera que uno muy pequeÑo puede causar `underfitting`.
  - `coef0`
    - Solo aplica a kernels del tipo `poly` y `sigmoid`.
    - Float, default a `0.0`.
    - Rangos
      - Según *Gemini*, no hay rango definido, pero dió un ejemplo de `[-1.0, -0.1, 0.0, 0.1, 1.0, 10.0]` (la fuente, te la debo).
      - Según *StackOverflow*, con utilizar cero alcanza ([fuente](https://stackoverflow.com/a/21392917)).
    - ***Diría de utilizar el valor por defecto***.
  - `C`
    - Parámetro de regularización.
    - Float, obligatoriamente positivo. Por defecto, es `1.0`.
    - En cuanto a rangos, vi que se exploran en escala logarítmica (`[0.1, 1, 10, 100, 1000]`; `np.logspace(-0, 4, 8)`).
  - `epsilon`
    - Float. Debe ser no negativo. Por default es `0.1`.
    - Encontré rangos como `[0.01, 0.1, 0.5, 1.0]`.
    - Según Gemini, el valor óptimo está asociado a la escala del *target*.
- Ejemplos
  - [SVR e hiperparámetros - Stackoverflow](https://stackoverflow.com/a/62834139)
  - [SVR e hiperparámetros - Medium](https://medium.com/@jayedakhtar96/learn-how-to-implement-svr-for-real-world-data-using-python-sklearn-and-hyperparameter-tuning-7b12a2df9074)

## Random Forest (regresión)

- [Scikit Learn model](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)
- Hiperparámetros
  - `n_estimators`
    - CAntidad de árboles en el bosque.
    - Entero, `100` por default.
    - Un ejemplo del rango puede ser `[200, 400, 600, 800, 1000, 1200, 1400, 1600, 1800, 2000]`, pero hay que considerar el tiempo de cómputo.
  - `criterion`
    - Función que mida la calidad de un split.
    - Valores: `“squared_error”, “absolute_error”, “friedman_mse”, “poisson”`.
    - Default: `squared_error”`.
  - `max_depth`
    - Máxima profundidad del árbol.
    - Entero. Default to `None` (sin límite).
    - En cuanto a rangos, vi que la mayoría utiliza `None`, pero otros recomiendan valores como 10, 30, etc.
  - `min_samples_split`
    - Cantidad mínima de ejemplos/registros necesarios para hacer un split.
    - Entero o float. Default a `2`.
    - Rango: vi ejemplos como `[2, 5, 10]`, `[2, 5]`.
  - `min_samples_leaf`
    - Cantidad mínima de ejemplos/registros necesarios para ser considerado un nodo hoja.
    - Entero o float. Default a `1`.
    - Rango: en towards data science utilizan `[1, 2]`.
  - `max_features`
    - El número de features para considerar al hacer splits.
    - Valores posibles: {“sqrt”, “log2”, None}, int or float.
    - Default `1.0`.
    - En towards data science utilizan `['sqrt', 'log2', None]`.
  - `bootstrap`
    - Booleano, default a `true`.
    - Indica si se hace bootstraping al trabajar con cada árbol. Si es falso, se utiliza todo el dataset en cada árbol.
    - ***Diría de utilizar el valor por defecto***.
  - `ccp_alpha`
    - Float mayor a cero, `0.0` es el default.
    - A mayor valor, se aplica un prunning más agresivo. Esto genera árboles más pequeÑos, que puede ayudar a prevenir `overfitting`.
    - **Podríamos probar con** `[0.1, 0.01, 0.001, 0.0]`, incluyendo el `0.0`.
- Ejemplos
  - [Random forest e hiperparámetros - Towards Data Science](https://towardsdatascience.com/understanding-random-forest-using-python-scikit-learn/)
  - [Random forest e hiperparámetros - Medium](https://medium.com/data-science/hyperparameter-tuning-the-random-forest-in-python-using-scikit-learn-28d2aa77dd74)
  - [Random forest e hiperparámetros - Stackoverflow](https://stackoverflow.com/questions/36107820/how-to-tune-parameters-in-random-forest-using-scikit-learn)
  - [Random Forest e hiperparámetros - explicación](https://www.analyticsvidhya.com/blog/2015/06/tuning-random-forest-model/)

## HistGradientBoostingRegressor

- [Scikit Learn model](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingRegressor.html)
- Hiperparámetros
  - `loss`
    - Función de perdida utilizada en el proceso de *boosting*.
    - Posibles valores: `squared_error, absolute_error, gamma, poisson, quantile`
  - `quantile`
    - Especifica que cuantil estimar.
    - Solo aplica cuando `loss` es `quantile`.
    - Acepta valores entre 0 y 1.
  - `learning_rate`
    - Se utiliza como factor de *shrinkage* en cada iteración de *boosting*.
    - Se suelen utilizar valores como 0.01, 0.5 y 0.1.
    - En caso de utilizar valores más pequeños, es posible que haya que aumentar el valor de `max_iter`.
  - `max_iter`
    - El número máximo de iteraciones en el proceso de boosting (o máximo número de árboles).
    - Suele utilizarse junto a *early stopping* junto a valores grandes (ej: *1000*).
  - `max_leaf_nodes`
    - Máximo número de hojas por árbol.
    - Estrictamente debe ser mayor a 1.
    - Default: 31.
    - Suelen utilizarse valores entre 2 y 31, además de *None*.
  - `max_depth`
    - Máxima profundidad de un árbol.
    - Suelen utilizarse valores entre 3 y 10, junto con *None*.
  - `min_samples_leaf`
    - Cantidad mínima de registros por hoja.
    - Default: 20.
    - Suelen utilizarse valores entre 1 y 50, aunque para datasets pequeños se recomienda utilizar valores pequeños.
  - `l2_regularization`
    - Regularización $L2$.
    - El valor $0$ significa que no se aplicará regularización $L2$.
    - Suelen utilizarse valores como $0.0001$, $0.001$, $0.01$, entre otros, en escalas logaritmicas.
  - `early_stopping`
    - Posibles valores: `auto`, o booleano.
    - Si se utiliza `auto`, early stopping se habilita cuando hay más de 10000 registros.
    - Valor por defecto: `auto`.
    - ***Se utilizará el valor por defecto***

## XGBoost

- [xgb.XGBRegressor model](https://xgboost.readthedocs.io/en/stable/parameter.html)
- Hiperparámetros
  - `n_estimators`
    - Se puede utilizar un rango entre 5-1000, pero también puede ser mayor utilizando `early stopping`. En Kaggle utilizan *180*.
  - `eta`
    - Learning rate. Rangos como `[0.01-0.2]` (tal como menciona Kaggle).
  - `max_depth`
    - Rangos como 3-10.
  - ***A continuación, copi y pego resultado de Gemini, ya que son varios hiperparámetros. Igualmente diría de ver el ejemplo de Kaggle***.
  - ***Diría de utilizar aquellos que se mencionan en Kaggle***
  - `min_child_weight`: Minimum sum of instance weight (hessian) needed in a child. Typical range: 1-200, often depends on the dataset size.
    - ***NOTA***: Según el ejemplo de Kaggle, usan valores como `hp.quniform('min_child_weight', 0, 10, 1)`.
  - `gamma` (or min_split_loss): Minimum loss reduction required to make a further partition on a leaf node. Range: [0, \(\infty \)). Higher values imply more regularization.
  - `subsample`: Subsample ratio of the training instances. Setting it to 0.5 means that XGBoost would randomly sample half of the training data prior to growing trees. Range: (0, 1].
    - ***NOTA***: Según el ejemplo de Kaggle, usan valores como `0.5, 1`. Podemos definir un rango entre esos números.
  - `colsample_bytree`: Subsample ratio of columns when constructing each tree. Range: (0, 1].
    - ***NOTA***: Según el ejemplo de Kaggle, usan valores como `hp.uniform('colsample_bytree', 0.5,1)`.
  - colsample_bylevel: Subsample ratio of columns for each level. Range: (0, 1].
  - colsample_bynode: Subsample ratio of columns for each node (split). Range: (0, 1].
  - `reg_alpha` (L1 regularization): L1 regularization term on weights. Range: [0, \(\infty \)).
    - ***NOTA***: Según el ejemplo de Kaggle, usan valores como `hp.quniform('reg_alpha', 40,180,1)`.
  - `reg_lambda` (L2 regularization): L2 regularization term on weights. Range: [0, \(\infty \)).
    - ***NOTA***: Según el ejemplo de Kaggle, usan valores como `hp.uniform('reg_lambda', 0,1)`.
  - max_delta_step: Maximum delta step we allow each leaf output to be. Range: [0, \(\infty \)). Usually set to 0 (no constraint), but can be helpful for imbalanced datasets in logistic regression
- Ejemplos
  - [XGBoost e hiperparámetros - Kaggle](https://www.kaggle.com/code/prashant111/a-guide-on-xgboost-hyperparameters-tuning)
