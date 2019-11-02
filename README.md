# DeepLocalProteinDocking

# Requirements 
1. PyTorch 1.1
2. TorchProteinLibrary (https://github.com/lupoglaz/TorchProteinLibrary,  commit 16166ce4847ad3ba95cd5afdb8bb4503cad32caa)
3. SE3CNN (https://github.com/mariogeiger/se3cnn)


# Step by step instructions

[Training dataset (13Gb)](https://drive.google.com/open?id=1QVl2yvgyV-Tk-OMQBU-Ry44th0o1HajO)

[Training logs (39Mb)](https://drive.google.com/open?id=1tq6cPKAadwVkiIFptP-__GRaNC_lhdiv)

[Preprocessed Docking Benchmark V4 (50Mb)](https://drive.google.com/open?id=1_1cOfN0cd9qyi8W3tPBbUWjoYTlHxKgl)

[Preprocessed Docking Benchmark V5 (50Mb)](https://drive.google.com/open?id=1XBCiSpa_3aeCzErGfl58HVSByVL3Cdc8
)


## Precomputed results
1. Chose some directories where you will store your data.
In the file *src/init.py* change the *storage_dir* for the local machine.

2. Download the dataset and unpack it in the *storage_dir*.
The directories configuration should look like *storage_dir*/LocalDockingDataset/...

3. Download logs and models and unpack it in the *storage_dir*, move the folders DPD_experiments and DPD_models to the *storage_dir*.

4. Download preprocessed benchmarks and store them in your *storage_dir/LocalDockingDataset*. Main point of interest are directories *Matched* in these benchmarks. They contain matched chains between bound and unbound structures. For more details see [here](./scripts/Dataset/README.md).

Your *storage_dir* should look like:

*storage_dir*/LocalDockingDataset/..

*storage_dir*/LocalDockingDataset/DockingBenchmarkV4/..

*storage_dir*/LocalDockingDataset/DockingBenchmarkV5/..

*storage_dir*/DPD_experiments/..

*storage_dir*/DPD_models/..



## Generating your own dataset
If you want to make your own dataset, follow intruction in the [scipts/Dataset](./scripts/Dataset/README.md) directory. The whole process takes around 2-3 days, also there might be some inconsistencies with the directories. If you find, that it does not work, please submit an [issue](https://github.com/lupoglaz/DeepLocalProteinDocking/issues).


## Code structure
1. Dataset loaders can be found in *src/Dataset* directory. We use two loaders: one for the generated dataset and one for the DockingBenchmark.

2. Models are defined in *src/Models*. They are split into Protein Representation Models and Docking Models. Additionally, here one can find the ranking loss function and volume multiplication modules

3. Training iteration is defined in *src/Training/LocalTrainer.py*

4. Docking iteration is defined in *src/Docker/Docker.py*

Main scripts for training and testing are: *src/train_local.py* and *src/test_local.py*. Training and testing parameters we used in the paper can be found in *src/server_train_local.sh* and *src/server_test_local.sh*

5. Rotations loading and circular volume plotting can be found in *src/Utils*

We used rotation generated by https://mitchell-lab.biochem.wisc.edu/SOI/index.php that can be found in *data* directory. However, they have different license than this repository. Be sure to obtain the license from MitchellLab.

## Training a model
Trainig a model from scratch takes around 4-5 days on TitanX Maxwell. To launch the scripts change the directory to <Repository directory>/src and run *local_train.py* with the appropriate parameters.

An example of the training parameters can be found in *server_train_local.sh*.

## Evaluating a model
To evaluate a model on the benchmark head to <Repository directory>/src and run *local_test.py* with the appropriate parameters. The evaluation takes 3 days on 5 nodes using K40 GPU.

__We did not evaluate docking of the bound structures.__ In case you would like to do this experiment change lines 62 and 63 in the file *src/local_test.py* to pass bound structures to the docker.

An example of the testing parameters can be found in *server_test_local.sh*.


# Plotting the results
The instruction on how to plot the results are [here](./scripts/Results/README.md)

# Citing this work
```bibtex
@article {Derevyanko738690,
	author = {Derevyanko, Georgy and Lamoureux, Guillaume},
	title = {Protein-protein docking using learned three-dimensional representations},
	elocation-id = {738690},
	year = {2019},
	doi = {10.1101/738690},
	publisher = {Cold Spring Harbor Laboratory},
	URL = {https://www.biorxiv.org/content/early/2019/08/19/738690},
	eprint = {https://www.biorxiv.org/content/early/2019/08/19/738690.full.pdf},
	journal = {bioRxiv}
}
```
