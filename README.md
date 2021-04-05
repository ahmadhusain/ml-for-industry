# :star2: Overview Algo ML for Industry

The following bookdown is produced by the team at Algoritma Data Science Academy for Machine Learning Application in Industry. 
Documentation inside the bookdown provided in **Bahasa**. The following below are list of the chapters contained in the bookdown :

* Telecommunication
  * Customer Churn Prediction
* Finance
  * Credit Risk Analysis
  * Evaluation Customer Financial Complaints
* Retail
  * E-Commerce Clothing Reviews
  * Customer Segmentation with RFM Analysis (in Python Programming Language)
* Insurance
  * Prediction of Total Claim Amount
* Bioinformatics
  * QTL Mapping for Disease Resistance
* Health
  * Survival Analysis of Parients with Lung Cancer
* Media & Publishing
  * Causal Impact on Leads generation
  
  
# :memo: Contributing Articles

Please follow the submission guidelines below:

1. Fork the repository.
2. Make a some development version or added a new article.
3. Submit a *pull request*.

If the project owner agrees with your work, they might merged your request into the original repository.

## 1. Fork a Repo 

**Step-1**:

1. On github, navigate to the [ahmadhusain/ml-for-industry](https://github.com/ahmadhusain/ml-for-industry) repository.
2. On the top-right corner of the page, click *fork*.

**Step-2**: Keep your fork synced using Git. If you haven't yet, you should first [set up Git](https://help.github.com/en/articles/set-up-git#setting-up-git).

1. On github, click **fork** on the *ml-for-industry* repository.
2. Open Git Bash
3. Type `git clone`, and then paste the URL of repository. It will look like this, with your GitHub username instead of `YOUR-USERNAME`:

```
$ git clone https://github.com/YOUR-USERNAME/ml-for-industry
```
4. Press **Enter**. Your local clone will be created.


## 2. Create an Article

1. Create an article about machine learning in industry that contains the following skeleton :
  * Background
  * Data Preparation
  * Exploratory Data Analysis 
  * Modelling
  * Conclusion
2. If you work in R programming language just go ahead into the 4th step
3. But if you work in Python programming language before going to any further, you should complete the following steps below :
  * Clone or download a bundle of code inside this repository : https://github.com/Litaa/convert-jupyter-to-rmd
  * Put your `.ipynb` file into ipynb folder
  * Go to assets folder
  * Open and run all code inside `jupyter.R` file. Make sure that there's no errors when you're running the code.
  * After `jupyter.R` already succeed, open and run all code inside `converter.R` file. Make sure that there's no errors when you're running the code.
  * After successfully running the code, converted `.ipynb` files automatically  will be in Rmd folder with `.Rmd` extension
  * Do some setting inside your .Rmd file to ensure the code be able to connect with your python environment. Here below simple setting you can do :
  
```{r echo=FALSE}
  Sys.setenv(RETICULATE_PYTHON = "C:/<your-anaconda-path>/envs/<your-env-name>/python.exe")
  library(reticulate)
  py_run_string("import os")
  py_run_string("os.environ['QT_QPA_PLATFORM_PLUGIN_PATH'] = 'C:<your-anaconda-path>/Library/plugins/platforms'")
```
4. Copy and paste all you code and documentation inside targeted .Rmd files based on the topic you have been created. Don't forget to adjust the heading files.
5. (optional) To ensure that your code working properly, knit your .Rmd file into HTML.


## 3. Submit Pull Request! :grin:

* Save your article
* Open git bash
  * `git add .`
  * `git commit -m 'your message'`
  * `git push`
* then submit a pull request to algoritma ml-industry repository.
