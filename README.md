# EMLO V3 - Session 03

## DVC and push to gdrive, Optuna hyperparams Sweep, Tensorboard 

* Optuna Hyperparam sweep (Grid search for hyperparameters)
* Adding hp_metrics and per step logging in Tensorboard
* DVC and git track
* Tensorboard Public link
* Gdrive link with data and logs
* References




## 1. Optuna Hyperparam sweep (Grid search for hyperparameters)
The below are the hyperparameters search space updated in hparams_search/cifar10_optuna.yaml:

Batchsize: choice(32, 64, 128)
lr rate: interval(0.0001, 0.1)
optimizers: choice(torch.optim.Adam, torch.optim.SGD, torch.optim.RMSprop)

Also, * Tensorboard is set as default logger *.


## 2. Adding hp_metrics and per step logging in Tensorboard

- hp_metrics: If you want to track a metric in the tensorboard hparams tab, log scalars to the key hp_metric. If tracking multiple metrics, initialize TensorBoardLogger with default_hp_metric=False and call log_hyperparams only once with your metric keys and initial values.



- Update in models/timm_module.py:
    * self.logger.log_hyperparams(self.hparams, {"hp_metric": 0}) under on_train_start() function
    * self.log("hp_metric", acc) under validation_step() function. We are using * accuracy as hp_metric * here. 

- step logging for training: 
In training_step(), set on_step=True in the log part. This logs for each step in an epoch.


## 3. DVC and git track
## Steps:

#### Remove git tracking for data and logs
git rm -r --cached 'data/'
git rm -r --cached 'logs/'

#### Stage DVC:
dvc add data
dvc add logs

#### Stage changes
git add .
dvc config core.autostage true

#### Add DVC remote
dvc remote add gdrive gdrive://1Xg8uBRgr0AgsVlgRpKiSAl4rfGXP7yjD

#### Push data & logs to gdrive
dvc push -r gdrive 

Note: The data and logs are stored as hash values.

## 4. Tensorboard Public link:
https://tensorboard.dev/experiment/Id4RR4mrS16G4rOD4PEt2g/#scalars - 20 trials
https://tensorboard.dev/experiment/heIO4mJURf6EJp1VjAZxDA/#scalars  - 5 trials

## 5. Gdrive link with data and logs:
https://drive.google.com/drive/folders/1KFEQpkbBF3oDLvezFCjNpGg9I853b-d1?usp=sharing 

## 6. References
https://pytorch-lightning.readthedocs.io/en/stable/extensions/logging.html 
https://hydra.cc/docs/plugins/optuna_sweeper/ 
https://colab.research.google.com/github/tensorflow/tensorboard/blob/master/docs/tbdev_getting_started.ipynb#scrollTo=n2PvxhOkW7vn 







