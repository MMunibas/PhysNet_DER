# Evidential version of PhysNet
Based on the Pytorch version of PhysNet, an evidential layer is implemented. 

## Using Physnet Evidential
### Setting up the environment

We recommend to use [ Miniconda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/download.html) for the creation of a virtual environment. 

Once in miniconda, you can create a virtual enviroment called *physnet_torch* from the `.yml` file with the following command

``` 
conda env create --file environment.yml
```
 
To activate the virtual environment use the command:

```
conda activate physnet_torch
```

### Creating a new Database

For the moment, our model can be trained by `.npz` file generated from `.xyz` files. The `.npz` is generated by `gen_npz.py` using the following command

```
python gen_npz.py -f folder_with_.xyz_files -o name_of_npz_file
```
You should add the path to the folder with your `.xyz` files and the desired name for your database. 
The generated `.npz` file will be saved in the current directory, we recommend to move it to a directory called `data`.

*Note: File `gen_npz.py` contains the values of the atomization energies of QM9 by default. You can change the values by editing the file.*


### Running a model

To run the model, you modify the desired values on the `input.inp` file and then you can run it on terminal as:

```
python run_train.py @input.inp
```

### Evaluating a model on it's test set.

For every training of the Neural Network model. The data is splited on 3. A set for training, another for validation, and one 
for test. We recommend a splitting 80% for training and 10% for validation and the same for test. The purpose of the test set
is to evaluate if the training of the model was correct. To do the evaluation, you can use the file `Error_test_evaluation.py`.
Inside this file there are three options to save a plot, a `.csv` file or the `.xyz` structures with a variance larges than 
the 95% of the values for variance. To run it, you use the following command:

```
python Error_test_evaluation.py @input.inp --checkpoint x.pt
```

Here the checkpoint should contain the parameters for the neural network. The file `input.inp` contains the parameters of the
NN architecture.

### Evaluating a molecule with a trained model

If you wish to evaluate a molecule with a trained model. You can use the example file `calc_energy.py`. 
On that file you should modify the path to your checkpoint for the trained model and the path to the input file for training.
Then, you can run it on terminal as:

```
python calc_energy.py -i file_to_eval.xyz
```
It will return a value for the energy of the molecule, the variance(epistemic uncertainty) and sigma(aleatory uncertainty).

*Note: The sigma(aleatory uncertainty) can be recovered by dividing the variance with nu*

*Note2: If your evaluation(or training) is done in a computer without gpu, you should set up the variable device to 'cpu'*

## To Do:

- Add other types of formats for the databases
- Make a better documentation

## Contact
L.I.Vazquez-Salazar, email: luisitza.vazquezsalazar@unibas.ch