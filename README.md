


D1 - Cross Validation (CV)
notebook on applying CV on a toy dataset and my own dataset.

Done:
- compare Kernel Ridge Regression (KRR), Random Forest Regression (RF), and Histogram Gradient Boosting Regression (HGB) on toy and my dataset
- compare different CV methods: k-fold, leave one out (LOO)

Toy dataset: sklearn diabetes dataset
    Model comparison shows RF and HGB perform similarly, and better than KRR.
        The KRR, RF, and HGB are minor improvements to the Dummy regressor, likely because the diabetes target is not deterministic from their features
    CV comparison indicates k-fold (n=5, no shuffling) gives virtually the same results as shuffled, 20-fold, and LOO, but is the fastest (tied with 5-fold with shuffling)
From the tests, I would use RF, 5-fold CV with or without shuffling for regression learning on sklearn's diabetes dataset

My dataset: B-site alloy halide perovskites DFT results and Magpie features
    Model comparison shows RF and HGB results in virtually the same MAE. HGB would be the model of choice because it was faster than RF
        KRR, RF, and HGB are major improvements over the Dummy regressor, indicating the deterministic nature of my dataset.
    CV comparison shows 5- and 10-fold result in the same MAEs, but shuffling reduces apparent variance
        My dataset is arranged such that non-alloy compositions are concatenated at the end of the dataset. This results in high MAE for the last fold in non-shuffled CV, which contributes to high variance.
        Shuffling narrows variance by dispersing the non-alloy compositions across all folds.
            Stratified k-fold CV, which will be explored in later notebooks, is expected to serve a similar purpose in ensuring non-alloy compositions are present in every fold.
            Grouped k-fold CV, which will also be explored in later notebooks, is expected to serve the opposite purpose as it ensures each CV fold contains a different group of data, and could give us an idea of model performance on unseen chemistry.
From the tests, I would use HGB, 5-fold CV with shuffling for regression learning on my dataset. Though the results indicate that the tested models is expected to perform worse on unseen B-site chemistry, and that something interesting might be happening with how my dataset can be grouped/classed.

Note on MAEs on my dataset:
MAE weights absolute errors equally regardless of band gap magnitude. A good MAE for semiconducting compositions may not reflect a good performance on metallic compositions as the percentage variance will be larger (owing to a smaller band gap value in the denominator)