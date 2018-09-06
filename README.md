# Cancer-Inhibitor
Predict small molecules' activity targeting protein kinase

### Outline

It was reported that an estimated 4292,000 new cancer cases and 2814,000 cancer deaths would occur in China in 2015. Chen, W., etc. (2016), Cancer statistics in China, 2015.

# Small molecules play an non-trivial role in cancer chemotherapy. Here I focus on inhibitors of 8 protein kinases(name: abbr):

Cyclin-dependent kinase 2: cdk2
Epidermal growth factor receptor erbB1: egfr_erbB1
Glycogen synthase kinase-3 beta: gsk3b
Hepatocyte growth factor receptor: hgfr
MAP kinase p38 alpha: map_k_p38a
Tyrosine-protein kinase LCK: tpk_lck
Tyrosine-protein kinase SRC: tpk_src
Vascular endothelial growth factor receptor 2: vegfr2
For each protein kinase, several thousand inhibitors are collected from chembl database, in which molecules with IC50 lower than 10 uM are usually considered as inhibitors, otherwise non-inhibitors.

## Challenge

Based on those labeled molecules, build your model, and try to make the right prediction.

Additionally, more than 70,000 small molecules are generated from pubchem database. And you can screen these molecules to find out potential inhibitors. P.S. the majority of these molecules are non-inhibitors.

# DataSets(hdf5 version)

There are 8 protein kinase files and 1 pubchem negative samples file. Taking "cdk2.h5" as an example:

import h5py
from scipy import sparse
hf = h5py.File("../input/cdk2.h5", "r")
ids = hf["chembl_id"].value # the name of each molecules
ap = sparse.csr_matrix((hf["ap"]["data"], hf["ap"]["indices"], hf["ap"]["indptr"]), shape=[len(hf["ap"]["indptr"]) - 1, 2039])
mg = sparse.csr_matrix((hf["mg"]["data"], hf["mg"]["indices"], hf["mg"]["indptr"]), shape=[len(hf["mg"]["indptr"]) - 1, 2039])
tt = sparse.csr_matrix((hf["tt"]["data"], hf["tt"]["indices"], hf["tt"]["indptr"]), shape=[len(hf["tt"]["indptr"]) - 1, 2039])
features = sparse.hstack([ap, mg, tt]).toarray() # the samples' features, each row is a sample, and each sample has 3*2039 features
labels = hf["label"].value # the label of each molecule
