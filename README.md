# Credit Card Customer Segmentation with K-Means

An unsupervised machine learning project that explores credit card usage and groups customers into interpretable behavioural segments.

Developed for the **Fondamenti di Machine Learning** final project, using the FinTech Solutions S.p.A. business scenario described in the assignment.

**[View the analysis notebook](customer_segmentation.ipynb.ipynb)**

The notebook is written in Italian. This README provides an English overview.

## Business objective

Identify patterns in purchases, cash advances, balances and payments to formulate differentiated marketing hypotheses. The analysis describes observed behaviour; it does not estimate individual credit risk or establish future customer preferences.

## Dataset

The [credit card customer dataset supplied for the assignment](https://proai-datasets.s3.eu-west-3.amazonaws.com/credit_card_customers.csv) contains **8,950 customers and 18 columns**, including the customer identifier `CUST_ID`.

Features cover purchase amounts and frequencies, cash advances, transaction counts, balances, credit limits, payments and tenure. The notebook downloads the CSV directly from the source.

## Workflow

1. **Explore and audit:** inspect missing values, duplicates, distributions and consistency between related variables.
2. **Prepare the data:** separate 19 records with purchase inconsistencies; exclude `CUST_ID` and `MINIMUM_PAYMENTS` from model features; mark eight cash-advance frequencies above 1 as missing under the documented preprocessing assumption.
3. **Impute and transform:** use median imputation, apply `log1p` to seven monetary features and standardise all 16 model features.
4. **Compare models:** evaluate K-Means for K = 2 through 8 using inertia, silhouette, cluster sizes and profile interpretation, with `random_state=42` and `n_init=20`.
5. **Interpret the final model:** examine profiles in the original feature scale, heatmaps and a two-component PCA projection.
6. **Propose and export:** document marketing experiments and export assignments, excluded records, quality checks and segment profiles.

## Results

The following values are reported in the saved outputs of the notebook currently published in this repository.

| Metric | Result |
|---|---:|
| Customers in the source | 8,950 |
| Customers segmented | 8,931 |
| Records excluded and retained separately | 19 |
| Model features | 16 |
| Selected number of clusters | 4 |
| Silhouette score | 0.211117 |
| Inertia | 84,992.940 |
| Variance explained by two PCA components | 52.039% |

### Customer segments

| Cluster | Behavioural profile | Customers | Share of segmented customers |
|---|---|---:|---:|
| 0 | Predominant use of cash advances | 2,806 | 31.419% |
| 1 | Frequent purchases with high total purchase amounts | 2,184 | 24.454% |
| 2 | Frequent purchases, predominantly in instalments | 2,226 | 24.924% |
| 3 | Less frequent purchases, predominantly one-off | 1,715 | 19.203% |

**Why four clusters?** The notebook prioritises behavioural detail and interpretability for differentiated campaigns. K = 2 achieves the highest silhouette score among the tested solutions; K = 4 is therefore an interpretive trade-off, not the optimum under that metric.

The final silhouette indicates limited separation. PCA is used only for visualisation, after clustering in the full preprocessed feature space; its two-dimensional view represents approximately half of the variance.

## Marketing hypotheses

The proposed actions include purchase-use experiments for the cash-advance segment, capped loyalty incentives for frequent purchasers, instalment-management communications and follow-up purchase incentives for less frequent purchasers.

These are proposals, not measured campaign outcomes. The notebook recommends randomised campaign and control groups, predefined evaluation metrics and consideration of incentive costs before drawing conclusions about business value.

## Limitations

- The assumed upper bound of 1 for `CASH_ADVANCE_FREQUENCY` requires confirmation from the data provider. Its median imputation is zero, which does not prove that affected customers never used cash advances.
- Excluding `MINIMUM_PAYMENTS` and 19 records affects the information available to the model.
- Correlated variables and extreme transaction counts can influence distances and cluster assignments.
- Stability across samples, random seeds and alternative preprocessing choices has not been assessed.
- Cluster labels are descriptive and arbitrary; the segmentation is not an individual financial-risk assessment.

## Repository and generated outputs

The repository contains this README and [customer_segmentation.ipynb.ipynb](customer_segmentation.ipynb.ipynb). Charts and tables are embedded in the notebook.

Running the export section creates five CSV files in `output_segmentazione/`:

- `clienti_segmentati_kmeans.csv`
- `clienti_esclusi.csv`
- `registro_qualita_dati.csv`
- `profili_cluster.csv`
- `strategie_marketing.csv`

It also creates `risultati_segmentazione_kmeans.zip`. These are runtime outputs, not files currently included in the repository. The archive contains results and supporting tables, not serialised trained model objects.

## How to run

Download the notebook and upload it to Google Colab, or open it in a local Jupyter environment. Run the cells in order with Internet access for the source CSV.

For a local environment, install:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

Full-dataset silhouette calculations for several values of K may take a few minutes. Library versions and future source-data updates can affect reproduced results.

## Author

[Claudio Lombardi](https://github.com/PsyClaudio)
