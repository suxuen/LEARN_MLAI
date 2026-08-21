# LEARN ML/AI


### 1 - Cross Validation (CV)
notebook on applying CV on a toy dataset and my own dataset.

Done:
1. compare Kernel Ridge Regression (KRR), Random Forest Regression (RF), and Histogram Gradient Boosting Regression (HGB) on toy and my dataset
2. compare different CV methods: k-fold, leave one out (LOO)

##### Toy dataset: sklearn diabetes dataset
- Model comparison shows RF and HGB perform similarly, and better than KRR.
    - The KRR, RF, and HGB are minor improvements to the Dummy regressor, likely because the diabetes target is not deterministic from their features
    - CV comparison indicates k-fold (n=5, no shuffling) gives virtually the same results as shuffled, 20-fold, and LOO, but is the fastest (tied with 5-fold with shuffling)
From the tests, I would use RF, 5-fold CV with or without shuffling for regression learning on sklearn's diabetes dataset

##### My dataset: B-site alloy halide perovskites DFT results and Magpie features
- Model comparison shows RF and HGB results in virtually the same MAE. HGB would be the model of choice because it was faster than RF
- KRR, RF, and HGB are major improvements over the Dummy regressor, indicating the deterministic nature of my dataset.
- CV comparison shows 5- and 10-fold result in the same MAEs, but shuffling reduces apparent variance
    - My dataset is arranged such that non-alloy compositions are concatenated at the end of the dataset. This results in high MAE for the last fold in non-shuffled CV, which contributes to high variance.
    - Shuffling narrows variance by dispersing the non-alloy compositions across all folds.
        - Stratified k-fold CV, which will be explored in later notebooks, is expected to serve a similar purpose in - ensuring non-alloy compositions are present in every fold.
        - Grouped k-fold CV, which will also be explored in later notebooks, is expected to serve the opposite purpose as it ensures each CV fold contains a different group of data, and could give us an idea of model performance on unseen chemistry.
From the tests, I would use HGB, 5-fold CV with or without shuffling for regression learning on my dataset. Shuffling will affect whether the model sees all B-site chemistry in each fold or not. Results indicate that the tested models is expected to perform worse on unseen B-site chemistry, and that something interesting might be happening with how my dataset can be grouped/classed.
###### Note on MAEs on my dataset:
MAE weights absolute errors equally regardless of band gap magnitude. A good MAE for semiconducting compositions may not reflect a good performance on metallic compositions as the percentage variance will be larger (owing to a smaller band gap value in the denominator)


### 2 - Learning Curve
Learning Curve on my own dataset.

Done:
1. Examined learning curve on my own dataset to analyze whether ML is limited by dataset size or model quality.


##### My dataset: B-site alloy halide perovskites DFT results and Magpie features
- I used HGB and RF as informed by Notebook 1's results. HGB is the model of choice due to speed, and has near equivalent results to RF
- For the CV, I did 5-fold with and without shuffling. From the notebook, we see that the MAEs are essentially the same between both, but MAE variance is smaller with shuffling. The latter is likely due to train-test leakage.
- Learning curves on HGB show that the validation curve flattens rapidly from ~500 data points to 4000+ data points, strongly indicates that my dataset is large enough for ML with HGB.
    - This suggests improvements to MAE will liekly come from using better models or better features
    - Learning curves on RF show that the validation curve also flattens rapidly from ~500 data points to 4000+ data points
- Comparing HGB and RF shows that RF yields a larger train/validation MAE difference. This is in line with RF's tendency to overfit training data. However, the validation MAEs from RF matches closely to HGB, indicating RF maintains the ability to generalize its predictions.
- A curious note is that shuffling the data leads to a drop in validation MAE from ~4,500 to ~5,500 datapoints.

From the learning curves, I believe my dataset is sufficiently large for Eg predictions and improvements to ML predictions will likely originate from choosing the right models and improving my feature set.


### 3 - Grouping data for CV
This notebook investigates the effects of grouping data in CV. Grouping data helps control test-train leakage and tells us about ML models' performance in extrapolating into unseen territory.

Done:
1. Evaluated differences in shuffled k-fold (full leakage limit) and different groupings for grouped k-fold (and Leave One Group Out, LOGO).


##### My dataset: B-site alloy halide perovskites DFT results and Magpie features
- From Notebooks 1 and 2, I learned that HGB performance is good for learning purposes. Different and more complex models such as XGBoost and Neural Networks will be touched on down the line.
- **Reminder: 5-fold with shuffling is the full leakage limit here. This is evidenced in the low MAE (0.074) and variance (+/- 0.002).**
- I tested grouping my data by
    - alloy_id: alloys are identified by their x_B = 0.5 formula, which encodes A1, A2, B1, B2, and X identity
    - B1_B2: alloys are identified by their B1 and B2 site, which encodes only B1 and B2 identity, ignoring A1, A2, and X identity.
- *Hypothesis: because B site has the most significant influence on perovskite band gaps, grouping by alloy_id would lead to more leakage than B1_B2 as alloys with different the same B1_B2 site but have different A1/A2/X site(s) would be treated as different groups.*
- TBC
- 
- 
- 