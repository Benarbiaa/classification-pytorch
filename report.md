# MLOps Full Workflow Reproducibility Report

## 1. Introduction
This document reports on the complete reproduction of the provided end-to-end MLOps workflow for image classification using PyTorch. The objective was to validate understanding of the full pipeline, from cloud credential setup to experiment tracking and analysis, using DVC, MLflow, and GitHub Actions.

The work was conducted strictly following the provided guide, with additional controlled experiments to explore the impact of hyperparameter changes on model performance.


## 2. Google Cloud Service Account Setup

![Downloaded service account JSON credentials](images/image.png)

![Google Cloud service account created](images/serviceCreated.png)



A Google Cloud account was created and configured as follows:

1. A new GCP project was created.
2. A service account was generated with the required permissions (Storage Admin / Drive access as specified in the guide).
3. A JSON key file was created for the service account.
4. The credentials were stored locally and referenced securely using environment variables (GOOGLE_APPLICATION_CREDENTIALS).

This approach ensured credentials were isolated from the codebase and not committed to version control.

## 3. Google Drive Shared Folder Configuration



![Shared folder](images/sharedFolder.png)



A shared Google Drive folder was configured for remote data storage:

- The service account email was granted Editor access to the shared folder.
- Folder access was validated by listing and uploading test files.
- The folder ID was extracted and used in the DVC remote configuration.

This step enabled seamless synchronization between local experiments and remote data storage.

## 5. Repository Cloning and Local Setup

The project repository was cloned locally using Git. After cloning:

- A virtual environment was created and activated.
- Project dependencies were installed.
- Environment variables were configured as required by the project.

The repository structure and scripts were reviewed to understand the training and experiment workflow.

## 6. DVC Configuration and Data Retrieval

DVC was initialized and configured to use Google Drive as the remote backend:

- The remote storage was added and set as default.
- Authentication was verified using the configured service account.
- Project datasets were retrieved using `dvc pull`.

The successful pull confirmed correct DVC setup and remote connectivity.

## 7. MLflow Installation and Configuration

MLflow was installed locally and configured for experiment tracking:

- The MLflow tracking URI was set to a local backend.
- MLflow was started using the MLflow UI server.
- The training script was verified to log parameters, metrics, artifacts, and models.

The MLflow UI was used throughout the experimentation phase for comparison and analysis.

## 8. GitHub Actions (CI/CD)

GitHub Actions were enabled for the repository:

- The provided workflow YAML file was reviewed.
- CI pipelines were triggered on push events.
- Successful runs confirmed correct environment setup and reproducibility of the pipeline.

Where applicable, screenshots were captured to demonstrate workflow execution.

## 8. Code Modification and Experiments

**Screenshot placeholder:**

- *Figure 4*: Command-line execution of the training script.

![Training](images/terminal.png)


### 9.1 Epoch Modification

The training configuration in `src/` was updated to reduce the number of epochs to 5. This change ensured faster experimentation while maintaining meaningful learning behavior.

### 9.2 Hyperparameter Experiments

Multiple experiments were conducted with the following variations:

- Optimizer: SGD vs Adam
- Learning rate adjustments
- Data augmentation strategies
- Batch size variations

Each experiment was assigned a unique MLflow experiment name to avoid ambiguity and ensure traceability.

## 9. MLflow Tracking and Visualization

**Screenshot placeholders:**

- *Figure 5*: MLflow UI – experiment list and parameter comparison.
- *Figure 6*: MLflow UI – loss and accuracy curves.

![MLflow](images/MLflow.png)
![MLflow](images/MLflow1.png)



For each experiment, the following were logged in MLflow:

- Parameters: optimizer type, learning rate, batch size, epochs, augmentation flags
- Metrics: training loss, validation loss, training accuracy, validation accuracy
- Artifacts: loss and accuracy plots, configuration files
- Models: serialized trained models

Using the MLflow UI, the following visualizations were analyzed:

- Loss curves across epochs
- Accuracy curves across epochs
- Parameter comparison tables
- Side-by-side model performance metrics

## 10. Results and Analysis

### 11.1 Best Performing Model

The best-performing model used the Adam optimizer with a moderate learning rate. This configuration demonstrated:

- Faster convergence
- Lower validation loss
- More stable training behavior compared to SGD

### 11.2 Impact of Hyperparameters

- Learning Rate: Higher learning rates led to unstable loss curves and poorer convergence.
- Optimizer Choice: Adam consistently outperformed SGD in early convergence within limited epochs.
- Data Augmentation: Moderate augmentation improved generalization, while aggressive augmentation slowed convergence.

### 11.3 Benchmarking Across Experiments

Benchmarking across all runs showed clear trade-offs between training stability and speed. MLflow comparison views were instrumental in identifying optimal configurations.

## 11. Insights from MLflow Artifacts

MLflow graphs and saved artifacts revealed:

- Clear correlations between learning rate and loss volatility.
- Improved generalization with controlled augmentation.
- Consistent performance improvements when using adaptive optimizers.

The ability to compare experiments side by side significantly reduced manual analysis effort and improved decision-making.

## 12. Conclusion

This assignment successfully demonstrated the ability to reproduce a full MLOps workflow, from cloud setup to experiment analysis. The integration of DVC, MLflow, and GitHub Actions ensured reproducibility, traceability, and automation across the ML lifecycle.

All deliverables, including screenshots and experiment logs, validate correct implementation and a solid understanding of the complete MLOps pipeline.

