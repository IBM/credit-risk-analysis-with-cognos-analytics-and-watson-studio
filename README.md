# !!! WORK IN PROGRESS

## Flow

## Included Components

## Steps

## 1. Clone the repo

```bash
git clone https://github.com/IBM/cognos-analytics-with-watson-studio.git
```

## 2. Upload data file into Cognos Analytics

* From the data directory in your local repo, unzip the file `combined_data.csv.zip` to extract the file `combined_data.csv`.

* Log into IBM's [Cognos Analytics](https://www.ibm.com/products/cognos-analytics).

* From the Cognos Analytics main dashboard, select the `+` icon in the lower left corner and select `Upload files`.

* From the file selection dialog, select the extracted csv file. In this example, the file has been loaded into the `cognos-studio-data` folder.

  ![ca-data-file](doc/source/images/ca-data-file.png)

## 3. Create a new Watson Studio project

* Log into IBM's [Watson Studio](https://dataplatform.cloud.ibm.com). Once in, you'll land on the main dashboard.

* Create a new project by clicking `New project +` and then click on `Create an empty project`.

* Enter a project name.

* Choose an existing Object Storage instance or create a new one.

* Click the `Create` button.

Upon a successful project creation, you are taken to the project Overview tab. Take note of the Assets and Settings tabs, we'll be using them to associate our project with any external assets (datasets and notebooks) and any IBM cloud services.

  ![new-project](doc/source/images/new-project.png)

## 4. Create a Cognos Analytics connection in Watson Studio

* From you Watson Studio project dashboard, click `Add to project +`, and select the `Connection` option.

  ![connection-option](doc/source/images/connection-option.png)

* Select `Cognos Analytics` as the data source.

  ![connection-types](doc/source/images/connection-types.png)

* Configure the connection to point to your Cognos Analytics instance. Provide a name for your connection, plus `namespace`, `username/password`, and `URL`.

  ![create-connection](doc/source/images/create-connection.png)

* Click the `Create` button.

## 5. Add Cognos Analytics data asset to Watson Studio project

* From you Watson Studio project dashboard, click `Add to project +`, and select the `Connected data` option.

  ![connected-data-option](doc/source/images/connected-data-option.png)

* Click `Select source` to view the available connections.

* From the `Select connection source` panel, select the connection you created in the previous step. Then select the path to the data file on Cognos Analytics.

![select-connector-path](doc/source/images/select-connector-path.png)

* Click `Select` to select the connection source path.

* From the `New connected data asset` panel, name the path and click `Create`.

![add-connected-path](doc/source/images/add-connected-path.png)

* At this point, you should have a `Connection` and a `Folder Asset` in your list of project assets.

![project-data-assets](doc/source/images/project-data-assets.png)

## 6. Create data access token

* From you Watson Studio project dashboard, selected the `Settings` tab.

![settings-tab](doc/source/images/settings-tab.png)

* Scroll down to the `Access tokens` list, and click on `New token +`.

![project-tokens](doc/source/images/project-tokens.png)

* Provide a name for your token and set the access level to `Editor`.

![create-token](doc/source/images/create-token.png)

* Click the `Create` button.
* To view the token value, select the action menu located on the right-hand side of the token row, and click on `View`.

![new-token](doc/source/images/new-token.png)

### 7. Create the notebook in Watson Studio

* From the new project `Overview` tab, click `+ Add to project` on the top right and choose the `Notebook` asset type.

  ![add-notebook.png](doc/source/images/add-notebook.png)

* Fill in the following information:

  * Select the `From URL` tab. [1]
  * Enter a `Name` for the notebook and optionally a description. [2]
  * For `Select runtime` select the `Default Python 3.6` option. [3]
  * Under `Notebook URL` provide the following url [4]:

  ```url
  https://raw.githubusercontent.com/IBM/cognos-analytics-with-watson-studio/master/notebooks/diet-related-disease.ipynb
  ```

  ![new-notebook](doc/source/images/new-notebook.png)

* Click the `Create` button.

  > **TIP:** Your notebook will appear in the `Notebooks` section of the `Assets` tab.

### 7. Add data to the notebook

* After creation, the notebook will automatically be loaded into the nodebook runtime environment. You can re-run the notebook at any time by clicking on the `pencil` edit icon displayed in right-hand column of the notebook row.

  ![start-notebook](doc/source/images/start-notebook.png)

