---
description: >-
  It's often useful to provide a front-end for machine learning models, so they
  can be used interactively by stakeholders or clients. Datapane's API provides
  a few primitives to help with this.
---

# 📈 Create dynamic reports from an ML model

## Introduction

Although you deploy Python scripts to Datapane to allow users to run them to generate reports, not all processing should happen on Datapane, and many teams prefer to run resource intensive tasks \(such as training a model\) on their own infrastructure. In this example, we will deploy a script which marketing teams can use to self-serve on a model which is trained outside of Datapane.

{% hint style="info" %}
The design goal of Datapane's API is to slot into other data infrastructure, as opposed to constraining all processing to happen on Datapane itself.
{% endhint %}

## Training and deploying a model

In the following example, we are training a simple model on our own infrastructure to predict which users will purchase which products. This training could happen in a Sagemaker notebook, or in the context of an existing pipeline such as Airflow or Luigi. To upload the trained model to Datapane, we serialise it and deploy it using Datapane's Blob API. This training step could be run on a cadence to keep our model up-to-date.

```python
from typing import Callable, Dict
import numpy as np
import pandas as pd
import acme_corp_data_lib
import pickle
import datapane as dp

from lightgbm import LGBMClassifier
from sklearn.metrics import make_scorer, roc_auc_score
from sklearn.model_selection import GridSearchCV, GroupKFold

user_id_col: str = "user_id"
item_id_col: str = "item_id"
purchased_col: str = "purchased"

def create_roc_scorer(purchased_col) -> Callable:
    """Creates a split-aware ROC AUC scorer"""

    split_index = 0

    def score(y_true, y_proba, df=None, splits=None, purchased_col=purchased_col):
        nonlocal split_index
        test_df = df.iloc[splits[split_index][1]]
        split_index = (split_index + 1) % len(splits)
        return roc_auc_score(test_df[purchased_col], y_proba)

    return score

in_df = acme_corp_data_lib.get_user_data();

features = [col for col in in_df.columns if col not in [user_id_col, item_id_col, purchased_col]]

classifier = LGBMClassifier(random_state=126)

cat_data = in_df[features].select_dtypes("object")

if not cat_data.empty:
    in_df[cat_data.columns] = cat_data.astype("category")

splits = list(
    GroupKFold(n_splits=3).split(in_df[features], in_df[purchased_col], in_df[user_id_col])
)
scorer = make_scorer(create_roc_scorer(purchased_col), df=in_df, splits=splits, needs_proba=True)

fitted = GridSearchCV(
    classifier,
    param_grid=dict(num_leaves=[10, 40, 80]),
    cv=splits,
    return_train_score=False,
    scoring=scorer,
).fit(in_df[features], in_df[purchased_col])

b = dp.Blob.upload_obj(fitted)
print(b.id)
```

## Creating a front-end

To create our script and output report on Datapane, we write a notebook which uses our serialised model to classify inputs, which are passed to Datapane through a form. In our code, we take a single item id, pull down recent order data from our database, and use the model to predict purchases. Our script creates a Report which contains the most likely purchasers of a given item inside of a Table component.

{% hint style="info" %}
Datapane supports providing your own Python and OS libraries. In this example, we're using our own `acme_corp_data_lib` to pull data from our database, and using `lightgbm` for the classification.
{% endhint %}

{% code title="dp\_script.ipynb" %}
```python
import datapane as dp
import pandas as pd
import pickle
import acme_corp_data_lib
import lightbgm

from datapane import Params, Report, Markdown, Table

item_id = dp.config.item_id
purchase_data = acme_corp_data_lib.get_purchase_data();
features = [col for col in in_df.columns if col not in ["user_id", "item_id", "purchased"]]

model = dp.Blob("deadbeef").download_obj()

predictions = fitted.predict_proba(purchase_data[features])

df = purchase_data[["user_id", "item_id", "purchased"]]
df["p_purchase"] = predictions[:, 1]
out_table = df[df['item_id'] == item_id]

Report.create(
  Markdown(f"# Predicted purchasers for {item_id}"), 
  Table.create(out_table)
 )
```
{% endcode %}

In our `datapane.yaml` we can define which fields to expose in our form, and specify the local and external library requirements.

{% code title="datapane.yaml" %}
```yaml
name: purchase_predictor

parameters:
  - name: item_id
    type: string
    
include:
  - acme_corp_data_lib

requirements:
  - lightgbm
```
{% endcode %}

Next, we deploy our script to Datapane using the CLI:

```bash
$ datapane script deploy
```

## Using the script

Once deployed, stakeholders are able to run this script through a form on our Datapane instance. Each time this form is run with an item id, it generates a Report, which contains the output predictions in a table component.

![](../.gitbook/assets/image%20%2824%29.png)

We can share this report itself, or embed the report in our company Notion wiki, where others can view the data and explore it themselves.

![](../.gitbook/assets/image%20%2821%29.png)



