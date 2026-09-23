## Multi stage build
### Before

```bash
docker build -t before_msb -f mlflow/Dockerfile --build-arg MLFLOW_VERSION=2.3.2 mlflow && docker run -p 5000:5000 before_msb
```

### After

```bash
docker build -t after_msb -f mlflow/Dockerfile-multistage-build --build-arg MLFLOW_VERSION=2.3.2 mlflow && docker run -p 5000:5000 after_msb
```

## Docker Compose
- Up and running Trino with Docker Compose

    ```shell
    docker compose -f trino-docker-compose.yaml up -d
    ```

- After setting up Trino and PostgreSQL, we can start mlflow using the above backend store URI:
    
    ```shell
    docker compose -f mlflow-docker-compose.yaml up -d
    ```

## How to use MLFlow in your training notebook:

    ```shell
    import mlflow
    from sklearn.model_selection import train_test_split
    from sklearn.datasets import load_diabetes
    from sklearn.ensemble import RandomForestRegressor

    # set the experiment id
    # mlflow.set_experiment(experiment_id="0")
    mlflow.set_tracking_uri("http://localhost:5001")

    print(mlflow.get_tracking_uri())

    mlflow.autolog()
    db = load_diabetes()

    X_train, X_test, y_train, y_test = train_test_split(db.data, db.target)

    # Create and train models.
    rf = RandomForestRegressor(n_estimators=100, max_depth=6, max_features=3)
    rf.fit(X_train, y_train)

    # Use the model to make predictions on the test dataset.
    predictions = rf.predict(X_test)

    ```