* The second cell of the notebook should be labeled as a `@hidden_cell`. This is where will load in our Cognos Analytics `csv` file.

  >**Note**: This cell is marked as a `@hidden_cell` because it will contain sensitive credentials.

* Use `Find and Add Data` (look for the 01/00 icon) and its `Connection` tab.

* Place your cursor at the bottom of the `@hidden_cell` (indicated by the red arrow), then select `insert to code`, and click on the `pandas DataFrame` option.

  ![notebook-add-data](doc/source/images/notebook-add-data.png)

* The `@hidden_cell` should now contain the access token and data connector that will allow you to load your Cognos Analytics `csv` file.

* Change the name of the DataFrame variable to `df_data_1`, which is the name used in the remaining notebook cells.

* Change the path and file name to point to your Cognos Analytics data.

  ![notebook-data-cell](doc/source/images/notebook-data-cell.png)

  > **TIP**: To search the data files that your connector points to,  run the following command in the cell:
  >
  >```python
  >CADataConnector.search_data("keyword")
  >```

## 8. Run the notebook

When a notebook is executed, what is actually happening is that each code cell in the notebook is executed, in order, from top to bottom.

Each code cell is selectable and is preceded by a tag in the left margin. The tag format is `In [x]:`. Depending on the state of the notebook, the `x` can be:

* A blank, this indicates that the cell has never been executed.
* A number, this number represents the relative order this code step was executed.
* A `*`, this indicates that the cell is currently executing.

There are several ways to execute the code cells in your notebook:

* One cell at a time.
  * Select the cell, and then press the `Play` button in the toolbar.
* Batch mode, in sequential order.
  * From the `Cell` menu bar, there are several options available. For example, you can `Run All` cells in your notebook, or you can `Run All Below`, that will start executing from the first cell under the currently selected cell, and then continue executing all cells that follow.
* At a scheduled time.
  * Press the `Schedule` button located in the top right section of your notebook panel. Here you can schedule your notebook to be executed once at some future time, or repeatedly at your specified interval.

## 9. Refine the data

SNAP refers to the Supplemental Nutrition Assistance Program that was formerly known as food stamps.

WIC is the Woman, Infant, Children program that provides nutrious foods for expecting mothers, infants and children.

* State
* County
* PCT_REDUCED_LUNCH10 - Percentage of reduced lunches in schools
* PCT_DIABETES_ADULTS10 - Percentage of adult population with diabetes
* PCT_OBESE_ADULTS10 - Percentage of adult population that is obese
* FOODINSEC_10_12
* PCT_OBESE_CHILD11 - Percentage of children in the population that is obese
* PCT_LACCESS_POP10
* PCT_LACCESS_CHILD10
* PCT_LACCESS_SENIORS10
* SNAP_PART_RATE10 - SNAP Participation Rate
* PCT_LOCLFARM07
* FMRKT13
* PCT_FMRKT_SNAP13
* PCT_FMRKT_WIC13
* FMRKT_FRVEG13
* PCT_FRMKT_FRVEG13
* PCT_FRMKT_ANMLPROD13
* FOODHUB12 - Food hubs connect the dots between producers and cosumers of food in local and regional food systems. Currently there are 236 food hubs in the U.S.
* FARM_TO_SCHOOL
* SODATAX_STORES11
* State_y
* GROC12
* SNAPS12
* WICS12
* PCT_NHWHITE10 - Percentage of the poluation that is white
* PCT_NHBLACK10 - Percentage of the poluation that is black
* PCT_HISP10 - Percentage of the poluation that is hispanic
* PCT_NHASIAN10 - Percentage of the poluation that is asian
* PCT_65OLDER10 - Percentage of the poluation that is 65 or older
* PCT_18YOUNGER10 - Percentage of the poluation that is 18 or younger
* POVRATE10 -
* CHILDPOVRATE10

## 10. Write out data using Cognos Analytics connection

Once we have refined our DataFrame, we can use the connection to write the data back out so that we can use it Cognos Analytics.

In this example, we are writing it out to a file named `focused_data.csv`.

![notebook-save-data](doc/source/images/notebook-save-data.png)

Once complete, you should see the file in your Cognos Analytics instance.

![ca-new-data-file](doc/source/images/ca-new-data-file.png)

## 11. Visualize the data in Cognos Analytics

## License

This code pattern is licensed under the Apache Software License, Version 2.  Separate third party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
