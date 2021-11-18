# Requirements

- Run ```poetry install``` to install all the packages needed.

- Get the guacamol smiles with ```bash guacamol_baselines/fetch_guacamol_dataset.sh```
- Unzip de folder models.zip in synthetic_scorers/RAscore
- During the generation, the smiles sampled are stored in Mongo database, so you need to have
two environment variables : ```MONGO_URL``` with the correct url, and ```DB_STORAGE```
with the name of the database. 
- If you want to use the score RScore, given by Spaya API, you need to have two
more environment variables : ```SPAYA_API_URL``` and ```SPAYA_API_TOKEN```, which
are your credentials to use the Spaya API.


# Package description 

- This package is a fork of the code of BenevolantAI https://github.com/BenevolentAI/guacamol_baselines.
It enables running generations with several models, for the tasks from guacamol benchmark ,
and for another task named "pi3kmtor" described in the paper. 
- Additionaly, the package enables to run generations with synthetic score as a new constraint.
The synthetic accessibility score can be one of the following : 
    - SA score
    - RA score
    - SC score
    - RScore (SpayaApi)
    - RSPred (Predictor of SpayaApi trained on Chembl24).


# Run generations

The goal directed generations have 2 essentials arguments :

- suite : this value will determine the generations that are going to be launched. A value of a 
suite correponds to a list of tasks. 
    - 'guacamol_paper' launches the generation of
guacamol benchamrk tasks with successively no synthetic scores, SA score, RScore and RSPred. 
    - 'pi3kmtor_paper' launches the pi3kmtor generation with successively each of the
    synthetic scorers.
    - 'pi3kmtor' just launches one generation of pi3kmtor, with the synthetic score indicated in the variable "synth_score" 
    
- synth_score : when indicated, tells the synthetic scorer to add in the generation, should
 be one of ```RScore, SAscore, SCscore, RAscore, RSPred```. There is not need to use synth_score if suite is 'guacamol_paper' or 'pi3kmtor_paper'.

For instance To run 10 steps of generation around pi3kmtor dataset, optimizing 4 constraints and with 
 SA score constraint run: 

```poetry run python -m guacamol_baselines.smiles_lstm_hc.goal_directed_generation --suite pi3kmtor --n_epochs 10 --synth_score SAscore ```

# Get the results

- The molecules generated are in the mongo database in your var env ```DB_STORAGE```, with
in the collection under the name ``benchmark_name+"_"+synth_score``` 

- For the pi3kmtor generations, the differents scores of the molecules are already in the collection.

- nYou can use the notebook in exploit_results/exploit_results_pi3kmtor.ipynb to analyse the results.
