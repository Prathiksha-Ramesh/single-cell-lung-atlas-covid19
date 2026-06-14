# A molecular single-cell lung atlas of lethal COVID-19
Single-cell transcriptomic analysis of lethal COVID-19 lungs: QC, cell type annotation, differential expression, and pathway analysis.
Reference : https://www.nature.com/articles/s41586-021-03569-1

**1. Cloning the repo **

**2. workflow for creating a virtual environment in VS Code :**

conda create -n singlecell python=3.11

conda activate singlecell 

**3. Installing packages**
pip install -r requirements.txt 

# To push the changes 

1. check what has changed - git status 
2. stage all changes - git add . 
3. create a commit - git commit -m "describe your commit"
4. push to a main branch - git push origin main 
5. To fetch the latest changes in the UI and to rebase it - git pull --rebase origin main


# To make directories 
mkdir -p data/raw

# to save the environement for reproducibility 
conda env export > environment.yml

# To move the files for example raw files 

mv /Users/prathiksharamesh/Downloads/GSE171524_RAW data/raw/

# To gunzip files 
gunzip data/raw/GSE171524_RAW/*.gz




   


